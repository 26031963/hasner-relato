# CODEX-ESTADO

> Ultima resposta processada: **18/11/2026 21:38** · publicado em **18/11/2026 21:39** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 18/11 21:38 · sessao 238ae9d67693 · E4C-F6F7 · construtor · origem=Code

A correção foi aplicada somente em [test_e4c_f6f7_c14_lei3_pela_leitura.py](/home/ronald/wt-codex-e4c-f6f7-213751/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:48). O diff é: [nao verificado]

```diff
- decisoes=N(all=lambda: ds))
+ decisoes=N(all=lambda ds=ds: ds))

- setattr(vista, 'respondida_em', instante)
+ vista.respondida_em = instante

- def funcao_mutada(arquivo, nome, *args, **kw):
+ def funcao_mutada(arquivo, nome, *args, antigo=antigo, novo=novo, **kw):

- decisoes=N(all=lambda: ds))
+ decisoes=N(all=lambda ds=ds: ds))
```

Sobre cada B023:

- **`ds`, linha 48:** a leitura é feita na linha 49, dentro da mesma iteração. O fechamento tardio não mudava o caso testado. [arquivo:linhas 44–49](/home/ronald/wt-codex-e4c-f6f7-213751/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:44)
- **`antigo` e `novo`, linha 179:** a função mutada é usada pelo teste chamado na linha 182, dentro da mesma iteração. O fechamento tardio não mudava o mutante testado. [arquivo:linhas 174–182](/home/ronald/wt-codex-e4c-f6f7-213751/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:174) [nao verificado]
- **`ds`, linha 221:** a leitura é feita na linha 222, dentro da mesma iteração. O fechamento tardio não mudava o caso testado. [arquivo:linhas 216–225](/home/ronald/wt-codex-e4c-f6f7-213751/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:216)

A verificação do Ruff ficou **não verificada**: o executável não está no PATH e `python3 -m ruff` também informou que o módulo não está instalado. Saída do comando solicitado, tentado antes da edição:

```text
$ PYTHONDONTWRITEBYTECODE=1 ruff check --config ruff.toml --no-cache chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py
/bin/bash: line 1: ruff: command not found
```

Não foi possível executar o RED nem os mutantes do lint pelo mesmo motivo. [não verificado] A suíte e o banco não foram executados, conforme solicitado. [nao verificado]

## 18/11 21:37 · sessao 954a693d4c56 · ESCALA-NAO-DESCREVE · verificador · origem=Code

1. **OK — a chamada nova chega à guarda da linha 313.** O teste chama diretamente `realocar_vinculo_para_destino` com o PK de `t12`, criado como `12x36`; a função extrai o ID, busca o template e avalia sua inclusão em `CICLICOS`, que contém `12x36`. [app/escala/tests/test_escala_nao_descreve.py:131](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/tests/test_escala_nao_descreve.py:131), [app/escala/tests/test_escala_nao_descreve.py:320](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/tests/test_escala_nao_descreve.py:320), [app/escala/models.py:386](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/models.py:386), [app/escala/regua_defesa.py:307](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/regua_defesa.py:307). Os únicos retornos anteriores são “Sem destino sugerido.” e “Template sugerido nao existe mais.”; nenhum contém `ciclico`. [app/escala/regua_defesa.py:308](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/regua_defesa.py:308). Após a guarda, a função monta o POST interno, chama o juiz, trata exceção e `no_op`, e então chama o executor; esse trecho não é alcançado no caso cíclico atual. [app/escala/regua_defesa.py:313](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/regua_defesa.py:313).

2. **OK — para esta chamada e estes dados, a mensagem que satisfaz a asserção é a da guarda:** `Destino ENDL-12 e ciclico: informe o Inicio do turno no wizard de escalas (nao se infere fase).` [app/escala/tests/test_escala_nao_descreve.py:131](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/tests/test_escala_nao_descreve.py:131), [app/escala/regua_defesa.py:314](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/regua_defesa.py:314). A regra de fase do juiz produziria outra mensagem, sem `ciclico`, e só seria chamada depois dessa guarda; a mensagem da view contém `ciclica`, mas esta chamada direta não passa pela view. [app/colaboradores/services/vinculo.py:126](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/colaboradores/services/vinculo.py:126), [app/escala/views.py:498](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/views.py:498). Em geral, a função interpola código ou descrição nas mensagens e devolve texto de exceções; portanto, a substring isolada não identificaria necessariamente a origem para dados arbitrários. [app/escala/regua_defesa.py:314](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/regua_defesa.py:314), [app/escala/regua_defesa.py:320](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/regua_defesa.py:320).

3. **OK, com limite — o controle prova uma gravação observável nesse cenário.** `x` começa em 5x2 desde setembro; o teste exige sucesso, uma linha ativa de `x` em 6x1 com início em `HOJE` e aumento de um no total do histórico. Assim, a existência exigida não é a linha inicial da fixture. [app/escala/tests/test_escala_nao_descreve.py:127](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/tests/test_escala_nao_descreve.py:127), [app/escala/tests/test_escala_nao_descreve.py:133](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/tests/test_escala_nao_descreve.py:133), [app/escala/tests/test_escala_nao_descreve.py:326](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/tests/test_escala_nao_descreve.py:326). A classe congela o relógio e passa `HOJE` explicitamente; o juiz e o executor ainda leem o relógio internamente, mas essas leituras ficam sob o congelamento neste teste. [app/escala/tests/test_escala_nao_descreve.py:30](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/tests/test_escala_nao_descreve.py:30), [app/escala/tests/test_escala_nao_descreve.py:116](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/tests/test_escala_nao_descreve.py:116), [app/colaboradores/services/vinculo.py:71](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/colaboradores/services/vinculo.py:71), [app/colaboradores/services/vinculo.py:194](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/colaboradores/services/vinculo.py:194). O caso troca tanto o colaborador quanto o destino e não verifica células: ele não isola qual diferença explica o resultado, nem prova regeneração de células. [app/escala/tests/test_escala_nao_descreve.py:320](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/tests/test_escala_nao_descreve.py:320).

4. **FILA — as contagens demonstram menos que uma comparação das linhas.** O próprio teste lê as contagens globais antes e depois da recusa e, no controle, filtra `EscalaColaborador` por colaborador, tipo, início e ativa; lê apenas o total global de `HistoricoVinculo`. [app/escala/tests/test_escala_nao_descreve.py:319](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/tests/test_escala_nao_descreve.py:319). No caminho de sucesso, o executor fecha vínculos anteriores por `fechar_vigencia`, que escreve `ativa` e `data_fim`, e cria a nova linha com os campos filtrados pelo teste. [app/colaboradores/services/vinculo.py:211](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/colaboradores/services/vinculo.py:211), [app/colaboradores/services/vinculo.py:641](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/colaboradores/services/vinculo.py:641), [app/colaboradores/services/vinculo.py:287](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/colaboradores/services/vinculo.py:287). O executor chama `registrar_mudanca`, que cria a linha de histórico; a tela de colaboradores lê esse histórico por colaborador. [app/colaboradores/services/vinculo.py:349](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/colaboradores/services/vinculo.py:349), [app/colaboradores/services/historico_vinculo.py:7](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/colaboradores/services/historico_vinculo.py:7), [app/colaboradores/views.py:317](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/colaboradores/views.py:317). Contagem igual na recusa não excluiria edição de linha existente; o `+1` global tampouco identifica, sozinho, o conteúdo ou colaborador da linha criada. [app/escala/tests/test_escala_nao_descreve.py:324](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/tests/test_escala_nao_descreve.py:324).

5. **FILA — há mordida na guarda, com limites de contrato.** O anexo informa que o mutante `mut_sem_recusa_ciclico` falhou nesse teste, mas não mostra a alteração do mutante nem a asserção que falhou; esses detalhes ficam **[não verificado]**. O teste fixa a recusa observável da porta para `12x36`, sem exigir que o julgamento more especificamente na linha 313; o juiz de fase em `vinculo.py` usa outra condição e outra mensagem, e o predicado da lista continua separado. [app/escala/tests/test_escala_nao_descreve.py:320](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/tests/test_escala_nao_descreve.py:320), [app/colaboradores/services/vinculo.py:128](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/colaboradores/services/vinculo.py:128), [app/escala/services/cadastro_realidade.py:849](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/services/cadastro_realidade.py:849). A outra consulta a `CICLICOS` na régua pertence ao diagnóstico, fora dessa chamada direta. [app/escala/regua_defesa.py:198](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/regua_defesa.py:198), [app/escala/regua_defesa.py:291](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/regua_defesa.py:291). O congelamento impede que este teste exercite uma virada de dia entre as leituras internas do relógio. [app/escala/tests/test_escala_nao_descreve.py:116](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/tests/test_escala_nao_descreve.py:116), [app/colaboradores/services/vinculo.py:189](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/colaboradores/services/vinculo.py:189). Quanto ao significado de `motivo`, o argumento textual vai para `confirm_txt` e depois `obs`, enquanto `HistoricoVinculo.motivo` recebe a categoria `mudanca` ou `correcao`; o caso novo só conta linhas e não verifica esses valores. [app/escala/regua_defesa.py:301](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/escala/regua_defesa.py:301), [app/colaboradores/services/vinculo.py:354](/home/ronald/codex_ws/a179c141e841f3303668dbd545eaa42237cc27a9/app/colaboradores/services/vinculo.py:354).

## 18/11 21:30 · sessao 7732a47fe90e · ESCALA-NAO-DESCREVE · construtor · origem=Code

Acrescentei um teste que chama a porta diretamente com destino 12x36, verifica a mensagem e as contagens, e usa uma realocação para 6x1 como controle ([teste:311](/home/ronald/wt-codex-escala-nao-descreve-212957/app/escala/tests/test_escala_nao_descreve.py:311)). A data é fixa ([teste:33](/home/ronald/wt-codex-escala-nao-descreve-212957/app/escala/tests/test_escala_nao_descreve.py:33)). [nao verificado]

O teste e o mutante **não foram executados**, conforme solicitado. `git diff --check` passou [não verificado por certificação]. [nao verificado]

Comando do teste: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python manage.py test escala.tests.test_escala_nao_descreve.ListaPorCargaTest.test_MORDE_R5_porta_recusa_ciclico_e_realoca_nao_ciclico
```

O mutante remove, em memória, a recusa de [regua_defesa.py:313](/home/ronald/wt-codex-escala-nao-descreve-212957/app/escala/regua_defesa.py:313), sem editar produção:

```bash
PYTHONDONTWRITEBYTECODE=1 python - <<'PY'
import inspect
import os
import sys

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'config.settings.saas')
import django
django.setup()

import escala.regua_defesa as regua
fonte = inspect.getsource(regua.realocar_vinculo_para_destino)
linhas = fonte.splitlines(keepends=True)
indices = [i for i, linha in enumerate(linhas)
           if "if te.tipo_ciclo in getattr(te, 'CICLICOS'" in linha]
assert len(indices) == 1
i = indices[0]
assert "return False" in linhas[i + 1] and "ciclico" in linhas[i + 1]
del linhas[i:i + 2]
exec(compile(''.join(linhas), regua.__file__, 'exec'), regua.__dict__)

from django.core.management import execute_from_command_line
execute_from_command_line([
    sys.argv[0], 'test',
    'escala.tests.test_escala_nao_descreve.ListaPorCargaTest.'
    'test_MORDE_R5_porta_recusa_ciclico_e_realoca_nao_ciclico',
])
PY
```

A reprovação esperada é `AssertionError` na busca por `'ciclico'` ([teste:323](/home/ronald/wt-codex-escala-nao-descreve-212957/app/escala/tests/test_escala_nao_descreve.py:323)): sem a recusa da porta, o juiz inferior retorna outra mensagem para a ausência de início do turno ([vinculo.py:128](/home/ronald/wt-codex-escala-nao-descreve-212957/app/colaboradores/services/vinculo.py:128)). **Resultado e classe exata da reprovação: [não verificado].** [nao verificado]

LEI-AKITA: origem=n/a (selo; porta inalterada); testemunha=`realocar_vinculo_para_destino` ([regua_defesa.py:291](/home/ronald/wt-codex-escala-nao-descreve-212957/app/escala/regua_defesa.py:291)); RED=`test_MORDE_R5_porta_recusa_ciclico_e_realoca_nao_ciclico` ([teste:311](/home/ronald/wt-codex-escala-nao-descreve-212957/app/escala/tests/test_escala_nao_descreve.py:311), mordida [não verificada]); quem-mais-le=[views.py:471](/home/ronald/wt-codex-escala-nao-descreve-212957/app/escala/views.py:471), [views.py:502](/home/ronald/wt-codex-escala-nao-descreve-212957/app/escala/views.py:502), [regua_defesa.py:332](/home/ronald/wt-codex-escala-nao-descreve-212957/app/escala/regua_defesa.py:332), [test_termometro_camada3.py:100](/home/ronald/wt-codex-escala-nao-descreve-212957/app/escala/tests/test_termometro_camada3.py:100), [test_termometro_camada3.py:105](/home/ronald/wt-codex-escala-nao-descreve-212957/app/escala/tests/test_termometro_camada3.py:105); juizes novos=0 ([diff do teste](/home/ronald/wt-codex-escala-nao-descreve-212957/app/escala/tests/test_escala_nao_descreve.py:311)); padrao=n/a (instrumento). [nao verificado]

## 18/11 21:27 · sessao 429e0eff8383 · RAIA-ESPURIAS · leitura · origem=Code

A interface de `logs/espuria/envelope.sh` não está disponível na cópia consultada; a execução do script por esse envelope está [não verificado]. A sonda abaixo pressupõe um ambiente Django configurado e inicializa o Django caso necessário [não verificado]. A função de batidas filtra marcas não retratadas ([app/ponto/turnos.py](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:2868)); a grade e a classificação seguem as funções da cópia ([app/escala/utils.py](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/utils.py:2334), [app/escala/orfa_classe.py](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/orfa_classe.py:53)). Efeitos indiretos de todas as funções chamadas estão [não verificado].

`DIAS` conta pares colaborador-dia com órfãs encontrados na grade, como no anexo fornecido [não verificado]. A sonda não foi executada [não verificado].

```python
import collections
import datetime as dt
import re

ERROS = 0
TOTAL_ORFAS = 0
TOTAL_DIAS = 0
POR_CLASSE = collections.Counter()


def erro(col, etapa, exc):
    global ERROS
    ERROS += 1
    print(f"ERRO;{col};{etapa};{type(exc).__name__}", flush=True)


