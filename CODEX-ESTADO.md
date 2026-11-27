# CODEX-ESTADO

> Ultima resposta processada: **27/11/2026 08:45** · publicado em **27/11/2026 08:50** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 27/11 08:45 · sessao c12b28a112d8 · CODEX-CONSTROI-MOTOR · leitura · origem=Code

## 1. Fonte de autoria do teste [nao verificado]

**Não existe na c12 um manifesto persistido que marque um caminho como `CRIADO` pelo Codex construtor. Existe o `<resposta>.inventario`, que persiste os caminhos tocados pela chamada.** O inventário emite `TOCADO`, `PROIBIDO` ou `FALHA`. `TOCADO` agrupa criação, edição e outras diferenças. A resposta é escolhida pelo chamador e fica sob `logs/`. O wrapper não exige `logs/<OBRA>/` e não grava o inventário na `wt`. `bin/codex_pergunta.sh:89–100,145–151,229–250,457–467,835–837`

- **Onde já se sabe que é criação.** `_INVENTARIO_PY` monta `arvore` com `git ls-tree` do `HASH`, compara o FS e o status Git com essa base e, no modo positivo, reconhece o teste do app autorizado, recusa se o caminho já está em `arvore` e exige que exista no FS final. A chamada exige inventário vazio na entrada e na revalidação. Assim, para aquela chamada em modo positivo, um `TOCADO` da classe de teste aceita permite derivar a criação. O escritor único dessa informação seria o próprio wrapper, na saída desse mesmo inventário final. O arquivo bruto é escrito antes da prova de vida da lease, que ainda pode invalidar sua atribuição à chamada. `bin/codex_pergunta.sh:164–166,196–250,555–570,791–797,835–853`
- **O que os recibos provam.** `CODEX_RESULTADO` e `.rc` dão o desfecho da chamada. `.r` é a resposta do Codex, e `.id` liga uma resposta bem-sucedida à obra, papel, hash de entrada, PID e SHA da resposta. O JSONL é a saída do executor e o registro da vaga guarda obra, `wt`, base, `HEAD`, `main` e desfecho. Nenhum desses recibos publica a lista de criações. `rc_sem_colher.py` procura o `.rc` em `logs/<OBRA>/`. O inventário e os recibos da chamada real desta obra ficam [nao verificado]. `bin/codex_pergunta.sh:306–325,353–359,587–605,765–775,809–827,856–890` · `bin/rc_sem_colher.py:50–82`

## 2. Fonte da aprovação pelo Opus final

**Não existe na c12 uma fonte executável que ateste `Opus final APROVOU este diff`. Existe um leitor executável do texto `APROVADO` no transcript do verificador final.** `_veredito_e_obra` lê a obra do prompt e o veredito da primeira linha de `SubagentHandback`. `_reprovacoes_finais` procura `agent-*.jsonl` com `agentType=verificador` e entrega ao B3 somente os `REPROVADO`. O leitor não vincula a aprovação a um hash do diff nem verifica que o modelo era Opus. O Stop e o PreToolUse acionam essa leitura para o B3. `bin/disjuntor.py:438–466,649–671,839–853,981–1001,2233–2235,2255–2264` · `bin/hook_stop_fila1.py:1305–1321` · `.claude/agents/verificador.md:2–7,18`

- **RELATO e merge.** `linhas_libera_b3` reconhece `DESENHO` ou `ERRO-BOBO` no topo e `decide_b3` usa a linha para liberar um B3 disparado. `commit_msg.sh`, quando há `MERGE_HEAD`, passa `OBRA: <id>` a `--merge-obra`. O juiz consulta `estado_b3` sem transcript e aceita `LIVRE`, inclusive quando a obra nunca disparou. `LIBERA-B3` e `B3-MERGE livre` são estados desse portão. `bin/disjuntor.py:729–757,1025–1043,1086–1094,1289–1305` · `bin/commit_msg.sh:23–38`
- **`opus.md` ou linha de aprovação no RELATO.** Leitor que use qualquer dos dois como aprovação do diff nos três sítios [nao verificado]. O parecer fornecido abre com `REPROVADO`. Aprovação final, hash aprovado e execução em produção [nao verificado]. `r.txt:1`

## 3. Seleção dos três sítios na VM

| Sítio | Seleção, árvore e ponto do filtro |
|---|---|
| **Régua** | O shell escolhe `${ONLY:-$LABELS}`. `LABELS` contém apps e `--only` fornece labels explícitas. A régua monta sempre `~/saas-hasner/app`, mesmo quando o script é chamado de uma `wt`, e chama `manage.py test` direto pelo `trava_teste.sh`. O ponto de despacho é `:165–169`. Com label de app ou pacote, a lista de casos se forma na descoberta do Django, na chamada `super().build_suite()` do runner de CI. A poda de quarentena vem depois da descoberta. Para impedir também a carga do módulo na VM, a exclusão precisa valer antes de o Django importar o candidato. O glob de selos de host é outro fluxo e o selo do RELATO abre o fonte por glob. `bin/regua.sh:13–23,46–51,138–145,165–169` · `app/config/settings/ci.py:40` · `app/core/runner_quarentena.py:72–96` · `bin/tests/test_relato_guarda_pedido_de_patch.sh:143–148` |
| **Vigia da esteira** | A lista do isolamento nasce em `_isolar`, na extração e deduplicação dos IDs de `FAIL`/`ERROR` de `green.out`. O reteste monta `d/orig/app`, fotografia da viva feita **antes** da cura, e usa Docker direto pela `_rodar_testes`. O ponto comum aos retestes é a lista `ids` imediatamente antes de montar o argv nessa função. Reversão e relógio alimentam o mesmo ponto com IDs de `logs/vigia_arvore.falhas` para `R/app`. A autocura usa uma cópia temporária da viva. `app/core/esteira_vigia.py:725–762,1301–1317,1352–1363,1429–1448` · `bin/molde_fatia/rodar.sh:9–14,43–51` |
| **Vizinhos da volta** | Lê `base..HEAD` Git da árvore recebida e enumera `test*.py` no FS de `app/`. `poe()` é o único ponto que acumula as labels de teste tocado presente, vizinho por AST ou texto, app de migration e selo global. `suite.sh --volta` chama o seletor e retém apps de `LABELS`. O seletor entrega candidatos antes de a rota ser conhecida. A exclusão **da execução na VM** cabe no ramo local de `suite.sh`, depois da decisão de rota e antes do `manage.py test`. Uma label de app ou pacote pode redescobrir o módulo mesmo se sua label pontual sumir. `bin/vizinhos_da_volta.py:25–51,92–132` · `bin/suite.sh:198–216,260–285,502–529,560–569` |

## 4. Como o mesmo teste chega à pista 2 [nao verificado]

- **`--dir <wt> --volta <base>`** passa por Vizinhos antes da rota. **`--dir <wt> --only <label>`** recebe a label do chamador e pula Vizinhos. A inteira **`--dir <wt> --motivo "pre-opus <OBRA>"`** usa os apps de `LABELS`. Nenhum desses pedidos executa `regua.sh` ou `esteira_vigia.py`. `suite.sh` lê a linha `LABELS=` da régua, usa o `HEAD` da cópia ou fotografa alterações de `app/` num commit, publica `refs/pista/<sha>` e envia `ssh pista pede <sha> --only ...`. O contrato do atendente remoto está documentado fora dos executores da VM. O job real desta obra fica [nao verificado]. `bin/suite.sh:195–216,417–452,502–507` · `app/docs/BACKLOG.md:2375`
- **O mesmo argv hoje pode cair na VM.** Indisponibilidade ou incompatibilidade antes do aceite, e recusa remota sem execução, levam a `ROTA VM`. Depois do aceite, falha de transporte vira erro de infraestrutura. A inteira de pouso e a chamada sem `--dir` seguem na VM. Filtrar a saída única de Vizinhos antes dessa bifurcação também tiraria o teste da pista. A inteira pré-Opus e o recorte explícito também precisam respeitar a restrição se chegarem ao executor local. `bin/suite.sh:319–342,351–381,444–449,468–479,502–529,560–569`

## 5. O que distingue antes e depois do merge

- **Régua.** A distinção atual é só a presença do arquivo no `app/` vivo. Se o teste está apenas na `wt`, a régua canônica não o vê. Se os bytes chegam ao disco vivo antes do commit de merge, o bind já os vê. Impressão digital e carimbo medem a árvore, sem julgar se a entrega entrou em `main`. `bin/regua.sh:14–15,25–45,165–169,185–203` · `CLAUDE.md:352–354`
- **Vigia.** `orig/app` fixa os bytes da viva na abertura daquela fatia. Um `orig` posterior pode conter o teste pousado. Os retestes de reversão e relógio usam a viva diretamente. A decisão do isolamento usa o `mtime` de `green.out` e pode reutilizar um resultado sem reler os IDs. Nenhum desses pontos consulta `main` para saber se aquela entrega pousou. `bin/molde_fatia/rodar.sh:9–14` · `app/core/esteira_vigia.py:725–743,1301–1317,1352–1363`
- **Vizinhos.** A entrada é `<árvore> <base>` e o delta termina no `HEAD` daquela árvore. A mesma `wt` pode fornecer o mesmo delta após o merge em outra árvore. Sem `--dir`, `suite.sh --volta` usa sua própria raiz. O desligamento automático exigido pelo aval depende de saber que a entrega foi integrada a `main`. A ref viva já é lida pelo wrapper na admissão, mas nenhum dos três seletores usa esse dado como prova de pouso. `bin/vizinhos_da_volta.py:25–30` · `bin/suite.sh:200–206` · `bin/codex_pergunta.sh:306–310,575–576` · `aval.txt:1`

## 6. REDs e impacto nos chamadores

Use o mesmo módulo da sonda, `app/core/tests/test_model_sem_migration_pendente.py`, e um teste existente como controle. O anexo relata `rc 0` e ausência na base para a criação da fixture. Os resultados abaixo são os esperados pela leitura. Execução dos três REDs e do controle na pista [nao verificado]. `matriz_sem_flag.txt:7–10` · `bin/codex_pergunta.sh:232–250` · `aval.txt:1`

| RED | Entrada e estado | Hoje | Esperado pelo aval |
|---|---|---|---|
| **Régua** | O `app/` montado pela régua de teste contém os dois módulos. O novo tem origem na chamada do construtor, está pendente e é descobrível por `core`. | A descoberta inclui os dois. | Na VM roda o existente. O novo roda na pista com a cópia que o contém. Uma fixture só na `wt` não exercita a régua canônica. `bin/regua.sh:14,23,165–169` · `app/core/runner_quarentena.py:72–96` · `bin/suite.sh:417–445` · `aval.txt:1` |
| **Vigia** | `green.out` nomeia os dois IDs, cache frio, `orig/app` da fixture com os módulos disponíveis. O novo está pendente. | `_isolar` entrega os dois IDs ao Docker. Na fotografia anterior à criação, o módulo novo estaria ausente, mas seu ID ainda seria passado e a tentativa influenciaria a classificação. | Retesta só o existente. Se todos os IDs forem excluídos, o isolamento fica sem reteste e sem veredito derivado dele. A pista pode rodar o novo na cópia que o contém. `app/core/esteira_vigia.py:125–144,725–762` · `bin/molde_fatia/rodar.sh:9–14` · `bin/suite.sh:417–445` · `aval.txt:1` |
| **Vizinhos** | Teste novo já commitado na `wt`, ausente de `<base>`, com um vizinho existente. Mesmo recorte `--dir … --volta …`, pendente, uma vez em `ROTA VM` e outra com pista aceita. | O novo sai como `obra` e chega aos dois executores. Antes do commit o diff não o nomeia, embora o glob ainda possa achá-lo como vizinho. | Na VM roda o existente, inclusive se uma label de pacote cobrir o novo. Na pista o mesmo novo continua na seleção. `bin/vizinhos_da_volta.py:25–45,92–95,116–132` · `bin/suite.sh:441–445,502–507,560–569` · `aval.txt:1` |

- **Pre-push e carimbo.** `bin/hooks.sh` instala wrappers para o `pre-push` da árvore chamadora e para o `commit-msg` da principal. `bin/pre-push.sh` testa o SHA empurrado em um `git archive`, lê `LABELS` da régua e pode pular o arsenal se o carimbo `OK` corresponder à árvore e ao commit. Um carimbo de corrida com exclusões pode, portanto, fazer o push pular testes que nunca rodaram. A variante em `app/core/hooks/pre-push.sh` lê a mesma linha, monta a viva e usa o carimbo por impressão digital. Qual hook está instalado [nao verificado]. `bin/hooks.sh:63–83` · `bin/pre-push.sh:27–45,50–74,142–145,184–199` · `app/core/hooks/pre-push.sh:18–25,54–55,80–88`
- **Outros leitores de `LABELS`.** `suite.sh`, vigia da árvore, réguas de relógio e calendário, molde de autocura e certificador da pista leem a declaração literal. Trocar seu formato quebra esses leitores e o selo da fonte única. Mudar a seleção dentro da régua deixa independentes as chamadas Docker do vigia da árvore e dos hooks. O certificador também exige que o argv corresponda a `LABELS` e que `Found − QUARENTENA-PODADO = Ran`. `bin/suite.sh:195–196` · `bin/vigia_arvore.sh:75–88,101–149` · `bin/regua_relogio.sh:27–29,51–55` · `bin/regua_calendario.sh:22–26` · `bin/molde_autocura/rodar.sh:22–26` · `bin/pista_certifica.sh:67–69,83–92` · `bin/tests/test_labels_fonte_unica.sh:20–35,80–95`
- **Volta intermediária e vigia.** `volta_intermediaria.sh` faz Ruff e o selo do disjuntor na VM, chama `suite.sh --dir … --volta …`, conta labels em `.porque` e agrega o rc. Se a seleção esvaziar antes da guarda, a volta já trata isso como vermelho. Se esvaziar depois e cair em `${ONLY:-$LABELS}`, reabre a suíte padrão. No vigia, reutilizar o cache por `mtime` após mudar aprovação ou pouso pode manter a classificação antiga. Passar uma lista vazia ao Docker ou tratá-la como reprovação também altera a classificação. `bin/volta_intermediaria.sh:18–46` · `bin/suite.sh:206–216,560–569` · `app/core/esteira_vigia.py:125–144,725–762`

## 27/11 08:41 · sessao 60054934ae3a · SONDA-ESPURIAS-TXT11 · leitura · origem=Code

1. **Como tirar a batida e reler o dia.** A porta da casa é `ponto.registro_batida.retratar_batida`. Ela preenche `retratada_em`, põe `aprovada=False` e agenda o recálculo para `on_commit`. `turnos.batidas_apuraveis` exclui a marca pelo `retratada_em` preenchido. Na sonda, B altera temporariamente só esse campo nos IDs da s1. Isso permite executar a mesma sequência explícita em A e B, sem disparar em B os julgamentos por batida ligados ao `save`. [app/ponto/registro_batida.py:255-301] [app/ponto/turnos.py:2941-2968] [app/ponto/signals.py:20-36,108-117,136-145] [app/ponto/services/fechamento.py:1822-1848]

   A sequência é `nucleo.recompute_turnos` → `cartorio.julgar_colab(apply_=True, forcar=True)` → `recalcular_fechamento_mes(somente_leitura=True, retornar_objeto=True, lavrar_dias_pagos=False)` → `classificar_export` → `montar_linhas_export` → `validar_layout_dominio` → `conteudo_do_txt`. O cartório relavra a ata e o veredito das células existentes. O fechamento usa as batidas e as células na apuração, e o export lê a célula para decidir retenção. A seleção da s1 é feita uma vez antes das passadas. O script acompanha o PK até a sobra emitida pela grade, preservando marcas distintas no mesmo minuto e marcas atribuídas a um turno que cruza a meia-noite. [app/ponto/nucleo.py:40-67] [app/ponto/services/cartorio.py:959-981,1187-1209,1905-1932] [app/ponto/services/fechamento.py:624-653,762-775,1037-1038,1152-1160] [app/folha/views.py:214-219,272-288] [app/folha/export.py:977-983,1256-1305] [app/escala/utils.py:623-681,1747-1782,1863-1902,2057-2094]

2. **O ensaio precisa de `READ WRITE`.** O núcleo escreve `TurnoMaterializado`, e o cartório escreve ata, veredito, trilha e outbox. O recálculo em memória e a montagem do TXT são os do molde. A aprovação em `FechamentoMensal` e o registro em `ExportacaoDominio` pertencem à view HTTP. A sonda usa `atomic(durable=True)` com erro sentinela no fim, e confere contagens e impressões das linhas antes e depois, inclusive nas duas tabelas da view. As sequências são reiniciadas dentro de cada savepoint para que a conferência também cubra seus valores. Execução e reversão na sombra [nao verificado]. [app/ponto/nucleo.py:57-66] [app/ponto/portas/celula.py:981-1002,1133-1140,1218-1239] [app/core/observ.py:124-138] [app/ponto/services/fechamento.py:1152-1168,1215-1225] [app/folha/views.py:253-263,292-307]

3. **Determinismo é medido.** A1, A2, B1 e B2 partem do mesmo estado, com o mesmo relógio, e são comparados byte a byte. Há ordenações sem desempate por PK no universo do fechamento e na leitura das batidas. A igualdade efetiva de cada lado é [nao verificado]. `BASE`, em stderr, compara A1 ao TXT produzido antes de relavrar. Se der `False`, o teto publicado mede o efeito das sobras entre as duas passadas relavradas, e a diferença do TXT corrente fica explícita. [app/ponto/services/fechamento.py:459-465,634-639] [app/ponto/turnos.py:2961-2968] [app/folha/export.py:525-539,725-755] [nao verificado]

`TXT_RUBRICAS_MUDADAS` conta pares `(colab_id, rubrica)` com diferença nos registros 10/11, como no molde. `RUBRICA` soma por código e mostra `B − A`. Horas saem dos centésimos do TXT convertidos em minutos. Faltas usam os minutos das datas filhas pelo mapa de faltas da casa. Uma troca de datas ou deltas que se anulam pode aumentar a contagem sem gerar uma linha `RUBRICA`. As até 20 linhas `COLAB` são um limite global. As medidas das empresas 2, 3 e 4 estão [nao verificado]. [app/folha/export.py:34-45,95-143,215-255,733-755]

### SCRIPT

