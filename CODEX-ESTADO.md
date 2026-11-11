# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 20:35** · publicado em **11/11/2026 20:39** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 20:35 · sessao 3e51f6e3629d · CASO-0711-HJ003 · sonda · origem=Code

**A regra atual é “disputa aberta mais recente do colaborador”, sem filtro pelo dia da pergunta.** Isso permite que uma disputa aberta em um dia receba perguntas de outros dias. A eleição usa `.first()` sobre a ordenação `-aberta_em`. (`app/chamados/services/disputa_emissao.py:531`; `app/chamados/models.py:1804`.)

**A sonda confirma a ligação 7279 → 33492, mas não identifica o chamador histórico que inseriu cada pergunta.** Esses valores são dado fornecido, sem verificação independente: **[nao verificado]**. A cópia também não contém metadados Git; identidade criptográfica com HEAD/PROD: **[nao verificado]**.

Referências abreviadas:

| Sigla | Arquivo |
|---|---|
| M | [app/chamados/models.py](/home/ronald/codex_ws/df2be6b12d7cd47a5b9ce0fda766382308c2c5b8/app/chamados/models.py) |
| E | [app/chamados/services/disputa_emissao.py](/home/ronald/codex_ws/df2be6b12d7cd47a5b9ce0fda766382308c2c5b8/app/chamados/services/disputa_emissao.py) |
| F | [app/chamados/services/fio.py](/home/ronald/codex_ws/df2be6b12d7cd47a5b9ce0fda766382308c2c5b8/app/chamados/services/fio.py) |
| J | [app/chamados/juizes.py](/home/ronald/codex_ws/df2be6b12d7cd47a5b9ce0fda766382308c2c5b8/app/chamados/juizes.py) |
| MT | [app/chamados/services/materializacao.py](/home/ronald/codex_ws/df2be6b12d7cd47a5b9ce0fda766382308c2c5b8/app/chamados/services/materializacao.py) |

**1) F0 DELTA — escritores encontrados.**

A tabela separa atribuição e persistência; construtor, proxy e pedido a uma porta não são automaticamente escritores persistentes. Exemplos concretos: inserção real em `M:3017`, proxy em `M:2876`, simulação restaurada em `F:779`/`F:783`.

| Sítio | Campos e operação persistente | Comparação com `f0f1.r` |
|---|---|---|
| `PerguntaDisputa._nascer` | Insere `P.disputa` e `P.chamado_destino`: construtor `M:3014`, save `M:3017`. | Mesmo endereço. |
| `_nascer`, reuso | Troca `P.chamado_destino`: atribuição `M:2982`, save com lista dinâmica `M:3006`. Retorna a pergunta existente, preservando sua dona: `M:3008`. | Mesmo endereço. |
| `reapontar_resolvedora` | Troca `P.chamado_destino`, inclusive para `None`: `F:925`, save `F:927`. | Mesmo endereço. |
| `_abrir_ou_agregar_disputa` | Insere DS e sua FK `chamado`: `E:587`, valor em `E:591`. | Mesmo endereço. |
| `criar_disputa_a_partir_de_chamados_orfaos` | Insere DS e sua FK `chamado`: `E:1123`, valor em `E:1127`. | Mesmo endereço. |
| `criar_disputa_ancora_de_chamado` | Insere DS e sua FK `chamado`: `E:1358`, valor em `E:1362`. | Mesmo endereço. |
| Nascimentos da DS acima | `aberta_em` pelo ORM, `auto_now_add=True`; fechamento inicialmente nulo; resolução e motivo vazios. (`M:1756`; `M:1757`; `M:1773`; `M:1793`.) | Nascimentos já listados; o script anterior não pesquisava todos esses campos. Comparação do script anexado: **[nao verificado]**. |
| `DisputaSupervisao.fechar` | `fechada_em=now`, `resolucao=argumento`, `motivo_fechamento=motivo`: `M:1932`, `M:1934`, `M:1935`; save `M:1936`. | Mesmo endereço. |
| `reabrir_admin` | Limpa os três campos: `M:2149`, `M:2151`, `M:2152`; save `M:2161`. | Mesmo endereço. |
| `reabrir_sistema` | Limpa os três campos: `M:2173`, `M:2175`, `M:2176`; save `M:2177`. | Mesmo endereço. |
| **`limpar_resolucao_residual`** | **Escreve `resolucao=''`: `M:2265`, save `M:2266`; somente aberta com texto residual: `M:2262`.** | **Sítio adicional ao inventário anterior. Ser código acrescentado depois das 02h: [nao verificado].** |

**Delta demonstrável:** o inventário anterior omitiu `limpar_resolucao_residual`; os escritores principais que ele enumerou continuam nos mesmos endereços da tabela. Nenhum deles desapareceu ou mudou de endereço. A classificação temporal “criado/movido/removido desde as 02h” exige a árvore anterior, ausente: **[nao verificado]**. (`M:2265`; demais endereços acima.)

Há deslocamentos em **chamadas**, sem mudança do escritor final: os fechamentos em materialização aparecem agora em `MT:591` e `MT:1338`, e a reabertura em `MT:691`; no anexo eram `586`, `1333` e `686`, respectivamente. Endereços anteriores: **[nao verificado]**; atuais conferidos: `MT:591`, `MT:1338`, `MT:691`.

Os efeitos e escapes também pertencem ao censo:

| Forma | Classificação conferida |
|---|---|
| `M:367` | Memória: reassocia instância do mesmo chamado após conferir o PK em `M:366`. |
| `M:2876` | Proxy para o juiz do dia; inserção real ocorre em `M:3014`/`M:3017`. |
| `F:779`, `F:783` | Simula destino e restaura no `finally`; não persiste essas atribuições. |
| Exclusão de chamado | Pode zerar `DS.chamado` e `P.chamado_destino` por `SET_NULL`. (`M:1751`; `M:3387`.) |
| Exclusão da DS | Elimina P por `CASCADE`; não transfere sua dona. (`M:2418`.) |
| `**campos` do nascimento | Expansão chega ao construtor; não existe restrição geral de nomes nessa assinatura. (`M:2776`; `M:2789`; `M:2820`; `M:3014`.) |
| `descarimbar_em_lote(**outros)` | Capacidade genérica latente: a guarda proíbe somente campos do carimbo; o update recebe os demais. (`M:3255`; `M:3264`.) Os chamadores nominais atuais passam campos de resposta/validação. (`M:1566`; `E:1593`; `E:1630`.) |
| `PerguntaDisputa.save` | Persistência genérica por `super().save`, dentro de atomic; não elege uma dona ou destino. (`M:3413`; `M:3422`; `M:3423`.) |
| `regularizacao_ext.py:256` | `reg.chamado` pertence a outra entidade. (`app/chamados/services/regularizacao_ext.py:256`.) |
| Migration `0051`, update | Escreve `colaborador_id`; lê `disputa_id` no `OuterRef`. (`app/chamados/migrations/0051_decisao_pergunta.py:25`.) |

As migrations pertinentes definem os campos, sem constituírem eleições adicionais de runtime: DS em `app/chamados/migrations/0013_disputasupervisao.py:23`; dona em `app/chamados/migrations/0014_perguntadisputa.py:27`; destino em `app/chamados/migrations/0016_s84_pergunta_chamado_destino.py:18`; motivo do fechamento em `app/chamados/migrations/0049_disputa_motivo_fechamento.py:14`.

O script abaixo enumera candidatos e mantém fila explícita para receptores desconhecidos, expansões, dicionários, reflexão e SQL. A tabela acima é a classificação após leitura dos corpos. Ausência universal de escrita por reflexão ou SQL montado dinamicamente: **[nao verificado]**.

```python
import ast
import sys
from collections import Counter
from pathlib import Path

FIELDS = {
    "disputa", "disputa_id", "chamado_destino", "chamado_destino_id",
    "aberta_em", "fechada_em", "resolucao", "motivo_fechamento",
    "chamado", "chamado_id",
}
MODELS = {"PerguntaDisputa", "DisputaSupervisao"}
OPS = {"create", "get_or_create", "update_or_create",
       "update", "bulk_create", "bulk_update", "save"}
PORTAS = {"cls", "abrir", "abrir_conversa", "_sob_contrato", "_nascer"}
rows, queue, errors = [], [], []

def strings(node):
    return {n.value for n in ast.walk(node)
            if isinstance(n, ast.Constant) and isinstance(n.value, str)}

for path in sorted(Path(".").rglob("*.py")):
    if (set(path.parts) & {".git", "__pycache__", "node_modules", "tests"}
            or path.name == "tests.py"
            or path.name.startswith(("test_", "tests_"))):
        continue
    try:
        tree = ast.parse(path.read_text(encoding="utf8"), str(path))
    except (SyntaxError, UnicodeError, OSError) as exc:
        errors.append((str(path), str(exc)))
        continue
    parents = {c: n for n in ast.walk(tree) for c in ast.iter_child_nodes(n)}
    aliases = {a.asname or a.name: a.name
               for n in ast.walk(tree) if isinstance(n, ast.ImportFrom)
               for a in n.names}

    def owners(node):
        result = []
        while node in parents:
            node = parents[node]
            if isinstance(node, (ast.ClassDef, ast.FunctionDef,
                                 ast.AsyncFunctionDef)):
                result.append(node.name)
        return result[::-1]

    def emit(node, kind, pending=False):
        section = "migration" if "migrations" in path.parts else "runtime"
        (queue if pending else rows).append(
            f"{path}:{node.lineno}|{section}|{'.'.join(owners(node))}|{kind}")

    for node in ast.walk(tree):
        if (isinstance(node, ast.Attribute)
                and isinstance(node.ctx, ast.Store) and node.attr in FIELDS):
            emit(node, "ATR " + ast.unparse(node))
        if (isinstance(node, ast.Assign)
                and isinstance(parents.get(node), ast.ClassDef)
                and parents[node].name in MODELS):
            for target in node.targets:
                if isinstance(target, ast.Name) and target.id in FIELDS:
                    emit(node, "SCHEMA " + target.id)
        if isinstance(node, (ast.Dict, ast.Subscript)) and strings(node) & FIELDS:
            emit(node, "REVER dict/subscript", True)
        if not isinstance(node, ast.Call):
            continue
        end = getattr(node.func, "attr", getattr(node.func, "id", ""))
        end = aliases.get(end, end)
        root = ast.unparse(node.func).split(".")[0]
        root = aliases.get(root, root)
        direct = {k.arg for k in node.keywords} & FIELDS
        known = False
        if (end in {"CreateModel", "AddField", "AlterField"}
                and strings(node) & (FIELDS | {
                    "perguntadisputa", "disputasupervisao"})):
            emit(node, "SCHEMA migration " + end)
        if direct and end in OPS | PORTAS | MODELS:
            emit(node, "KW " + ",".join(sorted(direct)))
            known = True
        if root in MODELS and end in {
                "create", "get_or_create", "update_or_create"}:
            emit(node, "[nome]/defaults " + root)
            known = True
        if end in OPS and strings(node) & FIELDS:
            emit(node, "REVER lista/expressao " + end)
            known = True
        if end in OPS | PORTAS | MODELS:
            if not known or any(k.arg is None for k in node.keywords):
                emit(node, "REVER receptor/kwargs " +
                     ast.unparse(node.func), True)
        if end in {"setattr", "execute", "executemany",
                   "raw", "RawSQL", "RunSQL"}:
            emit(node, "REVER reflexao/SQL " + end, True)
        if end in {"reapontar_resolvedora", "descarimbar_em_lote"}:
            emit(node, "DELEGACAO " + end)

for row in sorted(set(rows)):
    print(row)
print("FILA:", dict(Counter(
    r.rsplit("|", 1)[-1].split()[1] for r in queue)))
if "--fila" in sys.argv:
    print("\n".join(sorted(set(queue))))
for path, error in errors:
    print("ERRO:", path, error)
if errors:
    raise SystemExit(2)
```

O selo existente permite arquivos, não funções escritoras: destino admite `models.py` e `fio.py`; sua detecção usa regex de atribuição/update e exclui migrations. Portanto, sozinho não prova Single Writer por função. (`app/core/tests/test_contract_escritores_estado.py:61`; `:179`; `:191`.)

**2) Caso-prova 7279 — cada pergunta e sua eleição.**

As seis perguntas vinculadas à 7279 no dado fornecido são estas; a atribuição histórica individual a agrupador, fábrica, declaração ou API permanece **[nao verificado]**. O escritor normal atual de `P.disputa` é o construtor em `M:3014`, persistido em `M:3017`; reuso preserva a dona em `M:3008`.

| Pergunta | `data_evento` fornecida | `chamado_destino_id` fornecido | Sítio atual que insere a dona; autoria histórica |
|---|---|---|---|
| 47210 | 22/10/2026 | 33491 | `M:3014` → `M:3017`; histórico **[nao verificado]**. |
| 51727 | **29/10/2026** | 35801 | `M:3014` → `M:3017`; histórico **[nao verificado]**. |
| 51823 | 30/10/2026 | `None` | `M:3014` → `M:3017`; histórico **[nao verificado]**. |
| 52340 | 31/10/2026 | `None` | `M:3014` → `M:3017`; histórico **[nao verificado]**. |
| 53458 | 02/11/2026 | 36718 | `M:3014` → `M:3017`; histórico **[nao verificado]**. |
| 53628 | 03/11/2026 | 36809 | `M:3014` → `M:3017`; histórico **[nao verificado]**. |

As outras perguntas mostradas pela sonda pertencem a outras disputas: 47199 → 7272; 49408 → 7431. Dado fornecido: **[nao verificado]**.

Os caminhos atuais que podem entregar perguntas à dona são:

| Caminho | Eleição da DS e chegada ao escritor |
|---|---|
| Emissão comum/fábrica | Núcleo seleciona aberta em `E:531`; wrapper gera perguntas também no reuso em `E:682`; pedido de nascimento em `E:444`. A fábrica chega pelo wrapper em `app/chamados/services/fabrica_da_celula.py:172`. |
| Agrupamento de órfãos | Seleciona aberta em `E:1096`; pedidos passam por `_criar_pergunta_se_couber`, `E:837`. |
| Declaração do colaborador | Obtém DS em `app/chamados/services/declaracao_texto.py:360`; abre P em `:376`; seleção delegada ao núcleo em `E:627`. |
| Declaração administrativa | Obtém DS em `app/chamados/services/declaracao_texto.py:659`; abre P em `:666`; mesmo núcleo em `E:627`. |
| Âncora | Seleciona aberta em `E:1332`; abre conversa em `E:1371`. |
| Complemento de P existente | Pode usar diretamente a dona da pergunta encontrada, em vez da aberta global. (`E:1070`; `E:1073`; `E:1081`.) |

Nos três seletores de abertura, a expressão é:

```python
DisputaSupervisao.objects.filter(
    colaborador=colaborador, fechada_em__isnull=True,
).first()
```

Ela mora em `E:531`, `E:1096` e `E:1332`; a ordenação é `M:1804`. Portanto, é **`.first()` da mais recente aberta do colaborador**, sem condição sobre `data_evento`; não é eleição por dia. O helper leitor `colaborador_em_disputa` repete essa consulta em `E:40`.

**Se 7279 era a mais recente aberta no instante da emissão, esses caminhos a elegiam mesmo para outro dia.** O estado completo naquele instante e o caminho percorrido por cada PK: **[nao verificado]**. A possibilidade vem dos seletores acima; perguntas de dia novo são solicitadas também no reuso em `E:682`.

**3) Chamado 33492 ↔ disputa 7279.**

Pelo dado fornecido, **sim**: `7279.chamado_id=33492` e a consulta inversa devolveu `[7279]`; ambos são **[nao verificado]** independentemente. O modelo confirma FK nullable com `SET_NULL` e reversa `disputa_origem`; não existe unicidade nessa definição. (`M:1749`.)

Quem grava a ligação:

| Nascimento | Quem obtém o chamado e grava a FK |
|---|---|
| Núcleo | Obtém por `ChamadoColaborador.abrir` em `E:568`; grava em `E:591`. |
| Agrupador | Obtém o pai em `E:1104`; grava em `E:1127`. |
| Âncora | Obtém o pai em `E:1339`; grava em `E:1362`. |

Os ramos de reuso mantêm o chamado-pai e a abertura; alteram motivos/contexto. (`E:541`; `E:562`; `E:565`; `E:1131`; `E:1366`.) A assinatura de 33492 é compatível com o agrupador, mas atribuir seu nascimento histórico a esse caminho é **[nao verificado]**.

A apresentação combina dois leitores:

- `chamado_que_lista` escolhe `P.chamado_destino_id`; quando nulo, usa `DS.chamado_id`. (`F:116`; `F:117`.)
- O modal mostra **`Chamado.criado_em`**; o card também agrupa por essa data. (`app/templates/chamados/partials/modal_fio.html:86`; `app/templates/chamados/partials/_card_atendimento.html:5`.)

Aplicando essa regra às FKs fornecidas, **51823 e 52340 seriam listadas no pai 33492**, enquanto as outras quatro têm os destinos próprios indicados na tabela. A execução efetiva da tela: **[nao verificado]**. Regra: `F:117`, `F:240`, `F:243`.

Isso permite “chamado criado em 22/10 contendo pergunta de 30/10/31/10”: a data do cabeçalho é a criação do chamado, e as perguntas têm suas próprias datas. O catálogo declara os containers de disputa sem chave de dia. (`M:67`; `app/templates/chamados/partials/modal_fio.html:86`; `app/chamados/catalogo/modulos.py:144`; `:146`; `:223`.)

Quanto ao **`em_analise`**, a agregação não renova o pai por dia. Enquanto a DS está aberta, o receiver de fechamento do pai retorna; depois do fechamento, tenta `ch.reconciliar()`, que ainda pode recusar. (`E:531`; `E:566`; `app/chamados/signals.py:28`; `:42`.) O ato específico que deixou 33492 em análise e eventuais tentativas de encerramento são **[nao verificado]**.

**4) Fechamento — condição, chamadores e limite da prova.**

C7 exige simultaneamente:

```python
not self.respostas_sem_veredito()
and not fio_vivo(list(self.perguntas.all()))
```

Esse é o corpo de `pode_fechar`, em `M:1853`. A porta `fechar` recusa primeiro respostas sem veredito (`M:1886`), depois perguntas vivas no fechamento ordinário (`M:1895`); a escrita ocorre em `M:1932`.

| Pergunta | Bloqueio por resposta sem veredito, aplicando `M:1843` ao dado |
|---|---|
| 47210 | Não entra: validação/materialização preenchidas no dado. **[nao verificado]** |
| **51727** | **Entra se não houver tranca aplicável:** resposta registrada, validação e materialização nulas. **[nao verificado]** |
| 51823 | Não entra: materialização preenchida no dado. **[nao verificado]** |
| 52340 | Não entra: materialização preenchida no dado. **[nao verificado]** |
| 53458 | Não entra: validação/materialização preenchidas no dado. **[nao verificado]** |
| 53628 | Não entra: materialização preenchida no dado. **[nao verificado]** |

**51727 fornece o bloqueio concreto condicionado à tranca:** basta o timestamp da resposta; não é necessário conhecer seu texto para essa lista. (`M:1843`; `M:1844`; `M:1846`.) A sonda não trouxe a tranca nem os toques posteriores usados pelo juiz; por isso, o retorno efetivo de `pode_fechar(7279)` é **[nao verificado]**. (`J:974`; `J:991`.)

Materialização preenchida também não prova, sozinha, ausência de pergunta viva: C6 consulta via, marco, célula e teto. A vitalidade exata das perguntas sem validação não pode ser reconstruída integralmente com o dado entregue. (`J:1396`; `J:1428`; `J:1432`; `J:1445`; resultado concreto: **[nao verificado]**.)

Chamadas diretas encontradas à porta `DisputaSupervisao.fechar`:

| Contexto | Sítio atual |
|---|---|
| Foto recebida | `app/api/foto_recebida.py:80` |
| Reconciliador: sem turno aberto | `app/chamados/reconciliador.py:412` |
| Reconciliador: absolvição pela célula | `app/chamados/reconciliador.py:612` |
| Reconciliador: evento da célula | `app/chamados/reconciliador.py:1171` |
| Porta administrativa com motivo | `M:1950` |
| Remoção do último motivo | `M:2072` |
| Desligamento | `app/colaboradores/services/desligamento.py:142` |
| Entrada sincronizada | `app/ponto/services/entrada_sincronizada.py:59` |
| Materialização de ausência | `MT:591` |
| Materialização: disputa quitada | `MT:1338` |
| Autofecho do agrupador | `E:1284` |
| Resolução por chamado resolvido | `E:1964` |
| Competência trancada | `app/chamados/services/competencia_trancada.py:204` |
| Resolução de becos | `app/chamados/services/becos.py:57` |
| Comando de perguntas com fato | `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88` |

O juiz também é consultado separadamente na classificação de becos e no planejamento da competência trancada. (`app/chamados/services/becos.py:33`; `app/chamados/services/competencia_trancada.py:153`.)

**“Nunca fechou” não está provado pelo snapshot.** Ele informa aberta no instante fornecido: **[nao verificado]**. `reabrir_sistema` limpa fechamento sem consumir o contador administrativo, portanto os contadores zerados não excluem fechamento seguido de reabertura automática. (`M:2163`; `M:2173`; `M:2177`.) Quais chamadores executaram ou recusaram para 7279: **[nao verificado]**.

Os escritores e leitores dos campos usados no diagnóstico ficam assim; os leitores são os relevantes às respostas, sem pretensão de cobrir todas as telas:

| Campo | QUEM ESCREVE | QUEM LÊ |
|---|---|---|
| `P.disputa` | Nascimento `M:3014`/`M:3017`; escape genérico `M:3264`. | Identidade `M:3430`; roteamento `F:116`. |
| `P.chamado_destino` | Nascimento `M:3014`; reuso `M:2982`/`M:3006`; reaponte `F:925`/`F:927`; FK `M:3387`. | Apresentação `F:117`; reuso `M:2968`; simulação do juiz `F:778`. |
| `DS.aberta_em` | ORM nos três nascimentos; definição `M:1756`. | Ordenação da eleição `M:1804`; ordenação de cores `M:1819`. |
| `DS.fechada_em` | Nascimento `M:1757`; fechar `M:1932`; reabrir `M:2149`, `M:2173`. | Aberta `M:1824`; seletores `E:532`, `E:1097`, `E:1333`; signal `app/chamados/signals.py:28`. |
| `DS.resolucao` | Fechar `M:1934`; reabrir `M:2151`, `M:2175`; limpeza `M:2265`. | Idemp

[truncado em 20000 de 21940 caracteres]

## 11/11 20:33 · sessao cb17498e4229 · CASO-0711-HJ003 · censo · origem=Code

O “Decidir” **não garante autorização para decidir**: o verbo é calculado sem consultar o usuário; a autorização vem depois, pelo departamento do chamado e, em algumas portas, pela permissão da ação. Não encontrei veto de permissão diferente entre os 16 motivos de pergunta. (`app/chamados/catalogo/natureza.py:41`; `app/chamados/catalogo/natureza.py:58`; `app/chamados/services/fio.py:566`)

