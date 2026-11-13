# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 18:21** · publicado em **13/11/2026 18:21** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 18:21 · sessao 9b191b42339e · - · - · origem=Ronald (tmux)

**COBPARC-FILA-RELOGIO**

1. **O que cura — LEU:** o mesmo lote pode julgar “fato novo” em instantes diferentes: fora do julgamento, `agora_deste_julgamento()` retorna `None`, e cada `houve_fato_novo(..., ate=None)` lê `now()` novamente. Sítios: `app/ponto/services/cartorio.py:82`, `app/ponto/services/cartorio.py:373`.

2. **Ainda existe: SIM — LEU.**
   - c2 passa o relógio potencialmente `None`: `app/chamados/reconciliador.py:877` e `app/chamados/reconciliador.py:884`.
   - cluster captura `None` uma vez, mas o predicado abaixo resolve esse `None` a cada dia: `app/ponto/services/cluster_espurio.py:164`, `app/ponto/services/cluster_espurio.py:166`, `app/ponto/services/cluster_espurio.py:169`, `app/ponto/services/cartorio.py:428`.
   - retenção chama `dia_na_lista_do_admin` sem instante, que pergunta `agora_deste_julgamento()`: `app/chamados/services/pergunta_no_app.py:144`, `app/chamados/services/disputa_emissao.py:940`.
   - reabertura consulta fato novo sem teto explícito: `app/chamados/models.py:1347`.
   - não há evidência de cura desses sítios por outra obra; commit de história não acessível nesta cópia [nao verificado]. [nao verificado]

3. **Falta e reaplicação.** BACKLOG registra mutantes_v4, suíte, opus e resposta do padrão; a alegação de “census 28 sítios a 0” pertence à raia e não comprova HEAD: `app/docs/BACKLOG.md:860`. **INFERIU:** reaproveitar a ideia exige adaptar a costura atual: o cron da raia que chamava `reconciliar_chamados_colab` foi substituído por vigia, cujo HEAD só classifica/conta (`app/chamados/management/commands/reconciliar_chamados.py:1`, `app/chamados/management/commands/reconciliar_chamados.py:75`); o laço de retratação foi extraído para `retratar_premissa_morta_colab`, inclusive chamado pelo consumidor do O-OUTBOX (`app/chamados/reconciliador.py:1446`, `app/chamados/reconciliador.py:1457`). Aplicação textual/merge real: [nao verificado].

   O anexo marca `main_moveu=True` nos seguintes arquivos — metadados de raia não verificáveis por Git nesta cópia [nao verificado]:
   - `app/chamados/management/commands/{disparar_perguntas_competencia,reabrir_orfas_enterradas,reconciliar_chamados,smoke_chamados}.py`;
   - `app/chamados/{models,reconciliador}.py`;
   - `app/chamados/services/{adesao,becos,competencia_trancada,disputa_emissao,fabrica_da_celula,pergunta_no_app}.py`;
   - `app/chamados/tests/test_selo_universo_reconciliador.py`;
   - `app/ponto/management/commands/{detectar_ausencias,detectar_cluster_espurio,processar_alertas_avancados,processar_alertas_turno,reconciliar_fantasmas,supra_juiz}.py`;
   - `app/ponto/services/{cartorio,cluster_espurio,geofence,justificativa}.py`;
   - `app/ponto/tests/test_o_outbox_celula.py`; `app/ponto/views.py`.

4. **Em comum, segundo anexos:** `app/chamados/models.py` com BUG-PAR, O168 e E3; `app/chamados/services/disputa_emissao.py` com BUG-PAR e E3. Nenhum com HJ-090. Comparação dos anexos, sem arquivo local com linhas [nao verificado].

5. **Depende de:** resposta do padrão, explicitamente aberta, e do estacionamento por aval; a própria célula diz que é fila e não trava L1 nem contrato 20: `app/docs/BACKLOG.md:860`. A regra de revisão enumera relógio fora da borda e exige suíte inteira antes do opus: `CLAUDE.md:374`, `CLAUDE.md:378`. Não há `!` do motor, exportada ou vínculo indicado nessa célula: `app/docs/BACKLOG.md:860`.

6. **PROPOSTA — INFERIU: CONCLUIR**, após estabilização de BUG-PAR/O168/E3 nos dois arquivos compartilhados e resposta do padrão; manter abaixo de obra que trave L1/contrato 20, conforme o estacionamento registrado (`app/docs/BACKLOG.md:860`). O defeito continua demonstrável nos sítios acima; o antigo caminho do cron precisa ser descartado da reaplicação em favor dos caminhos por evento do HEAD (`app/chamados/management/commands/reconciliar_chamados.py:6`, `app/chamados/reconciliador.py:1446`). Posição ordinal em fila global sem leitura das demais obras: [nao verificado].

## 13/11 18:16 · sessao f735193635be · RAIA-PREPARACAO · censo · origem=Code

**LEU** = código presente nesta cópia. **INFERIU** = consequência indicada a partir dele. Não consegui certificar o HEAD: a pasta não contém repositório Git consultável; o hash informado permanece **[nao verificado]**.

Os anexos abaixo são os textos da mensagem, não arquivos conferidos no HEAD **[nao verificado]**. Para referências: `complemento_hj_091.txt` começa em “aval Ronald” como linha 1; `passivo.r` começa em `sonda_leitura` como linha 1 do bloco. A linha HJ-091 não aparece nas quatro linhas do BACKLOG fornecido; sua redação completa permanece **[nao verificado]**.

**1. Censo AST: script, resultado e autoridades**

Script de análise, para executar por stdin com `python3 -B`; não importa Django nem a aplicação e não escreve arquivos. `--detalhe` imprime os candidatos por linha. As atribuições de tipo não demonstradas pelo AST continuam sendo candidatos, não escritores/leitores certificados. **[nao verificado: script entregue em memória, sem artefato no HEAD]**

```python
import ast
import sys
from pathlib import Path
from collections import Counter

TARGETS = {
    "ponto.services.ausencia": """
        criar_ausencia garantir_ausencia editar_ausencia_registro
        rejulgar_status_pelo_tipo mudar_estado decidir anexar_documento
        reverter_rejeicao _reabrir escalonar_aguardando_documento
        espelhar_ferias apagar_ausencias_de_ferias
        rejeitar_ausencias_de_ferias apagar_ausencias_em_ensaio
        rederivar_fim derivar_data_fim
    """.split(),
    "ponto.views": ["_a4_decidir"],
    "ferias.services": """
        _trocar_janela em_gozo_hoje ferias_na_janela
    """.split(),
    "ferias.models": ["status_por_data"],
    "ponto.turnos": """
        ausencia_cobre cobertura_ausencia_periodo dias_da_ausencia
    """.split(),
}
WANTED = {m + "." + n for m, ns in TARGETS.items() for n in ns}
FIELDS = set("""
    colaborador colaborador_id tipo data data_fim dias_corridos minutos
    descricao status documento documento_referencia periodo_aquisitivo
    data_inicio dias agendado_por aprovado_por aprovada_em atualizado_em
    agendamento_ferias agendamento_ferias_id
""".split())
ORIGINS = {
    "ponto.services.ausencia", "ponto.models", "ponto.views",
    "ferias.models", "ferias.services", "ferias.situacao", "folha.export",
}
calls, fields, models = set(), set(), set()
errors, files = [], 0

for root in ("app", "bin", "mensageria"):
    for p in sorted(Path(root).rglob("*.py")):
        if any(x in {"tests", "migrations", "__pycache__"} for x in p.parts):
            continue
        if p.name.startswith(("test_", "tests")):
            continue
        try:
            tree = ast.parse(p.read_text(encoding="utf-8"), filename=str(p))
        except (SyntaxError, UnicodeError) as e:
            errors.append((str(p), str(e)))
            continue
        files += 1
        parts = p.with_suffix("").parts
        module = ".".join(parts[1:] if root == "app" else parts)
        package = module.split(".")[:-1]

        class Scan(ast.NodeVisitor):
            def __init__(self):
                self.scope, self.env = [], [{}]

            def resolve(self, n):
                if isinstance(n, ast.Name):
                    return next(
                        (d[n.id] for d in reversed(self.env) if n.id in d),
                        n.id,
                    )
                if isinstance(n, ast.Attribute):
                    return self.resolve(n.value) + "." + n.attr
                if isinstance(n, ast.Call):
                    return self.resolve(n.func) + "()"
                return "?"

            def visit_ImportFrom(self, n):
                base = (
                    ".".join(package[:len(package)-n.level+1]
                             + ([n.module] if n.module else []))
                    if n.level else (n.module or "")
                )
                for a in n.names:
                    self.env[-1][a.asname or a.name] = base + "." + a.name

            def visit_Import(self, n):
                for a in n.names:
                    self.env[-1][a.asname or a.name.split(".")[0]] = (
                        a.name if a.asname else a.name.split(".")[0]
                    )

            def body(self, n):
                self.env.append({
                    x.name: module + "." + x.name for x in n.body
                    if isinstance(x, (
                        ast.FunctionDef, ast.AsyncFunctionDef, ast.ClassDef
                    ))
                })
                for x in n.body:
                    self.visit(x)
                self.env.pop()

            def visit_Module(self, n):
                self.body(n)

            def visit_ClassDef(self, n):
                self.scope.append(n.name)
                self.body(n)
                self.scope.pop()

            def visit_FunctionDef(self, n):
                self.scope.append(n.name)
                self.body(n)
                self.scope.pop()

            visit_AsyncFunctionDef = visit_FunctionDef

            def visit_Call(self, n):
                name = self.resolve(n.func)
                scope = ".".join(self.scope) or "<module>"
                if name in WANTED:
                    calls.add((str(p), n.lineno, scope, name))
                if name.startswith((
                    "ponto.models.Ausencia.objects.",
                    "ferias.models.AgendamentoFerias.objects.",
                )) or name in {
                    "ponto.models.Ausencia",
                    "ferias.models.AgendamentoFerias",
                }:
                    models.add((str(p), n.lineno, scope, name))
                if module in ORIGINS:
                    for k in n.keywords:
                        if k.arg and k.arg.split("__")[0] in FIELDS:
                            fields.add((
                                str(p), k.value.lineno, scope, "KW", k.arg
                            ))
                self.generic_visit(n)

            def visit_Attribute(self, n):
                if module in ORIGINS and n.attr in FIELDS:
                    fields.add((
                        str(p), n.lineno,
                        ".".join(self.scope) or "<module>",
                        type(n.ctx).__name__, n.attr,
                    ))
                self.generic_visit(n)

        Scan().visit(tree)

print("FILES", files, "PARSE_ERRORS", len(errors),
      "CALLS", len(calls), "MODEL_CALLS", len(models),
      "FIELD_CANDIDATES", len(fields))
for e in errors:
    print("ERROR", *e)
for name in sorted(WANTED):
    sites = [
        f"{p}:{line}::{scope}"
        for p, line, scope, target in sorted(calls) if target == name
    ]
    print("CALL", name, " | ".join(sites) or "0")
print("MODEL_FILES")
for p, n in sorted(Counter(r[0] for r in models).items()):
    print(p, n)
print("FIELD_COUNTS")
for p in sorted({r[0] for r in fields}):
    print(p, dict(sorted(Counter(
        r[3] for r in fields if r[0] == p
    ).items())))
if "--detalhe" in sys.argv:
    for row in sorted(models):
        print("MODEL", *row)
    for row in sorted(fields):
        print("FIELD", *row)
```

Resultado da execução resumida: **881 arquivos, 0 erros de parse, 109 chamadas diretas aos alvos, 208 chamadas sintáticas de modelo/gerente e 728 candidatos de campos**. Esses totais são da execução em memória, sem relatório persistido no HEAD **[nao verificado]**.

| Arquivo | Load | Store | Keywords |
|---|---:|---:|---:|
| `app/ponto/services/ausencia.py` | 81 | 13 | 55 |
| `app/ponto/models.py` | 51 | 4 | 8 |
| `app/ponto/views.py` | 98 | 6 | 162 |
| `app/ferias/models.py` | 61 | 1 | 15 |
| `app/ferias/services.py` | 27 | 15 | 52 |
| `app/ferias/situacao.py` | 14 | 0 | 12 |
| `app/folha/export.py` | 32 | 0 | 21 |

Contagens sintáticas da execução, **[nao verificado]** como artefato no HEAD. `KW` pode ser filtro, construção ou argumento de serviço; `Store` pode ser só alteração em memória. O script não prova alcance por despacho dinâmico, aliases por atribuição, SQL, templates ou chamadas indiretas; estes exigem leitura complementar **[nao verificado]**.

**LEU — escritores e caminhos de entrada confirmados**

| Escritor | Campos/operação e quem chega |
|---|---|
| `criar_ausencia` | Criação geral; também procura rejeitada por pessoa/tipo/data e pode reabri-la. Chamadores: web lançar/submeter, API, espelho, veredito, IA e materializadores. `app/ponto/services/ausencia.py:130`, `:254`, `:291`, `:385`; `app/ponto/views.py:2415`, `:2536`; `app/api/views.py:1805`; `app/ponto/services/ausencia.py:1203`; `app/ponto/services/veredito_celula.py:337`; `app/ponto/services/checar_ausencia.py:46`; `app/chamados/services/materializacao.py:491`, `:527`. |
| `garantir_ausencia` | Delega ao criador; chamados atuais de regularização/abandono. `app/ponto/services/ausencia.py:461`; `app/ponto/services/regularizacao.py:118`; `app/ponto/management/commands/marcar_abandono_provisorio.py:76`. |
| `editar_ausencia_registro` | Escreve tipo, data, fim, dias, descrição e minutos; salva e invalida fechamento. Entradas web e comando de rederivação. `app/ponto/services/ausencia.py:498`, `:509`, `:524`; `app/ponto/views.py:2794`; `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169`. |
| `mudar_estado` | Escreve estado/prazo e persiste derivados. Chamado por decisão, rejulgamento, anexo, escalonamento e reabertura. `app/ponto/services/ausencia.py:546`, `:825`, `:836`, `:875`, `:912`, `:1003`, `:1142`. |
| `decidir` / `_aplicar_periodo` | Estado, decisor, carimbo, descrição e horas parciais. Adaptador HTTP `_a4_decidir`. `app/ponto/services/ausencia.py:894`, `:912`, `:915`, `:918`, `:921`, `:928`; `app/ponto/views.py:85`. |
| `anexar_documento` | Documento; pode mudar estado. Web, reenvio API e API de documento. `app/ponto/services/ausencia.py:858`, `:860`, `:875`; `app/ponto/views.py:2151`; `app/api/views.py:1838`, `:3108`. |
| `reverter_rejeicao` / `_reabrir` | Documento, descrição, estado e prazo. Reversão HTTP e reabertura pelo criador. `app/ponto/views.py:2584`; `app/ponto/services/ausencia.py:291`, `:1103`, `:1129`, `:1139`, `:1142`, `:1147`. |
| `rejulgar_status_pelo_tipo` / escalonador | Estado/prazo; alcançam férias se estas entrarem nas seleções gerais. `app/ponto/services/ausencia.py:541`, `:546`, `:551`, `:993`, `:1003`; `app/ponto/management/commands/rejulgar_status_por_tipo.py:31`; `app/ponto/management/commands/escalonar_documentos_ausencia.py:26`. |
| `espelhar_ferias` | Cria férias diárias pelo criador; apaga selecionadas quando agendamento inativo. Chamado por `_sync_ausencia`. `app/ponto/services/ausencia.py:1189`, `:1191`, `:1203`; `app/ferias/models.py:392`. |
| Apagador/rejeitador de férias | Selecionam por pessoa/tipo/janela, sem origem. Entradas: `AgendamentoFerias.delete`, `_trocar_janela`, cancelamento administrativo. `app/ponto/services/ausencia.py:1263`, `:1274`, `:1284`; `app/ferias/models.py:455`; `app/ferias/services.py:445`, `:411`. |
| `rederivar_fim` / apagador de ensaio | Novo `save` geral ou exclusão do queryset recebido. `app/ponto/services/ausencia.py:1287`, `:1297`; `app/ponto/management/commands/curar_ausencia_sem_fim.py:46`; `app/ponto/management/commands/zerar_dados_operacionais.py:90`. |
| `Ausencia.save` | Carimba decisão e deriva minutos de falta, dias e fim. Não é guarda de proveniência. `app/ponto/models.py:872`, `:877`, `:894`, `:907`. |

As portas de **aprovar, rejeitar e cancelar**, individuais e em lote, chegam ao mesmo adaptador; editar período também. A reversão e a edição geral entram por outros serviços. Portanto, proteger somente `cancelar_ausencia` deixaria outros caminhos. **INFERIU** de `app/ponto/views.py:2290`, `:2319`, `:2351`, `:2643`, `:2673`, `:2701`, `:2734`, `:2584`, `:2794`.

Dois achados AST não significam produção efetiva de férias: os materializadores atuais recebem mapas de **falta/atestado**; a prévia normal da IA recusa férias. **LEU:** `app/chamados/models.py:3717`; `app/chamados/services/materializacao.py:475`, `:476`, `:603`; `app/chamados/services/disputa_emissao.py:1541`; `app/ponto/services/checar_ausencia.py:21`.

**LEU — leitores dos campos impactados**

| Campos/pergunta | Leitores e interpretação |
|---|---|
| Pessoa, tipo, data e fim | Cobertura do dia/período; distingue folha de cobrança. `app/ponto/turnos.py:2511`, `:2538`, `:2554`. |
| Estado | Folha exige aprovada; cobrança exclui rejeitada. `app/ponto/turnos.py:2517`, `:2519`. |
| Data/tipo/fim após escrita | Signals rejulgam células e reconciliam consumidores. `app/ponto/signals.py:123`, `:143`, `:150`, `:261`, `:271`. |
| Datas, dias, estado, documento e descrição | Histórico e painel. `app/ponto/services/dia_decidido.py:593`, `:615`, `:619`, `:624`; `app/ponto/views.py:2074`; `app/templates/ponto/partials/painel_ausencia.html:50`, `:57`, `:123`. |
| `documento_referencia` | Agrupamento documental, não identidade de agendamento. `app/ponto/views.py:2074`, `:2077`; `app/ponto/models.py:791`. |
| Agendamento: pessoa, datas e estado | `ferias_na_janela`, consumido por `em_gozo_hoje`, folha e disponibilidade. `app/ferias/services.py:47`, `:51`, `:63`; `app/folha/export.py:491`; `app/ponto/utils.py:198`. |
| Datas/estado do agendamento | Sincronização do espelho e classificação temporal. `app/ferias/models.py:273`, `:444`, `:450`. |
| FK nova | Não existe na definição de `Ausencia` lida; leitores/escritores futuros ainda não implementados. `app/ponto/models.py:743`, `:786`, `:791`, `:814`. |

O AST localizou consumidores das autoridades de cobertura em **API/espelho, calendário, grade, cartório, fechamento, efeito da ausência, chamados, vigias e relatórios**. Exemplos concretos: `app/api/views.py:1560`; `app/colaboradores/services/calendario.py:485`, `:533`; `app/escala/utils.py:1188`; `app/ponto/services/cartorio.py:1228`; `app/ponto/services/fechamento.py:683`; `app/ponto/services/efeito_ausencia.py:53`; `app/chamados/services/materializacao.py:79`; `app/ponto/services/vigia_ausencia.py:92`; `app/relatorios/views.py:205`, `:292`.

**Juízes versus derivações próprias**

- **Registrados no lookup:** cobertura por `ausencia_cobre/cobertura_ausencia_periodo`, abertura por `ABERTAS`, saldo/abono/vencimento e aviso. **Registro vivo:** acrescenta explicitamente efeito, veto, estado inicial e `em_gozo_hoje`. `app/docs/JUIZES.md:55`; `app/core/juizes.py:251`.
- **Derivação temporal na origem:** `status_por_data` se declara juiz do estado pelas datas; não está nomeado na entrada AUSENCIA/FERIAS consultada do lookup/registro. Não concluir daí autorização para criar outro. `app/ferias/models.py:273`; `app/docs/JUIZES.md:55`; `app/core/juizes.py:251`; `CLAUDE.md:74`.
- **Derivações próprias de identidade:** espelho trata presença na janela como sua; `_guard_sobreposicao` chama de `_minhas` todas as férias na janela antiga; apagador/rejeitador também deduzem propriedade pela janela. `app/ponto/services/ausencia.py:1183`, `:1200`, `:1267`, `:1282`; `app/ferias/models.py:422`, `:430`.
- **Derivação de apresentação:** painel calcula intervalo por irmãos documentais; histórico calcula duração do registro. Não demonstram período do agendamento. `app/templates/ponto/partials/painel_ausencia.html:50`; `app/ponto/services/dia_decidido.py:619`.
- **Leitor delegado correto:** férias com batida consulta `ferias_na_janela`; não necessita casar descrição de Ausencia. `app/folha/export.py:484`, `:491`.

O selo atual libera escritas pelo **arquivo da porta**, sem distinguir os escritores internos. Logo, não prova a exclusividade específica pedida pela HJ-091. **INFERIU** de `app/ponto/tests/test_chokepoint_ausencia_gate.py:29`, `:35`, `:65`; `app/ponto/services/ausencia.py:1203`.

**2. ESMERIL-SEMANTICA — decisões do construtor, sem decisão aqui**

