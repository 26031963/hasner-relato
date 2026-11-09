# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 07:48** · publicado em **09/11/2026 07:48** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 07:48 · sessao 652ccdb4046d · - · - · origem=Ronald (tmux)

B/C concluídos em leitura; scripts e referências enviados ao agente principal.

- Ausência: cobertura `app/ponto/turnos.py:2511`, `:2735`; grade `app/escala/utils.py:1188`; Resolver `app/ponto/services/veredito_celula.py:237`; motor `app/ponto/services/fechamento.py:683`, `:751`; desconto `app/ponto/motor_calculo_v2.py:1085`; abono diário `app/ponto/services/dia_pago.py:364`; lavra `app/ponto/services/fechamento.py:1191`; exportação `app/folha/export.py:717`.
- Caso col39/15-10 aparece como precedente da regra que abona apenas o previsto não coberto pelas batidas, sem demonstrar o estado atual: `app/ponto/services/dia_pago.py:375`.
- Pareamento: entrada pela porta `app/ponto/turnos.py:2846`, alimentação canônica `:1918`, chamada ao pareador `:1992`.
- Espúrias: detecção `app/ponto/management/commands/detectar_par_relampago.py:63`, guardas `:158`, simulação `:229`, pares embutidos `:275`.
- Geo: juiz puro `app/ponto/services/geofence.py:95`; escritor com alertas/push não deve ser chamado na sonda, `:232`.
- Dono: usar `InsumosDoDono.dono`, que chama `dono_da_divergencia`, sem reproduzir seus critérios: `app/ponto/management/commands/e6_oraculo.py:182`, `:53`.
- Donos concretos e ponto atual da perda de cobertura dos alvos: [não verificado].
- Padrões candidatos quando comprovada ESTRUTURA: Single Source of Truth e CQRS com projeções reconstruíveis, `app/docs/GLOSSARIO.md:11`, `:22`.

## 09/11 07:47 · sessao b18df0b7c494 · - · - · origem=Ronald (tmux)

D — achados e sonda para consolidação. Nenhum arquivo criado e nenhum acesso ao banco executado.

1. O “8” é soma de unidades diferentes: chamados de premissa morta + perguntas absolvidas + chamados com rejunção pendente + chamados absolvidos pela célula + eventos mortos + chamados cujo juiz falhou. Não é contagem distinta de chamados. Fonte: `app/chamados/management/commands/reconciliar_chamados.py:118`.

2. No último trecho fornecido, `8 = 2 perguntas absolvidas + 6 chamados absolvidos pela célula`; o `16` anterior era `1 + 15`. Esses números vêm exclusivamente do anexo colado, sem arquivo correspondente na cópia: **[não verificado]**.

3. O vigia “premissa morta” desconta quem tem evento pendente por colaborador e exclui retenção/fecho recusado; “célula absolveu” apenas verifica ausência de evento pendente da célula e **não desconta retenção nem fecho recusado**. Portanto, parte dos seis pode ser retenção legítima, sem defeito de entrega. Fontes: `app/chamados/management/commands/reconciliar_chamados.py:62`, `app/chamados/management/commands/reconciliar_chamados.py:73`, `app/chamados/management/commands/reconciliar_chamados.py:79`, `app/chamados/reconciliador.py:1269`.

| Subclasse | Evento/consumidor e alcance demonstrado |
|---|---|
| Premissa morta | Recompute que remove um turno aberto emite pelo mesmo atomic, condicionado a existir célula: `app/ponto/nucleo.py:60`, `app/ponto/nucleo.py:86`. Nascimento de chamado de turno também emite: `app/chamados/models.py:974`, `app/chamados/models.py:1093`. Consumidor reavalia premissa: `app/chamados/reconciliador.py:1369`. Relógio/deploy que muda a premissa sem recompute continua sem produtor declarado: `app/chamados/management/commands/reconciliar_chamados.py:21`. |
| Pergunta absolvida | Save de veredito/ata emite: `app/ponto/portas/celula.py:878`, `app/ponto/portas/celula.py:894`. Consumidor chama `retratar_perguntas_absolvidas_por_celula`: `app/chamados/reconciliador.py:1365`. O juiz efetivo é `pergunta_viva`, depois a tranca: `app/chamados/reconciliador.py:650`, `app/chamados/reconciliador.py:661`. |
| Rejunção | Mesmo evento; consumidor liga primeiro a célula, antes da absolvição: `app/chamados/reconciliador.py:1363`. Regeneração também chama diretamente após commit: `app/ponto/portas/celula.py:493`. O candidato exige vivo, escopo DIA, célula nula e célula existente: `app/chamados/rejuncao.py:59`, `app/chamados/rejuncao.py:81`. |
| Célula absolveu | Mesmo evento; consumidor chama o retratador: `app/chamados/reconciliador.py:1368`. Predicado exige célula com veredito que não acusa e sem marcos pendentes: `app/chamados/reconciliador.py:1018`. A retratação ainda passa por retenção e `reconciliar()`: `app/chamados/reconciliador.py:518`. |
| Eventos mortos | Não falta necessariamente emissor: evento com ≥5 tentativas e sem processamento sai da fila viva. Fontes: `app/chamados/reconciliador.py:1207`, `app/chamados/reconciliador.py:1246`, `app/chamados/reconciliador.py:1252`. Falhas de chamados/premissa deixam o evento pendente e incrementam tentativas: `app/chamados/reconciliador.py:1389`. |
| Juiz falhou | É exceção do julgamento de premissa para chamado sem evento pendente, contada pelo vigia: `app/chamados/management/commands/reconciliar_chamados.py:73`, `app/chamados/management/commands/reconciliar_chamados.py:82`. No consumidor, falha da premissa é acumulada e retém o evento: `app/chamados/reconciliador.py:1462`. |
| Fantasma com “morfologia” | Morfologia é apenas diagnóstico; quem conta como fantasma é núcleo fechado nas datas + premissa do catálogo morta: `app/ponto/management/commands/reconciliar_fantasmas.py:199`, `app/ponto/management/commands/reconciliar_fantasmas.py:245`, `app/ponto/management/commands/reconciliar_fantasmas.py:256`. Mesmo caminho de evento/retratador da premissa; sem célula, o recompute não emite: `app/ponto/nucleo.py:86`. |

Há um mecanismo concreto de resíduo apesar de evento marcado como processado: os caminhos da pergunta e da rejunção engolem exceções, enquanto o consumidor os chama sem coletor `falhas`; depois marca o evento processado se a lista continuar vazia. Isso é possibilidade demonstrada pelo código, **não prova do ocorrido nos resíduos fornecidos**. Fontes: `app/chamados/reconciliador.py:601`, `app/chamados/reconciliador.py:615`, `app/chamados/reconciliador.py:669`, `app/chamados/rejuncao.py:90`, `app/chamados/rejuncao.py:109`, `app/chamados/reconciliador.py:1364`, `app/chamados/reconciliador.py:1390`.

Também há processamento sem fechamento por decisão legítima: `_retratar` retorna `False` sob retenção; isso não é falha para o consumidor. O contador “célula absolveu” e o contador de fantasmas podem continuar contando esse chamado. Fontes: `app/chamados/reconciliador.py:437`, `app/chamados/reconciliador.py:518`, `app/chamados/reconciliador.py:1269`, `app/ponto/management/commands/reconciliar_fantasmas.py:264`.

O conjunto completo de fantasmas colado tem 36 linhas: 11 diagnósticos de morfologia, 2 sem abertura, 2 incompletos e 21 `ok`; 13 têm pergunta e 23 não. O 38 não aparece no dado fornecido: **[não verificado]**. Não somar esses 36 ao alarme do reconciliador: universos e unidades diferem e podem sobrepor. Fontes dos universos: `app/ponto/management/commands/reconciliar_fantasmas.py:157`, `app/chamados/management/commands/reconciliar_chamados.py:59`, `app/chamados/reconciliador.py:1201`.

O evento não registra origem nem payload; registra célula/colaborador/dia e estado de processamento. Processados são apagados após sete dias. Portanto, o código e os logs colados não permitem atribuir a cada resíduo qual emissor faltou historicamente. Fontes: `app/escala/models.py:1845`, `app/escala/models.py:1857`, `app/chamados/reconciliador.py:1404`.

Dono provável do mecanismo sem produtor ou de exceção engolida: **ESTRUTURA**, por inferência de falha de entrega/consumo; padrão **Transactional Outbox / O-OUTBOX**, e, para morte por relógio/deploy sem evento, **sinal derivado do estado, reconciliação level-triggered**. Fontes dos padrões: `app/docs/GLOSSARIO.md:13`, `app/docs/GLOSSARIO.md:17`, `app/docs/PLANO-PADROES.md:10`. Retenção `irma_orfa` exige validação humana; não autoriza chamar o caso de bug estrutural. Fonte: `app/chamados/reconciliador.py:441`.

Sonda D, para executar somente depois do envelope seguro. Ela usa os predicados existentes, agrega por classe e não imprime nomes, CPF ou IDs. O guard deve ser instalado **antes** destes imports; não executar consumidor, semeadura nem comando com `--apply`. Fontes dos predicados: `app/chamados/reconciliador.py:645`, `app/chamados/rejuncao.py:66`, `app/chamados/reconciliador.py:1018`, `app/ponto/management/commands/reconciliar_fantasmas.py:245`.

```python
# Dentro do envelope SELECT-only já instalado.
# Não envolver em transaction.atomic(): reconciliaria() muda o caminho
# para trava_na_ordem quando in_atomic_block.
# Fontes: chamados/models.py:657 e :705-711.
import json
from collections import Counter
from django.db import connection
from django.db.models import Count

assert connection.get_autocommit() and not connection.in_atomic_block

from chamados.catalogo.motor import VIVOS
from chamados.catalogo.premissa import avaliar, FATO_TURNO_ABERTO
from chamados.juizes import dia_do_fato
from chamados.models import ChamadoColaborador
from chamados import reconciliador as r
from chamados.rejuncao import _candidatos
from colaboradores.models import Colaborador
from escala.models import EscalaColaborador, EventoCelula
from ponto.management.commands.reconciliar_fantasmas import (
    MODULOS_ALVO, _parse_pendentes, _completude,
)

conta = Counter()
alarme = Counter()
pendentes_colab = set(
    r.eventos_pendentes().values_list("colaborador_id", flat=True)
)
pendentes_celula = set(
    r.eventos_pendentes().values_list("celula_id", flat=True)
)

def add(classe, modulo="", subcategoria="", causa="", diagnostico=""):
    conta[(classe, modulo or "", subcategoria or "", causa, diagnostico)] += 1

def diagnostico_do_fecho(ch):
    try:
        ret = r.retencao_da_retratacao(ch)
        if ret is not None:
            return "retencao:" + ret[0]  # não imprime os IDs do detalhe
        return "fecharia" if ch.reconciliaria() else "gate_recusa"
    except Exception as exc:
        return "falha_gate:" + type(exc).__name__

# Universo literal do vigia de premissa.
for ch in ChamadoColaborador.objects.filter(
    status_local__in=VIVOS
).select_related("colaborador").order_by("pk").iterator():
    if ch.colaborador_id in pendentes_colab:
        continue
    try:
        vd = r._veredito_premissa(ch)
        if vd is None or vd.viva:
            continue
        ret = r.retencao_da_retratacao(ch)
        if ret is not None or not ch.reconciliaria():
            add("premissa_morta_retida_MEDIDA",
                ch.modulo_origem, ch.subcategoria, vd.motivo,
                "retencao:" + ret[0] if ret else "gate_recusa")
            continue
    except Exception as exc:
        add("juiz_falhou", ch.modulo_origem, ch.subcategoria,
            type(exc).__name__)
        alarme["juiz_falhou"] += 1
        continue
    add("premissa_morta_sem_evento", ch.modulo_origem,
        ch.subcategoria, vd.motivo)
    alarme["premissa_morta"] += 1

# Os geradores abaixo são os mesmos lidos pelo vigia.
for colab in Colaborador.objects.all().order_by("pk").iterator():
    if colab.pk in pendentes_colab:
        continue
    for p in r.perguntas_absolvidas_pela_celula(colab):
        add("pergunta_absolvida_sem_evento", causa=p.motivo)
        alarme["pergunta_absolvida"] += 1
    for ch, cel in _candidatos(colab):
        add("rejuncao_sem_evento", ch.modulo_origem, ch.subcategoria)
        alarme["rejuncao"] += 1

# Reproduz a contagem de célula SEM retirar retenções,
# e acrescenta o diagnóstico separado do motivo de não fechar.
for ch in r._chamados_de_turno_vivos().iterator():
    achado = r._absolvido_pela_celula(ch)
    if achado is None:
        continue
    dia, cel = achado
    if cel.pk not in pendentes_celula:
        add("celula_absolveu_sem_evento", ch.modulo_origem,
            ch.subcategoria, cel.veredito,
            diagnostico_do_fecho(ch))
        alarme["celula_absolveu"] += 1

for row in EventoCelula.objects.filter(
    processado_em__isnull=True,
    tentativas__gte=r.MAX_TENTATIVAS_EVENTO,
).values("tentativas").annotate(n=Count("pk")).order_by("tentativas"):
    n = row["n"]
    conta[("eventos_mortos", "", "",
           "tentativas=" + str(row["tentativas"]), "")] += n
    alarme["eventos_mortos"] += n

# Universo literal de reconciliar_fantasmas.
for ch in ChamadoColaborador.objects.filter(
    modulo_origem__in=MODULOS_ALVO,
    status_local__in=VIVOS,
).select_related("colaborador").order_by("pk").iterator():
    dia = dia_do_fato(ch)
    ctx = ch.contexto_json or {}
    if dia:
        datas = [dia]
    elif ch.modulo_origem == "entrada_bloqueada_turno_aberto":
        datas = _parse_pendentes(ctx.get("pendentes"))
    else:
        datas = []
    if not datas:
        add("fantasma_skip_sem_data", ch.modulo_origem, ch.subcategoria)
        continue
    if any(r._tem_turno_aberto(ch.colaborador, dia=d) for d in datas):
        add("fantasma_legitimo_nucleo", ch.modulo_origem, ch.subcategoria)
        continue
    vd = avaliar(ch.modulo_origem, ch.subcategoria,
                 fatos={FATO_TURNO_ABERTO: False})
    if not vd.escopo or vd.viva:
        add("fantasma_skip_catalogo", ch.modulo_origem, ch.subcategoria)
        continue

    ec = EscalaColaborador.objects.filter(
        colaborador_id=ch.colaborador_id, ativa=True
    ).select_related("tipo_escala").first()
    diags = set()
    if ec and ec.tipo_escala:
        te = ec.tipo_escala
        tem_int = bool(te.hora_inicio_intervalo and te.hora_fim_intervalo)
        for d in datas:
            motivo = _completude(ch.colaborador_id, ec, te, tem_int, d)[4]
            # Apenas rótulo agregado do diagnóstico já produzido.
            diags.add(
                "morfologia" if motivo.startswith("morfologia:") else
                "incompleto" if motivo.startswith("incompleto ") else motivo
            )
    else:
        diags.add("sem_escala_ativa_sem_diagnostico")
    add("fantasma", ch.modulo_origem, ch.subcategoria,
        "|".join(sorted(diags)), diagnostico_do_fecho(ch))

print(json.dumps({
    "alarme_por_subclasse": dict(alarme),
    "alarme_soma_unidades_heterogeneas": sum(alarme.values()),
    "classes": [
        {
            "classe": k[0], "modulo": k[1], "subcategoria": k[2],
            "causa": k[3], "diagnostico": k[4], "n": n,
        }
        for k, n in sorted(conta.items())
    ],
}, ensure_ascii=False, sort_keys=True))
```

