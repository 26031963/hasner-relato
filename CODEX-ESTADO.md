# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 07:53** · publicado em **09/11/2026 07:53** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 07:53 · sessao 0e7deca660a7 · AUDITORIA-0911 · leitura · origem=Code

**Não há número novo de produção nem commit de cura nesta auditoria.** As sondas abaixo não foram executadas; o hash informado não foi comprovado como commit Git nesta cópia. [nao verificado] [nao verificado]

**A — Os contadores não medem o mesmo universo.**

| Grandeza | Quem conta | Universo literal e unidade |
|---|---|---|
| Badge **1.177** do anexo [nao verificado] | `lavrar_badge_navbar` → `exige_o_admin` → `particionar_verbos` | `ChamadoColaborador.objects.filter(status_local__in=VIVOS)`, com `VIVOS=('aberto','em_analise','registrado')`; soma somente verbos `validar` e `decidir`. **Chamados**, não perguntas/toques. `app/core/management/commands/lavrar_badge_navbar.py:72`; `app/chamados/services/painel.py:155`; `app/chamados/catalogo/motor.py:24`; `app/chamados/services/verbos.py:22`. |
| **4.323** referido no aval [nao verificado] | Se corresponde à coluna `chamados_vivos`, é `vivos.count()` | Mesmo filtro `status_local__in=VIVOS`; **chamados**. Não corresponde a `lavradas` nem a `sobra`. `app/chamados/management/commands/lavrar_perguntas_stale.py:56`; `app/chamados/management/commands/lavrar_perguntas_stale.py:71`. |
| `universo=77xx` do stale | `universo.count()` | `PerguntaDisputa.objects.filter(chamado_destino__in=vivos)`; **perguntas**, sem o fallback pelo chamado da disputa que o badge utiliza. `app/chamados/management/commands/lavrar_perguntas_stale.py:57`; `app/chamados/services/raiox.py:54`. |
| `perguntas_stale_agora` | `len(pks_stale_por_varredura(universo))` | Perguntas daquele universo com lavra ausente, relógio vencido ou insumo posterior à lavra. `app/chamados/services/raiox.py:31`. |
| `lavradas / sobra` | Laço `lavrar(p)` / `max(len(stale)-teto,0)` | **Perguntas processadas nesta rodada / perguntas fora do teto**; teto padrão 500. `app/chamados/management/commands/lavrar_perguntas_stale.py:35`; `app/chamados/management/commands/lavrar_perguntas_stale.py:60`; `app/chamados/management/commands/lavrar_perguntas_stale.py:84`. |
| **7.716 literal encontrado** | Não é apresentado como contador do stale | A ocorrência documental é a **meta histórica de 98% do oráculo**, em **dia-colab**: `7.716−7.193=523`, sobre 7.873 comparados. Atribuir exatamente 7.716 ao universo de perguntas de uma rodada é [nao verificado]. `app/docs/RELATO-ARQUIVO.md:23683`; `app/docs/RELATO-ARQUIVO.md:23695`. |

O arquivo de log mistura dois lavradores: `lavrar_placar_situacional` conta o universo de colaboradores em operação; o badge pertence ao `lavrar_badge_navbar`. A diferença entre esses universos não demonstra, sozinha, defeito estrutural. `app/core/management/commands/lavrar_placar_situacional.py:54`; `app/core/management/commands/lavrar_badge_navbar.py:85`.

A separação solicitada tem estes limites de autoridade:

| Classe pedida | Juiz existente |
|---|---|
| Legítimo | **SEM JUIZ composto.** Premissa viva inclui resultado conservador quando falta insumo; não prova legitimidade geral. `app/chamados/catalogo/premissa.py:128`. |
| Repetido | O registro aponta para **`ChamadoColaborador.abrir`**, uma porta de escrita. **SEM JUIZ equivalente de leitura** para censar todos os repetidos; não chamar essa porta na sonda. `app/core/juizes.py:268`. |
| Sem porta | `porta_alcancavel_do_dia`; seu `None` significa ausência das portas daquele corte. “Resolver dia” e acesso manual por URL estão explicitamente fora desse juiz. `app/core/juizes.py:277`; `app/chamados/juizes.py:1361`; `app/chamados/juizes.py:1387`. |
| Premissa superada | `premissa_morta`, alimentada por `_veredito_premissa`; respeitar `escopo` e `viva`. `app/core/juizes.py:263`; `app/chamados/reconciliador.py:247`. |
| Exige humano | Medida pelo verbo existente `validar/decidir`; **SEM JUIZ composto adicional** para esse rótulo. `app/chamados/services/verbos.py:22`. |
| Pergunta viva/encerrada atualmente | C6, `pergunta_viva`. Via `duplicata` registrada é encerramento por identidade; não equivale a censo de repetição ativa. `app/core/juizes.py:266`; `app/chamados/juizes.py:1163`. |

Esses resultados podem sobrepor-se. Não há autoridade citada que estabeleça uma partição exclusiva entre os cinco rótulos; a sonda mantém os eixos separados. `app/core/juizes.py:263`; `app/core/juizes.py:266`; `app/core/juizes.py:277`.

**Nasce/permanece/encerra históricos antes/depois do deploy não são reconstruíveis com exatidão pelos campos atuais.** A pergunta não tem carimbo próprio de nascimento; `data_evento` é a data do fato. O chamado pode reabrir; seus carimbos atuais não fornecem todos os intervalos. `DecisaoPergunta` registra atos, sem registrar todas as mudanças de C6 provocadas por insumos externos. O fluxo histórico pedido permanece [nao verificado]. `app/chamados/models.py:2431`; `app/chamados/models.py:2455`; `app/chamados/models.py:292`; `app/chamados/models.py:3857`.

A sonda A mede os contadores literais, o estoque atual e os carimbos disponíveis por competência; não apresenta carimbo de fechamento como fluxo completo. As competências usam o juiz da empresa: com corte 21, outubro é 21/09–20/10 e novembro é 21/10–20/11. `app/ponto/janelas.py:10`; `app/ponto/janelas.py:36`; `app/chamados/juizes.py:2090`.

**Preâmbulo comum às sondas.** Executar somente após a cura do envelope, em Python/Django já inicializado e com conexões abertas em leitura. O envelope atual executa `GRANT` antes da sonda; este bloqueio interno não cobre aquela execução anterior. `bin/sonda_leitura.sh:157`; `bin/sonda_leitura.sh:161`; `bin/sonda_leitura.sh:192`.

```python
# PREAMBULO COMUM — não executado nesta auditoria.
import sys
sys.dont_write_bytecode = True

import json
import logging
import re
from contextlib import ExitStack, contextmanager
from django.db import connections

class SondaRecusada(BaseException):
    pass  # não pode ser engolida pelos except Exception da aplicação

def saida(**dados):
    print(json.dumps(dados, ensure_ascii=False, default=str))

@contextmanager
def leitura():
    anterior = logging.root.manager.disable
    logging.disable(10**9)  # nenhuma saída incidental nominal dos leitores
    try:
        with ExitStack() as pilha:
            for conn in connections.all():
                # O envelope curado deve inicializar as conexões antes daqui.
                if conn.connection is None:
                    raise SondaRecusada("conexao nao inicializada pelo envelope")
                if conn.in_atomic_block or not conn.get_autocommit():
                    raise SondaRecusada("exige autocommit, fora de atomic")

                def somente_select(execute, sql, params, many, context):
                    texto = str(sql).strip()
                    if (
                        not re.match(r"(?is)^SELECT\b", texto)
                        or ";" in texto
                        or re.search(
                            r"(?is)\bFOR\s+(UPDATE|SHARE|NO\s+KEY|KEY\s+SHARE)\b"
                            r"|\b(nextval|setval|set_config|pg_advisory\w*)\s*\(",
                            texto,
                        )
                    ):
                        raise SondaRecusada("SQL fora do contrato SELECT")
                    return execute(sql, params, many, context)

                pilha.enter_context(conn.execute_wrapper(somente_select))
                with conn.cursor() as cur:
                    cur.execute(
                        "SELECT current_setting('transaction_read_only'), "
                        "rolsuper FROM pg_roles WHERE rolname=current_user"
                    )
                    estado = cur.fetchone()
                if estado != ("on", False):
                    raise SondaRecusada("conexao sem contrato de leitura")
            yield
    finally:
        logging.disable(anterior)
```

Não envolver as sondas em `atomic`: `reconciliaria()` tem caminho de trava quando já está dentro de transação. O bloqueio usa `BaseException` porque o fechamento captura `Exception` e poderia ocultar uma tentativa recusada. `app/chamados/models.py:705`; `app/ponto/services/fechamento.py:1303`.

```python
# SONDA A — contadores e eixos atuais, competências 10 e 11.
# Fontes: lavrar_perguntas_stale.py:56; verbos.py:25;
# core/juizes.py:263,266,274,277.
from collections import Counter
from datetime import datetime
from zoneinfo import ZoneInfo

def sonda_A():
    from chamados.models import ChamadoColaborador as CH, PerguntaDisputa as PD
    from chamados.catalogo.motor import VIVOS
    from chamados.services.verbos import (
        particionar_verbos, exige_o_admin, VERBOS_DO_ADMIN,
    )
    from chamados.services.raiox import pks_stale_por_varredura
    from chamados.juizes import (
        competencia_do_chamado, pergunta_viva,
        alimentar_portas, porta_alcancavel_do_dia,
    )
    from chamados.reconciliador import _veredito_premissa
    from ponto.janelas import janela_atual

    deploy = datetime(2026, 11, 9, 1, 52, [nao verificado]
                      tzinfo=ZoneInfo("America/Sao_Paulo"))
    vivos = CH.objects.filter(status_local__in=VIVOS)
    universo = PD.objects.filter(chamado_destino__in=vivos)
    part = particionar_verbos(vivos)
    stale = set(pks_stale_por_varredura(universo))
    saida(
        item="A", recorte="global_literal",
        badge=exige_o_admin(part["contagens"]),
        chamados_vivos=vivos.count(),
        universo_perguntas=universo.count(),
        perguntas_stale_agora=len(stale),
        lavraria_com_teto_500=min(500, len(stale)),
        sobra_com_teto_500=max(len(stale)-500, 0),
    )

    chamados = list(CH.objects.select_related("colaborador__empresa", "celula"))
    alimentacao = alimentar_portas(
        {c.celula_id for c in chamados if c.celula_id}
    )
    cont = Counter()

    def contar(mes, entidade, causa, eixo, resultado):
        cont[(mes, entidade, causa, eixo, resultado)] += 1

    def lado(ts):
        return ("SEM CARIMBO" if ts is None else
                "antes_deploy" if ts < deploy else "depois_deploy") [nao verificado]

    for c in chamados:
        cp = competencia_do_chamado(c)
        if cp is None:
            contar(None, "chamado", (c.modulo_origem, c.subcategoria),
                   "competencia", "SEM FATO DATADO")
            continue
        if cp.ano != 2026 or cp.mes not in (10, 11):
            continue
        causa = (c.modulo_origem, c.subcategoria)
        contar(cp.mes, "chamado", causa, "status_atual", c.status_local)
        contar(cp.mes, "chamado", causa, "nascimento_registrado", lado(c.criado_em))
        contar(cp.mes, "chamado", causa, "carimbo_fecho_atual", lado(c.resolvido_em))
        contar(cp.mes, "chamado", causa, "legitimo", "SEM JUIZ")
        contar(cp.mes, "chamado", causa, "repetido_leitura", "SEM JUIZ")
        try:
            v = _veredito_premissa(c)
            resultado = (
                "SEM JUIZ" if v is None else
                "superada" if not v.viva else v.motivo
            )
            contar(cp.mes, "chamado", causa, "premissa", resultado)
        except Exception as exc:
            contar(cp.mes, "chamado", causa, "premissa",
                   "JUIZ FALHOU:" + type(exc).__name__)
        if c.status_local in VIVOS:
            porta = porta_alcancavel_do_dia(
                c.celula_id, alimentacao=alimentacao
            )
            contar(cp.mes, "chamado", causa, "porta_do_corte",
                   porta["rota"] if porta else "sem porta")
            verbo = part["mapa"].get(c.pk)
            contar(cp.mes, "chamado", causa, "verbo", verbo)
            if verbo in VERBOS_DO_ADMIN:
                contar(cp.mes, "chamado", causa, "exige_humano",
                       "verbo_do_admin")

    for p in PD.objects.select_related(
        "disputa__colaborador__empresa", "celula", "chamado_destino"
    ).iterator():
        causa = (p.motivo, p.instancia_marco)
        if p.conversa or p.data_evento is None:
            contar(None, "pergunta", causa, "competencia",
                   "SEM DIA DE TURNO")
            continue
        fim = janela_atual(p.data_evento, p.disputa.colaborador.empresa)[1]
        if fim.year != 2026 or fim.month not in (10, 11):
            continue
        contar(fim.month, "pergunta", causa, "C6_atual",
               "viva" if pergunta_viva(p) else "encerrada")
        contar(fim.month, "pergunta", causa, "via_registrada", p.via_resolucao)
        contar(fim.month, "pergunta", causa, "legitimo", "SEM JUIZ")
        if p.chamado_destino_id in part["mapa"]:
            contar(fim.month, "pergunta", causa, "universo_stale", "destino_vivo")
        if p.pk in stale:
            contar(fim.month, "pergunta", causa, "stale", "sim")

    for (mes, entidade, causa, eixo, resultado), n in sorted(
        cont.items(), key=lambda x: str(x[0])
    ):
        saida(item="A", competencia=mes, entidade=entidade,
              causa=causa, eixo=eixo, resultado=resultado, n=n)
    saida(
        item="A", nasce_permanece_encerra_historico=None,
        limite="[nao verificado]: sem fotos e insumos historicos; "
               "carimbos atuais nao reconstroem C6 e portas anteriores",
    )

with leitura():
    sonda_A()
```

**Dono A:** divergência entre um contador e sua própria lista seria provavelmente **ESTRUTURA**, padrão **Single Source of Truth / Aggregate Root**. Números diferentes com universos diferentes não bastam para essa atribuição. `app/docs/GLOSSARIO.md:11`; `app/ponto/management/commands/e6_oraculo.py:57`.

**B — Ausência aprovada até DiaPago/export.**

| Degrau | Leitura/efeito existente |
|---|---|
| Cobertura | Para folha, somente `status='aprovada'`; para cobrança, exclui rejeitada. Sobreposição vence por `(data,pk)` mais recente. `app/ponto/turnos.py:2511`; `app/ponto/turnos.py:2735`. |
| Célula/grade | Overlay da ausência na grade; leitor da célula entrega a ata lavrada. `app/escala/utils.py:1188`; `app/escala/services/leitor_celula.py:345`. |
| Resolver dia | Lê batidas cruas de D até D+2 e chama `grade_dia`. No pedido `trabalhou`, recusa se `tipo_dia != 'trabalho'`. Isso não demonstra perda no motor. `app/ponto/services/veredito_celula.py:234`; `app/ponto/services/veredito_celula.py:241`; `app/ponto/services/simular_dia.py:5`. |
| Motor | Fechamento lê cobertura, acrescenta ABONA a `datas_justificadas` e entrega ausências integrais ao motor. A integral zera atraso/saída antecipada, preservando horas trabalhadas. `app/ponto/services/fechamento.py:683`; `app/ponto/services/fechamento.py:751`; `app/ponto/motor_calculo_v2.py:1085`. |
| Abono diário | `por_dia_da_grade`: previsto menos cobertura das batidas no dia de ausência. O comentário cita col39/15-10 como precedente, não como medição atual de #4879. `app/ponto/services/dia_pago.py:364`; `app/ponto/services/dia_pago.py:375`. |
| DiaPago | Lavra recebe os mesmos períodos e mapas do fechamento. `somente_leitura=True` só permanece leitura com `lavrar_dias_pagos=False`. `app/ponto/services/fechamento.py:1191`; `app/ponto/services/fechamento.py:1201`. |
| Export | `classificar_export` julga entrada; `montar_linhas_export` lê fechamento e eventos, sem recalcular. Registros de horas são mensais; datas individuais existem no bloco de faltas. `app/folha/export.py:757`; `app/folha/export.py:717`; `app/folha/export.py:224`. |

O dado fornecido não contém os valores dos degraus de #4879. Portanto, o ponto concreto da perda de cobertura permanece [nao verificado].

