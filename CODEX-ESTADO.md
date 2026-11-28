# CODEX-ESTADO

> Ultima resposta processada: **28/11/2026 10:37** · publicado em **28/11/2026 10:38** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 28/11 10:37 · sessao f785008300d3 · E4C-F5-INSTANTE · construtor · origem=Code

O drawer do Retrato agora mostra **data, motivo da retenção e link de conferência** para os dias OS6 com destino conhecido. A rota da tarefa continua sendo lida de `porta_da_tarefa`. A oferta de «Resolver dia» continua no calendário. `folha/fora_da_folha.py:70–73,239–248` · `templates/folha/_retrato_o_que_fazer.html:67–76` · `colaboradores/services/calendario.py:94–104`

### Diff e leitores

- [`fora_da_folha.py:125`](/home/ronald/wt-codex-e4c-f5-instante-102848/app/folha/fora_da_folha.py:125) publica `destinos_retencao` com `data`, `motivo`, `tela` e `id`. Para cobrança indevida, usa os IDs que a carga da tarefa associa ao dia com verbo `cobrar`. Para dia indefinido, usa a ausência que a autoridade de cobertura devolve para a data, quando existe, ou o painel do vínculo. `folha/fora_da_folha.py:125–140,239–247,275–278` · `chamados/services/tarefa_do_dia.py:738–751,809–810` · `ponto/turnos.py:3157–3170`
- [`retrato_folha.py:1319`](/home/ronald/wt-codex-e4c-f5-instante-102848/app/folha/retrato_folha.py:1319) copia o campo para a linha lavrada. `portas_da_linha` lê os quatro valores e entrega o link ao drawer do colaborador. O outro chamador, `o_que_segura`, passa a projeção anterior. O PDF continua lendo `motivos_por_dia` pelo leitor existente. `folha/retrato_folha.py:99–118,1117–1126` · `colaboradores/views.py:1004–1017` · `folha/fora_da_folha.py:317–322` · `folha/views_retrato.py:441–445`
- [`_retrato_o_que_fazer.html:73`](/home/ronald/wt-codex-e4c-f5-instante-102848/app/templates/folha/_retrato_o_que_fazer.html:73) exibe o dia e a causa junto ao botão existente. Para col476, o link leva ao vínculo. A obra `RAIZ-DA-DISPUTA-17-SEM-PORTA` está registrada no backlog. Rota de tela para essa obra [nao verificado]. `docs/RELATO.md:14898` · `docs/BACKLOG.md:2048`

### Medida na cópia

Comparei a base `f6ba2bc4e` e o código editado com os mesmos insumos em memória, usando os corpos reais do retentor, da classificação, da montagem do TXT e dos contadores. Nas duas versões, foram **14 dias retidos**, os mesmos pares `status/motivo` em **7 itens**, **45 bytes idênticos de TXT sintético** e os mesmos contadores e pílulas. O total foi de **1 entra, 5 bloqueados, 1 rescisão**. Banco, renderização Django, Retrato pk 10 e certificação pela suíte [nao verificado]. `folha/export.py:525,757,832,1250` · `folha/retrato_folha.py:147,882,1365`

### REDs para o Opus

Cada mutante abaixo é uma substituição literal com **uma ocorrência** na cópia. Execução dos REDs e dos mutantes [nao verificado].

| Caso | O que conferir | Mutante literal |
|---|---|---|
| **col246 · 01/11** | Com jornada 09–15–16–21, batidas 10:04/15:04/16:06/22:08 e chamado residual 36424, conferir `ok`, retenção `espelho_cobranca_indevida`, `rota=None` e link para o painel do chamado **36424**. No cenário de reconciliação, conferir a nova lavra antes de verificar a saída da retenção. `docs/RELATO.md:14896` · `ponto/services/cartorio.py:1618–1629` · `chamados/urls.py:15` | `if verbo == 'cobrar'` → `if verbo == 'decidir'` em `folha/fora_da_folha.py:133`. |
| **col327 · 10/11** | Com atestado 5515 e quatro marcos acesos, conferir retenção `espelho_indefinida`, `rota=None` e painel da ausência **5515** com a edição sujeita à permissão da tela. Após correção válida pela porta de ausência, conferir `concorde/ok` e saída da retenção em uma fixture sem outro protesto. `docs/RELATO.md:14897` · `ponto/views.py:2095–2116,2930` · `ponto/status_dia.py:161–162` | `return [('ausencia', ausencia.pk)]` → `return [('ausencia', colab.pk)]` em `folha/fora_da_folha.py:138`. |
| **col476 · 25/10** | Com batidas só em 24/10 e quatro perguntas vivas fora das pendentes do app, conferir retenção em **25/10**, `rota=None`, oferta de «Resolver dia» fechada e link para o vínculo. `docs/RELATO.md:14898` · `chamados/services/pergunta_no_app.py:80–92` · `colaboradores/services/calendario.py:94–122` | `colab_tem_porta = any(i['pergunta_id'] in pendentes_app and` → `colab_tem_porta = any(i['pergunta_id'] is not None and` em `chamados/services/tarefa_do_dia.py:752`. |
| **col97 · ímpares de 01 a 19/11** | Conferir os **10** dias, cada qual com motivo e link para o vínculo. Após corrigir a fase na fixture, conferir nova lavra por dia de turno, incluindo os pares 02→03 e 18→19, e a retenção resultante por data. `docs/RELATO.md:14899` · `templates/colaboradores/partials/painel_vinculo.html:157` · `folha/export.py:1250–1267` | `for _destino in _destinos:` → `for _destino in _destinos[:1]:` em `folha/retrato_folha.py:1122`. |
| **Pergunta alcançável** | Usar um dia retido por `espelho_indefinida` com pergunta realmente pendente no app. Conferir `porta_app`, fio `colab` e ausência de `destinos_retencao` para o dia. `chamados/services/tarefa_do_dia.py:752–754,777–780` · `folha/fora_da_folha.py:244–247` | `if com_motivos_por_dia and not (_p or {}).get('rota'):` → `if com_motivos_por_dia:` em `folha/fora_da_folha.py:245`. |
| **Desligado** | Usar um dia anterior à saída que apareceria como retenção para um ativo. Conferir a data em `informativo`, sua ausência de `datas` e de `destinos_retencao`, e o item `fora/rescisao_modulo_proprio`. `folha/export.py:928–936,1279–1302` · `folha/retrato_folha.py:1312–1320` | `'informativo': _info.get(_cid, []),` → `'informativo': _c.get('destinos_retencao', []),` em `folha/retrato_folha.py:1315`. |
| **Linha legada** | Abrir uma linha lavrada sem `destinos_retencao`, com `tipo='escala'` e fio `ninguem`. Conferir que o painel do vínculo continua acessível pelo link agregado e que o drawer abre. `folha/retrato_folha.py:1023–1039,1107–1119` · `templates/folha/_retrato_o_que_fazer.html:67–76` | `_destinos = linha.get('destinos_retencao')` → `_destinos = linha['destinos_retencao']` em `folha/retrato_folha.py:1117`. |

## 28/11 10:36 · sessao b60736d02a75 · FDS-FERIAS-COM-BATIDA-PORTA · leitura · origem=Code

## Arquivo `app/folha/tests/test_retrato_portas_ferias_codex.py`

```python
from urllib.parse import parse_qs

from django.test import SimpleTestCase
from django.urls import reverse

from folha.retrato_folha import portas_da_linha


class PortasFeriasComBatidaTest(SimpleTestCase):
    def assert_href_com_consulta(self, href, rota, colab, consulta_esperada):
        caminho, separador, consulta = href.partition('?')
        self.assertEqual(caminho, reverse(rota, args=[colab]))
        self.assertEqual(separador, '?')
        self.assertEqual(
            parse_qs(consulta, keep_blank_values=True, strict_parsing=True),
            consulta_esperada,
        )

    def test_calendario_do_primeiro_dia_gravado_antes_da_secao_de_ferias(self):
        casos = (
            (731, ['2025-12-30', '2026-01-04'], '2025', '12', 'ferias'),
            (842, ['2026-02-07', '2026-01-29'], '2026', '2', 'ferias'),
            (619, ['2024-02-29'], '2024', '2', 'cadastro'),
        )
        for colab, datas, ano, mes, tipo in casos:
            with self.subTest(colab=colab, datas=datas, tipo=tipo):
                linha = {
                    'colab': colab,
                    'status': 'fora',
                    'motivo': 'ferias_com_batida',
                    'tipo': tipo,
                    'dono': 'admin',
                    'acoes': [],
                    'datas': datas.copy(),
                }

                portas = portas_da_linha(linha)

                self.assertEqual(len(portas), 2)
                calendario, ferias = portas
                self.assertTrue(calendario['rotulo'].strip())
                self.assertTrue(ferias['rotulo'].strip())
                self.assert_href_com_consulta(
                    calendario['href'],
                    'colaboradores:calendario_ponto',
                    colab,
                    {
                        'ano': [ano],
                        'mes': [mes],
                        'modo': ['custom'],
                        'data_ini': [datas[0]],
                        'data_fim': [datas[0]],
                    },
                )
                self.assertEqual(
                    ferias['href'],
                    reverse('ferias:secao_ferias_colab', args=[colab]),
                )
                self.assertEqual(linha['datas'], datas)

    def test_sem_datas_oferece_so_a_secao_de_ferias(self):
        colab = 953
        for campos_de_data in ({}, {'datas': None}, {'datas': []}):
            with self.subTest(campos_de_data=campos_de_data):
                portas = portas_da_linha({
                    'colab': colab,
                    'status': 'fora',
                    'motivo': 'ferias_com_batida',
                    'tipo': 'ferias',
                    'dono': 'admin',
                    'acoes': [],
                    **campos_de_data,
                })

                self.assertEqual(
                    [porta['href'] for porta in portas],
                    [reverse('ferias:secao_ferias_colab', args=[colab])],
                )
                self.assertTrue(portas[0]['rotulo'].strip())

    def test_outro_motivo_preserva_a_porta_da_acao_publicada(self):
        colab = 164
        for tipo in ('falta', 'ferias'):
            with self.subTest(tipo=tipo):
                portas = portas_da_linha({
                    'colab': colab,
                    'status': 'fora',
                    'motivo': 'furo_espelho',
                    'tipo': tipo,
                    'dono': 'admin',
                    'datas': ['2025-12-30', '2026-01-04'],
                    'acoes': [{'acao': 'resolver_dia', 'dia_do_turno': '2026-01-04'}],
                })

                self.assertEqual(len(portas), 1)
                self.assertEqual(portas[0]['rotulo'], 'Resolver dia no calendário')
                self.assert_href_com_consulta(
                    portas[0]['href'],
                    'colaboradores:calendario_ponto',
                    colab,
                    {'ano': ['2026'], 'mes': ['1'], 'dia': ['4']},
                )

    def test_outros_motivos_sem_acoes_preservam_as_portas_anteriores(self):
        colab = 275
        fio = reverse('chamados:modal_fio', args=[colab])
        escala = reverse('colaboradores:painel_vinculo', args=[colab])
        cadastro = reverse('colaboradores:painel_colaborador', args=[colab])
        casos = (
            (
                {'motivo': 'furo_espelho', 'tipo': 'escala', 'dono': 'colab',
                 'datas': ['2026-02-07']},
                [
                    {'rotulo': 'Cobrar no fio', 'href': fio},
                    {'rotulo': 'Vínculo da escala', 'href': escala},
                ],
            ),
            (
                {'motivo': 'furo_espelho', 'tipo': 'falta', 'dono': 'ninguem',
                 'datas': ['2026-02-07']},
                [],
            ),
            (
                {'motivo': 'sem_codigo_dominio', 'tipo': 'cadastro', 'dono': 'admin'},
                [
                    {'rotulo': 'Validar no fio', 'href': fio},
                    {'rotulo': 'Cadastro do colaborador', 'href': cadastro},
                ],
            ),
        )
        for campos, esperadas in casos:
            with self.subTest(campos=campos):
                linha = {'colab': colab, 'status': 'fora', 'acoes': [], **campos}
                self.assertEqual(portas_da_linha(linha), esperadas)
```

## Regra fixada por teste [nao verificado]

- `test_calendario_do_primeiro_dia_gravado_antes_da_secao_de_ferias` — “oferece as DUAS portas existentes” e “dias lidos do campo gravado (testemunha le)”. O teste exige calendário do colaborador primeiro, com `ano` e `mes` do primeiro dia gravado e recorte `custom` de um só dia, seguido da seção de férias do mesmo ID. A ordem e o recorte estão explícitos na cópia. `brief_c1.md:8` e `brief_c1.md:9` (anexo), `app/folha/retrato_folha.py:1078–1093`, `app/colaboradores/services/calendario.py:227–235`.
- `test_sem_datas_oferece_so_a_secao_de_ferias` — “CADASTRO = secao Ferias do colaborador” e “dias lidos do campo gravado”. Com `datas` ausente, `None` ou `[]`, a seção de férias é a única porta. `brief_c1.md:8` e `brief_c1.md:9` (anexo), `app/folha/retrato_folha.py:1080–1093`, `app/ferias/urls.py:14`.
- `test_outro_motivo_preserva_a_porta_da_acao_publicada` — “demais motivos inalterados”. A ação `resolver_dia` continua apontando para o dia de turno publicado, inclusive no caso sintético em que o campo `tipo` contém `ferias`. `brief_c1.md:9` (anexo), `app/folha/retrato_folha.py:1053`, `app/folha/retrato_folha.py:1095–1117`.
- `test_outros_motivos_sem_acoes_preservam_as_portas_anteriores` — “demais motivos inalterados”. Os controles preservam cobrança seguida de vínculo da escala, nenhuma porta para falta sem ação disponível, e validação seguida de cadastro para falta de código Domínio. `brief_c1.md:9` (anexo), `app/folha/retrato_folha.py:1023–1045`, `app/folha/retrato_folha.py:1118–1128`.

Usei datas fora de ordem e combinações divergentes de `motivo` e `tipo` como entradas sintéticas para distinguir a primeira data gravada da menor data e o motivo do tipo. O produtor ordena as datas e deriva o tipo das férias com batida. Ocorrência dessas combinações em produção [não verificado]. `app/folha/export.py:498–504`, `app/folha/retrato_folha.py:247–253`, `app/folha/retrato_folha.py:413–424`.

## Veredito esperado

- **VERMELHO sem a cura.** Removendo em memória apenas o bloco `ferias_com_batida`, os seis subcasos de férias falharam e os controles passaram. `app/folha/retrato_folha.py:1078–1093`, `app/folha/retrato_folha.py:1118–1128`.
- **VERDE em `de15021a9`.** Os quatro métodos passaram na cópia fornecida, em memória, com `unittest` e resolução de URL simulada a partir das rotas declaradas. Execução pelo runner Django e confirmação do hash via Git [não verificado]. `app/folha/retrato_folha.py:1058–1128`, `app/config/urls.py:23–32`, `app/colaboradores/urls.py:12`, `app/colaboradores/urls.py:24`, `app/colaboradores/urls.py:48`, `app/ferias/urls.py:14`, `app/chamados/urls.py:39`.

## Mutantes derrubados em memória

- Retirar uma das portas, inverter a ordem, esvaziar o rótulo do calendário, congelar o ID ou apontar direto para a retratação da batida ou para a edição do agendamento. `app/folha/retrato_folha.py:1083–1093`, `app/ponto/urls.py:62`, `app/ferias/urls.py:21`.
- Ler o campo do export, escolher o último ou o menor dia, ordenar a lista gravada, fixar o mês, abrir todo o intervalo, usar o parâmetro `dia` de `resolver_dia` ou omitir o recorte. `app/folha/retrato_folha.py:1079–1088`, `app/folha/retrato_folha.py:1106–1111`, `app/folha/retrato_folha.py:1297–1305`.
- Inventar um dia sem datas, falhar quando a chave está ausente, exigir mais de um dia para oferecer o calendário ou condicionar a seção de férias à presença de datas. `app/folha/retrato_folha.py:1080–1093`.
- Acionar as portas pelo `tipo` ou pela presença de datas, ou deixar o caso de férias cair no fluxo geral e ganhar uma porta extra. `app/folha/retrato_folha.py:1078–1095`, `app/folha/retrato_folha.py:1118–1128`.

## 28/11 10:27 · sessao 3d06f7064149 · E4D-CONTRATO-ROTA-B · leitura · origem=Code

# r.md — E4D-CONTRATO-ROTA-B, cura4

## A — O fio está certo. A catraca precisa seguir a origem de `leitura`

`montar_chamados_fio` é o único chamador de produção que passa `leitura`. Ele calcula `DecisaoPergunta.composta(_p)` e entrega o resultado à função na linha seguinte. `aguarda_o_colaborador` e `cobranca._calados` omitem o argumento e entram no ramo que calcula a composta localmente. A catraca atual apaga os vínculos dos parâmetros ao entrar na função e combina os ramos por interseção. Isso explica o falso vermelho em `fio.py:226`. `app/chamados/services/fio.py:199–226,442–446` · `app/chamados/services/cobranca.py:221–242` · `app/chamados/tests/test_e4c_f5_selo_catraca.py:237–250,321–327`

**Arquivo** `app/chamados/tests/test_e4c_f5_selo_catraca.py`. A catraca passa a provar a origem nos chamadores diretos antes de aceitar o parâmetro na função. As duas verificações usam a fonte que o teste recebeu, inclusive quando ela foi mutada. `app/chamados/tests/test_e4c_f5_selo_catraca.py:332–349` · `app/chamados/tests/e4c_f5_voltaE_mutantes.py:80–91` · `app/core/censo_escritas.py:66–82`

Na âncora `from chamados.tests.e4c_f5_voltaE_mutantes import fonte_mutada`, cole estes imports. `app/chamados/tests/test_e4c_f5_selo_catraca.py:9`

```python
from chamados.tests.e4c_f5_voltaE_mutantes import fonte_mutada
from core.censo_escritas import _e_codigo_de_producao
```

Substitua de `class Catraca(ast.NodeVisitor):` até `        self.vinculos = {}`, inclusive. `app/chamados/tests/test_e4c_f5_selo_catraca.py:68–70`

```python
class Catraca(ast.NodeVisitor):
    def __init__(self, *, parametros_compostos=None):
        self.parametros_compostos = parametros_compostos or {}
        self.vinculos = {}
```

Em `visit_FunctionDef`, substitua o trecho que começa em `        antes = self.vinculos.copy()` imediatamente antes de `        for arg in ast.walk(no.args):` e termina antes de `    visit_AsyncFunctionDef = visit_FunctionDef`. `app/chamados/tests/test_e4c_f5_selo_catraca.py:321–329`

```python
        antes = self.vinculos.copy()
        compostos = self.parametros_compostos.get(no, ())
        for arg in ast.walk(no.args):
            if isinstance(arg, ast.arg):
                self.vinculos.pop(arg.arg, None)
                if arg.arg in compostos:
                    self.vinculos[arg.arg] = VAZIO._replace(composta=True)
        for passo in no.body:
            self.visit(passo)
        self.vinculos = antes
```

Substitua de `def _vermelhos(funcao):` até antes de `    def _julgar(self, corpo):`. `app/chamados/tests/test_e4c_f5_selo_catraca.py:332–351`

