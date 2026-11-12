# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 13:17** · publicado em **12/11/2026 13:18** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 13:17 · sessao f268da51b608 · S139-RESPEITA-A-RESPOSTA · sonda · origem=Code

Referências `DADO:N` contam as linhas tabuladas do bloco fornecido, começando em `S R_COLUNAS` como linha 1.

1. **TOTAL**

| Medida | Número | Fonte |
|---|---:|---|
| perguntas | 152 | DADO:317 (`S TOTAL`) |
| relações | 152 | DADO:317 |
| hora_diferente | 62 | DADO:317 |
| acima30 | 31 | DADO:317 |
| delta_nv | 0 | DADO:317 |
| COLABS | 52 | DADO:318 |
| SEM_DIA | 0 | DADO:320 |
| FORA_RECORTE_CONTEXTUAL | 0 | DADO:321 |
| TOTAL_ERROS | 0 | DADO:322 |

Nenhum erro reportado torna os números pedidos impossíveis de afirmar; há campos de snapshot não verificados, discriminados abaixo. (DADO:322; DADO:15–137, filtrando `delta_relogio_min > 30`.)

2. **Das 31 perguntas acima30**

| Marco | DENTRO dos 90 min | FORA | NÃO_VERIFICADO |
|---|---:|---:|---:|
| Cadastrado | **23** | **8** | **0** |
| Snapshot | **7** | **1** | **23** |

Fontes da contagem pelo cadastro:

- **23 dentro:** DADO:15–17, 32–34, 42–43, 56, 59, 71–72, 82, 101–103, 105–109, 115, 130.
- **8 fora:** DADO:51, 54, 57, 113, 122, 128–129, 137.
- **0 não verificado:** todas essas 31 linhas têm classificação cadastral preenchida. (Mesmas linhas.)

Pelo snapshot, as **7 dentro** são as pks **49876, 50079, 51892, 52250, 53443, 53847 e 54585**; a **1 fora** é a pk **54584**; as outras **23** têm `NAO_VERIFICADO`. (DADO:33–34, 71, 82, 103, 115, 129–130; demais linhas acima30 enumeradas acima.)

**Retratadas: 0** entre as 31 perguntas acima30. (DADO:15–137, mesmas 31 linhas filtradas.)

3. **Colaboradores e dia-colab distintos, considerando somente acima30**

| Classificação pelo cadastro | Colabs | Dia-colab | Fonte |
|---|---:|---:|---|
| Ao menos uma pergunta dentro | **9** | **15** | DADO:214, 216, 229, 248–249, 251, 254–255, 282, 290, 312–316 |
| Somente perguntas fora | **2** | **6** | DADO:169, 183; 230–232, 256, 283, 289 |

Os **2 colaboradores somente fora** são **col301 e col707**. **col873 e col909** têm perguntas acima30 dentro e fora em dias diferentes: entram nos 9 colaboradores com alguma dentro, enquanto seus dias somente fora entram nos 6 dia-colab. (DADO:169, 183, 282–283, 289–290.)

4. **Pergunta pk 50833**

Sim: **col707**, batida pk **152915**, dia de turno **27/10/2026**; resposta **11:10**, batida local **2026-10-27T16:00:00-03:00**, marco cadastrado **16:00**, delta **290 minutos**, `dentro90_cadastro=0`: **FORA**. (DADO:51.)

5. **Distribuição do delta entre acima30**

Faixas sem sobreposição, incluindo o limite superior:

| Faixa em minutos | Perguntas | Fonte |
|---|---:|---|
| 30 < delta ≤ 60 | **22** | Linhas dentro do cadastro listadas no item 2, exceto DADO:82 |
| 60 < delta ≤ 120 | **3** | DADO:82 — 63 min; DADO:54, 57 — 120 min |
| 120 < delta ≤ 240 | **5** | DADO:113, 122, 128–129, 137 — 180 min |
| delta > 240 | **1** | DADO:51 — 290 min |

## 12/11 13:15 · sessao 50160a1d8a3a · - · - · origem=Ronald (tmux)

Conferência dos itens 2, 3 e 5:

- **Cadastro:** 23 DENTRO, 8 FORA, 0 NÃO_VERIFICADO. DENTRO sustentado pelas linhas R com pergunta_pk 49126, 49246, 49247, 49875, 49876, 50079, 50503, 50504, 51108, 51132, 51892, 51923, 52250, 53440, 53442, 53443, 53446, 53449, 53450, 53453, 53454, 53847, 54585. FORA sustentado pelas linhas R com pergunta_pk 50833, 51086, 51111, 53641, 54017, 54583, 54584, 55094.
- **Snapshot:** 7 DENTRO — linhas R com pergunta_pk 49876, 50079, 51892, 52250, 53443, 53847, 54585; 1 FORA — linha R com pergunta_pk 54584; 23 NÃO_VERIFICADO — demais linhas acima30 enumeradas no primeiro item.
- **Retratadas:** 0 nas 31 linhas acima30 enumeradas; todas têm retratada=0.

**Universos distintos entre perguntas acima30:**

| Cadastro | Colabs | Dia-colab |
|---|---:|---:|
| Ao menos uma DENTRO | 9 | 15 |
| Somente FORA | 2 | 6 |

Suporte para DENTRO, por linhas R acima30 e respectivas linhas S COLAB_DIA:

- col040: 2026-11-04.
- col055: 2026-10-25.
- col263: 2026-10-31.
- col502: 2026-10-24 e 2026-10-28.
- col516: 2026-10-22.
- col569: 2026-10-23 e 2026-10-30.
- col873: 2026-10-26.
- col909: 2026-11-05.
- col993: 2026-10-24, 2026-10-26, 2026-10-28, 2026-10-30 e 2026-11-01.

Suporte para SOMENTE FORA por colaborador: col707 (R 50833) e col301 (R 53641, 54017, 55094). col873 e col909 têm acima30 DENTRO em outros dias, portanto não entram como colaboradores “só fora” (R 50503, 50504, 54585).

Os 6 dia-colab somente FORA são col707/2026-10-27; col873/2026-10-28; col301/2026-11-03, 2026-11-04 e 2026-11-07; col909/2026-11-03 (linhas R 50833, 51086, 51111, 53641, 54017, 55094, 54583, 54584).

**Faixas sem sobreposição, com limite superior incluído:**

| Faixa | Perguntas | Suporte |
|---|---:|---|
| 30 < delta ≤ 60 | 22 | As linhas DENTRO acima, exceto R 52250 |
| 60 < delta ≤ 120 | 3 | R 52250=63; R 51086 e 51111=120 |
| 120 < delta ≤ 240 | 5 | R 53641, 54017, 54583, 54584, 55094=180 |
| delta > 240 | 1 | R 50833=290 |

## 12/11 13:10 · sessao e93ae91a7d8b · S139-RESPEITA-A-RESPOSTA · censo · origem=Code

**A. A S139 troca a hora antes da escrita; os leitores do slot herdam essa troca.** O trecho executável começa com `resposta_colab` e, para `intervalo_saida`/`intervalo_volta`, substitui-a pela hora de `_marco_faltante`, quando encontrada. Não há nesse trecho uma condição adicional sobre “intervalo fixo” nem uma verificação do envelope. (`app/chamados/services/materializacao.py:409-426`)

As referências `:342` e `:712` indicadas na pergunta são de **`chamados/juizes.py`**, onde estão os comentários sobre essa derivação. (`app/chamados/juizes.py:342`; `app/chamados/juizes.py:712`)

| Papel | Quem faz | Evidência |
|---|---|---|
| Resolve a hora e o tipo | `_ts_tipo_do_plantio` | `app/chamados/services/materializacao.py:333-428` |
| Busca o marco cobrado | `_marco_faltante` → `entrada_de_marco`: motivo primeiro; fallback por tipo somente no formato legado | `app/chamados/juizes.py:523-580` |
| Solicita o plantio | `_materializar_resposta_hora`, depois de derivar e procurar reuso | `app/chamados/services/materializacao.py:852-877` |
| Grava `Batida.timestamp` | `registrar_batida` → `Batida.objects.create` | `app/chamados/services/materializacao.py:933-940`; `app/ponto/registro_batida.py:157-159` |
| Grava a relação pergunta→batida | `anotar(... pergunta_origem=p)`, tanto para nova quanto para reusada | `app/chamados/services/materializacao.py:947-956` |
| Grava resposta e instante derivado na trilha | `_log_auditoria_materializacao`, ação `materializar_batida` | `app/chamados/services/materializacao.py:957-978` |

O campo persistido é **`Batida.pergunta_origem`**, reverso **`PerguntaDisputa.batidas_plantadas`**; não confundir com `PerguntaDisputa.batida_origem`, que identifica a batida sobre a qual outra família de perguntas pergunta. (`app/ponto/models.py:40-44`; `app/chamados/models.py:2437-2442`)

Os leitores diretos da derivação são estes:

| Leitor | Consequência |
|---|---|
| `estado_aplicacao_pergunta` | Calcula aplicação, reuso, paridade e horário anunciado na interface pelo instante derivado. (`app/chamados/services/materializacao.py:450-465`) |
| `_slot_esperado` | **Hoje chama o mesmo derivador**; o docstring “Reproduz” é anterior ao corpo que delega. (`app/chamados/juizes.py:342`; `app/chamados/juizes.py:369-374`) |
| `_pergunta_materializada_de_fato` | Confere as batidas vinculadas contra esse slot; a conferência aceita a janela de ±20 minutos. (`app/chamados/juizes.py:752-761`; `app/chamados/juizes.py:250-279`) |
| `CargaDaVida` | Prepara/cacheia os slots usados pelo juiz. (`app/chamados/services/carga_pergunta_viva.py:113-126`) |
| `_explicar_validada` | Busca batida perto do slot e escreve “validada como” usando sua hora. (`app/chamados/services/explicador.py:29-42`; `app/chamados/services/explicador.py:58-60`) |
| `classe_do_lote` | Confere conflito perto do slot que o plantio usaria. (`app/chamados/services/validacao.py:869-876`) |
| `vigia_carimbo.classificar` | Classifica slot não derivável, ausência de plantio ou plantio fora do slot. (`app/chamados/services/vigia_carimbo.py:105-113`) |

O juiz de materialização também alimenta o veredito lavrado, o raio-X, o explicador e a conferência posterior ao plantio. (`app/chamados/veredito_lavrado.py:263`; `app/chamados/services/raiox.py:187`; `app/chamados/services/explicador.py:93`; `app/chamados/services/materializacao.py:1239`)

**Outra cópia executável da substituição resposta→marco não foi localizada neste censo; exaustividade sobre todo consumidor indireto de `timestamp`: [nao verificado].** Os caminhos acima compartilham o derivador. (`app/chamados/services/materializacao.py:450`; `app/chamados/services/materializacao.py:852`; `app/chamados/juizes.py:372`)

No esmeril semântico:

- `resposta_colab` conserva a declaração; a substituição acontece na variável `_hora_fonte`, não na resposta persistida. A trilha grava os dois valores separadamente. (`app/chamados/services/materializacao.py:412-419`; `app/chamados/services/materializacao.py:969-977`)
- `Batida.timestamp` recebe o instante materializado, que nesse ramo pode ser o marco. O pareador consome esse campo como instante da batida. (`app/ponto/registro_batida.py:157-159`; `app/ponto/turnos.py:1217`; `app/ponto/turnos.py:1357`)
- Há textos que chamam esse slot de “hora respondida” ou “slot da resposta”, embora ele possa conter a hora do marco. (`app/chamados/juizes.py:663-667`; `app/chamados/services/explicador.py:31`; `app/chamados/services/vigia_carimbo.py:17-20`)
- `delta_da_resposta` e `motivo_implausivel_validacao` preservam a distinção: leem resposta e marco separadamente. (`app/chamados/juizes.py:1846-1868`; `app/chamados/juizes.py:1972-1983`)
- A reconciliação da grade reconhece a dependência: evita trocar o marco sob resposta viva já dada porque a materialização lê `marcos_faltantes`. (`app/ponto/management/commands/reconciliar_grade.py:437-440`; `app/ponto/management/commands/reconciliar_grade.py:460-462`)

**B. O caso protegido era uma volta de intervalo declarada no horário da saída do intervalo.** O teste fundador descreve volta esperada às `03:00`, resposta `02:00`, e exige impedir `E@02:00` no slot da saída. A fixture valida essa resposta e exige `E@03:00`, expressamente diferente de `02:00`. (`app/chamados/tests/test_s139.py:15-18`; `app/chamados/tests/test_s139.py:40-56`)

“Marco oposto” significa, nesse caso, jornada `19→07`, intervalo `02:00/S→03:00/E`: uma `E` próxima das `02:00` ocupa temporalmente o marco `S`. Outro teste reproduz a figura com `E@02:05` e exige a classificação `batida_retro_invertida`. (`app/chamados/tests/test_auditar_disputas_padroes.py:117-150`)

O dano diretamente demonstrado é **tipo `E` no horário do marco `S`**. Consequência financeira ou quebra específica de pareamento pela retirada da S139: **[nao verificado]**.

A O306 manteve explicitamente a S139: resposta `13:05` continua virando `13:00` quando esse é o marco do próprio motivo. Corrigiu outra falha: receber o marco de um motivo vizinho, deixando o marco perguntado vazio. (`app/chamados/tests/test_o306_marco_do_motivo_nao_e_do_vizinho.py:146-153`; `app/chamados/juizes.py:555-575`)

Commit, autoria, data e corte específico do nascimento da S139 por `git log/blame`: **[nao verificado]**. As consultas Git falharam porque esta cópia não foi reconhecida como repositório. **[nao verificado]** A L-032 e a L-099 locais não identificam o nascimento da S139; estabelecem, respectivamente, reconhecimento do intervalo dentro do envelope e coerência com o cadastro existente. (`app/docs/LEIS.md:80`; `app/docs/LEIS.md:122`)

**C. Os 90 minutos estão dentro de `parear_turnos`; não existe uma função única pronta com a assinatura perguntada.** O literal aparece no predicado interno `_absorvivel`, na guarda de continuidade e na absorção por marco. (`app/ponto/turnos.py:1251`; `app/ponto/turnos.py:1352`; `app/ponto/turnos.py:1358`)

A função reutilizável é **`_dist_marco(ts, marcos)`**: devolve a menor distância local em minutos, considerando dia anterior, mesmo dia e seguinte. Não recebe colaborador, não resolve o marco e não retorna um booleano. (`app/ponto/turnos.py:326-340`)

A composição disponível é:

| Pergunta | Código existente |
|---|---|
| Qual vínculo responde pelo colaborador/dia? | `escala_vigente` → `vinculo_do_dia`. (`app/escala/servico_jornada.py:15-66`) |
| Quais os marcos cadastrados desse dia? | `ec.marcos_do_dia`: DNA congelado, com fallback para template. (`app/escala/models.py:1098-1133`) |
| Qual marco corresponde ao motivo? | `MARCO_DA_PERGUNTA`: saída→`hii`, volta→`hfi`. (`app/chamados/juizes.py:922-926`) |
| Está no raio de 90 minutos desse marco? | `_dist_marco(...) <= 90`. (`app/ponto/turnos.py:326-340`; `app/ponto/turnos.py:1251`) |

**Estar nesse raio não prova absorção pelo pareador.** A absorção também compara a proximidade das bordas e depende do turno corrente e de seu teto de continuidade. O pareador ainda refina marcos pelo DNA na data civil local da batida. (`app/ponto/turnos.py:1229-1251`; `app/ponto/turnos.py:1349-1370`)

`intervalo_cadastrado` responde a duração cadastrada; não é a função de pertencimento ao envelope. (`app/ponto/juiz_batida.py:106-107`)

**D. Script para colar no shell Django, dentro do envelope de leitura.** Ele usa a **relação persistida**, incluindo reuso, sem consultar a trilha. Essa relação é escrita nos dois caminhos; logo, sua contagem não significa “batidas novas criadas”. Também pode ser reassociada a outra pergunta pelo escritor. (`app/chamados/services/materializacao.py:865-876`; `app/chamados/services/materializacao.py:952-956`)

O script imprime separadamente:

- raio do marco **cadastrado** do colaborador/dia;
- raio do marco **cobrado no snapshot** do chamado;
- absorção completa pelo pareador como `NAO_VERIFICADO`, pelas dependências descritas em C. (`app/escala/models.py:1098-1133`; `app/chamados/juizes.py:523-580`; `app/ponto/turnos.py:1349-1370`)

A diferença é a menor distância entre horas locais, inclusive pela meia-noite, calculada por `_dist_marco`; não atribui uma data inventada à resposta `HH:MM`. (`app/ponto/turnos.py:326-340`; `app/chamados/models.py:2448`)

`S total` conta perguntas distintas; `relacoes` conta batidas vinculadas. Havendo várias batidas para uma pergunta, `acima30` significa que pelo menos uma ultrapassou 30 minutos. Isso fica explícito no código abaixo. Execução Django e números resultantes: **[nao verificado]**.

