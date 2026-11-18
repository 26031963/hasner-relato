# CODEX-ESTADO

> Ultima resposta processada: **18/11/2026 19:37** · publicado em **18/11/2026 19:37** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 18/11 19:37 · sessao 262b866904af · QUEIMA-CODEX-B · leitura · origem=Code

A linha anexada traz **duas perguntas de lei**, não um menu explícito de opções para cada uma. Ela também fixa três classes por decisões existentes e mantém o restante como `INDEFINIDA` até o corte [dado fornecido: `3.linha`, sem arquivo:linha verificável]. A identidade desta cópia com o hash informado é **[não verificado]**.

Não há resultado de produção para col214, col263, col87 ou col975 no material fornecido: se suas batidas saem pareadas ou sozinhas, e o enquadramento de cada caso, são **[não verificado]**.

**Pergunta 1 — batida depois da saída final.** O pareador já forma turnos, mas sua resposta depende da alimentação recebida; isso não estabelece uma distância geral após a saída final que, por si só, transforme uma avulsa em novo turno. No laço, uma `E` não absorvida inicia `cur`; uma `S` fecha o turno corrente ou forma saída órfã quando não há turno corrente ([turnos.py:1192](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1192), [turnos.py:1402](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1402), [turnos.py:1491](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1491)). Antes disso, papel da ata, intervalo e outras guardas podem absorver a batida; sem mapa de papel, o pareador recorre ao tipo gravado ([turnos.py:1304](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1304), [turnos.py:1377](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1377)).

Os números relacionados respondem a perguntas distintas:

- `TETO_PAUSA_SEGUNDOS = 3 * 3600`, ou **3 horas**, é o padrão de `intra_max_s`; `cont_max_s` tem padrão **14 horas** ([turnos.py:377](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:377), [turnos.py:1192](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1192)). O motor pode fornecer outros valores: pausa de **0 horas** no turno partido ou **8 horas** no plantão, e continuidade de **26 horas** no plantão ([motor_calculo_v2.py:503](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/motor_calculo_v2.py:503), [motor_calculo_v2.py:728](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/motor_calculo_v2.py:728), [motor_calculo_v2.py:2859](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/motor_calculo_v2.py:2859), [motor_calculo_v2.py:3511](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/motor_calculo_v2.py:3511)).
- O envelope de fechamento usa jornada prevista **+ 4 horas**, ou o envelope recebido quando faltam bordas válidas; no laço de fechamento, o envelope de referência é **26 horas**, sujeito às guardas descritas no código ([turnos.py:66](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:66), [turnos.py:1421](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1421)).
- `VESPERA_LONGE_DO_INICIO_MIN = 90`, em **minutos**, participa da atribuição da data de um turno noturno; `_data_do_turno` também examina se a saída cabe no envelope da véspera. É uma decisão sobre **a qual dia pertence o turno formado**, não uma distância geral entre saída final e avulsa ([turnos.py:55](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:55), [turnos.py:248](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:248)).

A L-115 responde a uma figura específica: **“essa batida e a SAIDA do turno pela posicao”**, quando há pausa fechada e última batida posterior; ela não fornece uma regra geral para a avulsa *depois* da saída final ([LEIS.md:153](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:153)). A L-085 determina que a jornada pertence ao dia do início, mas isso também não resolve a condição geral de formação de um novo turno ([LEIS.md:117](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:117)). Para a pergunta completa, **nenhuma linha identificada responde**; foram buscados `RAZAO-DA-SOBRA`, `turno novo`, `novo turno`, `sobra`, `órfã` e `orfa` em LEIS e CORTES. Equivalentes não encontrados nessa busca são **[não verificado]**.

**Pergunta 2 — pausa, jornada adicional ou cadastro.** Já existem decisões parciais, com domínios diferentes:

- `_marca_intra_duracao` deduz pausa por duração; sem bordas, exige `S→E` dentro de `intra_max_s` e uma `S` posterior dentro de `cont_max_s`. Com bordas, há guardas adicionais de distância e continuidade ([turnos.py:385](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:385), [turnos.py:421](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:421)). O próprio pareador também reconhece uma pausa deslocada com prova de continuidade ([turnos.py:1458](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1458)).
- `papel_por_minuto_da_ata` considera sobras **dentro do vão**: proximidade de luz acesa até `JANELA_CLUSTER_MIN`, par restante sob `intra_max_s`, ou perna restante próxima de luz de intervalo. Se as condições não fecham, o dia segue sem aquele mapa de papel ([turnos.py:1711](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1711), [turnos.py:1735](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1735), [turnos.py:1761](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1761)).
- `cadastro_realidade` apresenta, entre outras, as lavras C9 e C12; C12 mostra o dia publicado pelo motor como cadastro x realidade ([cadastro_realidade.py:100](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/cadastro_realidade.py:100), [cadastro_realidade.py:128](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/cadastro_realidade.py:128), [cadastro_realidade.py:360](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/cadastro_realidade.py:360)). `dono_da_divergencia` atribui `CADASTRO`, `BATIDA` ou `ESTRUTURA` por insumos e precedência próprios; não é um classificador geral entre pausa e jornada adicional ([e6_oraculo.py:53](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/e6_oraculo.py:53), [e6_oraculo.py:98](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/e6_oraculo.py:98)).
- A L-032 diz que **“batida de intervalo fora do horario exato mas dentro do envelope E o intervalo”**; a L-084 diz que entrada **e** saída a mais de três horas dos respectivos marcos levam o dia a cadastro x realidade ([LEIS.md:80](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:80), [LEIS.md:116](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:116)). São respostas para figuras específicas. Para a pergunta completa sobre sobra que já não se enquadrou nas constantes, **nenhuma linha identificada responde**; foram buscados também `jornada adicional`, `cadastro inadequado`, `mais batidas que marcos`, `pausa` e `intervalo` em LEIS e CORTES. Equivalentes não encontrados são **[não verificado]**.

O significado dos leitores **não é uniforme**. `JANELA_CLUSTER_MIN = 15` serve ao classificador para agrupar órfãs consecutivas; no mapa da ata, a mesma constante mede proximidade de uma luz acesa dentro do vão ([orfa_classe.py:26](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/orfa_classe.py:26), [orfa_classe.py:40](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/orfa_classe.py:40), [turnos.py:1729](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1729)). O classificador retorna classes por `HH:MM`, trata qualquer `tipo_dia` diferente de `trabalho` como `FOLGA_DESMENTIDA` e hoje usa `ECO_CLUSTER`/`GEOMETRIA`; isso não é a mesma resposta que o mapa `X`/`Xi` do pareador ([orfa_classe.py:53](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/orfa_classe.py:53), [turnos.py:1201](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1201)). O censo completo de leitores e equivalência semântica continua **[não verificado]**; o dado anexo traz uma matriz preliminar, não uma prova de completude.

A sonda abaixo **não foi executada**. Ela assume que `col214` e `col263` são PKs e que o ano é 2026: ambas as premissas são **[não verificado]**. A janela cobre cinco dias civis por caso, pela porta `batidas_apuraveis`; imprime o pareamento direto com parâmetros padrão e, separadamente, o caminho `turnos_do_colab`, que alimenta o pareador com contexto de ata e escala ([turnos.py:2868](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:2868), [turnos.py:1940](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1940), [turnos.py:1997](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1997)). Diferenças entre os dois resultados precisam ser interpretadas com essa alimentação; nenhum deles é resultado conhecido antes da execução.

```python
# Para manage.py shell no envelope de sombra somente leitura.
# NÃO executado. Confirmar ano e que colN corresponde a colaborador_id.
import copy
from datetime import date, datetime, time, timedelta

from django.utils import timezone
from ponto.turnos import batidas_apuraveis, parear_turnos, turnos_do_colab

CASOS = (
    (214, date(2026, 10, 29)),
    (263, date(2026, 10, 31)),
)

def local(b):
    if b is None:
        return None
    return {
        "instante": timezone.localtime(b.timestamp).isoformat(),
        "tipo": getattr(b, "tipo", None),
    }

def mostrar_turno(t):
    return {
        "data_turno": str(t.data_turno) if t.data_turno else None,
        "entrada": local(t.entrada),
        "saida": local(t.saida),
        "aberto": t.aberto,
        "cross_meianoite": t.cross_meianoite,
        "batidas": [local(b) for b in t.batidas],
    }

for col_id, dia in CASOS:
    # [dia - 2 dias, dia + 3 dias): cinco dias civis completos.
    inicio = timezone.make_aware(
        datetime.combine(dia - timedelta(days=2), time.min)
    )
    limite = timezone.make_aware(
        datetime.combine(dia + timedelta(days=3), time.min)
    )
    batidas = list(
        batidas_apuraveis(col_id, inicio, limite)
        .filter(timestamp__lt=limite)
    )

    print("\nCASO", f"col{col_id}", dia.isoformat())
    print("JANELA", inicio.isoformat(), limite.isoformat(), "(fim exclusivo)")
    print("BATIDAS_APURAVEIS", [local(b) for b in batidas])

    # Cópias: o pareador carimba atributos transitórios nas instâncias.
    diretos = parear_turnos([copy.copy(b) for b in batidas])
    print("PAREADOR_DIRETO_PADROES",
          [mostrar_turno(t) for t in diretos])

    # Caminho da aplicação para o dia, com sua própria carga e pad.
    contextualizados = turnos_do_colab(col_id, dia, dia)
    print("TURNOS_DO_COLAB_COM_CONTEXTO",
          [mostrar_turno(t) for t in contextualizados])
```

| Opção ou classe da linha | Sítios a considerar | Leitores e significado | Lei existente |
|---|---|---|---|
| `FOLGA_DESMENTIDA` | [orfa_classe.py:62](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/orfa_classe.py:62) | Classificador usa `tipo_dia != trabalho`; equivalência para `indefinido` e todos os leitores **[não verificado]** | Lei específica para a classe **[não verificado]** |
| `ECO_PROXIMO_DE_LUZ` | [orfa_classe.py:28](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/orfa_classe.py:28), [turnos.py:1742](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1742) | Rajada de órfãs e proximidade de luz são significados distintos | Lei específica para estender a classe **[não verificado]** |
| `PAR_DE_PAUSA_DEDUZIDO` / perna | [turnos.py:385](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:385), [turnos.py:1761](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1761) | Pareador e mapa da ata aplicam guardas diferentes; motor fornece teto variável ([motor_calculo_v2.py:503](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/motor_calculo_v2.py:503)) | [L-032, parcial para intervalo cadastrado](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:80) |
| Após saída final: pergunta 1 | [turnos.py:1402](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1402), [turnos.py:1491](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1491) | Pareador direto e caminho alimentado por ata/escala podem receber contextos diferentes ([turnos.py:1940](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1940)) | Nenhuma linha identificada responde à condição geral; [L-115](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:153) e [L-085](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:117) são parciais |
| Excedente sem enquadramento: pergunta 2 | [turnos.py:1761](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1761), [e6_oraculo.py:98](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/e6_oraculo.py:98) | Dedução de pausa e dono da divergência respondem perguntas diferentes; demais leitores **[não verificado]** | Nenhuma linha identificada resolve a separação geral; [L-032](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:80) e [L-084](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:116) são parciais |

## 18/11 19:37 · sessao e05d25598695 · QUEIMA-CODEX-B · leitura · origem=Code

**As duas perguntas de lei permanecem abertas no material conferido:** quem fecha o chamado órfão quando não nasce disputa; e o que fazer quando chega motivo novo cuja dona está fechada. O registro da árvore mantém as duas questões na F1 e registra a decisão “não nasce” para o primeiro caso. ([BACKLOG.md:2115](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2115))

Este é um **pré-desenho parcial**: os sítios e leitores abaixo foram conferidos, mas não completei o censo de *todos* os leitores e caminhos indiretos. Logo, a exaustividade pedida para cada opção é **[nao verificado]**. A identidade Git da cópia com o hash informado também é **[nao verificado]**.

1. **Órfão sem disputa — quem fecha?**

O órfão tratado pelo agrupador é um **chamado**, inclusive um chamado criado diretamente sem disputa; não é necessariamente uma `PerguntaDisputa` sem dona. A FK de `PerguntaDisputa` para disputa não é declarada anulável. ([disputa_emissao.py:804](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:804), [models.py:2603](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:2603))

- **O que muda:** o ponto de decisão existente filtra chamados regularizáveis, escolhe uma disputa aberta do colaborador e, se não houver, cria uma antes do laço de perguntas. Ao fim, tenta fechá-la pelo C7. A decisão registrada de não nascer exige tratar esse caminho sem contar com a disputa recém-criada; o responsável pelo fechamento do chamado continua sendo a pergunta, não uma escolha demonstrada pelo código. ([disputa_emissao.py:1100](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:1100), [disputa_emissao.py:1110](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:1110), [disputa_emissao.py:1137](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:1137), [disputa_emissao.py:1292](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:1292))
- **Leitores e portas:** o agrupador é chamado pela ação administrativa, pelo app, pela cobrança do dia e por comandos de detecção; o reconciliador lê `chamados_orfaos_ids` e os estados dos filhos; existe uma porta humana de fechamento do chamado. O C7, porém, responde se **a disputa** pode fechar, a partir de suas perguntas, e não identifica por si quem deve fechar um chamado sem disputa. Portanto, esses leitores não respondem todos à mesma pergunta. ([acoes_disputa.py:226](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/acoes_disputa.py:226), [pergunta_no_app.py:261](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/pergunta_no_app.py:261), [cobrar_dia.py:49](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/cobrar_dia.py:49), [processar_alertas_turno.py:385](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/processar_alertas_turno.py:385), [detectar_ausencias.py:803](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/detectar_ausencias.py:803), [processar_alertas_avancados.py:159](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/processar_alertas_avancados.py:159), [reconciliador.py:239](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/reconciliador.py:239), [models.py:464](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:464), [models.py:2009](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:2009))
- **Lei existente:** nas buscas em `LEIS.md` e `CORTES.md` por `órf`, `orf`, `disputa`, `raiz`, `fechar`, `dona` e combinações relacionadas, **nenhuma linha encontrada responde especificamente quem fecha esse órfão**. A exaustividade semântica da busca é **[nao verificado]**.

2. **Motivo novo com dona fechada — três opções**

O núcleo hoje procura uma disputa **aberta** por colaborador, agrega nela os motivos ou cria outra quando não encontra aberta; depois gera perguntas. Isso descreve o caminho conferido, mas não prova que ele implemente a decisão de lei para toda chegada de motivo com dona fechada. ([disputa_emissao.py:540](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:540), [disputa_emissao.py:601](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:601), [disputa_emissao.py:676](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:676))

- **Reabrir a dona.** O seletor e a decisão de emissão precisariam considerar a dona fechada. Já existem duas portas distintas: a administrativa exige usuário, limita reaberturas e pode acrescentar motivo; a de sistema não consome esse limite. Ambas limpam os campos de fechamento, com consequências para seus leitores. Qual porta, condições e demais sítios necessários: **[nao verificado]**. ([models.py:2289](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:2289), [models.py:2326](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:2326))
- **Nascer outra.** O núcleo e o agrupador já têm caminhos que criam disputa quando não acham aberta; é necessário confrontar a eleição da dona e o reuso de perguntas existentes, pois `_nascer` pode devolver uma pergunta existente e seu construtor de pergunta nova recebe a disputa. Quais alterações seriam suficientes para esta opção: **[nao verificado]**. ([disputa_emissao.py:601](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:601), [disputa_emissao.py:1137](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:1137), [models.py:3198](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:3198), [models.py:3206](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:3206))
- **Pauta DP.** Seria necessário decidir onde detectar esse caso e encaminhá-lo à porta de pauta, com identidade e conteúdo do fato **[nao verificado]**. Há porta de escrita de pauta e há encaminhamento para DP em outro caso específico — limbo em competência entregue —, mas esse caso não demonstra que motivo novo de dona fechada já seja encaminhado. ([pautas/services.py:152](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/pautas/services.py:152), [fabrica_da_celula.py:210](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/fabrica_da_celula.py:210), [fabrica_da_celula.py:268](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/fabrica_da_celula.py:268))

**Leitores comuns às três opções:** `fechada_em` significa aberta quando nulo; o app usa esse estado para separar pendentes de histórico; o fio do admin inclui perguntas vivas de disputas fechadas; o vigia conta essas perguntas; e a regularização bloqueia por existência de disputa aberta do colaborador. Todos leem o mesmo estado de abertura, mas **não o usam para a mesma finalidade nem selecionam o mesmo universo**. A comparação de todos os demais leitores é **[nao verificado]**. ([models.py:1959](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:1959), [pergunta_no_app.py:80](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/pergunta_no_app.py:80), [fio.py:245](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/fio.py:245), [vigia_fio.py:700](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/vigia_fio.py:700), [regularizacao_ext.py:57](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/regularizacao_ext.py:57))

**Lei existente:** o corte registra literalmente a **“pergunta de lei N5 (motivo novo chegando em disputa ja fechada)”**, sem escolher entre as três opções. Outro corte diz **“NAO reabrir em lote”** no tratamento das perguntas vivas já existentes em disputas fechadas; ele não responde expressamente à chegada de motivo novo. Nas buscas por `dona`, `fechada`, `motivo novo`, `reabrir`, `disputa`, `raiz` e `pauta` em `LEIS.md` e `CORTES.md`, nenhuma linha encontrada escolhe especificamente entre as três; exaustividade semântica **[nao verificado]**. ([CORTES.md:515](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:515), [CORTES.md:415](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:415))

| Opção | Sítios conferidos | Leitores conferidos | Lei existente para a pergunta |
|---|---|---|---|
| Não nasce disputa; quem fecha o órfão? | [agrupador:1100](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:1100), [criação:1137](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:1137) | [ação:226](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/acoes_disputa.py:226), [app:261](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/pergunta_no_app.py:261), [reconciliador:239](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/reconciliador.py:239) | Nenhuma linha encontrada sobre **quem fecha**; busca exaustiva **[nao verificado]** |
| Reabrir dona | [reabertura admin:2289](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:2289), [sistema:2326](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:2326) | App, fio, vigia e regularização, acima; censo completo **[nao verificado]** | Nenhuma escolha específica encontrada; [corte de vivas:415](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:415) trata de outro recorte |
| Nascer outra | [núcleo:601](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:601), [agrupador:1137](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:1137), [pergunta:3206](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:3206) | Os mesmos leitores de estado e dono, acima; censo completo **[nao verificado]** | Nenhuma escolha específica encontrada; [N5:515](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:515) registra a pergunta |
| Pauta DP | [porta:152](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/pautas/services.py:152); detector para esta opção **[nao verificado]** | Leitores de pauta e efeitos sobre emissão: censo **[nao verificado]** | Nenhuma escolha específica encontrada; [N5:515](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:515) registra a pergunta |

## 18/11 19:37 · sessao e392c37855b2 · QUEIMA-CODEX-B · leitura · origem=Code

A pergunta tem duas decisões independentes: **(i)** qual universo de motivos as três portas consultariam; **(ii)** qual comportamento teriam quando falta o dia do fato. Na cópia, a composição atual consulta apenas metades de intervalo e distingue ausência de dia de competência entregue; a testemunha atual também tem alcance mais estreito que o das três portas. ([materializacao.py:804](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:804), [materializacao.py:811](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:811), [materializacao.py:884](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:884))

O texto do aval, da revisão e da linha de pergunta foi recebido raspado no enunciado; sua localização e suas linhas originais são **[nao verificado]**. As referências abaixo são da árvore desta cópia.

### (i) Composição como está ou K6 para todo motivo

**Composição como está.** As três portas precisariam passar a consultá-la antes de suas escritas: `recusar_pergunta` antes de gravar o motivo e reabrir; `conferir_data_do_fato` antes dos carimbos; e `_aceitar_a_folga` antes dos carimbos e da declaração, inclusive no ramo em que o dia já é folga. O retorno antecipado para folga ocorre antes da consulta comum de validar. ([validacao.py:916](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:916), [validacao.py:996](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:996), [validacao.py:511](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:511), [validacao.py:551](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:551), [validacao.py:711](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:711))

