# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 20:54** · publicado em **06/11/2026 20:59** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 20:54 · sessao 418dcb5a30a1 · PLACAR-LIVENESS · leitura · origem=Code

**Não há dados de produção suficientes para publicar nenhum dos cinco números. Todos permanecem [nao verificado].** Há autoridades aproveitáveis para medir (1); para (4) e (5), o enunciado integral excede o que as fontes existentes conseguem demonstrar. Os limites concretos estão abaixo. (`app/folha/fora_da_folha.py:203`; `app/api/views_core.py:402`; `app/chamados/services/vigia_fio.py:334`, `:577`)

**1. Dia que segura a folha sem porta**

**Autoridade real:** `folha.fora_da_folha.o_que_segura`. Ela pede os dias retentores a `motivos_retencao_celula` e consulta `porta_alcancavel_do_dia`; `fio == FIO_SEM_PORTA` é a resposta existente para “retém sem porta”. Não é necessário reproduzir a regra. (`app/folha/fora_da_folha.py:95`, `:203`, `:217`, `:221`; `app/chamados/juizes.py:1329`)

**Universo:** enumerar `universo_da_competencia`, por empresa, e chamar `o_que_segura` para cada colaborador. Usar somente `fora_da_folha`, `dias_que_seguram` ou `censo_alcance` reduz indevidamente a frota: a classificação padrão começa nos `FechamentoMensal` existentes e o segundo filtro aceita somente `furo_espelho`. (`app/ponto/services/fechamento.py:382`, `:439`; `app/folha/export.py:777`; `app/folha/fora_da_folha.py:119`, `:128`)

A composição existente de competências abertas é `competencias_a_lavrar`: corrente incondicionalmente, mais anterior enquanto não entregue. **Ela não enumera todas as competências históricas abertas.** A sonda abaixo declara esse escopo. (`app/ponto/services/he_pendente_lavrado.py:209`, `:220–225`)

**Exclusões da autoridade:** isentos; datas fora de admissão/demissão; dia sem célula e sem vínculo; protestos integralmente não cobráveis; pausa isolada absolvida pelo juiz correspondente. Cegueira passada com vínculo pode reter; não deve ser eliminada pela sonda. (`app/folha/export.py:1192–1214`, `:1227–1256`)

**Porta** significa chamado da célula na fila, ou pergunta viva no app com âncora na fila e resposta não vazia. Acesso manual por URL e “Resolver dia” ficam expressamente fora dessa definição; ela também não recebe um usuário admin concreto para testar sua permissão. (`app/chamados/juizes.py:1341–1370`, `:1372–1387`)

**2. POST de tela acima de 30 segundos, últimas 24 horas**

**Não existe medidor pronto identificado para esse contrato.** Existe a fonte: Gunicorn escreve método, rota, status e duração em segundos, em `/app/logs/access-ui.log*` e `/app/logs/access-core.log*`, além do console. O `timeout=30` é configuração, não contagem de requisições lentas. (`app/gunicorn.conf.py:21`, `:42–49`, `:83–104`; `app/gunicorn.core.conf.py:34–41`, `:75–96`)

Quem escreve os campos é o access logger; `colab` e `cli` vêm dos cabeçalhos produzidos por `LogContextoMiddleware`. `colab=u123` identifica **User**, não `Colaborador` 123: precisa resolver a relação antes de emitir `colNNN`. O leitor existente `_uso_no_log` conta uso de portas, mas não aplica duração nem janela de 24 horas. (`app/core/middleware.py:114–119`, `:139–162`; `app/core/portas_da_tela.py:82–100`)

Padrão proposto, sobre a mensagem original do access log:

```python
ACCESS = re.compile(
    r"^(?P<ts>\d{4}-\d\d-\d\d \d\d:\d\d:\d\d) "
    r"\S+ ip=(?P<ip>.*?) colab=(?P<ator>-|u?\d+) "
    r"cli=(?P<cli>.*?) (?P<metodo>[A-Z]+) (?P<rota>/\S*) -> "
    r"(?P<status>\d{3}) (?P<segundos>\d+(?:\.\d+)?)s ua=.*$"
)
```

O padrão exige a linha completa emitida pelo escritor; uma frase mencionando “POST”, “30s” ou “timeout” não basta. Depois do casamento: janela fixa `[agora−24h, agora)`, método `POST`, duração **estritamente maior que 30**, uma ocorrência por requisição. A forma dos campos vem do escritor; a disponibilidade de 24 horas completas e o fuso efetivo dos arquivos são [nao verificado]. (`app/gunicorn.conf.py:48–49`, `:83–84`)

“De tela” ainda precisa de um universo explícito de rotas/canais: a V2 envia ao endpoint API no core; excluir `/api/` perderia essa tela. O censo estático de portas exclui API e, portanto, sozinho não resolve essa seleção. (`app/templates/ponto/bater_ponto_v2.html:335–338`; `app/core/portas_da_tela.py:20`, `:112–117`)

**3. HTTP 500 com Traceback para admin ou colaborador, 24 horas**

**Existe um medidor próximo, mas insuficiente:** `selo_paginas_500.sh` procura `50[0-9]`, usa cinco minutos por padrão, não exige Traceback e permite falha do pipeline produzir zero. Não atende ao contrato. (`bin/selo_paginas_500.sh:13–19`)

**Fontes:** access logs para status, rota e ator; logger `django.request` para a exceção, escrito em `/app/logs/errors.log*` e console. O arquivo de erros não contém casca, usuário ou request ID no formato configurado. Para preservar a casca, ler separadamente o console de `saas_ui` e `saas_core`, com limites explícitos de tempo; não somar arquivo e console do mesmo erro. (`app/config/settings/saas.py:122`, `:131–142`, `:173`; `app/gunicorn.conf.py:48–49`; `bin/selo_paginas_500.sh:15–16`)

Padrões propostos para o registro de erro:

```python
ERROR500 = re.compile(
    r"^(?P<ts>\d{4}-\d\d-\d\d \d\d:\d\d:\d\d,\d{3}) "
    r"ERROR django\.request Internal Server Error: (?P<rota>/\S*)$"
)
TRACEBACK = re.compile(r"^Traceback \(most recent call last\):$")
```

Exigir o cabeçalho e o Traceback **no mesmo registro multilinha**, antes do próximo cabeçalho; casar também um access record com `status == 500`. Isso exclui prosa solta e tracebacks de boot ou de outro logger. O formatter é verificável; uma ocorrência concreta desse cabeçalho Django não foi fornecida, portanto o padrão precisa ser confrontado com a linha real antes de valer como instrumento. (`app/config/settings/saas.py:122`, `:173–174`; `app/gunicorn.conf.py:89–106`; [nao verificado])

**Limite decisivo:** correlação por horário e rota não identifica inequivocamente uma requisição sob concorrência. Sem vínculo seguro entre erro, access record e ator admin/colaborador, esse caso é `NAO SEI`; não cabe eliminá-lo. Os formatos existentes não incluem request ID compartilhado. (`app/config/settings/saas.py:122`; `app/gunicorn.conf.py:48–49`; `app/core/middleware.py:150–162`)

**4. Batida recusada sem trilha ou sem saída para o colaborador**

**Não existe fonte completa nem função de medida equivalente.** A ausência da própria tentativa no banco impede encontrar todas as recusas sem trilha; também falta comprovação persistida de apresentação/entrega da saída. Os contraexemplos são concretos:

| Caminho | O que a cópia demonstra |
|---|---|
| API | Recusas por situação e tipo inválido retornam antes da auditoria GPS. (`app/api/views_core.py:402–426`) |
| V2 antes do POST | Câmera/GPS podem interromper o envio apenas com aviso e `return false`. (`app/templates/ponto/bater_ponto_v2.html:285–301`, `:325`) |
| Colisão | `registrar_batida` pode devolver `None, conflito`; a API responde HTTP 200, `sucesso=True`, `ignorada_intervalo`. (`app/ponto/registro_batida.py:138–151`; `app/api/views_core.py:695–718`) |
| V2 depois do POST | O ramo `r.ok` não verifica esse status antes de apresentar sucesso. (`app/templates/ponto/bater_ponto_v2.html:340`) |
| Tela antiga | Há retorno `ignorada_intervalo` sem registro da tentativa nesse ramo. (`app/ponto/services/triagem_batida.py:707–732`) |
| Fila offline | A tentativa reside no IndexedDB; respostas 400 podem removê-la com `console.warn`. (`app/static/pwa.js:107–175`) |

A parte consultável é `LogAuditoria.acao == "batida_recusada_sem_gps"`, escrita pela API. Isso conta recusas GPS **com trilha**, não as recusas sem trilha. Até essa escrita é best effort. O middleware de mensagem melhora o JSON devolvido, mas não comprova recebimento pelo colaborador. (`app/api/views_core.py:440–458`; `app/core/observ.py:105`, `:133–139`; `app/core/middleware.py:19–46`)

Logo, a sonda devolve **`C4 NAO SEI`**, acompanhada do inventário GPS disponível. Transformar esse inventário em contador integral seria afirmar o que a fonte não mostra. (`app/api/views_core.py:402–458`)

**5. Pergunta ou chamado vivo que o admin não alcança**

**Não existe uma autoridade única para o enunciado global.** Existem três recortes distintos:

| Instrumento real | Universo e limite |
|---|---|
| `vigia_fio.censo` | Chamado vivo com célula, módulo do vocabulário de furo e célula acusando. Mede ausência de fio vivo, não permissão de acesso do admin. (`app/chamados/services/vigia_fio.py:334–379`, `:417`) |
| `vigia_fio.censo_alcance` | Dias retidos pela folha nas competências de `competencias_a_lavrar`; usa a porta real. Exclui hoje/futuro e quatro baldes de encaminhamento. (`app/chamados/services/vigia_fio.py:542`, `:577–589`, `:642–660`) |
| Fio mudo | Está dentro do emissor, com filtros próprios; não há função pública pura equivalente ao contador global. Não executar o comando como sonda de leitura. (`app/chamados/management/commands/disparar_perguntas_competencia.py:141–159`, `:251`, `:304`) |

Os quatro baldes são decisão do dia pendente, ausência em decisão, escala que não descreve o dia e fio do admin. Este último exige presença **do dia** na lista e dono lavrado BATIDA/CADASTRO. Não equivalem a provar que um admin concreto consegue alcançar todo objeto vivo. (`app/chamados/services/vigia_fio.py:274–315`; `app/chamados/juizes.py:1341`)

A sonda publica os dois censos como diagnósticos completos, mas mantém **`C5 NAO SEI`** para o enunciado literal. Falta definir e implementar o universo global, a identidade/permissões do admin e a composição sem duplicação entre pergunta, chamado e dia. (`app/chamados/services/vigia_fio.py:334`, `:577`; `app/chamados/juizes.py:1341`)

**Fontes de banco, escritores e leitores usados**

Os campos abaixo são fontes das autoridades e dos diagnósticos; valores derivados em memória, como `fio` e `dono`, não são uma nova tabela de fatos. (`app/folha/fora_da_folha.py:221–231`; `app/ponto/management/commands/e6_oraculo.py:172–183`)

| Modelo/campos | Quem escreve | Quem lê |
|---|---|---|
| `Empresa.dia_inicio_competencia` | Formulário → porta de estrutura. (`app/colaboradores/views.py:523`, `:538`; `app/colaboradores/services/estrutura.py:17`) | Janelas. (`app/ponto/janelas.py:36–57`) |
| `Colaborador.empresa`, `situacao`, admissão/demissão, `isento_ponto` | Cadastro/desligamento → portas de colaborador. (`app/colaboradores/views.py:176`, `:1313–1342`; `app/colaboradores/services/desligamento.py:103–108`; `app/colaboradores/services/colaborador.py:55–76`) | Universo e retenção. (`app/ponto/services/fechamento.py:439–445`; `app/folha/export.py:1192–1201`) |
| `CelulaDia.colaborador`, `data`, `trabalha` | Geração/edição pela porta de célula. (`app/ponto/portas/celula.py:600`, `:849–868`) | Retenção/censos. (`app/folha/export.py:1206`, `:1255`; `app/chamados/services/vigia_fio.py:378`) |
| `CelulaDia.veredito`, `protestos`, `ata` | `lavrar_veredito`. (`app/ponto/portas/celula.py:969–1035`) | Retenção e juiz da pausa. (`app/folha/export.py:1142`, `:1233–1254`) |
| `FechamentoMensal.colaborador`, mês/ano, horas, turnos abertos | Recalcular fechamento. (`app/ponto/services/fechamento.py:1121–1155`) | `classificar_export`. (`app/folha/export.py:777–780`, `:823–828`) |
| `ChamadoColaborador.colaborador`, `celula`, `modulo_origem`, `status_local` | `abrir`/`criar`; mudanças pelo núcleo de estado. (`app/chamados/models.py:222`, `:796`, `:1015`) | Painel e censos. (`app/chamados/services/painel.py:155`, `:187`; `app/chamados/services/vigia_fio.py:338`, `:607`) |
| `ChamadoColaborador.silenciado_ate` | Silenciar/levantar. (`app/chamados/services/arquivo.py:166`, `:201`) | Fila. (`app/chamados/services/painel.py:188`) |
| `PerguntaDisputa.celula`, `data_evento`, `chamado_destino` | Porta de abertura. (`app/chamados/models.py:2436–2476`) | Alimentação e porta. (`app/chamados/juizes.py:1300`, `:1378–1381`) |
| `resposta_colab`, `respondida_em`, `validada_em` | Resposta, validação e portas de carimbo. (`app/chamados/services/disputa_emissao.py:1282–1295`, `:1433–1450`; `app/chamados/services/validacao.py:131`; `app/chamados/models.py:2589–2592`) | Vitalidade e resposta vazia. (`app/chamados/juizes.py:1136`, `:1150`, `:1257`) |
| `materializada_em`, `via_resolucao` | `_escrever_carimbo`, pelas portas de carimbo. (`app/chamados/models.py:2556–2562`, `:2625`) | Vitalidade. (`app/chamados/juizes.py:1171`, `:1207`) |
| `LogAuditoria.pk`, `acao`, `modelo`, `objeto_id`, `criado_em`, payload GPS | Campos do modelo; `evento` persiste; API monta a recusa GPS. (`app/core/models.py:285–307`; `app/core/observ.py:134–137`; `app/api/views_core.py:440–451`) | Tela genérica de auditoria e inventário da sonda. (`app/core/views.py:686–738`) |
| `Batida.timestamp`, `retratada_em` | Registro e retratação. (`app/ponto/registro_batida.py:157–160`, `:267–281`) | `batidas_apuraveis` e insumos do dono. (`app/ponto/turnos.py:2844–2864`; `app/ponto/management/commands/e6_oraculo.py:144`) |
| Resultado `dias_cadastro_x_realidade` | Motor, em memória. (`app/ponto/motor_calculo_v2.py:2719`) | Insumos do dono. (`app/ponto/management/commands/e6_oraculo.py:154–161`) |
| `MetricaSnapshot` da fase: chave, escopo, `data_ref`, `calculado_em`, `detalhe.janelas/itens/itens_reabertos` | Lavra do censo de fase. (`app/escala/services/fase_12x36.py:85–93`) | Cobertura e fase as-of. (`app/escala/services/fase_12x36.py:97–148`) |

**Dono L-099 por caso**

Para divergência de **dia**, chamar `InsumosDoDono(...).dono(data)`, que monta os insumos e chama o juiz real. A ordem é: folga indefinida/sem vínculo → CADASTRO; batida solta ou paridade ímpar → BATIDA; fase deslocada → CADASTRO; Cadastro x Realidade → CADASTRO; restante → ESTRUTURA. Não inferir dono pela classe do vigia. (`app/ponto/management/commands/e6_oraculo.py:98–110`, `:113–183`; `app/chamados/services/vigia_fio.py:99–101`)

Para timeout, 500 ou falta de instrumentação, esse juiz diário não é autoridade de causa. Os defeitos demonstrados de código são estruturais; atribuir um evento de produção específico a CADASTRO/BATIDA/ESTRUTURA sem evidência é [nao verificado]. (`app/ponto/management/commands/e6_oraculo.py:113–125`)

Há um furo adicional: fase nunca medida volta como conjunto vazio. A sonda conserva `dono_e6`, mas marca `dono=NAO SEI` quando o residual ESTRUTURA depende dessa ausência de cobertura; usa o próprio predicado de cobertura existente. (`app/escala/services/fase_12x36.py:113–148`)

**Sonda devolvida como texto; execução e resultados [nao verificado].** Ela usa as funções citadas, impede escrita por transação PostgreSQL `READ ONLY`, não chama comandos lavradores e só publica campos selecionados. Qualquer exceção ou incremento de engolidos produz `NAO SEI`. Essa proteção é necessária porque a vitalidade engole falhas e pode alcançar `get_or_create` do usuário de sistema. (`app/chamados/juizes.py:1204–1218`, `:1069–1070`; `app/core/observ.py:60–81`; `app/core/utils.py:18`)

