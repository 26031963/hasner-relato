# CODEX-ESTADO

> Ultima resposta processada: **10/11/2026 11:44** · publicado em **10/11/2026 11:47** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 10/11 11:44 · sessao 808a5b13142c · URGENCIA-E4B · sonda · origem=Code

**Entre os valores informados, há empate: badge lavrado e Validar+Decidir valem 1.224, ambos a 9 de 1.233.** A identidade dos conjuntos permanece **[nao verificado]**: a saída declara que a lavra não conserva IDs. O código lê o badge armazenado e define a soma corrente separadamente (`app/core/context_processors.py:84`; `app/chamados/services/verbos.py:25`).

**(1) Contadores no retrato fornecido de 10/11/2026**

Os números abaixo vêm de `NAVBAR_EXIBIDA` e `ATENDIMENTO_CONTEXTO_REAL`; as referências indicam os critérios no código, não uma nova medição.

| Contador | Valor | Distância de 1.233 | Referência |
|---|---:|---:|---|
| Badge lavrado, às 11:35:43 | **1.224** | **9** | `app/core/context_processors.py:84` |
| Central — total da lista | 3.616 | 2.383 | `app/chamados/services/painel.py:333` |
| Central — controle de ativos/cobrança | 3.601 | 2.368 | `app/chamados/services/painel.py:222` |
| Validar | 543 | 690 | `app/chamados/services/verbos.py:30` |
| Cobrar | 2.355 | 1.122 | `app/chamados/services/verbos.py:30` |
| Decidir | 681 | 552 | `app/chamados/services/verbos.py:30` |
| **Validar + Decidir corrente** | **1.224** | **9** | `app/chamados/services/verbos.py:25` |
| Justificativas pendentes | **[nao verificado]** | **[nao verificado]** | Passo de medição ausente; critério em `app/ponto/views.py:1736` |

O empate é entre os contadores **conhecidos**; a ausência da medição de Justificativas impede afirmar o vencedor entre todos os candidatos (`app/ponto/views.py:1736`; **[nao verificado]**, passo de Justificativas).

**(2) Partição por classe/verbo e dono**

**Badge lavrado:** partição por classe, verbo, atribuído, pilha, departamento e empresas 2/3/4 **[nao verificado]**, passo `NAVBAR_EXIBIDA/decomposicao_da_lavra`. O contexto gravado é agregado, sem mapa de IDs (`app/chamados/services/painel.py:284`).

**Validar+Decidir corrente:** a tabela fecha pelo filtro desses verbos nos blocos completos `QUEBRA_CLASSE_DONO/lista_empresa_None` e `lista_empresa_2`, apesar do truncamento posterior. Fonte de **todas as células**: saída desses blocos, referências declaradas `app/chamados/services/painel.py:198`; `app/chamados/services/verbos.py:30`; `app/chamados/utils.py:32`.

| Classe/origem | Departamento | Validar global | Decidir global | Validar empresa 2 | Decidir empresa 2 |
|---|---|---:|---:|---:|---:|
| acesso_bloqueado | ti | 0 | 3 | 0 | 2 |
| agenda_dia | supervisao | 0 | 2 | 0 | 2 |
| ausencia_colab | dp | 0 | 26 | 0 | 18 |
| batida_ausente | supervisao | 303 | 228 | 237 | 162 |
| batida_fora_horario | supervisao | 0 | 1 | 0 | 0 |
| chamados | dp | 0 | 32 | 0 | 21 |
| chamados | hasner | 0 | 10 | 0 | 8 |
| chamados | rh | 0 | 5 | 0 | 3 |
| chamados | supervisao | 0 | 105 | 0 | 79 |
| chamados | ti | 0 | 3 | 0 | 1 |
| cluster_batida_espuria | supervisao | 0 | 1 | 0 | 1 |
| colaborador_em_ferias | dp | 0 | 2 | 0 | 1 |
| colaborador_sem_posto | supervisao | 0 | 2 | 0 | 2 |
| disputa_supervisao | supervisao | 74 | 88 | 58 | 72 |
| disputa_supervisao_manual | supervisao | 16 | 6 | 12 | 4 |
| geofence | supervisao | 0 | 10 | 0 | 7 |
| gps_fora_geofence | supervisao | 123 | 0 | 81 | 0 |
| par_relampago | supervisao | 0 | 23 | 0 | 19 |
| ping_geo_fora | supervisao | 0 | 1 | 0 | 0 |
| sem_previsao_escala | supervisao | 3 | 69 | 1 | 57 |
| turno_aberto | supervisao | 24 | 55 | 15 | 38 |
| turno_aberto_24h | supervisao | 0 | 9 | 0 | 6 |
| **Total** | | **543** | **681** | **404** | **503** |
| **Validar+Decidir** | | **1.224** | | **907** | |

A partição por dono fecha assim; todas as linhas têm `atribuido_a_pk=null` nos mesmos blocos fornecidos. Fonte de toda a tabela: `app/chamados/services/painel.py:198`; `app/chamados/utils.py:32`.

| Verbo | Pilha | Departamento | Global | Empresa 2 |
|---|---|---|---:|---:|
| validar | admin_validar | supervisao | 543 | 404 |
| decidir | sem_pergunta | supervisao | 600 | 449 |
| decidir | sem_pergunta | dp | 60 | 40 |
| decidir | sem_pergunta | hasner | 10 | 8 |
| decidir | sem_pergunta | rh | 5 | 3 |
| decidir | sem_pergunta | ti | 6 | 3 |
| **Total** | | | **1.224** | **907** |

Empresas **3 e 4 individualmente: [nao verificado]**, passos `ATENDIMENTO_CONTEXTO_REAL`, `TOTAL_POPULACAO` e `QUEBRA_CLASSE_DONO` dessas empresas ausentes do trecho. A diferença global−empresa 2 não identifica sua distribuição entre elas **[nao verificado]**.

**(3) Censo `sem_porta` e cruzamento**

O recorte considera a competência corrente e a anterior ainda não entregue; a competência segue a janela da empresa. Portanto, o zero entregue abaixo vale para esse alcance, sem medir todo o histórico entregue (`app/ponto/services/he_pendente_lavrado.py:209`; `app/chamados/services/vigia_fio.py:838`).

Fonte de todas as contagens: `UNIVERSO_E4B` e `E4B_EXPORTACAO` fornecidos (`app/chamados/services/vigia_fio.py:628`; `app/chamados/services/vigia_fio.py:822`). Rótulos das classes: `app/chamados/services/vigia_fio.py:538`.

| Classe E4B | Rótulo | Corrente 11/2026, não entregue | Anterior 10/2026, não entregue | Entregue/exportada | Total |
|---|---|---:|---:|---:|---:|
| a | chamado_vivo_com_veu | 2 | 0 | 0 | 2 |
| b | respondida_sem_resposta | 0 | 0 | 0 | 0 |
| c | respondida_destino_fechado | 4 | 12 | 0 | 16 |
| d | colab_na_lista_cxr_dia_fora | 156 | 101 | 0 | 257 |
| e | outra | 47 | 5 | 0 | 52 |
| **Total** | | **209** | **118** | **0** | **327** |

Os cruzamentos confirmados são todos da competência **11/2026, não entregue**; suas parcelas anterior e entregue são zero. Fonte de todas as células: `TOTAL_POPULACAO`, `CENTRAL_DENTRO_E4B` e `QUEBRA_CLASSE_DONO` dos respectivos recortes (`app/chamados/services/painel.py:198`; `app/chamados/services/verbos.py:30`; `app/chamados/services/vigia_fio.py:542`).

| População | Total | Dentro E4B: a/b/c/d/e | Dentro E4B total | Fora do universo atual E4B | Vínculo indeterminado | Fora empresas 2/3/4 |
|---|---:|---|---:|---:|---:|---:|
| Central — lista global | 3.616 | 0 / 0 / 1 / 1 / 0 | **2** | 3.422 | 191 | 1 |
| Central — cobrança global | 3.601 | 0 / 0 / 1 / 1 / 0 | **2** | 3.408 | 190 | 1 |
| **Validar+Decidir global** | **1.224** | **0 / 0 / 1 / 0 / 0** | **1** | **1.044** | **179** | **0** |
| Central — lista empresa 2 | 2.808 | 0 / 0 / 1 / 1 / 0 | **2** | 2.668 | 138 | 0 |
| Central — cobrança empresa 2 | 2.795 | 0 / 0 / 1 / 1 / 0 | **2** | 2.655 | 138 | 0 |
| **Validar+Decidir empresa 2** | **907** | **0 / 0 / 1 / 0 / 0** | **1** | **777** | **129** | **0** |

O item de **Validar+Decidir** dentro do E4B é `disputa_supervisao`, verbo `validar`, pilha `admin_validar`, departamento `supervisao`, atribuído nulo, classe E4B **c**, empresa **2**, competência **11/2026**, não exportada. O outro item da lista está em `cobrar`, classe E4B **d** (`QUEBRA_CLASSE_DONO/lista_empresa_2`, fonte `app/chamados/services/painel.py:198`; `app/chamados/services/vigia_fio.py:542`).

| Fechamento restante | Resultado / passo |
|---|---|
| Badge lavrado × E4B, por classe/competência/empresa | **[nao verificado]** — `NAVBAR_EXIBIDA/decomposicao_da_lavra`; igualdade numérica não recupera IDs |
| Validar+Decidir empresas 3 e 4 × E4B | **[nao verificado]** — recortes individuais ausentes |
| Continuação da sonda / eventual cadeia `ERRO_SONDA` | **[nao verificado]** — trecho termina em `QUEBRA_CLASSE_DONO`, `admin_atual_empresa_2`, classe `par_relampago`, campo `dono_pilha` |

## 10/11 11:42 · sessao 72ef5087b3f5 · - · - · origem=Ronald (tmux)

O badge e Validar+Decidir têm **1.224**, ambos a **9** de 1.233 no dado fornecido; Justificativas pendentes permanece **[nao verificado]**, pois seu passo não consta antes do truncamento. O código define a soma em `app/chamados/services/verbos.py:22-27`; Justificativas conta `status='pendente'` global em `app/ponto/views.py:1736`.

O badge é lido da lavra em `app/core/context_processors.py:68-84`. Seu escritor usa `so_placar=True` e grava `n_admin` e contexto agregado (`app/core/management/commands/lavrar_badge_navbar.py:39-41`, `:72-93`); esse contexto não conserva mapa/ids (`app/chamados/services/painel.py:284-299`). Portanto, igualdade numérica não prova igualdade de conjuntos; a decomposição e o cruzamento E4B **do badge lavrado** continuam **[nao verificado]**, passo `NAVBAR_EXIBIDA/decomposicao_da_lavra`.

A quebra **corrente** de Validar+Decidir pode ser fechada integralmente para global e empresa 2 filtrando as quebras completas `lista_empresa_None` e `lista_empresa_2`, apesar do truncamento posterior de `admin_atual_empresa_2`. Referências dos registros fornecidos: `app/chamados/services/painel.py:198`; `app/chamados/services/verbos.py:30`.

| Classe | Validar global | Decidir global | Validar empresa 2 | Decidir empresa 2 |
|---|---:|---:|---:|---:|
| acesso_bloqueado | 0 | 3 | 0 | 2 |
| agenda_dia | 0 | 2 | 0 | 2 |
| ausencia_colab | 0 | 26 | 0 | 18 |
| batida_ausente | 303 | 228 | 237 | 162 |
| batida_fora_horario | 0 | 1 | 0 | 0 |
| chamados | 0 | 155 | 0 | 112 |
| cluster_batida_espuria | 0 | 1 | 0 | 1 |
| colaborador_em_ferias | 0 | 2 | 0 | 1 |
| colaborador_sem_posto | 0 | 2 | 0 | 2 |
| disputa_supervisao | 74 | 88 | 58 | 72 |
| disputa_supervisao_manual | 16 | 6 | 12 | 4 |
| geofence | 0 | 10 | 0 | 7 |
| gps_fora_geofence | 123 | 0 | 81 | 0 |
| par_relampago | 0 | 23 | 0 | 19 |
| ping_geo_fora | 0 | 1 | 0 | 0 |
| sem_previsao_escala | 3 | 69 | 1 | 57 |
| turno_aberto | 24 | 55 | 15 | 38 |
| turno_aberto_24h | 0 | 9 | 0 | 6 |
| **Total** | **543** | **681** | **404** | **503** |

Dono corrente, derivado dos mesmos registros com aquelas referências:

| Verbo | Pilha | Departamento | Global | Empresa 2 |
|---|---|---|---:|---:|
| validar | admin_validar | supervisao | 543 | 404 |
| decidir | sem_pergunta | supervisao | 600 | 449 |
| decidir | sem_pergunta | dp | 60 | 40 |
| decidir | sem_pergunta | hasner | 10 | 8 |
| decidir | sem_pergunta | rh | 5 | 3 |
| decidir | sem_pergunta | ti | 6 | 3 |
| **Total** | | | **1.224** | **907** |

Todas essas linhas têm `atribuido_a_pk=null` no dado; isso preserva os donos por pilha/departamento, não autoriza chamá-las “sem dono” (`app/chamados/services/painel.py:198`, fonte das `QUEBRA_CLASSE_DONO` fornecidas).

Empresas **3 e 4 individualmente**: **[nao verificado]**, passos `ATENDIMENTO_CONTEXTO_REAL`/`QUEBRA_CLASSE_DONO` dessas empresas ausentes do trecho. Não distribuir o residual global−empresa2 entre elas: o escopo informa empresa1 também visível, sem sua quebra individual.

## 10/11 11:42 · sessao bd5e46f79c29 · FIO-E0-V4-E4A-2 · construtor · origem=Code

Alteradas **nove asserções**, que são todas as enumeradas no pedido. Números preservados; nenhuma asserção removida. O diff é:

```diff
--- app/chamados/tests/test_colab_nao_vaza_fio_mudo.py
+++ app/chamados/tests/test_colab_nao_vaza_fio_mudo.py
@@ -49 +49 @@
-        self.assertIn('ALVOS: 1 perguntas em 1 colaboradores', s)
+        self.assertIn('ALVOS: 1 motivos devidos sem pergunta viva em 1 colaboradores', s)
@@ -54 +54 @@
-        self.assertIn('ALVOS: 2 perguntas em 2 colaboradores', s)
+        self.assertIn('ALVOS: 2 motivos devidos sem pergunta viva em 2 colaboradores', s)
--- app/chamados/tests/test_fabrica_pela_ata.py
+++ app/chamados/tests/test_fabrica_pela_ata.py
@@ -51 +51 @@
-        self.assertIn('ALVOS: 0 perguntas', saida)
+        self.assertIn('ALVOS: 0 motivos devidos sem pergunta viva', saida)
@@ -64 +64 @@
-        self.assertIn('ALVOS: 0 perguntas', saida)
+        self.assertIn('ALVOS: 0 motivos devidos sem pergunta viva', saida)
@@ -71 +71 @@
-        self.assertIn('ALVOS: 0 perguntas', saida)
+        self.assertIn('ALVOS: 0 motivos devidos sem pergunta viva', saida)
--- app/chamados/tests/test_pergunta_do_dia_em_curso.py
+++ app/chamados/tests/test_pergunta_do_dia_em_curso.py
@@ -106 +106 @@
-        self.assertIn('ALVOS: 0 perguntas', out.getvalue())
+        self.assertIn('ALVOS: 0 motivos devidos sem pergunta viva', out.getvalue())
--- app/chamados/tests/test_selo_emissor_fio_mudo.py
+++ app/chamados/tests/test_selo_emissor_fio_mudo.py
@@ -84 +84 @@
-        self.assertIn('ALVOS: 1 perguntas', saida)
+        self.assertIn('ALVOS: 1 motivos devidos sem pergunta viva', saida)
@@ -119 +119 @@
-        self.assertIn('ALVOS: 1 perguntas', saida)
+        self.assertIn('ALVOS: 1 motivos devidos sem pergunta viva', saida)
@@ -173 +173 @@
-        self.assertIn('ALVOS: 1 perguntas', saida)
+        self.assertIn('ALVOS: 1 motivos devidos sem pergunta viva', saida)
```

Conferência executada em `app/`:

```bash
rg -n -uu -g '!.git*' -g '!**/.git*' \
  -g '!**/__pycache__/**' -g '!*.pyc' -g '!.env*' \
  'ALVOS: [0-9]+ perguntas' .
```

Saída vazia; **rc 1**, sem ocorrências. `git diff --check` também retornou saída vazia, **rc 0**.

Quem escreve e quem lê:

- Escritor: [disparar_perguntas_competencia.py:215](chamados/management/commands/disparar_perguntas_competencia.py:215).
- Leitores alterados: os nove pontos identificados no diff.
- Outros leitores do rótulo atual: [test_e4a2_matriz_memoria.py:311](chamados/tests/test_e4a2_matriz_memoria.py:311) e [test_e4a2_disparar_e_vigia.py:222](chamados/tests/test_e4a2_disparar_e_vigia.py:222). As ocorrências antigas com `%d` pertencem a mutantes/base: `:80` do primeiro e `:36,642` do segundo.
- Encadeamento/log: [config/crons.py:1521](config/crons.py:1521), com `fio_mudo.log` em `:1522`.
- Raspador externo: `../bin/placar_code.sh:390–391` lê **FABRICA-POR-EVENTO** do log. Leitura de `ALVOS` em produção: **[nao verificado]**.
- Outro comando preservado: [retratar_chamados_lei_cala.py:50](chamados/management/commands/retratar_chamados_lei_cala.py:50).

A verificação **textual em memória**, usando o formato extraído do escritor `:215` e as nove asserções acima, apresentou RED com nove falhas antes da edição, GREEN depois e rejeitou 20 mutantes: nove rótulos antigos, nove contagens N incorretas e duas contagens M incorretas. Comando reproduzível para os três:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import ast, re, subprocess, unittest
from pathlib import Path

base = 'bd807a034ea73cf4bcd65dc02e22f45258ea26dc'
writer = Path('chamados/management/commands/disparar_perguntas_competencia.py')
fmt, = [n.value for n in ast.walk(ast.parse(writer.read_text()))
        if isinstance(n, ast.Constant) and isinstance(n.value, str)
        and n.value.startswith('ALVOS: ')]
case = unittest.TestCase()
red = green = killed = 0

for rel in subprocess.check_output(
        ['git', 'diff', '--name-only'], text=True).splitlines():
    assert rel.startswith('app/chamados/tests/')
    old = subprocess.check_output(['git', 'show', f'{base}:{rel}'], text=True)
    new = Path(rel.removeprefix('app/')).read_text()
    assert new == re.sub(
        r'(ALVOS: [0-9]+) perguntas',
        r'\1 motivos devidos sem pergunta viva', old)
    for source, phase in ((old, 'RED'), (new, 'GREEN')):
        for node in ast.walk(ast.parse(source)):
            if not (isinstance(node, ast.Call)
                    and isinstance(node.func, ast.Attribute)
                    and node.func.attr == 'assertIn' and node.args):
                continue
            expected = (ast.literal_eval(node.args[0])
                        if isinstance(node.args[0], ast.Constant) else None)
            if not isinstance(expected, str) or not expected.startswith('ALVOS: '):
                continue
            nums = list(map(int, re.findall(r'\d+', expected)))
            n, m = nums[0], nums[1] if len(nums) == 2 else 1
            if phase == 'RED':
                with case.assertRaises(AssertionError):
                    case.assertIn(expected, fmt % (n, m))
                red += 1
                continue
            case.assertIn(expected, fmt % (n, m))
            green += 1
            mutants = [
                fmt.replace('motivos devidos sem pergunta viva', 'perguntas')
                % (n, m),
                fmt % (n + 1, m),
            ]
            if len(nums) == 2:
                mutants.append(fmt % (n, m + 1))
            for mutant in mutants:
                with case.assertRaises(AssertionError):
                    case.assertIn(expected, mutant)
                killed += 1