def executar():
    global TOTAL_ORFAS, TOTAL_DIAS

    try:
        import django
        from django.apps import apps

        if not apps.ready:
            django.setup()

        from django.db import connection

        if connection.settings_dict.get("NAME") != "sombra":
            print("NAO_FEITO;guarda_banco", flush=True)
            return

        from django.utils import timezone
        from colaboradores.models import Colaborador
        from ponto.turnos import batidas_apuraveis
        from escala.utils import grade_espelho_janela
        from escala.orfa_classe import classificar_orfas
    except Exception as exc:
        erro("col?", "inicializacao", exc)
        print("NAO_FEITO;inicializacao", flush=True)
        return

    try:
        agora = timezone.now()
        ini = dt.date(2026, 10, 21)
        fim = dt.date(2026, 11, 18)
        lo = (
            timezone.make_aware(dt.datetime.combine(ini, dt.time.min))
            - dt.timedelta(days=1)
        )
        hi = (
            timezone.make_aware(dt.datetime.combine(fim, dt.time.max))
            + dt.timedelta(days=1)
        )
        colaboradores = iter(
            Colaborador.objects.select_related("empresa", "posto")
            .filter(empresa_id__in=[1, 2, 3, 4])
            .order_by("pk")
            .iterator()
        )
    except Exception as exc:
        erro("col?", "preparacao", exc)
        print("NAO_FEITO;preparacao", flush=True)
        return

    while True:
        try:
            c = next(colaboradores)
        except StopIteration:
            break
        except Exception as exc:
            erro("col?", "iteracao_colaboradores", exc)
            print("NAO_FEITO;iteracao_colaboradores_restante", flush=True)
            break

        col = f"col{c.pk}"

        try:
            bs = list(batidas_apuraveis(c, lo, hi))
        except Exception as exc:
            erro(col, "batidas_apuraveis", exc)
            continue

        try:
            grade = grade_espelho_janela(c, bs, ini, fim, agora=agora)
            dias = grade.get("dias") or []
        except Exception as exc:
            erro(col, "grade_espelho_janela", exc)
            continue

        for d in dias:
            try:
                match = re.search(r"(\d{2})/(\d{2})", d.get("label", ""))
                if not match:
                    continue
                dia = dt.date(2026, int(match.group(2)), int(match.group(1)))
                horas = d.get("orfas") or []
                if not horas:
                    continue
                TOTAL_DIAS += 1
            except Exception as exc:
                erro(col, "dia_grade", exc)
                continue

            try:
                classes = classificar_orfas(d)
            except Exception as exc:
                erro(col, "classificar_orfas", exc)
                continue

            for hora in horas:
                try:
                    classe = classes.get(hora, "indefinida")
                    print(
                        f"VIVA;{col};{c.empresa_id};{dia.isoformat()};"
                        f"{hora};{classe}",
                        flush=True,
                    )
                    TOTAL_ORFAS += 1
                    POR_CLASSE[classe] += 1
                except Exception as exc:
                    erro(col, "emitir_orfa", exc)


try:
    executar()
except Exception as exc:
    erro("col?", "execucao", exc)
    print("NAO_FEITO;execucao_restante", flush=True)
finally:
    print(f"TOTAL_ORFAS={TOTAL_ORFAS}", flush=True)
    print(f"DIAS={TOTAL_DIAS}", flush=True)
    for classe, quantidade in sorted(POR_CLASSE.items()):
        print(f"CLASSE;{classe};{quantidade}", flush=True)
    print(f"ERROS={ERROS}", flush=True)
    print("FIM_FERNANDO_SV", flush=True)
```

## 18/11 21:25 · sessao e28abb7896fc · RAIA-ESPURIAS · leitura · origem=Code

O script abaixo preserva as regras e os agregados do anexo e acrescenta a emissão `NOSSA`; a comparação com `s1_v4_bruto.out`, a execução pelo envelope e `ERROS=0` permanecem **[não verificado]**, pois não há acesso ao ambiente de produção. O anexo `s1_v4.py` e o envelope não foram encontrados como arquivos nesta cópia **[não verificado]**.

As linhas acrescentadas estão marcadas com `# ACRESCENTADA`; seus números físicos são **[não verificado]**. A última linha de saída foi alterada para `FIM_FERNANDO_SN`. Quando uma falha impede obter a empresa ou consultar todos os minutos, o campo correspondente recebe `NAO_FEITO`.

```python
# ITEM 1 — AUDITORIA-ESPURIAS — medida, nao juiz.
# Universo: CelulaDia de empresas 2, 3 e 4, 2026-10-21 a 2026-11-20.
# Fonte das ocorrencias: CelulaDia.ata; porta da classificacao: grade_da_celula.
# Casamento: Batida viva do mesmo colab no minuto exato da data da celula.
# Exclusoes anteriores as classes: sem_hora; nao_mensuravel (a) e (b).
# F: tipo_dia == folga; le grade_da_celula.
# T6: trabalho e grade demonstra ausencia de marcos; le grade_da_celula.
# T1: trabalho e algum marco missing a <=90 min; le grade_da_celula e Batida.
# T2: trabalho, ha marco missing e todos estao a >90 min; le grade_da_celula e Batida.
# T5: outra Batida viva do mesmo tipo a <=10 min; le Batida.
# T4: entre as restantes, pares de orfas E seguidas da S seguinte, em ordem;
#     le grade_da_celula e Batida.
# T3: restante com instante e tipo decidiveis; le grade_da_celula e Batida.
# T5 auxiliar: outra Batida viva de qualquer tipo a <=5 min; le Batida.
# Diagnostico adicional: turnos_do_colab; nao decide as classes acima.
# Precedencia: F, T6, T1, T2, T5, T4, T3.
# Dias e pessoas sao conjuntos por classe e podem aparecer em varias classes.

from collections import Counter, defaultdict
from datetime import date, datetime, time, timedelta
import re

from django.db import DatabaseError
from django.utils import timezone

from colaboradores.models import Colaborador
from escala.models import CelulaDia
from escala.services.leitor_celula import grade_da_celula
from ponto.models import Batida
from ponto.turnos import turnos_do_colab

INI = date(2026, 10, 21)
FIM = date(2026, 11, 20)
EMPRESAS = (2, 3, 4)
CLASSES = ("F", "T6", "T1", "T2", "T5", "T4", "T3",
           "nao_mensuravel_a", "nao_mensuravel_b",
           "sem_hora", "NAO_CLASSIFICADA")

erros = 0
totais = Counter()
dias = defaultdict(set)
pessoas = defaultdict(set)
verdes = Counter()
motivos = Counter()
t5_aux = Counter()
total_ata = 0
turnos_encontrados = 0
universo_completo = True


def campos(modelo):
    print("CAMPOS", modelo._meta.label,
          " ".join(f.name for f in modelo._meta.get_fields()
                   if getattr(f, "concrete", False)))


def registrar(classe, cid, dia, verde, motivo=None): [nao verificado]
    totais[classe] += 1
    dias[classe].add((cid, dia))
    pessoas[classe].add(cid)
    if verde is True: [nao verificado]
        verdes[classe] += 1
    elif verde is None: [nao verificado]
        verdes[classe + "_verde_indeterminado"] += 1
    if motivo:
        motivos[(classe, motivo)] += 1


def falha(etapa, exc, cid=None):
    global erros
    erros += 1
    # Nao imprimir mensagem da excecao: pode conter nome ou outro dado pessoal.
    print("ERRO", "etapa=" + etapa, "tipo=" + type(exc).__name__,
          "col=" + ("col" + str(cid) if cid is not None else "universo"))


def perm_negada(exc):
    atual = exc
    while atual is not None:
        if getattr(atual, "pgcode", None) == "42501":
            return True
        if getattr(atual, "sqlstate", None) == "42501":
            return True
        atual = getattr(atual, "__cause__", None)
    return False


def hhmm(valor):
    if not isinstance(valor, str) or not re.fullmatch(r"\d{2}:\d{2}", valor):
        return None
    h, m = map(int, valor.split(":"))
    if h > 23 or m > 59:
        return None
    return time(h, m)


def local_minuto(ts):
    return timezone.localtime(ts).replace(second=0, microsecond=0)


def instante(dia, hora):
    return timezone.make_aware(datetime.combine(dia, hora),
                               timezone.get_current_timezone())


def emitir_nossa(cid, empresa_id, cel, valor, classe, motivo, por_minuto):  # ACRESCENTADA
    hora = hhmm(valor)  # ACRESCENTADA
    if hora is None:  # ACRESCENTADA
        pks = ""  # ACRESCENTADA
    elif por_minuto is None:  # ACRESCENTADA
        pks = "NAO_FEITO"  # ACRESCENTADA
    else:  # ACRESCENTADA
        batidas_minuto = por_minuto.get(instante(cel.data, hora), [])  # ACRESCENTADA
        pks = ",".join(str(b.pk) + ":" + str(b.tipo) for b in batidas_minuto)  # ACRESCENTADA
    print("NOSSA", "col" + str(cid), empresa_id, cel.data.isoformat(),  # ACRESCENTADA
          valor if hora is not None else "", classe,  # ACRESCENTADA
          (motivo or "") if classe == "NAO_CLASSIFICADA" else "", pks, sep=";")  # ACRESCENTADA


def ocorrencias_da_ata(ata):
    # O cartorio normalmente grava ambos; divergencia e conservada e avisada.
    if not isinstance(ata, dict):
        raise ValueError("ata nao e objeto")
    lista = ata.get("orfas") or []
    if not isinstance(lista, list):
        raise ValueError("ata.orfas nao e lista")
    lista = [str(x) if x else "?" for x in lista]
    n = ata.get("n_orfas")
    n = len(lista) if n is None else int(n)
    if n < 0:
        raise ValueError("n_orfas negativo")
    if lista and len(lista) != n:
        print("ATA_CONTAGEM_DIVERGENTE",
              "lista=" + str(len(lista)), "n_orfas=" + str(n))
    # A lista lavrada e a fonte quando existe, como em _orfas_da_ata.
    return lista if lista else ["?"] * n


def virada(cel, dia_grade):
    # True/False so quando ha os dois extremos; None significa nao sei.
    pares = {}
    for marco in dia_grade.get("marcos") or []:
        if isinstance(marco, dict) and marco.get("tipo") in ("E", "S"):
            h = hhmm(marco.get("hora"))
            if h is not None:
                pares.setdefault(marco["tipo"], []).append(h)
    if pares.get("E") and pares.get("S"):
        return pares["S"][-1] <= pares["E"][0]
    dna = cel.dna if isinstance(cel.dna, dict) else {}
    marcos = dna.get("marcos") or {}
    if isinstance(marcos, dict):
        hi, hf = hhmm(marcos.get("hi")), hhmm(marcos.get("hf"))
        if hi is not None and hf is not None:
            return hf <= hi
    return None


def verde_do_dia(d):
    if d.get("tipo_dia") != "trabalho":
        return False
    celulas = d.get("celulas")
    if not isinstance(celulas, list):
        return None
    if not celulas:
        return None
    if any(not isinstance(c, dict) or "missing" not in c for c in celulas):
        return None
    return not any(c["missing"] for c in celulas)


def distancias_missing(d, data_celula, ts_orfa):
    # A ordem dos marcos permite datar marcos apos a virada.
    celulas = d.get("celulas")
    if not isinstance(celulas, list):
        return None, "celulas da grade ausentes"
    if any(not isinstance(c, dict) or "missing" not in c for c in celulas):
        return None, "estado missing desconhecido"
    if not any(c["missing"] for c in celulas):
        return [], None
    resultado = []
    dia_marco = data_celula
    anterior = None
    for c in celulas:
        h = hhmm(c.get("hora_prevista"))
        if h is None:
            if c["missing"]:
                return None, "marco missing sem hora prevista"
            # Sem hora de um marco intermediario, a virada posterior e incerta.
            return None, "marco sem hora impede datar os missing"
        if anterior is not None and h < anterior:
            dia_marco += timedelta(days=1)
        anterior = h
        if c["missing"]:
            resultado.append(abs((instante(dia_marco, h) - ts_orfa)
                                 .total_seconds()) / 60)
    return resultado, None


def tem_outra(batida, batidas, minutos, mesmo_tipo):
    for outra in batidas:
        if outra.pk == batida.pk:
            continue
        if mesmo_tipo and outra.tipo != batida.tipo:
            continue
        if abs((outra.timestamp - batida.timestamp).total_seconds()) <= minutos * 60:
            return True
    return False


for modelo in (Colaborador, CelulaDia, Batida):
    campos(modelo)

print("UNIVERSO", INI.isoformat(), FIM.isoformat(), "empresas=2,3,4")
print("REFERENCIA_17_11=1267")
print("DIA_VERDE_REFERENCIA=83_dias/50_pessoas/105_orfas [nao verificado]")

try:
    ids = list(
        CelulaDia.objects.filter(
            data__range=(INI, FIM),
            colaborador__empresa_id__in=EMPRESAS,
        ).values_list("colaborador_id", flat=True).distinct().order_by("colaborador_id")
    )
except Exception as exc:
    falha("listar_colaboradores_da_celula", exc)
    ids = []
    universo_completo = False

for cid in ids:
    etiqueta = "col" + str(cid)
    etapa = "carregar_celulas"
    cels = None
    ocorrencias = []
    colab = None  # ACRESCENTADA
    minutos_completos = None  # ACRESCENTADA
    try:
        cels = list(CelulaDia.objects.filter(
            colaborador_id=cid, data__range=(INI, FIM)
        ).order_by("data", "pk"))
        for cel in cels:
            for valor in ocorrencias_da_ata(cel.ata or {}):
                ocorrencias.append((cel, valor))
        total_ata += len(ocorrencias)
        if not ocorrencias:
            continue

        etapa = "carregar_colaborador"
        colab = Colaborador.objects.get(pk=cid)

        etapa = "grade_da_celula(colab, INI, FIM)"
        grade = grade_da_celula(colab, INI, FIM)
        por_data = {d["data"]: d for d in grade["dias"]}

        etapa = "turnos_do_colab(colab, INI, FIM)"
        turnos = turnos_do_colab(colab, INI, FIM)
        turnos_encontrados += len(turnos)

        etapa = "consultar_Batida_viva"
        # Margem cobre T5 nas bordas e casamento no dia seguinte.
        batidas = list(Batida.objects.filter(
            colaborador_id=cid,
            retratada_em__isnull=True,
            timestamp__date__gte=INI - timedelta(days=1),
            timestamp__date__lte=FIM + timedelta(days=1),
        ).order_by("timestamp", "pk"))
        por_minuto = defaultdict(list)
        for b in batidas:
            por_minuto[local_minuto(b.timestamp)].append(b)
        minutos_completos = por_minuto  # ACRESCENTADA

        # Primeiro decide exclusoes e classes anteriores a T4.
        registros = []
        for cel, valor in ocorrencias:
            d = por_data.get(cel.data)
            r = {"cel": cel, "valor": valor, "classe": None,
                 "motivo": None, "batida": None, "verde": None} [nao verificado]
            registros.append(r)
            if d is None:
                r["classe"], r["motivo"] = "NAO_CLASSIFICADA", "dia ausente da grade"
                continue
            r["verde"] = verde_do_dia(d) [nao verificado]
            hora = hhmm(valor)
            if valor == "?":
                r["classe"] = "sem_hora"
                continue
            if hora is None:
                r["classe"], r["motivo"] = "NAO_CLASSIFICADA", "hora da ata invalida"
                continue

            alvo = instante(cel.data, hora)
            hoje = por_minuto.get(alvo, [])
            amanha = por_minuto.get(alvo + timedelta(days=1), [])
            atravessa = virada(cel, d)

            # (b) tem prioridade quando o proprio minuto da celula e multiplo.
            if len(hoje) > 1:
                r["classe"], r["motivo"] = "nao_mensuravel_b", "multiplas batidas no minuto da celula"
                continue
            if hoje and amanha:
                if atravessa is True:
                    r["classe"], r["motivo"] = "nao_mensuravel_a", "virada e batidas nos dois dias"
                    continue
                if atravessa is None:
                    r["classe"], r["motivo"] = "NAO_CLASSIFICADA", "virada desconhecida com batidas nos dois dias"
                    continue
            if not hoje:
                r["classe"], r["motivo"] = "NAO_CLASSIFICADA", "sem batida viva no minuto da data da celula"
                continue

            b = hoje[0]
            r["batida"] = b
            r["instante"] = local_minuto(b.timestamp)
            if b.tipo not in ("E", "S"):
                r["classe"], r["motivo"] = "NAO_CLASSIFICADA", "tipo da batida desconhecido"
                continue

            if tem_outra(b, batidas, 5, False):
                t5_aux["5min_qualquer_tipo"] += 1
            if tem_outra(b, batidas, 10, True):
                t5_aux["10min_mesmo_tipo"] += 1

            tipo = d.get("tipo_dia")
            if tipo == "folga":
                r["classe"] = "F"
                continue
            if tipo != "trabalho":
                r["classe"], r["motivo"] = "NAO_CLASSIFICADA", "tipo_dia=" + str(tipo)
                continue

            celdia = d.get("celulas")
            marcos = d.get("marcos")
            if not isinstance(celdia, list) or not isinstance(marcos, list):
                r["classe"], r["motivo"] = "NAO_CLASSIFICADA", "marcos ou celulas ausentes"
                continue
            if not marcos:
                if not celdia:
                    r["classe"] = "T6"
                else:
                    r["classe"], r["motivo"] = "NAO_CLASSIFICADA", "ata agregada nao identifica marcos"
                continue

            dist, motivo = distancias_missing(d, cel.data, alvo)
            if dist is None:
                r["classe"], r["motivo"] = "NAO_CLASSIFICADA", motivo
            elif dist:
                r["classe"] = "T1" if min(dist) <= 90 else "T2"
            elif tem_outra(b, batidas, 10, True):
                r["classe"] = "T5"

        # T4: pareia E com a S seguinte entre ocorrencias ainda elegiveis.
        # Se uma ocorrencia nao decidida pode interferir, nao forcar T4/T3.
        por_dia = defaultdict(list)
        for r in registros:
            por_dia[r["cel"].data].append(r)
        for data_celula, itens in por_dia.items():
            pendentes = [r for r in itens if r["classe"] is None]
            incerto = any(
                r["classe"] in ("NAO_CLASSIFICADA", "nao_mensuravel_a",
                                 "nao_mensuravel_b", "sem_hora")
                for r in itens
            )
            if incerto:
                for r in pendentes:
                    r["classe"] = "NAO_CLASSIFICADA"
                    r["motivo"] = "outra orfa incerta pode interferir no pareamento T4"
                continue
            pendentes.sort(key=lambda r: (r["instante"], r["batida"].pk))
            entrada = None
            for r in pendentes:
                if r["batida"].tipo == "E":
                    if entrada is not None:
                        entrada["classe"] = "T3"
                    entrada = r
                elif entrada is not None:
                    entrada["classe"] = r["classe"] = "T4"
                    entrada = None
                else:
                    r["classe"] = "T3"
            if entrada is not None:
                entrada["classe"] = "T3"

        for r in registros:
            registrar(r["classe"], cid, r["cel"].data,
                      r["verde"], r["motivo"]) [nao verificado]
            emitir_nossa(cid, colab.empresa_id, r["cel"], r["valor"],  # ACRESCENTADA
                         r["classe"], r["motivo"], minutos_completos)  # ACRESCENTADA

    except Exception as exc:
        falha(etapa, exc, cid)
        if perm_negada(exc):
            print("PERMISSAO_DE_ESCRITA_NEGADA",
                  "chamada=" + etapa, "col=" + etiqueta,
                  "acao=parar_este_colab")
        # Se a ata ja foi contada, preserva todas as suas ocorrencias.
        if ocorrencias:
            for cel, valor in ocorrencias:
                registrar("NAO_CLASSIFICADA", cid, cel.data, None,
                          "erro na etapa " + etapa)
                emitir_nossa(cid, colab.empresa_id if colab is not None else "NAO_FEITO",  # ACRESCENTADA
                             cel, valor, "NAO_CLASSIFICADA",  # ACRESCENTADA
                             "erro na etapa " + etapa, minutos_completos)  # ACRESCENTADA
        else:
            universo_completo = False

print("TURNOS_REAIS_RETORNADOS", turnos_encontrados)
print("T5_AUX_5MIN_QUALQUER_TIPO", t5_aux["5min_qualquer_tipo"])
print("T5_AUX_10MIN_MESMO_TIPO", t5_aux["10min_mesmo_tipo"])
print("T5_AUX_ESCOPO=orfas_casadas_com_tipo_valido_antes_da_precedencia")

for classe in CLASSES:
    print("CLASSE", classe,
          "dias=" + str(len(dias[classe])),
          "pessoas=" + str(len(pessoas[classe])),
          "orfas=" + str(totais[classe]),
          "dia_verde_orfas=" + str(verdes[classe]),
          "verde_indeterminado=" + str(verdes[classe + "_verde_indeterminado"]))

for (classe, motivo), n in sorted(motivos.items()):
    print("MOTIVO", classe, "orfas=" + str(n), motivo)

soma = sum(totais.values())
print("TOTAL_ORFAS_ATA", total_ata,
      "universo_completo=" + str(universo_completo))
print("SOMA_CLASSES_E_EXCLUSOES", soma)
print("SOMA_CONFERE", soma == total_ata and universo_completo)
if universo_completo:
    print("DIFERENCA_PARA_1267", total_ata - 1267)
else:
    print("DIFERENCA_PARA_1267 [nao verificado]: universo incompleto")
print("ERROS=" + str(erros))
print("FIM_FERNANDO_SN")
```