```python
from collections import defaultdict, Counter
from datetime import date, datetime, time
from django.db import connection
from django.db.models import Q
from django.utils import timezone
from chamados.juizes import MARCO_DA_PERGUNTA, _marco_faltante
from escala.servico_jornada import escala_vigente
from ponto.models import Batida
from ponto.turnos import _dist_marco

INICIO = date(2026, 10, 21)
# Raio existente: ponto/turnos.py:1251,1358.
RAIO_MIN = 90
TOTAL_ERROS = 0
cache = {}
grupos = defaultdict(lambda: defaultdict(set))
links_por_pergunta = Counter()
fora_recorte = set()
sem_dia = set()

def emitir(*campos):
    print("\t".join(str(x) for x in campos))

def erro(etapa, pk, exc):
    global TOTAL_ERROS
    TOTAL_ERROS += 1
    # Nunca imprimir mensagem da excecao, objetos ou resposta bruta.
    emitir("S", "ERRO", etapa, pk, type(exc).__name__)

def tentar(etapa, pk, func):
    try:
        return func()
    except Exception as exc:
        erro(etapa, pk, exc)
        return None

def apenas_select(execute, sql, params, many, context):
    if not str(sql).lstrip().upper().startswith("SELECT"):
        raise RuntimeError("SQL_nao_SELECT")
    return execute(sql, params, many, context)

def hora(texto):
    hh, mm = texto.strip().split(":")
    return time(int(hh), int(mm))

def marco_cadastro(cid, dia, chave):
    ec = escala_vigente(cid, dia)
    if ec is None:
        return None
    # Contrato: escala/models.py:1098-1133.
    return dict(zip(("hi", "hf", "hii", "hfi"),
                    ec.marcos_do_dia(dia)))[chave]

def raio(resp, marco, dia):
    if resp is None or marco is None:
        return None
    ts = timezone.make_aware(datetime.combine(dia, resp))
    return _dist_marco(ts, [marco]) <= RAIO_MIN

def preparar(p):
    if p.pk in cache:
        return cache[p.pk]
    dia = p.data_evento
    if dia is None:
        texto = ((p.disputa.contexto or {}).get(p.motivo) or {}).get("data")
        dia = tentar("dia_contexto", p.pk, lambda: date.fromisoformat(texto))
    if dia is None:
        sem_dia.add(p.pk)
        cache[p.pk] = None
        return None
    if dia < INICIO:
        fora_recorte.add(p.pk)
        cache[p.pk] = None
        return None
    cid = p.disputa.colaborador_id
    tipo, chave = MARCO_DA_PERGUNTA[p.motivo]
    resp = tentar("hora_resposta", p.pk, lambda: hora(p.resposta_colab))
    mc = tentar("marco_cadastro", p.pk,
                lambda: marco_cadastro(cid, dia, chave))
    def snapshot():
        texto = _marco_faltante(p.chamado_destino, p.motivo, tipo)
        return hora(texto) if texto else None
    ms = tentar("marco_snapshot", p.pk, snapshot)
    dc = tentar("raio_cadastro", p.pk, lambda: raio(resp, mc, dia))
    ds = tentar("raio_snapshot", p.pk, lambda: raio(resp, ms, dia))
    cache[p.pk] = (cid, dia, resp, mc, ms, dc, ds)
    return cache[p.pk]

def fmt_hora(h):
    return h.strftime("%H:%M") if h is not None else "NAO_VERIFICADO"

def flag(v):
    return "NAO_VERIFICADO" if v is None else int(v)

def registrar(chave, p, b, delta, dc, ds):
    g = grupos[chave]
    g["total"].add(p.pk)
    g["relacoes"].add(b.pk)
    if delta is None:
        g["delta_nv"].add(p.pk)
    else:
        if delta > 0:
            g["hora_diferente"].add(p.pk)
        if delta > 30:
            g["acima30"].add(p.pk)
    g["raio_cadastro_nv" if dc is None else
      "dentro90_cadastro" if dc else "fora90_cadastro"].add(p.pk)
    g["raio_snapshot_nv" if ds is None else
      "dentro90_snapshot" if ds else "fora90_snapshot"].add(p.pk)

emitir("S", "R_COLUNAS", "col", "pergunta_pk", "batida_pk", "dia_turno",
       "hora_respondida", "batida_local", "delta_relogio_min",
       "marco_cadastro", "dentro90_cadastro", "marco_snapshot",
       "dentro90_snapshot", "retratada", "absorcao_pareador")
try:
    with connection.execute_wrapper(apenas_select):
        qs = (Batida.objects.filter(
            pergunta_origem__motivo__in=("intervalo_saida", "intervalo_volta"),
            pergunta_origem__tipo_resposta="hora",
            pergunta_origem__respondida_em__isnull=False,
            pergunta_origem__validada_em__isnull=False)
            .filter(Q(pergunta_origem__data_evento__gte=INICIO) |
                    Q(pergunta_origem__data_evento__isnull=True))
            .select_related("pergunta_origem__disputa",
                            "pergunta_origem__chamado_destino")
            .order_by("pergunta_origem_id", "pk"))
        for b in qs.iterator(chunk_size=1000):
            try:
                p = b.pergunta_origem
                dados = preparar(p)
                if dados is None:
                    continue
                cid, dia, resp, mc, ms, dc, ds = dados
                if b.colaborador_id != cid:
                    raise ValueError("colaborador_divergente")
                links_por_pergunta[p.pk] += 1
                local = timezone.localtime(b.timestamp)
                delta = (tentar("delta_batida_resposta", p.pk,
                                lambda: _dist_marco(b.timestamp, [resp]))
                         if resp is not None else None)
                emitir("R", f"col{cid:03d}", p.pk, b.pk, dia.isoformat(),
                       fmt_hora(resp), local.isoformat(),
                       "NAO_VERIFICADO" if delta is None else f"{delta:.3f}",
                       fmt_hora(mc), flag(dc), fmt_hora(ms), flag(ds),
                       int(b.retratada_em is not None), "NAO_VERIFICADO")
                for chave in (("TOTAL",), ("COLAB", f"col{cid:03d}"),
                              ("COLAB_DIA", f"col{cid:03d}", dia.isoformat())):
                    registrar(chave, p, b, delta, dc, ds)
            except Exception as exc:
                erro("item", b.pk, exc)
except Exception as exc:
    erro("consulta", 0, exc)

metricas = ("total", "relacoes", "hora_diferente", "acima30", "delta_nv",
            "dentro90_cadastro", "fora90_cadastro", "raio_cadastro_nv",
            "dentro90_snapshot", "fora90_snapshot", "raio_snapshot_nv")
emitir("S", "RESUMO_COLUNAS", *metricas)
if ("TOTAL",) not in grupos:
    grupos[("TOTAL",)]
for chave in sorted(grupos):
    emitir("S", *chave, *(len(grupos[chave][m]) for m in metricas))
emitir("S", "COLABS", sum(k[0] == "COLAB" for k in grupos))
emitir("S", "PERGUNTAS_COM_MULTIPLAS_BATIDAS",
       sum(n > 1 for n in links_por_pergunta.values()))
emitir("S", "SEM_DIA", len(sem_dia))
emitir("S", "FORA_RECORTE_CONTEXTUAL", len(fora_recorte))
emitir("S", "TOTAL_ERROS", TOTAL_ERROS)
```

Os números `506/1282`, `376`, `133` e `59` ficam como informação do anexo, sem confirmação independente: **[nao verificado]**. Quantos dos 59 ultrapassam 30 minutos e quantos estão no raio cadastrado: **[nao verificado]**. O anexo também não fornece os marcos cadastrados necessários para confirmar que o caso 50833 é o contraexemplo “dentro do envelope”: **[nao verificado]**.

## 12/11 13:05 · sessao 529fdf18fac9 · - · - · origem=Ronald (tmux)

C — o número e o juiz real:

- A L-032 aponta o envelope de 90 minutos em `app/docs/LEIS.md:80`.
- O número **90 não é constante compartilhada**: aparece literalmente dentro de `parear_turnos`, no predicado interno `_absorvivel` (`app/ponto/turnos.py:1251`), na guarda do teto (`app/ponto/turnos.py:1352`) e na absorção por marco (`app/ponto/turnos.py:1358`).
- A função reutilizável real é `_dist_marco(ts, marcos)`: calcula a menor distância em minutos entre o instante local e cada hora de marco, considerando dia anterior, mesmo dia e seguinte. Retorna `None` sem marco; **não recebe colaborador nem dia da jornada e não responde um booleano** (`app/ponto/turnos.py:326`–`340`).
- O predicado completo de absorção exige distância ao intervalo `<=90` e intervalo tão próximo quanto a borda do turno; no laço exige também turno corrente e respeita o teto de continuidade. Portanto, estar no raio de X não prova que a resposta seria absorvida como intervalo numa jornada real (`app/ponto/turnos.py:1246`–`1251`; `app/ponto/turnos.py:1349`–`1370`).
- **Não há função única pronta** `(colab, dia, marco X, resposta) -> dentro_envelope`. Essa pergunta é composta por resolução de vínculo/marcos, distância e guardas internas do pareador (`app/escala/servico_jornada.py:15`–`66`; `app/ponto/turnos.py:1229`–`1251`; `app/ponto/turnos.py:1349`–`1370`).

Fontes de marco:

- Vínculo de um colaborador/dia: `escala_vigente`, que carrega células/vínculos e chama `vinculo_do_dia` (`app/escala/servico_jornada.py:15`–`66`).
- Marcos desse vínculo/dia: `ec.marcos_do_dia(dia)`, DNA congelado da célula com fallback para template (`app/escala/models.py:1098`–`1133`).
- Marcos efetivamente usados pelo pareador: template por `marcos_do_template` (`app/ponto/turnos.py:1504`–`1514`), refinado por DNA usando a **data civil local da batida** em `_ivm_de`/`_bm_de` (`app/ponto/turnos.py:1229`–`1244`). Com segundo intervalo, o chamador deixa de passar mapa DNA para preservar o segundo par do template (`app/ponto/turnos.py:1984`–`1989`).
- `intervalo_cadastrado` responde **duração cadastrada**, não pertencimento ao envelope (`app/ponto/juiz_batida.py:106`–`146`).

D — recomendação para a sonda:

Use o marco X **cobrado na pergunta**, pelo leitor puro `entrada_de_marco(p.chamado_destino, p.motivo, tipo=None)`; ele resolve por motivo no snapshot `marcos_faltantes`. A casca `_marco_faltante(ch,motivo,tipo)` devolve só a hora e admite fallback por tipo apenas no formato legado (`app/chamados/juizes.py:523`–`580`). Teste o raio chamando a função real:

```python
dist = _dist_marco(ts_resposta, [hora_marco])
dentro_raio_marco_cobrado = None if dist is None else dist <= 90
```

Esse resultado precisa ser descrito como **“raio de 90 minutos do marco cobrado no snapshot”**, e não “classificação plena do pareador”; o último resultado depende de contexto e guardas acima (`app/ponto/turnos.py:1246`–`1251`; `app/ponto/turnos.py:1349`–`1370`).

Não usar `grade_da_celula` para identificar `hii`/`hfi` apenas pelo tipo: ela entrega `{'tipo','hora'}` das lâmpadas, sem chave de marco ou motivo (`app/escala/services/leitor_celula.py:148`; `app/escala/services/leitor_celula.py:413`–`418`). Identificação só por E/S é precisamente a ambiguidade entrada/volta e intervalo/saída descrita em `_marco_faltante` (`app/chamados/juizes.py:526`–`531`). A grade faz leituras no corpo, mas sua régua é a ATA, uma fonte diferente do DNA/template usado no pareador (`app/escala/services/leitor_celula.py:361`–`362`; `app/escala/services/leitor_celula.py:413`–`418`).

## 12/11 13:05 · sessao 5af3ff980af8 · - · - · origem=Ronald (tmux)

Item A — achados da cópia, somente leitura:

- **Correção de caminho:** as referências `:342` e `:712` pertencem a `app/chamados/juizes.py`; em `app/core/juizes.py:342` está `SEM_JUIZ` e em `app/core/juizes.py:712` está `_TT4`. (`app/chamados/juizes.py:341`, `app/chamados/juizes.py:712`, `app/core/juizes.py:342`, `app/core/juizes.py:712`)

- **Derivador da hora:** `_ts_tipo_do_plantio` começa com `p.resposta_colab`; para os dois motivos de intervalo, pede `_marco_faltante` e, havendo hora, substitui a resposta pelo marco. Combina a hora com o dia e aplica overnight. O comentário “horário FIXO” não corresponde a uma condição explícita sobre `intervalo_modo` nesse trecho: o discriminador executável é motivo de intervalo + marco encontrado. (`app/chamados/services/materializacao.py:409`, `app/chamados/services/materializacao.py:413`, `app/chamados/services/materializacao.py:419`, `app/chamados/services/materializacao.py:426`)

- **Fonte do marco:** `_marco_faltante` delega a `entrada_de_marco`, que lê `ch.contexto_json['marcos_faltantes']`, casa primeiro por motivo; havendo algum motivo na lista, não cai no tipo de outro marco; somente lista inteiramente legada admite fallback por tipo. (`app/chamados/juizes.py:523`, `app/chamados/juizes.py:535`, `app/chamados/juizes.py:549`, `app/chamados/juizes.py:551`, `app/chamados/juizes.py:576`, `app/chamados/juizes.py:578`)

- **Quem escreve:** `_materializar_resposta_hora` chama o derivador e consulta reuso em ±20 minutos. Sem reuso, chama `registrar_batida(colaborador, ts, tipo, 'disputa_s84_retro', ...)`; a porta efetua `Batida.objects.create(timestamp=timestamp, ...)`. (`app/chamados/services/materializacao.py:852`, `app/chamados/services/materializacao.py:865`, `app/chamados/services/materializacao.py:933`, `app/ponto/registro_batida.py:157`)

- **Relação persistida:** nova ou reusada, recebe `pergunta_origem=p` via `anotar`; o FK reverso é `batidas_plantadas`. Portanto, esse vínculo inclui reuso e sozinho não prova que uma batida nova nasceu. A trilha `materializar_batida` também é emitida após os dois caminhos e grava `batida_pk`, timestamp pretendido, `pergunta_pk` e `resposta_colab`. (`app/chamados/services/materializacao.py:871`, `app/chamados/services/materializacao.py:952`, `app/chamados/services/materializacao.py:954`, `app/chamados/services/materializacao.py:957`, `app/chamados/services/materializacao.py:967`, `app/chamados/services/materializacao.py:976`, `app/ponto/models.py:40`)

- **Não localizei segunda cópia executável dessa substituição** no inventário de chamadas: `_ts_tipo_do_plantio` serve o plantio, o estado da UI e `_slot_esperado`. Este último **hoje delega** ao mesmo derivador; o docstring “Reproduz” ficou, mas o corpo declara expressamente que não reproduz mais. (`app/chamados/services/materializacao.py:450`, `app/chamados/services/materializacao.py:852`, `app/chamados/juizes.py:342`, `app/chamados/juizes.py:369`, `app/chamados/juizes.py:372`)

Leitores diretos desse slot, portanto herdando a hora do marco:

| Leitor | Uso | Evidência |
|---|---|---|
| `estado_aplicacao_pergunta` | Estado do modal; reuso, paridade e promessa de horário a registrar | `app/chamados/services/materializacao.py:450`, `app/chamados/services/materializacao.py:453`, `app/chamados/services/materializacao.py:458`, `app/chamados/services/materializacao.py:463` |
| `_pergunta_materializada_de_fato` | Compara batidas ligadas à hora do slot; fallback de acervo busca a mesma janela | `app/chamados/juizes.py:752`, `app/chamados/juizes.py:755`, `app/chamados/juizes.py:760`, `app/chamados/juizes.py:812` |
| `_plantio_na_hora_do_slot` | Batida ligada deve estar em ±20 minutos do slot; para hora, aceita dia do turno ou seguinte | `app/chamados/juizes.py:267`, `app/chamados/juizes.py:276`, `app/chamados/juizes.py:278` |
| `CargaDaVida` | Cache de slots e preparação das testemunhas do C6 | `app/chamados/services/carga_pergunta_viva.py:113`, `app/chamados/services/carga_pergunta_viva.py:119`, `app/chamados/services/carga_pergunta_viva.py:126` |
| `_explicar_validada` | Procura batida em ±20 minutos e diz “validada como …” usando a hora derivada | `app/chamados/services/explicador.py:29`, `app/chamados/services/explicador.py:34`, `app/chamados/services/explicador.py:58`, `app/chamados/services/explicador.py:60` |
| `classe_do_lote` | Confere conflito próximo do slot de plantio | `app/chamados/services/validacao.py:869`, `app/chamados/services/validacao.py:872`, `app/chamados/services/validacao.py:875` |
| `vigia_carimbo.classificar` | Distingue slot não derivável / plantio fora do slot / sem plantio | `app/chamados/services/vigia_carimbo.py:105`, `app/chamados/services/vigia_carimbo.py:110`, `app/chamados/services/vigia_carimbo.py:113` |

