# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 10:47** · publicado em **06/11/2026 10:47** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 10:47 · sessao 0bad524b2ece · R1-CAUSAD-V3 · leitura · origem=Code

O script usa o universo do comando: empresas ativas e colaboradores com `FechamentoMensal` na competência, sem `--so-txt`. Horas e colaboradores distintos vêm da tabela original; a conferência compara os dois números da SOMA impressa, sem refazer a apuração. Com corte 21, as janelas são **21/09–20/10** e **21/10–20/11**; prevalece o corte cadastrado por empresa. (`app/ponto/management/commands/e6_oraculo.py:291`, `:368`, `:422`, `:424`; `app/ponto/janelas.py:10`, `:54`.)

A fonte dos logs declara esse comando; o argv histórico completo dos arquivos não pôde ser confirmado. (`app/core/placar_estrutural.py:115`, `:138`; [não verificado].)

Não executei a sonda. Estimativa inicial: 1.200 pares colaborador/competência a 1–2 segundos cada significariam aproximadamente 20–40 minutos; o script imprime estimativas durante a execução e interrompe cooperativamente após 39 minutos. Tempo real e conclusão dentro desse prazo: [não verificado]. [nao verificado]

```python
def sonda_r1():
    import sys
    sys.dont_write_bytecode = True

    import ast
    import datetime as dt
    import io
    import logging
    import re
    import time
    import tokenize
    from collections import Counter
    from contextlib import redirect_stdout, redirect_stderr
    from pathlib import Path
    from unittest.mock import patch

    saida = sys.stdout
    erros = []
    vistos = {}
    somas = {}
    competencia = "-"
    raiz = None
    chamadas = 0
    universo_total = 0
    inicio = time.monotonic()
    limite = inicio + 39 * 60
    rastreador_anterior = sys.gettrace()
    arquivos = {}
    codigos = {}
    pendentes = {}
    pulsos = 0

    class TempoEsgotado(BaseException):
        pass

    def imprimir(*partes):
        print(*partes, file=saida, flush=True)

    def data_segura(valor):
        if isinstance(valor, dt.datetime):
            return valor.date().isoformat()
        if isinstance(valor, dt.date):
            return valor.isoformat()
        if isinstance(valor, str) and re.fullmatch(r"\d{4}-\d{2}-\d{2}", valor):
            return dt.date.fromisoformat(valor).isoformat()
        return None

    def contexto(frame):
        col, dia = None, None
        while frame is not None:
            loc = frame.f_locals
            if col is None:
                objetos = [loc.get("colaborador"), loc.get("colab"), loc.get("c")]
                obj_self = loc.get("self")
                if obj_self is not None:
                    objetos.append(getattr(obj_self, "__dict__", {}).get("c"))
                for obj in objetos:
                    meta = getattr(type(obj), "_meta", None)
                    if getattr(meta, "model_name", None) == "colaborador":
                        pk = obj.pk
                        if isinstance(pk, int):
                            col = "col%d" % pk
                            break
                if col is None:
                    for chave in ("colaborador_id", "colab_id"):
                        pk = loc.get(chave)
                        if isinstance(pk, int) and not isinstance(pk, bool):
                            col = "col%d" % pk
                            break
            if dia is None:
                # handle incrementa "dia" antes de comparar; "k" é o dia julgado.
                # app/ponto/management/commands/e6_oraculo.py:319-323
                chaves = ("k", "data", "dia", "data_turno")
                for chave in chaves:
                    valor = data_segura(loc.get(chave))
                    if valor is not None:
                        dia = valor
                        break
            frame = frame.f_back
        return col or "-", dia or "-"

    def registrar(exc, frame=None):
        chave = id(exc)
        if chave in vistos:
            return
        vistos[chave] = exc
        tb = exc.__traceback__
        ultimo = None
        while tb is not None:
            ultimo = tb
            tb = tb.tb_next
        contexto_frame = frame or (ultimo.tb_frame if ultimo else None)
        col, dia = contexto(contexto_frame)
        tipo = type(exc).__name__
        origem = "-"
        if ultimo is not None:
            caminho = Path(ultimo.tb_frame.f_code.co_filename)
            if raiz is not None and caminho.is_relative_to(raiz):
                origem = "app/%s:%d" % (
                    caminho.relative_to(raiz).as_posix(), ultimo.tb_lineno
                )
            else:
                origem = "[fora-do-app]:%d" % ultimo.tb_lineno
        # Última linha sanitizada: nenhum conteúdo da mensagem é publicado. [nao verificado]
        ultima_linha = (tipo + ": [mensagem suprimida]")[:200]
        erros.append((competencia, col, dia, tipo, origem, ultima_linha))

    def linhas_de_handlers(codigo):
        if codigo in codigos:
            return codigos[codigo]
        nome = codigo.co_filename
        caminho = Path(nome)
        if raiz is None or not caminho.is_relative_to(raiz):
            codigos[codigo] = frozenset()
            return codigos[codigo]
        if nome not in arquivos:
            with tokenize.open(nome) as fonte:
                arvore = ast.parse(fonte.read(), filename=nome)
            arquivos[nome] = {
                item.body[0].lineno
                for item in ast.walk(arvore)
                if isinstance(item, ast.ExceptHandler) and item.body
            }
        linhas_codigo = {linha for _, _, linha in codigo.co_lines() if linha}
        codigos[codigo] = frozenset(arquivos[nome] & linhas_codigo)
        return codigos[codigo]

    def rastrear(frame, evento, arg):
        nonlocal pulsos
        if evento == "call":
            pulsos += 1
            if pulsos % 256 == 0 and time.monotonic() >= limite:
                raise TempoEsgotado()
            return rastrear if linhas_de_handlers(frame.f_code) else None
        if evento == "exception":
            pendentes[id(frame)] = arg[1]
        elif evento == "line":
            if frame.f_lineno in codigos.get(frame.f_code, ()):
                exc = sys.exc_info()[1] or pendentes.get(id(frame))
                if exc is not None:
                    registrar(exc, frame)
        elif evento == "return":
            pendentes.pop(id(frame), None)
        return rastrear

    def receber_log(logger, registro):
        # Não repassa mensagens, handlers de arquivo ou dados livres.
        if registro.exc_info and registro.exc_info[1] is not None:
            registrar(registro.exc_info[1], sys._getframe(1))

    def ler_relatorio(texto):
        linhas = texto.splitlines()
        padrao_counter = (
            r"\{(?:'[A-Za-z0-9_]+'\s*:\s*\d+"
            r"(?:,\s*'[A-Za-z0-9_]+'\s*:\s*\d+)*)?\}"
        )
        contadores = [
            ast.literal_eval(linha)
            for linha in linhas if re.fullmatch(padrao_counter, linha)
        ]
        donos = [
            m for linha in linhas
            if (m := re.match(
                r"^\s*(ESTRUTURA|CADASTRO|BATIDA)\s+"
                r"(\d+) dia-colab\s+(\d+\.\d+) h\s+(\d+) colabs",
                linha
            ))
        ]
        soma = [
            m for linha in linhas
            if (m := re.match(
                r"^\s*SOMA\s+(\d+)\s+"
                r"<- tem de ser igual a soma das classes \((\d+)\)", linha
            ))
        ]
        colabs = [
            m for linha in linhas
            if (m := re.fullmatch(r"colabs com divergencia: (\d+)", linha))
        ]
        falhas_espelho = [
            m for linha in linhas
            if (m := re.fullmatch(r"erros no espelho: (\d+)", linha))
        ]
        if not (
            len(contadores) == len(soma) == len(colabs)
            == len(falhas_espelho) == 1
            and len(donos) == 3
            and {m[1] for m in donos} == {"ESTRUTURA", "CADASTRO", "BATIDA"}
        ):
            raise RuntimeError("FormatoDaAutoridadeIncompleto")

        t = contadores[0]
        imprimir("COMPETENCIA", competencia)
        imprimir("[1]", {
            chave: t.get(chave, 0)
            for chave in (
                "dias_comparados", "bate_ate_10min",
                "esp_zero_e6_trabalho", "e6_zero_esp_trabalho",
                "diverge_10_60", "diverge_acima_60",
                "dia_batida_impar", "impar_que_BATE", "impar_que_DIVERGE",
                "impar_dentro_da_faixa", "impar_FORA_da_faixa",
                "impar_no_conjunto_do_dia", "impar_SO_POR_CORTE_do_oraculo",
                "dia_sem_linha_no_espelho", "dia_sem_lavratura_no_espelho",
                "dia_isento_ou_ausencia", "dia_sem_trabalho_ambos",
                "esp_desconta_janela_declarada", "esp_lido_pelo_pago_h",
            )
        })
        for linha in linhas:
            if re.fullmatch(
                r"BATE \(ate 10 min\): \d+\.\d+% de \d+ dias comparados", linha
            ):
                imprimir("[2]", linha)
        for m in donos:
            imprimir("[3]", m[0].strip())
        imprimir("[4] TOTAL divergentes", soma[0][2])
        imprimir("[5]", colabs[0][0])
        imprimir("erros_no_espelho", falhas_espelho[0][1])

        # Só compara os valores da SOMA nativa; não soma novamente os donos.
        # app/ponto/management/commands/e6_oraculo.py:415-427
        somas[competencia] = (
            soma[0][0].strip(), int(soma[0][1]) == int(soma[0][2])
        )
        if not somas[competencia][1]:
            raise RuntimeError("SomaDaAutoridadeDivergente")
        if int(falhas_espelho[0][1]) and not erros:
            raise RuntimeError("ErroDoEspelhoSemDetalhamento")

    try:
        with (
            patch.object(logging.Logger, "handle", receber_log),
            redirect_stdout(io.StringIO()),
            redirect_stderr(io.StringIO()),
        ):
            from django.apps import apps
            from django.conf import settings
            from django.core.management import call_command
            from django.db import connection, transaction
            from ponto import janelas
            from ponto.management.commands import e6_oraculo
            from ponto.services import espelho as modulo_espelho

            raiz = Path(e6_oraculo.__file__).resolve().parents[3]
            if (
                settings.SETTINGS_MODULE != "config.settings.sombra"
                or not settings.BANCO_DE_ENSAIO
                or connection.settings_dict["NAME"] != "sombra"
                or connection.schema_name != "juliani"
                or rastreador_anterior is not None
            ):
                raise RuntimeError("AmbienteIncompativel")

            Empresa = apps.get_model("colaboradores", "Empresa")
            FM = apps.get_model("ponto", "FechamentoMensal")
            real_espelho = modulo_espelho.espelho_do_colab

            # Contagem para ETA, seguindo apenas a seleção do comando.
            # app/ponto/management/commands/e6_oraculo.py:291-302
            with transaction.atomic():
                try:
                    empresas = list(Empresa.objects.filter(ativa=True).order_by("pk"))
                    for mes in (10, 11):
                        competencia = "%02d/2026" % mes
                        quantidade = (
                            FM.objects.filter(
                                mes=mes, ano=2026,
                                colaborador__empresa__ativa=True,
                            ).values("colaborador_id").distinct().count()
                        )
                        universo_total += quantidade
                        imprimir("[0] UNIVERSO", competencia, quantidade)
                        janelas_vistas = {
                            janelas.janela_fechamento(mes, 2026, empresa)
                            for empresa in empresas
                        }
                        for ini, fim in sorted(janelas_vistas):
                            imprimir("JANELA", competencia, ini.isoformat(), fim.isoformat())
                finally:
                    transaction.set_rollback(True)

            def observar_espelho(*args, **kwargs):
                nonlocal chamadas
                if time.monotonic() >= limite:
                    raise TempoEsgotado()
                if chamadas and chamadas % 25 == 0:
                    decorrido = time.monotonic() - inicio
                    estimativa = decorrido * universo_total / chamadas / 60
                    imprimir(
                        "ESTIMATIVA", "processados", chamadas,
                        "universo", universo_total,
                        "minutos_total", round(estimativa, 1),
                        "ate_40_min", estimativa <= 40,
                    )
                chamadas += 1
                try:
                    with transaction.atomic():
                        try:
                            resultado = real_espelho(*args, **kwargs)
                            if connection.needs_rollback:
                                raise RuntimeError("TransacaoDoEspelhoAbortada")
                            return resultado
                        finally:
                            transaction.set_rollback(True)
                except BaseException as exc:
                    registrar(exc, sys._getframe())
                    raise

            for mes in (10, 11):
                competencia = "%02d/2026" % mes
                captura = io.StringIO()
                try:
                    with transaction.atomic():
                        try:
                            with connection.cursor() as cursor:
                                cursor.execute("SET LOCAL statement_timeout = '60s'")
                                cursor.execute("SET LOCAL lock_timeout = '5s'")
                            with patch.object(
                                modulo_espelho, "espelho_do_colab", observar_espelho
                            ):
                                sys.settrace(rastrear)
                                try:
                                    # Mesma entrada declarada pelo placar; sem CSV.
                                    # app/core/placar_estrutural.py:138
                                    call_command(
                                        "e6_oraculo", mes=mes, ano=2026,
                                        stdout=captura, stderr=captura,
                                        no_color=True,
                                    )
                                finally:
                                    sys.settrace(rastreador_anterior)
                        finally:
                            transaction.set_rollback(True)
                    ler_relatorio(captura.getvalue())
                except BaseException as exc:
                    sys.settrace(rastreador_anterior)
                    registrar(exc)
                    if isinstance(exc, TempoEsgotado):
                        break
    except BaseException as exc:
        sys.settrace(rastreador_anterior)
        registrar(exc)
    finally:
        sys.settrace(rastreador_anterior)

    imprimir("TEMPO_SEGUNDOS", round(time.monotonic() - inicio, 1))
    for tipo, quantidade in sorted(Counter(e[3] for e in erros).items()):
        imprimir("ERROS_POR_TIPO", tipo, quantidade)
    for comp, col, dia, tipo, origem, ultima in erros:
        imprimir("ERRO", comp, (col, dia, tipo, origem, ultima))
    for comp in ("10/2026", "11/2026"):
        if comp in somas:
            linha, confere = somas[comp]
            imprimir("SOMA", comp, linha.removeprefix("SOMA").strip(), confere)
        else:
            imprimir("SOMA", comp, "NAO_DISPONIVEL", False)
    if erros:
        imprimir("INCOMPLETO:", len(erros))


sonda_r1()
```