assert (red, green, killed) == (9, 9, 20)
print(f'Contrato textual: RED={red}; GREEN={green}; mutantes rejeitados={killed}')
PY
```

Não rodei os testes Django nem a suite: execução comportamental **[nao verificado]**. Não houve certificação pelo Code ou pelo opus final **[nao verificado]**. [nao verificado]

## 10/11 11:41 · sessao 1f54129ccaf4 · - · - · origem=Ronald (tmux)

Pelo agregado fornecido `E4B_EXPORTACAO`, o recorte E4B fecha assim; “anterior” = total não exportado menos novembro/2026, pois as competências declaradas são somente 10/2026 e 11/2026 (`app/ponto/services/he_pendente_lavrado.py:209`; `app/chamados/services/vigia_fio.py:583`; fonte declarada do agregado: `app/chamados/services/vigia_fio.py:822`).

| Classe | Corrente 11/2026, não entregue | Anterior 10/2026, não entregue | Entregue/exportada | Total |
|---|---:|---:|---:|---:|
| a | 2 | 0 | 0 | 2 |
| b | 0 | 0 | 0 | 0 |
| c | 4 | 12 | 0 | 16 |
| d | 156 | 101 | 0 | 257 |
| e | 47 | 5 | 0 | 52 |
| **Total** | **209** | **118** | **0** | **327** |

O zero entregue vale para este recorte: `competencias_a_lavrar` seleciona a corrente e acrescenta a anterior somente se não entregue; não cobre todo o histórico exportado (`app/ponto/services/he_pendente_lavrado.py:220`; `app/ponto/services/he_pendente_lavrado.py:223`). A identificação da competência de cada dia usa a janela da empresa; a entrega usa `competencia_entregue`, portanto mês civil da data não substitui competência (`app/chamados/services/vigia_fio.py:822`; `app/chamados/services/vigia_fio.py:838`).

As classes são exclusivas pela ordem dos ramos:

| Classe | Critério | Fonte |
|---|---|---|
| a | Chamado vivo do dia ou âncora viva fora da fila, após exclusão das portas e baldes anteriores | `app/chamados/services/vigia_fio.py:663`, `:669` |
| b | Pergunta viva respondida sem texto | `app/chamados/services/vigia_fio.py:673` |
| c | Pergunta viva respondida, com chamado âncora terminal | `app/chamados/services/vigia_fio.py:675` |
| d | Sem perguntas vivas; dia fora da lista; colaborador presente na lista | `app/chamados/services/vigia_fio.py:677` |
| e | Restante, incluindo ausência de célula e detalhes do ramo residual | `app/chamados/services/vigia_fio.py:667`, `:679` |

O universo é de dias que seguram a folha nas empresas visíveis intersectadas com o filtro, deduplicados por colaborador e data; o `sem_porta` é registrado após excluir porta viva, hoje/futuro e os quatro baldes legais (`app/chamados/services/vigia_fio.py:567`; `:581`; `:642`; `:648`; `:654`; `:699`). `limite` nunca omite uma linha `sem_porta`; as 2.130 “outras rotas omitidas” não são complemento de E4B a adicionar aos 327 (`app/chamados/services/vigia_fio.py:628`).

Somando `E4B_CLASSE_DONO_COMPETENCIA` fornecido, a quebra por dono do dia também fecha em 327; fonte declarada inclui `app/ponto/management/commands/e6_oraculo.py:182` e `app/chamados/services/vigia_fio.py:542`.

| Dono do dia | Corrente | Anterior | Total |
|---|---:|---:|---:|
| BATIDA | 10 | 3 | 13 |
| CADASTRO | 38 | 6 | 44 |
| ESTRUTURA | 22 | 14 | 36 |
| `[nao verificado]` | 139 | 95 | 234 |
| **Total** | **209** | **118** | **327** |

Os 234 com dono `[nao verificado]` não autorizam substituição por ESTRUTURA: `InsumosDoDono.dono` pode devolver `None` no modo sem motor (`app/ponto/management/commands/e6_oraculo.py:189`; `:193`).

A quebra E4B por empresa não foi emitida nos agregados completos apresentados: `[nao verificado]`, passo `E4B_CLASSE_DONO_COMPETENCIA`, que não contém `empresa_pk`. A lista `CELULA_E4B` pode permitir contagem somente se sua completude for conferida contra 327; essa conferência não foi realizada neste subtrabalho `[nao verificado]`.

## 10/11 11:37 · sessao 112a44ad6ecd · URGENCIA-E4B · sonda · origem=Code

**O badge lavrado, 1.228, é o contador conhecido mais próximo de 1.233: diferença de 5.** A comparação com Justificativas pendentes permanece **[nao verificado]**, pois seu valor não aparece no trecho recebido. Fonte: saída `NAVBAR_EXIBIDA`, `app/core/context_processors.py:68`.

Esta resposta consolida a saída fornecida. Nova execução, role efetiva, transação READ ONLY e rollback: **[nao verificado]**. O dado termina durante um `ITEM_CENTRAL`; conclusão da execução, eventual `ERRO_SONDA`, passo e cadeia de locais: **[nao verificado]**.

**1. Contadores — retrato fornecido**

| Contador | Valor | Distância de 1.233 | Registro da saída → arquivo:linha |
|---|---:|---:|---|
| **Badge lavrado** | **1.228** | **5** | `NAVBAR_EXIBIDA` → `app/core/context_processors.py:68` |
| Central de Atendimento — total da lista | 3.606 | 2.373 | `ATENDIMENTO_CONTEXTO_REAL` → `app/chamados/services/painel.py:198` |
| Validar | 543 | 690 | `ATENDIMENTO_CONTEXTO_REAL` → `app/chamados/services/verbos.py:30` |
| Cobrar | 2.346 | 1.113 | Mesmo registro → `app/chamados/services/verbos.py:30` |
| Decidir | 680 | 553 | Mesmo registro → `app/chamados/services/verbos.py:30` |
| Soma Validar + Cobrar + Decidir | 3.569 | 2.336 | Cálculo sobre o mesmo registro → `app/chamados/services/verbos.py:30` |
| Controle atual do admin: Validar + Decidir | 1.223 | 10 | `BADGE_EXIBIDO_VS_ADMIN_ATUAL` → `app/chamados/services/verbos.py:25` |
| Justificativas pendentes | **[nao verificado]** | **[nao verificado]** | Contagem definida em `app/ponto/views.py:1736`; resultado ausente |

O badge foi lavrado às **11:15:46 de 10/11/2026**; a saída informa diferença de **5** para o controle atual. A equivalência dos conjuntos e a decomposição dos 1.228 itens são **[nao verificado]**. Fonte: `NAVBAR_EXIBIDA` e `BADGE_EXIBIDO_VS_ADMIN_ATUAL` → `app/core/context_processors.py:68`; `app/chamados/services/verbos.py:30`.

Os controles adicionais são **3.591 ativos**, compostos por **2.010 abertos + 1.581 em análise**. A conciliação item a item entre esses controles, a lista de 3.606 e as pilhas de 3.569 é **[nao verificado]**. Fonte: `ATENDIMENTO_CONTEXTO_REAL` → `app/chamados/services/painel.py:198`.

**2. Partição por empresa, classe/verbo e dono**

| Empresa | Classe de origem × verbo | Atribuído | Pilha | Departamento |
|---|---|---|---|---|
| 2 | [nao verificado] | [nao verificado] | [nao verificado] | [nao verificado] |
| 3 | [nao verificado] | [nao verificado] | [nao verificado] | [nao verificado] |
| 4 | [nao verificado] | [nao verificado] | [nao verificado] | [nao verificado] |

A listagem parcial não permite fechar essas partições sobre os **3.606 itens**, nem sobre os **1.223 atuais do admin**; a partição do **badge lavrado de 1.228** também é **[nao verificado]**.

O código distingue pilha e departamento: `admin_validar → validar`, `colab → cobrar`, `sem_pergunta/cadastro → decidir`, `registro → sem verbo`; o departamento tem outra função de classificação. Fontes: `app/chamados/catalogo/natureza.py:58`; `app/chamados/utils.py:32`.

**3. `censo_alcance`: somente rota `sem_porta`, empresas 2, 3 e 4**

| Classe | Significado | Corrente 11/2026, não entregue | Anterior 10/2026, não entregue | Entregue/exportada no recorte | Total | Registro → arquivo:linha |
|---|---|---:|---:|---:|---:|---|
| a | Chamado vivo com véu | 2 | 0 | 0 | 2 | `E4B_EXPORTACAO` → `app/chamados/services/vigia_fio.py:822` |
| b | Respondida sem resposta | 0 | 0 | 0 | 0 | Mesmo registro/fonte |
| c | Respondida com destino fechado | 4 | 12 | 0 | 16 | Mesmo registro/fonte |
| d | Colaborador na lista; dia fora | 161 | 101 | 0 | 262 | Mesmo registro/fonte |
| e | Outra | 47 | 5 | 0 | 52 | Mesmo registro/fonte |
| **Total** | | **214** | **118** | **0** | **332** | Soma de `E4B_EXPORTACAO`; `UNIVERSO_E4B` → `app/chamados/services/vigia_fio.py:628` |

Os rótulos das classes estão em `app/chamados/services/vigia_fio.py:538`. As colunas “anterior” foram calculadas como total menos corrente; o recorte informado contém apenas as competências **11/2026 e 10/2026**. Fonte: `UNIVERSO_E4B` → `app/chamados/services/vigia_fio.py:628`.

Esse censo cobre a competência corrente e a anterior enquanto não entregue; **não cobre todo o histórico exportado**. Portanto, o zero de entregues da tabela vale apenas para esse recorte. Fonte: `app/ponto/services/he_pendente_lavrado.py:209`; `app/chamados/services/vigia_fio.py:583`.

| Cruzamento solicitado | Quantidade |
|---|---|
| Itens da Central de Atendimento que caem nas 332 células sem porta | **[nao verificado]** |
| Itens de Validar/Decidir que caem nelas | **[nao verificado]** |
| Itens do badge lavrado que caem nelas | **[nao verificado]** |

O controle antigo informa **4 fios mortos**, mas representa outro censo; não substitui os **332 sem porta por alcançabilidade**. Fontes: `CONTROLE_CENSO_ANTIGO` → `app/chamados/services/vigia_fio.py:449`; `UNIVERSO_E4B` → `app/chamados/services/vigia_fio.py:628`.

## 10/11 11:36 · sessao c4bcf884eb66 · FIO-E0-V4-E4A-2 · construtor · origem=Code

A regra foi restaurada; o **item 5 permanece pendente**.

Arquivos tocados:

- [fabrica_da_celula.py:120](/home/ronald/wt-codex-fio-e0-v4-e4a-2-112744/app/chamados/services/fabrica_da_celula.py:120): não-acusador retorna `[]`; arquivo inteiro idêntico a `e13fa92e3`. Diff vazio, rc 0 ([prova](/home/ronald/wt-codex-fio-e0-v4-e4a-2-112744/app/RELATO_FIO_E0_V4_E4A_2_9e.txt:8)).
- [test_e4a2_matriz_memoria.py:201](/home/ronald/wt-codex-fio-e0-v4-e4a-2-112744/app/chamados/tests/test_e4a2_matriz_memoria.py:201): concorde exige zero; mutantes ajustados.
- [test_pergunta_do_dia_em_curso.py:442](/home/ronald/wt-codex-fio-e0-v4-e4a-2-112744/app/chamados/tests/test_pergunta_do_dia_em_curso.py:442): guarda usa `furo`, consulta diretamente o juiz e inclui caso próprio `concorde → 0`.
- [Relato completo](/home/ronald/wt-codex-fio-e0-v4-e4a-2-112744/app/RELATO_FIO_E0_V4_E4A_2_9e.txt:1): comandos, provas e limites.

Validação local: **RED reproduzido contra a 9d; 29 testes passaram; 19 mutantes rejeitados**, com comandos individuais no [relato:55](/home/ronald/wt-codex-fio-e0-v4-e4a-2-112744/app/RELATO_FIO_E0_V4_E4A_2_9e.txt:55). `_pks_de_empresa`, rótulo ALVOS e seus testes foram preservados ([prova:43](/home/ronald/wt-codex-fio-e0-v4-e4a-2-112744/app/RELATO_FIO_E0_V4_E4A_2_9e.txt:43)). [nao verificado]

Os 14 testes mantêm seus corpos intactos, mas **10 exigem o rótulo antigo**, incompatível com o item 2 ([evidências:109](/home/ronald/wt-codex-fio-e0-v4-e4a-2-112744/app/RELATO_FIO_E0_V4_E4A_2_9e.txt:109)). Seus resultados Django e Ruff limpo estão **[não verificado]**: ambos indisponíveis neste Python ([limites:194](/home/ronald/wt-codex-fio-e0-v4-e4a-2-112744/app/RELATO_FIO_E0_V4_E4A_2_9e.txt:194)). [nao verificado]

O censo de **quem escreve e quem lê cada campo** está no [relato:134](/home/ronald/wt-codex-fio-e0-v4-e4a-2-112744/app/RELATO_FIO_E0_V4_E4A_2_9e.txt:134).

## 10/11 11:24 · sessao ac3391eb6fde · FIO-E0-V4-E4A-2 · verificador · origem=Code

**A 9d não está pronta: há nove asserções incompatíveis com o novo rótulo `ALVOS` e uma divergência entre a fábrica e o leitor de pergunta viva que precisa ser resolvida.** (`app/chamados/management/commands/disparar_perguntas_competencia.py:215`; `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:49`; `app/chamados/services/fabrica_da_celula.py:123`; `app/chamados/juizes.py:1386`.)

1. **Chamadores e mudança de comportamento**

   O censo imediato de produção é:

   | Chamador/cadeia | Efeito da mudança |
   |---|---|
   | `perguntar_pela_celula → motivos_a_perguntar` | Pode criar pergunta antes bloqueada. (`app/chamados/services/fabrica_da_celula.py:210`; `app/chamados/services/fabrica_da_celula.py:222`.) |
   | `cartorio.julgar_colab → perguntar_pela_celula` | Leva a mudança ao julgamento real das células desta passada. (`app/ponto/services/cartorio.py:1006`; `app/ponto/services/cartorio.py:1008`.) |
   | `reconciliador → perguntar_por_eventos → perguntar_pela_celula` | Leva a mudança ao consumidor de eventos. O arquivo existente é `app/chamados/reconciliador.py`. (`app/chamados/reconciliador.py:1386`; `app/chamados/services/fabrica_da_celula.py:304`.) |
   | `motivos_que_o_evento_deixou → motivos_a_perguntar` | Muda a pendência contada pelo vigia da célula; não emite. (`app/chamados/services/fabrica_da_celula.py:245`; `app/chamados/management/commands/disparar_perguntas_competencia.py:185`.) |
   | `contador_fio_mudo → motivos_a_perguntar` | Muda os motivos contados; não emite. (`app/chamados/management/commands/disparar_perguntas_competencia.py:115`; `app/chamados/management/commands/disparar_perguntas_competencia.py:120`.) |

   **Sim, existe caminho que passa a criar pergunta:** veredito preenchido não acusador, colaborador ativo, dia permitido pelo juiz de trabalho, turno ainda em curso, marco vencido faltante na ata, sem barreira parcial e sem pergunta ocupando o fato. A alteração alcança os escritores compartilhados, não somente o contador. (`app/chamados/services/fabrica_da_celula.py:112`; `app/chamados/services/fabrica_da_celula.py:119`; `app/chamados/services/fabrica_da_celula.py:123`; `app/chamados/services/fabrica_da_celula.py:126`; `app/chamados/services/fabrica_da_celula.py:142`; `app/chamados/services/fabrica_da_celula.py:160`.)

   | Estado | Resultado da 9d |
   |---|---|
   | `nunca_bateu` | Continua retido antes da nova condição. (`app/chamados/services/fabrica_da_celula.py:105`.) |
   | Veredito acusador | Não passa pelo novo teste de encerramento. (`app/chamados/services/fabrica_da_celula.py:120`.) |
   | Não acusador + encerrado | Continua sem motivos. (`app/chamados/services/fabrica_da_celula.py:123`.) |
   | Não acusador + em curso + falta vencida | Pode devolver motivo e alcançar a emissão. (`app/chamados/services/fabrica_da_celula.py:123`; `app/chamados/services/fabrica_da_celula.py:222`.) |
   | Noturno de ontem ainda em curso | Recebe o mesmo tratamento; ontem civil não implica encerramento. (`app/chamados/services/fabrica_da_celula.py:123`; `app/ponto/turnos.py:2111`.) |
   | Marco batido, sem falta lida | Não gera motivo. (`app/chamados/services/fabrica_da_celula.py:64`; `app/chamados/tests/test_pergunta_do_dia_em_curso.py:485`.) |
   | Sem ata suficiente ou sem marco vencido | Não gera motivo. (`app/chamados/services/fabrica_da_celula.py:58`; `app/chamados/services/fabrica_da_celula.py:65`.) |

   A emissão permanece **com push silenciado**; a porta de push retorna quando esse contexto está ativo. (`app/chamados/services/fabrica_da_celula.py:221`; `app/chamados/services/disputa_emissao.py:1753`.)

   Incluir hoje e conservar o teto no juiz atende ao ALVO. Porém a condição abrange **todos** os não acusadores, inclusive `trabalhou`, `faltou` e `justificado`, sem distinguir `veredito_via='admin'`. O aval específico dessa extensão administrativa é [nao verificado]. (`aval_alvo_v9c_guarda_1110.txt:1`, anexo; `app/chamados/services/fabrica_da_celula.py:120`; `app/escala/models.py:1759`.)

2. **Predicado e autoridade de `dia_encerrado`**

   O predicado efetivo é:

   `sem turno aberto vivo AND fim previsto existe AND agora >= fim previsto`.

   Sem fim previsto, responde falso. Uma aberta pode continuar viva até a expiração definida pelo fim da janela do template mais quatro horas; portanto ultrapassar o fim previsto, sozinho, não basta. (`app/ponto/turnos.py:2000`; `app/ponto/turnos.py:2030`; `app/ponto/turnos.py:2115`; `app/ponto/turnos.py:2124`.)

   A fábrica passa **`cel.data`, dia do turno**, e o `agora` recebido. `dia_encerrado_motivo` lê `timezone.now()` somente se não receber `agora`; a própria fábrica tem seu fallback anterior em `timezone.localtime()`. (`app/chamados/services/fabrica_da_celula.py:116`; `app/chamados/services/fabrica_da_celula.py:123`; `app/ponto/turnos.py:2096`; `app/ponto/turnos.py:2098`.)

   **É autoridade registrada**, para “o dia está encerrado?”. Os cortes corrigidos permitem esse uso e proíbem substituí-lo por `data < hoje`. `par_completo` responde outra pergunta; não é substituto obrigatório aqui. (`app/core/juizes.py:235`; `app/core/juizes.py:236`; `app/docs/CORTES.md:55`; `app/docs/CORTES.md:62`; `CLAUDE.md:185`.)

   Há, entretanto, outro critério já consumido pelo fio: `lastro.marcos_pendentes` identifica o caráter provisório da ata e é lido pelo C6. Esse critério não equivale necessariamente a `dia_encerrado`; a consequência está no item 6. (`app/ponto/services/lastro.py:138`; `app/chamados/juizes.py:1385`.)

3. **Guarda, corpos congelados e asserções alteradas**

   A guarda continua exigindo **`FIO MUDO: 1` às 13:40 e nenhuma pergunta escrita**. No delta 9d fornecido, seu corpo não foi alterado. Comparação independente byte a byte dos demais “corpos congelados”, sem baseline identificado na cópia: [nao verificado]. (`app/chamados/tests/test_pergunta_do_dia_em_curso.py:460`.)

   As mudanças nas verificações existentes da 9d são:

   - Troca do texto esperado de `ALVOS` e exigência da menção à Pauta DP: correspondem ao P3 da triagem. (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:222`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:224`; `triagem_preverif8.txt:4`, anexo.)
   - Mensagens específicas, mínimo de catálogo, trecho mutado único e `assertRaisesRegex`: fortalecem a rejeição dos mutantes e atendem ao P1. (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:242`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:248`; `triagem_preverif8.txt:2`, anexo.)
   - O helper passa a exigir rejeição de `concorde` quando `encerrado=True`. O teste anterior conserva esse padrão e continua exigindo zero; os casos em curso são acrescentados separadamente. (`app/chamados/tests/test_e4a2_matriz_memoria.py:100`; `app/chamados/tests/test_e4a2_matriz_memoria.py:169`; `app/chamados/tests/test_e4a2_matriz_memoria.py:194`; `app/chamados/tests/test_e4a2_matriz_memoria.py:199`.)

   **Regressão certa:** nove asserções ficaram esperando `ALVOS: N perguntas`, incompatível com a saída atual `ALVOS: N motivos devidos...`:

   - `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:49`, `:54`;
   - `app/chamados/tests/test_selo_emissor_fio_mudo.py:84`, `:119`, `:173`;
   - `app/chamados/tests/test_fabrica_pela_ata.py:51`, `:64`, `:71`;
   - `app/chamados/tests/test_pergunta_do_dia_em_curso.py:106`.

   A saída conflitante está em `app/chamados/management/commands/disparar_perguntas_competencia.py:215`.

4. **`_pks_de_empresa`**

   Os dois chamadores de produção são `contador_fio_mudo` e `contador_nunca_bateu`. O disparar entra com `None`; o vigiar entra com `None` ou lista materializada de instâncias `Empresa`. Não há escalar acidental nesses caminhos. (`app/chamados/management/commands/disparar_perguntas_competencia.py:52`; `app/chamados/management/commands/disparar_perguntas_competencia.py:88`; `app/chamados/management/commands/disparar_perguntas_competencia.py:189`; `app/chamados/management/commands/vigiar_fio_morto.py:156`; `app/chamados/management/commands/vigiar_fio_morto.py:160`.)

   Nos testes há listas de pks/instâncias, tuplas, iteradores e `None`. Empresa única, inteiro e texto aparecem **deliberadamente como entradas negativas**, esperando `TypeError`. (`app/chamados/tests/test_e4a2_nunca_bateu.py:119`; `app/chamados/tests/test_e4a2_nunca_bateu.py:125`; `app/chamados/tests/test_e4a2_nunca_bateu.py:310`; `app/chamados/tests/test_e4a2_c5_memoria.py:89`; `app/chamados/tests/test_e4a2_matriz_memoria.py:214`.)

   Não há consumo duplo do gerador original: a primeira normalização produz lista, reutilizada no ORM e repassada à segunda normalização. Queryset é compatível com esse contrato de iteração; um chamador efetivo passando queryset é [nao verificado]. (`app/chamados/management/commands/disparar_perguntas_competencia.py:33`; `app/chamados/management/commands/disparar_perguntas_competencia.py:36`; `app/chamados/management/commands/disparar_perguntas_competencia.py:104`; `app/chamados/management/commands/disparar_perguntas_competencia.py:121`.)

5. **Matriz (a)–(d) e bancos 1–4**

   A estrutura do contador permanece:

   | Contrato | Realização |
   |---|---|
   | (a) Retido `nunca_bateu` | Separado dos alvos; número de pares e esperado pela interseção com C5. (`app/chamados/management/commands/disparar_perguntas_competencia.py:66`; `:111`.) |
   | (b) Motivo devido | Consulta o juiz e deduplica `(colab,dia,motivo)`; esperado zero. (`app/chamados/management/commands/disparar_perguntas_competencia.py:115`; `:116`; `:122`.) |
   | (c) Pergunta viva | Qualquer pergunta viva ligada exclui o chamado inteiro, conforme o descarte P2. (`app/chamados/management/commands/disparar_perguntas_competencia.py:109`; `triagem_preverif8.txt:3`, anexo.) |
   | (d) Tranca | Universo `VIVOS`, sem filtro paralelo para esconder chamado que a tranca não encerrou, conforme P4. (`app/chamados/management/commands/disparar_perguntas_competencia.py:99`; `triagem_preverif8.txt:5`, anexo.) |

   O corte até ontem pertence à comparação C5; **não foi introduzido no universo do fio mudo**. (`app/chamados/management/commands/disparar_perguntas_competencia.py:64`; `app/chamados/management/commands/disparar_perguntas_competencia.py:99`.)

   Bancos 1–4 continuam implementados: saída dos juízes por varredura e remoção de `--apply`; publicação dos dois contadores; leitura pelo pipeline; exceção real vazia com controles sintéticos no selo. (`app/config/crons.py:2032`; `app/chamados/management/commands/disparar_perguntas_competencia.py:132`; `app/chamados/management/commands/vigiar_fio_morto.py:233`; `app/core/management/commands/pipeline_placar.py:135`; `bin/tests/test_papel_prazo_nao_deriva.sh:305`; `bin/tests/test_papel_prazo_nao_deriva.sh:384`.)

   A regressão comprovada é na compatibilidade dos testes do **banco 5**. A errata declara a medição de produção `06h35 FIO MUDO 0, ALVOS 0`; sua reprodução ou certificação para a semântica nova da 9d é [nao verificado]. (`app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:49`; `aval_errata_b3_matriz_1110.txt:1`, anexo.)

6. **Quem escreve, quem lê e significado**

   | Campo/valor | Escritor/fonte e leitores relevantes |
   |---|---|
   | `veredito` | Porta `lavrar_veredito`; o cartório fornece o julgamento e a via administrativa prevalece. Leem fábrica, C6, reconciliador e C5. (`app/ponto/portas/celula.py:974`; `:1003`; `:1035`; `app/chamados/services/fabrica_da_celula.py:120`; `app/chamados/juizes.py:1372`; `app/chamados/reconciliador.py:1027`; `app/escala/services/cadastro_realidade.py:181`.) |
   | `ata` | Cartório monta e aplica o teto; porta persiste. Fábrica lê faltantes pela cadeia `_ata_do_dia → _marcos_faltantes → grade_da_celula`; C6 lê pendentes. (`app/ponto/services/cartorio.py:1811`; `app/ponto/portas/celula.py:1033`; `app/chamados/services/disputa_emissao.py:895`; `app/chamados/services/declaracao_texto.py:100`; `app/chamados/juizes.py:1385`.) |
   | `julgada_em` | `carimbar` grava o instante. O cartório captura o valor anterior e o entrega à fábrica para `parcial_so_lista`; contador/avulso usam o persistido. (`app/ponto/portas/celula.py:1065`; `app/ponto/services/cartorio.py:954`; `:1008`; `app/chamados/services/fabrica_da_celula.py:126`.) |
   | `ALVOS` | Disparar imprime quantidade de motivos e pessoas distintas. Os leitores incompatíveis encontrados são os nove testes do item 3. (`app/chamados/management/commands/disparar_perguntas_competencia.py:215`; `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:49`.) |
   | `fio_mudo_alvos` | Contador deriva; vigiar publica número/esperado; pipeline compara; `fio_estado` também compara dinamicamente o carimbo inteiro. (`app/chamados/management/commands/disparar_perguntas_competencia.py:122`; `app/chamados/management/commands/vigiar_fio_morto.py:233`; `:241`; `app/core/management/commands/pipeline_placar.py:98`; `bin/fio_estado.py:125`.) |
   | `nunca_bateu_sem_adesao` | Número e esperado são pares; totais C5 são informativos. No JSON, o nome aparece como número em `censo/esperado` e como objeto detalhado no topo; os leitores acessam esses níveis explicitamente. (`app/chamados/management/commands/disparar_perguntas_competencia.py:66`; `:69`; `app/chamados/management/commands/vigiar_fio_morto.py:234`; `:241`; `:242`.) |
   | `rc` | Disparar: 2 somente por `--alarme` e pendência da célula. Vigiar: pelos próprios gritos. Pipeline: por seus alarmes com chave. (`app/chamados/management/commands/disparar_perguntas_competencia.py:219`; `app/chamados/management/commands/vigiar_fio_morto.py:252`; `app/core/management/commands/pipeline_placar.py:160`.) |

   `cel.data` é dia do turno; `hoje` do carimbo é dia civil local da medição. O pipeline compara o dia civil solicitado com esse `hoje`, sem reinterpretá-lo como dia de cada alvo. (`app/ponto/turnos.py:2111`; `app/chamados/management/commands/vigiar_fio_morto.py:167`; `:230`; `app/core/management/commands/pipeline_placar.py:95`.)

   **Divergência de significado encontrada:** a fábrica admite não acusador quando `dia_encerrado=False`; o C6 mata a pergunta não respondida desse não acusador se não houver `pendente` na ata. Assim, com falta vencida, turno em curso e ata sem pendentes, a fábrica pode produzir motivo enquanto o leitor considera a pergunta morta. O reconciliador também considera absolvido o não acusador sem pendentes. Ocorrência desse estado na produção fornecida: [nao verificado]. (`app/chamados/services/fabrica_da_celula.py:123`; `:142`; `app/chamados/juizes.py:1386`; `app/chamados/reconciliador.py:1029`.)

7. **Mordida e vacuidade**

   Os catálogos principais agora exigem mínimos: **11 NuncaBateu, 12 placar, 10 relatório, 19 matriz e 13 C5**. Os laços verificam controle verde, trecho único e falha pela mensagem pretendida; há provas específicas para catálogo vazio/encolhido. (`app/chamados/tests/test_e4a2_nunca_bateu.py:66`; `:318`; `:329`; `app/core/tests/test_fio_mudo_placar.py:53`; `:294`; `:319`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:39`; `:235`; `:245`; `app/chamados/tests/test_e4a2_matriz_memoria.py:82`; `:352`; `:365`; `app/chamados/tests/test_e4a2_c5_memoria.py:71`; `:201`; `:214`.)

   O selo por AST dos catálogos Django prova sua defesa contra esvaziamento; **não executa os mutantes ORM nem seus `setUp`**. Os testes de memória do encerramento usam resposta dublada; a integração noturna está em teste Django separado. (`app/chamados/tests/test_e4a2_matriz_memoria.py:314`; `:325`; `:334`; `:151`; `app/chamados/tests/test_pergunta_do_dia_em_curso.py:297`.)

   Nesta auditoria, os dois módulos de memória executaram **28 testes OK**, e o selo shell terminou **OK, rc 0**. Comprovantes persistidos dessas execuções e execução da integração/suíte inteira: [nao verificado]. [nao verificado]