```python
# Colar no Django shell já inicializado.
# Não grava script, carimbo, auditoria ou arquivo de saída.
import contextlib
import datetime as dt
import io
import json
import logging
import sys
from unittest.mock import patch

from django.db import connection, transaction
from django.utils import timezone
from django_tenants.utils import schema_context

agora = timezone.now()
hoje = timezone.localdate(agora)


def medir(rotulo, funcao):
    anterior = logging.root.manager.disable
    try:
        logging.disable(logging.CRITICAL)
        with contextlib.redirect_stdout(io.StringIO()), \
                contextlib.redirect_stderr(io.StringIO()):
            from core.observ import engolidos
            with schema_context("juliani"), transaction.atomic():
                if connection.vendor != "postgresql":
                    raise RuntimeError()
                with connection.cursor() as cursor:
                    cursor.execute(
                        "SET TRANSACTION ISOLATION LEVEL "
                        "REPEATABLE READ READ ONLY"
                    )
                antes = engolidos()["total"]
                with patch("django.utils.timezone.now", return_value=agora):
                    resultado = funcao()
                if engolidos()["total"] != antes:
                    raise RuntimeError()
                transaction.set_rollback(True)
    except Exception:
        resultado = [(rotulo, "NAO SEI")]
    finally:
        logging.disable(anterior)

    for etiqueta, valor in resultado:
        print(etiqueta, json.dumps(valor, ensure_ascii=True))


def anotador():
    from ponto.janelas import janela_atual
    from ponto.management.commands.e6_oraculo import InsumosDoDono
    from escala.services.fase_12x36 import _indice_de_cobertura, _cobre

    insumos = {}
    indice = None

    def anotar(colab, dia, rota):
        nonlocal indice
        ini, fim = janela_atual(dia, colab.empresa)
        chave = (colab.pk, ini, fim)
        if chave not in insumos:
            insumos[chave] = InsumosDoDono(
                colab, ini, fim, hoje, agora=agora
            )
        dono_e6 = insumos[chave].dono(dia)
        dono = dono_e6

        if dono_e6 == "ESTRUTURA":
            if indice is None:
                indice = list(_indice_de_cobertura(hoje))
            coberto = any(
                _cobre(janelas, colab.empresa_id, dia.isoformat())
                for _pk, janelas, _instante in indice
            )
            if not coberto:
                dono = "NAO SEI"

        return {
            "chave": "col%03d/%s" % (colab.pk, dia.isoformat()),
            "rota": rota,
            "dono": dono,
            "dono_e6": dono_e6,
        }
    return anotar


def separar(casos):
    estrutura = [x for x in casos if x["dono"] == "ESTRUTURA"]
    admin = [x for x in casos if x["dono"] in ("CADASTRO", "BATIDA")]
    incertos = [x for x in casos if x["dono"] == "NAO SEI"]
    if len(estrutura) + len(admin) + len(incertos) != len(casos):
        raise RuntimeError()
    return {
        "contagem_estrutura": "NAO SEI" if incertos else len(estrutura),
        "total_casos": len(casos),
        "casos_estrutura": estrutura,
        "lista_admin": admin,
        "dono_nao_sei": incertos,
    }


def sonda1():
    from colaboradores.models import Empresa
    from chamados.juizes import FIO_SEM_PORTA, tranca_em_lote
    from folha.fora_da_folha import o_que_segura
    from ponto.services.fechamento import universo_da_competencia
    from ponto.services.he_pendente_lavrado import competencias_a_lavrar

    empresas = list(Empresa.objects.all().order_by("pk"))
    if not empresas:
        raise RuntimeError()
    anotar = anotador()
    casos, vistos, escopo = {}, 0, []
    dias_retentores = 0

    with tranca_em_lote():
        for empresa in empresas:
            for mes, ano in competencias_a_lavrar(empresa, hoje):
                escopo.append([empresa.pk, mes, ano])
                frota = universo_da_competencia(
                    mes, ano, empresa.pk
                ).select_related("empresa").order_by("pk")
                for colab in frota:
                    vistos += 1
                    resposta = o_que_segura(colab, mes, ano)
                    # Nunca imprimir o dict: ele contém nome.
                    for dia in resposta["dias"]:
                        dias_retentores += 1
                        if dia["fio"] != FIO_SEM_PORTA:
                            continue
                        data = dt.date.fromisoformat(dia["data"])
                        casos[(colab.pk, data)] = anotar(
                            colab, data, "sem_porta"
                        )
    if not vistos:
        raise RuntimeError()

    resultado = separar([casos[k] for k in sorted(casos)])
    resultado.update({
        "fonte": "o_que_segura",
        "escopo": "competencias_a_lavrar",
        "empresa_mes_ano": escopo,
        "colaborador_competencias_examinados": vistos,
        "dias_retentores": dias_retentores,
        "medido_em": agora.isoformat(),
    })
    return [("C1", resultado)]


def sonda4():
    from core.models import LogAuditoria

    casos = []
    registros = LogAuditoria.objects.filter(
        acao="batida

[truncado em 20000 de 24804 caracteres]

## 06/11 20:52 · sessao 1cb72ae84c20 · BO-[nome]-0611-V2 · censo · origem=Code

**Classificação principal: (e) nos itens 02, 05, 07 e 08; (b) nos itens 09, 11 e 12.** Nos casos (b), o descarte aparece na própria consolidação de t1.r: ela registra a pendência e atribui “ja OK”. Não há despacho separado que permita atribuir esse descarte a outro responsável. [nao verificado]

A distinção entre tema não visto, achado descartado e requisito não perguntado segue `CLAUDE.md:377`. t1.q não definiu “ja OK hoje” como ação executada na tela, resultado posterior no espelho/DiaPago e validação operacional; cobrar retrospectivamente esse conjunto seria **(e)**. [nao verificado]

**02 — col993 → col78: (e), identificação humana não exigida.** A correção do ID vem de v2.r; a fonte humana que confirma o vínculo não foi anexada. [nao verificado]

**t1.q — trechos exatos:** [nao verificado]

> diga qual candidato colNNN e o caso (o de maior atividade na janela, salvo evidencia contraria) e o que os dados mostram;

> 02 correcao de horario do 02a nao chega (a do 02b chegou): compare os dois caminhos (Justificativa / RegularizacaoExterna / PerguntaDisputa) e onde o do 02a parou.

**t1.r — nascimento da escolha:** [nao verificado]

> **02 — col993 comparado com col1023: falta informacao.** Mantive col993, atividade 102 contra 91 e 19:

A resposta seguiu a heurística expressamente pedida. Sem os CANDIDATOS/FICHAS originais, não se demonstra que havia identificação contrária disponível e ignorada. A regra de correspondência por nome completo existe em `CLAUDE.md:142`; atividade não fornece essa confirmação. [nao verificado]

**Regra preventiva proposta:** “Só vincule o item após confirmação humana de colNNN + nome completo, preservando apenas colNNN na saída. Sem confirmação, falta informacao; maior atividade não decide identidade.” [nao verificado]

**05 — col155 → col456: (e), mesma lacuna de identificação.** A correção do ID vem de v2.r. [nao verificado]

**t1.q — trechos exatos:** [nao verificado]

> diga qual candidato colNNN e o caso (o de maior atividade na janela, salvo evidencia contraria) e o que os dados mostram;

> 05 colab diz que o GPS nao aciona e nao consegue bater:

> Veja as batidas e devices da FICHA 05.

**t1.r — nascimento da escolha:** [nao verificado]

> **05 — col155: nosso com obra, dono ESTRUTURA; sem obra específica localizada.** É o maior candidato, atividade 23 contra 15;

Encontrar a barreira de GPS no código não confirma qual candidato fez a tentativa relatada; a própria re-triagem mantém a causalidade individual sem demonstração. [nao verificado]

**Regra preventiva proposta:** exigir a mesma confirmação humana do item 02 e registrar separadamente identidade, bloqueio genérico e tentativa individual comprovada. [nao verificado]

**07 — col836: (e), “visibilidade explicada” sem critério de encerramento do caso.** [nao verificado]

**t1.q — trecho exato:** [nao verificado]

> 07 chamado de 14/10 existe e nao aparece "na lista": qual lista de chamados do admin, qual filtro dela exclui o chamado da FICHA 07 (status, superado_por, silenciado, celula, empresa visivel)?

**t1.r — nascimento do “OK” limitado, depois consolidado como classe do item:** [nao verificado]

> **07 — col836, 14/10: ja OK hoje quanto à exclusão da lista inicial.**

> A pendência de escala indicada na célula é separada da visibilidade desse chamado resolvido.

A pergunta pediu a explicação da lista, e o código confirma o filtro entre cobrança e histórico (`app/chamados/services/painel.py:298`, `:314`). Faltou definir que explicar a exclusão não encerrava o caso operacional. Não se pode imputar à primeira leitura a omissão da cronologia específica revelada em v2 sem a FICHA original completa. [nao verificado]

**Regra preventiva proposta:** “Explique a visibilidade separadamente. Só classifique o item inteiro como ja OK após conferir a pendência da célula e o critério operacional explícito; ressalvas devem permanecer na tabela.” [nao verificado]

**08 — col617: (e), refutação parcial promovida a encerramento.** [nao verificado]

**t1.q — trechos exatos:** [nao verificado]

> 08 2o atestado com data errada aparece no espelho SEM aceite:

> status, quem registrou/aprovou (proprio colab?), e qual regra deixa uma declarada pelo colab valer sem passo do admin

**t1.r — nascimento do “OK” e sua própria limitação:** [nao verificado]

> **08 — col617, 05–06/11: ja OK hoje quanto à alegação de autoaceite.**

> o dado não sustenta aprovação pelo próprio colaborador; tampouco mostra a data correta do documento ou a identidade do decisor.

A resposta viu as lacunas. A cópia confirma que documento anexo nasce aguardando decisão (`app/ponto/services/ausencia.py:791`), mas isso não comprova o percurso histórico desse registro. v2.r também não demonstra autoaceite indevido nem atestado atualmente pendente. O falso fechamento amplo depende do critério que t1.q deixou indefinido. [nao verificado]

**Regra preventiva proposta:** “Não comprovar autoaceite não encerra a queixa. Exija período correto, decisão autorizada e resultado conferido; sem essas evidências, falta informacao.” [nao verificado]

**09 — col1000: (b), pendências vistas e descartadas na classificação.** [nao verificado]

**t1.q — trecho exato:** [nao verificado]

> 09 batidas faltando, sem chamado, e a tela mostra "resolvido": que tela/status diz resolvido nos dias da FICHA 09 sem batida, e quem decidiu.

**t1.r — nascimento do “OK”:** [nao verificado]

> **09 — col1000: ja OK hoje para os dias sem batida apresentados.**

**t1.r — o que já havia visto:** [nao verificado]

> **27/10 tem perguntas pendentes; 29 e 30/10 têm chamados próprios em análise**, criados hoje. A tela exata originalmente chamada de “resolvido” permanece sem identificação.

A resposta usou 26 e 31/10 para sustentar o fechamento, embora registrasse outros dias pendentes e não identificasse a tela solicitada. Esse descarte já é demonstrável em t1.r, sem exigir o critério adicional de DiaPago de v2. [nao verificado]

**Regra preventiva proposta:** “Confronte todos os dias da queixa. Dias explicados não encerram outros dias pendentes; sem identificar tela/status e decisor, não classifique o item como ja OK.” [nao verificado]

**11 — col921: (b), explicação do botão sobreposta às irregularidades já vistas.** [nao verificado]

**t1.q — trecho exato:** [nao verificado]

> 11 "Resolver dia" some depois de 20/10: resolver_dia na FICHA 11+12 por data; e regra da janela/competencia (21-20) ou defeito?

**t1.r — nascimento do “OK”:** [nao verificado]

> **11 — col921: ja OK hoje; regra de competência.**

**t1.r — pendência expressamente reconhecida:** [nao verificado]

> e também nos dias **21–23 e 25–28/10**, ainda irregulares.

A regra do botão está sustentada pelo código: exige último recurso, passado e status permitido (`app/colaboradores/services/calendario.py:103`, `:738`). A re-triagem não refuta essa regra; o descarte foi transformar sua explicação em fechamento do item apesar dos dias “ainda irregulares”. [nao verificado]

**Regra preventiva proposta:** “Responda separadamente funcionamento do botão e situação dos dias. Regra de competência correta não autoriza fechar pendências já identificadas.” [nao verificado]

**12 — col921: (b), segunda pergunta reconhecida como sem resposta comprovada.** [nao verificado]

**t1.q — trecho exato:** [nao verificado]

> 12 falta em folga recusada: confirme a regra no codigo; qual lancamento cobre abandono de posto em dia de folga/escala?

**t1.r — nascimento do “OK”:** [nao verificado]

> **12 — col921: ja OK hoje quanto à recusa de falta em folga.**

**t1.r — lacuna que já havia visto:** [nao verificado]

> **Não há lançamento específico de “abandono de posto em folga” demonstrado neste caso.**

> isso não comprova uma porta de tela para o lançamento pretendido

O veto de falta sem jornada prevista existe (`app/ponto/precedencia.py:536`, `:538`). Confirmá-lo respondeu à primeira metade; a própria resposta reconheceu que não comprovou o lançamento solicitado na segunda, mas consolidou o item como “ja OK”. [nao verificado]

**Regra preventiva proposta:** “Item com duas perguntas só recebe ja OK quando ambas estiverem respondidas com evidência. Sem demonstrar o lançamento adequado, mantenha falta informacao.” [nao verificado]

Não há suporte suficiente neste recorte para classificar algum dos sete como **(a)** ou **(d)**. Dados históricos ausentes e resultados operacionais não anexados constituem limites **(c)**; eles não demonstram, por si, uma falha de leitura nem MODO-COMUM. [nao verificado]

## 06/11 20:51 · sessao 7a8c9a2ef04b · - · - · origem=Ronald (tmux)

Contador 1 tem autoridades reais, mas não há função única com o universo integral e o contador L-099 solicitado: `o_que_segura` cruza retenção e porta, porém classifica o export apenas entre snapshots existentes; `dias_que_seguram` restringe ainda a `motivo='furo_espelho'`. Para a frota inteira, enumerar `universo_da_competencia` e chamar `o_que_segura` para cada colaborador. Fontes: `app/folha/fora_da_folha.py:203`, `app/folha/fora_da_folha.py:215`, `app/folha/fora_da_folha.py:217`, `app/folha/fora_da_folha.py:221`, `app/folha/fora_da_folha.py:128`, `app/ponto/services/fechamento.py:382`, `app/folha/export.py:777`.

- Dia retentor: `motivos_retencao_celula(com_dias=True)` devolve `data`, `motivo`, `celula_id`, `veredito`; exclui isento, corta admissão/demissão, dispensa dia sem célula e sem vínculo, não acusa cegueira futura, dispensa protestos integralmente não cobráveis e pausa isolada pela autoridade existente. `app/folha/export.py:1168`, `app/folha/export.py:1192`, `app/folha/export.py:1198`, `app/folha/export.py:1227`, `app/folha/export.py:1231`, `app/folha/export.py:1253`.
- Porta: `porta_alcancavel_do_dia` aceita chamado da célula na fila ou pergunta viva no app com âncora na fila; acesso manual e “Resolver dia” ficam fora por decisão expressa. `app/chamados/juizes.py:1341`, `app/chamados/juizes.py:1367`, `app/chamados/juizes.py:1375`.
- `o_que_segura` já chama essa porta e expõe `fio`; comparar `fio == FIO_SEM_PORTA` usa a resposta existente, sem copiar regra. `app/folha/fora_da_folha.py:70`, `app/folha/fora_da_folha.py:221`, `app/chamados/juizes.py:1329`.

Fontes e escritores/leitores:

| Campo | Escreve | Lê |
|---|---|---|
| `Empresa.dia_inicio_competencia` | `app/colaboradores/views.py:523` → `app/colaboradores/services/estrutura.py:17` | `app/ponto/janelas.py:36` |
| `Colaborador.situacao/data_demissao` | `app/colaboradores/services/desligamento.py:103` → `app/colaboradores/services/colaborador.py:73` | universo `app/ponto/services/fechamento.py:439`; retenção `app/folha/export.py:1200` |
| `Colaborador.empresa/data_admissao` | `app/colaboradores/views.py:1313` → `app/colaboradores/views.py:1342` | universo `app/ponto/services/fechamento.py:443`; retenção `app/folha/export.py:1198` |
| `Colaborador.isento_ponto` | `app/colaboradores/views.py:176` → `app/colaboradores/services/colaborador.py:55` | `app/core/constants.py:13` → `app/folha/export.py:1192` |
| `CelulaDia.colaborador/data/trabalha/dna/escala_geradora` | geração `app/ponto/portas/celula.py:849`; atualização `app/ponto/portas/celula.py:868`; edição `app/ponto/portas/celula.py:600` | retenção `app/folha/export.py:1206`; pausa `app/folha/export.py:1142`; vínculo `app/escala/alimentacao.py:228` |
| `CelulaDia.veredito/protestos/ata` | `lavrar_veredito`, `app/ponto/portas/celula.py:969`, atribuições `:1028` | `app/folha/export.py:1233`, `app/folha/export.py:1246`, `app/folha/export.py:1142` |
| `FechamentoMensal.colaborador/mes/ano/horas_trabalhadas/turnos_abertos` | `recalcular_fechamento_mes`, `app/ponto/services/fechamento.py:448`, persistência `:1137` e `:1153` | `classificar_export`, `app/folha/export.py:777`, `:823` |
| `Colaborador.codigo_dominio` | `app/colaboradores/views.py:1334` → `app/colaboradores/views.py:1342` | `app/folha/export.py:832`, `:864` |

Dono: usar `InsumosDoDono(c, ini, fim, hoje, agora=agora).dono(data)`, sem remontar seus insumos. Ele reúne batidas apuráveis, realizado/turnos, Cadastro x Realidade, cadastro do vínculo e fase lavrada, chamando `dono_da_divergencia`. `app/ponto/management/commands/e6_oraculo.py:113`, `:128`, `:172`.

A ordem real é: folga indefinida ou sem vínculo → CADASTRO; batida fora de turno ou contagem ímpar → BATIDA; fase deslocada → CADASTRO; Cadastro x Realidade → CADASTRO; resto → ESTRUTURA. `app/ponto/management/commands/e6_oraculo.py:98`.

Furo relevante: ausência de lavra de fase devolve conjunto vazio, embora o próprio código diga que não houve medida; isso pode produzir dono residual ESTRUTURA sem fase perguntada. `app/escala/services/fase_12x36.py:124`, `:132`, `:148`.

Guard somente leitura sem copiar regra:

```python
from escala.services.fase_12x36 import _indice_de_cobertura, _cobre

indice = list(_indice_de_cobertura(hoje))
fase_medida = any(
    _cobre(janelas, c.empresa_id, dia.isoformat())
    for _pk, janelas, _agora in indice
)
dono_e6 = insumos.dono(dia)
dono = "NAO SEI" if dono_e6 == "ESTRUTURA" and not fase_medida else dono_e6
```

Esse guard reutiliza a mesma seleção de lavras e o mesmo predicado de cobertura do leitor real. `app/escala/services/fase_12x36.py:97`, `:113`, `:134`. Se ocorrer, o contador estrutural deve ser `NAO SEI`, preservando a lista diagnóstica. [nao verificado]

Outra limitação: `competencias_a_lavrar` não enumera qualquer competência histórica aberta; inclui corrente incondicionalmente e anterior enquanto não entregue. `app/ponto/services/he_pendente_lavrado.py:209`, `:221`, `:223`.

## 06/11 20:51 · sessao 3c3d98446fcc · - · - · origem=Ronald (tmux)

O contador (5), literalmente “qualquer pergunta/chamado vivo que o admin não alcança”, **não tem uma função única equivalente identificada**: os instrumentos existentes têm universos diferentes e não cobrem essa formulação inteira. O `censo` parte de chamados vivos com célula e lâmpada acusando; `censo_alcance` parte exclusivamente dos dias que retêm a folha nas competências abertas; o fio mudo do emissor parte dos chamados vivos com célula na janela pedida. (`app/chamados/services/vigia_fio.py:334`, `app/chamados/services/vigia_fio.py:577`, `app/chamados/management/commands/disparar_perguntas_competencia.py:141`)

- **Autoridade de alcance:** `porta_alcancavel_do_dia` oferece porta se houver chamado da célula na fila ou pergunta viva no app com âncora na fila. O acesso manual ao chamado fechado e “Resolver dia” ficam fora dessa definição; o retorno não verifica as permissões de um usuário admin concreto. (`app/chamados/juizes.py:1341`, `app/chamados/juizes.py:1367`, `app/chamados/juizes.py:1372`)
- **Fio morto antigo:** `censo(..., limite=sys.maxsize)` é leitura e entrega lista completa `celulas_do_fio_morto`; o contador ignora o limite da lista. Seu próprio comentário avisa que ele pode zerar enquanto persistem dias retidos cujo chamado morreu. (`app/chamados/services/vigia_fio.py:318`, `app/chamados/services/vigia_fio.py:454`, `app/chamados/services/vigia_fio.py:493`)
- **Alcance da folha:** `censo_alcance(..., limite=sys.maxsize)` fornece `[celula_id,colab_id,data,competencia,rota,causa,detalhe,papel]`; os casos `sem_porta` nunca são truncados. Exclui do contador alarmado hoje/futuro e os quatro baldes: decisão pendente, ausência aberta, escala não descreve o dia e fio do admin. (`app/chamados/services/vigia_fio.py:548`, `app/chamados/services/vigia_fio.py:628`, `app/chamados/services/vigia_fio.py:648`, `app/chamados/services/vigia_fio.py:654`)
- **Fio mudo:** existe dentro de `Command.handle`, sem função pura pública de medida: seleciona vivos com célula na janela, elimina aqueles com pergunta viva, parcial destinada à lista e casos sem motivo atual da ata. `--alarme` retorna antes das escritas; o modo dry comum entra em tentativa de escrita com rollback e não serve como sonda estritamente somente leitura. (`app/chamados/management/commands/disparar_perguntas_competencia.py:141`, `app/chamados/management/commands/disparar_perguntas_competencia.py:148`, `app/chamados/management/commands/disparar_perguntas_competencia.py:154`, `app/chamados/management/commands/disparar_perguntas_competencia.py:251`, `app/chamados/management/commands/disparar_perguntas_competencia.py:304`)
- **Outra autoridade de fio mudo:** invariante `F1` dentro de `auditar_invariantes_chamados`: elimina pergunta viva, qualquer resposta preenchida, colaborador não ativo, sem dia apurável, dia absolvido pela lei e fato já decidido. Não mede diretamente alcance do admin. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1624`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1648`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1715`)

Fontes e escrita/leitura:

| Campo | Escrita | Leitura |
|---|---|---|
| `ChamadoColaborador.status_local` | núcleo `_gravar_estado`, atribuição em `models.py:222`; nascimento por `criar` em `models.py:1015` | `painel.qs_vivos` em `painel.py:155`; `vigia_fio.censo` em `vigia_fio.py:335` |
| `ChamadoColaborador.celula`, `modulo_origem`, `colaborador` | campos do modelo em `models.py:32`, `models.py:40`, `models.py:44`; nascimento por `abrir`/`criar` em `models.py:796`, `models.py:1015` | censo lê célula/status/módulo em `vigia_fio.py:338`; alcance lê célula/status em `vigia_fio.py:607` |
| `silenciado_ate` | `arquivo.silenciar`, `arquivo.py:166`; `levantar`, `arquivo.py:201` | `painel.qs_na_fila`, `painel.py:188` |
| `PerguntaDisputa.celula`, `data_evento`, `chamado_destino` | `PerguntaDisputa.abrir`: `models.py:2436`, `models.py:2450`, `models.py:2476`; reapontamento também passa por serviço do fio | `alimentar_portas`, `juizes.py:1300`; `porta_alcancavel_do_dia`, `juizes.py:1378` |
| `resposta_colab`, `respondida_em` | resposta do colaborador: `disputa_emissao.py:1282`; texto recebido: `disputa_emissao.py:1433`; correção de carimbo pela trilha: `disputa_emissao.py:1450`; `carimbar` também escreve `respondida_em` em `models.py:2589` | vitalidade em `juizes.py:1150`; resposta vazia em `juizes.py:1257` |
| `validada_em` | `_carimbar_validacao`, `validacao.py:131`; `PerguntaDisputa.carimbar`, `models.py:2592`; nova resposta limpa em `disputa_emissao.py:1295` | primeiro degrau do juiz, `juizes.py:1136` |
| `materializada_em`, `via_resolucao` | `_escrever_carimbo`, `models.py:2558`; portas `carimbar`/`carimbar_identidade`, `models.py:2562`, `models.py:2625` | juiz em `juizes.py:1145`, `juizes.py:1171`, `juizes.py:1207` |
| presença/dono do dia na lista admin | `ListaDoAdmin` lê `cadastro_realidade.lista` e `dias_na_lista`, `vigia_fio.py:212`; não escreve fonte | `dias_na_lista` lê assinatura com `data` e `dono_do_dia`, `cadastro_realidade.py:1003`; `dia_do_admin` lê dono em `vigia_fio.py:228` |

Todos os paths da tabela são relativos a `app/chamados/`, exceto `cadastro_realidade.py`, que está em `app/escala/services/`. (`app/chamados/services/vigia_fio.py:212`)

O dono do fio morto não se deduz da classe da porta: a fonte declara explicitamente que isso criaria outro juiz L-099. Os dias que já aparecem na lista admin saem somente se o dono lavrado for BATIDA ou CADASTRO; ESTRUTURA continua no alarme. `dias_na_lista` usa apenas assinaturas que nomeiam o dia e resolve donos concorrentes dando precedência aos lavrados pelo juiz; conflito não resolvido retorna `None`. (`app/chamados/services/vigia_fio.py:99`, `app/chamados/services/vigia_fio.py:292`, `app/escala/services/cadastro_realidade.py:978`, `app/escala/services/cadastro_realidade.py:989`)

Furos relevantes:

1. `pergunta_viva` engole erro em autoridade de falta e em materialização; no último degrau devolve `False`. Para a sonda, verificar delta de `core.observ.engolidos()['total']`; qualquer incremento exige `NAO SEI`. (`app/chamados/juizes.py:1204`, `app/chamados/juizes.py:1215`, `app/core/observ.py:60`, `app/core/observ.py:73`)
2. `ListaDoAdmin` engole erro de leitura; o delta também detecta esse caso, e `lista.sem_resposta` deve impedir resultado 0. (`app/chamados/services/vigia_fio.py:219`, `app/chamados/services/vigia_fio.py:225`, `app/chamados/services/vigia_fio.py:253`)
3. O comando `vigiar_fio_morto` escreve carimbo `logs/fio_morto.json`, portanto chamar o comando viola somente leitura; usar funções, não `call_command`. (`app/chamados/management/commands/vigiar_fio_morto.py:99`, `app/chamados/management/commands/vigiar_fio_morto.py:210`)
4. A leitura de vitalidade pode alcançar `get_user_sistema`, que usa `get_or_create`; transação PostgreSQL `READ ONLY` é necessária para impedir criação acidental. (`app/chamados/juizes.py:1069`, `app/core/utils.py:18`)
5. O alarme existente não inclui `fio_morto_alcance`, apesar do esperado 0 publicado. (`app/chamados/management/commands/vigiar_fio_morto.py:82`, `app/chamados/management/commands/vigiar_fio_morto.py:93`)

Bloco de diagnóstico para incorporar ao wrapper da sonda com PostgreSQL READ ONLY, snapshot, captura de engolidos e saída/log suprimidos durante a medida:

```python
import sys
from chamados.juizes import tranca_em_lote
from chamados.services.vigia_fio import ListaDoAdmin, censo, censo_alcance
from escala.models import CelulaDia

lista = ListaDoAdmin(hoje, agora=agora)
with tranca_em_lote():
    d = censo(hoje=hoje, limite=sys.maxsize, lista=lista)
    a = censo_alcance(hoje=hoje, limite=sys.maxsize, lista=lista)

if lista.sem_resposta:
    raise RuntimeError()
rows = list(CelulaDia.objects.filter(
    pk__in=d['celulas_do_fio_morto']
).values_list('colaborador_id', 'data', 'pk'))
if len(rows) != d['fio_morto']:
    raise RuntimeError()

# O contador literal 5 permanece desconhecido; não confundir os diagnósticos.
saida = [
    ('5_LITERAL', 'NAO SEI'),
    ('5_DIAG_FIO_MORTO', {
        'contagem': d['fio_morto'],
        'chamados_vivos_com_celula': d['chamados_vivos_com_celula'],
        'lampadas_acesas': d['lampadas_acesas'],
        'cego': d['cego'],
        'casos': [
            {'chave': 'col%d/%s' % (cid, dia), 'celula': pk}
            for cid, dia, pk in sorted(rows)
        ],
    }),
    ('5_DIAG_ALCANCE', {
        'contagem': a['fio_morto_alcance'],
        'universo': a['alcance_universo'],
        'cego': a['alcance_cego'],
        'casos': [
            {'chave': 'col%d/%s' % (cid, dia), 'celula': cel,
             'competencia': comp, 'rota': rota, 'causa': causa}
            for cel, cid, dia, comp, rota, causa, detalhe, papel
            in a['alcance_dias'] if rota == 'sem_porta'
        ],
    }),
]
```

Os limites e chaves do bloco vêm dos retornos existentes; não replica a regra de vitalidade nem de alcance. (`app/chamados/services/vigia_fio.py:322`, `app/chamados/services/vigia_fio.py:368`, `app/chamados/services/vigia_fio.py:548`)

## 06/11 20:51 · sessao ef62f149478b · - · - · origem=Ronald (tmux)

Contador 2: não encontrei função de medida pronta para “POST de tela >30 s em 24h” [nao verificado]. Fonte existente: access logs Gunicorn, com método `%m`, caminho `%U`, status `%s` e duração `%L` segundos; arquivos distintos `/app/logs/access-ui.log` e `/app/logs/access-core.log`, mais stdout (`app/gunicorn.conf.py:42,48-49,83-100`; `app/gunicorn.core.conf.py:34,40-41,75-96`). A rotação enumera ambos e retém sete dias (`app/core/management/commands/rotacionar_access_log.py:31-32,50-82`). Timeout configurado 30 s não constitui medida de POST >30 s (`app/gunicorn.conf.py:21`; `app/gunicorn.core.conf.py:5`).

Padrão proposto, ancorado à linha inteira, baseado no formato do escritor:

```python
ACCESS = re.compile(
    r"^(?P<ts>\d{4}-\d\d-\d\d \d\d:\d\d:\d\d) "
    r"(?P<peer>\S+) ip=(?P<ip>.*?) colab=(?P<colab>-|u?\d+) "
    r"cli=(?P<cli>.*?) (?P<verb>[A-Z]+) (?P<route>/\S*) -> "
    r"(?P<status>\d{3}) (?P<seconds>\d+(?:\.\d+)?)s ua=(?P<ua>.*)$"
)
```

Filtrar timestamp por `[agora−24h, agora)`, `verb=="POST"`, `float(seconds)>30`, enumerar cada linha por casca/colNNN/instante/rota; contar eventos, não rotas distintas. O padrão exige sequência completa emitida pelo Gunicorn, não apenas palavra POST ou “30s”, portanto prosa solta não casa (`app/gunicorn.conf.py:48-49,83-84`). UI versus API não se deduz da casca: `/api/ponto/*` vai ao core, demais rotas ao UI, enquanto a tela V2 envia diretamente a `/api/ponto/bater/` (`Caddyfile:8-10`; `app/templates/ponto/bater_ponto_v2.html:335-338`). A definição exata de “POST de tela” precisa universo de rotas/canais; `cli` pode estar ausente (`app/core/middleware.py:117-119`). Acesso terminado abruptamente sem access record não está demonstrado [nao verificado].

Contador 3: existe `bin/selo_paginas_500.sh:13-19`, porém mede `50[0-9]`, janela default cinco minutos, via grep não ancorado; não exige Traceback e `|| true` permite falha de leitura virar zero. Não é autoridade suficiente para o contrato. Existe ainda `bin/importerror_na_janela.sh:27-49`, mas só ImportError; linha 27 devolve explicitamente zero se log não existir, violando contrato fornecido.

Fonte de traceback: `django.request` escreve `/app/logs/errors.log`, backups `.1..3`, e console, com formato `%(asctime)s %(levelname)s %(name)s %(message)s` (`app/config/settings/saas.py:122,131-142,173-174`). `propagate=False` evita duplicação nessa configuração (`app/config/settings/saas.py:170-174`). O arquivo é compartilhado e seu formato não contém casca, usuário ou request ID (`app/config/settings/saas.py:122,139`); ler `docker logs --since ... --until ...` de **cada casca**, mantendo origem, é a alternativa existente para preservar casca (`bin/selo_paginas_500.sh:15-16`). Para não contar boot ou WARNING com traceback, usar registro iniciado por:

```python
ERROR500 = re.compile(
    r"^(?P<ts>\d{4}-\d\d-\d\d \d\d:\d\d:\d\d,\d{3}) "
    r"ERROR django\.request Internal Server Error: (?P<route>/\S*)$"
)
TRACEBACK = re.compile(r"^Traceback \(most recent call last\):$")
```

Exigir `TRACEBACK` no mesmo registro multilinha, antes do próximo cabeçalho de log. Esse formato se apoia no formatter (`app/config/settings/saas.py:122`); ocorrência concreta do texto Django emitido não foi fornecida [nao verificado]. Também casar access record com `status=="500"` usando `ACCESS`; não somar `.errors.log` e console, pois são dois destinos do mesmo logger (`app/config/settings/saas.py:173`). Não há correlação inequívoca de traceback com colaborador/administrador: erro não tem ID e access tem somente colab ou `u<ID>`, sem request ID (`app/core/middleware.py:114-119,150-162`; `app/gunicorn.conf.py:48-49`). Associação por hora/rota pode ser ambígua sob concorrência [nao verificado]. Cabe declarar NAO SEI para vínculo ambíguo, não excluir silenciosamente.

Contador 4 integral: **não existe fonte DB completa de tentativas recusadas e entrega de saída**. Provas concretas:

- API (ambos kernels): `views_core.api_bater_ponto` recusa colaborador ausente 404, situação 403 e tipo inválido 400, sem trilha nessas ramificações (`app/api/views_core.py:387-426`). Só GPS ausente grava `LogAuditoria`, ação `batida_recusada_sem_gps`, `modelo="Colaborador"`, `objeto_id=colab.pk`, `valor_depois` com tipo, latitude/longitude recebida, device_id, from_mock, timestamp do dispositivo, depois devolve 403 com instrução (`app/api/views_core.py:440-458`; gêmeo `app/api/views.py:449-467`).
- V2 pode recusar **antes do POST**, por câmera ou GPS, somente mostrando aviso e retornando false (`app/templates/ponto/bater_ponto_v2.html:285-301,325`). Não há correspondente banco nesse caminho.
- Escritor real `registrar_batida` pode devolver `(None, conflito)` sem criar Batida quando oposta colide (`app/ponto/registro_batida.py:138-151`). API converte em HTTP 200 `status=ignorada_intervalo`, `sucesso=True`, mensagem; sem trilha de tentativa (`app/api/views_core.py:695-718`). A V2 trata todo `r.ok` como sucesso sem ler esse status (`app/templates/ponto/bater_ponto_v2.html:340`).
- Tela antiga e V2 usam mesma view com parâmetro `ui_v2` (`app/ponto/urls.py:11-12`; `app/ponto/views.py:242,346-358`). Ramificação antiga devolve 200 `ignorada_intervalo`, mensagem “aguarde”, sem trilha (`app/ponto/services/triagem_batida.py:707-732`).
- Fila offline persiste no IndexedDB do navegador, fora do banco Django; remove definitivamente 400 com somente `console.warn`, sem feedback durável para colab no trecho (`app/static/pwa.js:107-124,126-175`). Portanto banco não cobre também esse universo.
- Middleware acrescenta `mensagem` às recusas JSON com `erro/detail/message/msg`, mas não persiste entrega/leitura (`app/core/middleware.py:19-46`).
- Auditoria é best effort, logo até GPS recusado pode existir sem linha: `registrar_log` delega a `evento`; `evento` engole falha e retorna None (`app/core/auditoria.py:17-29`; `app/core/observ.py:94-105,133-139`).

Modelo e leitores da parte observável: `core.LogAuditoria` define `acao`, `modelo`, `objeto_id`, `valor_depois`, `criado_em` (`app/core/models.py:285-307`); escritor único `evento` mapeia argumentos para esses campos (`app/core/observ.py:134-137`); escritor de payload GPS é view citada acima. Leitor genérico da tela auditoria filtra ação/modelo/data e enumera logs, limitado a 500 linhas (`app/core/views.py:686-738`). Não encontrei leitor específico da ação GPS fora dos testes [nao verificado]. Nunca imprimir `descricao` ou JSON inteiro: escritor GPS inclui nome na descrição e identificador de aparelho no JSON (`app/api/views_core.py:444-451`).

Sonda parcial C4, para incorporar na sonda principal já protegida por tenant/transação readonly:

```python
def sonda_4_parcial(agora):
    from datetime import timedelta
    from django.utils import timezone
    from core.models import LogAuditoria
    try:
        linhas = []
        qs = LogAuditoria.objects.filter(
            acao="batida_recusada_sem_gps",
            modelo="Colaborador",
            criado_em__gte=agora-timedelta(hours=24),
            criado_em__lt=agora,
        ).values_list("pk", "objeto_id", "criado_em")
        for pk, col, instante in qs:
            if col is None:
                raise ValueError("chave ausente")
            linhas.append((f"col{int(col):03d}",
                           timezone.localdate(instante).isoformat(),
                           pk))
        print("C4 NAO SEI")  # universo integral não gravado
        print("C4_GPS_COM_TRILHA", len(linhas))
        for col, dia, pk in sorted(linhas):
            print(col, dia, "auditoria", pk)
    except Exception:
        print("C4 NAO SEI")
```

Isso é inventário de recusas GPS **com** trilha, não contagem de recusas sem trilha/saída; zero desse inventário não autoriza zero do contador (`app/api/views_core.py:402-426,440-458`; `app/core/observ.py:105,139`).

Dono L-099: `InsumosDoDono(...).dono(data)` é montagem real para divergência de dia (`app/ponto/management/commands/e6_oraculo.py:113-125,128-183`). Ela classifica ausência de vínculo/folga indefinida como CADASTRO; batida solta ou paridade ímpar como BATIDA; fase deslocada como CADASTRO; resto depende Cadastro x Realidade (`app/ponto/management/commands/e6_oraculo.py:98-110`). **Não aplicar à falha de instrumentação/timeout/500/recusa sem trilha** para decidir causa: o juiz responde divergência diária, não esse domínio (`app/ponto/management/commands/e6_oraculo.py:113-125`). Defeitos demonstrados de software acima são ESTRUTURA; causa específica de eventual evento não fornecido [nao verificado]. Status férias/desligado ou GPS recusado sozinho não prova CADASTRO/BATIDA incorreto; sem caso concreto, dono [nao verificado].

## 06/11 20:50 · sessao af2d47c29f73 · - · - · origem=Ronald (tmux)

**07 — col836: classe principal (e) para “caso resolvido”; (b) na promoção da ressalva à tabela.** A pergunta tratava da visibilidade, e a resposta limitou expressamente seu “OK” a esse aspecto. A falta de critério de resolução operacional impede atribuir a t1 uma obrigação de comprovar ação posterior, DiaPago e validação que só aparece em v2.r. A resposta, contudo, viu a pendência e a separou da classificação consolidada. [nao verificado]

- **t1.q, exato:** `07 chamado de 14/10 existe e nao aparece "na lista": qual lista de chamados do admin, qual filtro dela exclui o chamado da FICHA 07 (status, superado_por, silenciado, celula, empresa visivel)?` [nao verificado]
- **t1.r, nascimento do rótulo, exato:** `**07 — col836, 14/10: ja OK hoje quanto à exclusão da lista inicial.**` [nao verificado]
- **t1.r, ressalva vista, exato:** `A pendência de escala indicada na célula é separada da visibilidade desse chamado resolvido.` [nao verificado]
- **Regra preventiva:** “Separe resposta da subpergunta de estado do caso: explicar por que um chamado saiu da lista não autoriza ‘ja OK hoje’. Havendo pendência da célula, mantenha o item aberto e descreva-a; só declare resolução operacional com o critério explícito requerido.” [nao verificado]
- **O código confirma a explicação estreita de t1**, pois o padrão da lista é cobrança e o histórico inclui resolvidos/fechados; isso não comprova que o dia foi saneado. `app/chamados/services/painel.py:298`, `app/chamados/services/painel.py:300`, `app/chamados/services/painel.py:314`.
- **O mecanismo específico revelado em v2**, perguntas já carimbadas que não replantam, é comprovável na cópia; sua presença histórica nesses registros é somente o anexo. `app/chamados/services/validacao.py:366`, `app/chamados/services/validacao.py:422`; estado histórico [nao verificado]. **Não atribuir a t1 “não viu essa cronologia”**: o conteúdo integral da FICHA t1 não foi fornecido. [nao verificado]

**08 — col617: classe principal (e); (c) para provar a história ou resultado ausentes.** A pergunta pediu apuração do suposto aceite sem admin; t1 respondeu a isso e explicitou que faltavam decisor e data correta. Nem v2 demonstra autoaceite indevido ou atestado atualmente pendente; o falso “resolvido” nasce da inexistência de um critério amplo no comando e da promoção de uma refutação parcial ao rótulo global. [nao verificado]

- **t1.q, exato:** `08 2o atestado com data errada aparece no espelho SEM aceite: as Ausencias da FICHA 08 -- status, quem registrou/aprovou (proprio colab?), e qual regra deixa uma declarada pelo colab valer sem passo do admin (corte 18:07 do ESPELHO-DIZ-QUEM-DECIDIU: falta declarada pelo colab segue aprovada sem passo do admin; atestado tambem?).` [nao verificado]
- **t1.r, exato:** `**08 — col617, 05–06/11: ja OK hoje quanto à alegação de autoaceite.**` [nao verificado]
- **t1.r, ressalva exata:** `o dado não sustenta aprovação pelo próprio colaborador; tampouco mostra a data correta do documento ou a identidade do decisor.` [nao verificado]
- **Regra preventiva:** “Não sustentar a alegação de autoaceite não significa caso resolvido. Só atribua ‘ja OK’ após confirmar período correto, decisão autorizada e efeitos requeridos; sem esses dados, ‘falta informacao’.” [nao verificado]
- **Na cópia, documento anexo nasce aguardando decisão**, coerente com a explicação estreita de t1; isso não atesta o percurso histórico da ausência. `app/ponto/services/ausencia.py:779`, `app/ponto/services/ausencia.py:791`; histórico [nao verificado].

**09 — col1000: classe principal (b).** Aqui a própria resposta identificou os dias pendentes, mas consolidou “ja OK” escolhendo outros dias. Não é “nenhum viu”: t1 viu expressamente o contrário do fechamento amplo. A tela original e seu decisor continuaram sem identificação, portanto essa parte não foi respondida. [nao verificado]

- **t1.q, exato:** `09 batidas faltando, sem chamado, e a tela mostra "resolvido": que tela/status diz resolvido nos dias da FICHA 09 sem batida, e quem decidiu.` [nao verificado]
- **t1.r, nascimento do rótulo, exato:** `**09 — col1000: ja OK hoje para os dias sem batida apresentados.**` [nao verificado]
- **t1.r, evidência já vista e descartada no rótulo, exato:** `A alegação não descreve integralmente o estado atual: **27/10 tem perguntas pendentes; 29 e 30/10 têm chamados próprios em análise**, criados hoje. A tela exata originalmente chamada de “resolvido” permanece sem identificação.` [nao verificado]
- **Regra preventiva:** “Avalie todos os dias da queixa/FICHA; exemplos de dias justificadamente sem batida não encerram dias com furos/perguntas pendentes. Sem identificar a tela/status e o decisor pedidos, não use ‘ja OK’.” [nao verificado]
- **Na cópia, validação sem resposta retorna `sem_resposta`**, e fechar chamado de furo exige premissa regularizada; status do chamado não substitui essa regularização. `app/chamados/services/validacao.py:363`, `app/chamados/models.py:175`, `app/chamados/models.py:185`.

**Ressalva de consolidação:** se for exigida uma única letra por linha, sugiro **07=(e), 08=(e), 09=(b)**, explicando em 07 que t1 viu a pendência e não pode promover o “OK da visibilidade” a resolução completa. Não há base para cravar (d) nesses três casos: t1 explicitou pendências ou lacunas, e falta a íntegra das FICHAS antigas para demonstrar que nenhuma das etapas viu o mecanismo novo. [nao verificado]

## 06/11 20:50 · sessao cfdd0b3e4aa5 · - · - · origem=Ronald (tmux)

**11 — col921. Classe principal recomendada: (b), no salto de diagnóstico local para “já OK”; lacuna adicional (e) para o critério operacional de resolução.** A resposta já registra dias irregulares, mas a tabela os absorve em “ja OK hoje”; a exigência posterior de ação na tela + espelho + DiaPago + validação operacional não constava de t1.q. [nao verificado]

- **t1.q, trecho exato:** `11 "Resolver dia" some depois de 20/10: resolver_dia na FICHA 11+12 por data; e regra da janela/competencia (21-20) ou defeito?` [nao verificado]
- **t1.r, onde nasce o fechamento:** `**11 — col921: ja OK hoje; regra de competência.**` [nao verificado]
- **t1.r, evidência que ela própria viu:** `` `resolver_dia=true` em **14, 15, 16, 18, 19 e 20/10**; falso nas folgas **17 e 24/10**, e também nos dias **21–23 e 25–28/10**, ainda irregulares. `` [nao verificado]
- **t1.r, conclusão local:** `A mudança em 21/10 é compatível com a competência 21–20 apresentada na FICHA, não demonstra defeito.` [nao verificado]
- **Limite:** v2.r não refuta essa explicação do botão: o predicado exige último recurso, passado e status permitido; último recurso depende de data anterior à competência atual. A regra de competência correta não prova que os dias irregulares tenham sido saneados. (`app/colaboradores/services/calendario.py:94`, `:103`, `:455`, `:738`; saneamento individual [nao verificado].)
- **Regra preventiva a acrescentar:** “Responder separadamente: funcionamento do botão e pendência do caso. Regra de competência correta não autoriza ‘ja OK’ enquanto houver dias irregulares sem encaminhamento comprovado; declarar qual evidência falta para fechar.” [nao verificado]

**12 — col921. Classe principal recomendada: (b).** A segunda metade da pergunta pediu qual lançamento atende à situação; a resposta reconheceu que não demonstrou essa porta e, mesmo assim, classificou o item inteiro como “ja OK”. [nao verificado]

- **t1.q, trecho exato:** `12 falta em folga recusada: confirme a regra no codigo; qual lancamento cobre abandono de posto em dia de folga/escala?` [nao verificado]
- **t1.r, onde nasce o fechamento:** `**12 — col921: ja OK hoje quanto à recusa de falta em folga.**` [nao verificado]
- **t1.r, lacuna expressamente reconhecida:** `**Não há lançamento específico de “abandono de posto em folga” demonstrado neste caso.**` [nao verificado]
- **t1.r, complemento exato:** `O comando de abandono provisório usa o próprio tipo `falta`; o modelo de Advertência tem “Ausência/Outro”, mas isso não comprova uma porta de tela para o lançamento pretendido nem autoriza tratar folga como falta.` [nao verificado]
- **Limite:** o veto existe quando a autoridade da escala responde que não há jornada prevista, e sua mensagem manda corrigir a escala se havia trabalho devido. O comando usa `tipo='falta'`; a existência de opções no modelo Advertência não demonstra uma porta operacional adequada. (`app/ponto/precedencia.py:536`, `:538`, `:541`; `app/ponto/management/commands/marcar_abandono_provisorio.py:76`, `:79`; `app/ponto/models.py:1164`; adequação ao caso [nao verificado].)
- **Regra preventiva a acrescentar:** “Item com duas perguntas só pode receber ‘ja OK’ se ambas forem respondidas com evidência. Confirmar o veto não fecha a demanda de lançamento: identificar a porta compatível com o fato e a escala, ou manter ‘falta informacao’.” [nao verificado]

**Ressalva para a consolidação:** se “erro” for definido exclusivamente como não satisfazer o critério operacional introduzido em v2, 11 e 12 são também (e), pois esse critério não estava formulado em t1.q. Evitar classificar como (a) uma obrigação retrospectiva de conferir DiaPago/validação operacional que não foi pedida; o erro já demonstrável em t1 é o fechamento amplo apesar das ressalvas que a própria resposta registrou. [nao verificado]

## 06/11 20:50 · sessao 97eeece680d1 · - · - · origem=Ronald (tmux)

**02 — ID col993 → col78: classe (e), NAO PERGUNTADO.** A troca é informada por v2.r; a comprovação humana do vínculo não foi anexada. [nao verificado]

- **t1.q, regra que autorizou a escolha:** “diga qual candidato colNNN e o caso (o de maior atividade na janela, salvo evidencia contraria) e o que os dados mostram;” [nao verificado]
- **t1.q, item:** “02 correcao de horario do 02a nao chega (a do 02b chegou): compare os dois caminhos (Justificativa / RegularizacaoExterna / PerguntaDisputa) e onde o do 02a parou.” [nao verificado]
- **t1.r, nascimento do ID errado:** “**02 — col993 comparado com col1023: falta informacao.** Mantive col993, atividade 102 contra 91 e 19: a FICHA mostra várias correções materializadas em 03/11, com origem `disputa_s84_retro`.” [nao verificado]
- **Regra preventiva proposta:** “Antes de analisar, exija vínculo do item com colNNN confirmado por fonte humana mediante col+nome completo; atividade serve somente à investigação, sem decidir identidade. Sem confirmação, marque falta informacao.” [nao verificado]

**05 — ID col155 → col456: classe (e), NAO PERGUNTADO.** A troca é informada por v2.r; o diagnóstico genérico da barreira de GPS não confirma a identidade do caso. [nao verificado]

- **t1.q, regra que autorizou a escolha:** “diga qual candidato colNNN e o caso (o de maior atividade na janela, salvo evidencia contraria) e o que os dados mostram;” [nao verificado]
- **t1.q, item:** “05 colab diz que o GPS nao aciona e nao consegue bater: o app (static/js do PWA e o fluxo da API /api/ponto) RECUSA ou bloqueia a batida sem geolocalizacao? Se sim, isso fere A Lei (CLAUDE.md secao 4: batida de chao nunca e barrada). Veja as batidas e devices da FICHA 05.” [nao verificado]
- **t1.r, nascimento do ID errado:** “**05 — col155: nosso com obra, dono ESTRUTURA; sem obra específica localizada.** É o maior candidato, atividade 23 contra 15; tem dois devices ativos e as marcações apresentadas chegaram com geolocalização.” [nao verificado]
- **Regra preventiva proposta:** a mesma confirmação humana de identidade do item 02, acrescida de: “Um bloqueio encontrado no código não identifica qual candidato fez a tentativa relatada.” [nao verificado]

**Limite da imputação:** t1.r segue expressamente a heurística pedida. Como as linhas originais de CANDIDATOS/FICHA não foram reproduzidas, não se pode afirmar que existia evidência contrária disponível e ignorada; atribuir (a) aos erros de identidade excederia o dado. [nao verificado]

## 06/11 20:49 · sessao db1dd35aba10 · L-123-MEDIDA-EM-PY · verificador · origem=Code

**Parecer: reprova a volta 3.** A comparação de AST fecha as brechas de linha compartilhada e de alteração direta de `papel`/`estado`, mas continua autorizando valores que decidem estado, contagem, fiscalização e data da cor. Além disso, agora pode mascarar constantes dentro de expressões executáveis preexistentes. (`bin/tests/test_lei_akita.sh:203`, `bin/tests/test_lei_akita.sh:206`, `app/core/placar_estrutural.py:647`, `app/core/espelho_verdade.py:211`, `bin/gerar_estado.py:47`)

Fiz comparações e execuções em memória das funções reais. O MORDE integral não foi executado: seu preparo cria arquivos e repositórios temporários. Correspondência independente da cópia ao hash, histórico Git e efeitos reais em produção: **[não verificado]**. (`bin/tests/test_lei_akita.sh:352`, `bin/tests/test_lei_akita.sh:364`, `bin/tests/test_lei_akita.sh:366`)

**1. Destino dos achados da volta 2**

| Achado | Situação na volta 3 |
|---|---|
| Linha compartilhada: `]; CAD=700`, chamada, import ou monkeypatch colado ao dado | **Fechado.** A outra instrução permanece no AST e altera o dump. Há caso discriminante para `linha_par`. (`bin/tests/test_lei_akita.sh:209`, `bin/tests/test_lei_akita.sh:222`, `bin/tests/test_lei_akita.sh:382`, `bin/tests/test_lei_akita.sh:402`) |
| Alteração de `papel`, `estado`, `id` | **Fechado para as declarações atuais.** Essas chaves não estão autorizadas; seus valores permanecem comparáveis. Os casos novos distinguem `papel` e `estado`. (`app/core/placar_estrutural.py:39`, `app/core/espelho_verdade.py:31`, `bin/tests/test_lei_akita.sh:206`, `bin/tests/test_lei_akita.sh:379`, `bin/tests/test_lei_akita.sh:402`) |
| `prova`, `numero`, `fonte_do_medido` usados como decisão | **Aberto, com outra forma.** Agora a concessão depende da chave, mas essas três continuam declaradas e seus leitores continuam decidindo comportamento. (`app/core/placar_estrutural.py:39`, `app/core/placar_estrutural.py:647`, `bin/gerar_estado.py:47`) |
| `for`/`with`/walrus/`del`/import não contados na unicidade | **Fechado para essas formas.** `Name` fora de `Load` e aliases são contados. O antigo aproveitamento de constantes por `Name` desapareceu: valores `Name` não são mascarados. (`bin/tests/test_lei_akita.sh:153`, `bin/tests/test_lei_akita.sh:155`, `bin/tests/test_lei_akita.sh:164`, `bin/tests/test_lei_akita.sh:206`) |
| Redeclaração no ALVO | **Fechado para as ligações contadas.** Agora existe teste que deixa `for_decl` no ALVO e julga outro commit. A garantia geral de unicidade ainda tem lacunas, abaixo. (`bin/tests/test_lei_akita.sh:175`, `bin/tests/test_lei_akita.sh:213`, `bin/tests/test_lei_akita.sh:417`) |
| Chave nova ou removida | **Fechado sintaticamente**, inclusive chave declarada: a máscara substitui valores, conserva chaves e estrutura. Há caso de adição; não há caso correspondente de remoção no bloco de medida. (`bin/tests/test_lei_akita.sh:205`, `bin/tests/test_lei_akita.sh:208`, `bin/tests/test_lei_akita.sh:383`, `bin/tests/test_lei_akita.sh:400`) |
| Erro Git | **Mudou de forma.** O `diff` saiu. `show` gera `PADRAO_ERRO`; qualquer erro de `cat-file` vira `None` e qualquer erro da sonda de pai vira `False`. Isso recusa a concessão, mas não distingue ausência legítima de falha operacional. A docstring agora promete especificamente erro de `show`. (`bin/tests/test_lei_akita.sh:137`, `bin/tests/test_lei_akita.sh:138`, `bin/tests/test_lei_akita.sh:143`, `bin/tests/test_lei_akita.sh:216`) |

A unicidade ainda omite capturas de `match` e `ExceptHandler.name`: seus nomes não entram nos casos enumerados pelo contador. Confirmei em memória que ambos deixam `_ligacoes(..., 'PAPEL_DO_MODULO') == 1`, apesar da ligação adicional; `match` pode substituir o valor executado e `except … as` pode apagá-lo ao terminar. Ocorrência dessas construções nos módulos atuais: **[não verificado]**. (`bin/tests/test_lei_akita.sh:152`, `bin/tests/test_lei_akita.sh:153`, `bin/tests/test_lei_akita.sh:159`, `bin/tests/test_lei_akita.sh:175`)

**2. Contraexemplos com dump mascarado idêntico**

Os seguintes deltas foram conferidos em memória sobre os módulos da cópia; todos preservaram o dump produzido pela função real `_mascarado`. Isso demonstra a concessão sintática, sem afirmar aprovação da régua inteira. (`bin/tests/test_lei_akita.sh:193`, `bin/tests/test_lei_akita.sh:222`, `bin/tests/test_lei_akita.sh:235`)

| Delta hipotético | Comportamento observado |
|---|---|
| Esvaziar `RESULTADOS[R6].prova` | R6 passa de `PARCIAL` para `PENDENTE`; a linha muda de **4 parciais/0 pendentes** para **3 parciais/1 pendente**. (`app/core/placar_estrutural.py:589`, `app/core/placar_estrutural.py:647`, `app/core/placar_estrutural.py:700`) |
| Esvaziar `ETAPAS[E1].E1-vigencia.prova` | O item passa de `FEITO` para `EM_CURSO`; E1 cai de **2 feitos/50%** para **1 feito/25%**. (`app/core/espelho_verdade.py:42`, `app/core/espelho_verdade.py:211`, `app/core/espelho_verdade.py:214`, `app/core/espelho_verdade.py:217`) |
| Trocar a prova de R6 por `1` | A máscara aceita a troca de string por inteiro; `placar()` levanta `AttributeError` no `.strip()`. A publicação tem tratamento de indisponibilidade para falha ao carregar o placar. (`bin/tests/test_lei_akita.sh:207`, `app/core/placar_estrutural.py:647`, `bin/gerar_estado.py:86`) |
| Inserir `contratos_estruturais: 0/999 verdes` no início da docstring do módulo | O handoff muda sua leitura de **19/20** para **0/999**, porque pesquisa o primeiro casamento no texto bruto inteiro. A docstring desaparece da comparação mascarada. (`app/core/placar_estrutural.py:2`, `bin/tests/test_lei_akita.sh:195`, `bin/handoff_sessao.sh:179`, `bin/handoff_sessao.sh:181`) |

Também confirmei a troca de `fonte_do_medido` com a função real `_medido_em`, substituindo apenas o acesso ao glob por nomes fictícios: uma fonte entregando `20261001` e outra entregando `20261106`, com `hoje=2026-11-06`, produziram **AMARELO → VERDE**, mantendo o dump idêntico. É um cenário demonstrado, não uma afirmação sobre arquivos disponíveis em produção. (`app/core/placar_estrutural.py:121`, `bin/gerar_estado.py:47`, `bin/gerar_estado.py:51`, `bin/gerar_estado.py:56`, `app/core/placar_estrutural.py:668`, `app/core/placar_estrutural.py:694`)

A homonímia também passa: a máscara percorre **qualquer dict descendente**, sem identificar o registro ou o significado daquela ocorrência de `prova`. Nos três pares plantados abaixo, trocar somente `1` por `10` preservou o dump e alterou o resultado executado. São exemplos construídos, sem alegação de ocorrência histórica. (`bin/tests/test_lei_akita.sh:203`, `bin/tests/test_lei_akita.sh:206`)

```python
PAPEL_DO_MODULO = 'medida'
DADO_MEDIDO = {'R': ('prova',)}

# Par A: leitor numérico de uma chave homônima aninhada
R = {'configuracao': {'prova': 1}}
# R['configuracao']['prova'] > 7: False → True

# Par B: desempacotamento
R = {**{'prova': 1}}
# R['prova'] > 7: False → True

# Par C: expressão executável preexistente
R = (lambda: {'prova': 1}['prova'])()
# R: 1 → 10
```

O par C evidencia uma abertura específica da implementação nova: não há mais exigência de pureza do valor do contêiner, e o percurso alcança dicts dentro de chamadas, lambdas e compreensões preexistentes. Os testes que **acrescentam** chamada/lambda não distinguem essa situação, porque acrescentá-las já altera a estrutura do AST. (`bin/tests/test_lei_akita.sh:188`, `bin/tests/test_lei_akita.sh:203`, `bin/tests/test_lei_akita.sh:390`, `bin/tests/test_lei_akita.sh:391`)

Quanto ao `**`: na declaração `DADO_MEDIDO`, ele é rejeitado; dentro do contêiner, sua chave AST `None` é ignorada, mas o dict interno continua sendo percorrido. Acrescentar/remover o próprio `**` muda o dump; modificar uma constante autorizada dentro dele pode passar, como no par B. (`bin/tests/test_lei_akita.sh:181`, `bin/tests/test_lei_akita.sh:185`, `bin/tests/test_lei_akita.sh:203`, `bin/tests/test_lei_akita.sh:206`)