Limitação: o resultado descreve o estado observado durante a leitura, não reconstrói por si a história de entrega nem elimina concorrência entre SELECTs. **[não verificado]**

## 09/11 07:47 · sessao 2d3b248ff368 · - · - · origem=Ronald (tmux)

A — constatações e referências

- O badge 1.177 é produzido por `lavrar_badge_navbar.Command.handle`, que chama `exige_o_admin(ctx['verbos_contagens'])`, soma de `validar + decidir`; unidade = **chamado**, não pergunta ou toque. `app/core/management/commands/lavrar_badge_navbar.py:39`, `app/core/management/commands/lavrar_badge_navbar.py:72`, `app/chamados/services/verbos.py:22`.
- O universo dessa partição é `qs_vivos`: `ChamadoColaborador.objects.filter(status_local__in=VIVOS)`, sem filtros opcionais no lavrador; `VIVOS = ('aberto','em_analise','registrado')`. A partição pode devolver `None`, `arquivado` ou `historico`, que não entram em `validar + decidir`. `app/chamados/services/painel.py:155`, `app/chamados/services/painel.py:228`, `app/chamados/catalogo/motor.py:24`, `app/chamados/services/verbos.py:47`.
- `lavrar_placar_situacional` produz contadores de **colaboradores em operação**, não o badge dos chamados. `app/core/management/commands/lavrar_placar_situacional.py:54`, `app/core/management/commands/lavrar_placar_situacional.py:63`.
- No stale: `chamados_vivos=vivos.count()` conta chamados; `universo=universo.count()` conta perguntas cujo **destino** está vivo; `perguntas_stale_agora=len(stale)` conta perguntas desatualizadas; `lavradas=n` conta perguntas processadas nesta rodada; `sobra=max(len(stale)-teto,0)`, teto padrão 500. Não existe equivalência entre essas cinco grandezas. `app/chamados/management/commands/lavrar_perguntas_stale.py:35`, `app/chamados/management/commands/lavrar_perguntas_stale.py:56`, `app/chamados/management/commands/lavrar_perguntas_stale.py:60`, `app/chamados/management/commands/lavrar_perguntas_stale.py:71`, `app/chamados/management/commands/lavrar_perguntas_stale.py:84`.
- **7.716 exato não está nos anexos stale apresentados**; atribuí-lo a uma execução desse contador é `[não verificado]`. A busca literal encontrou `7.716` apenas como alvo de 98% do oráculo, unidade dia-colab, em documentação histórica — não como aquele contador de chamados/perguntas. `app/docs/RELATO-ARQUIVO.md:23683`, `app/docs/RELATO-ARQUIVO.md:23695`.
- **4.323 exato também não aparece nas linhas stale raspadas** `[não verificado]`; se a referência é `chamados_vivos`, sua unidade é chamado e não `lavradas/sobra`. `app/chamados/management/commands/lavrar_perguntas_stale.py:71`.

Matriz dos juízes:

| Rótulo pedido | Autoridade existente e limite |
|---|---|
| legítimo | **SEM JUIZ** composto; premissa viva não prova legitimidade geral. `app/chamados/catalogo/premissa.py:128` |
| repetido | Registro aponta `ChamadoColaborador.abrir`, **escritor**, impróprio para sonda; não há juiz de leitura agregado equivalente. `app/core/juizes.py:268` |
| sem porta | `porta_alcancavel_do_dia`; resultado `None`. É porta do corte, não todos os botões manuais existentes. `app/core/juizes.py:277`, `app/chamados/juizes.py:1361`, `app/chamados/juizes.py:1387` |
| premissa superada | `premissa_morta`; alimentação de leitura existente `_veredito_premissa`. Fora de escopo ou fato não coletado não significa legítimo. `app/core/juizes.py:263`, `app/chamados/reconciliador.py:247`, `app/chamados/catalogo/premissa.py:125` |
| exige humano | Verbo corrente `validar/decidir`, pela partição existente; não há juiz explícito desse rótulo global no registro. `app/chamados/services/verbos.py:22`, `app/chamados/catalogo/natureza.py:57` |
| pergunta viva/morta | C6 `pergunta_viva`. Duplicata retratada é identidade terminal, não prova de censo completo de repetição ativa. `app/core/juizes.py:266`, `app/chamados/juizes.py:1163`, `app/chamados/juizes.py:1182` |

Os rótulos são **eixos sobrepostos**, não partição exclusiva autorizada por algum juiz. Criar precedência entre eles seria predicado novo. `app/core/juizes.py:260`.

Histórico:

- Pergunta não tem timestamp de nascimento; `data_evento` é data do fato. `app/chamados/models.py:2414`, `app/chamados/models.py:2431`.
- `DecisaoPergunta` registra resposta, validação, devolução, recusa, reabertura, silêncio, retratação e absorção; não nascimento nem todas as mudanças de C6 causadas por insumos. `app/chamados/models.py:3839`, `app/chamados/models.py:3857`.
- Chamado tem `criado_em` e `resolvido_em`, mas estado pode reabrir; timestamps atuais não fornecem todos os intervalos históricos. A trilha de transição é best-effort. `app/chamados/models.py:60`, `app/chamados/models.py:67`, `app/chamados/models.py:292`, `app/core/observ.py:94`.
- Logo, a sonda enviada por mensagem mede estoque atual por competência/classe e nascimento registrado de chamados antes/depois do deploy. **Nasce/permanece/encerra históricos completos por C6/classe antes/depois = `[não verificado]`**, sem snapshots/insumos históricos. Não apresentar `resolvido_em` como fluxo completo. [nao verificado]
- Competências saem de `competencia_do_chamado` e `janela_atual`; corte efetivo respeita `Empresa.dia_inicio_competencia`, com 21 como default. `app/core/juizes.py:274`, `app/core/juizes.py:283`, `app/ponto/janelas.py:36`.

Dono provável de uma divergência real entre contador e sua própria lista: **ESTRUTURA**, padrão *Single Source of Truth / Aggregate Root*. A diferença entre universos corretamente rotulados não demonstra divergência estrutural. `app/docs/LEIS.md:122`, `app/docs/GLOSSARIO.md:11`.

## 09/11 07:47 · sessao e8248f22495a · SELOS-CANON-I1 · leitura · origem=Code

**Há dois furos efetivos no Agent na matriz pedida:** `O9 (C)` × célula `O9` e `O9 NOTURNO-X` × célula `O9`. A pertença retorna `False`; `_censo_nega` passa antes de examinar briefs. O wrapper `main_censo` recusa essas entradas qualificadas antes da pertença; portanto, seu `rc=1` sozinho não mata esse mutante. (`bin/disjuntor.py:1683–1686`, `bin/disjuntor.py:1702–1703`, `bin/disjuntor.py:2246–2248`)

**1. MATRIZ**

Cada entrada abaixo tem a forma **`P / A / W → LEI`**:

| Símbolo | Significado e ramo |
|---|---|
| `P` | `_pertence_fila1` recebendo literalmente a obra indicada. (`bin/disjuntor.py:1669`) |
| `A` | Pertença alcançada pelo Agent, após `_obra_de` e `_identidade`. (`bin/disjuntor.py:602–627`, `bin/disjuntor.py:1844–1848`) |
| `W` | Pertença alcançada por `main_censo`. (`bin/disjuntor.py:2246–2265`) |
| `T` | `True`, ramo `bin/disjuntor.py:1687–1688`. |
| `F` | `False`, ramo `bin/disjuntor.py:1685–1686`. |
| `N` | NAO SEI: `ValueError`, ramo `bin/disjuntor.py:1689–1690`. |
| `—A` | Obra ilegível no Agent; recusa anterior, sem chamar a pertença. (`bin/disjuntor.py:619–621`, `bin/disjuntor.py:1845–1847`) |
| `—W` | Obra fora da forma aceita por `main_censo`; `rc=1` anterior à pertença. (`bin/disjuntor.py:2246–2248`) |
| `d / f / n` | Pela lei: dentro / fora / NAO SEI. Identidade única, ambiguidades e fronteiras seguem o contrato existente; NAO SEI nega. (`bin/disjuntor.py:1674–1677`, `bin/hook_stop_fila1.py:117`, `bin/disjuntor.py:44–50`) |
| **`F!`** | **Furo: HOJE FORA, pela lei NAO SEI.** Falta consultar o token inteiro da célula dentro da obra; hoje só existe o sentido obra→célula. (`bin/disjuntor.py:1683–1686`) |

Na coluna **prosa**, a célula é `OUTRA-X (cita <obra inteira da linha>)`: seu token é outro, mas ela nomeia integralmente a obra. Isso produz ambiguidade, sem depender de substring ou de interpretar apenas parte da cauda. (`bin/disjuntor.py:1663–1666`, `bin/disjuntor.py:1684–1690`, `bin/hook_stop_fila1.py:117`)

| OBRA apresentada | Célula `O9` | Célula `O9 (C)` | Células `OBRA-Q (A)` + `OBRA-Q (B)` | Outro token + prosa | Célula `O142` |
|---|---|---|---|---|---|
| `O9` | T/T/T → d | N/N/N → n | F/F/F → f | N/N/N → n | F/F/F → f |
| `O9 (C)` | **F!/F!/—W → n** | T/T/—W → d | F/F/—W → f | N/N/—W → n | F/F/—W → f |
| `O9 NOTURNO-X` | **F!/F!/—W → n** | F/F/—W → f | F/F/—W → f | N/N/—W → n | F/F/—W → f |
| `OBRA-Q` | F/F/F → f | F/F/F → f | N/N/N → n | N/N/N → n | F/F/F → f |
| `OBRA-Q (A)` | F/F/—W → f | F/F/—W → f | N/N/—W → n | N/N/—W → n | F/F/—W → f |
| `o9` | N/—A/N → n | N/—A/N → n | F/—A/F → f | N/—A/N → n | F/—A/F → f |
| `09-TURNO-ABERTO-EXPOSTA` | F/—A/F → f | F/—A/F → f | F/—A/F → f | N/—A/N → n | F/—A/F → f |
| `O67b` | F/—A/F → f | F/—A/F → f | F/—A/F → f | N/—A/N → n | F/—A/F → f |
| `O1` — controle de prefixo | F/F/F → f | F/F/F → f | F/F/F → f | N/N/N → n | **F/F/F → f** |

Os ramos de cada símbolo estão identificados na legenda. As diferenças de leitura entre linhas vêm de `_OBRA_ID`, `_obra_lida` e `_identidade`; em particular, o Agent preserva a cauda de `O9`, mas transforma `OBRA-Q (A)` em `OBRA-Q`. (`bin/disjuntor.py:157–168`, `bin/disjuntor.py:619–627`)

Para completar as **formas alinhadas ao próprio ID**, sem deixar dígito inicial, minúscula final e hífen limitados a cruzamentos com IDs alheios:

| OBRA apresentada | Célula(s) | P / A / W → LEI |
|---|---|---|
| `OBRA-Q` | `OBRA-Q` | T/T/T → d |
| `OBRA-Q` | `OBRA-Q (A)` | T/T/T → d |
| `OBRA-Q (A)` | `OBRA-Q` | **F!/T/—W → n no argumento literal; d após identidade do Agent** |
| `OBRA-Q (A)` | `OBRA-Q (A)` | N/T/—W → n no argumento literal; d após identidade do Agent |
| `O9 NOTURNO-X` | `O9 NOTURNO-X` | T/T/—W → d |
| `o9` | `o9` | T/—A/T → d |
| `09-TURNO-ABERTO-EXPOSTA` | mesmo ID nu | T/—A/T → d |
| `09-TURNO-ABERTO-EXPOSTA` | mesmo ID + ` (C)` | N/—A/N → n |
| `O67b` | `O67b` | T/—A/T → d |
| `O67b` | `O67b (C)` | N/—A/N → n |
| `O142` | `O1` | F/F/F → f |

Esses resultados usam o aceite integral de célula nua por `OBRA_CENSO_ID`, a validação do token qualificado e os três ramos da pertença. O **F! literal de `OBRA-Q (A)` não abre vaga por essas interfaces**: o Agent normaliza a identidade; `main_censo` recusa o espaço antes da consulta. (`bin/disjuntor.py:162`, `bin/disjuntor.py:619–627`, `bin/disjuntor.py:1661–1690`, `bin/disjuntor.py:2246–2248`)

Quando alcançada, a pertença tem consequências distintas:

| Resultado | Agent / `_censo_nega` | Wrapper / `main_censo` |
|---|---|---|
| `F` | Retorna `None` antes do censo. (`bin/disjuntor.py:1702–1703`) | Grava `CENSO FORA`, retorna `0`. (`bin/disjuntor.py:2269–2272`) |
| `T` | Continua para forma e briefs; `True` não significa censo aprovado. (`bin/disjuntor.py:1704–1727`) | Chama `_censo_nega` com a mesma fila. (`bin/disjuntor.py:2273`) |
| `N` | Exceção vira negativa NAO SEI. (`bin/disjuntor.py:1728–1729`) | Exceção vira negativa e `rc=1`. (`bin/disjuntor.py:2266–2267`, `bin/disjuntor.py:2274–2279`) |

O shell construtor ainda tem uma barreira anterior mais restrita que `main_censo`: somente `[A-Z0-9-]+`. Assim, o aceite de `o9`/`O67b` no CLI acima não significa que o shell construtor os aceite. (`bin/codex_pergunta.sh:188`, `bin/disjuntor.py:162`, `bin/disjuntor.py:2246`)

**2. SELOS E MUTANTES**

O selo existente cobre obra nua→célula qualificada e prepara uma célula `O9` nua e única em `:580`; falta experimentar a obra qualificada contra essa célula antes de restaurar o BACKLOG em `:583`. (`bin/tests/test_censo_antes_da_vaga.sh:544–548`, `bin/tests/test_censo_antes_da_vaga.sh:564–566`, `bin/tests/test_censo_antes_da_vaga.sh:576–583`)

**Linhas propostas para esse ponto**, cobrindo os dois furos efetivos. Usam os helpers e o brief ruim já existentes; proposta não aplicada nem executada. (`bin/tests/test_censo_antes_da_vaga.sh:71–74`, `bin/tests/test_censo_antes_da_vaga.sh:202–214`, `bin/tests/test_censo_antes_da_vaga.sh:520–527`; execução: [nao verificado])

```bash
for o in 'O9 (C)' 'O9 NOTURNO-X'; do
  s="$(_pre "$B" "$(_pl_agent general-purpose \
    "Construa a obra $o na copia $WT. Brief: logs/e3/brief_ruim.md")")"
  _nega_censo_de "pertenca-agent-espelho-$o" "$o" "$s" || ok=1
  _sem_fora "pertenca-agent-espelho-$o" "$o" || ok=1

  rc="$(_censo "$B" "$o")"
  [ "$rc" = 1 ] || {
    _falha "pertenca-wrapper-qualificada-$o" "rc=$rc, esperado 1"
    ok=1
  }
done
```

