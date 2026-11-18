# CODEX-ESTADO

> Ultima resposta processada: **18/11/2026 19:10** · publicado em **18/11/2026 19:13** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

## 18/11 18:52 · sessao a7c0bb4360d8 · CURA-CHEGA-A-FROTA · sonda · origem=Code

```python
# Executar no Django shell da copia correspondente ao HASH informado.
# Execucao, resultados, identidade da base e efeitos externos: [nao verificado].
#
# Fontes:
# app/escala/models.py:871-876, 1635-1636, 1714-1729
# app/colaboradores/models.py:548-585
# app/ponto/services/cartorio.py:940-943, 1211-1268, 1409-1418, 1955
# app/ponto/management/commands/processar_cartorio.py:33-65
# Anexo v2.py fornecido no pedido: parte_b_colab e main.
#
# Ausencia em cods e a convencao PULARIA, nao prova de pulo real:
# app/ponto/services/cartorio.py:1211-1268, 1409-1418.
# EscalaColaborador nao declara campo de criacao:
# app/escala/models.py:871-1398. Portanto, a busca por outras escalas
# criadas em 18/11 e a inexistencia de um quinto colaborador ficam
# [nao verificado].
#
# O bloqueio de escrita no banco abaixo cobre PostgreSQL e SQLite.
# Efeitos externos e escritas em outras conexoes: [nao verificado].
# As comparacoes de estado observam apenas os campos/modelos indicados.

import datetime as dt
import hashlib
import json
from collections import Counter

from django.core.serializers.json import DjangoJSONEncoder
from django.db import connection, transaction
from django.db.models import Max
from django.utils import timezone

from chamados.models import ChamadoColaborador, PerguntaDisputa
from colaboradores.models import Colaborador, HistoricoVinculo
from escala.models import CelulaDia, EscalaColaborador
from ponto.management.commands.detectar_ausencias import Command as Emissor
from ponto.models import Batida
from ponto.services import cartorio


INI = dt.date(2026, 10, 21)
FIM_COMP = dt.date(2026, 11, 20)
DIA_CURA = dt.date(2026, 11, 18)
ALVOS = (868, 934, 1017, 1028)
PKS_CURA = (1603, 1604, 1605, 1606)

AGORA = timezone.localtime(timezone.now())
HOJE = timezone.localdate(AGORA)
FIM = min(FIM_COMP, HOJE - dt.timedelta(days=1))
CORTE = timezone.make_aware(dt.datetime(2026, 11, 18, 8, 19))

erros = 0
resumo_b = Counter()
resumo_b_corte = Counter()
resumo_b_regeneracao = Counter()
pularia_antes = []

AUSENTE = object()


class ReverterSonda(Exception):
    pass


def instante(valor):
    if valor is None:
        return "None"
    if timezone.is_aware(valor):
        return timezone.localtime(valor).isoformat()
    return valor.isoformat()


def dia_local(valor):
    if valor is None:
        return None
    if timezone.is_naive(valor):
        valor = timezone.make_aware(valor)
    return timezone.localtime(valor).date()


def registrar_erro(col=None, data=None):
    global erros
    erros += 1
    identificador = "col%d" % col if col is not None else "GERAL"
    print(
        "ERRO %s data=%s"
        % (identificador, data if data is not None else "[nao verificado]")
    )


def faixa_corte(valor):
    if valor is None:
        return "SEM_JULGAMENTO"
    if timezone.is_naive(valor):
        valor = timezone.make_aware(valor)
    return "ANTES_08_19" if valor < CORTE else "A_PARTIR_08_19"


def campo_celula(cel, nome):
    # O campo pode nao existir em outra copia do modelo.
    try:
        CelulaDia._meta.get_field(nome)
    except Exception:
        return AUSENTE
    return getattr(cel, nome)


def faixa_regeneracao(valor):
    if valor is AUSENTE:
        return "CAMPO_AUSENTE"
    if valor is not None and dia_local(valor) == DIA_CURA:
        return "REGENERADA_EM_18_11"
    return "SEM_REGENERACAO_EM_18_11"


def digest_linhas(qs):
    linhas = list(qs.order_by("pk").values())
    bruto = json.dumps(
        linhas,
        cls=DjangoJSONEncoder,
        sort_keys=True,
        ensure_ascii=True,
        separators=(",", ":"),
    ).encode("utf-8")
    return len(linhas), hashlib.sha256(bruto).hexdigest()


def estado_observado(c):
    cels = CelulaDia.objects.filter(colaborador=c).order_by("pk").values_list(
        "pk", "data", "impressao", "julgada_em", "veredito"
    )
    linhas = list(cels)
    bruto = json.dumps(
        linhas,
        cls=DjangoJSONEncoder,
        sort_keys=True,
        ensure_ascii=True,
        separators=(",", ":"),
    ).encode("utf-8")
    return {
        "celulas": (len(linhas), hashlib.sha256(bruto).hexdigest()),
        "perguntas": digest_linhas(
            PerguntaDisputa.objects.filter(colaborador=c)
        ),
        "chamados": digest_linhas(
            ChamadoColaborador.objects.filter(colaborador=c)
        ),
    }


def mostrar_comparacao_escrita(pk, antes, durante, depois):
    for nome in ("celulas", "perguntas", "chamados"):
        a = antes[nome]
        d = durante[nome] if durante is not None else None
        f = depois[nome] if depois is not None else None
        print(
            "col%d VERIFICACAO_%s antes=%s durante=%s depois=%s"
            % (pk, nome.upper(), a, d, f)
        )
        if d is not None and d != a:
            print(
                "ESCRITA_DETECTADA col%d modelo=%s momento=dentro"
                % (pk, nome)
            )
        if f is not None and f != a:
            print(
                "ESCRITA_DETECTADA col%d modelo=%s momento=depois"
                % (pk, nome)
            )


def campo_criacao_escala():
    # Inspecao explicita dos possiveis nomes; nenhum existe na copia lida:
    # app/escala/models.py:871-1398.
    for nome in ("criado_em", "criada_em", "created_at"):
        try:
            campo = EscalaColaborador._meta.get_field(nome)
        except Exception:
            continue
        if campo.get_internal_type() == "DateTimeField":
            return nome
    return None


def contar_historicos_e_escalas(escalas_cura):
    print("CONTAGEM_4_X_5 data_local=%s empresas_historico=2,3,4" % DIA_CURA)
    pessoas = set()

    historicos = HistoricoVinculo.objects.filter(
        colaborador__empresa_id__in=(2, 3, 4),
        criado_em__date=DIA_CURA,
    ).order_by("colaborador_id", "criado_em", "pk")

    total_historicos = 0
    for h in historicos:
        total_historicos += 1
        pessoas.add(h.colaborador_id)
        print(
            "HISTORICO col%d pk=%d tipo=%s motivo=%s criado_em=%s"
            % (
                h.colaborador_id, h.pk, h.tipo, h.motivo,
                instante(h.criado_em),
            )
        )
    print("HISTORICOS_TOTAL=%d" % total_historicos)

    campo_criacao = campo_criacao_escala()
    escalas = {e.pk: e for e in escalas_cura}
    if campo_criacao is None:
        print("ESCALAS_OUTRAS_CRIADAS_EM_18_11=[campo ausente]")
    else:
        outras = EscalaColaborador.objects.filter(
            **{campo_criacao + "__date": DIA_CURA}
        ).order_by("pk")
        for escala in outras:
            escalas[escala.pk] = escala

    for escala in sorted(escalas.values(), key=lambda e: e.pk):
        pessoas.add(escala.colaborador_id)
        criado = (
            instante(getattr(escala, campo_criacao))
            if campo_criacao is not None else "[campo ausente]"
        )
        print(
            "ESCALA col%d pk=%d vigencia_inicio=%s criado_em=%s"
            % (escala.colaborador_id, escala.pk, escala.data_inicio, criado)
        )
    print("ESCALAS_IDENTIFICADAS=%d" % len(escalas))
    print("COLABS_IDENTIFICADOS=%d" % len(pessoas))

    if len(pessoas) >= 5:
        print("HA_5O_COLAB=sim")
    elif campo_criacao is None:
        print("HA_5O_COLAB=[nao verificado]")
    else:
        print("HA_5O_COLAB=nao")


def parte_b_colab(pk):
    c = None
    antes = durante = depois = None
    cels = []
    out = cartorio.contadores()
    sucesso = False

    try:
        c = Colaborador.objects.get(pk=pk)
        antes = estado_observado(c)
        try:
            with transaction.atomic():
                try:
                    cels = list(
                        CelulaDia.objects.filter(
                            colaborador=c, data__lt=HOJE
                        ).order_by("data")
                    )
                    if cels:
                        cartorio.julgar_colab(
                            c,
                            cels,
                            hoje=HOJE,
                            agora=AGORA,
                            apply_=False,
                            emissor=Emissor(),
                            out=out,
                            prefix="[DRY] ",
                            write=lambda mensagem: None,
                            forcar=False,
                        )
                    sucesso = True
                finally:
                    durante = estado_observado(c)
                raise ReverterSonda()
        except ReverterSonda:
            pass
    except Exception:
        # A excecao pode conter dados pessoais.
        registrar_erro(pk)
    finally:
        if c is not None and antes is not None:
            try:
                depois = estado_observado(c)
                mostrar_comparacao_escrita(pk, antes, durante, depois)
            except Exception:
                registrar_erro(pk)

    if not sucesso:
        return

    cods = out.get("cods") or {}
    pulados = out["pulados"]
    soma = len(cods) + pulados
    diferenca = soma - len(cels)
    convencao = (
        "CONVENCAO_OK"
        if diferenca == 0
        else "CONVENCAO_QUEBRADA diferenca=%+d" % diferenca
    )
    print(
        "col%d cels_todas_data_lt_hoje=%d cods=%d pulados=%d soma=%d %s"
        % (pk, len(cels), len(cods), pulados, soma, convencao)
    )

    for cel in cels:
        if not (INI <= cel.data <= FIM):
            continue
        try:
            estado = "REJULGARIA" if cel.data in cods else "PULARIA"
            corte = faixa_corte(cel.julgada_em)
            regenerada = campo_celula(cel, "regenerada_em")
            regeneracoes = campo_celula(cel, "regeneracoes")
            dna_anterior = campo_celula(cel, "dna_anterior")
            faixa_reg = faixa_regeneracao(regenerada)

            resumo_b[estado] += 1
            resumo_b_corte[(estado, corte)] += 1
            resumo_b_regeneracao[(estado, faixa_reg)] += 1

            regenerada_texto = (
                "[campo ausente]"
                if regenerada is AUSENTE else instante(regenerada)
            )
            regeneracoes_texto = (
                "[campo ausente]"
                if regeneracoes is AUSENTE else str(regeneracoes)
            )
            dna_texto = (
                "[campo ausente]"
                if dna_anterior is AUSENTE
                else ("sim" if dna_anterior is not None else "nao")
            )

            linha = (
                "col%d data=%s julgada_em=%s estado=%s "
                "veredito_gravado=%r regenerada_em=%s "
                "regeneracoes=%s dna_anterior_existe=%s"
                % (
                    pk, cel.data, instante(cel.julgada_em), estado,
                    cel.veredito, regenerada_texto, regeneracoes_texto,
                    dna_texto,
                )
            )
            if estado == "REJULGARIA":
                linha += " cods_novos=%r x veredito_gravado=%r" % (
                    cods[cel.data], cel.veredito,
                )
            print(linha)

            if estado == "PULARIA" and corte == "ANTES_08_19":
                pularia_antes.append(
                    (pk, cel.data, instante(cel.julgada_em))
                )
        except Exception:
            registrar_erro(pk, cel.data)


def parte_b():
    print("PARTE_B")
    print(
        "HORA_SOMBRA=%s"
        % instante(Batida.objects.aggregate(v=Max("timestamp"))["v"])
    )
    print(
        "HOJE=%s AGORA=%s FIM_COMP_OBSERVADO=%s"
        % (HOJE, instante(AGORA), FIM)
    )
    print(
        "RESULTADOS=[nao verificado: executar na sombra] "
        "PULARIA_POR_AUSENCIA_EM_CODS=[nao verificado como pulo real]"
    )

    escalas_cura = list(
        EscalaColaborador.objects.filter(pk__in=PKS_CURA).order_by("pk")
    )
    print("PRESENCA_DA_CURA=%s" % ("sim" if escalas_cura else "nao"))
    if not escalas_cura:
        print("BASE_ANTERIOR_A_CURA")
        return

    contar_historicos_e_escalas(escalas_cura)

    for pk in ALVOS:
        parte_b_colab(pk)

    print("RESUMO_B_POR_ESTADO")
    for estado in ("PULARIA", "REJULGARIA"):
        print("%s=%d" % (estado, resumo_b[estado]))

    print("RESUMO_B_POR_ESTADO_E_JULGAMENTO")
    for estado in ("PULARIA", "REJULGARIA"):
        for corte in ("ANTES_08_19", "A_PARTIR_08_19", "SEM_JULGAMENTO"):
            print("%s %s=%d" % (estado, corte, resumo_b_corte[(estado, corte)]))

    print("RESUMO_B_POR_ESTADO_E_REGENERACAO")
    for estado in ("PULARIA", "REJULGARIA"):
        for faixa in (
            "REGENERADA_EM_18_11",
            "SEM_REGENERACAO_EM_18_11",
            "CAMPO_AUSENTE",
        ):
            print(
                "%s %s=%d"
                % (estado, faixa, resumo_b_regeneracao[(estado, faixa)])
            )

    print("PULARIA_JULGADOS_ANTES_08_19")
    for pk, data, julgada in pularia_antes:
        print("col%d data=%s julgada_em=%s" % (pk, data, julgada))
    print(
        "CAUSA_E_INSUMO_HISTORICO=[nao verificado] "
        "PULO_REAL_EM_CADA_LINHA=[nao verificado]"
    )


def main():
    sqlite_query_only_anterior = None
    try:
        if connection.in_atomic_block:
            print("SONDA_NAO_EXECUTADA transacao_preexistente")
            registrar_erro()
            return

        if connection.vendor == "sqlite":
            with connection.cursor() as cursor:
                cursor.execute("PRAGMA query_only")
                sqlite_query_only_anterior = cursor.fetchone()[0]
                cursor.execute("PRAGMA query_only = ON")
            with transaction.atomic():
                parte_b()
        elif connection.vendor == "postgresql":
            with transaction.atomic():
                with connection.cursor() as cursor:
                    cursor.execute("SET TRANSACTION READ ONLY")
                parte_b()
        else:
            print("SONDA_NAO_EXECUTADA bloqueio_de_escrita=[nao verificado]")
            registrar_erro()
    except Exception:
        # Nao imprimir a excecao: pode conter dados pessoais.
        registrar_erro()
    finally:
        if sqlite_query_only_anterior is not None:
            try:
                with connection.cursor() as cursor:
                    cursor.execute(
                        "PRAGMA query_only = %d"
                        % int(sqlite_query_only_anterior)
                    )
            except Exception:
                registrar_erro()
        print("ERROS=%d" % erros)
        print("FIM_SONDA")


main()
```

## 18/11 18:52 · sessao c0c28b92827c · CURA-CHEGA-A-FROTA · censo · origem=Code

**Dos 56 dias de 18/11, os 56 já constavam nos 106 de 17/11; nenhum é novo. Sete estão na lista RED dos 37 dias de intervalo/rajada do brief.** A atribuição histórica dos outros 49 dias não pode ser recuperada com certeza: o brief informa os totais, mas não publica os rótulos individuais dessas classes. [não verificado: anexos transcritos na pergunta, sem arquivo e linhas disponíveis na cópia]

A tabela abaixo oferece **uma aplicação deduzida e reproduzível**, não uma reconstrução comprovada dos rótulos históricos. O próprio cabeçalho da parte A marca os mapeamentos de paridade, tipo trocado e borda como não verificados. As referências aos anexos nesta resposta são identificadas por colaborador e data; como não há numeração de linhas dos anexos disponível, **[não verificado] aplica-se a cada afirmação e linha baseada neles**.

### Critério usado

- **Intervalo/rajada — ESTRUTURA:** a lista RED do brief identifica os 37 pares `(col,data)`. A leitura da régua e das batidas mostra a órfã associada ao intervalo ou à saída, próxima de marco vazio; o brief descreve o conflito com o marco vizinho. Para o cruzamento histórico, usei a **lista RED**, não apenas proximidade entre batidas. [não verificado]
- **Paridade noturna — CADASTRO [deduzido]:** régua noturna atravessando a meia-noite, batidas entre D-1 e D e órfã próxima de marco vazio, com padrão recorrente compatível com deslocamento de dia/fase. Isso é uma hipótese visual: a sonda não demonstra o cadastro nem a fase 12x36. [não verificado]
- **Horário diferente — CADASTRO [deduzido]:** sequência de batidas sugere jornada deslocada em relação à régua. A diferença observada não comprova qual horário foi cadastrado corretamente. [não verificado]
- **Tipo trocado — BATIDA [deduzido]:** tipo E/S informado contraria o papel sugerido pela sequência e pelo marco vazio. Uma sequência com tipos repetidos pode ter outras explicações. [não verificado]
- **Borda [deduzido]:** dados insuficientes ou jornada incompleta impedem atribuição segura às demais classes. [não verificado]

Essas regras não estabelecem uma precedência histórica entre sinais sobrepostos. Por exemplo, **col174 15/11** pode ser lido como horário diferente, e **col300 21/10** pode ser incluído no padrão noturno, embora só haja uma batida visível. Portanto, a distribuição seguinte é uma partição possível, não a única. [não verificado]

| Classe aplicada | Dias | Pessoas | [nao verificado]
|---|---:|---:|
| Intervalo/rajada — RED | 7 | 3 |
| Paridade noturna — deduzida | 42 | 11 |
| Horário diferente — deduzido | 2 | 2 |
| Tipo trocado — deduzido | 3 | 3 |
| Borda — deduzida | 2 | 2 |
| **Total** | **56** | **21** |

### As 56 linhas

Em todas as linhas, a classe é a aplicação acima; **exceto RED, ela é [deduzido] e a atribuição histórica é [não verificado]**. Os dados de cada motivo vêm da entrada correspondente em `parte_a_1811.out` transcrita na pergunta **[não verificado: sem arquivo:linha]**.