8. **Classes de reprovação**

   - **Segundo juiz registrado duplicado:** não identificado na nova chamada; ela usa a autoridade existente. (`app/core/juizes.py:235`; `app/chamados/services/fabrica_da_celula.py:123`.)
   - **Segundo escritor fora da porta:** não identificado no contador; a emissão segue pela fábrica existente. (`app/chamados/management/commands/disparar_perguntas_competencia.py:115`; `app/chamados/services/fabrica_da_celula.py:222`.)
   - **Novo relógio fora da borda nesse caminho:** não identificado; contador repassa `agora`, e a nova chamada o conserva. (`app/chamados/management/commands/disparar_perguntas_competencia.py:115`; `app/chamados/services/fabrica_da_celula.py:123`.)
   - **Selo vazio:** os mínimos corrigem o problema apontado; não certificam a suíte, que contém a incompatibilidade textual demonstrada. (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:242`; `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:49`.)
   - **Campo interpretado por critérios diferentes:** presente no caráter provisório do não acusador — encerramento do turno na fábrica versus pendência da ata no C6/reconciliador. (`app/chamados/services/fabrica_da_celula.py:123`; `app/chamados/juizes.py:1386`; `app/chamados/reconciliador.py:1029`.)
   - **Nova inversão de trava ou pouso antes do pré-requisito:** não demonstrados pelo material fornecido; execução de pouso, ordem dinâmica e comprovação operacional dos pré-requisitos: [nao verificado]. [nao verificado]

## 10/11 11:22 · sessao c72719cc1e36 · BO-0611-05 · censo · origem=Code

A classe excede o 403: há barreiras por GPS, selfie, situação, confirmação de turno e proximidade temporal. A própria L-030 já declara GPS, desligado/férias e tipo oposto como pendências; nesta cópia, a referência correta é `app/docs/LEIS.md:165`, além da lei em `CLAUDE.md:140`.

Execução da suíte, smoke nas duas cascas, comportamento do APK instalado e causalidade individual de marcação ausente de col456: **[nao verificado]**. [nao verificado]

**1. Caminhos e barreiras**

As portas de batida de chão encontradas por AST são:

| Porta | Caminho até o escritor |
|---|---|
| API do kernel | `/api/ponto/bater/` → `views_core.api_bater_ponto` → `registrar_batida`. `app/api/urls_core.py:14`; `app/api/views_core.py:681`. |
| API da outra casca | Mesma URL → `views.api_bater_ponto` → `registrar_batida`. `app/api/urls.py:67`; `app/api/views.py:684`. |
| Web clássico e V2 | `/ponto/bater/` e `/ponto/bater-v2/` → mesma view → `ramo_registrar_es` → escritor; o JS da V2 envia diretamente à API. `app/ponto/urls.py:11`; `app/ponto/urls.py:12`; `app/ponto/views.py:349`; `app/ponto/services/triagem_batida.py:701`; `app/templates/ponto/bater_ponto_v2.html:335`. |
| Offline antigo | Autenticado recebe **410**, `codigo=gone`; não chama escritor. O PWA atual sincroniza pela API. `app/ponto/views.py:417`; `app/ponto/views.py:426`; `app/static/pwa.js:153`. |

V2, legado `bater_ponto.html` e base da triagem herdam **literalmente `base.html`**. O processor fornece `base_tpl`, mas essas heranças não o usam; renderização dessas telas sob `base_app.html`: **[nao verificado]**. Ambas as bases carregam `pwa.js` e o diálogo. `app/templates/ponto/bater_ponto_v2.html:1`; `app/templates/ponto/bater_ponto.html:1`; `app/templates/ponto/triagem/base_triagem.html:1`; `app/core/context_processors.py:210`; `app/templates/base.html:20`; `app/templates/base.html:388`; `app/templates/base_app.html:20`; `app/templates/base_app.html:145`.

O clássico seleciona os templates de triagem, normalmente `triagem/apto.html`; `bater_ponto.html` não é escolhido pelos retornos dessa view. `app/ponto/views.py:225`; `app/ponto/views.py:235`; `app/ponto/views.py:356`; `app/ponto/views.py:358`.

Barreiras da API e do escritor:

| Condição e localização | Resposta / gravação | L-030 e teste que exige hoje | [nao verificado]
|---|---|---|
| `not latitude or not longitude`, incluindo zero numérico: `app/api/views_core.py:440`; `app/api/views.py:449`. | **403**, `gps_obrigatorio`, “Sua batida NAO sera registrada sem o GPS ativado”; auditoria de recusa; não chega ao escritor. `app/api/views_core.py:442`; `app/api/views_core.py:454`; `app/api/views.py:462`. | **Sim, explicitamente.** Exige 403/código: `app/api/tests/test_gps_obrigatorio.py:21`; `app/api/tests/test_gps_obrigatorio.py:22`. Exige zero gravação: `app/api/tests/test_gps_obrigatorio.py:38`. Exige auditoria: `app/api/tests/test_gps_obrigatorio.py:28`. Outro teste: `app/ponto/tests/test_batida_sideeffects.py:130`. |
| Situação fora de `ativo/afastado`: `app/api/views_core.py:402`; `app/api/views.py:415`. | **403**: desligado → `colab_desligado`; férias → `colab_ferias`; demais → `colab_situacao_invalida`. `app/api/views_core.py:403`; `app/api/views_core.py:410`; `app/api/views.py:423`. | **Sim, explicitamente para desligado/férias.** Desligado403: `app/api/tests/test_api_bater_ponto.py:65`; selo literal nos dois kernels: `app/ponto/tests/test_afastado_nunca_bloqueia.py:25`; `app/ponto/tests/test_afastado_nunca_bloqueia.py:36`. Férias: `app/api/tests/test_api_bater_ponto.py:88`; `app/ponto/tests/test_afastado_na_batida.py:115`. Teste para demais valores: **[nao verificado]**. |
| Mesmo tipo em **±5 minutos**, antes do escritor: `app/api/views_core.py:549`; `app/api/views.py:557`. | **200**, `ja_registrada`, ID anterior, `sucesso=True`; nenhuma nova linha, mesmo com instante diferente. `app/api/views_core.py:555`; `app/api/views_core.py:558`. | **É supressão temporal, distinta da duplicata exata.** Teste exige colapsar instantes separados por um minuto: `app/api/tests/test_bug127_dedup_unico.py:121`; `app/api/tests/test_bug127_dedup_unico.py:124`. Pela literalidade de `CLAUDE.md:140`, precisa entrar na cura da classe para fatos distintos. |
| Mesmo tipo em **±60 segundos**, dentro do escritor, excluindo retratadas: `app/ponto/registro_batida.py:106`; `app/ponto/registro_batida.py:109`; `app/ponto/registro_batida.py:113`. | Retorna **`(existente, None)`**, sem criar. `app/ponto/registro_batida.py:115`. | **Mesma distinção:** não exige chave exata. Teste exige colapso a +20s: `app/ponto/tests/test_registrar_batida.py:72`; `app/ponto/tests/test_registrar_batida.py:76`; `app/ponto/tests/test_registrar_batida.py:78`. |
| Mesma chave **colaborador+timestamp**, inclusive outro tipo ou retratada: `app/ponto/registro_batida.py:120`. | Retorna **`(existente, None)`**. `app/ponto/registro_batida.py:122`. | **Não é recusa de fato novo: idempotência exata.** Constraint: `app/ponto/models.py:106`. Testes: `app/ponto/tests/test_bater_mesma_chave_idempotente.py:48`; `app/ponto/tests/test_bater_mesma_chave_idempotente.py:57`. |
| Tipo oposto próximo, sem casar marco próprio, `autoridade_humana=False`: `app/ponto/registro_batida.py:145`; `app/ponto/registro_batida.py:150`. | **`(None, conflito)`**, sem gravar; APIs respondem **200**, `ignorada_intervalo`, `sucesso=True`, ID da oposta e “Aguarde…”. `app/ponto/registro_batida.py:151`; `app/api/views_core.py:709`; `app/api/views.py:712`. | **Sim, explicitamente.** API: `app/api/tests/test_api_bater_ponto.py:639`; `app/api/tests/test_api_bater_ponto.py:642`. Com template, +120s → None: `app/ponto/tests/test_dedup_envelope_template.py:43`; `app/ponto/tests/test_dedup_envelope_template.py:46`. |
| Competência fechada, **somente `not fato_de_chao`**: `app/ponto/registro_batida.py:132`. | **`(None, mensagem)`**. `app/ponto/registro_batida.py:137`. | **Não é barreira de chão.** As três portas passam `autoridade_humana=False`; o padrão deriva `fato_de_chao=True`. `app/api/views_core.py:683`; `app/api/views.py:686`; `app/ponto/services/triagem_batida.py:703`; `app/ponto/registro_batida.py:130`. Testes preservam chão e recusam materialização institucional: `app/ponto/tests/test_ajuste_retroativo_folha_fechada.py:40`; `app/ponto/tests/test_ajuste_retroativo_folha_fechada.py:61`. |

A janela oposta é **300s** com escala vigente/tipo/marcos; **60s** nos demais casos. Os limites da consulta são inclusivos, apesar da mensagem usar `<`. A exceção aceita marco do próprio tipo a **≤5 minutos**, no dia ou véspera. `app/ponto/registro_batida.py:30`; `app/ponto/registro_batida.py:32`; `app/ponto/registro_batida.py:142`; `app/ponto/registro_batida.py:143`; `app/ponto/registro_batida.py:147`; `app/ponto/registro_batida.py:42`; `app/ponto/registro_batida.py:48`.

Barreiras no web, templates e JS:

| Condição e localização | Resposta / efeito | Classe e teste exigindo | [nao verificado]
|---|---|---|
| V2 sem selfie após captura: `app/templates/ponto/bater_ponto_v2.html:287`. | Toast para habilitar câmera/tirar selfie, `false`; clique retorna antes de POST/fila. `app/templates/ponto/bater_ponto_v2.html:289`; `app/templates/ponto/bater_ponto_v2.html:290`; `app/templates/ponto/bater_ponto_v2.html:325`. | **Mesma classe pela lei:** prova ausente impede fato. `CLAUDE.md:140`. Teste exigindo: **[nao verificado]**. |
| V2 sem latitude ou longitude após novas tentativas: `app/templates/ponto/bater_ponto_v2.html:292`; `app/templates/ponto/bater_ponto_v2.html:297`. | Toast localização/Justificativa, `false`; não envia. `app/templates/ponto/bater_ponto_v2.html:299`; `app/templates/ponto/bater_ponto_v2.html:300`; `app/templates/ponto/bater_ponto_v2.html:325`. | **Sim.** `app/docs/LEIS.md:165`. Teste específico do template: **[nao verificado]**. |
| V2: fetch lança exceção e coordenada nula. `app/templates/ponto/bater_ponto_v2.html:349`; `app/templates/ponto/bater_ponto_v2.html:350`. | Toast “Nao foi possivel bater agora”; retorna antes da fila. `app/templates/ponto/bater_ponto_v2.html:352`; `app/templates/ponto/bater_ponto_v2.html:353`. | **Sim**, guarda adicional já antecedida pelo requisito acima. `CLAUDE.md:140`; `app/templates/ponto/bater_ponto_v2.html:325`. Teste: **[nao verificado]**. |
| V2 offline e coordenada nula. `app/templates/ponto/bater_ponto_v2.html:368`. | Toast GPS/Justificativa; retorna antes da fila. `app/templates/ponto/bater_ponto_v2.html:370`; `app/templates/ponto/bater_ponto_v2.html:371`. | **Sim.** `CLAUDE.md:140`. Teste: **[nao verificado]**. |
| V2 recebe HTTP não-2xx. `app/templates/ponto/bater_ponto_v2.html:340`. | GPS403 ganha toast específico; demais erros mostram mensagem/HTTP e retornam **sem enfileirar**. `app/templates/ponto/bater_ponto_v2.html:342`; `app/templates/ponto/bater_ponto_v2.html:345`; `app/templates/ponto/bater_ponto_v2.html:347`; `app/templates/ponto/bater_ponto_v2.html:348`. | É tratamento que encerra a tentativa recusada; a causa é a guarda do servidor correspondente. Teste exigindo ausência de fila: **[nao verificado]**. |
| Triagem: botões inicialmente ocultos; câmera bem-sucedida exige confirmar selfie para exibi-los; refazer oculta novamente. `app/templates/ponto/triagem/apto.html:93`; `app/templates/ponto/triagem/apto.html:182`; `app/templates/ponto/triagem/apto.html:185`; `app/templates/ponto/triagem/apto.html:191`. | Impede acesso ao botão enquanto aguarda a etapa; câmera falhando **libera** sem selfie. `app/templates/ponto/triagem/apto.html:156`; `app/templates/ponto/triagem/apto.html:163`. | Condicionamento por prova pertence à classe de `CLAUDE.md:140`. Teste exigindo: **[nao verificado]**. |
| Triagem offline: câmera falha. `app/templates/ponto/triagem/apto.html:253`. | “Camera indisponivel — contate o supervisor”; esconde captura, sem fila nesse ramo. `app/templates/ponto/triagem/apto.html:254`; `app/templates/ponto/triagem/apto.html:255`. | **Mesma classe**, selfie impedindo fato. `CLAUDE.md:140`. Teste: **[nao verificado]**. |
| Triagem offline: sem GPS, primeiro aviso da sessão e confirmação falsa. `app/templates/ponto/triagem/apto.html:277`; `app/templates/ponto/triagem/apto.html:278`; `app/templates/ponto/triagem/apto.html:281`. | Retorna antes de `registrarBatidaOffline`. `app/templates/ponto/triagem/apto.html:284`; `app/templates/ponto/triagem/apto.html:289`. | Confirmação adicional condicionada à falta de geo; mesma classe enquanto impede envio. `CLAUDE.md:140`. Teste: **[nao verificado]**. |
| POST web E com turno aberto, sem `confirmar`, após não entrar no ramo anterior de órfão. `app/ponto/views.py:275`; `app/ponto/views.py:316`. | Guarda GPS/foto em sessão e devolve **HTML 200**, sem escritor. `app/ponto/views.py:319`; `app/ponto/views.py:329`; `app/ponto/views.py:330`. | **Mesma classe por condição de jornada.** `CLAUDE.md:140`. Teste exige nenhuma linha nova e tela de confirmação: `app/ponto/tests/test_f7_ramos_frios.py:128`; `app/ponto/tests/test_f7_ramos_frios.py:141`; `app/ponto/tests/test_f7_ramos_frios.py:142`. |
| POST web mesmo tipo nos **5 minutos anteriores**. `app/ponto/services/triagem_batida.py:662`; `app/ponto/services/triagem_batida.py:663`. | **200**, JSON `ja_registrada`, sem escritor. `app/ponto/services/triagem_batida.py:669`; `app/ponto/services/triagem_batida.py:670`. | Supressão temporal, distinta da chave exata; alcançada pela literalidade de `CLAUDE.md:140`. Teste específico dessa janela web: **[nao verificado]**. |
| Escritor devolve None por oposta no web. `app/ponto/services/triagem_batida.py:714`. | **200**, `ignorada_intervalo`, “Aguarde antes de bater de novo”. A busca de resposta usa ±60s mesmo quando o escritor usou 300s. `app/ponto/services/triagem_batida.py:717`; `app/ponto/services/triagem_batida.py:720`; `app/ponto/services/triagem_batida.py:724`. | **Sim.** Testes: `app/ponto/tests/test_bater_colisao_oposto.py:42`; `app/ponto/tests/test_f9d_t1_caracterizacao_ramo_quente.py:115`; `app/ponto/tests/test_f9d_t1_caracterizacao_ramo_quente.py:120`. |
| `bloqueio_horario` recuperado da sessão. `app/ponto/views.py:263`; `app/ponto/views.py:266`. | Clássico seleciona tela de justificativa; V2 não desenha botão e manda “Abrir ponto”. `app/ponto/views.py:231`; `app/templates/ponto/triagem/fora_horario.html:20`; `app/templates/ponto/bater_ponto_v2.html:64`; `app/templates/ponto/bater_ponto_v2.html:71`. | **Barreira de acesso ao próximo registro**, embora a avaliação de horário grave o fato corrente. `app/ponto/services/triagem_batida.py:486`; `app/ponto/services/triagem_batida.py:487`; `app/ponto/services/triagem_batida.py:701`. Teste exigindo esse impedimento: **[nao verificado]**. |

`hxConfirmar(..., naoBloquear=true)` continua cancelável por Não, clique fora e Escape; o parâmetro apenas permite continuar se o markup não montar. O selo exige esse fail-open, sem exigir recusa por GPS. `app/templates/core/_dialogo.html:157`; `app/templates/core/_dialogo.html:161`; `app/templates/core/_dialogo.html:184`; `app/core/tests/test_selo_modal_nao_nasce_vazio.py:135`; `app/core/tests/test_selo_modal_nao_nasce_vazio.py:141`.

O legado também oculta botões até selfie/refazer; seu uso por uma rota atual é **[nao verificado]**. Seu envio online aceita ausência de GPS e falha HTTP/rede cai para fila; offline também passa coordenadas nulas à fila. `app/templates/ponto/bater_ponto.html:246`; `app/templates/ponto/bater_ponto.html:473`; `app/templates/ponto/bater_ponto.html:509`; `app/templates/ponto/bater_ponto.html:540`; `app/templates/ponto/bater_ponto.html:543`; `app/templates/ponto/bater_ponto.html:597`.

Há ainda barreiras de acesso e de contrato:

| Condição | Resposta e exigência atual |
|---|---|
| Onboarding incompleto | Redirect antes de GET/POST web. É cadastro condicionando acesso, não autenticação. `app/ponto/views.py:243`; `app/ponto/views.py:244`. Teste específico: **[nao verificado]**. |
| Comunicado pendente | **Só GET** redireciona para ciência; POST não entra no ramo. Teste exige 302. `app/ponto/views.py:251`; `app/ponto/views.py:255`; `app/comunicados/tests.py:181`; `app/comunicados/tests.py:186`. |
| Plataforma: carimbo `apk`; admin em mobile; navegador Android | **403** com interstitial. `/api/` é isenta; as views web de bater não declaram a isenção. `app/core/middleware.py:247`; `app/core/middleware.py:268`; `app/core/middleware.py:270`; `app/core/middleware.py:283`; `app/core/middleware.py:287`; `app/ponto/views.py:241`. Testes gerais exigem esses gates, sem testar especificamente POST de batida: `app/core/tests/test_plataforma_gate.py:31`; `app/core/tests/test_plataforma_gate.py:49`; `app/core/tests/test_plataforma_gate.py:66`. |
| Sem autenticação / credencial rejeitada | API exige `IsAuthenticated`; web exige login; JWT rejeita credencial trocada. `app/api/views_core.py:386`; `app/api/views.py:399`; `app/ponto/views.py:241`; `app/api/credencial.py:189`; `app/api/credencial.py:198`. Testes do mecanismo exigem401: `app/api/tests/test_401_diz_qual.py:45`; `app/api/tests/test_401_diz_qual.py:80`. |
| Sem colaborador vinculado | API **404**, “Colaborador não encontrado”; templates não oferecem botão. `app/api/views_core.py:391`; `app/api/views.py:404`; `app/templates/ponto/bater_ponto_v2.html:39`; `app/templates/ponto/triagem/base_triagem.html:20`. Teste específico: **[nao verificado]**. |
| Tipo fora de E/S | API **400**, “Tipo inválido”; web só encaminha E/S ao ramo de registro. `app/api/views_core.py:425`; `app/api/views_core.py:426`; `app/api/views.py:438`; `app/ponto/views.py:346`. Testes exigem400: `app/api/tests/test_api_bater_ponto.py:121`; `app/api/tests/test_api_bater_ponto.py:127`. |

Login/refresh têm guardas próprias, anteriores ao POST de batida: situação diferente de ativo é recusada inclusive para afastado; empresa inativa também é recusada. Login responde403; refresh401. Isso não é uma guarda geo, e não equivale apenas a validar senha/token. `app/api/login_helpers.py:141`; `app/api/login_helpers.py:143`; `app/api/login_helpers.py:38`; `app/api/login_helpers.py:59`; `app/api/views_core.py:173`; `app/api/views_core.py:179`; `app/api/views.py:116`; `app/api/views.py:122`. Testes exigem recusa por situação/empresa: `app/api/tests/test_api_login.py:137`; `app/api/tests/test_api_login.py:151`; `app/api/tests/test_api_login.py:243`; `app/api/tests/test_api_login.py:252`. Participação de renovação numa tentativa concreta do dado fornecido: **[nao verificado]**.

Na fila e service worker:

- A fila aceita lat/lng nulos; sync retorna sem enviar quando offline ou sem meta `user-perfil=colaborador`. Ambas as bases emitem essa meta apenas para não-staff. `app/static/pwa.js:107`; `app/static/pwa.js:116`; `app/static/pwa.js:127`; `app/static/pwa.js:128`; `app/templates/base.html:16`; `app/templates/base_app.html:16`.
- Sync omite coordenadas nulas, envia à API e apaga item em **2xx ou400**. Portanto também apaga respostas `ignorada_intervalo`/`ja_registrada` recebidas como200. Outros erros permanecem para nova sincronização. `app/static/pwa.js:145`; `app/static/pwa.js:153`; `app/static/pwa.js:158`; `app/static/pwa.js:162`; `app/static/pwa.js:166`; `app/static/pwa.js:172`; `app/api/views_core.py:558`; `app/api/views_core.py:709`. Teste exigindo esses descartes da fila: **[nao verificado]**.
- O SW **não intercepta POST**. Background sync delega à página; sem página apenas notifica para abrir o app. `app/static/sw.js:35`; `app/static/sw.js:36`; `app/static/sw.js:117`; `app/static/sw.js:119`; `app/static/sw.js:121`.
- `_enviando`/`_confirmandoOffline` impedem clique concorrente; erro de IndexedDB rejeita a gravação local. São controle de operação/falha técnica, não julgamento de geo/jornada. `app/templates/ponto/bater_ponto_v2.html:321`; `app/templates/ponto/triagem/apto.html:262`; `app/static/pwa.js:121`.

Não há barreira de raio, precisão ou `tolerante_offline` nesses caminhos:

- V2: raio apenas badge/mapa; precisão >200m apenas aviso. `app/templates/ponto/bater_ponto_v2.html:214`; `app/templates/ponto/bater_ponto_v2.html:220`; `app/templates/ponto/bater_ponto_v2.html:228`.
- API/web verificam cerca **depois** do escritor; testes exigem200 fora da cerca, inclusive sem accuracy. `app/api/views_core.py:811`; `app/api/views.py:811`; `app/ponto/services/triagem_batida.py:752`; `app/ponto/tests/test_batida_sideeffects.py:106`; `app/ponto/tests/test_batida_sideeffects.py:152`.
- `tolerante_offline` existe no Posto, mas perdeu o leitor e está somente leitura no admin; não é parâmetro desses handlers. `app/colaboradores/models.py:225`; `app/core/configuracao_efeito.py:123`; `app/colaboradores/admin.py:44`.
- Mock, assinatura inválida, liveness, falta de foto na API, sem posto/escala e órfão são sinais, não retornos de recusa nesses ramos. `app/api/views_core.py:471`; `app/api/views_core.py:480`; `app/api/views_core.py:497`; `app/api/views_core.py:504`; `app/ponto/services/triagem_batida.py:123`; `app/ponto/views.py:312`. Afastado200 e mock200 são exigidos por testes. `app/api/tests/test_api_bater_ponto.py:75`; `app/api/tests/test_api_bater_ponto.py:159`.

**2. Como “sem geo” já é representado**

O sinal existente é **`Batida.latitude`/`longitude` nulos**. Não há flag própria `sem_geo` em Batida; `gps_accuracy` nulo significa precisão ausente, não necessariamente posição ausente. `app/ponto/models.py:8`; `app/ponto/models.py:58`; `app/ponto/models.py:59`; `app/ponto/models.py:60`.

O escritor sanitiza `''`, `None` e valores não numéricos para `None`, por campo, antes do create. As três portas já entregam esses campos ao escritor. `app/ponto/registro_batida.py:3`; `app/ponto/registro_batida.py:11`; `app/ponto/registro_batida.py:16`; `app/ponto/registro_batida.py:105`; `app/api/views_core.py:687`; `app/api/views.py:690`; `app/ponto/services/triagem_batida.py:707`.

Existe também o sinal secundário **`AlertaGeofence.tipo='sem_gps'`**. Após gravar, `verificar_geofence` reconhece coordenada ausente e, **havendo posto**, cria alerta com coordenadas nulas; depois abre/anexa ocorrência `gps_ausente` e tenta avisar supervisão. Sem posto,

[truncado em 20000 de 29209 caracteres]

## 10/11 11:20 · sessao e4e84d07c69f · - · - · origem=Ronald (tmux)

1. **Chamadores e efeito da 9d.** As três chamadas diretas de produção a `motivos_a_perguntar` são `perguntar_pela_celula`, `motivos_que_o_evento_deixou` e `contador_fio_mudo`; o cartório chama a primeira; o consumidor do reconciliador chama `perguntar_por_eventos`, que chama a primeira. O caminho informado como `chamados/services/reconciliador.py` corresponde nesta cópia a `app/chamados/reconciliador.py`. Referências: `app/chamados/services/fabrica_da_celula.py:210`, `app/chamados/services/fabrica_da_celula.py:245`, `app/chamados/management/commands/disparar_perguntas_competencia.py:115`, `app/ponto/services/cartorio.py:1008`, `app/chamados/reconciliador.py:1386`, `app/chamados/services/fabrica_da_celula.py:304`.

   A mudança alcança **escritores reais**, não apenas o contador: um veredito preenchido fora de `VEREDITOS_ACUSAM` deixa de bloquear incondicionalmente e bloqueia somente quando `dia_encerrado=True`. Exige ainda colaborador ativo, dia não futuro, dia não dispensado, vencimento do marco, falta lida da ata, ausência de barreira parcial e ausência de pergunta que ocupe o fato. Referências: `app/chamados/services/fabrica_da_celula.py:105`, `app/chamados/services/fabrica_da_celula.py:112`, `app/chamados/services/fabrica_da_celula.py:119`, `app/chamados/services/fabrica_da_celula.py:123`, `app/chamados/services/fabrica_da_celula.py:126`, `app/chamados/services/fabrica_da_celula.py:142`, `app/chamados/services/fabrica_da_celula.py:160`.

   Portanto existe caminho executável em que passa a nascer pergunta antes bloqueada: não acusador + dia em curso + marco vencido faltante. A regra também vale para ontem noturno enquanto o dia do turno não encerrou. Se a ata não apresentar falta vencida, não nasce motivo. Nunca-bateu continua bloqueado antes desse ramo. Referências: `app/chamados/services/fabrica_da_celula.py:105`, `app/chamados/services/fabrica_da_celula.py:119`, `app/chamados/services/fabrica_da_celula.py:123`, `app/chamados/services/fabrica_da_celula.py:58`, `app/chamados/services/fabrica_da_celula.py:64`, `app/chamados/services/fabrica_da_celula.py:69`.

   Esse alargamento cobre todos os vereditos preenchidos não acusadores, inclusive os administrativos que não estão na tupla acusadora; não está limitado a `concorde`. Referências: `app/chamados/services/fabrica_da_celula.py:120`, `app/escala/models.py:1759`, `app/escala/models.py:1767`.

   A emissão pela fábrica permanece sob silêncio de push: `perguntar` roda dentro de `push_disputa_silenciado`, e o emissor de push retorna imediatamente quando o contexto está ativo. Referências: `app/chamados/services/fabrica_da_celula.py:220`, `app/chamados/services/disputa_emissao.py:1742`, `app/chamados/services/disputa_emissao.py:1753`.

   A coleta de carimbadas pode agora ser alcançada nesses estados; seu desfazimento ocorre fora do bloco de silêncio, mas recebe `reabrir_fechada=False`, relê somente disputa aberta e executa `descarimbar`; os receivers de Pergunta observados agendam fechamento de beco e outbox, sem envio de push nesses corpos. Referências: `app/chamados/services/fabrica_da_celula.py:159`, `app/chamados/services/fabrica_da_celula.py:211`, `app/chamados/services/relavra_via.py:238`, `app/chamados/services/relavra_via.py:246`, `app/chamados/models.py:3203`, `app/chamados/signals.py:227`, `app/chamados/signals.py:250`.

2. **`dia_encerrado` é a autoridade registrada, não um segundo juiz introduzido pela 9d.** Registro explícito: pergunta “o dia está encerrado?” → `ponto/turnos.py::dia_encerrado`. Referência: `app/core/juizes.py:235`.

   O predicado efetivo é: não existe turno aberto ainda vivo **e** existe fim previsto **e** `agora >= fim`; sem previsto retorna falso. Turno aberto vivo pode expirar depois do fim da janela do template mais quatro horas; enquanto vivo, bloqueia o encerramento. Referências: `app/ponto/turnos.py:2000`, `app/ponto/turnos.py:2010`, `app/ponto/turnos.py:2024`, `app/ponto/turnos.py:2030`, `app/ponto/turnos.py:2115`, `app/ponto/turnos.py:2124`.

   O novo chamador passa `cel.data` e o `agora` recebido; `dia_encerrado_motivo` só chama `timezone.now()` se `agora is None`. O carregamento de turnos é filtrado pelo `data_turno` do dia solicitado. Referências: `app/chamados/services/fabrica_da_celula.py:123`, `app/ponto/turnos.py:2096`, `app/ponto/turnos.py:2098`, `app/ponto/turnos.py:2111`.

   Os cortes corrigidos autorizam esse juiz por fato encerrado e proíbem `data < hoje`; não exigem chamar outro juiz no seu lugar. O contrato admite instante ou fato encerrado. Referências: `app/docs/CORTES.md:55`, `app/docs/CORTES.md:62`, `CLAUDE.md:185`.

3. **Leitor com semântica diferente a destacar no item 6/8.** `pergunta_viva` trata não acusador como provisório pela **ata**: consulta `lastro.marcos_pendentes`, e, se não há pendente, retorna falso. A fábrica passa a decidir essa exceção pelo **encerramento geométrico/temporal do turno**. São critérios distintos em código, embora possam coincidir nos casos usuais. Referências: `app/chamados/juizes.py:1372`, `app/chamados/juizes.py:1385`, `app/chamados/juizes.py:1386`, `app/ponto/services/lastro.py:138`, `app/chamados/services/fabrica_da_celula.py:123`.

   Consequência dedutível: uma célula não acusadora, com falta vencida lida da ata, `dia_encerrado=False` e **sem `pendente` na ata** pode produzir motivo na fábrica enquanto o C6 considera a pergunta não viva. Existência desse estado no dado de produção: **[não verificado]**. Referências do caminho: `app/chamados/services/fabrica_da_celula.py:119`, `app/chamados/services/fabrica_da_celula.py:142`, `app/chamados/services/fabrica_da_celula.py:160`, `app/chamados/juizes.py:1386`.

   O leitor de faltas não consulta o veredito nesse corpo: lê `missing` da grade e converte os marcos faltantes; assim o veredito não elimina por si só essa possibilidade. Referências: `app/chamados/services/declaracao_texto.py:100`, `app/chamados/services/declaracao_texto.py:119`, `app/chamados/services/declaracao_texto.py:131`.

4. **Limites da conclusão normativa.** O aval ALVO fornecido manda incluir hoje no contador e deixar o teto no juiz, o que justifica corrigir o juiz compartilhado; autorização específica para todos os não acusadores administrativos e para a divergência fábrica/C6 acima não está demonstrada no texto fornecido. **[não verificado]**. A nova condição em si chama autoridade registrada e respeita o teto por instante antes de produzir motivos. Referências: `app/core/juizes.py:235`, `app/chamados/services/fabrica_da_celula.py:119`, `app/chamados/services/fabrica_da_celula.py:123`.

## 10/11 11:19 · sessao b604c2a26d23 · - · - · origem=Ronald (tmux)

Achado bloqueante de regressão: a saída nova é `ALVOS: N motivos devidos sem pergunta viva`, mas nove asserções existentes ainda exigem `ALVOS: N perguntas`; essas verificações ficam incompatíveis com o comando da cópia. `app/chamados/management/commands/disparar_perguntas_competencia.py:215`; `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:49`, `:54`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:84`, `:119`, `:173`; `app/chamados/tests/test_fabrica_pela_ata.py:51`, `:64`, `:71`; `app/chamados/tests/test_pergunta_do_dia_em_curso.py:106`.

Item 3:

- A guarda conserva exatamente as três operações: montar mudo, exigir `FIO MUDO: 1` às 13:40 e exigir ausência de pergunta. O delta 9d fornecido só acrescenta testes nesse arquivo; não altera o corpo da guarda. `app/chamados/tests/test_pergunta_do_dia_em_curso.py:460`.
- Não confundir com `diff_obra_v9d.patch`: a obra inteira transformou a antiga exigência de emissão em contagem sem escrita, conforme matriz; a 9d não repete essa mudança. Corpo atual em `app/chamados/tests/test_pergunta_do_dia_em_curso.py:460`; comparação histórica depende do anexo fornecido [não verificado independentemente por Git].
- Nas asserções existentes alteradas na 9d, a mudança substantiva é o contrato textual de ALVOS, acompanhada da exigência de mencionar Pauta DP; isso atende P3 da triagem fornecida. As demais alterações adicionam mensagens específicas, mínimos, unicidade do trecho mutado e novas verificações. `app/chamados/tests/test_e4a2_disparar_e_vigia.py:205`, `:222`, `:224`, `:242`, `:248`.
- O helper de matriz condiciona a verificação de `concorde` a `encerrado`, mas o teste existente mantém o padrão `encerrado=True` e segue exigindo zero; novos testes cobrem `encerrado=False`. `app/chamados/tests/test_e4a2_matriz_memoria.py:100`, `:169`, `:194`, `:199`.
- Integridade de todos os “corpos congelados”: [não verificado], pois não há `.git` na cópia nem manifesto identificado que nomeie esses corpos e seus hashes.

Item 7:

- Os catálogos agora têm mínimos: NuncaBateu 11, placar 12, relatório 10, matriz 19, C5 13. `app/chamados/tests/test_e4a2_nunca_bateu.py:66`; `app/core/tests/test_fio_mudo_placar.py:53`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:39`; `app/chamados/tests/test_e4a2_matriz_memoria.py:82`; `app/chamados/tests/test_e4a2_c5_memoria.py:71`.
- Todos esses laços têm controle verde antes da mutação, trecho único e falha pela mensagem pretendida; catálogos vazios/encolhidos são exercitados explicitamente. `app/chamados/tests/test_e4a2_nunca_bateu.py:318`, `:329`; `app/core/tests/test_fio_mudo_placar.py:294`, `:319`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:235`, `:245`; `app/chamados/tests/test_e4a2_matriz_memoria.py:352`, `:365`; `app/chamados/tests/test_e4a2_c5_memoria.py:201`, `:214`.
- Executei os dois módulos de memória com `PYTHONDONTWRITEBYTECODE=1`: 28 testes OK, sem banco e sem escrita. Os carregadores executam funções reais por AST e mantêm mutações em memória. `app/chamados/tests/e4_apoio.py:8`; `app/chamados/tests/e4_catracas.py:5`; resultado da execução sem arquivo persistido [não verificável por arquivo:linha].
- O novo selo que carrega os catálogos Django por AST prova apenas sua defesa contra catálogo vazio/encolhido; não executa seus mutantes de integração nem seus `setUp`. Portanto não equivale a execução verde ORM. `app/chamados/tests/test_e4a2_matriz_memoria.py:314`, `:325`, `:334`.
- Os testes novos de cenário para `concorde` em curso e noturno usam resposta dublada de `dia_encerrado` na bateria de memória; há teste Django separado, mas sua execução nesta auditoria permanece [não verificado]. `app/chamados/tests/test_e4a2_matriz_memoria.py:151`, `:205`; `app/chamados/tests/test_pergunta_do_dia_em_curso.py:297`.

Consequência para bancos 1–4: a bateria de memória permanece verde e cobre seus contratos; banco 5/suíte inteira verde não está demonstrado e há incompatibilidade textual concreta introduzida na 9d. `app/chamados/tests/test_e4a2_matriz_memoria.py:87`, `:278`; `app/chamados/management/commands/disparar_perguntas_competencia.py:215`; `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:49`.

## 10/11 11:19 · sessao eef2ccc05813 · - · - · origem=Ronald (tmux)

Itens 4–6, leitura da cópia:

1. `_pks_de_empresa` tem somente dois chamadores de produção: `contador_nunca_bateu` e `contador_fio_mudo`; aceita `None`, rejeita texto/bytes/objeto com `.pk` e não iteráveis, e materializa lista de `int`. Não há consumo duplo do gerador original: a primeira normalização produz lista, reutilizada no ORM e passada à segunda normalização. (`app/chamados/management/commands/disparar_perguntas_competencia.py:25`, `:52`, `:88`, `:104`, `:121`.)

2. Entradas de produção: `Command.handle` do disparar omite `empresas` → `None`; vigiar passa `None` ou lista materializada de instâncias `Empresa`. Nenhum desses chamadores passa Empresa única ou pk escalar. (`app/chamados/management/commands/disparar_perguntas_competencia.py:189`; `app/chamados/management/commands/vigiar_fio_morto.py:156`, `:160`, `:181`.)

3. Chamadores de testes: [nao verificado]
   - Integração contador: listas de pk/Empresa (`app/chamados/tests/test_e4a2_nunca_bateu.py:119`, `:125`; `app/chamados/tests/test_pergunta_do_dia_em_curso.py:314`; `app/core/tests/test_fio_mudo_placar.py:282`).
   - `None` implícito (`app/chamados/tests/test_o261_vigia_fio.py:811`; `app/core/tests/test_fio_mudo_placar.py:291`).
   - Funções reais por AST: tupla, lista, iterador de Empresa/str; `dias` também iterador (`app/chamados/tests/test_e4a2_c5_memoria.py:76`, `:84`, `:89`; `app/chamados/tests/test_e4a2_matriz_memoria.py:237`).
   - Escalares deliberadamente negativos: str, int, Empresa/objeto `.pk`; `None` explicitamente permitido (`app/chamados/tests/test_e4a2_nunca_bateu.py:310`; `app/chamados/tests/test_e4a2_matriz_memoria.py:214`).
   - Não encontrei chamador passando queryset; aceitação de queryset é inferência direta de `iter()` + conversão de cada elemento. (`app/chamados/management/commands/disparar_perguntas_competencia.py:33`.)

4. Matriz preservada no contador:
   - (a) retido pela lâmpada `nunca_bateu`, sem escrever; unidade `(colab,dia)`, esperado interseção com C5; desligado só em pares presentes na C5; janela anterior por empresa até ontem (`app/chamados/management/commands/disparar_perguntas_competencia.py:54`, `:59`, `:64`, `:66`, `:111`).
   - (b) lê todos os motivos do juiz e deduplica `(colab,dia,motivo)`, esperado 0 (`:115`, `:116`, `:122`).
   - (c) qualquer pergunta viva ligada exclui o chamado inteiro dos dois contadores (`:109`).
   - (d) universo `VIVOS`; tranca deve retirar o chamado pela sua porta, sem filtro adicional próprio do contador (`:99`; prova real `app/chamados/tests/test_e4a2_nunca_bateu.py:168`).
   - O piso “até ontem” aplica-se apenas à comparação C5; fio mudo não exclui hoje (`:64`, `:99`).

5. Bancos 1–4:
   - 1: tupla de juízes vazia e papel vigia; parser sem apply (`app/config/crons.py:2032`, `:2168`; `app/chamados/management/commands/disparar_perguntas_competencia.py:132`).
   - 2/3: mesmo contador alimenta números/esperados do carimbo e pipeline compara ambos (`app/chamados/management/commands/vigiar_fio_morto.py:181`, `:233`, `:241`; `app/core/management/commands/pipeline_placar.py:98`, `:105`, `:135`).
   - 4: executei `PYTHONDONTWRITEBYTECODE=1 bash bin/tests/test_papel_prazo_nao_deriva.sh`: rc 0, `papel_prazo: 12 ... juizes_por_varredura=0`, `OK` [resultado de execução, sem arquivo persistido]. Exceção real vazia e controles sintéticos no próprio selo (`bin/tests/test_papel_prazo_nao_deriva.sh:305`, `:384`, `:405`).
   - Integração banco/suite inteira não executada [não verificado]. [nao verificado]

6. Escritores/leitores dos valores:
   - `ALVOS`: escritor stdout calcula `len(_mudos)` de motivos e pessoas distintas; distribuição empresa também conta motivos (`app/chamados/management/commands/disparar_perguntas_competencia.py:201`, `:215`).
   - `fio_mudo_alvos`: derivador único contador; vigiar escreve JSON por `_carimbar`, somente frota; pipeline lê dia civil e compara número/esperado; `bin/fio_estado.py` itera todas as chaves do esperado e mostra divergências (`app/chamados/management/commands/disparar_perguntas_competencia.py:122`; `app/chamados/management/commands/vigiar_fio_morto.py:118`, `:227`, `:241`; `app/core/management/commands/pipeline_placar.py:85`; `bin/fio_estado.py:125`, `:191`).
   - `nunca_bateu_sem_adesao`: mesmo nome designa contador aninhado completo no retorno/topo JSON e número dentro de `censo`/`esperado`; leitores acessam esses níveis explicitamente. Informativos C5 não viram esperado (`app/chamados/management/commands/disparar_perguntas_competencia.py:66`, `:124`; `app/chamados/management/commands/vigiar_fio_morto.py:234`, `:241`, `:242`; `app/core/management/commands/pipeline_placar.py:98`).
   - C5 é `{colab_id:[datas]}`, por empresa, anterior+corrente até ontem, desligados pela lavra do retrato; recebe pks, não Empresa (`app/escala/services/cadastro_realidade.py:137`, `:177`, `:181`, `:197`).
   - `rc` disparar=2 só `--alarme` e deixou; fio mudo/retidos não alteram rc. Vigiar=2 pelos próprios gritos; pipeline=2 por alarmes com chave. `crons_quebrados` trata 0 e 2 como saídas esperadas (`app/chamados/management/commands/disparar_perguntas_competencia.py:219`; `app/chamados/management/commands/vigiar_fio_morto.py:252`; `app/core/management/commands/pipeline_placar.py:160`; `app/core/services/crons_quebrados.py:16`).

**Regressão certa da 9d:** produção mudou para `ALVOS: N motivos devidos...`, mas nove asserções ainda exigem `ALVOS: N perguntas`: `app/chamados/tests/test_colab_nao_vaza_fio_mudo.py:49`, `:54`; `app/chamados/tests/test_selo_emissor_fio_mudo.py:84`, `:119`, `:173`; `app/chamados/tests/test_fabrica_pela_ata.py:51`, `:64`, `:71`; `app/chamados/tests/test_pergunta_do_dia_em_curso.py:106`. A saída atual é incompatível com elas (`app/chamados/management/commands/disparar_perguntas_competencia.py:215`); suite inteira verde desta cópia [não verificado].

## 10/11 11:19 · sessao 1d62df0db80f · URGENCIA-E4B · sonda · origem=Code

A sonda retornou **ERRO_SONDA** no passo `alcance_a_e`, com código `ERRO_INTERNO_REGISTRAR_ENGOLIDO`; resultado: **INCONCLUSIVA; nenhum contador publicado** [nao verificado]. [nao verificado]

Cadeia de locais, na ordem informada:

1. `<string>:625` [nao verificado]
2. `<string>:204` [nao verificado]
3. `app/chamados/services/vigia_fio.py:485`
4. `app/chamados/services/vigia_carimbo.py:106`
5. `app/chamados/juizes.py:734`
6. `app/chamados/juizes.py:359`
7. `app/chamados/services/materializacao.py:386`
8. `app/chamados/services/materializacao.py:302`
9. `<string>:105` [nao verificado]

O último ponto conferível chama `registrar_engolido` para `tipo_marco_divergente`, quando os tipos derivados divergem (`app/chamados/services/materializacao.py:299`, `app/chamados/services/materializacao.py:302`, `app/chamados/services/materializacao.py:303`).

A saída também encadeia `builtins.Exception`, sem código nem locais; ambas as exceções têm `sqlstate: null` [nao verificado].

Não há valores publicados para comparar com **1.233**, particionar por classe/dono/empresa ou medir `censo_alcance` e sua interseção com a Central [nao verificado]. [nao verificado]

## 10/11 11:17 · sessao 3bed5b87a8db · - · - · origem=Ronald (tmux)

Censo PWA por leitura, sem escrita.

**Rotas e cascas**

- `/ponto/bater/` chama `views.bater_ponto`; `/ponto/bater-v2/` chama a mesma view com `ui_v2=True`; `/ponto/batida-offline/` é outra porta de POST. (`app/ponto/urls.py:11`, `:12`, `:13`.)
- V2 renderiza `ponto/bater_ponto_v2.html`; clássico renderiza `_template_triagem(context)`, normalmente `ponto/triagem/apto.html`. O arquivo `bater_ponto.html` não é o template escolhido nesses returns. (`app/ponto/views.py:225`, `:235`, `:356`, `:358`.)
- V2, legado e base da triagem estendem **literalmente `base.html`**. Não usam a variável `base_tpl`, embora o processor a forneça. A execução destas telas dentro de `base_app.html` não está demonstrada por essas heranças. (`app/templates/ponto/bater_ponto_v2.html:1`; `app/templates/ponto/bater_ponto.html:1`; `app/templates/ponto/triagem/base_triagem.html:1`; `app/core/context_processors.py:194`, `:210`.)
- As duas cascas carregam `pwa.js`, `geo-colab-map.js`, UI, tabela, popover, ícones, diálogo e drawer; `base.html` acrescenta filtros/fita, nudge, cobrança e copiloto. (`app/templates/base.html:20`, `:83`, `:85`, `:86`, `:88`, `:92`, `:93`, `:388`, `:393`, `:399`, `:432`, `:440`; `app/templates/base_app.html:20`, `:77`, `:78`, `:80`, `:85`, `:145`, `:154`, `:194`.)

**Barreiras comprovadas**

| Sítio | Condição e resultado | Classe L-030 / teste exigindo | [nao verificado]
|---|---|---|
| `app/templates/ponto/bater_ponto_v2.html:285`, `:287`, `:290`, `:325` | Sem `_fotoBlob` após capturar frame: toast de habilitar câmera/tirar selfie; `false`; clique retorna antes do POST. | Sim, impede fato por prova ausente; teste que exija essa barreira [nao verificado]. |
| `app/templates/ponto/bater_ponto_v2.html:292`, `:297`, `:299`, `:300`, `:325` | Sem latitude ou longitude, após tentativas de GPS de 10 e 12 s: toast de habilitar localização/usar Justificativa; `false`; não envia. GPS velho dispara nova tentativa, mas GPS velho ainda existente não é recusado depois. | Sim; teste da barreira de template [nao verificado]. |
| `app/templates/ponto/bater_ponto_v2.html:350`, `:353` | Exceção no fetch **e** coordenada nula: retorna antes de enfileirar offline. | Sim; teste [nao verificado]. Ramo hoje já antecedido pelo requisito acima. |
| `app/templates/ponto/bater_ponto_v2.html:368`, `:370`, `:371` | Offline e coordenada nula: toast GPS/Justificativa; retorna antes da fila. | Sim; teste [nao verificado]. Ramo hoje já antecedido pelo requisito acima. |
| `app/templates/ponto/triagem/apto.html:248`, `:253`, `:255` | Câmera offline falhou: “Camera indisponivel — contate o supervisor”; esconde `btn-capturar-off`; não há chamada da fila nesse catch. | Sim; teste [nao verificado]. |
| `app/templates/ponto/triagem/apto.html:277`, `:278`, `:279`, `:281`, `:284` | GPS ausente e primeiro aviso da sessão: aguarda `hxConfirmar`; resposta falsa retorna antes de `registrarBatidaOffline`. | Confirmação condiciona fato à falta de prova; mesma classe enquanto impede envio. Teste exigindo cancelamento/guarda GPS [nao verificado]. |
| `app/ponto/views.py:316`, `:329`, `:330` | POST E, turno aberto e sem `confirmar`: guarda GPS/foto em sessão, devolve HTML de confirmação, sem registro. | Sim, condicional de jornada antes do escritor. **Teste exige:** `app/ponto/tests/test_f7_ramos_frios.py:128`, `:135`, `:140`, `:141`, `:142` — exige sessão, nenhuma nova Batida e template `turno_confirmar.html`. |
| `app/templates/ponto/bater_ponto_v2.html:64`, `:67`, `:69`, `:71` | `bloqueio_horario` ou `aguardando_confirmacao`: não renderiza botão; manda “Abrir ponto”. | Mesmo efeito de barreira de tela. Teste exigindo esse ramo V2 [nao verificado]. |
| `app/ponto/views.py:243`, `:244` | Onboarding incompleto: redirect antes de processar POST ou abrir tela. | Barreira de acesso à batida por cadastro; teste específico [nao verificado]. |
| `app/ponto/views.py:251`, `:253`, `:255` | **Só GET**, comunicado pendente: redirect para ciência antes da tela. POST não entra nesse ramo. | Interstitial de tela, não guarda da requisição de batida. **Teste exige:** `app/comunicados/tests.py:181`, `:184`, `:185`, `:186`; livre após ciência: `:188`, `:192`, `:193`. |

O diálogo é cancelável por botão Não, clique fora ou Escape. `naoBloquear=true` significa somente **fail-open se o markup não montar**; não elimina a confirmação nem seu cancelamento. (`app/templates/core/_dialogo.html:157`, `:158`, `:159`, `:161`, `:172`, `:184`, `:185`.) O selo exige alcançar o diálogo nas duas cascas e exige o fail-open/string `naoBloquear`; não exige bloquear GPS ausente. (`app/core/tests/test_selo_modal_nao_nasce_vazio.py:115`, `:117`, `:135`, `:138`, `:139`, `:141`.)

**Ausências de barreira e sinais**

- Precisão V2 `>200 m` apenas altera chip/texto; não retorna do envio. Raio apenas desenha mapa e altera badge “fora do raio”. (`app/templates/ponto/bater_ponto_v2.html:174`, `:214`, `:218`, `:220`, `:228`, `:229`, `:230`.)
- `v2AvisoGps` contém “NAO sera registrada sem GPS”, mas não encontrei chamada dessa função; sua execução é [nao verificado]. (`app/templates/ponto/bater_ponto_v2.html:312`, `:313`, `:316`.)
- Triagem online anuncia “Sem GPS - registrando para revisao” e segue até `form.submit()`. (`app/templates/ponto/triagem/apto.html:221`, `:225`, `:226`, `:235`.)
- Triagem online, quando câmera falha, libera os botões sem selfie; entretanto o caminho de câmera bem-sucedida ainda exibe botão de ponto somente após callback de `toBlob`. (`app/templates/ponto/triagem/apto.html:156`, `:159`, `:163`, `:182`, `:185`.)
- Legado mantém botões ocultos antes de selfie e no refazer; câmera negada oferece câmera do sistema; falha de processamento após confirmação libera mesmo sem blob. Seu uso por rota nesta cópia é [nao verificado]. (`app/templates/ponto/bater_ponto.html:246`, `:362`, `:366`, `:423`, `:449`, `:456`, `:465`, `:473`.)
- Legado online permite GPS ausente e falha HTTP/rede cai para fila. (`app/templates/ponto/bater_ponto.html:507`, `:509`, `:510`, `:533`, `:540`, `:543`.)
- A falta de colaborador elimina botão nas três famílias, como identificação necessária do titular. (`app/templates/ponto/bater_ponto_v2.html:39`; `app/templates/ponto/bater_ponto.html:8`; `app/templates/ponto/triagem/base_triagem.html:20`.)
- Travas de clique em andamento são `_enviando` V2 e `_confirmandoOffline` triagem; retornam para evitar submissões concorrentes. (`app/templates/ponto/bater_ponto_v2.html:321`; `app/templates/ponto/triagem/apto.html:260`, `:262`.)

**Fila PWA e service worker**

- `registrarBatidaOffline` aceita coordenadas nulas e salva em IndexedDB; não há guarda geo. Erro de IndexedDB rejeita a promessa. (`app/static/pwa.js:107`, `:113`, `:116`, `:120`, `:121`.)
- Sync só roda online e com meta `user-perfil=colaborador`; ordena por timestamp e omite campos latitude/longitude nulos do multipart. (`app/static/pwa.js:126`, `:127`, `:128`, `:137`, `:145`, `:146`.)
- Embora a fila armazene timestamp corrigido, precisão e instante de captura, o POST monta tipo, coordenadas, `timestamp_dispositivo`, liveness e foto; não envia `timestamp`, `offline`, `gps_accuracy` ou `gps_captured_at`. (`app/static/pwa.js:114`, `:117`, `:143`, `:147`, `:150`, `:152`, `:153`.)
- HTTP 2xx apaga item; HTTP 400 também apaga como definitivo, sem retry; 403/5xx permanecem na fila. (`app/static/pwa.js:158`, `:162`, `:166`, `:169`, `:172`, `:177`.)
- SW retorna antes de interceptar qualquer método diferente de GET: **não há barreira POST no SW**. (`app/static/sw.js:35`, `:36`.)
- Background sync delega à página aberta; sem página apenas notifica para abrir o app, não envia sozinho. (`app/static/sw.js:112`, `:116`, `:117`, `:119`, `:121`, `:123`.)
- O mapa global trata coordenadas não numéricas mostrando “Sem localização de batida recente” e retorna sem construir Google Maps; não afeta envio de batida. (`app/static/js/geo-colab-map.js:28`, `:39`, `:40`, `:41`.)

**Testes hoje explicitamente contrários à cura sem GPS** [nao verificado]

- API exige 403 e código `gps_obrigatorio`: `app/api/tests/test_gps_obrigatorio.py:19`, `:21`, `:22`.
- Exige auditoria `batida_recusada_sem_gps`: `app/api/tests/test_gps_obrigatorio.py:24`, `:28`.
- Exige zero nova batida sem GPS: `app/api/tests/test_gps_obrigatorio.py:34`, `:38`.
- Side-effects, apesar do nome antigo “cria alerta sem_gps”, exige 403/código após o flip: `app/ponto/tests/test_batida_sideeffects.py:119`, `:128`, `:130`, `:131`.
- Há selo no sentido oposto a bloqueio por órfão: GET V2 com entrada de 40 h deve ser 200 e não conter “Abrir ponto”. (`app/ponto/tests/test_bater_nao_bloqueia.py:18`, `:20`, `:23`, `:24`, `:25`.)

## 10/11 11:16 · sessao f90352db97b4 · - · - · origem=Ronald (tmux)

Censo de testes e chokepoint concluído por leitura/AST em memória; resultados de execução da suíte [nao verificado]. [nao verificado]

| Barreira/condição | Teste que a exige hoje | [nao verificado]
|---|---|
| GPS ausente → HTTP 403, `gps_obrigatorio` | `app/api/tests/test_gps_obrigatorio.py:21`, `:22`; `app/ponto/tests/test_batida_sideeffects.py:130`, `:131` |
| GPS ausente → zero gravação | `app/api/tests/test_gps_obrigatorio.py:38`; `app/ponto/tests/test_batida_sideeffects.py:133` |
| GPS ausente → auditoria da recusa | `app/api/tests/test_gps_obrigatorio.py:28` |
| Desligado → HTTP 403 | `app/api/tests/test_api_bater_ponto.py:65` |
| Desligado → conservar literalmente o dicionário de bloqueio nos dois kernels | `app/ponto/tests/test_afastado_nunca_bloqueia.py:33`, `:36`; kernels enumerados em `:25` |
| Férias → resposta diferente de 200 | `app/api/tests/test_api_bater_ponto.py:88` |
| Férias, inclusive com afastamento cobrindo hoje → HTTP 403 / `colab_ferias` | `app/ponto/tests/test_afastado_na_batida.py:115`, `:116` |
| Tipo oposto em segundos na API → `ignorada_intervalo`, sucesso verdadeiro, ID da primeira, apenas uma linha | `app/api/tests/test_api_bater_ponto.py:639`, `:640`, `:641`, `:642` |
| Tipo oposto em até 60 s no POST web → zero segunda batida | `app/ponto/tests/test_bater_colisao_oposto.py:42`; `app/ponto/tests/test_f9d_t1_caracterizacao_ramo_quente.py:120` |
| POST web oposto → HTTP 200, `ignorada_intervalo`, ID e tipo da anterior | `app/ponto/tests/test_f9d_t1_caracterizacao_ramo_quente.py:115`, `:117`, `:118`, `:119` |
| Tipo oposto a 120 s com template/marcos, origem `app` → `None` e conflito | `app/ponto/tests/test_dedup_envelope_template.py:41`, `:43`, `:46` |
| Tipo oposto a 30 s, origem institucional `reg_externa` → `None`, conflito e nenhuma nova linha | `app/ponto/tests/test_registrar_batida.py:37`, `:41`, `:42`, `:43` |
| Mesmo tipo a outro instante, +20 s → mesma linha, nenhuma nova | `app/ponto/tests/test_registrar_batida.py:72`, `:76`, `:78` |
| API com timestamps distintos por 1 minuto → `ja_registrada`, apenas uma linha | `app/api/tests/test_bug127_dedup_unico.py:121`, `:122`, `:123`, `:124` |
| Mesma chave exata, mesmo timestamp e outro tipo → batida existente | `app/ponto/tests/test_bater_mesma_chave_idempotente.py:47`, `:48`, `:49` |
| Mesma chave exata de batida retratada → batida existente | `app/ponto/tests/test_bater_mesma_chave_idempotente.py:56`, `:57`, `:58` |

A lei local nomeia explicitamente como pendências da mesma classe L-030: GPS ausente, desligado/férias e oposto em 60/300 s. A referência correta nesta cópia é `app/docs/LEIS.md:165`; `:162` corresponde à L-011. A regra superior está em `CLAUDE.md:140`.

A AST de `registrar_batida` contém seis retornos e nenhum `raise` explícito: existente por mesmo tipo em `app/ponto/registro_batida.py:115`, existente por chave exata em `:122`, `None` por competência institucional em `:137`, `None` por colisão oposta em `:151`, criação com conflito em `:156`, criação normal em `:161`. A janela oposta é 300 s havendo escala/tipo/marcos e 60 s no restante (`app/ponto/registro_batida.py:30`, `:31`, `:32`); a busca usa limites inclusivos (`:142`, `:143`), apesar de a mensagem usar `<` (`:147`). A exceção de marco próprio aceita distância **≤5 minutos**, inclusive na véspera (`:41`, `:42`, `:48`).

A guarda de competência não barra chão: depende de `not fato_de_chao`, cujo padrão deriva de `not autoridade_humana` (`app/ponto/registro_batida.py:130`, `:132`). Os testes exigem chão aceito em competência aprovada (`app/ponto/tests/test_ajuste_retroativo_folha_fechada.py:36`, `:40`, `:66`, `:70`) e materialização institucional com `fato_de_chao=False` recusada (`:56`, `:61`, `:62`).

Não confundir testes que **toleram** uma barreira com os que a exigem: `app/chamados/tests/test_invariantes_disputa.py:292` reconhece o 403 de GPS e o exclui da verificação `<400` em `:295`; não exige a rejeição. `app/ponto/tests/test_gate_ferias_a_tela_diz.py:25`, `:32`, `:39`, `:40` exige apenas aviso e confirmação dentro do ramo de sucesso.

Há testes que exigem aceitação, contrariando possíveis barreiras adicionais: [nao verificado]

- Fora da cerca com precisão 5 → HTTP 200 e alerta/chamado: `app/ponto/tests/test_batida_sideeffects.py:104`, `:106`, `:110`, `:116`.
- Fora da cerca sem precisão → HTTP 200 e `gps_incerto`: `app/ponto/tests/test_batida_sideeffects.py:150`, `:152`, `:158`.
- GPS mock → HTTP 200 e motivo `gps_mock`: `app/api/tests/test_api_bater_ponto.py:159`, `:160`, `:164`, `:165`, `:169`, `:170`.
- Afastado → HTTP 200, aviso e Pauta: `app/api/tests/test_api_bater_ponto.py:75`, `:76`, `:78`.
- Sem posto → gravação exigida: `app/ponto/tests/test_batida_sideeffects.py:202`, `:203`.
- Tipo fora de `E/S` → HTTP 400: `app/api/tests/test_api_bater_ponto.py:121`, `:127`.

Teste que exige rejeição por `tolerante_offline`, raio ou precisão no registro de chão: [nao verificado]. [nao verificado]

## 10/11 11:16 · sessao 2e59c613d923 · - · - · origem=Ronald (tmux)

Sinal já existente, sem modelo novo: `Batida.latitude` e `Batida.longitude` aceitam `NULL`; `gps_accuracy` também é nullable, mas representa incerteza, não ausência de posição. Não há flag `sem_geo` em `Batida`; o sinal primário existente são as coordenadas ausentes. (`app/ponto/models.py:8`, `app/ponto/models.py:58`, `app/ponto/models.py:59`, `app/ponto/models.py:60`.)

- Escritor: `_sanitiza_geo` transforma `''`, `None` ou valor não numérico em `None`; `registrar_batida` aplica isso antes da gravação. As portas web/API já repassam latitude/longitude ao escritor. (`app/ponto/registro_batida.py:3`, `app/ponto/registro_batida.py:11`, `app/ponto/registro_batida.py:16`, `app/ponto/registro_batida.py:105`, `app/ponto/services/triagem_batida.py:701`, `app/api/views_core.py:681`, `app/api/views.py:684`.)
- Sinal secundário existente: `AlertaGeofence.tipo='sem_gps'`; `verificar_geofence` detecta coordenada ausente e, havendo posto, cria alerta com coordenadas nulas e texto “localização não validada”. Sem posto, esse ramo não cria alerta. (`app/ponto/models.py:233`, `app/ponto/services/geofence.py:247`, `app/ponto/services/geofence.py:248`, `app/ponto/services/geofence.py:252`, `app/ponto/services/geofence.py:256`.)
- Gestão: o mesmo serviço abre/anexa ocorrência de `gps_ausente`, grava `batida_lat/batida_lon=None` no contexto e envia push de supervisão; o catálogo nomeia esse módulo “Batida sem GPS”. (`app/ponto/services/geofence.py:309`, `app/ponto/services/geofence.py:322`, `app/ponto/services/geofence.py:327`, `app/ponto/services/geofence.py:335`, `app/ponto/services/geofence.py:369`, `app/chamados/catalogo/modulos.py:162`.)
- Invocação posterior ao registro: API nas duas cascas chama `verificar_geofence`; web também o chama. (`app/api/views_core.py:809`, `app/api/views_core.py:811`, `app/api/views.py:809`, `app/api/views.py:811`, `app/ponto/services/triagem_batida.py:752`.)

Leitores e tratamento de `NULL`:

| Leitor | Comportamento comprovado |
|---|---|
| Detalhe/mapa do colaborador | Exclui latitude e longitude nulas antes de converter com `float`; conta separadamente batidas com geo e alertas `sem_gps`. (`app/colaboradores/services/detalhe.py:20`, `app/colaboradores/services/detalhe.py:22`, `app/colaboradores/services/detalhe.py:53`, `app/colaboradores/services/detalhe.py:67`, `app/colaboradores/services/detalhe.py:80`.) |
| Fio do colaborador | Histórico e última geo incluem só batidas com ambas as coordenadas. (`app/chamados/views.py:692`, `app/chamados/views.py:693`, `app/chamados/views.py:694`.) |
| Painel situacional/pino | Lote passa latitude, longitude e accuracy; pino com coordenada ausente retorna `sem_gps`, cinza. (`app/colaboradores/services/situacional.py:137`, `app/colaboradores/services/situacional.py:145`, `app/colaboradores/painel_cell.py:37`, `app/colaboradores/views.py:2474`, `app/colaboradores/views.py:2475`.) |
| Drawer do colaborador | `tem_gps=bool(latitude and longitude)`. (`app/colaboradores/services/drawer.py:63`.) |
| API de detalhe da batida / dashboard | Serializa nulos; JS mostra “Sem localizacao” e só cria mapa com ambas as coordenadas. (`app/core/views.py:1024`, `app/core/views.py:1025`, `app/templates/core/dashboard2.html:242`, `app/templates/core/dashboard2.html:246`, `app/templates/core/dashboard2.html:287`.) |
| Painel operacional | Propaga `None` na ausência de coordenada; presença usa `PingGeo`, não a coordenada da batida. (`app/core/services/painel_op.py:215`, `app/core/services/painel_op.py:227`, `app/core/services/painel_op.py:228`, `app/ponto/presenca.py:53`, `app/ponto/presenca.py:64`.) |
| Offline pendente | Conta sem GPS pela ausência de latitude; template mostra “Sem GPS”. (`app/ponto/views.py:485`, `app/ponto/views.py:486`, `app/templates/ponto/batidas_offline_pendentes.html:96`, `app/templates/ponto/batidas_offline_pendentes.html:100`.) |
| Relatório geo do chamado | Mostra “Batida registrada SEM GPS” e “sem coordenada (GPS ausente)”; não cria link de mapa sem latitude. (`app/templates/chamados/partials/_relatorio_geo.html:18`, `app/templates/chamados/partials/_relatorio_geo.html:19`, `app/templates/chamados/partials/_relatorio_geo.html:30`, `app/templates/chamados/partials/_relatorio_geo.html:34`.) |
| Alerta ainda válido | Sem coordenada continua válido; não tenta calcular distância. (`app/ponto/models.py:263`, `app/ponto/models.py:264`.) |
| Reconciliação de alertas | Coordenada ausente entra em `sem_dado` e é pulada. Comando é sob demanda. (`app/ponto/management/commands/reconciliar_alertas_geofence.py:42`, `app/ponto/management/commands/reconciliar_alertas_geofence.py:44`, `app/config/crons.py:1452`.) |
| Reconciliação de chamados geo | Filtra ocorrências sem coordenadas; se não sobra ocorrência, mantém chamado. Cron chama o comando com `--alarme`. (`app/ponto/services/geofence.py:403`, `app/ponto/services/geofence.py:405`, `app/ponto/management/commands/reconciliar_geofence.py:33`, `app/config/crons.py:486`.) |
| Reconciliação de vínculo | Seleciona somente ambas as coordenadas presentes; sem batidas com geo retorna `None`, sem julgar. (`app/ponto/services/geofence.py:51`, `app/ponto/services/geofence.py:55`, `app/ponto/services/geofence.py:67`.) |
| Detector de vínculo/lavra | Exclui latitude nula, mas não longitude nula; lê accuracy não nula. (`app/ponto/services/vinculo_na_lista.py:49`.) |

**Quebra concreta identificada:** meia coordenada (`latitude` preenchida, `longitude=None`, `gps_accuracy` preenchida) passa pelo filtro do detector e chega a `float(lon)`, que recebe `None`. Com ambas as coordenadas nulas, essa batida é excluída pelo filtro. A mesma insuficiência existe para posto com apenas latitude: seleção e guarda só verificam latitude antes de converter longitude. Ocorrência desses dados em produção: [nao verificado]. (`app/ponto/services/vinculo_na_lista.py:47`, `app/ponto/services/vinculo_na_lista.py:49`, `app/ponto/services/vinculo_na_lista.py:55`, `app/ponto/services/vinculo_na_lista.py:56`, `app/ponto/services/vinculo_na_lista.py:65`, `app/ponto/services/vinculo_na_lista.py:75`.)

AST dos Python de `app/relatorios` e `app/folha` não encontrou acesso por atributo a `latitude`, `longitude` ou `gps_accuracy`; a busca textual nos dois domínios e em `motor_calculo_v2.py`/`turnos.py` também não encontrou essas referências. Para o motor, os trechos examinados consomem timestamp/tipo das batidas; demonstração executada com banco de batidas sem geo: [nao verificado]. (`app/ponto/motor_calculo_v2.py:578`, `app/ponto/motor_calculo_v2.py:598`, `app/ponto/motor_calculo_v2.py:609`, `app/ponto/motor_calculo_v2.py:663`.)

Testes que já preservam a semântica sem geo: [nao verificado]

- Alerta sem GPS continua válido, sem remedição. (`app/ponto/tests/test_alerta_obsoleto.py:58`, `app/ponto/tests/test_alerta_obsoleto.py:63`.)
- Batida sem GPS não julga nem fecha divergência de vínculo. (`app/ponto/tests/test_varredura_vinculo_evento.py:57`, `app/ponto/tests/test_varredura_vinculo_evento.py:64`.)
- Pino sem coordenada fica cinza nos leitores. (`app/colaboradores/tests/test_o200_pino_geo_le_o_juiz.py:215`, `app/colaboradores/tests/test_o200_pino_geo_le_o_juiz.py:221`.)
- O teste chamado `test_sem_gps_cria_alerta_sem_gps_e_chamado` hoje exige o contrário do nome: HTTP 403, `gps_obrigatorio` e nenhuma batida gravada. (`app/ponto/tests/test_batida_sideeffects.py:119`, `app/ponto/tests/test_batida_sideeffects.py:128`, `app/ponto/tests/test_batida_sideeffects.py:130`, `app/ponto/tests/test_batida_sideeffects.py:135`.)

## 10/11 11:13 · sessao 9617f2b74b15 · FIO-E0-V4-E4B · leitura · origem=Code

1. **A origem das 1.233 permanece [nao verificado].** As candidatas encontradas são:

| Tela/contador | Autoridade do número exibido |
|---|---|
| **Central de Atendimento** | `painel_gestao → montar_contexto_painel`. O total da lista é `chamados_qs.count()`, depois dos filtros e exclusões; o queryset completo fica em `ctx['paginator'].object_list`. (`app/chamados/views.py:727`; `app/chamados/services/painel.py:333`; `app/chamados/services/painel.py:422`; `app/templates/chamados/partials/_lista_resumo.html:15`) |
| Pilhas **Validar/Cobrar/Decidir** | `particionar_verbos(qs_vivos(...))`, consumido pelo contexto e pelas pílulas. (`app/chamados/services/painel.py:230`; `app/chamados/services/verbos.py:30`; `app/templates/chamados/painel_gestao.html:42`) |
| **Central de Justificativas — Pendentes** | `fila_justificativas`: `Justificativa.objects.filter(status='pendente').count()`. **Esse card não recebe o filtro de empresa**, embora a lista receba. (`app/ponto/views.py:1717`; `app/ponto/views.py:1736`; `app/templates/ponto/fila_justificativas.html:41`) |

“Central de Chamados” é um link para a mesma Central de Atendimento. Há ainda dois números próximos: o **badge da navbar**, que prefere `PlacarNavbar.badge_chamados`, e o **atalho HE**, produzido por `pendentes_na_central`. A sonda lê ambos sem tratá-los como outra Central. (`app/templates/ponto/partials/painel_justificativa.html:80`; `app/core/context_processors.py:68`; `app/chamados/views.py:762`)

Se as 1.233 forem o badge lavrado, sua decomposição histórica exata permanece **[nao verificado]**: o escritor guarda o número e o contexto agregado, sem os IDs dos integrantes. A sonda distingue essa lavra da partição atual. (`app/core/management/commands/lavrar_badge_navbar.py:39`; `app/core/management/commands/lavrar_badge_navbar.py:73`; `app/core/management/commands/lavrar_badge_navbar.py:92`)

2. **Classe e dono têm dimensões distintas na própria casa:**

| Item | Classe | Dono | Ligação ao dia |
|---|---|---|---|
| Chamado | Origem: `modulo_origem → _rotulo_origem`; ação: `particionar_verbos`. (`app/chamados/services/painel.py:392`; `app/chamados/services/verbos.py:47`) | Atribuição humana: `atribuido_a_id`; pilha: `dono_do_chamado`, com insumos de `creditos_perguntas`; departamento: `departamento_do_chamado`. A sonda conserva os três. (`app/chamados/models.py:64`; `app/templates/chamados/partials/_lista_chamados.html:96`; `app/chamados/catalogo/natureza.py:41`; `app/chamados/services/raiox.py:51`; `app/chamados/utils.py:32`) | `(colaborador_id, dia_do_fato(chamado))`; a função prioriza a célula ligada. Dia não identificado fica separado, sem ser presumido fora do E4b. (`app/chamados/models.py:38`; `app/chamados/juizes.py:87`; `app/chamados/juizes.py:98`) |
| Justificativa | `tipo/get_tipo_display`. (`app/templates/ponto/fila_justificativas.html:108`) | O pendente diz “aguardando DP”; `aprovada_por` é autor da decisão, não atribuição corrente. (`app/ponto/models.py:131`; `app/ponto/models.py:160`) | `(colaborador_id, data_turno)`; `chamado_origem` é opcional. (`app/ponto/models.py:136`; `app/ponto/models.py:144`; `app/ponto/models.py:173`) |

Para **células-dia do E4b**, a sonda também consulta `InsumosDoDono.dono`, que chama o juiz real `dono_da_divergencia`, usando a alimentação lavrada de CxR. Ausência de resposta permanece `[nao verificado]`; não existe mapa classe a–e → dono. (`app/ponto/management/commands/e6_oraculo.py:182`; `app/escala/services/cadastro_realidade.py:1024`; `app/chamados/services/vigia_fio.py:99`)

3. **O universo a–e é `censo_alcance`.** Todas as linhas `rota == 'sem_porta'` são preservadas independentemente do limite; `limite=0` permite obter esse conjunto completo, conferindo tamanho e classes contra os contadores reais. Inclui o caso legítimo sem FK de célula, identificado pelo par colaborador/data. (`app/chamados/services/vigia_fio.py:548`; `app/chamados/services/vigia_fio.py:628`; `app/chamados/services/vigia_fio.py:667`; `app/chamados/services/vigia_fio.py:695`)

`censo` é outro universo e trunca suas listas; `classificar`, importada de `vigia_carimbo`, não fornece as classes a–e. (`app/chamados/services/vigia_fio.py:328`; `app/chamados/services/vigia_fio.py:454`; `app/chamados/services/vigia_fio.py:463`)

O juiz de exportação é `folha.export.competencia_entregue`, composto com `janela_atual` por `competencia_do_dia_entregue`. **O alcance cobre a competência corrente e a anterior enquanto não entregue; não cobre todo o histórico exportado.** (`app/chamados/services/vigia_fio.py:822`; `app/folha/export.py:556`; `app/ponto/services/he_pendente_lavrado.py:220`)

4. Script proposto abaixo; execução em Django/produção e resultados: **[nao verificado]**.

```python
# Proposta de sonda; execucao e resultados: [nao verificado].
# Na pasta que contem manage.py:
# PYTHONDONTWRITEBYTECODE=1 python manage.py tenant_command shell --schema=juliani \
#   -c 'exec(compile(open("/tmp/sonda_e4b.py", encoding="utf-8").read(), "/tmp/sonda_e4b.py", "exec"))'
import sys
sys.dont_write_bytecode = True
import contextlib
import datetime as dt
import io
import json
import logging
from collections import Counter
from pathlib import Path
from types import SimpleNamespace
from unittest.mock import patch

from django.db import connection, connections, transaction
from django.test import RequestFactory
from django.utils import timezone

EMPRESAS = (2, 3, 4)
PASSO = "inicio"
SAIDA = []
FALHAS_SQL = []
AGORA = timezone.now()
HOJE = timezone.localdate(AGORA)
FONTE = (
    "app/chamados/services/painel.py:198; "
    "app/chamados/services/verbos.py:30; "
    "app/ponto/views.py:1695; app/ponto/models.py:131; "
    "app/chamados/catalogo/natureza.py:41; app/chamados/utils.py:32; "
    "app/chamados/services/vigia_fio.py:542"
)

class FalhaSonda(BaseException):
    pass

class RollbackFinal(BaseException):
    pass

def exigir(condicao, codigo):
    if not condicao:
        raise FalhaSonda(codigo)

def emitir(tipo, fonte=FONTE, **dados):
    SAIDA.append(dict(tipo=tipo, fonte=fonte, **dados))

def col(pk):
    return None if pk is None else "col%03d" % pk

def erro_seguro(exc):
    # Sem str(exc): erros de banco/aplicacao podem conter nomes/CPF.
    cadeia = []
    atual = exc
    vistos = set()
    while atual is not None and id(atual) not in vistos:
        vistos.add(id(atual))
        locais = []
        tb = atual.__traceback__
        while tb:
            nome = tb.tb_frame.f_code.co_filename
            if "/app/" in nome:
                nome = "app/" + nome.split("/app/", 1)[1]
            else:
                nome = Path(nome).name
            locais.append("%s:%d" % (nome, tb.tb_lineno))
            tb = tb.tb_next
        cadeia.append(dict(
            tipo=type(atual).__module__ + "." + type(atual).__name__,
            codigo=(atual.args[0] if isinstance(atual, FalhaSonda) else None),
            sqlstate=getattr(atual, "sqlstate", getattr(atual, "pgcode", None)),
            locais=locais,
        ))
        atual = atual.__cause__ or atual.__context__
    return cadeia

def sql_so_leitura(execute, sql, params, many, context):
    alias = context["connection"].alias
    if alias != connection.alias:
        raise FalhaSonda("OUTRA_CONEXAO")
    s = str(sql).lstrip().upper()
    permitido = (
        s.startswith(("SELECT ", "WITH ", "SHOW ", "SAVEPOINT ",
                      "RELEASE SAVEPOINT ", "ROLLBACK TO SAVEPOINT "))
        or s.startswith("SET SEARCH_PATH")
        or s.startswith("SET SEARCH PATH")
    )
    exigir(permitido, "SQL_NAO_AUTORIZADO")
    try:
        return execute(sql, params, many, context)
    except Exception as exc:
        FALHAS_SQL.append(type(exc).__name__)
        raise FalhaSonda("ERRO_SQL") from exc

def log_estrito(self, level, msg, args, **kwargs):
    if level >= logging.WARNING:
        exc = kwargs.get("exc_info")
        if isinstance(exc, tuple) and isinstance(exc[1], BaseException):
            raise FalhaSonda("LOG_WARNING_OU_ERRO") from exc[1]
        raise FalhaSonda("LOG_WARNING_OU_ERRO")
    # Nao encaminhar logs: handlers podem gravar arquivos.

def engolido_estrito(*args, **kwargs):
    causa = next((x for x in args if isinstance(x, BaseException)), None)
    raise FalhaSonda("ERRO_INTERNO_REGISTRAR_ENGOLIDO") from causa

def chamada_estrita(func):
    def chamar(*args, **kwargs):
        try:
            return func(*args, **kwargs)
        except Exception as exc:
            raise FalhaSonda("ERRO_INTERNO_AUTORIDADE") from exc
    return chamar

def capturar_render(request, template, context=None, *args, **kwargs):
    exigir(template == "ponto/fila_justificativas.html", "TEMPLATE_INESPERADO")
    return context

def executar():
    global PASSO
    from chamados.services import painel as P
    from chamados.services.raiox import creditos_perguntas
    from chamados.services.verbos import (
        particionar_verbos, exige_o_admin, VERBOS_DO_ADMIN,
    )
    from chamados.catalogo.natureza import dono_do_chamado
    from chamados.catalogo import modulos
    from chamados.utils import departamento_do_chamado
    from chamados.juizes import dia_do_fato, tranca_em_lote
    from chamados.services.vigia_fio import (
        ListaDoAdmin, censo, censo_alcance, competencia_do_dia_entregue,
        CAUSAS_ALCANCE,
    )
    from colaboradores.models import Colaborador, Empresa
    from colaboradores.queries import empresas_visiveis
    from core.context_processors import navbar_context
    from core.models import PlacarNavbar
    from ponto import views as PV
    from ponto.janelas import janela_atual
    from ponto.management.commands.e6_oraculo import InsumosDoDono
    from ponto.services.cxr_na_lista import cxr_lavrado
    from ponto.services.gestao_he import pendentes_na_central

    PASSO = "schema_e_empresas"
    exigir(connection.schema_name == "juliani", "SCHEMA_DIFERENTE_DE_JULIANI")
    exigir(
        set(Empresa.objects.filter(pk__in=EMPRESAS)
            .values_list("pk", flat=True)) == set(EMPRESAS),
        "EMPRESA_AUSENTE",
    )
    visiveis = list(empresas_visiveis())
    emitir(
        "ESCOPO",
        empresas=list(EMPRESAS),
        empresas_visiveis_pk=[e.pk for e in visiveis],
        agora=timezone.localtime(AGORA).isoformat(),
        hoje=HOJE.isoformat(),
        parametros="GET vazio; adicionalmente empresa 2, 3 e 4",
    )

    # app/colaboradores/mixins.py:326; app/core/context_processors.py:42
    req = RequestFactory().get("/")
    req.user = SimpleNamespace(
        is_authenticated=True, is_superuser=True, is_staff=True,
    )

    PASSO = "navbar_contador_exibido"
    nav = navbar_context(req)
    badge = int(str(nav["badge_chamados"]))
    registrados = int(str(nav["badge_registrados"]))
    foto = PlacarNavbar.objects.first()
    emitir(
        "NAVBAR_EXIBIDA",
        fonte="app/core/context_processors.py:68; app/templates/base.html:173",
        badge_chamados=badge,
        badge_registrados=registrados,
        lavra_pk=foto.pk if foto else None,
        lavrado_em=(
            timezone.localtime(foto.lavrado_em).isoformat()
            if foto and foto.lavrado_em else None
        ),
        decomposicao_da_lavra=(
            "[nao verificado]: lavra nao conserva ids dos itens"
            if foto else "sem lavra; comparar admin_atual do tenant"
        ),
    )
    emitir(
        "LIMITE_E4B",
        fonte="app/ponto/services/he_pendente_lavrado.py:209",
        cobertura=(
            "competencia corrente e anterior se nao entregue; "
            "nao e todo o historico exportado"
        ),
    )

    PASSO = "alcance_a_e"
    lista = ListaDoAdmin(HOJE, EMPRESAS, agora=AGORA)
    lista._carregar()
    exigir(
        lista._dias is not None and lista._colabs is not None,
        "LISTA_ADMIN_SEM_RESPOSTA",
    )
    with tranca_em_lote():
        antigo = censo(
            EMPRESAS, hoje=HOJE, limite=sys.maxsize, lista=lista,
        )
        alcance = censo_alcance(
            EMPRESAS, hoje=HOJE, limite=0, lista=lista,
        )
    exigir(lista.sem_resposta == 0, "LISTA_ADMIN_FALHOU")
    exigir(
        len(antigo["celulas_do_fio_morto"]) == antigo["fio_morto"],
        "CENSO_ANTIGO_TRUNCADO",
    )
    emitir(
        "CONTROLE_CENSO_ANTIGO",
        fonte="app/chamados/services/vigia_fio.py:449",
        fio_morto=antigo["fio_morto"],
        fio_morto_cobrando=antigo["fio_morto_cobrando"],
        celula_pk=antigo["celulas_do_fio_morto"],
    )

    mortos = [x for x in alcance["alcance_dias"] if x[4] == "sem_porta"]
    exigir(len(mortos) == alcance["fio_morto_alcance"], "ALCANCE_TRUNCADO")
    por_causa = Counter(x[5] for x in mortos)
    for causa in CAUSAS_ALCANCE:
        exigir(
            por_causa[causa] == alcance["alcance_" + causa],
            "CLASSES_NAO_FECHAM",
        )
    exigir(
        set(por_causa) <= set(CAUSAS_ALCANCE),
        "CLASSE_E4B_DESCONHECIDA",
    )
    cols = Colaborador.objects.select_related("empresa").in_bulk(
        {x[1] for x in mortos}
    )
    cxr = cxr_lavrado()
    insumos, exportacoes, universo = {}, {}, {}

    for celula_pk, cid, data, competencia, rota, causa, detalhe, papel in mortos:
        PASSO = "celula_e4b_%s_%s" % (col(cid), data)
        exigir(cid in cols, "COLABORADOR_AUSENTE")
        c = cols[cid]
        exigir(c.empresa_id in EMPRESAS, "ALCANCE_FORA_DA_FROTA")
        dia = dt.date.fromisoformat(data)
        ini, fim = janela_atual(dia, c.empresa)
        comp = "%02d/%d" % (fim.month, fim.year)
        exigir(comp == competencia, "COMPETENCIA_DIVERGENTE")
        exportada = competencia_do_dia_entregue(
            dia, c.empresa, memo=exportacoes,
        )
        exigir(type(exportada) is bool, "EXPORTACAO_INDETERMINADA")

        chave_insumo = (cid, ini)
        if chave_insumo not in insumos:
            # app/escala/services/cadastro_realidade.py:1024
            insumos[chave_insumo] = InsumosDoDono(
                c, ini, fim, HOJE, agora=AGORA,
                sem_motor=True, cxr=cxr(cid, ini),
            )
        dono_dia = insumos[chave_insumo].dono(dia)
        exigir(
            dono_dia in ("ESTRUTURA", "CADASTRO", "BATIDA", None),
            "DONO_DIA_INVALIDO",
        )
        na_lista = lista.dia_na_lista(cid, data)
        chave = (cid, data)
        exigir(chave not in universo, "DIA_E4B_DUPLICADO")
        universo[chave] = dict(
            celula_pk=celula_pk,
            col=col(cid),
            empresa_pk=c.empresa_id,
            data=data,
            classe_e4b=causa,
            detalhe=detalhe,
            papel=papel,
            competencia=comp,
            novembro_2026=(comp == "11/2026"),
            exportada=exportada,
            dono_dia=dono_dia if dono_dia else "[nao verificado]",
            dia_na_lista=na_lista,
            colab_na_lista=lista.colab_na_lista(cid),
        )
        emitir(
            "CELULA_E4B",
            fonte=(
                "app/chamados/services/vigia_fio.py:699; "
                "app/ponto/management/commands/e6_oraculo.py:182"
            ),
            **universo[chave],
        )

    exigir(lista.sem_resposta == 0, "LISTA_ADMIN_FALHOU")
    emitir(
        "UNIVERSO_E4B",
        fonte="app/chamados/services/vigia_fio.py:628",
        total=len(universo),
        por_classe={k: por_causa[k] for k in CAUSAS_ALCANCE},
        competencias=alcance["alcance_competencias"],
        outras_rotas_omitidas=alcance["alcance_dias_omitidos"],
    )
    resumo_e4b = Counter(
        (x["classe_e4b"], x["dono_dia"], x["competencia"], x["exportada"])
        for x in universo.values()
    )
    for (causa, dono, comp, exp), n in sorted(resumo_e4b.items()):
        emitir(
            "E4B_CLASSE_DONO_COMPETENCIA",
            classe_e4b=causa,
            dono_dia=dono,
            competencia=comp,
            novembro_2026=(comp == "11/2026"),
            exportada=exp,
            n=n,
        )
    for causa in CAUSAS_ALCANCE:
        xs = [x for x in universo.values() if x["classe_e4b"] == causa]
        emitir(
            "E4B_EXPORTACAO",
            fonte="app/chamados/services/vigia_fio.py:822",
            classe_e4b=causa,
            total=len(xs),
            nao_exportada=sum(not x["exportada"] for x in xs),
            exportada=sum(x["exportada"] for x in xs),
            novembro_2026_nao_exportada=sum(
                x["novembro_2026"] and not x["exportada"] for x in xs
            ),
            novembro_2026_exportada=sum(
                x["novembro_2026"] and x["exportada"] for x in xs
            ),
        )

    PASSO = "atendimento_itens"
    vivo = P.qs_vivos()
    part = particionar_verbos(vivo, agora=AGORA)
    resp, viva = creditos_perguntas(vivo)
    itens_atendimento = {}

    # Propaga falhas que dia_do_fato capturaria: chamados/juizes.py:115.
    with patch.object(
        modulos, "data_do_chamado",
        chamada_estrita(modulos.data_do_chamado),
    ):
        for ch in vivo.prefetch_related(None).select_related("celula"):
            PASSO = "dia_chamado_pk_%d" % ch.pk
            dia = dia_do_fato(ch)
            if ch.celula_id:
                exigir(
                    ch.celula.colaborador_id == ch.colaborador_id,
                    "CELULA_DE_OUTRO_COLABORADOR",
                )
            itens_atendimento[ch.pk] = dict(
                pk=ch.pk,
                colaborador_id=ch.colaborador_id,
                empresa_pk=ch.colaborador.empresa_id,
                data=dia.isoformat() if dia else None,
                celula_pk=ch.celula_id,
                classe=ch.modulo_origem or "(sem_modulo)",
                classe_rotulo=P._rotulo_origem(ch.modulo_origem),
                verbo=part["mapa"][ch.pk],
                dono_pilha=dono_do_chamado(
                    ch.modulo_origem, ch.pk in resp, ch.pk in viva,
                ),
                dono_departamento=(
                    departamento_do_chamado(ch) or "[nao verificado]"
                ),
                atribuido_a_pk=ch.atribuido_a_id,
            )

    def publicar_populacao(central, recorte, itens, total_fonte):
        global PASSO
        PASSO = "cruzamento_%s_%s" % (central, recorte)
        exigir(len({x["pk"] for x in itens}) == len(itens), "ITEM_DUPLICADO")
        exigir(len(itens) == total_fonte, "CONTADOR_DIFERENTE_DOS_ITENS")
        resumo, quebra, por_e4b = Counter(), Counter(), Counter()
        celulas = set()

        for it in itens:
            chave = (it["colaborador_id"], it["data"])
            e4 = universo.get(chave)
            if it["empresa_pk"] not in EMPRESAS:
                situacao = "fora_recorte_empresas"
            elif it["data"] is None:
                situacao = "vinculo_indeterminado"
            elif e4 is not None:
                situacao = "dentro_e4b"
                celulas.add(chave)
            else:
                situacao = "fora_universo_atual_e4b"
            resumo[situacao] += 1
            ca = e4["classe_e4b"] if e4 else None
            comp = e4["competencia"] if e4 else None
            exp = e4["exportada"] if e4 else None
            chave_quebra = (
                it["classe"], it["verbo"], it["dono_pilha"],
                it["dono_departamento"], it["atribuido_a_pk"],
                situacao, ca, comp, exp,
            )
            quebra[chave_quebra] += 1
            if e4:
                por_e4b[(ca, exp, comp == "11/2026")] += 1
            seguro = {
                k: v for k, v in it.items() if k != "colaborador_id"
            }
            emitir(
                "ITEM_CENTRAL",
                central=central,
                recorte=recorte,
                col=col(it["colaborador_id"]),
                situacao=situacao,
                classe_e4b=ca,
                competencia_e4b=comp,
                exportada=exp,
                **seguro,
            )

        emitir(
            "TOTAL_POPULACAO",
            central=central,
            recorte=recorte,
            total=total_font

[truncado em 20000 de 28704 caracteres]

## 10/11 11:11 · sessao 92aed57ead37 · FIO-E0-V4-E4A-2 · construtor · origem=Code

Cura aplicada no juiz: veredito não acusador só cala quando `dia_encerrado` confirma o fato encerrado. A consulta fica nesse ramo; o contador continua delegando ao juiz. [fabrica_da_celula.py:120](/home/ronald/wt-codex-fio-e0-v4-e4a-2-105940/app/chamados/services/fabrica_da_celula.py:120), chamados/management/commands/disparar_perguntas_competencia.py:115.

Os **7 arquivos tocados** e suas provas:

| Arquivo | Mudança/prova |
|---|---|
| chamados/services/fabrica_da_celula.py:120 | Cura pela juíza existente, sem novo corte por data. |
| chamados/management/commands/disparar_perguntas_competencia.py:25 | Escalares recusados com `TypeError` explícito; `None` preservado. ALVOS corrigido em :215. |
| chamados/tests/test_e4a2_matriz_memoria.py:199 | Concorde em curso conta; noturno em :203; marcos batidos em :210; escalares em :214. |
| chamados/tests/test_e4a2_nunca_bateu.py:310 | Empresa real escalar; mínimo **11** e catálogo vazio/encolhido em :318. |
| core/tests/test_fio_mudo_placar.py:294 | Catálogo vazio/encolhido; mínimo **12** em :301. |
| chamados/tests/test_e4a2_disparar_e_vigia.py:235 | Catálogo vazio/encolhido; mínimo **10** e mensagem específica por mutante em :241. |
| chamados/tests/test_pergunta_do_dia_em_curso.py:297 | Integração noturna 05:50/06:40; concorde lavrada com marcos batidos em :465. Execução Django **[não verificado]**. |

Os corpos dos dois testes congelados foram preservados: chamados/tests/test_pergunta_do_dia_em_curso.py:460 e chamados/tests/test_e4a2_matriz_memoria.py:194.

Executei **28 testes em memória: OK**, incluindo os selos reais dos catálogos e os testes reais de relatório carregados por AST. Entradas: chamados/tests/test_e4a2_matriz_memoria.py:314,336,358 e chamados/tests/test_e4a2_c5_memoria.py:74.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4a2_matriz_memoria chamados.tests.test_e4a2_c5_memoria -v
```

Antes da cura, os REDs reproduziram os zeros indevidos e a fronteira escalar defeituosa; os casos estão em chamados/tests/test_e4a2_matriz_memoria.py:199,203,214. Os seis mutantes novos foram rejeitados pela mensagem pretendida; catálogo em :61 e execução em :358:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4a2_matriz_memoria juiz_cala_concorde_em_curso
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4a2_matriz_memoria juiz_corta_por_data
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4a2_matriz_memoria juiz_ignora_encerramento
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4a2_matriz_memoria juiz_pergunta_sem_falta_na_ata
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4a2_matriz_memoria fronteira_aceita_texto_escalar
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4a2_matriz_memoria rotulo_alvos_volta_a_perguntas
```

Quanto a **quem escreve e quem lê os campos envolvidos**:

| Campo | Escritor → leitor e significado |
|---|---|
| `cel.colaborador`, `cel.data` | Criação pela porta em ponto/portas/celula.py:259,600 → juiz em chamados/services/fabrica_da_celula.py:109,123. `data` ancora o turno, inclusive cruzando meia-noite; ponto/services/cartorio.py:831. |
| `cel.veredito`, `cel.ata` | Lavra em ponto/portas/celula.py:1033,1035 → juiz em chamados/services/fabrica_da_celula.py:120,126,142. |
| `cel.julgada_em` / `julgada_antes` | Carimbo escreve em ponto/portas/celula.py:1065. Cartório captura o anterior em ponto/services/cartorio.py:954 e repassa em :1008; fora desse ato, o juiz usa o persistido, chamados/services/fabrica_da_celula.py:127. |
| `agora`, `hoje` | Borda fornece instante e data civil local em chamados/management/commands/disparar_perguntas_competencia.py:147 e chamados/management/commands/vigiar_fio_morto.py:166. A juíza recebe o instante, ponto/turnos.py:2033. |
| `Empresa.pk` / `empresas` | Criação de Empresa em colaboradores/services/estrutura.py:14 → normalização em chamados/management/commands/disparar_perguntas_competencia.py:25 → ORM em :104 e C5 em :54. Unidade: empresa. |
| `numero`, `esperado`, `alvos`, `fio_mudo_alvos` | Contador produz em chamados/management/commands/disparar_perguntas_competencia.py:122; vigia publica em chamados/management/commands/vigiar_fio_morto.py:233,241; placar compara em core/management/commands/pipeline_placar.py:85. |

**Outros leitores alcançados:** o cartório chama `perguntar_pela_celula` em ponto/services/cartorio.py:1008; o consumidor de eventos chega pelo mesmo caminho em chamados/reconciliador.py:1387 e chamados/services/fabrica_da_celula.py:304. Ambos passam a admitir motivos vencidos de concorde ainda em curso. `motivos_que_o_evento_deixou` também recebe essa decisão e mantém a deduplicação de Pauta DP, chamados/services/fabrica_da_celula.py:245. O vigia e o placar recebem o número resultante, nas referências da tabela.

O placar lê **dia civil do carimbo**, enquanto o juiz recebe **dia do turno e instante**; essa diferença permanece explícita em core/management/commands/pipeline_placar.py:86 e ponto/turnos.py:2033. A chave `fio_mudo_alvos` e o trecho `FIO MUDO: N` permanecem em chamados/management/commands/disparar_perguntas_competencia.py:122,197.

Não executei a integração Django, a suíte de 165 módulos nem a suíte inteira: Django/freezegun ausentes e banco fora do escopo. **[não verificado]** Ruff foi tentado com `--no-cache`, mas o módulo está ausente; lint **[não verificado]**. Medida nova em produção e banco 6 **[não verificado]**. [nao verificado]

## 10/11 11:04 · sessao 53d91d880944 · - · - · origem=Ronald (tmux)

Achados para consolidação:

- A classe da origem mostrada na Central de Atendimento vem de `modulo_origem → painel._rotulo_origem → desenlace.rotulo`; a tela também exibe verbo de ação, proveniente de `particionar_verbos`. (`app/chamados/services/painel.py:36`, `app/chamados/services/painel.py:392`, `app/chamados/services/verbos.py:30`, `app/templates/chamados/partials/_lista_chamados.html:65`)
- Para “dono”, preservar três dimensões distintas: atribuição humana `atribuido_a_id`, mostrada na lista detalhada; pilha operacional de `natureza.dono_do_chamado`; departamento competente de `utils.departamento_do_chamado`. Não chamar `e6_oraculo` para substituir nenhuma dessas autoridades. (`app/chamados/models.py:64`, `app/templates/chamados/partials/_lista_chamados.html:96`, `app/chamados/catalogo/natureza.py:41`, `app/chamados/utils.py:32`)
- Insumos da pilha: `resp, viva = creditos_perguntas(qs_vivos(...))`; então `dono_do_chamado(ch.modulo_origem, ch.pk in resp, ch.pk in viva)`. A própria partição usa esses insumos e `verbo_do_chamado`, que delega ao dono. (`app/chamados/services/raiox.py:51`, `app/chamados/services/raiox.py:151`, `app/chamados/services/verbos.py:34`, `app/chamados/services/verbos.py:47`, `app/chamados/catalogo/natureza.py:72`)
- Vínculo canônico singular do chamado: `(ch.colaborador_id, dia_do_fato(ch))`; a função prioriza `celula`, depois a chave declarada no catálogo e `data_turno`, depois batida/disputa/carimbos. Usar a função singular: `dias_do_fato` lê somente `data_turno` no degrau de contexto, enquanto a singular consulta também a chave do catálogo. (`app/chamados/models.py:38`, `app/chamados/juizes.py:87`, `app/chamados/juizes.py:98`, `app/chamados/juizes.py:113`, `app/chamados/juizes.py:187`)
- `dia_do_fato` captura silenciosamente exceção da consulta ao catálogo; data inválida gera warning, e `fortes=True` devolve `None` antes dos degraus fracos. A sonda deve tratar falha capturada/aviso como incompletude; `None` não prova “fora do E4b”. (`app/chamados/juizes.py:115`, `app/chamados/juizes.py:125`, `app/chamados/juizes.py:127`)
- O placar `ativos` é `abertos + em_analise` sobre `qs_na_fila`; o total da lista vem de outro corte dentro da mesma função. `qs_na_fila` exclui silêncio futuro; a partição marca silêncio passado como `historico`, que a lista padrão exclui. Portanto medir os dois contadores reais, sem afirmar equivalência. (`app/chamados/services/painel.py:189`, `app/chamados/services/painel.py:212`, `app/chamados/services/painel.py:222`, `app/chamados/services/verbos.py:48`, `app/chamados/services/painel.py:328`, `app/chamados/services/painel.py:333`)
- Para obter a lista completa real sem reproduzir filtros: `montar_contexto_painel(..., f_modo='lista')`, seguido de `ctx['paginator'].object_list`; a paginação é montada depois do corte. Evita também o ramo resumo, que chama propositor e captura exceção. (`app/chamados/services/painel.py:333`, `app/chamados/services/painel.py:342`, `app/chamados/services/painel.py:422`, `app/chamados/services/painel.py:518`)
- Justificativas: classe é `j.tipo`/`get_tipo_display`; vínculo é `(j.colaborador_id, j.data_turno)`; pendente significa literalmente “Pendente — aguardando DP”. `aprovada_por` é autor da decisão, não dono atribuído corrente. (`app/templates/ponto/fila_justificativas.html:108`, `app/templates/ponto/fila_justificativas.html:109`, `app/ponto/models.py:131`, `app/ponto/models.py:136`, `app/ponto/models.py:144`, `app/ponto/models.py:160`, `app/templates/ponto/partials/painel_justificativa.html:29`)
- A tela de justificativas não possui helper do contador: o número exibido vem diretamente de `fila_justificativas`, em `Justificativa.objects.filter(status='pendente').count()`, sem filtro de empresa. Para chamar a lógica real, capturar o `render` da view com `RequestFactory` e usuário existente autorizado; não copiar esse queryset como se fosse função da tela. (`app/ponto/views.py:1695`, `app/ponto/views.py:1697`, `app/ponto/views.py:1736`, `app/ponto/views.py:1749`)
- “Central de Chamados” no painel da justificativa é link para `chamados:painel_gestao`, não uma terceira tela. (`app/templates/ponto/partials/painel_justificativa.html:80`)
- Não chamar a view do modal do fio: ela marca followups como lidos antes de montar contexto. (`app/chamados/views.py:877`, `app/chamados/views.py:880`)

## 10/11 11:03 · sessao 807eb8462b58 · - · - · origem=Ronald (tmux)

- O universo E4b das classes **a–e** vem de `vigia_fio.censo_alcance`, com uma linha por dia retido: `[celula_id, colab_id, data_iso, competencia, rota, causa, detalhe, papel]`. A lista inclui **todos** os `rota == 'sem_porta'`, independentemente de `limite`; pode chamar `limite=0` e filtrar essas linhas. Conferir `len(linhas)==fio_morto_alcance` e contagem por causa contra `alcance_a`…`alcance_e`. (`app/chamados/services/vigia_fio.py:542`, `:548`, `:625`, `:628`, `:695`)

- `censo` é outro universo: chamados vivos ligados à célula; sua lista `celulas_do_fio_morto` e `classes_do_fio_morto` sofre truncamento. Se for usado como controle, passar `limite=sys.maxsize` e conferir comprimento contra o contador. Ele não fornece classes a–e. (`app/chamados/services/vigia_fio.py:335`, `:449`, `:454`, `:475`)

- `classificar` é importada de **vigia_carimbo**, e suas classes são `com_registro`, `sem_plantio_nenhum`, `plantio_fora_do_slot`, `slot_nao_derivavel`; não representa a classificação a–e do alcance. (`app/chamados/services/vigia_fio.py:328`, `:463`; `app/chamados/services/vigia_carimbo.py:65`, `:74`, `:105`)

- A célula do alcance é buscada por `(colaborador_id, data)`; há membro legítimo sem célula (`celula_id=None`, causa e, detalhe `sem_celula`). Preservar o par mesmo sem FK. (`app/chamados/services/vigia_fio.py:593`, `:596`, `:667`)

- O juiz de exportação é `folha.export.competencia_entregue`, composto com o juiz de janela em `vigia_fio.competencia_do_dia_entregue(dia, empresa, memo)`. Para rotular competência, usar `janela_atual(dia, empresa)[1].month/year`, nunca mês civil. (`app/chamados/services/vigia_fio.py:822`, `:836`, `:838`, `:843`; `app/folha/export.py:556`)

- O alcance enumera a competência corrente e a anterior somente enquanto esta não foi entregue. Não declarar que o alcance cobre passivo exportado fora dessa enumeração. A corrente entra incondicionalmente, por isso ainda faz sentido consultar o juiz para cada célula. (`app/ponto/services/he_pendente_lavrado.py:209`, `:220`, `:223`; `app/chamados/services/vigia_fio.py:583`)

- O comando real compartilha uma `ListaDoAdmin(hoje, empresas, agora=agora)` e usa `tranca_em_lote()`. Não chamar o comando inteiro: ele grava carimbo em arquivo. (`app/chamados/management/commands/vigiar_fio_morto.py:165`, `:169`, `:173`, `:217`)

- Há erros engolidos: `ListaDoAdmin._carregar` registra e continua sem lista; `cadastro_realidade.lista` registra falhas parciais com `logging.exception`. Para cumprir “erro não conclui”, interceptar `registrar_engolido` e logs de erro com sentinela derivada de `BaseException`, além de conferir que `_dias/_colabs` responderam e `lista.sem_resposta==0`. O `BaseException` evita nova captura por `except Exception`. (`app/chamados/services/vigia_fio.py:205`, `:219`, `:225`, `:250`; `app/escala/services/cadastro_realidade.py:804`, `:807`, `:813`, `:817`, `:877`, `:880`)

- Fonte da classe d: ausência de pergunta viva, dia fora de `dias_na_lista`, colaborador presente na lista; isso não atribui dono ao dia. O dono lavrado da lista vem de `dias_na_lista`, que pode responder `None`; não criar mapa classe→dono. (`app/chamados/services/vigia_fio.py:677`; `app/escala/services/cadastro_realidade.py:1044`, `:1050`; `app/chamados/services/vigia_fio.py:99`)

## 10/11 10:13 · sessao ba7a69ea94fc · FIO-E0-V4-E4A-2 · verificador · origem=Code

**A cura principal está confirmada: FIO MUDO passou a consultar o juiz único. A saída integral da obra ainda não está certificada: há catálogos de mutantes que passam vazios, e os 165 módulos, a suíte inteira e o resultado de produção com a seleção nova permanecem [nao verificado].** (`app/chamados/management/commands/disparar_perguntas_competencia.py:106`; `app/chamados/tests/test_e4a2_nunca_bateu.py:309`; `app/core/tests/test_fio_mudo_placar.py:308`)

**1. MATRIZ: realização e caminhos alternativos**

| Linha | ESTADO → JUIZ → OPERAÇÃO | PERSISTIDO e RETORNO | Prova |
|---|---|---|---|
| **(a)** | Chamado vivo, sem pergunta viva, célula `nunca_bateu`: lê `dia_nunca_bateu_sem_adesao`, encaminha o par ao contador C5. Não recalcula adesão; lê a lâmpada lavrada. `numero=|pares|`, `esperado=|pares∩C5|`, dono CADASTRO. (`app/chamados/management/commands/disparar_perguntas_competencia.py:100`, `:102`, `:58`; `app/chamados/services/fabrica_da_celula.py:87`) | Nenhuma pergunta persistida. O ramo não lança rc 2. (`app/chamados/management/commands/disparar_perguntas_competencia.py:112`, `:208`) | Retido fora da C5 produz `(1,0)`; presente produz `(1,1)`. Teste em memória executado verde. Integração correspondente: [nao verificado]. (`app/chamados/tests/test_e4a2_matriz_memoria.py:188`; `app/chamados/tests/test_e4a2_nunca_bateu.py:218`) |
| **(b)** | Chamado vivo, sem pergunta viva: consulta `motivos_a_perguntar(celula, hoje, agora)`, conta **todos** os motivos, deduplicados por pessoa/dia/motivo. Esperado 0, dono ESTRUTURA. (`app/chamados/management/commands/disparar_perguntas_competencia.py:90`, `:106`, `:113`) | Nenhuma pergunta/questionário persistido; o ramo não lança rc 2. (`app/chamados/management/commands/disparar_perguntas_competencia.py:111`, `:208`) | Alvo sem escrita e alvo 1 tornando placar vermelho: executados verdes em memória. (`app/chamados/tests/test_e4a2_matriz_memoria.py:200`, `:222`) |
| **(c)** | Qualquer pergunta ligada **viva** retira o chamado dos dois contadores. Além disso, o juiz retira individualmente motivos cujo fato já tem pergunta. (`app/chamados/management/commands/disparar_perguntas_competencia.py:100`; `app/chamados/services/fabrica_da_celula.py:145`, `:157`) | Nenhuma escrita; nenhuma contribuição desse chamado aos contadores. (`app/chamados/management/commands/disparar_perguntas_competencia.py:100`) | Teste em memória tem controle positivo sem pergunta. O teste ORM materializa pela porta e confirma vitalidade antes de exigir zero; execução ORM [nao verificado]. (`app/chamados/tests/test_e4a2_matriz_memoria.py:205`; `app/chamados/tests/test_e4a2_nunca_bateu.py:148`) |
| **(d)** | O contador exclui o chamado **já encerrado** pela tranca por meio de `VIVOS`; não consulta diretamente a tranca. (`app/chamados/management/commands/disparar_perguntas_competencia.py:90`) | Nenhuma escrita pelo disparar; chamado encerrado não contribui. (`app/chamados/management/commands/disparar_perguntas_competencia.py:90`) | Memória verifica exclusão por status. Integração tranca pela porta, exige status fora de VIVOS e zero nos dois contadores; execução integrada [nao verificado]. (`app/chamados/tests/test_e4a2_matriz_memoria.py:215`; `app/chamados/tests/test_e4a2_nunca_bateu.py:163`) |

Os caminhos que impedem uma leitura absoluta da tabela são:

- **(a):** hoje, dias anteriores ao piso e desligados ausentes da C5 não entram no número comparado. Portanto, “todo nunca_bateu vira um retido contado” seria falso; o recorte está declarado. (`app/chamados/management/commands/disparar_perguntas_competencia.py:35`, `:51`, `:56`)
- **(b):** `concorde` é recusado pelo juiz; desligado, futuro, dia sem trabalho, parcial e fato existente também podem devolver zero. (`app/chamados/services/fabrica_da_celula.py:112`, `:114`, `:120`, `:123`, `:157`)
- **(c):** materialização isolada não significa pergunta viva. A retratada deixa de segurar o fato e pode voltar a produzir alvo, conforme a errata. (`app/chamados/juizes.py:1325`, `:1336`; `app/chamados/models.py:2754`)
- **(d):** existe intervalo com tranca gravada e encerramento pendente: acima de 100 ações, o encerramento segue em subprocesso; falhas também são tratadas como best-effort. Nesse intervalo, um chamado ainda vivo pode entrar. Isso limita a equivalência entre “trancado” e “fora de VIVOS”, sem transformar o item descartado pela errata em reprovação automática. (`app/ponto/services/fechamento.py:1420`; `app/chamados/services/competencia_trancada.py:45`, `:281`, `:297`)

**O rc global pode ser 2 coexistindo com qualquer dessas classes:** basta `--alarme` e `deixou` não vazio. A origem é o outro vigia, FABRICA-POR-EVENTO. Os testes que isolam rc 0 de (a)/(b) dublam explicitamente esse outro resultado. (`app/chamados/management/commands/disparar_perguntas_competencia.py:208`; `app/chamados/tests/test_e4a2_nunca_bateu.py:128`)

**2. Juiz único, universos e barreiras retiradas**

O contador não reconstrói mais motivos pela ata nem aplica as antigas quatro barreiras para decidir ALVOS: recebe os motivos do juiz e deduplica. Permanecem as seleções anteriores à consulta: chamado vivo, janela, empresa/colaborador, ausência de pergunta viva e separação de `nunca_bateu`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:90`, `:94`, `:100`, `:102`, `:106`)

