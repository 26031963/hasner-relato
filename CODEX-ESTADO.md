# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 15:05** · publicado em **11/11/2026 15:07** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 15:05 · sessao a0ad56da25bd · PROPORCAO-POR-RISCO · verificador · origem=Code

**APROVADO no delta v6.** A leitura da lista na BASE fecha a autoautorização dentro do intervalo julgado; a confiança na base continua sendo responsabilidade externa ao selo (`bin/selo_proporcao.py:47`, `bin/selo_proporcao.py:78`, `bin/selo_proporcao.py:91`). Bloqueantes comprovados contra a letra B3: nenhum; B3 exige corte do Ronald, mas não exige base igual a `main` nem autenticação automática desse corte (`aval_proporcao_b3_resposta_1111.txt:1`).

Todos os rc abaixo são **deduzidos por leitura**. Execução dos casos, mutantes, vínculo Git da cópia ao hash informado e uso em produção: [nao verificado].

**1) Autoautorização**

O ataque v5, **no mesmo diff e com a base original**, foi fechado: acrescentar escritor e `CORTES.md` à lista de head não altera a lista utilizada. Ambos ficam fora da lista original, produzindo **A/rc 1 quando declarado B**; o novo caso reproduz essa entrada (`bin/selo_proporcao.py:91`, `bin/selo_proporcao.py:60`, `bin/selo_proporcao.py:108`; `bin/tests/test_selo_proporcao.sh:78`).

**Ainda existe o caminho pela escolha de uma base já ampliada.** Contraexemplo mínimo em dois commits, construído para análise; execução: [nao verificado]. [nao verificado]

- **Commit P:** em `app/docs/CORTES.md:444`, inserir ``; `app/chamados/writer.py` `` imediatamente antes do marcador final.
- **Commit H, filho de P:** acrescentar: [nao verificado]

```diff
diff --git a/app/chamados/writer.py b/app/chamados/writer.py
new file mode 100644
--- /dev/null
+++ b/app/chamados/writer.py
@@ -0,0 +1,2 @@
+from chamados.models import ChamadoColaborador
+ChamadoColaborador.objects.update(status_local='fechado')
```

A chamada `bin/selo_proporcao.sh P H B <raiz>` recebe **B/rc 0 deduzido**: P é ancestral, sua lista já contém o escritor, e o diff P→H contém somente esse caminho (`bin/selo_proporcao.py:89`, `bin/selo_proporcao.py:91`, `bin/selo_proporcao.py:60`, `bin/selo_proporcao.py:111`). O campo utilizado existe; efeito dessa entrada em banco: [nao verificado] (`app/chamados/models.py:52`).

Isso **exclui do julgamento o commit que ampliou a lista**. Julgar desde o antecessor de P, com a lista original, inclui `CORTES.md` e o escritor e resulta em A/rc 1 declarado B (`bin/selo_proporcao.py:92`, `bin/selo_proporcao.py:60`, `bin/selo_proporcao.py:108`). Portanto, o contraexemplo demonstra o limite da base escolhida; não reproduz o defeito v5 para o mesmo intervalo (`bin/selo_proporcao.py:91`).

**2) O que a BASE garante**

| Entrada | Resultado deduzido |
|---|---|
| Revisão inexistente ou que não resolve para commit | **2**, na resolução (`bin/selo_proporcao.py:85`, `bin/selo_proporcao.py:98`). |
| Commit forjado, existente e ancestral de head | Pode ser aceito: não há verificação de procedência ou corte (`bin/selo_proporcao.py:84`, `bin/selo_proporcao.py:89`, `bin/selo_proporcao.py:91`). |
| Base diferente de `main` | Pode ser aceita; a validação usa ancestralidade, sem comparação com `main` (`bin/selo_proporcao.py:89`). |
| Commit anterior da própria raia já ampliou a lista | Pode produzir **B/rc 0**, conforme P→H acima (`bin/selo_proporcao.py:91`, `bin/selo_proporcao.py:60`, `bin/selo_proporcao.py:111`). |
| `CORTES.md` ausente na base | **2**: `git show` falha e vira `NaoJulgado`; não há fallback para head ou disco (`bin/selo_proporcao.py:47`, `bin/selo_proporcao.py:41`, `bin/selo_proporcao.py:98`). |

O **chamador escolhe a base**, pelo primeiro argumento; o wrapper repassa os argumentos integralmente (`bin/selo_proporcao.sh:7`, `bin/selo_proporcao.py:78`). No harness, a base normal é o commit sintético inicial; nos casos de lista torta, é o commit sintético que contém a deformação (`bin/tests/test_selo_proporcao.sh:29`, `bin/tests/test_selo_proporcao.sh:47`, `bin/tests/test_selo_proporcao.sh:50`). Quem escolhe e valida a base da obra real: [nao verificado].

O selo comprova resolução para commit, desigualdade, ancestralidade, lista formalmente válida e diff não vazio. **Não comprova corte do Ronald nem que o intervalo abrange a obra inteira**, cuja unidade declarada é “obra” (`bin/selo_proporcao.py:85`, `bin/selo_proporcao.py:87`, `bin/selo_proporcao.py:89`, `bin/selo_proporcao.py:91`, `bin/selo_proporcao.py:94`; `app/docs/CORTES.md:443`, `app/docs/CORTES.md:444`).

**3) Os oito `caso_lista`**

Por leitura, **nenhum dos oito foi deslocado para rc 2 por falha de preparo**: todos alteram a árvore inicial, `prepara` exige sucesso e commit novo, e o head seguinte modifica o template inicial de `<p>a</p>` para `z` (`bin/tests/test_selo_proporcao.sh:24`, `bin/tests/test_selo_proporcao.sh:31`, `bin/tests/test_selo_proporcao.sh:46`, `bin/tests/test_selo_proporcao.sh:48`).

| Caso | Motivo previsto de rc 2 |
|---|---|
| `sem_marcador` | Contagem dos marcadores (`bin/tests/test_selo_proporcao.sh:92`; `bin/selo_proporcao.py:48`). |
| `lista_vazia` | Item vazio, guarda de forma (`bin/tests/test_selo_proporcao.sh:93`; `bin/selo_proporcao.py:51`, `bin/selo_proporcao.py:53`). |
| `so_inicio` | Falta marcador final (`bin/tests/test_selo_proporcao.sh:94`; `bin/selo_proporcao.py:48`). |
| `fim_antes` | Cardinalidade válida; fatia vazia gera item inválido (`bin/tests/test_selo_proporcao.sh:95`; `bin/selo_proporcao.py:50`, `bin/selo_proporcao.py:53`). |
| `marcador_duplo` | Dois exemplares de cada marcador (`bin/tests/test_selo_proporcao.sh:96`; `bin/selo_proporcao.py:48`). |
| `item_sem_crase` | Forma do item (`bin/tests/test_selo_proporcao.sh:97`; `bin/selo_proporcao.py:53`). |
| `item_vazio` | Item vazio entre separadores (`bin/tests/test_selo_proporcao.sh:98`; `bin/selo_proporcao.py:51`, `bin/selo_proporcao.py:53`). |
| `cortes_apagado` | Falha de `git show`, pela guarda `git_rc` (`bin/tests/test_selo_proporcao.sh:99`; `bin/selo_proporcao.py:47`, `bin/selo_proporcao.py:41`). |

Falha no preparo é registrada como falha do harness e retorna **antes de chamar o selo**; não é aceita como rc 2 esperado (`bin/tests/test_selo_proporcao.sh:46`, `bin/tests/test_selo_proporcao.sh:49`). Entretanto, a chamada descarta a saída e compara somente rc; ela **não verifica dinamicamente o motivo** (`bin/tests/test_selo_proporcao.sh:50`).

**4) QUEM ESCREVE e QUEM LÊ**

| Objeto | Escritor/produtor | Leitor/consumidor |
|---|---|---|
| Lista autorizada | Autoridade normativa: **Ronald**, por corte; quem materializou a edição atual: [nao verificado] (`app/docs/CORTES.md:444`). | `lista_fechada`, por `git show <base>:app/docs/CORTES.md`; `motivo_b` consulta pertinência exata (`bin/selo_proporcao.py:47`, `bin/selo_proporcao.py:60`). |
| Lista dos testes | `lista()` escreve o documento; `base()` fornece os itens sintéticos; os casos o deformam (`bin/tests/test_selo_proporcao.sh:17`, `bin/tests/test_selo_proporcao.sh:22`, `bin/tests/test_selo_proporcao.sh:92`). | O mesmo selo, chamado pelo harness (`bin/tests/test_selo_proporcao.sh:41`, `bin/tests/test_selo_proporcao.sh:50`). |
| rc do selo | Python calcula 0/1/2 e emite 10/11/12; wrapper converte 10→0, 11→1 e demais→2 (`bin/selo_proporcao.py:108`, `bin/selo_proporcao.py:111`, `bin/selo_proporcao.py:121`; `bin/selo_proporcao.sh:8`). | Harness captura e compara; régua lê o rc do **harness**, executado por glob (`bin/tests/test_selo_proporcao.sh:41`, `bin/tests/test_selo_proporcao.sh:42`, `bin/tests/test_selo_proporcao.sh:50`; `bin/regua.sh:138`, `bin/regua.sh:140`). |

**Rc 0 significa julgamento concluído**, inclusive para declaração A com arquivos fora; não significa necessariamente classe B (`bin/selo_proporcao.py:105`, `bin/selo_proporcao.py:108`, `bin/selo_proporcao.py:111`). Chamador e leitor do rc sobre a obra real: [nao verificado].

**5) B3, bloqueantes e fila**

O delta mantém classificação por caminho e congela a lista antes do intervalo julgado; isso é compatível com a lista fechada e a exigência de corte do Ronald. A anterioridade do commit, isoladamente, não prova esse corte (`bin/selo_proporcao.py:47`, `bin/selo_proporcao.py:91`; `aval_proporcao_b3_resposta_1111.txt:1`). **Bloqueantes do delta: nenhum demonstrado por essas entradas** (`bin/tests/test_selo_proporcao.sh:78`, `bin/selo_proporcao.py:91`, `bin/selo_proporcao.py:108`).

Fila separada:

- Delimitar “A obra não se autoriza” à base autorizada e ao intervalo completo; a frase atual é mais ampla que a garantia implementada (`bin/selo_proporcao.py:20`, `bin/selo_proporcao.py:78`, `bin/selo_proporcao.py:91`; `app/docs/CORTES.md:443`).
- Corrigir o comentário remanescente “revisao head” (`bin/tests/test_selo_proporcao.sh:91`).
- A alegação “cada guarda [...] caso que só ela pega” continua excessiva: `marcador_duplo` duplica ambos os marcadores, sem isolá-los, e `caso_lista` só verifica rc (`bin/tests/test_selo_proporcao.sh:5`, `bin/tests/test_selo_proporcao.sh:96`, `bin/tests/test_selo_proporcao.sh:50`).
- A cobertura da exceção Python inesperada permanece sem demonstração: os stubs substituem o interpretador e não atravessam essa fronteira (`bin/tests/test_selo_proporcao.sh:145`, `bin/tests/test_selo_proporcao.sh:148`; `bin/selo_proporcao.py:118`, `bin/selo_proporcao.py:120`).

## 11/11 15:03 · sessao 23648d129730 · - · - · origem=Ronald (tmux)

**Eu aprovaria a cura estrita do delta, com limite explícito.** A v6 impede que alterações posteriores à base ampliem a lista usada naquele julgamento (`bin/selo_proporcao.py:47`, `bin/selo_proporcao.py:91`).

O contraexemplo P→H recebe B/rc 0 se P já autoriza o escritor; isso demonstra que o selo **não comprova corte do Ronald nem que o intervalo representa a obra inteira** (`bin/selo_proporcao.py:60`, `bin/selo_proporcao.py:89`, `bin/selo_proporcao.py:111`). O chamador fornece a base; chamador real e validação externa desse início: [nao verificado] (`bin/selo_proporcao.sh:7`, `bin/selo_proporcao.py:78`).

B3 exige corte do Ronald, mas não exige `main` nem autenticação automática desse corte (`app/docs/CORTES.md:444`). Portanto, classificar essa ausência isoladamente como bloqueante acrescentaria uma obrigação não expressa. A frase “A obra nao se autoriza” merece delimitação: vale para o intervalo fornecido, condicionado a uma base autorizada; sua validade incondicional não está demonstrada (`bin/selo_proporcao.py:20`, `app/docs/CORTES.md:443`).

## 11/11 15:02 · sessao 23648d129730 · - · - · origem=Ronald (tmux)

