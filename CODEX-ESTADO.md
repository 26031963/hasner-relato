# CODEX-ESTADO

> Ultima resposta processada: **18/11/2026 09:10** · publicado em **18/11/2026 09:11** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 18/11 09:10 · sessao 58e2cbee3fbf · ESPURIA-TELA · censo · origem=Code

O consumo direto identificado de `do_dia` lê as três chaves pelo nome; não encontrei nesse consumo direto comparação, iteração ou serialização do dict inteiro [app/ponto/services/espelho.py:340, app/colaboradores/services/calendario.py:748, app/ponto/tests/test_palavra_parcial_e_pendente.py:74]. Há, porém, **um teste que serializa e compara o espelho inteiro**, incluindo seus dicts de dia: uma chave adicional pode fazê-lo falhar se tiver valores diferentes nas duas montagens [app/api/tests/test_atestado_por_minutos.py:104, app/api/tests/test_atestado_por_minutos.py:126, app/relatorios/pdf_espelho.py:602].

A coluna “quebra” abaixo considera **acrescentar uma chave sem alterar as chaves e valores existentes**. “Condicional” significa que a adição não basta, por si só, para provocar falha.

### Retorno direto de `do_dia`

| Chamador — arquivo:linha | Consumo | Quebra com chave nova? |
|---|---|---|
| Produção: `app/ponto/services/espelho.py:340` | `_r`; lê `veredito`, `palavra` e `cor` individualmente em `:344–345`. | Não: seleciona três nomes. |
| Produção: `app/colaboradores/services/calendario.py:748` | Import local com apelido `_do_dia` em `:738`; `_dd` fornece as três chaves individualmente em `:757`. | Não: seleciona três nomes. |
| Teste: `app/ponto/tests/test_palavra_parcial_e_pendente.py:74` | Atribui a `r` e lê valores por chave em `:76–77`. | Não: acessos nominais. |
| Teste: `app/ponto/tests/test_palavra_parcial_e_pendente.py:82` e `:83` | Atribui a `sem` e `com`; compara valores individuais em `:85–88`. | Não: acessos nominais. |
| Teste: `app/ponto/tests/test_palavra_parcial_e_pendente.py:92` | Atribui a `r`; lê valores por chave em `:94–95`. | Não: acessos nominais. |
| Teste: `app/ponto/tests/test_palavra_parcial_e_pendente.py:98` | Compara uma **tupla de três valores**, em `:99`, não o dict. | Não: a tupla seleciona nomes. |

O espelho grava as três chaves derivadas em cada dia em `app/ponto/services/espelho.py:344–345`; seu placeholder está em `:895`. O calendário constrói outro dict de dia com essas chaves em `app/colaboradores/services/calendario.py:754–757`.

### Leitores das chaves derivadas e do dia inteiro

| Leitor — arquivo:linha | Consumo | Quebra com chave nova no dict de dia? |
|---|---|---|
| API: `app/api/views.py:1388–1395`, `:1533–1543`, `:1595–1603` | Lê três campos do dia do espelho e monta dicts novos para o payload. Não espalha o dia original nesses pontos. | Não nesses pontos: seleciona campos por nome. |
| PDF: `app/relatorios/pdf_espelho.py:602`, `:636–645`, `:923–924` | Preserva os dicts de dia na lista coletada e lê `palavra_dia` por nome ao desenhar. | Não na leitura da palavra; o coletor **transporta** a chave adicional. |
| Tela do espelho: `app/ponto/views.py:160–162`, `:196`; `app/templates/ponto/espelho.html:151`, `:189`, `:298` | Repassa dias ao contexto e o template lê campos e listas internas. | Não nos acessos examinados: não exige conjunto exato de chaves. |
| Grade: `app/templates/colaboradores/partials/_calendario_grade.html:67–68`, `:97–98`, `:127`, `:135` | Itera a lista de dias e listas internas; lê os três campos por nome. | Não nesses acessos. |
| Comandos: `app/ponto/management/commands/e6_oraculo.py:404`; `app/ponto/management/commands/e6_piso_autopsia.py:145` | `.get('veredito_dia')`. | Não: acesso nominal. |
| Teste: `app/api/tests/test_atestado_por_minutos.py:104–106`, `:117`, `:126` | Serializa com `json.dumps(..., default=str, sort_keys=True)` o resultado do coletor, que inclui dias inteiros; compara as strings. | **Condicional**: valores novos diferentes entre as montagens alteram a igualdade; valores iguais não. |
| Teste: `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:104`, `:116`, `:119–120` | Passa o dia inteiro ao template e faz `dict(dia, status_rotulo=...)`. | Não por adicionar chave: a cópia a preserva. |
| Testes: `app/api/tests/test_o21_app_diz_o_que_foi_decidido.py:92`, `:106`, `:138`, `:170`, `:180` | Leem e comparam campos nomeados ou tuplas desses valores. | Não: não comparam o dict inteiro nesses pontos. |
| Testes: `app/colaboradores/tests/test_calendario_mostra_o_carro.py:81`, `:140`, `:146`, `:164`; `app/ponto/tests/test_saida_orfa_dia_em_aberto.py:103` | Leem campos nomeados. | Não nesses acessos. |
| Testes: `app/relatorios/test_espelho_ausencia_pela_autoridade.py:54`, `:62`; `app/relatorios/tests/test_rotulo_ausencia_pdf.py:31` | Leem `palavra_dia` por nome. | Não nesses acessos. |
| Testes: `app/relatorios/tests/test_o197_futuro_nao_e_em_aberto.py:214`, `:243`, `:303`; `app/relatorios/tests/test_r3_em_aberto_alcanca_turno_aberto.py:202`, `:277`, `:300` | Leem campos nomeados; `:214` itera o mapa de dias, não as chaves de cada dia. | Não nesses acessos. |
| Testes: `app/relatorios/tests/test_palavra_do_dia.py:131–141`, `:161–191`, `:201–214`, `:234–252`, `:266–274`, `:290–304`, `:371–401` | Leem e comparam valores das três chaves por nome. | Não nesses acessos. |

Não foi identificada iteração Django da forma `dia.items`, `dia.keys` ou `dia.values`; a grade itera `dias` em `app/templates/colaboradores/partials/_calendario_grade.html:67`. A ausência de iteração equivalente com apelidos arbitrários em todo JavaScript, inclusive código de terceiros, permanece **[não verificado]**. O `ausencia_id` encontrado nos templates é lido de outros objetos, por exemplo `h` em `app/templates/ponto/partials/painel_ausencia.html:191` e contexto de chamado em `app/templates/chamados/partials/_card_atendimento.html:70`.

A existência de outros caminhos indiretos que comparem o conjunto exato de chaves, serializem em cache ou pickle, ou recebam o dia por aliases não rastreados permanece **[não verificado]**. Em particular, a passagem do dia inteiro torna insuficiente concluir que todo consumidor posterior ignora novas chaves a partir apenas dos acessos nominais acima [app/relatorios/pdf_espelho.py:602, app/ponto/views.py:196].

### Colisões de nome

- Nos literais de primeiro nível do dia examinados, não aparecem `quem`, `quando`, `ausencia_id`, `decidido_quem`, `decidido_quando` ou `decidido_ausencia_id` [app/ponto/services/espelho.py:845, app/ponto/services/espelho.py:870–895, app/colaboradores/services/calendario.py:754–785]. A ausência de escrita indireta posterior de todas essas chaves permanece **[não verificado]**.
- Há nomes em estruturas **aninhadas**, que não são colisões no primeiro nível: o calendário inclui `veredito_humano` [app/colaboradores/services/calendario.py:770], cuja estrutura relacionada contém `quem` e `quando` [app/ponto/services/veredito_celula.py:129–131]. O espelho transporta `_decisao_do_dia` [app/ponto/services/espelho.py:871]; a estrutura relacionada contém `ausencia_id`, `quem` e `quando` [app/ponto/services/dia_decidido.py:537–539], e a chave de transporte é removida quando `limpar=True` [app/ponto/services/espelho.py:346–349].

### Script AST

O script abaixo foi executado por stdin, sem criar arquivo. Ele resolve os imports estáticos indicados, imprime chamadas e destinos, acessos às três chaves e **candidatos** para inspeção manual. Sua análise de nomes é conservadora: não resolve fluxo entre funções, aliases dinâmicos, reatribuições por escopo ou acesso reflexivo; portanto, os candidatos não constituem prova de censo fechado [não verificado]. A execução produziu saída truncada pela ferramenta; sua conclusão integral não foi observada [não verificado].

```python
import ast
from pathlib import Path
from collections import Counter

K = {'veredito_dia', 'palavra_dia', 'cor_dia'}
c = Counter()

for f in sorted(Path('app').rglob('*.py')):
    try:
        t = ast.parse(f.read_bytes(), filename=str(f))
    except (SyntaxError, UnicodeDecodeError) as e:
        print('ERRO', f, e)
        continue

    imp = {}
    parent = {}
    for n in ast.walk(t):
        for x in ast.iter_child_nodes(n):
            parent[x] = n
        if isinstance(n, ast.Import):
            for a in n.names:
                imp[a.asname or a.name.split('.')[0]] = (
                    a.name if a.asname else a.name.split('.')[0]
                )
        if isinstance(n, ast.ImportFrom):
            for a in n.names:
                imp[a.asname or a.name] = (n.module or '') + '.' + a.name

    def q(n):
        if isinstance(n, ast.Name):
            return imp.get(n.id, n.id)
        if isinstance(n, ast.Attribute):
            return q(n.value) + '.' + n.attr
        return ''

    tracked = set()
    for n in ast.walk(t):
        if (isinstance(n, ast.Call)
                and q(n.func) == 'ponto.services.dia_decidido.do_dia'):
            p = parent.get(n)
            c['calls'] += 1
            print(f, n.lineno, 'CALL', ast.unparse(
                p if isinstance(p, (ast.Assign, ast.AnnAssign, ast.Return))
                else n
            ))
            if isinstance(p, (ast.Assign, ast.AnnAssign)):
                targets = p.targets if isinstance(p, ast.Assign) else [p.target]
                for x in targets:
                    tracked.update(
                        y.id for y in ast.walk(x) if isinstance(y, ast.Name)
                    )

        value = None
        if (isinstance(n, ast.Subscript)
                and isinstance(n.slice, ast.Constant)
                and n.slice.value in K):
            value = n.value
        if (isinstance(n, ast.Call)
                and isinstance(n.func, ast.Attribute)
                and n.func.attr == 'get'
                and n.args
                and isinstance(n.args[0], ast.Constant)
                and n.args[0].value in K):
            value = n.func.value
        if value is not None:
            c['key'] += 1
            print(f, n.lineno, 'KEY', ast.unparse(n))
            tracked.update(
                x.id for x in ast.walk(value) if isinstance(x, ast.Name)
            )

    for n in ast.walk(t):
        candidate = isinstance(
            n, (ast.Compare, ast.For, ast.comprehension, ast.Call, ast.Dict)
        )
        names = {x.id for x in ast.walk(n) if isinstance(x, ast.Name)}
        if candidate and tracked.intersection(names):
            c['candidate'] += 1
            print(
                f, getattr(n, 'lineno', '?'), 'CANDIDATO',
                ast.unparse(n).replace('\n', ' ')[:500]
            )

print('TOTAIS', dict(c))
```

`N_quebra=1` — contado como **um leitor com quebra condicional identificada**, o teste de serialização e igualdade [app/api/tests/test_atestado_por_minutos.py:104–106, :126]; não é um número comprovado de falhas inevitáveis nem um limite superior para leitores ainda não verificados. [nao verificado]

`censo fechado: nao`

## 18/11 09:10 · sessao 9a2fed870b4c · ESPINHA-REMEDIDA-1811 · censo · origem=Code

## 1 Premissas e sitios

**Resultado:** a leitura direta de R caiu em U3, U6, U7 e U8, mas os substitutos consultam o juiz L-130 sobre a **projeção**, não a face histórica de atos descrita no brief e na matriz ([cartorio.py:1642](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1642), [calendario.py:505](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:505), [juizes.py:1544](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1544)).

Os anexos `brief.md` e `matriz.md` foram fornecidos no texto da pergunta, sem arquivos ou numeração de linhas disponíveis na cópia: suas linhas físicas são **[não verificado]**. Identifico abaixo cada sítio pelo número U e pela linha histórica que o próprio anexo informa. Para as contagens, cada linha P1–P34 é uma unidade: P1–P24 cobrem os 24 degraus da matriz e as nove unidades do brief; P25–P34 cobrem os dez itens do contrato de saída. **CAIU** significa que a premissa descrita deixou de valer ou que o requisito ainda não está demonstrado, conforme indicado em cada linha.

| # | Premissa e sítio no brief/matriz | Julgamento no dd489c627 |
|---|---|---|
| P1 | U1a, matriz `procedencia:62`: exclui batida de app. | **CONFERE** — [procedencia_batida.py:62](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/procedencia_batida.py:62). |
| P2 | U1b, `:70–71`: busca perguntas em lote; teste antigo mede uma query. | **CONFERE** — [procedencia_batida.py:70](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/procedencia_batida.py:70), [test_procedencia_batida.py:126](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/tests/test_procedencia_batida.py:126). O teste não prova custo de uma futura leitura de atos conservada. |
| P3 | U1c, `:72`: autor V vem da projeção. | **CONFERE** — [procedencia_batida.py:72](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/procedencia_batida.py:72). |
| P4 | U1d, `:72`: autor C é fallback da projeção. | **CONFERE** — [procedencia_batida.py:72](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/procedencia_batida.py:72). |
| P5 | U1e, `:74`: instante V vem da projeção. | **CONFERE** — [procedencia_batida.py:74](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/procedencia_batida.py:74). |
| P6 | U1f, `:74`: instante C é fallback da projeção. | **CONFERE** — [procedencia_batida.py:74](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/procedencia_batida.py:74). |
| P7 | U1g, `:86/90/93`: frases para carimbo, ausência de carimbo e ausência de pergunta. | **CONFERE** — [procedencia_batida.py:86](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/procedencia_batida.py:86). |
| P8 | U2a, `flip_auto:129`: filtro de via de paridade recusada. | **CONFERE** — [flip_auto.py:129](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/flip_auto.py:129). |
| P9 | U2b, `:126/130`: relógio interno e filtro de V. | **CONFERE** — [flip_auto.py:126](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/flip_auto.py:126). |
| P10 | U2c, `:132`: regex lê motivo técnico da projeção. | **CONFERE** — [flip_auto.py:132](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/flip_auto.py:132); escritor técnico em [materializacao.py:1689](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1689). |
| P11 | U2d, `:148`: deduplicação por batida. | **CONFERE** — [flip_auto.py:144](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/flip_auto.py:144). |
| P12 | U3x, `cartorio:1641/1648`: leitura direta de R isenta o lado x. | **CAIU** — o uso existe, mas chama o juiz em [cartorio.py:1642](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1642), usado em [cartorio.py:1649](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1649). |
| P13 | U3y, `:1641/1665`: mesma leitura direta isenta o lado y. | **CAIU** — juiz em [cartorio.py:1642](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1642), uso em [cartorio.py:1666](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1666). |
| P14 | U4, `cartorio:1650`: todas as vias são duplicata. | **MOVEU** — [cartorio.py:1651](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1651). |
| P15 | U5a, `veredito_celula:180–181`: universo `q_vivas()`. | **CONFERE** — [veredito_celula.py:180](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:180). |
| P16 | U5b, `:196`: SILENCIO recebe o carimbo C do ato recém-fechado. | **CONFERE** — [veredito_celula.py:190](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:190), [veredito_celula.py:196](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:196). |
| P17 | U5c, `:197`: SILENCIO copia R da projeção. | **CONFERE** — [veredito_celula.py:197](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:197). |
| P18 | U5d, matriz: `registrar` é idempotente. | **CAIU** — a proteção da repetição está na porta `fechar_por_admin`, chamada em [veredito_celula.py:190](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:190); `registrar` usa `bulk_create` em [models.py:4152](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4152). |
| P19 | U6a, brief E3 `calendario:504`: `.exclude(resposta_colab='')`. | **CAIU** — substituído pela face SQL do juiz em [calendario.py:499](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:499) e [calendario.py:505](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:505). |
| P20 | U6b, E3 `:506/509`: juiz de vida recebe relógio e oferta conhecida. | **MOVEU** — [calendario.py:507](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:507), [calendario.py:510](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:510). |
| P21 | U7a, `ficha:171`: contagem lê R diretamente. | **CAIU** — usa o juiz em [ficha.py:171](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/ficha.py:171). |
| P22 | U7b, matriz `models:1873/1893`: método cruzado lê projeção. | **MOVEU** e mudou o predicado de R — [models.py:1898](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1898), [models.py:1918](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1918), [models.py:1932](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1932). |
| P23 | U8, `previa:141–142`: lê R diretamente. | **CAIU** — usa o juiz em [previa.py:141](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/previa.py:141). |
| P24 | U9, matriz `dia_do_colab:69–70`: serializa R da projeção. | **MOVEU** — continua direto em [dia_do_colab.py:70](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/dia_do_colab.py:70). |
| P25 | Contrato da matriz P1: todos usam face pública conservada de atos. | **CAIU** — as faces históricas não foram encontradas nesta cópia; os leitores migrados usam o juiz da projeção, [juizes.py:1544](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1544), [juizes.py:1575](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1575). |
| P26 | Contrato P2: paridade dos cinco leitores inclusive para espaços. | **CAIU** — U9 devolve texto cru em [dia_do_colab.py:70](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/dia_do_colab.py:70); os leitores migrados usam instante e texto aparado, [juizes.py:1544](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1544). Comparar retorno textual com booleano exige definir a interpretação do teste. |
| P27 | Contrato P3: motivo técnico permanece na projeção. | **CONFERE** — [flip_auto.py:132](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/flip_auto.py:132). |
| P28 | Contrato P4: via e C permanecem na projeção. | **CONFERE** — [flip_auto.py:129](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/flip_auto.py:129), [cartorio.py:1651](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1651), [procedencia_batida.py:72](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/procedencia_batida.py:72). |
| P29 | Contrato P5: prova de custo constante em N perguntas carimbadas. | **CAIU a suficiência da prova apontada**, não uma medição de custo: o teste antigo mede a implementação atual; custo de uma futura face conservada em N carimbadas **[não verificado]** ([test_procedencia_batida.py:126](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/tests/test_procedencia_batida.py:126)). |
| P30 | Contrato P6: SILENCIO usa R corrente da face conservada. | **CAIU** — ainda copia a projeção em [veredito_celula.py:197](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:197). |
| P31 | Contrato P7: U2 recebe `agora` do chamador. | **CAIU** — chama `_tz.now()` internamente em [flip_auto.py:126](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/flip_auto.py:126). |
| P32 | Contrato P8: selo AST impede leituras diretas fora da allowlist. | **CAIU como propriedade do código atual** — U5 e U9 ainda leem R diretamente ([veredito_celula.py:197](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:197), [dia_do_colab.py:70](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/dia_do_colab.py:70)). Existência de um selo com a allowlist proposta **[não verificado]**. |
| P33 | Contrato P9: pinos registrados para os três sítios. | **CONFERE quanto aos valores presentes** — [test_medida_turno_do_par.py:275](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/core/tests/test_medida_turno_do_par.py:275), [test_medida_turno_do_par.py:319](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/core/tests/test_medida_turno_do_par.py:319), [test_medida_turno_do_par.py:330](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/core/tests/test_medida_turno_do_par.py:330). |
| P34 | Contrato P10: F8 sobre R3 e a antiga F1 conservada. | **CAIU como premissa conjunta** — a forma R3 com `agora` está em [calendario.py:188](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:188), mas a face histórica de atos não foi encontrada; a face SQL atual lê projeção ([juizes.py:1575](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1575)). A ordem histórica de pousos **[não verificado]**. |