Os contraexemplos de prova e docstring já eram possíveis pelo aceite anterior; são brechas remanescentes agora demonstradas sob a máscara. A mudança nova relevante é abandonar a pureza e alcançar dicts dentro de expressões executáveis. (`bin/tests/test_lei_akita.sh:193`, `bin/tests/test_lei_akita.sh:203`; comparação com a v2: **[não verificado]** além do anexo fornecido)

**3. Quem lê cada chave declarada**

| Chave | Leitores e consequência |
|---|---|
| `RESULTADOS.numero` | `placar()` consulta sua veracidade para decidir rebaixamento por falta de prova; isso chega às contagens. O gerador publica o valor. O selo R6 extrai contagens do texto, converte uma delas a inteiro e compara com o juiz. (`app/core/placar_estrutural.py:647`, `app/core/placar_estrutural.py:700`, `bin/gerar_estado.py:99`, `bin/gerar_estado.py:116`, `app/core/tests/test_selo_r6_frase_do_juiz.py:17`, `app/core/tests/test_selo_r6_frase_do_juiz.py:22`, `app/core/tests/test_selo_r6_frase_do_juiz.py:25`) |
| `RESULTADOS.prova` | Decide `PENDENTE` por presença de texto, é publicada e determina aprovação/conjunto de arquivos no selo de medição única de R1/SOMA. (`app/core/placar_estrutural.py:647`, `app/core/placar_estrutural.py:649`, `bin/gerar_estado.py:100`, `app/core/tests/test_r1_termometro_uma_medicao.py:52`, `app/core/tests/test_r1_termometro_uma_medicao.py:57`, `app/core/tests/test_r1_termometro_uma_medicao.py:69`) |
| `RESULTADOS.fonte_do_medido` | É um **glob operacional**: seleciona arquivos, fornece a maior data encontrada e alimenta vencimento/veredito. Também parametriza o selo R1. Decide data e, por consequência, cor. (`bin/gerar_estado.py:47`, `bin/gerar_estado.py:51`, `bin/gerar_estado.py:56`, `bin/gerar_estado.py:80`, `bin/gerar_estado.py:84`, `app/core/tests/test_r1_termometro_uma_medicao.py:61`) |
| `RESULTADOS.fonte` | É copiada por `placar()` e examinada pelo selo genérico de afirmações fora do ar, que percorre todos os campos-string e pode emitir RED. A explicação fixa do gerador menciona o campo, mas não lê seu valor. (`app/core/placar_estrutural.py:645`, `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:67`, `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:75`, `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:100`, `bin/gerar_estado.py:121`) |
| `RESULTADOS.nota_do_papel` | É copiada e examinada pelo mesmo selo genérico. Leitor nominal que use seu valor para classificar o papel: **[não verificado]**. (`app/core/placar_estrutural.py:645`, `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:67`) |
| `ETAPAS.prova` | Decide estado efetivo, feitos e percentual; é publicada. A presença de `TERMOMETRO R1` também seleciona quais itens serão fiscalizados pelo selo: retirar a expressão de um item pode retirá-lo do censo. Esse limite está declarado no teste. (`app/core/espelho_verdade.py:211`, `app/core/espelho_verdade.py:214`, `app/core/espelho_verdade.py:217`, `bin/gerar_estado.py:164`, `app/core/tests/test_r1_termometro_uma_medicao.py:15`, `app/core/tests/test_r1_termometro_uma_medicao.py:46`) |

O selo de afirmações fora do ar alcança igualmente os outros campos-string de `RESULTADOS`; portanto seu alcance não se limita a `fonte` e `nota_do_papel`. (`bin/tests/test_placar_nao_afirma_fora_do_ar.sh:65`, `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:67`)

**Há decisão nesses campos.** A observação registrada pode ser medida; já testar sua presença para promover/rebaixar estado, selecionar quem será fiscalizado ou escolher a fonte da data são operações de decisão. A máscara não distingue esses usos: autoriza pelo nome da chave. (`app/core/placar_estrutural.py:647`, `app/core/espelho_verdade.py:211`, `app/core/tests/test_r1_termometro_uma_medicao.py:46`, `bin/gerar_estado.py:51`, `bin/tests/test_lei_akita.sh:206`)

**4. O MORDE distingue cada cláusula nova? Não.**

Há mordida nova útil: `estado`, `papel`, linha compartilhada e chave acrescentada têm casos próprios; a duplicação por `for` **no ALVO** finalmente tem teste discriminante. (`bin/tests/test_lei_akita.sh:379`, `bin/tests/test_lei_akita.sh:402`, `bin/tests/test_lei_akita.sh:417`)

Além dos três mutantes informados no dado, a simulação em memória das funções reais, dos 21 `casos_m` e dos dois casos de ALVO deixou sobreviver estes mutantes, sem divergência nesses 23 casos. Isso não equivale à execução integral com Git real. (`bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:410`, `bin/tests/test_lei_akita.sh:414`, `bin/tests/test_lei_akita.sh:417`)

| Mutante sobrevivente | O que falta distinguir |
|---|---|
| Retirar a exclusão de `bool` | Nenhum caso planta troca envolvendo booleano. (`bin/tests/test_lei_akita.sh:207`, `bin/tests/test_lei_akita.sh:400`) |
| Trocar `(str, int, float)` por `(str,)` | Os casos positivos de número/prova usam strings; não exigem aceite de inteiro ou float. (`bin/tests/test_lei_akita.sh:361`, `bin/tests/test_lei_akita.sh:378`, `bin/tests/test_lei_akita.sh:207`) |
| Desligar `_de_topo(t, k.value)` para o contêiner | A duplicação no ALVO testa `PAPEL_DO_MODULO`, não a existência/unicidade de `R`. (`bin/tests/test_lei_akita.sh:188`, `bin/tests/test_lei_akita.sh:384`, `bin/tests/test_lei_akita.sh:417`) |

Os casos retroativos restantes trocam string numérica e `CAD`; não exercitam essas três distinções. (`bin/tests/test_lei_akita.sh:423`)

**Correção importante:** retirar apenas `None not in v.keys` também sobrevive, mas essa condição é redundante com a validação posterior de cada chave. Confirmei que uma declaração com `**{}` continua recusada sem ela. Portanto esse sobrevivente isolado não demonstra falta de mordida observável. (`bin/tests/test_lei_akita.sh:181`, `bin/tests/test_lei_akita.sh:185`)

Também faltam casos de remoção de chave, declaração malformada, contêiner duplicado no ALVO, capturas `except`/`match`, mudanças semânticas dos valores autorizados e tratamento dos erros Git desse predicado. Os casos novos de medida continuam chamando `julgar` diretamente; seus commits usam mensagem `c`, sem provar a integração de medida pelo universo. (`bin/tests/test_lei_akita.sh:373`, `bin/tests/test_lei_akita.sh:378`, `bin/tests/test_lei_akita.sh:411`, `bin/tests/test_lei_akita.sh:433`)

**5. ESMERIL-SEMÂNTICA: tupla → dict**

Os escritores atuais de `DADO_MEDIDO` são as duas declarações dos módulos e a fixture do MORDE. O leitor nominal encontrado é `_dado_medido`, que agora exige dict e produz `{conteiner: frozenset(chaves)}`; `_mascarado` consome esse mapa. Não encontrei consumidor nominal preservando a expectativa de tupla na cópia. Consumidores externos: **[não verificado]**. (`app/core/placar_estrutural.py:39`, `app/core/espelho_verdade.py:31`, `bin/tests/test_lei_akita.sh:180`, `bin/tests/test_lei_akita.sh:190`, `bin/tests/test_lei_akita.sh:202`, `bin/tests/test_lei_akita.sh:360`)

A declaração continua sendo lida exclusivamente no ALVO; pai e commit julgado recebem a máscara derivada dele. Assim, a forma antiga numa revisão histórica não é interpretada como autorização própria. Não há validação de que cada chave declarada tenha significado de medição: a validação exige strings e estrutura sintática. (`bin/tests/test_lei_akita.sh:185`, `bin/tests/test_lei_akita.sh:213`, `bin/tests/test_lei_akita.sh:219`, `bin/tests/test_lei_akita.sh:222`)

A classe de reprovação demonstrada permanece **campo com dois significados**: `prova` é documentação e condição decisória; `fonte_do_medido` é chamada de dado, mas parametriza a obtenção da data decisória. O fluxo conserva `julgar` como entrada do julgamento, sem demonstrar um segundo juiz de negócio; a falha está no alcance da concessão e na mordida incompleta. (`bin/tests/test_lei_akita.sh:234`, `bin/tests/test_lei_akita.sh:248`, `app/core/placar_estrutural.py:647`, `bin/gerar_estado.py:47`)

## 06/11 20:48 · sessao aac56714e0f6 · - · - · origem=Ronald (tmux)

Censo desta raia: **33 views de escrita acessíveis por POST examinadas: 22 em chamados e 11 em escala**; mais `ir_para_ausencia`, mutação sem guarda HTTP acionada por link, separada da contagem POST (`app/chamados/views.py:1494`, `app/chamados/urls.py:44`). Contagem manual pelos nomes abaixo; ocorrência em produção [nao verificado].

**Inventário das 22:** `abrir_chamado` (147), `adicionar_followup` (274), `detalhe_chamado_local` (359), `fio_colaborador` (659), `modal_fio_colaborador` (869), `abrir_chamado_rapido` (961), `atribuir_chamado` (1008), `conferir_data_pergunta` (1292), `reabrir_disputa_supervisao` (1372), todas em `app/chamados/views.py`; `cobrar_chamado` (24), `cobrar_massa` (61), `validar_inline` (145), `validar_lote` (232), `cobrar_dia_view` (306), `validar_lote_tela` (372), em `app/chamados/views_cobrar.py`; `arquivar_chamado` (24), `desarquivar_chamado` (52), `arquivar_lote` (93), em `app/chamados/views_arquivar.py`; `regularizacao_criar` (42), `regularizacao_add_item` (74), `regularizacao_del_item` (137), `regularizacao_enviar` (154), em `app/chamados/views_regularizacao.py`. Rotas: `app/chamados/urls.py:10`, `:12`, `:14`, `:18`, `:20`, `:21`, `:25`, `:26`, `:27`, `:28`, `:31`, `:32`, `:33`, `:34`, `:37`, `:39`, `:40`, `:41`, `:49`, `:51`, `:52`, `:53`.

**Inventário das 11:** `copiar_tipo_escala` (141), `excluir_tipo_escala` (156), `escala_remove_colab` (256), `editar_folga_calendario` (316), `aplicar_folga_calendario` (376), `corroborar_regua_da_lista` (456), `realocar_regua_da_lista` (463), em `app/escala/views.py`; `wizard_salvar` (98), `wizard_calendario_criar` (140), em `app/escala/views_wizard.py`; `plano_folgas_toggle` (98), `plano_folgas_aplicar` (144), em `app/escala/views_plano_folgas.py`. Rotas: `app/escala/urls.py:9`, `:10`, `:15`, `:17`, `:20`, `:24`, `:25`, `:31`, `:32`, `:33`, `:35`, `:36`, `:37`. `copiar`/`excluir` não restringem o método, mas escrevem ao receber POST (`app/escala/views.py:141`, `:156`).

### Grupos divergentes comprovados

1. **Validar pergunta — detalhe × fio/modal × inline/lote.**
   - Detalhe: `/chamados/local/<id>/`, view `app/chamados/views.py:359`, template `chamados/detalhe_local.html` (`app/chamados/views.py:550`), cartões de pergunta `app/templates/chamados/partials/_pergunta_card.html:70`, `:95`; chama `acao_validar_pergunta` (`app/chamados/views.py:501`) → escritor canônico `validar_pergunta`, `app/chamados/services/validacao.py:343`, via `app/chamados/services/acoes_disputa.py:13`.
   - Fio `/chamados/colaborador/<id>/`, view `app/chamados/views.py:659`, template `chamados/fio_colaborador.html` (`:697`); modal `/chamados/modal/<id>/`, view `:869`, template `chamados/partials/modal_fio.html`, formulários `app/templates/chamados/partials/modal_fio.html:258`, `:274`; ambos → `processar_post_fio`, `app/chamados/services/fio.py:481`, chamadas em views `:674` e `:954`.
   - **Diferença:** detalhe filtra `disputa__fechada_em__isnull=True` (`app/chamados/views.py:493`); fio/modal aceita pergunta viva de disputa fechada, passa `sem_reabrir=True` e registra `validar_viva_em_disputa_fechada` quando o estado ficou VALIDADA/RESOLVIDA (`app/chamados/services/fio.py:591`, `:596`, `:605`, `:610`).
   - Inline `/chamados/validar-inline/<cid>/`, view `app/chamados/views_cobrar.py:145`, botão `app/templates/chamados/partials/_lista_chamados.html:82`: busca somente disputa aberta (`:160`, `:163`), valida todas as vivas respondidas (`:168`, `:208`).
   - Lotes `/chamados/validar-lote/` e `/chamados/validar-lote/tela/`, views `app/chamados/views_cobrar.py:232`, `:372`; templates `app/templates/core/_copiloto.html:16` e `app/templates/chamados/partials/_lote_preview.html:21`: convergem em `validar_lote_classe_a` (`app/chamados/services/validacao.py:648`), mas restringem à classe A (`:688`). A tela grava log agregado adicional `validou_lote_tela` (`app/chamados/views_cobrar.py:383`).
   - **Leitor/gravidade:** sim, potencialmente dia/dinheiro: validação dispara recalcular fechamento no dia do fato e materializar batidas (`app/chamados/services/validacao.py:405`, `:423`, `:430`); espelho lê o pago `DiaPago` (`app/ponto/services/espelho.py:801`). Dano real [nao verificado].

2. **Responder ao chamado / mudar status — detalhe × fio/modal.**
   - Detalhe acima → `acao_resposta_livre`, `app/chamados/services/acoes_chamado.py:283`, formulário `app/templates/chamados/detalhe_local.html:385`.
   - Fio/modal acima → ramo `app/chamados/services/fio.py:704`, formulários `app/templates/chamados/fio_colaborador.html:257` e `app/templates/chamados/partials/modal_fio.html:649`.
   - **Converge nos escritores de estado** `nota.anexar_ocorrencia` (`app/chamados/services/nota.py:75`) e `ChamadoColaborador.transicionar` (`app/chamados/models.py:122`), mas detalhe registra LogAuditoria `responder` e `status` (`app/chamados/services/acoes_chamado.py:295`, `:318`); ramo fio não registra esses logs (`app/chamados/services/fio.py:704` até `:738`).
   - Detalhe aceita transição sem conteúdo no servidor (`app/chamados/services/acoes_chamado.py:313`), fio exige conteúdo para entrar no ramo (`app/chamados/services/fio.py:705`); ambos os formulários HTML exigem texto (`app/templates/chamados/detalhe_local.html:387`, `app/templates/chamados/fio_colaborador.html:261`). Detalhe oferece “Reabrir/aberto”; fio oferece apenas em análise/resolvido/fechado (`app/templates/chamados/detalhe_local.html:398`, `app/templates/chamados/fio_colaborador.html:267`).
   - **Leitor:** histórico da própria tela e auditoria (`app/chamados/views.py:372`, `app/templates/chamados/detalhe_local.html:135`); estado tem guardas contra fechar disputa/perguntas vivas e resolver furo ainda irregular (`app/chamados/models.py:142`, `:159`, `:175`).
   - **Dinheiro/dia:** diferença de trilha/opções, sem escritor financeiro direto nesses dois ramos (`app/chamados/services/fio.py:704`, `app/chamados/services/acoes_chamado.py:283`); efeito financeiro efetivo [nao verificado].

3. **Abrir chamado/mensagem DP para colaborador — formulário completo × mensagem rápida.**
   - Completo `/chamados/abrir/`, `app/chamados/views.py:147`, `chamados/form_abertura.html` (`:212`), ramo admin `:77` → `abrir_chamado_para_colaborador`, `app/chamados/services/abertura.py:122`.
   - Rápido `/chamados/colaborador/<cid>/mensagem-rapida/`, rota `app/chamados/urls.py:40`, view `app/chamados/views.py:961`, disparador `app/templates/colaboradores/partials/painel_colaborador.html:188` → `ChamadoColaborador.abrir` em `app/chamados/views.py:973`, depois nota `:986`.
   - **Diferença:** completo cria chamado com assunto/descrição/prazo/tipo/dia e autor no contexto, faz deduplicação por mesma entrada/autor, grava log e toca dois drivers (`app/chamados/services/abertura.py:145`, `:154`, `:165`, `:172`, `:188`, `:194`, `:202`). Rápido reutiliza qualquer vivo `mensagem_dp` da pessoa, assunto fixo, conteúdo como nota, push `push_colaborador`, sem log explícito nessa view (`app/chamados/views.py:973` até `:1003`).
   - **Leitor:** lista de mensagens renderizada em `app/chamados/views.py:997`, template `colaboradores/partials/msgs_lista.html` (`:1000`); fio lê followups (`app/chamados/views.py:372`).
   - **Dinheiro/dia:** não diretamente para `mensagem_dp`; ramos escrevem chamado/nota (`app/chamados/views.py:973`, `app/chamados/services/abertura.py:172`).
   - **Observação de alcançabilidade:** rota usa argumento `colaborador_id`, mas função recebe `cid` (`app/chamados/urls.py:40`, `app/chamados/views.py:961`); execução bem-sucedida desse endpoint [nao verificado].

4. **Cobrar colaborador — central/modal individual × massa × IA.**
   - Individual `/chamados/cobrar/<cid>/`, `app/chamados/views_cobrar.py:24`, botões lista e modal em `app/templates/chamados/partials/_lista_chamados.html:84`, `app/templates/chamados/partials/modal_fio.html:345`.
   - Massa `/chamados/cobrar/massa/`, `app/chamados/views_cobrar.py:61`, `app/templates/chamados/painel_gestao.html:59`, `:65`.
   - Ambos enviam FCM e usam nota, mas o massa filtra somente a fila cobrar, departamento e `muda_a_folha`, um por pessoa (`app/chamados/views_cobrar.py:76`, `:78`, `:91`, `:95`, `:97`); individual não testa VIVOS/fila/dinheiro (`:29` até `:56`). Ambos limitam por nota `[COBRANCA]` no dia (`:36`, `:117`), mas tipos de nota são `cobranca_push` × `cobranca_push_em_massa` (`:55`, `:132`).
   - IA informado pelo pai → `cobrar_colaborador`, `app/chamados/services/cobranca.py:140`: verifica perguntas pendentes e prazo existente (`:155`, `:159`), grava `prazo_sla` nos pais antes do toque (`:176`, `:178`), nota e log (`:181`, `:188`). Individual/massa não renovam prazo (`app/chamados/views_cobrar.py:24`, `:61`).
   - **Leitor:** `linhas_para_a_pauta` em `app/chamados/services/cobranca.py:251`; comentário do prazo liga à pauta após 48h em `:173`. Dinheiro/dia direto não; muda prazo/escalonamento e seleção de quem cobrar (`:170`, `:178`).

5. **Criar regularização externa — colaboradores × painel chamados.**
   - Colaboradores `app/colaboradores/views.py:2948`, template `colaboradores/partials/_form_regularizar.html` (`:2987`) → `criar_regularizacao_externa` (`:2983`) em `app/chamados/services/regularizacao_ext.py:230`.
   - Chamados `/chamados/regularizacao/colab/<id>/criar/`, `app/chamados/views_regularizacao.py:42`, template `chamados/regularizacao_painel.html` (`:34`) → `abrir_lote` (`:54`) em `app/chamados/services/regularizacao_ext.py:309` → converge em `criar_regularizacao_externa` (`:318`); itens por outro POST `app/chamados/views_regularizacao.py:74` → `adicionar_item`, `app/chamados/services/regularizacao_ext.py:322`.
   - **Diferença:** colaboradores exige categoria e itens, não exige motivo preenchido (`app/colaboradores/views.py:2961`, `:2965`, `:2981`); chamados exige motivo mas deixa categoria vazia (`app/chamados/views_regularizacao.py:48`, `:54`). Colaboradores cria itens diretamente (`app/chamados/services/regularizacao_ext.py:240`); chamados deduplica item por get_or_create (`:325`), oferece corrigir/remover/intervalo e veta E==S (`app/chamados/views_regularizacao.py:102`, `:120`, `:124`).
   - **Leitor:** métrica por `motivo_categoria` (`app/chamados/services/regularizacao_ext.py:277`, `:284`); materializador lê ação/intervalo (`:123`, `:167`).
   - **Dinheiro/dia:** sim após confirmação do colaborador, que é obrigatória (`app/chamados/services/regularizacao_ext.py:51`); adicionar gera batidas via `registrar_batida` (`:77`), corrigir altera batida existente (`:153`), remover usa `delete` (`:136`). Não tratar criação do lote como batida já gravada.

6. **Criar calendário de folgas — editor × wizard.**
   - `/escala/folgas/criar/`, `app/escala/views.py:316`, template `app/templates/escala/folga_calendario_editar.html:9` → `salvar_calendario_de_folga` em `app/escala/folgas.py:548`, chamada view `:356`.
   - `/escala/tipos/wizard/calendario/criar/`, `app/escala/views_wizard.py:140`, disparador `app/templates/escala/wizard_tipo_escala.html:160` → `criar_calendario_de_folga`, `app/escala/folgas.py:577`, chamadas `app/escala/views_wizard.py:171`, `:192`.
   - **Diferença:** escritores separados; editor ignora entradas inválidas (`app/escala/views.py:328`, `:335`, `:345`), wizard recusa com 400 (`app/escala/views_wizard.py:165`, `:167`, `:187`). Editor permite grade diferente por tamanho de mês (`app/escala/views.py:340`); wizard replica uma lista aos tamanhos 31/30/28 com clamp (`app/escala/folgas.py:590`).
   - **Leitor:** `datas_que_o_calendario_manda` em `app/escala/folgas.py:43`, foto `:150`, re-lavra `:203`.
   - **Dinheiro/dia:** sim quando aplicado: calendário decide datas de folga, foto re-lavra células (`app/escala/folgas.py:158`, `:203`, `:272`). Valor afetado em produção [nao verificado].

7. **Corroborar cadastro errado — chamado × cadastro versus realidade.**
   - Chamado `/chamados/local/<id>/`, view `app/chamados/views.py:438`, disparador modal `app/templates/chamados/partials/modal_fio.html:559` → `corroborar_regua`, `app/escala/regua_defesa.py:647`, com chamado (`app/chamados/views.py:443`).
   - Lista `/escala/cadastro-x-realidade/regua/<ec>/corroborar/`, view `app/escala/views.py:456`, formulário `app/templates/escala/cadastro_x_realidade.html:28` → mesmo escritor (`app/escala/views.py:459`).
   - **Diferença:** chamado exige RBAC contextual (`app/chamados/views.py:420`) e busca vínculo ativo por tipo do contexto (`:441`); lista usa EC por PK e apenas `_is_gestor` (`app/escala/views.py:447`, `:449`). Só chamado recebe nota de confirmação no fio (`app/escala/regua_defesa.py:670`); ambos gravam régua/trilha pela mesma porta (`:662`).
   - **Leitor:** veto de cobrança `app/escala/regua_defesa.py:641`, rótulo de espelho `:620`; nota no fio `app/chamados/views.py:372`.
   - **Dinheiro/dia:** diferença provada é escopo/permissão/nota; mesmo EC recebe mesmos campos (`app/escala/regua_defesa.py:666`). Dano financeiro por diferença [nao verificado].

8. **Realocar régua — chamado × lista.**
   - Chamado ramo `app/chamados/views.py:449`, formulário `app/templates/chamados/partials/modal_fio.html:568` → `realocar_para_destino`, `app/escala/regua_defesa.py:326`.
   - Lista rota `app/escala/urls.py:17`, view `app/escala/views.py:463`, formulário `app/templates/escala/cadastro_x_realidade.html:28` → `realocar_vinculo_para_destino`, `app/escala/regua_defesa.py:291`.
   - **Converge** nesse núcleo (`app/escala/regua_defesa.py:329`), depois `julgar_vinculo`/`executar_vinculo` (`:316`, `:321`).
   - **Diferença:** destino do chamado vem do contexto armazenado (`:328`); lista usa `ultimo_lavrado()` (`app/escala/views.py:469`). Só chamado anexa ocorrência de realocação (`app/escala/regua_defesa.py:330`). Permissões diferentes iguais ao grupo anterior (`app/chamados/views.py:420`, `app/escala/views.py:447`).
   - **Dinheiro/dia:** sim potencial quando os destinos diferem: troca vínculo a partir de hoje pelo escritor que reescreve células (`app/escala/regua_defesa.py:293`, `:312`, `:321`); destino divergente real [nao verificado].

9. **Efeito transversal fio × modal, mesmo comando.**
   - Fio → `processar_post_fio` (`app/chamados/views.py:667`, `:674`).
   - Modal antes do mesmo escritor marca notas do colaborador como lidas pelo DP (`app/chamados/views.py:877`, `:880`, `:951`, `:954`) → escritor `marcar_lidas`, `app/chamados/services/nota.py:126`, update `:132`.
   - **Leitor:** não lidos da central `app/chamados/services/painel.py:362`, `:366`, badge `app/templates/chamados/partials/_lista_chamados.html:48`.
   - **Dinheiro/dia:** não, grava leitura de mensagens (`app/chamados/services/nota.py:59`, `:132`).

### Convergências reais para o fim do censo