```python
ALVO_FIO = 'espera_o_texto_do_colaborador'


class CatracaChamadasDoFio(Catraca):
    def __init__(self):
        super().__init__()
        self.chamadas_cruas = set()

    def _composta(self, no):
        if isinstance(no, ast.Name):
            return super()._composta(no)
        return (
            _metodo(no) == 'composta'
            and isinstance(no.func.value, ast.Name)
            and no.func.value.id == 'DecisaoPergunta'
        )

    def visit_Call(self, no):
        chamada_do_fio = (
            isinstance(no.func, ast.Name) and no.func.id == ALVO_FIO
        ) or _metodo(no) == ALVO_FIO
        if chamada_do_fio:
            argumentos_opacos = (
                len(no.args) > 1
                or any(isinstance(arg, ast.Starred) for arg in no.args)
                or any(kw.arg is None for kw in no.keywords)
            )
            leitura_crua = any(
                kw.arg == 'leitura'
                and not (isinstance(kw.value, ast.Constant) and kw.value.value is None)
                and not self._composta(kw.value)
                for kw in no.keywords
            )
            if argumentos_opacos or leitura_crua:
                self.chamadas_cruas.add(no.lineno)
        super().visit_Call(no)


def _vermelhos_das_chamadas_do_fio():
    vermelhos = []
    for caminho in sorted(RAIZ.rglob('*.py')):
        arquivo = caminho.relative_to(RAIZ).as_posix()
        if not _e_codigo_de_producao(arquivo):
            continue
        texto = fonte_mutada(arquivo, caminho.read_text(encoding='utf-8'))
        if ALVO_FIO in texto:
            catraca = CatracaChamadasDoFio()
            catraca.visit(ast.parse(texto, filename=arquivo))
            vermelhos.extend(f'{arquivo}:{linha} {ALVO_FIO} (leitura)'
                             for linha in sorted(catraca.chamadas_cruas))
    return vermelhos


def _vermelhos(funcao, *, parametros_compostos=()):
    catraca = Catraca(parametros_compostos={funcao: parametros_compostos})
    catraca.visit(funcao)
    return catraca.vermelhos


class SeloCatracaTest(SimpleTestCase):
    def test_producao_sem_leituras_cruas(self):
        self.assertEqual(_vermelhos_das_chamadas_do_fio(), [])
        vermelhos = []
        for arquivo, nomes in FUNCOES.items():
            fonte = ast.parse(fonte_mutada(arquivo, (RAIZ / arquivo).read_text(encoding='utf-8')),
                              filename=arquivo)
            for nome in nomes:
                locais = [no for no in fonte.body if isinstance(no, ast.FunctionDef) and no.name == nome]
                self.assertEqual(len(locais), 1, (arquivo, nome))
                compostos = ('leitura',) if (arquivo, nome) == ('chamados/services/fio.py', ALVO_FIO) else ()
                vermelhos.extend(
                    f'{arquivo}:{linha} {nome} ({tipo})'
                    for linha, tipo in sorted(_vermelhos(locais[0], parametros_compostos=compostos))
                )
        self.assertEqual(vermelhos, [])

```

**Mutantes A.** Cada tupla traz `id`, arquivo de produção, original e mutado. Aplique cada tupla independentemente. A1 cai em `self.assertEqual(_vermelhos_das_chamadas_do_fio(), [])` na chamada da linha 445. A2 e A3 caem em `self.assertEqual(vermelhos, [])` na leitura de `respondida_em`, respectivamente na linha 228 da fonte mutada e na linha 226. Rodei os nove métodos do selo em memória com `unittest.TestCase`. A fonte limpa passou e os três mutantes falharam nas assertivas indicadas. `app/chamados/services/fio.py:224–226,444–446` · `app/chamados/tests/test_e4c_f5_selo_catraca.py:338–447`

```python
[
    (
        'A1', 'app/chamados/services/fio.py',
        'espera_o_texto_do_colaborador(_p, leitura=leitura)',
        'espera_o_texto_do_colaborador(_p, leitura=_p)',
    ),
    (
        'A2', 'app/chamados/services/fio.py',
        '    if leitura is None:\n        leitura = DecisaoPergunta.composta(p)',
        '    if leitura is None:\n        leitura = DecisaoPergunta.composta(p)\n'
        '    else:\n        leitura = p',
    ),
    (
        'A3', 'app/chamados/services/fio.py',
        '    if leitura is None:\n        leitura = DecisaoPergunta.composta(p)',
        '    if leitura is None:\n        leitura = p',
    ),
]
```

## B — Caso 20 testa a prevalência da recusa humana em Q

O teste atual exige `pedido_id` nas assinaturas do núcleo de resposta e da reabertura, e exige que dois identificadores distintos fiquem nas linhas `ABSORCAO` da mesma ausência. O recibo HTTP de sucesso devolve `status`, `pergunta_id` e `respondida_em`. `pergunta_id` é o PK da pergunta e `respondida_em` vem do ato de resposta gravado pelo pedido. `app/chamados/tests/test_e4d_matriz_reds.py:1451–1478` · `app/api/views.py:2653–2666` · `app/chamados/services/disputa_emissao.py:1564–1571,1618–1631`

A regra decidida é “A recusa humana prevalece em Q”. O grupo 6 deixa em aberto a extensão da interrupção às irmãs e a identidade da passagem. O teste abaixo intercala uma recusa real depois de a passagem selecionar Q e antes de relê-la sob a trava. Verifica a recusa de Q, a autoria e a projeção pendente. A fixture dá a Q um marco apagado e uma célula que acusa. O cenário vizinho de `:1440` exige também um desfecho para as irmãs e continua fora do veredito deste teste. `logs/E4D-MATRIZ/r2.txt`, grupo 6, caso 20, conforme anexo, linha original [nao verificado] · `app/docs/PROMPTS.md:2873` · `app/chamados/tests/test_e4d_matriz_reds.py:1356–1379,1440–1449,1480–1514` · `app/chamados/services/materializacao.py:1047–1058,1067–1109` · `app/chamados/juizes.py:1544–1555`

**Arquivo** `app/chamados/tests/test_e4d_matriz_reds.py`. Substitua de `    def test_caso_20_pedidos_distintos_na_mesma_ausencia(self):` até antes de `    def verificar_recusa_na_passagem(self, instante):`. `app/chamados/tests/test_e4d_matriz_reds.py:1451–1480`

```python
    def test_caso_20_recusa_humana_prevalece_em_q(self):
        e = self.escape()
        celula = self.celula(
            self.DIA_PASSAGEM, trabalha=True, origem='gerada', dna={'marcos': {'hf': '19:00'}},
        )
        celula.lavrar_veredito(
            via='cartorio', veredito='furo', agora=T0,
            ata={'lampadas': [{'tipo': 'S', 'hora': '19:00', 'acesa': False}]},
        )
        q = self.respondida(disputa=self.raiz, dia=self.DIA_PASSAGEM, celula=celula)
        motivo = 'Confirme a hora de saída'

        def recusar_q():
            resultado = validacao.recusar_pergunta(q.pk, self.admin, motivo)
            self.assertEqual((resultado['ok'], resultado['estado']), (True, 'recusada'))

        recusadas = self.passagem(e, recusar_q)
        self.atestado()
        self.assertEqual(recusadas, [q.pk])
        self.assertEqual(self.atos(q), [DP.RESPOSTA, DP.RECUSA])
        self.assertEqual(self.foto(q), VAZIA)
        recusa = q.decisoes.get(ato=DP.RECUSA)
        self.assertEqual((recusa.ato_em, recusa.ato_por_id, recusa.ato_motivo), (T1, self.admin.pk, motivo))

```

**Mutante B1.** O original ocorre uma vez. Ao aceitar o carimbo de ausência apesar do C6, a passagem inclui Q na absorção. A primeira assertiva que deve cair é `self.assertEqual(recusadas, [q.pk])`. Execução ORM [nao verificado]. `app/chamados/models.py:3474–3501` · `app/chamados/services/materializacao.py:1106–1140`

```python
[
    (
        'B1', 'app/chamados/models.py',
        'viva = pergunta_viva(self)',
        'viva = pergunta_viva(self) and via != self.VIA_AUSENCIA_DIA',
    ),
]
```

**Contraprova já registrada na matriz.** O seletor antigo chama a porta com `pedido_id` e referencia o nome do teste substituído. Em `app/chamados/tests/e4d_matriz_mutantes.py`, substitua de `    if nome == '20_reusa_pedido_da_ausencia':` até antes de `    if nome in {'25_absorcao_so_por_coluna', '26_devolve_sem_vinculo'}:`. `app/chamados/tests/e4d_matriz_mutantes.py:394–405`

```python
    if nome == '20_absorve_q_apesar_da_recusa':
        return trocar_codigo(P.carimbar, (
            ('viva = pergunta_viva(self)',
             'viva = pergunta_viva(self) and via != self.VIA_AUSENCIA_DIA'),
        ))

```

No mesmo arquivo, substitua a declaração de `M20ReusaPedidoDaAusencia = a(`, até antes de `M26DevolveSemVinculo = a(`. `app/chamados/tests/e4d_matriz_mutantes.py:665–670`

```python
M20RecusaPrevaleceEmQ = a('M20RecusaPrevaleceEmQ', '20_absorve_q_apesar_da_recusa',
                         'test_caso_20_recusa_humana_prevalece_em_q')
```

## C1 — Cartão renderizado pelo GET e pelo HTMX

O GET filtra a lista pelo C6. O POST HTMX de recusa sem motivo usa a porta real, devolve o parcial pelo PK e preserva a fixture. Isso permite conferir pelo HTMX a hora de validação quando o ato ainda está vigente. Depois da `DEVOLUCAO`, as duas vias têm de mostrar o cartão com a hora da resposta vigente, sem opacidade nem selo de validação, e com as ações disponíveis. `app/chamados/services/acoes_chamado.py:337–358` · `app/templates/chamados/detalhe_local.html:295–311` · `app/chamados/views.py:394–408,506–519,550–552` · `app/chamados/services/acoes_disputa.py:94–102` · `app/chamados/services/validacao.py:934–937` · `app/templates/chamados/partials/_pergunta_card.html:20–44,118–127`

**Arquivo** `app/chamados/tests/test_e4d_matriz_reds.py`. Insira antes de `    def test_caso_30_sql_api_modal_e_raiox_leem_a_mesma_resposta(self):`, dentro de `MatrizRaizELeitoresTest`. `app/chamados/tests/test_e4d_matriz_reds.py:1616–1617`

```python
    def test_cura4_cartao_exibe_composta_no_get_e_no_htmx(self):
        from django.urls import reverse

        from chamados.tests._ato_da_fixture import registrar_na_fixture

        gestora = User.objects.create_superuser(username='gestora_cura4_cartao')
        self.client.force_login(gestora)
        chamado = self.pai_de_disputa()
        disputa = self.disputa(chamado=chamado)
        resposta_em = T0 + timedelta(minutes=15)
        validacao_residual = T0 + timedelta(minutes=30)
        validacao_vigente = T0 + timedelta(minutes=35)
        devolucao_em = T0 + timedelta(minutes=45)
        texto = 'Conferi o registro'
        p = self.cartao(
            disputa=disputa, motivo='foto_ausente', tipo='texto',
            respondida_em=T0, resposta_colab=texto,
            validada_em=validacao_residual, validada_por_id=gestora.pk,
        )
        registrar_na_fixture([
            dict(pergunta=p, ato=DP.RESPOSTA, ato_em=T0, ato_texto=texto),
            dict(pergunta=p, ato=DP.RESPOSTA, ato_em=resposta_em, ato_texto=texto),
            dict(pergunta=p, ato=DP.VALIDACAO, ato_em=validacao_residual, ato_por=gestora),
            dict(pergunta=p, ato=DP.VALIDACAO, ato_em=validacao_vigente, ato_por=gestora),
        ])
        url = reverse('chamados:detalhe_local', args=[chamado.pk])
        template = 'chamados/partials/_pergunta_card.html'
        raiz = f'<div id="pergunta-{p.pk}"'

        def htmx():
            retorno = self.client.post(
                url, {'acao': 'recusar_pergunta', 'pergunta_id': p.pk},
                HTTP_HX_REQUEST='true',
            )
            self.assertEqual(retorno.status_code, 200)
            self.assertTemplateUsed(retorno, template)
            self.assertTrue(retorno.content.decode().lstrip().startswith(raiz))
            self.assertEqual(json.loads(retorno.headers['HX-Trigger'])['showToast']['tipo'], 'erro')
            return retorno

        with timezone.override('UTC'):
            selo = htmx()
            self.assertContains(selo, f'✓ validada {validacao_vigente:%d/%m %H:%M}</span>')
            registrar_na_fixture([
                dict(pergunta=p, ato=DP.DEVOLUCAO, ato_em=devolucao_em, ato_por=gestora),
            ])
            for via, renderizar in (('GET', lambda: self.client.get(url)), ('HTMX', htmx)):
                with self.subTest(via=via):
                    retorno = renderizar()
                    self.assertTemplateUsed(retorno, template)
                    self.assertContains(retorno, raiz, count=1)
                    html = retorno.content.decode()
                    abertura = html.split(raiz, 1)[1].split('>', 1)[0]
                    self.assertNotIn('opacity:.55;', abertura)
                    self.assertNotContains(retorno, '✓ validada ')
                    self.assertContains(retorno, '⚠ aguardando validacao')
                    self.assertContains(retorno, f'— {resposta_em:%d/%m %H:%M}</span>')
                    self.assertContains(retorno, 'name="acao" value="recusar_pergunta"')

        p.refresh_from_db()
        self.assertEqual((p.respondida_em, p.validada_em), (T0, validacao_residual))
        self.assertEqual(list(p.decisoes.order_by('pk').values_list('ato', flat=True)),
                         [DP.RESPOSTA, DP.RESPOSTA, DP.VALIDACAO, DP.VALIDACAO, DP.DEVOLUCAO])

```

**Mutantes C1.** Cada original ocorre uma vez. C1.1 deve cair na ausência de `opacity:.55;`. C1.2 deve cair na ausência de `✓ validada ` após a devolução. C1.3 deve cair na hora do selo vigente, no controle HTMX anterior à devolução. C1.4 deve cair na hora exibida da resposta. C1.5 deve cair na presença do botão `recusar_pergunta`. Execução Django [nao verificado]. `app/templates/chamados/partials/_pergunta_card.html:21–33,43–44,118–127` · `app/chamados/models.py:4486–4515`

```python
[
    (
        'C1.1', 'app/templates/chamados/partials/_pergunta_card.html',
        '{% if resposta.validada_em %}opacity:.55;',
        '{% if perg.validada_em %}opacity:.55;',
    ),
    (
        'C1.2', 'app/templates/chamados/partials/_pergunta_card.html',
        '{% if resposta.validada_em %}<span style="color:#16a34a;',
        '{% if perg.validada_em %}<span style="color:#16a34a;',
    ),
    (
        'C1.3', 'app/templates/chamados/partials/_pergunta_card.html',
        '{{ resposta.validada_em|date:"d/m H:i" }}',
        '{{ perg.validada_em|date:"d/m H:i" }}',
    ),
    (
        'C1.4', 'app/templates/chamados/partials/_pergunta_card.html',
        '{{ resposta.respondida_em|date:"d/m H:i" }}',
        '{{ perg.respondida_em|date:"d/m H:i" }}',
    ),
    (
        'C1.5', 'app/templates/chamados/partials/_pergunta_card.html',
        '{% if not resposta.validada_em and pode_agir %}',
        '{% if not perg.validada_em and pode_agir %}',
    ),
]
```

## C3 — Recibo do ato deste POST

O teste envolve o registrador real para agendar uma segunda resposta no `on_commit` da primeira. O callback confere que saiu do bloco atômico e usa a porta real para gravar em `T2`. A resposta legada entra no mesmo lote que promove a projeção e grava o pedido em `T1`. Assim o teste distingue o primeiro ato do lote (`T0`), o ato do pedido (`T1`) e a resposta vigente quando a API monta o recibo (`T2`). `app/chamados/models.py:4429–4447,4590–4623` · `app/chamados/services/disputa_emissao.py:1485–1503,1523–1534,1564–1571,1618–1628` · `app/api/views.py:2653–2666`

**Arquivo** `app/chamados/tests/test_e4d_matriz_reds.py`. Substitua a âncora `from django.test import RequestFactory, SimpleTestCase` pelo import abaixo. Depois insira a classe antes de `class MatrizRaizELeitoresTest(ApoioMatriz):`. `app/chamados/tests/test_e4d_matriz_reds.py:29,1616`

```python
from django.test import RequestFactory, SimpleTestCase, TransactionTestCase
```