**Leitores transitivos do juiz de materialização:** veredito lavrado, raiox, explicador e fechamento/materialização voltam a esse juiz. (`app/chamados/veredito_lavrado.py:263`, `app/chamados/services/raiox.py:187`, `app/chamados/services/explicador.py:93`, `app/chamados/services/materializacao.py:1239`, `app/chamados/services/materializacao.py:1433`, `app/chamados/juizes.py:1473`)

**Esmeril semântica:** o campo bruto `resposta_colab` permanece a fala; `Batida.timestamp` pode conter o marco, e o slot esperado também passa a conter o marco. Assim, não é uma mistura dentro de `resposta_colab`; é uma divergência entre declaração preservada e instante materializado. O próprio código guarda ambos na trilha. (`app/chamados/services/materializacao.py:412`, `app/chamados/services/materializacao.py:416`, `app/chamados/services/materializacao.py:969`, `app/chamados/services/materializacao.py:977`)

Há frases que chamam o slot de “hora respondida” embora o cálculo possa usar o marco: docstring do juiz de materialização, explicador e descrição do vigia. Isso sustenta o descompasso textual e operacional; uma intenção adicional de seus autores é [nao verificado]. (`app/chamados/juizes.py:663`, `app/chamados/juizes.py:667`, `app/chamados/services/explicador.py:31`, `app/chamados/services/vigia_carimbo.py:17`)

**Leitores que preservam a distinção:** `delta_da_resposta` lê a resposta bruta separadamente do marco; `motivo_implausivel_validacao` faz o mesmo. Ambos procuram marco no chamado e depois na grade, sem substituir a resposta. O fio também apresenta `esperada_prevista` separadamente e calcula o estado da aplicação via derivador. (`app/chamados/juizes.py:1846`, `app/chamados/juizes.py:1850`, `app/chamados/juizes.py:1863`, `app/chamados/juizes.py:1972`, `app/chamados/juizes.py:1977`, `app/chamados/services/fio.py:325`, `app/chamados/services/fio.py:331`, `app/chamados/services/fio.py:336`)

**Testes da mesma política:** `test_s139.py` exige resposta `02:00` virar `E@03:00`, rejeitando expressamente `02:00`. O teste posterior O306 mantém S139: resposta `13:05` deve produzir `13:00` quando é o marco do próprio motivo. (`app/chamados/tests/test_s139.py:40`, `app/chamados/tests/test_s139.py:45`, `app/chamados/tests/test_s139.py:51`, `app/chamados/tests/test_s139.py:53`, `app/chamados/tests/test_s139.py:55`, `app/chamados/tests/test_o306_marco_do_motivo_nao_e_do_vizinho.py:146`, `app/chamados/tests/test_o306_marco_do_motivo_nao_e_do_vizinho.py:151`, `app/chamados/tests/test_o306_marco_do_motivo_nao_e_do_vizinho.py:153`)

**Outro consumidor explícito do caráter mutável do marco:** reconciliar grade evita reescrever a cobrança enquanto houver fio vivo respondido, porque a materialização lê `marcos_faltantes`; comentário declara que trocar o marco sob resposta já dada a orfanaria. (`app/ponto/management/commands/reconciliar_grade.py:437`, `app/ponto/management/commands/reconciliar_grade.py:460`)

## 12/11 13:05 · sessao 46494a3f7c2a · - · - · origem=Ronald (tmux)

B — O caso protegido está declarado em [app/chamados/tests/test_s139.py:15](/home/ronald/codex_ws/096ace9d0f9fe1a939f12e1f303bcb41ddb91e3a/app/chamados/tests/test_s139.py:15): volta esperada às 03:00; resposta livre 02:00; impedir uma `E@02:00` no horário da saída do intervalo. A fixture contém marco `03:00/E`, resposta `02:00` e validação; o teste exige batida `E`, diferente de `02:00`, exatamente `03:00` — [app/chamados/tests/test_s139.py:40](/home/ronald/codex_ws/096ace9d0f9fe1a939f12e1f303bcb41ddb91e3a/app/chamados/tests/test_s139.py:40), [app/chamados/tests/test_s139.py:46](/home/ronald/codex_ws/096ace9d0f9fe1a939f12e1f303bcb41ddb91e3a/app/chamados/tests/test_s139.py:46), [app/chamados/tests/test_s139.py:49](/home/ronald/codex_ws/096ace9d0f9fe1a939f12e1f303bcb41ddb91e3a/app/chamados/tests/test_s139.py:49).

“Cair no slot oposto” significa concretamente: jornada `19→07`, intervalo `02:00/S→03:00/E`; a batida `E@02:00` coincide com o marco `S`. O teste do auditor descreve esse caso e comprova detecção como `batida_retro_invertida` criando `E@02:05`; o detector anterior, limitado às bordas da jornada, não o encontrava — [app/chamados/tests/test_auditar_disputas_padroes.py:117](/home/ronald/codex_ws/096ace9d0f9fe1a939f12e1f303b41ddb91e3a/app/chamados/tests/test_auditar_disputas_padroes.py:117), [app/chamados/tests/test_auditar_disputas_padroes.py:140](/home/ronald/codex_ws/096ace9d0f9fe1a939f12e1f303bcb41ddb91e3a/app/chamados/tests/test_auditar_disputas_padroes.py:140). Consequência específica em pareamento, folha ou dinheiro causada pela retirada da S139: [nao verificado].

A S139 hoje não testa distância nem envelope: substitui a resposta pelo `_marco_faltante` quando há motivo de intervalo e marco disponível — [app/chamados/services/materializacao.py:409](/home/ronald/codex_ws/096ace9d0f9fe1a939f12e1f303bcb41ddb91e3a/app/chamados/services/materializacao.py:409).

A O306 preservou expressamente a regra para o marco do próprio motivo: resposta `13:05` continua produzindo `13:00` — [app/chamados/tests/test_o306_marco_do_motivo_nao_e_do_vizinho.py:146](/home/ronald/codex_ws/096ace9d0f9fe1a939f12e1f303bcb41ddb91e3a/app/chamados/tests/test_o306_marco_do_motivo_nao_e_do_vizinho.py:146). Corrigiu outra falha: fallback por tipo entregava marco de outro motivo, produzindo `08:00` para pergunta de `13:00`; o marco das 13:00 continuava vazio, a célula seguia `furo` e havia nova declaração — [app/chamados/juizes.py:555](/home/ronald/codex_ws/096ace9d0f9fe1a939f12e1f303bcb41ddb91e3a/app/chamados/juizes.py:555).

Commit, data, autoria e corte/lei específicos do nascimento da S139 por `git log/blame`: [nao verificado]. Raiz e `app/` não foram reconhecidas como repositório Git nas tentativas de consulta; nenhum arquivo foi criado. [nao verificado] [nao verificado]

A L-032 local exige reconhecer intervalo dentro do envelope apesar da diferença de horário, mas sua linha não cita S139 — [app/docs/LEIS.md:80](/home/ronald/codex_ws/096ace9d0f9fe1a939f12e1f303bcb41ddb91e3a/app/docs/LEIS.md:80). A L-099 exige coerência com o cadastro e encaminha divergências de cadastro/batida ao admin; tampouco cita a origem da S139 — [app/docs/LEIS.md:122](/home/ronald/codex_ws/096ace9d0f9fe1a939f12e1f303bcb41ddb91e3a/app/docs/LEIS.md:122).

## 12/11 12:57 · sessao 9d7727ee3e34 · AUDITORIA-DE-TRAVAS · verificador · origem=Code

**Parecer: atendimento parcial.** O cap foi implementado, mas permanecem a falta de recusa específica por ausência de entrada na tabela e a diferença entre teto da execução e prazo total do wrapper. (`bin/codex_pergunta.sh:89`, `:358`, `:383`, `:408`)

1. **Contrato, aval e furos apontados**

| Ponto | Resultado |
|---|---|
| Tetos e rebaixamento | Confere: construtor **1930**, verificador **894**, leitura/censo/sonda **1191**; override maior reduz `TIMEOUT` e emite “teto do papel”. (`contrato_item6.txt:3–4`; `bin/codex_pergunta.sh:89–92`) |
| Default sem override | Resolvido no código: variável **ausente ou vazia** usa o teto do papel. Isso altera o default anterior de 900 e escolhe uma das alternativas que o contrato deixara abertas. Aval específico dessa escolha: **[não verificado]**. (`diff_c8c3420f1.txt`, hunk `@@ -80,9 +80,16 @@`; `contrato_item6.txt:3–4`; `bin/codex_pergunta.sh:90`) |
| Domínio do override | Agora aceita somente `[1-9][0-9]*`: recusa zero, negativos, frações, sufixos e até `0600`. É uma restrição adicional não definida pelo contrato; “inteiro positivo” não explica a rejeição de zeros iniciais. (`contrato_item6.txt:4`; `bin/codex_pergunta.sh:91`) |
| Inteiro enorme | Passa pela guarda textual, mas pode exceder a faixa da comparação `-le`: produz diagnóstico do Bash antes da linha de rebaixamento. O cap continua aplicado, porém a promessa de **uma linha** no stderr não fica garantida nesse domínio. (`bin/codex_pergunta.sh:91–92`; `contrato_item6.txt:4`) |
| Mensagem do `flock -w` | Usa corretamente o `TIMEOUT` já limitado. Continua dizendo “ocupada há Ns” sem medir idade da ocupação e dá esse diagnóstico para qualquer falha do `flock`; esse comportamento foi preservado. (`bin/codex_pergunta.sh:383`; `diff_c8c3420f1.txt`, alterações do wrapper) |
| Papel sem entrada | **Nenhum dos cinco papéis atuais fica descoberto.** Entretanto, a tabela usa `*) TETO_S=1191`: acrescentar um papel somente à lista de aceitos concede default silencioso, sem exigir medida/entrada própria. A recusa contratada por ausência de medida não está materializada na tabela. (`contrato_item6.txt:4`; `bin/codex_pergunta.sh:84`, `:89`) |

Portanto, não certifico “o contrato e o aval, e só eles”: o núcleo do cap está entregue, mas o diff também fixa default e domínio antes indefinidos, enquanto mantém o fallback genérico da tabela. (`contrato_item6.txt:3–4`; `bin/codex_pergunta.sh:89–92`)

O `900` de `encadeado.sh` continua sendo outro parâmetro: espera por carimbos, com rc 2; sua preservação corresponde ao aval. (`aval_respostas_1211_1230.txt:3`; `bin/encadeado.sh:71`, `:84–96`)

2. **ESMERIL-SEMÂNTICA: quem escreve e quem lê**

O censo textual de `bin/`, incluindo `bin/tests/`, resulta nos seguintes escritores e leitores; produtores ou leitores dinâmicos fora dos sítios identificados são **[não verificado]**. (`bin/codex_pergunta.sh:90–92`; referências individuais abaixo)

| Variável | Quem escreve | Quem lê |
|---|---|---|
| `CODEX_TIMEOUT` | `20` no selo de limite; `20` no selo do disjuntor; `unset`, `3600`, `600` e `abc` no selo do wrapper. (`bin/tests/test_codex_limite.sh:24`; `bin/tests/test_disjuntor.sh:1213`; `bin/tests/test_codex_pergunta.sh:121`, `:458`, `:462`, `:467`) | O wrapper importa o ambiente para `TIMEOUT`. O selo encaminha esse ambiente pelo `_roda`. (`bin/codex_pergunta.sh:90`; `bin/tests/test_codex_pergunta.sh:132–137`) |
| `TIMEOUT` | O wrapper atribui override/default e depois aplica o cap. O mutante remove a atribuição do cap na cópia de teste. (`bin/codex_pergunta.sh:90`, `:92`; `bin/tests/test_codex_pergunta.sh:567`, `:583–585`) | Guarda de domínio; comparação e diagnóstico; espera e mensagem da trava; execução construtora; execução dos demais papéis. (`bin/codex_pergunta.sh:91`, `:92`, `:383`, `:408`, `:415`) |
| Referências textuais | O teste do disjuntor manipula literalmente o comando contendo `$TIMEOUT`; não calcula outro prazo. (`bin/tests/test_disjuntor.sh:1826`) | O mutador do selo lê o fonte e substitui a atribuição marcada. (`bin/tests/test_codex_pergunta.sh:567`, `:577–586`) |

Há dois usos documentados relevantes fora de `bin/`: **censo com 2400**, que nesta implementação seria reduzido a **1191**, e **construtor com 1800**, que permanece abaixo de 1930. São registros históricos, não prova de chamadores atualmente ativos nem da necessidade desses prazos. (`app/docs/RELATO.md:7756`, `:7782`, `:425`; `bin/codex_pergunta.sh:89`, `:92`)

`CODEX_TIMEOUT` **não recebe o valor limitado de volta**: pode continuar sendo `3600` no ambiente enquanto `TIMEOUT=1191` governa o wrapper. Um descendente que leia o primeiro como prazo efetivo: **[não verificado]**. (`bin/codex_pergunta.sh:90`, `:92`)

A unidade dos consumidores é segundos, mas há **dois orçamentos sucessivos**: a trava da resposta pode esperar um teto inteiro; depois o processo recebe outro teto inteiro. O construtor mantém ainda `-k 30`. Logo, 1930 segundos não é prazo total do lançador. (`bin/codex_pergunta.sh:6`, `:383`, `:408`, `:415`)

Antes disso, o gate bloqueia sem prazo e os slots repetem esperas de cinco segundos sem limite acumulado. A duração registrada começa em `T0C`, inclui preparação posterior, filas, trava e encerramento; não mede exclusivamente o intervalo sob `timeout`. (`bin/codex_pergunta.sh:259–267`, `:298–325`, `:358–370`, `:427`)

“Estourou = 124” também precisa desse escopo: a espera da resposta sai **3**, e o inventário construtor pode substituir inclusive um retorno 124 por **3**. O contador existente por papel conta `ini`, qualquer rc; não é o contador contratado de rc 124 em 24 horas. Meta `≤1` por papel: **[não verificado]**. (`contrato_item6.txt:4–5`; `bin/codex_pergunta.sh:383`, `:418`, `:427–440`; `bin/codex_limite_estado.py:71–79`)

3. **Casos novos, mutantes e fake timeout**

**Os dois mutantes têm mordida demonstrável pela leitura do código; execução efetiva permanece [não verificado].** O primeiro deixa `3600` no argumento, contrariando `1191`; o segundo deixa `abc` prosseguir até o cap e a chamada do Codex, contrariando rc 2 e ausência de captura exigidos por `_recusa`. (`bin/codex_pergunta.sh:91–92`; `bin/tests/test_codex_pergunta.sh:140–143`, `:458–460`, `:467`, `:567–568`)

`c_teto_papel` verifica três comportamentos: leitura `3600→1191`, verificador `600→600` e verificador sem override `→894`. `c_timeout_int` cobre somente `abc`. Não cobre os demais limites do domínio nem todos os tetos/defaults por papel. (`bin/tests/test_codex_pergunta.sh:455–467`)

**O fake desta cópia encaminha o valor ao timeout real:** grava `"$@"` e executa `/usr/bin/timeout "$@"`, sem transformação. Cada subcaso apaga previamente a captura, evitando argumento velho. (`bin/tests/test_codex_pergunta.sh:115–118`, `:458`, `:462`, `:464`)

A cobertura, porém, tem limites concretos:

- Apenas o primeiro subcaso exige rc 0; os de `600` e `894` verificam somente o argumento capturado. A mensagem é buscada em stdout+stderr mesclados, sem exigir stderr nem exatamente uma linha. (`bin/tests/test_codex_pergunta.sh:136`, `:459–465`)
- O Codex falso termina imediatamente: os casos não provam expiração, rc 124, escalada `-k 30`, nem aplicação do teto ao `flock`. (`bin/tests/test_codex_pergunta.sh:111`, `:455–467`; `bin/codex_pergunta.sh:383`, `:408`)
- Existe um falso verde possível além dos dois mutantes: separar `timeout "$TIMEOUT" true; codex ...` manteria a primeira linha capturada correta, deixando o Codex fora do relógio. Os casos novos não verificam qual comando recebe o timeout. Execução desse mutante adicional: **[não verificado]**. (`bin/tests/test_codex_pergunta.sh:456`, `:460`, `:463`, `:465`; chamada atual correta em `bin/codex_pergunta.sh:415`)

O selo integral não foi executado porque cria, altera e apaga arquivos, inclusive a cópia usada pelos mutantes. Resultado do selo e tabela de mutantes nesta auditoria: **[não verificado]**. (`bin/tests/test_codex_pergunta.sh:18–19`, `:74–75`, `:585`, `:596`)

4. **Classes de reprovação**

