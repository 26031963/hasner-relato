# CODEX-ESTADO

> Ultima resposta processada: **26/11/2026 08:42** · publicado em **26/11/2026 08:43** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 26/11 08:42 · sessao a20deb4d80b3 · FERIADO-SEM-LIMITE-DO-SUPRIMIDO · construtor · origem=Code

Implementei **K2 e K3**. **K1 segue bloqueado na zona inviolável do motor.** [fechamento.py:973](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/ponto/services/fechamento.py:973) · [porta_export.py:340](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/folha/porta_export.py:340) · [motor_calculo_v2.py:3092](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/ponto/motor_calculo_v2.py:3092)

- **K1 — equivalência das vias.** O RED com o motor real obteve **440 min na dobra regular e 470 min na promoção**. O ponto para autorização do Ronald é `motor_calculo_v2.py:3092–3095`. A conta do suprimido percorre apenas `resultado.periodos` e grava no segundo par em `:3127`. Para reutilizar essa mesma conta, o motor precisa aplicá-la aos pares de folga antes de entregar o resultado. O RED roda por comando explícito. O mutante fica pendente com a cura. [Teste:391](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:391) · [Motor:3092](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/ponto/motor_calculo_v2.py:3092) · [Motor:3127](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/ponto/motor_calculo_v2.py:3127) [nao verificado]
- **K2 — igualdade exata.** O RED reproduziu **1,07 h × 1,06 h**. FM e DiaPago agora usam a mesma distribuição diária e a mesma soma em minutos antes do arredondamento mensal. A comparação é exata no centésimo para as três rubricas HE100, nas vias regular e promovida. Em memória, ambas deram **1,07 h**, e o comparador acusou as três rubricas ao alterar a lavra para **1,06 h**. Os dois mutantes reproduziram a divergência de soma e a omissão do comparador, respectivamente. [Teste:351](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:351) · [DiaPago:173](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/ponto/services/dia_pago.py:173) · [Comparador:510](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/folha/porta_export.py:510) [nao verificado]
- **K3 — vigia no limite.** O RED acusava **331,3 min de HE para teto de 330,3 min**. O feriado em dobra voltou à soma da base e passa no limite de **+1 min**. O controle com **331,4 min** acusa. O mutante que reaplica a acumulação incremental ao feriado ressuscitou o falso positivo, e o mutante do dia comum suprimiu a acusação esperada nesse ramo. [Vigia:340](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/folha/porta_export.py:340) · [Testes:421](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:421) [nao verificado]

**Comandos para o Code**, com `DJANGO_SETTINGS_MODULE` do ensaio já configurado. [Runner:80](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/ponto/tests/mutantes_feriado_sem_limite_do_suprimido.py:80)

```bash
obra() { PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.mutantes_feriado_sem_limite_do_suprimido "$1"; }

obra red-k1
obra red-k2
obra k2-soma-em-horas
obra k2-tolerancia-he100
obra red-k3
obra k3-acumulacao-no-feriado
obra soma-reordenada
obra mensal-le-par
obra lavra-le-par
```

**Catraca**, linha registrada → linha atual, conferida pelo detector. [Registro:287](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/core/tests/test_medida_turno_do_par.py:287)

| Sítio | Linha |
|---|---:|
| [porta_export.py `medir`](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/folha/porta_export.py:594) | 586 → 594 |
| [dia_pago.py `_dia_de`](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/ponto/services/dia_pago.py:134) | 132 → 134 |
| [fechamento.py `_so_a_data`](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/ponto/services/fechamento.py:1588) | 1592 → 1588 |

Ficaram na fila o agrupamento civil e a cobertura de `periodos_ft` pelo vigia, o oráculo, a regra do TXT para 12x36 e G0.6. [Vigia:591](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/folha/porta_export.py:591) · [Oráculo:679](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/ponto/calculador/regras.py:679) · [TXT:420](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/folha/export.py:420) · [G0.6:323](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-003136/app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:323)

Conferi os casos em memória, com ORM simulado no K2. A suíte Django, a certificação e o efeito sobre competências já lavradas ficam **[nao verificado]**. [nao verificado]

## 26/11 01:19 · sessao 859e73b64e12 · O30-F3-CONSTRUIR · construtor · origem=Code

**O30-F3 c2 entregue na cópia sobre `52821ca5b`.** Os 21 testes sem banco passaram. Os quatro REDs reproduziram as falhas na c1 com `rc=1`, e os 23 mutantes executados localmente falharam como esperado. O diff adiciona zero ocorrências de `except`. A suíte do Code, a certificação do opus, a integração com PostgreSQL e o comportamento em produção ficam [nao verificado]. `ponto/tests/test_atestado_f3_contrato.py:87`, `ponto/tests/mutantes_atestado_f3.py:27`, `ponto/services/ausencia.py:1401`, `ponto/services/atestados_lote.py:160`

| Bloqueio | Correção e decisão |
|---|---|
| **B1** | Criar/reabrir, editar e anexar travam raiz → filha. A edição relê a linha antes de derivar minutos e estado. Arquivo e metadados ficam na referência que a entrega abre. A troca de um arquivo aprovado exige atuar na própria raiz. O reenvio do app preserva o comprovante já recebido. `ponto/services/ausencia.py:283`, `:452`, `:502`, `:684`, `:842`, `:1086`, `:1387`, `ponto/views_atestado.py:42`, `ponto/tests/test_atestado_f3.py:106` |
| **B2** | `rederivar_fim` grava só os campos derivados e o carimbo de atualização. A falha da trilha dos seis campos aborta a transação. O selo detecta o `save()` integral real, e a prova do histórico verifica a chamada à função compartilhada. `ponto/services/ausencia.py:300`, `:1860`, `core/observ.py:94`, `ponto/tests/test_atestado_f3_contrato.py:134`, `:158`, `:171`, `ponto/tests/test_atestados_lote.py:13` |
| **B3** | “Sem documento” consulta `estado_inicial`. O teto conta os tipos que o catálogo marca como médicos. Arquivo sumido ou foto recusada pelo Pillow rende “Anexo indisponível”. HEIC e PDF recebem o link protegido. Na sonda com seleção simulada, um atestado e 21 férias deram HTTP 400 na c1 e no mutante, e HTTP 200 na c2, nos três formatos. `core/juizes.py:263`, `ponto/services/atestados_lote.py:28`, `:154`, `:171`, `ponto/views_atestado.py:119`, `ponto/tests/test_atestado_f3.py:288` |
| **B4** | O admin oculta os seis campos de todos os perfis. A mídia normaliza o caminho e exige `ver_ausencias` e vínculo com uma ausência no schema da requisição. `ponto/admin.py:39`, `core/media_protegida.py:8`, `ponto/tests/test_atestado_f3_contrato.py:411` |

**Comandos de reprodução**, a partir de `app/`. Os seletores e testes-alvo estão em `ponto/tests/mutantes_atestado_f3.py:27`.

```bash
f3() { PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 "$1"; }

PYTHONDONTWRITEBYTECODE=1 python3 -m unittest ponto.tests.test_atestado_f3_contrato

f3 c1_b1
f3 filha_substitui_aprovado
f3 reabrir_separa_arquivo
f3 trava_invertida
f3 editar_sem_releitura
f3 anexar_ignora_filha
f3 reabrir_le_filha
f3 gate_le_filha
f3 reenvio_regrava_upload
f3 reenvio_tenta_trocar_aprovado

f3 c1_b2
f3 save_integral
f3 trilha_facultativa
f3 evento_engole_obrigatorio
f3 historico_sem_delegacao

f3 c1_b3
f3 lote_flag_crua
f3 lote_ferias
f3 pdf_sem_storage
f3 pdf_imagem_recusada

f3 c1_b4
f3 admin_expoe_seis
f3 midia_staff
f3 midia_sem_normalizacao
f3 midia_sem_vinculo
```

**PDF com 20 atestados** [nao verificado]. O comando de medição parou ao importar `django.core`. O tempo do PDF real e o `f3 lote_ferias` com o ORM ficam para a régua do Code. `manage.py:11`, `ponto/tests/test_atestado_f3.py:288`, `:330`

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  ponto.tests.test_atestado_f3.AtestadoF3MedidaTest --settings=config.settings.ci
```

**Ficou fora.** Relógio na borda, semântica de vigência, competência geral, fuso de `decidido_em`, JSON de reversão E4b e efeito da trilha na reversão H06. `ponto/services/atestados_lote.py:34`, `:48`, `:149`, `ponto/views_atestado.py:103`, `chamados/management/commands/backfill_fio_morto_e4b.py:355`, `ponto/management/commands/rederivar_minutos_h06_comp11.py:152`

**Máscara para a sessão.** Acrescentar em `VAZIO` e `NULO`, respectivamente. As duas regras gravam `''` e `None`. `bin/sombra_regras_pessoais.py:49`, `:70`, `bin/sombra_mascara_pessoal.py:17`, `ponto/models.py:787`

```python
    ('ponto', 'Ausencia'): ('atestado_medico', 'atestado_crm', 'atestado_crm_uf', 'atestado_cid'),
    ('ponto', 'Ausencia'): ('atestado_emissao', 'atestado_dias'),
```

## 26/11 01:13 · sessao 0fff049b4d8a · DIA-DA-BATIDA-JUIZ-UNICO · construtor · origem=Code

A adesão pós-vínculo agora usa o dia que `dia_das_batidas` atribui a cada PK. A consulta inclui um dia em cada ponta e aplica os dois limites inclusivos do chamado ao resultado do juiz. Sem dois limites, verifica se há batida apurável na vida do colaborador. O fechamento continua por `transicionar_com_pauta` e `pai.reconciliar`. [colaboradores/services/pos_vinculo.py:37] [ponto/turnos.py:2972]

Dei baixa nas duas chaves ORM da linha 41 da base. A catraca agora compara arquivo, função, expressão AST e ocorrência, com classe, motivo e dono por sítio. A varredura estática passou com **252 sítios registrados, 102 classificados como J**. A célula `turno/marcos × um juiz por pergunta` está vermelha e a dívida J integra o selo que bloqueia o verde. Conferi **zero novos `except`** nos quatro trechos alterados, incluindo testes. [ponto/tests/test_contract_no_batida_date.py:145] [ponto/tests/test_contract_no_batida_date.py:507] [ponto/tests/test_contract_no_batida_date.py:546] [core/contratos_estruturais.py:225] [core/tests/test_selo_contratos_estruturais.py:81] [nao verificado]

### RED e GREEN

Executei o comando RED antes de alterar o serviço e o comando GREEN depois. Ambos pararam ao importar `django.core`, antes da conexão. **RED e GREEN em banco de testes, suíte do Code e confirmação independente do hash** [nao verificado]. [manage.py:11] [nao verificado]

Em memória, o pareador e o mapa reais atribuíram as duas PKs a **25/10** nos pares **19:00–07:00** e **23:58–00:03**, com 43.200 e 300 segundos brutos. Com persistência e porta simuladas, a seleção civil fechou o pai da folga nos dois casos e a cura devolveu `0` e o manteve vivo. O teste ORM também exige o controle de 25/10 com gate apto, uma só nota após a repetição, exclusão da retratada e o ramo sem dois limites. [ponto/turnos.py:1192] [ponto/turnos.py:3010] [ponto/tests/test_x1_folga_sem_adesao.py:439] [nao verificado]

Comandos para o envelope do Code, a partir de `app/`. [config/settings/ci.py:6] [ponto/tests/test_x1_folga_sem_adesao.py:473]

```bash
A=ponto.tests.test_x1_folga_sem_adesao.FecharAdesaoDiaDoTurnoTest
C=ponto.tests.test_contract_no_batida_date.ContractNoBatidaDateTest
t() { PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test --settings=config.settings.ci "$@"; }

# RED na base com o teste novo aplicado [nao verificado]
t "$A.test_1900_a_0700_nao_fecha_adesao_da_folga" \
  "$A.test_2358_a_0003_nao_fecha_adesao_da_folga"

# GREEN com a cura
t ponto.tests.test_x1_folga_sem_adesao "$C" \
  core.tests.test_selo_contratos_estruturais colaboradores.tests.test_pos_vinculo
```

### Mutantes

Os oito casos de mutação da catraca passaram em memória com `unittest.TestCase`. Forçar a célula a verde fez falhar a asserção de dívida. [ponto/tests/test_contract_no_batida_date.py:554] [ponto/tests/test_contract_no_batida_date.py:651] [nao verificado]

```bash
t "$C.test_mutante_novo_em_funcao_registrada"
t "$C.test_mutante_registro_obsoleto_na_mesma_funcao"
t "$C.test_mutante_localdate_terminal_com_apelido_de_modulo"
t "$C.test_mutante_localdate_importado_e_renomeado_em_escopo_local"
t "$C.test_mutante_localtime_e_instante_em_variavel_derivada"
t "$C.test_mutante_data_local_com_apelido"
t "$C.test_mutante_lookup_relacional_em_dict_e_keyword"
t "$C.test_mutante_retorno_das_duas_chaves_da_adesao"
```

Os cinco mutantes do serviço foram exercitados com persistência simulada. Os comandos abaixo aplicam cada mutação em memória e devem falhar na asserção indicada quando rodados pelo Code. O resultado ORM dos mutantes está [nao verificado]. [colaboradores/services/pos_vinculo.py:39] [ponto/tests/test_x1_folga_sem_adesao.py:469]

```bash
mutar() {
  MUTANTE_ADESAO="$1" DJANGO_SETTINGS_MODULE=config.settings.ci \
    PYTHONDONTWRITEBYTECODE=1 python3 -B - "$2" <<'PY'
import os
import sys
from pathlib import Path

import django
from django.core.management import call_command

django.setup()
mutante = os.environ['MUTANTE_ADESAO']

if mutante == 'verde': [nao verificado]
    from core.contratos_estruturais import celula
    celula('turno/marcos', 'um juiz por pergunta')['verde'] = True [nao verificado]
else:
    from colaboradores.services import pos_vinculo as pos
    filtro = 'any(inicio <= dia <= fim for dia in dias.values())'
    antes, depois = {
        'civil': (filtro, 'batidas_apuraveis(colaborador).filter('
                          'timestamp__date__gte=per[0], timestamp__date__lte=per[1]).exists()'),
        'sem_vespera': ('inicio - um, fim + um', 'inicio, fim + um'),
        'sem_recorte': (filtro, 'bool(dias)'),
        'limites_exclusivos': ('inicio <= dia <= fim', 'inicio < dia < fim'),
        'sem_vida': ('tem_batida = batidas_apuraveis(colaborador).exists()', 'tem_batida = False'),
    }[mutante]
    fonte = Path(pos.__file__).read_text(encoding='utf-8')
    assert fonte.count(antes) == 1
    exec(compile(fonte.replace(antes, depois), pos.__file__, 'exec'), vars(pos))

call_command('test', sys.argv[1])
PY
}
mutar civil "$A.test_1900_a_0700_nao_fecha_adesao_da_folga"
mutar sem_vespera "$A.test_2358_a_0003_nao_fecha_adesao_da_folga"
mutar sem_recorte "$A.test_1900_a_0700_nao_fecha_adesao_da_folga"
mutar limites_exclusivos "$A.test_dia_do_trabalho_fecha_uma_vez"
mutar sem_vida "$A.test_sem_dois_limites_a_presenca_e_na_vida"
mutar verde "$C.test_celula_vermelha_enquanto_houver_divida" [nao verificado]
```

### Sonda de leitura da competência 10/2026

O recorte usa o corte de cada empresa. A sonda conta PKs e dias-colaborador distintos quando o dia civil ou o dia atribuído cai na competência. Para cada pai vivo com janela sobreposta ou com uma marca nesse recorte, compara a decisão sobre o **período integral gravado** e usa `reconciliaria()` para estimar transições. O snapshot manual evita o ramo de trava que o gate usa dentro de `atomic()`. **Contagens e transições da competência 10** [nao verificado]. [ponto/janelas.py:54] [colaboradores/services/pos_vinculo.py:37] [chamados/models.py:678] [chamados/models.py:761] [docs/BACKLOG.md:1858]

```bash
# Envelope do Code em app/, com SONDA_SCHEMA, SONDA_DB_USUARIO e SONDA_DB_SENHA.
PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'
import json
import os
from collections import Counter
from datetime import date, timedelta

import django
from django.conf import settings
from config.settings import base

shared = ['django_tenants', 'tenants', *(
    app for app in base.INSTALLED_APPS if app.startswith(('django.contrib.', 'rest_framework'))
)]
tenant = [
    app for app in base.INSTALLED_APPS
    if app not in shared or app in {'django.contrib.auth', 'django.contrib.contenttypes'}
] + ['api']
cfg = {k: getattr(base, k) for k in dir(base) if k.isupper()}
cfg.update(
    DEBUG=False, LOGGING_CONFIG=None,
    SHARED_APPS=shared, TENANT_APPS=tenant,
    INSTALLED_APPS=list(dict.fromkeys(shared + tenant)),
    DATABASE_ROUTERS=['django_tenants.routers.TenantSyncRouter'],
    TENANT_MODEL='tenants.Cliente', TENANT_DOMAIN_MODEL='tenants.Domain',
    EMAIL_BACKEND='django.core.mail.backends.dummy.EmailBackend',
    VAPID_PUBLIC_KEY='', VAPID_PRIVATE_KEY='', GLPI_APP_TOKEN='',
    DATABASES={'default': {
        'ENGINE': 'django_tenants.postgresql_backend',
        'HOST': 'sombra_db', 'PORT': '5432', 'NAME': 'sombra',
        'USER': os.environ['SONDA_DB_USUARIO'],
        'PASSWORD': os.environ['SONDA_DB_SENHA'],
        'OPTIONS': {'options': '-c default_transaction_read_only=on'},
    }},
)
settings.configure(**cfg)
django.setup()

from django.db import connection
from django.utils import timezone
from django_tenants.utils import schema_context
from chamados.catalogo.motor import VIVOS
from chamados.models import ChamadoColaborador
from ponto.janelas import janela_fechamento
from ponto.turnos import batidas_apuraveis, dia_das_batidas
from tenants.models import Cliente

UM = timedelta(days=1)
grupos = ('antes', 'depois', 'saem', 'entram', 'muda_dia', 'sem_dia')
pks = {g: set() for g in grupos}
civis = {g: set() for g in grupos}
jornadas = {g: set() for g in grupos}
transicoes = Counter(dict.fromkeys(('ambos', 'so_antes', 'so_depois', 'nenhum'), 0))
pais = Counter(dict.fromkeys(('no_recorte', 'sem_dois_limites', 'gate_recusou'), 0))

schema = os.environ['SONDA_SCHEMA']
with connection.cursor() as cur:
    cur.execute(
        'SET SESSION CHARACTERISTICS AS TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY'
    )
connection.set_autocommit(False)
try:
    with connection.cursor() as cur:
        cur.execute("SELECT current_database(), current_setting('transaction_read_only'), "
                    "current_setting('transaction_isolation')")
        if cur.fetchone() != ('sombra', 'on', 'repeatable read') or connection.in_atomic_block:
            raise RuntimeError('snapshot de leitura da sombra indisponível')
    if schema == 'public' or not Cliente.objects.filter(schema_name=schema).exists():
        raise ValueError('schema de ensaio inválido')

    with schema_context(schema), timezone.override(settings.TIME_ZONE):
        estoque = ChamadoColaborador.objects.filter(
            modulo_origem='sem_previsao_escala', subcategoria='nunca_bateu',
            status_local__in=VIVOS,
        ).select_related('colaborador__empresa')
        for pai in estoque:
            periodo = (pai.contexto_json or {}).get('periodo') or []
            if len(periodo) != 2:
                pais['sem_dois_limites'] += 1
                continue
            ini, fim = map(date.fromisoformat, periodo)
            colab = pai.colaborador
            a, z = janela_fechamento(10, 2026, colab.empresa)
            vivas = batidas_apuraveis(colab)
            antes = set(vivas.filter(timestamp__date__gte=ini, timestamp__date__lte=fim)
                        .values_list('pk', flat=True))
            mapa = dia_das_batidas(colab, ini - UM, fim + UM)
            depois = {pk for pk, dia in mapa.items() if ini <= dia <= fim}

            no_recorte = max(ini, a) <= min(fim, z)
            for pk, ts in vivas.filter(pk__in=antes | depois).values_list('pk', 'timestamp'):
                civil, dia = timezone.localdate(ts), mapa.get(pk)
                if not (a <= civil <= z or (dia is not None and a <= dia <= z)):
                    continue
                no_recorte = True
                em_antes, em_depois = pk in antes, pk in depois
                for grupo, conta in (
                    ('antes', em_antes), ('depois', em_depois),
                    ('saem', em_antes and not em_depois),
                    ('entram', em_depois and not em_antes),
                    ('muda_dia', dia is not None and civil != dia),
                    ('sem_dia', dia is None),
                ):
                    if conta:
                        pks[grupo].add(pk)
                        civis[grupo].add((colab.pk, civil))
                        if dia is not None:
                            jornadas[grupo].add((colab.pk, dia))
            if no_recorte:
                pais['no_recorte'] += 1
                candidato = bool(antes or depois)
                apto = candidato and pai.reconciliaria()
                pais['gate_recusou'] += int(candidato and not apto)
                assinatura = (bool(antes) and apto, bool(depois) and apto)
                transicoes[{(True, True): 'ambos', (True, False): 'so_antes',
                            (False, True): 'so_depois', (False, False): 'nenhum'}[assinatura]] += 1
finally:
    connection.rollback()
    connection.close()