- **Recusar pergunta:** detalhe `app/chamados/views.py:505` → `acao_recusar_pergunta` (`:518`), fio/modal `app/chamados/services/fio.py:621`, `:634` → mesma casca `app/chamados/services/acoes_disputa.py:65` → escritor `recusar_pergunta`, `app/chamados/services/validacao.py:494`. Disparadores: `app/templates/chamados/partials/_pergunta_card.html:112`, `app/templates/chamados/partials/modal_fio.html:287`. Vale ressalva de leitura automática modal acima.
- **Corrigir resposta:** detalhe `app/chamados/views.py:523`, `:536`; fio/modal `app/chamados/services/fio.py:635` → `acao_corrigir_resposta`, `app/chamados/services/acoes_disputa.py:298`, que depois valida pelo canônico (`:375`). Disparador modal `app/templates/chamados/partials/modal_fio.html:298`; disparador detalhe [nao verificado].
- **Autorizar geofence; autorizar com advertência; recusar:** detalhe `app/chamados/views.py:540`, `:543`, modal/fio `app/chamados/services/fio.py:529`, `:541`, `:542` → mesmos `acao_autorizar_geofence` (`app/chamados/services/acoes_chamado.py:165`) e `acao_recusar_geofence` (`:243`), este → `retratar_batida` (`:269`). Disparadores detalhe `app/templates/chamados/detalhe_local.html:285`, `:286`, `:287`; modal `app/templates/chamados/partials/modal_fio.html:501`, `:512`, `:523`. Recusa muda dia/apuração (`app/chamados/services/acoes_chamado.py:269`).
- **Regularizar disputa:** detalhe `app/chamados/views.py:467` → `acao_regularizar_disputa`, `app/chamados/services/acoes_disputa.py:118`; modal/fio mantém orquestração copiada `app/chamados/services/fio.py:642`. Ambas convergem em `materializar_perguntas_validadas_da_disputa`, chamadas `app/chamados/services/acoes_disputa.py:139` e `app/chamados/services/fio.py:666`; mesmos gates resolução e perguntas vivas sem resposta (`acoes_disputa.py:123`, `:130`; `fio.py:643`, `:656`). Disparadores `app/templates/chamados/detalhe_local.html:317`, `app/templates/chamados/partials/modal_fio.html:350`. Diferença observada: exceção de push é `pass` na casca (`acoes_disputa.py:152`, `:158`) e logger no fio (`fio.py:679`, `:686`), sem divergência de estado demonstrada.
- **Reabrir questionário:** detalhe e modal postam para o mesmo endpoint `reabrir_disputa_supervisao`, `app/chamados/views.py:1372`; disparadores `app/templates/chamados/detalhe_local.html:9`, `app/templates/chamados/partials/modal_fio.html:363`; escritor de reabertura `app/chamados/services/disputa_emissao.py:1362`, chamado via alias em `app/chamados/views.py:1435`.
- **Pedir confirmação de âncora:** detalhe `app/chamados/views.py:455` → `nucleo_pedir_confirmacao_ancora`, `app/chamados/services/acoes_disputa.py:178`; fio/modal `app/chamados/services/fio.py:517` → casca `acao_pedir_confirmacao_ancora`, `app/chamados/services/acoes_disputa.py:197`. Disparador modal `app/templates/chamados/partials/modal_fio.html:639`; disparador detalhe [nao verificado].

### Candidatos que não devem virar “botão atual” sem ressalva

- `arquivar_chamado` e `arquivar_lote` convergem no escritor `arquivo.silenciar` (`app/chamados/services/arquivo.py:141`), mas individual adiciona followup (`app/chamados/views_arquivar.py:44`) e lote não (`:140` até `:148`), com filtro idade/VIVOS/silêncio nulo no lote (`:105`, `:121`, `:135`). Nenhum disparador para arquivar individual/lote encontrado nos templates/static; tela que dispara [nao verificado]. Desarquivar tem botão comprovado (`app/templates/chamados/partials/_lista_chamados.html:85`).
- `reabrir_disputa` em `processar_post_fio` chama casca antiga que não recobra perguntas já pendentes (`app/chamados/services/acoes_disputa.py:95` até `:101`), enquanto endpoint atual recobra vivas (`app/chamados/views.py:1421`, `:1425`). Nenhum formulário atual com valor exato `reabrir_disputa` encontrado; disparador de tela [nao verificado].
- Toggle e aplicar lote do Plano têm portas diferentes e lote recusa competência passada (`app/escala/folgas.py:385`), toggle não tem esse filtro (`app/escala/views_plano_folgas.py:103` até `:137`). São endpoints da mesma tela; não contar como duas telas isoladamente (`app/escala/urls.py:8`, `:9`, `:10`). IA declarar folga é segunda tela: converge em `lancar_folga`, mas fecha também fio do dia (`app/ponto/services/declarar_dia.py:189`, `:204`), ao contrário do Plano (`app/escala/views_plano_folgas.py:131`).
- Wizard criar/editar tipo são duas rotas para a mesma view e template, usando `salvar_tipo` (`app/escala/urls.py:24`, `:25`, `app/escala/views_wizard.py:108`, `app/templates/escala/wizard_tipo_escala.html:15`); separar criação de edição de objeto evita inventar a mesma ação.

## 06/11 20:48 · sessao b826cfa5d075 · - · - · origem=Ronald (tmux)

Censo de `colaboradores`: **33 views POST administrativas**, mais **1 DELETE** (`agenda_bloco_deletar`, fora da contagem POST); `solicitar_beneficio` é autoatendimento e foi excluída; `views_fase.py` só renderiza. Referências: `app/colaboradores/urls.py:9`, `app/colaboradores/urls.py:89`, `app/colaboradores/views.py:1965`, `app/colaboradores/views.py:2261`, `app/colaboradores/views.py:2315`, `app/colaboradores/views.py:2340`, `app/colaboradores/views_fase.py:36`, `app/colaboradores/views_fase.py:118`.

**Achados divergentes comprovados**

1. **Mudar posto — grupo de três caminhos.**

   - `/colaboradores/<cid>/vincular/posto/` → `vincular_posto`, `app/colaboradores/views.py:311`; formulário `app/templates/colaboradores/partials/painel_vinculo.html:112`; escreve somente `posto` em `app/colaboradores/views.py:325` → `salvar_inteiro`, `app/colaboradores/services/colaborador.py:73`; histórico por `registrar_mudanca`, `app/colaboradores/views.py:329`.
   - `/colaboradores/postos/<pid>/colabs/add/` → `posto_add_colab`, `app/colaboradores/views.py:631`; acionador `app/templates/colaboradores/postos.html:97`, drawer `app/templates/colaboradores/partials/painel_posto.html:45`; escreve somente `posto` por `gravar`, `app/colaboradores/views.py:645` → `app/colaboradores/services/colaborador.py:55`; histórico de evento, `app/colaboradores/views.py:647`.
   - `/colaboradores/<cid>/transferir/` → `transferir_posto`, `app/colaboradores/views.py:1366`; formulário `app/templates/colaboradores/editar.html:215`; escreve **posto e empresa**, `app/colaboradores/views.py:1387`; usa `gate_disputa`, `app/colaboradores/views.py:1365`; demais caminhos não possuem esse decorator, `app/colaboradores/views.py:310`, `app/colaboradores/views.py:630`.
   - Convergem no módulo de escrita do colaborador, mas não no comando nem nos campos. `registrar_mudanca` cria campos `tipo/de/para`; `registrar_evento_de_vinculo` cria evento sem esses campos: `app/colaboradores/services/historico_vinculo.py:7`, `app/colaboradores/services/historico_vinculo.py:20`.
   - **Dinheiro/dia: sim, potencial por janela e empresa de exportação:** `classificar_export` filtra empresa do colaborador, `app/folha/export.py:778`; janela usa `colab.empresa`, `app/folha/export.py:337`. Ocorrência concreta em produção [nao verificado].

2. **Desativar/reativar posto — editar cadastro versus botão.**

   - `/colaboradores/postos/<pid>/editar/` → `form_posto`, `app/colaboradores/views.py:714`; checkbox `app/templates/colaboradores/posto_form.html:119`; passa `ativo` em `app/colaboradores/views.py:846` → `atualizar_posto`, `app/colaboradores/services/estrutura.py:64`.
   - `/colaboradores/postos/<pid>/toggle/` → `toggle_posto`, `app/colaboradores/views.py:876`; botão `app/templates/colaboradores/partials/painel_posto.html:67` → `gravar_posto_ativo`, `app/colaboradores/services/estrutura.py:73`.
   - Só o botão veta desativação com colaboradores ativos, `app/colaboradores/views.py:883`. O escritor de edição atribui dados e salva sem esse veto, `app/colaboradores/services/estrutura.py:66`.
   - Leitor: listas de postos disponíveis filtram `ativo=True`, `app/colaboradores/views.py:269`, `app/colaboradores/views.py:1242`. **Dinheiro/dia: não demonstrado diretamente;** esses escritores alteram cadastro do posto, `app/colaboradores/services/estrutura.py:64`, `app/colaboradores/services/estrutura.py:73`; efeito financeiro [nao verificado].

3. **Lançar benefício — individual versus lote; convergência de escritor com validações distintas.**

   - `/colaboradores/beneficios/novo/` → `lancar_beneficio`, `app/colaboradores/views.py:1767`, template `app/templates/colaboradores/beneficio_form.html:12`.
   - `/colaboradores/beneficios/lote/` → `lancar_beneficio_lote`, `app/colaboradores/views.py:1818`, template `app/templates/colaboradores/beneficio_lote.html:17`.
   - Ambos → `criar_beneficio`, `app/colaboradores/services/beneficios.py:9`. Só lote ignora benefício ativo já existente por pessoa/tipo/competência, `app/colaboradores/views.py:1839`; só individual exige campos no servidor, `app/colaboradores/views.py:1779`; competência individual usa `int`, `app/colaboradores/views.py:1798`, lote usa sanitização, `app/colaboradores/views.py:1825`.
   - O decorator individual busca `colaborador_id`, `app/colaboradores/views.py:1766`, mas template e view usam `colaborador`, `app/templates/colaboradores/beneficio_form.html:17`, `app/colaboradores/views.py:1772`; portanto o gate recebe `None` no formulário normal e passa, por `app/colaboradores/mixins.py:305`.
   - Leitores somam valores ativos, `app/colaboradores/views.py:1701`, e exportam cada lançamento, `app/colaboradores/views.py:1897`. **Dinheiro: sim no cadastro/exportação de benefícios; integração desse CSV na folha [nao verificado].**
   - Terceiro produtor do mesmo estado: aprovação `novo_vt`/`novo_va` cria benefício **R$0**, competência atual, descrição fixa e conclui solicitação/chamado, `app/colaboradores/views.py:2066`, `app/colaboradores/views.py:2072`, `app/colaboradores/views.py:2086`; tela `app/templates/colaboradores/painel_solicitacao_beneficio.html:15`.

4. **Regularização externa — divergente, também informado pelo agente de chamados.**

   - Drawer `/colaboradores/colab/<cid>/regularizar/` → `colab_regularizar`, `app/colaboradores/views.py:2948`; template `app/templates/colaboradores/partials/_form_regularizar.html:2`; exige categoria, permite motivo vazio, exige itens, `app/colaboradores/views.py:2961`, `app/colaboradores/views.py:2965`, `app/colaboradores/views.py:2981`; chama `criar_regularizacao_externa`, `app/colaboradores/views.py:2983`.
   - Chamados → `regularizacao_criar`, `app/chamados/views_regularizacao.py:42`; exige motivo, cria lote sem itens/categoria via `abrir_lote`, `app/chamados/views_regularizacao.py:48`, `app/chamados/views_regularizacao.py:54`.
   - Convergem em `app/chamados/services/regularizacao_ext.py:230`, mas drawer cria itens simples, `app/chamados/services/regularizacao_ext.py:240`, enquanto editor usa outra porta e validações; agente de chamados possui os detalhes.

5. **Redefinir senha — ficha administrativa versus cadastro do colaborador.**

   - `/configuracoes/usuarios-ad/ficha/<uid>/acao/` → `ficha_usuario_acao`, `app/core/views_usuarios.py:324`, template `app/templates/core/config/_ficha_usuario.html:25` → `resetar_senha_de_sistema`, `app/core/credenciais.py:185`: grava senha e `last_login=None`, `app/core/credenciais.py:187`.
   - Colaborador reset CPF → `app/colaboradores/views.py:1474` → `resetar_para_cpf`, `app/core/credenciais.py:47`; senha digitada → `app/colaboradores/views.py:1556` → `definir_senha_temporaria`, `app/core/credenciais.py:67`. Ambos marcam `deve_trocar_senha=True`, `app/core/credenciais.py:57`, `app/core/credenciais.py:73`.
   - **Leitor decisivo:** `precisa_trocar_senha` ignora `last_login` quando existe colaborador e lê sua flag, `app/colaboradores/mixins.py:282`. Inferência: staff com colaborador e flag falsa não ganha obrigação de troca pelo reset da ficha de sistema; a ficha permite qualquer `User(is_staff=True)`, `app/core/views_usuarios.py:331`, e perfis administrativos de colaborador tornam o usuário staff, `app/core/credenciais.py:132`. Usuários concretos nessa interseção [nao verificado].
   - Todos auditados pelo mesmo `_trilha`, `app/core/credenciais.py:40`. **Dinheiro/dia: não diretamente**, escritas são senha/flags, `app/core/credenciais.py:54`, `app/core/credenciais.py:187`.

6. **Bloquear/desbloquear acesso — ficha administrativa versus gestão de colaboradores.**

   - Ficha sistema → `app/core/views_usuarios.py:347` → `alternar_ativo_de_sistema`, `app/core/credenciais.py:196`.
   - Gestão colaborador → `toggle_acesso`, `app/colaboradores/views.py:1583`; template `app/templates/colaboradores/gestao_usuarios.html:67` → `alternar_acesso`, `app/core/credenciais.py:84`.
   - Ambos gravam `User.is_active` e auditam, `app/core/credenciais.py:89`, `app/core/credenciais.py:200`; só sistema veta autodesativação, `app/core/views_usuarios.py:348`.
   - Desligamento e reativação são comandos mais amplos que também escrevem acesso absoluto por `definir_acesso`, `app/colaboradores/services/desligamento.py:111`, `app/colaboradores/services/desligamento.py:216`; desligamento altera situação, fecha vínculos/disputas/chamados e abre auditoria, `app/colaboradores/services/desligamento.py:104`, `app/colaboradores/services/desligamento.py:122`, `app/colaboradores/services/desligamento.py:139`, `app/colaboradores/services/desligamento.py:154`, `app/colaboradores/services/desligamento.py:163`. Não equiparar desligar a bloquear acesso.

**Convergentes comprovados para o fim do censo**

- **Reset CPF em duas telas:** `app/templates/colaboradores/editar.html:230` e `app/templates/colaboradores/gestao_usuarios.html:60` → mesma rota `app/colaboradores/urls.py:58` → mesma view `app/colaboradores/views.py:1474` → `app/core/credenciais.py:47`.
- **Reativar colaborador em duas telas:** `app/templates/colaboradores/editar.html:287` e `app/templates/colaboradores/partials/painel_colaborador.html:162` → mesma rota `app/colaboradores/urls.py:53` → `app/colaboradores/views.py:1408` → `app/colaboradores/services/desligamento.py:203`.
- **Trocar escala via vínculo e IA:** vínculo → `app/colaboradores/views.py:411`, `app/colaboradores/views.py:423`; IA → `app/colaboradores/services/troca_pelo_chat.py:64`; ambos julgam em `app/colaboradores/services/vinculo.py:50` e executam em `app/colaboradores/services/vinculo.py:167`. IA chama novamente juiz antes de gravar, `app/colaboradores/services/troca_pelo_chat.py:72`.

**Candidato com segundo acionador não localizado**

- Calendário individual → `app/colaboradores/views.py:239`, fotos `app/colaboradores/views.py:245`, histórico `app/colaboradores/views.py:241`; lote por tipo → `app/escala/views.py:409`, fotos `app/escala/views.py:411`, relavra adicional `app/escala/views.py:416`. **Ambos já relavram dentro de `gerar_foto_folgas`**, `app/escala/folgas.py:203`; não declarar ausência de relavra individual. Rota do lote existe, `app/escala/urls.py:37`; template/acionador do lote [nao verificado]. A diferença comprovada é histórico individual, atomicidade externa do lote e relavra adicional do lote, `app/colaboradores/views.py:241`, `app/escala/views.py:407`, `app/escala/views.py:416`.

## 06/11 20:48 · sessao 9b3bd19952a6 · - · - · origem=Ronald (tmux)

Recorte **ponto/férias/folha**: **52 views admin com POST de escrita persistente**: ponto33, férias14, folha5. Excluídos `previa_he_periodo` (rollback obrigatório), fluxos exclusivos do colaborador e `importar_aej` (410). Referências: `app/ponto/urls.py:19`, `app/ponto/views.py:434`, `app/ponto/services/autorizacao_he_periodo.py:320`, `app/ponto/views.py:57`, `app/ferias/urls.py:9`, `app/folha/urls.py:8`. Minha classificação operacional dá **42 ações**, agrupando individual/lote; trata “resolver dia” como uma ação composta, portanto ajustar se o censo global separar seus vereditos. `[nao verificado]`

**Achados divergentes principais**

1. **Aprovar justificativa**  
   A: `/ponto/justificativa/<jid>/aprovar/` → `app/ponto/views.py:1911` → escritor `aprovar_justificativa_dp`, `app/ponto/services/justificativa.py:117`; form `app/templates/ponto/partials/painel_justificativa.html:51`.  
   B: `/ponto/justificativa/aprovar-lote/` → `app/ponto/views.py:1763` → escrita direta `j.status/save`, `app/ponto/views.py:1796`; form montado em `app/templates/ponto/fila_justificativas.html:176`. Rotas: `app/ponto/urls.py:40`.  
   Difere: individual recusa saída ≤ entrada (`services/justificativa.py:137`), só cria saída se tem entrada (`:137`), sai sem aprovar se já existe batida gerada (`:169`); lote cria saída informada sem exigir `batida_entrada` (`views.py:1808`). Individual encerra bloqueio de entrada (`services/justificativa.py:203`), deixa `LogAuditoria` (`:215`), follow-up (`:219`) e notificações (`:225`); lote encerra somente origem explicitamente (`views.py:1837`) e termina em `:1856`, sem esses passos.  
   Leitor financeiro: `Justificativa(status='aprovada')` compõe `datas_justificadas` no fechamento, `app/ponto/services/fechamento.py:651`. **Muda dinheiro/dia: sim, potencial**, pois aceita/aprova/materializa condições diferentes e a aprovação abona dia no fechamento. Incidência em produção `[nao verificado]`.

2. **Rejeitar justificativa**  
   A `/ponto/justificativa/<jid>/rejeitar/` → view `app/ponto/views.py:1923` → `rejeitar_justificativa_dp`, `app/ponto/services/justificativa.py:247`; template `app/templates/ponto/partials/painel_justificativa.html:58`.  
   B `/ponto/justificativa/rejeitar-lote/` → view/escritor direto `app/ponto/views.py:1859`, escrita `:1875`; template `app/templates/ponto/fila_justificativas.html:188`; rotas `app/ponto/urls.py:42`.  
   Individual usa atomic + lock (`services/justificativa.py:258`), repete como “já processada” (`:265`), cria chamado `justificativa_rejeitada` (`:276`) e follow-up (`:309`). Lote não cria esse chamado nem follow-up; faz get/save sem lock e push (`views.py:1874`). **Dinheiro/dia: não demonstrado para a mesma rejeição**; chamado/corrida/trilha diferem, consequência monetária concreta `[nao verificado]`.

3. **Aprovar fechamento — grupo de três entradas**  
   A `/ponto/fechamento/aprovar/` → `aprovar_fechamento`, `app/ponto/views.py:1257` → `aprovar_lote`, `app/ponto/services/fechamento.py:1307`; template `app/templates/ponto/fechamento.html:209`.  
   B `/ponto/fechamento/aprovar-colaborador/` → `aprovar_colaborador`, `app/ponto/views.py:1461` → `aprovar`, `app/ponto/services/fechamento.py:1319`; template `app/templates/ponto/partials/painel_fechamento.html:86`.  
   C `/folha/exportar/gerar/` → `gerar_dominio_txt`, `app/folha/views.py:102` → mesmo `aprovar_lote` (`:252`); template `app/templates/folha/exportar.html` (linha do form não coletada `[nao verificado]`). Rotas `app/ponto/urls.py:32`, `app/folha/urls.py:14`.  
   A classifica **objetos gravados** (`ponto/views.py:1290`); B classifica `fechamento_lido` (`:1480`, `:1506`); C recalcula em somente leitura (`folha/views.py:207`) e classifica esse resultado (`:212`). A pode dispensar `conta_vazia` só quando selecionado (`ponto/views.py:1304`), B sempre é selecionado (`:1509`), C aplica `TRAVAS_FORA_DO_CARIMBO_DO_TXT` (`folha/views.py:243`). A grava observação (`ponto/views.py:1363`) e tranca período quando sem pulados (`:1366`); B não; C não passa observação e registra Log de aprovação por geração (`folha/views.py:254`) e exportação (`:289`). Escritores A/C fazem `update` (`services/fechamento.py:1316`); B faz save integral (`:1324`).  
   Leitor: o próprio TXT usa `_fechs_lido` para as linhas (`folha/views.py:268`); **muda dinheiro/dia: sim, potencial**, pois as portas podem autorizar pessoas segundo fotos diferentes e C produz rubricas no arquivo. Valor/prod `[nao verificado]`.

4. **Reabrir fechamento**  
   A `/ponto/fechamento/reabrir-colaborador/` → `app/ponto/views.py:1434` → `reabrir`, `app/ponto/services/fechamento.py:1333`; B `/ponto/fechamento/reavaliar-vazios/` → `app/ponto/views.py:1383` → `reabrir_lote`, `app/ponto/services/fechamento.py:1328`; rotas `app/ponto/urls.py:34`.  
   A aceita qualquer aprovado (`views.py:1447`); B só aprovados cujo `classificar_export` gravado retorna `conta_vazia` (`:1409`). Mesmos três campos, escritores save vs update (`services/fechamento.py:1330`, `:1338`), logs distintos (`views.py:1424`, `:1453`). **Dinheiro/dia: não diretamente**; libera novo cálculo, que pode mudar valor `[nao verificado]`.

5. **Cancelar férias — DP versus admin versus massa/concluída**  
   A `/ferias/agendamento/<ag_id>/cancelar/` → `app/ferias/views.py:321` → `cancelar_agendamento_dp`, `app/ferias/services.py:330`; templates `app/templates/ferias/saldo.html:71`, `app/templates/ferias/_secao_colab.html:160`.  
   B `/ferias/agendamento/<ag_id>/cancelar-admin/` → `views.py:341` → `executar_cancelamento_admin`, `services.py:341`; template `_secao_colab.html:162`.  
   C `/ferias/cancelar-admin-massa/` → `views.py:519` → mesmo escritor (`:559`); template `ferias/cancelar_admin_massa.html`, render em `views.py:565`.  
   D `/ferias/agendamento/<ag_id>/corrigir-concluida/` → `views.py:378` → mesmo escritor (`:407`), exige justificativa ≥5 (`:399`). Rotas `app/ferias/urls.py:9`, `:18`.  
   A somente barra `concluido` (`views.py:325`), **não barra já cancelado**; escritor subtrai novamente `dias_concedidos` sem atomic (`services.py:333`). B/C barram via `pode_cancelar_admin`, só agendado/em_gozo (`models.py:335`), e usam atomic (`services.py:352`), rejeitam ausências explicitamente (`:356`), carimbam `obs` (`:360`). B deixa `LogAuditoria` (`views.py:368`), C não chama esse log entre execução e resposta (`:551–565`). D aceita concluído e registra justificativa no Log (`:410`).  
   **Não alegar ausência órfã só no DP:** ambos salvam AgendamentoFerias, cujo save sincroniza Ausencia (`app/ferias/models.py:434`, `:379`).  
   Leitor do saldo: `saldo_para_remarcar` usado por edição (`views.py:661`). **Muda dinheiro/dia: sim**, saldo pode ser devolvido novamente pelo POST de A já cancelado; incidência `[nao verificado]`.

6. **Agendar férias pela solicitação versus agendar individual/lote/chat**  
   Individual `/ferias/periodo/<periodo_id>/agendar/` → `app/ferias/views.py:269` → `_executar_agendamento` (`:298`) → `executar_agendamento`, `app/ferias/services.py:306`; template `app/templates/ferias/agendar.html`, render `views.py:312`, e `_secao_colab.html:60`.  
   Lote `/ferias/agendar-lote/` → view `:569` → mesmo escritor (`:617`), template render `:626`.  
   Solicitação `/ferias/solicitacoes/<sid>/avaliar/` → `views.py:711` → `aprovar_solicitacao`, `services.py:410`, **outro escritor**, `AgendamentoFerias(...).full_clean/save` (`:434–447`); template `ferias/solicitacoes.html`, render `views.py:705`.  
   Solicitação é marcada aprovada **antes** de tentar agendar (`services.py:416`); erro no agendamento vira aviso (`:448`). Seleciona primeiro PA adquirido (`:424`); individual recebe PA e usa juiz `_validar_agendamento` (`views.py:288`) e escritor com guarda de duplicata (`services.py:316`). Chat `agendar_ferias_porta` (`services.py:128`) **converge em executar_agendamento** (`:157`), mas pode vender dias no mesmo atomic (`:161`).  
   **Dinheiro/dia: sim, potencial**, solicitação aprovada pode não gerar gozo e escolha/validação/escritor diferem; Ausencia materializada sai do model save (`models.py:434`), leitor de gozo `ferias_na_janela`, `services.py:47`. Produção `[nao verificado]`.

**Convergências com diferenças nas cascas**