Com o filtro atual, a consulta ignora qualquer pergunta cujo `motivo` não seja `intervalo_saida` ou `intervalo_volta`. Assim, a mera chamada não barra os demais motivos por entrega nem por ausência de dia. A identificação da folga é pela **resposta**, não pelo motivo; o efeito concreto depende do motivo da pergunta. ([materializacao.py:136](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:136), [materializacao.py:804](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:804), [validacao.py:465](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:465))

**K6 do dia para todo motivo.** Além dos três pontos de porta, seria necessária uma consulta que alcance os outros motivos, usando a competência do `data_evento` e o K6 existente. O sítio exato da mudança compartilhada ou de uma composição específica das portas **[nao verificado: depende do desenho escolhido]**. `competencia_do_fato` já combina a janela do dia com `competencia_entregue` e devolve `None` sem data; ela própria não filtra motivo. ([materializacao.py:788](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:788))

**Quem mais lê e significado.**

- `entrega_que_barra_a_validacao` é lida por validar antes da escrita e novamente após a materialização, e pela testemunha de aplicação. Alargar essa função compartilhada pode alterar também esses leitores; manter uma consulta específica às três portas teria alcance diferente. ([validacao.py:740](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:740), [validacao.py:777](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:777), [materializacao.py:901](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:901))
- `_k6_que_barra` também é lida por `_competencia_que_barra`, que a combina com a guarda F6; essa composição é usada na previsão e na materialização. Hoje sua documentação diz expressamente “metades de intervalo”: alargar o predicado compartilhado mudaria o universo esperado desses caminhos. ([materializacao.py:831](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:831), [materializacao.py:925](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:925), [materializacao.py:1867](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:1867))
- `competencia_do_fato` também alimenta a devolução de paridade, a triagem L-130 e o comando da pauta. Todos recebem a mesma forma `None` ou `(mês, ano, entregue)`, mas a usam para finalidades diferentes; não se pode concluir que uma ampliação de guarda deva alterar esses universos. ([validacao.py:338](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:338), [validacao.py:400](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:400), [bug_par_103.py:59](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/management/commands/bug_par_103.py:59))
- A testemunha é lida pelo card, pelo modal e pelo contador da pauta. Ela só chega ao derivador para resposta de tipo hora, respondida e ainda não materializada; portanto, não representa hoje todas as situações das três portas. Rejeitar no card e no modal e Conferir data no modal têm condições próprias, sem a consulta ao bloqueio nesses pontos. ([chamados_tags.py:131](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/templatetags/chamados_tags.py:131), [fio.py:376](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/fio.py:376), [materializacao.py:342](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:342), [materializacao.py:884](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:884), [_pergunta_card.html:117](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/templates/chamados/partials/_pergunta_card.html:117), [modal_fio.html:163](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/templates/chamados/partials/modal_fio.html:163), [modal_fio.html:316](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/templates/chamados/partials/modal_fio.html:316))
- A casca de recusar classifica estados não enumerados como `sucesso`; um novo retorno de bloqueio exige examinar essa tradução. A casca de conferir lê `ok` e `msg`. Portanto, os leitores das duas portas não supõem o mesmo contrato de apresentação. ([acoes_disputa.py:81](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/acoes_disputa.py:81), [views.py:1313](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/views.py:1313))

**Lei ou corte existente.** Para as três portas e todos os motivos, **nenhuma linha encontrada responde à escolha inteira**. O corte das 163 responde explicitamente para a unidade “metade de intervalo” e diz: “competencia do fato entregue = BLOQUEADA_COMPETENCIA_ENTREGUE (L1), nada gravado”; o registro posterior mantém as seis portas humanas sem obra até corte. A L-113 altera a L-092 quanto ao recálculo em exportada, mas seu texto não escolhe entre estas duas implementações das três portas. Buscas: `portas humanas`, `recusar_pergunta`, `conferir_data_do_fato`, `aceitar_a_folga`, `metade de intervalo`, `U163`, `entregue` e `exportada`. ([CORTES.md:530](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:530), [CORTES.md:556](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:556), [LEIS.md:155](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:155))

### (ii) Sem dia do fato: recusa ou não?

**Recusa.** Nas três portas, a ausência de `data_evento` teria de ser tratada antes das escritas apontadas acima. No universo atual da composição, ela já devolve `BLOQUEADA_SEM_DIA_DO_FATO`, pois não consulta K6 sem data; `_recusa_da_entrega` preserva esse estado distinto. Para folga, já existe uma recusa própria `folga_sem_dia` antes das escritas: a escolha também afeta qual estado e mensagem prevalecem. ([materializacao.py:821](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:821), [validacao.py:228](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:228), [validacao.py:499](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:499))

**Não recusa por essa razão.** A nova guarda das portas teria de tratar separadamente o retorno sem dia, em vez de repassar todo resultado não vazio da composição. As demais regras de cada porta continuariam relevantes; em particular, o aceite de folga já recusa sem dia independentemente da nova guarda. A ordem exata entre essa regra e a nova consulta **[nao verificado: depende do desenho escolhido]**. ([validacao.py:499](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:499), [validacao.py:740](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:740))

**Quem mais lê e significado.** Validar e a testemunha usam os dois estados para distinguir “entregue” de “não foi possível consultar por falta de dia”; card e modal escondem Validar para ambos. O contador coloca o segundo no balde sem dia, enquanto o comando também lista perguntas sem dia para a pauta. Assim, chamar os dois casos de `bloqueada_competencia_entregue` mudaria o significado entendido por esses leitores. Por outro lado, se só algumas portas deixarem passar sem dia, a testemunha atual de validar não pode, sozinha, significar que **todas** as portas recusam. ([validacao.py:228](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:228), [_pergunta_card.html:47](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/templates/chamados/partials/_pergunta_card.html:47), [modal_fio.html:274](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/templates/chamados/partials/modal_fio.html:274), [materializacao.py:343](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:343), [bug_par_103.py:73](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/management/commands/bug_par_103.py:73))

**Lei ou corte existente.** Para as metades de intervalo das 163, há resposta literal: “sem dia do fato = BLOQUEADA_SEM_DIA_DO_FATO (L5)”. **Nenhuma linha encontrada responde se recusar e conferir data devem recusar sem dia para todo motivo**, nem resolve a precedência com `folga_sem_dia`. Buscas: `SEM.DIA.DO.FATO`, `sem dia do fato`, `portas humanas`, nomes das três portas e `U163`. ([CORTES.md:530](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:530), [CORTES.md:556](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:556))

| Opção | Sítios a considerar | Leitores e significado | Lei ou corte existente |
|---|---|---|---|
| (i) Composição como está | Três portas: [validacao.py:858](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:858), [validacao.py:957](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:957), [validacao.py:470](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:470); telas citadas acima | Guarda atual só consulta intervalo; testemunha tem alcance próprio. ([materializacao.py:804](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:804), [materializacao.py:884](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:884)) | Nenhuma linha encontrada escolhe para as três portas. ([CORTES.md:556](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:556)) |
| (i) K6 para todo motivo | Mesmas portas e telas; composição a definir **[nao verificado]**. ([materializacao.py:788](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:788)) | Alterar função compartilhada pode atingir validar, materialização e pauta, cujos universos não são idênticos. ([validacao.py:740](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:740), [materializacao.py:831](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:831)) | Nenhuma linha encontrada determina esse alcance para as três portas; corte localizado nas metades. ([CORTES.md:530](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:530)) |
| (ii) Recusa sem dia | Três portas, retorno e testemunha por botão; folga já tem recusa própria. ([validacao.py:499](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:499)) | Estado sem dia já é distinto e tem leitores de tela e contador. ([validacao.py:228](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:228), [materializacao.py:343](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:343)) | L5 responde para metades das 163; extensão às três portas: nenhuma linha encontrada. ([CORTES.md:530](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:530)) |
| (ii) Não recusa por falta de dia | Tratamento separado do resultado nas portas; precedência da folga **[nao verificado]**. ([materializacao.py:821](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:821), [validacao.py:499](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:499)) | A testemunha de validar não equivaleria automaticamente à decisão das outras portas. ([materializacao.py:892](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:892)) | Nenhuma linha encontrada responde para as três portas. ([CORTES.md:556](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:556)) |

## 18/11 19:36 · sessao 3012f86bda0b · QUEIMA-CODEX-B · leitura · origem=Code

A atribuição da migração de `pergunta_respondida` e de sua face SQL a uma fatia **permanece em aberto**: a E4c está descrita como fase de migração de leitores, a F1 estabelece o juiz e seus leitores, a F2 entrega uma leitura por ato para determinados usos, e a E4d exige que juízes registrados leiam o ato; as linhas consultadas não atribuem expressamente as duas funções a uma fatia específica. [app/docs/BACKLOG.md:2093; app/docs/BACKLOG.md:824; app/chamados/models.py:4295; app/docs/RELATO.md:10; não verificado]

A árvore disponível contém a pergunta em [RELATO.md:10](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/RELATO.md:10). Os arquivos `logs/` citados nela não estão presentes nesta cópia; os literais dos avais e da revisão abaixo são os fornecidos no enunciado ou reproduzidos pelo RELATO. A identidade Git da cópia como o hash informado não pôde ser confirmada: a árvore não contém um repositório Git acessível. [não verificado]

**(i) O item 3 também retifica o selo 2 da E4c?**

- O ponto a mudar é o **alcance da regra e do selo**, não um sítio de código determinado pela pergunta: o RELATO registra a colisão entre “substituir SO o E4d” / “NAO muda: E4c” e a troca do selo 2. [RELATO.md:10](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/RELATO.md:10)
- O BACKLOG ainda descreve a sequência anterior como “E4c LEITORES DIVERGE + selo 2 -> E4d parar projecao”; a linha da nova E4d registra o aval de projetor único. São dois registros cujo enquadramento depende da resposta. [BACKLOG.md:2093](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2093); [BACKLOG.md:2017](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2017)
- O selo antigo e os novos selos, como implementações ou consumidores concretos, não foram localizados; seus leitores e todos os sítios eventualmente afetados são **[não verificado]**. O juiz C6 está registrado e lê a projeção hoje, mas isso não demonstra qual selo futuro o abrangerá. [core/juizes.py:291](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/core/juizes.py:291); [chamados/juizes.py:1389](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:1389)
- **Lei existente:** nenhuma linha encontrada responde expressamente se há retificação do selo 2. Busquei em `LEIS.md` e `CORTES.md` por `E4c`, `E4d`, `selo 2`, `E4-EXPAND-CONTRACT`, `projecao` e `RESPOSTAS-1811-1800`. A regra geral de reconstrução de projeções da L-121 não resolve essa retificação. [LEIS.md:142](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:142)

**(ii) Juiz e linha de exceção ficam fora do selo (b) e do contador até o fato próprio?**

- O possível alcance é duplo: **quais leituras de juiz entram no selo** e **quais linhas entram na comparação de reconstrução**. A pergunta não identifica implementação existente do selo ou contador; seus sítios futuros são **[não verificado]**. [RELATO.md:10](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/RELATO.md:10)
- O exemplo verificável é `pergunta_viva`: a função temporal lê `validada_em`, `respondida_em`, `resposta_colab`, `materializada_em` e `via_resolucao`, corta valores futuros quando recebe `agora`, e alimenta o cálculo do C6. [chamados/juizes.py:1382](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:1382); [chamados/juizes.py:1403](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:1403)
- Outros leitores não presumem todos o mesmo significado: `cadastro_realidade` usa o instante temporal de resposta para um ramo M1, sem testar texto nesse ponto; o juiz de resposta exige instante **e** texto não vazio. `q_vivas` é uma seleção SQL deliberadamente mais ampla que o julgamento exato de vitalidade. [cadastro_realidade.py:500](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/cadastro_realidade.py:500); [chamados/juizes.py:1544](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:1544); [models.py:3580](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:3580)
- As seis exceções registradas são A03 reabertura administrativa, A05 assinatura, A06 duplicata/identidade, A17 pendência técnica de materialização, A18 recusa técnica de materialização e A20 conferência de data; cada linha mantém a projeção como fonte até o fato próprio. A correspondência exata de cada exceção com cada campo, juiz e linha do contador é **[não verificado]**. [BACKLOG.md:2097](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2097); [BACKLOG.md:2098](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2098); [BACKLOG.md:2099](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2099); [BACKLOG.md:2100](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2100); [BACKLOG.md:2101](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2101); [BACKLOG.md:2102](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2102)
- **Lei existente:** nenhuma linha encontrada responde expressamente à exclusão do juiz do selo **e** da linha do contador. Busquei `excecao`, `fato proprio`, `E4c`, `E4d`, `selo 2`, `projecao` e `reconstru` em `LEIS.md` e `CORTES.md`. A L-121 trata de reconstrução no pouso de cura de juiz, sem definir esse universo de exceções. [LEIS.md:142](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:142)

**Fatia que move as duas faces para o ato**

As candidatas e o que já dizem são:

| Candidata | Registro existente |
|---|---|
| **E4c, em geral** | É a fase de leitores divergentes e selo 2; a linha não nomeia a fatia dessas duas funções. [BACKLOG.md:2093](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2093) |
| **F1 nova / B2** | Institui um juiz para instante e texto e manda leitores B, C e D e a property delegarem; B2b tem 39 sítios pendentes segundo o registro. Isso não atribui expressamente a troca interna das duas faces para o ato. [BACKLOG.md:2021](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2021); [BACKLOG.md:824](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:824) |
| **F2** | `DecisaoPergunta.leitura` já deriva resposta e validação vigentes da sequência e inclui passivo da projeção sem linha; ela própria declara os leitores atendidos e uma face SQL **da validação**. Uma face SQL equivalente para resposta não está identificada nesses sítios. [models.py:4295](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:4295); [models.py:3539](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:3539) |
| **F9** | O dado fornecido diz que o brief não toca juiz **[não verificado: brief ausente]**. O RELATO registra uma decisão específica de manter o seletor de limbo na projeção, não uma atribuição da migração do juiz. [RELATO.md:89](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/RELATO.md:89) |
| **E4d, projetor único** | O item 3 fornecido exige juiz registrado lendo ato **[não verificado: arquivo do aval ausente]**; o BACKLOG situa a abertura da E4d depois de F6F7, F9, F5, F3 e B2b, sem atribuir esta migração a um projetor ou pouso concreto. [BACKLOG.md:2017](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2017) |

O sítio Python do juiz avalia os atributos do objeto **que recebe**; portanto, a fonte depende do chamador: a property `respondida` passa `DecisaoPergunta.leitura(self)`, enquanto o C6 passa uma tupla construída com a projeção temporal. A face SQL, por sua vez, consulta diretamente as colunas da pergunta. [chamados/juizes.py:1552](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:1552); [models.py:3791](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:3791); [chamados/juizes.py:1423](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:1423); [chamados/juizes.py:1580](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:1580)

Entre os demais leitores estão `q_vivas`, a fila de respostas a validar, a escolha da pergunta canônica e a prévia de folha; usam o predicado para decisões distintas, embora compartilhem a definição de resposta. A face negativa `q_nao_respondida` também depende do par projetado, de modo que sua equivalência precisa entrar na análise de uma eventual mudança da face positiva. O censo completo de leitores, inclusive indiretos e testes, é **[não verificado]**. [models.py:3597](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:3597); [chamados/juizes.py:1761](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:1761); [fio.py:1274](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/fio.py:1274); [previa.py:142](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/folha/previa.py:142); [chamados/juizes.py:1583](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:1583)

A lei já fixa o **significado** de respondida: “tem instante (respondida_em) E texto nao vazio” e “Um juiz so para todas as finalidades”. O corte identifica também a face SQL. Nenhuma dessas linhas atribui a migração das duas funções a uma fatia; busquei `L-130`, `JUIZ-RESPONDIDA`, `E4c`, `E4d`, `projecao` e `reconstru` em `LEIS.md` e `CORTES.md`. [LEIS.md:151](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:151); [CORTES.md:518](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/CORTES.md:518)

| Opção / pergunta | Sítios | Leitores | Lei existente |
|---|---|---|---|
| **(i) Retificação da E4c** | Registros de E4c e E4d: [BACKLOG:2093](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2093), [2017](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2017); implementação do selo **[não verificado]** | Consumidores do selo **[não verificado]** | Nenhuma linha encontrada responde à retificação; L-121 é geral. [LEIS:142](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:142) |
| **(ii) Exceções no selo e contador** | C6 e projeção temporal: [juizes.py:1382](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:1382); selo/contador futuros **[não verificado]** | C6, M1 e SQL de vivas fazem perguntas distintas. [juizes.py:1403](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:1403); [cadastro_realidade.py:502](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/cadastro_realidade.py:502); [models.py:3569](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/models.py:3569) | Nenhuma linha encontrada resolve ambas as exclusões; exceções registradas no [BACKLOG:2097](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/BACKLOG.md:2097). |
| **Fatia para juiz e SQL** | [juizes.py:1544](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:1544), [1575](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:1575), [1583](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:1583) | Exemplos acima; censo completo **[não verificado]** | L-130 define significado e juiz único, sem nomear fatia. [LEIS:151](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/docs/LEIS.md:151) |

## 18/11 19:36 · sessao 49a9cd60665e · E4C-F6F7 · verificador · origem=Code

**Não aprovo a obra com base nesta verificação: o censo completo de escritores e leitores e a avaliação dos 15 achados não foram concluídos.** Há divergências estáticas que merecem triagem, mas seu alcance e, em alguns casos, a regra aplicável permanecem **[não verificado]**.

A cópia acessível não contém metadados Git: não foi possível confirmar o hash nem calcular o merge-base [não verificado]. O teste `test_e4c_f6f7_c13_caso_pela_regra.py` não foi encontrado na árvore acessível; seu corpo foi fornecido no anexo, mas sua presença na cópia e sua execução ficam [não verificado]. Não executei testes nem mutantes; os resultados abaixo são de inspeção estática. [nao verificado]

### Fecho por achado dos postmortens

A coluna “situação” trata da **classe no sítio desta obra**, não declara que o caso do postmortem se repete. “Não concluído” não equivale a “não ocorre”.

