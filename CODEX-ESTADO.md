# CODEX-ESTADO

> Ultima resposta processada: **27/11/2026 10:50** · publicado em **27/11/2026 10:50** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 27/11 10:50 · sessao d0ab90c9cfc0 · E4C-F5-INSTANTE · sonda · origem=Code

**1. Casos (i): zero no recorte da sonda.** A saída é consistente com o script. Os subtotais por mês fecham com os totais por empresa, e `FIM 701 0` indica 701 candidatas, todas da classe (ii). A incidência fora do recorte ou após a coleta fica **[nao verificado]**. (`r1.md:90–96`, saída de `sonda.py:3–14`)

| Empresa | Total (i) | Casos (i) por mês de `data_evento` | Fonte |
|---|---:|---|---|
| 2 | 0 | 2026-08: 0 · 2026-09: 0 · 2026-10: 0 · 2026-11: 0 | Saída de `sonda.py:3,6–9` |
| 3 | 0 | 2026-10: 0 · 2026-11: 0 | Saída de `sonda.py:4,10–11` |
| 4 | 0 | 2026-10: 0 · 2026-11: 0 | Saída de `sonda.py:5,12–13` |

**2. Competência entregue: 0 de 0.** O script coleta até dez casos da classe (i). Com a amostra vazia, nenhuma consulta de entrega K6 foi feita pelo ramo da amostra. (`r1.md:73–88`, saída de `sonda.py:13–14`)

**3. Guarda da cura2: nenhum caso da amostra para barrar.** Pelo anexo, a recusa ocorreria se, após C6 e a checagem de presença, a redução real ainda desse T1 e a coluna trouxesse T2 posterior sem linha correspondente. A última `RESPOSTA` persistida, sozinha, não estabelece que T1 continua vigente, pois `RECUSA` ou `REABERTURA` pode limpá-la. (`ver2_r.md:27`, `app/chamados/models.py:4455`, `app/chamados/models.py:4495`)

A origem de um T2 em produção, seja conservação legítima de legado ou escrita de `carimbar`, fica **[nao verificado]**. A cópia mostra que `carimbar` pode gravar `respondida_em` sem criar a `RESPOSTA` correspondente quando recebe a coluna nula e há texto. Se a instância já chega com T2 preenchido, a chamada preserva esse instante. (`app/chamados/models.py:3474–3482`, `app/chamados/models.py:3514`, `app/chamados/models.py:4419`)

## 27/11 10:49 · sessao 86646eb56732 · E4C-F5-INSTANTE · leitura · origem=Code

**E4C-F5-INSTANTE.** A cópia tem `DecisaoPergunta.linhas_da_projecao_sem_linha`, que verifica a correspondência por pergunta, tipo de ato e instante, e `DecisaoPergunta.leitura`, que combina as linhas reais com a projeção sem linha. `composta` usa essa leitura, e `par_ato_coluna` compara a presença da validação. O redutor `_reduzir`, a leitura `_reais_reduzidos` e a guarda de instante da cura2 estão ausentes da cópia. A confirmação do SHA por Git, a versão implantada e a incidência atual em produção ficam **[nao verificado]**. [app/chamados/models.py:4416](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:4416) · [app/chamados/models.py:4473](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:4473) · [app/chamados/models.py:4507](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:4507) · [app/chamados/models.py:4518](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:4518) · [app/chamados/models.py:3492](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:3492)

### Sonda

No ambiente de produção, abra `python -B manage.py tenant_command shell --schema=SEU_SCHEMA --interface=python` e cole o bloco. A sonda usa uma transação `REPEATABLE READ, READ ONLY` e imprime o estado atual do universo, com os dez menores PKs de (i). A sintaxe foi conferida em memória. A execução com banco fica **[nao verificado]**. [app/docs/APP-ATESTADO.md:71](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/APP-ATESTADO.md:71) · [app/chamados/tests/cef_c3_sonda.py:21](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/tests/cef_c3_sonda.py:21)

A classe (i) usa a última `RESPOSTA` persistida na ordem `(ato_em, pk)` e a ausência de uma `RESPOSTA` correspondente na coluna. `leitura_antiga_em_T2` indica se a leitura antiga termina em T2. Uma recusa ou reabertura posterior pode limpar a resposta nessa leitura. As contagens mensais usam o mês civil de `data_evento`, como pedido. A entrega usa a competência da janela da empresa para esse dia, por `competencia_do_fato`, que chama `folha.export.competencia_entregue`. Sem `data_evento`, a pergunta entra na contagem e a entrega sai como `NA`. [app/chamados/models.py:4378](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:4378) · [app/chamados/models.py:4438](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:4438) · [app/chamados/models.py:4495](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:4495) · [app/chamados/services/materializacao.py:803](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/materializacao.py:803) · [app/chamados/services/passivo_validado.py:91](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/passivo_validado.py:91)

```python
exec(r"""
from collections import Counter, defaultdict

from django.conf import settings
from django.db import connection, transaction
from django.db.models import Exists, OuterRef, Prefetch
from django.utils import timezone

from chamados.models import DecisaoPergunta as D, PerguntaDisputa
from chamados.services.materializacao import competencia_do_fato

schema = connection.schema_name
if not schema or schema == getattr(settings, "PUBLIC_SCHEMA_NAME", "public"):
    raise RuntimeError("Abra o shell no schema do tenant.")

respostas = D.objects.filter(pergunta_id=OuterRef("pk"), ato=D.RESPOSTA)
decisoes = D.objects.only(
    "pk", "pergunta_id", "ato", "ato_em", "ato_texto", "resposta_mantem_validacao",
)
candidatas = (
    PerguntaDisputa.objects.filter(
        Exists(respostas),
        respondida_em__isnull=False,
        validada_em__isnull=True,
        materializada_em__isnull=True,
    )
    .select_related("disputa__colaborador__empresa")
    .only("data_evento", "respondida_em", "validada_em", "resposta_colab",
          "disputa__colaborador__empresa__dia_inicio_competencia")
    .prefetch_related(Prefetch("decisoes", queryset=decisoes))
    .order_by("pk")
)

totais = Counter()
por_empresa = defaultdict(Counter)
por_mes = defaultdict(Counter)
amostra = []

with transaction.atomic(durable=True):
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
        cursor.execute("SELECT current_schema(), pg_is_in_recovery()")
        schema_atual, em_recuperacao = cursor.fetchone()
        if schema_atual != schema or em_recuperacao:
            raise RuntimeError("A sonda exige o schema do tenant no banco primário.")

    for p in candidatas.iterator(chunk_size=500):
        reais = list(p.decisoes.all())
        ultima = next(d for d in reversed(reais) if d.ato == D.RESPOSTA)
        ja = {(p.pk, d.ato, d.ato_em) for d in reais}
        falta_resposta = any(
            linha["ato"] == D.RESPOSTA
            for linha in D.linhas_da_projecao_sem_linha([p], ja=ja)
        )
        leitura = D.leitura(p)
        classe = "i" if falta_resposta and ultima.ato_em < p.respondida_em else "ii"

        empresa = p.disputa.colaborador.empresa
        mes = p.data_evento.strftime("%Y-%m") if p.data_evento else "SEM_DATA_EVENTO"
        totais[classe] += 1
        por_empresa[empresa.pk][classe] += 1
        por_mes[empresa.pk, mes][classe] += 1

        if classe == "i" and len(amostra) < 10:
            k6 = competencia_do_fato(p, empresa)
            if k6 is None:
                competencia, entregue = "SEM_DATA_EVENTO", "NA"
            else:
                mes_k6, ano_k6, foi_entregue = k6
                competencia = f"{ano_k6:04d}-{mes_k6:02d}"
                entregue = "SIM" if foi_entregue else "NAO"
            ato_em = timezone.localtime(ultima.ato_em).isoformat()
            respondida_em = timezone.localtime(p.respondida_em).isoformat()
            amostra.append(
                f"PK {p.pk} empresa={empresa.pk} mes_data_evento={mes} "
                f"ato_em={ato_em} respondida_em={respondida_em} "
                f"competencia_k6={competencia} entregue={entregue} "
                f"leitura_antiga_em_T2={int(leitura.respondida_em == p.respondida_em)}"
            )

for empresa_id, n in sorted(por_empresa.items()):
    print(f"EMPRESA {empresa_id} i={n['i']} ii={n['ii']}")
for (empresa_id, mes), n in sorted(por_mes.items()):
    print(f"MES_DATA_EVENTO {empresa_id} {mes} i={n['i']} ii={n['ii']}")
for linha in amostra:
    print(linha)
print(f"FIM {totais['i'] + totais['ii']} {totais['i']}")
""")
```

### De onde pode vir T2

- **`PerguntaDisputa.carimbar` é o escritor de aplicação encontrado que preenche `respondida_em` sem registrar a `RESPOSTA` correspondente.** Com `resposta='responder'` ou `'validar'`, texto preenchido e `self.respondida_em` nulo, atribui `quando`. Se as guardas passam, grava a projeção por `save`. Nessa base, o próprio `carimbar(resposta='validar')` termina nesse `save`, sem registrar a foto `_sem_linha`. O fecho sem credor chama com `'responder'`. A absorção chama com `'validar'` e registra `ABSORCAO`. [app/chamados/models.py:3474](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:3474) · [app/chamados/models.py:3504](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:3504) · [app/chamados/services/disputa_emissao.py:2192](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/disputa_emissao.py:2192) · [app/chamados/services/materializacao.py:1108](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/materializacao.py:1108) · [app/chamados/services/materializacao.py:1129](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/materializacao.py:1129)
- **O acervo anterior à E1 também pode trazer o instante só na coluna.** A migração 0051 converteu as linhas antigas em `REABERTURA`. Na base, `_nascer` registra as assinaturas recebidas, e os escritores de resposta humana, automática e pela trilha registram a `RESPOSTA` no mesmo instante que gravam na coluna. [app/chamados/models.py:4419](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:4419) · [app/chamados/migrations/0051_decisao_pergunta.py:41](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/migrations/0051_decisao_pergunta.py:41) · [app/chamados/models.py:3333](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:3333) · [app/chamados/services/disputa_emissao.py:1562](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/disputa_emissao.py:1562) · [app/chamados/services/disputa_emissao.py:1757](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/disputa_emissao.py:1757) · [app/chamados/services/disputa_emissao.py:1793](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/disputa_emissao.py:1793)
- **`save` e `descarimbar_em_lote` são mecanismos de persistência sem `registrar` próprio.** Nos chamadores do lote que passam `respondida_em`, o valor passado é `None`. O `descarimbar` unitário pode tirar a materialização preservando T2, como faz `desfazer_carimbo_sem_lastro` quando C6 volta a dar a pergunta por viva. [app/chamados/models.py:3840](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:3840) · [app/chamados/models.py:3657](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:3657) · [app/chamados/models.py:1739](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:1739) · [app/chamados/services/disputa_emissao.py:1661](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/disputa_emissao.py:1661) · [app/chamados/services/validacao.py:476](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/validacao.py:476) · [app/chamados/models.py:3572](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:3572) · [app/chamados/services/relavra_via.py:242](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/relavra_via.py:242)

**T2 só na coluna é possível por construção.** Para o caso misto, `carimbar` exige que o instante esteja nulo na instância que recebe, então preserva um T1 já preenchido nessa instância. A sonda mede a divergência persistida. A origem e a legitimidade de cada T2, bem como a chegada de cada candidata ao ponto de recusa da cura2, ficam **[nao verificado]**. [app/chamados/models.py:3471](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:3471) · [app/chamados/models.py:3483](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:3483)

## 27/11 10:47 · sessao 4c37eb8792c5 · ORFAS-VOLTOU · leitura · origem=Code

O `sem_evento=0` informado às 06:36 é a leitura do vigia do **carimbo por lâmpada**. O estoque de perguntas candidatas a reate é publicado em `logs/reconciliador.log`, na linha `orfas reataveis (UNIVERSO, MEDIDA ...)`. O total atual da frota, a empresa das quatro células e o ramo das 16 perguntas estão [nao verificado]. `app/ponto/management/commands/reconciliar_perguntas_orfas.py:29–36` `app/chamados/management/commands/reconciliar_chamados.py:97–115` `app/config/crons.py:395`

## 1. Porta de reate, chamadores e trilha

`reatar_pergunta_orfa(p, colab)` primeiro deriva um destino vivo, usando `p.data_evento` antes da data do contexto. Sem destino derivado, monta o histórico de chamados `resolvido` ou `fechado` do colaborador para `contexto_json.data_turno`, excluindo `disputa_supervisao`. Havendo históricos, para pergunta datada em dia que o juiz põe no alcance do O261(C), C6 viva esvazia o histórico e C6 morta casa pelo juiz `motivos_atendidos`. Fora desse alcance, inclusive dia entregue ou trancado, o histórico usa os mapas de categoria e subcategoria. `app/chamados/reconciliador.py:838–871,1711–1765` `app/chamados/catalogo/motor.py:30`

| Retorno | Predicado da saída | Fonte |
|---|---|---|
| `False` | Sem destino vivo derivado e sem data da pergunta nem data no contexto do motivo. | `app/chamados/reconciliador.py:1711–1725` |
| `True` | Histórico vazio e `lavrar_sem_turno_aberto` consegue carimbar. É o c1, que pode deixar a FK nula. | `app/chamados/reconciliador.py:403–417,1766–1775` |
| `True` | Histórico vazio, c1 devolve falso e `reemitir_chamado_do_dia` consegue costurar a pergunta ao chamado vivo. | `app/chamados/reconciliador.py:1032–1080,1774–1777` |
| `False` | Histórico vazio e c1 e c2 devolvem falso, ou o histórico tem mais de um candidato. | `app/chamados/reconciliador.py:1766–1779` |
| `False` | Histórico único e a costura recusa, ou a cerca veta a escrita, desfaz o ato e registra a retenção no chamado histórico. | `app/chamados/reconciliador.py:1800–1816` |
| `True` | Histórico único, costura aceita, S128 retorna normalmente e a cerca libera. A porta descarta o retorno do S128, que pode ter carimbado zero perguntas. | `app/chamados/reconciliador.py:1800–1816` `app/chamados/services/disputa_emissao.py:2033–2042,2087–2122` |
| `False` | Há destino vivo derivado, mas `reapontar_resolvedora` devolve recusa. | `app/chamados/reconciliador.py:1821–1825` |
| `True` | Há destino vivo derivado e `reapontar_resolvedora` aceita. | `app/chamados/reconciliador.py:1821–1826` |
| Exceção | O corpo não tem `raise` explícito. Falhas não tratadas das chamadas internas sobem. Os dois parses locais tratam `ValueError` convertendo a data a `None`. A cerca própria converte veto anotado em recusa. Com cerca externa, a exceção do ato pode subir para quem abriu essa cerca. | `app/chamados/reconciliador.py:1697–1826` `app/chamados/services/fio.py:1564–1592` |

O **c1** exige motivo `orfao_14h` ou `saida_sem_entrada`, ausência de resposta pelo juiz, ausência de carimbo, ausência de turno aberto pelo núcleo, C6 morta e carimbo aceito. O **c2** passa pelos gates de motivo, dia, colaborador ativo, isenção, ausência, veto da célula e motor. Para nascer ou renascer, aplica também horizonte, protesto que cobra, regra da parcial só lista e régua. Para os dois motivos de intervalo, exige C6 viva e dia dentro do alcance O261(C). Por fim precisa obter chamado vivo e passar pela costura. Como `abrir` vem antes da costura, `False` no c2 pode vir depois de uma escrita no chamado. `app/chamados/reconciliador.py:333–417,838–880,930–1080`

**Chamadores diretos na cópia** são `gerar_perguntas_disputa` na emissão e `reconciliar_perguntas_orfas_colab` no laço por colaborador. A batida chega ao laço pelo `post_save` de `Batida`. Também chega o `on_commit` de um chamado filho que sai de `VIVOS`, quando há pai vivo do mesmo colaborador que lista o PK do filho. Execuções concretas para as quatro células [nao verificado]. `app/chamados/services/disputa_emissao.py:468–478` `app/chamados/reconciliador.py:1583–1588,1674–1685` `app/ponto/signals.py:211–223` `app/chamados/signals.py:351–371`

**Trilha.** A costura grava destino, veredito lavrado e evento de célula. A porta escreve log `INFO` em recusas de costura e, no veto do histórico, usa `registrar_retencao` para gravar `retratacao_retida_competencia_entregue` no chamado. Ramos delegados podem gravar trilhas de nascimento de chamado, fechamento de disputa e `fecho_sem_credor` em pergunta. A autoria humana da tentativa precisa ser registrada pelo chamador. `app/chamados/services/fio.py:1082–1095` `app/chamados/reconciliador.py:493–509,1800–1826` `app/chamados/models.py:1037–1046,2199–2207` `app/chamados/services/disputa_emissao.py:2190–2202`

A porta existente para a sessão é **`core.observ.evento`**, com `modelo="PerguntaDisputa"`, `objeto_id=p.pk`, `ator` igual ao objeto `User` do executor e o motivo em `descricao`. `LogAuditoria` guarda `usuario` e `criado_em`. O helper de request `core.auditoria.registrar_log` delega a ela. A sonda usa o precedente de levantar erro dentro do `atomic` se `evento` devolver `None`. `app/core/observ.py:94–139` `app/core/models.py:295–307` `app/core/auditoria.py:8–30` `app/chamados/services/fio.py:1202–1208` `app/chamados/services/validacao.py:395–405,497–498`

## 2. Elegibilidade e campos

| Seletor | O que exige |
|---|---|
| `perguntas_orfas_reatavies(colab)` | `chamado_destino` e `materializada_em` nulos, disputa aberta do colaborador e motivo em `orfao_14h`, `saida_sem_entrada`, `intervalo_saida`, `intervalo_volta`. `app/chamados/reconciliador.py:1637–1661` `app/chamados/models.py:4169–4170` |
| `_orfas_abertas(colaborador_ids=None)` | `materializada_em` e `validada_em` nulos, disputa aberta, `data_evento` presente e motivo `orfao_14h` ou `saida_sem_entrada`. Aceita qualquer valor de `chamado_destino`, com recorte de colaboradores opcional. `app/chamados/reconciliador.py:1162–1176` |

Com destino nulo, **disputa fechada, `materializada_em` preenchido ou motivo fora dos quatro** já bastam, cada um, para ficar fora dos dois universos. `validada_em` preenchido, `data_evento` nulo ou motivo de intervalo podem tirar só do universo do carimbo. Os dois seletores leem as condições acima, sem filtrar `via_resolucao` ou `veredito_lavrado`. `app/chamados/reconciliador.py:1166–1176,1654–1661` `app/chamados/models.py:4169–4170`

Na **pergunta**, “sem via” corresponde a `via_resolucao == ""` e veredito nunca lavrado a `veredito_lavrado == ""`. `julgado_em` é o instante da lavra. Na **célula**, os campos são `veredito` e `veredito_via`, ambos anuláveis. Há ainda o sentido de **resposta sem veredito**, que usa a validação da leitura composta, a materialização, o juiz de resposta e a tranca. A sonda mostra os campos da pergunta e da célula. Qual leitura produziu a descrição raspada [nao verificado]. `app/chamados/models.py:2040–2045,2872–2906` `app/escala/models.py:1782–1785`

## 3. Vigia da frota e o zero histórico

- **Estoque para reate.** `reconciliar_chamados.Command.handle` soma `perguntas_orfas_reatavies(c).count()` para todos os colaboradores do tenant quando roda sem `--colab`. A configuração agenda `reconciliar_chamados --alarme` a cada 15 minutos e grava a saída em `logs/reconciliador.log`. É a linha para ler o número já medido. Ela conta o universo elegível do item 2, inclui colaboradores com evento pendente, dispensa o C6 e a tentativa de reate, e fica fora da soma do alarme. O placar de corridas grava metadados da execução e o código de saída. `app/chamados/management/commands/reconciliar_chamados.py:57–62,89–124` `app/config/crons.py:17–21,29,395,1908–1917` `bin/cron_run.sh:2–13`
- **Fio solto.** O auditor diário das 07:18 também publica `I8` em `logs/invariantes.log` e no HTML. Ele parte de perguntas com destino nulo, motivo atendível, colaborador ativo e C6 viva. A faixa `I8o` registra à parte os casos com ao menos dois candidatos e nenhum `entrada_timestamp`. `app/config/crons.py:542–543` `app/chamados/management/commands/auditar_invariantes_chamados.py:51–95,818–831,860–872,1866–1884`
- **`332 → 0` de 29/10.** O BACKLOG resume o pouso `ac721607`. O RELATO documenta `sem_evento=332`, semeadura de 379 eventos, 332 perguntas encerradas e `sem_evento=0` depois. A função continua sendo `orfas_pela_lampada_sem_evento`. Hoje ela mede o mesmo resíduo de carimbo por lâmpada. Em `_orfas_abertas`, exige pergunta sem resposta pelo juiz, lâmpada do próprio marco acesa, C6 viva, dia passado e encerrado, e colaborador sem evento vivo pendente. A comparação sustentada pelo dado fornecido é **`sem_evento` 0 em 29/10 e 0 em 27/11**. O estoque `orfas reataveis` de hoje está [nao verificado]. `app/docs/BACKLOG.md:17,810` `app/docs/RELATO.md:7010–7015` `app/chamados/reconciliador.py:1182–1233,1325–1341`

## 4. Por que o cron da manhã não reata

O **06:40** era o horário antigo. Desde 29/09, a configuração encadeia `reconciliar_perguntas_orfas` ao fim de `disparar_perguntas_competencia`, com `--alarme` e saída em `perguntas_orfas.log`. O comando sempre conta e imprime `[VIGIA] sem_evento=N (esperado 0)`. Com `--alarme` e `N > 0`, também imprime até 20 PKs em stderr e levanta `SystemExit(2)`. Sem a flag, termina depois da contagem. O corpo tem apenas leitura e saída. O BACKLOG registra que a falta de redirecionamento do log foi sanada e que o arquivo já voltara a ser gravado em 08/11. `app/config/crons.py:479–485,1528–1531,1548–1554,1646–1651` `app/ponto/management/commands/reconciliar_perguntas_orfas.py:24–36` `app/docs/BACKLOG.md:811`

Em **23/10**, VARREDURA 1b-iii parte 3 extraiu a porta do laço antigo e ligou o reate ao nascimento na emissão. O RELATO cita a cópia `2263122b`. Em **29/10**, `ac721607` moveu o **carimbo fantasma** ao consumidor de `EventoCelula`. Em **05/11**, JUIZ-VARREDURA-3, documentado no deploy `de9fd69b`, converteu o `reconciliar_chamados` de `*/15`, que ainda reatava, em vigia. `app/chamados/reconciliador.py:1688–1695` `app/docs/RELATO.md:8882–8888,7010–7015,13032–13041` `app/config/crons.py:2094–2104` `app/chamados/management/commands/reconciliar_chamados.py:1–16`

A emissão pode criar com FK nula quando o derivador termina com zero ou múltiplos candidatos após o desempate. A tentativa imediata só ocorre se a pergunta foi **criada nessa chamada**, segue com FK nula e tem motivo do grupo do dia. Pode ficar sem vínculo se a tentativa recusa, falha no savepoint ou só carimba pelo c1. Uma emissão posterior de outra pergunta não percorre o estoque nesse bloco. O consumidor de `EventoCelula` faz o carimbo e chama a fábrica para os dias do lote, e a fábrica deduplica o fato já representado mesmo quando sua pergunta segue sem destino. O estoque elegível volta à porta no laço do colaborador, acionado pela batida ou pelo evento de filho descrito acima. Se a ambiguidade some sem um desses gatilhos, ou a pergunta já está fora do seletor do laço, não há nova tentativa por esses caminhos. Qual condição ocorreu nas 16 [nao verificado]. `app/chamados/services/disputa_emissao.py:191–244,417–478` `app/chamados/reconciliador.py:1523–1537,1583–1588,1654–1685,1774–1775` `app/chamados/services/fabrica_da_celula.py:138–174,207–219,288–302` `app/chamados/management/commands/reconciliar_chamados.py:24–25`

## 5. Sonda para o shell do tenant

Abra `python -B manage.py tenant_command shell --schema=SEU_SCHEMA` e cole o `exec` abaixo. O modo padrão é `ensaio`. Em `aplicar`, preencha `ATOR_PK` com o PK do usuário executor. `FIM` conta como reatada a pergunta que saiu de FK nula para FK preenchida com retorno `True`. A foto do ensaio é tirada antes do rollback de cada pergunta. A porta expõe um booleano e alguns logs de ramo, sem diagnóstico estruturado que cubra todas as recusas. Por isso a saída por tentativa traz retorno e estado. Sintaxe conferida. Execução com Django e banco [nao verificado]. `bin/sonda_frota.sh:45–46` `app/core/observ.py:108–137` `app/chamados/reconciliador.py:1774–1826`

A sonda valida os quatro pares de PK e data, as quatro perguntas por célula e o vínculo ainda nulo sob a trava existente. Em `aplicar`, registra também as tentativas que devolvem `False`, pois o c2 pode escrever antes da recusa. Ela interrompe o `aplicar` se encontrar regularização externa confirmada e pendente do colaborador. O fechamento de uma disputa pode retomar essa regularização depois do commit e escrever batidas. A checagem lê a pendência existente naquele instante. Uma confirmação concorrente pode gravar seu estado por outra transação, e a ausência dessa concorrência está [nao verificado]. `app/chamados/models.py:2930–2949` `app/chamados/reconciliador.py:410–417,1032–1080,1800–1807` `app/chamados/signals.py:122–146` `app/chamados/services/regularizacao_ext.py:71–82,122–156,211–225,348–357`