print(json.dumps({
    'competencia': '10/2026',
    'pais': pais,
    'transicoes_por_pai': transicoes,
    'transicoes_antes': transicoes['ambos'] + transicoes['so_antes'],
    'transicoes_depois': transicoes['ambos'] + transicoes['so_depois'],
    'marcas': {
        g: {'batidas': len(pks[g]), 'dias_colab_civil': len(civis[g]),
            'dias_colab_jornada': len(jornadas[g]),
            'dias_colab_uniao': len(civis[g] | jornadas[g])}
        for g in grupos
    },
}, ensure_ascii=False, sort_keys=True))
PY
```

## 26/11 01:03 · sessao 4737c85695ab · FERIADO-SEM-LIMITE-DO-SUPRIMIDO · verificador · origem=Code

**Parecer 5a — eu não daria o aval à c4b.** A promoção passa a usar o mesmo cálculo da dobra para os pares que recebe. O motor ainda entrega suprimido diferente nas duas vias do turno partido. Também reproduzi em memória duas divergências de fronteira: a igualdade mensal com a soma dos dias falha por 0,01 h e o ajuste do vigia do dia comum muda uma acusação em feriado. `app/ponto/motor_calculo_v2.py:3020–3024,3092–3129`; `app/ponto/services/fechamento.py:975–984` (c4b); `app/ponto/services/dia_pago.py:282–297` (c4b); `app/folha/porta_export.py:340–358` (c4b).

Usei a cópia e o diff fornecido, em somente leitura. Executei as contas puras em memória com Python 3.12. A execução Django, a comparação com os objetos Git `67dd8957e` e `160173bb1` e os efeitos em produção ficam **[não verificado]**. As referências marcadas **c4b** usam a numeração do anexo.

## 1. Quem escreve e quem lê

| Campo ou valor | Escritor e leitores |
|---|---|
| **Trabalhado, suprimido e limite comum**, em minutos | 12x36 e comercial lavram trabalhado e suprimido por par. O turno partido escreve o suprimido entre pares no fechamento do motor. `_limite_com_suprimido` lê o suprimido e devolve ao limite comum só até o intervalo declarado. A dobra lê o suprimido inteiro, descontado até o trabalhado do dia. `app/ponto/motor_calculo_v2.py:1840–1858,2884–2910,3092–3129,3359–3378,746–763`. |
| **HE100, HE100 feriado e HE100 noturna do motor**, em minutos | `_dobra_sem_o_suprimido` escreve `minutos_extra`, `minutos_extra_100`, `minutos_extra_100_feriado`, `hora_extra_100` e `minutos_extra_100_noturna` nos pares. `_aplicar_he_noturna` escreve o split intermediário e o split da HE comum. `ResultadoMes` e `lavrar` leem as rubricas dos pares regulares. `app/ponto/motor_calculo_v2.py:298–344,1063–1078,1885–1908`; `app/ponto/services/dia_pago.py:199–212`. |
| **Split da promoção e flag de prorrogação** | `get_motor_cct` monta a régua e o motor guarda `prorrogacao_noturna_pos5h`. `apurar_por_fatia` captura a flag do motor que julgou cada par de folga. A porta lê a flag por `id(p)`, as pontas registradas, o trabalhado, o suprimido e os intervalos carimbados pelo motor. Devolve `(HE100, HE100 noturna)` em minutos. Mensal e lavra recebem o mesmo mapa, sem nova consulta à CCT. `app/core/regua_cct.py:449–479`; `app/ponto/motor_calculo_v2.py:797–801,1939–1947`; `app/ponto/services/fechamento.py:302–334,975–984,1222–1227,1299–1304` (c4b); `app/ponto/services/dia_pago.py:135–168` (c4b). |
| **FechamentoMensal e DiaPago** | O fechamento soma a promoção em `horas_extras`, `horas_extras_100`, `horas_extras_100_feriado` e `horas_extras_100_noturna`, em horas com duas casas. `lavrar` grava as três HE100 como horas `FloatField`. `detalhe.folga_feriado_min` guarda o **trabalhado bruto da promoção** em minutos e `pago_do_dia` o converte em horas para tela, PDF, app e conferência da porta. A partição tira os promovidos de `horas_folga_trabalhada` e de `ft_sem_escala`. `app/ponto/services/fechamento.py:961–1001` (c4b), `1154–1186`; `app/ponto/services/dia_pago.py:282–298` (c4b), `374–389,921–927`; `app/ponto/models.py:325–337,412–425`; `app/folha/porta_export.py:106–124`. |
| **`porta_export`** | `fechamento_x_soma_dias_pagos` lê o mensal e agrega a lavra por campo, inclusive os três HE100. `minuto_em_duas_rubricas` lê HE, intra, trabalhado e a marca de dobra nos pares regulares. No ramo comum consulta previsto e intervalo declarado. `medir` acrescenta colab e dia e publica a lista. Os limiares são `0,015 h` para FM × DiaPago e `+1 min` para dupla rubrica. `app/ponto/services/dia_pago.py:34–44,579–592`; `app/folha/porta_export.py:245,431–438,496–512,579–592,715–724`; `app/folha/porta_export.py:340–358` (c4b). |

**Segundo escritor fora da porta — ausente nas rubricas auditadas.** `restaurar_fechamento` aceita valores de restauração pela porta do mensal. O retrato de exportada atualiza os três campos de grade. O oráculo tem derivação própria e grava outra `versao` pela mesma `lavrar`. `app/ponto/services/fechamento.py:1375–1414,1260–1313`; `app/ponto/services/dia_pago.py:341–389,434–467,544–574`; `app/ponto/models.py:319–323`.

## 2. Classes de reprovação e selos

- **Segundo juiz registrado — ausente no diff.** A promoção usa `_dia_de`, que prioriza o mapa do motor, e o predicado existente `feriado_vira_folga`. Já existem decisões paralelas no vigia por data civil e na supressão de feriado do TXT para 12x36. Esta última está registrada em `PENDENTES`. `app/docs/JUIZES.md:114–119`; `app/core/juizes.py:310–315,495–498`; `app/ponto/services/dia_pago.py:109–132`; `app/ponto/services/fechamento.py:963–969` (c4b); `app/folha/porta_export.py:579–590`; `app/folha/export.py:420–423`.
- **Relógio fora da borda — ausente no trecho novo.** A flag vem do motor da fatia. `_dia_de` usa o instante da entrada como degradação quando falta o mapa. As leituras de `now()` do fechamento já existiam. O diff acrescenta zero `except`. `app/ponto/services/fechamento.py:302–334` (c4b), `623–625,1154–1206`; `app/ponto/services/dia_pago.py:126–132`.
- **Derivação paralela — presente em parte.** A função de janela e a distribuição do suprimido têm um corpo cada no motor. A promoção replica o `min(noturna_cheia, HE100_do_par)` e monta a chamada de split com HE50 zero. O motor faz o mesmo teto em sua escrita e a casca dele recebe também o campo HE50 do par. O oráculo continua com outra derivação, usando o trabalhado da folga na HE100. `app/ponto/motor_calculo_v2.py:409–451,727–763,1070–1078,1885–1901`; `app/ponto/services/dia_pago.py:149–168` (c4b); `app/ponto/calculador/regras.py:679–712`.
- **Dois significados — unidades distintas, fronteira de dia divergente no vigia.** `folga_feriado_min` é trabalhado bruto, HE100 é líquida do suprimido e HE100 noturna é um subconjunto em minutos de relógio. `horas_noturnas` usa minutos legais. Com corte 21, entram os dias de jornada de 21 a 20, **ambos inclusivos**. À meia-noite a janela noturna reparte duração em trechos contíguos sem duplicar o instante, e o feriado da dobra pertence ao dia do início do turno. O vigia agrupa pela data da entrada e pode separar a cauda ou descartá-la na borda da competência. `app/ponto/models.py:319–337`; `app/ponto/services/dia_pago.py:249–251,283–297` (c4b); `app/ponto/janelas.py:10–33`; `app/ponto/motor_calculo_v2.py:89–118,953–989,1039–1061,2505–2537`; `app/folha/porta_export.py:579–592`.

**Mordida dos mutantes.** A tabela indica o efeito na asserção-alvo. Conferi a aritmética isolada. A execução dos nove processos Django fica **[não verificado]**. `app/ponto/tests/mutantes_feriado_sem_limite_do_suprimido.py:22–84` (c4b).

| Mutante | O que a asserção pega |
|---|---|
| `mensal-le-par`, `lavra-le-par` | Opus cai de 420 para 360 min de HE100 noturna no mensal ou na lavra, respectivamente. `app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:239–255` (c4b). |
| `sem-noturno`, `dia-civil` | FN2 dá `120/0` em vez de `120/120` de HE100/noturna. O segundo mutante perde o diurno disponível do par com entrada no dia civil seguinte. `app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:289–303` (c4b). |
| `sem-limite`, `relogio` | FD6 dá `100/120` em vez de `100/100`. G0.6 dá `270/270` em vez de `270/240`. `app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:300–318` (c4b). |
| `ultima-fatia`, `sem-intervalos` | A primeira fatia perde 120 min noturnos pela flag errada. A segunda ganha 60 min ao ignorar a pausa. O mensal esperado de 11 h noturnas vira 9 h ou 12 h, respectivamente. `app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:319–345` (c4b). |
| `soma-reordenada` | Reproduzido. Com HE `170,3`, a soma incremental acusa e o `sum` compensado devolve `None`. O mesmo hunk muda também o ramo **feriado**: trabalhados `(120; 120,2; 120,1)`, HE `(120; 120,1; 91,2)` e intra `30` no último par. A base devolve `None`, a c4b acusa `331,3` contra teto publicado `330,3`. `app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:348–361` (c4b); `app/folha/porta_export.py:340–355`, `340–358` (c4b). |

O V1 verifica que o espelho foi consultado, que a chamada mutada recebeu `noturno=` e que a acusação contém `540/480`. O controle exige HE100 feriado de 8 h e lista vazia. A assinatura `_antiga(periodos, noturno=None)` **já está na cópia**. O diff acrescenta `autospec` e a prova da chamada. O V1 cobre a dobra regular. Uma promoção errada que sobe o mesmo valor a FM e DiaPago pode passar pelos dois vigias, pois o comparador vê igualdade e a dupla rubrica só visita `periodos`. `app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:247–281,417–439` (último intervalo c4b); `app/folha/porta_export.py:496–512,579–592`.

**Lacunas que deixam código errado passar.** A bateria nova aceitaria agrupar todos os promovidos da competência num único lote: os casos com suprimido usam um dia e o teste de duas fatias usa `S=0` nos dois dias. A maioria dos REDs substitui `calcular_mes` e injeta `S` pronto, o que deixa passar a diferença real do turno partido abaixo. Contra a lógica da base, Opus, FN2, FD6 e três pares discriminam a cura, enquanto G0.6 e duas fatias com `S=0` são controles. O runner `base` devolve só o resultado global da suíte, sem conferir a causa de cada falha. `app/ponto/tests/test_feriado_sem_limite_do_suprimido.py:239–345` (c4b); `app/ponto/tests/mutantes_feriado_sem_limite_do_suprimido.py:63–84` (c4b).

## 3. O noturno do dia muda?

**O adicional noturno do dia fica igual com os mesmos pares julgados.** O motor continua preenchendo `minutos_noturnos_relogio` e `minutos_noturnos_legais` pelas pontas efetivas. FM e DiaPago continuam lendo o legal em `horas_noturnas`. O diff move a **HE100 noturna** da promoção. Quando acabam os diurnos, a HE100 noturna pode cair em relação à dobra cheia. Num par `22:00–05:00`, com 420 min trabalhados e `S=60`, ficam 360 min de HE100 noturna, 420 min de relógio e 480 min legais com o fator padrão. `app/ponto/motor_calculo_v2.py:44–46,319–321,740–763,1039–1061`; `app/ponto/services/fechamento.py:927`; `app/ponto/services/dia_pago.py:249–251,149–168` (porta em c4b).

O G0 anexo fixa **HE100 noturna / relógio**, em minutos, em `300/300`, `180/180`, `360/360`, `120/120 + 240/240`, `270/270` e `240/270`. No sexto caso, a saída registrada `05:30` faz o split recuar 270 min até `01:00`. As pontas efetivas do relógio são `00:30–05:00`. No F11c, o cálculo conserva 9,00 h de HE100, 1,00 h de HE100 noturna e 1,14 h de adicional noturno. Execução dos testes completos **[não verificado]**. `app/ponto/tests/test_feriado_dobra_noturna_g0.py:44–95` (c4b); `app/ponto/motor_calculo_v2.py:392–406,435–451,1039–1061,1068–1076`; `app/ponto/tests/test_o280_bateria_da_lei.py:1801–1823`.

## 4. Censo de leitores e efeito

- **Chamadores diretos das interfaces alteradas.** O fechamento é o único chamador de produção da porta da promoção e entrega o mapa às duas chamadas de `lavrar`. Espelho e PDF também chamam `apurar_por_fatia`, leem o resultado e os carimbos e ignoram a flag nova. O resultado bruto que recebem fica igual. `app/ponto/services/fechamento.py:975–984,1222–1227,1299–1304` (c4b); `app/ponto/services/espelho.py:1199–1202`; `app/relatorios/pdf_espelho.py:417–423`.
- **Espelho, PDF, calendário, app e cartão.** As rubricas diárias exibidas vêm de `DiaPago`, via `rubricas_do_dia` e `pago_do_dia`. A projeção inclui HE100 total e adicional noturno, sem coluna de HE100 noturna. Espelho e PDF ainda apuram o motor para geometria e resumo de períodos livres. O cartão de competência também consulta o fechamento online. `app/ponto/services/dia_pago.py:921–957`; `app/ponto/services/espelho.py:823–831,1200–1216,1315–1320`; `app/relatorios/pdf_espelho.py:844–856,901–914`; `app/colaboradores/services/calendario.py:688–698`; `app/api/views.py:1450–1457`; `app/relatorios/cartao_pela_celula.py:70–101,142–179`.
- **TXT e holerite.** O TXT lê FM e emite o adicional por `horas_noturnas`. A HE100 noturna fica sem evento próprio. Há uma decisão de emissão própria para 12x36, por ciclo e régua do fim da competência, enquanto o motor lê a régua do começo de cada fatia. O holerite entrega o PDF armazenado. Alteração de valor no holerite externo **[não verificado]**. `app/folha/export.py:341–380,420–448,714–739`; `app/ponto/services/fechamento.py:733–750`; `app/holerite/views.py:1445–1459`.
- **Painel e leitores online.** `fechamento_lido` reapura pela mesma porta em modo leitura. O painel mostra total de extras e adicional noturno, e o ranking mostra HE100 e HE100 feriado. Nenhum desses recortes exibe a parcela que muda no Opus. A foto de auditoria lê todos os campos concretos e pode registrar a mudança. O comparador da porta lê explicitamente a parcela alterada e pode continuar com lista vazia quando os dois lados mudam juntos. `app/ponto/services/fechamento.py:1752–1778`; `app/ponto/views.py:1018–1036`; `app/templates/ponto/partials/painel_fechamento.html:33–34`; `app/folha/services/ranking_he.py:137–152,206–229`; `app/chamados/services/foto_e4b.py:324–333`; `app/folha/porta_export.py:496–512`.

## 5. Casos fora do golden

| Caso | Resultado pela cópia e pelo diff |
|---|---|
| **12x36 noturno que cruza a meia-noite** | Feriado no início do plantão `19:00–07:00`, pausa `01:00–02:00`, prorrogação desligada e hora reduzida afastada. São 660 min trabalhados e 360 noturnos. Com dobra habilitada, HE100/noturna `660/360`. Com a régua simples, HE100 **feriado** `0`. O ciclo 12x36 está fora da promoção `feriado_vira_folga`. `app/ponto/motor_calculo_v2.py:2884–2955,3743,3813–3822`; `app/core/regua_cct.py:467–479`; `app/escala/models.py:28–34,1400–1406`. |
| **Feriado no dia 20 ou 21** | Corte 21, plantão `20/11 19:00 → 21/11 07:00`. Com dobra habilitada e feriado só em **20/11**, a dobra inteira pertence a novembro. Com feriado só em **21/11**, esse plantão dá zero HE100 feriado. Um plantão que comece em **21/11** pertence a dezembro. O selo existente verifica a atribuição única do 20→21 para horas trabalhadas. `app/ponto/janelas.py:10–33`; `app/ponto/motor_calculo_v2.py:953–973,2529–2537`; `app/ponto/tests/test_dupla_contagem_dia20.py:30–44`. |
| **Suprimido parcial no turno partido** | Use o desenho já testado `10:00–14:00` e `14:30–18:20`, com mínimo de 60 min e os dois pares reconhecidos como no selo. Em feriado regular em dobra, o laço grava `S=30`, portanto `470−30 = 440` min de HE100. Se o vínculo folga no feriado e os dois pares chegam em `periodos_ft`, o mesmo laço os omite e o cálculo por par do partido não escreve `S`. A promoção recebe `S=0` e paga **470 min**. A divergência está na alimentação da porta. `app/ponto/tests/test_motor_partido_intervalo_pelo_juiz.py:23–39,62–63`; `app/ponto/motor_calculo_v2.py:2550–2599,3004–3024,3092–3129,3219–3225`; `app/ponto/services/fechamento.py:947–984` (c4b). |
| **Intermitente** | O controle existente usa `21/04`, `07:00–19:00`. São 12 h trabalhadas, HE100 e HE100 feriado zero, inclusive com `feriado_em_dobra=True`. O motor injeta conjunto de feriados vazio e o vínculo intermitente passa fora da classificação de folga trabalhada. `app/ponto/tests/test_s6_intermitente_feriado.py:54–56,115–142`; `app/ponto/motor_calculo_v2.py:1019–1023,2466–2470,2550–2558,3689–3701`. |
| **FM × soma DiaPago com segundos** | Dois feriados promovidos com `S=0`, cada um começando às 22:00, com saídas às `22:30:18` e `22:33:36`. Para HE100 e HE100 noturna, arredondar `sum([30,3; 33,6])/60` dá **1,07 h** e somar as horas lavradas e arredondar dá **1,06 h**, reproduzido em memória. A tolerância de `0,015 h` deixa a diferença passar. `app/ponto/services/fechamento.py:975–984` (c4b); `app/ponto/services/dia_pago.py:282–292` (c4b), `579–592`; `app/folha/porta_export.py:245,496–512`. |

## 6. Bloqueia × fila

- **Bloqueia a equivalência das vias reais** — o turno partido com pausa parcial paga 440 min na dobra regular e 470 min na promoção quando o motor envia os pares a `periodos_ft`. `app/ponto/motor_calculo_v2.py:2597–2599,3092–3129,3219–3225`; `app/ponto/services/fechamento.py:975–984` (c4b).
- **Bloqueia C2(g) como igualdade exata** — o exemplo com segundos dá FM `1,07 h` e soma `1,06 h`, com o comparador verde. `app/ponto/services/fechamento.py:975–984` (c4b); `app/ponto/services/dia_pago.py:579–592`; `app/folha/porta_export.py:245,496–512`.
- **Bloqueia o ajuste do vigia no escopo de dia comum** — a ordem nova também muda o veredito de feriado no limite de 1 min. `app/folha/porta_export.py:340–355`, `340–358` (c4b).
- **Fila** — vigia por data da entrada e sem `periodos_ft`, oráculo com outra derivação, TXT com regra de 12x36 no fim da competência e divergência G0.6 de split × relógio. `app/folha/porta_export.py:579–592`; `app/ponto/calculador/regras.py:679–712`; `app/folha/export.py:341–380,420–423`; `app/ponto/motor_calculo_v2.py:392–406,1039–1076`.
- **Catraca** — o anexo desloca `medir` 583→586, `_so_a_data` 1586→1592, `apurar_por_fatia` 213→215 e `recalcular_fechamento_mes` `[664,666,672,692,763,802,848,860]`→`[669,671,677,697,768,807,853,865]`. Execução **[não verificado]**. `app/core/tests/test_medida_turno_do_par.py:287,329–331,563`.

## 26/11 01:00 · sessao ae5d8bfb59d4 · O30-F3-CONSTRUIR · verificador · origem=Code

**O30-F3-CONSTRUIR c1 — bloqueia.** A cura permite que o app substitua o anexo de uma raiz já aprovada por meio de uma filha aberta. Também introduz uma ordem de trava inversa entre reabertura e edição e deixa um `save()` integral capaz de sobrescrever os seis campos. `app/api/views.py:3102–3114,3138–3146`, `app/ponto/services/ausencia.py:1745–1757` (base), `app/ponto/services/ausencia.py:491–496,728–734,1104–1119` (diff).

Conferi a cópia em leitura e usei as linhas novas do diff fornecido. Hash por Git, `mapa.r` separado, execução da suíte completa e ocorrências em produção [nao verificado]. [nao verificado]

## 1. Quem escreve e quem lê

| Campo ou valor | Escritores e leitores |
|---|---|
| `atestado_medico`, `atestado_crm`, `atestado_crm_uf`, `atestado_cid`, `atestado_emissao`, `atestado_dias` | Para **cada um dos seis**, o POST lê o campo presente, o formulário valida, `criar_ausencia` inclui no `INSERT` e o helper `_gravar_dados_atestado` grava pela criação/reabertura, edição e anexo. O helper lê os valores atuais dos seis para detectar mudanças e preservar os omitidos. Drawer, CSV e PDF leem os seis. **Segundo escritor físico presente:** `rederivar_fim` faz `save()` integral com a instância que o comando carregou antes, podendo recolocar os seis valores antigos mesmo sem nomear nenhum campo. A migration inicializa os campos. `app/ponto/views_atestado.py:33–39`, `app/ponto/forms.py:12–21`, `app/ponto/services/ausencia.py:267–311,491–496,548–552,728–734`, `app/templates/ponto/partials/painel_ausencia.html:89–130`, `app/ponto/services/atestados_lote.py:121–155,189–207` (diff), `app/ponto/services/ausencia.py:1745–1753`, `app/ponto/models.py:787–795,892–932`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:29–50`, `app/ponto/migrations/0079_ausencia_atestado_dados.py:12–24` (base). |
| Leituras indiretas dos seis | O Django admin lê todos os campos concretos. A foto E4b lê todos e o comando pode copiá-los ao JSON de reversão. O reparo H06 também lê todos, para conferir quais colunas mudaram. `app/ponto/admin.py:37–48`, `app/core/admin.py:46–75`, `app/chamados/services/foto_e4b.py:247–252,284–288`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:59–67,355–372`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:94–95,166–177` (base). |
| `documento`, `documento_referencia_id` e raiz | `criar_ausencia` insere o arquivo e `anexar_documento` passa a substituir o da raiz. Fora das três portas, `_reabrir` também grava `documento` quando chamado por `reverter_rejeicao(documento=...)`, e o `save()` integral de `rederivar_fim` regrava arquivo e FK. `SET_NULL` limpa a FK ao apagar a raiz. No diff, a atribuição explícita da FK que aparece é de teste. Leem a raiz o drawer, a entrega protegida, a lista, a aprovação/rejeição em grupo e o histórico/lote. A API de reenvio, a fila do app, o gate de aprovação e a troca de plantão ainda leem o arquivo da linha individual. `app/ponto/services/ausencia.py:483–497,754–779,1249–1316,1745–1753`, `app/ponto/models.py:777–781,815–819`, `app/ponto/views.py:2100–2109,2146–2155,2786–2815,2862–2866`, `app/templates/ponto/ausencias.html:168–171`, `app/api/views.py:1053–1067,1852–1858`, `app/ponto/portas/celula.py:719–746` (base), `app/ponto/services/ausencia.py:282–287,1104–1119`, `app/ponto/services/dia_decidido.py:624–630`, `app/ponto/tests/test_atestado_f3.py:77–99` (diff). |
| Trilha | `_trilha_dados_atestado` emite `dados_atestado_editados` com os nomes dos campos e o ramo de upload emite `documento_anexado` com os nomes dos arquivos. Ambos persistem por `core.observ.evento`. A tela de auditoria lê ação e descrição sob `ver_saude_sistema`. O H06 lê qualquer evento posterior da ausência e pode barrar a reversão de minutos após uma edição só de metadados. `evento` é best-effort e os chamadores ignoram o retorno, portanto a gravação pode confirmar sem a trilha. `app/ponto/services/ausencia.py:289–311,623–627,1110–1116` (diff), `app/core/observ.py:94–105,124–139`, `app/core/views.py:686–710,733–738`, `app/templates/core/log_auditoria.html:55–74`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:152–159` (base). |
| `pode_ver_anexo` e `LIMITE_LOTE` | A view produz a flag com `tem_acao(ver_ausencias)`. CSV/PDF recebem a flag com default `False`. A view e o template também a leem para o ZIP. `LIMITE_LOTE = 20` tem uma definição de produção. A view lê até 21 **lançamentos-raiz de qualquer tipo ou status** e recusa os três formatos acima de 20. O teste substitui a constante por 1. Os testes de medida usam 20. Repeti em memória o ensaio ZIP do anexo com o serviço da base, que o diff preserva. Deu **0,374 s no total, 0,019 s/anexo** para 20 arquivos de 1 MB. Tempo do PDF e tempo em produção [nao verificado]. `app/ponto/views_atestado.py:88–95,115–131`, `app/ponto/services/atestados_lote.py:24,53–65,129–155,175–207`, `app/templates/relatorios/atestados_lote.html:20–25`, `app/ponto/tests/test_atestado_f3.py:221–250`, `app/ponto/tests/test_atestado_f3_contrato.py:164–186` (diff), `app/ponto/services/atestados_lote.py:135–173` (base). |

## 2. Classes que já reprovaram obra

| Classe | Veredito |
|---|---|
| Segundo juiz | **Presente para a exigência de documento na saída do lote.** O lote herda a flag crua do catálogo. O juiz registrado é `estado_inicial`, que isenta férias, e o drawer já o consulta. Assim, férias sem arquivo podem aparecer em “Sem documento” no lote e ficar fora da cobrança do drawer. Permissão, janela de competência e assinatura do arquivo continuam delegadas aos respectivos pontos existentes. `app/core/juizes.py:259–264`, `app/ponto/models.py:723–729`, `app/ponto/services/ausencia.py:894–898,926–944,709–729`, `app/ponto/catalogo/ausencias.py:101`, `app/ponto/janelas.py:54–57` (base), `app/ponto/services/atestados_lote.py:32–50,73–75,196–213`, `app/ponto/views_atestado.py:24–26,88–95` (diff). |
| Segundo escritor fora da porta | **Presente.** `rederivar_fim` grava a linha inteira. O selo exclui justamente o módulo que o contém e ainda exige menção textual a algum dos seis campos para examinar os demais arquivos. `app/ponto/services/ausencia.py:1745–1757` (base), `app/ponto/tests/test_atestado_f3_contrato.py:56–75,126–143` (diff). |
| Relógio fora da borda | **Presente, herdado e alcançável.** A view lê `timezone.localdate()`, mas `mes=` ou `ano=` explícito em branco passa pelo `setdefault` e chega vazio a `janela()`. O serviço usa `datetime.date.today()` do host. Em virada de mês com datas locais diferentes, pode selecionar outra competência. Fuso efetivo do host [nao verificado]. `app/ponto/views_atestado.py:49–50,98–112`, `app/ponto/services/atestados_lote.py:39–50` (diff), `app/config/settings/base.py:92–95` (base). |
| Derivação paralela | **Ausente** para `previa()['faltando']` e para o dia decidido. PDF chama `previa`, a tela lê a mesma chave e a montagem continua em `linha_do_tempo`. **Presente no recorte SQL se a pergunta for vigência.** O filtro trata todo `data_fim=NULL` como dia inicial e omite `afastamento_inss` aberto antes de `de`. `ausencias_vigentes` inclui esse caso. A exclusão já existia no lote. `app/ponto/services/atestados_lote.py:53–75,210–216`, `app/ponto/services/dia_decidido.py:624–672`, `app/templates/relatorios/atestados_lote.html:31–40` (diff), `app/ponto/services/atestados_lote.py:54–63`, `app/ponto/turnos.py:3207–3231`, `app/ponto/catalogo/ausencias.py:92–95` (base). |
| Selo/RED sem mordida | **Presente.** Por inspeção, `gravacao`, `cid_csv`, `editar`, `cid_pdf`, `foto`, `sem_documento`, `empresa` e `limite` alteram valores ou ramos com asserção correspondente. `link` só altera o texto, embora o teste também confira o URI. `escritor_externo` injeta um `update` literal por alias, caso que o AST detecta, e deixa escapar o `save()` integral real. O RED `base` cobre só assinatura das portas e CSV. Executei isoladamente o selo legado do histórico em memória: passou com a função do diff e também com o corpo trocado por `return []`, pois o nome exigido está no docstring. Execução dos mutantes anexos [nao verificado]. `app/ponto/tests/mutantes_atestado_f3.py:21–42,75–110`, `app/ponto/tests/test_atestado_f3_contrato.py:78–158`, `app/ponto/tests/test_atestado_f3.py:118–231`, `app/ponto/services/atestados_lote.py:68–75` (diff), `app/ponto/tests/test_atestados_lote.py:116–124` (base). |
| Ordem de trava | **Presente.** Na reabertura da filha rejeitada com dados, criação trava raiz → filha. Na edição da filha com dados, o `save` trava filha → raiz. As duas ordens permitem um ciclo entre transações. Deadlock observado [nao verificado]. `app/ponto/services/ausencia.py:282–287,491–496,728–734` (diff), `app/ponto/services/ausencia.py:392–399,1306–1327` (base). |
| ESMERIL-SEMÂNTICA | **Emissão e dias do atestado têm unidade própria.** Emissão é data civil opcional, inclusive futura, sem fuso e fora do filtro do lote. `atestado_dias` aceita 0 e fica separado de `dias_corridos` e de minutos. O período livre usa datas civis inclusivas nas duas pontas. **Ambiguidades presentes** em fim nulo, competência geral e teto: fim nulo pode ser dia único ou aberto, competência geral usa corte 21 quando empresas podem ter cortes distintos, e o teto conta lançamentos mesmo sem anexo. `decidido_em` é o instante da decisão/criação e o CSV já o formatava sem conversão explícita de fuso. `app/ponto/models.py:685–689,750–763,787–795,834–838`, `app/ponto/janelas.py:10–21,36–57`, `app/ponto/services/dia_decidido.py:558–567`, `app/ponto/services/atestados_lote.py:131` (base), `app/ponto/forms.py:19–21`, `app/ponto/views_atestado.py:56–63,107–119`, `app/ponto/services/atestados_lote.py:53–65,121–125,151–155`, `app/ponto/tests/test_atestado_f3.py:111–116` (diff). |

## 3. Permissões

- **Drawer POST:** exige `editar_ausencias` e `ver_ausencias`, com 403 na falta de qualquer uma. **Drawer GET e leitura do anexo:** exigem `ver_ausencias`, com 404 para o usuário autenticado sem a ação. As buscas por PK e o salto à raiz carecem de recorte por empresa. `app/ponto/views_atestado.py:22–40` (diff), `app/ponto/views.py:2094–2119,2137–2162` (base).
- **Lote:** sem as duas ações de leitura, responde 403. Só `ver_relatorios` permite CSV/PDF sem os seis campos, imagens e links, e recebe 403 no ZIP. Os seletores e a busca partem de todas as empresas e colaboradores do schema. A UI admite inativas e diagnósticas que `empresas_visiveis()` exclui dos seletores. O contrato desse helper ressalva os exports. O ORM SaaS tem isolamento por schema configurado. Execução entre tenants e política de acesso por empresa em produção [nao verificado]. `app/ponto/views_atestado.py:45–48,69–95,107–131`, `app/ponto/services/atestados_lote.py:129–155,189–207` (diff), `app/colaboradores/queries.py:78–87`, `app/config/settings/saas.py:34–41,67–94` (base).
- **CID fora das três portas:** staff autenticado com só `ver_relatorios` pode ler CID **contido no comprovante** pela rota de mídia, se conhecer o caminho do arquivo. A rota autoriza `is_staff` e entrega do storage compartilhado sem conferir ação, empresa ou schema do arquivo. O **campo estruturado** também fica visível no Django admin a staff com a permissão nativa adicional `ponto.view_ausencia`. Acessos reais [nao verificado]. `app/config/urls.py:24,38`, `app/core/media_protegida.py:6–17,38–49`, `Caddyfile:10–17`, `app/config/settings/saas.py:109–111`, `app/ponto/models.py:777–792`, `app/ponto/admin.py:37–48`, `app/core/admin.py:31–34,46–75` (base).
- **Sombra:** o restore copia a tabela, mas os seis campos estão fora do inventário de máscara. Conferi em memória que nenhum dos seis nomes casa com a regex do selo. Um dump com CID preenchido pode chegar à sombra com o valor cru e passar pelo selo. Dados efetivamente copiados [nao verificado]. `bin/sombra.sh:244–275`, `bin/sombra_regras_pessoais.py:48–81,109,160–164`, `bin/sombra_selo_pessoal.py:23–36` (base).

## 4. Dia pago, abono e falta

- **`dia_decidido.py`: nenhum efeito direto identificado.** A mudança seleciona raízes para o histórico e permite pular a leitura do tipo do arquivo. Leitores da mudança são o drawer e o lote. Espelho e calendário continuam lendo `decididos_do_periodo` e seu cartão, fora do hunk. `app/ponto/services/dia_decidido.py:624–672`, `app/ponto/services/atestados_lote.py:68–75` (diff), `app/ponto/views.py:2035–2043`, `app/ponto/services/dia_decidido.py:570–621`, `app/ponto/services/espelho.py:661`, `app/colaboradores/services/calendario.py:563` (base).
- **`ausencia.py`: os seis campos ficam fora do insumo da folha, mas a mudança nas portas pode mover dinheiro.** O `UPDATE` só dos seis não dispara `post_save`. O upload ainda passa pelo `save` e pelo cartório, e a ida entre os dois estados de espera, por si, fica fora da folha. No intervalo legado, a filha pode continuar devendo documento e derrubar a aprovação do grupo, impedindo o abono. Na edição, uma instância carregada como pendente pode ser aprovada por outro ato antes do `save(update_fields=...)`. Se o editor então mudar o tipo para `falta` com minutos omitidos, o modelo decide pela instância pendente, deixa zero, e a gravação parcial preserva o status aprovado do banco. O rateio da folha, o fechamento e `DiaPago` leem esse zero. Ocorrência em produção [nao verificado]. `app/ponto/services/ausencia.py:298–311,725–734,1104–1119` (diff), `app/ponto/services/ausencia.py:115–139,914–917,968–994`, `app/ponto/views.py:2786–2815`, `app/ponto/signals.py:143–147,250–275`, `app/ponto/turnos.py:2528–2537`, `app/ponto/models.py:892–911`, `app/folha/export.py:116–143`, `app/ponto/services/fechamento.py:893–895,1224–1229`, `app/ponto/services/dia_pago.py:274–292` (base).

## 5. Contraexemplos fora do golden

1. **Raiz aprovada com anexo e filha sem anexo próprio.** O DP cancela e reverte só a filha. A reabertura olha o arquivo da filha e a coloca em espera. O app aceita o upload pela filha aberta, mas o serviço do diff troca o arquivo da raiz aprovada. Em outra entrada, `criar_ausencia` reabrindo a filha com CID e arquivo grava o CID na raiz e o arquivo na filha, que a entrega protegida não abre. `app/ponto/views.py:2753–2763,2897–2918,2146–2155`, `app/ponto/services/ausencia.py:1296–1327`, `app/api/views.py:3102–3114,3138–3146` (base), `app/ponto/services/ausencia.py:491–496,1104–1119` (diff).
2. **Um arquivo sumido derruba o PDF inteiro.** Basta um `FileField` preenchido cujo arquivo não esteja no storage. Também basta um JPEG truncado que passe pelos bytes de assinatura e falhe no Pillow. O ZIP já prevê arquivo sumido e o golden usa fotos válidas. `app/ponto/services/atestados_lote.py:159–172,196–204`, `app/ponto/tests/test_atestado_f3.py:169–191` (diff), `app/ponto/services/ausencia.py:709–729`, `app/ponto/services/atestados_lote.py:156–164` (base).
3. **Um atestado e 21 dias de férias do mesmo colaborador já recusam CSV, PDF e ZIP.** O espelho de férias cria uma raiz por dia, e o teto é aplicado antes de distinguir tipo ou presença de arquivo. `app/ponto/services/ausencia.py:1379–1394` (base), `app/ponto/views_atestado.py:115–121`, `app/ponto/services/atestados_lote.py:53–65` (diff).

## 6. Bloqueios e fila

- **Bloqueia — privacidade:** alinhar a leitura dos seis e dos comprovantes nas portas, na mídia e no admin, e cobrir o CID na máscara da sombra. `app/core/media_protegida.py:6–17,42–48`, `app/core/admin.py:46–75`, `bin/sombra_regras_pessoais.py:48–81,160–164` (base).
- **Bloqueia — raiz, trava e dinheiro:** eliminar a substituição via filha aberta, a divisão arquivo/metadados, a inversão de trava e a derivação financeira sobre instância desatualizada. `app/ponto/services/ausencia.py:491–496,725–734,1104–1119` (diff).
- **Bloqueia — escrita e prova:** fechar o `save()` integral fora das três portas, assegurar a trilha exigida e fazer o selo morder a escrita implícita e a perda da delegação do histórico. `app/ponto/services/ausencia.py:1745–1757`, `app/core/observ.py:94–105`, `app/ponto/tests/test_atestados_lote.py:116–124` (base), `app/ponto/tests/test_atestado_f3_contrato.py:126–143` (diff).
- **Bloqueia — lote:** alinhar a exigência de documento e o universo do teto, cobrir arquivo indisponível e comprovar o tempo do PDF. `app/core/juizes.py:263` (base), `app/ponto/views_atestado.py:115–124`, `app/ponto/services/atestados_lote.py:32–36,196–213`, `app/ponto/tests/test_atestado_f3.py:232–250` (diff).
- **Fila — semântica herdada:** fixar a leitura de relógio na borda e explicitar se o período recorta lançamentos ou vigência, qual competência vale no geral e em que fuso sai `decidido_em`. `app/ponto/services/atestados_lote.py:32–43,54–63,131`, `app/ponto/turnos.py:3207–3231`, `app/ponto/janelas.py:36–57` (base), `app/ponto/views_atestado.py:98–112` (diff).
- **Fila — auditoria operacional:** revisar a cópia dos seis no JSON de reversão E4b e o efeito da trilha médica na reversão H06. `app/chamados/management/commands/backfill_fio_morto_e4b.py:355–372`, `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:152–159` (base).

## 26/11 00:58 · sessao 33edb894e08e · - · - · origem=Ronald (tmux)

- **Portas e permissão.** Drawer POST exige `editar_ausencias` e `ver_ausencias` e responde 403. Drawer GET e arquivo protegido exigem `ver_ausencias` e respondem 404. As três resolvem o registro por PK e seguem a referência sem recorte de empresa. No lote, só `ver_relatorios` permite tela/CSV/PDF sem os seis campos, imagens ou links. ZIP exige `ver_ausencias`. `app/ponto/views_atestado.py:22-40,88-95,119-131` (diff), `app/ponto/views.py:2094-2119,2137-2162`, `app/ponto/services/atestados_lote.py:130-154,189-207` (diff).
- **Empresa e tenant.** A tela e os filtros da nova view admitem empresas inativas e de diagnóstico, excluídas do dropdown canônico. A própria fixture usa duas empresas de diagnóstico. O ORM SaaS está configurado para consultar o schema do tenant. Isolamento executado [nao verificado]. `app/ponto/views_atestado.py:45-48,69-83,107-118` (diff), `app/colaboradores/queries.py:78-87`, `app/ponto/tests/test_atestado_f3.py:50-55,207-220` (diff), `app/config/settings/saas.py:34-41,67-94`.
- **CID fora da porta.** `/media/` autoriza `is_staff` e encaminha bytes cujo caminho seja conhecido, sem checar `ver_ausencias`, empresa ou propriedade do arquivo. Pode entregar CID presente no anexo a staff com apenas `ver_relatorios`. O Django admin é outro leitor dos seis campos, por permissão nativa de visualizar `Ausencia`. Atribuições e ocorrências reais [nao verificado]. `app/config/urls.py:24,38`, `app/core/media_protegida.py:6-18,38-49`, `Caddyfile:10-17`, `app/ponto/admin.py:37-48`, `app/core/admin.py:46-75`.
- **Contraexemplo de autorização introduzido pelo diff.** Raiz aprovada com comprovante e filha aberta. É possível obter esse estado cancelando e revertendo só a filha, cuja reabertura lê o documento local vazio. O app aprova a operação pela filha aberta, mas o novo serviço substitui o comprovante da raiz aprovada. `app/ponto/views.py:2740-2765,2897-2918`, `app/ponto/services/ausencia.py:1296-1327`, `app/api/views.py:3083-3114,3135-3146`, `app/ponto/services/ausencia.py:283-288,1105-1118` (diff).
- **Escrita e trava.** `rederivar_fim` é segundo escritor implícito dos seis campos e do documento por `save()` integral, com chamador no comando de cura. O selo exclui todo o módulo. Reabertura com dados trava raiz→filha e edição com dados trava filha→raiz, com possibilidade de deadlock. `_reabrir` ainda grava o arquivo na filha quando os dados foram gravados na raiz. `app/ponto/services/ausencia.py:1249-1282,1306-1327,1745-1753`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:29-31,42-50`, `app/ponto/services/ausencia.py:491-496,728-734` (diff), `app/ponto/tests/test_atestado_f3_contrato.py:126-143` (diff).
- **Dois juízes.** O novo SQL repete vigência e omite `afastamento_inss` aberto que começou antes da janela. `_exige_documento` lê o booleano cru do catálogo, enquanto `estado_inicial` isenta férias. O PDF reutiliza `previa()['faltando']`, herdando a classificação divergente. `app/ponto/services/atestados_lote.py:32-36,53-75,212-213` (diff), `app/ponto/turnos.py:3207-3231`, `app/core/juizes.py:259-264`, `app/ponto/catalogo/ausencias.py:92-93,101`, `app/ponto/services/ausencia.py:898,926-952`.
- **Relógio e limite.** Campo `mes=` ou `ano=` em branco aciona relógio do host dentro de `janela()`, embora a view já tenha lido `localdate()`. O limite conta lançamentos-raiz de todos os tipos, então 21 faltas sem atestado já recusam os três formatos. Tempos reais [nao verificado]. `app/ponto/views_atestado.py:49-50,98-121` (diff), `app/ponto/services/atestados_lote.py:39-65` (diff), `app/config/settings/base.py:92-95`.