| Achado | Classe | Sítio nesta obra e prova | Situação |
|---|---|---|---|
| F6F7-1: significado entre leitores | (a) | A tarefa é projetada para a folha em [fora_da_folha.py:70](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/folha/fora_da_folha.py:70); a folha também agrupa por pessoa em [fora_da_folha.py:94](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/folha/fora_da_folha.py:94). O teste c13 anexado compara a projeção e `_fio_da_porta`, mas não demonstra efeito em grupos ou frota [não verificado]. | **BLOQUEIA a conclusão do censo** |
| F6F7-2: função sem chamador | (a) | O contador é chamado pelo vigia em [vigiar_fio_morto.py:192](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/management/commands/vigiar_fio_morto.py:192) e seu número entra no carimbo em [vigiar_fio_morto.py:262](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/management/commands/vigiar_fio_morto.py:262). | **Não ocorre nesse contador** |
| F6F7-3: sequência interrompida | (a) | O laço só recua por respostas contíguas que mantêm validação em [tarefa_do_dia.py:163](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/tarefa_do_dia.py:163); a leitura do ato conserva validação através de `SILENCIO` em [models.py:4272](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:4272). A prioridade exigida para a tarefa nessa sequência é [não verificado]. | **FILA**, divergência sintética a decidir |
| F6F7-4: mutação indireta no selo | (a) | O selo examina escrita e chamadas cuja raiz é o nome `perguntas` em [test_contract_pergunta_viva.py:46](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/tests/test_contract_pergunta_viva.py:46). O teste enumera vários mutantes diretos em [test_e4c_f6f7_c7_sem_banco.py:88](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:88). Alcance por alias ou chamada indireta e os quatro mutantes anteriores: [não verificado]. | **FILA**, selo não demonstrado exaustivo |
| F6F7-5: omissão de pergunta | (e) | O postmortem fornecido não identifica pergunta nem resposta para este item [não verificado]. Os pontos de leitura por fora indicados abaixo são candidatos desta classe, não prova de que sejam o mesmo caso [não verificado]. | **BLOQUEIA a conclusão da verificação** |
| F6F7-6: evidência de teste/fixture | (c) | O teste de endpoint do passivo só verifica `True` em [test_f6_u4_aguarda_admin_pela_tarefa.py:67](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/api/tests/test_f6_u4_aguarda_admin_pela_tarefa.py:67); o teste unitário verifica também `False` em [test_f6_u4_aguarda_admin_pela_tarefa.py:80](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/api/tests/test_f6_u4_aguarda_admin_pela_tarefa.py:80). `ERROR` da fixture e execução atual: [não verificado]. | **FILA**, sem resultado de execução |
| Escala-1: mutante sobrevive ao RED | (a) | Um mutante `fim_medido = None` passaria pelo teste que só confere contador e universo em [test_residuo_alem_da_competencia.py:114](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/ponto/tests/test_residuo_alem_da_competencia.py:114), por inspeção; outro teste confere o máximo em [test_cadastro_futuro_sem_banco.py:800](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/ponto/tests/test_cadastro_futuro_sem_banco.py:800). Sobrevivência à suíte: [não verificado]. | **FILA**, teste isolado sem essa mordida |
| Escala-2: rótulo LEI-AKITA | (a) | A linha exige `juizes novos` em [CLAUDE.md:63](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/CLAUDE.md:63). Sem histórico Git, a linha de cada commit da obra e sua correspondência com o diff são [não verificado]. | **BLOQUEIA a conclusão do selo** |
| Escala-3: segundo juiz herdado | (a) | A face SQL de validação reexpressa a regra Python em [models.py:3551](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:3551) e [models.py:4292](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:4292). Equivalência para todo estado e eventual caracterização como juiz duplicado: [não verificado]. | **FILA**, equivalência integral não demonstrada |
| Escala-4: teste não chega à porta | (a) | O teste unitário de `aguarda_admin_pela_tarefa` chama diretamente a função em [test_f6_u4_aguarda_admin_pela_tarefa.py:76](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/api/tests/test_f6_u4_aguarda_admin_pela_tarefa.py:76); há testes de endpoint separados em [test_f6_u4_aguarda_admin_pela_tarefa.py:52](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/api/tests/test_f6_u4_aguarda_admin_pela_tarefa.py:52). Cobertura de todos os ramos até a porta: [não verificado]. | **FILA** |
| Escala-5: teste não confere um campo | (a) | O teste do contador confere o objeto de resultado em [test_f7_contador_validada_sem_linha.py:38](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/tests/test_f7_contador_validada_sem_linha.py:38); o censo de todos os campos de trilha alterados e respectivos asserts é [não verificado]. | **FILA** |
| Escala-6: lavra vencida | Sem classe compatível | A carga usa `esta_stale` para aceitar a lavra em [tarefa_do_dia.py:416](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/tarefa_do_dia.py:416). Correção para todas as causas de vencimento e teste correspondente: [não verificado]. | **FILA** |
| Escala-7: literal do aval | (e) | O alcance futuro usa `data__gt=hoje`, sem teto, em [cadastro_futuro.py:21](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/ponto/services/cadastro_futuro.py:21). A conferência integral contra os avais referidos no diff é [não verificado]. | **BLOQUEIA a conclusão da revisão do aval** |
| Escala-8a: rótulo do selo | (a) | O selo AST e seus mutantes estão em [test_contract_pergunta_viva.py:46](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/tests/test_contract_pergunta_viva.py:46) e [test_e4c_f6f7_c7_sem_banco.py:88](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:88). Rótulo e alcance de **todos** os selos da obra: [não verificado]. | **FILA** |
| Escala-8b: citação de lei | (e) | Correspondência completa das citações de lei com o diff e com os selos: [não verificado]. | **BLOQUEIA a conclusão da revisão documental** |

As classificações “BLOQUEIA a conclusão” indicam **verificação incompleta**, não uma demonstração independente de defeito funcional.

### Escritores, leitores e significado: recorte verificado

Este é **um censo parcial**, não a resposta solicitada para todo campo, chave e função do diff. Os demais escritores, leitores e sua concordância na árvore inteira permanecem [não verificado].

- **`resposta_mantem_validacao`:** a resposta humana grava `False` em [disputa_emissao.py:1534](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/disputa_emissao.py:1534); portas automática e de trilha gravam `True` em [disputa_emissao.py:1688](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/disputa_emissao.py:1688) e [disputa_emissao.py:1710](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/disputa_emissao.py:1710); a fotografia da projeção grava `True` em [models.py:4260](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:4260), e a migração histórica usa motivo não vazio em [0056:14](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:14). Leem-no a leitura Python, a face SQL e a tarefa em [models.py:4292](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:4292), [models.py:3551](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:3551) e [tarefa_do_dia.py:164](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/tarefa_do_dia.py:164). A diferença em sequências interrompidas consta no achado F6F7-3.

- **`da_ressuscitacao`:** a ressuscitação grava a marca em [models.py:1699](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:1699); o predicado a lê em [models.py:4221](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:4221), e a carga exclui esses atos em [tarefa_do_dia.py:399](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/tarefa_do_dia.py:399). Exclusividade em toda a árvore: [não verificado].

- **Passivo e `validada_projetada_sem_linha`:** a correspondência usa pergunta, ato e instante em [models.py:4254](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:4254). O contador usa essa autoridade em [backfill_decisoes_pergunta.py:108](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/management/commands/backfill_decisoes_pergunta.py:108); o vigia o lê e grava no carimbo em [vigiar_fio_morto.py:192](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/management/commands/vigiar_fio_morto.py:192) e [vigiar_fio_morto.py:262](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/management/commands/vigiar_fio_morto.py:262). Outros consumidores do carimbo e interpretação externa: [não verificado].

- **`hoje`, `ini`, `fim` e `ultimo_dia_medido`:** a porta seleciona células geradas com data civil **estritamente posterior** a `hoje` em [cadastro_futuro.py:21](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/ponto/services/cadastro_futuro.py:21). O vigia inclui células a partir de `ini`, sem usar `fim` como teto, e calcula `ultimo_dia_medido` como máximo ou `None` em [vigia_cadastro_futuro.py:30](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/ponto/services/vigia_cadastro_futuro.py:30) e [vigia_cadastro_futuro.py:46](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/ponto/services/vigia_cadastro_futuro.py:46). O detalhe é gravado e recuperado em [vigia_cadastro_futuro.py:107](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/ponto/services/vigia_cadastro_futuro.py:107) e [vigia_cadastro_futuro.py:119](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/ponto/services/vigia_cadastro_futuro.py:119). Concordância de todos os chamadores e leitores externos: [não verificado].

- **`agora` e `dias_esperando`:** a borda da fila obtém um instante em [views_mensageria.py:543](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/api/views_mensageria.py:543); o item calcula dias inteiros pela diferença entre instantes em [views_mensageria.py:572](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/api/views_mensageria.py:572). Se todos os consumidores esperam dias transcorridos, e não mudança de dia civil, é [não verificado].

### Achados novos desta volta

1. **FILA — leitura da projeção ao perguntar ao juiz de resposta.** `validar_inline` passa a pergunta ao juiz em [views_cobrar.py:170](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/views_cobrar.py:170) e [views_cobrar.py:178](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/views_cobrar.py:178); o juiz lê os campos recebidos em [juizes.py:1552](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/juizes.py:1552), enquanto a property entrega a leitura da sequência em [models.py:3791](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:3791). Se projeção e sequência divergirem, as respostas podem diferir; alcance real e regra para essa porta são [não verificado]. Um teste apenas com projeção e sequência concordantes passaria com a divergência; teste específico que sobreviva: [não verificado].

2. **FILA — modal mistura resposta da sequência com campos projetados.** A condição `perg.respondida` usa a property, mas o texto, o instante e o selo de validação são lidos diretamente em [modal_fio.html:217](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/templates/chamados/partials/modal_fio.html:217) e [modal_fio.html:345](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/templates/chamados/partials/modal_fio.html:345). O teste F7 cria projeção sem resposta e valida o rótulo em [test_f7_validada_conservada.py:44](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/tests/test_f7_validada_conservada.py:44); ele não demonstra o comportamento quando uma sequência posterior contradiz essa projeção. Existência e apresentação de casos reais: [não verificado].

3. **FILA — teste do endpoint não morde passivo sempre verdadeiro isoladamente.** Substituir o ramo passivo por `True` preservaria a expectativa do teste de endpoint em [test_f6_u4_aguarda_admin_pela_tarefa.py:67](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/api/tests/test_f6_u4_aguarda_admin_pela_tarefa.py:67), por inspeção; o teste unitário de `False` em [test_f6_u4_aguarda_admin_pela_tarefa.py:80](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/api/tests/test_f6_u4_aguarda_admin_pela_tarefa.py:80) deve rejeitar esse mutante, mas execução e resultado efetivo são [não verificado].

A verificação de todos os juízes registrados, de todos os campos e funções do diff e de todos os mutantes pedidos permanece **[não verificado]**. Não há base neste retorno para afirmar aprovação, ausência de outros bloqueantes ou impacto em produção.

## 18/11 19:35 · sessao a0368c88b4f9 · ESCALA-NAO-DESCREVE · verificador · origem=Code

**Resultado parcial: há uma lacuna de teste confirmada por inspeção, uma classificação cíclica duplicada e uma proposta antiga que continua aplicável sem verificação de idade.** Não posso dar um aval completo: não concluí o censo de **todos** os campos, chaves e funções do diff, nem a análise de todos os juízes e mutantes. Esses pontos ficam **[não verificado]**. [nao verificado]

A cópia não contém metadados Git; portanto, a identidade do hash e o merge-base com `main` não foram confirmados. Usei o diff fornecido e as linhas da cópia presente. Não executei testes nem mutantes; as conclusões sobre sobrevivência são por inspeção. Produção, banco e logs: **[não verificado]**. [nao verificado]

### Achados da árvore

- **FILA — classificação de destino cíclico duplicada.** O botão classifica pela pertinência a `TipoEscala.CICLICOS` em `app/escala/services/cadastro_realidade.py:849`; a porta repete a classificação em `app/escala/regua_defesa.py:313`; e `julgar_vinculo` contém ainda a tupla literal `('12x36', '24x48')` para exigir âncora em `app/colaboradores/services/vinculo.py:128`. As condições não são idênticas: a última se aplica à criação sem âncora. Hoje a constante contém os mesmos dois valores em `app/escala/models.py:386`; a possibilidade de divergência futura decorre das cópias. O teste R5 passaria se apenas a recusa de `regua_defesa` fosse removida, pois o POST é barrado antes em `app/escala/views.py:498` e o teste só verifica que não houve vínculo em `app/escala/tests/test_escala_nao_descreve.py:299`. Sobrevivência executada: **[não verificado]**.

- **FILA — proposta de lavra antiga pode ser aplicada.** `ler_lavra` devolve o snapshot mais recente disponível sem comparar a data com hoje em `app/ponto/services/esmeril_espelho.py:371`; o POST lê proposta e assinaturas dessa lavra e verifica o botão, mas não sua idade, em `app/escala/views.py:493`. A data da lavra é registrada no motivo em `app/escala/views.py:504`. Se isso viola um prazo autorizado ou ocorreu em produção: **[não verificado]**. O teste de lavra antiga remove as chaves novas, mas não exercita uma lavra de outro dia **com** proposta em `app/escala/tests/test_escala_nao_descreve.py:358`.

- **FILA — o teste R1 não fixa a borda da janela independentemente.** A produção filtra `data__gte=ini` e `data__lte=fim` em `app/ponto/services/esmeril_espelho.py:99`; a fixture põe células em 10 e 11/11 em `app/escala/tests/test_escala_nao_descreve.py:63`, e compara o `desde` com a própria função `janela` em `app/escala/tests/test_escala_nao_descreve.py:94`. Por inspeção, um mutante que deslocasse o início da janela, preservando essas células no intervalo, poderia passar nesse teste; a execução e o efeito sobre a bateria inteira são **[não verificado]**.

- **FILA — o selo antigo de agrupamento não prova agrupamento.** Ele verifica a presença do nome do posto em `app/escala/tests/test_cadastro_x_realidade.py:53`, enquanto o template apresenta posto como coluna em `app/templates/escala/cadastro_x_realidade.html:26`. O nome do teste ainda figura como selo de L-040 em `app/docs/LEIS.md:88`. A mudança de layout está declarada na própria L-040; o problema observado é o alcance do nome e da asserção do selo.

### Escritores, leitores e significado: recorte verificado

Isto **não é o censo completo solicitado**; os demais campos, consumidores indiretos e compatibilidade global ficam **[não verificado]**.

| Dado ou função | Escritor e leitores encontrados | Significado e limite verificado |
|---|---|---|
| `ata.orfas` | O cartório compõe a ata em `app/ponto/services/cartorio.py:738`; `assinaturas_do_colab` lê em `app/ponto/services/esmeril_espelho.py:130`. | A carga soma o número de itens, inclusive horas não interpretáveis, em `app/ponto/services/esmeril_espelho.py:131`. Outros escritores e leitores na árvore inteira: **[não verificado]**. |
| `medida['orfas']`, `orfas`, `orfas_desde` | Escritos em `app/ponto/services/esmeril_espelho.py:142` e `app/ponto/services/esmeril_espelho.py:236`; lidos pela lista em `app/escala/services/cadastro_realidade.py:969`. | Contagem por pessoa; zero é medida sem órfãs; ausência da chave é sem medida. A janela consulta datas de célula com ambas as bordas inclusivas e termina ontem: `app/ponto/services/esmeril_espelho.py:38`, `app/ponto/services/esmeril_espelho.py:63`, `app/ponto/services/esmeril_espelho.py:99`. Equivalência entre dia da célula, dia civil e dia do turno para todas as órfãs: **[não verificado]**. |
| `proposta` e `destino` | `propor_escala` é chamado em `app/ponto/services/esmeril_espelho.py:241`; a lavra escreve mapa estruturado e frase em `app/ponto/services/esmeril_espelho.py:242`; a lista e o POST leem a proposta em `app/escala/services/cadastro_realidade.py:971` e `app/escala/views.py:494`. | A proposta cobre os 60 dias até ontem em `app/ponto/services/esmeril_espelho.py:241`; a carga usa outra janela em `app/ponto/services/esmeril_espelho.py:233`. O ciclo da proposta não é guardado; o botão consulta o cadastro atual em `app/escala/services/cadastro_realidade.py:819`. Todos os demais consumidores e sua interpretação: **[não verificado]**. |
| `carga`, `carga_desde`, `carga_texto`, `linhas`, `carga_resumo` | A lista monta a linha em `app/escala/services/cadastro_realidade.py:981`, ordena em `app/escala/services/cadastro_realidade.py:1055` e publica o resumo em `app/escala/services/cadastro_realidade.py:1067`; tela e PDF leem a frase em `app/templates/escala/cadastro_x_realidade.html:17` e `app/escala/services/cadastro_realidade.py:1184`. | O resumo soma cargas positivas das linhas listadas, usa a menor data delas e conta sem medida separadamente em `app/escala/services/cadastro_realidade.py:788`. Ele não representa necessariamente toda a frota: o universo das linhas é formado a partir de assinaturas e cadastro em `app/escala/services/cadastro_realidade.py:961`. |
| `postos` e `linhas` | Ambos saem de `lista` em `app/escala/services/cadastro_realidade.py:1062`; a tela e o PDF usam `linhas`, enquanto o vigia usa `postos` em `app/chamados/services/vigia_fio.py:196`. | São duas apresentações dos registros montados pela lista, com ordenações diferentes, conforme `app/escala/services/cadastro_realidade.py:1055`. Todos os leitores de `postos`: **[não verificado]**. |
| `tipo_ciclo`, `template_id`, `confirm_txt` | `_vinculos` lê o vínculo do dia e o ciclo em `app/escala/services/cadastro_realidade.py:766`; a porta consulta o template em `app/escala/regua_defesa.py:310`, envia `confirm_txt` em `app/escala/regua_defesa.py:316`, e a execução o inclui em `obs` em `app/colaboradores/services/vinculo.py:350`. | `hoje` é uma data local passada do POST à porta em `app/escala/views.py:496` e `app/escala/views.py:506`. O censo completo dos escritores e leitores dessas colunas é **[não verificado]**. |

Quanto aos juízes registrados, `_vinculos` chama explicitamente os juízes de cobertura e vínculo do dia em `app/escala/services/cadastro_realidade.py:756` e `app/escala/services/cadastro_realidade.py:770`, correspondentes ao registro em `app/docs/JUIZES.md:146`. A duplicação cíclica descrita acima está comprovada entre as três condições citadas; que ela seja duplicação de um juiz **registrado em JUIZES.md**, ou que não haja outro juiz registrado contornado no diff inteiro, é **[não verificado]**.

### Matriz dos 15 achados dos postmortens

A classe e a identificação vêm dos anexos fornecidos; suas linhas originais são **[não verificado]**. “FILA” abaixo indica trabalho de verificação ou defeito de teste/documentação identificado, não uma aprovação da obra. [nao verificado]

| Achado | Classe | Sítio nesta árvore e resultado por classe | Triagem |
|---|---|---|---|
| F6F7 1 — significado entre leitores | (a) | Carga e janelas em `app/ponto/services/esmeril_espelho.py:99` e `app/escala/services/cadastro_realidade.py:981`: **[não verificado]** para todos os leitores e dias. | FILA |
| F6F7 2 — ausência de chamador | (a) | `medida` é passada pelo chamador de frota em `app/ponto/services/esmeril_espelho.py:235`; ausência de outros chamadores relevantes: **[não verificado]**. | FILA |
| F6F7 3 — sequência de eventos omitida | (a) | Sequência snapshot → mudança cadastral → POST em `app/escala/views.py:493`: lavra antiga é aceita; validade exigida: **[não verificado]**. | FILA |
| F6F7 4 — mutação que escapa ao selo | (a) | R1 e borda em `app/escala/tests/test_escala_nao_descreve.py:86`: mutante de janela descrito acima sobrevive **por inspeção**; execução **[não verificado]**. | FILA |
| F6F7 5 — pergunta omitida | (e) | Aval literal e citação em `app/docs/LEIS.md:90`: completude das perguntas contra os avais, cujos textos integrais não foram fornecidos, **[não verificado]**. | FILA |
| F6F7 6 — erro de fixture omitido | (c) | Fixtures em `app/escala/tests/test_escala_nao_descreve.py:43`: execução e eventuais erros de preparação **[não verificado]**. | FILA |
| Escala 1 — mutante R1 | (a) | O caso de quatro órfãs agora é afirmado em `app/escala/tests/test_escala_nao_descreve.py:91`; o mutante anterior específico e a bateria executada são **[não verificado]**. A borda de janela continua sem prova independente. | FILA |
| Escala 2 — rótulo LEI-AKITA | (a) | Novos predicados em `app/escala/services/cadastro_realidade.py:809` e `app/escala/services/cadastro_realidade.py:830`; rótulo da obra e sua correspondência integral **[não verificado]**. | FILA |
| Escala 3 — segundo juiz cíclico | (a) | **Sim**, há condições repetidas em `app/escala/services/cadastro_realidade.py:849`, `app/escala/regua_defesa.py:313` e `app/colaboradores/services/vinculo.py:128`; condições e propósito não são idênticos. | FILA |
| Escala 4 — R5 não chega à porta | (a) | **Sim**: guarda anterior em `app/escala/views.py:498`; R5 em `app/escala/tests/test_escala_nao_descreve.py:299`. Remover a recusa da porta sobrevive ao R5 por inspeção. | FILA |
| Escala 5 — R4 não confere `h.de` | (a) | **Não nesse teste atual**: R4 confere `h.de` em `app/escala/tests/test_escala_nao_descreve.py:288`. Cobertura de todos os formatos do antes **[não verificado]**. | não se aplica ao defeito específico |
| Escala 6 — lavra vencida | sem classe compatível | **Sim**, não há guarda de idade nos sítios `app/ponto/services/esmeril_espelho.py:371` e `app/escala/views.py:493`; prazo obrigatório **[não verificado]**. | FILA |
| Escala 7 — literal do aval | (e) | A lavra omite `ciclo` em `app/ponto/services/esmeril_espelho.py:245`; o link aponta ao vínculo em `app/escala/tests/test_escala_nao_descreve.py:305`. Conformidade com o aval integral: **[não verificado]**. | FILA |
| Escala 8 — rótulo do selo | (a) | **Sim**, o teste verifica presença, não agrupamento, em `app/escala/tests/test_cadastro_x_realidade.py:53`; ainda é citado em `app/docs/LEIS.md:88`. | FILA |
| Escala 8 — citação L-042 | (e) | L-042 afirma que proposta é recalculada em `app/docs/LEIS.md:90`; o novo mapa é guardado em `app/ponto/services/esmeril_espelho.py:246`. A célula PROTEGE vazia não obriga citação pelo selo, conforme `app/docs/LEIS.md:43` e `bin/tests/test_lei_protege_sitio.sh:77`; aplicabilidade material da lei e citações nos commits **[não verificado]**. | FILA |

