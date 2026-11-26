# CODEX-ESTADO

> Ultima resposta processada: **26/11/2026 12:13** · publicado em **26/11/2026 12:14** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 26/11 12:13 · sessao 0afee0d210dd · SUITE-ROTEIA-PARA-PISTA · leitura · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 26/11 12:13 · sessao 8c908102f452 · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 26/11 12:12 · sessao df4617291765 · - · - · origem=Ronald (tmux)

**Prova que morde.** Exercitar `suite.sh` real com Git de fixture e doubles de SSH, Docker e trava. Um recorte remoto deve publicar o SHA correto, fazer um `pede`, receber ID nu, esperar e buscar o log do mesmo ID, preservar o rc e ter zero chamadas à VM. Ping indisponível, espera estimada acima de 30 min e negativa inequívoca `71` devem produzir uma execução VM com motivo. Depois do aceite, vermelho remoto, perda de conexão e rc ambíguo jamais devem executar VM. O mutante `rota_pista_so_carimba` imprime a rota, omite o `pede` e roda na VM. O mesmo juiz tem de matá-lo pelos efeitos. A fixture precisa reproduzir o ID nu de `pede`, que derrubou M7b quando o selo simulava `id= sha=`, e usar o recibo estruturado apenas em `espera`. O glob da régua inclui `test_*.sh`. `app/docs/RELATO.md:577,603–604,676,2262`, `bin/pista_certifica.sh:13–16`, `bin/tests/test_pista_certifica.sh:48–52`, `bin/regua.sh:138`.

**Selos existentes.** `test_suite_sh.sh` mantém comando canônico e garantias da VM, com eventual ajuste de âncoras e mensagem. `test_suite_motivo.sh` e `test_rm_com_guarda.sh` precisam de fixture que escolha VM deterministicamente quando exercitam comportamento local e de correção de qualquer âncora deslocada. `test_pista_certifica.sh` preserva prova e o literal `_FLAG_SO` que o certificador lê. `test_volta_intermediaria_rc.sh` mantém o contrato de rc. `test_suite_uma_vez_por_arvore.sh` protege hash/régua/pre-push. `test_trava_teste.sh`, `test_montagem_vem_do_arvore_do_push.sh`, `test_labels_fonte_unica.sh` e `test_segredo_fora_do_argv.sh` continuam a guardar as respectivas invariantes locais. `bin/tests/test_suite_sh.sh:25–87,128–137`, `bin/tests/test_suite_motivo.sh:23–53,176–249,284–343`, `bin/tests/test_rm_com_guarda.sh:103–225,229–429`, `bin/pista_certifica.sh:67–70`, `bin/tests/test_pista_certifica.sh:63–99`, `bin/tests/test_volta_intermediaria_rc.sh:17–46`, `bin/tests/test_suite_uma_vez_por_arvore.sh:26,57–82`, `bin/tests/test_trava_teste.sh:40–64,117–137`, `bin/tests/test_montagem_vem_do_arvore_do_push.sh:35–65,67–125`, `bin/tests/test_labels_fonte_unica.sh:20–45`, `bin/tests/test_segredo_fora_do_argv.sh:23–48`.

**Medição.** A trava atual registra em `/tmp/juliani_db_test.lock.quem` só a aquisição do detentor e depois `livre desde`; em timeout registra o limite e retorna 75. `logs/trava_teste.hist`, com pedido/início/fim, está desenhado e aberto. Comandos de leitura pela sessão `date -Is`, `bash bin/trava_teste.sh --quem`, `ssh -o BatchMode=yes -o ConnectTimeout=5 pista estado` e `rg -n '^MEDIDA-SUITE|^POUSO PARCIAL-BORDA' app/docs/RELATO.md`. Contar IDs aceitos distintos no intervalo, atribuir espera VM a pedido→início quando houver os dois carimbos e usar a mesma unidade/janela antes e depois. O RELATO registra ao menos o job `1126000153` em 26/11. Totais, retenção e esperas sem trilha [nao verificado]. `bin/trava_teste.sh:21–42`, `app/docs/BACKLOG.md:944,2344,2957`, `app/docs/RELATO.md:13,2356,2751,3051,3054–3055,5755,12377`.

**Snapshot e concorrência.** Índice temporário isola staging, enquanto objetos e refs entram no Git comum. Capturar adições, alterações, exclusões e modos, com fonte íntegra e estável. Segregar as refs próprias dos commits descartáveis e guardar até o consumo terminal confirmado, retirando por comparação de SHA sem GC na rota. A revalidação atual da VM pode aceitar suja→suja porque compara classificação e SHA vazio. O remoto responde só pelo snapshot fechado. `staticfiles` é ignorado e a VM exige estáticos utilizáveis no bind `:ro`, mais cinco tmpfs para caches/logs/media. Prova equivalente na pista [nao verificado]. As duas vagas documentadas e observadas são concorrência de jobs, distinta de `--parallel` 1–8 dentro da suíte. Calcular ETA pelo primeiro slot que se libera considerando a fila, usar estado como estimativa e preservar o isolamento por job, cuja implementação [nao verificado]. `bin/arvore_hash.sh:13–16,22–28`, `app/docs/RELATO.md:108,111,1616,1651,2762`, `app/docs/BACKLOG.md:2344–2345`, `bin/suite.sh:108–143,172–175`, `bin/arvore_do_push.sh:31–56,80–81,104–118`, `app/docs/PROMPTS.md:2977`.

## 26/11 12:11 · sessao a273e5b5e6ab · - · - · origem=Ronald (tmux)

O único chamador operacional localizado em `bin/` é a volta intermediária. Ela passa `--dir --volta --espera "${VOLTA_ESPERA:-3600}"`. O helper preenche `ONLY` depois, enquanto combinar `--volta` e `--only` explícitos dá rc 2. O contrato deve definir a rota pela seleção efetiva para alcançar a volta intermediária. [bin/volta_intermediaria.sh:33] [bin/suite.sh:197–216] [nao verificado]

- A volta captura stdout e stderr em `<saida>.log`, usa `<saida>.porque` e `<saida>.disjuntor.log`, termina o log com `rc=`, `tempo_s=` e `fim=` e sinaliza em `<saida>.fim`. Seu próprio veredito vem dos códigos de processo. Falha de Ruff/disjuntor ou outro rc de teste dá vermelho, rc 75 significa sem vez e rc 3 significa que a suíte não rodou. `Ran` e `OK/FAILED` continuam disponíveis para a leitura humana do log. [bin/volta_intermediaria.sh:17–53] [nao verificado]
- Chamadas diretas da sessão e os comandos entregues pelos agentes usam o stdout/stderr da porta com a captura existente. A sessão principal espera em fundo e o agente entrega comando e caminho do log. Há exemplos com `--only` e com filtros `-v` e `-k`, uma inteira em cópia pela VM e uma validação `--seco`. [CLAUDE.md:95–105] [app/docs/PISTA.md:58–62] [app/docs/RELATO.md:804,814,1576,1614] [app/docs/PROMPTS.md:2343]
- `.claude/settings.json` aponta para hooks e statusline externos, e o verificador de `.claude/agents/` é leitura sem suíte. Os chamadores de teste são `test_suite_motivo.sh` pelas chamadas indiretas de `_roda`, e `test_rm_com_guarda.sh` pelas inteiras de pouso diretas/indiretas. `test_volta_intermediaria_rc.sh` invoca a volta real com dublê do `suite.sh`. [.claude/settings.json:2–77] [.claude/agents/verificador.md:7] [bin/tests/test_suite_motivo.sh:53,147,196–226] [bin/tests/test_rm_com_guarda.sh:145–187,234–256,313–339] [bin/tests/test_volta_intermediaria_rc.sh:12–20]
- A conduta requer `Ran N`, o último `OK/FAILED` com fronteira e o rc do processo. Prefixar as linhas remotas, devolver apenas o recibo de `espera`, truncar o log ou misturar dois runs põe essa leitura em risco. Os parsers automáticos da régua, pre-push, vigia, isolamento e moldes atuam sobre seus próprios logs. [CLAUDE.md:119–123] [app/docs/LEIS.md:101] [bin/regua.sh:165–179] [bin/pre-push.sh:130–151] [bin/vigia_arvore.sh:76–90,105–107,131–136] [bin/vigia_veredito.sh:10–15] [bin/isolamento.sh:25–34] [bin/molde_autocura/cadeia.sh:52] [bin/molde_autocura/esteira.sh:15] [bin/molde_fatia/cadeia.sh:55] [bin/molde_fatia/esteira.sh:16] [bin/molde_fatia/rodar.sh:31,48] [nao verificado]
- `AMBIENTE argv=` sai do próprio teste de `core` quando selecionado e representa cada argumento em uma palavra. `pista_certifica.sh` usa o campo para a inteira de pouso, rejeita recorte, importa `_FLAG_SO` diretamente de `suite.sh` e recebe preenchimento do recibo em arquivo separado. Preservar o log original e o formato de `_FLAG_SO`. [app/core/tests/test_ambiente_do_run.py:30–56] [bin/pista_certifica.sh:13–17,61–92] [CLAUDE.md:126] [nao verificado]
- `pede` foi documentado como ID nu em uma reprovação anterior. O parser deve reconhecer a forma comprovada pela sessão, e `espera` autenticar ID, SHA e rc contra o log integral. A interface antiga anuncia apenas `--only`/`--parallel` e há relato explícito de que não recebe `-k`. Se a pista não representar argv, semântica de `--espera` ou `REGUA_DB` da chamada, fica na VM com motivo. Estado e interface atuais do serviço [nao verificado]. [app/docs/RELATO.md:508,577,604,925] [app/docs/BACKLOG.md:2344] [bin/pista_certifica.sh:14–16,53–60] [bin/suite.sh:169–181,227–248,336–341,367–373] [nao verificado]
- O rc bruto 3 do processo é mapeado a 1 na VM, preservando 3 para run que não nasceu. A VM atualmente deixa rc 75 do comando passar, por isso a classificação remota precisa olhar a fase e a admissão, pois a volta trata 75 como “sem vez”. Perda de recibo após admissão possível requer reconciliação do ID e ausência de execução duplicada. [bin/suite.sh:145–153] [bin/trava_teste.sh:35–42] [bin/volta_intermediaria.sh:36–46] [app/docs/RELATO.md:603,925] [nao verificado]

## 26/11 11:43 · sessao 9765fa36b75c · UI-RESCISAO · leitura · origem=Code

## 0. Perguntas de lei

**As três perguntas do censo1 sobrevivem.** Caem as partes já decididas sobre agendamento, fonte dos números, leitura do espelho e encaminhamento ao Domínio. Restam os avos de férias, a projeção do aviso e o tratamento de DSR e banco no acerto parcial. A conferência compara o censo1 anexado com os arquivos da cópia. A cópia não contém `.git`, então a correspondência com `558de6bd4bba318bac4130a14936946e218f885d`, o diff contra `7621a973f` e o estado atual de produção ficam **[nao verificado]**. `CLAUDE.md:275–280`, `CLAUDE.md:412`, `app/docs/RELATO.md:3973`

1. **Férias proporcionais.** Quais avos e frações entram até o desligamento e qual efeito a projeção do aviso tem sobre esse direito? O código responde o saldo integral do aquisitivo. O backlog registra que a apuração de proporcionais e avos até a data saiu do piloto. Faltam medir os desligamentos com ciclo incompleto, sem ciclo cobrindo a data e com gozo na data, e cotejar com demonstrativos de acerto que tragam os avos concedidos. Numeradores e demonstrativos **[nao verificado]**. `app/ferias/models.py:94–105`, `app/ferias/models.py:219–220`, `app/docs/BACKLOG.md:1179`
2. **Aviso prévio.** Como registrar trabalhado e indenizado e qual data cada modalidade projeta para os direitos? **Cai a parte sobre agendamento.** A porta aceita data futura até hoje + 90 dias, inclusive, mantém o estado durante a espera e efetiva na data pelo cron. A referência à Lei 12.506 fundamenta o teto, e a porta recebe data e motivo. Faltam medir a frequência do agendamento registrado e obter a modalidade e as datas do aviso e do acerto para cotejo. Modalidades, projeções e respectivos totais **[nao verificado]**. `app/colaboradores/services/desligamento.py:17–19`, `app/colaboradores/services/desligamento.py:35–78`, `app/colaboradores/services/desligamento.py:102–126`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:37–57`
3. **DSR e banco de horas na competência parcial.** Qual reflexo de DSR pertence ao acerto e como liquidar ou baixar crédito e débito de banco? **Caem a escolha do canal e o rateio pela tela.** H12 encaminha as verbas ao módulo próprio do Domínio. `DiaPago` põe reflexo e banco no ajuste indivisível da competência e exclui esse ajuste da leitura por intervalo. Faltam medir ajustes ausentes, zerados, positivos e negativos, por competência e marco de exportação, e cotejar com as rubricas e baixas do acerto. Valores do acerto **[nao verificado]**. `app/folha/export.py:54–60`, `app/folha/export.py:453–460`, `app/folha/export.py:856–871`, `app/ponto/services/dia_pago.py:752–769`

### Filtro LEI-SO-QUANDO-NOVA

| Lei ou corte candidato | Veredito para as perguntas |
|---|---|
| **LEI-AKITA 2 / L-002**, L-001, L-007 e L-011, juiz único, escritor único e testemunha lê | **Respondem** quem decide, quem grava e quem lê. A tela usa autoridades existentes, o ato usa as portas, e um juiz novo exige corte. **Não respondem** a fórmula, a modalidade ou a destinação das verbas em 1–3. `app/docs/LEIS.md:59–65`, `app/docs/LEIS.md:69`, `app/docs/GLOSSARIO.md:11–12`, `CLAUDE.md:74–76` |
| **Célula soberana**, L-020, L-024, L-025, L-030, L-060/061, `CLAUDE.md` §4 e §4a | **Respondem** a origem de jornada, previsto, batidas e veredito, e o papel do cron e dos consumidores. **Não respondem** quantos avos, qual projeção ou qual verba sai do ajuste. Os vetos atuais de férias e desligado na porta da batida constam como exceções ainda presentes. `app/docs/LEIS.md:71–78`, `app/docs/LEIS.md:94–95`, `app/docs/LEIS.md:168`, `CLAUDE.md:142–174`, `app/api/views_core.py:389–397` |
| L-021, L-112, Lei 12.506 citada na porta e `aviso_home_office` | **Respondem** a validade da vigência, o início declarado, o horizonte de agendamento e o efeito abonado do lançamento de aviso em home office. **Não respondem** a projeção de direitos ou a modalidade indenizada da pergunta 2. `app/docs/LEIS.md:72`, `app/docs/LEIS.md:157`, `app/colaboradores/services/desligamento.py:17–19`, `app/colaboradores/services/desligamento.py:52–78`, `app/ponto/catalogo/ausencias.py:104` |
| CLT arts. 130/131, 134, 135 e 137 nos sítios locais, O157, L-039/088/089 e corte de fracionamento | **Respondem** o insumo de faltas decididas, as faixas de direito integral, o vencimento, o aviso de gozo e as frações de **gozo**. O157 responde que o art. 130 deve considerar falta e suspensão, excluindo atraso e saída antecipada. **Não respondem** a fração de mês que gera avo nem o efeito do aviso da dispensa nas perguntas 1–2. `app/ferias/models.py:52–62`, `app/ferias/models.py:144–150`, `app/ferias/situacao.py:192–265`, `app/docs/BACKLOG.md:1434`, `app/docs/BACKLOG.md:1541`, `app/docs/CORTES.md:520`, `app/docs/LEIS.md:87`, `app/docs/LEIS.md:120–121` |
| L-006, L-034/035/036 e `CLAUDE.md` §6b | **Respondem** de qual cadastro vem a régua aplicável e regras de jornada e adicional. **Não respondem** avos, projeção ou acerto parcial. A aplicação de qualquer convenção ao universo raspado **[nao verificado]**. `app/docs/LEIS.md:64`, `app/docs/LEIS.md:82–84`, `CLAUDE.md:210–238`, `app/core/regua_cct.py:297` |
| L-026/090, L-037/038, L-085, L-095 e O58/O59 | **Respondem** a janela de competência, a atribuição ao início da jornada, a origem das rubricas e a leitura do documento emitido. **Não respondem** avos, projeção nem a liquidação de um ajuste sem dia. A auditoria de L-085 registra divergências, inclusive no motor comercial. `app/docs/LEIS.md:77`, `app/docs/LEIS.md:85–86`, `app/docs/LEIS.md:113`, `app/docs/LEIS.md:117`, `app/docs/LEIS.md:123`, `app/docs/LEIS.md:178`, `app/docs/BACKLOG.md:1473–1474` |
| Lei 605/1949, Súmula 172, CLT art. 59 §2 e H12 | **Respondem** como o motor apura reflexo regular de HE e banco comercial, por que o reflexo fica a cargo do Domínio no TXT mensal e por onde saem as verbas rescisórias. **Não respondem** quais valores o acerto parcial deve aceitar ou baixar. `app/ponto/motor_calculo_v2.py:2581–2607`, `app/ponto/motor_calculo_v2.py:2645–2669`, `app/ponto/motor_calculo_v2.py:3445–3475`, `app/folha/export.py:453–460`, `app/folha/export.py:856–871` |
| L-050/051/052, L-092, L-113 e o corte L-113 × DINHEIRO 2 de 25/11 | **Respondem** a fronteira de chamados, a história da tranca e o tratamento do gravado e do TXT. A proteção do gravado contra recálculo genérico vale na guarda cadastrada, que lista 09/2026. Fora dela se aplica L-113, com trilha, versão anterior preservada, DIFF antes de ato de frota e aviso DP quando a exportada muda. **Não respondem** verba rescisória da pergunta 3. `app/docs/LEIS.md:91–93`, `app/docs/LEIS.md:125`, `app/docs/LEIS.md:155–156`, `CLAUDE.md:428–433`, `app/ponto/services/fechamento.py:36–49`, `app/ponto/services/fechamento.py:578–581` |
| `CORTES.md` DESLIGADO-FORA-DA-JANELA, JUIZ-ESTA-DESLIGADO e os cortes de DSR regular | **Respondem** a aposentadoria de linha fora da janela, o estado de desligado e efeitos de fatos na apuração regular. **Não respondem** projeção de aviso nem acerto de DSR/banco. `app/docs/CORTES.md:80`, `app/docs/CORTES.md:352`, `app/docs/CORTES.md:473`, `app/docs/CORTES.md:479`, `app/docs/CORTES.md:496`, `app/docs/CORTES.md:517`, `app/docs/CORTES.md:558` |
| COMPLEMENTO-DESLIGAR-1311, O50, DESLIGAR-LIQUIDA-TUDO, NAO-REABRE, LEI-DESLIGADO-NAO-TRAVA e L-130 | **Respondem** a composição do ato, o alcance em vínculos, perguntas e fios, a exceção humana à reabertura, o que é resposta válida e o destino informativo dos dias do desligado. **Não respondem** as verbas das três perguntas. `app/docs/RELATO.md:3981–3982`, `app/docs/BACKLOG.md:731–733`, `app/docs/BACKLOG.md:1461–1462`, `app/docs/BACKLOG.md:1474–1475`, `app/docs/LEIS.md:151` |
| `CLAUDE.md` §7b, L-003/110/117 e precedente CORTES-F2-F9 | **Respondem** o filtro: usar a lei escrita quando ela resolve, medir na fonte quando há juiz, e deixar explícito o residual que exige decisão. **Não respondem** os três residuais. `CLAUDE.md:252–280`, `CLAUDE.md:412`, `app/docs/LEIS.md:61`, `app/docs/LEIS.md:136`, `app/docs/LEIS.md:138`, `app/docs/CORTES.md:565` |

### Sonda de leitura para as três perguntas

Trecho para um shell Django já no tenant da amostra, com datas civis explícitas. Retorna agregados. Para a pergunta 1, mede ciclo e gozo no corte. Para a 2, mede a marca de agendamento e o prazo registrado, inclusive para quem já efetivou. Para a 3, mede a linha de ajuste lavrada e o marco da competência. As modalidades e os valores oficiais do acerto precisam vir de um demonstrativo raspado correspondente. Registros cuja reativação limpou `data_demissao` ficam fora desse universo. A sonda ficou **[nao verificado]** em execução. `app/colaboradores/models.py:323–325`, `app/colaboradores/models.py:577–584`, `app/colaboradores/services/desligamento.py:107–117`, `app/colaboradores/services/desligamento.py:241–247`, `app/ferias/services.py:49–70`, `app/ferias/models.py:65–105`, `app/ponto/models.py:311–323`, `app/ponto/services/fechamento.py:1542–1568`

```python
from collections import Counter

from django.db import connection, transaction

from colaboradores.models import Colaborador, HistoricoVinculo
from colaboradores.services.desligamento import esta_desligado
from ferias.models import PeriodoAquisitivo, status_de_periodo
from ferias.services import em_gozo_hoje
from ponto.janelas import janela_atual
from ponto.models import DiaPago
from ponto.services.fechamento import marcos_da_competencia


def faixa_horas(valor):
    if valor > 0:
        return "positivo"
    if valor < 0:
        return "negativo"
    return "zero"


def sondar(empresa_id, de, ate, hoje):
    contagem = Counter()
    horas = Counter()

    with transaction.atomic(durable=True):
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION READ ONLY")

        colabs = Colaborador.objects.filter(
            empresa_id=empresa_id, data_demissao__range=(de, ate)
        ).select_related("empresa").order_by("pk")

        for colab in colabs.iterator(chunk_size=500):
            dia = colab.data_demissao
            efetivado = esta_desligado(colab)
            estado = "efetivado" if efetivado else (
                "agendado" if dia > hoje else "data_vencida_sem_efetivacao"
            )
            contagem["cadastro", estado] += 1

            agendamento = HistoricoVinculo.objects.filter(
                colaborador=colab, motivo="desligamento", data=dia,
                obs__startswith="AGENDADO (aviso previo):",
            ).order_by("-criado_em", "-pk").first()
            contagem["aviso_rastreavel", estado, agendamento is not None] += 1
            if agendamento is not None:
                from django.utils.timezone import localtime
                prazo = (dia - localtime(agendamento.criado_em).date()).days
                contagem["prazo_registrado_dias", estado, prazo] += 1

            if not efetivado:
                continue

            ciclos = list(PeriodoAquisitivo.objects.filter(
                colaborador=colab, data_inicio__lte=dia, data_fim__gte=dia
            ))
            contagem["ciclos_cobrindo_data", len(ciclos)] += 1
            contagem["em_gozo_na_data", em_gozo_hoje(dia, colab).exists()] += 1
            for ciclo in ciclos:
                contagem["status_ciclo_no_corte", status_de_periodo(ciclo, dia)] += 1
                contagem["dias_civis_desde_inicio", (dia - ciclo.data_inicio).days] += 1

            inicio, fim = janela_atual(dia, colab.empresa)
            marco = marcos_da_competencia(empresa_id, fim.month, fim.year)["marco"]
            grupo = ("parcial" if dia < fim else "fim_da_janela", marco)
            contagem["inicio_da_janela", inicio.day] += 1
            ajuste = DiaPago.objects.filter(
                colaborador=colab, mes=fim.month, ano=fim.year,
                tipo=DiaPago.TIPO_AJUSTE, versao="motor",
            ).values_list("horas_reflexo_dsr", "saldo_banco_horas").first()
            if ajuste is None:
                contagem["ajuste_ausente", *grupo] += 1
                continue
            for rubrica, valor in zip(("reflexo_dsr", "banco"), ajuste):
                chave = (rubrica, *grupo, faixa_horas(valor))
                contagem[chave] += 1
                horas[chave] += valor

    print(dict(contagem))
    print({chave: round(valor, 2) for chave, valor in horas.items()})