```python
exec(r"""
from collections import Counter
from datetime import date

from django.conf import settings
from django.contrib.auth import get_user_model
from django.db import connection, transaction
from django.utils import timezone

from chamados.juizes import pergunta_viva
from chamados.models import PerguntaDisputa, RegularizacaoExterna
from chamados.reconciliador import (
    _orfas_abertas,
    perguntas_orfas_reatavies,
    reatar_pergunta_orfa,
)
from chamados.services.validacao import TrilhaNaoGravada
from core.observ import evento
from escala.models import CelulaDia

MODO = "ensaio"
ATOR_PK = None  # Em aplicar, PK do auth.User de quem executa.
AVAL = "ORFAS-VOLTOU-2711-1045"
DIAS = {
    123535: date(2026, 11, 1),
    123541: date(2026, 11, 7),
    123547: date(2026, 11, 13),
    123553: date(2026, 11, 19),
}


class DesfazerEnsaio(Exception):
    pass


def local(instante):
    return timezone.localtime(instante).isoformat() if instante is not None else None


def estado(p):
    return {
        "chamado_destino_id": p.chamado_destino_id,
        "materializada_em": local(p.materializada_em),
    }


def executar():
    if MODO not in ("ensaio", "aplicar"):
        raise ValueError("MODO deve ser ensaio ou aplicar")
    schema = getattr(connection, "schema_name", None)
    if not schema or schema == getattr(settings, "PUBLIC_SCHEMA_NAME", "public"):
        raise RuntimeError("Abra o shell no schema do tenant")
    if not transaction.get_autocommit():
        raise RuntimeError("A sonda precisa de uma transacao independente por pergunta")

    celulas = CelulaDia.objects.select_related("colaborador").in_bulk(DIAS)
    if {pk: c.data for pk, c in celulas.items()} != DIAS:
        raise RuntimeError("PKs e datas das celulas divergem do aval")
    donos = {c.colaborador_id for c in celulas.values()}
    if len(donos) != 1:
        raise RuntimeError("As quatro celulas precisam ter o mesmo colaborador")
    colab = celulas[next(iter(DIAS))].colaborador
    print("colaborador_id", colab.pk, "empresa_id", colab.empresa_id)
    for c in celulas.values():
        print("celula_id", c.pk, "data", c.data, "celula.veredito", c.veredito,
              "celula.veredito_via", c.veredito_via)

    qs = PerguntaDisputa.objects.select_related("celula", "disputa__colaborador")
    perguntas = list(qs.filter(celula_id__in=DIAS).order_by("celula_id", "pk"))
    pks = [p.pk for p in perguntas]
    reataveis = set(perguntas_orfas_reatavies(colab).filter(pk__in=pks)
                    .values_list("pk", flat=True))
    abertas = set(_orfas_abertas([colab.pk]).filter(pk__in=pks)
                  .values_list("pk", flat=True))

    for p in perguntas:
        print({
            "pk": p.pk, "celula_id": p.celula_id, "motivo": p.motivo,
            "data_evento": p.data_evento.isoformat() if p.data_evento else None,
            **estado(p), "validada_em": local(p.validada_em),
            "via_resolucao": p.via_resolucao, "veredito_lavrado": p.veredito_lavrado,
            "disputa_id": p.disputa_id, "disputa.fechada_em": local(p.disputa.fechada_em),
            "perguntas_orfas_reatavies": p.pk in reataveis,
            "_orfas_abertas": p.pk in abertas, "pergunta_viva": pergunta_viva(p),
        })

    if Counter(p.celula_id for p in perguntas) != {pk: 4 for pk in DIAS}:
        raise RuntimeError("A lista atual diverge das quatro perguntas por celula do aval")

    ator = None
    if MODO == "aplicar":
        if ATOR_PK is None:
            raise ValueError("Preencha ATOR_PK com o PK do usuario executor")
        ator = get_user_model().objects.get(pk=ATOR_PK)

    n_reatadas = 0
    for original in perguntas:
        try:
            with transaction.atomic():
                PerguntaDisputa.trava_do_colaborador(colab.pk)
                p = qs.get(pk=original.pk)
                if (p.celula_id != original.celula_id or
                    p.colaborador_id != colab.pk or
                    p.disputa.colaborador_id != colab.pk or
                    p.chamado_destino_id is not None):
                    raise RuntimeError(f"Pergunta {p.pk} diverge do alvo orfao do aval")

                if MODO == "aplicar" and RegularizacaoExterna.objects.filter(
                    colaborador_id=colab.pk, status="confirmada", materializada_em__isnull=True,
                ).exists():
                    raise RuntimeError(f"Colaborador {colab.pk} tem regularizacao confirmada pendente")

                antes = estado(p)
                retorno = reatar_pergunta_orfa(p, p.disputa.colaborador)
                p.refresh_from_db()
                depois = estado(p)
                resultado = {"pk": p.pk, "retorno": retorno, **depois}
                if MODO == "ensaio":
                    raise DesfazerEnsaio(resultado)

                log = evento(
                    "reatar_pergunta_orfa", "PerguntaDisputa", p.pk,
                    descricao=f"{AVAL} | Tentativa de reatar a pergunta pela porta existente",
                    antes=antes, depois={**depois, "retorno": retorno}, ator=ator,
                )
                if log is None:
                    raise TrilhaNaoGravada(f"{AVAL} | Pergunta {p.pk}")
        except DesfazerEnsaio as ensaio:
            resultado, = ensaio.args
        print(resultado)
        if resultado["retorno"] is True and resultado["chamado_destino_id"] is not None:
            n_reatadas += 1
    print(f"FIM {len(perguntas)} {n_reatadas}")


executar()
""")
```

## 27/11 10:44 · sessao 52b726a95a4a · E4C-F5-INSTANTE · construtor · origem=Code

**Parei conforme a condição do P3.** Em `validar_pergunta`, o veto detectado após a tentativa de plantio desfaz o `atomic` que adquiriu a trava do dono. Só depois do rollback a função relê a pergunta e chama `_recusa_da_entrega`, que chama `trilha_do_veto_k6`. A trava adquirida naquele bloco já caiu. [validacao.py:755](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/validacao.py:755) · [validacao.py:834](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/validacao.py:834) · [validacao.py:237](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/validacao.py:237) · [trava_do_colaborador.py:16](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/trava_do_colaborador.py:16)

- **Caminhos com trava.** O veto inicial da validação chama a trilha dentro do `atomic` que tomou a trava. O laço da materialização e o fluxo do passivo também tomam a trava antes de chegar a `_materializar_uma`. [validacao.py:755](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/validacao.py:755) · [validacao.py:795](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/validacao.py:795) · [materializacao.py:2228](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/materializacao.py:2228) · [materializacao.py:2300](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/materializacao.py:2300) · [materializacao.py:2318](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/materializacao.py:2318) · [passivo_validado.py:393](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/passivo_validado.py:393)
- **M16.** Pela leitura do código, o mutante apaga as fotos e a descrição. A asserção de fotos completas reprova as duas metades em cada porta. Execução **[nao verificado]**. [e4c_f5_c4red2_mutantes.py:253](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:253) · [e4c_f5_c4red2_mutantes.py:474](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:474) · [test_e4c_f5_c4_red.py:996](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:996)
- **Multiempresa.** Controle esperado por leitura estática. A porta passa todas as Empresas do lote à guarda, que encontra o primeiro dono e lança `EntregaNaoTravada` pela Empresa registrada depois dele. O teste novo ficou pendente com a parada. Execução **[nao verificado]**. [models.py:4868](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/models.py:4868) · [export.py:613](/home/ronald/wt-codex-e4c-f5-instante-012038/app/folha/export.py:613) [nao verificado]

Nenhum arquivo foi alterado, nenhum `except` foi adicionado e nenhum teste foi executado. A deduplicação, o isolamento e o M44 ficaram pendentes. [nao verificado]

Comandos dos casos existentes para o Code. [test_e4c_f5_c4_red.py:1034](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:1034) · [e4c_f5_c4red2_mutantes.py:474](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:474)

```bash
PYTHONDONTWRITEBYTECODE=1 python manage.py test chamados.tests.test_e4c_f5_c4_red.Item13PorPKTest.test_RED_item13_veto_repetido_deixa_uma_linha_de_trilha
PYTHONDONTWRITEBYTECODE=1 python manage.py test chamados.tests.e4c_f5_c4red2_mutantes.M16TrilhaVazia.test_mordida
```

## 27/11 10:44 · sessao fd1effd0b35b · - · - · origem=Ronald (tmux)

- A porta retorna `True` quando a costura no destino derivado funciona, quando a costura no único histórico funciona e S128 termina, quando c1 lavra ou quando c2 reemite e costura. Retorna `False` sem data no caminho histórico, quando há vários históricos, quando não há histórico e c1/c2 recusam, quando a costura recusa ou quando a cerca desfaz o histórico. O corpo não tem `raise` explícito. Os dois parses tratam `ValueError`, e falhas não tratadas propagam. A cerca própria converte veto em estado de recusa. `app/chamados/reconciliador.py:1711–1826`, `app/chamados/services/fio.py:1564–1592`.
- O histórico considera chamados encerrados do colaborador com o `data_turno` em questão, exceto `disputa_supervisao`. Dentro do alcance O261(C), uma pergunta viva esvazia essa lista e o encaixe de motivo usa `motivos_atendidos`. Fora do alcance, usa os mapas legados. `app/chamados/reconciliador.py:838–871`, `app/chamados/reconciliador.py:1740–1765`.
- A costura não lavra auditoria de autoria da sessão. Grava destino, campos do veredito e desperta outbox. A recusa de costura tem log `INFO` e o veto histórico pode registrar `retratacao_retida_competencia_entregue` no chamado. S128 pode fechar outras perguntas do colaborador e gravar `fecho_sem_credor`. Para autoria humana em shell, a porta existente é `core.observ.evento`, com `ator` como objeto `User` e motivo em `descricao`. `LogAuditoria.criado_em` registra o instante. O precedente `BUG-PAR-103` verifica o retorno da trilha dentro da mesma transação e lança erro se for `None`. `app/chamados/services/fio.py:1082–1095`, `app/chamados/reconciliador.py:493–509`, `app/chamados/reconciliador.py:1800–1825`, `app/chamados/services/disputa_emissao.py:2158–2202`, `app/core/observ.py:94–139`, `app/core/models.py:295–307`, `app/chamados/services/validacao.py:395–405`.
- `perguntas_orfas_reatavies` seleciona destino e materialização nulos, disputa aberta do colaborador e um dos quatro motivos do dia. `_orfas_abertas` seleciona materialização e validação nulas, disputa aberta, dia presente e os dois motivos `orfao_14h` e `saida_sem_entrada`. Qualquer destino cabe neste segundo filtro. Disputa fechada, materialização presente ou motivo fora dos quatro podem deixar destino nulo e excluir dos dois. `via_resolucao == ''` e `veredito_lavrado == ''` representam os campos relatados. `app/chamados/reconciliador.py:1162–1176`, `app/chamados/reconciliador.py:1654–1661`, `app/chamados/models.py:4169–4170`, `app/chamados/models.py:2872–2906`.
- Chamadores diretos são emissão e o laço de conciliação por colaborador. Este laço é acionado por `post_save` de batida e `on_commit` quando um filho sai de `VIVOS` e tem pai vivo referenciando-o. O outbox chega à emissão via fábrica, que exclui da nova emissão um fato que já tem pergunta canônica, mesmo com destino nulo. Na emissão, a tentativa é somente para criada agora, destino nulo e motivo do grupo dos quatro. Nenhuma evidência de que esses gatilhos tenham ou não ocorrido para as 16 foi fornecida. `app/chamados/services/disputa_emissao.py:462–478`, `app/chamados/reconciliador.py:1534–1537`, `app/chamados/reconciliador.py:1587–1588`, `app/chamados/reconciliador.py:1675–1684`, `app/ponto/signals.py:211–223`, `app/chamados/signals.py:351–371`, `app/chamados/services/fabrica_da_celula.py:138–174`, `app/chamados/services/fabrica_da_celula.py:207–219`, `app/chamados/services/fabrica_da_celula.py:288–302`. Última frase [nao verificado].
- O `reconciliar_chamados --alarme` de `*/15` publica `orfas reataveis (UNIVERSO, MEDIDA ...)` no `reconciliador.log`: soma o seletor por colaborador e a soma de alarme fica separada dessa medida. O auditor diário 07:18 traz ainda I8, que aplica C6, motivo atendível, colaborador ativo, destino nulo e separa I8o. O `332 → 0` documentado em 29/10 era `orfas_pela_lampada_sem_evento`, função que permanece consultada por `reconciliar_perguntas_orfas`, com ou sem `--alarme`. A flag só muda a saída quando há residual, com código 2. Conceitos, recortes e épocas estão documentados. `app/config/crons.py:395`, `app/config/crons.py:542`, `app/chamados/management/commands/reconciliar_chamados.py:97–124`, `app/chamados/management/commands/auditar_invariantes_chamados.py:818–831`, `app/chamados/management/commands/auditar_invariantes_chamados.py:860–872`, `app/docs/RELATO.md:7010–7015`, `app/chamados/reconciliador.py:1325–1341`, `app/ponto/management/commands/reconciliar_perguntas_orfas.py:24–36`.
- A porta desceu à emissão em 23/10 (`2263122b`) e a varredura que reatava foi rebaixada em 05/11 (`de9fd69b`). O ENC do carimbo foi rebaixado em 29/10 (`ac721607`) e roda por fim do gatilho, sem minuto próprio. `app/chamados/reconciliador.py:1691–1695`, `app/docs/RELATO.md:8882–8888`, `app/docs/RELATO.md:13032–13041`, `app/docs/RELATO.md:7010–7015`, `app/config/crons.py:1528–1531`, `app/config/crons.py:1548–1554`.
- A sonda deve tirar a foto com uma instância nova por chamada e usar `refresh_from_db()` dentro do atomic antes do rollback, pois S128 trabalha com outras instâncias. A saída booleana não carrega diagnóstico completo. `True` pode ser c1 sem novo destino, e `False` no c2 pode vir depois de `abrir`. A autoria pode registrar a tentativa e seu retorno. `app/chamados/juizes.py:911–952`, `app/chamados/services/disputa_emissao.py:2038–2042`, `app/chamados/reconciliador.py:403–417`, `app/chamados/reconciliador.py:1032–1044`, `app/chamados/reconciliador.py:1076–1080`, `app/chamados/reconciliador.py:1774–1826`.

## 27/11 10:43 · sessao 9df9f5737cd0 · - · - · origem=Ronald (tmux)

O cron `reconciliar_chamados --alarme` publica `orfas reataveis (UNIVERSO, MEDIDA -- o reatar nao tem predicado seco)` a cada 15 minutos em `logs/reconciliador.log`, no schema `juliani`. Soma `perguntas_orfas_reatavies(colab).count()` para todos os colaboradores quando executado sem `--colab`. O conjunto tem destino nulo, disputa aberta, `materializada_em` nulo e um dos quatro motivos do dia. A contagem dispensa `pergunta_viva`, tentativa de reatamento e ausência de evento pendente. Seu valor fica fora da soma do alarme. O placar de corridas armazena início, fim, duração e código de saída. [app/config/crons.py:17-21,395,1908-1917; app/chamados/management/commands/reconciliar_chamados.py:57-62,89-124; app/chamados/reconciliador.py:1637-1661; app/chamados/models.py:4169-4170; bin/cron_run.sh:2-13]

O auditor diário também publica `I8`, que mede perguntas vivas pelo C6, de colaboradores ativos, com destino nulo e motivo atendível. `I8o` destaca à parte o piso de casos com pelo menos dois candidatos daquele dia e nenhum `entrada_timestamp`. A execução das 07:18 gera `logs/invariantes.log` e `/app/media/invariantes_chamados.html`. [app/config/crons.py:542-543; app/chamados/management/commands/auditar_invariantes_chamados.py:51-95,818-831,860-872,1866-1884]

O `332 → 0` do BACKLOG foi `sem_evento` antes e depois da semeadura e do consumo de eventos no pouso `ac721607`, documentado no RELATO. A função `orfas_pela_lampada_sem_evento` ainda existe. Ela conta as perguntas de `_orfas_abertas` que `veredito_orfa` manda encerrar e cujos colaboradores estão sem evento pendente. O universo `_orfas_abertas` inclui perguntas com destino preenchido. O dado raspado permite comparar `sem_evento=0` de hoje com `sem_evento=0` de 29/10. O total de hoje de candidatas a reatar e o I8 são [nao verificado]. [app/docs/BACKLOG.md:17,810; app/docs/RELATO.md:7010-7015; app/config/crons.py:2102-2104; app/chamados/reconciliador.py:1166-1176,1182-1233,1325-1341; app/ponto/management/commands/reconciliar_perguntas_orfas.py:29-36]

A cópia confirma o gatilho indireto adicional `_filho_morto_acorda_o_pai`. Quando um filho sai de `VIVOS` e existe pai vivo do mesmo colaborador que lista seu PK, o receiver registra callback `on_commit` que chama `reconciliar_chamados_colab`, que percorre as órfãs elegíveis. [app/chamados/signals.py:351-371; app/chamados/reconciliador.py:1583-1588,1676-1684]

## 27/11 10:43 · sessao 57695d33e151 · - · - · origem=Ronald (tmux)

- O antigo minuto 06:40 saiu em 29/09. O comando atual roda encadeado após `disparar_perguntas_competencia`, com `--alarme` e log `perguntas_orfas.log`. Com ou sem o argumento, mede `orfas_pela_lampada_sem_evento` e imprime o tamanho. Com o argumento e resultado positivo, também escreve até 20 PKs em stderr e levanta `SystemExit(2)`. [crons.py:479–485](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/config/crons.py:479) · [crons.py:1551–1554](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/config/crons.py:1551) · [reconciliar_perguntas_orfas.py:24–36](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/management/commands/reconciliar_perguntas_orfas.py:24)
- Em 23/10, VARREDURA classe B 1b-iii parte 3, a emissão passou a chamar a porta extraída do antigo laço de `reconciliar_chamados`. O relato cita a cópia `2263122b`. Em 29/10, `ac721607` moveu o carimbo `fantasma` ao consumidor de `EventoCelula` e converteu `reconciliar_perguntas_orfas` em vigia. Em 05/11, JUIZ-VARREDURA-3, `reconciliar_chamados` também virou vigia. [reconciliador.py:1688–1695](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/reconciliador.py:1688) · [RELATO.md:8882–8888](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/RELATO.md:8882) · [LAPIDES.md:785](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/LAPIDES.md:785) · [reconciliar_chamados.py:1–16](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/management/commands/reconciliar_chamados.py:1)
- A emissão chama diretamente para pergunta recém-criada com destino nulo e motivo do dia, em savepoint próprio. O receiver `post_save` da batida e o receiver de chamado filho que sai de `VIVOS` também chegam ao reatar pelo laço do colaborador. O segundo exige pai vivo do mesmo colaborador que liste o filho no contexto e chama após commit. [disputa_emissao.py:462–478](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/disputa_emissao.py:462) · [ponto/signals.py:211–223](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/signals.py:211) · [chamados/signals.py:351–371](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/signals.py:351) · [reconciliador.py:1587–1588, 1676–1684](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/reconciliador.py:1587)
- O derivador retorna destino nulo quando termina com zero ou múltiplos candidatos. A pergunta pode ficar sem vínculo quando o reatar da emissão recusa, falha ou apenas carimba pelo c1. Emissão posterior de outra linha não revisita a existente pelo bloco de nascimento. Batida e filho só percorrem as elegíveis do colaborador. O outbox carimba e chama a fábrica para datas do lote, cuja emissão deduplica fatos já representados. Assim, mudança de elegibilidade sem batida, sem evento elegível do filho e sem novo nascimento que passe pela porta pode deixar o estoque sem nova tentativa de reatar. Causa concreta das 16 e eventos posteriores [nao verificado]. [disputa_emissao.py:191–244, 468–478](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/disputa_emissao.py:191) · [reconciliador.py:341–417, 1523–1537, 1640–1684](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/reconciliador.py:1523) · [fabrica_da_celula.py:139–157, 288–302](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/fabrica_da_celula.py:139)
- O histórico literal do `332 → 0` é o `sem_evento` do carimbo. O relato registra 332 antes, 379 eventos semeados, 332 perguntas encerradas e 0 depois. O BACKLOG registra o retorno do log em 08/11. [RELATO.md:7010–7015](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/RELATO.md:7010) · [BACKLOG.md:811](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/BACKLOG.md:811)

## 27/11 10:42 · sessao 8bc3e85087ce · E4C-F5-INSTANTE · verificador · origem=Code

## E4C-F5-INSTANTE · verificação do delta

Conferi `ad10296c4` sobre `d5b417695` na cópia indicada, em somente leitura. As conclusões sobre a mordida dos REDs vêm do código e da mutação correspondente. Execução dos testes, `makemigrations --check`, implantação e ocorrência em produção ficam **[nao verificado]**. O material fornecido contém o pedido, os recortes de leis, o registro de juízes e o diff. As perguntas com dados montados nos testes são fixtures. `app/chamados/tests/test_e4c_f5_c4_red.py:121`, `app/chamados/tests/test_e4c_f5_c4_red.py:962`.

### 1. P1 a P5

| Achado | Resultado do delta e mordida |
|---|---|
| **P1 · alta** | **Cura na origem o falso acordo de instantes.** Guarda e comparador reduzem as mesmas linhas reais. A guarda acrescenta os atos que o carimbo está prestes a gravar. M36 repõe `composta` e o RED do `carimbar` pega o defeito original em resposta e validação quando a coluna está **depois** do ato. O inverso é controle: a linha real posterior já vencia na leitura antiga. M37 pega o mesmo defeito nas duas colunas de uma instância relida do ORM. Ele chama a guarda diretamente. O RED anterior de absorção verifica a reconferência e o rollback pela porta em outro cenário. `app/chamados/models.py:4620`, `app/chamados/models.py:4637`, `app/chamados/models.py:4688`, `app/chamados/tests/test_e4c_f5_c4_red.py:121`, `app/chamados/tests/test_e4c_f5_c4_red.py:226`, `app/chamados/tests/test_e4c_f5_c4_red.py:484`, `app/chamados/tests/e4c_f5_c4red2_mutantes.py:685`. |
| **P2 · média** | **Cura na origem os dois desvios da lista.** O DRY e a porta consultam `veto_do_estoque` por campo. M38 tira essa consulta do DRY e a conversa volta à lista. M39 restaura a precedência por PK e o `b` novo volta a esconder o `b` indeterminado do outro campo. As duas asserções mordem o comportamento original. O quadro agregado continua tendo precedência por PK, coberta separadamente. `app/chamados/models.py:4828`, `app/chamados/models.py:4888`, `app/chamados/management/commands/estoque_ato_x_coluna.py:46`, `app/chamados/management/commands/estoque_ato_x_coluna.py:55`, `app/chamados/tests/test_e4c_f5_c4_red.py:498`, `app/chamados/tests/test_e4c_f5_c4_red.py:705`, `app/chamados/tests/test_e4c_f5_c4_red.py:783`, `app/chamados/tests/e4c_f5_c4red2_mutantes.py:701`. |
| **P3 · média** | **Sem cura.** Duas validações idênticas vetadas chamam o escritor duas vezes. O RED novo exige uma única linha e pega a duplicata original. O teste sem banco prova uma chamada com pergunta, autor e motivo em **uma tentativa**. M16 corrompe as fotos e a descrição. A repetição segue sem mutante próprio. `app/chamados/services/validacao.py:237`, `app/chamados/services/validacao.py:795`, `app/chamados/services/materializacao.py:846`, `app/core/observ.py:133`, `app/chamados/tests/test_e4c_f5_c4_red.py:1034`, `app/chamados/tests/test_bug_par_sem_banco.py:153`, `app/chamados/tests/e4c_f5_c4red2_mutantes.py:253`. |
| **P4 · média** | **Cura a lacuna de prova para dados persistidos. A cobertura dos pendentes em memória continua parcial.** O teste novo exercita os dois carregadores reais: `exigencia_das_perguntas` pela decisão retornada e `carga_da_tarefa` pelos dados que ela carregou em `_leituras` e `_passivo`. M41, M42 e M43 retiram, respectivamente, os cortes da linha, da coluna e do passivo. O teste novo já tem esses cortes na base. A injeção antiga continua sendo a prova específica de `_atos_do_carimbo` pendentes, que as duas faces públicas não recebem ao recarregar do ORM. `app/chamados/services/tarefa_do_dia.py:393`, `app/chamados/services/tarefa_do_dia.py:419`, `app/chamados/services/tarefa_do_dia.py:437`, `app/chamados/services/tarefa_do_dia.py:475`, `app/chamados/services/tarefa_do_dia.py:537`, `app/chamados/tests/test_e4c_f5_c4_red.py:260`, `app/chamados/tests/test_e4c_f5_c4_red.py:336`, `app/chamados/tests/e4c_f5_c4red2_mutantes.py:733`. |
| **P5 · média** | **Cura na origem a aceitação da ordem inversa pela porta.** O RED registra dono → Empresa no mesmo `atomic` e exige `EntregaNaoTravada` pelo motivo específico. M40 desliga só a conferência da ordem e repõe a aceitação antiga. A conferência ocorre antes da leitura do K6 e do `INSERT`. A aquisição das duas travas já aconteceu quando a porta confere. `app/chamados/models.py:4773`, `app/chamados/models.py:4874`, `app/chamados/models.py:4892`, `app/folha/export.py:602`, `app/chamados/tests/test_e4c_f5_c4_red.py:919`, `app/chamados/tests/e4c_f5_c4red2_mutantes.py:723`. |

### 2. Quem escreve e quem lê