- **Autoridade normativa da lista:** módulo novo só por corte do Ronald; lista atual contém `app/chamados/services/verbos.py` e `app/chamados/templatetags/chamados_tags.py` (`app/docs/CORTES.md:444`). Quem materializou essa edição: [nao verificado].
- **Escritor automatizado localizado:** helper `lista()` do harness escreve `app/docs/CORTES.md` sintético; `base()` fornece os dois módulos sintéticos (`bin/tests/test_selo_proporcao.sh:15`, `bin/tests/test_selo_proporcao.sh:17`, `bin/tests/test_selo_proporcao.sh:22`). Os casos alteram esse documento sintético, inclusive autoautorização e listas tortas (`bin/tests/test_selo_proporcao.sh:77`, `bin/tests/test_selo_proporcao.sh:78`, `bin/tests/test_selo_proporcao.sh:92`).
- **Leitor:** `lista_fechada(repo, base)` faz `git show <base>:app/docs/CORTES.md`; valida apenas cardinalidade dos marcadores e forma dos itens (`bin/selo_proporcao.py:46`, `bin/selo_proporcao.py:47`, `bin/selo_proporcao.py:48`, `bin/selo_proporcao.py:53`). Não há conferência de autor, corte, identidade de `main` ou origem confiável na sequência de validação (`bin/selo_proporcao.py:78`, `bin/selo_proporcao.py:84`, `bin/selo_proporcao.py:89`, `bin/selo_proporcao.py:91`).
- **Quem escolhe base:** o chamador entrega o primeiro argumento; wrapper apenas repassa `"$@"`, e Python o recebe como `base` (`bin/selo_proporcao.sh:7`, `bin/selo_proporcao.py:78`). No harness, `BASE` é o commit inicial sintético; `caso_lista` escolhe como `b` o commit sintético com a lista torta (`bin/tests/test_selo_proporcao.sh:29`, `bin/tests/test_selo_proporcao.sh:47`, `bin/tests/test_selo_proporcao.sh:50`). Pessoa/processo que escolhe a base da obra real: [nao verificado].
- **Garantia efetiva sobre base:** precisa resolver para commit, ser diferente de head e ancestral dele; diff deve ser não vazio; lista precisa existir e ser formalmente válida (`bin/selo_proporcao.py:85`, `bin/selo_proporcao.py:87`, `bin/selo_proporcao.py:89`, `bin/selo_proporcao.py:94`, `bin/selo_proporcao.py:47`). Base anterior da própria raia com lista ampliada satisfaz esses controles; não prova corte do Ronald nem que o intervalo corresponde à obra inteira (`bin/selo_proporcao.py:84`, `bin/selo_proporcao.py:89`, `bin/selo_proporcao.py:91`, `app/docs/CORTES.md:443`, `app/docs/CORTES.md:444`).
- **Quem escreve rc:** Python decide 0/1/2 e emite protocolo privado 10/11/12; wrapper traduz 10→0, 11→1, demais→2 (`bin/selo_proporcao.py:98`, `bin/selo_proporcao.py:108`, `bin/selo_proporcao.py:111`, `bin/selo_proporcao.py:120`, `bin/selo_proporcao.py:121`, `bin/selo_proporcao.sh:8`).
- **Quem lê rc:** `caso()` captura e compara, `caso_lista()` exige 2 e `rcde()` devolve o rc capturado (`bin/tests/test_selo_proporcao.sh:41`, `bin/tests/test_selo_proporcao.sh:42`, `bin/tests/test_selo_proporcao.sh:50`, `bin/tests/test_selo_proporcao.sh:108`). A régua executa o harness por glob e recusa se ele falhar (`bin/regua.sh:138`, `bin/regua.sh:140`, `bin/regua.sh:143`). Isso verifica repos sintéticos criados pelo harness (`bin/tests/test_selo_proporcao.sh:11`, `bin/tests/test_selo_proporcao.sh:13`). Leitor do rc do selo sobre base/head reais: [nao verificado].
- **Prosa obsoleta:** comentário ainda diz “revisao head” imediatamente antes dos oito casos movidos para base (`bin/tests/test_selo_proporcao.sh:91`).

## 11/11 15:01 · sessao 08292a1b6f3e · - · - · origem=Ronald (tmux)

Testes e migrações: **151 locais leitores únicos, 154 nós AST**, com lista e funções já enviadas. A migração filtra `conversa=False`; os testes verificam campos, snapshots e filtros de fixtures. (`app/chamados/migrations/0053_pergunta_unica_por_familia.py:32`, `app/chamados/tests/test_e1_v15.py:79`)

Escritas/entradas de fixtures: **420 locais**, mais **3 dicts de fixtures JSON externos**, total **423**. Há sobreposição leitor/escritor em `app/chamados/tests/test_e1_v5_portas_backfill.py:193`.

`Pergunta` em `test_e4_expand` é um mock derivado de `SimpleNamespace`, incluído como provável interface de PerguntaDisputa. (`app/chamados/tests/test_e4_expand.py:12`, `app/chamados/tests/test_e4_expand.py:62`, `app/chamados/tests/test_e4_expand.py:160`)

Leitor externo relevante: `fila_de_validacao` transforma `i.get('data_evento')` em `data_do_caso`. (`mensageria/nucleo/ferramentas.py:127`)

Outro modelo Django com campo `data_evento`: não encontrado pela varredura AST; a declaração identificada é PerguntaDisputa. (`app/chamados/models.py:2423`)

## 11/11 14:59 · sessao 59a166002cbf · PROPORCAO-POR-RISCO · verificador · origem=Code

**Há um bloqueante: a lista pode autorizar a alteração de si própria e de um escritor no mesmo diff, produzindo B/rc 0.** A pertinência à lista precede qualquer outra classificação (`bin/selo_proporcao.py:46`, `:52`, `:59`, `:107`).

Os rc abaixo são deduzidos por leitura. Resultados de execução, testes e mutantes: [nao verificado]. Referências aos anexos usam as linhas fornecidas no enunciado. [nao verificado]

1. **Bloqueantes dos dois vereditos v4**

| Bloqueante | Situação na v5 | Cura e teste que a cobre | [nao verificado]
|---|---|---|
| Opus B1 — property/outro receptor e método consumido antes de escrita | Deixa de exigir análise de consumo: arquivo fora dos conjuntos implica A; arquivo já autorizado passa pelo caminho. Isso corresponde à substituição por CAMINHO da B3 (`aval_proporcao_b3_resposta_1111.txt:1`). | Classificação em `bin/selo_proporcao.py:57`, `:107`; caso `py_fora`, `bin/tests/test_selo_proporcao.sh:58`. Não há reprodução específica da property nesse teste. |
| Opus B2 — dois saltos | Mesmo tratamento: quantidade de saltos deixou de ser critério (`aval_proporcao_b3_resposta_1111.txt:1`; `bin/selo_proporcao.py:57`). | `py_fora` cobre a recusa genérica por caminho, não os dois saltos: `bin/tests/test_selo_proporcao.sh:58`. |
| Opus B3 — atribuir fuga entre módulos ao “custo aceito” | A atribuição antiga foi corrigida: cabeçalho e contrato dizem expressamente que essa fuga **não** integra o custo aceito (`bin/selo_proporcao.py:20`; `v5_contrato.txt:1`). | `py_fora` e `py_fora_com_b` cobrem arquivos externos aos conjuntos (`bin/tests/test_selo_proporcao.sh:58`, `:59`). Fidelidade da prosa não é verificada pelo harness, que compara rc (`:41`, `:42`). |
| Fable BL1 — repo implícito, igualdade e diff vazio verdes | Fechado no código: repo obrigatório, raiz conferida, igualdade e diff vazio recusados (`bin/selo_proporcao.py:74`, `:80`, `:86`, `:93`). | `sem_repo`, `sem_repo_no_repo_do_script`, `repo_subdir`, `repo_nao_git`, `base_igual_head`, `diff_vazio` (`bin/tests/test_selo_proporcao.sh:104`, `:111`, `:114`, `:116`, `:119`, `:121`). O controle no repo do script é condicional à existência das revisões (`:116`). |
| Fable BL2 — revisões invertidas e exclusões ignoradas | Fechado pelo controle de ancestralidade e pela classificação de todos os nomes, incluindo exclusões; não se lê mais apenas o conteúdo novo (`bin/selo_proporcao.py:88`, `:91`, `:99`). | `revisoes_trocadas`, `nao_ancestral`, `apagou_fora` e renames nos dois sentidos (`bin/tests/test_selo_proporcao.sh:65`, `:66`, `:67`, `:103`, `:108`). Não há caso específico de migration apagada na v5. |

2. **Escapes: separar os aceitos por CAMINHO do defeito da lista**

Nas condições válidas de argumentos, revisões e lista, cada diff abaixo recebe **rc 0 declarado B**: o conteúdo não é inspecionado (`bin/selo_proporcao.py:90`, `:91`, `:100`, `:107`).

| Arquivo e diff mínimo descrito | Escrita alcançada | Texto literal da B3 |
|---|---|---|
| `app/chamados/services/verbos.py`: inserir `vivo_qs.update(status_local='fechado')` antes da linha 34. | A função recebe o queryset e é chamada pelo painel (`app/chamados/services/verbos.py:30`; `app/chamados/services/painel.py:230`); o campo persistido existe (`app/chamados/models.py:52`). | **ACEITA pelo caminho já listado**, sem análise de AST (`app/docs/CORTES.md:444`; `bin/selo_proporcao.py:59`). |
| `app/chamados/templatetags/chamados_tags.py`: antes do retorno de `area_label`, inserir `chamado.status_local='fechado'; chamado.save(update_fields=['status_local'])`. | O filtro recebe o chamado e tem consumidor no template (`app/chamados/templatetags/chamados_tags.py:34`; `app/templates/chamados/detalhe_local.html:56`). | **ACEITA pelo caminho já listado** (`app/docs/CORTES.md:444`; `bin/selo_proporcao.py:59`). |
| `app/static/pwa.js`: trocar `tipo, timestamp: tsCorrigido` por `tipo: 'saida', timestamp: tsCorrigido`. | Altera a batida gravada em IndexedDB e posteriormente enviada por POST (`app/static/pwa.js:112`, `:114`, `:144`, `:153`). | **ACEITA por `static/`** (`bin/selo_proporcao.py:63`; `aval_proporcao_b3_resposta_1111.txt:1`). |
| `app/templates/chamados/partials/_js_cobranca.html`: substituir a chamada de confirmação da linha 60 por `_go();`. | Antecipa o POST de cobrança à confirmação; a view pode registrar a ocorrência após envio bem-sucedido (`app/templates/chamados/partials/_js_cobranca.html:51`, `:60`; `app/chamados/views_cobrar.py:54`). | **ACEITA por `templates/`** (`bin/selo_proporcao.py:61`; `aval_proporcao_b3_resposta_1111.txt:1`). |
| `app/core/tests/test_contract_tblib_no_paralelo.py`: inserir no início de `tblib_instalado` o import de `ChamadoColaborador` e `ChamadoColaborador.objects.update(status_local='fechado')`. | Há consumidor em management command, apesar da localização do helper em testes (`app/core/tests/test_contract_tblib_no_paralelo.py:24`; `app/chamados/management/commands/tblib_ausente_na_imagem.py:17`). | **ACEITA pela identificação de teste por caminho/nome** (`bin/selo_proporcao.py:65`, `:68`; `aval_proporcao_b3_resposta_1111.txt:1`). |

Esses exemplos não exigem perseguir reflexão, alias ou saltos: a B3 determina “O selo checa CAMINHO, nao AST” (`aval_proporcao_b3_resposta_1111.txt:1`). Efeito efetivamente observado em produção: [nao verificado].

**O escape da autoautorização é diferente e é defeito do selo.** Diff mínimo descrito:

- Na lista de `app/docs/CORTES.md:444`, acrescentar, antes do marcador final: ``; `app/chamados/writer.py`; `app/docs/CORTES.md` ``.
- Acrescentar `app/chamados/writer.py` com:

```python
from chamados.models import ChamadoColaborador
ChamadoColaborador.objects.update(status_local='fechado')
```

O parser aceita ambos os itens; tanto o novo escritor quanto o documento alterado pertencem à lista lida de head. Portanto `fora=[]` e o rc previsto é **0**, mesmo declarado B (`bin/selo_proporcao.py:46`, `:52`, `:54`, `:59`, `:104`, `:110`).

Isso contraria “entra modulo novo so por corte meu” e a própria promessa “A obra nao se autoriza” (`aval_proporcao_b3_resposta_1111.txt:1`; `bin/selo_proporcao.py:19`). O caso `lista_se_autoriza` acrescenta somente o módulo; deixa `CORTES.md` fora e não morde esse escape (`bin/tests/test_selo_proporcao.sh:69`).

3. **Quem escreve e quem lê a lista**

A autorização para entrada de módulo novo pertence a Ronald; a lista presente na cópia contém `app/chamados/services/verbos.py` e `app/chamados/templatetags/chamados_tags.py` (`app/docs/CORTES.md:444`). Quem materializou essa edição e sua inclusão efetiva no hash informado: [nao verificado].

O leitor é `lista_fechada`: usa `git show <head>:app/docs/CORTES.md`, exige os dois marcadores únicos e verifica apenas a forma craseada dos itens. Não verifica autor, corte, extensão `.py` nem identidade da linha B3 (`bin/selo_proporcao.py:45`, `:47`, `:52`, `:54`).

Acrescentar **somente** o módulo deixa `CORTES.md` fora dos conjuntos e resulta em rc 1; acrescentar também `CORTES.md` permite a autoautorização descrita acima (`bin/tests/test_selo_proporcao.sh:69`; `bin/selo_proporcao.py:59`, `:107`).

4. **Retornos**