**Achados novos desta volta:** a borda da janela sem prova independente é **FILA**; a exaustividade dos leitores, dos juízes e dos mutantes permanece **FILA [não verificado]**. A duplicação cíclica, o R5 que não alcança a porta, a lavra vencida e o selo de agrupamento já constavam dos postmortens fornecidos, portanto não os conto como novos. Não há base suficiente nesta verificação parcial para declarar que nenhum achado **BLOQUEIA** exista.

## 18/11 19:35 · sessao 284d4a129c31 · - · - · origem=Ronald (tmux)

A auditoria da tarefa e dos selos encontrou:

- Os dois selos AST aceitam quatro mutantes que esvaziam ou alteram a lista `perguntas` por alias ou por métodos de `list`. Verifiquei os helpers em memória; a execução da suíte inteira é **[não verificada]**. A lista é criada em [pergunta_no_app.py](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/pergunta_no_app.py:84), e os selos estão em [test_contract_pergunta_viva.py](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/tests/test_contract_pergunta_viva.py:26) e [test_pergunta_no_app.py](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/tests/test_pergunta_no_app.py:29).

- A sequência `VALIDACAO → SILENCIO → RESPOSTA` que mantém validação produz `aguarda_validacao` na tarefa, pois o laço só recua por respostas contíguas; a leitura do ato conserva a validação. Qual resultado a lei exige para essa sequência é **[não verificado]**. [tarefa_do_dia.py](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/tarefa_do_dia.py:163), [models.py](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:4313).

- A face por pergunta chama `_pela_decisao` diretamente, enquanto a tarefa do dia aplica guardas adicionais de vida, lâmpada, teto e ausência. A equivalência das duas saídas em todos os casos é **[não verificada]**. [tarefa_do_dia.py](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/tarefa_do_dia.py:276), [face por pergunta](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/tarefa_do_dia.py:436).

- O teste de endpoint do passivo cobre apenas `conservado=True`, mas outro teste no mesmo arquivo cobre também `False`; portanto, o mutante sempre `True` sobrevive ao primeiro teste, não à tabela unitária por inspeção. [test_f6_u4_aguarda_admin_pela_tarefa.py](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/api/tests/test_f6_u4_aguarda_admin_pela_tarefa.py:67).

- A partição do app passa a projeção diretamente ao juiz de resposta; a property `respondida` passa a leitura da sequência. Sua concordância quando projeção e sequência divergem é **[não verificada]**. [pergunta_no_app.py](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/pergunta_no_app.py:80), [models.py](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:3785).

## 18/11 19:34 · sessao 568d698c9c3e · - · - · origem=Ronald (tmux)

A auditoria parcial de decisões e seus testes foi enviada ao agente principal com referências de arquivo e linha. A divergência potencial entre a face SQL e o C6 depende das demais guardas de vitalidade; sua incidência em produção permanece **[não verificado]**. [nao verificado]

## 18/11 19:34 · sessao 0f3aa6cde070 · - · - · origem=Ronald (tmux)

A auditoria do cadastro futuro, do vigia e das folgas foi enviada ao agente principal com referências de arquivo e linha, incluindo o risco de uma medição parcial sobrescrever o snapshot rotulado como frota. A ocorrência desse cenário em produção permanece **[não verificado]**.

## 18/11 19:34 · sessao 21982179cf05 · RAIA-ESPURIAS · leitura · origem=Code

**Não consigo entregar a classificação completa como sonda confiável com o que foi verificado.** A ata guarda horários de órfãs, não a identidade das batidas; pode também guardar só a contagem. Portanto, associar uma órfã a `origem` ou `pergunta_origem` exige resolver ambiguidades que não verifiquei. [app/ponto/services/cartorio.py:730](app/ponto/services/cartorio.py:730), [app/escala/services/leitor_celula.py:180](app/escala/services/leitor_celula.py:180) [nao verificado]

O juiz `dono_da_divergencia(horas_do_dia, em_cadastro_x_realidade, folga_nao_definida=False, sem_vinculo_no_dia=False, sem_vinculo_em_dia_de_trabalho=False, batida_fora_de_turno=False, fase_deslocada=False)` é uma função pura, mas não recebe diretamente `(colab, dia)`; `InsumosDoDono.dono(data)` monta os insumos e pode chamar o espelho. Não conferi toda a cadeia de efeitos dessa montagem, portanto a sonda parcial abaixo não a chama. [app/ponto/management/commands/e6_oraculo.py:53](app/ponto/management/commands/e6_oraculo.py:53), [app/ponto/management/commands/e6_oraculo.py:162](app/ponto/management/commands/e6_oraculo.py:162), [app/ponto/management/commands/e6_oraculo.py:182](app/ponto/management/commands/e6_oraculo.py:182) [nao verificado]

A lista do admin combina a lavra `esmeril_espelho` com outras fontes; parte das assinaturas é agregada por colaborador sem dia, enquanto outras são por dia. Não verifiquei uma leitura completa que permita concluir “sem destino nenhum” para cada dia. [app/escala/services/cadastro_realidade.py:773](app/escala/services/cadastro_realidade.py:773), [app/escala/services/cadastro_realidade.py:869](app/escala/services/cadastro_realidade.py:869), [app/escala/services/cadastro_realidade.py:922](app/escala/services/cadastro_realidade.py:922)

O texto abaixo é **uma sonda parcial**, para medir o universo da ata e expor a ausência de identidade das órfãs. Ela deixa todos os dias como não classificados e não declara o item fechado. Não foi executada. A janela inclui as datas fixas completas, inclusive dias futuros existentes na sombra; a medida anexada de 17/11 era limitada ao dia da medição, de modo que os totais não são diretamente comparáveis. [nao verificado]

```python
# SONDA PARCIAL — ITEM 2 FOLGA — NAO EXECUTADA
#
# Precedencia solicitada para a sonda completa:
# a > b > d > e > c > f.
# Esta sonda NAO aplica a precedencia: nao identificou com seguranca
# as batidas correspondentes a cada orfa da ata.
#
# Fontes:
# app/escala/orfa_classe.py:53 — classificar_orfas(dia), funcao pura.
# app/ponto/services/cartorio.py:730 — ata, contagem e horas de orfas.
# app/escala/services/leitor_celula.py:180 — ata antiga pode nao ter horas.
# app/ponto/management/commands/e6_oraculo.py:53 — juiz de dono puro;
# sua montagem completa e efeitos transitivos: [nao verificado].
#
# Universo desta parcial: CelulaDia nas empresas 2-4, datas fixas,
# ata.n_orfas > 0 e ata.tipo_dia em folga/ausencia. Nao corta em hoje.
# A classe da porta e confirmada quando a lista de horas esta disponivel.
# Sem lista coerente, a contagem entra no universo, com erro declarado.

import collections
import datetime

from escala.models import CelulaDia
from escala.orfa_classe import FOLGA_DESMENTIDA, classificar_orfas

INI = datetime.date(2026, 10, 21)
FIM = datetime.date(2026, 11, 20)
ERROS = collections.Counter()
dias = 0
orfas = 0
pessoas = set()
confirmados_porta = 0
sem_identidade = 0


def erro(etapa, exc):
    chave = "%s:%s" % (etapa, type(exc).__name__)
    ERROS[chave] += 1
    print("ERRO", chave)


# Antes de ler registros, mostra nomes de campos do modelo efetivamente lido.
print("CAMPOS", CelulaDia._meta.label,
      sorted(f.name for f in CelulaDia._meta.get_fields()))

print("PERIODO_FIXO", INI.isoformat(), FIM.isoformat())
print("EMPRESAS", "2,3,4")
print("CORTE_HOJE", "nenhum")
print("PRECEDENCIA", "a > b > d > e > c > f; nao aplicada") [nao verificado]

try:
    consulta = (
        CelulaDia.objects
        .filter(
            colaborador__empresa_id__in=(2, 3, 4),
            data__range=(INI, FIM),
            ata__n_orfas__gt=0,
            ata__tipo_dia__in=("folga", "ausencia"),
        )
        .order_by("colaborador_id", "data", "pk")
        .values_list("colaborador_id", "data", "ata")
    )
    for cid, data, ata in consulta.iterator():
        try:
            n = ata["n_orfas"]
            if isinstance(n, bool) or not isinstance(n, int) or n <= 0:
                raise ValueError("n_orfas invalido")

            dias += 1
            orfas += n
            pessoas.add(cid)

            horas = ata.get("orfas")
            if not isinstance(horas, list) or len(horas) != n:
                sem_identidade += 1
                raise ValueError("lista_de_orfas_ausente_ou_incoerente")

            resultado = classificar_orfas({
                "tipo_dia": ata.get("tipo_dia"),
                "orfas": horas,
            })
            if not resultado or any(
                classe != FOLGA_DESMENTIDA
                for classe in resultado.values()
            ):
                raise ValueError("porta_nao_confirmou_folga_desmentida")
            confirmados_porta += 1
        except Exception as exc:
            erro("dia", exc)
except Exception as exc:
    erro("consulta", exc)

# O dicionario da porta usa horas como chaves; nao se usa seu tamanho
# para contar orfas que possam compartilhar a mesma hora.
print("UNIVERSO_PARCIAL dias=%d pessoas=%d orfas=%d" %
      (dias, len(pessoas), orfas))
print("PORTA_CONFIRMADA_DIAS", confirmados_porta)
print("ATA_SEM_LISTA_COERENTE_DIAS", sem_identidade)
print("DONO=[nao chamado] razao=efeitos_transitivos_nao_verificados")
print("DESTINO=[nao verificado]")
print("SOMA_CONFERE [nao verificado]")
print("NAO_CLASSIFICADO=%d" % dias)
print("RESUMO_ITEM2 INCOMPLETO")
print("ERROS_DETALHE", dict(sorted(ERROS.items())))
print("ERROS=%d" % sum(ERROS.values()))
print("FIM_ITEM2_FOLGA INCOMPLETO")
```

Os valores das seis classes, seus donos e destinos permanecem **[nao verificado]**. A sonda parcial também não resolve a associação das órfãs a batidas, os pares noturnos ou a leitura completa da lista do admin. [nao verificado]

## 18/11 19:33 · sessao 885a42979426 · RAIA-ESPURIAS · leitura · origem=Code

Não consigo entregar uma sonda fiel à **mesma porta de órfãs** com o anexo fornecido: `sonda_sobra_1711.py` consulta e imprime batidas dos três casos `CASOS`, mas não mostra seleção de órfãs nem a classificação que produziu `sobra_1711.out`. A busca na cópia também não encontrou o código que imprime `SONDA-SOBRA` ou `toque_duplo`. [não verificado]

A cópia oferece outra porta, `grade_da_celula`, que repassa as órfãs da ata e usa `?` quando só há contagem. Ela é usada pelo raio-x, mas não há evidência de que seja a porta da sonda que produziu a partição solicitada. Usá-la como substituta seria decidir justamente o ponto que o pedido proíbe reinventar. [app/escala/services/leitor_celula.py:180; app/escala/services/leitor_celula.py:350; não verificado]

Sobre o **dia verde**, o código do anexo exige dia `trabalho`, ao menos uma célula e nenhuma célula com `missing` verdadeiro. Isso confirma o critério operacional de ausência de `missing`, mas não prova literalmente que cada marco tenha hora ou status `ok`: o leitor pode devolver `missing=False` com informações desconhecidas, inclusive na forma agregada. [app/escala/services/leitor_celula.py:114; app/escala/services/leitor_celula.py:160; não verificado]

A soma **não bate em órfãs**: T3 + T4 + T5 no `sobra_1711.out` é **71 + 28 + 11 = 110**, contra as **105** atribuídas ao raio-x. As contagens de dias das três linhas somam 91, mas não podem ser comparadas diretamente a 83 sem conhecer a sobreposição de dias entre classes; o mesmo cuidado vale para pessoas. A causa da diferença de cinco órfãs não é demonstrada pelos dados fornecidos. [não verificado]

Para escrever o script pedido sem trocar a fonte, falta o **código efetivo da sonda que gerou `sobra_1711.out`**, incluindo a seleção de órfãs e o cálculo das distâncias. A precedência pode então ser explicitada na sonda conforme a decisão técnica desta sessão; o resultado e a explicação de eventual diferença em relação às 1.267 órfãs continuam dependendo da execução na sombra. [não verificado]

## 18/11 19:30 · sessao e13c90ffe027 · RAIA-ESPURIAS · leitura · origem=Code

A sonda lê a ata lavrada em `CelulaDia.ata`, a mesma fonte que `grade_da_celula` consulta; `ata_do_dia` grava nela a lista `orfas`. Não recalcula a grade. [app/escala/models.py:1773](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/models.py:1773), [app/escala/services/leitor_celula.py:350](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/leitor_celula.py:350), [app/ponto/services/cartorio.py:730](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:730)

No bloco B, o casamento é **aproximado por HH:MM**, consultando a ata do dia civil de cada batida e a da véspera. A ambiguidade é medida entre batidas apuráveis do colaborador no mesmo dia civil. Ata ausente ou sem lista reconhecível produz resultado indeterminado, não negativo. A sonda mostra também o dia de apresentação atribuído pela função da casa para abrir os casos de col67. [app/ponto/turnos.py:2899](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:2899), [não verificado]

Funções da cópia chamadas diretamente:

- `batidas_apuraveis`: [app/ponto/turnos.py:2868](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:2868).
- `dia_das_batidas`: [app/ponto/turnos.py:2899](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:2899); `janela_com_pad`: [app/ponto/turnos.py:1834](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1834).

A sonda e seus resultados na sombra, inclusive contagens, preenchimento dos campos e `ERROS=0`, ficam **[não verificado]**.