| Col | Data | Classe | Motivo |
|---|---|---|---|
| col107 | 25/10 | Horário | A jornada observada começa às 07:01, enquanto a régua começa às 12:50. |
| col174 | 15/11 | Paridade | A jornada atravessa a meia-noite e a régua noturna está vazia. |
| col198 | 09/11 | Paridade | Há entrada na véspera e saída às 06:58 junto de S07:00 vazio. |
| col220 | 11/11 | Tipo | A órfã 06:00 é E junto ao marco S06:00 vazio. |
| col252 | 17/11 | RED | A volta 13:05E fica órfã junto a E13:38 após 12:50S. |
| col252 | 13/11 | RED | A volta 12:49E fica órfã junto a E13:38 após 12:35S. |
| col252 | 11/11 | RED | A volta 12:52E fica órfã junto a E13:38 após 12:39S. |
| col252 | 10/11 | RED | A volta 12:52E fica órfã junto a E13:38 após 12:39S. |
| col252 | 09/11 | RED | A volta 12:52E fica órfã junto a E13:38 após 12:38S. |
| col300 | 21/10 | Borda | Só há 06:00S visível, insuficiente para identificar a jornada. |
| col366 | 09/11 | Paridade | Entrada na véspera e três órfãs coincidem com marcos noturnos vazios. |
| col366 | 05/11 | Paridade | Entrada na véspera e três órfãs coincidem com marcos noturnos vazios. |
| col366 | 03/11 | Paridade | Entrada na véspera e três órfãs coincidem com marcos noturnos vazios. |
| col366 | 30/10 | Paridade | Entrada na véspera e três órfãs coincidem com marcos noturnos vazios. |
| col366 | 28/10 | Paridade | Entrada na véspera e três órfãs coincidem com marcos noturnos vazios. |
| col369 | 12/11 | Tipo | Há duas saídas finais S e a saída de intervalo está órfã. |
| col444 | 16/11 | RED | A saída 13:55S fica órfã junto a S13:00, seis minutos antes da volta. |
| col451 | 12/11 | Paridade | A saída 07:02S fica órfã junto a S07:00 após entrada na véspera. |
| col451 | 10/11 | Paridade | A saída 06:59S fica órfã junto a S07:00 após entrada na véspera. |
| col489 | 10/11 | Horário | As batidas da véspera sugerem jornada 16:01–00:00 frente à régua exibida. |
| col566 | 17/11 | Paridade | Entrada às 18:09 na véspera e saída 06:32S acompanham régua noturna vazia. |
| col566 | 15/11 | Paridade | Entrada às 18:09 na véspera e saída 06:13S acompanham régua noturna vazia. |
| col587 | 22/10 | Tipo | A sequência 17:58E, 18:57S e 19:03E sugere papéis E/S trocados. |
| col670 | 14/11 | Borda | A entrada prevista está vazia e só três batidas aparecem no dia. |
| col859 | 09/11 | Paridade | A saída 07:00S coincide com marco vazio após entrada na véspera. |
| col868 | 17/11 | Paridade | A jornada atravessa a meia-noite e 06:49S está órfã junto a S07:00. |
| col868 | 15/11 | Paridade | A jornada atravessa a meia-noite e 07:03S está órfã junto a S07:00. |
| col868 | 13/11 | Paridade | Batidas noturnas ficam órfãs junto a marcos vazios após entrada na véspera. |
| col868 | 11/11 | Paridade | Batidas noturnas ficam órfãs junto a marcos vazios após entrada na véspera. |
| col868 | 07/11 | Paridade | Batidas noturnas ficam órfãs junto a marcos vazios após entrada na véspera. |
| col868 | 05/11 | Paridade | Batidas noturnas ficam órfãs junto a marcos vazios após entrada na véspera. |
| col868 | 03/11 | Paridade | Batidas noturnas ficam órfãs junto a marcos vazios após entrada na véspera. |
| col868 | 01/11 | Paridade | Batidas noturnas ficam órfãs junto a marcos vazios após entrada na véspera. |
| col868 | 30/10 | Paridade | Batidas noturnas ficam órfãs junto a marcos vazios após entrada na véspera. |
| col868 | 28/10 | Paridade | Batidas noturnas ficam órfãs junto a marcos vazios após entrada na véspera. |
| col868 | 26/10 | Paridade | Batidas noturnas ficam órfãs junto a marcos vazios após entrada na véspera. |
| col868 | 24/10 | Paridade | Batidas noturnas ficam órfãs junto a marcos vazios após entrada na véspera. |
| col868 | 22/10 | Paridade | Batidas noturnas ficam órfãs junto a marcos vazios após entrada na véspera. |
| col876 | 07/11 | Paridade | A saída 06:55S fica órfã junto a S07:00 após entrada na véspera. |
| col934 | 16/11 | Paridade | A saída 11:04S fica órfã após entrada às 22:58 na véspera. |
| col934 | 14/11 | Paridade | A saída 11:00S fica órfã após entrada às 22:57 na véspera. |
| col934 | 12/11 | Paridade | A saída 11:00S fica órfã após entrada às 23:01 na véspera. |
| col934 | 10/11 | Paridade | A saída 11:09S fica órfã após entrada às 22:57 na véspera. |
| col934 | 08/11 | Paridade | A saída 10:45S fica órfã após entrada às 23:00 na véspera. |
| col964 | 28/10 | RED | A saída 11:35S fica órfã junto a S11:00, onze minutos antes da volta. |
| col1017 | 17/11 | Paridade | A saída 05:57S fica órfã junto a S06:00 após entrada na véspera. |
| col1017 | 15/11 | Paridade | A saída 05:58S fica órfã junto a S06:00 após entrada na véspera. |
| col1017 | 13/11 | Paridade | A saída 05:59S fica órfã junto a S06:00 após entrada na véspera. |
| col1017 | 11/11 | Paridade | A saída 05:59S fica órfã junto a S06:00 após entrada na véspera. |
| col1017 | 09/11 | Paridade | A saída 06:00S fica órfã junto a S06:00 após entrada na véspera. |
| col1017 | 07/11 | Paridade | A saída 06:00S fica órfã junto a S06:00 após entrada na véspera. |
| col1017 | 05/11 | Paridade | A saída 05:59S fica órfã junto a S06:00 após entrada na véspera. |
| col1031 | 17/11 | Paridade | A saída 06:55S fica órfã junto a S07:00 após entrada na véspera. |
| col1031 | 15/11 | Paridade | Batidas 00:01E e 06:55S ficam órfãs junto a marcos noturnos vazios. |
| col1031 | 13/11 | Paridade | A saída 06:52S fica órfã junto a S07:00 após entrada na véspera. |
| col1031 | 11/11 | Paridade | Batidas 00:07E e 06:52S ficam órfãs junto a marcos noturnos vazios. |

### Cruzamento e leitura da cura

Dos **56 repetidos**, sete têm classe histórica identificável pela lista RED: col252 em 09, 10, 11, 13 e 17/11; col444 em 16/11; e col964 em 28/10. Para os outros **49**, a classe histórica individual é **[não verificado]**; a tabela acima apresenta hipóteses. Novos: **0**. [não verificado: anexos transcritos sem arquivo:linha]

Há uma diferença relevante entre o critério descrito no brief e o código da cópia: a função protege quando a batida está a **até 90 minutos do próprio marco e a mais de 15 minutos da dona**; a guarda pode retirá-la quando essa proteção não se aplica, satisfeitas as demais condições. Isso permite retirada quando a distância à dona é até 15 minutos **ou** a distância ao próprio marco supera 90 minutos. Não demonstra, por si só, qual ramo executou em cada uma das sete linhas. [app/escala/utils.py:502](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/utils.py:502), [app/escala/utils.py:587](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/utils.py:587), [app/escala/utils.py:654](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/utils.py:654)

As fotografias também mudaram em alguns detalhes: em col252 17/11 aparece a saída final 17:05S na parte A; em col587 22/10 a órfã 20:59 deixa de aparecer. Essas diferenças são observações entre sondas, não erros de leitura comprovados. [não verificado: anexos transcritos sem arquivo:linha]

**rajada hoje: 7 dias** — entendida como sobreviventes da classe RED de 17/11, não como prova de sete rajadas verdadeiras. **erros de leitura: nenhum confirmado; pendentes de verificação estão os rótulos históricos individuais fora da RED e a procedência de cada órfã.** [não verificado]

## 18/11 18:51 · sessao 2cf110dd395d · - · - · origem=Ronald (tmux)

Cruzamento por colaborador e data: **56 já constavam da autópsia e 0 são novos** [não verificado: os anexos raspados não estão disponíveis como arquivos com numeração de linhas].

Dos **37 casos RED** enumerados no brief, **7 permanecem**: col252 em 09, 10, 11, 13 e 17/11; col444 em 16/11; e col964 em 28/10 [não verificado: anexos raspados sem arquivo:linha].

A implementação da cópia protege a batida quando ela está a até 90 minutos do marco próprio **e** a mais de 15 minutos da dona; a guarda atua quando essa proteção é falsa. Assim, a condição implementada permite atuação quando a distância ao marco é maior que 90 minutos **ou** a distância à dona é de até 15 minutos, divergindo do **e** registrado no brief ([utils.py](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/utils.py:502), [utils.py](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/utils.py:589)). As fronteiras são testadas separadamente ([test_cirineu_nao_desfaz_o_papel.py](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/tests/test_cirineu_nao_desfaz_o_papel.py:95)).

As diferenças entre saídas incluem novas batidas em D+1, o fechamento de S17:00 de col252 em 17/11 e a ausência da segunda órfã de col587 em 22/10; essas diferenças não demonstram, por si, erro de leitura [não verificado: anexos raspados sem arquivo:linha].

## 18/11 18:51 · sessao 12fbcde747c1 · - · - · origem=Ronald (tmux)

A lista RED do brief contém exatamente 37 pares `(col, data)`. Destes, sete aparecem na amostra de 18/11: col252 em 09, 10, 11, 13 e 17/11; col444 em 16/11; e col964 em 28/10. Os sete envolvem três pessoas. A classificação histórica por linha das demais classes não está explicitada no dado fornecido. [não verificado]

Uma partição possível dos 56 dias é: 42 de paridade noturna, sete estruturais, dois de horário diferente, três de tipo trocado e dois de borda. Essa partição é **deduzida e não única**; em particular, col174 em 15/11, col369 em 12/11 e col489 em 10/11 admitem interpretações concorrentes. [não verificado]

Na cópia, a proteção exige simultaneamente distância de até 90 minutos ao próprio marco e distância maior que 15 minutos à dona; a guarda usa a negação dessa proteção. Assim, a exclusão pode ocorrer com distância maior que 90 minutos **ou** vizinhança de até 15 minutos, enquanto o brief descreve as duas condições combinadas. [app/escala/utils.py:502](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/utils.py:502), [app/escala/utils.py:589](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/utils.py:589), [app/escala/utils.py:660](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/utils.py:660); redação do brief fornecido [não verificado].

## 18/11 18:51 · sessao d1a42be044ef · - · - · origem=Ronald (tmux)

O brief identifica os 37 pares da classe estrutural pela lista RED, mas não registra a atribuição individual das outras classes. O código disponível usa outro esquema de classificação de órfãs; portanto, as demais atribuições históricas são inferências, não uma reprodução verificada. ([app/escala/orfa_classe.py:53](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/orfa_classe.py:53); anexo brief.md [não verificado])

A interseção da lista RED com a sonda de 18/11 tem **7 dias**: col252 em 09, 10, 11, 13 e 17/11; col444 em 16/11; e col964 em 28/10. As distâncias entre as batidas do intervalo nesses casos variam de 6 a 15 minutos. (anexos brief.md e parte_a_1811.out [não verificado])

Na cópia, a proteção exige simultaneamente distância ao próprio marco de até 90 minutos e distância à dona vizinha superior a 15 minutos; quando as demais condições da guarda se cumprem, sua negação permite retirar a batida se ela estiver a mais de 90 minutos do próprio marco **ou** a até 15 minutos da dona. Isso difere da conjunção descrita no brief. A execução efetiva da guarda em cada linha e o produtor da órfã permanecem [não verificado]. ([app/escala/utils.py:502](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/utils.py:502), [app/escala/utils.py:587](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/utils.py:587), [app/escala/utils.py:654](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/utils.py:654); anexo brief.md [não verificado])

## 18/11 18:47 · sessao cec15d9ae23e · CURA-CHEGA-A-FROTA · sonda · origem=Code

```python
# Executar no Django shell da copia correspondente ao HASH informado.
# Resultados e execucao: [nao verificado].
#
# Fontes:
# app/ponto/management/commands/processar_cartorio.py:33-65
# app/ponto/services/cartorio.py:1211-1268, 1409-1418, 1477, 1955
# app/chamados/models.py:33-44, 2593-2620
# Molde fornecido: anexo molde_sonda_autopsia.py.
#
# O molde fornecido nao atribui FASE_12X36, TIPO_TROCADO ou BORDA.
# Seu unico conjunto selecionado recebe aqui o nome descritivo
# ORFA_PERTO_DE_MARCO_VAZIO; nao ha mapeamento verificado dessas classes.
#
# A ausencia em out['cods'] nao prova sempre que o dia foi pulado:
# no ramo sem adesao, o calculo de codigos fica dentro de apply_;
# no ramo normal, um dia sem grade tambem pode sair sem codigo.
# Assim, PULARIA abaixo e a convencao pedida para ausencia em cods,
# com essa ressalva; o contador pulados e exibido para conferencia.
#
# As comparacoes de estado detectam mudancas nos campos e modelos
# observados, inclusive dentro da transacao antes do rollback. Elas nao
# provam ausencia de toda escrita possivel ou de efeitos externos.
# A execucao e seus resultados permanecem [nao verificado].

import datetime as dt
import hashlib
import json
from collections import Counter, defaultdict

from django.core.serializers.json import DjangoJSONEncoder
from django.db import transaction
from django.db.models import Max
from django.utils import timezone

from chamados.models import ChamadoColaborador, PerguntaDisputa
from colaboradores.models import Colaborador
from escala.models import CelulaDia
from escala.utils import grade_espelho_janela
from ponto.management.commands.detectar_ausencias import Command as Emissor
from ponto.models import Batida
from ponto.services import cartorio
from ponto.turnos import batidas_apuraveis


INI = dt.date(2026, 10, 21)
FIM_COMP = dt.date(2026, 11, 20)
ALVOS = (868, 934, 1017, 1028)
AGORA = timezone.localtime(timezone.now())
HOJE = timezone.localdate(AGORA)
FIM = min(FIM_COMP, HOJE - dt.timedelta(days=1))
CORTE = timezone.make_aware(dt.datetime(2026, 11, 18, 8, 19))

erros = 0
resumo_b = Counter()
resumo_b_corte = Counter()
pularia_antes = []


class ReverterSonda(Exception):
    pass


def instante(valor):
    if valor is None:
        return "None"
    if timezone.is_aware(valor):
        return timezone.localtime(valor).isoformat()
    return valor.isoformat()


def registrar_erro(col, data=None):
    global erros
    erros += 1
    print("ERRO col%d data=%s" % (col, data if data else "[nao verificado]"))


def faixa_corte(valor):
    if valor is None:
        return "SEM_JULGAMENTO"
    if timezone.is_naive(valor):
        valor = timezone.make_aware(valor)
    return "ANTES_08_19" if valor < CORTE else "A_PARTIR_08_19"


def digest_linhas(qs):
    # Os dados nao sao impressos; o hash inclui todos os campos concretos
    # de Pergunta/Chamado e os campos solicitados de CelulaDia.
    linhas = list(qs.order_by("pk").values())
    bruto = json.dumps(
        linhas, cls=DjangoJSONEncoder, sort_keys=True,
        ensure_ascii=True, separators=(",", ":"),
    ).encode("utf-8")
    return len(linhas), hashlib.sha256(bruto).hexdigest()


def estado_observado(c):
    cels = CelulaDia.objects.filter(colaborador=c).order_by("pk").values_list(
        "pk", "data", "impressao", "julgada_em", "veredito"
    )
    linhas = list(cels)
    bruto = json.dumps(
        linhas, cls=DjangoJSONEncoder, sort_keys=True,
        ensure_ascii=True, separators=(",", ":"),
    ).encode("utf-8")
    return {
        "celulas": (len(linhas), hashlib.sha256(bruto).hexdigest()),
        "perguntas": digest_linhas(
            PerguntaDisputa.objects.filter(colaborador=c)
        ),
        "chamados": digest_linhas(
            ChamadoColaborador.objects.filter(colaborador=c)
        ),
    }


def mostrar_comparacao_escrita(pk, antes, durante, depois):
    for nome in ("celulas", "perguntas", "chamados"):
        a = antes[nome]
        d = durante[nome] if durante is not None else None
        f = depois[nome] if depois is not None else None
        print(
            "col%d VERIFICACAO_%s antes=%s durante=%s depois=%s"
            % (pk, nome.upper(), a, d, f)
        )
        if d is not None and d != a:
            print("ESCRITA_DETECTADA col%d modelo=%s momento=dentro" % (pk, nome))
        if f is not None and f != a:
            print("ESCRITA_DETECTADA col%d modelo=%s momento=depois" % (pk, nome))


def parte_a():
    print("PARTE_A")
    print(
        "MAPEAMENTO FASE_12X36=[nao verificado] "
        "TIPO_TROCADO=[nao verificado] BORDA=[nao verificado] "
        "classe_do_molde=ORFA_PERTO_DE_MARCO_VAZIO"
    )
    if FIM < INI:
        print("PARTE_A_JANELA_VAZIA")
        return

    lo = timezone.make_aware(
        dt.datetime(INI.year, INI.month, INI.day)
    ) - dt.timedelta(days=1)
    hi = timezone.make_aware(
        dt.datetime(FIM.year, FIM.month, FIM.day, 23, 59, 59)
    ) + dt.timedelta(days=1)

    dias_por_classe = Counter()
    pessoas_por_classe = defaultdict(set)

    for c in Colaborador.objects.filter(
        empresa_id__in=[2, 3, 4], situacao="ativo"
    ).order_by("pk"):
        try:
            bs = sorted(
                batidas_apuraveis(c, lo, hi),
                key=lambda b: b.timestamp,
            )
            g = grade_espelho_janela(c, bs, INI, FIM, agora=AGORA)
            marcos = [
                (m.get("tipo"), m.get("hora"))
                for m in (g.get("marcos") or [])
            ]
        except Exception:
            registrar_erro(c.pk)
            continue

        for d in (g.get("dias") or []):
            data = None
            try:
                orf = d.get("orfas") or []
                cels = d.get("celulas") or []
                if not orf or d.get("tipo_dia") != "trabalho":
                    continue

                hit = []
                for h in orf:
                    hm = int(h[:2]) * 60 + int(h[3:5])
                    best = None
                    for i, (tp, mh) in enumerate(marcos):
                        if (
                            not mh or len(mh) != 5 or mh[2] != ":"
                            or not (mh[:2] + mh[3:]).isdigit()
                        ):
                            continue
                        if i >= len(cels) or not (
                            isinstance(cels[i], dict)
                            and cels[i].get("missing")
                        ):
                            continue
                        mm = int(mh[:2]) * 60 + int(mh[3:5])
                        dist = abs(hm - mm)
                        dist = min(dist, 1440 - dist)
                        if best is None or dist < best[0]:
                            best = (dist, tp, mh)
                    if best is not None and best[0] <= 90:
                        hit.append(
                            "%s~%s%s(%dmin)"
                            % (h, best[1], best[2], best[0])
                        )
                if not hit:
                    continue

                lab = str(d.get("label"))
                data = dt.date(2026, int(lab[-2:]), int(lab[-5:-3]))
                regua = []
                for i, (tp, mh) in enumerate(marcos):
                    ce = cels[i] if i < len(cels) else None
                    st = (
                        "fora" if ce is None else
                        "VAZIO" if (
                            isinstance(ce, dict) and ce.get("missing")
                        ) else "ok"
                    )
                    regua.append("%s%s=%s" % (tp, mh, st))

                perto = []
                for b in bs:
                    lt = timezone.localtime(b.timestamp)
                    if data - dt.timedelta(days=1) <= lt.date() <= data + dt.timedelta(days=1):
                        perto.append(
                            "%s %s%s"
                            % (
                                lt.strftime("%d"),
                                lt.strftime("%H:%M"),
                                getattr(b, "tipo", "?"),
                            )
                        )

                classe = "ORFA_PERTO_DE_MARCO_VAZIO"
                dias_por_classe[classe] += 1
                pessoas_por_classe[classe].add(c.pk)
                print(
                    "col%d data=%s classe=%s orfa~marco=%s"
                    % (c.pk, data, classe, "; ".join(hit))
                )
                print("  regua=" + " ".join(regua))
                print("  batidas_D-1_a_D+1=" + ", ".join(perto))
            except Exception:
                registrar_erro(c.pk, data)

    print("TABELA_A classe dias pessoas")
    classe = "ORFA_PERTO_DE_MARCO_VAZIO"
    print(
        "%s %d %d"
        % (classe, dias_por_classe[classe], len(pessoas_por_classe[classe]))
    )


def parte_b_colab(pk):
    c = None
    antes = durante = depois = None
    cels = []
    out = cartorio.contadores()
    sucesso = False

    try:
        c = Colaborador.objects.get(pk=pk)
        antes = estado_observado(c)
        try:
            with transaction.atomic():
                try:
                    # Mesmo universo e ordem do comando.
                    cels = list(
                        CelulaDia.objects.filter(
                            colaborador=c, data__lt=HOJE
                        ).order_by("data")
                    )
                    if cels:
                        # Mesmo emissor e inicializacao de out do DRY.
                        # A saida textual do juiz e descartada para evitar
                        # divulgacao de nomes ou outros dados pessoais.
                        cartorio.julgar_colab(
                            c, cels,
                            hoje=HOJE,
                            agora=AGORA,
                            apply_=False,
                            emissor=Emissor(),
                            out=out,
                            prefix="[DRY] ",
                            write=lambda mensagem: None,
                            forcar=False,
                        )
                    sucesso = True
                finally:
                    durante = estado_observado(c)
                raise ReverterSonda()
        except ReverterSonda:
            pass
    except Exception:
        # Nao imprimir a excecao: pode conter dados pessoais.
        registrar_erro(pk)
    finally:
        if c is not None and antes is not None:
            try:
                depois = estado_observado(c)
                mostrar_comparacao_escrita(pk, antes, durante, depois)
            except Exception:
                registrar_erro(pk)

    if not sucesso:
        return

    cods = out.get("cods") or {}
    print(
        "col%d CONTADOR_PULADOS_TODAS_AS_CELULAS=%d"
        % (pk, out["pulados"])
    )
    for cel in cels:
        if not (INI <= cel.data <= FIM):
            continue
        try:
            estado = "REJULGARIA" if cel.data in cods else "PULARIA"
            corte = faixa_corte(cel.julgada_em)
            resumo_b[estado] += 1
            resumo_b_corte[(estado, corte)] += 1

            linha = (
                "col%d data=%s julgada_em=%s estado=%s veredito_gravado=%r"
                % (
                    pk, cel.data, instante(cel.julgada_em),
                    estado, cel.veredito,
                )
            )
            if estado == "REJULGARIA":
                linha += " cods_novos=%r x veredito_gravado=%r" % (
                    cods[cel.data], cel.veredito,
                )
            print(linha)

            if estado == "PULARIA" and corte == "ANTES_08_19":
                pularia_antes.append((pk, cel.data, instante(cel.julgada_em)))
        except Exception:
            registrar_erro(pk, cel.data)


def main():
    try:
        print(
            "HORA_SOMBRA_MAX_BATIDA=%s"
            % instante(Batida.objects.aggregate(v=Max("timestamp"))["v"])
        )
        print("HOJE=%s AGORA=%s FIM_COMP_OBSERVADO=%s" % (HOJE, instante(AGORA), FIM))
        print(
            "RESULTADOS=[nao verificado: executar na sombra] "
            "PULARIA_POR_AUSENCIA_EM_CODS=[nao verificado como pulo real]"
        )

        parte_a()

        print("PARTE_B")
        for pk in ALVOS:
            parte_b_colab(pk)

        print("RESUMO_B_POR_ESTADO")
        for estado in ("PULARIA", "REJULGARIA"):
            print("%s=%d" % (estado, resumo_b[estado]))

        print("RESUMO_B_POR_ESTADO_E_JULGAMENTO")
        for estado in ("PULARIA", "REJULGARIA"):
            for corte in ("ANTES_08_19", "A_PARTIR_08_19", "SEM_JULGAMENTO"):
                print(
                    "%s %s=%d"
                    % (estado, corte, resumo_b_corte[(estado, corte)])
                )

        print("PULARIA_JULGADOS_ANTES_08_19")
        for pk, data, julgada in pularia_antes:
            print("col%d data=%s julgada_em=%s" % (pk, data, julgada))
        print(
            "CAUSA_E_INSUMO_HISTORICO=[nao verificado] "
            "PULO_REAL_EM_CADA_LINHA=[nao verificado]"
        )
    finally:
        print("ERROS=%d" % erros)
        print("FIM_SONDA")


main()
```