As exceções tratadas no código do aplicativo também são observadas: o comando captura falhas do espelho, e o espelho possui fallbacks que esconderiam detalhes numa captura apenas externa. A mensagem da última linha do traceback é suprimida integralmente; permanecem tipo, origem e contexto disponível, sem inventar colaborador ou dia ausente. (`app/ponto/management/commands/e6_oraculo.py:305`; `app/ponto/services/espelho.py:411`, `:569`, `:650`; implementação proposta [não verificado].)

Funções reais chamadas, diretamente ou pelo comando, e suas autoridades na cópia fornecida:

| Função | Autoridade / referência |
|---|---|
| `Command.handle`, via `call_command` | Seleciona o universo, compara os dias e publica a tabela/SOMA. `app/ponto/management/commands/e6_oraculo.py:272`, `:291`, `:413`. |
| `janela_fechamento` | Resolve a competência pelo corte da empresa. `app/ponto/janelas.py:54`. |
| `espelho_do_colab` | Fonte compartilhada do espelho; o wrapper chama a implementação original. `app/ponto/services/espelho.py:1194`. |
| `autoridade_do_periodo` e `montar_dias` | Alimentam o resultado e as linhas lidas pelo comando. `app/ponto/services/espelho.py:971`, `:365`, `:1227`, `:1262`. |
| `folha_manda` | Alimenta os totais do espelho pela folha. `app/relatorios/cartao_pela_celula.py:296`; `app/ponto/services/espelho.py:1284`. |
| `InsumosDoDono.__init__`, `.dono`, `.fase` | Montagem compartilhada dos insumos do dono, incluindo a lavra de fase. `app/ponto/management/commands/e6_oraculo.py:128`, `:164`, `:172`. |
| `dono_da_divergencia` | Juiz efetivo de ESTRUTURA/CADASTRO/BATIDA, com a precedência vigente. `app/ponto/management/commands/e6_oraculo.py:53`, `:98`. |
| `cadastro_do_dia_carregado` | Fornece os defeitos cadastrais do dia ao juiz do dono. `app/ponto/supra_juiz.py:166`. |
| `minutos_do_oraculo` | Derivação central dos minutos comparados. `app/ponto/calculador/nucleo.py:42`. |
| `batidas_apuraveis`, `turnos_do_colab`, `realizado_do_dia` | Universo de batidas e autoridade do realizado usados pelos insumos. `app/ponto/turnos.py:2844`, `:1848`, `:772`; `app/ponto/management/commands/e6_oraculo.py:144`, `:151`, `:195`. |
| `dias_de_fase_as_of` | Lê a fase pela lavra mais recente que cobriu cada dia. `app/escala/services/fase_12x36.py:117`; `app/ponto/management/commands/e6_oraculo.py:166`. |

O R1 não chama o emissor de chamados; esse caminho pertence ao cartório com `apply_=True`. Portanto, a sonda não fornece contagem de emissão. (`app/ponto/services/cartorio.py:1725`, `:1777`.)

A contagem correta é **8 EM_CURSO + 1 ESPERA_RONALD = 9 não concluídos**. O BACKLOG pede este termômetro especificamente para cinco itens e a confirmação de um sexto já FEITO; ESTRUTURAL delega o dono ao comando e mantém os placares separados. (`app/core/espelho_verdade.py:36`, `:48`, `:81`, `:83`, `:130`, `:138`, `:168`, `:172`, `:178`; `app/docs/BACKLOG.md:1478`; `app/docs/ESTRUTURAL.md:34`.)

| Item / estado declarado | Número do script que atualiza; alcance |
|---|---|
| **E1-passivo-53 — EM_CURSO** (`app/core/espelho_verdade.py:36`) | Não mede. O contador específico é `vigencia_impossivel.medir()`: vínculos, colaboradores, ativos e competências lavradas. (`app/escala/services/vigencia_impossivel.py:23`, `:48`.) |
| **E1-sem-previsao — EM_CURSO** (`app/core/espelho_verdade.py:48`) | **[1] `esp_zero_e6_trabalho`** atualiza o indicador citado, mas não comprova previsão válida. A medição específica é `previsto_cego.medir()`, que usa a janela corrente. (`app/ponto/management/commands/e6_oraculo.py:358`; `app/escala/services/previsto_cego.py:114`, `:138`.) |
| **E3-turno-aberto — EM_CURSO** (`app/core/espelho_verdade.py:81`) | **[1] ímpares, batem/divergem e fora da faixa** atualizam o indicador citado. Não são um censo exato de turnos abertos; isso exigiria contar o realizado pela autoridade dos turnos. (`app/ponto/management/commands/e6_oraculo.py:347`, `:386`; `app/ponto/turnos.py:772`.) |
| **E4-rotulo-do-desenho — EM_CURSO** (`app/core/espelho_verdade.py:130`) | Não mede. A prova declarada envelheceu: a pílula já usa `desenho_do_turno`; falta conferir o selo indicado pelo próprio template. (`app/templates/escala/tipos_lista.html:55`.) |
| **E5-le-da-celula — EM_CURSO** (`app/core/espelho_verdade.py:138`) | Não mede a origem do fechamento nem sua conclusão; a pendência declarada é “falta o status encer

[truncado em 20000 de 21491 caracteres]

## 06/11 10:43 · sessao a6f3c423f042 · - · - · origem=Ronald (tmux)

1. **Procede — fluxo da C5 até lista/tela/PDF, sem emissão automática.** `_nunca_bateu` lê `colabs_nunca_bateu` e produz uma assinatura por colaborador, com contagem e primeiras três datas no texto (`app/escala/services/cadastro_realidade.py:169-175`). `lista` agrega C5 ao cadastro (`:802-805`), busca pessoa sem filtrar situação (`:701-704`), mantém a linha desde que pessoa exista e empresa seja visível (`:823-826`), contabiliza cadastro/C5 (`:854-857`, `:882-898`) e usa ação/link de cadastro individual (`:628-630`). Desligado sem vínculo vigente fica no grupo “sem vinculo vigente” (`:867`). A view usa essa lista para HTML ou PDF (`app/escala/views.py:487-492`); HTML renderiza detalhe e hyperlink (`app/templates/escala/cadastro_x_realidade.html:25-28`); PDF percorre exatamente os postos/assinaturas recebidos (`app/escala/services/cadastro_realidade.py:1013-1024`). C5 é só hyperlink; os formulários POST da tela são exclusivos da C7 (`app/templates/escala/cadastro_x_realidade.html:28`). Link C5 existe e detalhe não exclui desligado (`app/colaboradores/urls.py:23`, `app/colaboradores/views.py:461-467`); exige `ver_cadastro`, enquanto lista exige `ver_escala` (`app/colaboradores/views.py:459`, `app/escala/views.py:485`). Ocorrência concreta de falta dessa permissão: **[não verificado]**.

2. **Procede — C5 não fornece dias ao `dias_na_lista`; não cura automaticamente o fio do admin.** A assinatura C5 tem apenas `codigo` e `detalhe`, nenhuma chave `data` (`app/escala/services/cadastro_realidade.py:172-174`); `dias_na_lista` só inclui assinatura que tem `data` (`:974-980`). O vigia carrega tanto mapa de dias quanto conjunto de pessoas (`app/chamados/services/vigia_fio.py:216-218`). Assim, desligado com somente C5 passa a estar em `_colabs`, permanece fora de `_dias`, e C5 sozinha não satisfaz `dia_do_admin`/balde `fio_do_admin` (`:222-228`, `:313-314`). Na ausência das condições anteriores, `censo_alcance` pode mudar a causa de “e/outra” para “d/colab na lista, dia fora”, mantendo fio morto (`:654-680`). Isso é limitação comprovada do transporte da C5 para leitura por dia; ocorrência em produção **[não verificado]**.

3. **Procede — `vigia_retido_calado` passa desligado de calado a roteado por presença da pessoa, sem validar dia nem competência.** O consumidor usa `set(colabs_nunca_bateu(empresas))`, descartando as listas de datas (`app/folha/vigia_retido_calado.py:40-41`). Para cada retrato da última lavra diária por empresa (`:35-39`), toda linha bloqueada de fio `ninguem` cujo `colab` consta no conjunto vira roteada, reduzindo calados (`:44-53`). Presença C5 noutra competência já basta para essa classificação; coincidência exata dos dias do retrato com os da C5 não é testada ali (`:41`, `:46`). O comando imprime e pode retornar erro com alarme; não tem aplicação de reparo (`app/folha/management/commands/vigiar_retido_calado.py:28-39`). Não gera chamado/pergunta/push (`app/folha/vigia_retido_calado.py:27-55`).

4. **Não procede — presença C5 não faz comandos fecharem chamados C8/C7.** `fechar_chamados_vinculo_na_lista` lê `lista`, seleciona exclusivamente assinatura `C8` e chamados `vinculo_divergente` (`app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:35-45`); só `--apply` chama `ch.cancelar` (`:52-59`). `fechar_avisos_regua_na_lista` seleciona exclusivamente `C7` e módulo `regua_suspeita` (`app/escala/management/commands/fechar_avisos_regua_na_lista.py:27-37`); só `--apply` cancela (`:42-49`). Entrada exclusivamente por C5 não muda elegibilidade de nenhum fechamento nesses comandos (`respectivamente :40`, `:32`).

5. **Não procede — `placar_estrutural` não executa essa leitura.** Há referência declarativa em string `fonte` (`app/core/placar_estrutural.py:149`), mas o módulo declara atualização por quem mede (`:28`) e `placar()` apenas copia/processa `RESULTADOS`, sem I/O (`:635-660`). A inclusão do desligado não remede automaticamente o número R2 (`:140-149`).

6. **Procede — há escrita em disco num consumidor indireto; não confundir com escrita de negócio.** Serviço do vigia censo lê e devolve contadores (`app/chamados/services/vigia_fio.py:318-323`); comando monta lista e chama censos (`app/chamados/management/commands/vigiar_fio_morto.py:163-173`). Em rodada de frota, o comando grava carimbo com resultados, sem `--apply` (`:210-226`), e `_carimbar` cria pasta, arquivo temporário e faz `os.replace` (`:127-134`). Recorte `--empresa` não carimba (`:228-230`). C5 pode alterar o conteúdo dos contadores carimbados conforme achado 2; essa escrita preexistente não cria chamado/pergunta/push (`:210-226`).

7. **Não procede — exclusões remanescentes de desligado não são outra exclusão da C5/lista; respondem outros universos.**
   - `universo_adesao` lê `nunca_bateu` só de ativos (`app/chamados/services/adesao.py:25-34`), e sincronizador ainda veta desligado (`:66-67`); ele abre/reabre chamados de adesão (`:74-92`), portanto é emissão, distinta da mesa administrativa C5.
   - Cartório continua vetando emissão de furo e parcial para desligado (`app/ponto/services/cartorio.py:1696-1697`, `:1762`).
   - “Quem cobrar” mantém universo declarado de ativos (`app/escala/services/furos_diarios.py:247-259`) e elimina desligado antes de marcar `nunca_bateu` por padrão de batidas (`:272-283`); sua view usa esse universo (`app/relatorios/views.py:1637-1642`). É tela de cobrança, não C5.
   - Dia da Frota lê vereditos incluindo rótulo nunca bateu, mas universo só ativo (`app/relatorios/dia_da_frota.py:13`, `:34`, `:54-60`).
   - A exclusão remanescente em `_afastado_sem_registro` é da C4/furo (`app/escala/services/cadastro_realidade.py:541-556`), não da C5.
   - Nenhum filtro posterior por desligado aparece no fluxo efetivo da lista: `_vinculos` lê IDs sem situação e o filtro final é existência/empresa (`app/escala/services/cadastro_realidade.py:701`, `:825-826`). Por isso esses universos divergem por finalidade, mas não anulam a nova inclusão C5.

## 06/11 10:43 · sessao d7f337b57793 · - · - · origem=Ronald (tmux)

1. **Não procede — segundo escritor de produção introduzido.** O diff apenas consulta (`app/escala/services/cadastro_realidade.py:160-165`). `CelulaDia.veredito` passa por `lavrar_veredito`, delegado pelo modelo (`app/escala/models.py:1819-1828`) à porta, que atribui e salva (`app/ponto/portas/celula.py:814-819`). Chamadores automáticos: `app/ponto/services/cartorio.py:889`, `:1242`, `:1802`; decisão humana: `app/ponto/services/veredito_celula.py:330`. As gravações diretas novas são fixtures de teste (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:29-34`).

2. **Mapa de escritores dos campos.**
   - `CelulaDia.data`: campo em `app/escala/models.py:1685`; criado pela porta em `app/ponto/portas/celula.py:253-254` e `:633-636`. Não localizei reatribuição de data de célula em produção; ausência exaustiva de escrita dinâmica: **[não verificado]**.
   - `Colaborador.situacao`/`data_demissao`: definições em `app/colaboradores/models.py:323-325`; operação dona em `app/colaboradores/services/colaborador.py:32`; persistência em `:55-76`, criação em `:91-93`. Desligamento grava ambos pela porta (`app/colaboradores/services/desligamento.py:104-108`); agendamento grava somente demissão/motivo (`:82-86`); reativação põe ativo e limpa demissão (`:209-214`). Admissão cria ativo pela porta (`app/colaboradores/views.py:1181-1205`).
   - Outros escritores de `situacao`: lançamento de INSS escreve afastado pela porta (`app/ponto/views.py:2433-2435`); reversor escreve ativo pela mesma porta e deixa trilha (`app/ponto/management/commands/reverter_situacao_afastado.py:65-80`). Django admin de Colaborador é somente leitura (`app/colaboradores/admin.py:48`, `:65-68`).

3. **Leitores dos campos, além da consulta alterada.** `CelulaDia.data` alimenta o mapa por colaborador/dia (`app/escala/alimentacao.py:73-77`), e o retentor lê data/veredito em `app/folha/export.py:1206`, `:1224-1255`; a auditoria humana lê os mesmos campos (`app/ponto/services/veredito_celula.py:107-109`). `data_demissao` é lida pelo retentor sem consultar situação (`app/folha/export.py:1198-1201`), pelo cadastro do dia (`app/ponto/supra_juiz.py:142-162`, `:194`) e pelo cron de efetivação (`app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38-40`). `situacao` também filtra outras famílias da lista (`app/escala/services/cadastro_realidade.py:221`, `:420`, `:541`) e a evidência de vínculo (`app/ponto/services/vinculo_na_lista.py:70`, `:151-153`). Um inventário integral dos leitores dinâmicos de todos os campos continua **[não verificado]**.

4. **Não procede — RED sem mordida contra os cinco mutantes especificados, pela leitura das asserções.**

   | Mutante | Asserção que o distingue |
   |---|---|
   | M1, restaurar exclusão inteira | Desligado precisa existir e gerar C5/porta: `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:49-57`; dias explícitos: `:60-62` |
   | M2, `__gt` → `__gte` | Demissão é um dos dias da fixture (`:23-24`) e deve entrar (`:62`) |
   | M3, tirar corte | Dias posteriores existem (`:24`) e não podem entrar (`:62`) |
   | M4, cortar ativo | Ativo com demissão preenchida conserva todos os dias (`:81-84`) |
   | M5, excluir desligado sem data | Lista inteira de dias é exigida (`:69-73`); teste vizinho também exige presença (`app/folha/tests/test_vigia_retido_calado.py:104-106`) |

   A testemunha realmente é chamada, sem copiar a regra (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:40-46`, `:65-66`). A execução e as contagens RED/GREEN/mutantes informadas no commit são **[não verificado]**: os arquivos `logs/esemvivab/*` mencionados no anexo não estão disponíveis nesta cópia.