```python
# Executar no shell Django do tenant, pelo envelope da sessao.
# BLOCO B: casamento aproximado por HH:MM com ata do dia civil e da vespera.
# Somente consultas; nao chama portas de escrita nem monta a grade.

from collections import Counter, defaultdict
from datetime import date, datetime, time, timedelta
import re

from django.apps import apps
from django.utils import timezone
from colaboradores.models import Colaborador
from escala.models import CelulaDia
from ponto.models import Batida
from ponto.turnos import batidas_apuraveis, dia_das_batidas, janela_com_pad


ERROS = 0
UM_DIA = timedelta(days=1)
INI_DIA = date(2026, 10, 21)
FIM_DIA_EXCLUSIVO = date(2026, 11, 21)
EMPRESAS = (2, 3, 4)
FAIXAS = ("0-10s", "11-60s", "61s-10min")
CUMULATIVAS = ("ate_10s", "ate_60s", "ate_10min")


def erro(etapa, exc):
    global ERROS
    ERROS += 1
    # Nao imprime a mensagem: ela pode conter dados pessoais ou SQL.
    print("ERRO etapa={} tipo={}".format(etapa, type(exc).__name__))


def limite(dia):
    return timezone.make_aware(datetime.combine(dia, time.min),
                               timezone.get_current_timezone())


def local(ts):
    if ts is None:
        return None
    if timezone.is_naive(ts):
        raise ValueError("datetime sem timezone")
    return timezone.localtime(ts)


def instante(ts):
    valor = local(ts)
    return valor.strftime("%d/%m/%Y %H:%M:%S.%f %z") if valor else None


def faixa(segundos):
    # Limites reais, sem arredondar: 10 < s <= 60 pertence a 11-60s.
    if 0 <= segundos <= 10:
        return "0-10s"
    if segundos <= 60:
        return "11-60s"
    if segundos <= 600:
        return "61s-10min"
    return None


def grupos(segundos):
    f = faixa(segundos)
    if f is None:
        return ()
    saida = [f, "ate_10min"]
    if segundos <= 60:
        saida.append("ate_60s")
    if segundos <= 10:
        saida.append("ate_10s")
    return tuple(saida)


def registro(b, campos):
    resultado = {
        "col": "col{}".format(b.colaborador_id),
        "pk_batida": b.pk,
        "instante": instante(b.timestamp),
        "tipo": b.tipo,
        "origem": b.origem,
        "criado_em": instante(b.criado_em),
    }
    for nome in ("canal", "terminal", "timestamp_dispositivo"):
        if nome in campos:
            valor = getattr(b, nome)
            resultado[nome] = instante(valor) if nome == "timestamp_dispositivo" else valor
    return resultado


def lista_ata(celula):
    if celula is None:
        return "celula_ausente", None
    ata = celula.ata
    if not isinstance(ata, dict):
        return "ata_ausente_ou_invalida", None
    if "orfas" not in ata or not isinstance(ata["orfas"], list):
        return "lista_orfas_ausente_ou_invalida", None
    valores = ata["orfas"]
    if any(not isinstance(h, str) or not re.fullmatch(r"\d{2}:\d{2}", h)
           for h in valores):
        return "lista_orfas_invalida", None
    return "disponivel", valores


def mostrar_ata(cid, dia, cels):
    estado, horas = lista_ata(cels.get((cid, dia)))
    celula = cels.get((cid, dia))
    print("ATA col{} dia={} estado={} orfas={} marcos_dna={} lampadas={}".format(
        cid, dia.isoformat(), estado, horas,
        (celula.dna or {}).get("marcos")
        if celula and isinstance(celula.dna, dict) else None,
        (celula.ata or {}).get("lampadas")
        if celula and isinstance(celula.ata, dict) else None,
    ))


def cruzar(b, cels, minutos):
    dt = local(b.timestamp)
    dia = dt.date()
    hhmm = dt.strftime("%H:%M")
    consultas = []
    encontrou = False
    desconhecida = False
    for data_ata in (dia, dia - UM_DIA):
        estado, horas = lista_ata(cels.get((b.colaborador_id, data_ata)))
        casou = None if horas is None else hhmm in horas
        encontrou = encontrou or casou is True
        desconhecida = desconhecida or casou is None
        consultas.append({
            "dia_ata": data_ata.isoformat(),
            "estado": estado,
            "orfas": horas,
            "casou_hhmm": casou,
        })
    situacao = "sim" if encontrou else ("indeterminado" if desconhecida else "nao")
    candidatos = minutos[(b.colaborador_id, dia, hhmm)]
    return {
        "orfa_hhmm": situacao,
        "ambiguo": candidatos > 1,
        "batidas_apuraveis_no_minuto_civil": candidatos,
        "atas": consultas,
    }


def resultado_par(a, b):
    if "sim" in (a, b):
        return "sim"
    if "indeterminado" in (a, b):
        return "indeterminado"
    return "nao"


def abrir_caso(dia, campos, cels):
    cid = 67
    # O mapa da casa inclui batidas reclamadas pelo turno e atribui as demais
    # ao dia civil. O pad vem da mesma porta; a consulta e apuravel.
    colab = Colaborador.objects.filter(pk=cid, empresa_id__in=EMPRESAS).first()
    print("CASO col67 dia={}".format(dia.isoformat()))
    if colab is None:
        print("CASO col67 fora_da_frota_ou_ausente")
        return
    mapa = dia_das_batidas(colab, dia, dia)
    pad_ini, pad_fim = janela_com_pad(dia, dia)
    bs = list(
        batidas_apuraveis(colab, limite(pad_ini))
        .filter(timestamp__lt=limite(pad_fim + UM_DIA))
        .order_by("timestamp", "pk")
    )
    mostrar_ata(cid, dia, cels)
    for b in bs:
        civil = local(b.timestamp).date()
        atribuido = mapa.get(b.pk)
        if civil == dia or atribuido == dia:
            r = registro(b, campos)
            r["dia_civil"] = civil.isoformat()
            r["dia_apresentacao_casa"] = atribuido.isoformat() if atribuido else None
            r["no_dia_civil"] = civil == dia
            r["no_dia_apresentacao"] = atribuido == dia
            print("CASO_BATIDA", r)


def executar():
    # Inclui modelos que as funcoes da casa possam consultar indiretamente.
    # Sao nomes de campos, nao valores de pessoas.
    for modelo in sorted(apps.get_models(), key=lambda m: m._meta.label_lower):
        print("CAMPOS {} {}".format(
            modelo._meta.label, [f.name for f in modelo._meta.get_fields()]
        ))

    campos = {f.name for f in Batida._meta.get_fields()}
    print("CAMPOS_DIMENSOES_BATIDA", [
        n for n in ("origem", "canal", "terminal", "timestamp_dispositivo")
        if n in campos
    ])
    print("UNIVERSO empresas=2,3,4 inicio_local=21/10/2026_00:00:00 "
          "fim_local_exclusivo=21/11/2026_00:00:00")

    # ---------- BLOCO A: apenas Batida, sem grade nem ata ----------
    print("BLOCO_A")
    inicio = limite(INI_DIA)
    fim = limite(FIM_DIA_EXCLUSIVO)
    batidas = list(
        batidas_apuraveis(None, inicio)
        .filter(timestamp__lt=fim, colaborador__empresa_id__in=EMPRESAS)
        .order_by("colaborador_id", "timestamp", "pk")
    )
    pares = []
    anterior = None
    for b in batidas:
        if anterior is not None and anterior.colaborador_id == b.colaborador_id:
            segundos = (b.timestamp - anterior.timestamp).total_seconds()
            if anterior.tipo != b.tipo and faixa(segundos) is not None:
                pares.append((anterior, b, segundos))
        anterior = b

    # Cada par conta uma vez em cada faixa aplicavel; pessoas sao distintas
    # dentro de cada faixa e de cada combinacao ordenada de dimensoes.
    agregados = defaultdict(lambda: [0, set()])
    for a, b, segundos in pares:
        dimensoes = [
            ("origem_a/origem_b", (a.origem, b.origem)),
        ]
        for campo in ("canal", "terminal"):
            if campo in campos:
                dimensoes.append(
                    (campo + "_a/" + campo + "_b",
                     (getattr(a, campo), getattr(b, campo)))
                )
        for grupo in grupos(segundos):
            for nome, valor in dimensoes:
                item = agregados[(grupo, nome, valor)]
                item[0] += 1
                item[1].add(a.colaborador_id)

    for grupo in FAIXAS + CUMULATIVAS:
        selecionados = [(a, b, s) for a, b, s in pares if grupo in grupos(s)]
        print("A_TOTAL faixa={} pares={} pessoas={}".format(
            grupo, len(selecionados),
            len({a.colaborador_id for a, _, _ in selecionados})
        ))
        itens = [(chave, valor) for chave, valor in agregados.items()
                 if chave[0] == grupo]
        for (g, nome, valor), (qtd, pessoas) in sorted(itens, key=lambda x: repr(x[0])):
            print("A_DIMENSAO faixa={} dimensao={} valores={} pares={} pessoas={}".format(
                g, nome, valor, qtd, len(pessoas)
            ))

    # ---------- BLOCO B: ata lavrada; comparacao aproximada HH:MM ----------
    print("BLOCO_B criterio=HH:MM dia_civil_e_vespera")
    ids = {a.colaborador_id for a, _, _ in pares}
    ids.add(67)

    # Para ambiguidades, inclui dias civis completos mesmo nas bordas
    # da competencia. Os pares continuam restritos ao universo do bloco A.
    batidas_dias = list(
        batidas_apuraveis(None, limite(INI_DIA))
        .filter(
            timestamp__lt=limite(FIM_DIA_EXCLUSIVO),
            colaborador_id__in=ids,
            colaborador__empresa_id__in=EMPRESAS,
        )
        .order_by("colaborador_id", "timestamp", "pk")
    )
    minutos = Counter()
    for b in batidas_dias:
        dt = local(b.timestamp)
        minutos[(b.colaborador_id, dt.date(), dt.strftime("%H:%M"))] += 1

    cels = {
        (c.colaborador_id, c.data): c
        for c in CelulaDia.objects.filter(
            colaborador_id__in=ids,
            colaborador__empresa_id__in=EMPRESAS,
            data__gte=INI_DIA - UM_DIA,
            data__lt=FIM_DIA_EXCLUSIVO,
        ).order_by("colaborador_id", "data")
    }

    resultados = []
    for a, b, segundos in pares:
        ca = cruzar(a, cels, minutos)
        cb = cruzar(b, cels, minutos)
        situacao = resultado_par(ca["orfa_hhmm"], cb["orfa_hhmm"])
        ambiguo = ca["ambiguo"] or cb["ambiguo"]
        resultados.append((a, b, segundos, situacao, ambiguo))
        print("B_PAR faixa={} segundos={} a={} b={} "
              "a_cruzamento={} b_cruzamento={} par_orfa_hhmm={} ambiguo={}".format(
                  faixa(segundos), segundos, registro(a, campos), registro(b, campos),
                  ca, cb, situacao, ambiguo
              ))

    for dia in (date(2026, 11, 10), date(2026, 11, 14)):
        try:
            abrir_caso(dia, campos, cels)
        except Exception as exc:
            erro("caso_col67_" + dia.isoformat(), exc)

    for grupo in FAIXAS + CUMULATIVAS:
        itens = [r for r in resultados if grupo in grupos(r[2])]
        contagens = Counter(r[3] for r in itens)
        pessoas_sim = {r[0].colaborador_id for r in itens if r[3] == "sim"}
        print(
            "RESUMO_5II faixa={} pares={} pessoas={} "
            "par_orfa_hhmm_sim={} pessoas_orfa_hhmm_sim={} "
            "par_orfa_hhmm_nao={} par_orfa_hhmm_indeterminado={} "
            "pares_ambiguos={} criterio=casamento_HH:MM_dia_civil_e_vespera".format(
                grupo, len(itens), len({r[0].colaborador_id for r in itens}),
                contagens["sim"], len(pessoas_sim), contagens["nao"],
                contagens["indeterminado"], sum(r[4] for r in itens)
            )
        )


try:
    executar()
except Exception as exc:
    erro("execucao", exc)
finally:
    print("FIM_BATIDA_DEDUP_5II; ERROS={}".format(ERROS))
```

## 18/11 19:29 · sessao 7c4b518c2063 · RAIA-ESPURIAS · sonda · origem=Code

**Contagem: 14 plantadas distintas, 13 pessoas distintas e 14 pares.** Sim, a contagem impressa bate numericamente com “14 plantadas / 13 pessoas”: `col996` aparece em duas plantadas; as outras 12 pessoas aparecem uma vez cada. O texto original do aval não foi fornecido para conferência independente. [não verificado: arquivo e linha do envelope e do aval não fornecidos]

Os dados da tabela abaixo são transcritos do envelope fornecido no prompt; **não há arquivo:linha fornecido para verificá-los independentemente**. Em todos os casos, a classe atual do lote é `b_conversa`, há zero colisões de tipo oposto na janela de 300 segundos, e a real foi criada antes da validação. O valor na coluna “conflito” é apenas `conflito_no_detalhe`; a classe do lote e esse campo não são o julgamento de dono L-099. A implementação de `classe_do_lote` tem decisões próprias sobre resposta, marco e conflito ([validacao.py:1177](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:1177)). [não verificado: arquivo:linha do envelope]

| Caso | Plantada: ID, instante, tipo | Real: ID, instante | Distância | Real criada vs. declaração / validação | Conflito atual | Dono |
|---|---|---|---:|---|---|---|
| col40 | 139931 · 23/10 12:00 · S | 135364 · 23/10 13:37:31 | 97,52 min | antes / antes | sim | sem dono provado |
| col60 | 152873 · 03/11 11:00 · S | 144882 · 03/11 13:19:17 | 139,30 min | antes / antes | não | sem dono provado |
| col200 | 146569 · 03/11 02:00 · S | 144414 · 03/11 03:02:17 | 62,29 min | antes / antes | sim | sem dono provado |
| col263 | 146777 · 31/10 14:00 · S | 142735 · 31/10 16:16:40 | 136,67 min | antes / antes | não | sem dono provado |
| col301 | 151816 · 08/11 23:00 · S | 149559 · 08/11 20:00:06 | 179,89 min | antes / antes | sim | sem dono provado |
| col444 | 140817 · 28/10 16:00 · E | 139776 · 28/10 13:01:36 | 178,39 min | antes / antes | sim | sem dono provado |
| col446 | 144666 · 25/10 18:00 · S | 136990 · 25/10 15:57:29 | 122,50 min | antes / antes | sim | sem dono provado |
| col549 | 151894 · 10/11 17:20 · S | 151065 · 10/11 14:20:02 | 179,97 min | antes / antes | sim | sem dono provado |
| col727 | 140816 · 25/10 14:00 · S | 136998 · 25/10 16:01:28 | 121,47 min | antes / antes | sim | sem dono provado |
| col881 | 154095 · 13/11 12:00 · S | 154081 · 13/11 13:19:50 | 79,84 min | **depois / antes** | sim | sem dono provado |
| col899 | 151735 · 24/10 10:00 · E | 136112 · 24/10 11:17:42 | 77,70 min | antes / antes | não | sem dono provado |
| col909 | 149856 · 07/11 12:00 · S | 148650 · 07/11 14:59:24 | 179,41 min | antes / antes | sim | sem dono provado |
| col996 | 153862 · 29/10 15:50 · S | 140804 · 29/10 13:17:11 | 152,81 min | antes / antes | sim | sem dono provado |
| col996 | 153847 · 04/11 15:52 · S | 146242 · 04/11 12:54:21 | 177,65 min | antes / antes | sim | sem dono provado |

O motivo da classificação é comum aos 14: o envelope não traz o julgamento L-099 nem todos os seus insumos por dia. O juiz considera, entre outros, vínculo e folga, batida fora de turno, paridade, fase e cadastro versus realidade; a presença de um par próximo ou o conflito atual, isoladamente, não determina seu resultado ([e6_oraculo.py:53](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/e6_oraculo.py:53), [e6_oraculo.py:98](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/e6_oraculo.py:98)). A ausência desses insumos em cada caso é observação sobre o envelope do prompt. [não verificado: arquivo:linha do envelope]

**col881:** a declaração foi respondida às 13:19:38; a real foi criada às 13:19:50, **12 segundos depois da declaração**, mas antes da validação às 13:48:40. Assim, ela integra os 14 pares com real criada antes da validação, embora não integre os 13 com real criada antes da declaração. A guarda histórica e o dono seguem sem prova. [não verificado: arquivo:linha do envelope]

**RESUMO_5I — pares**, conforme impresso no envelope; a faixa compara a distância da plantada à real e a ordem compara a criação da real à validação. [não verificado: arquivo:linha do envelope]

| Criação da real vs. validação | Até 60 min | Fora de 60 min |
|---|---:|---:|
| Antes | 0 | 14 |
| Depois | 0 | 0 |
| Mesmo instante | 0 | 0 |
| Desconhecida | 0 | 0 |

**Erros e limites:** o envelope declara `ERROS=0` e `rc_sombra=0`; isso informa o resultado declarado da execução, sem reconstruir a guarda histórica. A própria saída marca a guarda histórica e a regra ou corte por caso como não verificados e informa que `registrar_batida` não foi chamado. O código da sonda, a execução, o acesso efetivo à sombra e a condição de ter rodado uma única vez em um arquivo **não foram verificados independentemente**. [não verificado: arquivo:linha da sonda e dos avais não fornecidos]

## 18/11 19:21 · sessao 4bcf8eb5e0d7 · RAIA-ESPURIAS · leitura · origem=Code

A guarda de **60 minutos é da classificação para validação em lote**: ela procura qualquer batida apurável do colaborador, de qualquer tipo, em ±60 minutos do instante do slot derivado para o plantio. Só chega a essa consulta depois dos demais filtros da classe A; uma ocorrência leva à classe B, que fica fora do lote. A validação individual não é barrada por essa condição de lote. A consulta usa o instante do slot, que em intervalo fixo pode diferir da hora declarada. [app/chamados/services/validacao.py:1067] [app/chamados/services/validacao.py:1133] [app/chamados/services/validacao.py:1177] [app/chamados/services/materializacao.py:687]

Antes de materializar, também existem estas verificações:

- **Reuso em ±20 minutos:** procura batida do mesmo tipo em torno do instante de plantio. A consulta usa `Batida.objects` sem excluir retratadas; se encontrar, o materializador reutiliza a batida. [app/chamados/juizes.py:239] [app/chamados/services/materializacao.py:1372]
- **Paridade:** procura a vizinha cronológica imediata anterior ou posterior, apurável, em até 12 horas; se ela tiver o mesmo tipo do plantio, pode impedir a materialização. Há exceção quando o plantio preenche um furo posicional e tratamento do par de intervalo. [app/chamados/services/materializacao.py:95] [app/chamados/services/materializacao.py:404] [app/chamados/services/materializacao.py:1386]
- **Porta de registro:** deduplica mesmo tipo em ±60 **segundos**, verifica a mesma chave de instante e consulta colisão de tipo oposto em ±60 ou ±300 segundos, com exceção para marco próprio. A materialização chama essa porta antes de criar a batida. [app/ponto/registro_batida.py:21] [app/ponto/registro_batida.py:35] [app/ponto/registro_batida.py:94] [app/ponto/registro_batida.py:118] [app/chamados/services/materializacao.py:1487]
- **DECLARADO-X-GRAVADO:** compara hora local e tipo declarados com hora e tipo que seriam gravados, não com uma batida real; a espera é limitada ao universo congelado de 163 perguntas de intervalo. [app/chamados/services/materializacao.py:709] [app/chamados/services/materializacao.py:724] [app/chamados/services/materializacao.py:736] [app/chamados/services/materializacao.py:748]

O script abaixo conta **plantadas distintas**, pessoas distintas e pares plantada–real separadamente. O cruzamento é por batida `app` apurável, mesmo colaborador e tipo, em ±3 horas do instante plantado; ele inclui reais criadas depois da validação, pois a ordem é justamente uma das saídas pedidas. O resumo é por **par**, para não esconder casos com mais de uma real. Empates e instantes ausentes ficam separados. O critério de origem `app` reproduz o raio-x fornecido; a porta real de apuração exclui retratadas. [não verificado] [app/ponto/turnos.py:2868]

A resposta atual de `classe_do_lote` é impressa sem seu detalhe textual: a própria plantada já existe e pode entrar na consulta, portanto essa chamada **não reconstrói a decisão histórica**. O mesmo vale para as outras consultas ao estado atual. A porta `registrar_batida` pode criar registros e acionar fechamento; não é chamada. [app/chamados/services/validacao.py:1204] [app/ponto/registro_batida.py:170]