## 26/11 00:58 · sessao 60e59a9f1ff5 · - · - · origem=Ronald (tmux)

- **Escritor efetivo fora das três portas.** `rederivar_fim` faz `save()` completo, inclusive dos seis campos, do arquivo e da referência. O comando carrega os objetos antes de gravá-los, permitindo sobrescrita com valores antigos após uma edição concorrente. O selo ignora `ausencia.py` inteiro e exige menção ao nome de um campo, então deixa esse caminho passar. [app/ponto/services/ausencia.py:1745–1753], [app/ponto/models.py:892–932], [app/ponto/management/commands/curar_ausencia_sem_fim.py:29–50], [diff app/ponto/tests/test_atestado_f3_contrato.py:56–75,121–138].
- **Arquivo e raiz.** Criação insere `documento`, e o novo `anexar_documento` atualiza a raiz. `_reabrir` também escreve o arquivo e é alcançável por `reverter_rejeicao(documento=...)`. Ao reabrir uma filha rejeitada com arquivo e CID, a cura grava CID na raiz e `_reabrir` grava o arquivo na filha. Drawer, leitura do arquivo e lote consultam a raiz. A referência pode virar `NULL` por exclusão da raiz via `SET_NULL`. [app/ponto/services/ausencia.py:483–497,1249–1310], [diff app/ponto/services/ausencia.py:492–496,1107–1119], [app/ponto/views.py:2100–2109,2146–2155], [diff app/ponto/services/dia_decidido.py:624–630], [app/ponto/models.py:815–818].
- **Ordem de trava invertida.** `editar_ausencia_registro(filha, **dados)` atualiza e trava a filha antes da raiz. `criar_ausencia` com metadados trava primeiro a raiz da filha rejeitada e depois atualiza a filha em `_reabrir`. As duas ordens formam um ciclo concorrente possível. Ocorrência real [nao verificado]. [diff app/ponto/services/ausencia.py:282–285,492–495,728–734], [app/ponto/services/ausencia.py:988,1307–1309,1320–1322].
- **Leitores laterais dos seis campos.** O Django admin expõe todos os campos a quem tiver a sua permissão de visualização, independente de `ver_ausencias`. A foto da E4b lê todos os campos e pode copiá-los para o JSON de reversão. O reparo H06 lê os seis na conferência genérica de que apenas minutos/carimbo mudaram. Audiência dos arquivos e concessões de produção [nao verificado]. [app/ponto/admin.py:37–48], [app/core/admin.py:31–34,46–75], [app/chamados/services/foto_e4b.py:247–252,284–288], [app/chamados/management/commands/backfill_fio_morto_e4b.py:59–67,355–372], [app/ponto/management/commands/rederivar_minutos_h06_comp11.py:94–95,166–177].
- **Trilha e efeito indireto em minutos.** O evento médico registra nomes de campos sem conteúdo em `core.observ.evento`, com savepoint best-effort. A UI de auditoria mostra ação e descrição sob `ver_saude_sistema`. O reparo H06 lê qualquer evento posterior de `Ausencia`, portanto só editar dados médicos pode impedir a reversão de minutos depois de um apply. [diff app/ponto/services/ausencia.py:289–312], [app/core/observ.py:94–139], [app/core/views.py:686–705], [app/templates/core/log_auditoria.html:55–74], [app/ponto/management/commands/rederivar_minutos_h06_comp11.py:152–159].
- **Leitura do CID no arquivo por outro caminho.** A mídia protegida aceita qualquer `is_staff` autenticado e encaminha o caminho ao Caddy, sem checar `ver_ausencias` ou vincular o arquivo à empresa/schema. Um staff com apenas `ver_relatorios` pode ler CID contido no próprio comprovante se conhecer seu caminho. A rota de mídia não devolve o campo estruturado `atestado_cid`. Acesso observado em produção [nao verificado]. [app/config/urls.py:38], [app/core/media_protegida.py:6–17,42–48], [Caddyfile:7–17], [app/ponto/models.py:777–791].
- **Aprovação legada pode mudar.** A API valida que a filha está aberta e pertence ao colaborador. O novo serviço muda o status da raiz e deixa a filha em `aguardando_documento`. A aprovação em grupo recusa esse estado e reverte a transação, impedindo o abono nesse caso. [app/api/views.py:3102–3114,3138–3146], [diff app/ponto/services/ausencia.py:1107–1119], [app/ponto/services/ausencia.py:914–916,968–974], [app/ponto/views.py:2786–2815].
- **Juiz e limite.** O lote herda a leitura da flag bruta do catálogo onde o juiz A4 exclui férias. Um único colaborador com um atestado e 21 dias de férias no recorte ultrapassa o teto, pois férias são criadas dia a dia e a view conta todas as raízes. [app/core/juizes.py:258–264], [app/ponto/services/atestados_lote.py:15–29,76–84], [app/ponto/services/ausencia.py:894–898,926–944,1379–1394], [diff app/ponto/views_atestado.py:115–120].

## 26/11 00:58 · sessao e461f6a92bc0 · - · - · origem=Ronald (tmux)

**Bloqueios verificados por leitura estática**

- A suíte existente falha: `inspect.getsource(LOTE.linhas)` precisa encontrar `linha_do_tempo` no próprio corpo. O diff delega a `linhas_do_historico`, que chama o montador compartilhado. O teste também roda por herança na classe de formatos. `app/ponto/tests/test_atestados_lote.py:116–127` e diff `app/ponto/services/atestados_lote.py:68–80`.
- A edição passa a gravar campos selecionados antes de reler sob trava. Se uma instância pendente foi carregada, outro ato aprovou, e a edição corrige o tipo para `falta` com minutos omitidos, `Ausencia.save()` usa o status pendente da instância e deixa minutos zero. O status aprovado continua no banco e o fechamento lavra falta zero. Diff `app/ponto/services/ausencia.py:725–734`, base `app/ponto/models.py:892–911`, `app/folha/export.py:87–92,121,138–143`, `app/ponto/services/fechamento.py:893–895,1224–1230`, `app/ponto/services/dia_pago.py:274–277`.
- Em intervalo legado com filha rejeitada, `criar_ausencia` com dados trava raiz → filha na reabertura, enquanto a edição da filha com dados toma filha → raiz. A consulta de rejeitada admite filha. Base `app/ponto/services/ausencia.py:392–399,1306–1327` e diff `app/ponto/services/ausencia.py:282–287,491–496,729–734`.
- O PDF abre arquivo do storage e decodifica foto sem tratamento. Um registro que aponta a arquivo sumido derruba o lote PDF. Um JPEG truncado pode passar pela assinatura e falhar no Pillow. O ZIP já prevê o primeiro caso. Diff `app/ponto/services/atestados_lote.py:166–173,197–204`, base `app/ponto/services/ausencia.py:709–724` e `app/ponto/services/atestados_lote.py:156–164`.

**Dinheiro e leitores**

- `dia_decidido` muda a seleção e a leitura do tipo de anexo no histórico consumido pelo drawer e pelo lote. `cartao_do_dia` e `decididos_do_periodo`, lidos pelo espelho e pelo calendário, ficam fora do hunk. Diff `app/ponto/services/dia_decidido.py:624–668`, base `app/ponto/views.py:2035–2043`, `app/ponto/services/espelho.py:661` e `app/colaboradores/services/calendario.py:563`.
- Os seis campos médicos ficam fora do insumo da folha. Edição só deles usa `update` sem disparar `post_save`. A transição do anexo entre duas esperas deixa ambas fora da folha, mas a escrita do arquivo ainda aciona cartório e consumidor de ausência. Diff `app/ponto/services/ausencia.py:298–309,1100–1115`, base `app/ponto/services/ausencia.py:115–139,154–164`, `app/ponto/turnos.py:2533–2537` e `app/ponto/signals.py:143–147,250–272`.
- Na aprovação de intervalos legados, a transição só da raiz pode deixar a filha devendo documento. Aprovar o intervalo propaga a decisão em transação e recua se a filha estiver em `aguardando_documento`. O golden confere a passagem da raiz, mas deixa essa consequência sem cobertura. Diff `app/ponto/services/ausencia.py:1100–1115` e `app/ponto/tests/test_atestado_f3.py:77–99`, base `app/ponto/views.py:2786–2815` e `app/ponto/services/ausencia.py:914–917,969–974`.

**Outros achados confirmados**