| Caso | Universo entregue ao juiz |
|---|---|
| Chamado retratado/encerrado | Fora do contador quando o status está fora de VIVOS; o caminho da célula não exige chamado vivo. (`app/chamados/management/commands/disparar_perguntas_competencia.py:90`; `app/ponto/services/cartorio.py:1006`) |
| Célula sem chamado | Não chega pelo contador; pode chegar pelo cartório e pelo consumidor de eventos. (`app/chamados/management/commands/disparar_perguntas_competencia.py:90`; `app/ponto/services/cartorio.py:1008`; `app/chamados/services/fabrica_da_celula.py:299`) |
| Desligado | Pode chegar ao juiz pelo contador, mas o próprio juiz recusa motivos. (`app/chamados/management/commands/disparar_perguntas_competencia.py:106`; `app/chamados/services/fabrica_da_celula.py:112`) |
| Sem adesão | O contador separa pela lâmpada antes do juiz de motivos; a fábrica também retorna antes de emitir. (`app/chamados/management/commands/disparar_perguntas_competencia.py:102`; `app/chamados/services/fabrica_da_celula.py:183`) |
| Competência trancada | Contador depende do encerramento do chamado; `motivos_a_perguntar` não tem guarda própria de tranca. (`app/chamados/management/commands/disparar_perguntas_competencia.py:90`; `app/chamados/services/fabrica_da_celula.py:105`) |

