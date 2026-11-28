# CODEX-ESTADO

> Ultima resposta processada: **28/11/2026 01:43** · publicado em **28/11/2026 01:47** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 28/11 01:43 · sessao db69fbcafc44 · FERIADO-UM-JUIZ · leitura · origem=Code

**O DIFF deve rodar duas vezes sobre a mesma sombra**. Monte o `app/` da árvore viva para o **ANTES** e o `app/` de `d765154a3` para o **DEPOIS**, via `SONDA_ARVORE`. O gravado da sombra é a terceira coluna, usada para estimar o que o apply escreveria. Deltas reais, lista do TXT e hashes de hoje seguem [não verificado]. `bin/sonda_leitura.sh:226-240` `bin/diff_o124_frota.py:4-14` `app/ponto/services/diff_frota.py:7-12`

### 1. Instrumento e universo

- **`bin/diff_o124_frota.py` é o precedente para comparar duas árvores.** Ele roda o escritor real em modo leitura em cada montagem e compara as duas fotos por colaborador e campo. O script original fixa uma competência por rodada e chama o recalculador sem filtro de empresa. **`diff_frota.medir`** é o serviço da casa para gravado, hoje e novo quando a mudança é uma mutação dentro de uma única árvore. Ele parte de quem já tem mensal gravado. `bin/diff_o124_frota.py:10-14` `bin/diff_o124_frota.py:20-42` `bin/diff_o124_frota.py:47-69` `app/ponto/services/diff_frota.py:45-89`
- **`diff_calculador` compara motor e calculador por dia e rubrica dentro do código carregado.** Recebe um par `--mes/--ano` por chamada, percorre todas as empresas ativas e parte dos IDs com mensal gravado. `--contra-gravado` acrescenta a comparação do calculador com o mensal. `--periodos-do-fechamento` captura os períodos da mesma árvore e intercepta a lavratura. `app/ponto/management/commands/diff_calculador.py:25-56` `app/ponto/management/commands/diff_calculador.py:98-135` `app/ponto/management/commands/diff_calculador.py:541-588`
- **`sombra.sh --cobre INI FIM` consulta o carimbo de cobertura.** Use a janela de cada empresa, incluindo o dia adicional em cada ponta que o fechamento consulta. A vigência começa em 11/2026 e alcança 12/2026. Com corte 21, 20/11 é a véspera da janela da 12 e entra na busca de batidas. O corte efetivo das três empresas e a cobertura de hoje seguem [não verificado]. `bin/sombra.sh:219-233` `app/core/sombra_cobertura.py:32-49` `app/ponto/janelas.py:10-21` `app/ponto/janelas.py:54-57` `app/ponto/services/fechamento.py:155-173` `app/ponto/services/fechamento.py:601-604` `app/ponto/services/fechamento.py:685-699` `app/folha/export.py:383-391`

### 2. Sonda de impacto

`--sombra` seleciona `sombra_db/sombra`. Por padrão, a sonda monta `$RAIZ/app:/app:ro`, onde `$RAIZ` é a raiz do **wrapper chamado**. A opção de árvore é `SONDA_ARVORE=<cópia>/app`. O `--dir` é da suíte. O HEAD vivo `93e305860` e a disponibilidade dessa opção no wrapper instalado seguem [não verificado]. `bin/sonda_leitura.sh:31` `bin/sonda_leitura.sh:38-45` `bin/sonda_leitura.sh:66-71` `bin/sonda_leitura.sh:93-97` `bin/sonda_leitura.sh:226-240` `bin/suite.sh:170`

A sonda usa o universo integral do recalculador para as empresas 2, 3 e 4 nas competências 11 e 12, inclusive quem ainda está sem mensal. Os totais incluem diferenças de **0,01 h** e mostram quantos valores preenchidos entraram em cada soma. No TXT 11, segue a chamada do gerador a `classificar_export` sobre os mensais em memória. A marca `ITEM_8` mostra quando zerar só a HE100 feriado numa cópia em memória devolve o motivo `cadastro_zero`. `SEM_CELULA` distingue esse caso de cadastro sem previsto com célula. A coluna `TXT` preserva o veredito real, inclusive quando uma retenção posterior ainda barra o colaborador. `app/ponto/services/fechamento.py:490-507` `app/ponto/services/fechamento.py:619-622` `app/ponto/services/fechamento.py:962-973` `app/ponto/services/fechamento.py:1016-1021` `app/ponto/management/commands/recalcular_fechamento.py:54-55` `app/folha/views.py:214-222` `app/folha/export.py:924-926` `app/folha/export.py:948-1005`

Conteúdo de `impacto_feriado.py` para a sessão. Sintaxe, comparador e projeção do item 8 conferidos com dados sintéticos. Execução com Django e a sombra real [não verificado].

```python
import json
import sys
from copy import copy
from decimal import Decimal

ANO, MESES, EMPRESAS = 2026, (11, 12), (2, 3, 4)


def projetar(registros, campos):
    saida = {}
    for f in registros:
        valores = (getattr(f, campo) for campo in campos)
        saida[str(f.colaborador_id)] = [None if v is None else str(v) for v in valores]
    return saida


def vereditos(empresa, fechamentos):
    from folha.export import classificar_export

    itens = classificar_export(empresa, 11, ANO, fechamentos=fechamentos)
    candidatos = []
    for f in fechamentos:
        if f.horas_trabalhadas == 0 and f.horas_extras_100_feriado > 0:
            copia = copy(f)
            copia.horas_extras_100_feriado = 0
            candidatos.append(copia)
    retidos_sem_he = {
        i["colaborador_id"]: (
            "SEM_CELULA" if i["detalhe"].startswith("Sem NENHUMA celula") else "SEM_PREVISTO"
        )
        for i in classificar_export(empresa, 11, ANO, fechamentos=candidatos)
        if i["motivo"] == "cadastro_zero"
    }
    return {
        str(i["colaborador_id"]): [
            i["status"], i["motivo"],
            retidos_sem_he.get(i["colaborador_id"]) if i["motivo"] != "cadastro_zero" else None,
        ]
        for i in itens
    }


def coletar():
    from django.db import connection, transaction

    with transaction.atomic():
        with connection.cursor() as cur:
            cur.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            cur.execute("SELECT current_database(), current_schema(), current_user, "
                        "current_setting('transaction_read_only')")
            if cur.fetchone() != ("sombra", "juliani", "leitor", "on"):
                raise RuntimeError("Sonda exige sombra/juliani com leitor em READ ONLY")

        from colaboradores.models import Empresa
        from ponto.janelas import janela_fechamento
        from ponto.management.commands.recalcular_fechamento import RUBRICAS
        from ponto.models import FechamentoMensal
        from ponto.services.fechamento import recalcular_fechamento_mes, universo_da_competencia

        dados = {}
        for mes in MESES:
            for eid in EMPRESAS:
                empresa = Empresa.objects.get(pk=eid)
                janela = janela_fechamento(mes, ANO, empresa)
                gravado = FechamentoMensal.objects.filter(
                    mes=mes, ano=ANO, colaborador__empresa_id=eid
                )
                lidos = recalcular_fechamento_mes(
                    mes, ANO, empresa_id=eid, somente_leitura=True,
                    retornar_objeto=True, lavrar_dias_pagos=False, propagar_falha=True,
                )
                esperados = set(universo_da_competencia(mes, ANO, eid).values_list("pk", flat=True))
                if lidos.keys() != esperados:
                    raise RuntimeError(f"Leitura incompleta emp{eid} {mes:02d}/{ANO}")
                dados[f"{mes:02d}/{ANO} emp{eid}"] = {
                    "janela": [d.isoformat() for d in janela],
                    "gravado": projetar(gravado, RUBRICAS),
                    "lido": projetar(lidos.values(), RUBRICAS),
                    "txt": vereditos(empresa, list(lidos.values())) if mes == 11 else {},
                }
        transaction.set_rollback(True)

    print("IMPACTO_JSON", json.dumps({"campos": RUBRICAS, "dados": dados}, sort_keys=True))


def delta(antes, depois):
    return None if antes is None or depois is None else Decimal(depois) - Decimal(antes)


def linha(*valores):
    print(*(v if v is not None else "-" for v in valores), sep="\t")


def comparar(antes, depois):
    campos = antes["campos"]
    a, b = antes["dados"], depois["dados"]
    if campos != depois["campos"] or a.keys() != b.keys():
        raise SystemExit("Escopos ou rubricas de comparação diferentes")
    if any(a[k]["gravado"] != b[k]["gravado"] or a[k]["janela"] != b[k]["janela"] for k in a):
        raise SystemExit("O gravado ou a janela mudou entre as leituras")

    linha("FOLHA", "comp empresa", "colaborador", "rubrica",
          "gravado", "antes", "depois", "efeito", "delta_apply")
    for grupo in sorted(a):
        linha("JANELA", grupo, *a[grupo]["janela"])
        gravado, velho, novo = a[grupo]["gravado"], a[grupo]["lido"], b[grupo]["lido"]
        fotos = (gravado, velho, novo)
        vazio = [None] * len(campos)
        for cid in sorted(gravado.keys() | velho.keys() | novo.keys(), key=int):
            col = f"col{int(cid):03d}"
            if not all(cid in foto for foto in fotos):
                linha("UNIVERSO", grupo, col, *(cid in foto for foto in fotos))
            for pos, campo in enumerate(campos):
                g, v, n = (foto.get(cid, vazio)[pos] for foto in fotos)
                linha("FOLHA", grupo, col, campo, g, v, n, delta(v, n), delta(g, n))

        for pos, campo in enumerate(campos):
            colunas = [[Decimal(v[pos]) for v in foto.values() if v[pos] is not None] for foto in fotos]
            g, v, n = (sum(valores, Decimal(0)) for valores in colunas)
            linha("TOTAL", grupo, "frota", campo, g, v, n, n - v, n - g,
                  *(len(valores) for valores in colunas))

        ta, tb = a[grupo]["txt"], b[grupo]["txt"]
        for cid in sorted(ta.keys() | tb.keys(), key=int):
            va = ta.get(cid, ["sem_fechamento", None, None])
            vb = tb.get(cid, ["sem_fechamento", None, None])
            movimento = "ENTROU" if vb[0] == "entra" and va[0] != "entra" else (
                "SAIU" if va[0] == "entra" and vb[0] != "entra" else "-"
            )
            linha("TXT", grupo, f"col{int(cid):03d}", movimento, *va[:2], *vb[:2])
            if va[2] or vb[2]:
                linha("ITEM_8", grupo, f"col{int(cid):03d}", movimento, va[2], vb[2])


if sys.argv[1:] == ["--comparar"]:
    comparar(json.loads(next(sys.stdin)), json.loads(next(sys.stdin)))
else:
    coletar()
```

As duas rodadas devem usar a mesma foto da sombra, sem refresh ou outro escritor no intervalo. A igualdade do gravado e das janelas detecta parte das mudanças de entrada. Cada rodada tem seu próprio snapshot e o fechamento usa o relógio para o teto de furos, relevante na 12 em curso. `app/ponto/services/fechamento.py:679-680` `app/ponto/services/fechamento.py:1147-1154` `bin/diff_o124_frota.py:10-14`

**Há uma diferença operacional importante.** O Python acima abre `READ ONLY` e termina com rollback. O wrapper provisiona role e GRANTs, reescreve `logs/.env_leitor`, copia a sonda para um temporário e grava trilha. Se a proibição de escrita inclui esses atos administrativos, use a montagem direta abaixo com leitor e rede **já provisionados**. Ela também evita depender de `app/logs` e `app/media` na cópia, usados como pontos de montagem pelo wrapper. A disponibilidade desses pontos e do leitor provisionado na sessão segue [não verificado]. `bin/sonda_leitura.sh:115-181` `bin/sonda_leitura.sh:184-203` `bin/sonda_leitura.sh:216-219` `bin/sonda_leitura.sh:245-250` `bin/sombra.sh:180-191`

```bash
(
  set -euo pipefail
  viva=/home/ronald/saas-hasner
  obra=/home/ronald/codex_ws/d765154a302318df66e6fd7c1a8ab01dab660004
  sonda=/caminho/da/sessao/impacto_feriado.py

  test "$(git -C "$viva" rev-parse --short=9 HEAD)" = 93e305860

  rodar() {
    SONDA_ARVORE="$1" bash "$viva/bin/sonda_leitura.sh" --sombra "$sonda"
  }

  medir() {
    rodar "$1" 2>&1 | awk '
      /^IMPACTO_JSON / { sub(/^IMPACTO_JSON /, ""); print; n++ }
      END { if (n != 1) exit 1 }
    '
  }

  antes=$(medir "$viva/app") || { echo "Falhou a leitura ANTES" >&2; exit 1; }
  depois=$(medir "$obra/app") || { echo "Falhou a leitura DEPOIS" >&2; exit 1; }
  printf '%s\n%s\n' "$antes" "$depois" | python3 -B "$sonda" --comparar
)
```

Para a montagem direta, substitua `rodar` no bloco acima pela função a seguir. Ela usa a imagem, a rede e a credencial de leitor da sonda, com a árvore em `/codigo:ro` e logs em tmpfs. Execução desse comando na sessão [não verificado]. `bin/sonda_leitura.sh:42-51` `bin/sonda_leitura.sh:232-240` `bin/recursos.sh:81` `app/config/settings/sombra.py:47-51` `app/config/settings/saas.py:118-143`

```bash
rodar() {
  (
    source "$viva/bin/recursos.sh"
    docker run --rm --read-only $TESTE_DOCKER \
      --network sonda_leitura --env-file "$viva/logs/.env_leitor" \
      -e POSTGRES_DB=sombra -e TZ=America/Sao_Paulo -e PYTHONDONTWRITEBYTECODE=1 \
      --mount "type=bind,src=$1,dst=/codigo,readonly" \
      --mount "type=bind,src=$sonda,dst=/sonda.py,readonly" \
      --tmpfs /app/logs --tmpfs /app/media --workdir /codigo \
      saas-hasner-core:latest \
      python /codigo/manage.py tenant_command shell --schema=juliani \
      --settings=config.settings.sombra \
      --command "exec(open('/sonda.py').read())"
  )
}
```

### 3. Reversão e apply da 11

- **Escritor genérico da foto pré-apply.** `carimbo_gravado --empresa ID --mes 11 --ano 2026` usa `foto()` e grava valores por colaborador e campo em `logs/carimbo_gravado/`. A obra da janela de HE é um precedente explícito. Seu apply grava as fotos `antes` em `logs/janela_he/` antes de alterar cadastro e chamar o recalculador. `recalcular_fechamento --apply` só grava seu próprio JSON **depois** do recálculo. `app/ponto/management/commands/carimbo_gravado.py:29-56` `app/ponto/management/commands/carimbo_gravado.py:101-113` `app/ponto/management/commands/carimbo_gravado.py:145-155` `app/ponto/management/commands/aplicar_janela_he_total.py:75-90` `app/ponto/management/commands/aplicar_janela_he_total.py:116-146` `app/ponto/management/commands/recalcular_fechamento.py:130-151`
- **Precedentes de reversão em `logs/`.** `bin/snapshot_relavratura_10_2026.py` escreve `/app/logs/reversao_relavratura_10_2026.json` e `bin/snapshot_o263_comp10.py` escreve `/app/logs/reversao_o263_comp10.json`. Ambos guardam mensal, `DiaPago.motor` e IDs sem mensal anterior. Para esta obra, a foto precisa cobrir também `DiaPago.oraculo`, pois a porta relavra as duas versões. `restaurar_fechamento` repõe campos de linhas mensais existentes. A existência de um arquivo de reversão específico da 11 em `logs/*/reversao*` segue [não verificado]. `bin/snapshot_relavratura_10_2026.py:25-64` `bin/snapshot_o263_comp10.py:53-84` `app/ponto/services/fechamento.py:1235-1239` `app/ponto/services/fechamento.py:1262-1266` `app/ponto/services/fechamento.py:1300-1319` `app/ponto/services/dia_pago.py:368-373` `app/ponto/services/fechamento.py:1398-1437`
- **Porta do apply.** É `tenant_command recalcular_fechamento --schema=juliani --mes 11 --ano 2026 --empresa ID --apply`, uma chamada por empresa do escopo. A chamada encaminha apenas `(11, 2026, ID)` e grava mensal e lavra nessa chave. A 12 pode mudar por uma chamada independente após o deploy, por exemplo, pelo recálculo por evento que resolve a competência da data do fato. Os contadores `recusas_oraculo` e `falhas_tecnicas_oraculo` saem no resumo da lavra. Seus valores e a prova pós-apply seguem [não verificado]. `app/ponto/management/commands/recalcular_fechamento.py:77-81` `app/ponto/management/commands/recalcular_fechamento.py:110-133` `app/ponto/services/fechamento.py:962-973` `app/ponto/services/fechamento.py:1235-1239` `app/ponto/services/fechamento.py:1320-1351` `app/ponto/services/fechamento.py:1836-1853`

### 4. Hash do FechamentoMensal de 06, 07, 08 e 09

A função da casa é **`ponto.portas.regen_exportada.hash_do_gravado(colaborador, mes, ano)`**. Ela consulta o mensal gravado e retorna 12 caracteres do MD5. O conjunto exclui chaves e carimbos como `previsto_em`, `derivado_em` e `atualizado_em`. Para um digest por empresa e competência, use SHA-256 do JSON ordenado de `carimbo_gravado.foto(eid, mes, 2026)` e publique também a contagem de linhas. Repita nos quatro meses, no mesmo banco antes e depois do apply, incluindo todas as empresas para a prova histórica. `app/ponto/portas/regen_exportada.py:35-38` `app/ponto/portas/regen_exportada.py:56-74` `app/ponto/portas/regen_exportada.py:121-122` `app/ponto/portas/regen_exportada.py:194-195` `app/ponto/management/commands/carimbo_gravado.py:29-56`

```python
from hashlib import sha256
import json

from colaboradores.models import Empresa
from ponto.management.commands.carimbo_gravado import foto

for mes in (6, 7, 8, 9):
    for eid in Empresa.objects.order_by("pk").values_list("pk", flat=True):
        mensal = foto(eid, mes, 2026)
        dados = json.dumps(mensal, sort_keys=True, separators=(",", ":")).encode()
        print(f"{mes:02d}/2026 emp{eid} linhas={len(mensal)} sha256={sha256(dados).hexdigest()}")
```

`ExportacaoDominio.hash_sha256` é o campo do **TXT**. Os hashes do mensal antes e depois do apply seguem [não verificado]. `app/folha/models.py:75-84` `app/ponto/portas/regen_exportada.py:62-74`

## 28/11 01:43 · sessao a0c0bb04d7f1 · RETRATO-PDF-CORTA-POR-QUE · censo · origem=Code

# Dossiê antes do construtor · RETRATO-PDF-CORTA-POR-QUE, item 2

O vínculo **dia → motivo** já existe na fonte. Ele se perde em `fora_da_folha`, que entrega à lavra os motivos agregados e as datas planas. O aval fornecido autoriza preservar esse vínculo em chave opcional, usar um mapa de rótulos junto ao produtor e mostrar no **Por quê** apenas as datas da janela. `app/folha/export.py:1268` · `app/folha/fora_da_folha.py:214` · `app/folha/fora_da_folha.py:234` · `app/folha/fora_da_folha.py:246` · `aval.txt:2`

Usei o aval anexado. A entrada indicada em `PROMPTS.md` está [nao verificado] na cópia, cujo arquivo termina na linha 3194. O SHA Git da cópia, as linhas de produção e a execução da suíte estão [nao verificado]. `app/docs/PROMPTS.md:3194`

## 1. Lavrador e contrato da fonte

- **Escritor único em produção.** `lavrar_retrato_folha --apply` chama `montar_linhas` e persiste `linhas` e `lavrado_em` por `empresa`, `mes` e `ano` com `update_or_create`. O cron usa o mesmo comando. O selo AST cobra um único importador de produção de `montar_linhas`. A linha nasce em `montar_linhas`; `o_que_fazer` é leitor. `app/folha/management/commands/lavrar_retrato_folha.py:65` · `app/folha/management/commands/lavrar_retrato_folha.py:72` · `app/folha/management/commands/lavrar_retrato_folha.py:77` · `app/config/crons.py:1659` · `app/folha/tests/test_retrato_folha_um_derivador.py:68` · `app/folha/retrato_folha.py:1237` · `app/folha/retrato_folha.py:1079`
- **`tipo`.** A lavra grava um valor por colaborador. Para `furo_espelho`, `_tipo_cru` consulta `TIPO_POR_CAUSA` com a `causa` já gravada em `FechamentoMensal.causa_espelho`. Para os demais bloqueios, usa `TIPO_POR_MOTIVO`. Há `sem_causa` para bloqueio sem tipo conhecido e `''` para quem entra ou sai da conta. `app/folha/retrato_folha.py:379` · `app/folha/retrato_folha.py:1167` · `app/folha/retrato_folha.py:1250`
- **`datas`.** Para `furo_espelho`, `fora_da_folha` já tem `_dias` vindos de `com_dias=True` e projeta `[str(d['data']) for d in _dias]`. Quando `_c.get('dias')` é `None`, a lavra tenta `dias_ferias_com_batida` do item do export. Grava uma lista plana `AAAA-MM-DD` ou `None` quando falta forma de dia. Retratos anteriores à E2 podem vir sem `datas`. O `informativo` de rescisão tem chave própria. `app/folha/fora_da_folha.py:115` · `app/folha/fora_da_folha.py:246` · `app/folha/export.py:885` · `app/folha/retrato_folha.py:1227` · `app/folha/retrato_folha.py:1243` · `app/folha/retrato_folha.py:1255` · `app/folha/models.py:334`
- **Retorno exato de `motivos_retencao_celula(com_dias=True)`.** Tupla `(motivos, dias)`. `motivos` é `list[str]`, sem duplicatas e ordenada lexicalmente. `dias` é `list[dict]`, em ordem cronológica, com um registro por **dia de retenção do colaborador**. As chaves são `data: datetime.date`, `motivo: str`, `veredito: str | None` e `celula_id: int | None`. `data` é o dia civil da célula, sem hora, ainda em Python. Na falta da célula, os dois últimos campos são `None`; célula existente sem veredito conserva o ID. Isento, desligado, intervalo colapsado ou nenhum dia retido resultam em `([], [])`. O modo padrão devolve só a lista de motivos. `app/folha/export.py:1214` · `app/folha/export.py:1247` · `app/folha/export.py:1265` · `app/folha/export.py:1268` · `app/folha/export.py:1273` · `app/folha/export.py:1304` · `app/escala/models.py:1696` · `app/escala/models.py:1783` · `app/escala/apps.py:5`

## 2. Leitores do JSON e tolerância a chave nova

`RetratoFolha.linhas` é `JSONField` sem validador de esquema por linha na declaração. `linhas_do_retrato` devolve os dicionários pelo recorte e valida o argumento `filtro`. `recortar` valida grupos e valores do filtro. A chave extra atravessa esses leitores. `app/folha/models.py:350` · `app/folha/retrato_folha.py:98` · `app/folha/retrato_folha.py:647`

| Leitor | O que faz com a chave desconhecida |
|---|---|
| **Tela inteira, parcial, barra e contadores** | `_recorte_da_barra` abre as linhas e `_cadastro` copia o dicionário. A barra lê os eixos conhecidos; `_tabela` projeta campos explícitos e hoje passa `motivo` a `rotulo_do_motivo`. A chave extra passa pelo recorte e fica fora da saída. `app/folha/views_retrato.py:116` · `app/folha/views_retrato.py:194` · `app/folha/views_retrato.py:278` · `app/folha/views_retrato.py:324` · `app/folha/views_retrato.py:345` · `app/folha/retrato_folha.py:114` |
| **PDF do retrato e Raio-X** | Usam `_recorte_da_barra`. O PDF extrai as células explicitamente. O Raio-X agrupa por `tipo` e projeta `motivo` e `dias`. Ambos toleram a chave e hoje a ignoram. `app/folha/views_retrato.py:388` · `app/folha/views_retrato.py:414` · `app/folha/views_retrato.py:489` · `app/folha/views_retrato.py:500` |
| **Drawer do colaborador** | `linha_do_colab` devolve o dicionário inteiro. `o_que_fazer`, `portas_da_linha` e o template leem campos específicos. A chave extra chega à linha e hoje fica sem consumo. O drawer mostra o rótulo agregado, o tipo único e as datas planas em ISO. `app/colaboradores/views.py:994` · `app/folha/retrato_folha.py:950` · `app/folha/retrato_folha.py:1047` · `app/folha/retrato_folha.py:1112` · `app/templates/folha/_retrato_o_que_fazer.html:42` |
| **Comandos e frescor** | `vigiar_retido_calado` lê os bloqueados e campos específicos, com tratamento próprio para ausência de `dias_sem_leitura`. Tolera a chave extra. `lavrar_retrato_folha` conta a linha recém-montada em memória, inclusive no dry-run. O frescor consulta o carimbo `lavrado_em`. `app/folha/management/commands/vigiar_retido_calado.py:30` · `app/folha/vigia_retido_calado.py:77` · `app/folha/management/commands/lavrar_retrato_folha.py:65` · `app/folha/retrato_folha.py:1413` |
| **TXT** | A geração segue `classificar_export` → `montar_linhas_export` e valida o layout do TXT. A descarga do histórico devolve `ExportacaoDominio.conteudo`. A chave do JSON do retrato está fora das duas rotas. `app/folha/views.py:219` · `app/folha/views.py:275` · `app/folha/views.py:282` · `app/folha/export.py:719` · `app/folha/views.py:574` |
| **Testes** | O selo da lavra compara o **conjunto exato de chaves** de uma linha e acusa a adição nessa fixture. O teste de idempotência compara duas lavras completas. O teste de lookup do drawer exige que a linha volte inteira. Os testes de período e de literal de mapa cobram a saída exibida. Os demais consumidores acima leem campos selecionados. `app/folha/tests/test_retrato_folha_quadros.py:1425` · `app/folha/tests/test_retrato_folha_contador.py:270` · `app/folha/tests/test_retrato_folha_drawer.py:140` · `app/folha/tests/test_retrato_folha_drawer.py:213` · `app/folha/tests/test_retrato_folha_periodo.py:371` |

## 3. Universo diário e molde AST

**Oito códigos possíveis na cópia.** `sem_celula` sai de dois caminhos do laço. Os outros sete são `espelho_furo`, `espelho_discordante`, `espelho_indefinida`, `espelho_cobranca_indevida`, `espelho_cobrado`, `espelho_nunca_bateu` e `espelho_fato_em_ausencia`, formados por `'espelho_' + c.veredito` para a tupla fechada `CelulaDia.VEREDITOS_ACUSAM`. Os protestos não cobráveis e a isenção de pausa podem suprimir o dia. `furo_espelho` é o motivo agregado do item do export. `ferias_com_batida` tem sua própria fonte de datas. `app/folha/export.py:1281` · `app/folha/export.py:1294` · `app/folha/export.py:1302` · `app/escala/models.py:1780` · `app/folha/export.py:977` · `app/folha/export.py:885`

`MotivoExaustivoTest` fica em `app/folha/tests/test_retrato_folha_quadros.py:314`. `_motivos_da_fonte` faz `ast.parse` de `export.py`, localiza `classificar_export` e colhe strings não vazias de atribuições a `['motivo']` e de pares literais em dicionários. O teste antívacuidade exige pelo menos seis códigos e dois exemplos concretos. O teste de cobertura subtrai os destinos declarados e exige conjunto vazio. Para o selo diário, o mesmo molde precisa colher as chamadas a `_retem` em `_dias_sem_decisao` e resolver a concatenação pelo AST de `VEREDITOS_ACUSAM`. Assim um emissor novo sem rótulo deixa o selo vermelho. Os textos dos oito rótulos estão [nao verificado]. `app/folha/tests/test_retrato_folha_quadros.py:255` · `app/folha/tests/test_retrato_folha_quadros.py:317` · `app/folha/tests/test_retrato_folha_quadros.py:323` · `app/folha/export.py:1268` · `app/folha/export.py:1302` · `app/escala/models.py:1780` · `aval.txt:2`

## 4. Janela do Q2