## 18/11 21:22 · sessao 2197ad35232a · RAIA-ESPURIAS · leitura · origem=Code

A sonda abaixo está **não executada**; seus resultados e `ERROS=N` permanecem [não verificado]. Ela conserva as ocorrências duplicadas dos dois literais e usa a janela local `dia..dia+1` para as candidatas físicas. O leitor fornece a lâmpada lavrada, mas não sua PK; a PK emitida em `OCUPADO_PK` resulta do critério de unicidade solicitado. ([leitor_celula.py:114](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:114), [turnos.py:2868](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:2868))

```python
import json
from collections import Counter
from datetime import date, datetime, time, timedelta

from django.db import connection
from django.utils import timezone

from colaboradores.models import Colaborador
from escala.models import CelulaDia
from escala.services.leitor_celula import grade_da_celula
from ponto.management.commands.e6_oraculo import InsumosDoDono
from ponto.services.cxr_na_lista import cxr_lavrado
from ponto.turnos import batidas_apuraveis, turnos_do_colab


# Projecao literal: colaborador_id;dia_turno;hora_orfa;marco_proximo;papel_marco.
# A duplicata de col451 em 31/10 e intencional.
ATE5 = """\
col63;2026-10-31;19:05;19:00;S
col67;2026-11-14;23:59;00:00;S
col67;2026-11-10;00:01;00:00;S
col81;2026-11-11;00:05;00:00;S
col118;2026-11-13;12:05;12:00;S
col121;2026-10-30;02:04;02:00;S
col211;2026-11-16;21:00;21:00;S
col211;2026-10-29;21:00;21:00;S
col211;2026-10-25;02:55;03:00;S
col220;2026-11-13;06:00;06:00;S
col220;2026-11-11;06:00;06:00;S
col278;2026-11-09;11:58;12:00;S
col281;2026-11-09;06:58;07:00;S
col282;2026-11-02;07:03;07:00;S
col300;2026-10-21;06:00;06:00;S
col366;2026-11-09;02:00;02:00;S
col366;2026-11-09;03:00;03:00;E
col366;2026-11-09;07:00;07:00;S
col366;2026-11-05;02:00;02:00;S
col366;2026-11-05;03:00;03:00;E
col366;2026-11-05;07:00;07:00;S
col366;2026-11-03;02:00;02:00;S
col366;2026-11-03;03:00;03:00;E
col366;2026-11-03;07:00;07:00;S
col366;2026-10-30;02:00;02:00;S
col366;2026-10-30;03:00;03:00;E
col366;2026-10-30;07:00;07:00;S
col366;2026-10-28;02:00;02:00;S
col366;2026-10-28;03:00;03:00;E
col366;2026-10-28;07:00;07:00;S
col407;2026-10-23;07:00;07:00;S
col444;2026-11-16;13:55;14:00;E
col451;2026-11-14;07:01;07:00;S
col451;2026-11-12;07:02;07:00;S
col451;2026-11-10;06:59;07:00;S
col451;2026-11-06;07:02;07:00;S
col451;2026-11-04;06:59;07:00;S
col451;2026-11-02;06:59;07:00;S
col451;2026-10-31;07:01;07:00;S
col451;2026-10-31;07:01;07:00;S
col451;2026-10-29;06:58;07:00;S
col451;2026-10-25;06:58;07:00;S
col451;2026-10-21;06:55;07:00;S
col489;2026-11-10;00:00;00:00;S
col491;2026-10-26;02:02;02:00;S
col491;2026-10-26;03:02;03:00;E
col552;2026-11-04;19:02;19:00;S
col587;2026-10-22;19:03;19:00;S
col627;2026-10-22;02:00;02:00;S
col627;2026-10-22;03:00;03:00;E
col627;2026-10-22;07:00;07:00;S
col673;2026-10-21;06:57;07:00;S
col843;2026-11-08;03:00;03:00;S
col843;2026-11-08;04:03;04:00;E
col843;2026-11-08;07:01;07:00;S
col859;2026-11-09;07:00;07:00;S
col876;2026-11-07;06:55;07:00;S
col881;2026-11-13;12:00;12:00;S
col909;2026-11-07;12:00;12:00;S
col913;2026-10-26;01:00;01:00;S
col913;2026-10-26;02:00;02:00;E
col913;2026-10-26;07:01;07:00;S
col922;2026-11-01;07:00;07:00;S
col947;2026-10-31;13:00;13:00;E
col1002;2026-11-09;18:59;19:00;S
col1010;2026-11-05;05:59;06:00;S
col1031;2026-11-17;06:55;07:00;S
col1031;2026-11-15;00:01;00:00;E
col1031;2026-11-15;06:55;07:00;S
"""

# Projecao literal: colaborador_id;dia_turno;hora_orfa.
NOMINAIS = """\
col673;2026-10-21;06:57
col82;2026-10-21;07:01
col82;2026-10-22;06:56
col82;2026-10-23;06:58
col82;2026-10-24;06:58
col82;2026-10-25;06:50
col89;2026-10-25;06:55
col107;2026-10-25;07:01
col107;2026-10-25;15:38
col129;2026-10-25;17:30
col155;2026-10-25;17:28
col214;2026-10-29;21:13
col214;2026-10-30;13:54
col82;2026-10-30;06:56
col214;2026-11-01;06:59
col107;2026-11-01;07:54
col107;2026-11-01;09:02
col129;2026-11-01;17:30
col155;2026-11-01;17:29
col129;2026-11-02;17:31
col214;2026-11-04;14:08
col89;2026-11-08;06:55
col129;2026-11-08;17:30
col214;2026-11-15;07:00
col82;2026-11-15;09:55
col82;2026-11-15;14:05
col82;2026-11-15;15:00
col82;2026-11-15;18:22
col89;2026-11-15;06:55
col107;2026-11-15;07:05
col107;2026-11-15;09:15
col129;2026-11-15;17:30
col155;2026-11-15;17:29
col107;2026-11-17;10:42
"""

INI = date(2026, 10, 21)
FIM = date(2026, 11, 20)
ERROS = Counter()
parados = set()
colabs = {}
grades = {}
fisicas = {}


def emitir(*campos):
    # Os campos de saida sao produzidos pela propria sonda; nunca imprime
    # mensagem de excecao, nome, CPF ou matricula.
    print(";".join(str(x) for x in campos))


def erro(col, etapa, exc):
    ERROS[(etapa, type(exc).__name__)] += 1
    emitir("ERRO", col, etapa, type(exc).__name__)
    atual = exc
    vistos = set()
    while atual is not None and id(atual) not in vistos:
        vistos.add(id(atual))
        codigo = getattr(atual, "sqlstate", None) or getattr(atual, "pgcode", None)
        if codigo in ("42501", "25006") or isinstance(atual, PermissionError):
            parados.add(col)
            break
        atual = getattr(atual, "__cause__", None) or getattr(atual, "__context__", None)


def obter_colab(col):
    if col not in colabs:
        if not col.startswith("col") or not col[3:].isdigit():
            raise ValueError("identificador invalido")
        colabs[col] = Colaborador.objects.get(pk=int(col[3:]))
    return colabs[col]


def obter_dia(col, dia):
    chave = (col, dia)
    if chave not in grades:
        colaborador = obter_colab(col)
        cels = list(CelulaDia.objects.filter(colaborador=colaborador, data=dia))
        if len(cels) > 1:
            raise ValueError("celulas duplicadas")
        grade = grade_da_celula(colaborador, dia, dia)
        dias = [d for d in grade["dias"] if d["data"] == dia]
        if len(dias) != 1:
            raise ValueError("dia ausente ou duplicado na grade")
        grades[chave] = (cels[0] if cels else None, dias[0])
    return grades[chave]


def instante(dia):
    return timezone.make_aware(
        datetime.combine(dia, time.min), timezone.get_current_timezone()
    )


def obter_batidas(col, dia):
    chave = (col, dia)
    if chave not in fisicas:
        lo = instante(dia)
        hi = instante(dia + timedelta(days=2))
        # A porta usa fim inclusivo; o filtro final exclui a meia-noite
        # posterior ao segundo dia.
        qs = batidas_apuraveis(obter_colab(col), lo, hi)
        fisicas[chave] = sorted(
            (b for b in qs if b.timestamp < hi),
            key=lambda b: (b.timestamp, b.pk),
        )
    return fisicas[chave]


def minuto(valor):
    if not isinstance(valor, str) or len(valor) != 5 or valor[2] != ":":
        return None
    try:
        h, m = int(valor[:2]), int(valor[3:])
    except ValueError:
        return None
    if not (0 <= h < 24 and 0 <= m < 60):
        return None
    return 60 * h + m


def distancia(a, b):
    # Menor distancia circular entre horarios; o sinal indica orfa - marco.
    return (a - b + 720) % 1440 - 720


def retrato(b):
    local = timezone.localtime(b.timestamp)
    return "{}:{}:{}".format(b.pk, b.tipo, local.strftime("%H:%M:%S"))


def no_minuto(batidas, hhmm, tipo=None):
    return [
        b for b in batidas
        if timezone.localtime(b.timestamp).strftime("%H:%M") == hhmm
        and (tipo is None or b.tipo == tipo)
    ]


def lampada_crua(cel, indice):
    # Somente campos da lampada pertinentes a pergunta. A ata pode conter
    # outros dados; nao serializar a ata inteira.
    if cel is None or not isinstance(cel.ata, dict):
        return "None"
    lampadas = cel.ata.get("lampadas")
    if not isinstance(lampadas, list) or indice >= len(lampadas):
        return "None"
    item = lampadas[indice]
    if not isinstance(item, dict):
        return "None"
    chaves = ("tipo", "hora", "luz", "tipo_real", "acesa", "dispensada", "isencao")
    seguro = {k: item.get(k) for k in chaves if k in item}
    return json.dumps(seguro, ensure_ascii=True, separators=(",", ":"))


def secao_c():
    linhas_ate5 = [x.split(";") for x in ATE5.strip().splitlines()]
    linhas_nominais = [x.split(";") for x in NOMINAIS.strip().splitlines()]
    if len(linhas_ate5) != 69 or len(linhas_nominais) != 34:
        raise ValueError("contagem dos literais incorreta")

    for linha in linhas_ate5 + linhas_nominais:
        col, data_txt, orfa = linha[:3]
        if col in parados:
            emitir("NAO_FEITO", col, data_txt, orfa, "permissao_negada")
            continue

        try:
            dia = date.fromisoformat(data_txt)
            bs = obter_batidas(col, dia)
            candidatas_orfa = no_minuto(bs, orfa)
            emitir(
                "ORFA_FISICA", col, data_txt, orfa,
                ",".join(retrato(b) for b in candidatas_orfa),
            )
        except Exception as exc:
            erro(col, "ORFA_FISICA", exc)
            emitir("ORFA_FISICA", col, data_txt, orfa, "NAO_FEITO")
            emitir(
                "MARCO", col, data_txt, orfa, "", "", "NAO_DETERMINAVEL",
                "", "", "", "", "None",
            )
            continue

        try:
            cel, d = obter_dia(col, dia)
            alvo = minuto(orfa)
            celulas = d.get("celulas") or []
            opcoes = []
            for indice, luz in enumerate(celulas):
                if not isinstance(luz, dict):
                    continue
                previsto = minuto(luz.get("hora_prevista"))
                if previsto is not None and alvo is not None:
                    opcoes.append((abs(distancia(alvo, previsto)), indice, luz))

            marco_hh = papel = pk = tipo = dist = candidatos = ""
            crua = "None"
            estado = "NAO_DETERMINAVEL"

            if opcoes:
                menor = min(x[0] for x in opcoes)
                proximas = [x for x in opcoes if x[0] == menor]
                # Empate entre marcos nao autoriza escolher um ocupante.
                if len(proximas) == 1:
                    _, indice, luz = proximas[0]
                    marco_hh = luz.get("hora_prevista") or ""
                    papel = luz.get("papel") or luz.get("tipo") or ""
                    dist = distancia(alvo, minuto(marco_hh))
                    crua = lampada_crua(cel, indice)

                    if (
                        cel is not None
                        and orfa in (d.get("orfas") or [])
                        and not luz.get("dispensada")
                        and not luz.get("isencao")
                    ):
                        acesa = luz.get("acesa_lavrada")
                        if acesa is False:
                            estado = "VAZIO"
                        elif acesa is True:
                            hora_real = luz.get("hora")
                            tipo_real = luz.get("tipo_real")
                            if minuto(hora_real) is not None and tipo_real in ("E", "S"):
                                matches = no_minuto(bs, hora_real, tipo_real)
                                candidatos = ",".join(retrato(b) for b in matches)
                                if len(matches) == 1:
                                    estado = "OCUPADO_PK"
                                    pk = matches[0].pk
                                    tipo = matches[0].tipo
                                else:
                                    estado = "OCUPADO_SEM_PK"

            emitir(
                "MARCO", col, data_txt, orfa, marco_hh, papel, estado,
                pk, tipo, dist, candidatos, crua,
            )
        except Exception as exc:
            erro(col, "MARCO", exc)
            emitir(
                "MARCO", col, data_txt, orfa, "", "", "NAO_DETERMINAVEL",
                "", "", "", "", "None",
            )


def secao_d():
    pedidos = {
        "col673": ("2026-10-21", "2026-10-22"),
        "col129": ("2026-10-25", "2026-11-01", "2026-11-02",
                   "2026-11-08", "2026-11-15"),
        "col155": ("2026-10-25", "2026-11-01", "2026-11-02",
                   "2026-11-15"),
        "col82": ("2026-11-15",),
        "col107": ("2026-11-15",),
        "col89": ("2026-11-15",),
        "col214": ("2026-11-15",),
    }
    try:
        ler_cxr = cxr_lavrado()
    except Exception as exc:
        erro("GLOBAL", "cxr_lavrado", exc)
        for col, datas in pedidos.items():
            for data_txt in datas:
                emitir("DONO", col, data_txt, "NAO_FEITO", "NAO_FEITO")
        return

    agora = timezone.now()
    hoje = timezone.localtime(agora).date()
    for col, datas in pedidos.items():
        if col in parados:
            for data_txt in datas:
                emitir("DONO", col, data_txt, "NAO_FEITO", "NAO_FEITO")
            continue
        try:
            colaborador = obter_colab(col)
            cxr = ler_cxr(colaborador.pk, INI)
            ins = InsumosDoDono(
                colaborador, INI, FIM, hoje, agora=agora,
                sem_motor=True, cxr=cxr,
            )
        except Exception as exc:
            erro(col, "InsumosDoDono", exc)
            for data_txt in datas:
                emitir("DONO", col, data_txt, "NAO_FEITO", "NAO_FEITO")
            continue

        for data_txt in datas:
            if col in parados:
                emitir("DONO", col, data_txt, "NAO_FEITO", "NAO_FEITO")
                continue
            try:
                dia = date.fromisoformat(data_txt)
                _, d = obter_dia(col, dia)
                dono = ins.dono(dia)
                emitir("DONO", col, data_txt, dono, d.get("tipo_dia"))
            except Exception as exc:
                erro(col, "dono", exc)
                emitir("DONO", col, data_txt, "NAO_FEITO", "NAO_FEITO")


def secao_e():
    col = "col214"
    pk_alvo = 155565
    if col in parados:
        emitir("TURNO214", "NAO_FEITO", "permissao_negada")
        emitir("PAR214", pk_alvo, "NAO_FEITO")
        return

    try:
        colaborador = obter_colab(col)
        dia = date(2026, 11, 15)
        alvos = [b for b in no_minuto(obter_batidas(col, dia), "07:00")
                 if b.pk == pk_alvo
                 and timezone.localtime(b.timestamp).date() == dia]
        turnos = turnos_do_colab(
            colaborador, date(2026, 11, 14), date(2026, 11, 16)
        )
        for t in turnos:
            emitir(
                "TURNO214", t.data_turno,
                t.entrada.pk if t.entrada is not None else "None",
                t.saida.pk if t.saida is not None else "None",
                ",".join(str(b.pk) for b in t.batidas),
                t.aberto, t.cross_meianoite,
            )

        ocorrencias = []
        for t in turnos:
            e = t.entrada.pk if t.entrada is not None else None
            s = t.saida.pk if t.saida is not None else None
            if pk_alvo == e or pk_alvo == s:
                ocorrencias.append((e, s))

        if len(alvos) != 1 or len(ocorrencias) != 1:
            emitir("PAR214", pk_alvo, "NAO_DETERMINAVEL")
        else:
            e, s = ocorrencias[0]
            outro = s if e == pk_alvo else e
            emitir(
                "PAR214", pk_alvo,
                "pareada_com={}".format(outro) if outro is not None else "SOZINHA",
            )
    except Exception as exc:
        erro(col, "turnos_do_colab", exc)
        emitir("TURNO214", "NAO_FEITO", "erro")
        emitir("PAR214", pk_alvo, "NAO_FEITO")


try:
    if connection.settings_dict.get("NAME") != "sombra":
        raise RuntimeError("banco nao e sombra")
    for nome, funcao in (("C", secao_c), ("D", secao_d), ("E", secao_e)):
        try:
            funcao()
        except Exception as exc:
            erro("GLOBAL", "secao_" + nome, exc)
            emitir(nome, "NAO_FEITO", type(exc).__name__)
except Exception as exc:
    erro("GLOBAL", "guarda_sombra", exc)
    for nome in ("C", "D", "E"):
        emitir(nome, "NAO_FEITO", "guarda_sombra")
finally:
    print("ERROS={}".format(sum(ERROS.values())))
    print("FIM_FERNANDO_S54")
```