5. **Procede — cobertura da igualdade é mais estreita que a alegação.** A fixture não fornece admissão (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:30-34`), enquanto a folha corta por admissão (`app/folha/export.py:1198-1199`). Só o teste de ativo cobre hoje/ontem (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:78-80`); a comparação contra folha usa os dias de desligado entre 5 e 12 de outubro (`:23-24`, `:65-66`), sem testar desligado hoje, futura célula acusadora, pré-admissão ou isento. A folha tem guarda própria de isento (`app/folha/export.py:1191-1196`).

6. **Procede — referência documental incorreta nesta cópia.** O novo docstring diz que a janela de retenção vive em `1224-1229` (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:4`), mas o corte por admissão/demissão real está em `app/folha/export.py:1198-1201`; `:1224-1229` percorre os dias e define `_dia_dem`. O teste importa a função pelo nome e continua chamando a testemunha correta (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:40-46`).

7. **Dúvida — ordem de trava e pouso antes do pré-requisito.** O trecho produtivo alterado contém somente uma consulta, sem nova trava/transação (`app/escala/services/cadastro_realidade.py:160-165`); portanto não demonstra inversão de locks. O contrato exige suíte inteira verde antes do opus final e impede certificação de pouso apenas pelo Codex (`CLAUDE.md:364-373`). A sequência efetiva de suíte inteira → opus → pouso deste commit é **[não verificado]**; o anexo relata somente testes direcionados/vizinhos. `app/docs/RELATO.md:3` ainda registra A/B como candidatas e descreve a exclusão antiga, sem comprovar o histórico desta obra.

## 06/11 10:43 · sessao cbe5c800c718 · - · - · origem=Ronald (tmux)

1. **Procede — a igualdade de janelas prometida não é geral.** A C5 usa `[início da competência anterior, ontem]`, sem admissão, com corte de demissão apenas quando `situacao='desligado'` (`app/escala/services/cadastro_realidade.py:154-164`). O retentor usa `[ini,fim]` fornecido, recortado por admissão e demissão independentemente da situação (`app/folha/export.py:1198-1206`). Divergências:

   - **Admissão:** C5 inclui célula `nunca_bateu` anterior à admissão dentro da janela; retentor exclui (`cadastro_realidade.py:160-164`; `export.py:1198-1199`).
   - **Ativo com demissão preenchida:** C5 mantém dias posteriores; retentor corta (`cadastro_realidade.py:162-163`; `export.py:1200-1201`). O teste exige esse comportamento da C5, mas não compara esse caso com a testemunha (`app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:81-84`).
   - **Hoje e futuro:** C5 exclui por `data__lte=ontem`; retentor pode reter uma célula acusadora hoje/futura até `fim`, pois `d < hoje` guarda apenas os ramos de célula ausente/sem veredito (`cadastro_realidade.py:155-161`; `export.py:1230-1256`).
   - **Competências:** C5 sempre agrega anterior e corrente transcorrida; retentor pode receber qualquer período. Com anterior isolada, C5 acrescenta corrente; com corrente isolada, acrescenta anterior; com competência mais antiga, C5 não a cobre (`cadastro_realidade.py:159-161`; `export.py:1168,1198-1206`; `app/ponto/janelas.py:60-72`).
   - **Isenção:** C5 não verifica Art.62; retentor retorna vazio (`cadastro_realidade.py:160-164`; `export.py:1191-1196`; `app/core/constants.py:13-18`).
   - **Protestos:** C5 aceita qualquer célula `nunca_bateu`; retentor ainda aplica não cobráveis e pausa (`cadastro_realidade.py:160-164`; `export.py:1245-1254`). A ocorrência dessas combinações em produção é **[não verificado]**.

2. **Não procede — divergência no dia da demissão ou na demissão nula, para desligado `nunca_bateu`.** C5 só exclui `data > data_demissao`; o retentor percorre `d <= fim`, e `_dia_dem` só isenta `sem_celula`. Sem demissão ambos deixam de cortar por esse campo (`cadastro_realidade.py:162-163`; `export.py:1200-1201,1225-1256`).

3. **Não procede, em sentido literal — duplicação de juiz nominalmente registrado para “quais dias do desligado a folha retém”.** JUIZES.md é lookup e dá precedência ao registro vivo (`app/docs/JUIZES.md:7-15`). O registro declara `classificar_export` para aprovação/TXT e entrada na folha, não declara nominalmente `motivos_retencao_celula` para os dias (`app/core/juizes.py:160-168,288-323,699-712`). Existe cadeia registrada `classificar_export → motivos_retencao_espelho → motivos_retencao_celula` (`export.py:960,1261-1267`).

4. **Procede — derivação paralela de fonte única.** O leitor declara cobrir a janela da testemunha, porém escreve SQL próprio com as diferenças acima (`cadastro_realidade.py:140-144,159-163`). O retentor já ganhou `com_dias=True` precisamente para impedir cópia dessa pergunta (`export.py:1178-1183`). Há precedente explícito de remoção de uma cópia que divergia em admissão/demissão e protestos (`app/folha/previa.py:149-170`). A lei exige que leitores usem a mesma autoridade (`CLAUDE.md:20-22`).

5. **Procede — a premissa “desligado retém a folha” precisa distinguir retenção da célula de bloqueio no pipeline real.** Se há demissão até o fim da competência, `classificar_export` sai por rescisão antes de consultar o retentor (`export.py:54-60,858-863,960`). O Retrato coloca rescisão em `modulo_proprio`, fora de `bloqueado` (`app/folha/retrato_folha.py:29-33,89-95`). Portanto o teste direto de `motivos_retencao_celula` não prova que aquele desligado estaria bloqueado no Retrato real (`test_esemvivab_desligado_nunca_bateu.py:40-46`).

6. **Procede — os testes de equivalência não cobrem as divergências.** A fixture do desligado tem somente dias 05–12/10, hoje 25/10, admissão nula e sem isenção/protestos informados (`test_esemvivab_desligado_nunca_bateu.py:21-34`; `app/colaboradores/models.py:324`). Empresa usa corte padrão 21; assim esses dias são todos da competência anterior (`app/colaboradores/models.py:46`; `app/ponto/janelas.py:10-21,24-33`). As comparações com o retentor estão apenas nos dois casos de desligado (`test_esemvivab_desligado_nunca_bateu.py:65-73`).

7. **Não procede — competência entregue como diferença direta entre os dois corpos.** Nenhum deles consulta `competencia_entregue`; a C5 inclui a anterior incondicionalmente e o retentor trabalha o período recebido (`cadastro_realidade.py:154-165`; `export.py:1192-1258`). O juiz específico de entrega existe em outro lugar (`app/core/juizes.py:312`; `export.py:556-584`). Se a anterior está entregue em produção é **[não verificado]**.

## 06/11 10:42 · sessao 481eb41296f1 · - · - · origem=Ronald (tmux)

Recomendo manter `F.perguntar_pela_celula` como autoridade da sonda e declarar que a medição descreve a recusa atual da fábrica sobre a célula lavrada; o ingresso histórico pelo cartório continua [nao verificado]. A fábrica efetivamente recebe a célula em `julgar_colab`; o comando é outra camada e não é obrigatório para chamar essa autoridade real. (`app/ponto/services/cartorio.py:997`, `app/chamados/services/fabrica_da_celula.py:145`)

`julgar_colab(apply_=False, forcar=True)` não cobre esse ingresso: a fábrica está dentro de `if apply_`, e o ramo `nunca_bateu` executa classificação/lavra somente dentro de outro `if apply_`. Portanto, adicioná-lo não elimina a limitação e cria uma segunda medição incompleta. (`app/ponto/services/cartorio.py:981`, `app/ponto/services/cartorio.py:1207`, `app/ponto/services/cartorio.py:1226`)

O emissor separado de chamado é `detectar_ausencias.Command._abrir_chamado`; seus parâmetros reais vêm de dois ramos específicos do cartório. Chamá-lo com parâmetros montados à mão mudaria a pergunta da sonda. Não encontrei modo somente leitura que atravesse integralmente esse emissor e depois a fábrica. (`app/ponto/management/commands/detectar_ausencias.py:570`, `app/ponto/services/cartorio.py:1717`, `app/ponto/services/cartorio.py:1740`, `app/ponto/services/cartorio.py:1768`, `app/ponto/services/cartorio.py:1778`)

Se o contrato for interpretado como exigência de medir também o chamado, seria necessário executar `apply_=True` com interceptação de todas as portas reais de escrita e lidar com as releituras posteriores; interromper na primeira escrita não chega à fábrica e não satisfaz essa interpretação. (`app/ponto/services/cartorio.py:1802`, `app/ponto/services/cartorio.py:997`, `app/ponto/services/cartorio.py:1038`)

## 06/11 10:41 · sessao 829ee648074e · - · - · origem=Ronald (tmux)

- A porta histórica é `diff_calculador.Command.handle`, com `--pares-da-autoridade`; ela chama `insumos_do_calculador` e `rubricas_por_dia` reais. (`app/ponto/management/commands/diff_calculador.py:287`, `:309`, `:312`, `:417`)
- Neste hash o instrumento ainda omite `minutos_extra_100_feriado` e soma indiscriminadamente `periodos_ft` como folga. Não serve para atribuir essas duas diferenças ao v2 sem confrontar o emissor real. (`app/ponto/management/commands/diff_calculador.py:253`, `:263`, `:268`; diagnóstico histórico: `app/docs/RELATO.md:560`)
- É possível observar o comando sem reconstruir cálculos: trace restrito ao `Command.handle.__code__`; na entrada do ramo divergente, linha 539, recolher `c.pk`, `k`, `r`, `a`, `b`; no retorno recolher `S`, `div`, `colabs_div`, `T`, `linhas`. Isso mantém valores sem arredondamento e respeita o limiar escolhido pelo comando. (`app/ponto/management/commands/diff_calculador.py:535`, `:538`, `:539`, `:542`)
- Soma dos deltas divergentes não equivale necessariamente ao delta de `S`: `S` recebe também diferenças inferiores ou iguais a 0,02 h. Para reconciliação completa, conservar uma parcela explícita residual/sub-limiar. (`app/ponto/management/commands/diff_calculador.py:535`, `:536`, `:537`, `:538`)
- Os erros capturados silenciosamente pelo comando podem ser registrados por eventos `exception` no frame do comando e nas funções locais do mesmo arquivo, sem alterar suas decisões. Sítios de captura: `app/ponto/management/commands/diff_calculador.py:148`, `:226`, `:314`, `:350`, `:368`, `:405`.
- O emissor real `dia_pago.lavrar` distribui HE100 de feriado e as parcelas de folga; o fechamento chama esse mesmo emissor nas versões `motor` e `oraculo`. (`app/ponto/services/dia_pago.py:135`, `:183`, `:223`, `:250`; `app/ponto/services/fechamento.py:1185`, `:1261`)
- Escritas exatas do emissor: `DiaPago.objects.filter(...).delete()` e `DiaPago.objects.bulk_create(novas)`. Dentro de um wrapper que chama exclusivamente o `lavrar` original, os patches podem limitar-se ao manager usado por essas duas operações: `filter` devolve objeto com `delete` simulado; `bulk_create` recolhe as instâncias em memória. O retorno original continua sendo `len(novas)`. (`app/ponto/services/dia_pago.py:163`, `:357`, `:358`, `:359`)
- Para alcançar esse emissor por `--periodos-do-fechamento`, o wrapper de `fechamento.recalcular_fechamento_mes` precisa restaurar temporariamente o emissor real instrumentado: o comando substitui `dia_pago.lavrar` por `_nada` antes de chamar o fechamento. (`app/ponto/management/commands/diff_calculador.py:123`, `:137`, `:140`, `:142`)
- Os totais v2 podem vir do retorno real de `recalcular_fechamento_mes(somente_leitura=True, retornar_objeto=True)`, que contém o próprio objeto destacado do fechamento. (`app/ponto/services/fechamento.py:439`, `:1112`, `:1119`, `:1293`)
- O fechamento também engole falhas do emissor e por colaborador; é necessário observá-las para cumprir `INCOMPLETO`. Seu log por colaborador contém nome, portanto não deve ir diretamente ao stdout. (`app/ponto/services/fechamento.py:1222`, `:1281`, `:1287`, `:1290`)
- Tempo de execução e universo atual não podem ser certificados sem execução/dados adicionais. [nao verificado]

## 06/11 10:41 · sessao 481eb41296f1 · - · - · origem=Ronald (tmux)

Montagem e reconciliação:

- Use exatamente `V.censo_alcance(limite=10**7)`; filtre `x[4] == 'sem_porta' and x[5] == 'd'`; agrupe por `x[1]`; monte `InsumosDoDono(c, min(datas), max(datas), hoje, agora=agora)` e retenha somente `ins.dono(data) == 'ESTRUTURA'`. Esta é a montagem do anexo; a autoridade do primeiro total é `r['alcance_d']`, e a do subconjunto é o retorno de `ins.dono`. (`app/chamados/services/vigia_fio.py:542`, `app/ponto/management/commands/e6_oraculo.py:128`, `app/ponto/management/commands/e6_oraculo.py:172`)
- A lista contém `[celula_id, colab_id, data, competencia, rota, causa, detalhe, papel]`; carregue a célula por `x[0]`, conferindo colaborador/data. Há unicidade colaborador/data na montagem do censo. (`app/chamados/services/vigia_fio.py:548`, `app/chamados/services/vigia_fio.py:593`, `app/chamados/services/vigia_fio.py:629`)
- `200` e `182/13/5` devem ser expectativas históricas, nunca totais impostos ao processamento. Valores atuais na sombra: [nao verificado].