É o intervalo inclusivo `de_data` / `ate_data` do calendário do filtro universal. `cfiltro.ler(request.GET)` valida as datas ISO e troca pontas invertidas. Uma ponta só já recorta. `_recorte_da_barra` passa os limites a `no_periodo`, que usa as `datas` planas para ajustar `dias` e `saldo` em cópia e conserva a lista integral. A janela sozinha mantém as linhas no universo. A janela da **competência**, usada pela fonte na lavra, vem de `janela_fechamento` e do corte da empresa. `app/templates/core/_filtro_campos.html:43` · `app/core/filtro.py:68` · `app/core/filtro.py:98` · `app/folha/views_retrato.py:216` · `app/folha/retrato_folha.py:1334` · `app/folha/fora_da_folha.py:146` · `app/ponto/janelas.py:54`

Tela, PDF e Raio-X recebem esse recorte. Os links da lista e do Raio-X abrem o drawer com `mes` e `ano`. O drawer busca a linha integral e hoje mostra as datas ISO de toda a competência. `app/folha/views_retrato.py:230` · `app/folha/views_retrato.py:337` · `app/templates/folha/_retrato_lista.html:60` · `app/templates/folha/_retrato_raiox.html:68` · `app/colaboradores/views.py:994` · `app/templates/folha/_retrato_o_que_fazer.html:42`

## 5. Matriz de RED

| Caso | Asserção |
|---|---|
| **Linha nova** | Dar à fonte `03/11` e `05/11` com `espelho_furo` e `12/11` com `espelho_cobrado`. Conferir que a lavra preserva os três pares da chamada já feita e que o **Por quê** usa os rótulos do mapa do produtor, com cada data sob seu motivo. Cobrir também férias, cuja lista plana vem de outra fonte. `app/folha/fora_da_folha.py:115` · `app/folha/fora_da_folha.py:214` · `app/folha/retrato_folha.py:1231` · `aval.txt:2` |
| **Linha antiga sem a chave** | Abrir a mesma linha legada. Conferir o rótulo agregado na tela e no PDF, o tipo único e as datas ISO planas no drawer. Incluir legado anterior à E2, com `datas` ausente e Dias vazio sob janela. `app/folha/views_retrato.py:291` · `app/folha/views_retrato.py:424` · `app/folha/retrato_folha.py:1118` · `app/templates/folha/_retrato_o_que_fazer.html:42` · `app/folha/tests/test_retrato_folha_periodo.py:172` |
| **Janela que corta** | Com `05/11` a `12/11`, exigir `05/11` e `12/11` em **Por quê**, dois dias contados e a linha gravada intacta. Cobrir uma ponta só e janela que corta todos os dias. O selo atual de período compara todas as colunas textuais salvo Dias e precisa contemplar a mudança autorizada em **Por quê**. A saída textual quando sobram zero pares está [nao verificado]. `app/folha/retrato_folha.py:1334` · `app/folha/tests/test_retrato_folha_periodo.py:155` · `app/folha/tests/test_retrato_folha_periodo.py:371` · `aval.txt:2` |
| **Motivo sem rótulo** | Retirar uma entrada do mapa ou acrescentar um emissor no produtor. O selo AST deve falhar informando o código faltante. Uma varredura vazia também deve falhar. `app/folha/tests/test_retrato_folha_quadros.py:317` · `app/folha/tests/test_retrato_folha_quadros.py:323` · `aval.txt:2` |
| **Tela, PDF e drawer leem o mesmo** | Alimentar as três portas com a mesma linha persistida. Cobrar igualdade de rótulos e datas em tela e PDF sob a mesma query, e leitura dos mesmos pares no drawer. Cobrar zero chamadas ao retentor nesses leitores. A igualdade de apresentação e a janela no drawer dependem da pergunta 1. `app/folha/views_retrato.py:260` · `app/folha/views_retrato.py:388` · `app/colaboradores/views.py:1001` · `app/folha/tests/test_retrato_pdf.py:138` · `app/folha/tests/test_retrato_folha_drawer.py:213` |

## 6. Arquivos × obras em voo

| Área | Toque previsto e cruzamento |
|---|---|
| **Fonte e lavra** | `app/folha/export.py` para o mapa, `app/folha/fora_da_folha.py` para transportar os pares já disponíveis e `app/folha/retrato_folha.py` para persistir e ler a chave. Cruzamento informado com **CODIGO-DOMINIO-VALIDO** em `export.py` e `retrato_folha.py`, que hoje também tratam o bloqueio de cadastro e sua projeção. `app/folha/export.py:881` · `app/folha/export.py:1185` · `app/folha/fora_da_folha.py:234` · `app/folha/retrato_folha.py:379` · `app/folha/retrato_folha.py:1237` |
| **Tela, PDF e Raio-X** | `app/folha/views_retrato.py` contém as três projeções e é o cruzamento informado com **RETRATO c1**. Os outros sítios informados da c1 são `app/relatorios/utils_audit.py` e `app/config/settings/base.py`. A ligação atual com o PDF auditável passa por `gerar_pdf_auditavel` e `get_commit_git`. `app/folha/views_retrato.py:291` · `app/folha/views_retrato.py:424` · `app/folha/views_retrato.py:443` · `app/folha/views_retrato.py:503` · `app/relatorios/pdf_base.py:135` · `app/relatorios/utils_audit.py:7` · `app/config/settings/base.py:4` |
| **Drawer e cadastro** | Se o drawer herdar a janela, os pontos são os links da lista e do Raio-X, `app/colaboradores/views.py` e o parcial `_retrato_o_que_fazer.html`. `colaboradores/views.py` também contém a edição do código de folha. `folha/views.py` contém o fluxo do TXT. O diff efetivo das duas obras em voo e a sobreposição exata nos trechos de views e cadastro estão [nao verificado]. `app/templates/folha/_retrato_lista.html:60` · `app/templates/folha/_retrato_raiox.html:68` · `app/colaboradores/views.py:994` · `app/colaboradores/views.py:1356` · `app/templates/folha/_retrato_o_que_fazer.html:42` · `app/folha/views.py:275` |

Os pontos de teste a tocar são o contrato de chaves da lavra, o molde AST, a janela, a paridade do PDF e o contrato do drawer conforme o alcance aprovado. A regra herdada é a testemunha ler a mesma autoridade, com um escritor canônico por estado. `app/folha/tests/test_retrato_folha_quadros.py:314` · `app/folha/tests/test_retrato_folha_quadros.py:1425` · `app/folha/tests/test_retrato_folha_periodo.py:371` · `app/folha/tests/test_retrato_pdf.py:138` · `app/folha/tests/test_retrato_folha_drawer.py:213` · `app/docs/LEIS.md:60` · `app/docs/LEIS.md:65`

## 7. Perguntas de lei

**0 para abrir a lavra e o recorte de tela/PDF.** Q1 e Q2 estão decididas no anexo. Restam dois detalhes de saída para fechar a matriz inteira. `aval.txt:2` · `CLAUDE.md:443`

1. **Drawer.** A paridade pedida inclui exibir os pares sob a mesma janela, inclusive ao abrir pelo Raio-X? Se inclui, o **Por quê** pode exibir rótulo com datas no próprio valor ou os pares devem aparecer em linhas de detalhe? O corte atual e o selo exigem valor literal de mapa no bloco. `app/docs/CORTES.md:405` · `app/folha/tests/test_retrato_folha_drawer.py:213` · `app/templates/folha/_retrato_raiox.html:68`
2. **Janela sem pares.** Para uma linha nova cujos pares ficam todos fora da janela, o **Por quê** fica vazio ou conserva o rótulo agregado, sem datas? O código atual define o vazio da coluna Dias; a saída de **Por quê** nesse caso está [nao verificado]. `app/folha/retrato_folha.py:1343` · `app/folha/views_retrato.py:291` · `aval.txt:2`

## 28/11 01:39 · sessao fb68df3fdb61 · RETRATO-PDF-CORTA-POR-QUE · verificador · origem=Code

**Parecer de verificação — RETRATO-PDF-CORTA-POR-QUE. O item 5 volta.** No bind-mount documentado, o leitor novo procura em `app/logs/` no host e o deploy grava em `logs/` na raiz do repositório. O RED do item 5 substitui o caminho padrão e deixa essa ligação sem prova. No item 4, os contadores vêm da barra, mas a precedência das linhas é definida na view do PDF e o RED deixa a posição de `None` sem cobertura. `app/docs/INFRA.md:22–30`, `app/config/settings/base.py:4–5`, `bin/deploy.sh:432–436`, `app/folha/views_retrato.py:399–420`, `app/folha/tests/test_retrato_pdf_pacote.py:60–84,228–267,274–315`.

Conferi a cópia e o diff anexo em leitura. A identidade Git da cópia, a execução do RED em `31ef2f199`, o GREEN informado de 358 testes e a configuração efetiva dos containers estão [nao verificado].

## 1. Quem escreve e quem lê

`JUIZES.md` é a tabela de consulta. O registro vivo mantém `classificar_export` como autoridade do fechamento e `tarefa_do_dia` como autoridade da porta diária. A permissão da tela é `tem_acao`, chamada tanto pela tela quanto pelo PDF. O PDF lê o retrato lavrado e delega a classificação visual a `led_da_linha`. Não encontrei uma segunda implementação desses juízes no diff. `app/docs/JUIZES.md:10–15,51,81–90,119–122`, `app/core/juizes.py:311,326–340`, `app/folha/views_retrato.py:257,377,391–405`, `app/folha/retrato_folha.py:516–532`.

| Campo ou valor | Quem produz ou grava | Quem lê |
|---|---|---|
| `RetratoFolha.linhas`, `colab`, `status`, `motivo`, `dias`, `datas`, `tipo`, `saldo`, `dono` | `classificar_export` entrega ID, estado e motivo. `fora_da_folha` entrega dias, datas e grupo de fio para o furo. `montar_linhas` compõe o JSON e `lavrar_retrato_folha --apply` grava por empresa, mês e ano. `app/folha/export.py:846–858,875–893,968–990`, `app/folha/fora_da_folha.py:146–165,210–253`, `app/folha/retrato_folha.py:1222–1255`, `app/folha/management/commands/lavrar_retrato_folha.py:65–79` | `linhas_do_retrato` abre o JSON. `_recorte_da_barra` monta a lista que tela, PDF e Raio-X leem. O drawer do colaborador e o vigia também passam pelo leitor do retrato. `app/folha/retrato_folha.py:98–111,930–953`, `app/folha/views_retrato.py:194–232,260–268,391–397,507–524`, `app/colaboradores/views.py:998–1004`, `app/folha/vigia_retido_calado.py:76–79` |
| `dono` gravado, dono de UI, cor e rótulo | `montar_linhas` grava o **fio** em `dono`. `fio_da_linha` o lê, com fallback para retrato legado. `dono_da_linha` projeta o balde da UI, incluindo `chamado → Admin` e `ninguem → Hoje` quando a primeira data é hoje ou posterior. `led_da_linha` acrescenta Entram e Rescisão como estados de linhas sem dono. `app/folha/retrato_folha.py:191–201,436–493,516–532,1246–1248` | Filtro, barra e drawer leem o dono de UI. Tela e PDF leem cor e rótulo via `desenho_do_led`. O PDF também lê a chave do LED para ordenar. O texto de `motivo` passa por `rotulo_do_motivo`, que lê `MOTIVO_RH`, também usado pela prévia e pelo PDF de retidos. `app/folha/retrato_folha.py:559–566,587–597,908–915,1112–1126,76–86`, `app/folha/views_retrato.py:286–292,399–405,435–446`, `app/folha/views.py:15–24,373,415,884–887` |
| Empresa, competência, filtros, `vis`, `universo`, `hoje` | `_recorte_da_barra` lê GET e preferência, usa `janela_atual` para o default, lê um instante e monta o recorte com `core.filtro`, `no_periodo`, `ler_grupos` e `recortar`. Quando vem `empresa=`, a própria porta chama `PreferenciaUI.set_pref`, inclusive no acesso pelo PDF. `app/folha/views_retrato.py:149–184,216–247`, `app/colaboradores/models.py:204–216` | Tela, PDF e Raio-X leem o recorte. `qs_do_recorte` produz o endereço do botão e a querystring guardada na auditoria. `core.filtro.chips` produz os rótulos e textos dos filtros ativos usados pela tela e pelo PDF. `app/folha/views_retrato.py:260–269,295–300,337,391–417,461–467,507–509`, `app/folha/retrato_folha.py:775–782`, `app/core/filtro.py:232–275` |
| `titulo`, `itens`, `ativo`, `rotulo`, `numero` da barra | `quadros_barra_retrato` monta o quadro. `_pilula` produz `numero` em **pessoas**. Para a pilula acesa, conta o recorte atual. Para a apagada, conta o recorte que sua ativação abriria. `app/folha/retrato_folha.py:785–813,838–845,879–915` | A barra e o PDF leem o mesmo `numero`. O PDF usa `ativo` para descrever o recorte e copia os pares rótulo/número de “Depende de”. O `n/total` do cabeçalho é outra medida, `len(vis)/len(linhas)`, tal como no filtro da tela. `app/templates/core/_barra_gestao.html:65–67`, `app/folha/views_retrato.py:315,324–325,411–430` |
| Razão social, nome, posto e descrição da escala | As portas de cadastro gravam empresa e posto, colaborador, vínculo de escala e `TipoEscala.descricao`. `_cadastro` projeta nome, posto e tipo de escala em memória, escolhendo o vínculo ativo mais recente por `data_inicio` e usando o posto do colaborador como fallback. `app/colaboradores/services/estrutura.py:13–22,43–45,67–79`, `app/colaboradores/views.py:1331–1333,1362–1365`, `app/colaboradores/services/colaborador.py:152–160,204–206`, `app/colaboradores/services/vinculo.py:274–292`, `app/escala/services/cadastro_tipo.py:184–188,307,382–401,431–446`, `app/folha/views_retrato.py:99–123` | O PDF usa a razão social no título e lê nome, posto e descrição nas células. A tela lê a mesma projeção cadastral. O item 3 passa a descrição por `escape` e `Paragraph` na coluna de 38 mm. Seu RED exige `Paragraph` e texto longo integral. A quebra visual na página e o comportamento com `<`, `>` e `&` estão [nao verificado]. `app/folha/views_retrato.py:395,424–425,435–450`, `app/templates/folha/_retrato_lista.html:60–66`, `app/folha/tests/test_retrato_pdf_pacote.py:47–48,210–222` |
| PDF, metadados e registro auditável | `retrato_pdf` fornece título, usuário, período, `tipo='outro'`, recorte e totais. `gerar_pdf_auditavel` escreve `RegistroPDFAuditavel`, incluindo `filtros_json`, `total_registros`, `commit_git` e o SHA-256 do PDF canônico. A view escreve `Content-Disposition` na resposta. `app/folha/views_retrato.py:461–475`, `app/relatorios/pdf_base.py:133–178` | O rodapé lê o commit e o SHA-256. A verificação pública busca pelo SHA-256 do PDF e exibe o commit armazenado, os filtros e o total. `app/relatorios/pdf_base.py:72–87`, `app/chamados/views.py:1202–1222`, `app/templates/chamados/verificar_pdf.html:37–50` |

## 2. Item 4 — `dias=None`, ordem e RED

- **O leitor que produz `None` é `no_periodo`.** Com pelo menos um limite, ele copia a linha e põe `dias=None` se `datas` estiver ausente, nula, vazia ou sem interseção com a janela inclusiva. Isso vale para qualquer motivo. Sem janela, conserva o valor lavrado. A cópia preserva `datas`, então uma linha pode continuar no dono Hoje com a coluna de dias vazia. `app/folha/retrato_folha.py:486–493,1334–1347`.
- **A lavra também produz `None`.** Entram, Rescisão, `sem_codigo_dominio`, `cadastro_zero` e `nao_homologado` chegam sem lista de dias. Férias com batida entrega datas. Para `furo_espelho`, `fora_da_folha` pode entregar o número `0` se a lista estiver vazia, e a lavra preserva esse zero. Tela e PDF exibem `None` em branco e `0` como número. Quantas linhas caem em cada caso em produção [nao verificado]. `app/folha/export.py:846–858,875–893,968–990`, `app/folha/fora_da_folha.py:146–165,234–246`, `app/folha/retrato_folha.py:1227–1243`, `app/templates/folha/_retrato_lista.html:66`, `app/folha/views_retrato.py:443`.
- **O RED de ordem deixa `None` sem cobertura.** A bancada tem `None` só em Entram e Rescisão, sem números nesses mesmos estados. Rodei em memória a chave extraída do código com a bancada, retirando apenas `dias is None`. A ordem esperada continuou igual. No controle sintético de mesmo dono, com ID 1 em `None` e ID 2 em `0`, a chave atual ordenou `[2, 1]` e a mutação ordenou `[1, 2]`. Os selos anteriores cobrem a produção de `None` na janela e na lavra. A decisão de posição no papel fica [nao verificado]. `app/folha/views_retrato.py:399–406`, `app/folha/tests/test_retrato_pdf_pacote.py:52–99,228–243`, `app/folha/tests/test_retrato_folha_periodo.py:126–153`, `app/folha/tests/test_retrato_folha_quadros.py:1409–1418`.
- **A ordem da tela ficou intacta.** `_recorte_da_barra` continua a fazer `ordenar(recortar(...))`, por balde, faixa crescente de saldo e ID. A view projeta `_vis` na mesma sequência e o template percorre `tabela` na página cheia e no parcial. Só o PDF cria outra lista com `sorted`. A guarda do pacote exige as mesmas pessoas e uma ordem diferente da do papel. O teste anterior de `ordenar` fixa a sequência esperada. `app/folha/views_retrato.py:225–232,260–268,278–293,318–346,391–406,435`, `app/folha/retrato_folha.py:754–772`, `app/templates/folha/_retrato_lista.html:35`, `app/templates/folha/retrato_folha.html:53`, `app/templates/folha/_retrato_parcial.html:7`, `app/folha/tests/test_retrato_pdf_pacote.py:245–249`, `app/folha/tests/test_retrato_folha_quadros.py:566–591`.
- **Fonte única.** O PDF lê o recorte existente e os números publicados pelo quadro. O RED compara esses números em dois recortes. O selo AST guarda a porta do universo, e a origem dos contadores é demonstrada pelo código. Para a ordem das linhas, a view lê `vis`, `dias`, `colab` e a chave do LED, mas escreve localmente a precedência `Sem ação → Sem leitura → Admin → Colab → Hoje → Entram → Rescisão`. O quadro publica os donos em outra sequência. Assim, a ordem reaproveita o julgamento de dono e tem regra de apresentação própria. Se o checklist exige que **a precedência** também venha publicada por `quadros_barra_retrato` ou `_recorte_da_barra`, essa exigência fica aberta. `app/folha/views_retrato.py:399–420`, `app/folha/retrato_folha.py:191–201,590–591,908–915`, `app/folha/tests/test_retrato_pdf_pacote.py:251–267`, `app/folha/tests/test_retrato_pdf.py:301–350`.

## 3. Item 5 — carimbo, alcance e efeitos

- **Caminho em produção.** A configuração SaaS herda `BASE_DIR / 'logs' / 'deploy.stamp'`, que no container vira `/app/logs/deploy.stamp`. O bind documentado o resolve para `/home/ronald/saas-hasner/app/logs/deploy.stamp`. `bin/deploy.sh` grava `/home/ronald/saas-hasner/logs/deploy.stamp`, via arquivo temporário e `mv`. Esse bind não expõe o arquivo escrito. Compose efetivo, montagem adicional, cópia manual e ambiente `COMMIT_GIT` do worker [nao verificado]. `app/config/settings/base.py:4–5`, `app/config/settings/saas.py:10,118`, `app/docs/INFRA.md:22–30`, `bin/deploy.sh:432–436`, `.gitignore:40–41`.
- **Comparação com o ramo antigo.** No cenário dado, sem `COMMIT_GIT`, sem `/app/.git` e sem arquivo no novo caminho, ambos retornam `''`. O rodapé segue `n/d` e o registro segue vazio. Um `COMMIT_GIT` não vazio continua prioritário, com `strip()` e limite de 40 caracteres. Onde `/app/.git` existia e funcionava, perde-se o fallback antigo. Se o carimbo ficar acessível, falha de leitura ou UTF-8 e `QUANDO` não numérico podem agora interromper a geração. O parser devolve `{}` quando falta `QUANDO`. Falhas reais em produção [nao verificado]. `obra.diff` anexo, hunk de `app/relatorios/utils_audit.py:1`, `app/relatorios/utils_audit.py:11–20`, `app/core/esteira_vigia.py:1032–1042`, `app/relatorios/pdf_base.py:72–77,135,164`.
- **Quem escreve e lê `COMMIT`.** O deploy escreve epoch em `QUANDO`, `git rev-parse HEAD` inteiro em `COMMIT` e o assunto em `TITULO`. O PDF reaproveita o parser do vigia e usa só `COMMIT`, limitado a 40 caracteres e sem validação de formato. O registro conserva esse valor e o rodapé exibe oito caracteres. O vigia lê no host e usa o SHA inteiro em `git revert`. A guarda de auth e o verificador de imports também leem o SHA do host, e o handoff o abrevia para exibição. Quem injeta `COMMIT_GIT` em produção [nao verificado]. `bin/deploy.sh:432–436`, `app/core/esteira_vigia.py:64,1032–1042,1202,1297–1309,1355`, `app/relatorios/utils_audit.py:13–20`, `app/relatorios/models.py:56`, `app/relatorios/pdf_base.py:72,164`, `bin/janela_auth.sh:39–45`, `bin/import_tardio_contra_o_ar.py:45–55`, `bin/handoff_sessao.sh:274–277`.
- **Dois chamadores diretos.** `gerar_pdf_auditavel` usa o mesmo commit nas duas passagens e no registro. Alcança retrato, espelho mensal, histórico de chamados, gestão de férias e gestão de HE. Se o commit impresso mudar, muda também a entrada do SHA-256 canônico. O outro chamador é o espelho informacional, usado no avulso por datas e no lote. Ele usa o commit só no rodapé. Uma falha comum aos dois caminhos pode fazer o lote pular a pessoa. `app/relatorios/pdf_base.py:133–178`, `app/folha/views_retrato.py:461–467`, `app/relatorios/pdf_espelho.py:1066–1074,1106–1116`, `app/relatorios/pdf_chamados.py:158–176`, `app/relatorios/services.py:235–255,338–347`, `app/relatorios/views.py:1135–1164`, `app/ponto/views.py:886–894`.
- **RED e ordem de publicação.** O RED prova leitor, persistência e passagem do SHA aos dois objetos de rodapé, com arquivo temporário e `override_settings`. O controle prova precedência da variável. O caminho padrão e os caracteres efetivamente impressos ficam fora das asserções. O deploy recarrega e prova as rotas **antes** de trocar o carimbo. Mesmo se o arquivo ficar visível, essa ordem permite código novo com carimbo anterior ou ausente. No fluxo `--reload-copia`, o reload ocorre antes do commit da cópia, e o carimbo lê o `HEAD` naquele momento. Ocorrência no ar [nao verificado]. `app/folha/tests/test_retrato_pdf_pacote.py:274–322`, `bin/deploy.sh:357–373,416–436`, `bin/molde_fatia/cadeia.sh:35–48,62–69`.

## 4. Import, relógio e travas

O import novo é `utils_audit → core.esteira_vigia`. O topo do vigia só importa biblioteca padrão, registra um logger, inicializa constantes e compila sete regex. A cadeia estática não volta a `relatorios`. O corpo do módulo não lê os arquivos operacionais nem resolve settings, e o CLI está protegido por `__main__`. Na chamada sem `COMMIT_GIT`, o helper resolve o setting, consulta o caminho e lê o arquivo se presente. Num processo local novo, `-X importtime` mediu cerca de **26 ms cumulativos** para importar o vigia e suas dependências, sem carregar Django. Custo incremental no worker e memória [nao verificado]. `app/relatorios/utils_audit.py:2–20`, `app/core/esteira_vigia.py:27–36,38–109,351–357,605,1202,1297,1355,1653–1654`.

O `hoje` novo na ordenação vem do instante já capturado na borda de `_recorte_da_barra`. Esse mesmo dia chega à barra e ao LED. O timestamp da auditoria segue uma leitura própria, explicitamente fora do contrato do recorte. Os trechos novos não adquirem trava. O pipeline mantém bloqueios fornecidos pelo chamador antes da geração, com a gravação entre as duas passagens. O retrato não fornece callbacks de bloqueio. O selo de relógio verifica o congelamento em testes, e a bancada está sob `freeze_time`. `app/folha/views_retrato.py:146–147,177–184,244–247,399–446,461–467`, `app/folha/retrato_folha.py:452–465`, `app/relatorios/utils_audit.py:13–20`, `app/relatorios/pdf_base.py:127–178`, `app/core/tests/test_selo_teste_sem_relogio.py:67–84`, `app/folha/tests/test_retrato_pdf_pacote.py:121–123`.

## 5. Item 1 — alcance do `attachment`

**Nenhum outro consumidor do partial encontrado aponta para essa rota.** `retrato_pdf` escreve o novo header apenas na própria resposta. A barra já fornece `pdf_href`, que o partial renderiza como âncora comum, sem `target`. Os outros consumidores continuam usando seus próprios destinos e headers, como Gestão HE (`inline`) e Furos (`attachment`). O RED exige `attachment` com o nome canônico e verifica a âncora na página cheia e no parcial OOB. O download efetivo no navegador [nao verificado]. `app/folha/urls.py:25`, `app/folha/views_retrato.py:337,468–475`, `app/templates/folha/_retrato_barra.html:50`, `app/templates/core/_btn_pdf.html:34–46`, `app/ponto/views.py:899`, `app/relatorios/views.py:1632`, `app/folha/tests/test_retrato_pdf_pacote.py:187–204`.

## 28/11 01:38 · sessao 39f9bf608e62 · CODIGO-DOMINIO-VALIDO · construtor · origem=Code

A cura está no diff da cópia. A revisão estática encontrou **13 arquivos de produção alterados e 0 `except` adicionados**. Os dois módulos do contrato declaram **22 casos, dos quais 15 têm prefixo `test_RED_`**. Resultado da suíte e GREEN [nao verificado]. [REDs da Folha](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/tests/test_codigo_dominio_invalido.py:90) [REDs do Cadastro](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/colaboradores/tests/test_codigo_dominio_numerico.py:76) [nao verificado]

### Diff por item

- **Juiz e TXT.** Um predicado compartilhado exige dígitos ASCII após `strip()`, sem impor comprimento. `classificar_export` preserva o valor no item e atribui o motivo literal `codigo_dominio_invalido` entre código ausente e férias com batida. O montador já seleciona os itens `entra`. [colaboradores/services/cadastro.py:53](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/colaboradores/services/cadastro.py:53) [folha/export.py:850](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/export.py:850) [folha/export.py:882](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/export.py:882) [folha/export.py:723](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/export.py:723)
- **Cadastro.** Admissão e edição reexibem o formulário com a mensagem «Código Domínio deve conter apenas números.» antes dos respectivos pontos de gravação. O campo vazio segue aceito na admissão e preserva o anterior na edição. A validação fica nas duas portas de tela. [colaboradores/views.py:1135](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/colaboradores/views.py:1135) [colaboradores/views.py:1239](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/colaboradores/views.py:1239) [colaboradores/views.py:1296](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/colaboradores/views.py:1296) [colaboradores/views.py:1365](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/colaboradores/views.py:1365) [core/respostas.py:74](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/core/respostas.py:74)
- **Retrato.** `MOTIVO_RH` dá o rótulo, `TIPO_POR_MOTIVO` dá `cadastro` e o fio existente dá `admin`. Um único leitor de `linha['codigo']` monta `codigo Dominio = <valor>` para lista, PDF, drawer e Raio-X. No drawer, `valor` continua sendo o rótulo literal do mapa. [folha/views.py:18](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/views.py:18) [folha/retrato_folha.py:223](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/retrato_folha.py:223) [folha/retrato_folha.py:382](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/retrato_folha.py:382) [folha/retrato_folha.py:89](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/retrato_folha.py:89) [templates/folha/_retrato_lista.html:65](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/templates/folha/_retrato_lista.html:65) [folha/views_retrato.py:423](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/views_retrato.py:423) [templates/folha/_retrato_o_que_fazer.html:37](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/templates/folha/_retrato_o_que_fazer.html:37) [templates/folha/_retrato_raiox.html:73](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/templates/folha/_retrato_raiox.html:73)
- **Listas fechadas.** `_MOTIVO_NA_TELA` ganhou pílula própria, `_PORTAS_CADASTRO` ganhou a natureza correspondente e Quem Cobrar lê o rótulo de `MOTIVO_RH`. `MotivoExaustivo` já colhe a atribuição literal por AST e verifica seu destino no mapa. O Raio-X do export já conta a chave em **Outras pendências**. [ponto/services/fechamento_lista.py:28](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/ponto/services/fechamento_lista.py:28) [templates/ponto/_fech_linha.html:19](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/templates/ponto/_fech_linha.html:19) [relatorios/views_pend.py:50](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/relatorios/views_pend.py:50) [relatorios/views.py:1729](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/relatorios/views.py:1729) [folha/tests/test_retrato_folha_quadros.py:325](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/tests/test_retrato_folha_quadros.py:325) [folha/views.py:625](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/views.py:625)