```python
import datetime as dt
import re
from collections import Counter

ERROS = 0


def erro(etapa, exc):
    global ERROS
    ERROS += 1
    # A mensagem livre de uma excecao pode conter dados pessoais.
    print("ERRO etapa=%s tipo=%s mensagem=[suprimida por privacidade]" %
          (etapa, type(exc).__name__))


def local(instante):
    if instante is None:
        return "[nao verificado]"
    return timezone.localtime(instante).strftime("%d/%m/%Y %H:%M:%S")


def dia(data):
    return data.strftime("%d/%m/%Y") if data else "[nao verificado]"


def ordem(a, b):
    if a is None or b is None:
        return "desconhecida"
    if a < b:
        return "antes"
    if a > b:
        return "depois"
    return "mesmo_instante"


def resposta_segura(valor):
    # Os primeiros 20 caracteres so sao exibidos quando consistem
    # exclusivamente em uma hora; texto livre pode conter nome ou CPF.
    trecho = (valor or "")[:20]
    if re.fullmatch(r"\s*\d{1,2}:\d{2}(?::\d{2})?\s*", trecho):
        return repr(trecho)
    return "[suprimida por privacidade]"


def main():
    from django.apps import apps
    from django.utils import timezone as django_timezone
    from ponto.models import Batida
    from ponto.turnos import batidas_apuraveis
    from chamados.services.validacao import classe_do_lote
    from ponto.registro_batida import colisoes_tipo_oposto

    global timezone
    timezone = django_timezone

    # Inclui os modelos lidos indiretamente pelas funcoes reais.
    for modelo in sorted(apps.get_models(), key=lambda m: m._meta.label):
        print("CAMPOS %s %s" %
              (modelo._meta.label, [f.name for f in modelo._meta.get_fields()]))

    tz = timezone.get_current_timezone()
    ini = timezone.make_aware(dt.datetime(2026, 10, 21, 0, 0), tz)
    fim_exclusivo = timezone.make_aware(dt.datetime(2026, 11, 21, 0, 0), tz)
    tres_horas = dt.timedelta(hours=3)

    print("CRITERIO plantadas=origem_disputa_s84_retro_apuravel "
          "timestamp_21_10_a_20_11_inclusive empresas=2,3,4")
    print("CRITERIO real=app_apuravel_mesmo_colaborador_mesmo_tipo "
          "distancia_absoluta_timestamp_ate_180_min_inclusive")
    print("CRITERIO ordem=real_criado_em_comparado_a_respondida_em_e_validada_em")
    print("CRITERIO resumo=pares; plantadas_e_pessoas_sao_distintas")
    print("AVAL 14_plantadas_13_pessoas=[nao verificado]")

    plantadas = (
        batidas_apuraveis(None, ini, fim_exclusivo)
        .filter(
            timestamp__lt=fim_exclusivo,
            colaborador__empresa_id__in=[2, 3, 4],
            origem="disputa_s84_retro",
        )
        .select_related("colaborador", "pergunta_origem")
        .order_by("colaborador_id", "timestamp", "pk")
    )

    ids_plantadas = set()
    pessoas = set()
    pares = 0
    resumo = Counter()

    for plantada in plantadas.iterator():
        try:
            reais = list(
                batidas_apuraveis(
                    plantada.colaborador,
                    plantada.timestamp - tres_horas,
                    plantada.timestamp + tres_horas,
                )
                .filter(origem="app", tipo=plantada.tipo)
                .order_by("timestamp", "pk")
            )
            if not reais:
                continue

            ids_plantadas.add(plantada.pk)
            pessoas.add(plantada.colaborador_id)
            pergunta = plantada.pergunta_origem

            if pergunta is None:
                classe = "[nao chamada: sem pergunta]"
                conflito = "[nao verificado]"
            else:
                try:
                    classe, detalhe = classe_do_lote(pergunta)
                    conflito = str(bool(detalhe.get("conflito", False)))
                except Exception as exc:
                    erro("classe_do_lote_pergunta_%s" % pergunta.pk, exc)
                    classe = "[nao verificado]"
                    conflito = "[nao verificado]"

            try:
                janela, colidem = colisoes_tipo_oposto(
                    plantada.colaborador, plantada.timestamp, plantada.tipo
                )
                colisao_atual = "janela_s=%d quantidade=%d" % (
                    int(janela.total_seconds()), len(colidem)
                )
            except Exception as exc:
                erro("colisoes_tipo_oposto_plantada_%s" % plantada.pk, exc)
                colisao_atual = "[nao verificado]"

            for real in reais:
                pares += 1
                distancia = abs(
                    (real.timestamp - plantada.timestamp).total_seconds()
                ) / 60.0
                ordem_resposta = ordem(
                    real.criado_em,
                    pergunta.respondida_em if pergunta else None,
                )
                ordem_validacao = ordem(
                    real.criado_em,
                    pergunta.validada_em if pergunta else None,
                )
                faixa = "dentro_60" if distancia <= 60 else "fora_60"
                resumo[(ordem_validacao, faixa)] += 1

                print(
                    "CASO col%d plantada_id=%s dia_turno=%s "
                    "plantada=%s tipo=%s plantada_criada_em=%s"
                    % (
                        plantada.colaborador_id,
                        plantada.pk,
                        dia(pergunta.data_evento if pergunta else None),
                        local(plantada.timestamp),
                        plantada.tipo,
                        local(plantada.criado_em),
                    )
                )
                print(
                    "  REAL id=%s instante=%s tipo=%s origem=%s criado_em=%s "
                    "distancia_min=%.2f"
                    % (
                        real.pk,
                        local(real.timestamp),
                        real.tipo,
                        real.origem,
                        local(real.criado_em),
                        distancia,
                    )
                )
                print(
                    "  PERGUNTA id=%s motivo=%s resposta_20=%s "
                    "respondida=%s validada=%s materializada=%s"
                    % (
                        pergunta.pk if pergunta else "[nao verificado]",
                        pergunta.motivo if pergunta else "[nao verificado]",
                        resposta_segura(pergunta.resposta_colab)
                        if pergunta else "[nao verificado]",
                        local(pergunta.respondida_em if pergunta else None),
                        local(pergunta.validada_em if pergunta else None),
                        local(pergunta.materializada_em if pergunta else None),
                    )
                )
                print(
                    "  ORDEM real_criado_%s_declaracao "
                    "real_criado_%s_validacao"
                    % (ordem_resposta, ordem_validacao)
                )
                print(
                    "  GUARDA_ATUAL classe_do_lote=%s conflito_no_detalhe=%s "
                    "colisoes_tipo_oposto=%s"
                    % (classe, conflito, colisao_atual)
                )
                print(
                    "  GUARDA_HISTORICA=[nao verificado] "
                    "REGRA_L_NNN_OU_CORTE_POR_CASO=[nao verificado] "
                    "registrar_batida=[nao chamada: pode gravar]"
                )
        except Exception as exc:
            erro("plantada_%s" % plantada.pk, exc)

    print(
        "CONTAGEM plantadas=%d pessoas=%d pares=%d "
        "aval_plantadas=14 aval_pessoas=13"
        % (len(ids_plantadas), len(pessoas), pares)
    )
    for posicao in ("antes", "depois", "mesmo_instante", "desconhecida"):
        for faixa in ("dentro_60", "fora_60"):
            print(
                "RESUMO_5I real_%s_validacao %s pares=%d"
                % (posicao, faixa, resumo[(posicao, faixa)])
            )


try:
    main()
except Exception as exc:
    erro("execucao_principal", exc)
finally:
    print("ERROS=%d" % ERROS)
    print("FIM_DECLARADO_X_GRAVADO_5I")
```

Funções reais chamadas diretamente pela sonda:

- `batidas_apuraveis` — [app/ponto/turnos.py:2868](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:2868).
- `classe_do_lote` — [app/chamados/services/validacao.py:1177](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/validacao.py:1177).
- `colisoes_tipo_oposto` — [app/ponto/registro_batida.py:94](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/registro_batida.py:94).

**Limites:** a contagem efetiva, o resultado por caso, a guarda efetivamente acionada em cada validação e a regra aplicável a cada caso ficam [não verificado]. A resposta livre é suprimida quando os primeiros 20 caracteres não são exclusivamente uma hora, para cumprir a restrição de não imprimir nomes ou CPF; mensagens livres de exceção também são suprimidas. [não verificado]

A sonda foi entregue como texto e não foi executada. Há uma incompatibilidade operacional a resolver antes de rodá-la sob a regra estrita de não criar nem alterar arquivos: o envelope exigido reescreve credencial, cria arquivo temporário e acrescenta uma trilha, além de poder alterar roles, grants e rede. [bin/sonda_leitura.sh:115](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/bin/sonda_leitura.sh:115) [bin/sonda_leitura.sh:153](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/bin/sonda_leitura.sh:153) [bin/sonda_leitura.sh:185](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/bin/sonda_leitura.sh:185) [bin/sonda_leitura.sh:216](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/bin/sonda_leitura.sh:216)

## 18/11 19:20 · sessao 444492dc2cb6 · RAIA-ESPURIAS · leitura · origem=Code

A cópia tem uma guarda de colisão para tipos opostos na porta de registro, mas **não consigo entregar uma sonda completa e confiável para a contagem de órfãs por batida**: a grade devolve as órfãs como horas `HH:MM`, sem data, segundos ou identificador da batida. Os dois registros de col67 às 23:59 e os dois às 00:01, por exemplo, ficam indistinguíveis por essa saída. A sonda `sonda_espuria2`, necessária para conferir como ela resolve essa associação, não foi encontrada na cópia. [app/escala/utils.py:631](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/utils.py:631), [app/escala/utils.py:673](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/utils.py:673), [não verificado]

- **Porta de registro:** para o mesmo tipo, reutiliza uma batida apurável em janela inclusiva de ±60 s. Para tipo oposto, procura colisões em ±300 s quando há escala vigente e marcos no dia local da nova batida; caso contrário, em ±60 s. A colisão não é aplicada se a nova batida casar, em até 300 s, com marco próprio do seu tipo no dia ou na véspera. Sem autoridade humana, a porta recusa a criação; com autoridade humana, cria e devolve conflito para revisão. No mesmo instante exato, antes dessa guarda, devolve a batida já existente mesmo que seja de tipo oposto ou retratada. [app/ponto/registro_batida.py:21](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/registro_batida.py:21), [app/ponto/registro_batida.py:35](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/registro_batida.py:35), [app/ponto/registro_batida.py:94](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/registro_batida.py:94), [app/ponto/registro_batida.py:130](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/registro_batida.py:130)

- **API do app:** antes da porta, reutiliza batida apurável do **mesmo tipo** em ±300 s do instante efetivo; essa consulta não deduplica tipo oposto. Depois chama a porta com `autoridade_humana=False`; quando ela retorna sem batida, procura a oposta e responde `ignorada_intervalo`. Portanto, para tipos opostos, vale a guarda da porta e suas exceções, não uma segunda janela independente da API. [app/api/views_core.py:492](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/api/views_core.py:492), [app/api/views_core.py:513](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/api/views_core.py:513), [app/api/views_core.py:643](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/api/views_core.py:643), [app/api/views_core.py:660](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/api/views_core.py:660)

- **Pareador:** não apaga nem deduplica as batidas. No caminho de fechamento de turno, recusa formar o par entrada–saída de duração **≤60 s**, carimba a recusa e mantém a saída separada; o tratamento depende também dos papéis e marcos fornecidos ao pareador. Acima de 60 s, essa guarda específica não recusa o par. [app/ponto/turnos.py:117](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:117), [app/ponto/turnos.py:1192](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1192), [app/ponto/turnos.py:1482](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1482), [app/ponto/management/commands/detectar_par_relampago.py:49](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/detectar_par_relampago.py:49)

- **Grade e cartório:** a pré-passagem da grade descarta da representação batidas consecutivas do **mesmo tipo** com distância inferior a 600 s; não descarta ali um par de tipos opostos. O cartório alimenta a grade com batidas apuráveis e copia as horas de `dia['orfas']` para a ata; isso não estabelece uma deduplicação de tipo oposto pelo cartório. [app/escala/utils.py:1333](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/utils.py:1333), [app/ponto/services/cartorio.py:1156](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:1156), [app/ponto/services/cartorio.py:730](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:730)

O modelo `Batida` desta cópia tem, entre outros campos, `terminal`, `canal` e `timestamp_dispositivo`; não vi nele campo de versão do app. O preenchimento desses campos na sombra e a existência de uma versão associável por outra fonte ficam **[não verificado]**. [app/ponto/models.py:8](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/models.py:8), [app/ponto/models.py:27](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/models.py:27), [app/ponto/models.py:45](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/models.py:45), [app/ponto/models.py:75](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/models.py:75)

Há também uma restrição operacional concreta: **o envelope exigido não é somente leitura no sistema de arquivos**. Ele reescreve `logs/.env_leitor`, pode alterar role e grants, e acrescenta uma trilha ao log. Por isso, não o executei sob a regra de não criar, editar ou apagar arquivos. [bin/sonda_leitura.sh:115](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/bin/sonda_leitura.sh:115), [bin/sonda_leitura.sh:140](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/bin/sonda_leitura.sh:140), [bin/sonda_leitura.sh:146](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/bin/sonda_leitura.sh:146), [bin/sonda_leitura.sh:245](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/bin/sonda_leitura.sh:245)

No dado fornecido, os pares de col67 têm distância de 2 s: **14/11 23:59:12 E → 23:59:14 S** e **11/11 00:01:42 E → 00:01:44 S**, este último apresentado na grade do dia de turno 10/11. O anexo atribui uma órfã `E` em cada caso, mas não mostra `criado_em` nem campos de aparelho; esses valores e o resultado atual da sombra permanecem **[não verificado]**. A razão de os pares históricos terem chegado ao banco também é **[não verificado]**.

Não apresento um script como completo: faltam a porta comprovada para associar órfã a batida exata e a validação da sonda no ambiente de destino. Assim, **RESUMO_5II, contagens da frota e ERROS=0 ficam [não verificado]**.

## 18/11 19:19 · sessao 526d0726e133 · RAIA-ESPURIAS · leitura · origem=Code

**Os anexos não identificam seis classes que somem o quadro inteiro.** [auditoria_espurias_fecha_1811_1920.txt:5; demais números de linha dos anexos: não verificados]

A partição mais próxima mostrada é **classe × tipo de dia** em `espuria_1711.out`. Ela tem cinco combinações e soma as **1.265 órfãs** da medição das 15:34; os totais abaixo são somas das linhas do anexo. [espuria_1711.out:3; espuria_1711.out:5; linhas dos agregados: não verificadas]

| Classe × tipo de dia | Órfãs | Linha do anexo que mostra o rótulo |
|---|---:|---|
| `folga_desmentida × folga` | 624 | `espuria_1711.out:7` |
| `folga_desmentida × ausencia` | 6 | `espuria_1711.out:30` |
| `geometria × trabalho` | 525 | `espuria_1711.out:8` |
| `indefinida × trabalho` | 108 | `espuria_1711.out:14` |
| `eco_cluster × trabalho` | 2 | `espuria_1711.out:32` |
| **Soma** | **1.265** | `espuria_1711.out:5` |

As linhas citadas mostram os rótulos, mas **não definem seis classes de dia-colab**; a classificação disponível na cópia retorna classe por horário de órfã e pode atribuir `geometria` e `eco_cluster` ao mesmo dia. [app/escala/orfa_classe.py:53; app/escala/orfa_classe.py:73]

O raio-x verde apresenta seis rótulos de **motivo da órfã**, com contagens 71, 14, 13, 3, 2 e 2, que somam 105. Eles se referem aos 83 dias e 50 pessoas do raio-x, e não constituem uma partição demonstrada das 1.265 órfãs. [raiox_verde_1811.out, linhas `RESUMO` e `CLASSE`: não verificadas] [nao verificado]

**Não consigo entregar honestamente o script completo solicitado com estes insumos.** O `sonda_espuria2.py` transcrito seleciona `CelulaDia` por `ata__n_orfas__gt=0` e imprime um vetor `sem_trabalho/cxr/regen`; a saída `espuria_1711.out` apresenta outro quadro, por `classe/tipo_dia/distância`, e o texto transcrito não mostra a chamada ao classificador que gerou esse quadro. [sonda_espuria2.py, trechos `todas.filter`, `chave` e `QUADRO`: linhas não verificadas; app/escala/orfa_classe.py:53] Não encontrei os arquivos dos anexos na cópia para resolver essa discrepância. [não verificado]

Além disso, a medida posterior registra **1.267 órfãs**, duas a mais que as 1.265 das 15:34; a causa da diferença e a composição histórica exata não são demonstradas pelos dados fornecidos. [cruza_1711.out, linha `CRUZA`: não verificada; espuria_1711.out:5]

Para escrever a sonda que satisfaça o pedido sem inventar precedência entre classes, falta a **sonda efetivamente usada para gerar `espuria_1711.out`** e uma regra autorizada para atribuir um dia com órfãs de classes diferentes a uma única classe de dia-colab. A identidade histórica do universo verde também depende dos dados da execução anterior, não apenas do critério executado sobre o estado atual. [app/escala/orfa_classe.py:73; raiox_verde.py, filtro e período: linhas não verificadas] [nao verificado]

Nenhuma função de produção foi executada nesta leitura; efeito colateral transitivo das funções necessárias e resultado no tenant permanecem **[não verificado]**.

## 18/11 19:16 · sessao 33520c760eaf · RAZAO-DA-SOBRA · censo · origem=Code

O anexo contém **49 expressões de leitura da chave de órfãs da ata ou da grade, em 29 funções e 24 arquivos**, contando cada nó sintático e incluindo os acessos usados para acrescentar ou ordenar a própria lista. As **14 expressões em 10 arquivos** da resposta anterior são um subconjunto: o recorte ali declarado era a **ata direta**, enquanto o pedido atual inclui também a grade. A identidade da cópia com o hash informado é **[não verificado]**: `git rev-parse HEAD` retornou que a cópia não é um repositório Git.

A unidade de contagem importa: `get` e indexação contam separadamente; uma expressão `['orfas'].append(...)` também aparece como leitura AST porque obtém a lista para mutá-la. Isso não significa que cada expressão seja um consumidor independente ([escala/utils.py:810](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/utils.py:810)).

### Separação das 88 linhas do anexo

| Classe | Linhas | Critério |
|---|---:|---|
| Leituras literais de `orfas`/`n_orfas` da ata ou grade em código fora de testes e snapshots | **49** | Inclui comandos e funções construtoras; execução efetiva em produção não é demonstrada pelo anexo. | [nao verificado]
| Homônimos | **22** | Não são a lista ou contagem de batidas órfãs da ata/grade. |
| Teste | **1** | [app/tenants/tests.py:60](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/tenants/tests.py:60), pista de chamado órfão. |
| Snapshot excluído | **1** | Lê a ata, mas está no diretório explicitamente excluído: [bin/snapshot_o118_col221.py:32](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/bin/snapshot_o118_col221.py:32). |
| Escritas na chave da grade, sem expressão de leitura naquele nó | **8** | [escala/utils.py:814](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/utils.py:814), `973`, `977`, `1164`, `1229`, `1264`, `1269`, `1849`. |
| Pistas relacionadas às órfãs da ata/grade, mas que não são por si leituras literais | **7** | Definições de funções, identificadas abaixo. |
| **Total** | **88** | |

Os **22 homônimos** são:

- **Perguntas órfãs: 8 linhas** — sete pistas em [chamados/reconciliador.py:1088](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/reconciliador.py:1088), `1104`, `1157`, `1227`, `1530`, `1554`, `1578`; e `out['orfas']` em [chamados/reconciliador.py:1421](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/reconciliador.py:1421), contador de perguntas encerradas.
- **Chamados ou cobranças órfãos: 8 pistas** — [acoes_disputa.py:214](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/acoes_disputa.py:214), [disputa_emissao.py:705](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/disputa_emissao.py:705), `804`, `1041`, [regeneracao.py:141](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/regeneracao.py:141), `158`, [gerar_celulas.py:232](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/gerar_celulas.py:232) e [reconciliar_grade.py:430](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/reconciliar_grade.py:430).
- **Turno sem par: 3 pistas** — [regularizacao.py:19](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/regularizacao.py:19), [turnos.py:234](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:234) e [utils_turno_orfao.py:9](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/utils_turno_orfao.py:9). O último trata, por exemplo, de entrada sem saída.
- **Outros: 3 linhas** — `c['orfas']` são perguntas fora de juízes em [gerar_diagrama.py:854](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/core/management/commands/gerar_diagrama.py:854); `_sobra` mede dias previstos sem batida em [propositor.py:246](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/propositor.py:246); `_acesas_de_sobra` compara batidas apuráveis com marcos vencidos em [vigia_de_hora.py:64](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/vigia_de_hora.py:64).

As sete pistas relacionadas são [orfa_classe.py:53](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/orfa_classe.py:53), [leitor_celula.py:180](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/leitor_celula.py:180), [tripwire_celulas.py:135](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/tripwire_celulas.py:135) e `199`, [cluster_espurio.py:40](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cluster_espurio.py:40) e `88`, e [cartao_pela_celula.py:277](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/relatorios/cartao_pela_celula.py:277).

### Matriz final

Uma linha abaixo representa **uma função**, com todas as linhas AST pertinentes agrupadas. **PAGA** significa que rastreei a influência até folha ou export; isso não comprova um valor pago. Onde o caminho não foi rastreado, marco **PAGA [não verificado]**, sem presumir ausência.