7. **Aprovar ausência individual/lote** converge em `app/ponto/services/ausencia.py:897` via `_a4_decidir`, `app/ponto/views.py:82`.  
   A `/ponto/ausencias/<aid>/aprovar/` → `views.py:2599`, template `app/templates/ponto/partials/painel_ausencia.html:309`; B `/ponto/ausencias/lote/aprovar/` → `views.py:2270`, template `app/templates/ponto/ausencias.html:308`; rotas `app/ponto/urls.py:54`, `:65`.  
   A propaga irmãos (`views.py:2615`), pode confirmar sem lastro (`:2608`) e gravar horas parciais (`:2630`, `:2644`), usa atomic para todos irmãos (`:2641`) e push (`:2646`). B só ids, gate sem confirmação (`:2284`), sem período, sem push (`:2282–2300`). O escritor só aplica `aprovada_em` se recebido (`services/ausencia.py:916`); A manda, B não. Leitor financeiro/grade: período parcial na autoridade (`services/ausencia.py:935`); **dinheiro/dia: sim, potencial**, cobertura do conjunto e período diferem. Incidência `[nao verificado]`.

8. **Rejeitar ausência individual/lote** converge no mesmo `decidir`.  
   A `/ponto/ausencias/<aid>/rejeitar/` → `views.py:2680`, template painel `:330`; B lote → `views.py:2304`, template `ausencias.html:321`. A propaga irmãos (`views.py:2688`), manda `aprovada_em` (`:2702`), push (`:2704`); B só ids e acrescenta motivo em descrição (`:2319`). **Dinheiro/dia: sim, potencial pelo conjunto de dias rejeitados**, não por escritor duplicado (`services/ausencia.py:897`).

9. **Cancelar ausência individual/lote** converge em `decidir` e ambos deixam Log por registro. A `views.py:2720` → `:2734`; B `views.py:2333` → `:2351`. Templates painel `[nao verificado]` / `ausencias.html:278`; rotas `urls.py:56`, `:75`. Mesmos campos/carimbos/status, muda seleção/envelope/log descritivo (`views.py:2357`, `:2739`). **Dinheiro/dia: não identifiquei divergência para mesmo registro**.

10. **Editar datas férias normal/admin** converge em `_trocar_janela`, `app/ferias/services.py:366`, via wrappers `:398`, `:403`.  
    Normal `/ferias/agendamento/<ag_id>/editar/` → `views.py:633`, form `_secao_colab.html:130`; admin `/editar-datas-admin/` → `views.py:421`, form `:146`; rotas `ferias/urls.py:21`. Normal só `status='agendado'` (`views.py:649`; não verifica `pode_editar` nessa view); admin qualquer não cancelado (`:452`) e motivo ≥5 (`:455`), Log só admin (`:497`). Mesma limpeza janela antiga e Ausencia (`services.py:387`), saldo/data/status (`:378`). **Dinheiro/dia: sim por alcance de status permitido**, para o mesmo agendado com mesmos parâmetros converge.

11. **Autorizar HE dia/período** converge em `decidir_he`, `app/ponto/portas/he.py:138` → `_gravar`, `:44`.  
    Dia `/ponto/gestao-he/decidir/` → `views.py:496`, chamada `:529`; form `app/templates/ponto/gestao_he.html:256`; também calendário `[nao verificado]`. Período `/ponto/gestao-he/periodo/autorizar/` → `views.py:725` → `autorizacao_he_periodo.confirmar`, `:751` → `_ato` `services/autorizacao_he_periodo.py:249` → `autorizar_em_lote` `portas/he.py:480` → mesmo `decidir_he`, `:562`; form `gestao_he_periodo_previa.html:79`.  
    Período só dias acima limite (`views.py:665`), não sobrescreve outra decisão (`portas/he.py:557`), exige DIFF igual (`autorizacao_he_periodo.py:343`) e cria Pauta DP se exportado (`:299`). Dia pode trocar decisão, sem essa pauta explícita (`portas/he.py:174`). Leitor dinheiro: porta relavra pelo evento (`portas/he.py:185`), **dinheiro/dia: sim, potencial pela aceitação/recusa diferente e HE autorizada**.

12. **Recusar HE dia/lote** converge em `decidir_he`; lote `app/ponto/views.py:546` → `recusar_em_lote` (`:614`) → `app/ponto/portas/he.py:237`. Forms `app/templates/ponto/gestao_he.html:68`, `:139`. Escrita/trilha iguais (`portas/he.py:181`); lote permite erros por item (`views.py:619`). **Dinheiro/dia: não por recusa inédita**, recusa é padrão; troca de autorizado é outra condição (`portas/he.py:12`).

13. **Lançar ausência / resolver dia justificado ou faltou / chat** converge em `criar_ausencia`, `app/ponto/services/ausencia.py:130`.  
    A `/ponto/ausencias/lancar/` → `views.py:2368` → `criar_ausencia` (`:2415`); template render `:2460`.  
    B `/ponto/celula/veredito/` → `views.py:3312` → `veredito_celula`, `services/veredito_celula.py:191` → criar (`:315`); forms `app/templates/ponto/partials/_veredito_dia.html:17`, `_veredito_lote_barra.html:2`.  
    C chat → `app/ponto/services/checar_ausencia.py:43` → criar (`:46`).  
    B fecha fio (`veredito_celula.py:325`), lavra célula admin (`:330`) e Log (`:334`); A permite minutos/documento/indeterminado/HE100 (`views.py:2417`) e escreve situação afastado para INSS (`:2433`), C não recebe esses campos e proíbe INSS/férias/falta (`checar_ausencia.py:10`). Criar já deixa trilha própria (`services/ausencia.py:440`), portanto não dizer A/C “sem Log”. **Dinheiro/dia: sim, potencial**, parâmetros e lavra diferentes.

14. **Mesmo endpoint em templates distintos**: férias agendar (`agendar.html` render `views.py:312` / `_secao_colab.html:60`), férias cancelar (`saldo.html:71` / `_secao_colab.html:160`); flip batida fila (`app/templates/ponto/flip_fila.html:16`) / calendário (`app/templates/colaboradores/partials/_calendario_grade.html:121`) → `app/ponto/views.py:3260` → `flip_tipo` (ver chamada `:3267`). Veredito dia/lote → mesmo endpoint `views.py:3312`, mesmo loop `:3339`. Calendário mês inclui grade em `calendario_partial.html:62`; drawer diagnóstico também (`_drawer_diagnostico.html:35`). **Convergências, sem divergência adicional demonstrada para mesmo payload**.

**Lista das 52 views para consolidação**

Ponto33: `batidas_offline_pendentes:434`, `decidir_he:496`, `recusar_he_em_lote:546`, `autorizar_he_periodo:725`, `recalcular_fechamento:1226`, `aprovar_fechamento:1257`, `reavaliar_vazios:1383`, `reabrir_colaborador:1434`, `aprovar_colaborador:1461`, `reabrir_periodo:1520`, `aprovar_lote_justificativas:1763`, `rejeitar_lote_justificativas:1859`, `aprovar_justificativa:1911`, `rejeitar_justificativa:1923`, `anexar_documento_ausencia:2133`, `aprovar_ausencia_lote:2270`, `rejeitar_ausencia_lote:2304`, `cancelar_ausencia_lote:2333`, `lancar_ausencia:2368`, `reverter_rejeicao_ausencia:2569`, `aprovar_ausencia:2599`, `editar_periodo_ausencia:2662`, `rejeitar_ausencia:2680`, `cancelar_ausencia:2720`, `editar_ausencia:2751`, `criar_cobertura:3037`, `materializar_saida_retroativa:3106`, `flip_batida_view:3260`, `retratar_batida_view:3274`, `desfazer_retratacao_view:3286`, `veredito_celula_view:3312` (todas `app/ponto/views.py`); `app/ponto/views_tipos_ausencia.py:57`; `app/ponto/views_motivos_retratacao.py:48`.

Férias14: `criar_periodo_aquisitivo:123`, `agendar_ferias:269`, `cancelar_agendamento:321`, `cancelar_agendamento_admin:341`, `corrigir_concluida_admin:378`, `editar_datas_admin:421`, `cancelar_admin_massa:519`, `agendar_lote:569`, `editar_agendamento:633`, `avaliar_solicitacao:711`, `editar_periodo:777`, `excluir_periodo:808`, `agendamento_enviar:830`, `vender_dias:906`, todas `app/ferias/views.py`.

Folha5: `lista_rubricas:27`, `gerar_dominio_txt:102`, `nova_integracao:466`, `editar_integracao:487`, `excluir_integracao:501`, todas `app/folha/views.py`.

## 06/11 20:47 · sessao dc2d4fc85da7 · DUAS-TELAS-E-PWA-OFFLINE · leitura · origem=Code

**O PWA tem fila offline real em IndexedDB. O “zero atrasadas” informado não prova ausência de fila nem perda:** o envio online e o reenvio da fila carregam campos diferentes. (`app/static/pwa.js:107`; `app/static/pwa.js:147`; `app/templates/ponto/bater_ponto_v2.html:330`)

1. **Sem rede: o que acontece**

   O dashboard padrão aponta para `/ponto/bater-v2/`; essa rota renderiza `bater_ponto_v2.html`, que herda `base.html`, carregador de `pwa.js`. (`app/core/views.py:270`; `app/templates/core/dashboard_colaborador_v2.html:31`; `app/ponto/urls.py:12`; `app/ponto/views.py:356`; `app/templates/ponto/bater_ponto_v2.html:1`; `app/templates/base.html:20`)

   Na v2, com a tela disponível, o clique desabilita o botão e exige **selfie e GPS antes de verificar a rede**. Sem esses requisitos, mostra aviso, reabilita o botão e não grava. Com eles e `navigator.onLine=false`, chama a gravação offline; concluída a gravação, mostra “salva no celular” e esconde o botão. Se a gravação falhar, mostra “Erro ao registrar” e reabilita o botão. (`app/templates/ponto/bater_ponto_v2.html:285`; `app/templates/ponto/bater_ponto_v2.html:320`; `app/templates/ponto/bater_ponto_v2.html:367`)

   A gravação usa **IndexedDB**, banco `juliani-offline`, versão 2, store `batidas_pendentes`, com `id` autoincremental. Guarda tipo, horário corrigido pelo gap, `timestamp_dispositivo`, gap, GPS e foto; só confirma depois de completar a transação. A fila de batidas não está em localStorage nem no Cache Storage. ([app/static/pwa.js:48](/home/ronald/codex_ws/8050df3f2bb337b246024e3f34fda870edd942ee/app/static/pwa.js:48); `app/static/pwa.js:107`)

   O **reenvio é executado pela página**, no evento `online`, na inicialização quando está online ou ao receber `SYNC_BATIDAS` do SW. Exige a meta de colaborador, ordena as pendências e faz POST multipart para `/api/ponto/bater/`. Sucesso com JSON legível remove a pendência; **HTTP 400 também remove**, com aviso apenas no console; outras respostas e exceções conservam o item para outra sincronização. (`app/static/pwa.js:15`; `app/static/pwa.js:126`; `app/static/pwa.js:158`; `app/static/pwa.js:320`)

   O handler Background Sync não envia sozinho: manda mensagem para janelas existentes ou, sem janela, notifica para abrir o app. A gravação da fila não registra a tag `sync-batidas`; portanto, esse handler não comprova sincronização autônoma com o app fechado. (`app/static/sw.js:112`; `app/static/pwa.js:107`; `app/static/pwa.js:323`)

   Há outra rota ativa, `/ponto/bater/`, que normalmente serve `triagem/apto.html`: offline, abre câmera e depois grava na mesma fila; câmera indisponível esconde a confirmação. Não confundir esse caminho com o arquivo `bater_ponto.html`, que não é escolhido pela view atual. (`app/ponto/urls.py:11`; `app/ponto/views.py:225`; `app/ponto/views.py:356`; `app/templates/ponto/triagem/apto.html:236`; `app/templates/ponto/triagem/apto.html:253`; `app/templates/ponto/triagem/apto.html:289`)

   **Abrir a tela sem rede é uma questão separada:** o SW pré-cacheia JS, manifest e ícones; seu ramo de navegação consulta cache, mas não grava HTML nele. Tela disponível desde uma sessão anterior não equivale a abertura offline garantida. (`app/static/sw.js:3`; `app/static/sw.js:45`)

2. **Horários gravados e comparação com APK**

   | Caminho | Instante enviado e gravado |
   |---|---|
   | **PWA v2 online** | O formulário não envia `timestamp_dispositivo`; a API usa `agora` como `timestamp` e deixa o horário do dispositivo nulo. (`app/templates/ponto/bater_ponto_v2.html:102`; `app/templates/ponto/bater_ponto_v2.html:330`; `app/api/hora_do_aparelho.py:135`) |
   | **PWA pela fila** | Envia `timestamp_dispositivo` ISO. O `timestamp` corrigido e o gap guardados localmente **não são enviados** nesse multipart. (`app/static/pwa.js:113`; `app/static/pwa.js:143`) |
   | **PWA pelo POST HTML `/ponto/bater/`** | O formulário não envia horário do dispositivo; o servidor grava `timezone.now()`. (`app/templates/ponto/triagem/apto.html:86`; `app/ponto/services/triagem_batida.py:625`; `app/ponto/services/triagem_batida.py:708`) |
   | **APK na API** | A porta classifica como APK quando recebe `device_id` não vazio e aplica a mesma função de leitura de horário, que aceita epoch em segundos/milissegundos e ISO. Comportamento interno da fila do cliente APK: **[nao verificado]**. (`app/ponto/registro_batida.py:53`; `app/api/hora_do_aparelho.py:72`; `app/api/hora_do_aparelho.py:98`) |

   Na API, horário legível entre **agora e 48 horas atrás** vira `timestamp`; ausente, ilegível, futuro ou anterior à janela faz usar `agora`. Um horário legível descartado continua sendo gravado em `timestamp_dispositivo`. A API entrega ambos à porta única, que cria a `Batida`. ([app/api/hora_do_aparelho.py:124](/home/ronald/codex_ws/8050df3f2bb337b246024e3f34fda870edd942ee/app/api/hora_do_aparelho.py:124); `app/api/views_core.py:680`; `app/ponto/registro_batida.py:157`)

   **Escritores e leitores relevantes:**

   | Campo | Escrita e leitura |
   |---|---|
   | `timestamp` | API escolhe o efetivo; porta grava; geometria e tela leem. (`app/api/views_core.py:529`; `app/ponto/registro_batida.py:157`; `app/ponto/turnos.py:353`; `app/templates/ponto/batidas_offline_pendentes.html:66`) |
   | `timestamp_dispositivo` | Fila PWA produz; API interpreta e passa à porta. Tela de pendentes lê; `descartadas()` compara com `timestamp`, medindo **descarte do horário**, não atraso de chegada. (`app/static/pwa.js:115`; `app/api/views_core.py:692`; `app/templates/ponto/batidas_offline_pendentes.html:71`; `app/api/hora_do_aparelho.py:178`) |
   | `sincronizado_em` | Campo nullable, **não preenchido por essa chamada da API nem acrescentado pela porta**; a tela de pendentes tenta exibi-lo. (`app/ponto/models.py:77`; `app/api/views_core.py:680`; `app/ponto/registro_batida.py:157`; `app/templates/ponto/batidas_offline_pendentes.html:88`) |
   | `criado_em` | Django preenche com `auto_now_add`; o pareador também o lê para identificar eco de flush. (`app/ponto/models.py:79`; `app/ponto/turnos.py:343`) |
   | `canal` / `origem` | API grava origem `app` para ambos; `device_id` decide `apk`/`pwa`. O juiz de selfie lê origem/canal para decidir o prazo. (`app/api/views_core.py:680`; `app/ponto/registro_batida.py:53`; `app/ponto/registro_batida.py:80`) |

   Assim, **atraso superior a uma hora é mensurável para PWA reenviado pela fila**, usando `criado_em - timestamp_dispositivo`; dentro da janela aceita, também `criado_em - timestamp`. Isso usa criação no banco como referência, não um carimbo explícito de chegada HTTP. Para PWA online sem horário do dispositivo, o instante original não fica disponível; `sincronizado_em` não resolve nessa porta. (`app/ponto/models.py:75`; `app/api/hora_do_aparelho.py:141`; `app/api/views_core.py:680`)

   A fórmula e o universo usados nos números informados — inclusive cobertura de tentativas que nunca chegaram — são **[nao verificado]**. Portanto, atribuir o zero a perda, ausência de tentativas offline ou somente limitação da medida é **[nao verificado]**.

3. **Rede cai durante o POST; duplicação**

   Na **v2 online**, rejeição do `fetch` entra no `catch` e tenta guardar na fila. Porém, o horário é gerado **ao enfileirar depois da falha**, não preservado desde o clique. Não há timeout explícito no POST; enquanto o `fetch` não terminar/rejeitar, esse `catch` não roda. Resposta HTTP não satisfatória, inclusive 5xx, mostra erro e reabilita o botão **sem enfileirar**. (`app/templates/ponto/bater_ponto_v2.html:335`; `app/templates/ponto/bater_ponto_v2.html:341`; `app/templates/ponto/bater_ponto_v2.html:349`; `app/static/pwa.js:109`)

   No **reenvio de item já guardado**, falha de rede ou de leitura do JSON conserva o item; outra sincronização pode reenviá-lo. No POST HTML de `/ponto/bater/`, não existe recuperação equivalente: ele faz `form.submit()` sem `catch` que grave a tentativa. (`app/static/pwa.js:153`; `app/static/pwa.js:178`; `app/templates/ponto/triagem/apto.html:210`)

   **Não há chave dedicada de idempotência enviada pelo PWA**, nem envio do `id` local. Há deduplicação servidor por colaborador/tipo em ±5 minutos do horário efetivo, deduplicação adicional na porta e unicidade de `(colaborador, timestamp)`. Isso protege repetições com dados compatíveis; não garante ausência de duplicata quando a resposta se perde e a nova tentativa ganha outro horário, especialmente fora das janelas. (`app/static/pwa.js:143`; `app/api/views_core.py:549`; `app/ponto/registro_batida.py:106`; `app/ponto/models.py:106`)

4. **Service worker e alcance dos testes** [nao verificado]

   O SW **não intercepta o POST de bater**: retorna imediatamente para qualquer método diferente de GET. O reenvio da fila vai para a API, roteada ao core e à porta de registro. ([app/static/sw.js:35](/home/ronald/codex_ws/8050df3f2bb337b246024e3f34fda870edd942ee/app/static/sw.js:35); `Caddyfile:8`; `app/api/urls_core.py:14`; `app/api/views_core.py:680`)

   `test_sw_offline_fallback.py` executa o SW real em Node, com rede/cache simulados e **requisições sempre GET**. Verifica erro de rede sem cache, retorno do cache existente e preferência pela rede. É teste do SW/PWA; não prova POST, criação de cache HTML, IndexedDB ou reenvio de batida. (`app/core/tests/test_sw_offline_fallback.py:25`; `app/core/tests/test_sw_offline_fallback.py:44`; `app/core/tests/test_sw_offline_fallback.py:79`; `app/core/tests/test_sw_offline_fallback.py:89`; `app/core/tests/test_sw_offline_fallback.py:96`)

   `test_batida_offline.py` envia POST direto com Django Client autenticado, horário ISO e **sem `device_id`**: pelo classificador, representa API/canal PWA, não cliente APK. Verifica criação, repetição imediata sem segunda linha, erros de payload e aceitação HTTP de horário antigo. Não executa navegador, fila ou reconexão; o caso antigo só exige HTTP 200, não preservação daquele horário em `timestamp`. Resultado de execução nesta análise: **[nao verificado]**. (`app/ponto/tests/test_batida_offline.py:49`; `app/ponto/tests/test_batida_offline.py:60`; `app/ponto/tests/test_batida_offline.py:68`; `app/ponto/tests/test_batida_offline.py:78`; `app/ponto/tests/test_batida_offline.py:104`; `app/ponto/registro_batida.py:61`)

5. **Veredito: GUARDADA — com tela disponível, requisitos atendidos e transação concluída, a batida offline fica em IndexedDB; existem bloqueios prévios e descarte em HTTP 400, portanto não há garantia universal de conservação.** ([app/static/pwa.js:107](/home/ronald/codex_ws/8050df3f2bb337b246024e3f34fda870edd942ee/app/static/pwa.js:107); `app/templates/ponto/bater_ponto_v2.html:325`; `app/static/pwa.js:166`)

   A LEI local L-030 diz que “batida de chão nunca é barrada”; os bloqueios prévios da v2 e a remoção em 400 precisam ser avaliados contra essa regra nos próprios produtores/fila, mantendo a escrita servidor na porta única. Esses são os sítios de origem do problema, não o contador de atrasos. Enquadramento jurídico externo específico da Portaria 671: **[nao verificado]**. (`app/docs/LEIS.md:78`; `app/templates/ponto/bater_ponto_v2.html:285`; `app/static/pwa.js:166`; `app/ponto/registro_batida.py:94`; `CLAUDE.md:17`)

## 06/11 20:47 · sessao 29b41450d58c · - · - · origem=Ronald (tmux)

Achados v2 e MORDE, por inspeção e testes somente em memória: [nao verificado]

| Achado v2 | Estado v3 |
|---|---|
| Linha compartilhada `]; CAD=700`, chamada/import/monkeypatch após `;` | **Fechado:** esses nós permanecem no dump; casos `linha_par`, `decisao`, `monkey` distinguem as alterações. (`bin/tests/test_lei_akita.sh:198`, `bin/tests/test_lei_akita.sh:209`, `bin/tests/test_lei_akita.sh:382`, `bin/tests/test_lei_akita.sh:393`, `bin/tests/test_lei_akita.sh:406`) |
| `papel`, `estado`, `id`, chave nova/removida | **Fechado sintaticamente** para chaves não declaradas: apenas valores de chaves declaradas são mascarados; estrutura/chaves ficam no dump. Os casos novos cobrem `estado`, `papel`, adição de chave; não há caso de remoção. (`bin/tests/test_lei_akita.sh:205`, `bin/tests/test_lei_akita.sh:209`, `bin/tests/test_lei_akita.sh:379`, `bin/tests/test_lei_akita.sh:383`, `bin/tests/test_lei_akita.sh:402`) |
| `for`, `with`, walrus, `del`, import na unicidade | **Fechado para essas formas:** `Name` não `Load` e aliases são contados. Confirmei cada forma em memória: duas ligações, declaração rejeitada. O mecanismo antigo de Name como constante-string desapareceu; valores `Name` não são mascarados. (`bin/tests/test_lei_akita.sh:153`, `bin/tests/test_lei_akita.sh:155`, `bin/tests/test_lei_akita.sh:164`, `bin/tests/test_lei_akita.sh:206`) |
| Redeclaração no ALVO | **Fechado para as formas contadas; agora há caso discriminante:** checkout de `for_decl` no ALVO antes de julgar outro commit. (`bin/tests/test_lei_akita.sh:175`, `bin/tests/test_lei_akita.sh:213`, `bin/tests/test_lei_akita.sh:417`) |
| Erro Git | **Mudou de forma:** `diff` foi eliminado; `show` continua gerando `PADRAO_ERRO`; qualquer erro de `cat-file` vira `None`, qualquer erro da sonda de pai vira `False`. A docstring agora promete especificamente erro de `git show`, mas continua sem distinguir falha operacional dessas sondas. (`bin/tests/test_lei_akita.sh:137`, `bin/tests/test_lei_akita.sh:138`, `bin/tests/test_lei_akita.sh:143`, `bin/tests/test_lei_akita.sh:216`, `bin/tests/test_lei_akita.sh:222`) |

**Nova lacuna da unicidade:** `ExceptHandler.name` e nomes de captura de `match` não são nós `Name`; o contador não os alcança. Em memória, os dois exemplos abaixo resultaram `_ligacoes(..., 'PAPEL_DO_MODULO') == 1` e `_dado_medido(...) == {'R': frozenset({'numero'})}`. Ocorrem ligações adicionais sem detecção. (`bin/tests/test_lei_akita.sh:152`, `bin/tests/test_lei_akita.sh:160`, `bin/tests/test_lei_akita.sh:175`)

```python
PAPEL_DO_MODULO = 'medida'
DADO_MEDIDO = {'R': ('numero',)}
R = [{'numero': 1}]

# Alternativa A:
try:
    1 / 0
except Exception as PAPEL_DO_MODULO:
    pass

# Alternativa B:
match 1:
    case PAPEL_DO_MODULO:
        pass
```

A alternativa A ainda apaga o nome ao sair do handler; B troca seu valor por `1`. A garantia de “única ligação” não corresponde ao Python executado. Os casos MORDE não incluem essas formas. (`bin/tests/test_lei_akita.sh:148`, `bin/tests/test_lei_akita.sh:153`, `bin/tests/test_lei_akita.sh:378`, `bin/tests/test_lei_akita.sh:408`)

**Há cláusulas sem mordida, mesmo aceitando os três mutantes vermelhos informados pelo usuário.** Extraí funções reais, fixture `PL`, os 21 `casos_m` e os dois casos de ALVO. Executei em memória com Git substituído por snapshots de strings; baseline e cada mutante abaixo tiveram **zero divergências nesses 23 casos**. Isso não foi execução integral do shell nem teste com Git real. (`bin/tests/test_lei_akita.sh:360`, `bin/tests/test_lei_akita.sh:378`, `bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:419`)

| Mutante sobrevivente nos 23 casos | Cláusula sem discriminação |
|---|---|
| Retirar `and not isinstance(v.value, bool)` | Exclusão explícita de booleanos. (`bin/tests/test_lei_akita.sh:207`) |
| Trocar `(str, int, float)` por `(str,)` | Aceitação de valores numéricos: positivos existentes são strings; nenhum caso exige `int` ou `float`. (`bin/tests/test_lei_akita.sh:207`, `bin/tests/test_lei_akita.sh:361`, `bin/tests/test_lei_akita.sh:378`) |
| Trocar `if _de_topo(t, k.value) is None:` por `if False:` | Existência/unicidade do contêiner declarado. O teste de ALVO duplicado liga `PAPEL_DO_MODULO`, não `R`. (`bin/tests/test_lei_akita.sh:188`, `bin/tests/test_lei_akita.sh:384`, `bin/tests/test_lei_akita.sh:417`) |
| Retirar `and None not in v.keys` | Rejeição de `**` na declaração `DADO_MEDIDO`; nenhuma fixture planta declaração com desempacotamento. (`bin/tests/test_lei_akita.sh:181`, `bin/tests/test_lei_akita.sh:360`, `bin/tests/test_lei_akita.sh:400`) |