## 18/11 18:44 · sessao ebee0648b218 · CURA-CHEGA-A-FROTA · sonda · origem=Code

Não consegui fechar a parte **(a)** com uma classificação reproduzível que preserve a identidade da batida órfã: a grade expõe as órfãs como horários, enquanto o tipo gravado pertence à `Batida` ([utils.py:1843](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/utils.py:1843), [models.py:24](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/models.py:24)). A sonda abaixo é **parcial**: cobre a comparação de impressões de (b), mas não deve ser usada como resposta completa ao pedido.

A igualdade do hash não demonstra, por si só, qual insumo histórico mudou: a função grava um resumo dos insumos, e o DNA anterior disponível é apenas a versão imediatamente anterior ([cartorio.py:471](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:471), [models.py:1714](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/models.py:1714)). A atribuição dos históricos à cura também fica `[nao verificado]`: o registro não contém um identificador inequívoco dessa execução ([vinculo.py:350](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/colaboradores/services/vinculo.py:350)).

```python
# SONDA PARCIAL, somente leitura. NAO IMPLEMENTA A PARTE (a).
# [nao verificado] Execucao na sombra e resultados.
#
# O processar_cartorio entrega todas as celulas passadas do colaborador,
# em ordem de data: app/ponto/management/commands/processar_cartorio.py:61.
# As duas chamadas de impressao reproduzidas abaixo sao as do corpo do
# cartorio: app/ponto/services/cartorio.py:1257 e :1398.
# Nenhuma funcao de julgamento ou escrita e chamada.

import datetime as dt
import re
from collections import Counter

from django.db.models import Max
from django.utils import timezone

from colaboradores.models import Colaborador, HistoricoVinculo
from escala.models import CelulaDia, EscalaColaborador
from escala.utils import montar_grade_prevista_periodo
from ponto.models import Batida
from ponto.turnos import ausencia_cobre, batidas_apuraveis, dias_encerrados
from ponto.services import cartorio
from ponto.services.furos_vetados import motivo_do_veto, vinculo_do_emissor
from ponto.supra_juiz import _dia_sem_trabalho_pelo_juiz
from chamados.juizes import fio_vivo
from core.constants import isento_ponto

INI = dt.date(2026, 10, 21)
FIM = dt.date(2026, 11, 20)
ALVOS = (868, 934, 1017, 1028)
HOJE = dt.date.today()
AGORA = timezone.now()
CORTE = timezone.make_aware(dt.datetime(2026, 11, 18, 8, 19))

erros = 0
resumo = Counter()


def erro(contexto, exc):
    global erros
    erros += 1
    # Nao imprime texto da excecao: pode conter nome ou CPF.
    print("ERRO %s tipo=%s detalhe=[nao verificado; texto omitido]"
          % (contexto, type(exc).__name__))


def instante(x):
    if x is None:
        return "None"
    return timezone.localtime(x).isoformat() if timezone.is_aware(x) else x.isoformat()


def preparar(c):
    # Mesma lista completa passada pelo comando ao julgar_colab.
    cels = list(CelulaDia.objects.filter(
        colaborador=c, data__lt=HOJE
    ).order_by("data"))
    if not cels:
        return cels, None

    d0, d1 = cels[0].data, cels[-1].data
    j_ini = timezone.make_aware(dt.datetime.combine(
        d0 - dt.timedelta(days=1), dt.time.min))
    j_fim = timezone.make_aware(dt.datetime.combine(
        d1 + dt.timedelta(days=3), dt.time.min))
    bs = list(batidas_apuraveis(
        c, j_ini, j_fim - dt.timedelta(microseconds=1)))
    ref = d1 + dt.timedelta(days=1)
    g = montar_grade_prevista_periodo(
        c, bs, dias=(d1 - d0).days + 2, ref=ref, agora=AGORA)
    dias = {
        ref - dt.timedelta(days=i): dia
        for i, dia in enumerate(g.get("dias") or [])
    }
    primeira = Batida.objects.filter(
        colaborador=c, retratada_em__isnull=True
    ).order_by("timestamp").first()
    adesao = timezone.localdate(primeira.timestamp) if primeira else None
    chamados = cartorio.chamados_que_falam_de_batida(c) if adesao else {}
    encerrados = dias_encerrados(
        c, d0, d1, agora=AGORA,
        celulas={(c.pk, x.data): x for x in cels}
    ) if adesao else {}
    return cels, {
        "bs": bs,
        "dias": dias,
        "marcos": g.get("marcos") or [],
        "adesao": adesao,
        "chamados": chamados,
        "encerrados": encerrados,
        "isento": isento_ponto(c),
    }


def impressao_atual(c, cel, ctx):
    data = cel.data
    dia = ctx["dias"].get(data) or {}
    cob = ausencia_cobre(c, data, para="cobranca")
    regra = cartorio.versao_para(data, getattr(c, "empresa", None))
    parcial = cartorio._cob_parcial_da_impressao(dia)

    if ctx["adesao"] is None:
        parcial_bool = bool(dia.get("cobertura_parcial"))
        coberto_inteiro = (
            dia.get("tipo_dia") in ("ausencia", "falta")
            and not parcial_bool
        )
        ata = cartorio.dispensar_por_ausencia(
            cartorio.ata_do_dia(dia, ctx["marcos"], dna=cel.dna)
            if dia else None,
            cob, dia=dia,
        )
        insumo = cartorio.com_insumo_dia_sem_trabalho(
            dia, trabalha=cel.trabalha, ata=ata)
        sem_trabalho = (
            _dia_sem_trabalho_pelo_juiz(insumo)
            and cartorio.sem_adesao_no_horizonte(
                data, getattr(c, "empresa", None))
        )
        return cartorio.impressao_insumos(
            [], getattr(cob, "status", None), [], cel.dna,
            regra=regra, cob_parcial=parcial, isento=ctx["isento"],
            sem_adesao=not coberto_inteiro and sem_trabalho,
        )

    lista = ctx["chamados"].get(data.isoformat(), [])
    bj = [
        b for b in ctx["bs"]
        if data - dt.timedelta(days=1)
        <= timezone.localdate(b.timestamp)
        <= data + dt.timedelta(days=1)
    ]
    vivos = [x for x in lista if x.status_local in cartorio.VIVOS]
    com_fio = [
        x for x in vivos if fio_vivo(x.perguntas_que_resolvem.all())
    ]
    veto = motivo_do_veto(c, vinculo_do_emissor(c, data), data)
    em_curso = data < HOJE and not ctx["encerrados"].get(data, False)

    # teto_de_hoje=False, igual ao processar_cartorio ordinario.
    return cartorio.impressao_insumos(
        bj, getattr(cob, "status", None), lista, cel.dna,
        veto=veto,
        enc=0 if em_curso else None,
        teto=None,
        regra=regra,
        fio=len(com_fio) if vivos else None,
        cob_parcial=parcial,
        isento=ctx["isento"],
    )


def contar_historicos():
    # HistoricoVinculo.criado_em existe, mas a porta nao grava ID da cura.
    # Portanto isto conta candidatos no dia, NAO "criados por esta cura".
    qs = HistoricoVinculo.objects.filter(
        criado_em__date=dt.date(2026, 11, 18),
        colaborador__empresa_id__in=(2, 3, 4),
        motivo="correcao",
        tipo__in=("escala", "ancora"),
    )
    print(
        "HISTORICOS_CANDIDATOS_18_11 linhas=%d pessoas=%d "
        "dos_quatro=%d atribuicao_a_cura=[nao verificado]"
        % (
            qs.count(),
            qs.values("colaborador_id").distinct().count(),
            qs.filter(colaborador_id__in=ALVOS)
              .values("colaborador_id").distinct().count(),
        )
    )
    campos = {f.name for f in EscalaColaborador._meta.fields}
    if "criado_em" not in campos:
        print(
            "ESCALAS_CRIADAS_18_11=[nao verificado: "
            "EscalaColaborador nao possui criado_em]"
        )
    else:
        print(
            "ESCALAS_CRIADAS_18_11=[nao verificado: "
            "data de criacao isolada nao identifica a cura]"
        )


def main():
    print("HORA_SOMBRA_MAX_BATIDA=%s" % instante(
        Batida.objects.aggregate(v=Max("timestamp"))["v"]))
    print("DATE_TODAY=%s AGORA=%s" % (HOJE, instante(AGORA)))
    print("PARTE_A=[nao verificado: nao implementada nesta sonda]")
    print(
        "PULOU significa apenas carimbo anterior ao corte e hash igual; "
        "efeito causal da cura e insumo historico mudado=[nao verificado]"
    )

    try:
        contar_historicos()
    except Exception as exc:
        erro("historicos", exc)

    for pk in ALVOS:
        try:
            c = Colaborador.objects.get(pk=pk)
            cels, ctx = preparar(c)
        except Exception as exc:
            erro("col%d preparacao" % pk, exc)
            continue

        if ctx is None:
            print("col%d SEM_CELULAS_PASSADAS" % pk)
            continue

        for cel in cels:
            if not (INI <= cel.data <= FIM and cel.data < HOJE):
                continue
            try:
                atual = impressao_atual(c, cel, ctx)
                gravada = cel.impressao
                igual = bool(gravada and gravada == atual)
                julgada = cel.julgada_em

                if not julgada or not gravada:
                    estado = "SEM_CARIMBO"
                elif julgada < CORTE and igual:
                    estado = "PULOU"
                elif julgada >= CORTE:
                    estado = "JULGADA_DEPOIS"
                    # Julgada depois nao prova que foi a cura que chegou.
                    if not igual:
                        estado += "_HASH_DIFERENTE_AGORA"
                elif not igual:
                    estado = "HASH_DIFERENTE_NAO_REJULGADO_DESDE_CORTE"
                else:
                    estado = "INDETERMINADO"

                resumo[estado] += 1
                dna = cel.dna or {}
                print(
                    "col%d %s julgada_em=%s gravada=%s hoje=%s "
                    "comparacao=%s estado=%s dna_versao=%r marcos=%r "
                    "veredito=%r"
                    % (
                        pk, cel.data, instante(julgada), gravada, atual,
                        "igual" if igual else "diferente",
                        estado, dna.get("dna_versao"), dna.get("marcos"),
                        cel.veredito,
                    )
                )
                if estado == "PULOU":
                    print(
                        "col%d %s insumo_mudou_e_fora_da_impressao="
                        "[nao verificado: hash nao reconstitui insumos "
                        "historicos]"
                        % (pk, cel.data)
                    )
            except Exception as exc:
                erro("col%d %s" % (pk, cel.data), exc)

    print("RESUMO_B")
    for chave in sorted(resumo):
        print("%s=%d" % (chave, resumo[chave]))
    print("ERROS=%d" % erros)
    print("FIM_SONDA")


main()
```

## 18/11 18:41 · sessao f24ff05adc45 · CURA-CHEGA-A-FROTA · verificador · origem=Code

**AVAL-DEVOLVIDO — PROCEDE nos itens 1 e 2.** A impressão pode ser estendida de forma seletiva, mas a passada automática identificada percorre todas as células passadas da empresa, sem excluir competência exportada; além disso, o rejulgamento alimenta leitores de folha e retenção. A substituição da regeneração pelo item 1 ainda não está demonstrada para o universo do item 2. ([cartorio.py:471](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:471); [processar_cartorio.py:45](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/processar_cartorio.py:45); [export.py:1231](/home/ronald/codex_ws/c8b6d1ec25ac1b921c37558f95021cc99cc2/app/folha/export.py:1231))

## Item 1 — [nome]-NA-IMPRESSÃO

**1. Premissas.**

- **(a) Conferem.** A frase literal está no comentário da disciplina do fio; veto, teto, fio e cobertura parcial acrescentam partes condicionais à lista que forma a impressão. As condições não são idênticas: veto e cobertura parcial usam teste de valor verdadeiro; teto e fio distinguem `None` de zero. ([cartorio.py:486](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:486); [cartorio.py:491](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:491); [cartorio.py:511](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:511); [cartorio.py:528](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:528))
- **(b) Confere como afirmação do arquivo:** a linha 283 diz que o bump rejulgaria a frota inteira do horizonte. A versão aplicável é escolhida por competência e entra na impressão quando não é `None`. ([cartorio.py:269](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:269); [cartorio.py:283](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:283); [cartorio.py:495](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:495))
- **(c) A guarda existe e a grade resultante alimenta o julgamento.** `_protegida_da_guarda` preserva a batida a até 90 minutos do próprio marco e a mais de 15 minutos da dona; é chamada tanto no alinhamento por papel quanto no caminho DP. O cartório monta a grade e a usa para formar a ata e classificar o dia. A condição protegida precisa ser caracterizada com cuidado: esses limites são uma **exceção à retirada** pela guarda, não uma definição genérica de toda rajada. ([utils.py:502](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/utils.py:502); [orfa_classe.py:28](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/orfa_classe.py:28); [regua_defesa.py:392](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/regua_defesa.py:392); [utils.py:586](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/utils.py:586); [utils.py:651](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/utils.py:651); [cartorio.py:1172](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:1172); [cartorio.py:1435](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:1435); [cartorio.py:1464](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:1464))
- **(d) O golden existe**, com cinco impressões literais comparadas à saída da função. Ele cobre essas fixtures e argumentos; sua existência não prova, por si só, identidade para todos os dias possíveis. Execução nesta revisão: **[não verificado]**. ([test_o219_versao_da_regra.py:39](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/tests/test_o219_versao_da_regra.py:39); [test_o219_versao_da_regra.py:98](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/tests/test_o219_versao_da_regra.py:98))
- **(e) Existe pulo**, com qualificações: exige julgamento e veredito, impressão igual e ausência de `forcar`; o ramo normal também exclui o veredito `nunca_bateu` da condição de estar em dia. O ramo sem adesão tem comparação e pulo próprios. ([cartorio.py:1257](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:1257); [cartorio.py:1409](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:1409))
- **(f) L-124 não diz simplesmente “competência aberta”.** Ela estabelece horizonte para frente, passado como foto até fato novo e backfill único no fim; também preserva o rejulgamento por fato novo em qualquer competência e a porta para correção provada em exportada. O aval precisa resolver expressamente a relação entre esse alcance e a alteração proposta da impressão. ([LEIS.md:145](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/LEIS.md:145))