```

## 1. Mapa curto

**A tela existe em partes.** O cadastro executa desligamento e reativação, a folha mostra a saída pelo módulo próprio, e o drawer tem Espelho e Férias. A posição do desligamento também teve um piloto documentado, limitado a dias lavrados e ao diagnóstico da folha. Sua integração nesta cópia **[nao verificado]**. O ato único com dossiê histórico continua registrado como UI-RESCISAO. `app/templates/colaboradores/editar.html:268–288`, `app/templates/colaboradores/partials/painel_colaborador.html:27–46`, `app/folha/export.py:866–871`, `app/docs/BACKLOG.md:1181`, `app/docs/BACKLOG.md:1475`, `app/docs/RELATO.md:6579–6584`

| Porta do censo1 | Estado na cópia e mudança comprovada |
|---|---|
| **1. Espelho 21 → desligamento** | **EXISTE** PDF livre informacional, com duas pontas inclusivas. Avulso e entrada web do lote recusam fim hoje/futuro pelo mesmo juiz. No avulso, a diferença máxima entre datas é 180 dias. O gerador do lote aceita seleção explícita, e a seleção padrão do lote de folha é ativo/afastado/férias. Retorna caderno, pulados, quantidade, informacionais e sem movimento. **FALTA** livre com trilha e arquivo por emissão. **Esclarecimento:** o auditável parametriza mês/ano e resolve o corte da empresa. O bloqueio explícito nesse caminho é disputa aberta do colaborador, inclusive fora do intervalo. A exigência de competência fechada consta no plano, e a checagem correspondente nesse caminho **[nao verificado]**. `app/relatorios/services.py:13–46`, `app/relatorios/services.py:101–105`, `app/relatorios/services.py:165–264`, `app/relatorios/views.py:1065–1099`, `app/relatorios/views.py:1183–1188`, `app/relatorios/lote_espelho.py:205`, `app/relatorios/pdf_espelho.py:287`, `app/relatorios/pdf_espelho.py:602`, `app/relatorios/pdf_espelho.py:654–663`, `app/relatorios/pdf_espelho.py:1077–1116`, `app/relatorios/pdf_base.py:127–136`, `app/docs/RELATORIOS-PLANO.md:192–194` |
| **2. DiaPago** | **EXISTE.** Escrita por competência/versão, com dia e ajuste. Intervalo inclui início e fim, exclui ajuste e distingue sem lavratura de sem apuração. Precisão sobre o censo1: a escrita usa o mapa do dia da jornada quando disponível e recorre à data local da entrada quando o mapa falta. `app/ponto/models.py:293–345`, `app/ponto/services/dia_pago.py:109–135`, `app/ponto/services/dia_pago.py:348–360`, `app/ponto/services/dia_pago.py:405`, `app/ponto/services/dia_pago.py:538–563`, `app/ponto/services/dia_pago.py:592`, `app/ponto/services/dia_pago.py:671–673`, `app/ponto/services/dia_pago.py:752–856` |
| **3. Ausências** | **EXISTE.** `ausencias_vigentes` lê sobreposição em todos os status, com tipos declarados para fim aberto. Para folha, aprovadas. Para cobrança, exclui rejeitadas. A cobertura diária é inclusiva e desempata por maior `(data, pk)`. A linha do tempo de decisões tem outra porta, inclui rejeição e recebe o recorte no leitor. `app/ponto/turnos.py:2540–2568`, `app/ponto/turnos.py:2796–2829`, `app/ponto/turnos.py:3207–3230`, `app/ponto/services/dia_decidido.py:587`, `app/ponto/services/dia_decidido.py:624`, `app/ponto/services/atestados_lote.py:46–64` |
| **4. Férias** | **MUDOU.** Criação e edição agora compartilham `datas_do_ciclo`, com aniversário, fim menos um dia, concessão mais um ano e tratamento de 29/2. Cai a divergência `+364/+365` citada no backlog. Criação continua gerando ciclos até hoje. **EXISTE** saldo integral. **DIVERGE** o uso de status gravado versus juiz, a soma de `situacao_de_ferias` inclui positivos fora dos adquiridos anunciados, e o filtro do art. 130 continua no literal `falta`. **FALTA** fonte de proporcionais/avos, registrada no backlog. `app/ferias/views.py:139–179`, `app/ferias/services.py:256–270`, `app/ferias/services.py:805–851`, `app/ferias/test_hj090_volta3.py:316–322`, `app/ferias/models.py:144–150`, `app/ferias/models.py:219–220`, `app/ferias/situacao.py:138–169`, `app/ferias/situacao.py:300–302`, `app/inteligencia/views.py:13–21`, `app/docs/BACKLOG.md:1179`, `app/docs/BACKLOG.md:1475`, `app/docs/BACKLOG.md:1541` |
| **5. Informativo de rescisão** | **MUDOU. EXISTE** `esta_desligado`, saída vazia do retentor para desligado e `informativo_rescisao` com a mesma lista de dias em campo próprio. O retrato e `o_que_segura` já leem esse dado. O Raio-X tem derivação própria registrada em fila. `app/colaboradores/services/desligamento.py:23–32`, `app/folha/export.py:1197–1229`, `app/folha/fora_da_folha.py:296–305`, `app/folha/retrato_folha.py:1260–1289`, `app/docs/BACKLOG.md:737` |
| **6. Registro do PDF** | **EXISTE** escrita da trilha auditável, verificação por hash e download quando o PDF foi anexado a um followup. O registro guarda metadados e filtros JSON. **FALTA** nele o arquivo ou ponteiro e as duas datas normalizadas. Leitor de dossiê por colaborador/período **[nao verificado]**. `app/relatorios/models.py:30–65`, `app/relatorios/pdf_base.py:137–180`, `app/chamados/views.py:1186–1222`, `app/relatorios/services.py:70–96`, `app/relatorios/views.py:1020–1044` |
| **7. Motivo de exportação** | **EXISTE** `rescisao_modulo_proprio`, `status='fora'`, com encaminhamento ao Domínio. Raio-X, retrato, lista de fechamento, relatório e mensageria leem ou rotulam esse motivo. A ressalva fica fora da conta de bloqueio. `app/folha/export.py:866–871`, `app/folha/views.py:615`, `app/folha/retrato_folha.py:33`, `app/folha/retrato_folha.py:89`, `app/ponto/services/fechamento_lista.py:25–37`, `app/relatorios/views.py:1661–1669`, `mensageria/nucleo/ferramentas.py:1196`, `mensageria/nucleo/ferramentas.py:1246–1289` |
| **8. H12** | **EXISTE**, como no censo1. A data `<= fim da janela` dá saída pela rescisão, inclusive se for anterior ao início. O predicado independe do estado, então também pode ser verdadeiro para data agendada dentro da janela. Classificador e resumo usam o mesmo predicado. Prontidão usa outra pergunta, com data `< início` no caso de denominador zero. `app/folha/export.py:54–60`, `app/folha/export.py:782–788`, `app/folha/export.py:815`, `app/folha/export.py:1051–1087`, `app/ponto/services/fechamento_lista.py:103–109` |
| **9. Ato do desligamento** | **EXISTE** juiz, executor, cron e reativação. **DIVERGE** o aviso de bloqueio imediato do agendado. **MUDOU** a regeneração de células futuras geradas existentes, que passou a alcançar datas `> hoje` sem teto, com recorte a partir da borda alterada. O wrapper ainda oferece apenas vínculos ativos e recusa início posterior à demissão. A cascata ainda pula respostas sem veredito e pode deixar o chamado ancorado aberto. `app/colaboradores/services/desligamento.py:35–78`, `app/colaboradores/services/desligamento.py:102–215`, `app/colaboradores/views.py:1443–1476`, `app/templates/colaboradores/editar.html:272`, `app/colaboradores/services/vinculo.py:623–666`, `app/colaboradores/services/vinculo.py:857–867`, `app/ponto/services/cadastro_futuro.py:8–25`, `app/ponto/services/cadastro_futuro.py:44–59`, `app/config/crons.py:440` |
| **10. Ficha corrida e F2** | **EXISTEM** ficha de serviço, histórico de chamados e drawer com superfícies separadas. A ficha atual lê vínculo, trocas, horas e pendências. O histórico de chamados recorta por idade de criação dos eventos e das ausências. O histórico do drawer de ausências aponta o aquisitivo atual ou mais recente, sem recortar nele a lista. A quinta aba e a F2 de faltas e ausências decididas pelo aquisitivo estão documentadas. Dossiê unificado nessa aba **[nao verificado]**. `app/colaboradores/services/ficha.py:99–173`, `app/colaboradores/services/ficha.py:235–250`, `app/chamados/services/historico.py:120–145`, `app/ponto/views.py:2035–2064`, `app/docs/ADVERTENCIA-FICHA.md:65–88`, `app/docs/BACKLOG.md:1434` |

## 2. Cen

[truncado em 20000 de 53327 caracteres]

## 26/11 11:39 · sessao a21029860268 · - · - · origem=Ronald (tmux)

- **Porta 5 mudou.** A cópia implementa `esta_desligado`, a isenção de retenção e `informativo_rescisao` no mesmo laço. O retrato, `o_que_segura` e o contador leem as portas. O dia da demissão entra no informativo quando há célula acusando. Falta de célula ou veredito nesse dia fica fora. `app/colaboradores/services/desligamento.py:23`, `app/folha/export.py:1197`, `app/folha/export.py:1216`, `app/folha/export.py:1269`, `app/folha/fora_da_folha.py:159`, `app/folha/fora_da_folha.py:296`, `app/folha/retrato_folha.py:1260`.
- **Portas 7 e 8 confirmadas.** H12 considera `data_demissao <= fim da janela`, mesmo sem estado efetivado e sem limite inferior. O universo é o dos fechamentos ou a seleção recebida. A saída do TXT é `rescisao_modulo_proprio`, com ressalva para o DP. `app/folha/export.py:54`, `app/folha/export.py:782`, `app/folha/export.py:866`, `app/ponto/services/fechamento_lista.py:109`, `mensageria/nucleo/ferramentas.py:1196`.
- **Porta 9 mudou no alcance das células.** O complemento pega células geradas já existentes depois de hoje, sem teto, e aplica a borda do menor fim anterior e novo mais um dia. O desligamento ainda fecha só vínculos marcados ativos que já iniciaram até a demissão. A interface segue anunciando bloqueio imediato no agendamento. Reativar só aceita desligamento efetivado, limpa data e motivo e libera credencial. `app/ponto/services/cadastro_futuro.py:8`, `app/colaboradores/services/vinculo.py:654`, `app/colaboradores/services/vinculo.py:857`, `app/templates/colaboradores/editar.html:272`, `app/colaboradores/views.py:1443`, `app/colaboradores/views.py:1476`, `app/colaboradores/services/desligamento.py:235`.
- **Cascata ainda diverge dos cortes.** Mantém a disputa com resposta sem veredito e pode manter o chamado que a ancora. O chamado de revisão ainda grava data, motivo, ator e instante, sem lista de dia, pergunta e resposta. A regra universal de reabertura de desligado ainda falta no juiz. `app/colaboradores/services/desligamento.py:164`, `app/colaboradores/services/desligamento.py:182`, `app/colaboradores/services/desligamento.py:209`, `app/chamados/models.py:1384`, `app/chamados/models.py:1070`, `app/chamados/services/adesao.py:91`, `app/docs/BACKLOG.md:732`, `app/docs/BACKLOG.md:733`. O registro de revisão é destino do DP e fica fora de `vivos_de_desligado`. `app/docs/RELATO.md:3978`, `app/chamados/catalogo/natureza.py:18`, `app/chamados/catalogo/modulos.py:265`.
- **Porta 10 parcial.** O drawer tem quatro abas e a ficha de serviço lê vínculo, horas e pendências da janela corrente. A quinta aba e a F2 constam nos documentos. Leitor de dossiê PDF por colaborador e período [nao verificado]. `app/templates/colaboradores/partials/painel_colaborador.html:27`, `app/colaboradores/services/ficha.py:99`, `app/colaboradores/services/ficha.py:235`, `app/docs/ADVERTENCIA-FICHA.md:65`, `app/docs/BACKLOG.md:1434`.
- **RED exportado e férias.** Exportação preserva o TXT anterior e permite nova apuração pela L-113. A tranca viva é outra regra. Para férias, as datas inclusivas dão o gozo e o cancelado fica fora, enquanto ambos os endpoints de batida barram pelo estado gravado. `app/docs/LEIS.md:155`, `app/chamados/juizes.py:2220`, `app/chamados/juizes.py:2241`, `app/ferias/services.py:49`, `app/api/views.py:403`, `app/api/views_core.py:389`.

## 26/11 11:38 · sessao 5c29a238d4d6 · E1-PREVISAO-24H · leitura · origem=Code

**Triagem das seis falhas:** quatro **(c)**, de caracterizações do limite antigo, e duas **(b)**, causadas pelo mesmo fixture novo que cria `Ausencia` diretamente. Os casos `78 != 30` e `0 != 30` são de fixtures sem previsto da grade. Para a condição de pouso, a sonda classifica como `valido-julgado` o dia que passou pelo juiz da previsão e foi aceito. [app/ponto/motor_calculo_v2.py:3404] [app/ferias/tests.py:68] [app/ponto/tests/test_motor_comercial_dia_do_turno.py:61] [app/ponto/tests/test_comercial_he_marcos.py:26] [app/scripts/sonda_e1_comercial.py:71] [app/scripts/sonda_e1_comercial.py:169] [nao verificado]

### Quatro falhas do comercial

O trecho alterado é `MotorComercial.calcular_mes`, na linha 3409. Ele troca o limite pelos marcos por `_jornada_do_dia(dia)`. Nos três testes numéricos, a chamada omite `colaborador_id`. Nesse caminho, `_previsto_do_dia` devolve `None` e `_jornada_do_dia` usa `self.jornada_minutos`, inicializado com o campo do template ou com 720 min quando o motor é criado sem template. A troca da fonte é a autorizada pelo **E1-V5-RESPOSTAS-2611, item 1**, sob a **L-024** e a condição de frota. [app/ponto/motor_calculo_v2.py:3404] [app/ponto/motor_calculo_v2.py:2296] [app/ponto/motor_calculo_v2.py:1252] [app/ponto/motor_calculo_v2.py:1290] [app/ponto/motor_calculo_v2.py:810] [app/docs/LEIS.md:75] `E1-V5-RESPOSTAS-2611.txt:1` [nao verificado]

1. **`test_acima_da_tolerancia_conta_integral` — (c).** Em 06/07, o fixture passa um `TipoEscala` em memória, sem célula nem vínculo. O `colaborador_id=1` pertence à batida e a chamada ao motor omite esse argumento. `_jornada_do_dia` devolve **600 min** pelo campo `minutos_jornada`. O teste espera **528 min** pelos marcos de 07:00–17:00 com pausa de 72 min. Com **558 min** trabalhados, o teto antigo deixava **30 min** de HE50 e o teto atual deixa **0**. A docstring invoca **P7** e **CLT art. 58, § 1º**, e a mensagem exige a contagem integral acima da jornada **dos marcos**. O ramo da tolerância integral permanece na linha 3416. O item 1 do aval substitui a fonte do teto. **Dia válido com HE diferente para o PAREI? Não.** O fixture fica fora do universo julgado pela sonda. [app/ponto/tests/test_comercial_he_marcos.py:6] [app/ponto/tests/test_comercial_he_marcos.py:15] [app/ponto/tests/test_comercial_he_marcos.py:20] [app/ponto/tests/test_comercial_he_marcos.py:35] [app/ponto/tests/test_comercial_he_marcos.py:42] [app/ponto/tests/test_comercial_he_marcos.py:47] [app/ponto/motor_calculo_v2.py:3409] [app/ponto/motor_calculo_v2.py:3416] [app/escala/utils.py:473] [nao verificado]

2. **`test_comercial_extra_50` — (c).** Em 22/04, o fixture passa só batidas, com `tipo_escala=None`, sem célula nem vínculo. A linha 3409 recebe **720 min** de `_jornada_do_dia`. O limite antigo era **480 min**, a referência diária do comercial. A docstring descreve 8h de jornada e 1h extra, sem citar lei ou corte. A asserção exige apenas HE50 **maior que zero**. Com **540 min** trabalhados, o limite atual explica o `0.0`. O item 1 substitui o limite antigo e o próprio aval pede um teste com `tipo_base='comercial'` de verdade. [app/ponto/tests/test_motor.py:1026] [app/ponto/motor_calculo_v2.py:810] [app/ponto/motor_calculo_v2.py:3228] [app/ponto/motor_calculo_v2.py:3409] `E1-V5-RESPOSTAS-2611.txt:1` [nao verificado]

3. **`test_comercial_diurno_intocado` — (c).** Em 10/06, o fixture tem um `TE` falso, sem célula nem vínculo. A linha 3409 recebe **480 min** de `_jornada_do_dia`, o default de `TE.minutos_jornada`. O teste espera o teto de **528 min** pelos marcos de 08:00–17:48 com pausa de 60 min. Os **558 min** trabalhados dão **78 min** de HE50 com o teto atual e **30 min** com o antigo. A docstring do módulo invoca **F1 TURNO, sítio 2**, sobre atribuir a HE ao dia do turno. O agrupamento continua nas linhas 3342–3347. A asserção diurna fixa o número que dependia do teto pelos marcos, substituído pelo item 1 e pela L-024. **Dia válido com HE diferente para o PAREI? Não.** O fixture fica fora do universo julgado pela sonda. [app/ponto/tests/test_motor_comercial_dia_do_turno.py:1] [app/ponto/tests/test_motor_comercial_dia_do_turno.py:16] [app/ponto/tests/test_motor_comercial_dia_do_turno.py:29] [app/ponto/tests/test_motor_comercial_dia_do_turno.py:61] [app/ponto/tests/test_motor_comercial_dia_do_turno.py:80] [app/ponto/motor_calculo_v2.py:3342] [app/ponto/motor_calculo_v2.py:3409] [app/escala/utils.py:473] [nao verificado]

4. **`test_MORDE_o_limite_do_dia_pergunta_a_regra` — (c).** O teste inspeciona a fonte de `MotorComercial.calcular_mes` e exige a chamada literal `turno_cruza_meia_noite(_hi, _hf)`. A docstring cita **F1 TURNO, sítio 11**, e identifica a âncora como guarda estrutural do limite pelos marcos. A troca autorizada pelo item 1 e pela L-024 remove esse cálculo da linha 3404 em diante. O teste só lê a fonte, então célula, previsto devolvido e limite numérico são inaplicáveis. [app/ponto/tests/test_turno_meia_noite_comercial.py:1] [app/ponto/tests/test_turno_meia_noite_comercial.py:17] [app/ponto/motor_calculo_v2.py:3404] [app/docs/LEIS.md:75] [nao verificado]

### Duas falhas da porta de `Ausencia`

5. **`test_NINGUEM_cria_Ausencia_fora_da_porta` — (b).** O diff acrescenta `FaltaComPrevisaoInvalidaTest.test_MORDE_cinco_faltas_validas_e_uma_invalida_dao_30_dias` com chamadas diretas em **`ferias/tests.py:68` e `:75`**. A varredura por regex inclui esse arquivo e agrega os achados por arquivo e expressão, por isso acusa `Ausencia.objects.create` uma vez. A docstring e a mensagem invocam o contrato escrito **B5-F1/P13**, com a trilha **P14**. [app/ferias/tests.py:54] [app/ferias/tests.py:68] [app/ferias/tests.py:75] [app/ponto/tests/test_contract_porta_ausencia.py:1] [app/ponto/tests/test_contract_porta_ausencia.py:44] [app/ponto/tests/test_contract_porta_ausencia.py:70]

6. **`test_MORDE_ninguem_cria_Ausencia_fora_do_produtor` — (b).** A varredura AST acusa **as duas linhas**. A docstring do corte **A1 de 08/09** declara os fixtures fora do universo, mas `_e_teste` reconhece apenas diretórios `/tests/` e arquivos com prefixo `test_`. Assim, `ferias/tests.py` entra na varredura. A mensagem da linha 96 indica `criar_ausencia/garantir_ausencia`. [app/ferias/tests.py:68] [app/ferias/tests.py:75] [app/ponto/tests/test_contract_um_produtor_de_ausencia.py:1] [app/ponto/tests/test_contract_um_produtor_de_ausencia.py:13] [app/ponto/tests/test_contract_um_produtor_de_ausencia.py:38] [app/ponto/tests/test_contract_um_produtor_de_ausencia.py:83] [app/ponto/tests/test_contract_um_produtor_de_ausencia.py:96]

O fixture comum às duas catracas monta vínculo e células, inclusive a previsão inválida de 30/09 e a de 16/09. Ele verifica a contagem de faltas para o **art. 130 da CLT** com o recorte **L-124**, via `faltas_periodo`. `_jornada_do_dia` e o limite de HE são inaplicáveis a essas asserções. A forma permitida é `criar_ausencia(colaborador=colab, tipo='falta', data=dia, status='aprovada', registrado_por=colab.usuario)` para cada falta de dia inteiro. O teste conta registros, e a porta deixa os minutos a cargo da grade. Para fixture reexecutável, `garantir_ausencia` chama a mesma porta e retorna `(ausencia, criada)`. [app/ferias/tests.py:60] [app/escala/tests/test_e1_v4_juiz_unico.py:31] [app/escala/tests/test_e1_v4_juiz_unico.py:36] [app/ferias/models.py:52] [app/ferias/models.py:144] [app/escala/utils.py:486] [app/ponto/services/ausencia.py:262] [app/ponto/models.py:887] [app/ponto/services/ausencia.py:602] [nao verificado]

**Condição de pouso da comp. 11:** o comentário do motor registra zero dias válidos com HE diferente. A saída da sonda com o comparativo por rubrica, a contagem e a lista por dono está **[nao verificado]**. O anexo traz o código que gera essa saída e dispara o `PAREI`. [app/ponto/motor_calculo_v2.py:3407] [app/scripts/sonda_e1_comercial.py:180] [app/scripts/sonda_e1_comercial.py:221] [nao verificado]

## 26/11 11:38 · sessao 689178c6a3de · - · - · origem=Ronald (tmux)

- **Mudança confirmada em férias.** A criação e a edição passam por `datas_do_ciclo`, que usa aniversário menos um dia para o fim e mais um ano para o limite. A divergência `+364/+365` do censo e do BACKLOG ficou obsoleta. A criação ainda gera ciclos até hoje. Proporcionais e avos continuam registrados como lacuna. [Criação](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ferias/views.py:164), [Edição](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ferias/services.py:849), [Teste bissexto](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ferias/test_hj090_volta3.py:316), [BACKLOG](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/docs/BACKLOG.md:1179).
- **Espelho livre continua informacional.** Avulso e lote recusam fim hoje/futuro. Avulso limita a diferença entre datas a 180 dias. Renderização usa pontas inclusivas e a janela auditável usa o corte empresarial. A disputa aberta de qualquer data é o único bloqueio passado pelo auditável de competência. No caminho inspecionado, inexiste guarda explícita de competência fechada. O parecer permanece condição escrita do hash livre. `app/relatorios/views.py:1083,1091,1098,1183,1188`, `app/relatorios/services.py:211,217,235,250,260`, `app/relatorios/pdf_espelho.py:287,602,654,1087,1116`, `app/relatorios/pdf_base.py:127`, `app/docs/BACKLOG.md:1473`.
- **Registro e O58.** Único INSERT em `app/relatorios/pdf_base.py:152`, chamado por espelho `app/relatorios/pdf_espelho.py:1106`, histórico `app/relatorios/pdf_chamados.py:168`, férias `app/relatorios/services.py:338`, HE `app/ponto/views.py:887` e retrato `app/folha/views_retrato.py:443`. Mascarador atualiza nome da cópia de sombra, `bin/sombra_mascara.py:88`. Hash é do PDF canônico anterior ao rodapé com hash e há dois passos de renderização, `pdf_base.py:137,149,152,168,178`. Modelo guarda tipo, autor, instante, colaborador, filtros, quantidade, IP, commit e host, sem intervalo explícito e PDF. Verificador público consulta por hash ou prefixo, leitura em memória anexa o espelho ao followup e baixa-se por seu id. Lote grava caderno inteiro. Leitor individual por pessoa/intervalo de arquivo da emissão livre [nao verificado]. A parte da O58 livre de parecer é persistir cada emissão com proveniência, período e acesso ao arquivo original. Isso basta à abertura histórica, enquanto hash CLT livre depende do parecer. O59 manda consultar a emissão original nos dois leitores. `app/relatorios/models.py:30,43,50`, `app/chamados/views.py:1186,1211,1215`, `app/relatorios/services.py:70,88`, `app/relatorios/views.py:1020,1042`, `app/relatorios/lote_espelho.py:250`, `app/docs/BACKLOG.md:1473–1475`. Os 1.709/1.720 do anexo calculam 99,36%, discrepantes dos 99,8% no BACKLOG. `app/docs/BACKLOG.md:1473`.
- **DiaPago e ausências.** DiaPago usa dia da jornada pelo mapa do motor, com data local da entrada quando o mapa falta. Motor e oráculo podem coexistir. Escritores `lavrar` substitui por pessoa/competência/versão, `retratar_exportada` atualiza ou cria pelo gravado e `apurar_furo_da_exportada` toca só furo/carimbo. Leituras de período são inclusivas, só linhas dia, e separam falta de lavratura de falta de apuração. Afastamentos e ausências por vigência incluem sobreposição de bordas e todos os status. Para folha são aprovadas, cobrança exclui rejeitadas. Cobertura escolhe maior `(data, pk)`, histórico inclui rejeições e histórico em linha do tempo lê raízes uma vez por lançamento, com filtro aplicado depois para O30-F3. `app/ponto/services/dia_pago.py:109,135,359,405,538,592,671,752,833,855`, `app/ponto/services/fechamento.py:1224,1301`, `app/ponto/turnos.py:2540,2796,2817,3207`, `app/ponto/services/dia_decidido.py:587,624,651`, `app/ponto/services/atestados_lote.py:46,57`. O histórico de ausência do drawer continua marcando aquisitivo contendo hoje ou último iniciado e devolve toda a linha do tempo. F2 da ficha abre uma pergunta própria, decididas no aquisitivo. `app/ponto/views.py:2035,2042,2050,2060`, `app/docs/BACKLOG.md:1434`.
- **Banco.** `MotorComercial` soma semanalmente minutos trabalhados menos extras pagos e menos previsto proporcional, em horas com duas casas. Usa vínculo e, para extras, data civil de entrada. L-085 documenta a discrepância civil no DSR. Fechamento grava seu saldo, porta operacional pode restaurar o campo, DiaPago escreve o ajuste sem data ou o retrata da exportada. Totais de competência incluem ajustes, período livre exclui ajustes mas PDF informacional pode exibir valor recalculado. Esses números não são saldo de acerto parcial. O código manda verbas rescisórias ao módulo próprio do Domínio e deixa o reflexo mensal do DSR à folha. `app/ponto/motor_calculo_v2.py:3445,3464,3475,3485`, `app/ponto/services/fechamento.py:1128,1382,1404`, `app/ponto/services/dia_pago.py:348,507,542,550,755`, `app/relatorios/pdf_espelho.py:531,815`, `app/relatorios/cartao_pela_celula.py:321`, `app/folha/export.py:439,453,856`, `app/docs/LEIS.md:117,178`.
- **Saldo integral de férias.** Direito menos concedido menos vendido. Saldo não positivo prevalece no status e o limite vence só antes da referência. `situacao_de_ferias` soma todos os saldos positivos, apesar do texto dizer ciclos adquiridos. GET de saldo pode escrever via sincronização. Para sonda, o modelo e `status_de_periodo` puro bastam. Férias em gozo por datas inclusivas excluem cancelamento, independentemente do status `concluido`. `app/ferias/models.py:98,144,152,219`, `app/ferias/situacao.py:146,163,300`, `app/ferias/views.py:107,121`, `app/ferias/services.py:49,367`.
- **Interseções confirmadas no código e no BACKLOG.** O58 compartilha trilha e emissão em `app/relatorios/{models.py:14,pdf_base.py:92,pdf_espelho.py:1077,services.py:165,views.py:1050,lote_espelho.py:205}`. O30-F3 c2 compartilha a linha do tempo `app/ponto/services/dia_decidido.py:624` e fronteira `app/ponto/views.py:2035`, conforme `app/docs/RELATO.md:4` e `app/docs/BACKLOG.md:2353`. PAR compartilha turnos e integração com chamados, `app/ponto/management/commands/detectar_par_relampago.py:82,285`, `app/ponto/turnos.py:1850,2185,2207`, `app/chamados/reconciliador.py:186`, `app/docs/BACKLOG.md:2337`. DIA-DA-BATIDA compartilha `app/ponto/turnos.py:1850,2992`, `app/relatorios/pdf_espelho.py:212,443,563`, `app/docs/RELATO.md:3`, `app/docs/BACKLOG.md:1858`. ABONO catálogo cruza `app/ponto/services/dia_pago.py:364`, `app/escala/utils.py:1213`, `app/ponto/services/fechamento.py:700`, `app/docs/BACKLOG.md:2138`. ABONO motor cruza `app/ponto/services/efeito_ausencia.py:28,53` e `app/relatorios/pdf_espelho.py:333,419,433`, conforme `app/docs/BACKLOG.md:2230`, `app/docs/CORTES.md:558`. E1 previsão cruza previsto e lavra, `app/ponto/services/fechamento.py:1236`, `app/ponto/services/dia_pago.py:135`, `app/docs/BACKLOG.md:2006,2009`. E1 do fio é outra obra e cruza `app/chamados/services/fio.py` e modelos, conforme `app/docs/BACKLOG.md:2246`. Arquivos de raias em efetiva edição [nao verificado].

## 26/11 11:38 · sessao 05ce8833791b · - · - · origem=Ronald (tmux)

**1. Férias proporcionais.** Sobrevive a pergunta sobre avos, fração de mês e efeito do aviso. O saldo existente usa dias integrais de direito menos concedidos e vendidos. As faixas do art. 130 usam registros de faltas aprovadas no aquisitivo. Art. 134 e o corte de 14 dias regem fracionamento do gozo, art. 137 rege vencimento e art. 135 rege aviso de férias. Nenhum desses sítios dá a fórmula rescisória. A ausência de avos também está registrada no contrato anterior. `app/ferias/models.py:52`, `app/ferias/models.py:144`, `app/ferias/models.py:219`, `app/ferias/situacao.py:192`, `app/ferias/situacao.py:220`, `app/ferias/situacao.py:245`, `app/docs/CORTES.md:520`, `app/docs/BACKLOG.md:1179`, `app/docs/RELATO.md:6582`.

**2. Aviso prévio.** Sobrevive a distinção trabalhado/indenizado e a projeção sobre vínculo e direitos. Cai a parte sobre agendamento, que a referência à Lei 12.506 e a porta resolvem: amanhã até hoje + 90 inclusive agenda, mantém estado e acesso até a execução na data. A referência menciona 30 + 3 dias por ano de serviço como lastro do teto, mas o código da porta valida o teto, sem calcular o prazo individual. Aviso em home office tem espécie de ausência abonada e não representa o aviso indenizado. `app/colaboradores/services/desligamento.py:17`, `app/colaboradores/services/desligamento.py:52`, `app/colaboradores/services/desligamento.py:75`, `app/colaboradores/services/desligamento.py:102`, `app/ponto/catalogo/ausencias.py:104`, `SERIE_T.md:4126`.

**3. DSR e banco em competência parcial.** Sobrevive o cálculo/destino específico do ajuste no acerto. Cai a dúvida sobre ratear o ajuste pela UI: a autoridade de intervalo exclui a linha de ajuste e proíbe rateá-la. Lei 605/1949 e Súmula 172 respondem reflexo semanal do DSR e o TXT mensal deixa o cálculo para o Domínio. Art. 59 §2 responde ao saldo semanal observado pelo motor, com extras já pagos excluídos e proporcionalidade de dias previstos na janela. H12 determina módulo de rescisão do Domínio, inclusive se demissão no último dia. Regra efetiva de reflexo e de pagamento/baixa dos créditos e débitos no acerto segue [nao verificado]. `app/ponto/services/dia_pago.py:752`, `app/ponto/motor_calculo_v2.py:2581`, `app/ponto/motor_calculo_v2.py:2645`, `app/ponto/motor_calculo_v2.py:3445`, `app/folha/export.py:453`, `app/folha/export.py:54`, `app/folha/export.py:856`.

**Candidatas transversais.** L-020/024/061, CLAUDE §4/4a, célula soberana e testemunha lê resolvem fonte da escala e o consumo da decisão, sem estabelecer verba. L-021 impede fim anterior ao início do vínculo e rege a escrita de vigência. L-034 e §6b respondem qual regime vale pelo cadastro, sem regra de acerto. L-039/088/089 respondem a distinção falta decidida/furo e efeito nos dias previstos. L-026/090 definem a janela pelo cadastro. L-037/038 e O58/O59 definem período e registro do documento. L-085 define o dia do turno, inclusive para DSR. L-095 veda recalcular dinheiro na tela. L-052/092/113 e DINHEIRO 2 disciplinam o gravado, com guarda atual de 09/2026 nas empresas que exportaram. L-050/051/130, DESLIGAR-LIQUIDA-TUDO e NAO-REABRE respondem vitalidade e atos de chamado. COMPLEMENTO-DESLIGAR-1311 responde escopo e ordem da obra. LEI-DESLIGADO-NAO-TRAVA, juiz único e escritor único respondem estado, retenção e autoridade de escrita. §7b e L-117 instruem a aplicar lei existente ou formular a dúvida residual com números. Nenhum desses recortes resolve a parte residual das três perguntas. `app/docs/LEIS.md:71`, `app/docs/LEIS.md:72`, `app/docs/LEIS.md:75`, `app/docs/LEIS.md:77`, `app/docs/LEIS.md:82`, `app/docs/LEIS.md:85`, `app/docs/LEIS.md:87`, `app/docs/LEIS.md:91`, `app/docs/LEIS.md:93`, `app/docs/LEIS.md:95`, `app/docs/LEIS.md:113`, `app/docs/LEIS.md:117`, `app/docs/LEIS.md:120`, `app/docs/LEIS.md:123`, `app/docs/LEIS.md:125`, `app/docs/LEIS.md:138`, `app/docs/LEIS.md:151`, `app/docs/LEIS.md:155`, `CLAUDE.md:20`, `CLAUDE.md:32`, `CLAUDE.md:142`, `CLAUDE.md:170`, `CLAUDE.md:210`, `CLAUDE.md:275`, `CLAUDE.md:428`, `app/docs/CORTES.md:80`, `app/docs/CORTES.md:177`, `app/docs/CORTES.md:485`, `app/docs/BACKLOG.md:731`, `app/docs/BACKLOG.md:732`, `app/docs/BACKLOG.md:733`, `app/docs/BACKLOG.md:1473`, `app/docs/BACKLOG.md:1474`, `app/docs/BACKLOG.md:1475`.

**Leis de tela.** O chamado `desligamento_revisao` é destino de auditoria e fica expressamente fora do contador `vivos_de_desligado`, apesar de nascer registrado. A cascata atual pula disputa com resposta sem veredito, e as duas guardas copiadas de reabertura seguem presentes. O informativo ao DP existe na cópia, lê o mesmo laço da retenção e `o_que_segura` devolve uma chave à parte. Padrão principal do ato é **Single Writer Principle**. Para a leitura, **CQRS com projeções reconstruíveis** e **Single Source of Truth**, com **Bitemporal data** na interface com O50. `app/docs/RELATO.md:3978`, `app/colaboradores/services/desligamento.py:164`, `app/chamados/models.py:1070`, `app/chamados/models.py:1384`, `app/chamados/services/adesao.py:90`, `app/folha/export.py:1197`, `app/folha/export.py:1216`, `app/folha/fora_da_folha.py:297`, `app/docs/GLOSSARIO.md:11`, `app/docs/GLOSSARIO.md:12`, `app/docs/GLOSSARIO.md:25`, `app/docs/GLOSSARIO.md:27`.

## 26/11 11:33 · sessao fde17af68971 · O30-F3-CONSTRUIR · verificador · origem=Code

## 1. PROCEDE — reparos pedidos na origem

| Achado da volta 5 | Veredito |
|---|---|
| **C6-1 — servir, aprovar, rejeitar e listar resolviam a raiz por conta própria** | **CURADO nas quatro vias.** Todas chamam `raiz_documento`. O drawer e as quatro vias recusam `A → B → C` com 404 no teste sem banco. A lista entrega a raiz pronta ao template. `app/ponto/views.py:2103`, `app/ponto/views.py:2151`, `app/ponto/views.py:2347`, `app/ponto/views.py:2787`, `app/ponto/views.py:2837`, `app/ponto/views.py:2873`, `app/ponto/views.py:2901`, `app/templates/ponto/ausencias.html:168`, `app/ponto/tests/test_atestado_f3_contrato.py:399` |
| **C6-2 — duas regras para substituir o arquivo compartilhado** | **CURADO na guarda.** `_validar_substituicao_na_referencia` barra a troca de arquivo existente se a raiz ou alguma filha estiver aprovada ou rejeitada. INSERT, anexo e reabertura chamam essa guarda. A regressão no fluxo de reenvio está no item 4. `app/ponto/services/ausencia.py:327`, `app/ponto/services/ausencia.py:678`, `app/ponto/services/ausencia.py:1190`, `app/ponto/services/ausencia.py:1486` |
| **C6-3 — prévia calculava o fim por conta própria** | **CURADO no comando.** Dry-run e aplicação usam `derivar_data_fim`. A fórmula residual em `Ausencia.save` permanece no ramo de fim nulo, conforme o recorte do brief. `app/ponto/management/commands/curar_ausencia_sem_fim.py:37`, `app/ponto/services/ausencia.py:1939`, `app/ponto/models.py:926` |

## 2. PROCEDE — ainda há leitores que tratam o arquivo da linha como o arquivo do lançamento

**NAO PROCEDE para outro resolvedor explícito da FK.** Na varredura da cópia, a escolha entre a própria linha e a referência imediata ficou no serviço. Comando de rejulgamento, histórico, formulário de atestado e lote CSV/PDF/ZIP usam essa porta. O relatório `atestados_acumulados` conta dias e o export da folha usa intervalo e minutos. `app/ponto/services/ausencia.py:283`, `app/ponto/management/commands/rejulgar_status_por_tipo.py:28`, `app/ponto/management/commands/rejulgar_status_por_tipo.py:38`, `app/ponto/services/dia_decidido.py:663`, `app/ponto/views_atestado.py:30`, `app/ponto/services/atestados_lote.py:107`, `app/ponto/services/atestados_lote.py:119`, `app/ponto/services/atestados_lote.py:178`, `app/ponto/services/atestados_lote.py:255`, `app/relatorios/views.py:291`, `app/folha/export.py:117`

**PROCEDE para a leitura por omissão da raiz.**

- **API do app** oferece “Anexar comprovante” quando a filha aberta tem `documento` vazio, mesmo se a raiz já tem arquivo. **Vigia** também conta documento ausente pela coluna da própria linha, inclusive no contador separado de férias. `app/api/views.py:1053`, `app/api/views.py:1061`, `app/ponto/services/vigia_ausencia.py:112`, `app/ponto/services/vigia_ausencia.py:214`
- **Cartão/espelho** pode rotular uma filha rejeitada como “sem anexo”. O ramo pendente também lê o arquivo local, mas esse parâmetro só entra no texto da rejeição. **Chamado de ausência** monta título, descrição e `tem_documento` pela linha, embora seu gate já consulte a raiz. **Troca de plantão** verifica e registra o arquivo de cada linha. Chamador produtivo da troca **[nao verificado]**. `app/ponto/services/dia_decidido.py:252`, `app/ponto/services/dia_decidido.py:473`, `app/ponto/services/dia_decidido.py:542`, `app/ponto/services/espelho.py:661`, `app/ponto/services/ausencia.py:932`, `app/ponto/services/ausencia.py:949`, `app/ponto/services/ausencia.py:958`, `app/ponto/portas/celula.py:726`, `app/ponto/portas/celula.py:746`
- **Admin** conserva os campos da própria linha em modo leitura. O **backfill histórico 0057** também leu a coluna local para definir o estado inicial. Na porta atual, o teste de reenvio ainda aceita `raiz.documento or ausencia.documento` como sinal de arquivo existente. `app/ponto/admin.py:38`, `app/core/admin.py:23`, `app/ponto/migrations/0057_a4_backfill_pendente.py:35`, `app/ponto/services/ausencia.py:1187`

**Escritores.** A FK entra em `criar_ausencia` e o `SET_NULL` a limpa na exclusão da raiz. O arquivo entra na criação, em `anexar_documento` ou em `_reabrir`. Os seis dados do atestado entram no INSERT ou pelo UPDATE de `_gravar_dados_atestado`. As escritas diretas de FK fora da porta encontradas são fixtures. `app/ponto/services/ausencia.py:345`, `app/ponto/services/ausencia.py:590`, `app/ponto/services/ausencia.py:671`, `app/ponto/services/ausencia.py:1198`, `app/ponto/services/ausencia.py:1491`, `app/ponto/models.py:815`, `app/ponto/tests/test_atestado_f3.py:81`, `app/ponto/tests/test_atestado_f3.py:110`

## 3. PROCEDE — segundo juiz, cobertura do selo e resíduos

| Classe | Veredito |
|---|---|
| Segundo juiz registrado | **PROCEDE.** A guarda nova define `decididas = ('aprovada', 'rejeitada')` e a porta consulta `ABERTAS` logo depois. A tupla reproduz o complemento para os quatro estados atuais, mas fica independente do juiz registrado para aberta/decidida. `app/ponto/services/ausencia.py:331`, `app/ponto/services/ausencia.py:1191`, `app/ponto/models.py:690`, `app/core/juizes.py:264`, `app/docs/JUIZES.md:58` |
| Segundo escritor | **NAO PROCEDE** no censo estático de produção, que retornou zero escritas atribuídas a `Ausencia` fora do serviço e do modelo. `app/core/censo_escritas.py:458`, `app/core/censo_escritas.py:479`, `app/ponto/admin.py:38` |
| Relógio fora da borda | **NAO PROCEDE no delta. PROCEDE como resíduo.** A cura ainda pode alcançar o relógio do modelo, `auto_now`, `_acordar_projecao` e o signal do cartório. Concordo com o encaminhamento à fila indicado no brief. `app/ponto/models.py:838`, `app/ponto/models.py:896`, `app/ponto/services/ausencia.py:191`, `app/ponto/services/ausencia.py:1958`, `app/ponto/signals.py:122`, `app/ponto/signals.py:143` |
| Derivação paralela | **PROCEDE como resíduo.** Saiu a fórmula da prévia. O modelo ainda calcula o fim quando o recebe nulo, enquanto a aplicação da cura entrega o fim já calculado. `app/ponto/management/commands/curar_ausencia_sem_fim.py:37`, `app/ponto/services/ausencia.py:1939`, `app/ponto/services/ausencia.py:1957`, `app/ponto/models.py:926` |
| Ordem de trava | **NAO PROCEDE** inversão demonstrada entre as vias do grupo, que começam pela raiz. **PROCEDE** a janela preexistente entre consultar K6 e salvar, pois o registro do TXT usa outras linhas. Concordo com a fila indicada no brief. `app/ponto/services/ausencia.py:309`, `app/ponto/services/ausencia.py:1141`, `app/ponto/services/ausencia.py:1937`, `app/ponto/services/ausencia.py:1951`, `app/ponto/services/ausencia.py:1958`, `app/folha/views.py:299`, `app/folha/models.py:174` |
| Campo com dois significados | **PROCEDE como resíduo.** O checkbox lê `data_fim=NULL` com `dias_corridos>1` como indeterminado. Para um tipo que exige fim, a cura lê a mesma combinação como fim faltante e a cobertura lê só o primeiro dia até a correção. `app/templates/ponto/partials/painel_ausencia.html:194`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:28`, `app/ponto/turnos.py:2851` |
| Pouso antes do pré-requisito | **PROCEDE como pendência.** Uma cadeia no recorte derruba a lista inteira com 404. O POST de `anexar_atestado` resolve a raiz antes do `suppress` e deixa a recusa de cadeia subir. Estoque de cadeias, migration aplicada e suíte inteira **[nao verificado]**. `app/ponto/views.py:2347`, `app/ponto/views_atestado.py:29`, `app/ponto/views_atestado.py:42`, `app/ponto/migrations/0079_ausencia_atestado_dados.py:8`, `CLAUDE.md:392` |