- `rederivar_fim` continua fazendo `save()` completo fora das três portas. Uma instância antiga pode regravar os seis campos. O selo exclui o arquivo inteiro. Base `app/ponto/services/ausencia.py:1745–1757`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:50` e diff `app/ponto/tests/test_atestado_f3_contrato.py:119–135`.
- A trilha usa `evento`, que engole erro e desfaz só o savepoint da trilha. As novas chamadas ignoram o valor retornado. O código já contém um exemplo de guarda contra esse caso na rejeição de férias. Base `app/core/observ.py:94–105,124–139`, `app/ponto/services/ausencia.py:1499–1507` e diff `app/ponto/services/ausencia.py:289–310,1110–1112`.
- O Django Admin expõe os seis campos como somente leitura sob sua própria permissão de view. A configuração de concessões reais e a existência de usuário com acesso ao admin sem `ver_ausencias` ficam [nao verificado]. `app/ponto/admin.py:37–48` e `app/core/admin.py:31–34,46–50,68–75`.
- O lote herda o significado de exigência cadastral, que diverge da cobrança real em férias. Também herda o recorte de lançamentos sem fim pela data inicial. Se o contrato for vigência, o INSS aberto iniciado antes da janela continua coberto pelo juiz e some do lote. Base `app/core/juizes.py:261–263`, `app/ponto/models.py:723–729`, `app/ponto/services/atestados_lote.py:15–29,58` e `app/ponto/turnos.py:2822–2842`.
- Competência sem empresa passa corte padrão 21 e compara todas as empresas com a mesma janela. A mesma competência com uma empresa de corte 26 aceita, por exemplo, uma ausência em 23/09 que a consulta geral de setembro exclui. Diff `app/ponto/views_atestado.py:109–117` e base `app/ponto/janelas.py:10–21,36–57`.

## 26/11 00:55 · sessao 14bcbe78aa34 · DIA-DA-BATIDA-JUIZ-UNICO · leitura · origem=Code

# DIA-DA-BATIDA-JUIZ-UNICO · dossiê, volta 2

A primeira fatia proposta é **fechamento de adesão pós-vínculo**, com a catraca por sítio. O filtro atual pode usar a saída de um plantão da véspera como prova de adesão no dia seguinte. Os arquivos da fatia ficam fora das listas de E1 v4 e PAR c7. A pauta de dias em aberto fica para depois dos pousos porque sua linha está congelada no teste do medidor compartilhado pelas duas raias. [app/colaboradores/services/pos_vinculo.py:37] [app/core/tests/test_medida_turno_do_par.py:324] [arquivos_e1v4.txt:1] [arquivos_parc7.txt:1] [nao verificado]

Usei a cópia identificada no pedido como `c314c099f3f447dbb628557c9ada9906183f1182`. A confirmação independente do hash e de `main` fica **[nao verificado]**. Os REDs abaixo são especificações com resultados esperados por leitura. A execução dos REDs, o DIFF da 10 e os números da nova sonda ficam **[nao verificado]**. O pedido de RED e DIFF por migração consta no BACKLOG. [app/docs/BACKLOG.md:1858]

## 1. Contrato de entrada

| Campo | Contrato |
|---|---|
| Fonte | `_T1` está registrado em `turnos_do_colab`. A função alimenta `parear_turnos`, e `_mk` obtém `Turno.data_turno` de `_data_do_turno`. Uma batida persistida pode ser consultada por `dia_do_turno_da_batida(colab, batida)` e uma janela por `dia_das_batidas(colab, ini, fim)` ou `CargaDaJanela.dia_das_batidas(colab)`. O mapa é por **PK de batida**. Quem já tem o `Turno` lê o `data_turno` que veio com ele. [app/core/juizes.py:215] [app/ponto/turnos.py:248] [app/ponto/turnos.py:312] [app/ponto/turnos.py:1997] [app/ponto/turnos.py:2951] [app/ponto/turnos.py:2992] [app/ponto/turnos.py:3113] |
| Unidade e fuso | `Batida.timestamp` é o **instante**, com fuso. A data civil e a hora crua são suas projeções locais em `America/Sao_Paulo`, com `USE_TZ=True`. `data_turno` é o **dia de início da jornada atribuído pelo juiz**. Uma entrada já na madrugada também pode pertencer à véspera. Prazo e encerramento seguem instante ou fato encerrado pela L-073. [app/config/settings/base.py:93] [app/config/settings/base.py:95] [app/ponto/turnos.py:45] [app/ponto/turnos.py:285] [app/ponto/turnos.py:299] [app/docs/LEIS.md:100] [app/docs/LEIS.md:117] |
| Universo | `batidas_apuraveis` seleciona `retratada_em IS NULL`, inclui pendente de DP e ordena por instante. Os limites opcionais dessa função são **instantes inclusivos**. Na carga em lote, a janela civil com pad é entrada do pareador. O recorte final de competência ou período de jornada usa o dia retornado. [app/ponto/turnos.py:1847] [app/ponto/turnos.py:2921] [app/ponto/turnos.py:2940] [app/ponto/turnos.py:3082] [app/docs/LEIS.md:117] |
| Exclusões e respostas | Retratada fica fora da apuração e continua disponível para exibição crua. Ausência de batida persistida ou de PK no mapa dá `None` no avulso. Uma marca apurável sem turno reclamante recebe o dia civil **pela própria autoridade**. Uma aberta já pode ter `data_turno`. Chave ausente em carga que só alcançou o pad deve ser tratada conforme a janela consultada. [app/ponto/turnos.py:318] [app/ponto/turnos.py:2924] [app/ponto/turnos.py:2954] [app/ponto/turnos.py:2992] [app/ponto/turnos.py:3010] [app/ponto/turnos.py:3024] |
| Escala e célula | A escala relevante é a do **dia candidato da jornada**. `vinculo_do_dia` lê a geradora da `CelulaDia` dentro da vigência. Geradora ausente da carga ou fora da vigência dá `None`. Sem célula ou sem geradora, vale a regra de vínculo que cobre a data. `turnos_do_colab` forma segmentos por dono, alimenta DNA e papel da ata e publica os turnos do segmento pelo `data_turno`. Dia sem dono ainda pode ter turno sem marcos de vínculo. [app/escala/alimentacao.py:228] [app/escala/alimentacao.py:269] [app/ponto/turnos.py:1940] [app/ponto/turnos.py:1956] [app/ponto/turnos.py:1993] |
| Exibição, férias e competência | No espelho e no PDF, a **linha** é do turno reclamante, ou civil para a avulsa. A **hora exibida** é a marca local crua. Férias do TXT e competência perguntam pelo dia da jornada. A competência usa o corte da empresa. Em exportada aplica-se a L-113 com a guarda vigente, TXT anterior preservado, trilha e DIFF antes de ato de frota. [app/docs/LEIS.md:86] [app/docs/LEIS.md:117] [app/ponto/services/espelho.py:457] [app/relatorios/pdf_espelho.py:596] [app/folha/export.py:495] [app/ponto/janelas.py:54] [app/docs/LEIS.md:155] [app/docs/LEIS.md:156] |

## 2. Censo fechado

**escritores:** o dia da jornada nasce em `_data_do_turno` e é publicado por `_mk` em `Turno.data_turno`. `_dia_das_batidas_puro` publica o mapa por PK e responde civil para as marcas apuráveis sem reclamante. `dia_do_turno_da_celula` declara a data de uma célula já identificada. Os sítios **J** abaixo ainda produzem respostas paralelas, inclusive quando só as passam a outro serviço. [app/ponto/turnos.py:248] [app/ponto/turnos.py:312] [app/ponto/turnos.py:3004] [app/ponto/turnos.py:3010] [nao verificado]

**leitores:** nas tabelas, **avulso** significa `dia_do_turno_da_batida` e **lote** significa `dia_das_batidas` ou sua face em `CargaDaJanela`. Dentro de um fluxo que já tem o `Turno` ou o mapa, consumir a resposta carregada. Nos caminhos pré-registro, o instante continua disponível para a geometria. Um sítio que só dispõe de instante ou texto legado fica pendente quando precisa atribuir jornada e ainda carece de identidade ou de âncora autorizada. [app/ponto/turnos.py:2951] [app/ponto/turnos.py:2974] [app/ponto/turnos.py:2992] [app/ponto/turnos.py:3113] [app/docs/BACKLOG.md:1858]

### J · juízo paralelo com caminho vivo

| Sítio | Destino |
|---|---|
| `app/api/views.py:584` e `app/api/views_core.py:570` | **Fica para a obra de tipagem pré-registro.** A data escolhe vínculo e marcos que podem trocar o tipo retroativo antes de existir PK. P4 e o recorte desta obra preservam a tipagem. [app/api/views.py:584] [app/api/views.py:594] [app/api/views_core.py:570] |
| `app/api/views.py:622,625,639` e `app/api/views_core.py:613,616,630` | **Avulso** para a data da batida efetivamente aceita e seu contexto. No ramo que já tem uma aberta, consumir o `data_turno` da aberta para a pergunta sobre ela. Preservar a observação pré-registro do estado do turno e o teto por instante. [app/api/views.py:615] [app/api/views.py:622] [app/api/views.py:625] [app/api/views.py:636] [app/api/views.py:639] [app/api/views_core.py:613] [app/api/views_core.py:616] [app/api/views_core.py:630] [app/docs/LEIS.md:100] |
| `app/api/views.py:719` e `app/api/views_core.py:713` | **Avulso** para o `data_turno` gravado no chamado de auditoria leve. [app/api/views.py:719] [app/api/views_core.py:713] |
| `app/chamados/detector_anomalias.py:140` | **Avulso**, passando a batida já disponível nas APIs. Resolver o dia antes de escolher vínculo. O código atual pode retornar por falta de vínculo civil antes de consultar o turno. [app/chamados/detector_anomalias.py:139] [app/chamados/detector_anomalias.py:146] [app/chamados/detector_anomalias.py:156] [app/api/views.py:704] [app/api/views_core.py:695] |
| `app/chamados/juizes.py:55,205` | **Avulso e lote**, respectivamente, para o degrau por `batida_id`. Manter a precedência da célula e do `data_turno` explícito, com a mesma resposta nas duas faces. [app/chamados/juizes.py:49] [app/chamados/juizes.py:52] [app/chamados/juizes.py:176] [app/chamados/juizes.py:202] |
| `app/chamados/juizes.py:171,276` | **Ficam pendentes de lastro**. `:171` lê carimbo legado que pode ser recepção. `:276` procura a própria prova do plantio quando falta `data_evento`. Consumir **avulso** quando a identidade estiver comprovada. Preservar célula e fato explícitos quando presentes. [app/chamados/juizes.py:143] [app/chamados/juizes.py:171] [app/chamados/juizes.py:255] [app/chamados/juizes.py:276] |
| `app/chamados/management/commands/auditar_disputas.py:111` e `carimbar_por_batida_coincidente.py:81` | **Avulso** para marcos do relatório e para o dia do carimbo, respectivamente. O primeiro é relatório sob demanda. O segundo opera sobre carimbo, com escopo próprio. [app/chamados/management/commands/auditar_disputas.py:111] [app/chamados/management/commands/carimbar_por_batida_coincidente.py:81] [app/config/crons.py:1325] [app/config/crons.py:1469] |
| `app/chamados/services/desvio_escala.py:65` e `limbo_folgas.py:36,158` | **Lote** para o agrupamento do desvio e da sugestão de folga. No desvio, converter o instante para o eixo do dia já atribuído, retirando a regra que recua o dia por horário. Em `semana_muda`, o recorte de existência sustenta a guarda da cobrança e deve filtrar pelos dias do lote. [app/chamados/services/desvio_escala.py:60] [app/chamados/services/desvio_escala.py:68] [app/chamados/services/desvio_escala.py:89] [app/chamados/services/limbo_folgas.py:36] [app/chamados/services/limbo_folgas.py:158] [app/chamados/services/limbo_folgas.py:167] |
| `app/chamados/services/disputa_emissao.py:729,730,1213` | Em `:729–730`, usar o dia do **turno já carregado** para a pertença da cobrança. Preservar o teste físico da travessia onde necessário ao predicado existente. Em `:1213`, usar **avulso** antes de classificar a falta. `:1219` já resolve `data_evento` pela autoridade. [app/chamados/services/disputa_emissao.py:722] [app/chamados/services/disputa_emissao.py:729] [app/chamados/services/disputa_emissao.py:797] [app/chamados/services/disputa_emissao.py:1213] [app/chamados/services/disputa_emissao.py:1219] |
| `app/chamados/services/materializacao.py:448` e `passivo_validado.py:118,597` | **Ficam pendentes de âncora pré-ato** `:448` e o ramo sem `data_evento` de `:118`. A pergunta, quando existe, fornece a célula ou o dia do fato. O instante do plantio ainda não identifica batida persistida. Em `:597`, **lote** para confrontar o dia atribuído das batidas recém-plantadas com a janela julgada, preservando o raio e a transação existentes. [app/chamados/services/materializacao.py:447] [app/chamados/services/materializacao.py:452] [app/chamados/services/passivo_validado.py:103] [app/chamados/services/passivo_validado.py:118] [app/chamados/services/passivo_validado.py:594] |
| `app/colaboradores/services/pos_vinculo.py:41` | **Lote. Primeira fatia.** A existência na janela do chamado passa a ser por dia atribuído. Os caminhos vivos são o pós-vínculo e o receiver de batida. [app/colaboradores/services/pos_vinculo.py:37] [app/colaboradores/services/pos_vinculo.py:89] [app/ponto/signals.py:232] |
| `app/escala/regua_defesa.py:157,524` e `app/escala/services/furos_diarios.py:267,268` | **Lote** para agrupar e escolher a primeira marca de cada jornada na régua e para a presença na janela em `universo_empresa`. Esta última separa `escala_x_real` de `sumiu`. A data descritiva da última marca tem outro uso. [app/escala/regua_defesa.py:153] [app/escala/regua_defesa.py:157] [app/escala/regua_defesa.py:524] [app/escala/services/furos_diarios.py:263] [app/escala/services/furos_diarios.py:281] |
| `app/escala/utils.py:187,1580,1870` | `:187` recebe o dia já atribuído no `Turno` que o reconciliador possui. **Lote** em `:1580` e `:1870` para seleção de jornada intermitente e destino de sobra que pode ocupar slot. A varredura por instante permanece insumo. [app/escala/utils.py:173] [app/escala/utils.py:187] [app/ponto/management/commands/reconciliar_fantasmas.py:101] [app/escala/utils.py:1580] [app/escala/utils.py:1870] [app/escala/utils.py:1874] |
| `app/folha/export.py:500` e `app/folha/porta_export.py:544` | **Lote** para o gate de férias, com carga da borda e interseção final por `data_turno`. **Dia do lote ou do período já alimentado** para a chave do invariante de HE e intrajornada. Preservar as demais condições do TXT. [app/folha/export.py:491] [app/folha/export.py:500] [app/folha/export.py:791] [app/folha/export.py:883] [app/folha/porta_export.py:541] [app/folha/porta_export.py:568] |
| `app/ponto/calculador/nucleo.py:141,237` | **Dia recebido do chamador**. `:141` já aceita o terceiro elemento do par. Completar essa alimentação via **lote** nos chamadores com lastro. O caminho de oráculo que recebe só instantes fica como diagnóstico pendente de alimentação identificada. [app/ponto/calculador/nucleo.py:125] [app/ponto/calculador/nucleo.py:134] [app/ponto/calculador/nucleo.py:237] [app/ponto/calculador/periodo.py:356] |
| `app/ponto/janelas.py:84` e `app/ponto/registro_batida.py:28` | **Avulso** quando a guarda de competência recebe uma batida existente. Para ato pré-registro, receber a âncora do fato já conhecida pela porta. Sem essa âncora, a guarda permanece pendente. O envelope de colisão de `:28` permanece na porta PAR enquanto a escolha puder interferir na aceitação ou tipagem. [app/ponto/janelas.py:75] [app/ponto/registro_batida.py:21] [app/ponto/registro_batida.py:156] [app/ponto/services/flip_batida.py:22] [app/chamados/services/materializacao.py:859] |
| `app/ponto/management/commands/detectar_par_relampago.py:303`, `diff_calculador.py:244`, `e6_piso_autopsia.py:91` | No primeiro, **avulso ou lote** para a chave do candidato após `par_completo`, depois de PAR c7. Nos outros dois, **lote** ou dia recebido com o par. O contrafactual sem identidade fica com o recorte documentado do instrumento até ser alimentado. [app/ponto/management/commands/detectar_par_relampago.py:300] [app/ponto/management/commands/detectar_par_relampago.py:303] [app/ponto/management/commands/diff_calculador.py:244] [app/ponto/management/commands/e6_piso_autopsia.py:91] [app/docs/PROMPTS.md:2999] |
| `app/ponto/management/commands/emitir_furo_retroativo.py:75,100`, `reconciliar_grade.py:90`, `supra_juiz.py:135,222`, `marcar_abandono_provisorio.py:56` | **Avulso** para primeira ou última marca identificada. **Lote** nas contagens por dia. Em `reconciliar_grade`, manter a identidade da marca que deu o mínimo. As portas e os critérios de abandono ou emissão continuam os existentes. [app/ponto/management/commands/emitir_furo_retroativo.py:75] [app/ponto/management/commands/emitir_furo_retroativo.py:100] [app/ponto/management/commands/reconciliar_grade.py:90] [app/ponto/management/commands/supra_juiz.py:135] [app/ponto/management/commands/supra_juiz.py:222] [app/ponto/management/commands/marcar_abandono_provisorio.py:56] |
| `app/ponto/management/commands/reconciliar_fantasmas.py:66` | **Fica como legado explícito** no ramo cujo único insumo é `pendentes` textual. O caminho com dia forte já o consulta primeiro. Migrar uma marca desse ramo para **avulso** exige identificação comprovada. [app/ponto/management/commands/reconciliar_fantasmas.py:58] [app/ponto/management/commands/reconciliar_fantasmas.py:179] |
| `app/ponto/management/commands/flip_automatico.py:64` e `app/ponto/services/flip_auto.py:63,140` | **Avulso/lote** para dia de diagnóstico e nota do comando. `:63,140` alimentam a seleção de flip. Manter esses dois sítios pendentes até demonstrar em sombra o efeito sobre a fila e preservar o tipo gravado no escopo desta obra. [app/ponto/management/commands/flip_automatico.py:62] [app/ponto/management/commands/flip_automatico.py:75] [app/ponto/services/flip_auto.py:63] [app/ponto/services/flip_auto.py:104] [app/ponto/services/flip_auto.py:140] |
| `app/ponto/propositor.py:364` e `app/ponto/services/cartorio.py:1236` | **Lote** para selecionar as marcas apuráveis do dia da completude posicional. **Avulso** ou mapa já disponível para o piso de adesão do cartório. [app/ponto/propositor.py:341] [app/ponto/propositor.py:364] [app/chamados/reconciliador.py:113] [app/ponto/services/cartorio.py:1236] |
| `app/ponto/services/credito_parcial.py:36`, `dia_pago.py:132`, `dias_em_aberto.py:123`, `esmeril_espelho.py:179,186`, `espelho.py:84,440` | **Dia atribuído em lote**, transportado com o período ou lido pela batida identificada. Na pauta, aplicar o dia à última marca da janela. No espelho, preservar a população de períodos e as rubricas, mudando a chave do recorte. O espelho já tem a ponte por instante e PK para os cards. [app/ponto/services/credito_parcial.py:36] [app/ponto/services/dia_pago.py:128] [app/ponto/services/dias_em_aberto.py:122] [app/ponto/services/esmeril_espelho.py:179] [app/ponto/services/esmeril_espelho.py:186] [app/ponto/services/espelho.py:84] [app/ponto/services/espelho.py:440] [app/ponto/services/espelho.py:465] |
| `app/ponto/services/entrada_sincronizada.py:27` | **Avulso**. A função já recebe `batida`. Consultar a cobrança do dia atribuído e preservar o gate de lastro e a transição. [app/ponto/services/entrada_sincronizada.py:19] [app/ponto/services/entrada_sincronizada.py:27] [app/ponto/services/entrada_sincronizada.py:30] |
| `app/ponto/services/fechamento.py:224,813,859,1593` | **Lote** para pertença à fatia e presença em dias órfãos. Em `:859`, levar ao período órfão a resposta já atribuída. Em `:1593`, migrar a entrada dos **eventos de batida** para entregar o dia do **avulso** à competência. A normalização ainda serve outros eventos com data ou instante próprios. [app/ponto/services/fechamento.py:224] [app/ponto/services/fechamento.py:280] [app/ponto/services/fechamento.py:813] [app/ponto/services/fechamento.py:859] [app/ponto/services/fechamento.py:1593] [app/ponto/services/fechamento.py:1833] [app/ponto/registro_batida.py:174] |
| `app/ponto/services/regularizacao.py:73`, `triagem_batida.py:247,254,260,538,552,655`, `app/ponto/views.py:1629,3411` | **Avulso** para a chave da justificativa, para a entrada sincronizada pela triagem, e para o dia exibido no retorno. Em `regularizacao:73`, já há `Turno` encontrado por PK. Em `triagem:247,254`, a geometria construindo o **instante** de saída permanece civil onde usa o dia físico da entrada. Em `:552`, a carga civil pode ficar e a chave final da cobrança passa ao juiz com prova da saída correspondente. Para `:655`, separar o estado pré-registro do dia da batida aceita. Em `views:1629`, **lote** para a entrada da justificativa de um `data_turno` já dado. [app/ponto/services/regularizacao.py:60] [app/ponto/services/regularizacao.py:73] [app/ponto/services/triagem_batida.py:247] [app/ponto/services/triagem_batida.py:254] [app/ponto/services/triagem_batida.py:260] [app/ponto/services/triagem_batida.py:538] [app/ponto/services/triagem_batida.py:552] [app/ponto/services/triagem_batida.py:655] [app/ponto/services/triagem_batida.py:729] [app/ponto/views.py:1629] [app/ponto/views.py:3411] |
| `app/ponto/motor_calculo_v2.py:578,686,687,951,1244,1330,2069,2109,2134,2214,2216,2226,2419,2424,2595,2655,2659,2660,2961,2974,3346,3467,3485,3510,3514,3515` | **Ficam como dívida explícita da zona inviolável.** Destino de desenho é alimentar o dia do juiz com o par/período e consumi-lo em previsto, pontualidade, feriado, semana, DSR, banco e recorte. A herança por gap é um escritor paralelo onde substitui o dia. Para esta obra, a correção da âncora NOTURNO-A é antecedente, com chamada ao dia do par já presente na cópia. [CLAUDE.md:143] [app/ponto/motor_calculo_v2.py:578] [app/ponto/motor_calculo_v2.py:686] [app/ponto/motor_calculo_v2.py:687] [app/ponto/motor_calculo_v2.py:951] [app/ponto/motor_calculo_v2.py:1244] [app/ponto/motor_calculo_v2.py:1330] [app/ponto/motor_calculo_v2.py:1545] [app/ponto/motor_calculo_v2.py:2069] [app/ponto/motor_calculo_v2.py:2109] [app/ponto/motor_calculo_v2.py:2134] [app/ponto/motor_calculo_v2.py:2214] [app/ponto/motor_calculo_v2.py:2216] [app/ponto/motor_calculo_v2.py:2226] [app/ponto/motor_calculo_v2.py:2419] [app/ponto/motor_calculo_v2.py:2424] [app/ponto/motor_calculo_v2.py:2595] [app/ponto/motor_calculo_v2.py:2655] [app/ponto/motor_calculo_v2.py:2659] [app/ponto/motor_calculo_v2.py:2660] [app/ponto/motor_calculo_v2.py:2961] [app/ponto/motor_calculo_v2.py:2974] [app/ponto/motor_calculo_v2.py:3346] [app/ponto/motor_calculo_v2.py:3467] [app/ponto/motor_calculo_v2.py:3485] [app/ponto/motor_calculo_v2.py:3510] [app/ponto/motor_calculo_v2.py:3514] [app/ponto/motor_calculo_v2.py:3515] |

O caminho de produção de `data_de_adesao` fica **[nao verificado]**. O `bin/remedir_congelados_placar_e.py` tem execução manual para a competência 09 fixada, com J diagnóstico em `:43`. Seu uso corrent

[truncado em 20000 de 56159 caracteres]

## 26/11 00:53 · sessao e058368a5b02 · FERIADO-SEM-LIMITE-DO-SUPRIMIDO · construtor · origem=Code

Implementei a cura na cópia `4d9de1216`. No caso do opus, a promoção passou de **450 min de HE100 e 360 min de HE100 noturna** para **450 e 420 min**, tanto no `FechamentoMensal` quanto no `DiaPago`, na execução com persistência em memória. `ponto/tests/test_feriado_sem_limite_do_suprimido.py:241`

- `apurar_por_fatia` registra, por par de folga, o `prorrogacao_noturna_pos5h` do motor que o julgou. `dobra_da_folga_feriado` usa essa flag e os intervalos do par para chamar `he_noturna` sobre o trabalhado inteiro. A conta agrupa pelo dia da jornada e limita a noturna à HE100 restante. `ponto/services/fechamento.py:303`, `ponto/services/dia_pago.py:135`, `ponto/motor_calculo_v2.py:1885`
- `recalcular_fechamento_mes` soma o mapa devolvido pela porta e passa o mesmo mapa a `lavrar`. A chamada auxiliar do oráculo também recebe esse mapa antes de aplicar as rubricas do calculador. `ponto/services/fechamento.py:977`, `ponto/services/fechamento.py:1225`, `ponto/services/fechamento.py:1302`, `ponto/services/dia_pago.py:289`, `ponto/services/dia_pago.py:356`
- `minuto_em_duas_rubricas_do_dia` voltou à acumulação incremental do pai `160173bb1:app/folha/porta_export.py:547`, com a mesma margem de 1 minuto. O caso sintético de três pares acusa HE de `170.3` com teto `169.29999999999995`. Com a soma reordenada, o teto sobe para `169.30000000000007` e o vigia devolve `None`. `folha/porta_export.py:340`, `ponto/tests/test_feriado_sem_limite_do_suprimido.py:353`

### RED sobre `67dd8957e`

| Caso | Valor na base → esperado | Asserção |
|---|---|---|
| Opus | HE100/noturna **450/360 → 450/420 min** | `ponto/tests/test_feriado_sem_limite_do_suprimido.py:248` |
| FN2 | **120/0 → 120/120 min** | `ponto/tests/test_feriado_sem_limite_do_suprimido.py:302` |
| FD6 | **100/120 → 100/100 min** | `ponto/tests/test_feriado_sem_limite_do_suprimido.py:307` |
| Três pares | **210/100 → 210/210 min** | `ponto/tests/test_feriado_sem_limite_do_suprimido.py:312` |
| Dia comum no limiar | `None` → uma acusação de 170,3 min | `ponto/tests/test_feriado_sem_limite_do_suprimido.py:361` |

Os quatro REDs da promoção caíram em `assertEqual(promovida, trabalhada)`, que compara o mensal, o `DiaPago` e a soma das linhas diárias. O caso do opus usa batidas e o pareamento real. FN2, FD6, três pares, G0 e duas fatias fixam a geometria na saída do motor e atravessam o fechamento e a lavra. Os controles com `S=0` passaram também na base. F11c conservou **9,00 h de HE100 e 1,00 h de HE100 noturna** no ensaio em memória. `ponto/tests/test_feriado_sem_limite_do_suprimido.py:223`, `ponto/tests/test_feriado_sem_limite_do_suprimido.py:258`, `ponto/tests/test_feriado_sem_limite_do_suprimido.py:318`, `ponto/tests/test_o280_bateria_da_lei.py:1815`

No V1, o spy recebeu `noturno=` e chegou à asserção de **540 min contra teto de 480** com a conta antiga. Com a conta curada, o mensal registrou **480 min** e `medir` absolveu o dia no ensaio com leitores adaptados em memória. `ponto/tests/test_feriado_sem_limite_do_suprimido.py:415`, `ponto/tests/test_feriado_sem_limite_do_suprimido.py:426`

### G0 caracterizado com o motor real

| Caso | HE100 noturna | Relógio | Teste | [nao verificado]
|---|---:|---:|---|
| Comercial 02:00–07:00, prorrogação ligada | 300 | 300 | `ponto/tests/test_feriado_dobra_noturna_g0.py:63` |
| Comercial 02:00–07:00, prorrogação desligada | 180 | 180 | `ponto/tests/test_feriado_dobra_noturna_g0.py:67` |
| 12x36 19:00–07:00, pausa 01:00–02:00, hora reduzida afastada | 360 | 360 | `ponto/tests/test_feriado_dobra_noturna_g0.py:70` |
| Partido 21:00–00:00 e 03:00–07:00 | 120 / 240 | 120 / 240 | `ponto/tests/test_feriado_dobra_noturna_g0.py:74` |
| Entrada 00:10, efetiva 00:30, saída 05:00 | 270 | 270 | `ponto/tests/test_feriado_dobra_noturna_g0.py:91` |
| Batidas 00:10–05:30, pontas efetivas 00:30–05:00 | **240** | **270** | `ponto/tests/test_feriado_dobra_noturna_g0.py:94` |

O split recebe as pontas registradas. No caso 6, recua os 270 min trabalhados da saída às 05:30 até 01:00 e conta até 05:00. O relógio recebe as pontas efetivas, 00:30–05:00. A igualdade entre os campos ficou como observação dos casos, sem força de regra. `ponto/motor_calculo_v2.py:392`, `ponto/motor_calculo_v2.py:440`, `ponto/motor_calculo_v2.py:1050`, `ponto/motor_calculo_v2.py:1068`, `ponto/tests/test_feriado_dobra_noturna_g0.py:1`

### Comandos para o Code

Os nove mutantes abaixo caíram por asserção na execução em memória. O executor carrega a base e os mutantes no processo. A suíte Django nesta cópia parou no import de `django.core`. Execução Django e certificação final **[nao verificado]**. `ponto/tests/mutantes_feriado_sem_limite_do_suprimido.py:21`, `ponto/tests/mutantes_feriado_sem_limite_do_suprimido.py:55`, `manage.py:11`

```bash
export PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings_test
mutante() { python3 -m ponto.tests.mutantes_feriado_sem_limite_do_suprimido "$1"; }

mutante base
mutante mensal-le-par
mutante lavra-le-par
mutante sem-noturno
mutante sem-limite
mutante relogio
mutante dia-civil
mutante ultima-fatia
mutante sem-intervalos
mutante soma-reordenada

python3 manage.py test \
  ponto.tests.test_feriado_dobra_noturna_g0 \
  ponto.tests.test_feriado_sem_limite_do_suprimido \
  ponto.tests.test_o280_bateria_da_lei.FronteiraTest.test_F11c_promocao_ft_feriado_leva_a_parte_noturna_para_a_folha -v 2