| Campo/parâmetro | LEU / divergência | DECISÃO e opções de encaixe |
|---|---|---|
| Origem / nova FK | O espelho recebe um agendamento, mas reaproveita qualquer férias da pessoa no dia; `#N` é somente escrito. `app/ponto/services/ausencia.py:1183`, `:1200`, `:1207`. | Propagar identidade por parâmetro interno do produtor **ou** por caminho específico que preserve a porta. Não deduzir origem pela coincidência de janela. |
| `se_existir` / rejeitada | O criador pode retornar/reabrir por pessoa/tipo/data antes da criação. `app/ponto/services/ausencia.py:247`, `:254`, `:291`, `:297`. | Fazer a identidade atravessar essas buscas **ou** impedir que o caminho genérico alcance derivadas. RED: rejeitada de outro agendamento no mesmo dia. |
| Origem × unicidade | A unicidade viva é pessoa/tipo/data, sem agendamento. `app/ponto/models.py:827`. | Definir tradução da colisão pela porta: recusa com identificação/lista/Pauta existente. Acrescentar FK não autoriza duplicar férias vivas. |
| Estado de Ausencia × estado do agendamento | Espelho considera `agendado/em_gozo/concluido` ativo e pula inclusive Ausencia rejeitada; cobertura de folha exige aprovada. `app/ponto/services/ausencia.py:1181`, `:1200`; `app/ponto/turnos.py:2517`. | Separar presença, aprovação e origem nos contadores; escolher sua representação e denominadores. O passivo fornecido deve ser listado, sem reparo automático. Anexo `complemento_hj_091.txt:4`, `:6` **[nao verificado]**. |
| “Cancelar” × “rejeitar” × apagar | Tela cancela mediante rejeição; agendamento cancelado provoca exclusão das ausências selecionadas. `app/ponto/views.py:2734`; `app/ferias/services.py:411`, `:417`; `app/ponto/services/ausencia.py:1189`. | Centralizar operação por identidade no espelho **ou** manter auxiliar com origem explícita. Não igualar os vocabulários de estado. |
| Janela × propriedade / `exceto` | Limpeza usa data inicial da ausência dentro da janela; `exceto` preserva ambas as bordas da janela nova. `app/ponto/services/ausencia.py:1267`, `:1270`; `app/ferias/services.py:445`. | Acrescentar origem ao auxiliar **ou** delegar limpeza ao espelho. Linha deslocada fora da janela precisa ser denunciada; sobrevivência é **INFERIU** de `app/ponto/services/ausencia.py:1185`, `:1189`. |
| `_minhas` | A guarda exclui todas as férias da janela antiga como se fossem deste agendamento. `app/ferias/models.py:422`, `:430`, `:437`. | Alimentar o veto com IDs da FK **ou** mecanismo equivalente por identidade. Manter exclusão por janela conservaria a ambiguidade. |
| Data diária × período inteiro / documento | Espelho cria um dia, `dias_corridos=1`; painel reúne irmãos documentais. `app/ponto/services/ausencia.py:1204`; `app/ponto/views.py:2074`; `app/templates/ponto/partials/painel_ausencia.html:50`. | Definir apresentação do período pelo agendamento e do dia consultado separadamente; definir link específico ou abertura localizada na gestão. Não expandir o registro diário para produzir a aparência do período. |
| `data_fim=NULL` | Texto do campo sugere aberto; leitor distingue dia único de tipo em aberto. `app/ponto/models.py:751`; `app/ponto/turnos.py:3150`. | Texto específico de férias **ou** apresentação separada do registro e da origem; não interpretar NULL isoladamente. |
| Dias / inclusão | Agendamento e Ausencia usam datas inclusivas e dias corridos. Não li divergência de unidade nesta dupla. `app/ferias/models.py:380`; `app/ponto/models.py:899`; `app/ponto/services/ausencia.py:1196`. | Sem decisão de lei; preservar unidade e bordas na implementação. |
| Minutos | Espelho passa zero; duração prevista não deve ser fabricada para mostrar férias. `app/ponto/services/ausencia.py:1205`; `app/ponto/models.py:759`; `CLAUDE.md:148`. | Apresentar dias do agendamento; não converter em duração de jornada por conta própria. |
| Dia civil × dia do turno × instante | Folha cruza batida pela data local; ausência parcial noturna pertence ao início do turno; carimbo de aprovação é DateTime. `app/folha/export.py:496`; `app/ponto/models.py:767`, `:810`. | Declarar a unidade dos novos contadores como par agendamento/dia de ausência; não usar carimbo ou data civil da batida como substituto. |
| `hoje` / fuso | Modelos/serviços usam `date.today`; cron usa `timezone.localdate`. `app/ferias/model

[truncado em 20000 de 27724 caracteres]

## 13/11 18:16 · sessao 92e7cc643279 · RAIA-PREPARACAO · censo · origem=Code

**O diff antigo não pode ser retomado literalmente.** No HEAD, há três pontos adicionais: o leitor da ata transforma qualquer dispensa por justificativa em dia inteiro justificado; a cadeia temporal já vem do turno; e o abono persistido tem uma fonte diferente da soma parcial do PDF. (`app/escala/services/leitor_celula.py:81`, `app/escala/utils.py:1178`, `app/ponto/services/dia_pago.py:395`, `app/relatorios/pdf_espelho.py:479`)

LEU = código/documento aberto nesta cópia. INFERIU = consequência indicada das fontes. Hash, base `b264acbbc`, refs da raia e situação independente de produção: **[nao verificado]**. Os números raspados são evidência fornecida, sem confirmação independente: **[nao verificado]**.

**1. Censo AST: script e resultado**

Script executado por `python3 -B`, via entrada padrão, sem importar a aplicação. O instrumento é entregue abaixo, sem arquivo criado. Resultado instrumental, sem arquivo de prova: **[nao verificado]**.

```python
import ast
from pathlib import Path
from collections import defaultdict

FUNCS = set("""_marcar_ausencias marcos_cobertos_por_justificativa
minutos_do_periodo_justificado efeito_da_justificativa efeitos_da_ausencia
creditos_por_dia dispensar_por_ausencia por_dia_da_grade horas_reais_do_dia
periodo_da_parcial_no_dia encadear_marcos deslocamentos_na_cadeia
_justificado_lavrado""".split())
KEYS = set("""justificado justificativa_parcial marcos_justificados
min_justificados min_parcial min_parcial_pagos datas_justificadas
justificadas dispensada""".split())
STRUCT = set("""celulas marcos missing delta luz acesa tipo_dia orfas
minutos_previstos minutos_realizados cobertura_parcial periodo_parcial""".split())
STRUCT_FILES = {
    "app/escala/utils.py", "app/escala/services/leitor_celula.py",
    "app/ponto/turnos.py", "app/ponto/services/cartorio.py",
    "app/ponto/services/efeito_ausencia.py", "app/ponto/services/dia_pago.py",
    "app/ponto/services/fechamento.py", "app/relatorios/pdf_espelho.py",
    "app/ponto/motor_calculo_v2.py",
}
MODEL = set("""tipo status data_turno batida_entrada batida_entrada_id
saida_informada batida_gerada batida_gerada_id aprovada_em aprovada_por
criado_em atualizado_em chamado_origem""".split())
# Nomes locais conferidos por leitura; isto nao e inferencia global de tipos.
WRITERS = {
    "app/api/views.py": {"j"}, "app/ponto/views.py": {"j"},
    "app/ponto/services/regularizacao.py": {"j"},
    "app/ponto/services/justificativa.py": {"j"},
    "app/ponto/services/entrada_sincronizada.py": {"just"},
    "app/ponto/services/triagem_batida.py": {"just"},
    "app/chamados/reconciliador.py": {"_j"},
}
def string(n):
    return n.value if isinstance(n, ast.Constant) and isinstance(n.value, str) else None
def chain(n):
    if isinstance(n, ast.Name): return n.id
    if isinstance(n, ast.Attribute): return chain(n.value) + "." + n.attr
    if isinstance(n, ast.Call): return chain(n.func) + "()"
    return "?"
rows, errors, parsed, omitted = set(), [], 0, 0
for p in sorted(Path("app").rglob("*.py")):
    try: tree = ast.parse(p.read_text(encoding="utf-8"), filename=str(p))
    except (SyntaxError, UnicodeError) as e:
        errors.append((str(p), str(e))); continue
    parsed += 1
    if "tests" in p.parts or "migrations" in p.parts or p.name.startswith("test"):
        omitted += 1; continue
    active = KEYS | (STRUCT if str(p) in STRUCT_FILES else set())
    aliases = {a.asname or a.name: a.name for n in ast.walk(tree)
               if isinstance(n, ast.ImportFrom) for a in n.names}
    def add(key, mode, n):
        rows.add((key, mode, str(p), n.lineno))
    for n in ast.walk(tree):
        if isinstance(n, (ast.FunctionDef, ast.AsyncFunctionDef)) and n.name in FUNCS:
            add(n.name, "DEF", n)
        if isinstance(n, ast.Call):
            raw = n.func.id if isinstance(n.func, ast.Name) else getattr(n.func, "attr", "")
            f = aliases.get(raw, raw)
            if f in FUNCS: add(f, "CALL", n)
            if chain(n.func).startswith("Justificativa.objects."):
                add("Justificativa", "ORM:" + getattr(n.func, "attr", ""), n)
            if isinstance(n.func, ast.Attribute) and n.func.attr in {
                "get", "pop", "setdefault", "update", "values", "values_list"}:
                for a in n.args:
                    if string(a) in active: add(string(a), "KEY:" + n.func.attr, n)
            for kw in n.keywords:
                if (kw.arg or "").split("__")[0] in active:
                    add(kw.arg.split("__")[0], "KW:" + f, n)
        if isinstance(n, ast.Subscript) and string(n.slice) in active:
            add(string(n.slice), type(n.ctx).__name__, n)
        if isinstance(n, ast.Dict):
            for k in n.keys:
                if string(k) in active: add(string(k), "DICT", k)
        if isinstance(n, ast.Name) and n.id in active:
            add(n.id, type(n.ctx).__name__, n)
        if isinstance(n, ast.Attribute):
            if n.attr in active: add(n.attr, type(n.ctx).__name__, n)
            if (isinstance(n.ctx, ast.Store) and isinstance(n.value, ast.Name)
                    and n.value.id in WRITERS.get(str(p), set()) and n.attr in MODEL):
                add("Justificativa." + n.attr, "Store?", n)
group = defaultdict(lambda: defaultdict(list))
for key, mode, path, line in sorted(rows):
    group[key][path].append((line, mode))
print("PARSED", parsed, "OMITTED_TEST_MIGRATION", omitted,
      "ERRORS", errors, "ROWS", len(rows))
for key in sorted(FUNCS | KEYS | set(group)):
    print(key)
    for p, entries in sorted(group[key].items()):
        print(" ", p, " ".join(f"{m}:{l}" for l, m in sorted(entries)))
```

Saída: `PARSED 3369 OMITTED_TEST_MIGRATION 2608 ERRORS [] ROWS 640`. Houve um `SyntaxWarning` no teste `app/ponto/tests/test_tela_gestao_he_calendario.py:645`, sem erro de parse. Contagens da execução inline: **[nao verificado]**.

Limite: são ocorrências sintáticas, não 640 leitores/escritores semanticamente distintos. `DICT` pode ser modelo/rótulo; `Store?` depende dos nomes locais conferidos; aliases ORM, chamadas dinâmicas, templates e identidade interprocedural completa não são resolvidos. Completude fora desse contrato: **[nao verificado]**.

Resultado consolidado das funções:

| Função | Definição → chamadas encontradas |
|---|---|
| `_marcar_ausencias` | `app/escala/utils.py:1156` → mesmo arquivo: `807`, `1008`, `1848` |
| `encadear_marcos` | `app/escala/utils.py:330` → mesmo arquivo: `363`, `427`, `773`, `1619` |
| `deslocamentos_na_cadeia` | `app/escala/utils.py:366` → mesmo arquivo: `787`, `1752` |
| `horas_reais_do_dia` | `app/ponto/turnos.py:2646` → `app/escala/utils.py:1234` |
| `periodo_da_parcial_no_dia` | `app/ponto/turnos.py:2616` → `app/escala/utils.py:1234`, `app/escala/utils.py:1249`, `app/chamados/services/materializacao.py:83` |
| `dispensar_por_ausencia` | `app/ponto/services/cartorio.py:781` → mesmo arquivo: `1251`, `1437`, `1851` |
| `_justificado_lavrado` | `app/escala/services/leitor_celula.py:71` → mesmo arquivo: `439` |
| `efeitos_da_ausencia` | `app/ponto/services/efeito_ausencia.py:28` → `app/ponto/services/espelho.py:1068`, `app/relatorios/pdf_espelho.py:333`, instrumento `app/ponto/management/commands/diff_espelho_alimentacao.py:116` |
| `creditos_por_dia` | `app/ponto/services/credito_parcial.py:15` → `app/ponto/services/fechamento.py:999` |
| `por_dia_da_grade` | `app/ponto/services/dia_pago.py:364` → `app/ponto/services/fechamento.py:1061`, `app/ponto/services/dia_pago.py:464`, `app/folha/porta_export.py:139` |

O script não encontrou definições/chamadas de `marcos_cobertos_por_justificativa`, `minutos_do_periodo_justificado`, `efeito_da_justificativa`, nem ocorrências das chaves `marcos_justificados`, `min_justificados`, `justificativa_parcial`, no universo de produção examinado. Resultado negativo do instrumento: **[nao verificado: sem arquivo de saída]**. O nascimento autorizado do primeiro está escrito em `app/docs/CORTES.md:320`.

LEU — escritores e leitores do modelo:

| Campo | Escritores encontrados/conferidos | Leitores relevantes à obra |
|---|---|---|
| Criação, `data_turno`, `tipo`, âncoras | API `app/api/views.py:1992`; web `app/ponto/views.py:1635`; regularização `app/ponto/services/regularizacao.py:91`; cron `app/ponto/management/commands/processar_alertas_turno.py:394` | Grade `app/escala/utils.py:1197`; efeito `app/ponto/services/efeito_ausencia.py:41`; fechamento `app/ponto/services/fechamento.py:661` |
| Alteração de `tipo` | `app/api/views.py:1986`; `app/ponto/views.py:1625` | Os três leitores financeiros acima consultam aprovação/data e não selecionam o tipo. (`app/escala/utils.py:1199`, `app/ponto/services/efeito_ausencia.py:44`, `app/ponto/services/fechamento.py:664`) |
| `batida_entrada` | Criações acima; alteração em `app/ponto/views.py:1630` | Aprovação/materialização `app/ponto/services/justificativa.py:155`; bloqueio de órfão `app/ponto/utils_turno_orfao.py:53` |
| `saida_informada` | `app/ponto/views.py:1628`; `app/ponto/services/regularizacao.py:103`; `app/ponto/services/justificativa.py:382` | Validação e registro da saída `app/ponto/services/justificativa.py:179` |
| `batida_gerada` | `app/ponto/services/justificativa.py:204`, `381`; lote `app/ponto/views.py:1831` | Guarda de idempotência `app/ponto/services/justificativa.py:184` |
| `status` | Aprovação/rejeição/materialização `app/ponto/services/justificativa.py:205`, `283`, `383`; lote `app/ponto/views.py:1796`, `1875`; regularização `app/ponto/services/regularizacao.py:105`; autorrejeições `app/ponto/services/entrada_sincronizada.py:41`, `app/ponto/services/triagem_batida.py:608`, `app/chamados/reconciliador.py:533` | Grade/efeito/fechamento acima; SLA `app/ponto/management/commands/alertar_justificativas_sla.py:19` |
| `aprovada_por`, `aprovada_em` | `app/ponto/services/justificativa.py:206`, `284`, `384`; lote `app/ponto/views.py:1797`, `1876` | Carimbos de fato novo `app/ponto/services/cartorio.py:387` |
| `chamado_origem` | `app/api/views.py:1989`; `app/ponto/views.py:1632`; criação da regularização `app/ponto/services/regularizacao.py:91` | Encerramento pela origem `app/ponto/services/justificativa.py:217` |
| `criado_em`, `atualizado_em` | Automáticos no modelo `app/ponto/models.py:179` | Fato novo `app/ponto/services/cartorio.py:387` |

Outros leitores ORM encontrados: histórico em `app/chamados/views.py:596`; API em `app/api/views.py:2088`; contadores em `app/core/views.py:205`, `244`, `1191`; telas em `app/ponto/views.py:998`, `1673`, `1704`, `1736`, `1738`, `1742`; instrumento em `app/relatorios/management/commands/diff_extrato_parcial.py:43`. O comando existente de limpeza também referencia o modelo; foi apenas lido: `app/ponto/management/commands/zerar_dados_operacionais.py:44`, `95`.

LEU — estado derivado e dinheiro:

| Campo | Escritor/origem | Leitores/reescritores |
|---|---|---|
| `celulas`, `missing`, `delta` | Casamento em `app/escala/utils.py:585`; delta absoluto em `app/escala/utils.py:325`; overlay da justificativa em `app/escala/utils.py:1256` | Ata `app/ponto/services/cartorio.py:721`; reconstrução `app/escala/services/leitor_celula.py:121`; horas reais `app/ponto/turnos.py:2660` |
| `justificado` | Grade escreve `True` em `app/escala/utils.py:1259` | Cartório `app/ponto/services/cartorio.py:802`; reconstrução escreve `True` em `app/escala/services/leitor_celula.py:440`; supra retira `FURO_*` em `app/ponto/supra_juiz.py:371` |
| `acesa`, `luz`, `dispensada` | Ata em `app/ponto/services/cartorio.py:711`, `722`; dispensa em `app/ponto/services/cartorio.py:808` | Leitor `app/escala/services/leitor_celula.py:119`, `145`; lastro `app/ponto/services/lastro.py:336`; status `app/ponto/status_dia.py:187`; supra `app/ponto/supra_juiz.py:312`; furos `app/relatorios/furos.py:32` |
| `cobertura_parcial`, `periodo_parcial` | Grade `app/escala/utils.py:1230`, `1234`; ata transporta em `app/ponto/services/cartorio.py:766` | Leitor transporta em `app/escala/services/leitor_celula.py:433`; guarda antirretroativo `app/ponto/services/cartorio.py:424` |
| `justificadas` / `datas_justificadas` | Efeito consulta datas em `app/ponto/services/efeito_ausencia.py:40`; fechamento repete consulta em `app/ponto/services/fechamento.py:660` | PDF `app/relatorios/pdf_espelho.py:334`; espelho `app/ponto/services/espelho.py:1114`; motor `app/ponto/motor_calculo_v2.py:796`, `2602`, `2608`, `3466` |
| `min_parcial` / `min_parcial_pagos` | Soma somente Ausência em `app/ponto/services/efeito_ausencia.py:46`, `50`, `64` | Leitura da chave em `app/relatorios/pdf_espelho.py:336`; soma em horas em `app/relatorios/pdf_espelho.py:479` |
| Crédito diário de atraso | `app/ponto/services/credito_parcial.py:15` | Fechamento aplica em `app/ponto/services/fechamento.py:999`; abatimento em `app/ponto/services/credito_parcial.py:50` |
| Abono persistido | `app/ponto/services/dia_pago.py:395`; fechamento grava em `app/ponto/services/fechamento.py:1062` | Resumo da competência lê em `app/relatorios/cartao_pela_celula.py:118`, `186`; retrato exportado chama a fonte em `app/ponto/services/dia_pago.py:464` |

**Juízes registrados versus derivações próprias — LEU:**

- Turno/marcos, casamento, cálculo/CCT e janela estão no lookup: `app/docs/JUIZES.md:20`, `22`, `27`, `29`, `35`. Cobertura de Ausência está em `app/docs/JUIZES.md:55`; período parcial está no registro vivo em `app/core/juizes.py:261`.
- `marcos_cobertos_por_justificativa` tem corte de nascimento, mas não está no bloco vivo de Ausência lido nesta cópia. (`app/docs/CORTES.md:320`, `app/core/juizes.py:251`)
- Derivam por conta própria: a grade considera qualquer data aprovada suficiente para limpar o dia; efeito e fechamento repetem a seleção de datas; o leitor da ata deriva dia inteiro por `any(dispensada)`; `_delta_min` recalcula por HH:MM; o PDF acrescenta localmente o parcial ao abono. (`app/escala/utils.py:1197`, `1256`; `app/ponto/services/efeito_ausencia.py:40`; `app/ponto/services/fechamento.py:660`; `app/escala/services/leitor_celula.py:32`, `81`; `app/relatorios/pdf_espelho.py:479`)
- `por_dia_da_grade` é fonte compartilhada de abono/previsto/realizado; seus chamadores não repetem essa conta. Isso está declarado e implementado em `app/ponto/services/dia_pago.py:364`, `395`, com consumidores em `app/ponto/services/fechamento.py:1061` e `app/folha/porta_export.py:139`.

**2. ESMERIL-SEMANTICA: decisões para o construtor**

As opções abaixo são alternativas de contrato/encaixe; nenhuma foi escolhida neste censo.

| Campo/parâmetro | LEU: significados divergentes | DECISÃO a explicitar |
|---|---|---|
| `data_turno` | Modelo diz dia do turno; web procura entrada pelo dia civil; regularização usa data local da entrada; API recebe payload. (`app/ponto/models.py:144`, `app/ponto/views.py:1598`, `app/ponto/services/regularizacao.py:73`, `app/api/views.py:1948`) | Transportar identidade do turno existente ou converter a data na fronteira usando o juiz. A regra do dia de início já está respondida por L-085. (`app/docs/LEIS.md:117`) |
| `tipo` e papel E/S | Tipo da justificativa nomeia problema; papel da batida deve vir do marco, não de `Batida.tipo`. (`app/ponto/models.py:116`, `app/docs/LEIS.md:119`) | Representar cobertura por instância do marco ou por chave estável do marco; preservar o papel dado pela autoridade. |
| `batida_entrada`, `chamado_origem` | Modelo vincula ao turno/episódio; o diff fornecido reduz a `{data: tipos}`. (`app/ponto/models.py:139`, `173`; algoritmo do anexo **[nao verificado no HEAD]**) | Alcance por dia/tipo ou por registro ancorado: lacuna 1 abaixo. |
| `saida_informada`, `batida_gerada`, `status` | Aprovação pode materializar uma saída real, enquanto a cobertura pretendida dispensa marco sem batida. (`app/ponto/services/justificativa.py:179`, `204`, `205`; `app/ponto/services/cartorio.py:808`) | Calcular efeito após o estado materializado ou excluir explicitamente o trecho já realizado; a representação precisa impedir pagamento duplicado. |
| `marcos`, índices e deslocamentos | Régua de colunas não é ordem cronológica do turno noturno; o diff reencadeia a régua. (`app/escala/utils.py:353`, `366`; algoritmo do anexo **[nao verificado no HEAD]**) | Carregar cadeia/instâncias ou deslocamentos já fornecidos pelo builder. Reencadear colunas restaura o defeito documentado. (`app/ponto/turnos.py:2655`) |
| `delta` | Grade: diferença de instantes absolutos, arredondada; leitor: menor distância circular de HH:MM, limitada por ±720. (`app/escala/utils.py:325`, `app/escala/services/leitor_celula.py:32`) | Transportar delta/proveniência lavrada ou restringir e verificar formalmente o domínio da reconstrução. Divergência efetiva na frota: **[nao verificado]**. |
| `None`, `missing`, `dispensada`, `luz` | `None` pode significar coluna fora do jogo; dispensa tem `acesa=True`, `luz=None`; batida real tem hora. (`app/ponto/services/cartorio.py:719`, `786`; `app/escala/services/leitor_celula.py:145`) | Cobertura explícita por marco ou marcador equivalente já transportável; não usar “acesa” como prova de batida. |
| `justificado` | Grade atual significa dia todo; leitor reconstruído usa qualquer dispensa. (`app/escala/utils.py:1259`, `app/escala/services/leitor_celula.py:81`) | Carimbo integral explícito ou composição integral na autoridade. **INFERIU:** dispensar um único marco com o leitor atual calaria o furo residual. (`app/escala/services/leitor_celula.py:440`, `app/ponto/supra_juiz.py:371`) |
| `min_justificados` / `min_parcial_pagos` | Raia propõe duração de período; HEAD fornece escalar de minutos de Ausência para o PDF. (anexo **[nao verificado no HEAD]**; `app/ponto/services/efeito_ausencia.py:64`) | Mapa por dia com período/proveniência ou mapa diário de minutos com perdas de informação declaradas; escalar mensal sozinho não identifica o dia do DSR. |
| Parcial pago versus crédito de atraso | Crédito abate atraso apenas do próprio dia; PDF trata total como abono. (`app/ponto/services/credito_parcial.py:50`, `app/relatorios/pdf_espelho.py:479`) | Compor efeitos distintos a partir da mesma cobertura ou ampliar o contrato diário existente; não equiparar implicitamente período pago a atraso. |
| `justificadas` versus abono monetário | Set de datas afasta falta inteira; motor só abona dia sem trabalho; folha abona complemento em dia de Ausência. (`app/ponto/motor_calculo_v2.py:2600`, `2606`; `app/ponto/services/dia_pago.py:395`) | Entrada comum que distinga integral/parcial ou canais explícitos consumindo a mesma origem. Acrescentar uma data ao set não representa duração parcial. |
| Minutos versus horas | Grade/fechamento usam minutos; motor/PDF expõem horas com `/60`. (`app/ponto/services/dia_pago.py:367`, `app/ponto/motor_calculo_v2.py:2618`, `app/relatorios/pdf_espelho.py:479`) | Fixar a conversão numa fronteira ou manter tipos/unidades explícitos em cada contrato; medir arredondamento. O recorte fornecido não contém segundos da batida 139122. **[nao verificado]** |
| Zero versus período ilegível | HEAD admite `None`/`nao_sei`; raia devolve zero quando não consegue ler delta. (`app/ponto/turnos.py:2624`, `2663`; anexo **[nao verificado no HEAD]**) | Resultado com valor e estado de conhecimento ou recusa explícita de apuração; zero não pode esconder ausência de proveniência. |
| Fuso e dia do DSR/crédito | Banco UTC/apresentação UTC-3; DSR base usa dia da jornada, override comercial e crédito usam data local da entrada. (`CLAUDE.md:146`, `app/ponto/motor_calculo_v2.py:2577`, `3465`, `app/ponto/services/credito_parcial.py:36`) | Transportar a chave soberana ou convertê-la na fronteira. Dia da jornada já está definido; não é pergunta de lei. (`app/docs/LEIS.md:117`) |
| Bordas temporais | Parcial usa `[ini,fim]` para pertinência; fato novo usa `(desde,até]`; batidas da sonda interna usam `[D−1,D+2)`; janelas de datas financeiras usam limites inclusivos. (`app/ponto/turnos.py:2641`, `app/ponto/services/cartorio.py:376`, `380`, `app/ponto/services/efeito_ausencia.py:43`) | Documentar separadamente pertinência de p

[truncado em 20000 de 27756 caracteres]

## 13/11 18:15 · sessao 89abfd41e1c4 · RAIA-PREPARACAO · censo · origem=Code

**perguntas de lei: 1 — alcance temporal da medida de 24 horas.** O contrato define início/fim das corridas, mas não escolhe qual deles determina a pertença à janela. (`app/docs/RELATO.md:1648`)

**LEU:** OTIMIZA-1 permanece interrompida; o complemento exige pós-E3, DIFF do vigia zero e instrumento RSS separado. A preparação autoriza somente leitura/desenho. (`app/docs/BACKLOG.md:2083`; `app/docs/BACKLOG.md:738`; `app/docs/BACKLOG.md:739`)

Nesta resposta, **HEAD** significa os arquivos disponíveis nesta cópia. `.git` não está presente; identidade do commit, base `4d5fd8df6` e blobs integrais da raia: **[nao verificado]**. A construção histórica está documentada, sem comprovar implementação nesta cópia. (`app/docs/RELATO.md:1645`)

**1. Censo AST: script e resultado**

Script executado em memória, sem importar o app. `W` identifica construção/atribuição sintática; não prova escrita persistente. Importações relativas, referências por strings, despacho dinâmico e identidade dos objetos exigem conferência manual. **[nao verificado: completude para chamadas dinâmicas]**

```python
import ast, sys
from pathlib import Path
from collections import defaultdict