Caminho verdadeiro do evento:

`_cartorio_post_save_batida` → `_julgar_dias` → `cartorio.julgar_celula` → `cartorio.julgar_colab` → `fabrica_da_celula.perguntar_pela_celula`. (`app/ponto/signals.py:131`, `app/ponto/signals.py:85`, `app/ponto/signals.py:93`, `app/ponto/services/cartorio.py:1960`, `app/ponto/services/cartorio.py:1989`, `app/ponto/services/cartorio.py:997`)

O comando `processar_cartorio` usa o mesmo `julgar_colab`, com `detectar_ausencias.Command` como emissor do chamado do dia. (`app/ponto/management/commands/processar_cartorio.py:36`, `app/ponto/management/commands/processar_cartorio.py:42`, `app/ponto/management/commands/processar_cartorio.py:65`)

Não confundir isso com `post_save` de `CelulaDia`: a porta documenta expressamente que o gatilho é o ato de corrigir cadastro, e recusa adicionar sinal à entidade. (`app/ponto/portas/celula.py:362`)

Instrumentação sem copiar predicados:

- `patch(..., wraps=real)` nas funções `dia_sem_trabalho`, `vencidos_no_instante`, `parcial_so_lista`, `_ata_do_dia`, `motivos_pela_ata`, `limbo_vai_a_pauta`.
- Para os retornos inline, `sys.settrace` limitado aos `__code__` reais de `motivos_a_perguntar`, `motivos_pela_ata`, `perguntar_pela_celula`; registrar `frame.f_lineno` no evento `return`. Assim a classificação decorre da linha executada, sem reproduzir `situacao`, `data`, `veredito` ou os filtros.
- Guardas de `motivos_a_perguntar`, na ordem: dia sem trabalho/inativo (`91–92`); futuro (`93–94`); vencimento calculado (`98`); veredito absolvente (`99–100`); parcial só lista (`102–104`); deduplicação após motivos da ata (`118–126`). (`app/chamados/services/fabrica_da_celula.py:91`, `app/chamados/services/fabrica_da_celula.py:93`, `app/chamados/services/fabrica_da_celula.py:98`, `app/chamados/services/fabrica_da_celula.py:99`, `app/chamados/services/fabrica_da_celula.py:102`, `app/chamados/services/fabrica_da_celula.py:126`)
- Guardas internas dos motivos: vencidos vazio (`45–46`), ata não autoriza (`50–51`), faltantes `None` com vencidos não `None` (`52–54`), classificador sem motivo (`56–57`), exclusão de marcos não vencidos (`60–61`), ausência de correspondência de motivo (`62–64`). (`app/chamados/services/fabrica_da_celula.py:45`, `app/chamados/services/fabrica_da_celula.py:50`, `app/chamados/services/fabrica_da_celula.py:52`, `app/chamados/services/fabrica_da_celula.py:56`, `app/chamados/services/fabrica_da_celula.py:60`, `app/chamados/services/fabrica_da_celula.py:62`)
- `parcial_so_lista` deve ter a própria linha de retorno registrada: `399` distingue parcial sem período; `400` distingue a regra de julgamento anterior/ausência de fato novo. A instrumentação não precisa copiar esses testes. (`app/ponto/services/cartorio.py:398`, `app/ponto/services/cartorio.py:400`)
- Depois dos motivos, limbo entregue segue para Pauta DP e retorna `[]`; isso é destino alternativo, não ausência de emissão. (`app/chamados/services/fabrica_da_celula.py:171`, `app/chamados/services/fabrica_da_celula.py:172`, `app/chamados/services/fabrica_da_celula.py:173`)

Armadilhas:

- `_ata_do_dia` captura `Exception` e devolve `(True, None)`. O contrato exige observar essa captura; envolver `core.observ.registrar_engolido` para contar a exceção recebida preservando/repropagando conforme desenho da sonda. Contar só erros que escapam do emissor perderia estes casos. (`app/chamados/services/disputa_emissao.py:828`, `app/chamados/services/disputa_emissao.py:830`, `app/chamados/services/disputa_emissao.py:832`)
- Chamar diretamente `perguntar_pela_celula` prova a resposta atual do emissor para o estado lido. Não demonstra que o evento histórico efetivamente o chamou: `julgar_colab` só chega ali com `apply_` e célula presente no `cods` do ato. (`app/ponto/services/cartorio.py:981`, `app/ponto/services/cartorio.py:986`, `app/ponto/services/cartorio.py:995`)
- Para não vazar conteúdo, represente o último frame como `arquivo:linha:funcao`, com até 200 caracteres; não imprima `str(exc)`, SQL nem linha de código contendo dados. A existência de dados pessoais em mensagens concretas permanece [nao verificado].
- Tempo de execução na sombra: [nao verificado]. O custo amplo é do censo e do motor solicitado uma vez por colaborador pelo carregador; as 200 chamadas diretas da fábrica evitam rejulgar a frota. (`app/chamados/services/vigia_fio.py:582`, `app/ponto/management/commands/e6_oraculo.py:154`, `app/ponto/management/commands/e6_oraculo.py:159`)

## 06/11 10:41 · sessao faa68ab99b0c · - · - · origem=Ronald (tmux)

R1 chama apenas o juiz do dono; cartório e emissor são caminhos separados:

- `app/ponto/management/commands/e6_oraculo.py:304`: chama `espelho_do_colab`; `:316` cria `InsumosDoDono`; `:366` chama seu `dono`; `:183` delega a `dono_da_divergencia`.
- `app/ponto/services/espelho.py:1227`: `autoridade_do_periodo`; `:1262` chama `montar_dias`; `:1284` chama `folha_manda`. Não identifiquei escrita nesse caminho. [não verificado quanto à totalidade transitiva dos métodos de modelos]
- `app/ponto/services/cartorio.py:895`: `julgar_colab` é outra entrada. Seu corpo chama `classificar_dia` em `:1413`; emissor `_abrir_chamado` é chamado somente com `apply_=True`, em `:1725`–`:1740` e `:1777`–`:1778`. Portanto `apply_=False` não mede o emissor.
- `app/ponto/management/commands/detectar_ausencias.py:693`: emissor chama a porta `ChamadoColaborador.abrir`; depois pode escrever marcos (`:744`), contexto/nota (`:765`, `:768`), enviar push (`:778`) e criar fio (`:801`). Não basta interceptar só a criação para fazer uma sondagem desse caminho.

O problema mais importante do contrato é erro engolido:

- `app/ponto/management/commands/e6_oraculo.py:305`: exceção do espelho vira apenas contador e `continue`.
- `app/ponto/services/espelho.py:411`: falha lendo DiaPago vira `{}`.
- `app/ponto/services/espelho.py:569`: falha da grade vira grade vazia.
- `app/ponto/services/espelho.py:650`: falha de fatos/palavra vira ausência da palavra.
- `app/ponto/motor_calculo_v2.py:1387`: falha de carregar empresa vira `None`; `:1419`: falha das decisões HE vira `{}`.
- `app/escala/services/leitor_celula.py:27`: erro de conversão vira `None`.

Capturar somente exceções que escapam do `call_command` ou somente registros de logging não atende “erro nunca engolido”, porque os dois últimos fallbacks do motor não registram logger. A alternativa instrumental sem reproduzir lógica é ler os módulos com `ast`, mapear a primeira linha executável de cada `ExceptHandler` e usar `sys.settrace` nesses frames; quando essa linha executar, registrar `sys.exc_info()` com contexto col/data da pilha. Deduplicar a mesma instância de exceção preservando referências evita contá-la novamente quando for tratada em mais de um nível. A execução usa Python 3.13 segundo `app/Dockerfile:1`.

## 06/11 10:40 · sessao 79d40633b87e · - · - · origem=Ronald (tmux)

O “resto dos 38” tem três blocos no registro: cenários B17–B33; fechamento/export para a propriedade “todo leitor lê o mesmo número”; e dimensionamentos de intervalo/tolerância. (`app/docs/BACKLOG.md:1621`)

**Oito cenários sem execução naquela costura, segundo o registro atual da bateria complementar:**

| Caso | O que falta / destino declarado | Evidência |
|---|---|---|
| B18 | Batida 00:00 UTC−3: pergunta dos leitores e do dia da jornada; `onde='fora'`. | `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:69` |
| B20 | Segundo vínculo vigente sobreposto ao primeiro; `onde='cadastro'`. | `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:76` |
| B24 | Só duas batidas da pausa: falta exercício pelos leitores/gravação; `onde='fora'`. O corte posterior manda EM ABERTO, soma dos pares fechados. | `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:100`; `app/docs/BACKLOG.md:1521` |
| B25 | Colaborador sem vínculo: precisa exercitar a porta de fechamento, pois a costura exige `ec`. | `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:104` |
| B29 | Retratação pertence ao universo apurável antes do motor; aponta para tripwire próprio, não para caso de período. | `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:122` |
| B30 | Registro ainda pede vínculo com âncora futura; corte posterior descreve extrapolação máxima cadastrada por TipoEscala, inicialmente 180 minutos. Essa diferença impede declarar o caso atualizado apenas pelo rótulo. | `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:126`; `app/docs/BACKLOG.md:1521` |
| B31 | Falta exercício da porta de cadastro e lista dos existentes; registro ainda usa pergunta antiga sobre início=fim. Corte posterior manda recusar na porta e listar existentes. | `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:129`; `app/docs/BACKLOG.md:1521` |
| B33 | Atestado aprovado em folga com par fechado; fixture de cadastro ausente e metade da lei declarada sem dono. | `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:136` |

O selo de cobertura só exige caso executável para entradas `onde='roda'`; para `fora`/`cadastro` exige explicação textual. Portanto os oito itens acima não são oito testes vermelhos nem oito bugs da v3. (`app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:503`; `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:514`)

**Casos já presentes que não devem ser apresentados como ainda ausentes:**

- B17: virada da competência, teste e asserções 720 na competência inicial e zero na seguinte. O `660+0` do backlog está desatualizado frente à correção explicitada pela bateria. (`app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:252`; `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:266`; `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:44`)
- B19, B22, B23 e B32: casos tabelados com respostas numéricas. (`app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:204`; `:213`; `:220`; `:226`)
- B21: teste presente, mas com redação anterior “abono se cancela”; corte posterior diz batidas contam e abono cobre apenas previsto descoberto. Não certificar aderência ao corte novo sem conferir esse comportamento. (`app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:276`; `app/docs/BACKLOG.md:1521`)
- B26 e B28: testes presentes. (`app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:319`; `:478`)
- B27: presente; B27b e B27c são pinos explícitos do defeito v2 ligado à ordem/deduplicação. (`app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:340`; `:371`; `:423`)

**Demais pendências:**

- Propriedade de quatro leitores: o backlog declara só 2/4 pontas e deixa fechamento/export de fixture pendentes. O teste de export explica por que compara a função isolada: montar entrada no TXT exigia catálogo, fechamento e célula julgada em todo dia, e as tentativas caíram em `cadastro_zero`/`furo_espelho`. Isso demonstra o obstáculo, não a cobertura integral dos quatro leitores. (`app/docs/BACKLOG.md:1522`; `app/docs/BACKLOG.md:1621`; `app/folha/tests/test_porta_do_export.py:216`; `:224`)
- Art.71 §4 pós-2017: dimensionamento suprimido versus “INTEGRAL” continua pergunta nomeada; comentário v2 contém “indeniza INTEGRAL”. (`app/docs/BACKLOG.md:1621`; `app/ponto/motor_calculo_v2.py:68`)
- Art.58 §1: dimensionamento 5 por marcação versus 10 por ponta; C13 declara explicitamente a contradição, e F9/F9b são indicados como exercícios da lei. (`app/docs/BACKLOG.md:1621`; `app/ponto/tests/test_o280_bateria_da_lei.py:605`; `:611`)
- Degrau HE >2h→100% continua explicitamente aberto no backlog; os casos só afirmam total e conservação das parcelas, sem certificar 120/restante como lei. (`app/docs/BACKLOG.md:1621`; `app/ponto/tests/test_o280_bateria_da_lei.py:1954`; `:5742`)

Há divergência documental de contagem: o backlog diz “15 células B17–B33” e chama B33 verde; a tabela atual contém **17 IDs**, o selo exige 17 e B33 está `cadastro`. (`app/docs/BACKLOG.md:1621`; `app/ponto/tests/test_o287_leg2_absurdos_do_esmeril.py:64`; `:136`; `:527`)

Nenhum resultado de execução desta bateria foi produzido nesta auditoria; estado verde no ambiente atual: [nao verificado]. [nao verificado]

## 06/11 10:40 · sessao f6ffc8ffebe3 · - · - · origem=Ronald (tmux)

Sítios de escrita reais, nesta cópia:

| Ramo | Sítio |
|---|---|
| Disputa existente: agrega contexto/motivos antes de gerar perguntas | `app/chamados/services/disputa_emissao.py:501` — `disputa_aberta.save(...)` |
| Disputa nova: abre/reutiliza pai | `app/chamados/services/disputa_emissao.py:504` — `ChamadoColaborador.abrir(...)` |
| Pai novo | `app/chamados/models.py:932` — `cls.objects.get_or_create(...)` |
| Disputa nova | `app/chamados/services/disputa_emissao.py:523` — `DisputaSupervisao.objects.create(...)` |
| Pergunta existente reapontada | `app/chamados/models.py:2474` — `ex.save(...)` |
| Pergunta nova | `app/chamados/models.py:2476` — `cls.objects.create(...)` |
| Descarimbo anterior à emissão | `app/chamados/services/relavra_via.py:231` → `app/chamados/models.py:2652` — `PerguntaDisputa.save(...)` |
| Pauta do limbo exportado | `app/pautas/services.py:180` — `Pauta.objects.create(...)` |
| Usuário sistema ausente, no ramo de pauta | `app/core/utils.py:18` — `User.objects.get_or_create(...)` |