Censo estático, sem cura. Correspondência desta cópia ao HEAD de `main`, permissões efetivas dos usuários e execução em produção: **[não verificado]**. A cópia disponibilizada não permite confirmar o hash por Git. **[não verificado]**

Para reduzir repetição nas tabelas, os caminhos abaixo são expandidos assim: `C/` = `app/chamados/`; `T/` = `app/templates/chamados/`; `U/` = `app/colaboradores/`.

**Perfis considerados e significado de S/N.**

- **Supervisor:** perfil administrativo `supervisao` criado pela UI, com `is_staff=True`, grupo `gestor` e setores de supervisão; permissões conforme a semeadura padrão. Existência dessa configuração nos usuários de produção: **[não verificado]**. (`app/core/views_usuarios.py:48`; `app/core/views_usuarios.py:52`; `app/core/views_usuarios.py:295`; `app/core/views_usuarios.py:299`; `U/mixins.py:355`)
- **Admin:** `admin_ti` criado pela UI como superusuário. (`app/core/views_usuarios.py:58`; `app/core/views_usuarios.py:296`)
- **Staff₀:** cenário explícito de usuário autenticado, somente `is_staff=True`, sem superusuário, competências ou permissões adicionais. Não é um perfil cadastrado comprovado pelo anexo. **[não verificado]**
- **S:** a porta da ação indicada aceita o perfil, satisfeitas as condições de estado/dado. **N:** falta botão autorizado ou a porta recusa a ação. Aceitar o perfil não garante materialização ou fechamento: essas operações ainda têm guardas de negócio. (`C/services/fio.py:566`; `C/services/fio.py:660`; `C/models.py:1848`)

`is_staff` permite entrar na fila, mas **não é bypass de `tem_acao`**. Na autorização contextual, usuário sem competências declaradas passa pelo ramo legado; usuário com competências precisa pertencer ao departamento, salvo gestor geral. (`C/views.py:35`; `U/mixins.py:322`; `C/utils.py:47`; `C/utils.py:60`; `C/utils.py:65`)

**Origem do rótulo.**

| Informação | Quem produz | Quem lê |
|---|---|---|
| Verbo `decidir` | `C/catalogo/natureza.py:58`; partição em `C/services/verbos.py:47` | Painel: `C/services/painel.py:383`; lista: `T/partials/_lista_chamados.html:86` |
| Texto “Decidir” da linha | `C/services/verbos.py:149` | `T/partials/_lista_resumo.html:71`; `T/partials/_lista_chamados.html:86` |
| “DECIDIR / Use os botões” do fio | `C/services/desenlace.py:78`; contexto em `C/services/fio.py:403` | `T/partials/modal_fio.html:117` |
| Autorização dos botões | `C/utils.py:68`; mapa em `C/views.py:933` | `T/partials/modal_fio.html:123`; `T/partials/modal_fio.html:652` |
| Tags de apresentação | `C/templatetags/chamados_tags.py:27` delega autorização; `:68` delega opções de movimento | Os templates acima; as tags não classificam o chamado como `decidir`. (`C/templatetags/chamados_tags.py:30`; `C/templatetags/chamados_tags.py:75`) |

A lista mostra o verbo e abre o fio; não contém um POST “Decidir” próprio. No modal, o verbo aparece antes da guarda contextual dos botões. (`T/partials/_lista_chamados.html:42`; `T/partials/_lista_chamados.html:86`; `T/partials/modal_fio.html:117`; `T/partials/modal_fio.html:123`)

**Portas usadas pelas matrizes.** Cada código identifica o botão, a URL, a view e a guarda; portanto, as linhas seguintes reutilizam a mesma cadeia comprovada.

| Código / ação | Template:linha do botão | URL → view:linha | Guarda:linha |
|---|---|---|---|
| **R — responder/mover chamado** | `T/partials/modal_fio.html:672`, seletor `:665` | `/chamados/modal/<colab>/` (`C/urls.py:39`) → `C/views.py:869` → `C/services/fio.py:708` | Entrada `C/views.py:872`; render `T/partials/modal_fio.html:652`; POST `C/services/fio.py:714`; transição `:721` |
| **Q — validar/rejeitar pergunta** | `T/partials/modal_fio.html:284`, `:298`; confirmação de horário `:269` | Mesma URL/view de R → `C/services/fio.py:585`, `:625` | `C/services/fio.py:566`; conferência da pergunta listada `:598`, `:635` |
| **H — corrigir resposta de hora** | `T/partials/modal_fio.html:312` | Mesma URL/view de R → `C/services/fio.py:625` | `C/services/fio.py:566`; condição de tipo no template `T/partials/modal_fio.html:300` |
| **F — regularizar/fechar disputa** | `T/partials/modal_fio.html:359` | Mesma URL/view de R → `C/services/fio.py:646` | Contextual `:566`; resolução `:648`; pergunta sem resposta `:660`; juiz final `C/models.py:1848` |
| **L — aceitar folga sugerida** | `T/partials/modal_fio.html:551` | `/chamados/local/<id>/` (`C/urls.py:14`) → `C/views.py:359`, despacho `:431` | Contextual `C/views.py:420`; condições da sugestão `C/services/limbo_folgas.py:84` |
| **G — confirmar/realocar régua** | `T/partials/modal_fio.html:567`, `:576` | URL local → `C/views.py:438`, `:449` | Contextual `C/views.py:420`; realocação consulta vínculo `app/escala/regua_defesa.py:316` |
| **E — aprovar exceção de horário** | `T/partials/modal_fio.html:599` | URL local → `C/views.py:426` | Contextual `:420`; `_pode_aprovar` aceita staff ou grupo gestor/admin `C/views.py:41` |
| **O — perguntar sobre furos sem pergunta** | `T/partials/modal_fio.html:629` | URL modal → `C/services/fio.py:549` → `C/services/acoes_disputa.py:210` | Entrada gestor `C/views.py:872`; render `T/partials/modal_fio.html:617` |
| **I — pedir confirmação da âncora legada** | `T/partials/modal_fio.html:645` | URL modal → `C/services/fio.py:521` → `C/services/acoes_disputa.py:197` | Contextual `C/services/acoes_disputa.py:187` |
| **P — aplicar/recusar proposta de escala** | `T/partials/modal_fio.html:54`, `:64` | URL modal → `C/services/fio.py:509` | `editar_escalas`: `C/services/proposta_no_fio.py:136`, `:168`; chave declarada em `app/core/intake.py:53` |
| **A — decidir ausência na tela própria** | Link `T/detalhe_local.html:233`; botões `app/templates/ponto/partials/painel_ausencia.html:319`, `:332` | `/chamados/local/<id>/ir-para-ausencia/` → `C/views.py:1494`; depois `/ponto/ausencias/<aid>/aprovar/` ou `rejeitar/` (`app/ponto/urls.py:65`, `:74`) | Link exige `ver_ausencias`, `C/views.py:1509`; POST exige `editar_ausencias`, `app/ponto/views.py:2603`, `:2684` |

Há também a apresentação genérica R no fio completo e no detalhe local: botões em `T/fio_colaborador.html:272` e `T/detalhe_local.html:403`; views em `C/views.py:659` e `:359`; guardas em `C/services/fio.py:714` e `C/views.py:420`. (`C/urls.py:21`; `C/urls.py:14`)

**Matriz por motivo da pergunta.** A autorização é a do chamado que lista a pergunta; o motivo sozinho não determina o verbo nem o departamento. A matriz abaixo considera um chamado de supervisão. (`C/services/fio.py:563`; `C/services/fio.py:566`; `C/services/fio.py:598`; `C/catalogo/natureza.py:41`)

| Motivo — fonte | Botão/porta | Supervisor | Admin | Staff₀ |
|---|---|:---:|:---:|:---:|
| `orfao_14h` — `C/catalogo/perguntas.py:48` | Q/H | S | S | S |
| `saida_sem_entrada` — `C/catalogo/perguntas.py:56` | Q/H | S | S | S |
| `intervalo_saida` — `C/catalogo/perguntas.py:124` | Q/H | S | S | S |
| `intervalo_volta` — `C/catalogo/perguntas.py:132` | Q/H | S | S | S |
| `ancora_ausente` — `C/catalogo/perguntas.py:118` | Q | S | S | S |
| `colisao_escala` — `C/catalogo/perguntas.py:70` | Q | S | S | S |
| `sem_posto` — `C/catalogo/perguntas.py:36` | Q | S | S | S |
| `posto_desativado` — `C/catalogo/perguntas.py:64` | Q | S | S | S |
| `ordem_substituicao_invalida` — `C/catalogo/perguntas.py:76` | Q | S | S | S |
| `em_ferias` — `C/catalogo/perguntas.py:42` | Q | S | S | S |
| `batida_fora_escala_grave` — `C/catalogo/perguntas.py:82` | Q | S | S | S |
| `gps_ausente` — `C/catalogo/perguntas.py:88` | Q | S | S | S |
| `foto_ausente` — `C/catalogo/perguntas.py:94` | Q | S | S | S |
| `gps_mock` — `C/catalogo/perguntas.py:100` | Q | S | S | S |
| `device_comprometido` — `C/catalogo/perguntas.py:106` | Q | S | S | S |
| `liveness_indisponivel` — `C/catalogo/perguntas.py:112` | Q | S | S | S |

A permissão `materializar_saida` **não é exigida por Q/H/F**: a validação entra em `C/services/validacao.py:373` e chama a materialização em `:461`. Essa permissão pertence à outra porta, de saída retroativa de justificativa. (`app/ponto/views.py:3118`)

Isso não significa que todos os motivos materializem com sucesso. `posto_dropdown` e `escala_dropdown` não têm ramo no despacho mostrado, que trata `hora`, `data_hora` e `escolha`; a ausência de efeito cai na recusa. Essa diferença atinge os perfis igualmente, não é veto ao supervisor. (`C/services/materializacao.py:1226`; `C/services/materializacao.py:1239`)

**Matriz por tipo do chamado na fila.** Nas linhas R, o resultado é para responder/mover o chamado. O departamento vem do catálogo: `colab/cadastro/supervisao → supervisao`, `dp → dp`, `sistema → ti`. (`C/utils.py:27`; `C/utils.py:41`; guardas da porta R acima)

| Tipo — declaração no catálogo | Porta | Supervisor | Admin | Staff₀ |
|---|---|:---:|:---:|:---:|
| `batida_ausente` — `C/catalogo/modulos.py:105` | R; O quando aplicável | S | S | S |
| `turno_aberto` — `C/catalogo/modulos.py:107` | R; O quando aplicável | S | S | S |
| `turno_aberto_24h` — `C/catalogo/modulos.py:109` | R; O quando aplicável | S | S | S |
| `intervalo_pendente` — `C/catalogo/modulos.py:111` | R; O quando aplicável | S | S | S |
| `par_relampago` — `C/catalogo/modulos.py:113` | R | S | S | S |
| `cluster_batida_espuria` — `C/catalogo/modulos.py:115` | R | S | S | S |
| `justificativa` — `C/catalogo/modulos.py:117` | R | S | S | S |
| `ausencia_colab` — `C/catalogo/modulos.py:120` | R / alternativa A | **N / S** | S / S | S / **N** |
| `colaborador_em_ferias` — `C/catalogo/modulos.py:123` | R | **N** | S | S |
| `sem_previsao_escala` — `C/catalogo/modulos.py:126` | R; L quando aplicável | S | S | S |
| `ancora_limbo` — `C/catalogo/modulos.py:131` | R; I quando aplicável | S | S | S |
| `regua_degenerada` — `C/catalogo/modulos.py:133` | R | S | S | S |
| `regua_suspeita` — `C/catalogo/modulos.py:135` | R; G quando aplicável | S | S | S |
| `escala_colaborador` — `C/catalogo/modulos.py:137` | R | S | S | S |
| `colaborador_sem_posto` — `C/catalogo/modulos.py:139` | R | S | S | S |
| `desligamento_revisao` — `C/catalogo/modulos.py:141` | R | **N** | S | S |
| `disputa_supervisao` — `C/catalogo/modulos.py:144` | R; Q/H/F conforme estado | S | S | S |
| `disputa_supervisao_manual` — `C/catalogo/modulos.py:146` | R; Q/H/F conforme estado | S | S | S |
| `chamados` — `C/catalogo/modulos.py:149` | R, conforme área¹ | S/N | S | S |
| `regularizacao_externa` — `C/catalogo/modulos.py:151` | R | S | S | S |
| `acesso_bloqueado` — `C/catalogo/modulos.py:154` | R | **N** | S | S |
| `agenda_dia` — `C/catalogo/modulos.py:156` | R | S | S | S |
| `gps_fora_geofence` — `C/catalogo/modulos.py:160` | **Validar, não Decidir**² | — | — | — |
| `gps_ausente` — `C/catalogo/modulos.py:162` | **Registro**² | — | — | — |
| `geofence` — `C/catalogo/modulos.py:166` | R | S | S | S |
| `batida_fora_escala_leve` — `C/catalogo/modulos.py:168` | **Registro**² | — | — | — |
| `entrada_bloqueada_turno_aberto` — `C/catalogo/modulos.py:182` | R | S | S | S |
| `batida_fora_horario` — `C/catalogo/modulos.py:184` | R; E quando aplicável | S | S | S |
| `ping_geo_fora` — `C/catalogo/modulos.py:186` | R | S | S | S |
| `app_colaborador` — `C/catalogo/modulos.py:188` | R, conforme área¹ | S/N | S | S |
| `justificativa_colab` — `C/catalogo/modulos.py:190` | R | S | S | S |
| `mensagem_dp` — `C/catalogo/modulos.py:192` | R | **N** | S | S |
| `beneficio` — `C/catalogo/modulos.py:195` | R³ | **N** | S | S |
| `smoke` — `C/catalogo/modulos.py:198` | R | **N** | S | S |

¹ Para `chamados` e `app_colaborador`, uma área válida da solicitação prevalece sobre o dono do módulo. Supervisor: **S** para supervisão; **N** para outro departamento, sem competência acumulada. (`C/rotulos.py:26`; `C/utils.py:39`; `C/utils.py:65`)

² A distinção é pelo **módulo**, não pelo motivo homônimo de pergunta. `gps_fora_geofence` recebe `validar`; `gps_ausente`, `batida_fora_escala_leve`, `plataforma_mudanca` e `desligamento_revisao` são módulos de auditoria. Portanto, `desligamento_revisao` também fica fora do `Decidir` produzido pela partição normal, apesar da autorização R indicada na tabela. (`C/catalogo/natureza.py:18`; `C/catalogo/natureza.py:29`; `C/catalogo/natureza.py:44`)

³ R existe no modal normal; o modo benefício suprime essa caixa, inclusive para admin. (`T/partials/modal_fio.html:651`)

**Condições adicionais e botões que não executam.**

| Causa/ação específica | Botão → porta/guarda | Supervisor | Admin | Staff₀ |
|---|---|:---:|:---:|:---:|
| Proposta de escala | P; exige `editar_escalas`, sem guarda correspondente no template. (`T/partials/modal_fio.html:40`; `C/services/proposta_no_fio.py:136`) | S | S | **N** |
| Datas divergentes: conferir | `T/partials/modal_fio.html:177` → `/chamados/disputa/pergunta/<id>/conferir-data/`, `C/urls.py:18` → `C/views.py:1292`; `gerir_chamados` em `:1310` | S | S | **N** |
| Reabrir questionário | `T/partials/modal_fio.html:372` → URL `C/urls.py:20`, view `C/views.py:1372`; a recusa exige faltar **tanto** `gerir_chamados` **quanto** autorização contextual. (`C/views.py:1404`) | S | S | S |
| Reabertura administrativa | `T/partials/modal_fio.html:412` → modal → `C/services/acoes_disputa.py:235`; aceita `is_staff`, apesar da mensagem citar admin. (`:245`) | S | S | S |
| Fechar disputa com pergunta sem resposta | F não é oferecido; aparece cobrança. (`T/partials/modal_fio.html:334`; `:349`; guarda POST `C/services/fio.py:660`) | N | N | N |
| Fechamento manual com `ancora_ausente` | Aviso no lugar de F. (`T/partials/modal_fio.html:327`) | N | N | N |
| Aplicar declaração extraída do texto, **no modal** | `T/partials/modal_fio.html:143`, ação `:140` → modal; sem despacho em `processar_post_fio`, termina em `C/services/fio.py:742` | **N** | **N** | **N** |
| Aplicar correção de pergunta cega | `T/partials/modal_fio.html:193`, ação `:190` → mesmo término `C/services/fio.py:742` | **N** | **N** | **N** |
| Apagar pergunta cega | `T/partials/modal_fio.html:200`, ação `:197` → mesmo término `C/services/fio.py:742` | **N** | **N** | **N** |

As três últimas falhas são de despacho, não de permissão. Aplicar declaração tem despacho na **outra** porta, o detalhe local. (`C/views.py:478`)

As recusas contextuais examinadas não precisam retornar HTTP 403: o modal renderiza a mensagem de erro; outras portas usam o envelope de resposta/redirect. A entrada sem gestor retorna 404. (`C/services/fio.py:566`; `C/services/fio.py:494`; `app/core/respostas.py:63`; `app/core/respostas.py:71`; `C/views.py:872`)

**Causas com “Decidir” sem botão executável pelo supervisor no fio.**

- `colaborador_em_ferias`, `mensagem_dp`, `beneficio` são do DP; `acesso_bloqueado` e `smoke`, do TI. O verbo não filtra o perfil, mas a caixa R é suprimida e o POST recusa pelo departamento. (`C/catalogo/modulos.py:123`; `:192`; `:195`; `:154`; `:198`; `C/catalogo/natureza.py:41`; `T/partials/modal_fio.html:652`; `C/services/fio.py:714`)
- Solicitações `chamados`/`app_colaborador` de departamento diferente de supervisão têm o mesmo resultado, sem competência adicional. (`C/utils.py:39`; `C/utils.py:65`)
- `ausencia_colab` tem essa divergência **no fio**, mas possui alternativa executável no detalhe → tela de ausências para o supervisor padrão; portanto, não é uma ausência absoluta de porta. (`C/catalogo/modulos.py:120`; `T/detalhe_local.html:233`; `C/views.py:1509`; `app/ponto/views.py:2603`; `U/mixins.py:361`)
- Proposta de escala perde execução se `editar_escalas` tiver sido revogada; conferência de data perde execução se faltar `gerir_chamados`. Grants/revogações reais: **[não verificado]**. (`C/services/proposta_no_fio.py:136`; `C/views.py:1310`; `app/core/views_quadro.py:43`; `:45`)

Nenhum desses tipos foi comprovado como integrante atual da fila de produção pelo dado raspado; também não foi demonstrado que alguma das perguntas da disputa 7279 sofreu recusa por permissão. **[não verificado]**

**Quem escreve e quem lê os campos/permissões usados neste censo.** A tabela identifica escritores dos caminhos rastreados; não afirma exclusividade onde ela não foi demonstrada.

| Campo/permissão | Quem escreve | Quem lê |
|---|---|---|
| `Group.permissions`, `rbac_*` | Semeadura `U/mixins.py:397`, acionada por `U/signals.py:30`; concessão/revogação na UI `app/core/views_quadro.py:43`, `:45` | `tem_acao`: `U/mixins.py:330`; consulta `get_all_permissions` em `:331` |
| `editar_escalas`, `gerir_chamados`, `ver_ausencias`, `editar_ausencias` padrão | Lista de supervisão `U/mixins.py:355`; aplicação em `:397` | Portas P/A/conferência citadas acima |
| `materializar_saida` | Padrões DP/TI `U/mixins.py:364`, `:376`; mesma UI de permissões acima | Porta de justificativa `app/ponto/views.py:3118` |
| `User.groups` | Criação `app/core/views_usuarios.py:299`; alterações `:204`, `:206` | Gestor `C/views.py:38`; competências `U/mixins.py:139` |
| `User.is_staff` | `app/core/views_usuarios.py:295`; sincronização de perfil `app/core/credenciais.py:139` | `C/views.py:36`, `:43`; reabertura administrativa `C/services/acoes_disputa.py:245` |
| `User.is_superuser` | `app/core/views_usuarios.py:296`; alternância `:82` | `U/mixins.py:163`, `:328`; gestor geral `C/utils.py:49` |
| `Setor.competencia`, `ativo` | Criação `U/services/estrutura.py:97`; ativação `:109` | Competências do usuário `U/mixins.py:140`; semeadura `U/signals.py:30` |
| `Colaborador.perfil` | `app/core/credenciais.py:135` | `get_perfil` em `U/mixins.py:171`; transição do chamado `C/models.py:131` |
| `modulo_origem`, `area`, `contexto_json` do chamado | Campos recebidos na abertura/criação: `C/models.py:961`, `:971`, `:1070` | Departamento `C/utils.py:37`, `:41`; verbo `C/services/verbos.py:44`; condições dos botões `T/partials/modal_fio.html:544`, `:588` |
| `dono` do módulo | Catálogo `C/catalogo/modulos.py:103`; estrutura `_m` em `:72` | Resolução de departamento `C/utils.py:41` |
| `status_local` | Núcleo de estado `C/models.py:228`; criação `:1070` | Transição `C/models.py:124`; botões `T/partials/modal_fio.html:651` |
| `motivo`, `tipo_resposta`, opções da pergunta | Catálogo `C/catalogo/perguntas.py:34`; abertura pelo emissor `C/services/disputa_emissao.py:439` | Template `T/partials/modal_fio.html:300`; materialização `C/services/materializacao.py:1226` |
| `resposta_colab`, `respondida_em` | `C/services/disputa_emissao.py:1467` | Validação `C/services/validacao.py:393`; template `T/partials/modal_fio.html:217` |
| `validada_em`, `validada_por` | Carimbo `C/services/validacao.py:134`; resposta nova limpa em `C/services/disputa_emissao.py:1480` | Validação `C/services/validacao.py:396`; template `T/partials/modal_fio.html:228` |
| `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento` | Porta de fechamento `C/models.py:1932` | Guarda `C/models.py:1879`; roteamento `C/services/fio.py:600` |
| `todas_respondidas` | Contexto `C/services/fio.py:340` | Escolha do botão F `T/partials/modal_fio.html:334` |
| `pode_agir_map`, `is_admin_ti`, `pode_conferir_data` | `C/views.py:933`, `:936`; contexto parcial `C/services/fio.py:474` | `T/partials/modal_fio.html:123`, `:382`, `:172` |

**Scripts AST/grep usados, executados por stdin, sem importar o aplicativo.** O primeiro enumera os catálogos; o segundo verifica chamadas RBAC nos serviços. (`C/catalogo/motivos.py:33`; `C/catalogo/modulos.py:103`; `C/services/validacao.py:373`; `C/services/materializacao.py:1226`)