```python
import logging
import sys
from collections import defaultdict
from contextlib import redirect_stderr, redirect_stdout
from datetime import date, datetime, time, timedelta
from decimal import Decimal
from io import StringIO
from traceback import extract_tb
from types import SimpleNamespace
from unittest.mock import patch

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone

from chamados import fcm_utils, push_utils [nao verificado]
from chamados.models import (
    ChamadoColaborador, DecisaoPergunta, DisputaSupervisao,
    FollowupChamado, PerguntaDisputa,
)
from colaboradores.models import Colaborador, Empresa
from core.models import LogAuditoria
from escala import utils as grade
from escala.models import CelulaDia, EventoCelula
from escala.orfa_classe import classificar_orfas
from folha import export
from folha.models import ExportacaoDominio
from ponto import turnos
from ponto.management.commands.detectar_ausencias import Command as Emissor
from ponto.models import (
    Batida, DiaPago, FechamentoMensal, Justificativa, TurnoMaterializado,
)
from ponto.nucleo import recompute_turnos
from ponto.services import cartorio, fechamento

MES, ANO = 11, 2026
EMPRESAS = (2, 3, 4)
JANELA = (date(2026, 10, 21), date(2026, 11, 20))
REF_S1 = "[app/escala/orfa_classe.py:53-78] [app/escala/utils.py:972-979,2138-2144]"
REF_TXT = "[app/folha/export.py:34-45,215-255,725-755]"
REF_FALTA = "[app/folha/export.py:95-143,215-221]"
REF_SOMBRA = "[app/config/settings/sombra.py:47-65] [bin/sombra.sh:78-80,490-491]"
MODELOS = (
    Batida, CelulaDia, TurnoMaterializado, FechamentoMensal, DiaPago,
    ExportacaoDominio, LogAuditoria, EventoCelula, ChamadoColaborador,
    Justificativa, DisputaSupervisao, PerguntaDisputa, DecisaoPergunta,
    FollowupChamado,
)


class ReverterSonda(Exception):
    pass


class FalhaSonda(RuntimeError):
    pass


class HoraComId(str):
    pass


class InstanteComId(datetime):
    def strftime(self, formato):
        hora = super().strftime(formato)
        if formato == "%H:%M" and hasattr(self, "batida_id"):
            hora = HoraComId(hora)
            hora.batida_id = self.batida_id
        return hora


def log_sem_dados(_logger, registro):
    if registro.levelno >= logging.ERROR:
        erro = registro.exc_info[1] if registro.exc_info else sys.exception()
        tipo = type(erro).__name__ if erro is not None else "ERROR"
        raise FalhaSonda(f"{registro.pathname}:{registro.lineno} {tipo}") from None


def alvos_da_s1(empresa, agora):
    ini, fim = export.janela_competencia(empresa, MES, ANO)
    if (ini, fim) != JANELA:
        raise FalhaSonda(
            f"Janela fora do aval empresa={empresa.pk} "
            "[app/folha/export.py:47-51] [app/ponto/janelas.py:54-57]"
        )
    primeiro = timezone.make_aware(datetime.combine(ini - timedelta(days=1), time.min))
    ultimo = timezone.make_aware(datetime.combine(fim + timedelta(days=3), time.min))
    ultimo -= timedelta(microseconds=1)
    ref = fim + timedelta(days=1)
    dias = (ref - ini).days + 1
    alvos = {}

    localtime_real = timezone.localtime
    pessoas = Colaborador.objects.filter(empresa=empresa).order_by("pk")
    with (
        patch.object(grade, "Exception", (), create=True),
        patch.object(turnos, "Exception", (), create=True),
    ):
        for colab in pessoas.iterator(chunk_size=200):
            batidas = list(turnos.batidas_apuraveis(colab, primeiro, ultimo))
            if not batidas:
                continue

            kwargs = dict(dias=dias, ref=ref, agora=agora)
            linhas = grade.montar_grade_prevista_periodo(colab, batidas, **kwargs)["dias"]
            if len(linhas) != dias:
                raise FalhaSonda(f"Grade fora da janela colab_id={colab.pk} {REF_S1}")
            classes = [classificar_orfas(linha) for linha in linhas[1:]]
            if not any(classes):
                continue

            # Só transporta a identidade. Os datetime e HH:MM mantêm seus valores.
            # [app/escala/utils.py:314-327,699-705,1367-1385,1766-1782,1882-1891]
            por_objeto = {id(b.timestamp): b.pk for b in batidas}

            def localtime_com_id(value=None, timezone=None):
                instante = localtime_real(value, timezone)
                pk = por_objeto.get(id(value))
                if pk is None or sys._getframe(1).f_globals is not vars(grade):
                    return instante
                marcado = InstanteComId(
                    instante.year, instante.month, instante.day,
                    instante.hour, instante.minute, instante.second,
                    instante.microsecond, tzinfo=instante.tzinfo, fold=instante.fold,
                )
                marcado.batida_id = pk
                return marcado

            with patch.object(timezone, "localtime", localtime_com_id):
                marcadas = grade.montar_grade_prevista_periodo(
                    colab, batidas, **kwargs,
                )["dias"]

            ids = set()
            for linha, marcada, classificacao in zip(
                linhas[1:], marcadas[1:], classes, strict=True,
            ):
                horas = marcada.get("orfas") or []
                if horas != (linha.get("orfas") or []):
                    raise FalhaSonda(f"Instrumentação mudou a grade colab_id={colab.pk} {REF_S1}")
                for hora in horas:
                    if hora in classificacao:
                        pk = getattr(hora, "batida_id", None)
                        if pk is None:
                            raise FalhaSonda(f"Sobra sem identidade colab_id={colab.pk} {REF_S1}")
                        ids.add(pk)
            alvos[colab.pk] = ids

    return alvos


def reler_celulas(empresa, alvo_colabs, agora):
    ini, fim = turnos.janela_com_pad(*JANELA)
    hoje = timezone.localdate(agora)
    emissor = Emissor(stdout=StringIO(), stderr=StringIO())
    pessoas = Colaborador.objects.filter(
        empresa=empresa, pk__in=alvo_colabs,
    ).order_by("pk")
    for colab in pessoas:
        recompute_turnos(colab, ini, fim)
        celulas = list(CelulaDia.objects.filter(
            colaborador=colab, data__range=(ini, fim), data__lt=hoje,
        ).order_by("data"))
        if celulas:
            cartorio.julgar_colab(
                colab, celulas, hoje=hoje, agora=agora, apply_=True,
                emissor=emissor, out=cartorio.contadores(), forcar=True,
            )


def medir_txt(empresa):
    with patch.object(fechamento, "Exception", fechamento._GradeSemCelula, create=True):
        fechs = fechamento.recalcular_fechamento_mes(
            MES, ANO, empresa_id=empresa.pk, somente_leitura=True,
            retornar_objeto=True, lavrar_dias_pagos=False,
        )
    objetos = list(fechs.values())
    itens = export.classificar_export(empresa, MES, ANO, fechamentos=objetos)

    montar = export.montar_linha_10
    campos = []

    def observar_linha_10(matricula, comp, rubrica, tipo, valor, cod_sistema):
        linha = montar(matricula, comp, rubrica, tipo, valor, cod_sistema)
        mat = str(matricula).zfill(10)
        cod = str(rubrica).zfill(4)
        inicio = len(f"10{mat}{comp}{cod}11")
        fim = -len(str(cod_sistema).zfill(10))
        campos.append((mat, cod, tipo, inicio, fim))
        return linha

    with patch.object(export, "montar_linha_10", observar_linha_10):
        linhas, entra = export.montar_linhas_export(
            empresa, MES, ANO, itens=itens, fechamentos=objetos,
        )
    erros = export.validar_layout_dominio(linhas)
    txt = export.conteudo_do_txt(linhas)

    por_matricula = defaultdict(list)
    for item in itens:
        cid = item["colaborador_id"]
        if cid in entra and fechs[cid].status in export.STATUS_NO_TXT:
            por_matricula[item["codigo_dominio"].zfill(10)].append(cid)

    grupos = defaultdict(list)
    pais = iter(campos)
    pai = None
    for linha in txt.splitlines():
        parte = export.partes_da_linha(linha)
        if linha.startswith("10"):
            mat, cod, tipo, inicio, fim = next(pais)
            if erros:
                cru = int(linha[inicio:fim])
            else:
                if parte is None or (parte["matricula"], parte["rubrica"]) != (mat, cod):
                    raise FalhaSonda(f"Registro 10 não confere {REF_TXT}")
                cru = parte["valor"]
            ids = por_matricula[mat]
            if len(ids) != 1 or not (cod.isascii() and cod.isdigit()):
                raise FalhaSonda(f"Autoria ou rubrica indeterminada {REF_TXT}")
            pai = (tipo, cru, [])
            grupos[ids[0], cod].append(pai)
        elif pai is not None and parte is not None and parte["reg"] == "11":
            pai[2].append(parte["data"])
        else:
            raise FalhaSonda(f"Registro filho inválido {REF_TXT}")
    if next(pais, None) is not None:
        raise FalhaSonda(f"Registro 10 sem linha no TXT {REF_TXT}")

    registros = {}
    minutos = {}
    faltas = {}
    for (cid, cod), grupo in grupos.items():
        chave = (cid, cod)
        registros[chave] = tuple(sorted(
            (cru, tuple(sorted(datas))) for _, cru, datas in grupo
        ))
        total = Decimal(0)
        for tipo, cru, datas in grupo:
            if tipo != "dias":
                total += Decimal(cru) * Decimal("0.6")
                continue
            if not datas:
                raise FalhaSonda(f"Rubrica em dias sem datas filhas colab_id={cid} {REF_FALTA}")
            if cid not in faltas:
                mapa = export.minutos_de_falta_por_dia(fechs[cid].colaborador, *JANELA)
                faltas[cid] = {d.strftime("%Y%m%d"): Decimal(str(m)) for d, m in mapa.items()}
            for dia in sorted(set(datas)):
                total += faltas[cid][dia]
        minutos[chave] = total

    return SimpleNamespace(txt=txt.encode("utf-8"), registros=registros,
                           minutos=minutos, erros_layout=len(erros))


def sequencias():
    # O banco da sombra é PostgreSQL 17. [bin/sombra_db.sh:45]
    quote = connection.ops.quote_name
    with connection.cursor() as cursor:
        cursor.execute(
            """
            SELECT n.nspname, c.relname, s.seqincrement, s.seqmin, s.seqmax, s.seqcycle
            FROM pg_catalog.pg_sequence s
            JOIN pg_catalog.pg_class c ON c.oid = s.seqrelid
            JOIN pg_catalog.pg_namespace n ON n.oid = c.relnamespace
            WHERE n.nspname IN (%s, %s)
            ORDER BY n.nspname, c.relname
            """,
            ("juliani", "public"),
        )
        saida = {}
        for schema, nome, passo, minimo, maximo, ciclo in cursor.fetchall():
            tabela = f"{quote(schema)}.{quote(nome)}"
            cursor.execute(f"SELECT last_value, is_called FROM {tabela}")
            valor, chamado = cursor.fetchone()
            saida[tabela] = (valor, chamado, passo, minimo, maximo, ciclo)
    return saida


def reiniciar_no_savepoint(estado):
    with connection.cursor() as cursor:
        for nome, (valor, chamado, passo, minimo, maximo, ciclo) in estado.items():
            proximo = valor + passo if chamado else valor
            if ciclo and proximo > maximo:
                proximo = minimo
            if ciclo and proximo < minimo:
                proximo = maximo
            cursor.execute(f"ALTER SEQUENCE {nome} RESTART WITH {int(proximo)}")


def estado_das_linhas():
    quote = connection.ops.quote_name
    schema = quote(connection.schema_name)
    estado = {}
    with connection.cursor() as cursor:
        for modelo in MODELOS:
            tabela = quote(modelo._meta.db_table)
            pk = quote(modelo._meta.pk.column)
            cursor.execute(
                f"""
                SELECT count(*),
                       md5(coalesce(string_agg(md5(to_jsonb(t)::text), '' ORDER BY t.{pk}), ''))
                FROM {schema}.{tabela} AS t
                """
            )
            estado[modelo.__name__] = cursor.fetchone()
    return estado


def rodada(empresa, colabs, pks, agora, estado_sequencias):
    with transaction.atomic():
        reiniciar_no_savepoint(estado_sequencias)
        if pks:
            marcas = turnos.batidas_apuraveis(None).filter(
                colaborador__empresa=empresa, pk__in=pks,
            )
            # Perturbação de sombra antes da relavratura comum.
            # [app/ponto/turnos.py:2941-2968] [app/ponto/signals.py:34-36,136-140,211-220]
            tiradas = marcas.update(retratada_em=agora)
            if tiradas != len(pks) or marcas.exists():
                raise FalhaSonda("Retirada incompleta [app/ponto/turnos.py:2960-2968]")
        reler_celulas(empresa, colabs, agora)
        foto = medir_txt(empresa)
        transaction.set_rollback(True)
    return foto


def linhas_da_medida(empresa_id, a, a2, b, tiradas):
    zero = Decimal(0)
    mudadas = set()
    total_a, total_b = defaultdict(Decimal), defaultdict(Decimal)
    detalhes = []
    for chave in sorted(a.registros.keys() | b.registros.keys()):
        cid, cod = chave
        ma, mb = a.minutos.get(chave, zero), b.minutos.get(chave, zero)
        total_a[cod] += ma
        total_b[cod] += mb
        if a.registros.get(chave) != b.registros.get(chave):
            mudadas.add(chave)
            if ma != mb:
                detalhes.append(
                    f"COLAB empresa={empresa_id} colab_id={cid} rubrica={cod} delta_min={mb - ma:f}"
                )
        elif ma != mb:
            raise FalhaSonda(f"Unidade ambígua para registros iguais colab_id={cid} {REF_TXT}")

    linhas = [
        f"TETO empresa={empresa_id} TXT_RUBRICAS_MUDADAS={len(mudadas)} "
        f"COLABS_TXT={len({cid for cid, _ in mudadas})} BATIDAS_TIRADAS={tiradas}"
    ]
    for cod in sorted(total_a.keys() | total_b.keys()):
        ma, mb = total_a[cod], total_b[cod]
        if ma != mb:
            linhas.append(
                f"RUBRICA empresa={empresa_id} rubrica={cod} "
                f"min_A={ma:f} min_B={mb:f} delta_min={mb - ma:f}"
            )
    linhas.append(f"IDEMP empresa={empresa_id} A1==A2={a.txt == a2.txt}")
    return linhas, detalhes


def main():
    if (
        not getattr(settings, "HASNER_SOMBRA", False)
        or connection.settings_dict["HOST"] != "sombra_db"
        or connection.settings_dict["NAME"] != "sombra"
        or connection.schema_name != "juliani"
    ):
        raise FalhaSonda(f"Conexão fora da sombra {REF_SOMBRA}")

    saida, detalhes, diagnosticos = [], [], []
    try:
        with patch.object(logging.Logger, "handle", log_sem_dados), transaction.atomic(durable=True):
            with connection.cursor() as cursor:
                cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ WRITE")
                cursor.execute("SELECT current_database(), current_schema()")
                if cursor.fetchone() != ("sombra", "juliani"):
                    raise FalhaSonda(f"Banco ou schema fora da sombra {REF_SOMBRA}")

            antes = estado_das_linhas()
            seq_antes = sequencias()
            agora = timezone.now()
            empresas = list(Empresa.objects.filter(pk__in=EMPRESAS).order_by("pk"))
            if tuple(e.pk for e in empresas) != EMPRESAS:
                raise FalhaSonda("Escopo incompleto de empresas [app/colaboradores/models.py:40]")

            # Silencia os transportes. As portas de cálculo, cartório e TXT seguem reais.
            # [app/chamados/push_utils.py:12-18,89-111] [nao verificado]
            # [app/chamados/services/disputa_emissao.py:1887-1906]
            with (
                patch.object(timezone, "now", new=lambda: agora),
                patch.object(push_utils, "_enviar_push_usuario", return_value={"ok": False}), [nao verificado]
                patch.object(fcm_utils, "push_fcm_colaborador", return_value=False), [nao verificado]
                redirect_stdout(StringIO()),
                redirect_stderr(StringIO()),
            ):
                for empresa in empresas:
                    alvos = alvos_da_s1(empresa, agora)
                    pks = {pk for ids in alvos.values() for pk in ids}
                    original = medir_txt(empresa)
                    a1 = rodada(empresa, alvos, set(), agora, seq_antes)
                    a2 = rodada(empresa, alvos, set(), agora, seq_antes)
                    b1 = rodada(empresa, alvos, pks, agora, seq_antes)
                   

[truncado em 20000 de 21750 caracteres]

## 27/11 08:29 · sessao 1814e0d2b981 · RAMO2-B-EM-TURNO-QUE-CRUZA · construtor · origem=Code

**c2t parcial.** Apliquei a cura do item 2. Os REDs do Plantão e do banco Comercial ficaram reproduzidos e continuam falhando. As duas curas exigem edição do motor, que a ordem também põe na lista proibida. O teste existente do banco exige −3,5 h para o mesmo caso em que o item 3 pede −4 h. [ponto/motor_calculo_v2.py:3712] [ponto/motor_calculo_v2.py:3781] [ponto/tests/test_ramo2b_c2l_red_motor.py:96] [nao verificado]

### Item 2 · A igualdade é necessária

Reverter os dois `<=` em memória fez falhar `test_pausa_colada_na_entrada_efetiva_e_a_mesma_do_motor` e `test_segmentos_descontam_a_pausa_colada_na_entrada_efetiva`, no 12x36 e no Comercial. A janela recorta a pausa de 18:50–19:20 para 19:00–19:20, colada à entrada efetiva. Com `<`, os dois leitores descartam os 20 min de pausa e devolvem 800 min trabalhados onde o motor devolve 780. [ponto/tests/test_ramo2b_c2h_saida.py:230] [ponto/tests/test_ramo2b_c2h_saida.py:247] [ponto/tests/test_ramo2b_c2h_saida.py:266]

O diff restringe a igualdade ao **dia da jornada** em que algum par tem `saida_da_base_da_he` preenchida. Os dois REDs novos falharam antes da cura porque o dia sem cauda devolvia 100 min em vez de 120. Ambos passaram depois, inclusive com a cauda registrada em outro par do mesmo dia. [ponto/calculador/alimentacao.py:82] [ponto/calculador/alimentacao.py:146] [ponto/calculador/periodo.py:307] [ponto/calculador/periodo.py:625] [ponto/tests/test_ramo2b_c2t_escopo.py:27]

### Plantão e banco Comercial

| Verificação | Resultado |
|---|---|
| Plantão 07:00–07:00(+1), batida às 07:40 | Saída efetiva às 07:40, 1.480 min trabalhados e 580 noturnos. O `main` deu 07:00, 1.440 e 540. A base da HE ficou em 1.440 min no caso testado. [ponto/tests/test_ramo2b_c2t_motor.py:14] |
| Pontualidade do Plantão | O cálculo por par delega ao `Motor12x36.calcular_periodo`, que termina sem chamar `_julgar_pontualidade`. O mensal herdado passa pelo teto L-093. O motor também compartilha o recorte da HE noturna e o crédito de pares fechados em dia aberto. [ponto/motor_calculo_v2.py:3804] [ponto/motor_calculo_v2.py:3099] [ponto/motor_calculo_v2.py:2944] [ponto/motor_calculo_v2.py:1100] [ponto/motor_calculo_v2.py:2763] |
| Limite diário e banco do Plantão | O cálculo delegado usa `_jornada_do_dia`. O `_lim_dia` da c2m e a gravação do saldo semanal pertencem ao override do Comercial. [ponto/motor_calculo_v2.py:3079] [ponto/motor_calculo_v2.py:3672] [ponto/motor_calculo_v2.py:3725] |
| Comercial com cauda de 30 min | O saldo atual deu −3,5 h e o `main` deu −4 h. No cálculo atual, o RED confirmou 2.430 min trabalhados, 2.400 min na base e HE zero antes de falhar no saldo. [ponto/tests/test_ramo2b_c2t_motor.py:39] |

### Execução e diff

Em memória, com as dependências do Django substituídas apenas no ensaio, **11 testes passaram**. Foram os dois REDs do item 2 após a cura, os cinco de `ReleituraDaBaseDaHeTest`, o da pausa com base curta e os três de virada da pontualidade. Os quatro mutantes abaixo deram **rc 1 por asserção** no mesmo ensaio. A execução normal de `test_ramo2b_*` e `test_o280_bateria_da_lei` parou na importação de `django.core`, antes de rodar testes. [ponto/tests/test_ramo2b_c2t_escopo.py:36] [ponto/tests/test_ramo2b_c2h_saida.py:199] [ponto/tests/test_ramo2b_c2l_red_motor.py:63] [ponto/tests/test_ramo2b_c2l_red_motor.py:113] [ponto/tests/ramo2b_c2t_mutantes.py:17] [manage.py:11] [nao verificado]

Comandos para a suíte e os mutantes com Django disponível. [manage.py:18] [ponto/tests/ramo2b_c2t_mutantes.py:31] [ponto/tests/ramo2b_c2l_mutantes.py:68] [ponto/tests/ramo2b_c2n_mutantes.py:51] [nao verificado]

```sh
export PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci

python3 manage.py test \
  $(for f in ponto/tests/test_ramo2b_*.py; do printf 'ponto.tests.%s\n' "$(basename "$f" .py)"; done) \
  ponto.tests.test_o280_bateria_da_lei

python3 -m ponto.tests.ramo2b_c2t_mutantes segmentos_aceitam_pausa_sem_cauda
python3 -m ponto.tests.ramo2b_c2t_mutantes releitura_aceita_pausa_sem_cauda
python3 -m ponto.tests.ramo2b_c2n_mutantes ignora_pausa_na_entrada
python3 -m ponto.tests.ramo2b_c2l_mutantes pausa_na_ponta_descartada
```

O diff tem cinco arquivos e **zero `except` novos**, incluindo os arquivos novos. [ponto/calculador/alimentacao.py:82] [ponto/calculador/periodo.py:291] [ponto/tests/test_ramo2b_c2t_escopo.py:1] [ponto/tests/test_ramo2b_c2t_motor.py:1] [ponto/tests/ramo2b_c2t_mutantes.py:1]

```sh
git --no-optional-locks diff --relative HEAD
for f in ponto/tests/ramo2b_c2t_mutantes.py ponto/tests/test_ramo2b_c2t_*.py; do
  git --no-optional-locks diff --no-index -- /dev/null "$f"
done
```

## 27/11 08:28 · sessao 92b2ddb05e4c · RAIA-ESPURIAS-1811-1410 · leitura · origem=Code

**O efeito das espúrias no TXT da competência 11 está [nao verificado]** para as empresas 2, 3 e 4. A sonda abaixo mede os dias com sobras vivas na grade. A execução na sombra e a correspondência da cópia com o hash informado estão [nao verificado].

**1. Autoridade e contagem.** O próprio `JUIZES.md` aponta para o registro vivo. Nele, a pergunta «a batida é espúria (cluster)?» está em `SEM_JUIZ['batida']`. As duas candidatas que o registro nomeia são `detectar_cluster_espurio`, que delega a `dias_com_cluster` e à assinatura da ata em `_leitura_da_celula` (duas ou mais órfãs com consecutivas a até 15 minutos e ao menos uma lâmpada apagada, em dia de trabalho), e a guarda de `_match_marcos`, que marca como órfã a batida que perde o marco pela disputa de proximidade, ressalvada a exceção da guarda. O propositor também tem um rótulo `ESPURIA` próprio para sobras próximas de um slot ocupado. [app/docs/JUIZES.md:10] [app/core/juizes.py:1231] [app/ponto/management/commands/detectar_cluster_espurio.py:26] [app/ponto/services/cluster_espurio.py:15] [app/ponto/services/cluster_espurio.py:40] [app/escala/utils.py:595] [app/escala/utils.py:654] [app/ponto/propositor.py:169]

Para o passo 3a, usei **espúria no sentido de órfã da grade**, como no cartão. `classificar_orfas` é a função nomeada no aval da razão da sobra e devolve as classes das órfãs que a grade já encontrou. O script conta um dia-colab quando esse retorno tem alguma classe, inclusive `indefinida`. A grade é refeita com `batidas_apuraveis`, cujo predicado de vida é `retratada_em__isnull=True`. Assim, a contagem mede sobras da grade alimentada por batidas vivas. A ata do cluster guarda horas `HH:MM`, o que limita a identificação individual de uma batida. [app/relatorios/pdf_espelho.py:1027] [app/docs/BACKLOG.md:2087] [app/escala/orfa_classe.py:53] [app/ponto/turnos.py:2941] [app/ponto/turnos.py:2961] [app/ponto/services/cartorio.py:761]

**2. Lei e lado B.** A **L-038**, pelo corte `PDF-SEM-REGRA-PROPRIA`, manda mostrar a batida que nenhum turno reclama no dia em que aconteceu. `dia_das_batidas` aplica isso para a apresentação, e a grade põe a sobra sem turno na sua data civil. A **L-025** define o universo da apuração pelo filtro de retratação. O fechamento já usa essa entrada. O motor também já lê papéis da ata, com cobertura parcial. O papel `X` inclui casos de pausa e por isso é insuficiente como seleção de espúrias. [app/docs/LEIS.md:86] [app/docs/CORTES.md:328] [app/ponto/turnos.py:2971] [app/ponto/turnos.py:3034] [app/escala/utils.py:1864] [app/docs/LEIS.md:76] [app/ponto/services/fechamento.py:652] [app/ponto/motor_calculo_v2.py:521] [app/ponto/turnos.py:1768]

O lado A da view usa `recalcular_fechamento_mes` em leitura, `classificar_export`, `montar_linhas_export`, validação do layout e `conteudo_do_txt`. Para o lado B, essa mesma sequência precisaria receber, na entrada do fechamento, as batidas já julgadas pela regra para toda a janela. Não encontrei essa porta na cópia. `dia_das_batidas` devolve datas para apresentação e `classificar_orfas` devolve classes por `HH:MM`. A troca de entrada exigiria decidir quais batidas correspondem às sobras e o que fazer com elas na apuração. Aplico o fallback pedido, só com o passo 3a. O juiz de dono disponível para o passo 3c é `dono_da_divergencia`, chamado por `InsumosDoDono.dono`. [app/folha/views.py:214] [app/folha/views.py:272] [app/folha/views.py:282] [app/ponto/services/fechamento.py:652] [app/ponto/turnos.py:2971] [app/escala/orfa_classe.py:53] [app/ponto/management/commands/e6_oraculo.py:53] [app/ponto/management/commands/e6_oraculo.py:182]

### SCRIPT

```python
import logging
import sys
from datetime import date, datetime, time, timedelta
from traceback import extract_tb
from unittest.mock import patch

from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Colaborador, Empresa
from escala import utils as grade
from escala.orfa_classe import classificar_orfas
from folha.export import janela_competencia
from ponto import turnos

MES, ANO = 11, 2026
EMPRESAS = (2, 3, 4)
JANELA_DO_AVAL = (date(2026, 10, 21), date(2026, 11, 20))

# Passo 3a. A razão da sobra vem por HH:MM e a entrada da folha usa Batida.
# [app/escala/orfa_classe.py:53] [app/ponto/services/fechamento.py:652]


class ReverterSonda(Exception):
    pass


class FalhaSonda(RuntimeError):
    pass


def log_sem_dados(_logger, registro):
    if registro.levelno >= logging.ERROR:
        erro = registro.exc_info[1] if registro.exc_info else sys.exception()
        tipo = type(erro).__name__ if erro is not None else "ERROR"
        raise FalhaSonda(f"{registro.pathname}:{registro.lineno} {tipo}") from None


def contar(empresa, agora):
    ini, fim = janela_competencia(empresa, MES, ANO)
    if (ini, fim) != JANELA_DO_AVAL:
        raise FalhaSonda(
            f"Empresa {empresa.pk}: janela {ini}/{fim} fora do aval "
            "[app/folha/export.py:47] [app/ponto/janelas.py:54]"
        )

    # Entrada e bordas da grade do cartório. [app/ponto/services/cartorio.py:1187-1209]
    primeiro = timezone.make_aware(datetime.combine(ini - timedelta(days=1), time.min))
    depois_do_ultimo = timezone.make_aware(datetime.combine(fim + timedelta(days=3), time.min))
    ultimo = depois_do_ultimo - timedelta(microseconds=1)
    ref = fim + timedelta(days=1)
    dias = (ref - ini).days + 1

    colabs = dias_colab = 0
    pessoas = Colaborador.objects.filter(empresa=empresa).order_by("pk")
    for colab in pessoas.iterator(chunk_size=200):
        batidas = list(turnos.batidas_apuraveis(colab, primeiro, ultimo))
        if not batidas:
            continue
        linhas = grade.montar_grade_prevista_periodo(
            colab, batidas, dias=dias, ref=ref, agora=agora,
        )["dias"]
        if len(linhas) != dias:
            raise FalhaSonda(
                f"Empresa {empresa.pk} colab={colab.pk}: grade fora da janela "
                "[app/escala/utils.py:972] [app/ponto/services/cartorio.py:1203]"
            )
        # linhas[0] é o dia de contexto após o fim. [app/escala/utils.py:972-979]
        n = sum(bool(classificar_orfas(linha)) for linha in linhas[1:])
        colabs += int(n > 0)
        dias_colab += n
    return colabs, dias_colab