Atenções para a sonda:

- Patch apenas em `ChamadoColaborador.objects.create` **não intercepta necessariamente** o `get_or_create`: este usa o `QuerySet.create`; patch de `ChamadoColaborador.save` cobre o INSERT real. (`app/chamados/models.py:932`; funcionamento interno do Django: [nao verificado])
- A disputa existente sempre recebe `merge_contexto=contexto or {}` neste caminho, salva o contexto e só depois chama a geração da pergunta. Interromper nesse `save` prova entrada no escritor, não que a pergunta passaria pelas guardas posteriores. (`app/chamados/services/disputa_emissao.py:479`, `:501`, `:603`, `:618`)
- Uma sentinela derivada diretamente de `BaseException` evita que os `except Exception` internos a engulam. Há captura desse tipo na emissão/reatação e na leitura da ata. (`app/chamados/services/disputa_emissao.py:411`, `:828`)
- Se houver candidata de descarimbo, o emissor pode tentar essa escrita **antes** da emissão. Não deve contabilizar o descarimbo como pergunta/chamado emitível. (`app/chamados/services/fabrica_da_celula.py:164`, `:166`, `:174`)
- `SELECT FOR UPDATE` ocorre antes das escritas na disputa e no descarimbo. Permissão do papel LEITOR para executá-lo: [nao verificado]. (`app/chamados/services/disputa_emissao.py:467`; `app/chamados/services/relavra_via.py:221`)
- O ramo real da pauta chama `autor_sistema` antes do `Pauta.objects.create`; logo também existe possível escrita na obtenção do usuário sistema. (`app/chamados/services/fabrica_da_celula.py:236`; `app/pautas/services.py:87`; `app/core/utils.py:18`)

Há outro emissor do chamado no cartório: `julgar_celula` instancia `detectar_ausencias.Command` e o corpo chama seu `_abrir_chamado`. A fábrica da pergunta roda depois da lavra, apenas nas células julgadas naquele ato. (`app/ponto/services/cartorio.py:1964`, `:1990`, `:1740`, `:1778`, `:981`, `:997`)

## 06/11 10:40 · sessao 963cba183f13 · - · - · origem=Ronald (tmux)

A contagem declarada é **8 `EM_CURSO` + 1 `ESPERA_RONALD` = 9 itens não `FEITO`**. O placar efetivo rebaixa `FEITO` sem prova, mas os itens `FEITO` lidos têm provas preenchidas; a regra está em `app/core/espelho_verdade.py:199-203`.

| Item | Estado/local | Número do R1 que atualiza; limite |
|---|---|---|
| E1-passivo-53 | `EM_CURSO`, `app/core/espelho_verdade.py:36-47` | Não mede. `escala/services/vigencia_impossivel.py:23-24` oferece `medir()` somente leitura e devolve vínculos/colabs/ativos/lavrados em `:48-53`. |
| E1-sem-previsao | `EM_CURSO`, `app/core/espelho_verdade.py:48-49` | Classe `esp_zero_e6_trabalho` (`app/ponto/management/commands/e6_oraculo.py:358-361`), proxy de zero no espelho com trabalho no oráculo. Não certifica previsão válida; a medição específica é `escala/services/previsto_cego.py:114-115`, que usa `janela_atual` em `:138` e cobra ativo com par e nenhum previsto (`:142-174`). |
| E3-turno-aberto | `EM_CURSO`, `app/core/espelho_verdade.py:81-82` | `dia_batida_impar`, batem/divergem e `impar_FORA_da_faixa` (`e6_oraculo.py:347-349`, `:402-412`). São paridade/faixa do oráculo, não censo exato de turnos abertos. |
| E4-rotulo-do-desenho | `EM_CURSO`, `app/core/espelho_verdade.py:130-131` | R1 não mede; verificar leitor da pílula/selo de UI. **Prova do placar envelhecida:** `app/templates/escala/tipos_lista.html:55` já usa `{{ t\|desenho_do_turno }}` e cita O74 e seu selo. |
| E5-le-da-celula | `EM_CURSO`, `app/core/espelho_verdade.py:138-139` | R1 não mede proveniência do fechamento nem conclusão da troca de leitores; a pendência declarada é “falta status encerrado” (`:139`). |
| E6-sistema-bate | `EM_CURSO`, `app/core/espelho_verdade.py:168-169` | `BATE`/dias comparados (`e6_oraculo.py:398-399`) e ESTRUTURA dias/horas/colabs (`:422-423`). |
| E6-exportavel | `EM_CURSO`, `app/core/espelho_verdade.py:172-173` | `colabs com divergencia` (`e6_oraculo.py:429`) e colabs por dono (`:423`). R1 10/11 não mede elegibilidade real do export nem a 09 citada no título do item. |
| E6-piso-185 | `EM_CURSO`, `app/core/espelho_verdade.py:178-179` | `SOMA` donos/classes (`e6_oraculo.py:424-427`) e ESTRUTURA dias/horas/colabs. **Não fecha causa:** o próprio placar exige “falta causa nomeada nos ... ESTRUTURA” (`espelho_verdade.py:179`). |
| E3-separados-30 | `ESPERA_RONALD`, `app/core/espelho_verdade.py:83-84` | Não mede: é universo separado da 09 e situação de migração/aplicação; o placar declara a 09 sob responsabilidade externa (`:84`). |

O backlog pede explicitamente R1 para **cinco itens**: E1-sem-previsao, E3-turno-aberto, E6-sistema-bate, E6-exportavel e E6-piso-185, e pede confirmar E6-batida-impar; este último já é `FEITO` (`app/docs/BACKLOG.md:1478`; `app/core/espelho_verdade.py:176-177`). A confirmação usa as linhas de ímpares e paridade (`app/ponto/management/commands/e6_oraculo.py:402-412`).

`app/docs/ESTRUTURAL.md:34-42` estabelece o dono único, o juiz `dono_da_divergencia`, o destino CADASTRO/BATIDA no admin e a separação do placar principal R1–R6 e ESPELHO-VERDADE. `app/core/placar_estrutural.py:115-138` declara a fonte `logs/e6_cauda2c/r1_dono_*.txt`, cadência, dias/horas/colabs e tabela impressa pelo próprio comando. `:105-113` exige a soma obtida no comando, sem sonda paralela.

## 06/11 10:40 · sessao 27a8fa81a908 · - · - · origem=Ronald (tmux)

- **V3-MOTOR-FERIADO-HE100-NOTURNA** está na fila e exige `!`; o registro relata HE100 noturna zero em feriado com HE100 e noturnas positivas. Isso é relato do backlog, sem reexecução nesta auditoria. (`app/docs/BACKLOG.md:937`)
  - No `MotorTurnoPartido`, as HE noturnas são zeradas antes do cálculo mensal; o ramo de dobra preenche HE100 e sai por `continue`, enquanto `_aplicar_he_noturna` aparece depois. (`app/ponto/motor_calculo_v2.py:2937`, `:3092`, `:3101`, `:3147`)
  - No `MotorComercial`, ocorre a mesma estrutura: zera os splits, preenche a dobra e sai antes da chamada de HE noturna. (`app/ponto/motor_calculo_v2.py:3323`, `:3337`, `:3352`, `:3399`)
  - Qual desses ramos explica cada um dos dez colaboradores registrados: **[nao verificado]**.

- **V3-NOTURNO-RECORTE-LONGO** espera `!` e está explicitamente fora do escopo V3-P5. (`app/docs/BACKLOG.md:939`)
  - `minutos_noturnos` devolve zero acima de 26 horas. (`app/ponto/motor_calculo_v2.py:93`)
  - Com prorrogação, estende `s1` até `fim` e soma separadamente `s2` de 22–24; portanto pode contar a sobreposição duas vezes. (`app/ponto/motor_calculo_v2.py:110`, `:112`, `:116`)
  - `MotorPlantao.calcular_periodo` delega a `Motor12x36`, que escreve noturnas pela autoridade que chama `minutos_noturnos`. (`app/ponto/motor_calculo_v2.py:3510`, `:2792`, `:983`)

- **Rubricas diagnósticas para a lista:** `horas_extras_100_noturna`, contextualizada por `horas_extras_100`, `horas_extras_100_feriado` e `horas_noturnas`, pode localizar a assinatura conhecida do feriado. (`app/docs/BACKLOG.md:937`; nomes dos campos: `app/ponto/services/dia_pago.py:35`)
  - `horas_noturnas`, `horas_extras_50_noturna` e `horas_extras_100_noturna` podem expor efeitos do recorte longo; HE50/100 também podem mudar quando a régua legal inclui o ganho noturno na base do excedente. (`app/ponto/motor_calculo_v2.py:983`, `:441`, `:3123`)
  - Há **cegueira compartilhada**: a v3 também chama `minutos_noturnos`; igualdade no diff não elimina o defeito comum. (`app/ponto/calculador/regras.py:85`; `app/ponto/calculador/periodo.py:510`)
  - Divergências fora dessas assinaturas são candidatos a investigação, não comprovação de bug novo. L-110 define a comparação com o código velho como **impacto**, nunca validação. (`app/docs/LEIS.md:136`)

## 06/11 10:40 · sessao fb0ce3409b50 · - · - · origem=Ronald (tmux)

Lei geral: `CLAUDE.md:166–170` faz célula/ata a lâmpada, evento o emissor, consumidor leitor. `app/docs/CORTES.md:263` explicita “lampada acesa no marco = fio vivo obrigatorio”; não transforma toda recusa em violação, porque há exceções escritas posteriores.

Guardas de `motivos_a_perguntar`, em ordem:

1. `dia_sem_trabalho(cel)` e colaborador inativo: `app/chamados/services/fabrica_da_celula.py:91`, retorno `:92`. O juiz é `app/chamados/juizes.py:413`, predicado `:426–428`. Dia sem trabalho tem cobertura expressa em `app/docs/CORTES.md:416`: `trabalha=False` ou tipo lavrado diferente de trabalho; `None` cobra. Não viola 4a quando a própria lâmpada diz ausência de trabalho. Inativo tem piso por domínio RH documentado/testado em `app/chamados/tests/test_selo_f3_celula_soberana.py:19–25`, `:86–103`; não encontrei corte nomeado nesta leitura que autorize especificamente **todo** status diferente de `ativo`, então ampliação além de desligado [nao verificado].

2. Dia futuro: fábrica `:93–94`. Teto temporal escrito em `CLAUDE.md:185`; `CORTES.md:263` exclui hoje por teto na formulação histórica. A fábrica atual trabalha por instante e inclui hoje quando marco venceu: docstring fábrica `:8–11` e autoridade `vencidos_no_instante :15–31`. Recusa de marco ainda não vencido é coberta; não há violação demonstrada.

3. Veredito preenchido fora de `VEREDITOS_ACUSAM`: fábrica `:99–100`. Obedece à lâmpada/consumidor em `CLAUDE.md:167–170`; não há obrigação de perguntar célula absolvida.

4. `parcial_so_lista`: fábrica `:101–104`; juiz real `app/ponto/services/cartorio.py:370–400`. Duas hipóteses documentadas `:378–387`: parcial sem período sempre lista do admin; parcial com período sem fato novo desde julgamento também lista. Corte explícito `app/docs/CORTES.md:319`, complemento da resposta `nao_sei :334`; aval literal “parcial sem periodo ... nunca chamado” está no código `cartorio.py:381–382`. Portanto não imputar quebra geral da 4a a esta recusa: é exceção escrita. Causa dos 200 por ela [nao verificado].

5. Construção dos motivos e dedup: fábrica `:118–126`. Dedup elimina todo motivo com pergunta histórica que não seja `VIA_DUPLICATA`, sem testar aqui se viva. Se essa for a causa medida de célula acusadora sem fio vivo, a lei que confronta é `CLAUDE.md:168–170` + `CORTES.md:263`; não há nesta leitura lei que autorize substituir **fio vivo** por mera existência histórica.

Guardas internas de `motivos_pela_ata`:

- Vencidos vazio: fábrica `:45–46`, coberto pelo teto `CLAUDE.md:185`.
- `_ata_do_dia` nega: fábrica `:49–51`. Autoridade real `disputa_emissao.py:818–844`; nega somente sem célula com escala vigente (`:833–843`). Docstring `:819–823` exige lavra antes de questionário. Se lâmpada acusadora existente e emissor negar por leitura inconsistente, confronto com `CLAUDE.md:167–170`; caso concreto [nao verificado].
- Sem ata e teto por marcos: fábrica `:52–54`; aguarda porque não há marco para comparar; lei específica além de teto/lâmpada [nao verificado].
- Sem ata e sem marcos passados: chama `classificar_falta` e pode obter nenhum motivo, fábrica `:55–57`. Se isso cala célula/ata que acusa, é consumidor rejulgando contra `CLAUDE.md:170`; ocorrência [nao verificado].
- Filtra faltantes por vencimento, data, mapeamento e unicidade: fábrica `:59–65`. Vencimento tem teto; identidade/data preservam marco do dia. Mapeamento ausente que silencie marco acusador confronta `CORTES.md:263`, ocorrência [nao verificado].

Depois dos motivos, `perguntar_pela_celula`:

- Limbo exportado vai à pauta em vez de pergunta: fábrica `:171–173`; juiz `limbo_vai_a_pauta :180–188`; pauta `:225–242`. Exceção expressa comentada fábrica `:167–170` e aval em `app/docs/PROMPTS.md:1719`. Não confundir com fronteira geral exportada, abolida por `app/docs/CORTES.md:219` e `app/docs/LEIS.md:91`.
- Pauta já existente impede duplicata: fábrica `:208–222`, `:234–236`; idempotência escrita em `CLAUDE.md:175` e fábrica `:228–230`.

Atenção ao contrato de erros: `_ata_do_dia` **engole** erro real de `_marcos_faltantes` em `app/chamados/services/disputa_emissao.py:828–832`, registrando por `core.observ.registrar_engolido` e retornando `(True, None)`. Sonda deve observar essa chamada/registrador por mock, contar o erro e marcar incompleto; retorno normal do emissor não garante ausência de erro.

## 06/11 10:35 · sessao 63c4f7796b6e · PEP-REENTRADA · verificador · origem=Code

Parecer estático: execução dos testes, dos mutantes e comportamento em produção `[nao verificado]`. [nao verificado]

**1. P1–P4: cura e mutantes sobreviventes**