**C — o que não prova:** a PK encontrada pela coincidência de minuto e tipo não demonstra qual batida o casador efetivamente usou; empate entre marcos também fica indeterminado. O literal não demonstra que a ata da sombra ainda contém a mesma órfã; isso depende da execução. ([leitor_celula.py:114](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:114), [não verificado])

**D — o que não prova:** `None` é uma resposta possível quando a lavra necessária não cobre o caso e o motor está desligado; não equivale a um dono identificado. Os valores específicos permanecem [não verificado]. ([e6_oraculo.py:182](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/management/commands/e6_oraculo.py:182))

**E — o que não prova:** o resultado descreve os turnos devolvidos pela porta na janela pedida, não estabelece o estado atual da produção. O par de `155565` permanece [não verificado]. ([turnos.py:1850](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1850))

O envelope indicado não foi encontrado na cópia; a garantia de não escrever em `logs/` do host permanece [não verificado]. O script direto citado pelo pedido escreve arquivos temporários e mantém trilha, de modo que essa garantia depende do envelope. ([sonda_leitura.sh:52](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/bin/sonda_leitura.sh:52), [sonda_leitura.sh:217](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/bin/sonda_leitura.sh:217), [não verificado])

## 18/11 21:19 · sessao 572fd5abab45 · RAIA-ESPURIAS · leitura · origem=Code

Não concluí a montagem e a revisão da sonda única. As duas faltas apontadas na volta 1 foram respondidas pelo material desta volta; não seria correto repeti-las como bloqueios. As contagens de referência e os quatro estados de MARCO estão no dado fornecido. [não verificado]

Não vou apresentar um script parcial como se cumprisse o contrato. Em particular, ainda falta integrar e conferir a transcrição completa de `s1_v4`, os dois literais de casos, o tratamento de todas as lâmpadas e a emissão de erros sem expor dados pessoais. A execução na sombra e o resultado `ERROS=N` permanecem [não verificado].

O que consegui confirmar na cópia para orientar a conclusão:

- **NOSSA:** a porta `grade_da_celula` lê as células e devolve os dias com órfãs da ata; isso não estabelece, sem rodar a sonda, a igualdade das contagens com a referência fornecida. ([leitor_celula.py:350](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:350), [leitor_celula.py:418](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:418))
- **VIVA:** `grade_espelho_janela` é a porta de recálculo e `classificar_orfas` é a porta de classificação; uma execução na sombra não prova o resultado da extração de produção. ([utils.py:2334](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/utils.py:2334), [orfa_classe.py:53](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/orfa_classe.py:53))
- **MARCO:** o leitor transporta `acesa_lavrada`, hora, tipo real, dispensa e isenção, mas não uma PK de ocupante. A regra de candidato único fornecida permite emitir a PK segundo o critério pedido; não prova por si só a decisão interna do casador. ([leitor_celula.py:114](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:114))
- **DONO:** `InsumosDoDono.dono` pode retornar `None` com `sem_motor=True`; esse valor deve ser mantido. O resultado para cada caso continua [não verificado]. ([e6_oraculo.py:182](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/management/commands/e6_oraculo.py:182))
- **col214:** `turnos_do_colab` chama `parear_turnos` e retorna turnos com entrada, saída e batidas; o par da batida indicada continua [não verificado] sem a execução. ([turnos.py:43](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:43), [turnos.py:1850](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1850), [turnos.py:1997](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1997))

Também não verifiquei o `envelope.sh` na cópia. O `bin/sonda_leitura.sh` diretamente cria ou reescreve arquivos em `logs/` e acrescenta uma trilha; portanto, a exigência de não escrever em `logs/` do host depende do envelope mencionado no contrato. ([sonda_leitura.sh:115](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/bin/sonda_leitura.sh:115), [sonda_leitura.sh:248](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/bin/sonda_leitura.sh:248), [não verificado])

## 18/11 21:17 · sessao 9d4eba666fe9 · RAIA-ESPURIAS · leitura · origem=Code

Não consigo entregar uma sonda que satisfaça integralmente o contrato de **MARCO** e a conferência classe a classe com as evidências disponíveis. Por isso, não apresento um script incompleto como se estivesse pronto para a rodada na sombra.

O bloqueio principal é a identidade do ocupante. `grade_da_celula` devolve, para cada lâmpada da ata, a hora realizada, o tipo real e o estado, mas não devolve a PK da batida; tampouco uma hora, por si só, identifica uma batida quando há mais de uma candidata ([leitor_celula.py:114](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:114), [cartorio.py:699](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/services/cartorio.py:699)). Além disso, `acesa_lavrada` pode ser `None`, e há lâmpadas dispensadas ou de isenção; portanto, reduzir todos os casos a **OCUPADO por uma PK** ou **VAZIO** pode inventar uma conclusão ([leitor_celula.py:130](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:130)).

A referência fornecida informa **1.292 no total**, mas não traz as contagens de referência de cada classe do `s1_v4`; logo, a sonda pode calcular as contagens na sombra e a diferença do total, mas não determinar a diferença *por classe* em relação à rodada anterior. [não verificado]

O que as portas verificadas permitem medir e seus limites:

- **NOSSA:** a ata fornece as órfãs e a grade as reapresenta; a lógica integral de classificação está no `s1_v4` fornecido. O resultado seria o da sombra na nova execução, sem provar que coincide classe a classe com a rodada de referência ([leitor_celula.py:180](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:180)). A referência de 1.292 é do texto fornecido. [não verificado]
- **VIVA:** `grade_espelho_janela` e `classificar_orfas` permitem recalcular e classificar a grade, alimentada pelas batidas apuráveis; isso não reproduz por si só o estado ou o instante da extração de produção ([utils.py:2334](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/utils.py:2334), [orfa_classe.py:53](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/orfa_classe.py:53), [turnos.py:2868](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:2868)).
- **MARCO:** a ata permite relatar marcos, papéis, horas e estados lavrados disponíveis, além de candidatos físicos separados; não prova necessariamente qual PK ocupou um marco nem por que o casador tomou uma decisão ([leitor_celula.py:114](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:114)).
- **DONO:** `InsumosDoDono.dono(dia)` é a porta pedida; com `sem_motor=True`, pode devolver `None` quando falta a lavra necessária, e isso deve permanecer sem resposta ([e6_oraculo.py:128](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/management/commands/e6_oraculo.py:128), [e6_oraculo.py:182](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/management/commands/e6_oraculo.py:182)). Para col214, a porta de produção é `turnos_do_colab`, que alimenta `parear_turnos`; o resultado específico permanece [não verificado] sem execução ([turnos.py:1850](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1850), [turnos.py:1997](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1997)).

Para fechar a entrega sem aproximar, falta permitir um estado explícito **NÃO DETERMINÁVEL** em MARCO, com candidatos identificados como candidatos, e fornecer as contagens por classe da rodada `s1_v4` de referência. A execução e seus `ERROS=N` continuam [não verificado].

## 18/11 21:16 · sessao 03c313a57f5f · E4C-F9 · censo · origem=Code

A auditoria das perguntas e triagens mostra duas distinções necessárias para a contagem: a falta de controle em M6 estava dentro do tema pedido, mas não foi apontada; e o limite de formas indiretas do selo foi apontado e mantido em fila. Este último não cabe literalmente em nenhuma das classes propostas: a classe **(b)** exige descarte pela triagem, e a triagem o encaminhou ao opus. [anexos `construtor_c1.q`, `q.txt` da volta 1 e `triagem.txt` da volta 3: sem linhas numeradas; **não verificado como arquivo:linha**]

Os anexos foram fornecidos no texto da solicitação e não têm numeração de linhas. Abaixo os identifico pelo nome e item; **suas referências exatas em arquivo:linha são [não verificado]**. As referências ao código são da cópia examinada.

**Auditoria de quem perguntou e de quem triou**

- O brief c1 pediu, para cada M, divergência entre projeção e atos, controle sem divergência e o mutante que o caso mataria. A pergunta ao verificador da volta 1 também pediu exame de “selo/RED sem mordida”, mas seu mutante explícito era o retorno de **M2** à coluna, não a remoção do predicado de M6. [anexos `construtor_c1.q`, parágrafos RED e selo, e `q.txt` da volta 1, itens 3–4: **não verificado como arquivo:linha**]
- O brief c1 pediu `ALLOW-iii` no backfill e no limbo e, ao mesmo tempo, proibiu editar arquivos fora dos três comandos e testes. A triagem da volta 1 reconheceu o conflito como erro do brief; c2 autorizou somente comentários nos dois arquivos e repetiu as classes `ALLOW-iii / ALLOW-C`. [anexos `construtor_c1.q`, FICA e PROIBIDO; `triagem_ver1.txt`, B3; `construtor_c2.q`, B3: **não verificado como arquivo:linha**] [nao verificado]
- A triagem da volta 1 considerou procedente a mistura em M7 e escolheu a guarda restritiva; a da volta 2 aceitou o fechamento de B1 e B3; a da volta 3 preservou em **FILA** as formas indiretas não vistas pelo selo. [anexos `triagem_ver1.txt`, B1; `triagem.txt` da volta 2, item 1; `triagem.txt` da volta 3, item 3: **não verificado como arquivo:linha**]