### REDs e mutantes para o Opus

| Caso RED | Mutante a derrubar |
|---|---|
| No lote misto, `VIGIA` e `１２３` ficam fora, `00123` gera `0000000123` e o layout retorna `[]`. Em colisão com férias com batida, vence o código inválido. Vazio e brancos mantêm `sem_codigo_dominio`. | Usar só `isdigit()`, remover a retenção, mover a checagem para depois de férias ou eliminar também o controle numérico. [TXT](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/tests/test_codigo_dominio_invalido.py:90) [Precedência](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/tests/test_codigo_dominio_invalido.py:152) |
| Conferir rótulo, tipo, pílula, natureza e presença literal na fonte. Em Quem Cobrar, conferir o rótulo na linha retida. | Apagar uma entrada de mapa, apontar a pílula para espelho ou trocar a atribuição literal do motivo por uma referência indireta. [Mapas e AST](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/tests/test_codigo_dominio_invalido.py:163) [Quem Cobrar](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/relatorios/views.py:1729) |
| Lavrar `VIGIA` com tipo `cadastro` e dono `admin`, corrigir o cadastro para `00123` e conferir o valor histórico na lista, PDF e drawer. | Buscar o código atual, omitir o complemento em um leitor ou interpolá-lo em `valor` no drawer. [Retrato](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/tests/test_codigo_dominio_invalido.py:241) |
| Enviar `VIGIA` nas duas portas e exigir formulário HTTP 200 e mensagem visível. Na admissão, conferir as contagens de usuários e colaboradores. Na edição, conferir código e telefone anteriores. | Validar depois da gravação, redirecionar a recusa ou levar a regra ao modelo e impedir a criação do legado usado na fixture. [Admissão](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/colaboradores/tests/test_codigo_dominio_numerico.py:85) [Edição](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/colaboradores/tests/test_codigo_dominio_numerico.py:118) [Fixture legada](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/tests/test_codigo_dominio_invalido.py:64) |

### Comando do GREEN

```bash
bash bin/suite.sh --dir /home/ronald/wt-codex-codigo-dominio-valido-013223 --only "folha.tests.test_codigo_dominio_invalido colaboradores.tests.test_codigo_dominio_numerico folha.tests.test_export ponto relatorios core.tests.test_selo_teste_sem_relogio"
```

## 28/11 01:37 · sessao 8ac04350367a · - · - · origem=Ronald (tmux)

**Item 4. O RED deixa a posição de `dias=None` em aberto.** O teste de ordem exige a sequência por dono, dias decrescentes e desempate por ID. Na bancada, `None` só aparece em Entram e Rescisão, sem dias numéricos nesses estados. Retirar a chave `dias is None` preservaria a ordem esperada da bancada. Para distinguir a chave de `-(dias or 0)` em um teste, o mesmo estado precisa de `0` e `None` com o ID menor no `None`. O zero é possível em `furo_espelho` cujo relatório devolve `len([])` e chega à lavra como zero. `app/folha/tests/test_retrato_pdf_pacote.py:52–98`, `:228–243`, `app/folha/views_retrato.py:399–406`, `app/folha/fora_da_folha.py:210–246`, `app/folha/retrato_folha.py:1227–1236`.

**Origens de `None`.** Na lavra, `montar_linhas` recebe a contagem de `furo_espelho` de `fora_da_folha` e usa a lista de férias quando essa contagem vem ausente. Sem as duas fontes, grava `None`. Isso abrange Entram, Rescisão, `sem_codigo_dominio`, `cadastro_zero` e `nao_homologado`. O leitor `no_periodo`, com pelo menos um limite temporal, cria uma cópia com `dias=None` quando `datas` está ausente/nula, vazia ou fora da janela. Isso se aplica a qualquer motivo. Sem janela, mantém os valores da lavra. Há testes para esses vazios, inclusive retrato legado, e para a faixa Cadastro permanecer. `app/folha/export.py:846–858`, `:875–893`, `:968–989`, `app/folha/fora_da_folha.py:146–165`, `:234–246`, `app/folha/retrato_folha.py:1222–1243`, `:1334–1347`, `app/folha/tests/test_retrato_folha_periodo.py:126–153`, `:172–183`, `app/folha/tests/test_retrato_folha_quadros.py:1394–1417`.

**Ordem da tela.** `_recorte_da_barra` segue produzindo `ordenar(recortar(...))`, com ordem por balde, faixa de saldo e ID. A tela projeta `_vis` na mesma ordem e o template percorre `tabela`. O PDF cria outra lista com `sorted(_vis, ...)`. O teste da tela no pacote confere mesmas pessoas e ordem diferente do PDF. A ordem exata da função e seu comportamento com o recorte de datas estão em testes anteriores. `app/folha/views_retrato.py:225–232`, `:260–268`, `:278–293`, `:318–346`, `:391–406`, `:435`, `app/folha/retrato_folha.py:754–772`, `app/templates/folha/_retrato_lista.html:35`, `app/folha/tests/test_retrato_pdf_pacote.py:245–249`, `app/folha/tests/test_retrato_folha_quadros.py:566–590`, `app/folha/tests/test_retrato_folha_periodo.py:354–368`.

**Juízes, campos e contadores.** O gravado `dono` é o fio escrito por `montar_linhas` e lido por `fio_da_linha`. O dono de UI é produzido por `dono_da_linha` e usado pela barra e pelo LED, com dois estados adicionais do LED para Entram e Rescisão. `hoje` vem da borda e é passado a todos os leitores. `_pilula` publica `numero` como quantidade de pessoas do recorte atual se acesa ou do recorte após ativação se apagada. Tela e PDF leem o número de `quadros_barra_retrato`. A ordem do PDF usa a chave e as linhas de suas fontes, com uma precedência local de apresentação distinta da sequência publicada no quadro. O RED de contadores compara resultados em dois recortes. O selo AST anterior guarda a fonte do universo, enquanto a dependência de `quadros_barra_retrato` é comprovada pelo código. `app/folha/management/commands/lavrar_retrato_folha.py:65–79`, `app/folha/retrato_folha.py:191–201`, `:436–493`, `:516–532`, `:588–597`, `:816–842`, `:904–915`, `:1246–1248`, `app/folha/views_retrato.py:177–182`, `:246–247`, `:324–325`, `:399–420`, `app/templates/core/_barra_gestao.html:67`, `app/folha/tests/test_retrato_pdf_pacote.py:251–267`, `app/folha/tests/test_retrato_pdf.py:301–350`.

**Item 3.** O RED verifica `Paragraph` e preservação do texto, com um exemplo longo sem metacaracteres. A troca para `Paragraph(escape(descricao))` acontece no leitor PDF. A geometria da quebra nas páginas e um exemplo com `<`, `>` ou `&` não recebem asserção direta. `app/folha/tests/test_retrato_pdf_pacote.py:46–48`, `:164–173`, `:210–222`, `app/folha/views_retrato.py:424`, `:441`, `:449–450`. Execução da suíte e distribuição em produção [nao verificado].

## 28/11 01:37 · sessao ca3696a25ee3 · - · - · origem=Ronald (tmux)

- **Import.** `utils_audit` importa o vigia no nível do módulo. O vigia importa apenas biblioteca padrão na carga, inicializa logger, constantes e sete regexes. A leitura do carimbo e o acesso ao valor em `settings` acontecem na chamada de `get_commit_git()` sem `COMMIT_GIT`. A rotina CLI fica sob `__main__`. A cadeia estática desse import não tem volta a `relatorios`. Tempo e memória adicionais [nao verificado]. `app/relatorios/utils_audit.py:2-20`, `app/core/esteira_vigia.py:27-36,38-81,83,90,101,104-109,351-357,605,1202,1297,1355,1653-1654`.
- **Attachment.** Só a resposta de `folha:retrato_pdf` recebe o novo `Content-Disposition`. A barra passa o URL dessa rota como `pdf_href`, que já era âncora direta sem `target`. Os modos submit e `hxAbrir` continuam definidos pelo mesmo partial e nenhum outro consumidor encontrado usa essa rota. Há exemplos independentes de inline em Cadastro × Realidade, HE e follow-up, e de attachment em Furos, Inteligência e histórico de exportações. `app/folha/urls.py:25`, `app/folha/views_retrato.py:337,461-475`, `app/templates/folha/_retrato_barra.html:50`, `app/templates/core/_btn_pdf.html:34-46`, `app/escala/views.py:528`, `app/ponto/views.py:899`, `app/relatorios/views.py:1110,1632`, `app/inteligencia/views.py:643`, `app/folha/views.py:861`.
- **Mordida.** O RED do item 1 exige attachment com nome canônico. A verificação da âncora é guarda de markup da página cheia e do parcial OOB. Download efetivo no navegador [nao verificado]. O RED de Escala exige `Paragraph` e texto integral com um caso longo. Quebra visual e caracteres especiais [nao verificado]. `app/folha/tests/test_retrato_pdf_pacote.py:47-49,187-204,210-222`.
- **Relógio e ordem do deploy.** O instante do recorte vem da borda, vira `hoje` e alimenta as novas chamadas. O timestamp da auditoria tem relógio próprio preexistente e exclusão explícita do contrato do recorte. Os trechos novos não adquirem locks. O reload e as sondas de rota vêm antes do `mv` do carimbo no host. Se esse carimbo passar a ser visível no container, há uma janela possível de código novo com stamp velho ou ausente. `app/folha/views_retrato.py:146-147,177-184,244-247,399-446`, `app/relatorios/pdf_base.py:129-135`, `app/relatorios/utils_audit.py:13-20`, `bin/deploy.sh:357-373,416-419,432-436`.

## 28/11 01:36 · sessao 5e85d18b9370 · - · - · origem=Ronald (tmux)

**Item 5 deve voltar.** A montagem documentada leva `/home/ronald/saas-hasner/app` a `/app`. O novo default aponta para `/app/logs/deploy.stamp`, equivalente a `app/logs/deploy.stamp` no host. `bin/deploy.sh` grava `logs/deploy.stamp` na raiz do repositório, por arquivo temporário e `mv`. Assim, o bind-mount documentado não expõe o carimbo ao PDF. O Compose efetivo, montagens extras e `COMMIT_GIT` do processo ficam [nao verificado]. `app/docs/INFRA.md:22–30`, `app/ponto/management/commands/apurar_furo_da_exportada.py:25–28`, `app/config/settings/base.py:4–5`, `app/config/settings/saas.py:10`, `bin/deploy.sh:432–436`, `.gitignore:40–41`.

- **Efeito na produção descrita.** Com `COMMIT_GIT`, a precedência e o limite de 40 caracteres seguem iguais. Sem a variável, sem `/app/.git` e sem carimbo no caminho novo, os dois ramos devolvem `''`. O rodapé fica `n/d` e o registro guarda vazio. Ambientes com checkout em `/app/.git` perdem aquele fallback. Se o carimbo passar a ser acessível, `read_text` sem tratamento pode propagar falha de leitura ou UTF‑8. `ler_deploy_stamp` devolve `{}` sem `QUANDO` e pode levantar `ValueError` com `QUANDO` não numérico. [nao verificado] o estado real desses arquivos. `obra.diff` anexo, `app/relatorios/utils_audit.py:11–20`, `app/core/esteira_vigia.py:1032–1042`, `app/relatorios/pdf_base.py:72–77,135,164`.
- **Consumidores.** `gerar_pdf_auditavel` lê uma vez e passa o valor às duas fases e a `RegistroPDFAuditavel.commit_git`. Isso alcança retrato, espelho mensal, histórico de chamados, gestão de férias e gestão de HE. O espelho informacional é o outro chamador direto e usa o valor só no rodapé. O lote usa os dois caminhos e pode pular o colaborador se ambos levantarem exceção. `app/relatorios/pdf_base.py:133–170`, `app/folha/views_retrato.py:461–467`, `app/relatorios/pdf_espelho.py:1041–1074,1106–1116`, `app/relatorios/pdf_chamados.py:158–176`, `app/relatorios/services.py:235–255,338–347`, `app/ponto/views.py:886–894`.
- **Semântica.** O shell escreve `git rev-parse HEAD` no `COMMIT`. O helper retorna até 40 caracteres sem validar formato. O rodapé mostra oito, a verificação pública mostra o valor persistido inteiro e a consulta usa o SHA‑256 do PDF canônico. O vigia passa o `COMMIT` inteiro ao `git revert` e usa oito só nas mensagens. `bin/deploy.sh:432–436`, `app/relatorios/utils_audit.py:13–20`, `app/relatorios/models.py:56`, `app/relatorios/pdf_base.py:72,147–164`, `app/chamados/views.py:1202–1222`, `app/templates/chamados/verificar_pdf.html:37,49`, `app/core/esteira_vigia.py:1297–1309`.
- **Alcance do RED.** Os testes criam carimbo temporário e substituem o setting. Checam o valor devolvido, o registro e o argumento recebido pelos dois objetos de rodapé. O controle cobre a precedência de `COMMIT_GIT`. O caminho padrão, a montagem e o texto efetivamente renderizado estão fora dessas asserções. `app/folha/tests/test_retrato_pdf_pacote.py:274–322`.
- **Import.** O topo de `core.esteira_vigia` carrega biblioteca padrão, cria logger, constantes e compila sete regex. As leituras ficam dentro de funções e o `main` é protegido por `__name__`. A inspeção estática não mostra ciclo nem leitura de disco ou settings pelo corpo do módulo. `utils_audit` resolve o setting na chamada. Tempo de import [nao verificado]. `app/core/esteira_vigia.py:27–36,64,83–109,351–357,605,1653–1654`, `app/relatorios/utils_audit.py:6–20`.

## 28/11 01:34 · sessao df0356fdefca · CODIGO-DOMINIO-VALIDO · leitura · origem=Code

```python
# Execução na sombra e contagens do recorte [nao verificado].
import re
from datetime import date

from django.conf import settings
from django.db import connection, transaction
from django_tenants.utils import schema_context

# Guarda da sombra (app/config/settings/sombra.py:29,49,60).
if (
    getattr(settings, "HASNER_SOMBRA", False) is not True
    or connection.settings_dict["NAME"] != "sombra"
):
    raise SystemExit(1)

from colaboradores.models import Empresa
from folha.export import classificar_export, janela_competencia

mes, ano = 11, 2026
entra = 0
maior_que_10 = []
nao_ascii_digito = []

# Schema do executor (bin/sonda_leitura.sh:239-240).
with schema_context("juliani"), transaction.atomic(durable=True):
    with connection.cursor() as cursor:
        # Transação somente leitura (bin/gate_cobranca.sh:23-25).
        cursor.execute("SET TRANSACTION READ ONLY")

    empresa = Empresa.objects.get(pk=2)
    # Janela pelo corte da empresa (app/folha/export.py:47-51 e app/ponto/janelas.py:36-57).
    if janela_competencia(empresa, mes, ano) != (date(2026, 10, 21), date(2026, 11, 20)):
        raise SystemExit(2)

    # Fechamentos gravados e código já normalizado com strip (app/folha/export.py:774-797,846-850).
    for item in classificar_export(empresa, mes, ano):
        if item["status"] != "entra":
            continue
        entra += 1
        codigo = item["codigo_dominio"]
        pk = item["colaborador_id"]
        if len(codigo) > 10:
            maior_que_10.append(pk)
        if re.fullmatch(r"[0-9]+", codigo) is None:
            nao_ascii_digito.append(pk)

    transaction.set_rollback(True)

for pk in sorted(maior_que_10):
    print(f"maior_que_10|col{pk:03d}")
for pk in sorted(nao_ascii_digito):
    print(f"nao_ascii_digito|col{pk:03d}")

print(
    f"S11|entra={entra}|maior_que_10={len(maior_que_10)}"
    f"|nao_ascii_digito={len(nao_ascii_digito)}"
)
```

## 28/11 01:25 · sessao 5f422d0683f6 · RETRATO-PDF-CORTA-POR-QUE · construtor · origem=Code

Implementei os itens 1, 3, 4 e 5 em três arquivos de produção. O item 2 e os arquivos de teste ficaram fora do diff. `folha/views_retrato.py:350` · `relatorios/utils_audit.py:11` · `config/settings/base.py:5`

### Cura por item

- **1 — Download.** A rota responde `attachment; filename="retrato-folha-emp<pk>-<MM>-<AAAA>.pdf"`. O link continua no partial único, sem `target` nem atributos `hx-*`. `folha/views_retrato.py:473` · `templates/folha/_retrato_barra.html:50` · `templates/core/_btn_pdf.html:34`
- **3 — Escala.** A descrição inteira entra em `Paragraph(escape(...))` na coluna de 38 mm. `folha/views_retrato.py:424` · `folha/views_retrato.py:441` · `folha/views_retrato.py:450`
- **4 — Ordem e contadores.** Só o papel ordena por Sem ação → Sem leitura → Admin → Colab → Hoje → Entram → Rescisão, com mais dias primeiro e ID crescente no empate. Dentro de cada dono, coloquei `dias=None` depois das linhas com número. O topo imprime `Rótulo: N` com o `numero` de cada pílula de `Depende de` retornada por `quadros_barra_retrato`. A ordem da tela, o recorte, a contagem e o cabeçalho da tabela foram preservados. `folha/views_retrato.py:232` · `folha/views_retrato.py:324` · `folha/views_retrato.py:399` · `folha/views_retrato.py:418` · `folha/views_retrato.py:432` · `folha/views_retrato.py:467`
- **5 — Carimbo.** `COMMIT_GIT` preenchida tem precedência. Em seguida, o leitor único usa `ler_deploy_stamp` pelo caminho `settings.HASNER_COMMIT_STAMP`. Arquivo ausente retorna `''`. O padrão resolve para `/app/logs/deploy.stamp` no container. O escritor grava no `logs/` da raiz do host e o bind documentado monta `app/` em `/app`. A exposição do carimbo nesse caminho em produção está [nao verificado]. `relatorios/utils_audit.py:11` · `core/esteira_vigia.py:1032` · `config/settings/base.py:4` · `bin/deploy.sh:436` · `CLAUDE.md:88`

### Leitores

- `_recorte_da_barra` é chamado pela tela, pelo PDF e pelo Raio‑X. `folha/views_retrato.py:260` · `folha/views_retrato.py:391` · `folha/views_retrato.py:507`
- `get_commit_git` é chamado diretamente por `gerar_pdf_auditavel` e pelo espelho informacional. Pela porta auditável, a mudança alcança o retrato, o espelho individual, o lote de espelhos, o histórico de chamados e o PDF de gestão de HE. `relatorios/pdf_base.py:34` · `relatorios/pdf_base.py:135` · `relatorios/pdf_espelho.py:1070` · `folha/views_retrato.py:461` · `relatorios/pdf_espelho.py:1106` · `relatorios/services.py:338` · `relatorios/pdf_chamados.py:168` · `ponto/views.py:887`

### REDs e mutantes para o opus

| RED | Caso e mutantes que deve pegar |
|---|---|
| **1. Download** | Conferir o `attachment` e o nome exato pela rota, além do link no HTML cheio e no parcial filtrado. Mutantes `inline`, nome alterado ou `target`/`hx-*` no link. `folha/tests/test_retrato_pdf_pacote.py:187` |
| **3. Escala** | Usar descrição longa com `<` e `&`, conferir o texto integral no `Paragraph` e a quebra dentro da coluna. Mutantes de string crua, truncamento ou retirada de `escape`. `folha/tests/test_retrato_pdf_pacote.py:210` · `folha/views_retrato.py:441` |
| **4a. Ordem** | Intercalar os sete estados, incluir dias distintos na mesma faixa e um empate com nomes em ordem inversa à dos IDs. Conferir a ordem da tela e a igualdade do conjunto de IDs. Mutantes de ordem compartilhada, dias crescentes, ordenação por faixa ou desempate por nome. `folha/tests/test_retrato_pdf_pacote.py:228` · `folha/tests/test_retrato_pdf_pacote.py:245` |
| **4b. Contadores** | Comparar todos os `Rótulo: N` com a tela, usando filtro universal, janela e Admin ativo. Mutantes que contam a partir das linhas visíveis, usam o universo inteiro ou imprimem só a pílula ativa. `folha/tests/test_retrato_pdf_pacote.py:251` · `folha/views_retrato.py:411` |
| **5a. Fonte do SHA** | Ler um carimbo válido sem `COMMIT_GIT`, repetir com a variável preenchida e com o arquivo ausente. Mutantes que ignoram o caminho configurado, preferem o carimbo ao ambiente ou voltam a consultar Git. `folha/tests/test_retrato_pdf_pacote.py:292` · `folha/tests/test_retrato_pdf_pacote.py:317` · `relatorios/utils_audit.py:11` |
| **5b. Trilha** | Conferir o SHA inteiro no registro e nas duas chamadas do rodapé, e os oito primeiros caracteres no texto impresso. Mutantes que gravam o SHA só no registro, omitem uma passagem ou truncam o registro. `folha/tests/test_retrato_pdf_pacote.py:300` · `relatorios/pdf_base.py:72` · `relatorios/pdf_base.py:139` · `relatorios/pdf_base.py:164` · `relatorios/pdf_base.py:170` |

**Diff** com **0 `except` adicionados e 1 removido**. A sintaxe dos três arquivos e `git diff --check` foram conferidos. Resultado da suíte e renderização real do PDF [nao verificado]. `relatorios/utils_audit.py:11` · `folha/views_retrato.py:350` · `config/settings/base.py:5`

```bash
bash bin/suite.sh --dir /home/ronald/wt-codex-retrato-pdf-corta-por-que-012113 --only "folha.tests.test_retrato_pdf_pacote folha.tests.test_retrato_pdf relatorios core.tests.test_selo_teste_sem_relogio"
```

## 28/11 01:24 · sessao d2ce5ce5364d · INVENTARIO-PELA-REGRA · leitura · origem=Code