```bash
python3 -B - <<'PY'
import ast
from pathlib import Path
for name in ('motivos', 'modulos'):
    p = Path('app/chamados/catalogo') / (name + '.py')
    t = ast.parse(p.read_text())
    print(p)
    for n in ast.walk(t):
        if isinstance(n, ast.Assign) and any(
            isinst

[truncado em 20000 de 22013 caracteres]

## 11/11 20:33 · sessao 1500ae33b4b3 · CASO-0711-HJ003 · sonda · origem=Code

**O par está fechado na saída TURNO fornecida, mas o dado não identifica quem emitiu a pk52340 nem demonstra que a saída já existia quando ela nasceu.** A sonda também informa um processamento da pergunta **29min45s antes** da criação da saída. Portanto, atribuir este caso a um corte por dia civil seria antecipar uma conclusão. [nao verificado]

**1) O sítio que emitiu a pk52340 permanece [nao verificado].**

Os discriminantes fornecidos são: `motivo=orfao_14h`, `data_evento=2026-10-31`, `celula_id=133672`, `batida_origem_id=None`, `instancia_marco=fim`, `conversa=False`, para col407. [nao verificado]

Esses campos não constituem uma assinatura exclusiva do emissor:

| Campo observado | O que o código permite concluir |
|---|---|
| `motivo=orfao_14h` | Pode significar **marco final S faltante**: `marco_para_motivo` apenas traduz o casamento de tipo/hora com o fim; não verifica par nem mede 14h. `app/ponto/marcos.py:33`, `app/ponto/marcos.py:34`, `app/ponto/marcos.py:49`, `app/ponto/marcos.py:50`. |
| `data_evento` e `celula_id` | A porta comum determina o dia do fato, busca sua célula e grava ambos; isso serve a diferentes chamadores. `app/chamados/models.py:2875`, `app/chamados/models.py:2896`, `app/chamados/models.py:3014`, `app/chamados/models.py:3016`. |
| `instancia_marco=fim` | Deriva automaticamente do motivo pela tabela posição/tipo; não identifica a rota. `app/chamados/services/declaracao_texto.py:44`, `app/chamados/services/declaracao_texto.py:55`, `app/chamados/models.py:3438`, `app/chamados/models.py:3440`. |
| `batida_origem_id=None` | É esperado para a família MARCO: o derivador automático de episódio só atende à família BATIDA. Logo, NULL não prova que o emissor deixou de consultar batidas. `app/chamados/models.py:3448`, `app/chamados/models.py:3461`. |
| `conversa=False` | Identifica nascimento pela porta do fato; não distingue os chamadores dessa porta. `app/chamados/models.py:2789`, `app/chamados/models.py:3014`. |

As cadeias aplicáveis na cópia são:

| Entrada | Cadeia verificada |
|---|---|
| **Cron do cartório** | `06:27 --apply`, por empresa → `processar_cartorio` → `julgar_colab` → `perguntar_pela_celula` → fábrica → disputa/pergunta. `app/config/crons.py:616`; `app/ponto/management/commands/processar_cartorio.py:65`; `app/ponto/services/cartorio.py:1032`; `app/chamados/services/fabrica_da_celula.py:172`. |
| **Signal de batida** | `post_save(Batida)` → julgamento da data local e da véspera → cartório → mesma fábrica. `app/ponto/signals.py:108`, `app/ponto/signals.py:131`, `app/ponto/signals.py:135`, `app/ponto/signals.py:93`; `app/ponto/services/cartorio.py:1032`. |
| **Detector contínuo** | Cron `*/5` → `detectar_ausencias` → chamado → repasse para disputa. A detecção corrente procura **entrada**; o cartório também reutiliza sua porta para cobrar marcos nomeados. `app/config/crons.py:364`; `app/ponto/management/commands/detectar_ausencias.py:335`, `app/ponto/management/commands/detectar_ausencias.py:693`, `app/ponto/management/commands/detectar_ausencias.py:802`; `app/ponto/services/cartorio.py:1815`. |
| **Turno aberto retroativo** | `turnos_do_colab` → exige aberto → exige idade ≥24h → abre chamado `turno_aberto_24h`. Não reconstrói o par. `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:92`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:100`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151`. |
| **API/anomalias** | Detector de órfão → disputa → geração de perguntas. O detector usa `batidas_apuraveis` e `parear_turnos`. `app/api/views.py:434`, `app/api/views.py:764`; `app/chamados/detector_anomalias.py:32`; `app/ponto/utils_turno_orfao.py:28`, `app/ponto/utils_turno_orfao.py:31`. |

A escrita converge em `PerguntaDisputa.abrir`, chamada pelo gerador ou pelo repasse, e na inserção de `_nascer`. O código registra um evento `pergunta_emitida`, mas esse evento não está no dado fornecido. `app/chamados/services/disputa_emissao.py:444`; `app/chamados/services/disputa_emissao.py:837`; `app/chamados/models.py:3014`; `app/chamados/models.py:3025`. A associação desse nascimento à pk52340 é **[nao verificado]**.

**2) Não está demonstrado que algum emissor tenha ignorado um par já disponível. [nao verificado]**

A saída fornecida é:

```text
TURNO (2026-10-31, 142873, 143189, (...), False, True)
```

Os últimos campos significam `aberto=False` e `cross_meianoite=True`; sua ordem e construção estão em `app/ponto/turnos.py:43`, `app/ponto/turnos.py:321`, `app/ponto/turnos.py:322`. As pks e o resultado concreto são da sonda. [nao verificado]

O pareador ordena por `timestamp`, abre pela entrada e fecha com a saída posterior; não separa esse par na virada civil. `app/ponto/turnos.py:1217`, `app/ponto/turnos.py:1419`, `app/ponto/turnos.py:1491`.

A localização exata das decisões na cópia é:

| Hipótese | O que foi encontrado |
|---|---|
| **Janela de um dia civil** | `turnos_do_colab(D,D)` amplia a carga para **D−2 até D+1**, chama o pareador e depois seleciona os turnos pelo dia. Não equivale a parear somente batidas civis de D. `app/ponto/turnos.py:1847`, `app/ponto/turnos.py:1863`, `app/ponto/turnos.py:1992`, `app/ponto/turnos.py:1995`. |
| **Filtro por tipo no universo** | `batidas_apuraveis` filtra retratação nula, colaborador e limites opcionais; não elimina E ou S. `app/ponto/turnos.py:2876`, `app/ponto/turnos.py:2877`, `app/ponto/turnos.py:2881`. |
| **Uso literal de `batidas_apuraveis`** | O detector de órfão a chama antes de `parear_turnos`. Já `turnos_do_colab` monta sua própria consulta com retratação nula e janela ampliada, chegando ao mesmo pareador; não chama literalmente `batidas_apuraveis`. `app/ponto/utils_turno_orfao.py:28`, `app/ponto/utils_turno_orfao.py:31`; `app/ponto/turnos.py:1863`, `app/ponto/turnos.py:1992`. |
| **Derivação por marco** | A fábrica lê faltantes da ata, verifica vencimento e traduz `(fim,S)` em `orfao_14h`. Esse caminho cobra uma lâmpada apagada, sem realizar um novo pareamento E→S. `app/chamados/services/fabrica_da_celula.py:64`, `app/chamados/services/fabrica_da_celula.py:69`, `app/chamados/services/fabrica_da_celula.py:71`; `app/chamados/services/declaracao_texto.py:44`. |
| **Relógio** | A fábrica pergunta ao cartório pelo vencimento do marco; o cartório encadeia os instantes e aplica marco +30min. `app/chamados/services/fabrica_da_celula.py:46`; `app/ponto/services/cartorio.py:879`, `app/ponto/services/cartorio.py:882`. |
| **Ordem de chegada** | A geometria ordena por `timestamp`; isso não prova quais linhas estavam disponíveis na emissão histórica. `app/ponto/turnos.py:1217`. Disponibilidade naquele instante: **[nao verificado]**. |

Há, contudo, **classificação própria residual** em `classificar_falta`: depois de obter os turnos, ela calcula presença de E/S, contagem por tipo e conserva um retorno final `orfao_14h`. `app/escala/servico_jornada.py:403`, `app/escala/servico_jornada.py:459`, `app/escala/servico_jornada.py:480`, `app/escala/servico_jornada.py:492`.

Precisamente: a saída órfã é derivada por `tem_e and not tem_s` em `app/escala/servico_jornada.py:467`; por turno aberto em `app/escala/servico_jornada.py:478`; por contagem insuficiente de S em `app/escala/servico_jornada.py:485`; e pelo retorno terminal em `app/escala/servico_jornada.py:492`. **Esse último pode devolver órfão com E+S presentes quando não há sequência esperada**, pois o retorno de dia completo está dentro de `if esperada`. `app/escala/servico_jornada.py:469`, `app/escala/servico_jornada.py:489`, `app/escala/servico_jornada.py:492`.

Isso comprova lógica residual na classe de código, mas **não comprova que esse ramo emitiu a pk52340**. A sonda não traz a alimentação histórica desse julgamento. [nao verificado]

**3) A cronologia impede tratar “par existente na sonda” como “par existente na emissão”. [nao verificado]**

| Registro | Instante fornecido |
|---|---|
| Batida pk142873, `criado_em` | 31/10/2026 19:00:40. [nao verificado] |
| Pergunta pk52340, `materializada_em` | 01/11/2026 06:30:48. [nao verificado] |
| Batida pk143189, `criado_em` | 01/11/2026 07:00:33. [nao verificado] |
| Pergunta pk52340, instante de criação | Não fornecido. **[nao verificado]** |

`materializada_em` é carimbo de **processamento**, escrito separadamente do nascimento; não pode ser renomeado como criação. `app/chamados/models.py:3393`, `app/chamados/models.py:3099`, `app/chamados/models.py:3017`.

O carimbo fornecido antecede a criação da saída em **29min45s**. Sob o fluxo normal, isso é compatível com uma pergunta já existente antes da saída; o dado não permite reconstruir sua emissão nem explicar por que recebeu aquele processamento. A comparação exata **criação da pergunta × criação das batidas** permanece **[nao verificado]**.

**4) Classe, censo e RED.**

A classe investigada é **violação do juiz único da geometria do turno: emissão de órfão por derivação concorrente de presença, contagem ou recorte de batidas**. O resíduo concreto está em `classificar_falta`; sua atribuição à pk52340 continua **[nao verificado]**. `app/escala/servico_jornada.py:459`, `app/escala/servico_jornada.py:480`, `app/escala/servico_jornada.py:492`.

O censo AST percorreu 758 arquivos, 4.960 funções e produziu 78 candidatas, sem erro de parse; esses números vêm da execução em memória, sem arquivo de resultado. [nao verificado]

| Emissor/alimentador revisado | Pareamento ou derivação encontrada |
|---|---|
| `detectar_anomalias` | Delega ao detector que chama universo apurável + pareador. `app/chamados/detector_anomalias.py:32`; `app/ponto/utils_turno_orfao.py:28`, `app/ponto/utils_turno_orfao.py:31`. |
| API em `views` e `views_core` | Consulta `turno_aberto_de` e motivo da lei. `app/api/views.py:615`, `app/api/views.py:623`; `app/api/views_core.py:606`, `app/api/views_core.py:614`. |
| Triagem PWA | Consulta `turno_aberto_de` e motivo da lei antes da emissão. `app/ponto/services/triagem_batida.py:647`, `app/ponto/services/triagem_batida.py:649`, `app/ponto/services/triagem_batida.py:736`. |
| `detectar_ausencias` | Consulta `turnos_do_colab`; seu caminho corrente procura entrada. `app/ponto/management/commands/detectar_ausencias.py:335`, `app/ponto/management/commands/detectar_ausencias.py:492`. |
| `emitir_turno_aberto_retroativo` | Consulta `turnos_do_colab` e exige aberto. `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:92`. |
| `processar_alertas_turno` | Recalcula núcleo e consome materialização. `app/ponto/management/commands/processar_alertas_turno.py:67`, `app/ponto/management/commands/processar_alertas_turno.py:324`. |
| `processar_alertas_avancados` | Recalcula núcleo e consulta duração do turno aberto. `app/ponto/management/commands/processar_alertas_avancados.py:57`, `app/ponto/management/commands/processar_alertas_avancados.py:63`. |
| Cartório → fábrica | **Por marco:** universo apurável → grade/ata → faltantes vencidos → motivo. `app/ponto/services/cartorio.py:1158`, `app/ponto/services/cartorio.py:1161`; `app/chamados/services/fabrica_da_celula.py:64`, `app/chamados/services/fabrica_da_celula.py:71`. |
| Cartório → detector | **Por marco:** lê apagados e traduz o motivo. `app/ponto/services/cartorio.py:1915`, `app/ponto/services/cartorio.py:1919`, `app/ponto/services/cartorio.py:1815`. |
| `reconciliar_grade` | **Por marco:** alimenta grade com batidas cruas, lê `missing`, traduz; antes de emitir consulta `classificar_falta`. Não prova corte civil do par. `app/ponto/management/commands/reconciliar_grade.py:118`, `app/ponto/management/commands/reconciliar_grade.py:151`, `app/ponto/management/commands/reconciliar_grade.py:173`, `app/ponto/management/commands/reconciliar_grade.py:243`. |
| Complementação/repasse de disputas | Consome ata ou `classificar_falta`, sem outro algoritmo E→S nesses corpos. `app/chamados/services/disputa_emissao.py:861`, `app/chamados/services/disputa_emissao.py:876`, `app/chamados/services/disputa_emissao.py:1200`, `app/chamados/services/disputa_emissao.py:1237`. |
| Declaração de horário | **Por marco:** faltantes da ata → casamento de horário → motivo → porta. `app/chamados/services/declaracao_texto.py:334`, `app/chamados/services/declaracao_texto.py:341`, `app/chamados/services/declaracao_texto.py:346`, `app/chamados/services/declaracao_texto.py:376`, `app/chamados/services/declaracao_texto.py:666`. |
| **`classificar_falta`** | **Derivação residual própria**, embora o universo venha do pareador: presença, contagem e retorno terminal. `app/escala/servico_jornada.py:403`, `app/escala/servico_jornada.py:459`, `app/escala/servico_jornada.py:480`, `app/escala/servico_jornada.py:492`. |

Nas rotas revisadas, não foi comprovado outro pareamento E→S independente por dia civil. **A certificação de TODO emissor possível permanece [nao verificado]**: o script seleciona candidatas sintáticas, e a revisão dos caminhos foi manual; não resolve integralmente chamadas dinâmicas, callbacks e reexportações.

Script reproduzível, somente leitura, sem importar a aplicação ou acessar banco:

```bash
python3 -B - <<'PY'
import ast
from pathlib import Path
from collections import Counter

motivos = {
    'orfao_14h', 'saida_sem_entrada', 'turno_aberto',
    'turno_aberto_24h', 'saida_pendente', 'entrada_sem_saida',
}
helpers = {
    'marco_para_motivo', '_marcos_faltantes', 'classificar_falta',
    'motivo_da_lei', 'detectar_bloqueio_turno_orfao',
    'criar_disputa_a_partir_de_chamados_orfaos',
    'criar_ou_atualizar_disputa', 'gerar_perguntas_disputa',
    '_criar_pergunta_se_couber',
}
totais = Counter()
linhas, erros = [], []

class Corpo(ast.NodeVisitor):
    def __init__(self):
        self.nodes = []

    def visit(self, node):
        self.nodes.append(node)
        if not isinstance(node, (
            ast.FunctionDef, ast.AsyncFunctionDef, ast.ClassDef
        )):
            super().visit(node)

for arquivo in sorted(Path('app').rglob('*.py')):
    if {'tests', 'migrations'} & set(arquivo.parts):
        continue
    if arquivo.name.startswith(('test_', 'tests')):
        continue
    totais['files'] += 1
    try:
        tree = ast.parse(arquivo.read_text(), filename=str(arquivo))
    except (SyntaxError, UnicodeError) as exc:
        erros.append((str(arquivo), str(exc)))
        continue
    aliases = {
        alias.asname or alias.name: alias.name
        for node in ast.walk(tree) if isinstance(node, ast.ImportFrom)
        for alias in node.names
    }
    for func in ast.walk(tree):
        if not isinstance(func, (ast.FunctionDef, ast.AsyncFunctionDef)):
            continue
        totais['functions'] += 1
        corpo = Corpo()
        for stmt in func.body:
            corpo.visit(stmt)
        chamadas = {
            aliases.get(node.func.id, node.func.id)
            if isinstance(node.func, ast.Name) else node.func.attr
            for node in corpo.nodes if isinstance(node, ast.Call)
            and isinstance(node.func, (ast.Name, ast.Attribute))
        }
        literais = {
            node.value for node in corpo.nodes
            if isinstance(node, ast.Constant)
            and isinstance(node.value, str)
        } & motivos
        if literais or chamadas & helpers:
            totais['candidates'] += 1
            linhas.append((
                str(arquivo), func.lineno, func.name,
                ','.join(sorted(literais)),
                ','.join(sorted(chamadas & helpers)),
            ))

print(dict(totais), 'parse_errors', erros)
for linha in linhas:
    print('%s:%s %s | %s | %s' % linha)
PY
```

O **RED mínimo proposto** usa duas batidas apuráveis: E em D às 19:00 e S em D+1 às 07:00, com célula/escala noturna correspondente. Congela o relógio depois da saída e do prazo aplicável, chama o caminho real de emissão e exige: turno único fechado e **nenhuma pergunta `orfao_14h` para D**. O mutante descarta a resposta fechada do pareador e procura S somente no dia civil D: deve gerar a pergunta e fazer o RED falhar. Proposta não executada. [nao verificado]

O teste existente de travessia 22–06 verifica apenas o pareador; não substitui esse teste de emissão. `app/ponto/tests/test_turnos.py:12`.

A correção de classe proposta é fazer os emissores de **órfão geométrico** consumirem exclusivamente a decisão do pareador, eliminando decisões concorrentes por presença/contagem; cobrança de marco deve conservar sua causa explícita. Aplicação concreta à pk52340 depende da origem e do instante de nascimento que faltam no dado. [nao verificado]

Para os campos usados acima, seguem os escritores e leitores relevantes da cópia; a tabela não pretende ser o censo de todos os leitores de cada coluna.

| Campo | Quem escreve | Quem lê |
|---|---|---|
| Pergunta `motivo` | Nascimento: `app/chamados/models.py:3014`. | Identidade: `app/chamados/services/fio.py:1460`. |
| Pergunta `data_evento` | Ponto de atribuição: `app/chamados/services/fio.py:800`; nascimento chama em `app/chamados/models.py:3016`. | Identidade: `app/chamados/services/fio.py:1460`, `app/chamados/services/fio.py:1495`. |
| Pergunta `celula_id` | Nascimento/reuso: `app/chamados/models.py:3014`, `app/chamados/models.py:2966`; rechaveamento: `app/chamados/services/fio.py:1038`. | Juízo da célula: `app/chamados/juizes.py:1331`. |
| Pergunta `batida_origem_id` | Derivação no nascimento: `app/chamados/models.py:3448`; MARCO retorna NULL em `app/chamados/models.py:3461`. | Dia do episódio: `app/chamados/services/fio.py:1487`. |
| Pergunta `instancia_marco` | `app/chamados/models.py:3440`. | Identidade: `app/chamados/services/fio.py:1460`, `app/chamados/services/fio.py:1495`. |
| Pergunta `conversa` | Portas do fato/conversa: `app/chamados/models.py:2789`, `app/chamados/models.py:2801`; inserção: `app/chamados/models.py:3014`. | `app/chamados/services/fio.py:1445`. |
| Pergunta `materializada_em` | Carimbo: `app/chamados/models.py:3099`. | Estado: `app/chamados/models.py:3046`. |
| Batida `timestamp` / `tipo` | Registro: `app/ponto/registro_batida.py:157`. | Ordenação/papel: `app/ponto/turnos.py:1217`, `app/ponto/turnos.py:1382`. |
| Batida `criado_em` | Django, `auto_now_add`: `app/ponto/models.py:79`. | Guarda de eco: `app/ponto/turnos.py:355`, `app/ponto/turnos.py:362`. |
| Batida `retratada_em` | Anotação/retratação: `app/ponto/registro_batida.py:252`, `app/ponto/registro_batida.py:274`. | Universo apurável: `app/ponto/turnos.py:2876`. |
| Turno `data_turno` / `aberto` / `cross_meianoite` | Construção: `app/ponto/turnos.py:318`, `app/ponto/turnos.py:321`, `app/ponto/turnos.py:322`. | Seleção/classificação: `app/escala/servico_jornada.py:405`, `app/escala/servico_jornada.py:478`, `app/escala/servico_jornada.py:256`. |
| Ata/grade `missing` | Montagem da grade e lavra da ata: `app/ponto/services/cartorio.py:1161`, `app/ponto/services/cartorio.py:1859`. | Leitura dos faltantes: `app/chamados/services/declaracao_texto.py:131`. |

## 11/11 20:31 · sessao 15554d92a5c8 · CASO-0711-HJ003 · sonda · origem=Code

O problema é compatível com **a mesma saída identificada por dois dias diferentes**: dia do turno e dia civil da saída. O dedup distingue essas datas; o escritor único atual não corrige automaticamente uma data civil recebida como se fosse da célula. Isso está demonstrado no código; qual emissor criou cada pk histórico permanece **[nao verificado]**. (`app/chamados/models.py:2753`; `app/chamados/services/fio.py:1638`; `app/ponto/turnos.py:2939`)

1. Para os dois pares, os valores fornecidos são estes. Datas em 2026; R=`respondida_em`, V=`validada_em`, M=`materializada_em`. Como a sonda veio no texto, sem arquivo numerado, sua transcrição recebe **[nao verificado]**.

| pk | motivo | `data_evento` | célula / `celula_data` / trabalho | disputa | R | V | M / via |
|---|---|---|---|---|---|---|---|
| 47199 | `orfao_14h` | 21/10 | 133662 / 21/10 / sim | 7272 | 01/11 07:02:58 | 03/11 08:05:06 | 22/10 06:45:19 / `celula_absolveu` **[nao verificado]** |
| 47210 | `orfao_14h` | 22/10 | 133663 / 22/10 / folga | 7279 | 22/10 08:07:38 | 06/11 14:45:08 | 06/11 14:45:08 / `declaracao` **[nao verificado]** |
| 53458 | `orfao_14h` | 02/11 | 133674 / 02/11 / sim | 7279 | 05/11 07:00:59 | 05/11 13:01:42 | 05/11 13:01:42 / `fato_ocupado` **[nao verificado]** |
| 53628 | `orfao_14h` | 03/11 | 133675 / 03/11 / folga | 7279 | — | — | 04/11 06:30:51 / `celula_absolveu` **[nao verificado]** |

As três primeiras têm resposta, validação e carimbo; 53628 tem somente carimbo. **Carimbo não prova criação de batida**: as portas também carimbam encerramentos por identidade, e o juiz exige testemunha externa para reconhecer materialização efetiva. (`app/chamados/models.py:3097`; `app/chamados/models.py:3168`; `app/chamados/juizes.py:662`)

O nascimento das quatro perguntas não pode ser datado pelos carimbos acima. `PerguntaDisputa` não declara `criado_em`; o INSERT e o registro `pergunta_emitida` ficam no nascimento, mas essa trilha não foi fornecida. Instante de criação e chamada histórica responsável por cada pk: **[nao verificado]**. (`app/chamados/models.py:2406`; `app/chamados/models.py:3017`; `app/chamados/models.py:3025`)

Segundo o aval fornecido, cada par representa a mesma saída faltante **[nao verificado]**. O mecanismo descrito no código corresponde ao caso: o resolvedor antigo entregava o dia da saída esperada, D+1; o leitor interpretava esse valor como dia do turno e acrescentava novamente a cauda noturna. (`app/escala/servico_jornada.py:498`)

A distinção entre as hipóteses é:

- **Dois emissores históricos diferentes ou duas invocações do mesmo:** a sonda não identifica; **[nao verificado]**.
- **Dois significados de `data_evento`:** o código documenta explicitamente esse defeito histórico. (`app/escala/servico_jornada.py:501`)
- **Dedup que não casa D com D+1:** confirmado estruturalmente. A consulta inclui `data_evento`; trocar somente a disputa não deveria duplicar a mesma chave, mas trocar o dia cria outra chave. (`app/chamados/models.py:2753`; `app/chamados/models.py:2776`; `app/chamados/models.py:3483`)

Portanto, não atribuo 47199 à fábrica e 47210 ao detector — nem faço essa atribuição para 53458/53628 — sem trilha de emissão **[nao verificado]**. No HEAD, o nascimento factual converge em `_nascer`, que recebe motivo/disputa/célula e grava o dia pela função única. (`app/chamados/models.py:3014`; `app/chamados/models.py:3016`)

2. **O HEAD corrigiu os caminhos principais, mas não permite garantir “todos os emissores usam o juiz da batida”.** A chave de MARCO é:

`(colaborador, data_evento, motivo, instancia_marco)`

Ela exclui conversa e vias fora do fato; o índice parcial cobre datas desde 21/09/2026. `orfao_14h` pertence à família MARCO. (`app/chamados/models.py:2403`; `app/chamados/models.py:3482`; `app/chamados/catalogo/motivos.py:34`)

Os pré-dedups da emissão ainda consultam disputa/motivo/data; a porta final procura o universo do fato e escolhe a canônica entre disputas. Nenhuma dessas etapas equipara automaticamente D a D+1 para um marco sem episódio. (`app/chamados/services/disputa_emissao.py:417`; `app/chamados/services/disputa_emissao.py:1415`; `app/chamados/models.py:2905`; `app/chamados/services/fio.py:1638`)

| Caminho de pergunta de saída | Origem do dia no HEAD |
|---|---|
| Detector → geração | Batida pendente → `resolver_data_evento` → `dia_do_turno_da_batida`. (`app/chamados/detector_anomalias.py:41`; `app/escala/servico_jornada.py:508`) |
| Pendentes adicionais | Consulta o juiz por pk; não utiliza o dia civil listado no item. (`app/chamados/services/disputa_emissao.py:272`) |
| Repasse com batida de origem | Usa `resolver_data_evento`; sem resposta, não emite. (`app/chamados/services/disputa_emissao.py:1205`) |
| Fábrica da célula | Entrega o dia da célula à geração. (`app/chamados/services/fabrica_da_celula.py:172`) |
| Complemento de marcos | Usa o dia do marco faltante pela porta da célula. (`app/chamados/services/disputa_emissao.py:873`; `app/chamados/services/disputa_emissao.py:880`) |
| Declaração do colaborador | Usa o dia da célula faltante. (`app/chamados/services/declaracao_texto.py:375`) |
| Declaração por texto/admin | Usa o dia da célula faltante. (`app/chamados/services/declaracao_texto.py:665`) |

Os caminhos que **ainda aceitam uma data sem consultar o turno da batida** são:

- `gerar_perguntas_disputa`: aceita `contexto[motivo]['data']`, converte diretamente para `date` e depois a envolve em `dia_do_turno_da_celula`. Isso é adequado quando a fonte é a célula; não verifica um contexto civil legado ou incorreto. (`app/chamados/services/disputa_emissao.py:371`; `app/chamados/services/disputa_emissao.py:443`)
- Repasse **sem origem**: usa `data_evento_do_contexto`, cujo parser aceita `data_turno`, `data_batida`, `entrada_timestamp`, `pendentes` e `horario_previsto`, sem consultar o pareador. (`app/chamados/services/disputa_emissao.py:1232`; `app/chamados/data_evento.py:16`)

`chamados/data_evento.py` é esse parser; o juiz da pertença da batida está em `ponto/turnos.py`. A função `dia_do_turno_da_celula(d)` apenas declara o dia recebido, sem convertê-lo. (`app/chamados/data_evento.py:27`; `app/ponto/turnos.py:2927`; `app/ponto/turnos.py:2939`)

Assim, **com os produtores corrigidos entregando D, o mesmo fato deve convergir para uma pergunta**; reprodução executada desse cenário: **[nao verificado]**. **Se uma rota ainda entregar D+1 como contexto de marco, permanece possível uma segunda pergunta**, porque a porta preserva esse dia e o dedup o considera outra chave. (`app/chamados/services/fio.py:1630`; `app/chamados/services/fio.py:1638`; `app/chamados/models.py:2753`)

3. A batida **147824** aparece na sonda com `origem=disputa_s84_retro`, timestamp **23/10 07:00**, criada/aprovada em **06/11 14:45:08**, aprovação por pk657, `aprovada=True`, `sequencia=1`, sem retratação, canal, GPS ou timestamps de dispositivo/sincronização **[nao verificado]**. `sequencia=1` não significa entrada: `tipo` é outro campo; o TURNO fornecido apresenta essa batida como S **[nao verificado]**. (`app/ponto/models.py:23`)

O aval identifica **47210 → 147824**, e os carimbos fornecidos coincidem **[nao verificado]**. Porém a sonda não trouxe `Batida.pergunta_origem_id`; a ligação por FK permanece **[nao verificado]**. No HEAD, o materializador grava essa ligação e a migração a lê. (`app/chamados/services/materializacao.py:951`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:138`)