```

### Catraca e pergunta em aberto

A medição encontrou quatro deslocamentos. O arquivo da catraca ficou intacto. Seus 35 testes passaram com as quatro referências ajustadas apenas em memória. O diff acrescenta **zero `except`**. `core/tests/test_medida_turno_do_par.py:563`, `ponto/services/dia_pago.py:135`, `ponto/services/fechamento.py:303`, `folha/porta_export.py:340`

| Referência | Linha velha → nova |
|---|---|
| `folha/porta_export.py::medir` | `583 → 586` |
| `ponto/services/fechamento.py::_so_a_data` | `1586 → 1592` |
| `ponto/services/fechamento.py::apurar_por_fatia` | `213 → 215` |
| `ponto/services/fechamento.py::recalcular_fechamento_mes` | `664, 666, 672, 692, 763, 802, 848, 860 → 669, 671, 677, 697, 768, 807, 853, 865` |

Fica para Ronald a pergunta sobre qual ancoragem o motor deve usar quando as pontas registradas e efetivas divergem, como nos **240/270 min** do G0. O agrupamento civil do vigia e a regra do oráculo seguem na fila. Ocorrência e impacto por rubrica nas competências 10 e 11 **[nao verificado]**. `ponto/tests/test_feriado_dobra_noturna_g0.py:94`, `folha/porta_export.py:582`, `ponto/calculador/regras.py:679`

## 26/11 00:52 · sessao 7fd7cd13849e · CADEIA-20-PASSO-1 · leitura · origem=Code

# Dossiê de leitura — CADEIA-20-PASSO-1

**Base proposta para o construtor: o main após os pousos de E1 v4 e PAR c7, com a raia como referência de código e de REDs.** A raia prova a costura grade → juiz → ata → tela/PDF. Ainda perde a identidade e o instante completo na escrita da ata, publica dois conceitos diferentes de “zero” e não incorpora as mudanças de parcial, protestos e projeção de perguntas do main. Restam **6 perguntas de lei** na leitura abaixo. [Raia `app/escala/utils.py:2117–2118`, `app/escala/orfa_classe.py:56–85`, `app/ponto/services/cartorio.py:962–1056`; main `app/escala/utils.py:1174–1196,1271–1282`, `app/ponto/services/cartorio.py:1115–1131,1693–1700`; `avais.txt:39`]

**Referências.** `R` é a cópia local indicada como `a4a9b6bfe`. `M` é o main indicado como `c314c099f`, citado pelo mapa e pelo diff fornecidos. Os anexos são citados pela posição no texto fornecido, ou pela seção quando indicado. A cópia local veio sem metadados Git. A identidade dos objetos Git, o `merge-tree`, os logs brutos e a execução dos REDs ficam [nao verificado]. A conferência local foi de código e da saída da catraca AST, em modo somente leitura. [`raia_log.txt:1–16`; `conflitos_raia_x_main.txt:1–6`; `mapa_contrato_c314.txt`, abertura; R `app/escala/tests/test_cadeia20_selo_orfas.py:197–222`]

## 1. Mapa curto

### (a) Já faz?

| Entrega | Estado |
|---|---|
| **Juiz** | A raia substitui `classificar_orfas` por `razao_da_sobra` e registra o corte. Para uma ligação válida, aplica dia sem trabalho → rajada → papel do pareador. CxR é uma marca `True`/`False`/`None`. `SEM_BATIDA`, `BATIDA_AMBIGUA` e `SEM_PAPEL` saem no campo `falha`, com `razao=None`. O residual explícito do aval 2320 está ausente do juiz da raia. [R `app/escala/orfa_classe.py:3–29,37–85`, `app/core/juizes.py:1327–1333`, `app/docs/CORTES.md:563`; `avais.txt:19,22`] |
| **Fonte e lavra** | A grade por turno mantém `(HH:MM, instante)` por ocorrência e linha, inclusive na retirada de uma ponta reconciliada. O cartório usa o pareador já carregado e lê CxR da lavra existente. Há ligação por instante com distinção de PK entre candidatos do pareador, mas o PK saiu de `locs` e a ata final serializa `hora`, `razao`, `falha` e `escala_nao_descreve`. O consumidor persistido ainda não recebe identidade nem segundos. O caminho legado do dispatcher não fornece o pacote da razão. [R `app/escala/utils.py:1343–1360,1723–1759,1843–1871,2034–2051,2084–2085,2117–2137`, `app/escala/orfa_classe.py:32–65,82–85`, `app/ponto/services/cartorio.py:962–1001`] |
| **Leitores** | `sobras_da_ata` entrega hora, frase ou estado ao espelho. A grade da célula publica `sobras` ao lado de `orfas`. PDF e template leem a projeção. O PDF ainda agrega horas em um conjunto de toda a janela para suprimir turno aberto e usa `Counter` por `HH:MM` para marcar batidas. Calendário, supra, chamados, causas da folha e vigias ainda têm acessos crus. `cluster_espurio` compartilha o predicado de rajada, mas continua lendo horas e lâmpadas. [R `app/escala/services/leitor_celula.py:193–223,453–454`, `app/ponto/services/espelho.py:703–710,862`, `app/relatorios/pdf_espelho.py:176–180,478,514,595,874`, `app/templates/ponto/espelho.html:151,249`, `app/colaboradores/services/calendario.py:409–410`, `app/ponto/supra_juiz.py:392,527`, `app/chamados/juizes.py:514`, `app/folha/causas.py:43,100`, `app/ponto/services/cluster_espurio.py:29–36`] |
| **Contadores** | `medir_razoes_sobras` mede ocorrências, falhas, chaves ausentes e desalinhamento por ata. `atas_no_horizonte_sem_razao` mede atas com estrutura incompleta no horizonte v5 e aceita entrada com `falha` explícita. Assim, **0 atas incompletas é compatível com 2 ocorrências sem razão**. A catraca local deu **5 Python, 0 HTML**, com teto 5/0 para exibição. A publicação do `sobras_sem_razao` exigido no placar e no HAIKU [nao verificado]. [R `app/ponto/services/cartorio.py:1004–1056,2117–2133`, `app/ponto/management/commands/processar_cartorio.py:75–82`, `app/escala/tests/test_cadeia20_selo_orfas.py:9–15,99–107,197–222`; `relato_linhas.txt:35–37`; `avais.txt:10,24`] |

**Para o PRONTO**, faltam prova de identidade persistida e leitura concordante, `sobras_sem_razao=0` por ocorrência, efeito em folha/retenção na base de pouso, idempotência na mesma base, os REDs que reproduzem as divergências atuais e o fechamento do desenho A. A suite de **17.069** verdes é da v2n `7efb46e8e`. Na volta 3, o relato registra **370 testes, 2 falhas** em `test_cartao_marca_espuria`, com **14/14 mutantes mortos**. O caso sem célula perde a fonte de sobras da grade para a exibição e a guarda de turno aberto do PDF. A frequência desse caso na frota, o `CHECK-DE-TELA` contra a referência, o verificador final com 0 BLOQUEIA, a Pista 2 verde da candidata, o opus e o `SMOKE DELE` estão [nao verificado]. [`avais.txt:23–24,29–33,39`; `relato_linhas.txt:35–42`; R `app/ponto/services/espelho.py:703–710`, `app/relatorios/pdf_espelho.py:478,514`]

### (b) Atravessa o quê?

`Batida` apurável → vínculo/DNA e linha dona na grade → testemunho de `turnos_do_colab`/`parear_turnos` e CxR lavrado → `razao_da_sobra` → cartório → `CelulaDia.lavrar_veredito`/`carimbar` → `salvar_e_emitir`/`EventoCelula` → `leitor_celula`, espelho e PDF. Durante o ato, supra e emissão recebem a ata em memória **antes** do ponto onde a raia hoje acrescenta as razões. A razão comum durante o próprio ato é, portanto, lacuna da raia. [R `app/ponto/turnos.py:2876–2903`, `app/escala/utils.py:1321–1337,1915–1936,2117–2118`, `app/ponto/services/cartorio.py:67–77,1090–1096,1579–1583,1607,2012–2037`, `app/ponto/portas/celula.py:957,1049–1067,1194–1202`, `app/escala/services/leitor_celula.py:193–223`]

O desenho A atravessa ainda o fecho C6, o veto de competência `_K6`, E2, fábrica, emissor, repasse, signals e consumidor do outbox. O censo histórico contou **64 sítios** e **36 registros** em `eb1e49c1e`, com **61** entradas iniciadas fora do cartório, **27** sítios alcançáveis de `on_commit` e **33** por signal ou fila. O recenso pós-F5 previsto no aval, com o rol completo e testemunha por sítio, está [nao verificado]. [`relato_linhas.txt:6–9`; `RASCUNHO_BRIEF.md:1,8–33`; `avais.txt:26–33`; R `app/chamados/services/fio.py:1358–1440`, `app/chamados/reconciliador.py:1366–1427`, `app/ponto/services/cartorio.py:1132–1177`]

### (c) Quanto se usa?

| Rodada e fonte | Resultado registrado |
|---|---|
| **B1, comp. 11**, `logs/QUEIMA-1911/B1/sonda_b1v2.out` | **1.307** sobras, incluindo **15** da empresa de teste. A função antiga real, chamada com ata reconstruída, devolveu **660** `folga_desmentida`, **540** `geometria`, **105** `indefinida`, **2** `eco_cluster`. Pareador: **655** pares fechados, **453** pausas, **136** abertos, **36** sem batida pelo minuto, **20** saídas sem entrada, **7** ambíguas pelo minuto. Entre as 453, **110** com `_intra_ata`, **343** outras. [R `app/docs/RELATO.md:253`] |
| **Resposta 2320**, `logs/CADEIA-20/passo1/sonda.r`, `pronto.md`, `sonda3.py` | Mesmo total de **1.307**, com **456** jornada adicional, **453** pausa com origem incompleta, **199** cadastro, **136** abertos, **36** sem batida pelo minuto, **20** saídas sem entrada, **7** ambíguas e **0** residual. Pareador × ata: **455 dias / 655 sobras**, inclusive col81 01/11, 18:31. Na inversa, **333 dias / 336 batidas** com luz acesa e avulsas no pareador, com **6.454** `ATA_DESALINHADA` fora da comparação. Col87 e col975 na lista CxR. [R `app/docs/RELATO.md:244,253–254`] |
| **Etapa A**, `logs/CADEIA-20/passo1/brief.md` | Em **outra foto**, **47/47** aparentes “sem batida” estavam vivas no dia civil seguinte. `sem_papel=0/25.664` batidas. Cadastro literal dos nominais incompleto. A causa das **2** falhas da v3 precisa de testemunha própria. [R `app/docs/RELATO.md:135`; `relato_linhas.txt:35–37`] |
| **v2l e v2m**, `logs/CADEIA-20-PASSO-1/v2l/dinheiro_{main,copia}.out`, `logs/cadeia20_copia_2m.out` | v2l: **618** colaboradores, **0** diferenças de folha, **0** em **4.380** retenções, **19** células mudando na segunda passada em cada árvore. v2m: **18.333** células, **618** colaboradores, **1.172/1.172** sobras com razão, **0** células mudando na segunda passada. [R `app/docs/RELATO.md:3–6,16`; `relato_linhas.txt:44–45`] |
| **v3 `a4a9b6bfe` × base `eb1e49c1e`**, `logs/CADEIA-20-PASSO-1/v3/medidas/{main,copia}.out`, `cartao_diff.txt`, `dia_diff.txt` | **34.667** células v5. **2.233** sobras = **2.231** com razão + **2** `SEM_BATIDA`; **0** desalinhamentos e **0** atas incompletas em três empresas × duas passadas. Hash de `FechamentoMensal`/`ExportacaoDominio` igual antes/depois, mas **80/2.412** cartões diferiram, **5** mudaram resumo e, na comp. 09, **63** cartões mudaram a lista de sobras em **101** dias. O aval 2325 registrou a opção de pousar após PRONTO e opus, com Pauta DP dos quatro. Essa medição é contra a base antiga. [`relato_linhas.txt:33–37`] |
| **Desenho A**, `logs/cadeia20_matriz_p1_clausula.out`, `logs/cadeia20_ficha_1545{,_b,_c}.out` e `logs/CADEIA-20-PASSO-1/ficha_1545/roda.porta` | **18** recusas na função real `_decisao_do_fechamento`, em **3 células × 2 relógios × 3 etapas**, pela mesma cláusula. No braço encerrado, **9, 5, 0** células mudaram de foto após o corpo inicial nas três rodadas. No braço `cruza`, a terceira rodada ainda alcançou a célula **129134** e **31 escritas da fábrica**. A expiração do turno retira `enc=0` da impressão e explica a possibilidade de relavra. A atribuição das 31 escritas a cada etapa [nao verificado]. [`relato_linhas.txt:8–15`; R `app/chamados/models.py:676–705`, `app/ponto/services/cartorio.py:1141,1168–1177,1540–1557`] |
| **Sondas laterais 5(i)/(ii)**, `logs/RAIA-ESPURIAS/auditoria_1811/{s5i,s5ii}.r` | DXG: **14 plantadas / 13 pessoas**, todas a **62–180 min** da batida real, fora de ±60 min. A função atual do lote respondeu `b_conversa` nas **14**. O caminho histórico que as gravou [nao verificado]. Dedup: **38** pares opostos adjacentes em **24.695** batidas, repartidos **9 / 7 / 22** nas três faixas medidas, e **1/38** colisão na função real. O anexo traz exemplos de aparelho/app, mas a tabela completa por aparelho e versão [nao verificado]. [R `app/docs/RELATO.md:247,299,308,316`; `avais.txt:11–13`] |

**Sonda proposta, como texto.** O núcleo abaixo é para a cópia da raia, com credencial de leitura da sombra e `SONDA_AGORA` fixado para a foto. Mede a resposta real do juiz e da lavra **em memória**, a coincidência de instantes no insumo, conflitos de CxR e os estados já gravados. O recorte da competência é por empresa e o contexto do cartório preserva a janela do lote. A sonda não foi executada [nao verificado]. [R `app/folha/export.py:47`, `app/ponto/management/commands/processar_cartorio.py:47–69`, `app/ponto/services/cartorio.py:962–1056,1273,1298–1317`, `app/ponto/services/cxr_na_lista.py:165–185`]

```python
# Corpo para o shell Django da sombra, com SONDA_AGORA em ISO 8601 com fuso.
from collections import Counter
from datetime import datetime, time, timedelta
from os import environ
import json

from django.db import connection, transaction
from django.utils import timezone
from escala.models import CelulaDia
from escala.utils import montar_grade_prevista_periodo
from folha.export import janela_competencia
from ponto.services import cartorio
from ponto.services.cxr_na_lista import cxr_lavrado
from ponto.services.vigia_pareador_celula import censo
from ponto.turnos import batidas_apuraveis

agora = datetime.fromisoformat(environ["SONDA_AGORA"])
hoje = timezone.localtime(agora).date()
um = timedelta(days=1)

def meia_noite(dia):
    return timezone.make_aware(datetime.combine(dia, time.min))

totais = Counter()
with transaction.atomic():
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
    cxr = cxr_lavrado()
    cels = list(
        CelulaDia.objects.filter(data__lt=hoje)
        .select_related("colaborador__empresa")
        .order_by("colaborador_id", "data")
    )

    from itertools import groupby

    for _, grupo in groupby(cels, key=lambda cel: cel.colaborador_id):
        lote = list(grupo)
        colab = lote[0].colaborador
        ini, fim = janela_competencia(colab.empresa, 11, 2026)
        alvo = [cel for cel in lote if ini <= cel.data <= fim]
        if not alvo:
            continue

        d0, d1 = lote[0].data, lote[-1].data
        ref = d1 + um
        batidas = list(batidas_apuraveis(
            colab, meia_noite(d0 - um),
            meia_noite(d1 + 3 * um) - timedelta(microseconds=1),
        ))
        por_instante = Counter(b.timestamp for b in batidas if b.tipo in ("E", "S"))
        grade = montar_grade_prevista_periodo(
            colab, batidas, dias=(d1 - d0).days + 2, ref=ref, agora=agora,
        )
        insumos = grade["insumos_razao_sobras"]
        medida = cartorio.medir_razoes_sobras(alvo)
        for chave in ("sobras_total", "com_razao", "sem_razao", "orfas_sem_chave",
                      "celulas_desalinhadas", "sem_razao_sem_falha"):
            totais["gravado", chave] += medida[chave]
        totais["gravado", "atas_incompletas"] += cartorio.atas_no_horizonte_sem_razao(alvo)[0]

        for cel in alvo:
            dia = grade["dias"][(ref - cel.data).days]
            ata = cartorio.ata_do_dia(dia, grade["marcos"], dna=cel.dna)
            cartorio._lavrar_razoes_sobras(ata, cel, cel.data, insumos, cxr, colab)
            pares = insumos["instantes"].get(cel.data, [])
            totais["memoria", "sobras"] += len(ata["orfas"])
            totais["memoria", "ordem_ou_multiplicidade_diverge"] += (
                [h for h, _ in pares] != ata["orfas"]
            )
            totais["memoria", "instante_com_varios_registros"] += sum(
                por_instante[ts] > 1 for _, ts in pares
            )
            for entrada in ata["razoes_orfas"]:
                chave = entrada["razao"] or entrada["falha"]
                totais["razao_ou_falha", str(chave)] += 1
                totais["cruzamento_cxr", str((chave, entrada["escala_nao_descreve"]))] += 1

    vigia = censo(11, 2026)
    print(json.dumps({
        "contagens": [[*k, v] for k, v in sorted(totais.items())],
        "pareador_x_ata": len(vigia["casos"]),
        "universo_vigia": vigia["universo"],
        "cego": vigia["cego"],
    }, ensure_ascii=False))
```

**Complementos de leitura da sonda.** Para medir o main, use a grade real de `c314` e a função antiga sobre o insumo reconstruído, com a metodologia B1. Compare com a raia na **mesma** foto e publique dois universos, ata existente e grade remontada. Para separar a origem das **453** pausas, conte em execução a linha que atribui `_intra_dur`/`_intra_ata` à batida **por segmento** e agregue por PK no resultado da grade. A foto final do carimbo não permite essa separação. Para dinheiro, idempotência e desenho A, a sonda somente leitura compara fotos já preparadas por rodada/etapa na sombra. Em cada foto, chama `reconciliaria()` para a decisão de fecho sem escrita e lê a trilha existente para contar o que efetivamente escreveu. Sem as fotos intermediárias, o escritor das 31 mudanças e o contrafactual de rollback continuam [nao verificado]. Os contadores específicos de cada escolha estão em §9. [R `app/docs/RELATO.md:225,244,253`, `app/ponto/turnos.py:415–416,456–457,486,1317–1318,1370,1413,1477–1478,1984–2004`, `app/chamados/models.py:670–705`; `relato_linhas.txt:8,18,21`]

## 2. Estado da raia × main

**Recomeçar a integração no main, reaproveitando o juiz, o contrato de transporte e os REDs por hunk após conferir as autoridades vigentes.** Os seis conflitos são textuais segundo o anexo. Há mudanças semânticas também em arquivos que passaram pelo `merge-tree`. Um rebase que apenas zere conflitos não demonstra preservação dessas mudanças. [`conflitos_raia_x_main.txt:1–6`; `raia_log.txt:1–16`; R `app/escala/orfa_classe.py:37–85`; M `app/ponto/turnos.py:2192–2215,2663–2728`]

| Sítio | Conflito e consequência |
|---|---|
| **`app/ponto/services/cartorio.py`** | **Conflito 5.** O main move `protestos` para a coluna final gravada por dia julgado no ato, inclusive `nunca_bateu`, e deduplica `julgadas`, `carimbadas` e `nunca_bateu` entre passadas. `carimbadas` depende do que a porta de fato gravou. Também faz prefetch das decisões, consulta `DecisaoPergunta.composta` e só descarrega o log após a precedência no caminho normal. A janela estável da segunda passada e a lavra da razão da raia precisam encaixar nessas mudanças. O `_tz.now()` na guarda, mesmo com `agora` informado, continua visível em ambos os recortes. [M `app/ponto/services/cartorio.py:141–149,1101–1131,1295–1302,1343–1352,1515–1522,1693–1700,1926–1946,2016–2023,2105–2134`; R `app/ponto/services/cartorio.py:1152–1164,1264–1273,2012–2037,2196–2205`; `conflitos_raia_x_main.txt:5`] |
| **`app/escala/utils.py`** | **Conflito 4.** O main transporta a direção de **cada sobra da linha** em todos os produtores para julgar cobertura parcial, inclusive legado, pontas sem coluna e conservação civil. A raia transporta índice/instante no builder por turno. O encaixe precisa preservar, na mesma ocorrência, linha, instante, identidade e direção para a parcial, inclusive ao reconciliar uma ponta. A direção da parcial tem efeito próprio na borda. [M `app/escala/utils.py:595–682,770–828,1174–1196,1271–1282,1744–1782,1867–1902`; R `app/escala/utils.py:595,1723–1759,1843–1887,2034–2051,2084–2086`; `conflitos_raia_x_main.txt:4`] |
| **`app/ponto/turnos.py`** | **Sem conflito listado, com mudança semântica.** O main substitui `horas_reais_do_dia` por `batidas_reais_do_dia`, retém o minuto mostrado e usa a cadeia para o deslocamento noturno em dias. `batida_dentro_da_parcial` lê a direção: S no início e E no fim ficam fora, E no início e S no fim ficam dentro. Marco previsto continua inclusivo. O main também extrai `preparar_par_completo_da_janela`. A contribuição da raia é o coletor opcional de avulsas nas cascas por segmento, com a chamada de `parear_turnos` preservada. [M `app/ponto/turnos.py:2189–2215,2641–2728`; R `app/ponto/turnos.py:1850,1920–1923,1984–2006`; `conflitos_raia_x_main.txt:1–6`] |
| **Catraca de linhas, registro e harness** | **Conflitos 1, 2, 3 e 6.** Reconciliar `LINHAS_REGISTRADAS` com a árvore resultante, incorporar no `CORTES.json` a entrada do juiz e conferir o `CORTES.md` gerado. Reaplicar o harness de `nunca_bateu` à interface final e manter as mordidas, inclusive a injeção da borda de lavra. O conteúdo concorrente do main nesses quatro conflitos [nao verificado]. [R `app/core/tests/test_medida_turno_do_par.py:247,559`, `app/docs/CORTES.json:2125–2126`, `app/docs/CORTES.md:563`, `app/ponto/tests/test_e3_nb_impressao_memoria.py:168–174`, `bin/gerar_cortes.py:23–24`; `conflitos_raia_x_main.txt:1–3,6`] |
| **Espelho, PDF e template** | O main lê o estado vigente de HE na janela e imprime o rótulo, com mudanças de apresentação da ausência no template. O transplante da razão da sobra precisa conviver com esses dados de tela e com a igualdade cartão/espelho. [M `app/ponto/services/espelho.py:784–798,930–950`, `app/relatorios/pdf_espelho.py:935–945`, `app/templates/ponto/espelho.html:186–194`; R `app/docs/LEIS.md:85`] |

## 3. Censo de escritores e leitores

**escritores:**

- **Grade, em memória.** Produz e altera `orfas` no casamento, no legado, nas pontas de override, no intervalo sem coluna e na conservação. Overlay e recorte limpam a lista. A reconciliação tira a ocorrência que voltou a marco. A cópia da ata para alimentar o pareador é em memória. A raia migrou o transporte de instantes no caminho por turno e a remoção em conjunto. [R `app/escala/utils.py:630–637,674–682,779–828,972–987,1028–1064,1174,1239,1274–1279,1723–1785,1829–1887,1923–1936,2034–2051,2084–2086`; M `app/escala/utils.py:628–681,770–828,1744–1808,1867–1902`]
- **Cartório, campos da ata.** `ata_do_dia` escreve `orfas` e `n_orfas`. `_lavrar_razoes_sobras`, só na raia, escreve `razoes_orfas` nos ramos normal e sem adesão. A porta exi

[truncado em 20000 de 59911 caracteres]

## 26/11 00:52 · sessao 8a3a0355f6bf · O30-F3-CONSTRUIR · construtor · origem=Code

Implementei a O30-F3 na cópia. O RED reproduzido sobre `a845dca16` acusou os seis argumentos ausentes nas três portas e a falta da flag de privacidade do CSV. Na cópia alterada, os seis testes sem banco passaram. O diff tem saldo de **zero `except` novos**. `ponto/tests/mutantes_atestado_f3.py:91` · `ponto/tests/test_atestado_f3_contrato.py:79` · `ponto/services/ausencia.py:1120`

### Cura e decisões

- **Porta.** `criar_ausencia`, `editar_ausencia_registro` e `anexar_documento` aceitam os seis campos opcionais. Campo omitido conserva o valor. O anexo e seus dados usam a raiz do intervalo. A trilha registra o ator e os campos alterados, sem copiar os valores médicos. A validação aceita emissão futura. `ponto/services/ausencia.py:269,282,291,315,678,1080` · `ponto/forms.py:13`
- **Drawer.** O formulário multipart aceita salvar os dados com ou sem arquivo. O POST exige `editar_ausencias` e `ver_ausencias` e devolve 403 na recusa. A leitura usa a permissão do anexo. `templates/ponto/partials/painel_ausencia.html:90` · `ponto/views_atestado.py:24` · `ponto/views.py:2095`
- **Lote.** A tela filtra por período ou competência, colaborador, empresa e termos do nome, com busca sem acentos. O PDF incorpora JPG e PNG e oferece **abrir anexo** para HEIC e PDF. A seção **Sem documento** usa `previa()['faltando']`. Quem tem apenas `ver_relatorios` recebe CSV e PDF sem dados médicos nem anexos. O ZIP exige `ver_ausencias`. `templates/ponto/ausencias.html:42` · `templates/relatorios/atestados_lote.html:6` · `relatorios/urls.py:16` · `ponto/views_atestado.py:45,70,89` · `ponto/services/atestados_lote.py:53,128,174` · `ponto/services/dia_decidido.py:624`
- **N provisório = 20 registros**, incluindo os sem documento. Escolhi recusar acima de N antes de abrir arquivos, com orientação para reduzir o recorte. O ZIP de 20 anexos sintéticos de 1 MB em memória levou **0,373 s**, ou **0,019 s por registro**. A medida de PDF por atestado e o tempo de resposta sob `gthread` ficam **[nao verificado]**. `ponto/services/atestados_lote.py:24` · `ponto/views_atestado.py:115` · `ponto/tests/test_atestado_f3_contrato.py:166` · `ponto/tests/test_atestado_f3.py:246` · `gunicorn.conf.py:19`

### Comandos a partir de `app/`

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 base
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest ponto.tests.test_atestado_f3_contrato
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test ponto.tests.test_atestado_f3 --settings=config.settings.ci
```