**1. M6: mutante sem predicado sobrevive — classe (a).**  
**Pergunta:** “para cada M, um caso onde PROJECAO e SEQUENCIA divergem”; “Controle”; “Diga o mutante de cada M” e, ao verificador, “selo/RED sem mordida”. O mutante pedido no brief era voltar ao filtro da coluna; a remoção completa do predicado não foi especificada. [anexos `construtor_c1.q`, RED, e `q.txt` da volta 1, item 4: **não verificado como arquivo:linha**]

**Resposta:** o construtor disse “inclui controles” e propôs para M6 o mutante que troca a face pelo filtro da coluna; o verificador disse que “o teste novo cobre o universo em dry-run”. **Não apontaram** a ausência de pergunta validada que deveria sair nem o mutante sem predicado sobrevivente. Na cópia, M6 cria a divergente e uma controle e espera contagem 2; M2–M5 também criam uma pergunta validada e verificam sua exclusão. [anexos `construtor_c1.r` e `r.txt` da volta 1, item 2: **não verificado como arquivo:linha**; código: [teste M6](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:96), [controle M2–M5](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:73)]

A sobrevivência relatada pelo opus é resultado do anexo; não há execução independente nesta auditoria. [anexo `veredito.md`, achado 1: **não verificado como arquivo:linha**]

**2. M1: falta controle positivo — classe (a).**  
**Pergunta:** o mesmo brief pediu controle e mutante para **cada M**; a pergunta da volta 1 incluiu “selo/RED sem mordida”. [anexos `construtor_c1.q`, RED, e `q.txt` da volta 1, item 4: **não verificado como arquivo:linha**]

**Resposta:** o construtor identificou `test_M1_rotulo_e_controle` como caso que deveria matar o mutante de retorno à coluna, mas **não apontou** a falta do controle positivo nem o ramo que sempre retorna `None`. O teste na cópia verifica apenas que as duas saídas **não** contêm `validada_em 1`. [anexo `construtor_c1.r`, tabela de mutantes: **não verificado como arquivo:linha**; [teste M1](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:85)]

**3. Comentários ALLOW incorretos ou incompletos — classe (a), para o defeito específico final.**  
**Pergunta:** c1 determinou `ALLOW-iii` para o backfill e os três sítios do limbo; c2 pediu comentários “como no brief c1”; o verificador da volta 2 foi perguntado se os achados da triagem estavam fechados e como o selo tratava comentários. [anexos `construtor_c1.q`, FICA; `construtor_c2.q`, B3; `q.txt` da volta 2, itens 1 e 3: **não verificado como arquivo:linha**]

**Resposta:** na volta 1, o verificador **viu a falta de comentários** e a triagem a considerou procedente; depois, o construtor disse ter acrescentado comentários e o verificador da volta 2 disse “B3 — PROCEDE o acréscimo dos dois comentários”. Ele também viu que o selo F9 não lê esses comentários. **Não apontou** que o backfill estava rotulado `ALLOW-C` contra o pedido `ALLOW-iii`, nem a cobertura por sítio descrita pelo opus. A triagem da volta 2 marcou B3 “comentarios: OK”. [anexos `r.txt` da volta 1, FICA; `triagem_ver1.txt`, B3; `construtor_c2.r`, B3; `r.txt` da volta 2, itens 1 e 3; `triagem.txt` da volta 2, item 1: **não verificado como arquivo:linha**; [comentário do backfill](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/management/commands/backfill_decisoes_pergunta.py:56), [outro sítio do backfill](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/management/commands/backfill_decisoes_pergunta.py:104), [comentário do limbo](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/management/commands/curar_perguntas_em_limbo.py:46)]

Não classifico o defeito específico como **(b)**: a triagem aceitou como fechado B3, mas os textos não mostram que ela tenha identificado e descartado o rótulo errado ou a falta de cobertura por sítio. [anexo `triagem.txt` da volta 2, item 1: **não verificado como arquivo:linha**]

**4. RED de c3 sem log Django — classe (c), alcance de execução.**  
**Pergunta:** c3 pediu mutantes para cada forma nova que fizessem o selo falhar; a volta 3 perguntou se cada forma tinha mutante que fazia o selo falhar. **Não pediu explicitamente um log Django do RED.** [anexos `construtor_c3.q` e `q.txt` da volta 3, item 1: **não verificado como arquivo:linha**]

**Resposta:** o construtor declarou: “O RED isolado mostrou” as formas escapando antes da cura e informou que a execução pelo Django ficou impedida; o verificador relatou 34/34 mutantes em memória e marcou a suíte Django completa como não verificada. Assim, **viram e declararam o limite de execução**, mas o dado não contém o log Django de RED apontado pelo opus. [anexos `construtor_c3.r`, parágrafo RED, e `r.txt` da volta 3, item 1: **não verificado como arquivo:linha**] [nao verificado]

**5. Formas indiretas escapam ao selo — sem classe aplicável literalmente.**  
**Pergunta:** a volta 3 perguntou expressamente se restavam formas como “variavel com o nome do campo, values(*lista), Meta/only via constante de modulo” e se os comandos as usavam. [anexo `q.txt` da volta 3, item 3: **não verificado como arquivo:linha**]

**Resposta:** o verificador **viu** variável, lista expandida, constante e `__dict__` fora dos padrões, e relatou não encontrar essas formas nos três comandos atuais. A triagem registrou “**FILA, anexo ao opus**”; o próprio opus concordou com ela. Portanto, não é (b): houve encaminhamento em fila, não descarte. Tampouco é (a), (c), (d) ou (e). [anexos `r.txt` da volta 3, item 3; `triagem.txt` da volta 3, item 3; `veredito.md`, achado 5: **não verificado como arquivo:linha**; [padrões reconhecidos pelo selo](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:145)]

**6. Guarda M7 e mutante equivalente da impressão — classe (b) para a mistura aceita; componente do mutante: (e).**  
**Pergunta:** a volta 1 pediu divergência entre leitores e derivação paralela; c2 determinou expressamente a guarda restritiva e pediu o mutante que **retira a guarda**. A volta 2 perguntou se B1 estava fechado. **Não foi perguntado** se o mutante da impressão era equivalente dentro da classe A ou se morria apenas pelo selo. [anexos `q.txt` da volta 1, itens 4–5; `construtor_c2.q`, B1; `q.txt` da volta 2, item 1: **não verificado como arquivo:linha**]

**Resposta e triagem:** o verificador da volta 1 **viu a mistura** entre leitura pelo ato e classificação pela projeção; a triagem reconheceu B1 e determinou a guarda como “cura ... mais RESTRITIVA”; o verificador da volta 2 considerou fechada a guarda solicitada. Para essa parte, **(b)** representa a aceitação explícita da composição pela triagem, embora ela a tenha tratado como cura, não simplesmente ignorado o achado. A equivalência do mutante da impressão **não foi vista nem especificamente perguntada: (e)**. [anexos `r.txt` da volta 1, item 5; `triagem_ver1.txt`, B1; `r.txt` da volta 2, item 1: **não verificado como arquivo:linha**; [guarda](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/management/commands/validar_classe_a.py:63), [impressão](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/management/commands/validar_classe_a.py:81)]

| Achado | Classe para o censo |
|---|---|
| 1 — M6 sem controle que exclui | **(a)** |
| 2 — M1 sem controle positivo | **(a)** |
| 3 — ALLOW errado / cobertura por sítio | **(a)** |
| 4 — RED c3 sem log Django | **(c)** |
| 5 — formas indiretas | **Sem classe aplicável:** visto e mantido em fila |
| 6 — guarda M7 / mutante da impressão | **(b)** para a guarda; **(e)** para o mutante |

Contando **achados**, com o 6 atribuído a **(b)** pela decisão de triagem sobre a guarda: **(a) 3; (b) 1; (c) 1; (d) 0; (e) 0; sem classe aplicável 1**. O componente não perguntado do achado 6 está registrado separadamente acima e não é contado uma segunda vez. Os achados 1–3 também são casos em que nenhum dos textos de resposta anexados apontou o defeito específico; foram atribuídos a **(a)** porque o tema estava presente na pergunta. [anexos citados nos achados 1–6: **não verificado como arquivo:linha**]

## 18/11 20:46 · sessao 153092d6b9cc · E4C-F6F7 · verificador · origem=Code

1. **PROCEDE para os acessos que `DecisaoPergunta.leitura` faz hoje; NÃO PROCEDE como equivalência integral dos objetos.** O delta fornecido mostra que c15 cortava os dois carimbos somente quando posteriores a `agora`; a vista aplica a mesma condição. A consulta mantém as decisões não excluídas até o horizonte, ordenadas por pergunta, instante e PK. A vista recebe essas mesmas decisões, e `leitura` as converte em lista e volta a ordenar a sequência combinada com a projeção. [tarefa_do_dia.py:390](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/tarefa_do_dia.py:390), [tarefa_do_dia.py:403](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/tarefa_do_dia.py:403), [models.py:4308](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/models.py:4308).

   Nos corpos atuais, `leitura` e `linhas_da_projecao_sem_linha` usam `pk`, `decisoes.all()`, os dois carimbos, `resposta_colab` e `validada_por_id` da pergunta; a vista entrega esses valores por delegação, com o corte declarado. Esses corpos não consultam `_state`, `_prefetched_objects_cache`, identidade ou `isinstance` da pergunta. [models.py:4251](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/models.py:4251), [models.py:4308](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/models.py:4308), [tarefa_do_dia.py:414](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/tarefa_do_dia.py:414).

   A vista é outro objeto, não uma instância da classe da pergunta; `decisoes.all()` retorna tupla, e o acesso a `_prefetched_objects_cache` delegaria ao objeto original, em vez de fornecer o cache substituído da cópia c15. Essas diferenças não afetam os acessos examinados, mas impedem afirmar identidade geral ou equivalência para leitores futuros. [tarefa_do_dia.py:403](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/tarefa_do_dia.py:403), [models.py:4308](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/models.py:4308).

2. **NÃO PROCEDE que a vista acrescente uma exceção ao selo.** Atribuição direta ao par e `setattr(p, 'respondida_em', ...)` ou `setattr(p, 'resposta_colab', ...)` em `tarefa_do_dia.py` continuam detectáveis e fora dos sítios permitidos. A vista usa `object.__setattr__` apenas para seus três campos internos e rejeita atribuição comum. [test_e3_porta_A_escritor_do_par.py:124](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e3_porta_A_escritor_do_par.py:124), [test_e3_porta_A_escritor_do_par.py:203](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e3_porta_A_escritor_do_par.py:203), [tarefa_do_dia.py:403](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/tarefa_do_dia.py:403).

   **Limite do selo:** `object.__setattr__(p, 'respondida_em', valor)` escapa dessa detecção, pois o scanner examina o primeiro argumento de `__setattr__`, que nessa forma é `p`, não o nome do campo. Esse limite não foi criado pela vista, mas impede afirmar que *toda forma possível* de escrever o par está coberta. [test_e3_porta_A_escritor_do_par.py:140](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e3_porta_A_escritor_do_par.py:140).

   O anexo relata a falha de c15 no `setattr` da cópia; resultado da suíte completa para c16 **[não verificado]**. [nao verificado]

3. **PROCEDE para estes quatro mutantes na execução local; NÃO PROCEDE que o teste garanta a origem do `AssertionError`.** Cada padrão apareceu uma vez e as quatro fontes mutadas compilaram. Na execução local em memória, falharam respectivamente: [nao verificado]

   - `>=`: na verificação da validação exatamente no horizonte. [teste:150](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:150).
   - `if False`: no resultado esperado para validação futura. [teste:149](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:149).
   - Inclusão da decisão futura: na igualdade da sequência cortada. [teste:147](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:147).
   - Escrita na pergunta: no bloqueio explícito de escrita do carimbo. [teste:118](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:118).

   O `assertRaises(AssertionError)` envolve o teste inteiro, sem conferir mensagem ou local: outro `AssertionError` dentro dele também satisfaria a catraca. `ImportError` e `SyntaxError`, por si, não a satisfazem. [teste:174](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:174).

4. **Custo ou efeito colateral relevante em lote: [não verificado].** A classe é criada uma vez por chamada de `_fatos_das_perguntas`, fora do laço; cada pergunta recebe uma instância e uma tupla das suas decisões mantidas. Há, portanto, custo adicional de alocação proporcional às perguntas e decisões, mas o impacto prático não foi medido. [tarefa_do_dia.py:403](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/tarefa_do_dia.py:403), [tarefa_do_dia.py:425](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/tarefa_do_dia.py:425).

5. **Fluxo dos valores do delta:**

   - **Projeção `respondida_em` / `resposta_colab`:** os sítios e delegações autorizados estão enumerados no selo; as portas de resposta humana, automática e pela trilha aparecem em `disputa_emissao.py`. A vista lê o carimbo e delega o texto; a leitura usa ambos para construir o passivo sem linha correspondente. [selo:40](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e3_porta_A_escritor_do_par.py:40), [disputa_emissao.py:1481](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/disputa_emissao.py:1481), [disputa_emissao.py:1682](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/disputa_emissao.py:1682), [disputa_emissao.py:1701](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/disputa_emissao.py:1701), [models.py:4259](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/models.py:4259).
   - **`validada_em` / `validada_por_id`:** a porta de validação grava o carimbo e registra a decisão; outras operações o removem. A vista corta o carimbo futuro, e a projeção sem linha lê também o identificador de quem validou. [validacao.py:168](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/validacao.py:168), [validacao.py:273](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/validacao.py:273), [models.py:4265](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/models.py:4265).
   - **Decisões e resultado:** `registrar` persiste as linhas; a carga lê e corta a sequência, a vista a entrega à `leitura`, e o mapa resultante é consumido por `_pela_decisao`. O passivo é calculado separadamente a partir das perguntas originais e também cortado por instante. [models.py:4328](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/models.py:4328), [tarefa_do_dia.py:390](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/tarefa_do_dia.py:390), [tarefa_do_dia.py:425](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/tarefa_do_dia.py:425), [tarefa_do_dia.py:471](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/tarefa_do_dia.py:471).

## 18/11 20:46 · sessao b0fefc5da3c6 · - · - · origem=Ronald (tmux)

Os quatro mutantes falharam pelos motivos esperados em execução local, em memória e sem banco:

- `>=` falhou na verificação do carimbo exatamente no horizonte ([teste:150](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:150)).
- `if False` falhou no resultado esperado para validação futura ([teste:149](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:149)).
- `if True` incluiu a decisão futura e falhou na comparação da sequência ([teste:147](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:147)).
- O `setattr` inserido acionou o bloqueio explícito de escrita ([teste:118](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:118)).

**Não procede** que o teste garanta sempre a causa correta: o `assertRaises(AssertionError)` aceita qualquer `AssertionError` emitido durante a chamada inteira, inclusive de outro assert ([teste:181](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:181)). `ImportError` e `SyntaxError` não satisfazem esse contexto; a fonte mutada é compilada em memória ([e4_apoio.py:8](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/e4_apoio.py:8)).

## 18/11 20:45 · sessao be0b0c59d597 · - · - · origem=Ronald (tmux)