| Arquivo:linha | Função | Uso e derivação própria |
|---|---|---|
| [chamados/juizes.py:514](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:514), duas expressões | `_celula_furada` | **DECIDE** se a sobra prende o dia; condiciona à confiabilidade da régua. Não classifica cada sobra. PAGA **[não verificado]**. |
| [materializacao.py:449](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:449) | `_plantio_preenche_furo_na_data` | **DECIDE** materialização; filtra horas válidas e as junta às marcas para atribuição. PAGA **[não verificado]**. |
| [calendario.py:409](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/colaboradores/services/calendario.py:409), `410` | `contexto_calendario` | **MOSTRA** horas no calendário; transforma em conjunto. Razão própria: não identificada. PAGA **[não verificado]**. |
| [orfa_classe.py:58](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/orfa_classe.py:58) | `classificar_orfas` | **DECIDE** classe por folga, marco ausente e rajada de até 15 minutos. Chamador de produção dinâmico **[não verificado]**. PAGA **[não verificado]**. |
| [furos_diarios.py:22](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/furos_diarios.py:22) | `_furos_do_dia` | **GRAVA** indiretamente: conta órfãs e a apuração entrega o número à porta que grava `FuroDiario` ([furos_diarios.py:54](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/furos_diarios.py:54), [portas/celula.py:1257](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/portas/celula.py:1257)). PAGA **[não verificado]**. |
| [leitor_celula.py:187](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/leitor_celula.py:187), `190` | `_orfas_da_ata` | **TRANSPORTA; PAGA** no sentido de chegar à folha: lista real ou `?` repetido pela contagem antiga → `grade_da_celula` ([linha 420](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/leitor_celula.py:420)) → `grade_do_fechamento` ([export.py:329](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/folha/export.py:329)) → causa gravada no fechamento ([fechamento.py:1080](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/fechamento.py:1080)). Não classifica a razão. |
| [escala/utils.py:810](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/utils.py:810), `813`, `814` | `montar_grade_prevista` | **GRAVA** na grade em memória: acrescenta batidas não consumidas e ordena. Produz a lista; não classifica motivo por sobra. PAGA **[não verificado]**. |
| [escala/utils.py:1054](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/utils.py:1054) | `_montar_grade_prevista_periodo_legacy` | **TRANSPORTA/GRAVA** a lista na grade unificada. Não deriva razão. PAGA **[não verificado]**. |
| [escala/utils.py:1121](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/utils.py:1121) | `montar_realizado_grade` | **MOSTRA/TRANSPORTA** cópia da lista junto ao realizado. Não classifica. PAGA **[não verificado]**. |
| [escala/utils.py:1843](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/utils.py:1843), `1845`, `1848`, `1849`, `1905`, `1906`, `1979`, `1984` | `montar_grade_prevista_periodo_por_turno` | **DECIDE/GRAVA** grade: acrescenta e ordena, fornece lista ao juiz e verifica órfãs das bordas, inclusive saída no dia seguinte; pode mover borda para marco conforme as guardas ([linha 2007](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/utils.py:2007)). PAGA **[não verificado]**. |
| [folha/causas.py:43](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/folha/causas.py:43), `100` | `classificar_causa` | **DECIDE/GRAVA; PAGA** pelo critério de chegada à folha: filtra dias úteis, conta dias com órfã e identifica trabalho em folga; o fechamento grava `causa_espelho` ([fechamento.py:1077](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/fechamento.py:1077), [1144](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/fechamento.py:1144)). Mudança de rubrica ou valor por essa causa **[não verificado]**. |
| [juiz_batida.py:161](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/juiz_batida.py:161) | `periodos_do_dia` | **MOSTRA/TRANSPORTA** órfãs no resultado; contrato diz que não somam ao período ([linha 18](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/juiz_batida.py:18)). A função é chamada pelo motor e pelo export, mas os trechos examinados usam outros campos, não essa lista ([motor_calculo_v2.py:616](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/motor_calculo_v2.py:616), [export.py:1150](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/folha/export.py:1150)). **PAGA pela lista [não verificado]**. |
| [detectar_par_relampago.py:140](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/detectar_par_relampago.py:140) | `_score_geometria` | **DECIDE/MOSTRA** escore de comando; conta órfãs na grade de um dia. PAGA **[não verificado]**. |
| [propor_reorganizacao.py:104](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/propor_reorganizacao.py:104) | `_run` | **DECIDE/MOSTRA** proposta; valida horas e as passa à atribuição global. PAGA **[não verificado]**. |
| [tripwire_celulas.py:264](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/tripwire_celulas.py:264) | `medir_sobra_com_tipo_trocado` | **DECIDE/MOSTRA** vigilância; filtra sobra na borda por raio. PAGA **[não verificado]**. |
| [ponto/propositor.py:39](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/propositor.py:39) | `dia_degenerado` | **DECIDE** escalada; valida horas, procura cluster em janela e combina órfãs com furo ([linha 52](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/propositor.py:52)). PAGA **[não verificado]**. |
| [bordas_realizado.py:49](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/bordas_realizado.py:49) | `_teve_batida_na_ata` | **DECIDE** presença de batida; considera isenção e contagem de marcos. PAGA **[não verificado]**. |
| [cartorio.py:736](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:736), `742`, `745` | `ata_do_dia` | **GRAVA** ata: passa lista às lâmpadas e grava contagem e horas; contagem própria, sem classe de razão. A ata é lavrada em [cartorio.py:1852](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:1852). Caminho financeiro completo a partir desta entrada **[não verificado]**. |
| [cartorio.py:839](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:839) | `insumos_da_ata` | **DECIDE/GRAVA** indiretamente entrada e saída desconhecidas quando não há lâmpadas e há contagem; o cartório carimba os insumos ([linha 1873](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:1873)). PAGA **[não verificado]**. |
| [cartorio.py:1426](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:1426) | `_julgar_colab_corpo` | **DECIDE/GRAVA** julgamento; soma quantidade de órfãs às batidas reconhecidas e lavra resultado. Efeito financeiro dessa contagem **[não verificado]**. |
| [cluster_espurio.py:29](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cluster_espurio.py:29), `31` | `_leitura_da_celula` | **DECIDE** assinatura: exige lista e lâmpadas, conta faltantes e procura duas órfãs consecutivas a até 15 minutos. PAGA **[não verificado]**. |
| [esmeril_espelho.py:125](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/esmeril_espelho.py:125) | `assinaturas_do_colab` | **DECIDE/MOSTRA** assinatura recorrente; agrupa horas em faixas de 30 minutos. PAGA **[não verificado]**. |
| [simular_dia.py:12](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/simular_dia.py:12) | `dia_perfeito` | **DECIDE** que dia com órfã não é perfeito. Não classifica a sobra. PAGA **[não verificado]**. |
| [vigia_pareador_celula.py:74](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/vigia_pareador_celula.py:74), duas expressões | `censo` | **DECIDE/MOSTRA** divergência; só monta conjunto quando há contagem e recorta janela por empresa. PAGA **[não verificado]**. |
| [status_dia.py:188](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/status_dia.py:188) | `fatos_do_status` | **DECIDE/GRAVA** indiretamente status: soma a quantidade às batidas para `tem_batida`; cartório passa os fatos à lavra ([cartorio.py:1858](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:1858)). PAGA **[não verificado]**. |
| [supra_juiz.py:392](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/supra_juiz.py:392), `527` duas expressões | `classificar_dia` | **DECIDE/GRAVA; PAGA**: usa presença e conteúdo, podendo emitir `BATIDA_ORFA_FORA_TOLERANCIA` com contagem e horas; cartório lavra códigos ([cartorio.py:1464](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:1464), [1855](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:1855)); export examina veredito e protestos para retenção ([export.py:1273](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/folha/export.py:1273)). Efeito em caso concreto **[não verificado]**. |
| [turnos.py:2679](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:2679) | `horas_reais_do_dia` | **DECIDE/TRANSPORTA** horas convertidas em minutos; alimenta a decisão de período de ausência parcial ([escala/utils.py:1245](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/utils.py:1245)). Caminho até valor ou retenção **[não verificado]**. |
| [cartao_pela_celula.py:288](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/relatorios/cartao_pela_celula.py:288) | `orfas_e_isencao_da_ata` | **MOSTRA/TRANSPORTA** conjunto de horas por data; chamador de produção **[não verificado]**. PAGA **[não verificado]**. |
| [pdf_espelho.py:477](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/relatorios/pdf_espelho.py:477), `552` | `_coletar_dados_espelho` | **MOSTRA** conjuntos de horas e por data no espelho/PDF. Não identifica razão individual. PAGA **[não verificado]**. |

### Confronto com as 14 leituras anteriores

As 14 expressões da tabela anterior continuam presentes, sem diferença de endereço ou quantidade por função:

| Linha da resposta anterior | Expressões conferidas | Diferença |
|---|---|---|
| `_celula_furada` | [514](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:514): 2 | Sem mudança. |
| `_plantio_preenche_furo_na_data` | [449](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:449): 1 | Sem mudança. |
| `_orfas_da_ata` | [187](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/leitor_celula.py:187), `190`: 2 | Acrescentado caminho rastreado até causa no fechamento. |
| `periodos_do_dia` | [161](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/juiz_batida.py:161): 1 | Precisão: ser chamado pelo motor/export não prova uso financeiro da lista de órfãs. |
| `medir_sobra_com_tipo_trocado` | [264](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/tripwire_celulas.py:264): 1 | Sem mudança. |
| `insumos_da_ata` | [839](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:839): 1 | Acrescentada a gravação indireta dos insumos. |
| `_leitura_da_celula` | [29](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cluster_espurio.py:29), `31`: 2 | Sem mudança; explicitado o limite de 15 minutos. |
| `assinaturas_do_colab` | [125](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/esmeril_espelho.py:125): 1 | Sem mudança; explicitadas faixas de 30 minutos. |
| `censo` | [74](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/vigia_pareador_celula.py:74): 2 | Sem mudança. |
| `orfas_e_isencao_da_ata` | [288](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/relatorios/cartao_pela_celula.py:288): 1 | A função lê para possível exibição; chamador de produção **[não verificado]**. |

A diferença numérica é **49 − 14 = 35 expressões adicionais**, todas discriminadas na matriz: são leituras da grade ou de sua construção e transporte que não pertenciam ao recorte anterior de ata direta. A busca é a do anexo e das expressões literais examinadas; aliases, acesso dinâmico e caminhos não percorridos permanecem **[não verificado]**.

**Leitores que PAGAM: 3 caminhos comprovados nesta leitura**, contando funções, sob o critério pedido de chegada à folha/export: `_orfas_da_ata`, `classificar_causa` e `classificar_dia`. O total real entre os 29 leitores é **[não verificado]**; os demais caminhos marcados como não verificados podem aumentar esse número. Nenhum dado fornecido prova pagamento efetivo ou valor pago.

## 18/11 19:14 · sessao 4dce2fc3f869 · E4C-F9 · construtor · origem=Code

Implementei as sete trocas para leitura pelo ato e acrescentei testes de divergência e um selo AST. **RED, GREEN e mutantes não foram executados**, conforme a instrução de não rodar banco nem suíte; seus resultados são **[não verificado]**. A análise sintática dos quatro arquivos passou, assim como `git diff --check` **[não verificado pelo Code]**. [nao verificado]

As alterações estão em:

- [auditar_invariantes_chamados.py:206](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/auditar_invariantes_chamados.py:206): M1 lê o rótulo pela sequência; M2–M5 usam a face SQL em [I21](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/auditar_invariantes_chamados.py:1586), [C2](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/auditar_invariantes_chamados.py:1617), [F2](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/auditar_invariantes_chamados.py:1781) e [F3](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/auditar_invariantes_chamados.py:1794). I18 e I20 receberam comentários de exceção em [I18](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/auditar_invariantes_chamados.py:1081) e [I20](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/auditar_invariantes_chamados.py:1113).
- [encerrar_perguntas_com_fato.py:33](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/encerrar_perguntas_com_fato.py:33): M6 seleciona perguntas materializadas e não validadas **pelo ato**. Com `--apply`, uma validação desfeita no ato deixa de excluir a pergunta por causa de um carimbo residual; uma validação vigente no ato passa a excluí-la mesmo com a coluna vazia. A escrita continua sujeita aos demais critérios e portas do comando ([encerrar_perguntas_com_fato.py:38](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/encerrar_perguntas_com_fato.py:38)).
- [validar_classe_a.py:80](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/management/commands/validar_classe_a.py:80): M7 imprime a resposta lida do ato.
- [test_e4c_f9_comandos_leem_o_ato.py:60](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:60): cria a divergência por `registrar` e `.update()`, inclui controles e selo AST. **Limite:** M2–M5 testam o predicado de cada sítio extraído da AST, isolado dos demais critérios; não exercitam o auditor inteiro ([teste:23](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:23)). [nao verificado]

Não editei `backfill_decisoes_pergunta.py` nem `curar_perguntas_em_limbo.py`: a solicitação de comentários nesses arquivos conflita com a proibição de editar fora dos três comandos e testes. Os sítios remanescentes constam do censo fornecido **[não verificado além do anexo do pedido]**. [nao verificado]

Para o Code, o comando do módulo — tanto para observar o **RED na base, com o novo teste presente**, quanto para o GREEN com a cura — é: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 bin/suite.sh --dir /home/ronald/wt-codex-e4c-f9-191111 --only "chamados.tests.test_e4c_f9_comandos_leem_o_ato"
```

A aceitação de módulo em `--only` é **[não verificado]**. O recorte explicitamente solicitado é:

```bash
PYTHONDONTWRITEBYTECODE=1 bin/suite.sh --dir /home/ronald/wt-codex-e4c-f9-191111 --only "chamados"
```

Para os mutantes, aplicar **um por vez**, executar o comando do módulo acima e restaurar a cura antes do seguinte. As mortes esperadas são **[não verificado]**:

| Mutante | Substituição | Caso que deve falhar |
|---|---|---|
| M1 | `DecisaoPergunta.leitura(_p).validada_em` → `_p.validada_em` | `test_M1_rotulo_e_controle` |
| M2, M3, M4, M5 | No sítio correspondente, `PD.q_nao_validada_pelo_ato()` → `validada_em__isnull=True` | `test_M2_M3_M4_M5_filtros_de_cada_sitio_e_controle`, subteste do sítio |
| M6 | `PerguntaDisputa.q_nao_validada_pelo_ato()` → `validada_em__isnull=True` | `test_M6_dry_run_universo_e_controle` |
| M7 | `DecisaoPergunta.leitura(q).resposta_colab` → `q.resposta_colab` | `test_M7_texto_do_ato_e_controle` |

Os escritores identificados na cópia incluem validação e devolução em [validacao.py:168](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/services/validacao.py:168) e [validacao.py:273](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/services/validacao.py:273), resposta humana e reabertura em [disputa_emissao.py:1481](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/services/disputa_emissao.py:1481) e [disputa_emissao.py:1617](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/services/disputa_emissao.py:1617). A enumeração completa de escritores é **[não verificado]**. Os M passam a consultar [q_nao_validada_pelo_ato](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:3564) e [DecisaoPergunta.leitura](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/models.py:4295); essa leitura também incorpora o passivo E4b descrito no próprio método.

**LEI-AKITA:** origem = os três comandos acima; testemunha = `DecisaoPergunta.leitura` / `q_nao_validada_pelo_ato`; RED = [teste E4C-F9](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:60) **[execução não verificada]**; quem mais lê = os cinco comandos do censo anexado, incluindo backfill e cura de limbo **[censo fornecido, não refeito]**; juízes novos = 0 **[diff dos três comandos e teste]**; padrão = CQRS com projeções reconstruíveis. [nao verificado]

## 18/11 19:10 · sessao c0153fbf6c21 · RAZAO-DA-SOBRA · leitura · origem=Code

O censo preliminar confirma **14 acessos literais diretos em 10 arquivos** quando a unidade é uma expressão de leitura de `orfas` ou `n_orfas` **da ata**. A retomada coincide com esse recorte; ao incluir a grade derivada da ata, o próprio escritor, comandos e decisões que reconstroem a sobra, o universo é maior. A revisão semântica de todos os equivalentes e chamadores indiretos ainda é **[não verificado]**.

A cópia não tem metadados Git acessíveis: `git rev-parse HEAD` retornou “not a git repository”. Portanto, a identidade desta cópia com o hash informado é **[não verificado]**. Não abri `models.py` nem `core/juizes.py`.

## (a) Censo

O script abaixo imprime **candidatos sintáticos**, com função e expressão, em `app/` e `bin/`. Ele inclui acessos à grade, escritas indiretas por métodos mutáveis e nomes que podem ser homônimos; a classificação exige leitura humana. Também procura referências ao classificador e nomes sugestivos para localizar derivações que não usem a chave literal. Não é prova de ausência de aliases, desempacotamento, SQL, acesso dinâmico ou outra grafia equivalente: **[não verificado]**.

Executar na raiz da cópia, sem criar arquivo:

```bash
python3 -B - <<'PY'
import ast
from pathlib import Path

RAIZES = (Path("app"), Path("bin"))
CHAVES = {"orfas", "n_orfas"}
PISTAS = ("orfa", "sobra", "cluster")
PROIBIDOS = {"app/core/juizes.py"}

def excluido(p):
    return (
        "tests" in p.parts
        or "migrations" in p.parts
        or p.name == "models.py"
        or p.as_posix() in PROIBIDOS
    )

def funcao(n, pais):
    while n in pais:
        n = pais[n]
        if isinstance(n, (ast.FunctionDef, ast.AsyncFunctionDef)):
            return n.name
    return "<modulo>"

def literal(n):
    return n.value if isinstance(n, ast.Constant) and isinstance(n.value, str) else None

for raiz in RAIZES:
    if not raiz.exists():
        continue
    for p in sorted(raiz.rglob("*.py")):
        if excluido(p):
            continue
        try:
            arvore = ast.parse(p.read_bytes(), filename=str(p))
        except (SyntaxError, ValueError, UnicodeError) as exc:
            print(f"ERRO\t{p}\t{exc}")
            continue

        pais = {
            filho: pai
            for pai in ast.walk(arvore)
            for filho in ast.iter_child_nodes(pai)
        }
        achados = []
        for n in ast.walk(arvore):
            classe = None
            if isinstance(n, ast.Subscript) and literal(n.slice) in CHAVES:
                classe = "CHAVE_" + (
                    "LE" if isinstance(n.ctx, ast.Load) else "ESCREVE"
                )
            elif (
                isinstance(n, ast.Attribute)
                and n.attr in CHAVES
                and isinstance(n.ctx, ast.Load)
            ):
                classe = "ATRIBUTO_LE"
            elif (
                isinstance(n, ast.Call)
                and isinstance(n.func, ast.Attribute)
                and n.func.attr in {"get", "pop", "setdefault"}
                and n.args
                and literal(n.args[0]) in CHAVES
            ):
                classe = "CHAVE_" + n.func.attr.upper()
            elif (
                isinstance(n, ast.Compare)
                and any(isinstance(op, (ast.In, ast.NotIn)) for op in n.ops)
                and any(literal(x) in CHAVES for x in [n.left, *n.comparators])
            ):
                classe = "PRESENCA_CHAVE"
            elif (
                isinstance(n, ast.Name)
                and n.id == "classificar_orfas"
            ):
                classe = "CLASSIFICADOR"
            elif isinstance(n, (ast.FunctionDef, ast.AsyncFunctionDef)):
                if any(s in n.name.lower() for s in PISTAS):
                    classe = "PISTA_FUNCAO"
            if classe:
                trecho = ast.get_source_segment(p.read_text(encoding="utf-8"), n)
                if isinstance(n, (ast.FunctionDef, ast.AsyncFunctionDef)):
                    trecho = n.name
                trecho = " ".join((trecho or "").split())[:180]
                achados.append((n.lineno, n.col_offset, classe, funcao(n, pais), trecho))

        for linha, coluna, classe, nome, trecho in sorted(achados):
            print(f"{p}:{linha}:{coluna}\t{nome}\t{classe}\t{trecho}")