```python
class ReciboCura4Test(TransactionTestCase):
    def test_cura4_recibo_do_post_preserva_o_ato_deste_pedido(self):
        from rest_framework.test import APIRequestFactory, force_authenticate

        from api.views import api_disputa_responder
        from chamados.tests.cortes_f2_f9_apoio import criar_empresa_e_colaborador

        with freeze_time(T1), calado():
            _, colab = criar_empresa_e_colaborador()
            titular = User.objects.create_user(username='titular_cura4_recibo')
            colab.usuario = titular
            colab.save(update_fields=['usuario'])
            disputa = DisputaSupervisao.objects.create(colaborador=colab)
            p = P.objects.create(
                disputa=disputa, motivo='foto_ausente', pergunta='Confirme o registro',
                tipo_resposta='texto', data_evento=DIA, conversa=True,
                resposta_colab='Resposta legada', respondida_em=T0,
            )
            texto_pedido = 'Resposta deste pedido'
            texto_posterior = 'Resposta de outro pedido'
            registrar = DP.registrar

            def gravar_resposta_posterior():
                self.assertFalse(connection.in_atomic_block)
                with freeze_time(T2):
                    atual = P.objects.get(pk=p.pk)
                    resultado = disputa_emissao.responder_pergunta_disputa_core_da_entrada(
                        atual, texto_posterior, titular, True, exige_viva=True,
                    )
                self.assertEqual(resultado, (True, None))

            def registrar_e_agendar(linhas):
                atos = registrar(linhas)
                if any(ato.pergunta_id == p.pk and ato.ato == DP.RESPOSTA
                       and ato.ato_texto == texto_pedido for ato in atos):
                    self.assertTrue(connection.in_atomic_block)
                    transaction.on_commit(gravar_resposta_posterior)
                return atos

            request = APIRequestFactory().post(
                f'/api/disputa/responder/{p.pk}/', {'resposta': texto_pedido}, format='json',
            )
            force_authenticate(request, user=titular)
            with patch.object(DP, 'registrar', new=staticmethod(regist

[truncado em 20000 de 23414 caracteres]

## 28/11 10:25 · sessao 7c32debc918d · FDS-FERIAS-COM-BATIDA-PORTA · leitura · origem=Code

## 1. Onde a linha nasce

- **Motivo.** `classificar_export` chama `dias_ferias_com_batida` e, quando recebe dias para o colaborador, carimba `status='fora'`, `motivo='ferias_com_batida'` e `dias_ferias_com_batida`. A lista contém datas civis locais distintas, ordenadas. `app/folha/export.py:491–505`, `:801`, `:892–900`.
- **`datas` e `dias`.** `montar_linhas` recebe o item por `fora_da_folha(com_itens=True)`, copia `dias_ferias_com_batida` para `datas` em ISO e grava `dias=len(datas)`. O comando `lavrar_retrato_folha --apply` persiste essas linhas. `app/folha/fora_da_folha.py:256–262`, `app/folha/retrato_folha.py:1274–1305`, `app/folha/management/commands/lavrar_retrato_folha.py:65–79`.
- **`acoes=[]`.** Quem atribui é `montar_linhas`, com `_c.get('acoes') or []`. `_c` fica vazio para esta linha porque `fora_da_folha` só carrega tarefas e monta `colabs` para os retidos por `furo_espelho`. `app/folha/fora_da_folha.py:150–165`, `:203–236`, `:242–261`, `app/folha/retrato_folha.py:1284–1289`, `:1310–1313`.

## 2. Quem tem a batida e quem dá o dia do turno

**A batida concreta está em mãos no laço de `dias_ferias_com_batida`.** Ali `b` vem de `batidas_apuraveis`, e o teste usa `localdate(b.timestamp)` contra as faixas de férias. O laço devolve somente o conjunto de datas civis. Quando `classificar_export` escreve o motivo e quando `montar_linhas` escreve a linha, ambos recebem só essas datas. O caminho de férias passa fora da carga de tarefas. `app/folha/export.py:491–505`, `:892–900`, `app/folha/fora_da_folha.py:203–213`, `app/folha/retrato_folha.py:1284–1311`.

**A autoridade é `ponto/turnos.py`.** O pareador atribui `Turno.data_turno` por `_data_do_turno`. `dia_das_batidas` projeta esse valor por PK, e `dia_do_turno_da_batida` é o acesso unitário. Para batida que nenhum turno reclama, a projeção devolve o dia civil. O Calendário `modo=custom` lê o intervalo pedido, agrupa os turnos da mesma autoridade por `t.data_turno` e desenha as marcações de `t.batidas`. Assim, o dia devolvido pela projeção e a presença da marcação na grade são verificações distintas. `app/ponto/turnos.py:285–324`, `:2971–2978`, `:3014–3025`, `:3034–3045`, `:3112–3140`, `app/colaboradores/services/calendario.py:227–253`, `:641–654`, `app/templates/colaboradores/partials/_calendario_grade.html:124–134`.

**`resolver_dia` já recebe um dia de turno como entrada.** A tarefa consulta a oferta do Calendário para esse dia, copia o dia para a porta publicada e o Retrato lê `dia_do_turno` da ação. A oferta de `resolver_dia` tem predicado próprio na tela. `app/chamados/services/tarefa_do_dia.py:214–215`, `:702–717`, `:781–790`, `:809–822`, `:830–840`, `app/colaboradores/services/calendario.py:97–104`, `app/folha/retrato_folha.py:1106–1111`.

## 3. Dá para publicar sem alterar `export.py`?

**Sim, com uma releitura das batidas. Só com o item atual, o dia do turno é irrecuperável.** Em `montar_linhas`, dá para usar as datas civis já certificadas pelo Export, reler `batidas_apuraveis` nesses dias e consultar `dia_das_batidas` por PK. Isso reaproveita o veredito de férias e o pareador. Para publicar uma porta que exiba a marcação, a evidência é o PK em `t.batidas` no dia indicado. `app/folha/export.py:498–505`, `:892–900`, `app/folha/retrato_folha.py:1284–1311`, `app/ponto/turnos.py:2941–2968`, `:3034–3045`, `app/colaboradores/services/calendario.py:641–654`.

**A fonte do conflito é o laço em `export.py:499–502`. A fonte do dia do turno é `turnos.py:318` e `:3042`.** Para transportar a evidência **da mesma passagem** que decidiu a retenção, o ponto é o laço do Export, antes de descartar o PK. O Retrato é o ponto de publicação da linha. O c3 fornecido só consome `acoes[].dia_do_turno` de `resolver_dia`. Com o `acoes=[]` produzido hoje, seu ramo oferece apenas Férias. `app/folha/export.py:498–505`, `app/ponto/turnos.py:312–324`, `:3034–3045`, `app/folha/retrato_folha.py:1299–1313`, anexo `c3.diff`, `retrato_folha.py:1049–1069` na versão proposta.

## 4. Quem lê e o que pode mudar

| Consumidor | Efeito |
|---|---|
| **Drawer** | Mostra `datas` literalmente. Na cópia, o ramo de férias abre o Calendário pela primeira data civil e acrescenta Férias, retornando antes da leitura genérica de `acoes`. “O que destrava” vem do fio gravado, que é `admin` para esse motivo. O outro chamador de `portas_da_linha`, `o_que_segura`, passa uma linha própria com `tipo='falta'`. `app/colaboradores/views.py:1004–1007`, `app/templates/folha/_retrato_o_que_fazer.html:43–47`, `app/folha/retrato_folha.py:406–410`, `:1078–1098`, `:1183–1184`, `app/folha/fora_da_folha.py:293–298`. |
| **Contadores e PDF do Retrato** | Publicar apenas o dado de navegação preserva as contagens. Trocar `datas` pode deslocar dias para dentro ou para fora do recorte, mudando `dias`, a faixa de saldo e os números das pílulas. O PDF usa o mesmo recorte e imprime `dias`. O total por situação conta linhas pelo balde de `status` e `motivo`. `app/folha/retrato_folha.py:122–154`, `:622–623`, `:872–876`, `:1403–1415`, `app/folha/views_retrato.py:230–232`, `:394–396`, `:450–457`. |
| **Painel do Export, PDF de Retidos e TXT** | Painel e PDF contam `dias_ferias_com_batida` no item do Export. O TXT seleciona os itens com `status='entra'`, então acrescentar o dia do turno como dado de navegação preserva o TXT. Mudar o cruzamento civil que decide `status` mexeria no critério de entrada. `app/folha/views.py:425–430`, `:884–886`, `app/folha/retidos_pdf.py:88–95`, `app/folha/export.py:723–736`, `:892–900`. |

## 5. Chamado 37604 e col976

- **Chamado 37604, J.A., competência 11, 07/11 — cruzamento da meia-noite [nao verificado].** O fixture de 07/11 injeta a data civil no retorno de `classificar_export`. O teste de porta também injeta a data na linha. Ambos exercitam a publicação ou o link, sem montar um par desse chamado. `app/folha/tests/test_retrato_por_que_por_dia.py:345–356`, `app/folha/tests/test_retrato_portas_ferias_com_batida.py:60–65`.
- **col976, 07–20/11 — cruzamento da meia-noite [nao verificado].** O dado fornecido não traz entrada, saída nem associação por PK. O fixture que prova a regra geral usa outro caso, com entrada dia 4 às 19h e saída dia 5 às 7h, ambas atribuídas ao dia 4. `app/ponto/tests/test_continuacao_turno_aberto.py:35–41`.

**Sonda exata, só leitura.** No tenant dos casos, resolver o colaborador do 37604 pelo identificador do chamado (`pk` se local, `ticket_id_glpi` se GLPI). Para cada colaborador, obter `dias_ferias_com_batida(colab.empresa, 11, 2026, [colab])` e recortar aos dias pedidos. Para cada dia civil `D` retornado, ler `batidas_apuraveis` do primeiro ao último instante local de `D`. Obter `turnos_do_colab(colab, D−1, D+1)`, passar esses turnos a `dia_das_batidas` e casar cada batida pelo PK em `t.batidas`. Emitir por batida a data civil, o `data_turno`, os timestamps locais de `entrada` e `saida` e `t.cross_meianoite`. Se faltar uma ponta, registrar o par incompleto. O booleano do código exige ambas as pontas e compara a data local da saída com `data_turno`. `app/chamados/models.py:50–51`, `app/chamados/views.py:359–363`, `app/folha/export.py:486–505`, `app/ponto/turnos.py:312–324`, `:1834–1852`, `:2941–2968`, `:2996–3004`, `:3034–3045`.

O comando existente `listar_ferias_com_batida` usa `Ausencia(tipo='ferias')` e marca `plantao_da_vespera` quando **todas** as batidas do dia são atribuídas à véspera. A sonda acima mede o universo do Retrato, que vem de agendamentos, e verifica o par de cada batida. `app/ferias/management/commands/listar_ferias_com_batida.py:4–11`, `:41–65`, `app/folha/export.py:484–505`.

## 28/11 10:20 · sessao 1cfdeddb2690 · E4C-F5-INSTANTE · construtor · origem=Code

A cura ficou em `app/core/censo_escritas.py`. O censo resolve `projecao=proj` quando `proj` recebeu uma tupla de dois elementos por atribuição simples anterior no mesmo escopo. O achado usa a linha da chamada e a mesma inferência da tupla literal. `DP._salvar_a_projecao(...)` gera um `save` de `PerguntaDisputa` fora de `DecisaoPergunta.registrar`, inclusive em função avulsa chamada `registrar`. `DP` é resolvido pela tabela de símbolos existente. `app/core/censo_escritas.py:221`, `app/core/censo_escritas.py:301`, `app/core/censo_escritas.py:422`, `app/core/censo_escritas.py:435`, `app/core/censo_escritas.py:442`

**Leitores e medição**

- O comando `censo_escritas` usa `entidade` e `arquivo` para filtrar e `expr` para exibir. A `tabela` também lê `op` para contar `delete`. Na cópia, o total ficou em **543 → 543**, com **137 → 137** sem entidade e **68 → 68** na família `chamado`. A comparação dos achados completos deu zero adições e zero remoções. `app/core/management/commands/censo_escritas.py:23`, `app/core/censo_escritas.py:525`, `app/core/censo_escritas.py:561`
- O gate de chamados filtra por `entidade` e `arquivo` e usa `expr` no diagnóstico. A contagem de sítios fora da porta ficou em **0 → 0**. Os dois `save()` físicos da porta continuaram visíveis com `entidade=None`. `app/chamados/tests/test_chokepoint_chamado_gate.py:316`, `app/chamados/tests/test_chokepoint_chamado_gate.py:349`, `app/chamados/models.py:4784`, `app/chamados/models.py:4802`

**REDs e mutantes para o Opus**

| Caso | Asserção | Mutante |
|---|---|---|
| Projeção por variável com `as DP` | Exatamente um `(4, 'PerguntaDisputa', 'save')` na chamada. Manter os controles da tupla literal e do `registrar` sem projeção. | Voltar a exigir `ast.Tuple` diretamente no argumento. A lista fica vazia. `app/core/censo_escritas.py:427`, `app/core/censo_escritas.py:442`, `app/chamados/tests/test_e4c_f5_c4_red.py:1564` |
| Helper direto em função avulsa `registrar` | Exatamente um `(3, 'PerguntaDisputa', 'save')` para `DP._salvar_a_projecao(p, ['respondida_em'])`. | Omitir o reconhecimento do helper ou dispensar pelo nome `registrar` sozinho. Em ambos os casos, a lista fica vazia. `app/core/censo_escritas.py:430`, `app/core/censo_escritas.py:435` |
| Controle dos dois saves da porta | Conferir todos os achados `save` da classe, com ou sem entidade. Exigir exatamente `instancia.save()` e `projecao[0].save()`, ambos com `entidade=None`. | Reclassificar um dos saves físicos ou contar a delegação interna como terceiro `save`. A igualdade da lista falha. `app/chamados/models.py:4784`, `app/chamados/models.py:4802`, `app/chamados/models.py:4824` |

**Script para o Code**

Execute a partir de `app/`. O script compara o censo da base com o da cópia atual e aplica o filtro do gate, cuja allowlist é vazia. `app/core/censo_escritas.py:525`, `app/chamados/tests/test_chokepoint_chamado_gate.py:56`, `app/chamados/tests/test_chokepoint_chamado_gate.py:316`

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
from collections import Counter
from subprocess import check_output
from core import censo_escritas as atual, portas

caminho = 'app/core/censo_escritas.py'
ref = '63680b06e97b766895151a423f771215528cd721'
base = {}
fonte = check_output(['git', 'show', f'{ref}:{caminho}'], text=True)
exec(compile(fonte, caminho, 'exec'), base)

def medir(rotulo, varrer, tabela):
    achados = varrer('.')
    familias, _, numeros = tabela(achados)
    gate = Counter(x['entidade'] for x in achados
                   if x['entidade'] and portas.familia_de(x['entidade']) == 'chamado'
                   and x['arquivo'] not in (portas.porta_de(x['entidade']) or ()))
    campos = ('arquivo', 'linha', 'entidade', 'op', 'expr')
    print(rotulo, len(achados), numeros, familias['chamado']['sitios'], dict(gate))
    return Counter(tuple(x[k] for k in campos) for x in achados)

antes = medir('antes', base['varrer_arvore'], base['tabela'])
depois = medir('depois', atual.varrer_arvore, atual.tabela)
print('removidos', antes - depois, 'adicionados', depois - antes)
PY
```

Suite, execução dos mutantes e produção [nao verificado]. [nao verificado]

## 28/11 10:20 · sessao 292486b76990 · FDS-FERIAS-COM-BATIDA-PORTA · construtor · origem=Code

Alterei apenas `folha/retrato_folha.py`. O Calendário abre um link por dia de turno publicado. **Na lavra atual de férias, falta `acoes[].dia_do_turno`**, pois a linha recebe `acoes=[]` e guarda as datas civis em `datas`. Nesse caso, o drawer oferece apenas a correção do lançamento de férias. [retrato_folha.py:1049](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/retrato_folha.py:1049) · [retrato_folha.py:1302](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/retrato_folha.py:1302) · [retrato_folha.py:1316](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/retrato_folha.py:1316) · [fora_da_folha.py:156](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/fora_da_folha.py:156)

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `acoes[].dia_do_turno` | A tarefa recebe o dia do turno, `porta_da_tarefa` o publica em `resolver_dia`, `fora_da_folha` copia em ISO e `montar_linhas` leva as ações ao retrato. [tarefa_do_dia.py:214](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/chamados/services/tarefa_do_dia.py:214) · [tarefa_do_dia.py:820](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/chamados/services/tarefa_do_dia.py:820) · [fora_da_folha.py:231](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/fora_da_folha.py:231) · [retrato_folha.py:1316](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/retrato_folha.py:1316) | Férias e Resolver dia usam a mesma função de leitura do campo. Cada botão de férias mostra a data ISO do turno. [retrato_folha.py:1042](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/retrato_folha.py:1042) · [retrato_folha.py:1087](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/retrato_folha.py:1087) · [retrato_folha.py:1111](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/retrato_folha.py:1111) |
| `motivo` e `tipo` | `classificar_export` escreve o motivo. `montar_linhas` o copia e grava o tipo calculado por `_tipo_cru` via `TIPO_POR_MOTIVO`. [export.py:894](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/export.py:894) · [retrato_folha.py:252](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/retrato_folha.py:252) · [retrato_folha.py:1308](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/retrato_folha.py:1308) · [retrato_folha.py:1317](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/retrato_folha.py:1317) | `tipo_da_linha` lê o tipo gravado e consulta o motivo quando precisa recuperar o tipo. As opções de férias são selecionadas pelo tipo, com a correção em `PORTA_DO_TIPO['ferias']`. Atualizei o comentário e a docstring. [retrato_folha.py:530](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/retrato_folha.py:530) · [retrato_folha.py:990](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/retrato_folha.py:990) · [retrato_folha.py:1015](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/retrato_folha.py:1015) · [retrato_folha.py:1072](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/retrato_folha.py:1072) |
| Texto de “O que destrava” | `o_que_fazer` compõe o texto com os rótulos das mesmas opções usadas pelos botões. Para os demais tipos, lê `DESTRAVA` pelo fio. [retrato_folha.py:1146](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/retrato_folha.py:1146) · [retrato_folha.py:1170](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/retrato_folha.py:1170) · [fora_da_folha.py:47](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/fora_da_folha.py:47) | O painel passa texto e portas ao template. `o_que_segura` também lê `portas_da_linha` com uma linha própria de tipo `falta`. [colaboradores/views.py:1006](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/colaboradores/views.py:1006) · [_retrato_o_que_fazer.html:33](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/templates/folha/_retrato_o_que_fazer.html:33) · [_retrato_o_que_fazer.html:73](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/templates/folha/_retrato_o_que_fazer.html:73) · [fora_da_folha.py:297](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/fora_da_folha.py:297) |

**Casos RED e mutantes para o Opus**

1. **Virada no primeiro dia de férias.** Batida às 07h de `01/11`, com `dia_do_turno` publicado em `31/10`. Exigir `modo=custom`, ambos os limites em `31/10`, `mes=10` e a porta de Férias. Repetir com `acoes=[]` e exigir somente Férias. Mutante a matar: usar `datas[0]` para montar o link. [retrato_folha.py:1060](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/retrato_folha.py:1060) · [retrato_folha.py:1091](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/retrato_folha.py:1091) [nao verificado]
2. **Mais de um dia.** Publicar dois dias de turno distintos e repetir um deles. Exigir dois links de Calendário, cada um com o próprio dia, e uma porta de Férias. Mutantes a matar: usar só a primeira ação ou emitir um link repetido. [retrato_folha.py:1060](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/retrato_folha.py:1060)
3. **Drawer e portas.** Com turno publicado, exigir as duas opções no texto. Sem turno publicado, exigir só a correção de férias. Alterar o rótulo de `PORTA_DO_TIPO['ferias']` durante o teste e exigir a alteração no botão e no texto. Mutantes a matar: ler a frase do fio admin em férias ou manter uma frase independente dos botões. [retrato_folha.py:1028](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/retrato_folha.py:1028) · [retrato_folha.py:1170](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/retrato_folha.py:1170) [nao verificado]
4. **Controle de tipo.** Verificar uma linha de `falta` com fio admin e `acoes=[]`, que conserva “Validar no fio” e a frase de `DESTRAVA['admin']`. Verificar também férias sem `tipo` gravado, recuperado pelo mapa, e férias com `tipo='ferias'` gravado e outro motivo na fixture. Mutantes a matar: decidir as portas olhando diretamente o motivo ou aplicar as portas de férias a `falta`. [retrato_folha.py:530](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/retrato_folha.py:530) · [retrato_folha.py:1055](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/retrato_folha.py:1055) · [retrato_folha.py:1123](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/retrato_folha.py:1123)

`git diff --check` e a compilação em memória passaram. Os testes existentes ainda esperam um link pela data civil e precisam da revisão do Opus. Suíte e produção [nao verificado]. [retrato_folha.py:1072](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/retrato_folha.py:1072) · [test_retrato_portas_ferias_com_batida.py:60](/home/ronald/wt-codex-fds-ferias-com-batida-porta-101539/app/folha/tests/test_retrato_portas_ferias_com_batida.py:60) [nao verificado]

## 28/11 10:11 · sessao 6e7e9bd87fa6 · DESLIGADO-PORTA-DO-CHAMADO · construtor · origem=Code

Implementei a c5 em 10 arquivos de produção. O diff passou em `git diff --check`, os módulos Python alterados passaram na leitura por AST e a contagem de `except` permaneceu igual. Suíte, REDs, concorrência real e comportamento em produção [nao verificado]. [nao verificado]

## Mudanças por item