**Não aparece outra batida real do mesmo instante/tipo no recorte fornecido**; inexistência fora desse recorte: **[nao verificado]**. Pergunta dupla para a saída de 22/10 e plantio deslocado para 23/10 não demonstram, por si, batida duplicada **[nao verificado]**.

A cura do +1 está presente nesta cópia: o resolvedor consulta o dia do turno, e o ajuste do plantio usa a cadeia de marcos a partir desse dia. “No ar desde 07/11” é premissa do enunciado **[nao verificado]**. (`app/escala/servico_jornada.py:495`; `app/chamados/juizes.py:296`)

O registro local do apply de 09/11 lista exclusivamente **50484, 45495, 41639 e 47164**, quatro vivas e zero materializadas. Logo **47199/47210 não foram migradas naquele apply**. (`app/docs/RELATO.md:612`)

O motivo exato de exclusão exige respeitar a ordem do comando:

| Pergunta | O que o código permite concluir |
|---|---|
| Ambas | A seleção exige `tipo_resposta='hora'`, motivo admitido, data preenchida e `conversa=False`. O tipo da resposta não foi raspado: participação na seleção inicial **[nao verificado]**. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:205`) |
| 47199 | Se não houver plantada viva ligada, a via `celula_absolveu` resulta em `sem_fonte`. Existência dessa ligação: **[nao verificado]**. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:65`) |
| 47210 | Admitindo a FK para 147824 e o turno 23/10 mostrado, a saída é 23/10 e `data_evento` é 22/10: o comando faz `continue`, fora do universo, antes das demais recusas. Premissa FK: **[nao verificado]**. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:158`) |
| Materializada que passe essas guardas | `orfao_14h` divergente é recusada como `fora_do_aval` **antes** da verificação de colisão. Portanto não há prova de exclusão de 47210 “por colisão com 47199”. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:168`) |

A migração rechaveia a pergunta pela porta; ela não repara o timestamp da batida 147824. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:400`)

O mapa de escrita/leitura dos campos citados é este; ele descreve o HEAD, não prova a chamada histórica de cada pk.

| Campos | Quem escreve | Quem lê |
|---|---|---|
| Pergunta: motivo, disputa, colaborador, conversa | Nascimento e identidade: `app/chamados/models.py:3014`, `:3430` | Dedup/identidade: `app/chamados/models.py:2753` |
| `data_evento` | Atribuição única: `app/chamados/services/fio.py:800`; nascimento chama em `app/chamados/models.py:3016` | Dedup: `app/chamados/models.py:2753`; plantio: `app/chamados/juizes.py:296` |
| `celula_id` | Seleção/nascimento: `app/chamados/models.py:2896`, `:3014`; reuso `:2966`; rechaveamento `app/chamados/services/fio.py:1038` | Guarda pessoa/dia: `app/chamados/juizes.py:651` |
| Célula: data, trabalha | Geração: `app/ponto/portas/celula.py:259` | Dia e folga: `app/chamados/juizes.py:99`, `:455` |
| `instancia_marco`, `batida_origem_id` | Identidade: `app/chamados/models.py:3438`, `:3448` | Dedup: `app/chamados/models.py:2753`, `:2717`; dia: `app/chamados/services/fio.py:1633` |
| `respondida_em` | Resposta: `app/chamados/services/disputa_emissao.py:1468`; automática/trilha `:1653`, `:1674`; carimbo `app/chamados/models.py:3132` | Estado: `app/chamados/models.py:3044` |
| `validada_em` | Validação: `app/chamados/services/validacao.py:134`; carimbo `app/chamados/models.py:3135`; nova resposta limpa em `app/chamados/services/disputa_emissao.py:1480` | Estado: `app/chamados/models.py:3041` |
| `materializada_em`, autor, via | Ponto único: `app/chamados/models.py:3097` | Estado: `app/chamados/models.py:3041`; migração: `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:147` |
| Batida: timestamp, tipo, origem, aprovação/autor/data | Materializador fornece: `app/chamados/services/materializacao.py:932`; registro persiste: `app/ponto/registro_batida.py:157` | Dedup: `app/ponto/registro_batida.py:107`; auditoria: `app/chamados/services/materializacao.py:965` |
| Batida: `pergunta_origem_id` | `app/chamados/services/materializacao.py:951` | Migração: `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:138` |
| Batida: `criado_em` | ORM, `auto_now_add`: `app/ponto/models.py:79` | Vigia: `app/ponto/management/commands/vigia_plantio_overnight.py:76` |
| Batida: sequência, intervalo, canal, GPS, dispositivo/sincronização, retratação | Campos/defaults: `app/ponto/models.py:25`, `:55`, `:60`, `:67`, `:75`; INSERT: `app/ponto/registro_batida.py:157` | Leitor original desses campos na sonda: **[nao verificado]**; retratação é lida pelo dedup em `app/ponto/registro_batida.py:109` |

4. O RED cabe em [app/chamados/tests/test_plantio_noturno_mais1.py:75](/home/ronald/codex_ws/df2be6b12d7cd47a5b9ce0fda766382308c2c5b8/app/chamados/tests/test_plantio_noturno_mais1.py:75), na classe `OrfaoNoturnoTest`. A fixture já dispõe de 12x36, horário 19–07, D=21/10 e relógio congelado em 22/10; os testes atuais verificam resolvedor, plantio e contexto do detector. (`app/chamados/tests/test_plantio_noturno_mais1.py:30`; `:48`; `:75`; `:78`; `:90`)

A extensão mínima proposta é:

- **Fixture:** colNNN, célula D de trabalho, D+1 de folga, E em D às 19h e nenhuma S; executar detector/geração e fábrica/repasse para essa mesma saída.
- **Resultado exigido:** exatamente uma pergunta `orfao_14h`, `data_evento=D`, célula D, `instancia_marco=fim`, `conversa=False`; repetir os caminhos conserva a pk.
- **Mutante:** o produtor do detector volta a entregar D+1 enquanto a fábrica entrega D. A contagem deve chegar a duas e reprovar o teste. [nao verificado]

Essa é uma especificação de RED, **não criada nem executada [nao verificado]**. Ela testa a cooperação dos emissores contra a separação D/D+1 que o dedup atual permite. (`app/chamados/services/disputa_emissao.py:444`; `app/chamados/services/fabrica_da_celula.py:172`; `app/chamados/models.py:2753`)

## 11/11 20:31 · sessao 5e912c9d1eed · - · - · origem=Ronald (tmux)

Censo executado em memória: **758 arquivos, 4.960 funções, 78 candidatas, zero erros de parse**. Escopo: `app/**/*.py`, excluindo diretórios `tests`, `migrations` e arquivos começando em `test_`/`tests`. Esses números são resultado da execução do script abaixo; sem arquivo persistido: `[nao verificado]`.

O levantamento **não demonstrou outro emissor que reconstrua E→S por dia civil sem consultar o juiz**. Demonstrou rotas que dão o nome `orfao_14h` a **marco de saída apagado**, além da contagem residual em `classificar_falta`; portanto, “não chama `parear_turnos` diretamente” não equivale a “pareia por conta própria”. (`app/chamados/services/fabrica_da_celula.py:64`, `app/chamados/services/fabrica_da_celula.py:71`, `app/escala/servico_jornada.py:403`, `app/escala/servico_jornada.py:480`)

| Candidato / rota de emissão | Origem da decisão e classificação |
|---|---|
| `detector_anomalias.detectar_anomalias` → disputa | Consulta `detectar_bloqueio_turno_orfao`; este lê `batidas_apuraveis` e chama `parear_turnos`. Sem reconstrução própria do par. (`app/chamados/detector_anomalias.py:32`, `app/ponto/utils_turno_orfao.py:28`, `app/ponto/utils_turno_orfao.py:31`) |
| `api.views.api_bater_ponto` | Consulta `turno_aberto_de`, `em_intervalo` e `_motivo_da_lei`; as anomalias então recebem `orfao_14h`. (`app/api/views.py:615`, `app/api/views.py:623`, `app/api/views.py:636`, `app/api/views.py:640`) |
| `api.views_core.api_bater_ponto` | Mesma delegação da cópia acima. (`app/api/views_core.py:606`, `app/api/views_core.py:614`, `app/api/views_core.py:627`, `app/api/views_core.py:631`) |
| `triagem_batida.triagem_bater_ponto` / `ramo_registrar_es` | Triagem usa universo/pareador; ramo S pergunta `turno_aberto_de` e `motivo_da_lei`, antes de emitir `saida_sem_entrada`. (`app/ponto/services/triagem_batida.py:137`, `app/ponto/services/triagem_batida.py:648`, `app/ponto/services/triagem_batida.py:736`) |
| `detectar_ausencias.handle` → `_abrir_chamado` | Busca comparecimento por `turnos_do_colab`, sem fatia civil de batidas; `_marcos_faltantes` nomeia uma entrada prevista, não calcula E→S. (`app/ponto/management/commands/detectar_ausencias.py:335`, `app/ponto/management/commands/detectar_ausencias.py:336`, `app/ponto/management/commands/detectar_ausencias.py:492`) |
| `emitir_turno_aberto_retroativo.handle` | Lê `turnos_do_colab`, depois abre chamado pela porta. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151`) |
| `processar_alertas_turno` → `_abrir_chamado_critico` | Faz `recompute_turnos`, consome materialização e abre chamado; não reconstitui o par. (`app/ponto/management/commands/processar_alertas_turno.py:67`, `app/ponto/management/commands/processar_alertas_turno.py:324`) |
| `processar_alertas_avancados.handle` | Faz recompute e pergunta `turno_aberto_ha`; emissão N5 deriva desse resultado. (`app/ponto/management/commands/processar_alertas_avancados.py:57`, `app/ponto/management/commands/processar_alertas_avancados.py:63`, `app/ponto/management/commands/processar_alertas_avancados.py:121`) |
| **Cartório → fábrica da célula** | Cartório entrega `batidas_apuraveis` à grade. A fábrica lê faltantes da ata, aplica vencimento por instante e traduz `(fim,S)` em motivo. É **rota por marco**, não pareamento E→S. (`app/ponto/services/cartorio.py:1158`, `app/ponto/services/cartorio.py:1161`, `app/chamados/services/fabrica_da_celula.py:64`, `app/chamados/services/fabrica_da_celula.py:69`, `app/chamados/services/fabrica_da_celula.py:71`, `app/chamados/services/fabrica_da_celula.py:172`) |
| **Cartório → `_marcos_do_furo_parcial` → detector** | Lê `lastro.marcos_apagados`, traduz pelo `marco_para_motivo` e repassa ao emissor de chamado. É **rota por marco**, sem novo cálculo do par. (`app/ponto/services/cartorio.py:1915`, `app/ponto/services/cartorio.py:1919`, `app/ponto/services/cartorio.py:1815`) |
| **`reconciliar_grade.handle`** | Monta grade com `c.batidas.all()` **cru**, identifica `missing` e traduz pelo `marco_para_motivo`. Divergência de universo real a censar; isso não prova separação civil E/S. (`app/ponto/management/commands/reconciliar_grade.py:118`, `app/ponto/management/commands/reconciliar_grade.py:151`, `app/ponto/management/commands/reconciliar_grade.py:173`) |
| `disputa_emissao._complementar_marcos_faltantes` | Usa `_marcos_faltantes`, mapeia posição/tipo e pede criação. **Rota por ata**. (`app/chamados/services/disputa_emissao.py:861`, `app/chamados/services/disputa_emissao.py:876`, `app/chamados/services/disputa_emissao.py:881`) |
| `disputa_emissao.criar_disputa_a_partir_de_chamados_orfaos` | Pode consumir ata, complementar marcos ou consultar `_classificar_falta`; não possui algoritmo próprio de pareamento. (`app/chamados/services/disputa_emissao.py:1081`, `app/chamados/services/disputa_emissao.py:1175`, `app/chamados/services/disputa_emissao.py:1200`, `app/chamados/services/disputa_emissao.py:1237`) |
| `declaracao_texto.declarar_hora_colab` / `aplicar_declaracao` | Primeiro lê faltantes; motivo vem da tabela `(posição,tipo)`; a abertura recebe o resultado. `_marcos_faltantes` lê `grade_da_celula` e `missing`. **Rota por ata**. (`app/chamados/services/declaracao_texto.py:103`, `app/chamados/services/declaracao_texto.py:131`, `app/chamados/services/declaracao_texto.py:334`, `app/chamados/services/declaracao_texto.py:346`, `app/chamados/services/declaracao_texto.py:376`, `app/chamados/services/declaracao_texto.py:666`) |
| **`classificar_falta`**, alimentador dos emissores | Universo canônico via `turnos_do_colab`; conserva lógica própria `tem_e/tem_s`, contagem por tipo e retorno final de órfão. Não se pode dizer “zero lógica residual”. (`app/escala/servico_jornada.py:403`, `app/escala/servico_jornada.py:459`, `app/escala/servico_jornada.py:465`, `app/escala/servico_jornada.py:478`, `app/escala/servico_jornada.py:480`, `app/escala/servico_jornada.py:492`) |

`marco_para_motivo` é tradutor: retorna `orfao_14h` ao identificar a saída final, sem consultar batidas nem declarar geometricamente uma entrada órfã. (`app/ponto/marcos.py:14`, `app/ponto/marcos.py:34`, `app/ponto/marcos.py:50`)

Script executado, reproduzível por stdin, sem arquivo:

```bash
python3 - <<'PY'
import ast, pathlib, collections
M = {'orfao_14h', 'saida_sem_entrada', 'turno_aberto',
     'turno_aberto_24h', 'saida_pendente', 'entrada_sem_saida'}
H = {'marco_para_motivo', '_marcos_faltantes', 'classificar_falta',
     'motivo_da_lei', 'detectar_bloqueio_turno_orfao',
     'criar_disputa_a_partir_de_chamados_orfaos',
     'criar_ou_atualizar_disputa', 'gerar_perguntas_disputa',
     '_criar_pergunta_se_couber'}
count = collections.Counter()
rows, errors = [], []

class Body(ast.NodeVisitor):
    def __init__(self):
        self.nodes = []
    def visit(self, n):
        self.nodes.append(n)
        if not isinstance(n, (ast.FunctionDef, ast.AsyncFunctionDef,
                              ast.ClassDef)):
            super().visit(n)

for p in sorted(pathlib.Path('app').rglob('*.py')):
    if any(x in {'tests', 'migrations'} for x in p.parts):
        continue
    if p.name.startswith(('test_', 'tests')):
        continue
    count['files'] += 1
    try:
        tree = ast.parse(p.read_text(), filename=str(p))
    except (SyntaxError, UnicodeError) as exc:
        errors.append((str(p), str(exc)))
        continue
    aliases = {a.asname or a.name: a.name
               for n in ast.walk(tree) if isinstance(n, ast.ImportFrom)
               for a in n.names}
    for f in ast.walk(tree):
        if not isinstance(f, (ast.FunctionDef, ast.AsyncFunctionDef)):
            continue
        count['functions'] += 1
        v = Body()
        for stmt in f.body:
            v.visit(stmt)
        calls = {
            aliases.get(n.func.id, n.func.id)
            if isinstance(n.func, ast.Name) else n.func.attr
            for n in v.nodes if isinstance(n, ast.Call)
            and isinstance(n.func, (ast.Name, ast.Attribute))
        }
        motifs = {n.value for n in v.nodes
                  if isinstance(n, ast.Constant)
                  and isinstance(n.value, str)} & M
        if motifs or calls & H:
            count['candidates'] += 1
            rows.append((str(p), f.lineno, f.name,
                         ','.join(sorted(motifs)),
                         ','.join(sorted(calls & H))))
print(dict(count), 'parse_errors', errors)
for row in rows:
    print('%s:%s %s | %s | %s' % row)
PY
```

Limite a preservar no relatório: esse AST gera **candidatas sintáticas**, depois os caminhos acima foram lidos transitivamente à mão; ele não resolve completamente imports reexportados, callbacks, `getattr`, dispatch dinâmico nem colisões de aliases locais. A afirmação “TODO emissor possível foi eliminado por grafo transitivo exaustivo” permanece **`[nao verificado]`**.

## 11/11 20:30 · sessao 1ac523deae44 · - · - · origem=Ronald (tmux)

Evidência para consolidação, considerando supervisor criado com `is_staff=True` e grupo `gestor` conforme recorte informado pelo pai:

- Todas as perguntas usam a mesma porta e a mesma guarda contextual, sem RBAC específico por motivo: `app/chamados/services/fio.py:558`, `app/chamados/services/fio.py:566`. Os serviços de validação/materialização não chamam `tem_acao`, `has_perm`, `get_perfil` ou `tem_perfil`; verificação AST resultou `[]` nos dois arquivos. O despacho da materialização é por `tipo_resposta`: `app/chamados/services/materializacao.py:1226`.
- `materializar_saida` pertence à porta de justificativa (`app/ponto/views.py:3118`), e não ao caminho de validar pergunta, que entra em `app/chamados/services/validacao.py:373` e chama materialização em `app/chamados/services/validacao.py:461`.
- Supervisor, admin e staff passam `_is_gestor` se staff ou nos grupos literais `dp_rh`, `gestor`, `admin_ti`: `app/chamados/views.py:24`, `app/chamados/views.py:35`. Staff não é bypass do departamento: o bypass é superuser/`tem_perfil(admin_ti)`, `app/chamados/utils.py:47`; a guarda normal lê competências, `app/chamados/utils.py:52`.
- “Reabrir administrativamente”, apesar da mensagem `admin_ti`, aceita qualquer `is_staff`: `app/chamados/services/acoes_disputa.py:245`.

Matriz dos motivos — mesma porta, mesma guarda:

| Motivo | Tipo | Fonte |
|---|---|---|
| `sem_posto` | `posto_dropdown` | `app/chamados/catalogo/perguntas.py:36` |
| `em_ferias` | `escolha` | `app/chamados/catalogo/perguntas.py:42` |
| `orfao_14h` | `hora` | `app/chamados/catalogo/perguntas.py:48` |
| `saida_sem_entrada` | `hora` | `app/chamados/catalogo/perguntas.py:56` |
| `posto_desativado` | `escolha` | `app/chamados/catalogo/perguntas.py:64` |
| `colisao_escala` | `escala_dropdown` | `app/chamados/catalogo/perguntas.py:70` |
| `ordem_substituicao_invalida` | `escolha` | `app/chamados/catalogo/perguntas.py:76` |
| `batida_fora_escala_grave` | `escolha` | `app/chamados/catalogo/perguntas.py:82` |
| `gps_ausente` | `escolha` | `app/chamados/catalogo/perguntas.py:88` |
| `foto_ausente` | `escolha` | `app/chamados/catalogo/perguntas.py:94` |
| `gps_mock` | `escolha` | `app/chamados/catalogo/perguntas.py:100` |
| `device_comprometido` | `escolha` | `app/chamados/catalogo/perguntas.py:106` |
| `liveness_indisponivel` | `escolha` | `app/chamados/catalogo/perguntas.py:112` |
| `ancora_ausente` | `data_hora` | `app/chamados/catalogo/perguntas.py:118` |
| `intervalo_saida` | `hora` | `app/chamados/catalogo/perguntas.py:124` |
| `intervalo_volta` | `hora` | `app/chamados/catalogo/perguntas.py:132` |

Botões e portas compartilhados:

| Botão | Template | URL → view → serviço/guarda |
|---|---|---|
| Validar | `app/templates/chamados/partials/modal_fio.html:284` (implausível: `:269`) | `chamados:modal_fio`, `app/chamados/urls.py:39` → `app/chamados/views.py:869`, gate `:872` → `app/chamados/services/fio.py:585`, guarda `:566` |
| Rejeitar resposta | `app/templates/chamados/partials/modal_fio.html:298` | mesma URL/view → `app/chamados/services/fio.py:625`, guarda `:566` |
| Corrigir (somente hora) | `app/templates/chamados/partials/modal_fio.html:312`, condição `:300` | mesma URL/view → `app/chamados/services/fio.py:625`, guarda `:566` |
| Validar/recusar/corrigir no detalhe | `app/templates/chamados/partials/_pergunta_card.html:93`, `:110` | `app/chamados/urls.py:14` → `app/chamados/views.py:359`; guarda área `:420`, despachos `:483`, `:505`, `:523` |

Atenção materialização: `posto_dropdown` e `escala_dropdown` não têm ramo em `app/chamados/services/materializacao.py:1226`; caem na recusa de ausência de efeito em `app/chamados/services/materializacao.py:1239`. Isso é recusa de implementação/estado para todos os perfis, não recusa de permissão. Rejeitar resposta segue disponível, conforme despacho `app/chamados/services/fio.py:637`.

Quem escreve / quem lê:

| Campo | Escritor | Leitor |
|---|---|---|
| `motivo`, `tipo_resposta`, opções | catálogo `app/chamados/catalogo/perguntas.py:34`; emissor lê em `app/chamados/services/disputa_emissao.py:314`, grava na abertura `:439` e `:448` | tipo lido no template `app/templates/chamados/partials/modal_fio.html:300` e materializador `app/chamados/services/materializacao.py:1226` |
| `resposta_colab`, `respondida_em` | `app/chamados/services/disputa_emissao.py:1467` | template `app/templates/chamados/partials/modal_fio.html:217`; validação `app/chamados/services/validacao.py:393` |
| `validada_em`, `validada_por` | carimbo `app/chamados/services/validacao.py:134`; resposta nova limpa em `app/chamados/services/disputa_emissao.py:1480` | validação `app/chamados/services/validacao.py:396`; template `app/templates/chamados/partials/modal_fio.html:228` |
| permissão `rbac_*` | semeadura `app/colaboradores/mixins.py:354`; supervisor inclui gerir_chamados `:362` | `tem_acao`, `app/colaboradores/mixins.py:322`; consulta `get_all_permissions` `:331` |
| departamento/dono | catálogo `app/chamados/catalogo/modulos.py:103`; mapa dono→departamento `app/chamados/utils.py:27` | `departamento_do_chamado`, `app/chamados/utils.py:32`; `pode_agir_chamado`, `:68` |

Script AST executado sem escrever arquivos:

```bash
python3 -B - <<'PY'
import ast
from pathlib import Path

for rel in [
    'app/chamados/services/validacao.py',
    'app/chamados/services/materializacao.py',
]:
    tree = ast.parse(Path(rel).read_text())
    found = [
        (n.lineno, ast.unparse(n.func))
        for n in ast.walk(tree)
        if isinstance(n, ast.Call)
        and any(k in ast.unparse(n.func)
                for k in ['tem_acao', 'has_perm', 'get_perfil', 'tem_perfil'])
    ]
    print(rel, 'chamadas RBAC:', found)
PY
```

Saída: `[]` em ambos. Nenhum teste/runtime/produção executado; permissões efetivas de usuários permanecem [não verificado]. [nao verificado]

## 11/11 20:30 · sessao 0db0d6465a77 · - · - · origem=Ronald (tmux)

AST em memória confirmou que `PerguntaDisputa`, declarada entre `app/chamados/models.py:2406` e `app/chamados/models.py:3546`, não declara `criado_em` nem `criada_em`; `materializada_em` é carimbo de processamento, atribuído em `app/chamados/models.py:3099`, não instante de nascimento (`app/chamados/models.py:3393`). A hora de emissão da pk52340 permanece `[nao verificado]`.

O pareador ordena por `timestamp`, abre em E e fecha na S posterior, incluindo travessia da meia-noite (`app/ponto/turnos.py:1217`, `app/ponto/turnos.py:1419`, `app/ponto/turnos.py:1491`). O `Turno` fornecido para 142873→143189 representa fechado e `cross_meianoite=True`, conforme a ordem dos campos e sua construção (`app/ponto/turnos.py:43`, `app/ponto/turnos.py:321`). A correspondência concreta com essas pks vem somente da sonda fornecida `[nao verificado]`.

| Campo | Escritor da cópia | Leitor relevante |
|---|---|---|
| Pergunta `motivo` | `_nascer`, `app/chamados/models.py:3014` | identidade, `app/chamados/services/fio.py:1460` |
| `data_evento` | `gravar_dia_do_fato`, `app/chamados/services/fio.py:800`; nascimento chama em `app/chamados/models.py:3016` | `app/chamados/services/fio.py:1460`, `app/chamados/services/fio.py:1495` |
| `celula` | busca pelo dia do fato em `app/chamados/models.py:2896`; nascimento em `app/chamados/models.py:3014`; reuso em `app/chamados/models.py:2966`; rechave em `app/chamados/services/fio.py:1038` | `app/chamados/juizes.py:1331` |
| `colaborador` | identidade no nascimento, `app/chamados/models.py:3430` | `app/chamados/services/fio.py:1459` |
| `instancia_marco` | `app/chamados/models.py:3437`, `app/chamados/models.py:3440`, usando `app/chamados/services/declaracao_texto.py:55` | `app/chamados/services/fio.py:1460`, `app/chamados/services/fio.py:1495` |
| `batida_origem` | `app/chamados/models.py:3448`; família MARCO retorna `None` em `app/chamados/models.py:3461` | `app/chamados/services/fio.py:1487` |
| `conversa` | porta `abrir` passa False, `app/chamados/models.py:2789`; `abrir_conversa` passa True, `app/chamados/models.py:2801`; persistência em `app/chamados/models.py:3014` | `app/chamados/juizes.py:78`, `app/chamados/services/fio.py:1445` |
| `materializada_em` / `via_resolucao` | `app/chamados/models.py:3099`, `app/chamados/models.py:3101` | `app/chamados/juizes.py:1370` |
| Batida `timestamp` / `tipo` / `sequencia` | porta app passa em `app/api/views_core.py:645`, `app/api/views_core.py:650`; create em `app/ponto/registro_batida.py:157` | `timestamp`: `app/ponto/turnos.py:1217`; `tipo`: `app/ponto/turnos.py:1382`; `sequencia`: contador em `app/ponto/turnos.py:2387` |
| Batida `criado_em` | Django `auto_now_add`, `app/ponto/models.py:79` | guard de eco, `app/ponto/turnos.py:355`, `app/ponto/turnos.py:362` |
| Batida `retratada_em` | `retratar_batida` passa anotação em `app/ponto/registro_batida.py:274`; grava por `setattr`/save em `app/ponto/registro_batida.py:252` | universo em `app/ponto/turnos.py:2876` |

`batidas_apuraveis` não filtra aprovação ou tipo: apenas retratação nula, colaborador e janela opcionais (`app/ponto/turnos.py:2876`, `app/ponto/turnos.py:2877`, `app/ponto/turnos.py:2881`). `turnos_do_colab` não chama literalmente essa função: carrega batidas com retratação nula e janela com pad próprio, depois chega a `parear_turnos` (`app/ponto/turnos.py:1847`, `app/ponto/turnos.py:1863`, `app/ponto/turnos.py:1992`). O cartório usa `batidas_apuraveis` na entrada da grade (`app/ponto/services/cartorio.py:1156`, `app/ponto/services/cartorio.py:1158`, `app/ponto/services/cartorio.py:1161`); seus insumos entrada/saída vêm das lâmpadas, não de presença de tipos E/S (`app/ponto/services/cartorio.py:823`, `app/ponto/services/cartorio.py:845`).

RED proposto: fixture com E às 19:00 em D e S às 07:00 em D+1, ambas apuráveis, escala/célula noturna; congelar o relógio após a S e o prazo aplicável; chamar a porta emissora e exigir nenhuma pergunta `orfao_14h` para D, além de turno único fechado. Mutante: descartar a resposta fechada do pareador e decidir ausência de S apenas no dia civil D; precisa gerar a pergunta e matar o RED. Proposta não executada `[nao verificado]`. O teste existente comprova somente o pareamento 22–06, não a emissão (`app/ponto/tests/test_turnos.py:12`).

## 11/11 20:29 · sessao bafd5a525e88 · - · - · origem=Ronald (tmux)

Censo só leitura das permissões e portas genéricas:

- **Supervisor da UI** recebe grupo `gestor` + grupos de setores de supervisão, e é criado com `is_staff=True`, `is_superuser=False`: `app/core/views_usuarios.py:48`, `:52`, `:291`, `:295`, `:296`, `:299`. Supervisor legado também ganha staff ao alterar `Colaborador.perfil`: `app/core/credenciais.py:132`, `:135`, `:139`.
- **Entrada da fila/fio:** `_is_gestor` aceita staff ou grupo literal `dp_rh`, `gestor`, `admin_ti`: `app/chamados/views.py:24`, `:35`. Painel recusa com 404 em `:723`; fio em `:662`; modal em `:872`. Não basta chamar-se `supervisao` sem staff/grupo gestor: `app/chamados/views.py:35`.
- **`tem_acao` não trata staff/admin_ti como bypass:** só superuser ou permissão efetiva nativa: `app/colaboradores/mixins.py:322`, `:328`, `:330`. Os grants reais da produção são **[não verificado]**.
- **Default supervisor:** `gerir_chamados`, `regularizar_batida`, `editar_escalas`, `editar_ausencias`, entre outros, mas sem `materializar_saida`: `app/colaboradores/mixins.py:355`. DP tem `materializar_saida` em `:364`; TI em `:376`.

| Porta/ação | Botão | URL / view | Guarda |
|---|---|---|---|
| Resposta + mover status no modal | `app/templates/chamados/partials/modal_fio.html:665`, `:672` | `/chamados/modal/<colab>/`, `app/chamados/urls.py:39`; `app/chamados/views.py:869`; executor `app/chamados/services/fio.py:708` | entrada `_is_gestor` `views.py:872`; render `modal_fio.html:652`; POST contextual `fio.py:714`; transição `:721` |
| Resposta + mover status no fio | `app/templates/chamados/fio_colaborador.html:265`, `:272` | `/chamados/colaborador/<colab>/`, `urls.py:21`; `views.py:659`; executor `fio.py:708` | entrada `views.py:662`; render `fio_colaborador.html:255`; contextual `fio.py:714`; transição `:721` |
| Resposta + mover status no detalhe | `app/templates/chamados/detalhe_local.html:393`, `:403` | `/chamados/local/<id>/`, `urls.py:14`; `views.py:359`; executor `acoes_chamado.py:283` | contextual `views.py:420`; execução status `acoes_chamado.py:313`; model `models.py:128` |
| Aprovar exceção Art.468 | `modal_fio.html:599`; `detalhe_local.html:258` | `/chamados/local/<id>/`, `urls.py:14`; `views.py:426`; `acoes_chamado.py:21` | `_pode_aprovar` aceita staff ou grupo gestor/admin_ti: `views.py:41`; contextual `views.py:420` |
| Autorizar/recusar geofence no modal | `modal_fio.html:506`, `:517` (ações; botão localizar no bloco) | modal acima; executor `fio.py:533`; `acoes_chamado.py:165`, `:243` | `_is_gestor` entrada; contextual `fio.py:541` |
| Regularizar disputa | `modal_fio.html:355`; `detalhe_local.html:317` | modal/local acima; `fio.py:646`; `views.py:467`; `acoes_disputa.py:118` | contextual `fio.py:566` / `views.py:420`; exige resolução `fio.py:648`; muda viva recusa `:660` / `acoes_disputa.py:130` |
| Arquivar | URL `app/chamados/urls.py:26`; botão não rastreado neste subcenso | `/chamados/arquivar/<id>/`, `views_arquivar.py:24` | `tem_acao gerir_chamados` `:26`; área `:32` retorna **403** em `:34` |
| Reabrir disputa administrativa | botão não rastreado neste subcenso | modal, `fio.py:581`; `acoes_disputa.py:235` | contextual `fio.py:566`; **staff OU grupo literal admin_ti** `acoes_disputa.py:245` |
| Reabrir/recobrar questionário | URL `urls.py:20`; view `views.py:1372` | `/chamados/local/<id>/reabrir-disputa/` | peculiaridade: só recusa se **não tem gerir_chamados E não pode_agir_chamado**, `views.py:1404` |

**RBAC contextual:** `app/chamados/utils.py:32` resolve departamento pelo dono de `MOTIVOS[modulo_origem]`; solicitações com área válida usam a área antes do dono (`:39`). `colab`, `cadastro`, `supervisao` → supervisão; `dp` → DP; `sistema` → TI (`:27`). Permite gestor geral superuser/`tem_perfil admin_ti` (`:47`, `:57`), legado sem competências (`:60`), ou competência igual ao departamento (`:65`). `tem_perfil` lê competências dos setores, não grupo literal por nome (`mixins.py:193`).

**Módulos do catálogo recusados ao supervisor puro pelo dono, se estiverem em Decidir:** `ausencia_colab` (`catalogo/modulos.py:120`), `colaborador_em_ferias` (`:123`), `desligamento_revisao` (`:141`), `mensagem_dp` (`:192`), `beneficio` (`:195`) → DP; `acesso_bloqueado` (`:154`), `smoke` (`:198`) → TI. Solicitações de qualquer área diferente de supervisão também: `utils.py:39`, `:65`. Ocorrência concreta na fila de produção **[não verificado]**.

**Veto de negócio não é veto por perfil:** `ChamadoColaborador.transicionar` consulta perfil (`models.py:130`), recusa justificativa pendente (`:136`), disputa viva (`:153`), pergunta viva ancorada (`:165`) e furo não regularizado (`:181`). Motor só reserva reabertura do fechado/admin overrides a `admin_ti`: `catalogo/motor.py:150`, `:153`. `DisputaSupervisao.pode_fechar` exige ausência de respostas sem veredito e fio vivo: `models.py:1848`; `fechar` recusa respostas sem veredito `:1886` e vivas `:1897`. `admin_encerra` existe no modelo (`:1943`) mas nenhum chamador web foi encontrado no grep de `app/chamados` **[não verificado quanto a outras apps]**.

**Quem escreve / quem lê:**

| Campo/permissão | Escritores identificados | Leitores |
|---|---|---|
| `Group.permissions`, `rbac_*` | semeadura `colaboradores/signals.py:25`, `:30` → `mixins.py:397`; alterações UI `core/views_quadro.py:42`, `:45` | `mixins.py:330`; exibição `core/views_quadro.py:59` |
| `User.groups` | criação `core/views_usuarios.py:299`; add/remove `:204`, `:206` | competência `mixins.py:139`; perfil `:116`; `_is_gestor` `chamados/views.py:38`; `_pode_aprovar` `:45` |
| `User.is_staff` | criação `core/views_usuarios.py:295`; mudança de perfil `core/credenciais.py:139` | `_is_gestor` `chamados/views.py:36`; aprovação `:43`; apresentação admin `:707`, `:936`; reabrir admin `acoes_disputa.py:245`; fallback perfil `mixins.py:177` |
| `User.is_superuser` | criação `core/views_usuarios.py:296`; alternância `:82` | `mixins.py:163`, `:194`, `:328`; geral contextual `chamados/utils.py:49` |
| `Colaborador.perfil` | `core/credenciais.py:135`, acionado em `colaboradores/views.py:1470` | `mixins.py:171`; `transicionar` consome `get_perfil` em `chamados/models.py:131` |
| `Setor.competencia/ativo` | criar `core/views_usuarios.py:248`, `:254` → `colaboradores/services/estrutura.py:97`; toggle ativo `core/views_usuarios.py:377` → `estrutura.py:109` | `mixins.py:117`, `:140`; semeadura `signals.py:30` |
| `ChamadoColaborador.status_local` | núcleo `chamados/models.py:228`, chamado por `_aplicar` `:450` | partição/rótulo por outro subagente; botões genéricos `modal_fio.html:651`; motor `catalogo/motor.py:139` |
| `Disputa.fechada_em/fechada_por/resolucao/motivo_fechamento` | `chamados/models.py:1932` | porta `:1879`; roteamento pergunta `services/fio.py:600`; busca detalhe `acoes_chamado.py:335` |

`user_permissions` individuais são lidas indiretamente por `get_all_permissions` (`mixins.py:331`) e exibidas em `core/views_usuarios.py:159`; escritor próprio na árvore não identificado **[não verificado]**.

## 11/11 20:29 · sessao 1bdb3edf051f · - · - · origem=Ronald (tmux)

Censo de emissão factual de saída na cópia:

| Caminho | Fonte do dia | Chamada da porta |
|---|---|---|
| Detector `orfao_14h` → geração | Batida pendente → `resolver_data_evento` → `_T1`; grava contexto com D. `app/chamados/detector_anomalias.py:41`; `app/escala/servico_jornada.py:495` | `app/chamados/services/disputa_emissao.py:444` |
| Pendentes adicionais | Cada `pk` → `dia_do_turno_da_batida`; ignora o dia civil do item. `app/chamados/services/disputa_emissao.py:272` | Mesma geração, `app/chamados/services/disputa_emissao.py:294` |
| Fábrica da célula | `cel.data` → contexto de motivo. `app/chamados/services/fabrica_da_celula.py:172` | Mesma geração, `app/chamados/services/fabrica_da_celula.py:174` |
| Repasse, grupo com origem | `resolver_data_evento(...batida_origem)`; sem resposta não emite. `app/chamados/services/disputa_emissao.py:1205` | Helper em `app/chamados/services/disputa_emissao.py:1213`; `abrir` em `app/chamados/services/disputa_emissao.py:837` |
| Repasse, sem origem | `data_evento_do_contexto` cru. `app/chamados/services/disputa_emissao.py:1232` | `app/chamados/services/disputa_emissao.py:1244` |
| Complemento de marcos | Dia do marco faltante → `dia_do_turno_da_celula`. `app/chamados/services/disputa_emissao.py:873` | `app/chamados/services/disputa_emissao.py:881` |
| Declaração do colab | Marco faltante casado com hora → dia da célula. `app/chamados/services/declaracao_texto.py:345` | `app/chamados/services/declaracao_texto.py:376` |
| Declaração por texto/admin | Mesmo marco faltante → dia da célula. `app/chamados/services/declaracao_texto.py:628` | `app/chamados/services/declaracao_texto.py:666` |

Os outros nascimentos encontrados são `abrir_conversa`, excluídos do fato de saída: `app/chamados/services/disputa_emissao.py:439`, `app/chamados/services/disputa_emissao.py:1371`, `app/chamados/services/acoes_disputa.py:263`.

**Dedup:** `orfao_14h` pertence à família MARCO (`app/chamados/catalogo/motivos.py:34`). Chave é `(colaborador, data_evento, motivo, instancia_marco)`, excluídas conversa e vias fora do fato, com índice só a partir do horizonte (`app/chamados/models.py:3482`). Dedup da porta atravessa disputas; procura o universo do fato e elege canônica (`app/chamados/models.py:2905`). Os pré-dedups do emissor ainda usam `(disputa,motivo,data_evento,conversa=False)` (`app/chamados/services/disputa_emissao.py:417`; `app/chamados/services/disputa_emissao.py:1415`).

**Limite importante:** não é correto afirmar que todas as fontes consultam `_T1`. A geração lê `contexto[motivo].data` diretamente (`app/chamados/services/disputa_emissao.py:371`) e o embrulha em `dia_do_turno_da_celula` (`app/chamados/services/disputa_emissao.py:443`). O repasse sem origem usa parser que aceita `data_batida`, `entrada_timestamp`, `pendentes` e `horario_previsto`, extraindo datas sem consultar turno (`app/chamados/data_evento.py:27`). Na porta, pergunta MARCO sem episódio devolve `dia_do_turno_da_celula(d)` (`app/chamados/services/fio.py:1638`); esse construtor apenas devolve `DiaDoTurno(d)`, sem converter (`app/ponto/turnos.py:2939`). Portanto D e D+1 errados continuam sendo chaves distintas; a porta única não corrige automaticamente contexto civil legado.

**Conclusão sobre hoje:** detector principal, pendentes e repasse com origem foram corrigidos para D (`app/chamados/detector_anomalias.py:45`; `app/chamados/services/disputa_emissao.py:274`; `app/chamados/services/disputa_emissao.py:1203`). Fábrica e declarações usam dia da célula. O cenário normal novo deve convergir pelo dedup; reprodução executada disso é [nao verificado]. Garantia universal contra dia civil errado não existe nos dois caminhos acima, pelas linhas citadas.

**Histórico dos pares:** a documentação junto ao resolvedor descreve exatamente o legado: `orfao_14h` gravava dia da saída esperada D+1 e o leitor somava mais um (`app/escala/servico_jornada.py:498`). Isso explica o mecanismo compatível com os pares da sonda; qual chamada efetiva criou cada PK, e se houve dois emissores ou duas invocações do mesmo, é [nao verificado]. A sonda não traz campo de emissor/criação da pergunta; não atribuir individualmente a fábrica/detector como fato.