**Mutantes — PROCEDE.** Reexecutei em memória os 14 mutantes avulsos do anexo. Todos falharam por asserção e os 55 testes sem banco passaram. `d2_so_linha_pedida` tem nome impreciso. Ele troca toda a consulta a irmãs por `False`, e a asserção falha porque o POST retorna 200 em vez de 409. `c6_insert_pula_guarda` demonstra a recusa **antes** do INSERT, com a segunda chamada à guarda simulada. Ele não demonstra que tirar a primeira guarda permitiria trocar o arquivo no fluxo completo. `app/ponto/tests/mutantes_atestado_f3.py:235`, `app/ponto/tests/test_atestado_f3_contrato.py:1043`, `app/ponto/tests/mutantes_atestado_f3.py:298`, `app/ponto/tests/test_atestado_f3_contrato.py:617`, `app/ponto/services/ausencia.py:678`, `app/ponto/services/ausencia.py:686`

O selo pega o `or` literal. Reintroduzi a escolha da raiz em `servir_documento_ausencia` com ternário, mantendo a validação anterior. O RED da cadeia e o selo AST passaram. Os seis blobs finais batem com o diff colado. A identidade do commit e os três REDs contra o commit c5 **[nao verificado]** na cópia sem `.git`. `app/ponto/views.py:2151`, `app/ponto/tests/test_atestado_f3_contrato.py:399`, `app/ponto/tests/test_atestado_f3_contrato.py:469`, `app/ponto/tests/mutantes_atestado_f3.py:392`

## 4. PROCEDE — o reenvio com documento corrigido pode ser barrado

- **Rejeitada com arquivo existente.** A própria notificação orienta “Reenvie com documento correto”. App e PWA passam o arquivo novo a `criar_ausencia`, que encontra a rejeitada e chama `_reabrir`. A guarda roda antes da transição e recusa quando a raiz é rejeitada. Também recusa quando a raiz está aberta e a própria filha a reabrir é a única decidida, pois a consulta de irmãs inclui essa filha. A reversão manual do DP continua sem arquivo. `app/ponto/views.py:2889`, `app/api/views.py:1824`, `app/ponto/views.py:2715`, `app/ponto/services/ausencia.py:503`, `app/ponto/services/ausencia.py:544`, `app/ponto/services/ausencia.py:1141`, `app/ponto/services/ausencia.py:1481`, `app/ponto/views.py:2763`
- **Upload próprio pelo app.** No ensaio sem banco, filha aberta de raiz rejeitada recebeu 200 para o primeiro arquivo e 409 quando a raiz já tinha arquivo. O 409 aplica a proteção pedida, mas o app ainda oferece a ação olhando só a filha. O drawer também oferece “Substituir anexo” quando a guarda recusará a troca. `app/api/views.py:1061`, `app/api/views.py:3129`, `app/ponto/services/ausencia.py:327`, `app/templates/ponto/partials/painel_ausencia.html:89`
- **Aprovada reenviada pelo POST de criação — NAO PROCEDE bloqueio novo.** Com arquivo existente, `reenvio=True` descarta o upload antes da guarda e devolve `ja_recebido`. Sem arquivo, essa via ainda permite a primeira entrega. O endpoint de upload próprio já recusava a primeira entrega em aprovada. `app/ponto/services/ausencia.py:493`, `app/ponto/services/ausencia.py:1187`, `app/api/views.py:1852`, `app/ponto/tests/test_anexar_ausencia_colab.py:74`
- **INSERT com referência — PROCEDE a recusa no serviço.** Se a filha nasce aprovada junto com um upload e a raiz aberta já tem arquivo, a primeira guarda pode passar. Depois do INSERT, `anexar_documento` relê as filhas, encontra a recém-criada como decidida e desfaz o ato. Chamador produtivo que passa `documento_referencia` nesse caminho **[nao verificado]**. `app/ponto/services/ausencia.py:590`, `app/ponto/services/ausencia.py:667`, `app/ponto/services/ausencia.py:686`, `app/ponto/services/ausencia.py:1141`, `app/ponto/services/ausencia.py:1190`

## 5. NAO PROCEDE — `except`, `suppress`, fallback ou “por enquanto” novo

O diff acrescenta duas traduções para 404 em handlers existentes e um teste que exige `Http404`. A escolha da própria linha quando a FK é nula foi extraída da regra anterior da porta. O `suppress(DocumentoAprovado)` do formulário é anterior, mas agora devolve 409 para mais casos porque a guarda passou a usar essa exceção também para raiz rejeitada e filha decidida. `app/ponto/views.py:2837`, `app/ponto/views.py:2901`, `app/ponto/tests/test_atestado_f3_contrato.py:454`, `app/ponto/services/ausencia.py:283`, `app/ponto/views_atestado.py:42`, `app/ponto/services/ausencia.py:333`

## 6. NAO PROCEDE — dinheiro no delta c6

**Não**, nos caminhos produtivos localizados. A alteração do comando muda a prévia, e o `--apply` continua chamando a mesma porta de cura. O fechamento continua lendo intervalo e minutos pelo mesmo rateio. O efeito numérico da **c5** permanece quando a cura admite uma falta aprovada sem fim e com minutos zerados. Quantidade de linhas, valores gravados e efeito em reais ou no TXT de produção **[nao verificado]**. `app/ponto/management/commands/curar_ausencia_sem_fim.py:37`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:46`, `app/ponto/services/ausencia.py:1957`, `app/ponto/models.py:901`, `app/folha/export.py:117`, `app/ponto/services/fechamento.py:893`

**REPROVADO.** O reenvio que a notificação orienta pode ser barrado antes da reabertura. O app e o vigia ainda leem o arquivo da filha como se fosse o do lançamento. A guarda nova replica a classificação de decidida e o selo deixa passar um resolvedor paralelo escrito com ternário. `app/ponto/views.py:2892`, `app/ponto/services/ausencia.py:1486`, `app/api/views.py:1061`, `app/ponto/services/vigia_ausencia.py:117`, `app/ponto/services/ausencia.py:331`, `app/core/juizes.py:264`, `app/ponto/tests/test_atestado_f3_contrato.py:482`

## 26/11 11:26 · sessao c9357fee11e2 · DIA-DA-BATIDA-JUIZ-UNICO · leitura · origem=Code

- A sonda chama `ponto.turnos.turnos_do_colab`, a entrada pública usada pelo esmeril do espelho. Ela delega ao pareador que aplica o pad por segmento de vínculo. [app/ponto/services/esmeril_espelho.py:102](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/services/esmeril_espelho.py:102) · [app/ponto/turnos.py:1850](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/turnos.py:1850) · [app/ponto/turnos.py:1980](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/turnos.py:1980)
- O universo vem de `batidas_apuraveis` pela data civil. A duplicidade é medida dentro da lista de turnos de cada competência e inclui PKs que o pareador trouxe do pad. O total deduplica PKs entre competências. [app/ponto/turnos.py:2921](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/turnos.py:2921) · [app/ponto/turnos.py:1997](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/turnos.py:1997)
- A classe mista indica que a PK aparece em turno incompleto e em par fechado. A sonda consulta `vinculo_do_dia` pela `data_turno` com a mesma carga do pareador. `sem_vinculo` indica que falta pelo menos um vínculo e que a sonda encontrou menos de dois vínculos distintos. [app/ponto/turnos.py:312](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/turnos.py:312) · [app/ponto/turnos.py:1889](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/turnos.py:1889) · [app/ponto/turnos.py:1965](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/turnos.py:1965)
- Erros que saem da função pública fazem a sonda falhar. A função real já captura internamente erros de `periodos_do_dia`. Execução e números na sombra [nao verificado]. [app/ponto/turnos.py:1632](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/turnos.py:1632)

```python
from collections import Counter, defaultdict
from datetime import datetime, time, timedelta

from django.db import connection, transaction
from django.db.models import Count
from django.utils import timezone

from escala.alimentacao import celulas_do_periodo, escalas_do_periodo, vinculo_do_dia
from ponto.janelas import periodo_apuracao
from ponto.turnos import batidas_apuraveis, janela_com_pad, turnos_do_colab


CLASSES = ("(i)_orfa", "(ii)_par_fechado", "(i)+(ii)_mista")
VINCULOS = ("igual", "diferente", "sem_vinculo")


def medir_competencia(mes, exemplos):
    ini, fim = periodo_apuracao(mes, 2026, dia_corte=21)
    inicio = timezone.make_aware(datetime.combine(ini, time.min))
    limite = timezone.make_aware(datetime.combine(fim + timedelta(days=1), time.min))
    universo = dict(
        batidas_apuraveis(None, ini=inicio)
        .filter(timestamp__lt=limite)
        .values_list("colaborador_id")
        .annotate(n=Count("pk"))
        .order_by("colaborador_id")
    )
    pad_ini, pad_fim = janela_com_pad(ini, fim)
    afetados = set()
    pks = set()
    contagem = Counter()

    for cid in universo:
        por_pk = defaultdict(list)
        for turno in turnos_do_colab(cid, ini, fim):
            for pk, batida in {b.pk: b for b in turno.batidas}.items():
                por_pk[pk].append((turno, batida.timestamp))

        repetidas = {pk: itens for pk, itens in por_pk.items() if len(itens) > 1}
        if not repetidas:
            continue

        afetados.add(cid)
        celulas = celulas_do_periodo([cid], pad_ini, pad_fim)
        escalas = escalas_do_periodo(
            cid, pad_ini, fim, mais=[c.escala_geradora_id for c in celulas.values()]
        )
        escalas.sort(key=lambda e: e.data_inicio)
        dias = {t.data_turno for itens in repetidas.values() for t, _ in itens}
        dono_por_dia = {
            dia: getattr(vinculo_do_dia(cid, dia, escalas, celulas=celulas), "pk", None)
            for dia in dias
        }

        for pk, itens in sorted(repetidas.items()):
            pks.add(pk)
            fechados = sum(t.entrada is not None and t.saida is not None for t, _ in itens)
            if fechados == 0:
                classe = "(i)_orfa"
            elif fechados == len(itens):
                classe = "(ii)_par_fechado"
            else:
                classe = "(i)+(ii)_mista"

            donos = {dono_por_dia[t.data_turno] for t, _ in itens}
            if len(donos - {None}) > 1:
                vinculo = "diferente"
            elif None in donos:
                vinculo = "sem_vinculo"
            else:
                vinculo = "igual"
            contagem[classe, vinculo] += 1

            if len(exemplos) < 15 and pk not in exemplos:
                instante = timezone.localtime(itens[0][1]).isoformat()
                dias_turno = ",".join(sorted(t.data_turno.isoformat() for t, _ in itens))
                exemplos[pk] = f"col{cid:03d} {pk} {instante} turnos=[{dias_turno}]"

    total_batidas = sum(universo.values())
    print(
        f"competencia={mes:02d}/2026 janela={ini}..{fim} "
        f"universo_colabs={len(universo)} universo_batidas={total_batidas} "
        f"colabs_afetados={len(afetados)} pks_multiplas={len(pks)}"
    )
    for classe in CLASSES:
        valores = " ".join(f"{v}={contagem[classe, v]}" for v in VINCULOS)
        print(f"  classe={classe} {valores}")

    return set(universo), total_batidas, afetados, pks