| Condição | Resultado deduzido |
|---|---|
| Declaração B com algum arquivo fora | **1**; é a única condição de retorno lógico 1 (`bin/selo_proporcao.py:107`). |
| Declaração A, com julgamento válido | **0**, inclusive com arquivos fora (`bin/selo_proporcao.py:107`, `:110`). |
| Argumentos inválidos; repo sem Git ou que não seja raiz; revisão inválida | **2** (`bin/selo_proporcao.py:74`, `:79`, `:80`, `:84`, `:97`). |
| `base==head`; base não ancestral; commits distintos com diff vazio | **2** (`bin/selo_proporcao.py:86`, `:88`, `:93`). |
| Lista ausente, vazia ou malformada | **2** (`bin/selo_proporcao.py:46`, `:47`, `:52`, `:97`). |
| Exceção inesperada na implementação Python | **2**, pela fronteira externa (`bin/selo_proporcao.py:117`, `:119`). |
| `GIT_*` herdado | Removido do ambiente de cada subprocesso Git; o harness também o limpa antes de criar seus repos (`bin/selo_proporcao.py:38`; `bin/tests/test_selo_proporcao.sh:6`, `:131`). |

“Repo errado” precisa de distinção: um subdiretório ou diretório sem Git dá 2; **outra raiz Git válida**, contendo as revisões fornecidas, será julgada normalmente. Não há comparação com uma identidade esperada do projeto (`bin/selo_proporcao.py:77`, `:79`, `:84`).

O wrapper preserva os resultados públicos **0/1/2**, traduzindo **10→0, 11→1 e qualquer outro código→2**. Não repassa arbitrariamente o código bruto do Python (`bin/selo_proporcao.sh:7`, `:8`, `:9`, `:10`, `:11`). O harness exige rc 1 também sob `bash -e` (`bin/tests/test_selo_proporcao.sh:134`).

5. **Testes com mecanismos sobrepostos e mutantes** [nao verificado]

- `marcador_duplo` duplica **os dois marcadores**. Remover só a checagem de cardinalidade de início em `bin/selo_proporcao.py:47` deixa a checagem de fim rejeitando o caso; o mutante simétrico também escapa dessa prova. Ausência do marcador ainda acaba em exceção normalizada para 2 (`bin/tests/test_selo_proporcao.sh:83`, `:85`, `:87`; `bin/selo_proporcao.py:49`, `:117`). Sobrevivência por execução: [nao verificado].
- `misto_b` usa `tests/test_b.py`, elegível por diretório **e** basename. Há controles separados: `tests_dir` usa `fixt.py`, enquanto `test_basename` fica fora de `tests/` (`bin/tests/test_selo_proporcao.sh:50`, `:51`, `:54`).
- `migration` prova apenas a classificação genérica de `.py` fora dos conjuntos; não existe mecanismo específico de migration na v5. `apagou_fora` cobre exclusão pelo mesmo mecanismo (`bin/tests/test_selo_proporcao.sh:63`, `:65`; `bin/selo_proporcao.py:57`, `:91`).
- O preparo de `git_dir_herdado` cria no outro repo um único commit e lista inválida. Prova resistência ao desvio que causaria erro; não compara dois diffs válidos, um A e outro B, como sugere o comentário (`bin/tests/test_selo_proporcao.sh:128`, `:129`, `:130`, `:131`).

**O problema de cobertura do antigo F2 permanece:** o mutante mínimo `bin/selo_proporcao.py:119`, `rc = 2` → `rc = 0`, tem sobrevivência dedutível. Os casos normais de erro exercitam argumentos e `NaoJulgado`; os stubs substituem o interpretador e não atravessam essa fronteira Python (`bin/selo_proporcao.py:76`, `:95`, `:117`; `bin/tests/test_selo_proporcao.sh:83`, `:122`, `:136`). Execução do mutante: [nao verificado].

A exigência de **um** marcador de cada tipo e de normalizar **qualquer** exceção, portanto, não fica individualmente demonstrada pela afirmação “cada guarda [...] tem caso que so ela pega” (`bin/tests/test_selo_proporcao.sh:5`; `bin/selo_proporcao.py:47`, `:117`).

6. **QUEM CHAMA, QUEM LÊ e atribuições ao aval**

O chamador concreto do selo localizado é o harness: define `SELO`, chama, captura e compara o rc (`bin/tests/test_selo_proporcao.sh:9`, `:41`, `:42`). O harness tem chamador por glob: `bin/regua.sh` executa `tests/test_*.sh` e recusa a régua se algum falhar (`bin/regua.sh:138`, `:140`, `:143`).

Essa cadeia julga repositórios **sintéticos**, criados pelo teste (`bin/tests/test_selo_proporcao.sh:11`, `:13`). Chamador e leitor de rc sobre base/head da obra real: [nao verificado]. O contrato atribui essa responsabilidade ao pouso B, mas isso é declaração de conduta (`v5_contrato.txt:2`, `:3`).

Quanto aos demais registros v4:

- A exclusão antes ignorada agora entra na classificação por nomes; o teste específico de migration apagada foi substituído pela cobertura genérica de exclusão (`bin/selo_proporcao.py:91`; `bin/tests/test_selo_proporcao.sh:65`).
- O wrapper agora usa `-B`; remoção e situação de versionamento do antigo `bin/__pycache__`: [nao verificado] (`bin/selo_proporcao.sh:7`).
- A frase e a lista constam da cópia (`app/docs/CORTES.md:443`, `:444`). Contrato PROPORCAO no RELATO e commit dessas alterações: [nao verificado].

O cabeçalho chama de “literal” a paráfrase **“qualquer outro arquivo”**; o aval diz **“Qualquer outro .py”**. A consequência para outros arquivos continua derivável da condição anterior sobre **TODO arquivo**, mas a paráfrase não é citação literal (`bin/selo_proporcao.py:10`, `:11`; `aval_proporcao_b3_resposta_1111.txt:1`).

Atribuir os escritores escondidos ao smoke/fila também vai além do que a B3 afirma expressamente. O aval original exige smoke de clique para B; não promete que esse smoke detectará esses escritores (`bin/selo_proporcao.py:21`; `v5_contrato.txt:4`; `aval_proporcao_por_risco_1111.txt:1`). Essa capacidade do smoke: [nao verificado].

7. **`.md` fora dos conjuntos é A pelo texto literal**

A frase decisiva é: **“CLASSE B so se TODO arquivo do diff estiver em templates/, static/, arquivo de teste, ou numa LISTA FECHADA de modulos de leitura declarada no CORTES.md”** (`aval_proporcao_b3_resposta_1111.txt:1`). [nao verificado]

Logo, `.md` fora desses conjuntos é **A**; a frase seguinte sobre `.py` não concede exceção aos documentos. A implementação e `doc_md` seguem essa leitura (`bin/selo_proporcao.py:70`, `:107`; `bin/tests/test_selo_proporcao.sh:60`).

L-106 exige docs no mesmo commit do marco; quando esses documentos ficam fora dos conjuntos, o diff completo recebe A (`CLAUDE.md:332`, `:333`; `bin/selo_proporcao.py:99`, `:104`). A afirmação contratual “qualquer .md [...] faz a obra A” é ampla demais: um `.md` sob `templates/` ou `static/` passa pelo caminho, e o defeito demonstrado permite listar o próprio `CORTES.md` (`v5_contrato.txt:4`; `bin/selo_proporcao.py:59`, `:61`, `:63`).

REPROVADO — bloqueante: autoautorização da obra ao incluir o próprio `CORTES.md` e um escritor na lista de head, obtendo B/rc 0 (`bin/selo_proporcao.py:46`, `:52`, `:59`, `:110`).

## 11/11 14:57 · sessao 6e228e64f5a4 · - · - · origem=Ronald (tmux)

Recorte: Python fora de `app/chamados/**`, sem testes/migrações. **16 linhas leitoras, 21 nós de leitura; 0 escritores; 0 colisões com outro modelo encontradas** nas referências abaixo. “Sim” compara o valor civil não nulo à resposta `None` do juiz para conversa; ocorrência concreta em produção permanece [nao verificado]. Juiz: `app/chamados/juizes.py:334`, `app/chamados/juizes.py:338`.

| Arquivo:linha | Função | Uso | Conversa diverge? |
|---|---|---|---|
| `app/api/views.py:1165` | `api_chamados_v2` | Serializa ISO em `perguntas_disputa[].data_evento`; dois acessos no ternário. | Sim, só repassa |
| `app/api/views.py:1166` | `api_chamados_v2` | Formata `data_evento_label`; dois acessos no ternário. | Sim |
| `app/api/views.py:2637` | `api_disputa_responder` | Insere valor cru em mensagem de auditoria `dia=%s`. | Sim, só repassa |
| `app/api/views_mensageria.py:568` | `_item_validar` | Serializa ISO na fila de validação; dois acessos no ternário. | Sim, só repassa |
| `app/colaboradores/services/calendario.py:473` | `contexto_calendario` | Filtra intervalo de perguntas para indexação de pergunta viva por dia. | Sim |
| `app/colaboradores/services/calendario.py:477` | `contexto_calendario` | Indexa `_perg_viva` pelo dia cru; usado na disponibilidade de “Resolver dia”. | Sim |
| `app/folha/previa.py:65` | `montar_previa` | Filtra perguntas pela competência. | Sim |
| `app/folha/previa.py:67` | `montar_previa` | Agrupa material da prévia por dia cru. | Sim |
| `app/ponto/management/commands/mapa_divergencia.py:272` | `Command.handle` | Dois filtros `gte/lte` e projeção `only('data_evento')`: três nós. | Sim |
| `app/ponto/management/commands/mapa_divergencia.py:273` | `Command.handle` | Constrói conjunto de dias para contador de furos cobertos por pergunta. | Sim |
| `app/ponto/management/commands/vigia_plantio_overnight.py:75` | `batidas_fora_do_turno_da_pergunta` | Lookup `pergunta_origem__data_evento__isnull=False`. | Sim se conversa alcançar o universo; ocorrência [nao verificado] |
| `app/ponto/management/commands/vigia_plantio_overnight.py:86` | mesma | Usa data crua para ampliar janela de busca dos turnos. | Mesma condição |
| `app/ponto/management/commands/vigia_plantio_overnight.py:90` | mesma | Compara com dia das batidas e apresenta diferença. | Mesma condição |
| `app/ponto/services/dia_do_colab.py:65` | `dia_do_colab` | Filtra perguntas para `perguntas_deste_dia`. | Sim |
| `app/ponto/services/veredito_celula.py:180` | `fechar_o_fio_do_dia` | Filtra perguntas do dia para fechamento; contém `conversa=False`. | Não se aplica: conversa excluída |
| `mensageria/nucleo/ferramentas.py:127` | `fila_de_validacao` | Lê chave serializada com `i.get('data_evento')`, renomeando para `data_do_caso`. | Sim, só repassa |

Proveniência e limites:

- A API v2 percorre `consulta['pendentes'] + consulta['historico']`, sem discriminar conversa nessa montagem: `app/api/views.py:1040`. `consulta_do_app` separa apenas por resposta/disputa: `app/chamados/services/pergunta_no_app.py:69`.
- A fila da mensageria vem de `perguntas_respondidas_a_validar`, sem exclusão de conversa: `app/api/views_mensageria.py:540`, `app/chamados/juizes.py:1664`. O cliente lê `/api/mensageria/validar/` e retorna JSON: `mensageria/nucleo/core_client.py:49`, `mensageria/nucleo/core_client.py:55`. Consumo: `mensageria/nucleo/ferramentas.py:107`, `mensageria/nucleo/ferramentas.py:129`.
- No vigia, o filtro exige `tipo_resposta='hora'`, mas não `conversa=False`: `app/ponto/management/commands/vigia_plantio_overnight.py:74`. A relação `Batida.pergunta_origem` aponta para `PerguntaDisputa`, portanto não é colisão de modelo: `app/ponto/models.py:40`, `app/ponto/models.py:41`.
- `app/escala/management/commands/hub_smoke.py:35` e `app/escala/management/commands/hub_smoke.py:51` são nomes de parâmetros usados por introspecção, sem acesso ao campo de modelo.
- O censo anterior documenta explicitamente o problema da prévia e de `dia_do_colab`: `app/chamados/services/fio.py:1398`, `app/chamados/services/fio.py:1399`.

## 11/11 14:57 · sessao 7361ac4aef50 · - · - · origem=Ronald (tmux)

(b) Templates: **5 ocorrências sintáticas em 5 linhas/3 arquivos; 4 leitores com origem demonstrada em PerguntaDisputa e 1 referência sem vínculo de contexto demonstrado**. A relação completa é:

| Arquivo:linha | Contexto e origem | Uso | Diverge para conversa? |
|---|---|---|---|
| `app/templates/chamados/partials/_card_atendimento.html:87` | `p` do laço `chamado.perguntas_disputa` em `:77`; `meu_atendimento` chama `montar_chamados_fio` (`app/chamados/views.py:569`), que consulta PerguntaDisputa (`app/chamados/services/fio.py:225`), agrupa (`:243`) e anexa a `ch.perguntas_disputa` (`:253`); os includes vêm de `app/templates/chamados/meu_atendimento.html:127` e `:144` | Testa existência para exibir cabeçalho do dia | **Sim**, havendo conversa com data: o cru habilita o cabeçalho; juiz retorna None (`app/chamados/juizes.py:338`) |
| `app/templates/chamados/partials/_card_atendimento.html:90` | Mesmo contexto acima | Exibe “Estamos falando do dia” (`:89`) formatando o cru | **Sim**, civil exibido no lugar do dia do fato; juiz None (`app/chamados/juizes.py:338`) |
| `app/templates/chamados/partials/_card_atendimento.html:164` | Mesmo contexto; ramo `p.tipo_resposta == 'hora' and p.opcoes` (`:158`) | Exibe dia na confirmação JS inline | **Sim se conversa alcançar esse ramo**; nenhuma exclusão `conversa` nesse ramo (`:158`); ocorrência real em produção [nao verificado] |
| `app/templates/chamados/partials/_lote_preview.html:24` | `x` de `linhas`; `lote_preview` passa classe A (`app/chamados/views_cobrar.py:363`); `fila_validar_classificada` insere `{'pergunta': p}` (`app/chamados/services/validacao.py:923`); `p` vem de `perguntas_respondidas_a_validar` (`:921`), cujo queryset é PerguntaDisputa (`app/chamados/juizes.py:1664`) | Exibe dia na coluna “Dia” (`_lote_preview.html:23`) | **Sim se conversa alcançar classe A**; classe usa resposta/marco e não exclui `conversa` explicitamente (`app/chamados/services/validacao.py:845`, `:877`); ocorrência real [nao verificado] |
| `app/templates/chamados/meu_atendimento.html:19` | Bloco `for reg in regularizacoes_pendentes` (`:10`); não há laço de `p` ali; view fornece `regularizacoes_pendentes`, mas não `p`/`opt` (`app/chamados/views.py:633`, `:647`, `:651`) | Tentativa de exibir dia em confirmação | **Não se aplica** como leitor demonstrado de PerguntaDisputa; associação de `p` [nao verificado] |

