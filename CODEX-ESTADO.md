# CODEX-ESTADO

> Ultima resposta processada: **19/11/2026 12:15** · publicado em **19/11/2026 12:15** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 19/11 12:15 · sessao 73c2244e8ef0 · FERIADO-POR-POSTO · censo · origem=Code

**Resultado:** há duas fontes com efeitos diferentes no código: a marca do posto alimenta o padrão de vínculo novo e o veto de cobrança; a chave gravada no vínculo alimenta `feriado_vira_folga` e, por ele, a geração e a precedência do dia ([escala/models.py:905](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/models.py:905), [escala/models.py:1373](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/models.py:1373), [detectar_ausencias.py:617](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/management/commands/detectar_ausencias.py:617)).

A cópia não contém `.git`; portanto, o hash `ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba` como HEAD é **[não verificado]**. Os anexos foram fornecidos no texto do pedido, não como arquivos com linhas verificáveis. Nenhuma sonda de produção foi executada; seus resultados são **[não verificado]**.

## 1. Censo AST

Escopo da contagem: código Python de `app`, sem testes e migrações; **20 sítios leitores**, contando uma vez cada linha que lê diretamente os campos ou chama os métodos pedidos. Escritores aparecem separadamente. O script imprime os aliases de import e os candidatos encontrados por sintaxe, inclusive `getattr`, argumentos ORM e gravações; a classificação da tabela exige leitura humana. A busca sintática não prova ausência de acesso dinâmico, SQL cru, templates ou leitores indiretos; completude fora desse escopo é **[não verificado]**. [nao verificado]

Execute a partir da raiz da cópia; o script somente lê arquivos:

```python
import ast
from pathlib import Path

ALVOS = {
    "opera_em_feriado",
    "trabalha_em_feriado",
    "padrao_trabalha_em_feriado",
    "feriado_vira_folga",
}
RAIZ = Path("app")

for caminho in sorted(RAIZ.rglob("*.py")):
    if {"tests", "migrations"} & set(caminho.parts):
        continue
    try:
        arvore = ast.parse(caminho.read_text(encoding="utf-8"), filename=str(caminho))
    except (SyntaxError, UnicodeError) as exc:
        print(f"ERRO_PARSE {caminho} {type(exc).__name__}")
        continue

    aliases = {}
    for no in ast.walk(arvore):
        if isinstance(no, ast.Import):
            for item in no.names:
                aliases[item.asname or item.name.split(".")[0]] = item.name
        elif isinstance(no, ast.ImportFrom):
            modulo = "." * no.level + (no.module or "")
            for item in no.names:
                aliases[item.asname or item.name] = f"{modulo}.{item.name}"

    pais = {}
    for pai in ast.walk(arvore):
        for filho in ast.iter_child_nodes(pai):
            pais[filho] = pai

    achados = []
    for no in ast.walk(arvore):
        nome = None
        papel = None
        if isinstance(no, ast.Attribute) and no.attr in ALVOS:
            nome = no.attr
            papel = type(no.ctx).__name__
        elif isinstance(no, ast.Call) and isinstance(no.func, ast.Name):
            if (no.func.id in {"getattr", "setattr", "hasattr"}
                    and len(no.args) >= 2
                    and isinstance(no.args[1], ast.Constant)
                    and no.args[1].value in ALVOS):
                nome = no.args[1].value
                papel = no.func.id
        elif isinstance(no, ast.keyword) and no.arg:
            if any(no.arg == x or no.arg.endswith("__" + x) for x in ALVOS):
                nome = no.arg
                papel = "keyword: classificar ORM/leitura ou construtor/escrita"
        elif isinstance(no, (ast.FunctionDef, ast.AsyncFunctionDef)):
            if no.name in ALVOS:
                nome = no.name
                papel = "definicao"
        elif isinstance(no, ast.Name) and no.id in ALVOS:
            nome = no.id
            papel = type(no.ctx).__name__
        if nome is None:
            continue
        atual = no
        contexto = "<modulo>"
        while atual in pais:
            atual = pais[atual]
            if isinstance(atual, (ast.FunctionDef, ast.AsyncFunctionDef, ast.ClassDef)):
                contexto = atual.name
                break
        achados.append((no.lineno, nome, papel, contexto))

    if achados:
        print(f"\n{caminho}")
        for alias, origem in sorted(aliases.items()):
            print(f"  IMPORT {alias} -> {origem}")
        for linha, nome, papel, contexto in sorted(set(achados)):
            print(f"  {linha}: {contexto} {nome} {papel}")
```

| Sítio | Papel e fonte |
|---|---|
| [escala/models.py:911](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/models.py:911) | **Decide padrão novo:** lê marca do posto; se nula, lê tipo e base da escala. |
| [colaboradores/services/vinculo.py:296](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/vinculo.py:296) | **Grava vínculo novo:** chama o padrão acima e grava sua resposta na chave do vínculo. |
| [escala/models.py:929](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/models.py:929), [932](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/models.py:932) | **Conta/exibe:** filtra chave True do vínculo e separa os que têm posto declarado True. |
| [escala/models.py:1390](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/models.py:1390) | **Decide:** `feriado_vira_folga` lê chave do vínculo, ciclo e feriado no lugar do vínculo; não lê a marca de operação. |
| [escala/models.py:1369](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/models.py:1369) | **Decide geração/fallback:** chama o predicado do vínculo para transformar dia-base de trabalho em folga. |
| [ponto/precedencia.py:377](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/precedencia.py:377) | **Decide fatos:** chama o predicado do vínculo para determinar `feriado_so_no_pagamento`. |
| [ponto/services/fechamento.py:949](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/fechamento.py:949), [ponto/calculador/alimentacao.py:521](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/calculador/alimentacao.py:521) | **Classificam pagamento:** chamam o predicado do vínculo; há diferença de dia-chave registrada em [BACKLOG.md:1654](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:1654). |
| [colaboradores/services/feriado_do_posto.py:69](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/feriado_do_posto.py:69) | **Decide veto:** `cobra_no_dia` lê marca do posto e feriado de seu município. |
| [colaboradores/services/feriado_do_posto.py:127](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/feriado_do_posto.py:127) | **Exibe/classifica:** lê marca do posto para excluir marcados das sugestões. |
| [colaboradores/management/commands/marcar_feriado_do_posto.py:85](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/management/commands/marcar_feriado_do_posto.py:85), [129](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/management/commands/marcar_feriado_do_posto.py:129) | **Exibe e lê antes para trilha:** marca do posto. |
| [colaboradores/services/estrutura.py:84](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/estrutura.py:84) | **Grava:** marca do posto. |
| [colaboradores/services/vinculo.py:890](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/vinculo.py:890), [892–894](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/vinculo.py:892) | **Decide e grava:** compara e altera chave do vínculo existente. São dois sítios leitores e um escritor. |
| [colaboradores/services/vinculo.py:974](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/vinculo.py:974), [1092](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/vinculo.py:1092), [1113](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/vinculo.py:1113) | **Copia/grava:** lê a chave de vínculo anterior e a passa ao novo objeto. |
| [sincronizar_feriado_admin.py:48](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/management/commands/sincronizar_feriado_admin.py:48), [52](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/management/commands/sincronizar_feriado_admin.py:52) | **Compara/exibe:** lê chave do vínculo; delega eventual gravação à porta na linha 56. |

Leitores indiretos relevantes: `detectar_ausencias` lê o resultado de `fatos_do_dia` e de `cobra_no_dia` ([detectar_ausencias.py:616](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/management/commands/detectar_ausencias.py:616)); `sugestoes` usa presença registrada na ata e somente postos ainda sem marca ([feriado_do_posto.py:74](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/feriado_do_posto.py:74), [136](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/feriado_do_posto.py:136)).

## 2. Matriz do feriado em dia de escala

Hipóteses para a tabela: vínculo weekday, feriado que alcança seu posto, **célula gerada normalmente**, dia-base previsto, sem ausência ou outro fato de precedência superior. Célula editada ou antiga pode divergir do gerador; nesse caso, `fatos_do_dia` lê primeiro o `trabalha` da célula ([precedencia.py:347](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/precedencia.py:347)). A batida não altera a marca nem a chave.

`F/T/F` abaixo significa `(faixa=feriado, previsto=True, só pagamento=False)`; na tabela gerada com chave False, o previsto é False, portanto aparece `F/F/F`. `T/T/T` significa `(faixa=trabalho, previsto=True, só pagamento=True)`.

| Posto | Chave | Batida | Célula; fatos | Emissor de falta, se alcançado | Dobra no motor |
|---|---:|---|---|---|---|
| Opera | False | Sem | False; F/F/F | `VETO_FERIADO` | Sem duração apurada: não há dobra por essa batida. |
| **Opera** | **False** | **Com** | **False; F/F/F — contradição declarada** | `VETO_FERIADO` se o funil for alcançado | Depende de duração apurada e `feriado_em_dobra` do ciclo. |
| Fecha | False | Sem | False; F/F/F | `VETO_FERIADO` chega antes do veto do posto. | Sem duração apurada: não há dobra por essa batida. |
| Fecha | False | Com | False; F/F/F | `VETO_FERIADO` se alcançado. | Depende de duração e configuração do ciclo. |
| Sem marca | False | Sem | False; F/F/F | `VETO_FERIADO`; a ausência de marca não veta por posto. | Sem duração apurada: não há dobra por essa batida. |
| Sem marca | False | Com | False; F/F/F | `VETO_FERIADO` se alcançado. | Depende de duração e configuração do ciclo. |
| Opera | True | Sem | True; T/T/T | Passa os dois vetos de feriado; emissão ainda depende das demais guardas. | Sem duração apurada: não há dobra por essa batida. |
| Opera | True | Com | True; T/T/T | Passa os dois vetos de feriado; demais guardas seguem aplicáveis. | Depende de duração e configuração do ciclo. |
| **Fecha** | **True** | **Sem** | **True; T/T/T — contradição declarada** | **`VETO_FERIADO_DO_POSTO`**, se nenhuma guarda anterior interromper. | Sem duração apurada: não há dobra por essa batida. |
| **Fecha** | **True** | **Com** | **True; T/T/T — contradição declarada** | `VETO_FERIADO_DO_POSTO` se o funil for alcançado. | Depende de duração e configuração do ciclo. |
| Sem marca | True | Sem | True; T/T/T | Passa os dois vetos de feriado; demais guardas seguem aplicáveis. | Sem duração apurada: não há dobra por essa batida. |
| Sem marca | True | Com | True; T/T/T | Passa os dois vetos de feriado; demais guardas seguem aplicáveis. | Depende de duração e configuração do ciclo. |

A contradição **opera/False** também está presente na primeira linha, com ou sem batida: o vínculo faz o dia virar feriado/folga apesar da marca de operação. A tabela identifica **dois tipos** de contradição, correspondentes a quatro combinações com batida; o número de vínculos contraditórios na frota é **[não verificado]**. Posto sem marca não é tratado aqui como declaração contraditória.

A cadeia da matriz está em [escala/models.py:1367](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/models.py:1367), [precedencia.py:376](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/precedencia.py:376), [detectar_ausencias.py:617](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/management/commands/detectar_ausencias.py:617) e [feriado_do_posto.py:60](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/feriado_do_posto.py:60). O motor pergunta `feriado_em_dobra` e `eh_feriado`, e aplica a dobra a duração positiva; ele não pergunta essas duas marcas nesse predicado ([motor_calculo_v2.py:914](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/motor_calculo_v2.py:914), [2809](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/motor_calculo_v2.py:2809)). O efeito monetário completo de cada linha é **[não verificado]**.

## 3. Sonda somente leitura

Universo: cada `EscalaColaborador` cujo ciclo esteja em `TipoEscala.CICLOS_FERIADO_VIRA_FOLGA` e que satisfaça **`q_vinculo_cobre_a_data(hoje)`**, a função SQL da casa para vigência ([escala/models.py:32](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/models.py:32), [escala/alimentacao.py:219](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/alimentacao.py:219)). É contagem de **vínculos**, inclusive sobrepostos, sem filtro adicional de situação do colaborador; não significa que todos sejam o vínculo escolhido para um dia.

A sonda usa `sugestoes()` real, que só sugere para postos não marcados ([feriado_do_posto.py:136](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/feriado_do_posto.py:136)). Saída identificadora: apenas PK de posto e vínculo. Executar no envelope Django da sombra; **não executada aqui**.

```python
from collections import Counter
from django.utils import timezone

from colaboradores.models import Posto
from colaboradores.services.feriado_do_posto import sugestoes
from escala.alimentacao import q_vinculo_cobre_a_data
from escala.models import EscalaColaborador, TipoEscala

ERROS = 0

try:
    hoje = timezone.localdate()
    postos = list(Posto.objects.values_list("pk", "opera_em_feriado"))
    marca_por_posto = dict(postos)
    postos_por_marca = Counter(
        "OPERA" if marca is True else "FECHA" if marca is False else "SEM_MARCA"
        for _, marca in postos
    )

    qs = EscalaColaborador.objects.filter(
        q_vinculo_cobre_a_data(hoje),
        tipo_escala__tipo_ciclo__in=TipoEscala.CICLOS_FERIADO_VIRA_FOLGA,
    )
    universo = qs.count()
    linhas = list(qs.values_list("pk", "posto_id", "trabalha_em_feriado"))
    contagem = Counter()
    for vinculo_pk, posto_pk, chave in linhas:
        if posto_pk is None:
            classe = "SEM_POSTO"
        else:
            marca = marca_por_posto[posto_pk]
            classe = "OPERA" if marca is True else "FECHA" if marca is False else "SEM_MARCA"
        contagem[(classe, chave)] += 1

    print("UNIVERSO=%d" % universo)
    for classe in ("OPERA", "FECHA", "SEM_MARCA"):
        print("POSTOS_%s=%d" % (classe, postos_por_marca[classe]))
    for classe in ("OPERA", "FECHA", "SEM_MARCA", "SEM_POSTO"):
        for chave in (True, False):
            print("VINCULOS_%s_%s=%d" % (classe, chave, contagem[(classe, chave)]))

    contador = sum(contagem.values())
    print("CONTADOR=%d UNIVERSO=%d IGUAL=%s" %
          (contador, universo, contador == universo))
    if contador != universo:
        ERROS += 1

    s = sugestoes()
    sug_opera = {x["posto_id"] for x in s["sugere_OPERA"]}
    sug_fecha = {x["posto_id"] for x in s["sugere_FECHA"]}
    bo = sorted((p, v) for v, p, chave in linhas if p in sug_opera and chave is False)
    inverso = sorted((p, v) for v, p, chave in linhas if p in sug_fecha and chave is True)

    for rotulo, pares in (("BO_FALSE_SUGERE_OPERA", bo),
                          ("INVERSO_TRUE_SUGERE_FECHA", inverso)):
        print("%s_VINCULOS=%d POSTOS=%d" %
              (rotulo, len(pares), len({p for p, _ in pares})))
        for posto_pk, vinculo_pk in pares:
            print("%s POSTO=%d VINCULO=%d" % (rotulo, posto_pk, vinculo_pk))

    if 596 in marca_por_posto:
        print("POSTO=596 MARCA=%s SUGERE_OPERA=%s SUGERE_FECHA=%s" %
              (marca_por_posto[596], 596 in sug_opera, 596 in sug_fecha))
    else:
        print("POSTO=596 AUSENTE")

    focal = EscalaColaborador.objects.filter(pk=1255).values_list(
        "pk", "posto_id", "trabalha_em_feriado"
    ).first()
    if focal is None:
        print("VINCULO=1255 AUSENTE")
    else:
        v, p, chave = focal
        print("VINCULO=%d POSTO=%s CHAVE=%s NO_UNIVERSO=%s" %
              (v, p, chave, any(linha[0] == v for linha in linhas)))

except Exception:
    ERROS += 1
    print("ERRO_SONDA")

print("ERROS=%d" % ERROS)
print("FIM_SONDA")
```

`ERROS=0`, igualdade e números de classe só podem ser afirmados após execução. A quantidade de postos fechados e vínculos mencionada no anexo de contexto tem outro universo — vínculos de qualquer vigência — e não substitui esta medição **[não verificado]**.

## 4. Pergunta aberta de lei

**Não achei uma regra geral que diga qual fonte deve governar todos os efeitos quando posto e vínculo existente divergem.** A busca nas referências disponíveis tem estes limites e achados:

- Para **vínculo novo**, a regra é explícita: “O posto que DECLAROU … vence”; o mesmo texto diz que vínculo existente não passa pelo padrão ([escala/models.py:905](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/models.py:905); também [TICKETS.md:1168](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/TICKETS.md:1168)).
- Para **cobrança**, o código registra que posto marcado fechado veta mesmo vínculo que trabalha em feriado ([detectar_ausencias.py:619](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/management/commands/detectar_ausencias.py:619)). Isso resolve esse veto específico.
- O backlog registra expressamente as duas fontes e encaminha a cópia por vínculo como segunda fonte para dossiê ([BACKLOG.md:1617](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:1617)).
- Não achei, nas buscas pelos termos de posto e feriado em `CORTES.md`, `LEIS.md`, `LAPIDES.md` e diretório ADR, uma regra adicional para a divergência geral. O conteúdo das **Pautas DP 140–149** não foi localizado na cópia; sua decisão é **[não verificado]**.

**Pergunta de lei L=1:** `lei: FERIADO-POR-POSTO fonte unica` — a marca declarada do posto deve mandar também nos efeitos hoje decididos pela chave do vínculo existente, ou a chave do vínculo permanece autoridade própria nesses efeitos? Outra decisão é **[não verificado]**. Números da classe do BO, inverso e contradições: **[não verificado]**, pendentes da sonda.

## 5. Tela e porta existente