**RED sugerido:** inserir em `app/chamados/tests/test_plantio_noturno_mais1.py:75`, `OrfaoNoturnoTest`. Há fixture noturna em `app/chamados/tests/test_plantio_noturno_mais1.py:48`, D=21/10 em `app/chamados/tests/test_plantio_noturno_mais1.py:30`, teste do detector em `app/chamados/tests/test_plantio_noturno_mais1.py:90`. Acrescentar emissão combinada detector + fábrica + repasse, E(D,19h), S ausente, célula D trabalho e D+1 folga; exigir exatamente uma pergunta `orfao_14h`, `data_evento=D`, célula D e mesma PK após repetição. Mutante: resolvedor/contexto do detector volta a D+1 enquanto fábrica emite D; deve nascer segunda pergunta, pois o dedup distingue as datas. Especificação não executada: [nao verificado].

## 11/11 20:29 · sessao 9de252d3d87f · - · - · origem=Ronald (tmux)

Eleição e vínculo conferidos na cópia, sem alterações.

| Ponto | Resultado e prova |
|---|---|
| Regra da dona | Núcleo, agrupador e âncora selecionam `filter(colaborador=..., fechada_em__isnull=True).first()`, sem filtro de dia. A ordenação do modelo é `['-aberta_em']`: escolhem a mais recente ainda aberta. (`app/chamados/services/disputa_emissao.py:531`, `:1096`, `:1332`; `app/chamados/models.py:1804`.) |
| Helper de leitura | `colaborador_em_disputa` aplica a mesma seleção; é leitor, não escritor. (`app/chamados/services/disputa_emissao.py:34`, `:40`.) |
| Novos dias na mesma dona | O wrapper chama `gerar_perguntas_disputa` também no reuso da DS, mesmo com motivo repetido. A fábrica envia o dia próprio da célula. (`app/chamados/services/disputa_emissao.py:662`, `:682`; `app/chamados/services/fabrica_da_celula.py:172`.) |
| Declarações | Colaborador e administração obtêm a DS por `disputa_para_declaracao` → núcleo; depois abrem P com seu dia e chamado. (`app/chamados/services/declaracao_texto.py:360`, `:376`, `:659`, `:666`; `app/chamados/services/disputa_emissao.py:627`.) |
| Complemento de pergunta existente | O agrupador pode selecionar a dona da P que já aponta ao chamado, em vez da aberta global. (`app/chamados/services/disputa_emissao.py:1070`, `:1073`, `:1081`.) |
| Escritor de `P.disputa` | O nascimento recebe `disputa` no construtor e persiste com `force_insert`; reuso retorna a P existente sem mudar sua dona. (`app/chamados/models.py:3014`, `:3017`, `:3008`.) |
| Autoria histórica dos seis registros | A sonda não contém trilha de nascimento/caller. Portanto, atribuir individualmente cada P a agrupador, fábrica, API ou declaração é **[nao verificado]**; o sumidouro canônico atual é o acima. (`app/chamados/models.py:3014`, `:3025`.) |

`DS.chamado` é FK nullable com `SET_NULL` e reverse `disputa_origem`, sem unicidade; a sonda confirma especificamente `7279.chamado_id=33492` e inversa `[7279]`. Os valores da sonda são dado recebido sem arquivo numerado: **[nao verificado]**. A definição do relacionamento é `app/chamados/models.py:1749`.

Há três escritores de nascimento da FK:

| Escritor | Eleição do pai |
|---|---|
| Núcleo: `app/chamados/services/disputa_emissao.py:587`, campo `:591` | `ChamadoColaborador.abrir`, categoria `disputa_supervisao`, módulo `disputa_supervisao`, status VIVOS. (`:568`.) |
| Agrupador: mesmo arquivo `:1123`, campo `:1127` | `abrir`, módulo `disputa_supervisao_manual`, subcategoria `orfao_14h`, VIVOS. (`:1104`.) |
| Âncora: mesmo arquivo `:1358`, campo `:1362` | `abrir`, módulo manual, subcategoria `ancora_ausente`, VIVOS. (`:1339`.) |

Os ramos de reuso alteram motivos/contexto, preservando pai e data de abertura. (`app/chamados/services/disputa_emissao.py:541`, `:562`, `:565`, `:1131`, `:1366`.) A assinatura do chamado 33492 é compatível com o nascimento pelo agrupador; identificar o ato histórico é **[nao verificado]**.

`DS.chamado` participa da apresentação em dois passos: `montar_chamados_fio` mapeia as disputas por pai, e `chamado_que_lista` escolhe `P.chamado_destino_id` ou, quando nulo, `DS.chamado_id`. (`app/chamados/services/fio.py:184`, `:192`, `:199`, `:116`, `:117`, `:240`, `:243`, `:252`, `:258`.)

Assim, pelas FKs fornecidas, P51823 e P52340 sem destino seriam listadas no container 33492; as outras quatro têm seus próprios destinos. A execução histórica dessa tela é **[nao verificado]**. A regra está em `app/chamados/services/fio.py:117`.

A data exibida no cabeçalho do modal é `Chamado.criado_em`; os cards também agrupam por esse campo. (`app/templates/chamados/partials/modal_fio.html:86`; `app/templates/chamados/partials/_card_atendimento.html:5`.) Seu escritor é `auto_now_add`, não a data da pergunta. (`app/chamados/models.py:67`.) O catálogo declara ambos os módulos de disputa sem chave de dia e `data_do_chamado` devolve `None` nessa condição. (`app/chamados/catalogo/modulos.py:144`, `:146`, `:219`, `:223`.)

Isso explica o mecanismo de “container criado em 22/10 contendo P de 30/10 e 31/10”; não demonstra qual interface foi vista historicamente. (`app/chamados/services/fio.py:117`; `app/templates/chamados/partials/modal_fio.html:86`; interface histórica: **[nao verificado]**.)

O fechamento do pai depende de outro ato: o receiver S88 retorna imediatamente enquanto `DS.fechada_em` está nulo; após fechamento tenta `ch.reconciliar()`. (`app/chamados/signals.py:28`, `:32`, `:42`.) A FK e a agregação não impõem fechamento ou renovação por dia. (`app/chamados/services/disputa_emissao.py:531`, `:566`; `app/chamados/signals.py:28`.)

`Chamado.status_local` é gravado em `_gravar_estado`; `analisar()` consulta o motor e delega a esse escritor. (`app/chamados/models.py:201`, `:228`, `:800`, `:803`, `:806`.) O motor permite `ANALISAR` de aberto/registrado para `em_analise`. (`app/chamados/catalogo/motor.py:112`.) Qual desses atos deixou especificamente 33492 em análise é **[nao verificado]**.

Não identifiquei deslocamento de linhas nem substituição dos sítios de eleição/vínculo citados no `f0f1.r`: os endereços acima coincidem. Comparação Git e identidade criptográfica com PROD são **[nao verificado]**.

## 11/11 20:29 · sessao 204cfd08d6af · - · - · origem=Ronald (tmux)

F0 pertinente confirmado por AST e abertura dos corpos; todos os sítios persistentes já citados no `f0f1.r` permanecem nos mesmos endereços abaixo. Um escritor foi omitido pelo inventário anterior: `limpar_resolucao_residual`. Ser alteração posterior às 02h, em vez de omissão anterior: [nao verificado].

| Campo | Escritor persistente / valor | Delta frente ao inventário |
|---|---|---|
| `P.disputa` | `_nascer`: construtor `app/chamados/models.py:3014`, inserção `:3017` | Mesmo endereço |
| `P.chamado_destino` nascimento | Mesmo construtor/inserção `app/chamados/models.py:3014`, `:3017` | Mesmo endereço |
| `P.chamado_destino` reuso | `_nascer`: atribuição `app/chamados/models.py:2982`, lista dinâmica `:2983`, persistência `:3006`; retorno preserva dona `:3008` | Mesmo endereço |
| `P.chamado_destino` reaponte | `reapontar_resolvedora`: `app/chamados/services/fio.py:925`, save `:927` | Mesmo endereço |
| `DS.chamado` nascimento núcleo | `app/chamados/services/disputa_emissao.py:587`, keyword `:591` | Mesmo endereço |
| `DS.chamado` agrupador | `app/chamados/services/disputa_emissao.py:1123`, keyword `:1127` | Mesmo endereço |
| `DS.chamado` âncora | `app/chamados/services/disputa_emissao.py:1358`, keyword `:1362` | Mesmo endereço |
| `DS.aberta_em` | ORM no nascimento, `auto_now_add=True`: `app/chamados/models.py:1756`; três inserções acima | Campo não incluído no script antigo |
| `DS.fechada_em`/`resolucao`/`motivo_fechamento` nascimento | `NULL`/`''`/`''` pelos campos: `app/chamados/models.py:1757`, `:1773`, `:1793` | Nascimentos permanecem |
| Os três campos no fechamento | `fechar`: `app/chamados/models.py:1932`, `:1934`, `:1935`; save `:1936` | Mesmo endereço |
| Os três campos na reabertura admin | `app/chamados/models.py:2149`, `:2151`, `:2152`; save `:2161` | Mesmo endereço |
| Os três campos na reabertura sistema | `app/chamados/models.py:2173`, `:2175`, `:2176`; save `:2177` | Mesmo endereço |
| **`DS.resolucao` limpeza residual** | **`limpar_resolucao_residual`: `app/chamados/models.py:2265`, save `:2266`; guarda aberta e texto não vazio `:2262`, trilha `:2269`** | **Novo no inventário; novidade temporal [nao verificado]** |

O script anterior também não incluía `resolucao` e `chamado` no conjunto `FIELDS`, embora enumerasse construtores da DS por nome. Isso explica por que não bastava para o escopo agora solicitado; literal do script anterior: [nao verificado].

Nenhum sítio movido/removido fica demonstrado pela comparação com os endereços fornecidos: os endereços principais permanecem. Comparação Git real com uma árvore das 02h: [nao verificado].

Classificação dos candidatos não persistentes:

| Candidato | Classificação |
|---|---|
| `app/chamados/models.py:367` | Memória: reassocia o mesmo chamado, conforme identidade conferida `:366`; não persiste essa atribuição |
| `app/chamados/models.py:2876` | Proxy temporário para o juiz do dia, chamada `:2875`; não é o `p` inserido em `:3014` |
| `app/chamados/services/fio.py:779`, `:783` | Simulação seguida de restauração em `finally`, para `pergunta_viva` `:781` |
| `app/chamados/services/regularizacao_ext.py:256`, save `:257` | Campo de outra entidade (`reg`), não DS |
| `app/chamados/migrations/0051_decisao_pergunta.py:25` | Escreve `colaborador_id`; `disputa_id` está somente no `OuterRef` de leitura |

Escapes e efeitos estruturais:

- `PerguntaDisputa.descarimbar_em_lote` permite campos adicionais em `**outros`; a guarda proíbe somente campos do carimbo, deixando capacidade latente para os alvos deste censo. (`app/chamados/models.py:3252`, `:3255`, `:3264`, `:3266`.)
- A cadeia de nascimento aceita `**campos`; encaminha por `abrir`/`abrir_conversa` → `_sob_contrato` → `_nascer`, terminando no construtor. (`app/chamados/models.py:2776`, `:2789`, `:2794`, `:2801`, `:2820`, `:3014`.) Uso produtivo atual de `disputa_id`/`chamado_destino_id` por essa expansão: [nao verificado].
- O `save` genérico da P chama `super().save` dentro de atomic. Não é uma eleição adicional. (`app/chamados/models.py:3413`, `:3422`, `:3423`.)
- Exclusão de chamado pode zerar `DS.chamado` ou `P.chamado_destino` por `SET_NULL`; exclusão da DS elimina P por `CASCADE`. (`app/chamados/models.py:1751`, `:3387`, `:2418`.)
- `texto.reescrever_texto` não é escape para esses campos: sua lista permite apenas título/assunto/descrição e rejeita os demais. (`app/chamados/services/texto.py:18`, `:25`, `:27`, `:33`.)
- `core.reexibir` é mutação em memória e devolução do objeto; não salva no próprio helper. (`app/core/reexibir.py:45`, `:49`, `:50`.) Ausência universal de persistência posterior por qualquer consumidor: [nao verificado].
- Chamador produtivo nominal de `limpar_resolucao_residual` não foi encontrado pela busca; execução dessa porta em produção: [nao verificado].

Leitores abertos e conferidos:

| Campo | Leitores |
|---|---|
| `P.disputa` | Identidade: `app/chamados/models.py:3430`; fallback do fio: `app/chamados/services/fio.py:116` |
| `P.chamado_destino` | Chamado de resolução do fio: `app/chamados/services/fio.py:117`; simulação do C6: `:778`; guarda e comparação do reuso: `app/chamados/models.py:2968` |
| `DS.chamado` | Fallback do fio: `app/chamados/services/fio.py:117`; fechamento do pai: `app/chamados/signals.py:28`, `:32`; trilha de reabertura: `app/chamados/models.py:2178`, `:2180` |
| `DS.aberta_em` | Ordenação que determina `.first()`: `app/chamados/models.py:1804`; cor por abertura crescente: `:1819` |
| `DS.fechada_em` | Propriedade aberta: `app/chamados/models.py:1824`; eleição núcleo: `app/chamados/services/disputa_emissao.py:532`; eleição agrupador `:1097`; âncora `:1333`; signal pai `app/chamados/signals.py:28` |
| `DS.resolucao` | Idempotência de fechar: `app/chamados/models.py:1880`; guarda/antes da limpeza `:2262`, `:2264`; texto para fechamento do pai `app/chamados/signals.py:45`; resolução pedida/fallback materializador `app/chamados/services/materializacao.py:1333` |
| `DS.motivo_fechamento` | Idempotência de fechar `app/chamados/models.py:1881`; vigia disputa vazia `app/chamados/services/vigia_fio.py:734` |

Migrations são schema, não novos eleitores: DS aberta/fechada/resolução/chamado em `app/chamados/migrations/0013_disputasupervisao.py:23`, `:24`, `:25`, `:26`; P.disputa em `app/chamados/migrations/0014_perguntadisputa.py:27`; destino em `app/chamados/migrations/0016_s84_pergunta_chamado_destino.py:18`; motivo de fechamento em `app/chamados/migrations/0049_disputa_motivo_fechamento.py:14`.

O selo existente continua por arquivo e regex, incluindo somente atribuição/update e excluindo migrations; não demonstra função escritora única nem pega todos os construtores/kwargs. (`app/core/tests/test_contract_escritores_estado.py:61`, `:179`, `:182`, `:191`.) A ferramenta de censo geral documenta baldes não atribuíveis e inferências; não usar “nenhum resultado” como prova universal de ausência de escrita dinâmica. (`app/core/censo_escritas.py:15`, `:38`, `:43`, `:54`.)

## 11/11 20:29 · sessao 5e279eb98337 · - · - · origem=Ronald (tmux)

Rastreamento de templates e causas, só leitura:

| Item/causa | Botão e condição | URL |
|---|---|---|
| Proposta de escala | `modal_fio.html:54` Aplicar; `:64` Recusar; basta `proposta_viva` (`:40`), sem guarda de perfil no template | `chamados:modal_fio`, `acao=aplicar_proposta_escala/recusar_proposta_escala`, forms `:49/:56` |
| Pergunta respondida não validada | `modal_fio.html:269/:284` Validar; `:298` Rejeitar; `:312` Corrigir hora; bloco condicionado ao `pode_agir_map` em `:123`, resposta `:217`, não validada `:228/:239` | `chamados:modal_fio` |
| Pergunta sem data, não respondida | `modal_fio.html:193` Aplicar correção; `:200` Apagar; condição `:185` e guarda externa `:123` | `chamados:modal_fio` |
| Datas divergentes | `modal_fio.html:177` Conferir data; depende `pode_conferir_data` `:172` | `chamados:conferir_data_pergunta`, form `:173` |
| Disputa aberta, sem `ancora_ausente`, todas respondidas | `modal_fio.html:359` Regularizar; condições `:326/:327/:334` e guarda externa `:123` | `chamados:modal_fio`, ação em `:355` |
| Disputa aberta com `ancora_ausente` | Não oferece fechamento manual, aviso `modal_fio.html:327-332`; continua oferecendo Reabrir `:372` | `chamados:reabrir_disputa_supervisao`, form `:368` |
| Disputa aberta com pergunta sem resposta | Não oferece Regularizar; oferece Cobrar `modal_fio.html:334-351` e Reabrir `:372` | `chamados:cobrar_chamado`, `chamados:reabrir_disputa_supervisao` |
| Disputa fechada | Reabertura administrativa `modal_fio.html:412`, exige `is_admin_ti` `:382` e contador <2 `:383` | `chamados:modal_fio`, `reabrir_disputa_admin` `:390` |
| GPS fora geofence | Aceitar `modal_fio.html:509`; advertir `:520`; recusar `:532`, guarda `:500` | `chamados:modal_fio` |
| `sem_previsao_escala`, aberto, sugestão fixa | Aceitar folga `modal_fio.html:551-553`; condição `:544`, guarda `:547` | `chamados:detalhe_local`, `aceitar_folga_limbo` `:550` |
| `regua_suspeita`, aberto/em análise | Confirmar cadastro errado `modal_fio.html:567-569`; realocar `:576-578` só se destino `:572`; guarda `:563` | `chamados:detalhe_local`, `corroborar_regua/realocar_regua` |
| `batida_fora_horario`, aberto, com motivo anuência | Aprovar exceção `modal_fio.html:599-601`; condição `:588`; guarda `:595`; alternativa desabilitada `:605` | `chamados:detalhe_local`, `aprovar_excecao` |
| Furo sem pergunta regularizável | Perguntar ao colaborador `modal_fio.html:629-630`, condição primeiro card + contador + gestor `:617` | `chamados:modal_fio`, `iniciar_disputa_orfaos` |
| `ancora_limbo`, aberto | Pedir confirmação `modal_fio.html:645`; condição e guarda `:636` | `chamados:modal_fio`, `pedir_confirmacao_ancora` |
| Demais locais não fechados, sem GLPI e fora modo benefício | Enviar `modal_fio.html:672`, seletor mover `:665-670`; condições `:651-652` | `chamados:modal_fio` |
| `ausencia_colab` | No detalhe, link Abrir registro e validar `detalhe_local.html:233-235`; condição `:221`; bloco ausente no fio [não verificado por referência positiva] | `chamados:ir_para_ausencia` |

Todos os caminhos acima são `app/templates/chamados/...`; esta tabela descreve renderização, a aceitação POST depende das guardas que o outro agente rastreia.

A lista mostra **Decidir sem botão inline** em `_lista_chamados.html:86`; clique abre fio em `:42`. Resumo mostra `Decidir` em `_lista_resumo.html:46/:71` e abre fio `:53-54`. O fio mostra o verbo em `modal_fio.html:117-120` **antes** da guarda de ação em `:123`.

Rótulos e seus produtores/leitores:

- `catalogo/natureza.py:41-54` classifica por módulo + resposta não validada + pergunta viva; `:58-63` mapeia `sem_pergunta/cadastro → decidir`; não lê usuário/perfil.
- `services/verbos.py:44-47` lê módulo e créditos das perguntas; `:80-84` escreve descrição do verbo; `:149-158` gera rótulo da linha.
- `services/painel.py:383-388` escreve `verbo/rotulo_linha`; templates leem `_lista_chamados.html:86`, `_lista_resumo.html:71`.
- `services/fio.py:400` escreve conjunto de módulos com bloco próprio; `:403-407` chama `desenlace`; `services/desenlace.py:78-80` escreve “Decisão sua / Use os botões” sem conferir perfil nem condições particulares do bloco; template lê `modal_fio.html:117-120`.
- `services/fio.py:340-345` escreve `todas_respondidas`; template lê `modal_fio.html:334`.
- `services/fio.py:467` e `views.py:933` escrevem `pode_agir_map` consultando `pode_agir_chamado`; leituras no modal `:123/:500/:547/:563/:595/:636/:652`.
- `services/fio.py:470` e `views.py:936` escrevem `is_admin_ti` como `is_staff OR grupo admin_ti`; template lê `modal_fio.html:382`.
- `services/fio.py:474` escreve `pode_conferir_data` via `tem_acao('gerir_chamados')`; template lê `modal_fio.html:172`.
- `templatetags/chamados_tags.py:27-30` delega `pode_agir` a `pode_agir_chamado`; `:53-64` delega espera; `:67-78` delega opções de movimento. Este arquivo não decide o rótulo “Decidir”.

Universo AST:

- 16 motivos em `app/chamados/catalogo/motivos.py:34-49`.
- 34 módulos em `app/chamados/catalogo/modulos.py:105-198`.
- Módulo `gps_ausente` é registro (`natureza.py:18-23`), embora exista também motivo de pergunta homônimo (`motivos.py:42`).
- Módulo `gps_fora_geofence` é Validar (`natureza.py:29-31/:46-47`), não Decidir.

Script AST efetivamente executado, por stdin, sem importar o app:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import ast
from pathlib import Path
for name in ('motivos','modulos'):
 p=Path('app/chamados/catalogo')/(name+'.py')
 t=ast.parse(p.read_text())
 print(p)
 for n in ast.walk(t):
  if isinstance(n,ast.Assign) and any(isinstance(x,ast.Name) and x.id=='MOTIVOS' for x in n.targets):
   if isinstance(n.value,ast.Dict):
    for k in n.value.keys: print(k.value,k.lineno)
  elif isinstance(n,ast.AnnAssign) and isinstance(n.target,ast.Name) and n.target.id=='MOTIVOS':
   for c in n.value.elts: print(next(k.value.value for k in c.keywords if k.arg=='id'),c.lineno)
PY
```

Grep usado:

```bash
rg -n 'Decidir|DECIDIR|decidir' app/templates
rg -n 'pode_agir_map|is_gestor|is_admin_ti|proposta_viva|todas_respondidas' \
  app/chamados/views.py app/chamados/services/fio.py
rg -n 'desenlace\(|tem_bloco_proprio|rotulo_linha|rotulo_de_linha' \
  app/chamados/services/fio.py app/chamados/services/painel.py