def main():
    try:
        with patch.object(logging.Logger, "handle", log_sem_dados), transaction.atomic(durable=True):
            with connection.cursor() as cursor:
                cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            if connection.schema_name != "juliani":
                raise FalhaSonda("Schema esperado juliani [bin/sombra.sh:490]")

            agora = timezone.now()
            # Deixa subir falhas capturadas nesses leitores.
            # [app/escala/utils.py:192,927,1242] [app/ponto/turnos.py:1634]
            with (
                patch.object(timezone, "now", new=lambda: agora),
                patch.object(grade, "Exception", (), create=True),
                patch.object(turnos, "Exception", (), create=True),
            ):
                empresas = list(Empresa.objects.filter(pk__in=EMPRESAS).order_by("pk"))
                if tuple(e.pk for e in empresas) != EMPRESAS:
                    raise FalhaSonda("Escopo incompleto de empresas [app/colaboradores/models.py:40]")
                medidas = [(e.pk, *contar(e, agora)) for e in empresas]
            raise ReverterSonda()
    except ReverterSonda:
        for empresa_id, colabs, dias_colab in medidas:
            print(f"ESPURIAS empresa={empresa_id} colabs={colabs} dias_colab={dias_colab}", flush=True)
    except FalhaSonda:
        raise
    except Exception as erro:
        origem = extract_tb(erro.__traceback__)[-1]
        raise FalhaSonda(f"{origem.filename}:{origem.lineno} {type(erro).__name__}") from None


main()
```

### FIM

## 27/11 07:41 · sessao 2b9617406233 · RAMO2-B-EM-TURNO-QUE-CRUZA · construtor · origem=Code

```diff
diff --git a/app/ponto/tests/test_ramo2b_c2l_red_motor.py b/app/ponto/tests/test_ramo2b_c2l_red_motor.py
--- a/app/ponto/tests/test_ramo2b_c2l_red_motor.py
+++ b/app/ponto/tests/test_ramo2b_c2l_red_motor.py
@@ -9 +9 @@
-from folha.tests.test_porta_do_export import _medir_com_espelho
+from ponto.tests.test_ramo2b_porta_7a_testemunha import _medir_com_espelho
```

O `rg` encontrou uma referência indireta ao caminho antigo em `app/ponto/tests/ramo2b_c2l_mutantes.py:57`. O script passa esse caminho ao carregador de testes em `app/ponto/tests/ramo2b_c2l_mutantes.py:77`, e a classe está em `app/ponto/tests/test_ramo2b_porta_7a_testemunha.py:43`. O diff se limita ao import solicitado.

## 27/11 06:59 · sessao 684d56ff6bd6 · RAMO2-B-EM-TURNO-QUE-CRUZA · construtor · origem=Code

Movi `_medir_com_espelho` e `TestemunhaDaHeNaPortaTest` para `ponto/tests`. Conferi que o helper, os patches e as asserções foram preservados byte a byte. A mudança ficou restrita a dois arquivos.

- [app/folha/tests/test_porta_do_export.py:15](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/folha/tests/test_porta_do_export.py:15) — removi o bloco e os imports de módulo que ficaram sem uso. O trecho de `he_acima_do_teto_por_dia` foi preservado. [Trecho preservado](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/folha/tests/test_porta_do_export.py:133)
- [app/ponto/tests/test_ramo2b_porta_7a_testemunha.py:1](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_porta_7a_testemunha.py:1) — adicionei a docstring de duas linhas e o bloco movido. [Helper](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_porta_7a_testemunha.py:11) · [Testemunha](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_porta_7a_testemunha.py:43)

### RED e mutantes

O RED falhou na base `c57f01232` com o achado `('folha/tests/test_porta_do_export.py', 'MotorBase')`. Após a mudança, os quatro testes do selo passaram. [Asserção do selo](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/core/tests/test_selo_bolha_nao_importa.py:27)

```bash
# RED na base
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest core.tests.test_selo_bolha_nao_importa.SeloBolhaNaoImportaTest.test_MORDE_a_bolha_da_folha_nao_importa_o_motor

# Selo completo após a cura
PYTHONDONTWRITEBYTECODE=1 python3 -W ignore::ResourceWarning -m unittest core.tests.test_selo_bolha_nao_importa
```

**Mutante 1.** Reintroduzi o import em `folha/` na AST em memória. O selo voltou a falhar pelo achado esperado. [Varredura de imports](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/core/mapa/contrato.py:128)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -W ignore::ResourceWarning - <<'PY'
import ast
import unittest
from pathlib import Path
from unittest.mock import patch
from core.mapa import contrato

folha = 'folha/tests/test_porta_do_export.py'
arvore = contrato._arvore
mutante = ast.parse(Path(folha).read_text() + '\nfrom ponto.motor_calculo_v2 import MotorBase\n')
alvo = ('core.tests.test_selo_bolha_nao_importa.SeloBolhaNaoImportaTest.'
        'test_MORDE_a_bolha_da_folha_nao_importa_o_motor')
with patch.object(contrato, '_arvore', side_effect=lambda rel: mutante if rel == folha else arvore(rel)):
    resultado = unittest.TextTestRunner().run(unittest.defaultTestLoader.loadTestsFromName(alvo))
assert resultado.testsRun == 1 and not resultado.errors
assert len(resultado.failures) == 1 and repr((folha, 'MotorBase')) in resultado.failures[0][1]
PY
```

**Mutante 2.** Omiti a classe no texto em memória. A conferência estrutural detectou a perda da testemunha. [Classe e método](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_porta_7a_testemunha.py:43)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'
import ast
from pathlib import Path

p = Path('ponto/tests/test_ramo2b_porta_7a_testemunha.py')
fonte = p.read_text()
classe = 'TestemunhaDaHeNaPortaTest'
metodo = 'test_he_da_cauda_entra_na_lista_e_nas_falhas_da_porta'

def contem_testemunha(texto):
    return any(
        isinstance(n, ast.ClassDef) and n.name == classe
        and any(isinstance(m, ast.FunctionDef) and m.name == metodo for m in n.body)
        for n in ast.parse(texto).body
    )