| Campo ou valor | Escrita e leitura |
|---|---|
| **`respondida_em`, `resposta_colab`, `validada_em`, `validada_por_id`** | A resposta humana escreve instante e texto e limpa a validação anterior. A automática escreve instante e texto. O carimbo pela trilha escreve o instante de resposta. `_carimbar_validacao` escreve instante e autor da validação. `carimbar` preenche os instantes faltantes conforme a opção e o texto, e pode restaurar a foto em memória. O nascimento aceita assinaturas e registra os atos correspondentes. Reabertura/recusa, devolução e ressuscitação limpam os campos aplicáveis. A guarda lê os dois instantes crus, `linhas_da_projecao_sem_linha` lê também texto e autor, o comparador lê os instantes, e a trilha K6 fotografa texto e instantes. C6 e a validação leem a composta. `app/chamados/services/disputa_emissao.py:1530`, `app/chamados/services/disputa_emissao.py:1661`, `app/chamados/services/disputa_emissao.py:1757`, `app/chamados/services/disputa_emissao.py:1793`, `app/chamados/services/validacao.py:170`, `app/chamados/services/validacao.py:308`, `app/chamados/services/validacao.py:387`, `app/chamados/services/validacao.py:487`, `app/chamados/models.py:1730`, `app/chamados/models.py:3322`, `app/chamados/models.py:3478`, `app/chamados/models.py:3531`, `app/chamados/models.py:4509`, `app/chamados/models.py:4622`, `app/chamados/models.py:4688`, `app/chamados/juizes.py:1499`, `app/chamados/services/validacao.py:768`, `app/chamados/services/materializacao.py:855`. |
| **Linhas reais, pendentes e passivo** | As portas montam os atos e `DecisaoPergunta.registrar` insere as linhas. Resposta humana deixa a marca de manutenção da validação falsa. Resposta automática, carimbo pela trilha e foto do passivo declaram a marca verdadeira. `carimbar` cria e remove `_atos_do_carimbo` na instância. `leitura`, a vista de horizonte e a guarda de instante leem os pendentes. `_reduzir` consome `ato_em`, ordem, PK, `ato`, `ato_texto` e o efeito de limpar a validação. Comparador e reprojeção leem a redução real. A presença de validação continua lendo `composta`, que também incorpora o passivo sintetizado. `app/chamados/models.py:3477`, `app/chamados/models.py:3520`, `app/chamados/models.py:3539`, `app/chamados/models.py:4511`, `app/chamados/models.py:4530`, `app/chamados/models.py:4557`, `app/chamados/models.py:4572`, `app/chamados/models.py:4642`, `app/chamados/models.py:4648`, `app/chamados/models.py:4675`, `app/chamados/models.py:4688`, `app/chamados/models.py:4778`, `app/chamados/services/tarefa_do_dia.py:419`. |
| **`marco`, `entrada_origem`, `entrada_em`, `div`** | `marco_do_estoque` lê o `applied` da 0057. No ato normal, a origem tem default `ato` e a entrada vem do relógio do banco. A 0058 marca o histórico com `estoque` e o marco. O payload de estoque declara os dois valores e `_lote_de_estoque` confere. O comparador produz `(classe, campo, novo)` a partir da redução, da coluna, do marco e de `fonte.entrada_origem`. `entrada_em` serve à marcação histórica e à admissão do payload. DRY, vigia e `_conferir_estoque` consomem `div`. O DRY e a porta passam `div`, `ato`, `p` e `marco` ao veto, que devolve texto de recusa ou `''`. `app/chamados/migrations/0057_decisao_entrada_do_ato.py:15`, `app/chamados/migrations/0058_decisao_entrada_do_ato_historico.py:9`, `app/chamados/models.py:4594`, `app/chamados/models.py:4620`, `app/chamados/models.py:4797`, `app/chamados/models.py:4828`, `app/chamados/models.py:4889`, `app/chamados/services/vigia_ato_x_coluna.py:18`, `app/chamados/management/commands/estoque_ato_x_coluna.py:34`. |
| **Identidade, projeção elegível e K6** | `_nascer` grava a FK da disputa, `conversa` e o dia pelo ponto único `gravar_dia_do_fato`. O dono vem de `DisputaSupervisao.colaborador` e a empresa do cadastro do colaborador. `_conferir_estoque` relê essas relações e compara pergunta/PK, tipo de ato, instante, texto, autor, marca de manutenção e motivo do payload com a foto de `linhas_da_projecao_sem_linha`. Depois aplica o veto e lê K6 por `data_evento` e empresa. A janela fornece mês/ano. `ExportacaoDominio.registrar` e `invalidar` escrevem o estado que `competencia_entregue` lê. O DRY lê a mesma composição. `app/chamados/models.py:1901`, `app/chamados/models.py:3092`, `app/chamados/models.py:3104`, `app/chamados/models.py:3322`, `app/chamados/models.py:4821`, `app/chamados/models.py:4864`, `app/chamados/models.py:4875`, `app/chamados/models.py:4892`, `app/chamados/services/fio.py:950`, `app/colaboradores/views.py:1335`, `app/chamados/services/materializacao.py:803`, `app/chamados/services/passivo_validado.py:91`, `app/folha/models.py:143`, `app/folha/models.py:174`, `app/folha/export.py:647`. |
| **Registros de trava e `antes_de`** | `travar_entrega` adquire a linha da Empresa e registra `_TravaDaEntrega.empresa_id`. `trava_na_ordem` adquire o dono e `_registrar_travados` registra `_TravaDoColaborador.cid` e `aquisicao`. Os leitores do dono são `colaboradores_travados`, `estampa_das_travas` e o predicado local `do_dono`. `registrar` exige a presença dos donos antes de `_conferir_estoque`. `exigir_entrega_travada` exige todas as Empresas do lote antes do **primeiro** sentinela de um dono do lote. Há um único chamador direto na cópia. `travar_entrega` também é chamado pelo fluxo de exportação, pela regeração do TXT e pela revisão de par relâmpago. Esses caminhos continuam adquirindo a trava. O argumento opcional só ativa a ordem para quem o fornece. `app/chamados/models.py:2968`, `app/chamados/models.py:4773`, `app/chamados/models.py:4867`, `app/chamados/services/trava_do_colaborador.py:26`, `app/chamados/services/trava_do_colaborador.py:39`, `app/chamados/services/trava_do_colaborador.py:58`, `app/folha/export.py:573`, `app/folha/export.py:588`, `app/folha/export.py:602`, `app/folha/views.py:134`, `app/folha/management/commands/regerar_txt_dominio.py:67`, `app/ponto/services/par_relampago_na_lista.py:183`. |

### 3. Recusas novas de P1

- **Condição.** A recusa nova alcança o campo em que a redução real ainda dá T1 e a coluna traz T2 posterior sem linha correspondente, quando a síntese antiga fazia a leitura terminar em T2. O carimbo precisa chegar à guarda depois do C6 e da checagem de presença. Legado sem instante vigente no ato e pares coincidentes continuam passando pela face de instante. Classe `a` por ausência ou limpeza pode continuar aparecendo só no comparador. `app/chamados/models.py:4509`, `app/chamados/models.py:4563`, `app/chamados/models.py:4628`, `app/chamados/models.py:4688`, `app/chamados/models.py:3495`, `app/chamados/tests/test_e4c_f5_c4_red.py:147`.
- **Produção.** A irmã na absorção pode ser recusada pelo `carimbar` ou pela reconferência do registro, que relê e levanta `_Reprovada` antes de gravar a `ABSORCAO`. O plantio unitário recebe `RECUSA_PAR_ATO_COLUNA` e desfaz a batida tentada. `materializar_pendentes_aprovadas` deixa de carimbar e pode emitir a trilha de recusa do fecho. Também herdam a guarda a resolução de perguntas por chamado, o fecho sem credor, o reconciliador de turno/célula e o encerramento de fantasma, quando seus filtros deixam a pergunta chegar ao `carimbar`. `carimbar_identidade` tem porta própria. `app/chamados/services/materializacao.py:1123`, `app/chamados/services/materializacao.py:1132`, `app/chamados/services/materializacao.py:1155`, `app/chamados/services/materializacao.py:2113`, `app/chamados/services/materializacao.py:2375`, `app/chamados/services/disputa_emissao.py:2118`, `app/chamados/services/disputa_emissao.py:2192`, `app/chamados/reconciliador.py:408`, `app/chamados/reconciliador.py:699`, `app/chamados/reconciliador.py:1290`, `app/chamados/models.py:3692`, `app/chamados/models.py:3541`.
- **Alta, risco de bloquear conservação legítima de legado.** Existe um caso estático em que a aceitação anterior podia terminar com o par correto: resposta real em T1, resposta legítima do acervo na coluna em T2 > T1, validação vazia e `carimbar(resposta='validar')` depois de T2. A escrita antiga podia registrar a resposta de T2 em `_sem_linha` junto da nova validação. A guarda nova vê T1 e barra antes desse `registrar`. O teste de legado puro cobre zero linhas reais, e o comparador classifica esse caso misto como `a`. Se T2 era legítimo e quantos casos assim existem em produção, **[nao verificado]**. `app/chamados/models.py:3477`, `app/chamados/models.py:3502`, `app/chamados/models.py:3522`, `app/chamados/models.py:4487`, `app/chamados/models.py:4628`, `app/chamados/tests/test_e4c_f5_c4_red.py:147`.

### 4. Fixtures e efeitos colaterais

- **Conversa · média.** O RED conserva a mordida de conteúdo. Ainda exige `ValueError` com PK, ato e motivo `conversa`, e exige pergunta e sequência intactas. M09 ainda simula a aceitação da conversa com dia civil. A Empresa antecipada retira desse teste a cronologia conversa → lote no mesmo `atomic`: a abertura registra o dono, e um `atomic` interno bem-sucedido mantém o sentinela até o fim do externo. Se a Empresa só for tomada depois, a porta de estoque recusa pela ordem antes de conferir a conversa. O RED de P5 cobre essa inversão diretamente. A separação em duas transações é possível pelo código. A frequência ou existência da composição no mesmo `atomic` em produção é **[nao verificado]**. `app/chamados/tests/test_e4c_f5_c4_red.py:396`, `app/chamados/tests/test_e4c_f5_c4_red.py:464`, `app/chamados/tests/test_e4c_f5_c4_red.py:919`, `app/chamados/tests/e4c_f5_c4red2_mutantes.py:177`, `app/chamados/tests/e4c_f5_c4red2_mutantes.py:448`, `app/chamados/models.py:3122`, `app/chamados/models.py:3153`, `app/chamados/services/trava_do_colaborador.py:16`.
- **`pk_misto` · média.** Mudaram duas expectativas da lista. Elas agora reprovam a omissão do `b` indeterminado e continuam exigindo a saída exata. Comparador, vigia e quadro conservam suas asserções de precedência. M13 ainda ataca o quadro e M39 ataca a lista. A troca corrige uma expectativa que reproduzia o defeito e altera uma asserção existente, vedação literal do pedido. `app/chamados/tests/test_e4c_f5_c4_red.py:705`, `app/chamados/tests/test_e4c_f5_c4_red.py:756`, `app/chamados/tests/test_e4c_f5_c4_red.py:782`, `app/chamados/tests/e4c_f5_c4red2_mutantes.py:468`, `app/chamados/tests/e4c_f5_c4red2_mutantes.py:718`.
- **Import privado · baixa.** O import é local ao caminho de estoque, e o módulo da trava declara que não importa `chamados.models`. Há acoplamento novo ao tipo privado, a `.cid` e à representação da fila `run_on_commit`. A leitura da ordem continua baseada no registro existente. Para lote multiempresa, a implementação requer **todas** as Empresas antes do primeiro dono do lote. O RED novo cobre uma Empresa e um dono. `app/chamados/models.py:4867`, `app/chamados/services/trava_do_colaborador.py:1`, `app/chamados/services/trava_do_colaborador.py:26`, `app/folha/export.py:613`, `app/chamados/tests/test_e4c_f5_c4_red.py:919`.
- **Comando fora da lista · baixa.** A mudança no DRY é necessária para P2 e altera a lista impressa. Ela pode ter mais ou menos PKs que o balde homônimo do quadro. Na fixture mista, o quadro espera um e a lista espera dois. O quadro ainda pode classificar conversa pelo dia civil, pois `competencia_do_fato` lê `data_evento`. A exclusão ocorre na lista. Reprojeção, lista de entregues e caráter de somente leitura são preservados pelo fluxo do comando. Consumidores externos do texto **[nao verificado]**. `app/chamados/management/commands/estoque_ato_x_coluna.py:32`, `app/chamados/management/commands/estoque_ato_x_coluna.py:55`, `app/chamados/management/commands/estoque_ato_x_coluna.py:60`, `app/chamados/services/materializacao.py:811`, `app/chamados/tests/test_e4c_f5_c4_red.py:720`, `app/config/crons.py:802`.

### 5. Classes que já reprovaram obra

| Classe | Presença no delta ou no código herdado |
|---|---|
| Segundo juiz registrado | **Ausente no delta.** `veto_do_estoque` extrai a regra de elegibilidade por campo. DRY e porta delegam o juízo de entrega a K6. A guarda de instante compara integridade do par. C21 e K6 continuam nas autoridades registradas. `app/chamados/models.py:4688`, `app/chamados/models.py:4828`, `app/chamados/services/materializacao.py:803`, `app/core/juizes.py:313`, `app/core/juizes.py:350`. |
| Segundo escritor fora da porta | **Ausente para decisões e travas. Presente a emissão dupla herdada do item 6.** Resposta salva a pergunta, o `post_save` acorda a célula e `registrar` também a acorda. Ambos chegam ao mesmo escritor de `EventoCelula`. `app/chamados/services/disputa_emissao.py:1562`, `app/chamados/signals.py:253`, `app/chamados/models.py:3628`, `app/chamados/models.py:4778`, `app/chamados/models.py:4791`, `app/chamados/tests/test_e4c_f5_c4_red.py:634`. |
| Relógio fora da borda | **Presente, herdado.** `carimbar` resolve `quando` e chama `pergunta_viva(self)` sem repassá-lo. O degrau da janela pode cair em `timezone.localdate()`. `app/chamados/models.py:3460`, `app/chamados/models.py:3496`, `app/chamados/juizes.py:642`, `app/chamados/juizes.py:1568`. |
| Derivação paralela | **Corrigida no defeito P1 e no veto P2. Resta duplicação de montagem, baixa.** `leitura` e `_reais_reduzidos` codificam separadamente as tuplas das linhas reais e dos pendentes. Ambas usam `_reduzir`. A presença ainda usa deliberadamente a composta. `app/chamados/models.py:4557`, `app/chamados/models.py:4572`, `app/chamados/models.py:4642`, `app/chamados/models.py:4675`. |
| Selo/RED sem mordida | **Há limites de alcance.** M37 prova a guarda relida, P4 novo prova o persistido, e o Mock de P3 prova encaminhamento. Cada asserção tem alvo próprio. Nenhuma dessas três provas, isoladamente, demonstra rollback da absorção no cenário novo, chegada de pendente real em memória ou idempotência da trilha. `app/chamados/tests/test_e4c_f5_c4_red.py:226`, `app/chamados/tests/test_e4c_f5_c4_red.py:260`, `app/chamados/tests/test_e4c_f5_c4_red.py:336`, `app/chamados/tests/test_e4c_f5_c4_red.py:484`, `app/chamados/tests/test_bug_par_sem_banco.py:173`. |
| Ordem de trava | **Corrigida na admissão P5.** A inversão pelo chamador ainda pode ocorrer antes de chegar à conferência. `app/chamados/models.py:4773`, `app/chamados/models.py:4874`, `app/folha/export.py:613`, `app/chamados/tests/test_e4c_f5_c4_red.py:924`. |
| Pouso antes do pré-requisito | **Ausente na ancestralidade conferida.** O pouso de PORTA-CONFERE-TRAVA `eb76e7be7` é ancestral da base `59600163c`. A 0058 declara dependência da 0057. Ordem efetiva em produção **[nao verificado]**. `app/docs/BACKLOG.md:2179`, `app/chamados/migrations/0058_decisao_entrada_do_ato_historico.py:25`. |
| Campo com dois significados | **Presente, herdado.** `data_evento` é dia do turno no fato e dia civil na conversa. `entrada_em` é instante real de inserção para `ato` e marco convencional para `estoque`. `app/chamados/models.py:3086`, `app/chamados/models.py:3097`, `app/chamados/models.py:4402`. |
| `except` novo | **Ausente.** A contagem de linhas adicionadas que casam com `^\+.*except` no delta é **0**. Os `raise` adicionados na conferência e no uso do veto estão em `app/folha/export.py:619` e `app/chamados/models.py:4891`. |

### 6. P3 e o conflito de desenho

**Média. O conflito existe com a montagem atual do teste antigo.** Ele usa as mesmas duas metades, o mesmo ator e a mesma entrega. Antes de **cada** porta, tira a foto dos PKs de trilha existentes. Depois exige, para cada metade, um evento com PK novo, fotos completas e motivo de entrega. Assim, uma deduplicação global do mesmo veto faria a segunda passagem

[truncado em 20000 de 21755 caracteres]

## 27/11 10:41 · sessao ea730b0547a5 · PAR-PELO-TURNO · leitura · origem=Code

**18 dos 19 blocos são da obra.** O smoke de pauta é independente na trilha de código que produz `SEM-LINHA []`. A causa específica no Chromium e a repetição desse resultado na base ficam [nao verificado]. As três combinações de turno textual contam como três blocos do mesmo método. [pautas/tests/test_smoke_clique_pauta_lida.py:78](/home/ronald/wt-codex-par-pelo-turno-010806/app/pautas/tests/test_smoke_clique_pauta_lida.py:78) · [chamados/tests/test_e3_c8b_sem_banco.py:438](/home/ronald/wt-codex-par-pelo-turno-010806/app/chamados/tests/test_e3_c8b_sem_banco.py:438)

### Catracas e Ruff

Comparei os censos AST de `59600163c` e `4ba908393` em memória. Os sete testes de `test_medida_turno_do_par` passaram com as fontes da base, reproduziram as falhas com as fontes da cópia e passaram com os registros atualizados em memória. A reexecução da suíte integrada na base fica [nao verificado]. [core/medida_turno_do_par.py:336](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/medida_turno_do_par.py:336) · [core/tests/test_medida_turno_do_par.py:504](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:504)

- **Números e identidades.** Pareadores `9 → 9`, derivações de dia `81 → 79`, A1 `6 → 6`, B1 `78 → 76`. Saem de B1 `autoridade_do_periodo`, `somar_periodos` e `_coletar_dados_espelho`. Entra `_turno_noturno_completo`, agora com `data_local(t.saida.timestamp)` no limite do recorte. O teste cobra igualdade dos números e das identidades. A cura é registrar `79`, `B1=76` e as quatro mudanças de identidade no mesmo commit. `core/tests/test_medida_turno_do_par.py:108`, `:109`, `:159`, `:508`, `:548`, `:556` · `ponto/services/espelho.py:84`, `:1035` · `relatorios/pdf_espelho.py:291` · `escala/servico_jornada.py:250`.
- **Mutantes e C8c.** O candidato amplo e a chamada morta medem `79` contra `81` e param em `DESCEU`. O mutante de dia acrescenta `1`, chega a `80` e também recebe `DESCEU`, quando o teste espera `SUBIU`. Com o registro `79`, o mutante volta a testar a subida. C8c chama a mesma `_catraca` e para em `79` contra `81`. A asserção seguinte, sobre a ausência de derivação no construtor de `CargaDaVida`, é satisfeita pelos dois censos. `core/tests/test_medida_turno_do_par.py:513`, `:580`, `:718`, `:845`, `:850` · `chamados/tests/test_e3_c8c_sem_banco.py:155`, `:160`, `:161`.
- **Composição.** A lista congelada tem `47` funções e a cópia mede `55`. Sai `_declara_separado_pelo_teto`. Entram `_chave_batida`, `_c9_do_par`, `_marca_intra_duracao.tipo_de_borda`, `_carregar_turnos_do_colab`, `marco_no_dia`, `marco_de_saida`, `minutos_ate_o_marco`, `localizar` e `MotorBase._cadastro_nao_descreve`. O censo segue as chamadas alcançadas a partir do juiz, inclusive as novas dependências da C9. A cura é registrar a composição no mesmo commit. `core/tests/test_medida_turno_do_par.py:110`, `:517` · `ponto/turnos.py:166`, `:171`, `:399`, `:1887` · `ponto/janela_he.py:20`, `:41`, `:64` · `ponto/motor_calculo_v2.py:73`, `:2046`.
- **Publicação `CLASSIFICAR`.** A2−A1 fica em `9`. B2−B1 sobe de `113` para `114` com `_coletar_dados_espelho`, que perde a ocorrência B1 no carregamento dos feriados e conserva o `.date()` do teto em ontem. A cura é registrar essa entrada no mesmo commit. `core/tests/test_medida_turno_do_par.py:351`, `:569` · `relatorios/pdf_espelho.py:278`, `:291`.
- **Ruff.** O teste exige código de saída `0`. A pista devolveu `1` e apontou `B023` em `ponto/tests/test_par_pelo_turno.py:212:63`. O hunk adiciona esse arquivo, incluindo `lambda: dna` que captura a variável reatribuída no laço. A cura é de código no teste, com `lambda dna=dna: dna`. Esse B023 está ausente da base porque o arquivo é novo. O resultado completo do Ruff na base fica [nao verificado]. [core/tests/test_contract_esmeril.py:22](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_contract_esmeril.py:22) · [ponto/tests/test_par_pelo_turno.py:212](/home/ronald/wt-codex-par-pelo-turno-010806/app/ponto/tests/test_par_pelo_turno.py:212)

O delta completo de `LINHAS_REGISTRADAS` abaixo mostra **esperado → medido**. São linhas da ocorrência que o censo conta, inclusive quando a mudança só desloca o código. A cura é registrar esses vetores no mesmo commit. [core/tests/test_medida_turno_do_par.py:251](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:251) · [core/tests/test_medida_turno_do_par.py:563](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:563) [nao verificado]

| Ocorrência | Esperado → medido |
|---|---|
| `ponto/turnos.py::_pares_marcados` | `[812] → [829]` |
| `ponto/turnos.py::_dia_das_batidas_puro` | `[3044] → [3087]` |
| `ponto/turnos.py::dia_do_turno_da_batida` | `[3021] → [3058]` |
| `ponto/turnos.py::par_completo` | `[2237] → [2267]` |
| `ponto/services/fechamento.py::_so_a_data` | `[1593] → [1588]` |
| `ponto/services/fechamento.py::apurar_por_fatia` | `[224] → [246]` |
| `ponto/services/fechamento.py::recalcular_fechamento_mes` | `[675, 677, 683, 703, 774, 813, 859, 871] → [673, 675, 685, 705, 776, 815, 854, 866]` |
| `ponto/services/espelho.py::autoridade_do_periodo` | `[1065] → ∅` no hunk `:1035` |
| `ponto/services/espelho.py::somar_periodos` | `[84] → ∅` no hunk `:84` |
| `relatorios/pdf_espelho.py::_coletar_dados_espelho` | `[298] → ∅` no hunk `:291` |
| `escala/servico_jornada.py::_turno_noturno_completo` | `∅ → [250]` |

### Comportamento e smoke

- **C8b / CargaBasicaLeitura.** O traceback termina em `AttributeError: 'types.SimpleNamespace' object has no attribute '_turnos_com_contexto'`. O teste extrai o método real e espera que duas chamadas leiam as batidas uma vez e usem o cache sem parear. Na base, `turnos` fazia a carga e devolvia a tupla do dublê. Na cópia, o hunk extrai `_turnos_com_contexto` e `turnos` filtra a resposta por `t.data_turno`. O replay passou na base, falhou na cópia e passou com o helper real ligado ao dublê e turnos datados. `chamados/tests/test_e3_c8b_sem_banco.py:537`, `:544`, `:546` · `ponto/turnos.py:3112` (base), `:3155`, `:3175` (cópia).
- **C8b / AlimentacaoC6, ausência e memo.** Ambos dão `False is not true` depois de remover a ausência da carga. O contrato é reabrir a pergunta nesse braço, preservando o memo avulso. O teste lê `janela_com_pad` por AST e injeta só `datetime`. O hunk passa a ler `_PAD_FECHO`. A carga estoura no dublê, e `classificar_falta` registra `nucleo_indisponivel_na_lei` e volta sem acusação. O replay passou na base e passou na cópia ao alimentar a constante real no ambiente AST. `chamados/tests/test_e3_c8b_sem_banco.py:312`, `:325`, `:326` · `chamados/tests/e3_c8b_apoio.py:139` · `chamados/services/carga_pergunta_viva.py:174` · `escala/servico_jornada.py:396`, `:400` · `ponto/turnos.py:54`, `:1869`.
- **C8b / AlimentacaoC6, três braços textuais.** O contrato espera `False` com turno fechado ou trilha de retratação e `True` com turno aberto sem trilha, sempre com `env.erros == []`. O mesmo hunk de `_PAD_FECHO` quebra a consulta de turno. O juiz registra `juiz_indisponivel` e devolve `False`. Nos dois primeiros braços o traceback mostra `NameError("name '_PAD_FECHO' is not defined")` em `env.erros`. No terceiro mostra `False != True`. Os três passaram no replay da base e com a alimentação da constante na cópia. `chamados/tests/test_e3_c8b_sem_banco.py:438`, `:447`, `:449` · `chamados/juizes.py:448`, `:865`, `:1586`, `:1593` · `chamados/services/carga_pergunta_viva.py:174` · `ponto/turnos.py:1869`.
- **Espelho do app.** O traceback dá `False is not true` na premissa «a fixture nao reproduz o desacordo», antes de testar o card. Reproduzi em memória o resultado do motor, o mapa por PK e o helper do teste com as três batidas. O motor conserva o período aberto de `06:57` em `31/08` e o fechado de `12:50–21:20` em `01/09` nas duas versões. Na base, o mapa consultado para setembro fica sem o turno de `31/08` e usa a data civil de `06:57`, jogando o aberto em `01/09` no helper. Na cópia, o julgamento com contexto recupera `31/08` para esse PK, e o helper retorna `False` em `01/09`. O contrato a preservar é o card ler a geometria. A cura é construir o desacordo do teste de forma explícita, com um resultado de motor divergente e uma geometria fechada para o mesmo dia. A execução integrada do endpoint na base fica [nao verificado]. `api/tests/test_espelho_app_fonte_e_geometria.py:295`, `:301`, `:305`, `:318`, `:325` · `ponto/turnos.py:2996`, `:3044` (base), `:2000`, `:3035`, `:3082`, `:3087` (cópia) · `api/views.py:1447`, `:1512`.
- **Espelho da tela.** O traceback diz `False is not true: somar_periodos deixou de recortar por data`. O teste inspeciona o AST e espera uma chamada `.date()`. A base tem essa chamada na linha `84`. A cópia tem zero e conserva o recorte, comparando `dentro` com o dia publicado em `dia_da_jornada[id(p)]`. A cura é atualizar a asserção estrutural para reconhecer o recorte pelo dia publicado e preservar o teste que proíbe derivação em `montar_dias`. `ponto/tests/test_espelho_tela_le_o_juiz_do_dia.py:193`, `:245`, `:248` · `ponto/services/espelho.py:84`, `:451`.
- **Ponta pequena.** O traceback dá `0 != 1` com `{}` às `03:10`. A fixture grava as três batidas sob o mesmo `freeze_time` de `20:05`. Isso faz a entrada de `20:04` parecer um eco de flush da entrada de `14:57`. No replay do pareador, a base ainda usa a entrada das `20:04` para inferir a pausa após a saída das `19:01` e deixa o turno aberto. A cópia exclui o eco da inferência e fecha `14:57–19:01`. Na consulta retroativa das `02:10`, o aberto da base ainda manteria a célula de ontem no vigia e expiraria às `03:00`. Com o par fechado, a cópia já pode descartar a célula antes de contar ou emitir. Gravar cada batida da fixture no próprio instante conservou o aberto nos dois pareadores. A ata efetivamente gravada e a execução integrada com ORM na base ficam [nao verificado]. `ponto/tests/test_ponta_pequena_prazo_evento.py:84`, `:388`, `:392`, `:400` · `ponto/models.py:79` · `ponto/turnos.py:369`, `:399`, `:404`, `:469`, `:1234`, `:1420`, `:2073` · `ponto/services/vigia_de_hora.py:59`, `:66`, `:169` · `ponto/services/cartorio.py:881`.
- **Smoke de pauta.** O traceback para em `'SEM-LINHA []' is not an instance of <class 'dict'>`. A sonda chegou à raiz e à subaba e encontrou zero rótulos aos `700 ms`, antes de clicar no texto e verificar ausência de POST. O caminho usa a casca do holerite, os GETs de pauta e o `fetch` injetado no Chromium. A âncora `2026-09` é de competência e a listagem filtra destinatário e estado, sem recorte pela data da competência. A trilha da linha de pauta e do clique está fora dos hunks. O JSON efetivo da supervisão, o motivo da ausência dos rótulos e a recorrência na base ficam [nao verificado]. `pautas/tests/test_smoke_clique_pauta_lida.py:54`, `:78`, `:90`, `:94`, `:144`, `:153`, `:169`, `:174`, `:211` · `pautas/views.py:29`, `:61` · `pautas/services.py:168`, `:304`, `:322` · `templates/core/_copiloto.html:359`, `:400`, `:475`.