Verificação do selo e do fluxo dos valores enviada ao agente principal, incluindo a limitação de detecção de `object.__setattr__` em [test_e3_porta_A_escritor_do_par.py:140](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/tests/test_e3_porta_A_escritor_do_par.py:140).

## 18/11 20:45 · sessao 07e237c42604 · - · - · origem=Ronald (tmux)

A vista preserva os valores usados pela implementação atual de `DecisaoPergunta.leitura`, com as diferenças de objeto e de tipo informadas ao agente principal. A equivalência foi verificada por inspeção de [models.py](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/models.py:4251) e [tarefa_do_dia.py](/home/ronald/codex_ws/0dc6512a858bccb518a415d7e00c8fe669d945b0/app/chamados/services/tarefa_do_dia.py:390).

## 18/11 20:44 · sessao 6f6dae60755f · E4C-F6F7 · construtor · origem=Code

A cura está no diff local: `_fatos_das_perguntas` entrega a `DecisaoPergunta.leitura` uma vista somente de leitura. Ela delega os atributos à pergunta, oculta os dois carimbos posteriores a `agora` e fornece as decisões já cortadas; a igualdade permanece no horizonte ([tarefa_do_dia.py:398](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/services/tarefa_do_dia.py:398), [tarefa_do_dia.py:402](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/services/tarefa_do_dia.py:402)).

A verificação pedida de `models.py` confirma que `leitura` lê `p.decisoes.all()` e passa a pergunta ao auxiliar; o auxiliar lê os atributos da projeção e constrói linhas em variáveis locais, sem escrever na pergunta ([models.py:4308](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/models.py:4308), [models.py:4251](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/models.py:4251)).

O teste de horizonte bloqueia escrita dos carimbos mesmo em cópia e verifica que o original segue intacto ([teste:118](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:118), [teste:138](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:138)). Na base c15, ele ficou **RED para ambos os carimbos**; com a cura, o módulo passou com 7 testes. Outros 13 testes relacionados sem banco passaram. O corpo do selo do par passou em execução sem Django, com substitutos de infraestrutura; nele, o mutante que reinsere `setattr(p, 'respondida_em', None)` foi detectado. O selo cobre essa forma de escrita, mas declara `obj.__dict__` fora de alcance ([selo:23](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e3_porta_A_escritor_do_par.py:23)). [nao verificado]