| Furo | Caso que o morde | Mutante visado |
|---|---|---|
| `O9 (C)` × `O9` | Iteração Agent `O9 (C)` acima. | Retirar o reconhecimento reverso token→obra, restaurando apenas o sentido atual de `bin/disjuntor.py:1684`. |
| `O9 NOTURNO-X` × `O9` | Iteração Agent `O9 NOTURNO-X` acima. | Mesmo mutante; também detecta reconhecimento reverso limitado a cauda entre parênteses. (`bin/disjuntor.py:164`, `bin/disjuntor.py:1684`) |
| Literal `OBRA-Q (A)` × `OBRA-Q` | Asserção direta abaixo; no Agent a normalização encobre esse F literal. | Mesmo mutante, aplicado ao predicado diretamente. (`bin/disjuntor.py:627`, `bin/disjuntor.py:1683–1686`) |

**Limite da exigência “nas duas vias”:** as iterações wrapper acima são controles da recusa de forma. Elas já passam com a pertença vulnerável e **não matam o mutante da pertença**, porque `main_censo` termina antes de chamá-la. Trocar apenas o rótulo `via` de `_censo_nega` também não prova a integração de `main_censo`. (`bin/disjuntor.py:1693`, `bin/disjuntor.py:2246–2248`, `bin/disjuntor.py:2265`)

Para morder diretamente os três F literais, a asserção pronta para `casos_pertenca` é esta; aceita `True` ou NAO SEI, mas reprova FORA. Proposta não executada. (`bin/disjuntor.py:1686–1690`; execução: [nao verificado])

```bash
python3 -B - "$B" <<'ESPELHO' || {
  _falha pertenca-espelho-direto "obra relacionada caiu em FORA"
  ok=1
}
import sys
sys.path.insert(0, sys.argv[1])
import disjuntor as D

for obra, celula in (
    ('O9 (C)', 'O9'),
    ('O9 NOTURNO-X', 'O9'),
    ('OBRA-Q (A)', 'OBRA-Q'),
):
    try:
        resultado = D._pertence_fila1(obra, [celula])
    except ValueError:
        continue
    if resultado is False:
        raise SystemExit(f'FORA indevido: {obra!r} x {celula!r}')
ESPELHO
```

Os mutantes atuais cobrem célula crua, exato que cala outra, dois candidatos, decisão própria do wrapper, token cru e substring; o reconhecimento reverso ainda não existe no ramo que seria retirado pelo novo mutante. (`bin/tests/test_censo_antes_da_vaga.sh:714–721`, `bin/disjuntor.py:1684`)

**3. ACHADOS 2, 3, 4 E 6**

| Achado do segundo laudo | Mesmo predicado ou outro? |
|---|---|
| **2 — `O67b`, `O51b`, caixa e dígito inicial** | **Outro: leitura/validação da obra.** As formas entram na matriz quando fornecidas ao predicado; serem barradas antes é decisão do extrator ou do shell. (`bin/disjuntor.py:157`, `bin/disjuntor.py:619–621`, `bin/codex_pergunta.sh:188`) |
| **2 — `FECHADA:` sem marca reconhecida** | **Outro: estado/seleção da fila.** `_FECHADO` exige negrito ou tachado; `_item_fechado` e `_itens_andaveis` aplicam essa decisão. (`bin/hook_stop_fila1.py:59`, `bin/hook_stop_fila1.py:168`, `bin/hook_stop_fila1.py:250–252`) |
| **3 — célula `O9 (C)` exata nunca abre** | **Outro: forma aceita pelo censo.** A pertença exata dá `True`; depois `_censo_nega` recusa a forma. No CLI, a recusa ocorre antes. **O espelho `O9 (C)`×`O9`, separadamente, é pertença e entra nesta volta.** (`bin/disjuntor.py:1687–1688`, `bin/disjuntor.py:1704–1707`, `bin/disjuntor.py:2246–2248`) |
| **4 — PARADA excluída** | **Outro: universo entregue à pertença.** `_fila1_viva` chama `_itens_andaveis`, que elimina `_item_parado`; a comparação token/obra recebe a lista já filtrada. (`bin/disjuntor.py:1637`, `bin/hook_stop_fila1.py:180`, `bin/hook_stop_fila1.py:250–252`) |
| **6 — texto CORTES/PAREI** | **Outro: contrato textual.** Literal vigente em `bin/disjuntor.py:1511`. A referência `CORTES.md:428` do anexo é [nao verificado]; a cópia termina em `app/docs/CORTES.md:425`. |
| **6 — PAREI permanece após fechar censo** | **Outro: ciclo de vida da faixa.** O escritor insere; o caminho de aprovação registra APROVADO e retorna, sem retirar a linha. (`app/core/esteira_vigia.py:883–890`, `bin/disjuntor.py:1717–1722`, `bin/disjuntor.py:2280–2281`) |
| **6 — papel leitura como prova** | **Outro: prova de processo/papel.** `leitura` integra `_PAPEL_CENSO`; a prova aceita o processo correspondente. Handoff publica o resultado desse reconhecedor. (`bin/hook_stop_fila1.py:726`, `bin/hook_stop_fila1.py:763–767`, `bin/handoff_sessao.sh:129–134`) |
| **6 — `_cru_abre_agent_copia`** | **Outro: classificação da abertura.** O cru truncado reconhece `Agent` ou `wt-`; o caminho normal exige agente não leitura e cópia reconhecida. (`bin/disjuntor.py:83–84`, `bin/disjuntor.py:1740–1741`) |
| **6 — `COPIA_RE` e `~/wt-*`** | **Outro: reconhecimento do caminho.** O regex exige a forma com barras `/…/wt-`; ele participa da classificação anterior ao censo. (`bin/disjuntor.py:153`, `bin/disjuntor.py:1844`) |

A inexistência de célula qualificada viva “hoje” e as quantidades de produção permanecem restritas ao dado raspado; confirmação independente: [nao verificado].

**4. ESCRITORES E LEITORES**

| Símbolo | Quem escreve/define | Quem lê/chama diretamente |
|---|---|---|
| `_pertence_fila1` | `bin/disjuntor.py:1669` | `_censo_nega`: `bin/disjuntor.py:1702`; `main_censo`: `bin/disjuntor.py:2265`. |
| `_token_da_celula` | `bin/disjuntor.py:1651` | Lista `exatas` da pertença: `bin/disjuntor.py:1683`. |
| `_casa_id_inteiro` | `bin/hook_stop_fila1.py:110` | Pertença: `bin/disjuntor.py:1684`; `_relato_parou`, IDs parados: `bin/hook_stop_fila1.py:1301`; item em curso: `bin/hook_stop_fila1.py:1313`. |
| `_fila1_viva` | `bin/disjuntor.py:1626` | `_censo_nega`: `bin/disjuntor.py:1702`; `main_censo`: `bin/disjuntor.py:2264`. |

Os leitores do Stop em `:1301` e `:1313` precisam permanecer no alcance da revisão de `_casa_id_inteiro`: decidem se o PAREI nomeia item parado ou o item em curso. O selo de fronteira os exercita por `_relato_parou`. (`bin/hook_stop_fila1.py:1298–1313`, `bin/tests/test_hook_parei_casa_id_inteiro.sh:53`)

O selo de censo lê indiretamente pelas portas `_censo`/`_pre` e escreve **cópias mutantes** dos corpos: pertença em `:714–717`, token em `:718–720`, fronteira em `:721`, leitura da fila em `:658–663` e `:679–680`. (`bin/tests/test_censo_antes_da_vaga.sh:71–74`, `bin/tests/test_censo_antes_da_vaga.sh:208`, `bin/tests/test_censo_antes_da_vaga.sh:658–663`, `bin/tests/test_censo_antes_da_vaga.sh:679–680`, `bin/tests/test_censo_antes_da_vaga.sh:714–721`)

## 09/11 07:46 · sessao a9dd49a63ff7 · - · - · origem=Ronald (tmux)

Interfaces reais: `validar_pergunta(pk, autor)` em `app/chamados/services/validacao.py:373`; resposta pela porta `responder_pergunta_disputa_core` em `app/chamados/services/disputa_emissao.py:1422`, com linha append-only em `:1508`; overnight por `_ts_tipo_do_plantio` em `app/chamados/services/materializacao.py:330`, ajuste em `:421`. Modelo de vínculo noturno já usado nos testes em `app/chamados/tests/test_plantio_intervalo_overnight.py:19`. Fechamento da disputa passa por `pode_fechar()` em `app/chamados/models.py:1856` e pela cauda da materialização em `app/chamados/services/materializacao.py:1302`.

Segue teste proposto, sem execução; campos cadastrais, escala e identidade dos usuários são sintéticos, pois não constam no dado raspado. O primeiro aceite permanece como decisão e aguarda a outra metade; essa política é a interpretação explícita usada pelo RED. [não verificado] [nao verificado]

```python
import datetime as dt
from zoneinfo import ZoneInfo

from django.contrib.auth.models import User
from django.test import TestCase, override_settings
from freezegun import freeze_time

from chamados.models import (
    ChamadoColaborador, DecisaoPergunta,
    DisputaSupervisao, PerguntaDisputa,
)
from chamados.services.disputa_emissao import responder_pergunta_disputa_core
from chamados.services.materializacao import _ts_tipo_do_plantio
from chamados.services.validacao import validar_pergunta
from colaboradores.models import Colaborador, Empresa, Posto, Praca
from escala.models import EscalaColaborador, TipoEscala
from ponto.models import Batida


DIA = dt.date(2026, 10, 21)
TZ = ZoneInfo("America/Sao_Paulo")


def ts(dia, hora, minuto=0):
    return dt.datetime.combine(dia, dt.time(hora, minuto), tzinfo=TZ)


@override_settings(TIME_ZONE="America/Sao_Paulo", USE_TZ=True)
@freeze_time("2026-11-09 10:45:00+00:00")
class BugParIntervaloCol960(TestCase):
    def setUp(self):
        User.objects.get_or_create(username="sistema")
        self.autor = User.objects.create_user(
            username="admin_fixture_960", is_staff=True, is_superuser=True
        )
        self.usuario = User.objects.create_user(username="col_fixture_960")
        emp = Empresa.objects.create(
            razao_social="FIXTURE", cnpj="00000000000000",
            dia_inicio_competencia=21,
        )
        pr = Praca.objects.create(
            nome="FIXTURE", cidade="Londrina", estado="PR"
        )
        posto = Posto.objects.create(
            nome="FIXTURE", empresa=emp, praca=pr
        )
        self.col = Colaborador.objects.create(
            pk=960, nome="col960", cpf="[cpf]",
            empresa=emp, posto=posto, situacao="ativo",
            usuario=self.usuario,
        )
        te = TipoEscala.objects.create(
            codigo="FIXTURE_960_NOTURNO", descricao="FIXTURE",
            tipo_ciclo=TipoEscala.CICLO_12x36,
            hora_inicio=dt.time(19), hora_fim=dt.time(7),
            hora_inicio_intervalo=dt.time(1),
            hora_fim_intervalo=dt.time(2),
            minutos_jornada=720,
        )
        EscalaColaborador.objects.create(
            colaborador=self.col, posto=posto, tipo_escala=te,
            data_inicio=DIA - dt.timedelta(days=30),
            data_ancora_colaborador=DIA, ativa=True,
        )
        self.ch = ChamadoColaborador.objects.create(
            pk=35664, colaborador=self.col, titulo="FIXTURE",
            assunto="FIXTURE", area="supervisao",
            categoria="Ausencia", subcategoria="Batida nao realizada",
            modulo_origem="batida_ausente", status_local="aberto",
            contexto_json={
                "data_turno": DIA.isoformat(),
                "marcos_faltantes": [
                    {"motivo": "intervalo_saida", "tipo": "S", "hora": "01:00"},
                    {"motivo": "intervalo_volta", "tipo": "E", "hora": "02:00"},
                ],
            },
        )
        self.d = DisputaSupervisao.objects.create(
            pk=5644, colaborador=self.col, chamado=self.ch,
            motivos=["intervalo_saida", "intervalo_volta"],
            contexto={
                "intervalo_saida": {"data": DIA.isoformat()},
                "intervalo_volta": {"data": DIA.isoformat()},
            },
        )
        self.saida = PerguntaDisputa.objects.create(
            disputa=self.d, chamado_destino=self.ch,
            motivo="intervalo_saida", data_evento=DIA,
            tipo_resposta="hora", pergunta="Que horas saiu?",
        )
        self.volta = PerguntaDisputa.objects.create(
            disputa=self.d, chamado_destino=self.ch,
            motivo="intervalo_volta", data_evento=DIA,
            tipo_resposta="hora", pergunta="Que horas voltou?",
        )
        Batida.objects.bulk_create([
            Batida(
                pk=133756, colaborador=self.col,
                timestamp=ts(DIA, 18, 32), tipo="E",
                origem="app", aprovada=True,
            ),
            Batida(
                pk=133794, colaborador=self.col,
                timestamp=ts(DIA + dt.timedelta(days=1), 6, 30),
                tipo="S", origem="app", aprovada=True,
            ),
        ])
        self.chao = list(
            Batida.objects.filter(pk__in=[133756, 133794])
            .order_by("pk")
            .values_list("pk", "timestamp", "tipo", "origem", "retratada_em")
        )

    def responder(self, pergunta, texto):
        ok, erro = responder_pergunta_disputa_core(
            pergunta, texto, self.usuario, ja_respondida=False
        )
        self.assertTrue(ok, erro)
        pergunta.refresh_from_db()

    def novas(self):
        return Batida.objects.filter(colaborador=self.col).exclude(
            pk__in=[133756, 133794]
        )

    def assert_aguarda(self, retorno):
        self.assertIn(
            "falta a outra metade do intervalo",
            retorno["msg"].lower(),
        )
        self.assertNotIn("colada", str(retorno).lower())
        self.assertFalse(self.novas().exists())
        self.d.refresh_from_db()
        self.assertIsNone(self.d.fechada_em)
        for pergunta in (self.saida, self.volta):
            pergunta.refresh_from_db()
            self.assertIsNone(pergunta.materializada_em)

    def test_RED_validar_saida_depois_volta_aplica_par(self):
        self.responder(self.saida, "01:00")
        self.responder(self.volta, "02:00")

        # Confirma a geometria da fixture pela derivação real.
        for pergunta, hora, tipo in (
            (self.saida, 1, "S"), (self.volta, 2, "E")
        ):
            erros = []
            self.assertEqual(
                _ts_tipo_do_plantio(self.d, pergunta, erros),
                (ts(DIA + dt.timedelta(days=1), hora), tipo),
                erros,
            )

        anteriores = list(
            DecisaoPergunta.objects.filter(
                pergunta__in=[self.saida, self.volta]
            ).order_by("pk").values()
        )

        r1 = validar_pergunta(self.saida.pk, self.autor)
        self.assert_aguarda(r1)

        r2 = validar_pergunta(self.volta.pk, self.autor)
        self.assertTrue(r2["ok"], r2)
        self.assertEqual(
            list(self.novas().order_by("timestamp").values_list(
                "tipo", "timestamp", "pergunta_origem_id"
            )),
            [
                ("S", ts(DIA + dt.timedelta(days=1), 1), self.saida.pk),
                ("E", ts(DIA + dt.timedelta(days=1), 2), self.volta.pk),
            ],
        )
        for pergunta in (self.saida, self.volta):
            pergunta.refresh_from_db()
            self.assertIsNotNone(pergunta.validada_em)
            self.assertIsNotNone(pergunta.materializada_em)

        self.d.refresh_from_db()
        self.assertIsNotNone(self.d.fechada_em)
        self.assertEqual(
            list(Batida.objects.filter(pk__in=[133756, 133794])
                 .order_by("pk")
                 .values_list(
                     "pk", "timestamp", "tipo", "origem", "retratada_em"
                 )),
            self.chao,
        )
        self.assertEqual(
            list(DecisaoPergunta.objects.filter(
                pk__in=[linha["id"] for linha in anteriores]
            ).order_by("pk").values()),
            anteriores,
        )

    def test_RED_so_saida_respondida_e_validada_aguarda(self):
        self.responder(self.saida, "01:00")
        r = validar_pergunta(self.saida.pk, self.autor)
        self.assert_aguarda(r)
        self.volta.refresh_from_db()
        self.assertIsNone(self.volta.respondida_em)
        self.assertEqual(self.volta.resposta_colab, "")
```