assert contem_testemunha(fonte)
mutante = fonte.split(f'\nclass {classe}(', 1)[0]
assert not contem_testemunha(mutante)
PY
```

A execução local da testemunha parou no import porque o Python disponível não tem `django.test`. A execução da testemunha e da suíte completa fica para a certificação do Code e do Opus final [nao verificado]. [Import da testemunha](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_porta_7a_testemunha.py:8) [nao verificado]

### Diff

```diff
diff --git a/app/folha/tests/test_porta_do_export.py b/app/folha/tests/test_porta_do_export.py
--- a/app/folha/tests/test_porta_do_export.py
+++ b/app/folha/tests/test_porta_do_export.py
@@ -14,68 +14,11 @@ seriam duas contas para a mesma pergunta -- a doenca que esta serie inteira pers
 """
 import datetime
 import inspect
-from types import SimpleNamespace
-from unittest.mock import MagicMock, Mock, patch
-from zoneinfo import ZoneInfo
 
-from django.test import SimpleTestCase, TestCase
+from django.test import TestCase
 from freezegun import freeze_time
 
 
-def _medir_com_espelho(resultado, *, previsto, declarado):
-    """Mede o espelho fornecido pelo caminho da porta, com as fronteiras de I/O em memória."""
-    from colaboradores.models import Colaborador
-    from escala.models import CelulaDia
-    from folha.porta_export import medir
-    from ponto.models import DiaPago, FechamentoMensal
-
-    empresa = SimpleNamespace(pk=1, dia_inicio_competencia=21)
-    colab = SimpleNamespace(pk=125)
-    sem_linhas = Mock(first=Mock(return_value=None), values_list=Mock(return_value=[]))
-    agora = datetime.datetime(2026, 11, 23, 12, tzinfo=ZoneInfo('America/Sao_Paulo'))
-
-    def cartao_x_txt(*_args, stdout, **_kwargs):
-        stdout.write('cartao_x_txt_divergentes=0\n')
-
-    with (patch('folha.porta_export._tz_ato.now', return_value=agora),
-          patch('folha.export.classificar_export', return_value=[{'status': 'entra', 'colaborador_id': colab.pk}]),
-          patch.object(Colaborador.objects, 'filter', return_value=Mock(first=Mock(return_value=colab))),
-          patch.object(CelulaDia.objects, 'filter', return_value=[]),
-          patch.object(DiaPago.objects, 'filter', return_value=MagicMock(values_list=Mock(return_value=[]))),
-          patch.object(FechamentoMensal.objects, 'filter', return_value=sem_linhas),
-          patch('ponto.services.espelho.espelho_do_colab', return_value={'resultado': resultado}),
-          patch('ponto.services.espelho.autoridade_do_periodo', return_value=SimpleNamespace(resultado=resultado)),
-          patch('relatorios.pdf_espelho._coletar_dados_espelho_mes', return_value={}),
-          patch('ponto.services.fechamento.empresas_exportadas_no_escopo', return_value=[]),
-          patch('ponto.services.dia_pago.soma_da_competencia', return_value=None),
-          patch('escala.utils.minutos_previstos_do_dia', return_value=previsto),
-          patch('escala.utils.intervalo_declarado_do_dia', return_value=declarado),
-          patch('django.core.management.call_command', side_effect=cartao_x_txt)):
-        return medir(empresa, 11, 2026)
-
-
-class TestemunhaDaHeNaPortaTest(SimpleTestCase):
-    def test_he_da_cauda_entra_na_lista_e_nas_falhas_da_porta(self):
-        # L-097(b), L-031: trabalhado 800, base e previsto 720, intra 60,
-        # declarado 0. HE 80 passa do teto 0 e aciona a sétima testemunha.
-        from ponto.motor_calculo_v2 import MotorBase, PeriodoCalculo
-
-        entrada = datetime.datetime(2026, 11, 5, 19, tzinfo=ZoneInfo('America/Sao_Paulo'))
-        p = PeriodoCalculo(entrada, entrada + datetime.timedelta(minutes=800),
-                           minutos_trabalhados=800, minutos_intrajornada_indenizada=60, minutos_extra=80)
-        MotorBase._escrever_base_da_he(p, 720, entrada + datetime.timedelta(minutes=720), 0)
-        resultado = SimpleNamespace(periodos=[p], periodos_ft=[], dia_da_jornada={id(p): entrada.date()})
-
-        medicao = _medir_com_espelho(resultado, previsto=720, declarado=0)
-
-        self.assertEqual(medicao['universo'], 1)
-        self.assertEqual(medicao['minuto_em_duas_rubricas'], [
-            {'colab': 125, 'dia': '2026-11-05', 'he_min': 80, 'intra_min': 60,
-             'trab_min': 800, 'previsto_min': 720, 'teto_he': 0},
-        ])
-        self.assertEqual((medicao['falhas_proprias'], medicao['falhas']), (1, 1))
-
-
 # RELOGIO CONGELADO: este arquivo fala de "carimbo de HOJE" e "carimbo de ONTEM", entao ele LE o relogio por
 # construcao -- e o selo `test_selo_teste_sem_relogio::test_MORDE_teste_novo_com_relogio_solto_e_vermelho` me
 # cobrou na hora, com razao. Congelado, as duas datas sao deterministicas e a lei do carimbo continua medida.
diff --git a/app/ponto/tests/test_ramo2b_porta_7a_testemunha.py b/app/ponto/tests/test_ramo2b_porta_7a_testemunha.py
new file mode 100644
--- /dev/null
+++ b/app/ponto/tests/test_ramo2b_porta_7a_testemunha.py
@@ -0,0 +1,62 @@
+"""Este caso mora em ponto/ porque constrói o PeriodoCalculo pelo motor.
+A bolha da folha não importa o motor (selo test_selo_bolha_nao_importa)."""
+import datetime
+from types import SimpleNamespace
+from unittest.mock import MagicMock, Mock, patch
+from zoneinfo import ZoneInfo
+
+from django.test import SimpleTestCase
+
+
+def _medir_com_espelho(resultado, *, previsto, declarado):
+    """Mede o espelho fornecido pelo caminho da porta, com as fronteiras de I/O em memória."""
+    from colaboradores.models import Colaborador
+    from escala.models import CelulaDia
+    from folha.porta_export import medir
+    from ponto.models import DiaPago, FechamentoMensal
+
+    empresa = SimpleNamespace(pk=1, dia_inicio_competencia=21)
+    colab = SimpleNamespace(pk=125)
+    sem_linhas = Mock(first=Mock(return_value=None), values_list=Mock(return_value=[]))
+    agora = datetime.datetime(2026, 11, 23, 12, tzinfo=ZoneInfo('America/Sao_Paulo'))
+
+    def cartao_x_txt(*_args, stdout, **_kwargs):
+        stdout.write('cartao_x_txt_divergentes=0\n')
+
+    with (patch('folha.porta_export._tz_ato.now', return_value=agora),
+          patch('folha.export.classificar_export', return_value=[{'status': 'entra', 'colaborador_id': colab.pk}]),
+          patch.object(Colaborador.objects, 'filter', return_value=Mock(first=Mock(return_value=colab))),
+          patch.object(CelulaDia.objects, 'filter', return_value=[]),
+          patch.object(DiaPago.objects, 'filter', return_value=MagicMock(values_list=Mock(return_value=[]))),
+          patch.object(FechamentoMensal.objects, 'filter', return_value=sem_linhas),
+          patch('ponto.services.espelho.espelho_do_colab', return_value={'resultado': resultado}),
+          patch('ponto.services.espelho.autoridade_do_periodo', return_value=SimpleNamespace(resultado=resultado)),
+          patch('relatorios.pdf_espelho._coletar_dados_espelho_mes', return_value={}),
+          patch('ponto.services.fechamento.empresas_exportadas_no_escopo', return_value=[]),
+          patch('ponto.services.dia_pago.soma_da_competencia', return_value=None),
+          patch('escala.utils.minutos_previstos_do_dia', return_value=previsto),
+          patch('escala.utils.intervalo_declarado_do_dia', return_value=declarado),
+          patch('django.core.management.call_command', side_effect=cartao_x_txt)):
+        return medir(empresa, 11, 2026)
+
+
+class TestemunhaDaHeNaPortaTest(SimpleTestCase):
+    def test_he_da_cauda_entra_na_lista_e_nas_falhas_da_porta(self):
+        # L-097(b), L-031: trabalhado 800, base e previsto 720, intra 60,
+        # declarado 0. HE 80 passa do teto 0 e aciona a sétima testemunha.
+        from ponto.motor_calculo_v2 import MotorBase, PeriodoCalculo
+
+        entrada = datetime.datetime(2026, 11, 5, 19, tzinfo=ZoneInfo('America/Sao_Paulo'))
+        p = PeriodoCalculo(entrada, entrada + datetime.timedelta(minutes=800),
+                           minutos_trabalhados=800, minutos_intrajornada_indenizada=60, minutos_extra=80)
+        MotorBase._escrever_base_da_he(p, 720, entrada + datetime.timedelta(minutes=720), 0)
+        resultado = SimpleNamespace(periodos=[p], periodos_ft=[], dia_da_jornada={id(p): entrada.date()})
+
+        medicao = _medir_com_espelho(resultado, previsto=720, declarado=0)
+
+        self.assertEqual(medicao['universo'], 1)
+        self.assertEqual(medicao['minuto_em_duas_rubricas'], [
+            {'colab': 125, 'dia': '2026-11-05', 'he_min': 80, 'intra_min': 60,
+             'trab_min': 800, 'previsto_min': 720, 'teto_he': 0},
+        ])
+        self.assertEqual((medicao['falhas_proprias'], medicao['falhas']), (1, 1))
```

## 27/11 06:51 · sessao 6c7f3f7c903e · RAMO2-B-EM-TURNO-QUE-CRUZA · leitura · origem=Code

**O atraso aparece porque a obra muda o dia usado para buscar o marco de entrada.** A mudança de `efetivo_saida` explica a passagem da cauda de `fora` para `trabalhado`. O cálculo da pontualidade recebe a **saída bruta** nas duas versões. `app/ponto/motor_calculo_v2.py:2166,2206,2330` no main → `app/ponto/motor_calculo_v2.py:2265–2267,2307,2431` na obra. As linhas do main abaixo foram reconstituídas pelo `diff.txt`.

### 1. Onde o main deixava o atraso em zero

No main, `Motor12x36ComEscala` chama `_julgar_pontualidade`, que consulta `marcos_do_dia(e.date())` e ancora a entrada prevista em `combine(e.date(), hora_prev)`. Se `hora_prev` for falsy, retorna com o atraso inicial em zero. Se houver marco, a condição para lançar atraso é `_de is not None and _de > 10`, com `_de = entrada_bruta − prevista_entrada`. Assim, se o main encontrou os horários informados para o 218 de 15/11 e o 200 de 03/11 na própria data civil, comparou `03:17:11` com `21:00` e `02:00:33` com `22:00` **do mesmo dia**. Os dois deltas são negativos. Os marcos que a consulta civil retornou de fato, inclusive para o 218 de 22/11, 296, 601 e 841, estão **[nao verificado]**. `dias.txt:3–10`; `app/ponto/motor_calculo_v2.py:60,226,2087,2119,2166,2179–2180,2206,3674` no main → `app/ponto/motor_calculo_v2.py:60,226,2182,2214,2265–2267,2280–2281,2307,3827` na obra.

Os hunks `@@ -2162,8 +2257,14`, `@@ -2203,7 +2304,7` e `@@ -2228,7 +2329,7` passam `_dia_j`, obtido de `_dia_do_turno_do_par(entrada)`, à consulta dos marcos, à data do marco de entrada e ao teste de volta do intervalo. A L-084 só devolve `(0, 0)` se **ambas** as distâncias aos marcos existirem e tiverem módulo **maior que 180 min**. Com o `hf` correto e as saídas próximas dele no anexo, a ponta de saída fica abaixo desse limiar. A igualdade `efetivo_saida == marco` é um resultado do recorte da janela. O hunk `@@ -1842,28 +1896,34` troca o retorno `(marco, fora)` por `(saida, 0.0, marco)` quando há bloqueio e o turno cruza, preservando o marco para a base da HE. `app/ponto/motor_calculo_v2.py:1863,2059–2062,2101–2118,2166,2206,2231–2232,2330` no main → `app/ponto/motor_calculo_v2.py:1908–1920,2154–2157,2196–2213,2265–2267,2307,2331–2333,2431` na obra.

### 2. Colab 218, em 15/11

**Sim. `420 − 102,87 = 317,13 min`**, exatamente o atraso exibido. Com o marco informado de `21:00–05:00`, o bruto reconstruído é **377,18 min**, de **14/11 às 21:00 (D−1)** até **15/11 às 03:17:11 (D)**. O marco previsto pode existir em 14/11 mesmo sem um par batido naquela noite. O resultado é compatível com a L-093 reduzindo o bruto a `317,13`. O valor imediatamente antes do rateio, após eventual abono parcial, está **[nao verificado]**. `dias.txt:3–5`; `app/ponto/motor_calculo_v2.py:1225–1247,1276–1286,2087,2206` no main → `app/ponto/motor_calculo_v2.py:1268–1290,1319–1329,2182,2265–2267,2307` na obra.

### 3. Colab 841

O rateio fica em `_aplicar_teto_pontualidade`. Se `soma > teto`, ele multiplica cada atraso e cada antecipada do dia por `teto / soma` e arredonda cada campo a duas casas. Os valores exibidos somam **38,25 min** nas duas versões (`38,25 + 0` e `5,42 + 32,83`). A mudança de data do marco permite que o segundo par entre na soma com atraso positivo e passe a receber parte do teto. A cauda do segundo par já entrava no cálculo do teto no main como `fora` e entra na obra como `trabalhado`. Para esse par, os valores publicados são `6,47 + 5,90` e `12,38`, respectivamente. O rateio observado é compatível com os dois pares no mesmo grupo diário. O mapa do grupo e os valores anteriores ao rateio estão **[nao verificado]**. `dias.txt:10`; `app/ponto/motor_calculo_v2.py:1125–1128,1263–1286,2166,2206,2831` no main → `app/ponto/motor_calculo_v2.py:1168–1171,1306–1329,2265–2267,2307,2944` na obra.

### 4. Dia da jornada pela L-085

A diferença mostrada pelo diff é **qual dia a pontualidade consulta**. O main já usava o mapa do juiz na janela e no agrupamento mensal. O agrupamento usa a entrada bruta e pode herdar o dia do par anterior quando o intervalo entre eles é de até seis horas, medido pela **saída bruta**. No 841, os dois pares estão separados por `1h51min14s`, o que satisfaz essa condição se forem consecutivos no laço. Uma mudança de `dia_da_jornada` entre as versões para qualquer par do anexo está **[nao verificado]**. `dias.txt:10`; `app/ponto/motor_calculo_v2.py:972–973,1646,2510–2521,2536,2573` no main → `app/ponto/motor_calculo_v2.py:1015–1016,1692,2611–2622,2637,2674` na obra.

## 27/11 06:25 · sessao 0496a3491b21 · RAMO2-B-EM-TURNO-QUE-CRUZA · verificador · origem=Code

## Volta 2

1. **`P_CRUZA_PARTIDO_SEM_DECISAO` — fechado.** O P2 bate saída às 07:30, conta até 07:00 e dá **240 min trabalhados, 30 fora, 240 noturnos de relógio, HE50 = 0 e HE100 = 0**. Com P1, são `300 + 240 = 540` min para uma jornada de 540. O controle `Autorizado` mantém 07:30, 270 trabalhados e 30 de HE50. O recibo do hash `c57f0123` registra **17/17 OK** no módulo. `app/ponto/tests/test_ramo2b_turno_que_cruza.py:141–154,203–241,279–283` · `app/ponto/motor_calculo_v2.py:1900–1920,3106,3131–3146` · `tqc.log:2–17`

2. **Split da HE noturna — divergência confirmada na cópia, origem em relação ao main pendente.** No Partido, o recorte devolve `s_he == s`, então a tripla fica `None`. O motor usa `periodo.saida` **bruta** nesse caso. O oráculo passa `efetivo_saida` à mesma função de split. Em memória, com saída batida às 05:30, efetiva às 04:00, prorrogação desligada e HE50 de `180/7` min fornecida ao período, obtive **0 min de HE50 noturna no motor e 25,7142857 min pela ponta que o oráculo usa**. O teste `PARTIDO_LEGAIS` exige HE zero, e os dois `P_CRUZA_PARTIDO_*` conferem HE50/100 sem conferir seus subconjuntos noturnos. `app/ponto/motor_calculo_v2.py:464–485,1094–1109,1900–1920,1967–1971,3106` · `app/ponto/calculador/alimentacao.py:88–115` · `app/ponto/calculador/regras.py:845–865` · `app/ponto/tests/test_ramo2b_c2_base_da_he.py:392–403` · `app/ponto/tests/test_ramo2b_turno_que_cruza.py:227–230`

   A leitura de `periodo.saida` em `59600163c:app/ponto/motor_calculo_v2.py:1059` é **[nao verificado]**. A cópia veio sem metadados Git e o diff solicitado falhou **[nao verificado]**. Se o main já passa a saída bruta com os mesmos insumos, o defeito do **motor** é preexistente e cabe na fila. A antiguidade da divergência no **oráculo** exige conferir também a ponta que ele passava no main. A linha `PARTIDO-HE-NOTURNA-SAIDA-BRUTA` fica **[nao verificado]**. O registro visível `PAUSA-INTERNA-NO-PARTIDO` trata da pausa dentro do período. `app/ponto/motor_calculo_v2.py:1094–1109` · `app/ponto/calculador/alimentacao.py:106–115` · `app/docs/BACKLOG.md:2159–2161` · `CLAUDE.md:425–426`

3. **Equivalência com o main — fechada para as asserções dos dois controles, pendente para o restante.** O recibo registra uma foto de trabalho `d72222b3` sobre `59600163c`, com dois testes passando e dois `ImportError`. Os dois `P_CRUZA_PARTIDO_*` dão evidência pelo `calcular_mes` para o par-alvo, o dia atribuído pelo juiz, a saída efetiva, os minutos arredondados, a acusação de cadastro e a conservação do bruto. Os dois testes c2p param no import antes de rodar `SEMANA_FLOAT` ou `PARTIDO_LEGAIS`. Portanto, o recibo comprova a ausência dos dois helpers naquela foto. Os valores dessas duas fixtures no main, o split, a equivalência bit a bit e as saídas dos demais leitores ficam **[nao verificado]**. `main.log:2–30` · `app/ponto/tests/test_ramo2b_turno_que_cruza.py:203–241` · `app/ponto/tests/test_ramo2b_c2_base_da_he.py:160–176,222–236,378–403`

## Alcance do motor e dos leitores

O anexo `delta.patch` traz os dois arquivos de teste. A lista **exata** de hunks de `59600163c..HEAD` fica **[nao verificado]**. Estes são os trechos da cópia que consegui rastrear até o Partido e o Partido Noturno, que herda o mesmo cálculo. `delta.patch:1` · `app/ponto/motor_calculo_v2.py:3103–3147,3830–3837`

| Trecho alcançado | Efeito verificável na cópia |
|---|---|
| `PeriodoCalculo`, helpers e porta da tripla | O construtor passa pela porta. Com a tripla vazia, os helpers leem o trabalhado e `noturno legal − noturno de relógio`. `app/ponto/motor_calculo_v2.py:235–299,1941–1971` |
| Flag, janela e `MotorTurnoPartido.calcular_periodo` | O `False` do Partido mantém `s_he == s`. Com recorte, ambos ficam no marco e a sobra vai para `minutos_fora_janela`. Com autorização, ambos ficam na batida. `app/ponto/motor_calculo_v2.py:792–795,1898–1939,3103–3146` |
| `MotorBase.calcular_mes` herdado | O bloqueio de folga/feriado sem previsto zera a tripla com as rubricas. O crédito de turno aberto acumula trabalhado, fora e base. Para os pares produzidos pelo Partido, o crédito lê a base pelo trabalhado e mantém a tripla vazia. `app/ponto/motor_calculo_v2.py:1628–1643,2671–2675,2763–2786,3155–3158` |
| `MotorTurnoPartido.calcular_mes` | A soma diária que alimenta também `semanal_44`, o piso de intra e a dobra de feriado leem o helper de trabalhado. A régua `legais` lê o helper de ganho. Com a tripla vazia, essas leituras devolvem os números do próprio par. O split compartilhado é chamado na dobra, na HE diária e na semanal. `app/ponto/motor_calculo_v2.py:286–299,3193–3195,3237–3252,3342–3354,3375–3440` |

O trecho da virada em `_julgar_pontualidade` é chamado pelo Comercial e pelo `Motor12x36ComEscala`. O Partido usa a própria Zona 5, com marcos do dia da jornada. `app/ponto/motor_calculo_v2.py:2260–2267,3257–3330,3566,3827`

- **`reler`.** É caminho de ensaio. Mede e reescreve o trabalhado pelo segmento efetivo e, com a tripla vazia, o helper lê esse mesmo valor para excesso e dobra. Também reescreve intra, HE e, quando recebe CCT, noturno. Um delta numérico contra o main por esse caminho fica **[nao verificado]**. `app/ponto/calculador/periodo.py:547–575,753–785,843–875`
- **`porta_export`.** O teto agrupa pelo mapa estrito de dia da jornada e usa o helper. Para o Partido, a base cai no trabalhado. A conferência só entra com **HE e intra positivas** e usa base de relógio e previsto líquido. A régua `legais` do motor tem ainda ganho noturno e limite de 480. Mudança no resultado da porta contra o main ou no TXT fica **[nao verificado]**. O split noturno, isoladamente, não altera as linhas de HE emitidas pelo TXT, que usa os totais e o adicional noturno. `app/folha/porta_export.py:342–366,588–595` · `app/ponto/motor_calculo_v2.py:3382–3409` · `app/folha/export.py:420–448`
- **Oráculo do fechamento.** Para o Partido com tripla vazia, a base segue os segmentos efetivos sem marcar `dias_com_cauda`. A divergência demonstrada na cópia está na **ponta do split**. O fechamento grava o split do motor e também lavra a versão `oraculo` com rubricas próprias. O valor dessa rubrica no main e o impacto em produção ficam **[nao verificado]**. `app/ponto/calculador/alimentacao.py:106–115,166–178,469–474,545–548` · `app/ponto/services/fechamento.py:938–944,1289–1318`

## Quem escreve e quem lê

| Campo conferido | Escrita e leitura |
|---|---|
| `entrada`, `saida`, `efetivo_entrada`, `efetivo_saida` | `calcular_periodo` inicializa as batidas cruas. `_carimbar_segmento` grava as efetivas e o bloqueio integral pode colapsar a saída efetiva. O split do motor lê as cruas no fallback. `reler` e o oráculo leem o segmento efetivo. `app/ponto/motor_calculo_v2.py:1094–1105,1642–1643,2042–2050,3122–3140` · `app/ponto/calculador/periodo.py:753–761` · `app/ponto/calculador/alimentacao.py:88–115` |
| `minutos_trabalhados`, `minutos_fora_janela`, `intra_descontada` | Os calculadores por par gravam trabalhado e fora. O Partido deixa o desconto interno no default. Bloqueio integral, crédito de aberto e `reler` têm reescritas próprias. O teto de pontualidade lê trabalhado + fora, `DiaPago` soma trabalhado, e o oráculo lê fora e o desconto para montar seus segmentos. `app/ponto/motor_calculo_v2.py:264,1319–1321,1628–1636,2777–2778,3015–3028,3139–3146,3501–3517` · `app/ponto/calculador/periodo.py:182–187,743–769` · `app/ponto/services/dia_pago.py:173–186` · `app/ponto/calculador/alimentacao.py:98–99,133–147` |
| `minutos_base_da_he`, `saida_da_base_da_he`, `ganho_noturno_da_base_da_he` | `_escrever_base_da_he` grava os três juntos. Motor, `reler` e porta leem a base pelo helper. O mensal `legais` lê o ganho pelo outro helper. Split, `reler` e oráculo leem a ponta. `app/ponto/motor_calculo_v2.py:286–299,1100–1105,1942–1971,3193–3195,3386–3390` · `app/ponto/calculador/periodo.py:758–767` · `app/folha/porta_export.py:342–354` · `app/ponto/calculador/alimentacao.py:114–115,168–178` |
| Noturno de relógio/legal, HE total/50/100, intra indenizada | `_escrever_noturnas` grava o par de noturnas. O mensal do Partido grava HE e o suprimido do vão. O bloqueio integral zera esses campos e `reler` os reescreve no ensaio. Ganho, split e limite leem os campos correspondentes. `DiaPago` soma as rubricas e a porta usa HE total e intra no teto. `app/ponto/motor_calculo_v2.py:293–299,1085–1109,1632–1636,1973–2011,3244–3252,3347–3354,3402–3409,3435–3440` · `app/ponto/calculador/periodo.py:769–785,866–875` · `app/ponto/services/dia_pago.py:178–186` · `app/folha/porta_export.py:350–361` |
| Dia do turno e acusação de cadastro | O mensal guarda o mapa do juiz e expõe o dia por período. `_acusa_cadastro_x_realidade` acrescenta a acusação de janela. O teste lê os dois, o oráculo e a porta usam o mapa por período e o espelho consome o relato. `app/ponto/motor_calculo_v2.py:1812–1829,2609–2617,2945,3190–3209` · `app/ponto/tests/test_ramo2b_turno_que_cruza.py:224–233` · `app/ponto/calculador/alimentacao.py:78–88` · `app/folha/porta_export.py:342–348` · `app/ponto/services/espelho.py:748–755` |

## Checklist

- **2º juiz:** o delta apresentado altera testes. Os casos mensais passam pela escolha de motor e pelo juiz de turno. O novo caso direto injeta a janela pronta, então a montagem real da janela fica coberta apenas pelos casos que passam pelo mensal. `delta.patch:1` · `app/core/juizes.py:222–224` · `app/ponto/tests/test_ramo2b_turno_que_cruza.py:203–226` · `app/ponto/tests/test_ramo2b_c2_base_da_he.py:437–458`
- **2º escritor:** executei a varredura AST do próprio selo em leitura. Para a tripla, ela encontrou apenas `_escrever_base_da_he`. Para as demais rubricas, `reler` é um escritor de ensaio e o motor tem os ajustes indicados acima. A varredura declara suas limitações para atributos dinâmicos. `app/ponto/tests/test_ramo2b_c2_base_da_he.py:644–677` · `app/ponto/motor_calculo_v2.py:1954–1958` · `app/ponto/calculador/periodo.py:547–552,743–785`
- **Relógio solto:** os casos mensais congelam o tempo. O teste direto usa instantes fixos, feriados vazios e janela injetada. `app/ponto/tests/test_ramo2b_turno_que_cruza.py:168` · `app/ponto/tests/test_ramo2b_c2_base_da_he.py:124,443–458`
- **Derivação paralela:** `reler` mede o trabalhado pelo carimbo e o oráculo mede os segmentos. O split tem uma função compartilhada, com **pontas diferentes** no caso demonstrado. `app/ponto/calculador/periodo.py:753–769` · `app/ponto/calculador/alimentacao.py:106–115,469–474` · `app/ponto/calculador/regras.py:845–865`
- **Selo com mordida:** o novo teste direto distingue `42761/60` de `44960/60 − 2199/60`, que diferem em ponto flutuante. Os selos do Partido cobram o recorte, mas deixam descoberto o split com HE positiva e cauda excluída. A suíte integral desta volta fica **[nao verificado]**. `app/ponto/tests/test_ramo2b_c2_base_da_he.py:222–236,392–403,443–463` · `app/ponto/tests/test_ramo2b_turno_que_cruza.py:227–230` · `tqc.log:3–17`
- **Campo com dois significados:** `minutos_trabalhados` é o contado, e a presença para o teto soma trabalhado e fora. A tripla `None` significa que a base numérica coincide com o trabalhado. O comentário “par sem cauda” também é usado para um Partido com cauda física já recortada, e o fallback do split trata esse caso como se pudesse usar a batida bruta. `app/ponto/motor_calculo_v2.py:228–246,1096–1105,1319–1321,1917–1920,1963–1969`

## Veredito — REPROVADO

1. **Item 1 fechado.** A asserção corrigida bate com a conta e o módulo registra 17/17 OK. `app/ponto/tests/test_ramo2b_turno_que_cruza.py:141–154` · `tqc.log:3–17`
2. **Item 2 pendente.** A diferença de split entre motor e oráculo está demonstrada na cópia. Para baixar o achado como fila preexistente, falta conferir as duas pontas no main e localizar o registro alegado **[nao verificado]**. `app/ponto/motor_calculo_v2.py:1094–1109` · `app/ponto/calculador/alimentacao.py:106–115` · `CLAUDE.md:425–426`
3. **Item 3 parcialmente fechado.** Os dois controles mensais sustentam equivalência para o que conferem. Os dois imports interrompem as outras comparações, e a lista exata de hunks e a equivalência dos demais caminhos seguem **[nao verificado]**. `main.log:2–30` · `app/ponto/tests/test_ramo2b_turno_que_cruza.py:217–241` · `app/ponto/tests/test_ramo2b_c2_base_da_he.py:222–236,387–403`

## 27/11 06:17 · sessao 25a38f481b30 · RAMO2-B-EM-TURNO-QUE-CRUZA · construtor · origem=Code

1. **Sem decisão.** Pela RAMO2-PARTIDO-2611-1812 (a) e pela L-097(b), o Partido 19:00–07:00 tem vão de 00:00 a 03:00 e o segundo par termina efetivamente no marco de 07:00. São **240 min trabalhados**, **30 min fora da janela** e **240 min noturnos** de 03:00 a 07:00 com a prorrogação da fixture. A HE do dia é `300 + 240 − 540 = 0`. A conta coincide com a saída registrada no anexo `tqc.log`. `docs/LEIS.md:115`, `ponto/motor_calculo_v2.py:3106`, [ponto/tests/test_ramo2b_turno_que_cruza.py:141](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_turno_que_cruza.py:141)

2. **Autorizado.** A conta concorda com o teste. A janela mantém a saída às 07:30, com 270 min trabalhados, 270 noturnos e zero fora da janela. `300 + 270 − 540 = 30` min de HE50 no último par, com HE100 zero. `ponto/motor_calculo_v2.py:1906`, `ponto/motor_calculo_v2.py:3402`, [ponto/tests/test_ramo2b_turno_que_cruza.py:154](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_turno_que_cruza.py:154)

3. **`acusa=False`.** `_TPART` fixa o marco em 07:00 na linha 60. O cadastro da empresa da fixture fixa `teto_saida` em 60 min na linha 179. A regra acusa acima do teto. Como `30 < 60`, a saída fica abaixo dele. `ponto/tests/test_ramo2b_turno_que_cruza.py:60`, `ponto/tests/test_ramo2b_turno_que_cruza.py:179`, `ponto/janela_he.py:200`

### Diff

```diff
--- a/app/ponto/tests/test_ramo2b_turno_que_cruza.py
+++ b/app/ponto/tests/test_ramo2b_turno_que_cruza.py
@@ -141,8 +141,8 @@
-    # TURNO PARTIDO que cruza (19-07, vao 00-03 > 2 h = partido de verdade): par 03:00 -> 07:30 de D+1, 30 min depois
-    # do marco 07:00. Conta 270; noturno com prorrogacao (jornada comum, Sumula 60 II): 03:00 -> 07:30 = 270. HE do
-    # DIA pela base recortada: 300 + 240 = 540 contra 540 -> 0.
+    # RAMO2-PARTIDO-2611-1812 (a) e L-097(b): no Partido 19-07 (vao 00-03), sem decisao, o par 03:00 -> 07:30
+    # de D+1 recorta no marco 07:00: 240 trabalhados, 30 fora da janela e 240 noturnos (03:00 -> 07:00 com
+    # prorrogacao). HE do dia: 300 + 240 = 540 contra a jornada de 540 -> 0.
     'P_CRUZA_PARTIDO_SEM_DECISAO': (
         _TPART, _cels(_TPART, _D),
         [(_D, '19:00:00', 'E'), (_D1, '00:00:00', 'S'), (_D1, '03:00:00', 'E'), (_D1, '07:30:00', 'S')], _D, None,
         '2026-11-06 03:00',
-        ('2026-11-06 03:00', '2026-11-06 07:30', 270.0, 0.0, 270.0, 0.0, 0.0), False),
+        ('2026-11-06 03:00', '2026-11-06 07:00', 240.0, 30.0, 240.0, 0.0, 0.0), False),
```

### RED e mutantes

Reproduzi em memória a tupla do anexo pelo `test_RED_partido_cruza_sem_decisao`, substituindo `_roda` pela observação registrada. A expectativa corrigida passou. A tupla antiga e os quatro mutantes individuais falharam na comparação da tupla. Suite Django após o diff e certificação do Code e do Opus final [nao verificado]. `ponto/tests/test_ramo2b_turno_que_cruza.py:230`, `ponto/tests/test_ramo2b_turno_que_cruza.py:279`

```bash
tqc_replay() {
  PYTHONDONTWRITEBYTECODE=1 python3 -S - "$1" <<'PY'
import ast
import sys
import unittest
from datetime import datetime
from pathlib import Path
from types import SimpleNamespace

nome = 'P_CRUZA_PARTIDO_SEM_DECISAO'
modo = sys.argv[1]
arquivo = Path('ponto/tests/test_ramo2b_turno_que_cruza.py')
arvore = ast.parse(arquivo.read_text())
arvore.body = [n for n in arvore.body if not isinstance(n, ast.ImportFrom)]
arvore.body[-1].decorator_list = []
escopo = dict(TestCase=unittest.TestCase, timezone=SimpleNamespace(localtime=lambda dt: dt))
exec(compile(arvore, str(arquivo), 'exec'), escopo)

casos = escopo['CASOS']
caso = casos[nome]
esperado = list(caso[-2])
antigos = {1: '2026-11-06 07:30', 2: 270.0, 3: 0.0, 4: 270.0}
for i in antigos if modo == 'red' else [] if modo == 'controle' else [int(modo)]:
    esperado[i] = antigos[i]
casos[nome] = (*caso[:-2], tuple(esperado), caso[-1])

e, s, batida = map(datetime.fromisoformat,
                   ('2026-11-06 03:00', '2026-11-06 07:00', '2026-11-06 07:30'))
par = SimpleNamespace(entrada=e, saida=batida, efetivo_entrada=e, efetivo_saida=s,
                      minutos_trabalhados=240, minutos_fora_janela=30, minutos_noturnos_relogio=240,
                      minutos_extra_50=0, minutos_extra_100=0)
motor = SimpleNamespace(_dia_turno_juiz_calc={e: '2026-11-05'}, _cadastro_x_realidade=[])
teste = escopo['Ramo2bTurnoQueCruzaTest']('test_RED_partido_cruza_sem_decisao') [nao verificado]
teste._roda = lambda nome: (motor, None, [par]) [nao verificado]
resultado = teste.run() [nao verificado]
assert not resultado.errors, resultado.errors
assert resultado.wasSuccessful() == (modo == 'controle'), resultado.failures
for _, falha in resultado.failures:
    assert 'Tuples differ' in falha and nome in falha, falha
print(modo, 'PASSOU' if resultado.wasSuccessful() else 'REJEITADO na comparação da tupla') [nao verificado]
PY
}

tqc_replay controle  # expectativa corrigida
tqc_replay red       # tupla antiga
tqc_replay 1         # saída efetiva 07:30
tqc_replay 2         # trabalhado 270
tqc_replay 3         # fora da janela 0
tqc_replay 4         # noturno 270
```

## 27/11 06:11 · sessao 4f8a76333721 · RAMO2-B-EM-TURNO-QUE-CRUZA · verificador · origem=Code

## Aval e main

**A leitura da c2p é defensável, mas o texto permite outra leitura.** A opção (a) responde a uma parada sobre **pausa interna**. A expressão “motor do Partido intocado”, junto do recorte de pouso para 12x36 e Comercial, sustenta preservar também o tratamento anterior da cauda. A leitura mais estreita adia apenas a grade de pausa interna e mantém a emenda geral da L-097(b), que diz que “nenhum par perde trabalhado”. A opção (a) delimita o pouso, mas deixa implícita a consequência para `minutos_fora_janela` no Partido. [`PROMPTS.md:3087`](/home/ronald/codex_ws/c19bf0db27a334753ab8fe9cf17fad0b5c9b585e/app/docs/PROMPTS.md:3087) · [`LEIS.md:115`](/home/ronald/codex_ws/c19bf0db27a334753ab8fe9cf17fad0b5c9b585e/app/docs/LEIS.md:115)

**Na cópia, o Partido põe a cauda não autorizada em `minutos_fora_janela` quando a janela se aplica.** O `False` da classe leva ao retorno `(marco, fora, marco)`, e o calculador grava `fora`. `Autorizado` passa antes desse ramo e mantém a saída batida. O comentário e dois controles descrevem o recorte como comportamento do main. Para **`59600163c` especificamente**, a cópia veio sem histórico Git e o anexo traz apenas o delta de testes. A confirmação direta desse commit fica **[nao verificado]**. `motor_calculo_v2.py:1900–1920,3106,3131–3146` · `test_ramo2b_c2h_saida.py:147–166` · `test_ramo2b_c2l_red_motor.py:51–61`

## 1. Contas

| Caso | Conta refeita |
|---|---|
| **Partido `SEMANA_FLOAT`** | Segunda, 20:07:19 → 05:00:00 = `31961/60 = 532,683333… min`. Até a batida de 05:36:39 são `34160/60 = 569,333333… min`. Cauda = `2199/60 = 36,65 min`. Na cópia, **trabalhado = base = 532,683333…**, com **36,65 fora**. As bases de segunda a sexta são `31961`, `32277`, `31873`, `32374` e `32915` segundos. Somam `2690 min`, exatamente `2640 + 5 × 10`. **HE semanal zero**, sem alerta. `test_ramo2b_c2_base_da_he.py:79–97,213–236` · `motor_calculo_v2.py:3193–3195,3410–3439` |
| **Partido 6x1 `legais`** | P1 = `221 min`. P2 até 04:00 = `180 min`, todos noturnos. De 04:00 a 05:00 ficam `60 min` fora. Base de relógio = `401`. Hora reduzida = `180 × 8/7 = 1440/7`, com ganho de `180/7`. Previsto = `600 − 180 = 420`. Vão gozado = `180`, suprimido = `0`, limite = `min(420, 480) + 0 = 420`. Excedente = `47/7 = 6,714285… min`, dentro dos 10. **HE zero**. Incluir apenas o ganho noturno da cauda elevaria o excedente a `107/7 = 15,285714… min`, integralmente em HE50. A exceção da cl. 38-d é do 12x36. `test_ramo2b_c2_base_da_he.py:64–65,100–104,378–403` · `motor_calculo_v2.py:44–51,1993–2011,3237–3252,3375–3404` · `LEIS.md:84` · `regua_cct.py:467–472` |
| **12x36 e Comercial** | 19:07:19 → 07:36:39 = `44960/60 = 749,3333333333334 min` trabalhados. Até 07:00 são `42761/60 = 712,6833333333333 min` de base. Os `36,65 min` da cauda entram no trabalhado e **fora = 0**. Subtrair em ponto flutuante dá `712,6833333333334`, um ULP acima da medição direta. O teste acerta ao exigir igualdade exata. `test_ramo2b_c2_base_da_he.py:437–463` · `motor_calculo_v2.py:1918–1920,2985–3028,3473–3517` |

## 2. Caminho do novo teste [nao verificado]

O teste chama os `calcular_periodo` reais e percorre o recorte, a medição e a gravação da base. O patch entrega pronta a parte que o mensal precisa resolver: pareamento e intervalos, dia do turno, marcos do vínculo, folga, vigência, L-084, extra declarada e decisão do admin. Um marco de D+1 errado na janela real ficaria invisível nesse teste. `test_ramo2b_c2_base_da_he.py:443–463` · `fechamento.py:297–311` · `motor_calculo_v2.py:1682–1722,1742–1810,2500–2511,2560–2568,2661–2675` · `janela_he.py:41–61` [nao verificado]

Há duas diferenças relevantes. O 12x36 com horário cadastrado usa `Motor12x36ComEscala`, que chama o mesmo cálculo e depois relê a entrada efetiva para pontualidade. O Comercial mensal zera a HE feita por par e a apura de novo por dia, usando o previsto do dia da jornada quando cruza. O teste também injeta `660` sem fornecer o intervalo descontado do previsto. Ele prova os três números da asserção. A mesma igualdade bit a bit pelo mensal fica **[nao verificado]**. Os dois testes reescritos do Partido já chamam `calcular_mes`. `motor_calculo_v2.py:1464–1492,3580–3602,3672–3685,3810–3827,3927–3928` · `test_ramo2b_c2_base_da_he.py:160–176,455–463` [nao verificado]

## 3. Escritores e leitores

| Sítio | O que faz |
|---|---|
| **Motor** | Os três `calcular_periodo` gravam trabalhado e fora. A tripla da base é gravada por `_escrever_base_da_he`. No Partido recortado, a saída efetiva já é o marco, a tripla fica vazia e `_trabalhado_da_base_da_he` devolve o trabalhado. No 12x36 e Comercial com cauda incorporada, o leitor devolve a base explícita. O Comercial mensal usa a base para HE e o trabalhado integral para banco. O crédito do **dia aberto** e o bloqueio de HE100 também ajustam os campos pela regra do motor. `motor_calculo_v2.py:286–299,1628–1635,1941–1971,2768–2786,3025–3028,3141–3146,3193–3195,3382–3393,3514–3519,3591–3594` |
| **`dia_pago.py`** | Soma trabalhado, noturno e HE já calculados, pelo dia do mapa quando fornecido. A cauda excluída do trabalhado do Partido fica fora dessa soma. `dia_pago.py:126–132,173–186` |
| **`periodo.py::reler`** | Mede e **reescreve** trabalhado pela geometria efetiva. Lê a tripla para HE e dobra, e soma trabalhado com fora para o teto de pontualidade. No caminho de par duplicado, zera também `minutos_fora_janela`. A geometria efetiva do Partido já chega cortada. `periodo.py:182–187,344–366,743–769,807–813,843–859` |
| **`folha/porta_export.py`** | Compara o trabalhado com a lavratura e lê a base pela mesma função do motor. A pendência vem do espelho. O teto de conferência quando há HE **e** intra usa base de relógio e previsto líquido. Ele não reproduz o ganho noturno nem `min(previsto, 480)` do Partido `legais`. `porta_export.py:106–122,181–201,342–365` · `motor_calculo_v2.py:3382–3404` |
| **`credito_parcial.py`** | Fornece ausências parciais e créditos. Dos períodos, lê atraso e agrupa pela data civil da entrada, que pode divergir do dia da jornada. O teto do motor usa `trabalhado + fora`, então a cauda do Partido continua contando como presença para a L-093. `credito_parcial.py:35–50,62–102` · `motor_calculo_v2.py:1319–1321,1356–1357` |

**Há um leitor que usa a ponta errada no Partido.** Quando a tripla está vazia, `_aplicar_he_noturna` usa a saída **bruta**. Rodei essa função em memória com 01:00 → 05:30, saída efetiva às 04:00, HE50 de `180/7` e prorrogação desligada. Com a tripla vazia, saiu **0 min de HE50 noturna**. Com o fim da base às 04:00, saiu **25,7142857 min**. O teste `PARTIDO_LEGAIS` espera HE zero, então não alcança essa divergência. Impacto em produção **[nao verificado]**. `motor_calculo_v2.py:464–485,1094–1109,1967–1971,3131–3146` · `test_ramo2b_c2_base_da_he.py:396–399`

## 4. Teste que contradiz [nao verificado]

**`test_RED_partido_cruza_sem_decisao` ainda exige a cauda dentro.** Para P2 de 03:00 a 07:30, com marco às 07:00, exige saída efetiva **07:30**, **270 trabalhados**, **0 fora** e **270 noturnos**. Pelo ramo da cópia, a expectativa é **07:00**, **240 trabalhados**, **30 fora** e **240 noturnos** com a prorrogação dessa fixture. O vizinho `Autorizado` continua coerente ao exigir os 270. O caso dos 11 minutos também é controle distinto, pois o juiz põe o par em D+1, fora da janela do turno anterior. A execução da suíte fica **[nao verificado]**. [`test_ramo2b_turno_que_cruza.py:141`](/home/ronald/codex_ws/c19bf0db27a334753ab8fe9cf17fad0b5c9b585e/app/ponto/tests/test_ramo2b_turno_que_cruza.py:141) · `test_ramo2b_turno_que_cruza.py:227–230,279–283` · `test_ramo2b_c2_base_da_he.py:425–434` · `motor_calculo_v2.py:1900–1920,3106,3141–3146`

## 5. Checklist

- **Segundo juiz e derivação paralela.** O delta só muda testes. Na cópia, `reler` rederiva o trabalhado e a versão oráculo do fechamento mede a base novamente pelos segmentos efetivos. O patch do novo teste fornece a resposta da janela. `test_ramo2b_c2_base_da_he.py:437–463` · `periodo.py:753–769` · `alimentacao.py:168–178,469–474,545–548` · `fechamento.py:1289–1305` [nao verificado]
- **Segundo escritor.** A varredura AST executada em leitura encontrou só `_escrever_base_da_he` para a tripla. Para trabalhado e fora há as reescritas do motor e de `reler` descritas acima. `test_ramo2b_c2_base_da_he.py:644–677` · `motor_calculo_v2.py:1954–1958` · `periodo.py:743–767`
- **Relógio solto.** Os testes mensais do Partido têm `freeze_time`. O teste direto usa instantes fixos, janela injetada e feriados vazios. `test_ramo2b_c2_base_da_he.py:124–125,437–458` [nao verificado]
- **Selo com mordida.** O anexo mostra **5/5 mutantes mortos**, com oito falhas de asserção. O resultado da suíte limpa e o efeito nas competências 11/12 e no TXT ficam **[nao verificado]**. `mut4.log:6,29,51,61,75,84,93,104` [nao verificado]
- **Campo com dois significados.** `minutos_trabalhados` é o contado após a janela. A presença para o teto soma trabalhado e fora. Uma tripla `None` significa que a base numérica coincide com o trabalhado, inclusive quando houve cauda física recortada. O comentário “par sem cauda” é impreciso nesse caso e a leitura da saída bruta no split noturno mostra a consequência. `motor_calculo_v2.py:228–246,286–290,1096–1105,1319–1321,1963–1969`

## Veredito — REPROVADO

1. O selo mensal legado do Partido sem decisão exige o oposto da c2p para a mesma regra de saída. `test_ramo2b_turno_que_cruza.py:144–148,279–280` · `test_ramo2b_c2_base_da_he.py:222–236`
2. O split de HE noturna do Partido pode usar a cauda excluída para posicionar HE devida antes do marco. A c2p cobre o ganho na base, mas o caso com HE positiva segue sem selo nessa volta. `motor_calculo_v2.py:1094–1109,1967–1971` · `test_ramo2b_c2_base_da_he.py:378–403`
3. A c2p apresenta o destino da cauda como conclusão direta do aval. O texto sustenta preservar o Partido, mas a equivalência com o comportamento de `59600163c` permanece **[nao verificado]**. `PROMPTS.md:3087` · `LEIS.md:115` · `test_ramo2b_c2_base_da_he.py:4–6`

## 27/11 05:56 · sessao 527ddadb4b26 · RAMO2-B-EM-TURNO-QUE-CRUZA · construtor · origem=Code

Corrigi as expectativas dos dois casos do Partido e acrescentei um RED com segundos para o 12x36 e o Comercial. O `diff --stat` registra apenas o arquivo de testes autorizado. [Testes revisados](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2_base_da_he.py:222) [nao verificado]

| Caso | Antes | Depois |
|---|---|---|
| `SEMANA_FLOAT`, Partido | Trabalhava `34160/60 = 569,333…` min, com base de `31961/60 = 532,683…` min. | Trabalhado e base medem `05:00:00 − 20:07:19 = 31961/60` min. Fora da janela ficam `(34160 − 31961)/60 = 36,65` min, medidos de `05:00:00` a `05:36:39`. [Asserção](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2_base_da_he.py:222) |
| `PARTIDO_LEGAIS`, 6x1 | Esperava `(221, 240)` trabalhados, 240 noturnos de relógio no P2 e HE zero. | Trabalhado `(221, 180)`, com 60 min fora no P2. A cl. 38-d é do 12x36. No Partido, 180 min noturnos dão `180 × 60/52,5 = 1440/7` min legais, com ganho de `180/7`. O vão gozado de 180 min dá suprimido zero. O excedente é `221 + 180 + 180/7 − min(420, 480) = 47/7` min, dentro da tolerância de 10, portanto **HE zero**. [Conta e asserções](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2_base_da_he.py:378) · [Cl. 38-d](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/docs/LEIS.md:84) |
| 12x36 e Comercial que cruzam | A asserção de trabalhado bruto estava no caso do Partido. | O novo RED usa os marcos da fixture de 19h–07h e injeta a janela. A batida `19:07:19 → 07:36:39` dá `44960/60` min trabalhados, zero fora e base de `42761/60` min. Reconstruir a base por `44960/60 − 2199/60` dá um float diferente nos dois motores. [RED](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2_base_da_he.py:438) |

A sonda Python sem banco reproduziu as duas falhas antigas. Com cadastro e fuso fornecidos em memória, os três casos revisados passaram e os cinco mutantes abaixo falharam por asserção. Execução com as fixtures Django e certificação pelo Code e pelo Opus final [nao verificado]. [Partido semanal](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2_base_da_he.py:222) · [Partido legais](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2_base_da_he.py:378) · [12x36 e Comercial](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2_base_da_he.py:438)

```text
$ git -C /home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946 diff --stat
 app/ponto/tests/test_ramo2b_c2_base_da_he.py | 87 ++++++++++++++++++++--------
 1 file changed, 63 insertions(+), 24 deletions(-)
```

**Recorte para a sessão**, a partir de `app` e com `DJANGO_SETTINGS_MODULE` apontando para as configurações de teste. [Entrada do Django](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/manage.py:7) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  --settings="${DJANGO_SETTINGS_MODULE:?defina o settings de teste da sessao}" \ [nao verificado]
  ponto.tests.test_ramo2b_c2_base_da_he \
  ponto.tests.test_ramo2b_c2l_red_motor \
  ponto.tests.test_ramo2b_c2h_saida
```

**Comandos dos mutantes**, na mesma sessão. As substituições usam o executor em memória da cópia. [Executor](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/ramo2b_c2l_mutantes.py:12)

```bash
muta_c2p() {
  DJANGO_SETTINGS_MODULE="${DJANGO_SETTINGS_MODULE:?defina o settings de teste da sessao}" \ [nao verificado]
  PYTHONDONTWRITEBYTECODE=1 python3 - "$1" <<'PY'
import sys
from unittest.mock import patch

import django
from django.conf import settings
from django.test.utils import get_runner

django.setup()

from ponto import motor_calculo_v2 as m
from ponto.tests.ramo2b_c2l_mutantes import _mutar

raiz = 'ponto.tests.test_ramo2b_c2_base_da_he.'
partido = raiz + 'Ramo2bC2BaseDaHeTest.'
semana = partido + 'test_RED_partido_semana_float_trabalha_ate_o_marco_e_classifica_a_cauda_fora'
legais = partido + 'test_RED_partido_legais_ganho_noturno_da_cauda_fora_da_base'
cruza = raiz + 'BaseDaHeMedidaNoMarcoTest.test_RED_12x36_e_comercial_trabalham_o_bruto_e_medem_a_base_no_marco'
campo = 'CAUDA_DO_TURNO_QUE_CRUZA_NO_TRABALHADO'
ganho = m._ganho_noturno_da_base_da_he

variantes = {
    'partido_inclui_cauda': (patch.object(m.MotorTurnoPartido, campo, True), [semana, legais]),
    'cruza_corta_cauda': (patch.object(m.MotorBase, campo, False), [cruza]),
    'base_por_subtracao': (patch.object(m.MotorBase, '_gravar_base_da_he', _mutar(
        m.MotorBase._gravar_base_da_he, 'periodo, base_min, s_he, float(_leg)',
        'periodo, periodo.minutos_trabalhados - (s - s_he).total_seconds() / 60, s_he, float(_leg)',
        metodo=True)), [cruza]),
    'partido_noturno_bruto': (patch.object(m.MotorTurnoPartido, 'calcular_periodo', _mutar(
        m.MotorTurnoPartido.calcular_periodo, 'self._escrever_noturnas(periodo, e, s, intervalos)',
        'self._escrever_noturnas(periodo, e, periodo.saida, intervalos)', metodo=True)), [legais]),
    'partido_he_inclui_ganho_da_cauda': (patch.object(
        m, '_ganho_noturno_da_base_da_he',
        side_effect=lambda p: ganho(p) + p.minutos_fora_janela / 7), [legais]),
}

mutacao, alvos = variantes[sys.argv[1]]
with mutacao:
    falhas = get_runner(settings)(verbosity=2).run_tests(alvos)
raise SystemExit(int(bool(falhas)))
PY
}

muta_c2p partido_inclui_cauda
muta_c2p cruza_corta_cauda
muta_c2p base_por_subtracao
muta_c2p partido_noturno_bruto
muta_c2p partido_he_inclui_ganho_da_cauda
```

## 27/11 05:43 · sessao 68521b7293d6 · CODEX-CONSTROI-MOTOR · verificador · origem=Code

# REPROVADO

A c12 substitui a lista manual por uma regra geral que protege os testes **existentes na base**. A regra impede as edições apontadas no placar e no diagrama, mas deixa leitores operacionais descobrirem, lerem e executarem testes novos. O cruzamento com o brief comprova presença textual do caminho. A autorização do caminho pelo brief e o `!` do motor continuam sem vínculo executável demonstrado. `bin/codex_pergunta.sh:232–241,555–565` · `bin/regua.sh:138–145,165–169` · `bin/tests/test_relato_guarda_pedido_de_patch.sh:143–163` · `CLAUDE.md:145–146,274–277`

## 1. Achados da c11 e situação na c12

| Achado | Classificação e resultado |
|---|---|
| Edição do BUG116, do A16 e dos testes reservados | **Procedente. Cura de classe para testes presentes na base.** Saiu `RESERVADOS`. A mesma guarda recusa a edição ou remoção de qualquer teste existente que caiba na forma permitida. Dos sete nomes antigos, seis continuam recusados por existência. O sétimo, o antigo teste da cerca, pode nascer de novo. A matriz trocou esse caso por `bin/cerca_schema.py`, que já cai nas proibições de host. `bin/codex_pergunta.sh:167–177,236–241` · `bin/tests/test_codex_lista_positiva.sh:97–100,108–109,125–137,151–152` · `mapa_delta_c12.md:2–10` |
| Seleção dinâmica pelo vigia e pela régua | **Procedente. Classe aberta.** A decisão do wrapper usa presença na base, enquanto os consumidores usam a árvore que montaram para rodar e os IDs encontrados nas saídas. O caminho antigo do teste padrão de reversão fica protegido. A seleção por ID continua aberta a testes novos. A interpolação de IDs em `sh -c` e a execução direta de Docker no reteste também permanecem como achados independentes. `bin/codex_pergunta.sh:236–241` · `app/core/esteira_vigia.py:725–762,1301–1317,1352–1363` · `bin/vigia_arvore.sh:75–88` |
| Flag escolhida pelo chamador, vínculo com o brief e `!` do motor | **Procedente. Cura de classe para menção literal, parcial para autorização.** Cada flag é checada pela mesma regra. A escolha do modo continua no argv, e a presença do caminho em texto não comprova o `!`. A referência de L-126 aos testes apagados continua como questão de desenho prevista no brief. `bin/codex_pergunta.sh:89–100,498,555–565` · `app/docs/LEIS.md:147` · `CLAUDE.md:145–146,274–277` · `brief_c12.md:18–19,30–31` |
| Matriz, helper e lacunas do mapa | **Procedente. Cura de classe no helper, cobertura parcial no mapa.** A mesma asserção agora cobra caminho e resultado em todos os chamadores de `_inv_recusa`. Permanecem, entre outros, a fixture de homônimo com `crons_pkg` em vez de `crons`, as lacunas para `base.py`, `signals.py`, `gunicorn.core.conf.py`, `quarentena_lista.py`, o artefato do diagrama e os casos cruzados com caminhos autorizados. A rodada `SO_CASOS` continua saindo com `3` quando tem zero falhas e termina antes dos mutantes. `bin/tests/_codex_construtor_cenario.sh:88–100` · `bin/tests/test_codex_lista_positiva.sh:85–142,165–190` · `app/core/management/commands/crons.py:23` · `bin/selo_reciclagem.sh:40–46` |
| Interseções de models e motor autorizados | **Procedente. Continuam abertas.** `tenants/models.py` nomeado continua alcançando o provisionamento por `auto_create_schema`. A migration histórica continua importando `colaboradores.models`. `core/models.py` continua na janela de auth. O controle do motor prova aceitação e `TOCADO`, sem consultar retorno ou carimbo do elo. Efeitos de alterações específicas em produção [nao verificado]. `bin/codex_pergunta.sh:234–241,555–563` · `app/tenants/models.py:14–21` · `app/tenants/services.py:1–13` · `app/colaboradores/migrations/0004_colaborador_data_demissao_alter_colaborador_cpf_and_more.py:3,25` · `bin/auth_sitios.txt:17` · `bin/tests/test_codex_lista_positiva.sh:165–177` · `bin/cron_run.sh:11–25` |
| `api`, deploy e custódia da cerca | **Procedente. Cura de origem herdada da c11.** O SaaS inclui `api`, a pergunta entrega os labels locais ao Django e M2 cobra código `1` com a operação esperada. A fonte fica em `bin/`, e o deploy publica conforme o retorno do primeiro `shell -c`. O portão de migrations pendentes consulta outra pergunta. A variante com `MIGRATION_MODULES` e as execuções efetivas em produção ficam [nao verificado]. `app/config/settings/saas.py:47–53` · `bin/cerca_schema.py:18–36` · `bin/deploy.sh:185–217` · `bin/tests/test_cerca_schema.sh:70–98` |
| Referências ao wrapper, item C | **Procedente. Cura parcial.** Conferi em memória o verificador atual e as sete substituições. A cópia passou e as sete falharam pela regra esperada. Seguem duas referências numéricas em `PENDENTE`, a extração em runtime pelo delimitador `PROIBIDOS = [` e a possibilidade de trocar uma citação por outra âncora válida sem acusar a troca de destino naquele sítio. `bin/tests/test_ref_codex_pergunta.sh:24–96,106–124` · `bin/retencao_disco.py:152–154` · `bin/tests/test_rc_sem_colher.sh:14` · `bin/disjuntor.py:2025–2041` |
| Pacote `models/` e despacho, item D | **Procedente. Cura de classe herdada da c11.** O wrapper define a regex, o disjuntor lê essa mesma regex e o selo mantém o caso de brief contendo apenas o pacote e o mutante que retira a tag. Em brief misto, um caminho elegível satisfaz o `any` da admissão do agente. `bin/codex_pergunta.sh:170–175` · `bin/disjuntor.py:1963,2025–2041,2104–2115` · `bin/tests/test_agente_no_modelo.sh:140–146,289–293,360–362` |
| Selos verdes, HUP e rodada em produção | **Procedente como pendência de evidência.** A régua chama os selos de host e o selo de HUP copia a cerca real para sua fixture. Os anexos trazem as matrizes. Os nove selos completos no hash, os logs dos REDs históricos, o Opus final e a execução em produção ficam [nao verificado]. `bin/regua.sh:138–145` · `bin/tests/test_deploy_reload_gracioso.sh:32–61,109–125` · `brief_c12.md:32–35` |

## 2. Contraexemplo de teste novo [nao verificado]

**Procedente.** Com `app/core/models.py` na flag e na pergunta, a criação de `app/core/tests/test_model_sem_migration_pendente.py` passa. O anexo registra `rc 0` e ausência na base. Conferi a ausência na cópia e reproduzi em memória o predicado do inventário com essa entrada ausente. Um `unittest.TestCase` comum nesse módulo entra na descoberta de `core` pela régua. O selo de host de RELATO já abre o arquivo pelo glob `app/*/tests/*.py`, antes mesmo de verificar seu conteúdo. Depois de incluído no `HEAD` da cópia, o mesmo arquivo entra como label em `suite.sh --volta`, cujo seletor lê o fonte por AST. `matriz_sem_flag.txt:7–10` · `bin/codex_pergunta.sh:164–166,203–209,236–241,555–565` · `bin/regua.sh:23,138–145,165–169` · `app/core/runner_quarentena.py:72–85` · `bin/tests/test_relato_guarda_pedido_de_patch.sh:143–163` · `bin/vizinhos_da_volta.py:25–45,92–95,116–129` · `bin/suite.sh:198–216,441–445`

- **Procedente para os dois leitores fixos na base dada.** O placar executa a classe do BUG116 por nome e seu contrato pula `tests/` na varredura. O bloco A16 abre um caminho fixo e lê duas constantes por AST. A regra impede editar os dois arquivos existentes. O antigo nome da cerca volta ao universo de testes, enquanto o deploy carrega o corpo de `bin/cerca_schema.py`. `bin/placar_code.sh:137–144` · `app/core/tests/test_bug116_credencial_deixa_trilha.py:156–170` · `app/core/management/commands/gerar_diagrama.py:132–157` · `bin/deploy.sh:194–209`
- **Procedente para o vigia, conforme a árvore.** Se um teste novo falhar na cura, `_isolar` já pode selecionar seu ID, mas o módulo está ausente de `orig/app` daquela fatia. O resultado do reteste ainda entra na classificação `fatia` ou `arvore`. Depois do pouso o módulo pode integrar o `orig` seguinte. O vigia da árvore também retesta IDs na árvore viva, e a reversão os usa se o módulo continuar presente na árvore pós-revert. Acionamento concreto em produção [nao verificado]. `bin/molde_fatia/rodar.sh:9–14,43–51` · `app/core/esteira_vigia.py:725–762,1301–1317,1352–1363` · `bin/vigia_arvore.sh:101–106,128–149`

## 3. Cruzamento flag × brief

**Procedente como checagem literal.** Para cada valor de `--lista-positiva`, o wrapper valida a forma, exige que `HASH:caminho` seja `blob` e executa `grep -qF -- "$_p" "$PERGUNTA_R"`. `PERGUNTA_R` é o `realpath` do argumento posicional `<arquivo_pergunta>`. Só `grep` com `rc 0` passa. A recusa sai antes do Codex, com diagnóstico textual e `CODEX_RESULTADO rc=2 motivo=PRE_CHECK`. `bin/codex_pergunta.sh:89–104,353–360,431–433,555–565,809–817`

**Procedente a lacuna de autorização.** Em memória, passaram `<!-- app/core/models.py -->`, `./app/core/models.py` e `app/core/models.py.bak`. O caminho literal pode ser prefixo de uma menção maior, estar em comentário ou aparecer numa instrução de proibição. Já uma menção só em outro anexo fica fora desse `grep`, embora o censo leia anexos e pergunta, e o prompt receba ambos. A validação também é uma leitura anterior ao hash de admissão. O hash posterior e a revalidação conferem o arquivo, enquanto a montagem do prompt relê o caminho e não guarda os bytes do `grep`. Alteração concorrente efetiva e argv da obra real [nao verificado]. `bin/codex_pergunta.sh:306–319,498,558–576,657–689,791–804` · `bin/disjuntor.py:2693–2729`

## 4. O que os selos provam

| Selo | Classificação e alcance |
|---|---|
| **F1 / F1-M** | **Procedente para o payload, duvidoso para execução pelo deploy.** `_f1` avalia as atribuições extraídas do deploy e executa o payload silencioso no shell Django do selo. Cobra `0` na casa e `1` com M1. A ausência da chamada deixa a definição e elimina o efeito esperado. A linha que despacha Docker é reconhecida por texto e fica fora do trecho executado pelo selo. F1 aceita qualquer `rc 1` no M1 e F1-M aceita qualquer falha de `_f1`. O M1 independente é mais específico porque cobra também a operação. `bin/tests/test_cerca_schema.sh:46–56,70–89,100–129` · `bin/deploy.sh:197–209` |
| **30 / 30-M** | **Procedente para estrutura, duvidoso para execução.** A cópia do deploy chega ao Docker falso. O AST exige `def` de topo e chamada direta posterior. Em memória, o predicado deu `0` para o payload atual, `1` com só a definição e `0` com saída antecipada antes da chamada. O falso devolve seu rc configurado independentemente do AST. O 30-M aceita contagem zero sem conferir `RC=0` nem que o payload mutado chegou à casca. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:97–111,134–152,283–295` |
| **F2** | **Procedente.** Saiu `\|\| true`. `grep` com `1` é verde, `0` acusa ocorrência e qualquer outro rc é vermelho. O alcance é textual, para a grafia buscada em `.py`. Prova de custódia por AST ou de todas as formas de definição [nao verificado]. `bin/tests/test_cerca_schema.sh:131–142` |
| **`_inv_recusa`** | **Procedente.** Cobra `PROIBIDO`, campo 2 igual ao caminho, motivo por substring no campo 3, rc, `.rc`, ausência de `.id` e prefixo `CODEX_RESULTADO rc=3 `. O primeiro mutante troca o caminho **esperado** pelo helper. O segundo apaga o marcador da saída. São provas do leitor da asserção. O marcador é aceito sem conferir `motivo=INVENTARIO` ou unicidade. `bin/tests/_codex_construtor_cenario.sh:88–100` · `bin/tests/test_codex_lista_positiva.sh:193–202` |
| **Sete entradas de `p_teste_existe`** | **Procedente para a guarda, com um limite.** São sete casos aplicados à mesma mutação. Sem a guarda, as seis entradas de edição perdem a recusa ou seu motivo. O caso de remoção ainda recusa por `pos_apagado` e morde pela ausência de `teste ja existe na base`. Assim, seu RED prova o motivo específico e a remoção já era recusada. O contador geral aceita qualquer `FALHA` emitida pelo caso. `bin/codex_pergunta.sh:236–241` · `bin/tests/test_codex_lista_positiva.sh:108–109,125–137,151–152,208–214,232–249` |
| **Mutante `pos_brief`** | **Procedente.** Troca `_recusa2` por `true` no sítio da guarda. A fixture usa caminho válido e existente com pergunta sem o literal. Cobra `rc 2`, diagnóstico, marcador e ausência da captura do Codex falso. Sua prova termina na ausência total da menção. `bin/tests/test_codex_lista_positiva.sh:62–68,159–164,221–249` · `bin/tests/_codex_construtor_cenario.sh:44–54,74–86` |

**Duvidoso como RED histórico.** F1-M e 30-M são mutações reconstruídas do deploy. O deploy já enviava a chamada na c11. A troca do caso `cerca_reds` por `cerca_bin` também muda a entrada e a autoridade sob teste. Esses resultados não substituem os logs `RED.*.0b3939987.log` pedidos pelo brief [nao verificado]. `bin/deploy.sh:194–209` · `bin/tests/test_cerca_schema.sh:120–129` · `bin/tests/test_deploy_portoes_antes_do_migrate.sh:287–295` · `bin/tests/test_codex_lista_positiva.sh:97–100` · `brief_c12.md:11–17,32–35`

## 5. Escritores, leitores e autoridades

| Dado ou artefato | Escritor, leitor e classificação |
|---|---|
| `HASH`, `arvore`, `pastas` | **Procedente.** O chamador fornece `REF`. O wrapper resolve o commit, exige `HEAD == HASH` e constrói `arvore` e `pastas` do `git ls-tree` da worktree ligada. O mesmo inventário usa esse mapa para comparar modo/blob, detectar remoção e classificar a existência do teste. `cat-file -t` consulta o mesmo commit para outra pergunta, a elegibilidade dos caminhos da flag. `bin/codex_pergunta.sh:100–117,186–225,236–241,555–568,791–797,835–840` |
| `POSITIVA`, `_POSRX`, `_POSV`, `POS`, `APPS`, `RESERVADOS` | **Procedente.** O chamador escreve as flags, Bash acumula, valida e serializa, o cabeçalho lê `_POSV` e o inventário final deriva `POS` e `APPS`. Já havia duas concessões no mesmo argumento, edição literal e autorização do app para testes. Sai o escritor da lista manual `RESERVADOS` e entra a consulta de existência no inventário. `bin/codex_pergunta.sh:89–95,161–171,236–241,555–565,670–676,835–836` |
| Pergunta, anexos, `_grc`, `insumos`, `CAB` | **Procedente.** O produtor da pergunta e dos anexos fornece arquivos ao wrapper. `grep` escreve o resultado em `_grc`, consumido pela validação. O censo lê pergunta e anexos e julga os briefs da obra. `_PROJ_PY` grava e confere os hashes. O wrapper monta `CAB`, e o executor o recebe como stdin. São leituras independentes de menção, censo, integridade e prompt. O produtor efetivo do brief da chamada real fica [nao verificado]. `bin/codex_pergunta.sh:89–104,306–319,498,558–576,657–691,798–817` · `bin/disjuntor.py:1872–1901,2693–2729` |
| `.inventario`, `.recusa`, `.rc`, `.id`, `CODEX_RESULTADO` | **Procedente.** O wrapper emite os artefatos. O helper agora lê o caminho e o motivo de `.recusa`, o `.rc`, a ausência de `.id` e o marcador em `$T/saida`. O coletor de `.rc` segue lendo o recibo do wrapper. O delta muda as asserções do selo, preservando a origem dos sinais. `bin/codex_pergunta.sh:353–360,591–608,767–773,835–860` · `bin/tests/_codex_construtor_cenario.sh:75–100` · `bin/rc_sem_colher.py:65–77` |
| Fixtures, `PQ`, `POS_DEF`, `RESV`, `MATRIZ`, `MATRIZ_BASE`, `SEM_FLAG`, `MUTANTES` | **Procedente.** O selo positivo escreve a pergunta de fixture e os casos. O cenário compartilhado lê `PQ` e passa o arquivo ao wrapper. `_pos` escreve o rc observado e o motivo esperado em `MATRIZ`, que o selo imprime e filtra para `MATRIZ_BASE`. `SEM_FLAG` recebe os rc do ramo comum. A tabela `MUTANTES` dirige alterações em cópias do wrapper e o selo lê os diagnósticos resultantes. O selo legado usa o mesmo helper. `bin/tests/test_codex_lista_positiva.sh:36–82,180–255` · `bin/tests/_codex_construtor_cenario.sh:1–11,28–36,74–100` · `bin/tests/test_codex_pergunta.sh:73–74,312–354,444–451` |
| Cerca, cópias de deploy, `payload_cerca.py`, `CERCA_EXECUTA`, `N_CERCA`, `dono`/`grc` e leitor de referências | **Procedente.** Deploy e selo leem `bin/cerca_schema.py`. F1 monta e roda seu payload, e 30 captura o último argumento do Docker falso em `payload_cerca.py`. O AST escreve `CERCA_EXECUTA` no log e o selo conta em `N_CERCA`. O rótulo passou a contar forma de chamada e sua mensagem ainda fala em origem do texto. F2 lê a saída e o rc do `grep`. O selo de referências lê os arquivos em bytes com decodificação `replace`. Há duas derivações de prova nos selos, F1 por execução e 30 por forma. Segundo juiz de drift ou segundo escritor de domínio acrescentado [nao verificado]. A pergunta continua delegada ao Django, conforme a regra de autoridade de `JUIZES.md`. `bin/deploy.sh:197–212` · `bin/cerca_schema.py:26–36` · `bin/tests/test_cerca_schema.sh:100–142` · `bin/tests/test_deploy_portoes_antes_do_migrate.sh:71–78,97–107,150–152,283–297` · `bin/tests/test_ref_codex_pergunta.sh:58–70` · `app/docs/JUIZES.md:7–15,29–34` · `CLAUDE.md:180–182` |

## 6. `except`, relógio e travas

- **Procedente.** No delta fornecido `c11 → c12`, contei **0 `except` acrescentados e 1 removido**. A remoção é o `except (UnicodeDecodeError, IsADirectoryError)` do selo de referências. O `except Exception` do inventário é anterior à c12. A contagem literal pedida para `1fc9ab682..HEAD` fica [nao verificado], pois a cópia veio sem metadados Git e o patch anexo começa na c11. `bin/tests/test_ref_codex_pergunta.sh:58–70` · `bin/codex_pergunta.sh:196–228` · `brief_c12.md:20–21`
- **Duvidoso como mudança de relógio ou inversão de trava.** O delta acrescenta execução de payload com timeout no selo e nenhuma consulta nova à hora. O `grep` entra com meta e lease já tomadas. O wrapper solta a meta antes de esperar slot e resposta, readquire para revalidar e solta antes do `GO`. O inventário final continua seguido da prova de vida da lease. `bin/tests/test_cerca_schema.sh:46–52,103–116` · `bin/codex_pergunta.sh:511–525,546–549,555–580,741–762,777–817,835–840`

## 7. Matriz sem flag

**Procedente como exposição anterior à c12.** O anexo registra **43/58** aceitos no universo c12 e **42/56** para o universo c11 reconstituído, incluindo o caso antigo medido à parte. A c12 alterou a guarda dentro de `if POS`. O modo comum segue pelas proibições gerais e pelas guardas específicas de motor e `models.py`. O mapa c10 já documentava `app/config/crons_duracao.json` como gravável pelo construtor comum. A exposição do JSON é consistente com a regra original documentada em L-126. A contagem exata e a árvore do commit imediatamente anterior ao CCM ficam [nao verificado]. `matriz_sem_flag.txt:1–11` · `mapa.md:28` · `bin/codex_pergunta.sh:171–184,232–242` · `bin/tests/test_codex_lista_positiva.sh:71–79,183–187` · `app/docs/LEIS.md:147`

**Procedente o alcance do JSON sobre a agenda.** `bin/crons_duracao.py` grava `dur_s`, `config/crons.py` incorpora os valores, e os horários de `sombra.sh` e `noturno.py` os consomem por `inicio_derivado`. A sonda comprova aceitação da fixture pelo wrapper. Pouso dessa alteração, crontab instalado e efeito operacional ficam [nao verificado]. `bin/crons_duracao.py:77–99` · `app/config/crons.py:146–160,245–278,350–352,594–595` · `bin/crons.sh:17–40` · `matriz_sem_flag.txt:7–10`

## 27/11 05:08 · sessao 96e803870767 · CODEX-CONSTROI-MOTOR · verificador · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 27/11 03:57 · sessao 0684ef753980 · E4D-CONTRATO-ROTA-B · construtor · origem=Code

# construtor.r

**Caso 30.** O cartão recente usa `DIA - timedelta(days=1)`, o que distingue a chave `pergunta_unica_por_marco` e preserva o motivo do alvo. [Fixture](chamados/tests/test_e4d_matriz_reds.py:1640) [Restrição](chamados/models.py:3944)  
A resposta recente continua um minuto depois da resposta do alvo. As asserções de SQL, API, modal e raio-x são as mesmas. [Asserções](chamados/tests/test_e4d_matriz_reds.py:1642)

Na base, a fila SQL exige `respondida_em` preenchido. A asserção esperada compara a fila que perde o alvo com `[p.pk, recente.pk]`. A falha após a correção fica **[nao verificado]** na suíte do Code. [Filtro SQL](chamados/juizes.py:1676) [Fila](chamados/juizes.py:1863) [Asserção](chamados/tests/test_e4d_matriz_reds.py:1647)

## Diff

| Arquivo | Inserções | Remoções |
|---|---:|---:|
| [e4d_matriz_mutantes.py](chamados/tests/e4d_matriz_mutantes.py:551) | 148 | 18 |
| [test_e4d_matriz_reds.py](chamados/tests/test_e4d_matriz_reds.py:1640) | 2 | 1 |
| **Total** | **150** | **19** |

`git diff --check` e a compilação em memória passaram. O diff acrescenta **0 `except`**. A comparação com a base confirmou que a única mudança nos REDs é a chamada da fixture do caso 30. [Fixture](chamados/tests/test_e4d_matriz_reds.py:1640) [Arnês](chamados/tests/e4d_matriz_mutantes.py:551)

## Seletores para o Code

O arnês expõe as **39 variantes da lista da fix5**, incluindo a da fixture. As outras 31 definições preexistentes ficaram fora do seletor desta volta. Cada classe roda controle, mutação com restauração registrada em `addCleanup` e novo controle. O conjunto de falhas é comparado por método e, no caso 30, por subteste de leitor. O ensaio sintético conferiu essa comparação e a restauração. As mordidas na suíte ficam **[nao verificado]**. [Arnês](chamados/tests/e4d_matriz_mutantes.py:551) [Classes](chamados/tests/e4d_matriz_mutantes.py:607) [Leitores](chamados/tests/e4d_matriz_mutantes.py:662)

RED do caso 30. [Teste](chamados/tests/test_e4d_matriz_reds.py:1617)

```bash
bin/suite.sh --only "chamados.tests.test_e4d_matriz_reds.MatrizRaizELeitoresTest.test_caso_30_sql_api_modal_e_raiox_leem_a_mesma_resposta"
```

Linha dos mutantes para `bin/suite.sh`. [Classes](chamados/tests/e4d_matriz_mutantes.py:607)

```bash
--only "chamados.tests.e4d_matriz_mutantes.M04ReverteOutraCompetencia chamados.tests.e4d_matriz_mutantes.M05SegundoEscritorM chamados.tests.e4d_matriz_mutantes.M06CarimboReescrito chamados.tests.e4d_matriz_mutantes.M07EscritorAstForaDaPorta chamados.tests.e4d_matriz_mutantes.M07FabricaConclusao chamados.tests.e4d_matriz_mutantes.M08RegravaExportada chamados.tests.e4d_matriz_mutantes.M09SegundoEscritorV chamados.tests.e4d_matriz_mutantes.M09MaterializaForaDaPorta chamados.tests.e4d_matriz_mutantes.M10SegundoEscritorR chamados.tests.e4d_matriz_mutantes.M10MaterializaForaDaPorta chamados.tests.e4d_matriz_mutantes.M11DescartaReaberturaVazia chamados.tests.e4d_matriz_mutantes.M11RetryGanhaNovoPedido chamados.tests.e4d_matriz_mutantes.M12AutorDaFotoSetNull chamados.tests.e4d_matriz_mutantes.M12OcultaAutoriaDesconhecida chamados.tests.e4d_matriz_mutantes.M18C6JulgaCompensacaoCedo chamados.tests.e4d_matriz_mutantes.M18I23ZeroFixo chamados.tests.e4d_matriz_mutantes.M18RegistrarAceitaCompensadorFuturo chamados.tests.e4d_matriz_mutantes.M20IgnoraRecusaNoEmpate chamados.tests.e4d_matriz_mutantes.M20ReusaPedidoDaAusencia chamados.tests.e4d_matriz_mutantes.M26DevolveSemVinculo chamados.tests.e4d_matriz_mutantes.M26PreservaSemEncaminhar chamados.tests.e4d_matriz_mutantes.M27JulgaInstanciaAntiga chamados.tests.e4d_matriz_mutantes.M27ReleSoACelula chamados.tests.e4d_matriz_mutantes.M27FantasmaForaDaPorta chamados.tests.e4d_matriz_mutantes.M28ViaSemCompensacao chamados.tests.e4d_matriz_mutantes.M28UsaPrimeiroEspelho chamados.tests.e4d_matriz_mutantes.M28ViaForaDaPorta chamados.tests.e4d_matriz_mutantes.M30FilaPeloCarimboCru chamados.tests.e4d_matriz_mutantes.M30ApiPeloParCru chamados.tests.e4d_matriz_mutantes.M30ModalPeloParCru chamados.tests.e4d_matriz_mutantes.M30RaioxPeloCarimboCru chamados.tests.e4d_matriz_mutantes.M31RaizForaDaPorta chamados.tests.e4d_matriz_mutantes.M31ChamadoForaDaPorta chamados.tests.e4d_matriz_mutantes.M32RaizForaDaPorta chamados.tests.e4d_matriz_mutantes.M32FilhaNasceComV chamados.tests.e4d_matriz_mutantes.M36IgnoraTranca chamados.tests.e4d_matriz_mutantes.M37DestranqueForaDaPorta chamados.tests.e4d_matriz_mutantes.M37RegravaRNoDestranque chamados.tests.e4d_matriz_mutantes.MFixtureOmiteEntradaCongelada"
```

## Censo de relógio

| Ocorrência | Cobertura |
|---|---|
| [`timezone.now()` nos REDs](chamados/tests/test_e4d_matriz_reds.py:185) | `freeze_time(T1)` no `setUp`. O teste isolado da fixture usa `freeze_time(T2)`. [SetUp](chamados/tests/test_e4d_matriz_reds.py:159) [Fixture](chamados/tests/test_e4d_matriz_reds.py:385) |
| [`timezone.now()` nos mutantes](chamados/tests/e4d_matriz_mutantes.py:130) | String de mutação que aponta para a leitura da fixture. [Fixture](chamados/tests/test_e4d_matriz_reds.py:185) |
| [`timezone.now()` nos mutantes](chamados/tests/e4d_matriz_mutantes.py:302) | Variante fora do seletor da fix5. Seu alvo, caso 16, herda `freeze_time(T1)`. [Caso 16](chamados/tests/test_e4d_matriz_reds.py:663) [SetUp](chamados/tests/test_e4d_matriz_reds.py:159) |
| [`timezone.now()` nos mutantes](chamados/tests/e4d_matriz_mutantes.py:334) | Caso 18 sob `freeze_time(T1)`, com `freeze_time(T2)` no controle que aceita o ato. [SetUp](chamados/tests/test_e4d_matriz_reds.py:159) [Controle T2](chamados/tests/test_e4d_matriz_reds.py:783) |
| [`timezone.now()` nos mutantes](chamados/tests/e4d_matriz_mutantes.py:475) | Variante fora do seletor da fix5. Seu alvo, caso 31, herda `freeze_time(T1)`. [Caso 31](chamados/tests/test_e4d_matriz_reds.py:1684) [SetUp](chamados/tests/test_e4d_matriz_reds.py:159) |
| [`timezone.now()` nos mutantes](chamados/tests/e4d_matriz_mutantes.py:496) | Caso 32 sob `freeze_time(T1)` do `setUp`. [Caso 32](chamados/tests/test_e4d_matriz_reds.py:1718) [SetUp](chamados/tests/test_e4d_matriz_reds.py:159) |
| `localdate()` e `date.today()` | Zero ocorrências nos dois arquivos. [REDs](chamados/tests/test_e4d_matriz_reds.py:1) [Mutantes](chamados/tests/e4d_matriz_mutantes.py:1) |

## 27/11 03:47 · sessao 501893001208 · E4D-CONTRATO-ROTA-B · construtor · origem=Code

O diff da fix5 está nos dois arquivos autorizados. A compilação e o `git diff --check` passaram. Em ensaios isolados, o selo AST rejeitou a escrita injetada do caso 07 e o teste da fixture rejeitou a omissão de `entrada_em`. A suíte Django e as mordidas em execução ficam **[nao verificado]**. [REDs](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1) [Mutantes](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/e4d_matriz_mutantes.py:1) [nao verificado]

## Diff e decisões

```bash
GIT_OPTIONAL_LOCKS=0 git diff -- \
  chamados/tests/test_e4d_matriz_reds.py \
  chamados/tests/e4d_matriz_mutantes.py
```

- O caso 14 e os casos 01/02/42/44/45 ficaram idênticos ao HEAD. `marco_do_estoque` também ficou intacto. Um assert incondicional ali anteciparia a falha dos casos 04/07/08/18 para a ausência da 0057 e esconderia os contratos desta volta. [Fixture de estoque](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:238) [Caso 14](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:644)
- Os REDs 04 e 08 propõem as interfaces `--dir-reversao`/`--reverter` e `--so-trilha-exportada`/`--aval`. O 04 usa um JSON por competência com foto e SHA-256. Essas interfaces e o formato do JSON são decisões do teste, com encaixe em produção **[nao verificado]**. O parser da base declara apenas `--aplicar`. [Comando atual](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/management/commands/backfill_decisoes_pergunta.py:455) [Reversão](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:459) [Exportada](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:540) [nao verificado]
- Os nomes `pedido_id`, `autoria_desconhecida`, `pergunta_sem_absorcao_encaminhada_admin` e `perguntas_sem_absorcao_na_lista_do_admin` são contratos propostos pelos REDs. O selo lê `PORTAS` e exige uma lápide com entidade, via, arquivo, função e motivo no próprio registro. O scanner cobre escritas com via literal e a observação SQL cobre as rotas exercitadas. A cobertura exaustiva de escritores **[nao verificado]**. [Pedido](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:570) [Autoria](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:628) [Admin](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1579) [Selo](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:305)
- O ensaio isolado revelou um `TypeError` ao desmontar a troca de `__code__` no runner. Corrigi a restauração em `finally` e conferi que a função original volta após o mutante. [Runner](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/e4d_matriz_mutantes.py:15)

## Caso → o que exige → mutante → morde?

`NV` significa **[nao verificado] na suíte**. A coluna registra o resultado observado, e os comandos abaixo identificam cada variante. [nao verificado]

| Caso | O que exige | Mutante | Morde? |
|---|---|---|---|
| 04 | Foto e hash próprios. Reverter B primeiro preserva A, o ato preexistente e a vizinha. [RED:459](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:459) | Reverte a outra competência junto | NV |
| 05 | `ABSORCAO` e `VALIDACAO` físicas antes de V/M, no mesmo atomic, pelas duas entradas. [RED:1395](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1395) | Segundo escritor de M no módulo e no alias | NV |
| 06/07 | Carimbo de célula pela exceção com lápide. Backfill preserva a foto e mantém zero ato novo e zero evento. [06:898](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:898) [07:526](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:526) | Reescreve carimbo, injeta escritor AST, fabrica conclusão | AST sim, com lápide sintética em memória. Suíte NV | [nao verificado]
| 08 | Via autorizada insere R e V na trilha. Hashes do TXT e do gravado iguais, zero escrita de coluna e zero evento. [RED:540](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:540) | Escreve uma coluna da exportada | NV |
| 09/10 | Ato físico precede a escrita de V/R. Resposta com texto, M pela exceção com lápide. [09:918](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:918) [10:959](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:959) | Segundo escritor de V/R ou M fora da porta | NV |
| 11/12 | Mesmo pedido preserva as linhas, pedido distinto grava por pergunta. FK protege e autoria histórica desconhecida tem marca explícita. [11:570](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:570) [12:608](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:608) | Descarta vazia, dá chave nova ao retry, volta `SET_NULL` ou omite a marca | NV |
| 18 | Compensador futuro restaurado, horizonte comum e recusa de ato futuro no `registrar`. Controles do I23 incluem `REABERTURA` e `RECUSA` com recuo do relógio. [Restore:701](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:701) [Registrar:766](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:766) | C6 antecipa, I23 zera fixo ou `registrar` aceita futuro | NV |
| 20 | Recusa para só Q, preserva irmãs e distingue dois pedidos na mesma ausência. [RED:1440](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1440) | Ignora empate ou reutiliza pedido da ausência | NV |
| 26 | Preserva R/V/M e DP. Registra Q e a apresenta na lista do admin. [RED:1579](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1579) | Devolve sem vínculo ou preserva sem encaminhar | NV |
| 27/28 | Releitura sob trava, com dia e célula atuais no fantasma. No espelho, resultado revogado preserva a pergunta, e H4 derrubado ganha `REABERTURA`. M exige lápide. [27:1126](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1126) [28:1167](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1167) | Instância antiga, só FK fresca, primeiro espelho, omissão da compensação ou escrita fora da porta | NV |
| 30 | Texto `09:00` em ambas as fontes. Instante do ato na ordem SQL, API, modal e crédito do raio-x. [RED:1617](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1617) | Um leitor por vez usa o instante cru | NV |
| 31/32 | Nascimento da raiz e do chamado pelas portas declaradas. Filha vazia na abertura direta e na emissão. [31:1683](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1683) [32:1717](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1717) | Escrita de mesmo valor fora da porta ou nascimento com V | NV |
| 36/37 | Consulta fresca vê a tranca e preserva a irmã de outra competência. Destranca preserva R e aplica M pela exceção com lápide. [36:1793](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1793) [37:1829](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1829) | Ignora tranca, regrava R ou destranca fora da porta | NV |
| Item 9 | `fato()` explicita `entrada_em` pelo relógio congelado quando o campo existe e preserva o instante dado pelo caller. [RED:377](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:377) | Omite a entrada e deixa o default agir | Sim, com relógio simulado em memória. Suíte NV | [nao verificado]

## Item 10 — conferência do verificador v2

| Caso | Veredito e motivo |
|---|---|
| 09 | **PROCEDENTE.** A rota carimba V sem registrar `VALIDACAO`. O recorte anterior só conferia o ato no primeiro ramo e deixava M sem selo. [Resolvedor:2118](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/services/disputa_emissao.py:2118) [RED:918](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:918) |
| 10 | **PROCEDENTE.** O ramo carimba R e emite `fecho_sem_credor`; esse log não é uma `RESPOSTA` física. A via de M também precisava de selo. [Resolvedor:2192](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/services/disputa_emissao.py:2192) [RED:959](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:959) |
| 11 | **PROCEDENTE.** A assinatura atual não recebe identidade do pedido e o filtro do lote exclui vazias. Uma chamada isolada não prova idempotência. [Porta:1628](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/services/disputa_emissao.py:1628) [Filtro:1695](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/services/disputa_emissao.py:1695) |
| 12 | **PROCEDENTE.** As quatro FKs já eram isoladas no teste. O modelo ainda usa `SET_NULL` e o teste anterior deixava passar autoria histórica nula sem marca. [Projeção:3804](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/models.py:3804) [Trilha:4347](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/models.py:4347) [RED:628](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:628) | [nao verificado]
| 20 | **PROCEDENTE.** A materialização reaproveita a ausência do mesmo tipo. Conferir só a FK da ausência deixa passar uma chave fixa para pedidos distintos. [Materialização:1037](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/services/materializacao.py:1037) [RED:1451](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1451) |
| 26 | **PROCEDENTE.** A devolução retorna cedo sem itens e reabre os selecionados pelo caminho atual. Só conferir a foto deixava passar a omissão do registro e da lista. [Materialização:1239](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/services/materializacao.py:1239) [RED:1579](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1579) |
| 27 | **PROCEDENTE como lacuna do teste.** O reconciliador relê a pergunta sob trava. A fixture anterior distinguia apenas a FK, enquanto o juiz também usa o dia. [Releitura:1265](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/reconciliador.py:1265) [Juiz:1182](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/reconciliador.py:1182) | [nao verificado]
| 28 | **PROCEDENTE.** `declaracao` fica fora de `SEM_HISTORIA`. O novo controle elegível com H4 chega à etapa que precisa de trava e compensação, e o espelho que muda de resposta exige releitura efetiva. [Seletor:31](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/management/commands/encerrar_perguntas_com_fato.py:31) [RED:1167](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1167) |
| 36 | **PROCEDENTE como lacuna do teste.** `_pv_memo` depende da instância e do instante. A consulta nova isola a regra da tranca. A causa dinâmica da falha da pista antiga fica **[nao verificado]**. [Memo:931](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/juizes.py:931) [RED:1793](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1793) | [nao verificado]

## Comandos para a pista do Code

O runner recebe a variante por `E4D_MUTANTE`. Os comandos usam a configuração CI de PostgreSQL e os seletores dos REDs. [Runner](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/e4d_matriz_mutantes.py:547) [Configuração CI](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/config/settings/ci.py:6)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --settings=config.settings.ci \
  chamados.tests.test_e4d_matriz_reds

m() {
  PYTHONDONTWRITEBYTECODE=1 E4D_MUTANTE="$1" python3 manage.py test \
    --settings=config.settings.ci \
    --testrunner=chamados.tests.e4d_matriz_mutantes.RunnerMutanteE4D \
    "chamados.tests.test_e4d_matriz_reds.$2"
}

h() { m "$1" "MatrizHistoricoTest.$2"; }
p() { m "$1" "MatrizPortasTest.$2"; }
a() { m "$1" "MatrizAusenciaTest.$2"; }
r() { m "$1" "MatrizRaizELeitoresTest.$2"; }

h 04_reverte_a_outra_competencia test_caso_04_aplica_fotos_separadas_por_competencia
a 05_segundo_escritor_m test_caso_05_absorcao_e_validacao_antes_de_v_m_no_mesmo_atomic
p 06_carimbo_reescrito test_caso_06_celula_absolve_so_com_carimbo
h 07_escritor_ast_fora_da_porta test_caso_07_backfill_celula_absolveu_fica_como_estava
h 07_fabrica_conclusao test_caso_07_backfill_celula_absolveu_fica_como_estava
h 08_regrava_exportada test_caso_08_exportada_veta_apply_comum_e_preserva_txt_e_gravado
p 09_segundo_escritor_v test_caso_09_resolvedor_validar_registra_antes_de_v
p 09_m_fora_da_porta test_caso_09_resolvedor_validar_registra_antes_de_v
p 10_segundo_escritor_r test_caso_10_resolvedor_responder_registra_so_com_texto
p 10_m_fora_da_porta test_caso_10_resolvedor_responder_registra_so_com_texto
h 11_descarta_reabertura_vazia test_caso_11_reabertura_vazia_grava_ato_sem_projetar
h 11_retry_ganha_novo_pedido test_caso_11_reabertura_vazia_grava_ato_sem_projetar
h 12_autor_da_foto_set_null test_caso_12_proteger_cada_referencia_de_autoria
h 12_oculta_autoria_desconhecida test_caso_12_backfill_marca_autoria_desconhecida
h 18_c6_julga_compensacao_cedo test_caso_18_estoque_futuro_compensa_no_horizonte_comum
h 18_i23_zero_fixo test_caso_18_estoque_futuro_compensa_no_horizonte_comum
h 18_registrar_aceita_compensador_futuro test_caso_18_registrar_recusa_compensador_futuro
a 20_ignora_recusa_no_empate test_caso_20_recusa_para_so_q_e_preserva_irma_ja_absorvida
a 20_reusa_pedido_da_ausencia test_caso_20_pedidos_distintos_na_mesma_ausencia
a 26_devolve_sem_vinculo test_caso_26_sem_absorcao_preserva_legado
a 26_preserva_sem_encaminhar test_caso_26_sem_absorcao_preserva_legado
p 27_julga_instancia_antiga test_caso_27_fantasma_trava_rele_julga_e_escreve
p 27_rele_so_a_celula test_caso_27_fantasma_trava_rele_julga_e_escreve
p 27_fantasma_fora_da_porta test_caso_27_fantasma_trava_rele_julga_e_escreve
p 28_via_sem_compensacao test_caso_28_espelho_trava_rele_e_reabre_ato_que_perde_h4
p 28_usa_o_primeiro_espelho test_caso_28_espelho_trava_rele_e_reabre_ato_que_perde_h4
p 28_via_fora_da_porta test_caso_28_espelho_trava_rele_e_reabre_ato_que_perde_h4
r 30_fila_pelo_carimbo_cru test_caso_30_sql_api_modal_e_raiox_leem_a_mesma_resposta
r 30_api_pelo_par_cru test_caso_30_sql_api_modal_e_raiox_leem_a_mesma_resposta
r 30_modal_pelo_par_cru test_caso_30_sql_api_modal_e_raiox_leem_a_mesma_resposta
r 30_raiox_pelo_carimbo_cru test_caso_30_sql_api_modal_e_raiox_leem_a_mesma_resposta
r 31_raiz_fora_da_porta test_caso_31_nascimento_da_raiz_e_do_chamado
r 31_chamado_fora_da_porta test_caso_31_nascimento_da_raiz_e_do_chamado
r 32_raiz_fora_da_porta test_caso_32_nascimento_da_filha_com_rvm_vazios
r 32_filha_nasce_com_v test_caso_32_nascimento_da_filha_com_rvm_vazios
r 36_ignora_tranca test_caso_36_tranca_retirada_da_fila_por_leitura
r 37_destranque_fora_da_porta test_caso_37_destranca_passivo_preserva_resposta
r 37_regrava_r_no_destranque test_caso_37_destranca_passivo_preserva_resposta
m fixture_omite_entrada_congelada FixtureMatrizTest.test_fato_explica_entrada_quando_o_campo_existe
```

## Perguntas de lei

1. **Caso 14.** Para o meio-par histórico de L-130, qual ato prevalece entre `SANEAMENTO` e `REABERTURA`? O teste continua congelado. `opus.md:19` · [RED:644](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:644) [nao verificado]
2. **Caso 18.** O zero para compensadores de restore deve sair no próprio I23 ou em um contador separado? O RED usa I23 e preserva o controle da `REABERTURA` legítima que fica adiantada após recuo do relógio. [I23 atual](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/management/commands/auditar_invariantes_chamados.py:1122) [Caracterização F5](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:319)
3. **Caso 28.** A presença de H4 vigente e de fato novo confirmado sob trava torna elegível uma pergunta com via `declaracao`? O RED preserva o cenário original e também exercita `legado_sem_lastro`, que já entra no seletor. [Seletor:36](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/management/commands/encerrar_perguntas_com_fato.py:36) [RED:1167](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1167)

## 27/11 03:46 · sessao d6cd1e928312 · E4C-F5-INSTANTE · construtor · origem=Code

Apliquei o diff nos dois arquivos autorizados. A compilação estática passou, e a comparação com o conteúdo inicial preservou os REDs nomeados e os skips. A suíte e as mordidas em PostgreSQL ficam para o Code. [test_e4c_f5_c4_red.py:492](chamados/tests/test_e4c_f5_c4_red.py:492), [e4c_f5_c4red2_mutantes.py:490](chamados/tests/e4c_f5_c4red2_mutantes.py:490). Execução em PostgreSQL [nao verificado].

1. **Escritores.** O campo declara `db_default=Now()`. O controle lê `statement_timestamp()` antes e depois de cada escritor, exige que `entrada_em` caia no intervalo inclusivo e mantém `entrada_em != MARCO`. M33 grava no marco e M34 grava um instante fixo anterior. A fonte instalada de `django/db/models/functions/datetime.py::Now.as_postgresql` estava indisponível nesta cópia, então a emissão SQL dessa instalação fica [nao verificado]. O comando abaixo imprime a fonte e a linha no ambiente do Code. [chamados/models.py:4406](chamados/models.py:4406), [requirements.txt:13](requirements.txt:13), [test_e4c_f5_c4_red.py:499](chamados/tests/test_e4c_f5_c4_red.py:499), [e4c_f5_c4red2_mutantes.py:623](chamados/tests/e4c_f5_c4red2_mutantes.py:623).
2. **Item 13.** `competencia_fechada` barra `aprovado` e `exportado`. A testemunha da F6 mostra `bloqueada_competencia`. A porta `validar_pergunta` devolve `(False, 'nao_gravou')`, conforme o teste já existente da F6, que também exige a trilha de devolução. O controle mantém o sucesso em FM aberto. Em FM exportado, exige a tupla exata, o motivo `COMPETENCIA_APROVADA` e a preservação das respostas, validações e carimbos do par, das batidas e do TXT. M32 injeta o retorno K6 `bloqueada_competencia_entregue`, com mordida esperada só em `fm='exportado'`. [ponto/janelas.py:87](ponto/janelas.py:87), [materializacao.py:943](chamados/services/materializacao.py:943), [validacao.py:869](chamados/services/validacao.py:869), [validacao.py:243](chamados/services/validacao.py:243), [test_bug_par_volta.py:240](chamados/tests/test_bug_par_volta.py:240), [test_e4c_f5_c4_red.py:902](chamados/tests/test_e4c_f5_c4_red.py:902), [e4c_f5_c4red2_mutantes.py:603](chamados/tests/e4c_f5_c4red2_mutantes.py:603).
3. **M21.** O anexo mostra as duas rodadas no conjunto observado e só o teste-pai no esperado. A cópia confirma que `vistos` é comparado dentro de `subTest`. Essa é a asserção certa, pois trocar `<=` por `<` deixa a linha empatada como `ato` nas duas rodadas. O conjunto esperado agora contém exatamente `rodada=1` e `rodada=2`. [0058_decisao_entrada_do_ato_historico.py:20](chamados/migrations/0058_decisao_entrada_do_ato_historico.py:20), [test_e4c_f5_c4_red.py:1062](chamados/tests/test_e4c_f5_c4_red.py:1062), [e4c_f5_c4red2_mutantes.py:490](chamados/tests/e4c_f5_c4red2_mutantes.py:490).

### Comandos para o Code

```bash
PYTHONDONTWRITEBYTECODE=1 python -c 'import inspect; from django.db.models.functions.datetime import Now; f = Now.as_postgresql; print(inspect.getsourcefile(f), inspect.getsourcelines(f)[1]); print(inspect.getsource(f))'

PYTHONDONTWRITEBYTECODE=1 python manage.py test chamados.tests.test_e4c_f5_c4_red

mut=chamados.tests.e4c_f5_c4red2_mutantes
PYTHONDONTWRITEBYTECODE=1 python manage.py test "$mut.M21Empate0058.test_mordida"
PYTHONDONTWRITEBYTECODE=1 python manage.py test "$mut.M32FMExportadoBarra.test_mordida"
PYTHONDONTWRITEBYTECODE=1 python manage.py test "$mut.M33EntradaNoMarco.test_mordida"
PYTHONDONTWRITEBYTECODE=1 python manage.py test "$mut.M34EntradaFixa.test_mordida"
```

### Diff

```diff
--- a/chamados/tests/test_e4c_f5_c4_red.py
+++ b/chamados/tests/test_e4c_f5_c4_red.py
@@ -500,7 +500,9 @@
                 p = self._historica(respondida_em=RESPOSTA_EM, resposta_colab='07:13')
                 with connection.cursor() as cursor:
-                    cursor.execute('SELECT transaction_timestamp()')
-                    instante_db, = cursor.fetchone()
-                self._escrever(p, escritor)
+                    cursor.execute('SELECT statement_timestamp()')
+                    antes_db, = cursor.fetchone()
+                    self._escrever(p, escritor)
+                    cursor.execute('SELECT statement_timestamp()')
+                    depois_db, = cursor.fetchone()
                 p.refresh_from_db()
                 atos = list(p.decisoes.order_by('ato_em', 'pk'))
@@ -517,5 +519,6 @@
                 self.assertEqual(D.divergencia_ato_x_coluna(p, MARCO), [])
                 self.assertNotEqual(novo.entrada_em, MARCO, 'ato novo entra fora do marco')
-                self.assertEqual(novo.entrada_em, instante_db, 'ato novo entra no instante da transação')
+                self.assertGreaterEqual(novo.entrada_em, antes_db, 'entrada do ato respeita o limite anterior à escrita')
+                self.assertLessEqual(novo.entrada_em, depois_db, 'entrada do ato respeita o limite posterior à escrita')
                 transaction.set_rollback(True)
 
@@ -898,20 +901,36 @@
 
     def test_item13_validacao_livre_com_txt_invalidado(self):
-        """O TXT invalidado sem substituto libera o par em FM aberto e exportado."""
+        """Sem TXT vigente, FM aberto libera o par e FM exportado aciona a F6."""
+        from chamados.catalogo.recusa import traduzir_recusa
+
         txt = self._entregar()
         with freeze_time(ITEM13 + HORA):
             txt.invalidar(motivo='Entrega cancelada na fixture', por=self.admin)
         self.assertFalse(competencia_entregue(self.empresa.pk, self.mes, self.ano))
-        batidas, arquivo_anterior, _ = self._gravado()
+        perguntas = (self.saida, self.volta)
+        campos_do_par = PAR + CARIMBO
+        par_antes = [valores(p, campos_do_par) for p in perguntas]
         for status in ('aberto', 'exportado'):
             with self.subTest(fm=status), transaction.atomic():
                 FechamentoMensal.objects.filter(pk=self.fm.pk).update(status=status)
+                gravado = self._gravado()
                 with freeze_time(ITEM13 + 2 * HORA):
                     r = validacao.validar_pergunta(self.saida.pk, self.admin, confirmar_implausivel=True)
-                self.assertTrue(r['ok'], 'TXT invalidado libera a validação')
-                self._assert_par_gravado()
-                self.assertEqual(self._gravado()[1], arquivo_anterior)
-                self.assertEqual(list(Batida.objects.filter(pk__in=self.chao).order_by('pk').values()), batidas)
-                for p in (self.saida, self.volta):
+                if status == 'aberto':
+                    batidas, arquivo_anterior, _ = gravado
+                    self.assertTrue(r['ok'], 'TXT invalidado libera a validação')
+                    self._assert_par_gravado()
+                    self.assertEqual(self._gravado()[1], arquivo_anterior)
+                    self.assertEqual(list(Batida.objects.filter(pk__in=self.chao).order_by('pk').values()), batidas)
+                else:
+                    self.assertEqual((r['ok'], r['estado']), (False, 'nao_gravou'),
+                                     'FM exportado sem TXT vigente recusa pela F6')
+                    for p in perguntas:
+                        p.refresh_from_db()
+                    self.assertEqual(traduzir_recusa(self.saida.recusa_motivo)[0], 'COMPETENCIA_APROVADA')
+                    self.assertEqual([valores(p, campos_do_par) for p in perguntas], par_antes,
+                                     'F6 preserva respostas, validações e carimbos do par')
+                    self.assertEqual(self._gravado(), gravado, 'F6 preserva batidas e TXT')
+                for p in perguntas:
                     self.assertEqual(D.divergencia_ato_x_coluna(p, None), [])
                 transaction.set_rollback(True)
--- a/chamados/tests/e4c_f5_c4red2_mutantes.py
+++ b/chamados/tests/e4c_f5_c4red2_mutantes.py
@@ -490,5 +490,6 @@
 M21Empate0058 = contraprova(
     'M21Empate0058', red.MarcoDa0058Test, 'test_0058_empate_e_reexecucao_sobre_update_ja_aplicado',
-    '0058 inclui a entrada no instante do marco', recorte_0058_estrito)
+    '0058 inclui a entrada no instante do marco', recorte_0058_estrito,
+    subcasos=(dict(rodada=1), dict(rodada=2)))
 
 M22SoValidacaoNaIrma = contraprova(
@@ -616,3 +617,27 @@
 M32FMExportadoBarra = contraprova(
     'M32FMExportadoBarra', red.Item13PorPKTest, 'test_item13_validacao_livre_com_txt_invalidado',
-    'TXT invalidado libera a validação', fm_exportado_barra_validacao, subcasos=(dict(fm='exportado'),))
+    'FM exportado sem TXT vigente recusa pela F6', fm_exportado_barra_validacao,
+    subcasos=(dict(fm='exportado'),))
+
+
+def entrada_em_fixa(instante):
+    bulk_create = D.objects.bulk_create
+
+    def gravar(linhas, *args, **kwargs):
+        linhas = list(linhas)
+        for linha in linhas:
+            if linha.entrada_origem == D.ATO_NOVO:
+                linha.entrada_em = instante
+        return bulk_create(linhas, *args, **kwargs)
+
+    return patch.object(D.objects, 'bulk_create', new=gravar)
+
+
+M33EntradaNoMarco = contraprova(
+    'M33EntradaNoMarco', red.EstoqueDoParTest, 'test_escritores_gravam_o_novo_ato_e_a_coluna_no_mesmo_instante',
+    'ato novo entra fora do marco', lambda: entrada_em_fixa(red.MARCO),
+    subcasos=tuple(dict(escritor=e) for e in ('carimbar', 'validar', 'responder')))
+M34EntradaFixa = contraprova(
+    'M34EntradaFixa', red.EstoqueDoParTest, 'test_escritores_gravam_o_novo_ato_e_a_coluna_no_mesmo_instante',
+    'entrada do ato respeita o limite anterior à escrita', lambda: entrada_em_fixa(red.MARCO - red.HORA),
+    subcasos=tuple(dict(escritor=e) for e in ('carimbar', 'validar', 'responder')))
```

## 27/11 03:44 · sessao aacf2bb78d5c · - · - · origem=Ronald (tmux)

```text
/bin/bash: line 4: /home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/INTRUSO_COPIA.txt: Read-only file system
/bin/bash: line 4: /home/ronald/saas-hasner/INTRUSO_VIVA.txt: Read-only file system
/bin/bash: line 4: /tmp/INTRUSO_TMP.txt: Read-only file system
```

## 27/11 03:44 · sessao 9dfe567edf08 · - · - · origem=Ronald (tmux)

ok

## 27/11 03:37 · sessao 55eda9ce28c0 · E4C-F5-INSTANTE · construtor · origem=Code

Apliquei a c4red5 nos dois arquivos de teste. O diff passa em `git diff --check`, os dois módulos compilam em memória e as asserções do item 10 são idênticas às do HEAD. O ensaio isolado do arnês aceitou o conjunto exato e rejeitou a falta de um subcaso. [REDs](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:301) · [Arnês](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:26) [nao verificado]

**PRONTO 9(a–c) [nao verificado].** Com a restrição de acesso ao banco, ficam pendentes o log dos REDs no HEAD, o verde da c1 e os ciclos verde → mordida → verde na pista do Code. [Configuração da pista](/home/ronald/wt-codex-e4c-f5-instante-012038/app/config/settings/ci.py:6) [nao verificado]

| Item | Teste | Mutante | Morde? | [nao verificado]
|---|---|---|---|
| 1 | Foto da recusa dentro das transações do [estoque](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:338) e do [worker](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:801) | [M30 × 2, M27](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:569) | [nao verificado] |
| 2 | PK e ato nos REDs de [fronteira, reprojeção e conversa](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:355). Mensagens integrais na [elegibilidade](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:434). | [M07, M09, M11](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:440) | [nao verificado] |
| 3 | Três escritores sem trava prévia da Empresa. O ato novo confere autor, motivo e instante da transação. [Controle vivo e skips A/B](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:464) | [M26](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:503) | Estacionado pela lei A |
| 4 | Dois PKs com B novo, um PK de estoque e skip da lei 1. [Comparador e leitores](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:613) | [M12a, M12b, M13](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:460) | [nao verificado] |
| 5 | Alarme e placar em subcasos próprios. [Asserções](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:673) | [M12a no contador, M12b nas duas saídas](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:460) | [nao verificado] |
| 6 | Identidade da candidata e instante futuro na coluna em memória. [Horizonte](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:233) | [M31](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:597) | [nao verificado] |
| 7 | [TXT invalidado com FM aberto e exportado](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:899) e [devolução via `validar_pergunta` após recusa por paridade](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:986) | [M32](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:616), [M29](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:535) | [nao verificado] |
| 8 | [Conjunto exato por `str(teste)`](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:51). A ordem das travas fica como [pré-condição da fixture](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:790). | [M07 exige dois subcasos, M11 exige sete](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:440). M14 e M25 removidos. | Conjunto exato conferido no ensaio isolado. Mordidas [nao verificado]. | [nao verificado]
| 9 | REDs, c1 e controles antes e depois de cada mutação. [Ciclo do arnês](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:46) | Comandos abaixo | [nao verificado] |
| 10 | Validação futura semeada como estoque restaurado. [Asserções da tarefa](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:301) | [M28](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:517) | [nao verificado] |

**Diff e comandos para a pista do Code**

```bash
git --no-optional-locks --no-pager diff -- \
  chamados/tests/test_e4c_f5_c4_red.py \
  chamados/tests/e4c_f5_c4red2_mutantes.py

export PYTHONDONTWRITEBYTECODE=1
python3 manage.py test --settings=config.settings.ci -v 2 \
  chamados.tests.test_e4c_f5_c4_red
python3 manage.py test --settings=config.settings.ci -v 2 \
  chamados.tests.test_e4c_f5_instante_c1

mordida() {
  python3 manage.py test --settings=config.settings.ci -v 2 \
    "chamados.tests.e4c_f5_c4red2_mutantes.$1.test_mordida"
}

# PRONTO 9(c), mais M32 no controle de TXT invalidado
for mutante in \
  M08QualquerResposta M11Elegibilidade M12aVigia M12bAlarme \
  M13Dry M21Empate0058 M24RelogioAposEspera M27K6LidoAntesDaTrava \
  M28CompensadorCru M29DevolucaoApagaResposta \
  M30EstoqueSemAtomic M30WorkerSemAtomic M31FotoRelida M32FMExportadoBarra
do
  mordida "$mutante"
done
```

Na cura, a mesma função recebe `M01DuasValidacoes`, `M02IgnoraResposta`, `M03FotoDaLavra`, `M04ValidacaoFutura`, `M07ValidacaoNoMarco`, `M09Conversa`, `M10Proveniencia`, `M15SoFM`, `M16TrilhaVazia`, `M17AlteraTXT`, `M18AlteraBatida`, `M19RecusaFutura`, `M20EmissaoDupla`, `M22SoValidacaoNaIrma` e `M23SoCandidata`. `mordida M26FotoViraAto` fica para depois da decisão da lei A. [Labels das contraprovas](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:425)

## 27/11 03:16 · sessao 196363bf4456 · E4D-CONTRATO-ROTA-B · verificador · origem=Code

A volta 2 ainda deixa passar uma implementação sem reversão por competência, sem a via autorizada da exportada e sem lista de exceções com lápides. Também aceita `REABERTURA` onde a regra exige `SANEAMENTO`. `app/chamados/tests/test_e4d_matriz_reds.py:328–400,438–455,655–671,1324–1362,1453–1474` · `app/docs/PROMPTS.md:2873,2950`

**Referências.** `T` = `app/chamados/tests/test_e4d_matriz_reds.py`. `M` = `app/chamados/tests/e4d_matriz_mutantes.py`. `C4` = `app/chamados/tests/test_e4c_f5_c4_red.py@48b24c39c`. `J` = `app/docs/JUIZES.md`. O cotejo das cópias mostrou diferenças somente em `T` e `M`. A cópia E4D é uma exportação sem `.git`, portanto a identidade do HEAD via Git fica [nao verificado]. Reexecução da pista, execução dos mutantes e dados de produção [nao verificado].

## 1. Procedentes da volta 1

### Os 14 motivos errados, um por um

| Caso | Situação e contraexemplo |
|---|---|
| **04** | **Curado mudando o que o caso pede.** Saiu o símbolo inventado e entrou o comando real, mas agora só se testa aplicação. Um apply que agrupe a prévia por competência e grave tudo sem foto, hash ou reversão por competência passa. `T:328–360` · `app/docs/PROMPTS.md:2873,2950` |
| **06** | **Curado mudando o que o caso pede.** Cobra M e zero ato. Uma escrita direta de M, uma vez por campo, sem entrada na lista e sem lápide passa. `T:655–671,232–236` · `app/docs/PROMPTS.md:2873` |
| **07** | **Curado mudando o que o caso pede.** Agora chama o comando efetivo e observa a ausência de ato, escrita e evento. O comando pode simplesmente ignorar a via e passar com a exceção ainda sem lápide. `T:362–373` · `app/docs/PROMPTS.md:2873` |
| **08** | **Curado mudando o que o caso pede.** A foto e os hashes vêm dos registros reais, mas o teste exige zero DP pela via comum. Uma implementação sem via autorizada passa, embora o caso peça deixar R/V/M, TXT e gravado intactos **e inserir a trilha** sob a autorização específica. `T:185–190,375–400` · `app/docs/PROMPTS.md:2873,2950` |
| **18 · contador** | **Curado quanto ao medidor.** A leitura agora é do I23 real, com controle positivo. Ainda passa uma implementação que isenta toda `REABERTURA` futura e conta `RECUSA` futura. A caracterização F5 exige contar uma `REABERTURA` futura após recuo do relógio. `T:499–503,536–543` · `app/chamados/management/commands/auditar_invariantes_chamados.py:1122–1134` · `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:319–325` |
| **21** | **Curado quanto à fixture.** O carimbo interposto passa pela trava real. O controle de sucesso ainda muda o motivo para `intervalo_volta`. Uma recusa que devolve sempre `ja_validada` para `orfao_14h`, após adquirir a trava, passa. `T:123,650–653,800–815` · `app/chamados/tests/_ato_da_fixture.py:16–27,47–51` |
| **23** | **Curado quanto à fixture e ao alvo do patch.** O alvo agora é o módulo usado pelo import local. Uma correção que recusa sempre `orfao_14h` depois da trava e aceita `orfao_22h` passa. A validação do controle positivo é simulada. `T:123,843–863` · `app/chamados/services/acoes_disputa.py:373–376` |
| **28** | **Não curado.** O controle `legado_sem_lastro` entra. A pergunta principal em `declaracao` segue excluída pelo seletor, antes da releitura e da compensação. Corrigir trava e releitura para todo item admitido ainda deixa o RED cair em `fato_materializado`. Alargar o seletor pode deixar passar uma decisão tomada pelo primeiro espelho, pois o mock responde `True` antes e depois da trava. `T:902–943` · `app/chamados/management/commands/encerrar_perguntas_com_fato.py:31–44,66–77` |
| **30 · SQL** | **Curado mudando o que o caso pede.** Foi retirada a consulta artificial, mas a fila real só é conferida por PK. Usar o ato para admitir P e devolver ou ordenar pelas colunas cruas passa. A fixture também mudou de dois pares completos divergentes para `NULL/08:00` contra `10:05/09:00`. `T:1288–1300` · `app/chamados/juizes.py:1863–1868` |
| **30 · modal** | **Curado quanto à fixture.** O modal real chega ao par cru. Ainda passa um leitor que usa a DP somente quando a coluna de instante está nula. Ele falharia no par completo divergente pedido pelo caso. `T:208–220,1288–1293,1310–1311` · `app/templates/chamados/partials/modal_fio.html:217–220` · `app/docs/PROMPTS.md:2873` |
| **31** | **Curado mudando o que o caso pede.** Cobre os nascimentos automático e manual, mas perdeu o selo. Um segundo escritor de D ou do chamado que grave os mesmos valores, fora da lista, passa. `T:1324–1351` · `app/docs/PROMPTS.md:2873,2950` |
| **32** | **Curado mudando o que o caso pede.** A cobrança indevida de `tipo_resposta` na lista saiu. A abertura direta prova R/V/M vazios e zero DP nessa porta. A emissão pode contornar `P.abrir` e nascer com V sem derrubar o 32. O selo dos escritores de D e chamado também saiu do 31. `T:1324–1362` · `app/docs/PROMPTS.md:2873,2950` |
| **37** | **Curado mudando o que o caso pede.** Preserva R, observa M e protege a vizinha. Um descarimbo que faça as mesmas alterações fora da exceção listada passa. `T:1453–1474` · `app/docs/PROMPTS.md:2873,2950` |
| **42** | **Curado mudando o que o caso pede.** Saiu `marcar_estoque` e entrou uma resposta real que muda a projeção. Ficaram de fora a migração, a marca, a hora real da entrada nova e a reconstrução do que se sabia em T1. Congelar o marco no valor da primeira resposta passa sem guardar instante de entrada algum. `T:545–564` · `C4:948–979` · `app/docs/PROMPTS.md:2974` |

### Mordida, guardas e caso 20

- **Escritor único e lápides — não curado.** O vínculo entre `INSERT`, ato e PK melhorou. O 05 também compara o savepoint de V/M com o dos atos. A presença de `registrar` na pilha ainda aceita dois escritores chamados por ele, e o observador perde um `UPDATE` que alcance P por outro predicado que não `id =/IN`. No sentido inverso, um projetor único chamado depois de `registrar`, dentro do mesmo atomic, falha pela forma da pilha. O censo estático devolveu um `bulk_create` atribuído a DP, mas descarta os achados sem entidade. Por exemplo, a chamada genérica `Model.save(pergunta.decisoes.model(...))` escapa desse filtro. Nenhum desses mecanismos recupera as lápides retiradas. `T:72–109,222–236,488–493,1123–1134` · `app/core/censo_escritas.py:364–430,469–491` · `app/chamados/models.py:4609`
- **Mutantes — não curado.** No 05, `M` troca o atributo de `materializacao`, enquanto a resposta chama o alias já importado em `disputa_emissao`. No 30/modal, `M` instala uma função de dois parâmetros e `T` chama com `request=req`. O primeiro pode deixar a escrita extra de fora. O segundo produz `TypeError` antes de comparar o par. No 31, fechar D após nascer prova a asserção de estado, mas nada diz sobre um escritor fora da lista que preserve esse estado. Mordida em execução [nao verificado]. `M:89–99,124–143,303–315,328–332` · `app/chamados/services/disputa_emissao.py:12,1596` · `T:215,1075–1078,1119–1120,1327–1334`
- **20 — curado quanto à referência e ao empate.** A docstring nomeia a caracterização invertida, o teste antigo permanece e a nova matriz verifica empate e posterioridade. A identidade do pedido saiu. Reusar uma chave fixa por ausência passa mesmo que dois pedidos distintos devam ser separados. `T:1144–1189` · `app/chamados/tests/test_e4c_f5_h4_resposta_apos_absorcao.py:364–380` · `app/docs/PROMPTS.md:2950`

| Guarda | Situação da lacuna da volta 1 |
|---|---|
| **13** | **Curada.** O snapshot integral de DP pega a alteração de texto ou motivo que escapava ao teste por tipo e autor. `T:181–183,731–748` | [nao verificado]
| **15** | **Parcial.** O controle aberto elimina a recusa incondicional. A entrega é interposta antes da aquisição da trava, então uma releitura feita após a interposição e ainda fora da trava pode passar. `T:239–252,750–776` |
| **19** | **Curada para o contraexemplo anterior.** A automática que sempre recusa agora falha no controle em branco. Uma guarda especializada no motivo do caso principal ainda passa, pois o controle usa outro motivo. `T:123,778–798` |
| **21, 23** | **Parciais.** Fixtures reparadas e caminhos positivos adicionados, com os escapes por motivo descritos acima. `T:650–653,800–815,843–863` |
| **22** | **Curada para o contraexemplo anterior.** O controle do mesmo motivo precisa validar e plantar. Como ele usa outro dia, uma recusa fixa para o dia inicial ainda passa. `T:123,817–841` |
| **24** | **Curada quanto à ordem e ao carimbo de E.** A ordem literal saiu e a foto de E passou a ser exigida. Ainda se exige exatamente o conjunto `{E,Q}`. Uma passagem que preserve E e produza os mesmos efeitos sobre Q, mas reporte só Q na lista, falha. `T:1191–1218` | [nao verificado]
| **27** | **Curada para a instância antiga.** A FK muda antes da trava, o juiz simulado decide por essa FK e o novo mutante conserva a releitura real, mas entrega a instância antiga ao juiz. Ainda se pode usar uma releitura atualizada só da FK e julgar outros campos obsoletos. A lápide de `fantasma` segue sem selo. `T:865–900` · `M:65–69` · `app/chamados/reconciliador.py:1265–1298` |
| **36** | **Parcial.** A irmã viva em outra competência e o pai aberto entraram. A asserção que cai reutiliza a instância e o mesmo `agora`. A chave de `_pv_memo` não contém a tranca. Uma consulta fresca pode dar `False` enquanto a instância anterior devolve `True`. A causa dinâmica exata da ocorrência [nao verificado]. `T:1424–1451` · `app/chamados/juizes.py:889–908,931–952` · `app/ponto/services/fechamento.py:1431–1453` |
| **43** | **Curada para a reescrita anterior.** O retry avança o relógio e acusa `UPDATE` de P, DP ou D mesmo com os mesmos valores. A contagem de eventos ainda deixa passar a troca de um evento por outro com o mesmo total. `T:173–174,1507–1527` |
| **46** | **Curada para a ata de fixture.** Depois do plantio, a ata recarregada precisa trazer 480 realizados. Plantar sem relavrar deixa a ata inicial em zero e falha. A mordida do mutante que corta o signal [nao verificado]. `T:1545–1570` · `M:79–85` · `app/ponto/signals.py:85–93,136–140` |

## 2. As 51 ocorrências da pista de 03:06

A lista anexa contém **51 falhas em 29 métodos**, com `errors=0` no placar fornecido. `T` declara 45 métodos numerados. Resultado individual dos 16 métodos sem ocorrência e placar dos mutantes [nao verificado]. A tabela classifica a primeira asserção que caiu, inclusive quando ela só cobre uma parte do caso.

| Caso · ocorrências | A asserção caiu pelo motivo certo? |
|---|---|
| **05 · 1** | **Sim.** Falta `VALIDACAO` física de Q. O caminho carimba V/M e registra `ABSORCAO`. `T:1123–1124` · `app/chamados/services/materializacao.py:1107–1132` |
| **20 · 2** | **Sim.** Tanto no empate quanto em T2, a recusa humana foi aceita, mas Q ficou fora de `recusadas`. Identidade do pedido segue sem teste. `T:1150–1153,1162–1189` | [nao verificado]
| **25 · 1** | **Sim.** M fica de pé apesar da `ABSORCAO` vigente. O seletor atual exige a âncora que essa entrada dispensa. `T:1223–1247` · `app/chamados/services/materializacao.py:1195–1212` |
| **26 · 1** | **Sim, para preservar o legado.** A rejeição muda R/V/M. Uma correção que deixe tudo intacto e nunca encaminhe Q ao admin passa. `T:1254–1272` · `app/chamados/services/materializacao.py:1235–1279` · `app/docs/PROMPTS.md:2873` |
| **01, 02 · 2** | **Sim, pela falta do ato.** O comando calcula a prévia de R/V, mas seu apply chama a porta de colisões e silêncio. As ocorrências ainda não chegam ao zero evento, e ambas aceitam ato novo com proveniência errada. `T:264–295` · `app/chamados/management/commands/backfill_decisoes_pergunta.py:486–516,628–631` |
| **03 · 2** | **Sim.** No vigente, a foto oferece retroativamente uma validação. No controle compensado, falta a validação nova de `09:00` após o apply. A segunda ocorrência prova a ausência do apply nesse controle. `T:311–326` · `app/chamados/models.py:4437–4447` |
| **04 · 1** | **Não para a reversão.** Falta `VALIDACAO` no apply. Uma aplicação irreversível pode ficar verde. `T:328–360` | [nao verificado]
| **11 · 1** | **Sim, pelo ato vazio ausente.** A assinatura atual aceita a chamada e a porta não gera linha. Uma porta que insira outra linha a cada repetição passa, pois há uma única chamada. `T:402–416` · `app/chamados/services/disputa_emissao.py:1695–1715` |
| **12 · 4** | **Sim, uma por FK.** `validada_por`, `materializada_por`, `ato_por` e `validada_original_por` estão isoladas e cada exclusão deveria ser protegida. Manter autoria histórica nula sem marca ainda passa. `T:418–436` · `app/chamados/models.py:3804–3829,4347–4353` |
| **14 · 1** | **Não.** Faltou a `REABERTURA` que o teste passou a exigir. A implementação correta com `SANEAMENTO`, lei em campo próprio e referência à resposta antiga também falha nessa asserção. A resposta mais nova da fixture só é criada depois do saneamento. `T:438–455` · `app/chamados/services/validacao.py:450–455,472–489` · `app/docs/PROMPTS.md:2873,2950` |
| **16 · 1** | **Sim.** A porta aceita a resposta futura. Se a exceção viesse depois de um `INSERT` bem-sucedido e revertido, o observador ainda o registraria. A ordem da projeção durante uma tentativa revertida fica sem verificação equivalente. `T:53–65,457–470` · `app/chamados/models.py:4598–4623` |
| **17 · 3** | **Sim.** `save` aceita DP futura com e sem `force_insert`, e a terceira falha mostra os dois `INSERT`s. O selo AST vem depois e essa pista não o alcança. `T:472–493` · `app/chamados/models.py:4405–4408` |
| **18 · C6 e modal · 2** | **Sim.** O C6 dá `False` em T1 e o modal não apresenta o par que ainda deveria valer. A tarefa, que devolve uma razão, não é a asserção `False != True`. `T:522–534` · `app/chamados/juizes.py:1499–1521` · `app/chamados/services/tarefa_do_dia.py:389–429` |
| **18 · I23 · 1** | **Depende da identidade do contador.** A pista mede a inclusão do compensador futuro pelo I23 real. O I23 vigente foi definido para incluí-lo. O brief exige `ato_futuro=0` no restore, mas atribuir esse contrato novo ao I23 [nao verificado]. Isentar toda `REABERTURA` passa no novo teste e quebra a caracterização F5. `T:536–543` · `app/chamados/management/commands/auditar_invariantes_chamados.py:1122–1134` · `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:319–325` |
| **42 · 1** | **Sim, pelo marco.** O leitor troca `10:00` por `10:30` depois da resposta retrodatada. A asserção de lavra vem depois e a ocorrência não prova entrada ou migração. `T:545–564` · `app/chamados/veredito_lavrado.py:197–209` |
| **44 · 1** | **Não pelo retry.** A primeira aplicação já deixou zero DP. A segunda nem foi alcançada. O teste repete o comando que recalcula o acervo e pode passar com uma porta que duplique o mesmo payload quando o recebe novamente. `T:199–200,566–587` | [nao verificado]
| **45 · autor, lastro, texto novo e FK nula · 4** | **Sim.** Erros de contagem A/B, coerentes com o vigia atual de presença. Um vigia que só compare diferenças observadas de autor e texto ainda passa sem detectar uma diferença exclusivamente de instante. `T:593–632` · `app/chamados/services/vigia_ato_x_coluna.py:8–25` |
| **45 · texto de estoque, absorvida histórica e quatro reconciliadas · 6** | **Interface C4.** As seis comparam `None` à chave `estoque` exigida. Elas não demonstram o valor histórico medido. Devolver zeros fixos na chave elimina cinco ocorrências e ainda perde a divergência do texto histórico. Alimentar um PK de cada vez também deixa escapar erros de precedência em acervo misto. `T:613–640` · `app/chamados/services/vigia_ato_x_coluna.py:21–25` · `C4:559–572,603–607` |
| **09, 10, 29 · 3** | **Sim.** Faltam `VALIDACAO`, `RESPOSTA` e `REABERTURA` físicas, respectivamente. As asserções posteriores de autoria, via e ordem ainda ficam atrás desses REDs. `T:683–700,712–723,957–964` · `app/chamados/services/disputa_emissao.py:2112–2122,2189–2202` · `app/chamados/services/relavra_via.py:230–261` |
| **28 · 1** | **Não para a trava ou compensação.** A pergunta ficou em `declaracao` porque nem entrou no seletor. A exigência de elegibilidade pode justificar um RED próprio. `T:906–943` · `app/chamados/management/commands/encerrar_perguntas_com_fato.py:31–44` |
| **39, 40, 41 · 4** | **Sim.** Caem pela ordem física de V, `recusa_motivo` e R, esta última nos dois retornos de plantio recusado. Um segundo escritor de `validada_por` ou `resposta_colab` em ramo não observado ainda escapa, e `RESULTADO` fica depois do RED do 41. `T:974–1035` · `app/chamados/services/validacao.py:164–178,995–1000` · `app/chamados/services/disputa_emissao.py:1559–1567` |
| **30 · SQL, API, modal, raio-x · 4** | **Sim para os observáveis exercitados.** SQL perde o PK, API e modal expõem a coluna e raio-x põe o chamado no grupo pendente. Só API e modal comparam o par exibido. SQL pode conservar a ordenação crua, e raio-x pode creditar por qualquer resposta histórica, inclusive uma já compensada. `T:1298–1314` · `app/chamados/juizes.py:1863–1868` · `app/api/views.py:1161–1165` · `app/templates/chamados/partials/modal_fio.html:217–220` · `app/chamados/services/raiox.py:135–158` |
| **34, 38 · 2** | **Sim.** `SILENCIO` entra depois de M. A verificação da cascata do pai está depois da primeira asserção. Um pai fechado por `UPDATE` direto ainda satisfaria a checagem de status. `T:1364–1381,1476–1505` · `app/chamados/models.py:2184–2207` · `app/chamados/signals.py:20–46` |
| **35 · 1** | **Sim.** O PK eleito difere do esperado. A vizinha de outro chamado e o empate já estão no cenário. Como o primeiro assert cai, as preservações posteriores ficam [nao verificado]. `T:1383–1418` · `app/chamados/services/disputa_emissao.py:539–542,582–605` |
| **36 · 1** | **Inconclusiva como RED da regra.** A ocorrência é no C6 após trancar, mas o teste admite o contraexemplo do memo descrito acima. A pista não mostra o resultado em P recarregada em nova instância. `T:1434–1447` · `app/chamados/juizes.py:889–908,931–952` |

## 3. Fix4 e o cartão

O **fix4 em si** preserva o que 18 e 30 pedem. O 18 passou a ligar a disputa e a pergunta ao cartão que o montador real usa. O 30 já tinha esses vínculos. `contexto_parcial_fio` monta chamados, disputa e permissões que o template consulta. A mudança de regra do 30 veio no recorte do teste, que injeta `NULL/08:00` e tira a asserção C6. `T:208–220,505–515,1283–1314` · `app/chamados/services/fio.py:251–254,305–362,573–603` · `app/templates/chamados/partials/modal_fio.html:123,157–160`

**Cartão ausente não passa em nenhum dos dois casos.** O `partition` produz `''`, mas a pergunta tem texto e `assertIn` falha antes que o helper devolva `(None, None)`. A lacuna é **dentro do cartão**. Uma irmã usa o mesmo texto, o helper pega a primeira `Resposta:` e não identifica a pergunta pelo PK. Em T2 do 18, a pergunta alvo pode sumir e uma irmã muda de mesmo título satisfazer o teste. Em T1 ou no 30, a resposta esperada pode vir da irmã errada. `T:123–127,216–220,532–534,1310–1311` · `app/templates/chamados/partials/modal_fio.html:157–160,217–220`

O 18 também perdeu a prova de que os três leitores enxergam o **mesmo par**. O C6 compara só um booleano e a tarefa só a razão. Trocar `08:00` por `09:00` ainda diverge da luz `19:00` e pode conservar o booleano, enquanto a tarefa pode manter `aguarda_validacao` com ação ou dono errado. `T:507–509,522–534` · `app/chamados/juizes.py:1428–1454,1521–1533` · `app/chamados/services/tarefa_do_dia.py:165–170` · `J:81–95`

## 4. Conflitos com c4red4

| Recorte da volta 1 | Situação na volta 2 |
|---|---|
| **01/02/44 · entrada e evento** | **Curado mudando o que os casos cobrem.** O backfill pelo comando não se opõe ao evento da resposta pública. As asserções de origem e entrada inicial sumiram. Gravar `entrada_em=ato_em` no primeiro apply e conservá-lo no retry passa em E4D. C4 exige `ESTOQUE/MARCO` na foto histórica e `ATO_NOVO` com um evento para a resposta pública. `T:264–295,566–587` · `C4:478–504` |
| **02/04/08/44 · admissibilidade** | **Curado mudando o que os casos cobrem.** 02 ficou antes do marco. 04 usa resposta vigente correspondente, sem a divergência que C4 manda reprojetar. 08 te

[truncado em 20000 de 31735 caracteres]

_8582 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