**2. Colisão com avais vigentes.** Pelo desenho declarado, o arquivo de edição pretendido é o cartório; alteração de `motor_calculo_v2.py` pelo item 1 não foi especificada **[não verificado]**. Mas há colisão **por leitor** com a restrição ESPURIAS-ABRE enquanto ela vigorar: a nova impressão pode provocar lavra de ata e veredito, que a retenção e os leitores de folha usam. O adiamento declarado para depois da simulação evita a colisão temporal **se cumprido**, mas o fim efetivo da simulação e a data do pouso são **[não verificado]**. ([cartorio.py:1851](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:1851); [export.py:1201](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/export.py:1201))

O item 1 também interfere no objetivo do item 4 de RAIA ao propor outro caminho para fazer chegar a cura, mas a equivalência com a regeneração manual é **[não verificado]**. O texto integral desses avais externos, além das descrições fornecidas na pergunta, é **[não verificado]**.

**3. Condições literais.**

- A impressão atual **não é hash de um dicionário inteiro**: concatena partes ordenadas, inclusive `str(dna_versao)` e `str(marcos)`, depois aplica SHA-256. Preservar o byte exige não acrescentar parte alguma no caso ausente e conservar as entradas e sua ordem; acrescentar chave ou parte vazia não equivale a omiti-la. O golden prova os casos que contém, não toda a frota. ([cartorio.py:480](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:480); [cartorio.py:554](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:554); [test_o219_versao_da_regra.py:98](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/tests/test_o219_versao_da_regra.py:98))
- A passada identificável é `processar_cartorio --apply`, agendada às **06:27**, por empresa, dependente de `reconciliar_grade`. Ela seleciona células de data anterior a hoje e chama o julgamento mesmo para células já julgadas; com impressão divergente, o pulo não se aplica. O cartório relavra sem regenerar o DNA da célula. A execução real do cron, seu sucesso e os dias concretos alcançados em produção são **[não verificado]**. ([crons.py:616](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/config/crons.py:616); [processar_cartorio.py:49](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/processar_cartorio.py:49); [cartorio.py:943](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:943))
- “Hoje PULADO” para um dia concreto com a rajada e palavra velha depende do estado gravado e dos demais insumos: **[não verificado]**. O código mostra a possibilidade do pulo, não sua ocorrência nos dias da frota. ([cartorio.py:1398](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:1398))
- Há motor geral de DIFF de frota para fechamento, por colaborador e campo, e há medidor de veredito velho com comparação de export; **um instrumento já pronto que liste cada dia movido por esta condição específica e identifique ausência de rajada: [não verificado]**. O DIFF financeiro agregado por colaborador, sozinho, não satisfaz literalmente essa tabela por dia. ([diff_frota.py:1](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/diff_frota.py:1); [diff_frota.py:45](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/diff_frota.py:45); [celula_veredito_velho.py:1](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/celula_veredito_velho.py:1))

**4. Exportada, dinheiro e `!`.** A competência exportada está fora **por promessa no aval, não por filtro dessa passada**: a consulta do comando filtra empresa e `data__lt=hoje`, sem recorte de competência aberta ou exclusão de exportada. Assim, um marcador novo calculado também em dia exportado pode tornar sua impressão diferente e levá-lo à lavra. ([processar_cartorio.py:45](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/processar_cartorio.py:45); [cartorio.py:1409](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:1409))

A relavra pode mudar ata, veredito e protestos, além de emitir ou atualizar cobrança; esses dados chegam à retenção, ao `classificar_export` e a `fora_da_folha`. Portanto há efeito potencial de folha e de entrada/saída do TXT, ainda que a relavra, por si, não prove recálculo ou alteração de valores financeiros gravados. Impacto financeiro e nominal nesta frota: **[não verificado]**. ([cartorio.py:1829](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:1829); [cartorio.py:1851](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:1851); [export.py:765](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/export.py:765); [export.py:1273](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/export.py:1273); [fora_da_folha.py:115](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/fora_da_folha.py:115); [celula_veredito_velho.py:44](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/celula_veredito_velho.py:44))

## Item 2 — substituir a regeneração manual

**1. Premissas.** O item depende de o item 1 estar no ar e alcançar os dias pretendidos. A árvore atual demonstra que a relavra usa a grade e pode ser disparada por impressão diferente, mas não demonstra que isso corrija os mesmos dias e os mesmos fatos que a regeneração de células prevista no outro aval. A relavra não regenera o DNA; portanto equivalência entre as duas operações é **[não verificado]**. ([cartorio.py:943](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:943); [cartorio.py:1172](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:1172))

**2. Colisão.** O item 2 declara superado o item 4 de RAIA antes da comprovação dessa equivalência; seu texto integral e o universo do backlog são **[não verificado]**. As mesmas restrições temporais e de leitor do item 1 se aplicam ao mecanismo do qual o item 2 depende. ([cartorio.py:1851](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:1851); [export.py:1201](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/export.py:1201))

**3. Literal.** A condição de parar se o DIFF mover dia sem rajada exige uma comparação **por colaborador-dia**, com predicado de rajada definido e lista de dias movidos. O motor geral encontrado devolve medições financeiras por colaborador; a sonda específica que cumpra essa condição permanece **[não verificado]**. Mesmo DIFF zerado fora da rajada não demonstraria, sozinho, que toda correção dependente de regeneração de DNA foi substituída. ([diff_frota.py:45](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/diff_frota.py:45); [cartorio.py:943](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:943))

**4. Exportada, dinheiro e `!`.** O item 2 não descreve uma escrita adicional, mas sua eficácia depende da relavra do item 1 e herda o risco de alcance a exportadas e de mudança dos leitores de retenção e exportação. Dias efetivamente movidos, dinheiro e impacto de cancelar a regeneração: **[não verificado]**. ([processar_cartorio.py:49](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/processar_cartorio.py:49); [export.py:1273](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/export.py:1273))

## Quem escreve e quem lê a impressão

O campo é `CelulaDia.impressao`. A busca nos arquivos Python de aplicação identificou os seguintes usos diretos; usos indiretos, dinâmicos ou fora desse escopo: **[não verificado]**. ([models.py:1741](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/models.py:1741))

- **Calcula e entrega à porta:** `impressao_insumos`; o cartório a chama e envia o resultado a `carimbar` nos ramos sem adesão e normal. ([cartorio.py:471](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:471); [cartorio.py:1257](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:1257); [cartorio.py:1314](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:1314); [cartorio.py:1398](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:1398); [cartorio.py:1874](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:1874))
- **Grava e também lê para idempotência:** `carimbar` compara o valor anterior e atribui o novo. ([celula.py:1181](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/portas/celula.py:1181))
- **Apaga em um caso específico:** `desfazer_julgamento_da_futura` inclui `impressao` entre os campos zerados para célula futura, respeitando a exceção de veredito administrativo. ([celula.py:1112](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/portas/celula.py:1112); [celula.py:1145](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/portas/celula.py:1145))
- **Lê para decidir o pulo:** o cartório, nos dois ramos. **Lê para snapshot de reversão:** `celula_veredito_velho`, na opção de aplicação. ([cartorio.py:1260](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:1260); [cartorio.py:1409](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:1409); [celula_veredito_velho.py:90](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/celula_veredito_velho.py:90))

**Fecho por item:**

- **Item 1 — PROCEDE:** falta um recorte efetivo que preserve exportadas, uma definição testável da condição protegida e prova do impacto nos leitores de folha. ([processar_cartorio.py:49](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/processar_cartorio.py:49); [utils.py:502](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/utils.py:502); [export.py:1273](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/export.py:1273))
- **Item 2 — PROCEDE:** a substituição da regeneração e o DIFF nominal por dia ainda não estão demonstrados; a relavra não regenera DNA. ([cartorio.py:943](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:943); [diff_frota.py:45](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/diff_frota.py:45))

## 18/11 18:40 · sessao 60639badeb09 · CURA-CHEGA-A-FROTA · leitura · origem=Code

**Conclusão:** a premissa técnica do item 1 confere: a grade atual pode entregar um dia diferente ao juiz enquanto a impressão continua igual, pois ela já contém batidas e DNA, mas não contém a decisão da guarda [nome]. A passada comum então pula uma célula já julgada que satisfaça os demais requisitos de idempotência. Há, porém, um limite material para o aval: **a passada existente percorre todas as células passadas e não exclui competência exportada**; a cura precisa respeitar esse recorte também nas passadas por evento. [app/escala/utils.py:581; app/ponto/services/cartorio.py:480; app/ponto/services/cartorio.py:1409; app/ponto/management/commands/processar_cartorio.py:49]

Não foi possível verificar o hash do HEAD por Git: a cópia acessível não é um repositório Git. A identidade da cópia com o hash informado é **[não verificado]**. Nenhum dado de produção foi consultado; a existência e a quantidade de dias afetados são **[não verificado]**.

### A. Onde a guarda mora

- As constantes são `RAIO_DO_MARCO_MIN = 90` e `JANELA_CLUSTER_MIN = 15`. `_protegida_da_guarda(batida, marco, dona)` compara instantes completos em segundos: distância ao próprio marco **≤ 90 minutos** e distância à batida dona **> 15 minutos**. [app/escala/regua_defesa.py:392; app/escala/orfa_classe.py:28; app/escala/utils.py:502]
- Ela é chamada em dois caminhos de `_match_marcos`: depois do alinhamento por papel e no alinhamento DP. Só interfere quando a batida foi atribuída a um marco, outro marco é o mais próximo dela, e este já está ocupado por uma dona ao menos tão próxima dele. A ordem dos marcos e das batidas e a atribuição feita antes da guarda, portanto, fazem parte do contexto necessário; a comparação não é feita apenas por `HH:MM`. [app/escala/utils.py:573; app/escala/utils.py:587; app/escala/utils.py:595; app/escala/utils.py:639]
- Sem a proteção, o casador desocupa o marco atribuído e transforma a batida em órfã; com ela, preserva a ocupação. O teste com 11:31 e 12:23 demonstra a ocupação preservada nos dois caminhos; o controle com dona a um minuto demonstra marco `missing` e órfã. [app/escala/utils.py:587; app/escala/utils.py:651; app/escala/tests/test_cirineu_nao_desfaz_o_papel.py:29; app/escala/tests/test_cirineu_nao_desfaz_o_papel.py:59]
- O cartório carrega batidas apuráveis, monta a grade e entrega o `dia` ao `classificar_dia`; a ata registra lâmpadas, quantidade e horas de órfãs. Assim, preservar uma batida pode mudar marco ocupado, lista de órfãs e, conforme o restante do dia, códigos e palavra lavrada — não necessariamente muda o veredito em todo caso. [app/ponto/services/cartorio.py:1170; app/ponto/services/cartorio.py:730; app/ponto/services/cartorio.py:1464]
- A classe **não é devolvida pelo casador**: `classificar_orfas` a deriva da grade. Em trabalho, sem marco vazio ela dá `indefinida`; com marco vazio, uma órfã isolada é `geometria` e uma sequência de pelo menos duas é `eco_cluster`. Alterar ocupação e órfãs pode, portanto, alterar também essa classe derivada; não encontrei chamada de produção a `classificar_orfas` no cartório. O juiz do dia lê a lista de órfãs diretamente. [app/escala/orfa_classe.py:53; app/escala/orfa_classe.py:68; app/ponto/supra_juiz.py:522]

### B. Qual insumo precisa entrar

A impressão atual já inclui `pk`, tipo e timestamp das batidas da janela e os marcos do DNA. Para mover o hash de um dia antigo **sem fato novo**, precisa acrescentar uma representação não vazia da **intervenção efetiva da guarda**: o caso em que o cluster-guard retiraria a batida, mas `_protegida_da_guarda` preserva o marco. Recalcular apenas a proximidade de qualquer batida com qualquer marco seria mais amplo que esse caso. [app/ponto/services/cartorio.py:471; app/escala/utils.py:587; app/escala/utils.py:655]

Formas possíveis, todas extraídas do casador que já decide, sem uma segunda regra:

| Forma acrescentada ao hash | Só aparece quando há intervenção? | Dias sem intervenção conservam os bytes? |
|---|---|---|
| Lista determinística dos pares **marco e batida preservados**. | Sim, se lista vazia for omitida. | Sim, se nenhuma outra parte da impressão mudar. |
| Índices determinísticos dos **marcos preservados**. | Sim, se lista vazia for omitida. | Sim, sob a mesma condição. |
| `cirineu:N`, com quantidade de preservações. | Sim, se zero for omitido. | Sim, sob a mesma condição. |

A lista de pares é a mais informativa; índice ou contagem bastam para fazer o **primeiro** hash antigo divergir quando o marcador aparece, mas sozinhos distinguem menos resultados posteriores. A garantia depende de capturar as intervenções reais dos dois caminhos e de uma serialização estável; isso ainda é desenho, **[não verificado]**. O hash é formado pela junção ordenada das partes, de modo que omitir integralmente a parte nova preserva a entrada antiga. [app/escala/utils.py:587; app/escala/utils.py:651; app/ponto/services/cartorio.py:556]

**Recorte necessário:** o marcador não pode simplesmente ser acrescentado a todas as datas com intervenção. A fila atual alcança exportadas; é preciso garantir a elegibilidade de competência aberta antes que a impressão nova provoque lavra, inclusive no caminho de evento. A forma concreta dessa proteção e sua prova de exportada intacta são **[não verificado]**. [app/ponto/management/commands/processar_cartorio.py:49; app/ponto/services/cartorio.py:2012]

### C. A impressão e a passada hoje

`impressao_insumos` monta SHA-256 das partes separadas por `|`: batidas, status da cobertura, chamados e versão/marcos do DNA; depois acrescenta partes opcionais. Os moldes pedidos são: [app/ponto/services/cartorio.py:471]

- `veto:<motivo>` se `veto` for verdadeiro. [app/ponto/services/cartorio.py:486]
- `teto:<valor>` se `teto is not None`, inclusive zero. [app/ponto/services/cartorio.py:491]
- `fio:<valor>` se `fio is not None`, inclusive `fio:0`. [app/ponto/services/cartorio.py:521]
- `cobparcial:1` para verdadeiro, ou `cobparcial:1:<estado>` para string; o estado pode conter `;disp=<índices>`. [app/ponto/services/cartorio.py:455; app/ponto/services/cartorio.py:532]

O ramo normal compara a impressão calculada com `cel.impressao` e pula se há julgamento e veredito compatível, salvo `forcar`; o ramo nunca-bateu tem sua própria comparação. A gravação passa por `carimbar`, no campo `CelulaDia.impressao`, `CharField` de até 64 caracteres. [app/ponto/services/cartorio.py:1257; app/ponto/services/cartorio.py:1398; app/ponto/portas/celula.py:1181; app/escala/models.py:1741]

O golden O219 fixa cinco hashes — vazio, uma batida, duas com chamado, veto e teto —, exige identidade com `regra=None`, verifica que versões distintas movem o hash, fixa a declaração atual de versões e usa AST para cobrar `regra` nas duas chamadas do corpo. Ele é uma proteção concreta, mas seus cinco exemplos não provam, sozinhos, todos os dias sem intervenção. [app/ponto/tests/test_o219_versao_da_regra.py:39; app/ponto/tests/test_o219_versao_da_regra.py:67; app/ponto/tests/test_o219_versao_da_regra.py:98; app/ponto/tests/test_o219_versao_da_regra.py:224]

O cron declarado é `processar_cartorio --apply`, às **06:27**, por empresa, sem `--forcar`, dependente de `reconciliar_grade`. Sua consulta não tem piso temporal: seleciona todas as células com `data < hoje`, por empresa e colaborador, inclusive já julgadas. **Não há filtro de competência exportada nessa consulta**; o próprio serviço adverte que o comando sem piso pode alcançar exportada. Logo, não se pode afirmar que a passada atual, por si, cumpre “exportada não se toca”. [app/config/crons.py:614; app/ponto/management/commands/processar_cartorio.py:45; app/ponto/management/commands/processar_cartorio.py:59; app/ponto/services/cartorio.py:957]

### D. Outros leitores do campo

Na busca textual dos usos Python do campo `CelulaDia.impressao`, além das comparações de pulo e da atribuição:

- `carimbar` também o compara para decidir se o conjunto do carimbo já está idempotente. [app/ponto/portas/celula.py:1187]
- `celula_veredito_velho` o inclui no snapshot de reversão do caminho `--aplicar`; o contador de divergência desse comando compara vereditos, não compara impressões. [app/ponto/management/commands/celula_veredito_velho.py:100; app/ponto/management/commands/celula_veredito_velho.py:166]
- Testes leem ou fixam o campo, por exemplo os de isenção, fio e a catraca que cataloga a coluna como saída. [app/ponto/tests/test_x1_folga_sem_adesao.py:148; app/ponto/tests/test_o261_cobrado_le_o_fio.py:179; app/escala/tests/test_d3_guarda_da_linha_celula.py:54]

Leitores indiretos, acesso dinâmico ou fora dos arquivos pesquisados: **[não verificado]**. Não identifiquei leitura direta desse campo nas buscas dos vigias, telas e tripwire; isso não demonstra que eles sejam imunes aos **efeitos da relavra**. [app/ponto/management/commands/tripwire_celulas.py:235; app/ponto/services/cartorio.py:1851]

### E. Contador do item 4 e julgamento em sombra