def sondar():
    exemplos = {}
    with transaction.atomic(), connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
        cursor.execute("SELECT current_database(), current_schema()")
        if cursor.fetchone() != ("sombra", "juliani"):
            raise RuntimeError("A sonda exige o banco sombra e o schema juliani.")

        universo_10, batidas_10, afetados_10, pks_10 = medir_competencia(10, exemplos)
        universo_11, batidas_11, afetados_11, pks_11 = medir_competencia(11, exemplos)

    print(
        f"total universo_colabs={len(universo_10 | universo_11)} "
        f"universo_batidas={batidas_10 + batidas_11} "
        f"colabs_afetados={len(afetados_10 | afetados_11)} "
        f"pks_multiplas={len(pks_10 | pks_11)}"
    )
    for exemplo in exemplos.values():
        print(exemplo)
    print("SONDA_FIM ok")


sondar()
```

## 26/11 11:22 · sessao 2c7d729085fe · R1-SEM-DONO-2611 · leitura · origem=Code

**Seis chaves auditadas, zero chaves com zeragem demonstrada por uma obra.** A sonda mostra os dois lados do delta nas seis chaves. O vínculo com o CSV dos 38 dias do R1 e a atribuição dos outros dias ficam **[nao verificado]**. A numeração abaixo começa na primeira linha do anexo `sonda.out`, incluindo a linha em branco. `sonda.out:1,3,30,50,64,87,105,135`

## 1 e 2. Mecanismo demonstrado e obra dona

Nas seis chaves, `selecionado` aponta para `minutos_realizados`. O espelho publica o total do juiz de turnos, e o E6 recebe só os instantes e soma pares por posição na data da primeira batida de cada trecho. O R1 deixa de listar diferenças de até 10 min, mesmo quando o delta aritmético é diferente de zero. `sonda.out:30,50,64,87,105,135` · [espelho.py:689](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/services/espelho.py:689) · [espelho.py:801](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/services/espelho.py:801) · [e6_oraculo.py:149](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/management/commands/e6_oraculo.py:149) · [e6_oraculo.py:362](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/management/commands/e6_oraculo.py:362) · [nucleo.py:235](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/calculador/nucleo.py:235)

| Chave | O que a sonda mostra | Obra dona e sítio da divergência |
|---|---|---|
| **col114 15/11** | **C, pausa posicional no E6.** `pares` deixa um vão de 59,54 min entre 15:05 e 16:05. As duas batidas internas têm tipos `E,S`, `papel_ata=null` e `intra_dur=true`. O juiz devolve `pausas=[]`, `fim_contado=18:17`, `janela_descontada=0` e 499 min. `delta_arredondado=-59`. `sonda.out:26,30,31` | **sem dono.** `_pares_marcados` exige autoridade da ata ou a sequência gravada `S→E` para esse par interno. O realizado fica com o vão inteiro. A obra de intervalo pré-assinalado muda o motor, e a ch​ave tem `intervalo_declarado=[null,null]`. [turnos.py:875](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/turnos.py:875) · [turnos.py:756](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/turnos.py:756) · [BACKLOG.md:2554](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/docs/BACKLOG.md:2554) · `sonda.out:30` |
| **col130 29/10** | **A + B + C.** O E6 soma dois `pares` em 29/10. O juiz, também em 29/10, tem `fim_contado=01:31` e desconta 60 min de 395. A batida de 07:06, que fecha o segundo par do E6, entra no juiz em 30/10 como `E`, `aberto=true` e vale 0. Repor os 60 deixa **+276,91 min**. `sonda.out:48,50,51,53` | **sem dono.** A diferença nasce do pareamento por posição do E6 contra o fechamento do juiz e a janela do realizado. O backlog cita **30/10** para a obra da saída de madrugada. Nessa foto, mover sozinho o turno aberto de 07:06 para 29/10 soma 0. O fechamento desse turno pela obra está **[nao verificado]**. [nucleo.py:245](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/calculador/nucleo.py:245) · [turnos.py:733](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/turnos.py:733) · [BACKLOG.md:2093](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/docs/BACKLOG.md:2093) · `sonda.out:50–55` |
| **col220 10/11** | **A + B + C.** O E6 usa `21:54→03:35` e `04:31→06:00`. No juiz, 03:35 tem `papel_ata=E` e fica interna, 04:31 é `fim_contado`, e 06:00 abre um turno de 0 min em 11/11. A janela tira 60 de 397. Repor os 60 deixa **+33,42 min**. `sonda.out:62,64,65,67` | **sem dono.** A divergência nasce do pareamento e da janela. A troca de dia na lavra alcança o período aberto de 06:00, cujo `minutos_trabalhados=0`. A janela de HE do motor atua em outro cálculo. [turnos.py:1310](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/turnos.py:1310) · [turnos.py:756](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/turnos.py:756) · [dia_pago.py:126](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/services/dia_pago.py:126) · [motor_calculo_v2.py:1545](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/motor_calculo_v2.py:1545) · `sonda.out:68,69` |
| **col874 22/10** | **B + A + C.** O E6 põe a batida de 10:00 de 22/10 no `dia_e6=21/10`, emparelhando duas `E`. Em 22/10 só soma `15:45→22:07`. O juiz usa `10:00→22:07`, com `janela_descontada=60`, chegando a 667. Repor os 60 amplia a diferença para **−344,18 min**. `sonda.out:75–78,83,84,87,88` | **sem dono.** A origem é o trecho que `minutos_do_oraculo` chaveia em 21/10 e soma por posição, combinado com a janela que o realizado tira em 22/10. Uma cura que apenas mude a chave do par de duas `E` preserva a geometria divergente. [nucleo.py:235](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/calculador/nucleo.py:235) · [nucleo.py:245](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/calculador/nucleo.py:245) · [turnos.py:760](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/turnos.py:760) · `sonda.out:83,84,88` |
| **col989 23/10** | **A + B.** O E6 forma `08:32→17:59`, de 567,37 min, entre dois cortes. No juiz, 08:32 fecha o turno de 22/10 e 17:59 abre o de 23/10, que fecha às 06:00 de 24/10 e soma 721 min. O `periodo_motor` do cartão de 23/10 tem as mesmas bordas do juiz. `sonda.out:100–108` | **sem dono.** A origem é o corte e o pareamento posicional do E6. Usar no piso o par observado do juiz daria 720,771 min e um delta de **−0,229 min**, dentro da faixa do R1. A adoção desse par pela obra do E6 nesta chave está **[nao verificado]**. [nucleo.py:215](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/calculador/nucleo.py:215) · [nucleo.py:245](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/calculador/nucleo.py:245) · [GRADE-MESTRA.md:303](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/docs/GRADE-MESTRA.md:303) · `sonda.out:105,106` |
| **col996 10/11** | **A + B, com corte por vão no E6.** `fronteira_anterior` mostra vão de 858,92 min, gap de 326,03 min e quatro pontas. O E6 abre outro trecho e soma `21:16→06:59` na chave de 10/11. No juiz, 21:16 é uma `S` sem entrada e vale 0, e 06:59 é entrada em 11/11. A pausa diurna de 90 min sai dos dois lados. `sonda.out:130,131,135–139` | **sem dono.** O corte por vão e a soma por posição do E6 produzem o par de 583,49 min. Retirar esse par do piso deixaria **−0,107 min**, dentro da faixa do R1. O registro da obra do E6 inclui a família do corte por vão. A mudança que retira este par está **[nao verificado]**. [nucleo.py:215](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/calculador/nucleo.py:215) · [nucleo.py:245](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/calculador/nucleo.py:245) · [BACKLOG.md:823](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/docs/BACKLOG.md:823) · [GRADE-MESTRA.md:303](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/docs/GRADE-MESTRA.md:303) · `sonda.out:130,131,135` |

**D, diferença entre o que soma e o que aparece nos cartões.** A sonda mostra `dia_cartao` diferente de `dia_mapa` em col130 e col220, ambos em períodos de 0 min. Em col996, a saída órfã também vale 0. A contribuição de D para qualquer dos seis deltas fica **[nao verificado]**. `sonda.out:55,69,137,141,142` · [espelho.py:465](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/services/espelho.py:465)

**HJ-026.** `PAR-PELO-TURNO` muda a rejeição pelo teto `previsto + 4 h`. Os fechamentos observados e os turnos sem uma ponta da tabela não mostram essa rejeição como causa. O corte de 14 h observado em col996 vem do E6. [BACKLOG.md:2020](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/docs/BACKLOG.md:2020) · [turnos.py:66](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/turnos.py:66) · [turnos.py:1447](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/turnos.py:1447) · [nucleo.py:215](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/ponto/calculador/nucleo.py:215) · `sonda.out:31,51,53,65,67,88,106,131,136,137`

## 3. Dias ESTRUTURA que zeram por obra

| Obra | Chaves que zera | Contagem comprovada |
|---|---|---:|
| Nenhuma estabelecida | Zeragem por obra **[nao verificado]** nas seis chaves. `sonda.out:30,50,64,87,105,135` | **0** |

## 4. Linhas do quadro

**Nenhuma entre as zeragens comprovadas.** O backlog vincula `PAR-PELO-TURNO` a **HJ-026** e descreve as mudanças de **NOTURNO-A** e **INTERVALO**, mas nenhuma dessas obras tem uma chave atribuída na tabela. A equivalência entre `ORACULO-E6-43-DEFEITOS` e a linha **oráculo 93** está **[nao verificado]**. [BACKLOG.md:1989](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/docs/BACKLOG.md:1989) · [BACKLOG.md:2020](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/docs/BACKLOG.md:2020) · [BACKLOG.md:2554](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/docs/BACKLOG.md:2554) · [GRADE-MESTRA.md:303](/home/ronald/codex_ws/558de6bd4bba318bac4130a14936946e218f885d/app/docs/GRADE-MESTRA.md:303)

## 26/11 11:21 · sessao ad3079f9cb3c · O30-F3-CONSTRUIR · construtor · origem=Code

C6 implementada na cópia sobre `aa32babb3299b8dd53812225c4934d87fd8022ba`. [Base dos REDs](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:20)

- **C6-1.** A cadeia `A → B → C` recebe 404 no drawer, na entrega do anexo, na aprovação, na rejeição e na lista. A lista devolve 404 se a cadeia estiver no resultado. Para vínculos válidos, o template recebe a raiz da view. O selo AST derruba a reintrodução de `documento_referencia or` em uma view. [Resolvedor](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/ausencia.py:291) · [Lista](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/views.py:2347) · [Template](/home/ronald/wt-codex-o30-f3-construir-010143/app/templates/ponto/ausencias.html:168) · [Teste e selo](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:399) [nao verificado]
- **C6-2.** Uma guarda, chamada na criação com referência, no anexo e na reabertura, protege o arquivo existente se a raiz ou qualquer filha estiver aprovada ou rejeitada. O RED reproduz a filha aberta tentando substituir o arquivo de uma raiz rejeitada. [Guarda](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/ausencia.py:327) · [Casos de substituição](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:574)
- **C6-3.** O dry-run chama `derivar_data_fim`, assim como `rederivar_fim`. Ao trocar o retorno do derivador no teste, a c5 ainda imprime `07/09/2026` e a c6 imprime `13/09/2026`. [Prévia](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/management/commands/curar_ausencia_sem_fim.py:37) · [Aplicação](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/ausencia.py:1939) · [Teste](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/test_atestado_f3_contrato.py:629) [nao verificado]

**Verificação local.** Os REDs contra c5 deram **8, 6 e 1 falhas por asserção**, respectivamente. O GREEN passou **55 testes sem banco**. Os 12 mutantes C6 e os dois seletores históricos ajustados deram `FAIL` por asserção, `rc=1`, zero `ERROR`. O `diff --check` passou. Contagem AST nos Python alterados, incluindo testes e runner: `except` 81 → 81 e `suppress` 0 → 0. [REDs](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:93) · [Mutantes](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:267) · [Tratamento HTTP](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/views.py:2837) [nao verificado]

**Diff e comandos**, a partir de `app/`. [Runner](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/tests/mutantes_atestado_f3.py:424)

```bash
git --no-optional-locks diff aa32babb3299b8dd53812225c4934d87fd8022ba -- .
git --no-optional-locks diff --check aa32babb3299b8dd53812225c4934d87fd8022ba -- .

export PYTHONDONTWRITEBYTECODE=1
python3 -m ponto.tests.mutantes_atestado_f3 red_c5_c6_1
python3 -m ponto.tests.mutantes_atestado_f3 red_c5_c6_2
python3 -m ponto.tests.mutantes_atestado_f3 red_c5_c6_3
python3 -m unittest -q ponto.tests.test_atestado_f3_contrato