```python
# SONDA B — ausência/célula/Resolver/motor/projeção/export, sem escritores.
# Fontes: fechamento.py:448,683,751,1128,1191; dia_pago.py:364;
# veredito_celula.py:234; export.py:682.
def sonda_B():
    import datetime as dt
    from collections import Counter
    from django.utils import timezone
    from colaboradores.models import Colaborador
    from ponto.models import Ausencia, Batida, DiaPago, FechamentoMensal
    from ponto.turnos import cobertura_ausencia_periodo, cobre_parte_do_dia
    from ponto.services.efeito_ausencia import efeitos_da_ausencia
    from escala.services.leitor_celula import grade_da_celula
    from ponto.services.simular_dia import grade_dia
    from ponto.services.dia_pago import CAMPOS_DIA, por_dia_da_grade
    from ponto.services.fechamento import recalcular_fechamento_mes
    from folha.export import (
        janela_competencia, classificar_export,
        montar_linhas_export, partes_da_linha,
    )
    from folha.models import ExportacaoDominio, anotar_vigencia, so_vigentes

    c = Colaborador.objects.select_related("empresa").get(pk=39)
    d = dt.date(2026, 10, 15)
    agora = timezone.now()
    ini, fim = janela_competencia(c.empresa, 10, 2026)
    a = Ausencia.objects.get(pk=4879, colaborador=c)
    campos_a = ("status", "tipo", "data", "data_fim",
                "minutos", "hora_ini", "hora_fim")
    saida(item="B", degrau="ausencia",
          lido={k: getattr(a, k) for k in campos_a},
          efeito=a.efeito, parcial=cobre_parte_do_dia(a))

    for para in ("folha", "cobranca"):
        vencedora = cobertura_ausencia_periodo(c, d, d, para=para).get(d)
        saida(item="B", degrau="cobertura", para=para,
              ausencia_vencedora=getattr(vencedora, "pk", None),
              efeito=getattr(vencedora, "efeito", None))
    efeitos = efeitos_da_ausencia(c, d, d)
    saida(item="B", degrau="efeitos",
          lido={k: d in efeitos[k]
                for k in ("justificadas", "suprimidas", "dia_inteiro")})

    campos_grade = (
        "tipo_dia", "cobertura_parcial", "ausencia_id",
        "minutos_previstos", "minutos_realizados", "n_missing", "n_ok",
    )
    g = grade_da_celula(c, d, d)
    saida(item="B", degrau="celula",
          lido=[{k: x.get(k) for k in campos_grade}
                for x in g.get("dias") or []])
    mapas = por_dia_da_grade(g)
    def valor_dia(m):
        return m.get(d, m.get(str(d)))
    saida(item="B", degrau="grade_para_DiaPago",
          lido=dict(zip(("abono", "previsto", "realizado"),
                        (valor_dia(m) for m in mapas))))

    j0 = timezone.make_aware(dt.datetime.combine(d, dt.time.min))
    reais = list(Batida.objects.filter(
        colaborador=c, timestamp__gte=j0,
        timestamp__lt=j0+dt.timedelta(days=2),
    ).order_by("timestamp"))
    saida(item="B", degrau="batidas_lidas_pelo_Resolver",
          lido=[{k: getattr(b, k) for k in (
              "timestamp", "tipo", "origem", "is_intervalo",
              "aprovada", "retratada_em",
          )} for b in reais])
    marcos, dd = grade_dia(c, reais, d, agora=agora)
    saida(item="B", degrau="Resolver_grade",
          lido={k: dd.get(k) for k in campos_grade},
          marcos=[{k: m.get(k) for k in ("tipo", "hora")}
                  if isinstance(m, dict) else None for m in marcos])

    # Observa os objetos/locais do caminho real, em memória.
    def perfil(frame, event, arg):
        if event != "return":
            return
        path, fn = frame.f_code.co_filename, frame.f_code.co_name
        loc = frame.f_locals
        if path.endswith("/ponto/services/fechamento.py") and fn == "_montar_motor":
            saida(item="B", degrau="motor_recebeu",
                  integral=d in arg.dias_com_ausencia_inteira,
                  justificada=d in arg.datas_justificadas)
        if path.endswith("/ponto/services/fechamento.py") and fn == "recalcular_fechamento_mes":
            r = loc.get("resultado")
            mapa = getattr(r, "dia_da_jornada", {}) or {}
            periodos = [p for p in (getattr(r, "periodos", []) or [])
                        if mapa.get(id(p)) == d]
            saida(item="B", degrau="motor_devolveu",
                  periodos=[{k: getattr(p, k, None) for k in (
                      "entrada", "saida", "minutos_trabalhados",
                      "minutos_atraso", "minutos_saida_antecipada",
                  )} for p in periodos],
                  furo_motor=d in (getattr(r, "datas_falta", []) or []),
                  mapas_para_DiaPago={
                      k: valor_dia(loc.get(k) or {})
                      for k in ("_abono_dia", "_prev_dia_m", "_real_dia_m",
       

[truncado em 20000 de 44487 caracteres]

## 09/11 07:52 · sessao 7c9934466d7c · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · leitura · origem=Code

O bloqueio está na aplicação **por pergunta**, antes da porta de escrita: cada metade encontra uma vizinha do mesmo tipo e é recusada. A palavra “colada” traduz uma guarda cujo raio atual é **12 horas**. `app/chamados/services/validacao.py:461`; `app/chamados/services/materializacao.py:85`; `app/chamados/services/materializacao.py:875`; `app/chamados/catalogo/recusa.py:26`.

O commit introdutor, o censo de produção e o resultado executado do RED permanecem **[nao verificado]**. Esta cópia não disponibilizou histórico Git; o dado raspado fornece o caso nominal, sem população completa. **[nao verificado]** [nao verificado]

1. **Origem da recusa.**

A recusa concreta deste caminho é:

```python
_viz = _vizinha_mesmo_tipo(disputa.colaborador, ts, _tipo)
if _viz is not None and not _plantio_preenche_furo_na_data(
        disputa.colaborador, ts):
    ...
```

Fonte: `app/chamados/services/materializacao.py:875`.

`_vizinha_mesmo_tipo` procura a batida imediatamente anterior e, depois, a imediatamente posterior, dentro de ±12h. Retorna a primeira vizinha encontrada do mesmo tipo; não filtra aprovação nem `retratada_em`. A exceção consulta se o plantio preenche um furo na célula da **data civil do plantio**. `app/chamados/services/materializacao.py:85`; `app/chamados/services/materializacao.py:142`; `app/chamados/services/materializacao.py:170`.

Quando essa recusa acontece, o código não chega a `registrar_batida`. Se o marco cobrado não estiver comprovadamente aceso, registra a recusa e mantém a pergunta sem materialização; a validação recusada é devolvida ao administrador. `app/chamados/services/materializacao.py:901`; `app/chamados/services/materializacao.py:913`; `app/chamados/services/materializacao.py:926`; `app/chamados/services/validacao.py:464`.

A porta `registrar_batida` **não contém uma recusa geral de duas batidas iguais distantes**. Ela deduplica mesmo tipo próximo, verifica competência para escrita administrativa e pode recusar colisão com tipo oposto. `app/ponto/registro_batida.py:106`; `app/ponto/registro_batida.py:132`; `app/ponto/registro_batida.py:138`.

As tentativas de `git log -L` e `git blame` falharam com `fatal: not a git repository`; portanto, commit, data e mensagem de introdução de cada trecho abaixo são **[nao verificado]**. [nao verificado]

| Trecho | Evidência disponível na cópia | Introdução Git |
|---|---|---|
| Vizinha do mesmo tipo, raio 12h | Comentário atribui a guarda a “fio-quebrado 01/08”. `app/chamados/services/materializacao.py:85` | **[nao verificado]** |
| Recusa antes do plantio | Comentário também menciona 01/08. `app/chamados/services/materializacao.py:872` | **[nao verificado]** |
| Tratamento por lâmpada do marco | Comentário atribui a alteração a 18/10. `app/chamados/services/materializacao.py:889` | **[nao verificado]** |
| Frase “colada / duas seguidas iguais” | Regra `PARIDADE`. `app/chamados/catalogo/recusa.py:26` | **[nao verificado]** |
| Dedup/colisão na porta única | Predicados efetivos da porta. `app/ponto/registro_batida.py:106`; `app/ponto/registro_batida.py:138` | **[nao verificado]** |

O relato associa `225256c1` ao fecho/trava do repasse, `e017181f` a `REPASSE_RECUSADO` e `e9bf5de2` aos consumidores dessa recusa. Isso não prova a introdução da guarda de paridade nesses commits. `app/docs/RELATO.md:27`; `app/docs/RELATO.md:17`; `app/docs/RELATO.md:11`.

**Qual commit introduziu a recusa que trava o caso 960: [nao verificado].** Os comentários apontam antecedentes anteriores à E1; não substituem blame. A identificação do deploy às 01:52 também não foi confirmada: o relato da V2-2 registra pouso às 03:3x. `app/chamados/services/materializacao.py:872`; `app/docs/RELATO.md:3`.

2. **Unidade de trabalho hoje.**

O fluxo de validação é:

```text
view/fio
  → acao_validar_pergunta
  → validar_pergunta(UMA pergunta)
      atomic + travas
      → carimbar validação
      → _materializar_uma(pergunta atual)
      → devolver validação se recusada
  → varrer outras perguntas validadas da disputa
```

Fontes: `app/chamados/views.py:483`; `app/chamados/services/fio.py:585`; `app/chamados/services/acoes_disputa.py:13`; `app/chamados/services/validacao.py:425`; `app/chamados/services/validacao.py:461`; `app/chamados/services/validacao.py:469`.

A varredura seleciona perguntas respondidas, validadas e sem materialização, chamando `_materializar_uma` individualmente. Cada unidade abre seu próprio `atomic`, planta, exige lastro e pede carimbo ao C6; reprovação desfaz aquela unidade. `app/chamados/services/materializacao.py:1369`; `app/chamados/services/materializacao.py:1220`; `app/chamados/services/materializacao.py:1232`; `app/chamados/services/materializacao.py:1277`.

Quem grava a batida é `registrar_batida`, chamado pelo ramo de hora com `autoridade_humana=False` e `fato_de_chao=False`. `app/chamados/services/materializacao.py:926`; `app/ponto/registro_batida.py:157`.

“Regularizar disputa” também entrega as perguntas ao materializador granular; não constitui um ato próprio para o par de intervalo. `app/chamados/services/acoes_disputa.py:139`; `app/chamados/services/fio.py:670`.

Existe ainda “decidir o dia”, via `veredito_celula_view → veredito_celula`, com simulação do conjunto e escrita pela mesma porta, mas esse caminho decide o dia e pode preencher marcos; não é o aceite das duas declarações deste par. `app/ponto/views.py:3312`; `app/ponto/views.py:3340`; `app/ponto/services/veredito_celula.py:223`; `app/ponto/services/veredito_celula.py:277`; `app/ponto/services/veredito_celula.py:322`.

**SEM JUIZ DO PAR DE PERGUNTAS.** Há vínculo por disputa, motivo, dia do turno e posição do marco, mas nenhuma identidade compartilhada da pausa. O código declara expressamente que duas pausas no mesmo dia ainda colidem nos mesmos motivos e que nenhum emissor carrega o índice da pausa. `app/chamados/models.py:2424`; `app/chamados/models.py:2431`; `app/chamados/models.py:2442`; `app/chamados/services/declaracao_texto.py:48`.

Os mais próximos são:

| Autoridade existente | O que responde |
|---|---|
| `intervalos_do_turno` | Intervalos entre **batidas** julgadas pelo pareador. `app/core/juizes.py:202`; `app/ponto/turnos.py:502` |
| `MARCO_DA_PERGUNTA` | Saída → S/hii; volta → E/hfi. `app/chamados/juizes.py:882` |
| `instancia_do_motivo` | Posições `int_ida` e `int_volta`, sem índice compartilhado da pausa. `app/chamados/services/declaracao_texto.py:42` |
| `CelulaDia.PARES_PAUSA_DNA` | Pares canônicos dos **marcos**: hii/hfi e hii2/hfi2. `app/escala/models.py:1652` |
| Emissor de intervalo ausente | Lê `marcos_faltantes` e emite uma pergunta por marco S/E. `app/chamados/services/disputa_emissao.py:1132` |

Preencher arbitrariamente `instancia_marco` para inventar essa identidade é recusado pelo modelo. `app/chamados/models.py:3436`.

3. **Texto e tolerância.**

“Marcação […] colada nesse horário” e “duas seguidas iguais” nascem na regra `PARIDADE`. A tradução reconhece substrings do erro; não mede distância temporal. A casca de validação e o followup usam essa tradução. `app/chamados/catalogo/recusa.py:26`; `app/chamados/catalogo/recusa.py:77`; `app/chamados/services/acoes_disputa.py:32`; `app/chamados/services/materializacao.py:1091`.

| Uso real | Tolerância efetiva | Consequência |
|---|---:|---|
| Guarda de paridade | ±12h | Recusa condicionada ao furo. `app/chamados/services/materializacao.py:85`; `app/chamados/services/materializacao.py:875` |
| Mesmo tipo na porta | ±60s, inclusivos | Reutiliza a existente. `app/ponto/registro_batida.py:106` |
| Reuso do materializador | ±20min | Reutiliza a existente. `app/chamados/juizes.py:235` |
| Colisão de tipo oposto | ±300s com escala/marcos; ±60s sem | Pode recusar. `app/ponto/registro_batida.py:21`; `app/ponto/registro_batida.py:138` |
| Exceção por marco próprio | ±5min | Afasta colisão oposta. `app/ponto/registro_batida.py:35`; `app/ponto/registro_batida.py:145` |

Não existe, nessa regra de apresentação, um predicado próprio de “colada”. Associar a frase automaticamente aos 5min da CCT seria introduzir uma ligação que o código atual não faz. `app/chamados/catalogo/recusa.py:77`; `app/core/models.py:59`.

4. **Desenho de cura — proposto, não implementado. [nao verificado]**

Para o par cuja identidade esteja comprovada, o ato deve:

- Registrar o aceite da primeira metade, preservar a declaração e responder **“falta a outra metade do intervalo”**, sem plantar nem reconciliar parcialmente. **[nao verificado: desenho]**
- Quando ambas estiverem respondidas e validadas, travar o conjunto, reler as duas, derivar os instantes pela função existente e gravar ambas numa transação pela porta única. **[nao verificado: desenho]**
- Julgar alternância e lastro sobre o conjunto completo; qualquer reprovação deve desfazer as duas batidas e os carimbos de aplicação. Só depois reconciliar destinos e avaliar fechamento. **[nao verificado: desenho]**

Um `atomic` ao redor do laço atual não basta: a primeira metade continuaria encontrando a guarda individual antes do plantio. O reparador O330 já julga geometria após um grupo do dia, mas ainda chama `_materializar_uma` para cada pergunta. `app/chamados/services/materializacao.py:875`; `app/chamados/services/passivo_validado.py:474`; `app/chamados/services/passivo_validado.py:516`; `app/chamados/services/passivo_validado.py:536`.

**Limite do desenho:** sem juiz novo, é necessário demonstrar como os juízes existentes de marco/turno e os pares canônicos do DNA identificam inequivocamente as duas perguntas. Esse adaptador não existe hoje; “mesma disputa + mesmo dia” sozinho não resolve a lacuna declarada das duas pausas. `app/escala/models.py:1652`; `app/chamados/services/declaracao_texto.py:48`. O desenho geral ainda não está fechado. **[nao verificado]**

| Sítio/campo | Quem escreve hoje | Quem lê / ponto afetado |
|---|---|---|
| `resposta_colab`, `respondida_em` | Porta de resposta. `app/chamados/services/disputa_emissao.py:1464` | Validação e derivação. `app/chamados/services/validacao.py:393`; `app/chamados/services/materializacao.py:846` |
| `validada_em/por` | `_carimbar_validacao`; devolução da recusa. `app/chamados/services/validacao.py:127`; `app/chamados/services/validacao.py:195` | Seleção e estado da pergunta. `app/chamados/services/materializacao.py:1369`; `app/chamados/models.py:3330` |
| Unidade aplicada | `_materializar_uma`. `app/chamados/services/materializacao.py:1158` | Validação direta e varredura. `app/chamados/services/validacao.py:461`; `app/chamados/services/materializacao.py:1375` |
| Batida: instante/tipo/origem | `registrar_batida`. `app/ponto/registro_batida.py:157` | Juiz do lastro após plantio. `app/chamados/services/materializacao.py:1233` |
| `pergunta_origem` | Materializador por `anotar`. `app/chamados/services/materializacao.py:946` | Lastro vinculado à pergunta. `app/chamados/services/materializacao.py:941` |
| Materialização/via | `PerguntaDisputa.carimbar`. `app/chamados/models.py:3103` | C6 e seletor granular. `app/chamados/models.py:3139`; `app/chamados/services/materializacao.py:1372` |
| Recusa/espera e apresentação | Recusa canônica; casca traduz. `app/chamados/services/materializacao.py:1083`; `app/chamados/services/acoes_disputa.py:32` | Modal e estado de aplicação. `app/templates/chamados/partials/modal_fio.html:225`; `app/chamados/services/materializacao.py:425` |
| Fechamento | `DisputaSupervisao.fechar`. `app/chamados/models.py:1863` | `pode_fechar` e cauda materializadora. `app/chamados/models.py:1856`; `app/chamados/services/materializacao.py:1302` |
| Sequência E1 | `DecisaoPergunta.registrar`. `app/chamados/models.py:3942` | Histórico `respostas_anteriores`. `app/chamados/models.py:3849` |

Os riscos concretos são:

- **Fechamento antecipado:** `validada_em` mata a pergunta no primeiro degrau; guardar uma metade validada exige manter a pendência do par corretamente visível aos leitores e ao fechamento. `app/chamados/models.py:3330`; `app/chamados/models.py:1856`.
- **Reconciliação entre metades:** o ramo atual reconcilia imediatamente após cada plantio; ambas precisam permanecer sob o escopo de aplicação antes dessa cauda. `app/chamados/services/materializacao.py:974`; `app/chamados/services/materializacao.py:1114`.
- **Concorrência:** preservar ordem Colaborador → Disputas → Chamados → Perguntas, incluindo destinos e clones, antes da escrita. `app/chamados/models.py:2637`; `app/chamados/services/validacao.py:431`.
- **Chão e motor:** conservar a separação institucional `fato_de_chao=False`; não transferir essa recusa para a batida de chão nem produzir horas não declaradas. `app/chamados/services/materializacao.py:829`; `app/chamados/services/materializacao.py:927`; `app/ponto/registro_batida.py:129`.
- **Append-only E1:** acrescentar atos pela porta; nunca editar/apagar decisões anteriores nem perder projeção legada sem linha. `app/chamados/models.py:3821`; `app/chamados/models.py:3914`; `app/chamados/models.py:3942`.

5. **Sonda SELECT pronta como texto; não executada. [nao verificado]**

O contador do badge é `exige_o_admin(particionar_verbos(qs_vivos())['contagens'])`. O lavrador persiste esse total em `PlacarNavbar`; `PlacarSituacional` é outro placar. `app/chamados/services/verbos.py:25`; `app/core/context_processors.py:88`; `app/core/management/commands/lavrar_badge_navbar.py:72`; `app/core/management/commands/lavrar_placar_situacional.py:56`.

A sonda abaixo reutiliza derivação, reuso e guarda reais. Não chama materialização nem `registrar_batida`. Como falta juiz do par de perguntas, sua saída se chama **candidatos com dupla recusa**, sem certificar “mesma pausa”. O enquadramento de produção e os resultados continuam **[nao verificado]**.

```python
# Executar no shell Django do tenant já selecionado.
# Proposta não executada [nao verificado].
from collections import Counter, defaultdict
from contextlib import ExitStack
import json
import re
from unittest.mock import patch