Existe um **molde parcial**: `celula_veredito_velho` compara o veredito gravado com o que a porta escreveria ao executar `julgar_colab(apply_=True, forcar=True)` dentro de transação e, no ensaio, pedir rollback. Ele conta mudanças de **veredito**, não todas as diferenças possíveis de ata ou julgamento; seleciona uma competência fornecida e não demonstra por si o recorte “aberta” nem filtra “sem fato novo”. É declarado como ensaio sob demanda, não cron. [app/ponto/management/commands/celula_veredito_velho.py:19; app/ponto/management/commands/celula_veredito_velho.py:74; app/ponto/management/commands/celula_veredito_velho.py:129; app/ponto/management/commands/celula_veredito_velho.py:166; app/config/crons.py:1094]

A função real é **`julgar_colab`**, com `forcar=True`, para atravessar a impressão; `julgar_celula` é sua porta unitária. O `apply_=False` fornece códigos no ramo normal, mas **não equivale ao julgamento completo gravável**: no ramo nunca-bateu, a classificação e a escolha da palavra estão dentro de `if apply_`. Por isso, o ensaio existente usa a porta real de lavra e rollback; não há prova nesta leitura de que rollback reverta efeitos externos eventualmente disparados durante o ensaio. [app/ponto/services/cartorio.py:940; app/ponto/services/cartorio.py:1267; app/ponto/services/cartorio.py:1464; app/ponto/services/cartorio.py:2012; app/ponto/management/commands/celula_veredito_velho.py:118]

### F. Moldes para o RED

- [test_cirineu_nao_desfaz_o_papel.py](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/tests/test_cirineu_nao_desfaz_o_papel.py:28): monta a geometria que a guarda preserva, os dois caminhos e as bordas por segundo.
- [test_e3_isento_na_impressao.py](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/tests/test_e3_isento_na_impressao.py:158): cria célula e batidas, carimba, demonstra o pulo, muda o insumo, verifica a nova impressão e usa mutante que remove o insumo.
- [test_o261_cobrado_le_o_fio.py](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/tests/test_o261_cobrado_le_o_fio.py:164): exercita passagem equivalente à do cron **sem `forcar`** e verifica que a célula deixa de ser pulada.
- [test_hx_cartorio_universo.py](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/tests/test_hx_cartorio_universo.py:34): molde de chamada do comando `processar_cartorio --apply`.

O RED específico deve plantar **impressão e palavra antigas para o mesmo conjunto de fatos**, demonstrar o pulo sem marcador e, com o marcador, a relavra sem `forcar`; o controle sem intervenção deve comparar o hash antigo byte a byte. O recorte exportado e o caminho de evento também precisam de prova própria. Esses testes específicos ainda são **[não verificado]**. [app/ponto/services/cartorio.py:1409; app/ponto/tests/test_o219_versao_da_regra.py:98]

**Arquivos que a cura tocaria — desenho provável:** [app/escala/utils.py](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/utils.py:502) para expor a intervenção real; [app/ponto/services/cartorio.py](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:471) para incorporá-la e respeitar o recorte; testes novos ou existentes em `app/escala/tests/` e `app/ponto/tests/`. Arquivos adicionais para o contador e para garantir o recorte de competência: **[não verificado]**.

**Juízes novos: 0**, neste desenho: reutilizam-se o casador e o cartório existentes. [app/escala/utils.py:595; app/ponto/services/cartorio.py:940]

**Perguntas de lei: 0.** A exclusão de exportada é uma condição expressa do aval fornecido; sua implementação e prova permanecem trabalho técnico em aberto **[não verificado]**.

## 18/11 18:18 · sessao 987d7175aba3 · TIPO-NA-FONTE · leitura · origem=Code

**A cópia fornecida não contém a porta da volta 5.** Nesta árvore, os dez leitores identificados acessam `orfas` ou `n_orfas` diretamente; a ata sintética também é montada diretamente. A busca por `leitura_orfas`, `horas_orfas`, `contar_orfas`, `gravar_orfas`, `instantes_orfas` e `copiar_ata` em `app/**/*.py` não encontrou ocorrências. A existência e o conteúdo de `app/escala/services/ata.py` nesta cópia: **[não verificado: arquivo ausente]**. Exemplos do código presente: [leitor_celula.py:187](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/services/leitor_celula.py:187), [cartorio.py:742](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:742).

O diretório recebido não tem `.git`; `git rev-parse HEAD` e `git log` falharam. Assim, **não foi possível verificar o HEAD de hoje, a ancestralidade ou atribuir mudanças a commits**. O hash informado no pedido identifica a cópia solicitada, mas sua identidade como HEAD atual permanece **[não verificado]**. O anexo da volta 5 descreve outra implementação, cuja presença no HEAD atual também fica **[não verificado]**. [nao verificado]

A entrega abaixo é, portanto, um **pré-desenho parcial sobre a árvore recebida**, com a comparação ao anexo explicitamente limitada. Não criei `logs/TIPO-NA-FONTE/item3/retomada/`, conforme a regra de somente leitura.

## 1. Censo: cópia recebida × pré-desenho

A busca AST literal em produção, excluindo testes e migrações, encontrou **zero chamadas aos acessores nomeados** e **14 operações de leitura crua da ata em dez arquivos**, listadas abaixo. Isso não é uma comparação de commits: as **12 chamadas em dez arquivos** são o censo da cópia da volta 5 transcrito no pedido; a relação histórica entre ela e a árvore recebida é **[não verificado]**. Leituras dinâmicas, aliases não identificados e chamadas indiretas: **[não verificado]**. [nao verificado]

| Leitor de ata | Leituras cruas presentes |
|---|---|
| Materialização | [materializacao.py:449](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/materializacao.py:449) |
| Juiz de chamado | [juizes.py:514](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/juizes.py:514), duas leituras |
| Leitor da grade | [leitor_celula.py:187](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/services/leitor_celula.py:187), [linha 190](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/services/leitor_celula.py:190) |
| Juiz de batida | [juiz_batida.py:161](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/juiz_batida.py:161) |
| Esmeril | [esmeril_espelho.py:125](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/esmeril_espelho.py:125) |
| Vigia | [vigia_pareador_celula.py:74](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/vigia_pareador_celula.py:74), duas leituras |
| Cartório | [cartorio.py:839](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:839) |
| Cluster | [cluster_espurio.py:29](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cluster_espurio.py:29), [linha 31](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cluster_espurio.py:31) |
| Tripwire | [tripwire_celulas.py:264](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/tripwire_celulas.py:264) |
| Cartão | [cartao_pela_celula.py:288](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/relatorios/cartao_pela_celula.py:288) |

Pelo mesmo critério do pré-desenho, as **33 leituras da grade/saída-dia** continuam reconhecíveis, com linhas deslocadas:

| Arquivo | Linhas |
|---|---|
| [calendario.py:409](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/colaboradores/services/calendario.py:409) | 409, 410 |
| [orfa_classe.py:58](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/orfa_classe.py:58) | 58 |
| [furos_diarios.py:22](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/services/furos_diarios.py:22) | 22 |
| [utils.py:810](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/utils.py:810) | 810, 813, 814, 1054, 1121, 1843, 1845, 1848, 1849, 1905, 1906, 1979, 1984 |
| [causas.py:43](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/folha/causas.py:43) | 43, 100 |
| [detectar_par_relampago.py:140](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/detectar_par_relampago.py:140) | 140 |
| [propor_reorganizacao.py:104](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/propor_reorganizacao.py:104) | 104 |
| [propositor.py:39](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/propositor.py:39) | 39 |
| [bordas_realizado.py:49](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/bordas_realizado.py:49) | 49 |
| [cartorio.py:736](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:736) | 736, 742, 745, 1426 |
| [simular_dia.py:12](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/simular_dia.py:12) | 12 |
| [status_dia.py:188](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/status_dia.py:188) | 188 |
| [supra_juiz.py:392](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/supra_juiz.py:392) | 392, 527 |
| [turnos.py:2679](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/turnos.py:2679) | 2679 |
| [pdf_espelho.py:477](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/relatorios/pdf_espelho.py:477) | 477, 552 |

**Correção da contagem:** a lista acima contém **34**, não 33: há quatro leituras da grade em `cartorio.py` (736, 742, 745 e 1426), enquanto o pré-desenho listava três. A quarta reconhecida aqui é a leitura usada na montagem da contagem em [cartorio.py:742](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:742). Se o censo anterior excluía essa ocorrência por outro critério semântico, essa intenção é **[não verificado]**. A origem por commit/obra da diferença é **[não verificado]**.

## 2. Matriz de ação dos leitores presentes

Os seis nomes são usados aqui como **rótulos analíticos da classificação descrita no anexo**, pois a implementação classificadora não está disponível nesta cópia: **[não verificado no HEAD]**. `N` = não lavrada; `S` = legado sem chaves; `C` = só contagem; `L` = só lista; `D` = descasada; `V` = lavrada. `v` é a lista/valor cru e `n` a contagem. As ações condicionais e erros por tipos inadequados são relevantes: a classificação antiga admitia valores inadequados em certos estados segundo o anexo; sua classificação atual é **[não verificado]**.

| Leitor | N | S | C | L | D | V |
|---|---|---|---|---|---|---|
| **Grade** | A chamada corrente recebe `{}` e devolve `[]` | `[]` | `['?'] * int(n)` | Converte elementos verdadeiros em strings; se resultado vazio, usa contagem | Lista convertida se houver; senão usa contagem; conversão pode falhar | Lista convertida; se vazia usa contagem. [leitor_celula.py:187](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/services/leitor_celula.py:187), [379](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/services/leitor_celula.py:379) |
| **Materialização** | Sem lâmpadas, pula | Sem lâmpadas, pula; com lâmpadas, nenhuma marca órfã | Nenhuma marca órfã | Filtra horas válidas e junta às marcas | Filtra o valor iterável; forma inadequada pode falhar | Filtra horas e junta às marcas. [materializacao.py:440](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/materializacao.py:440) |
| **Juiz de chamado** | Sem `lampadas`, retorna `None` | Sem `lampadas`, `None`; com elas pode chegar a falso | `n` verdadeiro aciona consulta à régua | `v` verdadeiro aciona consulta à régua | `v or n` verdadeiro aciona consulta à régua | Idem; lâmpada apagada pode decidir antes. [juizes.py:497](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/juizes.py:497) |
| **Cartão** | Conjunto vazio | Conjunto vazio | Conjunto vazio | Conjunto dos elementos string | Itera e filtra strings; forma inadequada pode falhar | Conjunto dos elementos string. [cartao_pela_celula.py:287](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/relatorios/cartao_pela_celula.py:287) |
| **Juiz de batida** | Lista vazia | Lista vazia | Lista vazia | Converte elementos a string | Itera e converte; forma inadequada pode falhar | Converte elementos a string. [juiz_batida.py:157](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/juiz_batida.py:157) |
| **Esmeril** | Nenhuma contribuição A5 | Nenhuma contribuição A5 | Nenhuma contribuição A5 | Converte horas e conta faixas | Itera; forma inadequada pode falhar | Conta faixas para A5. [esmeril_espelho.py:125](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/esmeril_espelho.py:125) |
| **Vigia** | Conjunto vazio | Conjunto vazio | `n` verdadeiro permite leitura de `v` ausente e resulta vazio | `n` ausente impede a leitura e descarta `v` | `n` verdadeiro → `set(v or ())`; falso → vazio | Contagem controla leitura e conjunto. [vigia_pareador_celula.py:73](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/vigia_pareador_celula.py:73) |
| **Cluster** | `None`, salvo guarda de dia sem trabalho | `None` se falta lista | `None` se falta lista | Se `v` e lâmpadas são listas, calcula assinatura; senão `None` | Depende dos tipos e conteúdo; conversão de hora pode falhar | Calcula assinatura ou `()`, conforme rajada e lâmpadas. [cluster_espurio.py:25](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cluster_espurio.py:25) |
| **Cartório/insumos** | Sem lâmpadas: `(False, False)` | Sem lâmpadas: `(False, False)`; com lâmpadas usa marcos | Sem lâmpadas: `n` verdadeiro → `(None, None)`; falso → `(False, False)` | Sem lâmpadas ignora lista e retorna `(False, False)` | Sem lâmpadas decide pela verdade de `n`; com lâmpadas usa marcos | Mesma decisão por lâmpadas e contagem. [cartorio.py:833](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:833) |
| **Tripwire** | Pula | `{}` pula; com ata e marcos suficientes, valor ausente vira vazio na peneira | Valor ausente vira vazio na peneira | Peneira `v` | Valor falso vira vazio; verdadeiro é iterado | Peneira a lista, se guarda permitir. [tripwire_celulas.py:258](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/tripwire_celulas.py:258), [165](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/tripwire_celulas.py:165) |

**Catraca — não LAVRADA que pode virar vazio:** grade em N/S e C com contagem falsa; materialização em S/C; cartão, juiz de batida e esmeril em N/S/C; vigia em N/S/C/L e D conforme contagem; tripwire em S/C/D conforme guarda e valor. Esses são os sítios da tabela acima, não uma decisão nova sobre o que cada estado deve passar a fazer. A adaptação futura e o DIFF de frota por leitor são **[não verificado]**.

## 3. Normalização da ata antes da leitura

Nos leitores da matriz, os sítios reconhecidos são [materializacao.py:440](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/materializacao.py:440), [juizes.py:497](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/juizes.py:497), [leitor_celula.py:379](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/services/leitor_celula.py:379), [juiz_batida.py:157](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/juiz_batida.py:157), [esmeril_espelho.py:125](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/esmeril_espelho.py:125), [vigia_pareador_celula.py:73](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/vigia_pareador_celula.py:73), [cluster_espurio.py:27](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cluster_espurio.py:27), [cartorio.py:833](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/services/cartorio.py:833) e [cartao_pela_celula.py:287](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/relatorios/cartao_pela_celula.py:287). O tripwire troca não-dict por `None` e pula na guarda, em [tripwire_celulas.py:258](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/tripwire_celulas.py:258).

Há outros `ata or {}` na árvore, inclusive [juizes.py:454](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/juizes.py:454), [tripwire_celulas.py:341](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/management/commands/tripwire_celulas.py:341) e [celula.py:1054](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/portas/celula.py:1054). Se cada um alcança a futura porta de órfãs, diretamente ou por transporte: **[não verificado]**; a lista de fluxo anterior à porta é, portanto, incompleta.

## 4. Ata sintética e universo do censo

A grade copia a célula em memória e atribui uma ata com `lampadas` e `orfas`, sem `n_orfas`, antes de passá-la a `turnos_do_colab`: [utils.py:1897](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/utils.py:1897), [1903](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/utils.py:1903), [1909](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/escala/utils.py:1909). O caminho de turnos chama o juiz de batida, que lê `ata.get('orfas')`: [turnos.py:1569](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/turnos.py:1569), [juiz_batida.py:161](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/juiz_batida.py:161).

Pela classificação transcrita no pré-desenho, essa forma seria `LEGADO_SO_LISTA`; passando pela porta da volta 5, lista e contagem seriam gravadas juntas. **O resultado concreto da porta e o universo do contador no HEAD são [não verificado]**, pois suas funções não estão na cópia recebida. O aval fornecido determina que a ata sintética passe pela porta e que o censo conte o mesmo universo que ela vê; essa é uma exigência de desenho, ainda sem implementação verificável aqui. **[não verificado em arquivo da cópia]**

## 5–7. Histórico, APIs e cópia de consulta

- **`ponto/portas/celula.py` desde `7621a973f`: [não verificado].** Sem objetos Git não é possível produzir log ou diff por commit, nem um resumo confiável por função. O arquivo presente pode ser lido, mas isso não demonstra seu delta: [celula.py:1054](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/ponto/portas/celula.py:1054).
- **`orfas_instantes`, `instantes_orfas`, `copiar_ata`:** a busca literal em `app/**/*.py` não encontrou definições nem chamadores na árvore recebida. Chamadores dinâmicos e a situação no HEAD real: **[não verificado]**. A regra L-111 está registrada em [CONDUTAS.md:15](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/CONDUTAS.md:15); o aval transcrito determina a retirada, mas nenhuma alteração foi feita. **[não verificado em arquivo da cópia]**
- **Base da cópia nova:** o aval fornecido determina HEAD de hoje; seu hash efetivo é **[não verificado]**. `wt-codex-tipo-na-fonte-184856` não estava no caminho consultado; o que ainda serve por arquivo e o que está velho são **[não verificado]**. O conteúdo transcrito de `predesenho.r`, `matriz_v5c.txt` e `veredito.md` serve como referência histórica fornecida, com linhas a reconferir na respectiva árvore; não prova o HEAD.

**Pergunta de lei:** o aval fornecido condiciona o uso do padrão “tipo soma” à existência do termo no glossário. Busquei `tipo soma`, `sum type`, `algebraic` e variantes em [GLOSSARIO.md:1](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/GLOSSARIO.md:1) e `PLANO-PADROES.md`; não encontrei essas formas. A resposta escrita à inclusão ou denominação desse padrão é **[não verificado]**. Qual termo do glossário deve reger o desenho, ou deve “tipo soma” ser incluído?

perguntas de lei: 1

## 18/11 18:18 · sessao d97fcbb47de4 · E4D-PROJECAO-UM-ESCRITOR · censo · origem=Code

Não consegui produzir um script que cumpra todo o contrato com segurança. Em particular, a cópia contém um `Q` construído e atribuído antes de ser aplicado ao queryset; atribuir seu modelo exige acompanhar esse fluxo ([models.py:2858](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:2858)). Também há um `getattr` cujo nome vem de uma tupla percorrida em laço ([historico.py:96](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/historico.py:96)).

O script abaixo é **uma parte utilizável do trabalho**, não o censo solicitado. Ele lista candidatos a `getattr` dinâmico quando uma das 25 colunas aparece como string literal em tupla ou lista no mesmo módulo. A associação é conservadora: a presença no módulo não prova que a string alcance o `getattr`, nem identifica o modelo ou o comportamento; por isso todas as linhas saem como `INDETERMINADO` [não verificado]. Ele não resolve receptores, templates, juízes, donos ou as classes FILTRA/MOSTRA/DECIDE, e não produz os totais solicitados por entidade [não verificado]. Não executei o script [não verificado]. [nao verificado]