python3 -m ponto.tests.mutantes_atestado_f3 c6_view_reintroduz_or
python3 -m ponto.tests.mutantes_atestado_f3 c6_servir_pula_validacao
python3 -m ponto.tests.mutantes_atestado_f3 c6_lista_pula_validacao
python3 -m ponto.tests.mutantes_atestado_f3 c6_rejeitar_reintroduz_or
python3 -m ponto.tests.mutantes_atestado_f3 c6_aprovar_reintroduz_or
python3 -m ponto.tests.mutantes_atestado_f3 c6_template_le_elo
python3 -m ponto.tests.mutantes_atestado_f3 c6_anexo_raiz_so_aprovada
python3 -m ponto.tests.mutantes_atestado_f3 c6_anexo_ignora_irmas
python3 -m ponto.tests.mutantes_atestado_f3 c6_anexo_libera_na_raiz
python3 -m ponto.tests.mutantes_atestado_f3 c6_reabrir_pula_guarda
python3 -m ponto.tests.mutantes_atestado_f3 c6_insert_pula_guarda
python3 -m ponto.tests.mutantes_atestado_f3 c6_previa_formula_propria
python3 -m ponto.tests.mutantes_atestado_f3 filha_substitui_aprovado
python3 -m ponto.tests.mutantes_atestado_f3 d2_so_linha_pedida
```

**Fora da volta.** De acordo com a exclusão da janela K6 × exportação, dos relógios residuais e do `elif` e da ordem do `Ausencia.save`. A gravação c5 continua entregando o fim conferido ao `Ausencia.save`. [K6 e gravação](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/ausencia.py:1949) · [Exportação](/home/ronald/wt-codex-o30-f3-construir-010143/app/folha/models.py:174) · [Relógio da projeção](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/services/ausencia.py:191) · [Modelo](/home/ronald/wt-codex-o30-f3-construir-010143/app/ponto/models.py:892)

Produção, efeito financeiro real, renderização com Django e suíte com banco [nao verificado]. Certificação a cargo do Code e do Opus final. [nao verificado]

## 26/11 11:18 · sessao 6e7da3f50006 · CLEAR-NAO-PARA-A-NOITE · verificador · origem=Code

Os quatro arquivos alterados conferem com os hashes de conteúdo do lado novo do diff. Conferi a sintaxe dos selos e exercitei em memória as condições dos 20 mutantes, sem gravar arquivos. A identidade Git de `HEAD` e do pai, a execução integral dos selos e o comportamento em produção ficam [nao verificado]. `bin/disjuntor.py:413`, `bin/hook_stop_fila1.py:1075`, `bin/orcamento_contexto.py:95`, `bin/tests/test_orcamento_contexto.sh:32`, `bin/tests/test_orcamento_contexto.sh:899`.

## Achados da v2b.r

| Achado | Efeito da volta 3 |
|---|---|
| **T1 / S4c / S6f** — janela e borda de 30 min | **Resolve a lacuna do `>=`.** A faixa continua de 23:00 inclusive a 08:00 exclusiva. De dia, 1800 s entram como presença e 1801 s abrem ausência com fala antiga válida. O selo mata a troca do operador, embora veja a projeção `None` aos 1800 s. `bin/orcamento_contexto.py:102`, `bin/orcamento_contexto.py:116`, `bin/tests/test_orcamento_contexto.sh:826`, `bin/tests/test_orcamento_contexto.sh:935`. |
| **T2 / S4a** — disparo ativo entra à noite e disparo pendente | **Mantém a cura nominal da volta 2.** Há anúncio, fim de marco ou recado prioritário conforme o estado observado. O pendente sobrevive à queda da medida e arma no primeiro Stop elegível. A escrita efetiva do AVISO no RELATO fica [nao verificado]. `bin/hook_stop_fila1.py:1092`, `bin/hook_stop_fila1.py:1134`, `bin/hook_stop_fila1.py:1211`. |
| **T3 / S1a–b / S3a / S4e** — repetição e consumo de handoff | **Resolve em parte.** O mesmo `mtime` já consumido por fim noturno deixa de repetir. A troca noturna e a primeira liberação diurna registram o cursor. O fim de marco diurno ainda guarda só a hora do Stop, a liberação diurna durante a espera não atualiza o cursor e um valor futuro alto pode suprimir handoffs novos. A identidade do marco continua inferida pelo `mtime`. `bin/hook_stop_fila1.py:1114`, `bin/hook_stop_fila1.py:1125`, `bin/hook_stop_fila1.py:1177`, `bin/hook_stop_fila1.py:1184`, `bin/hook_stop_fila1.py:1201`. |
| **T4 / S3b** — guarda antes do recado e até a emissão | **Resolve em parte.** Com o arquivo existente e `LOCK_SH` obtido, a guarda dura até o processo sair. Se o arquivo ainda não existe, o Stop passa sem fd e o primeiro DIFF pode criar e tomar a trava antes da emissão. O `siga` sob trava exclusiva continua sendo o recado de conclusão do ato. `bin/disjuntor.py:419`, `bin/disjuntor.py:428`, `bin/sombra_trava.sh:25`, `bin/hook_stop_fila1.py:1101`, `bin/hook_stop_fila1.py:1456`. |
| **T5 / S1c / S4b / S4f** — retorno, espera e reaproveitamento | **Resolve em parte.** `nao_sei` deixa de registrar `CLEAR-VOLTA`. Na volta registrada, o relógio de espera e as marcas são limpos, e o handoff precisa superar o cursor consumido. O corte sobre a fala é inclusivo, aceita como presença uma fala futura e pode aproveitar fala e handoff da própria faixa noturna. Com `nao_sei`, o ramo diurno ainda pode liberar o par antigo sem registrar volta. `bin/orcamento_contexto.py:102`, `bin/orcamento_contexto.py:116`, `bin/hook_stop_fila1.py:1075`, `bin/hook_stop_fila1.py:1085`, `bin/hook_stop_fila1.py:1143`, `bin/hook_stop_fila1.py:1183`. |
| **T6 / T7 / S2c** — juiz compartilhado e classificação | **Mantém a centralização.** ESCADA e orçamento usam `ultima_fala`. A mesma abertura de `orcamento.log` lê `ATENDIDO` e `CLEAR-NOITE`. As exclusões de imagem sem texto e `tool_result`, a aceitação de `enqueue` seguido de `remove` e a aceitação da string sintética como `user` ficam como estavam. `bin/disjuntor.py:1354`, `bin/disjuntor.py:1417`, `bin/disjuntor.py:2139`, `bin/orcamento_contexto.py:110`. |
| **T8 / S2a–b / S8** — transcript inconclusivo, truncado, fora de ordem e tamanho da cauda | **Resolve em parte.** Volta a 4 MiB e as duas sondas de falsa ausência da v2b passam a `nao_sei`. Ainda pode descartar uma fala inteira quando o seek coincide com o início da linha. Uma fala recente com `duvida` rende `presente` e fornece o epoch ao retorno. Uma fala anterior no arquivo e sem instante fica fora da comparação que detecta inversão. `bin/disjuntor.py:1377`, `bin/disjuntor.py:1386`, `bin/disjuntor.py:1397`, `bin/disjuntor.py:1412`, `bin/orcamento_contexto.py:114`. |
| **S3c** — fallback de exceção libera à noite | **Resolve o achado.** O fallback consulta o mesmo juiz e veda `CLEAR-LIBERA` quando há ausência, inclusive ausência diurna identificada. Continua aceitando `nao_sei` de dia e um handoff recente ou futuro pelo teste unilateral de idade. `bin/hook_stop_fila1.py:1267`, `bin/orcamento_contexto.py:132`. |
| **T9 / S5a–b** — contador e bordas | **Resolve em parte.** Os truncamentos e deslocamentos amostrados dão `None`, e a janela passa a ser `(agora−24h, agora]`. Uma linha de três palavras com data válida e sem detalhe conta como recado. Um `CLEAR-NOITE` deslocado para a quarta coluna de uma linha com terceiro token desconhecido é ignorado. `bin/disjuntor.py:2144`, `bin/disjuntor.py:2161`, `bin/disjuntor.py:2173`. |
| **T10 / S6a–g / S7** — mordida do selo | **Resolve em parte.** Preserva os três casos já curados na volta 2, acrescenta os REDs da seção 9 e cobre topo neutro sob DIFF, anúncio inteiro e zero diurno. A frase diurna de fim de marco após a volta ainda é conferida por prefixo. O teste AST ainda permite um juiz paralelo que evite os literais proibidos e mantenha uma chamada a `DJ.ultima_fala` em qualquer ponto do módulo. `bin/tests/test_orcamento_contexto.sh:519`, `bin/tests/test_orcamento_contexto.sh:529`, `bin/tests/test_orcamento_contexto.sh:573`, `bin/tests/test_orcamento_contexto.sh:598`, `bin/tests/test_orcamento_contexto.sh:853`, `bin/tests/test_orcamento_contexto.sh:862`, `bin/tests/test_orcamento_contexto.sh:877`, `bin/tests/test_orcamento_contexto.sh:885`. |
| **T11** — registro e config | **Não resolve o registro.** O 7b conserva a regra geral do marco e `PROMPTS.md` termina sem o aval específico. A config da cópia tem `PreCompact` manual e automático chamando o handoff. O registro prometido no pouso fica [nao verificado]. `CLAUDE.md:343`, `app/docs/PROMPTS.md:3005`, `.claude/settings.json:14`, `.claude/settings.json:25`. |

## Descartes da triagem

| Descarte | O motivo procede? |
|---|---|
| **T1a** — a fala teria de responder a pergunta | **Sim**, pelo trecho de contrato transcrito na triagem e pela regra registrada na cópia. O critério é tempo desde a fala humana. `bin/orcamento_contexto.py:89`, `bin/orcamento_contexto.py:113`. |
| **T2** — AVISO efetivamente escrito | **Sim** para o escopo do hook, que emite a instrução. Cumprimento pela sessão [nao verificado]. `bin/hook_stop_fila1.py:1077`, `bin/hook_stop_fila1.py:1138`, `bin/hook_stop_fila1.py:1459`. |
| **T7a** — imagem sem texto e resposta em `tool_result` | **Em parte.** Os zeros reportados sustentam priorizar outros casos na amostra de oito transcripts. O classificador continua excluindo essas formas, e ocorrência fora da amostra fica [nao verificado]. `v3/triagem.md:6`, `bin/disjuntor.py:1364`. |
| **T7b** — `enqueue` seguido de `remove` | **Sim** como regra explícita do juiz. O `enqueue` já representa fala para a obra. As contagens fornecidas não demonstram o significado de todo `remove` do harness [nao verificado]. `bin/disjuntor.py:1354`, `bin/disjuntor.py:1362`. |
| **T8b / S4c** — hora decide antes do transcript, mesmo se houver fala na faixa | **Sim.** A condição horária é suficiente pelo aval indicado, e o código retorna antes da leitura. `bin/orcamento_contexto.py:89`, `bin/orcamento_contexto.py:102`. |
| **T9b** — hora local sem offset | **Em parte.** Escritor e leitor usam a convenção local. O código não garante o mesmo `TZ` entre processos nem desambigua hora local repetida. Host e fuso efetivos [nao verificado]. `bin/hook_stop_fila1.py:881`, `bin/disjuntor.py:2151`. |
| **S2c** — `Async agent launched` como `user` | **Em parte.** As 119 ocorrências internas a `tool_result` reportadas ficam excluídas. A forma `user` texto existe nos selos como retorno sintético e segue aceita pelo juiz. Fora da amostra [nao verificado]. `v3/triagem.md:6`, `bin/disjuntor.py:481`, `bin/disjuntor.py:1364`, `bin/tests/test_disjuntor.sh:235`, `bin/tests/test_disjuntor.sh:297`. |
| **S3d** — `None` cai no `ALLOW` comum | **Sim.** A fila pendente já permite encerrar, e o lembrete pode ir à trilha. `bin/hook_stop_fila1.py:1456`, `bin/hook_stop_fila1.py:1486`, `bin/hook_stop_fila1.py:1498`. |
| **S4d** — dois DIFFs sem Stop na folga | **Em parte.** A deduplicação por estado observado é coerente. Um Stop durante o segundo ato ainda pode ver a mesma string e omitir o recado, pois a marca não identifica o ato. Isso preserva uma lacuna se a unidade exigida for cada ato real. `bin/hook_stop_fila1.py:1101`, `bin/hook_stop_fila1.py:1110`, `bin/sombra_trava.sh:37`. |
| **S4g** — tempo noturno no clear tardio | **Sim** para a medida de tempo corrido desde o `PAREI` observado, ou desde o disparo quando falta essa marca. O fechamento mede antes de avaliar a janela ou limpar a espera na volta. `bin/hook_stop_fila1.py:1033`, `bin/hook_stop_fila1.py:1040`, `bin/hook_stop_fila1.py:1075`. |
| **S5c** — trilha ausente | **Em parte.** Zero é a convenção implementada para caminho ausente. A exceção não distingue arquivo nunca criado de arquivo removido, portanto não comprova zero recados históricos. `bin/disjuntor.py:2180`. |
| **S5d** — censo abre a trilha principal de novo | **Sim.** O censo mede outro conjunto de eventos. O descarte procede para a leitura unificada de `CLEAR-NOITE` e `ATENDIDO`. `bin/disjuntor.py:2139`, `bin/disjuntor.py:2219`, `bin/disjuntor.py:2259`. |

## 1. Escritores e leitores de `disparo`

O escritor operacional localizado é `_orcamento_travado` no Stop, sob a trava do orçamento. `_orc_salva` faz `os.replace` e depois apenda os eventos. `disjuntor.ler` e `orcamento_contexto.ler` leem o JSON para outras medidas. Os selos semeiam estado diretamente. `bin/hook_stop_fila1.py:904`, `bin/hook_stop_fila1.py:975`, `bin/hook_stop_fila1.py:1010`, `bin/disjuntor.py:2190`, `bin/orcamento_contexto.py:402`, `bin/tests/test_disjuntor.sh:103`.

| Campo | Quem escreve e quem lê no Stop |
|---|---|
| `hora` | O armamento grava o epoch do disparo. Fechamento, AVISO e cortes de handoff leem esse instante. `bin/hook_stop_fila1.py:1040`, `bin/hook_stop_fila1.py:1077`, `bin/hook_stop_fila1.py:1126`, `bin/hook_stop_fila1.py:1147`, `bin/hook_stop_fila1.py:1201`, `bin/hook_stop_fila1.py:1235`. |
| `motivo` | O armamento grava a causa. Fechamento, volta, fim de marco diurno e lembrete a leem. `bin/hook_stop_fila1.py:1049`, `bin/hook_stop_fila1.py:1090`, `bin/hook_stop_fila1.py:1206`, `bin/hook_stop_fila1.py:1235`. |
| `desfecho` | A expiração grava `seguiu`. Fechamento, volta e ramos do disparo leem para exigir ciclo ativo. `bin/hook_stop_fila1.py:1035`, `bin/hook_stop_fila1.py:1079`, `bin/hook_stop_fila1.py:1092`, `bin/hook_stop_fila1.py:1144`, `bin/hook_stop_fila1.py:1187`, `bin/hook_stop_fila1.py:1198`. |
| `noite` | Armamento e processamento sem Ronald gravam `True`. A volta lê e apaga. Representa passagem pela conduta sem Ronald, também por silêncio de dia. `bin/hook_stop_fila1.py:1079`, `bin/hook_stop_fila1.py:1087`, `bin/hook_stop_fila1.py:1097`, `bin/hook_stop_fila1.py:1239`. |
| `noite_anunciada` | Armamento, aviso de DIFF, troca de `PAREI`, fim noturno e anúncio gravam `True`. O anúncio lê para deduplicar. A volta apaga. `bin/hook_stop_fila1.py:1087`, `bin/hook_stop_fila1.py:1103`, `bin/hook_stop_fila1.py:1114`, `bin/hook_stop_fila1.py:1128`, `bin/hook_stop_fila1.py:1134`, `bin/hook_stop_fila1.py:1239`. |
| `noite_clear_visto` | O ramo de `PAREI` noturno lê e marca o pedido de troca. Topo sem `PAREI`, quando processado depois da guarda, ou volta registrada apagam. `bin/hook_stop_fila1.py:1087`, `bin/hook_stop_fila1.py:1101`, `bin/hook_stop_fila1.py:1111`, `bin/hook_stop_fila1.py:1122`. |
| `handoff_visto` | Troca noturna, fim noturno e primeira liberação diurna gravam o maior `mtime` consumido. A volta e o fim noturno leem. Sobrevive à volta e some com o ciclo. O fim diurno só grava `fim_dia_em`. `bin/hook_stop_fila1.py:1054`, `bin/hook_stop_fila1.py:1085`, `bin/hook_stop_fila1.py:1115`, `bin/hook_stop_fila1.py:1126`, `bin/hook_stop_fila1.py:1179`, `bin/hook_stop_fila1.py:1203`. |
| `handoff_min` | O adiamento diurno grava o epoch do Stop. A volta grava o máximo entre corte anterior, instante do fim diurno, epoch da fala e um valor estritamente acima do cursor. Fim noturno, par diurno e fim diurno leem como limite inferior inclusivo de `mtime`. `bin/hook_stop_fila1.py:1085`, `bin/hook_stop_fila1.py:1126`, `bin/hook_stop_fila1.py:1147`, `bin/hook_stop_fila1.py:1163`, `bin/hook_stop_fila1.py:1201`. |
| `fim_visto` / `fim_dia_em` | O fim diurno lê e grava a flag do pedido e grava `agora` como instante do pedido. Volta e fim noturno usam esse instante como corte. A volta apaga os dois. `bin/hook_stop_fila1.py:1085`, `bin/hook_stop_fila1.py:1126`, `bin/hook_stop_fila1.py:1201`. |
| `clear_parei_desde` | A primeira liberação diurna grava o início da espera. A guarda que adia zera, a volta apaga, e expiração e fechamento leem. `bin/hook_stop_fila1.py:1040`, `bin/hook_stop_fila1.py:1088`, `bin/hook_stop_fila1.py:1145`, `bin/hook_stop_fila1.py:1163`, `bin/hook_stop_fila1.py:1177`. |
| `sem_handoff_visto` / `adiado` / `adiado_noite` | Os respectivos ramos leem e gravam a cobrança emitida ou o motivo de adiamento. A volta limpa os três. O ramo noturno também limpa `adiado_noite` ao passar pela guarda sem indicação de DIFF. `bin/hook_stop_fila1.py:1087`, `bin/hook_stop_fila1.py:1101`, `bin/hook_stop_fila1.py:1110`, `bin/hook_stop_fila1.py:1151`, `bin/hook_stop_fila1.py:1165`. |

Os tempos armazenados são instantes em segundos, sem campo usado como data civil. A ambiguidade é de **prova**. `fim_dia_em` é relógio do Stop usado como substituto do `mtime` consumido. `handoff_visto` mistura consumo diurno e noturno num cursor comum e pode marcar um arquivo que a troca sequer precisou validar. Na primeira liberação diurna, a checagem e a gravação fazem dois `stat`, então uma escrita concorrente pode fazer o cursor registrar outro arquivo. O handoff também é gerado por `PreCompact`, portanto avanço de `mtime` por si só não identifica marco fechado. `bin/hook_stop_fila1.py:930`, `bin/hook_stop_fila1.py:1111`, `bin/hook_stop_fila1.py:1147`, `bin/hook_stop_fila1.py:1179`, `bin/hook_stop_fila1.py:1203`, `.claude/settings.json:14`, `bin/handoff_sessao.sh:320`.

## 2. Juízes e contrato da 4-tupla

Há um classificador de fala, `disjuntor._fala_do_ronald`, e um juiz de janela, `orcamento_contexto.estado_ronald`. `janela_sem_ronald` projeta o motivo da ausência. O ramo nominal do Stop faz a mesma projeção localmente. Os chamadores encontrados estão compatíveis com a 4-tupla. `estado_ronald` e o selo desempacotam quatro valores, e `ultima_humana` continua lendo `[0]` para a ESCADA. Os leitores do prompt inicial do B3 e do comando `/clear` fazem outras perguntas. `bin/disjuntor.py:661`, `bin/disjuntor.py:1354`, `bin/disjuntor.py:1417`, `bin/orcamento_contexto.py:110`, `bin/orcamento_contexto.py:132`, `bin/orcamento_contexto.py:303`, `bin/hook_stop_fila1.py:1075`, `bin/tests/test_orcamento_contexto.sh:509`.

## 3. Ordem e duração das travas

- O Stop pega a trava do orçamento com tentativas limitadas e depois sonda a sombra com `LOCK_SH | LOCK_NB`. O DIFF pega `LOCK_EX` com `flock -w`, padrão de 1800 s, e os filhos que herdam a trava pulam a segunda aquisição. O handoff faz uma sondagem exclusiva não bloqueante. Nesse encadeamento, não há ciclo de espera nem espera infinita do DIFF pelo `flock` configurado. `bin/hook_stop_fila1.py:975`, `bin/hook_stop_fila1.py:1099`, `bin/disjuntor.py:428`, `bin/sombra_trava.sh:18`, `bin/sombra_trava.sh:21`, `bin/handoff_sessao.sh:257`.
- O fd retido permanece aberto até a saída do processo Stop, inclusive depois de fechar a trava do orçamento. Se o handoff sondar a mesma sombra nesse intervalo, imprime **TOMADA** por causa do `LOCK_SH` do próprio Stop, mesmo sem DIFF. O handoff que roda depois da saída encontra o fd liberado. O cruzamento em produção fica [nao verificado]. `bin/disjuntor.py:429`, `bin/disjuntor.py:439`, `bin/hook_stop_fila1.py:987`, `bin/hook_stop_fila1.py:1459`, `bin/handoff_sessao.sh:250`.
- A corrida da primeira criação permanece. A guarda devolve livre sem fd quando o caminho está ausente. O DIFF pode então criar o arquivo e pegar `LOCK_EX` antes do recado. O RED R2 começa com o arquivo já criado. `bin/disjuntor.py:419`, `bin/sombra_trava.sh:25`, `bin/hook_stop_fila1.py:1459`, `bin/tests/test_orcamento_contexto.sh:691`.

## 4. Transições e saída

| Transição | Saída observável pelo código |
|---|---|
| **Dia → noite, disparo ativo** | Prioridade para recado de DIFF, troca de `PAREI`, fim com handoff elegível e anúncio de entrada, nessa ordem. Sem condição nova, devolve `None` ao fluxo comum. `bin/hook_stop_fila1.py:1092`, `bin/hook_stop_fila1.py:1142`. |
| **Noite → dia** | `presente` registra `CLEAR-VOLTA`. Topo neutro com handoff elegível recebe pedido de `PAREI`; `PAREI` com par apto libera. Um `mtime` igual ao epoch da fala pode passar. `nao_sei` preserva a marca `noite` e executa o ramo diurno, que ainda pode aceitar o par anterior. Uma fala feita dentro da faixa pode se tornar a prova quando o Stop roda depois das 08:00. `bin/orcamento_contexto.py:102`, `bin/orcamento_contexto.py:116`, `bin/hook_stop_fila1.py:1075`, `bin/hook_stop_fila1.py:1143`, `bin/hook_stop_fila1.py:1183`, `bin/hook_stop_fila1.py:1201`. |
| **DIFF começa e acaba à noite** | Primeiro Stop sob a trava dá `siga` para concluir o ato. O mesmo motivo cai no fluxo comum. Ao observar a trava livre, o Stop limpa o adiamento e reavalia o topo e o handoff. Como o aviso de DIFF já marca a noite anunciada, pode ficar só no fluxo comum. `bin/hook_stop_fila1.py:1101`, `bin/hook_stop_fila1.py:1110`, `bin/hook_stop_fila1.py:1134`. |
| **`PAREI` diurno atravessa a noite e reaparece** | Com a guarda livre, há um `siga` de troca. Persistindo no topo, cai no fluxo comum. Um Stop que processa o topo neutro limpa a marca e permite novo `siga` quando o `PAREI` reaparece. Se o único Stop com topo neutro estiver sob DIFF, ele retorna antes de limpar a marca e o `PAREI` reaparecido pode ficar sem recado. `bin/hook_stop_fila1.py:1101`, `bin/hook_stop_fila1.py:1111`, `bin/hook_stop_fila1.py:1122`. |
| **Pendente entra à noite** | Mantém `PENDENTE` enquanto houver impedimento. No primeiro Stop elegível, arma e emite `siga` com handoff e AVISO, mesmo se a medida tiver baixado. `bin/hook_stop_fila1.py:1211`, `bin/hook_stop_fila1.py:1235`. |
| **Marco de clear novo à noite** | Fecha o ciclo velho antes de decidir a janela. Registra `ATENDIDO` dentro do prazo, `SEGUIU` se tardio, ou `RESET` se mudou o marco sem clear elegível. Pode armar outro ciclo no mesmo Stop. O escritor do marco de clear não exige prova de handoff anterior. `bin/hook_stop_fila1.py:1033`, `bin/hook_stop_fila1.py:1067`, `bin/hook_stop_fila1.py:1235`, `bin/marco_de_clear.py:39`, `bin/marco_de_clear.py:55`. |
| **Handoff com `mtime` futuro** | O mesmo valor gera um fim noturno e depois fica suprimido. Um handoff realmente novo com `mtime` menor também fica suprimido, inclusive na volta pelo `nextafter`. O mesmo arquivo futuro já usado num fim **diurno** ainda pode gerar fim noturno, pois o pedido diurno não gravou o cursor. Reproduzido em memória. `bin/hook_stop_fila1.py:1085`, `bin/hook_stop_fila1.py:1125`, `bin/hook_stop_fila1.py:1201`. |

**Mesmo estado observado:** o `siga` de anúncio, de `PAREI` contínuo, de DIFF contínuo e de um `mtime` noturno já consumido fica deduplicado no disparo persistido. **A noite pode encerrar o Stop:** `None` pode virar `ALLOW` silencioso pelo fluxo comum. O ramo noturno e o fallback com ausência reconhecida vedam `CLEAR-LIBERA` pelo par. Prova do RELATO e B4 têm prioridade no `main`. `bin/hook_stop_fila1.py:1101`, `bin/hook_stop_fila1.py:1122`, `bin/hook_stop_fila1.py:1269`, `bin/hook_stop_fila1.py:133`, `bin/hook_stop_fila1.py:1437`, `bin/hook_stop_f

[truncado em 20000 de 27046 caracteres]

## 26/11 11:15 · sessao d9abafd3bc46 · PORTA-CONFERE-TRAVA · censo · origem=Code

**45 sítios. A 10 · B 7 · C 25 · D 3 · ? 0.** Contagem pela classe em negrito de cada linha. **B** prevalece quando há um caminho demonstrável com Empresa em cache antes da aquisição de C. Quando há entregas A e C no mesmo sítio, o rótulo segue a primeira entrega descrita na linha. Os dois caminhos ficam registrados na tabela.

**Antes/depois** refere-se à aquisição de C do dono do par. A trava devolve PKs, e uma chamada reentrante conserva a aquisição original. **Fria** descreve a FK à entrada do C20. Em caso de falta no memo, o C20 busca a Empresa por `in_bulk` e aquece a FK. O C6 pode retornar antes do C20 se a pergunta já estiver validada. `app/chamados/models.py:2910–2922`, `app/chamados/services/trava_do_colaborador.py:45–64`, `app/chamados/juizes.py:1041–1051`, `app/chamados/juizes.py:1086–1118`, `app/chamados/juizes.py:1494–1503`

| Sítio | Instância chega de (arquivo:linha) | Antes/depois | FK cache/fria | Classe |
|---|---|---|---|---|
| `app/api/foto_recebida.py:_validar_pelo_dono` | A validação de `app/api/foto_recebida.py:93` roda sob a **segunda** trava, em `:90`. Ela carrega outra pergunta em `app/chamados/services/validacao.py:726–746`. A cauda chega ao C6 pelo reverso em `app/chamados/services/materializacao.py:2137` e `app/chamados/juizes.py:1600–1601`. A primeira trava, em `app/api/foto_recebida.py:73`, cobre a resposta automática. `app/chamados/services/disputa_emissao.py:1746–1764` | Depois | Fria na cauda. Cache pós-C no gate de chamado. `app/chamados/models.py:364–387` | **C**, também A |
| `app/chamados/management/commands/backfill_fio_morto_e4b.py:_executar_dia` | A fábrica em `:166` pode chegar à pergunta relida em `app/chamados/services/relavra_via.py:236–242`, via `app/chamados/services/fabrica_da_celula.py:207–209`. A reabertura em `:149–153` também carrega perguntas para C6. A materialização em `:169` relê em `app/chamados/services/materializacao.py:2282` e pode chegar ao gate de `app/chamados/models.py:364–387`. A trava de `:163` é condicional. O `handle` conserva C em `:361–362` até a chamada em `:404`. | Depois do C exterior | Fria na fábrica e na reabertura. Cache pós-C no gate de chamado. | **C**, também A |
| `app/chamados/management/commands/backfill_fio_morto_e4b.py:handle` | Sob `:361–362`, o replanejamento de `:390` chega ao lote de `app/chamados/services/tarefa_do_dia.py:528` e `:554–555`, que carrega seu próprio cadastro. A reabertura de `:92–94` usa outra pergunta. A foto de `:365`, `:384`, `:407` e `:435` lê janela e K6. `:406`, omitida da lista do anexo, pode executar o callback do beco dentro da transação. `app/chamados/services/foto_e4b.py:51–80`, `:229–255`, `app/chamados/services/becos.py:86–92` | Depois, para os donos travados pelo plano. `app/chamados/management/commands/backfill_fio_morto_e4b.py:243–244`, `:361–362` | Cache no cadastro da tarefa. Fria na pergunta de `:92–94`. | **A**, também C |
| `app/chamados/management/commands/backfill_fio_morto_e4b.py:_travar_colaboradores` | `:244` → `:252` → `:216–221` → `app/chamados/services/disputa_emissao.py:1917` → perguntas de `app/chamados/models.py:1549–1555`, julgadas em `:1599`. | Depois | Fria na primeira entrega. Dono lazy em `app/chamados/juizes.py:1041`. | **C** |
| `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:handle` | Perguntas com `disputa__colaborador__empresa` em `:97–99`, depois de `:78`. A chamada de `:105` também relê com Empresa. `app/chamados/services/validacao.py:434`, `:465–468` | Depois | Cache pós-C | **A** |
| `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:_rodar` | O pai relido em `:113` está na fila de cobrança. `:118` reata sem fornecer dia novo. A colisão de data fica de fora e a costura para no destino vivo, antes do C6. `app/chamados/services/painel.py:188–190`, `app/chamados/catalogo/motor.py:24–31`, `app/chamados/services/limbo_folgas.py:239–245`, `app/chamados/services/fio.py:924–936`, `:1022`, `:1049` | — | — | **D** |
| `app/chamados/management/commands/validar_classe_a.py:handle` | `:125–128` relê e reclassifica. O C20 vem pela validação de `:132`, com pergunta própria de `app/chamados/services/validacao.py:726–746`. O `atomic` de `:118` mantém a trava de `:124` durante a cauda de `app/chamados/services/validacao.py:830`. `app/chamados/services/materializacao.py:2137`, `:2293`, `app/chamados/juizes.py:1600–1601` | Depois | Fria na cauda se ainda intacta. Cache pós-C quando K6 ou o gate de chamado já carregou Empresa. `app/chamados/services/materializacao.py:818–835`, `app/chamados/models.py:364–387` | **C**, também A |
| `app/chamados/models.py:ChamadoColaborador.abrir` | `:971` → `:988` → `:1085` chega às perguntas de `:1549–1555`, julgadas em `:1599`, ou à migração que lê em `app/chamados/services/fio.py:2478–2484`. A guarda em `app/chamados/models.py:1099–1110` usa K7, com C6 na coleta das perguntas de `app/chamados/juizes.py:2523–2526`. | Depois | Fria no primeiro par desses seletores | **C** |
| `app/chamados/models.py:ChamadoColaborador.ressuscitar_perguntas` | `:1636` é triagem. Sob `:1655`, a chamada de `:1676` recompõe o universo em `:1549–1555` e julga a cópia em `:1570–1599`. | Depois | Fria na primeira entrega. O mapa pode reutilizar o dono aquecido no laço. `app/chamados/models.py:1562`, `app/chamados/juizes.py:1117`, `:1275–1283` | **C** |
| `app/chamados/models.py:DisputaSupervisao.fechar` | As perguntas são consultadas de novo em `:2097`, pelo reverso da **mesma** disputa `self`. A guarda de `:2116` pode aquecer seu Colaborador antes de `:2123`. O refresh parcial de `:2130` conserva essa relação para `:2138`. Há chamador que já faz C6 pelo mesmo `d` antes de `fechar`. `app/chamados/management/commands/encerrar_perguntas_com_fato.py:82–89`, `app/chamados/models.py:2688–2691`, `app/chamados/juizes.py:1117` | Dono/Empresa antes no caminho B. Perguntas depois. | Cache anterior | **B** |
| `app/chamados/models.py:DisputaSupervisao.remover_motivo` | O carimbo de `:2292` é de identidade. `:2303` está após o `atomic` local. Quando o signal roda no `atomic` de `app/colaboradores/services/estrutura.py:66–77`, o C de `:2253` continua valendo para as perguntas de `app/chamados/models.py:2097`, na disputa carregada em `app/colaboradores/signals.py:71–77`. | Depois, com `atomic` exterior | Fria na primeira entrega nesse caminho. `app/chamados/models.py:2045`, `:2097–2116`, `:3491–3520` | **C** com C exterior, D no escopo local |
| `app/chamados/models.py:DisputaSupervisao.reabrir_por_colab` | `:2481`, depois de `:2469`, consulta novamente o queryset global de `app/chamados/juizes.py:1853–1854`. O C6 é acionado para a pergunta materializada em `app/chamados/juizes.py:1595`. | Depois | Fria | **C** |
| `app/chamados/models.py:PerguntaDisputa._nascer` | `ex` vem da coleta de `:3185`, por `app/chamados/services/fio.py:2144–2145` e `app/chamados/models.py:2995–3001`, ou do `get` de `:3235`. Após C em `:3123`, a costura de `:3258` acessa Empresa antes de passar a mesma pergunta ao C6. `app/chamados/services/fio.py:930–936` | Depois | Cache aquecido sob C | **A** |
| `app/chamados/reconciliador.py:retratar_com_motivo` | Sob `:576`, `:592` → `:606` chega às perguntas e Empresas de `app/chamados/models.py:364–387`. O resolvedor de `:616` também usa suas próprias cargas. A retenção em `:588` usa `ignorar_tranca=True`. `app/chamados/juizes.py:2212–2214`, `app/chamados/services/disputa_emissao.py:2038–2042`, `:2071–2074` | Depois | Cache pós-C no gate. Fria em parte do resolvedor. | **A**, também C |
| `app/chamados/reconciliador.py:retratar_perguntas_absolvidas_por_celula` | `frescas` em `:681–683`, com `disputa__colaborador__empresa`, vai à consulta de `:695`. C foi adquirido em `:679`. A chamada de `:691` é reentrante em C e acrescenta as demais travas. | Depois | Cache pós-C | **A** |
| `app/chamados/reconciliador.py:encerrar_orfas_pela_lampada` | O dono de `:1499` substitui o da pergunta relida em `:1268–1269`. O C6 de `:1210` pode aquecer esse mesmo dono na primeira iteração. Outra iteração entra em `:1267` com a Empresa já em cache. `:1299`, `:1306` e `:1307` ficam após o `atomic` por pergunta. `app/chamados/juizes.py:1117`, `:1279–1283` | Dono antes. Pergunta depois. | Cache de iteração anterior no caminho B. Inicialmente fria. | **B** |
| `app/chamados/services/acoes_disputa.py:corrigiveis_sob_a_trava` | `:116` chama o seletor de `app/chamados/juizes.py:1853–1854` após `:112`. A pergunta materializada chega a C6 por `app/chamados/juizes.py:1595`. | Depois | Fria | **C** |
| `app/chamados/services/acoes_disputa.py:acao_corrigir_resposta` | A pergunta relida em `:425` vai ao core de `:442` com `exige_viva=False`. A hora de `:380` fica fora dos escapes. A validação de `:454` roda após o `atomic` de `:418`. `app/chamados/services/disputa_emissao.py:1471`, `:1517–1519`, `:1593–1596`, `app/chamados/services/materializacao.py:1169–1172` | — | — | **D** |
| `app/chamados/services/becos.py:fechar_beco` | Sob `:53`, o callback faz o refresh integral de `d` em `:59` e consulta o reverso de novo por `app/chamados/models.py:2052`, `:2097`. `:58` também pode acionar o resolvedor com as cargas de `app/chamados/services/disputa_emissao.py:2038–2042`, `:2071–2074`. O callback é síncrono. `app/chamados/services/fio.py:1571–1578` | Depois | Fria no C7 próprio. Cache pós-C nas ancoradas do resolvedor. | **C**, também A |
| `app/chamados/services/declaracao_texto.py:declarar_hora_colab` | Candidata de `:391`, carregada por `:252` e `app/chamados/services/fio.py:2144–2145` sob a trava de `:363`. C6 por `:256` e `app/chamados/juizes.py:1595`. A costura de `:398` pode aquecer a FK em `app/chamados/services/fio.py:930`. O chamado de `:369` é usado na guarda de horário. | Depois | Fria na primeira guarda da candidata. Cache pós-C na costura. | **C**, também A |
| `app/chamados/services/declaracao_texto.py:aplicar_declaracao` | Candidata de `:698`, carregada por `:252` e `app/chamados/services/fio.py:2144–2145` após `:685`. `:702` pode chegar ao reuso de `app/chamados/models.py:3185`, `:3258`. `:738` fica após o `atomic` local. | Depois | Fria na guarda da candidata. Cache pós-C na costura do reuso. `app/chamados/juizes.py:1595`, `app/chamados/services/fio.py:930–936` | **C**, também A |
| `app/chamados/services/disputa_emissao.py:_abrir_ou_agregar_disputa` | `:582–586` restringe o pai a `VIVOS`, excluindo o ramo de renascimento de `app/chamados/models.py:987–988`. Nos chamadores identificados, o contexto do pai não fornece dia forte ao sinal. `app/chamados/catalogo/modulos.py:144–145`, `app/chamados/signals.py:215–218`, `app/chamados/services/disputa_emissao.py:641–644`, `app/chamados/services/fabrica_da_celula.py:172`, `app/api/views.py:748–770`, `app/api/views_core.py:742–764`, `app/ponto/services/triagem_batida.py:734–743`, `app/ponto/management/commands/marcar_foto_ausente_retro.py:96–103` | — | — | **D** |
| `app/chamados/services/disputa_emissao.py:criar_disputa_ancora_de_chamado` | Sob `:1345`, o `analisar()` de `:1393` pode chamar o cartório pelo sinal de status. O `data_inicio` do limbo dá o dia forte. As perguntas vêm de `app/ponto/services/cartorio.py:1701–1702`, ou da carga E2 de `app/chamados/reconciliador.py:681–695` via `app/ponto/services/cartorio.py:1026–1027`. A conversa de `:1385` não fornece dia de turno. `app/chamados/models.py:866–873`, `app/chamados/signals.py:193–218`, `app/ponto/management/commands/detectar_ausencias.py:294–297`, `app/chamados/juizes.py:339` | Depois | Fria na carga do cartório. Cache pós-C no E2. | **C**, também A |
| `app/chamados/services/disputa_emissao.py:responder_pergunta_disputa_core` | Refresh integral de `p` em `:1494` após `:1490`. Com `exige_viva`, `:1519` usa essa pergunta, com dono lazy em `app/chamados/juizes.py:1041`. A ausência de `:1596` fica após o `atomic` local e tem aquisição própria. `app/chamados/services/materializacao.py:1075` | Depois | Fria na guarda direta | **C** |
| `app/chamados/services/disputa_emissao.py:resolver_perguntas_por_chamado_resolvido` | Sob `:2001`, `:2038–2042` carrega perguntas com Colaborador, e `:2072` carrega ancoradas com Empresa via `app/chamados/models.py:364–366`. O mapa de `:2071–2074` pode substituir o dono da primeira carga pelo da segunda. `:2167` e `app/chamados/models.py:2052` também alimentam o fecho. `:2055` repassa um memo. | Depois | Fria nas perguntas sem dono vindo das ancoradas. Cache pós-C nas ancoradas e onde o mapa reutiliza seu dono. | **C**, também A |
| `app/chamados/services/fio.py:processar_post_fio` | `:751–752` carrega pergunta e Colaborador após `:750` para C6 em `:758`. Os renders de `:755`, `:759` e da ação de `:760` podem reler em `:296–304` e julgar em `:316`. As chamadas de `:782–876` listadas no anexo pertencem a outros ramos. `app/chamados/services/fio.py:630–634`, `:746–776` | Depois | Fria nessas cargas | **C** |
| `app/chamados/services/fio.py:_costura_nao_mata_o_fio` | No histórico, o Colaborador de `app/chamados/signals.py:370` é ligado à pergunta em `app/chamados/reconciliador.py:1669–1672`. A Empresa é acessada em `:866`, via `:1741`, antes da costura de `:1794`. `app/chamados/services/fio.py:930–936` usa a mesma pergunta e o mesmo dono. A trava de `:923` é reentrante nesse percurso. | Antes no histórico | Cache anterior. No reuso de `_nascer`, o cache pode ser aquecido sob C. `app/chamados/models.py:3235`, `:3258` | **B**, também A |
| `app/chamados/services/fio.py:reapontar_resolvedora` | A mesma pergunta do histórico de `app/chamados/reconciliador.py:1669–1672`, `:1741`, `:1794` segue de `:1044` a `:1064` e ao C6 em `:936`. A colisão de `:1050` também julga outras perguntas coletadas em `:1109`, `:1776–1778`, `:2266–2271`, em C6 na `:1115`. | Antes no histórico. Depois para as outras perguntas da colisão. | Cache anterior no histórico. Fria na primeira entrega da colisão. | **B**, também C |
| `app/chamados/services/fio.py:migrar_resolvedoras_vivas` | Depois de `:2446`, as perguntas vêm de `:2478` e `:2482`; C6 em `:2479`, `:2484`. O mapa de `:2477` começa vazio. | Depois | Fria na primeira entrega. `:2525` pode aproveitar a mesma instância já aquecida sob C. `app/chamados/juizes.py:1117`, `:1275–1283` | **C** |
| `app/chamados/services/limbo_folgas.py:agrupar_furo` | `:282`, depois de `:275`, usa perguntas de `app/chamados/models.py:364–366`, julgadas em `:387`. Os fechos de `:284` e `:287` passam novamente pelo gate de `app/chamados/models.py:400–402`, `:708`. | Depois | Cache pós-C. O S143 também tem carga fria. `app/chamados/services/disputa_emissao.py:2038–2042`, `:2071–2074` | **A**, também C |
| `app/chamados/services/materializacao.py:_materializar_ausencia_de_resposta` | `q` é relida em `:1076`, mas o reverso de `:1048` conserva a disputa de `:1032` e o dono de `:1038`. A criação da ausência pode aquecer a Empresa antes de `:1075`. Uma irmã anterior também pode aquecê-la antes de nova aquisição. C6 em `:1104` → `app/chamados/models.py:3445`. `app/ponto/services/ausencia.py:483–484`, `app/ponto/signals.py:85–93`, `:143–147`, `app/ponto/services/cartorio.py:1225`, `app/chamados/juizes.py:1117` | Dono antes no caminho B. `q` depois. | Cache anterior nesses caminhos. Fria se ainda intacta. | **B**, também C |
| `app/chamados/services/materializacao.py:materializar_perguntas_validadas_da_disputa` | `:2282` relê `p`, mas `:2291` repassa a disputa recebida. Ela pode ter sido aquecida pelo C6 de `app/chamados/services/acoes_disputa.py:169` antes do plantio de `:178`. A batida recebe esse dono em `app/chamados/services/materializacao.py:1581–1582`, e o sinal passa a mesma instância ao mapa das órfãs. `app/ponto/registro_batida.py:170–177`, `app/ponto/signals.py:206–215`, `app/chamados/reconciliador.py:1581`, `:1672`, `:866–869`, `:1741`. `:2293` fica após a trava local por pergunta. | Dono antes de `:2278` no caminho B. Pergunta depois. | Cache anterior na cascata da batida | **B** |
| `app/chamados/services/passivo_validado.py:reparar` | `:400–404` carrega pergunta com Empresa após `:399`, entrega ao plantio em `:409`, `:491`, `:585`. O plantio usa a disputa dessa carga para a batida e seus sinais. `app/chamados/services/materializacao.py:1581–1582`, `app/ponto/signals.py:206–215`. `:447` está em outro `atomic`. | Depois no plantio | Cache pós-C no dono entregue pelo plantio | **A** |
| `app/chamados/services/relavra_via.py:desfazer_carimbo_sem_lastro` | Pergunta de `:236–239`, após `:230`, com disputa e célula. C6 em `:242`, dono lazy em `app/chamados/juizes.py:1041`. | Depois | Fria | **C** |
| `app/chamados/services/validacao.py:devolver_instante_sem_texto_l130` | `:467` carrega pergunta e Empresa após `:465`. `:468` chega ao C6 em `:434`. | Depois | Cache pós-C | **A** |
| `app/chamados/services/validacao.py:_aceitar_a_folga` | Sob `:552` ou `:584`, `:563` ou `:588` chega às perguntas de `app/ponto/services/veredito_celula.py:180–193`. A cascata do chamado em `app/ponto/services/veredito_celula.py:208` usa `app/chamados/models.py:364–387`. `:628` agenda `on_commit`. `app/ponto/services/declarar_dia.py:205–208`, `app/ponto/services/fechamento.py:1848` | Depois | Fria no fecho direto das perguntas. Cache pós-C no gate dos chamados. | **C**, também A |
| `app/chamados/services/validacao.py:validar_pergunta` | `:746` relê integralmente `perg` sob a trava de `:745`. O ramo de folga em `:779` usa as perguntas de `app/ponto/services/veredito_celula.py:180–193`; `:815` e `:819` podem alcançar os sinais do plantio. K6 pode aquecer o dono relido. `:798` agenda `on_commit`. `:830` fica após o `atomic` local. `app/chamados/services/materializacao.py:835`, `:1581–1582`, `app/ponto/services/fechamento.py:1848` | Depois | Fria no fecho direto da folga. Cache pós-C nos caminhos em que K6 ou o gate carrega Empresa. `app/chamados/models.py:364–387` | **C**, também A |
| `app/chamados/services/validacao.py:validar_lote_classe_a` | `:1162` reclassifica a pergunta de `:1157–1158`. O caminho a C20 vem de `:1165` → `app/chamados/services/acoes_disputa.py:21` → nova pergunta em `app/chamados/services/validacao.py:730`, `:746`, e cauda em `:830`. O `atomic` de `:1153` mantém C nessa cauda. `app/chamados/services/validacao.py:1265–1301`, `app/chamados/juizes.py:1600–1601` | Depois | Fria no reverso se ainda intacta. Cache pós-C nas cargas do gate de chamado. `app/chamados/models.py:364–387` | **C**, também A |
| `app/chamados/views_cobrar.py:validar_inline` | `_d` de `:222`, após `:218`, vem de `app/chamados/services/fio.py:148–155`. A guarda de `:224` lê as perguntas em `app/chamados/juizes.py:1600–1601`. `:226` delega à validação e conserva C durante sua cauda. `app/chamados/services/validacao.py:730`, `:746`, `:830` | Depois | Fria na guarda. Cache pós-C possível na cascata. `app/chamados/models.py:364–387` | **C**, também A |
| `app/ponto/services/cluster_espurio.py:tratar_legado_encerrado_sem_lastro` | `:287`, após `:279`, chega ao C6 pelas perguntas de `app/chamados/juizes.py:2523–2526`, via `app/chamados/models.py:1109`. Se reabrir, também pode usar as perguntas de `app/chamados/models.py:1549–1555`, `:1676`, ou de `app/chamados/services/fio.py:2478–2484`. O dono do chamado de `:281` é usado na guarda K7. `app/chamados/models.py:1110` | Depois | Fria nessas cargas de perguntas | **C** |
| `app/ponto/services/declarar_dia.py:_escrever_folga` | `:206`, após `:190`, chega às perguntas de `app/ponto/services/veredito_celula.py:180–193`. `:191` também pode relavrar e acionar o cartório. `app/escala/folgas.py:333–339`, `app/ponto/portas/celula.py:392`, `:479`, `app/ponto/services/cartorio.py:1026–1027` | Depois | Fria no fecho direto. Cache pós-C no E2 ou no gate de chamado. `app/chamados/reconciliador.py:681–695`, `app/chamados/models.py:364–387` | **C**, também A |
| `app/ponto/services/justificativa.py:encerrar_chamado_de_origem` | Após `:41`, `:46` delega à retratação. O C6 do gate usa perguntas com Empresa de `app/chamados/models.py:364–387`. A retenção ignora a tranca. `app/chamados/reconciliador.py:606`, `:616`, `app/chamados/juizes.py:2214` | De