| Item | Arquivo e função | O que ficou na cópia |
|---|---|---|
| 1 e 6 | `colaboradores/services/desligamento.py:90` `executar_desligamento` | Trava o cadastro, relê a instância e reaplica `julgar_desligamento` no mesmo `atomic` da escrita. Usa a situação relida no S89. Na efetivação agendada, recusa se a data ou o motivo mudaram depois da seleção. `colaboradores/services/desligamento.py:100`, `colaboradores/services/desligamento.py:180`. |
| 1 e 6 | `colaboradores/views.py:1474` `desligar_colaborador` e `colaboradores/management/commands/efetivar_desligamentos_agendados.py:31` `Command._rodar` | Cada borda captura `timezone.localdate()` uma vez e passa o mesmo dia ao juiz e ao executor. O POST distingue recusa, agendamento e efetivação. A CLI só soma a efetivação aceita. `colaboradores/views.py:1482`, `colaboradores/management/commands/efetivar_desligamentos_agendados.py:37`, `colaboradores/management/commands/efetivar_desligamentos_agendados.py:51`, `config/settings/base.py:93`. |
| 2 | `chamados/services/acoes_disputa.py:274` `acao_reabrir_disputa_admin` | Trava também as disputas ligadas por pergunta-destino antes do chamado. Para cadastro não desligado, exige `ch.reabrir() is True` antes de reabrir a disputa, criar pergunta, anotar ou notificar. A recusa desfaz inclusive a migração que `reabrir` pode tentar quando há irmão vivo. A eleição da disputa continua compartilhada com o fio e a condição vigente para pai vivo de desligado segue sob LU-12. `chamados/services/acoes_disputa.py:302`, `chamados/services/acoes_disputa.py:315`, `chamados/services/acoes_disputa.py:335`, `chamados/services/fio.py:160`, `chamados/models.py:1598`. |
| 5a e 5b | `chamados/services/historico.py:69` `_eventos_dos_chamados` e `chamados/services/trilha_reabertura.py:14` `reabertura_assinada` | O histórico mostra encerramento da pergunta por desligamento e conserva seus eventos de resposta e validação. Identifica autoria assinada com FK nula. A testemunha `recusada_desligamento` e a classificação I5 pela última travessia assinada já estavam na base. `chamados/services/historico.py:92`, `chamados/services/historico.py:101`, `chamados/services/materializacao.py:868`, `chamados/management/commands/auditar_invariantes_chamados.py:673`. |
| 7 | `api/views.py:388` e `api/views_core.py:374` `api_bater_ponto` | Os dois kernels mantêm o sucesso e o ID da batida. Se a emissão devolve `None`, respondem `em_disputa=False` e `motivos_disputa=[]`. Os motivos ainda alimentam o aviso da batida. `api/views.py:768`, `api/views.py:811`, `api/views_core.py:762`, `api/views_core.py:805`. |
| 7 | `chamados/services/regularizacao_ext.py:310` `abrir_lote` | O dedup roda sob a trava do cadastro e só devolve lote com chamado vinculado. No desligado, a recusa passa pela porta de criação e chega às duas bordas antes de gravar lote ou itens. Um lote legado sem chamado fica fora do dedup. `chamados/services/regularizacao_ext.py:237`, `chamados/services/regularizacao_ext.py:319`, `chamados/views_regularizacao.py:54`, `colaboradores/views.py:3049`. |
| 7 | `chamados/services/acoes_disputa.py:217` `nucleo_pedir_confirmacao_ancora` e `templates/ponto/triagem/fora_horario.html:13` | O aviso de âncora informa que nenhuma nova pergunta foi enviada. O formulário de anuência exibe a recusa já produzida pelo serviço. O formulário de abertura de chamado já exibia a mensagem na base. `chamados/services/acoes_disputa.py:228`, `ponto/services/triagem_batida.py:349`, `templates/chamados/form_abertura.html:12`. |

No detalhe e no fio rápido, o `ValueError` de `transicionar` já chega ao envelope como `str(_ve)`. O detalhe usa `resposta_acao`, que envia o texto no toast HTMX. O desarquivamento usa o booleano de `reabrir` e devolve a recusa no JSON. `chamados/services/acoes_chamado.py:326`, `chamados/services/acoes_chamado.py:336`, `chamados/services/fio.py:873`, `chamados/views.py:426`, `core/respostas.py:63`, `chamados/views_arquivar.py:65`.

## Item 7 por chamador e selos para o opus

Nos sítios abaixo, a guarda indicada já existia no HEAD. Para cada um, o opus pode usar a chave existente com cadastro desligado e o controle com cadastro ativo. Quando um filtro anterior impedir chegar à porta, a costura deve devolver `(None, False)` no ponto da chamada. Esses casos são caracterizações da guarda, com mutantes próprios. Os controles de chave ausente e veto O261 continuam no roteiro de verificação. `chamados/models.py:940`, `chamados/tests/test_desligado_porta_do_chamado.py:479`, `ponto/management/commands/reconciliar_grade.py:527`.

| Chamador | `None` no HEAD e na c5 | Caso e mutante |
|---|---|---|
| API justificativa | HTTP 403 antes de buscar ou atualizar pendente e associar o chamado. `api/views.py:1984`, `api/views.py:1999`. | Plante pendente e preserve seus campos. Mutante deixa seguir para `find-or-answer`. |
| Reconciliador C2 | Retorna `False` antes de ler o status ou costurar a pergunta. `chamados/reconciliador.py:1032`, `chamados/reconciliador.py:1055`. | Preserve destino e disputa. Mutante lê `ch_novo.status_local`. |
| Adesão | Pula antes dos contadores, notas e agrupamento. `chamados/services/adesao.py:76`, `chamados/services/adesao.py:89`. | Preserve pai, filhos e placar. Mutante usa o pai ausente ou conta `reabrir=False`. |
| Emissão comum | Retorna `(None, False, [])` antes de nota ou criação da disputa. `chamados/services/disputa_emissao.py:582`, `chamados/services/disputa_emissao.py:598`. | Espere zero disputa, pergunta e push. Mutante cai na recorrência. |
| Repasse de órfãos | Desfaz a tentativa e devolve `REPASSE_RECUSADO`. `chamados/services/disputa_emissao.py:1128`, `chamados/services/disputa_emissao.py:1144`, `chamados/services/disputa_emissao.py:1320`. | Induza recusa com um filho já complementado e outro ainda sem pai. Mutante retorna de dentro do `atomic` e deixa persistir o complemento. |
| Emissão de âncora | Devolve `None` antes da nota, pergunta, `analisar` e push. A c5 ajusta o aviso da borda. `chamados/services/disputa_emissao.py:1366`, `chamados/services/disputa_emissao.py:1382`, `chamados/services/acoes_disputa.py:229`. | Espere zero efeitos e aviso coerente. Mutante cai no reuso ou anuncia envio. |
| Mensagem rápida | Renderiza a recusa antes da nota, push e atualização do fio. `chamados/views.py:998`, `chamados/views.py:1011`. | Espere o alerta com zero nota e push. Mutante remove a guarda. |
| Régua degenerada | Descarta o retorno acessório e mantém `True` para o veto. `escala/regua_defesa.py:51`, `escala/regua_defesa.py:80`. | Mutante faz o veto depender de `criado`. |
| Ausências, limbo de folgas | `criado=False` impede a sugestão. `ponto/management/commands/detectar_ausencias.py:191`, `ponto/management/commands/detectar_ausencias.py:235`. | Mutante chama `aplicar_sugestao` com `None`. |
| Ausências, limbo de âncora | Pula antes de `_ch.status_local`. `ponto/management/commands/detectar_ausencias.py:272`, `ponto/management/commands/detectar_ausencias.py:301`. | Mutante lê o status ou tenta reabrir. |
| Ausências, dia sem grade | Suprime a nota de recorrência. `ponto/management/commands/detectar_ausencias.py:513`, `ponto/management/commands/detectar_ausencias.py:538`. | Mutante guarda apenas `not _cr`. |
| Ausências, emissão de furo | Retorna `VETO_NA_PORTA` antes do status, espelho dos marcos e fio. `ponto/management/commands/detectar_ausencias.py:696`, `ponto/management/commands/detectar_ausencias.py:740`. | Mutante deixa a recusa chegar ao espelho ou à emissão como sucesso. |
| Retroativo | Classifica a recusa e deixa o contador de emitidos intacto. `ponto/management/commands/emitir_turno_aberto_retroativo.py:151`, `ponto/management/commands/emitir_turno_aberto_retroativo.py:170`. | Cubra recusa genérica, célula, veto e emissão. Mutante conta antes do retorno. |
| Alerta crítico de turno | `criado=False` suprime o push de emissão. O chamador também barra alerta e justificativa. `ponto/management/commands/processar_alertas_turno.py:317`, `ponto/management/commands/processar_alertas_turno.py:335`, `ponto/management/commands/processar_alertas_turno.py:355`. | Mutante retira a guarda no marco pós-20. |
| Reconciliação de grade | Retorna `VETO_NA_PORTA` antes do status ou reabertura. `ponto/management/commands/reconciliar_grade.py:507`, `ponto/management/commands/reconciliar_grade.py:527`. | Mutante remove a guarda ou contabiliza como emissão. |
| Aviso de ausência | Devolve `None` e suprime o push condicionado a `criado`. `ponto/services/ausencia.py:805`, `ponto/services/ausencia.py:826`. | Mutante envia push mesmo com recusa. |
| Cluster espúrio | `criado=False` deixa o contador de abertos intacto. `ponto/services/cluster_espurio.py:196`, `ponto/services/cluster_espurio.py:214`. | Mutante incrementa na tentativa. |
| Ping geo | Preserva resposta de geofence e suprime push. `ponto/views.py:3191`, `ponto/views.py:3203`. | Mutante dispara o push sem chamado. |

Os complementos de `criar` também pedem selos por borda. No próprio e no DP, recusa sem push, GLPI, trilha de sucesso ou redirect ao PK. Na API, sem nota, push ou link. No benefício, sem solicitação. Na agenda, rascunhos preservados. No vínculo, sem substituição do aceite anterior. Os mutantes removem a guarda de cada porta. `chamados/services/abertura.py:66`, `chamados/services/abertura.py:174`, `chamados/services/abertura.py:200`, `chamados/views.py:141`, `chamados/views.py:217`, `api/views.py:2202`, `colaboradores/views.py:2045`, `colaboradores/views.py:2439`, `colaboradores/services/vinculo.py:423`.

Na anuência, preserve sessão e confira a mensagem renderizada. Na triagem de horário, preserve a batida sem PK de chamado nem sessão vinculada. Em posto e férias, preserve o diagnóstico sem push de chamado. Na entrada com turno aberto, preserve a batida sem anunciar primeira emissão. Na rejeição de justificativa, preserve a decisão e seu push próprio mesmo quando o chamado acessório recusar. No smoke, identifique a recusa antes das arestas. Mutantes retiram a guarda ou vinculam à recusa um efeito reservado à emissão. `ponto/services/triagem_batida.py:349`, `ponto/services/triagem_batida.py:488`, `templates/ponto/triagem/fora_horario.html:13`, `ponto/utils.py:58`, `ponto/utils.py:130`, `ponto/views.py:322`, `ponto/services/justificativa.py:402`, `ponto/services/justificativa.py:429`, `chamados/management/commands/smoke_chamados.py:124`.

## REDs da c5 para o opus

- **1 e 6.** Com duas conexões, faça o executor receber uma instância anterior a outro desligamento. Confira recusa, data e motivo preservados, uma cascata e um S89 efetivo. Repita com agendamento vencido reagendado enquanto a CLI espera a trava. Congele um instante em que São Paulo e UTC estejam em datas diferentes e confira a mesma decisão no POST e na CLI. Mutantes usam instância anterior, honram o agendamento velho, releem o relógio ou contam a recusa. `colaboradores/services/desligamento.py:100`, `colaboradores/services/desligamento.py:172`, `colaboradores/views.py:1482`, `colaboradores/management/commands/efetivar_desligamentos_agendados.py:51`.
- **2.** Cubra pai vivo, pai terminal aceito, recusa por irmão vivo e limite de quota. Na recusa, compare fecho, quota, perguntas, notas e pushes. Na corrida, B segura outra disputa fechada ligada por destino. Quando A estiver esperando essa disputa, B tenta travar o chamado com `NOWAIT` em savepoint. O mutante omite a outra disputa da trava inicial e já segura o chamado nesse ponto. Preserve o controle c4 da própria disputa ressuscitada com uma quota. `chamados/services/acoes_disputa.py:309`, `chamados/models.py:1768`, `chamados/tests/test_desligado_porta_do_chamado.py:545`.
- **5a e 5b.** Compare o carimbo de desligamento com uma aplicação efetiva e preserve resposta e validação no histórico. Apague o usuário de uma reabertura assinada e cubra chave ausente com FK nula, ID inválido, ID divergente e travessia automática posterior. Mutantes retiram a via do mapa, tratam todo carimbo como fato ou aceitam assinatura sem prova. `chamados/services/historico.py:92`, `chamados/services/historico.py:118`, `chamados/services/materializacao.py:869`, `core/observ.py:106`, `chamados/services/trilha_reabertura.py:5`.
- **7.** Em cada kernel, preserve ID e sucesso da batida na recusa, com flag falsa e lista vazia. No lote, teste recusa com dedup existente, controle ativo com fio e dois POSTs concorrentes. Mutantes forçam a flag, devolvem dedup antes da trava ou criam lote antes do chamado. Para o C12, o opus pode ancorar por arquivo, escopo e chamada no AST, exigindo uma ocorrência e deixando a linha para diagnóstico. Mutantes deslocam a chamada, duplicam a chamada autorizada e introduzem um escritor externo. `api/views.py:768`, `api/views_core.py:762`, `chamados/services/regularizacao_ext.py:319`, `chamados/tests/e4b_c12_ast.py:17`, `chamados/tests/e4b_c12_ast.py:224`.

## Quem escreve e quem lê

| Campos | Escritor | Leitor nesta volta |
|---|---|---|
| `Colaborador.situacao`, `data_demissao`, `motivo_desligamento` | Executor chama `gravar` no agendamento e `salvar_inteiro` na efetivação. `colaboradores/services/desligamento.py:131`, `colaboradores/services/desligamento.py:150`. | Juiz, executor após a trava e seleção da CLI. Lote e reabertura administrativa consultam `esta_desligado`. `colaboradores/services/desligamento.py:29`, `colaboradores/services/desligamento.py:101`, `colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`, `chamados/services/regularizacao_ext.py:320`, `chamados/services/acoes_disputa.py:303`. |
| S89 e agendamento, `valor_antes`, `valor_depois`, autoria e instante | Executor monta o payload. `core.observ.evento` insere. `colaboradores/services/desligamento.py:141`, `colaboradores/services/desligamento.py:173`, `core/observ.py:143`. | Auditoria geral e sua tela. `core/views.py:705`, `templates/core/log_auditoria.html:59`. |
| Acesso, histórico de vínculo e vigência | `definir_acesso` lê e grava `is_active`. Executor encaminha histórico e data de fim pelas portas existentes. `core/credenciais.py:107`, `colaboradores/services/desligamento.py:156`, `colaboradores/services/desligamento.py:159`, `colaboradores/services/desligamento.py:168`. | A própria porta evita regravar o acesso idêntico. `fechar_vinculos_ativos` lê os vínculos ativos. `core/credenciais.py:110`, `colaboradores/services/vinculo.py:865`. |
| Estado, via e instante do chamado, motivo e autor da reabertura | `_gravar_estado` persiste o estado e emite a trilha. `core.observ.evento` carimba `autor_reabertura_id`. `chamados/models.py:274`, `chamados/models.py:340`, `core/observ.py:105`. | `reabrir` consulta o estado e o histórico e o I5 leem a assinatura. `chamados/models.py:1543`, `chamados/services/historico.py:92`, `chamados/management/commands/auditar_invariantes_chamados.py:673`. |
| Fecho, resolução, quota e motivos da disputa, pergunta e nota | `reabrir_admin` grava a disputa. `abrir_conversa` e `anexar_ocorrencia` escrevem pergunta e nota. `chamados/models.py:2561`, `chamados/models.py:3260`, `chamados/services/nota.py:102`. | A ação e o fio elegem a disputa por `fechada_em`. O histórico lê a nota. `chamados/services/acoes_disputa.py:304`, `chamados/services/fio.py:160`, `chamados/services/historico.py:81`. |
| `materializada_em`, `materializada_por`, `via_resolucao`, resposta e validação | `fechar_por_desligamento` usa `carimbar` e `_escrever_carimbo`. A sequência de decisões fornece a leitura de resposta e validação. `chamados/models.py:3526`, `chamados/models.py:3593`, `chamados/services/historico.py:101`. | Testemunha de aplicação e histórico. `chamados/services/materializacao.py:893`, `chamados/services/historico.py:105`. |
| Vínculo do lote e campos de resposta | `criar_regularizacao_externa` grava lote, `chamado` e itens. Os kernels montam `em_disputa` e `motivos_disputa`. A triagem preenche `anuencia_erro`. `chamados/services/regularizacao_ext.py:249`, `api/views.py:820`, `api/views_core.py:814`, `ponto/services/triagem_batida.py:350`. | Dedup do lote, consumidores da resposta HTTP [nao verificado] e template de anuência. `chamados/services/regularizacao_ext.py:321`, `templates/ponto/triagem/fora_horario.html:13`. |

## Perguntas de lei

1. **Q1/I5f.** Uma reabertura humana assinada anterior à efetivação deve permanecer viva no passivo ou entrar na cascata? `chamados/management/commands/auditar_invariantes_chamados.py:673`, `colaboradores/services/desligamento.py:196`.
2. **LU-12.** Qual autorização, se houver, permite reabrir a disputa administrativa de desligado e criar nova pergunta? `chamados/services/acoes_disputa.py:314`, `chamados/models.py:1679`.
3. **Pai vivo de cadastro ativo.** Apliquei a exigência de aceite explícito da c5. O motor só aceita `REABRIR` de terminal, enquanto dois controles c3c esperam sucesso com pai aberto. Deve a lei manter a recusa aplicada nesta volta ou permitir reabrir exclusivamente a disputa nesse caso? Resultado da suíte [nao verificado]. `chamados/services/acoes_disputa.py:315`, `chamados/catalogo/motor.py:113`, `chamados/tests/test_e4c_f5_82c3c_cartao_misto.py:139`, `chamados/tests/test_e4c_f5_82c3c_cartao_misto.py:178`.
4. **Via nominal do chamado.** Qual evidência autoriza atribuir `cascade` legado a desligamento? A via nominal depende de pouso conjunto do escritor, do catálogo, do guarda de renascimento e dos leitores. O rito único do item 4 também segue com o opus na origem do fecho. `chamados/catalogo/motor.py:88`, `chamados/models.py:1132`, `chamados/models.py:1918`, `colaboradores/services/desligamento.py:253`, `chamados/models.py:2180`.
5. **Instância humana anterior ao fecho.** A matriz deve aceitar a reabertura quando a instância via o chamado vivo e o estado relido sob trava já é terminal? `chamados/models.py:148`, `chamados/models.py:802`.

## 28/11 10:05 · sessao 061f43170c1f · - · - · origem=Ronald (tmux)

Conferi as bordas do item 7 sem editar arquivos.

- O formulário de chamado próprio e o do DP já recebem a recusa antes do redirect. O dedup do DP relê o cadastro sob trava e registra a recusa. O template já apresenta a mensagem de erro. `chamados/services/abertura.py:66,167,174,200`, `chamados/views.py:141,217`, `templates/chamados/form_abertura.html:12`.
- O lote novo já depende do chamado, dentro do mesmo `atomic`. As duas bordas já tratam `None`. O caminho de dedup ainda devolve um lote existente antes de consultar o cadastro. `chamados/services/regularizacao_ext.py:235,247,313`, `chamados/views_regularizacao.py:54`, `colaboradores/views.py:3049`.
- O aceite de vínculo já recusa antes de substituir o aceite anterior, notificar ou auditar a abertura. O `ValueError` desfaz a transação do vínculo e chega à borda. `colaboradores/services/vinculo.py:169,387,423,429,440`, `colaboradores/views.py:451`.
- Anuência já guarda `None` e preserva a sessão, mas o template servido não mostra `anuencia_erro`. O horário já continua pelo contrato da batida, com PK e sessão condicionados a chamado presente. Posto e férias já devolvem a condição de bloqueio com `None`, sem push vinculado. `ponto/services/triagem_batida.py:349,381,488,499`, `ponto/views.py:358`, `templates/ponto/triagem/fora_horario.html:3`, `ponto/utils.py:58,130`.
- Os dois kernels descartam a emissão acessória de auditoria. A rejeição de justificativa grava a decisão antes de tentar abrir chamado, e seus pushes descrevem a rejeição sem apontar para o chamado. `api/views.py:720`, `api/views_core.py:714`, `ponto/services/justificativa.py:402,411,429`.
- O selo C12 mora na pasta de testes. Para o opus, identificar cada chamada permitida por arquivo, escopo e AST, exigir uma ocorrência e manter a linha para diagnóstico. Os mutantes devem deslocar o corpo, duplicar uma chamada autorizada e introduzir um escritor em outro escopo. `chamados/tests/e4b_c12_ast.py:16,224`.
- 5a e 5b já constam do HEAD. Os controles devem remover a entrada de desligamento no mapa de recusas, remover a autoria do payload e aceitar indevidamente uma assinatura ausente, inválida ou divergente da FK. O histórico ainda traduz o carimbo por desligamento como fato registrado. `chamados/services/materializacao.py:868,893`, `core/observ.py:105`, `chamados/services/trilha_reabertura.py:14`, `chamados/management/commands/auditar_invariantes_chamados.py:673`, `chamados/services/historico.py:104`.