```python
#!/usr/bin/env python3
"""Candidatos conservadores a getattr dinamico. Uso: python3 script.py COPIA.

Somente leitura. A saida nao e o censo E4d completo.
"""
import ast
import os
import sys
from collections import Counter

COLUNAS = {
    "status_local", "resolvido_em", "via_encerramento", "respondido_por",
    "fechada_em", "fechada_por", "resolucao", "motivo_fechamento",
    "reaberturas_admin", "reaberturas_colab", "motivos", "materializada_em",
    "materializada_por", "via_resolucao", "respondida_em", "resposta_colab",
    "validada_em", "validada_por", "recusa_motivo", "pendencia_admin",
    "data_conferida_em", "data_conferida_por", "veredito_lavrado",
    "julgado_em", "janela_expira_em",
}


def arquivos_python(raiz):
    for diretorio, subdirs, arquivos in os.walk(raiz):
        subdirs[:] = sorted(
            d for d in subdirs
            if d not in {".git", "__pycache__", "node_modules", "staticfiles"}
        )
        for nome in sorted(arquivos):
            if nome.endswith(".py"):
                yield os.path.join(diretorio, nome)


def candidatos_no_modulo(arvore):
    encontrados = set()
    for no in ast.walk(arvore):
        if isinstance(no, (ast.Tuple, ast.List)):
            for elemento in no.elts:
                if (isinstance(elemento, ast.Constant)
                        and isinstance(elemento.value, str)
                        and elemento.value in COLUNAS):
                    encontrados.add(elemento.value)
    return encontrados


class BuscaGetattr(ast.NodeVisitor):
    def __init__(self):
        self.escopo = []
        self.sitios = []

    def _escopo(self, no):
        self.escopo.append(no.name)
        self.generic_visit(no)
        self.escopo.pop()

    visit_FunctionDef = _escopo
    visit_AsyncFunctionDef = _escopo
    visit_ClassDef = _escopo

    def visit_Call(self, no):
        if (isinstance(no.func, ast.Name)
                and no.func.id == "getattr"
                and len(no.args) >= 2
                and not isinstance(no.args[1], ast.Constant)):
            self.sitios.append(
                (no.lineno, ".".join(self.escopo) or "<modulo>")
            )
        self.generic_visit(no)


def main():
    if len(sys.argv) != 2 or not os.path.isdir(sys.argv[1]):
        print("uso: python3 script.py COPIA", file=sys.stderr)
        return 2

    raiz = os.path.abspath(sys.argv[1])
    linhas = []
    erros = []

    for caminho in arquivos_python(raiz):
        rel = os.path.relpath(caminho, raiz).replace(os.sep, "/")
        try:
            with open(caminho, encoding="utf-8", errors="replace") as arquivo:
                arvore = ast.parse(arquivo.read(), filename=rel)
        except (OSError, SyntaxError, ValueError) as erro:
            erros.append((rel, str(erro)))
            continue

        colunas = candidatos_no_modulo(arvore)
        if not colunas:
            continue

        busca = BuscaGetattr()
        busca.visit(arvore)
        for linha, funcao in busca.sitios:
            for coluna in sorted(colunas):
                linhas.append(
                    ("INDETERMINADO", coluna, "INDETERMINADO",
                     f"{rel}:{linha}", funcao, "?")
                )

    for registro in sorted(linhas, key=lambda r: (r[3], r[1], r[4])):
        print("\t".join(registro))

    print("# TOTAL_CANDIDATOS\t" + str(len(linhas)))
    por_coluna = Counter(registro[1] for registro in linhas)
    for coluna, total in sorted(por_coluna.items()):
        print(f"# TOTAL_CANDIDATOS_COLUNA\t{coluna}\t{total}")

    for rel, erro in erros:
        print(f"ERRO_LEITURA_OU_PARSE\t{rel}\t{erro}", file=sys.stderr)
    return 1 if erros else 0


if __name__ == "__main__":
    raise SystemExit(main())
```

## 18/11 18:16 · sessao 5f0a28b44986 · E4D-PROJECAO-UM-ESCRITOR · leitura · origem=Code

A remedição **não está concluída**: não tenho uma classificação revisada dos 1.697 registros por receptor e comportamento, nem a lista por sítio dos 67 `getattr` dinâmicos. Por isso, os totais finais e a lista completa de juízes com selo (b) permanecem **[não verificado]**. A cópia disponível não contém metadados Git que permitam confirmar o HEAD ou o hash informado **[não verificado]**.

A amostra confirma que a saída bruta não pode ser usada como contagem final:

- **Homônimo de outro modelo:** `materializada_em` em [regularizacao_ext.py:219](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/regularizacao_ext.py:219) pertence ao queryset de `RegularizacaoExterna`, portanto deve sair da contagem das três entidades.
- **Chave POST, não coluna:** `resolucao` em [acoes_chamado.py:187](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/acoes_chamado.py:187) é uma chave de `request.POST`.
- **Indireção com receptor resolvido:** `trava_na_ordem` lê `fechada_em` de `DisputaSupervisao` e usa `status_local` no queryset de `ChamadoColaborador` em [models.py:2858](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:2858).
- **DECIDE, amostra:** `fechar_beco` verifica `d.fechada_em` antes de chamar `d.fechar` em [becos.py:55](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/becos.py:55).
- **MOSTRA, amostra:** o card exibe resposta e instante em [\_card_atendimento.html:106](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/templates/chamados/partials/_card_atendimento.html:106); a atribuição do objeto ao modelo, nesse trecho isolado, é **[não verificado]**.
- **Leitura dinâmica alcançável:** [historico.py:96](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/historico.py:96) obtém `PerguntaDisputa` e passa três nomes de colunas ao `getattr` da linha 105. Se esse sítio está entre os 67 da sonda é **[não verificado]**.

A tabela abaixo reproduz o **antes informado no anexo**; os valores do anexo e todos os valores “depois” não foram reconciliados sítio a sítio **[não verificado]**.

| Coluna | Antes informado | Depois |
|---|---:|---|
| status_local | 467 | [não verificado] |
| resolvido_em | 27 | [não verificado] |
| via_encerramento | 20 | [não verificado] |
| respondido_por | 8 | [não verificado] |
| fechada_em | 199 | [não verificado] |
| fechada_por | 2 | [não verificado] |
| resolucao | 25 | [não verificado] |
| motivo_fechamento | 18 | [não verificado] |
| reaberturas_admin | 6 | [não verificado] |
| reaberturas_colab | 11 | [não verificado] |
| motivos | 26 | [não verificado] |
| materializada_em | 232 | [não verificado] |
| materializada_por | 1 | [não verificado] |
| via_resolucao | 162 | [não verificado] |
| respondida_em | 86 | [não verificado] |
| resposta_colab | 157 | [não verificado] |
| validada_em | 187 | [não verificado] |
| validada_por | 7 | [não verificado] |
| recusa_motivo | 20 | [não verificado] |
| pendencia_admin | 5 | [não verificado] |
| data_conferida_em | 5 | [não verificado] |
| data_conferida_por | 1 | [não verificado] |
| veredito_lavrado | 15 | [não verificado] |
| julgado_em | 5 | [não verificado] |
| janela_expira_em | 5 | [não verificado] |
| **Total** | **1.697** | **[não verificado]** |

Quanto ao selo (b), um juiz registrado cuja leitura foi conferida é `pergunta_respondida`: o registro está em [core/juizes.py:306](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/core/juizes.py:306), e as leituras de `respondida_em` e `resposta_colab` estão em [chamados/juizes.py:1552](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/juizes.py:1552). Outro é `motivo_fora_da_fabrica`, registrado em [core/juizes.py:297](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/core/juizes.py:297), que lê `status_local` e filtra `materializada_em` em [disputa_emissao.py:769](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/disputa_emissao.py:769). **A lista completa e seu total são [não verificado].**

**Fecho:** DECIDE = **[não verificado]**; MOSTRA/FILTRA = **[não verificado]**; INDETERMINADO = **[não verificado]**; juízes que leem projeção = **[não verificado]**. A contagem de quantos dos 67 `getattr` podem alcançar projeção também é **[não verificado]**.

## 18/11 18:14 · sessao fdb6b644d62b · E4D-PROJECAO-UM-ESCRITOR · leitura · origem=Code

O desenho ainda **não está fechado para construção**: a sequência de decisões da pergunta já fornece parte da reconstrução, mas os atos existentes de chamado e de disputa não demonstram, nesta leitura, uma reconstrução completa e obrigatória das colunas. Em particular, `evento()` é *best-effort*: uma falha pode deixar a alteração de negócio seguir sem a linha de auditoria. [app/core/observ.py:94](app/core/observ.py:94), [app/chamados/models.py:298](app/chamados/models.py:298), [app/chamados/models.py:2100](app/chamados/models.py:2100).

Este é um **pré-desenho parcial**. O censo individual de leitores e a comprovação de todos os atos, autores, relógios e transações permanecem abertos; por isso, não apresento contagens inventadas nem trato o contador proposto como prova já existente. [nao verificado]

**Contrato comum proposto.** Cada projetor deve reconstruir os valores a partir de atos persistidos, escrever na mesma transação do ato e ter, no mesmo pouso, um contador por linha da frota: `reconstruido != gravado`, esperado zero, com um caso que provoque divergência. O selo AST deve impedir qualquer outro escritor das colunas projetadas, com allowlist zero; o selo de juízes deve identificar leituras de coluna por autoridades registradas. Essas exigências vêm dos itens 3 e 5 do aval fornecido; o texto raspado não tem arquivo:linha local conferido. [nao verificado] [nao verificado]

## PerguntaDisputa

| Coluna | Ato e derivação identificados | Porta e transação | Prova proposta e lacuna |
|---|---|---|---|
| `respondida_em` | `DecisaoPergunta.RESPOSTA.ato_em`; `RECUSA` e `REABERTURA` limpam o par. [app/chamados/models.py:4167](app/chamados/models.py:4167), [app/chamados/models.py:4319](app/chamados/models.py:4319) | Resposta humana, automática e por trilha registram decisão nos respectivos `atomic`; projetar depois do registro, ainda no `atomic` externo. [app/chamados/services/disputa_emissao.py:1517](app/chamados/services/disputa_emissao.py:1517), [app/chamados/services/disputa_emissao.py:1684](app/chamados/services/disputa_emissao.py:1684), [app/chamados/services/disputa_emissao.py:1706](app/chamados/services/disputa_emissao.py:1706) | Comparar por PK com a dobra da sequência, inclusive resposta posterior à validação e reabertura; cobertura integral do legado sem linha **[nao verificado]**. [app/chamados/models.py:4256](app/chamados/models.py:4256) |
| `resposta_colab` | `RESPOSTA.ato_texto`, com limpeza por `RECUSA`/`REABERTURA`. [app/chamados/models.py:4170](app/chamados/models.py:4170), [app/chamados/models.py:4322](app/chamados/models.py:4322) | Mesmas portas e transações do par anterior. [app/chamados/services/disputa_emissao.py:1532](app/chamados/services/disputa_emissao.py:1532) | Comparação por PK e caso de texto e instante inconsistentes; o tratamento D3 permanece aberto. [app/chamados/services/validacao.py:414](app/chamados/services/validacao.py:414) |
| `validada_em` | `VALIDACAO.ato_em`; `DEVOLUCAO`, `RECUSA`, `REABERTURA` e certas respostas posteriores retiram a validação. [app/chamados/models.py:4278](app/chamados/models.py:4278) | `_carimbar_validacao` e devolução registram no `atomic`; projetar após registrar. [app/chamados/services/validacao.py:170](app/chamados/services/validacao.py:170), [app/chamados/services/validacao.py:275](app/chamados/services/validacao.py:275) | Comparação por PK, exercitando `resposta_mantem_validacao` nas duas posições. [app/chamados/models.py:4195](app/chamados/models.py:4195) |
| `validada_por` | Candidato: `VALIDACAO.ato_por`, limpo pelos mesmos atos que retiram a validação. O método de leitura aberto retorna instante, mas não autor; a dobra completa do autor e seu legado são **[nao verificado]**. [app/chamados/models.py:4169](app/chamados/models.py:4169), [app/chamados/models.py:4295](app/chamados/models.py:4295) | Mesmas portas de validação e devolução; cobrir também correção em lote e ressuscitação. [app/chamados/services/validacao.py:170](app/chamados/services/validacao.py:170), [app/chamados/models.py:1702](app/chamados/models.py:1702) | Comparação por PK incluindo autor nulo e validações sucessivas; **[nao verificado]** se os atos históricos bastam. |
| `materializada_em` | O ato de conclusão ainda precisa nascer na materialização; o backlog exige choice em `DecisaoPergunta`, mesmo instante e backfill medido. Não há esse choice na lista aberta. [app/docs/BACKLOG.md:2015](app/docs/BACKLOG.md:2015), [app/chamados/models.py:4152](app/chamados/models.py:4152) | Materialização registra conclusão e chama o projetor no mesmo `atomic`; o encaixe para todas as vias e descarimbos é **[nao verificado]**. [app/chamados/models.py:3299](app/chamados/models.py:3299) | Comparar por PK, incluindo conclusão, desfazimento e legado; estoque em competência entregue e autorização para alcançá-lo **[nao verificado]**. [app/docs/BACKLOG.md:2015](app/docs/BACKLOG.md:2015) |
| `materializada_por` | Candidato: autor do ato de conclusão; escolha de campo e cobertura dos atos de identidade/desfazimento **[nao verificado]**. [app/chamados/models.py:3295](app/chamados/models.py:3295), [app/chamados/models.py:3364](app/chamados/models.py:3364) | Mesmo ponto transacional da conclusão e dos atos compensadores; fronteiras exatas **[nao verificado]**. | Comparação por PK com autores distintos e nulos; reconstruibilidade histórica **[nao verificado]**. |
| `via_resolucao` | A fonte única para todas as vias, inclusive relavra, identidade e desfazimento, **[nao verificado]**. Há escrita direta no carimbo e na relavra. [app/chamados/models.py:3295](app/chamados/models.py:3295), [app/chamados/services/relavra_via.py:125](app/chamados/services/relavra_via.py:125) | Os pontos de gravação precisam convergir para o projetor junto de seus atos; transações de todos os ramos **[nao verificado]**. | Comparação por PK com mudança de via sem mudança de instante; ato suficiente para cada ramo **[nao verificado]**. |

A leitura atual de decisões ainda incorpora, como passivo, resposta e validação da projeção sem linha correspondente. Portanto, usá-la diretamente como contador independente poderia comparar a coluna consigo mesma e esconder divergência; o contador precisa distinguir e resolver esse legado antes de afirmar reconstrução exclusiva dos atos. [app/chamados/models.py:4256](app/chamados/models.py:4256), [app/chamados/models.py:4309](app/chamados/models.py:4309).

**Escritores atuais a retirar das colunas projetadas**, sujeitos às exceções FATO e à solução dos atos faltantes: carimbo e carimbo de identidade, descarimbo individual e em lote, resposta humana/automática/por trilha, validação/devolução, correção, relavra e devoluções D1–D4. Os pontos confirmados incluem [models.py:3324](app/chamados/models.py:3324), [models.py:3378](app/chamados/models.py:3378), [models.py:3399](app/chamados/models.py:3399), [models.py:3485](app/chamados/models.py:3485), [disputa_emissao.py:1481](app/chamados/services/disputa_emissao.py:1481), [disputa_emissao.py:1617](app/chamados/services/disputa_emissao.py:1617), [disputa_emissao.py:1682](app/chamados/services/disputa_emissao.py:1682), [disputa_emissao.py:1705](app/chamados/services/disputa_emissao.py:1705), [validacao.py:168](app/chamados/services/validacao.py:168), [validacao.py:273](app/chamados/services/validacao.py:273), [validacao.py:346](app/chamados/services/validacao.py:346), [models.py:1702](app/chamados/models.py:1702), [validacao.py:440](app/chamados/services/validacao.py:440) e [relavra_via.py:246](app/chamados/services/relavra_via.py:246). A exaustividade, inclusive nomes dinâmicos e chamadores, **[nao verificado]**.

**R2 fora da projeção por enquanto:** `recusa_motivo`, `pendencia_admin`, `data_conferida_em` e `data_conferida_por`: o aval fornecido classifica essas colunas como sem fato próprio para a E4d; a existência de ocorrência ou auditoria em algum ramo não prova fonte completa para todos os ramos. [app/chamados/services/materializacao.py:1365](app/chamados/services/materializacao.py:1365), [app/chamados/services/validacao.py:916](app/chamados/services/validacao.py:916), [app/chamados/services/validacao.py:996](app/chamados/services/validacao.py:996), [nao verificado].

O cache de lavra `veredito_lavrado`, `julgado_em` e `janela_expira_em` permanece separado dessa matriz de atos; seus escritores L1–L5 precisam ser examinados para escritor único, conforme a classe R3 do aval fornecido. [app/chamados/veredito_lavrado.py:250](app/chamados/veredito_lavrado.py:250), [app/chamados/services/fio.py:975](app/chamados/services/fio.py:975), [nao verificado].

## DisputaSupervisao e RAIZ-DA-DISPUTA

| Coluna | Ato candidato e porta | Transação e prova proposta |
|---|---|---|
| `fechada_em` | `LogAuditoria` de ação `disputa_fechada`: seu relógio `criado_em` é candidato, mas é gerado depois do relógio gravado na coluna; igualdade exata não está demonstrada. [app/chamados/models.py:2095](app/chamados/models.py:2095), [app/core/models.py:307](app/core/models.py:307) | `fechar` tem `atomic`, mas o evento é best-effort; antes de projetar, é necessário um ato obrigatório com o mesmo relógio. Contador por PK e teste de falha de gravação do ato. [app/chamados/models.py:2068](app/chamados/models.py:2068), [app/core/observ.py:98](app/core/observ.py:98) |
| `fechada_por` | Candidato: `LogAuditoria.usuario` do fechamento; equivalência para todos os atores e histórico **[nao verificado]**. [app/core/models.py:295](app/core/models.py:295), [app/chamados/models.py:2100](app/chamados/models.py:2100) | Mesma lacuna de obrigatoriedade; comparar por PK inclusive ator nulo. [app/core/observ.py:98](app/core/observ.py:98) |
| `resolucao` | Candidato: `valor_depois.resolucao` do fechamento; reabertura e limpeza residual também alteram o valor e precisam de trilha suficiente. [app/chamados/models.py:2103](app/chamados/models.py:2103), [app/chamados/models.py:2338](app/chamados/models.py:2338), [app/chamados/models.py:2428](app/chamados/models.py:2428) | Comparar por PK depois de fechar, reabrir e limpar resíduo; ato obrigatório em todos os ramos **[nao verificado]**. |
| `motivo_fechamento` | Candidato: `valor_depois.motivo_fechamento`, limpo na reabertura. [app/chamados/models.py:2103](app/chamados/models.py:2103), [app/chamados/models.py:2339](app/chamados/models.py:2339) | Mesma necessidade de ato obrigatório e comparação por PK após cada ciclo. [app/core/observ.py:98](app/core/observ.py:98) |
| `reaberturas_colab` | Ocorrência `pedido_de_correcao` é candidata; comprovação de uma ocorrência obrigatória por incremento e reconstrução histórica **[nao verificado]**. [app/chamados/models.py:2402](app/chamados/models.py:2402), [app/chamados/models.py:2407](app/chamados/models.py:2407) | Projetar na porta de reabertura do colaborador, no mesmo `atomic` do ato; atomicidade integral dessa porta **[nao verificado]**. Contar por PK e testar repetição/no-op. |