[truncado em 20000 de 22543 caracteres]

## 26/11 11:13 · sessao a976abdc32aa · - · - · origem=Ronald (tmux)

Censo dos 16 sítios atribuídos. Nas linhas com A e C, a origem de cada entrega está determinada pelo ramo indicado.

| Sítio | Origem da entrega sob a trava |
|---|---|
| `app/chamados/reconciliador.py:558` `retratar_com_motivo` | **A** no gate de `ch.reconciliar`, acionado em `:606`. Perguntas e donos com Empresa relidos em `app/chamados/models.py:364` e julgados em `app/chamados/models.py:387`. O resolvedor de `:616` tem caminhos A e C. A retenção em `:588` passa `ignorar_tranca=True` em `app/chamados/juizes.py:2214`. |
| `app/chamados/reconciliador.py:629` `retratar_perguntas_absolvidas_por_celula` | **A**. Trava C em `:679`, materializa perguntas com `disputa__colaborador__empresa` em `:681` e consulta C20 em `:695`. A chamada de `trava_na_ordem` em `:691` acrescenta as outras travas. |
| `app/chamados/reconciliador.py:1236` `encerrar_orfas_pela_lampada` | **B**, com FK inicialmente fria. O Colaborador de `:1499` é preservado em `:1268`, via `app/chamados/juizes.py:1279`, embora a pergunta seja nova. O primeiro C20 que lê a Empresa aquece a mesma instância em `app/chamados/juizes.py:1117`. O próximo `atomic` adquire C em `:1267` com essa FK em cache da iteração anterior. `:1299`, `:1306` e `:1307` ficam após o `atomic` de cada item. |
| `app/chamados/services/acoes_disputa.py:105` `corrigiveis_sob_a_trava` | **C**. A segunda consulta de `perguntas_corrigiveis`, disparada em `:116`, carrega P em `app/chamados/juizes.py:1853` depois de `:112`. O C6 só é chamado para as carimbadas via `app/chamados/juizes.py:1595`, com dono lazy em `app/chamados/juizes.py:1041`. |
| `app/chamados/services/acoes_disputa.py:343` `acao_corrigir_resposta` | **D** sob `:423`. A pergunta de `:425` chega ao core em `:442` com `exige_viva=False`, `app/chamados/services/disputa_emissao.py:1471` e `:1517`. A hora de `:380` fica fora dos escapes de `app/chamados/services/materializacao.py:1169`. A validação de `:454` vem depois da saída do `atomic`. |
| `app/chamados/services/becos.py:38` `fechar_beco` | **C** no C7 de `:60` depois do `d.refresh_from_db()` integral em `:59`. O reverso é lido de novo em `app/chamados/models.py:2052` e `:2097`, com Empresa fria. O resolver em `:58` tem caminhos A/C. `executar_sob_cerca` executa o callback dentro do bloco, `app/chamados/services/fio.py:1571` e `:1577`. |
| `app/chamados/services/declaracao_texto.py:308` `declarar_hora_colab` | **C** na candidata de `:391`, carregada por `:252` e `app/chamados/services/fio.py:2144` depois de `:363`. Colaborador lazy, Empresa fria no primeiro C6 de `:256` via `app/chamados/juizes.py:1595`. A costura de `:398` pode aquecer a Empresa dentro do mesmo C em `app/chamados/services/fio.py:930`. A releitura de `:369` alimenta a guarda de horário. |
| `app/chamados/services/declaracao_texto.py:669` `aplicar_declaracao` | **C** na candidata de `:698`, mesma carga de `:252` e `app/chamados/services/fio.py:2144`, depois de `:685`. O caminho de reuso do nascimento também pode chegar com Empresa aquecida sob C, `app/chamados/models.py:3185` e `:3258`, `app/chamados/services/fio.py:930`. `:738` fica fora do `atomic` local. |
| `app/chamados/services/disputa_emissao.py:483` `_abrir_ou_agregar_disputa` | **D** nos chamadores identificados. O filtro `status_local__in=VIVOS` em `:586` exclui o renascimento terminal de `app/chamados/models.py:987`, conforme `app/chamados/catalogo/motor.py:24` e `:30`. O módulo não tem chave de data em `app/chamados/catalogo/modulos.py:144`. Os contextos de produção não passam `data_turno` no topo, de modo que o signal de `app/chamados/signals.py:215` não chama cartório. `:641`, `app/chamados/services/fabrica_da_celula.py:172`, `app/api/views.py:761`, `app/api/views_core.py:755`, `app/ponto/services/triagem_batida.py:734`, `app/ponto/management/commands/marcar_foto_ausente_retro.py:96`. |
| `app/chamados/services/disputa_emissao.py:1316` `criar_disputa_ancora_de_chamado` | **C/A** pelo `analisar` de `:1393`, sob `:1345`. `app/chamados/models.py:869` e `:308` salvam o estado. O signal síncrono `app/chamados/signals.py:215` aceita `data_inicio` do `ancora_limbo`, `app/chamados/catalogo/modulos.py:131`, `app/ponto/management/commands/detectar_ausencias.py:297`. Se houver julgamento da célula, `app/ponto/services/cartorio.py:1701` relê P sem Empresa (C), e `:1026` pode acionar E2 com a carga A de `app/chamados/reconciliador.py:681`. A conversa de `:1385` não entrega um dia de fato. |
| `app/chamados/services/disputa_emissao.py:1471` `responder_pergunta_disputa_core` | **C**. `:1494` recarrega integralmente P depois de `:1490`; `:1519` usa essa P, com a relação lazy em `app/chamados/juizes.py:1041`. A ausência em `:1596` fica depois do `atomic` local e tem trava própria em `app/chamados/services/materializacao.py:1075`. |
| `app/chamados/services/disputa_emissao.py:1985` `resolver_perguntas_por_chamado_resolvido` | **A/C**. Depois de `:2001`, a consulta de `:2074` recebe P ancoradas carregadas com Empresa por `app/chamados/models.py:364` e P de `:2038` sem Empresa. O mapa de `:2071` reaproveita o dono das ancoradas quando há a mesma PK. P adicionais são lidas em `:2167` e `app/chamados/models.py:2052`. `:2055` copia um memo do cartório, sem substituir o dono da P. |
| `app/chamados/services/fio.py:621` `processar_post_fio` | **C**. Sob `:750`, P e Colaborador sem Empresa em `:751`, C6 em `:758`. O render de `:755` e `:759` também relê P em `:296` e julga em `:316` e `:402`. A listagem do anexo inclui outros ramos, como `:782` e `:824`, fora desse `atomic`. |
| `app/chamados/services/fio.py:901` `_costura_nao_mata_o_fio` | **B** no histórico demonstrável. `app/chamados/signals.py:370` carrega o Colaborador numa chamada pós-commit; `app/chamados/reconciliador.py:1669` e `:1672` ligam-no à P e `:866`, alcançada por `:1741`, aquece a FK antes do `reapontar` de `:1794`. A trava `:923` é reentrante nesse caminho. `:930` e `:936` leem a mesma P. Com FK originalmente fria, `:930` a aquece sob C antes de C20. |
| `app/chamados/services/fio.py:959` `reapontar_resolvedora` | **B** no histórico acima, em relação à primeira trava `:1044`. Também **C** no ramo de colisão `:1050`, com outras perguntas de `:1109` e `:2144`, julgadas em `:1115`. |
| `app/chamados/services/fio.py:2402` `migrar_resolvedoras_vivas` | **C** nas primeiras entregas. Depois de `:2446`, as P são outras consultas em `:2478` e `:2482`, com mapa vazio em `:2477` e carga do dono em `app/chamados/juizes.py:1041`. `:2525` pode entregar a mesma instância aquecida pelo C20 sob a mesma trava, `app/chamados/juizes.py:1117`. |

## 26/11 11:12 · sessao 37a822ff5230 · - · - · origem=Ronald (tmux)

| Sítio | Entrega ao C20 sob a trava indicada | Classe |
|---|---|---|
| `limbo_folgas.agrupar_furo` | Depois de `limbo_folgas.py:275`, as perguntas de `models.py:364–366` chegam com Empresa em cache a `models.py:387`. Os fechos de `limbo_folgas.py:284,287` repetem a coleta pelo gate de `models.py:400,708`. | A, com A/C no S143 (`disputa_emissao.py:2038–2042,2071–2074`) |
| `materializacao._materializar_ausencia_de_resposta` | `q` é relida em `materializacao.py:1076`, mas o reverso de `materializacao.py:1032,1048,1058` a prende à mesma `disputa`. O Colaborador é acessado em `materializacao.py:1038` antes de C em `materializacao.py:1075`. A criação síncrona da ausência pode aquecer a Empresa antes, via `ponto/services/ausencia.py:483,529,562`, `ponto/signals.py:85–93,143–147` e `cartorio.py:1016,1225,2114`. Iterações também reutilizam o dono aquecido pela primeira irmã em `juizes.py:1117` depois que o atomic por irmã termina. O C6 de `materializacao.py:1104` passa por `models.py:3445`. | B, com C se a FK chegar fria |
| `materializacao.materializar_perguntas_validadas_da_disputa` | `p` é relida após `materializacao.py:2278` em `materializacao.py:2282`, mas a chamada de `materializacao.py:2291` repassa a disputa de entrada. A pré-triagem de `acoes_disputa.py:169` pode aquecer seu dono pelo reverso de `juizes.py:1600–1601,1041,1117`. Uma iteração anterior também pode aquecê-lo e liberar C. O plantio usa esse mesmo `disputa.colaborador` em `materializacao.py:1581–1582,1698–1699`, preservado em `ponto/registro_batida.py:170–177`, e o sinal de `ponto/signals.py:206–215` chega ao mapa semeado em `reconciliador.py:1672` e a C20 em `reconciliador.py:1741,866,869`. O carimbo direto de pergunta já validada para em `juizes.py:1498`. | B |
| `passivo_validado.reparar` | Depois de `passivo_validado.py:399`, as perguntas e Empresas são relidas em `passivo_validado.py:400–404` e repassadas ao plantio por `passivo_validado.py:409,491,585`. A cauda `passivo_validado.py:447` usa outro atomic e pode abrir a própria trava em `models.py:2123`. | A na trava de `:399` |
| `relavra_via.desfazer_carimbo_sem_lastro` | A consulta `relavra_via.py:236–239` sucede `relavra_via.py:230` e junta só disputa e célula. A primeira leitura da Empresa acontece pelo C20, em `relavra_via.py:242` → `juizes.py:1041,1104–1107`. | C |
| `validacao.devolver_instante_sem_texto_l130` | Pergunta e Empresa carregadas depois de C em `validacao.py:465–467`. C20 via `validacao.py:468,434`. | A |
| `validacao._aceitar_a_folga` | O fecho principal de `validacao.py:563` e o de `validacao.py:588` via `declarar_dia.py:167,206` levam perguntas carregadas em `veredito_celula.py:180–181` e julgadas em `veredito_celula.py:193`, com FK fria, ou as perguntas e a Empresa de `models.py:364–387` no fecho de `veredito_celula.py:208`. Os carimbos `validacao.py:556,596` escrevem via de identidade (`models.py:3347,3491–3520`). | C/A |
| `validacao.validar_pergunta` | A pergunta de `validacao.py:730` sofre `refresh_from_db()` em `validacao.py:746` após C de `validacao.py:745`. A Empresa pode ser aquecida sob C pelo K6 de `validacao.py:782` → `materializacao.py:835`, antes da passagem ao plantio e aos sinais de batida do caminho `validacao.py:815,819`. Folga em `validacao.py:779` tem as entregas acima. `validacao.py:798` agenda `on_commit` (`fechamento.py:1848`), e `validacao.py:830` fica após o atomic local. | A/C |
| `validacao.validar_lote_classe_a` | A pergunta é relida em `validacao.py:1157–1158` após C de `validacao.py:1156`. `validacao.py:1162` calcula classe do lote sem passar por C20 (`validacao.py:1265–1301`), e a entrega ocorre pelo caminho `validacao.py:1165` → `acoes_disputa.py:21` → `validacao.py:746,815,819,830`. O atomic de `validacao.py:1153` abrange também essa cauda. | A/C |
| `views_cobrar.validar_inline` | Guarda `views_cobrar.py:224` usa `_d` relida em `views_cobrar.py:222`, após `views_cobrar.py:218`. Disputa de `fio.py:148–149,153–155`, perguntas via reverso `juizes.py:1600–1601` e Empresa inicialmente fria. `views_cobrar.py:226` carrega a pergunta pelo processo de validação e mantém o atomic externo até a cauda. | C na guarda, A/C na validação |
| `cluster_espurio.tratar_legado_encerrado_sem_lastro` | `cluster_espurio.py:281` relê o Chamado depois de `cluster_espurio.py:279`. O Colaborador direto de `models.py:1110` vai ao K7. O C20 vem das perguntas de `juizes.py:2523–2526`, chamadas por `juizes.py:2545` e `models.py:1109`, ou da ressuscitação em `models.py:1517,1549–1555,1599,1676` e migração em `models.py:1481` → `fio.py:2478–2484`. Nesses seletores o primeiro acesso a Empresa é frio. | C |
| `declarar_dia._escrever_folga` | O fecho `declarar_dia.py:206` carrega perguntas em `veredito_celula.py:180–181`, depois de C em `declarar_dia.py:190`, com FK fria para `veredito_celula.py:193`. A relavra síncrona de `declarar_dia.py:191` → `escala/folgas.py:333,337` → `ponto/portas/celula.py:392,479` usa `ec.colaborador` acessado depois de C. O seletor anterior do vínculo não faz join no colaborador (`escala/servico_jornada.py:46–48,64`, `escala/alimentacao.py:148–153`). Cartório e fecho de chamados têm as entregas frescas de `reconciliador.py:681–695`, `models.py:364–387` e frias de `cartorio.py:145–146,1424`. | C/A |
| `justificativa.encerrar_chamado_de_origem` | Após `justificativa.py:41`, `justificativa.py:42,46` repassa o Chamado. As perguntas com Empresa do gate vêm de `models.py:364–387`. A retenção de `reconciliador.py:451` usa `ignorar_tranca=True` em `juizes.py:2214`. | A, com A/C no S143 |
| `justificativa._encerrar_bloqueio_de_entrada` | Após `justificativa.py:80`, `justificativa.py:81,87` repassa o Chamado. A retenção ignora C20 (`juizes.py:2214`), e `reconciliador.py:543` chama `models.py:1779,1791,364–387` com Empresa em cache fresco. | A, com A/C no S143 |
| `veredito_celula.fechar_o_fio_do_dia` | Perguntas em `veredito_celula.py:180–181` são novas depois da trava C de `veredito_celula.py:176` e antes da segunda trava `veredito_celula.py:185`, que acrescenta Disputa e Chamado. C6 em `veredito_celula.py:193` acessa o dono lazy com Empresa fria. Fechos de `veredito_celula.py:208` carregam Empresa por `models.py:364–387` e abrem S143. | C/A |
| `veredito_celula.veredito_celula` | A view carrega `ponto/views.py:3497` e reutiliza o mesmo Colaborador em vários dias em `ponto/views.py:3518–3519`. A batida `veredito_celula.py:328` preserva a instância em `registro_batida.py:170–177`. Depois que o cartório aquece `c.empresa` numa data (`cartorio.py:1225,1783`), a batida numa data posterior pode reutilizar esse cache anterior à nova trava de `veredito_celula.py:231`. O sinal `ponto/signals.py:206–215` percorre `reconciliador.py:1581,1672,1741,866,869`, que entrega a mesma instância ao C20. `veredito_celula.py:352` também tem o caminho C/A do fecho direto. | B |

Em `materializacao._materializar_ausencia_de_resposta`, `materializacao.py:1151,1154,1155` acontece fora dos atomics de `materializacao.py:1074`. O chamador principal chama a absorção ao sair de seu próprio atomic (`disputa_emissao.py:1568,1596`). Em `materializacao.materializar_perguntas_validadas_da_disputa`, `materializacao.py:2293` também acontece depois do atomic local por pergunta. A trava de um chamador pode persistir nesses trechos, como na validação inline e em lote (`views_cobrar.py:214–226`, `validacao.py:1153–1165,830`).