O RED usa funções existentes; a mensagem esperada e a unidade de aplicação do par ainda são comportamento proposto. A cópia atual chama `_materializar_uma` imediatamente após o carimbo de uma pergunta em `app/chamados/services/validacao.py:461`, devolvendo a validação após recusa em `:464`; portanto, o ponto esperado de falha por leitura é `assert_aguarda`, mas a execução e o vermelho efetivo permanecem [não verificado].

Pontos adicionais para a consolidação:

- Duas respostas presentes com apenas a saída validada já são cobertas pelo primeiro teste; uma resposta presente com a outra muda é o segundo teste. [não verificado: teste proposto] [nao verificado]
- O snapshot compara todas as colunas das decisões antigas, cobrando que nenhuma linha prévia tenha sido reescrita; o mecanismo de imutabilidade possui teste existente em `app/chamados/tests/test_e1_entradas.py:78`.
- `TestCase` mantém transação externa; por isso, esse RED comprova o resultado funcional e preservação do chão, mas não demonstra por si só a atomicidade em produção nem concorrência. [não verificado]
- Para o requisito de atomicidade, acrescentar teste de falha na segunda chamada à porta `registrar_batida`, exigindo rollback das duas batidas e dos carimbos, depois que a função concreta de aplicação do par existir. A porta real é `app/ponto/registro_batida.py:94`. [não verificado: teste adicional proposto]

## 09/11 07:46 · sessao 6e84b50fe4db · - · - · origem=Ronald (tmux)

**SEM JUIZ DO PAR DE PERGUNTAS.** O registro tem juiz do vão entre **batidas**, `intervalos_do_turno`, e juízes de turno/marcos; nenhum deles responde “estas duas perguntas são saída e volta da mesma pausa”. `app/core/juizes.py:195`, `app/core/juizes.py:202`, `app/ponto/turnos.py:502`.

O vínculo atual é estrutural: `PerguntaDisputa.disputa`, `motivo`, `data_evento`, `instancia_marco`; saída e volta recebem posições diferentes, `int_ida`/`int_volta`, sem identificador compartilhado da pausa. `app/chamados/models.py:2424`, `app/chamados/models.py:2431`, `app/chamados/models.py:2442`, `app/chamados/services/declaracao_texto.py:42`.

O código explicita que **duas pausas do mesmo dia hoje caem nos mesmos motivos, nenhum emissor carrega índice da pausa e separar pausas está reservado à E2**. Portanto mesma disputa+dia+motivos identifica candidato, mas não prova geral de mesma pausa; preencher arbitrariamente `instancia_marco` diferente da derivada também é recusado. `app/chamados/services/declaracao_texto.py:48`, `app/chamados/models.py:3436`.

O emissor mais próximo lê `marcos_faltantes` de um chamado `intervalo_ausente`, converte cada marco S/E nos motivos saída/volta e emite uma pergunta por marco, sem constituir unidade operacional do par. `app/chamados/services/disputa_emissao.py:1132`.

**Aplicação hoje:** `views.py` e fio chamam a casca `acao_validar_pergunta`, que chama `validar_pergunta`. Esta função valida **UMA** pergunta; abre `atomic`, toma travas, carimba a pergunta e executa `_materializar_uma` para ela. Depois do `atomic`, executa a varredura da disputa, que também aplica uma pergunta por vez. `app/chamados/views.py:483`, `app/chamados/services/fio.py:585`, `app/chamados/services/acoes_disputa.py:13`, `app/chamados/services/validacao.py:373`, `app/chamados/services/validacao.py:425`, `app/chamados/services/validacao.py:461`, `app/chamados/services/validacao.py:469`, `app/chamados/services/materializacao.py:1369`.

O ato atual grava `validada_em/por` e uma linha `DecisaoPergunta.VALIDACAO` na mesma transação; recusa posterior pode desfazer a projeção e acrescentar `DEVOLUCAO`, preservando a validação anterior. `app/chamados/services/validacao.py:127`, `app/chamados/services/validacao.py:195`.

O materializador abre transação por pergunta, chama o ramo de hora, que invoca a porta única `registrar_batida` com `autoridade_humana=False` e `fato_de_chao=False`; a porta cria a batida, e então o materializador exige lastro e carimbo aceito pelo C6. Falha provoca rollback daquela pergunta. Não existe transação envolvendo as duas metades como unidade. `app/chamados/services/materializacao.py:1220`, `app/chamados/services/materializacao.py:926`, `app/ponto/registro_batida.py:152`, `app/chamados/services/materializacao.py:1232`, `app/chamados/services/materializacao.py:1277`.

| Campo/função | Escreve | Lê |
|---|---|---|
| `resposta_colab`, `respondida_em` | `responder_pergunta_disputa_core`, `app/chamados/services/disputa_emissao.py:1460` | validação `app/chamados/services/validacao.py:393`; materialização `app/chamados/services/materializacao.py:829`; tela `app/templates/chamados/partials/modal_fio.html:218` |
| `validada_em/por` | `_carimbar_validacao`, `app/chamados/services/validacao.py:127`; devolução `app/chamados/services/validacao.py:195` | seletor granular `app/chamados/services/materializacao.py:1369`; tela `app/templates/chamados/partials/modal_fio.html:227` |
| `materializada_em/por`, `via_resolucao` | `PerguntaDisputa.carimbar`, `app/chamados/models.py:3103` | seletor `app/chamados/services/materializacao.py:1372`; C6 consultado no próprio carimbo `app/chamados/models.py:3139` |
| `recusa_motivo` | `_carimbar_recusa`, `app/chamados/services/materializacao.py:1083` | tela `app/templates/chamados/partials/modal_fio.html:225`; tradução da casca `app/chamados/services/acoes_disputa.py:32` |
| `Batida.timestamp/tipo/origem` | `registrar_batida`, `app/ponto/registro_batida.py:152` | lastro após escrita `app/chamados/services/materializacao.py:1233` |
| `Batida.pergunta_origem` | `anotar` chamado pelo materializador, `app/chamados/services/materializacao.py:946` | juiz do lastro explicado no mesmo trecho, `app/chamados/services/materializacao.py:941` |
| `DecisaoPergunta` | porta `registrar`, `app/chamados/models.py:3942` | histórico `respostas_anteriores`, `app/chamados/models.py:3500` |
| fechamento disputa | `DisputaSupervisao.fechar`, `app/chamados/models.py:1863` | `pode_fechar`, `app/chamados/models.py:1856`; cauda materializadora `app/chamados/services/materializacao.py:1302` |

**Desenho proposto, condicionado à identificação legítima do par:** ampliar a unidade materializadora para receber as duas perguntas provadas como par; tomar o conjunto de travas antes de qualquer escrita, reler ambas; guardar a validação de uma metade sem plantar enquanto falta anuência/validação da outra e devolver mensagem específica; quando ambas estiverem prontas, uma única transação escreve as duas pela mesma porta, exige alternância e lastro **depois do conjunto**, carimba ambas e só então reconcilia destinos e fechamento. Os pontos concretos a alterar são os anteriores sítios singulares de validação/aplicação, seleção e tradução; este comportamento ainda não existe. `app/chamados/services/validacao.py:425`, `app/chamados/services/validacao.py:461`, `app/chamados/services/materializacao.py:872`, `app/chamados/services/materializacao.py:1220`, `app/chamados/services/materializacao.py:1369`, `app/chamados/services/acoes_disputa.py:32` [desenho não implementado].

Cuidados concretos do desenho:

- Não manter a execução de `_materializar_uma` isolada para a primeira metade nem reconciliar o chamado entre as duas: o ramo atual reconcilia imediatamente após cada plantio, e os efeitos do fechamento podem atingir perguntas irmãs. `app/chamados/services/materializacao.py:974`, `app/chamados/services/disputa_emissao.py:1779`.
- Preservar ordem global **Colaborador → Disputa → Chamado → Pergunta**, incluindo as duas perguntas e todos os destinos/clones; o helper exige transação e orienta reler as instâncias após a trava. `app/chamados/models.py:2637`.
- Conservar recusa/espera como estado lido pela tela, com escritor canônico; `nao_gravou` hoje supõe `erros[0]`, portanto novo retorno de espera precisa tratamento explícito para não indexar lista vazia. `app/chamados/services/acoes_disputa.py:32`.
- Não reutilizar `validada_em` de metade aguardando como justificativa para absolver ou fechar: a porta de fechamento exige ausência de perguntas vivas **e** de respostas sem veredito. `app/chamados/models.py:1856`.
- Preservar texto humano nas duas perguntas: ramo de hora recusa resposta vazia, e validação do administrador não substitui a declaração do colaborador. `app/chamados/services/materializacao.py:822`, `app/chamados/services/materializacao.py:829`.
- Preservar append-only E1: registrar atos novos pela porta; não editar/apagar decisões anteriores; correção precisa conservar a projeção legada sem linha antes de alterá-la. `app/chamados/models.py:3821`, `app/chamados/models.py:3903`, `app/chamados/models.py:3914`.
- Não mover a guarda institucional para uma recusa global da batida de chão: a própria porta distingue `fato_de_chao`, e o evento de fechamento roda após commit sem barrar o registro do fato. `app/ponto/registro_batida.py:123`, `app/ponto/registro_batida.py:170`.

A limitação estrutural deve ficar explícita na consolidação: chamar a combinação “par” apenas por mesma disputa/dia/motivos inventaria a autoridade ausente; o caso fornecido identifica a mesma pausa por declaração do aval, enquanto a população inteira exige reconhecer a lacuna de identidade já documentada na E2. `app/chamados/services/declaracao_texto.py:48` [dados do caso somente no aval fornecido, sem arquivo local confirmado].

## 09/11 07:45 · sessao ff8f02861ed6 · - · - · origem=Ronald (tmux)

A recusa que explica o impasse está em `app/chamados/services/materializacao.py:875`: `_vizinha_mesmo_tipo(...)` combinada com `not _plantio_preenche_furo_na_data(...)`; o erro é montado em `:878-883`, antes de chegar à porta de escrita `:926-934`. O helper procura a vizinha imediata anterior/posterior em raio de **12 horas**, pelo tipo gravado, e não filtra `retratada_em` nem aprovação (`app/chamados/services/materializacao.py:85-106`).

A leitura da tela usa os mesmos helpers e classifica `bloqueada_paridade` em `app/chamados/services/materializacao.py:452-456`; o modal apresenta o detalhe em `app/templates/chamados/partials/modal_fio.html:229-241`. A validação chama `_materializar_uma` antes de varrer as demais perguntas, e devolve a validação recusada (`app/chamados/services/validacao.py:461-469`).

O texto **“marcacao […] colada nesse horario” / “duas seguidas iguais”** nasce em `app/chamados/catalogo/recusa.py:26-30`. `traduzir_recusa` escolhe essa frase por substrings do erro, sem comparar horários (`app/chamados/catalogo/recusa.py:77-83`). A casca VALIDAR traduz em `app/chamados/services/acoes_disputa.py:32-41`; regularizar traduz em `:169-174`; o followup também usa a tradução em `app/chamados/services/materializacao.py:1091` e `:1107`. Portanto “colada” descreve hoje uma guarda de até 12h, não proximidade de minutos (`app/chamados/services/materializacao.py:85-106`; `app/chamados/catalogo/recusa.py:26-30`).

As tolerâncias efetivamente encontradas são distintas:

- Mesmo tipo na porta: ±**60 segundos**, inclusivos; retorna a existente, sem criar (`app/ponto/registro_batida.py:106-115`).
- Reuso da materialização: ±**20 minutos**, mesmo tipo; não recusa (`app/chamados/juizes.py:235-247`).
- Colisão de tipo oposto: ±**300 segundos** com escala e marcos, ±**60 segundos** sem eles (`app/ponto/registro_batida.py:21-32`, `:138-151`).
- Exceção por marco próprio: ±**5 minutos** (`app/ponto/registro_batida.py:35-49`, `:145`).

Não foi encontrada uma constante nomeada “colada” ligada à regra PARIDADE; sua janela efetiva é `janela_h=12` (`app/chamados/services/materializacao.py:85`). Usar a tolerância CCT de 5 minutos para essa frase seria uma alteração não sustentada pelo predicado atual (`app/core/models.py:59`; `app/core/regua_cct.py:74`; `app/chamados/services/materializacao.py:85-106`).

`registrar_batida` não contém a recusa de duas iguais distantes: mesma espécie próxima é dedup; sua recusa temporal própria é colisão **oposta**, além da competência fechada para registro administrativo (`app/ponto/registro_batida.py:106-151`). Neste caminho a materialização passa `autoridade_humana=False, fato_de_chao=False` (`app/chamados/services/materializacao.py:927-934`).

**Histórico Git indisponível.** `git rev-parse HEAD`, `git log -L 870,922:app/chamados/services/materializacao.py` e `git blame -L 870,922 -- app/chamados/services/materializacao.py` retornaram `fatal: not a git repository`; a cópia não apresenta `.git`. Commit, data e mensagem que introduziram a recusa: **[não verificado]**.

Há evidência documental de antiguidade: o código atribui a guarda a “fio-quebrado 01/08”, a via a 20/08 e a ponte da lâmpada a 18/10 (`app/chamados/services/materializacao.py:85-90`, `:872-874`, `:884-889`). Isso permite registrar o que os comentários dizem; não permite afirmar a autoria Git **[não verificado]**.

Os commits citados aparecem no relato com outro escopo: `225256c1` como fecho/trava do repasse; `e017181f` como tratamento `REPASSE_RECUSADO`; `e9bf5de2` como consumidores de recusas (`app/docs/RELATO.md:27`, `:17`, `:11`). O relato registra merge/deploy da obra em 09/11 03:3x (`app/docs/RELATO.md:3`); a identificação com o deploy 01:52 fornecido pelo usuário e o commit `89df8c52` permanece **[não verificado]**.