Os quatro mutantes — fronteira `>=`, ausência do corte dos carimbos, inclusão de decisão futura e retorno da escrita — são executados pelo mesmo comando ([teste:161](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:161)): [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4c_f6f7_c14_lei3_pela_leitura.HorizonteProjecao.test_mutantes_da_vista_morrem
```

Para reproduzir o módulo completo e consultar o diff:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4c_f6f7_c14_lei3_pela_leitura
git diff -- chamados/services/tarefa_do_dia.py chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py
```

**LEI-AKITA:** padrão = juiz único; a vista apenas adapta os fatos ao horizonte e conserva `DecisaoPergunta.leitura` como autoridade da leitura ([tarefa_do_dia.py:427](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/services/tarefa_do_dia.py:427)). Suite Django e certificação pela pista: **[não verificado]**. [nao verificado]

## 18/11 20:40 · sessao 9255151745e5 · E4C-F9 · verificador · origem=Code

1. **PROCEDE, para as formas literais descritas.** O selo conta `getattr`, `setattr` e `hasattr` quando o nome do campo é uma string literal no segundo argumento posicional; também conta chaves literais de um dicionário expandido com `**` diretamente em qualquer chamada, incluindo sufixos `__lookup` ([selo, linhas 156–174](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:156)). Há mutantes para as três funções, para `getattr` com e sem valor padrão, para chave simples em outra chamada e para lookup em `filter`, com ambos os campos; eles exigem contagem diferente da base ([selo, linhas 207–226](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:207)). Executei a lógica desses 34 mutantes em memória: **34/34 alteraram a contagem**. [não verificado: execução da suíte Django completa]

2. **NÃO PROCEDE que o delta cause falso positivo nos três comandos atuais ou deixe de contar alguma forma anterior.** Reconstituindo c2 a partir do patch fornecido e executando ambas as versões em memória sobre os arquivos atuais, as contagens foram iguais: duas ocorrências no auditor, nenhuma em `encerrar_perguntas_com_fato` e uma em `validar_classe_a`; são exatamente as três permitidas ([selo, linhas 194–205](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:194); [auditor, I18](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/management/commands/auditar_invariantes_chamados.py:1084); [auditor, I20](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/management/commands/auditar_invariantes_chamados.py:1116); [validador, guarda](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/management/commands/validar_classe_a.py:63)). A alteração acrescenta condições à condição anterior, sem retirar as anteriores ([selo, linhas 166–174](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:166)). Em código futuro, contudo, a detecção é sintática: uma chamada ou objeto sem relação com a coluna pode ser contado se usar esses literais ([selo, linhas 146–174](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:146)).

3. **PROCEDE: restam formas não vistas.** Confirmei em memória que o selo retorna contagem vazia para `nome = 'validada_em'; getattr(q, nome)`, `values(*['validada_em'])`, `CAMPO = 'validada_em'; only(CAMPO)` e `q.__dict__['validada_em']`. A análise exige formatos específicos de nós e não resolve variáveis ou listas expandidas ([selo, linhas 148–174](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:148)). Uma referência ao nome em `Meta` via constante também fica fora desses padrões ([selo, linhas 156–174](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:156)).

   Nos três comandos atuais, **não encontrei uso dessas formas para os dois campos**; as ocorrências literais relevantes encontradas são as exceções citadas e acessos através de `DecisaoPergunta.leitura` ([auditor](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/management/commands/auditar_invariantes_chamados.py:206); [validador](/home/ronald/codex_ws/d3e9544fd122a0a43b87d0afa8ef074ab0e160ad/app/chamados/management/commands/validar_classe_a.py:63)). Nomes construídos dinamicamente ou recebidos de fora desses arquivos: **[não verificado]**.

## 18/11 20:38 · sessao dea090c8635b · E4C-F9 · construtor · origem=Code

O diff está em [test_e4c_f9_comandos_leem_o_ato.py](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:141); a base da cópia é `c939f3bfefb7d2844c4eb0ae9a22c7388efa4696`. Os comandos de produção não foram alterados (`git status --short`).

- `getattr`, com ou sem default, `setattr` e `hasattr`: reconhecimento em [linha 168](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:168); mutantes em [linha 216](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:216).
- Chaves de dict literal expandido em qualquer chamada, com campo ou campo e lookup: reconhecimento em [linha 162](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:162); mutantes em [linha 218](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:218).
- String com lookup nas chamadas já reconhecidas: normalização em [linha 172](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:172); mutante adicional em [linha 220](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:220). Essa forma já era reconhecida antes do diff.

O RED isolado mostrou que as seis primeiras formas novas escapavam antes da cura; após a alteração, os dois métodos do selo passaram isolados, incluindo os mutantes para ambos os campos e as contagens dos comandos reais. A execução pelo Django ficou impedida: `python3` não encontrou `django.core`. Certificação pela suíte Code e pelo opus: **[não verificado]**. [nao verificado]

O comando abaixo executa **todos os mutantes**, cada um como subteste, e o selo sobre os comandos reais, sem Django nem banco:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import ast
import unittest
from pathlib import Path

p = Path('chamados/tests/test_e4c_f9_comandos_leem_o_ato.py')
tree = ast.parse(p.read_text())
selected = [n for n in tree.body
            if isinstance(n, (ast.FunctionDef, ast.ClassDef))
            and n.name in ('leituras_cruas', 'SeloProjecaoTest')]
ns = {'ast': ast, 'SimpleTestCase': unittest.TestCase,
      'COMANDOS': Path('chamados/management/commands')}
exec(compile(ast.Module(body=selected, type_ignores=[]), str(p), 'exec'), ns)
suite = unittest.defaultTestLoader.loadTestsFromTestCase(ns['SeloProjecaoTest']) [nao verificado]
result = unittest.TextTestRunner(verbosity=2).run(suite) [nao verificado]
raise SystemExit(not result.wasSuccessful())
PY
```

Para inspecionar o diff: `git diff -- chamados/tests/test_e4c_f9_comandos_leem_o_ato.py`.

LEI-AKITA: origem=[leituras_cruas:141](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:141); RED e mutantes=[teste:207](/home/ronald/wt-codex-e4c-f9-191111/app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py:207); padrao=n/a (instrumento). [nao verificado]

## 18/11 20:36 · sessao 3b6a85200046 · E4C-F9 · verificador · origem=Code

**Resultado: NÃO PROCEDE que todos os achados da volta 1 estejam fechados.** A guarda de B1 e os comentários de B3 estão no delta; B2 foi parcialmente tratado, mas ainda há leituras por string que o selo F9 não detecta ([validar_classe_a.py:63](app/chamados/management/commands/validar_classe_a.py#L63), [test_e4c_f9_comandos_leem_o_ato.py:141](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L141), [backfill_decisoes_pergunta.py:56](app/chamados/management/commands/backfill_decisoes_pergunta.py#L56), [curar_perguntas_em_limbo.py:46](app/chamados/management/commands/curar_perguntas_em_limbo.py#L46)).

1. **Fechamento dos achados da triagem**

   - **B1 — PROCEDE o fechamento da guarda solicitada.** A divergência é contada e interrompe o processamento antes da classificação e da inclusão na lista validável; o teste exercita `--apply` com uma divergente e uma controle ([validar_classe_a.py:63](app/chamados/management/commands/validar_classe_a.py#L63), [validar_classe_a.py:100](app/chamados/management/commands/validar_classe_a.py#L100), [test_e4c_f9_comandos_leem_o_ato.py:122](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L122)). A saída identifica a **contagem**, não imprime a identidade de cada divergente ([validar_classe_a.py:77](app/chamados/management/commands/validar_classe_a.py#L77)).
   - **B2 — NÃO PROCEDE o fechamento integral.** O delta acrescenta detecção de várias strings e contagem por ocorrência, mas deixa passar, por exemplo, `getattr(q, 'resposta_colab')` e `filter(**{'resposta_colab': 'x'})`: `getattr` e `filter` não estão na lista de chamadas reconhecidas para constantes string ([test_e4c_f9_comandos_leem_o_ato.py:146](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L146), [test_e4c_f9_comandos_leem_o_ato.py:156](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L156)).
   - **B3 — PROCEDE o acréscimo dos dois comentários**, exatamente nos arquivos indicados ([backfill_decisoes_pergunta.py:56](app/chamados/management/commands/backfill_decisoes_pergunta.py#L56), [curar_perguntas_em_limbo.py:46](app/chamados/management/commands/curar_perguntas_em_limbo.py#L46)). O efeito desses comentários no selo é tratado no item 3.
   - A inclusão do censo da fila LEI-AKITA no commit c2: **[não verificado]**. O anexo de delta fornecido não mostra esse censo [não verificado]. [nao verificado]

2. **Contagem e universo — PROCEDE.** A comparação usa o valor devolvido por `DecisaoPergunta.leitura(q).resposta_colab` e a coluna `q.resposta_colab` ([validar_classe_a.py:63](app/chamados/management/commands/validar_classe_a.py#L63)). Essa leitura não é necessariamente composta só de linhas reais: ela também incorpora, como linhas faltantes, valores da projeção sem correspondência na sequência ([models.py:4295](app/chamados/models.py#L4295), [models.py:4308](app/chamados/models.py#L4308)).

   **PROCEDE:** o `continue` retira cada divergente da classe que o classificador lhe atribuiria — A, B, C ou `(sem hora)` — e a coloca no novo contador ([validar_classe_a.py:63](app/chamados/management/commands/validar_classe_a.py#L63), [juizes.py:2034](app/chamados/juizes.py#L2034)). **PROCEDE:** somando o novo contador às demais classes, cada entrada de `pendentes` incrementa exatamente uma classe, desde que o laço termine sem exceção; esse universo é a lista construída pelo comando, não uma contagem verificada de toda a produção ([validar_classe_a.py:44](app/chamados/management/commands/validar_classe_a.py#L44), [validar_classe_a.py:60](app/chamados/management/commands/validar_classe_a.py#L60)). `--limite` corta a lista de classe A depois da contagem ([validar_classe_a.py:71](app/chamados/management/commands/validar_classe_a.py#L71)).

3. **Comentários ALLOW — NÃO PROCEDE que o selo os aceite por texto livre.** O selo F9 analisa a AST de apenas três comandos; os dois arquivos que receberam comentários não integram essa lista, e os comentários não fazem parte da AST analisada ([test_e4c_f9_comandos_leem_o_ato.py:143](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L143), [test_e4c_f9_comandos_leem_o_ato.py:196](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L196)). A catraca E3, por sua vez, declara as exceções por **caminho e função**, com contagem por sítio ([test_e3_catraca_respondedores.py:66](app/chamados/tests/test_e3_catraca_respondedores.py#L66), [test_e3_catraca_respondedores.py:81](app/chamados/tests/test_e3_catraca_respondedores.py#L81), [test_e3_catraca_respondedores.py:393](app/chamados/tests/test_e3_catraca_respondedores.py#L393)).

   **NÃO PROCEDE** que colar o mesmo comentário em outro comando conceda exceção: o texto não altera a chave declarada na E3 nem as permissões da F9 ([test_e3_catraca_respondedores.py:66](app/chamados/tests/test_e3_catraca_respondedores.py#L66), [test_e4c_f9_comandos_leem_o_ato.py:189](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L189)).

4. **Selo F9 — PROCEDE que varre AST**, construída com `ast.parse`, para os dois campos e três arquivos explicitamente enumerados ([test_e4c_f9_comandos_leem_o_ato.py:141](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L141), [test_e4c_f9_comandos_leem_o_ato.py:196](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L196)).

   **NÃO PROCEDE que haja caso que morde para cada forma perguntada.** Há mutantes para `values`, `values_list`, `only`, `defer`, `order_by`, `F`, `Q` e `annotate`, além de argumentos nomeados extras dentro de `add`; não há mutante de `getattr`, e essa forma não é detectada pelo analisador ([test_e4c_f9_comandos_leem_o_ato.py:147](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L147), [test_e4c_f9_comandos_leem_o_ato.py:202](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L202)). Atributos diretos são analisados, mas o teste de mutação apresentado não injeta um atributo direto; `filter(resposta_colab=...)` é reconhecido como keyword, enquanto a chave string em `filter(**{...})` escapa ([test_e4c_f9_comandos_leem_o_ato.py:150](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L150), [test_e4c_f9_comandos_leem_o_ato.py:154](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L154), [test_e4c_f9_comandos_leem_o_ato.py:161](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L161)).

   **PROCEDE com limite:** uma ocorrência nova de uma forma **reconhecida** em comando varrido altera a contagem ou cria chave e falha na igualdade; o mutante verifica ocorrências extras dentro de `add`. Uma forma não reconhecida, como `getattr`, não é pega por essa contagem ([test_e4c_f9_comandos_leem_o_ato.py:184](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L184), [test_e4c_f9_comandos_leem_o_ato.py:196](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L196), [test_e4c_f9_comandos_leem_o_ato.py:217](app/chamados/tests/test_e4c_f9_comandos_leem_o_ato.py#L217)).

5. **Campos envolvidos — PROCEDE a seguinte rastreabilidade dos caminhos examinados; inventário completo de todos os escritores e leitores da aplicação: [não verificado].**

   | Campo | Escrita e leitura relevantes |
   |---|---|
   | `PerguntaDisputa.resposta_colab` | A porta de resposta atribui e salva a coluna; a reabertura prepara seu esvaziamento ([disputa_emissao.py:1481](app/chamados/services/disputa_emissao.py#L1481), [disputa_emissao.py:1529](app/chamados/services/disputa_emissao.py#L1529), [disputa_emissao.py:1617](app/chamados/services/disputa_emissao.py#L1617)). A guarda nova e o classificador leem a coluna; backfill e cura também a leem ([validar_classe_a.py:63](app/chamados/management/commands/validar_classe_a.py#L63), [juizes.py:2041](app/chamados/juizes.py#L2041), [models.py:4260](app/chamados/models.py#L4260), [curar_perguntas_em_limbo.py:47](app/chamados/management/commands/curar_perguntas_em_limbo.py#L47)). |
   | `respondida_em` | A porta de resposta e a cura pela trilha o gravam; a reabertura prepara sua limpeza ([disputa_emissao.py:1528](app/chamados/services/disputa_emissao.py#L1528), [disputa_emissao.py:1705](app/chamados/services/disputa_emissao.py#L1705), [disputa_emissao.py:1617](app/chamados/services/disputa_emissao.py#L1617)). Backfill e cura filtram a coluna, e `leitura` a usa na reconstrução do passivo sem linha ([backfill_decisoes_pergunta.py:57](app/chamados/management/commands/backfill_decisoes_pergunta.py#L57), [curar_perguntas_em_limbo.py:47](app/chamados/management/commands/curar_perguntas_em_limbo.py#L47), [models.py:4251](app/chamados/models.py#L4251)). |
   | `validada_em` | `_carimbar_validacao` grava a coluna e registra o ato; resposta nova e reabertura podem limpá-la ([validacao.py:161](app/chamados/services/validacao.py#L161), [disputa_emissao.py:1484](app/chamados/services/disputa_emissao.py#L1484), [disputa_emissao.py:1617](app/chamados/services/disputa_emissao.py#L1617)). O backfill lê a coluna; a seleção de pendentes lê a validação devolvida por `leitura` ([backfill_decisoes_pergunta.py:57](app/chamados/management/commands/backfill_decisoes_pergunta.py#L57), [models.py:2003](app/chamados/models.py#L2003)). |
   | `DecisaoPergunta.ato`, `ato_em`, `ato_texto` e `resposta_mantem_validacao` | `registrar` insere as linhas recebidas; a porta de resposta fornece esses valores ([models.py:4329](app/chamados/models.py#L4329), [disputa_emissao.py:1532](app/chamados/services/disputa_emissao.py#L1532)). `leitura` os percorre para reconstruir resposta e validação; também acrescenta o passivo da projeção sem linha ([models.py:4280](app/chamados/models.py#L4280), [models.py:4308](app/chamados/models.py#L4308)). |

Os testes não foram executados [não verificado]. A identidade da cópia pelo hash informado também está **[não verificado]**: o diretório disponível não é um repositório Git [não verificado]. [nao verificado]

## 18/11 20:34 · sessao 4d39f1c0a7d7 · E4C-F6F7 · verificador · origem=Code

**Resultado: os três achados da triagem estão corrigidos no código do delta, com uma ressalva de execução dos testes novos:** as três classes acrescentadas depois de `unittest.main()` são descobertas por `python -m unittest`, mas não quando o arquivo é executado diretamente. Os seis testes do módulo c14 passaram pela primeira forma; a catraca geral dos mutantes E3 não pôde ser importada neste ambiente por falta de `django.test`. [test_e4c_f6f7_c14_lei3_pela_leitura.py:101; test_e3_mutantes_alvo.py:9; execução local, não registrada em arquivo] [nao verificado]

1. **Fechamento dos achados e chamadores**

   - **PROCEDE — default silencioso fechado.** `leitura` e `leituras` são argumentos nomeados obrigatórios; os dois pontos de `_tarefa_pura` indexam o mapa, de modo que uma chave ausente falha em vez de produzir leitura vazia. O teste cobre ausência dos argumentos e mapa incompleto. [tarefa_do_dia.py:142; tarefa_do_dia.py:196; tarefa_do_dia.py:279; tarefa_do_dia.py:340; test_e4c_f6f7_c14_lei3_pela_leitura.py:151] [nao verificado]
   - **PROCEDE — projeção futura cortada para a leitura.** A cópia recebe `None` nos dois carimbos posteriores a `agora`, antes de `DecisaoPergunta.leitura`; há teste para futuro e igualdade nos dois campos. [tarefa_do_dia.py:405; test_e4c_f6f7_c14_lei3_pela_leitura.py:134] [nao verificado]
   - **PROCEDE — alvo velho corrigido.** A string do mutante inclui a nova chamada nomeada e corresponde a uma ocorrência na cópia; a catraca exige exatamente uma ocorrência. A execução completa dessa catraca está **[não verificado]**, pois sua importação falhou por falta de `django.test`. [e3_v8_mutantes.py:76; tarefa_do_dia.py:279; test_e3_mutantes_alvo.py:14; execução local, não registrada em arquivo]
   - **NÃO PROCEDE — chamador omitido encontrado.** A busca textual no repositório não revelou chamada de produção ou fixture à função real sem o novo argumento: produção passa pelas chamadas em `279`, `340`, `458` e `659–663`; os testes que omitem argumento o fazem para verificar `TypeError`. A busca não prova ausência de chamada construída dinamicamente sem o nome literal. [tarefa_do_dia.py:279; tarefa_do_dia.py:340; tarefa_do_dia.py:458; tarefa_do_dia.py:659; test_e4c_f6f7_c14_lei3_pela_leitura.py:151; busca textual, não registrada em arquivo] [nao verificado]

2. **Alcance do corte**

   **PROCEDE — o original fica intacto quanto aos dois carimbos nesse trecho.** O código faz `copy(p)`, altera atributos da cópia e passa essa cópia exclusivamente à leitura; o teste verifica os dois valores originais após as chamadas. A cópia é superficial, portanto isso não é prova geral de isolamento de todos os objetos relacionados. [tarefa_do_dia.py:403; test_e4c_f6f7_c14_lei3_pela_leitura.py:137] [nao verificado]

   Depois da leitura, os originais `_ps` ainda alimentam `linhas_da_projecao_sem_linha`, `esta_stale`, a seleção de âncoras e chamados; a carga também conserva as perguntas originais em `_perg` e as passa à tarefa. A leitura resultante pode, porém, mudar a razão e as ações publicadas: o efeito observável do corte não se limita ao valor interno da leitura. A face por pergunta chega à API; a carga alimenta, entre outros, fora da folha, retrato e vigia. [tarefa_do_dia.py:417; tarefa_do_dia.py:422; tarefa_do_dia.py:432; tarefa_do_dia.py:521; tarefa_do_dia.py:659; tarefa_do_dia.py:163; api/views.py:2544; folha/fora_da_folha.py:62; folha/retrato_folha.py:1212; chamados/services/vigia_fio.py:357]

   **PROCEDE — mesma fronteira temporal e mesmo `agora`:** o corte descarta `instante > agora`, conservando igualdade; o passivo conserva `ato_em <= agora`. Os predicados completos têm entradas diferentes: o passivo primeiro exige projeção sem linha correspondente, enquanto o novo corte atua diretamente sobre os dois campos da cópia. [tarefa_do_dia.py:407; tarefa_do_dia.py:417; models.py:4251]

3. **Exceção no contrato de vitalidade**

   - **NÃO PROCEDE — allowlist anterior zero para serviços.** Já havia entradas para funções de serviços, inclusive `validacao.py`, `relavra_via.py` e `raiox.py`. [test_contract_leitores_vitalidade.py:19; test_contract_leitores_vitalidade.py:40; test_contract_leitores_vitalidade.py:52]
   - **PROCEDE para os caminhos de produção examinados — a razão descreve a origem do valor.** A carga preenche o mapa com `DecisaoPergunta.leitura`, que retorna `LeituraDoAto`; `_pela_decisao` recebe esse resultado. A assinatura, por si só, não impõe o tipo, e fixtures passam objetos sintéticos. [tarefa_do_dia.py:413; tarefa_do_dia.py:458; tarefa_do_dia.py:528; models.py:4326; test_e3_tarefa_admin_sem_banco.py:126]
   - **PROCEDE — há alternativa sem essa entrada.** O contrato AST reconhece o nome do atributo e a forma sintática, sem distinguir a origem do objeto. Uma alteração explícita do contrato para reconhecer a leitura do ato permitiria preservar a verificação de carimbo cru sem isentar toda `_pela_decisao`; essa alternativa é uma proposta, não uma alteração aplicada nem validada. [test_contract_leitores_vitalidade.py:299; test_contract_leitores_vitalidade.py:333; test_contract_leitores_vitalidade.py:346] [nao verificado]

4. **Fixtures**

   **NÃO PROCEDE — não identifiquei assert removido, caso retirado ou tolerância ampliada no delta fornecido.** As alterações acrescentam principalmente mapas de leituras sintéticas, campos e um stub de `leitura`; os asserts mostrados permanecem. Isso não demonstra equivalência geral de cobertura: por exemplo, a fixture V8 fornece `validada_em=None` para todas as perguntas, sem reproduzir a leitura real. [e3_v8_apoio.py:126; test_e3_r3_c7_sem_banco.py:179; test_e3_r3_c7_sem_banco.py:195; test_e3_c8b_sem_banco.py:246]

5. **Mutantes do aval**

   **PROCEDE no conjunto dos quatro casos enumerados; fora dele, [não verificado].** Cada mutante é exigido como diferente do resultado correto no caso designado, e igual à função original nos outros três casos enumerados. Assim, a exclusividade demonstrada é apenas sobre essa matriz, não sobre todas as sequências possíveis. [test_e4c_f6f7_c14_lei3_pela_leitura.py:173; test_e4c_f6f7_c14_lei3_pela_leitura.py:192]

   **PROCEDE — sobrevivência faz o teste falhar**, desde que o teste seja executado: `assertNotEqual` falha se o mutante devolver o resultado esperado; `subTest` registra a falha no resultado do `unittest`, sem `try/except` que a suprima. O teste passou na execução por descoberta de módulo. Há a ressalva já indicada: execução direta do arquivo chega a `unittest.main()` antes da definição de `MutantesDoAval`, `HorizonteProjecao` e `LeituraObrigatoria`. [test_e4c_f6f7_c14_lei3_pela_leitura.py:101; test_e4c_f6f7_c14_lei3_pela_leitura.py:104; test_e4c_f6f7_c14_lei3_pela_leitura.py:166; test_e4c_f6f7_c14_lei3_pela_leitura.py:183; execução local, não registrada em arquivo] [nao verificado]

6. **Escritores e leitores dos valores relevantes**

   | Campo ou valor | Escrita e leitura verificadas |
   |---|---|
   | `respondida_em` e `resposta_colab` | A resposta humana escreve os dois e registra ato de resposta; resposta automática e carimbo pela trilha também os escrevem. A leitura usa a projeção para compor linhas passivas e devolve resposta vigente. [disputa_emissao.py:1481; disputa_emissao.py:1532; disputa_emissao.py:1682; disputa_emissao.py:1705; models.py:4259; models.py:4312] |
   | `validada_em` | A validação escreve o carimbo e o ato; devolução e resposta humana podem limpá-lo; o modelo também pode carimbá-lo. O corte escreve apenas `None` na cópia e a leitura usa o campo na composição da sequência. [validacao.py:161; validacao.py:269; disputa_emissao.py:1484; models.py:3325; tarefa_do_dia.py:407; models.py:4265] |
   | `ato`, `ato_em`, `resposta_mantem_validacao` | Escritores registram atos pela porta `registrar`; resposta humana grava `False`, automática e trilha gravam `True`, assim como a linha passiva de resposta. A carga filtra `ato_em`, a leitura reconstrói a vigência e `_pela_decisao` percorre a cauda. [models.py:4328; disputa_emissao.py:1532; disputa_emissao.py:1685; disputa_emissao.py:1707; models.py:4260; tarefa_do_dia.py:390; models.py:4308; tarefa_do_dia.py:158] |
   | `leituras[p.pk]`, `leitura.validada_em` | `_fatos_das_perguntas` constrói o mapa; a carga o guarda e o entrega a `_tarefa_pura`; `_pela_decisao` usa a validação vigente para selecionar o ato decisório. A face por pergunta usa o mesmo mapa. [tarefa_do_dia.py:404; tarefa_do_dia.py:528; tarefa_do_dia.py:659; tarefa_do_dia.py:163; tarefa_do_dia.py:458] |
   | `passivo`, `lavra`, `efeitos`, `fila` | `_fatos_das_perguntas` constrói esses insumos; `_pela_decisao` os lê para escolher razão, ações e porta. A lavra vem de `veredito_lavrado` sujeito a `esta_stale`; efeitos vêm de batidas; fila vem de `qs_na_fila`. [tarefa_do_dia.py:414; tarefa_do_dia.py:420; tarefa_do_dia.py:424; tarefa_do_dia.py:439; tarefa_do_dia.py:152] |

A lista acima identifica os escritores e leitores necessários para avaliar este delta; uma enumeração exaustiva de todos os consumidores e de todos os caminhos de escrita desses campos no repositório é **[não verificado]**. Não há resultado de produção nem execução da suíte inteira nos dados fornecidos: **[não verificado]**. [nao verificado]

## 18/11 20:31 · sessao 752bc2796335 · E4C-F6F7 · construtor · origem=Code

**AVAL-DEVOLVIDO — PRONTO (ii)–(iv):** esses casos já tinham o resultado esperado na ponta anterior; sua mordida foi exercitada com mutantes próprios. A contagem de produção solicitada no aval permanece **[não verificado]**: o dado fornecido não traz a contagem nem há acesso ao banco.

O diff está na cópia, sem commit. A certificação pela suite Code e pelo opus final permanece **[não verificado]**. [nao verificado]

- **A — leitura obrigatória:** assinaturas sem default e acesso por `leituras[p.pk]` nos dois pontos de decisão; o teste verifica ausência de argumento e chave ausente. [serviço:142](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/services/tarefa_do_dia.py:142), [serviço:196](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/services/tarefa_do_dia.py:196), [teste:151](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:151). [nao verificado]
- **B — horizonte:** a cópia entregue à leitura corta `respondida_em` e `validada_em` posteriores a `agora`; o corte do passivo permanece no lugar. O RED, executado antes da cura, retornou `silencio_decidido` onde se esperava `aguarda_validacao`; depois passou, inclusive o espelho no limite. [serviço:405](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/services/tarefa_do_dia.py:405), [teste:104](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:104). [nao verificado]
- **C — alvo:** a string do mutante de absorção acompanha a chamada nova; o teste de alvo único passou pela pista AST. [mutante:76](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/e3_v8_mutantes.py:76), [pista:96](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e3_volta7_sem_banco.py:96). [nao verificado]
- **D — contrato e fixtures:** o contrato detecta comparações com campos chamados `validada_em`, sem distinguir a origem. Registrei a exceção porque aqui o valor vem de `DecisaoPergunta.leitura`, juiz da sequência; não é o carimbo cru da pergunta nem julgamento de vitalidade por `pergunta_viva`. O contrato isolado passou com `SimpleTestCase` substituído por `unittest.TestCase`. [contrato:290](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_contract_leitores_vitalidade.py:290), [juiz:4303](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/models.py:4303). [nao verificado]
- **Fixtures:** `_leituras` faltava nas cargas sintéticas que pulam o construtor; uma delas também precisava preencher o mapa para as perguntas efetivamente carregadas. O dublê de `DecisaoPergunta` não fornecia `leitura`; e a fixture de pergunta fornecia `celula`, mas não a FK `celula_id`. Foram corrigidas nas fixtures, sem fallback no serviço. [carga:360](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e3_degrau_dono_e_porta.py:360), [mapa:246](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e3_c8b_sem_banco.py:246), [dublê:197](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e3_r3_c7_sem_banco.py:197), [FK:115](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:115).

Os mutantes **(ii), (iii) e (iv)** abaixo foram executados **em memória**, sem alterar o serviço. Para cada um, o teste também compara os outros três casos do aval com o comportamento original. [teste:166](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:166). [nao verificado]

**(ii) Recuar no máximo uma resposta**, substituição única em `tarefa_do_dia.py`:

```text
antiga:     while k and decisoes[k - 1].ato == D.RESPOSTA and decisoes[k - 1].resposta_mantem_validacao:
nova:       if k and decisoes[k - 1].ato == D.RESPOSTA and decisoes[k - 1].resposta_mantem_validacao:
```

**(iii) Dispensar leitura validada**, substituição única:

```text
antiga:     if 0 < k < len(decisoes) and leitura.validada_em is not None:
nova:       if 0 < k < len(decisoes) and True:
```

**(iv) Tornar transparente a resposta que desfaz**, substituição única do bloco:

```python
    while k and decisoes[k - 1].ato == D.RESPOSTA and decisoes[k - 1].resposta_mantem_validacao:
        k -= 1
    if 0 < k < len(decisoes) and leitura.validada_em is not None:
```

por:

```python
    while k and decisoes[k - 1].ato == D.RESPOSTA:
        k -= 1
    if 0 < k < len(decisoes) and (leitura.validada_em is not None or not decisoes[-1].resposta_mantem_validacao):
```

Comando da prova em memória para **cada um dos três**:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4c_f6f7_c14_lei3_pela_leitura.MutantesDoAval -v
```

O teste completo da leitura e o teste da pista de alvos também passaram: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e4c_f6f7_c14_lei3_pela_leitura -v
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e3_volta7_sem_banco -v
```

Os mutantes específicos das alterações A e B ainda **não foram executados** [não verificado]. A verificação local dos módulos sem banco selecionados passou: 60 testes. A execução integral com Django e o DIFF-RETIDO permanecem **[não verificado]**. [nao verificado]

Por erro no comando destinado ao contrato isolado, iniciei descoberta ampla de testes, contrariando a instrução de não rodar a suite; interrompi a execução ao perceber. Ela apresentou erros de importação por ausência de Django e não constitui resultado de suite. A extensão de possíveis efeitos dessa descoberta interrompida é **[não verificado]**. [nao verificado]

LEI-AKITA: origem=`chamados/services/tarefa_do_dia.py::_pela_decisao`, testemunha=`DecisaoPergunta.leitura`, RED=item 2 e horizonte, juizes novos=0, **padrao=juiz unico**. [serviço:142](/home/ronald/wt-codex-e4c-f6f7-174627/app/chamados/services/tarefa_do_dia.py:142)

## 18/11 20:25 · sessao 69896ae881ff · E4C-F6F7 · verificador · origem=Code

**AVAL-DEVOLVIDO — BLOQUEIA.** A c14 consulta o juiz existente, mas a leitura alimentada à tarefa não é necessariamente a leitura da sequência completa da pergunta; além disso, pode incorporar uma validação da projeção posterior a `agora`. Os REDs (ii)–(iv) também não ficam vermelhos na ponta anterior, embora o aval exija que cada caso morda e que os quatro estejam vermelhos antes da cura. [tarefa_do_dia.py:387](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:387), [models.py:4295](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/models.py:4295), [teste c14:34](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:34), [aval fornecido: não verificado em arquivo]

1. **BLOQUEIA — nem todos os caminhos garantem leitura.** Os três pontos de chamada são `expor_admin`, o ramo da instância e `exigencia_das_perguntas`. Os dois primeiros usam `(leituras or {}).get(p.pk)`: se o mapa ou a chave faltar, passam `None`; o terceiro indexa a leitura obrigatoriamente. A carga normal constrói e passa o mapa, mas `_tarefa_pura` aceita `leituras=None`, e há chamadas diretas de teste sem esse argumento. [tarefa_do_dia.py:196](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:196), [tarefa_do_dia.py:279](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:279), [tarefa_do_dia.py:340](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:340), [tarefa_do_dia.py:454](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:454), [tarefa_do_dia.py:522](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:522), [test_e3_c8b_sem_banco.py:187](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/test_e3_c8b_sem_banco.py:187).

   Sim: é **default silencioso**. Sem leitura, a guarda falha e `V→R(m)` termina em `aguarda_validacao`, mesmo que a leitura correta fosse validada. Isso não demonstra que a carga normal omita a leitura; demonstra que a interface permite a omissão e produz uma resposta comum, sem sinalizar desconhecimento. [tarefa_do_dia.py:142](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:142), [tarefa_do_dia.py:158](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:158).

2. **BLOQUEIA — sequência e horizonte alteram a resposta do juiz.** A carga remove atos de ressuscitação e atos reais posteriores a `agora`; a leitura comum usa todas as decisões da pergunta e não tem relógio. Portanto, por exemplo, uma reabertura de ressuscitação ou uma resposta que desfaz posterior ao horizonte pode limpar a validação na leitura completa e estar ausente da leitura alimentada à tarefa; uma validação real posterior pode fazer o inverso. [tarefa_do_dia.py:387](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:387), [models.py:4221](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/models.py:4221), [models.py:4295](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/models.py:4295).

   O literal fornecido manda perguntar a `DecisaoPergunta.leitura(p)` pela sequência de atos de uma pergunta; não autoriza expressamente substituir sua sequência pela filtrada. A exigência temporal da carga está documentada no código, mas a conciliação das duas obrigações **não fecha no literal apresentado**; não proponho reabrir a regra. [aval fornecido: não verificado em arquivo], [tarefa_do_dia.py:489](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:489).

   Há ainda um efeito temporal específico: **o ato real futuro sai de `dec`, mas pode voltar como linha passiva sintética**. A cópia conserva `p.validada_em`; como o `ja` é formado somente pelos atos cortados, uma validação projetada posterior a `agora` pode ser considerada sem linha e adicionada à leitura, sem corte temporal. O conjunto `passivo`, calculado separadamente, descarta essa linha futura e não bloqueia a decisão. Exemplo condicionado: `S→R(m)` até agora e validação projetada depois podem fazer a leitura dizer validada e a tarefa escolher `silencio_decidido`. [tarefa_do_dia.py:398](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:398), [tarefa_do_dia.py:403](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:403), [tarefa_do_dia.py:413](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:413), [models.py:4265](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/models.py:4265), [models.py:4308](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/models.py:4308).

3. **OK quanto ao cache; BLOQUEIA quanto ao conteúdo, pelo item 2.** `leitura` chama `p.decisoes.all()` e seu próprio contrato diz que essa chamada usa o cache de prefetch. Ela monta `ja` a partir dos reais recebidos e o passa a `linhas_da_projecao_sem_linha`, evitando a query própria que essa função faria se `ja` fosse omitido. Assim, neste caminho, lê a lista `dec` injetada **mais a projeção passiva em memória**, inclusive a ressalva temporal acima. [models.py:4249](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/models.py:4249), [models.py:4304](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/models.py:4304), [tarefa_do_dia.py:403](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:403).

4. **FILA — as duas fixtures apontadas não comprovam `V→R(m)`.** Na fixture c8b alterada, a sequência construída é uma resposta que **não** mantém validação; nas duas r3/c5, `_tarefa_pura` é substituída por um stub e `_dec` está vazio. Portanto, não há base nesses trechos para afirmar que seus casos antigos `V→R(m)` passam pelo motivo errado: **não identifiquei tal caso nessas fixtures**. [test_e3_c8b_sem_banco.py:200](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/test_e3_c8b_sem_banco.py:200), [test_e3_r3_c5_sem_banco.py:305](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/test_e3_r3_c5_sem_banco.py:305), [test_e3_r3_c5_sem_banco.py:336](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/test_e3_r3_c5_sem_banco.py:336).

   Já a fixture c13 fornece uma leitura **sintética**, calculada pela última linha; seu `V→R(m)` verifica a propagação da leitura fornecida e o resultado, mas não verifica que a carga obteve a leitura real. O default silencioso continua sendo um risco para outros testes que omitirem o mapa. [test_e4c_f6f7_c13_caso_pela_regra.py:24](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/test_e4c_f6f7_c13_caso_pela_regra.py:24), [test_e4c_f6f7_c13_caso_pela_regra.py:49](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/test_e4c_f6f7_c13_caso_pela_regra.py:49).

5. **BLOQUEIA — só (i) é RED contra a ponta anterior.** Pelo diff fornecido, a guarda anterior pulava a cauda quando o ato anterior era validação. Assim, (ii) já chegava à lavra; (iii) e (iv) já chegavam a `aguarda_validacao`. Isso contraria o requisito literal de os quatro estarem vermelhos antes da cura; não basta que sejam úteis como regressão. [diff fornecido: não verificado em arquivo], [teste c14:34](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:34), [aval fornecido: não verificado em arquivo].

   Mutantes que cada asserção mataria, por inspeção:

   - **(i)** trocar a consulta à leitura pela adjacência à validação; o teste explícito demonstra o resultado mutado esperado. [teste c14:85](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:85).
   - **(ii)** impedir o recuo sobre duas respostas mantidas, por exemplo permitindo recuar apenas uma. [tarefa_do_dia.py:160](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:160), [teste c14:37](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:37).
   - **(iii)** retirar a exigência de leitura validada e sempre pular a cauda mantida. [tarefa_do_dia.py:163](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:163), [teste c14:38](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:38).
   - **(iv)** tratar também resposta que desfaz como transparente e pular para a validação. [tarefa_do_dia.py:161](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:161), [teste c14:39](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:39).

   Essas mortes, exceto a adjacência expressamente codificada, são previsões por inspeção; execução dos mutantes **[não verificado]**. O selo AST cobre as formas sintáticas previstas e seu mutante, não prova toda possível forma de derivar validação. [teste c14:56](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:56).

6. **FILA — escritores e leitores; mudanças possíveis nos consumidores.**

   | Valor | Quem escreve ou produz | Quem lê |
   |---|---|---|
   | `validada_em` da projeção e ato `VALIDACAO` | `_carimbar_validacao` grava ambos; devolução e resposta humana podem limpar o carimbo e registrar outro ato. [validacao.py:161](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/validacao.py:161), [validacao.py:269](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/validacao.py:269), [disputa_emissao.py:1494](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/disputa_emissao.py:1494) | `linhas_da_projecao_sem_linha` usa o carimbo; `leitura` produz seu próprio `validada_em`; a c14 consulta este último. [models.py:4251](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/models.py:4251), [models.py:4317](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/models.py:4317), [tarefa_do_dia.py:163](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:163) |
   | `decisoes` e `resposta_mantem_validacao` | `registrar` é a porta declarada para a sequência; resposta humana registra `False`, automática e pela trilha registram `True`. [models.py:4329](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/models.py:4329), [disputa_emissao.py:1532](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/disputa_emissao.py:1532), [disputa_emissao.py:1686](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/disputa_emissao.py:1686) | `leitura` interpreta os atos; a tarefa carrega e filtra a sequência e examina a cauda. [models.py:4308](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/models.py:4308), [tarefa_do_dia.py:390](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:390) |
   | `leituras` e razão/ações do dia | `_fatos_das_perguntas` produz o mapa em memória; `_pela_decisao` produz razão e ações, inseridas na tarefa. A alteração não grava esses valores no banco. [tarefa_do_dia.py:404](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:404), [tarefa_do_dia.py:340](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:340) | `_portas_da_tarefa` transforma ações e razões em fio/rota; os consumidores leem a porta publicada. [tarefa_do_dia.py:701](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:701) |

   **Sim, podem ver valores diferentes**, dependendo das demais ações e portas do dia: `V→S→R(m)` troca uma ação de validar por silêncio sem ação; isso pode mudar fio, rota e classificações derivadas. Folha lê a porta; retrato lê folha; cadastro, vigia e pautas também consomem essa saída. Não se pode afirmar que cada consumidor mudará em toda ocorrência, pois a porta agrega outras ações. [tarefa_do_dia.py:165](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:165), [tarefa_do_dia.py:711](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:711), [fora_da_folha.py:70](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/folha/fora_da_folha.py:70), [retrato_folha.py:1212](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/folha/retrato_folha.py:1212), [cadastro_realidade.py:466](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/escala/services/cadastro_realidade.py:466), [vigia_fio.py:359](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/vigia_fio.py:359), [pautas_dos_discordantes.py:85](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/ponto/services/pautas_dos_discordantes.py:85).

7. **BLOQUEIA / FILA — classes de reprovação.**

   - **Presente:** RED sem mordida na ponta anterior, item 5; e leitura com universo diferente do juiz comum, item 2. A função do juiz é reutilizada, mas o cache adulterado faz sua resposta depender de uma sequência parcial: isso é uma derivação divergente da fonte, mesmo sem criar outra função juiz. [models.py:4295](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/models.py:4295), [tarefa_do_dia.py:403](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:403).
   - **Presente como risco de selo:** a alteração da chamada deixou o alvo literal do mutante `absorcao_admin_sem_rotulo` com a assinatura antiga; a catraca exige exatamente um alvo. A inclusão desse mutante na matriz cobrada pela catraca **[não verificado]**. [e3_v8_mutantes.py:76](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/e3_v8_mutantes.py:76), [test_e3_mutantes_alvo.py:20](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/tests/test_e3_mutantes_alvo.py:20), [tarefa_do_dia.py:279](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:279).
   - **Não demonstrados no diff:** novo escritor persistente, relógio lido fora da borda, mudança da ordem de trava ou pouso. Ausência global dessas classes e situação do pouso **[não verificado]**. O campo de leitura pode significar leitura integral em `models.py` e leitura de sequência cortada nesta carga; a diferença semântica está demonstrada no item 2. [tarefa_do_dia.py:374](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:374), [models.py:4295](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/models.py:4295).

8. **FILA — a regra do ramo existe, mas o passivo intercepta o caminho normal.** Sem linha passiva posterior, `V→DEVOLUCAO→R(m)` dá leitura não validada e, portanto, `aguarda_validacao`; o mesmo vale para recusa e reabertura, pois limpam a validação. Se uma leitura fornecida disser validada, a função recua e escolhe o último ato não transparente: devolução → `materializacao_recusada`; recusa/reabertura → `devolvida_ao_colaborador`. Isso corresponde ao literal do aval e à triagem fornecida. [models.py:4272](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/models.py:4272), [tarefa_do_dia.py:160](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:160), [triagem fornecida: não verificado em arquivo].

   Porém, se a validação vier de passivo cuja data é até `agora`, a carga inclui a pergunta em `passivo` e `_pela_decisao` retorna `passivo_sem_linha` **antes** de aplicar aquele ramo. Assim, a afirmação da triagem de que o caso passivo chega ao ramo do ato não está demonstrada para a carga normal; há tensão com o literal nesse caminho. Passivo futuro tem a anomalia separada do item 2. [tarefa_do_dia.py:154](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:154), [tarefa_do_dia.py:413](/home/ronald/codex_ws/0ee6521f52998c4296b84e2d14126ee939977e3d/app/chamados/services/tarefa_do_dia.py:413).

A sombra fornecida reporta zero perguntas com validação antes de silêncio nas 51.212 examinadas e zero candidatas `V→S` para a segunda medição; são resultados dos anexos raspados, sem acesso independente ao banco ou aos programas da sonda **[não verificado]**. Não executei a suíte nem os mutantes; seus resultados de execução **[não verificado]**. [nao verificado]

_7163 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