Os três primeiros mutantes abaixo falharam na asserção esperada. Os sete de integração ficam **[nao verificado]**. O comando Django parou no import de `django.core`, antes de acessar o banco. A suíte, o smoke no navegador, a medida do PDF e a certificação Code/Opus final ficam **[nao verificado]**. `ponto/tests/mutantes_atestado_f3.py:21,73` · `ponto/tests/test_atestado_f3.py:77,134,162,181,205,217,233,246` · `manage.py:11`

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 gravacao
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 cid_csv
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 escritor_externo
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 editar
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 cid_pdf
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 foto
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 link
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 sem_documento
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 empresa
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_atestado_f3 limite
```

## 26/11 00:44 · sessao 9f889a65ec6f · DIA-DA-BATIDA-JUIZ-UNICO · leitura · origem=Code

# DIA-DA-BATIDA-JUIZ-UNICO

**Parcialmente feito.** A cópia já tem um juiz do dia do turno, leitores migrados e duas catracas. Ainda há consumidores que usam o dia civil para escolher a célula, o vínculo, a competência ou o dia pago. A célula `turno/marcos × um juiz por pergunta` continua declarada verde com um teste de outro alcance. [app/core/juizes.py:215] [app/ponto/turnos.py:2992] [app/core/contratos_estruturais.py:224] [app/docs/BACKLOG.md:1858] [nao verificado]

Usei a cópia fornecida como `c314c099f3f447dbb628557c9ada9906183f1182`. A confirmação independente do hash e da referência `main` fica **[nao verificado]** porque a cópia veio sem metadados Git.

## (a) O que já faz

- **Juiz registrado** — `turnos_do_colab` compõe `parear_turnos`. A atribuição dentro do pareador sai de `_data_do_turno`. O leitor avulso é `dia_do_turno_da_batida`, e o leitor em lote é `dia_das_batidas`, também servido por `CargaDaJanela`. O mapa usa `Turno.data_turno` por PK quando um turno reclama a batida. Para a marca viva sem turno reclamante, devolve o dia civil local. Fora do universo, o avulso devolve `None`. [app/core/juizes.py:215] [app/ponto/turnos.py:248] [app/ponto/turnos.py:1192] [app/ponto/turnos.py:1850] [app/ponto/turnos.py:2951] [app/ponto/turnos.py:2992] [app/ponto/turnos.py:3010] [app/ponto/turnos.py:3113]
- **Quem já pergunta** — espelho web, PDF via espelho, espelho e justificativas do app, painel operacional e listagem de férias com batida. Para efeitos de negócio, já perguntam os fluxos de saída sem entrada das duas APIs e do PWA, o fio, a emissão de disputa e de órfão, a foto E4B, a migração de `data_evento` e o vigia de plantio. `batidas_do_dia_turno` também alimenta a guarda de emissão e a fila de flip. [app/ponto/services/espelho.py:465] [app/relatorios/pdf_espelho.py:596] [app/api/views.py:750] [app/api/views.py:1444] [app/api/views.py:2042] [app/api/views_core.py:744] [app/core/services/painel_op.py:330] [app/ferias/management/commands/listar_ferias_com_batida.py:54] [app/ponto/services/triagem_batida.py:729] [app/chamados/services/fio.py:1822] [app/chamados/services/disputa_emissao.py:283] [app/chamados/services/disputa_emissao.py:831] [app/escala/servico_jornada.py:509] [app/chamados/services/foto_e4b.py:283] [app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:160] [app/ponto/management/commands/vigia_plantio_overnight.py:87] [app/ponto/services/flip_auto.py:104]
- **Catracas existentes** — `test_contract_no_batida_date` tem duas allowlists motivadas **por arquivo**. A verificação encontrou os 6 arquivos da lista `.date()` e os 21 da lista ORM, sem entradas novas ou obsoletas. O primeiro detector tem pontos cegos para `localdate(timestamp)` terminal e apelidos de import. Um novo sítio em arquivo já permitido passa pelo critério de inclusão dessas listas. A catraca de `medida_turno_do_par` resolve apelidos, congela identidade e linhas e mede **9 parcelas de pareamento e 81 de derivação** nesta cópia. Ela usa B1 lexical por função e deixa o lookup ORM fora. As duas suítes passaram juntas, **39 testes**, em memória com `unittest.TestCase` no lugar de `SimpleTestCase`. Espelho, app e PDF também têm selos locais. [app/ponto/tests/test_contract_no_batida_date.py:10] [app/ponto/tests/test_contract_no_batida_date.py:97] [app/ponto/tests/test_contract_no_batida_date.py:156] [app/ponto/tests/test_contract_no_batida_date.py:196] [app/ponto/tests/test_contract_no_batida_date.py:231] [app/core/medida_turno_do_par.py:197] [app/core/tests/test_medida_turno_do_par.py:108] [app/core/tests/test_medida_turno_do_par.py:501] [app/ponto/tests/test_espelho_tela_le_o_juiz_do_dia.py:193] [app/api/tests/test_o14_app_le_o_juiz_do_dia.py:48] [app/relatorios/tests/test_pdf_sem_regra_propria.py:150] [nao verificado]
- **O que falta da obra** — migrar os juízos paralelos sobre pertença à jornada, motivar as exceções por sítio e ligar essa dívida à célula estrutural. O caso col673 já está registrado como prova. RED específico e DIFF da competência 10 para essa migração **[nao verificado]**. O aval das 12h absorveu o pedido em `PERGUNTA-SEM-SELO`, e o corte posterior voltou a nomear esta obra para a prova col673. [app/docs/PROMPTS.md:1535] [app/docs/PROMPTS.md:1536] [app/docs/PROMPTS.md:2823] [app/docs/BACKLOG.md:1858] [app/core/contratos_estruturais.py:224]

## (b) Censo AST da cópia

**212 ocorrências em 64 arquivos de `app/`**, mais **4 em 3 arquivos de `bin/`**, total **216 em 67 arquivos**. Cada expressão ou chave ORM conta uma vez, inclusive dois limites na mesma linha. A unidade explica parte da diferença para os ~98 em 50 arquivos estimados no BACKLOG. A busca usa o universo do medidor do projeto, mais `bin/` e os demais diretórios Python elegíveis. Fiz revisão de origem para incluir aliases de valores, datas em texto usadas como dia, carimbos de plantio e a herança por gap, e retirar datas que eram apenas relógio, marco previsto ou limite de relatório. Origem dinâmica e equivalências interprocedurais fora das explicitadas no script **[nao verificado]**. [app/core/medida_turno_do_par.py:26] [app/core/medida_turno_do_par.py:32] [app/core/medida_turno_do_par.py:197] [app/core/medida_turno_do_par.py:231] [app/docs/BACKLOG.md:1858]

**E = EXIBICAO**, incluindo data civil informativa de instrumentos. **P = ENTRADA DO PAR**, incluindo janela de carga ou geometria sem atribuição final. **J = JUIZO PARALELO**, quando a conversão decide uma chave, seleção ou resultado fora da autoridade da jornada. Em J, indico o efeito, inclusive quando fica só em diagnóstico. [app/docs/BACKLOG.md:1858] [app/ponto/tests/test_contract_no_batida_date.py:87]

### `app/api` — 12 [app/api/views.py:297] [app/api/views_core.py:285]

| Arquivo, linha e função | Expressão e classe |
|---|---|
| `app/api/views.py:297:api_me` · `app/api/views_core.py:285:api_me` | `timestamp__date=hoje` — **E**, contador da porta. |
| `app/api/views.py:584:api_bater_ponto` · `app/api/views_core.py:570:api_bater_ponto` | `timezone.localtime(ts_efetivo).date()` — **J**, escolhe vínculo e marcos do tipo retroativo antes de gravar. |
| `app/api/views.py:622,625,639:api_bater_ponto` · `app/api/views_core.py:613,616,630:api_bater_ponto` | `agora_local.date()` — **J**, dia usado na guarda dos motivos e no contexto de saída sem entrada. |
| `app/api/views.py:719:api_bater_ponto` · `app/api/views_core.py:713:api_bater_ponto` | `_tzc9.localdate(batida.timestamp)` — **J**, grava `data_turno` civil no chamado de auditoria leve. |

### `app/chamados` — 28 [app/chamados/detector_anomalias.py:140] [app/chamados/services/passivo_validado.py:595]

| Arquivo, linha e função | Expressão e classe |
|---|---|
| `app/chamados/detector_anomalias.py:140:detectar_batida_fora_escala` | `ts_local.date()` — **J**, escolhe o vínculo antes de consultar o turno e serve de recuo para o dia do desvio. |
| `app/chamados/juizes.py:55:_dia_da_batida` · `:205:dias_do_fato` | `timezone.localdate(b.timestamp)` — **J**, degrau batida no dia fraco do chamado, avulso e lote. |
| `app/chamados/juizes.py:171:_dia_do_contexto_fraco` | `_dt.datetime.fromisoformat(bruto).date()` — **J**, dia do fato a partir do instante guardado no contexto. |
| `app/chamados/juizes.py:276:_plantio_na_hora_do_slot` | `_local.date()` — **J**, dia de busca da prova de plantio quando falta `data_evento`. |
| `app/chamados/management/commands/auditar_disputas.py:111:Command.handle` | `timezone.localdate(b.timestamp)` — **J**, escolhe os marcos para acusar inversão no relatório. |
| `app/chamados/management/commands/carimbar_por_batida_coincidente.py:81:Command.handle` | `timezone.localdate(b.timestamp)` — **J**, dia usado para casar o carimbo por batida coincidente. |
| `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:102:_fonte_viva` · `:158:classificar` | `data_local(t.saida.timestamp)` — **P**, identifica a origem pela antiga chave civil. A data nova vem do juiz em `:160`. |
| `app/chamados/services/acoes_chamado.py:388:montar_contexto_detalhe` | `timestamp__date=_d` — **E**, evidência física no detalhe. |
| `app/chamados/services/desvio_escala.py:61:medir_desvio` | `timestamp__date__gte=ini`, `timestamp__date__lte=hoje` — **P**, carga da amostra. |
| `app/chamados/services/desvio_escala.py:65:medir_desvio` | `t.date()` — **J**, agrupa a amostra para julgar desvio de escala. |
| `app/chamados/services/disputa_emissao.py:729,730:_orfao_resolvido_cross_meianoite` | `_tz.localtime(t.entrada.timestamp).date()`, `_tz.localtime(t.saida.timestamp).date()` — **J**, os dois dias civis entram na guarda que considera o órfão resolvido. |
| `app/chamados/services/disputa_emissao.py:1213:criar_disputa_a_partir_de_chamados_orfaos` | `_tz.localtime(batida_origem.timestamp).date()` — **J**, decide se há motivo para emitir. O `data_evento` emitido já consulta o juiz em `:1219`. |
| `app/chamados/services/explicador.py:77:_explicar_validada` | `_tz.localtime(ts).date()` — **E**, sobra sem uso de `_d0` no explicador. A janela efetiva é montada em `:78`. |
| `app/chamados/services/limbo_folgas.py:36:sugerir_folga` | `_tz.localtime(t).date()` — **J**, agrupa entradas para sugerir a folga. |
| `app/chamados/services/limbo_folgas.py:37:sugerir_folga` | `timestamp__date__gte=ini`, `timestamp__date__lte=hoje` — **P**, recorte da amostra da sugestão. |
| `app/chamados/services/limbo_folgas.py:158:semana_muda` | `timestamp__date__gte=ini`, `timestamp__date__lte=data` — **J**, existência de entrada na semana civil decide a guarda do furo. |
| `app/chamados/services/materializacao.py:448:_plantio_preenche_furo_na_data` | `loc.date()` — **J**, escolhe a célula que pode absolver o plantio quando inexiste pergunta. |
| `app/chamados/services/materializacao.py:469:_plantio_preenche_furo_na_data` | `loc.date()` — **P**, calcula o deslocamento físico do instante sobre o eixo da célula já escolhida. |
| `app/chamados/services/passivo_validado.py:118:_dia_do_plantio` · `:357:reparar` | `timezone.localdate(ts)` — **J** em `:118`, recuo para a chave de competência do plantio se falta `data_evento`. **E** em `:357`, contador contrafactual de divergência. |
| `app/chamados/services/passivo_validado.py:595,597:_plantar_o_dia` | `timezone.localdate(b.timestamp)` — **E** em `:595`, enumera datas no erro. **J** em `:597`, o raio civil pode abortar o plantio inteiro. |

### `app/colaboradores` — 10 [app/colaboradores/services/calendario.py:273] [app/colaboradores/services/pos_vinculo.py:41]

| Arquivo, linha e função | Expressão e classe |
|---|---|
| `app/colaboradores/management/commands/corrigir_escala_retroativa.py:96,97:Command._rodar` | `timestamp__date__gte=desde`, `timestamp__date__lte=ate`, `'timestamp__date'` — **E**, coluna de evidência “BATEU” que o humano lê na prévia. |
| `app/colaboradores/services/calendario.py:273:contexto_calendario` | `timezone.localdate(_b.timestamp)` — **E**, marcação retratada mostrada no dia do instante. |
| `app/colaboradores/services/detalhe.py:36,39,41:contexto_detalhe` | `timestamp__date__gte=geo_de`, `timestamp__date__lte=geo_ate` — ambos em `:36`, um em `:39` e um em `:41`. **E**, recorte das marcações de GPS no detalhe. |
| `app/colaboradores/services/pos_vinculo.py:41:fechar_adesao_com_batida` | `timestamp__date__gte=per[0]`, `timestamp__date__lte=per[1]` — **J**, existência na janela fecha a adesão. |

### `app/core` — 10 [app/core/cell.py:10] [app/core/views.py:194] [app/core/services/painel_op.py:102]

| Arquivo, linha e função | Expressão e classe |
|---|---|
| `app/core/cell.py:10,15:cell_ultimas_batidas` | `timestamp__date=hoje`, `batida__timestamp__date=hoje` — **E**, batidas e alertas de hoje. |
| `app/core/services/painel_op.py:102,269:montar_painel_operacional` | `batida__timestamp__date=hoje`, `timestamp__date=hoje` — **E**, alertas e presença no painel. |
| `app/core/views.py:194,200:dashboard` · `:784,793:painel_tv` · `:1167,1171:dashboard_dados` | `timestamp__date=hoje` em `:194,793,1167`, `batida__timestamp__date=hoje` em `:200,784,1171` — **E**, indicadores do dia de calendário. |

### `app/escala` — 17 [app/escala/utils.py:187] [app/escala/utils.py:1870]

| Arquivo, linha e função | Expressão e classe |
|---|---|
| `app/escala/management/commands/hub_smoke.py:93,95:Command.handle.win_map` | `timestamp__date__gte=ini`, `timestamp__date__lte=fim`, `ts.date()` — **E**, mapa civil bruto do ensaio. |
| `app/escala/regua_defesa.py:157:diagnostico_regua` · `:524:entradas_fora_do_inicio` | `_tz.localdate(b.timestamp)` — **J**, agrupa a régua e seleciona a primeira marca por dia. `:157` ainda pode recuar um dia por hora. Efeito de diagnóstico de cadastro. |
| `app/escala/services/furos_diarios.py:267,268:universo_empresa` | `timestamp__date__gte=_ini`, `timestamp__date__lte=_fim` — **J**, presença no recorte civil distingue `escala_x_real` de `sumiu`. |
| `app/escala/services/furos_diarios.py:281,335:universo_empresa` | `_tz.localtime(_ult[l['chave']]).date()`, `_tz.localtime(_ult[pk]).date()` — **E**, data da última marca mostrada. |
| `app/escala/utils.py:187:_eh_abertura` | `dt_local.date()` — **J**, escolhe os marcos usados para distinguir abertura de volta de intervalo. O reconciliador chama em `app/ponto/management/commands/reconciliar_fantasmas.py:108`. |
| `app/escala/utils.py:814:montar_grade_prevista` | `locs[_k][0].date()` — **E**, aloca sobra não reclamada na linha civil. |
| `app/escala/utils.py:1526,1546,1556,1569:montar_grade_prevista_periodo_por_turno` | `_l[0].date()`, `_locs_i[_j][0].date()`, `_locs_i[0][0].date()`, `_locs_i[_anc_ix][0].date()` — **P**, carrega e ancora a varredura posicional do intermitente. |
| `app/escala/utils.py:1580,1870:montar_grade_prevista_periodo_por_turno` | `_locs_i[k][0].date()` — **J**, escolhe as primeiras marcas da jornada intermitente. `locs[_k][0].date()` — **J**, pode encaixar sobra marcada como intervalo em slot da linha civil. [app/escala/utils.py:1874] |

### `app/folha` — 2 [app/folha/export.py:500] [app/folha/porta_export.py:544]

| Arquivo, linha e função | Expressão e classe |
|---|---|
| `app/folha/export.py:500:dias_ferias_com_batida` | `_tz.localdate(b.timestamp)` — **J, dinheiro**, interseção civil com férias pode reter entrada no TXT. [app/folha/export.py:791] [app/folha/export.py:883] |
| `app/folha/porta_export.py:544:medir` | `_p.entrada.date()` — **J, auditoria**, chave diária do invariante de HE e intrajornada. [app/folha/porta_export.py:541] [app/folha/porta_export.py:568] |

### `app/ponto` — 131 [app/ponto/turnos.py:48] [app/ponto/views.py:3411]

| Arquivo, linha e função | Expressão e classe |
|---|---|
| `app/ponto/calculador/nucleo.py:117,235:minutos_do_oraculo` | `t.date()` — **E**, contagem de batidas brutas civis. |
| `app/ponto/calculador/nucleo.py:141,237:minutos_do_oraculo` | `_e.date()`, `tu[0].date()` — **J**, agrupa minutos do oráculo quando o par veio sem dia, ou no caminho de pareamento próprio do oráculo. |
| `app/ponto/calculador/nucleo.py:180:minutos_do_oraculo._dentro_do_envelope` · `:223:minutos_do_oraculo` | `a.date()`, `b.date()` — **P**, busca envelopes candidatos. `cur[0].date()` — **E**, carimba corte diagnóstico do oráculo. |
| `app/ponto/janelas.py:84:competencia_fechada` | `_tz.localtime(timestamp).date()` — **J, guarda de dinheiro**, escolhe a competência da recusa de ajuste de batida. [app/ponto/registro_batida.py:159] [app/ponto/services/flip_batida.py:22] [app/chamados/services/materializacao.py:859] |
| `app/ponto/management/commands/desvio_o68b.py:112,121,136:Command._autopsia` | `_lt(x.entrada).date()` em `:112,136`, `dt.date()` em `:121` — **E**, faixa, feriado e dias trabalhados do laudo. |
| `app/ponto/management/commands/detectar_par_relampago.py:303:varrer_pares_embutidos` | `data_local(a.timestamp)` — **J**, usa o dia civil como `ref` e grupo de escolha do par retratável depois de consultar `par_completo`. |
| `app/ponto/management/commands/diff_calculador.py:244:Command.handle._dia_do` | `_l.date()` — **J**, atribui dia ao contrafactual do DIFF se faltam mapa e dia explícito. |
| `app/ponto/management/commands/e6_piso_autopsia.py:91:Command.handle` | `tu[0].date()` — **J**, chave de minutos do pareamento do instrumento. |
| `app/ponto/management/commands/emitir_furo_retroativo.py:75,100:Command.handle` | `_tz.localdate(_p.timestamp)`, `_tz.localdate(b.timestamp)` — **J**, piso de adesão e contagem civil na emissão de furo. |
| `app/ponto/management/commands/flip_automatico.py:64:Command.handle` | `timezone.localdate(b.timestamp)` — **J**, reancora o candidato no dia civil no caminho do flip. |
| `app/ponto/management/commands/mapa_divergencia.py:62,63:Command.handle` | `timestamp__date__gte=_emp_ini - datetime.timedelta(days=2)`, `timestamp__date__lte=_emp_fim + datetime.timedelta(days=2)` — **P**, amostra alargada da sonda. |
| `app/ponto/management/commands/marcar_abandono_provisorio.py:56:Command.handle` | `timezone.localtime(ult_e.timestamp).date()` — **J**, dia civil da última entrada na decisão de abandono. |
| `app/ponto/management/commands/plano_b_no_dinheiro.py:88:Command.handle._espiao._cm_espiao` | `_lt(_e).date()` — **E**, recuo para selecionar períodos no laudo de um dia. |
| `app/ponto/management/commands/reconciliar_fantasmas.py:66:_parse_pendentes` | `datetime.datetime.strptime(part, '%d/%m/%Y %H:%M').date()` — **J**, reconstitui datas das marcas pendentes a partir de texto histórico. |
| `app/ponto/management/commands/reconciliar_grade.py:85,90:Command.handle` | `timestamp__date__range=(win0, ref)` — **P**, seleção inicial do universo. `timezone.localtime(row['p']).date()` — **J**, fixa o piso de adesão. |
| `app/ponto/management/commands/supra_juiz.py:135,222:Command.handle` | `_tz.localdate(_p.timestamp)`, `_tz.localdate(b.timestamp)` — **J**, adesão e contagem civil levadas ao veredito. |
| `app/ponto/management/commands/testar_pwa.py:169,179,195,200:Command.t05_batida_entrada` · `:214:Command.t06_batida_saida` · `:293:Command.t11_batida_com_gps` | `timestamp__date=hoje` nos cinco primeiros, `batida__timestamp__date=hoje` em `:293` — **P** em `:169`, recorte de limpeza do ensaio, **E** nos demais, observação do ensaio. |
| `app/ponto/management/commands/vigia_plantio_overnight.py:86:batidas_fora_do_turno_da_pergunta` | `data_local(b.timestamp)` — **P**, abre a janela do mapa do juiz em `:87`. |
| `app/ponto/propositor.py:363,364:dia_posicional_completo` | `timestamp__date__in=viz` — **P**, carrega vizinhança. `_tzp.localdate(b.timestamp)` — **J**, escolhe as marcas do dia para declarar completude posicional. |
| `app/ponto/registro_batida.py:28:_janela_oposto` · `:43:_casa_marco_proprio` | `_tz.localtime(timestamp).date()` — **J**, escolhe marcos e janela de colisão na porta de escrita. `ts_l.date()` — **P**, enumera dia e véspera para testar o marco por instante. [app/ponto/registro_batida.py:94] |
| `app/ponto/services/adesao.py:45:data_de_adesao` | `(tz or timezone).localdate(b.timestamp)` — **J**, dia civil da primeira marca viva. Chamador de produção desta função **[nao verificado]**. |
| `app/ponto/services/cartorio.py:1236,1393:_julgar_colab_corpo` | `_tz.localdate(_p.timestamp)` — **J**, piso de adesão que entra no veredito. `_tz.localdate(b.timestamp)` — **P**, pad de ±1 dia da célula. [app/ponto/services/cartorio.py:1511] |
| `app/ponto/services/credito_parcial.py:36:dia_do_periodo` | `timezone.localtime(p.entrada).date()` — **J, dinheiro**, agrupa atraso para compensar crédito no próprio dia. [app/ponto/services/credito_parcial.py:39] |
| `app/ponto/services/dia_pago.py:132:_dia_de` | `timezone.localtime(p.entrada).date()` — **J, dinheiro**, recuo sem dia no mapa para a chave da lavra diária. [app/ponto/services/dia_pago.py:128] [app/ponto/services/dia_pago.py:173] |
| `app/ponto/services/dias_em_aberto.py:123:passivo_da_competencia` | `timezone.localtime(ts[-1]).date()` — **J**, última marca decide classe de passivo. [app/ponto/services/dias_em_aberto.py:130] |
| `app/ponto/services/entrada_sincronizada.py:27:resolver_cobranca_da_entrada` | `timezone.

[truncado em 20000 de 42642 caracteres]

## 26/11 00:39 · sessao 60811e18da84 · RAZAO-DA-SOBRA · leitura · origem=Code

# RAZAO-DA-SOBRA — contrato + matriz RED

**O contrato é uma razão por ocorrência que restou na grade e entrou na ata, com identidade e instante preservados desde a origem.** A grade já decide quais ocorrências sobram e em qual linha pousam. O cartório já lavra a lista e a contagem, hoje em `HH:MM`, sem razão. A leitura é da cópia indicada como `c314c099f3f447dbb628557c9ada9906183f1182`. A identidade do hash por Git, o conteúdo integral do aval de 26/11 e a execução dos REDs [nao verificado]. `app/docs/BACKLOG.md:2079,2189` · `app/docs/PROMPTS.md:3002` · `app/escala/utils.py:1857–1891` · `app/ponto/services/cartorio.py:749–765`

## 1. Contrato de entrada