PY
```

O script cobre a sintaxe literal, não detecta por si só uma decisão baseada em nomes como `_resto`, nem faz análise de fluxo entre funções; por isso a matriz inclui achados da leitura dirigida ([app/ponto/turnos.py:1711](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1711)).

### Leituras diretas da ata: 14 expressões, 10 arquivos

| Arquivo:linha | Função | O que lê | Papel preliminar |
|---|---|---|---|
| [chamados/juizes.py:514](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/juizes.py:514) | `_celula_furada` | `orfas` e `n_orfas`: 2 | **DECIDE** se sobra prende o dia, condicionado à régua |
| [chamados/services/materializacao.py:449](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/chamados/services/materializacao.py:449) | `_plantio_preenche_furo_na_data` | `orfas`: 1 | **DECIDE** plantio/materialização |
| [escala/services/leitor_celula.py:187](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/leitor_celula.py:187) | `_orfas_da_ata` | `orfas` e `n_orfas`: 2 | **MOSTRA/transporta**; contagem antiga vira `?` |
| [ponto/juiz_batida.py:161](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/juiz_batida.py:161) | `periodos_do_dia` | `orfas`: 1 | **MOSTRA**; contrato declara que órfã não soma |
| [ponto/management/commands/tripwire_celulas.py:264](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/tripwire_celulas.py:264) | `medir_sobra_com_tipo_trocado` | `orfas`: 1 | **DECIDE/MOSTRA** assinatura para vigilância |
| [ponto/services/cartorio.py:839](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:839) | `insumos_da_ata` | `n_orfas`: 1 | **DECIDE** entrada/saída desconhecidas quando não há lâmpadas |
| [ponto/services/cluster_espurio.py:29](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cluster_espurio.py:29) | `_leitura_da_celula` | verificação e índice de `orfas`: 2 | **DECIDE** cluster, inclusive sua premissa para chamado |
| [ponto/services/esmeril_espelho.py:125](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/esmeril_espelho.py:125) | `assinaturas_do_colab` | `orfas`: 1 | **DECIDE/MOSTRA** assinatura recorrente |
| [ponto/services/vigia_pareador_celula.py:74](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/vigia_pareador_celula.py:74) | `censo` | `n_orfas` e `orfas`: 2 | **DECIDE/MOSTRA** divergência do pareador |
| [relatorios/cartao_pela_celula.py:288](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/relatorios/cartao_pela_celula.py:288) | `orfas_e_isencao_da_ata` | `orfas`: 1 | **MOSTRA** no cartão |

O total não conta duas vezes a leitura no mesmo nó, mas conta separadamente `get` e índice, inclusive na mesma função; não inclui o snapshot em `bin/`, que também lê a chave ([bin/snapshot_o118_col221.py:32](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/bin/snapshot_o118_col221.py:32)).

### Grade e derivações adicionais: matriz preliminar

| Arquivo:linha | Função | O que lê ou deriva | Papel |
|---|---|---|---|
| [escala/orfa_classe.py:58](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/orfa_classe.py:58) | `classificar_orfas` | órfãs, tipo do dia, marco vazio e rajada | **DECIDE** classe; hoje sem chamador de produção encontrado |
| [ponto/turnos.py:1711](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1711) | `papel_por_minuto_da_ata` | sobra dentro do vão; luz próxima e par/perna de pausa sob teto | **DECIDE/PAGA** indiretamente: atribui papel `X` ou deixa o dia sem mapa; efeito financeiro por caso **[não verificado]** |
| [ponto/propositor.py:34](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/propositor.py:34) | `dia_degenerado` | grade, órfãs, furos e janela de cluster | **DECIDE** escalada de proposta |
| [ponto/propositor.py:77](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/propositor.py:77) | atribuição global | limiar de sobra e tolerância de slot próprios | **DECIDE** atribuição proposta; abrangência de produção **[não verificado]** |
| [escala/services/furos_diarios.py:22](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/furos_diarios.py:22) | `_furos_do_dia` | contagem da grade | **GRAVA** indiretamente em `FuroDiario`, pela porta ([ponto/portas/celula.py:1257](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/portas/celula.py:1257)) |
| [folha/causas.py:43](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/folha/causas.py:43) | `classificar_causa` | presença de sobra, inclusive em folga | **DECIDE/MOSTRA** causa; efeito em pagamento **[não verificado]** |
| [ponto/supra_juiz.py:392](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/supra_juiz.py:392) e [527](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/supra_juiz.py:527) | `classificar_dia` | presença e conteúdo de `orfas` | **DECIDE/MOSTRA** fato e motivo do dia |
| [ponto/status_dia.py:188](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/status_dia.py:188) | `fatos_do_status` | quantidade de órfãs da grade | **DECIDE** fatos do status |
| [ponto/services/bordas_realizado.py:49](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/bordas_realizado.py:49) | `_teve_batida_na_ata` | presença de órfãs na grade | **DECIDE** existência de batida |
| [ponto/turnos.py:2679](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:2679) | `horas_reais_do_dia` | horas das órfãs | **DECIDE** insumo temporal |
| [ponto/services/simular_dia.py:12](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/simular_dia.py:12) | `dia_perfeito` | presença de órfãs | **DECIDE** perfeição do dia |
| [colaboradores/services/calendario.py:409](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/colaboradores/services/calendario.py:409) | `contexto_calendario` | presença e horas | **MOSTRA** |
| [relatorios/pdf_espelho.py:477](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/relatorios/pdf_espelho.py:477) e [552](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/relatorios/pdf_espelho.py:552) | `_coletar_dados_espelho` | horas por grade/data | **MOSTRA** |
| [ponto/management/commands/propor_reorganizacao.py:104](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/propor_reorganizacao.py:104) | `_run` | horas da grade | **DECIDE/MOSTRA** proposta |
| [ponto/management/commands/detectar_par_relampago.py:140](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/management/commands/detectar_par_relampago.py:140) | `_score_geometria` | quantidade de órfãs | **DECIDE/MOSTRA** escore |
| [ponto/services/cartorio.py:736](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:736) e [1426](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:1426) | `ata_do_dia`, `_julgar_colab_corpo` | sobra da grade | **DECIDE/GRAVA** ata e insumos |
| [escala/utils.py:1979](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/utils.py:1979) | `montar_grade_prevista_periodo_por_turno` | órfãs da própria grade | **DECIDE/GRAVA** estrutura da grade; somente endereço, sem proposta de alteração |

A categoria **PAGA** sinaliza caminho potencial, não prova de valor pago em produção; a classificação completa de efeito financeiro é **[não verificado]**.

## (b) Classes propostas

São classes **dentro de `classificar_orfas`**, usando instante e contexto da ata quando disponíveis. A ata atual grava horas como strings ([ponto/services/cartorio.py:742](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:742)); o classificador atual devolve dicionário por `HH:MM` ([escala/orfa_classe.py:53](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/orfa_classe.py:53)). Isso não distingue duas batidas no mesmo minuto, como o par de col67; a identidade por batida e o instante completo são necessários antes de contar sobras com fidelidade. O caso de col67 está no dado fornecido, **[não verificado em arquivo:linha]**.

| Classe proposta | Base e decisão |
|---|---|
| `FOLGA_DESMENTIDA` | **DECISÃO existente:** prevalece para dia não considerado trabalho pelo classificador ([escala/orfa_classe.py:62](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/orfa_classe.py:62)). Se o dia reconstruído for `indefinido`, a aplicação desse ramo também precisa ser examinada; sua correção para todos os estados é **[não verificado]**. |
| `ECO_PROXIMO_DE_LUZ` | **DECISÃO candidata, limitada ao domínio já existente:** `JANELA_CLUSTER_MIN = 15` ([escala/orfa_classe.py:28](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/orfa_classe.py:28)); o papel atual aplica distância até luz acesa **dentro do vão** ([ponto/turnos.py:1711](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1711), [1742](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1742)). Col67 motiva preservar segundos, sem usar tipo gravado **[não verificado em arquivo:linha]**. A constante não autoriza chamar qualquer par oposto de eco nem estender o domínio para fora do vão. |
| `PAR_DE_PAUSA_DEDUZIDO` / `PERNA_DE_PAUSA` | **DECISÃO candidata no domínio já descrito:** o código usa `intra_max_s` no par ou na perna ligada a luz de intervalo ([ponto/turnos.py:1761](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1761)). Col44 e col331 motivam o par; col87 e col975 mostram que menos marcos que batidas não basta para afirmar pausa **[não verificado em arquivo:linha]**. O valor aplicável depende do regime/chamador; seleção para cada caso **[não verificado]**. |
| `APOS_SAIDA_FINAL` | A posição relativa à saída final é descritiva; **qual distância ou condição faz a avulsa contar como novo turno? PERGUNTA DE LEI 1.** Col214 em 29/10 e col263 em 31/10 motivam o caso **[não verificado em arquivo:linha]**. Não atribuo um teto novo. |
| `EXCEDENTE_SEM_ENQUADRAMENTO` | Para escala com menos marcos que batidas, ou par sem elementos suficientes para pausa, manter `INDEFINIDA` até haver critério; **qual limite ou evidência separa pausa, jornada adicional e cadastro inadequado? PERGUNTA DE LEI 2.** Col87 e col975 motivam a pergunta **[não verificado em arquivo:linha]**. |

A sonda fornecida rotulou 16 ocorrências como próximas de tipo oposto e 89 como longe, em 83 dias e 50 pessoas; esses são rótulos da sonda, não a saída real de `classificar_orfas` nem contagem de classes propostas **[não verificado em arquivo:linha]**. Em particular, o dado não demonstra que as 16 satisfaçam o domínio de `ECO_PROXIMO_DE_LUZ` ([ponto/turnos.py:1735](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/turnos.py:1735)).

A precedência entre classes propostas e a correspondência inequívoca de cada sobra a uma batida são **[não verificado]**. Não há regra proposta baseada no tipo gravado.

## (c) Esboço da sombra

Não há acesso ao banco nesta sessão; os totais antes e depois são **[não verificado]**. O esboço abaixo mede a **ata lavrada**, por competência de cada empresa, usa a função real e inicia uma transação somente leitura. A seleção do universo exato da sonda original e a equivalência entre ata e grade original precisam ser conferidas. `?`, horários duplicados e colisões do dicionário são contados como não mensuráveis, sem inventar instante ([escala/services/leitor_celula.py:180](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/leitor_celula.py:180), [escala/orfa_classe.py:53](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/orfa_classe.py:53)).

```python
# Esboço para manage.py shell; não executado.
from collections import Counter
from django.db import connection, transaction
from escala.models import CelulaDia
from escala.services.leitor_celula import grade_da_celula
from escala.orfa_classe import classificar_orfas, INDEFINIDA
from folha.export import janela_competencia

ANO, MES = 2026, 11

with transaction.atomic():
    with connection.cursor() as cursor:
        # Deve ser o primeiro comando da transação.
        cursor.execute("SET TRANSACTION READ ONLY")

    # ESBOÇO: fornecer aqui o MESMO universo de empresas/colaboradores
    # da sonda, sem filtrar apenas vínculos ativos hoje.
    colaboradores = UNIVERSO_DA_SONDA  # [não verificado]

    antes = Counter()
    depois = Counter()
    for colab in colaboradores:
        # Confirmar no ambiente a relação empresa do colaborador.
        empresa = EMPRESA_DO_COLABORADOR_NO_UNIVERSO  # [não verificado]
        ini, fim = janela_competencia(empresa, MES, ANO)
        cels = {
            c.data: c
            for c in CelulaDia.objects.filter(
                colaborador=colab, data__range=(ini, fim)
            )
        }
        grade = grade_da_celula(colab, ini, fim, celulas=cels)
        for dia in grade["dias"]:
            horas = dia.get("orfas") or []
            if not horas:
                continue
            if (any(not isinstance(h, str) or len(h) < 5
                    or h[2] != ":" or not h[:2].isdigit()
                    or not h[3:5].isdigit() for h in horas)
                    or len(set(horas)) != len(horas)):
                antes["nao_mensuravel_por_hhmm"] += len(horas)
                continue

            classes = classificar_orfas(dia)  # função real do HEAD da cópia
            assert len(classes) == len(horas)
            antes["sobras"] += len(horas)
            antes["indefinidas"] += sum(
                classes[h] == INDEFINIDA for h in horas
            )

            # PSEUDOCÓDIGO: somente após construir a proposta na MESMA
            # autoridade e disponibilizar identidade + instante por sobra:
            # novas = classificar_orfas(dia_com_instantes_e_contexto)
            # depois["indefinidas"] += número de sobras INDEFINIDA
            # depois["sobras"] += número de sobras

    print("ANTES", dict(antes))
    print("DEPOIS [não executável: proposta ainda não construída]", dict(depois))
```

`janela_competencia` delega à janela de fechamento ([folha/export.py:47](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/folha/export.py:47)). O esboço não permite afirmar o número depois: além da proposta inexistente, precisa do universo e da identidade de cada batida. Uma comparação válida deve conservar o mesmo universo e informar separadamente os não mensuráveis **[não verificado]**.

## (d) Escrita, leitura e chamador

`ata_do_dia` constrói `n_orfas` e `orfas` a partir da grade ([ponto/services/cartorio.py:730](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:730)); é chamado no julgamento, entre outros pontos, em [ponto/services/cartorio.py:1437](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/ponto/services/cartorio.py:1437). O conjunto dos leitores diretos encontrados está na tabela de 14 expressões acima; `_orfas_da_ata` alimenta a grade lida por outros consumidores ([escala/services/leitor_celula.py:420](/home/ronald/codex_ws/cd692834dde86e1c0c326a90201d08f6293dc5d6/app/escala/services/leitor_celula.py:420)).

A busca por `classificar_orfas`

[truncado em 20000 de 21164 caracteres]

## 18/11 19:08 · sessao bb0c978e545a · E4C-F6F7 · verificador · origem=Code

**Fecho: LIMPO — 0 achados BLOQUEIA procedentes na leitura do delta fornecido.** Há uma limitação de prova na causa exata da queda da c12 e na identificação da árvore: esta cópia não contém metadados Git nem os logs da volta; o hash, os 20 failures/10 errors e o resultado verde de 8110 testes são dados informados no pedido, **[nao verificado]**. [nao verificado]

1. **Item 1(a): fecha no literal, pelo código examinado.** O cron de produção declara `vigiar_fio_morto --alarme --alarme-passivo` em [app/config/crons.py:1737](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/config/crons.py:1737). O comando chama a função real uma vez por rodada, sobre todas as perguntas em frota ou com recorte por empresa, em [vigiar_fio_morto.py:189](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/management/commands/vigiar_fio_morto.py:189). O esperado é zero em [vigiar_fio_morto.py:102](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/management/commands/vigiar_fio_morto.py:102).

   Se o número for diferente de zero, o comando o imprime e, em frota, o grava no carimbo; **esse contador não provoca `SystemExit(2)`**, pois não está nas listas de alarme. O ESTADO mostra a divergência como `FORA DO ESPERADO` ([vigiar_fio_morto.py:85](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/management/commands/vigiar_fio_morto.py:85), [vigiar_fio_morto.py:216](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/management/commands/vigiar_fio_morto.py:216), [vigiar_fio_morto.py:241](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/management/commands/vigiar_fio_morto.py:241), [bin/fio_estado.py:191](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/bin/fio_estado.py:191)). O papel de vigia, contador esperado zero e nunca juiz está declarado em [CLAUDE.md:169](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/CLAUDE.md:169).

   O caso positivo do teste exige `1` no carimbo e na saída, esperado `0` e ausência nas listas de alarme; portanto, **mataria**, por exemplo, o mutante que grava sempre `0` em `censo.validada_projetada_sem_linha` ([test_f7_vigia_validada_sem_linha.py:39](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/tests/test_f7_vigia_validada_sem_linha.py:39)). Um mutante que limitasse o resultado a `min(numero, 1)` sobreviveria aos três casos desse arquivo, que só esperam zero ou um; essa é análise do teste, **não execução de mutantes** ([test_f7_vigia_validada_sem_linha.py:39](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/tests/test_f7_vigia_validada_sem_linha.py:39), [test_f7_vigia_validada_sem_linha.py:47](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/tests/test_f7_vigia_validada_sem_linha.py:47), [test_f7_vigia_validada_sem_linha.py:55](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/tests/test_f7_vigia_validada_sem_linha.py:55)). A execução efetiva desses mutantes é **[nao verificado]**.

2. **Escritores e leitores.**

   - A validação normal grava `validada_em` e `validada_por` e registra o ato de validação com o mesmo instante em [validacao.py:161](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/services/validacao.py:161). Há outros caminhos para esses campos; a própria árvore mostra, por exemplo, limpeza em [models.py:1702](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:1702).
   - A porta `DecisaoPergunta.registrar` grava as linhas em [models.py:4329](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:4329). O predicado compartilhado lê projeção e atos, confrontando pergunta, tipo e instante, em [models.py:4238](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/models.py:4238). O contador lê esse predicado e soma somente linhas de validação faltantes em [backfill_decisoes_pergunta.py:94](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/management/commands/backfill_decisoes_pergunta.py:94).
   - O vigia escreve `censo.validada_projetada_sem_linha` e `esperado.validada_projetada_sem_linha` no JSON de frota; `_carimbar` grava por arquivo temporário e `os.replace` em [vigiar_fio_morto.py:126](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/management/commands/vigiar_fio_morto.py:126), [vigiar_fio_morto.py:250](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/management/commands/vigiar_fio_morto.py:250). Rodada com `--empresa` imprime o recorte, mas não o publica como frota ([vigiar_fio_morto.py:276](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/management/commands/vigiar_fio_morto.py:276)).
   - `fio_estado` lê o carimbo e compara genericamente cada chave de `esperado` com `censo`; `gerar_estado` usa esse leitor e seu compositor em [bin/fio_estado.py:121](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/bin/fio_estado.py:121) e [bin/gerar_estado.py:260](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/bin/gerar_estado.py:260). O `pipeline_placar` **não compara essa chave**: suas chamadas explícitas são para `fio_mudo_alvos` e `nunca_bateu_sem_adesao` em [pipeline_placar.py:135](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/core/management/commands/pipeline_placar.py:135).

3. **Classes já reprovadas:** no delta fornecido, não identifiquei segundo juiz, escritor de domínio, relógio fora da borda, derivação paralela, mudança de ordem de trava ou segundo significado para o campo novo. O contador delega ao predicado existente; a nova escrita é a fotografia do vigia, não uma gravação da pergunta ou do ato ([backfill_decisoes_pergunta.py:100](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/management/commands/backfill_decisoes_pergunta.py:100), [vigiar_fio_morto.py:183](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/management/commands/vigiar_fio_morto.py:183)). O caso positivo tem a mordida descrita no item 1; cobertura para contagens maiores que um nesse teste específico permanece a limitação indicada, sem achado BLOQUEIA demonstrado ([test_f7_vigia_validada_sem_linha.py:39](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/tests/test_f7_vigia_validada_sem_linha.py:39)).

4. **Folha, exportação, retenção, DiaPago e competência entregue:** o delta **c10 → c12b fornecido** altera o comando do vigia e testes, sem alterar diretamente arquivos desses domínios **[nao verificado: diff não conferível por Git nesta cópia]**. A afirmação mais ampla de que a obra não os toca pelo comportamento não é sustentada: a própria regra de tarefa do dia tem leitores de retenção em [app/docs/JUIZES.md:73](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/docs/JUIZES.md:73). Para **este delta**, a chamada nova do contador só lê perguntas e decisões e grava o carimbo do vigia ([backfill_decisoes_pergunta.py:101](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/management/commands/backfill_decisoes_pergunta.py:101), [vigiar_fio_morto.py:250](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/management/commands/vigiar_fio_morto.py:250)). Impacto medido na competência 11 e em produção: **[nao verificado]**.

5. **Harness c12 → c12b.** As duas linhas fornecidas acrescentam um mock do novo contador aos ambientes dos testes de C5 em memória e de fio mudo/placar; não alteram o comando de produção ([test_e4a2_c5_memoria.py:124](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/tests/test_e4a2_c5_memoria.py:124), [test_fio_mudo_placar.py:113](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/core/tests/test_fio_mudo_placar.py:113)). A causa **compatível com o código** é que a c12 acrescentou ao `handle` uma dependência que esses ambientes isolados ainda não dublavam; o primeiro extrai o `handle` por AST e o segundo declara que dubla derivações externas ([vigiar_fio_morto.py:192](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/management/commands/vigiar_fio_morto.py:192), [test_e4a2_c5_memoria.py:135](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/tests/test_e4a2_c5_memoria.py:135), [test_fio_mudo_placar.py:116](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/core/tests/test_fio_mudo_placar.py:116)). As linhas curam essa dependência ausente nos dois harnesses; a integração real do contador continua coberta pelo teste próprio do vigia em [test_f7_vigia_validada_sem_linha.py:27](/home/ronald/codex_ws/7d5a40f57c4e70510df41ba5c19c2aed8d5a36b5/app/chamados/tests/test_f7_vigia_validada_sem_linha.py:27).

   **Não posso afirmar que essa foi a causa de cada uma das 30 ocorrências, identificar o terceiro módulo ou confirmar pelos logs que a c12b curou toda a causa da volta:** os logs e tracebacks não foram fornecidos **[nao verificado]**. A leitura das duas linhas, por si, também não demonstra uma falha de produção escondida; a execução real e o verde informado continuam **[nao verificado]**. [nao verificado]

_7115 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