```python
import sys

sys.dont_write_bytecode = True

import json
import logging
from collections import defaultdict
from datetime import date
from functools import lru_cache
from html.parser import HTMLParser
from types import SimpleNamespace
from typing import NamedTuple


def parar(fonte, motivo):
    print(f"BECO|ABORTOU|{fonte}|{motivo}", flush=True)
    raise SystemExit(1) from None


def fonte_segura(caminho, linha):
    _, separador, relativo = caminho.rpartition("/app/")
    return f"{'app/' + relativo if separador else caminho.rsplit('/', 1)[-1]}:{linha}"


class FalhasSemDados:
    def __enter__(self):
        self.anterior = logging.getLogRecordFactory()

        def fabrica(*args, **kwargs):
            registro = self.anterior(*args, **kwargs)
            if registro.levelno >= logging.ERROR or (
                registro.exc_info and registro.exc_info[0]
            ):
                parar(fonte_segura(registro.pathname, registro.lineno), "erro_do_leitor")
            registro.msg, registro.args = "", ()
            registro.levelno = logging.NOTSET - 1
            return registro

        logging.setLogRecordFactory(fabrica)

    def __exit__(self, tipo, erro, quadro):
        logging.setLogRecordFactory(self.anterior)
        if tipo is None or (tipo is SystemExit and erro.code == 1):
            return False
        while quadro.tb_next is not None:
            quadro = quadro.tb_next
        parar(fonte_segura(quadro.tb_frame.f_code.co_filename, quadro.tb_lineno), tipo.__name__)


with FalhasSemDados():
    from django.conf import settings
    from django.db import connection
    from django.utils import timezone
    from django_tenants.utils import schema_context

    if getattr(settings, "HASNER_SOMBRA", False) is not True:
        parar("bin/sonda_leitura.sh:94", "HASNER_SOMBRA_ausente")
    db = connection.settings_dict
    if (db["NAME"], db["HOST"]) != ("sombra", "sombra_db"):
        parar("bin/sonda_leitura.sh:94", "conexao_fora_da_sombra")
    if connection.in_atomic_block or not connection.get_autocommit():
        parar("app/chamados/models.py:761", "sonda_exige_transacao_propria")

    from django.contrib.auth.models import User
    from django.db.models import Q
    from django.template import Context, engines

    from chamados.catalogo.motor import TERMINAIS, VIVOS, decidir
    from chamados.juizes import (
        _dia_de_turno_da_pergunta,
        competencia_do_chamado,
        dia_do_fato,
        pergunta_viva,
        perguntas_vivas_de,
    )
    from chamados.models import (
        ChamadoColaborador,
        DecisaoPergunta,
        DisputaSupervisao,
        PerguntaDisputa,
        motivo_implausivel_validacao,
    )
    from chamados.reconciliador import (
        _veredito_premissa,
        eventos_pendentes,
        retencao_da_retratacao,
    )
    from chamados.services.acoes_chamado import montar_contexto_detalhe
    from chamados.services.fio import (
        chamado_que_lista,
        contexto_parcial_fio,
        data_do_fato_de,
        disputas_do_chamado_em_lote,
    )
    from chamados.services.materializacao import (
        _data_em_disputa,
        estado_aplicacao_pergunta,
        espera_declarado_x_gravado,
    )
    from chamados.services.tarefa_do_dia import (
        carga_da_tarefa,
        exigencia_das_perguntas,
        porta_da_tarefa,
    )
    from chamados.services.validacao import (
        _e_aceite_de_folga,
        _resposta_de_hora,
        entrega_que_barra_a_validacao,
        fila_validar_classificada,
    )
    from chamados.services.verbos import particionar_verbos
    from chamados.utils import pode_agir_chamado
    from chamados.views import _is_gestor
    from chamados.views_cobrar import _pode_validar_lote
    from colaboradores.mixins import tem_acao
    from colaboradores.models import Empresa
    from folha.fora_da_folha import dias_que_seguram
    from folha.models import RetratoFolha
    from folha.retrato_folha import TIPO_SEM_CAUSA, TIPOS, dono_da_linha
    from ponto.janelas import janela_fechamento
    from ponto.services.declarar_dia import checar_declaracao, o_dia_ja_e_folga
    from ponto.supra_juiz import perguntas_que_seguram_csf


MES, ANO = 11, 2026
EMPRESAS = (2, 3, 4)
GESTOR_S1_PK = 909
NV = "[nao verificado]"
FAMILIAS = ("a", "b", "c", "d", "e", "e_retido_pelo_fechamento",
            "e_retido_na_retratacao", "f")
OBRAS = {
    "a": ("O261-FIO-MORTO-PELO-RETRATO", 1132),
    "b": ("ORFAS-VOLTOU", 2102),
    "c": ("FDS-S1-C2-SEM-ACAO", 2197),
    "d": ("O261-E-CELULA-20-UM-DONO", 1012),
    "e": ("FDS-S1-C3-PREMISSA-MORTA", 2198),
    "f": ("FDS-S1-C1-SEM-SAIDA", 2196),
}
ACOES = {"validar_pergunta", "recusar_pergunta", "corrigir_resposta", "conferir_data"}


class Linha(NamedTuple):
    colab: int
    dia: date | None
    referencia: str
    motivo: str
    fonte: str
    obra: tuple


def ref(pk):
    return str(pk) if pk is not None else "-"


def dia_pergunta(p):
    # O fallback pela celula e a costura da S1. app/chamados/juizes.py:58,339
    dia = _dia_de_turno_da_pergunta(p)
    if dia is None and not p.conversa and p.celula_id:
        return p.celula.data, "celula"
    return dia, "dia_do_turno"


def consulta_perguntas(emp):
    return PerguntaDisputa.objects.filter(disputa__colaborador__empresa_id=emp).select_related(
        "disputa__colaborador__empresa", "disputa__chamado",
        "chamado_destino__colaborador", "chamado_destino__celula", "celula",
    ).prefetch_related("decisoes")


class Cartoes(HTMLParser):
    def __init__(self, html, chamado_local=None):
        super().__init__()
        self.perguntas = defaultdict(set)
        self.pares = defaultdict(set)
        self.formularios = defaultdict(list)
        self.chamado_local = chamado_local
        self.formulario = None
        self.feed(html)
        self.close()

    def handle_starttag(self, tag, atributos):
        a = dict(atributos)
        if "data-beco-pergunta" in a:
            self.perguntas[int(a["data-beco-pergunta"])].add(int(a["data-beco-chamado"]))
        if "data-beco-par" in a:
            self.pares[int(a["data-beco-par"])].add(int(a["data-beco-chamado"]))
        if tag == "form":
            self.formulario = {}
            if "data-beco-conferir" in a:
                self.formulario.update(
                    acao="conferir_data",
                    pergunta_id=a["data-beco-conferir"],
                    chamado_id=a["data-beco-chamado"],
                )
        elif tag == "input" and self.formulario is not None and a.get("type") == "hidden":
            nome = a.get("name")
            if nome in {"acao", "pergunta_id", "chamado_id", "confirmar_implausivel"}:
                self.formulario[nome] = a.get("value", "")

    def handle_endtag(self, tag):
        if tag != "form" or self.formulario is None:
            return
        f, self.formulario = self.formulario, None
        if f.get("acao") in ACOES:
            cid = int(f.get("chamado_id", self.chamado_local))
            self.formularios[int(f["pergunta_id"])].append(
                (cid, f["acao"], f.get("confirmar_implausivel") == "1")
            )


@lru_cache(maxsize=2)
def template_marcado(modal):
    # Marcadores em memoria nas ramificacoes reais. O HTML fica em memoria.
    # app/templates/chamados/partials/modal_fio.html:160,173,219,470
    # app/templates/chamados/partials/_pergunta_card.html:28,31
    engine = engines["django"].engine
    nome = "modal_fio.html" if modal else "_pergunta_card.html"
    caminho = f"chamados/partials/{nome}"
    fonte = engine.get_template(caminho).source
    pk = "perg.pk"
    card = "chamado.pk"

    def marca(tipo, pergunta=pk):
        return f'<span data-beco-{tipo}="{{{{ {pergunta} }}}}" data-beco-chamado="{{{{ {card} }}}}"></span>'

    resposta = (
        "<strong>Resposta:</strong> {{ perg.resposta_colab }}" if modal
        else "<strong>Resposta do colab:</strong> {{ perg.resposta_display }}"
    )
    pergunta = "{{ perg.pergunta }}</div>"
    trocas = [(pergunta, marca("pergunta") + pergunta), (resposta, marca("par") + resposta)]
    if modal:
        gaveta = "{{ it.dia }}</span>"
        conferir = """<form hx-post="{% url 'chamados:conferir_data_pergunta' perg.pk %}" """
        trocas.extend((
            (gaveta, marca("pergunta", "it.pergunta_id") + gaveta),
            (conferir, conferir + 'data-beco-conferir="{{ perg.pk }}" data-beco-chamado="{{ chamado.pk }}" '),
        ))
    for original, marcado in trocas:
        if fonte.count(original) != 1:
            parar(f"app/templates/{caminho}:1", "marcador_do_template_mudou")
        fonte = fonte.replace(original, marcado, 1)
    return engine.from_string(fonte)


@lru_cache(maxsize=None)
def chamado_de(pk):
    return ChamadoColaborador.objects.select_related(
        "colaborador__empresa", "celula"
    ).get(pk=pk)


def validar_em_leitura(p, confirma):
    # RECONSTRUIDO: testemunhas da S1, sem executar o ato sob trava.
    # app/chamados/services/validacao.py:218,755,779,782
    if not p.respondida or DecisaoPergunta.composta(p).validada_em is not None:
        return False
    if not confirma and motivo_implausivel_validacao(p):
        return False
    if _e_aceite_de_folga(p):
        dia, colab = p.data_evento, p.disputa.colaborador
        return bool(dia and (
            o_dia_ja_e_folga(colab, dia)
            or checar_declaracao(colaborador=colab, dias=[dia], tipo="folga")["aceitos"]
        ))
    if entrega_que_barra_a_validacao(p):
        return False
    if _resposta_de_hora(p):
        if espera_declarado_x_gravado(p):
            return False
        aplicacao = estado_aplicacao_pergunta(p.disputa, p)
        if aplicacao is None:
            parar("app/chamados/services/materializacao.py:880", "sem_testemunha_da_resposta_de_hora")
        return aplicacao["estado"] in ("aplicavel", "ja_satisfeita", "aplicada") [nao verificado]
    return not (p.tipo_resposta == "data_hora" and _data_em_disputa(p))


class TelaGestor:
    def __init__(self, conta, verbos, lote):
        self.conta = conta
        self.req = SimpleNamespace(user=conta, GET={})
        self.verbos = verbos
        self.lote = lote
        self.pode_inline = tem_acao(conta, "gerir_chamados")

    @lru_cache(maxsize=None)
    def modal(self, colab):
        # O GET marca lidas. O construtor do parcial le o mesmo fio.
        # app/chamados/views.py:870,881,890; app/chamados/services/fio.py:573,610
        ctx = contexto_parcial_fio(self.req, colab)
        ctx["cor_disputas_map"] = DisputaSupervisao.cor_map_por_colaborador(colab)
        for ch in ctx["chamados"]:
            if ch.disputa_obj is not None and ch.explicacao is None:
                parar("app/chamados/services/fio.py:437", "explicador_sem_resposta")
            for p in ch.perguntas_disputa:
                estado = p.estado_aplicacao or {}
                if estado.get("detalhe") == "estado de aplicacao indisponivel -- sem validar":
                    parar("app/chamados/services/fio.py:470", "erro_na_testemunha_da_aplicacao")
        return Cartoes(template_marcado(True).render(Context(ctx, use_l10n=False)))

    @lru_cache(maxsize=None)
    def local(self, pk):
        ch = chamado_de(pk)
        # Colisao GLPI -> detalhe local, igual a casca. app/chamados/views.py:219
        if ch.ticket_id_glpi is not None:
            alvo = ChamadoColaborador.objects.filter(
                pk=ch.ticket_id_glpi, ticket_id_glpi__isnull=True
            ).values_list("pk", flat=True).first()
            return self.local(alvo) if alvo is not None else Cartoes("")
        ctx = montar_contexto_detalhe(self.req, ch, True, ())
        html = "".join(
            template_marcado(False).render(Context({**ctx, "perg": p}, use_l10n=False))
            for p in ctx["perguntas_disputa"]
        )
        return Cartoes(html, chamado_local=pk)

    @lru_cache(maxsize=None)
    def eleita(self, pk):
        return disputas_do_chamado_em_lote([pk]).get(pk)

    @lru_cache(maxsize=None)
    def inline(self, pk):
        ch = chamado_de(pk)
        if not (self.pode_inline and self.verbos.get(pk) == "validar"
                and pode_agir_chamado(self.conta, ch)):
            return set()
        d = self.eleita(pk)
        if d is None or d.colaborador_id != ch.colaborador_id:
            return set()
        return {p.pk for p in perguntas_vivas_de(d) if p.respondida}

    def formularios(self, p, cartoes, canal):
        ancora = chamado_que_lista(p)
        for cid, acao, confirma in cartoes.formularios.get(p.pk, ()):
            if acao == "conferir_data":
                # O POST e por pergunta, com a permissao de tela. app/chamados/views.py:1288
                if self.pode_inline and data_do_fato_de(p)[1] and p.data_conferida_em is None:
                    yield acao
                continue
            if cid != ancora or not pode_agir_chamado(self.conta, chamado_de(cid)):
                continue
            if canal == "local" and not p.disputa.aberta:
                continue
            if acao == "validar_pergunta":
                if canal == "fio" and chamado_de(cid).colaborador_id != p.disputa.colaborador_id:
                    continue
                if validar_em_leitura(p, confirma):
                    yield acao
                continue
            # app/chamados/services/fio.py:694,709,776
            if not p.disputa.aberta or (canal == "fio" and self.eleita(cid) is None):
                continue
            if DecisaoPergunta.composta(p).validada_em is not None:
                continue
            if acao == "recusar_pergunta" and p.respondida and not p.fora_do_fato():
                yield acao
            if acao == "corrigir_resposta" and p.tipo_resposta == "hora":
                yield acao

    def consultar(self, p, ch=None, *, medir_acoes=False):
        ids = {chamado_que_lista(p), p.disputa.chamado_id, ch.pk if ch else None} - {None}
        colabs = {p.disputa.colaborador} | {chamado_de(pk).colaborador for pk in ids}
        canais = [("fio", self.modal(colab)) for colab in sorted(colabs, key=lambda c: c.pk)]
        canais += [("local", self.local(pk)) for pk in sorted(ids)]
        no_lote = p.pk in self.lote  # app/chamados/views_cobrar.py:371
        visivel = no_lote or any(p.pk in c.perguntas for _, c in canais)
        par = no_lote or any(p.pk in c.pares for _, c in canais)
        acoes = set()
        if medir_acoes and par:
            acoes = {acao for canal, c in canais for acao in self.formularios(p, c, canal)}
            if (no_lote or any(p.pk in self.inline(pk) for pk in ids)) and validar_em_leitura(p, False):
                acoes.add("validar_pergunta")
        return visivel, par, acoes


def medir_a(empresa, janela, agora, hoje, vivos, perguntas_dia, tela):
    universo = dias_que_seguram(empresa, MES, ANO, hoje=hoje)
    ancoras = defaultdict(set)
    for ch in vivos.values():
        dia = dia_do_fato(ch, fortes=True)
        if dia is not None:
            ancoras[ch.colaborador_id, dia].add(ch.pk)
    for chave, perguntas in perguntas_dia.items():
        for p, _ in perguntas:
            pk = chamado_que_lista(p)
            if pk in vivos and vivos[pk].colaborador_id == chave[0]:
                ancoras[chave].add(pk)

    candidatos = {}
    for cid, (_, dias) in universo["dias"].items():
        for r in dias:
            chave = (cid, r["data"])
            if chave not in ancoras:
                candidatos[chave] = r
    if not candidatos:
        return []

    colabs = universo["colabs"]
    carga = carga_da_tarefa(
        [colabs[cid] for cid in sorted({cid for cid, _ in candidatos})],
        *janela, agora,
    )
    linhas = []
    for (cid, dia), r in sorted(candidatos.items()):
        tarefa = carga.tarefa(colabs[cid], dia)
        porta = porta_da_tarefa(tarefa)
        fila = (porta or {}).get("fila") or ()
        if any(pk in vivos and vivos[pk].colaborador_id == cid for pk in fila):
            continue
        ps = [p for p, viva in perguntas_dia.get((cid, dia), ()) if viva]
        if any(tela.consultar(p)[0] for p in ps):
            continue
        linhas.append(Linha(
            cid, dia, "-",
            f"{r['motivo']} celula={ref(r['celula_id'])} "
            f"rota_tarefa={(porta or {}).get('rota') or '-'} "
            f"fio_tarefa={(porta or {}).get('fio') or '-'} nao_sei={int(tarefa['nao_sei'])} "
            "RECONSTRUIDO: juncao_colab_dia_ancora_e_superficies_admin",
            "app/folha/export.py:1240", OBRAS["a"],
        ))
    return linhas


def medir_c(emp, janela, agora, hoje, vivos, tela, sem_dia):
    pares = defaultdict(set)
    for ch in vivos.values():
        motor = decidir(ch.status_local, "RECONCILIAR")
        if motor.permitido and motor.novo_estado in TERMINAIS:
            plano = ch._plano_pergunta_viva(motor.novo_estado)
            if not ch._gate_pergunta_viva(motor.novo_estado, registrar=False, plano=plano):
                for p in plano[0]:
                    pares[ch.pk, p.pk].add("pergunta_viva_na_competencia_aberta")
        if not ch._gate_conversa_do_pai(registrar=False):
            for pk in perguntas_que_seguram_csf(ch):
                pares[ch.pk, pk].add("conversa_exige_decisao_humana")

    # A pergunta e relida no estado corrente, pois o plano julgou o pos-estado.
    ps = consulta_perguntas(emp).in_bulk({pk for _, pk in pares})
    encontrados = []
    for (ch_pk, p_pk), guardas in sorted(pares.items()):
        p = ps.get(p_pk)
        if p is None or not p.respondida or not pergunta_viva(p, hoje=hoje, agora=agora):
            continue
        ch = vivos[ch_pk]
        dia, fonte_dia = dia_pergunta(p)
        if dia is None:
            comp = competencia_do_chamado(ch)
            dia = comp.dia if comp is not None else None
            fonte_dia = "competencia_do_chamado"
        if dia is not None and not janela[0] <= dia <= janela[1]:
            continue
        _, par, acoes = tela.consultar(p, ch, medir_acoes=True)
        if not par or acoes:
            continue
        if dia is None:
            sem_dia["c"] += 1
            continue
        encontrados.append((ch, p, dia, fonte_dia, guardas))

    razoes = exigencia_das_perguntas({p.pk for _, p, _, _, _ in encontrados}, agora)
    linhas = []
    for ch, p, dia, fonte_dia, guardas in encontrados:
        razao, _ = razoes[p.pk]
        nascente = (
            "app/chamados/models.py:429" if "pergunta_viva_na_competencia_aberta" in guardas
            else "app/ponto/supra_juiz.py:561"
        )
        linhas.append(Linha(
            p.disputa.colaborador_id, dia, str(ch.pk),
            f"pergunta={p.pk} disputa={p.disputa_id} ancora={ref(chamado_que_lista(p))} "
            f"guardas={','.join(sorted(guardas))} tarefa={razao.partition(':')[0]} "
            f"RECONSTRUIDO: alcance_do_par_gestor_S1_sem_POST dia_por={fonte_dia}",
            nascente, OBRAS["c"],
        ))
    return linhas


def medir_e(janela, vivos, com_evento, grupos, sem_dia):
    # Ordem de producao. app/chamados/management/commands/reconciliar_chamados.py:62,73,79
    for ch in vivos.values():
        if ch.colaborador_id in com_evento:
            continue
        comp = competencia_do_chamado(ch)
        dia = comp.dia if comp is not None else None
        if dia is not None and not janela[0] <= dia <= janela[1]:
            continue
        veredito = _veredito_premissa(ch)
        if veredito is None or veredito.viva:
            continue
        ret = retencao_da_retratacao(ch)
        if ret is not None:
            fam, det = "e_retido_na_retratacao", f"retencao={ret[0]}"
        elif not ch.reconciliaria():
            fam, det = "e_retido_pelo_fechamento", "reconciliaria=False"
        else:
            fam, det = "e", "reconciliaria=True"
        if dia is None:
            sem_dia[fam] += 1
            continue
        grupos[fam].append(Linha(
            ch.colaborador_id, dia, str(ch.pk), f"{ver

[truncado em 20000 de 32189 caracteres]

## 28/11 01:19 · sessao 49d483221237 · CODIGO-DOMINIO-VALIDO · leitura · origem=Code

# Dossiê — CODIGO-DOMINIO-VALIDO

**O ponto de mudança é `classificar_export`, na resposta de quem entra no TXT.** Hoje o juiz só verifica se o código do colaborador está vazio depois de `strip()`. Um valor preenchido como `VIGIA` pode passar por essa etapa e, se houver rubrica a emitir, fazer a validação recusar o TXT inteiro. O Retrato já grava o código na linha e já atribui `Admin` aos bloqueios de cadastro. [app/folha/export.py:849](/home/ronald/codex_ws/93e3058609b2dc9cd2ef551217e26ce8339babf9/app/folha/export.py:849) [app/folha/export.py:881](/home/ronald/codex_ws/93e3058609b2dc9cd2ef551217e26ce8339babf9/app/folha/export.py:881) [app/folha/views.py:282](/home/ronald/codex_ws/93e3058609b2dc9cd2ef551217e26ce8339babf9/app/folha/views.py:282) [app/folha/retrato_folha.py:1239](/home/ronald/codex_ws/93e3058609b2dc9cd2ef551217e26ce8339babf9/app/folha/retrato_folha.py:1239) [app/folha/retrato_folha.py:374](/home/ronald/codex_ws/93e3058609b2dc9cd2ef551217e26ce8339babf9/app/folha/retrato_folha.py:374)

Leitura da cópia e prova em memória do reg10, sem alteração de arquivos. Hash do conteúdo `[nao verificado]`, pois a cópia veio sem metadados Git. O anexo entregue contém a tabela `JUIZES.md`, cujo verbete de fechamento consta na cópia. Ocorrências de `VIGIA` em produção, quantidade de pessoas afetadas, empresa, competência e frequência de uso real `[nao verificado]`. `app/docs/JUIZES.md:119`, `CLAUDE.md:35`.

## A. Onde o juiz decide e onde o layout acusa

- **Precedência atual:** `rescisao_modulo_proprio` → `sem_codigo_dominio` → `ferias_com_batida` → `cadastro_zero` → `furo_espelho` → `nao_homologado` → `entra`. A rescisão vence quando a data de demissão existe e é até o fim da janela. O ramo seguinte usa `if not item['codigo_dominio']`, com o item já preenchido por `(colab.codigo_dominio or '').strip()`. Cada retenção encerra a classificação daquela pessoa. `app/folha/export.py:54–60`, `app/folha/export.py:774–782`, `app/folha/export.py:846–893`, `app/folha/export.py:968–990`.
- **Matrícula do reg10:** `montar_linhas_export` pega os IDs com `status == 'entra'` e lê `(fech.colaborador.codigo_dominio or '').strip().zfill(10)`. O reg10 põe a matrícula em `[2:12]`. `validar_layout_dominio` exige 43 caracteres na linha e aplica `.isdigit()` a esse campo. `VIGIA` vira `00000VIGIA` e dá `reg10 campo matric nao-numerico`. A view devolve `Falha de layout no export` antes de entregar ou registrar o TXT. O command de regeração também recusa erro de layout. `app/folha/export.py:34–45`, `app/folha/export.py:272–279`, `app/folha/export.py:723–753`, `app/folha/views.py:275–304`, `app/folha/management/commands/regerar_txt_dominio.py:94–96`.
- **Predicados existentes:** a checagem numérica da matrícula está no validador de layout e também em `partes_da_linha`, que lê um reg10 já montado. `codigo_no_sistema_dominio` procura a integração ativa **da empresa**, acusa ausência de configuração e preenche com zeros o código da integração. `nao_homologado` consulta códigos **de rubrica**. `app/folha/export.py:23–33`, `app/folha/export.py:245–254`, `app/folha/export.py:277–279`, `app/folha/export.py:983–987`, `app/folha/export.py:1003`.
- **Cadastro hoje:** admissão e edição tratam o campo como texto, aplicam `strip()` e atribuem o valor se preenchido. A validação de obrigatórios só trata nome e CPF, e a de pendências verifica presença. O campo é `CharField(max_length=20, blank=True, default='')`, sem validador numérico, e o `save()` do colaborador não valida esse campo. O `clean()` em `models.py:515` pertence a `HistoricoSalario`. Na admissão o código fica fora dos dados enviados à guarda de tamanho. Na edição o POST usa `codigo_folha` e a guarda compara nomes exatos do modelo. `app/colaboradores/views.py:1131–1138`, `app/colaboradores/views.py:1169–1185`, `app/colaboradores/views.py:1231–1237`, `app/colaboradores/views.py:1297–1306`, `app/colaboradores/views.py:1353–1365`, `app/colaboradores/services/cadastro.py:22–30`, `app/colaboradores/services/cadastro.py:49–87`, `app/core/validacao.py:46–59`, `app/colaboradores/models.py:311–314`, `app/colaboradores/models.py:415–439`, `app/colaboradores/models.py:478`, `app/colaboradores/models.py:515`.

## B. Censo do campo `Colaborador.codigo_dominio`

| Sítio | Leitura ou escrita encontrada |
|---|---|
| **Admissão** | A rota `novo/` recebe `codigo_folha` no input de texto. A view cria o colaborador e depois atribui o código preenchido por `salvar_campos(..., 'cadastro', ...)`. `app/colaboradores/urls.py:49`, `app/templates/colaboradores/admitir.html:58`, `app/colaboradores/views.py:1131`, `app/colaboradores/views.py:1204–1237`. |
| **Edição** | A rota `editar/` exibe o código atual no input `codigo_folha`. A view atribui o valor preenchido e chama `salvar_inteiro(..., 'cadastro')`. POST vazio mantém o código anterior. Os aliases usados para reexibir a admissão e a edição preenchem rascunhos em memória. `app/colaboradores/urls.py:50`, `app/templates/colaboradores/editar.html:62`, `app/colaboradores/views.py:1113–1118`, `app/colaboradores/views.py:1267–1285`, `app/colaboradores/views.py:1353–1365`. |
| **Persistência genérica** | `cadastro` inclui o campo, e `gravar`, `salvar_campos`, `atualizar_em_lote` e `criar_colaborador` podem transportá-lo via argumentos. `salvar_inteiro` e `salvar_reativacao` fazem `save()` completo e podem regravar o valor que já estava carregado. A migration declara o campo e o modelo fornece o vazio inicial. `app/colaboradores/services/colaborador.py:46`, `app/colaboradores/services/colaborador.py:126–173`, `app/colaboradores/services/colaborador.py:184–206`, `app/colaboradores/migrations/0026_colaborador_codigo_dominio.py:15`, `app/colaboradores/models.py:311–314`. |
| **Admin, importadores, commands, API e signals** | Admin Django lê o campo por `SoLeitura`. O importador Nexti encontrado grava telefone e e-mail. O signal de `Colaborador` localizado trata isenção de ponto. A API de cadastro incompleto consulta o campo e o upload de holerite usa o código para casar páginas. Outros escritores explícitos do campo nessas categorias não apareceram na busca na cópia. Escritas por fora da cópia `[nao verificado]`. `app/colaboradores/admin.py:47–48`, `app/core/admin.py:53–75`, `app/colaboradores/management/commands/importar_telefones_nexti.py:113`, `app/chamados/signals.py:270–281`, `app/api/views_mensageria.py:1275–1288`, `app/holerite/motor.py:216–230`. |
| **Export, prévias e auditoria do TXT** | O juiz lê o código para classificar, e o montador lê para preencher a matrícula. A prévia nominal, a proposta em tela/CSV e o PDF de retidos leem o campo do item. O PDF de validação e o vigia indexam pela matrícula preenchida com zeros. O PDF do TXT gravado e `haiku_export` também consultam o cadastro para cruzar matrículas. `app/folha/export.py:737`, `app/folha/export.py:849–884`, `app/folha/views.py:427–429`, `app/folha/previa.py:97–105`, `app/folha/views.py:762–768`, `app/folha/views.py:813–837`, `app/folha/retidos_pdf.py:88–95`, `app/folha/validacao_pdf.py:65–72`, `app/folha/services/vigia_txt.py:98–116`, `app/folha/pdf_export.py:147–148`, `app/folha/haiku_export.py:111–116`. |
| **Retrato, fechamento e relatórios** | O Retrato grava `codigo` do item. Fechamento, Pendências de Cadastro e Quem Cobrar leem o veredito do juiz. O contador de cadastro lê o valor pelo alias `codigo_folha`, e a API de cadastro incompleto tem filtro próprio de `NULL`/`''`, sem `strip()`. `app/folha/retrato_folha.py:1212–1243`, `app/ponto/services/fechamento_lista.py:25–37`, `app/ponto/services/fechamento_lista.py:65–73`, `app/relatorios/views_pend.py:47–69`, `app/relatorios/views.py:1723–1737`, `app/colaboradores/services/cadastro.py:74–87`, `app/colaboradores/services/cadastro.py:99–120`, `app/api/views_mensageria.py:1275–1288`. |
| **Holerite** | O parser Domínio extrai um identificador numérico do PDF. A configuração padrão Domínio casa por nome completo, com CPF quando fornecido pela página. O modo configurável oferece `codigo_dominio` e pode filtrar o colaborador por esse campo e pela empresa. `app/holerite/motor.py:96–118`, `app/holerite/motor.py:161–163`, `app/holerite/motor.py:192–230`, `app/holerite/motor.py:298–313`, `app/holerite/views.py:1741`, `app/templates/holerite/upload_lote_v2.html:46`. |

O corte anterior `EXPORT-UM-JUIZ-DO-CODIGO` registra um censo de **8 escritores, 20 leitores e 4 predicados vivos**, sem enumerá-los naquela linha. Equivalência entre esse levantamento e o censo da cópia `[nao verificado]`. `app/docs/CORTES.md:587`.

## C. Quem lê o vocabulário do motivo

| Sítio | Efeito de emitir `codigo_dominio_invalido` |
|---|---|
| **`MOTIVO_RH` e `rotulo_do_motivo`** | A entrada no mapa dá o rótulo humano à prévia do export, ao PDF de retidos e, via `rotulo_do_motivo`, à lista, ao PDF, ao Raio-X e ao drawer do Retrato. Chave desconhecida aparece crua. `app/folha/views.py:12–24`, `app/folha/views.py:373`, `app/folha/views.py:415–429`, `app/folha/views.py:884–885`, `app/folha/retidos_pdf.py:83–95`, `app/folha/retrato_folha.py:76–86`, `app/folha/retrato_folha.py:1119`, `app/folha/views_retrato.py:291`, `app/folha/views_retrato.py:424`, `app/folha/views_retrato.py:503`. |
| **Tipo e faixa** | A nova chave precisa de destino `cadastro` em `TIPO_POR_MOTIVO`, junto de `sem_codigo_dominio`. `_tipo_cru` usa esse mapa para retenções de cadastro e `TIPO_POR_CAUSA` para `furo_espelho`. `MOTIVOS_DE_CADASTRO` deriva do primeiro mapa e `_saldo_cru` já devolve `cadastro` mesmo com `dias=None`. `app/folha/retrato_folha.py:210–230`, `app/folha/retrato_folha.py:379–390`, `app/folha/retrato_folha.py:422–433`. |
| **Dono e fio** | `_fio_cru` já devolve `admin` para bloqueio diferente de `furo_espelho`, excetuado o módulo próprio. `montar_linhas` grava esse fio em `dono`, `DONO_DO_FIO` o leva a `admin` e o rótulo é `Admin`. `models.py:321` documenta a regra e dá `sem_codigo_dominio` como exemplo. `app/folha/retrato_folha.py:33`, `app/folha/retrato_folha.py:194–201`, `app/folha/retrato_folha.py:372–376`, `app/folha/retrato_folha.py:486–493`, `app/folha/retrato_folha.py:1248–1251`, `app/folha/models.py:315–323`. |
| **Listas e rótulos locais** | `_MOTIVO_NA_TELA` do fechamento cai na pílula `espelho` para chaves novas. `_PORTAS_CADASTRO` filtra apenas as chaves declaradas, então Pendências de Cadastro omite o novo motivo. Quem Cobrar e `EM_PORTUGUES` da mensageria caem na chave crua. Esses sítios precisam conhecer a chave para mostrá-la com o mesmo sentido. A API que conta apenas código faltante tem outro universo e outro predicado. `app/ponto/services/fechamento_lista.py:21–37`, `app/templates/ponto/_fech_linha.html:11`, `app/relatorios/views_pend.py:47–69`, `app/relatorios/views.py:1727–1737`, `app/relatorios/views.py:1762`, `mensageria/nucleo/ferramentas.py:1196–1203`, `mensageria/nucleo/ferramentas.py:1246–1253`, `app/api/views_mensageria.py:1275–1288`. |
| **Agrupadores e vigias** | O Raio-X do export põe chaves fora de `_COM_PILULA` em **Outras pendências**. `fora_da_folha`, a API de prontidão, as métricas e o simulador agrupam pela chave recebida. O vigia do TXT transporta o motivo. A enumeração em `vigia_pausa.py:29` é documentação da precedência. O contador daquele vigia usa `motivos_retencao_celula`. `app/folha/views.py:615–627`, `app/folha/views.py:661–667`, `app/folha/fora_da_folha.py:147–173`, `app/api/views_mensageria.py:283–302`, `app/inteligencia/calculadores.py:210–220`, `app/folha/management/commands/simular_folha.py:133–146`, `app/folha/services/vigia_txt.py:114–116`, `app/folha/services/vigia_pausa.py:26–32`, `app/folha/services/vigia_pausa.py:123–132`. |

## D. Como mostrar o valor gravado

`montar_linhas` já grava `codigo` ao lado de `motivo`. O valor vem do item do juiz depois de `strip()`, antes de `zfill(10)`. `detalhe` também existe no item, nasce vazio e é preenchido pelos ramos, inclusive pelo de espelho a partir de `:977`. A linha JSON do Retrato usa as chaves estruturadas e deixa `detalhe` no item do export. `app/folha/export.py:846–858`, `app/folha/export.py:881–884`, `app/folha/export.py:977–987`, `app/folha/retrato_folha.py:1237–1256`, `app/folha/models.py:350–354`.

**Já há forma para tela e PDF.** A projeção compartilhada do *Por quê* pode entregar o rótulo literal de `MOTIVO_RH` e, para a nova chave, o rótulo fixo `codigo Dominio =` com o dado `linha['codigo']`. A lista, o PDF e o Raio-X recebem os mesmos componentes. No drawer, `r.valor` continua literal do mapa, com o código exibido como dado ao lado. O próprio drawer já exibe datas gravadas como dados e o PDF de retidos já tem coluna estruturada de código. Isso resolve `Código Domínio inválido · codigo Dominio = VIGIA` sem prosa livre nem consulta ao cadastro atual. Interpolar o código dentro de `r.valor` quebraria o selo literal assim que a nova linha entrasse na fixture. `app/folha/views_retrato.py:278–293`, `app/folha/views_retrato.py:414–425`, `app/folha/views_retrato.py:500–506`, `app/folha/retrato_folha.py:1079–1093`, `app/templates/folha/_retrato_o_que_fazer.html:33–50`, `app/folha/retidos_pdf.py:88–95`, `app/folha/tests/test_retrato_folha_drawer.py:213–231`, `app/docs/CORTES.md:405`.

## E. Leis e juiz registrado

- **L-001, L-002, L-011 e L-095.** A cura da classificação fica na fonte, as testemunhas leem a resposta, autoridade nova exigiria corte e o dinheiro lavrado segue sendo lido. O registro vivo põe `_K2` **quem entra no TXT** e `_K1` **o que bloqueia aprovação/tranca** na mesma função. O motivo pertence a `_K2`. `_K1` sai em `travas_aprovacao`, montada antes dos retornos antecipados. `app/docs/LEIS.md:59–60`, `app/docs/LEIS.md:69`, `app/docs/LEIS.md:113`, `app/core/juizes.py:180–181`, `app/core/juizes.py:326–337`, `app/folha/export.py:779–782`, `app/folha/export.py:839–858`, `app/docs/DOSSIES.md:85–89`, `app/docs/DOSSIES.md:118–122`.
- **L-099, O256 e Saldo Cadastro.** L-099 separa a inconsistência estrutural do valor de cadastro, cuja correção fica com o Admin. A O256 manda o drawer ler os dados lavrados. O complemento põe cadastro sem forma de dia na faixa **Cadastro** e sela a igualdade entre a soma do Saldo e Bloqueados. `app/docs/LEIS.md:122`, `app/docs/CORTES.md:392`, `app/docs/CORTES.md:405`, `app/folha/tests/test_retrato_folha_quadros.py:1133–1160`.
- **L-026/L-090, L-092/L-113 e L-121.** A janela vem do corte da empresa. Para exportadas, a L-113 e sua emenda qualificam a regra histórica da L-092, preservam o TXT anterior e tratam DIFF e aviso quando aplicáveis. L-121 liga cura de juiz à reconstrução das projeções pela porta. `app/docs/LEIS.md:77`, `app/docs/LEIS.md:123–125`, `app/docs/LEIS.md:142`, `app/docs/LEIS.md:155–156`, `app/folha/export.py:47–51`.
- **Diferença entre corte e cópia.** `RESPOSTAS-COERENCIA-1.6` manda levar `STATUS_NO_TXT` para dentro do juiz. Nesta cópia o juiz consulta todos os fechamentos da competência e o montador aplica `STATUS_NO_TXT = ('aberto', 'aprovado')`. O dossiê usa esse comportamento observado como linha de base. `app/docs/CORTES.md:523`, `app/folha/export.py:696`, `app/folha/export.py:723–731`, `app/folha/export.py:791–797`.

## F. Selos atuais e REDs descritos

**Selos atuais.** `MotivoExaustivoTest` colhe por AST as seis chaves literais atuais e fica vermelho se a nova chave vier sem destino no eixo Tipo. Os testes de dono e faixa têm listas literais de exemplos, e a asserção que deriva Cadastro do mapa já cobre entradas novas. O selo Saldo = Bloqueados só observa a nova linha se ela entrar na fixture. O selo do drawer cobra o valor literal em mapa e cinco linhas do bloco. `app/folha/tests/test_retrato_folha_quadros.py:255–280`, `app/folha/tests/test_retrato_folha_quadros.py:131–136`, `app/folha/tests/test_retrato_folha_quadros.py:314–335`, `app/folha/tests/test_retrato_folha_quadros.py:375–398`, `app/folha/tests/test_retrato_folha_quadros.py:1133–1160`, `app/folha/tests/test_retrato_folha_drawer.py:208–231`.

**Precedência já selada em partes.** Há casos de rescisão, código vazio, vazio simultâneo a turno aberto com preservação de `travas_aprovacao`, férias com batida, cadastro zero antes de furo e rubrica não homologada. Há também selo de motivo desconhecido que continua visível e selo de PDF que lê o mesmo recorte sem chamar o juiz. Esses testes protegem regressões, mas nenhum desses casos exige o novo motivo. `app/folha/tests/test_rescisao_modulo_proprio.py:24–38`, `app/folha/tests/test_classificar_export.py:34–66`, `app/folha/tests/test_classificar_export.py:113–130`, `app/folha/tests/test_contrato_export_um_juiz.py:132–150`, `app/folha/tests/test_ferias_com_batida.py:74–94`, `app/folha/tests/test_cadastro_zero_com_batida.py:161–168`, `app/folha/tests/test_integracao.py:143–152`, `app/folha/tests/test_retrato_pdf.py:138–150`, `app/folha/tests/test_retrato_pdf.py:330–345`.

| Entrada de teste | Saída esperada | [nao verificado]
|---|---|
| **`VIGIA`, fechamento apto nos demais critérios e rubrica que geraria reg10** | Um item `fora/codigo_dominio_invalido`, com `codigo_dominio='VIGIA'`. O ID sai do conjunto de aptos e nenhuma linha leva `00000VIGIA`. A rubrica positiva faz o caso detectar a falha atual de layout. `app/folha/export.py:723–753`, `app/folha/export.py:849–884`, `app/folha/export.py:277–279`. |
| **Controle numérico `00123`, mesma janela e demais insumos** | Continua `entra`, matrícula `0000000123`, reg10 idêntico ao controle anterior. No lote misto com esse controle o TXT tem linha, `validar_layout_dominio(linhas) == []` e a rota entrega o arquivo. `app/folha/export.py:34–45`, `app/folha/export.py:725–755`, `app/folha/views.py:275–292`, `app/folha/tests/test_integracao.py:211`. |
| **`''` e `'   '`** | Ambos continuam `fora/sem_codigo_dominio`, sem linha no TXT. `app/folha/export.py:849`, `app/folha/export.py:881–884`, `app/folha/tests/test_classificar_export.py:40–45`. |
| **Retrato lavrado com `VIGIA`, depois cadastro alterado para `00123`** | Na lista, no PDF do mesmo recorte e no drawer, verificar o rótulo do motivo, o valor `VIGIA` da foto e `Depende de = Admin`. A linha está em **Cadastro**, com `dias`/`datas` vazios. No drawer, verificar também que o valor de mapa segue literal. `app/folha/retrato_folha.py:1212–1251`, `app/folha/views_retrato.py:194–201`, `app/folha/views_retrato.py:278–293`, `app/folha/views_retrato.py:414–425`, `app/folha/tests/test_retrato_folha_drawer.py:213–231`. |
| **POST real de admissão e edição com os outros campos válidos** | `VIGIA` retorna mensagem visível, por exemplo `Código Domínio deve conter apenas números.` Na admissão, não nasce colaborador nem usuário. Na edição, o registro anterior fica intacto e o valor digitado reaparece. `00123` grava exatamente `00123` nas duas portas. Casos de regressão para manter a admissão sem código e a edição com campo vazio conforme o comportamento atual. `app/colaboradores/views.py:1131`, `app/colaboradores/views.py:1165–1168`, `app/colaboradores/views.py:1190–1237`, `app/colaboradores/views.py:1267–1285`, `app/colaboradores/views.py:1353–1365`, `app/core/respostas.py:58–65`, `app/core/respostas.py:74–85`. |

**Bordas verificadas em memória com o código da cópia.** `' 00123 '` e `'00123'` saem como `0000000123`. `'00000'` sai como `0000000000`. Espaço interno e sinal são rejeitados pelo `.isdigit()` do layout. Onze dígitos produzem reg10 de 44 caracteres e falham no comprimento. O `.isdigit()` local aceita dígitos Unicode, inclusive `１２３` e `²`. Aceitação desses caracteres pelo importador externo `[nao verificado]`. Os REDs de integração e a suíte ficaram `[nao verificado]`. `app/folha/export.py:34–45`, `app/folha/export.py:272–279`, `app/folha/export.py:737`, `app/folha/export.py:849–884`.

## G. Contrato de entrada e perguntas de lei

- **Fonte e unidade.** Para o TXT, `FechamentoMensal` e `Colaborador.codigo_dominio`, uma decisão por colaborador × empresa × competência. O caminho normal consulta o fechamento gravado e o caminho de export pode injetar os fechamentos que já tem em mãos. Para o cadastro, uma submissão de `codigo_folha` na admissão ou edição. 

[truncado em 20000 de 22700 caracteres]

## 28/11 01:10 · sessao d4649f8dfc9b · RETRATO-PDF-CORTA-POR-QUE · leitura · origem=Code

## RETRATO-PDF-CORTA-POR-QUE

A linha lavrada tem **um `tipo` por pessoa e uma lista plana de `datas`**. O vínculo entre motivo e data existe na fonte anterior à lavra, mas fica de fora do JSON. O anexo `JUIZES.md` localiza a autoridade de entrada no TXT. Linhas reais, contadores de produção, PDF renderizado em produção e ambiente da casca estão `[nao verificado]`. `app/folha/export.py:1202` · `app/folha/fora_da_folha.py:234` · `app/folha/retrato_folha.py:1237` · `app/docs/JUIZES.md:119`

### A. Mapa das colunas e do vocabulário

Tela e PDF passam por `_recorte_da_barra`. Ele lê `RetratoFolha.linhas` via `linhas_do_retrato`, junta o cadastro, aplica o filtro universal, passa por `no_periodo` e entrega `ordenar(recortar(...))`. A tela projeta `_tabela` e usa `_retrato_lista.html`, tanto na página cheia quanto no parcial. O PDF monta a tabela em `_story`, dentro de `retrato_pdf`. `app/folha/retrato_folha.py:98` · `app/folha/views_retrato.py:194` · `app/folha/views_retrato.py:225` · `app/folha/views_retrato.py:278` · `app/folha/views_retrato.py:345` · `app/templates/folha/retrato_folha.html:53` · `app/templates/folha/_retrato_parcial.html:7` · `app/folha/views_retrato.py:406`

| Coluna | Origem e saída |
|---|---|
| LED na tela / **Depende de** no PDF | `desenho_do_led` lê `led_da_linha`, que usa `dono_da_linha` para bloqueados e o próprio balde para `Entram` e `Rescisão`. A tela desenha o ponto com cor, rótulo e link de filtro. O PDF imprime o rótulo na cor do LED. `app/folha/retrato_folha.py:308` · `app/folha/retrato_folha.py:531` · `app/folha/retrato_folha.py:559` · `app/templates/folha/_retrato_lista.html:47` · `app/folha/views_retrato.py:418` |
| **Colaborador** | `_cadastro` junta `Colaborador.nome` pelo ID `colab`. As duas saídas usam `(cadastro ausente)` se o nome estiver vazio. Na tela, o nome abre o drawer com mês e ano. `app/folha/views_retrato.py:99` · `app/folha/views_retrato.py:118` · `app/folha/views_retrato.py:287` · `app/folha/views_retrato.py:421` · `app/templates/folha/_retrato_lista.html:60` |
| **Posto** | `_cadastro` prefere o posto do vínculo ativo mais recente e, na falta dele, usa o posto do colaborador. As duas saídas exibem `posto.nome`. `app/folha/views_retrato.py:107` · `app/folha/views_retrato.py:115` · `app/templates/folha/_retrato_lista.html:63` · `app/folha/views_retrato.py:422` |
| **Escala** | `_cadastro` pega `tipo_escala` daquele vínculo ativo. As duas saídas exibem `descricao`. `app/folha/views_retrato.py:107` · `app/folha/views_retrato.py:122` · `app/templates/folha/_retrato_lista.html:64` · `app/folha/views_retrato.py:423` |
| **Por quê** | Ambas aplicam `rotulo_do_motivo(linha['motivo'])`. O HTML imprime `l.motivo`. O rótulo visível **“Espelho com pendencia”** vem de `MOTIVO_RH['furo_espelho']`. O export também monta um `detalhe` com esse prefixo e os códigos, mas essa chave fica fora da linha lavrada. `app/folha/views_retrato.py:291` · `app/folha/views_retrato.py:424` · `app/templates/folha/_retrato_lista.html:65` · `app/folha/retrato_folha.py:76` · `app/folha/views.py:18` · `app/folha/export.py:977` · `app/folha/retrato_folha.py:1237` |
| **Dias que seguram** | `dias` vem da lavra. Com janela, `no_periodo` conta as datas no intervalo inclusivo e recalcula a faixa `saldo`. `None` sai em branco nas duas saídas. `app/folha/retrato_folha.py:1227` · `app/folha/retrato_folha.py:1334` · `app/templates/folha/_retrato_lista.html:66` · `app/folha/views_retrato.py:425` |

- **Pílulas e Tipo.** `ROTULO_DO_DONO` alimenta o quadro `Depende de` e os LEDs. `ROTULO_DO_TIPO` contém `Escala`, `Folga`, `Sem escala`, `Não bate`, `Falta`, `Cadastro`, `Férias` e `Sem causa`. O eixo `tipo` continua no recorte, no Raio-X e no drawer. A barra atual desenha `dono` e, com Bloqueados ativo, `saldo`. O **Por quê** consulta o mapa `MOTIVO_RH`. As expressões do exemplo “Batida faltando” e “pergunta aberta” não são entradas desses mapas. `app/folha/retrato_folha.py:200` · `app/folha/retrato_folha.py:308` · `app/folha/retrato_folha.py:587` · `app/folha/retrato_folha.py:906` · `app/folha/retrato_folha.py:1118` · `app/folha/views_retrato.py:500` · `app/folha/views.py:15`
- **Forma exata da linha construída.** As chaves são `colab`, `codigo`, `status`, `motivo`, `dias`, `datas`, `dias_sem_leitura`, `dias_sem_porta`, `dono`, `acoes`, `tipo`, `saldo` e `informativo`. `datas` é lista plana ISO `AAAA-MM-DD` ou `None` quando a fonte não dá datas, com possibilidade de a chave faltar em retratos legados. As duas listas `dias_sem_*` registram sem leitura e sem porta. `acoes` guarda as portas publicadas, algumas com `dia_do_turno`. `informativo` é uma lista ISO separada para rescisão. Nenhuma dessas chaves guarda a associação geral `data → motivo/tipo de retenção`. `app/folha/retrato_folha.py:1227` · `app/folha/retrato_folha.py:1237` · `app/folha/retrato_folha.py:1288` · `app/folha/fora_da_folha.py:215` · `app/folha/models.py:334`
- **Onde havia o par.** `motivos_retencao_celula(com_dias=True)` devolve motivos agregados e registros `{data, motivo, veredito, celula_id}`. `fora_da_folha` tem esses registros em `_dias`, mas publica no resumo por pessoa `motivos` agregados e `datas` planas. O lavrador leva as datas ao retrato e usa `dias_ferias_com_batida` como segunda fonte. A função avulsa `o_que_segura` expõe `data`, `motivo`, `veredito` e `fio` por dia e consulta o export em sua própria execução. `app/folha/export.py:1195` · `app/folha/export.py:1268` · `app/folha/fora_da_folha.py:214` · `app/folha/fora_da_folha.py:234` · `app/folha/fora_da_folha.py:257` · `app/folha/fora_da_folha.py:273` · `app/folha/fora_da_folha.py:292` · `app/folha/retrato_folha.py:1231`

### B. Censo de escritores e leitores

| Campo | Escrita e leitura na cópia |
|---|---|
| `motivo` | `classificar_export` produz a chave no item. `montar_linhas` copia o valor para a linha. `_balde` usa o motivo para reconhecer rescisão. `fio_da_linha`, `tipo_da_linha`, `saldo_da_linha` e `no_periodo` também o consultam. Tela, PDF, Raio-X e drawer passam o valor a `rotulo_do_motivo`. `app/folha/fora_da_folha.py:147` · `app/folha/export.py:977` · `app/folha/retrato_folha.py:1225` · `app/folha/retrato_folha.py:89` · `app/folha/retrato_folha.py:444` · `app/folha/retrato_folha.py:496` · `app/folha/retrato_folha.py:1338` · `app/folha/views_retrato.py:291` · `app/folha/views_retrato.py:424` · `app/folha/views_retrato.py:503` · `app/folha/retrato_folha.py:1116` |
| `dono` | `_fio_cru` grava o **fio** (`admin`, `colab`, `chamado`, `ninguem`, `nao_sei` ou vazio). `fio_da_linha` lê a chave, com tratamento de legado. `dono_da_linha` traduz o fio para o balde da UI e trata `Hoje` usando `datas`. Esses leitores alimentam o recorte, os contadores, o LED, o drawer e o vigia. `app/folha/retrato_folha.py:178` · `app/folha/retrato_folha.py:351` · `app/folha/retrato_folha.py:436` · `app/folha/retrato_folha.py:486` · `app/folha/retrato_folha.py:588` · `app/folha/retrato_folha.py:1112` · `app/folha/retrato_folha.py:1248` · `app/folha/vigia_retido_calado.py:77` |
| `tipo` | `_tipo_cru` grava um tipo por pessoa a partir do motivo ou de `FechamentoMensal.causa_espelho['causa']`. `_causas_gravadas` lê essa causa. `tipo_da_linha` é o leitor do recorte, do agrupamento do Raio-X e dos rótulos e portas do drawer. `app/folha/retrato_folha.py:379` · `app/folha/retrato_folha.py:496` · `app/folha/retrato_folha.py:588` · `app/folha/retrato_folha.py:1066` · `app/folha/retrato_folha.py:1124` · `app/folha/retrato_folha.py:1167` · `app/folha/retrato_folha.py:1250` · `app/folha/views_retrato.py:506` |
| `datas` | `montar_linhas` guarda a lista. `no_periodo` lê a lista e devolve cópias com `dias` e `saldo` ajustados, mantendo `datas` integral. `dono_da_linha` lê a lista para `Hoje`. O drawer localiza a linha via `linha_do_colab` e imprime as datas em ISO. `app/folha/retrato_folha.py:1231` · `app/folha/retrato_folha.py:489` · `app/folha/retrato_folha.py:1334` · `app/colaboradores/views.py:1001` · `app/templates/folha/_retrato_o_que_fazer.html:42` |
| `motivos_espelho` | Chave do **item do export**, fora do JSON do retrato. `classificar_export` a preenche com a lista de códigos agregados de `motivos_retencao_espelho`, que delega ao retentor. Leitores diretos do item incluem a prévia da folha, o PDF legado de retidos e `fechamento_lista._folha_do_juiz`. Há um campo homônimo em `FechamentoMensal`, preenchido por `recalcular_fechamento_mes` a partir do mesmo juiz. Tela e PDF do retrato leem `motivo` para a coluna. `app/folha/export.py:977` · `app/folha/export.py:1310` · `app/folha/views.py:267` · `app/folha/views.py:424` · `app/folha/retidos_pdf.py:90` · `app/ponto/services/fechamento_lista.py:28` · `app/ponto/models.py:451` · `app/ponto/services/fechamento.py:1091` · `app/ponto/services/fechamento.py:1210` · `app/folha/retrato_folha.py:1237` · `app/folha/views_retrato.py:291` · `app/folha/views_retrato.py:424` |

O escritor persistente do retrato é `lavrar_retrato_folha --apply`, com `update_or_create` por empresa, mês e ano. `montar_linhas` constrói o JSON. O cron chama a mesma porta. O selo AST exige um único chamador de produção de `montar_linhas`. `app/folha/management/commands/lavrar_retrato_folha.py:65` · `app/folha/management/commands/lavrar_retrato_folha.py:72` · `app/config/crons.py:1659` · `app/folha/tests/test_retrato_folha_um_derivador.py:68`

**Contadores de dono.** A tela chama `quadros_barra_retrato`, e `_pilula` calcula `numero = len(recortar(...))` sobre o universo após filtro universal e janela. A pílula ativa conta o recorte atual. Cada pílula inativa conta o recorte que seu clique abriria, substituindo o dono e mantendo os outros eixos. O PDF chama a mesma função, com o mesmo universo e grupos, mas consome apenas título e rótulo dos filtros ativos. Hoje imprime `n de total`, sem os `numero` por dono. `contadores(retrato)` é outra função e conta os baldes gerais `entram`, `bloqueado`, `modulo_proprio` e `total`. `app/folha/views_retrato.py:324` · `app/folha/retrato_folha.py:808` · `app/folha/retrato_folha.py:838` · `app/templates/core/_barra_gestao.html:67` · `app/folha/views_retrato.py:399` · `app/folha/views_retrato.py:409` · `app/folha/retrato_folha.py:114`

### C. Como abre e precedente de `attachment`

O botão da barra é `<a href="{{ pdf_href }}">` do partial `core/_btn_pdf.html`, sem `target="_blank"`, sem `download` e sem `onclick` nesse ramo. A view monta o `href` da rota `/folha/exportar/retrato.pdf` com `qs_do_recorte`; o mesmo botão volta no parcial. A resposta atual é `application/pdf` com `Content-Disposition: inline; filename="retrato-folha-emp…-MM-AAAA.pdf"`. Resultado do clique no navegador de produção `[nao verificado]`. `app/templates/folha/_retrato_barra.html:50` · `app/templates/core/_btn_pdf.html:34` · `app/templates/folha/_retrato_parcial.html:8` · `app/folha/views_retrato.py:337` · `app/config/urls.py:34` · `app/folha/urls.py:25` · `app/folha/views_retrato.py:450`

Na mesma casa, o PDF do histórico do TXT, `retidos_pdf` e `validar_export_pdf` usam `application/pdf` com `Content-Disposition: attachment; filename="…"`. O `retidos_pdf` legado chama `classificar_export` ao vivo. O precedente para este item é o cabeçalho HTTP. `app/folha/views.py:860` · `app/folha/views.py:884` · `app/folha/views.py:911`

### D. Rodapé e fontes de commit [nao verificado]

`gerar_pdf_auditavel` lê `get_commit_git()` uma vez, passa o resultado aos rodapés das duas passagens e grava `RegistroPDFAuditavel.commit_git`. `RodapeAuditavel.__call__` imprime os oito primeiros caracteres ou `n/d` quando recebe vazio. O helper prioriza `COMMIT_GIT` e, na falta dela, tenta `git -C /app rev-parse HEAD` se `/app/.git` existir. Se nenhum caminho entregar valor, retorna `''`. `app/relatorios/pdf_base.py:72` · `app/relatorios/pdf_base.py:133` · `app/relatorios/pdf_base.py:152` · `app/relatorios/pdf_base.py:170` · `app/relatorios/utils_audit.py:7`

A montagem documentada leva `app/` a `/app`, enquanto o repositório fica no diretório pai no host. O Dockerfile usa `python:3.13-slim`, copia o contexto e não declara `COMMIT_GIT` nem instala `git` explicitamente. Isso explica o caminho de `n/d` quando o ambiente efetivo também não fornece a variável. O compose de produção é excluído da cópia versionada. A variável, o executável, `/app/.git` e o SHA efetivo da casca de produção estão `[nao verificado]`. `CLAUDE.md:88` · `app/.dockerignore:8` · `app/Dockerfile:1` · `app/Dockerfile:30` · `app/Dockerfile:40` · `.gitignore:40` · `app/relatorios/utils_audit.py:12`

| Fonte existente | O que identifica |
|---|---|
| `AMBIENTE sha=` | `HASNER_SHA` injetado pela suite para a árvore de teste medida. O teste imprime a variável e `pista_certifica.sh` compara o valor com o SHA do pouso. A disponibilidade na casca web de produção está `[nao verificado]`. `bin/suite.sh:109` · `bin/suite.sh:566` · `app/core/tests/test_ambiente_do_run.py:52` · `bin/pista_certifica.sh:48` |
| Carimbo de deploy | `bin/deploy.sh` escreve o `COMMIT` lido do Git do **host** em `logs/deploy.stamp`. O vigia já lê esse carimbo. A correspondência com o código carregado pelo worker que gera um PDF está `[nao verificado]`. `bin/deploy.sh:430` · `app/core/esteira_vigia.py:1032` · `app/core/esteira_vigia.py:1202` |
| `/health/` e versão do app | O health do core devolve `ok`, `schema` e `host`. O de mensageria devolve `ok` e `servico`. A versão da API é a versão do aplicativo Android/iOS. Esses retornos não incluem SHA do servidor. `app/config/urls_core.py:5` · `app/core/views.py:1306` · `mensageria/config/urls.py:7` · `app/api/views.py:3068` |

### E. Escala no PDF

A célula recebe `getattr(_l.get('_escala'), 'descricao', '') or ''` como **string crua**. As larguras são `26 / 68 / 48 / 38 / 60 / 24 mm`, com **38 mm para Escala** e **60 mm para Por quê**. A tabela repete o cabeçalho, usa fonte 7 e alinhamento vertical no topo. Os `Paragraph` dessa função estão no título e no resumo, fora da tabela. O PDF legado de retidos mostra o uso de `Paragraph(escape(...))` dentro de célula. O vazamento em um PDF real está `[nao verificado]`. `app/folha/views_retrato.py:406` · `app/folha/views_retrato.py:423` · `app/folha/views_retrato.py:431` · `app/folha/retidos_pdf.py:91`

### F. Ordem atual

- **Linhas da tela e do PDF.** Ambos iteram o mesmo `_vis`, ordenado por **Bloqueados → Entram → Rescisão**, depois pelas faixas **1 → 2 a 5 → 6 ou mais → Cadastro → sem faixa**, depois pelo ID do colaborador. Dois bloqueados na mesma faixa são ordenados pelo ID, mesmo com quantidades de dias diferentes. `app/folha/views_retrato.py:232` · `app/folha/views_retrato.py:293` · `app/templates/folha/_retrato_lista.html:35` · `app/folha/views_retrato.py:417` · `app/folha/retrato_folha.py:754`
- **Donos na barra.** **Admin → Colab → Hoje → Sem ação disponível → Sem leitura (sistema)**. A ordem vem de `DONOS` via `VALORES_DO_GRUPO` e `quadros_barra_retrato`. `Entram` e `Rescisão` são baldes sem dono, com rótulos próprios no PDF. `app/folha/retrato_folha.py:191` · `app/folha/retrato_folha.py:200` · `app/folha/retrato_folha.py:308` · `app/folha/retrato_folha.py:590` · `app/folha/retrato_folha.py:908`

### G. Leis e cortes

- **L-002, L-007 e L-011.** “TESTEMUNHA LE, NAO RECALCULA” exige uma autoridade comum para tela, PDF, TXT, badge e contador. L-007 exige um escritor canônico por estado. L-011 exige a frase de corte para registrar um juiz novo. `classificar_export` está registrado em K1 e K2. O corte `O256-LEGENDA-E-PDF-ANTES-DO-RAIO-X` manda o PDF respeitar os filtros da barra e fixa “Juizes novos = 0”. `app/docs/LEIS.md:60` · `app/docs/LEIS.md:65` · `app/docs/LEIS.md:69` · `app/core/juizes.py:336` · `app/docs/CORTES.md:407`
- **L-026/L-090 e L-038.** As duas primeiras protegem a janela da empresa em `ponto/janelas.py`, usada na entrada do recorte. L-038 diz “o PDF nao tem regra de dia; le o MESMO juiz do espelho” e nomeia `relatorios/pdf_espelho.py::_coletar_dados_espelho`. A paridade específica do PDF do retrato está em L-002 e no corte O256 acima. `app/docs/LEIS.md:77` · `app/docs/LEIS.md:86` · `app/docs/LEIS.md:123` · `app/folha/views_retrato.py:177` · `app/docs/CORTES.md:407`
- **Leis de UI L1, L2, L6, L7 e L9.** Contador e lista vêm da mesma fonte, tela e ação consultam a mesma função, o filtro é cumulativo e único, a ajuda sai do mesmo mapa, e o botão PDF usa um partial único com rótulo `PDF` e ajuda no `title`. `RETRATO-DONO-SAO-TRES` registrou a soma dos donos com os bloqueados, e LU-05 deu balde próprio ao dia sem porta. A cópia também declara o balde `Sem leitura (sistema)`. `app/docs/LEIS-UI.md:12` · `app/docs/LEIS-UI.md:23` · `app/docs/LEIS-UI.md:68` · `app/docs/LEIS-UI.md:77` · `app/docs/LEIS-UI.md:95` · `app/docs/CORTES.md:408` · `app/docs/CORTES.md:539` · `app/folha/retrato_folha.py:191`
- **“NENHUM TEXTO SE GERA”.** O literal de `O256-ORDEM-DRAWER-PRIMEIRO` se refere ao bloco **O que fazer** do drawer, “montado SO das chaves gravadas na linha do retrato (motivo, dono, tipo, datas), sem texto gerado”. A docstring diz “os valores sao ROTULOS de mapa [...] nunca frase montada”. O selo do drawer exige que o valor inteiro pertença literalmente a um mapa existente. O exemplo do item 2 tem a forma de **rótulos concatenados com datas**. A permissão para essa composição na coluna da tela e no PDF, e sua eventual extensão ao drawer, fica em aberto. `app/docs/CORTES.md:405` · `app/folha/retrato_folha.py:1089` · `app/folha/tests/test_retrato_folha_drawer.py:213`

### H. Testes atuais e o que fica vermelho [nao verificado]

- **PDF.** `test_retrato_pdf.py` cobre bytes `%PDF`, tipo, nome canônico no header, hash e registro, igualdade de tamanho do recorte com a tela, filtros na trilha, zero chamada à fonte, papel vazio, permissão, link no parcial e recorte comum por AST. O teste de header procura o **nome** e aceita `attachment` com o mesmo nome. O teste de clique segue o `href` com `client.get`. Ele não mede abas ou navegação de navegador. `app/folha/tests/test_retrato_pdf.py:93` · `app/folha/tests/test_retrato_pdf.py:99` · `app/folha/tests/test_retrato_pdf.py:108` · `app/folha/tests/test_retrato_pdf.py:127` · `app/folha/tests/test_retrato_pdf.py:138` · `app/folha/tests/test_retrato_pdf.py:152` · `app/folha/tests/test_retrato_pdf.py:163` · `app/folha/tests/test_retrato_pdf.py:221` · `app/folha/tests/test_retrato_pdf.py:252` · `app/folha/tests/test_retrato_pdf.py:330`
- **Por quê.** Há assert do cabeçalho e do vínculo com `MOTIVO_RH`. O teste da listagem verifica a presença do rótulo de cadastro, sem exigir igualdade da célula inteira. Se a composição for aplicada ao valor do **drawer**, o selo de literal de mapa fica vermelho. Se a janela passar a mudar o texto de **Por quê** na linha comparada pelo teste de período, o assert que preserva todas as colunas salvo Dias fica vermelho. `app/folha/tests/test_retrato_folha_tela2.py:253` · `app/folha/tests/test_retrato_folha_contador.py:190` · `app/folha/tests/test_retrato_folha_drawer.py:181` · `app/folha/tests/test_retrato_folha_drawer.py:213` · `app/folha/tests/test_retrato_folha_periodo.py:371`
- **Ordem, contagem e botão.** Alterar `DONOS` para obter a ordem do PDF quebra o assert da ordem da barra. Mudar a ordem compartilhada de forma que a tela deixe de seguir as faixas quebra as expectativas de ordenação e período. Escrever um botão PDF fora do partial quebra L9. Os REDs abaixo são previsões por leitura. Execução da suíte `[nao verificado]`. `app/folha/tests/test_retrato_folha_dono_sao_tres.py:47` · `app/folha/tests/test_retrato_folha_dono_sao_tres.py:105` · `app/folha/tests/test_retrato_folha_quadros.py:569` · `app/folha/tests/test_retrato_folha_periodo.py:354` · `app/core/tests/test_contract_btn_pdf.py:87`

### I. Matriz de REDs descritos

| Item | Entrada de teste | Saída esperada | [nao verificado]
|---|---|---|
| **1. Download** | Admin com empresa, competência e pílula ativos. Clicar no PDF após atualizar o filtro. | Mesmo `href` do recorte vigente, PDF válido, `attachment` com o nome canônico, mesma URL da tela e mesma quant

[truncado em 20000 de 24044 caracteres]

## 28/11 00:31 · sessao 48b9f6bb0a8f · FERIADO-BOTAO-CORTADO · verificador · origem=Code

**APROVADO para a cura c12 no recorte `base.html` a 1366 px, página e drawer. Nenhum bloqueante de código encontrado.** O BLOQ 1 está curado pela ordem de pintura. A execução do GREEN e do RED da c12, a suíte inteira e o clique real pré-pouso seguem [nao verificado]. O clique real continua como pré-requisito do deploy. `aval_desktop.txt:1` · `ver3_triagem.md:3` · `app/templates/core/_drawer_generico.html:3` · `app/templates/colaboradores/partials/_calendario_grade.html:68` · `CLAUDE.md:206` · `CLAUDE.md:427`

### 1. Ordem de pintura e chamadores

O cabeçalho continua `sticky; top:0` e passa de `auto` para `z-index:1` **dentro** do drawer `10999`. Toda caixa de célula passa atrás do cabeçalho quando há sobreposição. A célula mensal isolada fica na camada `0` do drawer. O `1` do gatilho e o `0` da região rolável são internos à célula. O título e o × pertencem ao mesmo cabeçalho. No Retrato, o calendário entra direto no corpo rolável. No Espelho, entra pela aba com rolagem própria. `app/templates/core/_drawer_generico.html:3` · `app/templates/core/_drawer_generico.html:8` · `app/templates/core/_drawer_generico.html:134` · `app/templates/colaboradores/partials/_calendario_grade.html:68` · `app/templates/colaboradores/partials/_calendario_grade.html:95` · `app/templates/colaboradores/partials/painel_colaborador.html:182` · `app/templates/folha/_retrato_o_que_fazer.html:73` · `app/folha/retrato_folha.py:1019`

**A célula continua abaixo do cabeçalho com o popover aberto. O painel destacado fica acima.** O JS move esse painel para o `body` em `11000`. A confirmação em `100000` cobre a viewport e recebe o clique sobre o × enquanto estiver visível. O posicionador limita o popover ao retângulo visível do drawer sem reservar a faixa do cabeçalho. Se o painel cruzar o ×, ele ganha o hit-test. O cruzamento efetivo a 1366 px [nao verificado]. O detalhe geo hospedado no corpo também já fica acima do cabeçalho pelo `9999` local. `app/static/js/popover-acao.js:36` · `app/static/js/popover-acao.js:67` · `app/static/js/popover-acao.js:127` · `app/templates/core/_dialogo.html:112` · `app/templates/core/_dialogo.html:119` · `app/templates/colaboradores/calendario_partial.html:70` · `app/templates/colaboradores/partials/_geo_modal.html:2`

**A promoção também alcança outros conteúdos na camada `0`.** A barra inferior `sticky` de Ausências e os controles do painel do colaborador enquanto recebem `transform` podiam pintar acima do cabeçalho por ordem de árvore. Agora pintam abaixo dele se houver interseção. O cabeçalho da tabela da seção de Férias (`2`), as sugestões de Posto (`5`) e as de Vínculo (`50`) continuam acima em caso de interseção. Com o cabeçalho subindo de `0` para `1`, nenhum conteúdo passa de baixo para cima. Sobreposição efetiva nesses caminhos e as camadas geradas pelas bibliotecas externas de mapa [nao verificado]. `app/templates/core/_drawer_generico.html:4` · `app/templates/ponto/partials/painel_ausencia.html:286` · `app/templates/colaboradores/partials/painel_colaborador.html:147` · `app/templates/ferias/_secao_colab.html:112` · `app/static/css/hasner-ponto.css:433` · `app/templates/colaboradores/partials/painel_posto.html:57` · `app/templates/colaboradores/partials/painel_vinculo.html:153` · `app/templates/colaboradores/postos.html:74`

| Caminho | Chamadores na cópia |
|---|---|
| Inclusão do genérico | As duas cascas incluem o drawer no corpo. A inclusão textual da Central está dentro do `<title>`. A de Folha está fora dos blocos do template filho. O drawer ativo nesses dois caminhos vem de `base.html`. `app/templates/base.html:7` · `app/templates/base.html:399` · `app/templates/base_app.html:154` · `app/templates/chamados/painel_gestao.html:1` · `app/templates/folha/index.html:1` |
| Colaboradores | Lista via `abrirPainelColab`, Benefícios, Postos e diagnóstico pelas duas entradas. `app/templates/colaboradores/partials/_lista_tabela.html:16` · `app/templates/core/_drawer_generico.html:91` · `app/templates/core/_drawer_generico.html:106` · `app/templates/colaboradores/beneficios_lista.html:86` · `app/templates/colaboradores/postos.html:65` · `app/templates/colaboradores/partials/_ps_linha.html:4` · `app/templates/colaboradores/partials/pendencias_disputa.html:9` |
| Folha e Retrato | Raio-X da exportação, Raio-X do Retrato, ficha pela lista ou pelo Raio-X, e links de ação para Fio, Vínculo, Cadastro, Calendário e Ausência. `app/templates/folha/index.html:26` · `app/templates/folha/_retrato_barra.html:73` · `app/templates/folha/_retrato_lista.html:61` · `app/templates/folha/_retrato_raiox.html:70` · `app/templates/folha/_retrato_o_que_fazer.html:73` · `app/folha/retrato_folha.py:989` · `app/folha/retrato_folha.py:1003` · `app/folha/retrato_folha.py:1016` |
| Chamados e mapas | Arquivados do atendimento, lote, busca de arquivo e Raio-X da Central, troca de empresa na lente e três entradas que escrevem direto no drawer para mostrar mapa. `app/templates/chamados/meu_atendimento.html:45` · `app/templates/chamados/painel_gestao.html:14` · `app/templates/chamados/painel_gestao.html:416` · `app/templates/chamados/painel_gestao.html:629` · `app/templates/chamados/partials/_raiox_lente.html:5` · `app/templates/chamados/painel_gestao.html:574` · `app/templates/chamados/fio_colaborador.html:304` · `app/static/js/geo-colab-map.js:33` |
| Ausências, Férias e Holerite | Painel de Ausências, seção de Férias pela lista e pela aba da ficha, upload de lote pela lista DP e pelo histórico, e detalhe da matriz por modo. `app/templates/ponto/ausencias.html:217` · `app/templates/ferias/lista.html:81` · `app/templates/core/_drawer_generico.html:147` · `app/templates/holerite/lista_dp.html:12` · `app/templates/holerite/historico_lotes.html:7` · `app/templates/holerite/_quadrinho_celula.html:17` |

### 2. Quem escreve e quem lê

| Valor | Escrita e leitura |
|---|---|
| Estilo do cabeçalho e camadas | O template escreve `sticky`, `top:0` e o novo `z-index:1`. O navegador consome essas declarações para pintar e resolver o hit-test. O mutante lê o retorno de `removeProperty` e remove o valor inline apenas no DOM do ensaio. Drawer, painel destacado e confirmação têm seus próprios escritores de camada. `app/templates/core/_drawer_generico.html:3` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:214` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:231` · `app/static/js/popover-acao.js:133` · `app/templates/core/_dialogo.html:112` |
| Seletores do cabeçalho e do drawer | O genérico fornece `#drawer-geral`, `#drawer-geral-backdrop`, `#drawer-geral-titulo` e o botão que chama `fecharDrawer`. A sonda lê o drawer e o fundo e acha o botão no pai do título. A abertura padrão e os três caminhos de mapa escrevem texto no mesmo título. O botão continua chamando a função que recolhe o drawer e emite `drawer:fechou`. `app/templates/core/_drawer_generico.html:2` · `app/templates/core/_drawer_generico.html:11` · `app/templates/core/_drawer_generico.html:71` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:210` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:246` · `app/templates/chamados/painel_gestao.html:576` · `app/templates/chamados/fio_colaborador.html:306` · `app/static/js/geo-colab-map.js:35` |
| Seletores e dia do feriado | O parcial do feriado escreve `data-feriado-vira-trabalho` e `value` no campo oculto `dia`, com a data ISO. A grade escreve `data-celula-dia` no ancestral. Python escreve `DIA.isoformat()` no JSON. As duas sondas usam os mesmos seletores para achar ação e célula. O formulário envia a mesma data à view. O delta reaproveita os atributos existentes. `app/templates/ponto/partials/_feriado_vira_trabalho.html:9` · `app/templates/colaboradores/partials/_calendario_grade.html:68` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:212` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:298` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:756` · `app/ponto/views.py:3584` · `app/ponto/views.py:3615` |
| Entrada e saída novas do smoke | Python escreve `cabecalhoDrawer` no JSON para selecionar o cenário do cabeçalho. A sonda o lê para buscar `urlCalendario` direto e entrar na medição. `urlPainel` continua indicando que o cenário usa drawer e `respostas` serve os HTMLs do Client. A sonda escreve `gridArea`, solicita a rolagem, relê `scrollTop` e mede os centros após um quadro. Ela escreve `fecharNoCabecalho` e `tituloNoCabecalho` como resultados de hit-test. O driver CDP transmite os valores e Python os compara. Os booleanos medem quem recebe o ponto. O fechamento do drawer e o texto do título são observações distintas. `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:124` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:218` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:235` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:244` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:579` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:756` · `app/core/tests/smoke_feriado_chromium.js:52` |