## 28/11 10:05 · sessao 02117846da0a · - · - · origem=Ronald (tmux)

As guardas diretas de `abrir` já estão no HEAD. Justificativa para antes de criar ou atualizar a pendente, mensagem rápida para antes de nota e push, e adesão pula a recusa antes de reagrupar. A emissão comum e a âncora devolvem `None` antes dos efeitos. O repasse usa `REPASSE_RECUSADO` e desfaz o complemento anterior dos filhos. `api/views.py:1985-2023`, `chamados/views.py:998-1021`, `chamados/services/adesao.py:76-125`, `chamados/services/disputa_emissao.py:582-619`, `chamados/services/disputa_emissao.py:1080-1155`, `chamados/services/disputa_emissao.py:1320-1326`, `chamados/services/disputa_emissao.py:1366-1411`.

Os dois kernels descartam o retorno da emissão e respondem `em_disputa=True` mesmo quando ela devolve `None`. O RED de cada kernel deve preservar a batida gravada e seu ID, esperar `em_disputa=False` e `motivos_disputa=[]` na recusa e incluir o controle de emissão aceita. O mutante reinstala a flag incondicional. `api/views.py:758-771`, `api/views.py:813-824`, `api/views_core.py:752-765`, `api/views_core.py:807-818`, `chamados/services/disputa_emissao.py:678-689`.

O RED de repasse deve induzir a recusa na abertura com um filho elegível a complemento e outro que ainda exija pai. O esperado é rollback do complemento, preservação dos estados e perguntas, ausência de push e a trilha `repasse_recusado`. O mutante devolve antecipadamente o resultado, deixando o complemento persistir. `chamados/services/disputa_emissao.py:1084-1146`, `chamados/services/disputa_emissao.py:1316-1326`.

O lote novo já evita qualquer escrita de lote ou item se `criar` recusar, e as duas bordas refletem esse retorno. O dedup de `abrir_lote` ainda pode devolver um lote legado antes da porta. A âncora já interrompe os efeitos na recusa, mas seu toast hoje confunde recusa com solicitação anterior ou resolução. `chamados/services/regularizacao_ext.py:231-259`, `chamados/services/regularizacao_ext.py:310-320`, `chamados/views_regularizacao.py:54-59`, `colaboradores/views.py:3049-3054`, `chamados/services/acoes_disputa.py:217-233`.

A mensagem do formulário já é entregue por `resposta_render` e pelo bloco `messages` do template. A reancoragem C12 fica em arquivo de teste proibido nesta volta. `chamados/views.py:141-148`, `chamados/views.py:217-224`, `core/respostas.py:74-86`, `templates/chamados/form_abertura.html:12-16`, `chamados/tests/e4b_c12_ast.py:17-31`.

As duas curas 5a e 5b estão no HEAD. A testemunha classifica a via de pergunta como recusa antes do atalho `aplicada`. A auditoria lê a identificação gravada pelo escritor antes da autoria da requisição, com suporte a FK apagada. `chamados/services/materializacao.py:868-907`, `core/observ.py:105-125`, `core/models.py:295`, `chamados/services/trilha_reabertura.py:5-22`, `chamados/management/commands/auditar_invariantes_chamados.py:671-679`.

Comportamento dinâmico e REDs [nao verificado].

## 28/11 10:05 · sessao 5f0462781f73 · - · - · origem=Ronald (tmux)

- **Item 2.** A ação pode travar a disputa-alvo e todas as disputas das perguntas destinadas ao pai antes de travar o chamado. A ressuscitação usa um subconjunto dessas perguntas. Para a corrida, uma conexão mantém a outra disputa fechada travada. Quando a ação esperar por essa disputa, a primeira conexão verifica com `NOWAIT` que ainda consegue travar o chamado. `chamados/services/acoes_disputa.py:300-312`, `chamados/models.py:1768-1780`.
- **Pai vivo.** `reabrir` aceita `resolvido` e `fechado`. Dois testes c3c esperam sucesso administrativo com pai `aberto`. A exigência de aceite de `ch.reabrir` para o pai vivo conflita com esse contrato de modelo e com os testes existentes. `chamados/catalogo/motor.py:109-115`, `chamados/tests/test_e4c_f5_82c3c_cartao_misto.py:82-86`, `:139-154`, `:178-193`.
- **Recusa.** O ramo `irmao_vivo` pode migrar perguntas e gravar nota antes de devolver `False`. O RED deve verificar fecho, quota, perguntas, notas e pushes. O controle c4 exige uma quota quando a ressuscitação já reabriu a própria disputa pelo sistema. `chamados/models.py:1598-1623`, `:1872-1874`, `chamados/tests/test_desligado_porta_do_chamado.py:545-572`.
- **Itens 5a/5b.** A base já distingue `recusada_desligamento` e valida a assinatura explicitamente gravada quando a FK é zerada. O histórico ainda atribui materialização ao carimbo de desligamento e sistema ao registro sem FK. Os REDs devem cobrir o carimbo sem fato, autor excluído, assinatura ausente ou inválida e reabertura automática posterior. `chamados/services/materializacao.py:868-899`, `core/observ.py:106-112`, `chamados/services/trilha_reabertura.py:5-22`, `chamados/management/commands/auditar_invariantes_chamados.py:673-679`, `chamados/services/historico.py:91-116`.
- **Via nominal do chamado.** `encerrar` tem via fixa e serve à cascata, à regularização e ao vínculo. Um novo valor exige mudar o escritor e o guarda do renascimento juntos, com o rótulo do cluster e o selo que enumera vias literalmente. `chamados/models.py:1123-1132`, `:1904-1919`, `colaboradores/services/desligamento.py:234-236`, `chamados/services/regularizacao_ext.py:206`, `colaboradores/services/vinculo.py:429`, `ponto/tests/test_o169_cluster_encerrado_sem_lastro.py:236-250`.
- **Itens 1/6.** A função de trava retorna uma instância limitada a PK e situação. O executor deve reler o cadastro inteiro sob a trava e o cron deve passar ao juiz o mesmo `hoje` capturado por `localdate`. O caso concorrente de agendamento tem de cobrir reagendamento para o futuro após a seleção inicial. `chamados/models.py:3127-3133`, `colaboradores/services/desligamento.py:88-137`, `colaboradores/management/commands/efetivar_desligamentos_agendados.py:37-58`.

Verificação por leitura de código. Resultado dos testes [não verificado]. [nao verificado]

## 28/11 09:36 · sessao 095f250de144 · FDS-FERIAS-COM-BATIDA-PORTA · construtor · origem=Code

Ajustei `portas_da_linha` para oferecer **Calendário do dia (retratar marcação)** e **Férias do colaborador (corrigir lançamento)**, nessa ordem. O Calendário abre no primeiro dia gravado. Se a linha vier sem datas, aparece só a porta de Férias. O lavrador já copia `dias_ferias_com_batida` do export para `datas` no Retrato, e o roteamento lê essa lista. `app/folha/retrato_folha.py:1078` · `app/folha/retrato_folha.py:1297` · `app/folha/retrato_folha.py:1305`

### Leitores

- **Painel do colaborador e drawer do Retrato.** O painel chama `portas_da_linha` para a linha gravada, e o template exibe os botões na ordem recebida. O drawer abre esse mesmo painel com a competência selecionada. `app/colaboradores/views.py:1007` · `app/templates/folha/_retrato_o_que_fazer.html:73` · `app/templates/folha/_retrato_lista.html:61`
- **`fora_da_folha::o_que_segura`.** Continua devolvendo em `dias[].portas` os links da tarefa de cada dia, montados a partir de uma linha de tipo `falta`. `app/folha/fora_da_folha.py:286` · `app/folha/fora_da_folha.py:297`
- **PDFs.** O PDF do Retrato lê o recorte da barra. O PDF de retidos lê os itens de `classificar_export`. As duas renderizações seguem por essas fontes. `app/folha/views_retrato.py:394` · `app/folha/views_retrato.py:437` · `app/folha/views.py:885`

### Casos RED para o Opus

1. **Férias com batida em 07/11/2026.** Entrada `colab=344`, `motivo='ferias_com_batida'`, `acoes=[]`, `datas=['2026-11-07']`. Esperado, nesta ordem, **Calendário do dia (retratar marcação)** com `/colaboradores/344/calendario/?ano=2026&mes=11&modo=custom&data_ini=2026-11-07&data_fim=2026-11-07` e **Férias do colaborador (corrigir lançamento)** com `/ferias/344/secao/`. Repetir com `2026-11-08` como segunda data e exigir os mesmos links. Mutantes a derrubar incluem manter o fio, inverter a ordem, usar a última data ou abrir o mês inteiro. `app/folha/retrato_folha.py:1078` · `app/colaboradores/urls.py:24` · `app/colaboradores/services/calendario.py:227` · `app/ferias/urls.py:14`
2. **`furo_espelho`.** Entrada `colab=344`, `dono='admin'`, `tipo='falta'`, `acoes=[]`. Esperado apenas **Validar no fio** com `/chamados/modal/344/`. Mutante a derrubar é aplicar as portas de férias a toda linha com `dono='admin'`. `app/folha/retrato_folha.py:1024` · `app/folha/retrato_folha.py:1118` · `app/chamados/urls.py:39`
3. **Férias com batida sem datas.** Mesma linha do primeiro caso, com `datas` ausente, `None` ou `[]`. Esperado apenas **Férias do colaborador (corrigir lançamento)** com `/ferias/344/secao/`. Mutantes a derrubar incluem inventar um dia para o Calendário, cair em **Validar no fio** ou devolver uma lista vazia. `app/folha/retrato_folha.py:1080` · `app/folha/retrato_folha.py:1089`

A conferência local foi de sintaxe e `git diff --check`. Suite, RED e navegação no drawer **[nao verificado]**. `app/folha/retrato_folha.py:1058`

## 28/11 09:30 · sessao 161b8cb42ffa · FDS-FERIAS-COM-BATIDA-PORTA · sonda · origem=Code

**O Retrato oferece “Validar no fio” nas duas linhas. A cópia tem ações para corrigir a batida ou o agendamento, conforme o fato apurado pelo admin. A lista do HJ-090 é de leitura e não tem uma ação que decida a sobreposição.** `app/folha/retrato_folha.py:1077` · `app/ponto/services/retratar_humano.py:146` · `app/ferias/views.py:470` · `app/ferias/management/commands/listar_ferias_com_batida.py:1`

Usei a saída raspada fornecida. A execução de `--sonda` por esta sessão ficou **[nao verificado]**. O envelope roda antes de chamar o Codex e grava arquivos intermediários. `bin/codex_pergunta.sh:15` · `bin/codex_pergunta.sh:628`

### 1. Juiz e portas existentes

- **Juiz da retenção.** `folha/export.py::dias_ferias_com_batida` cruza as datas dos agendamentos não cancelados com as batidas apuráveis pelo dia civil, dentro da competência. `classificar_export` recebe essas datas e grava `status='fora'`, `motivo='ferias_com_batida'` e `dias_ferias_com_batida`. É o juiz registrado para a entrada no TXT. `app/folha/export.py:464` · `app/folha/export.py:491` · `app/ferias/services.py:49` · `app/folha/export.py:892` · `app/core/juizes.py:336` · `app/docs/JUIZES.md:119`
- **BATIDA, quando a marcação não foi trabalho.** No Calendário de Ponto, a ação da marcação “Não foi trabalho (retratar)” chama `ponto.views.retratar_batida_view` → `ponto.services.retratar_humano.retratar`. A retratação exige motivo e retira a marcação do conjunto apurável. Se houver outra batida apurável no mesmo dia coberto, o juiz continua indicando a sobreposição. `app/colaboradores/views.py:1986` · `app/templates/colaboradores/partials/_calendario_grade.html:134` · `app/ponto/views.py:3453` · `app/ponto/services/retratar_humano.py:146` · `app/ponto/turnos.py:2961` · `app/folha/export.py:499`
- **CADASTRO, quando o lançamento de férias está errado.** A seção Férias do colaborador (`secao_ferias_colab`) oferece “Corrigir datas” → `editar_datas_admin` → `corrigir_datas_agendamento`. Também oferece cancelamento administrativo e correção justificada de férias concluídas, conforme o estado, ambas via `executar_cancelamento_admin`. Corrigir a janela pode retirar o dia da sobreposição. Mudar apenas o estado para `concluido` mantém a janela no universo do juiz. `app/ferias/views.py:851` · `app/templates/ferias/_secao_colab.html:143` · `app/ferias/views.py:541` · `app/ferias/services.py:736` · `app/templates/ferias/_secao_colab.html:159` · `app/ferias/views.py:397` · `app/ferias/views.py:451` · `app/ferias/services.py:662` · `app/ferias/services.py:53`
- **Lista do admin, HJ-090 item (7).** `listar_ferias_com_batida` é o comando que lista os dias e lê o dono L-099 via `InsumosDoDono`. O universo dele parte de `Ausencia(tipo='ferias')` e cruza batidas apuráveis. A retenção da Folha parte de `AgendamentoFerias`. O comando imprime o resultado. O `dono='admin'` gravado no Retrato é o fio da UI. O dono L-099 de cada dia da amostra ficou **[nao verificado]**. `app/docs/BACKLOG.md:2159` · `app/docs/LEIS.md:122` · `app/ferias/management/commands/listar_ferias_com_batida.py:1` · `app/ferias/management/commands/listar_ferias_com_batida.py:41` · `app/ferias/management/commands/listar_ferias_com_batida.py:57` · `app/folha/retrato_folha.py:1261`

### 2. Por que cai no fio

`_fio_cru` grava `admin` no ramo `motivo != 'furo_espelho'`. `TIPO_POR_MOTIVO` dá o tipo `ferias`. `dono_da_linha` traduz o fio para o balde **Admin** da UI. Para montar os links, `portas_da_linha` tenta primeiro as ações gravadas. Com `acoes=[]`, cai em `PORTA_DO_FIO['admin']`, que dá **Validar no fio**. `PORTA_DO_TIPO['ferias']` vale `None`. `app/folha/retrato_folha.py:385` · `app/folha/retrato_folha.py:229` · `app/folha/retrato_folha.py:497` · `app/folha/retrato_folha.py:1001` · `app/folha/retrato_folha.py:1021` · `app/folha/retrato_folha.py:1055` · `app/folha/retrato_folha.py:1077`

O ramo que colhe ações da `tarefa_do_dia` só percorre os retidos por `furo_espelho`. Para férias, o lavrador lê as datas do juiz do export e grava `acoes=[]`. O link passa o **PK do colaborador** ao modal, sem selecionar chamado ou dia. No modal, o botão Validar atua sobre uma `PerguntaDisputa`. O chamado 37604 veio como `registrado` no dado fornecido, e seu módulo `batida_fora_escala_leve` é de auditoria. A presença de um botão Validar no cartão específico do 37604 ficou **[nao verificado]**. `app/folha/fora_da_folha.py:156` · `app/folha/fora_da_folha.py:210` · `app/folha/retrato_folha.py:1250` · `app/folha/retrato_folha.py:1264` · `app/folha/retrato_folha.py:1082` · `app/chamados/urls.py:39` · `app/templates/chamados/partials/modal_fio.html:306` · `app/chamados/services/validacao.py:697` · `app/chamados/catalogo/natureza.py:18`

### 3. Linhas da sonda

As datas abaixo vêm da saída fornecida **[nao verificado em produção]**. Reproduzi os dois links com a função da cópia e a rota declarada. `app/folha/retrato_folha.py:1035` · `app/folha/retrato_folha.py:1077` · `app/config/urls.py:28` · `app/chamados/urls.py:39`

| Colaborador | Dia(s) | Porta atual | Porta que pode resolver |
|---|---|---|---|
| `col344` | 07/11/2026 | **Validar no fio** · `/chamados/modal/344/` | Calendário, se a marcação for indevida, ou seção Férias, se o lançamento estiver errado. Qual se aplica **[nao verificado]**. `app/ponto/services/retratar_humano.py:146` · `app/ferias/views.py:470` |
| `col976` | 07 a 20/11/2026, inclusive | **Validar no fio** · `/chamados/modal/976/` | As mesmas portas condicionais. Qual se aplica em cada dia **[nao verificado]**. `app/ponto/services/retratar_humano.py:146` · `app/ferias/views.py:470` |

**Classe do roteamento: 2 linhas, 15 dias-colaborador**, pelo dado fornecido e pelo mesmo fallback nas duas linhas. O dado associa um chamado específico apenas a `col344`. Chamados correspondentes em `col976` ficam **[nao verificado]**. `app/folha/retrato_folha.py:1001` · `app/folha/retrato_folha.py:1077`

### 4. Ponto de parada

**É pergunta de lei/desenho.** A lista HJ-090 entrega o diagnóstico ao admin, e as duas ações existentes dependem de qual fato ele apurar. O dado fornecido não permite escolher uma porta de decisão para a classe. O ponto do roteamento é o fallback em `portas_da_linha`, compartilhado pelo painel do colaborador e por `fora_da_folha::o_que_segura`. `app/ferias/management/commands/listar_ferias_com_batida.py:66` · `app/ferias/situacao.py:284` · `app/folha/retrato_folha.py:1077` · `app/colaboradores/views.py:1007` · `app/folha/fora_da_folha.py:289`

## 28/11 09:24 · sessao a8cc50a11a53 · E4D-CONTRATO-ROTA-B · verificador · origem=Code

**REPROVADO.** A apresentação pelo carimbo cru no cartão e a releitura do carimbo após a porta foram corrigidas. A catraca do fio continua vermelha em `espera_o_texto_do_colaborador`, tanto na cópia quanto com o delta invertido em memória. `app/templates/chamados/partials/_pergunta_card.html:20–43` · `app/api/views.py:2653–2665` · `app/chamados/services/fio.py:213–226` · [app/chamados/tests/test_e4c_f5_selo_catraca.py:338–349](/home/ronald/codex_ws/f300431afb8d70fff57d33183044e245c1724870/app/chamados/tests/test_e4c_f5_selo_catraca.py:338)

### 1. Apresentação e HTMX

O cartão usa `resposta_composta` para opacidade, selo de validação, hora da resposta e abertura das ações. A presença da resposta passa por `perg.respondida`, que chama o C21 com a composta. O censo dos arquivos tocados e dos caminhos GET e HTMX encontrou zero leitores de apresentação desses três carimbos pela coluna crua da pergunta. O serviço ainda lê colunas cruas para fotos, escrita e auditoria. `app/templates/chamados/partials/_pergunta_card.html:20–43` · `app/chamados/templatetags/chamados_tags.py:144–147` · `app/chamados/models.py:3958–3965` · `app/api/views.py:1132–1150,2567–2582,2653–2665` · `app/chamados/services/disputa_emissao.py:1523–1546,1578–1583,1698–1705,2185–2205`

O GET inclui o parcial após carregar `decisoes` e filtrar pelo C6. O HTMX renderiza o parcial diretamente após buscar o PK e também faz `prefetch_related('decisoes')`. A composta consome esse cache via `p.decisoes.all()`. O parcial calcula a composta para os carimbos e volta a calculá-la por `perg.respondida`, sobre as mesmas decisões carregadas. Contagem de consultas em Django [nao verificado]. `app/chamados/views.py:394–405,550–552` · `app/templates/chamados/detalhe_local.html:307–311` · `app/chamados/services/acoes_chamado.py:345–358` · `app/templates/chamados/partials/_pergunta_card.html:20` · `app/chamados/models.py:3958–3965,4482–4492,4506–4515`