| Item | De onde sai hoje | Unidade e perda |
|---|---|---|
| **Fonte** | `montar_grade_prevista_periodo` despacha para o builder por turno por padrão, com caminho legado. `_match_marcos` produz sobras no próprio casamento. O builder também produz sobras de ponta de override fora da régua e de intervalo sem slot `I`, mesmo quando a batida já foi consumida. Por fim, a conservação S133 recolhe as não consumidas. O cartório recebe a lista final. | O mapa descreve corretamente a conservação **residual**, mas ela é um dos ramos da fonte. A sobra do casamento e a sobra sem coluna entram na **linha do turno**. A residual entra pela **data civil local**. `app/config/settings/base.py:165` · `app/escala/utils.py:630–681,808–828,1747–1782,1806–1808,1857–1891,2138–2144` |
| **Unidade** | A `Batida` tem `timestamp` e identidade de registro. `locs` retém instante local, tipo e flag de intervalo. A saída da grade reduz a ocorrência a `HH:MM`. A direção ainda circula por ocorrência, em memória, para a cobertura parcial. | O **PK já sai na montagem de `locs`**. A **data do instante, os segundos e o instante completo** saem no `strftime`. A ata mantém duas strings iguais e conta duas ocorrências. O `dict` de `classificar_orfas` colapsa a chave repetida. A reconciliação de ponta viva remove a primeira ocorrência de `HH:MM` igual. O contrato precisa carregar **ocorrência identificada + instante + linha dona da grade**, inclusive nessa remoção. `app/ponto/models.py:23` · `app/escala/utils.py:699–706,1177–1192,1274–1279,1367–1385,1883–1887,2093–2094` · `app/ponto/services/cartorio.py:761–764` · `app/escala/orfa_classe.py:53–77` |
| **Universo** | Por colaborador e célula julgada, o cartório busca batidas apuráveis de `D0−1` até antes de `D1+3`, monta as linhas `D0…D1+1` e toma a linha da célula. A grade trabalha com `E` e `S`, pelo vínculo e pelos marcos do dia. O total da razão é o total das **ocorrências finais em `ata.orfas`**. | A janela de entrada dá contexto ao pareamento. O dia do turno e a data civil da marcação podem divergir da linha dona da sobra. A raia registrou que se lê a atribuição da grade. O cartório já usa a multiplicidade da lista no julgamento. `app/ponto/services/cartorio.py:1187–1210,1451–1461` · `app/escala/utils.py:1322–1328,1355–1368` · `app/escala/models.py:1109` · `app/docs/RELATO.md:2154` |
| **Exclusões** | Retratada sai da apuração. Pendente de DP entra. `_dedup_dez_min` descarta a marca do **mesmo tipo que a última sobrevivente**, com diferença estritamente menor que **10 minutos**. Marca acomodada em slot ou reconciliada com uma ponta sai da lista de sobras. Folga lançada, falta aprovada, ausência integral e justificativa aprovada têm ramos que zeram a lista. A parcial pode preservá-la. | A exclusão ocorre antes da razão, na entrada ou na lista final da grade. **Folga do ciclo pode conservar uma sobra.** O corte de futuro e pré-piso pertence à exibição da janela. A marcação crua tem seu próprio universo de exibição pela L-025. `app/docs/LEIS.md:76` · `app/ponto/turnos.py:2921–2948` · `app/escala/utils.py:972–987,1170–1174,1252–1304,1371–1385,1851–1888,2093–2094,2377–2393` |

## 2. Porta única pela lei herdada

**Juiz proposto: `razao_da_sobra`.** O aval registra a junção entre a sobra da ata e o testemunho de `parear_turnos`, com precedência **dia sem trabalho → rajada → pareador**. `classificar_orfas(dia)` tem zero chamadores Python de produção na cópia. Os usos vivos de seu módulo importam a constante de 15 min. Pela L-111, o corpo antigo é incorporado ou retirado. O `RELATO` registra a retirada na raia `109a2ba41`, seguida da leitura do carimbo do pareador e da ligação por ocorrência. O código desse commit [nao verificado]. `CLAUDE.md:17–27,177–179` · `app/docs/LEIS.md:60,95,137` · `app/escala/orfa_classe.py:53–77` · `app/escala/utils.py:504` · `app/ponto/turnos.py:1581` · `app/docs/BACKLOG.md:2189` · `app/docs/RELATO.md:2147–2148,2123–2125`

**Quem escreve e quem lê.** A grade entrega a ocorrência final e sua linha. O cartório é o escritor da razão na ata, pela lavra existente de `CelulaDia`. O supra-juiz e os emissores que atuam no julgamento recebem a mesma razão na ata desse ato, antes da persistência. Depois, `leitor_celula` é a porta para tela, PDF, chamados e causas da folha lerem a razão lavrada. A ata anterior ao horizonte segue como foto pela L-124. Hoje o leitor legado pode ter só a contagem e devolver `?`. O registro do nascimento do juiz é uma pendência documental identificada no mapa. A trava consulta `CORTES.md`, gerado de `CORTES.json`. `app/escala/utils.py:2093–2125` · `app/ponto/services/cartorio.py:1470–1475,1506–1517,1906–1914` · `app/ponto/portas/celula.py:1069–1078,1127–1135` · `app/escala/services/leitor_celula.py:180–190,420` · `app/docs/LEIS.md:145` · `app/docs/RELATO.md:81,2022` · `bin/tests/test_juiz_novo_tem_corte.sh:37–44` · `bin/gerar_cortes.py:23–24`

| Razão | Lei ou sítio que já responde |
|---|---|
| `JORNADA_ADICIONAL` | O aval manda ler o **novo turno fechado que o pareador efetivamente formou** depois da saída final. A E abre e a S fecha, pelas guardas desse juiz. Uma E avulsa pode ficar aberta. A razão não cria HE. `app/docs/RELATO.md:2348,2358` · `app/ponto/turnos.py:1402–1421,1491–1502` · `app/docs/LEIS.md:115` |
| `NAO_MEDIDA_ORIGEM_PAUSA` | O aval manda ler `PAR_DE_PAUSA_DEDUZIDO` pelo pareador e o envelope de 90 min da L-032. A dedução usa `intra_max_s`, cujo padrão é `TETO_PAUSA_SEGUNDOS`. `_carimbada_como_miolo` reúne os três carimbos, inclusive `_intra_ata`, mas o carimbo isolado também pode vir de eco ou de outro ramo. O esperado é ler o reconhecimento da pausa, sem inferir a origem a partir de uma flag nem usar o piso de intervalo gozado para julgar a geometria. `app/docs/LEIS.md:80` · `app/docs/RELATO.md:2147–2148,2271,2300,2306,2358` · `app/ponto/turnos.py:377–417,453–457,707–718,932–940,1356–1376,1711–1745` |
| `PERIODO_FORA_ESCALA_CADASTRO` | O aval manda ler o **dia presente em `resultado.dias_cadastro_x_realidade`**. A L-084 decide o caso das **duas** pontas a mais de 180 min. O mapa erra o endereço da lista de produção. `periodo.py:283` documenta `Censo.cxr`, enquanto o motor publica `resultado.dias_cadastro_x_realidade` e o oráculo lê esse resultado. A lista de produção também recebe causas da janela de HE. A colisão com uma razão do pareador fica na **Pergunta 5**. `app/docs/LEIS.md:116` · `app/docs/RELATO.md:2358` · `app/ponto/calculador/periodo.py:283–288` · `app/ponto/motor_calculo_v2.py:1628–1642,1669–1686,1725–1727,1763–1765,1946–1965,2735` · `app/ponto/management/commands/e6_oraculo.py:162–171` |
| `TURNO_ABERTO` | A ocorrência é a entrada de um turno que continuou sem saída **depois** das passagens finais do pareador. `app/ponto/turnos.py:312–324,1497–1502` · `app/docs/RELATO.md:2300,2358` |
| `SAIDA_SEM_ENTRADA` | O pareador entrega uma saída com `entrada is None`, inclusive no ramo que recusa um par degenerado. `datas_saida_orfa` já lê essa forma. `app/ponto/turnos.py:234–245,1482–1502` · `app/docs/RELATO.md:2300,2358` |
| `MINUTO_SEM_BATIDA` | Zero correspondência na ligação da **sonda por minuto**. A raia registra `SEM_BATIDA` como falha contável em `sobras_sem_razao`. **Pergunta 3.** `app/docs/RELATO.md:2003,2147,2306` |
| `MINUTO_AMBIGUO` | Múltiplas candidatas na ligação por minuto. O código vivo já se abstém de inferir papel quando o minuto é ambíguo. A raia tratou como bloqueio transformar duas sobras identificáveis em `BATIDA_AMBIGUA`. **Pergunta 4.** `app/ponto/turnos.py:1604–1617,1734–1747` · `app/docs/RELATO.md:2131,2147,2306` |
| `FORA_DA_ESCALA_SEM_ENQUADRAMENTO` | O residual vem do aval 2320, depois de ler as autoridades existentes. `e6_oraculo.dono_da_divergencia` responde **quem é o dono do dia divergente**, em outra pergunta. `app/docs/RELATO.md:2358` · `app/ponto/management/commands/e6_oraculo.py:53–70,98–110` |

## 3. Matriz RED

`D` é a linha em exame. Os horários das fixtures novas são sintéticos. As linhas indicam o resultado a cobrar pela regra e o ponto da mordida. Sua execução nesta cópia [nao verificado]. O caso de duas pontas vem de uma fixture existente. `app/docs/LEIS.md:154` · `app/ponto/tests/test_bateria_lavratura.py:479–483,625–647`

| Caso | Cadastro e batidas locais | Esperado pela regra e onde o RED morde |
|---|---|---|
| **Duas sobras no mesmo minuto** | Trabalho, 6x1 `09:00–17:00`, sem pausa cadastrada. `E 06:40:04`, `S 06:40:42`. | **Duas ocorrências**, com segundos e razões endereçáveis separadamente. Fora do envelope da grade, ambas sobram e os tipos opostos passam pelo dedup. O pareador recusa o par de 38 s e dá os insumos `TURNO_ABERTO` para E e `SAIDA_SEM_ENTRADA` para S. O rótulo final fica condicionado à precedência de rajada da **P2**. Morde a chave única do classificador, a perda de instante na ata e qualquer produção de `MINUTO_AMBIGUO` só pela coincidência visual. `app/escala/utils.py:638–644,1367–1385,1864–1891` · `app/ponto/turnos.py:117–131,1487–1502` · `app/escala/orfa_classe.py:73–77` · `app/ponto/services/cartorio.py:761–764` · `app/docs/RELATO.md:2131,2123–2125` |
| **Sobra cross-meia-noite** | 6x1 noturno `21:00–06:00`, D e D+1 de trabalho com DNA, sem batidas vizinhas. Só `S D+1 09:00:10`. | `SAIDA_SEM_ENTRADA`, com a **sobra na linha D+1** da conservação e o **turno em D** pelo envelope da véspera. Morde o leitor que usa igualdade de dias para ligar a ocorrência ou chama a linha D+1 de dia do turno. O supra atual usa a lista da linha recebida e pode acusar o genérico `BATIDA_ORFA_FORA_TOLERANCIA` ali. Controle da outra origem: no teste 12x36 `19:00–07:00`, `E D 19:00:00`, `E D+1 06:54:26`, `S D+1 06:55:54`, a sobra `06:54` já vem **na linha D**. `app/docs/LEIS.md:117` · `app/ponto/turnos.py:66–82,216–231,285–298,1253–1264,1295–1302,1495–1498` · `app/escala/utils.py:638–644,1864–1887` · `app/ponto/supra_juiz.py:523–537` · `app/escala/tests/test_sobra_na_borda_nao_vira_intervalo.py:214–217,233–246` |
| **Rajada de até 15 min** | Trabalho, 6x1 `09:00–17:00`, sem pausa cadastrada. `S 06:25:00`, `S 06:36:00`, marcos apagados. | Duas sobras do mesmo tipo a 11 min sobrevivem ao dedup e satisfazem a assinatura atual de rajada. O rótulo canônico é **P2**. Morde o vigia se ele voltar a calcular a classe por `HH:MM`. O pareador fornece duas saídas sem entrada e o classificador antigo responde `ECO_CLUSTER`. `app/escala/utils.py:1367–1385,1864–1887` · `app/ponto/turnos.py:1495–1498` · `app/escala/orfa_classe.py:40–49,73–77` · `app/ponto/services/cluster_espurio.py:15–37` |
| **Intervalo curto S→E, dentro da L-032** | Trabalho, `08:00–17:00`, pausa cadastrada `12:38–13:38`. `E 08:00:00`, `S 12:50:10`, `E 13:04:20`, `S 17:00:00`. Usar como fixture da porta com a volta na lista final de sobras. | `NAO_MEDIDA_ORIGEM_PAUSA` para a ocorrência que sobrou e que o pareador reconheceu no intervalo. A L-032 preserva essa natureza dentro do envelope. Morde a reconstrução que dá `GEOMETRIA` para a sobra isolada ou `ECO_CLUSTER` ao receber as duas pontas com marco apagado. Nos sete casos registrados a ata tinha **uma** sobra por dia e o resultado reconstruído foi `GEOMETRIA`. Os segundos originais [nao verificado]. `app/docs/LEIS.md:80` · `app/ponto/turnos.py:1356–1376` · `app/escala/orfa_classe.py:70–77` · `app/docs/CORTES.md:592,600` · `app/docs/RELATO.md:2340` |
| **Folga com batida** | 12x36 `19:00–07:00`, folga do ciclo, sem folga lançada. `E 14:03:21`, `S 15:03:39`, fora dos turnos vizinhos. | Duas ocorrências na ata, com o ramo **dia sem trabalho** antes da geometria. O classificador disponível dá `FOLGA_DESMENTIDA`. A escolha do rótulo final é **P1**. Morde a lavra se a folga desaparecer do total ou se o leitor tomar o recorte de `FuroDiario` como total da ata. `app/escala/utils.py:1170–1174,1851–1887` · `app/chamados/juizes.py:490–505` · `app/escala/orfa_classe.py:62–66` · `app/escala/services/furos_diarios.py:43–57` · `app/docs/RELATO.md:2338` |
| **Turno aberto** | Trabalho, 6x1 `09:00–17:00`, sem pausa. Só `E 06:40:04`, sem S posterior. | `TURNO_ABERTO` para a E fora do envelope. Morde a ata genérica de `HH:MM` e o supra, que no dia previsto com realizado zero usa `BATIDA_ORFA_FORA_TOLERANCIA`. `app/escala/utils.py:638–644,1864–1887` · `app/ponto/turnos.py:312–324,1402–1420,1497–1502` · `app/ponto/services/cartorio.py:761–764` · `app/ponto/supra_juiz.py:527–537` |
| **Saída sem entrada** | Mesmo cadastro. Só `S 06:40:42`, sem turno da véspera que a reivindique. | `SAIDA_SEM_ENTRADA`. Morde o mesmo rótulo genérico do supra, que hoje não distingue essa saída da E do caso anterior. `app/ponto/turnos.py:234–245,1495–1502` · `app/ponto/supra_juiz.py:527–537` |
| **Oposta a 10 s** | Mesmo cadastro. `E 06:40:55`, `S 06:41:05`. Controle: trocar a segunda por `E 06:41:05`. | O par de tipos opostos passa inteiro pelo dedup. No controle a segunda E cai **antes** da conservação. Com os opostos, o pareador recusa o par de 10 s e testemunha E aberta e S sem entrada. A classificação final de rajada é **P2**. Morde a perda da ocorrência na lavra ou a promoção da proximidade a razão por conta do leitor. O dedup de opostas aqui é **controle verde**. `app/escala/utils.py:1367–1385` · `app/ponto/turnos.py:117–131,1482–1502` · `app/ponto/management/commands/detectar_par_relampago.py:49` · `app/ponto/services/cartorio.py:761–764` · `app/docs/BACKLOG.md:2561` |
| **Jornada adicional** | Trabalho, 6x1 `09:00–17:00`, sem pausa cadastrada. `E 09:00:03`, `S 17:00:08`, `E 18:40:10`, `S 19:10:20`. | `JORNADA_ADICIONAL` nas duas sobras do segundo par fechado, com a primeira S na borda de saída e as duas extras fora do envelope da grade. Morde a ata sem razões e a reconstrução `INDEFINIDA` quando os marcos do dia estão acesos. `app/docs/RELATO.md:2358` · `app/ponto/turnos.py:429–457,1491–1502` · `app/escala/utils.py:638–644` · `app/ponto/services/cartorio.py:761–764` · `app/escala/orfa_classe.py:68–71` |
| **Cadastro fora da realidade** | Fixture 5x2, trabalho `08:00–17:00`, sem pausa. `E 03:10:00`, `S 22:50:00`. | Dois `PERIODO_FORA_ESCALA_CADASTRO` ao ler o dia acusado pelo motor pela L-084. O par fechado tem as duas pontas fora, e a fixture já cobra que ambas permaneçam na ata. Morde a falta de razão junto aos horários e ao protesto diário `FURO_PARCIAL` + `REALIZADO_INFLADO`. `app/docs/LEIS.md:116` · `app/ponto/motor_calculo_v2.py:1946–1965,2735` · `app/ponto/tests/test_bateria_lavratura.py:562–594,628–657` |

**Propriedades fixas do RED**

- Para cada ata no alcance da lavra nova, **cada ocorrência tem exatamente uma razão** e **a soma das contagens por razão é igual a `n_orfas`**, incluindo horários repetidos. Uma ocorrência que saiu da lista por reconciliação sai também da contagem por razão. As falhas `SEM_BATIDA` e `BATIDA_AMBIGUA` ficam visíveis na verificação até a decisão das P3 e P4. `app/ponto/services/cartorio.py:761–764` · `app/escala/utils.py:2093–2094` · `app/docs/LEIS.md:145,154` · `app/docs/RELATO.md:2131,2147`
- Para a mesma ocorrência e a mesma lavra, **tela, PDF, supra-juiz, chamado e causas da folha recebem a mesma razão**. O RED compara a razão entregue a cada consumidor, mesmo quando seus resultados têm unidades diferentes, como um protesto diário ou uma causa mensal. `app/docs/LEIS.md:60,95,154` · `app/ponto/services/cartorio.py:1470–1475,1506–1517` · `app/folha/causas.py:39–45,74–108`
- As **1.307** sobras do anexo fecham na soma da sonda de 19/11, com zero residual reportado. A origem das 453 pausas ficou incompleta. Essa soma verifica o universo daquela sonda por minuto. A propriedade por identidade e a concordância entre leitores [nao verificado]. `app/docs/RELATO.md:2299–2300,2306`

## 4. Quem mais lê

| Ponto do mapa | Migra para a porta ou é exibição |
|---|---|
| **1. Dia — supra-juiz e vigia do pareador** | **Migram** o acesso à ocorrência e à sua linha dona. O supra lê a lista da linha que recebeu e tem um caminho por `batidas_no_dia` quando falta a chave. O vigia recompõe a busca por data civil e `HH:MM`. Ele conserva o cotejo borda do pareador × lâmpada apagada da ata. O pareador continua fornecendo geometria e seu próprio `data_turno`. **EXIBIÇÃO** de marcação usa `dia_das_batidas`, que já lê por PK, com dia do turno para a marca reclamada e data local para a avulsa. `app/ponto/supra_juiz.py:522–537` · `app/ponto/services/vigia_pareador_celula.py:67–78,97–112` · `app/ponto/turnos.py:2951–2974,3010–3021` |
| **2. Instante — tela e PDF** | **Migram** os casamentos por `HH:MM`, a anotação `b.orfa` e a guarda de turno aberto do PDF. O calendário transforma a ata em `set` por dia/mês. O espelho tem origem híbrida na janela. O PDF remonta a grade e, para suprimir turno aberto, usa a união de `HH:MM` de todos os dias. **EXIBIÇÃO** lê o cru para mostrar a marcação, inclusive a retratada, pela L-025. Horário e estilo visual são projeções. O juiz da batida já separa sobras dos períodos. `app/colaboradores/services/calendario.py:361–363,409–410,652–660` · `app/ponto/services/espelho.py:498–550,703–704,862` · `app/relatorios/pdf_espelho.py:472–477,504–515,539–552,607–609,873–886` · `app/ponto/juiz_batida.py:18,161–168` · `app/docs/LEIS.md:76` |
| **3. Quinze minutos — grade, papel e cluster** | **Migra** a assinatura de rajada refeita por `cluster_espurio`, na parte que responde a mesma pergunta sobre a sobra, conforme P2. A abertura e o fecho de chamado conservam seus recortes. A guarda de atribuição de marco tem o corte D1 vigente. O mapa menciona três leituras, mas há também **sobra perto de uma luz** em `papel_por_minuto_da_ata`, que alimenta o pareador. A guarda de marco, o papel do pareador e a razão final são perguntas distintas. `app/escala/orfa_classe.py:40–49` · `app/escala/utils.py:502–508,654–666` · `app/ponto/turnos.py:1711–1747,1804–1806` · `app/ponto/services/cluster_espurio.py:15–37,53–77,153–173` · `app/docs/CORTES.md:600` |
| **4. Quatro pares nominais** | **Tela × PDF**, col42 22/10, 0 × 1, é divergência na apresentação da sobra. **Ata × furos**, col43 02/11, 2 × fora do universo de folga, é diferença de recorte. **Ata × cluster**, col42 22/10, 1 × sem assinatura, também é compatível com o predicado atual de duas sobras e lâmpada apagada. **Pareador × ata**, col81 01/11, é divergência borda de entrada × sobra. Migram a identidade e a razão comuns. Cada contagem mantém seu universo declarado. Frequência de cada par [nao verificado]. `app/docs/RELATO.md:2300,2338` · `app/escala/services/furos_diarios.py:25–57` · `app/ponto/services/cluster_espurio.py:15–37` · `app/ponto/services/vigia_pareador_celula.py:97–112` |

**Chamado e folha.** `_celula_furada` migra a leitura de sobra e preserva a guarda de cadastro. Materialização migra a entrada de marcas identificadas e continua perguntando se o plantio preenche o furo. Causas da folha passam a receber a razão da ocorrência, mantendo a causa mensal como sua própria decisão. O export lê a retenção e os protestos lavrados. A matriz não atribui um novo efeito de cobrança, retenção ou pagamento a nenhum rótulo. `app/chamados/juizes.py:554–566` · `app/chamados/services/materializacao.py:447–475` · `app/folha/causas.py:43,58–83,93–108` · `app/ponto/services/fechamento.py:1090–1106` · `app/folha/export.py:968–973,1273–1286`