**Escritores a substituir nessa fatia:** `fechar`, `reabrir_admin`, `reabrir_sistema` e `limpar_resolucao_residual` nos quatro campos de fechamento, conforme aplicável, e `reabrir_por_colab` no contador. A reabertura administrativa continua com a exceção FATO A03 declarada no aval fornecido: sua mudança depende da fonte própria, cuja forma e momento **[nao verificado]**. [app/chamados/models.py:2095](app/chamados/models.py:2095), [app/chamados/models.py:2312](app/chamados/models.py:2312), [app/chamados/models.py:2336](app/chamados/models.py:2336), [app/chamados/models.py:2402](app/chamados/models.py:2402), [app/chamados/models.py:2428](app/chamados/models.py:2428), [nao verificado].

**R2:** `reaberturas_admin` e `motivos` ficam fora até fato próprio; o primeiro é incrementado em A03 e o segundo também é atualizado em outras portas, sem fonte substituta completa demonstrada. [app/chamados/models.py:2316](app/chamados/models.py:2316), [app/chamados/models.py:2129](app/chamados/models.py:2129), [app/chamados/services/disputa_emissao.py:550](app/chamados/services/disputa_emissao.py:550), [nao verificado].

### Matriz conjunta da raiz e do projetor

| Evento | RAIZ decide | PROJETOR grava |
|---|---|---|
| Nascer | Aplicar o contrato de nascimento e vínculo com filhos; o tratamento de órfãos e dona fechada ainda exige conferir as decisões F1 e seus números atuais. [app/docs/BACKLOG.md:2113](app/docs/BACKLOG.md:2113), [nao verificado] | Estado inicial correspondente ao ato de nascimento; ato suficiente e chamada exata **[nao verificado]**. |
| Filho muda | No fim de `carimbar` e de `DecisaoPergunta.registrar`, no mesmo `atomic`, reavaliar `pode_fechar`; se puder, chamar `fechar`. Isso é o desenho solicitado no complemento fornecido, ainda não demonstrado como implementação atual. [app/chamados/models.py:2009](app/chamados/models.py:2009), [app/chamados/models.py:4329](app/chamados/models.py:4329), [nao verificado] | Depois do ato de fechamento obrigatório, projetar os quatro campos do fechamento; não gravar fechamento só porque um filho mudou. [app/chamados/models.py:2095](app/chamados/models.py:2095) |
| Par | A regra do par decide o efeito dos filhos; a devolução BUG-PAR usa `DEVOLUCAO`, distinta da reabertura de ressuscitação. A suficiência da BUG-PAR como F4 da raiz ainda exige censo. [app/chamados/services/validacao.py:308](app/chamados/services/validacao.py:308), [app/chamados/models.py:4180](app/chamados/models.py:4180), [nao verificado] | Projetar a pergunta a partir do ato compensador; projetar disputa apenas se a raiz decidir mudança do ciclo. [app/chamados/services/validacao.py:355](app/chamados/services/validacao.py:355) |
| Fechar | `pode_fechar` exige ausência de resposta sem veredito e de fio vivo; `fechar` aplica suas guardas e exceções. [app/chamados/models.py:2009](app/chamados/models.py:2009), [app/chamados/models.py:2017](app/chamados/models.py:2017) | Gravar os quatro campos a partir do ato obrigatório de fechamento, ainda não demonstrado suficiente. [app/chamados/models.py:2095](app/chamados/models.py:2095), [app/core/observ.py:98](app/core/observ.py:98) |
| Reabrir | Distinguir reabertura administrativa, de sistema e pedido do colaborador; a administrativa tem limite e fato próprio pendente. [app/chamados/models.py:2289](app/chamados/models.py:2289), [app/chamados/models.py:2326](app/chamados/models.py:2326), [app/chamados/models.py:2345](app/chamados/models.py:2345) | Limpar os campos de fechamento ou incrementar o contador correspondente **somente quando houver ato suficiente**; a ocorrência de sistema é condicional à existência de chamado. [app/chamados/models.py:2340](app/chamados/models.py:2340) |
| Tranca | `fechar` distingue a exceção de competência trancada e declara que ela não carimba as perguntas. [app/chamados/models.py:2027](app/chamados/models.py:2027) | Projetar o fechamento decidido pela porta, sem fabricar carimbo de filho; ato de fechamento suficiente **[nao verificado]**. |
| Desligamento | A porta excepcional pode encerrar perguntas mudas e registrar `SILENCIO`, mantendo a guarda de resposta sem veredito. [app/chamados/models.py:2027](app/chamados/models.py:2027), [app/chamados/models.py:2082](app/chamados/models.py:2082) | Projetar cada filho a partir de seus atos e a disputa a partir do fechamento; ordem e prevenção de reentrada no gancho F3 **[nao verificado]**. |

**Pergunta 29 / fronteira F5.** A proposta é F3 possuir a reação da raiz e a decisão de chamar `fechar`; F5/E4d possuir a substituição das gravações diretas e os selos, em conjunto, para que a escrita direta de `fechar` e das reaberturas acima morra uma vez só. O acoplamento concreto entre o gancho da raiz e os projetores de pergunta e disputa, inclusive reentrada e ordem de travas, ainda precisa ser demonstrado; o backlog registra F5 com E4d e F3 antes dela. [app/docs/BACKLOG.md:1904](app/docs/BACKLOG.md:1904), [app/docs/BACKLOG.md:2113](app/docs/BACKLOG.md:2113), [app/chamados/models.py:2069](app/chamados/models.py:2069).

## ChamadoColaborador

| Coluna | Ato candidato | Porta, transação e prova proposta |
|---|---|---|
| `status_local` | `LogAuditoria.valor_depois.status_local` de `estado_chamado`; nascimento usa caminho próprio. [app/chamados/models.py:298](app/chamados/models.py:298), [app/chamados/models.py:1274](app/chamados/models.py:1274) | `_gravar_estado`, dentro do `atomic` de [models.py:286](app/chamados/models.py:286); o evento precisa deixar de ser fonte best-effort se for a fonte do projetor. Contador por PK deve cobrir nascimento e transições. [app/core/observ.py:98](app/core/observ.py:98) |
| `resolvido_em` | A transição grava `timezone.now()` e preserva instante entre estados encerrados; o evento aberto não demonstra guardar esse mesmo instante. [app/chamados/models.py:248](app/chamados/models.py:248), [app/chamados/models.py:298](app/chamados/models.py:298) | Projetar na mesma transação da transição, após ato com relógio suficiente; comparar por PK e testar transição encerrado→encerrado. |
| `via_encerramento` | Candidata: `valor_depois.via` de `estado_chamado`; verificar normalização do vazio e nascimento. [app/chamados/models.py:299](app/chamados/models.py:299) | Mesmo `atomic`; comparar por PK depois de encerrar e reabrir, condicionado à garantia de persistência do ato. [app/core/observ.py:98](app/core/observ.py:98) |
| `respondido_por` | A porta só altera quando `set_user=True`; a suficiência do ator do evento para distinguir “não alterar” de “alterar para nulo” **[nao verificado]**. [app/chamados/models.py:236](app/chamados/models.py:236), [app/chamados/models.py:301](app/chamados/models.py:301) | Mesmo `atomic`; contador por PK precisa cobrir ambos os valores de `set_user` e atores nulos. |

**Escritores atuais:** `_gravar_estado` nos quatro campos; defaults de `abrir` e `criar` em `status_local` devem passar a refletir o estado inicial do projetor. As migrations históricas 0038/0039 são mantidas pelo aval fornecido, portanto não devem ser apresentadas como código histórico a editar para satisfazer o selo de execução atual. [app/chamados/models.py:234](app/chamados/models.py:234), [app/chamados/models.py:1000](app/chamados/models.py:1000), [app/chamados/models.py:1154](app/chamados/models.py:1154), [app/chamados/migrations/0038_a3_registrado_backfill.py:10](app/chamados/migrations/0038_a3_registrado_backfill.py:10), [app/chamados/migrations/0039_a3_via_backfill.py:24](app/chamados/migrations/0039_a3_via_backfill.py:24), [nao verificado].

## Leitores e selo de juízes

A **remedida pedida não foi concluída**: o anexo fornecido contém totais agregados por nome, sem a lista de arquivo:linha por leitor, e informa 67 `getattr` dinâmicos não atribuídos; assim, os números 1.588 e 1.336 não são contagens remedidas de DECIDE e MOSTRA/FILTRA. A contagem de cada classe por coluna e a lista completa de sítios permanecem **[nao verificado]**.

O critério para concluir a remedida é identificar o modelo pelo receptor, queryset/manager, relação ORM ou contexto do template; quando a origem não puder ser resolvida, manter o sítio como indeterminado em vez de atribuí-lo pelo nome. Por exemplo, `reaberturas_colab` em `fio.py` é anotação em memória sobre chamado, não escrita da coluna homônima da disputa. [app/chamados/services/fio.py:390](app/chamados/services/fio.py:390), [app/core/contratos_estruturais.py:719](app/core/contratos_estruturais.py:719).

**Juízes registrados com leitura de projeção confirmada nesta amostra, para o selo (b):**

- C21 `pergunta_respondida` lê `respondida_em` e `resposta_colab` do objeto recebido; é preciso distinguir chamadas com objeto projetado das que recebem leitura do ato. [app/core/juizes.py:306](app/core/juizes.py:306), [app/chamados/juizes.py:1544](app/chamados/juizes.py:1544).
- C6 `pergunta_viva` utiliza a projeção temporal que acessa resposta, validação, materialização e via; o caminho concreto de cada chamador permanece a medir. [app/core/juizes.py:291](app/core/juizes.py:291), [app/chamados/juizes.py:1382](app/chamados/juizes.py:1382).
- `DisputaSupervisao.respostas_sem_veredito`, regist

[truncado em 20000 de 22625 caracteres]

## 18/11 18:10 · sessao 6f3a3060a3a3 · RAIZ-DA-DISPUTA · leitura · origem=Code

**F4: PARCIAL.** A BUG-PAR reúne a validação e o plantio das duas metades em um ato, mas o agrupamento identificado no código usa disputa, dia e motivos complementares; ele não lê os pares de marcos da célula para escolher a irmã. Assim, a parte “o par pela célula” ainda falta. ([materializacao.py:136](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/materializacao.py:136), [materializacao.py:139](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/materializacao.py:139), [validacao.py:613](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/validacao.py:613), [BACKLOG.md:2162](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/BACKLOG.md:2162).)

A cópia não contém metadados Git; portanto, sua identidade com o HEAD informado e a atribuição de cada alteração a um commit são **[nao verificado]**. Os números atuais de produção e os fatos históricos do CASO-0711 também são **[nao verificado]**. A reconferência abaixo cobre os pontos que consegui confrontar; **não completa a verificação individual de todas as linhas e cadeias dos anexos**. [nao verificado]

**F0 e F1 — reconferência dos pontos principais**

| Afirmação anterior | Situação na cópia |
|---|---|
| Três sítios persistentes de destino em duas funções; reuso preserva a dona | **Ainda vale nos sítios conferidos:** `_nascer` atribui no reuso e no construtor; `reapontar_resolvedora` atribui e salva. O selo continua permitindo os dois arquivos, não uma função única. ([models.py:3174](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:3174), [models.py:3206](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:3206), [fio.py:974](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/fio.py:974), [test_contract_escritores_estado.py:61](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/core/tests/test_contract_escritores_estado.py:61).) Exaustividade do censo atual de escritores: **[nao verificado]**. |
| Disputa aberta é `fechada_em is None`; há dono, destino e chamado-pai distintos | **Vale**, com linhas deslocadas. ([models.py:1886](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:1886), [models.py:1960](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:1960), [models.py:3206](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:3206).) |
| Núcleo pode criar disputa antes de gerar pergunta; agrupador pode criar e depois tentar fechar vazia | **Vale:** o núcleo cria a disputa e retorna antes de o wrapper gerar perguntas; o agrupador cria antes do laço e, ao final, consulta C7. O protocolo c21 de ensaio/replay não foi identificado nesses caminhos. ([disputa_emissao.py:601](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/disputa_emissao.py:601), [disputa_emissao.py:676](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/disputa_emissao.py:676), [disputa_emissao.py:1137](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/disputa_emissao.py:1137), [disputa_emissao.py:1292](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/disputa_emissao.py:1292).) |
| Seletor comum escolhe disputa aberta do colaborador sem filtro por dia | **Vale para o núcleo e o agrupador conferidos.** ([disputa_emissao.py:540](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/disputa_emissao.py:540), [disputa_emissao.py:1110](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/disputa_emissao.py:1110).) |
| Há eleições distintas de destino | **Vale nos caminhos conferidos:** o derivador exige candidato único, enquanto o caminho de intervalo do agrupador entrega o chamado corrente. ([disputa_emissao.py:166](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/disputa_emissao.py:166), [disputa_emissao.py:236](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/disputa_emissao.py:236), [disputa_emissao.py:1152](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/disputa_emissao.py:1152).) |
| Escape genérico `descarimbar_em_lote(**outros)` | **Ainda existe**, mas mudou: há guardas adicionais para o par resposta/instante e para a cerca; o `update` ainda expande `outros`. O comentário atribui essas guardas à E3 e à E4c F1/B2. ([models.py:3448](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:3448), [models.py:3456](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:3456), [models.py:3477](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:3477), [models.py:3485](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:3485).) |
| Limpeza de resolução residual, acrescentada ao inventário pelo CASO-0711 | **Continua existindo.** ([models.py:2413](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:2413).) |
| Elo F3 da raiz ao fim de `carimbar` e `registrar` | **Não aparece nos corpos conferidos:** `carimbar` salva e retorna; `registrar` grava decisões, acorda células e retorna. Isso não prova a ausência de reação por toda cadeia indireta. ([models.py:3342](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:3342), [models.py:4329](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:4329).) |

Há mudanças identificáveis da E4c na resposta: `respostas_sem_veredito` passou a consultar `pergunta_respondida`, e o nascimento registra atos de projeção no mesmo fluxo. A atribuição de **cada** mudança à E4c F1/F2/F4 ou à F6F7, com seu commit, permanece **[nao verificado]**. ([models.py:1989](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:1989), [models.py:3210](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:3210).) O registro disponível informa o merge `41f5648af` para a volta da BUG-PAR; a genealogia Git independente desse merge nesta cópia é **[nao verificado]**. ([BACKLOG.md:2142](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/BACKLOG.md:2142).)

**Censo do par.** `metade_irma_do_intervalo` procura motivo complementar na mesma disputa e data, exige resposta e ausência de materialização e recusa ambiguidade; `par_do_intervalo` usa essa irmã para julgar o ato, e `_validar_o_par` usa o resultado para validar e materializar sob savepoint. ([materializacao.py:139](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/materializacao.py:139), [materializacao.py:175](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/materializacao.py:175), [validacao.py:632](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/validacao.py:632).)

**Há uma segunda busca de irmã**, `_situacao_da_irma`: para diagnóstico, também procura mesma disputa/dia e motivo complementar, mas escolhe a primeira por PK; não é o agrupador do ato de plantio. ([materializacao.py:281](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/materializacao.py:281).) Falta fazer a seleção do par pelos marcos da célula, tratar a ausência de marco conforme cadastro e reconciliar essa segunda busca com a mesma autoridade; ausência de outros agrupadores por todas as formas indiretas é **[nao verificado]**. ([BACKLOG.md:2162](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/BACKLOG.md:2162).)

**Decisões de F1 registradas:**

1. **Órfãos sem disputa:** a opção de não nascer disputa está registrada, e resta identificar quem fecha os órfãos sem disputa. Não localizei, nas passagens conferidas de `LEIS.md` e `CORTES.md`, uma resposta específica ao responsável; exaustividade dessa busca: **[nao verificado]**. ([BACKLOG.md:2113](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/BACKLOG.md:2113).)
2. **Motivo novo cuja dona está fechada:** reabrir, criar outra ou encaminhar à Pauta. O corte posterior registra essa questão como insumo N5 da raiz; não encontrei resposta específica nas passagens conferidas. O corte sobre perguntas vivas em disputa fechada resolve a apresentação e validação dessas perguntas, mas não demonstra resposta para a chegada de motivo novo. Exaustividade da busca: **[nao verificado]**. ([CORTES.md:515](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/CORTES.md:515), [CORTES.md:415](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/docs/CORTES.md:415).)

Não posso fechar um número confiável sem concluir a busca por leis e cortes aplicáveis às duas decisões. **[nao verificado]**

perguntas de lei: [nao verificado]

## 18/11 18:08 · sessao 8eb156857230 · RAIZ-DA-DISPUTA · sonda · origem=Code

**A remedição não foi executada:** o enunciado informa que não há acesso a banco ou logs, e o envelope `--rodar` exige arquivo em `logs/sombra/`, grava saída e chama `lavra_de_prod`; isso conflita com a regra de não criar nem escrever arquivos. ([bin/sombra.sh](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/bin/sombra.sh:483), [bin/sombra.sh](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/bin/sombra.sh:136); disponibilidade da sombra agora: [nao verificado].)

A sonda abaixo está preparada para a sessão, mas sua execução e seus resultados atuais são **[nao verificado]**. O dado fornecido informa `antes=23` para N2 e `antes=94` para N3; os números anteriores de N1 e N4 não aparecem no dado fornecido: **[nao verificado]**.

Funções reais usadas:

- **N1:** consulta `DisputaSupervisao` aberta e seu relacionamento `perguntas`; C7 considera a lista de perguntas da disputa. ([models.py](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:2009).)
- **N2:** `DisputaSupervisao.pode_fechar()`, o C7 real, que chama os leitores de resposta sem veredito e de fio vivo. ([models.py](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:2009).)
- **N3:** `metade_irma_do_intervalo()` seleciona a irmã; `pergunta_viva()` julga C6; `DecisaoPergunta.leitura()` lê a validação vigente. ([materializacao.py](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/materializacao.py:139), [juizes.py](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/juizes.py:866), [models.py](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/models.py:4295).) O agrupamento **mudou**: o anexo agrupava por colaborador/data e posição, mesmo entre disputas; o HEAD procura motivo complementar na **mesma disputa e data**, com irmã de tipo hora, respondida com texto, não materializada, dentro do fato e única entre candidatas. A sonda parte da metade com validação vigente e conta cada par selecionado uma vez; por isso `94` é referência histórica, não comparação de universos equivalentes. (Composição antiga: [nao verificado]; [materializacao.py](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/materializacao.py:139).) `par_do_intervalo()` aplica ainda condições de plantio e paridade; não é o seletor geral de irmã para esta contagem. ([materializacao.py](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/materializacao.py:175).)
- **N4:** `grupos_de_colisao()`, `universo_do_grupo()` e `canonica_do_grupo()`. Aqui “duplicadas” significa **perguntas não canônicas em disputa aberta** nos grupos detectados pelo leitor de colisão, mesmo se a canônica estiver em outra disputa. O leitor aplica suas chaves por família, horizonte e exclusões; não é uma contagem de todas as linhas historicamente rotuladas com via `duplicata`, nem afirma que toda perdedora possa ser retratada. ([fio.py](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/fio.py:1515), [fio.py](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/fio.py:2087), [fio.py](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/fio.py:1274), [fio.py](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/fio.py:1311).)