### 2. Escritores, leitores e contrato de retorno

- **`ato_em`**. A porta humana fornece `_agora` no ato `RESPOSTA`. `DecisaoPergunta.registrar` persiste o lote e devolve os objetos criados. O recibo lê `ato_em` da última instância devolvida. Os leitores existentes incluem o redutor, o recorte temporal da tarefa e o histórico do modal. O campo registra o instante do ato. A regra de ajuste temporal é aplicada a `REABERTURA`, `DEVOLUCAO` e `RECUSA`. `app/chamados/services/disputa_emissao.py:1531–1534,1564–1571` · `app/chamados/models.py:4339–4346,4486–4504,4572–4587,4590–4623` · `app/chamados/services/tarefa_do_dia.py:383–403` · `app/templates/chamados/partials/modal_fio.html:204–212`
- **`com_recibo` e retorno**. Só a API passa `True`. A entrada reexportada encaminha a opção e o núcleo a lê para montar o retorno. A tupla sempre tem dois itens. O segundo contém uma mensagem na recusa, `None` no sucesso padrão ou um `datetime` no sucesso com recibo. No JSON, `respondida_em` identifica o instante do pedido no POST e o instante vigente nas listagens. `app/chamados/models.py:4763–4766` · `app/chamados/services/disputa_emissao.py:1471–1484,1567–1571,1618–1628` · `app/api/views.py:1132–1150,2567–2582,2653–2665`

| Chamador | Consumo do retorno |
|---|---|
| Web | Usa o padrão, desempacota `ok, erro` e lê `erro` na recusa. `app/chamados/views.py:1275–1283` |
| `declarar_hora_colab` | Usa o padrão e lê `erro` na recusa. `app/chamados/services/declaracao_texto.py:445–454` |
| `aplicar_declaracao` | Usa o padrão e lê `_erro` na recusa. `app/chamados/services/declaracao_texto.py:723–735` |
| `acoes_disputa` | Chama o núcleo diretamente, com o padrão, e lê `erro` na recusa. `app/chamados/services/acoes_disputa.py:416–418,442–447` |
| API | Pede recibo, usa `resultado` como mensagem na recusa e chama `isoformat()` no sucesso. `app/api/views.py:2653–2665` |

### 3. Qual ato o recibo devolve

- **Resposta nova.** A porta trava o colaborador, relê a pergunta, colhe `_agora`, salva a projeção e acrescenta a `RESPOSTA` deste pedido ao fim do lote. `registrar` insere na transação aninhada e a porta captura `atos[-1].ato_em` antes de sair do bloco externo. Na simulação em memória com outra resposta inserida após a saída desse bloco, a cópia devolveu o instante do pedido e o lado anterior devolveu o instante posterior. `app/chamados/services/disputa_emissao.py:1489–1503,1523–1534,1564–1571` · `app/chamados/models.py:2975–2980,4572–4587,4590–4623` · `app/api/views.py:2653–2665`
- **Reenvio aceito.** O núcleo grava outro ato mesmo com texto idêntico. `ja_respondida` afeta a trilha e o rótulo do follow-up. O recibo é do novo ato. Os atalhos de declaração que aceitam ou reaproveitam a resposta anterior pulam o núcleo e seguem o retorno próprio da declaração. `app/chamados/services/disputa_emissao.py:1523–1571,1586–1591,1602–1618` · `app/chamados/services/declaracao_texto.py:445–447,723–739` · `app/chamados/tests/test_porta_resposta_pergunta.py:196–218`
- **Recusa.** As guardas de vazio, valor inválido, absorção vigente e C6 retornam `(False, erro)` antes da gravação. A API envia a mensagem sem carimbo. Há zero ramos de sucesso sem ato novo no núcleo atual. A assertiva de `pedido_id` do caso 20 da matriz também fica insatisfeita na cópia e no lado anterior reconstruído. Execução do caso completo [nao verificado]. `app/chamados/services/disputa_emissao.py:1439–1484,1504–1524,1564–1571,1618` · `app/api/views.py:2637–2658` · `app/chamados/tests/test_e4d_matriz_reds.py:1451–1453`

### 4. Catracas e classes de reprovação

Executei **170 checagens em memória, com 169 aprovações e uma falha**, usando `unittest.TestCase` para as checagens sem banco e extraindo as constantes de modelo usadas pelos selos. A falha foi `fio.py:226`. A catraca exige origem composta nos dois ramos do `if`. Com `leitura` recebida por parâmetro, ela só consegue provar essa origem no ramo que compõe localmente. O montador atual entrega a composta. A falha já existe no lado anterior deste delta. Os selos de chamada, par ato/coluna e escritor único passaram. `app/chamados/services/fio.py:213–226,442–446` · `app/chamados/tests/test_e4c_f5_selo_catraca.py:237–250,338–349` · `app/chamados/tests/test_e4c_f5_selo_chamada.py:188–193` · `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:572–594` · `app/chamados/tests/test_e1_entradas.py:185–227`

O delta acrescenta zero juízes, escritores persistentes, leituras de relógio, derivações paralelas e blocos `except`. O recibo lê o ato da porta existente e os carimbos do cartão usam o redutor existente. Os quatro blobs conferem com os lados do anexo. Identidade dos commits, suíte Django, concorrência real e efeitos em produção [nao verificado]. `app/chamados/services/disputa_emissao.py:1489–1534,1564–1571,1586–1628` · `app/chamados/views.py:394–405` · `app/templates/chamados/partials/_pergunta_card.html:20–43` · `app/api/views.py:2653–2665`

## 28/11 09:21 · sessao 0b3354320924 · FERIADO-BOTAO-CORTADO · verificador · origem=Code

**APROVADO na volta faixa c2.** O achado do laudo anterior está fechado na cópia. `badge.chave` seleciona a posição da faixa. `badge.label` fornece o texto visível e o `title`. [calendario.py:714](/home/ronald/codex_ws/4174901d58ba92b38a53f22bddf93762ec216f94/app/colaboradores/services/calendario.py:714) · [_calendario_grade.html:100](/home/ronald/codex_ws/4174901d58ba92b38a53f22bddf93762ec216f94/app/templates/colaboradores/partials/_calendario_grade.html:100) · [_calendario_grade.html:150](/home/ronald/codex_ws/4174901d58ba92b38a53f22bddf93762ec216f94/app/templates/colaboradores/partials/_calendario_grade.html:150)

1. **Leitores do rótulo e do dict.** Na busca da cópia, as comparações restantes com `label` são asserções de texto de rubricas, como `Atraso` e `extra`. O selo estrutural de UI consulta o `title`. Nenhum consumidor encontrado exige chaves exatas ou igualdade do dict. O JS do drawer insere o HTML da rota. O PDF de Furos usa data e status, e o PDF do espelho lê `dia['feriado']`. [test_calendario_le_dia_pago.py:212](/home/ronald/codex_ws/4174901d58ba92b38a53f22bddf93762ec216f94/app/colaboradores/tests/test_calendario_le_dia_pago.py:212) · [test_ui_grade_calendario.py:112](/home/ronald/codex_ws/4174901d58ba92b38a53f22bddf93762ec216f94/app/colaboradores/tests/test_ui_grade_calendario.py:112) · [_drawer_generico.html:142](/home/ronald/codex_ws/4174901d58ba92b38a53f22bddf93762ec216f94/app/templates/core/_drawer_generico.html:142) · [relatorios/views.py:1616](/home/ronald/codex_ws/4174901d58ba92b38a53f22bddf93762ec216f94/app/relatorios/views.py:1616) · [pdf_espelho.py:867](/home/ronald/codex_ws/4174901d58ba92b38a53f22bddf93762ec216f94/app/relatorios/pdf_espelho.py:867)
2. **Quem escreve e quem lê.** `contexto_calendario` é o único produtor encontrado. Grava `chave` nos cinco tipos e publica a lista em `dia.badges`. Os dois ramos da grade são os únicos leitores. Ficha e aba Espelho das duas cascas usam a mesma rota. Fita de diagnóstico e Furos também chamam o produtor e exibem a faixa uma vez no bloco normal. O PWA recebe HTML nesse caminho, e a API do app monta seu próprio JSON de dias sem `badges`. [calendario.py:712](/home/ronald/codex_ws/4174901d58ba92b38a53f22bddf93762ec216f94/app/colaboradores/services/calendario.py:712) · [calendario.py:796](/home/ronald/codex_ws/4174901d58ba92b38a53f22bddf93762ec216f94/app/colaboradores/services/calendario.py:796) · [detalhe.html:215](/home/ronald/codex_ws/4174901d58ba92b38a53f22bddf93762ec216f94/app/templates/colaboradores/detalhe.html:215) · [_drawer_generico.html:142](/home/ronald/codex_ws/4174901d58ba92b38a53f22bddf93762ec216f94/app/templates/core/_drawer_generico.html:142) · [colaboradores/views.py:2000](/home/ronald/codex_ws/4174901d58ba92b38a53f22bddf93762ec216f94/app/colaboradores/views.py:2000) · [drawer.py:123](/home/ronald/codex_ws/4174901d58ba92b38a53f22bddf93762ec216f94/app/colaboradores/services/drawer.py:123) · [_drawer_diagnostico.html:35](/home/ronald/codex_ws/4174901d58ba92b38a53f22bddf93762ec216f94/app/templates/colaboradores/partials/_drawer_diagnostico.html:35) · [furos.py:164](/home/ronald/codex_ws/4174901d58ba92b38a53f22bddf93762ec216f94/app/relatorios/furos.py:164) · [furos.html:42](/home/ronald/codex_ws/4174901d58ba92b38a53f22bddf93762ec216f94/app/templates/relatorios/furos.html:42) · [api/views.py:1541](/home/ronald/codex_ws/4174901d58ba92b38a53f22bddf93762ec216f94/app/api/views.py:1541) · [api/views.py:1604](/home/ronald/codex_ws/4174901d58ba92b38a53f22bddf93762ec216f94/app/api/views.py:1604)
3. **Cada badge sai uma vez.** Sai fora da região rolável se os quatro termos forem verdadeiros: chave `feriado`, layout `mes`, permissão e oferta. O bloco interno usa a negação exata dessa conjunção. A checagem em memória dos predicados deu exatamente um ramo em 72 combinações. O produtor acrescenta no máximo um badge de feriado por dia. [_calendario_grade.html:96](/home/ronald/codex_ws/4174901d58ba92b38a53f22bddf93762ec216f94/app/templates/colaboradores/partials/_calendario_grade.html:96) · [_calendario_grade.html:100](/home/ronald/codex_ws/4174901d58ba92b38a53f22bddf93762ec216f94/app/templates/colaboradores/partials/_calendario_grade.html:100) · [_calendario_grade.html:150](/home/ronald/codex_ws/4174901d58ba92b38a53f22bddf93762ec216f94/app/templates/colaboradores/partials/_calendario_grade.html:150) · [calendario.py:712](/home/ronald/codex_ws/4174901d58ba92b38a53f22bddf93762ec216f94/app/colaboradores/services/calendario.py:712)
4. **Classes de reprovação.** O delta introduz zero segundo juiz, regra própria de feriado no template, `except`, leitura de relógio ou derivação paralela. A grade e o POST consomem a oferta publicada pelo mesmo serviço. As sete verificações do selo estrutural de UI passaram isoladamente em memória, com `unittest`. [calendario.py:347](/home/ronald/codex_ws/4174901d58ba92b38a53f22bddf93762ec216f94/app/colaboradores/services/calendario.py:347) · [calendario.py:803](/home/ronald/codex_ws/4174901d58ba92b38a53f22bddf93762ec216f94/app/colaboradores/services/calendario.py:803) · [_calendario_grade.html:96](/home/ronald/codex_ws/4174901d58ba92b38a53f22bddf93762ec216f94/app/templates/colaboradores/partials/_calendario_grade.html:96) · [ponto/views.py:3624](/home/ronald/codex_ws/4174901d58ba92b38a53f22bddf93762ec216f94/app/ponto/views.py:3624) · [test_ui_grade_calendario.py:45](/home/ronald/codex_ws/4174901d58ba92b38a53f22bddf93762ec216f94/app/colaboradores/tests/test_ui_grade_calendario.py:45)

Os dois arquivos conferem com os hashes de blob do patch anexo. Histórico Git da cópia, suíte Django completa, smoke no navegador e produção **[nao verificado]**. [nao verificado]

## 28/11 09:18 · sessao 0713f15f0aca · FERIADO-BOTAO-CORTADO · construtor · origem=Code

### Cura aplicada [nao verificado]

- `contexto_calendario` passou a emitir `chave` em cada badge. As chaves são `feriado` na linha 714, `extra_50` na 718, `extra_100` na 723, `atraso` na 728 e `antecipada` na 733. [calendario.py:714](/home/ronald/wt-codex-feriado-botao-cortado-090709/app/colaboradores/services/calendario.py:714) [nao verificado]
- A grade seleciona por `badge.chave` nos dois ramos. A faixa externa exige a chave `feriado`, layout mensal, permissão e oferta. O ramo interno usa a condição complementar. `badge.label` preenche o texto e o `title`. [_calendario_grade.html:100](/home/ronald/wt-codex-feriado-botao-cortado-090709/app/templates/colaboradores/partials/_calendario_grade.html:100) · [_calendario_grade.html:150](/home/ronald/wt-codex-feriado-botao-cortado-090709/app/templates/colaboradores/partials/_calendario_grade.html:150)

### Censo de leitores

- Os dois laços da grade são os únicos leitores diretos em código de produção encontrados na cópia. As views e serviços repassam o contexto à grade. O POST consulta a oferta. A busca em JS e PDF encontrou o seletor do popover e os contadores próprios do PDF, respectivamente. Nenhum outro leitor de produção usa o rótulo como chave. [_calendario_grade.html:99](/home/ronald/wt-codex-feriado-botao-cortado-090709/app/templates/colaboradores/partials/_calendario_grade.html:99) · [_calendario_grade.html:149](/home/ronald/wt-codex-feriado-botao-cortado-090709/app/templates/colaboradores/partials/_calendario_grade.html:149) · [colaboradores/views.py:2020](/home/ronald/wt-codex-feriado-botao-cortado-090709/app/colaboradores/views.py:2020) · [drawer.py:123](/home/ronald/wt-codex-feriado-botao-cortado-090709/app/colaboradores/services/drawer.py:123) · [furos.py:164](/home/ronald/wt-codex-feriado-botao-cortado-090709/app/relatorios/furos.py:164) · [ponto/views.py:3626](/home/ronald/wt-codex-feriado-botao-cortado-090709/app/ponto/views.py:3626) · [popover-acao.js:111](/home/ronald/wt-codex-feriado-botao-cortado-090709/app/static/js/popover-acao.js:111) · [pdf_espelho.py:766](/home/ronald/wt-codex-feriado-botao-cortado-090709/app/relatorios/pdf_espelho.py:766)
- Os testes existentes usam o texto para conferir extras, atraso e saída antecipada, ou inspecionam o `title`. `test_calendario_le_dia_pago.py` faz essas leituras nas linhas 212, 219 e 234–235. [test_calendario_badge_minutos.py:79](/home/ronald/wt-codex-feriado-botao-cortado-090709/app/colaboradores/tests/test_calendario_badge_minutos.py:79) · [test_calendario_carimbo_intra.py:70](/home/ronald/wt-codex-feriado-botao-cortado-090709/app/colaboradores/tests/test_calendario_carimbo_intra.py:70) · [test_calendario_le_dia_pago.py:212](/home/ronald/wt-codex-feriado-botao-cortado-090709/app/colaboradores/tests/test_calendario_le_dia_pago.py:212) · [test_calendario_um_juiz.py:111](/home/ronald/wt-codex-feriado-botao-cortado-090709/app/colaboradores/tests/test_calendario_um_juiz.py:111) · [test_ui_grade_calendario.py:114](/home/ronald/wt-codex-feriado-botao-cortado-090709/app/colaboradores/tests/test_ui_grade_calendario.py:114) [nao verificado]

### RED para o Opus

1. **Produtor real.** Com um feriado aplicável, verificar que `contexto_calendario` emite exatamente um badge com `chave='feriado'` e `label='Feriado'`. O mutante que remove a chave falha nessa asserção. Se o retorno também for renderizado no mês com permissão e oferta, a faixa some da posição entre o botão e a região rolável e reaparece dentro da região. [calendario.py:714](/home/ronald/wt-codex-feriado-botao-cortado-090709/app/colaboradores/services/calendario.py:714) · [_calendario_grade.html:96](/home/ronald/wt-codex-feriado-botao-cortado-090709/app/templates/colaboradores/partials/_calendario_grade.html:96) · [_calendario_grade.html:150](/home/ronald/wt-codex-feriado-botao-cortado-090709/app/templates/colaboradores/partials/_calendario_grade.html:150)
2. **Posição e contagem no HTML.** Renderizar a célula mensal com permissão e oferta, usando `feriado` com rótulo `Dia festivo` e `extra_50` com rótulo `Feriado`. Exigir uma única faixa `Dia festivo`, entre o botão e a região rolável, e um único badge `Feriado`, dentro da região. Reverter só o ramo externo para `label` faz sumir `Dia festivo` e duplica `Feriado`. Reverter só o interno duplica `Dia festivo` e faz sumir `Feriado`. Reverter ambos troca as posições. Contar os elementos dos badges, pois o ícone também usa `title="Feriado"`. [_calendario_grade.html:97](/home/ronald/wt-codex-feriado-botao-cortado-090709/app/templates/colaboradores/partials/_calendario_grade.html:97) · [_calendario_grade.html:106](/home/ronald/wt-codex-feriado-botao-cortado-090709/app/templates/colaboradores/partials/_calendario_grade.html:106) · [_calendario_grade.html:122](/home/ronald/wt-codex-feriado-botao-cortado-090709/app/templates/colaboradores/partials/_calendario_grade.html:122) · [_calendario_grade.html:150](/home/ronald/wt-codex-feriado-botao-cortado-090709/app/templates/colaboradores/partials/_calendario_grade.html:150)
3. **Condições da faixa externa.** Repetir com `layout='fita'`, sem permissão e sem oferta, variando um termo por vez. Em cada caso, exigir uma única faixa `Dia festivo` no bloco normal. [_calendario_grade.html:96](/home/ronald/wt-codex-feriado-botao-cortado-090709/app/templates/colaboradores/partials/_calendario_grade.html:96) · [_calendario_grade.html:150](/home/ronald/wt-codex-feriado-botao-cortado-090709/app/templates/colaboradores/partials/_calendario_grade.html:150)

`git diff --check` passou. Execução dos REDs, dos mutantes, da suíte e verificação no navegador [nao verificado]. [nao verificado]

## 28/11 09:16 · sessao ed50ffde2cf5 · E4D-CONTRATO-ROTA-B · construtor · origem=Code

Implementei a cura4 dos achados 1 e 3 na cópia `fdb45ed2633cd2d4f4d22520396a6d5e6fd03af3`. O cartão lê a validação vigente da composta, e o POST devolve o instante do ato `RESPOSTA` gravado pelo próprio pedido. `app/templates/chamados/partials/_pergunta_card.html:20–43` · `app/chamados/services/disputa_emissao.py:1564–1571` · `app/api/views.py:2653–2666`

### Mudanças e leitores