F = set("""
bin.gerar_estado.gerar
chamados.management.commands.vigiar_fio_morto._carimbar
chamados.management.commands.vigiar_fio_morto.caminho_do_carimbo
chamados.services.vigia_fio.censo_alcance
chamados.services.tarefa_do_dia.carga_da_tarefa
chamados.services.tarefa_do_dia.tarefa_do_dia
chamados.juizes.alimentar_portas
chamados.juizes.alimentar_fila
chamados.juizes.porta_alcancavel_do_dia
""".split())
K = set("""
medido_em hoje esperado censo alcance gritou alarma_em_0
fila_ids fila_cel verbo vivas instancias no_app
_pares _portas agora ini fim pico_rss_mb inteiras_por_pouso
""".split())
P = set("""
bin/fio_estado.py
app/core/management/commands/pipeline_placar.py
app/chamados/management/commands/vigiar_fio_morto.py
app/chamados/services/vigia_fio.py
app/chamados/services/tarefa_do_dia.py
app/chamados/juizes.py
""".split())
A = """
.git bin/pista_estado.py bin/suite_impacto.py
bin/suite_impacto.sh bin/suite_impacto
""".split()

out = defaultdict(set)
errors = []
total = 0
for p in sorted(Path(".").rglob("*.py")):
    rel = p.as_posix()
    mod = rel[:-3].replace("/", ".")
    if mod.startswith("app."):
        mod = mod[4:]
    try:
        tree = ast.parse(p.read_text(encoding="utf-8"), filename=rel)
    except (OSError, UnicodeError, SyntaxError) as e:
        errors.append((rel, type(e).__name__))
        continue
    total += 1
    aliases = {}
    for n in ast.walk(tree):
        if isinstance(n, ast.ImportFrom) and n.module:
            for a in n.names:
                aliases[a.asname or a.name] = n.module + "." + a.name
        elif isinstance(n, ast.Import):
            for a in n.names:
                aliases[a.asname or a.name.split(".")[0]] = (
                    a.name if a.asname else a.name.split(".")[0]
                )

    def resolve(n):
        if isinstance(n, ast.Name):
            return aliases.get(n.id, mod + "." + n.id)
        if isinstance(n, ast.Attribute):
            return resolve(n.value) + "." + n.attr
        return ""

    def add(kind, key, n):
        out[(kind, key)].add(f"{rel}:{n.lineno}")

    for n in ast.walk(tree):
        if isinstance(n, (ast.FunctionDef, ast.AsyncFunctionDef)):
            if mod + "." + n.name in F:
                add("DEF", mod + "." + n.name, n)
        if isinstance(n, ast.Call) and resolve(n.func) in F:
            add("CALL", resolve(n.func), n)
        if rel not in P:
            continue
        if isinstance(n, ast.Dict):
            for k in n.keys:
                if isinstance(k, ast.Constant) and k.value in K:
                    add("W", k.value, k)
        if isinstance(n, ast.keyword) and n.arg in K:
            add("KW", n.arg, n)
        if isinstance(n, ast.arg) and n.arg in K:
            add("PARAM", n.arg, n)
        if isinstance(n, ast.Attribute) and n.attr in K:
            add("R" if isinstance(n.ctx, ast.Load) else "W", n.attr, n)
        if isinstance(n, ast.Subscript):
            if isinstance(n.slice, ast.Constant) and n.slice.value in K:
                add("R" if isinstance(n.ctx, ast.Load) else "W",
                    n.slice.value, n)
        if isinstance(n, ast.Call) and isinstance(n.func, ast.Attribute):
            if n.func.attr in ("get", "pop", "setdefault") and n.args:
                k = n.args[0]
                if isinstance(k, ast.Constant) and k.value in K:
                    add(n.func.attr, k.value, n)

print("PARSED", total, "ERRORS", errors)
for a in A:
    print("EXISTS", a, Path(a).exists())
for (kind, key), sites in sorted(out.items()):
    if kind in ("DEF", "CALL") or "--sites" in sys.argv:
        print(kind, key, "=>", "; ".join(sorted(sites)))
for k in sorted(K):
    print("FIELD", k, {
        t: len(out[(t, k)])
        for t in ("W", "R", "get", "pop", "setdefault", "KW", "PARAM")
        if out[(t, k)]
    })
```

Resultado operacional: `PARSED 3574 ERRORS []`; os cinco caminhos de `A` retornaram `False`. As chaves candidatas `pico_rss_mb` e `inteiras_por_pouso` não apareceram no recorte `P`. **[nao verificado: saída transitória, sem arquivo gravado pela restrição de leitura]**

Chamadas encontradas fora dos testes — os sítios abaixo foram conferidos no código: [nao verificado]

| Função | Definição | Chamadores |
|---|---|---|
| `gerar` | `bin/gerar_estado.py:178` | `bin/gerar_estado.py:396` |
| `_carimbar` | `app/chamados/management/commands/vigiar_fio_morto.py:120` | Mesmo arquivo, `:232` |
| `caminho_do_carimbo` | `app/chamados/management/commands/vigiar_fio_morto.py:107` | Mesmo arquivo, `:132`; `app/core/management/commands/pipeline_placar.py:92` |
| `censo_alcance` | `app/chamados/services/vigia_fio.py:542` | `app/chamados/management/commands/vigiar_fio_morto.py:178`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:78` |
| `carga_da_tarefa` | `app/chamados/services/tarefa_do_dia.py:557` | Mesmo arquivo, `:569`; `app/chamados/management/commands/diff_e3_sombra.py:115` |
| `tarefa_do_dia` | `app/chamados/services/tarefa_do_dia.py:562` | `app/chamados/management/commands/diff_e3_sombra.py:125`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:87` |
| `alimentar_fila` | `app/chamados/juizes.py:1531` | Mesmo arquivo, `:1576`; `app/chamados/services/tarefa_do_dia.py:529` |
| `alimentar_portas` | `app/chamados/juizes.py:1552` | Mesmo arquivo, `:1656`; `app/chamados/services/vigia_fio.py:609`; `app/chamados/management/commands/diff_e3_sombra.py:114`; `app/escala/services/cadastro_realidade.py:456`; `app/folha/fora_da_folha.py:62` |
| `porta_alcancavel_do_dia` | `app/chamados/juizes.py:1622` | `app/chamados/services/vigia_fio.py:640`; `app/escala/services/cadastro_realidade.py:460`; `app/folha/fora_da_folha.py:67` |

Os testes também aparecem na saída do script: carga/tarefa, portas, alcance, carimbo e placar têm consumidores de teste existentes. Exemplos: `app/chamados/tests/test_e2_tarefa_do_dia.py:443`; `app/chamados/tests/test_e3_portas_tarefa.py:120`; `app/chamados/tests/test_o261_vigia_fio.py:895`; `app/core/tests/test_fio_mudo_placar.py:65`.

**LEU — escritores e leitores dos dados:**

| Dados | Escritor/origem | Leitores e distinções |
|---|---|---|
| `inicio`, `fim`, `quem`, `escopo`, `rc` da pista | O contrato atribui uma linha por run à trava. O corpo HEAD termina executando `"$@"`, sem essa emissão. | Leitor previsto: `pista_estado.ler/linha` e seção no gerador. É contrato/proposta, não implementação comprovada no HEAD. (`app/docs/RELATO.md:1648`; `bin/trava_teste.sh:42`; `bin/gerar_estado.py:265`) |
| `ESTEIRA_QUEM`, `ONLY`, `LABELS` | Chamadores fornecem identidade e seleção; a trava acrescenta PID a `QUEM`. | Trava lê identidade; executores leem seleção. Shell foi conferido diretamente, **não por AST Python**. (`bin/suite.sh:226`; `bin/suite.sh:232`; `bin/regua.sh:165`; `bin/pre-push.sh:142`; `bin/trava_teste.sh:23`) |
| `REGUA_*` | Régua grava impressão, árvore, resultado e instante. | Pre-push lê o carimbo e confere correspondência com o push. Campos adicionais de certificado do anexo: **[nao verificado no HEAD]**. (`bin/regua.sh:196`; `bin/pre-push.sh:65`; `bin/pre-push.sh:68`) |
| `medido_em`, `hoje`, `esperado`, `censo`, `alcance`, alarmes | `handle` monta o registro; `_carimbar` serializa com troca atômica. | ESTADO lê tempo, contadores e esperados; placar lê o mesmo caminho e contadores nomeados. (`app/chamados/management/commands/vigiar_fio_morto.py:232`; `:137`; `bin/fio_estado.py:121`; `app/core/management/commands/pipeline_placar.py:92`) |
| `ini`, `fim`, `agora`, `_pares`, `_portas` | Base recebe limites; carga recebe instante, materializa pares e carrega portas sob demanda. | `tarefa` usa janela fechada e instante da carga. (`app/ponto/turnos.py:2976`; `app/chamados/services/tarefa_do_dia.py:367`; `:420`; `:427`; `:529`; `:517`) |
| `fila_ids`, `fila_cel`, `verbo` | `alimentar_fila` monta a alimentação. | Censo lê pertença global; tarefa lê pertença, verbos e recorte da célula. (`app/chamados/juizes.py:1549`; `app/chamados/services/vigia_fio.py:610`; `app/chamados/services/tarefa_do_dia.py:546`) |
| `vivas`, `instancias`, `no_app` | `alimentar_portas` monta vivas/mapa; `_no_app` preenche cache. | Censo usa vivas, identidade compartilhada e app. Isso não equivale à seleção de perguntas da carga. (`app/chamados/juizes.py:1580`; `:1585`; `:1593`; `app/chamados/services/vigia_fio.py:615`; `:620`; `:625`) |
| RSS e razão inteiras/pouso | Nenhum escritor desses novos dados foi comprovado nos corpos HEAD examinados. | RSS pertence ao complemento; a razão pertence ao contrato da pista. (`app/chamados/management/commands/vigiar_fio_morto.py:120`; `app/docs/BACKLOG.md:738`; `app/docs/RELATO.md:1648`) |

**Juízes versus derivações:**

- **Registrado no lookup:** `tarefa_do_dia`, puro, relógio `agora`, lote `CargaDaTarefa`. Também consta no registro vivo. (`app/docs/JUIZES.md:78`; `app/docs/JUIZES.md:84`; `app/core/juizes.py:295`)
- **Registrado no vivo:** `porta_alcancavel_do_dia`, pergunta C17. Sua autoridade consta expressamente em `core/juizes.py`; não atribuo sua inscrição ao trecho do lookup que descreve C19. (`app/core/juizes.py:132`; `app/core/juizes.py:293`)
- **Alimentadores:** `alimentar_fila` e `alimentar_portas` carregam fontes dos juízes; não são autorização para substituir uma pergunta por outra. (`app/chamados/juizes.py:1532`; `app/chamados/juizes.py:1556`)
- **Derivações próprias de instrumento:** censo categoriza alcance; leitor do ESTADO calcula idade e divergência dos esperados; placar compara contadores. `_carimbar` escreve fotografia. (`app/chamados/services/vigia_fio.py:630`; `bin/fio_estado.py:125`; `bin/fio_estado.py:132`; `app/core/management/commands/pipeline_placar.py:105`; `app/chamados/management/commands/vigiar_fio_morto.py:21`)
- **Pista:** o contrato dá ao Git a autoridade dos pousos e ao leitor a agregação da medida. “Juiz da seleção”, escrito no anexo, não comprova registro em JUIZES. (`app/docs/RELATO.md:1648`; **[nao verificado: registro do seletor da raia]**)

**2. ESMERIL-SEMANTICA — decisões para o construtor, sem escolher**

Nas linhas referentes ao patch, **LEU anexo** significa precisamente **[nao verificado no HEAD]**.

| Campo/parâmetro | Divergência ou concordância lida | DECISÃO / opções e sítios |
|---|---|---|
| `escopo` | Anexo identifica recorte pela presença de `ONLY`. HEAD reconhece inteira pela cobertura efetiva da seleção, inclusive com `--only`. | Transportar o resultado do reconhecimento existente ou uma descrição explícita equivalente; não criar outro classificador. (`bin/suite.sh:155`; `bin/suite.sh:173`; anexo **[nao verificado no HEAD]**) |
| `modo`, `--motivo`, `--cobertura` | `_porta` anexada chama inteiras sem motivo. HEAD exige motivo e possui interface posterior à base da raia. | Adaptar os chamadores preservando a guarda; definir encaixe compatível da cobertura. Remover a guarda contraria lei escrita. (`CLAUDE.md:101`; `bin/suite.sh:53`; `bin/suite.sh:173`; anexo **[nao verificado no HEAD]**) |
| `quem`, `inteira`, `evitada` | `_teste_irmao` atende CI e control-plane; este último roda mesmo quando o arsenal é pulado. | Manter total de todos os runs e explicitar suas partições; escolher apresentação que não faça “evitada” significar ausência de qualquer execução. (`bin/pre-push.sh:142`; `:159`; `:186`; `:190`; `:198`; `app/docs/RELATO.md:1648`) |
| `inicio`, `fim`, janela | Anexo grava ao terminar, mas seleciona pelo início; corrida ainda viva não aparece. | Início, fim ou sobreposição; explicitar tratamento da corrida em andamento e bordas. É a pergunta de alcance nº 1. (`app/docs/RELATO.md:1648`; anexo **[nao verificado no HEAD]**) |
| `desde`, `agora`, fuso | Anexo usa datas ingênuas, somente limite inferior e Git sem offset explícito. | Definir representação comum de instante e limites; a apresentação UTC−3 já está escrita. Fuso real dos processos e pertença temporal dos merges: **[nao verificado]**. (`CLAUDE.md:146`; `app/docs/RELATO.md:1648`; anexo **[nao verificado no HEAD]**) |
| `pousos` | Anexo converte Git ilegível em `None`, mas apresenta isso como “sem pouso”, igual a zero. | Estado explícito por fonte ou propagação de indisponibilidade. A ausência de sinal não permite afirmar zero. (`CLAUDE.md:35`; `bin/gerar_estado.py:258`; anexo **[nao verificado no HEAD]**) |
| `runs`, `evitadas`, `escapou`, `suspensa` | Anexo descarta linhas inválidas e converte medida auxiliar ilegível em lista vazia. Suspensão é presença atual de arquivo, enquanto os contadores são de janela. | Distinguir ausência, parcialidade, zero observado e estado atual; registrar descartes ou invalidar a fonte afetada. (`app/docs/RELATO.md:1648`; `CLAUDE.md:192`; anexo **[nao verificado no HEAD]**) |
| `inteiras_por_pouso`, `meta` | Anexo arredonda antes de comparar; uma razão ligeiramente superior pode parecer dentro de `1,5`. | Comparação crua com exibição arredondada, ou mudança explícita do contrato se o responsável quiser tolerância. Não há tolerância escrita. (`app/docs/RELATO.md:1648`; anexo **[nao verificado no HEAD]**) |
| `rc`, “run” | Contrato pede rc real; trava retorna 75 antes de executar quando não obtém vez. Docker pode falhar depois de obter a trava. | Explicitar na apresentação “comando executado” versus “testes iniciados”, preservando rc e a contagem contratada. (`app/docs/RELATO.md:1648`; `bin/trava_teste.sh:36`; `:38`; `:42`; `bin/pre-push.sh:148`) |
| RSS, `esperado`, operador | Complemento pede `<=1500`; ESTADO compara por igualdade e comando alarma por valor verdadeiro do contador. | Bloco de memória com operador explícito, ou representação de violação com esperado zero; avaliar leitores sem mudar a igualdade dos contadores existentes. (`app/docs/BACKLOG.md:738`; `bin/fio_estado.py:125`; `app/chamados/management/commands/vigiar_fio_morto.py:187`) |
| RSS, unidade e período | `_carimbar` só serializa; coleta de pico não está implementada ali. O aval escolhe rodada/próprio processo. | Contratar fonte, conversão MB/MiB, início/fim da acumulação e falha de coleta. Reduzir retenção não comprova redução do pico já ocorrido. (`app/chamados/management/commands/vigiar_fio_morto.py:120`; `app/docs/BACKLOG.md:738`; `app/docs/RELATO.md:14`) |
| `carga`, último leitor | HEAD `:176` abre tranca; o censo consulta C17. A localização pós-E3 fornecida não corresponde ao caminho atual. | Reidentificar todos os aliases/último uso após E3; liberar no chamador ou encerrar escopo auxiliar. Último leitor futuro: **[nao verificado]**. (`app/chamados/management/commands/vigiar_fio_morto.py:176`; `app/chamados/services/vigia_fio.py:640`) |
| `ini`, `fim`, `dia_do_turno` | Há concordância: limites inclusivos e pergunta por dia do turno. | Preservar esses contratos; encurtar o retângulo é alteração diferente de soltar a carga. (`app/chamados/services/tarefa_do_dia.py:376`; `:420`; `:517`; `:562`) |
| `agora`, fotografia histórica | Carga exige o mesmo instante, mas declara leitura de vários estados atuais. Alimentação C17 repassa `agora` à fila e chama `pergunta_viva(p)` sem esse argumento. | Não tratar o lote como fotografia histórica integral; decidir eventual alinhamento temporal em escopo próprio, sem embuti-lo na liberação de memória. (`app/chamados/services/tarefa_do_dia.py:350`; `:355`; `:570`; `app/chamados/juizes.py:1576`; `:1583`) |
| `fila_ids`, `vivas`, `instancias`, `no_app` | C17 e carga não possuem o mesmo universo: célula/C6/app versus colaborador/janela/pergunta do fato. | Preservar as alimentações; substituição por interface pública equivalente exigiria outra demonstração de contrato. Não deduzir equivalência por sobreposição de SQL. (`app/chamados/juizes.py:1581`; `:1593`; `app/chamados/services/tarefa_do_dia.py:375`; `app/chamados/services/vigia_fio.py:615`) |
| `hoje`, `medido_em` | Vigia deriva dia civil local de um instante; placar exige o dia civil julgado; ESTADO converte offset ao fuso do host. | Confirmar fusos e rótulos; não reinterpretar `hoje` como dia do turno nesta obra. Igualdade dos fusos efetivos: **[nao verificado]**. (`app/chamados/management/commands/vigiar_fio_morto.py:168`; `:233`; `app/core/management/commands/pipeline_placar.py:86`; `:95`; `bin/fio_estado.py:129`) |

**3. Arquivos em comum**

A associação **cópia → arquivo** abaixo foi **LEU exclusivamente no anexo `arquivos_em_voo.txt`**; estado atual dessas raias: **[nao verificado]**.

| Necessidade da obra | Arquivo | Cruzamento fornecido |
|---|---|---|
| Principal: gerador, trava, declaração de escopo e selo | `bin/gerar_estado.py`, `bin/trava_teste.sh`, `bin/suite.sh`, `bin/regua.sh`, `bin/pre-push.sh`, futuros leitor/selo | Nenhum caminho correspondente no anexo. **[nao verificado fora da lista fornecida]** |
| Complemento: coleta/carimbo/alarme | `app/chamados/management/commands/vigiar_fio_morto.py` | `wt-bugpar-124848`, `wt-codex-hj-090-152256`, `wt-e3-b6-114947` |
| Complemento: possível local de liberação pós-E3 | `app/chamados/services/vigia_fio.py` | `wt-bugpar-124848`; necessidade exata pós-E3 **[nao verificado]** |
| Se reutilizar teste existente do vigia | `app/chamados/tests/test_o261_vigia_fio.py` | `wt-bugpar-124848`, `wt-codex-hj-090-152256`, `wt-e3-b6-114947` |

**INFERIU:** mudar o juiz/lote ou o cron não decorre necessariamente do complemento, que pede liberação no caminho consumidor e instrumento RSS. (`app/docs/BACKLOG.md:738`)

Caso o desenho amplie esses toques, os cruzamentos fornecidos são:

| Arquivo condicional | Cópias do anexo — **[nao verificado no HEAD]** |
|---|---|
| `app/chamados/services/tarefa_do_dia.py` | `wt-bugpar-124848`, `wt-e3-b6-114947` |
| `app/chamados/juizes.py` | `wt-bugpar-124848`, `wt-o168v6-233014`, `wt-e3-b6-114947` |
| `app/config/crons.py` | `wt-bugpar-124848`, `wt-codex-hj-090-152256` |

**4. perguntas de lei: 1**

**INFERIU — lacuna de alcance do instrumento:** quais corridas pertencem às “últimas 24 h”? Candidatas: início na janela; término na janela; qualquer sobreposição. Cada candidata precisa declarar bordas inclusivas/exclusivas, teto `agora` e tratamento das corridas ainda abertas. O contrato enumera início/fim, mas não escolhe essa pertença. (`app/docs/RELATO.md:1648`)

A exigência de unidade/universo/exclusões manda explicitar a resposta, sem fornecer a escolha. Não escolhi nenhuma candidata. (`CLAUDE.md:192`)

**Não contei novamente questões já respondidas:**

- Pouso = merges first-parent do main; fonte = Git; meta `<=1,5`; escopo declarado pelo chamador; falha da escrita não altera rc; log ilegível = sem medida. (`app/docs/RELATO.md:1648`)
- Suite inteira exige motivo, inclusive seleção efetivamente completa; a árvore certificada deve ser a que pousa. (`CLAUDE.md:101`; `CLAUDE.md:320`) [nao verificado]
- Instrumento exige contrato de cinco linhas e pouso próprio; não pode virar juiz de negócio. (`app/docs/CORTES.md:394`; `CLAUDE.md:327`; `CLAUDE.md:172`)
- Complemento é pós-E3, sem mudar E3,

[truncado em 20000 de 22288 caracteres]

## 13/11 18:15 · sessao f564a3cfb31d · RAIA-PREPARACAO · censo · origem=Code

**Achado principal — LEU:** o retro entrega `foto_ausente.batida_id`; o nascimento da identidade e o gerador procuram `ultima_batida.id`. A chave gravada pelo retro não satisfaz esses leitores. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:100`; `app/chamados/models.py:3464`; `app/chamados/services/disputa_emissao.py:387`)