**CONFERE 17, MOVEU 4, CAIU 13.** Essas contagens são das 34 unidades explicitadas acima; não são uma contagem de cada afirmação histórica de frota ou de cada teste citado nos anexos. [nao verificado]

## 2 O que mudou

O patch fornecido compara `fbc5c76ce` com `dd489c627`; a cópia não tem `.git`, portanto a comparação aqui é a do patch anexado, confrontada com a árvore atual. O diffstat e o patch são dados fornecidos, sem arquivo:linha na cópia **[não verificado]**.

- **Quatro leitores F8 mudaram:** cartório, calendário, ficha e prévia passaram de texto direto para `pergunta_respondida` ou sua face SQL ([cartorio.py:1642](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1642), [calendario.py:505](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:505), [ficha.py:172](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/ficha.py:172), [previa.py:142](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/previa.py:142)). O juiz exige instante **e** texto após trim, ambos da projeção ([juizes.py:1544](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1544)). Assim, a mudança já afeta texto sem instante e espaços; ela não implementa a antiga leitura da sequência de atos.
- **O leitor cruzado da ficha mudou junto:** `sem_veredito_entre` substituiu o antigo “instante ou texto” pelo juiz, mas continua usando V e C da projeção ([models.py:1918](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1918)).
- **Novos leitores do juiz no patch** incluem API ([api/views.py:2578](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:2578), [api/views.py:2622](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:2622)), telas de chamados ([chamados/views.py:1281](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views.py:1281)), validação ([validacao.py:682](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/validacao.py:682)) e materialização ([materializacao.py:872](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:872)). A nova guarda de anuência chama `_carimbar_recusa`, de modo que também pode escrever o motivo técnico lido por U2 ([materializacao.py:1287](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1287), [materializacao.py:1689](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1689)).
- **O inventário “dois escritores” do brief não deve ser tratado como dois sítios de código:** além da recusa administrativa, há a porta técnica e uma atribuição técnica direta ([validacao.py:916](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/validacao.py:916), [materializacao.py:1689](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1689), [materializacao.py:1352](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1352)).
- **F2 acrescentou marcas na decisão:** a fotografia sintética escreve `resposta_mantem_validacao=True`; escritores de resposta humana e automática declaram seus valores ([models.py:4143](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4143), [disputa_emissao.py:1527](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1527), [disputa_emissao.py:1681](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1681)). A tarefa passou a excluir da sequência a reabertura marcada pelo predicado novo ([models.py:4103](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4103), [tarefa_do_dia.py:427](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:427)).
- O patch também altera o fluxo da porta de exportação e o tratamento de feriado do motor intermitente ([folha/views.py:128](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/views.py:128), [motor_calculo_v2.py:3578](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/motor_calculo_v2.py:3578)). Não há, nesses trechos, uma troca dos leitores R/V/C de F8.

Os números raspados — inclusive 187 perguntas, seis batidas e as contagens da fila — são observações dos anexos sobre árvores e momentos anteriores; seu valor em produção ou nesta árvore **[não verificado]**. Não é possível concluir neutralidade atual de frota apenas a partir do código.

## 3 Nao verificado de ALCANCE