### 3. Mordida e cobertura

- **Tirar o `z-index`** é o RED explícito. A sonda tira a declaração inline, exige que a célula esteja sob os dois pontos e o `MORDE` exige `False` nos dois hit-tests. Uma falha de montagem ou a ausência prévia da declaração produz outra mensagem e reprova o controle negativo. A execução do par GREEN/RED [nao verificado]. `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:214` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:231` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:612` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:765`
- **Pôr `z-index` abaixo de `1`** deve derrubar o positivo. Em `0`, a célula posterior volta a ganhar por ordem de árvore. Com valor negativo, o cabeçalho desce ainda mais. **Tirar o `sticky`** também deve derrubá-lo, pela guarda de sobreposição. Cabeçalho e célula passam a rolar juntos e a célula deixa de cruzar os centros do título e do ×. São consequências da geometria e das asserções. A execução desses dois mutantes [nao verificado]. `app/templates/core/_drawer_generico.html:3` · `app/templates/colaboradores/partials/_calendario_grade.html:68` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:219` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:231` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:716`
- **Lacuna no desktop.** O novo teste carrega o parcial direto numa casca de ensaio, alarga uma célula e mede os dois centros com o painel fechado. O smoke anterior cobre o popover na ficha e no Espelho, e fecha o drawer chamando `window.fecharDrawer()`. Falta o percurso integrado pelo link real do Retrato, com a grade intacta, rolagem e clique no ×, inclusive a disputa com o popover e a confirmação. O submit do feriado tem hit-test no navegador e teste do POST com motivo no Client Django. O percurso inteiro pelo navegador segue [nao verificado]. `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:208` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:260` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:421` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:488` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:731` · `app/templates/folha/_retrato_o_que_fazer.html:73` · `app/ponto/tests/test_kaua_feriado_pela_tela.py:149`