| Classe | Constatação neste delta |
|---|---|
| Segundo juiz | Não identificado para o teto: a decisão permanece no wrapper. (`bin/codex_pergunta.sh:89–92`) |
| Segundo escritor fora da porta | Não identificado: default e cap escrevem a variável no mesmo wrapper; a escrita dos mutantes ocorre no cenário de teste. (`bin/codex_pergunta.sh:90`, `:92`; `bin/tests/test_codex_pergunta.sh:585`) |
| Relógio fora da borda | **Presente e preservado:** fila sem prazo global, espera da resposta separada da execução, preparação e inventário fora do timeout do Codex. (`bin/codex_pergunta.sh:358–370`, `:383`, `:408`, `:427`) |
| Derivação paralela de fonte única | Não aparece uma segunda tabela operacional de tetos. Entretanto, o comentário afirma janela de 20 “DO PAPEL”; p95 individual de leitura/censo/sonda continua **[não verificado]**, pois o anexo relata janela agrupada. (`bin/codex_pergunta.sh:85–89`; `contrato.r`, seção 3) |
| Selo/RED sem mordida | Os dois mutantes têm mordida estática; a cobertura não certifica toda a semântica temporal. (`bin/tests/test_codex_pergunta.sh:455–467`, `:567–594`) |
| Ordem de trava | Preservada: vaga construtora → gate/slot → resposta → porta B3. Não há inversão introduzida no diff fornecido. (`bin/codex_pergunta.sh:247`, `:358`, `:363`, `:383–385`) |
| Campo com dois significados | **Presente, inclusive previsto no contrato:** `TIMEOUT` governa espera da trava e duração do processo. A falha semântica é tratá-lo como prazo global ou equipará-lo a `duracao_s`. (`contrato_item6.txt:3`; `bin/codex_pergunta.sh:267`, `:383`, `:408`, `:415`) |

## 12/11 12:54 · sessao ede65d4988a8 · - · - · origem=Ronald (tmux)

- **Implementação principal confere:** 1930/894/1191, default no teto, cap com diagnóstico e compartilhamento do valor entre `flock` e execução; escolhas estão em `bin/codex_pergunta.sh:89–92`, leitores em `:383`, `:408`, `:415`. Exatidão dos p95 individuais e eficácia futura: [não verificado].
- **Default resolvido pelo código:** ausência ou vazio vira teto, pois usa `${CODEX_TIMEOUT:-$TETO_S}`. É uma escolha entre as alternativas antes apontadas, não explicitada no contrato fornecido (`bin/codex_pergunta.sh:90`; `contrato_item6.txt:3–4`).
- **Domínio novo:** aceita apenas decimal sem zero inicial; rejeita `0`, `0600`, negativos, frações e sufixos. A recusa de `0600`, inclusive, excede a descrição “inteiro positivo”. Essa restrição não aparece no contrato (`bin/codex_pergunta.sh:91`; `contrato_item6.txt:4`).
- **Papel sem entrada permanece um furo estrutural:** desconhecido atual sai 2 pela lista, mas a tabela tem `*)1191`. Acrescentar papel somente à lista produz teto silencioso; inexiste recusa específica por falta de entrada (`bin/codex_pergunta.sh:84`, `:89`; `contrato_item6.txt:4`).
- **Mensagem do flock usa o valor efetivo:** `${TIMEOUT}s` acompanha o cap. Porém “ocupada há” não mede idade da ocupação, e qualquer falha de `flock` recebe esse diagnóstico; comportamento preexistente (`bin/codex_pergunta.sh:383`).
- **Relógio fora da borda e campo com dois significados permanecem:** gate e repetição de slots sem limite acumulado; depois a resposta pode consumir um teto inteiro e a execução recebe outro teto inteiro. Logo 1930 segundos não limita duração total do wrapper (`bin/codex_pergunta.sh:358–370`, `:383`, `:408`, `:415`).
- **124 não cobre todo estouro/resultado:** trava sai 3; inventário construtor pode substituir inclusive um 124 por 3. São comportamentos preservados, não introduzidos pelo diff (`bin/codex_pergunta.sh:383`, `:427–440`).
- **Métrica tem borda diferente:** `duracao_s` começa em `T0C`, inclui preparação posterior e filas, e termina na saída; não é apenas duração sob `timeout` (`bin/codex_pergunta.sh:259–267`, `:298–325`, `:358–415`).
- **Fonte histórica não prova p95 individual:** comentário afirma últimas 20 “DO PAPEL”, mas o anexo `contrato.r` informa 20 chamadas do grupo leitura/censo/sonda. Atribuir p95 individual aos três segue [não verificado] (`bin/codex_pergunta.sh:85–88`; `contrato.r`, seção 3).
- **Outras classes:** o diff não demonstra segundo juiz, segundo escritor ou inversão de travas: concentra decisão temporal em `:89–92` e mantém consumidores/ordem existentes; ocorrência no restante do sistema [não verificado] (`bin/codex_pergunta.sh:89–92`, `:355–415`).

## 12/11 12:54 · sessao d52cf582f8d8 · MAPA-DO-FIO-MORTO · sonda · origem=Code

Referências `dado:N`: numeração do texto colado, contando somente linhas não vazias, de `S UNIVERSO_D...` como linha 1 até `S ERROS...` como linha 256.

1. **Universo e comparação com os 241**

| Medida | Dia-colab | Sustentação |
|---|---:|---|
| Universo d | **243** | `dado:1` |
| `fio=nada` | **155** | `dado:245` |
| Outro fio conhecido | **68**: `sistema=60`, `admin=8` | Soma de `dado:245` |
| `fio=nao_sei` | **20** | `dado:245` |
| Conferência | **155 + 60 + 8 + 20 = 243** | `dado:245` |

**243 não bate com 241: são 2 dia-colab a mais.** O anexo anterior separa classe d em **241 `SEM_ACAO` + 2 `COLAB_RESPONDE`**, cuja soma é 243 (`censo_mapa.r` fornecido, referências transcritas `recorte.out:50` e `recorte.out:49`; `dado:1`).

Causa provável **[nao verificado]**: a sonda atual inclui toda a classe d, reunindo aqueles dois subconjuntos. Que sejam exatamente os mesmos dias, sob o mesmo recorte temporal, é **[nao verificado]**.

Também **155 `nada` não equivale aos 241 `SEM_ACAO`: diferença de 86**. A classificação anterior `SEM_ACAO` não media diretamente `tarefa_do_dia == nada` (`dado:245`; anexo `censo_mapa.r`, referências transcritas `recorte.out:50` e `sonda_mapa_fio.py:39`).

2. **Grupos e comportamento da folha**

| Fio | Motivo de retenção | Veredito | n | Folha | Linha do dado |
|---|---|---|---:|---|---|
| `nada` | `espelho_nunca_bateu` | `nunca_bateu` | 97 | Retém pelo motivo indicado | `dado:247` |
| `sistema` | `espelho_nunca_bateu` | `nunca_bateu` | 58 | Retém pelo motivo indicado | `dado:248` |
| `nada` | `espelho_indefinida` | `indefinida` | 27 | Retém pelo motivo indicado | `dado:249` |
| `nada` | `sem_celula` | `None` | 25 | Retém como `sem_celula` | `dado:250` |
| `nao_sei` | `sem_celula` | `None` | 20 | Retém como `sem_celula` | `dado:251` |
| `admin` | `espelho_fato_em_ausencia` | `fato_em_ausencia` | 8 | Retém pelo motivo indicado | `dado:252` |
| `nada` | `espelho_furo` | `furo` | 4 | Retém pelo motivo indicado | `dado:253` |
| `sistema` | `espelho_indefinida` | `indefinida` | 2 | Retém pelo motivo indicado | `dado:254` |
| `nada` | `espelho_discordante` | `discordante` | 2 | Retém pelo motivo indicado | `dado:255` |

**Todos os 243 apresentados retêm; nenhum grupo aparece como “não retém”.** Os grupos somam 243, e a função devolve em `dias` somente dias retidos (`dado:247–255`; `app/folha/export.py:1193`, `app/folha/export.py:1226`).

Na função, **`espelho_X` significa retenção pelo veredito acusador `X` da célula**, após as guardas de protestos não cobráveis e de furo somente da pausa (`app/folha/export.py:1241`, `app/folha/export.py:1253`, `app/folha/export.py:1261`, `app/folha/export.py:1262`). O significado dos vereditos na cópia é:

- **`nunca_bateu`**: ramo sem adesão, descrito como “zero batida na vida”; o literal depende das guardas de cobertura integral e dia sem trabalho (`app/ponto/services/cartorio.py:1211`, `app/ponto/services/cartorio.py:1289`, `app/ponto/services/cartorio.py:1295`).
- **`indefinida`**: agrega previsto com dia indefinido, fase não declarada com previsto ou parcial sem período (`app/escala/models.py:1615`).
- **`fato_em_ausencia`**: trabalho em dia coberto por ausência (`app/escala/models.py:1755`).
- **`furo`**: agrega os códigos de furo enumerados no mapa; o dado não identifica qual código gerou cada ocorrência **[nao verificado]** (`app/escala/models.py:1612`).
- **`discordante`**: agrega batida órfã fora da tolerância, realizado inflado ou realizado zero com turno; o código individual não consta **[nao verificado]** (`app/escala/models.py:1616`).

**`sem_celula` não significa liberação**: pode indicar célula ausente, havendo vínculo, ou célula existente sem veredito e com `trabalha` definido, respeitadas as condições de dia passado e demissão. Qual dessas situações corresponde a cada um dos 45 é **[nao verificado]** (`app/folha/export.py:1235`, `app/folha/export.py:1238`, `app/folha/export.py:1263`; `dado:246`).

3. **Erros da sonda**

**0 erros reportados; nenhum tipo listado (`{}`).** O item 1 não é parcial por erros reportados: são 243 classificados e zero faltantes por erro na saída fornecida (`dado:256`; `dado:245`).

## 12/11 12:54 · sessao aa21dc92d6e9 · - · - · origem=Ronald (tmux)

Censo literal completo em `bin/` e `bin/tests/`, incluindo ignorados:

| Variável | Quem escreve | Quem lê |
|---|---|---|
| `CODEX_TIMEOUT` | Teste limite: `20`; teste disjuntor: `20`; selo wrapper: `unset`, `3600`, `600`, `abc`. (`bin/tests/test_codex_limite.sh:24`; `bin/tests/test_disjuntor.sh:1213`; `bin/tests/test_codex_pergunta.sh:121`, `:458`, `:462`, `:467`) | Wrapper importa em `TIMEOUT` com default para ausente **ou vazio**. Outros leitores funcionais em `bin/`: não encontrados no censo textual; leitores dinâmicos: [não verificado]. (`bin/codex_pergunta.sh:90`) |
| `TIMEOUT` | Wrapper atribui override/default e depois cap; mutante remove a segunda atribuição apenas na cópia de teste. (`bin/codex_pergunta.sh:90`, `:92`; `bin/tests/test_codex_pergunta.sh:567`, `:583`) | Validação, comparação/diagnóstico, espera da resposta, duração do processo nos dois ramos; mutante externo menciona literalmente o comando de flock. (`bin/codex_pergunta.sh:91`, `:92`, `:383`, `:408`, `:415`; `bin/tests/test_disjuntor.sh:1826`) |

`CODEX_TIMEOUT` **não recebe de volta** o cap: pode permanecer `3600` no ambiente herdado enquanto `TIMEOUT=1191` rege o wrapper. Leitor descendente que suponha `CODEX_TIMEOUT` efetivo: [não verificado]. (`bin/codex_pergunta.sh:90`, `:92`; `bin/tests/test_codex_pergunta.sh:458`)

ESMERIL: unidade declarada é segundos; o campo tem duas aplicações reais, **espera da trava** e **duração do processo**, com orçamentos renovados em série. Flock falha com rc3; timeout é outro sítio; construtor acrescenta `-k 30`. Não existe orçamento global compartilhado. (`bin/codex_pergunta.sh:6`, `:383`, `:408`, `:415`)

A fila anterior permanece fora desse orçamento: gate bloqueante e repetição de tentativas de cinco segundos sem prazo acumulado. `duracao_s` vai de `T0C` ao trap e inclui preparação/fila/trava, portanto não equivale ao tempo controlado por `timeout`. (`bin/codex_pergunta.sh:259`, `:267`, `:358`, `:360`, `:368`, `:383`, `:408`)

A mensagem de flock usa o valor efetivo corretamente, mas continua afirmando “ocupada há Ns” em **qualquer** falha de flock; o código não mede idade da ocupação nem distingue erro da espera vencida. (`bin/codex_pergunta.sh:383`)

Chamador acima do teto em código `bin/`: o próprio selo, leitura `3600 → 1191`. Registro histórico externo: PC2-FIO-MUDO documenta `2400`, com papel censo definido no contrato desse piloto; hoje seria capado a1191. Necessidade atual desses2400: [não verificado]. O construtor1800 documentado fica abaixo de1930. (`bin/tests/test_codex_pergunta.sh:458`, `:460`; `app/docs/RELATO.md:7756`, `:7782`, `:425`; `bin/codex_pergunta.sh:89`, `:92`)

O `timeout 900` em teste disjuntor é texto enviado ao parser para verificar reconhecimento de chamada com prefixo; não é leitura de `TIMEOUT`/`CODEX_TIMEOUT`, nem execução desse timeout. (`bin/tests/test_disjuntor.sh:520`, `:563`; `bin/disjuntor.py:1342`)

O `900` de encadeado é outro parâmetro, `TETO`, vindo do quarto argumento e aplicado à espera por carimbos, com rc2; não participa do circuito dessas variáveis. (`bin/encadeado.sh:71`, `:84`, `:92`, `:94`)

## 12/11 12:53 · sessao 8a1026fde32e · - · - · origem=Ronald (tmux)

- **Os dois mutantes têm mordida estática.** Sem `TIMEOUT=$TETO_S`, o argumento permanece `3600`, contrariando a asserção `1191`; sem `exit 2`, `abc` continua até o cap e permite chamar o Codex, contrariando `_recusa`, que exige rc 2 e ausência de captura. (`bin/codex_pergunta.sh:91–92`; `bin/tests/test_codex_pergunta.sh:140–143`, `:458–460`, `:467`, `:567–568`) Execução e resultado efetivo dos mutantes: **[não verificado]**.

- **O fake timeout não troca o valor nesta cópia:** grava `"$@"` e executa `/usr/bin/timeout "$@"`. O arquivo de captura é apagado antes de cada subcaso, evitando reutilizar argumento velho. (`bin/tests/test_codex_pergunta.sh:115–118`, `:458`, `:462`, `:464`)

- **A prova cobre passagem do argumento, não expiração.** O Codex falso termina imediatamente com rc 0; os casos novos não demonstram rc 124, escalada `-k 30`, teto do construtor, `flock -w`, mensagem da trava ou recusa de papel sem entrada. (`bin/tests/test_codex_pergunta.sh:111`, `:455–467`; `bin/codex_pergunta.sh:383`, `:408`)

- **Há lacunas menores no selo:** somente o primeiro subcaso exige rc 0; os de `600` e default `894` verificam apenas a primeira linha do argv. A mensagem é procurada na saída mesclada, sem provar stderr nem quantidade de linhas. (`bin/tests/test_codex_pergunta.sh:136`, `:459–465`) O caso de domínio exercita somente `abc`, sem testar zero, negativos, frações, sufixos, zeros iniciais ou inteiros muito grandes. (`bin/tests/test_codex_pergunta.sh:467`)

- **Não executar o selo preserva a regra de só leitura:** o próprio script cria diretório temporário, escreve arquivos, reescreve o wrapper copiado para os mutantes e apaga o cenário ao sair. (`bin/tests/test_codex_pergunta.sh:18–19`, `:74–75`, `:585`)

## 12/11 12:32 · sessao 8db6fd841a74 · AUDITORIA-DE-TRAVAS · leitura · origem=Code

As contas batem com a medida fornecida; há furos na semântica do prazo e na cobertura do selo. (`contrato_item6.txt:3–6`; `medida_6_8.r:9–11`; `bin/codex_pergunta.sh:348–408`; `bin/tests/test_codex_pergunta.sh:447–548`)

Nos anexos, conto linhas desde a primeira linha de conteúdo fornecida.

1. **Sítios de tempo no wrapper**

| Sítio | Hoje | Efeito da tabela por papel com cap |
|---|---|---|
| Documentação e atribuição | Segundos, default `900`; `CODEX_TIMEOUT` entra diretamente em `TIMEOUT`. | Muda a origem do valor; o contrato deixa ambíguo o default quando a variável está ausente/vazia. (`bin/codex_pergunta.sh:6`, `:83`; `contrato_item6.txt:3–4`) |
| Validação do papel | Aceita exatamente leitura, censo, verificador, sonda e construtor; demais saem com rc 2. | Esses cinco papéis precisam estar cobertos pela tabela. (`bin/codex_pergunta.sh:81–85`; `contrato_item6.txt:3–4`) |
| Vaga do construtor | `flock -n`: recusa imediata com rc 2. | Não usa `TIMEOUT`; permanece imediato. (`bin/codex_pergunta.sh:239–240`) |
| Gate e slots | Gate bloqueante sem prazo; busca slots imediatamente e repete espera de cinco segundos enquanto não consegue. | A tabela não limita essa espera acumulada. `CODEX_TETO` controla quantidade de slots, não segundos. (`bin/codex_pergunta.sh:84`, `:348–364`) |
| Diagnóstico de espera | Mede desde antes do gate até obter o slot. | Continua medindo espera efetiva; não é o teto da tabela. (`bin/codex_pergunta.sh:350–365`) |
| Trava da resposta | `flock -w "$TIMEOUT"`; mensagem usa `${TIMEOUT}s`; falha sai com rc 3. | Espera e mensagem passam a usar o valor efetivo limitado pelo papel. (`bin/codex_pergunta.sh:374–376`) |
| Execução construtora | `timeout -k 30 "$TIMEOUT" codex …`. | Usa o valor limitado, mantendo os 30 segundos adicionais de escalada. (`bin/codex_pergunta.sh:401–405`) |
| Execução dos quatro papéis de leitura | `timeout "$TIMEOUT" codex …`. | Verificador recebe seu teto; leitura/censo/sonda recebem o teto comum previsto. (`bin/codex_pergunta.sh:408–409`; `contrato_item6.txt:3`) |
| Duração e retorno | `fim` grava papel, rc e duração desde `T0C`; captura o rc da execução, com possíveis substituições posteriores. | Duração observada muda, mas seu significado permanece; o valor efetivo de `TIMEOUT` não é registrado. (`bin/codex_pergunta.sh:252–260`, `:411`, `:433–455`) |