A menção `data_evento` em `app/templates/chamados/partials/modal_fio.html:161` é comentário, sem acesso de variável nessa linha. O local citado no anexo não corresponde a leitor cru na cópia: `app/templates/colaboradores/partials/painel_colaborador.html:198` contém include de `msgs_lista.html`.

(c) Serialização/API: **2 endpoints, 2 chaves exatas `data_evento` + 1 chave derivada `data_evento_label` = 3 posições de emissão**, em 3 linhas:

| Arquivo:linha | Função / caminho | Uso | Diverge para conversa? |
|---|---|---|---|
| `app/api/views.py:1165` | `api_chamados_v2` (`:973`), rota `app/api/urls.py:73` | Repassa cru em ISO ou None, dentro de `chamados[].perguntas_disputa[]` (`views.py:1155`) | **Sim no valor transmitido**: conversa com cru preenchido transmite ISO; juiz None (`juizes.py:338`). Exibição pelo app nativo [nao verificado] |
| `app/api/views.py:1166` | Mesmo endpoint | Repassa cru formatado `dd/mm/aaaa` em `data_evento_label` | **Sim no valor transmitido**; consumo do app nativo [nao verificado] |
| `app/api/views_mensageria.py:568` | `_item_validar` (`:554`) chamado por `api_mensageria_validar` (`:546`), retornado como JSON (`:549`); rota `app/api/urls.py:12` | Repassa cru ISO ou string vazia | **Sim no valor transmitido**; juiz None (`juizes.py:338`) |
| `mensageria/nucleo/ferramentas.py:127` | `fila_de_validacao` (`:98`); chama cliente (`:107`); cliente lê `/api/mensageria/validar/` (`mensageria/nucleo/core_client.py:50`) e retorna JSON (`:55`) | Consome `i.get('data_evento')`, renomeia para `data_do_caso` em casos enviados ao copiloto (`ferramentas.py:116`, `:137`) | **Sim no valor repassado**, se item de conversa; interpretação do copiloto [nao verificado] |

Não encontrei acesso a `data_evento` em arquivos JS/TS/TSX/Dart da cópia por varredura textual [nao verificado quanto a código externo/cliente nativo]. O JS inline de templates é o de `_card_atendimento.html:164` e `meu_atendimento.html:19`, já contado em (b).

Para a allowlist (a), incluir também o leitor de chave serializada **`mensageria/nucleo/ferramentas.py:127`**. As chaves em `mensageria/nucleo/tests/test_ferramenta_validar.py:18`, `:24` e `mensageria/nucleo/tests/test_proposta_lote.py:24` são construção de fixtures de resposta, não leitura do modelo.

## 11/11 14:57 · sessao 029825b73c6b · - · - · origem=Ronald (tmux)

Achado bloqueante: a obra consegue se autodeclarar B adicionando **também o próprio CORTES.md à lista**. A leitura vem de head (`bin/selo_proporcao.py:46`), o parser admite qualquer caminho craseado (`bin/selo_proporcao.py:49`), e `motivo_b` aceita a lista antes de qualquer classificação (`bin/selo_proporcao.py:59`). O teste `lista_se_autoriza` acrescenta somente `views.py`, deixando CORTES fora; portanto não morde este escape (`bin/tests/test_selo_proporcao.sh:69`).

Diff mínimo textual, **rc previsto 0, sem executar** (`bin/selo_proporcao.py:107`):

```diff
--- a/app/docs/CORTES.md
+++ b/app/docs/CORTES.md
@@ linha 444, antes de FIM-LISTA-FECHADA-PROPORCAO-B:
+; `app/chamados/writer.py`; `app/docs/CORTES.md`
--- /dev/null
+++ b/app/chamados/writer.py
@@
+from chamados.models import ChamadoColaborador; ChamadoColaborador.objects.update(status_local='fechado')
```

Aqui a obra muda a autorização em seu próprio diff; isso contraria “modulo novo na lista so por corte do Ronald” (`app/docs/CORTES.md:444`). A afirmação “A obra nao se autoriza” é falsa para esse caso (`bin/selo_proporcao.py:19`). O campo persistido existe (`app/chamados/models.py:52`). Execução efetiva do writer: [nao verificado].

Escapes **aceitos pelo critério de caminho**, com rc previsto 0 e sem execução:

| Diff mínimo | Escrita e caminho | Aval |
|---|---|---|
| Em `particionar_verbos`, acrescentar `vivo_qs.update(status_local='fechado')` antes da linha atual 34 | Queryset recebido pela função (`app/chamados/services/verbos.py:30`); consumidor real no painel (`app/chamados/services/painel.py:230`); módulo listado (`app/docs/CORTES.md:444`), aceito por `bin/selo_proporcao.py:59`. | ACEITA pelo caminho; não é defeito de AST. |
| Em `area_label`, acrescentar `chamado.status_local='fechado'; chamado.save(update_fields=['status_local'])` antes do return | Filtro recebe chamado (`app/chamados/templatetags/chamados_tags.py:34`), consumido em template (`app/templates/chamados/detalhe_local.html:56`); módulo listado (`app/docs/CORTES.md:444`). | ACEITA pelo caminho. |
| Trocar `tipo, timestamp: tsCorrigido` por `tipo: 'saida', timestamp: tsCorrigido` | Grava batida em IndexedDB (`app/static/pwa.js:112`); conteúdo enviado por POST depois (`app/static/pwa.js:144`, `app/static/pwa.js:153`); static aceito (`bin/selo_proporcao.py:63`). | ACEITA pelo caminho. |
| Acrescentar writer na função `tblib_instalado`, por exemplo import do modelo e `.objects.update(...)` | Helper vive sob tests (`app/core/tests/test_contract_tblib_no_paralelo.py:24`) e é chamado por management command de produção (`app/chamados/management/commands/tblib_ausente_na_imagem.py:17`); tests aceito (`bin/selo_proporcao.py:65`). | ACEITA pelo caminho. |

O texto B3 diz “CLASSE B so se TODO arquivo do diff estiver em templates/, static/, arquivo de teste, ou numa LISTA FECHADA” e “O selo checa CAMINHO, nao AST”; não ordena inspecionar esses conteúdos (`app/docs/CORTES.md:444`; literal completo no anexo fornecido [nao verificado no filesystem]).

QUEM CHAMA / QUEM LÊ:

- O harness define `SELO` em `bin/tests/test_selo_proporcao.sh:9`, chama e captura rc em `bin/tests/test_selo_proporcao.sh:41`, compara em `bin/tests/test_selo_proporcao.sh:42`; outras chamadas estão em `:95`, `:99`, `:105`, `:124`, `:134`, `:139`, `:141`.
- O harness tem chamador real: `bin/regua.sh:138` percorre `tests/test_*.sh`, executa em `bin/regua.sh:140` e recusa em `bin/regua.sh:143`.
- Essa cadeia verifica repositórios sintéticos (`bin/tests/test_selo_proporcao.sh:11`, `bin/tests/test_selo_proporcao.sh:13`). Chamador automatizado do selo sobre base/head **da obra real** não foi localizado na cópia; uso humano/prod [nao verificado]. O pre-push explica que o loop de host não roda ali (`bin/pre-push.sh:15`).
- A documentação exige docs no mesmo commit (`CLAUDE.md:333`); isso torna a obra A se o `.md` estiver fora dos conjuntos, conforme `bin/selo_proporcao.py:70` e `bin/selo_proporcao.py:107`. A primeira frase literal do B3 (“TODO arquivo”) sustenta isso; “qualquer outro .py” não constitui permissão para outros sufixos (`app/docs/CORTES.md:444`).
- O cabeçalho chama de “literal” a paráfrase “qualquer outro arquivo”, enquanto B3 escreve “qualquer outro .py” (`bin/selo_proporcao.py:10`, `app/docs/CORTES.md:444`); a consequência para `.md` continua derivável do “TODO”.
- O cabeçalho atribui ao smoke/fila os writers em caminhos permitidos (`bin/selo_proporcao.py:21`); essa atribuição específica não consta do B3 fornecido [nao verificado no filesystem]. B3 autoriza o critério por caminho; não declara expressamente que smoke detectará esses writers.
- Não há contrato PROPORCAO no RELATO da cópia; a única ocorrência de “proporcao” encontrada ali é assunto antigo (`app/docs/RELATO.md:3378`). Existência do contrato v5 no RELATO/prod: [nao verificado].

## 11/11 14:56 · sessao 2b46e4366f87 · - · - · origem=Ronald (tmux)

Achados por leitura; execução dos rc e dos mutantes [nao verificado].

- **Autoautorização passa:** acrescentar à lista head tanto `app/docs/CORTES.md` quanto `app/x/views.py`, e mudar `views.py` para gravar. O parser aceita qualquer item entre crases; os dois arquivos ficam na lista e `fora=[]`, portanto rc previsto **0**. O caso `lista_se_autoriza` acrescenta somente `views.py`, deixando CORTES fora; não prova a defesa contra a alteração conjunta. Isso contradiz a promessa “A obra nao se autoriza”. (`bin/selo_proporcao.py:19`, `:52`, `:54`, `:59`, `:102`, `:110`; `bin/tests/test_selo_proporcao.sh:69`.)

- **Mutante sobrevivente de rc:** em `bin/selo_proporcao.py:119`, trocar `rc = 2` por `rc = 0`. Os erros dos testes entram nos retornos explícitos de argumentos ou em `NaoJulgado`; nenhum caso provoca exceção inesperada na fronteira Python. Os stubs substituem o interpretador, portanto não passam por esse bloco. Sobrevivência prevista por leitura, execução [nao verificado]. (`bin/selo_proporcao.py:74`, `:95`, `:117`; `bin/tests/test_selo_proporcao.sh:83`, `:122`, `:136`.)

- **Mutante sobrevivente de marcador:** em `bin/selo_proporcao.py:47`, remover somente `texto.count(INICIO) != 1 or`. O caso `marcador_duplo` duplica ambos os marcadores; a checagem restante continua rejeitando. A ausência de início ainda termina em `ValueError`, normalizado para rc 2. Não há caso duplicando somente o início. O mutante simétrico, removendo somente a contagem do fim, também tem sobrevivência prevista. (`bin/selo_proporcao.py:47`, `:49`, `:117`; `bin/tests/test_selo_proporcao.sh:83`, `:85`, `:87`.)

- **Dois gatilhos:** `marcador_duplo` mistura as duas violações de cardinalidade; `misto_b` usa `tests/test_b.py`, elegível simultaneamente por diretório e basename. Neste segundo caso existem controles separados: `tests_dir` usa `fixt.py`, e `test_basename` fica fora de `tests/`. (`bin/tests/test_selo_proporcao.sh:50`, `:51`, `:54`, `:87`.)

- **Nome versus mecanismo:** `migration` prova que um `.py` fora dos conjuntos recebe rc 1; não existe guarda específica de migration na v5. `apagou_fora` cobre exclusão pelo mesmo mecanismo genérico. (`bin/tests/test_selo_proporcao.sh:63`, `:65`; `bin/selo_proporcao.py:57`, `:91`, `:107`.)

- **Repo errado:** subdiretório e diretório sem Git recebem rc previsto 2. Uma raiz Git válida de outro repositório não é identificada como “errada”: se resolver as revisões e satisfizer as demais condições, o selo julga aquele repo e pode devolver 0/1. Não há comparação com identidade esperada do projeto. (`bin/selo_proporcao.py:77`, `:79`, `:84`, `:90`; `bin/tests/test_selo_proporcao.sh:119`, `:121`.)

- **rc:** o único retorno lógico 1 exige `classe == 'B' and fora`; argumentos, igualdade, não ancestralidade, lista inválida e diff vazio dão 2. `GIT_*` é filtrado nas chamadas Git. O wrapper traduz 10→0, 11→1 e todo outro código→2; o teste do chamador com `bash -e` exige rc 1. (`bin/selo_proporcao.py:38`, `:74`, `:86`, `:88`, `:93`, `:97`, `:107`, `:120`; `bin/selo_proporcao.sh:8`; `bin/tests/test_selo_proporcao.sh:134`.)

## 11/11 14:53 · sessao 1c94fdcaac33 · FIO-E0-V4-E4B · construtor · origem=Code