### Tabela final

| Teste | Da obra? | Classe | Sítio | Cura proposta | [nao verificado]
|---|---|---|---|---|
| `test_carregar_batidas_uma_vez_sem_executar_pareador` · `chamados/tests/test_e3_c8b_sem_banco.py:544` | sim | comportamento | `ponto/turnos.py:3155`, `:3175` | Ligar o helper real ao dublê com janela e turnos datados, preservando uma leitura e zero pareamentos. |
| `test_MORDE_o_card_diz_o_que_a_GEOMETRIA_diz_e_nao_o_que_o_motor_diz` · `api/tests/test_espelho_app_fonte_e_geometria.py:318` | sim | comportamento | `ponto/turnos.py:2000`, `:3035`, `:3082` | Tornar explícito o desacordo entre o motor e a geometria no mesmo dia da fixture. |
| `test_MORDE_clicar_o_texto_e_nao_o_rotulo_nao_posta` · `pautas/tests/test_smoke_clique_pauta_lida.py:210` | não pela trilha estática. Repetição na base [nao verificado] | relógio/ambiente | `pautas/tests/test_smoke_clique_pauta_lida.py:90`, `:94`, `:174` | Afirmar A no JSON de supervisão e sincronizar a sonda com a renderização da subaba. |
| `test_RED_a_celula_de_ONTEM_fica_no_vigia_ate_o_aberto_expirar_e_o_evento_sai` · `ponto/tests/test_ponta_pequena_prazo_evento.py:400` | sim | comportamento | `ponto/turnos.py:399`, `:404`, `:469`, `:1234` | Gravar cada batida no próprio instante e manter os horários das passadas do vigia. |
| `test_ruff_zero` · `core/tests/test_contract_esmeril.py:24` | sim | ruff | `ponto/tests/test_par_pelo_turno.py:212` | Vincular `dna` na definição com `lambda dna=dna: dna`. |
| `test_CATRACA_composicao_congelada_exige_registro_no_mesmo_commit` · `core/tests/test_medida_turno_do_par.py:517` | sim | catraca-registro | `ponto/turnos.py:166`, `:171`, `:399`, `:1887` | Registrar a composição de `55` funções no mesmo commit. |
| `test_CATRACA_linhas_reais_congeladas_exigem_registro_no_mesmo_commit` · `core/tests/test_medida_turno_do_par.py:563` | sim | catraca-registro | `ponto/turnos.py:829` e demais ocorrências da tabela de linhas | Atualizar `LINHAS_REGISTRADAS` no mesmo commit. |
| `test_CATRACA_numeros_exatos_e_registro_no_mesmo_commit` · `core/tests/test_medida_turno_do_par.py:547` | sim | catraca-registro | `ponto/services/espelho.py:84`, `:1035` · `relatorios/pdf_espelho.py:291` · `escala/servico_jornada.py:250` | Registrar `79`, B1 `76` e as identidades revistas no mesmo commit. |
| `test_MORDE_candidato_amplo_novo_e_publicado_sem_travar_a_catraca` · `core/tests/test_medida_turno_do_par.py:718` | sim | catraca-registro | `core/tests/test_medida_turno_do_par.py:108`, `:513` | Registrar `79` no mesmo commit para o mutante voltar a provar a publicação. |
| `test_MORDE_chamada_morta_no_juiz_exige_registro_da_composicao` · `core/tests/test_medida_turno_do_par.py:580` | sim | catraca-registro | `core/tests/test_medida_turno_do_par.py:108`, `:513`, `:583` | Atualizar números e composição no mesmo commit para o mutante chegar à asserção de composição. |
| `test_MORDE_dia_novo_sobe_contador_2_e_catraca_fica_RED` · `core/tests/test_medida_turno_do_par.py:850` | sim | catraca-registro | `core/tests/test_medida_turno_do_par.py:108`, `:847`, `:850` | Registrar `79` no mesmo commit para `80` produzir `SUBIU`. |
| `test_a_classificar_publicado_sem_entrar_como_criterio_amplo` · `core/tests/test_medida_turno_do_par.py:569` | sim | catraca-registro | `relatorios/pdf_espelho.py:278`, `:291` | Incluir `_coletar_dados_espelho` em `B2_menos_B1` no mesmo commit. |
| `test_MORDE_o_selo_distingue_JANELA_de_DIA` · `ponto/tests/test_espelho_tela_le_o_juiz_do_dia.py:248` | sim | comportamento | `ponto/services/espelho.py:84`, `:451` | Atualizar o selo para verificar recorte pelo dia publicado. |
| `test_ausencia_alimentada_nao_pode_sumir_ou_ser_de_outro_colab` · `chamados/tests/test_e3_c8b_sem_banco.py:312` | sim | comportamento | `ponto/turnos.py:1869` · `chamados/tests/e3_c8b_apoio.py:139` | Alimentar `_PAD_FECHO` real no dublê AST. |
| `test_memo_avulso_nao_contamina_nem_e_contaminado_pela_carga` · `chamados/tests/test_e3_c8b_sem_banco.py:325` | sim | comportamento | `ponto/turnos.py:1869` · `chamados/tests/e3_c8b_apoio.py:139` | Alimentar `_PAD_FECHO` real e preservar a asserção sobre o memo avulso. |
| `test_turno_textual_fechado_e_trilha_com_e_sem_carga` · `fechado=True, auditada=False` · `chamados/tests/test_e3_c8b_sem_banco.py:449` | sim | comportamento | `ponto/turnos.py:1869` · `chamados/juizes.py:1593` | Alimentar `_PAD_FECHO` real para o turno fechado resolver sem erro. |
| `test_turno_textual_fechado_e_trilha_com_e_sem_carga` · `fechado=False, auditada=True` · `chamados/tests/test_e3_c8b_sem_banco.py:449` | sim | comportamento | `ponto/turnos.py:1869` · `chamados/juizes.py:865` | Alimentar `_PAD_FECHO` real para o juiz chegar à leitura da trilha. |
| `test_turno_textual_fechado_e_trilha_com_e_sem_carga` · `fechado=False, auditada=False` · `chamados/tests/test_e3_c8b_sem_banco.py:447` | sim | comportamento | `ponto/turnos.py:1869` · `chamados/juizes.py:1593` | Alimentar `_PAD_FECHO` real para a pergunta aberta sem trilha voltar a `True`. |
| `test_selo_turno_sem_derivacao_nova` · `chamados/tests/test_e3_c8c_sem_banco.py:160` | sim | catraca-registro | `core/tests/test_medida_turno_do_par.py:108`, `:513` | Atualizar o registro compartilhado para `79` no mesmo commit. |

## 27/11 10:38 · sessao 93cc64114c4e · - · - · origem=Ronald (tmux)

Os seis blocos de C8b são da obra. A reprodução em memória repetiu os cinco `FAIL` e um `ERROR` na cópia. Sob o mesmo dublê da estampa de trava, trocar os trechos extraídos pelos da base `59600163c` fez os quatro métodos passarem. `chamados/tests/e4_apoio.py:8`, `chamados/tests/e4_catracas.py:8`, `chamados/juizes.py:1168`

- **Ausência** (`chamados/tests/test_e3_c8b_sem_banco.py:312`) e **memo** (`chamados/tests/test_e3_c8b_sem_banco.py:325`) esperam que a pergunta fique viva ao remover a ausência da carga. Recebem `False is not true`. A lei chama `carga.turnos`, que avalia o `janela_com_pad` real extraído sem sua nova constante. Ela registra `nucleo_indisponivel_na_lei` e deixa de acusar a falta. `escala/servico_jornada.py:393`, `escala/servico_jornada.py:400`, `chamados/services/carga_pergunta_viva.py:174`, `ponto/turnos.py:1869`
- **Texto, fechado sem trilha** e **texto, aberto com trilha** chegam ao `False` esperado, mas registram `juiz_indisponivel`, com `NameError("name '_PAD_FECHO' is not defined")`, violando `env.erros == []`. **Texto, aberto sem trilha** recebe `False != True` pelo mesmo retorno conservador. O caminho da exceção passa pelo leitor da completude textual e pela carga até a mesma constante. `chamados/tests/test_e3_c8b_sem_banco.py:447`, `chamados/tests/test_e3_c8b_sem_banco.py:449`, `chamados/juizes.py:448`, `chamados/juizes.py:865`, `chamados/juizes.py:1587`, `chamados/juizes.py:1593`
- **Cura dos cinco blocos de AlimentacaoC6.** Levar a constante real ao ambiente que compila `janela_com_pad` por AST. O dublê injeta apenas `datetime` e o código novo lê `_PAD_FECHO`. A injeção da constante derivada da fonte fez os cinco blocos passarem em memória. `chamados/tests/e3_c8b_apoio.py:139`, `ponto/turnos.py:54`, `ponto/turnos.py:56`, `ponto/turnos.py:1869`
- **CargaBasicaLeitura.** O dublê tinha uma tupla com marcador textual e extraía `turnos` isoladamente. Na base, o método carregava as batidas e devolvia o cache. A cópia passou a chamar `_turnos_com_contexto` e a filtrar por `data_turno`, gerando `AttributeError` no dublê. Ligar o helper real e fornecer `ini`, `fim` e turnos datados preservou no replay a asserção de uma única leitura e zero pareamento. `chamados/tests/test_e3_c8b_sem_banco.py:537`, `chamados/tests/test_e3_c8b_sem_banco.py:544`, `chamados/tests/test_e3_c8b_sem_banco.py:546`, `ponto/turnos.py:3155`, `ponto/turnos.py:3175`

## 27/11 10:38 · sessao 19f2fc3b4622 · - · - · origem=Ronald (tmux)

Os sete testes de `CatracaTurnoDoParTest` passaram no replay em memória da base `59600163c` e reproduziram as falhas da pista na cópia. O selo C8c mede os mesmos números e mantém ausente o construtor de `CargaDaVida` nos dois censos. [app/core/tests/test_medida_turno_do_par.py:504](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:504) [app/chamados/tests/test_e3_c8c_sem_banco.py:151](/home/ronald/wt-codex-par-pelo-turno-010806/app/chamados/tests/test_e3_c8c_sem_banco.py:151)

- **Números**. Pareadores `9→9`, derivações `81→79`, A1 `6→6`, B1 `78→76`. B1 perde duas funções do espelho e a coleta do PDF, e ganha `_turno_noturno_completo`. Registrar os números e as identidades no mesmo commit. O mutante amplo e a chamada morta medem `79`, e o mutante de dia mede `80`, abaixo do `81` congelado. [app/core/tests/test_medida_turno_do_par.py:108](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:108) [app/core/tests/test_medida_turno_do_par.py:546](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:546) [app/core/tests/test_medida_turno_do_par.py:577](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:577) [app/core/tests/test_medida_turno_do_par.py:714](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:714) [app/core/tests/test_medida_turno_do_par.py:845](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:845) [nao verificado]
- **Composição**. `47→55`, com nove inclusões alcançadas pelo fechamento estático e remoção de `_declara_separado_pelo_teto`. Registrar a composição no mesmo commit. [app/core/tests/test_medida_turno_do_par.py:110](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:110) [app/core/tests/test_medida_turno_do_par.py:517](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:517) [app/ponto/turnos.py:171](/home/ronald/wt-codex-par-pelo-turno-010806/app/ponto/turnos.py:171) [nao verificado]
- **Linhas**. O primeiro delta é `_pares_marcados`, `[812]→[829]`. A lista completa foi entregue ao agente principal. Registrar ocorrências no mesmo commit. [app/core/tests/test_medida_turno_do_par.py:251](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:251) [app/core/tests/test_medida_turno_do_par.py:563](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:563) [app/ponto/turnos.py:829](/home/ronald/wt-codex-par-pelo-turno-010806/app/ponto/turnos.py:829) [nao verificado]
- **CLASSIFICAR**. A2−A1 `9→9`, B2−B1 `113→114`. Só entra `_coletar_dados_espelho`, que perde a ocorrência B1 e conserva a data ampla. Registrar `CLASSIFICAR` no mesmo commit. [app/core/tests/test_medida_turno_do_par.py:351](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:351) [app/core/tests/test_medida_turno_do_par.py:569](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_medida_turno_do_par.py:569) [app/relatorios/pdf_espelho.py:278](/home/ronald/wt-codex-par-pelo-turno-010806/app/relatorios/pdf_espelho.py:278) [nao verificado]
- **Ruff**. A pista obteve retorno `1` frente ao `0` exigido. O único B023 apontado vem de `lambda: dna` no arquivo introduzido pela obra. Vincular `dna` na definição com `lambda dna=dna: dna`. Execução do Ruff na base [nao verificado]. [app/core/tests/test_contract_esmeril.py:24](/home/ronald/wt-codex-par-pelo-turno-010806/app/core/tests/test_contract_esmeril.py:24) [app/ponto/tests/test_par_pelo_turno.py:212](/home/ronald/wt-codex-par-pelo-turno-010806/app/ponto/tests/test_par_pelo_turno.py:212)

## 27/11 10:37 · sessao 64cf12af4b89 · - · - · origem=Ronald (tmux)

- **Ponta pequena — sim, comportamento.** A fixture grava as três batidas às 20:05, por isso a entrada de 20:04 vira eco de flush. A base ainda usava essa entrada para inferir uma pausa das 19:01 às 20:04, deixando o turno aberto. A cópia exclui o eco dessa inferência e fecha o par às 19:01. Confirmei os dois resultados em memória. No vigia das 03:10, a consulta retroativa às 02:10 mantém a célula na base e a elimina na cópia, compatível com `0 != 1 ... {}`. A cura proposta é gravar cada batida da fixture no próprio instante e preservar o relógio das passadas. O teste completo com ORM segue [nao verificado]. `app/ponto/tests/test_ponta_pequena_prazo_evento.py:82,388,392,400` · `app/ponto/models.py:79` · `app/ponto/turnos.py:360,369,399,404,469,1234,1420,2053,2073` · `app/ponto/services/vigia_de_hora.py:59,66,169`.

- **Smoke pauta — independente pelo caminho lido, classe relógio/ambiente.** `SEM-LINHA []` indica que a sonda achou raiz e subaba e viu zero rótulos no prazo fixo de 700 ms. A falha ocorreu antes do clique e das asserções de POST e de leitura. O teste usa a casca do holerite e injeta no Chromium o JSON dos GETs. A lista de pautas vivas filtra por destinatário e estado. A data da âncora é um identificador nessa leitura. A cura proposta é afirmar a presença de A no JSON e aguardar a renderização da subaba antes do clique. O motivo preciso dos zero rótulos e a repetição da falha na base seguem [nao verificado]. `app/pautas/tests/test_smoke_clique_pauta_lida.py:54,78,90,144,153,169,174,210` · `app/pautas/services.py:304,322` · `app/pautas/models.py:57` · `app/templates/core/_copiloto.html:344,359,400,475`.

## 27/11 10:24 · sessao 1bca4d244636 · CASO-LITERAL-14 · leitura · origem=Code

```python
import json
import logging
import re
from datetime import datetime, timedelta
from difflib import unified_diff
from inspect import getclosurevars

from django.contrib.auth import get_user_model
from django.db import connection, transaction
from django.db.models import Q
from django.test import TestCase
from django.utils import timezone

from chamados.catalogo.motor import COBRANCA
from chamados.models import ChamadoColaborador, PerguntaDisputa
from colaboradores.mixins import tem_acao
from colaboradores.models import Empresa
from folha import export
from ponto.models import Batida, FechamentoMensal
from ponto.services import fechamento, retratar_humano
from ponto.services.par_relampago_na_lista import travar_batidas_do_sinal

# Executar no shell de `tenant_command shell --schema=juliani`.
# Escopo autorizado em aval.txt:1. Alvo em r_f1b.md:22 e r_f1b.md:41.
# HEAD Git, versão instalada, dados atuais de produção e execução [nao verificado].
MODO = "ensaio"
USUARIO_ID = None
MOTIVO_ID = None

ALVO = 159844
ANTERIOR = 159843
COLAB_ID = 1036
EMPRESA_ID = 2
COMPETENCIA = (11, 2026)
HORA_LOCAL = datetime(2026, 11, 19, 18, 4, 22)
OBSERVACAO = (
    "CASO-LITERAL-14-v2-2711-0955: toque excedente 1 s apos 159843 "
    "(classe b, fase 1)"
)

# Campos consumidos na montagem e classificação do TXT.
# app/folha/export.py:402, :699, :774, :977 e :1310
CAMPOS_TXT = (
    "colaborador_id", "mes", "ano", "status", "horas_trabalhadas",
    "turnos_abertos", "inconsistencias", "horas_extras_50",
    "horas_extras_100", "horas_extras_100_feriado", "horas_folga_trabalhada",
    "horas_noturnas", "horas_intra_indenizada", "horas_atraso",
    "horas_saida_antecipada",
)


def exigir(condicao, mensagem):
    if not condicao:
        raise ValueError(mensagem)


def imprimir_json(rotulo, dado):
    print(rotulo, json.dumps(dado, default=str, ensure_ascii=False, sort_keys=True))


def rotulo_publicavel(texto):
    # Os rótulos solicitados vêm de cadastros com texto livre.
    # app/ponto/models.py:641. Conteúdo desses cadastros [nao verificado].
    if re.search(r"@|\b(?:\d{11}|\d{3}\.\d{3}\.\d{3}-\d{2})\b", texto):
        return "[oculto]"
    return texto


def referencias_no_contexto(campo):
    # IDs em contexto, incluindo a forma textual do acervo.
    # app/chamados/models.py:1919, :3914
    # app/chamados/juizes.py:131
    # app/chamados/services/disputa_emissao.py:59
    # app/chamados/services/acoes_chamado.py:244
    ids = (ALVO, str(ALVO))
    return (
        Q(**{f"{campo}__batida_id__in": ids})
        | Q(**{f"{campo}__ultima_batida__id__in": ids})
        | Q(**{f"{campo}__entrada_sem_saida__entrada_id__in": ids})
        | Q(**{f"{campo}__ocorrencias__contains": [{"batida_id": ALVO}]})
        | Q(**{f"{campo}__ocorrencias__contains": [{"batida_id": str(ALVO)}]})
    )


def guardas(travar):
    # A ordem Empresa -> Colaborador -> Batidas vem da porta existente.
    # app/ponto/services/par_relampago_na_lista.py:172
    # app/folha/export.py:556 e app/folha/views.py:134
    if travar:
        exigir(Empresa.objects.filter(pk=EMPRESA_ID).exists(),
               "Empresa 2 ausente. app/colaboradores/models.py:40")
        marcas = travar_batidas_do_sinal(COLAB_ID, (ALVO, ANTERIOR), {EMPRESA_ID})
    else:
        marcas = Batida.objects.filter(pk__in=(ALVO, ANTERIOR))

    por_id = {marca.pk: marca for marca in marcas}
    batida = por_id.get(ALVO)
    exigir(batida is not None, "Batida 159844 ausente do escopo. app/ponto/models.py:8")
    exigir(batida.colaborador_id == COLAB_ID,
           "Colaborador divergente. app/ponto/models.py:22")
    exigir(batida.colaborador.empresa_id == EMPRESA_ID,
           "Empresa divergente. app/colaboradores/models.py:321")
    exigir(timezone.localtime(batida.timestamp).replace(tzinfo=None) == HORA_LOCAL,
           "Timestamp local divergente. app/ponto/models.py:23 e app/config/settings/base.py:93")
    exigir(batida.tipo == "S", "Tipo divergente. app/ponto/models.py:24")
    exigir(batida.retratada_em is None, "Batida já retratada. app/ponto/models.py:67")
    exigir(batida.aprovada_por_id is None, "Batida aprovada por admin. app/ponto/models.py:68")

    anterior = por_id.get(ANTERIOR)
    exigir(
        anterior is not None
        and anterior.colaborador_id == COLAB_ID
        and anterior.tipo == "E"
        and anterior.timestamp == batida.timestamp - timedelta(seconds=1)
        and anterior.retratada_em is None,
        "Batida anterior divergente. r_f1b.md:22 e app/ponto/models.py:22–24,67",
    )

    # .viva delega ao juiz da pergunta. A abertura da disputa usa fechada_em.
    # app/chamados/models.py:1997, :2751, :3792 e :3812
    # app/chamados/juizes.py:911 e app/ponto/models.py:40
    perguntas = PerguntaDisputa.objects.filter(disputa__fechada_em__isnull=True).filter(
        Q(batida_origem_id=ALVO)
        | Q(batidas_plantadas__pk=ALVO)
        | referencias_no_contexto("disputa__contexto")
        | referencias_no_contexto("disputa__chamado__contexto_json")
        | referencias_no_contexto("chamado_destino__contexto_json")
    ).distinct()
    exigir(not any(pergunta.viva for pergunta in perguntas),
           "Pergunta aberta ligada à batida. app/chamados/models.py:3792")

    # COBRANCA contém os estados aberto e em_analise.
    # app/chamados/catalogo/motor.py:29 e app/chamados/models.py:59, :1923, :3812
    chamados = ChamadoColaborador.objects.filter(status_local__in=COBRANCA).filter(
        referencias_no_contexto("contexto_json")
        | referencias_no_contexto("disputa_origem__contexto")
        | Q(disputa_origem__perguntas__batida_origem_id=ALVO)
        | Q(perguntas_que_resolvem__batida_origem_id=ALVO)
        | Q(disputa_origem__perguntas__batidas_plantadas__pk=ALVO)
        | Q(perguntas_que_resolvem__batidas_plantadas__pk=ALVO)
    )
    exigir(not chamados.exists(), "Chamado aberto ligado à batida. app/chamados/catalogo/motor.py:29")

    # competencia_do_fato usa a janela da empresa. O juiz de “exportada”
    # é competencia_entregue, também chamado por marcos_da_competencia.
    # app/ponto/services/fechamento.py:1626, :1561 e :36
    # app/ponto/services/dia_pago.py:702 e app/folha/export.py:565
    comp = fechamento.competencia_do_fato(batida.colaborador, batida.timestamp)
    exigir(comp == COMPETENCIA, "Competência divergente. app/ponto/services/fechamento.py:1626")
    exigir(not export.competencia_entregue(EMPRESA_ID, *comp),
           "Competência exportada. app/folha/export.py:565")
    exigir(comp not in fechamento.GUARDA_COMPETENCIA,
           "Competência consta na guarda. app/ponto/services/fechamento.py:36")
    return batida


def listar():
    # As duas consultas usam os critérios da porta.
    # app/ponto/services/retratar_humano.py:108, :118, :127 e :162
    # app/colaboradores/mixins.py:322 e app/config/settings/saas.py:34
    motivos = {motivo.pk: motivo for motivo in retratar_humano.motivos_ativos()}
    usuarios = {
        usuario.pk: usuario
        for usuario in get_user_model().objects.only(
            "id", "username", "is_active", "is_superuser"
        ).order_by("pk")
        if tem_acao(usuario, retratar_humano.ACAO_EXIGIDA)
    }
    imprimir_json("MOTIVOS_ATIVOS", [(pk, rotulo_publicavel(m.nome)) for pk, m in motivos.items()])
    imprimir_json("USUARIOS_REGULARIZAR_BATIDA",
                  [(pk, rotulo_publicavel(u.username)) for pk, u in usuarios.items()])
    return motivos, usuarios


def fotografia(rotulo, batida, apos_retratacao=None):
    # values() fotografa as colunas, com IDs nas FKs. Preserva as chaves dos
    # campos de texto livre e do caminho da foto, ocultando valores preenchidos.
    # app/ponto/models.py:22–95
    dados_batida = Batida.objects.filter(pk=batida.pk).values().first()
    exigir(dados_batida is not None, "Batida ausente na fotografia. app/ponto/models.py:8")
    for campo in ("terminal", "tipo_registro", "justificativa", "foto"):
        if dados_batida[campo]:
            dados_batida[campo] = "[oculto]"

    colab = batida.colaborador
    empresa = colab.empresa
    mes, ano = COMPETENCIA
    gravado = FechamentoMensal.objects.filter(
        colaborador_id=colab.pk, mes=mes, ano=ano,
    ).values("id", "derivado_em", *CAMPOS_TXT).first()
    if apos_retratacao is not None:
        exigir(dados_batida["retratada_em"] == apos_retratacao and not dados_batida["aprovada"],
               "Estado após a retratação divergente. app/ponto/registro_batida.py:294")
        # O callback contém falhas. O carimbo confirma a lavra do fechamento.
        # app/ponto/services/fechamento.py:1167, :1213 e :1842
        exigir(gravado and gravado["derivado_em"] and gravado["derivado_em"] >= apos_retratacao,
               "Lavra do fechamento sem confirmação. app/ponto/services/fechamento.py:1167,1213")

    # A view entrega o fechamento lido ao classificador e ao montador. O
    # recorte na entrada mantém os registros 11 junto às rubricas do colab.
    # app/folha/views.py:214, :219 e :275
    # app/ponto/services/fechamento.py:468 e app/folha/export.py:215, :699
    lido = fechamento.recalcular_fechamento_mes(
        mes, ano, empresa_id=empresa.pk, colaborador_ids=[colab.pk],
        somente_leitura=True, retornar_objeto=True,
    ).get(colab.pk)
    exigir(lido is not None, "Fechamento lido indisponível. app/ponto/services/fechamento.py:1333")
    itens = export.classificar_export(empresa, mes, ano, fechamentos=[lido])
    linhas, _ = export.montar_linhas_export(
        empresa, mes, ano, itens=itens, fechamentos=[lido],
    )
    # Validação e conteúdo da view. Hash sobre os bytes UTF-8 com CRLF do TXT.
    # app/folha/views.py:282 e app/folha/export.py:266, :525 e :542
    exigir(not export.validar_layout_dominio(linhas), "Layout inválido. app/folha/export.py:266")
    sha256 = export.hash_do_txt(export.conteudo_do_txt(linhas))

    # As rubricas e seus insumos vêm dos mesmos leitores da montagem.
    # app/folha/export.py:184, :341, :363, :402, :739 e :993
    ciclo = export._ciclo_do_colab(colab, mes, ano)
    dobra = export.feriado_12x36_em_dobra(colab, mes, ano)
    eventos = dict(export.eventos_do_fechamento(lido, tipo_ciclo=ciclo, feriado_12x36_em_dobra=dobra))
    rubricas = export._rubricas_map(empresa)
    insumos_rubricas = {}
    for evento in (*eventos, "falta_diurna"):
        rubrica = rubricas.get(evento)
        insumos_rubricas[evento] = (
            None if rubrica is None else {
                "id": rubrica.pk,
                "empresa_id": rubrica.empresa_id,
                "codigo_fopag": rotulo_publicavel(rubrica.codigo_fopag),
                "tipo": rotulo_publicavel(rubrica.tipo),
            }
        )

    imprimir_json(f"{rotulo} BATIDA", dados_batida)
    imprimir_json(f"{rotulo} FECHAMENTO_GRAVADO", gravado)
    imprimir_json(f"{rotulo} FECHAMENTO_LIDO", {campo: getattr(lido, campo) for campo in CAMPOS_TXT})
    imprimir_json(f"{rotulo} CLASSIFICACAO", [
        {campo: item[campo] for campo in ("colaborador_id", "status", "motivo", "travas_aprovacao")}
        for item in itens
    ])
    imprimir_json(f"{rotulo} RUBRICAS", {
        "tipo_ciclo": ciclo,
        "feriado_12x36_em_dobra": dobra,
        "eventos": eventos,
        "datas_falta": export.datas_falta(colab, empresa, mes, ano),
        "mapeamento": insumos_rubricas,
    })
    print(f"{rotulo} TXT linhas={len(linhas)} sha256={sha256}")
    for linha in linhas:
        print(linha)
    return linhas


def retratar_e_capturar_evento(batida, usuario, motivo):
    # Botão -> rota -> view -> retratar -> retratar_batida -> fechamento por evento.
    # app/templates/colaboradores/partials/_calendario_grade.html:127
    # app/ponto/urls.py:62, app/ponto/views.py:3453
    # app/ponto/services/retratar_humano.py:146
    # app/ponto/registro_batida.py:195, :287 e :301
    # captureOnCommitCallbacks coleta os callbacks reais sem executá-los.
    # app/chamados/tests/test_e2_celula_regenerada_reavalia.py:227
    with TestCase.captureOnCommitCallbacks(execute=False) as callbacks:
        ok, _ = retratar_humano.retratar(
            batida, motivo=motivo, observacao=OBSERVACAO, ator=usuario,
        )
        exigir(ok, "Porta recusou a retratação. app/ponto/services/retratar_humano.py:146")

    eventos = []
    for callback in callbacks:
        if (
            getattr(callback, "__module__", None) == fechamento.__name__
            and getattr(callback, "__qualname__", None) == "recalcular_por_evento.<locals>._rodar"
        ):
            contexto = getclosurevars(callback).nonlocals
            if (
                contexto["motivo"] == "retratacao"
                and contexto["colaborador"].pk == COLAB_ID
                and contexto["quando"] == batida.timestamp
            ):
                eventos.append(callback)
    exigir(len(eventos) == 1, "Evento da retratação sem confirmação. app/ponto/services/fechamento.py:1822")

    batida.refresh_from_db()
    exigir(batida.retratada_em is not None and not batida.aprovada,
           "Retratação sem confirmação. app/ponto/registro_batida.py:294")
    prazo = retratar_humano.prazo_de_desfazer(batida)
    exigir(prazo is not None, "Trilha de reversão ausente. app/ponto/services/retratar_humano.py:38")
    return eventos[0], prazo


def comparar(batida, antes):
    depois = fotografia("DEPOIS", batida, apos_retratacao=batida.retratada_em)
    diff = list(unified_diff(antes, depois, fromfile="antes", tofile="depois", lineterm=""))
    print("DIFF TXT")
    for linha in diff:
        print(linha)
    # diff_linhas soma as linhas removidas e adicionadas, sem os cabeçalhos.
    return sum(linha.startswith(("-", "+")) for linha in diff[2:])


def imprimir_reversao(usuario_id, prazo):
    # O comando é Python para o mesmo shell juliani. Janela de 7 dias da trilha.
    # app/ponto/views.py:3465 e app/ponto/services/retratar_humano.py:28, :38 e :186
    print(f"REVERSAO prazo_dias={retrar_dias()} prazo_local={timezone.localtime(prazo).isoformat()}")
    print(
        "from django.contrib.auth import get_user_model; "
        "from ponto.models import Batida; "
        "from ponto.services import retratar_humano; "
        f"print(retratar_humano.desfazer(Batida.objects.get(pk={ALVO}), "
        f"ator=get_user_model().objects.get(pk={usuario_id})))"
    )


def retrar_dias():
    # app/ponto/services/retratar_humano.py:28
    return retratar_humano.DIAS_PARA_DESFAZER


def fim(retratada, diff_linhas):
    print(f"FIM modo={MODO} retratada={retratada} diff_linhas={diff_linhas}", flush=True)


def executar():
    exigir(MODO in ("ensaio", "aplicar"), "MODO inválido. aval.txt:1")
    exigir(getattr(connection, "schema_name", None) == "juliani",
           "Schema divergente. app/config/settings/saas.py:22")
    exigir(transaction.get_autocommit() and not connection.in_atomic_block,
           "Use o shell fora de outra transação. app/ponto/services/fechamento.py:1848")

    ids_definidos = USUARIO_ID is not None and MOTIVO_ID is not None
    if MODO == "ensaio" and not ids_definidos:
        guardas(travar=False)
        listar()
        fim("nao", 0)
        return

    diff_ensaio = None
    try:
        with transaction.atomic(durable=True):
            batida = guardas(travar=True)
            motivos, usuarios = listar()
            exigir(type(USUARIO_ID) is int and USUARIO_ID in usuarios,
                   "USUARIO_ID sem permissão. app/ponto/services/retratar_humano.py:127")
            exigir(type(MOTIVO_ID) is int and MOTIVO_ID in motivos,
                   "MOTIVO_ID ausente ou inativo. app/ponto/services/retratar_humano.py:162")

            antes = fotografia("ANTES", batida)
            evento, prazo = retratar_e_capturar_evento(
                batida, usuarios[USUARIO_ID], motivos[MOTIVO_ID],
            )
            # O botão agenda _rodar em on_commit. A execução é síncrona após
            # o commit externo, na mesma conexão. O ensaio chama esse callback [nao verificado]
            # dentro do atomic, cuja reversão descarta os callbacks pendentes.
            # app/ponto/services/fechamento.py:1822–1851
            if MODO == "ensaio":
                evento()
                diff = comparar(batida, antes)
                imprimir_reversao(USUARIO_ID, prazo)
                diff_ensaio = diff
                raise ValueError("Rollback intencional do ensaio. app/ponto/services/fechamento.py:1848")
    finally:
        if diff_ensaio is not None:
            fim("nao", diff_ensaio)

    # O commit já executou o callback antes da fotografia em modo aplicar. [nao verificado]
    # Em caso de falha na fotografia após o commit, a reversão ainda é impressa. [nao verificado]
    # app/ponto/services/fechamento.py:1848 e app/ponto/services/retratar_humano.py:186
    try:
        diff = comparar(batida, antes)
    finally:
        imprimir_reversao(USUARIO_ID, prazo)
    fim("sim", diff)


# A configuração SaaS envia mensagens a streams e arquivos. O motor inclui
# nome em mensagens de erro. Silencia o logging Python durante esta execução,
# com restauração ao sair. A trilha ORM passa pela porta de auditoria.
# app/config/settings/saas.py:119–183, app/ponto/services/fechamento.py:1327
# app/ponto/registro_batida.py:245 e app/core/observ.py:133
nivel_anterior = logging.root.manager.disable
logging.disable(logging.CRITICAL)
try:
    executar()
finally:
    logging.disable(nivel_anterior)
```