2. **Furos e limites do contrato**

- **Teto da execução não é prazo total do lançador.** Há fila sem limite acumulado; depois, a trava da resposta pode consumir um `TIMEOUT` inteiro e o Codex recebe outro inteiro. Preparação, revalidação e inventário ficam fora do timeout do Codex. Portanto, “até ~32 min para cair” não fecha como limite total do wrapper. (`bin/codex_pergunta.sh:295–319`, `:348–376`, `:391–420`; `aval_respostas_1211_1230.txt:3`)

- **“Estourou = rc 124” precisa de escopo.** Vencer a espera da resposta produz rc **3**. No construtor, recusa do inventário também vence qualquer retorno anterior com rc **3**. (`contrato_item6.txt:4`; `bin/codex_pergunta.sh:376`, `:417–433`)

- **Default não está fechado.** Conservar `${CODEX_TIMEOUT:-900}` e apenas aplicar cap deixa construtor e leitura/censo/sonda em **900** quando não houver override; usar o teto como default produz **1930/1191**. O contrato especifica os tetos e o rebaixamento, mas não explicita essa escolha. (`bin/codex_pergunta.sh:83`; `contrato_item6.txt:3–4`)

- **Papéis atuais estão cobertos pelo texto**, desde que leitura, censo e sonda tenham entradas correspondentes a 1191. Não há papel atual omitido na enumeração; a recusa de papel desconhecido já existe. A futura tabela e sua recusa por ausência de entrada permanecem **[não verificado]**. (`bin/codex_pergunta.sh:85`; `contrato_item6.txt:3–4`)

- **ESMERIL-SEMÂNTICA:** os leitores demonstrados recebem o mesmo valor apresentado como segundos, mas com significados distintos: espera da trava e duração permitida ao processo. Já `duracao_s` inclui preparação e espera; ultrapassar o teto nessa medida não prova estouro do Codex. Leitor externo que suponha outra unidade: **[não verificado]**. O domínio aceito para override — zero, negativos, frações, sufixos ou texto — também não está definido no contrato; hoje o valor é repassado diretamente. (`bin/codex_pergunta.sh:83`, `:252–260`, `:348–408`; `contrato_item6.txt:4`)

- **Há uso documentado acima do teto:** PC2-FIO-MUDO registra tentativa com `CODEX_TIMEOUT=2400`, em papel **censo**; pelo contrato seria rebaixado a **1191**. Dependência atual desse prazo ou necessidade comprovada dos 2400 segundos: **[não verificado]**. O construtor documentado com 1800 fica abaixo de 1930. (`app/docs/RELATO.md:7756`, `:7782`, `:425`; `contrato_item6.txt:3`)

- **A premissa “selo hoje fixa 900” não corresponde à cópia.** O `900` do selo conta worktrees falsas. O falso captura argumentos do **Codex**, não o argumento de `timeout`; os casos e mutantes atuais não cobrem cap. Assim, `3600 → 1191` e mutante sem cap vermelho são exigências futuras, ainda **[não verificado]** por execução. (`bin/tests/test_codex_pergunta.sh:101–111`, `:322–328`, `:447–548`; `contrato_item6.txt:6`)

- **Registro disponível não equivale ao contador contratado.** `fim` contém papel, rc e duração; o contador existente por papel conta linhas `ini`, qualquer rc. A meta de até um rc 124 por papel em 24 horas permanece **[não verificado]** pelo dado fornecido. (`bin/codex_pergunta.sh:253–260`; `bin/codex_limite_estado.py:71–79`; `contrato_item6.txt:5`)

O outro `900`, em `encadeado.sh`, limita espera por carimbos e sai com rc 2; sua manutenção está expressamente autorizada no aval. (`bin/encadeado.sh:71`, `:84–96`; `aval_respostas_1211_1230.txt:3`)

3. **Conferência dos números**

| Papel/grupo medido | p95 + 300 s | Contrato |
|---|---:|---:|
| Construtor | 1630 + 300 = **1930 s** | Confere |
| Verificador | 594 + 300 = **894 s** | Confere |
| Leitura/censo/sonda | 891 + 300 = **1191 s** | Confere |

Fontes das três contas: `medida_6_8.r:9–11`; margem e tetos: `contrato_item6.txt:3`.

A terceira janela contém **20 chamadas do grupo**, não 20 de cada papel: p95 individual de leitura, censo e sonda é **[não verificado]**. Os números conferem para a duração registrada; sua capacidade de evitar futuros rc 124 é **[não verificado]**. (`medida_6_8.r:5–11`; `bin/codex_pergunta.sh:252–260`, `:348–408`)

## 12/11 12:31 · sessao 796afec6fbfb · - · - · origem=Ronald (tmux)

Leitura concluída dos dois arquivos; sem execução de teste ou alteração. [não verificado] [nao verificado]

- **“Selo hoje fixa 900”: falso nesta cópia.** O único `900` do selo é a quantidade de worktrees falsas no caso da lista grande, sem relação com segundos (`bin/tests/test_codex_pergunta.sh:322–328`). O selo captura argv/stdin do `codex` falso, não o argumento de `timeout` (`bin/tests/test_codex_pergunta.sh:101–111`); casos e mutantes atuais não cobrem timeout/cap (`bin/tests/test_codex_pergunta.sh:447–452`, `:474–548`). O caso `3600 → 1191` é exigência futura do anexo, não prova existente (`contrato_item6.txt:6`, numeração do texto fornecido).

- **Prazo global de ~32 min não resulta desses usos.** Há gate bloqueante sem prazo (`bin/codex_pergunta.sh:351`), loop de slots com tentativas de cinco segundos sem limite total (`:353–363`), depois espera da resposta de até `TIMEOUT` (`:376`) e posteriormente outro `TIMEOUT` inteiro para Codex (`:401`, `:408`). Preparação/revalidação/inventário também ficam fora do timeout do Codex (`:295–319`, `:391–399`, `:419–427`). Logo, o teto de 1930 s limita o processo chamado naquela linha; não limita toda a espera do lançador (`:401–411`, `:455`).

- **“Estourou = rc 124 como hoje” precisa de escopo.** Espera vencida da resposta devolve rc 3, não 124 (`bin/codex_pergunta.sh:376`). Inventário recusado vence qualquer rc anterior e também devolve 3 (`:417`, `:433`). O ramo construtor ainda tem tolerância de kill de 30 s (`:401`); 1930 s não é o instante do kill forçado.

- **Default permanece ambíguo no contrato.** Hoje `TIMEOUT=${CODEX_TIMEOUT:-900}` (`bin/codex_pergunta.sh:83`). Aplicar somente `min(default900, teto)` deixa construtor/leitura/censo/sonda em 900 quando o ambiente não define valor; usar o teto da tabela como default muda esse comportamento. O anexo especifica teto e cap, mas não escreve explicitamente a regra para ambiente ausente/vazio (`contrato_item6.txt:3–4`, numeração do texto fornecido).

- **Domínio do override também fica aberto.** Hoje o valor chega sem validação ao `flock` e ao `timeout`, enquanto o diagnóstico o apresenta como segundos (`bin/codex_pergunta.sh:83`, `:376`, `:401`, `:408`). O contrato não define aceitação/rejeição de zero, negativos, frações, sufixos ou texto (`contrato_item6.txt:4`). Portanto, um cap exclusivamente numérico não fecha sozinho a semântica desse campo.

- **Cinco papéis atuais, três classes de teto.** A whitelist contém exatamente leitura/censo/verificador/sonda/construtor e já recusa os demais com rc 2 (`bin/codex_pergunta.sh:85`). O contrato cobre todos somente se `leitura/censo/sonda` significar três chaves mapeadas para 1191, não uma chave literal contendo barras (`contrato_item6.txt:3`). O ramo de execução também agrupa todos os quatro papéis de leitura, embora verificador tenha teto próprio (`bin/codex_pergunta.sh:391–409`).

- **Medida existente não registra timeout efetivo.** A linha `fim` registra papel, rc e duração (`bin/codex_pergunta.sh:253–260`); a duração começa em `T0C`, antes da preparação e das esperas (`:252`, `:295–319`, `:348–376`). O sidecar `.rc` do construtor registra id/rc/fim, sem papel/duração/timeout (`:261–263`). Assim, duração acima do teto não demonstra estouro do `codex exec`; o contador por rc 124 também não inclui espera vencida da resposta nem timeout substituído por recusa de inventário (`:376`, `:433`).

- **Aritmética bate:** 1630+300=1930, 594+300=894, 891+300=1191 (`contrato_item6.txt:3`; `medida_6_8.r:9–11`, numeração do texto fornecido). A terceira medida é do **grupo** leitura/censo/sonda; p95 individual de cada um desses papéis permanece [não verificado]. A medida não demonstra que esses tetos evitarão rc 124 futuros; o próprio intervalo medido inclui preparação/esperas (`bin/codex_pergunta.sh:252`, `:260`, `:348–376`).

## 12/11 12:30 · sessao c1edd9f2c0a0 · LASTRO-FAIXA-SEM-DEFAULT · censo · origem=Code

Há **três casamentos ativos por HH:MM**: classificação de órfãs, exclusão de anomalias e supressão de alertas de turno. Nesta cópia estão em `app/relatorios/pdf_espelho.py:479`, `app/relatorios/pdf_espelho.py:679` e `app/relatorios/pdf_espelho.py:923`. **Colisões efetivamente ocorridas em produção: [nao verificado].**

L-037 determina que o PDF desenhe os dias da tela; L-038 determina que leia o mesmo juiz, sem regra própria de dia (`app/docs/LEIS.md:85`, `app/docs/LEIS.md:86`).

**1. Censo manual**

Nas tabelas, **PERDE-A-DATA** identifica uma operação que permite confundir registros de datas diferentes; não afirma ocorrência em produção. A possibilidade de D/D+1 na mesma linha decorre de todas as batidas reclamadas pelo turno receberem `t.data_turno`, inclusive quando a saída tem outra data civil (`app/ponto/turnos.py:2953`, `app/ponto/turnos.py:322`).

Os produtores comuns aos casamentos são:

| Dado | Produtor e transporte |
|---|---|
| `dia['batidas']` | A tela agrupa pelo dia respondido pelo juiz, ordena por timestamp e publica a lista (`app/ponto/services/espelho.py:580`, `app/ponto/services/espelho.py:582`, `app/ponto/services/espelho.py:665`, `app/ponto/services/espelho.py:776`). O PDF recebe a tela em `app/relatorios/pdf_espelho.py:474` e recorta os dias em `app/relatorios/pdf_espelho.py:562`. |
| `orfas_grade` | A ata transporta strings de horas; o leitor as recupera e anexa ao dia (`app/ponto/services/cartorio.py:736`, `app/escala/services/leitor_celula.py:187`, `app/escala/services/leitor_celula.py:390`, `app/escala/services/leitor_celula.py:420`). A tela seleciona a grade por data, adapta e publica as órfãs (`app/ponto/services/espelho.py:531`, `app/ponto/services/espelho.py:684`, `app/ponto/services/espelho.py:699`, `app/escala/utils.py:1113`, `app/ponto/services/espelho.py:854`). |
| Órfãs pelo builder | Batidas não consumidas são atribuídas pela data civil e convertidas para HH:MM (`app/escala/utils.py:1844`, `app/escala/utils.py:1856`, `app/escala/utils.py:1858`). |
| `dia['turnos']` / `pp.entrada` | O adaptador conserva o timestamp de entrada; o período recebe a entrada localizada completa (`app/ponto/adaptador_motor.py:37`, `app/ponto/adaptador_motor.py:43`, `app/ponto/motor_calculo_v2.py:2751`). A tela associa entrada → pk → dia do juiz e publica os períodos nessa linha (`app/ponto/services/espelho.py:458`, `app/ponto/services/espelho.py:461`, `app/ponto/services/espelho.py:463`, `app/ponto/services/espelho.py:777`). |
| `anomalias_mes` | O adaptador escreve mensagens com **DD/MM HH:MM**, tanto para saída órfã como para entrada aberta (`app/ponto/adaptador_motor.py:35`, `app/ponto/adaptador_motor.py:42`). O motor instala as anomalias no resultado, que o PDF publica (`app/ponto/motor_calculo_v2.py:2354`, `app/ponto/motor_calculo_v2.py:2357`, `app/relatorios/pdf_espelho.py:607`). |

O inventário dos casamentos e das coleções que os sustentam é:

| Local / função | Operandos, chave ou elemento; origem | Classificação e alcance entre dias |
|---|---|---|
| `app/relatorios/pdf_espelho.py:475` — `_coletar_dados_espelho` | Dict: chave `d['data']`; valor `set(d.get('orfas_grade') or [])`. Ambos vêm da tela recebida em `app/relatorios/pdf_espelho.py:474`; produtores acima. | **SEGURA apenas a chave externa**, que particiona por dia da linha. O set interno contém somente horários; essa partição não garante data civil única para as batidas confrontadas em `app/relatorios/pdf_espelho.py:479` (`app/ponto/turnos.py:2953`). |
| `app/relatorios/pdf_espelho.py:479` — `_coletar_dados_espelho` | Esquerda: `localtime(b.timestamp).strftime('%H:%M')`, com `b` de `d['batidas']` em `app/relatorios/pdf_espelho.py:478`. Direita: `_orfas_por_data[d['data']]`, produzido em `app/relatorios/pdf_espelho.py:475`. Operação: `in`. | **PERDE-A-DATA.** Compara hora da batida com strings da grade. A linha pode conter D e D+1; não há comparação da data civil de cada item (`app/ponto/turnos.py:2953`, `app/ponto/turnos.py:322`). |
| `app/relatorios/pdf_espelho.py:675` — `_build_story` | Elemento/chave do set `_hh_esp_periodo`: HH:MM de cada batida `orfa`, varrendo **todos os dias**. Dias vêm de `app/relatorios/pdf_espelho.py:604`; a marca `orfa` nasce em `app/relatorios/pdf_espelho.py:568`. | **PERDE-A-DATA.** O conjunto reúne horários do período inteiro e elimina qualquer distinção de data (`app/relatorios/pdf_espelho.py:676`, `app/relatorios/pdf_espelho.py:677`). |
| `app/relatorios/pdf_espelho.py:679` — `_build_story` | Esquerda: `_hh`, do set produzido em `app/relatorios/pdf_espelho.py:675`. Direita: `str(a)`, de `dados['anomalias_mes']`, publicado em `app/relatorios/pdf_espelho.py:607`. Operação: substring `in`; resultado negado pelo filtro. | **PERDE-A-DATA.** Uma órfã pode excluir uma anomalia de outro dia com o mesmo HH:MM. O produtor fornece DD/MM, mas esse filtro não o compara (`app/ponto/adaptador_motor.py:35`, `app/ponto/adaptador_motor.py:42`). |
| `app/relatorios/pdf_espelho.py:919` — `_build_story` | Elemento/chave do set `_hh_espurias`: HH:MM de `b.timestamp`, selecionando `b.orfa` na linha. Marca produzida em `app/relatorios/pdf_espelho.py:568`; batidas publicadas em `app/ponto/services/espelho.py:776`. | **PERDE-A-DATA.** Mantém a linha, mas descarta a data civil dos instantes usados na próxima comparação (`app/relatorios/pdf_espelho.py:920`, `app/ponto/turnos.py:2953`). |
| `app/relatorios/pdf_espelho.py:923` — `_build_story` | Esquerda: `_abre_em`, HH:MM de `pp.entrada`, produzido em `app/relatorios/pdf_espelho.py:922`. Direita: `_hh_espurias`, produzido em `app/relatorios/pdf_espelho.py:919`. `pp` vem de `dia['turnos']` em `app/relatorios/pdf_espelho.py:921`; produtores acima. | **PERDE-A-DATA.** Igualdade de relógio basta para executar `continue`, suprimindo tanto “turno aberto” como seus alertas; não exige igualdade dos instantes nem das datas civis (`app/relatorios/pdf_espelho.py:928`, `app/relatorios/pdf_espelho.py:929`, `app/relatorios/pdf_espelho.py:931`). Colisão concreta nessa linha: [nao verificado]. |