As barreiras **não são todas predicados equivalentes**:

- `desligado` está no juiz; `parcial_lista` consulta o mesmo `parcial_so_lista`; `fato_ja_existe` usa `q_dos_fatos`, fonte compartilhada com `linhas_dos_fatos`. (`app/chamados/services/fabrica_da_celula.py:112`, `:123`, `:145`; `app/chamados/models.py:2743`)
- `ja_viva` tem uma diferença importante: o contador exclui o chamado inteiro por **qualquer** pergunta ligada viva; o juiz deduplica **por motivo**. Com pergunta viva A e motivo B ainda devido, o contador não consulta o juiz, mas a célula pode pedir B. É o recorte “fio mudo”, não igualdade dos universos. (`app/chamados/management/commands/disparar_perguntas_competencia.py:100`; `app/chamados/services/fabrica_da_celula.py:154`, `:157`)
- O parcial usa o mesmo predicado, mas o cartório entrega o julgamento anterior capturado antes do ato; o contador lê o `julgada_em` persistido. Essa distinção de instantâneo está documentada. (`app/ponto/services/cartorio.py:389`, `:954`, `:1008`; `app/chamados/services/fabrica_da_celula.py:123`)

Há outra diferença de significado: os motivos devolvidos pelo juiz podem ser destinados a **Pauta DP do limbo exportado**, em vez de pergunta no aplicativo. FIO MUDO conta esses motivos diretamente; FABRICA-POR-EVENTO desconta as pautas já existentes. Logo, ALVOS não equivale universalmente a “perguntas que o escritor criaria no app”. (`app/chamados/services/fabrica_da_celula.py:214`, `:242`; `app/chamados/management/commands/disparar_perguntas_competencia.py:106`)