Travas separadas em duas fases: [fecho_runtime.py:119](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/fecho_runtime.py:119).

RED c7 e mutante mordem; Ruff verde e 118 testes em memória passaram. Comandos, resultados e fila no [recibo c8](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c8.txt:23). [nao verificado]

ORM, suíte integral e certificação Code/opus: [nao verificado]. [nao verificado]

## 11/11 14:47 · sessao d4ad4297d1f3 · GABARITO-FABLE-SOMBRA-AJUSTE · verificador · origem=Code

**BLOQUEANTE** — comportamento errado na tela exige volta, inclusive em classe B: “achado de comportamento errado na tela continua sendo volta” (anexo fornecido `aval_proporcao_por_risco_1111.txt:1`).

- **Entrada concreta:** conversa aberta com data local `11/11/2026` grava `conversa=True, data_evento=2026-11-11`; `abrir` distingue esse caso da pergunta datada de turno ([models.py:2783](app/chamados/models.py:2783), [models.py:2801](app/chamados/models.py:2801)). A emissão sem dia encaminha para `abrir_conversa` ([disputa_emissao.py:431](app/chamados/services/disputa_emissao.py:431)).
- **Resultado, estando na lista pendente:** painel mostra **“Dia: 11/11/2026”** pelo campo cru; juiz responde **`None` — sem dia de turno** ([painel_colaborador.html:198](app/templates/colaboradores/partials/painel_colaborador.html:198), [juizes.py:338](app/chamados/juizes.py:338)).
- **Enquadramento:** a função única de **rótulo** exigida pelo aval não autoriza regra independente de **dia** (anexo fornecido `aval_ui_painel_raspado.txt:8`). “Nenhum leitor tem regra propria de dia” e “Fatia que fura qualquer linha abaixo NAO FECHA” ([CLAUDE.md:21](CLAUDE.md:21), [CLAUDE.md:15](CLAUDE.md:15)). É divergência concreta da apresentação, não achado sobre a forma de testar.

Ocorrência efetiva em produção: **[nao verificado]**.

## 11/11 14:45 · sessao 5928edd1fdd0 · FIO-E0-V4-E4B · verificador · origem=Code

**REPROVADO.** Há um **BLOQUEANTE novo de ordem de trava**: a serialização por competência intercala travas de âncora com a trava compartilhada do contador DP. A suíte inteira também permanece pendente para encerramento da obra. (`app/chamados/services/fecho_runtime.py:115`; `app/pautas/services.py:188`; `app/pautas/services.py:354`; `CLAUDE.md:375`)

**B1 — BLOQUEANTE: deadlock entre âncoras e contador DP.** O laço faz `travar(A) → escrever(A) → travar(B)`. Dentro de `escrever(A)`, `lavrar` atualiza o contador DP; essa trava permanece na transação externa. Ordenar as âncoras não ordena a trava intermediária do contador. (`app/chamados/services/fecho_runtime.py:116`; `app/chamados/services/fecho_runtime.py:122`; `app/pautas/services.py:188`; `app/pautas/services.py:354`; `app/chamados/services/fecho_runtime.py:142`)

Intercalação permitida pelo código, com contador DP existente e Pautas ainda ausentes:

| Passo | Transação T1: competências A,B | Transação T2: competência B |
|---|---|---|
| 1 | Trava A, escreve e segura contador DP. | |
| 2 | | Trava B; tenta escrever e espera contador DP. |
| 3 | Pede B e espera T2. | Continua esperando T1. |

O lote com duas competências é expressamente suportado; portanto o contraexemplo não depende de inventar outro escritor. Reprodução em duas conexões: **[nao verificado]**. (`app/chamados/services/fecho_runtime.py:105`; `app/chamados/services/fecho_runtime.py:116`; `app/chamados/tests/test_e4b_c6_memoria.py:119`; `app/chamados/tests/test_e4b_c6_memoria.py:128`)

1. Quanto aos achados da c6:

| Achado anterior | Classificação nesta volta |
|---|---|
| Dia civil da conversa virando competência no novo agrupador | **Curado pela CLASSE nesse leitor.** Toda pergunta passa por `_dia_de_turno_da_pergunta`; `None` sai antes de `janela_atual`, sem data substituta. (`app/chamados/services/fecho_runtime.py:100`; `app/chamados/services/fecho_runtime.py:107`; `app/chamados/services/fecho_runtime.py:108`; `app/chamados/juizes.py:336`) |
| Duas Pautas concorrentes para marcador+âncora | **Curado pela CLASSE dos escritores E4B participantes**, pela trava determinística anterior ao `exists`, na mesma transação da criação. A cura introduz B1. **Na porta `escrever`, não achei idempotência:** ela continua fazendo `create`; o modelo não impõe unicidade dessa entrega. (`app/chamados/services/fecho_runtime.py:86`; `app/chamados/services/fecho_runtime.py:115`; `app/chamados/services/fecho_runtime.py:117`; `app/pautas/services.py:180`; `app/pautas/models.py:123`) |
| Motivo limbo truncado pela porta | **Curado pela CLASSE dos formatos produzidos pelo S143.** Reserva os motivos completos e abrevia ambas as listas com identificadores inteiros e contagens. O teste ORM mede antes da porta. (`app/chamados/services/disputa_emissao.py:1840`; `app/chamados/services/fecho_runtime.py:64`; `app/chamados/services/fecho_runtime.py:76`; `app/chamados/tests/test_e4b_c6_orm.py:226`) |
| Coleta em memória sobrevivendo ao rollback do S143 | **Curado pela CLASSE das falhas propagadas pelo resolver ou pela saída do savepoint.** O lote só é incorporado depois dessa saída sem erro; o token sempre é restaurado. (`app/chamados/signals.py:115`; `app/chamados/services/fecho_runtime.py:39`; `app/chamados/services/fecho_runtime.py:40`) |
| Reentrega não acrescenta novos IDs à Pauta existente | **Não curado — FILA.** Encontrar Pauta viva continua executando `continue`; a nota S143 conserva o rastro individual. (`app/chamados/services/fecho_runtime.py:118`; `app/chamados/services/fecho_runtime.py:120`; `app/chamados/services/disputa_emissao.py:1855`) |
| Fechamentos futuros fora das duas bordas sem entrega E4B | **Não curado — FILA de alcance.** `fechar()` continua usando transição direta; sem contexto, o coletor não entrega. O teste de passivo continua partindo de chamado já encerrado. (`app/chamados/models.py:454`; `app/chamados/services/fecho_runtime.py:19`; `app/chamados/tests/test_e4b_c6_orm.py:162`) |
| Relógio fora da borda | **Não curado — FILA preexistente.** S143 fornece `hoje=None`; C6 pode executar `date.today()`. (`app/chamados/services/disputa_emissao.py:1945`; `app/chamados/juizes.py:577`) |
| Falta de prova concorrente e de falha real de escrita da Pauta | **Não curado — FILA de prova.** O ensaio de trava é serial; o erro de memória ocorre durante a transição. Não achei ensaio de falha real de `escrever` nesses testes. (`app/chamados/tests/test_e4b_c6_orm.py:208`; `app/chamados/tests/test_e4b_c6_orm.py:209`; `app/chamados/tests/test_e4b_c6_memoria.py:113`) |
| Suíte inteira pendente | **Não curado — BLOQUEANTE de encerramento.** Os resultados fornecidos não são a suíte inteira exigida antes da leitura final. (`CLAUDE.md:375`; execução integral **[nao verificado]**) | [nao verificado]

A resposta às duas perguntas especiais é distinta: **não achei `data_evento` cru em `fecho_runtime.py`; ainda existe no caminho completo do S143**. A recusa de exportada lê `q.data_evento`; S128/HX passam o atributo cru para determinar a via, e o gate do chamado também o lê. Portanto a c7 cura o leitor acrescentado na c6, sem concluir a migração integral — **FILA preexistente**. (`app/chamados/services/fecho_runtime.py:107`; `app/chamados/services/disputa_emissao.py:1836`; `app/chamados/services/disputa_emissao.py:1984`; `app/chamados/services/disputa_emissao.py:2056`; `app/chamados/models.py:364`)

A retirada do retorno antecipado permanece: S128, HX e C7 seguem seus próprios juízes. O chamado pode fechar enquanto a disputa mista permanece aberta; isso é exigido pelo teste, não uma contradição entre estados. (`app/chamados/services/disputa_emissao.py:1950`; `app/chamados/services/disputa_emissao.py:1964`; `app/chamados/services/disputa_emissao.py:2007`; `app/chamados/services/disputa_emissao.py:2089`; `app/chamados/tests/test_e4b_c6_orm.py:117`; `app/chamados/tests/test_e4b_c6_orm.py:118`)

2. O mapa de escritores e leitores do delta é:

| Campo/valor | Quem escreve ou produz | Quem lê e com qual significado |
|---|---|---|
| `_RECUSAS` e token do ato | Declara `None`, instala o ato e restaura no `finally`. (`app/chamados/services/fecho_runtime.py:13`; `app/chamados/services/fecho_runtime.py:144`; `app/chamados/services/fecho_runtime.py:148`) | `registrar_recusa` e o contexto de confirmação leem o ato corrente. (`app/chamados/services/fecho_runtime.py:18`; `app/chamados/services/fecho_runtime.py:30`) |
| `ato/lote['chamado']` | Ato recebe `chamado.pk`; lote copia esse valor. (`app/chamados/services/fecho_runtime.py:143`; `app/chamados/services/fecho_runtime.py:34`) | Coletor compara com **PK de chamado**, não de pergunta ou disputa. (`app/chamados/services/fecho_runtime.py:19`) |
| Recusas coletadas | S143 produz motivo e chama o coletor; este indexa por **PK da pergunta**, guardando objeto+motivo. (`app/chamados/services/disputa_emissao.py:1840`; `app/chamados/services/disputa_emissao.py:1860`; `app/chamados/services/disputa_emissao.py:1950`; `app/chamados/services/fecho_runtime.py:20`) | Agrupador lê objeto, empresa, dia e motivo; montador interpreta os formatos do S143. (`app/chamados/services/fecho_runtime.py:106`; `app/chamados/services/fecho_runtime.py:110`; `app/chamados/services/fecho_runtime.py:66`) |
| “Savepoint confirmado” | **Não há campo persistido.** É o merge do lote após saída normal dos contextos. (`app/chamados/signals.py:115`; `app/chamados/services/fecho_runtime.py:40`) | Adaptador entrega o dicionário confirmado ao agrupador após a transição. Não significa commit da transação externa. (`app/chamados/services/fecho_runtime.py:142`; `app/chamados/services/fecho_runtime.py:149`) |
| `data_evento` / `conversa` | `abrir` fornece turno e `False`; `abrir_conversa` fornece dia civil e `True`; atribuição central em `gravar_dia_do_fato`. (`app/chamados/models.py:2789`; `app/chamados/models.py:2801`; `app/chamados/services/fio.py:801`) | Juiz distingue os significados; agrupador usa esse juiz. Os leitores crus remanescentes estão apontados acima. (`app/chamados/juizes.py:336`; `app/chamados/services/fecho_runtime.py:107`) |
| Empresa e corte | Relação do colaborador e cadastro de empresa; manutenção pela porta cadastral. (`app/colaboradores/models.py:321`; `app/colaboradores/models.py:46`; `app/colaboradores/services/estrutura.py:19`) | Runtime usa a empresa da pergunta; `janela_atual` consulta o corte efetivo. (`app/chamados/services/fecho_runtime.py:110`; `app/ponto/janelas.py:36`; `app/ponto/janelas.py:60`) |
| Competência, marcador, âncora e chave | Fim da janela produz `<empresa>:AAAA-MM`; marcador+tipo+âncora produzem a chave advisory. (`app/chamados/services/fecho_runtime.py:12`; `app/chamados/services/fecho_runtime.py:111`; `app/chamados/services/fecho_runtime.py:112`; `app/chamados/services/fecho_runtime.py:86`) | Dedup lê marcador+âncora; porta serializa o ID; chip e payload conservam o par. É competência por corte. Não achei leitor desse percurso reinterpretando-a como mês civil. (`app/chamados/services/fecho_runtime.py:118`; `app/pautas/services.py:184`; `app/pautas/services.py:334`; `app/pautas/views.py:40`) |
| Texto e listas | Montador reserva nomes e distribui os 500 caracteres; porta persiste. (`app/chamados/services/fecho_runtime.py:76`; `app/chamados/services/fecho_runtime.py:79`; `app/pautas/services.py:185`) | Payload DP entrega texto; motivo integral permanece na nota original. (`app/pautas/views.py:40`; `app/chamados/services/disputa_emissao.py:1855`) |
| Destinatário, remetente e autor | Runtime fornece DP/TI/autor central; porta valida e grava. (`app/chamados/services/fecho_runtime.py:122`; `app/chamados/services/fecho_runtime.py:124`; `app/pautas/services.py:157`; `app/pautas/services.py:180`) | Listagem filtra departamento; autoria é trilha. (`app/pautas/services.py:308`; `app/pautas/models.py:105`) |
| `feito_em` / `cancelado_em` | Portas centrais de estado da Pauta. (`app/pautas/services.py:247`; `app/pautas/services.py:294`) | Dedup usa `.vivas()`: feita ou cancelada permite nova entrega. (`app/chamados/services/fecho_runtime.py:119`; `app/pautas/models.py:57`; `app/pautas/models.py:64`) |
| `status_local` / destino / usuário | Bordas transportam; `_gravar_estado` atribui e salva. (`app/chamados/services/acoes_chamado.py:316`; `app/chamados/services/fio.py:722`; `app/chamados/models.py:228`; `app/chamados/models.py:296`) | Adaptador lê VIVOS→ENCERRADOS; signal lê estado salvo e se houve escrita do status. (`app/chamados/services/fecho_runtime.py:139`; `app/chamados/signals.py:96`; `app/chamados/signals.py:104`) |