### 4. Classes que já reprovaram obra

- **Segundo juiz, segundo escritor, relógio fora da borda e derivação paralela** — zero introduzidos no delta fornecido. A grade e o POST leem a oferta de `contexto_calendario`. O POST captura `agora` uma vez e passa esse instante à consulta e à porta. A porta grava a célula e a trilha. `app/templates/colaboradores/partials/_calendario_grade.html:96` · `app/colaboradores/services/calendario.py:791` · `app/ponto/views.py:3622` · `app/ponto/portas/celula.py:650` · `app/ponto/portas/celula.py:665`
- **Campo com dois significados, selo sem mordida e exceções** — zero campos ambíguos encontrados. O novo flag escolhe o cenário, e os dois campos de saída têm uma única leitura de hit-test. O RED exige a falha específica nos dois alvos. O delta introduz **0 `except` Python e 0 `catch` JavaScript**. O tratamento existente propaga a falha da sonda como `motivo`, que o positivo exige nulo. `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:235` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:260` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:502` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:579` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:612`
- **Ordem de trava e pré-requisitos do pouso** — a porta entra em `atomic`, lê com `select_for_update`, aplica `so_se_folga` e então grava. A ordem operacional exigida é suíte inteira verde, revisão final e pouso, com o smoke humano antes do push. O cumprimento dessa ordem e a identidade Git do hash nesta cópia seguem [nao verificado]. A leitura usa o delta fornecido e as linhas correspondentes da cópia. `app/ponto/portas/celula.py:642` · `app/ponto/portas/celula.py:655` · `CLAUDE.md:206` · `CLAUDE.md:421` · `CLAUDE.md:427`

### 5. Lista do pouso [nao verificado]

- **360 px e `base_app` real.** Conferir página e drawer a 360, incluindo rolagem e acionabilidade. A ficha tem coluna inicial de `420px` e o drawer é limitado a `92vw`. Conferir também a entrada e o bootstrap do WebView. O ensaio existente de `base_app` usa desktop a 1366 e desativa o PWA. Resultados reais [nao verificado]. `aval_desktop.txt:1` · `app/templates/colaboradores/detalhe.html:87` · `app/templates/core/_drawer_generico.html:3` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:699` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:745`
- **Smoke humano pré-pouso.** Clique do Ronald no 20/11 real nas duas cascas, com as verificações pendentes do drawer em rolagem. Resultado [nao verificado]. `ver3_triagem.md:3` · `app/docs/LEIS.md:99` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:231`
- **Lacunas da ver3.** Tab, POST com motivo pelo Chromium, paginação real da lista com drawer aberto e reabertura após restaurar histórico. Snapshot antigo com painel órfão também fica na lista. O fechamento pré-snapshot atua sobre o popover corrente. Ocorrência de snapshot antigo afetado [nao verificado]. `ver3_triagem.md:4` · `ver3_triagem.md:5` · `app/templates/colaboradores/partials/_lista_tabela.html:50` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:742` · `app/static/js/popover-acao.js:144` · `app/static/js/popover-acao.js:176`

## 28/11 00:07 · sessao 85f79d3ba089 · FERIADO-UM-JUIZ · verificador · origem=Code

**APROVADO para a c14, sem bloqueantes no delta.** O achado específico da ver9 foi curado. Fixar o total em `1` e retirar o prefixo do `ERROR` agora derrubam os testes. O terceiro mutante confirma a propagação da falha de alimentação. Os três deram RC 0 no harness, que exige controle verde, falha de todos os alvos previstos e zero erros de execução. `app/ponto/tests/feriado_um_juiz_mutantes.py:189`, `app/ponto/tests/feriado_um_juiz_mutantes.py:781`, `app/ponto/tests/feriado_um_juiz_mutantes.py:802`

### 1. Mordida dos mutantes

| Mutante | Teste e asserção que falhou | [nao verificado]
|---|---|
| `M_c14_total_fixo_1` | `test_RED_c14_resumo_publica_zero_falhas_tecnicas` e `test_RED_c14_resumo_soma_duas_falhas_tecnicas_na_mesma_passada`. Ambos caíram no `assertIn` do token inteiro em `resumos[0].split()`. Esperavam `falhas_tecnicas_oraculo=0` e `=2`, respectivamente. `app/ponto/tests/test_feriado_um_juiz.py:339`, `app/ponto/tests/test_feriado_um_juiz.py:467`, `app/ponto/tests/test_feriado_um_juiz.py:479` |
| `M_c14_sem_prefixo` | `test_RED_c10_falha_sql_da_alimentacao_reverte_o_savepoint` e `test_RED_c13_value_error_generico_na_alimentacao_e_falha_tecnica`. Ambos caíram no `assertRegex` ancorado em `^dia_pago oraculo: falha tecnica colab=1\b`. `app/ponto/tests/test_feriado_um_juiz.py:481`, `app/ponto/tests/test_feriado_um_juiz.py:484`, `app/ponto/tests/test_feriado_um_juiz.py:505` |
| `M_c14_engole_alimentacao` | `test_RED_c13_falha_na_alimentacao_propaga_quando_solicitado`. Caiu no `assertRaises(RuntimeError)` com `propagar_falha=True`. A asserção de identidade da causa fica depois e não foi alcançada. `app/ponto/tests/test_feriado_um_juiz.py:507`, `app/ponto/tests/test_feriado_um_juiz.py:510`, `app/ponto/tests/test_feriado_um_juiz.py:512` |

**Ainda há lacunas.** O incremento tem mutante c13 e o resumo agora tem prova para `0`, `1` e `2`. Em sondas adicionais em memória, o controle de 58 testes continuou verde ao fixar o ID do `ERROR` em `1`, trocar a competência do `ERROR`, silenciar o `ERROR` só na falha de alimentação com `propagar_falha=True` e publicar o resumo mesmo sem tentativa. O teste de dois colaboradores confere duas chamadas de `error`, e o teste de propagação confere a exceção e a causa. Essas lacunas ficam fora dos três defeitos reproduzidos pelos mutantes da c14. `app/ponto/tests/feriado_um_juiz_mutantes.py:226`, `app/ponto/tests/feriado_um_juiz_mutantes.py:776`, `app/ponto/tests/test_feriado_um_juiz.py:477`, `app/ponto/tests/test_feriado_um_juiz.py:500`, `app/ponto/tests/test_feriado_um_juiz.py:507`, `app/ponto/services/fechamento.py:1327`, `app/ponto/services/fechamento.py:1346`

### 2. Quem escreve e quem lê

- **Contador.** `recalcular_fechamento_mes` zera uma vez por passada, incrementa no ramo de exceção técnica da sombra e lê o acumulado como argumento do `INFO`. O resumo sai se houve tentativa e a execução chegou ao fim. A saída por exceção propagada ocorre antes do resumo. No teste, `_confere_vigia` localiza um único resumo e lê o token exato. O vigia do harness também lê o token `=1`. `app/ponto/services/fechamento.py:683`, `app/ponto/services/fechamento.py:1320`, `app/ponto/services/fechamento.py:1327`, `app/ponto/services/fechamento.py:1338`, `app/ponto/services/fechamento.py:1346`, `app/ponto/tests/test_feriado_um_juiz.py:333`, `app/ponto/tests/feriado_um_juiz_mutantes.py:173`
- **`ERROR` técnico.** O mesmo serviço emite `dia_pago oraculo: falha tecnica colab=%s %02d/%s: %s` pelo logger `ponto.services.fechamento`. A regex nova casa o início da mensagem interpolada e distingue esse formato dos formatos atuais de recusa, falha do motor e resumo. Ela aceita outros textos com o mesmo início, inclusive `colab=1-extra`, e deixa competência e resto da mensagem fora do padrão. A asserção anterior confere o motivo separadamente. Os testes de falha de escrita capturam o logger e conferem o motivo injetado. `app/ponto/services/fechamento.py:18`, `app/ponto/services/fechamento.py:1275`, `app/ponto/services/fechamento.py:1323`, `app/ponto/services/fechamento.py:1328`, `app/ponto/services/fechamento.py:1347`, `app/ponto/tests/test_feriado_um_juiz.py:504`, `app/ponto/tests/test_feriado_um_juiz.py:117`, `app/ponto/tests/test_feriado_um_juiz.py:146`, `app/ponto/tests/test_feriado_um_juiz_porta.py:497`
- **Comandos, crons e vigias.** A tela e `recalcular_fechamento` leem `processados`. O backfill conta colaboradores do dicionário retornado. `diff_calculador` lê rubricas diretamente. O cron `pre_fechamento` lê bloqueios e marcos. No SaaS, `pontofile` recebe `INFO` e `ERROR`, e o console recebe `ERROR`. O formatador põe data, nível e nome do logger antes da mensagem, então a regex nova, com `^`, atua sobre o corpo capturado no teste. ESTADO e HAIKU leem `ponto.log` por um parser que exige `ponto.recusa_critica RECUSA-ACAO-CRITICA`. Leitor automatizado da telemetria técnica em produção **[nao verificado]**. `app/ponto/views.py:1280`, `app/ponto/management/commands/recalcular_fechamento.py:130`, `app/ponto/management/commands/lavrar_dias_pagos.py:106`, `app/ponto/management/commands/diff_calculador.py:406`, `app/config/crons.py:406`, `app/folha/management/commands/pre_fechamento.py:46`, `app/folha/management/commands/pre_fechamento.py:74`, `app/config/settings/saas.py:122`, `app/config/settings/saas.py:160`, `app/config/settings/saas.py:181`, `bin/gerar_estado.py:265`, `bin/recusa_estado.py:45`, `bin/recusa_estado.py:68`, `app/api/views_mensageria.py:1872`, `app/core/recusa_critica.py:46`

### 3. Isolamento e execução

Os dois testes passaram isolados e nas duas ordens. Cada cenário cria seus próprios mocks e batidas, fixa `timezone.now()` em 05/11/2026 às 12h e captura diretamente o logger do fechamento. O teste de duas falhas exige a ordem da lista que ele mesmo injeta. O delta acrescenta **0 `try` e 0 `except`**. `app/ponto/tests/test_feriado_um_juiz.py:161`, `app/ponto/tests/test_feriado_um_juiz.py:244`, `app/ponto/tests/test_feriado_um_juiz.py:256`, `app/ponto/tests/test_feriado_um_juiz.py:286`, `app/ponto/tests/test_feriado_um_juiz.py:309`, `app/ponto/tests/test_feriado_um_juiz.py:462`, `app/ponto/tests/feriado_um_juiz_mutantes.py:781`

Rodei os três módulos localmente com o harness em memória e substitutos de `SimpleTestCase`, `DiaPago` e `timezone.make_aware`. Resultado **102 testes, OK, uma falha esperada**, na borda 20/21 com troca de 6x1 para 12x36. Os dois blobs de teste conferem com o anexo. Identidade do commit inteiro, execução da pista, ORM com banco e logs de produção **[nao verificado]**. `app/ponto/tests/feriado_um_juiz_mutantes.py:502`, `app/ponto/tests/feriado_um_juiz_mutantes.py:519`, `app/ponto/tests/test_feriado_um_juiz.py:986`

### 4. Demais achados da ver9

- **Saída antecipada.** Permanece o cenário de `06:00 E / 06:30 S`: mensal e motor preservam zero trabalhado e 420 min de saída antecipada, e a sombra conserva a lavra anterior por recusa de procedência. O descarte do bloqueio 2 é o informado no pedido. A L-095 atribui ao motor a lavra do dinheiro do dia. `app/ponto/tests/test_feriado_um_juiz.py:322`, `app/ponto/tests/test_feriado_um_juiz.py:386`, `app/ponto/services/dia_pago.py:321`, `app/docs/LEIS.md:113`
- **C13, BUG-144 e O116.** A classificação por `RecusaDeProcedencia`, os 357 min preservados, a guarda por dia e os três tipos de FT continuam cobertos. A falha técnica que chega ao fechamento continua propagando na revisão, cujo `atomic` externo desfaz o ato. A captura interna da fábrica de motor continua podendo absorver uma exceção antes desse tratador. `app/ponto/tests/test_feriado_um_juiz.py:341`, `app/ponto/tests/test_feriado_um_juiz.py:396`, `app/ponto/tests/test_feriado_um_juiz.py:419`, `app/ponto/services/fechamento.py:1320`, `app/ponto/services/fechamento.py:1893`, `app/ponto/services/par_relampago_na_lista.py:479`, `app/ponto/calculador/alimentacao.py:429`
- **C12.** A catraca de linhas passou. Duas ocorrências na mesma linha continuam virando uma entrada, pelo `set` do censo. `app/core/tests/test_medida_turno_do_par.py:565`, `app/core/medida_turno_do_par.py:224`, `app/core/medida_turno_do_par.py:228`

### 5. Classes de reprovação