No censo dos consumidores examinados, não apareceu leitor de produção dependente de `[BARRADO]`, `FIO DECIDIDO` ou `cfm['barrados']`; vigia e placar consomem os números do contador/carimbo. O teste do relatório agora exige ausência desses textos. (`app/chamados/management/commands/vigiar_fio_morto.py:181`, `:241`; `app/core/management/commands/pipeline_placar.py:98`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:208`)

**3. ERRATA: fronteira de EMPRESA**

Existe **uma implementação comum**, chamada pelos dois contadores. No fluxo aninhado ela é invocada duas vezes; não são duas normalizações independentes. A entrada direta de `contador_nunca_bateu` também passa por ela. (`app/chamados/management/commands/disparar_perguntas_competencia.py:24`, `:44`, `:79`, `:112`)

Comportamentos confirmados em memória pela função real:

| Entrada | Resultado |
|---|---|
| Lista/queryset/iterador de Empresa; lista de pks; `['12']` | Lista de inteiros: `[12]`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:28`) |
| `None`; lista vazia | Respectivamente frota inteira e nenhuma empresa. (`app/chamados/management/commands/disparar_perguntas_competencia.py:26`, `:95`) |
| Empresa escalar; inteiro escalar | `TypeError`: exige iterável. (`app/chamados/management/commands/disparar_perguntas_competencia.py:28`) |
| String escalar `'12'`; `'12,13'` | Respectivamente `[1,2]` e `ValueError`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:28`) |
| Lista contendo pk de colaborador | Aceita como inteiro de empresa; não valida o domínio lógico do pk. (`app/chamados/management/commands/disparar_perguntas_competencia.py:28`) |

Chamadores concretos: o disparar usa `None`; `vigiar_fio_morto --empresa` passa lista de objetos Empresa, corrigida pela fronteira; pipeline só lê carimbo. Os testes passam objetos, pks, strings dentro de lista, iteradores e `None`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:179`; `app/chamados/management/commands/vigiar_fio_morto.py:156`, `:160`, `:181`; `app/core/management/commands/pipeline_placar.py:92`; `app/chamados/tests/test_e4a2_c5_memoria.py:89`; `app/chamados/tests/test_e4a2_matriz_memoria.py:183`, `:188`)