Os dois casos retroativos não foram executados nessa simulação; alteram número string e `CAD`, portanto não exercitam as quatro cláusulas acima por inspeção. (`bin/tests/test_lei_akita.sh:423`)

Outras lacunas por inspeção: validação de declaração vazia/malformada, tupla de chaves vazia/tipo errado, múltiplos alvos, contêiner inexistente, import/def/global como ligação adicional **no ALVO**, módulo criado/apagado/inválido, commit raiz, erros de `show`/`cat-file`/pai, chave removida e efeitos semânticos de valores declarados. Não há esses casos no bloco de medida. (`bin/tests/test_lei_akita.sh:164`, `bin/tests/test_lei_akita.sh:181`, `bin/tests/test_lei_akita.sh:185`, `bin/tests/test_lei_akita.sh:216`, `bin/tests/test_lei_akita.sh:378`, `bin/tests/test_lei_akita.sh:433`)

O comentário do MORDE ainda diz “por LINHA” e “nem concessão retroativa”, embora a implementação use AST e os casos seguintes exijam concessão retroativa; são resíduos documentais. (`bin/tests/test_lei_akita.sh:350`, `bin/tests/test_lei_akita.sh:413`, `bin/tests/test_lei_akita.sh:420`)

## 06/11 20:46 · sessao c811294be807 · BO-[nome]-0611-V2 · sonda · origem=Code

**Não fecho nenhum item como “resolvido”: faltam evidências da ação na tela, do resultado posterior no DiaPago e da validação operacional. A sonda fornecida termina com `ERROS {}`; as limitações abaixo são de evidência disponível.** [nao verificado]

Nos blocos, a sequência da célula é **trabalha / veredito / origem / status**. Os estados de produção vêm exclusivamente do texto fornecido e recebem `[nao verificado]`.

**02 — col78**

- **ESTADO HOJE:** entre 21/10 e 05/11, os plantões apresentados estão `true / concorde / gerada / ok`, alternados com `false / concorde / gerada / folga`. Em 06/11: célula #131373 `true / null / gerada / aberto`, `pede_decisao=true`, `resolver_dia=false`. Chamado #34349 resolvido via sistema; pergunta #49403 sem resposta/validação, mas carimbada por `completude_turno`. A FICHA não apresenta DiaPago nem identifica uma correção solicitada por dia/hora/ID. [nao verificado]
- **DONO L-099:** **não atribuível com o dado disponível**. Não há uma divergência específica identificada para separar ESTRUTURA, CADASTRO e BATIDA. O juiz exige insumos adicionais, incluindo cadastro×realidade, vínculo e fase; não basta encontrar uma célula `ok`. (`app/ponto/management/commands/e6_oraculo.py:53`, `:98`, `:110`.)
- **PORTA DA TELA:** a porta que resolveria **a queixa concreta** permanece **[nao verificado]**, porque essa queixa não foi identificada no dado corrigido. A pergunta #49403 carimbada não demonstra que uma correção humana foi executada. [nao verificado]
- **OBRA:** nenhuma das quatro obras pode ser atribuída apenas pelo ID corrigido. Não proponho ID novo sem localizar o fato pendente. **Validação operacional pendente:** identificar a correção alegada e conferir seu resultado no espelho e DiaPago. [nao verificado]

**05 — col456**

- **ESTADO HOJE:** plantões apresentados até 04/11: `true / concorde / gerada / ok`; folgas intermediárias: `false / concorde / gerada / folga`. Em 06/11, célula #134856 `true / cobrado / gerada / irregular`, retenção `espelho_cobrado`, sem Resolver dia. Chamado #37287 aberto; perguntas #54491/#54727 sem resposta, validação ou materialização. Em 31/10, #36201 resolvido e #36202 `registrado`; justificativa #3408 rejeitada. DiaPago não fornecido. [nao verificado]
- **DONO L-099:** **ESTRUTURA para a barreira de GPS comprovada no código**: V2 interrompe a tentativa sem coordenadas e a API retorna 403 antes do registro. **Não está demonstrado que essa barreira causou alguma marcação ausente de col456.** (`app/templates/ponto/bater_ponto_v2.html:297`; `app/api/views_core.py:440`, `:454`; causalidade individual [nao verificado].)
- **PORTA DA TELA:** **PORTA NAO EXISTE para o admin retirar essa barreira pelo fluxo analisado**: a condição da API é direta, sem parâmetro de cadastro nesse ramo. A tela de bater ponto existe, mas não é uma ação administrativa que sane o passado. (`app/ponto/urls.py:12`; `app/ponto/views.py:242`; `app/templates/ponto/bater_ponto_v2.html:297`; `app/api/views_core.py:440`.)
- **OBRA:** complementar a pendência existente **L-030 PELA-METADE**, que já nomeia essas barreiras; ela cabe melhor que DST, O10-C, O261 ou o job do lote. Não precisa de ID novo. (`app/docs/LEIS.md:162`.)
- **RESULTADO/VALIDAÇÃO:** remover a barreira deverá permitir alcançar o escritor de batidas; o registro agenda recálculo por evento. Isso não recupera automaticamente uma tentativa histórica não apresentada. O caso exige conferir tentativa, marcação efetivamente gravada, espelho e DiaPago posterior. (`app/ponto/registro_batida.py:157`, `:175`; resultado individual [nao verificado].)

**07 — col836, perguntas #43643 e #44051**

- **ESTADO HOJE:** células #112095/#112096, 13–14/10, `true / indefinida / gerada / escala_suspeita`, retenção `espelho_indefinida`, sem Resolver dia. Ambas responderam `17:00`, foram validadas em 16/10 e têm recusa preenchida, com 192 caracteres. Entretanto, seus carimbos `materializada_em`, via `cadastro_folga`, são de **14/10 e 15/10, anteriores às respostas e à validação**. Chamados #30870/#31151 resolvidos via sistema. DiaPago 0 consta no enunciado; as linhas correspondentes não foram anexadas. [nao verificado]
- **GUARDA comprovada na cópia:** a validação só chama `_materializar_uma` quando `materializada_em is None`; o laço também seleciona somente perguntas sem esse carimbo. A devolução RECUSA-FALA igualmente não age quando já há `materializada_em`. (`app/chamados/services/validacao.py:422`; `app/chamados/services/materializacao.py:1344`; `app/chamados/services/validacao.py:182`.)
- **Limite da conclusão:** esses são os ramos atuais que impedem novo plantio nesse estado. **Qual ramo escreveu historicamente a recusa de 192 caracteres é [nao verificado]**: comprimento não identifica motivo. A via `cadastro_folga` corresponde ao agrupamento LIMBO-FOLGAS; não significa, por si, batida plantada. (`app/chamados/models.py:2265`; `app/chamados/juizes.py:1463`.)
- **DONO L-099:** **ESTRUTURA para o percurso sem reprocessamento acessível de pergunta validada/carimbada cujo dia continua acusando**; a correção do cadastro subjacente deve permanecer separada. O dono formal dos dois dias não foi medido. (`app/chamados/services/validacao.py:366`, `:422`; `CLAUDE.md:176`; dono formal [nao verificado].)
- **PORTA DA TELA:** existe Validar no detalhe, mas **não resolve essas duas perguntas hoje**: o template não oferece o botão para pergunta validada e o backend responde `ja_validada`. **PORTA NAO EXISTE nesse fluxo para reprocessar esse estado.** (`app/chamados/views.py:483`, `:501`; `app/templates/chamados/partials/_pergunta_card.html:42`, `:92`; `app/chamados/services/validacao.py:366`.)
- **OBRA:** **complemento O261**, abrangendo carimbo, validação, lastro e saída operacional. Não confundir com o plantio noturno deslocado: esse mecanismo não foi demonstrado aqui. (`app/docs/BACKLOG.md:1450`; mecanismo individual [nao verificado].)
- **VALIDAÇÃO OPERACIONAL pendente:** esclarecer a recusa textual e o lastro do carimbo, executar a resolução adequada pela tela e conferir célula, eventual batida e DiaPago. Nenhum desses resultados posteriores foi fornecido. [nao verificado]

**08 — col617, atestado**

- **ESTADO HOJE:** a FICHA **não mostra atestado pendente atualmente**: #5380 está rejeitado e #5399 aprovado para 05–06/11. Células #122699/#122700 `true / concorde / gerada / ausencia`; chamados #37140/#37319 resolvidos. As perguntas de 05/11 foram carimbadas por `ausencia_dia`; #54517, de 06/11, por `celula_absolveu`. DiaPago não fornecido. [nao verificado]
- **O QUE O ESPELHO MOSTRA ENQUANTO PENDENTE:** o montador separa cobertura de cobrança e cobertura de folha. O juiz da palavra pode mostrar **“Atestado aguardando decisao”**, preservando o veredito: o pedido aberto não concede o abono da aprovação. (`app/ponto/services/espelho.py:582`, `:588`, `:878`; `app/ponto/services/dia_decidido.py:224`, `:420`.)
- **DONO L-099:** **não há divergência atual demonstrada para atribuir um dono**. Aprovação documental é ato administrativo; eventual incoerência persistente entre os leitores após a decisão seria ESTRUTURA. (`CLAUDE.md:176`; divergência do caso [nao verificado].)
- **PORTA DA TELA:** **Ausências → abrir o registro → Aprovar**, com permissão `editar_ausencias`; o botão depende das condições documentais. View chama `decidir`, que escreve status e carimbos do decisor. (`app/ponto/urls.py:65`; `app/ponto/views.py:2599`, `:2603`, `:2643`; `app/templates/ponto/partials/painel_ausencia.html:306`, `:321`, `:324`; `app/ponto/services/ausencia.py:911`, `:913`.)
- **RESULTADO:** o save da ausência provoca julgamento dos dias e a porta grava veredito/status. **A aprovação apenas invalida o fechamento; não grava imediatamente o DiaPago.** O recálculo posterior chama sua lavratura. (`app/ponto/signals.py:93`, `:143`; `app/ponto/portas/celula.py:1030`; `app/ponto/services/ausencia.py:946`; `app/ponto/services/fechamento.py:1640`, `:1184`.)
- **PORTA COMPLEMENTAR para apuração:** botão **Recalcular** no fechamento, cuja view chama `recalcular_fechamento_mes`. Sua execução e resultado neste caso não foram fornecidos. (`app/templates/ponto/fechamento.html:87`; `app/ponto/views.py:1226`, `:1252`; execução [nao verificado].)
- **OBRA:** não atribuir automaticamente a DST: seu escopo declara ausência lançada como dia decidido, sem nova porta de decisão do dia. **Validação pendente:** conferir período aprovado, resultado no espelho e lavratura posterior do DiaPago. (`app/docs/BACKLOG.md:1793`; resultado operacional [nao verificado].)

**09 — col1000, 29–30/10, chamados #37325/#37326**

- **ESTADO HOJE:** células #142904/#142905 `true / furo / gerada / irregular`, `pede_decisao=true`, `resolver_dia=false`, sem retenção apresentada. Chamados `em_analise`, sem resolução/via. Perguntas #54529/#54530, `intervalo_volta`, sem resposta, validação ou materialização, veredito `nao`. O dado apresenta E/S/S em cada dia, sem retorno E do intervalo. DiaPago não fornecido. [nao verificado]
- **DONO L-099:** **BATIDA como encaminhamento do retorno ausente**, condicionado à confirmação dos insumos cadastrais. O juiz prioriza folga não definida/ausência de vínculo antes da contagem ímpar; esses insumos por dia não vieram. (`app/ponto/management/commands/e6_oraculo.py:98`, `:106`; dono formal [nao verificado].)
- **PORTA DA TELA:** detalhe `/chamados/local/<id>/` → resposta do colaborador → **Validar pelo admin**. A view chama a autoridade de validação; sem resposta ela recusa `sem_resposta`. (`app/chamados/urls.py:14`; `app/chamados/views.py:359`, `:483`, `:1283`; `app/templates/chamados/partials/_pergunta_card.html:29`, `:92`; `app/chamados/services/validacao.py:363`.)
- **Limite da porta:** o modal **Corrigir** existe, mas fica dentro da condição `respondida_em`; para essas perguntas ainda sem resposta ele só mostra espera. Não há nessa tela uma correção administrativa de horário oferecida **nesse estado**. (`app/templates/chamados/partials/modal_fio.html:212`, `:295`, `:313`.)
- **RESULTADO esperado:** materialização escreve pela porta `registrar_batida`; em intervalo fixo pode usar o horário do marco, não a hora livre informada. A validação agenda recálculo após commit, chegando à lavratura do DiaPago. Aceitação e resultado concreto dependem das guardas e não foram observados. (`app/chamados/services/materializacao.py:404`, `:901`; `app/chamados/services/validacao.py:406`; `app/ponto/services/fechamento.py:1794`, `:1808`, `:1184`; resultado [nao verificado].)
- **OBRA:** tratamento do dado pela porta existente; **complemento O261** se o requisito for dar ao admin saída antes da resposta. Não é a classe DST dos dias sem trabalho. (`app/docs/BACKLOG.md:1450`, `:1793`.)
- **VALIDAÇÃO OPERACIONAL pendente:** obter o fato do intervalo, executar a ação e conferir batida, espelho e DiaPago; apenas mudar o status do chamado não sana o furo. (`app/chamados/models.py:185`; execução [nao verificado].)

**10 — col399, 31/10**

- **ESTADO HOJE:** célula #138663 `false / fato_sem_previsao / gerada / folga_trabalhada`, sem Resolver dia/retenção. Batidas #142321 E 06:20:43 e #142798 S 18:22:41 aprovadas e não retratadas. Pergunta #52088 carimbada por `celula_absolveu`. A troca #5373 é **para 24/10**, aprovada e criada **em 04/11**, não “registrada em 24/10”; não há par/documentos apresentados. DiaPago não fornecido. [nao verificado]
- **DONO L-099:** **ESTRUTURA da porta de troca faltante**. O dono formal da divergência cadastral/financeira desse dia não foi medido. (`app/docs/RELATO.md:64`; `app/ponto/management/commands/e6_oraculo.py:98`; dono do dado [nao verificado].)
- **PORTA DA TELA: PORTA NAO EXISTE para confirmar a troca.** O backend O10-B existe: `trocar_plantao` exige os dois lados e documentos, e chama `editar_dia` para cedente e substituto dentro do atomic. Escreve trabalha, DNA, geradora, origem editada, autor/data e trilha. (`app/ponto/portas/celula.py:632`, `:653`, `:662`, `:673`, `:603`.)
- **O QUE FALTA:** view e formulário que chamem essa operação com substituto, duas cartas e motivo. O despacho O10-C registra expressamente ausência de chamador de UI. **Resolver dia não substitui isso**: seu formulário não confirma o par de troca, e “trabalhou” recusa natureza diferente de trabalho. (`app/docs/RELATO.md:64`; `app/templates/ponto/partials/_veredito_dia.html:21`; `app/ponto/services/veredito_celula.py:221`.)
- **OBRA:** **sim, O10-C**, complemento da porta já construída. DST pode tratar decisão sobre batida em folga, mas não substitui a confirmação documental da troca. (`app/docs/BACKLOG.md:1811`, `:1793`.)
- **RESULTADO/VALIDAÇÃO:** a porta reavalia os dias e solicita recálculo; este grava DiaPago. Falta executar pela futura tela e conferir **os dois lados** no espelho e DiaPago, inclusive repetição idempotente. (`app/ponto/portas/celula.py:422`, `:445`; `app/ponto/services/dia_pago.py:357`; resultado operacional [nao verificado].)

**11/12 — col921, competência 11**

- **ESTADO HOJE:** `batidas=[]`. Em 21–23 e 25–29/10: `true / furo / gerada / irregular`; em 30/10 e 01–05/11: `true / cobrado / gerada / irregular`; em 06/11: `true / cobrado / gerada / previsto`. Folgas 24 e 31/10: `false / concorde / gerada / folga`. Resolver dia falso em todos esses dias. Há perguntas sem resposta/materialização e chamados de batida abertos, além do cadastro #37165 aberto. As seis faltas aprovadas apresentadas pertencem a 14–20/10, competência anterior. DiaPago não fornecido. [nao verificado]
- **CONFERÊNCIA DST:** **não corresponde a `folga_trabalhada sem Resolver dia`**: esse status não aparece na FICHA de col921. Para os dias irregulares da competência corrente, a oferta de Resolver dia é barrada por `pode_ultimo_recurso`; para folga trabalhada, adicionalmente, o status nem integra o conjunto que oferece o botão. (`app/colaboradores/services/calendario.py:94`, `:103`, `:455`, `:738`; estado individual [nao verificado].)
- **DONO L-099:** **CADASTRO como encaminhamento da pendência #37165**, mas sua incidência formal em cada dia é **[nao verificado]**. Zero batidas, isoladamente, não permite chamar o caso de BATIDA pelo ramo de paridade ímpar. (`app/ponto/management/commands/e6_oraculo.py:98`, `:106`, `:110`; chamado [nao verificado].)
- **PORTA DA TELA existente para decisão factual:** **Ausências → Lançar**, escolhendo colaborador, tipo e data; a view cria ausência aprovada pelo admin. Se a decisão for falta, é necessário confirmar primeiro a previsão de trabalho: a autoridade veta falta em folga. (`app/ponto/urls.py:57`; `app/ponto/views.py:2368`, `:2415`; `app/templates/ponto/lancar_ausencia.html:12`, `:26`, `:88`; `app/ponto/precedencia.py:536`.)
- **RESULTADO:** falta decidida pode lavrar status `falta`; ausência aprovada pode lavrar `ausencia`. Isso não demonstra atualização do DiaPago: conferir a lavratura após apuração, pelo percurso de recálculo já indicado. (`app/ponto/status_dia.py:139`, `:141`; `app/ponto/services/fechamento.py:1184`; resultado individual [nao verificado].)
- **OBRA:** não atribuir a DST apenas porque falta o botão. Sanear cadastro e decidir os fatos pelas portas existentes; eventual beco de pergunta/fio cabe em **O261**, depois de identificado. Não há fundamento apresentado para ID novo. (`app/docs/BACKLOG.md:1793`, `:1450`; enquadramento individual [nao verificado].)
- **VALIDAÇÃO OPERACIONAL pendente para ambos:** confirmar cadastro dos dias, executar decisões justificadas e conferir competência 11 no espelho/DiaPago. Os itens continuam abertos no critério solicitado. [nao verificado]

**03 — validar-lote: duração HTTP e recusas são problemas separados**

- **ESTADO HOJE:** os logs agregados fornecidos mostram tentativas com 29, 37, 40 e 42 recusadas; as quatro mais recentes apresentadas têm 40. O `ui_lote.log` mostra respostas **HTTP 200**, inclusive em 40–122 segundos. Não há, nesse anexo, timeout efetivamente registrado. [nao verificado]
- **DONO L-099:** **ESTRUTURA para o processamento longo dentro da requisição**. A view chama o serviço sincronamente antes de renderizar. (`app/chamados/views_cobrar.py:372`, `:381`; `CLAUDE.md:89`.)
- **TIMEOUT:** `gthread` e `timeout=30` estão configurados, mas a própria documentação da cópia distingue worker travado de requisição longa. Portanto, **“passou de 30 segundos, caiu por timeout” não está provado**. (`app/gunicorn.conf.py:19`, `:21`; `app/docs/TICKETS.md:132`; queda concreta [nao verificado].)
- **PORTA DA TELA:** `/chamados/validar-lote/tela/`, formulário HTMX; o resultado distingue recusadas e validadas sem gravação e apresenta motivos por referência. (`app/chamados/urls.py:37`; `app/chamados/views_cobrar.py:372`; `app/templates/chamados/partials/_lote_preview.html:7`, `:21`.)
- **OBRA:** **LOTE-VALIDAR-VIRA-JOB**, já existente para esse endpoint. O job resolve a ocupação da requisição; não elimina recusas de negócio. (`app/docs/BACKLOG.md:1806`; `app/chamados/services/validacao.py:682`.)

**Não é possível distribuir as 40 recusas por causa com o dado anexado.** Faltam as referências daquele POST e o retorno `recusados[].motivo`; `RECUSA03` fornece somente comprimento do texto, motivo da pergunta e via atual. [nao verificado]

Os ramos que recusam são:

| Motivo/ramo da cópia | Referência | Contagem nas 40 |
|---|---|---|
| Caso não encontrado | `app/chamados/services/validacao.py:682` | [nao verificado] |
| Já validada | `app/chamados/services/validacao.py:685` | [nao verificado] |
| Fora da classe A permitida ao lote | `app/chamados/services/validacao.py:688` | [nao verificado] |
| Escape, resposta de posto ou confirmação sem marco | `app/chamados/services/validacao.py:807`, `:809`, `:815` | [nao verificado] |
| Escolha que exige decisão individual | `app/chamados/services/validacao.py:816` | [nao verificado] |
| Resposta vazia, tipo inadequado ou hora ilegível | `app/chamados/services/validacao.py:813`, `:817` | [nao verificado] |
| Sem marco esperado | `app/chamados/services/validacao.py:820` | [nao verificado] |
| AM/PM ou desvio além do envelope | `app/chamados/services/validacao.py:822` | [nao verificado] |
| Batida apurável próxima da hora | `app/chamados/services/validacao.py:827` | [nao verificado] |
| Validação individual não deixou carimbo | `app/chamados/services/validacao.py:712` | [nao verificado] |

O agrupamento possível dos **68 registros anexados**, por **via atual**, é: `admin_decidiu=48`, `declaracao=8`, `paridade_recusada=7`, `ausencia_dia=3`, `fato_ocupado=1`, via vazia=1. **Isso não é decomposição causal das 40 recusas de um POST.** [nao verificado]

Uma recusa antiga pode coexistir com materialização posterior: a validação não limpa necessariamente `recusa_motivo`. O resultado do lote também separa `validadas`, `recusados` e `nao_gravados`. (`app/chamados/services/validacao.py:396`, `:705`, `:715`; `app/chamados/services/materializacao.py:1064`.)

**Validação operacional pendente:** preservar o resultado por referência de uma tentativa, agrupar seus motivos reais e conferir espelho/DiaPago dos itens efetivamente aceitos. O anexo não permite executar essa conferência. [nao verificado]

**Quem escreve e quem lê os campos de estado usados acima**

| Campo | Escritor | Leitor |
|---|---|---|
| Célula `trabalha`, `origem`, DNA | geração/regeneração/edição na porta: `app/ponto/portas/celula.py:259`, `:272`, `:603`, `:849` | autoridade de dia de trabalho: `app/escala/models.py:1094`; edição/regeneração lê origem em `app/ponto/portas/celula.py:263`, `:854` |
| Célula `veredito`, via, data e ata | `lavrar_veredito`: `app/ponto/portas/celula.py:1028` | status: `app/ponto/status_dia.py:113`; decisão humana no calendário: `app/colaboradores/services/calendario.py:413` |
| `status_dia`, `status_rotulo` | mesma lavra, por `_aplicar_status`: `app/ponto/portas/celula.py:961`, `:1033` | D4: `app/ponto/status_dia.py:287`; calendário: `app/colaboradores/services/calendario.py:663`; template: `app/templates/colaboradores/partials/_calendario_grade.html:99` |
| `resolver_dia`, `pede_decisao` | derivados, não estad

[truncado em 20000 de 23383 caracteres]

## 06/11 20:46 · sessao a5ee1a5bb961 · - · - · origem=Ronald (tmux)

Leitores das chaves declaradas:

| Campo | Quem lê e efeito |
|---|---|
| `RESULTADOS.numero` | `placar()` usa sua veracidade na guarda de prova: valor verdadeiro com prova vazia vira `PENDENTE`; valor falso evita esse rebaixamento. A contagem publica o estado resultante. (`app/core/placar_estrutural.py:647`, `app/core/placar_estrutural.py:649`, `app/core/placar_estrutural.py:700`) `gerar_estado` publica o valor ou “não medido”. (`bin/gerar_estado.py:99`, `bin/gerar_estado.py:116`) O selo R6 lê sua frase, extrai contagens por regex e converte uma delas a inteiro para confrontar o juiz. (`app/core/tests/test_selo_r6_frase_do_juiz.py:17`, `app/core/tests/test_selo_r6_frase_do_juiz.py:22`, `app/core/tests/test_selo_r6_frase_do_juiz.py:25`) |
| `RESULTADOS.prova` | `.strip()` decide rebaixamento para `PENDENTE`, portanto estado e contagem. (`app/core/placar_estrutural.py:647`, `app/core/placar_estrutural.py:649`, `app/core/placar_estrutural.py:700`) Publicada pelo gerador. (`bin/gerar_estado.py:100`) As provas de R1 e SOMA determinam aprovação do selo de medição única e o conjunto de arquivos citados. (`app/core/tests/test_r1_termometro_uma_medicao.py:52`, `app/core/tests/test_r1_termometro_uma_medicao.py:57`, `app/core/tests/test_r1_termometro_uma_medicao.py:69`) |
| `RESULTADOS.fonte_do_medido` | É glob operacional: o gerador busca caminhos, extrai datas de seus nomes e escolhe a maior; entrega isso a `placar()` e `veredito()`. Decide data/cadência/cor, além do resultado do selo R1. (`bin/gerar_estado.py:47`, `bin/gerar_estado.py:51`, `bin/gerar_estado.py:56`, `bin/gerar_estado.py:80`, `bin/gerar_estado.py:84`, `app/core/tests/test_r1_termometro_uma_medicao.py:52`) |
| `RESULTADOS.fonte` | É copiada para o resultado de `placar()`. (`app/core/placar_estrutural.py:645`) O gerador apenas menciona o nome do campo em uma explicação fixa; não consulta seu valor nesse trecho. (`bin/gerar_estado.py:121`) Seu valor-string participa do selo genérico de afirmações fora do ar; pode mudar acusação e RED. (`bin/tests/test_placar_nao_afirma_fora_do_ar.sh:67`, `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:75`, `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:100`, `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:110`) |
| `RESULTADOS.nota_do_papel` | Copiada por `placar()` e examinada pelo mesmo selo genérico de todos os campos-string; pode mudar o resultado desse selo. (`app/core/placar_estrutural.py:645`, `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:67`, `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:75`) Leitor nominal que use esse valor para classificar papel: [não verificado]. |
| `ETAPAS.prova` | Decide `FEITO → EM_CURSO`, quantidade de feitos e percentual. (`app/core/espelho_verdade.py:211`, `app/core/espelho_verdade.py:214`, `app/core/espelho_verdade.py:217`) Publicada pelo gerador. (`bin/gerar_estado.py:164`) A presença de `TERMOMETRO R1` seleciona quais itens serão fiscalizados pelo selo de medição única; removê-la retira um item do censo. (`app/core/tests/test_r1_termometro_uma_medicao.py:45`, `app/core/tests/test_r1_termometro_uma_medicao.py:64`) O limite está explicitamente confessado no próprio teste. (`app/core/tests/test_r1_termometro_uma_medicao.py:15`) |