3. Em `signals.py`, quem dispara continua sendo o `post_save` de `ChamadoColaborador`, quando não é carga `raw`, o estado está encerrado e o save escreveu o status. Um `.update()` direto não passa por esse receiver. (`app/chamados/signals.py:90`; `app/chamados/signals.py:96`; `app/chamados/signals.py:104`; `app/chamados/signals.py:95`)

No runtime, a sequência é transação do ato → transação de estado → save/receiver → savepoint S143. A novidade envolve o savepoint com o contexto coletor; Python sai primeiro do `atomic` e depois confirma o lote. Exceção no resolver ou na saída do savepoint descarta esse lote e é capturada fora dos contextos. **Não achei entrega de recusa revertida nesse percurso de falha.** (`app/chamados/services/fecho_runtime.py:142`; `app/chamados/models.py:280`; `app/chamados/models.py:296`; `app/chamados/signals.py:115`; `app/chamados/signals.py:117`; `app/chamados/services/fecho_runtime.py:39`; `app/chamados/services/fecho_runtime.py:40`)

A Pauta é escrita depois do receiver, ainda na transação externa: falha de escrita propaga e desfaz esse ato no banco. Fora das duas bordas, o contexto continua ausente; becos, reconciliador e fantasmas não ganham entrega E4B. (`app/chamados/services/fecho_runtime.py:149`; `app/chamados/services/fecho_runtime.py:19`; `app/chamados/services/becos.py:54`; `app/chamados/reconciliador.py:536`; `app/chamados/reconciliador.py:1648`; `app/ponto/management/commands/reconciliar_fantasmas.py:281`)

4. Das classes já reprovadas, **ordem de trava reaparece como B1**. Relógio fora da borda e campo temporal com dois significados permanecem nos leitores preexistentes, como FILA. **Não achei segundo juiz, fórmula paralela de competência ou persistência de Pauta fora da porta no delta produtivo.** (`app/chamados/services/fecho_runtime.py:117`; `app/pautas/services.py:354`; `app/chamados/juizes.py:577`; `app/chamados/services/disputa_emissao.py:1836`; `app/chamados/services/fecho_runtime.py:107`; `app/chamados/services/fecho_runtime.py:111`; `app/chamados/services/fecho_runtime.py:122`)

5. Nas provas, **não achei falso verde demonstrável nos quatro RED c7**, mas há limites importantes: [nao verificado]

- O fake de memória aceita escrita imediatamente numa lista, e seu `atomic` só controla profundidade. Ele não certifica rollback nem o contador DP — justamente a dependência de B1. (`app/chamados/tests/test_e4b_c6_memoria.py:36`; `app/chamados/tests/test_e4b_c6_memoria.py:62`)
- O teste ORM da trava usa uma conexão, chamadas seriais e segundo chamado/colaborador em memória. Prova `lock→exists`, sem testar a intercalação concorrente nem múltiplas âncoras disputando o contador. (`app/chamados/tests/test_e4b_c6_orm.py:196`; `app/chamados/tests/test_e4b_c6_orm.py:203`; `app/chamados/tests/test_e4b_c6_orm.py:208`)
- O texto não passa só por truncamento: mede antes da porta e compara com o persistido. O rollback exige escrita real revertida, chamado fechado e zero Pauta. (`app/chamados/tests/test_e4b_c6_orm.py:226`; `app/chamados/tests/test_e4b_c6_orm.py:228`; `app/chamados/tests/test_e4b_c6_orm.py:237`; `app/chamados/tests/test_e4b_c6_orm.py:249`)
- Não achei fixture invisível de viva/morta: as precondições consultam C6 e entrega. Já o teste de conversa injeta a recusa e chama diretamente o agrupador; sua prova é desse leitor. (`app/chamados/tests/test_e4b_c6_orm.py:78`; `app/chamados/tests/test_e4b_c6_orm.py:82`; `app/chamados/tests/test_e4b_c6_orm.py:182`)
- O protocolo exige GREEN anterior e assertion-alvo, rejeitando erro e precondição quebrada. Há mordida para as quatro curas, mas **não achei mordida para B1**. (`app/chamados/tests/e4b_c6_verificar.py:187`; `app/chamados/tests/e4b_c6_verificar.py:199`; `app/chamados/tests/e4b_c6_verificar.py:34`)
- O lint fornecido continua vermelho; o import não usado existe. O aviso sobre `falha` não demonstra falso verde: o contexto é consumido na própria iteração. (`app/chamados/tests/test_e4b_c6_memoria.py:7`; `app/chamados/tests/test_e4b_c6_memoria.py:264`; `app/chamados/tests/test_e4b_c6_memoria.py:272`; `c7_orm.log:6`, anexo)

Os GREENs e roteiros fornecidos não eliminam B1 nem substituem a suíte inteira. Execução independente, ocorrência do deadlock em produção e correspondência Git integral ao hash/base: **[nao verificado]**; metadados Git nesta cópia: **não achei**. [nao verificado]

## 11/11 14:44 · sessao af472d58e1b7 · - · - · origem=Ronald (tmux)

Censo de `app/chamados`: **181 coordenadas leitoras de produção** (123 runtime +58 management), **143 de testes** e **2 de migrations**; total **326**. Unidade: `arquivo:linha`, deduplicando várias ocorrências na mesma linha; excluída somente `_dia_de_turno_da_pergunta` em `app/chamados/juizes.py:338`.

Atribuição direta de produção: **1**, `gravar_dia_do_fato`, em `app/chamados/services/fio.py:800`. O nascimento instancia sem `data_evento` e chama esse escritor em `app/chamados/models.py:3014` e `app/chamados/models.py:3016`.

Sete leitores dinâmicos adicionais, todos em `app/chamados/services/fio.py`: `1946` (exclude por chave), `1948` (values), `1949` (order_by), `1950` (dicionário serializado `g[k]`), `1958` (values_list/order_by), `1970` (filter via zip) e `2000` (filter de marco sem dia). As definições dos campos ficam em `app/chamados/services/fio.py:1373` e `app/chamados/services/fio.py:1378`; o universo exclui conversa em `app/chamados/services/fio.py:1432`.

Guardas de conversa comprovadas:

- `juizes.py:80,81` — excluída em `app/chamados/juizes.py:78`.
- `juizes.py:214` — excluída em `app/chamados/juizes.py:213`.
- `juizes.py:2327,2331` — filtro `conversa=False` em `app/chamados/juizes.py:2328`.
- `tarefa_do_dia.py:376,380,486` — filtro `conversa=False` em `app/chamados/services/tarefa_do_dia.py:375`.
- `pergunta_no_app.py:115` — discriminador na própria expressão, `app/chamados/services/pergunta_no_app.py:115`.
- `fio.py:1447` — retorno antecipado em `app/chamados/services/fio.py:1445`.
- `fio.py:1854` — filtro em `app/chamados/services/fio.py:1852`.
- `fio.py:1955` — filtro em `app/chamados/services/fio.py:1956`.

Atenção: `dia_do_turno_da_linha` lê o campo em `app/chamados/services/fio.py:1630` e **deliberadamente devolve o dia civil da conversa** em `app/chamados/services/fio.py:1631`. Esse valor diverge do `None` do juiz em `app/chamados/juizes.py:338`, embora participe do contrato de escrita. `violacao_de_saida` também alcança conversa: leituras em `app/chamados/services/fio.py:1133` e `app/chamados/services/fio.py:1136`, universo explicitamente incluindo conversa em `app/chamados/services/fio.py:1156`.

As listas completas de coordenadas, funções, management, testes e ressalvas foram enviadas nas mensagens de consolidação; nenhuma escrita foi realizada. [nao verificado] [nao verificado]

## 11/11 14:43 · sessao 0fe14d66a330 · - · - · origem=Ronald (tmux)

**BLOQUEANTE — nova inversão entre travas de âncora e contador DP.** O laço adquire uma âncora, escreve a Pauta e só depois adquire a próxima; a escrita chama `lavrar`, cujo `UPDATE` trava o contador do departamento. Tudo permanece no `atomic` externo. (`app/chamados/services/fecho_runtime.py:115`, `app/chamados/services/fecho_runtime.py:117`, `app/chamados/services/fecho_runtime.py:122`, `app/pautas/services.py:188`, `app/pautas/services.py:352`, `app/pautas/services.py:354`, `app/chamados/services/fecho_runtime.py:142`)

Contraexemplo estático: T1 tem competências A,B e segura A+contador DP; T2 tem somente B, segura B e espera contador DP; T1 solicita B e espera T2. Ordenar âncoras não impede esse ciclo, porque a trava compartilhada do contador entra entre elas. O suporte a lote com duas competências está explicitamente exercitado. Reprodução concorrente real: **[nao verificado]**. (`app/chamados/services/fecho_runtime.py:116`, `app/chamados/services/fecho_runtime.py:122`, `app/pautas/services.py:354`, `app/chamados/tests/test_e4b_c6_memoria.py:119`, `app/chamados/tests/test_e4b_c6_memoria.py:128`)

Curas anteriores:

- **Dia civil no agrupador: curado pela classe.** Todo agrupamento passa pelo juiz, `None` sai antes da janela; competência usa o fim da janela por corte existente. Não achei `data_evento` cru em `fecho_runtime.py`. (`app/chamados/services/fecho_runtime.py:100`, `app/chamados/services/fecho_runtime.py:107`, `app/chamados/services/fecho_runtime.py:108`, `app/chamados/services/fecho_runtime.py:111`, `app/ponto/janelas.py:60`)
- **Duas Pautas pela mesma âncora: curado entre escritores E4B participantes**, por advisory determinístico adquirido antes de consultar, mantido na mesma transação da criação. **Não está implementado dentro de `pautas.services.escrever`**: essa porta continua fazendo `create` incondicional e não existe unicidade da entrega no modelo. Não achei outro emissor produtivo usando o marcador E4B. (`app/chamados/services/fecho_runtime.py:86`, `app/chamados/services/fecho_runtime.py:115`, `app/chamados/services/fecho_runtime.py:117`, `app/chamados/services/fecho_runtime.py:118`, `app/pautas/services.py:180`, `app/pautas/models.py:116`, `app/pautas/models.py:123`, `app/chamados/services/fecho_runtime.py:12`)
- **Motivo truncado: curado pela classe dos motivos produzidos.** Reserva nomes, abrevia ambas as listas preservando identificadores inteiros e contagens; produtor S143 fornece exatamente os formatos esperados pelo parser. (`app/chamados/services/fecho_runtime.py:43`, `app/chamados/services/fecho_runtime.py:64`, `app/chamados/services/fecho_runtime.py:76`, `app/chamados/services/disputa_emissao.py:1840`, `app/chamados/services/disputa_emissao.py:1844`)

Mapa de campos:

| Valor | Quem escreve/produz | Quem lê |
|---|---|---|
| ContextVar do ato | Instala ato com PK do chamado e dicionário vazio; restaura token. (`app/chamados/services/fecho_runtime.py:143`, `:144`, `:148`) | Coletor compara **PK de chamado**; não usa PK de pergunta/disputa nessa comparação. (`app/chamados/services/fecho_runtime.py:18`, `:19`) |
| Lote confirmado | Contexto cria lote, instala token e transfere perguntas depois do `yield` com saída normal; sempre restaura token. (`app/chamados/services/fecho_runtime.py:34`, `:35`, `:39`, `:40`) | Agrupador recebe somente dicionário do ato após a transição. (`app/chamados/services/fecho_runtime.py:149`, `:106`) |
| Recusas | Coletor indexa por PK da pergunta, guardando objeto+motivo. (`app/chamados/services/fecho_runtime.py:20`) | Agrupador usa objeto para dia/empresa, transporta PK e motivo; texto interpreta formato produzido pelo S143. (`app/chamados/services/fecho_runtime.py:107`, `:110`, `:113`, `:66`) |
| Dia/conversa | `abrir` fornece turno com `conversa=False`; `abrir_conversa` fornece dia civil com `conversa=True`. (`app/chamados/models.py:2789`, `:2801`) | Novo agrupador usa juiz discriminador. (`app/chamados/services/fecho_runtime.py:107`) |
| Empresa/corte | Cadastro central possui corte; porta cadastral atualiza e salva. (`app/colaboradores/models.py:46`, `app/colaboradores/services/estrutura.py:17`, `:19`, `:20`) | Agrupador usa empresa da pergunta; janela lê corte efetivo. (`app/chamados/services/fecho_runtime.py:110`, `app/ponto/janelas.py:36`, `:60`) |
| Âncora/competência | Fim da janela gera `<empresa>:AAAA-MM`; hash inclui marcador+tipo+âncora. (`app/chamados/services/fecho_runtime.py:111`, `:112`, `:86`) | Dedup usa marcador+âncora; porta serializa `ancora_id` em string; leitores gerais conservam mesmo par. (`app/chamados/services/fecho_runtime.py:118`, `app/pautas/services.py:184`, `:334`) |
| Texto | Montador limita a representação; porta persiste até 500. (`app/chamados/services/fecho_runtime.py:76`, `:79`, `app/pautas/services.py:185`) | Payload DP entrega texto e âncora sem recalcular competência. (`app/pautas/views.py:39`, `:40`, `:41`) |
| Destino/remetente/autor | Runtime fornece DP/TI/autor central; porta valida e persiste. (`app/chamados/services/fecho_runtime.py:122`, `:124`, `app/pautas/services.py:70`, `:157`, `:180`) | Listagem usa departamento; autor fica na trilha. (`app/pautas/services.py:308`, `app/pautas/models.py:105`) |
| Feita/cancelada | Portas centrais atualizam esses campos. (`app/pautas/services.py:247`, `:294`) | Dedup delega a `.vivas()`, que exclui ambas. (`app/chamados/services/fecho_runtime.py:119`, `app/pautas/models.py:57`, `:64`) |
| Status | Adaptador delega; `_gravar_estado` atribui e salva. (`app/chamados/services/fecho_runtime.py:139`, `:146`, `app/chamados/models.py:228`, `:296`) | Adaptador lê conjuntos do motor para instalar coleta. (`app/chamados/services/fecho_runtime.py:137`, `:139`) |