**Limite de proveniência:** a cópia não contém repositório Git reconhecido por `git rev-parse HEAD`; vínculo ao HEAD/hash informado **[nao verificado]**. As referências abaixo são das linhas efetivamente lidas nesta cópia. Não gravei arquivos, importei a aplicação ou consultei banco/log **[nao verificado — registro desta execução]**.

**1. LEITORES E ESCRITORES POR AST**

Escopo: costura da obra B — envelope da batida, porta, gerador e derivação do episódio. O censo geral de todos os contratos de entrada mencionado no item C do aval fornecido fica fora deste levantamento **[nao verificado no HEAD — delimitação pelo anexo]**.

Script executado por `python3 -B -`, integralmente por stdin. `LITERAL` identifica candidato sintático; persistência e domínio são conferidos na leitura posterior. Despacho dinâmico, `getattr`, aliases por atribuição e fluxo interprocedural completo **[nao verificado]**.

```python
import ast
from pathlib import Path

TARGETS = {"criar_ou_atualizar_disputa", "gerar_perguntas_disputa",
           "batida_origem_do_nascimento"}
KEYS = {"ultima_batida", "foto_ausente"}
SKIP = {"tests", "migrations", "logs", ".git", "node_modules", ".venv", "venv"}
rows, errors = set(), []
parsed = 0

def literal(node):
    return node.value if isinstance(node, ast.Constant) else None

def source(node):
    return ast.unparse(node).replace("\n", " ")

for path in sorted(Path(".").rglob("*.py")):
    if SKIP.intersection(path.parts) or path.name.startswith("test"):
        continue
    try:
        tree = ast.parse(path.read_text(encoding="utf-8"), filename=str(path))
    except (SyntaxError, UnicodeError) as exc:
        errors.append((str(path), str(exc)))
        continue
    parsed += 1
    parents = {}
    for parent in ast.walk(tree):
        for child in ast.iter_child_nodes(parent):
            parents[child] = parent
    aliases = {n.asname or n.name: n.name for n in ast.walk(tree)
               if isinstance(n, ast.ImportFrom) for n in n.names}

    def owner(node):
        while node in parents:
            node = parents[node]
            if isinstance(node, (ast.FunctionDef, ast.AsyncFunctionDef)):
                return node.name
        return "<module>"

    def emit(kind, node, detail):
        rows.add((str(path), node.lineno, kind, owner(node), detail))

    def dict_leaves(node, prefix):
        if not isinstance(node, ast.Dict):
            return
        for key, value in zip(node.keys, node.values):
            k = literal(key)
            if isinstance(k, str):
                if isinstance(value, ast.Dict):
                    dict_leaves(value, prefix + "." + k)
                else:
                    emit("LITERAL", key,
                         prefix + "." + k + " <- " + source(value))

    for node in ast.walk(tree):
        if isinstance(node, (ast.FunctionDef, ast.AsyncFunctionDef)):
            if node.name in TARGETS:
                emit("DEF", node, node.name + "(" + source(node.args) + ")")
        if isinstance(node, ast.Call):
            name = (aliases.get(node.func.id, node.func.id)
                    if isinstance(node.func, ast.Name)
                    else node.func.attr
                    if isinstance(node.func, ast.Attribute) else "")
            if name in TARGETS:
                emit("CALL", node, source(node))
            if (isinstance(node.func, ast.Attribute)
                    and node.func.attr == "get" and node.args):
                k = literal(node.args[0])
                if k in KEYS or (
                    k in {"id", "timestamp", "tipo"}
                    and source(node.func.value) in {"ub", "ult"}
                ):
                    emit("R-key", node, source(node))
        if isinstance(node, ast.Subscript) and literal(node.slice) in KEYS:
            emit("W-key" if isinstance(node.ctx, ast.Store) else "R-key",
                 node, source(node))
            parent = parents.get(node)
            if isinstance(node.ctx, ast.Store) and isinstance(parent, ast.Assign):
                dict_leaves(parent.value, str(literal(node.slice)))
        if isinstance(node, ast.Dict):
            for key, value in zip(node.keys, node.values):
                if literal(key) in KEYS:
                    dict_leaves(value, str(literal(key)))

print("PARSED", parsed, "ERRORS", len(errors))
for path, line, kind, function, detail in sorted(rows):
    print(f"{kind} {path}:{line} {function}: {detail}")
for path, error in errors:
    print("PARSE_ERROR", path, error)
```

Resultado instrumental: `PARSED 887 ERRORS 0`; 5 chamadas da porta, 5 do gerador e 3 do derivador de origem **[nao verificado em arquivo — saída desta execução]**. Sítios correspondentes:

| Função | Chamadores encontrados — LEU |
|---|---|
| `criar_ou_atualizar_disputa` | `app/api/views.py:764`; `app/api/views_core.py:758`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:96`; `app/ponto/services/triagem_batida.py:735`; `app/chamados/services/fabrica_da_celula.py:172` |
| `gerar_perguntas_disputa` | `app/chamados/management/commands/smoke_chamados.py:103`; `app/chamados/services/disputa_emissao.py:294`; `app/chamados/services/disputa_emissao.py:670`; `app/chamados/services/disputa_emissao.py:682`; `app/chamados/services/fabrica_da_celula.py:174` |
| `batida_origem_do_nascimento` | `app/chamados/models.py:2867`; `app/chamados/models.py:3450`; `app/chamados/services/disputa_emissao.py:326` |

A porta é definida em `app/chamados/services/disputa_emissao.py:634` e reexportada por `app/chamados/models.py:4094`; os cinco emissores acima a importam por `chamados.models`. O gerador está em `app/chamados/services/disputa_emissao.py:300`; o derivador, em `app/chamados/models.py:3453`.

| Emissor | Payload relevante — LEU | Família |
|---|---|---|
| API principal | `ultima_batida={id:batida.pk, tipo:tipo, timestamp:agora.isoformat()}`; `foto_ausente={tipo_batida:tipo}`. (`app/api/views.py:758`; `app/api/views.py:453`) | Pode emitir BATIDA; `foto_ausente` pertence a BATIDA. (`app/chamados/catalogo/motivos.py:43`) |
| API gêmea | Mesmo formato. (`app/api/views_core.py:752`; `app/api/views_core.py:439`) | Mesma classificação. (`app/chamados/catalogo/motivos.py:43`) |
| Retro | Somente `foto_ausente={tipo_batida,batida_id,batida_em,_por}`. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:98`) | BATIDA literal. (`app/chamados/catalogo/motivos.py:43`) |
| Triagem | `saida_sem_entrada={data,hora_tentativa}`. (`app/ponto/services/triagem_batida.py:739`) | MARCO literal. (`app/chamados/catalogo/motivos.py:35`) |
| Fábrica | `{motivo:{data:dia.isoformat()}}`. (`app/chamados/services/fabrica_da_celula.py:172`) | **INFERIU MARCO no fluxo lido:** motivos vêm da ata e da tabela de marcos. (`app/chamados/services/fabrica_da_celula.py:139`; `app/chamados/services/fabrica_da_celula.py:71`; `app/chamados/services/declaracao_texto.py:42`) |

Assim, “os outros quatro emissores cumprem”, no anexo, não demonstra quatro outros emissores BATIDA: a cópia mostra duas APIs BATIDA, uma triagem MARCO e uma fábrica cujo fluxo lido produz MARCO. Cobertura de chamadas dinâmicas **[nao verificado]**.

| Campo da costura | Escritores — LEU | Leitores — LEU |
|---|---|---|
| `ultima_batida.id` | APIs: `app/api/views.py:759`; `app/api/views_core.py:753` | Origem: `app/chamados/models.py:3464`; dia: `app/chamados/services/disputa_emissao.py:72`, `app/chamados/services/disputa_emissao.py:387`; upload: `app/api/foto_recebida.py:68` |
| `ultima_batida.timestamp` | APIs: `app/api/views.py:761`; `app/api/views_core.py:755` | Último degrau do dia: `app/chamados/juizes.py:160`, com `.date()` em `app/chamados/juizes.py:166` |
| `ultima_batida.tipo` | APIs: `app/api/views.py:760`; `app/api/views_core.py:754` | Nenhum leitor específico localizado; leitura dinâmica/genérica **[nao verificado]** |
| `foto_ausente.tipo_batida` | APIs e retro: `app/api/views.py:453`; `app/api/views_core.py:439`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:99` | Nenhum leitor específico desse caminho localizado **[nao verificado]** |
| `foto_ausente.batida_id` | Retro: `app/ponto/management/commands/marcar_foto_ausente_retro.py:100` | Os leitores de episódio procuram outro caminho: `app/chamados/models.py:3464`; `app/chamados/services/disputa_emissao.py:387` |
| `foto_ausente.batida_em` / `_por` | Retro: `app/ponto/management/commands/marcar_foto_ausente_retro.py:101`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:102` | Nenhum leitor semântico específico localizado **[nao verificado]** |

O escritor persistente do contexto é `_abrir_ou_agregar_disputa`: nascimento em `app/chamados/services/disputa_emissao.py:590`; atualização em `app/chamados/services/disputa_emissao.py:562`. Ele funde o dicionário do motivo, acrescenta `qtd`/`ultima_em` e substitui outras chaves de topo. (`app/chamados/services/disputa_emissao.py:548`; `app/chamados/services/disputa_emissao.py:556`; `app/chamados/services/disputa_emissao.py:561`)

A coluna de episódio nasce pelo derivador em `_nascer` e na preparação da identidade; o contrato escrito diz que ela é preenchida uma vez. (`app/chamados/models.py:2867`; `app/chamados/models.py:3450`; `app/chamados/models.py:2437`)

Homônimos descartados após leitura: o literal `foto_ausente` do catálogo de perguntas e as `ultima_batida` de ficha/ranking não são esse envelope de disputa. (`app/chamados/models.py:3604`; `app/colaboradores/services/ficha.py:215`; `app/escala/services/furos_diarios.py:281`; `app/relatorios/views.py:1675`)

**Autoridade registrada versus derivação própria:**

| Sítio | Classificação |
|---|---|
| `turnos_do_colab` | Juiz registrado no lookup e no vivo. (`app/docs/JUIZES.md:20`; `app/core/juizes.py:208`) |
| `dia_do_turno_da_batida` | Adaptador da resposta desse juiz, pelo mapa por PK; não cria outra regra de dia. (`app/ponto/turnos.py:2927`; `app/ponto/turnos.py:2935`) |
| `selfie_ausente` | Registrado no vivo, com corte escrito; entrada nominal no lookup não localizada **[nao verificado]**. O vivo prevalece. (`app/core/juizes.py:1199`; `app/docs/CORTES.md:283`; `app/docs/JUIZES.md:14`) |
| Retro | Delega ausência/prazo ao juiz; monta o próprio envelope incompatível. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:74`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:98`) |
| Gerador / derivador de origem | Selecionam insumos do contexto; registro como juízes próprios não localizado **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:372`; `app/chamados/models.py:3464`) |
| `_dia_do_contexto_fraco` | Deriva dia civil do timestamp por conta própria, como último degrau declarado. (`app/chamados/juizes.py:151`; `app/chamados/juizes.py:166`) |
| `resolver_disputas_de_foto` | Seleciona disputa pelo contexto e encaminha perguntas em conjunto; a validação delega ao juiz registrado. (`app/api/foto_recebida.py:67`; `app/api/foto_recebida.py:73`; `app/api/foto_recebida.py:51`; `app/docs/JUIZES.md:38`) |

**Selos lidos:** o selo AST da família prende a declaração de família e os índices; o da selfie prende delegação ao juiz. Nenhum desses prende o envelope entregue pela porta. (`app/chamados/tests/test_e1_v5_familia.py:25`; `app/chamados/tests/test_e1_v5_familia.py:98`; `app/ponto/tests/test_selfie_ausente_juiz.py:130`)

O teste do retro verifica `foto_ausente.batida_id`; sua idempotência conta disputas, sem verificar identidade das perguntas. **INFERIU:** ele pode deixar passar esta quebra de contrato. Execução dos testes **[nao verificado]**. (`app/ponto/tests/test_bug126_selfie_invisivel.py:71`; `app/ponto/tests/test_bug126_selfie_invisivel.py:113`)

**2. DIVERGÊNCIAS SEMÂNTICAS — DECISÕES PARA O CONSTRUTOR**

As alternativas abaixo são de desenho; não escolhi nenhuma.

| DECISÃO | Desencontro e opções |
|---|---|
| **D1 — Transporte do episódio** | O retro entrega PK em `foto_ausente.batida_id`; os consumidores exigem `ultima_batida.id`. Opções: montar o envelope consumido diretamente no emissor; ou usar um montador comum dos emissores que entregue esse mesmo contrato. Alterar consumidores para aceitar outro transporte acrescenta alcance de arquivos. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:100`; `app/chamados/models.py:3464`; `app/chamados/services/disputa_emissao.py:387`) |
| **D2 — Evento desta emissão versus contexto acumulado** | O merge preserva chaves antigas ausentes no payload novo. **INFERIU:** o retro pode deixar uma `ultima_batida` de outro evento disponível ao nascimento. Opções: envelope completo e substituído no ato; ou transporte explícito do episódio corrente, independente do agregado. O acervo não pode ganhar episódio pela última batida do contexto fundido. (`app/chamados/services/disputa_emissao.py:548`; `app/chamados/services/disputa_emissao.py:561`; `app/chamados/models.py:3457`) |
| **D3 — Instante do fato versus recepção** | A API grava a batida com `ts_efetivo`, mas escreve `ultima_batida.timestamp=agora`; o retro escreve `batida_em` com o timestamp efetivo local. Opções: declarar e manter separados os instantes de recepção e fato; ou normalizar o campo existente, com alcance explícito sobre seus leitores. Não são sinônimos apenas convertendo fuso. (`app/api/views.py:507`; `app/api/views.py:655`; `app/api/views.py:761`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:101`) |
| **D4 — Dia transportado versus dia do episódio** | O gerador tenta `motivo.data` antes de consultar o episódio; a porta, quando possui episódio, consulta o juiz para o dia efetivo. Opções: transportar episódio e delegar o dia; ou transportar também o dia previamente obtido do mesmo juiz. Extrair o prefixo de `batida_em` como dia do turno não encontra apoio nesse contrato. (`app/chamados/services/disputa_emissao.py:372`; `app/chamados/services/disputa_emissao.py:387`; `app/chamados/models.py:2876`; `app/ponto/turnos.py:2928`) |
| **D5 — Upload por disputa versus identidade por episódio** | O upload compara uma `ultima_batida.id`, depois encaminha todas as perguntas de foto — ou todas da disputa — à validação. A identidade BATIDA é por episódio. Opções: demonstrar por bateria que o alcance coletivo preserva os episódios; ou incluir a migração desse leitor para seleção por episódio numa fatia declarada. (`app/api/foto_recebida.py:68`; `app/api/foto_recebida.py:73`; `app/api/foto_recebida.py:79`; `app/chamados/catalogo/motivos.py:16`) |
| **D6 — Contagem de emissões versus episódios** | `qtd` incrementa a cada merge do motivo e `ultima_em` recebe o instante do merge. Esses campos não contam batidas distintas. Opções: mantê-los declarados como recorrência de emissão; ou declarar separadamente uma necessidade de medida por episódio. Não usar `qtd` como prova de quantidade de fatos. (`app/chamados/services/disputa_emissao.py:552`; `app/chamados/services/disputa_emissao.py:557`; `app/chamados/services/disputa_emissao.py:558`) |

Conferência dos demais campos/parâmetros:

| Campo/parâmetro | Significado lido |
|---|---|
| `colaborador` / PK da batida | O derivador exige que a batida pertença ao colaborador da disputa. (`app/chamados/models.py:3468`) |
| `motivos` | A porta elimina repetições da lista; a família vem do catálogo, não da presença de uma chave no payload. (`app/chamados/services/disputa_emissao.py:659`; `app/chamados/catalogo/motivos.py:53`) |
| `origem` da porta / `_por` | `origem` participa da descrição/trilha; `_por` é metadado do retro. Não são a chave de identidade do episódio. (`app/chamados/services/disputa_emissao.py:664`; `app/chamados/services/disputa_emissao.py:597`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:102`; `app/chamados/models.py:3490`) |
| `tipo` / `tipo_batida` | APIs transportam a variável `tipo`; retro transporta `b.tipo`. Equivalência em todos os caminhos de dedup/conflito **[nao verificado]**. (`app/api/views.py:760`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:99`) |
| `data_evento` / `conversa` | Pergunta de fato usa dia do turno; conversa usa dia civil do pedido. São domínios distintos já escritos. (`app/chamados/models.py:2784`; `app/chamados/models.py:2795`; `app/chamados/models.py:2802`) |
| `tem_foto` | `FotoBatida OU Batida.foto`, coerente entre cron e juiz. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:75`; `app/ponto/registro_batida.py:83`) |
| `decorrido_min` / prazo | Minutos reais desde a batida; igualdade vence: `>=`. PWA=0; APK/NULL=90; override não altera PWA. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:76`; `app/ponto/registro_batida.py:72`; `app/ponto/registro_batida.py:89`; `app/ponto/registro_batida.py:91`) |
| `--dias` | Duração de varredura desde `agora`, com ambas as bordas inclusivas; não representa quantidade de dias de turno. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:65`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:67`) |
| Fuso da apresentação | O retro converte `batida_em` para local, mas seu cabeçalho formata `agora`/`corte` crus. Divergência lateral com a regra de apresentação; necessidade de incluí-la nesta obra **[nao verificado]**. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:101`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:81`; `CLAUDE.md:146`) |

**3. ARQUIVOS EM COMUM**

**INFERIU — desenho restrito à cura no emissor:** a origem necessária é o retro; o teste existente do retro precisa receber cobertura da pergunta/episódio, além do selo AST solicitado. Esses dois arquivos não aparecem na lista fornecida. Localização do selo futuro ainda não definida **[nao verificado]**. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:96`; `app/ponto/tests/test_bug126_selfie_invisivel.py:65`; anexo `arquivos_em_voo.txt` **[nao verificado no HEAD]**)

Se as decisões acima ampliarem a alteração aos consumidores, há estes cruzamentos. As atribuições de raia são leitura do anexo, sem comprovação no HEAD **[nao verificado]**:

| Arquivo candidato a alteração adicional | Raia(s) do anexo | Motivo da dependência |
|---|---|---|
| `app/chamados/services/disputa_emissao.py` | `wt-bugpar-124848`, `wt-e3-b6-114947` | Consome envelope e funde contexto. (`app/chamados/services/disputa_emissao.py:387`; `app/chamados/services/disputa_emissao.py:548`) |
| `app/chamados/models.py` | As duas anteriores + `wt-o168v6-233014` | Deriva episódio e mantém identidade. (`app/chamados/models.py:2867`; `app/chamados/models.py:3464`) |
| `app/api/views.py` | `wt-bugpar-124848`, `wt-e3-b6-114947` | Escritor de `ultima_batida`, se D3 alterar seu significado. (`app/api/views.py:758`) |
| `app/chamados/juizes.py` | As duas anteriores + `wt-o168v6-233014` | Leitor do timestamp, se D3 alterar seu significado. (`app/chamados/juizes.py:159`) |

`app/api/foto_recebida.py` e `app/api/views_core.py` não aparecem no anexo; sua eventual alteração depende de D5/D3, respectivamente **[nao verificado no HEAD quanto ao voo]**. (`app/api/foto_recebida.py:67`; `app/api/views_core.py:752`)

**4. perguntas de lei: 0**

**INFERIU, no alcance examinado:** as candidatas abaixo já têm resposta escrita; não foram contadas como lacunas.

| Candidata | Resposta existente |
|---|---|
| “Foto identifica dia ou batida?” | BATIDA identifica episódio; `foto_ausente` é dessa família. (`app/chamados/catalogo/motivos.py:16`; `app/chamados/catalogo/motivos.py:43`) |
| “Qual dia para uma batida noturna?” | Juiz do turno; batida não reclamada recebe o dia local em que aconteceu. (`app/ponto/turnos.py:2928`; `app/ponto/turnos.py:2955`) |
| “Pode extrair o episódio depois, do contexto fundido?” | O contrato explica por que isso não recupera o episódio do 

[truncado em 20000 de 23356 caracteres]

## 13/11 18:12 · sessao 4922a0386829 · - · - · origem=Ronald (tmux)

`_declarar_ausencia_de_pergunta` não produz férias no fluxo atual: resolve tipo pelo mapa em `app/chamados/services/materializacao.py:475`, retorna se desconhecido em `:476`, e o mapa contém somente falta/atestado (`app/chamados/models.py:3717`). Chamador exige motivo `declara_falta` (`app/chamados/services/materializacao.py:1083`). Portanto, esse achado AST não exige editar `materializacao.py` na HJ-091; inferência baseada nesses caminhos.

A irmã genérica `_materializar_ausencia_de_resposta` recebe tipos exclusivamente do mapa falta/atestado no chamador atual (`app/chamados/services/materializacao.py:603`; `app/chamados/services/disputa_emissao.py:1541`).

Quem mais lê, para caracterizar regressão:

- Célula: data/tipo/fim e eventos de save/delete (`app/ponto/signals.py:123`, `:124`, `:143`, `:150`).
- Fio: reconciliação e ausência decidida após commit (`app/ponto/signals.py:261`, `:265`, `:271`, `:272`).
- Cobrança: `ausencia_cobre(..., para='cobranca')`, explicação integral/parcial (`app/chamados/services/materializacao.py:79`, `:82`).
- Cobertura: folha exige aprovada; cobrança exclui rejeitada (`app/ponto/turnos.py:2517`, `:2519`); filtro por data/fim e desempate data/PK (`:2554`, `:2559`).
- Folha: filtro de aprovadas na janela e expansão delegada ao juiz (`app/folha/export.py:87`, `:70`).
- Histórico: raízes documentais, estado, datas/dias/documento/descrição (`app/ponto/services/dia_decidido.py:593`, `:615`, `:619`, `:621`, `:623`, `:624`).

## 13/11 18:12 · sessao 83dfb72e585c · - · - · origem=Ronald (tmux)

AST executado com `python3 -B`: **3369 arquivos parseados, 0 erros de parse, 103 registros**; contagem derivada em memória, sem arquivo de prova. [nao verificado: resultado instrumental desta execução] Cada etapa de uma cadeia ORM aparece como chamada separada; não são 103 sítios independentes. [nao verificado]