A guarda `_entrada_espuria` **já compara instantes completos** nesta cópia: `periodo.entrada in orfas_instantes`. Entretanto, o conjunto recebido foi selecionado pelo casamento HH:MM de `:479`; preservar o timestamp depois não corrige uma seleção anterior indevida (`app/relatorios/pdf_espelho.py:178`, `app/relatorios/pdf_espelho.py:478`, `app/relatorios/pdf_espelho.py:479`, `app/relatorios/pdf_espelho.py:516`).

As demais ocorrências de relógio são **SO-EXIBICAO**. Nas formatações abaixo, o segundo operando é o formato literal ou o compositor de texto, não outro registro:

| Local / função | Operand​os e produtores | Classificação |
|---|---|---|
| `app/relatorios/pdf_espelho.py:691`, `app/relatorios/pdf_espelho.py:692` — `_build_story` | `tipo_esc.hora_inicio` e `tipo_esc.hora_fim` × `'%H:%M'`. São `TimeField`; escala selecionada em `app/relatorios/pdf_espelho.py:306`, tipo obtido em `app/relatorios/pdf_espelho.py:307`, transportado em `app/relatorios/pdf_espelho.py:605` e lido em `app/relatorios/pdf_espelho.py:689` (`app/escala/models.py:50`, `app/escala/models.py:51`). | **SO-EXIBICAO**, cabeçalho (`app/relatorios/pdf_espelho.py:699`). |
| `app/relatorios/pdf_espelho.py:840` — `_build_story` | Timestamp das batidas selecionadas em `app/relatorios/pdf_espelho.py:835` × `'%H:%M'`; produtor da lista na tabela anterior. | **SO-EXIBICAO**, entregue a `Marcacoes` (`app/relatorios/pdf_espelho.py:852`). |
| `app/relatorios/pdf_espelho.py:844` — `_build_story` | Timestamp das órfãs selecionadas em `app/relatorios/pdf_espelho.py:836` × `'%H:%M'`. | **SO-EXIBICAO**, texto cinza (`app/relatorios/pdf_espelho.py:855`). |
| `app/relatorios/pdf_espelho.py:866` — `_build_story` | Duas formatações: `_te.hora_inicio` e `_te.hora_fim` × `'%H:%M'`. `_te` vem de `dados['tipo_escala']` em `app/relatorios/pdf_espelho.py:862`; origem em `app/relatorios/pdf_espelho.py:307`. | **SO-EXIBICAO**, coluna Horário (`app/relatorios/pdf_espelho.py:937`). |
| `app/relatorios/pdf_espelho.py:914` — `_build_story` | Timestamp de `_bp`, iterado de `dia['batidas']` em `app/relatorios/pdf_espelho.py:910`, × `'%H:%M'`; combinado com a frase de procedência. | **SO-EXIBICAO**, observação (`app/relatorios/pdf_espelho.py:915`, `app/relatorios/pdf_espelho.py:933`). |
| `app/relatorios/pdf_espelho.py:139`, `app/relatorios/pdf_espelho.py:143`, `app/relatorios/pdf_espelho.py:165` — `Marcacoes._quebrar` / `draw` | `hhmm` recebido por `self.pares` em `app/relatorios/pdf_espelho.py:128`, a partir de `app/relatorios/pdf_espelho.py:840` e `app/relatorios/pdf_espelho.py:852`; usado em medição, transporte e desenho. | **SO-EXIBICAO.** A comparação derivada `usado + ESPACO + w > largura` compara largura gráfica com largura disponível, não identidades de horários (`app/relatorios/pdf_espelho.py:140`). |
| `app/relatorios/pdf_espelho.py:849` — `_build_story` | `_h_isc` × texto `'(i) %s'`; strings produzidas de `c['hora']` em `app/ponto/services/espelho.py:364` e publicadas em `app/ponto/services/espelho.py:871`. | **SO-EXIBICAO**, texto cinza (`app/relatorios/pdf_espelho.py:855`). |
| `app/relatorios/pdf_espelho.py:901` — `_build_story` | Texto de `marco` recebido da tela × compositor da observação. A tela transporta `hora_marco` da grade, sem o PDF fazer casamento (`app/escala/utils.py:1111`, `app/ponto/services/espelho.py:387`). | **SO-EXIBICAO**, observação (`app/relatorios/pdf_espelho.py:900`). |
| `app/relatorios/pdf_espelho.py:680`, `app/relatorios/pdf_espelho.py:932`, `app/relatorios/pdf_espelho.py:933` — `_build_story` | Texto de anomalia `[:40]`, alerta `[:60]` e observação composta `[:120]`; fontes em `app/relatorios/pdf_espelho.py:678`, `app/relatorios/pdf_espelho.py:931` e `app/relatorios/pdf_espelho.py:875`. | **SO-EXIBICAO**: truncamento de textos que podem conter horários; não extraem HH:MM para casamento. |

Há também candidatos nominais que **não são hora do dia**: `_h` compara duração com `0`, minutos com `60` e minutos com `0`; o banco compara saldo com `0`. São decisões de exibição de quantidades (`app/relatorios/pdf_espelho.py:629`, `app/relatorios/pdf_espelho.py:633`, `app/relatorios/pdf_espelho.py:635`, `app/relatorios/pdf_espelho.py:774`).

As ordenações encontradas usam datas de vigência ou datas dos dias, não relógio (`app/relatorios/pdf_espelho.py:325`, `app/relatorios/pdf_espelho.py:363`, `app/relatorios/pdf_espelho.py:461`, `app/relatorios/pdf_espelho.py:462`, `app/relatorios/pdf_espelho.py:563`, `app/relatorios/pdf_espelho.py:578`). Os carimbos de emissão preservam data completa (`app/relatorios/pdf_espelho.py:1006`, `app/relatorios/pdf_espelho.py:1053`). Não encontrei regex, chamada `.time()` ou slicing específico de string HH:MM no PDF; ausência além desta inspeção: [nao verificado].

**2. Script AST**

O script abaixo apenas lê o PDF e imprime candidatos. Propaga dependências por atribuições e iteráveis de comprehensions, alcançando `_hh` em `:679` e `_abre_em` em `:923`. A ponte externa `orfas_grade` é declarada explicitamente porque seu produtor fica fora do PDF (`app/relatorios/pdf_espelho.py:675`, `app/relatorios/pdf_espelho.py:679`, `app/relatorios/pdf_espelho.py:922`, `app/relatorios/pdf_espelho.py:923`, `app/ponto/services/espelho.py:854`).

```python
import ast
from collections import defaultdict
from pathlib import Path

path = Path("app/relatorios/pdf_espelho.py")
source = path.read_text(encoding="utf-8")
tree = ast.parse(source, filename=str(path))
scope = {}


def index(node, owner="<module>"):
    if isinstance(node, (ast.FunctionDef, ast.AsyncFunctionDef, ast.ClassDef)):
        owner += "." + node.name
    scope[node] = owner
    for child in ast.iter_child_nodes(node):
        index(child, owner)


index(tree)
env = defaultdict(set)
# Ponte externa comprovada: espelho.py:854 -> escala/utils.py:1113.
bridge = {"orfas_grade"}


def hourly(name):
    name = name.lower()
    return "hora" in name or "hhmm" in name or name == "hr"


def seed(node, name):
    return {f"{node.lineno}:{name}"}


def deps(node):
    if node is None:
        return set()
    if isinstance(node, ast.Name):
        return env[(scope[node], node.id)] | (
            seed(node, node.id) if hourly(node.id) else set()
        )
    if isinstance(node, ast.Constant):
        return set()
    if isinstance(node, ast.Attribute):
        return deps(node.value) | (
            seed(node, node.attr) if hourly(node.attr) else set()
        )
    if isinstance(node, ast.Subscript):
        key = node.slice
        extra = set()
        if isinstance(key, ast.Constant) and isinstance(key.value, str):
            if hourly(key.value) or key.value in bridge:
                extra = seed(node, key.value)
        return deps(node.value) | deps(key) | extra
    if isinstance(node, ast.Call):
        extra = set()
        if isinstance(node.func, ast.Attribute):
            if node.func.attr == "strftime":
                for arg in node.args:
                    if isinstance(arg, ast.Constant):
                        if isinstance(arg.value, str) and "%H" in arg.value:
                            extra |= seed(node, "strftime " + repr(arg.value))
            if node.func.attr == "get" and node.args:
                key = node.args[0]
                if isinstance(key, ast.Constant) and isinstance(key.value, str):
                    if hourly(key.value) or key.value in bridge:
                        extra |= seed(node, key.value)
        return extra | union(
            [node.func, *node.args, *(k.value for k in node.keywords)]
        )
    if isinstance(node, (ast.ListComp, ast.SetComp, ast.GeneratorExp)):
        # Não confundir dependência da seleção com identidade do elemento.
        return deps(node.elt)
    if isinstance(node, ast.DictComp):
        return deps(node.key) | deps(node.value)
    if isinstance(node, ast.IfExp):
        return deps(node.body) | deps(node.orelse)
    return union(ast.iter_child_nodes(node))


def union(nodes):
    result = set()
    for node in nodes:
        result |= deps(node)
    return result


def bind(target, value):
    if isinstance(target, ast.Name):
        env[(scope[target], target.id)].update(value)
    elif isinstance(target, (ast.Tuple, ast.List)):
        for element in target.elts:
            bind(element, value)
    elif isinstance(target, ast.Starred):
        bind(target.value, value)


while True:
    before = {key: frozenset(value) for key, value in env.items()}
    for node in ast.walk(tree):
        if isinstance(node, ast.Assign):
            for target in node.targets:
                bind(target, deps(node.value))
        elif isinstance(node, (ast.AnnAssign, ast.AugAssign)):
            bind(node.target, deps(node.value))
        elif isinstance(node, (ast.For, ast.AsyncFor, ast.comprehension)):
            bind(node.target, deps(node.iter))
    if before == {key: frozenset(value) for key, value in env.items()}:
        break


def emit(kind, node, *parts):
    if not any(deps(expr) for label, expr in parts):
        return
    print(f"{path}:{node.lineno}:{node.col_offset} {scope[node]} {kind}")
    for label, expr in parts:
        print(
            " ", label, "=", ast.unparse(expr),
            "| fontes:", ", ".join(sorted(deps(expr))) or "-"
        )


nodes = sorted(
    ast.walk(tree),
    key=lambda n: (getattr(n, "lineno", 0), getattr(n, "col_offset", 0)),
)
for node in nodes:
    if isinstance(node, ast.Compare):
        left = node.left
        for op, right in zip(node.ops, node.comparators):
            # In/NotIn são operadores de Compare, não expressões separadas.
            emit(
                "Compare/" + type(op).__name__, node,
                ("esq", left), ("dir", right),
            )
            left = right
    elif isinstance(node, ast.Subscript):
        emit("Subscript", node, ("base", node.value), ("indice", node.slice))
    elif isinstance(node, ast.Dict):
        for key, value in zip(node.keys, node.values):
            if key is not None:
                emit("dict-key/value", node, ("chave", key), ("valor", value))
    elif isinstance(node, ast.DictComp):
        emit(
            "dict-key/value", node,
            ("chave", node.key), ("valor", node.value),
        )
    elif isinstance(node, (ast.SetComp, ast.Set)):
        parts = (
            (("elemento", node.elt),)
            if isinstance(node, ast.SetComp)
            else tuple(("elemento", e) for e in node.elts)
        )
        emit("set-element/key", node, *parts)
```

Limites: é análise conservadora, sem tipos nem execução. Não resolve chamadas entre funções/arquivos, mutações de containers, escrita em atributos, formatos construídos dinamicamente ou identidades escondidas em aliases externos. Une reatribuições e trata comprehensions no escopo da função; pode superestimar dependências. Completude geral: [nao verificado].

Dois falsos positivos são esperados pelo critério solicitado: nomes contendo `hora` incluem durações; `%H` também aparece em carimbos que conservam data. O script lista candidatos, não determina sozinho PERDE-A-DATA/SEGURA (`app/relatorios/pdf_espelho.py:629`, `app/relatorios/pdf_espelho.py:774`, `app/relatorios/pdf_espelho.py:1006`, `app/relatorios/pdf_espelho.py:1053`).

**3. Tela e adaptador**

| Leitor / operação | Casa com data? |
|---|---|
| Períodos por entrada | **Sim, pelo instante completo:** `resultados_por_entrada[p.entrada] = p` (`app/ponto/services/espelho.py:425`). |
| Turnos abertos | **Sim, pelo instante completo:** guarda `p.entrada` e consulta `_p.entrada in _abertos_ts` (`app/ponto/services/espelho.py:427`, `app/ponto/services/espelho.py:476`). |
| Período → dia da tela | **Sim:** timestamp → pk → data do juiz; depois indexa por essa data (`app/ponto/services/espelho.py:458`, `app/ponto/services/espelho.py:461`, `app/ponto/services/espelho.py:463`). |
| Batida → dia / ordenação | **Sim:** agrupa pela data do juiz e ordena pelo timestamp completo (`app/ponto/services/espelho.py:580`, `app/ponto/services/espelho.py:582`, `app/ponto/services/espelho.py:665`). |
| Anomalia → dia inconsistente | **Sim, por `date`:** extrai DD/MM, tenta os anos da janela/navegação, aceita uma data dentro da janela e consulta `data in _datas_anomalia` (`app/ponto/services/espelho.py:250`, `app/ponto/services/espelho.py:252`, `app/ponto/services/espelho.py:258`, `app/ponto/services/espelho.py:261`, `app/ponto/services/espelho.py:668`). A mensagem original não tem ano (`app/ponto/adaptador_motor.py:35`, `app/ponto/adaptador_motor.py:42`). |
| Grade / `orfas_grade` | Seleciona a **grade por data**, mas não reconstrói identidade de batida por HH:MM: repassa as órfãs da grade (`app/ponto/services/espelho.py:684`, `app/ponto/services/espelho.py:699`, `app/escala/utils.py:1113`, `app/ponto/services/espelho.py:854`). |
| Renderização da tela | Apenas imprime `c.hora`, órfãs e, no fallback, timestamp formatado. Não executa os três casamentos do PDF (`app/templates/ponto/espelho.html:237`, `app/templates/ponto/espelho.html:240`, `app/templates/ponto/espelho.html:241`, `app/templates/ponto/espelho.html:246`). |
| `adaptador_motor.periodos_de_turnos` | **Preserva instantes:** períodos usam `(e_ts, s_ts)` e intervalos usam `e_ts` como chave. As mensagens incluem DD/MM HH:MM (`app/ponto/adaptador_motor.py:35`, `app/ponto/adaptador_motor.py:37`, `app/ponto/adaptador_motor.py:38`, `app/ponto/adaptador_motor.py:42`, `app/ponto/adaptador_motor.py:43`, `app/ponto/adaptador_motor.py:44`). |

Existem casamentos `(tipo, hora)` nos adaptadores da grade, delimitados pelo objeto do dia: chamados já selecionados por `data`, e remapeamento cujo dict é recriado para cada dia. Eles operam sobre marcos/células, sem provar identidade de uma batida por data civil (`app/ponto/services/espelho.py:700`, `app/escala/utils.py:1099`, `app/escala/services/leitor_celula.py:284`, `app/escala/services/leitor_celula.py:294`, `app/escala/services/leitor_celula.py:297`).

Portanto, a restrição existente é **dia 

[truncado em 20000 de 20172 caracteres]

## 12/11 12:30 · sessao d079e99aa35c · - · - · origem=Ronald (tmux)

- Leitores de `TIMEOUT` no wrapper: atribuição em `bin/codex_pergunta.sh:83`; espera da resposta e diagnóstico em `:376`; execução construtora em `:401`; demais papéis em `:408`. Os dois comandos recebem o mesmo valor, mas começam esperas distintas; o gate não tem prazo (`:351`) e o laço de slots repete tentativas de cinco segundos sem teto acumulado (`:353–363`).

- O antecedente documental acima do teto é PC2-FIO-MUDO: `CODEX_TIMEOUT=2400` (`app/docs/RELATO.md:7782`), cujo papel é `censo` (`:7756`); com o contrato passaria a1191. Dependência atual desse prazo ou duração necessária: **[não verificado]**. O construtor documentado com1800 fica abaixo1930 (`app/docs/RELATO.md:425`).

- Nos lançadores de teste, as atribuições explícitas são20: `bin/tests/test_codex_limite.sh:24` e `bin/tests/test_disjuntor.sh:1213`. Há `timeout900` externo numa cadeia de teste do parser, sem prova de lançador operacional (`bin/tests/test_disjuntor.sh:563`). Arquivos operacionais `lanca*.sh` e ambiente efetivo da sessão: **[não verificado]**.