**Distinção necessária:** a C5 crua, `colabs_nunca_bateu`, não usa essa fronteira; espera pks. Seus outros chamadores continuam passando pks: tela usa conjunto, outro comando usa lista de inteiros/None. Portanto, todas as entradas dos **contadores do disparar** são protegidas; todas as chamadas C5 da árvore não passam pelo helper. (`app/escala/services/cadastro_realidade.py:177`, `:207`; `app/escala/views.py:487`; `app/folha/vigia_retido_calado.py:43`; `app/folha/management/commands/vigiar_retido_calado.py:29`)

**4. Contrato de saída e banco 6**

| Banco | Verificação |
|---|---|
| **1** | Realizado: cron com `--alarme`, papel vigia, fora dos juízes; parser rejeita `--apply`. Teste em memória verde. (`app/config/crons.py:1521`, `:2032`, `:2168`; `app/chamados/tests/test_e4a2_matriz_memoria.py:68`) |
| **2** | Realizado no caminho contador → carimbo → placar; alvo 1 gera alarme no teste executado em memória. (`app/chamados/management/commands/vigiar_fio_morto.py:241`; `app/core/management/commands/pipeline_placar.py:135`; `app/chamados/tests/test_e4a2_matriz_memoria.py:222`) |
| **3** | Realizado com pks de empresa e esperado por interseção C5; publicação/comparação passaram em memória. (`app/chamados/management/commands/disparar_perguntas_competencia.py:44`, `:59`; `app/chamados/tests/test_e4a2_c5_memoria.py:145`) |
| **4** | Selo executado verde: rc 0, 12 crons prazo, zero juízes. Exceção real vazia; casos sintéticos continuam cobrando mordida e isolamento. (`bin/tests/test_papel_prazo_nao_deriva.sh:305`, `:384`, `:410`) |
| **5** | 165 módulos e suíte inteira: **[nao verificado]**. | [nao verificado]