```python
import ast, pathlib

fields = {
    'tipo', 'status', 'data_turno', 'batida_entrada', 'batida_entrada_id',
    'saida_informada', 'batida_gerada', 'batida_gerada_id', 'aprovada_em',
    'atualizado_em', 'criado_em',
}
writers = {
    'app/api/views.py': {'j'},
    'app/ponto/views.py': {'j'},
    'app/ponto/services/regularizacao.py': {'j'},
    'app/ponto/services/justificativa.py': {'j'},
    'app/ponto/services/entrada_sincronizada.py': {'just'},
    'app/ponto/services/triagem_batida.py': {'just'},
    'app/chamados/reconciliador.py': {'_j'},
}

def chain(n):
    if isinstance(n, ast.Name):
        return n.id
    if isinstance(n, ast.Attribute):
        return chain(n.value) + '.' + n.attr
    if isinstance(n, ast.Call):
        return chain(n.func) + '()'
    return '?'

rows, parsed, errors = [], 0, []
for p in sorted(pathlib.Path('app').rglob('*.py')):
    try:
        tree = ast.parse(p.read_text(), filename=str(p))
        parsed += 1
    except (SyntaxError, UnicodeError) as e:
        errors.append((str(p), str(e)))
        continue
    parent = {c: n for n in ast.walk(tree)
              for c in ast.iter_child_nodes(n)}

    def owner(n):
        while n in parent:
            n = parent[n]
            if isinstance(n, (ast.FunctionDef, ast.AsyncFunctionDef)):
                return n.name
        return '<module>'

    if ('tests' in p.parts or 'migrations' in p.parts
            or p.name.startswith('test_')):
        continue
    for n in ast.walk(tree):
        if (isinstance(n, ast.Call)
                and chain(n.func).startswith('Justificativa.objects.')):
            rows.append((
                str(p), n.lineno, owner(n), 'ORM', chain(n.func),
                ','.join(k.arg or '**' for k in n.keywords),
            ))
        if (isinstance(n, ast.Attribute)
                and isinstance(n.ctx, ast.Store)
                and isinstance(n.value, ast.Name)
                and n.value.id in writers.get(str(p), set())
                and n.attr in fields):
            rows.append((
                str(p), n.lineno, owner(n), 'W', chain(n), '',
            ))
for r in sorted(rows):
    print('|'.join(map(str, r)))
print('PARSED', parsed, 'ERRORS', errors, 'ROWS', len(rows))
```

Resultado agrupado, com sítios positivos:

| Grupo | Sítios encontrados |
|---|---|
| Criação/payload | `app/api/views.py:1992`, `app/ponto/views.py:1635`, `app/ponto/services/regularizacao.py:91`, `app/ponto/management/commands/processar_alertas_turno.py:394` |
| Alteração `tipo` | `app/api/views.py:1986`, `app/ponto/views.py:1625` |
| Alteração `saida_informada` | `app/ponto/views.py:1628`, `app/ponto/services/regularizacao.py:103`, `app/ponto/services/justificativa.py:382` |
| Alteração `batida_entrada` | `app/ponto/views.py:1630` |
| Alteração `batida_gerada` | `app/ponto/services/justificativa.py:204`, `app/ponto/services/justificativa.py:381`, `app/ponto/views.py:1831` |
| Alteração `status` | `app/ponto/services/justificativa.py:205`, `app/ponto/services/justificativa.py:283`, `app/ponto/services/justificativa.py:383`, `app/ponto/views.py:1796`, `app/ponto/views.py:1875`, `app/ponto/services/regularizacao.py:105`, `app/ponto/services/entrada_sincronizada.py:41`, `app/ponto/services/triagem_batida.py:608`, `app/chamados/reconciliador.py:533` |
| Alteração `aprovada_em` | `app/ponto/services/justificativa.py:207`, `app/ponto/services/justificativa.py:285`, `app/ponto/services/justificativa.py:385`, `app/ponto/views.py:1798`, `app/ponto/views.py:1877` |
| Cobertura/financeiro | `app/escala/utils.py:1197`, `app/ponto/services/efeito_ausencia.py:41`, `app/ponto/services/fechamento.py:661`, `app/relatorios/management/commands/diff_extrato_parcial.py:43` |
| Fato novo | `app/ponto/services/cartorio.py:387` |
| Bloqueio por turno órfão | `app/ponto/utils_turno_orfao.py:53` |
| Telas/histórico/contadores | `app/api/views.py:2088`, `app/chamados/views.py:596`, `app/core/views.py:205`, `app/core/views.py:244`, `app/core/views.py:1191`, `app/ponto/views.py:998`, `app/ponto/views.py:1673`, `app/ponto/views.py:1704`, `app/ponto/views.py:1736`, `app/ponto/views.py:1738`, `app/ponto/views.py:1742` |
| SLA | `app/ponto/management/commands/alertar_justificativas_sla.py:19` |
| Comando destrutivo existente, apenas lido | `app/ponto/management/commands/zerar_dados_operacionais.py:44`, `app/ponto/management/commands/zerar_dados_operacionais.py:95` |

Limites: o filtro ORM exige nome literal `Justificativa`; aliases como `_Just` não são resolvidos. Stores usam os nomes locais previamente lidos e não fazem inferência de tipos global. Não inclui templates, SQL dinâmico, defaults aninhados como Stores nem gravações indiretas; os kwargs de criação e `defaults` aparecem na linha da chamada. [nao verificado: limites do instrumento entregue]

## 13/11 18:11 · sessao de30e1eb0201 · - · - · origem=Ronald (tmux)

LEU — AST em `app/**/*.py` de produção, excluindo `tests/`, `migrations/` e arquivos `test*`, confirmou estes sítios:

| Campo da costura | Escritores | Leitores específicos |
|---|---|---|
| `ultima_batida.id` | `app/api/views.py:759`; `app/api/views_core.py:753` | `app/api/foto_recebida.py:68`; `app/chamados/models.py:3464`; `app/chamados/services/disputa_emissao.py:72`, `:73`, `:387` |
| `ultima_batida.timestamp` | `app/api/views.py:761`; `app/api/views_core.py:755` | `app/chamados/juizes.py:159`, `:160`, `:166` |
| `ultima_batida.tipo` | `app/api/views.py:760`; `app/api/views_core.py:754` | Nenhum leitor literal específico localizado nessa varredura; consumidores dinâmicos/genéricos [nao verificado]. |
| `foto_ausente.tipo_batida` | `app/api/views.py:453`; `app/api/views_core.py:439`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:99` | Nenhum leitor específico desse caminho localizado; consumidores dinâmicos/genéricos [nao verificado]. |
| `foto_ausente.batida_id` | `app/ponto/management/commands/marcar_foto_ausente_retro.py:100` | Nenhum leitor específico desse caminho localizado; consumidores dinâmicos/genéricos [nao verificado]. |
| `foto_ausente.batida_em` | `app/ponto/management/commands/marcar_foto_ausente_retro.py:101` | Nenhum leitor literal localizado; consumidores dinâmicos/genéricos [nao verificado]. |
| `foto_ausente._por` | `app/ponto/management/commands/marcar_foto_ausente_retro.py:102` | Nenhum leitor específico desse caminho localizado; consumidores dinâmicos/genéricos [nao verificado]. |

LEU — `foto_recebida` também depende da chave problemática: compara `d.contexto.ultima_batida.id` à batida recebida e depois valida todas as perguntas `foto_ausente` da disputa, ou todas as perguntas quando só resta aquele motivo. Não filtra perguntas por `batida_origem`. (`app/api/foto_recebida.py:67`, `:68`, `:73`, `:79`)

INFERIU — harmonizar o emissor altera também esse caminho de upload tardio; exige cenário com duas batidas/fotos na mesma disputa e fotos recebidas em ordem diferente. A necessidade decorre do contexto mutável usado para selecionar a disputa e da validação coletiva. (`app/api/foto_recebida.py:67`, `:73`, `:79`; `app/chamados/services/disputa_emissao.py:548`)

LEU — há três significados temporais diferentes:

- `ultima_batida.timestamp` nos APIs recebe **agora**, o instante de recepção/processamento. (`app/api/views.py:497`, `:761`; `app/api/views_core.py:483`, `:755`)
- `Batida.timestamp` recebe **ts_efetivo**, escolhido por `hora_do_aparelho`; pode diferir de agora. (`app/api/views.py:507`, `:654`; `app/api/views_core.py:493`, `:645`)
- `foto_ausente.batida_em` recebe o timestamp da batida convertido para horário local. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:101`)

LEU — o consumidor fraco do dia usa `datetime.fromisoformat(...).date()` sobre `ultima_batida.timestamp`; os consumidores fortes usam `ultima_batida.id` e o juiz do turno. (`app/chamados/juizes.py:159`, `:166`; `app/chamados/services/disputa_emissao.py:72`, `:76`, `:387`, `:391`)