- Não encontrei consumidor externo do `TIMEOUT` do wrapper na busca integral da cópia, incluindo ocultos/ignorados e excluindo `.git`; todos os leitores encontrados estão nas linhas acima. A divergência semântica comprovada é a duração registrada: nasce antes da preparação (`bin/codex_pergunta.sh:252`) e termina no trap (`:260`), enquanto `TIMEOUT` limita separadamente o lock e o `codex exec` (`:376,:401,:408`). O valor efetivo não aparece no registro `fim` (`:259–260`).

- O contrato “estourou=124” não cobre toda espera: o lock sai3 (`bin/codex_pergunta.sh:376`); no construtor, inventário recusado também vence o retorno com3 (`:417,:420–425,:433`).

- Nesta cópia, `test_codex_pergunta.sh` **não fixa timeout900**: a literal900 corresponde à fabricação de falsos gitdirs (`bin/tests/test_codex_pergunta.sh:324`); `_roda` não atribui `CODEX_TIMEOUT` (`:124–128`). Nenhuma mordida de cap está demonstrada pelo selo atual (`:124–128`; cap: **[não verificado]**).

## 12/11 12:29 · sessao 223e35d7f556 · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO · sonda · origem=Code

Contando `sonda_leitura:` como linha 1 do dado fornecido:

1. **N = 4 vínculos, 4 colaboradores distintos, 143 células e 51 células futuras.** Linha 4 do dado: `S VINCULOS_GERADORA 4 COLABS 4 CELULAS 143 CELULAS_FUTURAS 51`.

   | Vínculo × colaborador | Células | Futuras | Fonte |
   |---|---:|---:|---|
   | vinc965 × col305 | 92 | 0 | dado:5 |
   | vinc1351 × col476 | 31 | 31 | dado:6 |
   | vinc1353 × col203 | 10 | 10 | dado:7 |
   | vinc1473 × col789 | 10 | 10 | dado:8 |

2. Para **cada um dos quatro**, num dia dentro de `[data_inicio, ∞)`:

   - **`vinculo_do_dia` devolve o próprio vínculo**, quando a célula daquele dia o nomeia como geradora e ele está carregado em `escalas`: esse ramo só verifica os limites de datas; não exige `ativa=True`. Se a geradora faltar na lista, devolve `None`. ([alimentacao.py:269](/home/ronald/codex_ws/096ace9d0f9fe1a939f12e1f303bcb41ddb91e3a/app/escala/alimentacao.py:269))
   - **`vinculo_cobre_a_data` devolve `False`**: desativado e sem `data_fim` faz `tem_vigencia_declarada` retornar `False`, mesmo dentro desse intervalo. ([alimentacao.py:196](/home/ronald/codex_ws/096ace9d0f9fe1a939f12e1f303bcb41ddb91e3a/app/escala/alimentacao.py:196), [alimentacao.py:214](/home/ronald/codex_ws/096ace9d0f9fe1a939f12e1f303bcb41ddb91e3a/app/escala/alimentacao.py:214))

   Portanto, **os 4 vínculos compõem o N de linhas de CADASTRO previsto pelo aval**, quando o vigia passar a ler esse juiz; vinc965 também entra na medida, embora tenha zero células futuras. (dado:4–8; anexo `aval_respostas_1211_1230.txt`, item 1)

3. **Não há erro aritmético aparente:** as quatro linhas somam 143 células e 51 futuras, com quatro vínculos e quatro colaboradores distintos. (dado:4–8) **Erros internos da sonda, completude da consulta e término sem erro: [não verificado]** — o código da sonda e um status de saída não constam do dado apresentado.

## 12/11 12:27 · sessao 8e9e87c033ca · - · - · origem=Ronald (tmux)

Censo do PDF: **três casamentos ativos perdem a data**, com seis pontos de construção/consulta de coleções de apoio. A colisão efetiva em produção é [nao verificado]; a possibilidade decorre do código abaixo. A lei exige ler os dias da tela e o mesmo juiz: `app/docs/LEIS.md:85-86`.

| Local e função | Operandos e produtores | Classificação |
|---|---|---|
| `app/relatorios/pdf_espelho.py:475`, `_coletar_dados_espelho` | Chave `d['data']`; valor `set(d.get('orfas_grade') or [])`. `d` vem de `_espelho(...)` em `:474`; `orfas_grade` é publicado em `app/ponto/services/espelho.py:854`, calculado em `:699-700`, repassando `gd['orfas']` por `app/escala/utils.py:1113`. | **SEGURA somente na chave externa de data**; o set interno conserva apenas HH:MM. Não garante o dia civil de cada batida (`app/ponto/turnos.py:2952-2955`). |
| `app/relatorios/pdf_espelho.py:479`, `_coletar_dados_espelho` | Esquerda: `localtime(b.timestamp).strftime('%H:%M')`, com `b` vindo de `d['batidas']` em `:478`. Direita: `_orfas_por_data[d['data']]`, construído em `:475`. | **PERDE-A-DATA**: restringe à linha do turno, que pode conter batidas de D e D+1; todas as batidas de um turno recebem `t.data_turno` em `app/ponto/turnos.py:2952-2953`; a saída pode ter data civil distinta em `:322`. O matching não distingue esses dias nem segundos. |
| `app/relatorios/pdf_espelho.py:675-677`, `_build_story` | Chave de set: HH:MM de `b.timestamp`, para toda batida marcada `orfa` de todos os dias. `dados['dias']` vem do recorte em `:562` e retorno em `:604`; `b.orfa` é atribuído em `:568`. | **PERDE-A-DATA**: o set reúne o período inteiro sem data. |
| `app/relatorios/pdf_espelho.py:679`, `_build_story` | `_hh` do set em `:675`; `str(a)` de `dados['anomalias_mes']`, publicado de `resultado.anomalias` em `:607`. | **PERDE-A-DATA**: substring HH:MM de órfã de qualquer dia elimina anomalia de outro dia. Não é regex; é `in` textual. Anomalias incluem data no produtor `app/ponto/adaptador_motor.py:33-35` e `:40-42`, mas o PDF não a compara. |
| `app/relatorios/pdf_espelho.py:919-920`, `_build_story` | Chave de set: `localtime(b.timestamp).strftime('%H:%M')`, de batidas órfãs da linha atual. Marca produzida em `:568`; linha produzida em `app/ponto/services/espelho.py:774-777`. | **PERDE-A-DATA**: set da linha perde o instante; linha do turno admite D+1 (`app/ponto/turnos.py:2952-2953`). |
| `app/relatorios/pdf_espelho.py:923`, `_build_story` | `_abre_em`, HH:MM de `pp.entrada` em `:922`; `_hh_espurias`, set de `:919`. `pp` vem de `dia['turnos']` em `:921`; tela associa períodos por instante→pk→dia em `app/ponto/services/espelho.py:454-463`, publica `turnos_dia` em `:777`. | **PERDE-A-DATA**: uma órfã de D+1 com mesmo HH:MM da entrada de D pode suprimir `turno aberto` e todos os alertas em `:923-932`. Ocorrência concreta [nao verificado]. |

A cadeia normal de órfãs perde o instante antes de chegar ao PDF: `app/ponto/services/cartorio.py:736` serializa `dia['orfas']` como strings; `app/escala/services/leitor_celula.py:187` lê essas strings, `:390` cria a linha com data, `:420` anexa as órfãs; a tela chama esse leitor em `app/ponto/services/espelho.py:524-525` e indexa a data da linha em `:529-532`. O caminho builder também produz HH:MM sem data em `app/escala/utils.py:1856-1858`, depois de atribuir a órfã pela data civil em `:1844`.

Operações próximas que **preservam o instante**: `_entrada_espuria` compara `periodo.entrada in orfas_instantes` em `app/relatorios/pdf_espelho.py:178`; o set recebe `b.timestamp` em `:478`; a classificação usa `_b.timestamp in _orfas_instantes` em `:568`; os turnos abertos guardam `p.entrada` em `:518` e consultam entrada completa em `:576`. São comparações estruturalmente seguras quanto à data, mas o conteúdo de `_orfas_instantes` já depende do casamento defeituoso em `:479`.

Formatação **SO-EXIBICAO**, sem casar registros:

- `app/relatorios/pdf_espelho.py:691-692`: início/fim da escala do cabeçalho; `tipo_escala` nasce em `:307`, retorna em `:605` e é lido em `:689`.
- `app/relatorios/pdf_espelho.py:840-841`: horários das batidas não órfãs selecionadas em `:835`; entrega ao Flowable em `:852`.
- `app/relatorios/pdf_espelho.py:844-845`: horários das órfãs selecionadas em `:836`, usados no texto cinza.
- `app/relatorios/pdf_espelho.py:866`: início/fim de `dados['tipo_escala']`, lido em `:862`.
- `app/relatorios/pdf_espelho.py:914-915`: horário da batida como prefixo da procedência, vindo de `dia['batidas']` em `:910`.
- `app/relatorios/pdf_espelho.py:139` e `:165`: `hhmm` recebido em `Marcacoes.pares` (`:128`, chamado em `:852`) é medido e desenhado. A comparação de `:140` é de largura gráfica, não de horários.
- `app/relatorios/pdf_espelho.py:849`: strings de `celulas_isencao` são impressas; o PDF não as usa para casamento.

Os slices de `app/relatorios/pdf_espelho.py:680`, `:932` e `:933` truncam texto de anomalias/alertas/observações; **SO-EXIBICAO**, posteriores ao casamento. `:763` fatia badges, não horários. As ordenações são por datas (`:325`, `:363`, `:461-462`, `:563`, `:578`), não HH:MM. Não há regex, criação/consulta de objeto `time` nem slicing de string de hora no PDF lido (`app/relatorios/pdf_espelho.py:1-1073`).

`_h` opera durações, não hora do dia: comparações com zero em `app/relatorios/pdf_espelho.py:629`, com 60 em `:633` e com zero em `:635`; **SO-EXIBICAO**. Similarmente `saldo_banco_horas != 0` em `:774` compara quantidade, não relógio. Os carimbos em `:1006` e `:1053` incluem data completa.

## 12/11 12:27 · sessao 41ddbe2848a5 · - · - · origem=Ronald (tmux)

Para o script, as armadilhas comprovadas nesta cópia são:

- `ast.In` aparece em `Compare.ops`, sem linha própria; a ocorrência deve ser reportada como `Compare`, mostrando os dois operandos. As três comparações relevantes estão em `app/relatorios/pdf_espelho.py:479`, `app/relatorios/pdf_espelho.py:679` e `app/relatorios/pdf_espelho.py:923`.
- `_hh` exige propagação do iterável da comprehension: o conjunto nasce de `strftime` em `app/relatorios/pdf_espelho.py:675` e alimenta `for _hh in _hh_esp_periodo` em `app/relatorios/pdf_espelho.py:679`.
- `_abre_em` exige propagação pela atribuição de `app/relatorios/pdf_espelho.py:922` até a comparação em `app/relatorios/pdf_espelho.py:923`.
- Um `SetComp` deve herdar a derivação de `elt`, sem herdar automaticamente os testes `ifs`: `app/relatorios/pdf_espelho.py:477` produz timestamps completos; sua guarda perde a data em `app/relatorios/pdf_espelho.py:479`.
- Um `DictComp` deve analisar chave e valor separadamente: em `app/relatorios/pdf_espelho.py:475`, a chave é `d['data']`, embora o valor contenha horários.
- A regra nominal `"hora"` inclui durações e produz candidatos sem relação com data: `horas == 0` em `app/relatorios/pdf_espelho.py:629` e saldo do banco em `app/relatorios/pdf_espelho.py:774`.
- Buscar `"%H"` como substring também captura formatos com data preservada em `app/relatorios/pdf_espelho.py:1006` e `app/relatorios/pdf_espelho.py:1053`.
- AST local não resolve produtores interarquivo, tipos em execução, contratos do dia do turno nem ocorrência real em produção: [nao verificado].

Censo direto de `strftime` sem data: `app/relatorios/pdf_espelho.py:479`, `:675`, `:691`, `:692`, `:840`, `:844`, `:866` (duas chamadas), `:914`, `:919` e `:922`. A única chamada `.sort` usa data em `app/relatorios/pdf_espelho.py:563`; não encontrei chamada `.time()` ou regex no arquivo, mediante inspeção AST integral [nao verificado: evidência negativa sem linha única].

Os snippets somente leram o arquivo e escreveram em stdout. Nesta cópia, usar `python3`; o executável `python` não foi encontrado [nao verificado: condição do ambiente, sem arquivo:linha].

## 12/11 12:27 · sessao 16f591caa689 · - · - · origem=Ronald (tmux)

Leis: L-037 manda PDF desenhar `dias` da tela e L-038 proíbe regra própria de dia no PDF (`app/docs/LEIS.md:85-86`).

A tela preserva data/instante nos cruzamentos principais:

| Campo/operação | Produtor e uso | Conclusão |
|---|---|---|
| Período por entrada | `resultados_por_entrada[p.entrada] = p`, `app/ponto/services/espelho.py:424-425` | Chave é datetime completo, não HH:MM. |
| Turno aberto | `_abertos_ts.add(p.entrada)`, `app/ponto/services/espelho.py:426-427`; teste `_p.entrada in _abertos_ts`, `:476` | Compara instantes completos. |
| Batida → linha do dia | timestamp → pk em `app/ponto/services/espelho.py:455-458`; pk → dia do juiz em `:460-463`; batida agrupada por dia em `:579-582` | Casa com data pelo juiz. |
| Ordem das batidas | `sorted(..., key=lambda b: b.timestamp)`, `app/ponto/services/espelho.py:665` | Ordena datetime completo. |
| Anomalia → dia | parser DD/MM, anos candidatos e filtro da janela em `app/ponto/services/espelho.py:250-263`; consumo `data in _datas_anomalia` em `:668` | Casa por `date`, não HH:MM. Ano recuperado pela janela; mensagem sem ano em `app/ponto/adaptador_motor.py:35,42`. |
| Grade do dia | leitura e mapa `_grade_por_data[_d]` em `app/ponto/services/espelho.py:524-532`; acesso `_grade_por_data.get(data)` em `:684` | Seleção por data do dia/turno. |
| Órfãs apresentadas | `montar_realizado(_gd, ..., _ch_por_data.get(data))`, `app/ponto/services/espelho.py:699-700`; retorna `list(gd.get('orfas') or [])`, `app/escala/utils.py:1113`; publica `orfas_grade`, `app/ponto/services/espelho.py:854` | Lista HH:MM já pertencente à grade; tela não a cruza novamente com timestamp da batida. |
| Renderização órfã | `for o in dia.orfas_grade`, imprime `o`, `app/templates/ponto/espelho.html:240-241` | Só exibição. |
| Renderização marcação | imprime `c.hora`, `app/templates/ponto/espelho.html:232-237`; fallback `b.timestamp|date:"H:i"`, `:244-246` | Só exibição. |

Cadeia de produção de `orfas_grade`: ata grava strings de horas em `app/ponto/services/cartorio.py:736`; leitor repassa essas strings em `app/escala/services/leitor_celula.py:187-190`; anexa a uma grade cuja data é `_cur` em `:390` e cujas órfãs vêm de `_orfas_da_ata(ata)` em `:420`; tela indexa a grade por data em `app/ponto/services/espelho.py:528-532`; adaptador retorna as órfãs sem remontagem em `app/escala/utils.py:1113`; tela publica em `app/ponto/services/espelho.py:854`.

**Limite essencial:** “mesma linha da tela” não implica “mesma data civil”. O juiz põe todas as batidas reclamadas pelo turno em `t.data_turno` (`app/ponto/turnos.py:2949-2953`); as demais recebem a data civil (`:2954-2955`). Assim, uma linha do turno pode conter D e D+1; a saída do cross-meia-noite de `fim` pode morar em `fim+1` (`:2907-2908`). Logo o HH:MM da lista de órfãs não é identificador inequívoco de uma batida civil dessa linha (`app/escala/utils.py:1113`; `app/ponto/turnos.py:2953`).

O adaptador preserva datas nos dados estruturados: `e_ts = t.entrada.timestamp`, `s_ts = t.saida.timestamp`, períodos `(e_ts,s_ts)` e intervalos chaveados por `e_ts` (`app/ponto/adaptador_motor.py:37-44`). Textos de anomalia também incluem DD/MM HH:MM: órfã em `:33-35`, aberta em `:40-42`. Motor consome o adaptador em `app/ponto/motor_calculo_v2.py:530`, instala anomalias em `:2354-2357`, e seus períodos guardam `localizar(entrada)` completo, por exemplo em `:2751`, `:2878`, `:3216`.

Há comparações HH:MM dentro de adaptadores da grade, porém delimitadas pelo objeto do dia: chamado com mesmo tipo/hora em `app/escala/utils.py:1093-1103`, alimentado com `_ch_por_data.get(data)` pela tela em `app/ponto/services/espelho.py:699-700`; remapeamento de células `(tipo,hora)` reinicializa `por_chave` para cada `d` em `app/escala/services/leitor_celula.py:284-298`. Elas não provam identidade de uma batida por data civil; operam sobre marcos/células do dia indicado nesses laços.

## 12/11 12:19 · sessao 856fc304b1e9 · AUDITORIA-DE-TRAVAS-CORTE · verificador · origem=Code