| Item | Parecer |
|---|---|
| **P1** | **Cura implementada para observar entrada no serviço durante o voo:** o espia registra `pergunta_em_voo(p.pk)` e exige pelo menos um `True`. Ele não identifica exclusivamente o receiver que originou a chamada. `app/chamados/tests/test_pep_reentrada.py:90`, `app/chamados/tests/test_pep_reentrada.py:97`, `app/chamados/tests/test_pep_reentrada.py:99`. |
| **P2** | **Curado quanto ao estado persistido do trio nos dois alvos**, mas **parcial quanto à exigência “dentro do voo”**: sem credor verifica depois de sair do `with`; S144 verifica ainda dentro. `app/chamados/tests/test_pep_reentrada.py:253`, `app/chamados/tests/test_pep_reentrada.py:256`, `app/chamados/tests/test_pep_reentrada.py:259`, `app/chamados/tests/test_pep_reentrada.py:274`, `app/chamados/tests/test_pep_reentrada.py:278`. |
| **P3** | **O mutante original passa a ser distinguido:** ambas sem destino atravessariam `if q.chamado_destino_id: continue`, inclusive a P protegida; a exigência de P sem carimbo o reprovaria. A alteração, porém, muda o ingresso da disputa em `_cands`. `app/chamados/tests/test_pep_reentrada.py:244`, `app/chamados/tests/test_pep_reentrada.py:250`, `app/chamados/tests/test_pep_reentrada.py:259`, `app/chamados/services/disputa_emissao.py:1652`. |
| **P4** | **Curado:** C7 e C9 agora coincidem com registro e mapa. `app/docs/JUIZES.md:54`, `app/docs/JUIZES.md:55`, `app/core/juizes.py:267`, `app/core/juizes.py:269`, `app/docs/MAPA-JUIZES.md:97`, `app/docs/MAPA-JUIZES.md:105`. |

Mutantes de produção que os testes curativos ainda não distinguem — sobrevivência inferida pela leitura; execução `[nao verificado]`: [nao verificado]

- **P1:** substituir o corpo do resolver por retorno de contadores zerados. O espia continuaria registrando entrada em voo; o próprio materializador fecharia o destino e produziria seu carimbo. O teste exige entrada, destino fechado, batida e um `carimbar` bem-sucedido pela via da declaração, sem exigir efeito exclusivo do serviço observado. `app/chamados/tests/test_pep_reentrada.py:99`, `app/chamados/tests/test_pep_reentrada.py:101`, `app/chamados/tests/test_pep_reentrada.py:103`, `app/chamados/tests/test_pep_reentrada.py:105`, `app/chamados/services/materializacao.py:949`, `app/chamados/services/materializacao.py:1251`.
- **P2:** nas guardas de voo dos dois alvos, executar `descarimbar()` antes de `continue`. Haveria escrita indevida do trio, mas os valores continuariam exatamente os vazios esperados. Portanto, conferir estado intacto não prova ausência de escritor. `app/chamados/services/disputa_emissao.py:1671`, `app/chamados/services/materializacao.py:1399`, `app/chamados/models.py:2646`, `app/chamados/models.py:2652`, `app/chamados/tests/test_pep_reentrada.py:260`, `app/chamados/tests/test_pep_reentrada.py:279`.
- **P3:** trocar a guarda por `if pergunta_em_voo(q.pk) and not q.chamado_destino_id: continue`. O novo caso permanece protegido, mas perde-se a proteção da P em voo **com destino** no laço sem credor. O laço admite destinos encerrados; só um destino vivo barra a disputa. `app/chamados/tests/test_pep_reentrada.py:250`, `app/chamados/services/disputa_emissao.py:1662`, `app/chamados/services/disputa_emissao.py:1671`.

**2. O espia de P1 está no nome correto?**

**Sim.** O receiver faz import local de `chamados.models.resolver_perguntas_por_chamado_resolvido` e chama esse nome; é precisamente o atributo substituído pelo teste. Não há outro ramo do próprio receiver que chame o resolver por um nome diferente. `app/chamados/signals.py:89`, `app/chamados/signals.py:92`, `app/chamados/tests/test_pep_reentrada.py:97`.

Há outros **chamadores do serviço**, também pelo nome observado: `_retratar`, reatamento de órfã e fechamento de beco. Eles não escapam do espia; podem, contudo, tornar ambígua a atribuição “foi o receiver S143”. Quais efetivamente chamam durante esse fixture, inclusive com S143 desligado, `[nao verificado]`. `app/chamados/reconciliador.py:508`, `app/chamados/reconciliador.py:536`, `app/chamados/reconciliador.py:1528`, `app/chamados/reconciliador.py:1631`, `app/chamados/services/becos.py:40`, `app/chamados/services/becos.py:49`.

O registro ocorre **antes** de executar o serviço, e o receiver engole exceções: o `True` comprova entrada, não conclusão bem-sucedida. `app/chamados/tests/test_pep_reentrada.py:91`, `app/chamados/tests/test_pep_reentrada.py:92`, `app/chamados/signals.py:93`.

**3. P3 mantém o caminho anterior?**

**Mantém o laço, mas muda o caminho de entrada.** Antes, a P destinada também colocava a disputa no conjunto derivado de `perguntas`; agora ambas estão sem destino, o filtro inicial fica vazio para elas e a disputa entra pela consulta das abertas do colaborador. `app/chamados/tests/test_pep_reentrada.py:244`, `app/chamados/tests/test_pep_reentrada.py:250`, `app/chamados/services/disputa_emissao.py:1572`, `app/chamados/services/disputa_emissao.py:1652`, `app/chamados/services/disputa_emissao.py:1653`.

Pai resolvido e validações nulas permitem alcançar `_abertas`; o controle da irmã exige que alguma escrita realmente aconteça nesse laço. Portanto, a nova configuração não torna a guarda inatingível. `app/chamados/tests/test_pep_reentrada.py:246`, `app/chamados/tests/test_pep_reentrada.py:251`, `app/chamados/tests/test_pep_reentrada.py:258`, `app/chamados/services/disputa_emissao.py:1659`, `app/chamados/services/disputa_emissao.py:1665`.

O limite de realismo continua relevante: o fixture mantém texto, apaga resposta/validação, declara voo manualmente e força C6 a “morta”. A porta normal seleciona perguntas com resposta e validação gravadas; com C6 real, o próprio docstring declara que a irmã não receberia o carimbo. Isso prova a guarda isolada e perde cobertura do ingresso anterior por destino; não demonstra a cadeia real nesse estado. `app/chamados/tests/test_pep_reentrada.py:238`, `app/chamados/tests/test_pep_reentrada.py:246`, `app/chamados/tests/test_pep_reentrada.py:253`, `app/chamados/services/materializacao.py:1343`.

**4. P4: correspondência documental**

| Autoridade | JUIZES | Registro | MAPA |
|---|---|---|---|
| C7: `DisputaSupervisao.pode_fechar` | `app/docs/JUIZES.md:54` | `app/core/juizes.py:267` | `app/docs/MAPA-JUIZES.md:97` |
| C9: `catalogo/motor.py::prazo_estourou` | `app/docs/JUIZES.md:55` | `app/core/juizes.py:269` | `app/docs/MAPA-JUIZES.md:105` |

**5. Classes de reprovação no delta**

| Classe | Resultado da leitura |
|---|---|
| **Segundo juiz** | O voo é pertencimento por PK condicionado a atomic aberto; não recalcula vitalidade nem fechamento. C6 continua na porta do carimbo e C7 na cauda. `app/chamados/services/materializacao.py:1129`, `app/chamados/models.py:2596`, `app/chamados/services/disputa_emissao.py:1712`. |
| **Segundo escritor fora da porta** | As guardas novas apenas pulam os candidatos. Entretanto, o espia principal conta somente chamadas de `carimbar` que retornam `True`: uma escrita direta seguida de sobrescrita pelo materializador pode escapar dessa contagem. `app/chamados/services/disputa_emissao.py:1586`, `app/chamados/services/materializacao.py:1399`, `app/chamados/tests/test_pep_reentrada.py:69`, `app/chamados/tests/test_pep_reentrada.py:105`. |
| **Relógio fora da borda** | O sinal novo não usa relógio; o fixture congela o instante. `app/chamados/services/materializacao.py:1115`, `app/chamados/services/materializacao.py:1129`, `app/chamados/tests/test_pep_reentrada.py:35`. |
| **Derivação paralela** | O sinal guarda PKs; os carimbos continuam delegando aos juízes existentes. `app/chamados/services/materializacao.py:1120`, `app/chamados/services/materializacao.py:1207`, `app/chamados/models.py:2596`. |
| **Selo sem mordida** | Há controles positivos, mas persistem os limites de origem do espia, estado versus escrita e mudança de caminho de P3 descritos acima. `app/chamados/tests/test_pep_reentrada.py:99`, `app/chamados/tests/test_pep_reentrada.py:258`, `app/chamados/tests/test_pep_reentrada.py:281`. |
| **Ordem de trava** | A mudança do bloco acrescenta o contexto depois do atomic; o contexto só manipula `ContextVar`. Validação concorrente da ordem de travas `[nao verificado]`. `app/chamados/services/materializacao.py:1194`, `app/chamados/services/materializacao.py:1120`. |
| **Pouso antes do pré-requisito** | O atomic entra antes do voo; declarar voo fora de atomic levanta erro. O carimbo normal permanece depois do plantio e do juiz do lastro. `app/chamados/services/materializacao.py:1118`, `app/chamados/services/materializacao.py:1194`, `app/chamados/services/materializacao.py:1207`, `app/chamados/services/materializacao.py:1251`. |
| **Estado de módulo que sobrevive** | Existe `ContextVar` de módulo, mas o valor anterior é restaurado por token em `finally`; há testes de rollback, exceção e aninhamento. Passagem desses testes `[nao verificado]`. `app/chamados/services/materializacao.py:1106`, `app/chamados/services/materializacao.py:1124`, `app/chamados/tests/test_pep_reentrada.py:150`, `app/chamados/tests/test_pep_reentrada.py:167`, `app/chamados/tests/test_pep_reentrada.py:179`. |

**6. Quem escreve e quem lê o trio dentro do voo?**

**Nos caminhos atuais examinados, não identifiquei escritor da P além do materializador. Essa garantia está nos caminhos, não nas portas genéricas.** `app/chamados/services/materializacao.py:1251`, `app/chamados/services/disputa_emissao.py:1586`, `app/chamados/services/disputa_emissao.py:1671`, `app/chamados/services/materializacao.py:1399`, `app/chamados/models.py:2625`, `app/chamados/models.py:2646`.

| Papel | Caminho |
|---|---|
| **Escritor normal** | `_materializar_uma → carimbar → _escrever_carimbo → save`, atribuindo o trio junto. `app/chamados/services/materializacao.py:1251`, `app/chamados/models.py:2556`, `app/chamados/models.py:2609`. |
| **Escritor alternativo do próprio materializador** | A recusa por paridade usa `carimbar_identidade` e retorna `None`, impedindo o carimbo normal posterior. `app/chamados/services/materializacao.py:899`, `app/chamados/services/materializacao.py:900`, `app/chamados/services/materializacao.py:1201`. |
| **S143/S144** | Os dois laços de S143 pulam a P; sua cauda não fecha disputa com P em voo; S144 também pula antes de carimbar. `app/chamados/services/disputa_emissao.py:1586`, `app/chamados/services/disputa_emissao.py:1671`, `app/chamados/services/disputa_emissao.py:1710`, `app/chamados/services/materializacao.py:1399`. |
| **Reconciliador** | A batida o aciona sincronamente. A absolvição por célula pode carimbar, mas exige validação nula, excluindo a P selecionada pelos chamadores atuais; a lavra sem turno exige ausência de texto. `app/ponto/signals.py:215`, `app/chamados/reconciliador.py:586`, `app/chamados/reconciliador.py:620`, `app/chamados/services/materializacao.py:1345`, `app/chamados/services/vigia_carimbo.py:98`, `app/chamados/reconciliador.py:364`. |
| **Dedup/descarimbo** | Migração considera perguntas vivas, enquanto C6 elimina imediatamente a validada; ressuscitação exige carimbo existente e texto vazio. `app/chamados/services/fio.py:935`, `app/chamados/juizes.py:1136`, `app/chamados/models.py:1434`. |
| **Leitores** | A porta fotografa o trio anterior; C6 e lastro leem materialização/via; S143, S144 e irmãs sem consumo usam materialização nos filtros. O autor é preservado no snapshot. `app/chamados/models.py:2583`, `app/chamados/juizes.py:1171`, `app/chamados/juizes.py:647`, `app/chamados/services/disputa_emissao.py:1574`, `app/chamados/services/materializacao.py:1393`, `app/chamados/juizes.py:1748`. |

A recusa por colisão no `finally` pode escrever identidade **depois da saída do voo**; ela não constitui outro escritor dentro desse bloco. Já `carimbar_identidade` e `descarimbar` continuam sem consultar o voo: ampliar os chamadores para estados como o fixture de P3 exigiria nova análise de alcançabilidade. `app/chamados/services/materializacao.py:1074`, `app/chamados/services/materializacao.py:1261`, `app/chamados/services/materializacao.py:1271`, `app/chamados/models.py:2625`, `app/chamados/models.py:2646`, `app/chamados/tests/test_pep_reentrada.py:234`.

## 06/11 10:33 · sessao 083c8ea8b2b0 · - · - · origem=Ronald (tmux)

No bloco atual, não encontrei escritor do trio da P além do materializador; a garantia depende dos caminhos atuais, pois as portas genéricas não consultam o voo (`app/chamados/services/materializacao.py:1194`, `app/chamados/models.py:2562`, `app/chamados/models.py:2625`, `app/chamados/models.py:2646`).

**Quem escreve:**

- O carimbo normal sai de `_materializar_uma → p.carimbar`, após o juiz do lastro; a porta atribui os três campos e grava (`app/chamados/services/materializacao.py:1207`, `app/chamados/services/materializacao.py:1251`, `app/chamados/models.py:2556`, `app/chamados/models.py:2609`).
- O próprio ramo hora pode escrever identidade por paridade e retornar imediatamente, impedindo o carimbo normal; continua sendo o materializador (`app/chamados/services/materializacao.py:899`, `app/chamados/services/materializacao.py:900`, `app/chamados/services/materializacao.py:1201`).
- A recusa `COLISAO` pode escrever identidade no `finally`, mas esse `finally` roda após a saída de `materializacao_em_voo`; portanto não é outro escritor **dentro** do voo (`app/chamados/services/materializacao.py:1074`, `app/chamados/services/materializacao.py:1261`, `app/chamados/services/materializacao.py:1271`).