## 09/11 07:44 · sessao 48f844357e2d · - · - · origem=Ronald (tmux)

O furo real do Agent é o sentido ausente: `_pertence_fila1('O9 (C)', ['O9'])` e `_pertence_fila1('O9 NOTURNO-X', ['O9'])` devolvem `False`; `_censo_nega` retorna `None` imediatamente, sem censo. (`bin/disjuntor.py:1683-1686`, `bin/disjuntor.py:1702-1703`)

O wrapper real recusa ambos antes da pertinência: `main_censo` exige `[A-Za-z0-9-]+`; caudas com espaço não chegam ao ramo compartilhado. (`bin/disjuntor.py:162`, `bin/disjuntor.py:2246-2248`, `bin/disjuntor.py:2265`)

Matriz literal do predicado:

| Obra literal | `O9` | `O9 (C)` | `OBRA-Q (A)` + `OBRA-Q (B)` | Outro token citando a obra inteira | `O142` |
|---|---:|---:|---:|---:|---:|
| `O9` | T | N | F | N | F |
| `O9 (C)` | **F*** | T | F | N | F |
| `O9 NOTURNO-X` | **F*** | F | F | N | F |
| `OBRA-Q` | F | F | N | N | F |
| `OBRA-Q (A)` | F | F | N | N | F |
| `o9` | N | N | F | N | F |
| `9ABC` | F | F | F | N | F |
| `O67b` | F | F | F | N | F |

T corresponde ao ramo `bin/disjuntor.py:1687-1688`; F ao ramo `bin/disjuntor.py:1685-1686`; N à exceção `bin/disjuntor.py:1689-1690`. As células são interpretadas pelo token `bin/disjuntor.py:1660-1666`, e a nomeação ignora caixa e respeita fronteiras em `bin/hook_stop_fila1.py:117`.

`*`: pela relação inversa indicada no laudo, o token inteiro `O9` aparece na obra e deve impedir FORA; sem identidade exata, o resultado conservador é NAO SEI. A relação inversa hoje não é consultada. (`bin/disjuntor.py:1683-1690`)

A matriz por formas alinhadas acrescenta:

| Obra | Célula | Predicado hoje |
|---|---|---|
| `OBRA-Q (A)` literal | `OBRA-Q` | **F*** |
| `OBRA-Q` | `OBRA-Q (A)` | T |
| `o9` | `o9` | T |
| `9ABC` | `9ABC` | T |
| `9ABC` | `9ABC (C)` | N |
| `O67b` | `O67b` | T |
| `O67b` | `O67b (C)` | N |
| `O1` | `O142` | F |

Esses resultados decorrem do aceite integral pelo regex de censo, da validação restrita do extrator e da comparação exata de token. (`bin/disjuntor.py:157`, `bin/disjuntor.py:162`, `bin/disjuntor.py:619-627`, `bin/disjuntor.py:1661-1666`, `bin/disjuntor.py:1683-1690`)

No Agent, `OBRA-Q (A)` vira `OBRA-Q`, portanto o F cru acima não representa furo dessa via. `o9`, `9ABC` e `O67b` não passam pela leitura da obra e recebem negativa sem obra. (`bin/disjuntor.py:602-621`, `bin/disjuntor.py:624-627`, `bin/disjuntor.py:1740-1741`, `bin/disjuntor.py:1758`, `bin/disjuntor.py:1847`)

Um teste wrapper `--censo 'O9 (C)'` não mata o mutante da direção ausente: ele já é recusado pela validação anterior. A cobertura “nas duas vias” desse mutante é impossível pela interface atual de `main_censo`; testar diretamente `_censo_nega(..., via='wrapper')` provaria apenas a função com esse rótulo. (`bin/disjuntor.py:2246-2248`, `bin/disjuntor.py:2265`, `bin/disjuntor.py:1693`)

## 09/11 07:44 · sessao 54c206c89ddb · - · - · origem=Ronald (tmux)

| Símbolo | Quem escreve/define | Leitores diretos |
|---|---|---|
| `_pertence_fila1` | `bin/disjuntor.py:1669` | `_censo_nega`, `bin/disjuntor.py:1702`; `main_censo`, `bin/disjuntor.py:2265` |
| `_token_da_celula` | `bin/disjuntor.py:1651` | Lista `exatas` de `_pertence_fila1`, `bin/disjuntor.py:1683` |
| `_casa_id_inteiro` | `bin/hook_stop_fila1.py:110` | `bin/disjuntor.py:1684`; `_relato_parou`, sobre ids parados, `bin/hook_stop_fila1.py:1301`, e item em curso, `bin/hook_stop_fila1.py:1313` |
| `_fila1_viva` | `bin/disjuntor.py:1626` | `_censo_nega`, `bin/disjuntor.py:1702`; `main_censo`, `bin/disjuntor.py:2264` |

O selo lê indiretamente pelas portas `_censo`/`_pre`; o mutador altera uma cópia textual do ramo `exatas` em `bin/tests/test_censo_antes_da_vaga.sh:714`; os testes de pertinência ficam em `bin/tests/test_censo_antes_da_vaga.sh:529`. O selo antigo de fronteira chama `_relato_parou` em `bin/tests/test_hook_parei_casa_id_inteiro.sh:53`, cobrindo os leitores externos ao censo em `bin/hook_stop_fila1.py:1301` e `bin/hook_stop_fila1.py:1313`.

| Achado | Classificação e evidência |
|---|---|
| 2: O67b/O51b/caixa/dígito | As formas entram na matriz **quando chegam como argumento da pertinência**; a recusa anterior é outro predicado: wrapper limita `[A-Z0-9-]`, `bin/codex_pergunta.sh:188`; extrator Agent usa `_OBRA_ID`, `bin/disjuntor.py:157`, e valida caixa em `bin/disjuntor.py:619`. |
| 2: FECHADA literal | Outro predicado, seleção/estado da fila: `_FECHADO` exige negrito ou tachado, `bin/hook_stop_fila1.py:59`; `_item_fechado` aplica em `bin/hook_stop_fila1.py:168`; exclusão acontece em `bin/hook_stop_fila1.py:250`. |
| 3: O9 (C) exata nunca abre | Outro predicado, representação aceita pelo censo: pertinência exata retorna True em `bin/disjuntor.py:1687`, mas `_censo_nega` recusa fora da forma em `bin/disjuntor.py:1704`; `main_censo` recusa antes da pertinência em `bin/disjuntor.py:2246`. O espelho O9 (C)×O9 é, separadamente, pertinência e pertence à matriz, `bin/disjuntor.py:1683`. |
| 4: PARADA excluída | Outro predicado, universo da fila: `_fila1_viva` usa `_itens_andaveis`, `bin/disjuntor.py:1637`; este exclui `_item_parado`, `bin/hook_stop_fila1.py:250`, decidido em `bin/hook_stop_fila1.py:180`. |
| 6: CORTES/PAREI | Outro predicado. Literal vivo em `bin/disjuntor.py:1511`; inserção via `bin/disjuntor.py:324`, somente acrescenta em `app/core/esteira_vigia.py:883`; aprovação não retira em `bin/disjuntor.py:1717`. Referência `CORTES.md:428` do anexo: [nao verificado]; a cópia termina em `app/docs/CORTES.md:425`. |
| 6: leitura como prova | Outro predicado, processo/papel: `CODEX_PAPEL=leitura` é aceito em `bin/hook_stop_fila1.py:726`, usado na prova em `bin/hook_stop_fila1.py:763`. Handoff publica o resultado em `bin/handoff_sessao.sh:129`. |
| 6: `_cru_abre_agent_copia` | Outro predicado, classificação de abertura: JSON truncado aceita qualquer `Agent` ou `wt-`, `bin/disjuntor.py:83`; normal exige não leitura e `COPIA_RE`, `bin/disjuntor.py:1740`. |
| 6: COPIA_RE | Outro predicado, reconhecimento de caminho: exige caminho com `/.../wt-`, `bin/disjuntor.py:153`; usado para classificar abertura em `bin/disjuntor.py:1740`. |

Os literais duplicados mencionados no opus 1 já leem `DJ.PAREI_CENSO` nesta cópia, em `bin/hook_stop_fila1.py:434` e `bin/hook_stop_fila1.py:783`. A negativa possui parser único em `bin/disjuntor.py:1515`, lido pela medida em `bin/disjuntor.py:2096`.

## 09/11 07:44 · sessao bd302f54505a · - · - · origem=Ronald (tmux)

O selo existente cobre o sentido obra nua → célula qualificada, mas não o espelho: entradas fake são `O9 (C)` e `OBRA-Q (SILENCIADO-ATE-X)` em `bin/tests/test_censo_antes_da_vaga.sh:45` e `:46`; os casos de obra nua estão em `:544`, `:559`, `:564`. A célula `O9` nua e única é preparada em `:576` e `:580`; é o ponto adequado para acrescentar o espelho antes da restauração em `:583`.

Linhas propostas nesse ponto, para as duas formas efetivamente legíveis pelo Agent, com breve ruim já preparado em `bin/tests/test_censo_antes_da_vaga.sh:214`:

```bash
for o in 'O9 (C)' 'O9 NOTURNO-X'; do
  s="$(_pre "$B" "$(_pl_agent general-purpose "Construa a obra $o na copia $WT. Brief: logs/e3/brief_ruim.md")")"
  _nega_censo_de "pertenca-agent-espelho-$o" "$o" "$s" || ok=1
  _sem_fora "pertenca-agent-espelho-$o" "$o" || ok=1
done
```

Esses casos matam o mutante que remove o reconhecimento reverso token→obra, isto é, restaura somente o atual `nomeiam = [c ... H._casa_id_inteiro(obra, c)]` de `bin/disjuntor.py:1684`. Hoje o predicado devolve `False` em `:1686`, e `_censo_nega` libera em `:1703`; após reconhecimento conservador, a obra qualificada ainda pode ser recusada pelo ramo fora da forma em `:1704` a `:1707`.

Para o wrapper, o controle pronto é:

```bash
for o in 'O9 (C)' 'O9 NOTURNO-X'; do
  rc="$(_censo "$B" "$o")"
  [ "$rc" = 1 ] || {
    _falha "pertenca-wrapper-espelho-$o" "rc=$rc, esperado 1"
    ok=1
  }
done
```

Esse controle **não mata o mutante da pertinência**: `main_censo` recusa as duas entradas antes de consultar a fila ou chamar `_pertence_fila1`, em `bin/disjuntor.py:2246` a `:2248`. Não seria correto chamar esse verde de cobertura do espelho pela integração wrapper. Para morder a regressão no predicado compartilhado, a asserção direta deve exigir que `_pertence_fila1(o, ['O9'])` não devolva `False`, aceitando `ValueError` como NAO SEI; o consumidor wrapper da função está em `bin/disjuntor.py:2265`.

Os mutantes já existentes para pertinência são célula crua, exato que cala outra, dois candidatos, wrapper próprio, token cru e substring, em `bin/tests/test_censo_antes_da_vaga.sh:714` a `:721`; nenhum retira um reconhecimento reverso, porque esse reconhecimento não existe em `bin/disjuntor.py:1684`.

## 09/11 07:43 · sessao 22308627191e · SONDA-SEM-GRANT · censo · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 09/11 07:42 · sessao 693ce8822a20 · SELOS-CANON-I1 · censo · origem=Code

**A v5.q não enunciava os dois sentidos da pertença.** Explicitava o achado de igualdade com célula crua; “nas duas vias (agente e wrapper)” designava percursos de execução. Ainda assim, perguntava pela semântica de pertença, incluindo “id cru x id com sufixo”. Portanto, classifico o achado 1 como **(a), pelo tema solicitado**, sem afirmar que o espelho tivesse sido expressamente formulado. (`i1_preverif_v5.q:3–10`)

As referências aos anexos abaixo contam as linhas do texto colado, incluindo linhas vazias, começando pela primeira linha de cada anexo.