REPROVADO

O delta cura o truncamento e reforça as transições dos casos 5 e 8. Persistem as divergências com o aval literal e a compatibilidade descoberta da cota antiga; além disso, um ramo do caso 6 ainda aceita aviso ausente. (`bin/disjuntor.py:218`, `bin/disjuntor.py:1031`, `bin/disjuntor.py:1185`; `bin/tests/test_disjuntor.sh:1222`, `bin/tests/test_disjuntor.sh:1250`, `bin/tests/test_disjuntor.sh:1306`)

Referências aos anexos contam desde a primeira linha de conteúdo. Execução local do selo e confirmação do intervalo pelo histórico Git: [nao verificado]. As mordidas abaixo são as do log fornecido.

**1. Destino de cada achado procedente da volta 2**

| Achado | O delta cura? | O selo morde? |
|---|---|---|
| Aviso truncado em nomes longos | **Sim.** DISJUNTOR/SOBE recebem `inteiro=True`; somente a linha livre final do SOBE fica limitada a 200 caracteres. (`bin/disjuntor.py:153`, `bin/disjuntor.py:218`, `bin/disjuntor.py:1062`, `bin/disjuntor.py:1081`) | **Sim:** `B3-aviso-cortado`, `B3-sobe-cortado` e `B3-sobe-linha-sem-limite` caem no caso 6. (`item2v3_mutantes.log:30`, `item2v3_mutantes.log:45`, `item2v3_mutantes.log:60`) |
| Testemunha positiva da medida `lib['auto']` | Já estava curado; permanece exigindo trilha positiva, igualdade com o medidor e apresentação correspondente. (`bin/tests/test_disjuntor.sh:1284`, `bin/tests/test_disjuntor.sh:1292`) | Mutante continua declarado; sua execução nesta volta: [nao verificado]. (`bin/tests/test_disjuntor.sh:2036`) |
| Contrato FALHA × SOBE | Já estava conciliado: contrato pede SOBE e código produz SOBE. (`contrato_item2.txt:4`; `bin/disjuntor.py:1060`) | Caso 2 exige exatamente um SOBE; mutante específico FALHA/SOBE não consta entre os onze do item. (`bin/tests/test_disjuntor.sh:1195`, `bin/tests/test_disjuntor.sh:2031`, `bin/tests/test_disjuntor.sh:2049`) |
| Cota e medida com nomes distintos | Mantido: `auto_usada` governa a cota; `lib['auto']` conta eventos em 24 h. (`bin/disjuntor.py:1185`, `bin/disjuntor.py:2029`, `bin/disjuntor.py:2033`, `bin/disjuntor.py:2058`) | Há verificações de ambas; a compatibilidade com a chave antiga continua sem caso/mutante específico. (`bin/tests/test_disjuntor.sh:1179`, `bin/tests/test_disjuntor.sh:1292`, `bin/tests/test_disjuntor.sh:2031`, `bin/tests/test_disjuntor.sh:2049`) |
| Cota herdada em `r['auto']` ignorada | **Não.** O predicado continua lendo somente `auto_usada`; a migração existente trata o limiar, sem converter aquela chave. Existência desse estado em produção: [nao verificado]. (`bin/disjuntor.py:815`, `bin/disjuntor.py:832`, `bin/disjuntor.py:1185`) | Não há mordida demonstrada para esse caminho. (`bin/tests/test_disjuntor.sh:2031`, `bin/tests/test_disjuntor.sh:2049`) |
| Precedência das portas antigas | Já estava curado; mantém ordenação estável antes de AUTO. (`bin/disjuntor.py:1050`) | Caso 8 foi reforçado; mutante de precedência continua declarado, mas sua execução nesta volta: [nao verificado]. (`bin/tests/test_disjuntor.sh:1306`, `bin/tests/test_disjuntor.sh:2037`) |
| Disparo não condicionado literalmente a DESENHO | **Não.** Continua disparando pelo número de reprovações/voltas e retornando PARADA antes de examinar liberações. A classe só entra na porta posterior. (`aval_auditoria_de_travas_corte_1211.txt:4`; `bin/disjuntor.py:1031`, `bin/disjuntor.py:1040`, `bin/disjuntor.py:1074` [correção: validação da classe em `bin/disjuntor.py:774`]) | Não há mutante declarado que cobre essa divergência entre aval e contrato. (`contrato_item2.txt:2`; `bin/tests/test_disjuntor.sh:2031`, `bin/tests/test_disjuntor.sh:2049`) |
| Segunda ocorrência pode sair pela porta antiga sem autoria Ronald | **Não.** ERRO-BOBO exige motivo, sem conferir autor; a cota restringe apenas AUTO. Isso preserva o contrato, mas mantém a divergência com o aval literal. (`contrato_item2.txt:2`; `aval_auditoria_de_travas_corte_1211.txt:4`; `bin/disjuntor.py:768`, `bin/disjuntor.py:1057`) | O caso 5 exige que essa porta continue liberando com cota gasta. (`bin/tests/test_disjuntor.sh:1227`, `bin/tests/test_disjuntor.sh:1230`) |
| Caso 5 podia passar sem provar liberação | **Sim.** Agora exige estado previamente disparado, LIBERA do tipo correto acrescentada e preservação da cota. (`bin/tests/test_disjuntor.sh:1220`, `bin/tests/test_disjuntor.sh:1228`, `bin/tests/test_disjuntor.sh:1232`) | **Sim:** `B3-c5-solta-sem-libera` passa a derrubar também o caso 5. Antes já derrubava o caso 6; não era sobrevivente da suíte inteira. (`item2v3_mutantes.log:67`, `item2v3_mutantes.log:73`) |
| Caso 8 podia passar sem segundo disparo/liberação final | **Sim.** Exige segundo disparo, evento AUTO e cota unitária finais. (`bin/tests/test_disjuntor.sh:1300`, `bin/tests/test_disjuntor.sh:1306`) | **Sim:** `B3-c8-sem-2o-disparo` passa de rc=0 para rc=8. (`item2v3_mutantes.log:81`, `item2v3_mutantes.log:89`) |
| Relógio fora da borda | **Não; permanece herdado.** Há `time.time()` dentro de `decide_b3` e `datetime.now()` dentro de `trilha`. (`bin/disjuntor.py:999`, `bin/disjuntor.py:217`) | Os cinco mutantes novos não atacam isso. (`bin/tests/test_disjuntor.sh:2041`, `bin/tests/test_disjuntor.sh:2049`) |

**2. Quem escreve e quem lê; detalhe maior que 300**

| Campo/valor | Quem escreve/produz | Quem lê |
|---|---|---|
| Parâmetro `inteiro` | Padrão `False` na assinatura; `_anuncia` fornece o resultado de `ev in B3_AVISO_INTEIRO`. O espião encaminha `**kw`. (`bin/disjuntor.py:210`, `bin/disjuntor.py:1081`; `bin/tests/test_disjuntor.sh:1077`) | `trilha` escolhe corte em 300 ou ausência desse corte. O parâmetro não é persistido na linha. (`bin/disjuntor.py:217`, `bin/disjuntor.py:218`) |
| `B3_AVISO_INTEIRO` | Constante `('DISJUNTOR', 'SOBE')`. (`bin/disjuntor.py:153`) | `_anuncia`; os mutantes retiram o encaminhamento ou SOBE da tupla. (`bin/disjuntor.py:1081`; `bin/tests/test_disjuntor.sh:2041`, `bin/tests/test_disjuntor.sh:2042`) |
| `B3_SOBE_LINHA` | Constante `200`. (`bin/disjuntor.py:154`) | Montagem do SOBE e assert do sufixo limitado; mutante remove o corte. (`bin/disjuntor.py:1062`; `bin/tests/test_disjuntor.sh:1278`, `bin/tests/test_disjuntor.sh:2043`) |
| Detalhe DISJUNTOR B3 | `decide_b3` monta obra, motivo e recado; `_anuncia → trilha` grava após persistência, normalizando espaços. (`bin/disjuntor.py:1036`, `bin/disjuntor.py:1038`, `bin/disjuntor.py:1081`, `bin/disjuntor.py:217`) | `ler()` conta pelo prefixo; selo procura o aviso completo e observa evento/trava. (`bin/disjuntor.py:2059`; `bin/tests/test_disjuntor.sh:1234`, `bin/tests/test_disjuntor.sh:1265`, `bin/tests/test_disjuntor.sh:1086`) |
| Detalhe SOBE B3 | `decide_b3` monta obra, SHA, cota, instrução e linha limitada ao final; passa pela mesma porta de anúncio. (`bin/disjuntor.py:1060`, `bin/disjuntor.py:1062`, `bin/disjuntor.py:1081`) | `conta()` e listas `sobe/sobe2` do selo. `ler()` não contabiliza SOBE. (`bin/tests/test_disjuntor.sh:1165`, `bin/tests/test_disjuntor.sh:1261`, `bin/tests/test_disjuntor.sh:1276`; `bin/disjuntor.py:2035`) |

**Os leitores identificados em `bin/` continuam compatíveis com o detalhe ampliado:**

- `ler()` percorre linhas completas, filtra pelo token do evento e conta DISJUNTOR usando `p[3]`; o sufixo longo não muda essa posição. (`bin/disjuntor.py:2039`, `bin/disjuntor.py:2041`, `bin/disjuntor.py:2060`)
- `_censo_24h()` também percorre linhas completas; seu regex ancorado descarta DISJUNTOR/SOBE. (`bin/disjuntor.py:2105`, `bin/disjuntor.py:2147`, `bin/disjuntor.py:2149`)
- ESTADO e CLI usam `ler/linha`. A referência do hook ao caminho apenas o exclui como prova de espera. (`bin/gerar_estado.py:280`, `bin/gerar_estado.py:281`; `bin/disjuntor.py:2403`; `bin/hook_stop_fila1.py:666`)
- Os selos usam leitura integral por linha, `grep` ou `tail`, sem teto de 300. Isso inclui o selo de censo fora do diff. (`bin/tests/test_disjuntor.sh:96`, `bin/tests/test_disjuntor.sh:1167`, `bin/tests/test_disjuntor.sh:1236`; `bin/tests/test_censo_antes_da_vaga.sh:158`, `bin/tests/test_censo_antes_da_vaga.sh:525`)

**3. Segunda AUTO, DESENHO, rearme e reinício**

- **Segunda AUTO sozinha, com `auto_usada` preservada: não libera.** Registra SHA recusado em `auto_sobe`, deduplica SOBE e continua sem consumir a linha. (`bin/disjuntor.py:1057`, `bin/disjuntor.py:1059`, `bin/disjuntor.py:1063`, `bin/disjuntor.py:1069`)
- **Com estado antigo apenas em `auto`: pode aceitar outra AUTO.** Ausência de `auto_usada` é lida como zero, conforme o próprio contrato. Ocorrência em produção: [nao verificado]. (`bin/disjuntor.py:1185`, `bin/disjuntor.py:1065`; `contrato_item2.txt:4`)
- **`classe=DESENHO` não libera pela porta nova.** Um motivo dizendo “erro de desenho” sob rótulo FORMA pode passar: o reconhecedor valida o rótulo e motivo alfanumérico. Classificação real em produção: [nao verificado]. (`bin/disjuntor.py:148`, `bin/disjuntor.py:774`)
- **Rearme e reinício com JSON preservado não zeram a cota.** Rearme altera outros campos; o carregamento seguinte recupera o objeto persistido. Arquivo ausente inicia `{}`. (`bin/disjuntor.py:1148`, `bin/disjuntor.py:1150`, `bin/disjuntor.py:377`, `bin/disjuntor.py:380`)
- **Uma linha antiga ERRO-BOBO pode liberar com cota gasta**, sem comprovar autoria Ronald; isso não é uma segunda AUTO aceita. (`bin/disjuntor.py:768`, `bin/disjuntor.py:1050`, `bin/disjuntor.py:1057`)

**4. Espião do caso 5 de `b3_v4`**

**Sim, preserva a prova anterior:** tenta adquirir a trava por outro descritor, registra se estava ocupada e exige presença de DISJUNTOR/LIBERA/REARMA antes do `all(...)`. `**kw` é encaminhado intacto à função original. Não prova SOBE nem verifica diretamente o valor de `inteiro`. (`bin/tests/test_disjuntor.sh:1067`, `bin/tests/test_disjuntor.sh:1071`, `bin/tests/test_disjuntor.sh:1077`, `bin/tests/test_disjuntor.sh:1086`, `bin/tests/test_disjuntor.sh:1087`)

**5. Assert vazio e mordida dos mutantes**

**Casos 5 e 8:** os vazios apontados foram curados pelas testemunhas de transição descritas acima. (`bin/tests/test_disjuntor.sh:1222`, `bin/tests/test_disjuntor.sh:1228`, `bin/tests/test_disjuntor.sh:1306`)

**Caso 6: ainda há ramo vazio.** O retorno de `dispara('OBRA-F')` é ignorado; `ultima()` pode retornar `''`. Nesse caso, “`AUTO ja usada` ausente” passa, e `mensagem_b3` ainda ensina AUTOLIBERA sem exigir registro da obra. Portanto esse ramo aceita ausência de disparo/aviso F. As provas novas de textos longos exigem eventos positivos e não passam integralmente vazias. (`bin/tests/test_disjuntor.sh:1240`, `bin/tests/test_disjuntor.sh:1245`, `bin/tests/test_disjuntor.sh:1250`, `bin/tests/test_disjuntor.sh:1265`, `bin/tests/test_disjuntor.sh:1278`; `bin/disjuntor.py:1202`)

| Mutante novo | Mordida no log final |
|---|---|
| `B3-aviso-cortado` | Caso 6, rc=6. (`item2v3_mutantes.log:30`, `item2v3_mutantes.log:31`) |
| `B3-sobe-cortado` | Caso 6, rc=6. (`item2v3_mutantes.log:45`, `item2v3_mutantes.log:46`) |
| `B3-sobe-linha-sem-limite` | Caso 6, rc=6. (`item2v3_mutantes.log:60`, `item2v3_mutantes.log:61`) |
| `B3-c5-solta-sem-libera` | Casos 5/6, rc=5. (`item2v3_mutantes.log:73`, `item2v3_mutantes.log:74`) |
| `B3-c8-sem-2o-disparo` | Caso 8, rc=8. (`item2v3_mutantes.log:88`, `item2v3_mutantes.log:89`) |

**Nenhum dos cinco novos sobrevive no log; o controle passa.** A execução nesta volta dos seis antigos e de `b3_v4`: [nao verificado]. (`item2v3_mutantes.log:15`, `item2v3_mutantes.log:16`; `bin/tests/test_disjuntor.sh:2031`, `bin/tests/test_disjuntor.sh:2039`)

**Contrato:** continuam oito casos numerados, agora com mais cenários internos; há onze mutantes do item. “8 casos” permanece correto; “6 mutantes” virou divergência documental relevante para rastrear cobertura, sem alterar por si só a regra operacional. (`contrato_item2.txt:5`; `bin/tests/test_disjuntor.sh:1180`, `bin/tests/test_disjuntor.sh:1307`, `bin/tests/test_disjuntor.sh:2031`, `bin/tests/test_disjuntor.sh:2049`)

**6. Classes de reprovação**

| Classe | Resultado |
|---|---|
| Segundo escritor fora da porta | Não identificado para a cota operacional: acréscimo específico em `decide_b3`, persistência por `_com_estado`. (`bin/disjuntor.py:1065`, `bin/disjuntor.py:1089`, `bin/disjuntor.py:390`) |
| Relógio fora da borda | **Presente, herdado:** fallback em `decide_b3`; wrapper chega sem instante explícito; `trilha` captura seu próprio horário. (`bin/disjuntor.py:999`, `bin/disjuntor.py:1172`, `bin/disjuntor.py:217`; `bin/codex_pergunta.sh:378`) |
| Selo sem mordida | Nenhum sobrevivente entre os cinco novos demonstrados; permanece a checagem vazia de aviso F. Mordida atual dos seis antigos: [nao verificado]. (`item2v3_mutantes.log:30`, `item2v3_mutantes.log:45`, `item2v3_mutantes.log:60`, `item2v3_mutantes.log:73`, `item2v3_mutantes.log:88`; `bin/tests/test_disjuntor.sh:1250`) |
| Campo com dois significados | Não nos campos atuais: cota e medida permanecem distintas. A chave antiga continua sem conversão. (`bin/disjuntor.py:1185`, `bin/disjuntor.py:2033`, `bin/disjuntor.py:2058`, `bin/disjuntor.py:815`) |
| Ordem de trava | Não identifiquei inversão introduzida: gravação e anúncio permanecem sob a trava do estado; wrapper/triagem tomam resposta antes de estado. (`bin/disjuntor.py:370`, `bin/disjuntor.py:391`, `bin/disjuntor.py:402`, `bin/disjuntor.py:405`, `bin/disjuntor.py:908`, `bin/disjuntor.py:940`; `bin/codex_pergunta.sh:376`, `bin/codex_pergunta.sh:378`) |

_4248 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