O cadastro está em `form_posto`, protegido por login e pela ação `editar_postos`; a lista exige `ver_postos` ([views.py:699](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/views.py:699), [733](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/views.py:733)). As rotas de criar e editar estão em [urls.py:38](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/urls.py:38); o template é [posto_form.html:10](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/templates/colaboradores/posto_form.html:10). O dicionário salvo pela view não inclui `opera_em_feriado` ([views.py:85

[truncado em 20000 de 21131 caracteres]

## 19/11 12:12 · sessao 97052842952f · CADEIA-DO-CONTRATO-20 · censo · origem=Code

A regra pedida para o passo 2 é preservar o **instante completo de cada sobra**, mantendo sua associação ao dia do turno informada no brief do passo 1. A ata atual recebe uma lista que já foi reduzida a `HH:MM`; portanto, trocar somente a serialização em `ata_do_dia` não recuperaria data nem segundos. ([utils.py:631](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/utils.py:631), [utils.py:673](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/utils.py:673), [cartorio.py:764](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/cartorio.py:764); brief do passo 1 fornecido: linha [não verificado])

**Limite da cópia:** os arquivos estão disponíveis, mas `git rev-parse HEAD` falhou porque o diretório não contém um repositório Git acessível. Assim, a identidade da cópia com o HEAD informado é **[não verificado]**. O censo abaixo foi executado nos arquivos presentes, com `python3 -B`, sem criar arquivos. [não verificado: identidade do HEAD]

## Casos pela regra, antes de qualquer código

A L-110 pede o caso e a resposta da regra antes do código; a comparação com o comportamento anterior serve para medir impacto, não para certificar a resposta. ([CLAUDE.md:46](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/CLAUDE.md:46))

Os exemplos são casos especificados, **não resultados de testes executados**. A representação ISO com offset abaixo concretiza o instante proposto no pré-desenho; o formato efetivamente adotado em implementação futura é [não verificado]. [não verificado: linha local do anexo `predesenho.r`] [nao verificado]

| Caso e entrada | Resposta esperada pela regra |
|---|---|
| Uma sobra em 19/11/2026 às `14:17:23-03:00`, pertencente ao turno de 19/11 | A ata conserva `2026-11-19T14:17:23-03:00`; a apresentação pode mostrar `14:17`, sem usar esse texto como identidade. ([cartorio.py:764](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/cartorio.py:764)) |
| Turno de 19/11 cruza meia-noite; sua sobra ocorre em `2026-11-20T04:00:12-03:00` | O instante conserva a data civil de 20/11 e a sobra pertence ao **dia do turno 19/11**, conforme o brief fornecido. Não se deve substituir a data do instante pela data do turno. [não verificado: linha local do brief do passo 1] |
| Duas sobras do mesmo turno em `14:17:05-03:00` e `14:17:48-03:00` | São duas ocorrências distintas; ambas sobrevivem à lavra e à leitura, embora as duas sejam exibidas como `14:17`. A reconciliação de uma não remove a outra. O código atual compara e remove por `HH:MM`. ([utils.py:2025](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/utils.py:2025), [utils.py:2052](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/utils.py:2052)) |
| O mesmo instante expresso com offsets diferentes | A comparação temporal reconhece o mesmo instante; a apresentação não decide identidade por texto. Esse é um caso de representação proposto no pré-desenho. [não verificado: linha local do anexo `predesenho.r`] |
| Ata antiga com `orfas=["14:17"]` ou apenas `n_orfas=1` | A contagem pode demonstrar presença; o instante não é conhecido e não deve ser fabricado a partir da data da célula. O leitor atual fabrica `?` quando só dispõe da contagem. ([leitor_celula.py:187](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/services/leitor_celula.py:187)) |
| Lista lavrada explicitamente vazia, sem contagem contraditória | Zero sobras conhecido; é diferente de uma lista ausente ou incompleta. A gravação atual registra lista e contagem separadamente. ([cartorio.py:761](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/cartorio.py:761)) |
| Sobra existe, mas não há marcos para desenhar | A existência e o instante da sobra continuam disponíveis ao leitor e à apresentação. Hoje o adaptador pode devolver duas listas vazias quando não há marcos. ([utils.py:1095](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/utils.py:1095), [utils.py:1121](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/utils.py:1121)) |

**Ressalva no noturno:** o brief fornecido diz que a sobra pertence ao dia do turno, enquanto a reconciliação atual procura a **ponta de saída** na lista de `data_turno + 1` quando o juiz informa cruzamento de meia-noite. O caso acima registra a resposta solicitada; não prova que o código atual já a cumpra, nem autoriza remover as guardas existentes da reconciliação. ([utils.py:1965](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/utils.py:1965), [utils.py:1981](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/utils.py:1981); brief fornecido, linha [não verificado])

## Censo dos escritores e leitores

**Papel** indica o efeito comprovado no sítio: **MOSTRA** apresenta ou transporta fatos; **DECIDE** usa o dado em classificação, seleção ou chamada; **PAGA** altera cálculo monetário. Uma chamada à grade, por si só, não prova que o chamador consuma órfãs nem define seu papel; o script a inclui como candidato para inspeção. ([leitor_celula.py:420](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/services/leitor_celula.py:420))

| Sítio | Papel e efeito constatado | Dependência do passo 1 |
|---|---|---|
| Grade: formação e promoções | **DECIDE/transporta:** forma sobras em `HH:MM`, acrescenta e ordena; também promove células para a lista. O instante precisa sobreviver desde a fonte. ([utils.py:631](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/utils.py:631), [utils.py:810](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/utils.py:810), [utils.py:1733](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/utils.py:1733)) | Preservar instante **não depende** da razão; atribuir razão a cada ocorrência **depende** da lavra do passo 1. [não verificado: linha local do brief] |
| Grade: ata sintética e reconciliação | **DECIDE/transporta:** constrói ata em memória e procura/remove bordas por hora. A migração deve preservar as guardas existentes e operar sobre a ocorrência correta. ([utils.py:1898](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/utils.py:1898), [utils.py:1979](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/utils.py:1979), [utils.py:2039](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/utils.py:2039)) | Comparação por instante **não depende** da razão; transportar uma razão já associada **depende** do contrato entregue pelo passo 1. [não verificado: formato futuro] |
| Cartório: `lampadas_do_dia` e `ata_do_dia` | **DECIDE/escreve:** o primeiro usa horas de órfãs na geometria; o segundo grava `n_orfas` e `orfas`. ([cartorio.py:647](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/cartorio.py:647), [cartorio.py:755](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/cartorio.py:755), [cartorio.py:764](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/cartorio.py:764)) | Gravar e comparar instantes **não depende** da razão; gravar a razão na mesma lavra **depende** do passo 1. [não verificado: linha local do brief] |
| Porta da célula | **Escreve/transporta:** substitui a ata na lavra; outra operação copia a ata para carimbar precedência. A projeção `out.ata` é cópia em memória. ([celula.py:1003](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/portas/celula.py:1003), [celula.py:1064](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/portas/celula.py:1064), [celula.py:1214](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/portas/celula.py:1214)) | Transportar o novo valor **não depende** da razão; preservar o campo de razão quando presente **depende** do formato que o passo 1 lavrar. [não verificado: formato futuro] |
| `leitor_celula` | **MOSTRA/transporta:** lê a lista crua ou cria marcadores pela contagem e publica `dia.orfas`. É a fronteira natural para distinguir instante conhecido de dado legado incompleto. ([leitor_celula.py:180](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/services/leitor_celula.py:180), [leitor_celula.py:420](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/services/leitor_celula.py:420)) | Ler instantes e contagem **não depende** da razão; publicar a razão **depende** de ela estar lavrada. [não verificado: contrato futuro] |
| Espelho e template | **MOSTRA:** a grade passa por adaptador e chega a `orfas_grade`, impresso no template. ([espelho.py:703](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/espelho.py:703), [espelho.py:858](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/espelho.py:858), [espelho.html:242](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/templates/ponto/espelho.html:242)) | Mostrar hora **não depende** da razão; mostrar o motivo **depende** do passo 1. [não verificado: apresentação futura] |
| Calendário | **MOSTRA, com seleção:** usa `orfas` da grade para marcar batidas; a associação atual usa a lista e a hora. ([calendario.py:409](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/calendario.py:409), [calendario.py:655](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/calendario.py:655)) | Marcação por instante **não depende** da razão; eventual rótulo de motivo **depende** dela. [não verificado: rótulo futuro] |
| PDF | **MOSTRA/DECIDE apresentação:** coleta órfãs e usa `HH:MM` para reconhecer entrada espúria e marcas. ([pdf_espelho.py:174](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/relatorios/pdf_espelho.py:174), [pdf_espelho.py:477](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/relatorios/pdf_espelho.py:477), [pdf_espelho.py:552](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/relatorios/pdf_espelho.py:552)) | Reconhecer a ocorrência e formatar **não depende** da razão; imprimir razão **depende** dela. [não verificado: impressão futura] |
| `periodos_do_dia` | **MOSTRA/transporta:** copia as strings da ata no resultado; a própria documentação afirma que a órfã aparece e não entra em período. ([juiz_batida.py:18](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/juiz_batida.py:18), [juiz_batida.py:161](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/juiz_batida.py:161)) | Transporte de instante **não depende** da razão. Efeito PAGA de uma eventual mudança nesse caminho: **[não verificado]**. |
| Chamados: juiz de furo e materialização | **DECIDE:** o juiz testa lista ou contagem; a materialização filtra horas e usa as marcas na pergunta de preenchimento. ([juizes.py:514](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/juizes.py:514), [materializacao.py:449](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/materializacao.py:449)) | Presença por contagem e identidade por instante **não dependem** da razão. Fazer a razão influenciar uma decisão existente seria outra mudança; autorização e efeito **[não verificado]**. |
| Cluster espúrio | **DECIDE:** lê a ata, calcula proximidade por minutos e pode produzir contexto de chamado. ([cluster_espurio.py:29](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/cluster_espurio.py:29), [cluster_espurio.py:34](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/cluster_espurio.py:34), [cluster_espurio.py:211](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/cluster_espurio.py:211)) | Diferença entre instantes **não depende** da razão; exibir ou usar razão dependeria do passo 1 e do contrato do chamado **[não verificado]**. |
| Esmeril e vigias | **DECIDE medida/alarme:** esmeril conta e agrupa por faixa; vigia do pareador compara horas; tripwire recebe a lista crua. ([esmeril_espelho.py:130](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/esmeril_espelho.py:130), [vigia_pareador_celula.py:74](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/vigia_pareador_celula.py:74), [tripwire_celulas.py:264](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/management/commands/tripwire_celulas.py:264)) | Contar, medir e reconhecer instante **não dependem** da razão; medir por motivo **depende** dela. [não verificado: medida futura] |
| Propositor, ausência parcial e suprajuiz | **DECIDE/MOSTRA:** o propositor analisa horas; turnos converte sobras em minutos do dia; suprajuiz testa presença e descreve. ([propositor.py:39](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/propositor.py:39), [turnos.py:2679](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/turnos.py:2679), [supra_juiz.py:527](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/supra_juiz.py:527)) | Adaptar a representação **não depende** da razão; classificar ou descrever por razão **depende** dela. Consequência PAGA da cadeia de ausência parcial: **[não verificado]**. |
| Agregados e classificações | **DECIDE/conta:** causas da folha, status, furos, bordas e simulação usam presença ou quantidade. Não se deve converter identidade desconhecida em zero. ([causas.py:43](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/folha/causas.py:43), [status_dia.py:188](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/status_dia.py:188), [furos_diarios.py:22](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/services/furos_diarios.py:22), [bordas_realizado.py:49](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/bordas_realizado.py:49), [simular_dia.py:12](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/simular_dia.py:12)) | Presença e contagem **não dependem** da razão. Classificação por motivo depende do passo 1 e de decisão autorizada **[não verificado]**. O nome `folha` não prova, sozinho, efeito **PAGA**. |
| Classificador atual | **DECIDE:** `classificar_orfas` lê `dia.orfas` e devolve classes chaveadas por hora. O brief fornecido determina que seu corpo vire o juiz da razão ou morra no marco. ([orfa_classe.py:53](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/orfa_classe.py:53), [orfa_classe.py:58](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/orfa_classe.py:58); brief fornecido, linha [não verificado]) | A **razão** depende do passo 1; a chave de identidade por instante é necessidade do passo 2. A forma integrada final é **[não verificado]**. |

O AST também encontrou leitura direta no cartão e no snapshot, além do comando do propositor; sua existência ou chamada por nome não comprova, por si, uso produtivo atual. ([cartao_pela_celula.py:288](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/relatorios/cartao_pela_celula.py:288), [snapshot_o118_col221.py:32](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/bin/snapshot_o118_col221.py:32), [propor_reorganizacao.py:104](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/management/commands/propor_reorganizacao.py:104))

O script abaixo é o executado na sessão, com `python3 -B`. Lista ocorrências sintáticas e chamadas às funções-alvo, inclusive `from ... import nome as apelido`; a classificação de papel na tabela exige leitura do contexto. Ele também lista homônimos, não resolve fluxo de dados, aliases atribuídos em tempo de execução, acesso dinâmico ou templates; completude para esses casos é **[não verificado]**. Os templates foram conferidos por busca textual. ([calendario.py:362](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/calendario.py:362), [espelho.html:242](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/templates/ponto/espelho.html:242))

```python
import ast
from pathlib import Path

roots = (Path('app'), Path('bin'))
targets = {
    '_orfas_da_ata', 'grade_da_celula', 'montar_realizado_grade',
    'ata_do_dia', 'periodos_do_dia', 'lavrar_veredito',
}
keys = {'orfas', 'n_orfas', 'orfas_grade'}

for root in roots:
    for path in sorted(root.rglob('*.py')):
        if 'tests' in path.parts or path.name.startswith('test_'):
            continue
        try:
            tree = ast.parse(path.read_text(encoding='utf-8'), filename=str(path))
        except (SyntaxError, UnicodeError):
            print(f'NAO_ANALISADO {path}')
            continue

        aliases = {}
        for node in ast.walk(tree):
            if isinstance(node, ast.ImportFrom):
                for item in node.names:
                    if item.name in targets:
                        aliases[item.asname or item.name] = item.name

        hits = set()
        for node in ast.walk(tree):
            if isinstance(node, ast.Dict):
                for key in node.keys:
                    if isinstance(key, ast.Constant) and key.value in keys:
                        hits.add((key.lineno, 'CHAVE ' + key.value))
            elif isinstance(node, ast.Subscript):
                k = node.slice
                if isinstance(k, ast.Constant) and k.value in keys:
                    kind = 'ESCREVE' if isinstance(node.ctx, (ast.Store, ast.Del)) else 'LE'
                    hits.add((node.lineno, kind + ' ' + k.value))
            elif isinstance(node, ast.Call):
                f = node.func
                if isinstance(f, ast.Attribute) and f.attr in {'get', 'pop', 'setdefault'} and node.args:
                    k = node.args[0]
                    if isinstance(k, ast.Constant) and k.value in keys:
                        hits.add((node.lineno, f.attr.upper() + ' ' + k.value))
                name = f.id if isinstance(f, ast.Name) else f.attr if isinstance(f, ast.Attribute) else None
                original = aliases.get(name, name)
                if original in targets:
                    suffix = f' COMO {name}' if original != name else ''
                    hits.add((node.lineno, 'CHAMA ' + original + suffix))
                for kw in node.keywords:
                    if kw.arg in keys:
                        hits.add((kw.value.lineno, 'ARG ' + kw.arg))
            elif isinstance(node, ast.Attribute) and node.attr == 'ata' and isinstance(node.ctx, ast.Store):
                hits.add((node.lineno, 'ESCREVE .ata'))

        for line, role in sorted(hits):
            print(f'{path}:{line} {role}')
```

**Perguntas de lei: 0**, para os casos de preservação de instante e transporte aqui descritos, mantendo as autoridades e guardas existentes. A execução dos casos, o impacto em dados reais e a comprovação de ausência de efeito monetário permanecem **[não verificado]**. ([CLAUDE.md:46](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/CLAUDE.md:46))

## 19/11 12:11 · sessao 859ef8c33903 · CADEIA-20-PASSO-1 · construtor · origem=Code

**PAREI: a marca `ESCALA_NAO_DESCREVE` não chega ao fluxo da lavra sem nova consulta identificada.** A pergunta está ao fim; não escrevi código nem testes. [nao verificado]

`ata_do_dia` tem três chamadas diretas de produção, todas em `_julgar_colab_corpo`:

- Ramo de quem nunca bateu: monta `_ata_nb` em [cartorio.py:1280](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:1280), depois a passa à lavra em [cartorio.py:1321](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:1321).
- Ramo normal, ata deste julgamento para leitura durante o julgamento: [cartorio.py:1464](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:1464).
- Ramo normal, ata passada a `lavrar_veredito`: [cartorio.py:1898](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:1898).

Nos três casos, a grade vem da chamada em [cartorio.py:1204](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:1204). Os insumos têm estas origens e limites:

- **Turnos:** a grade por turno já chama `turnos_do_colab` com a ata desta montagem em [escala/utils.py:1890](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1890) e [escala/utils.py:1909](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:1909), mas retorna somente `marcos` e `dias` em [escala/utils.py:2082](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:2082). O dispatcher também admite o caminho legado em [escala/utils.py:2095](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:2095). Portanto, reutilizar os turnos exige transportá-los até a lavra; disponibilidade equivalente no legado: **[não verificado]**.
- **Tipo do dia:** `dia_sem_trabalho` lê `cel.trabalha` e `cel.ata`, inclusive `cobertura_parcial`, em [chamados/juizes.py:445](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/chamados/juizes.py:445). Para este julgamento, a testemunha deve ser a ata montada agora com `cel.trabalha`, não `cel.ata` antiga: o próprio cartório distingue a ata atual da anterior em [cartorio.py:1460](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:1460).
- **Pausas:** o juiz recebe batidas de pausa avulsas já atribuídas ao dia em [orfa_classe.py:23](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/orfa_classe.py:23). O pareador carimba a batida e, se não houver turno corrente, não a acrescenta a um turno em [ponto/turnos.py:1365](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/turnos.py:1365). `turnos_do_colab` pareia cópias e devolve turnos em [ponto/turnos.py:1984](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/turnos.py:1984). Uma lista dessas pausas avulsas, com atribuição ao dia, disponível na lavra sem nova consulta: **[não verificado]**.
- **Marca:** a lista publicada vem de `espelho_do_colab(...).resultado.dias_cadastro_x_realidade` em [cxr_na_lista.py:77](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cxr_na_lista.py:77). Existe leitura da lavra por consulta a `MetricaSnapshot` em [cxr_na_lista.py:152](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cxr_na_lista.py:152); sua ausência de cobertura retorna `None`, não lista vazia, em [cxr_na_lista.py:165](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cxr_na_lista.py:165). Não identifiquei essa lista já carregada no fluxo de `cartorio.py` **[não verificado quanto a acesso indireto em tempo de execução]**. [nao verificado]

**Aval devolvido, revisão por item:**

1. As chamadas e a ata atual conferem nas referências acima; a disponibilidade dos turnos e pausas até a lavra não está demonstrada, e a marca não foi identificada carregada no fluxo.
2. Compatibilidade integral com outros avais vigentes: **[não verificado]**; foi fornecido apenas o aval raspado.
3. A condição literal de não acrescentar consulta não está demonstrada para a marca; a alimentação das pausas avulsas também precisa ser esclarecida ([cxr_na_lista.py:152](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cxr_na_lista.py:152), [orfa_classe.py:27](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/orfa_classe.py:27)).
4. Alteração de exportação, dinheiro ou `!`: **[não verificado]**; nenhuma alteração foi feita.

**Pergunta:** qual fonte já carregada deve alimentar a marca `ESCALA_NAO_DESCREVE` na lavra e como devem chegar as pausas avulsas atribuídas ao dia, sem consulta nova nem novo pareamento? Em particular, se a única fonte da marca for o snapshot, como proceder quando `cxr_lavrado` retorna `None` por falta de cobertura ([cxr_na_lista.py:170](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cxr_na_lista.py:170))?

Arquivos tocados: nenhum. Testes RED, mutantes, teto de consultas e comando Django executado: **[não verificado; não realizados]**. Perguntas de lei: **[não verificado]**; há uma pergunta de alimentação acima. [nao verificado]

## 19/11 12:11 · sessao 8aba5c841c41 · CADEIA-DO-CONTRATO-20 · censo · origem=Code

O pré-mapa passa de **34 para 38 arquivos necessários, provisoriamente**, com **3 não decididos** e **4 conflitos de conteúdo**. Os 38 incluem os 34 classificados como necessários no anexo de 18/11, cuja classificação integral não foi refeita arquivo a arquivo nesta resposta; os quatro acréscimos são `calendario.py`, `dia_decidido.py`, `ausencia.py` e `export.py`. A necessidade do registro final de `test_medida_turno_do_par.py`, de `situacao.py` e de `test_a2_precedencia.py` permanece não decidida. [não verificado]

A medição fornecida aponta **5 dos 41 arquivos alterados no main** e **nenhum arquivo em comum** com as listas fornecidas da F5 e do passo 1; isso mede interseção por arquivo, não elimina dependências indiretas. [não verificado]

| Arquivo alterado | Pouso e efeito observado | Mudança LASTRO | [nao verificado]
|---|---|---|
| `calendario.py` | **ESPURIA-TELA c11**, pela linhagem `585dbb8bf` informada: transporta quem decidiu, quando e o ID da ausência a partir de `_dd`. [HEAD: calendario.py:757](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/calendario.py:757) | **Necessária**: LASTRO alimenta o status e o compositor com recusas por data; isso não aparece no trecho novo do HEAD. [cópia: calendario.py:499](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/colaboradores/services/calendario.py:499), [cópia: calendario.py:710](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/colaboradores/services/calendario.py:710) |
| `test_medida_turno_do_par.py` | **ESCALA c7, ESPURIA-TELA c11 e DST c5**, conforme os commits de catraca informados; o HEAD registra posições medidas. A atribuição individual de cada linha a cada pouso está **[não verificado]**. [HEAD: test_medida_turno_do_par.py:247](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/tests/test_medida_turno_do_par.py:247) | **Não decidida**: a cópia registra deslocamentos LASTRO, mas os valores finais dependem de nova medição da árvore integrada. [cópia: test_medida_turno_do_par.py:247](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/core/tests/test_medida_turno_do_par.py:247), [HEAD: teste:559](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/tests/test_medida_turno_do_par.py:559) |
| `cartorio.py` | **DST c5** e sua linhagem DST: o fecho passa a chamar a rejeição em lote dentro da transação; o predicado de protesto também passa pelo juiz de dia sem trabalho. [HEAD: cartorio.py:1028](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/cartorio.py:1028), [HEAD: cartorio.py:1480](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/cartorio.py:1480) | **Necessária**: LASTRO transporta campos da cobertura e amplia a leitura de cobertura inteira; o HEAD ainda usa o predicado anterior nesse ponto. [cópia: cartorio.py:773](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/cartorio.py:773), [cópia: cartorio.py:1253](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/cartorio.py:1253), [HEAD: cartorio.py:1273](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/cartorio.py:1273) |
| `dia_decidido.py` | **ESPURIA-TELA c11**, pela linhagem informada: acrescenta ícone de classe e trilha da ausência que deu a palavra. [HEAD: dia_decidido.py:196](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/dia_decidido.py:196), [HEAD: dia_decidido.py:438](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/dia_decidido.py:438) | **Necessária** para a saída LASTRO: a cópia reconhece também `folga_da_escala`; o HEAD reconhece apenas `folga` nesse ramo. [cópia: dia_decidido.py:371](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/dia_decidido.py:371), [HEAD: dia_decidido.py:400](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/dia_decidido.py:400) |
| `espelho.py` | **ESPURIA-TELA c11**, pela linhagem informada: produz e limpa trilha da decisão e `classe_dia`. [HEAD: espelho.py:344](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/espelho.py:344) | **Necessária**: LASTRO propaga e apresenta a recusa por dia. [cópia: espelho.py:339](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/espelho.py:339), [cópia: espelho.py:649](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/espelho.py:649) |

A lista de commits fornecida não demonstra alteração adicional desses cinco atribuível a **F6F7, F9, F3 ou DST c6 v4**; atribuição por esses pousos: **[não verificado]**. [nao verificado]

**Conflitos novos.**

- **`calendario.py`: exige decisão de integração e verificação, não apenas escolher um lado.** O HEAD lê `veredito`, `palavra`, `cor` e a trilha de decisão do retorno de `do_dia`; LASTRO substitui esse retorno por um dicionário processado por `aplicar_palavra_do_dia` e lê as chaves terminadas em `_dia`. [HEAD: calendario.py:757](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/colaboradores/services/calendario.py:757), [cópia: calendario.py:710](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/colaboradores/services/calendario.py:710) A dificuldade concreta é que LASTRO chama o compositor com `limpar=True`, enquanto o HEAD limpa nessa opção justamente as três chaves da trilha que o calendário precisa entregar; a forma de preservar a trilha na tela e a limpeza no documento precisa ser acertada e verificada. [cópia: calendario.py:719](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/colaboradores/services/calendario.py:719), [HEAD: espelho.py:355](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/espelho.py:355)
- **`espelho.py`: união textual orientada pelo contrato, com verificação funcional.** No trecho conflitado, o HEAD acrescenta à limpeza a trilha e `classe_dia`; LASTRO acrescenta `_recusa_do_dia`. O texto da docstring também diverge na contagem das chaves. A união das chaves é identificável, mas o ramo de recusa e a possibilidade de deixar dados anteriores no dicionário integrado precisam ser verificados; não classifico a integração inteira como puramente mecânica. [HEAD: espelho.py:317](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/espelho.py:317), [cópia: espelho.py:334](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/espelho.py:334), [cópia: espelho.py:350](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/espelho.py:350)

**Conflitos antigos.** A natureza do teste continua sendo a catraca de posições, agora com mais deslocamentos a medir após a integração; não se deve escolher os números de um lado por simples precedência. [HEAD: test_medida_turno_do_par.py:247](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/tests/test_medida_turno_do_par.py:247), [cópia: teste:247](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/core/tests/test_medida_turno_do_par.py:247) Em `ausencia.py`, agora é possível identificar o trecho: o HEAD renomeou o parâmetro para `tipo_ausencia`, enquanto LASTRO insere validação de código e faixa usando o nome antigo `tipo`; integrar requer adaptar a validação ao parâmetro atual e preservar os dois comportamentos. O pré-mapa não identificava o trecho, portanto não permite concluir que sua natureza **mudou** desde então. [HEAD: ausencia.py:1761](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/ausencia.py:1761), [cópia: ausencia.py:1303](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/ausencia.py:1303)

**Ordem sugerida para o rebase futuro, por dependência de arquivo:** `precedencia.py` e catálogo → `ausencia.py` e produtores de cobertura/grade → `dia_pago.py`, `cartorio.py` e `dia_decidido.py` → `espelho.py` → `calendario.py`, `export.py` e demais leitores → testes e fixtures → remedição final de `test_medida_turno_do_par.py`. É uma ordem de trabalho sugerida, não uma prova de ordenação obrigatória de todos os commits. A precedência fornece faixa e fatos; o espelho compõe os dados usados pelo calendário; o teste congela posições da árvore resultante. [cópia: precedencia.py:105](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/precedencia.py:105), [cópia: espelho.py:297](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/espelho.py:297), [cópia: calendario.py:710](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/colaboradores/services/calendario.py:710), [HEAD: teste:559](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/tests/test_medida_turno_do_par.py:559)

**Folha, dia pago e retenção.** O toque de LASTRO em `export.py` é agora identificável: na prontidão sem snapshot, troca a derivação local pela leitura de `por_dia_da_grade`; é necessário para acompanhar a mudança de cobertura da cópia. [HEAD: export.py:1093](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/folha/export.py:1093), [cópia: export.py:1093](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/folha/export.py:1093) Em `dia_pago.py`, LASTRO amplia o reconhecimento de ausência com `ausencia_tipo` e declara preservar a fórmula legada; não há evidência, entre os cinco arquivos medidos como alterados hoje, de mudança direta nesse arquivo. [cópia: dia_pago.py:385](/home/ronald/codex_ws/f3ebd821b71369c9e790c8352b57b00c8a33c1de/app/ponto/services/dia_pago.py:385), [não verificado]

A comparação da cópia com sua base mostra o toque de `export.py` na prontidão, sem alteração direta em `motivos_retencao_celula`; já DST c5 usa um **portão de retenção de retratação** na rejeição de justificativas, assunto relacionado ao fecho e distinto da função de retenção da folha. Consequência indireta conjunta após integração: **[não verificado]**. [HEAD: export.py:1176](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/folha/export.py:1176), [HEAD: justificativa.py:292](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/justificativa.py:292)

**38 arquivos necessários [provisório; não verificado], 4 conflitos, ordem: precedência e catálogo → ausência e cobertura → dia pago, cartório e dia decidido → espelho → calendário, exportação e leitores → testes e medição, perguntas de lei: [não verificado].** [nao verificado]

## 19/11 12:11 · sessao f8349cca706f · CADEIA-DO-CONTRATO-20 · leitura · origem=Code

**NÃO PROCEDE literalmente — AVAL-DEVOLVIDO.** As duas listas estão vazias e o aval mantém a célula parcial, mas **o texto da nota não é interpretado para decidir a cor**. A célula está não verde porque sua declaração omite `verde`, cujo padrão é `False`. A nota documenta a razão dessa declaração. [app/config/crons.py:2061](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/config/crons.py:2061), [app/core/juizes.py:481](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/juizes.py:481), [app/core/contratos_estruturais.py:62](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/contratos_estruturais.py:62), [app/core/contratos_estruturais.py:262](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/contratos_estruturais.py:262).

- A primeira tupla começa em `JUIZES_POR_VARREDURA: tuple[str, ...] = (` e termina sem elementos ativos; a segunda é literalmente `PENDENTES['chamado'] = ( )`. A célula aponta para ela por `PENDENTES_CHAMADO = PENDENTES['chamado']`. [app/config/crons.py:2061](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/config/crons.py:2061), [app/config/crons.py:2126](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/config/crons.py:2126), [app/core/juizes.py:481](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/juizes.py:481), [app/core/juizes.py:1382](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/juizes.py:1382).
- A nota mora na própria célula: **“esta celula fica PARCIAL ate a O168, a O169 e o LASTRO”**. O aval que a originou está identificado como **CELULA-CHAMADO-PARCIAL-ATE-O168-O169-LASTRO**, de 08/10; o texto também proíbe reclassificar papel para virar verde. [app/core/contratos_estruturais.py:270](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/contratos_estruturais.py:270), [app/docs/PROMPTS.md:1083](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/PROMPTS.md:1083).
- Quem lê a nota: o placar a inclui para células não verdes; o selo da matriz exige que uma célula não verde tenha nota não vazia. Nenhum desses trechos interpreta O168, O169 ou LASTRO como condição executável. [app/core/placar_tickets.py:346](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/placar_tickets.py:346), [app/core/tests/test_selo_contratos_estruturais.py:98](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/tests/test_selo_contratos_estruturais.py:98).
- `total()` calcula o **teto**, sem ler nota nem cor; `verdes()` conta as declarações com `verde=True`. A leitura do placar usa ambos e identifica as células com `verde=False`. [app/core/contratos_estruturais.py:1038](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/contratos_estruturais.py:1038), [app/core/contratos_estruturais.py:1053](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/contratos_estruturais.py:1053), [app/core/arquitetura_leitura.py:49](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/arquitetura_leitura.py:49).

A condição literal, com os estados documentados, é a seguinte:

| Item | Texto do aval ou registro literal | Estado documentado | O que o selo da célula mede |
|---|---|---|---|
| Aval 1 | “celula chamado x juiz fica PARCIAL ate O168, O169 e o LASTRO; proibido reclassificar papel para virar verde.” [app/docs/PROMPTS.md:1083](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/PROMPTS.md:1083) | A condição permanece registrada na nota. O corte posterior de chegada lista O168 e LASTRO, com O169 já pousada. [app/core/contratos_estruturais.py:270](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/contratos_estruturais.py:270), [app/docs/CORTES.md:469](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/CORTES.md:469) | Não interpreta essa condição. [app/chamados/tests/test_contract_juiz_chamado.py:22](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/tests/test_contract_juiz_chamado.py:22) |
| O168 | “a guarda de conversa do supra_juiz (pode_executar_csf) vale tambem no evento”; o `--executar` sai depois e com `CSF_RETRATADO` medido em zero. [app/docs/BACKLOG.md:1544](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:1544) | BACKLOG registra pouso em `89a5b0d85`; o complemento do gate registra **NO AR** com `d33c0a899`. Valor de `CSF_RETRATADO` em produção hoje: **[não verificado]**. [app/docs/BACKLOG.md:1544](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:1544), [app/docs/BACKLOG.md:2176](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:2176) | A classe da célula não mede o contador de produção nem comprova essa sequência. [app/chamados/tests/test_contract_juiz_chamado.py:22](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/tests/test_contract_juiz_chamado.py:22) |
| O169 | A decisão registrada é “cluster nao fecha por idade”, com RED de 15 dias. [app/docs/BACKLOG.md:1944](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:1944) | C20-O169 consta **POUSADA** em 05/11; commit do pouso **[não verificado]**. A linha O169-FECHADA registra separadamente contador e mutante como delta, em B3 desenho; conclusão desse delta hoje **[não verificado]**. [app/docs/BACKLOG.md:1944](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:1944), [app/docs/BACKLOG.md:2254](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:2254) | A classe da célula não executa o caso de cluster com 15 dias nem mede `fechados_por_idade`. [app/chamados/tests/test_contract_juiz_chamado.py:22](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/tests/test_contract_juiz_chamado.py:22) |
| LASTRO | “todo tipo ATIVO no mapa ... com faixa nomeada; faixa_de fora do mapa = recusa nomeada; contador tipos_ativos_sem_faixa esperado 0; RED pk 5356 + tipo fantasma”. [app/docs/BACKLOG.md:1965](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:1965) | **ABERTA**; depende do acessor TIPO-NA-FONTE. `f3ebd821b` é a **base da cópia**, não um commit de pouso. A cadeia exige base refeita, DIFF de frota e proíbe virar a célula antes do passo 3. [app/docs/BACKLOG.md:1965](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:1965), [app/docs/BACKLOG.md:1960](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:1960) | A classe da célula não mede cobertura de faixas, recusa, contador ou RED do LASTRO. [app/chamados/tests/test_contract_juiz_chamado.py:22](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/tests/test_contract_juiz_chamado.py:22) |

O selo nomeado em `teste=` é `JuizChamadoContratoTest`: confere existência das autoridades declaradas, ausência de perguntas sem juiz, teto zero dos pendentes e padrões de resposta fora da autoridade, com casos que exercitam sua detecção e verificações específicas de SLA e COBRANÇA. Ele **não muda a declaração `verde`**. [app/core/contratos_estruturais.py:262](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/contratos_estruturais.py:262), [app/chamados/tests/test_contract_juiz_chamado.py:24](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/tests/test_contract_juiz_chamado.py:24), [app/chamados/tests/test_contract_juiz_chamado.py:85](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/tests/test_contract_juiz_chamado.py:85).

**Sem a nota, a célula continuaria não verde**, pois `verde` continuaria no padrão `False`; ademais, o selo da matriz rejeitaria a célula não verde sem nota. Declarada verde, a matriz verifica a existência da classe e as exceções vazias, mas esses testes não transformam as condições textuais dos avais em verificações executáveis. Resultado atual de execução da classe: **[não verificado]**. [app/core/contratos_estruturais.py:62](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/contratos_estruturais.py:62), [app/core/tests/test_selo_contratos_estruturais.py:71](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/tests/test_selo_contratos_estruturais.py:71), [app/core/tests/test_selo_contratos_estruturais.py:98](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/tests/test_selo_contratos_estruturais.py:98).

**falta:** LASTRO no ar após os passos anteriores da cadeia; prova atual de `CSF_RETRATADO = 0` **[não verificado]**; estado final do delta de contador e mutante da O169 **[não verificado]**. A cadeia proíbe o verde antes do passo 3. [app/docs/BACKLOG.md:1960](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:1960), [app/docs/BACKLOG.md:1544](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:1544), [app/docs/BACKLOG.md:2254](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/BACKLOG.md:2254).

**perguntas de lei: 0** identificadas nesta leitura do passo 4. [não verificado para os passos 2 e 3]

## 19/11 12:02 · sessao 689240b7551f · CADEIA-20-PASSO-1 · construtor · origem=Code

A cura está em [orfa_classe.py:31](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/orfa_classe.py:31): `razao_da_sobra` consulta `_carimbada_como_miolo`, que reconhece os três carimbos ([turnos.py:932](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/turnos.py:932)). Escolhi import local para evitar dependência no carregamento entre os módulos; `ponto.turnos` também importa `escala.orfa_classe` localmente ([turnos.py:1581](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/turnos.py:1581)). Os papéis de entrada, saída e aberto continuam lidos dos campos do `Turno` ([orfa_classe.py:65](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/orfa_classe.py:65)).

A fixture agora explicita os três atributos ([teste:13](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_razao_da_sobra.py:13)). O novo caso usa somente `_intra_ata=True` ([teste:112](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_razao_da_sobra.py:112)): [nao verificado]

- **RED → verde:** antes da cura, retornou `PERIODO_ADICIONAL` e falhou; depois, passou. No executor isolado, os 13 testes passaram. [nao verificado]
- **Mutante:** substituir a chamada por `getattr(b, 'is_intervalo', False)` fez o novo caso falhar com `PERIODO_ADICIONAL`; mutante morto.
- A execução Django real e a certificação pela suíte permanecem **[não verificado]**: nesta cópia, `python` não está disponível e `python3` não encontra `django.core`. [nao verificado]

Para reproduzir os resultados isolados, execute o bloco abaixo no diretório `app`. Ele usa o teste da cópia e extrai a função de autoridade do arquivo real; a mutação ocorre somente em memória. [nao verificado]

```bash
verificar() {
  PYTHONDONTWRITEBYTECODE=1 MUTANTE="$1" python3 - <<'PY'
import ast, collections, os, sys, types, unittest

source = open('ponto/turnos.py').read()
node = next(n for n in ast.parse(source).body
            if isinstance(n, ast.FunctionDef) and n.name == '_carimbada_como_miolo')
ponto = types.ModuleType('ponto.turnos')
exec(compile(ast.Module(body=[node], type_ignores=[]), 'ponto/turnos.py', 'exec'), ponto.__dict__)
ponto.Turno = collections.namedtuple(
    'Turno', 'data_turno entrada saida batidas aberto cross_meianoite')
sys.modules['ponto.turnos'] = ponto

django = types.ModuleType('django')
django.__path__ = []
test = types.ModuleType('django.test')
test.SimpleTestCase = unittest.TestCase
sys.modules.update({'django': django, 'django.test': test})

if os.environ['MUTANTE'] == '1':
    source = open('escala/orfa_classe.py').read()
    old = '_carimbada_como_miolo(b) for b, _ in encontrados'
    assert source.count(old) == 1
    source = source.replace(old, "getattr(b, 'is_intervalo', False) for b, _ in encontrados")
    module = types.ModuleType('escala.orfa_classe')
    exec(compile(source, 'escala/orfa_classe.py', 'exec'), module.__dict__)
    sys.modules['escala.orfa_classe'] = module
    nome = 'escala.tests.test_razao_da_sobra.RazaoDaSobraTest.test_carimbo_intra_ata_no_turno'
else:
    nome = 'escala.tests.test_razao_da_sobra'

suite = unittest.defaultTestLoader.loadTestsFromName(nome) [nao verificado]
sys.exit(not unittest.TextTestRunner(verbosity=2).run(suite).wasSuccessful()) [nao verificado]
PY
}
verificar 0  # verde: 13 testes [nao verificado]
verificar 1  # mutante: falha esperada no novo caso
```

Com Django disponível, o comando do teste real é: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test escala.tests.test_razao_da_sobra --noinput
```

## 19/11 12:01 · sessao 848b07639c50 · ESCALA-NAO-DESCREVE · sonda · origem=Code

**Resultado:** a sonda informa 19 casos `nenhuma`, 9 `nova==cadastro` e 2 `velha==cadastro`; no corte A3, informa 15 chaves com mais de um bloco, em 9 colaboradores. A saída informa `ERROS=0` e `CONTADOS=34 (esperado 34)`. [não verificado: saída fornecida no pedido, sem arquivo e linhas locais]

Os valores abaixo são transcrições do envelope fornecido, não uma nova leitura da sombra. Como o envelope não tem arquivo:linha disponível na cópia, suas afirmações estão marcadas `[não verificado]`. O diretório também não tem metadados Git disponíveis para confirmar o hash do HEAD. [não verificado]

## 1. As 30 linhas A1

Em **cadastro**, **velha** e **nova**, a notação é `pk: entrada–saída / início–fim do intervalo (cruza)`. `—` significa sem intervalo; `∅`, sem proposta. Em **civil** e **núcleo**, a notação é `entrada/saída; marcos; dias`. As classes comparam os marcos informados, não a igualdade dos PKs. [não verificado: linhas A1 fornecidas]

| Vínculo:colab | Cadastro | Velha | Nova | Civil | Núcleo | Classe |
|---|---|---|---|---|---|---|
| 1140:85 | 483: 19:00–07:00/— (1) | 39: 07:00–07:00/— (1) | 378: 21:00–07:00/— (1) | 06:57/06:57; 1; 24 | 20:50/06:57; 2; 12 | nenhuma |
| 821:110 | 180: 19:00–07:00/02:00–03:00 (1) | 172: 19:00–07:00/— (1) | 380: 19:00–07:00/01:00–02:00 (1) | 18:55/07:00; 1; 25 | 18:55/07:00; 4; 11 | nenhuma |
| 829:111 | 180: 19:00–07:00/02:00–03:00 (1) | 172: 19:00–07:00/— (1) | 380: 19:00–07:00/01:00–02:00 (1) | 18:55/07:00; 1; 44 | 18:55/07:00; 4; 22 | nenhuma |
| 832:121 | 180: 19:00–07:00/02:00–03:00 (1) | 172: 19:00–07:00/— (1) | 380: 19:00–07:00/01:00–02:00 (1) | 18:59/07:01; 1; 60 | 19:00/07:01; 4; 26 | nenhuma |
| 1027:134 | 179: 19:00–07:00/— (1) | 39: 07:00–07:00/— (1) | 172: 19:00–07:00/— (1) | 06:58/06:58; 1; 44 | 18:55/06:58; 2; 22 | nova==cadastro |
| 187:219 | 180: 19:00–07:00/02:00–03:00 (1) | 172: 19:00–07:00/— (1) | 380: 19:00–07:00/01:00–02:00 (1) | 19:04/07:04; 1; 60 | 19:04/07:03; 4; 28 | nenhuma |
| 219:253 | 378: 21:00–07:00/— (1) | 39: 07:00–07:00/— (1) | 172: 19:00–07:00/— (1) | 06:55/06:55; 1; 60 | 18:56/06:55; 2; 30 | nenhuma |
| 238:272 | 235: 18:00–06:00/01:00–02:00 (1) | 182: 18:00–06:00/— (1) | 235: 18:00–06:00/01:00–02:00 (1) | 17:52/05:55; 1; 34 | 17:52/05:55; 4; 16 | nova==cadastro |
| 247:282 | 172: 19:00–07:00/— (1) | 172: 19:00–07:00/— (1) | 380: 19:00–07:00/01:00–02:00 (1) | 18:59/07:00; 3; 58 | 18:59/07:00; 4; 30 | velha==cadastro |
| 255:290 | 179: 19:00–07:00/— (1) | 39: 07:00–07:00/— (1) | 172: 19:00–07:00/— (1) | 07:00/07:00; 1; 57 | 19:00/07:00; 2; 28 | nova==cadastro |
| 1576:340 | 172: 19:00–07:00/— (1) | 39: 07:00–07:00/— (1) | 209: 18:30–06:30/— (1) | 06:30/06:30; 1; 45 | 18:33/06:30; 2; 21 | nenhuma |
| 302:344 | 186: 19:00–07:00/23:00–00:00 (1) | 172: 19:00–07:00/— (1) | 380: 19:00–07:00/01:00–02:00 (1) | 18:43/06:59; 1; 28 | 18:43/06:59; 4; 14 | nenhuma |
| 1336:416 | 196: 19:00–07:00/03:00–04:00 (1) | 39: 07:00–07:00/— (1) | 172: 19:00–07:00/— (1) | 07:00/07:00; 1; 42 | 18:55/06:56; 2; 19 | nenhuma |
| 1334:418 | 180: 19:00–07:00/02:00–03:00 (1) | 172: 19:00–07:00/— (1) | 380: 19:00–07:00/01:00–02:00 (1) | 19:00/07:00; 3; 45 | 19:00/07:00; 4; 22 | nenhuma |
| 401:467 | 183: 19:00–07:00/00:00–01:00 (1) | 172: 19:00–07:00/— (1) | 380: 19:00–07:00/01:00–02:00 (1) | 18:51/06:51; 3; 51 | 18:51/06:51; 4; 24 | nenhuma |
| 425:496 | 179: 19:00–07:00/— (1) | 39: 07:00–07:00/— (1) | 172: 19:00–07:00/— (1) | 07:00/07:00; 1; 52 | 18:58/07:00; 2; 23 | nova==cadastro |
| 1150:502 | 278: 10:00–18:20/14:00–15:00 (0) | 197: 07:00–19:00/14:00–15:00 (0) | 204: 10:20–19:29/14:40–15:40 (0) | 06:59/19:00; 4; 50 | 09:54/19:00; 4; 46 | nenhuma |
| 1164:511 | 186: 19:00–07:00/23:00–00:00 (1) | 39: 07:00–07:00/— (1) | 172: 19:00–07:00/— (1) | 07:01/07:01; 1; 57 | 18:58/07:01; 2; 27 | nenhuma |
| 950:612 | 180: 19:00–07:00/02:00–03:00 (1) | 172: 19:00–07:00/— (1) | 380: 19:00–07:00/01:00–02:00 (1) | 18:50/06:50; 1; 34 | 18:50/06:50; 4; 15 | nenhuma |
| 994:841 | 380: 19:00–07:00/01:00–02:00 (1) | 172: 19:00–07:00/— (1) | 380: 19:00–07:00/01:00–02:00 (1) | 18:56/06:54; 1; 58 | 18:56/06:54; 4; 29 | nova==cadastro |
| 1006:854 | 34: 19:00–07:00/— (1) | 39: 07:00–07:00/— (1) | 172: 19:00–07:00/— (1) | 06:51/06:51; 1; 60 | 18:55/06:51; 2; 30 | nova==cadastro |
| 1337:877 | 203: 10:00–22:00/14:00–15:00 (0) | 193: 10:30–22:30/15:30–16:30 (0) | 185: 10:00–22:00/13:00–14:00 (0) | 10:56/22:09; 4; 28 | 09:59/22:09; 4; 25 | nenhuma |
| 1478:888 | 172: 19:00–07:00/— (1) | 39: 07:00–07:00/— (1) | 172: 19:00–07:00/— (1) | 07:05/07:05; 1; 58 | 19:01/07:05; 2; 28 | nova==cadastro |
| 1388:898 | 253: 12:30–00:30/19:00–20:00 (1) | 314: 00:00–00:00/— (1) | 253: 12:30–00:30/19:00–20:00 (1) | 00:30/00:30; 3; 45 | 12:28/00:30; 4; 22 | nova==cadastro |
| 1594:900 | 35: 07:00–19:00/— (0) | 212: 07:00–16:00/— (0) | 365: 07:00–15:50/12:00–13:00 (0) | 06:55/15:54; 1; 39 | 06:54/15:54; 4; 28 | nenhuma |
| 1181:926 | 496: 11:00–20:00/16:00–17:00 (0) | 541: 11:00–20:00/15:30–17:00 (0) | 291: 08:00–20:00/12:00–13:00 (0) | 11:00/20:00; 4; 43 | 08:00/20:00; 4; 30 | nenhuma |
| 1592:932 | 35: 07:00–19:00/— (0) | 274: 10:00–22:00/— (0) | 177: 07:00–19:00/— (0) | 10:00/22:00; 2; 30 | 06:52/18:54; 2; 18 | nova==cadastro |
| 1186:935 | 172: 19:00–07:00/— (1) | 172: 19:00–07:00/— (1) | 380: 19:00–07:00/01:00–02:00 (1) | 18:50/06:50; 3; 58 | 18:50/06:50; 4; 29 | velha==cadastro |
| 1511:993 | 178: 07:00–19:00/12:00–13:00 (0) | 197: 07:00–19:00/14:00–15:00 (0) | 177: 07:00–19:00/— (0) | 07:00/19:00; 4; 18 | 07:00/19:00; 2; 14 | nenhuma |
| 1123:915 | 279: 14:40–23:00/18:00–19:00 (0) | 449: 12:00–20:20/16:00–17:00 (0) | ∅; cruza — | 11:48/20:24; 4; 19 | 11:48/17:27; 4; 16 | nenhuma |

**CLASSES:** `nenhuma=19 nova==cadastro=9 velha==cadastro=2`. [não verificado: linha CLASSES fornecida]

### Por que a proposta muda

O detector civil agrupa batidas pela data local e toma, para cada dia, a primeira e a última; calcula separadamente as modas dessas bordas e do número de batidas. O núcleo, no ramo sequencial, corta blocos por gaps e vão, atribui cada bloco à data da primeira batida e acumula suas horas nessa chave. Isso permite que madrugada e noite sejam separadas no civil e reunidas no núcleo. [app/escala/detector_proposta.py:53](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/detector_proposta.py:53), [app/ponto/calculador/nucleo.py:186](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/calculador/nucleo.py:186)

O casamento exige entrada e saída a até 30 minutos do template e escolhe a menor soma das distâncias. Para intervalo, testa apenas se `marcos_moda >= 4` e se o template **tem** os dois marcos de intervalo; não compara seus horários com batidas ou cadastro. O código tampouco usa `cruza` nesse casamento. [app/escala/detector_proposta.py:34](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/detector_proposta.py:34), [app/escala/detector_proposta.py:99](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/detector_proposta.py:99)

- **Entrada muda de madrugada para noite:** 1140:85, 1027:134, 219:253, 255:290, 1576:340, 1336:416, 425:496, 1164:511 e 1006:854 e 1478:888 passam de entrada civil próxima da saída para entrada de bloco noturna; 1388:898 também muda a entrada e a moda de marcos. A troca observada acompanha essa diferença de chave e de bordas. [não verificado: linhas A1 fornecidas; mecanismo em [detector_proposta.py:60](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/detector_proposta.py:60) e [nucleo.py:236](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/calculador/nucleo.py:236)]
- **Mesmas bordas aproximadas, intervalo muda:** 821:110, 829:111, 832:121, 187:219, 238:272, 247:282, 302:344, 1334:418, 401:467, 950:612, 994:841, 1594:900 e 1186:935 têm moda civil de 1 ou 3 e nuclear de 4; 1511:993 faz o inverso, de 4 para 2. Isso muda o filtro de presença de intervalo. O horário específico do intervalo escolhido continua dependendo do catálogo e da ordem em empate, não de comparação dos marcos internos. [não verificado: linhas A1 fornecidas; regra em [detector_proposta.py:105](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/detector_proposta.py:105)]
- **Bordas modais diferentes com mesma categoria de marcos:** em 1150:502, 1337:877, 1181:926 e 1592:932 a entrada medida muda e, com ela, o candidato mais próximo. O envelope não contém as batidas completas nem todos os candidatos para atribuir a diferença modal a um evento individual. [não verificado: linhas A1 fornecidas; regra em [detector_proposta.py:113](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/detector_proposta.py:113)]
- **1123:915:** a saída modal muda de 20:24 para 17:27; a referência informa proposta nova `NONE` e confiança insuficiente. Com 16 dias, o limite de cinco dias não explica a ausência; o código também retorna insuficiente quando nenhum template casa as bordas e a categoria de intervalo. O catálogo completo não foi fornecido para verificar os candidatos individualmente. [não verificado: A1 e referência fornecidos; regra em [detector_proposta.py:125](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/detector_proposta.py:125)]

## 2. A2: batidas e medições por dia

Datas são de novembro de 2026. Em cada linha, **núcleo** mostra `chave: horas; ímpar`; `∅` reproduz `horas=None`. A chave impressa é a data da linha mesmo quando não há horas nessa chave. As tabelas reproduzem somente as linhas A2 fornecidas, sem inventar dias ausentes. [não verificado: linhas A2 fornecidas]

**845:146** — cadastro pk 383, `00:00–23:59/—`, cruza 0; velha pk 39; `nbatidas=6`. [não verificado]

| Dia | Batidas locais | Civil E/S; n | Núcleo: chave, horas; ímpar |
|---|---|---|---|
| 09 | 06:30 | 06:30/06:30; 1 | 09: ∅; falso |
| 10 | 21:29 | 21:29/21:29; 1 | 10: 21:29, 06:31; falso |
| 11 | 06:31 | 06:31/06:31; 1 | 11: ∅; falso |
| 16 | 21:31 | 21:31/21:31; 1 | 16: 21:31, 06:35; falso |
| 17 | 06:35 | 06:35/06:35; 1 | 17: ∅; falso |

O civil vê uma batida em cada data exibida, portanto primeira e última coincidem. O núcleo reúne as duas noites exibidas com suas saídas da manhã seguinte. Nenhuma dessas bordas reproduz o cadastro `00:00–23:59`; a referência de outra janela informa nova pk 500 e E/S nuclear modal `21:29/06:30`. A sexta batida contada no cabeçalho não aparece nas linhas A2 transcritas. [não verificado: A2 e sonda34d_ref.out fornecidos; agrupamento em [detector_proposta.py:60](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/detector_proposta.py:60) e [nucleo.py:236](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/calculador/nucleo.py:236)]

**1547:220** — cadastro pk 591, `22:00–06:00/02:00–03:00`, cruza 1; velha pk 252; `nbatidas=24`. [não verificado]

| Dia | Batidas locais | Civil E/S; n | Núcleo: chave, horas; ímpar |
|---|---|---|---|
| 10 | 21:54 | 21:54/21:54; 1 | 10: 21:54, 03:35, 04:31, 06:00; falso |
| 11 | 03:35, 04:31, 06:00 | 03:35/06:00; 3 | 11: ∅; falso |
| 12 | 21:59 | 21:59/21:59; 1 | 12: 21:59, 04:01, 05:00, 06:00; falso |
| 13 | 04:01, 05:00, 06:00, 22:00 | 04:01/22:00; 4 | 13: 22:00, 02:00, 03:02, 06:00; falso |
| 14 | 02:00, 03:02, 06:00, 21:58 | 02:00/21:58; 4 | 14: 21:58, 02:02, 03:00, 06:00; falso |
| 15 | 02:02, 03:00, 06:00, 21:57 | 02:02/21:57; 4 | 15: 21:57, 02:09, 03:00, 06:00; falso |
| 16 | 02:09, 03:00, 06:00 | 02:09/06:00; 3 | 16: ∅; falso |
| 17 | 21:54 | 21:54/21:54; 1 | 17: 21:54, 02:00, 03:00, 06:01; falso |
| 18 | 02:00, 03:00, 06:01 | 02:00/06:01; 3 | 18: ∅; falso |

Nesses dias, o civil separa ou mistura bordas de jornadas em datas civis; o núcleo mostra jornadas noturnas próximas ao cadastro, embora alguns intervalos exibidos sejam diferentes. A referência de janela mais ampla informa, porém, nova e velha pk 252 e E/S nuclear modal `15:00/23:00`, divergente do cadastro. **A2 não mostra por que essa moda da janela completa prevalece.** [não verificado: A2 e sonda34d_ref.out fornecidos; regras em [detector_proposta.py:70](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/detector_proposta.py:70) e [nucleo.py:236](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/calculador/nucleo.py:236)]

**1597:489** — cadastro pk 172, `19:00–07:00/—`, cruza 1; velha pk 252; `nbatidas=23`. [não verificado]

| Dia | Batidas locais | Civil E/S; n | Núcleo: chave, horas; ímpar |
|---|---|---|---|
| 09 | 16:01, 20:00, 21:00 | 16:01/21:00; 3 | 09: 16:01, 20:00, 21:00, 00:00; falso |
| 10 | 00:00 | 00:00/00:00; 1 | 10: ∅; falso |
| 11 | 19:00, 23:00 | 19:00/23:00; 2 | 11: 19:00, 23:00, 00:00, 07:00; falso |
| 12 | 00:00, 07:00 | 00:00/07:00; 2 | 12: ∅; falso |
| 13 | 19:00, 23:00 | 19:00/23:00; 2 | 13: 19:00, 23:00, 00:00, 07:00; falso |
| 14 | 00:00, 07:00 | 00:00/07:00; 2 | 14: ∅; falso |
| 15 | 19:00, 23:00 | 19:00/23:00; 2 | 15: 19:00, 23:00, 00:00, 07:00; falso |
| 16 | 00:00, 07:00 | 00:00/07:00; 2 | 16: ∅; falso |
| 18 | 19:00, 23:00 | 19:00/23:00; 2 | 18: 19:00, 23:00, 00:00; verdadeiro |

O civil divide as jornadas de 19:00–07:00 entre datas. O núcleo as reúne, mas as linhas completas exibidas têm quatro marcos, enquanto o cadastro não tem intervalo; a última é ímpar. A referência da janela mais ampla informa nova e velha pk 252 e moda nuclear `15:00/23:00`; os dez dias exibidos não explicam a prevalência dessa moda. [não verificado: A2 e sonda34d_ref.out fornecidos; regra de intervalo em [detector_proposta.py:105](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/detector_proposta.py:105)]

**1604:934** — cadastro pk 590, `23:00–11:00/05:00–06:00`, cruza 1; velha pk 177; `nbatidas=10`. [não verificado]

| Dia | Batidas locais | Civil E/S; n | Núcleo: chave, horas; ímpar |
|---|---|---|---|
| 09 | 22:57 | 22:57/22:57; 1 | 09: 22:57, 11:09; falso |
| 10 | 11:09 | 11:09/11:09; 1 | 10: ∅; falso |
| 11 | 23:01 | 23:01/23:01; 1 | 11: 23:01, 11:00; falso |
| 12 | 11:00 | 11:00/11:00; 1 | 12: ∅; falso |
| 13 | 22:57 | 22:57/22:57; 1 | 13: 22:57, 11:00; falso |
| 14 | 11:00 | 11:00/11:00; 1 | 14: ∅; falso |
| 15 | 22:58 | 22:58/22:58; 1 | 15: 22:58, 11:04; falso |
| 16 | 11:04 | 11:04/11:04; 1 | 16: ∅; falso |
| 17 | 23:27 | 23:27/23:27; 1 | 17: 23:27, 11:40; falso |
| 18 | 11:40 | 11:40/11:40; 1 | 18: ∅; falso |

O civil vê uma batida por data; o núcleo reúne cada entrada com a saída da manhã seguinte, mas nessas jornadas mostra dois marcos, sem os dois marcos de intervalo do cadastro. A referência da janela mais ampla informa nova pk 197 e E/S nuclear modal `07:01/19:00`; os dez dias exibidos não explicam essa moda. [não verificado: A2 e sonda34d_ref.out fornecidos; regras em [detector_proposta.py:70](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/detector_proposta.py:70) e [detector_proposta.py:105](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/escala/detector_proposta.py:105)]

## 3. A3: contagem de chaves multibloco

O corte referenciado da autópsia deduplica batidas a menos de dez minutos e separa blocos por gap; essa contagem replicada não deve ser confundida automaticamente com o corte atual do núcleo, que não faz esse dedup e também tem limite de vão. [app/ponto/management/commands/e6_piso_autopsia.py:65](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/management/commands/e6_piso_autopsia.py:65), [app/ponto/calculador/nucleo.py:45](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/calculador/nucleo.py:45), [app/ponto/calculador/nucleo.py:215](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/calculador/nucleo.py:215)

| Colab pk | Chaves | Chaves com >1 bloco |
|---|---:|---:|
| 109 | 27 | 1 |
| 328 | 34 | 1 |
| 451 | 54 | 1 |
| 466 | 30 | 2 |
| 467 | 25 | 1 |
| 638 | 23 | 1 |
| 877 | 28 | 1 |
| 911 | 40 | 4 |
| 932 | 29 | 3 |

**A3_TOTAL:** `universo_colabs=233 chaves=7524 chaves_multibloco=15 colabs_com_multibloco=9`. [não verificado: A3 fornecido]

**Controle da sonda principal:** `ERROS=0 UNIVERSO=233 CONTADOS=34 (esperado 34)`. **Controle do anexo de referência:** `ERROS=0 UNIVERSO=233 CONTADOS=233`. [não verificado: envelopes fornecidos]

## 19/11 11:58 · sessao ddd2b81beddc · CADEIA-20-PASSO-1 · construtor · origem=Code

A volta 1 está implementada, **com uma limitação de evidência**: os casos usam fixtures sintéticas construídas a partir das respostas da regra. O cadastro literal dos casos nominais e a identificação de um colaborador real para substituir col950 não estão no dado fornecido **[não verificado]** ([test_razao_da_sobra.py:1](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_razao_da_sobra.py:1)).

**Aval devolvido, por item:**

1. As interfaces verificadas na cópia incluem `Turno.data_turno` e o carimbo de pausa ([turnos.py:43](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/turnos.py:43), [turnos.py:1371](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/turnos.py:1371)).
2. A implementação segue a precedência fornecida quando há uma batida identificável; ausência, ambiguidade e falta de papel têm resultados explícitos ([orfa_classe.py:49](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/orfa_classe.py:49)). Colisão com outros avais vigentes **[não verificado]**.
3. A troca do caso de dia sem trabalho por um colaborador real não pôde ser comprovada com o dado disponível **[não verificado]**.
4. O diff contém o juiz e seus testes; efeitos em dinheiro e certificação pela suíte **[não verificado]**. [nao verificado]

Arquivos alterados:

- [orfa_classe.py](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/orfa_classe.py:23): juiz puro, com fuso e pausas avulsas fornecidos pelo chamador. A junção usa `data_turno`; a saída preserva cada ocorrência da sobra.
- [test_razao_da_sobra.py](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_razao_da_sobra.py:29): casos pela regra, marca, meia-noite, cobertura, idempotência e precedência.
- `escala/tests/test_orfa_classe.py`: removido junto com `classificar_orfas`, pois seus casos sustentavam somente o classificador antigo. Permanecem `_runs_de_rajada` e `JANELA_CLUSTER_MIN`; a constante tem consumidores existentes ([utils.py:504](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/utils.py:504), [turnos.py:1581](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/turnos.py:1581)).

| Caso fornecido ou fixture | Razão pela regra | Teste | [nao verificado]
|---|---|---|
| 15/11 11:25 e 16:59, dia sem trabalho; colaborador real **[não verificado]** | `TRABALHO_EM_DIA_SEM_TRABALHO` | [linha 31](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_razao_da_sobra.py:31) |
| col836, 06/11 08:25 e 08:38 | `BATIDA_REPETIDA` | [linha 37](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_razao_da_sobra.py:37) |
| col203, 21/10 12:35; col204, 17/11 14:44 | `INTERVALO_FORA_DA_ESCALA` | [linha 44](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_razao_da_sobra.py:44) |
| col174, 15/11 05:30 e 06:33 | `PERIODO_ADICIONAL` | [linha 51](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_razao_da_sobra.py:51) |
| col174, 11/11 18:59; col220, 11/11 06:00 | `TURNO_ABERTO` | [linha 57](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_razao_da_sobra.py:57) |
| col174, 12/11 07:01; col300, 21/10 06:00 | `SAIDA_SEM_ENTRADA` | [linha 65](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_razao_da_sobra.py:65) |

As identidades e respostas nominais da tabela vêm do texto fornecido; os cadastros e batidas reais permanecem **[não verificado]**. O caso de meia-noite foi exercitado com a batida civil em 24/10 atribuída ao turno de 23/10 ([teste:76](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_razao_da_sobra.py:76)). [nao verificado]

O RED anterior à implementação falhou por ausência de `razao_da_sobra`. Depois, 12 testes passaram num executor isolado com substitutos em memória para `SimpleTestCase` e `Turno`; os seis mutantes abaixo foram mortos nesse mesmo executor. A execução Django real e a suíte certificadora ficam **[não verificado]**. Com Django disponível, o comando é: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python manage.py test escala.tests.test_razao_da_sobra --noinput
```

Execute cada `sed` isoladamente, rode o comando de teste acima e restaure a linha antes do próximo mutante: [nao verificado]

| Mutante — comando `sed` | Caso que deve falhar |
|---|---|
| `sed -i "s/razao = 'TRABALHO_EM_DIA_SEM_TRABALHO'/razao = 'MUTANTE'/" escala/orfa_classe.py` | `test_dia_sem_trabalho_duas_sobras` |
| `sed -i "s/razao = 'BATIDA_REPETIDA'/razao = 'MUTANTE'/" escala/orfa_classe.py` | `test_rajada_col836` |
| `sed -i "s/razao = 'INTERVALO_FORA_DA_ESCALA'/razao = 'MUTANTE'/" escala/orfa_classe.py` | `test_pausas_nominais` |
| `sed -i "s/razao = 'PERIODO_ADICIONAL'/razao = 'MUTANTE'/" escala/orfa_classe.py` | `test_periodos_col174` |
| `sed -i "s/razao = 'TURNO_ABERTO'/razao = 'MUTANTE'/" escala/orfa_classe.py` | `test_turnos_abertos` |
| `sed -i "s/razao = 'SAIDA_SEM_ENTRADA'/razao = 'MUTANTE'/" escala/orfa_classe.py` | `test_saidas_sem_entrada` |

Na investigação executei uma vez `python3` sem `PYTHONDONTWRITEBYTECODE=1`, contrariando a instrução; se isso criou bytecode está **[não verificado]**. As execuções dos testes e mutantes usaram a variável. [nao verificado]

## 19/11 11:53 · sessao 27d4574d0081 · DST-L2-JZ1-C7 · censo · origem=Code

**A etapa A fica parcialmente verificada.** O censo abaixo identifica as chamadas diretas encontradas e os caminhos indiretos relevantes; não estabelece para todos os caminhos indiretos se uma transação externa já mantém a trava. Por isso, a contagem final de perguntas de lei também fica **[não verificado]**.

**A1 — Chamadores diretos em produção**

| Função chamada | Chamador | Trava antes da chamada |
|---|---|---|
| `retratar_com_motivo` | `_retratar`, [reconciliador.py:525](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:525) | Depende do caminho que chamou `_retratar`: **[não verificado]** para todos. A própria unidade só trava depois da primeira leitura de retenção, em [reconciliador.py:544](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:544) e [reconciliador.py:558](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:558). |
| `retratar_com_motivo` | `reavaliar_chamados_geofence`, [geofence.py:444](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/geofence.py:444) | Não há trava anterior na função; trava herdada de transação externa **[não verificado]**. [geofence.py:409](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/geofence.py:409) |
| `retratar_com_motivo` | comando `reconciliar_fantasmas`, [reconciliar_fantasmas.py:281](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/management/commands/reconciliar_fantasmas.py:281) | Trava anterior ao chamado **[não verificado]**. |
| `retencao_da_retratacao` | `encerrar_com_retencao`, [reconciliador.py:509](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:509) | Depende do chamador. O fecho do bloqueio de entrada trava antes em [justificativa.py:80](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/justificativa.py:80); a cobertura de todos os caminhos da cascata é **[não verificado]**. |
| `retencao_da_retratacao` | `retratar_com_motivo`, [reconciliador.py:544](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:544) | Antes da trava da própria unidade, [reconciliador.py:558](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:558); trava externa depende da entrada **[não verificado]**. |
| `retencao_da_retratacao` | vigia `reconciliar_chamados`, [reconciliar_chamados.py:79](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/management/commands/reconciliar_chamados.py:79) | Trava anterior **[não verificado]**. |
| `retencao_da_retratacao` | `rejeitar_justificativas_dos_chamados`, [justificativa.py:346](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/justificativa.py:346) | A função trava justificativas antes, em [justificativa.py:337](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/justificativa.py:337); ordem global herdada para cada chamador **[não verificado]**. |
| `registrar_retencao` | `encerrar_com_retencao`, [reconciliador.py:511](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:511) | Mesma condição da chamada em `:509`: depende do caminho. |
| `registrar_retencao` | retenção inicial de `retratar_com_motivo`, [reconciliador.py:546](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:546) | Antes da trava própria em [reconciliador.py:558](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:558); trava externa **[não verificado]**. |
| `registrar_retencao` | recusa da cerca em `retratar_com_motivo`, [reconciliador.py:562](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:562) | Sim, quando há `colaborador_id`: [reconciliador.py:556](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:556). |
| `registrar_retencao` | ramo HIST, [reconciliador.py:1707](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:1707) | Trava ainda vigente ao registrar **[não verificado]**. |
| `registrar_retencao` | `fechar_beco`, [becos.py:70](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/becos.py:70) | Sim: [becos.py:52](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/becos.py:52). |
| `registrar_retencao` | resolver de perguntas, [disputa_emissao.py:1923](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/disputa_emissao.py:1923) | Sim, nesse ramo: [disputa_emissao.py:1913](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/disputa_emissao.py:1913). |

Nas aprovações, há evidência positiva de trava externa: a porta individual chama `travar_origem_antes` em [justificativa.py:140](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/justificativa.py:140), o lote em [ponto/views.py:1795](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/views.py:1795), e a função toma `trava_na_ordem` em [justificativa.py:129](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/justificativa.py:129). A porta da origem também trava explicitamente antes de delegar a `retratar`, em [justificativa.py:41](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/justificativa.py:41).

Os chamadores indiretos de `retratar` incluem os laços em [reconciliador.py:1051](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:1051) e [reconciliador.py:1512](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:1512), além de [retratar_chamados_lei_cala.py:59](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/management/commands/retratar_chamados_lei_cala.py:59), [processar_alertas_turno.py:216](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/management/commands/processar_alertas_turno.py:216), [reconciliar_grade.py:309](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/management/commands/reconciliar_grade.py:309) e [reconciliar_vinculo.py:68](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/management/commands/reconciliar_vinculo.py:68). A trava herdada em cada caminho e em todas as reentradas é **[não verificado]**.

**Testes, separados:** chamadas diretas encontradas de `retencao_da_retratacao` estão em [test_e4c_b2_cerca_retratar.py:409](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/tests/test_e4c_b2_cerca_retratar.py:409), [test_dst_l2_jz1_c6_retencao_origem.py:65](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:65), [linha 68](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:68), [linha 69](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:69), [linha 81](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/tests/test_dst_l2_jz1_c6_retencao_origem.py:81), e [test_dst_l2_jz1_c3_lote_portao.py:261](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:261) e [linha 264](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/tests/test_dst_l2_jz1_c3_lote_portao.py:264). Chamadas dinâmicas ou por aliases em testes **[não verificado]**.

**A2 — Leitura da instância depois do retorno**

- Geofence lê `ch.pk` e `ch.colaborador_id` depois da chamada e devolve a lista `perdeu` contendo as mesmas instâncias; o comando consumidor imprime dados dos chamados. Esses campos explícitos não são o ponteiro, mas uma pessoa que receba a instância da lista poderá observar o ponteiro atualizado. [geofence.py:437](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/geofence.py:437), [geofence.py:447](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/geofence.py:447), [geofence.py:454](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/geofence.py:454), [reconciliar_geofence.py:36](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/management/commands/reconciliar_geofence.py:36).
- `reconciliar_fantasmas` passa a mesma instância `c` ao resolver no fallback e lê `pk` e `colaborador_id` para registro. O efeito do ponteiro atualizado dentro de todos os caminhos do resolver é **[não verificado]**. [reconciliar_fantasmas.py:288](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/management/commands/reconciliar_fantasmas.py:288), [reconciliar_fantasmas.py:305](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/management/commands/reconciliar_fantasmas.py:305).
- Os laços do reconciliador leem `ch.pk` em tratamento de exceção, e `reconciliar_vinculo` lê `pk` e `colaborador_id` depois do sucesso. [reconciliador.py:1059](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:1059), [reconciliador.py:1515](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:1515), [reconciliar_vinculo.py:73](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/management/commands/reconciliar_vinculo.py:73).

A diferença demonstrável é que `pendentes_do_vinculo` usa o ID presente na instância; atualizar esse campo antes da nova consulta pode incluir uma justificativa apontada que a instância velha não alcançava e mudar a retenção ou o detalhe escolhido. [justificativa.py:279](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/justificativa.py:279), [reconciliador.py:453](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:453). Censo completo de leitores transitivos após o retorno **[não verificado]**.

**A3 — Transcrição da sonda fornecida**

A saída fornecida informa `achadas=1` para cada FK e `ERROS=0`; como não há arquivo da sonda acessível nesta análise, a atribuição é ao dado fornecido **[não verificado]**.

| FK | `deferrable` | `initially_deferred` | `on_delete` |
|---|---:|---:|---|
| `ChamadoColaborador.justificativa_relacionada` | `True` | `True` | `a` |
| `Justificativa.chamado_origem` | `True` | `True` | `a` |

Os nomes informados são `chamados_chamadocola_justificativa_relaci_fe18e9bb_fk_ponto_jus` e `ponto_justificativa_chamado_origem_id_5f6170cc_fk_chamados_` **[não verificado]**.

**A4 — Perguntas de lei: N = [não verificado].** Não há base suficiente nesta leitura para declarar zero: faltou concluir o censo transitivo e a conferência integral de `LEIS.md` e `CORTES.md` para B1–B3. A lei escrita exige um escritor canônico e trilha para ação crítica em [LEIS.md:65](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/LEIS.md:65); a implementação de retenção consulta a última linha e evita repetição sequencial em [reconciliador.py:495](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:495). Se a lei exige garantia mais forte para registros concorrentes ou falha da trilha neste caso específico, permanece **[não verificado]**.

Nenhum arquivo foi criado ou alterado **[não verificado]**.

## 19/11 11:53 · sessao b4dccd72f2e5 · DST-L2-JZ1-C7 · verificador · origem=Code

**AVAL-DEVOLVIDO — 2 pontos procedem:** o endereço exato da leitura do ponteiro é `:280`, e a frase “J3 morre em 0 de 12” ainda não está demonstrada como resultado medido; o anexo a apresenta como análise condicional, com catálogo e isolamento de produção não verificados. [justificativa.py:279–280](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/justificativa.py:279); matriz fornecida, seção “Matriz de intercalação” [não verificado em banco].

1. **PROCEDE parcialmente — premissas da FONTE.** A leitura inicial está em `:544`, antes do `atomic` em `:549`, da chamada de trava em `:558` e do ato cercado em `:559`. A chamada de trava é condicional a `ch.colaborador_id`; o modelo declara o colaborador obrigatório. [reconciliador.py:544](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:544); [models.py:44](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:44).

   `trava_na_ordem` consulta e devolve IDs, sem atualizar a instância `ch`. A correção de linha é: `:279` monta o predicado por **origem**; `:280` lê o **ponteiro da instância**. [models.py:2873](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:2873); [justificativa.py:279](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/justificativa.py:279).

   Quanto aos números 3/12 e 0/12, a matriz fornecida os condiciona às intercalações e pressupostos descritos nela; a ocorrência em produção e a garantia geral de 0/12 são **[não verificado]**.

2. **NÃO PROCEDE — colisão identificada de B1 com lei ou aval.** A proposta chama a autoridade de retenção e o escritor de trilha existentes; não acrescenta escritor de estado nem juiz. A rejeição continua na porta existente. [reconciliador.py:439](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:439); [reconciliador.py:465](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:465); [justificativa.py:292](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/justificativa.py:292); [LEIS.md:65](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/docs/LEIS.md:65).

   B1 coloca a consulta **depois** da ordem existente e não pede alteração de `justificativa.py` ou `vinculos.py`; a consulta de candidatas usada pela retenção não pede trava por padrão. [justificativa.py:262](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/justificativa.py:262); [models.py:2853](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:2853). A ausência de colisão com algum aval não disponível nesta cópia ou nos anexos é **[não verificado]**.

3. **PROCEDE — há condições ainda não demonstradas; não identifiquei impossibilidade literal de B1 ou B2.**

   - `retencao_da_retratacao(ch)` consulta o banco para candidatas, mas usa o ID do ponteiro **da instância recebida** para construir a consulta e escolher o detalhe. Chamar novamente com `ch` velha não basta para o caso só pelo ponteiro. [reconciliador.py:453](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:453); [justificativa.py:280](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/ponto/services/justificativa.py:280).
   - Recarregar somente o campo é possível sem editar `justificativa.py`: a própria cópia usa `refresh_from_db(fields=[...])`. Isso altera o ponteiro na instância, preservando os demais campos; o efeito nos leitores após o retorno exige o censo A2. [reconciliador.py:657](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:657); A2 **[não verificado]**.
   - Em chamadas **sequenciais**, com a mesma retenção e a primeira trilha gravada com sucesso, `registrar_retencao` compara a última linha e não grava outra. Não é garantia geral de concorrência: a consulta e a gravação são operações separadas, e o escritor do evento é *best effort*. [reconciliador.py:489](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:489); [core/observ.py:94](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/core/observ.py:94).
   - Fora de `_APROVANDO`, a retenção por justificativa alheia não é consultada; o escritor de rejeição segue no ato. Isso é compatível com B2(iii). [reconciliador.py:453](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:453); [reconciliador.py:580](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:580).
   - A saída antecipada autorizada por B1 pode retornar antes da trava; portanto, a afirmação de que **toda** decisão ocorrerá dentro dela não seria literal. [reconciliador.py:544](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:544); anexo do aval, B1 [não verificado em arquivo].
   - A3 exige catálogo efetivo, indisponível no dado fornecido: existência efetiva e `DEFERRABLE / INITIALLY DEFERRED` são **[não verificado]**. O RED, os três mutantes, o verde e o PRONTO também são **[não verificado]**. Um teste da janela não basta, por si só, para converter a matriz condicional de 12 células em medição de produção. [models.py:75](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/models.py:75); matriz fornecida, seção “FKs e releitura proposta” [não verificado em banco].

4. **NÃO PROCEDE — toque fora do declarado no pedido de B1.** O pedido descrito acrescenta leitura do ponteiro, consulta de retenção e eventual trilha; não pede escrita em competência exportada, dinheiro ou dados de escala/vínculo. A cerca existente continua envolvendo o ato de retratação. [reconciliador.py:549](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/reconciliador.py:549); [fio.py:1431](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/app/chamados/services/fio.py:1431); [CLAUDE.md:265](/home/ronald/codex_ws/ff5c27a90996d1b7e2b4a9c18c8829e0e6d797ba/CLAUDE.md:265).

   **Diff efetivo e efeitos indiretos futuros: [não verificado]**; não há diff construído para conferir. O hash Git da cópia também é **[não verificado]**, pois ela não contém metadados Git acessíveis.

## 19/11 11:48 · sessao 506dc2b576d0 · E4C-F5-CONSTROI · verificador · origem=Code

1. **NÃO BLOQUEIA — o bloqueio específico do `strip()` está fechado.** Os logs fornecidos mostram o mesmo mutante aplicado nas duas árvores: na antiga, os três testes passam; na nova, o teste novo falha com `(0, 1) != (1, 1)`. A falha ocorre na assertiva que exige contar a diferença entre `ato` e `ato `, não na montagem da fixture. O teste também exige contar um espaço no início, embora a execução mutante pare na primeira assertiva. [test_e4c_f5_contador_texto.py:21](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/tests/test_e4c_f5_contador_texto.py:21), [test_e4c_f5_contador_texto.py:40](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/tests/test_e4c_f5_contador_texto.py:40). Os logs foram fornecidos no enunciado, sem arquivo com linhas acessível na cópia; sua execução independente e a identidade Git da cópia são **[não verificado]**.

2. **NÃO BLOQUEIA — há mordida para espaços nas pontas, não prova de toda comparação literal.** O teste chama o contador vivo e exige uma divergência para cada projeção `ato ` e ` ato` diante do ato `ato`. O contador atual compara diretamente a leitura com a projeção, após converter projeção falsa para `''`. [test_e4c_f5_contador_texto.py:40](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/tests/test_e4c_f5_contador_texto.py:40), [vigia_texto_resposta.py:16](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/services/vigia_texto_resposta.py:16).

   Um mutante que compare ambos os textos com `casefold()` é plausível e, pela leitura dos casos, **deve sobreviver**: eles não confrontam textos que diferem apenas por maiúsculas e minúsculas. Sua sobrevivência executada é **[não verificado]**. Já ignorar `None` versus `''` não apresenta, no caminho normal atual, a mesma lacuna plausível: a projeção é normalizada pelo contador e a leitura inicializa ou normaliza seu texto como string vazia. [test_e4c_f5_contador_texto.py:26](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/tests/test_e4c_f5_contador_texto.py:26), [models.py:4310](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/models.py:4310), [models.py:4315](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/models.py:4315).

3. **NÃO BLOQUEIA — a causa identificável do `TypeError` foi corrigida, mas o fio inteiro não está demonstrado.** Antes, o patch devolvia inteiros; o desenlace testa pertinência de IDs nos dois valores. Conjuntos vazios suportam essa operação e correspondem ao formato de conjuntos de IDs esperado. Assim, o `TypeError` causado por `id in 0` deixa de ocorrer; a ausência de qualquer outra exceção no bloco, em execução, é **[não verificado]**. [test_e4c_f5_volta1_leem_o_ato.py:143](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:143), [fio.py:441](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/services/fio.py:441), [fio.py:454](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/services/fio.py:454), [raiox.py:51](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/services/raiox.py:51).

   O teste continua com dependências substituídas, inclusive o crédito real, e o bloco captura exceções; ele não verifica explicitamente o resultado do desenlace. Mede igualdade de consultas entre uma e várias perguntas, exigindo chamadas à leitura, nessas condições. A correção permite avançar pelo bloco antes interrompido e pode alterar o número medido; os números anterior e novo são **[não verificado]**. Os logs desta volta fornecidos executam apenas o módulo do contador. [test_e4c_f5_volta1_leem_o_ato.py:88](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:88), [test_e4c_f5_volta1_leem_o_ato.py:139](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:139), [fio.py:459](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/services/fio.py:459).

4. **NÃO BLOQUEIA — no delta fornecido, aparece a correção da falta de mordida do RED para `strip()`; as demais classes listadas não aparecem como alterações.** O delta acrescenta um teste e altera o retorno de um patch de teste; não apresenta mudança funcional de juiz, escritor, relógio, derivação, trava ou significado de campo. Isso é uma conclusão sobre o **diff fornecido**, cuja correspondência integral com as árvores é **[não verificado]**. [test_e4c_f5_contador_texto.py:40](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/tests/test_e4c_f5_contador_texto.py:40), [test_e4c_f5_volta1_leem_o_ato.py:143](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:143).

## 19/11 11:28 · sessao 2bf3a546f9a7 · DIA-CONVOCADO-PELO-ADMIN · leitura · origem=Code

**PERGUNTA DE DESENHO DIA-CONVOCADO — J [não verificado].** No caso focal, a célula diz trabalho previsto, mas a palavra do espelho pode ser “Feriado” e o emissor pode vetar a cobrança pela faixa de feriado. O número de leitores que contradizem a célula permanece **[não verificado]**: o censo e a sonda abaixo não estão fechados. ([CLAUDE.md:148](app/../CLAUDE.md:148), [dia_decidido.py:405](app/ponto/services/dia_decidido.py:405), [detectar_ausencias.py:599](app/ponto/management/commands/detectar_ausencias.py:599))

**Entrega incompleta.** Fechei a leitura principal do caso focal e entrego um script AST para conferência, mas não completei a sonda executável nem provei a inexistência de todas as telas e atos em lote. Resultados de produção e da sombra: **[não verificado]**.

## A. Script do censo AST

O script abaixo é texto para ser executado **na raiz `app/`**. Conta sítios sintáticos de leitura de atributos, `getattr` literal, campos literais ou keywords dos métodos ORM pedidos e usos de nomes importados com apelido; separa `tests` e `migrations`. Uma linha corresponde a uma ocorrência sintática, não necessariamente a um leitor funcional distinto. A resolução de apelidos é estática e por arquivo: sombreamento de nomes, nomes construídos dinamicamente, chamadas indiretas e outros métodos ORM exigem revisão manual **[não verificado]**.

```python
import ast
from collections import Counter
from pathlib import Path

ALVOS = {"trabalha_em_feriado", "feriado_vira_folga"}
ORM = {"filter", "exclude", "values", "values_list", "update"}


def campo(s):
    if not isinstance(s, str):
        return None
    partes = s.split("__")
    return next((p for p in partes if p in ALVOS), None)


class Censo(ast.NodeVisitor):
    def __init__(self, caminho):
        self.caminho = caminho
        self.pilha = []
        self.aliases = {}
        self.sitios = []

    def emitir(self, no, forma):
        funcao = ".".join(self.pilha) or "<modulo>"
        self.sitios.append((self.caminho, no.lineno, funcao, forma))

    def visit_ClassDef(self, no):
        self.pilha.append(no.name)
        self.generic_visit(no)
        self.pilha.pop()

    def visit_FunctionDef(self, no):
        self.pilha.append(no.name)
        self.generic_visit(no)
        self.pilha.pop()

    visit_AsyncFunctionDef = visit_FunctionDef

    def visit_ImportFrom(self, no):
        for a in no.names:
            if a.name in ALVOS:
                nome_local = a.asname or a.name
                self.aliases[nome_local] = a.name
                self.emitir(no, f"import:{a.name}:as:{nome_local}")

    def visit_Name(self, no):
        if isinstance(no.ctx, ast.Load) and no.id in self.aliases:
            self.emitir(no, f"nome-importado:{self.aliases[no.id]}:{no.id}")

    def visit_Attribute(self, no):
        if no.attr in ALVOS and isinstance(no.ctx, ast.Load):
            self.emitir(no, f"atributo:{no.attr}")
        self.generic_visit(no)

    def visit_Call(self, no):
        if (
            isinstance(no.func, ast.Name)
            and no.func.id == "getattr"
            and len(no.args) >= 2
            and isinstance(no.args[1], ast.Constant)
            and no.args[1].value in ALVOS
        ):
            self.emitir(no, f"getattr:{no.args[1].value}")

        if isinstance(no.func, ast.Attribute) and no.func.attr in ORM:
            metodo = no.func.attr
            for arg in no.args:
                if isinstance(arg, ast.Constant):
                    alvo = campo(arg.value)
                    if alvo:
                        self.emitir(arg, f"orm:{metodo}:string:{arg.value}")
            for kw in no.keywords:
                alvo = campo(kw.arg)
                if alvo:
                    self.emitir(kw.value, f"orm:{metodo}:keyword:{kw.arg}")
        self.generic_visit(no)


totais = Counter()
arquivos = Counter()
erros = 0

for caminho in sorted(Path(".").rglob("*.py")):
    partes = caminho.parts
    grupo = (
        "migrations" if "migrations" in partes
        else "tests" if "tests" in partes or caminho.name.startswith("test_")
        else "app"
    )
    try:
        arvore = ast.parse(caminho.read_bytes(), filename=str(caminho))
    except (OSError, SyntaxError, UnicodeError) as exc:
        erros += 1
        print(f"{caminho}:0 <modulo> ERRO:{type(exc).__name__}")
        continue

    visita = Censo(caminho.as_posix())
    visita.visit(arvore)
    for arquivo, linha, funcao, forma in sorted(visita.sitios):
        print(f"{arquivo}:{linha} {funcao} {forma} [{grupo}]")
        totais[grupo] += 1
    if visita.sitios:
        arquivos[grupo] += 1

for grupo in ("app", "tests", "migrations"):
    print(f"TOTAL {grupo} sitios={totais[grupo]} arquivos={arquivos[grupo]}")
print(f"ERROS={erros}")
```

**Limitação relevante:** o script não resolve `Q(**dicionario)` nem campos obtidos por variáveis, e não identifica usos de um nome importado quando há reatribuição ou sombreamento com precisão de escopo. Por isso, seus totais ainda não provam “todo leitor” nem classificam cada sítio como decide/exibe/grava. **Censo fechado e totais: [não verificado].**

## B. Linha focal

Hipóteses: ciclo weekday, chave `False`, célula editada `trabalha=True`, feriado reconhecido e sem ausência. A sessão já confirmou o resultado da precedência; não o refaço. **Valor concreto de col945: [não verificado].**

| Autoridade | Resposta pelo código | Célula ou chave; contradição |
|---|---|---|
| **Espelho — palavra do dia** | Com os fatos focais e sem decisão ou pedido de ausência que substitua a palavra, `veredito_do_dia` devolve `FERIADO`; `do_dia` fornece a palavra e `aplicar_palavra_do_dia` a põe na linha. O espelho completo também possui um `status_rotulo` separado, que pode vir de lavra ou do relógio; seu valor concreto é **[não verificado]**. ([dia_decidido.py:397](app/ponto/services/dia_decidido.py:397), [dia_decidido.py:481](app/ponto/services/dia_decidido.py:481), [espelho.py:329](app/ponto/services/espelho.py:329), [status_dia.py:270](app/ponto/status_dia.py:270)) | A palavra recebe fatos que combinam célula e predicado do vínculo; **sim**, diz “Feriado” no sentido de contradição definido no pedido. A resposta real da tela depende também dos demais insumos. ([espelho.py:332](app/ponto/services/espelho.py:332), [precedencia.py:347](app/ponto/precedencia.py:347)) |
| **Motor — previsto** | No caminho do fechamento, o cursor inclui a data quando `esc.eh_dia_trabalho` é verdadeiro e ela não está suprimida. Para a célula focal, sem supressão, isso aponta para data prevista; a alimentação concreta e eventual supressão de col945 são **[não verificado]**. ([fechamento.py:724](app/ponto/services/fechamento.py:724), [CLAUDE.md:155](app/../CLAUDE.md:155)) | Lê a célula por `eh_dia_trabalho`; **não**, quanto ao previsto, nas hipóteses indicadas. ([escala/models.py:1069](app/escala/models.py:1069)) |
| **Motor — dobra** | `_feriado_dobra_no_dia` devolve `feriado_em_dobra and eh_feriado(dia)`, sem perguntar se a data é prevista. O padrão do ciclo exclui `12x36` da dobra; uma régua pode fornecer o valor explicitamente. Com duração positiva e dobra ativa, o trecho citado atribui a duração a `minutos_extra_100_feriado`. Sem batida, valor concreto e rubrica apurada de col945 são **[não verificado]**. ([motor_calculo_v2.py:914](app/ponto/motor_calculo_v2.py:914), [motor_calculo_v2.py:2809](app/ponto/motor_calculo_v2.py:2809), [motor_calculo_v2.py:3623](app/ponto/motor_calculo_v2.py:3623), [motor_calculo_v2.py:3694](app/ponto/motor_calculo_v2.py:3694)) | O predicado da dobra lê configuração do motor e feriado, não a célula; **não sei** se há contradição com a célula: ele responde pagamento, e a L-086 vigente admite dobra de feriado previsto conforme o ciclo. ([LEIS.md:118](app/docs/LEIS.md:118)) |
| **Fechamento** | Só examina a partição depois de obter `resultado.periodos_ft` e filtrar escala certa. Para cada período remanescente, se o vínculo do dia diz `feriado_vira_folga`, promove as horas para HE 100 de feriado e as retira de `horas_folga_trabalhada`. Se o período focal entra em `_ft_certa`: **[não verificado]**; sem período trabalhado, não há horas desse período a promover. ([fechamento.py:933](app/ponto/services/fechamento.py:933)) | Nesse teste lê o predicado do vínculo, não `CelulaDia.trabalha`; **não sei** se contradiz o caso focal, pois não foi demonstrado que um período focal chega a esse ramo. ([fechamento.py:944](app/ponto/services/fechamento.py:944)) |
| **DiaPago** | Para períodos normais, acumula as rubricas do período; para `ft_feriado`, acumula minutos em `horas_extras_100` e `horas_extras_100_feriado`. A partição é fornecida pelo fechamento; valores lavrados concretos são **[não verificado]**. ([dia_pago.py:175](app/ponto/services/dia_pago.py:175), [dia_pago.py:241](app/ponto/services/dia_pago.py:241)) | Nesse ramo lê listas e minutos já partilhados, não consulta diretamente célula ou chave; **não sei** se contradiz a célula, pois depende do período e da partição recebidos. ([dia_pago.py:234](app/ponto/services/dia_pago.py:234)) |

O texto da casa declara “A GRADE e fonte unica de previsto” e que `EC.eh_dia_trabalho` lê a célula; a L-020 chama a célula soberana, e a L-086 registra a revogação parcial da antiga cláusula sobre feriado previsto. Esses textos distinguem previsto de pagamento, mas não encontrei neles uma linha que resolva expressamente o veto de cobrança para **célula editada `True` em feriado com chave `False`**. ([CLAUDE.md:148](app/../CLAUDE.md:148), [LEIS.md:71](app/docs/LEIS.md:71), [LEIS.md:118](app/docs/LEIS.md:118))

## C. Sonda

**Não entrego a sonda como pronta:** ainda faltam a montagem e a verificação do contador de frota, do espelho e do tratamento de erros em conjunto. Portanto `ERROS`, `UNIVERSO` e `CONTADOS` são **[não verificado]**.

O modelo correto para descobrir o colaborador é `escala.models.EscalaColaborador`: a linha com `pk=1255` tem a FK `colaborador`, cujo identificador Django é `colaborador_id`; o valor efetivo do PK é **[não verificado]**. Para o vínculo que responde por um dia, o carregador real é `vinculo_do_dia_carregado(colaborador_id, data)`. ([escala/models.py:871](app/escala/models.py:871), [escala/alimentacao.py:289](app/escala/alimentacao.py:289))

O emissor encontrado é `Command._abrir_chamado(self, colab, ec, data_ref, dt_inicio, agora, marcos_faltantes=None)`. **Ele grava** por `ChamadoColaborador.abrir`; também pode chamar `push_supervisao` para ciclos não flexíveis. Assim, `atomic` com rollback desfaz escritas transacionais, mas não demonstra por si só reversão de efeito externo de push. No caso focal 6x1, o código classifica o ciclo como flexível e não chama esse push específico; outros efeitos externos síncronos permanecem **[não verificado]**. ([detectar_ausencias.py:571](app/ponto/management/commands/detectar_ausencias.py:571), [detectar_ausencias.py:586](app/ponto/management/commands/detectar_ausencias.py:586), [detectar_ausencias.py:694](app/ponto/management/commands/detectar_ausencias.py:694), [detectar_ausencias.py:775](app/ponto/management/commands/detectar_ausencias.py:775))

`editar_dia` pode devolver `editada`, `igual`, `barrada` ou `sem_vinculo`; um resultado diferente de `editada` precisa ser contado antes de interpretar o depois. O código agenda trabalho posterior ao commit; a extensão completa de efeitos externos síncronos do caminho é **[não verificado]**. ([celula.py:572](app/ponto/portas/celula.py:572), [celula.py:586](app/ponto/portas/celula.py:586), [celula.py:590](app/ponto/portas/celula.py:590))

## D. Tela

- **Ausência para um dia:** `lancar_ausencia` recebe colaborador, tipo e data; a duração padrão é um dia. O formulário correspondente seleciona um colaborador e uma data. ([views.py:2476](app/ponto/views.py:2476), [lancar_ausencia.html:12](app/templates/ponto/lancar_ausencia.html:12), [lancar_ausencia.html:37](app/templates/ponto/lancar_ausencia.html:37))
- **Folga:** há `folga_compensatoria` no catálogo de ausência e na lista de tipos de duração da tela; isso prova esse tipo de lançamento, mas não identifica conclusivamente toda tela de **folga avulsa**. ([ausencias.py:100](app/ponto/catalogo/ausencias.py:100), [lancar_ausencia.html:95](app/templates/ponto/lancar_ausencia.html:95))
- **Troca de plantão:** há a porta `trocar_plantao`, que exige as duas cartas; view e template específicos **[não verificado]**. ([celula.py:670](app/ponto/portas/celula.py:670))
- **Lote por posto × data:** **[não verificado]**. O formulário de ausência examinado é individual; a busca por `folga.avulsa|troca.de.plant[aã]o|trocar_plantao|folga_avulsa` não basta para provar inexistência de outro ato em lote. ([lancar_ausencia.html:14](app/templates/ponto/lancar_ausencia.html:14), [views.py:2483](app/ponto/views.py:2483))

**perguntas de lei: [não verificado]**

- Candidata: qual regra escrita decide a cobrança quando a célula foi editada para trabalho num feriado em que o vínculo folga? Procurei a regra de previsto em [CLAUDE.md:148](app/../CLAUDE.md:148), a soberania da célula em [LEIS.md:71](app/docs/LEIS.md:71), a foto em [LEIS.md:74](app/docs/LEIS.md:74), e a regra de pagamento em [LEIS.md:118](app/docs/LEIS.md:118); não localizei nesses trechos resposta expressa para esse conflito. A busca exaustiva em `CORTES.md` permanece **[não verificado]**.

## 19/11 11:25 · sessao 6f71d61fc4a3 · DIA-CONVOCADO-PELO-ADMIN · leitura · origem=Code

**PERGUNTA DE DESENHO DIA-CONVOCADO — J ainda não fechado.** Na hipótese de célula editada para `trabalha=True`, feriado reconhecido, vínculo weekday com chave `False` e sem ausência de precedência superior, `fatos_do_dia` registra o dia como previsto, mas mantém a faixa `feriado`; o emissor de ausência pode então devolver `VETO_FERIADO` antes de consultar o veto da célula. Há guardas anteriores ao veto de feriado, portanto o retorno efetivo do emissor para col945 permanece **[não verificado]**. ([precedencia.py:347](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/ponto/precedencia.py:347), [precedencia.py:376](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/ponto/precedencia.py:376), [detectar_ausencias.py:599](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/ponto/management/commands/detectar_ausencias.py:599), [detectar_ausencias.py:616](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/ponto/management/commands/detectar_ausencias.py:616))

**INCOMPLETO:** não fechei o censo AST, o script do censo, a sonda executável como texto, as colunas de espelho e DiaPago para os 18 estados, nem o levantamento completo das telas. Não há resultado de sonda para relatar. O diretório disponível não apresentou metadados Git para confirmar o HEAD solicitado; **HEAD [não verificado]**. Nenhum arquivo foi criado, alterado ou apagado; não executei banco nem suíte. **[não verificado por auditoria externa]** [nao verificado]

### Matriz parcial

A matriz abaixo considera feriado reconhecido pelo vínculo, célula existente com `trabalha` booleano, ausência de férias/afastamento/atestado e de outra folga lançada que altere a precedência. `G` é célula gerada; `E✓` e `E✗` são células editadas para `True` e `False`. Para `G` no weekday com chave `True` e no 12x36, o resultado depende de o ciclo prever trabalho naquela data. ([precedencia.py:336](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/ponto/precedencia.py:336), [precedencia.py:357](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/ponto/precedencia.py:357), [models.py:1365](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/escala/models.py:1365))

Nas células da coluna **Fatos**, `F/F/F` significa `(feriado, era_dia_previsto=False, feriado_so_no_pagamento=False)`; `F/T/F` significa `(feriado, True, False)`; `T/T/T` significa `(trabalho, True, True)`. O resultado não depende da batida nesse trecho. ([precedencia.py:376](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/ponto/precedencia.py:376), [precedencia.py:379](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/ponto/precedencia.py:379), [precedencia.py:402](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/ponto/precedencia.py:402))

| Ciclo e chave | Célula | Batida | Fatos | Emissor | Espelho | Motor / DiaPago |
|---|---|---|---|---|---|---|
| Weekday False | G | Sim | F/F/F | Não sei: guardas e contexto | Não sei | Não sei |
| Weekday False | G | Não | F/F/F | Veto por feriado, se passar guardas anteriores | Não sei | Não sei |
| Weekday False | E✓ | Sim | F/T/F | Não sei: guardas e contexto | Não sei | Não sei |
| Weekday False | E✓ | Não | F/T/F | Veto por feriado, se passar guardas anteriores | Não sei | Não sei |
| Weekday False | E✗ | Sim | F/F/F | Não sei: guardas e contexto | Não sei | Não sei |
| Weekday False | E✗ | Não | F/F/F | Veto por feriado, se passar guardas anteriores | Não sei | Não sei |
| Weekday True | G | Sim | T/T/T ou F/F/F, conforme célula | Não sei | Não sei | Não sei |
| Weekday True | G | Não | T/T/T ou F/F/F, conforme célula | Não sei | Não sei | Não sei |
| Weekday True | E✓ | Sim | T/T/T | Não sei | Não sei | Não sei |
| Weekday True | E✓ | Não | T/T/T | Não sei | Não sei | Não sei |
| Weekday True | E✗ | Sim | F/F/F | Não sei | Não sei | Não sei |
| Weekday True | E✗ | Não | F/F/F | Veto por feriado, se passar guardas anteriores | Não sei | Não sei |
| 12x36 | G | Sim | T/T/T ou F/F/F, conforme célula | Não sei | Não sei | Não sei |
| 12x36 | G | Não | T/T/T ou F/F/F, conforme célula | Não sei | Não sei | Não sei |
| 12x36 | E✓ | Sim | T/T/T | Não sei | Não sei | Não sei |
| 12x36 | E✓ | Não | T/T/T | Não sei | Não sei | Não sei |
| 12x36 | E✗ | Sim | F/F/F | Não sei | Não sei | Não sei |
| 12x36 | E✗ | Não | F/F/F | Veto por feriado, se passar guardas anteriores | Não sei | Não sei |

As células “não sei” não representam resultado negativo: faltou rastrear o caminho completo ou faltam dados como jornada, guardas, ata e lavra. O emissor examinado é `Command._abrir_chamado`; a faixa `feriado` produz o veto citado, mas existem verificações anteriores e posteriores. ([detectar_ausencias.py:571](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/ponto/management/commands/detectar_ausencias.py:571), [detectar_ausencias.py:599](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/ponto/management/commands/detectar_ausencias.py:599))

O motor configura 12x36 entre os ciclos de feriado simples e, no trecho de cálculo examinado, atribui minutos de HE 100 de feriado quando há duração positiva e `_feriado_dobra_no_dia` retorna verdadeiro. Isso não basta, por si só, para preencher todas as rubricas da matriz: o fechamento ainda separa períodos conforme escala certa e `feriado_vira_folga`, e `DiaPago` recebe essa partição. ([motor_calculo_v2.py:3623](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/ponto/motor_calculo_v2.py:3623), [motor_calculo_v2.py:2809](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/ponto/motor_calculo_v2.py:2809), [fechamento.py:933](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/ponto/services/fechamento.py:933), [dia_pago.py:241](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/ponto/services/dia_pago.py:241))

### Leitores e lei: achados parciais

| Sítio | Papel observado | Célula ou vínculo | Caso editado `True`, chave `False` |
|---|---|---|---|
| [models.py:1369](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/escala/models.py:1369), `eh_dia_trabalho_calculado` | Decide o cálculo gerador | Vínculo/ciclo; este trecho não lê a célula | Se a base for `True`, retorna `False` pelo feriado. |
| [models.py:1373](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/escala/models.py:1373), `feriado_vira_folga` | Decide o predicado | Chave, ciclo e lugar do vínculo; não lê a célula | Retorna `True`, pressuposto feriado reconhecido. |
| [precedencia.py:347](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/ponto/precedencia.py:347), `fatos_do_periodo` | Decide precedência | Lê célula **e** predicado do vínculo | `previsto=True`, mas faixa `feriado`, nas hipóteses da matriz. `fatos_do_dia` delega ao período ([precedencia.py:242](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/ponto/precedencia.py:242)). |
| [alimentacao.py:514](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/ponto/calculador/alimentacao.py:514) | Alimenta cálculo | Passa informação da célula e o predicado do vínculo | Passa `folga_e_feriado=True`, se o feriado estiver no conjunto alimentado. |
| [fechamento.py:944](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/ponto/services/fechamento.py:944) | Decide partição de rubrica | Consulta o vínculo do dia para períodos de folga trabalhada já selecionados | Classifica esses períodos como feriado quando o predicado é verdadeiro; alcance no caso concreto **[não verificado]**. |
| [models.py:920](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/escala/models.py:920), `vinculos_5x2_6x1_trabalha_feriado` | Decide universo de contador | Filtro pela chave do vínculo | O vínculo com chave `False` fica fora desse filtro. |
| [vinculo.py:890](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/colaboradores/services/vinculo.py:890), `marcar_trabalha_em_feriado` | Lê e grava a chave | Vínculo | Compara o valor atual ao desejado; não responde diretamente ao dia editado. |
| [sincronizar_feriado_admin.py:48](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/colaboradores/management/commands/sincronizar_feriado_admin.py:48) | Compara e exibe; pode chamar a porta de gravação | Vínculo | Depende do valor desejado pelo comando. |

Esta tabela **não é o censo fechado**: faltam a análise AST de apelidos e strings, todos os sítios de cópia/gravação, a contagem separada de testes e migrations e a classificação completa. Logo, **K e J = [não verificado]**. A divergência da precedência e o veto condicional do emissor estão demonstrados acima; não os transformei em uma contagem de leitores. [nao verificado]

A lei escrita localizada é: “**A GRADE e fonte unica de previsto**” e “**EC.eh_dia_trabalho e EC.marcos_do_dia LEEM a celula**”, com fallback aritmético/template. ([CLAUDE.md:148](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/CLAUDE.md:148), [CLAUDE.md:155](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/CLAUDE.md:155))

Para pagamento, a própria L-086 informa que sua cláusula original sobre feriado foi parcialmente revogada: feriado previsto dobra conforme `feriado_em_dobra` do ciclo, e 12x36 não dobra por essa regra. A L-023 trata da relação entre foto e ciclo, incluindo folga avulsa; seu texto não resolve sozinho a divergência observada na emissão. ([LEIS.md:118](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/docs/LEIS.md:118), [LEIS.md:74](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/docs/LEIS.md:74))

### Sonda: limites já identificados

Não entrego um script como se estivesse pronto: ainda falta conferir a obtenção segura do colaborador pelo apelido raspado, os argumentos de início exigidos pelo emissor, o formato das rubricas do espelho e a busca do segundo ramo. Tudo isso é **[não verificado]**.

Para quem completar a sonda, `editar_dia` pode devolver `barrada` ou `sem_vinculo`, além de `editada` e `igual`; a sonda precisa verificar esse retorno antes de interpretar o “depois”. ([celula.py:572](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/ponto/portas/celula.py:572), [celula.py:606](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/ponto/portas/celula.py:606))

Com `raise` saindo do `atomic` externo, as alterações transacionais feitas dentro dele, inclusive célula e trilha, são revertidas; callbacks `on_commit` não executam. Em particular, não rodam a rejunção e a reavaliação da falta espúria agendadas no rabo, nem o recálculo por evento agendado para o commit. O julgamento síncrono anterior pode ser chamado dentro da transação; seus efeitos transacionais também são revertidos. Efeitos externos eventualmente disparados por caminhos síncronos: **[não verificado]**. ([celula.py:460](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/ponto/portas/celula.py:460), [celula.py:528](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/ponto/portas/celula.py:528), [fechamento.py:1798](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/ponto/services/fechamento.py:1798))

### Tela: achado parcial

A tela de lançamento de ausência usa a view [lancar_ausencia](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/ponto/views.py:2476) e o [template correspondente](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/templates/ponto/lancar_ausencia.html:12). A view recebe um `colaborador`, uma data e duração cujo padrão é um dia; essa constatação não fecha o levantamento de outras telas ou de lote por posto e data. ([views.py:2483](/home/ronald/codex_ws/c5ea4e5d75dd4b2e50304d3f765810b9acc70a97/app/ponto/views.py:2483))

View e template específicos de folga avulsa e troca de plantão, e existência de ato em lote por posto × data: **[não verificado]**.

perguntas de lei: [não verificado]

## 19/11 11:21 · sessao 3978636e78d4 · E4C-F5-CONSTROI · verificador · origem=Code

**Resultado: BLOQUEIA nos itens 4 e 6**, pela lacuna de prova do contador; os demais itens não bloqueiam no escopo examinado, com a ressalva interpretativa do item 3. A cópia disponibilizada não é um repositório Git: a identidade do HEAD, a base e a correspondência integral do patch com a árvore permanecem **[não verificado]**.

1. **NÃO BLOQUEIA — lei B nos sítios alterados.** No fio, a leitura fornece o instante e o texto é substituído pelo corrente da **mesma pergunta**. Sem instante, ou com instante e texto corrente vazio após trim, o leitor considera que ainda espera texto. [app/chamados/services/fio.py:178](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/services/fio.py:178), [app/chamados/juizes.py:1526](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/juizes.py:1526)

   No histórico, resposta e validação usam os instantes da leitura; materialização continua usando o instante projetado. Os três rótulos usam o texto corrente da mesma pergunta, limitado a 14 caracteres ou apresentado como “sem resposta”; evento sem instante não sai. Texto e instante podem, portanto, corresponder a **momentos diferentes**, mas não identifiquei mistura entre perguntas nesses sítios. Essa separação é a opção B segundo `respostas.txt` fornecido, cujo original com linha não está disponível **[não verificado]**. [app/chamados/services/historico.py:96](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/services/historico.py:96)

   Fora dos sítios alterados, a API ainda expõe `respondida_em` da projeção no JSON; o botão de correção, porém, consulta o leitor alterado. A extensão da migração a esse campo da API não está demonstrada pelo patch fornecido **[não verificado]**. [app/api/views.py:1163](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/api/views.py:1163), [app/api/views.py:1174](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/api/views.py:1174)

2. **NÃO BLOQUEIA — mapa dos valores no escopo examinado; não têm todos o mesmo significado.**

   - `respondida_em` e `resposta_colab`: a resposta humana grava ambos e registra o ato; resposta automática também os grava, enquanto a cura pela trilha carimba texto já existente. A resposta humana limpa validação anterior; as outras duas preservam-na. O fio e o histórico leem instante pela leitura e texto da projeção. [app/chamados/services/disputa_emissao.py:1474](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/services/disputa_emissao.py:1474), [app/chamados/services/disputa_emissao.py:1662](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/services/disputa_emissao.py:1662), [app/chamados/services/disputa_emissao.py:1692](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/services/disputa_emissao.py:1692)
   - `validada_em`: a porta de validação grava projeção e ato com o mesmo instante; devolução pode limpar a projeção e registrar devolução. O modelo também pode preencher resposta e validação ao carimbar. O histórico lê a validação vigente da leitura. [app/chamados/services/validacao.py:161](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/services/validacao.py:161), [app/chamados/services/validacao.py:262](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/services/validacao.py:262), [app/chamados/models.py:3299](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/models.py:3299)
   - `materializada_em`: a porta do modelo atribui o carimbo; o histórico lê esse campo diretamente, com significado de materialização, não de resposta ou validação. [app/chamados/models.py:3269](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/models.py:3269), [app/chamados/services/historico.py:104](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/services/historico.py:104)
   - `decisoes`/`leitura`: `registrar` grava as linhas; `leitura` ordena os atos, incorpora passivo da projeção sem linha correspondente e aplica seus efeitos. Logo, “leitura” não significa exclusivamente linhas físicas existentes. [app/chamados/models.py:4238](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/models.py:4238), [app/chamados/models.py:4295](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/models.py:4295), [app/chamados/models.py:4329](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/models.py:4329)
   - `divergencia_texto_resposta`: o contador compara o texto da leitura com o da projeção, por pergunta; o vigia grava o resultado em `censo` e zero em `esperado`; o leitor do ESTADO compara as duas entradas genericamente. O texto da leitura pode incluir passivo projetado, portanto “texto do ato” nesse contador tem esse limite. [app/chamados/services/vigia_texto_resposta.py:8](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/services/vigia_texto_resposta.py:8), [app/chamados/management/commands/vigiar_fio_morto.py:103](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/management/commands/vigiar_fio_morto.py:103), [bin/fio_estado.py:121](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/bin/fio_estado.py:121)

   A enumeração exaustiva de **todos** os escritores e leitores fora desse escopo, e a uniformidade de seus fusos e significados, permanecem **[não verificado]**.

3. **NÃO BLOQUEIA — descarte da triagem é sustentável, mas contestável.** O descarte é uma interpretação plausível: a lei diz que cron de varredura é vigia que só alarma, e a regra de parada atribui à sessão a colocação de perguntas de lei ou desenho no topo do RELATO. Nenhuma dessas linhas, isoladamente, determina literalmente que o código deva promover este contador ao topo. [CLAUDE.md:172](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/CLAUDE.md:172), [CLAUDE.md:269](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/CLAUDE.md:269)

   O comportamento concreto é mais estreito que “PERGUNTA no topo”: valor positivo aparece como **FORA DO ESPERADO** na seção Fio morto do ESTADO, abaixo de outras seções; a manchete dessa seção trata de alcance. O diff não cria uma pergunta automática no topo. Portanto, se o aval exige essa promoção **pela máquina**, não entrega o literal; a triagem só se sustenta lendo a promoção como dever da sessão. O texto do aval fornecido não está disponível em arquivo com linha **[não verificado]**. [bin/fio_estado.py:125](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/bin/fio_estado.py:125), [bin/fio_estado.py:191](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/bin/fio_estado.py:191), [bin/gerar_estado.py:312](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/bin/gerar_estado.py:312)

4. **BLOQUEIA — há uma lacuna de mordida identificável.** Segundo os logs e scripts transcritos, os oito mutantes alteraram os arquivos anunciados e os testes indicados morreram; a execução sobre o hash declarado e os logs originais não podem ser verificados independentemente **[não verificado]**. Os cinco de custo falham nas comparações de consultas correspondentes; `M_hist_cru` é pego pelo selo e pelo cenário de histórico; `M_censo_zero` falha na assertiva de censo positivo. As assertivas respectivas estão em [app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:104](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:104) e [app/chamados/tests/test_e4c_f5_contador_texto.py:41](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/tests/test_e4c_f5_contador_texto.py:41).

   O `KeyError` de `M_fora_do_esperado` é **mordida legítima**: a mutação anunciada retira exatamente a chave que o teste exige em `reg['esperado']`; a exceção ocorre nessa leitura, não na montagem da fixture. O texto exato da mutação e do log só está no anexo fornecido **[não verificado]**. [app/chamados/tests/test_e4c_f5_contador_texto.py:52](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/tests/test_e4c_f5_contador_texto.py:52)

   **Mutante plausível sem mordida demonstrada:** comparar os textos após `strip()` no contador deixaria de contar diferenças apenas de espaços, embora o código atual compare os valores diretamente. Os casos apresentados usam textos sem essa diferença; a sobrevivência é inferida da leitura dos testes, não de execução de mutante **[não verificado]**. Isso deixa sem prova a fidelidade literal da comparação de textos. [app/chamados/services/vigia_texto_resposta.py:16](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/services/vigia_texto_resposta.py:16), [app/chamados/tests/test_e4c_f5_contador_texto.py:26](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/tests/test_e4c_f5_contador_texto.py:26)

   Os logs transcritos também mostram `TypeError` capturado em `desenlace` durante testes de custo; não é a exceção que causa as falhas de contagem, mas limita o que esses testes provam sobre o funcionamento completo do fio. A origem exata da fixture defeituosa e seu efeito sobre outros comportamentos são **[não verificado]**. [app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:140](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:140)

5. **NÃO BLOQUEIA — não identifiquei N+1 de decisões nos chamadores reais examinados.** Cobrança, fio, histórico e contador carregam `decisoes` em lote; `classificar` faz prefetch de contingência e `coletar` já o traz para todas as disputas. A API, chamador adicional de `aguarda_o_colaborador`, recebe as mesmas instâncias de `consulta_do_app`, cuja consulta também faz prefetch. [app/chamados/services/cobranca.py:235](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/services/cobranca.py:235), [app/chamados/services/fio.py:262](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/services/fio.py:262), [app/chamados/services/historico.py:96](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/services/historico.py:96), [app/chamados/relatorios_disputa.py:11](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/relatorios_disputa.py:11), [app/chamados/services/pergunta_no_app.py:57](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/services/pergunta_no_app.py:57), [app/api/views.py:1033](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/api/views.py:1033)

   Essa conclusão é sobre as consultas de `decisoes` introduzidas por esses leitores; custo total e outros N+1 das telas ou da frota são **[não verificado]**. [app/chamados/models.py:4304](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/models.py:4304)

6. **BLOQUEIA — pela lacuna de RED/mutação do item 4.** Não identifiquei no patch apresentado segundo escritor dos campos da pergunta, nova ordem de trava ou relógio novo: a mudança funcional acrescenta leitores, prefetch e a chave do carimbo; o contador é um predicado de divergência, distinto do juiz registrado de pergunta respondida. A resposta humana examinada relê o relógio depois da trava. [app/chamados/services/vigia_texto_resposta.py:8](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/services/vigia_texto_resposta.py:8), [app/chamados/juizes.py:1544](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/juizes.py:1544), [app/chamados/services/disputa_emissao.py:1517](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/services/disputa_emissao.py:1517)

   Há duas representações com possíveis momentos diferentes, e a leitura incorpora passivo: isso deve ser mantido explícito, conforme itens 1 e 2. Uma auditoria completa de escritores externos, relógios e ordem de trava fora do diff permanece **[não verificado]**. [app/chamados/models.py:4295](/home/ronald/codex_ws/71905e5e6f40294c004f71e91d360c9e6f410312/app/chamados/models.py:4295)

## 19/11 11:12 · sessao a30249330466 · ESCALA-NAO-DESCREVE · leitura · origem=Code

**Resultado principal:** o núcleo pode produzir uma amostra por data da primeira batida de cada bloco, mas essa chave não garante um turno por dia nem um turno fechado. Blocos distintos que começam no mesmo dia são concatenados em `horas[k]`, enquanto `impares` registra se **algum bloco** dessa chave teve contagem ímpar. [app/ponto/calculador/nucleo.py:215] [app/ponto/calculador/nucleo.py:236]

A análise abaixo usa o ramo sem `pares` e sem `envelopes`. A cópia não possui metadados Git acessíveis no diretório informado; que seja o HEAD do hash declarado é **[não verificado]**.

## 1. Casos do brief e cinco vínculos

Para ler a tabela: `D` é a data local da primeira batida do bloco e `k = str(D)`. “Entrada, saída, marcos” significa o que sairia **se o detector passasse a consumir essa lista** como primeira, última e comprimento; o detector atual continua agrupando por data civil. [app/ponto/calculador/nucleo.py:236] [app/escala/detector_proposta.py:59]

| Caso do brief | Resultado do núcleo e leitura hipotética |
|---|---|
| **(a) 12x36, 19→07** | Para um turno isolado com exatamente `D 19:00` e `D+1 07:00`: `horas[k]=['19:00','07:00']`, `k ∉ impares`; entrada `19:00`, saída `07:00`, 2 marcos. Datas, bordas da janela e eventuais outras batidas da amostra: **[não verificado]**. [app/ponto/calculador/nucleo.py:215] [app/ponto/calculador/nucleo.py:236] |
| **(b) mesmo horário, 4 batidas** | **Condicional:** se as quatro forem `19:00, I1, I2, 07:00`, em ordem, e nenhum corte ocorrer, `horas[k]=['19:00', I1, I2, '07:00']`, `k ∉ impares`; entrada `19:00`, saída `07:00`, 4 marcos. Faltam horários e datas de `I1` e `I2`; um gap de pelo menos 8 h após quantidade par pode separar o bloco. Resultado exato: **[não verificado]**. [app/ponto/calculador/nucleo.py:186] |
| **(c) diurno 07→19** | Para um turno isolado com somente as duas pontas: `horas[k]=['07:00','19:00']`, `k ∉ impares`; entrada `07:00`, saída `19:00`, 2 marcos. O brief não informa se há batidas intermediárias, datas ou outras batidas: resultado da amostra completa **[não verificado]**. [app/ponto/calculador/nucleo.py:236] |
| **(d) 6x1, 22→06** | Para um turno isolado com somente as pontas: `horas[k]=['22:00','06:00']`, `k ∉ impares`; entrada `22:00`, saída `06:00`, 2 marcos. O gap de exatamente 8 h com uma batida na mão não dispara a condição de paridade. Intermediárias, datas e composição da janela: **[não verificado]**. [app/ponto/calculador/nucleo.py:215] |
| **(e) aberto / solta** | Faltam instantes, datas e tipos das batidas. Para uma única batida isolada `H`, o núcleo devolve `horas[k]=[H]`, `k ∈ impares`, e a leitura hipotética seria `H→H`, 1 marco. Com outras batidas próximas, pode juntá-las; não é possível reproduzir os 11,4 h apenas pelo dado fornecido. **[não verificado]**. [app/ponto/calculador/nucleo.py:186] [app/ponto/calculador/nucleo.py:236] |
| **(f) troca de regime** | Faltam a sequência, as datas, a quantidade de batidas e o momento da troca. Portanto `horas`, `impares`, pontas e marcos por chave são **[não verificado]**; `07→19` no brief é o resultado agregado informado para o detector atual, não a sequência necessária para executar o núcleo. [app/escala/detector_proposta.py:70] |

Para **845:146, 1547:220, 1485:451, 1597:489 e 1604:934**, o dado raspado informa classificações de cadastro e proposta, mas não fornece as batidas individuais, datas ou limites efetivos da amostra. Assim, os retornos por chave dos cinco são **[não verificado]**. Os exemplos da tabela não devem ser atribuídos a esses vínculos. O núcleo depende dos instantes efetivamente recebidos e os ordena antes de cortar. [app/ponto/calculador/nucleo.py:45] [app/ponto/calculador/nucleo.py:109]

## 2. Outro corte sem consultar escala

- **`parear_turnos`**, chamado sem alimentar marcos, mapa de dias ou papéis, recebe objetos com `timestamp` e `tipo`, ordena e usa `E`/`S`; não consulta banco por si. No modo sem bordas, pode reconhecer `S→E` como pausa por duração quando há `S` posterior como prova de continuação. Uma `E` com outra aberta pode deixar a anterior aberta; uma `S` sem entrada pode virar órfã. Ele devolve objetos `Turno`, com `aberto`, e não um conjunto de paridade como o núcleo. Sem bordas da véspera, a data é a data local da referência; com alimentação, pode atribuir à véspera. O fechamento sem jornada declarada usa envelope de 26 h nesse caminho, em vez do teto geral de vão de 14 h do núcleo. [app/ponto/turnos.py:1192] [app/ponto/turnos.py:385] [app/ponto/turnos.py:248] [app/ponto/turnos.py:312] [app/ponto/turnos.py:1417] [app/ponto/turnos.py:1430]

- **`Command.handle` de `e6_piso_autopsia`** contém outro corte sequencial embutido, não uma função reutilizável separada: antes remove batidas a menos de 10 minutos; corta por gap ≥8 h com contagem par ou gap >14 h, sem o teto adicional de vão total do núcleo. A chave é a data da primeira batida; turno ímpar marca a data e é inteiramente excluído da soma, enquanto o núcleo soma seus pares fechados. O comando completo também consulta espelho e previsto; o trecho de corte citado não lê escala para decidir o corte. [app/ponto/management/commands/e6_piso_autopsia.py:65] [app/ponto/management/commands/e6_piso_autopsia.py:75] [app/ponto/management/commands/e6_piso_autopsia.py:89]

Exaustividade de outras implementações ou trechos de corte na casa: **[não verificado]**. O registro de censo do próprio repositório identifica também outras funções de pareamento, mas isso, por si só, não demonstra que cortem turno sem insumo de escala. [app/core/tests/test_medida_turno_do_par.py:151]

## 3. Padrões que expõem o corte

Os exemplos são sequências **ilustrativas**, não batidas atribuídas aos cinco vínculos. A coluna “detector com a fonte trocada” significa primeira e última posição e tamanho de `horas[k]`; isso não é o comportamento atual de agrupamento civil. [app/escala/detector_proposta.py:59] [app/escala/detector_proposta.py:70]

| Padrão | Sequência e retorno do núcleo | Detector com a fonte trocada |
|---|---|---|
| **Dois turnos iniciados em D** | `D 00:00, 02:00; D 10:00, 12:00`: o gap de 8 h corta após duas batidas, mas ambos os blocos têm a mesma chave. `horas[k]=['00:00','02:00','10:00','12:00']`; `k ∉ impares`; `envelope[k]=240` min, soma dos dois vãos. [app/ponto/calculador/nucleo.py:215] [app/ponto/calculador/nucleo.py:236] | `00:00→12:00`, **4 marcos**: os blocos não aparecem separados. [app/ponto/calculador/nucleo.py:238] |
| **Intervalo de 8 h ou mais** | `D 07:00, 09:00, 17:00, 18:00`: o gap de 8 h após duas batidas corta; como os dois blocos começam em D, `horas[k]=['07:00','09:00','17:00','18:00']`, sem `k` em `impares`; `envelope[k]=180` min. O corte exige contagem par para o limiar de 8 h. [app/ponto/calculador/nucleo.py:215] | `07:00→18:00`, **4 marcos**, apesar de o núcleo ter separado os blocos. [app/ponto/calculador/nucleo.py:236] |
| **Dobra** | `D 07:00, D+1 07:00`, apenas duas pontas: o gap e o vão são 24 h, então surgem dois blocos unitários, chaves `D` e `D+1`; cada lista é `['07:00']` e ambas as chaves estão em `impares`. [app/ponto/calculador/nucleo.py:215] [app/ponto/calculador/nucleo.py:243] | Em cada chave, `07:00→07:00`, **1 marco**. Uma dobra com batidas intermediárias pode produzir outra partição; depende dos instantes. **[não verificado]**. [app/ponto/calculador/nucleo.py:186] |
| **Mais de 14 h** | O exemplo documentado `D 06:52, D 21:34` é cortado em dois blocos unitários; como ambos começam em D, `horas[k]=['06:52','21:34']`, `k ∈ impares`, `envelope[k]=0` e `k ∈ cortes`. [app/ponto/calculador/nucleo.py:15] [app/ponto/calculador/nucleo.py:215] | `06:52→21:34`, **2 marcos**: a concatenação esconde a divisão se o consumidor só olhar a lista. [app/ponto/calculador/nucleo.py:236] |
| **Batida esquecida** | Na sequência documentada `D 07:00, 12:00, 17:00; D+1 06:00, 17:00`, o vão até `D+1 06:00` excede 14 h: `horas[D]=['07:00','12:00','17:00']`, `D ∈ impares` e `D ∈ cortes`; `horas[D+1]=['06:00','17:00']`, sem essa chave em `impares`. [app/ponto/calculador/nucleo.py:192] [app/ponto/calculador/nucleo.py:215] | Para D, `07:00→17:00`, **3 marcos**; para D+1, `06:00→17:00`, **2 marcos**. A posição da batida esquecida muda o resultado. [app/ponto/calculador/nucleo.py:236] |

Mesmo quando a lista concatenada tem comprimento par, `impares` pode conter a chave por causa de blocos ímpares individuais, como no exemplo de 14 h 42. [app/ponto/calculador/nucleo.py:236]

## 4. Importação, independência e tripwire

Um leitor de `escala/` importar `ponto.calculador.nucleo` **não cria, pelo código examinado, um ciclo direto de imports**: o módulo do núcleo importa biblioteca padrão e não importa `escala`; `e6_oraculo` já importa a mesma função e chama `minutos_do_oraculo(ts)` sem envelopes. Um novo leitor, isoladamente, não altera essa chamada nem a função. Ciclos indiretos introduzidos por uma implementação ainda não escrita: **[não verificado]**. [app/ponto/calculador/nucleo.py:22] [app/ponto/management/commands/e6_oraculo.py:42] [app/ponto/management/commands/e6_oraculo.py:149]

A independência relevante para essa chamada é que ela continua sem alimentar envelopes de escala. Há teste comportamental que exige o corte nesse modo, e um teste AST que procura derivação aritmética duplicada no núcleo e no comando. **Não localizei, na busca feita, um selo que congele todos os importadores do núcleo ou proíba por AST qualquer leitura de escala dentro dele; exaustividade dessa busca é [não verificado].** O teste citado de unicidade aritmética não é uma lista de importadores. [app/ponto/tests/test_calculador_regras.py:259] [app/ponto/tests/test_e6_oraculo_julga_impar.py:110] [nao verificado]

O tripwire de data verifica padrões AST de `.date()` associados a `timestamp` e tem allowlist para o detector. Se a alteração remover a última ocorrência detectada no detector, o teste de entrada obsoleta exigirá retirar essa entrada da allowlist; isso depende do código final. O visitante também tem limites sintáticos: uma chave derivada dentro do núcleo a partir de `t.date()` não é, por esse fato, prova de correção semântica do dia do turno. [app/ponto/tests/test_contract_no_batida_date.py:18] [app/ponto/tests/test_contract_no_batida_date.py:186] [app/ponto/tests/test_contract_no_batida_date.py:239] [app/ponto/calculador/nucleo.py:236] [nao verificado]

## 5. Consultas e local de execução

**Hoje, por chamada de `propor_escala`:**

- **1 consulta de batidas** ao consumir o queryset de `batidas_apuraveis` em `medir_padrao`. [app/escala/detector_proposta.py:53] [app/ponto/turnos.py:2891]
- **Mais 1 consulta de templates** quando há pelo menos 5 dias e se chega ao laço de `_casa_template`; com menos de 5 dias, retorna antes. Portanto, **1 ou 2 consultas**, sem somar consultas dos chamadores para obter colaborador ou vínculo. [app/escala/detector_proposta.py:99] [app/escala/detector_proposta.py:122]

Uma troca que reutilize a **mesma consulta de batidas**, converta os instantes locais e chame o núcleo em memória acrescentaria **0 consultas pelo núcleo**: nesse ramo a função ordena e opera sobre os dados recebidos. O total continuaria **1 ou 2**, condicionado à implementação e à mesma carga; buscar bordas adicionais ou outros insumos tem custo **[não verificado]** até existir implementação. [app/ponto/calculador/nucleo.py:109] [app/ponto/calculador/nucleo.py:157]

Na **Cadastro x Realidade**, a view chama `lista`, que lê `proposta` da lavra e passa o valor ao botão; esse caminho não recalcula a proposta no clique de leitura da tela. A lavra chama `anomalias_da_frota`, que chama o detector para colaboradores com assinatura. O detector também roda em outros caminhos: coleta do PDF de propostas, comando de limbo e `escala_auto`. [app/escala/views.py:512] [app/escala/services/cadastro_realidade.py:960] [app/ponto/services/esmeril_espelho.py:239] [app/ponto/services/esmeril_espelho.py:321] [app/escala/proposta_pdf.py:43] [app/escala/management/commands/propor_escala_limbo.py:35] [app/escala/services/escala_auto.py:117]

## 19/11 11:08 · sessao df35418f7377 · ESCALA-NAO-DESCREVE · censo · origem=Code

O caminho de **Cadastro x realidade usa a lavra do `MetricaSnapshot`** para a coluna “Destino sugerido”; `botao_da_proposta` não chama o detector. A ausência da chave estruturada `proposta` numa lavra não implica ausência da frase `destino`: a tela lê as duas chaves separadamente. Isso torna compatível, no código, uma frase visível com a falta do botão baseado na proposta estruturada. [cadastro_realidade.py:960](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/cadastro_realidade.py:960), [cadastro_realidade.py:982](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/cadastro_realidade.py:982), [cadastro_x_realidade.html:32](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/templates/escala/cadastro_x_realidade.html:32).

**Limite do censo:** não consegui provar exaustividade para referências calculadas dinamicamente, fluxo arbitrário de dicionários ou a frase exata pedida. Abaixo estão os sítios identificados e o script efetivamente executado; ele é uma busca estática de candidatos, não uma prova de ausência de toda referência dinâmica. Também **NAO ACHEI** confirmação do hash da cópia: `git rev-parse HEAD` respondeu que o diretório não é um repositório Git.

## (a) Chamadores e leitores encontrados

**Produção.**

- `medir_padrao`: chamada por `propor_escala` em [detector_proposta.py:124](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/detector_proposta.py:124).
- `_casa_template`: chamada por `propor_escala` em [detector_proposta.py:130](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/detector_proposta.py:130).
- `propor_escala`: quatro chamadas encontradas, todas com import local: `coletar` em [proposta_pdf.py:43](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/proposta_pdf.py:43); `handle` em [propor_escala_limbo.py:22](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/management/commands/propor_escala_limbo.py:22); `propostas_do_vinculo` em [escala_auto.py:117](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/escala_auto.py:117); e `anomalias_da_frota` em [esmeril_espelho.py:215](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/esmeril_espelho.py:215).

**Testes.** As chamadas diretas encontradas estão em [test_detector_proposta.py:64](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/tests/test_detector_proposta.py:64), :82, :94, :105, :119, :130, :157 e :180. Os testes substituem a função por caminho textual em [test_escala_nao_descreve.py:83](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/tests/test_escala_nao_descreve.py:83) e :110; há inspeção textual em [test_detector_proposta.py:189](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/tests/test_detector_proposta.py:189) e referências de contrato em [test_medida_turno_do_par.py:180](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/core/tests/test_medida_turno_do_par.py:180) e :277.

**Alias, `getattr`, templates e JS.** NAO ACHEI chamada adicional por esses mecanismos na busca textual `rg -n 'medir_padrao|propor_escala|_casa_template' --glob '*.{py,html,js}' app` e na busca AST abaixo. Isso não exclui nome construído em tempo de execução. O template lê dados derivados, como `x.destino` e `x.proposta.codigo`, em [cadastro_x_realidade.html:32](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/templates/escala/cadastro_x_realidade.html:32).

### Chaves e leitores identificados

Na tabela, **D** = [detector_proposta.py](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/detector_proposta.py:53), **PDF** = [proposta_pdf.py](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/proposta_pdf.py:53), **CLI** = [propor_escala_limbo.py](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/management/commands/propor_escala_limbo.py:22), **Auto** = [escala_auto.py](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/escala_auto.py:117), **Esmeril** = [esmeril_espelho.py](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/esmeril_espelho.py:241) e **Teste** = [test_detector_proposta.py](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/tests/test_detector_proposta.py:64). Os números nas células são linhas desses arquivos.

| Chave | Nascimento | Leitores identificados |
|---|---|---|
| `padrao` | D:127, 132, 157 | PDF:77,121; CLI:41; Auto:122,124; Teste:65–67,83,95,106,159,161 | [nao verificado]
| `proposta` | D:127,132,158 | PDF:75,130–131; CLI:39,50–53; Auto:121; Esmeril:243–244,261–263; Teste:69–70,85,97,108,120,158,160 | [nao verificado]
| `confianca` | D:127,132,157 | PDF:76; CLI:38,53,57; Auto:122; Esmeril:247,264; Teste:68,84,96,107 | [nao verificado]
| `porque` | D:128,133,157 | PDF:138; CLI:54,57; Auto:124; Esmeril:262; Teste:68–69,84,98,121 | [nao verificado]
| `padrao.dias` | D:67,87 | D:125; PDF:77,124; CLI:47; Auto:122,124; Teste:95,106,159,161 | [nao verificado]
| `padrao.marcos_moda` | D:67,88 | D:105,136,145,153; PDF:124; CLI:47; Teste:65,83 | [nao verificado]
| `padrao.marcos_freq` | D:89 | D:137,145,153. Não nasce no retorno sem dados de D:67–69; o ramo de poucos dias retorna antes da leitura em D:125–129 |
| `padrao.entrada` | D:67,90 | D:102,104,135; PDF:125; CLI:47; Teste:66 | [nao verificado]
| `padrao.saida` | D:67,91 | D:102,104,135; PDF:125; CLI:47; Teste:67 | [nao verificado]
| `padrao.duracao_h` | D:68,92 | PDF:126; CLI:48 |
| `padrao.dispersao_entrada_min` | D:68,93 | D:141,146,155; PDF:127; CLI:48 |
| `padrao.dows` | D:68,94 | PDF:128; CLI:49 |
| `padrao.padrao_dias` | D:69,95 | PDF:128; CLI:49 |
| `proposta.tipo_escala_id` | D:158 | Auto:122,125; Esmeril:246; a cópia lavrada é lida em [cadastro_realidade.py:824](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/cadastro_realidade.py:824), :843–849 e [regua_defesa.py:307](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/regua_defesa.py:307) |
| `proposta.codigo` | D:158 | PDF:130; CLI:52; Auto:123; Esmeril:246,264; Teste:70,85; cópia lavrada em [cadastro_x_realidade.html:32](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/templates/escala/cadastro_x_realidade.html:32) e [views.py:505](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/views.py:505) |
| `proposta.descricao` | D:159 | Auto:123; Esmeril:264 |
| `proposta.ciclo` | D:159 | PDF:131; CLI:53 |
| `proposta.horario` | D:160 | PDF:130; CLI:52; Auto:123; Esmeril:264 |

Os testes da lavra estruturada e do botão também leem as **cópias** das chaves, por exemplo em [test_escala_nao_descreve.py:98](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/tests/test_escala_nao_descreve.py:98) e :155–165. A tabela registra os leitores identificados, não prova todos os possíveis leitores indiretos.

### Script AST executado

Foi executado via `python3 -B` com o texto abaixo na entrada padrão; não foi criado arquivo. Ele percorre os `.py`, identifica imports locais e apelidos dos três nomes, chamadas e strings com esses nomes; para chaves, imprime acessos literais nos arquivos previamente identificados. A busca de templates e JS foi complementar, por `rg`.

```python
import ast, pathlib

names = {'medir_padrao', 'propor_escala', '_casa_template'}
keys = {
    'padrao', 'proposta', 'confianca', 'porque', 'dias',
    'marcos_moda', 'marcos_freq', 'entrada', 'saida',
    'duracao_h', 'dispersao_entrada_min', 'dows', 'padrao_dias',
    'tipo_escala_id', 'codigo', 'descricao', 'ciclo', 'horario',
}
key_files = {
    'app/escala/detector_proposta.py',
    'app/escala/proposta_pdf.py',
    'app/escala/management/commands/propor_escala_limbo.py',
    'app/escala/services/escala_auto.py',
    'app/ponto/services/esmeril_espelho.py',
    'app/escala/tests/test_detector_proposta.py',
}

for path in pathlib.Path('.').rglob('*.py'):
    try:
        tree = ast.parse(path.read_bytes(), filename=str(path))
    except (SyntaxError, UnicodeError, OSError) as e:
        print('ERRO', path, e)
        continue

    aliases = set(names)
    for n in ast.walk(tree):
        if isinstance(n, ast.ImportFrom) and n.module == 'escala.detector_proposta':
            for a in n.names:
                if a.name in names:
                    aliases.add(a.asname or a.name)
                    print(f'{path}:{n.lineno}: import {a.name} as {a.asname or a.name}')

    for n in ast.walk(tree):
        if isinstance(n, ast.Call):
            f = n.func
            if (isinstance(f, ast.Name) and f.id in aliases
                    or isinstance(f, ast.Attribute) and f.attr in names):
                print(f'{path}:{n.lineno}: chamada {ast.unparse(f)}')
        if (isinstance(n, ast.Constant) and isinstance(n.value, str)
                and any(x in n.value for x in names)):
            print(f'{path}:{n.lineno}: string {n.value[:100]!r}')

    if str(path) in key_files:
        for n in ast.walk(tree):
            if (isinstance(n, ast.Subscript) and isinstance(n.ctx, ast.Load)
                    and isinstance(n.slice, ast.Constant)
                    and n.slice.value in keys):
                print(f'{path}:{n.lineno}: chave {ast.unparse(n)}')
            if (isinstance(n, ast.Call) and isinstance(n.func, ast.Attribute)
                    and n.func.attr == 'get' and n.args
                    and isinstance(n.args[0], ast.Constant)
                    and n.args[0].value in keys):
                print(f'{path}:{n.lineno}: get {ast.unparse(n)}')
```

## (b) Tela e frase

1. A rota chama `cadastro_x_realidade`; a view chama `lista` e renderiza o template: [urls.py:14](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/urls.py:14), [views.py:512](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/views.py:512).
2. `lista` chama `ler_lavra`, lê `destino` e `proposta`, coloca ambos na linha e consulta o botão: [cadastro_realidade.py:960](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/cadastro_realidade.py:960), :966–972 e :982–987. `ler_lavra` seleciona o snapshot `esmeril_espelho` e devolve `detalhe`: [esmeril_espelho.py:359](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/esmeril_espelho.py:359).
3. Na produção da lavra, `anomalias_da_frota` chama o detector para pessoas com assinatura, transforma o retorno em frase com `_destino` e guarda separadamente a proposta estruturada: [esmeril_espelho.py:233](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/esmeril_espelho.py:233), :241–247 e :259–264. `lavrar` chama o produtor e grava o resultado no snapshot: [esmeril_espelho.py:321](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/esmeril_espelho.py:321), :342–355.
4. O template mostra `x.destino`; o botão depende de `x.botao_proposta`: [cadastro_x_realidade.html:24](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/templates/escala/cadastro_x_realidade.html:24), :32. O POST relê a lavra antes de aplicar: [views.py:493](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/views.py:493).

**Frase pedida:** NAO ACHEI o formato literal “entradas próximas … % de … dias” na cópia com `rg -n -i 'entradas|pr[oó]ximas' app --glob '*.py' --glob '*.html' --glob '*.js'` nem com a busca por `entrada real` e `% de dias` em `app/escala`, `app/templates/escala` e `app/ponto/services`.

Há, contudo, uma frase **parecida** na assinatura C7: `diagnostico_regua` produz `real = 'E~HH:MM (N% de N dias)'` em [regua_defesa.py:167](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/regua_defesa.py:167); o diagnóstico é copiado para a lavra do termômetro em [regua_na_lista.py:29](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/regua_na_lista.py:29), :42–64, e `_termometro` o formata como detalhe C7 em [cadastro_realidade.py:315](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/cadastro_realidade.py:315). Não consegui provar que essa seja a frase observada para col451.

Para essa frase semelhante e a coluna de destino, são **medidas distintas**:

- A régua usa, por padrão, `ref - 30 dias` até `ref`, com possível reavaliação de 14 dias: [regua_defesa.py:100](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/regua_defesa.py:100), :118–129 e :181–192. O Esmeril chama o detector de `hoje - 60 dias` até ontem: [esmeril_espelho.py:241](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/esmeril_espelho.py:241).
- A régua desloca para a véspera batidas anteriores ao início declarado menos duas horas e exige ao menos duas por grupo: [regua_defesa.py:151](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/regua_defesa.py:151). O detector agrupa por data civil local: [detector_proposta.py:57](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/detector_proposta.py:57).
- Ambos consultam `batidas_apuraveis`, mas cada um deriva seu agrupamento; a régua declara que não usa o turno pareado, e o detector documenta a circularidade com o cadastro: [regua_defesa.py:118](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/regua_defesa.py:118), [detector_proposta.py:15](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/detector_proposta.py:15).

## (c) Encaixe das decisões e leitores afetados

1. **Amostra com turno fechado:** encaixa na alimentação e agregação de `medir_padrao`, hoje em [detector_proposta.py:55](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/detector_proposta.py:55). O juiz devolve `entrada`, `saida`, `batidas` e `aberto` em [turnos.py:43](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:43); `turnos_do_colab` e sua dependência de escalas/células estão em [turnos.py:1850](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1850) e :1892–1920. Leitores afetados: as chaves de `padrao` e os quatro consumidores de `propor_escala` da tabela.
2. **Turno iniciado na janela conta inteiro:** encaixa na mesma fronteira de amostragem em [detector_proposta.py:57](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/detector_proposta.py:57); a porta de turnos já declara janela por `data_turno` e carga com margem em [turnos.py:1847](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1847). Leitores afetados: os mesmos consumidores da amostra.
3. **Menos de 80% no mesmo eixo implica abstenção:** encaixa antes do retorno de proposta em [detector_proposta.py:130](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/detector_proposta.py:130). O `0.8` atual mede **frequência do número de marcos**, e sua falha hoje produz confiança média, em [detector_proposta.py:137](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/detector_proposta.py:137) e :147–157. Além dos leitores da tabela, o PDF classifica qualquer ausência de proposta com dias suficientes como “sem template” em [proposta_pdf.py:74](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/proposta_pdf.py:74), :99–105.
4. **Desempate por `tipo_ciclo` × `padrao_dias`:** encaixa na escolha do melhor candidato em [detector_proposta.py:105](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/detector_proposta.py:105), :107–119; `padrao_dias` nasce em :79–95. Mudam os leitores de `proposta` e seus campos listados acima.
5. **Excluir `hora_inicio == hora_fim`:** encaixa no filtro de candidatos em [detector_proposta.py:107](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/detector_proposta.py:107). Mudam os mesmos leitores de `proposta`, inclusive frase e botão derivados da lavra em [esmeril_espelho.py:241](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/esmeril_espelho.py:241) e [cadastro_realidade.py:971](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/services/cadastro_realidade.py:971).

## 19/11 11:03 · sessao 0fc23b4a7ea9 · E4C-F5-CONSTROI · construtor · origem=Code

O teste solicitado foi acrescentado em [test_e4c_f5_contador_texto.py:40](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/chamados/tests/test_e4c_f5_contador_texto.py:40). `git diff --check` passou. Conforme solicitado, não executei o teste nem os mutantes; a mordida permanece **[não verificado]**. [nao verificado]

Comando do caso positivo, para o certificador executar com banco de teste configurado ([ci.py:6](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/config/settings/ci.py:6)): [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4c_f5_contador_texto.ContadorTextoTest.test_vigia_carimba_divergencia_de_texto --settings=config.settings.ci -v 2
```

Os comandos seguintes aplicam cada mutante diretamente ao arquivo de produção. Devem ser executados **separadamente em cópias descartáveis**, seguidos do comando de teste acima. [nao verificado]

**M_censo_zero** — espera-se falha na asserção do censo em [test_e4c_f5_contador_texto.py:52](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/chamados/tests/test_e4c_f5_contador_texto.py:52) **[não verificado]**:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
from pathlib import Path
p = Path('chamados/management/commands/vigiar_fio_morto.py')
s = p.read_text()
antes = '                              divergencia_texto_resposta=texto_divergente,'
depois = '                              divergencia_texto_resposta=0,'
assert s.count(antes) == 1
p.write_text(s.replace(antes, depois, 1))
PY
```

**M_fora_do_esperado** — espera-se `KeyError` na consulta ao esperado em [test_e4c_f5_contador_texto.py:53](/home/ronald/wt-codex-e4c-f5-constroi-090617/app/chamados/tests/test_e4c_f5_contador_texto.py:53) **[não verificado]**:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
from pathlib import Path
p = Path('chamados/management/commands/vigiar_fio_morto.py')
s = p.read_text()
antes = '                           desligados_retidos=0, ferias_ausencia_sem_agendamento=0, ferias_dia_sem_ausencia=0,\n                           divergencia_texto_resposta=0)'
depois = '                           desligados_retidos=0, ferias_ausencia_sem_agendamento=0, ferias_dia_sem_ausencia=0)'
assert s.count(antes) == 1
p.write_text(s.replace(antes, depois, 1))
PY
```

Diff obtido por `git diff -- chamados/tests/test_e4c_f5_contador_texto.py`:

```diff
diff --git a/app/chamados/tests/test_e4c_f5_contador_texto.py b/app/chamados/tests/test_e4c_f5_contador_texto.py
index 0fdf23aa0..d0011c621 100644
--- a/app/chamados/tests/test_e4c_f5_contador_texto.py
+++ b/app/chamados/tests/test_e4c_f5_contador_texto.py
@@ -1,8 +1,15 @@
 """Contador de texto: divergencia real, universo por pergunta e custo em lote."""
+import io
+import json
+import os
+import tempfile
 from unittest.mock import patch
 
+from django.core.management import call_command
 from django.db import connection
+from django.test import override_settings
 from django.test.utils import CaptureQueriesContext
+from freezegun import freeze_time
 
 from chamados.models import DecisaoPergunta as D, PerguntaDisputa
 from chamados.services.vigia_texto_resposta import contar
@@ -30,6 +37,23 @@ class ContadorTextoTest(BaseCortes):
             D.registrar([dict(pergunta=outro, ato=D.RESPOSTA, ato_em=RESPOSTA_EM, ato_texto='ato')])
         self.assertEqual(contar(), (1, 3), 'dois atos na mesma pergunta: ainda UMA pergunta no universo')
 
+    @freeze_time('2026-11-13 12:00:00')
+    def test_vigia_carimba_divergencia_de_texto(self):
+        p = self.pergunta()
+        self.responder(p)
+        PerguntaDisputa.objects.filter(pk=p.pk).update(resposta_colab='projecao')
+        out = io.StringIO()
+        with tempfile.TemporaryDirectory() as pasta:
+            carimbo = os.path.join(pasta, 'fio_morto.json')
+            with override_settings(VIGIA_FIO_CARIMBO=carimbo):
+                call_command('vigiar_fio_morto', stdout=out)
+            with open(carimbo, encoding='utf-8') as arquivo:
+                reg = json.load(arquivo)
+        self.assertEqual(reg['censo']['divergencia_texto_resposta'], 1)
+        self.assertEqual(reg['esperado']['divergencia_texto_resposta'], 0)
+        self.assertIn('VFM|DIVERGENCIA_TEXTO_RESPOSTA=1 (esperado 0)', out.getvalue())
+        self.assertGreaterEqual(reg['divergencia_texto_resposta_universo'], 1)
+
     def medir(self, minimo):
         contar()  # aquece a primeira chamada fora da medida
         with patch.object(D, 'leitura', wraps=D.leitura) as leitor:
```

LEI-AKITA: padrao=uma pergunta com divergência real, comando real e conferência do carimbo e stdout; certificação e mordida dos mutantes **[não verificado]**.

## 19/11 10:58 · sessao f9b2b6327f63 · E4C-F5-CONSTROI · verificador · origem=Code

**BLOQUEIA para a volta 2b.** O contador chega ao ESTADO como “fora do esperado”, mas o código examinado não o promove a **PERGUNTA no topo**. Também faltam provas de mordida para o caminho positivo completo e para a retirada dos prefetches dos leitores. [bin/fio_estado.py:121](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/bin/fio_estado.py:121), [bin/gerar_estado.py:312](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/bin/gerar_estado.py:312), [app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:88](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:88)

A cópia contém os arquivos examinados, mas o diretório disponibilizado não é um repositório Git; portanto, a identidade do HEAD, a base e a integridade do diff em relação à árvore são **[não verificado]**. O resumo de runs foi fornecido pelo enunciado; os logs completos dos mutantes e o dado de produção não foram conferidos independentemente **[não verificado]**.

1. **NÃO BLOQUEIA — lei B nos sítios do diff.** `espera_o_texto_do_colaborador` pega o instante da leitura e substitui nela o texto pelo da **mesma pergunta** na projeção. Se a leitura estiver pendente, espera mesmo com texto corrente; se tiver instante e o texto corrente estiver vazio após trim, também espera. [app/chamados/services/fio.py:178](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/fio.py:178), [app/chamados/juizes.py:1526](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/juizes.py:1526)

   O histórico combina os instantes vigentes da leitura com o texto corrente da mesma pergunta: não emite evento de resposta ou validação cujo instante esteja vazio; para os demais, mostra o texto corrente, truncado, ou “sem resposta”. Materialização continua com instante da projeção. Assim, instante e texto podem representar **momentos diferentes**, embora não perguntas diferentes nesse laço. Isso corresponde à separação declarada no anexo de resposta B, que não está disponível como arquivo com linha **[não verificado]**. [app/chamados/services/historico.py:96](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/historico.py:96)

2. **NÃO BLOQUEIA — com limites na verificação dos escritores.** O mapa relevante é:

   - `respondida_em` e `resposta_colab`: a resposta humana grava ambos e registra o ato; a resposta automática também grava ambos, enquanto a cura pela trilha carimba o instante de texto já existente. A resposta humana limpa validação anterior. [app/chamados/services/disputa_emissao.py:1474](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/disputa_emissao.py:1474), [app/chamados/services/disputa_emissao.py:1671](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/disputa_emissao.py:1671), [app/chamados/services/disputa_emissao.py:1692](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/disputa_emissao.py:1692)
   - `validada_em`: `_carimbar_validacao` grava o instante e o ato com o mesmo valor; devolução pode limpá-lo e registrar outro ato. `carimbar` também pode preencher resposta e validação na projeção. [app/chamados/services/validacao.py:161](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/validacao.py:161), [app/chamados/services/validacao.py:262](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/validacao.py:262), [app/chamados/models.py:3299](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/models.py:3299)
   - `materializada_em`: a porta do modelo atribui o carimbo; o histórico o lê diretamente. [app/chamados/models.py:3269](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/models.py:3269), [app/chamados/services/historico.py:104](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/historico.py:104)
   - `decisoes` é escrita por `registrar`; `leitura` ordena atos por instante e PK, acrescenta passivo da projeção sem linha correspondente e aplica reabertura, recusa e devolução. Portanto, “leitura” não significa somente linhas físicas de atos. [app/chamados/models.py:4238](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/models.py:4238), [app/chamados/models.py:4295](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/models.py:4295), [app/chamados/models.py:4329](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/models.py:4329)
   - `divergencia_texto_resposta` é calculada pelo contador, escrita em `censo` no carimbo com esperado zero e lida genericamente pelo ESTADO. [app/chamados/services/vigia_texto_resposta.py:8](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/vigia_texto_resposta.py:8), [app/chamados/management/commands/vigiar_fio_morto.py:260](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/management/commands/vigiar_fio_morto.py:260), [bin/fio_estado.py:121](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/bin/fio_estado.py:121)

   Os campos de resposta, validação e materialização são `DateTimeField`; o carimbo do vigia publica instante com offset e dia local separadamente. A uniformidade de fuso e significado em **todos** os escritores e leitores fora do diff permanece **[não verificado]**. [app/chamados/models.py:2634](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/models.py:2634), [app/chamados/models.py:3647](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/models.py:3647), [app/chamados/management/commands/vigiar_fio_morto.py:255](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/management/commands/vigiar_fio_morto.py:255)

3. **BLOQUEIA — saída exigida para divergência positiva.** O contador percorre cada pergunta do queryset recebido, inclui o passivo pela leitura e soma uma divergência por pergunta; no vigia, o universo é toda a frota ou o recorte de empresas. O teste cobre divergência, passivo, recorte e dois atos na mesma pergunta. O `0 em 51.563` de produção consta somente do dado fornecido **[não verificado]**. [app/chamados/services/vigia_texto_resposta.py:8](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/vigia_texto_resposta.py:8), [app/chamados/management/commands/vigiar_fio_morto.py:191](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/management/commands/vigiar_fio_morto.py:191), [app/chamados/tests/test_e4c_f5_contador_texto.py:19](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/tests/test_e4c_f5_contador_texto.py:19)

   A comparação de textos é um predicado novo de divergência para o vigia, expressamente solicitado pelo anexo de resposta; não é chamada ao juiz registrado de pergunta respondida, que julga instante mais texto não vazio. A autorização literal do anexo não tem arquivo:linha disponível **[não verificado]**. [app/chamados/services/vigia_texto_resposta.py:16](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/vigia_texto_resposta.py:16), [app/chamados/juizes.py:1544](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/juizes.py:1544)

   **Falha concreta:** com valor positivo, o leitor inclui a chave em “FORA DO ESPERADO” dentro da seção Fio morto; sua manchete continua sendo a de alcance. O gerador coloca essa seção abaixo de Fábrica, Raias e Obras interrompidas. Não há, nesse caminho, promoção a **PERGUNTA no topo**, como exige o texto fornecido. [bin/fio_estado.py:165](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/bin/fio_estado.py:165), [bin/fio_estado.py:191](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/bin/fio_estado.py:191), [bin/gerar_estado.py:312](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/bin/gerar_estado.py:312)

4. **BLOQUEIA — prova de custo incompleta.** Os caminhos identificados carregam decisões em lote: cobrança, montagem do fio, histórico e contador; `coletar` carrega `perguntas__decisoes`, e `classificar` tem prefetch de contingência para suas perguntas. Não identifiquei N+1 novo nesses caminhos com o código atual. [app/chamados/services/cobranca.py:236](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/cobranca.py:236), [app/chamados/services/fio.py:262](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/fio.py:262), [app/chamados/services/historico.py:96](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/historico.py:96), [app/chamados/relatorios_disputa.py:11](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/relatorios_disputa.py:11)

   Os testes comparam uma e várias perguntas e exigem chamadas à leitura; o de `coletar` cria várias disputas, necessário para expor uma consulta de contingência por disputa. Contudo, o dado fornecido só relata mutante de custo para o **contador**; a mordida à retirada de cada prefetch dos demais chamadores não está demonstrada **[não verificado]**. No fio, a contagem global de chamadas a `leitura` tampouco identifica, por si só, qual consumidor as produziu. [app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:88](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:88), [app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:121](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:121)

5. **BLOQUEIA — selo positivo de ponta a ponta ausente.** Os testes novos exercitam os leitores reais para divergência de instante, reabertura, texto da projeção e histórico; o contador tem caso positivo próprio. O resumo fornecido relata morte dos mutantes correspondentes, mas os logs completos e o conteúdo exato das mutações são **[não verificado]**. [app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:42](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:42), [app/chamados/tests/test_e4c_f5_contador_texto.py:19](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/tests/test_e4c_f5_contador_texto.py:19)

   O teste vizinho de memória usa `(0, 0)` como mock: preserva sua prova específica sobre C5, mas não prova o novo contador. O teste O261 chama o contador real e conserva suas comparações anteriores, porém não demonstra publicação de divergência **positiva** nem sua chegada como PERGUNTA no topo. Assim, a igualdade com zero pode passar sem testar o caso que deve alarmar. [app/chamados/tests/test_e4a2_c5_memoria.py:118](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/tests/test_e4a2_c5_memoria.py:118), [app/chamados/tests/test_o261_vigia_fio.py:825](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/tests/test_o261_vigia_fio.py:825)

6. **BLOQUEIA — selo sem mordida suficiente**, pelos itens 4 e 5. O diff mistura intencionalmente duas fontes para estado e texto, e a leitura incorpora passivo; essas diferenças de significado são explícitas, mas a uniformidade de todos os escritores externos não foi esgotada **[não verificado]**. A resposta humana relê o relógio após a trava no caminho examinado; não concluo uma auditoria completa de relógios e ordem de trava de todos os escritores externos **[não verificado]**. Pré-requisitos e pouso real também são **[não verificado]**. [app/chamados/models.py:4295](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/models.py:4295), [app/chamados/services/disputa_emissao.py:1517](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/disputa_emissao.py:1517)

7. **NÃO BLOQUEIA — escopo do diff fornecido.** Os leitores alterados preservam o texto da projeção; o diff acrescenta prefetch, contador e publicação no carimbo, sem alteração apresentada em motor, folha, export, DiaPago, escala ou vínculo. Há escrita da **nova chave no carimbo existente**, autorizada pelo contador descrito no anexo; não identifiquei nova escrita de campos da pergunta no diff. A completude do diff frente ao Git permanece **[não verificado]**. [app/chamados/services/fio.py:188](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/fio.py:188), [app/chamados/services/historico.py:96](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/services/historico.py:96), [app/chamados/management/commands/vigiar_fio_morto.py:255](/home/ronald/codex_ws/b7ccc61ce4a22ad893c70352b9ec637df83a9ae4/app/chamados/management/commands/vigiar_fio_morto.py:255)

BLOQUEIA: 4 -- itens 3, 4, 5 e 6

## 19/11 10:50 · sessao 7eb07578ea99 · DST-L2-JZ1 · leitura · origem=Code

A triagem não identifica corretamente todos os criadores nesta cópia. Os criadores diretos de `Justificativa` encontrados no código de aplicação são API, tela de ponto, cron de alertas e regularização; os endereços de benefícios, advertência e materialização citados criam outras entidades. A conclusão sobre a corrida depende também da transação externa: nas portas de aprovação examinadas, o colaborador já é travado antes de se chegar à retratação. [api/views.py:2011](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/api/views.py:2011), [ponto/views.py:1638](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/views.py:1638), [processar_alertas_turno.py:394](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/management/commands/processar_alertas_turno.py:394), [regularizacao.py:91](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/services/regularizacao.py:91), [justificativa.py:139](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/services/justificativa.py:139), [ponto/views.py:1795](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/views.py:1795).

A matriz abaixo é uma **análise condicional**, não uma reprodução em banco. O código não estabelece que cada view ou o cron inteiro seja uma única transação; portanto, não é seguro atribuir um único “commit de T-cria” ao fluxo inteiro. Estado efetivo das constraints, seu instante de verificação, isolamento e esperas em produção: **[não verificado]**. Os settings exibem PostgreSQL, mas não provam o catálogo efetivo nem o isolamento da conexão de produção. [prod.py:9](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/config/settings/prod.py:9), [saas.py:83](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/config/settings/saas.py:83).

## Travas e escritor comuns à matriz

- **R**: após a primeira leitura de retenção, `retratar_com_motivo` abre `atomic` e pede `FOR UPDATE` no colaborador, nas disputas abertas e nos chamados pertinentes, inclusive o chamado alvo. Essa chamada não fornece IDs de perguntas; a função só as trava se receber IDs. Não usa `no_key=True`. Depois, o escritor consulta as justificativas candidatas com `SELECT FOR UPDATE`. [reconciliador.py:544](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/reconciliador.py:544), [models.py:2853](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/models.py:2853), [models.py:2875](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/models.py:2875), [justificativa.py:283](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/services/justificativa.py:283).
- **P**: ao apontar uma justificativa, a porta abre `atomic`, pede `FOR UPDATE` no colaborador e no chamado, nessa ordem, e atualiza o ponteiro. A guarda de ponteiro nulo ocorre **antes** da trava. [vinculos.py:81](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/services/vinculos.py:81), [models.py:488](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/models.py:488).
- **FK**: sob a hipótese de constraints PostgreSQL efetivas e verificadas, a verificação da FK pede `FOR KEY SHARE` na linha referenciada. O `INSERT` de J3 refere colaborador e, quando preenchido, chamado; o `UPDATE` do ponteiro refere J3. `FOR KEY SHARE` conflita com o `FOR UPDATE` de R/P. O instante real da verificação — inclusive eventual postergação até o commit — é **[não verificado]**. As declarações estão em [ponto/models.py:136](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/models.py:136), [ponto/models.py:173](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/models.py:173) e [chamados/models.py:75](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/models.py:75).

O universo que decide a morte é a consulta de `pendentes_do_vinculo(..., travar=True)`: ela seleciona pendentes pela origem **ou** pelo ID do ponteiro presente na instância `ch`, exclui a justificativa em aprovação e ordena por PK. Em seguida, `rejeitar_justificativas_dos_chamados` grava `status='rejeitada'` e descrição. A retratação chama esse escritor com `portao_de_retencao=False`. [justificativa.py:279](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/services/justificativa.py:279), [justificativa.py:337](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/services/justificativa.py:337), [reconciliador.py:573](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/reconciliador.py:573).

## Matriz de intercalação

As células de resultado pressupõem: J3 é distinta de J1, está pendente e alcançável pelo predicado; não há irmã que retenha nem outra recusa da transição; ambas as transações terminam sem erro. Esses pressupostos são necessários: a retenção por justificativa alheia só é consultada dentro de `_APROVANDO`, e a própria transição pode ser recusada. [reconciliador.py:449](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/reconciliador.py:449), [reconciliador.py:560](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/reconciliador.py:560).

| Criador ou caminho | C cria e commita J3 **antes da leitura inicial de R** | R lê sem J3; C commita J3 **antes de R adquirir as travas** | R adquire as travas e executa o escritor **antes do commit de C** | [nao verificado]
|---|---|---|---|
| **API**, com `chamado_origem=ch`; depois pode executar P. [api/views.py:1974](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/api/views.py:1974), [api/views.py:2011](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/api/views.py:2011) | Em aprovação, a leitura vê J3 e R retorna retido: chamado permanece no estado anterior, J3 pendente; não chega às travas R. Fora de aprovação, não há essa retenção: se a transição passar, R trava J3 e a rejeita. | Com a leitura antiga vazia e J3 já confirmada pela origem, R adquire suas travas e o `SELECT FOR UPDATE` posterior alcança J3: chamado resolvido, J3 rejeitada. Esse cronograma exige que uma transação externa de R ainda **não** detenha trava conflitante. | J3 não confirmada não integra o snapshot do escritor: se R conclui, chamado resolvido e J3 não é rejeitada por esse ato. A verificação FK ou P pode fazer C esperar R; se C já detiver trava FK conflitante, R pode esperar C, impedindo esta ordem específica. Após R, uma criação já decidida com base em leitura anterior pode deixar J3 pendente ligada a chamado resolvido. Instante concreto da espera: **[não verificado]**. |
| **Tela de ponto**, com `chamado_origem=ch`; depois pode executar P. [ponto/views.py:1618](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/views.py:1618), [ponto/views.py:1638](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/views.py:1638) | Mesmo resultado da API, condicionado a a busca ter escolhido esse chamado. | Mesmo resultado da API: J3 alcançada pela origem e rejeitada, se C commitou nessa janela e R não estava já protegido por trava externa conflitante. | Mesmo resultado da API quanto a invisibilidade e espera FK/P; chamado resolvido e J3 pendente se os atos terminarem nessa ordem. |
| **Cron**, quando `get_or_create` efetivamente cria J3 com `chamado_origem=ch`; depois pode executar P. [processar_alertas_turno.py:392](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/management/commands/processar_alertas_turno.py:392) | Mesmo resultado da API. Se `get_or_create` encontra uma linha existente, não há nascimento de J3 nessa execução. | Mesmo resultado da API, para o commit da criação pela origem na janela indicada. | Mesmo resultado da API para o nascimento não confirmado. O método não declara um `atomic` envolvendo criação **e** P; tratar ambos como um único commit é **[não verificado]**. |
| **Regularização**, `get_or_create` sem origem e sem apontamento nesse bloco. [regularizacao.py:88](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/services/regularizacao.py:88) | Por esse criador isoladamente, J3 não entra no vínculo de `ch`; não produz retenção por J3 nem rejeição dela. R pode resolver; J3 fica pendente. | Mesmo resultado: o commit de J3, por si só, não a torna candidata. | Mesmo resultado quanto ao predicado. A regularização tem `atomic` próprio e depois chama `ch.analisar()` nos chamados encontrados; esperas adicionais e resultado integral desse fluxo dependem dos seus demais caminhos: **[não verificado]**. [regularizacao.py:126](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/services/regularizacao.py:126) |

Nas duas primeiras linhas, há também o ramo **atualiza J existente**: API e tela fazem `j.save()` e podem preencher uma origem antes nula; ele não é nascimento de J3 e tem travas de escrita sobre essa J. O resultado depende do vínculo e do status visíveis ao statement de R. [api/views.py:2000](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/api/views.py:2000), [ponto/views.py:1624](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/views.py:1624).

Há uma limitação importante para a segunda coluna: na aprovação individual e no lote examinados, `travar_origem_antes` adquire a ordem global **antes** de travar J1 e de prosseguir com a aprovação. Para uma J3 do mesmo colaborador, uma FK efetiva e sua verificação podem, portanto, impedir que o criador confirme nessa janela. Não se pode afirmar a ocorrência dessa célula nesses caminhos sem demonstrar a ordem completa da transação externa e das constraints. [justificativa.py:110](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/services/justificativa.py:110), [justificativa.py:140](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/services/justificativa.py:140), [ponto/views.py:1795](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/views.py:1795).

### Endereços da triagem que não são criadores de J3

| Endereço citado | O que o código desta cópia mostra |
|---|---|
| `colaboradores/views.py:2036` | Passa `chamado_origem` para `registrar_solicitacao_beneficio`, que cria `SolicitacaoBeneficio`, não `Justificativa`. [views.py:2031](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/colaboradores/views.py:2031), [beneficios.py:20](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/colaboradores/services/beneficios.py:20) |
| `chamados/services/acoes_chamado.py:212` | Campo da criação de `Advertencia`. [acoes_chamado.py:199](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/services/acoes_chamado.py:199) |
| `chamados/services/materializacao.py:989` e `:1025` | Argumentos de `criar_ausencia`, não criação de justificativa. [materializacao.py:983](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/services/materializacao.py:983), [materializacao.py:1019](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/services/materializacao.py:1019) |
| `chamados/services/cobertura.py:110` e `ponto/services/vinculos.py:87` | Esses caminhos não foram encontrados nesta cópia: **[não verificado]**. Existe `chamados/services/vinculos.py:87`, que aponta uma justificativa já recebida e toma P; não a cria. [vinculos.py:81](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/services/vinculos.py:81) |

## FKs e releitura proposta

Os dois campos são declarados como FKs Django, ambos anuláveis e com `on_delete=models.SET_NULL`: `ChamadoColaborador → Justificativa` para o ponteiro, e `Justificativa → ChamadoColaborador` para a origem. As migrations adicionam os mesmos campos sem declarar `db_constraint=False`. A existência e definição das constraints no banco efetivo são **[não verificadas]**; `on_delete` é a política declarada no Django, não prova, por si só, de uma ação SQL `ON DELETE SET NULL` no catálogo. [chamados/models.py:75](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/models.py:75), [migration 0010:15](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/migrations/0010_chamadocolaborador_justificativa_relacionada.py:15), [ponto/models.py:173](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/models.py:173), [migration 0020:15](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/migrations/0020_justificativa_chamado_origem.py:15).

Se a retenção for relida depois de R, em `READ COMMITTED`, uma J3 **já commitada e alcançável pela origem** no início dessa nova consulta será vista e, durante `_APROVANDO`, reterá o chamado. Isso fecha a célula da segunda coluna para esse vínculo, desde que a releitura ocorra de fato. O snapshot é por statement; a consulta posterior do escritor não herda o universo da leitura anterior. [reconciliador.py:453](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/reconciliador.py:453), [justificativa.py:283](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/services/justificativa.py:283).

Essa releitura **não é uma garantia geral demonstrada pelo código**:

- Fora de `_APROVANDO`, ela não pergunta por justificativa alheia; uma J3 pendente já visível continua podendo ser rejeitada pelo escritor. [reconciliador.py:453](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/reconciliador.py:453), [reconciliador.py:580](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/reconciliador.py:580).
- Para vínculo **somente pelo ponteiro**, a consulta usa `ch.justificativa_relacionada_id` da instância recebida. `trava_na_ordem` devolve IDs e não atualiza essa instância. Se o ponteiro mudou antes da trava, reler apenas a função com a instância velha pode ignorá-lo; se o escritor mais tarde receber uma instância com ponteiro atualizado, poderá alcançá-lo. A ocorrência dessa sequência concreta no fluxo inteiro é **[não verificada]**. [justificativa.py:279](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/services/justificativa.py:279), [models.py:2873](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/models.py:2873).
- Uma J3 não confirmada no instante da releitura não é vista por ela. Para J3 ligada por FK ao mesmo chamado ou colaborador, o `FOR UPDATE` de R e a verificação efetiva da FK podem impedir o commit concorrente até R terminar; não se deve presumir que ela consiga commitar entre a releitura e o escritor. O momento concreto de verificação da FK permanece **[não verificado]**. [models.py:2853](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/models.py:2853), [ponto/models.py:173](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/models.py:173).

## Espera nova ou deadlock

A própria releitura proposta chama `pendentes_do_vinculo` com `travar=False`: é uma leitura simples, portanto não acrescenta `SELECT FOR UPDATE` em J3. A espera nova identificável vem de **mover a leitura para depois da aquisição de R**: R pode esperar uma trava conflitante de C antes de ler, ou C pode esperar R na verificação de FK ou em P. O sentido depende da ordem e do momento efetivo da verificação. [justificativa.py:262](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/services/justificativa.py:262), [reconciliador.py:556](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/reconciliador.py:556), [vinculos.py:86](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/chamados/services/vinculos.py:86).

**Não está demonstrado um deadlock novo causado pela releitura simples** com os criadores de J3 examinados. Um ciclo exige provar duas esperas simultâneas, inclusive quais versões de J são visíveis e quando as FKs são verificadas; esses dados não estão no anexo nem no catálogo do banco: **[não verificado]**. O código registra que outros caminhos, entre eles regularização, têm ordem própria e que pode haver ciclo em cenários mais amplos, mas isso não prova um deadlock específico desta alteração. [justificativa.py:101](/home/ronald/codex_ws/c5a74ace3629aec9a904245fcf471ac2168bed15/app/ponto/services/justificativa.py:101).

## 19/11 10:48 · sessao b95331667c03 · ESPELHO-VERDADE · leitura · origem=Code

**O campo `minutos_realizados` da linha do espelho é calculado a partir de turnos pedidos novamente por `turnos_do_colab` para a janela visual inteira.** Ele não é copiado do resultado do motor nem do campo de minutos da ata. A sonda informa 11 valores positivos com `horas_oraculo=[]` e sem par completo na lista que ela mostrou, mas não fornece o código da própria sonda, as janelas exatas nem os turnos carregados pela chamada interna do espelho. Portanto, **o par e a causa específica de cada um dos 11 resultados permanecem [não verificado]**. A observação dos valores vem do anexo fornecido, sem arquivo e linhas verificáveis na cópia: [não verificado].

### 1. De onde vem o dia creditado

1. `espelho_do_colab` determina `vis_ini` e chama `autoridade_do_periodo(colaborador, vis_ini, hoje)`; depois chama `montar_dias` com esse mesmo intervalo visual. [app/ponto/services/espelho.py:1241](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:1241), [app/ponto/services/espelho.py:1278](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:1278).
2. `montar_dias` pede `turnos_do_colab(colaborador, data_ini, data_fim_mes)` uma vez, percorre cada data da janela e passa essa data e os turnos carregados a `_realizado_do_dia_tela`. A chave `data` da linha recebe a data do laço. [app/ponto/services/espelho.py:623](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:623), [app/ponto/services/espelho.py:667](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:667), [app/ponto/services/espelho.py:778](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:778).
3. `realizado_do_dia` encaminha os turnos a `realizado_dos_turnos`, que seleciona **`t.data_turno == data`**. Portanto, o crédito vai à linha cuja data coincide com o `data_turno` atribuído pelo pareador. [app/ponto/turnos.py:774](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:774), [app/ponto/turnos.py:719](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:719).
4. Ao formar um `Turno`, `_mk` chama `_data_do_turno`: a referência inicial é a data local do `timestamp` da entrada ou, se não houver entrada, da saída. Conforme os marcos da véspera, a travessia de meia-noite e o envelope da jornada, pode atribuir a véspera. [app/ponto/turnos.py:285](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:285), [app/ponto/turnos.py:312](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:312).

O motor também é chamado nessa montagem, mas por uma trilha paralela: recebe batidas e células e calcula por fatias de vínculo, ou diretamente se não houver escalas; seu resultado é passado a `montar_dias`. Isso não altera a origem do campo `minutos_realizados` descrita acima. [app/ponto/services/espelho.py:1035](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:1035), [app/ponto/services/espelho.py:1147](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:1147), [app/ponto/services/espelho.py:1178](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:1178).

### 2. De onde vêm as pontas que viram minutos

`turnos_do_colab` carrega batidas não retratadas na janela com dois dias de margem antes e um depois; carrega células, vínculos e DNA; divide a janela em segmentos por vínculo do dia; e chama `parear_turnos` com cópias das batidas do segmento, marcos de intervalo e de borda, marcos por dia, papéis vindos da ata e provedor da última batida. Retém os turnos cujo `data_turno` pertence ao segmento. [app/ponto/turnos.py:1834](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1834), [app/ponto/turnos.py:1867](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1867), [app/ponto/turnos.py:1940](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1940), [app/ponto/turnos.py:1961](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1961), [app/ponto/turnos.py:1977](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1977).

No cálculo usual, a entrada é `t.entrada`; o fim é `t.saida` ou, em turno aberto, uma saída anterior que feche par dentro das batidas do turno. Sem entrada, `_fim_fechado` não devolve fim. O cálculo trunca as pontas ao minuto, subtrai pausas reconhecidas ou, quando aplicável, a sobreposição com o intervalo declarado, e soma os líquidos. [app/ponto/turnos.py:609](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:609), [app/ponto/turnos.py:733](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:733).

Existe uma **exceção posicional** antes desse laço: se as guardas de `_borda_posicional` forem satisfeitas, o cálculo usa a entrada do único turno com fim fechado e a última batida dos turnos daquele dia, descontando um único vão interior elegível. Entre as guardas estão: um único turno com fim fechado, ausência de pausas marcadas, pelo menos quatro batidas e exatamente um vão interior elegível. A última batida pode pertencer a outro turno devolvido pelo pareador. A condição ter ocorrido em qualquer um dos 11 casos é [não verificado]. [app/ponto/turnos.py:638](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:638), [app/ponto/turnos.py:673](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:673), [app/ponto/turnos.py:725](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:725).

### 3. Onde muda a autoridade ou a janela

Para o campo do espelho, **não há um ponto em que a lista interna de `parear_turnos` seja simplesmente descartada**: `turnos_do_colab` devolve seus turnos, e `realizado_dos_turnos` os lê. Há, porém, a exceção posicional acima, que escolhe uma ponta entre as batidas de mais de um turno do dia. [app/ponto/turnos.py:1997](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1997), [app/ponto/turnos.py:719](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:719).

O oráculo tem outra derivação: o carregador E6 busca timestamps apuráveis entre `ini - 12h` e `fim + 1 dia + 14h` e chama `minutos_do_oraculo(ts)` **sem `pares`**. Nesse ramo, a função ordena os instantes, aplica cortes próprios e chaveia as horas pela data civil do primeiro instante de cada sequência. [app/ponto/management/commands/e6_oraculo.py:149](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/e6_oraculo.py:149), [app/ponto/calculador/nucleo.py:109](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/nucleo.py:109), [app/ponto/calculador/nucleo.py:215](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/nucleo.py:215), [app/ponto/calculador/nucleo.py:235](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/nucleo.py:235).

O carregador E6 também tem uma chamada separada de `turnos_do_colab(c, ini-1 dia, fim+1 dia)`, mas **não é possível afirmar que essa foi a chamada da sonda nem quais `ini` e `fim` ela usou**: o código `sonda_v3.py` não foi fornecido no anexo e sua implementação não foi encontrada na cópia [não verificado]. [app/ponto/management/commands/e6_oraculo.py:156](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/e6_oraculo.py:156).

No motor há uma substituição concreta em outra trilha: `_periodos_via_autoridade` converte os turnos pareados e passa o resultado a `_periodos_pelo_marco` em [app/ponto/motor_calculo_v2.py:530](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/motor_calculo_v2.py:530). Esse caminho pode substituir períodos pela leitura da ata; retorna o trio original, por exemplo, para turno partido ou sem células alimentadas. Essa substituição **não é a atribuição do campo `minutos_realizados` da linha**. [app/ponto/motor_calculo_v2.py:534](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/motor_calculo_v2.py:534), [app/ponto/motor_calculo_v2.py:558](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/motor_calculo_v2.py:558), [app/ponto/services/espelho.py:797](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:797).

### 4. Caminhos existentes e os 11 casos

| Caminho | Condição e efeito |
|---|---|
| **Realizado da linha** | Sempre chama `_realizado_do_dia_tela` para a data iterada; dentro dele há o ramo posicional e o ramo usual descritos acima. [app/ponto/services/espelho.py:688](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:688), [app/ponto/turnos.py:725](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:725). |
| **Grade visual por célula ou builder** | A célula é lida a partir de `max(data_ini, apur_ini)`; o trecho anterior usa builder, e `AtaAgregadaSemMarcos` faz o período cair no builder. A grade preenche `realizado_grade`, enquanto o número vem de `_real_dia.minutos`. [app/ponto/services/espelho.py:532](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:532), [app/ponto/services/espelho.py:542](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:542), [app/ponto/services/espelho.py:551](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:551), [app/ponto/services/espelho.py:786](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:786). |
| **Motor: pareador e ata** | O motor passa os turnos ao adaptador e depois pode usar a ata para períodos; as guardas de cobertura ficam em `_periodos_pelo_marco`. [app/ponto/motor_calculo_v2.py:530](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/motor_calculo_v2.py:530), [app/ponto/motor_calculo_v2.py:549](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/motor_calculo_v2.py:549). |
| **Fallback de data civil no motor** | Se o mapa do juiz não contém a entrada do período, o motor usa a data local da entrada; em um caminho também existe herança do dia anterior para período subsequente com intervalo de até seis horas. Essas regras são do motor, não da seleção de `t.data_turno` para o relógio da linha. [app/ponto/motor_calculo_v2.py:2400](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/motor_calculo_v2.py:2400), [app/ponto/motor_calculo_v2.py:2940](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/motor_calculo_v2.py:2940). |

Para os 11, o **campo informado** segue o primeiro caminho pela atribuição em `montar_dias`; qual dos ramos interno usual ou posicional ocorreu em cada caso, quais turnos ele recebeu e qual caminho desenhou a grade são [não verificado]. [app/ponto/services/espelho.py:689](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:689), [app/ponto/services/espelho.py:797](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:797).

### 5. Escritores, leitores e significado dos campos

| Campo ou insumo | Escrita e leitura verificadas | Significado relevante |
|---|---|---|
| `Batida.timestamp`, `tipo`, `colaborador` | `registro_batida` cria a batida com esses campos; `turnos_do_colab` busca por colaborador e data do timestamp; o pareador ordena por timestamp e usa tipo e alimentação. [app/ponto/registro_batida.py:170](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/registro_batida.py:170), [app/ponto/turnos.py:1867](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1867), [app/ponto/turnos.py:1192](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1192). | `timestamp` é instante; sua data local é referência para o juiz, mas pode resultar em `data_turno` da véspera. O `tipo` gravado pode ser superado pelo papel da ata quando esta responde. [app/ponto/turnos.py:285](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:285), [app/ponto/turnos.py:1201](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1201). |
| `retratada_em` | A porta de retratação escreve o instante; a porta de desfazer pode limpar o campo. Tanto a consulta do espelho quanto `batidas_apuraveis` excluem retratadas. [app/ponto/registro_batida.py:294](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/registro_batida.py:294), [app/ponto/services/retratar_humano.py:207](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/retratar_humano.py:207), [app/ponto/turnos.py:1868](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1868), [app/ponto/turnos.py:2892](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:2892). | É filtro do universo, não dia da jornada. [app/ponto/turnos.py:2872](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:2872). |
| `CelulaDia.data`, `escala_geradora`, `dna.marcos` | A porta de célula cria e pode regenerar esses dados; `vinculo_do_dia` lê a geradora e sua vigência; `marcos_dna_periodo` lê os marcos congelados. [app/ponto/portas/celula.py:289](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/portas/celula.py:289), [app/ponto/portas/celula.py:305](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/portas/celula.py:305), [app/escala/alimentacao.py:269](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/alimentacao.py:269), [app/escala/utils.py:96](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/utils.py:96). | A data identifica a célula do dia; seus marcos alimentam a geometria e a pausa. A escolha do vínculo pode retornar `None` se a geradora estiver fora da vigência; sem geradora, há fallback para vínculo que cobre a data. [app/escala/alimentacao.py:279](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/alimentacao.py:279), [app/escala/models.py:1135](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/escala/models.py:1135). |
| `CelulaDia.ata`: lâmpadas, luzes e órfãs | O cartório monta a ata a partir da grade e a entrega à porta de lavra; `periodos_do_dia` lê ata e DNA, e `papel_por_minuto_da_ata` transforma a resposta em papéis para o pareador, sujeitos às guardas de correspondência. [app/ponto/services/cartorio.py:749](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cartorio.py:749), [app/ponto/services/cartorio.py:1898](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cartorio.py:1898), [app/ponto/portas/celula.py:1091](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/portas/celula.py:1091), [app/ponto/juiz_batida.py:149](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/juiz_batida.py:149), [app/ponto/turnos.py:1631](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:1631). | A luz tem precisão de hora e minuto; o leitor reconstrói instantes ancorados na data da célula e na ordem dos marcos. O motor procura a batida correspondente para recuperar o timestamp com segundos. [app/ponto/juiz_batida.py:206](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/juiz_batida.py:206), [app/ponto/motor_calculo_v2.py:580](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/motor_calculo_v2.py:580). |
| `Turno.data_turno` e `minutos_realizados` da linha | `_mk` constrói o `Turno` em memória; `realizado_dos_turnos` lê `data_turno`; `montar_dias` escreve no dicionário da linha o resultado calculado; o leitor E6 prefere esse campo quando não é `None`. [app/ponto/turnos.py:312](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:312), [app/ponto/turnos.py:719](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:719), [app/ponto/services/espelho.py:797](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:797), [app/ponto/management/commands/e6_oraculo.py:263](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/management/commands/e6_oraculo.py:263). | `data_turno` é dia atribuído à jornada, não necessariamente data civil do instante. A ata também possui uma chave chamada `minutos_realizados`, escrita pelo cartório, mas ela não é a fonte direta desse campo da linha. [app/ponto/turnos.py:285](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:285), [app/ponto/services/cartorio.py:757](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/cartorio.py:757), [app/ponto/services/espelho.py:797](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/services/espelho.py:797). |

A diferença de significado verificável é, portanto, principalmente **instante da batida → data civil local → dia atribuído ao turno**: o oráculo sem pares agrupa pela data civil do início de sua sequência, enquanto o espelho seleciona o `data_turno` produzido pelo juiz. Isso demonstra uma diferença de regra capaz de mudar a linha, mas **não demonstra que ela explica qualquer um dos 11 casos** [não verificado]. [app/ponto/calculador/nucleo.py:235](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/calculador/nucleo.py:235), [app/ponto/turnos.py:285](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:285), [app/ponto/turnos.py:719](/home/ronald/codex_ws/f8f9bb34eaf0a632a8fbaf38d40ef538dda46e33/app/ponto/turnos.py:719).

A lista acima identifica escritores e leitores encontrados para os campos decisivos; **exaustividade de todos os escritores indiretos e históricos** [não verificado].

_7336 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