| achado | classe | trecho q | trecho r | linha da triagem |
|---|---|---|---|---|
| **1 — espelho: token da célula dentro da obra** | **(a)**: pertença e diferença entre ID cru/qualificado estavam perguntadas; o espelho passou despercebido. O código procura somente a obra dentro da célula em `nomeiam`. (`bin/disjuntor.py:1683–1689`) | “Os achados 1 e 4 do laudo estao curados em TODOS os sitios”; “nas duas vias (agente e wrapper)?”; “id cru x id com sufixo”. (`i1_preverif_v5.q:5,10`) | “**Achado 1, nos dois pontos alterados: curado.**”; “Pedir `O9` diante dessa última célula resulta em NAO SEI”. Examina obra `O9`/célula `O9 (C)`, enquanto o laudo 2 aponta obra `O9 (C)`/célula `O9`. (`i1_preverif_v5.r:5–6`; `i1_opus_final_2.md:2`) | **L2:** “CURADO nas duas vias [...] -- conferido no diff.” Foi acolhimento da conclusão de cura, **não descarte de um espelho identificado**. (`i1_triagem_v5.txt:2`) |
| **2 — minúsculas no wrapper** | **(b)**: incompatibilidade vista e descartada. (`i1_preverif_v5.r:34`; `i1_triagem_v5.txt:6`) | “Algum leitor supoe significado diferente”. (`i1_preverif_v5.q:9`) | “O wrapper shell aceita somente `[A-Z0-9-]+`; o CLI aceita também minúsculas.” (`i1_preverif_v5.r:34`) | **L6:** “wrapper [A-Z0-9-] x CLI minusculas [...] anteriores, fora do diff. FILA.” (`i1_triagem_v5.txt:6`) |
| **2 — O67b/O51b também rejeitados pelo Agent** | **(a), parcialmente visto**: a resposta identificou wrapper/CLI, mas não diagnosticou o impedimento nas duas vias para esses IDs. A validação do Agent exige a gramática sensível à caixa. (`i1_preverif_v5.r:34`; `bin/disjuntor.py:157,616–620`; `app/docs/BACKLOG.md:1325,1327`) | “nas duas vias (agente e wrapper)?” (`i1_preverif_v5.q:5`) | O trecho pertinente limita-se a “O wrapper shell aceita somente [...]”; não apresenta o diagnóstico específico do Agent para O67b/O51b. (`i1_preverif_v5.r:34`) | **L6** descarta wrapper/CLI; não registra descarte específico desse impacto no Agent. (`i1_triagem_v5.txt:6`) |
| **2 — FECHADA sem negrito** | **(e)**: reconhecimento das formas de fechamento não foi perguntado especificamente. O caso e a restrição do reconhecedor constam da cópia. (`i1_preverif_v5.q:1–16`; `app/docs/BACKLOG.md:768`; `bin/hook_stop_fila1.py:59,168`) | O pedido próximo é sobre funções “que o diff cria ou muda”; não nomeia `_FECHADO` nem `FECHADA:`. (`i1_preverif_v5.q:7–10`) | Não há diagnóstico desse caso; a resposta descreve a delegação a `_itens_andaveis`. (`i1_preverif_v5.r:19`) | Sem entrada específica. (`i1_triagem_v5.txt:1–8`) |
| **3 — célula nua com cauda nunca abre** | **(b), com ressalva**: a impossibilidade de aprovação foi vista; a triagem justificou o descarte pelo efeito na medida. (`i1_preverif_v5.r:53`; `i1_triagem_v5.txt:3`) | “negativa de obra fora da forma nunca fecha intervalo em _censo_24h”; “Quem e afetado hoje?” (`i1_preverif_v5.q:13–14`) | “não torna possível aprovar a mesma identidade fora da forma”; “isso não equivale a vaga aberta”. (`i1_preverif_v5.r:53`) | **L3:** “nao fecha intervalo de obra qualificada -> FILA (pre-existente, so medida [...]). Nao bloqueia.” Essa justificativa não trata expressamente a indisponibilidade da abertura. (`i1_triagem_v5.txt:3`; `bin/disjuntor.py:1704–1707`) |
| **4 — PARADA excluída e tratada como FORA** | **(a)**: o universo produzido por `_fila1_viva` estava perguntado; a resposta descreveu a coleta sem apontar essa consequência. (`i1_preverif_v5.q:7`; `i1_preverif_v5.r:19`) | “QUEM ESCREVE e QUEM LE [...] (_casa_id_inteiro, _fila1_viva, _token_da_celula”. (`i1_preverif_v5.q:7`) | “Lê BACKLOG, delega a `_itens_andaveis` e devolve **lista de células cruas**, preservando duplicidades”. (`i1_preverif_v5.r:19`) | Sem descarte correspondente. A exclusão ocorre em `_itens_andaveis`; a ausência conduz a FORA. (`i1_triagem_v5.txt:1–8`; `bin/hook_stop_fila1.py:250–252`; `bin/disjuntor.py:1637,1685–1686,2269–2272`) |
| **5 — descartes confirmados não bloqueantes** | **(b)**: o próprio achado confirma os descartes, sem apresentar nova omissão. (`i1_opus_final_2.md:6`) | “sao regressao desta volta ou estado anterior? Quem e afetado hoje?” (`i1_preverif_v5.q:14`) | “o defeito já existia”; “Quem é afetado é o consumidor da medida”. (`i1_preverif_v5.r:53`) | **L3–6:** motivos de anterioridade, efeito na medida e casos fora do diff; **L7:** lacunas encaminhadas ao opus. (`i1_triagem_v5.txt:3–7`) |
| **6 — papel leitura aceito como prova** | **(a)**: a semântica da prova estava perguntada; a resposta mapeou consumidores sem apontar essa aceitação. O conjunto admite `censo` e `leitura`. (`i1_preverif_v5.q:8–10`; `bin/hook_stop_fila1.py:726,763`) | “prova_parei_censo, _eh_faixa_censo”; “Algum leitor supoe significado diferente”. (`i1_preverif_v5.q:8–9`) | “Produz `None`, prova `''` ou motivo → Stop, alarme e handoff.” (`i1_preverif_v5.r:27`) | Sem entrada específica. (`i1_triagem_v5.txt:1–8`) |
| **6 — retirada do PAREI; `_cru_abre_agent_copia`; `~/wt-*`** | **(e)** para esses temas específicos: ciclo de retirada, guarda de importação e forma abreviada do caminho não foram formulados. (`i1_preverif_v5.q:1–16`) | O trecho próximo pergunta quem “escreve o literal do PAREI fora do ponto unico”; não pergunta quando retirá-lo. (`i1_preverif_v5.q:6`) | “A escrita material do PAREI continua passando por `no_topo`”; não diagnostica retirada nem os dois reconhecedores de cópia. (`i1_preverif_v5.r:30`; `bin/disjuntor.py:65–84,153`) | Sem entrada específica. O laudo os chama de “herdados opus 1”, portanto não sustentam **(d)** por si mesmos. (`i1_triagem_v5.txt:1–8`; `i1_opus_final_2.md:7`) |
| **6 — teor de `CORTES.md:428`** | **[não verificado]**: não há conteúdo suficiente para classificar a obrigação desse corte. A cópia termina na linha 425. (`app/docs/CORTES.md:425`) | “Leis dos sitios em anexo”, mas o conteúdo desse anexo não foi fornecido. (`i1_preverif_v5.q:2`) | Não apresenta o teor desse corte. (`i1_preverif_v5.r:1–70`) | Sem entrada específica. (`i1_triagem_v5.txt:1–8`) |

**(c) aplica-se à comprovação operacional que faltava**, como execução integral/RED e eventos reais de produção; não à possibilidade de examinar estaticamente o espelho. A resposta declarou essas limitações, enquanto o código permite identificar a comparação unilateral. (`i1_preverif_v5.r:46,54,68`; `bin/disjuntor.py:1684`) Atribuir **(d), modo comum**, aos casos restantes exigiria evidência adicional sobre quem os examinou: **[não verificado]**.

## 09/11 07:41 · sessao e9a3685d39c6 · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 09/11 07:40 · sessao 44256442ce4b · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 09/11 07:40 · sessao 9d8baeed0bd3 · - · - · origem=Ronald (tmux)

Auditoria estática concluída; execução dos selos nesta sessão e estado vivo dos privilégios: [nao verificado].

| Selo | O que o código cobra | Lacuna / efeito do contrato |
|---|---|---|
| `test_codex_sonda.sh` | Usa envelope e Codex falsos; injeta nomes/CPF e verifica raspagem; falha do envelope/lista vazia bloqueia Codex com **rc 4**; cobra chamada padrão a `sonda_leitura.sh`, ausência de `psql/manage.py/credencial/docker` no wrapper e sandbox `read-only`; confere trilha obra/papel/hash/rc. (`bin/tests/test_codex_sonda.sh:21`, `:28`, `:46`, `:57`, `:71`, `:79`, `:93`) | **Não executa o envelope real nem observa SQL**: injeta `CODEX_SONDA_ENVELOPE` nas chamadas. (`bin/tests/test_codex_sonda.sh:49`) Contrato rc5 do envelope conflita com rc externo se exigir propagação: o selo hoje exige rc4 do wrapper. (`bin/tests/test_codex_sonda.sh:73`) |
| `test_sondas_rollback.sh` | AST encontra `atomic`, exige último comando `raise _Rollback()`, `exec` dentro dele e referência a `_canario_existe` depois; mutante retira o `raise`; exige `sondas.sh` referir `sonda_frota.sh` e `sondas_rollback.py`, sem `docker run` próprio. (`bin/tests/test_sondas_rollback.sh:15`, `:23`, `:28`, `:31`, `:37`, `:49`, `:63`) | **É prova de forma AST, não execução/efeito no PostgreSQL**: lê e analisa o fonte. (`bin/tests/test_sondas_rollback.sh:42`) O envelope real faz `CREATE TEMP TABLE` antes de executar a sonda: contradiz contrato global “nenhum CREATE”. (`bin/sondas_rollback.py:49`) Tirar canário sem substituir desenho deixa teste vermelho por ausência da conferência; trocar roteamento de `sondas.sh` deixa vermelho por exigência de frota. (`bin/tests/test_sondas_rollback.sh:38`, `:69`) |
| `test_ensaio_fora_do_postgres_de_prod.sh` | Consulta `pg_database` de `saas_db`; cobra ausência de `sombra/juliani_ref_*`; busca `dropdb/createdb/pg_restore` em shell de `bin/` apontando para prod; cobra host literal `sombra_db`; pares sintéticos mordem os detectores. (`bin/tests/test_ensaio_fora_do_postgres_de_prod.sh:38`, `:58`, `:65`, `:79`, `:94`) | O detector de escrita só procura esses três utilitários; não fiscaliza GRANT, ALTER ROLE, CREATE TEMP ou Python da sonda. (`bin/tests/test_ensaio_fora_do_postgres_de_prod.sh:60`) A consulta de catálogo real impede executar esse selo sem banco, e ele cria fixtures temporárias. (`bin/tests/test_ensaio_fora_do_postgres_de_prod.sh:38`, `:99`) |
| `test_recursos_sourced.sh` | Cobra source imune aos argumentos, proíbe suite em `saas_core`, exige cpuset nas suites; para `sombra_db`, exige cpuset/restart/memória no nascimento/update e pertencimento a `TESTE_CONTAINERS`. (`bin/tests/test_recursos_sourced.sh:18`, `:31`, `:38`, `:88`) | Não pergunta por SQL nem role leitor; inspeciona recursos/forma dos scripts. (`bin/tests/test_recursos_sourced.sh:94`) |
| `test_cpuset_de_teste.sh` | Procura `--cpuset-cpus` à mão e confere containers vivos cujo comando contém `manage.py test`; pares sintéticos verificam detector. (`bin/tests/test_cpuset_de_teste.sh:26`, `:41`, `:55`) | A medição viva filtra **suite**, não sondas: não mede isolamento real do container de `sonda_leitura.sh`. (`bin/tests/test_cpuset_de_teste.sh:44`) |
| `test_codex_pergunta.sh` | Com Codex falso e repositórios temporários, cobra sandbox, quatro papéis leitores, guardas do construtor e mutantes; não é prova de PostgreSQL. (`bin/tests/test_codex_pergunta.sh:2`, `:24`, `:73`, `:86`) | Alterações no envelope não são exercitadas por esse cenário; ele substitui apenas o Codex e testa o wrapper. (`bin/tests/test_codex_pergunta.sh:75`, `:86`) |
| `prova_codex_pergunta_real.sh` | No modo leitor, pede criações na cópia/viva/tmp e compara hash/status/ausência de arquivos. (`bin/tests/prova_codex_pergunta_real.sh:174`, `:180`, `:184`) | Não é prova de role/SQL. O selo de sonda só o invoca com `CODEX_SELO_REAL=1`. (`bin/tests/test_codex_sonda.sh:107`) |

O próprio `sonda_leitura.sh` documenta que `test_sonda_leitura.sh` **ainda não pousou** e que a proteção restante era documental mais RED em log; esse log não foi consultado. (`bin/sonda_leitura.sh:16`; estado do RED: [nao verificado]).

Os selos de host são chamados pelo loop da régua; falha interrompe antes da suite. Isso não prova que tenham sido executados nem que estejam verdes nesta árvore. (`bin/regua.sh:138`; resultados: [nao verificado]). O pre-push documenta que esse loop não roda ali. (`bin/pre-push.sh:13`).

Pontos essenciais para a consolidação do selo novo:

- O canário de rollback já é DDL executado pelo envelope Python, independentemente do SQL enviado a `psql`; vem antes do `exec` da sonda. (`bin/sondas_rollback.py:50`)
- O contador de tentativas soma apenas `n_tup_ins + n_tup_upd + n_tup_del`; não conta comandos DDL/DCL nem exige que tentativas sejam zero para emitir `OK`. (`bin/sondas_rollback.py:35`, `:72`)
- `sondas.sh` aceita qualquer linha `SONDA_ROLLBACK|OK|*` para mover a sonda a “feitas”, inclusive contador de tentativas positivo. (`bin/sondas.sh:38`)
- A sonda Python é arbitrária e recebe execução direta; um docker falso que só registre `psql` não observa SQL emitido por Django/driver dentro desse processo. (`bin/sonda_leitura.sh:239`, `bin/sondas_rollback.py:53`)
- O próprio código reconhece que `SET default_transaction_read_only=off` desfaz o parâmetro de sessão; não basta provar default `on`. (`bin/sonda_leitura.sh:13`)

Proposta ainda não implementada [nao verificado]: selo do envelope real, em raiz descartável, com Docker falso que registre SQL por stdin/`-c`/`-f` e recuse operações desconhecidas; matriz prod/sombra, grants prontos/faltantes, role ausente, login divergente, erros antes/depois do Python, chamada direta e via wrapper. Deve verificar controle positivo de consulta, nenhum provisionamento, rc5 nomeando falta, trilha coerente e mutante que reinsere `_garantir_grants`. O contador precisa representar a observação feita; campo literal `sql_escrita=0` não acrescenta evidência [nao verificado].

Proposta adicional, não provada na cópia [nao verificado]: separar selo host do envelope de prova de capacidades efetivas em cluster descartável equivalente, incluindo CREATE em `public/juliani`, CREATE TEMP, DML, sequência, GRANT/ALTER, memberships/SET ROLE, propriedade, privilégios herdados de PUBLIC e funções SECURITY DEFINER. Um grep de verbos em chamadas `psql` não demonstra recusa a SQL originado no Python arbitrário, cujo ponto de execução está em `bin/sonda_leitura.sh:240`.

## 09/11 07:39 · sessao 55358bb0102a · - · - · origem=Ronald (tmux)

Para o **espelho específico**, recomendo **(e)**: a pergunta não escreve a condição “token da célula dentro da obra”. Ela contextualiza “pertenca a fila 1 por igualdade com celula crua” e pede cura “nas duas vias (agente e wrapper)”; essas duas vias são percursos de execução, não os dois sentidos da relação. (`i1_preverif_v5.q:3-6`, numeração do anexo colado)

A pergunta aborda o tema geral: “id cru x id com sufixo” e mordida de `casos_pertenca`. Isso permite discutir **(a)** numa classificação ampla do tema pertença, mas não sustenta afirmar que a pergunta exigiu explicitamente os dois sentidos. (`i1_preverif_v5.q:7-10,15`)

A resposta demonstra apenas o sentido inverso: “Pedir `O9` diante dessa última célula resulta em NAO SEI”. A triagem declara “CURADO nas duas vias”; não relata ter visto e descartado o espelho. Portanto **(b)** não cabe para esse defeito. (`i1_preverif_v5.r`, §1, segundo item; linha absoluta [não verificado]; `i1_triagem_v5.txt:2`)

## 09/11 07:39 · sessao a46b11001e13 · - · - · origem=Ronald (tmux)

| Achado | Classe sugerida | Trecho literal q | Trecho literal r | Triagem/motivo |
|---|---|---|---|---|
| 5 — descartes confirmados | **(b)** | “sao regressao desta volta ou estado anterior? Quem e afetado hoje?” (`i1_preverif_v5.q`, item 4) | “O defeito já existia”; “Quem é afetado é o consumidor da medida” (`i1_preverif_v5.r`, seção 4) | Linha 3: “FILA (pre-existente, so medida [...] Nao bloqueia”. Linhas 4–6: demais limitações anteriores. Linha 7: lacunas encaminhadas ao opus. |
| 6 — PAREI/faixa e papel leitura | **(a), com ressalva de escopo** | “prova_parei_censo, _eh_faixa_censo”; “Algum leitor supoe significado diferente” (`i1_preverif_v5.q`, item 2) | “Classifica a linha literal válida”; “Produz `None`, prova `''` ou motivo” (`i1_preverif_v5.r`, tabela seção 2) | Ausente da triagem. A pergunta trouxe as funções e sua semântica; a resposta mapeou leitores, mas não apontou esses resíduos. O ciclo de retirada do PAREI não foi expressamente pedido. |
| 6 — `_cru_abre_agent_copia` e `COPIA_RE`/`~/wt-*` | **(e)** para os temas específicos | Não menciona essas funções nem caminhos `~/wt-*`; pedido geral sobre achados 1/4 e funções alteradas (`i1_preverif_v5.q`, itens 1–2). | Não os discute (`i1_preverif_v5.r`, seções 1–5). | Ausentes da triagem. Não cabe afirmar “nenhum viu”: o próprio laudo os apresenta como “herdados opus 1” (`i1_opus_final_2.md`, achado 6). |
| 6 — `CORTES.md:428` | **Conteúdo não classificável: [não verificado]** | A pergunta menciona “Leis dos sitios em anexo”, mas esse anexo não foi fornecido (`i1_preverif_v5.q`, linha inicial de anexos). | Sem discussão desse corte (`i1_preverif_v5.r`). | Ausente; o teor da referência não está no dado fornecido. |

Evidência do código:

- Papel `leitura` conta como prova: `bin/hook_stop_fila1.py:726`; o reconhecimento usa o conjunto em `bin/hook_stop_fila1.py:763`.
- A faixa preserva linhas de censo: `bin/hook_stop_fila1.py:481`; o escritor adiciona/reconhece a linha sem apresentar remoção nesse percurso: `bin/disjuntor.py:301–326`. Uma linha provada libera: `bin/hook_stop_fila1.py:804–806`.
- `_cru_abre_agent_copia` decide pela presença textual de `wt-`: `bin/disjuntor.py:82–84`; `COPIA_RE` exige caminho com barras antes de `wt-`: `bin/disjuntor.py:153`, usado na classificação do Agent em `bin/disjuntor.py:1740` e `bin/disjuntor.py:1844`.
- A cópia de `app/docs/CORTES.md` termina na linha 425; o conteúdo citado como `:428` permanece **[não verificado]** (`app/docs/CORTES.md:425`).

## 09/11 07:39 · sessao c019cdd8aca8 · - · - · origem=Ronald (tmux)

Sugestão de classificação por subtema:

| Achado | Classe | Trecho literal q | Trecho literal r | Triagem / ressalva |
|---|---|---|---|---|
| 2 — wrapper/minúsculas | **b** | “Algum leitor supoe significado diferente” (`i1_preverif_v5.q:9`) | “O wrapper shell aceita somente `[A-Z0-9-]+`; o CLI aceita também minúsculas.” | `i1_triagem_v5.txt:6`: “wrapper [A-Z0-9-] x CLI minusculas [...] anteriores, fora do diff. FILA.” Código: `bin/codex_pergunta.sh:188`; `bin/disjuntor.py:162`. |
| 2 — impacto específico O67b/O51b e rejeição Agent | **a**, como extensão não diagnosticada da incompatibilidade já identificada | “nas duas vias (agente e wrapper)” (`i1_preverif_v5.q:5`) | A resposta menciona wrapper/CLI, sem nomear O67b/O51b ou diagnosticar a rejeição Agent. (`i1_preverif_v5.r`, linha exata [não verificado]) | Não há descarte específico do Agent. Código: `bin/disjuntor.py:616`; IDs: `app/docs/BACKLOG.md:1325`, `app/docs/BACKLOG.md:1327`. |
| 2 — FECHADA sem negrito | **e**, se “tema presente” exigir pergunta específica sobre fechamento | Não há pergunta específica sobre formas de FECHADA. (`i1_preverif_v5.q:1`, demais linhas [não verificado]) | Não há diagnóstico desse caso. [não verificado] | Sem descarte específico. Caso em `app/docs/BACKLOG.md:768`; regex em `bin/hook_stop_fila1.py:59`; aplicação em `bin/hook_stop_fila1.py:168`. |
| 3 — identidade fora da forma nunca aprova | **b**, com ressalva sobre o motivo da triagem | “negativa de obra fora da forma nunca fecha intervalo em _censo_24h” (`i1_preverif_v5.q:13`) | “a volta corrige a leitura da negativa, mas não torna possível aprovar a mesma identidade fora da forma” | `i1_triagem_v5.txt:3` descarta o residual como “pre-existente, so medida”. Essa linha não analisa expressamente a indisponibilidade de abertura; a recusa está em `bin/disjuntor.py:1704`. |
| 4 — PARADA excluída antes da pertença | **a** | “QUEM ESCREVE e QUEM LE cada funcao/constante [...] (_casa_id_inteiro, _fila1_viva, _token_da_celula” (`i1_preverif_v5.q:7`) | “Lê BACKLOG, delega a `_itens_andaveis` e devolve **lista de células cruas**, preservando duplicidades” | Sem descarte correspondente. A resposta descreve a coleta, mas não diagnostica PARADA→FORA: `bin/disjuntor.py:1637`; `bin/hook_stop_fila1.py:250`; `bin/disjuntor.py:2269`. |

A ausência atual de célula viva do achado 3 é informação do laudo fornecido, não conclusão obtida por acesso a produção (`i1_opus_final_2.md:4`).

## 09/11 06:43 · sessao 41bbd4564418 · DEPLOY-EXCLUSAO-ENVELOPE · verificador · origem=Code

**Parecer: não daria VERDE nesta volta.** O fluxo de sucesso fecha as duas janelas para envelopes desta versão, mas encontrei autoespera no `vigia_arvore`, execução auxiliar sem compartilhada, retenção sem teto global e lacunas nos leitores e no selo. (`bin/deploy.sh:316`, `bin/deploy.sh:442`, `app/core/esteira_vigia.py:1267`, `bin/cron_run.sh:50`, `app/core/management/commands/pipeline_placar.py:74`, `bin/tests/test_deploy_exclusao_cron.sh:168`)

Validação por execução, duração real dos deploys e comportamento observado em produção: **[nao verificado]**. [nao verificado]

**1. Fechamento das janelas, saídas e morte do deploy** [nao verificado]

No fluxo de sucesso, **sim, para quem passa pelo envelope novo e disputa o mesmo arquivo**: o único apply está depois da aquisição exclusiva; o único restart também, com a última consulta de marcas já dentro dela. O envelope adquire compartilhada antes de criar MARCA ou lançar `CMD`; o unlock explícito só vem depois das três provas de rota. (`bin/deploy.sh:319`, `bin/deploy.sh:337`, `bin/deploy.sh:363`, `bin/deploy.sh:364`, `bin/deploy.sh:437`, `bin/deploy.sh:442`, `bin/cron_run.sh:17`, `bin/cron_run.sh:22`, `bin/cron_run.sh:24`)

Não localizei ramo que aplique migration ou execute restart sem passar pela exclusiva. HUP sem migration fica deliberadamente fora dela; `--conferir` termina antes desses atos. (`bin/deploy.sh:237`, `bin/deploy.sh:316`, `bin/deploy.sh:336`, `bin/deploy.sh:359`, `bin/deploy.sh:381`)

**A promessa “segura até reload conferido” vale somente no sucesso.** `falhou()` executa `exit 1`; pode encerrar após falha do migrate, do HUP, da troca de workers ou das rotas, sem chegar ao unlock normal. Não há mecanismo nesses caminhos que mantenha a exclusão até uma recuperação posterior. (`bin/deploy.sh:92`, `bin/deploy.sh:337`, `bin/deploy.sh:381`, `bin/deploy.sh:394`, `bin/deploy.sh:440`, `bin/deploy.sh:442`)

O arquivo permanece; a posse está no fd 4. A saída fecha o descritor do processo, mas **não garante liberação imediata se algum descendente ainda o conservar**: a biblioteca declara herança, e o lançamento do comando não fecha o fd. Descendente efetivamente sobrevivendo à morte de um deploy nesta produção: **[nao verificado]**. (`bin/trava_deploy_exclusao.sh:10`, `bin/trava_deploy_exclusao.sh:32`, `bin/trava_deploy_exclusao.sh:42`, `bin/cron_run.sh:24`)

A transição de versão também merece distinção: o restart relê marcas para acomodar envelope antigo já em memória; o migrate normal não consulta essas marcas. Portanto a exclusiva, sozinha, não demonstra proteção contra um cron antigo que ainda não participa do protocolo. Existência desse cron durante a publicação: **[nao verificado]**. (`bin/deploy.sh:108`, `bin/deploy.sh:315`, `bin/deploy.sh:337`, `bin/deploy.sh:362`, `bin/deploy.sh:363`)

**2. Envelope, MARCA/FIM e rc 75**

O `CMD` só roda no ramo armado após aquisição compartilhada bem-sucedida. MARCA fica nesse ramo; FIM só é criado para rc 0 ou 2. Sem vez, o envelope grava placar com rc 75, não cria MARCA/FIM e sai pelo epílogo. (`bin/cron_run.sh:17`, `bin/cron_run.sh:20`, `bin/cron_run.sh:22`, `bin/cron_run.sh:24`, `bin/cron_run.sh:27`, `bin/cron_run.sh:30`, `bin/cron_run.sh:39`, `bin/cron_run.sh:55`)

**Há uma exceção à afirmação ampla “nenhum comando entra no core”:** sem compartilhada, o epílogo ainda tenta executar `docker exec ... cron_quebrado`. Esse alarme pode entrar no core enquanto a exclusiva responsável pela recusa continua tomada. Sua saída é descartada; a FLAG só nasce se a chamada funcionar. (`bin/cron_run.sh:49`, `bin/cron_run.sh:50`)

O 75 não identifica exclusivamente timeout de deploy: também cobre biblioteca ausente, falha de `mkdir`, abertura do arquivo ou aquisição; além disso, um `CMD` pode devolver 75 por conta própria. A linha do placar não registra uma causa separada. (`bin/cron_run.sh:17`, `bin/cron_run.sh:24`, `bin/cron_run.sh:27`, `bin/cron_run.sh:30`, `bin/trava_deploy_exclusao.sh:31`, `bin/trava_deploy_exclusao.sh:32`, `bin/trava_deploy_exclusao.sh:34`)

Há significados anteriores de 75: “sem vez” na trava de testes, tratado pelo pre-push como “não rodou”; limite/indisponibilidade e pausa no wrapper Codex. Isso não constitui colisão automática entre scripts, mas impede interpretar 75 como um significado global único. (`bin/trava_teste.sh:18`, `bin/trava_teste.sh:38`, `bin/pre-push.sh:149`, `bin/codex_pergunta.sh:22`, `bin/codex_pergunta.sh:41`)

Para os leitores de cron, 75 é erro: `EXITS_DE_ALARME=(0,2)`; o vigia aceita o inteiro e abre pauta sem tratamento particular para 75. A corrente registra esse retorno, continua os elos e não faz retry; um dependente sem FIM pode terminar no próprio teto com rc 2 e ALARME. (`app/core/services/crons_quebrados.py:16`, `app/core/services/crons_quebrados.py:46`, `app/core/management/commands/cron_quebrado.py:16`, `app/core/management/commands/cron_quebrado.py:37`, `bin/encadeado.sh:64`, `bin/encadeado.sh:66`, `bin/encadeado.sh:92`)

**3. Autoespera, aninhamentos e segundo plano**

O reload das 03:30 evita autoespera: é RAW envelopado, mas `--reload-agendado` recusa migration pendente e não arma exclusiva. (`app/config/crons.py:326`, `app/config/crons.py:1863`, `bin/deploy.sh:211`, `bin/deploy.sh:316`)

**Existe outro caminho cron → deploy:** `vigia_arvore.sh` chama `esteira_vigia.py curar`, que pode chamar deploy `--sem-sombra`, inclusive novamente ao desfazer o revert. Se esse deploy precisar aplicar migration, tentará exclusiva enquanto seu envelope ancestral conserva compartilhada: espera até 600 s e recusa contra si mesmo. A chamada Python também descarta o retorno do deploy e segue para os testes. (`app/config/crons.py:296`, `bin/vigia_arvore.sh:176`, `app/core/esteira_vigia.py:1267`, `app/core/esteira_vigia.py:1269`, `app/core/esteira_vigia.py:1275`, `bin/cron_run.sh:24`, `bin/deploy.sh:233`, `bin/deploy.sh:319`, `bin/trava_deploy_exclusao.sh:26`)

O outro cron encontrado chamando deploy, `vigia_logins --auto`, usa `--sem-migrate`; com pendência ele recusa, sem pendência não pede exclusiva. (`app/config/crons.py:303`, `bin/vigia_logins.sh:87`, `bin/deploy.sh:215`, `bin/deploy.sh:316`)

A corrente não aninha envelopes por sua construção: `; encadeado.sh` vem depois do envelope raiz, e cada elo faz `exec` de seu próprio envelope. `deploy_agendado` entra diretamente por cron.d e executa a esteira recebida, sem envelope próprio; as fatias realmente agendadas são **[nao verificado]**. (`app/config/crons.py:1885`, `bin/encadeado.sh:102`, `bin/encadeado.sh:104`, `bin/deploy_agendado.sh:55`, `bin/deploy_agendado.sh:73`)

A cadeia toma árvore e régua antes do deploy final. Já `sombra.sh --conferir`, chamado pelo deploy, não passa pelos ramos que adquirem a trava da sombra e não executa o bloco de crons. (`bin/molde_fatia/cadeia.sh:11`, `bin/molde_fatia/cadeia.sh:14`, `bin/molde_fatia/cadeia.sh:89`, `bin/deploy.sh:162`, `bin/sombra.sh:438`, `bin/sombra.sh:451`, `bin/sombra.sh:463`)

**Comandos efetivamente agendados em `crons.py` com lançamento explícito por `&`, `nohup` ou `setsid`: nenhum localizado.** O gerador executa RAW/Django sincronamente e acrescenta a corrente com `;`. Os jobs `Popen` citados no arquivo pertencem a `FORA_DE_PIPELINE`. (`app/config/crons.py:282`, `app/config/crons.py:350`, `app/config/crons.py:594`, `app/config/crons.py:688`, `app/config/crons.py:705`, `app/config/crons.py:769`, `app/config/crons.py:1369`, `app/config/crons.py:1863`, `app/config/crons.py:1885`)

Há assíncronos vizinhos: fabricante usa `setsid nohup ... &`; o observador de árvore fecha fds 6–9, mas deixa 4 aberto e vive enquanto o pai existir. Caminho demonstrado de um cron até esses lançamentos, ou filho de cron segurando compartilhada indefinidamente: **[nao verificado]**. (`bin/fabricante.sh:187`, `bin/trava.sh:68`, `bin/trava.sh:69`)

**4. Quem escreve e quem lê**

| Objeto | Escritores e consumidores localizados |
|---|---|
| Caminho da trava | Uma definição literal, derivada da raiz da biblioteca; usada para criar diretório e abrir o arquivo. (`bin/trava_deploy_exclusao.sh:16`, `bin/trava_deploy_exclusao.sh:31`, `bin/trava_deploy_exclusao.sh:32`) |
| fd 4 | A função comum abre; `flock` adquire; fecha na recusa; a soltura explícita faz unlock e close. (`bin/trava_deploy_exclusao.sh:32`, `bin/trava_deploy_exclusao.sh:34`, `bin/trava_deploy_exclusao.sh:37`, `bin/trava_deploy_exclusao.sh:42`) |
| Funções da biblioteca | Wrappers compartilhado/exclusivo chamam a implementação comum; consumidores são envelope e deploy; soltura explícita só no deploy. (`bin/trava_deploy_exclusao.sh:28`, `bin/trava_deploy_exclusao.sh:40`, `bin/trava_deploy_exclusao.sh:41`, `bin/cron_run.sh:17`, `bin/deploy.sh:319`, `bin/deploy.sh:442`) |
| `espera_deploy` | Envelope escreve; leitores explícitos localizados somente no selo novo. (`bin/cron_run.sh:30`, `bin/tests/test_deploy_exclusao_cron.sh:165`, `bin/tests/test_deploy_exclusao_cron.sh:182`, `bin/tests/test_deploy_exclusao_cron.sh:237`) |

Os leitores operacionais diretos do JSONL localizados foram estes; nenhum usa `espera_deploy`:

| Leitor | Interpretação e consequência |
|---|---|
| `crons_quebrados` | Usa chegada `ini` para escolher a última corrida em 26 h e classifica por exit. (`app/core/services/crons_quebrados.py:36`, `app/core/services/crons_quebrados.py:39`, `app/core/services/crons_quebrados.py:42`, `app/core/services/crons_quebrados.py:46`) |
| `pipeline_placar` | Toda linha incrementa `rodou`: uma tentativa sem comando vira `rodou=1, erros=1`, situação ERRO. (`app/core/management/commands/pipeline_placar.py:73`, `app/core/management/commands/pipeline_placar.py:74`, `app/core/management/commands/pipeline_placar.py:78`, `app/core/management/commands/pipeline_placar.py:92`) |
| `crons_duracao.py` | Inclui `dur` sem filtrar exit; tentativas sem comando entram no p95 e podem reduzir a medida. Efeito efetivo: **[nao verificado]**. (`bin/crons_duracao.py:69`, `bin/crons_duracao.py:75`, `bin/crons_duracao.py:76`) |
| `corrida_no_horario` | Recebe `ini` e filtra seu minuto, embora a documentação diga “começou no horário”. Agora isso mede chegada ao envelope, podendo anteceder o comando em até 300 s. (`app/config/crons.py:2305`, `app/config/crons.py:2328`, `app/config/crons.py:2332`, `bin/cron_run.sh:7`, `bin/cron_run.sh:24`) |

`dur` exclui a aquisição, mas inclui preparação da MARCA; `fim−ini` inclui a espera. Nenhum dos leitores diretos acima calcula essa diferença. Há, portanto, uma mudança real entre “chegou”, “rodou” e “começou o comando” que o comentário novo não resolve nos consumidores. (`bin/cron_run.sh:7`, `bin/cron_run.sh:19`, `bin/cron_run.sh:22`, `bin/cron_run.sh:29`, `bin/cron_run.sh:30`)

**5. Tetos de 300/600 e corrente**

**600 s limita aquisição, não retenção.** Depois de obter exclusiva, o deploy inclui migrate sem timeout, consultas `docker top` sem timeout, até 270 s nominais de sleeps dos workers, três esperas de disponibilidade de até 180 s cada e provas de rota sem timeout. Não existe teto global demonstrável para quanto segura. (`bin/trava_deploy_exclusao.sh:26`, `bin/trava_deploy_exclusao.sh:34`, `bin/deploy.sh:337`, `bin/deploy.sh:377`, `bin/deploy.sh:386`, `bin/deploy.sh:391`, `bin/deploy.sh:403`, `bin/deploy.sh:406`, `bin/deploy.sh:419`, `bin/deploy.sh:442`)

Assim, **um deploy com migration pode terminar com sucesso e ainda fazer um cron atingir 300 s**, sair 75 e alarmar sem FIM. A afirmação de que 300 “cobre” a troca e deixa 600 para a corrente não demonstra cobertura de migration, disponibilidade e rotas. (`bin/trava_deploy_exclusao.sh:17`, `bin/trava_deploy_exclusao.sh:21`, `bin/cron_run.sh:27`, `bin/cron_run.sh:39`, `bin/cron_run.sh:50`)

Os 900 s pertencem ao dependente e começam antes da consulta dos carimbos; não são folga integral garantida quando o deploy começa. HE espera deslogados por 1800 s, mas quem espera HE continua com 900 s. Tempo efetivo disponível e duração real do deploy: **[nao verificado]**. (`bin/encadeado.sh:86`, `bin/encadeado.sh:92`, `app/config/crons.py:1550`, `app/config/crons.py:1563`)

**6. Classes de reprovação anteriores**

- **Fonte do caminho:** há uma definição textual, mas ela é relativa à biblioteca carregada. Deploy carrega da própria raiz; envelope carrega da viva absoluta. Executar deploy de outra cópia pode disputar outro arquivo enquanto opera os mesmos containers. Ocorrência operacional: **[nao verificado]**. (`bin/trava_deploy_exclusao.sh:16`, `bin/deploy.sh:317`, `bin/cron_run.sh:17`, `bin/deploy.sh:337`)
- **Derivação paralela:** aquisição, tetos e descritor estão centralizados na biblioteca; a diferença de raiz acima continua possível. (`bin/trava_deploy_exclusao.sh:16`, `bin/trava_deploy_exclusao.sh:21`, `bin/trava_deploy_exclusao.sh:26`, `bin/trava_deploy_exclusao.sh:28`)
- **Ordem:** cadeia → árvore → régua → exclusiva está demonstrada. Não demonstrei ciclo inverso envolvendo árvore/régua; a autoespera concreta é compartilhada ancestral → exclusiva no auto-revert. (`bin/molde_fatia/cadeia.sh:11`, `bin/molde_fatia/cadeia.sh:14`, `bin/molde_fatia/cadeia.sh:89`, `app/core/esteira_vigia.py:1267`, `bin/deploy.sh:319`)
- **Relógio:** espera registrada e `dur` usam subtração de `date +%s`, sem proteção contra salto do relógio; podem divergir do tempo transcorrido ou ficar negativos. (`bin/trava_deploy_exclusao.sh:33`, `bin/trava_deploy_exclusao.sh:35`, `bin/cron_run.sh:19`, `bin/cron_run.sh:29`)
- **Campo com dois significados:** `rodou` passa a incluir tentativa recusada; 75 agrega causas diferentes; filtro de “início” usa chegada. (`app/core/management/commands/pipeline_placar.py:74`, `bin/cron_run.sh:17`, `bin/cron_run.sh:27`, `app/config/crons.py:2328`)

Os mutantes “sem exclusiva” e “exit 75 antes do epílogo” têm asserções capazes de acusá-los em a1/a2 e b2. Porém o arquivo apenas aceita fontes alternativas; execução e mordida observadas dos mutantes/RED: **[nao verificado]**. (`bin/tests/test_deploy_exclusao_cron.sh:32`, `bin/tests/test_deploy_exclusao_cron.sh:135`, `bin/tests/test_deploy_exclusao_cron.sh:152`, `bin/tests/test_deploy_exclusao_cron.sh:187`)

**Falta mordida para soltura prematura:** b1 exige comando após HUP; b3, após restart. O Docker falso troca workers e responde às provas imediatamente; esses casos não demonstram reprovar soltura antes da conferência final. d4 apenas verifica `30 < teto < 900`, sem validar orçamento temporal. (`bin/tests/test_deploy_exclusao_cron.sh:90`, `bin/tests/test_deploy_exclusao_cron.sh:96`, `bin/tests/test_deploy_exclusao_cron.sh:168`, `bin/tests/test_deploy_exclusao_cron.sh:205`, `bin/tests/test_deploy_exclusao_cron.sh:258`)

**7. Contrato e selos vizinhos**

Pela leitura, não vejo quebra das condições textuais de `contrato_deploy.sh`: HUP único e conjunto, restart manual único precedido de recusa e três provas de rota continuam presentes. Esse contrato não cobra a nova exclusiva; sua checagem de migration usa a primeira ocorrência de `migrate_schemas`, que é consulta, não apply. Resultado executado: **[nao verificado]**. (`bin/contrato_deploy.sh:24`, `bin/contrato_deploy.sh:33`, `bin/contrato_deploy.sh:47`, `bin/contrato_deploy.sh:54`, `bin/deploy.sh:184`, `bin/deploy.sh:337`, `bin/deploy.sh:437`)

Copiar a biblioteca sem substituir caminho nos vizinhos está correto nesta implementação: ela deriva o lock de seu próprio local. Entretanto, os vizinhos continuam plantando MARCA sem adquirir compartilhada; preservam a prova da recusa por marca, não comprovam o protocolo novo. O selo novo entra na régua pelo glob existente. (`bin/trava_deploy_exclusao.sh:16`, `bin/tests/test_deploy_reload_gracioso.sh:41`, `bin/tests/test_deploy_reload_gracioso.sh:92`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:71`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:78`, `bin/regua.sh:138`)

Há divergência do contrato anterior registrado: cron 600/deploy não bloqueante/`espera_lock`/dois contadores ESTADO versus cron 300/deploy esperando 600/`espera_deploy`, sem consumo operacional desse campo nos leitores encontrados. Aval posterior para essa mudança de desenho: **[nao verificado]**. (`app/docs/RELATO.md:25`, `bin/trava_deploy_exclusao.sh:21`, `bin/trava_deploy_exclusao.sh:26`, `bin/trava_deploy_exclusao.sh:34`, `bin/cron_run.sh:30`)

## 09/11 06:42 · sessao c9691c154609 · - · - · origem=Ronald (tmux)

1. **Caminho de sucesso fecha as duas janelas para envelopes desta versão.** O único apply está em `bin/deploy.sh:337`, e o único restart em `bin/deploy.sh:364`; ambos passam pela aquisição exclusiva em `bin/deploy.sh:316-325`. O envelope só cria MARCA e executa após adquirir compartilhada (`bin/cron_run.sh:17-24`). A lib mantém a trava no fd 4 (`bin/trava_deploy_exclusao.sh:32-36`).

2. **Falha não preserva a exclusão até uma prova concluída.** `falhou()` chama `exit 1` (`bin/deploy.sh:92`); pode ocorrer depois de começar migrate, depois do HUP, durante conferência de workers ou nas rotas (`bin/deploy.sh:337`, `bin/deploy.sh:381`, `bin/deploy.sh:394`, `bin/deploy.sh:440`). Só no sucesso há unlock explícito depois das três provas (`bin/deploy.sh:437-442`). Assim, um deploy abortado pode liberar a exclusão sem reload conferido; consistência resultante do banco/código depende do ponto da falha e das migrations [nao verificado].

3. **A morte do processo não garante liberação imediata se houver descendente vivo.** A própria lib declara herança pelos filhos (`bin/trava_deploy_exclusao.sh:10-11`); o fd é aberto sem fechamento no comando de cron (`bin/trava_deploy_exclusao.sh:32`, `bin/cron_run.sh:24`). O arquivo de lock permanece; exclusão depende dos descritores, não da existência do arquivo (`bin/trava_deploy_exclusao.sh:14-16`, `bin/trava_deploy_exclusao.sh:32-42`). Descendente concreto de cron que retenha indefinidamente: [nao verificado].

4. **Aninhamentos encontrados:**
   - Reload agendado é RAW envelopado → deploy `--reload-agendado` (`app/config/crons.py:326-327`, `app/config/crons.py:1861-1863`). Esse modo não arma migration pendente: recusa em `bin/deploy.sh:211-214`; sem restart/apply não pede exclusiva (`bin/deploy.sh:316`). Portanto esse caminho evita autoespera.
   - Outro caminho existe: cron `vigia_arvore.sh` (`app/config/crons.py:296`) → `core/esteira_vigia.py curar` (`bin/vigia_arvore.sh:176`) → deploy `--sem-sombra` (`app/core/esteira_vigia.py:1267-1268`, `app/core/esteira_vigia.py:1275`). Se esse deploy armar apply, tenta exclusiva enquanto o envelope ancestral segura compartilhada: autoespera até 600 s e recusa (`bin/cron_run.sh:17-24`, `bin/deploy.sh:233`, `bin/deploy.sh:316-323`, `bin/trava_deploy_exclusao.sh:26`).
   - `vigia_logins.sh --auto` é cron e chama deploy, mas usa `--sem-migrate`, que recusa migrations pendentes e evita exclusiva (`app/config/crons.py:303`, `bin/vigia_logins.sh:87`, `bin/deploy.sh:215-218`, `bin/deploy.sh:316`).
   - Corrente não aninha envelopes por construção: o render acrescenta `; encadeado.sh` depois do envelope raiz, e cada elo faz exec do seu envelope (`app/config/crons.py:1884-1885`, `bin/encadeado.sh:102-104`).
   - `deploy_agendado` entra diretamente por cron.d e executa a esteira fornecida; não cria envelope próprio (`bin/deploy_agendado.sh:54-56`, `bin/deploy_agendado.sh:68-73`). Fatias realmente agendadas [nao verificado].
   - Cadeia toma árvore e régua antes de deploy; o deploy final pode adquirir exclusiva nessa ordem (`bin/molde_fatia/cadeia.sh:11-14`, `bin/molde_fatia/cadeia.sh:88-89`, `bin/deploy.sh:316-325`). `sombra.sh --conferir`, usado pelo deploy, não adquire a trava da sombra (`bin/deploy.sh:162`, `bin/sombra.sh:438`, `bin/sombra.sh:451`, `bin/sombra.sh:463`).

5. **Segundo plano:** não há `&`, `nohup` ou `setsid` nos comandos efetivamente declarados em CRONS; o gerador lança RAW/Django pelo envelope e a corrente por `;` (`app/config/crons.py:282-352`, `app/config/crons.py:594-595`, `app/config/crons.py:688-706`, `app/config/crons.py:1861-1885`). Os RAW inspecionados usam execução síncrona, incluindo `noturno.py` (`eval/noturno.py:32-44`, `eval/noturno.py:93-94`), fábrica (`bin/fabrica_estado.py:63-66`), alarme de sessão (`bin/alarme_sessao_ociosa.py:142-150`) e backup (`scripts/backup_all.sh:6-8`). Efeitos transitivos arbitrários dos comandos Django [nao verificado].

   Há lançamentos assíncronos vizinhos, sem caminho demonstrado a partir dessas entradas: fabricante usa `setsid nohup ... &` (`bin/fabricante.sh:187`); controlador/esteiras usam `Popen` (`app/core/esteira_vigia.py:801-802`, `app/core/esteira_vigia.py:917-918`). A esteira está declarada como timer fora do cron (`app/config/crons.py:764-767`). O observador de árvore fecha fds 6–9, mas não 4, e dura enquanto o pai existir (`bin/trava.sh:68-69`); cron → esse observador [nao verificado]. Os três jobs `Popen` descritos em `crons.py` pertencem a FORA_DE_PIPELINE, não a CRONS (`app/config/crons.py:769`, `app/config/crons.py:1369-1371`).

6. **Posse não tem teto de 600 s.** Esse teto limita a aquisição (`bin/trava_deploy_exclusao.sh:26`, `bin/trava_deploy_exclusao.sh:34`). Depois dela, o deploy inclui migrate sem timeout (`bin/deploy.sh:337`), consultas `docker top` sem timeout (`bin/deploy.sh:377-379`), até 270 s nominais de sleeps dos workers (`bin/deploy.sh:382-395`), três esperas de disponibilidade de até 180 s cada (`bin/deploy.sh:402-408`) e provas de rota sem timeout (`bin/deploy.sh:419-439`). Portanto um deploy com migration pode superar 300 s mesmo terminando com sucesso, levando cron ao rc 75; duração efetivamente observada em produção [nao verificado].

_3054 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