Nos 16, B demonstrável em `_materializar_ausencia_de_resposta`, `materializar_perguntas_validadas_da_disputa` e `veredito_celula`. Execuções efetivas e frequência em produção [nao verificado].

## 26/11 11:12 · sessao 952a9a3ed210 · - · - · origem=Ronald (tmux)

`models.py` abaixo é `app/chamados/models.py`. As referências abreviadas de comandos ficam em `app/chamados/management/commands/`.

| Sítio | Instância e entrega |
|---|---|
| `app/api/foto_recebida.py:_validar_pelo_dono` | **C, com A na cascata.** A validação `:93` está sob a segunda aquisição `:90`. A primeira, `:73`, cobre a resposta automática, que relê e escreve sem C6. `validacao.py:726–730,746` carrega nova Pergunta sob C, usando a disputa antiga só como filtro. A cauda `validacao.py:830` → `materializacao.py:2293,2137` → `juizes.py:1600–1601` lê o colaborador da disputa recém-carregada. Empresa fria para o motivo foto de tipo escolha, com A no fecho via `_ancoradas`. `app/chamados/services/disputa_emissao.py:1746–1763`, `app/chamados/catalogo/perguntas.py:94–97`, `app/chamados/services/validacao.py:234,798`, `app/chamados/services/materializacao.py:825–835,1361,1772`, `models.py:364–365`. |
| `backfill_fio_morto_e4b.py:_executar_dia` | **A/C.** `:163` só roda no ramo `:161`. `:166,:169` herdam o C exterior de `:361–362,:404` também quando o ramo `:161` é pulado. A fábrica alcança Pergunta relida com `disputa` e `celula`, Empresa fria, em `app/chamados/services/fabrica_da_celula.py:207–209` → `app/chamados/services/relavra_via.py:230,236–242`. No plantio, a disputa semente sai de `:136–137` e a Pergunta é relida em `app/chamados/services/materializacao.py:2282`. A cauda `:2293,:2137` chama `app/chamados/juizes.py:1600–1601` ainda sob C exterior. Empresa pode estar fria ou ter sido acessada sob C no intervalo `materializacao.py:825–835`; o gate de chamado também carrega Empresa sob C em `models.py:364–365`. `:149–153` tem C6 em `disputa_emissao.py:1941` antes do `:163` e sob o C exterior, com Empresa fria inicialmente. |
| `backfill_fio_morto_e4b.py:handle` | **A/C.** `:361–362` mantém C no replanejamento e na execução. `:390` → `:90` envia ao lote o cadastro com Empresa relido em `app/chamados/services/tarefa_do_dia.py:528,554–555,837`; `:92–94` → `app/chamados/services/disputa_emissao.py:1941` usa Pergunta recém-carregada sem Empresa. `:397` repete o inventário de `:263`; `:404` chama `_executar_dia` acima. `:440` → `:81` → `app/chamados/services/vigia_fio.py:580–581,614` → `app/chamados/juizes.py:1751–1757` e `tarefa_do_dia.py:528,555` releem Empresa para os donos do plano, além de ler outros colaboradores no alcance global. O anexo omite `:406`, que pode executar deliberadamente o callback do beco sob C. `app/chamados/services/foto_e4b.py:51–54,75–80`, `app/chamados/signals.py:235–240`, `app/chamados/services/becos.py:35,86–92`. As fotos em `:365,:384,:407,:435` não entregam suas Perguntas ao C20. `foto_e4b.py:235–239,254–280,326–333`. |
| `backfill_fio_morto_e4b.py:_travar_colaboradores` | **C.** `:244` → `:252` → `:216–221` → `app/chamados/services/disputa_emissao.py:1917` → `models.py:1549–1555,1599`. A Consulta de ressuscitação carrega Pergunta, Disputa e Célula sob C. Colaborador lazy em `app/chamados/juizes.py:1041`, Empresa fria no primeiro par. |
| `estoque_l130_instante_sem_texto.py:handle` | **A.** Sob `:78`, Pergunta com Empresa em `:97–99` → `app/chamados/services/validacao.py:434`. `:105` delega e relê novamente com Empresa em `validacao.py:465,467–468`. |
| `reatar_beco_folga_ao_limbo.py:_rodar` | **D.** O pai relido sob `:112–113` está na fila, portanto em `COBRANCA`. A chamada `:118` → `app/chamados/services/limbo_folgas.py:238–244` não passa data. A colisão de data é pulada e a guarda de costura retorna antes de C6 para destino vivo. `app/chamados/services/painel.py:188–189`, `app/chamados/catalogo/motor.py:29–31`, `app/chamados/services/fio.py:924–936,959,1022,1049,1064`. |
| `validar_classe_a.py:handle` | **A/C.** Trava em `:124`, Pergunta de reclassificação em `:125`, classificador de resposta em `:128` sem C20. `:132` passa a disputa prévia somente como filtro. A cauda da validação e as cascatas ainda rodam no `atomic` exterior de `:118`. Pergunta nova `app/chamados/services/validacao.py:726–730,746`; Colaborador lazy `:234,798`; C20 via `:830` → `app/chamados/services/materializacao.py:2293,2137` → `app/chamados/juizes.py:1600–1601`. Empresa fria quando ainda não acessada, cache pós-C quando o intervalo lê K6 ou `_ancoradas` carrega o grafo no fecho. `validar_classe_a.py:37–42`, `juizes.py:2131–2149`, `materializacao.py:825–835`, `models.py:364–365`. |
| `models.py:ChamadoColaborador.abrir` | **C.** `:971–972` → `:988` → `:1085` → `:1517` → `:1636,1676` → `:1549–1555,1599`. A Pergunta da ressuscitação carrega pós-C, com Colaborador lazy e Empresa fria no primeiro par em `app/chamados/juizes.py:1041`. Redirecionamento para irmão em `models.py:1481` faz o mesmo com Perguntas de `app/chamados/services/fio.py:2478–2484`, sob `:2446`. A guarda direta de `models.py:1075` usa K7 em `:1099–1110`. |
| `models.py:ChamadoColaborador.ressuscitar_perguntas` | **C.** `:1636` é triagem antes de `:1655`; `:1676` reavalia o universo de `:1549–1555,1599`, cujo mapa começa sem sementes. Empresa fria no primeiro par, reutilização possível dentro do próprio laço. `models.py:1562,1570–1574`, `app/chamados/juizes.py:1041,1117,1275–1283`. |
| `models.py:DisputaSupervisao.fechar` | **B demonstrável**, com C se a FK segue fria e A quando a foto veio de C exterior. `:2097` carrega Perguntas via reverso `self.perguntas`; `.disputa` leva ao mesmo `self` nas guardas `:2116` e `:2138`. `:2130` atualiza campos de estado da disputa. O C20 de triagem preenche a Empresa antes de `:2123`; caso externo concreto em `encerrar_perguntas_com_fato.py:82–89`, que faz `fio_vivo` no mesmo d antes de chamar `fechar`. Caminho A via E2 em `app/chamados/reconciliador.py:679,681–683,707,719`. `models.py:2045,2107,2688–2691`, `app/chamados/juizes.py:1041,1117`. |
| `models.py:DisputaSupervisao.remover_motivo` | **D no escopo local, C com C exterior retido.** `:2253,2259` têm o `atomic` que termina após `:2296`; o carimbo de identidade `:2292` grava sem C6 (`:3491–3521`). `:2303` fica fora do `atomic` local e `fechar` tem aquisição própria em `:2123`. Em chamada por `app/colaboradores/services/estrutura.py:66–77` e `app/colaboradores/signals.py:71–77`, o `atomic` exterior retém C para a cauda, cuja disputa nasce do queryset simples do receiver. A entrada `app/colaboradores/views.py:893–913` → `estrutura.py:82–86` não abre esse `atomic` exterior. `models.py:2097,2107,2116`. |
| `models.py:DisputaSupervisao.reabrir_por_colab` | **C.** `:2469` → `:2481` chama novamente o juiz de `app/chamados/juizes.py:1853–1854`, que carrega Pergunta pelo manager global. `:1595` chama C6 para a materializada. Colaborador lazy em `:1041`, Empresa fria. |
| `models.py:PerguntaDisputa._nascer` | **A.** C em `:3123` via `:2898`; `ex` vem do universo pós-C `:3185` → `app/chamados/services/fio.py:2144–2145,1776–1778` → `models.py:2995–3001`, com `disputa__colaborador`, ou do novo `get` em `:3235`. No ramo de destino terminal, `:3258` → `fio.py:930` lê Empresa sob C antes de entregar a mesma Pergunta em `:936`. Destino vivo retorna em `fio.py:926–927`; nascimento novo em `models.py:3292–3297` não passa por essa guarda. |

## 26/11 11:10 · sessao 5986b5cfd949 · O30-F3-CONSTRUIR · verificador · origem=Code