**FILA — alcance/rastreabilidade mantidos:** encontrar Pauta viva executa `continue`, sem acrescentar recusas de outro chamado; concluída/cancelada deixa de deduplicar. A UI recebe âncora de competência mas ainda não tem rota para abri-la; isso é preexistente e não troca competência por mês civil. (`app/chamados/services/fecho_runtime.py:118`, `:120`, `app/pautas/models.py:57`, `:64`, `app/templates/core/_copiloto.html:564`, `:570`, `:572`)

## 11/11 14:43 · sessao 5eb076058285 · FABLE-GABARITO · verificador · origem=Code

(1) **BLOQUEANTE.** Motivo literal: “CLASSE A: o diff grava estado, dinheiro”; B exige “só muda tela ou texto e não grava nada”. O custo aceito é “rótulo ou texto pode regredir sem o selo pegar”. Alterar o valor monetário persistido ultrapassa esse custo; a regra de fila para achados de forma em B não autoriza classificar essa escrita como B. (aval_proporcao_por_risco_1111.txt:1, anexo fornecido)

Entrada concreta: no mesmo arquivo, `Colab.valor_hora` retorna `self.salario / 220`; `Parcela.recalcular` executa `self.valor = self.colab.valor_hora; self.save()`. Mudar apenas `220 → 200`, com salário 2200, muda o valor gravado de 10 para 11. O consumo perde `valor_hora` porque seu receptor é `self.colab`; depois descarta os atributos gerais na comparação. (bin/selo_proporcao.py:258, bin/selo_proporcao.py:483, bin/selo_proporcao.py:524)

A reprodução em memória retornou zero achados nos dois lados; sem achados, a declaração B recebe rc 0. (bin/selo_proporcao.py:557, bin/selo_proporcao.py:676, bin/selo_proporcao.py:680)

## 11/11 14:43 · sessao 03b6d7f3c138 · - · - · origem=Ronald (tmux)

- **BLOQUEANTE — ordem de trava nova permite deadlock.** O laço adquire uma âncora, escreve a Pauta e só então pede a próxima (`app/chamados/services/fecho_runtime.py:116`, `:117`, `:122`). A porta atualiza o mesmo contador DP e conserva essa trava até o commit (`app/pautas/services.py:188`, `:352`, `:354`; `app/chamados/services/fecho_runtime.py:142`). Intercalação permitida: A possui âncoras `[a,b]`, segura `a` e contador DP; B possui `[b,c]`, segura `b` e espera contador DP; A espera `b`. Ordenar cada lote não remove o ciclo. Reprodução em duas conexões: **[nao verificado]**.

- **Dia civil do novo agrupador: curado pela CLASSE.** `fecho_runtime.py` delega ao juiz existente e exclui `None` antes da janela, sem substituição por data civil (`app/chamados/services/fecho_runtime.py:100`, `:107`, `:108`, `:111`; `app/chamados/juizes.py:332`, `:336`).

- **“Nenhum `data_evento` cru no caminho S143”: NÃO. FILA preexistente.** O cálculo da recusa de exportada continua lendo `q.data_evento` (`app/chamados/services/disputa_emissao.py:1835`, `:1836`); S128 e HX passam o atributo cru ao juiz da via (`app/chamados/services/disputa_emissao.py:1984`, `:2056`). O gate do chamado também usa a data crua (`app/chamados/models.py:364`, `:390`). A fila para esses significados já está declarada (`app/chamados/services/fio.py:1398`, `:1404`, `:1406`).

- **Coleta revertida: curada pela CLASSE para exceções propagadas e falhas levantadas ao sair do savepoint.** O `post_save` do chamado exige status encerrado e escrita do status (`app/chamados/signals.py:90`, `:96`, `:104`). O contexto coletor envolve o `atomic`, portanto recebe a saída dele antes de publicar; restaura o ContextVar sempre e faz o merge somente após sucesso (`app/chamados/signals.py:115`; `app/chamados/services/fecho_runtime.py:35`, `:39`, `:40`). O receiver conserva best-effort, capturando exceções depois dos contextos (`app/chamados/signals.py:117`). Falhas de carimbo/save propagam (`app/chamados/models.py:3138`, `:3142`, `:3148`, `:3156`). **Não achei caminho concreto** com rollback silencioso de todo o savepoint S143 e publicação do lote; cobertura desse cenário **[nao verificado]**.

- **Relógio fora da borda: não curado, FILA preexistente.** S143 fornece `hoje=None`, e C6 pode consultar `date.today()` (`app/chamados/services/disputa_emissao.py:1943`, `:1945`; `app/chamados/juizes.py:577`). Há ainda leituras de instante para carimbos, anteriores ao delta (`app/chamados/services/disputa_emissao.py:1984`, `:2041`).

- **Alcance restrito permanece.** Só as duas bordas humanas instalam o adaptador (`app/chamados/services/acoes_chamado.py:315`, `:316`; `app/chamados/services/fio.py:721`, `:722`). Fora de contexto, o coletor não faz nada (`app/chamados/services/fecho_runtime.py:13`, `:19`). `fechar()` continua chamando a transição diretamente (`app/chamados/models.py:452`, `:454`); becos, reconciliador e fantasmas continuam chamando o resolver sem instalar o ato (`app/chamados/services/becos.py:54`; `app/chamados/reconciliador.py:536`, `:1648`; `app/ponto/management/commands/reconciliar_fantasmas.py:281`). O teste de passivo planta o chamado já encerrado por SQL; não prova todos os fechamentos humanos futuros (`app/chamados/tests/test_e4b_c6_orm.py:161`, `:162`, `:165`).

- **Não achei juiz novo ou escritor paralelo no delta produtivo.** Vitalidade, tranca, HX e fechamento de disputa continuam nas autoridades existentes (`app/chamados/services/disputa_emissao.py:1964`, `:1968`, `:2007`, `:2046`, `:2089`); a persistência de Pauta continua exclusivamente pela porta (`app/chamados/services/fecho_runtime.py:122`; `app/pautas/services.py:180`).

## 11/11 14:43 · sessao 68bad0c49046 · - · - · origem=Ronald (tmux)

Censo externo a `app/chamados`, com unidade **arquivo:linha única**:

- **16 leitores de produção**, zero escritor direto.
- **6 leitores em testes**, **36 escritores diretos em testes**, mais **1 dict destinado a create** e **2 fixtures SimpleNamespace**. [nao verificado]
- **Zero colisão com outro modelo encontrada**; única definição de campo em classe: `app/chamados/models.py:2423` (`PerguntaDisputa`).
- Não há `AGENTS.md` na cópia [nao verificado por arquivo:linha; busca `rg --files` sem resultado].

| Local | Função | Uso | Conversa |
|---|---|---|---|
| `app/api/views.py:1165` | `api_chamados_v2` | ISO em resposta JSON | sim |
| `app/api/views.py:1166` | `api_chamados_v2` | Rótulo de data em JSON; comentário chama de dia do fato em `:1160` | sim |
| `app/api/views.py:2637` | `api_disputa_responder` | Repassa a mensagem de log | não se aplica |
| `app/api/views_mensageria.py:568` | `_item_validar` | ISO em JSON | sim |
| `app/colaboradores/services/calendario.py:473` | `contexto_calendario` | Filtra intervalo; import com alias `_PD` em `:469` | sim |
| `app/colaboradores/services/calendario.py:477` | `contexto_calendario` | Indexa pergunta viva por dia | sim |
| `app/folha/previa.py:65` | `montar_previa` | Filtra competência | sim |
| `app/folha/previa.py:67` | `montar_previa` | Agrupa material por dia; consumido em `:95` | sim |
| `app/ponto/management/commands/mapa_divergencia.py:272` | `Command.handle` | Dois filtros de limite e `.only('data_evento')` | sim |
| `app/ponto/management/commands/mapa_divergencia.py:273` | `Command.handle` | Marca cobertura do furo por data | sim |
| `app/ponto/management/commands/vigia_plantio_overnight.py:75` | `batidas_fora_do_turno_da_pergunta` | Filtra relação por data não nula | sim se conversa alcançar o universo; ocorrência real [nao verificado] |
| `app/ponto/management/commands/vigia_plantio_overnight.py:86` | mesma | Determina janela pela data declarada | idem |
| `app/ponto/management/commands/vigia_plantio_overnight.py:90` | mesma | Compara data declarada ao turno real | idem |
| `app/ponto/services/dia_do_colab.py:65` | `dia_do_colab` | Filtra perguntas do retrato diário | sim |
| `app/ponto/services/veredito_celula.py:180` | `fechar_o_fio_do_dia` | Filtra dia **com `conversa=False`** | não se aplica |
| `mensageria/nucleo/ferramentas.py:127` | `fila_de_validacao` | Consome JSON: `i.get('data_evento')` → `data_do_caso` | sim |

A relação de batida com pergunta está declarada em `app/ponto/models.py:40`. Os leitores de `vigia_plantio_overnight` não excluem conversa explicitamente (`app/ponto/management/commands/vigia_plantio_overnight.py:73`).

Leitores em testes: [nao verificado]

- `app/escala/tests/test_f3_beco1_cadastro_entrou.py:90`: `_perguntas_no_passado`, filtro `data_evento__lt`.
- `app/escala/tests/test_f3_beco1_cadastro_entrou.py:129`, `:133`, `:135`: `test_RED_caso_107_folga_em_01_09_rejulga_22_e_27_09_e_poupa_a_exportada`, filtros por dia para conferir emissão/preservação.
- `app/ponto/tests/test_bateria_lavratura.py:1194`: `test_O222_o_furo_ja_sai_cobrado_na_PRIMEIRA_passada`, filtro por dia e contagem.
- `app/ponto/tests/test_cobparc_marco_no_periodo.py:386`: `_perguntas_vivas`, filtro por dia.

Escritores em testes, **36 linhas**: [nao verificado]

```text
app/api/tests/test_api_mensageria_validar.py:85
app/api/tests/test_atestado_na_resposta.py:34
app/api/tests/test_falta_na_resposta.py:41
app/colaboradores/tests/test_calendario_ultimo_recurso_le_o_c6.py:36
app/colaboradores/tests/test_desligar_guards.py:102
app/colaboradores/tests/test_desligar_guards.py:108
app/core/tests/test_ia_cobrar.py:94
app/core/tests/test_selo_performance.py:321
app/core/tests/test_selo_placar_lavrado.py:55
app/escala/tests/test_cobranca_orfa_regeneracao.py:342
app/escala/tests/test_f3_beco1_folga_na_lista.py:53
app/escala/tests/test_f3_beco1_folga_na_lista.py:146
app/escala/tests/test_f3_beco1_folga_na_lista.py:188
app/ponto/tests/test_bug114_fechador_de_dois_dias.py:238
app/ponto/tests/test_cobparc_v4_procedentes.py:230
app/ponto/tests/test_cxr_tela_le_o_motor.py:363
app/ponto/tests/test_declarar_dia.py:174
app/ponto/tests/test_fechar3_troca_escala_gate.py:202
app/ponto/tests/test_fechar3_troca_escala_gate.py:252
app/ponto/tests/test_fechar3_troca_escala_gate.py:301
app/ponto/tests/test_fio_resolve_cron.py:67
app/ponto/tests/test_justificativa_encerra_origem.py:312
app/ponto/tests/test_justificativa_encerra_origem.py:354
app/ponto/tests/test_justificativa_encerra_origem.py:515
app/ponto/tests/test_justificativa_encerra_origem.py:556
app/ponto/tests/test_jz1_autoridade_unica.py:36
app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:83
app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:60
app/ponto/tests/test_orfas_vigia.py:66
app/ponto/tests/test_procedencia_batida.py:66
app/ponto/tests/test_procedencia_batida.py:79
app/ponto/tests/test_procedencia_batida.py:124
app/ponto/tests/test_reconciliar_fantasmas.py:84
app/ponto/tests/test_selo_chokepoint_escrita.py:271
app/ponto/tests/test_selo_chokepoint_escrita.py:306
app/ponto/tests/test_x1_folga_sem_adesao.py:384
```

`app/api/tests/test_api_mensageria_validar.py:85` é `Store`; sua persistência está em `:86` com `save(update_fields=['data_evento'])`. Os outros 35 são kwargs em `create`/`abrir`.