Contraexemplo novo confirmado em memória, sem criar arquivos: inserir `contratos_estruturais: 0/999 verdes` no início da docstring de `placar_estrutural.py` altera o primeiro casamento da regex do handoff de `19/20` para `0/999`, enquanto o dump com docstring mascarada permanece idêntico. O leitor pesquisa o texto bruto inteiro e publica o primeiro casamento. (`app/core/placar_estrutural.py:2`, `bin/tests/test_lei_akita.sh:195`, `bin/handoff_sessao.sh:179`, `bin/handoff_sessao.sh:181`) O selo R6 compara exclusivamente `RESULTADOS['R6']['numero']` e não essa leitura do handoff. (`app/core/tests/test_selo_r6_frase_do_juiz.py:17`, `app/core/tests/test_selo_r6_frase_do_juiz.py:22`)

A interpretação “prova/fonte apenas documentam uma medição” não vale para esses leitores: `prova` serve como condição de promoção/rebaixamento e seleção de fiscalização; `fonte_do_medido` seleciona os arquivos que fornecem a data decisória. (`app/core/placar_estrutural.py:647`, `app/core/espelho_verdade.py:211`, `app/core/tests/test_r1_termometro_uma_medicao.py:46`, `bin/gerar_estado.py:51`)

## 06/11 20:46 · sessao 1b79a84b9a49 · - · - · origem=Ronald (tmux)

- **Caminho principal da cópia:** dashboard padrão usa `dashboard_colaborador_v2.html`, cujo botão aponta para `/ponto/bater-v2/`; essa rota renderiza `bater_ponto_v2.html`. (`app/core/views.py:270`; `app/templates/core/dashboard_colaborador_v2.html:31`; `app/ponto/urls.py:12`; `app/ponto/views.py:356-357`)

- **Sem rede, com tela disponível:** `v2Confirmar()` trava o botão, exige selfie e GPS e só então consulta `navigator.onLine`. Sem foto/GPS retorna com aviso e botão reabilitado; havendo requisitos, chama `registrarBatidaOffline`, mostra “salva no celular” e oculta o botão. Falha da gravação mostra “Erro ao registrar” e reabilita o botão. (`app/templates/ponto/bater_ponto_v2.html:285-302`; `app/templates/ponto/bater_ponto_v2.html:320-325`; `app/templates/ponto/bater_ponto_v2.html:367-380`)

- **GUARDADA em IndexedDB:** banco `juliani-offline`, versão 2; object store `batidas_pendentes`, chave `id` autoincremental. Guarda tipo, timestamp corrigido pelo gap, timestamp_dispositivo ISO, gap, GPS e foto Blob; resolve após completar a transação. Não é fila em localStorage ou Cache Storage. (`app/static/pwa.js:48-59`; `app/static/pwa.js:107-123`)

- **Reenvio real:** evento `online`, inicialização da página quando `navigator.onLine`, ou mensagem SW `SYNC_BATIDAS`; exige meta `user-perfil=colaborador`, lê toda fila, ordena e envia multipart para `/api/ponto/bater/`. Sucesso com JSON interpretável exclui item; HTTP 400 também exclui, com aviso somente no console; outras falhas deixam item para próxima sincronização. (`app/static/pwa.js:15-17`; `app/static/pwa.js:126-180`; `app/static/pwa.js:320-328`)

- **Background Sync incompleto:** SW tem handler `sync-batidas`: se houver janela, delega por mensagem; sem janela, apenas notifica para abrir o app. Não existe chamada de registro da tag nos produtores da fila ou na inicialização examinados; tampouco SW faz upload autônomo. (`app/static/sw.js:112-130`; `app/static/pwa.js:107-124`; `app/static/pwa.js:323-346`)

- **Timestamp muda entre caminhos:** v2 online monta `FormData` de formulário com tipo/GPS/foto e envia sem `timestamp_dispositivo`; fila guarda esse campo e o acrescenta ao POST do reenvio. O timestamp corrigido/gap guardados não são acrescentados ao multipart de sincronização; GPS accuracy/capturedAt guardados também não são enviados ali. (`app/templates/ponto/bater_ponto_v2.html:102-107`; `app/templates/ponto/bater_ponto_v2.html:330-338`; `app/static/pwa.js:113-118`; `app/static/pwa.js:143-156`)

- **Queda no POST v2:** rejeição de `fetch` entra no catch e grava fila, gerando o timestamp **nesse momento**, não no clique original; não há AbortController/timeout no POST. HTTP não-ok, inclusive 5xx, mostra erro e reabilita o botão, sem enfileirar. Se servidor gravou e resposta se perdeu, o catch pode guardar nova tentativa com novo timestamp; payload PWA não manda chave explícita de idempotência, nem o `id` local. (`app/templates/ponto/bater_ponto_v2.html:335-365`; `app/static/pwa.js:109-118`; `app/static/pwa.js:143-156`)

- **Outra rota ativa:** `/ponto/bater/` renderiza triagem (`apto.html` nos casos normais), com POST nativo para a própria rota, sem catch/retry/fila na queda durante envio online. Offline declarado abre câmera e depois grava a mesma IndexedDB; falha da câmera esconde confirmar. O formulário também não inclui timestamp_dispositivo. (`app/ponto/urls.py:11`; `app/ponto/views.py:225-235`; `app/ponto/views.py:358`; `app/templates/ponto/triagem/apto.html:86-91`; `app/templates/ponto/triagem/apto.html:210-255`; `app/templates/ponto/triagem/apto.html:289-307`)

- **POST web dessa outra rota:** `ramo_registrar_es` usa `agora=timezone.now()`, entrega esse instante à porta, origem `'app'` e canal calculado sem device_id. Somente `/ponto/batida-offline/` foi aposentado com HTTP 410; `/ponto/bater/` continua roteado. (`app/ponto/services/triagem_batida.py:625-626`; `app/ponto/services/triagem_batida.py:707-719`; `app/ponto/views.py:417-428`; `app/ponto/urls.py:11-13`)

- **Template legado não comprova rota atual:** `bater_ponto.html` tem catch que enfileira rede/HTTP não-ok, mas a view atual escolhe v2 ou triagem. Não atribuir esse tratamento à triagem servida. (`app/templates/ponto/bater_ponto.html:527-556`; `app/ponto/views.py:356-358`)

- **SW não intercepta POST:** retorna de imediato para qualquer método diferente de GET. Navegação HTML usa rede/cache/erro, mas esse ramo não grava a página no cache; instalação pré-cacheia somente JS, manifest e ícones. Assim, não há prova de que seja possível **abrir** a tela v2 pela primeira vez sem rede; a fila descrita pressupõe tela já disponível. (`app/static/sw.js:3-12`; `app/static/sw.js:33-50`; `app/static/sw.js:67-79`)

- **LEI local e origem de eventual cura:** a regra “batida de chão nunca é barrada” e porta única estão em `CLAUDE.md:140-141`; os bloqueios selfie/GPS estão no produtor v2 antes da gravação, e a vulnerabilidade de recebimento/retry está na construção/envio do payload e fila, respectivamente. (`app/templates/ponto/bater_ponto_v2.html:285-325`; `app/templates/ponto/bater_ponto_v2.html:330-365`; `app/static/pwa.js:107-180`)

## 06/11 20:45 · sessao 9290390fa592 · - · - · origem=Ronald (tmux)

- Roteamento da cópia: `Caddyfile:8-9` manda `/api/ponto/*` para `core:8000`; `app/config/urls_core.py:6` inclui `api.urls_core`, que importa `views_core` e registra `ponto/bater/` em `app/api/urls_core.py:5,14`. A outra casca também tem a rota em `app/api/urls.py:67`.

- A API lê `timestamp_dispositivo`, chama `hora_do_aparelho` e recebe `(ts_efetivo, ts_aparelho, motivo)` em `app/api/views_core.py:519-531`. A versão UI faz o mesmo em `app/api/views.py:535-539`.

- A regra aceita epoch em segundos/milissegundos e ISO, converte para datetime aware e devolve `None` quando não consegue ler (`app/api/hora_do_aparelho.py:72-121`). Se o aparelho diz um instante de 0 a 48 horas atrás, esse instante vira o efetivo; se ausente/ilegível, futuro ou mais antigo que 48 horas, o efetivo vira `agora`, mas um horário legível descartado continua retornado como `ts_aparelho` (`app/api/hora_do_aparelho.py:62-67,124-153`).

- Escrita da API: `registrar_batida(colab, ts_efetivo, tipo, 'app', ..., canal=canal_da_batida(device_id), timestamp_dispositivo=ts_aparelho)` em `app/api/views_core.py:680-694` e `app/api/views.py:683-697`; o chokepoint cria `Batida(timestamp=timestamp, origem=origem, **campos)` em `app/ponto/registro_batida.py:152-159`.

- `canal_da_batida` escreve `"apk"` se `device_id` não é vazio e `"pwa"` nos demais casos (`app/ponto/registro_batida.py:53-61`). Portanto, é classificação pelo payload; não uma prova independente do aplicativo efetivamente usado (`app/ponto/registro_batida.py:61`). `origem='app'` é comum aos dois canais na API (`app/api/views_core.py:680-684`).

- `timestamp` é o instante efetivo, `timestamp_dispositivo` é nullable e recebe o instante legível do aparelho; `sincronizado_em` é nullable; `criado_em` tem `auto_now_add=True` (`app/ponto/models.py:23,75-79`). A chamada da API não fornece `sincronizado_em` nem `timestamp_gap_ms`, e o chokepoint não os acrescenta; portanto novas linhas desse caminho deixam esses campos NULL (`app/api/views_core.py:680-694`; `app/ponto/registro_batida.py:104-158`; `app/ponto/models.py:76-79`).

- Consequência para a medida: em uma batida recebida pela API com timestamp do dispositivo legível, é possível medir `criado_em - timestamp_dispositivo`; nos casos aceitos pela janela, também `criado_em - timestamp`. `criado_em` é criação no banco, não um campo explícito de chegada HTTP. Medir por `sincronizado_em` não funciona nessa porta, pois não é preenchido (`app/api/hora_do_aparelho.py:141-153`; `app/api/views_core.py:680-694`; `app/ponto/models.py:75-79`). A fórmula usada para os números de produção informados permanece [nao verificado].

- Leitores concretos: a tela de pendentes mostra `timestamp`, `timestamp_dispositivo`, `timestamp_gap_ms` e `sincronizado_em` (`app/templates/ponto/batidas_offline_pendentes.html:66-90`); `descartadas()` compara `timestamp - timestamp_dispositivo` para contar horários descartados, não atraso de chegada (`app/api/hora_do_aparelho.py:156-185`); `sequencia_relogio_x_jornada()` filtra por `criado_em` e ordena/lê `timestamp` (`app/ponto/turnos.py:2376-2385`); o cron de selfie lê `origem`, `canal` e idade calculada com `timestamp` (`app/ponto/management/commands/marcar_foto_ausente_retro.py:66-77`).

- Atenção à terceira porta: o formulário web em `app/ponto/services/triagem_batida.py:708-719` grava `timestamp=agora`, `origem='app'` e canal derivado de `device_id`, mas **não fornece `timestamp_dispositivo`**. Se esse formulário for usado, a medida de instante do dispositivo não é possível pelos campos dessa batida (`app/ponto/services/triagem_batida.py:708-719`; `app/ponto/models.py:75-79`). O agente de PWA deve estabelecer qual template usa qual porta.

- Idempotência no servidor existe por dados, sem token dedicado nessas chamadas: API consulta mesmo colaborador/tipo em ±5 minutos do `ts_efetivo` e retorna `status='ja_registrada'` (`app/api/views_core.py:549-568`); o chokepoint deduplica mesmo tipo em ±60 segundos e devolve uma linha existente para mesma chave `(colaborador, timestamp)`, mesmo se tipo diferente/retratada (`app/ponto/registro_batida.py:106-122`); o modelo impõe `unique_together = [["colaborador", "timestamp"]]` (`app/ponto/models.py:106`). Isso não substitui uma fila nem mantém o instante original quando o cliente faz uma nova tentativa com horário novo (`app/api/views_core.py:529-554`; `app/ponto/models.py:106`).

- `test_batida_offline.py` não exercita navegador, falta de rede, fila, reconexão ou SW: usa `Client.force_login()` e dois POSTs diretos em `/api/ponto/bater/`, com ISO em `timestamp_dispositivo`, sem `device_id` (`app/ponto/tests/test_batida_offline.py:49-66,78-88`). Pelo classificador, esse payload fica no canal PWA (`app/ponto/registro_batida.py:61`). Prova pretendida: payload válido cria uma linha, repetição imediata do mesmo payload resulta em uma linha e `ja_registrada`, payload inválido dá 400 e timestamp de 72 horas continua aceito em HTTP (`app/ponto/tests/test_batida_offline.py:68-109`); não prova persistência offline de APK ou PWA (`app/ponto/tests/test_batida_offline.py:49-109`).

- O backend trata o formato epoch esperado do APK e ISO esperado do PWA pela mesma função (`app/api/hora_do_aparelho.py:72-121`); o comentário associa APK a `device_id`, assinatura Keystore e foto em POST separado (`app/ponto/registro_batida.py:53-59`; `app/api/views_core.py:459-488`). Armazenamento local e gatilhos de reenvio do **cliente APK** não foram comprovados por código nativo nesta cópia: [nao verificado].

## 06/11 20:45 · sessao e1ea6b97aab5 · - · - · origem=Ronald (tmux)

- `test_sw_offline_fallback.py` lê o **sw.js real**, executando-o em Node com `self`, rede e cache simulados (`app/core/tests/test_sw_offline_fallback.py:23`, `:25`, `:35`). O harness sempre envia **GET** (`:44`), para navegação, tela de bater e ramo restante (`:62`). Os testes exigem `Response.error()` sem rede/cache (`:79`), conteúdo do cache quando existe (`:89`) e preferência pela rede disponível (`:96`). Assim, não exercitam POST, IndexedDB, clique, persistência ou reenvio de batida (`:37`, `:44`).

- `test_batida_offline.py` usa **Django Client autenticado por sessão**, enviando diretamente `/api/ponto/bater/` (`app/ponto/tests/test_batida_offline.py:60`). O payload contém `timestamp_dispositivo` e não contém `device_id` (`:49`); ele representa o caminho API/PWA, não o cliente APK, cuja identificação deve ser conferida na porta (`:51`). Testa criação (`:68`), dois envios do mesmo timestamp retornando `ja_registrada` e apenas uma linha (`:78`), tipo/payload inválidos (`:91`, `:95`), timestamp antigo aceito (`:104`), colaborador sem posto (`:111`) e disputa (`:119`). Não executa `pwa.js`, não desconecta navegador e não prova recuperação real de fila (`:60`).

- O teste adicional explicitamente chamado PWA também usa Django Client e payload sem `device_id` (`app/api/tests/test_api_bater_pwa.py:42`, `:47`). Exercita foto/disputa, não funcionamento offline (`:64`, `:78`, `:95`).

- O SW **não intercepta POST**: retorna antes do `respondWith` quando o método não é GET (`app/static/sw.js:35`). Seu evento Background Sync apenas manda `SYNC_BATIDAS` para páginas abertas; sem página, mostra notificação pedindo abertura (`app/static/sw.js:112`, `:116`, `:119`, `:122`).

- A fila existe: IndexedDB `juliani-offline`, store `batidas_pendentes` (`app/static/pwa.js:50`, `:54`), gravação transacional em `registrarBatidaOffline` (`:107`, `:112`, `:120`); há chamadas reais nos templates v2 (`app/templates/ponto/bater_ponto_v2.html:356`, `:374`) e clássico (`app/templates/ponto/bater_ponto.html:543`, `:597`). Portanto, os testes limitados não autorizam concluir perda, e a hipótese de inexistência de fila é contrariada pelo código citado (`app/static/pwa.js:107`).

- A **lei local** L-030 determina “batida de chão nunca é barrada em runtime”, escrita pela porta `registro_batida.py`, sem fabricação pelo motor (`CLAUDE.md:140`, `app/docs/LEIS.md:78`). A referência explícita à Portaria 671 encontrada nesses textos trata de exibição da marcação crua (`CLAUDE.md:164`, `app/docs/LEIS.md:76`). Artigo, vigência e enquadramento jurídico externo da Portaria: **[nao verificado]**.

- Caso apareça necessidade de cura, a regra local manda atuar na fonte (`CLAUDE.md:17`); para perda na captura/fila PWA, os sítios de origem são chamadas dos templates e `registrarBatidaOffline`/`sincronizarBatidas`, mantendo a gravação servidor na porta única (`app/templates/ponto/bater_ponto_v2.html:356`, `app/static/pwa.js:107`, `:126`, `app/ponto/registro_batida.py:94`). Nenhuma cura foi executada **[nao verificado por evidência de alteração; somente leitura nesta tarefa]**.

## 06/11 20:44 · sessao 1209bcebfb4b · - · - · origem=Ronald (tmux)

**08 — col617**

- **Estado hoje:** não há atestado pendente na FICHA: #5380 rejeitado; #5399 aprovado, cobrindo 05–06/11, ambas células `trabalha=true / concorde / gerada / ausencia`; chamados #37140 e #37319 resolvidos via sistema. DiaPago não foi fornecido. [nao verificado]
- **Pendente no espelho:** a cobertura para cobrança inclui ausências não rejeitadas; a cobertura para folha exige aprovação (`app/ponto/turnos.py:2509`). O montador separa as duas (`app/ponto/services/espelho.py:582`, `:588`) e entrega a aberta como `_pendente_do_dia` (`:878`). O juiz da palavra mantém o veredito e mostra **“Atestado aguardando decisao”**, amarelo (`app/ponto/services/dia_decidido.py:420`, `:224`, `:57`). Portanto, aparência de pendência e abono são estados distintos no código.
- **Porta:** Ausências → drawer → Aprovar (`app/ponto/urls.py:65`; `app/ponto/views.py:2067`; `app/templates/ponto/partials/painel_ausencia.html:306`, `:309`, `:321`). Exige `editar_ausencias`, POST e ausência aberta (`app/ponto/views.py:2603`); chama `_a4_decidir` (`:2643`), que chega à decisão e escritor de estado (`app/ponto/services/ausencia.py:897`, `:911`, `:824`, `:835`). Documento anexado leva à decisão do DP (`:779`, `:791`); sem documento obrigatório, não há botão Aprovar (`painel_ausencia.html:324`).
- **Espelho após ação:** save de Ausência julga os dias (`app/ponto/signals.py:143`, `:147`, `:93`); cartório escreve ata, veredito e status (`app/ponto/services/cartorio.py:1800`; `app/ponto/portas/celula.py:1030`). Palavra de ausência aprovada sai da cobertura de folha (`app/ponto/services/dia_decidido.py:428`).
- **DiaPago após ação:** **não prometer recálculo imediato pela aprovação.** `decidir` invalida fechamento (`app/ponto/services/ausencia.py:946`), zerando `derivado_em` (`app/ponto/services/fechamento.py:1636`). O próprio código distingue invalidação de recálculo (`:1606`). O recálculo posterior escreve DiaPago por `fechamento.py:1184` → `dia_pago.py:357`.
- **Dono/obra:** aprovação documental pertence ao admin; defeito de coerência de tela, se demonstrado, é ESTRUTURA pela L-099 (`CLAUDE.md:176`). A sonda atual não demonstra pendência nem divergência entre espelho/DiaPago. [nao verificado] A obra DST declara explicitamente dia com ausência lançada como “SEM porta, dia decidido” (`app/docs/BACKLOG.md:1793`), portanto não abrir Resolver dia para substituir aprovação de atestado.
- **Fechamento:** não fechar: falta DiaPago e validação operacional do percurso. [nao verificado]

**09 — col1000, 29–30/10**

- **Estado hoje:** células #142904/#142905: `true / furo / gerada / irregular`, `resolver_dia=false`, `pede_decisao=true`; chamados #37325/#37326 `em_analise`, sem resolução/via; perguntas #54529/#54530 `intervalo_volta`, sem resposta, validação ou materialização, veredito `nao`. Existem três marcações em cada dia: E/S/S, faltando retorno E do intervalo. DiaPago não fornecido. [nao verificado]
- **Porta atual:** chamado local `/chamados/local/<id>/` (`app/chamados/urls.py:14`; `app/chamados/views.py:359`). O cartão oferece Validar somente após resposta (`app/templates/chamados/partials/_pergunta_card.html:29`, `:42`, `:92`); view encaminha à autoridade (`app/chamados/views.py:483`, `:501`). Sem resposta, o serviço recusa `sem_resposta` (`app/chamados/services/validacao.py:363`).
- **Ação operacional necessária hoje:** colaborador responde ao horário na porta de atendimento (`app/chamados/views.py:1228`, `:1283`), writer `resposta_colab/respondida_em` (`app/chamados/services/disputa_emissao.py:1282`); admin valida. Não inventar hora: ela não veio na sonda. [nao verificado]
- **Existe Corrigir:** modal oferece HH:MM e motivo (`app/templates/chamados/partials/modal_fio.html:295`, `:307`), view local também reconhece a ação (`app/chamados/views.py:523`). Porém o formulário está dentro de `if perg.respondida_em` (`modal_fio.html:212`); sem resposta há somente “Aguardando resposta do colaborador” (`:313`). Assim, **não há nessa tela porta de Corrigir oferecida hoje para essas duas perguntas sem resposta**.
- **O que escreve:** Corrigir preserva declaração anterior em followup, grava pela autoridade de resposta e chama validar (`app/chamados/services/acoes_disputa.py:355`, `:366`, `:375`). Materialização de intervalo fixo usa horário do marco, quando disponível, em vez de hora livre (`app/chamados/services/materializacao.py:404`); batida passa por `registrar_batida` (`:901`). Validação agenda recálculo (`app/chamados/services/validacao.py:406`), que recalcula após commit (`app/ponto/services/fechamento.py:1791`, `:1808`) e grava DiaPago (`:1184`; `app/ponto/services/dia_pago.py:357`).
- **Dono:** falta de retorno é candidata BATIDA: juiz formal prioriza contagem ímpar (`app/ponto/management/commands/e6_oraculo.py:62`). Contudo, `folga_nao_definida` e `sem_vinculo_no_dia` precedem essa regra (`:77`, `:85`); a sonda não traz esses insumos do juiz para 29/30, portanto dono formal não medido [nao verificado]. O chamado #35214 de cadastro aberto, sozinho, não prova sua incidência nestes dois dias. [nao verificado]
- **Obra:** complemento O261 para eventual falta de porta admin antes da resposta/resultado verificável; DST não corresponde aos dias `trabalha=true` fornecidos. Encaixe proposto [nao verificado]; DST cobre batida em folga/feriado (`app/docs/BACKLOG.md:1793`).
- **Fechamento:** não fechar: faltam resposta, ação efetiva, DiaPago posterior e validação operacional. [nao verificado]

**Escritores/leitores adicionais**

| Campo utilizado | Escritor | Leitor |
|---|---|---|
| Ausência.status | `app/ponto/services/ausencia.py:808`, `:824` | cobertura `app/ponto/turnos.py:2514`; cabeçalho `app/templates/ponto/partials/painel_ausencia.html:24` |
| Ausência.aprovado_por/aprovada_em | `app/ponto/services/ausencia.py:913` | auditoria `app/templates/ponto/partials/painel_ausencia.html:257` |
| Pergunta.resposta_colab/respondida_em | `app/chamados/services/disputa_emissao.py:1282` | `app/templates/chamados/partials/_pergunta_card.html:29`; guarda `app/chamados/services/validacao.py:363` |
| Pergunta.validada_em | autoridade `app/chamados/services/validacao.py:396` | `app/templates/chamados/partials/_pergunta_card.html:23` |
| Chamado.status_local/resolvido_em/via | `app/chamados/models.py:195`, `:222` | `app/templates/chamados/detalhe_local.html:68`, `:88` |
| Célula.veredito/status | `app/ponto/portas/celula.py:1030` | `app/ponto/status_dia.py:287`; calendário `app/colaboradores/services/calendario.py:606` |
| DiaPago | `app/ponto/services/fechamento.py:1184` → `app/ponto/services/dia_pago.py:357` | `app/ponto/services/espelho.py:406` |

Não executei validação operacional; análise de código somente leitura. [nao verificado] [nao verificado]

_2544 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