## 27/11 10:21 · sessao e678aeb86a9e · RC-SEM-PROXIMA · verificador · origem=Code

**REPROVADO.** O candidato preserva a lista `RC SEM COLHER` quando a árvore, o RELATO e o instante são fixos. A interface mudou: a mesma obra pode aparecer nas duas listas numa única chamada, e o `~/vercodex.py` conta `RC SEM PROXIMA` como `RC SEM COLHER`. O prazo também pode disparar antes de completados 15 minutos reais. `bin/rc_sem_colher.py:89–113,116–163` · `/home/ronald/vercodex.py:163–171`

### 1. A lista `SEM COLHER` é idêntica?

**Para a mesma árvore, o mesmo RELATO e o mesmo `agora`, sim.** A regex aceita as mesmas linhas e `marcas()` devolve as mesmas chaves `(obra, caminho, HH:MM do rc)`. A seleção do `.rc`, a janela de 24 h, `--min-idade`, a folga, a leitura do código e a ordenação foram preservadas. A comparação com o anterior está no `diff.txt` anexo, hunks `-22`, `-47`, `-73` e `-101`. `bin/rc_sem_colher.py:26–56,63–102`

**Alta — o resultado observado do comando muda.** Com uma única obra colhida e parada, antes eram zero linhas e rc `0`. Agora sai `RC SEM PROXIMA` e rc `3`. Além disso, se a primeira passagem imprime um órfão e um diretório fica ilegível antes da segunda, a nova chamada termina com rc `1` depois de emitir a linha. A anterior terminaria com rc `3`. Reproduzi os dois resultados em memória. `bin/rc_sem_colher.py:59–73,153–163` · `diff.txt`, hunk `-101`

**Alta — as duas listas podem descrever estados incompatíveis.** Com `a.rc` colhido às 10:01 e `b.rc` mais novo e sem marca, a primeira passagem lista `b.rc`. Se `b.rc` some entre as passagens, a segunda lista `a.rc` como `SEM PROXIMA`. Reproduzi as duas linhas para a mesma obra na mesma chamada. O RELATO é lido uma vez, mas o índice de marcas, a varredura e o relógio são refeitos. `bin/rc_sem_colher.py:36–49,89–102,116–135,153–163`

### 2. Contraexemplos de `SEM PROXIMA`

| Gravidade | Critério | Contraexemplo |
|---|---|---|
| Média | Forma da marca | Uma colheita registrada como `colhido OBRA-X r.rc (10:00)` sem o prefixo `- HH:MM `, ou citada no meio da linha, deixa a obra parada fora de `SEM PROXIMA`. Ela pode aparecer como `SEM COLHER`. É o efeito previsto pela gramática preservada. `bin/rc_sem_colher.py:30–49,95–101,125–130` |
| Alta | Marca do rc mais novo | `a.rc` foi colhido e chegou um `b.rc` sem marca. Só `b.rc` é avaliado e, se elegível, sai em `SEM COLHER`. Já se `a.rc` for refeito no **mesmo caminho e minuto**, o novo resultado herda a marca antiga e pode sair em `SEM PROXIMA` sem ter sido colhido. Uma marca de outro dia com a mesma chave também colide. Em empate exato de mtime, fica o primeiro `.rc` visitado. `bin/rc_sem_colher.py:36–49,72–85,95–101,125–132` |
| Alta | `superado` e marcas múltiplas | Qualquer `superado` da mesma chave veta a saída, inclusive um antigo que colidiu com um rc refeito. O inverso também ocorre: `superado a.rc` deixa passar `b.rc` com `colhido`, mesmo que `b.rc` seja só um artefato posterior de uma obra já encerrada. Entre vários `colhido`, vence o maior horário inferido. Para rc às 10:00, uma marca válida às 10:01 e outra linha às 09:59 para a mesma chave fazem a segunda virar “amanhã” e esconder a obra parada. `bin/rc_sem_colher.py:36–43,105–113,125–134` |
| Alta | Escrita depois do rc | Um brief escrito **exatamente 5 s** depois do `.rc` ainda deixa a obra na lista. Um brief criado com mtime preservado ou um link simbólico também pode passar despercebido. No sentido inverso, um simples `touch` num arquivo regular da obra mais de 5 s depois do rc tira uma obra que continua parada. Um brief real escrito mais de 5 s depois do rc, ainda antes da marca, tira a obra conforme o caso do selo. `bin/rc_sem_colher.py:72–85,122–124` · `bin/tests/test_rc_sem_colher.sh:60–72` |
| Média | Escrita do juiz ou da espera | O juiz lê o RELATO, consulta a árvore e escreve em stdout. A espera cria a marca com `mktemp` e imprime os caminhos achados. Se `TMPDIR` estiver dentro da obra, ou a sessão redirecionar a saída para `logs/OBRA-X/espera.log`, o próprio arquivo de controle pode tirar a obra da lista. A ocorrência desses arranjos em produção é [nao verificado]. `bin/rc_sem_colher.py:52–56,72–85,153–163` · `bin/disjuntor.py:1967–1969` |
| Alta | 15 minutos, meia-noite e fração | Com rc às `10:00:30.7`, colheita às `10:00:59` e consulta às `10:15:01`, o juiz lista com **14 min 02 s** reais de espera. Ele usa `10:00:00` como instante da marca. A fração do mtime foi tratada corretamente para manter o mesmo minuto, e `23:50 → 00:10` funciona num dia de 24 h. Ainda assim, um rc de ontem às `23:50:59.9` e uma marca de hoje às `23:50:10` podem ser associados ao minuto de ontem enquanto o rc ainda está na janela de 24 h. Num dia com mudança de offset, somar `86400` também pode cair em outra hora local. `bin/rc_sem_colher.py:65–85,105–113,131–134` · `bin/tests/test_rc_sem_colher.sh:88–102` |

### 3. Quem escreve e quem lê

| Componente | Contrato observado |
|---|---|
| Produtores | O wrapper do Codex escreve o `.rc` da chamada do construtor em temporário e publica por `mv`. A sessão colhe e registra a marca no RELATO. O juiz usa o mtime do arquivo e o texto da marca, não o `fim=` gravado no `.rc`. `bin/codex_pergunta.sh:565–574` · `CLAUDE.md:332–337` · `app/docs/RELATO.md:13639–13646` · `bin/rc_sem_colher.py:78–85,125–132` |
| `ESPERA_UNICA` | Lê `$?`, repassa stdout e pula o laço com rc `3` de qualquer lista. A espera termina com rc `0` para rc `0` ou `3` do juiz e ainda pode imprimir caminhos do `find` final. Já dentro do laço, só observa `.rc` por `-cnewer`. Uma marca que completa 15 minutos durante a espera não a acorda por si só, e o juiz não é chamado de novo ao sair. `bin/disjuntor.py:1963–1969` |
| `espera_nega` e PreToolUse | Leem o **texto do comando Bash**. Verificam forma do laço, `find` e teto. A forma com o juiz retirado também passa, como confirmei em memória. O caso `caso_forma` exige a presença do juiz por uma asserção adicional. `.claude/settings.json:62–69` · `bin/disjuntor.py:2157–2174,2217–2228` · `bin/tests/test_rc_sem_colher.sh:160–162` |
| Stop e handoff | O Stop usa a prova do RELATO, as decisões do disjuntor e a fila. O handoff importa o Stop para ler fila, travas e `PAREI`, e grava `HANDOFF-SESSAO.md`. Esses caminhos não consomem stdout ou rc de `rc_sem_colher.py`. `.claude/settings.json:3–29` · `bin/hook_stop_fila1.py:1305–1329,1458–1501` · `bin/handoff_sessao.sh:17–21,44–78,124–145,292–338` |
| `~/vercodex.py` | **Alta — contagem errada no código local explicitamente nomeado.** Captura stdout e rc, conta toda linha não vazia sob `RC SEM COLHER` e exibe até 15 linhas. Três `SEM PROXIMA` viram `RC SEM COLHER: 3`, mesmo com zero órfãos. A chamada observada também não passa `--min-idade 900`, embora o contrato da cópia o documente. A versão efetivamente usada no pouso é [nao verificado]. `/home/ronald/vercodex.py:5,163–171` · `app/docs/RELATO.md:13639–13642` |
| `logs/espera_unica.sh` e outros | O script é citado no RELATO e `logs/` é ignorado. Seu conteúdo e seu tratamento do stdout e do rc são [nao verificado]. O selo da cópia lê o juiz diretamente e usa a forma de `ESPERA_UNICA`. Outros consumidores externos são [nao verificado]. `app/docs/RELATO.md:13598` · `.gitignore:53–54` · `bin/tests/test_rc_sem_colher.sh:55,75–108,143–144` |

### 4. Selo e RED

- **Os seis mutantes novos mordem nas verificações em memória.** `proxima-desligada` perde rc `3` mesmo imprimindo as três linhas. Os mutantes de etapa, idade e `superado` incluem respectivamente `OBRA-LANCADA`, `OBRA-RECENTE` e `OBRA-ENCERRADA`. Os de virada e fração erram a diferença de segundos esperada. `bin/tests/test_rc_sem_colher.sh:57–102,197–210`
- **O RED funcional das três obras cai pelo motivo certo no comportamento anterior mostrado no anexo.** O juiz antigo dá rc `0` e stdout vazio para as três colhidas. Os três casos de exclusão também ficam vermelhos, mas porque falta o controle positivo exigido por `_proxima_fora`. Os casos de meia-noite e fração cairiam por ausência de `_instante_da_marca`. A espera antiga entra no laço e não imprime `SEM PROXIMA`. `diff.txt`, hunks `-83` e `-101` · `bin/tests/test_rc_sem_colher.sh:75–108` · `bin/disjuntor.py:1967–1969`
- **Média — cobertura vazia e relógio.** `caso_proxima_vazia` fica verde com a funcionalidade ausente, inclusive no comportamento antigo. Tem três obras de exclusão e serve como controle negativo. `caso_proxima_espera` exige uma linha real, rc `0` e menos de 3 s, mas não tem mutante novo próprio. A fixture usa horas relativas com margens amplas e a meia-noite comum está coberta pelo cálculo. O selo não exercita `900`/`901` s nem os segundos desconhecidos da marca. A medição de `<3 s` usa `date +%s` e pode falhar por atraso de execução ou ajuste do relógio. `bin/tests/test_rc_sem_colher.sh:57–108,197–210`
- Fiz as sondas em memória porque o selo integral cria, altera e apaga arquivos. **Verde integral, RED executado no commit `59600163c` e pouso pela pista** ficam [nao verificado]. As três obras do selo são fixtures. Seu estado em produção fica [nao verificado]. `bin/tests/test_rc_sem_colher.sh:26–28,57–73,165–182` · `aval.txt:1`

### 5. Classes já reprovadas

O diff mantém um executável, uma regex de marca e um seletor de rc. Não acrescenta `except` ao juiz. A derivação em paralelo aparece **na execução**: duas passagens sobre a mesma fonte, com o predicado da folga repetido e dois `time.time()` internos em vez de um instante comum capturado na borda. O campo `HH:MM` também tem dois usos: identifica o minuto exibido e vira um instante com segundos `00` para decidir o prazo. A dupla observação e a antecipação do prazo têm contraexemplos reproduzidos acima. `bin/rc_sem_colher.py:30–49,59–102,105–135,138–167`

## 27/11 10:17 · sessao 60ded10607a8 · CASO-LITERAL-14 · sonda · origem=Code

# CASO-LITERAL-14 — Fase 1

A segunda volta cobre **17 sobras, em 12 dias de 5 colaboradores**. `154900` já estava nas 83 da primeira volta. O bloco completo de 14/11 muda sua classe de **(d)** para **(c)**. O consolidado fica em **99 batidas únicas, 43 dias e 14 colaboradores**. `compara.out:59–171` · `r_f1.md:44` · `r_f1.md:62` · `saída da sonda:3–23`

As referências à `saída da sonda` contam as linhas do texto colado a partir de `sonda_leitura`, incluindo a linha em branco. O ensaio local usou `parear_turnos` com as batidas e os marcos do envelope. Os papéis da ata e o ramo exato que gerou as sobras em produção estão [nao verificado]. `app/ponto/turnos.py:1192` · `app/ponto/turnos.py:1948` · `app/ponto/turnos.py:2005`

## 1. Classificação por dia

Cada ID entra no dia de seu `DIA.sobras_ids`. **Não** em `validada_por_admin` significa `aprovada_por_id=null`. O modelo distingue esse campo de `aprovada`, que tem valor padrão `True`. Nas linhas **(c)**, cabe ao admin conferir a escala na tela. O código também prevê folga trabalhada. `saída da sonda:3–445` · `app/ponto/models.py:62` · `app/escala/models.py:1080` · `app/ponto/motor_calculo_v2.py:2576`

| empresa | colab_id | data | batidas SOBRA (id hora tipo) | célula (estado, marcos) | classe | prova | validada_por_admin | dúvida |
|---:|---:|---|---|---|:---:|---|---|---|
| 2 | 986 | 2026-11-14 | `154900 11:59:38 S` | `127362` trabalho, `E10:00/S16:00/E17:00/S22:00` | **c** | `154840 10:01:39 E` → sobra → `154953 13:02:58 E` → `155336 22:00:59 S`. As bordas conferem. O ensaio reconheceu a pausa de **1 h 03 min 20 s**, cerca de 4 h antes dos marcos internos. A pergunta `58126` está pendente na célula, com chamados `31901` e `39082` abertos. `saída da sonda:3–23` · `app/ponto/turnos.py:1466` · `app/escala/utils.py:477` | Não | Horário de pausa autorizado e marcos anteriores à regeneração [nao verificado]. |
| 2 | 986 | 2026-11-20 | `160434 12:08:00 S` | `127368` trabalho, `E10:00/S16:00/E17:00/S22:00` | **c** | `160310 10:02:41 E` → sobra → `160436 13:10:07 E` → `160805 22:01:34 S`. As bordas conferem e o ensaio reconheceu pausa de **1 h 02 min 07 s** perto de 12h–13h. A pergunta `60335` aponta para `160434`. `60335` e `60540` estão pendentes, com chamados `31901` e `40124` abertos. `saída da sonda:26–41` · `app/ponto/turnos.py:1466` · `app/escala/utils.py:477` | Não | Horário autorizado, marcos anteriores à regeneração e resolução da pergunta de liveness [nao verificado]. |
| 2 | 989 | 2026-10-22 | `134543 17:08:58 E` | `127432` folga, `null` | **c** | No ensaio, `134543` fechou com `135077 08:32:05 S` de 23/10, **15 h 23 min 07 s** depois. A jornada começa em 22/10, em folga na célula, com vínculo `1423` de `18:00–06:00`. O chamado `33904` está aberto e inclui o par. `saída da sonda:50–64` · `app/docs/LEIS.md:117` · `app/ponto/turnos.py:312` · `app/ponto/turnos.py:1430` | Não | Escala autorizada e teor do chamado `33904` [nao verificado]. |
| 2 | 989 | 2026-10-23 | `135077 08:32:05 S` | `127433` trabalho, `E18:00/S06:00 (+1)` | **c** | No ensaio, a sobra fechou a jornada de **22/10** iniciada por `134543 17:08:58 E`, cuja célula `127432` está em folga. A jornada de 23/10 tem outro par, `135589 17:59:28 E` → `135908 06:00:14 S` de 24/10. A conferência de escala recai sobre 22/10. `saída da sonda:50–57` · `saída da sonda:84–100` · `app/docs/LEIS.md:117` · `app/ponto/turnos.py:312` | Não | Escala autorizada em 22/10 e teor do chamado aberto `33904` [nao verificado]. |
| 2 | 989 | 2026-10-25 | `137193 22:11:08 S` | `127435` trabalho, `E18:00/S06:00 (+1)`, pausa `null` | **a** | O ensaio fechou `136992 15:57:34 E` → `137291 05:58:14 S` de 26/10 no dia **25/10**, com pausa `137193` → `137210 23:02:41 E` de **51 min 33 s**. As duas bordas externas ficam a menos de 3 h dos marcos. O chamado `33904` está aberto e inclui a sobra. Sem ação na batida. `saída da sonda:122–136` · `app/ponto/turnos.py:429` · `app/ponto/turnos.py:453` · `app/docs/LEIS.md:116–117` | Não | Teor do chamado, papéis da ata e causa exata da sobra [nao verificado]. |
| 2 | 1010 | 2026-11-04 | `146083 17:56:08 E` | `143296` folga, `null` | **d** | `145446 06:00:53 S` fecha a jornada iniciada em 03/11 no ensaio. A sobra da tarde fica aberta com as batidas apuráveis. O ensaio só a liga a `146380 05:59:33 S` de 05/11 quando inclui `146307 00:05:03 S`, já retratada por `657`. O chamado `36783` está aberto e inclui essas batidas. `saída da sonda:157–180` · `app/ponto/turnos.py:1417` · `app/ponto/turnos.py:2961` | Não | Faltam a decisão sobre `146307` e a confirmação do papel de `146083` na jornada de 04/11 [nao verificado]. |
| 2 | 1010 | 2026-11-05 | `146321 01:04:44 E` · `146380 05:59:33 S` | `143297` trabalho, `E18:00/S06:00 (+1)`, pausa `null` | **d** | Com as batidas apuráveis, o ensaio fecha as duas sobras como um par na madrugada de 05/11. Ao incluir `146307 00:05:03 S` só no ensaio, elas passam a compor a jornada iniciada por `146083` em 04/11. O plantão cadastrado de 05/11 tem `147009 17:58:49 E` e `147338 06:26:13 S` de 06/11. `saída da sonda:199–224` · `app/ponto/turnos.py:285` · `app/ponto/turnos.py:1491` · `app/ponto/turnos.py:2961` | Não, ambas | Faltam a confirmação da jornada efetiva de 04/11, a decisão sobre `146307` e o teor do chamado aberto `36783` [nao verificado]. |
| 2 | 1010 | 2026-11-07 | `151808 05:54:00 S` | `143299` trabalho, `E18:00/S06:00 (+1)`, pausa `null` | **d** | A pergunta validada `54816` materializou `151808` para `data_evento=06/11`, célula `143298`. A célula de 07/11 tem a própria saída `149056 05:53:56 S` em **08/11**, após `148746 17:59:35 E` de 07/11. `151808.retratada_em=null`. Chamado-pai `36652` aberto e destino `37414` resolvido. `saída da sonda:243–267` · `app/chamados/models.py:3095` · `app/docs/LEIS.md:117` | **Sim, `657`** | Faltam os marcos de `143298` e o teor da declaração validada para identificar a jornada que `151808` deveria fechar [nao verificado]. |
| 2 | 1036 | 2026-11-14 | `154907 12:00:48 E` | `143957` trabalho, `E14:40/S18:00/E19:00/S23:00` | **a** | O ensaio fechou `154907` → `155066 15:50:47 S` e `155091 16:50:34 E` → `155363 23:00:24 S` no próprio dia, com pausa de **59 min 47 s**. A entrada está a **2 h 39 min 12 s** de `hi` e a saída a **24 s** de `hf`. Sem ação na batida. `saída da sonda:317–336` · `app/ponto/turnos.py:1491` · `app/ponto/motor_calculo_v2.py:2044` | Não | Papéis da ata e causa exata da sobra [nao verificado]. |
| 2 | 1036 | 2026-11-19 | `159844 18:04:22 S` | `143962` trabalho, `E14:40/S18:00/E19:00/S23:00` | **b** | A sobra veio **1 s** depois de `159843 18:04:21 E`. O par `159785 17:04:57 S` → `159843` já fecha a pausa de **59 min 24 s**, entre `159655 14:40:28 E` e `160048 23:00:07 S`. No ensaio, omitir apenas `159844` preservou as bordas e a pausa. Omitir `159843` desfez o par da pausa. A `S` de 1 s é o toque excedente. `saída da sonda:337–364` · `app/docs/RELATO.md:259` · `app/docs/LEIS.md:80` · `app/ponto/turnos.py:880` · `app/ponto/turnos.py:1487` | **Não**, `aprovada_por_id=null` | `retratada_em=null`. Sem pergunta ou chamado aberto sobre `159844` no recorte. As quatro perguntas e os chamados resolvidos exibidos são de 20/11 e apontam para outros IDs. `saída da sonda:352–374` |
| 2 | 1036 | 2026-11-20 | `160483 14:43:43 E` · `160622 18:18:19 S` · `160742 19:18:33 E` · `160829 23:01:03 S` | `143963` folga, `null` | **c** | Dois períodos de trabalho no próprio dia, com pausa de **1 h 00 min 14 s**, compatíveis com `14:40–23:00` do vínculo `1552`. As quatro perguntas apontam para a célula e para os quatro IDs. Os chamados foram resolvidos pelo sistema via julgamento da célula. `saída da sonda:380–409` · `app/escala/orfa_classe.py:62` · `app/chamados/models.py:2847` | Não, as quatro | Escala autorizada e causa da folga [nao verificado]. |
| 2 | 1039 | 2026-11-19 | `159385 12:02:11 S` · `159454 12:59:59 E` | `144160` trabalho, `E10:00/S16:00/E17:00/S22:00` | **c** | `159304 09:56:22 E` → duas sobras → `160033 22:05:39 S`. O ensaio reconheceu a pausa de **57 min 48 s**, cerca de 4 h antes dos marcos internos. `59844` aponta para `159454`. As três perguntas estão respondidas e sem validação, com chamados `38937` e `39967` abertos. `saída da sonda:415–431` · `app/ponto/turnos.py:1466` · `app/escala/utils.py:477` | Não, ambas | Horário de pausa autorizado e teor das respostas [nao verificado]. |