Escritores auxiliares:

- `app/ponto/tests/test_vassoura_orfas_resposta.py:63`: `VassouraOrfasNaoEnterraRespostaTest._perg`, `dict(data_evento=...)`; consumido por `PerguntaDisputa.objects.create(**kw)` em `:66`.
- `app/escala/tests/test_signal_ancora.py:144`: `RebaseE2LeitoresTest._confere_avulso`, fixture `NS(conversa=True, data_evento=...)`.
- `app/escala/tests/test_signal_ancora.py:381`: `RebaseE2ContratoRelogioTest._confere_backfill`, fixture `NS(data_evento=...)`.

As strings de testes que contêm código mutante/documentação, como `app/escala/tests/test_signal_ancora.py:158`, não são leitura AST executável do atributo e devem ficar fora da allowlist de leitores.

## 11/11 14:43 · sessao a7d7fc29f2f0 · - · - · origem=Ronald (tmux)

Auditoria das provas, sem executar testes nem acessar banco: [nao verificado]

- **Não achei falso verde demonstrável nos quatro RED c7.** Há limites relevantes: o lock ORM é ensaiado serialmente, na mesma conexão, e o segundo chamado/colaborador é `SimpleNamespace`; prova a ordem SQL `lock→exists` e a reentrega, sem demonstrar concorrência entre duas conexões. **FILA de comprovação**, sem concluir que a trava está errada. (`app/chamados/tests/test_e4b_c6_orm.py:187`, `app/chamados/tests/test_e4b_c6_orm.py:196`, `app/chamados/tests/test_e4b_c6_orm.py:203`, `app/chamados/tests/test_e4b_c6_orm.py:208`, `app/chamados/tests/test_e4b_c6_orm.py:210`)

- **O fake de memória aceita a escrita imediatamente:** apenas acrescenta os argumentos à lista; `atomic` somente controla profundidade, sem desfazer objetos ou escritas. Portanto os verdes de memória não certificam a porta nem rollback real. Isso está compensado parcialmente pelas provas ORM, não ocultado por elas. (`app/chamados/tests/test_e4b_c6_memoria.py:36`, `app/chamados/tests/test_e4b_c6_memoria.py:62`, `app/chamados/tests/test_e4b_c6_memoria.py:65`)

- **Texto não passa só pelo truncamento da porta:** o ORM envolve `escrever` com `wraps`, verifica o comprimento do argumento anterior à porta e exige igualdade com o texto persistido. O ensaio de memória exige contagens exatas de perguntas/limbo, além dos dois motivos preservados. (`app/chamados/tests/test_e4b_c6_orm.py:224`, `app/chamados/tests/test_e4b_c6_orm.py:226`, `app/chamados/tests/test_e4b_c6_orm.py:228`, `app/chamados/tests/test_e4b_c6_memoria.py:191`, `app/chamados/tests/test_e4b_c6_memoria.py:199`)

- **Rollback ORM tem mordida material:** o resolver substituto coleta, faz `UPDATE` real e levanta exceção; depois o teste exige chamado fechado, `UPDATE` revertido e nenhuma Pauta. A captura best-effort da exceção não basta para fazê-lo passar. (`app/chamados/tests/test_e4b_c6_orm.py:237`, `app/chamados/tests/test_e4b_c6_orm.py:246`, `app/chamados/tests/test_e4b_c6_orm.py:249`, `app/chamados/tests/test_e4b_c6_orm.py:250`, `app/chamados/tests/test_e4b_c6_orm.py:251`)

- **Saída do savepoint também tem controle**, porém somente em memória: exige que a nova coleta ainda não esteja no pai durante a saída, simula erro nessa saída e preserva lote anterior. O fechamento da variável `falha` é consumido dentro da própria iteração; não achei execução adiada que produzisse o falso verde sugerido pelo B023. (`app/chamados/tests/test_e4b_c6_memoria.py:260`, `app/chamados/tests/test_e4b_c6_memoria.py:264`, `app/chamados/tests/test_e4b_c6_memoria.py:268`, `app/chamados/tests/test_e4b_c6_memoria.py:272`, `app/chamados/tests/test_e4b_c6_memoria.py:275`)

- **Conversa:** o ORM cria a conversa pela porta e certifica `conversa=True` e dia civil preenchido; entretanto injeta o motivo de recusa e chama o agrupador diretamente. Demonstra a cura do agrupador, não uma execução integral de S143 para conversa. A memória acrescenta o caso de pergunta factual sem data. (`app/chamados/tests/test_e4b_c6_orm.py:174`, `app/chamados/tests/test_e4b_c6_orm.py:178`, `app/chamados/tests/test_e4b_c6_orm.py:182`, `app/chamados/tests/test_e4b_c6_memoria.py:165`)

- **Não achei fixture invisível de “viva/morta” no cenário ORM:** as precondições consultam C6 e entrega antes do fechamento; o caso usa corte 24 e datas do mesmo mês civil em competências diferentes. A prova de memória do corte substitui o fake de janela pelo derivador real. (`app/chamados/tests/test_e4b_c6_orm.py:23`, `app/chamados/tests/test_e4b_c6_orm.py:24`, `app/chamados/tests/test_e4b_c6_orm.py:78`, `app/chamados/tests/test_e4b_c6_orm.py:82`, `app/chamados/tests/test_e4b_c6_memoria.py:119`, `app/chamados/tests/test_e4b_c6_memoria.py:124`)

- **Gate de mutantes tem mordida:** exige GREEN anterior; depois exige rc=1, um teste, um failure, nome do caso, `AssertionError` e mensagem-alvo, rejeitando erro/precondição. A instrumentação altera `__code__` do objeto receiver registrado e o restaura em `finally`, com controle explícito no erro. (`app/chamados/tests/e4b_c6_verificar.py:187`, `app/chamados/tests/e4b_c6_verificar.py:199`, `app/chamados/tests/e4b_c6_verificar.py:75`, `app/chamados/tests/e4b_c6_verificar.py:94`, `app/chamados/tests/e4b_c6_verificar.py:117`, `app/chamados/tests/test_e4b_c6_memoria.py:237`)

- **Limites restantes:** não achei nesses testes ensaio de falha real de `pautas.services.escrever`; o erro antigo exercitado ocorre durante `transicionar`. O teste de passivo continua plantando o chamado já encerrado e não cobre todos os fechamentos futuros fora das duas bordas. (`app/chamados/tests/test_e4b_c6_memoria.py:108`, `app/chamados/tests/test_e4b_c6_memoria.py:113`, `app/chamados/tests/test_e4b_c6_orm.py:157`, `app/chamados/tests/test_e4b_c6_orm.py:162`)

- O dado fornecido informa GREEN 398 e 91 e sucesso dos roteiros; não demonstra suíte inteira nem concorrência real em duas conexões. **[nao verificado]** [nao verificado]

## 11/11 14:41 · sessao b077e75a6436 · FABLE-GABARITO · verificador · origem=Code

1. **FILA.** O aval determina literalmente: “achado do Codex sobre a FORMA de testar (como o selo poderia ser enganado) vira linha de fila, nunca volta”. O salto perdido é um escape da detecção; isso não torna B a obra que grava. (`aval_proporcao_por_risco_1111.txt:1`, anexo fornecido, itens 1–2)

   Entrada concreta: no mesmo módulo, `h()` retorna `1`, `g()` retorna `h()`, e `f(obj)` faz `obj.valor = g(); obj.save()`. Alterar somente o retorno de `h()` para `2` passa B: o consumo procura interseção direta com unidades já marcadas como escritoras, sem propagar pelo intermediário. (`bin/selo_proporcao.py:516-529`; `bin/selo_proporcao.py:676-680`)

2. **BLOQUEANTE quanto à atribuição de “custo aceito no aval”.** O custo literal autorizado é “rotulo ou texto pode regredir sem o selo pegar; quem pega e o meu smoke”; B exige que o diff “nao grava nada”. Chamar escrita entre módulos de custo autorizado amplia esse aval. (`aval_proporcao_por_risco_1111.txt:1`, anexo fornecido, itens 1–2 e frase final)

   Entrada concreta: `leitura.py` muda `h(): return 1` para `return 2`; outro módulo mantém `obj.valor = leitura.h(); obj.save()`. Esse escape está expressamente documentado na cópia, que também declara que o aval **só aceitou rótulo/texto**. A redação alegada no contrato e no commit está **[nao verificado]**. (`bin/selo_proporcao.py:33-37`; `bin/selo_proporcao.py:641-646`)

## 11/11 14:40 · sessao 57d93ad8e30c · - · - · origem=Ronald (tmux)

(b) **4 leituras comprovadas em 2 templates**, mais **1 expressão com variável não vinculada**:

| Local | Origem/contexto | Uso | Diverge para conversa |
|---|---|---|---|
| `app/templates/chamados/partials/_card_atendimento.html:87` | `p` no laço `chamado.perguntas_disputa` (`:77`); perguntas carregadas por `montar_chamados_fio`, `app/chamados/services/fio.py:225`, agrupadas `:243`, anexadas `:253`; view `app/chamados/views.py:569`, contexto `:639`, `:640`; includes `app/templates/chamados/meu_atendimento.html:127`, `:144` | Testa presença para mostrar cabeçalho “Estamos falando do dia” (`_card_atendimento.html:89`) | **sim**: conversa possui data civil; juiz retorna `None`, `app/chamados/juizes.py:338` |
| `app/templates/chamados/partials/_card_atendimento.html:90` | Mesmo contexto | Exibe como dia de referência/fato sob aquele cabeçalho | **sim**, `app/chamados/juizes.py:338` |
| `app/templates/chamados/partials/_card_atendimento.html:164` | Mesmo contexto | Exibe dia na confirmação JS de opção “Não trabalhou nesse dia?” (`:160`) | **sim**, se pergunta de conversa alcançar esse ramo; ocorrência concreta em produção **[nao verificado]** |
| `app/templates/chamados/partials/_lote_preview.html:24` | `x.pergunta` de `linhas`; view `app/chamados/views_cobrar.py::lote_preview:361`, `:363`; serviço `app/chamados/services/validacao.py::fila_validar_classificada:921`, `:923` coloca instâncias de pergunta no dicionário | Exibe dia na prévia de validação | **sim**, se conversa alcançar classe A; alcance concreto **[nao verificado]**; juiz `app/chamados/juizes.py:338` |

A expressão `app/templates/chamados/meu_atendimento.html:19` usa `p.data_evento` fora de um laço de perguntas; seu formulário está no laço de `reg`, iniciado `:10`. O contexto de `meu_atendimento` não fornece `p` (`app/chamados/views.py:633–652`). Portanto, **não é leitura comprovada de PerguntaDisputa**; divergência **não se aplica**. Há cinco expressões `.data_evento` executáveis ao todo, mas quatro com origem comprovada.

`app/templates/chamados/partials/modal_fio.html:161` contém somente comentário; não integra leitores.

(c) **2 produtores de chave JSON `data_evento`, em 2 endpoints; 1 chave derivada `data_evento_label`; 1 consumidor explícito encontrado na cópia**:

| Local | Função/rota | Uso | Diverge para conversa |
|---|---|---|---|
| `app/api/views.py:1165` | `api_chamados_v2`, rota `app/api/urls.py:73` | Serializa `p.data_evento` em ISO ou `None`; perguntas pertencem a `perguntas_por_chamado`, `app/api/views.py:1040`, `:1046`, `:1174`; resposta `:1189` | **sim** no valor transmitido, juiz `app/chamados/juizes.py:338`; exibição pelo APK **[nao verificado]** |
| `app/api/views.py:1166` | Mesmo endpoint; chave derivada `data_evento_label` | Formata o mesmo valor como `%d/%m/%Y`; resposta `:1189` | **sim**, juiz `app/chamados/juizes.py:338`; exibição pelo APK **[nao verificado]** |
| `app/api/views_mensageria.py:568` | `_item_validar`; usado por `api_mensageria_validar`, `:546`; rota `app/api/urls.py:12`; resposta `views_mensageria.py:549` | Serializa ISO ou `''`; só repassa | **sim** no valor transmitido, juiz `app/chamados/juizes.py:338` |
| `mensageria/nucleo/ferramentas.py:127` | `fila_de_validacao` (`:98`) | Lê `i.get('data_evento')`, renomeia para `data_do_caso`, repassa ao contexto; recebe payload `:107`, percorre itens `:129` | **sim** no valor repassado; uso final pela IA **[nao verificado]** |

O consumidor HTTP da mensageria chama `/api/mensageria/validar/` em `mensageria/nucleo/core_client.py:50` e devolve JSON integral em `:55`. As outras chamadas à fila em `mensageria/nucleo/ferramentas.py:190` e `:1529` usam classe/referências/contagens, sem ler a chave (respectivamente `:194–205` e `:1532–1543`).

Consumidores JS ou código do APK para essas chaves: **[nao verificado]**. Não apareceu literal `data_evento` nos arquivos JS/TS/JSX/TSX/Vue/Dart/Kotlin/Swift pesquisados **[nao verificado]**.

Observação para contagem AST de (a): `api/views.py:1165`, `:1166` e `api/views_mensageria.py:568` contêm **dois acessos de atributo cada** (teste condicional + formatação), portanto **6 nós AST em 3 posições arquivo:linha**. O consumidor de dicionário é mais uma posição: `mensageria/nucleo/ferramentas.py:127`.

_3926 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