rg -n 'modal_fio|ir_para_ausencia|detalhe_local' app/chamados/urls.py
```

## 11/11 20:29 · sessao 95b4102067cc · - · - · origem=Ronald (tmux)

Conclusão de leitura: **não há prova suficiente para atribuir a emissão da pk52340 a um sítio específico**. Os campos citados são compartilhados entre rotas de emissão: `instancia_marco=fim` deriva automaticamente do motivo; `batida_origem_id=None` é esperado para toda a família MARCO. (`app/chamados/models.py:3436`, `app/chamados/models.py:3452`, `app/chamados/models.py:3461`; mapa em `app/chamados/services/declaracao_texto.py:42`.)

Cadeias verificadas:

| Rota | Cadeia e referências |
|---|---|
| Detector contínuo | Cron a cada 5 minutos em `app/config/crons.py:364`; relógio local e passadas hoje/ontem em `app/ponto/management/commands/detectar_ausencias.py:70`, `:136`; procura **entrada** via `turnos_do_colab` em `:335`; emite chamado por `_abrir_chamado` em `:439`, `:693`; repassa chamados à fábrica em `:790`–`:802`. `_marcos_faltantes` usa explicitamente tipo `E`, produzindo normalmente `saida_sem_entrada`, em `:492`–`:497`. |
| Cartório | Cron 06:27 com `--apply`, por empresa, em `app/config/crons.py:616`; comando delega a `julgar_colab` em `app/ponto/management/commands/processar_cartorio.py:65`; cartório emite furo parcial com marcos nomeados em `app/ponto/services/cartorio.py:1805`, `:1815`; tradução dos apagados por `marco_para_motivo` em `:1915`–`:1922`. |
| Fábrica da célula | Após julgamento, `app/ponto/services/cartorio.py:1016`–`:1032` chama `perguntar_pela_celula`; usa teto de marcos vencidos em `app/chamados/services/fabrica_da_celula.py:35`–`:49`; lê ata e traduz posição/tipo em `:64`–`:73`; chama `criar_ou_atualizar_disputa` e `gerar_perguntas_disputa` em `:172`–`:174`; origem `julgamento_celula` em `:219`. |
| Batida → cartório | `post_save` da batida em `app/ponto/signals.py:131`–`:135`; dias afetados são data local e véspera em `:108`–`:112`. |
| Retroativo | `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88` lê `turnos_do_colab`; exige `aberto` em `:92`; exige idade ≥24h em `:100`–`:103`; cria chamado `turno_aberto_24h` em `:151`. Portanto esta rota **não deriva o par por conta própria**. |
| Alertas avançados | Cron a cada 30 minutos em `app/config/crons.py:363`; recomputa núcleo em `app/ponto/management/commands/processar_alertas_avancados.py:57`; lê turno materializado em `:58`; delega duração a `turno_aberto_ha` em `:62`–`:63`. |
| Guarda de virada na fábrica | `app/chamados/services/disputa_emissao.py:783` chama `_orfao_resolvido_cross_meianoite`; esta lê `turnos_do_colab` numa janela de D−2 a D+3 em `:707`–`:710`, exige turno completo em `:711` e limita a 26h em `:713`. |

`app/ponto/marcos.py:34` e `:50` **somente traduzem S no horário de fim para o rótulo `orfao_14h`**; não avaliam existência de par nem idade de 14h. As condições são `tipo=='S'` mais casamento de hora com marco/template. (`app/ponto/marcos.py:29`, `:33`, `:49`.)

A escrita comum da pergunta ocorre em `app/chamados/services/disputa_emissao.py:444`–`:452` ou via `_criar_pergunta_se_couber` em `:837`–`:845`; ambas chegam à porta `_nascer`, que instancia e salva em `app/chamados/models.py:3014`–`:3017`. Existe log de nascimento `pergunta_emitida` no código em `:3025`, mas o dado fornecido não contém esse evento nem horário de criação da pergunta. [nao verificado]

O repasse absorve perguntas de outros dias porque busca uma disputa aberta **somente por colaborador**, sem filtro de dia, em `app/chamados/services/disputa_emissao.py:1096`–`:1098`; se não houver, cria disputa com FK `chamado=chamado_pai` em `:1123`–`:1127`.

Cronologia relevante: `materializada_em` de pk52340 é 01/11 06:30:48; pk143189 foi criada às 07:00:33, **29m45s depois**. pk142873 já existia desde 31/10 19:00:40. Esses são valores da sonda sem arquivo local. [nao verificado] **`materializada_em` não é criação**: é o carimbo de resolução escrito em `app/chamados/models.py:3097`–`:3101`; a leitura que o usa para estado está em `:3046`. Portanto não se pode afirmar que o emissor deixou de ver uma saída já existente; tampouco se pode afirmar o instante exato da emissão. [nao verificado]

O campo `instancia_marco` é escrito no nascimento por `_chave_de_identidade` (`app/chamados/models.py:3420`, `:3436`–`:3440`) e lido nas chaves de busca em `:2753`; `batida_origem` é escrito em `:3448` e derivado em `:3452`, sendo nulo para MARCO em `:3461`; `motivo` e `data_evento` entram pela emissão em `app/chamados/services/disputa_emissao.py:446`, `:451`; `celula` entra na instância em `app/chamados/models.py:3014`; a consulta por célula/dia dos fatos ocorre em `app/chamados/services/fabrica_da_celula.py:145`–`:153`.

## 11/11 20:29 · sessao aff05e4e673a · - · - · origem=Ronald (tmux)

C7 exige **zero respostas sem veredito e zero perguntas vivas**; `pode_fechar()` faz essa conjunção em `app/chamados/models.py:1853`. A porta `fechar()` recusa primeiro respostas sem veredito (`:1886`–`:1894`), depois perguntas vivas no fecho ordinário (`:1895`–`:1903`), e grava o fechamento em `:1932`–`:1936`.

**Bloqueio demonstrável com ressalva explícita:** 51727 tem `respondida_em` preenchida e `validada_em/materializada_em=NULL` no dado recebido; sem tranca aplicável, satisfaz `respostas_sem_veredito()` e impede o fechamento inclusive pelas portas de exceção. A tranca não veio na sonda: o retorno real de `pode_fechar(7279)` continua **[nao verificado]**. Regra: `app/chamados/models.py:1843`–`:1846`; tranca: `app/chamados/juizes.py:974`–`:992`.

| Pergunta | Leitura do bloqueio por resposta |
|---|---|
| 47210 | Fora da lista porque há validação/materialização no dado. Regra `models.py:1843`; dado: [nao verificado]. |
| 51727 | Entra se o dia não estiver trancado; basta `respondida_em`, não é necessário conhecer o texto. Regra `models.py:1843`–`:1846`; dado: [nao verificado]. |
| 51823 | Fora por materialização preenchida. `models.py:1843`; dado: [nao verificado]. |
| 52340 | Fora por materialização preenchida. `models.py:1843`; dado: [nao verificado]. |
| 53458 | Fora por validação/materialização preenchidas, ainda que `veredito_lavrado=nao`. `models.py:1843`; dado: [nao verificado]. |
| 53628 | Fora por materialização preenchida. `models.py:1843`; dado: [nao verificado]. |

C6 também mantém 51727 viva se não houver tranca: não está validada; não tem via/materialização terminal; a célula 133670 acusa `furo` no dado, e o degrau da célula acusatória devolve `True`. A resposta com hora não elimina essa conclusão: `_resposta_concorda_com_a_luz` exige que o dia não acuse. Código: `app/chamados/juizes.py:1383`–`:1413`, `:1359`, `:1432`–`:1435`; dado: **[nao verificado]**.

Para 51823/52340/53628, `materializada_em` e célula concordante não bastam para reconstruir todo C6: faltam DNA/ata/lâmpadas e eventual tranca. O C6 verifica o marco e o teto provisório antes de absolver pela célula. `app/chamados/juizes.py:1428`–`:1447`. Vitalidade exata dessas três: **[nao verificado]**.

**Não confundir lavra persistida com C7.** `pode_fechar()` chama C6 atual; não lê `veredito_lavrado` nem `esta_stale`. `app/chamados/models.py:1852`–`:1853`; `app/chamados/juizes.py:862`–`:903`, `:1383`. A lavra é escrita por `veredito_lavrado.lavrar` em `app/chamados/veredito_lavrado.py:270`–`:275`; `esta_stale` lê lavra/data/insumos em `:224`–`:233`. Logo, o `nao` lavrado de 53458 não é sozinho bloqueio de C7.

**Chamadores diretos atuais de `DisputaSupervisao.fechar`, todos abertos e conferidos:**

| Arquivo:linha | Condição/contexto |
|---|---|
| `app/api/foto_recebida.py:80` | Foto recebida; valida perguntas antes em `:79`. |
| `app/chamados/reconciliador.py:412` | Sem turno aberto; C7 em `:411`. |
| `app/chamados/reconciliador.py:612` | Absolvição por célula; C7 em `:611`. |
| `app/chamados/reconciliador.py:1171` | Evento da célula; resposta sem veredito em `:1163`, C7 em `:1170`. |
| `app/chamados/models.py:1950` | Porta `admin_encerra`; exige motivo em `:1947`–`:1949`. |
| `app/chamados/models.py:2072` | Remoção do último motivo; `:2066`–`:2072`. |
| `app/colaboradores/services/desligamento.py:142` | Porta desligamento; resposta sem veredito impede em `:140`. |
| `app/ponto/services/entrada_sincronizada.py:59` | Dissolve pai de único órfão; C7 em `:58`. |
| `app/chamados/services/materializacao.py:591` | Ausência; C7 em `:590`. |
| `app/chamados/services/materializacao.py:1338` | `_fechar_disputa_se_quitada`; C7 em `:1321`. |
| `app/chamados/services/disputa_emissao.py:1284` | Autofecho do agrupador; C7 em `:1278`; veto ausência de ata em `:1279`. |
| `app/chamados/services/disputa_emissao.py:1964` | Chamado resolvido; respeita perguntas em voo em `:1961`; C7 em `:1963`. |
| `app/chamados/services/competencia_trancada.py:204` | Porta competência trancada; C7 em `:202`. |
| `app/chamados/services/becos.py:57` | Resolve destinos encerrados; C7 em `:56`. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88` | Comando; testa `fio_vivo` em `:83`, porta ainda pode recusar resposta sem veredito. |

São 15 chamadas diretas identificadas; a chamada `ch.fechar` de `fechar_chamados_meio_periodo.py:109` pertence a chamado, não DS. Receptores dinâmicos universais: **[nao verificado]**.

**Quem escreve e lê os campos necessários ao diagnóstico:**

| Campo | Escritores conferidos | Leitores decisórios |
|---|---|---|
| `respondida_em`, `resposta_colab` | Resposta `app/chamados/services/disputa_emissao.py:1467`–`:1468`, persistência `:1508`; carimbo pode preencher `respondida_em` em `app/chamados/models.py:3131`–`:3133`. Outros resets: censo principal. | `models.py:1844`–`:1845`; `juizes.py:1369`, `:1401`. |
| `validada_em` | Validação `app/chamados/services/validacao.py:134`, save `:137`; resposta nova limpa `disputa_emissao.py:1480`; carimbo pode preencher `models.py:3135`. Outros resets: censo principal. | `models.py:1843`; `juizes.py:1385`. |
| `materializada_em`, `via_resolucao` | Atribuição canônica em memória `models.py:3099`–`:3101`; saves `:3152`, `:3190`; descarimbo `:3208`, `:3212`. | `models.py:1843`; `juizes.py:1370`, `:1396`, `:1422`, `:1428`. |
| `fechada_em`, `resolucao`, `motivo_fechamento` | Fechar `models.py:1932`–`:1936`; admin reabre `:2149`–`:2161`; sistema reabre `:2173`–`:2177`. | Aberta `:1824`; porta/idempotência `:1879`–`:1883`; sinal `app/chamados/signals.py:28`, resolução `:45`. |
| `resolucao` adicional | **Sítio omitido em f0f1.r:** `limpar_resolucao_residual`, `models.py:2265`, save `:2266`, guarda `:2262`. | Mesma porta lê em `:2262`–`:2264`, trilha `:2271`. |

**Sobre “nunca fechou”:** a sonda demonstra aberta no instante recebido, mas o histórico completo não está provado pelo snapshot. `reabrir_sistema` limpa fechamento sem consumir os contadores admin/colab; portanto contadores zero não excluem fechamento seguido de reabertura automática. `app/chamados/models.py:2163`–`:2181`. “Nunca fechou” como sequência histórica: **[nao verificado]**.

**Relação com o chamado:** quando DS fecha, o receiver lê `fechada_em/chamado_id` em `app/chamados/signals.py:28`, busca o pai em `:32` e pede `reconciliar()` em `:42`; o encerramento do pai depende também desse gate. Uma DS permanecer aberta não prova, sozinha, toda a causa histórica do `em_analise`. Tentativas/erros efetivos no caso: **[nao verificado]**.

## 11/11 20:29 · sessao d3544cd60c5d · - · - · origem=Ronald (tmux)

Mapa para consolidar (todos caminhos relativos a `app/`):

| Campo | Escritor no HEAD | Leitor pertinente |
|---|---|---|
| `motivo`, `disputa` | nascimento `chamados/models.py:3014`; `disputa` chega como parâmetro da porta `:2776` | dedup lê motivo `models.py:2753`; identidade lê disputa/colaborador `:3430`; juiz lê `p.disputa.colaborador` em `chamados/juizes.py:697` |
| `data_evento` | único ponto de atribuição `chamados/services/fio.py:800`; nascimento chama em `models.py:3016`; dia normalizado antes em `:2875` | dedup `models.py:2753`; `_dia_do_fato` em `fio.py:1495`; plantio/juiz em `chamados/juizes.py:271` e `:296` |
| `celula`/`celula_id` | nascimento `models.py:3014`, escolhida por dia do fato em `:2896`; reuso `:2966`; rechaveamento `fio.py:1038` | guarda de célula mesma pessoa/dia em `chamados/juizes.py:651`–`:655` |
| `instancia_marco` | derivação do motivo em `models.py:3438`–`:3440`, recusa divergente em `:3441`–`:3447` | identidade/dedup `models.py:2753`; chave `fio.py:1460` |
| `batida_origem_id` | nascimento `models.py:2866`–`:2868`; `save` assegura em `:3448`–`:3449`; família MARCO retorna `None` em `:3460`–`:3462` | episódio no dedup `models.py:2717`–`:2724`; juiz do dia `fio.py:1487`–`:1493` |
| `respondida_em` | resposta `chamados/services/disputa_emissao.py:1468`; resposta automática `:1653`; reparo pela trilha `:1674`; `carimbar(resposta=...)` em `models.py:3132`; reabertura limpa em `disputa_emissao.py:1593` | estado em `models.py:3044`–`:3048`; propriedade `respondida` em `:3497`–`:3498` |
| `validada_em` | `_carimbar_validacao`, `chamados/services/validacao.py:134`; `carimbar(resposta='validar')` em `models.py:3135`; nova resposta limpa em `disputa_emissao.py:1480`; reabertura `:1593` | estado em `models.py:3041`; fila/materializador será coberto pelo outro explorador |
| `materializada_em`, `materializada_por_id`, `via_resolucao` | atribuição única `_escrever_carimbo`, `models.py:3099`–`:3101`; portas `carimbar` `:3129`/`:3152` e `carimbar_identidade` `:3182`–`:3190` | estado `models.py:3041`–`:3047`; juiz do lastro `chamados/juizes.py:679` e `:707` |
| criação da pergunta | `PerguntaDisputa` não declara `criado_em`: schema `models.py:2406`–`:3411`; INSERT em `:3017` e log `pergunta_emitida` em `:3025` | sonda não trouxe log nem campo de criação; instante de nascimento das quatro é **[nao verificado]** |

O carimbo `materializada_em` não significa necessariamente batida criada: identidade e absolvição também passam pelas portas do carimbo (`models.py:3168`–`:3190`); o juiz exige testemunha externa (`chamados/juizes.py:662`–`:669`). Não confundir os três instantes respondida/validada/carimbada com criação.

Dedup em banco para MARCO: `(colaborador, data_evento, motivo, instancia_marco)`, exclui conversa e `VIAS_FORA_DO_FATO`, no horizonte desde21/09 (`models.py:3482`–`:3487`; horizonte `:2403`). Consulta da porta usa mesmas colunas (`:2753`–`:2755`) e não inclui `disputa_id`. Portanto mudar disputa não deveria sozinho fazer segunda pergunta no mesmo dia/chave; mudar o dia cria chave diferente, permitida pelo índice. A porta normaliza pergunta MARCO sem episódio pelo dia recebido (`fio.py:1495`–`:1497`, `:1620`), logo o ponto único de gravação não corrige sozinho um produtor que lhe passe dia civil no lugar do dia do turno.

RED cabe precisamente em **`chamados/tests/test_plantio_noturno_mais1.py::OrfaoNoturnoTest`**, existente em `:75`–`:96`. A fixture `_Base` já monta escala12x36 e batidas (`:39`–`:60`), usa D=21/10, D1=22/10 (`:30`–`:32`) e relógio22/10 14h local (`:75`). Hoje os testes provam dia resolvidoD (`:78`–`:81`), plantioD1 (`:83`–`:88`) e contexto do detectorD (`:90`–`:96`), **sem verificar duas rotas emitindo a mesma pergunta**.

Extensão conceitual mínima: colNNN, 12x36 19–07, célulaD trabalho e D1 folga, uma E emD19h semS; chamar os dois caminhos reais de emissão para essa E e repetir; exigir exatamente uma pergunta `orfao_14h`, `data_evento=D`, `celula.data=D`, `instancia_marco='fim'`, `conversa=False`, mesma pk no reprocessamento. Mutante: o segundo produtor passa D1 (dia civil da saída esperada), fazendo nascer segunda pergunta; assert de contagem/dia falha. Não basta `objects.create` manualmente nem apenas executar seloAST: isso não demonstra cooperação dos emissores. Teste/fixture propostos, não criados nem executados **[nao verificado]**. [nao verificado]

## 11/11 20:29 · sessao 9ba100b232c2 · - · - · origem=Ronald (tmux)

Passivo e migração, só leitura:

- **147824:** a sonda mostra col407, instante **23/10/2026 07:00**, `origem=disputa_s84_retro`, `sequencia=1`, `is_intervalo=False`, `aprovada=True`, `aprovada_por_id=657`, aprovação/criação **06/11/2026 14:45:08**, sem canal/GPS/timestamp de aparelho/sincronização/retratação. Fonte fornecida não tem linhas locais: [nao verificado].
- **47210 materializou 147824:** consta literalmente no aval fornecido; os carimbos coincidem e a recusa registrada corresponde ao ramo de plantio por motivo sem marco (`materializacao.py:399-404`, `:851-856`). Entretanto a sonda **não imprime `Batida.pergunta_origem_id` nem `PerguntaDisputa.resposta_colab`**: vínculo por FK e resposta efetiva continuam [nao verificado].
- **Não há duplicata real de mesmo instante/tipo na lista fornecida:** em 23/10 aparecem 147824 às07:00 e135695 às19:00:34; afirmar inexistência fora desse recorte é [nao verificado]. Não confundir pergunta dupla para a saída de22/10 com batida duplicada: o plantio está em23/10 [nao verificado].
- `sequencia=1` **não determina E/S**: é inteiro independente; o tipo está em `Batida.tipo` (`app/ponto/models.py:23-26`). O TURNO raspado apresenta147824 como S [nao verificado].

**Quem escreve/lê os campos da origem**

| Campo | Escritor | Leitor |
|---|---|---|
| `timestamp`,`tipo` | cálculo único `_ts_tipo_do_plantio`, `app/chamados/services/materializacao.py:332`, `:343`, `:418`, `:425`; persistência `app/ponto/registro_batida.py:157-159` | dedup `app/ponto/registro_batida.py:107-122`; reconhecimento `app/chamados/juizes.py:268-279` |
| `origem`, aprovação/autor/data | chamador `app/chamados/services/materializacao.py:932-939`; persistência `app/ponto/registro_batida.py:157-159` | auditoria `app/chamados/services/materializacao.py:965-975`; definição de campos `app/ponto/models.py:45`, `:63-79` |
| `pergunta_origem` | `app/chamados/services/materializacao.py:951-955` via `anotar` | migração `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:138-139`; juiz `app/chamados/juizes.py:755-760` |
| `criado_em` | ORM `auto_now_add`, `app/ponto/models.py:79`, no create `app/ponto/registro_batida.py:157` | leitor original da sonda [nao verificado] |
| `materializada_em`, `materializada_por_id`, `via_resolucao` | `_escrever_carimbo`, `app/chamados/models.py:3097-3101`, chamado pela porta `:3128-3129` | migração estado `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:147-152`; reconhecimento `app/chamados/juizes.py:288`, `:690-707` |

**+1 no HEAD:** a implementação exige `data_evento` como dia do turno, escolhe somente dia e dia+1 pela cadeia real de marcos, e sem marcos conserva o dia recebido (`app/chamados/juizes.py:296-331`). O reconhecedor agora examina `(0,1)` (`app/chamados/juizes.py:271-279`). Isso comprova código presente; deploy desde07/11 vem do fato fornecido, não de Git nessa cópia [nao verificado].

**Apply09/11:** o registro local declara quatro vivas, zero materializadas, quatro migradas, zero recusas/erros e DRY seguinte zero. Lista exclusivamente pks50484,45495,41639,47164: **47199 e47210 não foram migradas nesse apply** (`app/docs/RELATO.md:612`). `RELATO.md` é registro, não execução/log acessado.

**Universo e motivo preciso de exclusão — cuidado com a ordem:**

1. Query exige motivo `orfao_14h`/`saida_sem_entrada`, `tipo_resposta='hora'`, data não nula e `conversa=False` (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:205-207`). `tipo_resposta` não foi raspado [nao verificado].
2. Se materializada, primeiro procura batida ligada e viva. Sem ela: `absorcao` apenas para ausência/declaracão; demais vias `sem_fonte` (`:65-79`). Portanto **47199 via `celula_absolveu` daria `sem_fonte` se não houver plantada ligada**; existência dessas FKs não raspada [nao verificado].
3. Com fonte encontrada, exige que `data_evento` seja o dia civil da saída do turno da fonte. Se não for, faz `continue`, **nem entra na lista de universo/FORA** (`:158-159`).
4. Se `47210 ->147824` por FK e turno da fonte é o turno23/10 mostrado, saída23/10 difere de `data_evento=22/10`: **fica fora do universo nessa guarda**, antes de `fora_do_aval` e colisão (`:158-172`). Premissa FK [nao verificado].
5. Se passasse as guardas anteriores e divergisse o dia, `orfao_14h` materializada cai em `fora_do_aval` **antes** de procurar colisão (`:168-172`). Logo não há base para afirmar que47210 ficou fora “por colisão com47199”.
6. A migração altera pergunta pela porta `rechavear_data_evento`, não batida (`:400-407`); não corrige passivo147824 (`app/docs/RELATO.md:612` lista as linhas efetivamente migradas).

**Dedup de batida no HEAD:** plantio primeiro reutiliza mesmo tipo em±20min (`app/chamados/services/materializacao.py:864`; `app/chamados/juizes.py:235-247`); chokepoint reutiliza mesmo tipo em±60s ou mesma chave exata `(colaborador,timestamp)` (`app/ponto/registro_batida.py:106-122`); constraint exata em `app/ponto/models.py:106`. Esses controles não provam retrospectivamente toda execução06/11 [nao verificado].

## 11/11 20:19 · sessao 1afb4ce14cf2 · LASTRO-FOLGA-FORA-DO-CATALOGO · censo · origem=Code

B1: a | pergunta: “(5) Segundo juiz? Ja existe autoridade de "tipo valido" (criar_ausencia :193-195, precedencia.faixa_de, TIPO_CHOICES) -- a guarda e LEITORA da mesma autoridade ou deriva em paralelo?” preverif_25ce.q:7 | resposta: “`faixa_de` responde outra pergunta, a precedência; seu default continua sendo `atestado` para código desconhecido” preverif_25ce.r:78 | motivo: reconheceu o default, mas não identificou o problema de faixa para tipo ativo fora do mapa. O tema estava ao alcance: `folga` estava cadastrada (app/docs/RELATO.md:7369), passa pela guarda (app/ponto/models.py:884), mas falta no mapa e recebe `atestado` (app/ponto/precedencia.py:52–64, app/ponto/precedencia.py:79). Sem trecho de triagem descartando esse achado.

B2: c | pergunta: “selo/RED sem mordida (os 5 testes novos da classe VoltaC2PreverifCodex mordem?)” preverif_c2.q:9 | resposta: “Isso não certifica pouso: RED/GREEN executados, suite inteira desta árvore e opus final permanecem **[nao verificado]**” preverif_c2.r:1 | motivo: a resposta delimitou a execução do RED como não verificada; a falha por fixture é informada pelo opus (opus_c2c_achados.txt:2), mas o conteúdo de `c2_red.log:53–98` não foi fornecido [nao verificado]. [nao verificado]

## 11/11 20:14 · sessao 9d3d6fd7c87c · FIO-E0-V4-E4B · construtor · origem=Code

Cura implementada: fontes históricas congeladas com SHA256; Git usado apenas na conferência explícita de host ([carregador:167](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c6_verificar.py:167)).

- `troca_atributo` derruba a assert sobre o código registrado ([teste:324](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c6_memoria.py:324)). [nao verificado]
- `arvore_atual` derruba a ausência de `LOAD_FAST confirmar_recusas_do_resolver` ([teste:325](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c6_memoria.py:325)). [nao verificado]
- `sem_restauro` derruba a restauração após erro ([teste:329](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c6_memoria.py:329)). [nao verificado]

Localmente: 31 testes verdes e seis mutantes mortos por assertions. [Evidências, censo e comandos individuais](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c10_entrega.txt:20). Container/suite inteira: [nao verificado]. [nao verificado]

Dentro de `/app` no container:

```bash
PYTHONDONTWRITEBYTECODE=1 python -m unittest chamados.tests.test_e4b_c6_memoria -v
```

Commit não executado pela proibição expressa de `git commit`/`.git`; [mensagem LEI-AKITA preparada](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c10_commit.txt:3). [nao verificado]

## 11/11 17:30 · sessao f796a81303b6 · TRIAGEM-OFFLINE-SEM-CAMERA · verificador · origem=Code

**APROVADO para o delta v6 do armazém:** o teto deixou de rejeitar, abortar ou fechar a conexão tardia; a conclusão da escrita permanece vinculada a `oncomplete`. Corrijo meu parecer v5: tratar o aborto temporizado como cura foi incorreto. (`app/static/pwa.js:71`, `app/static/pwa.js:81`, `app/static/pwa.js:95`, `app/static/pwa.js:99`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:672`)