## 2. Contagem desta volta

Cada célula mostra **dias / batidas**. A lista de IDs confere com `compara.out:151–171`. As classes vêm da tabela acima, com base nos `DIA.sobras_ids` da saída. `saída da sonda:3–445`

| colab_id | (a) | (b) | (c) | (d) | total |
|---:|---:|---:|---:|---:|---:|
| 986 | 0 / 0 | 0 / 0 | 2 / 2 | 0 / 0 | 2 / 2 |
| 989 | 1 / 1 | 0 / 0 | 2 / 2 | 0 / 0 | 3 / 3 |
| 1010 | 0 / 0 | 0 / 0 | 0 / 0 | 3 / 4 | 3 / 4 |
| 1036 | 1 / 1 | 1 / 1 | 1 / 4 | 0 / 0 | 3 / 6 |
| 1039 | 0 / 0 | 0 / 0 | 1 / 2 | 0 / 0 | 1 / 2 |
| **Total** | **2 / 2** | **1 / 1** | **6 / 10** | **3 / 4** | **12 / 17** |

## 3. Batidas (b) sem dúvida e sem validação admin

`159844` é a única batida que atende aos critérios no envelope. `compara.out:164` · `saída da sonda:350–364` · `app/docs/RELATO.md:259`

```text
B batida_id=159844 colab_id=1036 data=2026-11-19 hora=18:04:22 tipo=S
```

## 4. Batidas (b) com validação admin ou dúvida

**Lista vazia.** A sobra com `aprovada_por_id` preenchido, `151808`, ficou em **(d)** pela falta de prova sobre a jornada a que pertence. `saída da sonda:243–267` · `app/ponto/models.py:62`

## 5. Porta do botão e desfazer

- **Retratar.** O botão **Não foi trabalho (retratar)** envia `POST` para `ponto:retratar_batida`. A view exige `regularizar_batida` e chama `retratar_humano.retratar`. O serviço reconfere a permissão, exige motivo ativo e chama `registro_batida.retratar_batida`. A porta preserva o registro, grava `aprovada=False` e `retratada_em`, registra a trilha e aciona a atualização do fechamento. A apuração seleciona `retratada_em__isnull=True`. `app/templates/colaboradores/partials/_calendario_grade.html:127` · `app/ponto/urls.py:62` · `app/ponto/views.py:3453` · `app/ponto/services/retratar_humano.py:158` · `app/ponto/services/retratar_humano.py:175` · `app/ponto/registro_batida.py:287` · `app/ponto/turnos.py:2961`
- **Desfazer.** O botão envia `POST` para `ponto:desfazer_retratacao`. A view exige a mesma permissão e chama `retratar_humano.desfazer`. A porta exige `retratada_em` preenchido, última trilha `retratar_batida_admin` e prazo de **7 dias** a partir da retratação. Usa `registro_batida.anotar` para gravar `aprovada=True`, limpar `retratada_em` e registrar `desfazer_retratacao_batida`. `app/templates/colaboradores/partials/_calendario_grade.html:127` · `app/ponto/urls.py:64` · `app/ponto/views.py:3465` · `app/ponto/services/retratar_humano.py:26` · `app/ponto/services/retratar_humano.py:38` · `app/ponto/services/retratar_humano.py:186` · `app/ponto/services/retratar_humano.py:203`
- A cadeia dos botões foi conferida nos arquivos da cópia. O topo de `sonda_f1.py`, seu hash e o `HEAD` Git da cópia estão [nao verificado].

## Consolidado das duas voltas

O ajuste retira a linha parcial de `154900` da classe **(d)** da primeira volta e conserva a linha completa em **(c)**. O colab `986` fica com **5 dias / 5 batidas**, todos em **(c)**. `r_f1.md:41–44` · `r_f1.md:61–62` · `saída da sonda:3–41` · `compara.out:147–152`

| classe | primeira volta | segunda volta | ajuste por sobreposição | total único |
|:---:|---:|---:|---:|---:|
| (a) | 1 / 1 | 2 / 2 | 0 / 0 | **3 / 3** |
| (b) | 0 / 0 | 1 / 1 | 0 / 0 | **1 / 1** |
| (c) | 19 / 65 | 6 / 10 | 0 / 0 | **25 / 75** |
| (d) | 12 / 17 | 3 / 4 | −1 / −1 | **14 / 20** |
| **Total** | **32 / 83** | **12 / 17** | **−1 / −1** | **43 / 99** |

**14 colaboradores e 99 batidas únicas.** `compara.out:59–171` · `r_f1.md:62` · `saída da sonda:3–445`

## 27/11 10:10 · sessao 88a168481d71 · CASO-LITERAL-14 · sonda · origem=Code

# CASO-LITERAL-14 — Fase 1

O trecho colado contém **83 batidas em `DIA.sobras_ids`, em 32 dias de 10 colaboradores**. A saída termina no bloco de 14/11 do colab 986, antes da célula. Em relação ao universo informado de 99 batidas e 14 colaboradores, faltam 16 batidas no recorte. A distribuição delas por dia, colaborador e classe está [nao verificado]. Usei os blocos `DIA`, `BAT`, `CEL`, `VINCULO`, `PERGUNTA` e `CHAMADO` colados como fonte dos fatos abaixo. A numeração original da saída, o topo de `sonda_f1.py`, `compara.out` item 4 e o hash Git da cópia estão [nao verificado].

## 1. Classificação por dia

A data e as sobras de cada linha vêm de `DIA.sobras_ids`. Uma `BAT` marcada `SOBRA` na janela de um dia vizinho entra na linha da sua própria lista `sobras_ids`. `Não` em `validada_por_admin` significa `aprovada_por_id=null` nas sobras da linha. O modelo distingue esse campo de `aprovada`, cujo valor padrão é `True`. [app/ponto/models.py:62](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/models.py:62)

Nas linhas **(c)**, a divergência observável vai para conferência de escala/cadastro pelo admin. Onde a célula marca folga, o código também prevê folga trabalhada. A necessidade de editar a célula, ajustar o vínculo ou manter a folga e tratar o trabalho depende da escala efetivamente autorizada [nao verificado]. [app/escala/models.py:1080](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/escala/models.py:1080) · [app/ponto/motor_calculo_v2.py:2576](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/motor_calculo_v2.py:2576)

| empresa | colab_id | data | batidas SOBRA (id hora tipo) | célula (estado, marcos) | classe | prova | validada_por_admin | dúvida |
|---:|---:|---|---|---|:---:|---|---|---|
| 3 | 163 | 2026-11-18 | `158099 05:06:08 S` | `140479` folga, `null` | **d** | `157833 18:53:02 E` de 17/11 precede `158092 04:07:13 S`, a sobra e `158201 06:59:03 E`. A pergunta `59348` liga a sobra à célula `140478` de 17/11 e ao chamado `35571`, aberto. A L-085 permite atribuir a madrugada à véspera. `app/ponto/turnos.py:285` | Não | Faltam os marcos e os papéis julgados da véspera, além do teor da resposta `59348`. A sequência tem duas `S` consecutivas. [nao verificado] |
| 3 | 163 | 2026-11-20 | `160135 05:12:54 S` | `140481` folga, `null` | **d** | `159878 18:53:11 E` de 19/11 precede `160127 04:13:41 S`, a sobra e `160202 06:56:06 E`. A pergunta `60098` liga a sobra à célula `140480` de 19/11 e ao chamado aberto `35571`. `app/ponto/turnos.py:285` | Não | Faltam os marcos e os papéis julgados da véspera e o teor da resposta `60098`. [nao verificado] |
| 2 | 219 | 2026-11-14 | `154790 07:03:07 E` · `154793 07:04:17 S` | `138398` folga, `null` | **d** | O par dura **70 s**, acima do piso de 60 s que impede formar turno. A véspera tem `154453 19:07:37 E` e três `S` na madrugada, incluindo `159534 02:00:00 S` aprovada por `657`. As perguntas `57827` e `57828` ligam as duas sobras ao chamado-pai `16611`, aberto. `app/ponto/turnos.py:130`, `app/ponto/turnos.py:1487` | Não, ambas | Faltam o fechamento e a ata da jornada de 13/11 ou confirmação de que a dupla foi um toque indevido. [nao verificado] |
| 2 | 301 | 2026-10-22 | `134361 14:45:11 E` · `134720 20:00:23 S` · `134740 21:00:21 E` · `134778 22:50:11 S` | `123197` folga, `null` | **c** | Dois períodos completos no próprio dia, com pausa de cerca de 1 h. O padrão se repete em 21/10 e 23/10. O vínculo `265`, template `397`, omite quinta-feira. `app/escala/orfa_classe.py:62` | Não | Escala autorizada para a quinta [nao verificado]. |
| 2 | 301 | 2026-10-29 | `140896 14:45:26 E` · `141196 20:00:05 S` · `141220 21:00:15 E` · `141257 22:50:02 S` | `123204` folga, `null` | **c** | Dois períodos no dia, com horários equivalentes aos de 28/10 e 30/10. O vínculo `265` omite quinta-feira. `app/escala/orfa_classe.py:62` | Não | Escala autorizada para a quinta [nao verificado]. |
| 2 | 301 | 2026-11-05 | `146858 14:45:34 E` · `147164 20:00:12 S` · `147191 21:02:44 E` · `147218 22:50:03 S` | `123211` folga, `null` | **c** | Dois períodos no dia. A série aprovada `152818`–`152821` é de **04/11**, na célula `123210`. A sequência de 05/11 tem IDs e instantes próprios. `app/escala/orfa_classe.py:62`, `app/docs/LEIS.md:117` | Não | Escala autorizada para a quinta [nao verificado]. |
| 2 | 301 | 2026-11-12 | `153134 14:45:13 E` · `153464 20:00:05 S` · `153492 21:00:04 E` · `153529 22:50:04 S` | `123218` folga, `null` | **c** | Dois períodos no dia, repetidos em 11/11 e 13/11. As perguntas `57176`, `57302`, `57325` e `57342` apontam para a própria célula e para o chamado aberto `22355`. `app/escala/orfa_classe.py:62` | Não | Escala autorizada para a quinta [nao verificado]. |
| 2 | 301 | 2026-11-19 | `159659 14:45:03 E` · `159996 20:04:10 S` · `160018 21:04:15 E` · `160039 22:50:05 S` | `123225` folga, `null` | **c** | Dois períodos no dia, repetidos em 18/11 e 20/11. As perguntas `59897`, `60018`, `60024` e `60038` apontam para a própria célula e para o chamado aberto `22355`. `app/escala/orfa_classe.py:62` | Não | Escala autorizada para a quinta [nao verificado]. |
| 2 | 334 | 2026-11-02 | `143656 01:40:09 S` · `143671 02:34:17 E` | `132547` folga, `null` | **c** | `143583 20:21:17 E` de 01/11 → pausa das duas sobras → `143844 07:52:44 S` de 02/11. O ensaio com as quatro batidas cruas formou um turno de **01/11**, com as sobras internas. As duas pontas estão mais de 3 h distantes de `10:00–22:00` do vínculo `1499`. A divergência é com o horário da jornada iniciada em 01/11. `app/ponto/turnos.py:385`, `app/ponto/turnos.py:407`, `app/ponto/turnos.py:312`, `app/ponto/motor_calculo_v2.py:2044` | Não | Marcos efetivos da célula `132546` de 01/11 e horário autorizado [nao verificado]. |
| 2 | 334 | 2026-11-10 | `150563 01:00:52 S` | `132555` folga, `null` | **d** | `150460 19:51:14 E` de 09/11 → sobra → `150585 02:02:38 E`. A pergunta `55996` liga `150585` à célula `132554` de 09/11. O ramo sem marcos exige uma `S` posterior para demonstrar continuidade após a volta. `app/ponto/turnos.py:393` | Não | Faltam a saída após `150585`, os marcos da véspera e a confirmação do papel de `150563`. [nao verificado] |
| 2 | 334 | 2026-11-16 | `156062 01:38:42 S` · `157001 20:34:22 E` | `132561` folga, `null` | **d** | `156062` fica entre `155980 20:12:08 E` de 15/11 e `156083 02:31:57 E`. Já `157001` pode formar jornada própria com `157080 00:03:27 S` de 17/11. As perguntas `58903` e `58935` ligam esse segundo trecho a 16/11 e ao chamado aberto `17636`. `app/ponto/turnos.py:312`, `app/ponto/turnos.py:1491` | Não | Falta o fechamento da jornada de 15/11 e a confirmação de qual `S` encerrou a de 16/11. As duas sobras da linha podem pertencer a jornadas diferentes. [nao verificado] |
| 2 | 334 | 2026-11-17 | `157080 00:03:27 S` · `157102 01:34:42 S` · `157339 07:57:06 S` | `132562` trabalho, `E10:00/S15:00/E16:00/S22:00` | **d** | As três `S` vêm depois de `157001 20:34:22 E` de 16/11 e antes de `163470 10:00:00 E`, plantada e aprovada por `657`. A pergunta `58935` liga `157080` a 16/11. Chamados `17636` e `39479` abertos. `app/ponto/turnos.py:293`, `app/ponto/turnos.py:1491` | Não | Faltam a saída efetiva de 16/11 e o papel das outras `S`. [nao verificado] |
| 2 | 446 | 2026-10-25 | `136793 08:00:46 E` | `136270` trabalho, `E14:40/S18:00/E19:00/S23:00` | **d** | `136990 15:57:29 S` permite um período diurno. `144666 18:00:00 S` e `144667 23:00:00 S` foram plantadas e aprovadas por `657` para a mesma célula. A L-084 exige identificar as duas bordas reais antes de comparar com os marcos. `app/ponto/motor_calculo_v2.py:2046` | Não | Falta confirmar se `136990` encerrou o trabalho e como os registros diurnos se relacionam às declarações validadas. [nao verificado] |
| 2 | 446 | 2026-10-27 | `138687 12:30:18 E` | `136272` trabalho, `E14:40/S18:00/E19:00/S23:00` | **d** | `138784 13:31:02 S` é candidata a fechar um período de 1 h. Também há `144664 18:00:00 S` e `145038 23:00:00 S`, plantadas e aprovadas por `657`. O chamado `36462` está `em_analise`. `app/ponto/turnos.py:1192` | Não | Faltam os papéis julgados e o teor da declaração para distinguir o período diurno da jornada declarada. [nao verificado] |
| 2 | 446 | 2026-11-01 | `143320 13:04:48 E` | `136277` trabalho, `E14:40/S18:00/E19:00/S23:00` | **d** | A entrada é 95 min 12 s anterior ao `hi`. Seguem `143451 18:01:12 S` e `143543 19:01:11 E`. A primeira `S` e a `E` seguinte estão junto dos marcos da pausa. `app/ponto/turnos.py:1356` | Não | Falta a saída após `143543` ou a ata que confirme a borda de encerramento. [nao verificado] |
| 2 | 446 | 2026-11-20 | `160414 13:00:43 E` | `136296` trabalho, `E14:40/S18:00/E19:00/S23:00` | **a** | A sonda lista `160414` como sobra. O ensaio de `parear_turnos` com as quatro batidas do dia e os marcos da célula fechou `160414 → 160528 15:52:59 S` e `160559 16:54:13 E → 160790 21:20:55 S`, ambos em 20/11. As bordas externas estão a menos de 3 h dos respectivos marcos. Sem ação sobre a batida. `app/ponto/turnos.py:1402`, `app/ponto/turnos.py:1430`, `app/ponto/turnos.py:1491`, `app/ponto/motor_calculo_v2.py:2044` | Não | O ramo exato que deixou a sobra e a obra que o corrige estão [nao verificado]. O ensaio usou as batidas do dia e os marcos exibidos, sem os papéis da ata. `app/ponto/turnos.py:1201`, `app/ponto/turnos.py:2005` |
| 2 | 465 | 2026-11-12 | `152798 09:00:29 S` · `152993 12:38:07 S` | `130759` trabalho, `E07:30/S12:30/E13:30/S19:30` | **d** | `152993` fica a 8 min 07 s de `hii` e tem `153079 13:34:14 E` como volta compatível. No ensaio com os marcos, `152798` fechou o período de `152760 07:33:15 E` e `152988 12:31:42 E` abriu o segundo período. `app/ponto/turnos.py:1356`, `app/ponto/turnos.py:1491` | Não | Falta confirmar o papel real de `152798` e `152988` para classificar o dia inteiro. [nao verificado] |
| 2 | 707 | 2026-10-27 | `152914 06:55:00 E` | `130929` trabalho, `E11:00/S16:00/E17:00/S23:00` | **d** | `152914` foi plantada pela pergunta validada `50832`. `152916 12:10:00 E` também foi plantada. `152915 16:00:00 S` já foi retratada, e `151794 19:00:00 S` tem trilha de retratação, desfazer e inversão. A célula foi regenerada. Chamados `30747` aberto e `35226` em análise. `app/ponto/models.py:62`, `app/ponto/turnos.py:2941` | **Sim, `657`** | Faltam os marcos anteriores à regeneração e a decisão sobre qual entrada correspondeu à jornada. `152914.retratada_em=null` no recorte. [nao verificado] para a jornada real. |
| 2 | 917 | 2026-10-24 | `136272 15:00:39 E` · `136465 19:00:26 S` · `136519 20:01:27 E` · `136570 22:58:12 S` | `123158` folga, `null` | **c** | Dois períodos no sábado, com horários equivalentes aos de 23/10. O vínculo `1134`, template `282`, omite sábado. `app/escala/orfa_classe.py:62` | Não | Escala autorizada para o sábado [nao verificado]. |
| 2 | 917 | 2026-10-31 | `142679 15:00:20 E` · `142887 19:02:25 S` · `142930 20:00:34 E` · `142989 22:53:43 S` | `123165` folga, `null` | **c** | Dois períodos no sábado, com horários equivalentes aos de 30/10 e 01/11. O vínculo `1134` omite sábado. `app/escala/orfa_classe.py:62` | Não | Escala autorizada para o sábado [nao verificado]. |
| 2 | 917 | 2026-11-14 | `155033 15:00:59 E` · `155258 19:00:45 S` · `155296 20:00:18 E` · `155357 22:57:03 S` | `123179` folga, `null` | **c** | Dois períodos no sábado, com horários equivalentes aos de 13/11. As perguntas `57974`, `58082`, `58106` e `58134` apontam para essa célula e para o chamado aberto `20366`. `app/escala/orfa_classe.py:62` | Não | Escala autorizada para o sábado [nao verificado]. |
| 2 | 968 | 2026-10-22 | `134368 14:49:09 E` · `134573 18:02:02 S` · `134668 19:00:22 E` · `134777 22:50:06 S` | `122828` folga, `null` | **c** | Dois períodos na quinta, com horários equivalentes aos de 21/10. O vínculo `1358`, template `397`, omite quinta-feira. `app/escala/orfa_classe.py:62` | Não | Escala autorizada para a quinta [nao verificado]. |
| 2 | 968 | 2026-10-29 | `140898 14:50:08 E` · `141045 18:00:11 S` · `141137 19:00:15 E` · `141258 22:50:07 S` | `122835` folga, `null` | **c** | Dois períodos na quinta, com horários equivalentes aos de 28/10. O vínculo `1358` omite quinta-feira. `app/escala/orfa_classe.py:62` | Não | Escala autorizada para a quinta [nao verificado]. |
| 2 | 968 | 2026-11-02 | `144020 14:50:08 E` · `144120 18:00:05 S` · `144221 19:00:17 E` · `144323 22:50:05 S` | `122839` folga, `null` | **c** | Dois períodos no dia, com horários equivalentes aos de 01/11 e 03/11. A etiqueta do template `397` inclui segunda-feira, e a célula está em folga. `app/escala/models.py:1080`, `app/escala/models.py:1384` | Não | Causa da folga e escala autorizada [nao verificado]. |
| 2 | 968 | 2026-11-05 | `146860 14:49:19 E` · `147014 18:00:08 S` · `147112 19:00:08 E` · `147219 22:50:23 S` | `122842` folga, `null` | **c** | Dois períodos na quinta, com horários equivalentes aos de 04/11. O vínculo `1358` omite quinta-feira. `app/escala/orfa_classe.py:62` | Não | Escala autorizada para a quinta [nao verificado]. |
| 2 | 968 | 2026-11-12 | `153136 14:50:08 E` · `153317 18:00:06 S` · `153416 19:00:25 E` · `153531 22:50:14 S` | `122849` folga, `null` | **c** | Dois períodos na quinta, com horários equivalentes aos de 11/11. As perguntas `57177`, `57256`, `57276` e `57343` apontam para essa célula. O chamado `37385` consta como resolvido pelo sistema. `app/escala/orfa_classe.py:62` | Não | Escala autorizada para a quinta [nao verificado]. |
| 2 | 968 | 2026-11-15 | `155745 14:49:57 E` · `155836 18:00:14 S` · `155922 19:00:05 E` · `156007 22:50:16 S` | `122852` folga, `null` | **c** | Dois períodos no dia, com horários equivalentes aos de 16/11. A etiqueta do template `397` inclui domingo. A série aprovada `158537`–`158540` pertence a **14/11**, célula `122851`. `app/escala/models.py:1080`, `app/docs/LEIS.md:117` | Não | Causa da folga e escala autorizada [nao verificado]. |
| 2 | 968 | 2026-11-19 | `159662 14:49:14 E` · `159831 18:00:16 S` · `159931 19:00:15 E` · `160040 22:50:06 S` | `122856` folga, `null` | **c** | Dois períodos na quinta, com horários equivalentes aos de 18/11. As perguntas `59898`, `59966`, `59987` e `60039` apontam para a célula `122856`, com quatro chamados resolvidos pelo sistema. `app/escala/orfa_classe.py:62` | Não | Escala autorizada para a quinta [nao verificado]. |
| 2 | 986 | 2026-10-29 | `140694 12:01:23 S` | `127346` trabalho, `E10:00/S16:00/E17:00/S22:00` | **c** | `140600 10:00:39 E` → sobra → `140772 13:00:41 E` → `141243 22:00:18 S`. As bordas conferem e a pausa de 59 min 18 s fica cerca de 4 h antes dos marcos da pausa. Célula regenerada em 13/11. Pergunta `57451` pendente e chamado `38767` aberto. `app/escala/utils.py:477`, `app/ponto/motor_calculo_v2.py:2046` | Não | Motivo do horário da pausa e marcos anteriores à regeneração [nao verificado]. |
| 2 | 986 | 2026-11-06 | `147648 12:03:33 S` | `127354` trabalho, `E10:00/S16:00/E17:00/S22:00` | **c** | `147551 09:53:34 E` → sobra → `147741 13:09:54 E` → `148171 22:03:20 S`. As bordas conferem e a pausa de 1 h 06 min 21 s volta a ocorrer perto de 12h–13h. Célula regenerada em 13/11. Pergunta `57452` pendente e chamado `38769` aberto. `app/escala/utils.py:477`, `app/ponto/motor_calculo_v2.py:2046` | Não | Motivo do horário da pausa e marcos anteriores à regeneração [nao verificado]. |
| 2 | 986 | 2026-11-10 | `150969 12:50:48 S` | `127358` trabalho, `E10:00/S16:00/E17:00/S22:00` | **c** | `150825 09:51:19 E` → sobra → `151040 13:50:21 E` → `151438 22:03:41 S`. As bordas conferem e a pausa de 59 min 33 s fica mais de 3 h antes de ambos os marcos internos. Pergunta `56242` aponta diretamente para a sobra e está pendente. Chamados `31901` e `38770` abertos. `app/escala/utils.py:477`, `app/ponto/motor_calculo_v2.py:2046` | Não | Motivo do horário da pausa e marcos anteriores à regeneração [nao verificado]. |
| 2 | 986 | 2026-11-14 | `154900 11:59:38 S` | [nao verificado] | **d** | `154840 10:01:39 E` → sobra → `154953 13:02:58 E`. A pausa candidata dura 1 h 03 min 20 s. A saída colada termina durante a auditoria de `154953`. `app/ponto/turnos.py:407` | Não | Faltam célula, marcos, saída final e eventual pergunta ou chamado desse dia. [nao verificado] |

