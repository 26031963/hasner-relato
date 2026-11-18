# CODEX-ESTADO

> Ultima resposta processada: **18/11/2026 18:18** · publicado em **18/11/2026 18:22** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

## 18/11 18:06 · sessao dedab06910a2 · - · - · origem=Ronald (tmux)

Resultados enviados ao agente principal, com referências para o card, o modal do fio e as portas de rejeição, correção, conferência de data e reabertura.

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

_7086 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