**O dinheiro muda quando a cura grava uma falta aprovada sem fim e com minutos zerados.** A c5 entrega o fim ao `Ausencia.save` antes da soma. Com isso, a R8 lê o intervalo inteiro, enquanto a base somava apenas o primeiro dia nessa chamada. Também ficaram duas regras para proteger o anexo compartilhado e uma janela de concorrência entre a consulta ao K6 e a exportação. Quantidade de linhas e valor em produção **[nao verificado]**. (`app/ponto/services/ausencia.py:1924`, `app/ponto/services/ausencia.py:1934`, `app/ponto/services/ausencia.py:1942`, `app/ponto/models.py:901`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:618`, `app/ponto/services/ausencia.py:312`, `app/ponto/services/ausencia.py:1175`, `app/folha/models.py:174`)

A cópia corresponde aos trechos c5 inspecionados. Ela veio sem `.git`, então a identidade do commit e a igualdade integral de `0191ece77..aa32babb3` com o anexo ficam **[nao verificado]**. O diff colado também omite a remoção da tag e da biblioteca narrada pelo construtor. Conferi o estado final do template e as referências na cópia. (`app/ponto/services/ausencia.py:1942`, `app/templates/ponto/partials/painel_ausencia.html:1`, `app/ponto/views.py:2103`)

## 1. PROCEDE — escritores e leitores

O registro vivo atribui a cobertura e os dias ocupados a `ponto/turnos.py`, os minutos previstos a `escala/utils.py`, a exigência do documento a `estado_inicial` e os estados abertos a `ABERTAS`. Para dinheiro, o recorte é a janela da empresa e o juiz da entrega é o K6. (`app/docs/JUIZES.md:35`, `app/docs/JUIZES.md:40`, `app/docs/JUIZES.md:58`, `app/docs/JUIZES.md:131`, `app/core/juizes.py:193`, `app/core/juizes.py:259`, `app/core/juizes.py:349`)

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `data_fim` | `criar_ausencia` calcula e insere, `editar_ausencia_registro` calcula e grava, `rederivar_fim` grava o fim conferido, `Ausencia.save` preenche fim ausente, `mudar_estado` persiste a derivação do modelo. (`app/ponto/services/ausencia.py:425`, `:569`, `:660`, `:740`, `:763`, `:1102`, `:1942`; `app/ponto/models.py:918`) | Cobertura, sobreposição e expansão por dia, cálculo de minutos no modelo, insumo e invalidação da folha, rateio de faltas, signal do cartório, recorte do lote, histórico e painel. (`app/ponto/turnos.py:2540`, `:2555`, `:2796`, `:2851`, `:2870`; `app/ponto/models.py:906`; `app/ponto/services/ausencia.py:119`, `:189`, `:1924`; `app/folha/export.py:63`, `:87`, `:117`; `app/ponto/signals.py:115`; `app/ponto/services/atestados_lote.py:55`; `app/ponto/services/dia_decidido.py:681`; `app/templates/ponto/partials/painel_ausencia.html:159`) |
| `minutos` | Entrada de criação e edição, derivação R8 de falta aprovada com zero no modelo, persistência pelas portas de estado e cura. H06 também chega pela edição. (`app/ponto/services/ausencia.py:569`, `:771`, `:1102`, `:1943`; `app/ponto/models.py:901`; `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:164`) | Juiz de parcialidade, insumo da folha, rateio e total de faltas, cartão/histórico/CSV, API e painel. (`app/ponto/turnos.py:2617`; `app/ponto/services/ausencia.py:119`; `app/folha/export.py:121`, `:140`; `app/ponto/services/fechamento.py:893`; `app/ponto/services/dia_decidido.py:551`, `:578`; `app/ponto/services/atestados_lote.py:149`; `app/api/views.py:1879`; `app/templates/ponto/partials/painel_ausencia.html:160`) |
| `dias_corridos` | Criação e edição recebem a duração, `Ausencia.save` ajusta pelo intervalo preenchido, estado e cura persistem a coluna. O espelho de férias nasce pela criação com um dia. (`app/ponto/services/ausencia.py:569`, `:764`, `:1102`, `:1561`, `:1943`; `app/ponto/models.py:917`) | Modelo e comando de cura leem a duração para propor fim. Histórico usa a duração gravada quando o fim é nulo. Painel mostra a duração e a usa no checkbox. CSV recebe `dias` do histórico. (`app/ponto/models.py:917`; `app/ponto/management/commands/curar_ausencia_sem_fim.py:29`; `app/ponto/services/ausencia.py:1924`; `app/ponto/services/dia_decidido.py:686`; `app/templates/ponto/partials/painel_ausencia.html:158`, `:194`; `app/ponto/services/atestados_lote.py:149`) |
| `aprovada_em` e `atualizado_em` | Criação de aprovada e decisão explícita escrevem `aprovada_em`. O modelo o preenche quando falta em aprovada ou rejeitada. `atualizado_em` é `auto_now` e entra nos `update_fields` da edição, decisão com período, rejeição de férias e cura. (`app/ponto/services/ausencia.py:576`, `:772`, `:1253`, `:1266`, `:1668`, `:1943`; `app/ponto/models.py:834`, `:838`, `:896`) | `quem_quando` alimenta cartão, histórico e CSV. O painel lê `aprovada_em` e o cartório consulta os dois carimbos para detectar fato novo. H06 lê `atualizado_em` na reversão. (`app/ponto/services/dia_decidido.py:558`, `:573`; `app/ponto/services/atestados_lote.py:152`; `app/templates/ponto/partials/painel_ausencia.html:310`; `app/ponto/services/cartorio.py:401`; `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:151`) |
| `documento_referencia`, `documento` e os seis campos de atestado | Referência no INSERT, com `SET_NULL` na exclusão da raiz. Arquivo na criação, em `anexar_documento` e em `_reabrir`. Os seis campos entram no INSERT ou no `UPDATE` de `_gravar_dados_atestado`, chamado pelas portas de criação, edição, anexo e reabertura. (`app/ponto/models.py:815`; `app/ponto/services/ausencia.py:326`, `:572`, `:575`, `:580`, `:652`, `:664`, `:774`, `:1180`, `:1183`, `:1472`, `:1476`) | Raiz e irmãs na porta, gate de aprovação, comando de rejulgamento, drawer, visualização protegida, aprovar/rejeitar e lista. Lote lê a raiz para extensão, metadados CSV/PDF e bytes PDF/ZIP. Histórico lê tipo e presença do arquivo na raiz. API, troca de plantão e rótulo do dia também têm leitores do arquivo próprio. (`app/ponto/services/ausencia.py:283`, `:803`, `:869`, `:890`, `:1122`; `app/ponto/management/commands/rejulgar_status_por_tipo.py:28`, `:38`; `app/ponto/views.py:2103`, `:2148`, `:2787`, `:2863`; `app/ponto/views_atestado.py:30`; `app/templates/ponto/ausencias.html:170`; `app/templates/ponto/partials/painel_ausencia.html:71`, `:99`, `:125`, `:162`; `app/ponto/services/atestados_lote.py:107`, `:119`, `:178`, `:255`; `app/ponto/services/dia_decidido.py:477`, `:555`, `:674`, `:688`; `app/api/views.py:1061`; `app/ponto/portas/celula.py:726`, `:746`) |

## 2. PROCEDE — classes com resíduos

| Classe | Veredito |
|---|---|
| **Segundo juiz** | **PROCEDE.** `servir_documento_ausencia`, aprovar, rejeitar e a lista ainda resolvem o elo por conta própria. Em `A → B → C`, o drawer recusa `A` porque `B` também aponta para outra linha. A entrega protegida tenta servir o arquivo de `B`, podendo devolver bytes ou 404. Também há duas regras para substituir arquivo compartilhado. A guarda da raiz só considera `aprovada`. A guarda de irmãs considera `aprovada` e `rejeitada`, só em anexo próprio. Uma filha aberta pode substituir o arquivo de uma raiz rejeitada se nenhuma filha estiver decidida. O alcance desse caso no estoque fica **[nao verificado]**. (`app/ponto/services/ausencia.py:283`, `:312`, `:1171`, `:1175`; `app/ponto/views.py:2103`, `:2148`, `:2787`, `:2863`; `app/templates/ponto/ausencias.html:170`) |
| **Segundo escritor fora da porta** | **NAO PROCEDE na cópia.** A varredura estática de produção com `detalhar_saves=True` retornou zero escritas atribuídas a `Ausencia` fora do serviço e do modelo. O admin é de leitura e `SET_NULL` é a ação declarada da FK. (`app/core/censo_escritas.py:396`, `:455`; `app/ponto/admin.py:38`; `app/ponto/models.py:815`, `:892`; `app/ponto/services/ausencia.py:1943`) |
| **Relógio fora da borda** | **PROCEDE como resíduo.** A leitura nova da c4 saiu de `rederivar_fim`. A cura ainda passa pelo `_tz.now()` do modelo se faltar `aprovada_em`, pelo `auto_now` e, quando o insumo anterior tinha fim nulo, pelo `localdate()` de `_acordar_projecao`. O `post_save` síncrono do cartório também lê `localdate()`. O default de mês/ano do lote é lido na view e entregue explicitamente ao serviço. (`app/ponto/services/ausencia.py:177`, `:190`, `:1943`; `app/ponto/models.py:838`, `:896`; `app/ponto/signals.py:115`, `:143`; `app/ponto/views_atestado.py:103`; `app/ponto/services/atestados_lote.py:35`) |
| **Derivação paralela** | **PROCEDE para o fim residual.** A cópia c4 de **minutos, dias corridos, carimbo e validação** saiu da cura. No caminho com fim proposto preenchido, o modelo usa esse fim. A fórmula do **fim** continua em três lugares, `derivar_data_fim`, o `elif` de `Ausencia.save` e a prévia do comando. Uma alteração na função real pode fazer o dry anunciar outro fim. O duplo cálculo no caminho da cura com fim preenchido **NAO PROCEDE**. (`app/ponto/services/ausencia.py:198`, `:1924`, `:1942`; `app/ponto/models.py:918`, `:926`; `app/ponto/management/commands/curar_ausencia_sem_fim.py:37`) |
| **Selo ou RED sem mordida** | **PROCEDE nas fronteiras.** Os 46 testes sem banco passaram. Rodei `d5_ignora_fim_cercado`, os cinco mutantes `c5_` do trecho final, `save_integral`, `d4_append` e `d4_atribuicao_em_ramo`. Todos deram `FAIL` por asserção. `d1_zip_le_filha` e `d1_historico_le_filha` também deram `FAIL`, mas a fixture injeta uma filha diretamente e os dois alimentadores de produção selecionam só raízes. O selo de referência verifica atribuição direta e `.update()` com keyword literal dentro do serviço. O do painel verifica presença textual e atribuição a `raiz`, sem renderizar nem exigir que a view entregue a raiz correta. REDs dependentes de commits e suíte com banco **[nao verificado]**. (`app/ponto/tests/mutantes_atestado_f3.py:134`, `:204`, `:218`, `:230`, `:380`; `app/ponto/tests/test_atestado_f3_contrato.py:241`, `:257`, `:273`, `:624`, `:735`, `:747`; `app/ponto/services/dia_decidido.py:624`; `app/ponto/services/atestados_lote.py:50`; `app/ponto/views.py:2042`) |
| **Ordem de trava** | **NAO PROCEDE** uma inversão demonstrada no grupo do documento. O caminho trava raiz, registro solicitado e, quando necessário, filhas por PK. **PROCEDE** a janela de concorrência com exportação. A cura trava `Ausencia`, consulta K6 e só depois salva. A emissão do TXT pode registrar uma exportação entre a consulta e o save, pois o registro da exportação usa outro conjunto de linhas, sem exclusão comum com a cura. Ocorrência em produção **[nao verificado]**. (`app/ponto/services/ausencia.py:296`, `:1122`, `:1921`, `:1934`, `:1943`; `app/folha/export.py:588`; `app/folha/views.py:270`, `:299`; `app/folha/models.py:174`) |
| **Pouso antes do pré-requisito** | **PROCEDE como pendência de verificação.** A mudança de leitura pressupõe estoque sem cadeia. O anexo relata zero cadeias de segunda mão, sem trazer a sonda primária. Uma cadeia legada pode derrubar o drawer ou abortar um comando de acervo depois de outras linhas já processadas. A migration `0079` está na cópia e o deploy prevê migrar antes do reload. Aplicação da migration, suíte inteira verde, cadeia em produção e pouso efetivo **[nao verificado]**. (`app/ponto/services/ausencia.py:290`, `:1921`; `app/ponto/views.py:2103`; `app/ponto/management/commands/curar_ausencia_sem_fim.py:47`; `app/ponto/migrations/0079_ausencia_atestado_dados.py:8`; `bin/deploy.sh:316`; `CLAUDE.md:398`) |
| **ESMERIL-SEMANTICA** | **PROCEDE para fim nulo.** O checkbox chama `NULL` com `dias_corridos > 1` de indeterminado. A cura usa exatamente essa forma como fim faltante quando o tipo admite fim, e a cobertura trata a falta sem fim como dia único até a cura. Para `data_fim` preenchida, **NAO PROCEDE** a divergência inclusiva/exclusiva nos caminhos auditados. Modelo, cobertura e cerca incluem a última data. A parcial noturna ancora `hora_ini` e `hora_fim` no dia de início do turno. A R8 caminha pelas datas do intervalo e pergunta o previsto da jornada ancorada em cada data, inclusive quando ela cruza meia-noite. (`app/templates/ponto/partials/painel_ausencia.html:194`; `app/ponto/models.py:751`, `:918`; `app/ponto/management/commands/curar_ausencia_sem_fim.py:29`; `app/ponto/turnos.py:2555`, `:2644`, `:2851`; `app/ponto/services/ausencia.py:1934`; `app/escala/utils.py:2320`) |

## 3. NAO PROCEDE — leitor órfão pela L-111

A busca por `_fixar_raiz_documento` deixou só a lápide. A busca por `ausencia_tags`, pelo antigo uso da tag e por imports correspondentes deu zero na cópia. O único render do partial entrega `raiz` no contexto, e o partial lê essa variável diretamente. Isso cobre o vínculo estático entre view e template. Renderização nas duas cascas **[nao verificado]**. (`app/docs/LEIS.md:137`, `app/ponto/services/ausencia.py:304`, `app/ponto/views.py:2103`, `app/ponto/views.py:2108`, `app/templates/ponto/partials/painel_ausencia.html:1`, `app/templates/ponto/partials/painel_ausencia.html:71`)

## 4. PROCEDE — mudança de insumo de folha

- **Chamador:** o único chamador de produção localizado é `curar_ausencia_sem_fim --apply`, com `somente_sem_fim=True`. A configuração o declara fora do pipeline, como ato de acervo. Sem `--apply`, o comando só imprime. Os signals são consumidores do `save`. Frequência real de execução, execução manual anterior e agenda instalada **[nao verificado]**. (`app/config/crons.py:769`, `app/config/crons.py:1302`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:29`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:42`, `app/ponto/signals.py:143`, `app/ponto/signals.py:275`)
- **Universo:** falta aprovada, `data_fim IS NULL`, `dias_corridos > 1` e fim retornado pela função real. O número gravado de minutos só muda se era zero e a soma prevista for positiva. A diferença **c5 versus base** exige minutos zerados e previsto positivo depois do primeiro dia. Mesmo com minutos positivos já gravados, a cura amplia o intervalo que a folha usa no rateio. `dias_corridos` já coincide com a duração do fim derivado nesse recorte. Um fim já preenchido é preservado na chamada genérica e é excluído da chamada do comando. A coluna sozinha não identifica a origem desse fim. (`app/ponto/management/commands/curar_ausencia_sem_fim.py:29`, `:48`; `app/ponto/services/ausencia.py:198`, `:1924`, `:1942`; `app/ponto/models.py:901`, `:917`; `app/folha/export.py:113`, `:121`; `app/ponto/tests/test_atestado_f3_contrato.py:747`)
- **Exportadas:** se qualquer dia do intervalo cair em exportação vigente pelo K6, a porta retorna antes de gravar a ausência inteira, inclusive quando os demais dias pertencem a uma competência aberta. A `GUARDA_COMPETENCIA` atua em outro ponto e na cópia só lista 09/2026. Para uma cura admitida, `_acordar_projecao` invalida o carimbo. O fechamento posterior pode consumir o intervalo e os minutos novos pelo rateio, se o catálogo classificar o tipo como desconto. A janela de concorrência do item 2 impede afirmar proteção atômica entre exportar e curar. Valores gravados, valor em reais e efeito em TXT de produção **[nao verificado]**. (`app/ponto/services/ausencia.py:1930`, `:1945`; `app/chamados/services/vigia_fio.py:814`; `app/folha/export.py:580`, `:138`; `app/ponto/services/fechamento.py:36`, `:578`, `:893`, `:1676`; `CLAUDE.md:422`)

## 5. PROCEDE — sonda de leitura entregue

O script lê o schema `juliani`, recorte que o backup da sombra restaura. Use `python3 -` a partir de `app/`, com o bloco na entrada padrão. `colNNN` usa o PK com pelo menos três dígitos. A saída conta cada ausência uma vez por competência tocada e mostra 11/12 de 2026 e todas as competências efetivamente entregues pelo K6, com zeros por empresa. `fim_gravado` é o cenário de chamada direta, separado de `cura_sem_fim`, que é o caminho do comando. As linhas barradas aparecem com a proposta simulada. O script desativa a abertura dos logs de arquivo antes do `django.setup()`. Execução na sombra **[nao verificado]**. (`bin/sombra.sh:244`, `bin/sombra_mascara.py:64`, `app/config/settings/sombra.py:47`, `app/config/settings/sombra.py:60`, `app/config/settings/saas.py:136`, `app/config/settings/saas.py:160`, `app/ponto/services/dia_pago.py:702`, `app/folha/export.py:556`, `app/ponto/services/ausencia.py:1924`)

```python
import os
import sys
from collections import Counter, defaultdict

sys.dont_write_bytecode = True
os.environ.setdefault("DJANGO_SETTINGS_MODULE", "config.settings.sombra")

import django
from django.conf import settings

assert settings.HASNER_SOMBRA is True
assert settings.DATABASES["default"]["NAME"] == "sombra"
assert settings.DATABASES["default"]["HOST"] == "sombra_db"
settings.LOGGING_CONFIG = None
django.setup()

from django.db import connection, transaction
from django_tenants.utils import schema_context

from colaboradores.models import Empresa
from escala.utils import minutos_previstos_do_dia
from folha.export import competencia_entregue
from folha.models import ExportacaoDominio
from ponto.models import Ausencia
from ponto.services.ausencia import derivar_data_fim
from ponto.services.dia_pago import competencias_do_intervalo
from ponto.turnos import dias_da_ausencia

meses_alvo = {(11, 2026), (12, 2026)}
modos = ("cura_sem_fim", "fim_gravado")
totais = defaultdict(Counter)

with schema_context("juliani"), transaction.atomic(durable=True):
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
        cursor.execute("SELECT current_database()")
        assert cursor.fetchone() == ("sombra",)

    empresas = list(Empresa.objects.order_by("pk").values_list("pk", flat=True))
    triplas = ExportacaoDominio.objects.order_by().values_list(
        "empresa_id", "mes", "ano"
    ).distinct()
    entregues = {tripla for tripla in triplas if competencia_entregue(*tripla)}
    recorte = entregues | {
        (empresa, mes, ano) for empresa in empresas for mes, ano in meses_alvo
    }

    ausencias = (
        Ausencia.objects.filter(tipo="falta", status="aprovada", dias_corridos__gt=1)
        .select_related("colaborador__empresa", "documento_referencia")
        .order_by("pk")
    )
    for a in ausencias.iterator(chunk_size=500):
        fim = a.data_fim or derivar_data_fim(a.tipo, a.data, a.dias_corridos, False)
        if fim is None or fim < a.data:
            continue

        empresa = a.colaborador.empresa
        comps = {
            (empresa.pk, mes, ano)
            for mes, ano in competencias_do_intervalo(a.data, fim, empresa)
        }
        exibidas = comps & recorte
        if not exibidas:
            continue

        modo = "cura_sem_fim" if a.data_fim is None else "fim_gravado"
        entregues_no_intervalo = comps & entregues
        referencia_invalida = (
            a.documento_referencia_id is not None
            and a.documento_referencia.documento_referencia_id is not None
        )
        dias = dias_da_ausencia(a.tipo, a.data, fim, a.data, fim)
        previsto = (
            [minutos_previstos_do_dia(a.colaborador, dia) for dia in dias]
            if not a.minutos else []
        )
        minutos_r8 = sum(previsto) if previsto else a.minutos
        muda_minutos = minutos_r8 != a.minutos
        muda_dias = len(dias) != a.dias_corridos
        muda_fim = fim != a.data_fim
        afetada = muda_minutos or muda_dias or muda_fim
        delta_c5 = sum(previsto[1:]) if modo == "cura_sem_fim" else 0
        passaria = not referencia_invalida and not entregues_no_intervalo

        for chave in exibidas:
            n = totais[chave, modo]
            n["candidatas"] += 1
            n["mudam_fim"] += muda_fim
            n["mudam_dias_corridos"] += muda_dias
            n["mudam_minutos"] += muda_minutos
            n["minutos_c5_diferem_da_base"] += delta_c5 != 0
            n["afetadas_barradas_k6"] += bool(afetada and entregues_no_intervalo)
            n["afetadas_ref_invalida"] += bool(afetada and referencia_invalida)
            n["afetadas_passariam"] += bool(afetada and passaria)

        if afetada:
            rotulos = lambda chaves: ",".join(
                f"{mes:02d}/{ano}"
                for _, mes, ano in sorted(chaves, key=lambda k: (k[2], k[1]))
            ) or "-"
           

[truncado em 20000 de 21177 caracteres]

## 26/11 11:08 · sessao 86c45000af0e · R1-SEM-DONO-2611 · leitura · origem=Code

Nas seis chaves, o E6 selecionou `minutos_realizados` e calculou **piso − realizado**. O teto é a soma dos vãos dos trechos e só entra na classificação de faixa quando há trecho ímpar. As seis chaves têm `impares=false`, `guarda_janela=false` e dono `ESTRUTURA`. A correspondência com os deltas publicados do R1 e a identidade da foto lida estão **[nao verificado]**. `SONDA1/roda.out:15–21` · `app/ponto/management/commands/e6_oraculo.py:263–269,339–404` · `app/ponto/calculador/nucleo.py:235–254`

## 1. Ramos que produziram os seis deltas

O E6 recebeu os instantes apuráveis em hora local, sem `pares` nem `envelopes`. Nesse ramo, `brutas` conta por data civil, `horas` agrupa pela data da primeira batida do trecho, o piso soma os pares por posição e o teto soma `última − primeira` de cada trecho. No espelho, o realizado agrupa pelo `data_turno` do juiz. Um carimbo de 60 confirma a passagem pelo ramo de desconto da janela declarada. Um carimbo de zero deixa em aberto qual ramo do realizado forneceu o saldo. `app/ponto/management/commands/e6_oraculo.py:149–154` · `app/ponto/calculador/nucleo.py:157–159,178–254` · `app/ponto/services/espelho.py:625–628,689–690,801,835–837` · `app/ponto/turnos.py:719–771`

| Dia de 2026 | Realizado · brutas · piso / teto · delta em min | O que fecha pelo dado e o que falta medir |
|---|---|---|
| **col114 15/11** | **499 · 4 · 439,847646 / 499,385183 · −59,152354 → −59** | O E6 tem um trecho com dois pares, `09:58→15:05` e `16:05→18:17`. Teto menos piso dá **59,537537 min**. O realizado coincide com as bordas `09:58→18:17` truncadas ao minuto e traz janela **0**. As bordas efetivamente escolhidas e o reconhecimento de `15:05→16:05` como pausa pelo juiz estão **[nao verificado]**. Ler `data_turno`, `_fim_fechado`, a pausa valorizada e `borda_posicional`. `SONDA1/roda.out:15` · `app/ponto/calculador/nucleo.py:235–254` · `app/ponto/turnos.py:542–546,605–622,719–771` |
| **col130 29/10** | **335 · 1 · 671,905433 / 729,936250 · +336,905433 → +337** | O E6 soma `18:56→00:33` e `01:31→07:06` no trecho chaveado em **29/10**. Três pontas são de 30/10. O espelho descontou **60** de janela. Repondo esses 60, o realizado fica em 395 e sobra uma diferença de **276,905433 min** para o piso. A atribuição e o fim contado de cada turno em 29 e 30/10 estão **[nao verificado]**. `SONDA1/roda.out:16` · `app/ponto/calculador/nucleo.py:235–254` · `app/ponto/turnos.py:285–324,733–771` |
| **col220 10/11** | **337 · 1 · 430,420683 / 486,063217 · +93,420683 → +93** | O E6 soma `21:54→03:35` e `04:31→06:00` no trecho chaveado em **10/11**. Três pontas são de 11/11. O espelho descontou **60** de janela. Repondo esses 60, o realizado fica em 397 e sobra **33,420683 min** para o piso. O papel da ata para `03:35`, `04:31` e `06:00`, o fim contado e o dia de cada turno estão **[nao verificado]**. `SONDA1/roda.out:17` · `app/ponto/calculador/nucleo.py:235–254` · `app/ponto/turnos.py:609–622,733–771,1940–1944` |
| **col874 22/10** | **667 · 3 · 382,822567 / 382,822567 · −284,177433 → −284** | O E6 soma apenas `15:45→22:07` na chave de 22/10. A terceira batida civil foi agrupada em chave anterior. O espelho descontou **60** de janela e ainda computou 667, acima da duração do único par do E6 na chave. O destino da terceira batida, as bordas que entraram nos 667 e a contribuição de cada turno estão **[nao verificado]**. `SONDA1/roda.out:18` · `app/ponto/calculador/nucleo.py:215–247` · `app/ponto/turnos.py:719–771` |
| **col989 23/10** | **721 · 2 · 567,370050 / 567,370050 · −153,629950 → −154** | O E6 soma um único par, `08:32→17:59`. O realizado com janela **0** excede em **153,629950 min** todo o trecho do E6. As mesmas duas bordas contadas uma vez não produzem 721. A borda efetiva, eventual turno adicional e sua presença nos cartões estão **[nao verificado]**. `SONDA1/roda.out:19` · `app/ponto/calculador/nucleo.py:235–247` · `app/ponto/turnos.py:719–771` · `app/ponto/services/espelho.py:465–474,778–801` |
| **col996 10/11** | **443 · 5 · 1026,385217 / 1116,385217 · +583,385217 → +583** | O E6 abre dois trechos na mesma chave. O primeiro soma `06:57→13:00` e `14:30→15:50`, com vão interno de **90 min**. A batida de `21:16` entra **mais de 14 h** após `06:57`, com gap de cerca de **5 h 26** desde `15:50`. Assim, o corte vem do **vão total** com quatro pontas, o que explica `cortes=false`. O segundo trecho soma `21:16→06:59` na chave de **10/11**. O delta tem a ordem de grandeza desse par inteiro. As pontas efetivamente contadas nos 443 e o destino do par noturno no juiz estão **[nao verificado]**. `SONDA1/roda.out:20` · `app/ponto/calculador/nucleo.py:215–254` · `app/ponto/turnos.py:719–771` |

## 2. Encaixe nas obras

- **Pausa.** Col114 15/11 tem o contraste aritmético da família **C**. Em col130, col220 e col874, **C** aparece no ramo do espelho como desconto comprovado de 60, com diferença residual em todos os três. A L-032 e o corte de **14/09** regem a pausa valorizada e a janela declarada. `INTERVALO-PRE-ASSINALADO-CONTRATO` V2 trata a dedução no motor e a intrajornada pela L-031/L-032. A aplicação dessa obra aos deltas está **[nao verificado]**. `SONDA1/roda.out:15–18` · `app/docs/LEIS.md:79–80` · `app/ponto/turnos.py:707–717,756–771` · `app/docs/BACKLOG.md:2554`
- **Dia e pareamento.** Col130, col220, col874 e col996 mostram o problema de **comparar chaves civis e de turno**, família **B**, com a chave do E6 demonstrada e a do juiz **[nao verificado]**. Para col996, o **corte por vão** do lado E6 também tem família expressamente registrada em `ORACULO-E6-43-DEFEITOS`. A L-085 rege o dia do plantão. `MOTOR-LE-O-TURNO-DIA-DO-PAR` trata a herança de 0–6 h na lavra/FT. `NOTURNO-A-JANELA-NO-DIA-DO-TURNO-2411-0210` trata a âncora da janela de HE pela L-085/L-097. `PAR-PELO-TURNO-V6-2311-1958` trata a separação da jornada pelo teto `previsto + 4 h` do pareador. O corte comprovado de col996 é o **vão de 14 h do E6**. O nexo de cada delta com essas obras do motor ou do pareador está **[nao verificado]**. `SONDA1/roda.out:16–18,20` · `app/docs/LEIS.md:115–117,154` · `app/docs/GRADE-MESTRA.md:303` · `app/docs/BACKLOG.md:1989,1991,2020` · `app/docs/CORTES.md:646` · `app/ponto/calculador/nucleo.py:215–224` · `app/ponto/motor_calculo_v2.py:1545–1567,2415–2429`
- **Geometria, previsão e abono.** Col989 tem separação de geometria a apurar, família **A**. O encaixe em **D** depende de encontrar a diferença entre os turnos somados e os cartões. A **L-115** é o corte aplicável se aparecer borda posicional após pausa fechada. `O327` trata falha de lavratura do oráculo de folha, `E1-PREVISAO-24H` trata previsão inválida e `ABONADO-COM-SAIDA-ANTECIPADA` trata desconto de pontualidade em dia coberto pelo abono, pelo corte `ABONADO-COM-SAIDA-NA-ZONA-INVIOLAVEL`. Esses três mecanismos estão **[nao verificado]** nas seis chaves. Em todas elas, a fonte selecionada foi o realizado. `SONDA1/roda.out:15–20` · `app/docs/LEIS.md:71,93,153` · `app/docs/BACKLOG.md:1793,1837,2006` · `app/docs/RELATO.md:7039` · `app/ponto/services/espelho.py:465–474,689–690,778–801`

## 3. Sonda única de leitura

O bloco é autocontido dentro de um Django já inicializado em sombra. Usa a janela visual do espelho para o juiz de turno declarado em `core/juizes.py`, e imprime o alvo e as datas vizinhas. Fixei `hoje` e `agora` nos valores do rodapé fornecido. A execução e a equivalência da foto estão **[nao verificado]**. O bloco não chama o wrapper que prepara credenciais e grava trilha. `SONDA1/roda.out:21` · `app/config/settings/sombra.py:49–60` · `app/core/juizes.py:215–216` · `app/ponto/services/espelho.py:625–649,1298–1302,1330–1332` · `bin/sonda_leitura.sh:126–142,235–250`

```python
import datetime as dt
import json
import logging
import sys
from contextlib import ExitStack

sys.dont_write_bytecode = True

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone
from django_tenants.utils import schema_context

from colaboradores.models import Colaborador
from escala.alimentacao import celulas_do_periodo, escalas_do_periodo, vinculo_do_dia
from ponto import turnos as juiz
from ponto.calculador.nucleo import minutos_do_oraculo
from ponto.janelas import janela_fechamento
from ponto.management.commands.e6_oraculo import (
    InsumosDoDono,
    diferenca_e_a_janela_declarada,
    minutos_do_espelho,
)
from ponto.services.dia_pago import _dia_de
from ponto.services.espelho import espelho_do_colab

assert getattr(settings, "HASNER_SOMBRA", False) is True
assert not connection.in_atomic_block

ALVOS = (
    (114, "2026-11-15"), (130, "2026-10-29"), (220, "2026-11-10"),
    (874, "2026-10-22"), (989, "2026-10-23"), (996, "2026-11-10"),
)
HOJE = dt.date(2026, 11, 26)
AGORA = dt.datetime.fromisoformat("2026-11-26T11:01:55.389818-03:00")
UM_DIA = dt.timedelta(days=1)


def emitir(**dados):
    print(json.dumps(dados, ensure_ascii=True, default=str))


def local(instante):
    return timezone.localtime(instante).isoformat() if instante is not None else None


def meia_noite(data):
    return timezone.make_aware(dt.datetime.combine(data, dt.time.min))


def min_entre(inicio, fim):
    return (fim - inicio).total_seconds() / 60


def medir(c, alvo):
    chave = f"col{c.pk} {alvo}"
    dias = (alvo - UM_DIA, alvo, alvo + UM_DIA)
    ini, fim = janela_fechamento(11, 2026, c.empresa)
    esp = espelho_do_colab(c, HOJE, ini, fim, agora=AGORA)
    vis_ini, vis_fim = esp["data_ini"], esp["data_fim"]
    linhas = {d["data"]: d for d in esp["dias"]}
    traco = {}

    # Observa os trechos e o papel efetivos, sem refazer o corte ou o pareamento.
    def observar(frame, evento, retorno):
        if evento != "return":
            return
        if frame.f_code is minutos_do_oraculo.__code__:
            traco["trechos"] = frame.f_locals["turnos"]
        if frame.f_code is juiz._turnos_do_colab_puro.__code__:
            traco["papel"] = frame.f_locals["_papel"]
            traco["batidas_juiz"] = frame.f_locals["bs"]

    with ExitStack() as restaurar:
        restaurar.callback(sys.setprofile, sys.getprofile())
        sys.setprofile(observar)
        ins = InsumosDoDono(c, ini, fim, HOJE, espelho=esp, agora=AGORA)
        turnos = juiz.turnos_do_colab(c, vis_ini, vis_fim)

    v = ins.oraculo
    apelidos = {}
    ecos = {b.pk for b in traco["batidas_juiz"] if getattr(b, "_eco_flush", False)}

    def batida(b):
        if b is None:
            return None
        return {
            "id": apelidos.setdefault(b.pk, f"b{len(apelidos) + 1}"),
            "instante_local": local(b.timestamp),
            "tipo": b.tipo,
            "papel_ata": juiz._papel_da_ata(b, traco["papel"]),
            "eco_flush": b.pk in ecos,
            "is_intervalo": b.is_intervalo,
            "intra_dur": bool(getattr(b, "_intra_dur", False)),
            "intra_declarado": bool(getattr(b, "_intra_declarado", False)),
        }

    emitir(chave=chave, competencia=(ini, fim), janela_visual=(vis_ini, vis_fim))
    brutas = juiz.batidas_apuraveis(
        c, meia_noite(alvo - 2 * UM_DIA), meia_noite(alvo + 3 * UM_DIA)
    )
    for b in brutas:
        emitir(chave=chave, batida_apuravel=batida(b))

    anterior = None
    for trecho in traco["trechos"]:
        if any(t.date() in dias for t in trecho):
            pares = list(zip(trecho[::2], trecho[1::2]))
            emitir(
                chave=chave, dia_e6=trecho[0].date(),
                instantes=[local(t) for t in trecho],
                pares=[(local(a), local(z), min_entre(a, z)) for a, z in pares],
                piso_trecho=sum(min_entre(a, z) for a, z in pares),
                teto_trecho=min_entre(trecho[0], trecho[-1]),
                fronteira_anterior=None if anterior is None else {
                    "n_pontas": len(anterior),
                    "gap_min": min_entre(anterior[-1], trecho[0]),
                    "vao_min": min_entre(anterior[0], trecho[0]),
                },
            )
        anterior = trecho

    celulas = celulas_do_periodo(
        [c.pk], min(vis_ini, dias[0]), max(vis_fim, dias[-1])
    )
    cels_vis = {k: cel for k, cel in celulas.items() if vis_ini <= k[1] <= vis_fim}
    geradoras = {cel.escala_geradora_id for cel in cels_vis.values() if cel.escala_geradora_id}
    escalas = sorted(
        escalas_do_periodo(c, vis_ini, vis_fim, mais=geradoras),
        key=lambda ec: ec.data_inicio,
    )

    for dia in dias:
        k = dia.isoformat()
        cel = celulas.get((c.pk, dia))
        de = linhas.get(dia)
        emit_cel = None if cel is None else {
            "trabalha": cel.trabalha, "veredito": cel.veredito,
            "marcos": (cel.dna or {}).get("marcos"), "dna": cel.dna,
        }
        ec = vinculo_do_dia(c.pk, dia, escalas, cels_vis) if de is not None else None
        te = ec.tipo_escala if ec is not None else None
        livre = bool(te and te._intervalo_livre())
        intervalo = ec.intervalo_do_dia(dia, celulas=cels_vis) if te else (None, None)
        do_dia = [t for t in turnos if t.data_turno == dia]
        real = juiz.realizado_dos_turnos(do_dia, dia, livre=livre, intervalo=intervalo)
        pos = juiz._borda_posicional(do_dia)
        selecionado = minutos_do_espelho(de)
        piso = v.minutos.get(k, 0.0)
        delta = None if selecionado[0] is None else piso - selecionado[0]
        emitir(
            chave=chave, dia=dia, celula=emit_cel,
            vinculo_id=ec.pk if ec is not None else None,
            livre=livre, intervalo_declarado=intervalo,
            realizado_juiz=real._asdict() if de is not None else None,
            borda_L115=None if pos is None else {
                "entrada": batida(pos[0].entrada), "saida": batida(pos[1]),
                "pausa": [batida(b) for b in pos[2]],
            },
            espelho=None if de is None else {
                **{campo: de.get(campo) for campo in (
                    "minutos_realizados", "pago_h", "realizado_janela_descontada",
                    "realizado_sem_turno", "realizado_aberto", "isencao", "veredito_dia",
                )},
                "ausencia_presente": de.get("ausencia") is not None,
                "cobertura_folha_presente": de.get("_cobertura_folha") is not None,
            },
            selecionado=selecionado, piso=piso, teto=v.envelope.get(k, 0.0),
            pausa_e6=v.pausas.get(k, 0.0), horas=v.horas.get(k, []),
            brutas=v.brutas.get(k, 0), impar=k in v.impares, corte=k in v.cortes,
            delta=delta, delta_arredondado=round(delta) if delta is not None else None,
            guarda_janela=(delta is not None and diferenca_e_a_janela_declarada(delta, de)),
        )

        for t in do_dia:
            isolado = None
            if de is not None and pos is None:
                isolado = juiz.realizado_dos_turnos(
                    [t], dia, livre=livre, intervalo=intervalo
                )._asdict()
            emitir(
                chave=chave, data_turno=t.data_turno, aberto=t.aberto,
                cruza_meia_noite=t.cross_meianoite,
                entrada=batida(t.entrada), saida=batida(t.saida),
                fim_contado=batida(juiz._fim_fechado(t)),
                batidas_turno=[batida(b) for b in t.batidas],
                pausas=[(local(a), local(z)) for a, z in juiz.intervalos_do_turno(t)],
                abaixo_do_piso=[
                    (local(a), local(z)) for a, z in juiz.pausas_abaixo_do_piso(t)
                ],
                realizado_turno_isolado=isolado,
            )

    resultado = esp["resultado"]
    mapa = getattr(resultado, "dia_da_jornada", None) or {}
    dia_cartao = {id(p): d for d, linha in linhas.items() for p in linha["turnos"]}
    for ft, periodos in ((False, resultado.periodos), (True, resultado.periodos_ft)):
        for p in periodos:
            d_cartao = dia_cartao.get(id(p))
            d_mapa = mapa.get(id(p))
            d_civil = timezone.localtime(p.entrada).date()
            if {d_cartao, d_mapa, d_civil}.intersection(dias):
                emitir(
                    chave=chave, periodo_motor={
                        "ft": ft, "dia_cartao": d_cartao, "dia_mapa": d_mapa,
                        "dia_para_lavra": _dia_de(p, mapa),
                        "entrada": local(p.entrada), "saida": local(p.saida),
                        "efetivo_entrada": local(p.efetivo_entrada),
                        "efetivo_saida": local(p.efetivo_saida),
                        "minutos_trabalhados": p.minutos_trabalhados,
                        "minutos_fora_janela": p.minutos_fora_janela,
                        "intra_descontada": p.intra_descontada,
                    },
                )


with schema_context("juliani"), transaction.atomic(), ExitStack() as restaurar:
    restaurar.callback(logging.disable, logging.root.manager.disable)
    logging.disable(logging.CRITICAL)
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ READ ONLY")
        cursor.execute(
            "SELECT current_database(), current_schema(), "
            "current_setting('transaction_isolation'), "
            "current_setting('transaction_read_only'), txid_current_snapshot()::text"
        )
        banco, schema, isolamento, leitura, snapshot = cursor.fetchone()
    assert (banco, schema, isolamento, leitura) == ("sombra", "juliani", "repeatable read", "on")
    emitir(banco=banco, schema=schema, snapshot=snapshot, hoje=HOJE, agora=local(AGORA))
    for pk, data in ALVOS:
        medir(Colaborador.objects.select_related("empresa").get(pk=pk), dt.date.fromisoformat(data))
```

## 4. Dia → mecanismo → obra → total dos 18

| Dia | Mecanismo demonstrado ou localizado | Obra dona | Quantos dos 18 a obra zera, somando o mapa |
|---|---|---|---|
| **col114 15/11** | **C**, diferença entre piso e vão interno com janela do espelho zero. Origem da valorização **[nao verificado]**. `SONDA1/roda.out:15` · `app/ponto/turnos.py:719–771` | **[nao verificado]** | **[nao verificado]** |
| **col130 29/10** | **B** na chave do E6 e **C** no desconto de 60. Origem do saldo **[nao verificado]**. `SONDA1/roda.out:16` · `app/ponto/calculador/nucleo.py:235–247` · `app/ponto/turnos.py:760–771` | **[nao verificado]** | **[nao verificado]** |
| **col220 10/11** | **B** na chave do E6 e **C** no desconto de 60. Origem do saldo **[nao verificado]**. `SONDA1/roda.out:17` · `app/ponto/calculador/nucleo.py:235–247` · `app/ponto/turnos.py:760–771` | **[nao verificado]** | **[nao verificado]** |
| **col874 22/10** | **B** na migração de uma batida civil no E6 e **C** no desconto de 60. Geometria que completa o saldo **[nao verificado]**. `SONDA1/roda.out:18` · `app/ponto/calculador/nucleo.py:235–247` · `app/ponto/turnos.py:719–771` | **[nao verificado]** | **[nao verificado]** |
| **col989 23/10** | **A**, o par do E6 sozinho não fecha os 721 min. Participação de **D** **[nao verificado]**. `SONDA1/roda.out:19` · `app/ponto/turnos.py:719–771` · `app/ponto/services/espelho.py:465–474,778–801` | **[nao verificado]** | **[nao verificado]** |
| **col996 10/11** | **B** e corte por vão com dois trechos na chave do E6. `SONDA1/roda.out:20` · `app/ponto/calculador/nucleo.py:215–247` | `ORACULO-E6-43-DEFEITOS` registra o ramo do instrumento. Obra que resolve este delta **[nao verificado]**. `app/docs/GRADE-MESTRA.md:303` | O mapa relaciona **2 A**, **2 B** e **2 candidatas C** ao instrumento. Esta sonda acrescenta **1 ocorrência do ramo de corte**. Total que a obra zera **[nao verificado]**. `DOSSIE/r.md:27–34` · `SONDA1/roda.out:20` |

Nenhuma das seis chaves sustenta abrir **classe nova** com este dado. Para uma classe que a sonda venha a demonstrar, o padrão depende do produtor divergente. O GLOSSÁRIO dá **Single Source of Truth / um juiz por pergunta** para par, dia ou pausa julgados em fontes distintas, **CQRS com projeções reconstruíveis** quando a divergência está na projeção, e **Architecture Conformance Test** para selar o result

[truncado em 20000 de 20282 caracteres]

_8364 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