DECISÃO DE DESENHO — declarar se o payload harmonizado mantém instante de detecção e instante da batida como conceitos distintos, ou se muda o significado do timestamp existente e migra seus leitores. Não tratar `batida_em` e `ultima_batida.timestamp` como sinônimos sem essa decisão. (`app/api/views.py:761`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:101`; `app/chamados/juizes.py:166`)

LEU — o selo AST existente da família verifica `Motivo(..., familia=FAMILIA_*)`; os testes dos índices verificam listas e chaves das famílias. Não examinam o contexto entregue pelos emissores. (`app/chamados/tests/test_e1_v5_familia.py:25`, `:98`, `:104`, `:111`)

LEU — o selo AST da porta de disputa impede a declaração de fabricar diretamente o chamado pai; não verifica o episódio no payload dos emissores. (`app/chamados/tests/test_porta_disputa_emissao.py:144`)

LEU — testes existentes constroem manualmente `ultima_batida` e testam contexto ausente como conversa; não exercitam o payload emitido pelo retro. (`app/chamados/tests/test_a2_data_evento_batida.py:21`, `:53`)

Selo AST geral “todo emissor FAMILIA_BATIDA entrega episódio consumível”: não localizado por busca nominal e leitura dos selos acima; inexistência completa [nao verificado].

Homônimos excluídos da costura: `tipo_batida` de ações de chamado é lido no contexto raiz, e `ultima_batida` de ficha/ranking é data ou timestamp apresentado, não o dict de disputa. (`app/chamados/services/acoes_chamado.py:30`, `:195`; `app/colaboradores/services/ficha.py:147`, `:215`; `app/escala/services/furos_diarios.py:281`)

Nenhum template com caminho literal `foto_ausente.batida_id`, `foto_ausente.batida_em` ou `ultima_batida.id` foi localizado; renderização genérica de contexto [nao verificado].

## 13/11 18:11 · sessao 88a38d37473c · - · - · origem=Ronald (tmux)

LEU por AST (python3 -B, `ast.parse`, sem importar app): **5 sítios de chamada de produção** da porta; todos importam por `chamados.models`, que reexporta o símbolo em `app/chamados/models.py:4094`.

| Sítio | Contexto entregue | Família |
|---|---|---|
| `app/api/views.py:764` | `contexto_anomalia`; acrescenta `ultima_batida={id:batida.pk,tipo:tipo,timestamp:agora.isoformat()}` em `:758` | Pode BATIDA; `foto_ausente` escrito em `:453`; catálogo BATIDA em `app/chamados/catalogo/motivos.py:43`. |
| `app/api/views_core.py:758` | Mesmo formato, escrito em `:752` | Pode BATIDA; `foto_ausente` em `:439`; catálogo `motivos.py:43`. |
| `app/ponto/management/commands/marcar_foto_ausente_retro.py:96` | Apenas `foto_ausente={tipo_batida:b.tipo,batida_id:b.pk,batida_em:localtime(b.timestamp).isoformat(),_por:...}` em `:98` | BATIDA literal (`motivos.py:43`). |
| `app/ponto/services/triagem_batida.py:735` | `saida_sem_entrada={data:_dia_sse.isoformat(),hora_tentativa:agora_local.strftime('%H:%M')}` em `:739` | MARCO literal (`motivos.py:35`). |
| `app/chamados/services/fabrica_da_celula.py:172` | `{motivo:{data:dia.isoformat()}}` | **INFERIU MARCO**: motivo dinâmico; fluxo `:207` → `:139` → `:62`/`:71` → tabela `app/chamados/services/declaracao_texto.py:42`, cujos quatro valores são MARCO no catálogo `motivos.py:34`, `:35`, `:48`, `:49`. |

Logo, a frase do dado “outros 4 emissores cumprem” descreve quatro outros chamadores da porta, mas **não quatro outros chamadores BATIDA identificados nesta cópia**: dois são MARCO no fluxo lido; cobertura dinâmica completa [nao verificado]. Referências acima.

**Desencontros e decisões de desenho, sem escolher:**

1. **Episódio no caminho errado:** retro escreve `foto_ausente.batida_id` (`marcar_foto_ausente_retro.py:100`); identidade lê exclusivamente `ultima_batida.id` (`models.py:3464`) e data do turno também (`disputa_emissao.py:387`). Opções para o construtor: emissor entregar o envelope atualmente lido, ou contrato explícito de parâmetro de episódio consumido pelos dois leitores. Aval fornecido já manda cura na origem; mudança de leitores não é necessidade demonstrada pelo código. Validação de pertencimento já existe em `models.py:3468`.

2. **Instante de chegada versus instante do fato:** API define `agora` em `views.py:497`, resolve `ts_efetivo` em `:507` e grava Batida com este em `:654`; porém envelope registra `timestamp=agora.isoformat()` em `:761`. Gêmeo: `views_core.py:483`, `:493`, `:645`, `:755`. Retro `batida_em` usa instante efetivo da Batida em `marcar_foto_ausente_retro.py:101`. Opções: declarar metadado como instante da chegada, distinguindo-o do fato; ou alinhar envelope ao instante da Batida. Não é apenas UTC versus local: offline permite fatos anteriores ao recebimento (`views.py:504`, `:519`). Os dois leitores principais usam o ID e consultam a Batida, não os timestamps do envelope (`models.py:3464`, `disputa_emissao.py:387`).

3. **Contexto agregado versus episódio imutável:** merge sobrescreve chaves genéricas por `ctx[k]=v` em `disputa_emissao.py:561`; modelo adverte que ler contexto posteriormente pode apontar outra anomalia em `models.py:3457`. Identidade deve nascer uma vez (`models.py:3454`). Opções de desenho: garantir transporte correto no ato e preservar coluna imutável; avaliar separadamente consumidores tardios do agregado. Não retroderivar acervo pelo contexto atual: proibição expressa no comentário `models.py:3458`.

4. **Upload por disputa versus pergunta por episódio:** `app/api/foto_recebida.py:67` seleciona disputa pela `ultima_batida.id`; `:73` valida todas as perguntas `foto_ausente` dela, ou `:79` todas as perguntas quando motivo único. Episódios distinguem perguntas BATIDA (`models.py:3474`, `disputa_emissao.py:321`). Opções: demonstrar por bateria que a seleção agregada preserva os episódios, ou tratar leitor por episódio numa fatia autorizada. Isso merece medição/RED; não ampliar silenciosamente a cura do emissor. Identidade por episódio já escrita, não pergunta de lei.

**Sem divergência de unidade/borda no predicado de prazo lido:** cron calcula minutos reais por `total_seconds()/60` (`marcar_foto_ausente_retro.py:76`) e juiz cobra inclusive `decorrido_min >= prazo` (`registro_batida.py:91`); `tem_foto` é `FotoBatida OU Batida.foto` nos dois lados (`marcar_foto_ausente_retro.py:75`, `registro_batida.py:83`). PWA=0, APK=90 e desconhecido=APK (`registro_batida.py:72`, `:77`). `--minutos` só sobrepõe APK/NULL (`registro_batida.py:89`), período de varredura é intervalo fechado `timestamp__gte=desde, timestamp__lte=agora` (`marcar_foto_ausente_retro.py:67`). Não alterar estes parâmetros como cura de episódio.

**Selos existentes lidos, não executados:** `app/ponto/tests/test_bug126_selfie_invisivel.py:65` verifica disputa e metadado `batida_id` em `:71`, mas não episódio/data da pergunta; idempotência em `:108` conta disputas (`:113`), não perguntas. `app/ponto/tests/test_selfie_ausente_juiz.py:130` usa AST para impedir `_eh_apk` próprio nas APIs; `:142` sela papel de cron/delegação. Não sela envelope BATIDA da chamada. `app/chamados/tests/test_a2_data_evento_batida.py:21` monta envelope correto `ultima_batida`, e `:53` testa sem envelope nascer conversa. **INFERIU:** selos podem estar verdes e ainda permitir o defeito do retro, pois verificam condições distintas; execução [nao verificado].

Script executado (consulta, sem gravar):

```python
import ast
from pathlib import Path

for p in sorted(Path(".").rglob("*.py")):
    if any(x in p.parts for x in (".git", "__pycache__", "tests", "migrations")):
        continue
    try:
        tree = ast.parse(p.read_text())
    except (SyntaxError, UnicodeError):
        continue
    aliases = {"criar_ou_atualizar_disputa"}
    for node in ast.walk(tree):
        if isinstance(node, ast.ImportFrom):
            for imported in node.names:
                if imported.name == "criar_ou_atualizar_disputa":
                    aliases.add(imported.asname or imported.name)
    for node in ast.walk(tree):
        if not isinstance(node, ast.Call):
            continue
        fn = node.func
        match = (
            isinstance(fn, ast.Name) and fn.id in aliases
        ) or (
            isinstance(fn, ast.Attribute)
            and fn.attr == "criar_ou_atualizar_disputa"
        )
        if match:
            print(f"{p}:{node.lineno} {ast.unparse(node)}")
```

Saída: exatamente as cinco chamadas da tabela. Limite: AST encontra nomes/atributos e aliases de import direto; despacho dinâmico, atribuição de alias e prova interprocedural [nao verificado].

## 13/11 18:10 · sessao 323f94675b3f · - · - · origem=Ronald (tmux)

**Revisão: perguntas de lei/alcance: 1.** O contrato registra início e fim do run e anuncia janela de 24 h, mas não define pertença por início, fim ou sobreposição, nem bordas inclusivas/exclusivas. (`app/docs/RELATO.md:1648`)

Candidatas: contar início na janela; fim na janela; qualquer sobreposição — com bordas explicitadas. Nenhuma foi escolhida. A lei exige universo e rótulo precisos, mas não resolve essa escolha. (`CLAUDE.md:35`; `CLAUDE.md:192`)

## 13/11 18:10 · sessao 89b687144e07 · - · - · origem=Ronald (tmux)

LEU — A raia antiga não basta para alcançar o dinheiro no HEAD: `min_parcial_pagos` é produzido em `efeito_ausencia.py:46-64`, mas seu único consumidor executável de produção encontrado é o PDF (`app/relatorios/pdf_espelho.py:336,479`). O espelho injeta apenas `justificadas` no motor (`app/ponto/services/espelho.py:1114`); a folha monta sua própria lista equivalente (`app/ponto/services/fechamento.py:660-665,693-701`).

LEU — Fluxo atual:

| Dado | Escritor/origem | Leitor/efeito |
|---|---|---|
| Dias justificados | Todas as `Justificativa.aprovada.data_turno`, sem ler tipo: `app/ponto/services/efeito_ausencia.py:40-45`; cópia independente em `app/ponto/services/fechamento.py:660-665` | PDF `app/relatorios/pdf_espelho.py:334,396`; espelho `app/ponto/services/espelho.py:1114`; motor armazena set em `app/ponto/motor_calculo_v2.py:796` |
| Minutos parciais | Soma de `Ausencia.minutos`, após juiz de parcial: `app/ponto/services/efeito_ausencia.py:47-50` | PDF soma `/60` a `horas_abono`: `app/relatorios/pdf_espelho.py:479` |
| Crédito por dia | Outra porta, apenas Ausência: `app/ponto/services/credito_parcial.py:15-29` | Abate atraso próprio dia em folha: `app/ponto/services/fechamento.py:994-1001`; chega à lavratura em `app/ponto/services/fechamento.py:1281-1282`; aplicado em `app/ponto/services/dia_pago.py:188-195` |
| Abono persistido | `por_dia_da_grade`: somente `tipo_dia == 'ausencia'`, previsto menos realizado limitado ao previsto: `app/ponto/services/dia_pago.py:386-402` | Fecha total em `app/ponto/services/fechamento.py:1060-1064`; PDF/espelho competência leem `minutos_abonados/60` pela folha/lavratura: `app/relatorios/cartao_pela_celula.py:118,186` |
| Dias/horas de abono do motor | Dias previstos sem período de trabalho, presentes em datas justificadas: `app/ponto/motor_calculo_v2.py:2606-2622` | PDF `app/relatorios/pdf_espelho.py:478-479`; espelho badge `app/ponto/services/espelho.py:1252` |
| Falta apurada | Motor: dias obrigatórios sem trabalho nem justificativa; soma previsto diário `/60`: `app/ponto/motor_calculo_v2.py:2600-2619` | Alimenta “furo apurado” no espelho: `app/ponto/services/espelho.py:1249-1251` |
| Falta decidida da folha | Ausências do catálogo que desconta, rateadas pela expansão do registro: `app/folha/export.py:95-143` | Folha `app/ponto/services/fechamento.py:873-875,1003`; resumo distingue falta/furo: `app/relatorios/cartao_pela_celula.py:156-167` |
| DSR | Motor base `app/ponto/motor_calculo_v2.py:2571-2655`; override comercial reexecuta `app/ponto/motor_calculo_v2.py:3457-3503` | Folha copia semanas e reflexo em `app/ponto/services/fechamento.py:1101-1103` |

LEU — `por_dia_da_grade` também é consumida pela porta de retrato da exportada (`app/ponto/services/dia_pago.py:464`); ela não lê `justificado`, somente o tipo do dia (`app/ponto/services/dia_pago.py:387-400`). A grade atual conserva `tipo_dia='trabalho'` ao marcar a justificativa (`app/escala/utils.py:1254-1259`). INFERIU — mudar exclusivamente a soma no PDF deixa a folha sem o pagamento e pode ser sobrescrito pelo resumo da competência (`app/relatorios/cartao_pela_celula.py:118,186`; `app/ponto/services/espelho.py:1283-1284`).

DECISÕES de implementação/semântica a entregar, sem decidir:

1. **Período versus crédito de atraso:** o parcial existente significa crédito que abate atraso do próprio dia (`app/ponto/services/credito_parcial.py:50-60`); o campo que o PDF chama abono é uma soma mensal (`app/relatorios/pdf_espelho.py:479`). Candidatas: transportar período identificado por dia/instantes e produzir os efeitos próprios; ou usar mapa diário de minutos, declarando a informação perdida. Não tratar implicitamente período inteiro como atraso.
2. **Abono de dia sem trabalho versus período coberto com batida:** motor abona apenas dias sem trabalho (`app/ponto/motor_calculo_v2.py:2606-2622`), enquanto a folha abona complemento do previsto em dia de ausência (`app/ponto/services/dia_pago.py:393-401`). Candidatas: distinguir cobertura integral de crédito parcial na entrada; ou transportar uma estrutura comum que explicite ambos. Não basta acrescentar uma data ao set.
3. **Dia da jornada versus dia civil local:** DSR base usa mapa da jornada (`app/ponto/motor_calculo_v2.py:2577-2579`); override comercial usa `localizar(p.entrada).date()` (`app/ponto/motor_calculo_v2.py:3465`). Crédito de atraso também usa dia civil local (`app/ponto/services/credito_parcial.py:32-36`), mas lavratura usa mapa da jornada (`app/ponto/services/dia_pago.py:128-132,174`). Candidatas: alinhar chaves ao dia soberano já fornecido ou declarar conversão na fronteira; efeito real no dado fornecido [nao verificado].
4. **Furo apurado versus falta decidida:** não são o mesmo campo de negócio; motor produz apuração (`app/ponto/motor_calculo_v2.py:281-283,2600-2619`), folha exporta decisão do catálogo (`app/folha/export.py:125-143`). Candidatas: preservar nomes distintos nas medições ou publicar os dois explicitamente; usar um para validar o outro confundiria universos.
5. **Dia com abono:** motor conta apenas dia sem trabalho justificado (`app/ponto/motor_calculo_v2.py:2606-2620`); lavratura conta qualquer linha diária com minutos abonados positivos (`app/ponto/services/dia_pago.py:687-699`). Candidatas: manter dois indicadores nomeados ou fazer o leitor pedir a autoridade adequada ao rótulo.

LEU — Juízes documentados: motor/CCT (`app/docs/JUIZES.md:29-34`), janela (`app/docs/JUIZES.md:35-36`), cobertura Ausência (`app/docs/JUIZES.md:55-58`). A duplicação de lista de justificadas no fechamento deriva por conta própria (`app/ponto/services/fechamento.py:660-665` versus `app/ponto/services/efeito_ausencia.py:40-45`); não consulta um juiz do marco. A soma de abono no PDF deriva localmente (`app/relatorios/pdf_espelho.py:479`); o abono persistido tem outra origem (`app/ponto/services/dia_pago.py:364-402`).

INFERIU — Além dos arquivos S3 fornecidos, o construtor precisa ao menos resolver o consumo em `app/ponto/motor_calculo_v2.py:2600-2622`, os três alimentadores `app/ponto/services/espelho.py:1111-1115`, `app/relatorios/pdf_espelho.py:394-398`, `app/ponto/services/fechamento.py:736-741`, e a origem do abono persistido `app/ponto/services/dia_pago.py:364-402`. `credito_parcial.py` depende da decisão sobre abatimento (`app/ponto/services/fechamento.py:994-1001`). Alterar TXT diretamente não se justifica por este rastreio: o exportador lê falta decidida e resultados da folha (`app/folha/export.py:740-744`; `app/relatorios/cartao_pela_celula.py:89-118`).

AST foi executado por `python3 -B`, sem importar app. Resultado específico de `min_parcial_pagos`: escrita de chave `app/ponto/services/efeito_ausencia.py:64`; leitura de chave `app/relatorios/pdf_espelho.py:336`; nenhuma leitura dessa chave nos outros seis arquivos examinados pelo script — `espelho.py`, `fechamento.py`, `motor_calculo_v2.py`, `cartao_pela_celula.py`, `dia_pago.py`, além do produtor. Ausência além desse universo depende do censo global do pai [nao verificado].

## 13/11 18:10 · sessao 380f2191aded · - · - · origem=Ronald (tmux)

**LEU: o ponto do complemento não existe ainda no HEAD.** `vigiar_fio_morto.py:176` é `with tranca_em_lote()`, e o caminho chama `censo_alcance` em `:178`; o censo usa `alimentar_portas` em `vigia_fio.py:609` e `porta_alcancavel_do_dia` em `:640`. AST encontrou **zero** chamadas `carga_da_tarefa` dentro de `censo_alcance` (`app/chamados/services/vigia_fio.py:542`). Assim, o último leitor da carga no caminho **pós-E3** permanece **[nao verificado]**; não se deve transplantar liberação pela linha antiga do anexo. O registro vivo afirma que os cinco respondedores seguem vivos até E3 e nenhum leitor migrou em E2 (`app/core/juizes.py:150`; `app/docs/JUIZES.md:85`).

**Origem das provas locais do complemento:** backlog registra literalmente pós-E3, sem construção agora, soltar carga, DIFF zero e RSS esperado <=1500 (`app/docs/BACKLOG.md:738`); o RELATO registra os números RSS fornecidos no anexo (`app/docs/RELATO.md:14`) e o recebimento do complemento sem mudar E3 (`app/docs/RELATO.md:5`). Reproduzir esses números no HEAD ou atribuir o pico atual à carga permanece **[nao verificado]**.

**Censo AST executado, produção Python, sem importar app** (busca sintática por nomes; `_carimbar` de `aplicar_janela_he_total` foi falso homônimo, excluído manualmente após leitura; `app/ponto/management/commands/aplicar_janela_he_total.py:75`):

```text
app/chamados/juizes.py:1576 CALL alimentar_fila
app/chamados/juizes.py:1656 CALL alimentar_portas
app/chamados/management/commands/backfill_fio_morto_e4b.py:78 CALL censo_alcance
app/chamados/management/commands/backfill_fio_morto_e4b.py:87 CALL T.tarefa_do_dia
app/chamados/management/commands/diff_e3_sombra.py:114 CALL alimentar_portas
app/chamados/management/commands/diff_e3_sombra.py:115 CALL carga_da_tarefa
app/chamados/management/commands/diff_e3_sombra.py:125 CALL tarefa_do_dia
app/chamados/management/commands/vigiar_fio_morto.py:132 CALL caminho_do_carimbo
app/chamados/management/commands/vigiar_fio_morto.py:178 CALL censo_alcance
app/chamados/management/commands/vigiar_fio_morto.py:232 CALL _carimbar
app/chamados/services/tarefa_do_dia.py:569 CALL carga_da_tarefa
app/chamados/services/tarefa_do_dia.py:529 CALL alimentar_fila
app/chamados/services/vigia_fio.py:609 CALL alimentar_portas
app/chamados/services/vigia_fio.py:640 CALL porta_alcancavel_do_dia
app/core/management/commands/pipeline_placar.py:92 CALL caminho_do_carimbo
app/escala/services/cadastro_realidade.py:456 CALL alimentar_portas
app/escala/services/cadastro_realidade.py:460 CALL porta_alcancavel_do_dia
app/folha/fora_da_folha.py:62 CALL alimentar_portas
app/folha/fora_da_folha.py:67 CALL porta_alcancavel_do_dia
```

O script usado para essa parte foi:

```python
import ast
from pathlib import Path

wanted = {
    "carga_da_tarefa", "tarefa_do_dia", "_carimbar",
    "caminho_do_carimbo", "censo_alcance",
    "alimentar_portas", "alimentar_fila", "porta_alcancavel_do_dia",
}
for p in sorted(Path("app").rglob("*.py")):
    if "tests" in p.parts or p.name.startswith("test"):
        continue
    try:
        tree = ast.parse(p.read_text())
    except (SyntaxError, UnicodeError):
        continue
    for node in ast.walk(tree):
        if not isinstance(node, ast.Call):
            continue
        f = node.func
        name = (f.id if isinstance(f, ast.Name)
                else f.attr if isinstance(f, ast.Attribute) else "")
        if name in wanted:
            print(f"{p}:{node.lineno} CALL {ast.unparse(f)}")
```

**Juízes e derivações:**

- `tarefa_do_dia` está no lookup e no registro vivo (`app/docs/JUIZES.md:78`; `app/core/juizes.py:295`). A carga é seu lote, com ação pura e relógio de borda (`app/chamados/services/tarefa_do_dia.py:562`).
- `porta_alcancavel_do_dia` está registrado como `_C17`, com pergunta diferente: chegar ao dia pela fila/app (`app/core/juizes.py:132`; `:293`). Seu corpo pede chamado da célula na fila ou pergunta viva no app ancorada na fila (`app/chamados/juizes.py:1622`).
- `censo_alcance` conta e categoriza a saída do juiz; documenta que não deriva porta (`app/chamados/services/vigia_fio.py:542`), mas suas próprias categorias e decisões a/b/c/d/e são derivadas em `:668`–`:704`.
- `ListaDoAdmin` lê lista e dias lavrados e memoriza o resultado, incluindo falha (`app/chamados/services/vigia_fio.py:205`; `:222`).
- `_resolver_da_rodada` junta dias por colaborador e consulta `resolver_dia_oferecido_nas_datas`; não cria juiz novo (`app/chamados/services/vigia_fio.py:258`).
- `_carimbar` é escritor de fotografia, e não juiz; serializa o registro inteiro em `.novo` e faz `os.replace` (`app/chamados/management/commands/vigiar_fio_morto.py:120`).

**Campo/semântica com decisões para o construtor:**

1. **RSS e operador. LEU:** leitor genérico do ESTADO compara todos os `esperado` por **igualdade**, `censo.get(k) != v` (`bin/fio_estado.py:125`). O comando monta `gritou` verificando verdade dos contadores, sem operador de limite (`app/chamados/management/commands/vigiar_fio_morto.py:186`). **INFERIU:** gravar `esperado.pico_rss_mb=1500` nesse esquema fará 1237 aparecer fora do esperado, embora complemento peça <=1500. **DECISÃO:** contrato de memória separado com limite/operador explícitos, ou extensão dos leitores para comparadores tipados; não escolher silenciosamente nem alterar igualdade dos contadores existentes (`bin/fio_estado.py:125`; `app/docs/BACKLOG.md:738`).

2. **Pico/processo/rodada/unidade. LEU:** HEAD `_carimbar` recebe um registro e só o serializa; não coleta RSS (`app/chamados/management/commands/vigiar_fio_morto.py:120`). **DECISÃO:** especificar no instrumento fonte de RSS, conversão exata para MB, janela de acumulação, instante da amostra e identificação do processo/rodada; MB decimal versus MiB e disponibilidade da fonte permanecem **[nao verificado]**. Complemento já escolheu unidade de trabalho rodada e pico do próprio processo; não perguntar isso como lei nova (`app/docs/BACKLOG.md:738`; anexo complementar **[nao verificado: localização integral do aval fora da cópia]**).

3. **Soltar carga e medir pico. LEU:** carga materializa dias de toda a janela para todos os colaboradores (`app/chamados/services/tarefa_do_dia.py:420`) e retém esses pares em `self._pares` (`:427`); `_ler_vida` percorre os pares (`:458`). **INFERIU:** liberação depois do último uso resolve retenção posterior, mas ganho no pico da rodada não pode ser inferido só do -392MB do anexo. **DECISÃO:** desenhar redução do tempo de vida no chamador pós-E3, ou escopo auxiliar que devolva só resultados; demonstrar último leitor e medir RSS retido e pico separadamente. O caminho futuro e os aliases que reterão a carga estão **[nao verificado]**.

4. **Janela. LEU:** `[ini,fim]` é fechada, perguntas filtradas por `data_evento__gte/__lte` (`app/chamados/services/tarefa_do_dia.py:374`); carga rejeita dia fora de `ini <= dia <= fim` (`:517`). `dia_do_turno` é explicitamente dia do turno; `agora` é instante e a casca rejeita carga com instante diferente (`:562`). Não há divergência nesses contratos lidos; preservá-los ao liberar carga (`:570`).

5. **Histórico no instante. LEU:** a carga corta decisão e efeitos pelo instante, mas documenta leitura de estado atual para identidade da pergunta, ata, cobertura, fila, cadastro, vínculo e tranca (`app/chamados/services/tarefa_do_dia.py:350`). **INFERIU:** tratar carga como fotografia histórica completa seria falso; otimização de tempo de vida não autoriza mudar horizonte ou restringir retângulo (`:354`; `:420`).

6. **Data civil versus instante. LEU:** borda lê `timezone.now()` e deriva `hoje=localdate(agora)`; carimbo grava `localtime(agora).isoformat()` e `hoje.isoformat()` (`app/chamados/management/commands/vigiar_fio_morto.py:168`; `:233`). ESTADO converte offset ao fuso do host para medir idade (`bin/fio_estado.py:129`). Já `pipeline_placar` usa `date.today()` do host e exige igualdade do `hoje` do carimbo com o dia civil julgado (`app/core/management/commands/pipeline_placar.py:94`; `:148`). Fuso efetivo igual nos dois processos é **[nao verificado]**; não ajustar relógios nesta obra sem prova de impacto.

**Contrato existente do carimbo e quem mais lê:**

- Escritor `_carimbar` é chamado apenas em frota e antes de `SystemExit`; o registro contém `medido_em`, `hoje`, banco/schema, esperados, listas de alarmes, gritou, censo e alcance completos (`app/chamados/management/commands/vigiar_fio_morto.py:231`).
- Recorte `--empresa` não carimba; mensagem declara isso (`app/chamados/management/commands/vigiar_fio_morto.py:253`).
- Falha de escrita não é engolida; JSON usa troca atômica (`app/chamados/management/commands/vigiar_fio_morto.py:123`).
- `bin/fio_estado.py::_ler_carimbo` lê censo/esperado/tempo e a manchete do alcance; `linha` publica (`bin/fio_estado.py:121`; `:165`).
- `pipeline_placar.alarmes_fio_mudo` lê mesmo caminho e os campos `hoje`, `censo[nome]`, `esperado[nome]`; acusa ausência/forma errada (`app/core/management/commands/pipeline_placar.py:85`). Só o placar chama os nomes fio_mudo e nunca_bateu, portanto ele ainda não verifica memória (`:135`).
- CI aponta para carimbo temporário para não sobrescrever produção (`app/config/settings/ci.py:31`).
- Cron chama o vigia com `--alarme --alarme-passivo`, teto 900s, e dependências declaradas; não é necessário inferir novo cron para o alarme de memória (`app/config/crons.py:1721`).

**Arquivos comuns, segundo anexo de voo [nao verificado: não encontrei arquivo local correspondente]:**

- Necessário para instrumento RSS: `app/chamados/management/commands/vigiar_fio_morto.py` é compartilhado com `wt-bugpar-124848`, `wt-codex-hj-090-152256`, `wt-e3-b6-114947`.
- Provável para liberação pós-E3: `app/chamados/services/vigia_fio.py` está em `wt-bugpar-124848`. Local preciso pós-E3 **[nao verificado]**.
- Se desenho da liberação pedir alterar o juiz/lote: `tarefa_do_dia.py` cruza `wt-bugpar-124848` e `wt-e3-b6-114947`; isto ampliaria escopo, porque complemento pede soltar carga no chamador (`app/docs/BACKLOG.md:738`).
- Juiz `chamados/juizes.py` cruza `wt-bugpar-124848`, `wt-o168v6-233014`, `wt-e3-b6-114947`; nenhuma mudança nele decorre necessariamente do complemento.
- `config/crons.py` cruza `wt-bugpar-124848` e `wt-codex-hj-090-152256`; só precisa tocar se mudar encadeamento/opções/teto, o que o complemento não pede (`app/config/crons.py:1721`; `app/docs/BACKLOG.md:738`).
- Se usar selo existente `test_o261_vigia_fio.py`, cruza bugpar/HJ090/E3; não executar esse teste nesta preparação (`app/docs/BACKLOG.md:739`).

**Brief complementar sugerido:** pós-E3, confirmar censo e último leitor; reduzir só retenção da carga no chamador; RED mede antes/depois e DIFF completo do vigia zero; RSS é instrumento separado, contrato antes, unidade/operador/falha/quem-lê explícitos; não usar igualdade `esperado=1500`; não mudar autoridades, retângulo de carga, horizonte `agora`, contadores de negócio, alarme passivo ou encadeamento; medir pico e retenção no caminho real, incluindo aliases e partes após o censo (`app/docs/BACKLOG.md:738`; `app/chamados/services/tarefa_do_dia.py:350`; `bin/fio_estado.py:125`; `app/chamados/management/commands/vigiar_fio_morto.py:176`).

## 13/11 18:10 · sessao 83dfb72e585c · - · - · origem=Ronald (tmux)

LEU — pontos críticos para consolidar:

| Campo/contrato | Escritor e leitores no HEAD |
|---|---|
| `Justificativa.data_turno` | `DateField`, “data do turno”: `app/ponto/models.py:144`. API recebe data do payload e escreve: `app/api/views.py:1948`, `app/api/views.py:1995`; web procura primeira entrada pelo dia civil `timestamp__date`: `app/ponto/views.py:1598`; regularização usa dia local da entrada: `app/ponto/services/regularizacao.py:73`. |
| `tipo` | Choices em `app/ponto/models.py:116`; web aceita texto e escreve em `app/ponto/views.py:1591`, `app/ponto/views.py:1625`; API escreve em `app/api/views.py:1986`; cron automático grava `esqueci_saida`: `app/ponto/management/commands/processar_alertas_turno.py:399`. |
| `saida_informada` / `batida_entrada` / `batida_gerada` | Saída é instante completo, suporta madrugada: `app/ponto/models.py:149`; entrada é FK do turno: `app/ponto/models.py:139`; gerada é FK única da saída materializada: `app/ponto/models.py:167`. Web lê texto e torna aware: `app/ponto/views.py:1606`; regularização grava entrada e saída: `app/ponto/services/regularizacao.py:93`; API não entrega esses dois campos na criação: `app/api/views.py:1992`. |
| `status` | Aprovação avulsa e materialização: `app/ponto/services/justificativa.py:205`, `app/ponto/services/justificativa.py:383`; lote aprova antes de materializar saída no mesmo atomic: `app/ponto/views.py:1796`; rejeição avulsa/lote: `app/ponto/services/justificativa.py:283`, `app/ponto/views.py:1875`; regularização recoloca pendente: `app/ponto/services/regularizacao.py:105`; auto-rejeições: `app/ponto/services/entrada_sincronizada.py:41`, `app/ponto/services/triagem_batida.py:608`, `app/chamados/reconciliador.py:533`. |
| Aprovação → cartório | Todo save da justificativa dispara julgamento no seu `data_turno`, independentemente de status: `app/ponto/signals.py:162`, `app/ponto/signals.py:165`; aprovação invalida fechamento: `app/ponto/services/justificativa.py:214`. |
| `justificado` da grade | `_marcar_ausencias` consulta somente datas aprovadas, sem tipo: `app/escala/utils.py:1197`; limpa todos os missing, apaga órfãs e escreve True: `app/escala/utils.py:1254`. |
| `delta` | Escritor `_celula_do_marco` usa diferença entre instantes absolutos, arredondada em minutos: `app/escala/utils.py:325`; leitor da ata refaz diferença de HH:MM pela menor distância ±720: `app/escala/services/leitor_celula.py:14`, `app/escala/services/leitor_celula.py:32`. |
| `acesa`, `luz`, `dispensada` | Ata traduz células em lâmpadas: `app/ponto/services/cartorio.py:711`, `app/ponto/services/cartorio.py:722`; dispensa significa acesa True, luz None e motivo, escrito por `dispensar_por_ausencia`: `app/ponto/services/cartorio.py:807`; leitor preserva `dispensada`: `app/escala/services/leitor_celula.py:145`. |
| `justificado` reconstruído | `_justificado_lavrado` usa **qualquer** lâmpada dispensada por justificativa: `app/escala/services/leitor_celula.py:81`; `grade_da_celula` escreve True: `app/escala/services/leitor_celula.py:439`; `supra_juiz.classificar_dia` usa True para retirar acusações FURO_*: `app/ponto/supra_juiz.py:371`. |

INFERIU — consequências de portar literalmente o diff:

1. **Dispensar um marco viraria novamente dia inteiro justificado no leitor da ata.** A cadeia é `any(dispensada='justificativa')` → `dia.justificado=True` → retirada de FURO_*. Exigir RED pela grade reconstruída da ata, além da grade fresca. Fontes: `app/escala/services/leitor_celula.py:81`, `app/escala/services/leitor_celula.py:439`, `app/ponto/supra_juiz.py:371`.

2. **O diff usa contrato temporal já superado.** `_marcar_ausencias` hoje recebe colaborador e mapa de deslocamentos, não marcos: `app/escala/utils.py:1156`. Os builders entregam os deslocamentos em `app/escala/utils.py:807`, `app/escala/utils.py:1848`. A coluna não é cadeia cronológica do turno; a própria função documenta E19/S02/E03/S07 contra régua E03/S07/E19/S02: `app/escala/utils.py:353`. A origem existente é `deslocamentos_na_cadeia`, que casa coluna com instante já encadeado: `app/escala/utils.py:366`. `horas_reais_do_dia` documenta explicitamente que reencadear régua foi removido: `app/ponto/turnos.py:2655`.

3. **A guarda anti-push do diff duplicaria um predicado que já nasceu no HEAD.** `parcial_so_lista` lê carimbos e `houve_fato_novo`: `app/ponto/services/cartorio.py:398`, `app/ponto/services/cartorio.py:423`. Os portões atuais passam instantâneo anterior do julgamento e o mesmo `agora`: `app/ponto/services/cartorio.py:1757`. A escolha do construtor é compor o alcance da justificativa nessa porta ou criar representação específica que ela leia; copiar a guarda do diff perde a evolução do instantâneo/relógio. Fontes: `app/ponto/services/cartorio.py:417`, `app/ponto/services/cartorio.py:1757`.

Divergências a apresentar como DECISÃO de contrato, sem decidir:

- Data de payload/dia civil versus dia local da entrada/dia do turno: transportar a instância do marco/turno existente ou continuar contrato por data com alcance explicitado; sítios `app/api/views.py:1948`, `app/ponto/views.py:1598`, `app/ponto/services/regularizacao.py:73`, `app/ponto/models.py:144`.
- `delta` absoluto arredondado versus HH:MM com menor distância: preservar valor/proveniência lavrada ou assumir formalmente domínio limitado; sítios `app/escala/utils.py:325`, `app/escala/services/leitor_celula.py:32`. O dado de produção fornecido não permite medir casos divergentes. [nao verificado]
- Ausência de missing versus dispensa versus batida real: usar carimbo de cobertura integral explícito ou composição da cobertura na autoridade; manter “qualquer dispensa” deixa significado incorreto para cobertura parcial. Sítios `app/escala/utils.py:1256`, `app/ponto/services/cartorio.py:807`, `app/escala/services/leitor_celula.py:81`.
- Período/valor não legível versus zero devido: a raia retorna zero quando falta delta [nao verificado: implementação só no anexo]; o HEAD trata hora ilegível como None e distingue `nao_sei`: `app/ponto/turnos.py:2624`, `app/ponto/turnos.py:2663`. Falta definir como o dinheiro transportará essa falta de proveniência, sem inventar período.

Juízes:

- JUIZES lista marcos soberanos na célula e casamento na grade: `app/docs/JUIZES.md:22`, `app/docs/JUIZES.md:27`; cobertura de ausência: `app/docs/JUIZES.md:55`.
- O nascimento de `marcos_cobertos_por_justificativa` está autorizado por escrito: `app/docs/CORTES.md:319`, `app/docs/CORTES.md:320`; ainda não aparece no bloco vivo de ausência onde o diff pretende inseri-lo: `app/core/juizes.py:251`, `app/core/juizes.py:261`. A ausência integral da função foi encontrada por busca, sem uma linha positiva para citar. [nao verificado]
- `_justificado_lavrado` e `_delta_min` derivam por conta própria: respectivamente any de motivos e distância circular das horas: `app/escala/services/leitor_celula.py:81`, `app/escala/services/leitor_celula.py:32`.

Arquivo adicional necessário ao desenho e ausente de S3: **`app/escala/services/leitor_celula.py`**, por semântica de `justificado` e transporte dos campos novos; evidência em `app/escala/services/leitor_celula.py:439`. Não consta do anexo arquivos_em_voo. [nao verificado: comparação com anexo textual]

Escritores de justificativa são dependências para medir; não inferir automaticamente necessidade de editar API/views: a cobertura pode ser corrigida na origem sem mudar emissão. Fontes dos escritores: `app/api/views.py:1992`, `app/ponto/views.py:1635`, `app/ponto/services/regularizacao.py:91`, `app/ponto/management/commands/processar_alertas_turno.py:394`.

## 13/11 18:10 · sessao 4b2bbf1f4bdb · - · - · origem=Ronald (tmux)

LEU os trechos abaixo na cópia; a identificação deles como HEAD Git não foi verificada por mim. [nao verificado]

**Semântica de férias e espelho**

| Campo/parâmetro | LEU: escritores e leitores | Divergência / decisão a deixar ao construtor |
|---|---|---|
| Identidade da origem | O espelhador recebe UM agendamento, mas seleciona qualquer `Ausencia(tipo='ferias')` do colaborador cuja `data` esteja na janela, e pula todo dia encontrado, sem verificar origem nem status. A descrição escrita contém `#pk`. `app/ponto/services/ausencia.py:1161`, `:1181`, `:1183`, `:1200`, `:1207`. | **DECISÃO técnica:** como transmitir a identidade do agendamento ao produtor genérico: parâmetro interno ou caminho de criação específico. A exigência de não reaproveitar outro agendamento vem do complemento fornecido; implementação ainda [nao verificado]. |
| Reabertura indireta | `criar_ausencia` busca rejeitada por `(colaborador,tipo,data)` e retorna `_reabrir`, sem identidade de agendamento. `app/ponto/services/ausencia.py:254`, `:291`. | **INFERIU:** filtrar somente `existentes` por FK não fecha a obra: o criador pode reabrir rejeitada de outro agendamento. **DECISÃO:** propagar a identidade à busca/reabertura ou impedir esse reaproveitamento no caminho do espelho. RED próprio: outra origem + rejeitada no mesmo dia. `app/ponto/services/ausencia.py:254`, `:291`, `:1203`. |
| `status` de Ausencia versus agendamento | Agendamento ativo para o espelho significa `agendado/em_gozo/concluido`. Qualquer Ausencia existente, inclusive rejeitada, satisfaz o espelho. Cobertura para folha exige aprovada; para cobrança basta não rejeitada. `app/ponto/services/ausencia.py:1181`, `:1183`, `:1200`; `app/ponto/turnos.py:2511`, `:2516`, `:2518`. | **DECISÃO de desenho:** no fluxo futuro, presença de linha rejeitada da mesma origem será apenas denunciada pelo contador ou será escrita pelo agendamento em ato próprio? O complemento manda listar o passivo e proíbe correção automática; não converter a introdução da FK em cura histórica. Fonte do mandato: anexo `complemento_hj_091.txt:4` (numeração por linha textual fornecida). |
| Cancelamento/rejeição | A tela “cancelar Ausencia” faz flip para `rejeitada`; cancelar agendamento muda seu status e o espelho apaga as ausências selecionadas. Cancelamento admin primeiro rejeita a janela e depois salva cancelado, disparando remoção. `app/ponto/views.py:2721`, `:2734`; `app/ferias/services.py:384`, `:385`, `:411`, `:417`; `app/ponto/services/ausencia.py:1189`. | **DECISÃO técnica:** concentrar a remoção por identidade no espelho ou manter auxiliar chamado pelo agendamento com identidade explícita. Não confundir o status “cancelado” do agendamento com o estado “rejeitada” da Ausencia. `app/ferias/models.py:293`; `app/ponto/views.py:2721`. |
| Janela versus propriedade | Espelho cancelado apaga somente na janela corrente. `delete()` do agendamento e auxiliares também selecionam por colaborador/tipo/data, sem origem. `app/ponto/services/ausencia.py:1183`, `:1189`, `:1263`, `:1274`; `app/ferias/models.py:453`. | **INFERIU:** linha deslocada para fora da janela sobrevive; linha de outra origem dentro da janela pode ser apagada/rejeitada. **DECISÃO:** como compatibilizar operação por FK com restos legados sem FK; listar irresolvidos, sem adivinhar origem por coincidência de data. `app/ponto/services/ausencia.py:1183`, `:1189`, `:1267`, `:1282`. |
| `_minhas` na guarda | O comentário diz “Ausencias que ESTE agendamento materializou”; a consulta considera todas as férias do colaborador na janela antiga e as exclui do veto. `app/ferias/models.py:422`, `:430`, `:435`. | **DECISÃO técnica:** excluir somente IDs com FK deste agendamento ou criar mecanismo de alimentação equivalente para o veto. Com FK, preservar o critério atual por janela continuaria dispensando possíveis linhas de outra origem. `app/ferias/models.py:430`, `:437`. |
| Remarcação e `exceto` | `_trocar_janela` é declarado escritor único das datas; lê agendamento/PA sob trava, calcula saldo, apaga trecho antigo fora da janela nova e salva novo intervalo. Apagador não recebe agendamento. `app/ferias/services.py:422`, `:434`, `:436`, `:445`, `:450`; `app/ponto/services/ausencia.py:1263`. | **DECISÃO técnica:** acrescentar identidade ao auxiliar ou delegar a limpeza ao espelho, preservando trava/atomic e ordem documentada. A mudança cruza `app/ferias/services.py`, que consta da HJ-090 no anexo do usuário. `app/ferias/services.py:423`, `:425`; anexo `arquivos_em_voo.txt` [numeração não calculada por mim]. |
| `data_inicio/data_fim/dias` | Datas do agendamento são `DateField`; dias são corridos, inclusivos: `(fim-inicio).days+1`. `clean` confere coerência; criação deriva fim por `dias-1`. `app/ferias/models.py:309`, `:310`, `:369`, `:380`; `app/ferias/services.py:313`. | **Sem divergência lida** sobre unidade ou inclusão. Não transformar dias em úteis nem usar fim exclusivo. `app/ferias/models.py:380`; `app/ferias/services.py:313`. |
| Dia de Ausencia versus período inteiro | Espelhador cria uma Ausencia diária com `dias_corridos=1`, `minutos=0`, sem `data_fim`. Painel busca irmãos por documento e mostra intervalo desses irmãos ou data/fim da Ausencia. `app/ponto/services/ausencia.py:1203`; `app/ponto/views.py:2073`; `app/templates/ponto/partials/painel_ausencia.html:50`. | **DECISÃO de apresentação:** obter período/dias/status do agendamento ligado, mantendo a data diária como dia consultado. Não escrever período inteiro nos campos da Ausencia diária apenas para servir ao painel: alteraria significado consumido pelos juízes. `app/ponto/turnos.py:2538`, `:2554`; `app/ponto/models.py:894`. |
| `data_fim=NULL` | Help text genérico diz “indeterminado/em aberto”; leitor explica que NULL significa dia único fora de `TIPOS_EM_ABERTO`. `app/ponto/models.py:751`; `app/ponto/turnos.py:3150`. | **DECISÃO de texto/contexto:** painel específico de férias deve distinguir ausência diária do período do agendamento; sem derivar “indeterminado” pelo NULL isolado. `app/ponto/turnos.py:3150`; `app/ponto/services/ausencia.py:1204`. |
| Status do agendamento versus datas | `status_por_data` deriva agendado/em_gozo/concluído; cancelado é decisão humana fora da função. `ferias_na_janela` usa interseção inclusiva das datas e exclui só cancelado, inclusive com status concluído em data viva. `app/ferias/models.py:273`; `app/ferias/services.py:47`, `:57`. | **Sem lei nova:** contador do par deve usar o juiz de férias na janela como testemunha, evitando `ATIVOS=('agendado','em_gozo')` como substituto para cobertura histórica. `app/ferias/services.py:44`, `:47`, `:58`. |
| Hoje / fuso | Defaults de `status_por_data`, `em_gozo_hoje` e remarcação usam `datetime.date.today()`; cron passa `timezone.localdate()`. Configuração base é São Paulo e `USE_TZ=True`. `app/ferias/models.py:280`; `app/ferias/services.py:62`, `:438`; `app/ferias/management/commands/sincronizar_status_ferias.py:27`; `app/config/settings/base.py:93`, `:95`. | **DECISÃO de contrato para nova sonda:** hoje explícito proveniente de qual borda, se ela precisar de “hoje”? Datas concretas da janela dispensam isso. **INFERIU:** APIs de data podem divergir perto da meia-noite se fuso do processo e Django diferirem; fuso do processo em produção [nao verificado]. Não ampliar HJ091 para corrigir relógios existentes sem mandato. |
| Dia civil versus turno | Folha cruza timestamp de batida por `timezone.localdate` com datas inclusivas do agendamento; ausências cobrem a data fornecida. Parcial noturna tem contrato distinto: madrugada é deslocamento do dia inicial do turno. `app/folha/export.py:491`, `:496`, `:500`; `app/ponto/turnos.py:2554`, `:2627`, `:2630`. | **DECISÃO de explicitação:** contadores do vínculo diário devem comparar datas do agendamento/Ausencia, sem reinterpretar a regra de batida como dia do turno. Folha está expressamente fora da mudança no complemento. Fonte do mandato: anexo `complemento_hj_091.txt:2`. |
| FK nullable / remoção da origem | Há `AgendamentoFerias.delete()` que limpa janela e depois remove agendamento; ainda não existe FK de férias na definição lida de Ausencia. `app/ferias/models.py:453`; `app/ponto/models.py:743`, `:786`, `:791`. | **Possível lacuna de alcance**, a submeter ao exame das leis: `on_delete=PROTECT`, `SET_NULL` ou `CASCADE` têm efeitos diferentes sobre prova da origem e linhas deslocadas. Não escolhi. Se lei escrita já responder, vira implementação, não pergunta de lei. |

**Pontos adicionais para o brief**

- O painel atual só oferece evidência de grupo documental, não de agendamento; `#N` é texto exibido. `app/ponto/views.py:2073`, `:2079`; `app/templates/ponto/partials/painel_ausencia.html:50`, `:57`.
- O selo precisa seguir o alcance indireto de criação/reabertura, não somente atribuições no espelhador: `criar_ausencia → _reabrir` pode escapar da nova identidade. `app/ponto/services/ausencia.py:254`, `:291`.
- `dias_ferias_com_batida` já usa `ferias_na_janela`, recorte da competência, datas civis locais e batidas apuráveis; não precisa consumir FK de Ausencia para responder à pergunta que faz. `app/folha/export.py:484`, `:486`, `:491`, `:499`, `:500`.
- Regra de unicidade da Ausencia continua `(colaborador,tipo,data)` apenas entre não rejeitadas; acrescentar FK não permite presumir coexistência livre de duas linhas vivas de férias no mesmo dia. `app/ponto/models.py:826`.
- “Não reutilizar outra origem” e “não criar sobre fato conflitante” devem coexistir: a criação futura continua passando pelo veto, cuja recusa gera Pauta DP. `app/ponto/services/ausencia.py:1164`, `:1203`, `:1210`, `:1216`.
- A lista raspada tem caso com data `0026-10-07`; validade ou causa desse valor não foi examinada no código/banco. Anexo `passivo.r` [numeração não calculada por mim]; causa [nao verificado].

## 13/11 18:10 · sessao 4922a0386829 · - · - · origem=Ronald (tmux)

LEU no código da cópia; não houve escrita. `AGENTS.md` não apareceu na busca por nome; inexistência universal [nao verificado]. HEAD Git não verificável nesta árvore sem `.git` [nao verificado].

**Portas/escritores confirmados, linhas atuais**

| Porta | Caminho efetivo e alcance |
|---|---|
| Editar | Busca apenas PK (`app/ponto/views.py:2761`), chama editor (`:2794`); editor altera tipo/data/fim/dias/descrição/minutos e salva (`app/ponto/services/ausencia.py:498`, `:509`). Não há guarda de origem nessa sequência. |
| Cancelar individual/lote | Seleciona qualquer não rejeitada e decide rejeitada, preservando/prefixando descrição (`app/ponto/views.py:2728`, `:2729`, `:2734`; lote `:2345`, `:2351`). |
| Rejeitar individual/lote | Seleciona abertas e passa pelo decisor (`app/ponto/views.py:2687`, `:2701`; lote `:2316`, `:2319`). Individual também alcança irmãos por `documento_referencia` (`:2688`, `:2690`). |
| Reverter rejeição | Busca apenas PK e chama reversor (`app/ponto/views.py:2583`, `:2584`), que chega a `_reabrir` (`app/ponto/services/ausencia.py:1103`). |
| Aprovar individual/lote | Busca abertas, gate documental e decisão; sem filtro de origem (`app/ponto/views.py:2606`, `:2608`, `:2643`; lote `:2282`, `:2284`, `:2290`). |
| Anexar web | Busca qualquer PK, chama anexador (`app/ponto/views.py:2140`, `:2151`); grava documento e pode mudar estado (`app/ponto/services/ausencia.py:858`, `:860`, `:875`). |
| Editar período | Busca aprovada e chama decisor (`app/ponto/views.py:2670`, `:2673`); só passa se juiz a julgar parcial (`app/ponto/services/ausencia.py:888`). |
| Lançar/submeter web | Tipo vem do POST (`app/ponto/views.py:2378`, `:2476`), criador chamado em `:2415` / `:2536`; lançar força aprovada (`:2420`). |
| API criar/reenvio | Tipo recebido (`app/api/views.py:1705`), cria (`:1805`), reenvio existente pode anexar (`:1833`, `:1838`). |
| API documento | Busca PK+colaborador (`app/api/views.py:3069`), exige aberta (`:3079`), chama anexador (`:3108`). |
| Veredito célula | Cria pelo mesmo produtor (`app/ponto/services/veredito_celula.py:337`). |
| IA | Escritor genérico cria (`app/ponto/services/checar_ausencia.py:46`), mas prévia normal recusa férias (`:21`). |

**Achado de desvio indireto relevante:** filtrar existentes do espelho pela nova FK não basta para garantir “não reaproveitar outro agendamento”: `criar_ausencia` procura rejeitada por pessoa/tipo/data (`app/ponto/services/ausencia.py:254`) e a reabre antes de criar (`:291`), sem origem. INFERIU: o contrato da nova FK precisa atravessar esse caminho também, preservando o escritor único (`CLAUDE.md:32`).

**AST executado (stdlib, sem importar app)** encontrou chamadas adicionais atuais:

- `curar_ausencia_sem_fim.handle → rederivar_fim` (`app/ponto/management/commands/curar_ausencia_sem_fim.py:46`).
- `rejulgar_status_por_tipo.handle → rejulgar_status_pelo_tipo` (`app/ponto/management/commands/rejulgar_status_por_tipo.py:31`).
- `escalonar_documentos_ausencia.handle → escalonar_aguardando_documento` (`app/ponto/management/commands/escalonar_documentos_ausencia.py:26`).
- `rederivar_minutos_h06_comp11.tratar_linha → editar_ausencia_registro` (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169`).
- `zerar_dados_operacionais.handle → apagar_ausencias_em_ensaio` (`app/ponto/management/commands/zerar_dados_operacionais.py:90`).

Busca AST por nome simples `decidir` também encontrou homônimos em chamados/core; não são prova de chamada ao decisor de Ausencia. Deve resolver imports ou rotular candidatos [nao verificado].

**Campos e leitores do painel**

- Modelo presente tem `tipo`, `data`, `data_fim`, `minutos`, `dias_corridos`, `descricao`, `status`, `documento_referencia`; FK para agendamento ainda não aparece no trecho da classe lido (`app/ponto/models.py:749`, `:750`, `:751`, `:755`, `:761`, `:776`, `:791`, `:799`).
- Painel agrupa por documento, não por férias/origem (`app/ponto/views.py:2074`, `:2077`).
- Exibe intervalo de irmãos, senão range, senão dia (`app/templates/ponto/partials/painel_ausencia.html:50`, `:51`, `:52`); descrição é texto (`:57`).
- Mostra dias da Ausencia (`:123`), origem chamado/documento (`:126`, `:127`); form editar permanece disponível por permissão geral (`:131`, `:134`).
- Form de anexo não tem guarda de tipo no template (`:90`).
- `AusenciaForm.clean_tipo` recusa férias (`app/ponto/forms.py:65`), mas as portas acima passam POST diretamente ao produtor/editor (`app/ponto/views.py:2415`, `:2536`, `:2794`); o veto do form não protege essas chamadas.
- Histórico deriva duração localmente de `data_fim/data` ou `dias_corridos` (`app/ponto/services/dia_decidido.py:616`, `:621`) e mostra descrição (`:624`).

**Divergências/DECISÕES técnicas a levar ao construtor**

1. Origem por agendamento × unicidade pessoa/tipo/dia: constraint atual não permite duas férias vivas no mesmo dia, independentemente da origem (`app/ponto/models.py:827`, `:829`). Opções de encaixe: recusar/listar colisão pela porta existente; outras alternativas que dupliquem viva exigem revisão do contrato, não são consequência automática da nova FK. Não decidi.
2. Nova FK × reabertura genérica: identidade atual para reabrir rejeitada é pessoa/tipo/data (`app/ponto/services/ausencia.py:254`), enquanto aval exige mesmo agendamento [nao verificado no HEAD; anexo]. Opções: parametrizar identidade de origem na porta ou impedir caminho genérico de alcançar derivada; não contornar com escrita direta.
3. Dia consultado × período: tela usa o registro diário e irmãos documentais (`app/ponto/views.py:2074`; template `:50`), enquanto férias deverão mostrar período do agendamento [anexo, não verificado no HEAD]. Opções de apresentação devem preservar dia consultado separadamente; período não deve ser reconstruído contando irmãos documentais.
4. “Cancelada” da UI é `status='rejeitada'` no dado (`app/ponto/views.py:2721`, `:2734`), diferente do vocabulário de AgendamentoFerias; não usar igualdade textual de status entre modelos.
5. `data/data_fim` são DateField (`app/ponto/models.py:750`, `:751`), range inclui ambas pontas (`:899`); horas parciais pertencem ao início do turno quando cruzam meia-noite (`:767`). `aprovada_em` é instante (`:810`, `:873`); não usar esse carimbo para decidir dia de cobertura.

**Lei/alcance:** L-007 + aval “escrita SO pelo agendamento” respondem extensão da proteção a aprovar e anexar derivada: ambos escrevem estado/documento (`CLAUDE.md:32`; `app/ponto/services/ausencia.py:858`, `:875`, `:912`). Não contar como nova pergunta de lei. Legado sem FK deve ser lista, conforme anexo; não inferir “sem FK = avulsa” [nao verificado no HEAD].

**Cruzamentos condicionais com anexo**

- Se guardas forem nas portas API, `app/api/views.py` cruza `wt-bugpar-124848` e `wt-e3-b6-114947` [anexo arquivos_em_voo, não verificável no HEAD].
- Se a proteção ficar na porta canônica de Ausencia, as chamadas API podem permanecer consumidoras sem tocar `app/api/views.py`; escolha técnica [INFERIU de `app/api/views.py:1805`, `:1838`, `:3108`].
- `app/ponto/views.py`, `app/ponto/services/ausencia.py`, `app/ponto/models.py` e painel não aparecem no anexo colado [anexo, não verificável no HEAD].
- Admin já é somente leitura (`app/ponto/admin.py:38`; proibições `app/core/admin.py:59`, `:62`, `:65`); não precisa virar nova porta corretiva.

## 13/11 18:10 · sessao 4be5e8c3e569 · - · - · origem=Ronald (tmux)

LEU no HEAD:

- A OTIMIZA-1 continua registrada como PRONTO 6 construído somente na raia, faltando suite, replay, verificador e pouso próprio; contrato identifica escritor `trava_teste.sh`, leitor `pista_estado.py::ler/linha`, razão/meta e tratamento de ausência. (`app/docs/RELATO.md:1645`, `app/docs/RELATO.md:1648`)
- O gerador HEAD passa diretamente de `recusa_txt` a `codex_txt`, sem seção de pista no molde ou nos argumentos. (`bin/gerar_estado.py:265`, `bin/gerar_estado.py:267`, `bin/gerar_estado.py:333`, `bin/gerar_estado.py:337`, `bin/gerar_estado.py:375`)
- A trava HEAD grava somente seu arquivo `.quem` e executa o comando; não há emissão de `suites.log` nesse corpo. (`bin/trava_teste.sh:40`, `bin/trava_teste.sh:41`, `bin/trava_teste.sh:42`)
- Existência de `.git`, `bin/pista_estado.py`, `bin/suite_impacto.py`, `bin/suite_impacto.sh` e diretório `bin/suite_impacto`: todos `False` na inspeção por `Path.exists`; não há arquivo:linha possível para ausência. **[nao verificado: identidade do HEAD/ref e blobs da raia]**
- Não encontrei juiz de pista/suite/pouso nos registros consultados; “juiz da seleção” do anexo não equivale a juiz registrado. **[nao verificado: ausência universal]** O contrato atribui pousos ao Git. (`app/docs/RELATO.md:1648`)

Mapa de escritores/leitores confirmado:

| Dado/argumento | Origem HEAD | Leitor HEAD / mudança no anexo |
|---|---|---|
| `ESTEIRA_QUEM` | `suite.sh:226`; `regua.sh:165`; `pre-push.sh:142` | `trava_teste.sh:23` gera `QUEM` com PID; anexo remove PID para a coluna | [nao verificado]
| `ONLY` | `suite.sh:56` ou helper `suite.sh:88`; `regua.sh:51` | executor `suite.sh:232`, `regua.sh:168`; guarda efetiva `suite.sh:155` | [nao verificado]
| `LABELS` | fonte única declarada em `regua.sh` | `suite.sh:77`; `pre-push.sh:184`; contrato pista em `RELATO.md:1648` | [nao verificado]
| status/veredito | log do comando | `regua.sh:170`, `regua.sh:178`, `regua.sh:179`; recorte não carimba em `regua.sh:181` |
| identidade certificada | árvore da régua | escritor `regua.sh:196`; leitor `pre-push.sh:65` e `pre-push.sh:68` | [nao verificado]
| `SUITE_ESCOPO`, `SUITES_LOG`, campos de pista | não implementados nos corpos acima | somente anexo **[nao verificado no HEAD]** |

DECISÕES sem decidir:

1. **`escopo`: presença de `--only` versus seleção efetiva.** O anexo diz recorte sempre que `ONLY` não vazio; HEAD já reconhece seleção que cobre a descoberta como inteira, inclusive labels de classe/método. Opções técnicas: reutilizar resultado do julgador existente; transportar descrição mais rica da corrida. Não clonar a decisão. (`bin/suite.sh:133`, `bin/suite.sh:140`, `bin/suite.sh:155`, `bin/suite.sh:173`; comportamento do anexo **[nao verificado no HEAD]**)

2. **Chamadas da raia versus motivo obrigatório.** `_porta` do anexo chama inteira sem `--motivo`; HEAD recusa antes do custo, e desconhece `--cobertura`. Opções: adaptar interface mantendo motivo e guarda; separar porta necessária à geração certificada com contrato compatível. Retirar a guarda não é opção autorizada pela lei escrita. (`bin/suite.sh:53`, `bin/suite.sh:66`, `bin/suite.sh:173`, `CLAUDE.md:101`; `_porta` **[nao verificado no HEAD]**)

3. **Inteiras CI e control-plane.** O mesmo `_teste_irmao` roda CI/LABELS e tenants/teste_motor. O anexo marca ambas como inteira. O control-plane roda mesmo quando arsenal foi evitado. Decisão de alcance: uma contagem agregada de todos os runs ou partições explicitamente identificadas; não presumir que “evitada” significa push sem run. (`bin/pre-push.sh:142`, `bin/pre-push.sh:159`, `bin/pre-push.sh:184`, `bin/pre-push.sh:186`, `bin/pre-push.sh:190`, `bin/pre-push.sh:198`, `app/docs/RELATO.md:1648`)

4. **Janela `24 h` versus runs terminados/iniciados.** Anexo escreve linha ao final, mas filtra por início; corrida ainda viva não aparece; corrida atravessando borda é contada pelo início. Opções: início, conclusão ou outra unidade explicitamente contratada; não são dia civil nem dia do turno. O contrato HEAD enumera início/fim e janela, sem resolver esta borda. (`app/docs/RELATO.md:1648`; algoritmo **[nao verificado no HEAD]**)

5. **Limites temporais e fuso.** Anexo usa `datetime.now()` ingênuo, timestamps shell sem offset, `ini >= desde` e nenhum teto `<= agora`; merges usam `git --since` sem offset. Opções: um instante/fuso explícito e intervalo comum; documentar preservação da convenção atual após medi-la. A semântica real de Git e timezone de prod não foi medida. (`bin/trava_teste.sh:40`, `app/docs/RELATO.md:1648`; algoritmo/análises de produção **[nao verificado no HEAD]**)

6. **Git ilegível versus zero pousos.** No anexo `_pousos` devolve `None`, mas `linha` publica “sem pouso” tanto para `None` como zero. Opções: separar indisponibilidade de contagem zero; propagar estado de medida da fonte. HEAD já exige “sem medida” para log ilegível, e falta de sinal não autoriza zero. (`app/docs/RELATO.md:1648`, `bin/gerar_estado.py:258`; defeito do anexo **[nao verificado no HEAD]**)

7. **Log vazio/parcial e fontes auxiliares ausentes.** Anexo distingue `suites.log` ilegível, mas linhas inválidas viram descartes e `medida.log` ilegível vira lista vazia: evitadas/ESCAPOU zero. Opções: estados por fonte e contagem de descartes; contrato explícito de parcialidade. (`app/docs/RELATO.md:1648`; comportamento **[nao verificado no HEAD]**)

8. **Razão arredondada versus meta.** Anexo calcula razão arredondada antes de comparar com `1.5`; valores pouco acima podem sair sem “ACIMA”. Opções: comparar fração crua e arredondar exibição; definir tolerância se aval explícito vier. Meta escrita é `<=1,5`, sem tolerância. (`app/docs/RELATO.md:1648`; algoritmo **[nao verificado no HEAD]**)

9. **Pouso Git versus mero merge first-parent.** Definição operacional já está escrita: pouso `merge --no-ff --no-commit`, com instrumento em pouso próprio. Logo não perguntar novamente se FF conta ou se instrumento pode pousar com produto. Registro de evento Git não comprova sucesso de deploy; essa possível exigência nova de denominação requer alcance explícito, não inferência. (`CLAUDE.md:325`, `CLAUDE.md:326`, `CLAUDE.md:327`, `app/docs/RELATO.md:1648`)

10. **`rc`, resultado e ausência de execução.** Contrato manda contar run com rc real, sem filtrar só sucesso; não perguntar se vermelho conta como inteira. Rc75 sai antes do comando na trava, logo não é corrida executada. Falha do Docker depois da concessão da trava é execução do comando, ainda que testes não tenham começado; decidir apresentação/partição sem alterar rc. (`app/docs/RELATO.md:1648`, `bin/trava_teste.sh:36`, `bin/trava_teste.sh:38`, `bin/trava_teste.sh:42`, `bin/pre-push.sh:148`, `bin/pre-push.sh:150`)

Rebase/replay: escolha já registrada entre commit atual com escape reproduzível ou mapa regenerado; mapa velho deixa IMPACTO vazio; não afirmar pronto nem repetir resultado histórico como atual. (`app/docs/RELATO.md:1635`, `app/docs/RELATO.md:1637`)

## 13/11 18:10 · sessao e205bf074ada · - · - · origem=Ronald (tmux)

LEU — leis que já respondem e não devem aumentar `perguntas de lei: N`:

| Pergunta | Resposta escrita no HEAD |
|---|---|
| Justificativa aprovada cobre dia inteiro automaticamente? | Cobre o marco que seu tipo nomeia; o abono deve consultar o mesmo juiz. `app/docs/CORTES.md:319`, `app/docs/CORTES.md:320` |
| Pode implantar cobertura estreita antes de pagar o período? | Não: a parte justificativa foi separada e retida até haver pagamento; virar falta inteira com DSR perdido foi expressamente recusado. `app/docs/CORTES.md:330` |
| Motor pode ler período pago? | Sim, pelo aval posterior: mesma fonte do PDF, uma mudança no motor, DIFF novo antes, limites explícitos. O aval de 18:4x supera “motor intocado” dos cortes anteriores nesse alcance. `app/docs/CORTES.md:353` |
| Furo residual após mudança de regra emite push retroativo? | Mudança de regra não é fato novo; lista do admin sem push. Piso fixo de data foi recusado. `app/docs/CORTES.md:323` |
| Sem batida casada a marco? | Classe CADASTRO, lista do admin, sem cura por código. `app/docs/CORTES.md:336` |
| Dia de parcial noturna? | `hora_fim < hora_ini` cruza meia-noite e pertence ao dia de início. `app/docs/CORTES.md:330` |
| DSR pode piorar indiscriminadamente? | Condição posterior é zero semana de DSR perdida a mais fora dos dias CADASTRO/BATIDA listados. `app/docs/CORTES.md:353` |
| Exportada e col709 25/08? | TXT intacto; col709 é BATIDA e vira Pauta DP com pago/devido. `app/docs/CORTES.md:353`; regra geral de gravado exportado em `CLAUDE.md:411` |
| Data civil ou dia de início para jornada/DSR? | Dia de início, inclusive semana/DSR. `app/docs/LEIS.md:117` |
| BATIDA.tipo ou papel da ata? | Instante da batida contra DNA/ata; tipo gravado não decide. `app/docs/LEIS.md:119` |
| Previsto inventado fora da grade? | Proibido, grade fonte única. `CLAUDE.md:148`, `app/docs/LEIS.md:75` |
| Caso CADASTRO/BATIDA pode ser curado por código? | Proibido; só ESTRUTURA é fatia. `CLAUDE.md:179` |

INFERIU — duas candidatas de alcance, sem decisão:

1. **Uma JUST ancorada cobre todos os turnos elegíveis daquele dia, ou apenas seu episódio?** O modelo declara `batida_entrada` como “entrada do turno com problema”, `data_turno` e `chamado_origem`, mas o diff fornecido colapsa justificativas em `{data: tipos}` e perde as âncoras. Opções: alcance por dia/tipo; alcance pelo turno da âncora; sem âncora encaminhar para cadastro. O corte diz “marco que o tipo nomeia”, sem descrever resolução de múltiplos episódios quando uma aprovação tem âncora específica. `app/ponto/models.py:139`, `app/ponto/models.py:144`, `app/ponto/models.py:173`, `app/docs/CORTES.md:319`; algoritmo da raia apenas no anexo, correspondência HEAD **[nao verificado]**.

2. **Se Just parcial, Ausência paga e realizado se sobrepõem, qual precedência/união de cobertura?** O HEAD soma minutos de Ausência e trata Just como conjunto de dias; a raia acrescentaria outra soma de minutos. Opções: união líquida por período; prioridade de uma fonte; manter efeitos distintos com exclusões explícitas. Há obrigação geral de partes coerentes, mas não localizei nas buscas em LEIS/CORTES/CLAUDE regra específica entre Just parcial e Ausência paga sobreposta. `app/ponto/services/efeito_ausencia.py:40`, `app/ponto/services/efeito_ausencia.py:47`, `app/ponto/services/efeito_ausencia.py:53`, `app/docs/LEIS.md:136`. Ocorrência em produção **[nao verificado]**. Antes de contar como pergunta efetiva, medir se a classe existe no universo ou fixar seu contrato de alcance.

Alertas para o desenho do construtor:

- O HEAD já **não tem** o acumulador `_min_parcial_pagos` no fechamento: foi removido como órfão, e o crédito é por dia via `credito_parcial.creditos_por_dia`. Portanto o endereço nominal do aval não pode ser restaurado cegamente. `app/ponto/services/fechamento.py:672`, `app/ponto/services/fechamento.py:994`, `app/ponto/services/fechamento.py:999`.
- O crédito existente abate atraso do próprio dia, sem transportar saldo para outros dias; o leitor atribui período pela data civil local da entrada, o que precisa ser confrontado com L-085 no censo semântico. `app/ponto/services/credito_parcial.py:32`, `app/ponto/services/credito_parcial.py:50`, `app/docs/LEIS.md:117`.
- Preservar a cura posterior H12H06-B: parcial é `cobre_parte_do_dia`, não `minutos` truthy como no diff antigo. `app/ponto/services/efeito_ausencia.py:35`, `app/ponto/services/efeito_ausencia.py:48`, `app/ponto/services/credito_parcial.py:23`, `app/ponto/services/fechamento.py:688`.
- Motor é zona inviolável; batida só pela porta, motor não fabrica batida; UTC no banco/UTC-3 na apresentação; DNA congelado e universo `batidas_apuraveis`. `CLAUDE.md:141`, `CLAUDE.md:143`, `CLAUDE.md:146`, `CLAUDE.md:149`, `CLAUDE.md:158`.
- L-126 ainda proíbe ao papel **construtor Codex** motor, core/juizes, portas/célula, exportada e itens que pedem `!`; o aval de motor não equivale, por si, a ampliar esse papel. `app/docs/LEIS.md:147`.
- RED antes de implementação, casos pela regra e adversarial; DIFF serve impacto, não certificação. `CLAUDE.md:28`, `CLAUDE.md:45`, `app/docs/LEIS.md:136`.
- `marcos_cobertos_por_justificativa`, `minutos_do_periodo_justificado` e `efeito_da_justificativa` não apareceram nas buscas em turnos/core/JUIZES do HEAD; criação aceita em corte é diferente de implementação presente. `app/docs/CORTES.md:320`; ausência no HEAD deve ser sustentada pelo script AST do pai, não por linha inexistente. **[nao verificado: resultado negativo do censo AST ainda não recebido]**.

## 13/11 18:09 · sessao bfd4d78ec20a · - · - · origem=Ronald (tmux)

LEU nesta cópia:

- **Escritor único / trilha:** `CLAUDE.md:32` e `app/docs/LEIS.md:65` (L-007).
- **Testemunha não recalcula:** `CLAUDE.md:20`; origem da cura em `CLAUDE.md:17`.
- **RED antes / quem mais lê:** `CLAUDE.md:28`; casos pela regra em `CLAUDE.md:45`.
- **Idempotência:** “2x = mesmo estado, 1 linha de trilha, 0 duplicata”, `CLAUDE.md:178`.
- **Cadastro/batida não se curam por código:** `CLAUDE.md:179`.
- **Juiz novo exige corte:** `CLAUDE.md:74`; registro vivo prevalece sobre lookup em `app/docs/JUIZES.md:11` e `:14`.
- **Zonas invioláveis:** motor só com aval em `CLAUDE.md:142`; batida em `:143`; CPF/nome completo em `:145`; UTC/apresentação UTC-3 em `:146`; grade/célula em `:148`; universo apurável em `:158`.
- **Exportada:** L-092 está em `app/docs/LEIS.md:125`, mas é alterada por L-113 em `:153`; forma consolidada vigente em `CLAUDE.md:411` e `:422`. O anexo HJ091 restringe o próprio passivo a lista/Pauta DP; não ampliar esse escopo.
- **Escolhas técnicas do construtor:** `CLAUDE.md:276`; não contar automaticamente escolha de FK/on_delete/encaixe como pergunta de lei.
- **Pergunta já respondida pela lei escrita não conta:** `CLAUDE.md:269`.

**Autoridades registradas pertinentes:** `app/core/juizes.py:251` a `:274` inclui `efeito_vigente`, `cobertura_ausencia_periodo`, `veto_de_lancamento`, `estado_inicial`, `ABERTAS`, `em_gozo_hoje`, saldo, `AgendamentoFerias.clean`, `fim_de`; lookup resumido em `app/docs/JUIZES.md:55`. `status_por_data` se descreve como juiz em `app/ferias/models.py:274`, mas não consta nesse bloco do registro vivo; `espelhar_ferias`, `_guard_sobreposicao` e `ferias_na_janela` tampouco constam nele. Ausência universal no registro fora do bloco: busca textual executada, sem ocorrência desses nomes.

**Selo atual insuficiente para HJ091:** aceita qualquer escrita dentro do arquivo autorizado (`app/ponto/tests/test_chokepoint_ausencia_gate.py:29`), e declara a porta de Ausencia em `:65`; não distingue escritores internos. O espelhador chama `criar_ausencia` em `app/ponto/services/ausencia.py:1203`, portanto o selo futuro precisa representar essa chamada transitiva legítima, além de detectar entradas genéricas.

**INFERIU — perguntas de lei: 0 no escopo expresso.** Exclusividade de escrita das derivadas já decorre do aval fornecido e L-007; aprovar/reabrir/anexar também precisa entrar no desenho de alcance. FK nula não prova ausência avulsa: o aval manda listar o vínculo não resolvido. `on_delete`, localização da guarda e representação AST da escrita transitiva são decisões técnicas; nenhuma resposta encontrada em LEIS/CORTES que autorize inventar vínculo de origem por coincidência de janela.

`AGENTS.md` não apareceu em `rg --files -g AGENTS.md`; existência fora do universo dessa busca: **[nao verificado]**.

## 13/11 18:09 · sessao 323f94675b3f · - · - · origem=Ronald (tmux)

LEU — fontes e limites:

- A obra principal permanece interrompida, faltando suite, replay, verificador e pouso próprio; a preparação é leitura/desenho sem suite/sombra/prod. (`app/docs/BACKLOG.md:2083`; `app/docs/BACKLOG.md:739`)
- O complemento só entra depois do pouso da E3; soltar carga exige DIFF do vigia zero; pico RSS tem teto ≤1500 MB e exige contrato de cinco linhas/pouso próprio. A E3 não muda. (`app/docs/BACKLOG.md:738`; `app/docs/RELATO.md:5`)
- O contrato original de OTIMIZA-1 está integralmente em `app/docs/RELATO.md:1648`: uma linha TAB por run, início/fim/quem/escopo/rc; escopo declarado pelo chamador; pousos são merges first-parent do main; meta ≤1,5; arquivo ilegível significa “sem medida”; escrita da medida não altera rc; nada no pouso depende dessa seção.
- A construção da raia e as pendências estão documentadas em `app/docs/RELATO.md:1645`. O diagnóstico PRONTO 5 registra mapa velho e escolha pendente entre replay com escape reproduzível em commit atual ou mapa regenerado. (`app/docs/RELATO.md:1635`; `app/docs/RELATO.md:1637`)

Leis que já respondem — não contar como perguntas de lei:

| Questão | Lei escrita |
|---|---|
| Fonte do contador, sem derivador paralelo | `CLAUDE.md:17`; `CLAUDE.md:20`; `CLAUDE.md:35` |
| RED antes da cura e censo de quem mais lê | `CLAUDE.md:28` |
| Escopo literal, sem ampliar obra | `CLAUDE.md:37`; `app/docs/LEIS.md:62` |
| Contrato de entrada com fonte/unidade/universo/exclusões | `CLAUDE.md:192` |
| Cron somente vigia, sem decidir negócio | `CLAUDE.md:172` |
| Contrato de instrumento com quem chama/espera, falha/limite, medida, quem desliga | `CLAUDE.md:394` |
| Instrumento em pouso separado de produto | `CLAUDE.md:327`; `app/docs/LEIS.md:131` |
| Mesma árvore certificada pela suite é a que pousa | `CLAUDE.md:320` | [nao verificado]
| Pouso por merge sem fast-forward | `CLAUDE.md:326` | [nao verificado]
| Escolhas de encaixe/caminho são decisões técnicas | `CLAUDE.md:276` |
| Nenhuma lei nova por iniciativa própria | `CLAUDE.md:269` |
| Juiz novo exige corte nominal | `CLAUDE.md:74` |

INFERIU — para o escopo literal OTIMIZA-1 + complemento, não identifiquei lacuna de lei que obrigue inventar regra: **candidata a contagem “perguntas de lei: 0”**. As escolhas abaixo são semântica/encaixe de instrumento, não autorização para alterar o contrato. (`app/docs/RELATO.md:1648`; `app/docs/BACKLOG.md:738`; `CLAUDE.md:276`)

Autoridade:

- `JUIZES.md` é lookup; a lei está em CLAUDE e o registro vivo em `core/juizes.py`. (`app/docs/JUIZES.md:7`)
- `tarefa_do_dia` está registrado: puro, zero escritor, relógio `agora`, lote `CargaDaTarefa`; pergunta por instância de marco. (`app/docs/JUIZES.md:78`; `app/docs/JUIZES.md:84`; `app/core/juizes.py:150`; `app/core/juizes.py:295`)
- O carimbo do vigia é fotografia, não fonte de julgamento; sua autoridade é o censo ao vivo. (`app/chamados/management/commands/vigiar_fio_morto.py:21`)

Divergências/decisões para transmitir:

1. **Teto RSS ≤1500 versus igualdade dos leitores existentes.** `fio_estado._ler_carimbo` compara todos os valores de `esperado` por `!=`; inserir `pico=1500` faria 1237 virar divergência. Candidatas técnicas: representar operador/unidade no contrato e leitores; ou representar excesso acima do teto como contador esperado zero. Não escolhi. (`bin/fio_estado.py:121`; `bin/fio_estado.py:125`; `app/docs/BACKLOG.md:738`)
2. **Ancoragem obsoleta do complemento.** No HEAD, `vigiar_fio_morto.py:176` abre `tranca_em_lote()`; não contém `CargaDaTarefa` nem sua liberação. A localização pós-E3 precisa ser refeita após o pouso. (`app/chamados/management/commands/vigiar_fio_morto.py:176`; `app/docs/BACKLOG.md:738`)
3. **RSS da rodada/processo versus momento do carimbo.** `_carimbar` hoje apenas serializa e faz replace; o carimbo nasce depois dos censos. O método e a unidade exata do pico ainda precisam constar no contrato do complemento. (`app/chamados/management/commands/vigiar_fio_morto.py:120`; `app/chamados/management/commands/vigiar_fio_morto.py:232`; `app/docs/BACKLOG.md:738`)
4. **Universo de carimbo.** O carimbo só nasce em frota; `--empresa` declara que não carimbou. O encaixe do alarme de RSS em recortes precisa ser explicitado sem transformar recorte em fotografia da frota. (`app/chamados/management/commands/vigiar_fio_morto.py:27`; `app/chamados/management/commands/vigiar_fio_morto.py:231`; `app/chamados/management/commands/vigiar_fio_morto.py:253`)
5. **Instante/data civil.** O vigia lê `agora` uma vez, deriva `hoje` local e grava `medido_em` com offset; o leitor converte o offset ao fuso do host. O fuso real do host é [nao verificado]. (`app/chamados/management/commands/vigiar_fio_morto.py:164`; `app/chamados/management/commands/vigiar_fio_morto.py:233`; `bin/fio_estado.py:129`)

Zonas proibidas para o brief:

- Motor v2 só com aval explícito; nenhuma edição exigida pelo escopo apresentado. (`CLAUDE.md:142`; `app/docs/BACKLOG.md:738`)
- Não tocar gravado exportado, escala/vínculo ou regra de negócio fora do pedido; não pular testes. (`CLAUDE.md:265`) [nao verificado]
- Não rejulgar por vigia nem derivar previsto fora da grade/célula. (`CLAUDE.md:148`; `CLAUDE.md:172`)
- A árvore certificada da E3 permanece intocada pelo complemento. (`app/docs/BACKLOG.md:738`; `app/docs/RELATO.md:5`)

## 13/11 18:09 · sessao 7870d3302bf5 · - · - · origem=Ronald (tmux)

Leis e autoridades lidas para consolidar:

| Tema | Resposta já escrita / evidência |
|---|---|
| Juiz/escritor único; parâmetro consumido | `CLAUDE.md:178`; testemunha lê autoridade e não deriva em paralelo em `CLAUDE.md:20-22`; consultar lei existente antes de novo corte em `CLAUDE.md:25-27`. |
| Identidade BATIDA | Catálogo declara `(batida_origem,motivo)`, episódio, separado de MARCO em `app/chamados/catalogo/motivos.py:12-19`; `foto_ausente` é BATIDA em `app/chamados/catalogo/motivos.py:43`. Não é lacuna de lei. |
| Episódio imutável | Escrita uma vez no nascimento em `app/chamados/models.py:2437-2442`; origem usa `ultima_batida.id`, validada contra colaborador em `app/chamados/models.py:3453-3468`. |
| Contexto fundido/acervo | O comentário do consumidor proíbe derivar posteriormente o episódio do contexto fundido, porque já pode apontar outra anomalia; o acervo não ganha episódio assim: `app/chamados/models.py:3457-3459`. |
| Dia do turno versus civil | `abrir` exige dia do turno e `abrir_conversa` usa dia civil: `app/chamados/models.py:2784-2803`. Contrato de saída mantém essa distinção em `app/chamados/models.py:2811-2817`. |
| Juiz do dia | Registro vivo aponta `turnos_do_colab` em `app/core/juizes.py:207-208`; lookup registra geometria em `app/docs/JUIZES.md:20-25`. Adaptador `dia_do_turno_da_batida` chama o mapa da autoridade em `app/ponto/turnos.py:2927-2936`, não deriva outra regra. |
| Batida não reclamada por turno | Dia em que aconteceu, pela autoridade, em `app/ponto/turnos.py:2886-2893`; implementação em `app/ponto/turnos.py:2945-2955`. Selo de fonte única do dia está aprovado em `app/docs/CORTES.md:369`. |
| Fuso | UTC no banco, apresentação UTC-3: `CLAUDE.md:146-147`; `app/docs/LEIS.md:107` (L-080). |
| Prazo selfie | PWA zero, APK/NULL 90 minutos em `app/ponto/registro_batida.py:69-77`; decorrido em minutos e comparação `>=` em `app/ponto/registro_batida.py:80-91`. |
| Janela retro | `[agora − dias, agora]` inclusiva, não dias de turno: `app/ponto/management/commands/marcar_foto_ausente_retro.py:63-77`. |
| Prazo versus teto do turno | Comparar instante ou exigir fato encerrado, nunca só data: `CLAUDE.md:188`; L-073 em `app/docs/LEIS.md:100`. |
| Alcance | L-124: horizonte para frente, passado FOTO até fato novo, backfill único em `app/docs/LEIS.md:145`; reafirmada em `app/docs/CORTES.md:419`. |
| Exportada | L-113 permite nascimento em competência exportada, expressamente: `app/docs/LEIS.md:153`. Dinheiro/gravado seguem regras consolidadas em `CLAUDE.md:406-422`; não usar isoladamente o texto histórico de L-092. |
| Dono | L-099: só ESTRUTURA vira cura; CADASTRO/BATIDA vão ao admin: `CLAUDE.md:179`, `app/docs/LEIS.md:122`. |
| RED | Caso pela regra antes do código: `CLAUDE.md:28-29`, `CLAUDE.md:45-51`; L-110 em `app/docs/LEIS.md:136`; certificado/impacto/termômetro separados em `app/docs/LEIS.md:152`. |

Autoridades versus derivações:

- **Registradas no lookup:** `turnos_do_colab` e demais geometria em `app/docs/JUIZES.md:20-25`; `data_do_chamado` em `app/docs/JUIZES.md:59-63`; validar em `app/docs/JUIZES.md:38-39`; fio em `app/docs/JUIZES.md:40`.
- **Registrada apenas no vivo, na leitura realizada:** `selfie_ausente` em `app/core/juizes.py:1196-1199`, com corte em `app/docs/CORTES.md:283`. A busca nominal por `selfie_ausente` na cópia de `JUIZES.md` não encontrou entrada; lookup diz que o vivo manda em `app/docs/JUIZES.md:7-15`.
- **Consumidores que selecionam dados, sem registro de juiz encontrado:** `gerar_perguntas_disputa` escolhe `motivo.data` antes de perguntar pelo episódio em `app/chamados/services/disputa_emissao.py:371-392`; `batida_origem_do_nascimento` escolhe apenas `ultima_batida.id` em `app/chamados/models.py:3461-3468`. Classificá-los como autoridades novas seria **INFERIU**, não registro lido.
- **Emissor retro delega a ausência ao juiz:** `app/ponto/management/commands/marcar_foto_ausente_retro.py:73-77`; seu payload incompatível está em `:96-104`. Não deriva ausência por conta própria.
- **Apresentação fora da costura:** cabeçalho do retro formata `agora`/`corte` crus em `app/ponto/management/commands/marcar_foto_ausente_retro.py:80-82`; diverge da apresentação local exigida em `CLAUDE.md:146-147`. Não precisa ser tocado para entregar episódio (**INFERIU**).

**perguntas de lei: 0**, no escopo B e nas fontes acima (**INFERIU**). Episódio, dia do turno, fuso, identidade, nascimento em exportada e alcance já têm resposta; formato de transporte/chave e organização do selo permanecem decisões de desenho, não lei nova. A eventual rota concreta para recuperar passivo sem episódio confiável ainda precisa medir e desenhar; não autoriza reconstrução pela última batida do contexto fundido (`app/chamados/models.py:3457-3459`).

_4753 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