- **Cartão do admin.** `resposta.validada_em` governa a opacidade, o selo com hora e a abertura das ações. `perg.respondida` continua delegando ao C21, e a hora da resposta vem da composta. O GET carrega as decisões em lote antes do C6. O HTMX carrega as decisões na busca por PK e renderiza o mesmo parcial. `app/templates/chamados/partials/_pergunta_card.html:20–44` · `app/chamados/models.py:3958–3965` · `app/chamados/services/acoes_chamado.py:345–358` · `app/chamados/views.py:394–405,483–502`
- **Recibo do POST.** A porta captura `ato_em` do último ato devolvido por `registrar`, a `RESPOSTA` deste pedido, dentro da transação que trava o colaborador. A API é a única chamadora que pede `com_recibo=True` e serializa o instante em ISO. A entrada web, as duas declarações de horário e a correção do admin continuam recebendo `(True, None)` no sucesso ou `(False, erro)` na recusa. `app/chamados/services/disputa_emissao.py:1471–1477,1489–1494,1564–1571,1618–1628` · `app/chamados/models.py:4608–4623` · `app/api/views.py:2653–2666` · `app/chamados/views.py:1275–1283` · `app/chamados/services/declaracao_texto.py:445–449,727–731` · `app/chamados/services/acoes_disputa.py:442–447`
- **Outros leitores dos carimbos.** A validação vigente também alimenta `estado`, C6, as guardas de validar e recusar e os dois selos do modal. O instante da resposta vigente também alimenta C21, C6, o fio, o modal e as APIs de consulta. Consumidores externos do recibo do POST ficam [nao verificado]. `app/chamados/models.py:3361–3370` · `app/chamados/juizes.py:1476–1504,1521–1533,1644–1653` · `app/chamados/services/validacao.py:755–765,948–958` · `app/chamados/services/fio.py:442–446` · `app/templates/chamados/partials/modal_fio.html:217–230,346–347` · `app/api/views.py:1133–1150,2567–2582`

### REDs para o Opus

1. **Cartão com validação devolvida.** Crie um chamado local com disputa aberta, pergunta viva respondida, admin autorizado e `RESPOSTA(T1) → VALIDACAO(T2) → DEVOLUCAO(T3)`. Reponha na coluna `validada_em` exatamente `T2`. Confirme na fixture que a composta conserva `respondida_em=T1` e devolve `validada_em=None`. Exercite o GET e um POST ao detalhe com `HX-Request: true`, `acao=recusar_pergunta` e motivo vazio, que chega ao fragmento sem alterar a fixture. No cartão do PK, afirme separadamente a ausência de `opacity:.55`, a ausência do selo com `T2`, a presença de “aguardando validacao”, a hora `T1` e os formulários de ação. Para o mutante, volte ao cru **um ponto por vez**: opacidade, selo e condição das ações. Cada troca tem uma asserção própria. `app/chamados/models.py:4443–4456,4486–4515` · `app/chamados/views.py:394–405,506–519` · `app/chamados/services/validacao.py:934–937` · `app/templates/chamados/partials/_pergunta_card.html:20–43,101–126`
2. **Recibo após outra escrita no mesmo PK.** Faça o POST autenticado de uma resposta válida em `T1`. Envolva a porta chamada pela API e, após o retorno da porta real e antes de entregá-lo à view, use uma instância fresca para gravar uma segunda resposta em `T2` ou, em outro subcaso, uma `REABERTURA` em `T2`. Confirme por leitura fresca que a composta ficou em `T2` ou `None`, respectivamente. Nos dois subcasos, exija HTTP 200, o PK original e `respondida_em == T1.isoformat()`. O mutante que relê a composta após a porta devolve `T2` ou `None`. Para morder também o mutante que lê a coluna, injete `T0 < T1` apenas na instância do primeiro pedido, logo após o `registrar` real, preservando o ato devolvido. O recibo deve continuar em `T1`. `app/api/views.py:2653–2666` · `app/chamados/services/disputa_emissao.py:1489–1494,1564–1571,1618–1628,1673–1719` · `app/chamados/models.py:4493–4504`

A análise sintática dos três arquivos Python e `git diff --check` passaram. O interpretador local não conseguiu importar `django.template`. Renderização, REDs, mutantes e suíte ficam [nao verificado]. [nao verificado]

## 28/11 09:15 · sessao 2cc2ccceddc6 · FERIADO-BOTAO-CORTADO · verificador · origem=Code

**REPROVADO — `badge.label` passou a ter dois papéis.** O produtor entrega `label` como texto de exibição. O template agora também usa esse texto como chave para identificar a faixa de feriado e decidir sua posição. Esse é o achado de campo com dois significados, na seleção da faixa. [calendario.py:714](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/colaboradores/services/calendario.py:714) · [_calendario_grade.html:100](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/templates/colaboradores/partials/_calendario_grade.html:100) · [_calendario_grade.html:150](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/templates/colaboradores/partials/_calendario_grade.html:150) · [CLAUDE.md:426](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/CLAUDE.md:426)

1. **Inclusões.** Na ficha e na aba Espelho das duas cascas, a célula mensal com permissão e oferta gera exatamente uma faixa textual `Feriado`, entre o botão e a região rolável. As três entradas chegam à mesma `calendario_ponto`, que obtém `dias` de `contexto_calendario` e fornece `pode_editar_dia=True` quando há `editar_escalas`. `calendario_partial.html` é a única inclusão que passa `layout='mes'`. [detalhe.html:215](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/templates/colaboradores/detalhe.html:215) · [base.html:399](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/templates/base.html:399) · [base_app.html:154](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/templates/base_app.html:154) · [_drawer_generico.html:142](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/templates/core/_drawer_generico.html:142) · [views.py:2000](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/colaboradores/views.py:2000) · [views.py:2017](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/colaboradores/views.py:2017) · [calendario_partial.html:67](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/templates/colaboradores/calendario_partial.html:67) · [_calendario_grade.html:96](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/templates/colaboradores/partials/_calendario_grade.html:96)

   A fita de diagnóstico usa `layout='fita'`, recebe `pode_editar_dia` do contexto do drawer e usa o mesmo produtor de `dias`. Furos também usa esse produtor e inclui a grade com `only`, passando apenas `dias` e `resumo`. Nesses dois caminhos, a faixa sai uma vez no fluxo normal da célula. [drawer.py:123](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/colaboradores/services/drawer.py:123) · [drawer.py:159](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/colaboradores/services/drawer.py:159) · [_drawer_diagnostico.html:35](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/templates/colaboradores/partials/_drawer_diagnostico.html:35) · [furos.py:164](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/relatorios/furos.py:164) · [furos.html:42](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/templates/relatorios/furos.html:42) · [_calendario_grade.html:150](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/templates/colaboradores/partials/_calendario_grade.html:150)

2. **Produtor e rótulos.** `contexto_calendario` cria `badges` por dia, acrescenta uma única entrada `Feriado` quando a data está no conjunto de feriados do colaborador e publica a lista em `dia.badges`. O mesmo rótulo cobre feriados nacionais, estaduais e municipais aplicáveis, inclusive em dia trabalhado. As outras entradas da lista são extra 50%, extra 100%, atraso e saída antecipada. Portanto, nessa lista há um único rótulo de feriado e nenhum badge separado de ponto facultativo. O produtor também entrega `dia.feriado`, que a grade já lê para desenhar o ícone, e `dia.feriado_vira_trabalho_oferecido`, que a grade lê para o botão. [calendario.py:347](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/colaboradores/services/calendario.py:347) · [feriados.py:57](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/core/feriados.py:57) · [calendario.py:712](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/colaboradores/services/calendario.py:712) · [calendario.py:784](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/colaboradores/services/calendario.py:784) · [_calendario_grade.html:122](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/templates/colaboradores/partials/_calendario_grade.html:122)

3. **Condições.** A condição que **exibe dentro** é a negação exata da que **exibe fora**. Por entrada de `dia.badges`, sai fora se os quatro termos forem verdadeiros: rótulo `Feriado`, layout `mes`, permissão e oferta. Se qualquer termo falhar, a entrada sai no bloco normal. Sem entrada `Feriado`, há zero faixas `Feriado`. O produtor acrescenta essa entrada no máximo uma vez e toda oferta implica que a data está no conjunto de feriados. Assim, no HTML gerado por esse produtor, nenhuma entrada é omitida ou duplicada por esses ramos. [_calendario_grade.html:96](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/templates/colaboradores/partials/_calendario_grade.html:96) · [_calendario_grade.html:150](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/templates/colaboradores/partials/_calendario_grade.html:150) · [calendario.py:712](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/colaboradores/services/calendario.py:712) · [calendario.py:791](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/colaboradores/services/calendario.py:791)

4. **132 px, clique e rolagem.** O código fixa a altura em 132 px, põe botão e faixa em itens `flex:none` e deixa o espaço restante para a região com `min-height:0` e `overflow-y:auto`. Todas as batidas continuam dentro dessa região. Ao abrir, o JS move o formulário para `document.body` e o posiciona como `fixed`. O encaixe visual da faixa e do botão, o clique e o alcance real das quatro batidas a 1366×960 neste diff estão **[nao verificado]**. O smoke da cópia mede o gatilho e a última batida após rolar, mas não mede os retângulos da nova faixa. [_calendario_grade.html:68](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/templates/colaboradores/partials/_calendario_grade.html:68) · [_calendario_grade.html:97](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/templates/colaboradores/partials/_calendario_grade.html:97) · [_calendario_grade.html:106](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/templates/colaboradores/partials/_calendario_grade.html:106) · [_calendario_grade.html:139](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/templates/colaboradores/partials/_calendario_grade.html:139) · [popover-acao.js:127](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/static/js/popover-acao.js:127) · [test_smoke_clique_feriado_vira_trabalho.py:453](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:453)

5. **LEI-AKITA 2.** A classe do dia e a oferta seguem vindo de `contexto_calendario`. A porta POST consulta o mesmo `feriado_vira_trabalho_oferecido`. Há zero segundo juiz ou derivação paralela da oferta no diff. A regra nova é de identidade da faixa: a testemunha toma o texto de exibição como identificador da classe do badge. [CLAUDE.md:20](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/CLAUDE.md:20) · [calendario.py:784](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/colaboradores/services/calendario.py:784) · [ponto/views.py:3624](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/ponto/views.py:3624) · [_calendario_grade.html:100](/home/ronald/codex_ws/2588b5c3afeb6931ebfeebdb407652d8336c7bf0/app/templates/colaboradores/partials/_calendario_grade.html:100)

## 28/11 09:15 · sessao 66d48682229c · FIM-DE-SEMANA-JA · sonda · origem=Code

Usei a saída raspada fornecida. O modo `--sonda` executa o envelope antes de chamar o Codex e inclui a saída raspada na pergunta. [bin/codex_pergunta.sh:634](/home/ronald/codex_ws/3545ebc0799d2b28ff4c2f99af8c9a6f449161bc/bin/codex_pergunta.sh:634) · [bin/codex_pergunta.sh:665](/home/ronald/codex_ws/3545ebc0799d2b28ff4c2f99af8c9a6f449161bc/bin/codex_pergunta.sh:665)

| Empresa | PK | Lavrado em | Linha do col673 |
|---|---:|---|---|
| 2 | 10 | 28/11 06:50:52 | Sim, 1 linha `('entra', '')` [nao verificado] |
| 3 | 11 | 28/11 06:50:52 | Não, 0 linhas [nao verificado] |
| 4 | 12 | 28/11 06:50:52 | Não, 0 linhas [nao verificado] |

A tela lê o único retrato gravado por empresa e competência. O lavrador atualiza esse registro com `update_or_create`, e o modelo garante a unicidade de `(empresa, mes, ano)`. [app/folha/management/commands/lavrar_retrato_folha.py:77](/home/ronald/codex_ws/3545ebc0799d2b28ff4c2f99af8c9a6f449161bc/app/folha/management/commands/lavrar_retrato_folha.py:77) · [app/folha/models.py:366](/home/ronald/codex_ws/3545ebc0799d2b28ff4c2f99af8c9a6f449161bc/app/folha/models.py:366)

A consulta da tela filtra por empresa e competência, ordena pela razão social e lê as linhas gravadas. `lavrado_em` serve para a indicação de atualização e para a checagem de frescor. [app/folha/views_retrato.py:194](/home/ronald/codex_ws/3545ebc0799d2b28ff4c2f99af8c9a6f449161bc/app/folha/views_retrato.py:194) · [app/folha/views_retrato.py:205](/home/ronald/codex_ws/3545ebc0799d2b28ff4c2f99af8c9a6f449161bc/app/folha/views_retrato.py:205) · [app/folha/retrato_folha.py:120](/home/ronald/codex_ws/3545ebc0799d2b28ff4c2f99af8c9a6f449161bc/app/folha/retrato_folha.py:120) · [app/folha/retrato_folha.py:1428](/home/ronald/codex_ws/3545ebc0799d2b28ff4c2f99af8c9a6f449161bc/app/folha/retrato_folha.py:1428)

## 28/11 09:12 · sessao 6a014cfb449e · DESLIGADO-PORTA-DO-CHAMADO · leitura · origem=Code

**DESLIGADO-PORTA-DO-CHAMADO · brief c4.** Os oito achados procedem na árvore entregue. Reproduzi as duas falhas dos selos em memória. Identidade Git do hash `1434b0d5d1ca53aa9066b939bdf5581e92fedbad`, suíte Django, corridas reais, navegador e efeitos em produção [nao verificado]. [C12](app/chamados/tests/e4b_c12_verificar.py:92) · [selo de turno](app/core/tests/test_medida_turno_do_par.py:562)

O `reds_r.md` integral [nao verificado]. A tabela traz REDs propostos para a c4 e distingue o RED 4 citado no laudo do teste que existe na cópia. [Teste de reabertura da cópia](app/chamados/tests/test_desligado_porta_do_chamado.py:229)

| Achado | Conferência na cópia | Cura na origem e responsável | RED — fixture → asserção → mutante |
|---|---|---|---|
| 1. Decisão antes da trava | **Procede.** `criar` lê antes do `atomic`. `transicionar` decide pela instância e omite `admin_sem_fato_novo`. `abrir` já trava antes da busca. O executor aceita veredito anterior à trava e a guarda de cadastro libera destino `desligado`. `app/chamados/models.py:146`, `:305`, `:988`, `:1159`, `:1197`, `:1317`, `:1487` · `app/colaboradores/services/desligamento.py:132` · `app/colaboradores/services/colaborador.py:63` | **OPUS** trava e relê na entrada de `criar` e `transicionar`. Toda travessia terminal→vivo consulta `recusa_da_reabertura` sobre a linha travada, preservando `_gravar_estado`. **CODEX** faz o executor reler o cadastro e chamar o juiz existente sob a mesma trava antes de agendar ou efetivar. `app/chamados/models.py:226`, `:1455`, `:3094` · `app/colaboradores/services/desligamento.py:40`, `:88` | Duas conexões, cadastro muda entre leitura e escrita, chamado ABA com `VIA_ADMIN` sem fato novo, dois pedidos de efetivação → zero nascimento indevido, veto na transição e um único ato com histórico/S89 → mover guarda para antes da trava, omitir fato novo ou reaproveitar veredito velho. `app/chamados/tests/test_desligado_porta_do_chamado.py:392` · `app/colaboradores/tests/test_desligar_guards.py:36` |
| 2. Reabertura administrativa | **Procede.** Pai vivo pula a porta. Pai terminal pode pedir outra disputa fechada pela ressuscitação depois de travar chamados e executar `reabrir_sistema` antes de `reabrir_admin`. A ação admite disputa de desligado em conflito com LU-12 A. Impasse executado [nao verificado]. `app/chamados/services/acoes_disputa.py:300`, `:309` · `app/chamados/models.py:1593`, `:1744`, `:1838` · `app/docs/CORTES.md:546` | **OPUS** concentra o veredito do chamado para ambos os estados e a aplicação em `DisputaSupervisao.reabrir_admin`, com uma escrita de fecho por ato. Trava o universo da ressuscitação antes dos chamados e coordena a disputa-alvo para consumir quota uma vez. **CODEX** faz ação e formulário lerem esse contrato e só dispara pergunta, nota e push após aceite. O ramo desligado depende de `lei:`. `app/chamados/models.py:2502`, `:2539`, `:3098` · `app/chamados/services/fio.py:160` · `app/chamados/services/acoes_disputa.py:312`, `:324` | Pai vivo e terminal, disputa-alvo fechada e outra fechada ligada por pergunta-destino, com controle ativo → recusa preserva fecho, quota e perguntas. Aceite autorizado consome uma quota e abre uma pergunta, com travas C→D→Ch→P → pular pai vivo ou pedir disputa depois do chamado. O laudo descreve aceite no RED 4, cujo fixture e mutante originais [nao verificado]. O teste da cópia exige reabrir só o chamado. `app/chamados/models.py:1744`, `:2529`, `:3098` · `app/chamados/tests/test_desligado_porta_do_chamado.py:229` |
| 3. Reversão do passivo | **Procede.** `update(**restaurados)` contorna os escritores. `SILENCIO` continua efetivo para a tarefa e a nota fica no fio. O JSON anterior ao apply tem só pré-imagem e a reversão exige pós-imagem. `app/chamados/management/commands/reparar_passivo_desligamento.py:195`, `:211`, `:244` · `app/colaboradores/services/desligamento.py:222`, `:236` · `app/chamados/services/tarefa_do_dia.py:182` | **OPUS** faz a inversão pelo núcleo de cada estado e compensa `SILENCIO` em `DecisaoPergunta.registrar`, com a tarefa lendo a sequência compensada. **CODEX** mantém pré-imagem durável, vincula pós-imagem e IDs dos efeitos ao recibo do mesmo commit e delega a inversão sob trava, com guarda de divergência e nota compensatória pela porta do fio. O recorte I5f depende de `lei:`. `app/docs/LEIS.md:65` · `app/chamados/models.py:226`, `:2305`, `:3557`, `:4420`, `:4719` · `app/chamados/services/nota.py:75` · `app/chamados/management/commands/reparar_passivo_desligamento.py:225` | Chamado vivo, disputa aberta, muda e respondida, com interrupção antes do commit e variante com ato posterior → pré-imagem recuperável, estado e tarefa restaurados, resposta preservada, nota compensada, repetição idempotente e divergência recusada → restaurar só colunas ou aceitar reversão sem recibo. `app/chamados/management/commands/reparar_passivo_desligamento.py:198`, `:229` · `app/chamados/services/tarefa_do_dia.py:158`, `:182` |
| 4. Cascata, outbox e ordem | **Procede.** A cascata duplica carimbo, ato e evento de `DisputaSupervisao.fechar`. `registrar` acorda a célula apesar dos receivers guardados. O consumidor trabalha por colaborador. A relavra grava perguntas-destino fora da seleção inicial. Fila e corrida reais [nao verificado]. `app/colaboradores/services/desligamento.py:200`, `:218`, `:238` · `app/chamados/models.py:341`, `:2281`, `:4751` · `app/chamados/signals.py:272` · `app/chamados/veredito_lavrado.py:279` · `app/chamados/reconciliador.py:1498` | **OPUS** põe o rito único na origem de `DisputaSupervisao.fechar`, inclusive para residuais com fecho preservado. **CODEX** faz efetivação e passivo delegarem com um plano travado que inclui perguntas por disputa **e** destino, pais e disputas alcançados, em C→D→Ch→P por PK. Cala a cascata no escritor único do outbox, com o escopo já restaurado em `finally`. O conjunto protegido depende de `lei:` I5f. `app/chamados/models.py:2189`, `:2281`, `:3094` · `app/colaboradores/services/desligamento.py:203`, `:238` · `app/ponto/portas/celula.py:981` | Disputa aberta e fechada com residual, pergunta cujo destino entra por outra disputa e fio I5f → um carimbo/ato por elegível, resposta intacta, travas completas em ordem, zero evento da cascata e fio protegido intacto após drenar fila → religar outbox no `SILENCIO`, tirar destinos do plano ou duplicar o rito. `app/chamados/models.py:2287`, `:3120`, `:4751` · `app/chamados/management/commands/reparar_passivo_desligamento.py:92` |
| 5. Juiz e campos com duplo sentido | **Procede.** Cartório, vínculo e vigia comparam `situacao`. Adesão pergunta ao juiz, mas trata `cascade` como desligamento, assim como renascimento. Regularização e troca de aceite também usam `ENCERRAR`. O histórico toma carimbo de recusa como fato registrado. `app/core/juizes.py:283` · `app/ponto/services/cartorio.py:1803`, `:1868` · `app/ponto/services/vigia_de_hora.py:166` · `app/colaboradores/services/vinculo.py:91`, `:429` · `app/chamados/services/regularizacao_ext.py:206` · `app/chamados/services/adesao.py:68`, `:94` · `app/chamados/models.py:1106` · `app/chamados/services/historico.py:104` | **OPUS** declara via nominal no motor, gravada pelo mesmo núcleo, e centraliza o veto de reabertura no chamado. **CODEX** liga os leitores a `esta_desligado`, faz adesão consumir o veredito e o histórico consumir a testemunha de aplicação. Atribuição de desligamento a `cascade` legado exige evidência [nao verificado]. `app/chamados/catalogo/motor.py:88`, `:95` · `app/chamados/models.py:1868` · `app/colaboradores/services/desligamento.py:28` · `app/chamados/services/materializacao.py:869`, `:893` | Ativo, afastado, agendado e desligado, fecho genérico e nominal, pergunta aplicada e recusada → leitores concordam com o juiz, fecho genérico conserva a decisão pelos demais critérios e histórico distingue recusa de aplicação → comparar `situacao` na borda, equiparar `cascade` à via nominal ou voltar a julgar pelo instante do carimbo. `app/colaboradores/services/desligamento.py:28` · `app/chamados/catalogo/motor.py:95` · `app/chamados/services/historico.py:104` |
| 6. Dia local e mensagem | **Procede em parte.** A view injeta o dia e `28/11 01:30Z` ainda é dia 27 em São Paulo. O cron omite `hoje` no juiz e a view anuncia bloqueio mesmo no agendamento. `app/config/settings/base.py:93` · `app/colaboradores/views.py:1472`, `:1477` · `app/colaboradores/services/desligamento.py:46`, `:84`, `:111` · `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:37`, `:51`, `:57` | **CODEX** torna `hoje` obrigatório no juiz, usa a mesma data de borda na seleção e no executor e faz view e cron consumirem o resultado efetivo da porta. Motivo ausente no agendado recebe recusa explícita na origem. `app/colaboradores/services/desligamento.py:40`, `:73`, `:88` · `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:51` | Relógio em `28/11 01:30Z` e segunda leitura simulada já no dia 28 → uma leitura por entrada, data explícita dia 28 agendada com acesso preservado, toast correto e cron contando só efetivação confirmada → omitir `hoje`, trocar dia local por UTC ou fixar mensagem de bloqueio. `app/colaboradores/views.py:1472`, `:1477` · `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:37`, `:58` |
| 7. Recusa vendida como sucesso e reúso | **Procede.** Ambos os kernels ignoram `None`. Template ignora `anuencia_erro`. `abrir` devolve o encontrado mesmo com renascimento recusado. Lote vazio, emissão, âncora e justificativa com pai vivo têm atalhos. `app/api/views.py:768`, `:1974`, `:2002` · `app/api/views_core.py:762` · `app/ponto/services/triagem_batida.py:349` · `app/templates/ponto/triagem/fora_horario.html:3` · `app/chamados/models.py:992` · `app/chamados/services/regularizacao_ext.py:313` · `app/chamados/views_regularizacao.py:58` · `app/chamados/services/disputa_emissao.py:544`, `:1394` | **OPUS** faz `abrir` propagar a recusa e usa a mesma autorização travada para nascimento e reúso. **CODEX** encaminha emissão, âncora, lote, justificativa e reúso da anuência pela porta antes dos efeitos, faz os kernels consumirem o retorno, renderiza `anuencia_erro` e distingue lote criado de reaproveitado. A recusa fica no atendimento. `app/chamados/models.py:988`, `:1017` · `app/ponto/services/triagem_batida.py:330`, `:349`, `:488` · `app/docs/LEIS.md:78` | Desligado com pai vivo/terminal, lote vazio e disputa aberta, mais controles ativos e batida de chão → recusa visível, `em_disputa=false`, zero pergunta, nota, push ou justificativa indevida, reúso ativo idempotente e batida segue à porta → descartar `None`, devolver terminal recusado ou recolocar cada atalho. `app/chamados/tests/test_desligado_porta_do_chamado.py:140`, `:161` · `app/api/views.py:771` · `app/ponto/services/triagem_batida.py:488` |
| 8. Selos | **Procede, reproduzido.** C12 acusa três chamadas autorizadas deslocadas e S20 lança `IndexError`. Medida de turno acusa quatro sítios deslocados, dois em emissão, um em triagem e um em `ponto/views`. `app/chamados/tests/e4b_c12_ast.py:27`, `:223` · `app/chamados/models.py:1593`, `:1882`, `:1902` · `app/chamados/tests/e4b_c12_sitios.json:158` · `app/chamados/tests/e4b_c12_verificar.py:92` · `app/core/tests/test_medida_turno_do_par.py:562` | **OPUS** atualiza o corpo nominal e ancora S20 na função atual, preservando a função congelada para o RED. Faz sítio ausente falhar por asserção identificada e registra as linhas medidas depois da árvore final, com conjunto de exceções vazio. `app/colaboradores/services/desligamento.py:235` · `app/chamados/tests/e4b_c12_ast.py:31` · `app/chamados/tests/e4b_c12_verificar.py:114`, `:145` · `app/core/tests/test_medida_turno_do_par.py:562` | Árvore final e mutações em memória → censo limpo, S20 removido falha pelo próprio ID, escritor direto (`update(**delta)`) é flagrado e quatro sítios de turno são conferidos → restaurar `encerrar` direto, inserir escrita crua ou introduzir derivação de dia. `app/chamados/tests/test_e4b_c12_selo.py:22` · `app/core/tests/test_medida_turno_do_par.py:509`, `:523` |