## 2. Contagem por classe e por colab

Cada célula mostra **dias / batidas**. Fonte da contagem: os `DIA.sobras_ids` transcritos na tabela acima. A contagem da competência completa está [nao verificado].

| colab_id | (a) | (b) | (c) | (d) | total |
|---:|---:|---:|---:|---:|---:|
| 163 | 0 / 0 | 0 / 0 | 0 / 0 | 2 / 2 | 2 / 2 |
| 219 | 0 / 0 | 0 / 0 | 0 / 0 | 1 / 2 | 1 / 2 |
| 301 | 0 / 0 | 0 / 0 | 5 / 20 | 0 / 0 | 5 / 20 |
| 334 | 0 / 0 | 0 / 0 | 1 / 2 | 3 / 6 | 4 / 8 |
| 446 | 1 / 1 | 0 / 0 | 0 / 0 | 3 / 3 | 4 / 4 |
| 465 | 0 / 0 | 0 / 0 | 0 / 0 | 1 / 2 | 1 / 2 |
| 707 | 0 / 0 | 0 / 0 | 0 / 0 | 1 / 1 | 1 / 1 |
| 917 | 0 / 0 | 0 / 0 | 3 / 12 | 0 / 0 | 3 / 12 |
| 968 | 0 / 0 | 0 / 0 | 7 / 28 | 0 / 0 | 7 / 28 |
| 986 | 0 / 0 | 0 / 0 | 3 / 3 | 1 / 1 | 4 / 4 |
| **Total** | **1 / 1** | **0 / 0** | **19 / 65** | **12 / 17** | **32 / 83** |

## 3. Batidas (b) sem dúvida e sem validação admin

**Lista vazia no recorte.** Nenhuma linha `B batida_id=...` é sustentada pelos blocos da tabela. A classificação das sobras ausentes está [nao verificado].

## 4. Batidas (b) com validação admin ou dúvida

**Lista vazia no recorte.** Os casos duvidosos estão em **(d)**. Em particular, `154790` e `154793` têm `aprovada_por_id=null` e `retratada_em=null`, perguntas `57827` e `57828` materializadas via `celula_absolveu` e chamado-pai `16611` aberto. O chamado `39246` também está aberto na célula `138398`, com `batidas_contexto_ids=[]`. O código descreve `celula_absolveu` como resultado do julgamento da célula. A causa da dupla de 70 s está [nao verificado]. [app/chamados/models.py:2847](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:2847)

## 5. Botão de retratar e desfazer