from django.db import connections
from django.utils import timezone

from chamados.models import PerguntaDisputa as P
from chamados.services.materializacao import (
    _ts_tipo_do_plantio,
    _batida_existente_na_janela,
    _vizinha_mesmo_tipo,
    _plantio_preenche_furo_na_data,
)
from chamados.services.painel import qs_vivos
from chamados.services.verbos import (
    particionar_verbos, exige_o_admin, VERBOS_DO_ADMIN,
)
from core.models import PlacarNavbar
from ponto.janelas import janela_atual

COLAB_PK = None  # None: população inteira; 960: recorte nominal.

class ForaDoEnvelope(BaseException):
    pass

def somente_select(execute, sql, params, many, context):
    # BaseException evita ser engolida por helpers com except Exception.
    if many or not re.match(r"^\s*SELECT\b", sql, re.I):
        raise ForaDoEnvelope("Sonda interrompida: comando fora de SELECT")
    return execute(sql, params, many, context)

def novo_balde():
    return {
        "classes": Counter(),
        "disputas": set(),
        "disputas_no_badge": set(),
        "chamados_do_badge": set(),
        "colabs_pk": set(),
    }

agora = timezone.now()  # Um relógio para toda a rodada.

with ExitStack() as stack:
    for conn in connections.all():
        stack.enter_context(conn.execute_wrapper(somente_select))
    stack.enter_context(patch("django.utils.timezone.now", return_value=agora))

    # Fonte real do total e da pertença aos verbos do administrador.
    part = particionar_verbos(qs_vivos())
    badge = exige_o_admin(part["contagens"])
    admin_ids = {
        pk for pk, verbo in part["mapa"].items()
        if verbo in VERBOS_DO_ADMIN
    }
    assert len(admin_ids) == badge

    lavrado = PlacarNavbar.objects.filter(pk=1).values(
        "badge_chamados", "lavrado_em"
    ).first()
    if lavrado and lavrado["lavrado_em"]:
        lavrado["lavrado_em"] = timezone.localtime(
            lavrado["lavrado_em"]
        ).isoformat()

    perguntas = (
        P.objects.filter(
            disputa__fechada_em__isnull=True,
            motivo__in=("intervalo_saida", "intervalo_volta"),
            conversa=False,
        )
        .exclude(via_resolucao__in=P.VIAS_FORA_DO_FATO)
        .select_related(
            "disputa__colaborador__empresa", "chamado_destino", "celula"
        )
        .order_by("disputa_id", "data_evento", "pk")
    )
    if COLAB_PK is not None:
        perguntas = perguntas.filter(disputa__colaborador_id=COLAB_PK)

    grupos = defaultdict(lambda: defaultdict(list))
    for p in perguntas:
        grupos[(p.disputa_id, p.data_evento)][p.motivo].append(p)

    baldes = defaultdict(novo_balde)
    todas_disputas = set()
    todas_no_badge = set()

    for (disputa_pk, dia), metades in grupos.items():
        linhas = [p for lista in metades.values() for p in lista]
        col = linhas[0].disputa.colaborador

        if dia is None:
            chave = (str(col.empresa_id), "SEM_DATA", "", "")
        else:
            ini, fim = janela_atual(dia, col.empresa)
            chave = (
                str(col.empresa_id), fim.strftime("%Y-%m"),
                ini.isoformat(), fim.isoformat(),
            )
        b = baldes[chave]

        saidas = metades["intervalo_saida"]
        voltas = metades["intervalo_volta"]
        if dia is None:
            b["classes"]["sem_data"] += 1
            continue
        if not saidas or not voltas:
            b["classes"]["sem_duas_metades"] += 1
            continue
        if len(saidas) != 1 or len(voltas) != 1:
            b["classes"]["multiplas_linhas_ambiguas"] += 1
            continue

        par = [saidas[0], voltas[0]]
        if any(p.materializada_em is not None for p in par):
            b["classes"]["com_carimbo_nao_reaplicado_pela_porta_atual"] += 1
            continue
        if any(
            p.respondida_em is None or not (p.resposta_colab or "").strip()
            for p in par
        ):
            b["classes"]["sem_duas_respostas"] += 1
            continue

        recusas = []
        derivacao_ok = True
        for p in par:
            erros = []
            instante, tipo = _ts_tipo_do_plantio(p.disputa, p, erros)
            if instante is None:
                derivacao_ok = False
                break

            # Composição dos helpers efetivamente usados no plantio.
            existente = _batida_existente_na_janela(col, instante, tipo)
            vizinha = (
                _vizinha_mesmo_tipo(col, instante, tipo)
                if existente is None else None
            )
            recusas.append(
                existente is None
                and vizinha is not None
                and not _plantio_preenche_furo_na_data(col, instante)
            )

        if not derivacao_ok:
            b["classes"]["derivacao_indisponivel"] += 1
            continue
        if not all(recusas):
            b["classes"]["sem_dupla_recusa"] += 1
            continue

        b["classes"]["candidato_dupla_recusa"] += 1
        b["disputas"].add(disputa_pk)
        b["colabs_pk"].add(col.pk)
        todas_disputas.add(disputa_pk)

        # Associação explicitamente por pai/destinos do candidato.
        # Não confunde quantidade de disputas com quantidade de chamados.
        vinculados = {par[0].disputa.chamado_id}
        vinculados.update(p.chamado_destino_id for p in par)
        comuns = (vinculados - {None}) & admin_ids
        if comuns:
            b["disputas_no_badge"].add(disputa_pk)
            b["chamados_do_badge"].update(comuns)
            todas_no_badge.add(disputa_pk)

    saida = []
    for chave, b in sorted(baldes.items()):
        empresa_pk, competencia, ini, fim = chave
        saida.append({
            "empresa_pk": empresa_pk,
            "competencia": competencia,
            "inicio": ini,
            "fim": fim,
            "classes": dict(b["classes"]),
            "disputas_candidatas": len(b["disputas"]),
            "disputas_com_chamado_admin_vinculado": len(
                b["disputas_no_badge"]
            ),
            "chamados_admin_vinculados

[truncado em 20000 de 30506 caracteres]

## 09/11 07:48 · sessao 652ccdb4046d · - · - · origem=Ronald (tmux)

B/C concluídos em leitura; scripts e referências enviados ao agente principal.

- Ausência: cobertura `app/ponto/turnos.py:2511`, `:2735`; grade `app/escala/utils.py:1188`; Resolver `app/ponto/services/veredito_celula.py:237`; motor `app/ponto/services/fechamento.py:683`, `:751`; desconto `app/ponto/motor_calculo_v2.py:1085`; abono diário `app/ponto/services/dia_pago.py:364`; lavra `app/ponto/services/fechamento.py:1191`; exportação `app/folha/export.py:717`.
- Caso col39/15-10 aparece como precedente da regra que abona apenas o previsto não coberto pelas batidas, sem demonstrar o estado atual: `app/ponto/services/dia_pago.py:375`.
- Pareamento: entrada pela porta `app/ponto/turnos.py:2846`, alimentação canônica `:1918`, chamada ao pareador `:1992`.
- Espúrias: detecção `app/ponto/management/commands/detectar_par_relampago.py:63`, guardas `:158`, simulação `:229`, pares embutidos `:275`.
- Geo: juiz puro `app/ponto/services/geofence.py:95`; escritor com alertas/push não deve ser chamado na sonda, `:232`.
- Dono: usar `InsumosDoDono.dono`, que chama `dono_da_divergencia`, sem reproduzir seus critérios: `app/ponto/management/commands/e6_oraculo.py:182`, `:53`.
- Donos concretos e ponto atual da perda de cobertura dos alvos: [não verificado].
- Padrões candidatos quando comprovada ESTRUTURA: Single Source of Truth e CQRS com projeções reconstruíveis, `app/docs/GLOSSARIO.md:11`, `:22`.

## 09/11 07:47 · sessao b18df0b7c494 · - · - · origem=Ronald (tmux)

D — achados e sonda para consolidação. Nenhum arquivo criado e nenhum acesso ao banco executado.

1. O “8” é soma de unidades diferentes: chamados de premissa morta + perguntas absolvidas + chamados com rejunção pendente + chamados absolvidos pela célula + eventos mortos + chamados cujo juiz falhou. Não é contagem distinta de chamados. Fonte: `app/chamados/management/commands/reconciliar_chamados.py:118`.

2. No último trecho fornecido, `8 = 2 perguntas absolvidas + 6 chamados absolvidos pela célula`; o `16` anterior era `1 + 15`. Esses números vêm exclusivamente do anexo colado, sem arquivo correspondente na cópia: **[não verificado]**.

3. O vigia “premissa morta” desconta quem tem evento pendente por colaborador e exclui retenção/fecho recusado; “célula absolveu” apenas verifica ausência de evento pendente da célula e **não desconta retenção nem fecho recusado**. Portanto, parte dos seis pode ser retenção legítima, sem defeito de entrega. Fontes: `app/chamados/management/commands/reconciliar_chamados.py:62`, `app/chamados/management/commands/reconciliar_chamados.py:73`, `app/chamados/management/commands/reconciliar_chamados.py:79`, `app/chamados/reconciliador.py:1269`.

| Subclasse | Evento/consumidor e alcance demonstrado |
|---|---|
| Premissa morta | Recompute que remove um turno aberto emite pelo mesmo atomic, condicionado a existir célula: `app/ponto/nucleo.py:60`, `app/ponto/nucleo.py:86`. Nascimento de chamado de turno também emite: `app/chamados/models.py:974`, `app/chamados/models.py:1093`. Consumidor reavalia premissa: `app/chamados/reconciliador.py:1369`. Relógio/deploy que muda a premissa sem recompute continua sem produtor declarado: `app/chamados/management/commands/reconciliar_chamados.py:21`. |
| Pergunta absolvida | Save de veredito/ata emite: `app/ponto/portas/celula.py:878`, `app/ponto/portas/celula.py:894`. Consumidor chama `retratar_perguntas_absolvidas_por_celula`: `app/chamados/reconciliador.py:1365`. O juiz efetivo é `pergunta_viva`, depois a tranca: `app/chamados/reconciliador.py:650`, `app/chamados/reconciliador.py:661`. |
| Rejunção | Mesmo evento; consumidor liga primeiro a célula, antes da absolvição: `app/chamados/reconciliador.py:1363`. Regeneração também chama diretamente após commit: `app/ponto/portas/celula.py:493`. O candidato exige vivo, escopo DIA, célula nula e célula existente: `app/chamados/rejuncao.py:59`, `app/chamados/rejuncao.py:81`. |
| Célula absolveu | Mesmo evento; consumidor chama o retratador: `app/chamados/reconciliador.py:1368`. Predicado exige célula com veredito que não acusa e sem marcos pendentes: `app/chamados/reconciliador.py:1018`. A retratação ainda passa por retenção e `reconciliar()`: `app/chamados/reconciliador.py:518`. |
| Eventos mortos | Não falta necessariamente emissor: evento com ≥5 tentativas e sem processamento sai da fila viva. Fontes: `app/chamados/reconciliador.py:1207`, `app/chamados/reconciliador.py:1246`, `app/chamados/reconciliador.py:1252`. Falhas de chamados/premissa deixam o evento pendente e incrementam tentativas: `app/chamados/reconciliador.py:1389`. |
| Juiz falhou | É exceção do julgamento de premissa para chamado sem evento pendente, contada pelo vigia: `app/chamados/management/commands/reconciliar_chamados.py:73`, `app/chamados/management/commands/reconciliar_chamados.py:82`. No consumidor, falha da premissa é acumulada e retém o evento: `app/chamados/reconciliador.py:1462`. |
| Fantasma com “morfologia” | Morfologia é apenas diagnóstico; quem conta como fantasma é núcleo fechado nas datas + premissa do catálogo morta: `app/ponto/management/commands/reconciliar_fantasmas.py:199`, `app/ponto/management/commands/reconciliar_fantasmas.py:245`, `app/ponto/management/commands/reconciliar_fantasmas.py:256`. Mesmo caminho de evento/retratador da premissa; sem célula, o recompute não emite: `app/ponto/nucleo.py:86`. |

Há um mecanismo concreto de resíduo apesar de evento marcado como processado: os caminhos da pergunta e da rejunção engolem exceções, enquanto o consumidor os chama sem coletor `falhas`; depois marca o evento processado se a lista continuar vazia. Isso é possibilidade demonstrada pelo código, **não prova do ocorrido nos resíduos fornecidos**. Fontes: `app/chamados/reconciliador.py:601`, `app/chamados/reconciliador.py:615`, `app/chamados/reconciliador.py:669`, `app/chamados/rejuncao.py:90`, `app/chamados/rejuncao.py:109`, `app/chamados/reconciliador.py:1364`, `app/chamados/reconciliador.py:1390`.

Também há processamento sem fechamento por decisão legítima: `_retratar` retorna `False` sob retenção; isso não é falha para o consumidor. O contador “célula absolveu” e o contador de fantasmas podem continuar contando esse chamado. Fontes: `app/chamados/reconciliador.py:437`, `app/chamados/reconciliador.py:518`, `app/chamados/reconciliador.py:1269`, `app/ponto/management/commands/reconciliar_fantasmas.py:264`.

O conjunto completo de fantasmas colado tem 36 linhas: 11 diagnósticos de morfologia, 2 sem abertura, 2 incompletos e 21 `ok`; 13 têm pergunta e 23 não. O 38 não aparece no dado fornecido: **[não verificado]**. Não somar esses 36 ao alarme do reconciliador: universos e unidades diferem e podem sobrepor. Fontes dos universos: `app/ponto/management/commands/reconciliar_fantasmas.py:157`, `app/chamados/management/commands/reconciliar_chamados.py:59`, `app/chamados/reconciliador.py:1201`.

O evento não registra origem nem payload; registra célula/colaborador/dia e estado de processamento. Processados são apagados após sete dias. Portanto, o código e os logs colados não permitem atribuir a cada resíduo qual emissor faltou historicamente. Fontes: `app/escala/models.py:1845`, `app/escala/models.py:1857`, `app/chamados/reconciliador.py:1404`.

Dono provável do mecanismo sem produtor ou de exceção engolida: **ESTRUTURA**, por inferência de falha de entrega/consumo; padrão **Transactional Outbox / O-OUTBOX**, e, para morte por relógio/deploy sem evento, **sinal derivado do estado, reconciliação level-triggered**. Fontes dos padrões: `app/docs/GLOSSARIO.md:13`, `app/docs/GLOSSARIO.md:17`, `app/docs/PLANO-PADROES.md:10`. Retenção `irma_orfa` exige validação humana; não autoriza chamar o caso de bug estrutural. Fonte: `app/chamados/reconciliador.py:441`.

Sonda D, para executar somente depois do envelope seguro. Ela usa os predicados existentes, agrega por classe e não imprime nomes, CPF ou IDs. O guard deve ser instalado **antes** destes imports; não executar consumidor, semeadura nem comando com `--apply`. Fontes dos predicados: `app/chamados/reconciliador.py:645`, `app/chamados/rejuncao.py:66`, `app/chamados/reconciliador.py:1018`, `app/ponto/management/commands/reconciliar_fantasmas.py:245`.

```python
# Dentro do envelope SELECT-only já instalado.
# Não envolver em transaction.atomic(): reconciliaria() muda o caminho
# para trava_na_ordem quando in_atomic_block.
# Fontes: chamados/models.py:657 e :705-711.
import json
from collections import Counter
from django.db import connection
from django.db.models import Count

assert connection.get_autocommit() and not connection.in_atomic_block

from chamados.catalogo.motor import VIVOS
from chamados.catalogo.premissa import avaliar, FATO_TURNO_ABERTO
from chamados.juizes import dia_do_fato
from chamados.models import ChamadoColaborador
from chamados import reconciliador as r
from chamados.rejuncao import _candidatos
from colaboradores.models import Colaborador
from escala.models import EscalaColaborador, EventoCelula
from ponto.management.commands.reconciliar_fantasmas import (
    MODULOS_ALVO, _parse_pendentes, _completude,
)

conta = Counter()
alarme = Counter()
pendentes_colab = set(
    r.eventos_pendentes().values_list("colaborador_id", flat=True)
)
pendentes_celula = set(
    r.eventos_pendentes().values_list("celula_id", flat=True)
)

def add(classe, modulo="", subcategoria="", causa="", diagnostico=""):
    conta[(classe, modulo or "", subcategoria or "", causa, diagnostico)] += 1

def diagnostico_do_fecho(ch):
    try:
        ret = r.retencao_da_retratacao(ch)
        if ret is not None:
            return "retencao:" + ret[0]  # não imprime os IDs do detalhe
        return "fecharia" if ch.reconciliaria() else "gate_recusa"
    except Exception as exc:
        return "falha_gate:" + type(exc).__name__

# Universo literal do vigia de premissa.
for ch in ChamadoColaborador.objects.filter(
    status_local__in=VIVOS
).select_related("colaborador").order_by("pk").iterator():
    if ch.colaborador_id in pendentes_colab:
        continue
    try:
        vd = r._veredito_premissa(ch)
        if vd is None or vd.viva:
            continue
        ret = r.retencao_da_retratacao(ch)
        if ret is not None or not ch.reconciliaria():
            add("premissa_morta_retida_MEDIDA",
                ch.modulo_origem, ch.subcategoria, vd.motivo,
                "retencao:" + ret[0] if ret else "gate_recusa")
            continue
    except Exception as exc:
        add("juiz_falhou", ch.modulo_origem, ch.subcategoria,
            type(exc).__name__)
        alarme["juiz_falhou"] += 1
        continue
    add("premissa_morta_sem_evento", ch.modulo_origem,
        ch.subcategoria, vd.motivo)
    alarme["premissa_morta"] += 1

# Os geradores abaixo são os mesmos lidos pelo vigia.
for colab in Colaborador.objects.all().order_by("pk").iterator():
    if colab.pk in pendentes_colab:
        continue
    for p in r.perguntas_absolvidas_pela_celula(colab):
        add("pergunta_absolvida_sem_evento", causa=p.motivo)
        alarme["pergunta_absolvida"] += 1
    for ch, cel in _candidatos(colab):
        add("rejuncao_sem_evento", ch.modulo_origem, ch.subcategoria)
        alarme["rejuncao"] += 1

# Reproduz a contagem de célula SEM retirar retenções,
# e acrescenta o diagnóstico separado do motivo de não fechar.
for ch in r._chamados_de_turno_vivos().iterator():
    achado = r._absolvido_pela_celula(ch)
    if achado is None:
        continue
    dia, cel = achado
    if cel.pk not in pendentes_celula:
        add("celula_absolveu_sem_evento", ch.modulo_origem,
            ch.subcategoria, cel.veredito,
            diagnostico_do_fecho(ch))
        alarme["celula_absolveu"] += 1

for row in EventoCelula.objects.filter(
    processado_em__isnull=True,
    tentativas__gte=r.MAX_TENTATIVAS_EVENTO,
).values("tentativas").annotate(n=Count("pk")).order_by("tentativas"):
    n = row["n"]
    conta[("eventos_mortos", "", "",
           "tentativas=" + str(row["tentativas"]), "")] += n
    alarme["eventos_mortos"] += n

# Universo literal de reconciliar_fantasmas.
for ch in ChamadoColaborador.objects.filter(
    modulo_origem__in=MODULOS_ALVO,
    status_local__in=VIVOS,
).select_related("colaborador").order_by("pk").iterator():
    dia = dia_do_fato(ch)
    ctx = ch.contexto_json or {}
    if dia:
        datas = [dia]
    elif ch.modulo_origem == "entrada_bloqueada_turno_aberto":
        datas = _parse_pendentes(ctx.get("pendentes"))
    else:
        datas = []
    if not datas:
        add("fantasma_skip_sem_data", ch.modulo_origem, ch.subcategoria)
        continue
    if any(r._tem_turno_aberto(ch.colaborador, dia=d) for d in datas):
        add("fantasma_legitimo_nucleo", ch.modulo_origem, ch.subcategoria)
        continue
    vd = avaliar(ch.modulo_origem, ch.subcategoria,
                 fatos={FATO_TURNO_ABERTO: False})
    if not vd.escopo or vd.viva:
        add("fantasma_skip_catalogo", ch.modulo_origem, ch.subcategoria)
        continue

    ec = EscalaColaborador.objects.filter(
        colaborador_id=ch.colaborador_id, ativa=True
    ).select_related("tipo_escala").first()
    diags = set()
    if ec and ec.tipo_escala:
        te = ec.tipo_escala
        tem_int = bool(te.hora_inicio_intervalo and te.hora_fim_intervalo)
        for d in datas:
            motivo = _completude(ch.colaborador_id, ec, te, tem_int, d)[4]
            # Apenas rótulo agregado do diagnóstico já produzido.
            diags.add(
                "morfologia" if motivo.startswith("morfologia:") else
                "incompleto" if motivo.startswith("incompleto ") else motivo
            )
    else:
        diags.add("sem_escala_ativa_sem_diagnostico")
    add("fantasma", ch.modulo_origem, ch.subcategoria,
        "|".join(sorted(diags)), diagnostico_do_fecho(ch))

print(json.dumps({
    "alarme_por_subclasse": dict(alarme),
    "alarme_soma_unidades_heterogeneas": sum(alarme.values()),
    "classes": [
        {
            "classe": k[0], "modulo": k[1], "subcategoria": k[2],
            "causa": k[3], "diagnostico": k[4], "n": n,
        }
        for k, n in sorted(conta.items())
    ],
}, ensure_ascii=False, sort_keys=True))
```

Limitação: o resultado descreve o estado observado durante a leitura, não reconstrói por si a história de entrega nem elimina concorrência entre SELECTs. **[não verificado]**

## 09/11 07:47 · sessao 2d3b248ff368 · - · - · origem=Ronald (tmux)

A — constatações e referências

- O badge 1.177 é produzido por `lavrar_badge_navbar.Command.handle`, que chama `exige_o_admin(ctx['verbos_contagens'])`, soma de `validar + decidir`; unidade = **chamado**, não pergunta ou toque. `app/core/management/commands/lavrar_badge_navbar.py:39`, `app/core/management/commands/lavrar_badge_navbar.py:72`, `app/chamados/services/verbos.py:22`.
- O universo dessa partição é `qs_vivos`: `ChamadoColaborador.objects.filter(status_local__in=VIVOS)`, sem filtros opcionais no lavrador; `VIVOS = ('aberto','em_analise','registrado')`. A partição pode devolver `None`, `arquivado` ou `historico`, que não entram em `validar + decidir`. `app/chamados/services/painel.py:155`, `app/chamados/services/painel.py:228`, `app/chamados/catalogo/motor.py:24`, `app/chamados/services/verbos.py:47`.
- `lavrar_placar_situacional` produz contadores de **colaboradores em operação**, não o badge dos chamados. `app/core/management/commands/lavrar_placar_situacional.py:54`, `app/core/management/commands/lavrar_placar_situacional.py:63`.
- No stale: `chamados_vivos=vivos.count()` conta chamados; `universo=universo.count()` conta perguntas cujo **destino** está vivo; `perguntas_stale_agora=len(stale)` conta perguntas desatualizadas; `lavradas=n` conta perguntas processadas nesta rodada; `sobra=max(len(stale)-teto,0)`, teto padrão 500. Não existe equivalência entre essas cinco grandezas. `app/chamados/management/commands/lavrar_perguntas_stale.py:35`, `app/chamados/management/commands/lavrar_perguntas_stale.py:56`, `app/chamados/management/commands/lavrar_perguntas_stale.py:60`, `app/chamados/management/commands/lavrar_perguntas_stale.py:71`, `app/chamados/management/commands/lavrar_perguntas_stale.py:84`.
- **7.716 exato não está nos anexos stale apresentados**; atribuí-lo a uma execução desse contador é `[não verificado]`. A busca literal encontrou `7.716` apenas como alvo de 98% do oráculo, unidade dia-colab, em documentação histórica — não como aquele contador de chamados/perguntas. `app/docs/RELATO-ARQUIVO.md:23683`, `app/docs/RELATO-ARQUIVO.md:23695`.
- **4.323 exato também não aparece nas linhas stale raspadas** `[não verificado]`; se a referência é `chamados_vivos`, sua unidade é chamado e não `lavradas/sobra`. `app/chamados/management/commands/lavrar_perguntas_stale.py:71`.

Matriz dos juízes:

| Rótulo pedido | Autoridade existente e limite |
|---|---|
| legítimo | **SEM JUIZ** composto; premissa viva não prova legitimidade geral. `app/chamados/catalogo/premissa.py:128` |
| repetido | Registro aponta `ChamadoColaborador.abrir`, **escritor**, impróprio para sonda; não há juiz de leitura agregado equivalente. `app/core/juizes.py:268` |
| sem porta | `porta_alcancavel_do_dia`; resultado `None`. É porta do corte, não todos os botões manuais existentes. `app/core/juizes.py:277`, `app/chamados/juizes.py:1361`, `app/chamados/juizes.py:1387` |
| premissa superada | `premissa_morta`; alimentação de leitura existente `_veredito_premissa`. Fora de escopo ou fato não coletado não significa legítimo. `app/core/juizes.py:263`, `app/chamados/reconciliador.py:247`, `app/chamados/catalogo/premissa.py:125` |
| exige humano | Verbo corrente `validar/decidir`, pela partição existente; não há juiz explícito desse rótulo global no registro. `app/chamados/services/verbos.py:22`, `app/chamados/catalogo/natureza.py:57` |
| pergunta viva/morta | C6 `pergunta_viva`. Duplicata retratada é identidade terminal, não prova de censo completo de repetição ativa. `app/core/juizes.py:266`, `app/chamados/juizes.py:1163`, `app/chamados/juizes.py:1182` |

Os rótulos são **eixos sobrepostos**, não partição exclusiva autorizada por algum juiz. Criar precedência entre eles seria predicado novo. `app/core/juizes.py:260`.

Histórico:

- Pergunta não tem timestamp de nascimento; `data_evento` é data do fato. `app/chamados/models.py:2414`, `app/chamados/models.py:2431`.
- `DecisaoPergunta` registra resposta, validação, devolução, recusa, reabertura, silêncio, retratação e absorção; não nascimento nem todas as mudanças de C6 causadas por insumos. `app/chamados/models.py:3839`, `app/chamados/models.py:3857`.
- Chamado tem `criado_em` e `resolvido_em`, mas estado pode reabrir; timestamps atuais não fornecem todos os intervalos históricos. A trilha de transição é best-effort. `app/chamados/models.py:60`, `app/chamados/models.py:67`, `app/chamados/models.py:292`, `app/core/observ.py:94`.
- Logo, a sonda enviada por mensagem mede estoque atual por competência/classe e nascimento registrado de chamados antes/depois do deploy. **Nasce/permanece/encerra históricos completos por C6/classe antes/depois = `[não verificado]`**, sem snapshots/insumos históricos. Não apresentar `resolvido_em` como fluxo completo. [nao verificado]
- Competências saem de `competencia_do_chamado` e `janela_atual`; corte efetivo respeita `Empresa.dia_inicio_competencia`, com 21 como default. `app/core/juizes.py:274`, `app/core/juizes.py:283`, `app/ponto/janelas.py:36`.

Dono provável de uma divergência real entre contador e sua própria lista: **ESTRUTURA**, padrão *Single Source of Truth / Aggregate Root*. A diferença entre universos corretamente rotulados não demonstra divergência estrutural. `app/docs/LEIS.md:122`, `app/docs/GLOSSARIO.md:11`.

## 09/11 07:47 · sessao e8248f22495a · SELOS-CANON-I1 · leitura · origem=Code

**Há dois furos efetivos no Agent na matriz pedida:** `O9 (C)` × célula `O9` e `O9 NOTURNO-X` × célula `O9`. A pertença retorna `False`; `_censo_nega` passa antes de examinar briefs. O wrapper `main_censo` recusa essas entradas qualificadas antes da pertença; portanto, seu `rc=1` sozinho não mata esse mutante. (`bin/disjuntor.py:1683–1686`, `bin/disjuntor.py:1702–1703`, `bin/disjuntor.py:2246–2248`)

**1. MATRIZ**

Cada entrada abaixo tem a forma **`P / A / W → LEI`**:

| Símbolo | Significado e ramo |
|---|---|
| `P` | `_pertence_fila1` recebendo literalmente a obra indicada. (`bin/disjuntor.py:1669`) |
| `A` | Pertença alcançada pelo Agent, após `_obra_de` e `_identidade`. (`bin/disjuntor.py:602–627`, `bin/disjuntor.py:1844–1848`) |
| `W` | Pertença alcançada por `main_censo`. (`bin/disjuntor.py:2246–2265`) |
| `T` | `True`, ramo `bin/disjuntor.py:1687–1688`. |
| `F` | `False`, ramo `bin/disjuntor.py:1685–1686`. |
| `N` | NAO SEI: `ValueError`, ramo `bin/disjuntor.py:1689–1690`. |
| `—A` | Obra ilegível no Agent; recusa anterior, sem chamar a pertença. (`bin/disjuntor.py:619–621`, `bin/disjuntor.py:1845–1847`) |
| `—W` | Obra fora da forma aceita por `main_censo`; `rc=1` anterior à pertença. (`bin/disjuntor.py:2246–2248`) |
| `d / f / n` | Pela lei: dentro / fora / NAO SEI. Identidade única, ambiguidades e fronteiras seguem o contrato existente; NAO SEI nega. (`bin/disjuntor.py:1674–1677`, `bin/hook_stop_fila1.py:117`, `bin/disjuntor.py:44–50`) |
| **`F!`** | **Furo: HOJE FORA, pela lei NAO SEI.** Falta consultar o token inteiro da célula dentro da obra; hoje só existe o sentido obra→célula. (`bin/disjuntor.py:1683–1686`) |

Na coluna **prosa**, a célula é `OUTRA-X (cita <obra inteira da linha>)`: seu token é outro, mas ela nomeia integralmente a obra. Isso produz ambiguidade, sem depender de substring ou de interpretar apenas parte da cauda. (`bin/disjuntor.py:1663–1666`, `bin/disjuntor.py:1684–1690`, `bin/hook_stop_fila1.py:117`)

| OBRA apresentada | Célula `O9` | Célula `O9 (C)` | Células `OBRA-Q (A)` + `OBRA-Q (B)` | Outro token + prosa | Célula `O142` |
|---|---|---|---|---|---|
| `O9` | T/T/T → d | N/N/N → n | F/F/F → f | N/N/N → n | F/F/F → f |
| `O9 (C)` | **F!/F!/—W → n** | T/T/—W → d | F/F/—W → f | N/N/—W → n | F/F/—W → f |
| `O9 NOTURNO-X` | **F!/F!/—W → n** | F/F/—W → f | F/F/—W → f | N/N/—W → n | F/F/—W → f |
| `OBRA-Q` | F/F/F → f | F/F/F → f | N/N/N → n | N/N/N → n | F/F/F → f |
| `OBRA-Q (A)` | F/F/—W → f | F/F/—W → f | N/N/—W → n | N/N/—W → n | F/F/—W → f |
| `o9` | N/—A/N → n | N/—A/N → n | F/—A/F → f | N/—A/N → n | F/—A/F → f |
| `09-TURNO-ABERTO-EXPOSTA` | F/—A/F → f | F/—A/F → f | F/—A/F → f | N/—A/N → n | F/—A/F → f |
| `O67b` | F/—A/F → f | F/—A/F → f | F/—A/F → f | N/—A/N → n | F/—A/F → f |
| `O1` — controle de prefixo | F/F/F → f | F/F/F → f | F/F/F → f | N/N/N → n | **F/F/F → f** |

Os ramos de cada símbolo estão identificados na legenda. As diferenças de leitura entre linhas vêm de `_OBRA_ID`, `_obra_lida` e `_identidade`; em particular, o Agent preserva a cauda de `O9`, mas transforma `OBRA-Q (A)` em `OBRA-Q`. (`bin/disjuntor.py:157–168`, `bin/disjuntor.py:619–627`)

Para completar as **formas alinhadas ao próprio ID**, sem deixar dígito inicial, minúscula final e hífen limitados a cruzamentos com IDs alheios:

| OBRA apresentada | Célula(s) | P / A / W → LEI |
|---|---|---|
| `OBRA-Q` | `OBRA-Q` | T/T/T → d |
| `OBRA-Q` | `OBRA-Q (A)` | T/T/T → d |
| `OBRA-Q (A)` | `OBRA-Q` | **F!/T/—W → n no argumento literal; d após identidade do Agent** |
| `OBRA-Q (A)` | `OBRA-Q (A)` | N/T/—W → n no argumento literal; d após identidade do Agent |
| `O9 NOTURNO-X` | `O9 NOTURNO-X` | T/T/—W → d |
| `o9` | `o9` | T/—A/T → d |
| `09-TURNO-ABERTO-EXPOSTA` | mesmo ID nu | T/—A/T → d |
| `09-TURNO-ABERTO-EXPOSTA` | mesmo ID + ` (C)` | N/—A/N → n |
| `O67b` | `O67b` | T/—A/T → d |
| `O67b` | `O67b (C)` | N/—A/N → n |
| `O142` | `O1` | F/F/F → f |

Esses resultados usam o aceite integral de célula nua por `OBRA_CENSO_ID`, a validação do token qualificado e os três ramos da pertença. O **F! literal de `OBRA-Q (A)` não abre vaga por essas interfaces**: o Agent normaliza a identidade; `main_censo` recusa o espaço antes da consulta. (`bin/disjuntor.py:162`, `bin/disjuntor.py:619–627`, `bin/disjuntor.py:1661–1690`, `bin/disjuntor.py:2246–2248`)

Quando alcançada, a pertença tem consequências distintas:

| Resultado | Agent / `_censo_nega` | Wrapper / `main_censo` |
|---|---|---|
| `F` | Retorna `None` antes do censo. (`bin/disjuntor.py:1702–1703`) | Grava `CENSO FORA`, retorna `0`. (`bin/disjuntor.py:2269–2272`) |
| `T` | Continua para forma e briefs; `True` não significa censo aprovado. (`bin/disjuntor.py:1704–1727`) | Chama `_censo_nega` com a mesma fila. (`bin/disjuntor.py:2273`) |
| `N` | Exceção vira negativa NAO SEI. (`bin/disjuntor.py:1728–1729`) | Exceção vira negativa e `rc=1`. (`bin/disjuntor.py:2266–2267`, `bin/disjuntor.py:2274–2279`) |

O shell construtor ainda tem uma barreira anterior mais restrita que `main_censo`: somente `[A-Z0-9-]+`. Assim, o aceite de `o9`/`O67b` no CLI acima não significa que o shell construtor os aceite. (`bin/codex_pergunta.sh:188`, `bin/disjuntor.py:162`, `bin/disjuntor.py:2246`)

**2. SELOS E MUTANTES**

O selo existente cobre obra nua→célula qualificada e prepara uma célula `O9` nua e única em `:580`; falta experimentar a obra qualificada contra essa célula antes de restaurar o BACKLOG em `:583`. (`bin/tests/test_censo_antes_da_vaga.sh:544–548`, `bin/tests/test_censo_antes_da_vaga.sh:564–566`, `bin/tests/test_censo_antes_da_vaga.sh:576–583`)

**Linhas propostas para esse ponto**, cobrindo os dois furos efetivos. Usam os helpers e o brief ruim já existentes; proposta não aplicada nem executada. (`bin/tests/test_censo_antes_da_vaga.sh:71–74`, `bin/tests/test_censo_antes_da_vaga.sh:202–214`, `bin/tests/test_censo_antes_da_vaga.sh:520–527`; execução: [nao verificado])

```bash
for o in 'O9 (C)' 'O9 NOTURNO-X'; do
  s="$(_pre "$B" "$(_pl_agent general-purpose \
    "Construa a obra $o na copia $WT. Brief: logs/e3/brief_ruim.md")")"
  _nega_censo_de "pertenca-agent-espelho-$o" "$o" "$s" || ok=1
  _sem_fora "pertenca-agent-espelho-$o" "$o" || ok=1

  rc="$(_censo "$B" "$o")"
  [ "$rc" = 1 ] || {
    _falha "pertenca-wrapper-qualificada-$o" "rc=$rc, esperado 1"
    ok=1
  }
done
```

| Furo | Caso que o morde | Mutante visado |
|---|---|---|
| `O9 (C)` × `O9` | Iteração Agent `O9 (C)` acima. | Retirar o reconhecimento reverso token→obra, restaurando apenas o sentido atual de `bin/disjuntor.py:1684`. |
| `O9 NOTURNO-X` × `O9` | Iteração Agent `O9 NOTURNO-X` acima. | Mesmo mutante; também detecta reconhecimento reverso limitado a cauda entre parênteses. (`bin/disjuntor.py:164`, `bin/disjuntor.py:1684`) |
| Literal `OBRA-Q (A)` × `OBRA-Q` | Asserção direta abaixo; no Agent a normalização encobre esse F literal. | Mesmo mutante, aplicado ao predicado diretamente. (`bin/disjuntor.py:627`, `bin/disjuntor.py:1683–1686`) |

**Limite da exigência “nas duas vias”:** as iterações wrapper acima são controles da recusa de forma. Elas já passam com a pertença vulnerável e **não matam o mutante da pertença**, porque `main_censo` termina antes de chamá-la. Trocar apenas o rótulo `via` de `_censo_nega` também não prova a integração de `main_censo`. (`bin/disjuntor.py:1693`, `bin/disjuntor.py:2246–2248`, `bin/disjuntor.py:2265`)

Para morder diretamente os três F literais, a asserção pronta para `casos_pertenca` é esta; aceita `True` ou NAO SEI, mas reprova FORA. Proposta não executada. (`bin/disjuntor.py:1686–1690`; execução: [nao verificado])

```bash
python3 -B - "$B" <<'ESPELHO' || {
  _falha pertenca-espelho-direto "obra relacionada caiu em FORA"
  ok=1
}
import sys
sys.path.insert(0, sys.argv[1])
import disjuntor as D

for obra, celula in (
    ('O9 (C)', 'O9'),
    ('O9 NOTURNO-X', 'O9'),
    ('OBRA-Q (A)', 'OBRA-Q'),
):
    try:
        resultado = D._pertence_fila1(obra, [celula])
    except ValueError:
        continue
    if resultado is False:
        raise SystemExit(f'FORA indevido: {obra!r} x {celula!r}')
ESPELHO
```

Os mutantes atuais cobrem célula crua, exato que cala outra, dois candidatos, decisão própria do wrapper, token cru e substring; o reconhecimento reverso ainda não existe no ramo que seria retirado pelo novo mutante. (`bin/tests/test_censo_antes_da_vaga.sh:714–721`, `bin/disjuntor.py:1684`)

**3. ACHADOS 2, 3, 4 E 6**

| Achado do segundo laudo | Mesmo predicado ou outro? |
|---|---|
| **2 — `O67b`, `O51b`, caixa e dígito inicial** | **Outro: leitura/validação da obra.** As formas entram na matriz quando fornecidas ao predicado; serem barradas antes é decisão do extrator ou do shell. (`bin/disjuntor.py:157`, `bin/disjuntor.py:619–621`, `bin/codex_pergunta.sh:188`) |
| **2 — `FECHADA:` sem marca reconhecida** | **Outro: estado/seleção da fila.** `_FECHADO` exige negrito ou tachado; `_item_fechado` e `_itens_andaveis` aplicam essa decisão. (`bin/hook_stop_fila1.py:59`, `bin/hook_stop_fila1.py:168`, `bin/hook_stop_fila1.py:250–252`) |
| **3 — célula `O9 (C)` exata nunca abre** | **Outro: forma aceita pelo censo.** A pertença exata dá `True`; depois `_censo_nega` recusa a forma. No CLI, a recusa ocorre antes. **O espelho `O9 (C)`×`O9`, separadamente, é pertença e entra nesta volta.** (`bin/disjuntor.py:1687–1688`, `bin/disjuntor.py:1704–1707`, `bin/disjuntor.py:2246–2248`) |
| **4 — PARADA excluída** | **Outro: universo entregue à pertença.** `_fila1_viva` chama `_itens_andaveis`, que elimina `_item_parado`; a comparação token/obra recebe a lista já filtrada. (`bin/disjuntor.py:1637`, `bin/hook_stop_fila1.py:180`, `bin/hook_stop_fila1.py:250–252`) |
| **6 — texto CORTES/PAREI** | **Outro: contrato textual.** Literal vigente em `bin/disjuntor.py:1511`. A referência `CORTES.md:428` do anexo é [nao verificado]; a cópia termina em `app/docs/CORTES.md:425`. |
| **6 — PAREI permanece após fechar censo** | **Outro: ciclo de vida da faixa.** O escritor insere; o caminho de aprovação registra APROVADO e retorna, sem retirar a linha. (`app/core/esteira_vigia.py:883–890`, `bin/disjuntor.py:1717–1722`, `bin/disjuntor.py:2280–2281`) |
| **6 — papel leitura como prova** | **Outro: prova de processo/papel.** `leitura` integra `_PAPEL_CENSO`; a prova aceita o processo correspondente. Handoff publica o resultado desse reconhecedor. (`bin/hook_stop_fila1.py:726`, `bin/hook_stop_fila1.py:763–767`, `bin/handoff_sessao.sh:129–134`) |
| **6 — `_cru_abre_agent_copia`** | **Outro: classificação da abertura.** O cru truncado reconhece `Agent` ou `wt-`; o caminho normal exige agente não leitura e cópia reconhecida. (`bin/disjuntor.py:83–84`, `bin/disjuntor.py:1740–1741`) |
| **6 — `COPIA_RE` e `~/wt-*`** | **Outro: reconhecimento do caminho.** O regex exige a forma com barras `/…/wt-`; ele participa da classificação anterior ao censo. (`bin/disjuntor.py:153`, `bin/disjuntor.py:1844`) |

A inexistência de célula qualificada viva “hoje” e as quantidades de produção permanecem restritas ao dado raspado; confirmação independente: [nao verificado].

**4. ESCRITORES E LEITORES**

| Símbolo | Quem escreve/define | Quem lê/chama diretamente |
|---|---|---|
| `_pertence_fila1` | `bin/disjuntor.py:1669` | `_censo_nega`: `bin/disjuntor.py:1702`; `main_censo`: `bin/disjuntor.py:2265`. |
| `_token_da_celula` | `bin/disjuntor.py:1651` | Lista `exatas` da pertença: `bin/disjuntor.py:1683`. |
| `_casa_id_inteiro` | `bin/hook_stop_fila1.py:110` | Pertença: `bin/disjuntor.py:1684`; `_relato_parou`, IDs parados: `bin/hook_stop_fila1.py:1301`; item em curso: `bin/hook_stop_fila1.py:1313`. |
| `_fila1_viva` | `bin/disjuntor.py:1626` | `_censo_nega`: `bin/disjuntor.py:1702`; `main_censo`: `bin/disjuntor.py:2264`. |

Os leitores do Stop em `:1301` e `:1313` precisam permanecer no alcance da revisão de `_casa_id_inteiro`: decidem se o PAREI nomeia item parado ou o item em curso. O selo de fronteira os exercita por `_relato_parou`. (`bin/hook_stop_fila1.py:1298–1313`, `bin/tests/test_hook_parei_casa_id_inteiro.sh:53`)

O selo de censo lê indiretamente pelas portas `_censo`/`_pre` e escreve **cópias mutantes** dos corpos: pertença em `:714–717`, token em `:718–720`, fronteira em `:721`, leitura da fila em `:658–663` e `:679–680`. (`bin/tests/test_censo_antes_da_vaga.sh:71–74`, `bin/tests/test_censo_antes_da_vaga.sh:208`, `bin/tests/test_censo_antes_da_vaga.sh:658–663`, `bin/tests/test_censo_antes_da_vaga.sh:679–680`, `bin/tests/test_censo_antes_da_vaga.sh:714–721`)

## 09/11 07:46 · sessao a9dd49a63ff7 · - · - · origem=Ronald (tmux)

Interfaces reais: `validar_pergunta(pk, autor)` em `app/chamados/services/validacao.py:373`; resposta pela porta `responder_pergunta_disputa_core` em `app/chamados/services/disputa_emissao.py:1422`, com linha append-only em `:1508`; overnight por `_ts_tipo_do_plantio` em `app/chamados/services/materializacao.py:330`, ajuste em `:421`. Modelo de vínculo noturno já usado nos testes em `app/chamados/tests/test_plantio_intervalo_overnight.py:19`. Fechamento da disputa passa por `pode_fechar()` em `app/chamados/models.py:1856` e pela cauda da materialização em `app/chamados/services/materializacao.py:1302`.

Segue teste proposto, sem execução; campos cadastrais, escala e identidade dos usuários são sintéticos, pois não constam no dado raspado. O primeiro aceite permanece como decisão e aguarda a outra metade; essa política é a interpretação explícita usada pelo RED. [não verificado] [nao verificado]

```python
import datetime as dt
from zoneinfo import ZoneInfo

from django.contrib.auth.models import User
from django.test import TestCase, override_settings
from freezegun import freeze_time

from chamados.models import (
    ChamadoColaborador, DecisaoPergunta,
    DisputaSupervisao, PerguntaDisputa,
)
from chamados.services.disputa_emissao import responder_pergunta_disputa_core
from chamados.services.materializacao import _ts_tipo_do_plantio
from chamados.services.validacao import validar_pergunta
from colaboradores.models import Colaborador, Empresa, Posto, Praca
from escala.models import EscalaColaborador, TipoEscala
from ponto.models import Batida


DIA = dt.date(2026, 10, 21)
TZ = ZoneInfo("America/Sao_Paulo")


def ts(dia, hora, minuto=0):
    return dt.datetime.combine(dia, dt.time(hora, minuto), tzinfo=TZ)


@override_settings(TIME_ZONE="America/Sao_Paulo", USE_TZ=True)
@freeze_time("2026-11-09 10:45:00+00:00")
class BugParIntervaloCol960(TestCase):
    def setUp(self):
        User.objects.get_or_create(username="sistema")
        self.autor = User.objects.create_user(
            username="admin_fixture_960", is_staff=True, is_superuser=True
        )
        self.usuario = User.objects.create_user(username="col_fixture_960")
        emp = Empresa.objects.create(
            razao_social="FIXTURE", cnpj="00000000000000",
            dia_inicio_competencia=21,
        )
        pr = Praca.objects.create(
            nome="FIXTURE", cidade="Londrina", estado="PR"
        )
        posto = Posto.objects.create(
            nome="FIXTURE", empresa=emp, praca=pr
        )
        self.col = Colaborador.objects.create(
            pk=960, nome="col960", cpf="[cpf]",
            empresa=emp, posto=posto, situacao="ativo",
            usuario=self.usuario,
        )
        te = TipoEscala.objects.create(
            codigo="FIXTURE_960_NOTURNO", descricao="FIXTURE",
            tipo_ciclo=TipoEscala.CICLO_12x36,
            hora_inicio=dt.time(19), hora_fim=dt.time(7),
            hora_inicio_intervalo=dt.time(1),
            hora_fim_intervalo=dt.time(2),
            minutos_jornada=720,
        )
        EscalaColaborador.objects.create(
            colaborador=self.col, posto=posto, tipo_escala=te,
            data_inicio=DIA - dt.timedelta(days=30),
            data_ancora_colaborador=DIA, ativa=True,
        )
        self.ch = ChamadoColaborador.objects.create(
            pk=35664, colaborador=self.col, titulo="FIXTURE",
            assunto="FIXTURE", area="supervisao",
            categoria="Ausencia", subcategoria="Batida nao realizada",
            modulo_origem="batida_ausente", status_local="aberto",
            contexto_json={
                "data_turno": DIA.isoformat(),
                "marcos_faltantes": [
                    {"motivo": "intervalo_saida", "tipo": "S", "hora": "01:00"},
                    {"motivo": "intervalo_volta", "tipo": "E", "hora": "02:00"},
                ],
            },
        )
        self.d = DisputaSupervisao.objects.create(
            pk=5644, colaborador=self.col, chamado=self.ch,
            motivos=["intervalo_saida", "intervalo_volta"],
            contexto={
                "intervalo_saida": {"data": DIA.isoformat()},
                "intervalo_volta": {"data": DIA.isoformat()},
            },
        )
        self.saida = PerguntaDisputa.objects.create(
            disputa=self.d, chamado_destino=self.ch,
            motivo="intervalo_saida", data_evento=DIA,
            tipo_resposta="hora", pergunta="Que horas saiu?",
        )
        self.volta = PerguntaDisputa.objects.create(
            disputa=self.d, chamado_destino=self.ch,
            motivo="intervalo_volta", data_evento=DIA,
            tipo_resposta="hora", pergunta="Que horas voltou?",
        )
        Batida.objects.bulk_create([
            Batida(
                pk=133756, colaborador=self.col,
                timestamp=ts(DIA, 18, 32), tipo="E",
                origem="app", aprovada=True,
            ),
            Batida(
                pk=133794, colaborador=self.col,
                timestamp=ts(DIA + dt.timedelta(days=1), 6, 30),
                tipo="S", origem="app", aprovada=True,
            ),
        ])
        self.chao = list(
            Batida.objects.filter(pk__in=[133756, 133794])
            .order_by("pk")
            .values_list("pk", "timestamp", "tipo", "origem", "retratada_em")
        )

    def responder(self, pergunta, texto):
        ok, erro = responder_pergunta_disputa_core(
            pergunta, texto, self.usuario, ja_respondida=False
        )
        self.assertTrue(ok, erro)
        pergunta.refresh_from_db()

    def novas(self):
        return Batida.objects.filter(colaborador=self.col).exclude(
            pk__in=[133756, 133794]
        )

    def assert_aguarda(self, retorno):
        self.assertIn(
            "falta a outra metade do intervalo",
            retorno["msg"].lower(),
        )
        self.assertNotIn("colada", str(retorno).lower())
        self.assertFalse(self.novas().exists())
        self.d.refresh_from_db()
        self.assertIsNone(self.d.fechada_em)
        for pergunta in (self.saida, self.volta):
            pergunta.refresh_from_db()
            self.assertIsNone(pergunta.materializada_em)

    def test_RED_validar_saida_depois_volta_aplica_par(self):
        self.responder(self.saida, "01:00")
        self.responder(self.volta, "02:00")

        # Confirma a geometria da fixture pela derivação real.
        for pergunta, hora, tipo in (
            (self.saida, 1, "S"), (self.volta, 2, "E")
        ):
            erros = []
            self.assertEqual(
                _ts_tipo_do_plantio(self.d, pergunta, erros),
                (ts(DIA + dt.timedelta(days=1), hora), tipo),
                erros,
            )

        anteriores = list(
            DecisaoPergunta.objects.filter(
                pergunta__in=[self.saida, self.volta]
            ).order_by("pk").values()
        )

        r1 = validar_pergunta(self.saida.pk, self.autor)
        self.assert_aguarda(r1)

        r2 = validar_pergunta(self.volta.pk, self.autor)
        self.assertTrue(r2["ok"], r2)
        self.assertEqual(
            list(self.novas().order_by("timestamp").values_list(
                "tipo", "timestamp", "pergunta_origem_id"
            )),
            [
                ("S", ts(DIA + dt.timedelta(days=1), 1), self.saida.pk),
                ("E", ts(DIA + dt.timedelta(days=1), 2), self.volta.pk),
            ],
        )
        for pergunta in (self.saida, self.volta):
            pergunta.refresh_from_db()
            self.assertIsNotNone(pergunta.validada_em)
            self.assertIsNotNone(pergunta.materializada_em)

        self.d.refresh_from_db()
        self.assertIsNotNone(self.d.fechada_em)
        self.assertEqual(
            list(Batida.objects.filter(pk__in=[133756, 133794])
                 .order_by("pk")
                 .values_list(
                     "pk", "timestamp", "tipo", "origem", "retratada_em"
                 )),
            self.chao,
        )
        self.assertEqual(
            list(DecisaoPergunta.objects.filter(
                pk__in=[linha["id"] for linha in anteriores]
            ).order_by("pk").values()),
            anteriores,
        )

    def test_RED_so_saida_respondida_e_validada_aguarda(self):
        self.responder(self.saida, "01:00")
        r = validar_pergunta(self.saida.pk, self.autor)
        self.assert_aguarda(r)
        self.volta.refresh_from_db()
        self.assertIsNone(self.volta.respondida_em)
        self.assertEqual(self.volta.resposta_colab, "")
```

O RED usa funções existentes; a mensagem esperada e a unidade de aplicação do par ainda são comportamento proposto. A cópia atual chama `_materializar_uma` imediatamente após o carimbo de uma pergunta em `app/chamados/services/validacao.py:461`, devolvendo a validação após recusa em `:464`; portanto, o ponto esperado de falha por leitura é `assert_aguarda`, mas a execução e o vermelho efetivo permanecem [não verificado].

Pontos adicionais para a consolidação:

- Duas respostas presentes com apenas a saída validada já são cobertas pelo primeiro teste; uma resposta presente com a outra muda é o segundo teste. [não verificado: teste proposto] [nao verificado]
- O snapshot compara todas as colunas das decisões antigas, cobrando que nenhuma linha prévia tenha sido reescrita; o mecanismo de imutabilidade possui teste existente em `app/chamados/tests/test_e1_entradas.py:78`.
- `TestCase` mantém transação externa; por isso, esse RED comprova o resultado funcional e preservação do chão, mas não demonstra por si só a atomicidade em produção nem concorrência. [não verificado]
- Para o requisito de atomicidade, acrescentar teste de falha na segunda chamada à porta `registrar_batida`, exigindo rollback das duas batidas e dos carimbos, depois que a função concreta de aplicação do par existir. A porta real é `app/ponto/registro_batida.py:94`. [não verificado: teste adicional proposto]

## 09/11 07:46 · sessao 6e84b50fe4db · - · - · origem=Ronald (tmux)

**SEM JUIZ DO PAR DE PERGUNTAS.** O registro tem juiz do vão entre **batidas**, `intervalos_do_turno`, e juízes de turno/marcos; nenhum deles responde “estas duas perguntas são saída e volta da mesma pausa”. `app/core/juizes.py:195`, `app/core/juizes.py:202`, `app/ponto/turnos.py:502`.

O vínculo atual é estrutural: `PerguntaDisputa.disputa`, `motivo`, `data_evento`, `instancia_marco`; saída e volta recebem posições diferentes, `int_ida`/`int_volta`, sem identificador compartilhado da pausa. `app/chamados/models.py:2424`, `app/chamados/models.py:2431`, `app/chamados/models.py:2442`, `app/chamados/services/declaracao_texto.py:42`.

O código explicita que **duas pausas do mesmo dia hoje caem nos mesmos motivos, nenhum emissor carrega índice da pausa e separar pausas está reservado à E2**. Portanto mesma disputa+dia+motivos identifica candidato, mas não prova geral de mesma pausa; preencher arbitrariamente `instancia_marco` diferente da derivada também é recusado. `app/chamados/services/declaracao_texto.py:48`, `app/chamados/models.py:3436`.

O emissor mais próximo lê `marcos_faltantes` de um chamado `intervalo_ausente`, converte cada marco S/E nos motivos saída/volta e emite uma pergunta por marco, sem constituir unidade operacional do par. `app/chamados/services/disputa_emissao.py:1132`.

**Aplicação hoje:** `views.py` e fio chamam a casca `acao_validar_pergunta`, que chama `validar_pergunta`. Esta função valida **UMA** pergunta; abre `atomic`, toma travas, carimba a pergunta e executa `_materializar_uma` para ela. Depois do `atomic`, executa a varredura da disputa, que também aplica uma pergunta por vez. `app/chamados/views.py:483`, `app/chamados/services/fio.py:585`, `app/chamados/services/acoes_disputa.py:13`, `app/chamados/services/validacao.py:373`, `app/chamados/services/validacao.py:425`, `app/chamados/services/validacao.py:461`, `app/chamados/services/validacao.py:469`, `app/chamados/services/materializacao.py:1369`.

O ato atual grava `validada_em/por` e uma linha `DecisaoPergunta.VALIDACAO` na mesma transação; recusa posterior pode desfazer a projeção e acrescentar `DEVOLUCAO`, preservando a validação anterior. `app/chamados/services/validacao.py:127`, `app/chamados/services/validacao.py:195`.

O materializador abre transação por pergunta, chama o ramo de hora, que invoca a porta única `registrar_batida` com `autoridade_humana=False` e `fato_de_chao=False`; a porta cria a batida, e então o materializador exige lastro e carimbo aceito pelo C6. Falha provoca rollback daquela pergunta. Não existe transação envolvendo as duas metades como unidade. `app/chamados/services/materializacao.py:1220`, `app/chamados/services/materializacao.py:926`, `app/ponto/registro_batida.py:152`, `app/chamados/services/materializacao.py:1232`, `app/chamados/services/materializacao.py:1277`.

| Campo/função | Escreve | Lê |
|---|---|---|
| `resposta_colab`, `respondida_em` | `responder_pergunta_disputa_core`, `app/chamados/services/disputa_emissao.py:1460` | validação `app/chamados/services/validacao.py:393`; materialização `app/chamados/services/materializacao.py:829`; tela `app/templates/chamados/partials/modal_fio.html:218` |
| `validada_em/por` | `_carimbar_validacao`, `app/chamados/services/validacao.py:127`; devolução `app/chamados/services/validacao.py:195` | seletor granular `app/chamados/services/materializacao.py:1369`; tela `app/templates/chamados/partials/modal_fio.html:227` |
| `materializada_em/por`, `via_resolucao` | `PerguntaDisputa.carimbar`, `app/chamados/models.py:3103` | seletor `app/chamados/services/materializacao.py:1372`; C6 consultado no próprio carimbo `app/chamados/models.py:3139` |
| `recusa_motivo` | `_carimbar_recusa`, `app/chamados/services/materializacao.py:1083` | tela `app/templates/chamados/partials/modal_fio.html:225`; tradução da casca `app/chamados/services/acoes_disputa.py:32` |
| `Batida.timestamp/tipo/origem` | `registrar_batida`, `app/ponto/registro_batida.py:152` | lastro após escrita `app/chamados/services/materializacao.py:1233` |
| `Batida.pergunta_origem` | `anotar` chamado pelo materializador, `app/chamados/services/materializacao.py:946` | juiz do lastro explicado no mesmo trecho, `app/chamados/services/materializacao.py:941` |
| `DecisaoPergunta` | porta `registrar`, `app/chamados/models.py:3942` | histórico `respostas_anteriores`, `app/chamados/models.py:3500` |
| fechamento disputa | `DisputaSupervisao.fechar`, `app/chamados/models.py:1863` | `pode_fechar`, `app/chamados/models.py:1856`; cauda materializadora `app/chamados/services/materializacao.py:1302` |

**Desenho proposto, condicionado à identificação legítima do par:** ampliar a unidade materializadora para receber as duas perguntas provadas como par; tomar o conjunto de travas antes de qualquer escrita, reler ambas; guardar a validação de uma metade sem plantar enquanto falta anuência/validação da outra e devolver mensagem específica; quando ambas estiverem prontas, uma única transação escreve as duas pela mesma porta, exige alternância e lastro **depois do conjunto**, carimba ambas e só então reconcilia destinos e fechamento. Os pontos concretos a alterar são os anteriores sítios singulares de validação/aplicação, seleção e tradução; este comportamento ainda não existe. `app/chamados/services/validacao.py:425`, `app/chamados/services/validacao.py:461`, `app/chamados/services/materializacao.py:872`, `app/chamados/services/materializacao.py:1220`, `app/chamados/services/materializacao.py:1369`, `app/chamados/services/acoes_disputa.py:32` [desenho não implementado].

Cuidados concretos do desenho:

- Não manter a execução de `_materializar_uma` isolada para a primeira metade nem reconciliar o chamado entre as duas: o ramo atual reconcilia imediatamente após cada plantio, e os efeitos do fechamento podem atingir perguntas irmãs. `app/chamados/services/materializacao.py:974`, `app/chamados/services/disputa_emissao.py:1779`.
- Preservar ordem global **Colaborador → Disputa → Chamado → Pergunta**, incluindo as duas perguntas e todos os destinos/clones; o helper exige transação e orienta reler as instâncias após a trava. `app/chamados/models.py:2637`.
- Conservar recusa/espera como estado lido pela tela, com escritor canônico; `nao_gravou` hoje supõe `erros[0]`, portanto novo retorno de espera precisa tratamento explícito para não indexar lista vazia. `app/chamados/services/acoes_disputa.py:32`.
- Não reutilizar `validada_em` de metade aguardando como justificativa para absolver ou fechar: a porta de fechamento exige ausência de perguntas vivas **e** de respostas sem veredito. `app/chamados/models.py:1856`.
- Preservar texto humano nas duas perguntas: ramo de hora recusa resposta vazia, e validação do administrador não substitui a declaração do colaborador. `app/chamados/services/materializacao.py:822`, `app/chamados/services/materializacao.py:829`.
- Preservar append-only E1: registrar atos novos pela porta; não editar/apagar decisões anteriores; correção precisa conservar a projeção legada sem linha antes de alterá-la. `app/chamados/models.py:3821`, `app/chamados/models.py:3903`, `app/chamados/models.py:3914`.
- Não mover a guarda institucional para uma recusa global da batida de chão: a própria porta distingue `fato_de_chao`, e o evento de fechamento roda após commit sem barrar o registro do fato. `app/ponto/registro_batida.py:123`, `app/ponto/registro_batida.py:170`.

A limitação estrutural deve ficar explícita na consolidação: chamar a combinação “par” apenas por mesma disputa/dia/motivos inventaria a autoridade ausente; o caso fornecido identifica a mesma pausa por declaração do aval, enquanto a população inteira exige reconhecer a lacuna de identidade já documentada na E2. `app/chamados/services/declaracao_texto.py:48` [dados do caso somente no aval fornecido, sem arquivo local confirmado].

## 09/11 07:45 · sessao ff8f02861ed6 · - · - · origem=Ronald (tmux)

A recusa que explica o impasse está em `app/chamados/services/materializacao.py:875`: `_vizinha_mesmo_tipo(...)` combinada com `not _plantio_preenche_furo_na_data(...)`; o erro é montado em `:878-883`, antes de chegar à porta de escrita `:926-934`. O helper procura a vizinha imediata anterior/posterior em raio de **12 horas**, pelo tipo gravado, e não filtra `retratada_em` nem aprovação (`app/chamados/services/materializacao.py:85-106`).

A leitura da tela usa os mesmos helpers e classifica `bloqueada_paridade` em `app/chamados/services/materializacao.py:452-456`; o modal apresenta o detalhe em `app/templates/chamados/partials/modal_fio.html:229-241`. A validação chama `_materializar_uma` antes de varrer as demais perguntas, e devolve a validação recusada (`app/chamados/services/validacao.py:461-469`).

O texto **“marcacao […] colada nesse horario” / “duas seguidas iguais”** nasce em `app/chamados/catalogo/recusa.py:26-30`. `traduzir_recusa` escolhe essa frase por substrings do erro, sem comparar horários (`app/chamados/catalogo/recusa.py:77-83`). A casca VALIDAR traduz em `app/chamados/services/acoes_disputa.py:32-41`; regularizar traduz em `:169-174`; o followup também usa a tradução em `app/chamados/services/materializacao.py:1091` e `:1107`. Portanto “colada” descreve hoje uma guarda de até 12h, não proximidade de minutos (`app/chamados/services/materializacao.py:85-106`; `app/chamados/catalogo/recusa.py:26-30`).

As tolerâncias efetivamente encontradas são distintas:

- Mesmo tipo na porta: ±**60 segundos**, inclusivos; retorna a existente, sem criar (`app/ponto/registro_batida.py:106-115`).
- Reuso da materialização: ±**20 minutos**, mesmo tipo; não recusa (`app/chamados/juizes.py:235-247`).
- Colisão de tipo oposto: ±**300 segundos** com escala e marcos, ±**60 segundos** sem eles (`app/ponto/registro_batida.py:21-32`, `:138-151`).
- Exceção por marco próprio: ±**5 minutos** (`app/ponto/registro_batida.py:35-49`, `:145`).

Não foi encontrada uma constante nomeada “colada” ligada à regra PARIDADE; sua janela efetiva é `janela_h=12` (`app/chamados/services/materializacao.py:85`). Usar a tolerância CCT de 5 minutos para essa frase seria uma alteração não sustentada pelo predicado atual (`app/core/models.py:59`; `app/core/regua_cct.py:74`; `app/chamados/services/materializacao.py:85-106`).

`registrar_batida` não contém a recusa de duas iguais distantes: mesma espécie próxima é dedup; sua recusa temporal própria é colisão **oposta**, além da competência fechada para registro administrativo (`app/ponto/registro_batida.py:106-151`). Neste caminho a materialização passa `autoridade_humana=False, fato_de_chao=False` (`app/chamados/services/materializacao.py:927-934`).

**Histórico Git indisponível.** `git rev-parse HEAD`, `git log -L 870,922:app/chamados/services/materializacao.py` e `git blame -L 870,922 -- app/chamados/services/materializacao.py` retornaram `fatal: not a git repository`; a cópia não apresenta `.git`. Commit, data e mensagem que introduziram a recusa: **[não verificado]**.

Há evidência documental de antiguidade: o código atribui a guarda a “fio-quebrado 01/08”, a via a 20/08 e a ponte da lâmpada a 18/10 (`app/chamados/services/materializacao.py:85-90`, `:872-874`, `:884-889`). Isso permite registrar o que os comentários dizem; não permite afirmar a autoria Git **[não verificado]**.

Os commits citados aparecem no relato com outro escopo: `225256c1` como fecho/trava do repasse; `e017181f` como tratamento `REPASSE_RECUSADO`; `e9bf5de2` como consumidores de recusas (`app/docs/RELATO.md:27`, `:17`, `:11`). O relato registra merge/deploy da obra em 09/11 03:3x (`app/docs/RELATO.md:3`); a identificação com o deploy 01:52 fornecido pelo usuário e o commit `89df8c52` permanece **[não verificado]**.

## 09/11 07:44 · sessao 48f844357e2d · - · - · origem=Ronald (tmux)

O furo real do Agent é o sentido ausente: `_pertence_fila1('O9 (C)', ['O9'])` e `_pertence_fila1('O9 NOTURNO-X', ['O9'])` devolvem `False`; `_censo_nega` retorna `None` imediatamente, sem censo. (`bin/disjuntor.py:1683-1686`, `bin/disjuntor.py:1702-1703`)

O wrapper real recusa ambos antes da pertinência: `main_censo` exige `[A-Za-z0-9-]+`; caudas com espaço não chegam ao ramo compartilhado. (`bin/disjuntor.py:162`, `bin/disjuntor.py:2246-2248`, `bin/disjuntor.py:2265`)

Matriz literal do predicado:

| Obra literal | `O9` | `O9 (C)` | `OBRA-Q (A)` + `OBRA-Q (B)` | Outro token citando a obra inteira | `O142` |
|---|---:|---:|---:|---:|---:|
| `O9` | T | N | F | N | F |
| `O9 (C)` | **F*** | T | F | N | F |
| `O9 NOTURNO-X` | **F*** | F | F | N | F |
| `OBRA-Q` | F | F | N | N | F |
| `OBRA-Q (A)` | F | F | N | N | F |
| `o9` | N | N | F | N | F |
| `9ABC` | F | F | F | N | F |
| `O67b` | F | F | F | N | F |

T corresponde ao ramo `bin/disjuntor.py:1687-1688`; F ao ramo `bin/disjuntor.py:1685-1686`; N à exceção `bin/disjuntor.py:1689-1690`. As células são interpretadas pelo token `bin/disjuntor.py:1660-1666`, e a nomeação ignora caixa e respeita fronteiras em `bin/hook_stop_fila1.py:117`.

`*`: pela relação inversa indicada no laudo, o token inteiro `O9` aparece na obra e deve impedir FORA; sem identidade exata, o resultado conservador é NAO SEI. A relação inversa hoje não é consultada. (`bin/disjuntor.py:1683-1690`)

A matriz por formas alinhadas acrescenta:

| Obra | Célula | Predicado hoje |
|---|---|---|
| `OBRA-Q (A)` literal | `OBRA-Q` | **F*** |
| `OBRA-Q` | `OBRA-Q (A)` | T |
| `o9` | `o9` | T |
| `9ABC` | `9ABC` | T |
| `9ABC` | `9ABC (C)` | N |
| `O67b` | `O67b` | T |
| `O67b` | `O67b (C)` | N |
| `O1` | `O142` | F |

Esses resultados decorrem do aceite integral pelo regex de censo, da validação restrita do extrator e da comparação exata de token. (`bin/disjuntor.py:157`, `bin/disjuntor.py:162`, `bin/disjuntor.py:619-627`, `bin/disjuntor.py:1661-1666`, `bin/disjuntor.py:1683-1690`)

No Agent, `OBRA-Q (A)` vira `OBRA-Q`, portanto o F cru acima não representa furo dessa via. `o9`, `9ABC` e `O67b` não passam pela leitura da obra e recebem negativa sem obra. (`bin/disjuntor.py:602-621`, `bin/disjuntor.py:624-627`, `bin/disjuntor.py:1740-1741`, `bin/disjuntor.py:1758`, `bin/disjuntor.py:1847`)

Um teste wrapper `--censo 'O9 (C)'` não mata o mutante da direção ausente: ele já é recusado pela validação anterior. A cobertura “nas duas vias” desse mutante é impossível pela interface atual de `main_censo`; testar diretamente `_censo_nega(..., via='wrapper')` provaria apenas a função com esse rótulo. (`bin/disjuntor.py:2246-2248`, `bin/disjuntor.py:2265`, `bin/disjuntor.py:1693`)

## 09/11 07:44 · sessao 54c206c89ddb · - · - · origem=Ronald (tmux)

| Símbolo | Quem escreve/define | Leitores diretos |
|---|---|---|
| `_pertence_fila1` | `bin/disjuntor.py:1669` | `_censo_nega`, `bin/disjuntor.py:1702`; `main_censo`, `bin/disjuntor.py:2265` |
| `_token_da_celula` | `bin/disjuntor.py:1651` | Lista `exatas` de `_pertence_fila1`, `bin/disjuntor.py:1683` |
| `_casa_id_inteiro` | `bin/hook_stop_fila1.py:110` | `bin/disjuntor.py:1684`; `_relato_parou`, sobre ids parados, `bin/hook_stop_fila1.py:1301`, e item em curso, `bin/hook_stop_fila1.py:1313` |
| `_fila1_viva` | `bin/disjuntor.py:1626` | `_censo_nega`, `bin/disjuntor.py:1702`; `main_censo`, `bin/disjuntor.py:2264` |

O selo lê indiretamente pelas portas `_censo`/`_pre`; o mutador altera uma cópia textual do ramo `exatas` em `bin/tests/test_censo_antes_da_vaga.sh:714`; os testes de pertinência ficam em `bin/tests/test_censo_antes_da_vaga.sh:529`. O selo antigo de fronteira chama `_relato_parou` em `bin/tests/test_hook_parei_casa_id_inteiro.sh:53`, cobrindo os leitores externos ao censo em `bin/hook_stop_fila1.py:1301` e `bin/hook_stop_fila1.py:1313`.

| Achado | Classificação e evidência |
|---|---|
| 2: O67b/O51b/caixa/dígito | As formas entram na matriz **quando chegam como argumento da pertinência**; a recusa anterior é outro predicado: wrapper limita `[A-Z0-9-]`, `bin/codex_pergunta.sh:188`; extrator Agent usa `_OBRA_ID`, `bin/disjuntor.py:157`, e valida caixa em `bin/disjuntor.py:619`. |
| 2: FECHADA literal | Outro predicado, seleção/estado da fila: `_FECHADO` exige negrito ou tachado, `bin/hook_stop_fila1.py:59`; `_item_fechado` aplica em `bin/hook_stop_fila1.py:168`; exclusão acontece em `bin/hook_stop_fila1.py:250`. |
| 3: O9 (C) exata nunca abre | Outro predicado, representação aceita pelo censo: pertinência exata retorna True em `bin/disjuntor.py:1687`, mas `_censo_nega` recusa fora da forma em `bin/disjuntor.py:1704`; `main_censo` recusa antes da pertinência em `bin/disjuntor.py:2246`. O espelho O9 (C)×O9 é, separadamente, pertinência e pertence à matriz, `bin/disjuntor.py:1683`. |
| 4: PARADA excluída | Outro predicado, universo da fila: `_fila1_viva` usa `_itens_andaveis`, `bin/disjuntor.py:1637`; este exclui `_item_parado`, `bin/hook_stop_fila1.py:250`, decidido em `bin/hook_stop_fila1.py:180`. |
| 6: CORTES/PAREI | Outro predicado. Literal vivo em `bin/disjuntor.py:1511`; inserção via `bin/disjuntor.py:324`, somente acrescenta em `app/core/esteira_vigia.py:883`; aprovação não retira em `bin/disjuntor.py:1717`. Referência `CORTES.md:428` do anexo: [nao verificado]; a cópia termina em `app/docs/CORTES.md:425`. |
| 6: leitura como prova | Outro predicado, processo/papel: `CODEX_PAPEL=leitura` é aceito em `bin/hook_stop_fila1.py:726`, usado na prova em `bin/hook_stop_fila1.py:763`. Handoff publica o resultado em `bin/handoff_sessao.sh:129`. |
| 6: `_cru_abre_agent_copia` | Outro predicado, classificação de abertura: JSON truncado aceita qualquer `Agent` ou `wt-`, `bin/disjuntor.py:83`; normal exige não leitura e `COPIA_RE`, `bin/disjuntor.py:1740`. |
| 6: COPIA_RE | Outro predicado, reconhecimento de caminho: exige caminho com `/.../wt-`, `bin/disjuntor.py:153`; usado para classificar abertura em `bin/disjuntor.py:1740`. |

Os literais duplicados mencionados no opus 1 já leem `DJ.PAREI_CENSO` nesta cópia, em `bin/hook_stop_fila1.py:434` e `bin/hook_stop_fila1.py:783`. A negativa possui parser único em `bin/disjuntor.py:1515`, lido pela medida em `bin/disjuntor.py:2096`.

## 09/11 07:44 · sessao bd302f54505a · - · - · origem=Ronald (tmux)

O selo existente cobre o sentido obra nua → célula qualificada, mas não o espelho: entradas fake são `O9 (C)` e `OBRA-Q (SILENCIADO-ATE-X)` em `bin/tests/test_censo_antes_da_vaga.sh:45` e `:46`; os casos de obra nua estão em `:544`, `:559`, `:564`. A célula `O9` nua e única é preparada em `:576` e `:580`; é o ponto adequado para acrescentar o espelho antes da restauração em `:583`.

Linhas propostas nesse ponto, para as duas formas efetivamente legíveis pelo Agent, com breve ruim já preparado em `bin/tests/test_censo_antes_da_vaga.sh:214`:

```bash
for o in 'O9 (C)' 'O9 NOTURNO-X'; do
  s="$(_pre "$B" "$(_pl_agent general-purpose "Construa a obra $o na copia $WT. Brief: logs/e3/brief_ruim.md")")"
  _nega_censo_de "pertenca-agent-espelho-$o" "$o" "$s" || ok=1
  _sem_fora "pertenca-agent-espelho-$o" "$o" || ok=1
done
```

Esses casos matam o mutante que remove o reconhecimento reverso token→obra, isto é, restaura somente o atual `nomeiam = [c ... H._casa_id_inteiro(obra, c)]` de `bin/disjuntor.py:1684`. Hoje o predicado devolve `False` em `:1686`, e `_censo_nega` libera em `:1703`; após reconhecimento conservador, a obra qualificada ainda pode ser recusada pelo ramo fora da forma em `:1704` a `:1707`.

Para o wrapper, o controle pronto é:

```bash
for o in 'O9 (C)' 'O9 NOTURNO-X'; do
  rc="$(_censo "$B" "$o")"
  [ "$rc" = 1 ] || {
    _falha "pertenca-wrapper-espelho-$o" "rc=$rc, esperado 1"
    ok=1
  }
done
```

Esse controle **não mata o mutante da pertinência**: `main_censo` recusa as duas entradas antes de consultar a fila ou chamar `_pertence_fila1`, em `bin/disjuntor.py:2246` a `:2248`. Não seria correto chamar esse verde de cobertura do espelho pela integração wrapper. Para morder a regressão no predicado compartilhado, a asserção direta deve exigir que `_pertence_fila1(o, ['O9'])` não devolva `False`, aceitando `ValueError` como NAO SEI; o consumidor wrapper da função está em `bin/disjuntor.py:2265`.

Os mutantes já existentes para pertinência são célula crua, exato que cala outra, dois candidatos, wrapper próprio, token cru e substring, em `bin/tests/test_censo_antes_da_vaga.sh:714` a `:721`; nenhum retira um reconhecimento reverso, porque esse reconhecimento não existe em `bin/disjuntor.py:1684`.

## 09/11 07:43 · sessao 22308627191e · SONDA-SEM-GRANT · censo · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 09/11 07:42 · sessao 693ce8822a20 · SELOS-CANON-I1 · censo · origem=Code

**A v5.q não enunciava os dois sentidos da pertença.** Explicitava o achado de igualdade com célula crua; “nas duas vias (agente e wrapper)” designava percursos de execução. Ainda assim, perguntava pela semântica de pertença, incluindo “id cru x id com sufixo”. Portanto, classifico o achado 1 como **(a), pelo tema solicitado**, sem afirmar que o espelho tivesse sido expressamente formulado. (`i1_preverif_v5.q:3–10`)

As referências aos anexos abaixo contam as linhas do texto colado, incluindo linhas vazias, começando pela primeira linha de cada anexo.

| achado | classe | trecho q | trecho r | linha da triagem |
|---|---|---|---|---|
| **1 — espelho: token da célula dentro da obra** | **(a)**: pertença e diferença entre ID cru/qualificado estavam perguntadas; o espelho passou despercebido. O código procura somente a obra dentro da célula em `nomeiam`. (`bin/disjuntor.py:1683–1689`) | “Os achados 1 e 4 do laudo estao curados em TODOS os sitios”; “nas duas vias (agente e wrapper)?”; “id cru x id com sufixo”. (`i1_preverif_v5.q:5,10`) | “**Achado 1, nos dois pontos alterados: curado.**”; “Pedir `O9` diante dessa última célula resulta em NAO SEI”. Examina obra `O9`/célula `O9 (C)`, enquanto o laudo 2 aponta obra `O9 (C)`/célula `O9`. (`i1_preverif_v5.r:5–6`; `i1_opus_final_2.md:2`) | **L2:** “CURADO nas duas vias [...] -- conferido no diff.” Foi acolhimento da conclusão de cura, **não descarte de um espelho identificado**. (`i1_triagem_v5.txt:2`) |
| **2 — minúsculas no wrapper** | **(b)**: incompatibilidade vista e descartada. (`i1_preverif_v5.r:34`; `i1_triagem_v5.txt:6`) | “Algum leitor supoe significado diferente”. (`i1_preverif_v5.q:9`) | “O wrapper shell aceita somente `[A-Z0-9-]+`; o CLI aceita também minúsculas.” (`i1_preverif_v5.r:34`) | **L6:** “wrapper [A-Z0-9-] x CLI minusculas [...] anteriores, fora do diff. FILA.” (`i1_triagem_v5.txt:6`) |
| **2 — O67b/O51b também rejeitados pelo Agent** | **(a), parcialmente visto**: a resposta identificou wrapper/CLI, mas não diagnosticou o impedimento nas duas vias para esses IDs. A validação do Agent exige a gramática sensível à caixa. (`i1_preverif_v5.r:34`; `bin/disjuntor.py:157,616–620`; `app/docs/BACKLOG.md:1325,1327`) | “nas duas vias (agente e wrapper)?” (`i1_preverif_v5.q:5`) | O trecho pertinente limita-se a “O wrapper shell aceita somente [...]”; não apresenta o diagnóstico específico do Agent para O67b/O51b. (`i1_preverif_v5.r:34`) | **L6** descarta wrapper/CLI; não registra descarte específico desse impacto no Agent. (`i1_triagem_v5.txt:6`) |
| **2 — FECHADA sem negrito** | **(e)**: reconhecimento das formas de fechamento não foi perguntado especificamente. O caso e a restrição do reconhecedor constam da cópia. (`i1_preverif_v5.q:1–16`; `app/docs/BACKLOG.md:768`; `bin/hook_stop_fila1.py:59,168`) | O pedido próximo é sobre funções “que o diff cria ou muda”; não nomeia `_FECHADO` nem `FECHADA:`. (`i1_preverif_v5.q:7–10`) | Não há diagnóstico desse caso; a resposta descreve a delegação a `_itens_andaveis`. (`i1_preverif_v5.r:19`) | Sem entrada específica. (`i1_triagem_v5.txt:1–8`) |
| **3 — célula nua com cauda nunca abre** | **(b), com ressalva**: a impossibilidade de aprovação foi vista; a triagem justificou o descarte pelo efeito na medida. (`i1_preverif_v5.r:53`; `i1_triagem_v5.txt:3`) | “negativa de obra fora da forma nunca fecha intervalo em _censo_24h”; “Quem e afetado hoje?” (`i1_preverif_v5.q:13–14`) | “não torna possível aprovar a mesma identidade fora da forma”; “isso não equivale a vaga aberta”. (`i1_preverif_v5.r:53`) | **L3:** “nao fecha intervalo de obra qualificada -> FILA (pre-existente, so medida [...]). Nao bloqueia.” Essa justificativa não trata expressamente a indisponibilidade da abertura. (`i1_triagem_v5.txt:3`; `bin/disjuntor.py:1704–1707`) |
| **4 — PARADA excluída e tratada como FORA** | **(a)**: o universo produzido por `_fila1_viva` estava perguntado; a resposta descreveu a coleta sem apontar essa consequência. (`i1_preverif_v5.q:7`; `i1_preverif_v5.r:19`) | “QUEM ESCREVE e QUEM LE [...] (_casa_id_inteiro, _fila1_viva, _token_da_celula”. (`i1_preverif_v5.q:7`) | “Lê BACKLOG, delega a `_itens_andaveis` e devolve **lista de células cruas**, preservando duplicidades”. (`i1_preverif_v5.r:19`) | Sem descarte correspondente. A exclusão ocorre em `_itens_andaveis`; a ausência conduz a FORA. (`i1_triagem_v5.txt:1–8`; `bin/hook_stop_fila1.py:250–252`; `bin/disjuntor.py:1637,1685–1686,2269–2272`) |
| **5 — descartes confirmados não bloqueantes** | **(b)**: o próprio achado confirma os descartes, sem apresentar nova omissão. (`i1_opus_final_2.md:6`) | “sao regressao desta volta ou estado anterior? Quem e afetado hoje?” (`i1_preverif_v5.q:14`) | “o defeito já existia”; “Quem é afetado é o consumidor da medida”. (`i1_preverif_v5.r:53`) | **L3–6:** motivos de anterioridade, efeito na medida e casos fora do diff; **L7:** lacunas encaminhadas ao opus. (`i1_triagem_v5.txt:3–7`) |
| **6 — papel leitura aceito como prova** | **(a)**: a semântica da prova estava perguntada; a resposta mapeou consumidores sem apontar essa aceitação. O conjunto admite `censo` e `leitura`. (`i1_preverif_v5.q:8–10`; `bin/hook_stop_fila1.py:726,763`) | “prova_parei_censo, _eh_faixa_censo”; “Algum leitor supoe significado diferente”. (`i1_preverif_v5.q:8–9`) | “Produz `None`, prova `''` ou motivo → Stop, alarme e handoff.” (`i1_preverif_v5.r:27`) | Sem entrada específica. (`i1_triagem_v5.txt:1–8`) |
| **6 — retirada do PAREI; `_cru_abre_agent_copia`; `~/wt-*`** | **(e)** para esses temas específicos: ciclo de retirada, guarda de importação e forma abreviada do caminho não foram formulados. (`i1_preverif_v5.q:1–16`) | O trecho próximo pergunta quem “escreve o literal do PAREI fora do ponto unico”; não pergunta quando retirá-lo. (`i1_preverif_v5.q:6`) | “A escrita material do PAREI continua passando por `no_topo`”; não diagnostica retirada nem os dois reconhecedores de cópia. (`i1_preverif_v5.r:30`; `bin/disjuntor.py:65–84,153`) | Sem entrada específica. O laudo os chama de “herdados opus 1”, portanto não sustentam **(d)** por si mesmos. (`i1_triagem_v5.txt:1–8`; `i1_opus_final_2.md:7`) |
| **6 — teor de `CORTES.md:428`** | **[não verificado]**: não há conteúdo suficiente para classificar a obrigação desse corte. A cópia termina na linha 425. (`app/docs/CORTES.md:425`) | “Leis dos sitios em anexo”, mas o conteúdo desse anexo não foi fornecido. (`i1_preverif_v5.q:2`) | Não apresenta o teor desse corte. (`i1_preverif_v5.r:1–70`) | Sem entrada específica. (`i1_triagem_v5.txt:1–8`) |

**(c) aplica-se à comprovação operacional que faltava**, como execução integral/RED e eventos reais de produção; não à possibilidade de examinar estaticamente o espelho. A resposta declarou essas limitações, enquanto o código permite identificar a comparação unilateral. (`i1_preverif_v5.r:46,54,68`; `bin/disjuntor.py:1684`) Atribuir **(d), modo comum**, aos casos restantes exigiria evidência adicional sobre quem os examinou: **[não verificado]**.

## 09/11 07:41 · sessao e9a3685d39c6 · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 09/11 07:40 · sessao 44256442ce4b · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 09/11 07:40 · sessao 9d8baeed0bd3 · - · - · origem=Ronald (tmux)

Auditoria estática concluída; execução dos selos nesta sessão e estado vivo dos privilégios: [nao verificado].

| Selo | O que o código cobra | Lacuna / efeito do contrato |
|---|---|---|
| `test_codex_sonda.sh` | Usa envelope e Codex falsos; injeta nomes/CPF e verifica raspagem; falha do envelope/lista vazia bloqueia Codex com **rc 4**; cobra chamada padrão a `sonda_leitura.sh`, ausência de `psql/manage.py/credencial/docker` no wrapper e sandbox `read-only`; confere trilha obra/papel/hash/rc. (`bin/tests/test_codex_sonda.sh:21`, `:28`, `:46`, `:57`, `:71`, `:79`, `:93`) | **Não executa o envelope real nem observa SQL**: injeta `CODEX_SONDA_ENVELOPE` nas chamadas. (`bin/tests/test_codex_sonda.sh:49`) Contrato rc5 do envelope conflita com rc externo se exigir propagação: o selo hoje exige rc4 do wrapper. (`bin/tests/test_codex_sonda.sh:73`) |
| `test_sondas_rollback.sh` | AST encontra `atomic`, exige último comando `raise _Rollback()`, `exec` dentro dele e referência a `_canario_existe` depois; mutante retira o `raise`; exige `sondas.sh` referir `sonda_frota.sh` e `sondas_rollback.py`, sem `docker run` próprio. (`bin/tests/test_sondas_rollback.sh:15`, `:23`, `:28`, `:31`, `:37`, `:49`, `:63`) | **É prova de forma AST, não execução/efeito no PostgreSQL**: lê e analisa o fonte. (`bin/tests/test_sondas_rollback.sh:42`) O envelope real faz `CREATE TEMP TABLE` antes de executar a sonda: contradiz contrato global “nenhum CREATE”. (`bin/sondas_rollback.py:49`) Tirar canário sem substituir desenho deixa teste vermelho por ausência da conferência; trocar roteamento de `sondas.sh` deixa vermelho por exigência de frota. (`bin/tests/test_sondas_rollback.sh:38`, `:69`) |
| `test_ensaio_fora_do_postgres_de_prod.sh` | Consulta `pg_database` de `saas_db`; cobra ausência de `sombra/juliani_ref_*`; busca `dropdb/createdb/pg_restore` em shell de `bin/` apontando para prod; cobra host literal `sombra_db`; pares sintéticos mordem os detectores. (`bin/tests/test_ensaio_fora_do_postgres_de_prod.sh:38`, `:58`, `:65`, `:79`, `:94`) | O detector de escrita só procura esses três utilitários; não fiscaliza GRANT, ALTER ROLE, CREATE TEMP ou Python da sonda. (`bin/tests/test_ensaio_fora_do_postgres_de_prod.sh:60`) A consulta de catálogo real impede executar esse selo sem banco, e ele cria fixtures temporárias. (`bin/tests/test_ensaio_fora_do_postgres_de_prod.sh:38`, `:99`) |
| `test_recursos_sourced.sh` | Cobra source imune aos argumentos, proíbe suite em `saas_core`, exige cpuset nas suites; para `sombra_db`, exige cpuset/restart/memória no nascimento/update e pertencimento a `TESTE_CONTAINERS`. (`bin/tests/test_recursos_sourced.sh:18`, `:31`, `:38`, `:88`) | Não pergunta por SQL nem role leitor; inspeciona recursos/forma dos scripts. (`bin/tests/test_recursos_sourced.sh:94`) |
| `test_cpuset_de_teste.sh` | Procura `--cpuset-cpus` à mão e confere containers vivos cujo comando contém `manage.py test`; pares sintéticos verificam detector. (`bin/tests/test_cpuset_de_teste.sh:26`, `:41`, `:55`) | A medição viva filtra **suite**, não sondas: não mede isolamento real do container de `sonda_leitura.sh`. (`bin/tests/test_cpuset_de_teste.sh:44`) |
| `test_codex_pergunta.sh` | Com Codex falso e repositórios temporários, cobra sandbox, quatro papéis leitores, guardas do construtor e mutantes; não é prova de PostgreSQL. (`bin/tests/test_codex_pergunta.sh:2`, `:24`, `:73`, `:86`) | Alterações no envelope não são exercitadas por esse cenário; ele substitui apenas o Codex e testa o wrapper. (`bin/tests/test_codex_pergunta.sh:75`, `:86`) |
| `prova_codex_pergunta_real.sh` | No modo leitor, pede criações na cópia/viva/tmp e compara hash/status/ausência de arquivos. (`bin/tests/prova_codex_pergunta_real.sh:174`, `:180`, `:184`) | Não é prova de role/SQL. O selo de sonda só o invoca com `CODEX_SELO_REAL=1`. (`bin/tests/test_codex_sonda.sh:107`) |

O próprio `sonda_leitura.sh` documenta que `test_sonda_leitura.sh` **ainda não pousou** e que a proteção restante era documental mais RED em log; esse log não foi consultado. (`bin/sonda_leitura.sh:16`; estado do RED: [nao verificado]).

Os selos de host são chamados pelo loop da régua; falha interrompe antes da suite. Isso não prova que tenham sido executados nem que estejam verdes nesta árvore. (`bin/regua.sh:138`; resultados: [nao verificado]). O pre-push documenta que esse loop não roda ali. (`bin/pre-push.sh:13`).

Pontos essenciais para a consolidação do selo novo:

- O canário de rollback já é DDL executado pelo envelope Python, independentemente do SQL enviado a `psql`; vem antes do `exec` da sonda. (`bin/sondas_rollback.py:50`)
- O contador de tentativas soma apenas `n_tup_ins + n_tup_upd + n_tup_del`; não conta comandos DDL/DCL nem exige que tentativas sejam zero para emitir `OK`. (`bin/sondas_rollback.py:35`, `:72`)
- `sondas.sh` aceita qualquer linha `SONDA_ROLLBACK|OK|*` para mover a sonda a “feitas”, inclusive contador de tentativas positivo. (`bin/sondas.sh:38`)
- A sonda Python é arbitrária e recebe execução direta; um docker falso que só registre `psql` não observa SQL emitido por Django/driver dentro desse processo. (`bin/sonda_leitura.sh:239`, `bin/sondas_rollback.py:53`)
- O próprio código reconhece que `SET default_transaction_read_only=off` desfaz o parâmetro de sessão; não basta provar default `on`. (`bin/sonda_leitura.sh:13`)

Proposta ainda não implementada [nao verificado]: selo do envelope real, em raiz descartável, com Docker falso que registre SQL por stdin/`-c`/`-f` e recuse operações desconhecidas; matriz prod/sombra, grants prontos/faltantes, role ausente, login divergente, erros antes/depois do Python, chamada direta e via wrapper. Deve verificar controle positivo de consulta, nenhum provisionamento, rc5 nomeando falta, trilha coerente e mutante que reinsere `_garantir_grants`. O contador precisa representar a observação feita; campo literal `sql_escrita=0` não acrescenta evidência [nao verificado].

Proposta adicional, não provada na cópia [nao verificado]: separar selo host do envelope de prova de capacidades efetivas em cluster descartável equivalente, incluindo CREATE em `public/juliani`, CREATE TEMP, DML, sequência, GRANT/ALTER, memberships/SET ROLE, propriedade, privilégios herdados de PUBLIC e funções SECURITY DEFINER. Um grep de verbos em chamadas `psql` não demonstra recusa a SQL originado no Python arbitrário, cujo ponto de execução está em `bin/sonda_leitura.sh:240`.

## 09/11 07:39 · sessao 55358bb0102a · - · - · origem=Ronald (tmux)

Para o **espelho específico**, recomendo **(e)**: a pergunta não escreve a condição “token da célula dentro da obra”. Ela contextualiza “pertenca a fila 1 por igualdade com celula crua” e pede cura “nas duas vias (agente e wrapper)”; essas duas vias são percursos de execução, não os dois sentidos da relação. (`i1_preverif_v5.q:3-6`, numeração do anexo colado)

A pergunta aborda o tema geral: “id cru x id com sufixo” e mordida de `casos_pertenca`. Isso permite discutir **(a)** numa classificação ampla do tema pertença, mas não sustenta afirmar que a pergunta exigiu explicitamente os dois sentidos. (`i1_preverif_v5.q:7-10,15`)

A resposta demonstra apenas o sentido inverso: “Pedir `O9` diante dessa última célula resulta em NAO SEI”. A triagem declara “CURADO nas duas vias”; não relata ter visto e descartado o espelho. Portanto **(b)** não cabe para esse defeito. (`i1_preverif_v5.r`, §1, segundo item; linha absoluta [não verificado]; `i1_triagem_v5.txt:2`)

## 09/11 07:39 · sessao a46b11001e13 · - · - · origem=Ronald (tmux)

| Achado | Classe sugerida | Trecho literal q | Trecho literal r | Triagem/motivo |
|---|---|---|---|---|
| 5 — descartes confirmados | **(b)** | “sao regressao desta volta ou estado anterior? Quem e afetado hoje?” (`i1_preverif_v5.q`, item 4) | “O defeito já existia”; “Quem é afetado é o consumidor da medida” (`i1_preverif_v5.r`, seção 4) | Linha 3: “FILA (pre-existente, so medida [...] Nao bloqueia”. Linhas 4–6: demais limitações anteriores. Linha 7: lacunas encaminhadas ao opus. |
| 6 — PAREI/faixa e papel leitura | **(a), com ressalva de escopo** | “prova_parei_censo, _eh_faixa_censo”; “Algum leitor supoe significado diferente” (`i1_preverif_v5.q`, item 2) | “Classifica a linha literal válida”; “Produz `None`, prova `''` ou motivo” (`i1_preverif_v5.r`, tabela seção 2) | Ausente da triagem. A pergunta trouxe as funções e sua semântica; a resposta mapeou leitores, mas não apontou esses resíduos. O ciclo de retirada do PAREI não foi expressamente pedido. |
| 6 — `_cru_abre_agent_copia` e `COPIA_RE`/`~/wt-*` | **(e)** para os temas específicos | Não menciona essas funções nem caminhos `~/wt-*`; pedido geral sobre achados 1/4 e funções alteradas (`i1_preverif_v5.q`, itens 1–2). | Não os discute (`i1_preverif_v5.r`, seções 1–5). | Ausentes da triagem. Não cabe afirmar “nenhum viu”: o próprio laudo os apresenta como “herdados opus 1” (`i1_opus_final_2.md`, achado 6). |
| 6 — `CORTES.md:428` | **Conteúdo não classificável: [não verificado]** | A pergunta menciona “Leis dos sitios em anexo”, mas esse anexo não foi fornecido (`i1_preverif_v5.q`, linha inicial de anexos). | Sem discussão desse corte (`i1_preverif_v5.r`). | Ausente; o teor da referência não está no dado fornecido. |

Evidência do código:

- Papel `leitura` conta como prova: `bin/hook_stop_fila1.py:726`; o reconhecimento usa o conjunto em `bin/hook_stop_fila1.py:763`.
- A faixa preserva linhas de censo: `bin/hook_stop_fila1.py:481`; o escritor adiciona/reconhece a linha sem apresentar remoção nesse percurso: `bin/disjuntor.py:301–326`. Uma linha provada libera: `bin/hook_stop_fila1.py:804–806`.
- `_cru_abre_agent_copia` decide pela presença textual de `wt-`: `bin/disjuntor.py:82–84`; `COPIA_RE` exige caminho com barras antes de `wt-`: `bin/disjuntor.py:153`, usado na classificação do Agent em `bin/disjuntor.py:1740` e `bin/disjuntor.py:1844`.
- A cópia de `app/docs/CORTES.md` termina na linha 425; o conteúdo citado como `:428` permanece **[não verificado]** (`app/docs/CORTES.md:425`).

## 09/11 07:39 · sessao c019cdd8aca8 · - · - · origem=Ronald (tmux)

Sugestão de classificação por subtema:

| Achado | Classe | Trecho literal q | Trecho literal r | Triagem / ressalva |
|---|---|---|---|---|
| 2 — wrapper/minúsculas | **b** | “Algum leitor supoe significado diferente” (`i1_preverif_v5.q:9`) | “O wrapper shell aceita somente `[A-Z0-9-]+`; o CLI aceita também minúsculas.” | `i1_triagem_v5.txt:6`: “wrapper [A-Z0-9-] x CLI minusculas [...] anteriores, fora do diff. FILA.” Código: `bin/codex_pergunta.sh:188`; `bin/disjuntor.py:162`. |
| 2 — impacto específico O67b/O51b e rejeição Agent | **a**, como extensão não diagnosticada da incompatibilidade já identificada | “nas duas vias (agente e wrapper)” (`i1_preverif_v5.q:5`) | A resposta menciona wrapper/CLI, sem nomear O67b/O51b ou diagnosticar a rejeição Agent. (`i1_preverif_v5.r`, linha exata [não verificado]) | Não há descarte específico do Agent. Código: `bin/disjuntor.py:616`; IDs: `app/docs/BACKLOG.md:1325`, `app/docs/BACKLOG.md:1327`. |
| 2 — FECHADA sem negrito | **e**, se “tema presente” exigir pergunta específica sobre fechamento | Não há pergunta específica sobre formas de FECHADA. (`i1_preverif_v5.q:1`, demais linhas [não verificado]) | Não há diagnóstico desse caso. [não verificado] | Sem descarte específico. Caso em `app/docs/BACKLOG.md:768`; regex em `bin/hook_stop_fila1.py:59`; aplicação em `bin/hook_stop_fila1.py:168`. |
| 3 — identidade fora da forma nunca aprova | **b**, com ressalva sobre o motivo da triagem | “negativa de obra fora da forma nunca fecha intervalo em _censo_24h” (`i1_preverif_v5.q:13`) | “a volta corrige a leitura da negativa, mas não torna possível aprovar a mesma identidade fora da forma” | `i1_triagem_v5.txt:3` descarta o residual como “pre-existente, so medida”. Essa linha não analisa expressamente a indisponibilidade de abertura; a recusa está em `bin/disjuntor.py:1704`. |
| 4 — PARADA excluída antes da pertença | **a** | “QUEM ESCREVE e QUEM LE cada funcao/constante [...] (_casa_id_inteiro, _fila1_viva, _token_da_celula” (`i1_preverif_v5.q:7`) | “Lê BACKLOG, delega a `_itens_andaveis` e devolve **lista de células cruas**, preservando duplicidades” | Sem descarte correspondente. A resposta descreve a coleta, mas não diagnostica PARADA→FORA: `bin/disjuntor.py:1637`; `bin/hook_stop_fila1.py:250`; `bin/disjuntor.py:2269`. |

A ausência atual de célula viva do achado 3 é informação do laudo fornecido, não conclusão obtida por acesso a produção (`i1_opus_final_2.md:4`).

_3056 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