**Outros candidatos e por que não escrevem a P atual:**

- S143 pula a P nos dois laços e bloqueia o fecho da disputa com qualquer P em voo (`app/chamados/services/disputa_emissao.py:1586`, `app/chamados/services/disputa_emissao.py:1671`, `app/chamados/services/disputa_emissao.py:1710`).
- S144 pula antes do juiz e do carimbo (`app/chamados/services/materializacao.py:1399`, `app/chamados/services/materializacao.py:1405`).
- A batida chama sincronamente `reconciliar_chamados_colab`; sua absolvição por célula carimba sem consultar voo, mas exige `validada_em IS NULL`. A porta normal seleciona validação não nula; a validação individual grava antes de materializar; o passivo também seleciona validação não nula (`app/ponto/signals.py:206`, `app/ponto/signals.py:215`, `app/chamados/reconciliador.py:586`, `app/chamados/reconciliador.py:620`, `app/chamados/services/materializacao.py:1343`, `app/chamados/services/validacao.py:396`, `app/chamados/services/validacao.py:424`, `app/chamados/services/vigia_carimbo.py:98`).
- `lavrar_sem_turno_aberto` exige ausência de texto, incompatível com hora que chega ao plantio e com `data_hora` parseável (`app/chamados/reconciliador.py:364`, `app/chamados/reconciliador.py:405`, `app/chamados/services/materializacao.py:804`, `app/chamados/services/materializacao.py:970`).
- Dedup só processa perguntas vivas; o C6 dá por morta imediatamente a P validada (`app/chamados/services/fio.py:931`, `app/chamados/services/fio.py:935`, `app/chamados/juizes.py:1136`).
- Ressuscitação/descarimbo filtra pergunta já materializada e sem texto; a P hora em voo tem texto (`app/chamados/models.py:1434`, `app/chamados/models.py:1437`, `app/chamados/models.py:1504`, `app/chamados/services/materializacao.py:804`).
- Re-lavra de via não está ligada ao sinal da batida; `_gravar_estado` re-lavra somente o veredito, não o trio (`app/ponto/signals.py:94`, `app/chamados/models.py:291`, `app/chamados/veredito_lavrado.py:227`).

**Quem lê:**

- `carimbar` fotografa o trio anterior e escreve pela porta única; C6 e juiz do lastro leem materialização/via; a lavra delega ao juiz do lastro (`app/chamados/models.py:2583`, `app/chamados/models.py:2586`, `app/chamados/juizes.py:1145`, `app/chamados/juizes.py:1171`, `app/chamados/juizes.py:1177`, `app/chamados/juizes.py:1207`, `app/chamados/juizes.py:647`, `app/chamados/juizes.py:673`, `app/chamados/veredito_lavrado.py:219`).
- Os universos de S143, S144, irmãs sem consumo e reconciliador filtram `materializada_em`; o autor é preservado no snapshot e atribuído pela porta, sem participar desses predicados (`app/chamados/services/disputa_emissao.py:1574`, `app/chamados/services/disputa_emissao.py:1666`, `app/chamados/services/materializacao.py:1393`, `app/chamados/juizes.py:1748`, `app/chamados/reconciliador.py:619`, `app/chamados/models.py:2583`, `app/chamados/models.py:2559`).

O limite estrutural merece constar: uma chamada nova de `_materializar_uma` com validação nula poderia entrar no universo do reconciliador, porque ele não consulta voo; os chamadores atuais evitam isso pelos filtros acima, e o próprio teste P3 declara esse limite (`app/chamados/reconciliador.py:620`, `app/chamados/services/materializacao.py:1132`, `app/chamados/tests/test_pep_reentrada.py:234`).

## 06/11 10:33 · sessao d90ff57f7b2e · - · - · origem=Ronald (tmux)

- **Item 2:** o patch de P1 entra no nome correto: o teste troca `chamados.models.resolver_perguntas_por_chamado_resolvido` em `app/chamados/tests/test_pep_reentrada.py:97`; o receiver faz import local desse mesmo nome e o chama em `app/chamados/signals.py:89` e `app/chamados/signals.py:92`; o reexport vem de `app/chamados/models.py:3281`. Não há outro ramo do próprio receiver que execute o resolver sem passar por esse alias (`app/chamados/signals.py:81`, `app/chamados/signals.py:88`).

- **Limite de P1:** o espia comprova entrada no serviço durante o voo, mas não identifica qual chamador entrou nem comprova conclusão do serviço: só registra `pergunta_em_voo(p.pk)` antes de chamar o original (`app/chamados/tests/test_pep_reentrada.py:90`); o receiver engole exceções (`app/chamados/signals.py:93`). Há chamadas diretas pelo mesmo alias, também vistas pelo espia, em `app/chamados/reconciliador.py:508`, `app/chamados/reconciliador.py:536`, `app/chamados/reconciliador.py:1528`, `app/chamados/reconciliador.py:1631`, `app/chamados/services/becos.py:40` e `app/chamados/services/becos.py:49`. Quais efetivamente acontecem no fixture: `[nao verificado]`.

- **Mutante sobrevivente de P1, no teste que o cura:** substituir o serviço por retorno `{'perguntas_resolvidas': 0, 'disputas_fechadas': 0}` preservaria a chamada observada em voo e deixaria o materializador produzir seu carimbo único; o método exige apenas ocorrência de `True`, destino fechado, batida presente e uma gravação com `VIA_DECLARACAO` (`app/chamados/tests/test_pep_reentrada.py:99`, `app/chamados/tests/test_pep_reentrada.py:101`, `app/chamados/tests/test_pep_reentrada.py:103`, `app/chamados/tests/test_pep_reentrada.py:105`; carimbo do materializador em `app/chamados/services/materializacao.py:1251`). Resultado executado do mutante: `[nao verificado]`.

- **Item 3:** P3 continua atingindo o mesmo laço-alvo, mas **não pelo mesmo ingresso em `_cands`**: antes P apontava para `destino`; agora remove a FK em `app/chamados/tests/test_pep_reentrada.py:250`. Como a irmã também nasce sem destino (`app/chamados/tests/test_pep_reentrada.py:244`), o filtro `chamado_destino=chamado` fica sem essas perguntas (`app/chamados/services/disputa_emissao.py:1572`). A disputa entra pela busca de todas as abertas do colaborador (`app/chamados/services/disputa_emissao.py:1653`), enquanto antes também entrava pelo conjunto derivado de `perguntas` (`app/chamados/services/disputa_emissao.py:1652`). Pai resolvido e validações nulas satisfazem os filtros do laço (`app/chamados/tests/test_pep_reentrada.py:246`, `app/chamados/tests/test_pep_reentrada.py:251`; filtros em `app/chamados/services/disputa_emissao.py:1659`, `app/chamados/services/disputa_emissao.py:1665`).

- **P3 mata o mutante original:** trocar a guarda por `if q.chamado_destino_id: continue` deixaria ambas passarem, inclusive P em voo, contrariando a exigência de P sem carimbo (`app/chamados/tests/test_pep_reentrada.py:250`, `app/chamados/tests/test_pep_reentrada.py:259`; ponto de mutação em `app/chamados/services/disputa_emissao.py:1671`). Execução: `[nao verificado]`.

- **Mutante sobrevivente de P3:** `if pergunta_em_voo(q.pk) or q.chamado_destino_id: continue` mantém o teste verde porque ambas estão sem destino, mas passa a pular indevidamente irmãs fora de voo com destino morto; esse universo é aceito pelo laço de produção, que só exclui a disputa quando existe destino **vivo** (`app/chamados/services/disputa_emissao.py:1662`, `app/chamados/services/disputa_emissao.py:1671`; fixture em `app/chamados/tests/test_pep_reentrada.py:244`, `app/chamados/tests/test_pep_reentrada.py:250`). Execução: `[nao verificado]`.

- **Realismo de P3:** o limite de C6 mockado está agora declarado explicitamente (`app/chamados/tests/test_pep_reentrada.py:238`); a modificação adicional perde cobertura do ingresso por pergunta com destino, mas não impede atingir a guarda do laço sem credor (`app/chamados/services/disputa_emissao.py:1653`, `app/chamados/services/disputa_emissao.py:1671`). O caso continua sintético: remove carimbos de resposta/validação mantendo o texto e declara voo manualmente (`app/chamados/tests/test_pep_reentrada.py:245`, `app/chamados/tests/test_pep_reentrada.py:246`, `app/chamados/tests/test_pep_reentrada.py:254`); portanto sua evidência é da guarda, não da cadeia real de materialização (`app/chamados/tests/test_pep_reentrada.py:239`).

## 06/11 10:33 · sessao 8b3b6fa3ee26 · - · - · origem=Ronald (tmux)

P4 está curado: `app/docs/JUIZES.md:54` aponta C7 para `DisputaSupervisao.pode_fechar`, como `app/core/juizes.py:267` e `app/docs/MAPA-JUIZES.md:97`; SLA/C9 em `JUIZES.md:55` coincide com `core/juizes.py:269` e `MAPA-JUIZES.md:105`.

P2 está curado quanto ao **estado persistido do trio** nos dois cenários pedidos: sem credor verifica `materializada_em` e `(via_resolucao, materializada_por_id)` em `app/chamados/tests/test_pep_reentrada.py:259` e `:260`; S144 verifica em `:278` e `:279`. Contudo, sem credor lê depois de sair do bloco (`:253`, `:256`); somente S144 lê dentro (`:274`, `:276`). A afirmação literal “trio intacto dentro do voo nos dois testes” é mais forte que o observado no primeiro (`:253`, `:256`).

Mutante substantivo sobrevivente aos **dois testes curativos de P2**: estreitar as respectivas guardas para `if pergunta_em_voo(pk) and p.tipo_resposta == 'hora': continue`. Os fixtures são todos `hora` (`app/chamados/tests/test_pep_reentrada.py:57`, `:245`, `:270`), mas produção põe também `data_hora` e `escolha` em voo (`app/chamados/services/materializacao.py:1194`, `:1197`, `:1199`); as guardas atuais não restringem tipo (`app/chamados/services/disputa_emissao.py:1671`, `app/chamados/services/materializacao.py:1399`). Sobrevivência inferida estaticamente, sem executar mutação: `[nao verificado]`.

Outro limite: assertar o trio vazio não prova ausência de escrita. Um `descarimbar()` na guarda grava o mesmo trio vazio (`app/chamados/models.py:2646`, `:2652`), já esperado pelos testes (`app/chamados/tests/test_pep_reentrada.py:260`, `:279`); os valores iniciais vêm dos defaults (`app/chamados/models.py:2323`, `:2791`, `:2795`). Isso é uma limitação do selo por estado, sem demonstrar defeito no código atual (`app/chamados/services/disputa_emissao.py:1671`, `app/chamados/services/materializacao.py:1399`).

Nas classes solicitadas, o delta v4 acrescenta observação/assertions e corrige lookup, sem introduzir decisão de produção: `app/chamados/tests/test_pep_reentrada.py:90`, `:97`, `:250`, `:260`, `:279`; `app/docs/JUIZES.md:54`. No diff completo, o sinal não replica C6/C7: consulta apenas pertencimento e transação (`app/chamados/services/materializacao.py:1127`), enquanto carimbo e fecho continuam lendo os juízes existentes (`app/chamados/models.py:2596`, `app/chamados/services/disputa_emissao.py:1712`). O reset está em `finally`, com token (`app/chamados/services/materializacao.py:1120`, `:1123`); há cobertura explícita de rollback, exceção e aninhamento (`app/chamados/tests/test_pep_reentrada.py:150`, `:167`, `:179`). O bloco entra primeiro no atomic e depois declara voo (`app/chamados/services/materializacao.py:1194`); a declaração exige atomic aberto (`:1118`). Não há evidência fornecida de execução dos testes ou de validação concorrente de ordem de travas: `[nao verificado]`.

## 06/11 10:32 · sessao 6d8557c6b3d4 · H12H06-B · sonda · origem=Code

Transcrição literal do dado fornecido [nao verificado]. Não executei o wrapper: ele escreve arquivos (`bin/sonda_leitura.sh:132`, `bin/sonda_leitura.sh:250`).

```text
sonda_leitura: pago_x_devido.py [b04491167a9f3c6b] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

col|status_fm|entregue|grav_ok|grav_perd|grav_reflexo|antes_diff(ok,perd,refl)|devido(ok,perd,refl)|grav==antes
col159|aberto|False|4|1|0.9943|(4, 1, None)|(5, 0, None)|True
col284|aprovado|True|4|1|0.1914|(4, 1, None)|(5, 0, None)|True
col342|aprovado|True|4|1|2.4046|(4, 1, 1.9195)|(5, 0, 3.2714)|False
col400|aprovado|True|4|1|1.2723|(4, 1, 0.9864)|(5, 0, 1.2125)|False
col433|aberto|True|2|3|0.0000|(2, 3, None)|(4, 1, None)|True
col842|aberto|True|3|1|1.3746|(3, 1, 1.3849)|(4, 0, 1.7164)|False
col905|aberto|True|0|1|0.0000|(0, 1, None)|(1, 0, None)|True
UNIVERSO 7 esperado 7
```

(1) UNIVERSO fecha: **sim, 7 esperado 7**, conforme impresso. [nao verificado]

(2) e (3), conforme impresso:

| col | grav==antes | entregue | Referência |
|---|---|---|---|
| col159 | True | False | [nao verificado] |
| col284 | True | True | [nao verificado] |
| col342 | False | True | [nao verificado] |
| col400 | False | True | [nao verificado] |
| col433 | True | True | [nao verificado] |
| col842 | False | True | [nao verificado] |
| col905 | True | True | [nao verificado] |

## 06/11 10:29 · sessao cf215076862f · H12H06-B · sonda · origem=Code

Não executei `dry_comp11.py`: o arquivo não foi localizado na cópia. Segue a saída literal fornecida no pedido. [nao verificado] [nao verificado]