**1. Desfechos do IndexedDB e quem os escreve**

| Estado | Comportamento do escritor e da tela |
|---|---|
| Abertura lenta | Após 2 s, `abrirDB` aciona o aviso; continua esperando. Se abrir, prossegue para a transação. (`app/static/pwa.js:71`, `app/static/pwa.js:81`, `app/static/pwa.js:275`) |
| `onblocked` | Aciona o aviso imediatamente, sem rejeitar. Pode prosseguir quando chegar `onsuccess`. (`app/static/pwa.js:81`, `app/static/pwa.js:83`) |
| Transação acima do teto | Mostra aviso e continua esperando; o timer não aborta nem resolve. (`app/static/pwa.js:95`, `app/static/pwa.js:99`) |
| `oncomplete` | Resolve a escrita; o `finally` remove o aviso; os templates apresentam sucesso. (`app/static/pwa.js:99`, `app/static/pwa.js:284`; `app/templates/ponto/triagem/apto.html:294`; `app/templates/ponto/bater_ponto_v2.html:339`) |
| `onabort` | Rejeita, remove o aviso e entrega o erro ao template. (`app/static/pwa.js:101`, `app/static/pwa.js:284`) |
| `onerror` | Rejeita imediatamente, sem aguardar `onabort`; remove o aviso e entrega o erro ao template. Portanto, esse retorno não comprova, sozinho, rollback concluído. (`app/static/pwa.js:100`, `app/static/pwa.js:101`, `app/static/pwa.js:284`) |
| Conexão aberta depois do teto | É aceita; não existe mais fechamento pelo atraso. (`app/static/pwa.js:71`, `app/static/pwa.js:81`) |
| Erro de abertura | `req.onerror` rejeita e chega ao mesmo tratamento visual de erro. (`app/static/pwa.js:82`, `app/static/pwa.js:284`) |
| Quota | Se aparecer como exceção síncrona ou erro da transação, rejeita. Quota real nesta cópia: **[nao verificado]**. (`app/static/pwa.js:98`, `app/static/pwa.js:100`, `app/static/pwa.js:102`) |
| Nenhum evento definitivo | A promessa permanece pendente indefinidamente, com aviso persistente. (`app/static/pwa.js:71`, `app/static/pwa.js:95`, `app/static/pwa.js:262`) |

O **aviso** tem emissor em `pwa.js`: cria o elemento, escreve o texto e o remove. O **sucesso/erro** é escrito pelos templates depois do `await`: triagem escreve “salva no celular” ou “Erro ao registrar”; v2 escreve sucesso ou o erro correspondente ao ramo. (`app/static/pwa.js:239`, `app/static/pwa.js:250`, `app/static/pwa.js:253`; `app/templates/ponto/triagem/apto.html:289`, `app/templates/ponto/triagem/apto.html:294`, `app/templates/ponto/triagem/apto.html:308`; `app/templates/ponto/bater_ponto_v2.html:323`, `app/templates/ponto/bater_ponto_v2.html:330`, `app/templates/ponto/bater_ponto_v2.html:342`)

**O botão não volta clicável em todos os desfechos.** No erro, triagem libera `_confirmandoOffline` e v2 remove `disabled`/libera `_enviando`. No sucesso, escondem os controles; triagem programa redirecionamento. Enquanto pendente, as guardas continuam impedindo outro escritor, mesmo com o botão visualmente presente na triagem. (`app/templates/ponto/triagem/apto.html:269`, `app/templates/ponto/triagem/apto.html:297`, `app/templates/ponto/triagem/apto.html:306`, `app/templates/ponto/triagem/apto.html:308`; `app/templates/ponto/bater_ponto_v2.html:294`, `app/templates/ponto/bater_ponto_v2.html:331`, `app/templates/ponto/bater_ponto_v2.html:341`)

No escritor isolado, não encontrei caminho demonstrado que anuncie sucesso sem `oncomplete`, nem o antigo erro provocado apenas por demora. Um `error` seguido de `complete` manteria a promessa rejeitada pela guarda `feito`; ocorrência desse encadeamento no IndexedDB real com estes chamadores: **[nao verificado]**. (`app/static/pwa.js:94`, `app/static/pwa.js:99`, `app/static/pwa.js:100`)

No caminho online, permanece outra incerteza: o POST pode ter gravado antes de a espera expirar; se a fila subsequente falhar, v2 apresenta erro. O código não confirma rollback no servidor; ocorrência real: **[nao verificado]**. (`app/static/pwa.js:299`; `app/templates/ponto/bater_ponto_v2.html:323`, `app/templates/ponto/bater_ponto_v2.html:330`)

**2. Tetos: números, origem e poder de decisão**

| Teto | Origem declarada e efeito |
|---|---|
| `ARMAZEM_TETO_MS = 2000` | Número reconhecido pelo teste como sem medição; agora decide somente o aviso. (`app/static/pwa.js:58`, `app/static/pwa.js:71`, `app/static/pwa.js:95`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:838`) |
| `HORA_TETO_MS = 2000` | O comentário remete ao relógio da v2. Aborta a consulta `/hora/`; nas operações de config, produz rejeição externa após o prazo, sem abortar a transação subjacente. (`app/static/pwa.js:111`, `app/static/pwa.js:120`, `app/static/pwa.js:133`, `app/static/pwa.js:135`, `app/static/pwa.js:152`) |
| `GAP_TETO_MS = 4000` | Derivado de `2 * HORA_TETO_MS`, preservando o valor numérico anterior. Decide abandonar a espera pelo gap e usar zero; também limita a espera do gap na carga. (`app/static/pwa.js:139`, `app/static/pwa.js:142`, `app/static/pwa.js:271`, `app/static/pwa.js:511`) |
| `SELFIE_TETO_MS = 1500`; abertura = 3000 | Limites declarados do juiz. Decidem retornar foto/stream ausente e aviso; a abertura tardia ainda pode ligar o vídeo. Medição justificadora desses números: **[nao verificado]**. (`app/static/pwa.js:183`, `app/static/pwa.js:209`, `app/static/pwa.js:216`, `app/static/pwa.js:231`) |
| `BATIDA_POST_TETO_MS = 30000` | Justificado no comentário pelo timeout da casca. Aborta o fetch e provoca recuperação pela fila; limita também a espera pelo corpo de erro. (`app/static/pwa.js:291`, `app/static/pwa.js:293`, `app/static/pwa.js:299`, `app/static/pwa.js:303`; `app/templates/ponto/bater_ponto_v2.html:321`) |

**Nenhum desses tetos aborta a escrita IndexedDB da batida.** Ainda existem decisões temporizadas sobre config, gap, câmera e POST. A transação de config pode continuar e gravar depois de seu chamador receber rejeição. (`app/static/pwa.js:95`, `app/static/pwa.js:112`, `app/static/pwa.js:114`, `app/static/pwa.js:116`, `app/static/pwa.js:271`, `app/static/pwa.js:299`)

A justificativa dos 30 s merece precisão: `CLAUDE.md` associa esse timeout ao `saas_ui`, mas declara `/api/ponto` roteado ao core. A aplicabilidade desse prazo ao endpoint efetivo permanece **[nao verificado]**. (`CLAUDE.md:89`; `app/static/pwa.js:291`)

Fora desses limites, os caminhos de GPS conservam esperas de 8 s na triagem/PWA e 10 s + 12 s na confirmação v2; terminada a tentativa, o fluxo prossegue. (`app/static/pwa.js:481`; `app/templates/ponto/triagem/apto.html:218`; `app/templates/ponto/bater_ponto_v2.html:280`, `app/templates/ponto/bater_ponto_v2.html:281`)

**3. Chamadores, contratos, escritores e leitores**

Os três chamados de produção encontrados ao escritor são triagem offline, v2 após falha do POST e v2 offline. Todos aguardam a promessa e ignoram seu valor: nenhum exige booleano, texto de erro ou rejeição obrigatória no teto. O escritor resolve sem retorno explícito. (`app/templates/ponto/triagem/apto.html:289`; `app/templates/ponto/bater_ponto_v2.html:323`, `app/templates/ponto/bater_ponto_v2.html:336`; `app/static/pwa.js:286`)

`abrirDB` atende escritor, config, sync e indicador. `_transacao` atende config e escrita da fila; sync/indicador continuam com transações próprias. Estes últimos chamam a abertura sem callback de aviso: podem esperar silenciosamente, mas não seguram a conclusão do escritor, que dispara o indicador sem `await`. (`app/static/pwa.js:113`, `app/static/pwa.js:123`, `app/static/pwa.js:275`, `app/static/pwa.js:286`, `app/static/pwa.js:310`, `app/static/pwa.js:368`)

`obterGap` é chamado pelo escritor e por `JulianiPWA.agora`; chama `lerConfig` e, quando necessário, `calcularESalvarGap`. Este último também roda na carga e chama `salvarConfig`. (`app/static/pwa.js:158`, `app/static/pwa.js:159`, `app/static/pwa.js:166`, `app/static/pwa.js:271`, `app/static/pwa.js:488`, `app/static/pwa.js:511`)

Sync/indicador são disparados na carga e nos eventos de conectividade; sync também dispara o indicador ao terminar. O SW apenas envia `SYNC_BATIDAS` às páginas, cujo listener chama sync; não recebe confirmação de conclusão. (`app/static/pwa.js:16`, `app/static/pwa.js:363`, `app/static/pwa.js:504`, `app/static/pwa.js:505`, `app/static/pwa.js:512`; `app/static/sw.js:116`, `app/static/sw.js:119`)

| Valor/campo | Quem escreve → quem lê |
|---|---|
| Fila e `id` | `abrirDB` cria store com chave automática; escritor faz `add` → sync lê/exclui por `id`; indicador conta. (`app/static/pwa.js:77`, `app/static/pwa.js:276`, `app/static/pwa.js:313`, `app/static/pwa.js:343`, `app/static/pwa.js:353`, `app/static/pwa.js:371`) |
| `config.chave/valor` | `salvarConfig.put` → `lerConfig.get`, que devolve `valor`. (`app/static/pwa.js:114`, `app/static/pwa.js:123`, `app/static/pwa.js:127`) |
| `ntp_gap` | Diferença calculada com hora do servidor e meia latência → recuperação de erro e `obterGap`. (`app/static/pwa.js:155`, `app/static/pwa.js:158`, `app/static/pwa.js:162`, `app/static/pwa.js:170`) |
| `ntp_calculado_em` | `Date.now()` após salvar gap → `obterGap`, com validade de 30 min. (`app/static/pwa.js:159`, `app/static/pwa.js:167`, `app/static/pwa.js:168`) |
| Instante, `timestamp`, `timestamp_dispositivo`, `timestamp_gap_ms` | Escritor captura um instante e grava corrigido, cru e gap → sync ordena pelo corrigido, mas envia somente o cru. (`app/static/pwa.js:270`, `app/static/pwa.js:277`, `app/static/pwa.js:279`, `app/static/pwa.js:318`, `app/static/pwa.js:328`) |
| Tipo, coordenadas, foto | Parâmetros das telas → fila → `FormData` do sync. (`app/static/pwa.js:277`, `app/static/pwa.js:281`, `app/static/pwa.js:325`, `app/static/pwa.js:333`) |
| `gps_accuracy`, `gps_captured_at` | Escritor guarda; não seguem no `FormData` desse sync. (`app/static/pwa.js:280`, `app/static/pwa.js:324`, `app/static/pwa.js:334`) |

A API lê o timestamp cru, aplica `hora_do_aparelho` e entrega instante efetivo e cru ao chokepoint. Portanto, o gap local não determina a hora gravada nesse caminho. (`app/api/views.py:519`, `app/api/views.py:666`, `app/api/views.py:678`)

O instante capturado continua sendo o da **entrada no escritor**, depois das esperas anteriores da tela; na recuperação online, depois do POST frustrado. A hora exibida no feedback é lida ainda depois da gravação. (`app/static/pwa.js:270`; `app/templates/ponto/bater_ponto_v2.html:323`, `app/templates/ponto/bater_ponto_v2.html:324`; `app/templates/ponto/triagem/apto.html:290`)

**4. Lei e juiz**

O delta remove a barreira temporal da fila e mantém a admissibilidade da câmera centralizada: ausência devolve aviso e foto nula, que os templates encaminham. Não encontrei segundo juiz da falta de selfie introduzido nessa mudança. (`CLAUDE.md:143`; `app/static/pwa.js:198`, `app/static/pwa.js:232`, `app/static/pwa.js:276`; `app/templates/ponto/triagem/apto.html:275`; `app/templates/ponto/bater_ponto_v2.html:274`)

No servidor, `selfie_ausente` decide a cobrança da ausência; esse ramo acrescenta motivo de anomalia, sem recusar a batida. A escrita segue para `registrar_batida`. Isso não certifica cumprimento universal da L-030: a própria documentação registra outras barreiras residuais. (`app/ponto/registro_batida.py:80`; `app/api/views.py:461`, `app/api/views.py:464`, `app/api/views.py:666`; `app/docs/LEIS.md:78`)

**5. Força e limites do teste** [nao verificado]

O atraso agora existe no falso IDB: transação lenta conclui após mais 15 s; abertura bloqueada libera após 15 s. O wrapper chama o escritor real e devolve sua promessa. O reclique ocorre em 12 s, e o caso lento verifica que caiu antes do término do escritor. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:161`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:216`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:309`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:510`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:689`)

Os casos distinguem falha, demora que termina e pendência; os mutantes restauram rejeição/aborto no teto ou retiram emissão/remoção do aviso. A propriedade troca 2000 por 10 e `1e9`, comparando desfechos finais. O controle precisa passar no mesmo lote do mutante. **Resultado executado nesta cópia: [nao verificado].** (`app/ponto/tests/test_selfie_nao_barra_chromium.py:489`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:516`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:639`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:845`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:1028`)

Há lacunas concretas:

- **Remover `abrirDB.req.onerror` é indistinguível nesses mundos:** o falso `open` nunca emite erro definitivo nem lança. (`app/static/pwa.js:82`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:209`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:676`)
- **Fechar a conexão antes de entregá-la também é indistinguível para o falso:** `close()` é vazio e `transaction()` não verifica fechamento. Isso deixa sem proteção específica a cura do fechamento tardio. (`app/static/pwa.js:81`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:226`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:236`)
- **A prova temporal é parcial:** os julgadores não comparam o instante do sucesso/término da promessa com `fila_ms`; a propriedade compara estados finais. Porém resolver incondicionalmente no sucesso do pedido seria detectado pelos mundos de aborto/erro/pendência; não afirmo que esse mutante simples passaria. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:489`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:516`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:705`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:833`)
- **“Persistidas” não mede durabilidade nativa:** `add` já põe a linha no mapa; o contador lê esse mapa. O falso de quota força rollback antes de emitir somente `onerror`. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:166`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:195`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:267`)

Essas são limitações da certificação, sem regressão correspondente demonstrada no código atual. Execução dos mutantes adicionais: **[nao verificado]**.

**6. Pendências da v5**

| Item | Situação na cópia |
|---|---|
| Aborto/rejeição por demora e fechamento tardio | Removidos do escritor; cobertura de fechamento ainda limitada pelo falso. (`app/static/pwa.js:81`, `app/static/pwa.js:95`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:236`) |
| Identidade do clique | Continua separada; a linha da fila não contém identificador original do clique. (`app/static/pwa.js:265`, `app/static/pwa.js:276`) |
| `DataTransfer` fora do `try` | Continua: uma exceção impede alcançar o submit, depois de desabilitar o botão. Ocorrência real: **[nao verificado]**. (`app/templates/ponto/triagem/apto.html:212`, `app/templates/ponto/triagem/apto.html:234`, `app/templates/ponto/triagem/apto.html:241`) |
| HTTP 400 no sync | Continua excluindo a fila com apenas `console.warn`. (`app/static/pwa.js:347`, `app/static/pwa.js:350`, `app/static/pwa.js:353`) |
| Fetch v2 sem PWA | Continua sem teto para fetch/corpo. (`app/templates/ponto/bater_ponto_v2.html:313`, `app/templates/ponto/bater_ponto_v2.html:314`) |
| Sync/indicador pendentes | Continuam com transações próprias sem proteção de aborto; abertura agora pode esperar sem aviso. (`app/static/pwa.js:310`, `app/static/pwa.js:312`, `app/static/pwa.js:368`, `app/static/pwa.js:370`) |
| TICKETS desatualizado e lacunas dos scanners | Permanecem. (`app/docs/TICKETS.md:693`; `app/tools/check_frontend_urls.py:25`; `app/scripts/check_a_lei_batida.py:14`, `app/scripts/check_a_lei_batida.py:31`) |
| “ArmazemEPostTest toca banco” | A alegação não se sustenta nesta leitura: herda `SimpleTestCase` e usa contexto sintético. Execução completa: **[nao verificado]**. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:385`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:579`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:781`) |

Suíte verde e smoke das duas cascas: **[nao verificado]**. Não executei o Chromium: o ensaio cria HTML/JS/perfil temporários e depois os apaga, contrariando a regra de somente leitura. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:398`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:405`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:417`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:426`)

## 11/11 17:29 · sessao c9a8c795d59f · PONTAS-SOLTAS-1725 · sonda · origem=Code

Não reexecutei a sonda. O envelope executa e raspa o resultado **antes** de entregá-lo ao Codex (`bin/codex_pergunta.sh:311`, `bin/codex_pergunta.sh:318`). Segue integralmente o dado recebido, com a única alteração solicitada: IDs como `colNNN`. Autenticidade por nova execução: [nao verificado].

```text
sonda_leitura: sonda_comp09_cel.py [0185702789f8352c] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

campo julgada_em linhas 7 colabs 3
  via cartorio 7
  min 11:10 6
  min 11:16 1
  amostra [(col899, datetime.date(2026, 8, 23), 'concorde', 'cartorio'), (col824, datetime.date(2026, 9, 3), 'furo', 'cartorio'), (col824, datetime.date(2026, 9, 5), 'furo', 'cartorio'), (col824, datetime.date(2026, 9, 13), 'furo', 'cartorio'), (col824, datetime.date(2026, 9, 18), 'fato_sem_previsao', 'cartorio'), (col824, datetime.date(2026, 9, 19), 'furo', 'cartorio'), (col909, datetime.date(2026, 8, 30), 'concorde', 'cartorio')]
campo veredito_em linhas 1 colabs 1
  via cartorio 1
  min 11:16 1
  amostra [(col909, datetime.date(2026, 8, 30), 'concorde', 'cartorio')]
fim
```

**Sim, segundo o dado fornecido**, há campos com horários dentro da janela consultada: `julgada_em` em **7 linhas**, de 3 colaboradores, todas com `veredito_via='cartorio'` — 6 às **11:10** e 1 às **11:16**. `veredito_em` aparece em **1 linha**, às **11:16**, também via `cartorio`; essa célula já integra as sete anteriores. Portanto, são **7 células distintas**, não oito. Fonte: dado fornecido; consulta e filtros do script [nao verificado].

Pelo código, os escritores são:

- **`julgada_em`:** `carimbar`, que atribui `agora` e persiste os campos (`app/ponto/portas/celula.py:1065`, `app/ponto/portas/celula.py:1072`).
- **`veredito_em`:** `lavrar_veredito`, que atribui `agora or timezone.now()` (`app/ponto/portas/celula.py:1037`). O serviço do cartório chama essa porta com `via='cartorio'`, `save=False`, e persiste a lavra junto do carimbo (`app/ponto/services/cartorio.py:1839`, `app/ponto/services/cartorio.py:1859`).

**Qual execução produziu essas sete linhas: [nao verificado].** `via='cartorio'` não identifica o disparador: o comando chama `julgar_colab`, enquanto chamadas de signals chegam ao mesmo serviço por `julgar_celula` (`app/ponto/management/commands/processar_cartorio.py:65`, `app/ponto/signals.py:93`, `app/chamados/signals.py:216`, `app/ponto/services/cartorio.py:2026`).

O cron está configurado para **06:27**, com `--apply`; sua execução efetiva nessa data/janela é [nao verificado] (`app/config/crons.py:616`). O comando captura **um único `agora` antes do lote** e o passa aos julgamentos; uma execução iniciada às 11:18–11:19, seguindo esse código, não atribui 11:10 ou 11:16 a esses campos (`app/ponto/management/commands/processar_cartorio.py:39`, `app/ponto/management/commands/processar_cartorio.py:65`). A execução manual mencionada e seus efeitos concretos são [nao verificado].

Os horários dos campos são o `agora` recebido pelo escritor; **o instante físico dos UPDATEs e sua autoria operacional nessa janela são [nao verificado]** (`app/ponto/portas/celula.py:1037`, `app/ponto/portas/celula.py:1065`).

_4013 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