**Outros consumidores do mapa.** `status_dia`, `bordas_realizado`, `simular_dia`, `propositor`, `esmeril_espelho`, `tripwire_celulas`, `detectar_par_relampago` e `p

[truncado em 20000 de 26187 caracteres]

## 26/11 00:37 · sessao b4c622c272a3 · - · - · origem=Ronald (tmux)

- O instrumento AST da cópia reportou **780 arquivos, 0 ilegíveis, A1=6, A2=15, B1=78, B2=191**. São 81 parcelas de derivação em 50 arquivos e 133 linhas registradas, incluídas três parcelas nomeadas. A seleção lexical B1 tem falsos positivos e exclui a composição do juiz. `app/core/medida_turno_do_par.py:197,336,392,426,448` · `app/core/tests/test_medida_turno_do_par.py:108,250`.
- A varredura independente encontrou **57 folhas AST de lookup ORM**, com 56 keywords e 1 string de projeção. Nenhum `TruncDate`, `Cast`, `.dates()` ou `.datetimes()` da batida apareceu nessa busca. `app/ponto/tests/test_contract_no_batida_date.py:156,237` · `app/colaboradores/management/commands/corrigir_escala_retroativa.py:97`.
- O selo proíbe novos **arquivos** ofensores e cobra entradas podres. O visitante de `.date()` perde `localdate()` terminal e apelidos. A catraca do motor reconhece apelidos e `data_local`, conta por função e deixa lookups fora. `app/ponto/tests/test_contract_no_batida_date.py:178,196,231,246,265` · `app/core/medida_turno_do_par.py:48,51,197`.
- **Dinheiro direto**: `fechamento.py:224` recorta com pad e também seleciona a fatia do vínculo pela data civil (`:273,280`), `:813,859` decidem os dias e minutos órfãos que entram no total; `:1593` define a competência recalculada pela escrita da batida. `dia_pago.py:132` apenas quando falta mapa, com efeito na distribuição da lavra diária. `credito_parcial.py:36` agrupa atraso para compensar crédito no próprio dia. `folha/export.py:500` decide a retenção do TXT por férias. `folha/porta_export.py:544` é auditoria de HE/intrajornada, sem comprovação de lançamento financeiro. `app/ponto/services/fechamento.py:224,273,280,813,859,1593,1619` · `app/ponto/registro_batida.py:174,180,195` · `app/ponto/services/dia_pago.py:128,132,173` · `app/ponto/services/credito_parcial.py:36,39,50` · `app/folha/export.py:500,791,883` · `app/folha/porta_export.py:544`.
- **Motor inviolável**: EP `:470` alimenta marcos/ata com pad e `:97,98` constrói faixas de hora noturna em cada dia civil. JP `:578` escolhe a célula por data civil; `:686,687,951,2419,2595,2961,2974,3346` são fallbacks que priorizam o mapa. `:2424` é equivalente sem `.date()` e sobrescreve o dia pelo gap. `:2069,2109,2134,2214,2216,2226` escolhem o dia de marcos de pontualidade; `:2655,2659,2660,3485,3510,3514,3515` decidem DSR/reflexo por data civil, `:3467` decide banco. `app/ponto/motor_calculo_v2.py:97,470,578,611,686,951,2069,2226,2419,2424,2595,2655,2961,3346,3467,3485,3510`.
- **Estado/escrita**: cartório `:1236` usa data civil de adesão, `:1393` faz pad EP; sinais `:23,111` fazem pad EP; `ponto/views.py:1629::_abrir_justificativa_fluxo` usa `timestamp__date=data_turno` para escolher uma entrada que grava na justificativa; `pos_vinculo.py:41` decide fechar adesão por existência no período. `limbo_folgas.py:36` usa dias civis para sugestão de cadastro e `:158` decide calar furo pela presença na semana civil. `ponto/janelas.py:84` escolhe guarda para ajuste de competência chamado por registro, flip e materialização. `app/ponto/services/cartorio.py:1236,1393` · `app/ponto/signals.py:23,111` · `app/ponto/views.py:1629,1657,1666` · `app/colaboradores/services/pos_vinculo.py:41` · `app/chamados/services/limbo_folgas.py:36,158` · `app/ponto/janelas.py:84` · `app/ponto/registro_batida.py:159` · `app/ponto/services/flip_batida.py:22` · `app/chamados/services/materializacao.py:859`.
- **Equivalentes fáceis de perder**: `strftime('%Y-%m-%d')` em `utils_turno_orfao.py:39` e `triagem_batida.py:185,186` são EX de turno aberto, enquanto `triagem_batida.py:247,254,260` escolhe saída informada/chave de justificativa. Núcleo do oráculo `:117,235` conta batidas civis brutas, `:141,237` atribui dia à jornada na ausência de resposta pronta, `:180` busca envelope, `:223` marca corte diagnóstico. `app/ponto/utils_turno_orfao.py:39,44` · `app/ponto/services/triagem_batida.py:185,247,254,260` · `app/ponto/calculador/nucleo.py:117,141,180,223,235,237`.

## 26/11 00:37 · sessao 059e9c0e9d60 · - · - · origem=Ronald (tmux)

- `razao_da_sobra` é a porta proposta. A cópia tem `classificar_orfas` com quatro classes e saída por `HH:MM`, sem chamador de produção conforme o registro. A raia `109a2ba41` extinguiu a função antiga, conforme a L-111. O resultado do cartório deve ser lido pela tela, PDF, supra, chamado e folha pela Lei-Akita 2 e L-061. A frase do juiz falta em CORTES segundo o registro. A trava procura a frase no Markdown, e o processo vigente gera esse Markdown do JSON. `app/escala/orfa_classe.py:53–77`, `app/docs/RELATO.md:81,2147–2148,2359`, `app/docs/BACKLOG.md:2189`, `app/docs/LEIS.md:60,95,137`, `bin/tests/test_juiz_novo_tem_corte.sh:37–44`, `app/docs/CORTES.md:570`.

- `JORNADA_ADICIONAL` lê o segundo par fechado que a autoridade realmente montou. Só uma E após a saída final pode abrir turno, mas o estado final sem S é `TURNO_ABERTO`; `SAIDA_SEM_ENTRADA` provém da estrutura com `entrada=None`, inclusive a S que teve recusado um par de até 60 s. Nenhum intervalo próprio entre turnos foi autorizado. `app/docs/RELATO.md:2348,2358`, `app/ponto/turnos.py:117–131,312–324,1402–1421,1487–1502`.

- `NAO_MEDIDA_ORIGEM_PAUSA` lê o carimbo que a geometria já deu ao miolo, incluindo `_intra_ata`. As duas durações de 15 min são perguntas diferentes. Uma pausa menor que 15 min pode colar o turno sem contar como intervalo gozado. O envelope cadastrado L-032 tem 90 min, exige que o marco de pausa prevaleça sobre a borda, e a duração deduzida tem o `intra_max_s` do pareador. `_intra_dur` não distingue S→E de S,S. Suas suborigens por ocorrência no dado apresentado são [nao verificado]. `app/docs/LEIS.md:80`, `app/ponto/turnos.py:382–486,495–546,932–940,1356–1375,1761–1786`, `app/docs/RELATO.md:2147–2148,2290`.

- `PERIODO_FORA_ESCALA_CADASTRO` lê a publicação real do motor. `app/ponto/calculador/periodo.py:283–288` é a lista separada `Censo.cxr`, contrariando o mapa. O motor exige duas pontas estritamente além de 180 min para a L-084, mas publica na mesma lista acusações de uma ponta fora da janela de HE. Pergunta de lei se todo dia publicado ganha a razão ou apenas acusações da L-084. A lavra disponível distingue lista vazia de competência sem cobertura. `app/docs/LEIS.md:116`, `app/docs/RELATO.md:2358`, `app/ponto/motor_calculo_v2.py:1636–1642,1725–1727,1763–1765,1947–1965,1988–2021,2735`, `app/ponto/services/cxr_na_lista.py:165–184`.

- `MINUTO_SEM_BATIDA` e `MINUTO_AMBIGUO` vêm da ligação técnica da sonda por `HH:MM`. A raia tratou o reencontro por minuto que fabricava ambiguidade com duas sobras reais como bloqueio. Pergunta de lei sob o requisito de taxonomia fechada: razão por ocorrência legada ou diagnóstico de medição fora das razões, com o legado explicitamente desconhecido. `FORA_DA_ESCALA_SEM_ENQUADRAMENTO` é o resto no registro do aval; `dono_da_divergencia` responde ao dono do dia, não à razão de uma batida. `app/docs/RELATO.md:2131,2147,2306,2358`, `app/ponto/management/commands/e6_oraculo.py:53–55,98–110`.

- A precedência herdada põe dia sem trabalho e rajada antes do pareador, mas `FOLGA_DESMENTIDA` e a classe de eco não estão nas oito razões pedidas. Para folga, a candidata específica é `FOLGA_DESMENTIDA`, contra o residual da lista fechada. O juiz existente de cobrança preserva a exceção de ausência parcial e não toma limbo por folga. Para eco, ainda há dois operandos, duas sobras consecutivas ou uma sobra perto de uma luz. A guarda Cirineu é outra pergunta, de ocupação de marco, pelo corte vigente. O relato aponta que o curto S→E aceito pela L-032 produz uma sobra e `GEOMETRIA` na ata em cada um dos sete casos, enquanto `ECO_CLUSTER` seria a saída hipotética com as duas sobras e marco faltante. `app/docs/BACKLOG.md:2189`, `app/escala/orfa_classe.py:40–49,65–77`, `app/chamados/juizes.py:477–505`, `app/ponto/services/cluster_espurio.py:29–37`, `app/ponto/turnos.py:1711–1745`, `app/docs/CORTES.json:2113–2118`, `app/docs/RELATO.md:2340,2375`.

- RED cross-meia-noite sintético. D e D+1 são trabalho em 6x1 noturno 21:00–06:00, sem pausa, somente S às 09:00:10 de D+1. A grade conserva por data civil D+1 porque está fora de ambos os envelopes de atribuição. O pareador dá S sem E ao turno de D porque cai dentro do envelope da véspera até 10:00. O resultado da porta é `SAIDA_SEM_ENTRADA` na ocorrência que a grade guardou em D+1, sem presumir que a chave de ambos os lados coincide. O supra lê este balde como “dia-turno” e acusa genericamente. `app/escala/utils.py:638–644,1863–1888`, `app/ponto/turnos.py:66–82,216–230,285–324,1253–1263,1295–1301,1495–1498`, `app/ponto/supra_juiz.py:522–537`.

## 26/11 00:37 · sessao 008d8e8c3a9b · - · - · origem=Ronald (tmux)

### Juiz e catracas

- O registro `_T1` aponta para `turnos_do_colab`. A leitura por uma batida é `dia_do_turno_da_batida` e a leitura em lote é `dia_das_batidas`. Uma batida reclamada recebe `Turno.data_turno` por PK. Uma batida viva sem turno que a reclame recebe a data civil local. Fora do universo, o leitor singular devolve `None`. `app/core/juizes.py:215–219`, `app/ponto/turnos.py:1850`, `:2951–2982`, `:2992–3021`.
- Já perguntam a ele o espelho, o PDF via espelho, o espelho e as justificativas do app, o painel operacional, SSE, a emissão e o fio de disputa, o alcance E4B, a emissão de órfão da escala e a migração/vigia. `app/ponto/services/espelho.py:465`, `app/relatorios/pdf_espelho.py:596`, `app/api/views.py:750`, `:1444`, `:2042`, `app/api/views_core.py:744`, `app/core/services/painel_op.py:330`, `app/ponto/services/triagem_batida.py:729`, `app/chamados/services/fio.py:1822`, `app/chamados/services/disputa_emissao.py:76`, `:283`, `:400`, `app/chamados/services/foto_e4b.py:283`, `app/escala/servico_jornada.py:509`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:160`, `app/ponto/management/commands/vigia_plantio_overnight.py:87`.
- Existe uma catraca AST geral por **arquivo**, com duas allowlists motivadas. A execução somente leitura nesta cópia encontrou exatamente **6/6** arquivos da lista `.date()` e **21/21** da lista ORM, sem novos nem obsoletos. A catraca admite qualquer sítio que acrescente dentro de arquivo já listado e só detecta `.date()` sobre forma conhecida, sem resolver apelidos nem detectar `localdate(timestamp)` isolado. `app/ponto/tests/test_contract_no_batida_date.py:7–153`, `:156–277`.
- O instrumento complementar do motor já congela contagem, identidade, linhas e composição. Executado sobre 780 arquivos, deu `A1=6`, `A2=15`, `B1=78`, `B2=191`, 9 pareadores e 81 derivações com os três sítios nomeados. A unidade de B1 é função, seu critério é lexical e não cobre o lookup ORM. A célula `turno/marcos` permanece declarada verde pelo teste focal e `PENDENTES_TURNO`. `app/core/medida_turno_do_par.py:2–19`, `:32–52`, `:197–225`, `:392–454`, `app/core/tests/test_medida_turno_do_par.py:108–109`, `:501–575`, `app/core/contratos_estruturais.py:224–239`.
- Há selos locais do espelho, do app e de relatórios. A lei e a autoridade já existem. A classificação por sítio com alcance dos apelidos e o vínculo dessa dívida com o verde seguem pendentes no backlog. `app/ponto/tests/test_espelho_tela_le_o_juiz_do_dia.py:176–204`, `app/api/tests/test_o14_app_le_o_juiz_do_dia.py:48–105`, `app/relatorios/tests/test_pdf_sem_regra_propria.py:150–175`, `app/docs/BACKLOG.md:1858`.

### Perguntas de lei

1. **Turno aberto, sem S**. Candidatas: dia do início já reconhecido, dia civil da batida, esperar fechamento. L-085 manda o dia do início para a jornada identificada. O código atribui `data_turno` à entrada sem S, inclusive véspera quando as condições do juiz valem. Para declarar encerramento ou cobrar marco, L-073 exige instante ou fato encerrado. `app/docs/LEIS.md:100`, `:117`, `app/docs/CORTES.md:328`, `:393`, `CLAUDE.md:189`, `app/ponto/turnos.py:248–324`.
2. **Linha de apresentação**. Candidatas: dia civil para todas, dia do turno para reclamadas e civil para espúrias sem reclamante. O corte do PDF decide a segunda e a aplica ao espelho. A marcação exibida conserva o instante local. Sem turno pareável e com ata, L-102 manda a palavra sem número no espelho, e L-103 manda zero declarado no fechamento. `app/docs/CORTES.md:327`, `app/docs/LEIS.md:86`, `:107`, `:128–129`, `CLAUDE.md:168`.
3. **Feriado, domingo e DSR na virada**. Candidatas: dia do início do turno ou dia civil da ponta. L-085 decide pelo início da jornada. A janela de HE também recebeu corte específico pelo dia do turno do par. `app/docs/LEIS.md:117`, `app/docs/CORTES.md:645`.
4. **Batida sem lastro como a de col673**. Candidatas: E pela sequência sem turno aberto, S pelo marco da escala, papel testemunhado pela ata contra o DNA. L-087 decide quando a ata responde. L-115 decide S pela posição após pausa fechada. Para mera escala versus sequência sem testemunho apto, nenhuma herdada decide. O código sugere E sem turno aberto. O caso consta como prova da obra de dia da batida com par de três minutos, preservação das originais e da trilha. Resultado do juiz nesta cópia sobre o caso real `[nao verificado]`. `app/docs/LEIS.md:119`, `:153`, `app/ponto/turnos.py:2491–2495`, `app/docs/RELATO.md:370`, `app/docs/BACKLOG.md:1858`.
5. **Retenção por férias × batida**. Candidatas: qualquer instante civil que caiu durante férias ou jornada alocada pelo `data_turno`. L-085 resolve a pertença da jornada. A extensão dela ao gate que cruza batida e férias para reter o TXT fica sem decisão herdada localizada. A implementação atual cruza pela data civil. `app/docs/LEIS.md:117`, `app/folha/export.py:464–504`, `:791`.

## 26/11 00:36 · sessao 1e97221d6dcb · - · - · origem=Ronald (tmux)

**Medições documentadas.** Na sombra da competência 10, `turnos_do_colab` contou 514 colaboradores, 8.318 turnos e 2.361 viradas de meia-noite. Com dia civil diferente de `data_turno` **e** marcos distintos, foram 3.496 batidas de borda em 1.986 dias-colaborador e 169 colaboradores, e 3.464 batidas de intervalo em 1.974 dias-colaborador e 168 colaboradores. Houve 351 dias civis com dois turnos. O contrafactual ficou sem medida. A sonda histórica da fatia UTC achou 7 batidas em 6 dias-colaborador e 5 colaboradores, sem impacto monetário medido. `app/docs/RELATO.md:7356`, `app/docs/RELATO.md:7366`.

O censo histórico ESPÚRIAS contou 94 parcelas, incluindo 9 pareadores e 85 derivações, em 88 funções. Classificou 59 juízos paralelos, 18 entradas do par, 6 exibições e 11 sem verificação. O mínimo com caminho vivo que grava ou paga era 23. Os ~98 sítios em 50 arquivos do BACKLOG eram estimativa. A execução do instrumento desta cópia retornou 780 arquivos, A1=6, A2=15, B1=78 e B2=191, 9 pareadores e 81 derivações com parcelas nomeadas. `app/docs/RELATO.md:6090`, `app/docs/RELATO.md:6094`, `app/docs/BACKLOG.md:1858`, `app/core/medida_turno_do_par.py:426`, `app/core/tests/test_medida_turno_do_par.py:108`.

**Col673.** A escala previa trabalho em 21/10 e folga em 22/10. Faltou a entrada de 21/10 por volta das 19h. A batida do app às 06:57:21 de 22/10 virou E sem turno aberto e a pergunta 46936 sobre 21/10 gerou S às 07h. O par de cerca de três minutos é a prova registrada. O recorte separado NOTURNO-A na sombra da competência 11 encontrou 18 dias com realizado positivo e horas trabalhadas zeradas, 11 pessoas e 4.433 minutos realizados. Um subgrupo de 7 dias, 6 pessoas e 25h32 realizadas apresentou inversão pela janela civil, incluindo col673 em 21/10. `app/docs/BACKLOG.md:1858`, `app/docs/RELATO.md:370`, `app/docs/RELATO.md:13177`, `app/docs/RELATO.md:13184`.

**Sonda pendente.** Comparar dia civil, `data_turno` e casos sem atribuição em todas as batidas apuráveis da competência 10 e das abertas, com as bordas, contando por sítio batidas, dias-colaborador, colaboradores e diferenças em lavra, DiaPago, fechamento e TXT. Impacto da migração [nao verificado]. `app/docs/BACKLOG.md:1858`, `app/docs/RELATO.md:7356`.

**Cruzamentos.** E1 v4 tem sobreposição funcional documentada com `recalcular_fechamento_mes`, cujo cursor está em `app/ponto/services/fechamento.py:743` e cuja derivação por batida está em `:813`, e com `_julgar_colab_corpo`, que inclui tanto o censo de `app/ponto/services/cartorio.py:1393` quanto a impressão em `:1433`. `apurar_por_fatia:224` fica no corpo citado pelo problema de atribuição de vínculo descrito no RELATO, mas o hunk de E1 v4 está [nao verificado]. O overlay nomeado corresponde nesta cópia a `_marcar_ausencias`, chamado pelos montadores que contêm conversões. A edição dos corpos dos montadores está [nao verificado]. Em `folha/export.py`, o sítio `dias_ferias_com_batida` é função distinta da emissão do desconto citada no RELATO. `app/docs/RELATO.md:1`, `app/docs/RELATO.md:2`, `app/escala/utils.py:1195`, `app/escala/utils.py:1901`, `app/folha/export.py:464`, `app/folha/export.py:690`.

PAR c7 é a volta de PAR-RELÂMPAGO-A-LISTA-DO-ADMIN. O aval documenta a identidade do dia vivo da lista/vigia, recálculo na restauração, flip sob a trava, vínculo histórico e Pauta DP HTML/PDF/vigia. `varrer_pares_embutidos` é o sítio funcional da divergência civil no detector. O hunk exato do c7 e a coincidência nas outras funções de arquivos compartilhados estão [nao verificado]. `app/docs/RELATO.md:115`, `app/docs/RELATO.md:13211`, `app/docs/PROMPTS.md:2999`, `app/ponto/management/commands/detectar_par_relampago.py:275`, `app/ponto/management/commands/detectar_par_relampago.py:303`.

## 26/11 00:36 · sessao 18f23306242d · - · - · origem=Ronald (tmux)

A cópia tem dois relógios para a sobra. S133 atribui o resíduo não consumido à data civil. `_match_marcos` e os encaixes sem coluna produzem órfãs já consumidas e as entregam na linha de montagem do turno. O exemplo de uma S de D−1 órfã na linha D está documentado junto à direção por ocorrência. O contrato da raia é conservar a linha dona da grade, inclusive para quem lê o dia do turno por outra finalidade. O comentário do supra-juiz generaliza dia-turno, mas seu corpo usa o dia recebido e só recorre à contagem externa quando falta a chave. `app/escala/utils.py:676`, `:1182`, `:1748`, `:1761`, `:1806`, `:1864`, `app/ponto/supra_juiz.py:522`, `:527`, `app/docs/RELATO.md:2154`

- **Tela e PDF.** Calendário lê a ata, faz `set` por dia/mês e casa `HH:MM` a cada objeto. O espelho usa a ata na competência e o builder no trecho anterior. O PDF recompõe a grade, anota `b.orfa` por data/`HH:MM` e ainda usa a união de todas as horas do período para suprimir o turno aberto. Migram os casamentos e a guarda do PDF para a ocorrência e razão canônicas. A batida gravada segue visível na apresentação, inclusive fora do turno e com marca visual. `app/colaboradores/services/calendario.py:361`, `:409`, `:652`, `app/ponto/services/espelho.py:499`, `:532`, `app/templates/ponto/espelho.html:242`, `app/relatorios/pdf_espelho.py:174`, `:477`, `:513`, `:552`, `:609`, `:874`, `app/ponto/juiz_batida.py:18`, `CLAUDE.md:25`
- **Furos.** A contagem lê a ata pela grade e tem recorte próprio de trabalho/ausência parcial, dias encerrados, previsão e elegibilidade. O caso nominal folga com duas sobras mede a diferença desse universo. A ocorrência e a contagem da ata vêm da porta, a métrica segue contando seu recorte. `app/escala/services/furos_diarios.py:12`, `:25`, `:38`, `:49`, `:55`, `:107`, `app/docs/RELATO.md:2338`
- **Cluster.** A assinatura se refaz com dois horários em até 15 minutos e lâmpada apagada. Uma sobra sem assinatura é compatível com esse predicado. A premissa de rajada deve ler a classificação compartilhada quando a lei resolver sua representação, e o serviço segue decidindo abertura, fecho e não medido pela política do chamado. A guarda de atribuição de marco é outra pergunta e já tem o corte vigente próprio. `app/ponto/services/cluster_espurio.py:15`, `:25`, `:33`, `:53`, `:153`, `app/escala/utils.py:502`, `app/docs/CORTES.md:592`, `:600`, `app/docs/RELATO.md:2338`
- **Vigia do pareador.** Migra a identificação de órfã, hoje refeita com data civil mais `HH:MM`. Preserva o cotejo entre a borda do pareador e a lâmpada da ata, incluindo o recorte dos turnos posicionáveis. O pareador segue fonte da geometria. `app/ponto/services/vigia_pareador_celula.py:67`, `:82`, `:97`, `:106`, `app/docs/JUIZES.md:17`
- **Supra, chamado e folha.** Supra emite protesto amplo por órfã só no ramo de trabalho com previsto e realizado zero, que pode tornar o veredito discordante e reter o export. Lê a razão no mesmo julgamento antes da persistência. Chamado usa presença de órfã com guarda de cadastro, e materialização injeta os `HH:MM` no cálculo posicional do plantio. Ambas precisam da mesma ocorrência/razão no insumo, com seus efeitos próprios. A causa mensal usa presença de órfã e presença em folga, e é gravada pelo fechamento. Migra seu insumo de razão, e a causa mensal conserva a sua pergunta. `app/ponto/supra_juiz.py:392`, `:527`, `app/escala/models.py:1627`, `app/folha/export.py:968`, `:1273`, `app/ponto/services/cartorio.py:1470`, `:1506`, `app/chamados/juizes.py:543`, `:559`, `app/chamados/services/materializacao.py:418`, `:461`, `app/folha/causas.py:43`, `:76`, `:100`, `app/ponto/services/fechamento.py:1101`
- **Outros insumos.** `status_dia` e bordas leem presença/contagem de fato. `propositor` tem filtro próprio do dia e `esmeril_espelho` agrega quantidade e horário. `detectar_par_relampago` pontua a grade por contagem, `propor_reorganizacao` usa as marcas candidatas e `tripwire_celulas` peneira por minuto e confirma por instante e par. `juiz_batida` projeta o cru para exibir. `cartao_pela_celula.orfas_e_isencao_da_ata` tem só definição na busca Python de produção. `horas_reais_do_dia` do mapa morreu e a leitora atual `batidas_reais_do_dia` recebe direção por ocorrência para a ausência parcial. `app/ponto/status_dia.py:188`, `app/ponto/services/bordas_realizado.py:49`, `app/ponto/services/simular_dia.py:12`, `app/ponto/propositor.py:39`, `app/ponto/services/esmeril_espelho.py:130`, `app/ponto/management/commands/detectar_par_relampago.py:140`, `app/ponto/management/commands/propor_reorganizacao.py:104`, `app/ponto/management/commands/tripwire_celulas.py:145`, `:264`, `:283`, `app/ponto/juiz_batida.py:165`, `app/relatorios/cartao_pela_celula.py:277`, `app/ponto/turnos.py:2694`, `:2714`, `app/escala/utils.py:1279`

A ata preserva a quantidade e a repetição de `HH:MM`; o classificador e os leitores por `set` perdem a identidade. A raia registrou a classificação por minuto como técnica contestável da sonda e depois marcou como bloqueante transformar duas sobras no mesmo minuto em ambiguidade. O RED já pode exigir duas razões individualmente atribuíveis e impedir que uma sobra seja removida por coincidir só no minuto. A escolha dos rótulos diagnósticos para o legado continua distinta do contrato por ocorrência. `app/ponto/services/cartorio.py:761`, `app/escala/orfa_classe.py:53`, `:73`, `app/escala/utils.py:2093`, `app/docs/RELATO.md:2131`, `:2147`, `:2306`

## 26/11 00:36 · sessao 01fad129843c · - · - · origem=Ronald (tmux)

A unidade da ata é uma ocorrência de `HH:MM`. O PK sai na projeção de `locs`, enquanto a data, os segundos e o instante completo saem na redução para `HH:MM`. Duas ocorrências no mesmo minuto continuam na lista e somam duas em `n_orfas`, mas o dicionário do classificador as reduz a uma chave. A reconciliação da ponta por `remove(HH:MM)` remove a primeira ocorrência correspondente. [app/escala/utils.py:1367](/home/ronald/codex_ws/c314c099f3f447dbb628557c9ada9906183f1182/app/escala/utils.py:1367), [app/escala/utils.py:1870](/home/ronald/codex_ws/c314c099f3f447dbb628557c9ada9906183f1182/app/escala/utils.py:1870), [app/escala/utils.py:2093](/home/ronald/codex_ws/c314c099f3f447dbb628557c9ada9906183f1182/app/escala/utils.py:2093), [app/ponto/services/cartorio.py:749](/home/ronald/codex_ws/c314c099f3f447dbb628557c9ada9906183f1182/app/ponto/services/cartorio.py:749), [app/escala/orfa_classe.py:65](/home/ronald/codex_ws/c314c099f3f447dbb628557c9ada9906183f1182/app/escala/orfa_classe.py:65).

A atribuição civil cobre especificamente as residuais da S133. O casador também emite órfãs vinculadas à linha do início do turno, inclusive pontas fora da régua fixa e excesso de slots internos. A testemunha 12x36 19h–07h registra a E `06:54:26` de D+1 como órfã na grade D, entre E `19:00:00` de D e S `06:55:54` de D+1. O supra-juiz usa a linha tal como recebe, com comentário geral de dia-turno. Para a exibição, `dia_das_batidas` já usa PK, dia do turno que reclama a marcação e dia civil para a avulsa. [app/escala/utils.py:1748](/home/ronald/codex_ws/c314c099f3f447dbb628557c9ada9906183f1182/app/escala/utils.py:1748), [app/escala/utils.py:1806](/home/ronald/codex_ws/c314c099f3f447dbb628557c9ada9906183f1182/app/escala/utils.py:1806), [app/escala/utils.py:1863](/home/ronald/codex_ws/c314c099f3f447dbb628557c9ada9906183f1182/app/escala/utils.py:1863), [app/escala/tests/test_sobra_na_borda_nao_vira_intervalo.py:233](/home/ronald/codex_ws/c314c099f3f447dbb628557c9ada9906183f1182/app/escala/tests/test_sobra_na_borda_nao_vira_intervalo.py:233), [app/ponto/supra_juiz.py:523](/home/ronald/codex_ws/c314c099f3f447dbb628557c9ada9906183f1182/app/ponto/supra_juiz.py:523), [app/ponto/turnos.py:2951](/home/ronald/codex_ws/c314c099f3f447dbb628557c9ada9906183f1182/app/ponto/turnos.py:2951).

O `_dedup_dez_min` remove uma batida do mesmo tipo que a última sobrevivente, a menos de dez **minutos**. E e S a dez segundos passam. A mordida por minuto está em `papel_por_minuto_da_ata`, que se abstém com luz ambígua ou batidas internas duplicadas no minuto. A raia corrigiu seu RED para E `21:00:10` e S `21:00:40`, depois de o ensaio com duas E perder uma na dedup. `MINUTO_AMBIGUO` e `MINUTO_SEM_BATIDA` são decisões técnicas registradas para a sonda por minuto. O rótulo normativo de uma ocorrência capturada na fonte com identidade individual permanece [não verificado]. [app/escala/utils.py:1371](/home/ronald/codex_ws/c314c099f3f447dbb628557c9ada9906183f1182/app/escala/utils.py:1371), [app/ponto/turnos.py:1604](/home/ronald/codex_ws/c314c099f3f447dbb628557c9ada9906183f1182/app/ponto/turnos.py:1604), [app/ponto/turnos.py:1734](/home/ronald/codex_ws/c314c099f3f447dbb628557c9ada9906183f1182/app/ponto/turnos.py:1734), [app/docs/RELATO.md:2123](/home/ronald/codex_ws/c314c099f3f447dbb628557c9ada9906183f1182/app/docs/RELATO.md:2123), [app/docs/RELATO.md:2306](/home/ronald/codex_ws/c314c099f3f447dbb628557c9ada9906183f1182/app/docs/RELATO.md:2306).

_8293 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