```text
sonda_leitura: dry_comp11.py [6be44cbafd75e60b] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

pk|col|tipo|data|data_fim|status|grav|decl|jornada|novo|novo==jornada(falta)/0(atestado)|cerca|veredito
5096|col940|atestado|2026-10-21|-|aguardando_documento|720|-|480|0|True|-|mudaria
5124|col940|atestado|2026-10-22|-|aguardando_documento|720|-|480|0|True|-|mudaria
5184|col723|atestado|2026-10-23|-|aguardando_documento|720|-|440|0|True|-|mudaria
5206|col558|atestado|2026-10-24|-|aguardando_documento|720|-|720|0|True|-|mudaria
5305|col493|atestado|2026-10-28|-|aguardando_documento|720|-|530|0|True|-|mudaria
5332|col735|atestado|2026-10-29|-|aguardando_decisao|720|-|540|0|True|-|mudaria
5334|col371|atestado|2026-10-30|-|aguardando_documento|720|-|420|0|True|-|mudaria
5336|col363|atestado|2026-10-22|-|aguardando_documento|720|-|480|0|True|-|mudaria
5345|col288|atestado|2026-10-30|-|aguardando_decisao|720|-|660|0|True|-|mudaria
5348|col723|atestado|2026-10-31|-|aguardando_documento|720|-|440|0|True|-|mudaria
5349|col723|atestado|2026-10-30|-|aguardando_documento|720|-|440|0|True|-|mudaria
5366|col455|atestado|2026-11-01|-|rejeitada|720|-|720|0|True|-|pulada:identidade_mudou
5128|col967|atestado|2026-10-21|-|aprovada|720|-|440|0|True|-|mudaria
5182|col200|atestado|2026-10-24|2026-10-25|aprovada|720|-|840|0|True|-|mudaria
5197|col701|atestado|2026-10-23|-|aprovada|720|-|480|0|True|-|mudaria
5236|col891|atestado|2026-10-27|-|aprovada|720|-|720|0|True|-|mudaria
5198|col444|falta|2026-10-24|-|aprovada|720|-|420|420|True|-|mudaria
5335|col905|falta|2026-10-30|-|aprovada|720|-|660|660|True|-|mudaria
5339|col304|falta|2026-10-29|-|aprovada|720|-|555|555|True|-|mudaria
5340|col304|falta|2026-10-28|-|aprovada|720|-|555|555|True|-|mudaria
5351|col823|falta|2026-11-02|-|aprovada|720|-|240|240|True|-|mudaria
5355|col869|falta|2026-11-02|-|aprovada|720|-|480|480|True|-|mudaria
5357|col924|falta|2026-10-26|-|aprovada|720|-|440|440|True|-|mudaria
5358|col924|falta|2026-11-01|-|aprovada|720|-|440|440|True|-|mudaria
5359|col924|falta|2026-10-28|-|aprovada|720|-|440|440|True|-|mudaria
5360|col924|falta|2026-10-27|-|aprovada|720|-|440|440|True|-|mudaria
5369|col924|falta|2026-10-29|-|aprovada|720|-|440|440|True|-|mudaria
5333|col145|atestado|2026-10-30|2026-12-28|rejeitada|720|-|19800|0|True|-|pulada:identidade_mudou
5353|col769|falta|2026-11-01|-|aprovada|720|-|660|660|True|-|mudaria
5311|col33|atestado|2026-10-29|-|aguardando_decisao|720|-|660|0|True|-|mudaria
UNIVERSO 30 esperado 30
RESUMO mudaria=28 ja_aplicada=0 pulada=2 parcial_dp=0 erros=0
SOMA 30 == 30: True
```

1. No dado fornecido, SOMA e UNIVERSO fecham: `30 == 30: True` e `30 esperado 30`. [nao verificado]
2. Vereditos diferentes de `mudaria`, por pk: [nao verificado]
   - `5333`: `pulada:identidade_mudou`. [nao verificado]
   - `5366`: `pulada:identidade_mudou`. [nao verificado]
3. Nenhuma linha fornecida tem `novo==jornada(falta)/0(atestado)` igual a `False`. [nao verificado]

## 06/11 10:26 · sessao faf686f2cce0 · CAUSA-D-ESTRUTURA · sonda · origem=Code

**O raspado fecha os 200 dias; os três grupos chegam a ESTRUTURA pelo mesmo caminho residual. Ele não demonstra três causas materiais distintas.** A diferença medida entre os grupos é a cardinalidade par; os outros predicados informados são iguais. (`app/ponto/management/commands/e6_oraculo.py:98`, `:104`, `:106`, `:108`, `:110`; valores recebidos: [nao verificado])

**1. Saída e conferência**

Esta é a saída recebida, não uma reexecução minha. O conteúdo de `sonda_causa.py`, sua correspondência com o hash `688ae6ab76dea90a` e a execução pelo envelope não foram conferidos independentemente. [nao verificado]

| Conferência do raspado | Resultado | Evidência |
|---|---:|---|
| `SEM_PORTA_CLASSE_D` | 217 | Anexo [nao verificado] |
| `alcance_dias_omitidos` | 0 | Anexo [nao verificado] |
| `UNIVERSO_DONO_ESTRUTURA` | **200 / esperado 200 — CONFERE** | Anexo [nao verificado] |
| Fora do universo | BATIDA 3; CADASTRO 14 | Anexo [nao verificado] |
| Reconciliação da classe d | 200 + 3 + 14 = **217** | Cálculo sobre o anexo [nao verificado] |
| `SOMA_DOS_GRUPOS` | 182 + 13 + 5 = **200 — True** | Cálculo sobre o anexo [nao verificado] |
| `rc_sombra` | **0** | Anexo [nao verificado] |
| `INCOMPLETO` / erros impressos | Nenhum consta no trecho recebido | Anexo [nao verificado] |

| Grupo | n | Cadastro: folga indefinida / sem vínculo / sem vínculo em trabalho | Turno | Batidas | Fase lavrada | CXR do motor | Evidência |
|---|---:|---|---|---|---|---|---|
| 1 | **182** | False / False / False | não aberto | 0, par | False | False | Anexo [nao verificado] |
| 2 | **13** | False / False / False | não aberto | 4+, par | False | False | Anexo [nao verificado] |
| 3 | **5** | False / False / False | não aberto | 2, par | False | False | Anexo [nao verificado] |
| **SOMA** | **200** | | | | | | Cálculo sobre o anexo [nao verificado] |

| MARGINAL | Distribuição | Evidência |
|---|---|---|
| P1 — cadastro | três flags False: **200** | Anexo [nao verificado] |
| P2 — batida fora de turno | `turno_nao_aberto`: **200** | Anexo [nao verificado] |
| P3 — paridade | 0: **182**; 4+ pares: **13**; 2: **5** | Anexo [nao verificado] |
| P4 — fase | `fase_lavrada=False`: **200** | Anexo [nao verificado] |
| P5 — CXR | `em_cxr_do_motor=False`: **200** | Anexo [nao verificado] |

**2. Quais predicados deixaram os dias no resíduo**

A montagem real é `InsumosDoDono.dono`: carrega cadastro, consulta `batida_fora_de_turno`, lê a fase e pergunta a pertença à CXR do motor. A paridade usa `oraculo.horas`. ([e6_oraculo.py:172](/home/ronald/codex_ws/0fca15818137d882b634bdf8261780370e63582d/app/ponto/management/commands/e6_oraculo.py:172), `app/ponto/management/commands/e6_oraculo.py:144`)

Esta cadeia vale **para cada um dos três grupos**, com a variação de P3 explicitada:

| Predicado real | Valor recebido | Por que não captura o dia |
|---|---|---|
| P1 — `folga_nao_definida or sem_vinculo_no_dia or sem_vinculo_em_dia_de_trabalho` | False / False / False [nao verificado] | O OR é falso; não retorna CADASTRO. (`app/ponto/management/commands/e6_oraculo.py:98`) |
| P2 — `batida_fora_de_turno` | `turno_nao_aberto` [nao verificado] | `.aberto=False` faz a função retornar False imediatamente; a consulta das entradas soltas nas batidas apuráveis não é alcançada. (`app/ponto/management/commands/e6_oraculo.py:196`, `:198`) |
| P3 — `len(horas_do_dia or ()) % 2` | G1: 0; G2: 4+ par; G3: 2 [nao verificado] | O resto é zero nos três grupos; não retorna BATIDA. (`app/ponto/management/commands/e6_oraculo.py:106`) |
| P4 — `fase_deslocada` | False [nao verificado] | Não retorna CADASTRO. (`app/ponto/management/commands/e6_oraculo.py:108`) |
| P5 — `em_cadastro_x_realidade` | False [nao verificado] | O ternário final escolhe **ESTRUTURA**. ([e6_oraculo.py:110](/home/ronald/codex_ws/0fca15818137d882b634bdf8261780370e63582d/app/ponto/management/commands/e6_oraculo.py:110)) |

Há quatro limites para interpretar esses negativos:

- **Cadastro False não certifica todo o cadastro.** Os flags vêm do vínculo do dia e de `defeito_folga_nao_definida`; este último acusa especificamente o 6x1 sem folga semanal no template e sem `FolgaDia` no mês. (`app/ponto/supra_juiz.py:193`, `:200`; `app/escala/models.py:1241`, `:1247`)
- **“Turno não aberto” não prova turno fechado.** Sem qualquer turno do dia, a autoridade também retorna `aberto=False`, junto de `sem_turno=True`. ([turnos.py:717](/home/ronald/codex_ws/0fca15818137d882b634bdf8261780370e63582d/app/ponto/turnos.py:717), `app/ponto/turnos.py:596`)
- **Fase False não prova fase correta.** Dia sem nenhuma lavra cobrindo também fica fora do conjunto de fase deslocada. (`app/escala/services/fase_12x36.py:123`)
- **CXR False significa ausência na lista lida.** A leitura admite resultado/lista ausente como vazio e descarta itens sem `dia`; o raspado não distingue essas situações. (`app/ponto/management/commands/e6_oraculo.py:160`; situação dos casos: [nao verificado])

**Lei do desenho e limite por grupo**

Existe lei escrita comum ao beco: **“Emissao, pergunta e chamado nascem do sinal da celula”**, e o corte explicita **“lampada acesa no marco = fio vivo obrigatorio”**. ([CLAUDE.md:168](/home/ronald/codex_ws/0fca15818137d882b634bdf8261780370e63582d/CLAUDE.md:168), [CORTES.md:263](/home/ronald/codex_ws/0fca15818137d882b634bdf8261780370e63582d/app/docs/CORTES.md:263))

Contudo, a classe **d atual** é definida por ausência de pergunta viva, dia fora da lista e colaborador presente na lista, depois de recusadas as rotas anteriores. Ela não identifica qual emissor falhou nem equivale automaticamente ao GRUPO-D histórico. (`app/chamados/services/vigia_fio.py:635`, `:654`, [vigia_fio.py:677](/home/ronald/codex_ws/0fca15818137d882b634bdf8261780370e63582d/app/chamados/services/vigia_fio.py:677), `app/docs/CORTES.md:263`)

| Grupo | O que fica explicado | Lei escrita e o que não fica demonstrado |
|---|---|---|
| **1 — 182, zero par** | P1/P2/P4/P5 negativos; P3 é `0 % 2 = 0`. O código declara expressamente zero como par e mantém o dia no resíduo ESTRUTURA. (`app/ponto/management/commands/e6_oraculo.py:72`, `:98`, `:106`, `:110`; valores: [nao verificado]) | Lei comum do fio: `CLAUDE.md:168` / `CORTES.md:263`. Atribuição errada à meia-noite aparece como explicação no comentário, mas sua ocorrência nesses 182 dias não foi mostrada. (`app/ponto/management/commands/e6_oraculo.py:73`; ocorrência: [nao verificado]) |
| **2 — 13, quatro ou mais pares** | P1/P2/P4/P5 negativos; P3 tem resto zero. Nenhum desses testes retira o dia do resíduo. (`app/ponto/management/commands/e6_oraculo.py:98`, `:104`, `:106`, `:108`, `:110`; valores: [nao verificado]) | Mesma lei do fio. Quantidade par não demonstra casamento correto com marcos, intervalo ou valor; horários, ata e valores desses casos estão [nao verificado]. |
| **3 — 5, duas pares** | P1/P2/P4/P5 negativos; P3 é `2 % 2 = 0`. O retorno final continua ESTRUTURA. (`app/ponto/management/commands/e6_oraculo.py:98`, `:104`, `:106`, `:108`, `:110`; valores: [nao verificado]) | Mesma lei do fio. Duas batidas do oráculo e `aberto=False` não demonstram um turno real fechado; o realizado também admite ausência de turno com esse flag. (`app/ponto/turnos.py:717`; situação dos cinco casos: [nao verificado]) |

A L-099 exige coerência com o cadastro existente e nomeia esse juiz; o contrato estrutural exige **“1 JUIZ por pergunta”**. O raspado não mostra dois juízes discordando, nem valores divergentes entre leitores, portanto não prova uma quebra específica desses contratos em cada grupo. (`app/docs/LEIS.md:122`, [ESTRUTURAL.md:4](/home/ronald/codex_ws/0fca15818137d882b634bdf8261780370e63582d/app/docs/ESTRUTURAL.md:4), `app/docs/ESTRUTURAL.md:29`; divergências específicas: [nao verificado])

**Não há lacuna de lei demonstrada para o retorno do dono: o resíduo já está escrito.** O que falta é evidência da causa material. Chamar essa falta de evidência de “sem lei” inventaria uma necessidade de critério novo. (`app/ponto/management/commands/e6_oraculo.py:69`, `:110`; causa material por grupo: [nao verificado])

**3. Dois exemplos por grupo**

| Grupo | Exemplo 1 | Exemplo 2 | Evidência |
|---|---|---|---|
| 1 | col144 — 2026-09-21 | col144 — 2026-09-23 | Anexo [nao verificado] |
| 2 | col137 — 2026-11-03 | col137 — 2026-11-05 | Anexo [nao verificado] |
| 3 | col222 — 2026-10-28 | col292 — 2026-10-22 | Anexo [nao verificado] |

“Zero batidas” foi preservado como rótulo recebido: na montagem do dono, a contagem vem de `oraculo.horas`, agrupada pela data inicial do turno do oráculo, enquanto `brutas` conta por data civil. Zero nessa chave não prova ausência de marcações civis naquela data. (`app/ponto/management/commands/e6_oraculo.py:144`, `:183`; `app/ponto/calculador/nucleo.py:235`, `:237`)

_2220 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