- **Segundo juiz e segundo escritor.** As duas versões continuam pela mesma `lavrar`, com a partição existente. O retrato da exportada é uma porta preexistente. `app/ponto/services/fechamento.py:997`, `app/ponto/services/fechamento.py:1236`, `app/ponto/services/fechamento.py:1296`, `app/ponto/services/fechamento.py:1300`, `app/ponto/services/dia_pago.py:370`, `app/ponto/services/dia_pago.py:417`
- **Relógio e ordem de trava.** O teto do furo recebe o instante da borda da passada e a lista resultante vai às duas lavras. A revisão mantém Empresa por PK → Colaborador → Batida por PK, antes do recálculo. `app/ponto/services/fechamento.py:680`, `app/ponto/services/fechamento.py:1154`, `app/ponto/services/fechamento.py:1262`, `app/ponto/services/fechamento.py:1316`, `app/ponto/services/par_relampago_na_lista.py:172`, `app/ponto/services/par_relampago_na_lista.py:483`
- **Derivação paralela e campo com dois significados.** Persistem as divergências anteriores. Períodos e FT usam o mapa da jornada quando fornecido, enquanto o atraso de ligação usa a data civil de entrada. `nao_decide` sinaliza rubrica sem afirmação no DIFF e também satisfaz a declaração de dono na guarda. Efeito em produção **[nao verificado]**. `app/ponto/services/dia_pago.py:62`, `app/ponto/services/dia_pago.py:109`, `app/ponto/services/dia_pago.py:228`, `app/ponto/services/credito_parcial.py:73`, `app/ponto/management/commands/diff_calculador.py:508`
- **Selo sem mordida.** Os três mutantes novos morreram nas asserções indicadas, com as lacunas remanescentes registradas no item 1. `app/ponto/tests/feriado_um_juiz_mutantes.py:189`, `app/ponto/tests/feriado_um_juiz_mutantes.py:802`

## 28/11 00:00 · sessao 6e7123508798 · FIM-DE-SEMANA-JA · leitura · origem=Code

**FIM-DE-SEMANA-JA.** O recorte traz sete pares em cinco disputas fechadas e uma cobrança indevida no chamado 38343. A cópia local não tem metadados Git para confirmar o hash `59600163ccc9cffb8f3ae148beede7cd7866ae86`. Identidade com o HEAD informado, estado atual dos casos e resultado de qualquer operação em produção [nao verificado]. `lista_por_par.md:3–8` `S2_r.md:14`

## 1. Quem decide

`ChamadoColaborador._decisao_do_fechamento` é a autoridade das duas recusas. `reconciliar()` executa a decisão e `reconciliaria()` consulta a mesma decisão em seco. A ordem é motor de estados, premissa do furo, conversa do pai e pergunta viva. Assim, `dia_furado_para_a_folha` pode ser a primeira recusa mesmo quando também há pergunta viva. `app/chamados/models.py:661–715`

- **`pergunta_viva_na_competencia_aberta`.** O gate busca perguntas sem validação ancoradas no chamado. A âncora é `chamado_destino`, o filho, ou `disputa.chamado` quando falta destino. Ele filtra pela competência do dia e consulta o C6 com o **estado que o filho teria após fechar**. O C6 lê a sequência de decisões, a resposta, a tranca, a célula e o lastro. Em um ramo do lastro, lê também o estado do filho. `fechada_em` da disputa, por si só, não encerra a pergunta. Se todas as vivas podem ser reatadas a outra cobrança legítima do mesmo colab-dia, o próprio gate reaponta e grava a trilha. Se alguma não pode, recusa. `app/chamados/models.py:349–414` `app/chamados/models.py:429–464` `app/chamados/juizes.py:835` `app/chamados/juizes.py:1499–1589`
- **`dia_furado_para_a_folha`.** O gate lê o chamado de furo e seu dia forte, depois `premissa_regularizada(para='folha')`. Entram justificativa aprovada, célula, veredito do admin, ata e lâmpadas dos marcos, cobertura de ausência e os fatos de jornada quando a célula não responde. Também aceita agrupamento sob pai vivo ou deduplicação se outra cobrança for a sobrevivente do C8. O gate de conversa do pai é separado e protege respostas sem veredito. `app/chamados/models.py:718–739` `app/chamados/models.py:783–837` `app/chamados/models.py:1318–1382` `app/chamados/juizes.py:508–569`
- **Retenção da folha.** `classificar_export` consulta `motivos_retencao_celula` pelo espelho. O leitor usa a lavra e os protestos da célula, com as exceções previstas. A S4 mede pares do C2 no universo de chamados vivos. A prova de saída da retenção exige conferir o dia na Folha além de conferir o par na S4. `app/folha/export.py:977–982` `app/folha/export.py:1282–1316` `app/folha/fora_da_folha.py:125–165` `S4_r.md:1`

## 2. Portas existentes e seus efeitos

| Porta | Quem aciona, trilha e repetição | Batida, célula, escala/vínculo e dinheiro gravado |
|---|---|---|
| **Responder à pergunta**, no app ou PWA | O próprio colaborador autenticado. O POST confere dono e C6 mesmo com a disputa fechada. Grava `RESPOSTA` com usuário, texto e instante, preserva os atos anteriores e registra o motivo da pergunta no log. O app também registra o resultado do POST. Reenvio pode gravar outro ato. Uma resposta de hora pode seguir viva até o veredito. `app/api/views.py:2607–2664` `app/chamados/views.py:1228–1279` `app/chamados/services/disputa_emissao.py:1529–1611` `app/chamados/juizes.py:1521–1533` | **Batida não. Célula sim**, se a resposta gerar ausência ou disparar a reconciliação do dia. **Escala/vínculo não. Dinheiro gravado não diretamente**. Falta ou atestado pode alterar o insumo da folha e invalidar sua projeção. `app/chamados/services/disputa_emissao.py:1591–1596` `app/chamados/services/materializacao.py:1010–1046` `app/ponto/signals.py:148–152` `app/ponto/services/ausencia.py:581–597` |
| **Validar** no Fio do admin, no cartão do filho | Gestor autenticado com permissão contextual no chamado. O Fio checa a pergunta exata, a âncora e o C6 sob trava, e passa `sem_reabrir=True` para a fechada. Exige resposta com texto e instante. Grava `VALIDACAO` com usuário, instante e texto, além de `val_perg_disp` e `validar_viva_em_disputa_fechada` quando o ato fica. A sequência preserva os atos anteriores. Já validada é recusada sem regravar. `app/chamados/views.py:24–38` `app/chamados/views.py:869–875` `app/chamados/utils.py:47–72` `app/chamados/services/fio.py:694–768` `app/chamados/services/validacao.py:164–178` `app/chamados/services/validacao.py:755–765` `app/chamados/services/validacao.py:825–843` | **Batida sim, pode plantar. Célula sim. Escala/vínculo não** no sentido de editar o vínculo ou o tipo de escala, mas o aceite de folga pode gravar `FolgaDia`. **Dinheiro gravado sim**, pode recalcular e persistir `FechamentoMensal`. O materializador pode alcançar irmãs já validadas. `app/chamados/services/validacao.py:587–602` `app/chamados/services/validacao.py:777–830` `app/chamados/services/materializacao.py:2040–2061` `app/chamados/services/materializacao.py:2240–2301` `app/ponto/services/fechamento.py:1184–1192` |
| **Resolver dia**, no calendário, para um colab-dia | Admin com `regularizar_batida`. O botão exige dia passado, estado elegível e condição de último recurso. O serviço seleciona as perguntas de fato vivas do dia, inclusive de disputa fechada. Grava `admin_decidiu`, `SILENCIO` com autor, instante e motivo, e a lavra com antes/depois. O carimbo e a lavra iguais são idempotentes. Um novo POST aceito pode repetir o log do despachante. `app/ponto/views.py:3517–3556` `app/colaboradores/services/calendario.py:94–122` `app/colaboradores/services/calendario.py:493–508` `app/templates/colaboradores/partials/_calendario_grade.html:134` `app/ponto/services/veredito_celula.py:176–212` `app/ponto/services/veredito_celula.py:351–363` `app/chamados/models.py:3372–3386` `app/ponto/portas/celula.py:1113–1141` | **Batida sim** em `trabalhou`, se houver correção ou plantio. **Batida não** em `faltou` ou `justificado`, que podem criar ausência aprovada. **Célula sim. Escala/vínculo não. Dinheiro gravado sim**, se houver plantio com recálculo. A ausência pode invalidar a projeção, com efeito nos valores quando rederivados. `app/ponto/services/veredito_celula.py:317–357` `app/ponto/registro_batida.py:176–198` `app/ponto/services/ausencia.py:154–195` `app/ponto/services/ausencia.py:597` `app/ponto/services/fechamento.py:1822–1848` |
| **Encerrar** (`resolvido`) no chamado local | Gestor autenticado com permissão contextual. No detalhe local, o chamado precisa estar sem GLPI. A mensagem com motivo fica no fio com o usuário. `estado_chamado` grava antes/depois e via `admin`. A transição exige premissa regularizada para chamado de furo e recusa pergunta viva ancorada ou disputa própria aberta que segure. Repetir o status é recusado, mas a mensagem já terá sido gravada e pode duplicar. `app/chamados/views.py:358–370` `app/chamados/views.py:418–421` `app/chamados/services/verbos.py:132–136` `app/chamados/services/acoes_chamado.py:292–331` `app/chamados/models.py:140–211` `app/chamados/models.py:304–308` `app/chamados/catalogo/motor.py:148–149` | **Batida não diretamente. Célula sim**, pelo rejulgamento disparado pela mudança de estado. **Escala/vínculo não. Dinheiro gravado não diretamente.** Se a cascata fechar outra disputa aberta, pode retomar regularização já confirmada e alcançar batida e recálculo. Esse efeito em 38343 [nao verificado]. `app/chamados/signals.py:91–118` `app/chamados/signals.py:123–146` `app/chamados/signals.py:189–218` `app/chamados/services/disputa_emissao.py:2221–2225` `app/chamados/services/regularizacao_ext.py:77–78` `app/ponto/registro_batida.py:176–198` |
| **`supra_juiz --executar`**, retratação por lavra | Processo autorizado a rodar `manage.py` no tenant. Exige lavra `cobranca_indevida` com código, preserva conversa sem veredito, exclui a órfã de regeneração e passa pela retratação canônica e pelos gates de fechamento. Grava `estado_chamado` antes/depois com ator de processo `sistema:supra_juiz` e nota `retratado` com o motivo. Já encerrado sai da seleção de vivos. A unidade pode rejeitar justificativa vinculada. O menor recorte disponível é empresa e data, sem filtro de chamado ou colab. `app/ponto/management/commands/supra_juiz.py:61–74` `app/ponto/management/commands/supra_juiz.py:110` `app/ponto/management/commands/supra_juiz.py:246–295` `app/chamados/reconciliador.py:558–620` `app/core/observ.py:22–32` | **Batida não diretamente. Célula sim**, pelo cartório. **Escala/vínculo não. Dinheiro gravado não diretamente.** Vale a mesma ressalva de cascata do fechamento do chamado. `app/chamados/reconciliador.py:602–620` `app/chamados/signals.py:123–146` `app/chamados/signals.py:189–218` |

Em **Resolver dia**, a tentativa de reconciliar os chamados vem antes da lavra `admin`. A premissa pode recusar nessa primeira tentativa. A lavra posterior faz o coletor da premissa aceitar a decisão humana, mas o fechamento do filho ainda precisa ser comprovado. A fila de eventos da célula volta a tentar os casos elegíveis. `app/ponto/services/veredito_celula.py:207–212` `app/ponto/services/veredito_celula.py:351–357` `app/chamados/models.py:1345–1349` `app/ponto/portas/celula.py:1130–1135` `app/chamados/reconciliador.py:1513–1533`

## 3. Pergunta viva em disputa fechada

**Há portas sem reabrir.** O colaborador pode responder pelo POST autenticado, o gestor pode validar uma resposta no Fio, e **Resolver dia** pode encerrar uma pergunta de fato com decisão comprovada do dia. O app põe as vivas de disputas fechadas no histórico, e o Fio web corrente do colaborador carrega as abertas. O teste da API comprova o aceite do POST de uma pergunta do histórico. A existência de um botão alcançável e de sessão utilizável para estes casos [nao verificado]. `app/api/tests/test_hx_api_estado.py:54–65` `app/chamados/services/pergunta_no_app.py:80–92` `app/chamados/services/fio.py:287–291` `app/chamados/services/fio.py:721–768` `app/ponto/services/veredito_celula.py:176–202`

**SEM PORTA de expiração avulsa da muda.** A expiração por silêncio saiu do comando de arquivamento. `reconciliar_perguntas_orfas` é vigia, e o escritor por lâmpada seleciona disputas abertas. `admin_encerra` é método sem ação de tela localizada e, para disputa já fechada, encontra a guarda de fechamento antes de carimbar a pergunta. `app/chamados/management/commands/arquivar_competencia_encerrada.py:61–64` `app/ponto/management/commands/reconciliar_perguntas_orfas.py:1–8` `app/chamados/reconciliador.py:1166–1173` `app/chamados/models.py:2094–2102` `app/chamados/models.py:2210–2217` `app/docs/BACKLOG.md:1635`

A **Reabertura administrativa** existe no cartão do pai. Staff ou `admin_ti`, com permissão contextual, escolhe motivo do catálogo e escreve justificativa de pelo menos 20 caracteres. A porta escolhe a fechada mais recente daquele pai, consome uma das duas reaberturas, cria nova pergunta, pode reabrir o chamado pai e grava nota assinada e log com motivo e contador. A nota guarda a justificativa. O log de reabertura não traz campos estruturados de antes/depois. Repetir o POST pode selecionar outra fechada do mesmo pai. O ato direto tem **batida não, célula possível pelo sinal do chamado, escala/vínculo não, dinheiro gravado não**. O refechamento comum continua sujeito ao C7, que recusa viva ou resposta sem veredito. **Reabrir e refechar não é uma saída para dar baixa na muda.** `app/templates/chamados/partials/modal_fio.html:418–443` `app/chamados/services/fio.py:694–719` `app/chamados/services/acoes_disputa.py:274–339` `app/chamados/models.py:2396–2431` `app/chamados/models.py:2122–2145` `app/chamados/signals.py:189–218`

## 4. Roteiro para as oito ocorrências

| Ocorrência | Porta recomendada |
|---|---|
| **col296 · 10/11 · 38343** | **Encerrar**, no chamado exato, pelo gestor autorizado, com o motivo factual da cobrança indevida. Confirmar primeiro que o seletor está disponível e que as guardas permitem `resolvido`. **Validar** trata uma resposta de pergunta. **Cancelar** é evento de sistema e o seletor humano o recusa. Se **Encerrar** recusar, a baixa desse PK fica **SEM PORTA comprovada no recorte**. O `supra_juiz --executar` tem alcance sobre outros chamados da empresa no dia. `S2_r.md:14` `app/templates/chamados/detalhe_local.html:374–405` `app/chamados/models.py:160–211` `app/chamados/views_cobrar.py:160–172` `app/chamados/catalogo/motor.py:139–149` `app/ponto/management/commands/supra_juiz.py:61–74` |
| **col719 · 01/11 · 7751 → 36403** | **Responder**, pelo próprio col719, se a pergunta exata estiver alcançável no canal existente. A S4 indica que falta a resposta. **SEM PORTA** de baixa administrativa avulsa com a evidência fornecida. Se a resposta continuar viva aguardando veredito, o gestor usa **Validar** no mesmo filho. `lista_por_par.md:4` `app/api/tests/test_hx_api_estado.py:54–65` `app/chamados/services/fio.py:721–768` `app/chamados/services/validacao.py:755–758` |
| **col719 · 03/11 · 7817 → 36796** | **Responder**, nas mesmas condições, para a pergunta deste filho. Alcance real [nao verificado]. `lista_por_par.md:5` `app/chamados/views.py:1228–1279` |
| **col719 · 05/11 · 7888 → 37210** | **Responder**, nas mesmas condições, para a pergunta deste filho. Alcance real [nao verificado]. `lista_por_par.md:6` `app/chamados/views.py:1228–1279` |
| **col935 · 18/11 · 8272 → 40002** | **Resolver dia**, individual, pelo admin com `regularizar_batida`, se a UI oferecer o dia e houver evidência para o veredito. **SEM PORTA** para baixar só a pergunta e liberar um dia cuja premissa ainda acusa. Oferta e veredito cabível [nao verificado]. `lista_por_par.md:7` `app/colaboradores/services/calendario.py:94–122` `app/ponto/views.py:3517–3546` `app/chamados/models.py:793–837` |
| **col976 · 22/10 · 7385 → 34061 · três pares** | **Um Resolver dia** para 22/10, sob as mesmas condições. Conferir o conjunto de perguntas de fato vivas desse dia antes do ato, pois a porta opera no colab-dia. **SEM PORTA** para baixar os três pares mantendo uma premissa de furo que ainda acusa. IDs das três perguntas, oferta e veredito cabível [nao verificado]. `lista_por_par.md:8` `app/ponto/services/veredito_celula.py:176–212` `app/chamados/models.py:793–837` |

1. **Antes dos atos**, obter a foto nominal em Sombra atualizada, empresa 2, competência 11. Registrar PK da pergunta, destino, C6, resposta válida ou não, `reconciliaria()` e lavra. Em seguida, tratar 38343 pelo chamado exato, os três dias de col719 pela resposta e os dois dias com recusa de premissa pelo calendário, conforme as condições da tabela. Essa ordem é operacional. A decisão do gate é por chamado e dia. `S4_r.md:1–3` `app/chamados/models.py:677–715` `app/chamados/models.py:793–837`
2. **Depois de 38343**, conferir a transição com motivo no fio e a célula de 10/11. A mudança de status dispara o cartório em melhor esforço. **O col296 sai da marca `cobranca_indevida` da S2 quando a lavra deixar de trazer essa marca.** `COBRANCA_SEM_FURO` exige cobrança viva que ainda protesta, dia de trabalho, realizado suficiente e todas as células casadas. O encerramento de 38343 pode retirar esse insumo. Outra cobrança, outra retenção ou falha de rejulgamento no caso concreto [nao verificado]. `S2_r.md:14` `app/chamados/signals.py:189–218` `app/ponto/services/cartorio.py:1498–1508` `app/ponto/services/cartorio.py:1905–1913` `app/ponto/supra_juiz.py:476–484` `app/escala/models.py:1624` `app/folha/export.py:1282–1305`
3. **Repetir S4 e S2 na foto posterior.** Para os sete pares, usar o coletor **`S4|C2` embutido em `S4_r.md`**, que requer o fonte original de `sonda_S1.py`. O bloco colado com o nome `s4_sonda.py` imprime **`S3|C1`** e serve para detalhar dias. Comparar por `(chamado, pergunta)` e conferir o ato gravado, o C6 e o estado do filho. Para a S2, repetir a coleta S1 que sustenta seu C1 e conferir a lavra de 10/11 do col296. Por fim, confirmar o dia em `dias_que_seguram` e a linha do colaborador em `classificar_export`. Disponibilidade do fonte original, nova coleta e efeito final no TXT [nao verificado]. `S4_r.md:1–3` `S2_r.md:7–14` `app/chamados/models.py:677–681` `app/folha/fora_da_folha.py:125–165` `app/folha/export.py:977–989`

## 27/11 23:59 · sessao f5b9cdfcd0a7 · - · - · origem=Ronald (tmux)

Para o **col296, chamado 38343**, a menor porta nominativa é o detalhe do chamado local. Um gestor autenticado da área registra o motivo no campo de resposta e seleciona **Encerrar** (`resolvido`). A ação exige chamado local sem GLPI e, para chamado de furo, exige premissa regularizada. Recusa se houver disputa própria aberta com guarda ou pergunta viva ancorada. Status exato, vínculo com GLPI, permissões efetivas e resultado para o #38343 ficam [nao verificado]. `app/chamados/views.py:24,35–38,358–370,418–421,547–552`, `app/chamados/utils.py:47–72`, `app/templates/chamados/detalhe_local.html:374–405`, `app/chamados/services/verbos.py:132–136`, `app/chamados/models.py:140–211`.

A trilha é a nota com motivo e usuário, os logs `responder`/`status` e `estado_chamado` com antes/depois, via `admin` e autor. Repetir o status é recusado, mas a nota precede a transição e pode duplicar. Efeitos diretos do encerramento e do rejulgamento usual: batida **não**, célula **sim** pelo cartório, escala/vínculo **não**, dinheiro gravado **não**. Existe cascata condicional se o S143 fechar outra disputa e ela destravar regularização externa confirmada, com possível alteração em batida e recálculo. Incidência em #38343 [nao verificado]. `app/chamados/services/acoes_chamado.py:292–331`, `app/chamados/models.py:304–308,466–469`, `app/chamados/catalogo/motor.py:148–149`, `app/chamados/signals.py:112–146,189–218`, `app/chamados/services/disputa_emissao.py:2206–2225`, `app/chamados/services/regularizacao_ext.py:170–226`, `app/ponto/registro_batida.py:176–198`.

`validar_inline` valida perguntas vivas respondidas em disputa aberta. `cancelado` é rejeitado na transição humana, mesmo para `admin_ti`; o `cancelar()` interno tem chamadores de comandos de revisão de vínculo/régua. O command `supra_juiz --executar` é a alternativa específica de sistema para lavra `cobranca_indevida`, mas o menor recorte por flags é empresa × dia. Ele preserva conversa sem veredito, cobrança órfã de regeneração e as guardas da retratação/reconciliação. Assina como `sistema:supra_juiz`, com `estado_chamado` antes/depois e nota `retratado` com motivo. O fechamento idempotente só atua em vivos e a retratação pode rejeitar justificativa pendente vinculada. `app/chamados/views_cobrar.py:145–172`, `app/chamados/catalogo/motor.py:139–156`, `app/chamados/models.py:839–858`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:58`, `app/escala/management/commands/fechar_avisos_regua_na_lista.py:48`, `app/ponto/management/commands/supra_juiz.py:61–67,110–118,190–212,246–295`, `app/chamados/reconciliador.py:558–620`, `app/core/observ.py:22–32,94–137`.

**S2:** a baixa faz diferença quando o cartório relavra a célula. O status do chamado integra a impressão e `COBRANCA_SEM_FURO` exige chamado vivo entre os que ainda protestam. Após o ato, conferir o #38343 fora de `VIVOS`, a lavra de 10/11 sem essa marca e o dia ausente de `dias_que_seguram`. Outras retenções do col296 e efeito no TXT [nao verificado]. `app/ponto/services/cartorio.py:490–504,1432–1449,1498–1508,1905–1932`, `app/ponto/supra_juiz.py:476–484`, `app/escala/models.py:1624,1780`, `app/folha/export.py:1282–1303`, `app/folha/fora_da_folha.py:115–165`.

Para órfãs de disputa fechada, a **validação no fio** admite uma viva fechada sem reabertura, desde que respondida. Os endpoints de resposta aceitam dona/dono + C6 viva sem exigir disputa aberta, mas a UI web exclui as fechadas e a consulta do app coloca a muda de disputa fechada no histórico. `Resolver dia`, quando oferecido e respaldado pelos fatos, pega perguntas não conversacionais de colab/dia sem filtrar disputa aberta e as carimba com autor e `SILENCIO`; pode mexer nas batidas e decide o dia inteiro. O `reconciliar` dos filhos vem antes da lavra administrativa, então verificar depois o chamado e a retenção independentemente do C6. `app/chamados/services/fio.py:287–291,721–769`, `app/chamados/services/validacao.py:755–758`, `app/chamados/views.py:1228–1279`, `app/api/views.py:2607–2658`, `app/chamados/services/pergunta_no_app.py:68–92`, `app/ponto/services/veredito_celula.py:180–212,317–363`, `app/colaboradores/services/calendario.py:94–104`.

O método `admin_encerra` exige motivo e só atende silêncio, com trava para resposta sem veredito. A ausência de tela para ele está documentada. Reabrir pela UI acrescenta pergunta de conversa, e o fechamento ordinário volta a recusar C6 viva. `app/chamados/models.py:2122–2146,2184–2217`, `app/docs/BACKLOG.md:1635`, `app/chamados/services/acoes_disputa.py:274–339`.

## 27/11 23:59 · sessao ff179defc550 · - · - · origem=Ronald (tmux)

Para col935, disputa 8272, filho 40002 em 18/11, e col976, disputa 7385, filho 34061 em 22/10, os quatro pares têm `dia_furado_para_a_folha`. As perguntas individuais, o fato do dia e o módulo dos filhos estão [nao verificado]. `lista_por_par.md:9–10` do anexo.

- **Quem decide.** `ChamadoColaborador._decisao_do_fechamento` pergunta ao motor `RECONCILIAR`, depois à premissa, ao CSF e à pergunta viva. `_gate_premissa_do_fechamento` consulta o dia forte do filho, `premissa_regularizada(para='folha')`, pai agrupador vivo e o sobrevivente C8. O coletor lê justificativa aprovada, lavra administrativa, dia sem trabalho, lâmpadas e aba de faltas da célula. O TXT tem leitor próprio da lavra e dos protestos. `app/chamados/models.py:683–712,783–837,1332–1382`, `app/chamados/juizes.py:508–567`, `app/folha/export.py:1185–1222,1282–1305`.
- **Porta por dia, quando houver fato confirmado.** Admin com `colaboradores.rbac_regularizar_batida` ou superusuário usa **Resolver dia** no calendário individual. O botão exige dia passado de competência anterior e status elegível, e a tela considera se existe pergunta C6-viva com resposta. O serviço seleciona perguntas de fato C6-vivas do mesmo colaborador e dia, inclusive em disputa fechada, grava via `admin_decidiu` e ato `SILENCIO` com autor, instante, texto anterior e razão fixa. Registra ações na trilha do despachante e antes/depois na lavra. `app/colaboradores/mixins.py:322–331`, `app/colaboradores/services/calendario.py:94–104,117–124,493–508,774`, `app/templates/colaboradores/partials/_calendario_grade.html:134–135`, `app/ponto/views.py:3517–3556`, `app/ponto/services/veredito_celula.py:180–213,351–364`, `app/ponto/portas/celula.py:1082–1083,1130–1141`.
- **Efeitos e reaplicação.** `Trabalhou` pode inverter ou materializar batida e recalcular o dinheiro gravado após commit. `Faltou` e `Justificado` criam ausência, com batida **não**, dinheiro gravado **não** no ato e possível invalidação da projeção. Célula **sim**, se existente, e escala/vínculo **não** nesses três ramos. Carimbo individual e lavra repetida são idempotentes. O despachante registra outra trilha quando repetido com sucesso e a sobreposição de ausência pode recusar. `app/ponto/services/veredito_celula.py:216–218,317–364`, `app/ponto/registro_batida.py:174–198`, `app/ponto/services/ausencia.py:465–471,581–599`, `app/ponto/services/fechamento.py:1640–1684,1822–1848`, `app/chamados/models.py:3372–3386`, `app/ponto/portas/celula.py:1113–1116`.
- **Resultado garantido pelo código sob condição.** A lavra via `admin` libera a cláusula de premissa e os três vereditos administrativos ficam fora de `VEREDITOS_ACUSAM`. O despachante tenta reconciliar antes da lavra, então o filho ainda pode recusar nessa tentativa. A lavra emite evento e o consumidor reconcilia os módulos elegíveis, com outras guardas ainda aplicáveis. A disponibilidade do botão, a escolha do veredito e a baixa desses filhos estão [nao verificado]. Sem veredito factual confirmado, **SEM PORTA** para baixar só a pergunta e eliminar `dia_furado_para_a_folha`. `app/chamados/models.py:1345–1349`, `app/escala/models.py:1772–1781`, `app/ponto/services/veredito_celula.py:204–212,351–357`, `app/ponto/portas/celula.py:977–1002,1130–1135`, `app/chamados/reconciliador.py:1118–1131,1513–1533`.
- **Resposta sem reabrir.** POST web/API confere dono autenticado e C6, inclusive para disputa fechada. O Meu Atendimento web exclui fechadas da montagem, e o app envia fechadas ao histórico, fora de pendentes. O admin pode validar uma resposta de fechada no fio sob a âncora correta, com `sem_reabrir=True`. `Regularizar disputa` exige disputa aberta e recusa pergunta muda. `admin_encerra` exige motivo mas carece de chamador na UI, e `fechar` retorna cedo ou levanta quando a disputa já está fechada. `app/chamados/views.py:466–475,560–570,1228–1247,1275–1279`, `app/api/views.py:2607–2644,2652–2664`, `app/chamados/services/pergunta_no_app.py:80–92`, `app/chamados/services/fio.py:287–309,694–709,721–768`, `app/chamados/services/validacao.py:755–765`, `app/chamados/services/acoes_disputa.py:156–178`, `app/chamados/models.py:2094–2100,2210–2217`, `app/docs/BACKLOG.md:1635`.
- **Comandos.** `reconciliar_perguntas_orfas` só mede e seu escritor real seleciona disputa aberta e dois motivos. `processar_cartorio --apply` rejulga por empresa em todas as datas anteriores, sem filtro dos IDs. `consumir_eventos_celula` é consumidor de eventos no tenant inteiro. `app/ponto/management/commands/reconciliar_perguntas_orfas.py:20–36`, `app/chamados/reconciliador.py:1162–1175`, `app/ponto/management/commands/processar_cartorio.py:25–29,45–65`, `app/chamados/management/commands/consumir_eventos_celula.py:13–35`.

## 27/11 23:58 · sessao 3f74ace4fec9 · FERIADO-UM-JUIZ · construtor · origem=Code

**c14 entregue** com alterações apenas nos dois arquivos de teste autorizados. [nao verificado]

- `test_feriado_um_juiz.py` (`+27 / −6`) exige totais exatos de `0` e `2`. O segundo caso injeta a falha de alimentação em dois colaboradores na mesma passada, com `propagar_falha=False`. O ERROR técnico exige o prefixo literal `dia_pago oraculo:` e o ID do colaborador. [Testes do resumo](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:462) · [Asserção do ERROR](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:505) [nao verificado]
- `feriado_um_juiz_mutantes.py` (`+40 / −9`) registra os três mutantes e exige que falhem exatamente os testes listados. **Diff com `0` `except` e `0` `try` adicionados ou removidos.** [Registro dos mutantes](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/feriado_um_juiz_mutantes.py:189) · [Verificação dos alvos](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/feriado_um_juiz_mutantes.py:802) [nao verificado]

| Mutante | Testes que falharam | [nao verificado]
|---|---|
| `M_c14_total_fixo_1` | `test_RED_c14_resumo_publica_zero_falhas_tecnicas` e `test_RED_c14_resumo_soma_duas_falhas_tecnicas_na_mesma_passada`. Ambos receberam `1`. [Alvos](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:462) |
| `M_c14_sem_prefixo` | `test_RED_c10_falha_sql_da_alimentacao_reverte_o_savepoint` e `test_RED_c13_value_error_generico_na_alimentacao_e_falha_tecnica`. Ambos falharam na asserção do prefixo. [Alvos](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:481) |
| `M_c14_engole_alimentacao` | `test_RED_c13_falha_na_alimentacao_propaga_quando_solicitado`. O `RuntimeError` esperado não foi lançado. [Alvo](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:507) |

Os três comandos retornaram `0` após as falhas previstas. O controle local sem banco rodou 58 testes, com 57 aprovados e uma falha esperada. [Executor](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/feriado_um_juiz_mutantes.py:776) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.feriado_um_juiz_mutantes M_c14_total_fixo_1
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.feriado_um_juiz_mutantes M_c14_sem_prefixo
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.feriado_um_juiz_mutantes M_c14_engole_alimentacao
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.feriado_um_juiz_mutantes controle
```