lei: **Q1/I5f** — o passivo preserva todo ciclo vivo com reabertura assinada ou só o aberto a partir da efetivação? **A** preserva todos e o fio ligado. **B** inclui no reparo os comprovadamente anteriores e preserva os demais. Em ambas, instante S89 ou assinatura histórica sem comprovação fica `INCOMPLETO`, sem inferir autor pela FK viva. Formulação original das candidatas [nao verificado]. `app/chamados/management/commands/reparar_passivo_desligamento.py:74`, `:92`, `:181` · `app/chamados/services/trilha_reabertura.py:14` · `app/core/models.py:295`

lei: **LU-12** — o motivo próprio autoriza reabrir também a disputa de desligado? **A**, registrada, autoriza só o chamado. **B**, candidata inferida do RED 4 citado, autoriza também a disputa pelo ato administrativo com autor, justificativa e veredito do chamado. Revogação de A [nao verificado]. `app/docs/CORTES.md:546` · `app/chamados/services/acoes_disputa.py:290`, `:309` · `app/chamados/tests/test_desligado_porta_do_chamado.py:229`

**Ordem de construção proposta.** Enquanto a vaga OPUS aguarda, CODEX pode adiantar relógio, origem do outbox e leitores independentes. Com OPUS, primeiro REDs e portas travadas (1, 2, 7), depois via nominal e rito único (5, 4), então reversão (3). Integrar as bordas aos retornos das portas, fechar os ramos LU-12 e Q1 após o corte e atualizar os dois selos por último (8). `app/chamados/models.py:226`, `:2281`, `:3094` · `app/chamados/management/commands/reparar_passivo_desligamento.py:194` · `app/chamados/tests/e4b_c12_ast.py:17` · `app/core/tests/test_medida_turno_do_par.py:562`

ARQUIVOS: **OPUS** — `app/chamados/models.py`, `app/chamados/catalogo/motor.py`, `app/chamados/tests/{test_desligado_porta_do_chamado.py,desligado_porta_do_chamado_mutantes.py,e4b_c12_ast.py,e4b_c12_sitios.json,e4b_c12_verificar.py,test_e4b_c12_selo.py}`, `app/colaboradores/tests/test_desligar_guards.py`, `app/core/tests/test_medida_turno_do_par.py`. Escopo proposto acima.

ARQUIVOS: **CODEX** — `app/colaboradores/services/{desligamento,colaborador,vinculo}.py`, `app/colaboradores/views.py`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py`, `app/chamados/services/{acoes_disputa,fio,adesao,historico,tarefa_do_dia,trilha_reabertura,disputa_emissao,regularizacao_ext}.py`, `app/chamados/management/commands/{reparar_passivo_desligamento,auditar_invariantes_chamados}.py`, `app/chamados/views_regularizacao.py`, `app/ponto/services/{cartorio,vigia_de_hora,triagem_batida}.py`, `app/ponto/portas/celula.py`, `app/api/{views,views_core}.py`, `app/templates/ponto/triagem/fora_horario.html`. Escopo proposto acima.

## 28/11 09:11 · sessao 1792e5234a22 · E4D-CONTRATO-ROTA-B · verificador · origem=Code

**REPROVADO.** A classe de apresentação pelo cru sobrevive no cartão do admin. O delta também deixa vermelha a catraca do fio e introduz no POST uma consulta fora da trava que pode devolver o carimbo de outra resposta. `app/templates/chamados/partials/_pergunta_card.html:21,24,45` · `app/chamados/services/fio.py:213–226` · `app/chamados/tests/test_e4c_f5_selo_catraca.py:237–250,338–349` · `app/api/views.py:2653–2665` · `app/chamados/services/disputa_emissao.py:1487–1494,1562–1568`

1. **Cada leitor usa a autoridade da guarda ou do estado? Sobrou apresentação pelo cru?**

   - **`_pergunta_card`, parcialmente.** A presença da resposta usa `perg.respondida`, que delega ao C21 sobre a composta, e a hora exibida vem da composta. A opacidade, o selo com a hora de validação e a abertura do bloco de ações ainda dependem de `perg.validada_em` cru. Com uma `DEVOLUCAO` posterior e carimbo residual, a composta perde a validação, mas o cartão conserva o selo e esconde as ações. O GET filtra pelo C6. O retorno HTMX também renderiza esse parcial após buscar a pergunta por PK, sem esse filtro. `app/templates/chamados/partials/_pergunta_card.html:20–45` · `app/chamados/models.py:3958–3965,4493–4504` · `app/chamados/services/validacao.py:755–765` · `app/chamados/services/acoes_chamado.py:345–358` · `app/chamados/views.py:394–405,484–502`
   - **`montar_chamados_fio`, `espera_o_texto_do_colaborador` e `_card_atendimento`, sim.** O montador passa a mesma composta para o veredito de envio e para o carimbo. A regra existente do helper equivale ao inverso do C21. A confirmação, a linha junto ao campo de hora e a etiqueta “Enviado” usam o carimbo do contexto. `app/chamados/services/fio.py:213–226,442–446` · `app/chamados/juizes.py:1626–1634,1644–1653` · `app/templates/chamados/partials/_card_atendimento.html:94–106,141–144,393–395`
   - **`modal_fio` e `api_disputa_responder`, sim quanto à fonte.** No modal, os dois selos usam a validação vigente da composta, como `estado`. O autor exibido no histórico vem de `DecisaoPergunta.validada_original_por`, uma foto histórica. No POST, a guarda anterior à escrita usa C21 sobre a composta e o sucesso relê a composta. Nos cinco arquivos, os usos ativos de apresentação de `respondida_em`, `validada_em` ou `validada_por` pela coluna da pergunta ficaram concentrados nos três pontos do cartão do admin. `app/templates/chamados/partials/modal_fio.html:204–230,346–351` · `app/chamados/models.py:3358–3367,4350–4353` · `app/api/views.py:1133–1150,2567–2582,2646–2665` · `app/chamados/services/fio.py:213–226,442–446` · `app/templates/chamados/partials/_card_atendimento.html:100–106,141–144,393–395` · `app/templates/chamados/partials/_pergunta_card.html:21–45`

2. **Quem escreve e lê os quatro valores? Há mudança de unidade, fuso ou sentido?**

   | Valor | Escrita e leitura |
   |---|---|
   | `resposta_enviada` | `montar_chamados_fio` atribui o booleano em memória. `_card_atendimento` o lê para confirmação, seleção e preenchimento, Enviar/Reenviar e a etiqueta de envio. `app/chamados/services/fio.py:442–446` · `app/templates/chamados/partials/_card_atendimento.html:100–114,141–164,393–395` |
   | `resposta_enviada_em` | O mesmo montador copia `composta.respondida_em`, um `datetime` ou `None`. Os três usos na cópia formatam data e hora. `app/chamados/services/fio.py:442–446` · `app/chamados/models.py:4344–4346,4493–4515` · `app/templates/chamados/partials/_card_atendimento.html:106,143,394` |
   | `composta.respondida_em` | A porta humana, a automática e a da trilha alimentam `RESPOSTA`. Nascimento e carimbo alimentam a projeção que o redutor incorpora quando falta a linha correspondente. `RECUSA` e `REABERTURA` retiram a resposta vigente. Leem o valor o C21, a projeção temporal do C6, o fio, os cartões, o modal e as APIs. `app/chamados/services/disputa_emissao.py:1529–1567,1757–1763,1785–1797` · `app/chamados/models.py:3330–3336,3473–3482,4429–4456,4486–4515` · `app/chamados/juizes.py:1476–1503,1644–1653` · `app/chamados/services/fio.py:442–446` · `app/templates/chamados/partials/_pergunta_card.html:30–36` · `app/templates/chamados/partials/modal_fio.html:217–221` · `app/api/views.py:1150,2582,2665` |
   | `composta.validada_em` | `_carimbar_validacao` grava as colunas e fornece `VALIDACAO` a `registrar`, que insere os atos. O redutor incorpora o legado e aplica as compensações. Leem o instante `estado`, C6, as guardas de validar e recusar e o modal. `app/chamados/services/validacao.py:164–178,755–765,948–958` · `app/chamados/models.py:3358–3367,4443–4456,4493–4515,4590–4609` · `app/chamados/juizes.py:1499–1504` · `app/templates/chamados/partials/modal_fio.html:229–230,346–347` |

   Os consumidores conferidos tratam os dois carimbos como instantes. O C6 compara com `agora`, as telas formatam data e hora com `USE_TZ=True` e fuso padrão `America/Sao_Paulo`, e o JSON serializa em ISO. A mudança semântica está no **recibo do POST**. A coluna da instância guardava o instante da resposta deste pedido. A nova leitura pode observar uma resposta posterior ou uma reabertura já gravada depois da saída da transação. Reproduzi em memória, com o redutor da cópia, `T2` e `None` para um pedido que gravou `T1`. A corrida no endpoint integrado fica [nao verificado]. `app/chamados/juizes.py:1476–1494` · `app/config/settings/base.py:92–95` · `app/chamados/services/disputa_emissao.py:1487–1494,1529–1532,1562–1568,1591–1614` · `app/chamados/models.py:4486–4515` · `app/api/views.py:2653–2665`

3. **O delta reincide nas classes de reprovação?**

   - **Catraca vermelha.** Executei a checagem AST existente em memória, usando `unittest.TestCase` no lugar de `SimpleTestCase`. O trecho anterior do anexo passou isoladamente. A cópia acusa `fio.py:226`: a catraca exige que a origem de `leitura` seja composta nos dois ramos, e a assinatura nova permite recebê-la sem essa garantia local. O chamador novo efetivamente passa a composta. `app/chamados/tests/test_e4c_f5_selo_catraca.py:17–22,237–250,338–349` · `app/chamados/services/fio.py:213–226,442–446`
   - **Consultas.** Fio e modal reutilizam o prefetch em lote. O modal passa a fazer uma redução adicional em memória para perguntas sem resposta. O GET do detalhe também tem prefetch. No HTMX, a busca da pergunta vem sem prefetch e a composição acrescenta um `SELECT` quando a pergunta respondida tem validação crua. No POST da API, a composição pós-sucesso acrescenta um `SELECT` de decisões sem prefetch. São contagens por inspeção. Medição com Django [nao verificado]. `app/chamados/services/fio.py:295–311,442–446` · `app/templates/chamados/partials/modal_fio.html:217–229,346–351` · `app/chamados/services/acoes_chamado.py:350–358` · `app/chamados/views.py:394–405` · `app/templates/chamados/partials/_pergunta_card.html:20–36` · `app/api/views.py:2637–2647,2653–2665` · `app/chamados/models.py:4482–4492`
   - **Demais classes.** Zero novo juiz, escritor persistente, leitura de relógio, derivação paralela, `except` ou fallback direto ao cru. O legado continua no redutor comum. O carimbo cru residual do cartão e a mudança de sentido do recibo são os desvios identificados. `app/chamados/services/fio.py:213–226,442–446` · `app/chamados/models.py:4429–4448,4486–4515` · `app/templates/chamados/partials/_pergunta_card.html:21–45` · `app/api/views.py:2653–2665`

4. **Os REDs descritos mordem cada mutante?** Pelo desenho, mordem os mutantes nomeados abaixo quando a fixture alcança o ramo e a asserção isola o trecho. Execução dos REDs e mutantes em Django [nao verificado]. `app/chamados/tests/e4d_matriz_mutantes.py:598–612,687–694`

   | Leitor | Mordida |
   |---|---|
   | Cartão do admin | A hora no bloco “Resposta do colab” distingue ato de coluna nula. O RED descrito deixa sobreviver os três usos de `validada_em` cru. `app/templates/chamados/partials/_pergunta_card.html:21–45` |
   | Fio e Meu Atendimento | A hora afere o mutante do montador. Para morder também a troca crua em cada ponto do HTML, é preciso afirmar cada trecho separadamente. A etiqueta exige um furo com o formulário de declaração visível. O teste existente verifica presença da caixa e do botão, sem afirmar a hora. `app/chamados/services/fio.py:442–446` · `app/templates/chamados/partials/_card_atendimento.html:100–106,141–144,212–228,393–395` · `app/chamados/tests/test_e3_item1b.py:150–171` |
   | Modal | `VALIDACAO` vigente com coluna nula e `DEVOLUCAO` com resíduo distinguem os dois sentidos da troca, com e sem resposta vigente. O teste F7 existente usa validação legada que o cru também mostra. O caso 30 afirma a hora da resposta, sem testar o selo. `app/templates/chamados/partials/modal_fio.html:217–230,346–351` · `app/chamados/models.py:4443–4447,4493–4504` · `app/chamados/tests/test_f7_validada_conservada.py:44–53` · `app/chamados/tests/test_e4d_matriz_reds.py:1659–1660` |
   | POST | Injetar `T0` só na instância após a porta gravar `T1` distingue os retornos. Confirmei essa divergência no redutor em memória. A asserção existente de `AGORA` aceita o mutante porque a porta grava o mesmo instante na coluna e no ato. O RED descrito cobre a fonte do carimbo e deixa a corrida sem cobertura. `app/api/views.py:2653–2665` · `app/chamados/services/disputa_emissao.py:1529–1532,1562–1567` · `app/chamados/tests/test_e4c_f5_voltaB_sitios.py:83–97` |

**Achados 1 e 2:** sem agravamento pelo delta. O pré-filtro do fio, `q_vivas`, a seleção e ordenação da fila SQL e a serialização da mensageria conservam os mesmos caminhos. `app/chamados/services/fio.py:306–317` · `app/chamados/models.py:3770–3789` · `app/chamados/juizes.py:1855–1868` · `app/api/views_mensageria.py:539–547,555–575`

Os cinco blobs da cópia conferem com os lados novos de `cura3.diff`. Identidade do commit e do pai, suíte Django e efeitos em produção [nao verificado]. [nao verificado]

_8889 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