A sonda recusa qualquer alias adicional, banco diferente de `sombra` ou schema diferente de `juliani`; abre transação PostgreSQL `REPEATABLE READ, READ ONLY` e bloqueia SQL que não comece por `SELECT` após a guarda. Qualquer exceção torna os quatro resultados inconclusivos e é contada por tipo; mensagens de exceção não são emitidas. O método de seleção de irmã pode omitir grupos ambíguos por contrato, e o C6 possui tratamento interno de algumas exceções que a sonda externa não consegue contar. ([materializacao.py](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/services/materializacao.py:139), [juizes.py](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/chamados/juizes.py:1481).)

```python
# Executar apenas numa sessao Django ja inicializada com
# DJANGO_SETTINGS_MODULE=config.settings.sombra.
# Nenhum arquivo e criado por esta sonda.
import logging
import os
from collections import Counter

erros = Counter()
resultados = None
log_anterior = logging.root.manager.disable

try:
    from django.db import connections, transaction
    from django_tenants.utils import schema_context
    from chamados.models import (
        DecisaoPergunta,
        DisputaSupervisao,
        PerguntaDisputa,
    )
    from chamados.juizes import pergunta_viva
    from chamados.services.materializacao import metade_irma_do_intervalo
    from chamados.services.fio import (
        canonica_do_grupo,
        grupos_de_colisao,
        universo_do_grupo,
    )

    logging.disable(logging.CRITICAL)

    if os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra":
        raise RuntimeError("settings incorretos")
    if set(connections) != {"default"}:
        raise RuntimeError("aliases inesperados")

    conn = connections["default"]
    if conn.vendor != "postgresql" or conn.settings_dict["NAME"] != "sombra":
        raise RuntimeError("configuracao de banco incorreta")
    if conn.in_atomic_block:
        raise RuntimeError("transacao preexistente")

    def somente_select(execute, sql, params, many, context):
        partes = sql.lstrip().split(None, 1)
        if not partes or partes[0].upper() != "SELECT":
            raise RuntimeError("SQL nao permitido")
        return execute(sql, params, many, context)

    with schema_context("juliani"):
        with transaction.atomic(using="default"):
            with conn.cursor() as cur:
                cur.execute(
                    "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
                )
                cur.execute(
                    "SELECT current_database(), current_schema(), "
                    "current_setting('transaction_read_only')"
                )
                banco, schema, somente_leitura = cur.fetchone()
                if (banco, schema, somente_leitura) != (
                    "sombra", "juliani", "on"
                ):
                    raise RuntimeError("guarda de banco ou schema falhou")

            with conn.execute_wrapper(somente_select):
                abertas = list(
                    DisputaSupervisao.objects.filter(
                        fechada_em__isnull=True
                    )
                    .select_related("colaborador")
                    .prefetch_related("perguntas")
                    .order_by("pk")
                )

                n1 = sum(1 for d in abertas if not d.perguntas.all())
                n2 = sum(1 for d in abertas if d.pode_fechar() is True)

                # Parte da metade com validacao vigente. A outra metade e
                # selecionada pelo proprio leitor do HEAD e julgada pelo C6.
                # Nao restringe o universo a disputas abertas: a sonda antiga
                # tambem nao o restringia.
                pares = set()
                candidatas = (
                    PerguntaDisputa.objects.filter(
                        motivo__in=("intervalo_saida", "intervalo_volta")
                    )
                    .select_related("disputa__colaborador", "celula")
                    .prefetch_related("decisoes")
                    .order_by("pk")
                )
                for p in candidatas.iterator(chunk_size=500):
                    if DecisaoPergunta.leitura(p).validada_em is None:
                        continue
                    q = metade_irma_do_intervalo(p.disputa, p)
                    if q is not None and pergunta_viva(q) is True:
                        pares.add(tuple(sorted((p.pk, q.pk))))
                n3 = len(pares)

                # Excedentes nao canonicos que pertencem a disputas abertas.
                # A escolha da canonica usa o universo inteiro do grupo.
                duplicadas = set()
                for _familia, _chave, pks in grupos_de_colisao():
                    grupo = universo_do_grupo(pks)
                    if len(grupo) < 2:
                        continue
                    canonica = canonica_do_grupo(grupo)
                    for p in grupo:
                        if (
                            p.pk != canonica.pk
                            and p.disputa_id is not None
                            and p.disputa.fechada_em is None
                        ):
                            duplicadas.add(p.pk)
                n4 = len(duplicadas)

                resultados = (n1, n2, n3, n4)

except Exception as exc:
    # Nao imprime texto da excecao: pode conter dados pessoais.
    nome_tipo = type(exc).__name__
    if not nome_tipo.isidentifier():
        nome_tipo = "Erro"
    erros[nome_tipo] += 1
    resultados = None

finally:
    logging.disable(log_anterior)

antes = ("[nao verificado]", "23", "94", "[nao verificado]")
for indice in range(4):
    valor = "INCONCLUSIVO" if resultados is None else str(resultados[indice])
    print("N%d=%s antes=%s" % (indice + 1, valor, antes[indice]))

for tipo, quantidade in sorted(erros.items()):
    print("ERRO_TIPO=%s ERROS=%d" % (tipo, quantidade))
print("ERROS=%d" % sum(erros.values()))
print("FIM_REMEDIDA_1011")
```

## 18/11 18:07 · sessao 1e6ace81736a · B2B-RETAGUARDA-1811-1810 · leitura · origem=Code

**Há ações condicionais disponíveis ao admin**: para uma pergunta respondida e não validada, o card pode oferecer **Rejeitar**; o modal também pode oferecer **Corrigir** e **Conferir data**. A validação dessa pergunta é escondida e a porta a recusa pelo K6. O dado fornecido não identifica uma pergunta, suas permissões, se a disputa está aberta ou se as datas divergem; a disponibilidade no caso concreto é **[não verificado]**. (`app/templates/chamados/partials/_pergunta_card.html:42`, `:47`, `:117`; `app/templates/chamados/partials/modal_fio.html:162`, `:274`, `:316`; `app/chamados/services/validacao.py:740`)

**Origem e leitores.** `estado_aplicacao_pergunta` deriva o estado para pergunta de resposta `hora`, respondida e ainda não materializada; consulta `entrega_que_barra_a_validacao`, que usa o K6 para metades de intervalo. O K6 consulta a competência do **dia do fato** e devolve `bloqueada_competencia_entregue` quando há exportação vigente não invalidada. O detalhe exibível diz que a pergunta está preservada para Pauta DP; essa frase, por si, não comprova a criação de uma pauta. (`app/chamados/services/materializacao.py:788`, `:804`, `:811`, `:865`, `:892`; `app/chamados/services/validacao.py:209`; `app/folha/export.py:580`)

O card lê a testemunha pelo filtro `aplicacao_da_resposta`; o fio a calcula ao montar o contexto. A própria porta de validação consulta a mesma composição antes do carimbo e volta a conferi-la depois da tentativa de materialização, com rollback se necessário. (`app/chamados/templatetags/chamados_tags.py:131`; `app/chamados/services/fio.py:375`; `app/chamados/services/validacao.py:729`, `:776`)

**(a) Card da pergunta.** Mostra pergunta, resposta e situação; se respondida, não validada e `pode_agir`, mostra o detalhe da competência entregue e esconde as duas variantes de **Validar**. Se a pergunta já estiver validada ou `pode_agir` for falso, esse bloco de detalhe e ações não aparece. (`app/templates/chamados/partials/_pergunta_card.html:20`, `:29`, `:42`, `:44`, `:47`, `:75`, `:100`)

- **Rejeitar**, com motivo obrigatório, permanece fora do ramo que esconde Validar. O POST do card chega à view de detalhe, passa por `acao_recusar_pergunta` e alcança `recusar_pergunta`. A porta exige motivo, resposta ainda não validada e pergunta não excluída do fato; não consulta K6 e, satisfeitas as condições, grava o motivo e reabre a resposta para o colaborador. (`app/templates/chamados/partials/_pergunta_card.html:117`; `app/chamados/views.py:505`; `app/chamados/services/acoes_disputa.py:81`; `app/chamados/services/validacao.py:886`, `:898`, `:906`, `:916`)
- Um POST direto de **Validar**, embora sem botão nesse estado, alcança `validar_pergunta`; a guarda K6 devolve a recusa antes do carimbo. (`app/chamados/views.py:483`; `app/chamados/services/validacao.py:740`)

**(b) Modal do fio.** Quando o bloco da disputa é acessível ao admin, mostra pergunta e resposta. O detalhe de competência entregue aparece nos ramos de resposta validada ou não validada, mas o ramo anterior de `pendencia_admin` tem precedência e mostra sua própria pendência e motivo. Para resposta não validada, o estado entregue esconde **Validar**. (`app/templates/chamados/partials/modal_fio.html:123`, `:157`, `:217`, `:222`, `:228`, `:251`, `:274`)

- **Rejeitar resposta** aparece para resposta não validada, sem o ramo anterior de pendência, se a pergunta não estiver em disputa fechada. `modal_fio_colaborador` encaminha ao serviço do fio, que verifica permissão, disputa e pertencimento da pergunta; chega à mesma `recusar_pergunta`, que pode gravar e reabrir sem guarda de entrega. (`app/templates/chamados/partials/modal_fio.html:316`; `app/chamados/views.py:869`; `app/chamados/services/fio.py:605`, `:672`; `app/chamados/services/validacao.py:898`, `:916`)
- **Corrigir**, com hora e motivo, aparece no mesmo ramo se `tipo_resposta == 'hora'`. A mesma view e o serviço do fio chegam a `acao_corrigir_resposta`: ela registra a ocorrência e chama `responder_pergunta_disputa_core` **antes** de chamar a validação. Assim, pode gravar a resposta corrigida e depois informar que a validação recusou pela entrega. (`app/templates/chamados/partials/modal_fio.html:327`; `app/chamados/services/fio.py:672`; `app/chamados/services/acoes_disputa.py:338`, `:358`, `:374`, `:383`; `app/chamados/services/validacao.py:740`)
- **Conferir data**, com justificativa, aparece se as datas divergirem, ainda não houver conferência e `pode_conferir_data` for verdadeiro. A view `conferir_data_pergunta` chama `conferir_data_do_fato`; a porta exige divergência e justificativa, mas não consulta entrega, e grava `data_conferida_em/por`. (`app/templates/chamados/partials/modal_fio.html:162`, `:170`; `app/chamados/views.py:1293`; `app/chamados/services/validacao.py:983`, `:990`, `:996`)
- No nível da disputa aberta, **Reabrir questionário** alcança `reabrir_disputa_supervisao`, que seleciona perguntas corrigíveis e chama `reabrir_para_correcao`; pode gravar a reabertura sem consultar esse estado. Se esta pergunta pertence ao conjunto corrigível, é **[não verificado]**. Se não houver corrigíveis, a view pode cobrar pendências vivas ou informar que não há o que reabrir. (`app/templates/chamados/partials/modal_fio.html:353`, `:394`; `app/chamados/views.py:1373`, `:1414`, `:1421`, `:1437`)
- Ainda na disputa aberta, se houver pergunta sem resposta, aparece **Cobrar resposta**; se todas estiverem respondidas e não for o ramo de âncora, aparece **Regularizar disputa e fechar**. A primeira chama a view de cobrança, sujeita às suas condições; a segunda chama a materialização das perguntas validadas e pode terminar com pendências, sem garantir o fechamento. O estado desta pergunta, sozinho, não determina qual botão aparece. (`app/templates/chamados/partials/modal_fio.html:353`, `:354`, `:361`, `:376`, `:379`; `app/chamados/views_cobrar.py:24`; `app/chamados/services/fio.py:693`, `:708`, `:718`, `:741`)

**(c) Lista de chamados/perguntas.** A lista resumida e a detalhada mostram o verbo e o estado do **chamado**, sem exibir o detalhe `bloqueada_competencia_entregue`. O estado informado não determina, sozinho, se a linha aparecerá como “validar”, “cobrar” ou outro verbo. (`app/templates/chamados/partials/_lista_resumo.html:64`; `app/templates/chamados/partials/_lista_chamados.html:74`; `app/chamados/services/verbos.py:30`; `app/chamados/services/painel.py:380`)

- Na resumida, expandir o grupo é interação local; **Abrir fio** e a linha do chamado abrem o modal. Na detalhada, clicar a linha também abre o fio. O telefone, quando existente, é link `tel:`. A view do modal é `modal_fio_colaborador`; os controles da pergunta e suas portas são os descritos acima. (`app/templates/chamados/partials/_lista_resumo.html:30`, `:38`, `:53`, `:58`; `app/templates/chamados/partials/_lista_chamados.html:42`, `:56`; `app/chamados/urls.py:39`)
- Na detalhada, se `item.verbo == "validar"`, aparece **Validar inline**. A view exige permissão, área e disputa aberta e percorre perguntas vivas respondidas pela porta de validação: a pergunta entregue é recusada, mas outras perguntas do mesmo lote podem ser validadas. (`app/templates/chamados/partials/_lista_chamados.html:79`; `app/chamados/views_cobrar.py:145`, `:160`, `:168`, `:202`; `app/chamados/services/validacao.py:740`)
- Se o verbo for `cobrar`, aparece **Cobrar** quando ainda não cobrado no dia e **Resgatar** se o chamado estiver silenciado. As views de cobrança e desarquivamento não usam este estado derivado como guarda; podem, conforme suas demais condições, enviar cobrança ou alterar o arquivamento do chamado. (`app/templates/chamados/partials/_lista_chamados.html:83`; `app/chamados/views_cobrar.py:24`; `app/chamados/views_arquivar.py:52`)
- O painel também oferece prévia de **validação em lote**; a porta reclassifica o lote e usa a validação. A pergunta entregue encontra a guarda K6, embora outras possam ser gravadas. A inclusão desta pergunta no lote é **[não verificado]**. (`app/templates/chamados/painel_gestao.html:14`; `app/chamados/views_cobrar.py:336`, `:373`; `app/chamados/services/validacao.py:740`)

**(d) Pauta do DP.** A gaveta de pautas não mostra esse estado derivado: sua serialização e apresentação trabalham com texto, âncora, leitura, conclusão e respostas. A existência de uma pauta para esta pergunta é **[não verificado]**; há também um mecanismo de pauta para competência *trancada*, condição que não se pode inferir apenas de “entregue”. (`app/pautas/views.py:37`; `app/templates/core/_copiloto.html:394`; `app/chamados/services/competencia_trancada.py:95`)

- Em pauta existente, clicar o remetente marca leitura quando o usuário é destinatário e tenta abrir a âncora; âncoras de dia ou competência recebem aviso. **Marcar feita** aparece para pauta recebida, permitida e ainda não concluída e grava conclusão/leitura. **Responder** aparece fora da aba sistema e cria resposta. **Cancelar** aparece para pauta enviada pelo próprio usuário quando permitido e grava cancelamento se ainda não concluída. São escritas na pauta, sem decisão sobre a pergunta bloqueada. (`app/templates/core/_copiloto.html:398`, `:431`, `:440`, `:451`, `:562`; `app/pautas/views.py:91`, `:99`, `:107`, `:118`; `app/pautas/services.py:192`, `:204`, `:233`, `:281`)
- O compositor, fora da aba sistema, permite escolher objeto e escrever uma pauta; a view e o serviço criam pauta com âncora válida, sem consultar esse estado. O modelo aceita âncoras como colaborador, chamado, dia e competência, não uma âncora de pergunta. (`app/templates/core/_copiloto.html:331`, `:535`; `app/pautas/views.py:70`; `app/pautas/services.py:152`; `app/pautas/models.py:30`)

ACOES DISPONIVEIS AO ADMIN: rejeitar resposta; corrigir resposta; conferir data; reabrir questionário; cobrar; regularizar disputa; abrir fio; validar inline ou em lote outras perguntas elegíveis; resgatar chamado; criar, ler, responder, concluir ou cancelar pauta — cada ação sujeita às condições indicadas acima.

## 18/11 18:06 · sessao 778413d53eac · - · - · origem=Ronald (tmux)

A pauta do DP não mostra automaticamente o estado `bloqueada_competencia_entregue`: a resposta da view contém texto, âncora, leitura, conclusão e respostas da pauta, mas não o estado da pergunta ([views.py](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/pautas/views.py:37)). A existência de uma pauta para o caso é **[não verificado]**.

Na aba Pautas, o administrador pode escrever uma pauta para o DP com texto e uma âncora válida; não há âncora específica de pergunta, mas há âncora de chamado e de dia ([models.py](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/pautas/models.py:30)). O formulário aparece fora da aba Sistema e envia para `escrever`, que cria a pauta sem consultar o estado da pergunta ([template](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/templates/core/_copiloto.html:349), [view](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/pautas/views.py:70), [service](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/pautas/services.py:152)).

Em pauta existente, os controles dependem da pauta e do usuário, não desse estado:

- **Abrir objeto:** clique no remetente; marca a pauta lida se o usuário for destinatário e abre âncoras de colaborador, chamado ou posto. Para dia ou competência, exibe aviso de que não há tela própria ([template](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/templates/core/_copiloto.html:398), [rota](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/templates/core/_copiloto.html:562), [view](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/pautas/views.py:91), [service](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/pautas/services.py:204)).
- **Marcar feita:** aparece para pauta recebida, quando o usuário pode fechá-la e ela ainda não está feita; a porta grava conclusão e leitura somente para destinatário ([template](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/templates/core/_copiloto.html:440), [view](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/pautas/views.py:99), [service](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/pautas/services.py:233)).
- **Responder:** aparece fora da aba Sistema; a porta cria uma pauta filha com a mesma âncora ([template](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/templates/core/_copiloto.html:451), [view](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/pautas/views.py:107), [service](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/pautas/services.py:192)).
- **Cancelar:** aparece em pauta enviada pelo próprio usuário ainda não feita; a porta exige autoria e grava o cancelamento ([template](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/templates/core/_copiloto.html:431), [view](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/pautas/views.py:118), [service](/home/ronald/codex_ws/c8b6d1ec25acb4ea1b921c37558f95021cc99cc2/app/pautas/services.py:281)).

## 18/11 18:06 · sessao b58431db4ec7 · - · - · origem=Ronald (tmux)

Achados enviados ao agente principal, com as condições da lista, as portas de validação, cobrança e resgate, e a distinção entre o estado da pergunta e o verbo do chamado.

_7097 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