Certificação pelo Code e pelo Opus final [nao verificado].

## 27/11 23:58 · sessao b0324e92ec8c · - · - · origem=Ronald (tmux)

A pergunta viva de disputa fechada tem duas saídas sem reabrir. O colaborador autenticado consegue responder pelo POST próprio do PWA ou do app, desde que o C6 ainda a considere viva. O app a coloca no histórico, e o fio web do colaborador exclui as fechadas. O teste confirma que o POST aceita a pergunta do histórico. O alcance por uma tela e uma credencial reais nos cinco casos fica [nao verificado]. O registro `DecisaoPergunta.RESPOSTA` guarda autor, texto e instante, preservando os atos anteriores. Novo envio pode gravar outra resposta. Uma resposta de hora normalmente passa à validação e pode seguir viva. `FALTA_DIA` e `ATESTADO` podem criar ausência e repercutir em célula e fechamento. A resposta por si não planta batida nem altera vínculo. `app/chamados/views.py:1228`, `app/chamados/views.py:1275`, `app/api/views.py:2607`, `app/api/tests/test_hx_api_estado.py:54`, `app/chamados/services/pergunta_no_app.py:80`, `app/chamados/services/fio.py:287`, `app/chamados/services/disputa_emissao.py:1530`, `app/chamados/services/disputa_emissao.py:1562`, `app/chamados/services/disputa_emissao.py:1591`, `app/chamados/services/materializacao.py:1010`, `app/chamados/juizes.py:1521`.

O Fio do gestor aceita **Validar** por pergunta respondida, no cartão do filho que a lista, com verificação de permissão contextual e do C6 sob trava. Passa `sem_reabrir=True`; sem resposta íntegra retorna `sem_resposta`. A UI condiciona o formulário à resposta e ao estado de aplicação. A validação persistida é idempotente por estado, grava autor, instante e texto em `DecisaoPergunta.VALIDACAO`, `val_perg_disp` e o log específico `validar_viva_em_disputa_fechada`. O motivo da pergunta aparece no log e a eventual devolução tem antes/depois. Pode plantar batida, repercutir na célula, lançar FolgaDia no aceite de folga e recalcular valores gravados em `FechamentoMensal`. O laço pode alcançar irmãs validadas. `app/chamados/views.py:869`, `app/chamados/views.py:952`, `app/chamados/utils.py:47`, `app/chamados/services/fio.py:694`, `app/chamados/services/fio.py:721`, `app/templates/chamados/partials/modal_fio.html:217`, `app/templates/chamados/partials/modal_fio.html:273`, `app/chamados/services/validacao.py:164`, `app/chamados/services/validacao.py:332`, `app/chamados/services/validacao.py:755`, `app/chamados/services/validacao.py:777`, `app/chamados/services/validacao.py:796`, `app/chamados/services/validacao.py:839`, `app/chamados/services/validacao.py:587`, `app/chamados/services/materializacao.py:2240`, `app/chamados/services/materializacao.py:2273`, `app/ponto/services/fechamento.py:1161`, `app/ponto/services/fechamento.py:1822`.

**Resolver dia** pode encerrar a pergunta muda sem abrir a disputa quando há um veredito factual para o dia. Seleciona perguntas de fato, pelo mesmo colaborador e `data_evento`, sem filtro de disputa aberta, reavalia o C6 e grava `admin_decidiu` e `SILENCIO` com ator e motivo. Em seguida tenta reconciliar os chamados. A célula recebe lavra com antes/depois e autor; `trabalhou` pode retipar ou plantar batidas, `faltou` ou `justificado` podem criar ausência. Escala/vínculo é insumo; valores derivados podem mudar por batida/ausência. O carimbo e a lavra têm guardas idempotentes, mas o log do despachante volta a ser gravado quando um novo POST `trabalhou` passa. A UI exige `regularizar_batida` e só mostra o controle para dia passado, último recurso e status elegível. Veredito, oferta do botão e toda condição de efeito nos casos exatos ficam [nao verificado]. `app/ponto/views.py:3517`, `app/ponto/services/veredito_celula.py:176`, `app/ponto/services/veredito_celula.py:207`, `app/ponto/services/veredito_celula.py:318`, `app/ponto/services/veredito_celula.py:336`, `app/ponto/services/veredito_celula.py:351`, `app/ponto/portas/celula.py:1113`, `app/ponto/portas/celula.py:1130`, `app/chamados/models.py:3372`, `app/chamados/juizes.py:1516`, `app/templates/colaboradores/partials/_calendario_grade.html:134`, `app/colaboradores/services/calendario.py:94`.

A reabertura administrativa existente é no cartão **pai**, para staff ou `admin_ti` com permissão contextual. Escolhe a fechada mais recente daquele pai, exige motivo do catálogo, justificativa de pelo menos 20 caracteres e teto de duas. Limpa fechamento, cria pergunta nova de confirmação, tenta reabrir o pai terminal, grava nota com justificativa e log do autor/motivo/contador. A seleção por pai impede tratar o POST como idempotente por ID de disputa. Refechar continua sujeito ao C7, que preserva vivas e respostas sem veredito. A exceção de modelo `admin_encerra` encontra a guarda de já fechada antes do carimbo e ficou sem caller de produção localizado. A expiração por silêncio saiu do comando de arquivamento. `app/templates/chamados/partials/modal_fio.html:411`, `app/chamados/services/fio.py:160`, `app/chamados/services/fio.py:694`, `app/chamados/services/acoes_disputa.py:274`, `app/chamados/services/acoes_disputa.py:303`, `app/chamados/services/acoes_disputa.py:338`, `app/chamados/models.py:2047`, `app/chamados/models.py:2094`, `app/chamados/models.py:2184`, `app/chamados/models.py:2210`, `app/chamados/models.py:2396`, `app/chamados/management/commands/arquivar_competencia_encerrada.py:61`.

Para os sete pares, **SEM PORTA administrativa avulsa para expirar pelo silêncio**. A recomendação condicional é a UI **Resolver dia** por dia individual com fato comprovado, ou confirmação do alcance real da resposta do próprio colaborador quando o responsável ainda precisa dizer o fato. Nos quatro pares `dia_furado_para_a_folha`, é indispensável medir novamente o veto da premissa e a retenção real da célula, além do C6. O desaparecimento de um par na S4 pode decorrer da exclusão por status de chamado vivo; a folha lê `classificar_export` e `motivos_retencao_celula`. `app/ponto/services/veredito_celula.py:176`, `app/chamados/views.py:1228`, `app/chamados/models.py:783`, `app/folha/fora_da_folha.py:115`, `app/folha/fora_da_folha.py:125`, `app/folha/export.py:1282`.

## 27/11 23:54 · sessao 2133c5a2fd15 · FERIADO-UM-JUIZ · verificador · origem=Code

**REPROVADO.** A c13 cura o bloqueio 1 da ver8 no tratador do fechamento. O bloqueio 2 persiste: um dia válido com 420 min de saída antecipada deixa a sombra inteira na lavra anterior. A prova da nova telemetria também tem uma lacuna demonstrada: fixar o total de falhas técnicas em `1` no resumo passa pelos seis alvos c13 e pelo controle completo. `app/ponto/services/fechamento.py:1320` `app/ponto/tests/test_feriado_um_juiz.py:384` `app/ponto/tests/feriado_um_juiz_mutantes.py:168` `app/ponto/tests/feriado_um_juiz_mutantes.py:753`

## 1. Bloqueio 1 e demais achados da ver8

**Bloqueio 1 curado.** A guarda lança `RecusaDeProcedencia` para campos sem dono declarado. Só esse tipo incrementa `recusas_oraculo`. As demais exceções que chegam ao tratador incrementam `falhas_tecnicas_oraculo` e emitem erro próprio. Com a flag ligada, a falha técnica é encadeada como causa de `RuntimeError`. A recusa de procedência ainda permite retornar `1` quando o motor gravou. `app/ponto/services/dia_pago.py:135` `app/ponto/services/dia_pago.py:321` `app/ponto/services/fechamento.py:1320` `app/ponto/services/fechamento.py:1339` `app/ponto/tests/test_feriado_um_juiz.py:493`

- **Bloqueio 2 aberto.** Reproduzi em memória `06:00 E / 06:30 S`, escala `07:00–15:00`. O mensal e o `motor` ficam com zero trabalhado e 420 min de antecipada. O oráculo registra uma recusa e conserva a versão anterior. O dia fechado de 7 h, isolado, grava as duas versões. Junto ao BUG-144, o `motor` grava 5,95 h e 7 h, o mensal fica em 12,95 h e a sombra inteira conserva a lavra anterior. `app/ponto/tests/test_feriado_um_juiz.py:255` `app/ponto/tests/test_feriado_um_juiz.py:350` `app/ponto/tests/test_feriado_um_juiz.py:384` `app/ponto/tests/test_feriado_um_juiz.py:498` `app/ponto/services/dia_pago.py:329`
- **Ver7 e Opus.** Os 357 min do BUG-144 continuam preservados no mensal e no `motor`. A exigência por dia segue baseada em `CAMPOS_DIA`, com acesso direto à foto dos períodos quando falta a chave em `rubricas`. `do_motor` chega à guarda nos dois casos, e a foto inclui os três tipos de FT. Reexecutei os mutantes c10 e os dois c11 desses pontos, todos com a falha esperada. `app/ponto/tests/test_feriado_um_juiz.py:339` `app/ponto/services/dia_pago.py:228` `app/ponto/services/dia_pago.py:238` `app/ponto/services/dia_pago.py:252` `app/ponto/services/dia_pago.py:262` `app/ponto/services/dia_pago.py:325` `app/ponto/tests/feriado_um_juiz_mutantes.py:123` `app/ponto/tests/feriado_um_juiz_mutantes.py:206`
- **C12.** O registro de linhas B1 passou na catraca. Persiste o ponto cego da ver8: duas ocorrências na mesma linha viram uma entrada no registro. `app/core/tests/test_medida_turno_do_par.py:332` `app/core/tests/test_medida_turno_do_par.py:565` `app/core/medida_turno_do_par.py:217` `app/core/medida_turno_do_par.py:228`

## 2. Quem escreve e quem lê

| Item | Escritores, leitores e significado |
|---|---|
| `RecusaDeProcedencia` | A classe herda de `ValueError`, e a guarda de `lavrar` é o único `raise` desse tipo na cópia. O fechamento importa e testa o tipo no tratador da sombra. Os leitores legados localizados são testes que capturam `ValueError` e conferem a mensagem ou os campos. O116 também captura `ValueError` no caso com todos os donos para excluir a mensagem da guarda. `app/ponto/services/dia_pago.py:135` `app/ponto/services/dia_pago.py:329` `app/ponto/services/fechamento.py:1236` `app/ponto/services/fechamento.py:1321` `app/ponto/tests/test_feriado_um_juiz.py:380` `app/ponto/tests/test_feriado_um_juiz.py:410` `app/ponto/tests/test_feriado_um_juiz.py:431` `app/ponto/tests/test_o116_lavratura_le_o_dia_do_motor.py:223` `app/ponto/tests/test_o116_lavratura_le_o_dia_do_motor.py:246` |
| `recusas_oraculo` | O fechamento zera por chamada e incrementa uma vez por colaborador cuja tentativa termina em `RecusaDeProcedencia`. Lê o acumulado no `ERROR` como `total_recusas` e no resumo `INFO`. Os testes leem o erro individual e extraem o total do resumo por regex. O vigia do harness também usa a chave para localizar o resumo. `app/ponto/services/fechamento.py:683` `app/ponto/services/fechamento.py:1321` `app/ponto/services/fechamento.py:1347` `app/ponto/tests/test_feriado_um_juiz.py:321` `app/ponto/tests/test_feriado_um_juiz.py:337` `app/ponto/tests/feriado_um_juiz_mutantes.py:176` |
| `falhas_tecnicas_oraculo` | O fechamento zera por chamada, incrementa no outro ramo do tratador e lê o total no resumo. O vigia do harness lê `falhas_tecnicas_oraculo=1`. Na chamada que relança o erro, há `ERROR` individual e a execução sai antes do resumo. `app/ponto/services/fechamento.py:683` `app/ponto/services/fechamento.py:1327` `app/ponto/services/fechamento.py:1338` `app/ponto/services/fechamento.py:1347` `app/ponto/tests/feriado_um_juiz_mutantes.py:168` |
| `falha_lavra` | É local ao colaborador. Recebe `None` no início, recebe o erro do `motor` depois de marcar rollback, ou recebe o erro técnico da sombra com a flag ligada. O tratador da sombra lê `is None` antes de atribuir. Ao sair do `atomic`, o fechamento usa a variável para contar `processados` ou relançar. Portanto `None` ainda admite falha técnica da sombra no modo padrão. `app/ponto/services/fechamento.py:928` `app/ponto/services/fechamento.py:1274` `app/ponto/services/fechamento.py:1331` |
| `propagar_falha` | A assinatura fornece `False`. O único chamador de produção localizado que fornece `True` é `recalcular_na_revisao`. As leituras são a atribuição da falha técnica da sombra, o relançamento após o `atomic` e o encadeamento no `except` externo. O teste da revisão verifica a flag nas duas competências. Os testes c13 passam `True` diretamente para falha de alimentação e recusa. `app/ponto/services/fechamento.py:513` `app/ponto/services/fechamento.py:1331` `app/ponto/services/fechamento.py:1338` `app/ponto/services/fechamento.py:1893` `app/ponto/tests/test_revisar_par_relampago.py:163` `app/ponto/tests/test_feriado_um_juiz.py:486` |
| `dia_pago oraculo` | O fechamento escreve três formatos sob o prefixo. `lavratura falhou` agora significa recusa de procedência. `falha tecnica` é o erro técnico individual. O `INFO` publica os dois totais. O teste da recusa lê trechos do erro e usa regex no resumo. O teste técnico usa regex para a expressão “falha técnica”. Os testes de escrita, incluindo o ORM, capturam o logger e conferem o motivo injetado. A configuração SaaS encaminha `INFO` e `ERROR` a `ponto.log`, e `ERROR` também ao console. `app/ponto/services/fechamento.py:18` `app/ponto/services/fechamento.py:1323` `app/ponto/services/fechamento.py:1328` `app/ponto/services/fechamento.py:1347` `app/ponto/tests/test_feriado_um_juiz.py:117` `app/ponto/tests/test_feriado_um_juiz.py:321` `app/ponto/tests/test_feriado_um_juiz.py:480` `app/ponto/tests/test_feriado_um_juiz_porta.py:496` `app/config/settings/saas.py:130` `app/config/settings/saas.py:160` `app/config/settings/saas.py:181` |

A tela e o comando de recálculo leem `processados`. O backfill lê o dicionário retornado e conta seus colaboradores. `diff_calculador` lê `Rubricas` diretamente. O cron `pre_fechamento` lê bloqueios e marcos. O vigia do `ESTADO` e a API do HAIKU leem `ponto.log` pelo parser de `ponto.recusa_critica`, cujo regex exige `RECUSA-ACAO-CRITICA`. Leitor automatizado dos três formatos `dia_pago oraculo` em produção **[nao verificado]**. `app/ponto/views.py:1280` `app/ponto/management/commands/recalcular_fechamento.py:130` `app/ponto/management/commands/lavrar_dias_pagos.py:104` `app/ponto/management/commands/diff_calculador.py:406` `app/config/crons.py:406` `app/config/crons.py:1153` `app/folha/management/commands/pre_fechamento.py:41` `app/folha/management/commands/pre_fechamento.py:69` `bin/gerar_estado.py:265` `bin/recusa_estado.py:45` `bin/recusa_estado.py:68` `app/api/views_mensageria.py:1872` `app/core/recusa_critica.py:46`

## 3. Outros `ValueError`

Na cadeia de `lavrar`, **nenhum outro `raise ValueError` explícito de procedência** foi localizado. A guarda é o único `raise` explícito no corpo e nos auxiliares locais. O `raise ValueError` de `apurar_furo_da_exportada` exige que o chamador forneça a apuração do furo e pertence a outra porta. `app/ponto/services/dia_pago.py:50` `app/ponto/services/dia_pago.py:109` `app/ponto/services/dia_pago.py:139` `app/ponto/services/dia_pago.py:329` `app/ponto/services/dia_pago.py:653`

- **Na lavra**, os pontos de falha de valor incluem conversões numéricas de grade, rubricas e ajustes, desempacotamento de FT e a chamada de `timezone.localtime` no fallback de dia. São operações de entrada ou escrita. Se a exceção escapar da execução da sombra, cai como técnica. Tipos exatos de exceções de Django e do banco fora da cópia **[nao verificado]**. `app/ponto/services/dia_pago.py:132` `app/ponto/services/dia_pago.py:238` `app/ponto/services/dia_pago.py:300` `app/ponto/services/dia_pago.py:339` `app/ponto/services/dia_pago.py:360` `app/ponto/services/dia_pago.py:370` `app/ponto/services/fechamento.py:1326`
- **Na alimentação**, datas ISO, conversões numéricas e dados de pontualidade podem falhar antes de `lavrar`. A fábrica de motor tem dois casos explícitos, régua inválida e `CicloSemMotor`, subclasse de `ValueError`. Ambos são problemas de configuração. O `except` preexistente em `rubricas_por_dia` captura erros da fábrica e segue com `_mcct=None`. Reproduzi uma injeção de `ValueError` ali em um dia fechado: gravaram as duas versões, com `recusas_oraculo=0` e `falhas_tecnicas_oraculo=0`. A c13 classifica as exceções que **chegam** ao tratador, e a alimentação já pode absorvê-las antes. `app/ponto/calculador/alimentacao.py:429` `app/ponto/calculador/alimentacao.py:512` `app/ponto/calculador/alimentacao.py:524` `app/ponto/calculador/alimentacao.py:568` `app/ponto/calculador/regras.py:707` `app/core/regua_cct.py:457` `app/core/regua_cct.py:479` `app/ponto/motor_calculo_v2.py:769` `app/ponto/motor_calculo_v2.py:3701` `app/ponto/motor_calculo_v2.py:3769`

## 4. Onde muda o que fica gravado

**Muda na restauração administrativa de batida retratada pelo cron.** O POST `decidir_par_relampago` seleciona `restaurar_retratacao_cron`, que abre o `atomic` do ato. `restaurar_batida` anota a batida e a trilha e chama `recalcular_na_revisao`. Este roda uma vez por competência elegível do dia civil e do dia de turno, pula as entregues por K6 e passa `propagar_falha=True`. A sombra é tentada a partir de 10/2026, após sucesso do `motor`. `app/ponto/urls.py:63` `app/ponto/views.py:3486` `app/ponto/views.py:3497` `app/ponto/services/par_relampago_na_lista.py:479` `app/ponto/services/par_relampago_na_lista.py:492` `app/ponto/registro_batida.py:283` `app/ponto/registro_batida.py:317` `app/ponto/services/fechamento.py:1888` `app/ponto/services/fechamento.py:1284`

O savepoint da sombra desfaz a sombra. O `atomic` interno do colaborador já saiu quando o `raise` é executado. **Quem desfaz `FechamentoMensal`, `DiaPago motor`, batida e trilha é o `atomic` externo da restauração**, inclusive se outra competência já tiver sido recalculada no mesmo ato. Antes da c13, a falha técnica da sombra deixava a revisão gravar. Em chamada direta com `True` e sem transação externa, reproduzi o `RuntimeError` com o mensal e o `motor` novos ainda gravados na simulação. `app/ponto/services/fechamento.py:930` `app/ponto/services/fechamento.py:1181` `app/ponto/services/fechamento.py:1197` `app/ponto/services/fechamento.py:1287` `app/ponto/services/fechamento.py:1331` `app/ponto/services/par_relampago_na_lista.py:479` `app/ponto/tests/test_feriado_um_juiz.py:129` `app/ponto/tests/test_feriado_um_juiz.py:149`

Tela, comando e recálculo por evento usam o padrão `False`, que preserva mensal e `motor` diante de falha técnica da sombra. O backfill usa o mesmo padrão e grava `DiaPago` em modo de leitura do mensal. Incidência, valores afetados, DIFF de frota e aval para o novo efeito na revisão **[nao verificado]**. `app/ponto/views.py:1280` `app/ponto/management/commands/recalcular_fechamento.py:130` `app/ponto/services/fechamento.py:1850` `app/ponto/management/commands/lavrar_dias_pagos.py:106` `app/ponto/tests/test_feriado_um_juiz_porta.py:498` `app/docs/LEIS.md:112`

## 5. Mordida dos REDs

**Os seis mutantes c13 morreram na execução local.** Eles cobrem alargar a recusa para `ValueError`, suprimir a propagação na escrita, lançar o tipo genérico na guarda, retirar a herança de `ValueError`, trocar o rótulo do erro técnico e retirar seu incremento. M4 cobre propagar indevidamente uma recusa, M5 cobre tirar a contagem de recusas e M2_savepoint cobre retirar o savepoint da alimentação. Reconstituí em memória os dois blobs de produção da base do anexo e confirmei que nela falham os testes de classificação técnica, propagação e resumo. `app/ponto/tests/feriado_um_juiz_mutantes.py:123` `app/ponto/tests/feriado_um_juiz_mutantes.py:184` `app/ponto/tests/feriado_um_juiz_mutantes.py:535` `app/ponto/tests/feriado_um_juiz_mutantes.py:758` `app/ponto/tests/test_feriado_um_juiz.py:460`

**Há lacunas.** Propagar só erros de escrita e engolir erros de alimentação passa pelos seis alvos c13. O RED de alimentação com `True` derruba essa alteração no controle, mas ficou sem mutante dedicado. Fixar o resumo técnico em `1` ou tirar `dia_pago oraculo:` do `ERROR` técnico passa também pelo controle de 56 testes. O controle só exige o total técnico positivo em um cenário, e a regex do erro técnico só exige a expressão “falha técnica”. Já o ramo falso de `falha_lavra is None` é inalcançável ali: a sombra está no `else` da lavra `motor`. `app/ponto/tests/test_feriado_um_juiz.py:480` `app/ponto/tests/test_feriado_um_juiz.py:486` `app/ponto/tests/feriado_um_juiz_mutantes.py:168` `app/ponto/tests/feriado_um_juiz_mutantes.py:189` `app/ponto/tests/feriado_um_juiz_mutantes.py:753` `app/ponto/services/fechamento.py:1274` `app/ponto/services/fechamento.py:1331`

## 6. Classes de reprovação e limites da prova

- **Juiz, escritor e derivação.** O delta reutiliza a partição existente e a mesma `lavrar` para as duas versões. O retrato da exportada é uma porta preexistente. Persiste a divergência de chave da ver8: períodos, FT e alimentação usam o dia da jornada quando há mapa, enquanto o atraso de ligação e o O85 usam data civil da entrada. Efeito dessa divergência em produção **[nao verificado]**. `app/ponto/services/fechamento.py:905` `app/ponto/services/fechamento.py:997` `app/ponto/services/fechamento.py:1237` `app/ponto/services/fechamento.py:1296` `app/ponto/services/fechamento.py:1300` `app/ponto/services/dia_pago.py:109` `app/ponto/services/dia_pago.py:370` `app/ponto/services/dia_pago.py:417` `app/ponto/services/credito_parcial.py:73` `app/ponto/calculador/alimentacao.py:73`
- **Relógio e trava.** O teto do furo usa um instante lido na borda da passada e a mesma lista nas duas lavras. A revisão mantém Empresa por PK → Colaborador → Batida por PK, antes do recálculo e do savepoint. `app/ponto/services/fechamento.py:680` `app/ponto/services/fechamento.py:1152` `app/ponto/services/fechamento.py:1262` `app/ponto/services/fechamento.py:1316` `app/ponto/services/par_relampago_na_lista.py:172` `app/ponto/services/par_relampago_na_lista.py:402` `app/ponto/services/par_relampago_na_lista.py:483`
- **Campo com dois significados.** `nao_decide` continua significando rubrica sem afirmação no DIFF e suficiência para declarar dono na guarda. Reproduzi a chamada direta com `horas_falta` só em `NAO_DECIDE` e sem `faltas_por_dia`: gravou `0.0` com a chave em `calculador_nao_decide`. O fechamento fornece o mapa de faltas. `app/ponto/calculador/regras.py:532` `app/ponto/management/commands/diff_calculador.py:508` `app/ponto/services/dia_pago.py:59` `app/ponto/services/dia_pago.py:172` `app/ponto/services/dia_pago.py:340` `app/ponto/services/fechamento.py:946` `app/ponto/services/fechamento.py:1304`
- **Selo e `except`.** O delta adiciona **0** cláusulas `except` e remove **0**. Na AST dos blobs de produção, `dia_pago.py` ficou em 1 e `fechamento.py` em 9. Rodei 56 controles com uma falha esperada e 41 estruturais verdes. A falha esperada local é a borda 20/21 com troca de 6x1 para 12x36. Os quatro blobs de saída coincidem com o anexo. A identidade do commit inteiro, os 100 testes da pista e a execução ORM com banco ficam **[nao verificado]** nesta cópia. `app/ponto/services/dia_pago.py:469` `app/ponto/services/fechamento.py:1320` `app/ponto/tests/feriado_um_juiz_mutantes.py:535` `app/ponto/tests/feriado_um_juiz_mutantes.py:584` `app/ponto/tests/feriado_um_juiz_mutantes.py:753` `app/ponto/tests/test_feriado_um_juiz.py:965`

_8830 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