O banco 6 anexado contém FIO MUDO/ALVOS **1, depois 0 e 0**; FABRICA-POR-EVENTO aparece separadamente com **83, 111 e 131**. Os horários 06:35 mostrados estão nos warnings, não nas linhas dessas contagens. Assim, o bloco sustenta zeros do contador antigo, mas não vincula inequivocamente cada contagem àquela hora nem ao código novo. Referências abaixo contam as linhas do bloco fornecido. (Anexo `banco6_fio_mudo_0635.txt:1`, `:3`, `:5`, `:7`, `:9`, `:11`, `:13`)

**Não é possível calcular o novo número de produção.** A troca pode retirar falsos alvos como `concorde`, mas também passa a contar todos os motivos: a seleção antiga podia escolher primeiro um motivo já coberto e encerrar aquela passagem sem examinar outro devido. Os agregados anexados não mostram células/motivos para recalcular ou separar (a)/(b). (`app/chamados/tests/test_e4a2_matriz_memoria.py:164`; seleção antiga preservada em `app/chamados/tests/test_e4a2_disparar_e_vigia.py:533`, `:590`; seleção nova em `app/chamados/management/commands/disparar_perguntas_competencia.py:106`; novo total de produção **[nao verificado]**)

**5. Quem escreve, quem lê e significados**

| Campo/valor | Produtor → leitor |
|---|---|
| **rc do `--alarme`** | Disparar produz 2 por `deixou`; `cron_run` registra o exit e aceita 0/2 como conclusão; `crons_quebrados` acusa somente outros códigos. (`app/chamados/management/commands/disparar_perguntas_competencia.py:208`; `bin/cron_run.sh:11`, `:22`, `:32`; `app/core/services/crons_quebrados.py:46`) |
| **VIGIA FIO MUDO / ALVOS / por empresa** | Contador produz motivos distintos; relatório imprime motivos e pessoas distintas. A publicação durável usa o contador novamente pelo vigia. (`app/chamados/management/commands/disparar_perguntas_competencia.py:107`, `:187`, `:193`, `:205`; `app/chamados/management/commands/vigiar_fio_morto.py:181`) |
| **NUNCA_BATEU** | Contador produz pares únicos e esperado pela C5; vigia imprime e grava número/esperado e diagnóstico completo. Totais C5 permanecem informativos. (`app/chamados/management/commands/disparar_perguntas_competencia.py:58`; `app/chamados/management/commands/vigiar_fio_morto.py:210`, `:233`, `:242`) |
| **FABRICA-POR-EVENTO** | Conta motivos faltando pergunta **ou pauta**, além de células distintas. `placar_code.sh` raspa a última contagem. (`app/chamados/management/commands/disparar_perguntas_competencia.py:199`, `:202`; `bin/placar_code.sh:390`) |
| **Carimbo** | Vigia grava atomicamente, somente em frota e antes do rc. Pipeline compara número/esperado; `fio_estado` compara genericamente todos os esperados presentes. (`app/chamados/management/commands/vigiar_fio_morto.py:118`, `:227`, `:252`; `app/core/management/commands/pipeline_placar.py:98`; `bin/fio_estado.py:125`) |
| **Tela C5** | C5 fornece dias; apresentação cria uma assinatura por pessoa com quantidade de dias. Isso é diferente do contador de pares retidos. (`app/escala/services/cadastro_realidade.py:204`, `:208`; `app/chamados/management/commands/disparar_perguntas_competencia.py:58`) |

Divergências concretas que permanecem:

- **rc 2 × A5 OK:** pipeline registra rc 2 como alarme, mas a situação `ALARME` não entra na lista que determina o alarme A5. O novo alarme por carimbo entra explicitamente; são caminhos diferentes. (`app/core/management/commands/pipeline_placar.py:76`, `:124`, `:131`, `:135`, `:163`)
- **Motivo/pauta × pergunta:** o binário continua rotulando FABRICA como `perguntas_que_o_julgamento_nao_fez`, embora a fonte inclua pauta. (`bin/placar_code.sh:392`; `app/chamados/management/commands/disparar_perguntas_competencia.py:202`)
- **Atualidade da linha:** o binário valida mtime do arquivo, não a data da linha raspada; um arquivo atualizado hoje pode conter uma última linha FABRICA antiga. (`bin/placar_code.sh:390`, `:391`)
- **Dia civil × turno:** o carimbo usa data civil local do instante; o universo usa `celula.data`, dia do turno. Pipeline exige o dia civil do carimbo igual ao pedido, mas sua borda usa relógio do processo. Equivalência dos fusos em produção: **[nao verificado]**. (`app/chamados/management/commands/vigiar_fio_morto.py:166`, `:230`; `app/chamados/management/commands/disparar_perguntas_competencia.py:90`; `app/core/management/commands/pipeline_placar.py:95`, `:148`)
- **Esquema antigo:** pipeline denuncia chaves ausentes; `fio_estado` só compara esperados existentes, portanto não exige por si mesmo os novos campos num carimbo antigo. (`app/core/management/commands/pipeline_placar.py:98`, `:102`; `bin/fio_estado.py:123`)

**6. Os testes mordem?** [nao verificado]

**Sim nos dois módulos de memória executados:** 22 testes verdes, incluindo 26 mutantes. Cada mutante exige trecho único, controle verde anterior e falha com mensagem pretendida. Também passaram 11 testes extraídos por AST de fonte/relógio/relatório/SQL e 10 do censo de delegação; isso não constitui execução ORM. (`app/chamados/tests/test_e4a2_matriz_memoria.py:262`; `app/chamados/tests/test_e4a2_c5_memoria.py:207`; `app/chamados/tests/test_e4a2_disparar_e_vigia.py:72`; `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:442`)

O RED `concorde` passou no controle e falhou diretamente com o mutante que volta à ata: **`1 != 0 : concorde rejeitado pelo juiz nao e alvo`**. O teste consulta o juiz real por AST e a tupla real de vereditos. A execução histórica sobre a base exata `1a505ad69` continua **[nao verificado]**. (`app/chamados/tests/test_e4a2_matriz_memoria.py:124`, `:143`, `:164`; mutante em `:51`)

**A cura de vacuidade está incompleta:**

- Matriz e C5 em memória exigem mínimo 13 e testam catálogo vazio/encolhido. (`app/chamados/tests/test_e4a2_matriz_memoria.py:256`, `:263`; `app/chamados/tests/test_e4a2_c5_memoria.py:201`, `:208`)
- Os catálogos ORM de nunca_bateu e placar **não têm mínimo**. Seus corpos reais, executados por AST com `MUTANTES=[]`, passaram vazios. (`app/chamados/tests/test_e4a2_nunca_bateu.py:309`; `app/core/tests/test_fio_mudo_placar.py:308`)
- O catálogo do relatório também não tem mínimo e captura qualquer `AssertionError`, sem exigir a mensagem da asserção pretendida. (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:220`)
- Com entradas presentes, os laços ORM têm controle verde e mensagem pretendida; sua execução integrada permanece **[nao verificado]**. (`app/chamados/tests/test_e4a2_nunca_bateu.py:315`, `:322`; `app/core/tests/test_fio_mudo_placar.py:313`, `:327`)

A troca das fixtures `concorde→furo` preservou contagem, ausência de escrita e retorno, isolando o outro vigia; `concorde` ganhou proteção própria. A retirada dos baldes reduziu o contrato de apresentação antigo, mas os comportamentos continuam verificados no juiz/C5. Não encontrei afrouxamento funcional sem justificativa da matriz nessas alterações. (`app/chamados/tests/test_e4a2_disparar_e_vigia.py:309`; `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:133`; `app/chamados/tests/test_e4a2_matriz_memoria.py:164`, `:176`; `app/chamados/tests/test_e4a2_nunca_bateu.py:239`)

**7. Classes históricas de reprovação**

- **Segundo juiz/derivação paralela de motivos:** curada neste contador. Ele delega à fábrica; a coexistência herdada dos respondedores de `_C19` continua explicitamente declarada no registro, sem migração nesta obra. (`app/chamados/management/commands/disparar_perguntas_competencia.py:106`; `app/core/juizes.py:150`)
- **Segundo escritor fora da porta:** não permanece no disparar examinado; emissão continua pela fábrica e portas existentes. (`app/chamados/management/commands/disparar_perguntas_competencia.py:111`, `:182`, `:208`; `app/chamados/services/fabrica_da_celula.py:165`, `:172`, `:219`)
- **Relógio fora da borda:** disparar lê um instante e o passa à vitalidade e ao juiz; o teto compara com `agora`. A antiga passagem pelo `_ja_perguntou→fio_vivo` saiu desse contador. (`app/chamados/management/commands/disparar_perguntas_competencia.py:137`, `:100`, `:106`; `app/ponto/services/cartorio.py:858`)
- **Selo/RED sem mordida:** o RED principal morde; **a possibilidade de passar

[truncado em 20000 de 20987 caracteres]

_3380 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