- **Fechado: leitor cruzado da ficha.** `ficha_do_colab` chama `respostas_sem_veredito`, que delega a `sem_veredito_entre`; este lê V/C e o juiz de R ([ficha.py:169](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/ficha.py:169), [models.py:1898](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1898), [models.py:1932](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1932)).
- **Fechado para os chamadores citados do calendário e procedência:** tela, drawer, relatório e tarefa chegam ao calendário; calendário e espelho chamam procedência ([colaboradores/views.py:1990](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/views.py:1990), [drawer.py:123](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/drawer.py:123), [furos.py:164](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/relatorios/furos.py:164), [tarefa_do_dia.py:619](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:619), [calendario.py:651](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:651), [espelho.py:756](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/espelho.py:756)).
- **Fechado para as duas cascas citadas:** mês e drawer incluem a grade; a grade lê procedência, e o PDF lê a frase de papel ([calendario_partial.html:67](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/colaboradores/calendario_partial.html:67), [_drawer_diagnostico.html:35](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/colaboradores/partials/_drawer_diagnostico.html:35), [_calendario_grade.html:127](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/colaboradores/partials/_calendario_grade.html:127), [pdf_espelho.py:950](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/relatorios/pdf_espelho.py:950)).
- **Aberto 1 — censo global completo de escritores e leitores dos campos.** Os sítios acima e os novos leitores do patch foram localizados, mas não foi refeito o censo global citado pelo brief. Falta percorrer todas as ocorrências e aliases de `resposta_colab`, `respondida_em`, V, C, via e motivo em `app/`; há inclusive leitores diretos em templates de chamados ([modal_fio.html:219](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/modal_fio.html:219)).
- **Aberto 2 — alcance do selo de queries para N perguntas carimbadas.** Há testes de custo do calendário ([test_e4cal_l1_calendario_le_o_d4.py:230](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:230)), mas não ficou demonstrado um teste que varie especificamente N perguntas com R e V carimbadas. Falta abrir e confrontar os testes de queries dos demais apps com esse cenário.
- **Aberto 3 — efeito de `respondida_em > agora` no calendário.** O calendário passa `agora` ao juiz de vida ([calendario.py:507](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:507)), enquanto a face de resposta não recebe relógio ([juizes.py:1575](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/jui

[truncado em 20000 de 21243 caracteres]

## 18/11 09:10 · sessao b24a819bcba2 · ESPINHA-REMEDIDA-1811 · censo · origem=Code

## 1 Premissas e sitios

**Critério da contagem:** cada P corresponde a um dos 18 degraus da matriz, incluindo as 12 unidades do brief, o ramo APAGA e os dois itens SOME. **CONFERE** significa que o sítio e a operação descritos permanecem; **MOVEU** inclui deslocamento de linha ou substituição da operação no mesmo fluxo; **CAIU** significa que não foi encontrado sítio correspondente. A classificação descreve o código atual, não afirma que o alvo F6 foi implementado.

Os anexos `brief.md` e `matriz.md` foram fornecidos no texto da pergunta, sem arquivos físicos numerados na cópia. Identifico cada linha pela unidade e pelo sítio antigo literalmente declarado no anexo; o número da linha *do próprio anexo* é **[nao verificado]**.

- **P1 — U1a, CONFERE.** Brief U1 e matriz U1a, `views_mensageria.py:567`: `_item_validar` ainda entrega `resposta_colab` cru. [app/api/views_mensageria.py:567](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views_mensageria.py:567).
- **P2 — U1b, CONFERE.** Brief U1 e matriz U1b, linha antiga 569: `respondida_em.isoformat()` continua sem guarda. O universo atual exige instante e texto; o 500 previsto para a F1 antiga não está demonstrado nesse universo atual. [app/api/views_mensageria.py:539](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views_mensageria.py:539); [app/chamados/juizes.py:1761](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1761).
- **P3 — U1c, CONFERE.** Brief U1 e matriz U1c, linha antiga 570: o leitor ainda usa `timezone.now()` e subtrai o instante cru. [app/api/views_mensageria.py:570](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views_mensageria.py:570).
- **P4 — U2, CONFERE.** Brief U2 e matriz U2, linhas antigas 1163–1165: o JSON ainda lê texto e instante da projeção e `resposta_display` da propriedade. [app/api/views.py:1163](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:1163); [app/chamados/models.py:3699](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3699).
- **P5 — U3, CONFERE.** Brief U3 e matriz U3, linhas antigas 2561–2563: o JSON ainda lê os mesmos campos da projeção. [app/api/views.py:2561](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:2561).
- **P6 — U4, MOVEU.** Brief U4 e matriz U4, linha antiga 2553: a comparação com `estado == RESPONDIDA` permanece, mas `estado` agora usa a propriedade `respondida`, que delega ao juiz L-130; ainda não lê a tarefa do dia. [app/api/views.py:2550](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:2550); [app/chamados/models.py:3152](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3152); [app/chamados/models.py:3674](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3674).
- **P7 — U5, MOVEU.** Brief U5 e matriz U5, linha antiga 2577: `respondidas` usa `pergunta_respondida`, não mais apenas instante não nulo. [app/api/views.py:2578](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:2578); [app/chamados/juizes.py:1544](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1544).
- **P8 — U6, MOVEU.** Brief U6 e matriz U6, linhas antigas 2620 e 2637: `ja_respondida` usa o juiz; o JSON ainda lê o instante recém-gravado. A escolha também afeta o log e a tag do follow-up. [app/api/views.py:2622](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:2622); [app/api/views.py:2639](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:2639); [app/chamados/services/disputa_emissao.py:1547](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1547); [app/chamados/services/disputa_emissao.py:1562](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1562).
- **P9 — U7a, MOVEU.** Brief U7 e matriz U7a, linha antiga 169: `pendentes` usa o juiz e alimenta a porta de validação. [app/chamados/views_cobrar.py:168](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views_cobrar.py:168); [app/chamados/views_cobrar.py:203](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views_cobrar.py:203).
- **P10 — U7b, MOVEU.** Brief U7 e matriz U7b, linha antiga 175: `_aguard` usa `q_nao_respondida`; continua sem filtro de vitalidade e conta também perguntas mortas que satisfaçam a face SQL. [app/chamados/views_cobrar.py:176](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views_cobrar.py:176); [app/chamados/juizes.py:1583](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1583).
- **P11 — U8, MOVEU.** Brief U8 e matriz U8, linha antiga 291: o sítio passou à linha 293 e continua lendo `resposta_colab` cru. [app/chamados/views_cobrar.py:293](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views_cobrar.py:293).
- **P12 — U9a, MOVEU.** Brief U9 e matriz U9a, linha antiga 1280: a view web passa o resultado do juiz ao core, com o mesmo efeito sobre o rótulo persistido de P8. [app/chamados/views.py:1280](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views.py:1280); [app/chamados/services/disputa_emissao.py:1562](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1562).
- **P13 — U9b / APAGA, CONFERE.** Brief APAGA e matriz U9b, linhas antigas 1256–1264: o ramo ainda existe; a porta anterior elimina a pergunta não viva, sem mutação entre as duas verificações. Portanto a remoção pedida ainda resta. [app/chamados/views.py:1238](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views.py:1238); [app/chamados/views.py:1256](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views.py:1256).
- **P14 — U10, MOVEU.** Brief U10 e matriz U10, linha antiga 1416: `_sem_resp` usa `q_nao_respondida`; o resultado e `perguntas_corrigiveis` escolhem recobrança ou reabertura. [app/chamados/views.py:1417](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views.py:1417); [app/chamados/juizes.py:1753](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1753).
- **P15 — U11, MOVEU.** Brief U11 e matriz U11, linha antiga 64: canal usa a face negada do juiz, cujo universo inclui texto sem instante e instante sem texto, em vez de `resposta_colab=''`. [app/chamados/canal.py:64](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/canal.py:64); [app/chamados/juizes.py:1591](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1591).
- **P16 — U12, MOVEU.** Brief U12 e matriz U12, linha antiga 142: o filtro está na linha 145 e a guarda de texto cru na 166; o classificador também lê a projeção. [app/chamados/templatetags/chamados_tags.py:145](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/templatetags/chamados_tags.py:145); [app/chamados/juizes.py:2034](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:2034).
- **P17 — S1 / SOME, CONFERE.** Brief SOME e matriz S1, linhas antigas 11–28: o relatório delega a `pode_fechar` e `aguarda_o_colaborador`; `coletar` filtra `fechada_em`. [app/chamados/relatorios_disputa.py:23](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/relatorios_disputa.py:23); [app/chamados/relatorios_disputa.py:46](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/relatorios_disputa.py:46).
- **P18 — S2 / SOME, CONFERE.** Brief SOME e matriz S2, linhas antigas 1174–1181: `pode_solicitar_correcao` segue delegado a `fio.aguarda_o_colaborador`. [app/api/views.py:1174](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:1174).

**CONFERE 8, MOVEU 10, CAIU 0.** Essas contagens são dos 18 sítios acima; não contam como concluído o alvo de migração dos que ainda leem projeção.

## 2 O que mudou

O patch fornecido é o diff `fbc5c76ce..dd489c627`; a cópia não fornece `.git`, portanto a comparação desta seção vem do patch anexado e da conferência dos sítios atuais. O outro hash mencionado no pedido, `520d7b557`, não tem patch comparativo próprio no dado fornecido: **[nao verificado]** o seu delta específico.

- **Juiz e leitores:** a alteração relevante para F6 troca os filtros de U5, U6, U7a, U7b, U9a, U10 e U11 para `pergunta_respondida` ou sua face negada. O juiz exige **instante e texto após trim**; as duas metades isoladas não contam como resposta. Isso altera também `estado` e as condições de exibição dos dois templates. [app/chamados/juizes.py:1544](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1544); [app/chamados/models.py:3152](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3152); [app/templates/chamados/partials/_pergunta_card.html:24](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/_pergunta_card.html:24); [app/templates/chamados/partials/modal_fio.html:217](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/modal_fio.html:217).
- **Leitores novos no delta:** além das views e do canal, a face negada passou a alimentar a contagem de `recobrar_questionario` e o universo de ressuscitação; `perguntas_corrigiveis` passou à face positiva. Assim, a seleção do escritor em U10 e a contagem interna da recobrança mudam juntas. [app/chamados/services/disputa_emissao.py:1737](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1737); [app/chamados/models.py:1529](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1529); [app/chamados/juizes.py:1753](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1753).
- **Campos novos da decisão:** `resposta_mantem_validacao` é declarado pela resposta humana como falso e pelas respostas automática, pela trilha e pela foto da projeção como verdadeiro. `da_ressuscitacao` é lido pela carga da tarefa por `es_da_ressuscitacao`; o código o descreve como inerte até um escritor posterior marcar verdadeiro. Isso muda a preparação da autoridade que U4 pretende ler, não faz U4 já lê-la. [app/chamados/models.py:4067](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4067); [app/chamados/models.py:4143](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4143); [app/chamados/services/disputa_emissao.py:1527](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1527); [app/chamados/services/disputa_emissao.py:1681](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1681); [app/chamados/services/disputa_emissao.py:1703](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1703); [app/chamados/services/tarefa_do_dia.py:427](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:427).
- **Cerca e remoção lateral:** o patch acrescenta vigilância de competência na escrita e no lote e cerca o resolver; também remove `reabrir_fantasma` com lápide. São mudanças nas portas cruzadas, não a remoção U9b, que permanece. [app/chamados/models.py:3394](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3394); [app/chamados/models.py:3559](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3559); [app/chamados/services/disputa_emissao.py:1901](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1901); [app/chamados/models.py:3426](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3426); [app/chamados/views.py:1256](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views.py:1256).
- **Fora da F6:** o hunk de `core/juizes.py` baixa um pendente de feriado intermitente; o patch não mostra nesse hunk alteração dos leitores da pergunta. [app/core/juizes.py:492](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/core/juizes.py:492).

A antiga premissa “F6 é só leitura, nada persiste” precisa ser lida por degrau: as views U6/U9 passam um booleano que altera trilha persistida, U7 chama validação e U10 escolhe portas com efeitos. Isso já era apontado como correção pela matriz e permanece no código. [app/chamados/services/disputa_emissao.py:1547](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1547); [app/chamados/views_cobrar.py:203](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views_cobrar.py:203); [app/chamados/views.py:1421](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views.py:1421).

## 3 Nao verificado de ALCANCE

- **Caddy: fechado na cópia.** As rotas especiais de batida vão a `core`; o proxy restante vai a `ui`, abrangendo as rotas F6 citadas no brief. Configuração efetivamente implantada em produção: **[nao verificado]**. [Caddyfile:8](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/Caddyfile:8); [app/api/urls.py:73](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/urls.py:73); [app/api/urls.py:92](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/urls.py:92).
- **Dono F5 e leitores das duas cascas: fechados na cópia.** `particao_do_app` hoje usa o juiz, não mais só o instante. O filtro é chamado nos dois templates; o card é incluído em detalhe local e o modal é renderizado pela view. O modal também exibe `resposta_colab` diretamente: migrar só o filtro não cobre essa leitura. [app/chamados/services/pergunta_no_app.py:76](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/pergunta_no_app.py:76); [app/templates/chamados/partials/_pergunta_card.html:43](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/_pergunta_card.html:43); [app/templates/chamados/partials/modal_fio.html:219](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/modal_fio.html:219); [app/templates/chamados/partials/modal_fio.html:274](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/modal_fio.html:274); [app/templates/chamados/detalhe_local.html:310](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/detalhe_local.html:310); [app/chamados/views.py:927](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views.py:927).
- **Contrato APK: fechado por registro de corte, não por leitura independente do aplicativo.** O complemento registra que `respondida_em` é opcional e `dias_esperando` não é lido; o código Kotlin correspondente não foi aberto nesta cópia: **[nao verificado]** independentemente. [app/docs/CORTES.md:512](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/CORTES.md:512).
- **Aberto 1 — censo completo de escritores e leitores cruzados:** foram identificadas as portas e os leitores citados acima, mas não foi refeito o censo AST integral de todos os usos dos quatro campos na árvore atual. Falta executar um censo somente de leitura e abrir os resultados por função, inclusive atribuições indiretas e ORM. **[nao verificado]** se a lista de P1–P18 e dos leitores novos esgota todos os consumidores cruzados. [app/chamados/services/disputa_emissao.py:1675](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1675); [app/chamados/models.py:4143](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4143).
- **Aberto 2 — contrato antigo `atos_pergunta.leitura`:** a matriz descreve uma F1 antiga; não foi encontrado esse módulo na árvore pesquisada. Falta abrir o artefato exato da F1 antiga para comparar seu contrato com o desenho vigente; não se pode atribuir o P5 ou o 500 hipotético daquela versão ao HEAD apenas pela matriz. O universo atual da mensageria exige instante e texto. **[nao verificado]** o contrato daquela cópia antiga. [app/chamados/juizes.py:1761](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1761).

**alcance nao verificado restante: 2**

## 4 Perguntas de lei

- **DECIDIDO PELA LEI CORTES-F2-F9-1611, item 6:** a pergunta original da matriz sobre `aguarda_admin` nos estados omitidos usa o dono declarado pela tarefa: admin verdadeiro, colaborador falso e ausência de dono falso. O código atual ainda compara `estado` com `RESPONDIDA`, de modo que decisão e implementação são coisas distintas. [app/docs/CORTES.md:509](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/CORTES.md:509); [app/api/views.py:2553](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:2553).
- **DECIDIDO PELA LEI L-130:** para P5, texto sem instante não constitui pergunta respondida; o corte substitui a antiga JUIZ-RESPONDIDA e a pergunta F6(2). A compatibilidade de `null` foi registrada no complemento posterior. [app/docs/LEIS.md:151](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/LEIS.md:151); [app/docs/CORTES.md:512](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/CORTES.md:512).
- **Silêncio:** o corte anexado ao brief registra que a F5 deve passar a cobrar o admin e declara F5/F6 resolvidas; o código atual ainda devolve `silencio_decidido` sem ações. É uma diferença entre corte e implementação, não uma nova pergunta de lei demonstrada pelo material. [app/chamados/services/tarefa_do_dia.py:176](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:176).

perguntas de lei: 0

## 18/11 09:10 · sessao 7f82c1b07e52 · DST-L2 · censo · origem=Code

HASH LIDO = dd489c627bb0a8d5f8782945c7e240e6621c98ed

[nao verificado] O hash acima é o identificador informado e presente no caminho do workspace; não consegui confirmá-lo por Git.

**0.** Script AST consolidado: [nao verificado] Não o executei. A resolução é estática: chamadas dinâmicas, reexportações, atribuições de apelidos fora de `import` e sombreamento dependente de execução podem exigir inspeção manual. `literal_cobrado` inclui ocorrências candidatas, que precisam ser distinguidas entre leitura, escrita e dados. [nao verificado]

```python
#!/usr/bin/env python3
import ast
import pathlib
import sys

ALVOS = {
    "chamados.juizes.dia_sem_trabalho",
    "chamados.juizes.ausencia_parcial_cobra_o_resto",
    "ponto.supra_juiz._dia_sem_trabalho_pelo_juiz",
    "ponto.services.lastro.marcos_acesos",
}
CHAVES = {"tipo_dia", "cobertura_parcial"}


def nome_modulo(rel):
    partes = list(rel.with_suffix("").parts)
    if partes and partes[-1] == "__init__":
        partes.pop()
    return ".".join(partes)


def caminho_expr(no):
    if isinstance(no, ast.Name):
        return no.id
    if isinstance(no, ast.Attribute):
        base = caminho_expr(no.value)
        return base + "." + no.attr if base else None
    return None


def string(no):
    return no.value if isinstance(no, ast.Constant) and isinstance(no.value, str) else None


def resolver_relativo(modulo, eh_init, nivel, nome):
    pacote = modulo.split(".") if eh_init else modulo.split(".")[:-1]
    if nivel:
        corte = nivel - 1
        if corte > len(pacote):
            return None
        pacote = pacote[:len(pacote) - corte]
        if nome:
            pacote.extend(nome.split("."))
        return ".".join(pacote)
    return nome or ""


class Censo(ast.NodeVisitor):
    def __init__(self, arquivo, rel, arvore):
        self.arquivo = arquivo
        self.rel = rel
        self.modulo = nome_modulo(rel)
        self.eh_init = rel.name == "__init__.py"
        self.escopos = [{}]
        self.funcoes = []
        self.linhas = set()
        self.locais = {
            n.name for n in arvore.body
            if isinstance(n, (ast.FunctionDef, ast.AsyncFunctionDef))
        }
        partes = set(rel.parts)
        self.classe = (
            "tests" if "tests" in partes or "test" in partes
            or rel.name.startswith("test_") else "producao"
        )

    def emitir(self, no, item, operacao):
        funcao = ".".join(self.funcoes) or "<modulo>"
        self.linhas.add((
            str(self.rel) + ":" + str(no.lineno),
            funcao, item, operacao, self.classe,
        ))

    def buscar(self, nome):
        for escopo in reversed(self.escopos):
            if nome in escopo:
                return escopo[nome]
        return None

    def resolver(self, no):
        texto = caminho_expr(no)
        if not texto:
            return None
        primeiro, separador, resto = texto.partition(".")
        base = self.buscar(primeiro)
        if base is not None:
            return base + (separador + resto if separador else "")
        if not separador and primeiro in self.locais:
            return self.modulo + "." + primeiro
        return texto

    def visit_Import(self, no):
        for alias in no.names:
            if alias.asname:
                self.escopos[-1][alias.asname] = alias.name
            else:
                primeiro = alias.name.split(".")[0]
                self.escopos[-1][primeiro] = primeiro

    def visit_ImportFrom(self, no):
        base = resolver_relativo(
            self.modulo, self.eh_init, no.level, no.module
        )
        if base is None:
            return
        for alias in no.names:
            if alias.name != "*":
                nome = alias.asname or alias.name
                self.escopos[-1][nome] = ".".join(
                    p for p in (base, alias.name) if p
                )

    def escopo_funcao(self, no):
        for decorador in no.decorator_list:
            self.visit(decorador)
        for default in list(no.args.defaults) + [
            x for x in no.args.kw_defaults if x is not None
        ]:
            self.visit(default)
        self.funcoes.append(no.name)
        self.escopos.append({})
        for filho in no.body:
            self.visit(filho)
        self.escopos.pop()
        self.funcoes.pop()

    visit_FunctionDef = escopo_funcao
    visit_AsyncFunctionDef = escopo_funcao

    def visit_ClassDef(self, no):
        for decorador in no.decorator_list:
            self.visit(decorador)
        for base in no.bases:
            self.visit(base)
        self.funcoes.append(no.name)
        self.escopos.append({})
        for filho in no.body:
            self.visit(filho)
        self.escopos.pop()
        self.funcoes.pop()

    def visit_Call(self, no):
        alvo = self.resolver(no.func)
        if alvo in ALVOS:
            self.emitir(no, alvo, "chamada")

        if isinstance(no.func, ast.Attribute):
            metodo = no.func.attr
            if metodo == "get" and no.args:
                chave = string(no.args[0])
                if chave in CHAVES:
                    self.emitir(no, chave, "leitura_get")
            if metodo == "update":
                for arg in no.args:
                    if isinstance(arg, ast.Dict):
                        for chave in arg.keys:
                            valor = string(chave)
                            if valor in CHAVES:
                                self.emitir(chave, valor, "escrita_update_literal")
                for kw in no.keywords:
                    if kw.arg in CHAVES:
                        self.emitir(kw.value, kw.arg, "escrita_update_kw")

        if isinstance(no.func, ast.Name) and no.func.id == "dict":
            for kw in no.keywords:
                if kw.arg in CHAVES:
                    self.emitir(kw.value, kw.arg, "escrita_dict_kw")

        self.generic_visit(no)

    def visit_Subscript(self, no):
        chave = string(no.slice)
        if chave in CHAVES:
            if isinstance(no.ctx, ast.Load):
                operacao = "leitura_subscript"
            elif isinstance(no.ctx, ast.Del):
                operacao = "exclusao_subscript"
            else:
                operacao = "escrita_subscript"
            self.emitir(no, chave, operacao)
        self.generic_visit(no)

    def visit_Dict(self, no):
        for chave in no.keys:
            valor = string(chave)
            if valor in CHAVES:
                self.emitir(chave, valor, "escrita_dict_literal")
        self.generic_visit(no)

    def visit_Attribute(self, no):
        if no.attr == "VEREDITOS_ACUSAM" and isinstance(no.ctx, ast.Load):
            self.emitir(no, "VEREDITOS_ACUSAM", "leitura_atributo")
        self.generic_visit(no)

    def visit_Constant(self, no):
        if no.value == "cobrado":
            self.emitir(no, "cobrado", "literal_cobrado")


def main():
    if len(sys.argv) != 2:
        print("uso: python3 script.py <raiz_app>", file=sys.stderr)
        return 2

    raiz = pathlib.Path(sys.argv[1]).resolve()
    if not raiz.is_dir():
        print("raiz_app nao e diretorio: " + str(raiz), file=sys.stderr)
        return 2

    print("arquivo:linha\tfuncao\titem\toperacao\tgrupo")
    erros = 0
    for arquivo in sorted(raiz.rglob("*.py")):
        rel = arquivo.relative_to(raiz)
        if "migrations" in rel.parts:
            continue
        try:
            with arquivo.open("r", encoding="utf-8") as fonte:
                arvore = ast.parse(fonte.read(), filename=str(rel))
        except (OSError, UnicodeError, SyntaxError) as exc:
            print(str(rel) + ": " + str(exc), file=sys.stderr)
            erros += 1
            continue
        censo = Censo(arquivo, rel, arvore)
        censo.visit(arvore)
        for linha in sorted(censo.linhas):
            print("\t".join(linha))
    return 1 if erros else 0


if __name__ == "__main__":
    sys.exit(main())
```

**1.** [nao verificado] Será respondido pela execução do script; o resultado não foi produzido nesta leitura.

**2.** Classificação parcial:

- [app/chamados/juizes.py:442](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:442): `registro` aceita tanto ATA quanto DIA da grade.
- [app/chamados/juizes.py:454](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:454): `ata` é a ATA de `cel`.
- [app/ponto/services/cartorio.py:738](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:738): lê `tipo_dia` do DIA e escreve na ATA; [app/ponto/services/cartorio.py:766](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:766): faz a mesma transferência de `cobertura_parcial`.
- [app/ponto/services/cartorio.py:401](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:401): `registro` de `parcial_so_lista` pode ser DIA ou ATA.
- [app/escala/services/leitor_celula.py:397](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/services/leitor_celula.py:397): lê ATA para formar o DIA; [app/escala/services/leitor_celula.py:433](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/services/leitor_celula.py:433): transfere a cobertura da ATA para o DIA.
- [nao verificado] Falta classificar individualmente os demais acessos de produção.

**3.** Leitores identificados, ainda sem alegação de exaustividade:

- [app/ponto/status_dia.py:68](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/status_dia.py:68): inclui `cobrado` no conjunto de vereditos irregulares usado também na parcial.
- [app/chamados/services/fabrica_da_celula.py:120](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fabrica_da_celula.py:120): interrompe perguntas quando há veredito fora de `VEREDITOS_ACUSAM`.
- [app/chamados/juizes.py:1379](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1379): testa se o veredito está fora de `VEREDITOS_ACUSAM`; [app/chamados/juizes.py:1456](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1456): testa a pertinência ao conjunto.
- [app/relatorios/dia_da_frota.py:55](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/relatorios/dia_da_frota.py:55): seleciona células que acusam ou têm veredito `faltou`.
- [nao verificado] Faltam os demais leitores e a descrição individual de suas consequências.

**4.** Reações verificadas nesse caminho, condicionalmente às guardas:

- [app/ponto/services/cartorio.py:1539](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1539) chama a transição com `ato=x.reconciliar`; [app/chamados/services/fecho_runtime.py:161](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fecho_runtime.py:161) executa o ato em transação e entrega recusas à escrita de pautas.
- [app/chamados/models.py:651](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:651) decide o fechamento e, quando permitido, chama `_gravar_estado`; [app/chamados/models.py:228](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:228) prepara status, carimbo, via e urgência.
- [app/chamados/signals.py:180](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/signals.py:180) captura o status anterior no `pre_save`; [app/chamados/signals.py:54](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/signals.py:54) pode rebaixar urgência por `update`, implementado em [app/chamados/models.py:637](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:637).
- [app/chamados/signals.py:91](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/signals.py:91) chama o resolvedor de perguntas; seu corpo pode materializar perguntas e fechar disputa, conforme [app/chamados/services/disputa_emissao.py:1920](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1920).
- [app/chamados/signals.py:189](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/signals.py:189) retorna durante julgamento do cartório; fora dele, pode chamar `julgar_celula`. [app/chamados/signals.py:351](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/signals.py:351) pode agendar reconciliação do pai após commit.
- [app/chamados/services/fecho_runtime.py:128](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fecho_runtime.py:128) só escreve pauta quando há recusa ancorada, usando a porta em [app/chamados/services/fecho_runtime.py:139](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fecho_runtime.py:139).
- [nao verificado] A cascata completa de receivers e escritores indiretos não foi fechada.

**5.** Predicados próprios identificados:

- [app/ponto/services/cartorio.py:1458](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1458): `_protesta` exige `trabalha is not False`, além de testar os marcos; seu resultado participa da seleção de chamados a reconciliar em [app/ponto/services/cartorio.py:1533](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1533).
- [app/chamados/services/declaracao_texto.py:120](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/declaracao_texto.py:120): dispensa os marcos quando o DIA não é `trabalho`, salvo ausência parcial que cobra o resto.
- [app/ponto/supra_juiz.py:317](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/supra_juiz.py:317): define `dia_de_cobranca` a partir de `tipo_dia` e cobertura parcial para o ramo iniciado em [app/ponto/supra_juiz.py:326](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/supra_juiz.py:326).
- [nao verificado] A lista de todos os predicados do escopo ainda não foi completada.

**6.** Caminhos e origem identificados:

- [app/chamados/services/fabrica_da_celula.py:201](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fabrica_da_celula.py:201) recebe a célula de `perguntar_pela_celula`; no julgamento, [app/ponto/services/cartorio.py:1043](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1043) passa as células julgadas naquele ato. A mesma função chama o juiz de [app/chamados/services/fabrica_da_celula.py:112](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fabrica_da_celula.py:112) por `motivos_a_perguntar`, em [app/chamados/services/fabrica_da_celula.py:207](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fabrica_da_celula.py:207).
- [app/chamados/services/fabrica_da_celula.py:299](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fabrica_da_celula.py:299): o caminho por eventos busca células no banco antes de chamar `perguntar_pela_celula`.
- [app/ponto/services/cluster_espurio.py:46](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cluster_espurio.py:46) e [app/ponto/services/cluster_espurio.py:71](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cluster_espurio.py:71): os caminhos de abertura e fechamento consultam células no banco antes de chegar ao juiz de [app/ponto/services/cluster_espurio.py:25](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cluster_espurio.py:25). O julgamento chama a reconciliação do cluster após a lavra em [app/ponto/services/cartorio.py:1035](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1035).
- [nao verificado] Faltam todos os chamadores transitivos e a confirmação da proveniência em cada ramo.

censo fechado: nao — faltam executar e validar o script, classificar todos os acessos, completar os leitores e predicados, e fechar as cascatas e os chamadores transitivos. [nao verificado]

## 18/11 09:09 · sessao 3494b3c56996 · ESPINHA-REMEDIDA-1811 · censo · origem=Code

## 1 Premissas e sitios

**Resultado parcial:** os 32 itens abaixo cobrem os 28 degraus nomeados da matriz e os quatro grupos de exceções do brief. Não cobrem todas as demais premissas narrativas, provas, dependências e instruções de pouso dos dois anexos; portanto, as contagens ao final **não são a contagem integral solicitada**. Os anexos foram fornecidos no texto da pergunta, sem arquivo numerado na cópia: suas linhas são [não verificado]. Para identificá-los, uso os nomes dos degraus e as linhas históricas citadas nos próprios anexos. [nao verificado]

Critério desta tabela: **CONFERE** preserva o sítio e a leitura descrita; **MOVEU** preserva parte relevante, mas mudou de linha ou de composição; **CAIU** indica que a leitura específica descrita deixou de existir naquele sítio.

| Premissa | Julgamento |
|---|---|
| P1 — matriz C1, card antigo :77, `p.viva` | **CONFERE** — [card:77](app/templates/chamados/partials/_card_atendimento.html:77). A propriedade delega ao juiz de vida em [models.py:3507](app/chamados/models.py:3507). |
| P2 — C2, :100, caixa por `resposta_enviada` | **CONFERE** — [card:100](app/templates/chamados/partials/_card_atendimento.html:100); a flag é posta em [fio.py:360](app/chamados/services/fio.py:360). |
| P3 — C3, :106, texto e instante | **CONFERE** — [card:106](app/templates/chamados/partials/_card_atendimento.html:106); `resposta_display` ainda lê a projeção em [models.py:3699](app/chamados/models.py:3699). |
| P4 — C4, :114, radio por resposta | **CONFERE** — [card:114](app/templates/chamados/partials/_card_atendimento.html:114). |
| P5 — C5, :141–144, resposta de hora | **CONFERE** — [card:141](app/templates/chamados/partials/_card_atendimento.html:141) e [card:143](app/templates/chamados/partials/_card_atendimento.html:143). |
| P6 — C6, :133–137, input de hora sem prefill | **CONFERE** — [card:133](app/templates/chamados/partials/_card_atendimento.html:133). |
| P7 — C7, :148, prefill `datetime-local` | **CONFERE** — [card:148](app/templates/chamados/partials/_card_atendimento.html:148). |
| P8 — C8, :154, `selected` pela projeção | **CONFERE** — [card:154](app/templates/chamados/partials/_card_atendimento.html:154). |
| P9 — C9, :161, Enviar/Reenviar | **CONFERE** — [card:161](app/templates/chamados/partials/_card_atendimento.html:161). |
| P10 — C10, :182–183, histórico de resposta | **CONFERE** — [card:182](app/templates/chamados/partials/_card_atendimento.html:182). |
| P11 — C11, :189, pendência e estado | **CONFERE** — [card:189](app/templates/chamados/partials/_card_atendimento.html:189). `estado` ainda usa `validada_em` e `materializada_em`, com o juiz de respondida no ramo restante: [models.py:3152](app/chamados/models.py:3152). |
| P12 — C12, :390–391, chip enviado | **CONFERE** — [card:390](app/templates/chamados/partials/_card_atendimento.html:390). |
| P13 — P0 da matriz, `acoes_chamado.py:352`, universo vivo | **MOVEU** — a consulta está em [acoes_chamado.py:352](app/chamados/services/acoes_chamado.py:352) e o filtro por juiz em [acoes_chamado.py:358](app/chamados/services/acoes_chamado.py:358); não há prefetch nessa consulta. O fragmento consulta separadamente em [views.py:399](app/chamados/views.py:399). |
| P14 — P1, pergunta-card :20, opacidade por validação | **CONFERE** — [pergunta-card:20](app/templates/chamados/partials/_pergunta_card.html:20). |
| P15 — P2, :23, rótulo e data de validação | **CONFERE** — [pergunta-card:23](app/templates/chamados/partials/_pergunta_card.html:23). |
| P16 — P3, :24, condição por `respondida_em` | **CAIU** — agora usa `perg.respondida` em [pergunta-card:24](app/templates/chamados/partials/_pergunta_card.html:24). |
| P17 — P4, :29, condição por `respondida_em` | **CAIU** — agora usa `perg.respondida` em [pergunta-card:29](app/templates/chamados/partials/_pergunta_card.html:29). |
| P18 — P5, :31–32, texto e instante | **CONFERE** — [pergunta-card:31](app/templates/chamados/partials/_pergunta_card.html:31). |
| P19 — P6, :42, gate de validação | **CONFERE** — [pergunta-card:42](app/templates/chamados/partials/_pergunta_card.html:42); há também adaptação de aplicação dentro do bloco em [pergunta-card:43](app/templates/chamados/partials/_pergunta_card.html:43). |
| P20 — P7, :43, filtro de leitura da resposta | **CONFERE** — [pergunta-card:43](app/templates/chamados/partials/_pergunta_card.html:43); o filtro lê `resposta_colab` em [chamados_tags.py:166](app/chamados/templatetags/chamados_tags.py:166). |
| P21 — M0, fio :246–273, universo e prefetch | **CONFERE** — [fio.py:251](app/chamados/services/fio.py:251), [fio.py:264](app/chamados/services/fio.py:264) e [fio.py:270](app/chamados/services/fio.py:270). |
| P22 — M1, modal :185, condição por `respondida_em` | **CAIU** — usa `perg.respondida` em [modal:185](app/templates/chamados/partials/modal_fio.html:185). |
| P23 — M2, :205–216, respostas anteriores pela sequência | **CONFERE** — [modal:205](app/templates/chamados/partials/modal_fio.html:205) e [models.py:3680](app/chamados/models.py:3680). |
| P24 — M3, :217–220, condição, texto e instante crus | **MOVEU** — a condição usa `respondida`; texto e instante seguem na projeção: [modal:217](app/templates/chamados/partials/modal_fio.html:217). |
| P25 — M4, :228, fase validada por carimbo | **CONFERE** — [modal:228](app/templates/chamados/partials/modal_fio.html:228). |
| P26 — M5, :230–248, estado de aplicação | **CONFERE** — [modal:230](app/templates/chamados/partials/modal_fio.html:230); contexto em [fio.py:376](app/chamados/services/fio.py:376). |
| P27 — M6, antigo :250, filtro de resposta | **MOVEU** — [modal:274](app/templates/chamados/partials/modal_fio.html:274). |
| P28 — L1, lote :24, texto da projeção | **CONFERE** — [lote:24](app/templates/chamados/partials/_lote_preview.html:24). |
| P29 — exceção card :192, via e motivo | **CONFERE** — [card:192](app/templates/chamados/partials/_card_atendimento.html:192). |
| P30 — exceção modal :170–171, conferência | **CONFERE** — [modal:170](app/templates/chamados/partials/modal_fio.html:170). |
| P31 — exceção modal :222–226, pendência e recusa | **CONFERE** — [modal:222](app/templates/chamados/partials/modal_fio.html:222). |
| P32 — exceção modal antigo :379, fechamento da disputa | **MOVEU** — [modal:406](app/templates/chamados/partials/modal_fio.html:406). |

**Contagem dos 32 itens acima:** CONFERE 25, MOVEU 4, CAIU 3. A contagem de todas as premissas do brief e da matriz continua [não verificado].

## 2 O que mudou

O patch anexado compara **520d7b557 com dd489c627**; ele não é um diff da base original do brief, 77dfa94fa. A cópia não tem `.git`, e a comparação independente por Git está [não verificado].

Nos quatro templates, o patch mostra mudanças apenas em `_pergunta_card.html` e `modal_fio.html`: quatro condições antes baseadas em `respondida_em` passaram a `respondida` ([pergunta-card:24](app/templates/chamados/partials/_pergunta_card.html:24), [pergunta-card:29](app/templates/chamados/partials/_pergunta_card.html:29), [modal:185](app/templates/chamados/partials/modal_fio.html:185), [modal:217](app/templates/chamados/partials/modal_fio.html:217)). A propriedade agora chama `pergunta_respondida`, que exige instante **e** texto não vazio da projeção ([models.py:3673](app/chamados/models.py:3673), [juizes.py:1544](app/chamados/juizes.py:1544)). Isso altera os casos de carimbo sem texto, mas não demonstra a leitura da sequência de atos pretendida pela F7.

O patch também altera `estado`: seus ramos de respondida passam pela propriedade, mas a precedência de `validada_em` e `materializada_em` permanece ([models.py:3159](app/chamados/models.py:3159)). Assim, a premissa de que a F2 já teria feito esse estado ler o ato não está demonstrada neste HEAD. O texto formatado continua baseado em `resposta_colab` ([models.py:3699](app/chamados/models.py:3699)); o lote continua mostrando esse campo ([lote:24](app/templates/chamados/partials/_lote_preview.html:24)).

O patch acrescenta as marcas `da_ressuscitacao` e `resposta_mantem_validacao` em `DecisaoPergunta` ([models.py:4065](app/chamados/models.py:4065)). São escritores novos **da marca**: a resposta humana declara `False`, enquanto resposta automática, resposta pela trilha e foto da projeção declaram `True` ([disputa_emissao.py:1526](app/chamados/services/disputa_emissao.py:1526), [disputa_emissao.py:1679](app/chamados/services/disputa_emissao.py:1679), [disputa_emissao.py:1701](app/chamados/services/disputa_emissao.py:1701), [models.py:4139](app/chamados/models.py:4139)). O comentário do modelo informa que o leitor de `resposta_mantem_validacao` ainda não chegou nesse pouso ([models.py:4065](app/chamados/models.py:4065)).

Entre os leitores novos do juiz no patch estão API, validação e recusa, resposta pela view, validação inline, fechamento do fio e seleção de perguntas corrigíveis ([api/views.py:2575](app/api/views.py:2575), [validacao.py:681](app/chamados/services/validacao.py:681), [validacao.py:901](app/chamados/services/validacao.py:901), [views.py:1280](app/chamados/views.py:1280), [views_cobrar.py:169](app/chamados/views_cobrar.py:169), [fio.py:707](app/chamados/services/fio.py:707), [juizes.py:1755](app/chamados/juizes.py:1755)). A enumeração completa de todos os escritores e leitores cruzados dos campos tocados está [não verificado].

A cerca nova intervém nas portas de escrita apenas dentro de atos cercados; o próprio código declara o restante da B2b aberto ([fio.py:1371](app/chamados/services/fio.py:1371), [fio.py:1398](app/chamados/services/fio.py:1398)). O patch também contém mudanças de folha, ausência e feriado; seu efeito sobre a F7 está [não verificado].

## 3 Nao verificado de ALCANCE

- **Cascas do card:** a página inclui o parcial duas vezes e escolhe `base_tpl`; o context processor fornece `base_app.html` ou `base.html` ([meu_atendimento.html:1](app/templates/chamados/meu_atendimento.html:1), [meu_atendimento.html:127](app/templates/chamados/meu_atendimento.html:127), [meu_atendimento.html:144](app/templates/chamados/meu_atendimento.html:144), [context_processors.py:210](app/core/context_processors.py:210)). Isso fecha o caminho estático descrito, não uma execução nas duas cascas.
- **Pergunta-card:** a página de detalhe estende `base.html` e inclui o parcial; a resposta HTMX o renderiza com objeto carregado separadamente, sem o prefetch mostrado no modal ([detalhe_local.html:1](app/templates/chamados/detalhe_local.html:1), [detalhe_local.html:310](app/templates/chamados/detalhe_local.html:310), [views.py:399](app/chamados/views.py:399)).
- **Modal e lote:** estão confirmados o painel em `base.html`, seu alvo do fio e o drawer do lote ([painel_gestao.html:1](app/templates/chamados/painel_gestao.html:1), [painel_gestao.html:260](app/templates/chamados/painel_gestao.html:260), [painel_gestao.html:14](app/templates/chamados/painel_gestao.html:14)). Continua aberto [não verificado] percorrer integralmente os caminhos da fila de benefícios e do retrato e os includes transitivos das duas bases; faltam abrir [fila_solicitacoes_beneficio.html](app/templates/colaboradores/fila_solicitacoes_beneficio.html), [retrato_folha.html](app/templates/folha/retrato_folha.html), [retrato_folha.py](app/folha/retrato_folha.py) e os includes transitivos de [base.html](app/templates/base.html) e [base_app.html](app/templates/base_app.html).
- **Escritor de reabertura:** o sítio confirmado limpa o par e a validação e registra a foto anterior ([disputa_emissao.py:1610](app/chamados/services/disputa_emissao.py:1610), [disputa_emissao.py:1632](app/chamados/services/disputa_emissao.py:1632)). A afirmação do brief sobre a chamada de `linhas_da_projecao_sem_linha` nesse caminho continua [não verificado]: falta examinar todo o corpo de `reabrir_para_correcao` e suas portas chamadas ([disputa_emissao.py:1577](app/chamados/services/disputa_emissao.py:1577)).
- **Leitores cruzados:** `leitura_da_resposta` lê a projeção, e `aplicacao_da_resposta` também a consulta ([chamados_tags.py:145](app/chamados/templatetags/chamados_tags.py:145), [chamados_tags.py:131](app/chamados/templatetags/chamados_tags.py:131)). A lista completa de leitores e escritores de `resposta_colab`, `respondida_em`, `validada_em` e das funções alcançadas continua [não verificado]; falta um censo da árvore inteira, incluindo chamadas indiretas e consultas ORM.
- **Contrato F1 antigo:** o anexo descreve `atos_pergunta.leitura`, mas a implementação desse contrato histórico não foi encontrada na cópia; sua porta efetiva e seus leitores em dd489c627 continuam [não verificado]. Falta a árvore ou o conteúdo da implementação que se pretende pousar.
- **Frota U1/U2:** permanece [não verificado]; os dados fornecidos não trazem uma medição dessa frota.

alcance nao verificado restante: 5

## 4 Perguntas de lei

**DECIDIDO PELA LEI L-130:** “respondida” exige instante e texto não vazio; a lei também trata do par na reabertura e ressuscitação ([LEIS.md:151](app/docs/LEIS.md:151)). A conferência A20 para o par conferido está decidida pelo corte **LU-08** ([CORTES.md:487](app/docs/CORTES.md:487)). A conservação das fases e a exigibilidade aditiva constam do registro **E4C-D04-D05** ([BACKLOG.md:2137](app/docs/BACKLOG.md:2137)).

**Candidata ainda não resolvida nesta leitura:** no caso R1b descrito no brief, quando há validação projetada sem linha correspondente, a F7 deve mostrar a validação pela porta conservada ou omiti-la pela leitura pura? A função que identifica linhas ausentes existe ([models.py:4122](app/chamados/models.py:4122)), mas a aplicação das duas portas candidatas à F7 e uma resposta específica nos documentos de lei estão [não verificado]. Logo, não é possível afirmar que essa candidata seja uma pergunta de lei efetivamente aberta.

A enumeração integral de perguntas de lei da F7 permanece [não verificado]; não há base suficiente nesta leitura para fornecer o número exigido sem apresentar uma contagem como completa indevidamente.

## 18/11 09:09 · sessao aaf420912c18 · - · - · origem=Ronald (tmux)

A análise foi comunicada ao agente principal, incluindo a serialização e comparação do espelho inteiro em [test_atestado_por_minutos.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/tests/test_atestado_por_minutos.py:104), os leitores por chave nomeada e as chaves aninhadas relacionadas às possíveis colisões.

## 18/11 09:09 · sessao 3ff84be040b9 · - · - · origem=Ronald (tmux)

Resultados enviados ao agente principal, com referências aos leitores de templates, testes, comandos e à cópia do dia inteiro. A análise universal de iteração em JavaScript de terceiros minificado permanece **[não verificado]**. [nao verificado]

## 18/11 09:09 · sessao 3dc31b9bf024 · - · - · origem=Ronald (tmux)

Chamadores diretos encontrados de `ponto.services.dia_decidido.do_dia`:

- Produção: [espelho.py:328](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/espelho.py:328), chamada em [linha 340](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/espelho.py:340); `_r` fornece as três chaves individualmente em [linha 344](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/espelho.py:344).
- Produção: [calendario.py:738](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:738), chamada pelo apelido `_do_dia` em [linha 748](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:748); `_dd` fornece as três chaves individualmente em [linha 757](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:757).
- Testes: cinco chamadas em [test_palavra_parcial_e_pendente.py:74](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/tests/test_palavra_parcial_e_pendente.py:74), [linha 82](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/tests/test_palavra_parcial_e_pendente.py:82), [linha 83](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/tests/test_palavra_parcial_e_pendente.py:83), [linha 92](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/tests/test_palavra_parcial_e_pendente.py:92) e [linha 98](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/tests/test_palavra_parcial_e_pendente.py:98). Cada retorno é lido por chave; a comparação da linha 99 compara uma tupla dos três valores, não o dicionário inteiro. Chaves adicionais não alteram esses acessos.

## 18/11 09:08 · sessao 323bb8be610c · - · - · origem=Ronald (tmux)

Achados de alcance e escritores novos enviados ao agente principal, com os sítios atuais. O comportamento completo de U6 para uma resposta futura ainda depende do juiz de vitalidade e de uma fixture; o filtro de resposta em si não recebe `agora` ([calendario.py:503](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:503), [juizes.py:1575](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1575)).

## 18/11 09:08 · sessao 2f7652246931 · - · - · origem=Ronald (tmux)

A pergunta de lei F8 sobre validação legada foi **DECIDIDA por CORTES-F2-F9-1611**: a validação permanece quando uma resposta sintética posterior é criada pelo backfill. O corte pede medição na sombra contra a F1 que pousar; essa medição não consta do dado fornecido. [app/docs/CORTES.md:509](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/CORTES.md:509)

A L-130 define pergunta respondida como instante e texto não vazio após trim. [app/docs/LEIS.md:151](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/LEIS.md:151)

O alcance de `respostas_sem_veredito` foi fechado: a ficha chama o método, que delega a `sem_veredito_entre`; este usa `pergunta_respondida`, cujo corpo lê os campos da projeção. [ficha.py:169](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/ficha.py:169), [models.py:1898](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1898), [models.py:1927](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1927), [juizes.py:1544](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1544)

Há testes de custo da montagem do calendário, mas os trechos examinados não demonstram custo constante ao variar o número de perguntas carimbadas. [test_e4cal_l1_calendario_le_o_d4.py:230](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:230), [test_calendario_hoje_nao_e_falta.py:180](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:180)

## 18/11 09:08 · sessao 5b357bcf55bd · E4C-F2-P2 · censo · origem=Code

**A. FECHADO — as 13 linhas dinâmicas**

- `F(campo)`: **não** alcança os campos-alvo; `_campo` recebe apenas campos temporais de `CelulaDia`. As leituras de `PerguntaDisputa` nesse método usam `_ate`, não `_campo`. [app/chamados/veredito_lavrado.py:166; app/chamados/veredito_lavrado.py:191; app/chamados/veredito_lavrado.py:201]
- `getattr(self, c)`: **não**; lê campos de `ChamadoColaborador`, limitados a `urgencia` e `urgencia_original`. [app/chamados/models.py:460; app/chamados/models.py:474; app/chamados/models.py:638]
- `getattr(p, campo)` no histórico: **sim, leitura de `PerguntaDisputa`**: `respondida_em`, `validada_em` e `materializada_em`. Os nomes constam como `STR` no TSV. [app/chamados/services/historico.py:96; app/chamados/services/historico.py:99; app/chamados/services/historico.py:105]
- `getattr(p, c)` na reversão: **sim, leitura de `PerguntaDisputa`**: `materializada_em`, `materializada_por_id`, `via_resolucao`, `respondida_em`, `validada_em` e `validada_por_id`. Os nomes constam como `STR` no TSV; a própria chamada consta como `DINAMICO`. [app/chamados/services/passivo_validado.py:64; app/chamados/services/passivo_validado.py:81; app/chamados/services/passivo_validado.py:370]
- `getattr(self, campo)` das etapas: **não**; lê `camera_concedida_em` ou `gps_concedido_em` de `Colaborador`. [app/colaboradores/models.py:406; app/colaboradores/models.py:413]
- `hasattr`, `setattr` e `getattr` de `situacao_exibida`, respectivamente as linhas **456, 457 e 458**: **não**; consultam ou atribuem o cache `_situacao_exibida` em `Colaborador`. [app/colaboradores/models.py:444; app/colaboradores/models.py:456; app/colaboradores/models.py:457; app/colaboradores/models.py:458; app/colaboradores/services/situacao_exibida.py:34]
- `setattr(colab, k, v)`: **não** escreve os modelos-alvo; a porta grava campos de `Colaborador` e confere os nomes permitidos para a operação. [app/colaboradores/services/colaborador.py:110; app/colaboradores/services/colaborador.py:126; app/colaboradores/services/colaborador.py:135]
- `getattr(fech, campo)` em `_gravado_x_grade`: **não**; o nome é limitado aos três campos de minutos do fechamento. [app/folha/porta_export.py:135; app/folha/porta_export.py:143]
- `getattr(fech, _campo)` em `_medir_dia_pago`: **não** lê os modelos-alvo; lê o fechamento com chaves vindas da soma de dia pago. [app/folha/porta_export.py:332; app/folha/porta_export.py:388; app/folha/porta_export.py:451]
- `values(*chave)` e `values_list(*chave)`, respectivamente linhas **2112 e 2122**: **não** leem campos-alvo; as chaves de `PerguntaDisputa` são as das famílias de marco e batida. [app/chamados/services/fio.py:1515; app/chamados/services/fio.py:2109; app/chamados/services/fio.py:2112; app/chamados/services/fio.py:2122]

**B. ABERTO.** Busquei em `app/` os nomes dos dois modelos e dos campos em arquivos de forms, serializers, signals e `admin.py`, além de comandos de management; busquei também `RawSQL`, `.extra(`, `.raw(`, `cursor(` e `.execute(` nos Python de `app/`, excluindo testes e migrations. A busca identificou dois receivers de `PerguntaDisputa`: `_becos_post_save_pergunta`, **fora das linhas de campos do TSV e nomeado aqui**, e `_outbox_post_save_pergunta`, cuja chamada aparece como `CALL` no TSV; este último consulta a lista de campos que acordam células. [app/chamados/signals.py:229; app/chamados/signals.py:252; app/chamados/signals.py:257; app/chamados/signals.py:259; app/chamados/models.py:3339]

Nos comandos, por exemplo, os filtros de `encerrar_perguntas_com_fato` e o `ato_motivo` de `backfill_decisoes_pergunta` aparecem no TSV como `KW`; as leituras por atributo desses comandos aparecem como `ATTR_LOAD`. [app/chamados/management/commands/encerrar_perguntas_com_fato.py:31; app/chamados/management/commands/backfill_decisoes_pergunta.py:324]

A busca revelou ainda código fora de `app/`, não abrangido pelo TSV fornecido: `bin/censo_folha.py` lê `respondida_em` e `validada_em`; `bin/sombra_regras_pessoais.py` lista `resposta_colab` e `ato_motivo` para tratamento genérico. Estão nominados aqui, mas não concluí a auditoria de todos os caminhos genéricos dessa rotina nem de toda a árvore fora de `app/`; por isso não afirmo que nenhum leitor ou escritor ficou fora da lista. [bin/censo_folha.py:28; bin/censo_folha.py:29; bin/sombra_regras_pessoais.py:81; bin/sombra_regras_pessoais.py:142]

**C. ABERTO.** As chamadas diretas abaixo coincidem com as linhas `CALL` fornecidas:

- `descarimbar_em_lote`: [app/chamados/models.py:1637], [app/chamados/services/disputa_emissao.py:1649], [app/chamados/services/validacao.py:440].
- `vigiar_escrita_de_perguntas`: imports locais e chamadas em [app/chamados/models.py:3398] e [app/chamados/models.py:3565].
- `ressuscitar_perguntas`: [app/chamados/models.py:1498], [app/chamados/views_arquivar.py:86], [app/ponto/services/cartorio.py:1674].
- `_universo_da_ressuscitacao`: [app/chamados/models.py:1507], [app/chamados/models.py:1512], [app/chamados/models.py:1599].
- `executar_sob_cerca`: [app/chamados/reconciliador.py:553], [app/chamados/reconciliador.py:1701], [app/chamados/services/becos.py:66], [app/chamados/services/disputa_emissao.py:1912].
- `linhas_da_projecao_sem_linha`: [app/chamados/management/commands/backfill_decisoes_pergunta.py:67], [app/chamados/management/commands/backfill_fio_morto_e4b.py:141], [app/chamados/models.py:1619], [app/chamados/models.py:3137], [app/chamados/services/disputa_emissao.py:1480], [app/chamados/services/disputa_emissao.py:1674], [app/chamados/services/tarefa_do_dia.py:436], [app/chamados/services/validacao.py:167], [app/chamados/services/validacao.py:272], [app/chamados/services/validacao.py:345], [app/chamados/services/validacao.py:436].
- `DecisaoPergunta.registrar`: [app/chamados/management/commands/backfill_decisoes_pergunta.py:324], [app/chamados/management/commands/backfill_fio_morto_e4b.py:141], [app/chamados/models.py:1619], [app/chamados/models.py:2013], [app/chamados/models.py:2145], [app/chamados/models.py:3137], [app/chamados/services/disputa_emissao.py:1525], [app/chamados/services/disputa_emissao.py:1632], [app/chamados/services/disputa_emissao.py:1679], [app/chamados/services/disputa_emissao.py:1701], [app/chamados/services/materializacao.py:1048], [app/chamados/services/validacao.py:173], [app/chamados/services/validacao.py:279], [app/chamados/services/validacao.py:354], [app/chamados/services/validacao.py:439], [app/ponto/services/veredito_celula.py:195].

Não concluí a prova exaustiva de aliases, referências passadas como argumento, `getattr` por string e reexportações; portanto, não posso afirmar que não haja chamador indireto fora do TSV. [nao verificado]

**D. FECHADO — pre-existente, nao aberto pelo diff.** No HEAD, a ressuscitação já seleciona IDs antes da trava e o lote já exclui as vias fora do fato no `UPDATE`; o comando também já seleciona instâncias antes de gravá-las sem a trava do colaborador. A opção A, conforme descrita, fixa um subconjunto sob a trava para suas escritas, mas não cria a exposição ao escritor que usa a instância previamente carregada. [app/chamados/models.py:1599; app/chamados/models.py:1618; app/chamados/models.py:3404; app/chamados/management/commands/encerrar_perguntas_com_fato.py:41; app/chamados/management/commands/encerrar_perguntas_com_fato.py:65; app/chamados/models.py:3413]

`fato_materializado` **não está** em `VIAS_FORA_DO_FATO`: essa tupla contém apenas `duplicata` e `motivo_retratado`. [app/chamados/models.py:2506; app/chamados/models.py:2606]

censo fechado: nao -- auditoria de caminhos genericos fora de app e de chamadores indiretos permanece aberta.

## 18/11 09:08 · sessao 2a19390e677a · - · - · origem=Ronald (tmux)

Conferência dos sítios U1–U9 enviada ao agente principal, com linhas atuais, mudanças de leitura e a ressalva sobre a idempotência de U5d.

## 18/11 09:07 · sessao a2125936859d · E4C-F2-P2 · sonda · origem=Code

```text
DB saas_hasner
MIGR ['0055_decisao_resposta_mantem_validacao', '0054_decisao_da_ressuscitacao', '0053_pergunta_unica_por_familia']
DEN total_perguntas 51291 | fonte PerguntaDisputa.objects.count()
DEN validada_em_nulo 35683 | fonte filter(validada_em__isnull=True)
DEN com_algum_ato 2172 | fonte filter(decisoes__isnull=False).distinct()
DEN com_ato_VALIDACAO 566 | fonte filter(decisoes__ato=VALIDACAO).distinct()
N1 0 de 35683 (validada_em nulo)
N2 0 de 0 (N1)
N2_PKS []
CORRIDA_BASELINE 0 de 2172 (perguntas com algum ato; universo do aval = 0 antes do reload da P2)
CORRIDA_PKS []
```

## 18/11 09:06 · sessao 1c86d37464f2 · ESPINHA-REMEDIDA-1811 · censo · origem=Code

## 1 Premissas e sitios

**A remedição está incompleta.** Julguei os sítios centrais abaixo, mas não conferi individualmente todas as premissas do brief e da matriz. Por isso, as contagens totais CONFERE/MOVEU/CAIU e o total definitivo de perguntas de lei ficam **[nao verificado]**.

Retifico também minha atualização anterior: citei `app/docs/CORTES.md:560` para o corte posterior sobre silêncio sem tê-lo confirmado nessa linha. O corte está transcrito no anexo F6, mas sua localização na árvore fornecida é **[nao verificado]**. O código atual ainda retorna `silencio_decidido` sem ações. ([app/chamados/services/tarefa_do_dia.py:176](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:176))

### F6

Na tabela, **MOVEU** indica mudança de implementação ou de linha; não significa, por si só, que o destino proposto pelo brief tenha sido entregue.

| Premissa e sítio antigo | Julgamento em dd489c627 |
|---|---|
| U1a–c, brief:15, matriz:32–34: texto, instante e dias da mensageria | **CONFERE** a leitura projetada e o relógio no serializer; **CAIU** o contrato antigo `LeituraDosAtos` como interface demonstrada nesta cópia. A fila atual exige instante e texto, portanto P5 não chega ao serializer por essa fila. ([views_mensageria.py:567](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views_mensageria.py:567), [juizes.py:1761](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1761)) |
| U2, brief:16, matriz:35: JSON de chamados/v2 | **CONFERE** a leitura de texto e instante da projeção. ([views.py:1163](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:1163)) |
| U3, brief:17, matriz:36: JSON de disputa | **CONFERE** a leitura projetada de texto e instante. ([views.py:2561](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:2561)) |
| U4, brief:18, matriz:37: `aguarda_admin` deriva de `estado == RESPONDIDA` | **CONFERE**; a saída ainda não consulta diretamente a tarefa do dia. ([views.py:2553](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:2553)) |
| U5, brief:19, matriz:38: filtro apenas por instante | **MOVEU** para o juiz de respondida. ([views.py:2578](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:2578)) |
| U6, brief:20, matriz:39: `ja_respondida` apenas por instante | **MOVEU** para o juiz; o instante da resposta ainda é serializado depois da porta. ([views.py:2622](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:2622), [views.py:2639](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:2639)) |
| U7a/b, brief:21, matriz:40–41: seleção por instante | **MOVEU** para o juiz e sua face SQL negada. O segundo filtro continua partindo de todas as perguntas da disputa, sem filtro de vitalidade nesse trecho. ([views_cobrar.py:170](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views_cobrar.py:170), [views_cobrar.py:178](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views_cobrar.py:178)) |
| U8, brief:22, matriz:42: texto projetado na proposta | **MOVEU** de linha; a leitura continua. ([views_cobrar.py:293](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views_cobrar.py:293)) |
| U9a, brief:23, matriz:43: `ja_respondida` por instante | **MOVEU** para o juiz. ([views.py:1281](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views.py:1281)) |
| U9b, brief:33–35, matriz:44: ramo materializada e não viva é inalcançável | **CONFERE** por leitura estática: a guarda anterior elimina a pergunta não viva; o ramo continua presente. A resposta web é erro/redirecionamento; o 404 explícito é da API. ([views.py:1242](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views.py:1242), [views.py:1257](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views.py:1257), [views.py:2619](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:2619)) |
| U10, brief:24, matriz:45: pendentes por instante nulo | **MOVEU** para a face negada e depois filtra vivas; corrigíveis usam a face positiva. ([views.py:1418](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views.py:1418), [juizes.py:1753](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1753)) |
| U11, brief:25, matriz:46: canal por string vazia literal | **MOVEU** para `q_nao_respondida()`. ([canal.py:64](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/canal.py:64)) |
| U12, brief:26, matriz:47: tag lê texto projetado | **MOVEU** de linha; a leitura continua. O retorno da tag é a estrutura `impl`/`fr`, não diretamente o texto “X” ou “sem texto” pressuposto no RED antigo. ([chamados_tags.py:166](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/templatetags/chamados_tags.py:166)) |

S1/S2 e as demais premissas gerais, provas, exceções e transições do brief e da matriz ainda precisam de julgamento individual. **CONFERE a, MOVEU b, CAIU c: [nao verificado]**.

### F7

| Premissa e sítio antigo | Julgamento em dd489c627 |
|---|---|
| C1/C2/C9/C12, matriz: card recebe `viva` e `resposta_enviada` do contexto | **CONFERE** a flag fornecida pelo montador; o predicado usa instante e o juiz de resposta sem texto. ([fio.py:178](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:178), [fio.py:360](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:360)) |
| C3–C5, C7–C8, C10–C12: apresentação e prefill do card leem projeção | **CONFERE** nos sítios principais: texto/data, radio, input, select, histórico e chip continuam presentes. ([\_card_atendimento.html:106](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/_card_atendimento.html:106), [\_card_atendimento.html:114](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/_card_atendimento.html:114), [\_card_atendimento.html:148](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/_card_atendimento.html:148), [\_card_atendimento.html:154](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/_card_atendimento.html:154), [\_card_atendimento.html:182](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/_card_atendimento.html:182), [\_card_atendimento.html:391](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/_card_atendimento.html:391)) |
| P0, matriz: detalhe e fragmento sem prefetch explícito de decisões | **CONFERE** nos dois trechos. ([acoes_chamado.py:352](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/acoes_chamado.py:352), [views.py:399](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views.py:399)) |
| P1/P2/P6: validação projetada controla aparência e gate | **CONFERE**. ([\_pergunta_card.html:20](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/_pergunta_card.html:20), [\_pergunta_card.html:42](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/_pergunta_card.html:42)) |
| P3/P4: os dois gates eram `respondida_em` | **MOVEU** para `perg.respondida`, que delega ao juiz. A data exibida continua projetada. ([\_pergunta_card.html:24](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/_pergunta_card.html:24), [models.py:3674](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3674)) |
| P7/M6: filtro compartilhado | **CONFERE** no card; no modal **MOVEU** de `:250` para `:274`. ([\_pergunta_card.html:43](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/_pergunta_card.html:43), [modal_fio.html:274](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/modal_fio.html:274)) |
| M0: modal tem prefetch de decisões | **MOVEU** de `fio.py:261` para `:264`. ([fio.py:264](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:264)) |
| M1/M3: gates eram `respondida_em` | **MOVEU** para `perg.respondida`; texto e data seguem projetados. ([modal_fio.html:185](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/modal_fio.html:185), [modal_fio.html:217](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/modal_fio.html:217)) |
| M4 e L1: validação projetada no modal e resposta projetada no lote | **CONFERE**. ([modal_fio.html:228](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/modal_fio.html:228), [\_lote_preview.html:24](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/_lote_preview.html:24)) |
| A03 no modal, brief `:379` | **MOVEU** para `:406`. ([modal_fio.html:406](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/modal_fio.html:406)) |

C6, C11, P5, M2, M5, as demais exceções e as premissas de contrato, provas e estados ainda precisam de julgamento individual. **CONFERE a, MOVEU b, CAIU c: [nao verificado]**.

## 2 O que mudou

### F6

O patch fornecido de `fbc5c76ce` para `dd489c627` mostra que `respondidas`, os dois `ja_respondida`, a seleção de validação, o canal e a seleção de reabertura passaram ao juiz ou às suas faces SQL. O juiz exige **instante e texto não vazio**; sua face negada inclui ausência de instante ou instante sem texto. Isso altera os casos de meio par em relação aos filtros antigos. ([juizes.py:1544](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1544), [juizes.py:1583](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1583), [views_cobrar.py:170](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views_cobrar.py:170))

Novos leitores no delta incluem `perguntas_corrigiveis`, `recobrar_questionario` e o universo de ressuscitação; este último exclui texto sem instante e sem validação para não apagá-lo sem uma linha que o conserve. ([juizes.py:1753](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1753), [disputa_emissao.py:1737](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1737), [models.py:1533](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1533))

O delta também acrescenta as marcas `da_ressuscitacao` e `resposta_mantem_validacao`: os escritores de resposta humana, automática e pela trilha declaram valores diferentes para a segunda; a carga da tarefa consulta a primeira. Isso **não demonstra** que os leitores da F6 passaram a exibir texto e instante de um redutor de atos. ([models.py:4067](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4067), [disputa_emissao.py:1527](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1527), [disputa_emissao.py:1681](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1681), [tarefa_do_dia.py:427](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:427))

### F7

O patch de `520d7b557` para `dd489c627` troca quatro gates nos dois templates para `perg.respondida`; a property passou a delegar ao juiz. Os campos de texto, instante e validação identificados na seção 1 continuam em uso. ([models.py:3674](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3674), [\_pergunta_card.html:24](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/_pergunta_card.html:24), [modal_fio.html:217](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/modal_fio.html:217))

O mesmo patch altera leitores em `fio.py` e `validacao.py`, incluindo a escolha da pergunta canônica e as guardas de validar e recusar, e acrescenta a porta para o estoque L-130. O inventário completo de novos escritores e leitores de todos os campos tocados pelas duas fatias permanece **[nao verificado]**. ([fio.py:1283](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fio.py:1283), [validacao.py:681](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/validacao.py:681), [validacao.py:901](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/validacao.py:901))

## 3 Nao verificado de ALCANCE

### F6

A antiga dúvida sobre a partição do app fecha quanto ao **código atual**: ela consulta `pergunta_respondida` e a disputa aberta. A atribuição organizacional à F5 pelo brief próprio continua **[nao verificado]**. ([pergunta_no_app.py:79](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/pergunta_no_app.py:79))

Continuam abertos o inventário integral de leitores e escritores, acessos dinâmicos, manifestos futuros de arquivos cruzados e consumidores externos efetivos do JSON; falta concluir a busca por referências e conferir os clientes externos disponíveis. **[nao verificado]**

### F7

O card de atendimento chega às duas cascas: a página o inclui e escolhe `base_tpl`, que o context processor define conforme `e_app`. O detalhe inclui o outro card e estende `base.html`; o fragmento também é renderizado diretamente pela view. Isso fecha esses caminhos específicos, mas não completa a auditoria de todos os includes e leitores cruzados. ([meu_atendimento.html:1](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/meu_atendimento.html:1), [meu_atendimento.html:127](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/meu_atendimento.html:127), [context_processors.py:210](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/core/context_processors.py:210), [detalhe_local.html:1](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/detalhe_local.html:1), [views.py:404](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views.py:404))

**alcance nao verificado restante: [nao verificado]** — a contagem requer terminar o levantamento integral; os itens acima não constituem uma contagem fechada.

## 4 Perguntas de lei

### F6

- **DECIDIDO PELA LEI L-130:** texto sem instante não é respondida, assim como instante sem texto; o juiz é único para as finalidades enumeradas na lei. ([LEIS.md:151](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/LEIS.md:151))
- **DECIDIDO PELO CORTE CORTES-F2-F9-1611, item 6:** os estados omitidos da matriz usam o dono declarado pela tarefa, sem tabela própria no leitor. ([CORTES.md:509](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/CORTES.md:509))
- **DECIDIDO PELO CORTE COMPLEMENTO-1-CORTES-F2-F9-1611, item 3:** o registro documental diz que o app aceita `respondida_em` opcional e não lê `dias_esperando`. A implementação Android independente permanece **[nao verificado]**. ([CORTES.md:512](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/CORTES.md:512))
- **Pergunta herdada ainda aberta:** resposta vazia/ressuscitada deve voltar ao colaborador? A lei a registra como não decidida; o corte distingue essa pergunta do tratamento já determinado para a F5. ([LEIS.md:151](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/LEIS.md:151), [CORTES.md:509](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/CORTES.md:509))

F6: **1 pergunta herdada identificada**; total definitivo da fatia **[nao verificado]**, pois a revisão de todas as premissas não terminou.

### F7

A classificação de todas as candidatas de F7 permanece **[nao verificado]**. Em particular, não concluí a verificação de todas as premissas sobre a escolha entre leitura pura e conservação, nem sua relação com as decisões posteriores.

F7: **[nao verificado]**. Total F6 + F7: **[nao verificado]**.

perguntas de lei: [nao verificado]

## 18/11 09:06 · sessao 2dc2a33c16b9 · ESPINHA-REMEDIDA-1811 · censo · origem=Code

## 1 Premissas e sitios

A identificação independente da cópia como `dd489c627` continua **[nao verificado]**: a árvore foi fornecida sem `.git`. As referências a `brief` e `matriz` abaixo são as linhas numeradas na remedição anexada; a correspondência delas com arquivos externos originais é **[nao verificado]**.

A revisão abaixo não cobre todas as premissas e todos os degraus pedidos. Por isso, não seria correto apresentar uma contagem fechada de CONFERE, MOVEU e CAIU.

| Premissa ou sítio dos anexos | Julgamento na cópia |
|---|---|
| D01 conservado e SQL-R lidos da sequência de atos — brief:28–29; matriz:22–23 | **CAIU.** `pergunta_respondida` e sua face SQL leem instante e texto da projeção. [juizes.py:1544](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1544), [juizes.py:1575](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1575) |
| Grade filtra apenas `respondida_em` — brief:84; matriz D01:33 | **CAIU.** A grade usa `q_respondida_com_texto()` antes de `fio_vivo`. O RED antigo que exige prender uma pergunta com resposta apenas no ato não decorre dessa implementação. [reconciliar_grade.py:460](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/management/commands/reconciliar_grade.py:460), [juizes.py:1552](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1552) |
| I21a testa só instante — brief:37; matriz D22:71 | **CAIU.** A contraprova chama `pergunta_respondida`. [auditar_invariantes_chamados.py:176](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:176) |
| Piso I2 testa só instante — brief:41; matriz D26:75 | **CAIU.** O piso chama o mesmo juiz. [auditar_invariantes_chamados.py:592](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:592) |
| I21 usa R cru — brief:46; matriz D32:81 | **CAIU.** I21 usa a face SQL de respondida; V e C continuam em filtros projetados. [auditar_invariantes_chamados.py:1576](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:1576) |
| C2 exige R nulo — brief:47; matriz D33:82 | **CAIU.** C2 usa a negação SQL do juiz de resposta e mantém V/C projetados. [auditar_invariantes_chamados.py:1605](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:1605) |
| F1 testa texto isolado — brief:48; matriz D34:83 | **CAIU.** O ramo chama `pergunta_respondida`. [auditar_invariantes_chamados.py:1657](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:1657) |
| Dedup rotula pela presença de texto — brief:55; matriz D14:59 | **CAIU.** `com_resp` usa `pergunta_respondida`. [dedup_perguntas_fato.py:76](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/dedup_perguntas_fato.py:76) |
| Reabrir órfãs, sua porta e colisão com RELAVRA-11 — brief:42–44,114–118; matriz D07–D10b:47–51 | **CAIU.** A lápide registra a saída do comando e de `reabrir_fantasma`; os degraus e a colisão textual pressupostos deixam de ser sítios vivos da cópia. [crons.py:835](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/config/crons.py:835), [models.py:3429](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3429) |
| Limbo é reparo da projeção com guarda nominal — brief:54,138–140; matriz D11–D13 e P11:54–56,118–119 | **CONFERE.** O filtro é texto com instante nulo e o comando recusa quando o conjunto difere de `MEDIDAS`. [curar_perguntas_em_limbo.py:5](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/curar_perguntas_em_limbo.py:5), [curar_perguntas_em_limbo.py:46](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/curar_perguntas_em_limbo.py:46) |
| I18 usa V e autor da projeção — matriz D29:78 | **MOVEU** para [auditar_invariantes_chamados.py:1077](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:1077): o filtro usa `validada_em` e exclui por `validada_por__username`. |
| F2/F3 pré-filtram V projetada — matriz D35–D36:84–85 | **MOVEU** para [auditar_invariantes_chamados.py:1770](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:1770) e [auditar_invariantes_chamados.py:1784](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:1784). |
| `universo_i6` devolve pares `(pk, materializada_em)` — matriz P8:111 | **CAIU.** Os pares são dados internos da consulta; o retorno é conjunto de violações e dicionário de pisos. [auditar_invariantes_chamados.py:471](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:471), [auditar_invariantes_chamados.py:485](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:485) |
| Existe selo parcial de vitalidade, sem cobertura geral de R — matriz:14–19 | **CONFERE.** O selo declara apenas C/V e seu walker examina formas específicas de comparação, `__isnull` e condição. [test_contract_leitores_vitalidade.py:14](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_contract_leitores_vitalidade.py:14), [test_contract_leitores_vitalidade.py:313](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_contract_leitores_vitalidade.py:313) |
| Três crons e auditoria sem persistência — brief:141–146; matriz P10:116–117 | **CAIU parcialmente.** As três agendas conferem, mas o auditor com `--html` grava arquivo. [crons.py:392](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/config/crons.py:392), [crons.py:462](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/config/crons.py:462), [crons.py:542](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/config/crons.py:542), [auditar_invariantes_chamados.py:1859](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:1859) |

**CONFERE a, MOVEU b, CAIU c: [nao verificado]** para o conjunto completo. Faltam o julgamento individual dos demais sítios do brief, D01–D36/D10b e contratos P1–P12, inclusive provas e destinos propostos.

## 2 O que mudou

Li o conteúdo do `diff_F9.patch` fornecido no pedido. A aplicação exata do patch entre os dois hashes e a identidade dos objetos Git permanecem **[nao verificado]**, pois não há `.git` na cópia.

- **Grade:** o patch troca o filtro por instante pela face SQL de L-130. Isso já resolve, no código atual, o caso da remedição anterior em que instante sem texto prendia a órfã como respondida. [reconciliar_grade.py:460](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/management/commands/reconciliar_grade.py:460), [juizes.py:1575](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1575)
- **Backfill:** o patch acrescenta `respondida_em` ao `only()` usado na escolha da canônica; a leitura de resposta deixa de depender apenas do texto carregado nessa consulta. [backfill_decisoes_pergunta.py:504](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/backfill_decisoes_pergunta.py:504)
- **Juiz e modelos:** nasce `q_nao_respondida`, usada, por exemplo, na ressuscitação; a propriedade `respondida` delega ao juiz. O patch também introduz as marcas `da_ressuscitacao` e `resposta_mantem_validacao`. [juizes.py:1583](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1583), [models.py:1533](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1533), [models.py:3674](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3674), [models.py:4071](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4071)
- **Escritores e leitores novos identificados dessas marcas:** a resposta humana declara `resposta_mantem_validacao=False`; respostas automática e pela trilha declaram `True`; a fotografia da projeção também declara `True`. A tarefa lê `es_da_ressuscitacao`. [disputa_emissao.py:1527](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1527), [disputa_emissao.py:1681](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1681), [disputa_emissao.py:1703](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/disputa_emissao.py:1703), [models.py:4143](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4143), [tarefa_do_dia.py:427](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:427)
- **Materialização:** o patch passa a consultar o juiz de resposta e reúne a guarda de anuência dos três tipos de resposta. A seleção distingue respondida com texto de instante sem texto, que entra para recusa. [materializacao.py:871](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:871), [materializacao.py:1269](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1269), [materializacao.py:2090](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:2090)
- **Fantasmas:** o patch acrescenta tratamento e contador de recusa por competência entregue; o trecho de exibição de C continua existente. [reconciliar_fantasmas.py:213](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/management/commands/reconciliar_fantasmas.py:213), [reconciliar_fantasmas.py:274](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/management/commands/reconciliar_fantasmas.py:274)
- **Selo e retirada da reabertura:** o patch acrescenta uma entrada à allowlist e retira a entrada do comando extinto. A lápide no modelo confirma a retirada da porta. [test_contract_leitores_vitalidade.py:16](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_contract_leitores_vitalidade.py:16), [test_contract_leitores_vitalidade.py:177](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_contract_leitores_vitalidade.py:177), [models.py:3429](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:3429)

Há uma **inconsistência a resolver**: a remedição anexada atribui à base `fbc5c76ce` leituras antigas em I21a, I2, C2, F1 e dedup, enquanto a cópia tem juízes nesses sítios e o patch fornecido não traz hunks desses dois arquivos. Não há evidência suficiente aqui para atribuir essas mudanças a um commit do intervalo nem para declarar qual retrato histórico está errado. [auditar_invariantes_chamados.py:176](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:176), [auditar_invariantes_chamados.py:592](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/auditar_invariantes_chamados.py:592), [dedup_perguntas_fato.py:76](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/dedup_perguntas_fato.py:76); histórico **[nao verificado]**.

## 3 Nao verificado de ALCANCE

Foram fechados estes vínculos pontuais:

- `universo_i6` é chamado por `relavrar_via_i6`; o consumidor desempacota o retorno real. [relavrar_via_i6.py:47](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/relavrar_via_i6.py:47), [relavrar_via_i6.py:49](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/relavrar_via_i6.py:49)
- Dedup importa `aplicar` e `CONTADORES_DO_APPLY` do backfill. [dedup_perguntas_fato.py:105](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/management/commands/dedup_perguntas_fato.py:105)
- A worklist importa e usa `_parse_pendentes` e `MODULOS_ALVO`. [worklist.py:37](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/worklist.py:37)
- O selo atual exclui `models.py` e `juizes.py` do percurso e não constitui, sozinho, censo integral dos campos RVC. [test_contract_leitores_vitalidade.py:14](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_contract_leitores_vitalidade.py:14), [test_contract_leitores_vitalidade.py:323](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_contract_leitores_vitalidade.py:323)

Continuam abertos **[nao verificado]**: o inventário integral de escritores e leitores RVC/atos, inclusive ambas as cascas; chamadas dinâmicas; todas as interseções de arquivos entre fatias; e a revisão de cada ponto de alcance assinalado no brief e na matriz. Falta um censo completo da árvore e a reconciliação de cada ocorrência com os anexos.

**alcance nao verificado restante: [nao verificado]** — não foi possível contar com segurança os pontos individuais ainda abertos.

## 4 Perguntas de lei

A busca nos documentos de lei encontrou respostas já registradas para parte das perguntas antigas:

- **DECIDIDO PELA LEI L-130:** respondida exige instante e texto não vazio; os dois meios-pares são ilegais. [LEIS.md:151](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/LEIS.md:151)
- **DECIDIDO PELA LEI CORTES-F2-F9-1611:** a face SQL de validação é face do juiz registrado, condicionada ao selo de equivalência; o corte também mantém guardas de reparo da projeção na projeção. [CORTES.md:509](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/CORTES.md:509)
- **DECIDIDO PELA LEI LU-08:** a conferência vale para o par conferido. [CORTES.md:487](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/CORTES.md:487)
- **DECIDIDO PELA LEI L-111:** a regra registrada trata da retirada de função que perdeu o último chamador de produção. [LEIS.md:137](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/LEIS.md:137)

A revisão foi parcial: **zero perguntas novas foi identificado nos pontos acima, mas a contagem integral de perguntas de lei da F9 continua [nao verificado]**. Em particular, a existência de texto sem instante na ressuscitação está protegida por uma exceção no código; sua decisão de lei e sua pertinência específica à F9 não foram fechadas nesta revisão. [models.py:1538](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1538)

perguntas de lei: 0

## 18/11 09:06 · sessao 96825741f0ad · ESPINHA-REMEDIDA-1811 · censo · origem=Code

## 1 Premissas e sitios

**A remedição abaixo é parcial:** não consegui julgar todas as premissas do brief e da matriz nem fechar uma contagem defensável de CONFERE/MOVEU/CAIU. As referências `B` e `M` são as linhas dos anexos fornecidos, usando a numeração da remedição; a correspondência com arquivos externos não foi verificada.

| Premissa e sítio antigo | Julgamento no código da cópia |
|---|---|
| B:20–28; M:22–28, U1: procedência escolhe autor e instante de V, com fallback para C. | **CONFERE** — [procedencia_batida.py:71](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/procedencia_batida.py:71) e [procedencia_batida.py:74](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/procedencia_batida.py:74). |
| B:21; M:33–35, U2: filtro de via e V e leitura da recusa técnica. | **CONFERE** — [flip_auto.py:129](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/flip_auto.py:129). |
| B:22; M:41–42, U3: gate lê texto projetado por `strip`. | **CAIU** — os dois usos passam pelo juiz `pergunta_respondida`: [cartorio.py:1641](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1641), [cartorio.py:1649](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1649), [cartorio.py:1666](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1666). |
| B:23; M:43, U4: gate lê via duplicata da projeção. | **MOVEU** para [cartorio.py:1651](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1651). |
| B:24; M:48–50, U5: SILÊNCIO recebe carimbo C e texto projetado. | **CONFERE** — [veredito_celula.py:196](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:196). |
| B:25; M:56, U6: calendário usa `exclude(resposta_colab='')`. | **CAIU** — usa a face SQL do juiz: [calendario.py:499](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:499), [calendario.py:505](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:505). |
| B:26; M:62–63, U7: ficha lê texto diretamente e a retenção usa I **ou** T. | **CAIU** — a ficha e a retenção perguntam ao juiz: [ficha.py:169](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/ficha.py:169), [ficha.py:172](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/ficha.py:172), [models.py:1933](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1933). |
| B:27; M:68, U8: prévia decide pelo texto isolado. | **CAIU** — [previa.py:142](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/previa.py:142). |
| B:28; M:73, U9: dia serializa o texto projetado por truthiness. | **CONFERE** — [dia_do_colab.py:70](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/dia_do_colab.py:70). |
| B:61–64; M:75–84, F1 fornece a antiga face conservada de leitura dos atos. | **CAIU como contrato operativo da F8** — o juiz atual devolve booleano para instante e texto; há faces SQL positiva e negativa: [juizes.py:1544](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1544), [juizes.py:1575](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1575), [juizes.py:1583](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1583). |

CONFERE, MOVEU e CAIU para **todas** as premissas: **[nao verificado]**. Os dez julgamentos acima não são uma contagem da matriz completa.

## 2 O que mudou

O patch fornecido compara `fbc5c76ce` com `dd489c627`; a cópia não permite confirmar o diff independentemente por Git. Os seguintes efeitos do patch foram conferidos nos sítios da árvore atual. [nao verificado]

- A B2 substituiu as leituras isoladas de R em cartório, calendário, ficha e prévia pelo juiz ou sua face SQL. A retenção chamada pela ficha também passou de I **ou** T para I **e** T. Isso retira essas mudanças do trabalho ainda pendente da F8, embora o resultado em dados reais permaneça **[nao verificado]**. [cartorio.py:1642](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1642), [calendario.py:505](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:505), [ficha.py:172](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/ficha.py:172), [previa.py:142](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/previa.py:142), [models.py:1933](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1933).
- Novos leitores do juiz no intervalo incluem a lista `respondidas` e a resposta na API, as duas portas web e o vigia; a face negativa também passou a alimentar a reabertura. [api/views.py:2578](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:2578), [api/views.py:2622](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:2622), [chamados/views.py:1281](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views.py:1281), [chamados/views.py:1418](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/views.py:1418), [vigia_fio.py:636](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/vigia_fio.py:636).
- A materialização ganhou uma guarda comum de anuência nos três ramos; quando o juiz diz que não há resposta, ela chama a porta de recusa técnica. Assim, U2 continua dependente do motivo projetado, inclusive de um caminho novo que pode escrevê-lo. [materializacao.py:1269](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1269), [materializacao.py:1289](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1289), [materializacao.py:1670](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:1670).
- A F2 acrescentou as marcas `da_ressuscitacao` e `resposta_mantem_validacao`; a carga da tarefa exclui da sequência os atos identificados pela primeira. A resposta sintética da projeção declara a segunda como verdadeira. [models.py:4067](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4067), [models.py:4143](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:4143), [tarefa_do_dia.py:427](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:427).
- O teste B2 antes indisponível agora está na cópia e contém casos para meios pares e para a face SQL negativa; sua execução e a cobertura completa dos degraus F8 permanecem **[nao verificado]**. [test_f1nova_b2_leitores.py:61](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_f1nova_b2_leitores.py:61), [test_f1nova_b2_leitores.py:134](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_f1nova_b2_leitores.py:134).

O inventário completo de escritores e leitores novos do intervalo **não foi concluído**. [nao verificado]

## 3 Nao verificado de ALCANCE

- **U7b foi fechado quanto ao corpo:** a ficha chama `respostas_sem_veredito`, que delega a `sem_veredito_entre`; este agora usa o juiz. Outros consumidores encontrados incluem o reconciliador e o suprajuiz. [ficha.py:169](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/ficha.py:169), [models.py:1914](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1914), [models.py:1933](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1933), [reconciliador.py:1208](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/reconciliador.py:1208), [supra_juiz.py:573](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/supra_juiz.py:573).
- **Cadeia de U2:** além do comando automático, há consumo de `avaliar_fila` pela tela. [flip_automatico.py:32](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/management/commands/flip_automatico.py:32), [views.py:3459](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/views.py:3459).
- **Templates globais:** há leitura direta de texto em templates de chamados; isso não prova que recebam o objeto pelos consumidores específicos de ficha, prévia ou calendário. [modal_fio.html:219](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/modal_fio.html:219), [_lote_preview.html:24](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/chamados/partials/_lote_preview.html:24).
- **Continua aberto:** censo exaustivo de escritores e leitores dos campos, cruzamento completo com F3/F5/F6/F9 e consumidores nas duas cascas. Falta percorrer esses grafos e confrontá-los com os briefs respectivos; não se pode concluir ausência a partir dos sítios acima. **[nao verificado]**

alcance nao verificado restante: **[nao verificado]** — não fiz a contagem exaustiva dos pontos abertos.

## 4 Perguntas de lei

- V legada com resposta sintética posterior continua validada? **DECIDIDO PELA LEI CORTES-F2-F9-1611, item 8: sim.** O mesmo corte pede medir perguntas e batidas contra a árvore pousada; essa medida permanece **[nao verificado]**. [CORTES.md:509](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/CORTES.md:509).
- Instante sem texto ou texto sem instante conta como resposta? **DECIDIDO PELA LEI L-130: não**; o juiz exige instante e texto não vazio. [LEIS.md:151](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/LEIS.md:151), [juizes.py:1544](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:1544).
- A dúvida sobre substituir o texto corrente pela foto do último ato foi respondida no anexo `matriz.md`, seção **RESPOSTAS-1711-1005 item 4**, pela opção A: leitores e SILÊNCIO seguem o texto corrente; a foto fica como histórico. Não localizei esse corte nos três arquivos de leis pesquisados; a linha de um registro na árvore permanece **[nao verificado]**. O SILÊNCIO atual copia o texto projetado em [veredito_celula.py:197](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:197).

Não identifiquei pergunta de lei restante entre essas três, mas **não concluí a revisão de todas as premissas**; o número abaixo não certifica que a fatia inteira foi esgotada. [nao verificado]

perguntas de lei: 0

## 18/11 09:06 · sessao 1219370d65e6 · ESCALA-NAO-DESCREVE · censo · origem=Code

**O censo ainda não está fechado:** a cópia permite fechar os pontos sobre reexports e o escopo dos escritores do vínculo, mas não comprova a identidade do hash nem a exaustividade de acessos dinâmicos e de todos os leitores dos nomes em dicionários. [não verificado]

**P1 — leitor dinâmico: aberto quanto à exaustividade.**

- `serie(chave, escopo='frota', escopo_id=None)` aceita qualquer chave como argumento, inclusive `esmeril_espelho`, e devolve objetos `MetricaSnapshot` inteiros. Na busca por chamadas e imports de `serie` em `app/` e `bin/`, não encontrei chamador; chamadas montadas dinamicamente permanecem [não verificado]. [app/inteligencia/metricas.py:62](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/inteligencia/metricas.py:62)
- Os leitores parametrizados encontrados com conjuntos **restritos** não recebem essa chave nas chamadas mostradas: `_snap` recebe `fabricante_estado` ou `pacotes_caidos`; a iteração da saúde usa `preenchimento_espelho` e `preenchimento_marcos`; e o vigia de cadastro futuro itera suas quatro constantes `celula_futura_*`. [app/api/views_mensageria.py:324](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views_mensageria.py:324), [app/inteligencia/management/commands/lavrar_fabrica.py:21](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/inteligencia/management/commands/lavrar_fabrica.py:21), [app/api/views_mensageria.py:597](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views_mensageria.py:597), [app/ponto/services/vigia_cadastro_futuro.py:6](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/vigia_cadastro_futuro.py:6), [app/ponto/services/vigia_cadastro_futuro.py:111](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/vigia_cadastro_futuro.py:111)
- O `admin.py` de inteligência está vazio de registros, e a busca por registro explícito não encontrou `MetricaSnapshot`; registro indireto permanece [não verificado]. Não identifiquei export genérico do modelo na busca feita; exaustividade permanece [não verificado]. A sombra lê **literalmente** `esmeril_espelho` e serializa o detalhe inteiro. [app/inteligencia/admin.py:2](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/inteligencia/admin.py:2), [bin/sombra.sh:138](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/bin/sombra.sh:138)

**P2 — reexports: fecha para a busca estática.** Os dois `__init__.py` de serviços existem e estão vazios; a busca nos `__init__.py` de `app/` não encontrou referências aos módulos, e a busca em `app/` e `bin/` não encontrou os imports relativos especificados. Reexport construído dinamicamente permanece [não verificado]. [app/ponto/services/__init__.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/__init__.py), [app/escala/services/__init__.py](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/services/__init__.py)

**P3 — escritores globais: fecha para o escopo declarado da obra.** Para censar *novos caminhos de escrita introduzidos pela obra*, basta seguir a chamada da porta e seus efeitos: a lista lê o vínculo vigente e copia seus dados para as linhas; a realocação encaminha ao juiz e ao executor existentes, que fecham ou alteram vigências. O censo global continua necessário se a pergunta for concorrência, interferência de outros caminhos ou exclusividade de escrita desses campos; essas propriedades globais permanecem [não verificado]. [app/escala/services/cadastro_realidade.py:748](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/services/cadastro_realidade.py:748), [app/escala/regua_defesa.py:291](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/regua_defesa.py:291), [app/colaboradores/services/vinculo.py:211](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/vinculo.py:211)

**P4 — amostra confere; identidade do HEAD não.** Os trechos existem e correspondem ao descrito: o esmeril só insere colaborador e calcula destino quando `sig` é verdadeiro; a lista lê a lavra, monta linhas, agrupa em `postos` e retorna os campos citados. Isso comprova o conteúdo da amostra, não que a cópia seja o archive do hash informado: identidade do HEAD **[não verificado]**. [app/ponto/services/esmeril_espelho.py:206](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/esmeril_espelho.py:206), [app/escala/services/cadastro_realidade.py:872](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/services/cadastro_realidade.py:872)

**N1 — colisões em dicionários.** Há homônimos com significados diferentes; os acessos encontrados usam dicionários diferentes do detalhe superior da lavra:

- `orfas`: listas de horas no `dia` ou na `ata`, lidas, por exemplo, pelo esmeril, cartório, propositor, juiz e grade; há também `out['orfas']` como contador do reconciliador. [app/ponto/services/esmeril_espelho.py:125](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/esmeril_espelho.py:125), [app/ponto/services/cartorio.py:736](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:736), [app/ponto/propositor.py:39](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/propositor.py:39), [app/ponto/juiz_batida.py:161](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/juiz_batida.py:161), [app/escala/utils.py:799](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:799), [app/chamados/reconciliador.py:1420](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/reconciliador.py:1420)
- `proposta`: o resultado de `propor_escala` contém uma proposta individual, consumida por `_destino`, escala automática, PDF e comando; o chat lê outra proposta em `dados`, e o comando de ritmo lê proposta em uma linha de vínculo. [app/ponto/services/esmeril_espelho.py:234](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/esmeril_espelho.py:234), [app/escala/services/escala_auto.py:117](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/services/escala_auto.py:117), [app/escala/proposta_pdf.py:75](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/proposta_pdf.py:75), [app/escala/management/commands/propor_escala_limbo.py:39](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/management/commands/propor_escala_limbo.py:39), [app/core/views_ia.py:124](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/core/views_ia.py:124), [app/escala/management/commands/ritmo_discordante.py:17](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/management/commands/ritmo_discordante.py:17)
- A busca textual por `orfas_desde` em código, templates e shell não encontrou ocorrência; leitores com nome construído dinamicamente e a identificação exaustiva de **todo** acesso a `orfas` e `proposta` em qualquer dict permanecem [não verificado]. Acrescentar as três chaves ao detalhe muda seu MD5; a reposição da sombra também usa o tamanho do detalhe como `valor`. [app/ponto/services/esmeril_espelho.py:260](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/esmeril_espelho.py:260), [bin/sombra.sh:155](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/bin/sombra.sh:155)

**N2 — parâmetro `medida`:** os dois chamadores locais passam três argumentos posicionais; `escala_certa` passa os mesmos três posicionais e `so_escala=True` por palavra-chave. Acrescentar `medida` depois de `so_escala` preserva essas chamadas identificadas; chamadas dinâmicas permanecem [não verificado]. [app/ponto/services/esmeril_espelho.py:79](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/esmeril_espelho.py:79), [app/ponto/services/esmeril_espelho.py:195](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/esmeril_espelho.py:195), [app/ponto/services/esmeril_espelho.py:220](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/esmeril_espelho.py:220), [app/escala/services/escala_certa.py:42](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/services/escala_certa.py:42)

**N3 — novos campos da lista.** Nenhum dos nomes propostos aparece no dict atual de `_vinculos` ou no retorno de `lista`; `linha = dict(v, ...)` propagaria também o novo `ciclo`. Nos consumidores e testes examinados, encontrei acesso por campos e iteração de `postos`, não comparação ou serialização integral da linha ou do retorno. A ausência desses usos em **todos** os leitores e testes permanece [não verificado]. [app/escala/services/cadastro_realidade.py:754](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/services/cadastro_realidade.py:754), [app/escala/services/cadastro_realidade.py:887](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/services/cadastro_realidade.py:887), [app/escala/services/cadastro_realidade.py:956](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/services/cadastro_realidade.py:956), [app/escala/tests/test_cadastro_x_realidade.py:133](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/tests/test_cadastro_x_realidade.py:133), [app/escala/tests/test_e4_parte4_cinco_copias.py:99](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/tests/test_e4_parte4_cinco_copias.py:99)

**N4 — rota, view e selos.** O nome e o caminho propostos não aparecem nas rotas atuais de escala, e a busca pelo nome da view não encontrou definição existente. [app/escala/urls.py:6](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/urls.py:6)

Há varreduras relevantes: o selo de sessão percorre rotas e tenta POST quando GET retorna 405; `portas_do_codigo` descobre views POST por inspeção e alimenta o mapa de portas, sem exigir cadastro manual no trecho examinado; o selo do diagrama compara os artefatos gerados. A prova de casca também percorre os URLconfs para verificar imports. Outros selos ou mapas de permissão que possam exigir cadastro permanecem [não verificado]. [app/core/tests/test_rota_pede_sessao.py:235](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/core/tests/test_rota_pede_sessao.py:235), [app/core/portas_da_tela.py:103](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/core/portas_da_tela.py:103), [app/core/management/commands/gerar_diagrama.py:17](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/core/management/commands/gerar_diagrama.py:17), [bin/prova_de_casca.py:72](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/bin/prova_de_casca.py:72)

censo fechado: nao -- faltam exaustividade dos acessos dinâmicos e dos leitores de nomes em dicts, verificação integral dos consumidores da lista e dos selos de rota, e comprovação independente da identidade do hash [não verificado].

## 18/11 09:05 · sessao 6a83051f4826 · ESPURIA-TELA · leitura · origem=Code

**Os três sítios são consumidores posteriores**, segundo as cadeias de chamadas encontradas na cópia. Isso não significa que toda consulta à precedência seja posterior: `fatos_do_dia` também é chamado dentro da cadeia de `julgar_celula`, inclusive durante a lavra e na emissão de chamado ([cartorio.py:2041](app/ponto/services/cartorio.py#L2041), [cartorio.py:1858](app/ponto/services/cartorio.py#L1858), [cartorio.py:1792](app/ponto/services/cartorio.py#L1792)).

O HEAD informado, `dd489c627`, está **[não verificado]**: a cópia aberta não contém metadados Git que permitam conferi-lo.

### S1 — `veredito_do_dia`

**Consumidor.** A linha 376 normaliza `fatos`; a leitura de `tipo_do_dia` ocorre efetivamente na linha 384. A função traduz fatos e decisões recebidos em uma palavra visual ([dia_decidido.py:337](app/ponto/services/dia_decidido.py#L337), [dia_decidido.py:376](app/ponto/services/dia_decidido.py#L376), [dia_decidido.py:384](app/ponto/services/dia_decidido.py#L384)).

O único chamador direto de produção encontrado para `veredito_do_dia` é `do_dia`, em [dia_decidido.py:421](app/ponto/services/dia_decidido.py#L421). Os **dois** chamadores diretos de produção de `do_dia` são:

- `aplicar_palavra_do_dia`, em [espelho.py:340](app/ponto/services/espelho.py#L340). Uma cadeia de tela é [ponto/views.py:161](app/ponto/views.py#L161) → [espelho.py:1291](app/ponto/services/espelho.py#L1291) → `aplicar_palavra_do_dia` → `do_dia` → `veredito_do_dia`. Há também a entrada de PDF em [pdf_espelho.py:638](app/relatorios/pdf_espelho.py#L638).
- `contexto_calendario`, em [calendario.py:748](app/colaboradores/services/calendario.py#L748). Uma cadeia de tela é [colaboradores/views.py:1990](app/colaboradores/views.py#L1990) → `contexto_calendario` → `do_dia` → `veredito_do_dia`.

O calendário também é usado pela porta de tarefa: [tarefa_do_dia.py:619](app/chamados/services/tarefa_do_dia.py#L619) → [calendario.py:157](app/colaboradores/services/calendario.py#L157) → [calendario.py:748](app/colaboradores/services/calendario.py#L748). Esse caminho é condicionado à ausência de rota e consulta uma tarefa que já recebeu a lavra; portanto, a descrição precisa é **consumidor posterior**, não exclusivamente tela/PDF ([tarefa_do_dia.py:588](app/chamados/services/tarefa_do_dia.py#L588), [tarefa_do_dia.py:611](app/chamados/services/tarefa_do_dia.py#L611)).

### S2 — `status_dos_dias_lavrados`

**Consumidor.** Ele lê a célula e devolve o status lavrado quando disponível; nos ramos sem lavra e sem batida, consulta a precedência ou usa fatos já fornecidos ([status_dia.py:272](app/ponto/status_dia.py#L272), [status_dia.py:286](app/ponto/status_dia.py#L286), [status_dia.py:296](app/ponto/status_dia.py#L296)). A linha 250 é o import; as chamadas são as linhas 298 e 304 ([status_dia.py:250](app/ponto/status_dia.py#L250), [status_dia.py:298](app/ponto/status_dia.py#L298), [status_dia.py:304](app/ponto/status_dia.py#L304)).

Todos os chamadores diretos de produção encontrados são:

| Chamador | Cadeia até uma entrada consumidora |
|---|---|
| `status_do_dia_lavrado` em [status_dia.py:218](app/ponto/status_dia.py#L218) | É o invólucro de um par; não encontrei chamada de produção dele na cópia. |
| `contexto_calendario` em [calendario.py:639](app/colaboradores/services/calendario.py#L639) | [colaboradores/views.py:1990](app/colaboradores/views.py#L1990) → calendário; também há a cadeia posterior pela tarefa descrita em S1. |
| `montar_semanas` em [espelho.py:170](app/ponto/services/espelho.py#L170) | [ponto/views.py:187](app/ponto/views.py#L187) → `montar_semanas`. |
| `_d4_em_lote` em [painel_op.py:336](app/core/services/painel_op.py#L336) | [core/views.py:607](app/core/views.py#L607) → [painel_op.py:236](app/core/services/painel_op.py#L236) → `_d4_em_lote`. |
| `api_espelho_v2` em [api/views.py:1490](app/api/views.py#L1490) | A própria view é a entrada em [api/views.py:1327](app/api/views.py#L1327). |
| `dataset_furos` em [furos.py:127](app/relatorios/furos.py#L127) | [relatorios/views.py:1509](app/relatorios/views.py#L1509) → `dataset_furos`; outra entrada está em [relatorios/views.py:1543](app/relatorios/views.py#L1543). |

**Distinção necessária:** a função vizinha `fatos_do_status` não é esse leitor. Ela consulta `fatos_do_dia` condicionalmente e é chamada pelo cartório para fornecer fatos à lavra ([status_dia.py:166](app/ponto/status_dia.py#L166), [status_dia.py:177](app/ponto/status_dia.py#L177), [cartorio.py:1854](app/ponto/services/cartorio.py#L1854)).

### S3 — `dias_com_furo`

**Consumidor.** Ele primeiro identifica lâmpadas apagadas na ata e depois consulta a precedência para selecionar os dias de trabalho ([sem_furo.py:20](app/folha/services/sem_furo.py#L20), [sem_furo.py:25](app/folha/services/sem_furo.py#L25)).

O único chamador direto de produção encontrado é `colabs_sem_furo`, em [sem_furo.py:51](app/folha/services/sem_furo.py#L51). A cadeia completa até a entrada é [bin/placar_code.sh:385](bin/placar_code.sh#L385) → [command `colabs_sem_furo`:13](app/folha/management/commands/colabs_sem_furo.py#L13) → `colabs_sem_furo` → `dias_com_furo`; o comando imprime o contador ([colabs_sem_furo.py:20](app/folha/management/commands/colabs_sem_furo.py#L20)).

### Chamadores de produção da precedência

A lista abaixo contém as **chamadas diretas** encontradas, incluindo a chamada interna de `fatos_do_dia` para `fatos_do_periodo`; imports e referências apenas em comentários não são contados.

| Função chamada | Chamadores diretos de produção |
|---|---|
| `fatos_do_dia` | `fatos_do_status` ([status_dia.py:181](app/ponto/status_dia.py#L181)); `_fato_do_dia` ([semana_do_posto.py:177](app/escala/services/semana_do_posto.py#L177)); `_dia_de_descanso` ([situacao.py:94](app/ferias/situacao.py#L94)); `espurios` ([feriado_reavaliar.py:34](app/chamados/services/feriado_reavaliar.py#L34)); `apurar` ([deslogados.py:166](app/ponto/services/deslogados.py#L166)); `com_nomes` ([abriu_nao_bateu.py:123](app/ponto/services/abriu_nao_bateu.py#L123)); `faltas_de_hoje_sem_cobranca` ([faltas_de_hoje.py:61](app/ponto/services/faltas_de_hoje.py#L61)); `dia_do_colab` ([dia_do_colab.py:15](app/ponto/services/dia_do_colab.py#L15)); `aplicar_precedencia_na_celula` ([cartorio.py:2000](app/ponto/services/cartorio.py#L2000)); `Command._abrir_chamado` ([detectar_ausencias.py:617](app/ponto/management/commands/detectar_ausencias.py#L617)). |
| `fatos_do_periodo` | `fatos_do_dia` ([precedencia.py:256](app/ponto/precedencia.py#L256)); `status_dos_dias_lavrados`, em dois ramos ([status_dia.py:298](app/ponto/status_dia.py#L298), [status_dia.py:304](app/ponto/status_dia.py#L304)); `montar_dias`, pelo alias `_fatos_periodo` ([espelho.py:648](app/ponto/services/espelho.py#L648)); `contexto_calendario` ([calendario.py:539](app/colaboradores/services/calendario.py#L539)); `dias_com_furo` ([sem_furo.py:25](app/folha/services/sem_furo.py#L25)). |

**Sim, há chamadas dentro da cadeia de `julgar_celula`:**

1. **Durante a lavra:** [julgar_celula:2041](app/ponto/services/cartorio.py#L2041) → `julgar_colab` → [_julgar_colab_corpo:996](app/ponto/services/cartorio.py#L996) → [`fatos_do_status`:1858](app/ponto/services/cartorio.py#L1858) → [`fatos_do_dia`:181](app/ponto/status_dia.py#L181) → [`fatos_do_periodo`:256](app/ponto/precedencia.py#L256). A consulta em `fatos_do_status` ocorre quando o veredito é `indefinida` ([status_dia.py:177](app/ponto/status_dia.py#L177)).
2. **No funil de emissão:** `julgar_celula` → `julgar_colab` → `_julgar_colab_corpo` → [`_abrir_chamado`:1792](app/ponto/services/cartorio.py#L1792), também chamado em [cartorio.py:1830](app/ponto/services/cartorio.py#L1830) → [`fatos_do_dia`:617](app/ponto/management/commands/detectar_ausencias.py#L617) → `fatos_do_periodo`.
3. **Depois do julgamento, ainda dentro de `julgar_celula`:** [cartorio.py:2049](app/ponto/services/cartorio.py#L2049) → [`aplicar_precedencia_na_celula`:2000](app/ponto/services/cartorio.py#L2000) → `fatos_do_dia` → `fatos_do_periodo`. O próprio código situa essa gravação depois de `julgar_colab` ([cartorio.py:2044](app/ponto/services/cartorio.py#L2044)).

O registro vivo identifica `fatos_do_dia` como autoridade de precedência e de feriado que suprime ([juizes.py:196](app/core/juizes.py#L196), [juizes.py:312](app/core/juizes.py#L312)).

## 18/11 09:04 · sessao a89b9ddeaa16 · - · - · origem=Ronald (tmux)

S1 é consumidor de apresentação: `veredito_do_dia` lê `tipo_do_dia` em [dia_decidido.py:384](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/dia_decidido.py:384) e produz um rótulo para o dia; a linha 376 apenas normaliza `fatos` ([dia_decidido.py:376](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/dia_decidido.py:376)).

O único chamador direto de produção de `veredito_do_dia` é `do_dia` ([dia_decidido.py:421](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/dia_decidido.py:421)). Os dois chamadores diretos de produção de `do_dia` são `aplicar_palavra_do_dia` ([espelho.py:340](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/espelho.py:340)) e `contexto_calendario` ([calendario.py:748](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:748)).

Cadeias até entradas consumidoras:

- Tela: [ponto/views.py:161](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/views.py:161) → `espelho_do_colab` → [espelho.py:1291](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/espelho.py:1291) → `aplicar_palavra_do_dia` → [espelho.py:340](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/espelho.py:340) → `do_dia` → [dia_decidido.py:421](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/dia_decidido.py:421) → S1.
- App: [api/views.py:1361](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/api/views.py:1361) → `espelho_do_colab` → mesma cadeia ([espelho.py:1291](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/espelho.py:1291)).
- Calendário: [colaboradores/views.py:1990](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/views.py:1990) → `contexto_calendario` → [calendario.py:748](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:748) → `do_dia` → [dia_decidido.py:421](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/dia_decidido.py:421) → S1.
- PDF: [pdf_espelho.py:598](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/relatorios/pdf_espelho.py:598) chama o espelho; [pdf_espelho.py:638](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/relatorios/pdf_espelho.py:638) chama diretamente `aplicar_palavra_do_dia`.

O calendário também é consultado pela tarefa depois de `_tarefa_pura` e da leitura da lavra ([tarefa_do_dia.py:588](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:588), [tarefa_do_dia.py:619](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:619), [calendario.py:157](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:157)).

## 18/11 09:04 · sessao 12ac5374c6e9 · - · - · origem=Ronald (tmux)

S3 é **consumidor**: `dias_com_furo` lê lâmpadas apagadas da ata em [sem_furo.py:20](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/services/sem_furo.py:20), consulta a precedência em [sem_furo.py:25](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/services/sem_furo.py:25) e seleciona dias de trabalho em [sem_furo.py:26](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/services/sem_furo.py:26). O registro vivo o classifica como TELA em [juizes.py:821](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/core/juizes.py:821).

A cadeia de produção encontrada é:

`bin/placar_code.sh:385` → `folha/management/commands/colabs_sem_furo.py:13` → `folha/services/sem_furo.py:51` → `dias_com_furo`.

Esses são os únicos chamadores de produção encontrados para `colabs_sem_furo` e `dias_com_furo`, respectivamente. O comando imprime o contador em [colabs_sem_furo.py:20](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/management/commands/colabs_sem_furo.py:20).

_6840 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