- **Retratar.** O botão **Não foi trabalho (retratar)** envia `POST` à rota `ponto:retratar_batida`. A view exige `regularizar_batida` e chama `retratar_humano.retratar`. O serviço reconfere a permissão, exige motivo ativo e chama `registro_batida.retratar_batida`. A porta grava `aprovada=False` e `retratada_em`, registra a trilha e aciona a atualização do fechamento. O filtro de apuração usa `retratada_em__isnull=True`. [app/templates/colaboradores/partials/_calendario_grade.html:127](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/templates/colaboradores/partials/_calendario_grade.html:127) · [app/ponto/urls.py:62](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/urls.py:62) · [app/ponto/views.py:3453](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/views.py:3453) · [app/ponto/services/retratar_humano.py:158](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/retratar_humano.py:158) · [app/ponto/services/retratar_humano.py:175](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/retratar_humano.py:175) · [app/ponto/registro_batida.py:287](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/registro_batida.py:287) · [app/ponto/turnos.py:2961](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/turnos.py:2961)
- **Desfazer.** O botão envia `POST` à rota `ponto:desfazer_retratacao`. A view chama `retratar_humano.desfazer`. A porta exige a mesma permissão, `retratada_em` preenchido, última trilha `retratar_batida_admin` e prazo de **7 dias** a partir da retratação. Usa `registro_batida.anotar` para gravar `aprovada=True`, limpar `retratada_em` e registrar `desfazer_retratacao_batida`. [app/templates/colaboradores/partials/_calendario_grade.html:127](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/templates/colaboradores/partials/_calendario_grade.html:127) · [app/ponto/urls.py:64](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7

[truncado em 20000 de 20597 caracteres]

## 27/11 10:08 · sessao 5706d8bf958d · RAZAO-DA-SOBRA · construtor · origem=Code

**reds1 entregue na cópia.** São 37 testes de comportamento e controle, mais 15 mutantes em memória. A checagem estática confirmou sintaxe válida, 10 âncoras presentes uma vez e 5 ausentes. O diff consiste nos seis módulos de teste abaixo. [Fonte](/home/ronald/wt-codex-razao-da-sobra-094857/app/escala/tests/test_razao_sobra_fonte.py:1) · [Lavra](/home/ronald/wt-codex-razao-da-sobra-094857/app/ponto/tests/test_razao_sobra_lavra.py:1) · [Leitores](/home/ronald/wt-codex-razao-da-sobra-094857/app/ponto/tests/test_razao_sobra_leitores.py:1) · [Repasse](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_repasse.py:1) · [E2 e fábrica](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_e2_fabrica.py:1) · [Mutantes](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:1) [nao verificado]

**Veredito da suite e linhas de `red.log` [nao verificado].** A rota da Pista 2 usa SSH, cria um commit de foto para a cópia suja e grava `refs/pista`. Essas operações esbarram nas restrições da volta. A rota alternativa também escreve um env-file. O comando autorizado para quem puder executar está no fim. [suite.sh](/home/ronald/wt-codex-razao-da-sobra-094857/bin/suite.sh:324) · [SSH](/home/ronald/wt-codex-razao-da-sobra-094857/bin/suite.sh:378) · [Foto e ref](/home/ronald/wt-codex-razao-da-sobra-094857/bin/suite.sh:420) · [Rota alternativa](/home/ronald/wt-codex-razao-da-sobra-094857/bin/suite.sh:509) [nao verificado]

### REDs e controles

Os motivos abaixo vêm da leitura da base. O resultado de cada asserção em execução segue **[nao verificado]**.

| Resposta ou volta | Caso e motivo esperado na base |
|---|---|
| R1 | Oposta a 8 s, borda inclusiva de 10 s e residual a 11 s. Falha explícita `autoridade escala.orfa_classe.razao_da_sobra ausente na base`. A classe legada opera por minuto. [RED](/home/ronald/wt-codex-razao-da-sobra-094857/app/escala/tests/test_razao_sobra_fonte.py:74) · [Base](/home/ronald/wt-codex-razao-da-sobra-094857/app/escala/orfa_classe.py:53) |
| R2 | Duas sobras no mesmo minuto em foto antiga. Falhas independentes `autoridade escala.services.leitor_celula.sobras_da_ata ausente na base` e `autoridade ponto.services.cartorio.medir_razoes_sobras ausente na base`. O leitor atual entrega horas. [REDs](/home/ronald/wt-codex-razao-da-sobra-094857/app/escala/tests/test_razao_sobra_fonte.py:140) · [Base](/home/ronald/wt-codex-razao-da-sobra-094857/app/escala/services/leitor_celula.py:180) |
| R3 | O callback autônomo de isenção segue executando com TXT vigente. O mutante estende `_K6` ao callback e cobra a perda do silêncio. [Controle](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_repasse.py:190) · [Mutante](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:174) |
| R4 | Um admitido e um vetado. O teste exige agregado com um item e trilha da recusa por item. A base grava os dois IDs antes de processar as perguntas. [RED](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_repasse.py:130) · [Base](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/services/disputa_emissao.py:1117) | [nao verificado]
| R5 / A emissor | Origem aberta e alvo efetivo entregue. O teste observa a resolução do dia dentro de `_nascer`, os `INSERT`s, os emitidos e a releitura do chamador. A base executa `save(force_insert=True)` e a guarda B2 do `save` incide em alterações. [REDs](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_repasse.py:72) · [Nascimento](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/models.py:3333) · [Guarda](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/models.py:3847) | [nao verificado]
| R6 | O residual é provocado depois do `carimbar` real. O teste exige rollback ao `SAVEPOINT`, contagens iguais em todas as tabelas detectadas na escrita e vizinha intacta. A base chama o corpo e a precedência sem a cerca desse ato. [RED](/home/ronald/wt-codex-razao-da-sobra-094857/app/ponto/tests/test_razao_sobra_lavra.py:271) · [Base](/home/ronald/wt-codex-razao-da-sobra-094857/app/ponto/services/cartorio.py:2112) | [nao verificado]
| V1 | Identidades distintas no mesmo instante, segundos no mesmo minuto, duas linhas na meia-noite e CxR com par adicional. A fonte atual reduz o insumo a instante, tipo e marca de intervalo e entrega sobras em `HH:MM`. [REDs](/home/ronald/wt-codex-razao-da-sobra-094857/app/escala/tests/test_razao_sobra_fonte.py:166) · [Entrada](/home/ronald/wt-codex-razao-da-sobra-094857/app/escala/utils.py:1367) · [Saída](/home/ronald/wt-codex-razao-da-sobra-094857/app/escala/utils.py:1883) |
| V2 | Primeira ata para o supra, persistência, retirada da ocorrência na reconciliação, duas relavras e contadores. A base lavra horas e contagem, retira por minuto e omite os contadores de razão. [REDs](/home/ronald/wt-codex-razao-da-sobra-094857/app/ponto/tests/test_razao_sobra_lavra.py:60) · [Ata](/home/ronald/wt-codex-razao-da-sobra-094857/app/ponto/services/cartorio.py:749) · [Reconciliação](/home/ronald/wt-codex-razao-da-sobra-094857/app/escala/utils.py:2093) · [Contadores](/home/ronald/wt-codex-razao-da-sobra-094857/app/ponto/services/cartorio.py:2021) |
| V3 | Mesma ocorrência e razão na tela, PDF, supra e insumos de chamados e folha. O selo cobra leituras pela porta. A base ainda leva a hora crua ao template e compara por hora no PDF. [REDs](/home/ronald/wt-codex-razao-da-sobra-094857/app/ponto/tests/test_razao_sobra_leitores.py:74) · [Template](/home/ronald/wt-codex-razao-da-sobra-094857/app/templates/ponto/espelho.html:242) · [PDF](/home/ronald/wt-codex-razao-da-sobra-094857/app/relatorios/pdf_espelho.py:607) |
| A cartório | Margem de meia-noite, 10:00 com ambiente 10:02, C6 no gate e no tripwire, primeira lavra C6 `0→1` sem subir emitidos e veto antes da escrita. A base consulta o relógio ambiente no avulso e chama C6 sem `agora` no caminho compartilhado. [REDs](/home/ronald/wt-codex-razao-da-sobra-094857/app/ponto/tests/test_razao_sobra_lavra.py:28) · [Avulso](/home/ronald/wt-codex-razao-da-sobra-094857/app/ponto/services/cartorio.py:2083) · [C6](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/models.py:368) |
| A E2 e fábrica | Duas candidatas, TXT em um dia, carimbo do admitido, fecho adiado da vetada e veto antes do descarimbo. A base percorre as candidatas do E2 por colaborador e chama o descarimbo antes da guarda de limbo. [REDs](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_e2_fabrica.py:65) · [E2](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/reconciliador.py:681) · [Fábrica](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/services/fabrica_da_celula.py:206) |

Os controles escritos cobrem saída residual a 11 s, pausa L-032, foto histórica, ordem C → D → Ch → P, callback autônomo, dia sem ata, lista vazia, leitor alimentado com zero consultas e efeito zero em dinheiro, causa e retenção. **Verde em suite [nao verificado].** [Fonte](/home/ronald/wt-codex-razao-da-sobra-094857/app/escala/tests/test_razao_sobra_fonte.py:106) · [Repasse](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_repasse.py:143) · [Fábrica](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_e2_fabrica.py:128) · [Lavra](/home/ronald/wt-codex-razao-da-sobra-094857/app/ponto/tests/test_razao_sobra_lavra.py:135) · [Leitores](/home/ronald/wt-codex-razao-da-sobra-094857/app/ponto/tests/test_razao_sobra_leitores.py:170) [nao verificado]

Os testes assumem `batida_id`, `instante` e `razao` no registro de ocorrência, além das chaves `minutos_ambiguos` e `sobras_por_razao` nos contadores. A forma final dessas interfaces, a cobertura da borda 36, o toque em viewport real, o teto total de consultas da lavra e a publicação no placar e no HAIKU seguem **[nao verificado]**. O brief transcrito foi a referência disponível. A leitura integral do dossiê §6/§8 na cópia segue **[nao verificado]**. [Ocorrência e conferência](/home/ronald/wt-codex-razao-da-sobra-094857/app/escala/tests/test_razao_sobra_fonte.py:153) · [Contadores](/home/ronald/wt-codex-razao-da-sobra-094857/app/ponto/tests/test_razao_sobra_lavra.py:101) [nao verificado]

### Mutantes

Cada rótulo abaixo é argumento de `rodar_mutante`. **Presente** indica âncora única conferida estaticamente, com mordida **[nao verificado]**. **Skip** indica âncora ausente na base e mensagem `espera a cura RAZAO-DA-SOBRA`. [Executor](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:28)

| Rótulo | Âncora | RED | Estado |
|---|---|---|---|
| `CatracaMutantesRazaoSobraTest.test_leitor_cru` | `_marcos_faltantes`, chamada a `grade_da_celula` [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:62) | V3 catraca | Presente | [nao verificado]
| `CatracaMutantesRazaoSobraTest.test_relogio_solto_no_gate_avulso` | `julgar_celula`, `agora < _fim` [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:73) | A1 10:00/10:02 | Skip | [nao verificado]
| `LavraMutantesRazaoSobraTest.test_falta_de_razao_aparece_no_contador` | `julgar_celula`, `return out` [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:89) | V2 `sobras_sem_razao=0` | Presente | [nao verificado]
| `LavraMutantesRazaoSobraTest.test_soma_por_razao_perde_uma_sobra` | `julgar_celula`, `return out` [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:96) | V2 soma igual a `n_orfas` | Presente | [nao verificado]
| `LavraMutantesRazaoSobraTest.test_correcao_so_no_cron` | `julgar_celula`, guarda `competencia_do_dia_entregue` [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:106) | A1 veto no avulso | Skip | [nao verificado]
| `LavraMutantesRazaoSobraTest.test_relogio_so_no_gate` | `_gravar_estado`, `_vivas_entre(..., agora=agora)` [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:113) | A1 C6 no tripwire | Skip | [nao verificado]
| `LavraMutantesRazaoSobraTest.test_relogio_perdido_na_escrita` | `_julgar_colab_corpo`, `carimbar(..., agora=agora)` [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:122) | A1 instante persistido | Presente | [nao verificado]
| `LavraMutantesRazaoSobraTest.test_parei_residual_libera_savepoint` | `executar_sob_cerca`, `raise` após veto anotado [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:129) | R6 escrita parcial | Presente | [nao verificado]
| `E2MutantesRazaoSobraTest.test_veta_colaborador_inteiro` | `retratar_perguntas_absolvidas_por_celula`, `if not frescas` [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:146) | A2 carimbo da admitida | Presente | [nao verificado]
| `E2MutantesRazaoSobraTest.test_guarda_so_depois_do_descarimbo` | `perguntar_pela_celula`, guarda de entrega antes do descarimbo [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:156) | A2 veto antes da escrita | Skip | [nao verificado]
| `E2MutantesRazaoSobraTest.test_inventa_zero_de_lista_vazia` | `julgar_colab`, registro `[PERGUNTA]` [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:164) | A2 preserva emissão confirmada | Presente | [nao verificado]
| `RepasseMutantesRazaoSobraTest.test_k6_estendido_a_signal_on_commit_autonomo` | `_isentos_post_save_chamado`, `silenciar_do_colab(colab_id)` [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:174) | R3 alcance | Presente | [nao verificado]
| `RepasseMutantesRazaoSobraTest.test_filtra_o_dia_antes_de_nascer` | `criar_disputa_a_partir_de_chamados_orfaos`, import de `_dtc_i` [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:185) | R5 duas candidatas chegam a `_nascer` | Presente | [nao verificado]
| `RepasseMutantesRazaoSobraTest.test_insere_antes_do_veto` | `_nascer`, `vigiar_escrita_de_perguntas([p])` antes do `save` [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:195) | R5 `INSERT` só para admitido | Skip | [nao verificado]
| `RepasseMutantesRazaoSobraTest.test_veta_o_lote_inteiro` | `criar_disputa_a_partir_de_chamados_orfaos`, seleção `_intervalo` [teste](/home/ronald/wt-codex-razao-da-sobra-094857/app/chamados/tests/test_razao_sobra_mutantes.py:203) | R5 retorno de sucesso | Presente | [nao verificado]

### Comandos

A partir da raiz da cópia, com autorização para a rota da Pista 2 e com o diretório de log disponível. Destino `logs/RAZAO-DA-SOBRA/reds1/red.log`. [Contrato da suite](/home/ronald/wt-codex-razao-da-sobra-094857/bin/suite.sh:22)

```bash
copia=/home/ronald/wt-codex-razao-da-sobra-094857
cd "$copia"

PYTHONDONTWRITEBYTECODE=1 bash bin/suite.sh --dir "$copia" \
  --only "escala.tests.test_razao_sobra_fonte ponto.tests.test_razao_sobra_lavra ponto.tests.test_razao_sobra_leitores chamados.tests.test_razao_sobra_e2_fabrica chamados.tests.test_razao_sobra_repasse chamados.tests.test_razao_sobra_mutantes" \
  --parallel 1 > logs/RAZAO-DA-SOBRA/reds1/red.log

rodar_mutante() {
  PYTHONDONTWRITEBYTECODE=1 bash bin/suite.sh --dir "$copia" \
    --only "chamados.tests.test_razao_sobra_mutantes.$1" --parallel 1
}
```

A certificação continua com Code e opus final. **`red.log` e veredito [nao verificado].**

## 27/11 10:03 · sessao 123515df12cb · CASO-LITERAL-14 · leitura · origem=Code

```python
SOBRAS = [
    # Preencha com (empresa, colab_id, "AAAA-MM-DD", batida_id).
    # Exemplos da amostra_sobra.txt:1-2
    # (2, 43, "2026-11-02", 143768),
    # (2, 43, "2026-11-02", 143890),
]

# O envelope recebe um .py do chamador e o executa com
# tenant_command shell --schema=juliani e exec(open(...).read()).
# O código e a sonda são montados em leitura e o ORM usa a role leitor.
# bin/sonda_leitura.sh:63-91,153-181,235-243
#
# PORTA DA FASE 2
# O botão "Não foi trabalho (retratar)" faz POST com motivo_id e observacao.
# app/templates/colaboradores/partials/_calendario_grade.html:127
# ponto:retratar_batida -> retratar_batida_view -> retratar_humano.retratar
# -> registro_batida.retratar_batida -> registro_batida.anotar.
# O serviço humano exige a permissão regularizar_batida e motivo cadastrado
# ativo. O escritor preserva a linha, grava aprovada=False e retratada_em,
# registra o motivo na auditoria e dispara o evento de fechamento.
# app/ponto/urls.py:62; app/ponto/views.py:3453-3462
# app/ponto/services/retratar_humano.py:118-183
# app/ponto/registro_batida.py:255-302
#
# O botão "desfazer" usa desfazer_retratacao_view -> retratar_humano.desfazer
# -> anotar. A mesma permissão é exigida. A última linha de auditoria da
# batida deve ser retratar_batida_admin e o prazo é de sete dias a partir
# de retratada_em. O ato grava retratada_em=None e aprovada=True.
# app/templates/colaboradores/partials/_calendario_grade.html:127
# app/ponto/urls.py:64; app/ponto/views.py:3465-3473
# app/ponto/services/retratar_humano.py:26-49,186-206
#
# Campos de aprovação/admin em Batida: aprovada, aprovada_por (lido via
# aprovada_por_id), aprovada_em e retratada_em. O sinal de aprovação
# atribuída a um usuário é aprovada_por_id preenchido. aprovada tem
# default True. Edições de tipo e de horário têm autoria e data na trilha.
# app/ponto/models.py:22-95; app/ponto/views.py:490-494
# app/ponto/services/flip_auto.py:285-298; app/ponto/services/flip_batida.py:41-49
# app/chamados/services/regularizacao_ext.py:150-162; app/core/models.py:285-307
#
# Lista completa dos 14, dados de produção e execução com o ORM no
# envelope [nao verificado]. Sintaxe e funções puras conferidas em memória.

import json
from collections import defaultdict
from datetime import date, datetime, time, timedelta
from functools import lru_cache
from zoneinfo import ZoneInfo

from django.db import connection
from django.db.models import Q
from django.utils import timezone

from chamados.catalogo.modulos import data_do_chamado
from chamados.models import (
    ChamadoColaborador,
    DecisaoPergunta,
    FollowupChamado,
    PerguntaDisputa,
    RegularizacaoItemBatida,
)
from colaboradores.models import Colaborador
from core.models import LogAuditoria
from escala.models import CelulaDia
from escala.servico_jornada import escala_vigente
from ferias.models import AgendamentoFerias
from ponto.catalogo.ausencias import CATALOGO, INDETERMINADO
from ponto.models import Ausencia, Batida, Justificativa, TipoAusencia

TZ = ZoneInfo("America/Sao_Paulo")  # app/config/settings/base.py:93-95
UM_DIA = timedelta(days=1)
NV = "[nao verificado]"


def formatar(valor):
    if isinstance(valor, datetime):
        return timezone.localtime(valor, TZ).isoformat()
    if isinstance(valor, (date, time)):
        return valor.isoformat()
    if isinstance(valor, str) and valor and "W004" not in valor and all(
        c.isalnum() or c in "_-+.:/" for c in valor
    ):
        return valor
    # Escapes JSON preservam o valor ao passar pelo filtro do envelope.
    # bin/sonda_leitura.sh:239-243
    return json.dumps(valor, ensure_ascii=False).replace(
        "W004", r"\u0057004"
    ).replace("objects imported", r"objects\u0020imported")


def linha(rotulo, *, marca="", **campos):
    valores = " ".join(f"{k}={formatar(v)}" for k, v in campos.items())
    print(f"{rotulo} {valores}" + (f" {marca}" if marca else ""))


def meia_noite(dia):
    return timezone.make_aware(datetime.combine(dia, time.min), TZ)


def agrupar():
    dias = {}
    todas = set()
    for empresa, cid, data_iso, bid in SOBRAS:
        if any(type(pk) is not int or pk <= 0 for pk in (empresa, cid, bid)):
            raise ValueError("SOBRAS exige IDs inteiros positivos")
        dia = date.fromisoformat(data_iso)
        if dia.isoformat() != data_iso or not date(2026, 10, 21) <= dia <= date(2026, 11, 20):
            raise ValueError(f"Data fora do formato ou da comp 11 de 2026 (colab_id={cid})")
        empresa_dia, ids = dias.setdefault((cid, dia), (empresa, set()))
        if empresa != empresa_dia:
            raise ValueError(f"Empresas divergentes no mesmo dia (colab_id={cid} data={dia})")
        ids.add(bid)
        todas.add(bid)
    if not dias:
        raise ValueError("Preencha SOBRAS antes da execucao")
    return dias, todas


def batidas_do_recorte(cid, dia, sobras_dia):
    # Janela semiaberta entre as meias-noites locais de D-1 e D+2.
    # Leitura da linha bruta, incluindo retratadas e pendentes.
    # app/ponto/models.py:22-29,40-73; app/ponto/registro_batida.py:99-109
    batidas = list(Batida.objects.filter(
        colaborador_id=cid, timestamp__gte=meia_noite(dia - UM_DIA),
        timestamp__lt=meia_noite(dia + 2 * UM_DIA),
    ).order_by("timestamp", "pk").values(
        "id", "timestamp", "tipo", "origem", "canal", "tipo_registro",
        "is_intervalo", "escala_id", "pergunta_origem_id",
        "aprovada", "aprovada_por_id", "aprovada_em", "retratada_em",
    ))
    faltam = sobras_dia - {b["id"] for b in batidas}
    if faltam:
        raise ValueError(
            f"SOBRA fora da janela ou do colaborador "
            f"(colab_id={cid} data={dia} ids={sorted(faltam)})"
        )
    return batidas


def trilhas_de(batida_ids):
    # A ação csrf_batida usa o ID do colaborador no lugar do ID da batida.
    # app/core/views.py:1224-1242; app/core/models.py:285-307
    saida = defaultdict(list)
    for ato in (LogAuditoria.objects.filter(modelo="Batida", objeto_id__in=batida_ids)
                .exclude(acao="csrf_batida").order_by("criado_em", "pk")
                .values("id", "objeto_id", "acao", "usuario_id", "criado_em", "valor_depois")):
        saida[ato["objeto_id"]].append(ato)
    return saida


def retratacao_vigente(instante, atos):
    # Casa o carimbo vigente com a trilha. Motivo humano sai como ID.
    # Quando a justificativa só existe em texto livre, a trilha identifica
    # o ato e motivo_retratacao sai como [nao verificado].
    # app/ponto/registro_batida.py:231-239,270-296
    # app/ponto/services/retratar_humano.py:172-181
    # app/chamados/services/acoes_chamado.py:261-279
    # app/ponto/management/commands/passivo_disputa_retro.py:164-171
    if instante is None:
        return None, None
    for ato in reversed(atos):
        depois = ato["valor_depois"]
        if isinstance(depois, dict) and depois.get("retratada_em"):
            if datetime.fromisoformat(depois["retratada_em"]) == instante:
                motivo = depois.get("motivo_retratacao") or {}
                motivo_id = motivo.get("id")
                if type(motivo_id) is int:
                    return ato, motivo_id
                if depois.get("motivo_classe") == "passivo_s84_folga":
                    return ato, "passivo_s84_folga"
                return ato, NV
    return None, NV


def imprimir_batida(batida, atos, todas_as_sobras):
    bid = batida["id"]
    local = timezone.localtime(batida["timestamp"], TZ)
    retratacao, motivo = retratacao_vigente(batida["retratada_em"], atos)
    linha(
        "BAT", id=bid, data=local.date(), hora=local.strftime("%H:%M:%S"),
        marca="SOBRA" if bid in todas_as_sobras else "",
        **{k: v for k, v in batida.items() if k not in ("id", "timestamp")},
        motivo_retratacao=motivo,
        retratacao_trilha_id=retratacao["id"] if retratacao else None,
        retratada_por_id=retratacao["usuario_id"] if retratacao else None,
        retratacao_acao=retratacao["acao"] if retratacao else None,
    )
    for ato in atos:
        linha("BAT_AUD", batida_id=bid, trilha_id=ato["id"], acao=ato["acao"],
              usuario_id=ato["usuario_id"], quando=ato["criado_em"])


def imprimir_escala(cid, dia):
    # hi/hf são entrada/saída. hii/hfi e hii2/hfi2 são saída/volta das pausas.
    # Os instantes previstos já gravados permitem ler a virada de meia-noite.
    # app/escala/models.py:1656-1747,1843-1847
    # app/ponto/management/commands/gerar_celulas.py:36-70,88-113
    celula = (CelulaDia.objects.filter(colaborador_id=cid, data=dia)
              .select_related("escala_geradora__tipo_escala").first())
    celulas = {(cid, dia): celula} if celula is not None else {}
    if celula is None:
        linha("CEL", id=None, estado=NV)
    else:
        dna = celula.dna or {}
        linha(
            "CEL", id=celula.pk, origem=celula.origem, trabalha=celula.trabalha,
            estado={True: "trabalho", False: "folga", None: "indeterminado"}[celula.trabalha],
            marcos=dna.get("marcos"), instantes_previstos=dna.get("lampadas"),
            escala_geradora_id=celula.escala_geradora_id, tipo_escala_dna_id=dna.get("tipo_escala_id"),
            regime_dna=dna.get("tipo_ciclo"), gerada_em=celula.gerada_em,
            regenerada_em=celula.regenerada_em, regeneracoes=celula.regeneracoes,
            editada_em=celula.editada_em, editada_por_id=celula.editada_por_id,
        )
    # Leitor do vínculo do dia, com a precedência e a vigência da geradora.
    # app/escala/servico_jornada.py:15-66; app/escala/alimentacao.py:269-286
    # app/escala/models.py:117-121,801-804,871-877
    escala = escala_vigente(cid, dia, celulas=celulas)
    if escala is None:
        linha("VINCULO", id=None)
    else:
        template = escala.tipo_escala
        linha("VINCULO", id=escala.pk, posto_id=escala.posto_id, template_id=template.pk,
              template=str(template), regime=template.tipo_ciclo, inicio=escala.data_inicio,
              fim=escala.data_fim, ativa=escala.ativa)


def imprimir_ausencias(cid, dia, tipos):
    # Mostra cada lançamento e seu estado, inclusive rejeitados e cancelados.
    # efeito_tipo=abona é a regra do tipo. O estado do lançamento sai em status.
    # app/ponto/models.py:571-590,660-705,743-820
    # app/ponto/turnos.py:2548-2579; app/ferias/models.py:119-121,291-319
    em_aberto = [codigo for codigo, dados in tipos.items() if dados[1] == INDETERMINADO]
    ausencias = list(Ausencia.objects.filter(colaborador_id=cid, data__lte=dia).filter(
        Q(data=dia) | Q(data_fim__gte=dia) | Q(data_fim__isnull=True, tipo__in=em_aberto)
    ).order_by("data", "pk").values(
        "id", "tipo", "status", "data", "data_fim", "minutos", "hora_ini", "hora_fim",
        "aprovado_por_id", "aprovada_em", "agendamento_ferias_id", "chamado_origem_id",
    ))
    ferias = list(AgendamentoFerias.objects.filter(
        periodo_aquisitivo__colaborador_id=cid, data_inicio__lte=dia, data_fim__gte=dia,
    ).order_by("data_inicio", "pk").values("id", "status", "data_inicio", "data_fim"))
    linha("AUSENCIAS", ids=[a["id"] for a in ausencias], ferias_ids=[f["id"] for f in ferias])
    for ausencia in ausencias:
        efeito, _, medico = tipos.get(ausencia["tipo"], (NV, None, NV))
        linha("AUS", **ausencia, efeito_tipo=efeito, tipo_medico=medico)
    for agendamento in ferias:
        linha("FERIAS", **agendamento)
    return {a["chamado_origem_id"] for a in ausencias if a["chamado_origem_id"] is not None}


@lru_cache(maxsize=None)
def chamados_de(cid):
    return list(ChamadoColaborador.objects.filter(colaborador_id=cid).order_by("pk").values(
        "id", "status_local", "modulo_origem", "contexto_json", "celula_id", "celula__data",
        "justificativa_relacionada_id", "via_encerramento", "respondido_por_id", "resolvido_em",
    ))


def imprimir_chamados(cid, dia, batidas, chamados_ids):
    # Vínculos explícitos da pergunta: dia/célula, batida de origem e batida
    # plantada. Estado resume os atos lidos e os carimbos armazenados.
    # Visibilidade atual na fila [nao verificado].
    # app/chamados/models.py:2724-2760,3351-3370,3811-3838,4472-4515
    # app/chamados/juizes.py:1626-1628,1644-1653; app/ponto/models.py:40-44
    ids = {b["id"] for b in batidas}
    ids_texto = {str(pk) for pk in ids}
    origens = {b["pergunta_origem_id"] for b in batidas if b["pergunta_origem_id"] is not None}
    perguntas = (PerguntaDisputa.objects.filter(colaborador_id=cid).filter(
        Q(celula__data=dia) | Q(data_evento=dia, conversa=False)
        | Q(batida_origem_id__in=ids) | Q(pk__in=origens)
    ).select_related("disputa").prefetch_related("decisoes").order_by("pk"))
    for p in perguntas:
        lido = DecisaoPergunta.composta(p)
        respondida = lido.respondida_em is not None and bool((lido.resposta_colab or "").strip())
        estado = "respondida" if respondida else "pendente"
        if lido.materializada_em is not None:
            estado = "materializada"
        elif lido.validada_em is not None:
            estado = "validada"
        linha(
            "PERGUNTA", id=p.pk, estado=estado, motivo=p.motivo, data_evento=p.data_evento,
            celula_id=p.celula_id, batida_origem_id=p.batida_origem_id,
            batidas_plantadas_ids=[b["id"] for b in batidas if b["pergunta_origem_id"] == p.pk],
            disputa_id=p.disputa_id, disputa_fechada_em=p.disputa.fechada_em,
            chamado_pai_id=p.disputa.chamado_id, chamado_destino_id=p.chamado_destino_id,
            respondida=respondida, respondida_em=lido.respondida_em, validada_em=lido.validada_em,
            materializada_em=lido.materializada_em, via=lido.via_resolucao,
        )
        chamados_ids |= set(map(int, p.chamados_relacionados_ids or ()))
        chamados_ids |= {p.disputa.chamado_id, p.chamado_destino_id}

    # Ligações formais por justificativa, regularização ou ausência.
    # app/ponto/models.py:139-144,167-177,786-790
    # app/chamados/models.py:81-85,1923-1931,4658-4662,4687-4715
    justificativas = Justificativa.objects.filter(colaborador_id=cid).filter(
        Q(data_turno=dia) | Q(batida_entrada_id__in=ids) | Q(batida_gerada_id__in=ids)
    ).order_by("pk").values_list("pk", "chamado_origem_id")
    justificativas_ids = set()
    for jid, chamado_id in justificativas:
        justificativas_ids.add(jid)
        chamados_ids.add(chamado_id)
    itens = RegularizacaoItemBatida.objects.filter(regularizacao__colaborador_id=cid).filter(
        Q(data=dia) | Q(batida_entrada_id__in=ids)
        | Q(batida_saida_id__in=ids) | Q(batida_alvo_id__in=ids)
    )
    chamados_ids |= set(itens.order_by().values_list("regularizacao__chamado_id", flat=True))

    # Chamado por célula, data declarada ou batida no contexto. Seguimentos
    # mostram autoria e instante sem expor o conteúdo da conversa.
    # app/chamados/models.py:39-72,1835-1848
    # app/chamados/juizes.py:89-129; app/chamados/catalogo/modulos.py:219-225
    # app/chamados/services/acoes_chamado.py:244-248
    relacionados = []
    for ch in chamados_de(cid):
        ctx = ch["contexto_json"] or {}
        data_ctx = data_do_chamado(ch["modulo_origem"], ctx) or ctx.get("data_turno")
        batidas_ctx = {str(ctx.get("batida_id"))}
        batidas_ctx |= {str((o or {}).get("batida_id")) for o in ctx.get("ocorrencias") or []}
        if not (
            ch["id"] in chamados_ids or ch["justificativa_relacionada_id"] in justificativas_ids
            or ch["celula__data"] == dia or batidas_ctx & ids_texto
            or (ch["celula_id"] is None and str(data_ctx)[:10] == dia.isoformat())
        ):
            continue
        relacionados.append(ch["id"])
        linha("CHAMADO", id=ch["id"], estado=ch["status_local"], celula_id=ch["celula_id"],
              batidas_contexto_ids=sorted(map(int, batidas_ctx & ids_texto)),
              via_encerramento=ch["via_encerramento"], respondido_por_id=ch["respondido_por_id"],
              resolvido_em=ch["resolvido_em"])
    for resposta in (FollowupChamado.objects.filter(chamado_id__in=relacionados)
                     .order_by("criado_em", "pk")
                     .values("id", "chamado_id", "autor_tipo", "autor_user_id", "criado_em")):
        linha("CHAMADO_SEGUIMENTO", **resposta)


def main():
    if connection.schema_name != "juliani":
        raise RuntimeError("A sonda exige o schema juliani")
    dias, sobras = agrupar()

    # Leitura direta do catálogo para propagar falhas de consulta. O catálogo
    # da cópia é usado quando a consulta retorna zero tipos ativos.
    # app/ponto/catalogo/ausencias.py:86-105,240-281,430-435
    tipos = {codigo: (efeito, fim, medico) for codigo, efeito, fim, medico in
             TipoAusencia.objects.filter(ativo=True).order_by().values_list("codigo", "efeito", "fim", "medico")}
    if not tipos:
        tipos = {codigo: (dados[0], dados[5], dados[6]) for codigo, dados in CATALOGO.items()}
    empresas_cadastradas = dict(Colaborador.objects.filter(
        pk__in={cid for cid, _ in dias},
    ).order_by().values_list("pk", "empresa_id"))  # app/colaboradores/models.py:319-322

    total_batidas = 0
    for (cid, dia), (empresa, sobras_dia) in sorted(dias.items()):
        batidas = batidas_do_recorte(cid, dia, sobras_dia)
        trilha = trilhas_de([b["id"] for b in batidas])
        linha("DIA", colab_id=cid, data=dia, empresa=empresa,
              empresa_cadastro_id=empresas_cadastradas[cid], sobras_ids=sorted(sobras_dia))
        for batida in batidas:
            imprimir_batida(batida, trilha[batida["id"]], sobras)
        imprimir_escala(cid, dia)
        chamados_ids = imprimir_ausencias(cid, dia, tipos)
        imprimir_chamados(cid, dia, batidas, chamados_ids)
        total_batidas += len(batidas)

    # batidas conta linhas BAT emitidas, inclusive em janelas sobrepostas.
    print(f"FIM dias={len(dias)} batidas={total_batidas}")


main()
```

## 27/11 10:02 · sessao b89a8e9ccbee · PAR-PELO-TURNO · verificador · origem=Code

## Procedentes

1. **O mesmo PK ainda muda de dia.** Com DNA `21:30–05:42`, `E 23/10 01:30` e `S 24/10 00:00`, a casca e o lote publicam a entrada em **22/10** na consulta de 21/10 e em **23/10** na consulta de 22/10. A segunda consulta alcança a saída e reclassifica a entrada. Há divergência mesmo com todas as marcas carregadas. Com A noturno até 21/10, B diurno a partir de 22/10 e `E 21/10 21:30`, `S 22/10 08:00`, `S 22/10 20:00`, a última saída recebe **22/10** na casca e na consulta individual, mas **21/10** na mesma janela com `turnos=`. Dois segmentos reclamam o PK e a última atribuição vence. O RED de `r.txt` passou na geometria original de um vínculo, assim como o controle sintético de col297. A invariância exigida pela L-085 segue aberta. `app/docs/LEIS.md:117` · `app/ponto/turnos.py:287` · `app/ponto/turnos.py:1998` · `app/ponto/turnos.py:3037` · `app/ponto/turnos.py:3060` · `app/ponto/turnos.py:3082` · `app/ponto/tests/test_par_pelo_turno_c1v6.py:57` · `app/ponto/tests/test_par_pelo_turno_c1v6.py:73`

2. **A pré-passagem da pausa ainda perde o relógio da entrada.** O caso anterior da `E` interna passou com flag gravada e com `Xi`. O novo contraexemplo usa marcos de borda `19:00–07:00`, `E 20/10 19:00`, `S 19:00:30`, `S 21/10 01:00`, `E 02:00`, `S 07:00`. Sem marcos de borda, a sonda fecha `19:00→07:00` com 60 min de pausa. Com os marcos, ela produz `19:00→01:00` e `02:00→07:00`, sem pausa. A pré-passagem com bordas zera `limite` pela saída de 30 s, enquanto o laço principal conserva a entrada. O RED da saída degenerada só chama o ramo sem bordas. `app/ponto/turnos.py:435` · `app/ponto/turnos.py:447` · `app/ponto/turnos.py:508` · `app/ponto/turnos.py:1501` · `app/ponto/tests/test_par_pelo_turno_c1v6.py:82` · `app/ponto/tests/test_par_pelo_turno_c1v6.py:116`

3. **O item 2 do aval ainda falha por regra.** A sonda original com só `E 22/10 12:00` deu **0,00 h** nos dois braços. Em `21/09–20/10`, com previsão comercial e sem ausência, usei `E 21/10 01:30`, `S 22/10 00:00` com papel `Xi` e uma batida posterior em `23/10 08:00`. A casca dispõe do fato posterior da O95, fecha o par e data ambas as marcas em **21/10**. A projeção nova da folha passa `ultima_batida=None`, mantém o turno aberto e atribui ambas a **20/10**. Com zero períodos no motor, os trechos reais do fechamento deram **0,00 h** sem as marcas e **−193,60 h** com elas. Esse saldo alimenta o `FechamentoMensal` e o banco do `DiaPago`. `app/ponto/tests/par_pelo_turno_c1v6_verificacao.py:204` · `app/ponto/turnos.py:1172` · `app/ponto/turnos.py:1186` · `app/ponto/turnos.py:1931` · `app/ponto/services/fechamento.py:165` · `app/ponto/services/fechamento.py:679` · `app/ponto/services/fechamento.py:1123` · `app/ponto/services/fechamento.py:1199` · `app/ponto/services/fechamento.py:1227`

4. **A segunda virada continua dependente do cadastro do dia da saída.** Para `E 20/10 23:30 → S 22/10 01:30`, o juiz devolve um par fechado de 26 h. A sonda de `_turno_noturno_completo` devolveu `True` com o vínculo noturno e `False` com vínculo diurno ou sem vínculo em 22/10. A guarda rejeita o caso antes de consultar o par. O teste usa o mesmo vínculo nas duas datas e simula o lote com a própria casca. A transferência das duas pontas para a fatia de A passou no caso anterior de 26 h, mas o teste usa dois vínculos sem `tipo_escala`, o mesmo DNA e ata vazia. A fatia continua consumindo o mapa sujeito à dupla atribuição do item 1. `app/escala/servico_jornada.py:230` · `app/escala/servico_jornada.py:245` · `app/escala/servico_jornada.py:354` · `app/ponto/tests/test_par_pelo_turno_c1v6.py:125` · `app/ponto/tests/test_par_pelo_turno_c1v6.py:149` · `app/ponto/services/fechamento.py:284`

5. **O item 1 do aval, 4b, continua reservado e produz diferença de dinheiro.** Com `E 19/10 22:00 → S 20/10 22:00` e `E 21/10 03:00 → S 21/10 11:00`, o juiz data o segundo par em **21/10**. No mesmo `Motor12x36`, variar só a carga da antiga borda do PDF para `ini−4` fez **8,00 h virarem 0,00 h** na janela iniciada em 21/10. A primeira jornada inteira passa a chegar ao motor, que aplica a herança pelo vão de 5 h e exclui o segundo par como dia 20. A folha já recebe a carga ampliada. O PDF livre passou a recebê-la nesta volta e soma a saída desse motor. A outra pendência reservada, 3c, também permanece. Para fim em 20/10, a testemunha da folha pede julgamento até 21/10, carrega até 23/10 inclusive e só recorta a comparação depois. Uma marca em `23/10 00:00` entra nessa apuração e fica fora da carga da competência. `app/docs/par_pelo_turno_c1v6_evidencia.txt:137` · `app/core/juizes.py:216` · `app/ponto/motor_calculo_v2.py:2516` · `app/ponto/motor_calculo_v2.py:2542` · `app/ponto/services/fechamento.py:655` · `app/relatorios/pdf_espelho.py:281` · `app/relatorios/pdf_espelho.py:495` · `app/folha/porta_export.py:77` · `app/folha/porta_export.py:106` · `app/folha/porta_export.py:114`

## Escritores e leitores

| Valor | Onde nasce e onde o contrato diverge |
|---|---|
| `janela_com_pad` e `batidas_apuraveis_na_janela` | `turnos.py` escreve `ini−4..fim+2`, por dias locais completos, do primeiro ao último microssegundo inclusive. Casca e lote usam filtros de data local. Grade, carga de perguntas, fechamento, autoridade do espelho e PDF leem a janela. O fechamento usa o helper também na passada de órfãos. A carga de perguntas escreve a borda equivalente como `< 00:00` do dia seguinte, mas recorta de um lote já carregado. A divergência demonstrada está no período pedido pela testemunha da folha e no provedor da O95. `app/ponto/turnos.py:1869` · `app/ponto/turnos.py:1895` · `app/ponto/turnos.py:2990` · `app/ponto/turnos.py:3001` · `app/ponto/turnos.py:3149` · `app/escala/utils.py:1352` · `app/chamados/services/carga_pergunta_viva.py:174` · `app/ponto/services/fechamento.py:831` · `app/ponto/services/espelho.py:1031` · `app/relatorios/pdf_espelho.py:281` |
| Dia por PK e dia do período | `_dia_das_batidas_puro` escreve o dia do turno reclamante e completa as demais marcas pelo dia civil. Espelho e app filtram por PK, painel, férias e vigia consultam PKs específicos, e disputa e fio chegam pela consulta individual. Na tela, a soma de horas passou a ler o dia publicado pelo motor, mas `turnos` e `turnos_abertos` ainda filtram pela entrada civil. No próprio caso 3d, `E 21/09 00:00 → S 07:50` atribuído a 20/09 soma **7 h 50 min** no recorte até dia 20 e acrescenta **zero** a `turnos`. `app/ponto/turnos.py:3070` · `app/ponto/services/espelho.py:435` · `app/ponto/services/espelho.py:451` · `app/ponto/services/espelho.py:588` · `app/api/views.py:1444` · `app/api/views.py:1461` · `app/core/services/painel_op.py:330` · `app/ferias/management/commands/listar_ferias_com_batida.py:54` · `app/ponto/management/commands/vigia_plantio_overnight.py:87` · `app/chamados/services/disputa_emissao.py:283` · `app/chamados/services/fio.py:1827` · `app/relatorios/tests/test_par_pelo_turno_c1v6_pdf.py:103` |
| `dias_inconsistentes` | A tela incrementa uma vez por dia inconsistente. O PDF reconta as linhas. `folha_manda` pode sobrescrever ambos com `FechamentoMensal.inconsistencias`, que conta anomalias e acrescenta 1 por `sem_movimento`. Com zero anomalias e nenhuma linha inconsistente, o contador de dias vale **0** e o fechamento sem movimento publica **1**. O badge, o app e a comparação da porta leem a mesma chave com esses dois significados. `app/ponto/services/espelho.py:670` · `app/ponto/services/espelho.py:685` · `app/relatorios/pdf_espelho.py:604` · `app/ponto/services/fechamento.py:1119` · `app/relatorios/cartao_pela_celula.py:113` · `app/relatorios/cartao_pela_celula.py:346` · `app/templates/ponto/espelho.html:67` · `app/api/views.py:1434` · `app/folha/porta_export.py:502` |
| `pdf_espelho` | O coletor lê as linhas de `espelho_do_colab`, mas monta insumos e roda `apurar_por_fatia` ou `calcular_mes` em paralelo. Em período livre, publica o próprio saldo do motor. O mesmo coletor alimenta a geração do PDF, a testemunha de exportação e a pauta de dias em aberto. Com `agora=25/11` e pedido `26–27/11`, o `max` faz a apuração começar e terminar em **26/11**, enquanto a tela consultada para as linhas termina em **24/11**. É uma divergência de teto usando o mesmo `agora`. O delta fornecido tem **+0** cláusulas `except`. `app/relatorios/pdf_espelho.py:277` · `app/relatorios/pdf_espelho.py:410` · `app/relatorios/pdf_espelho.py:426` · `app/relatorios/pdf_espelho.py:524` · `app/relatorios/pdf_espelho.py:589` · `app/relatorios/pdf_espelho.py:647` · `app/relatorios/pdf_espelho.py:1041` · `app/folha/porta_export.py:502` · `app/ponto/services/dias_em_aberto.py:112` · `app/docs/par_pelo_turno_c1v6_evidencia.txt:149` |

## O que os testes deixam passar [nao verificado]

- **c1v6.** Ata vazia derrubou o teste novo, fechando aquele mutante de `r.txt`. Encurtar a carga do PDF, restaurar o recorte civil da soma e usar `not batidas` também derrubaram as respectivas sondas. Em contrapartida, mudei só `dia <= _data_fim` para `dia < _data_fim` no `sem_movimento`. Os **95 testes e sondas em memória** passaram. Uma única marca apurável atribuída ao dia 20, sem ausência, passaria a ser tratada como mês sem movimento. Os dois braços do 4c exercitam apenas o caso sem fato na competência. `app/ponto/tests/test_par_pelo_turno_c1v6.py:182` · `app/ponto/tests/par_pelo_turno_c1v6_verificacao.py:266` · `app/ponto/tests/par_pelo_turno_c1v6_verificacao.py:319` · `app/ponto/services/fechamento.py:679` · `app/relatorios/tests/test_par_pelo_turno_c1v6_pdf.py:120`
- **c1v7.** O AST distingue os dois nomes por igualdade e contou uma chamada do embrulho na linha 281. Os cinco testes estruturais passaram em memória. Uma segunda chamada via `carregar = batidas_apuraveis_na_janela; carregar(...)` deixou o censo idêntico. A chamada real a `apurar_por_fatia` também está fora de `AUTORIDADES`. O verde do selo permite a derivação paralela do coletor. `app/relatorios/tests/test_pdf_nao_calcula.py:34` · `app/relatorios/tests/test_pdf_nao_calcula.py:63` · `app/relatorios/tests/test_pdf_nao_calcula.py:96` · `app/relatorios/pdf_espelho.py:281` · `app/relatorios/pdf_espelho.py:412`
- **c1v8.** Comparei os literais via AST. A fixture copia as nove chaves e valores de `espelho.py`, incluindo `dias_inconsistentes`. Ela corrige o `KeyError` da montagem manual e preserva a asserção de horas. Como injeta o próprio resumo e chama `montar_dias`, o 3d deixa passar a remoção de `dias_inconsistentes` do inicializador de produção e a remoção do incremento. A comparação da fixture com o contrato atual é exata, mas o teste não protege esse contrato contra mudança futura. `app/relatorios/tests/test_par_pelo_turno_c1v6_pdf.py:100` · `app/relatorios/tests/test_par_pelo_turno_c1v6_pdf.py:113` · `app/ponto/services/espelho.py:685` · `app/ponto/services/espelho.py:1236`

A execução Django parou no import de `django.core`. O `Ran 356 OK`, a identidade Git de HEAD e das bases, o efeito no gravado e no TXT, e o laudo dos 29 casos por rubrica ficam **[nao verificado]**. Para col155/08-11, a cópia só oferece o ensaio sintético de **782 min**, teto de **732 min** e uma C9. `app/manage.py:11` · `app/docs/BACKLOG.md:2027` · `app/ponto/tests/test_par_pelo_turno_c1v6.py:195`

**REPROVADO — PK muda de dia e de dono (`app/ponto/turnos.py:3085`), pausa perde a entrada (`app/ponto/turnos.py:508`), 4c perde a prova da O95 e muda o saldo (`app/ponto/services/fechamento.py:165`), guarda noturna rejeita par fechado (`app/escala/servico_jornada.py:230`), 3c amplia a janela (`app/folha/porta_export.py:77`), 4b perde 8 h (`app/ponto/motor_calculo_v2.py:2521`), PDF recalcula e usa outro recorte (`app/relatorios/pdf_espelho.py:412`), resumo mistura dias e ocorrências (`app/relatorios/cartao_pela_celula.py:113`) e os testes deixam passar borda exclusiva, chamada indireta e perda da chave de produção (`app/ponto/services/fechamento.py:679` · `app/relatorios/tests/test_pdf_nao_calcula.py:77` · `app/relatorios/tests/test_par_pelo_turno_c1v6_pdf.py:113`).**

## 27/11 09:52 · sessao ca7d3c808b96 · - · - · origem=Ronald (tmux)

- **R4 e R5.** O cenário de intervalo permite exercitar origem aberta e marco efetivo entregue na virada do corte. Na base, o repasse grava os IDs de ambos os itens no agregado antes de tentar as perguntas. O nascimento distingue alteração de inserção e só aplica a guarda B2 à alteração. O teste pode observar `_nascer`, o dia efetivo e a tentativa de `INSERT`, além da releitura pública de `cobrar_dia`. `chamados/tests/test_fio_intervalo.py:45`, `ponto/janelas.py:60`, `folha/tests/test_export_vigente.py:33`, `chamados/services/disputa_emissao.py:1117`, `chamados/models.py:3192`, `chamados/models.py:3333`, `chamados/models.py:3847`, `chamados/services/cobrar_dia.py:45`.
- **R3.** O callback autônomo de silêncio de isentos já tem fixture com callback real. Adicionar TXT vigente e cobrar o registro do silêncio detecta uma extensão indevida de `_K6`. O beco tem cerca B2 própria e é outro cenário. `chamados/signals.py:285`, `chamados/tests/test_varredura_isentos_evento.py:43`, `chamados/services/silencio_isentos.py:34`, `chamados/services/becos.py:63`.
- **Volta 3.** O template imprime só a hora, o PDF consulta HH:MM e o supra só imprime a lista de horas. Os pontos observáveis de transporte são a ata em curso no emissor, `grade_da_celula` durante `_marcos_faltantes` e `grade_do_fechamento` na folha. `templates/ponto/espelho.html:242`, `relatorios/pdf_espelho.py:472`, `relatorios/pdf_espelho.py:607`, `ponto/supra_juiz.py:527`, `ponto/services/cartorio.py:1505`, `chamados/services/declaracao_texto.py:101`, `folha/export.py:329`.

Execução dos cenários [nao verificado].

_8625 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
