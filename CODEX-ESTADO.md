# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 23:30** · publicado em **14/11/2026 23:31** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 14/11 23:30 · sessao 2376bf496618 · GRADE-RESOLVE-67 · censo · origem=Code

(1) **col148 — 02/11**. O registro não individualiza os demais cobrados nem explicita o ano; a sonda adota 2026 pelo contexto desta chamada **[nao verificado]**. (app/docs/BACKLOG.md:2551)

(2) **PRECISA SONDA**. A obra FERIADO-ESCALA está vinculada ao caso, mas seu balde não comprova dono L-099: a mesma grade inclui HJ-092 como LATERAL-ESTRUTURA e registra seu fechamento como BATIDA. O juiz depende dos insumos reais do dia. (app/docs/GRADE-MESTRA.md:127–128; app/ponto/management/commands/e6_oraculo.py:182–195)

O `git grep` solicitado falhou por ausência de metadados Git; a busca equivalente com `rg` não encontrou testes citando literalmente HJ-074 ou BACKLOG-HJ. **[nao verificado]** [nao verificado]

(3) Sonda para shell Django já inicializado no schema do caso, com credencial somente SELECT. Usa a assinatura conferida e a janela oficial da empresa; execução e resultado **[nao verificado]**. (app/ponto/management/commands/e6_oraculo.py:128–129; app/ponto/janelas.py:60–63; bin/sonda_leitura.sh:239–240)

```python
import contextlib
import datetime as dt
import io
import logging
import re

from django.db import connections
from django.utils import timezone

def somente_select(execute, sql, params, many, context):
    texto = sql.strip()
    if (
        many
        or not re.match(r"SELECT\b", texto, re.I)
        or ";" in texto
        or re.search(r"\bINTO\b|\bFOR\s+(UPDATE|SHARE)\b", texto, re.I)
    ):
        raise RuntimeError("SQL recusado")
    return execute(sql, params, many, context)

erros = 0
linhas = []
nivel_anterior = logging.root.manager.disable

try:
    logging.disable(logging.CRITICAL)
    with contextlib.redirect_stdout(io.StringIO()), \
         contextlib.redirect_stderr(io.StringIO()), \
         contextlib.ExitStack() as pilha:
        for conexao in connections.all():
            pilha.enter_context(conexao.execute_wrapper(somente_select))

        from colaboradores.models import Colaborador
        from ponto.janelas import janela_atual
        from ponto.management.commands.e6_oraculo import InsumosDoDono

        agora = timezone.now()
        hoje = timezone.localtime(agora).date()

        for identificador, dia in [(148, dt.date(2026, 11, 2))]:
            try:
                colab = Colaborador.objects.get(pk=identificador)
                ini, fim = janela_atual(dia, colab.empresa_id)
                dono = InsumosDoDono(
                    colab, ini, fim, hoje, agora=agora
                ).dono(dia)
                if dono not in {"ESTRUTURA", "CADASTRO", "BATIDA"}:
                    raise RuntimeError("Dono sem resposta")
                linhas.append(
                    f"DONO|col{identificador}|{dia.isoformat()}|{dono}"
                )
            except Exception:
                erros += 1
except Exception:
    erros += 1
finally:
    logging.disable(nivel_anterior)

for linha in linhas:
    print(linha)
print(f"FIM|erros={erros}")
```

`dono(dia)` classifica os insumos e não retorna “SEM DIVERGENCIA”; essa ausência não pode ser certificada apenas por sua resposta. (app/ponto/management/commands/e6_oraculo.py:98–110, :182–195)

(4) Padrão quebrado: **[nao verificado]**, enquanto faltar o dono. Obra documentalmente vinculada: **FERIADO-ESCALA**; fechamento do caso **[nao verificado]**. (app/docs/GRADE-MESTRA.md:127)

RESULTADO|BACKLOG-HJ|HJ-074|PRECISA SONDA|-|-

## 14/11 23:30 · sessao 6edd0b133cbb · - · - · origem=Ronald (tmux)

(1) **SEM DIA-COLAB.** O registro pede dono L-099 para “40 dias de férias com batida”, mas não enumera colaboradores e datas; o complemento repete essa contagem sem individualização. (app/docs/BACKLOG.md:1917; app/docs/PROMPTS.md:2226)

O teste correspondente cria ausência e batida em uma fixture; não identifica dia-colab de produção do caso. (app/ferias/test_hj090_um_juiz.py:63; app/ferias/test_hj090_um_juiz.py:480–490)

(4) Dono ESTRUTURA e respectivo padrão/obra: [nao verificado]. Encerrado sem sonda pela ausência de dia-colab identificado no registro. (app/docs/BACKLOG.md:1917)

RESULTADO|BACKLOG-HJ|HJ-090|SEM DIA-COLAB|-|-

## 14/11 23:30 · sessao fa2040bf1b05 · GRADE-RESOLVE-67 · censo · origem=Code

(1) **SEM DIA-COLAB.** O registro identifica **col1032**, mas não informa o dia da escala contestada: “13/11 08:52” é o momento da conferência do servidor. A remissão mantém “aguarda print”, sem identificar esse dia. (app/docs/BACKLOG.md:2552; app/docs/PROMPTS.md:2212)

(4) Padrão e obra: **—**. A classificação estrutural está escrita para o achado lateral **HJ-075-EC-FIRST**; o dono L-099 do caso HJ-075 permanece **[nao verificado]**. (app/docs/BACKLOG.md:2553)

RESULTADO|BACKLOG-HJ|HJ-075|SEM DIA-COLAB|-|-

## 14/11 23:28 · sessao 86e63e85780f · GRADE-RESOLVE-67 · censo · origem=Code

(1) **SEM DIA-COLAB.** HJ-071 pede mostrar `hora_ini-hora_fim` da declaração no espelho/PDF e primeiro medir se o valor está gravado; não identifica colNNN nem a data do turno. (app/docs/BACKLOG.md:2549)

As referências complementares remetem HJ-071 à fila de tela, sem individualizar dia-colab. (app/docs/PROMPTS.md:2125; app/docs/RELATO.md:643)

(4) Dono ESTRUTURA: [nao verificado]. Padrão e obra não atribuídos; encerramento pela ausência de dia-colab no registro. (app/docs/BACKLOG.md:2549)

RESULTADO|BACKLOG-HJ|HJ-071|SEM DIA-COLAB|-|-

## 14/11 23:28 · sessao fefbe27aca41 · GRADE-RESOLVE-67 · censo · origem=Code

(1) **Sem dia-colab identificado.** HJ-073 descreve um censo dos botões “Decidir” por causa e permissão do supervisor; não informa colNNN nem data do dia apurado. O “11/11” é a data do registro. (app/docs/BACKLOG.md:2550)

A casa registra que “Decidir” não consulta a permissão do usuário e que a autorização fica nos botões, sem individualizar dia-colab para HJ-073. (app/docs/RELATO.md:643)

(4) Dono L-099, padrão quebrado e obra que fecha: [nao verificado]. Sem dia-colab, encerra-se conforme a condição do pedido.

RESULTADO|BACKLOG-HJ|HJ-073|SEM DIA-COLAB|-|-

## 14/11 23:28 · sessao b0d6fe45e50d · GRADE-RESOLVE-67 · censo · origem=Code

(1) **O registro não identifica dia-colab.** HJ-068 pede fio lateral do admin com itens acionáveis, encerrados em Histórico e contador correspondente; não informa colNNN nem data de jornada. O “11/11” é a data do registro. (app/docs/BACKLOG.md:2548)

(4) Dono L-099: [nao verificado]. Sem dia-colab identificado, encerro conforme solicitado; padrão e obra ficam “-”.

RESULTADO|BACKLOG-HJ|HJ-068|SEM DIA-COLAB|-|-

## 14/11 23:28 · sessao 70e71a86fbf2 · GRADE-RESOLVE-67 · censo · origem=Code

(1) **SEM DIA-COLAB.** HJ-066 registra a retirada dos cards de Férias e Benefícios das duas dashboards do colaborador, sem identificar colNNN nem data de jornada; “11/11” acompanha o registro da demanda. (app/docs/BACKLOG.md:2547)

As demais referências apenas encaminham HJ-066 à fila 2, sem individualizar dia-colab. (app/docs/PROMPTS.md:2125; app/docs/RELATO.md:643; app/docs/RELATO.md:651)

O `git grep` falhou por ausência de metadados Git; a busca equivalente com `rg` não encontrou testes citando HJ-066. [nao verificado] [nao verificado]

(4) Dono L-099: **[nao verificado]**. Conforme a condição do item (1), encerro sem sonda e sem atribuir padrão ou obra.

RESULTADO|BACKLOG-HJ|HJ-066|SEM DIA-COLAB|-|-

## 14/11 23:28 · sessao dadc9caf6ef5 · GRADE-RESOLVE-67 · censo · origem=Code

(1) **Sem dia-colab identificado.** HJ-058 registra anexos HEIC e contagens, mas não informa colNNN nem data completa vinculada a colaborador. A documentação relacionada também não individualiza esse par. (app/docs/BACKLOG.md:2554; app/docs/BACKLOG.md:1192; app/docs/PROMPTS.md:2212)

(4) Dono L-099, padrão quebrado e obra que fecha: **[nao verificado]**. A remissão à O30 F3 descreve o encaminhamento da conversão HEIC, sem estabelecer dono pelo juiz. (app/docs/BACKLOG.md:2554)

RESULTADO|BACKLOG-HJ|HJ-058|SEM DIA-COLAB|-|-

## 14/11 23:24 · sessao 91bc3e2e85f6 · E3-RESTO-R3 · censo · origem=Code

## CENSO
leitores:

- app/chamados/services/tarefa_do_dia.py:683 lê o fio cru e publica uma cópia quando há rota; sem rota devolve a porta inteira como `None`, eliminando também o fio interno.
- app/folha/fora_da_folha.py:85 `_fio_da_porta` interpreta porta `None` como `'nada'`, constante de app/chamados/juizes.py:1652; com porta presente devolve literalmente `porta['fio']`, inclusive eventual valor inesperado.
- app/folha/fora_da_folha.py:194 lê esse resultado e conta dias por fio em app/folha/fora_da_folha.py:200; aceita como chaves `admin/colab/chamado/nada`, declaradas em app/folha/fora_da_folha.py:37; `sistema/nao_sei/None/outro` publicados provocariam `KeyError`; ocorrência desses valores publicados [nao verificado].
- app/folha/fora_da_folha.py:93 `_grupo` interpreta `nada→D`, depois `chamado→C`, depois `colab→B`, restante não vazio→A; conjunto vazio→None em app/folha/fora_da_folha.py:91.
- app/folha/fora_da_folha.py:244 `o_que_segura` lê o fio publicado e procura `DESTRAVA[_f]` em app/folha/fora_da_folha.py:252: admin executa ação administrativa, colab responde, chamado resolve a porta aberta, nada significa “SEM PORTA” — app/folha/fora_da_folha.py:45; valores fora dessas quatro chaves provocariam `KeyError`.
- app/chamados/services/vigia_fio.py:354 `censo` lê a existência da porta: qualquer porta presente conta como fio vivo em app/chamados/services/vigia_fio.py:369; `None` segue para contagem temporal e baldes históricos em app/chamados/services/vigia_fio.py:372 e app/chamados/services/vigia_fio.py:382.
- app/chamados/services/vigia_fio.py:589 `censo_alcance` lê a porta; presente, conserva qualquer fio como papel em app/chamados/services/vigia_fio.py:602 e exige admin somente para `'admin'` em app/chamados/services/vigia_fio.py:601; `None` segue para hoje/futuro, baldes históricos ou `sem_porta`, respectivamente em app/chamados/services/vigia_fio.py:604, app/chamados/services/vigia_fio.py:610 e app/chamados/services/vigia_fio.py:655.
- app/chamados/services/furo_sem_canal.py:97 lê a projeção, sem interpretar fio: porta presente fornece rota; `None` entra em `sem_canal` em app/chamados/services/furo_sem_canal.py:104; desconhecimento vem da porta ou da tarefa em app/chamados/services/furo_sem_canal.py:100.
- app/escala/services/cadastro_realidade.py:469 lê existência da porta: com perguntas vivas, `None` permite candidato a beco; qualquer porta presente exclui o dia.
- app/ponto/services/pautas_dos_discordantes.py:81 lê `_fio_do_dia`, que passa por `_fio_da_porta` em app/folha/fora_da_folha.py:79; conserva somente `'nada'`, excluindo os demais fios.
- app/chamados/management/commands/diff_e3_sombra.py:49 instrumento projeta porta ausente como fio `'nada'` para Folha, mas fio `None` para vigia em app/chamados/management/commands/diff_e3_sombra.py:54; isso é simulação declarada dos consumidores, não execução deles — app/chamados/management/commands/diff_e3_sombra.py:47.
- app/chamados/management/commands/diff_e3_sombra.py:143 instrumento também lê rota/fio para construir a assinatura comparada.
- app/folha/retrato_folha.py:1181 lê a Folha agregada; app/folha/retrato_folha.py:1213 transforma grupo em fio gravado: A/B/C/D→admin/colab/chamado/ninguem, pelo mapa de app/folha/retrato_folha.py:177 e função de app/folha/retrato_folha.py:368.
- app/folha/retrato_folha.py:438 `fio_da_linha` aceita os quatro fios gravados; ausência ou valor inválido aciona a leitura legada em app/folha/retrato_folha.py:441.
- app/folha/retrato_folha.py:474 `dono_da_linha` lê esse fio como agrupamento da UI: admin→Admin, colab→Colab, chamado/ninguem→Admin pelo mapa de app/folha/retrato_folha.py:192; ninguem com todas as datas atuais/futuras→Hoje em app/folha/retrato_folha.py:477.
- app/folha/retrato_folha.py:1038 lê o fio gravado para oferecer link de fallback; ações publicadas têm prioridade em app/folha/retrato_folha.py:1025.
- app/folha/retrato_folha.py:1081 `o_que_fazer` lê fio e agrupamento separadamente; mostra o dono em app/folha/retrato_folha.py:1089 e a frase correspondente ao fio em app/folha/retrato_folha.py:1092.
- app/folha/vigia_retido_calado.py:47 lê o fio gravado `'ninguem'` e separa roteados/calados pela lista em app/folha/vigia_retido_calado.py:49.
- app/colaboradores/views.py:986 lê as respostas prontas do retrato e as entrega ao template em app/colaboradores/views.py:996; app/templates/colaboradores/partials/painel_colaborador.html:26 inclui app/templates/folha/_retrato_o_que_fazer.html:33, que renderiza rótulo/valor/cor, sem decidir fio.
- app/folha/views_retrato.py:318 usa os quadros do módulo do retrato; o agrupamento por dono consulta `dono_da_linha` em app/folha/retrato_folha.py:576.
- app/chamados/management/commands/backfill_fio_morto_e4b.py:87 lê a tarefa, mas não `tarefa['fio']`: seleciona ações sistêmicas perguntar/materializar/rejulgar em app/chamados/management/commands/backfill_fio_morto_e4b.py:101.

- app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:17 funções permitidas vigiadas: `carga_da_tarefa` e `porta_da_tarefa`; nos itens seguintes, I=importação e C=chamada de produção.
- app/folha/fora_da_folha.py:63 I `carga_da_tarefa`; app/folha/fora_da_folha.py:64 C.
- app/folha/fora_da_folha.py:69 e app/folha/fora_da_folha.py:231 I `porta_da_tarefa`; app/folha/fora_da_folha.py:70 e app/folha/fora_da_folha.py:243 C.
- app/chamados/services/furo_sem_canal.py:50 I ambas; app/chamados/services/furo_sem_canal.py:93 C carga; app/chamados/services/furo_sem_canal.py:97 C porta.
- app/chamados/services/vigia_fio.py:277 e app/chamados/services/vigia_fio.py:501 I ambas; app/chamados/services/vigia_fio.py:352 e app/chamados/services/vigia_fio.py:587 C carga; app/chamados/services/vigia_fio.py:354 e app/chamados/services/vigia_fio.py:589 C porta.
- app/escala/services/cadastro_realidade.py:438 I ambas; app/escala/services/cadastro_realidade.py:464 C carga; app/escala/services/cadastro_realidade.py:469 C porta.
- app/chamados/management/commands/diff_e3_sombra.py:22 I ambas; app/chamados/management/commands/diff_e3_sombra.py:117 C carga; app/chamados/management/commands/diff_e3_sombra.py:43 cria alias `candidata=porta_da_tarefa`, chamado em app/chamados/management/commands/diff_e3_sombra.py:129.
- app/chamados/services/tarefa_do_dia.py:698 C local `carga_da_tarefa`, sem importação.

- app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:21 também vigia oito funções proibidas naquele leitor: `porta_alcancavel_do_dia`, `alimentar_portas`, `prazo_estourou`, `particao_do_app`, `consulta_do_app`, `perguntas_do_app`, `fio_pelos_verbos`, `pergunta_viva`; `FIO_DO_VERBO` é constante.
- app/chamados/management/commands/diff_e3_sombra.py:20 I `porta_alcancavel_do_dia`; app/chamados/management/commands/diff_e3_sombra.py:127 C.
- app/chamados/management/commands/diff_e3_sombra.py:20 I `alimentar_portas`; app/chamados/management/commands/diff_e3_sombra.py:116 C; app/escala/services/cadastro_realidade.py:437 I e app/escala/services/cadastro_realidade.py:458 C; app/chamados/services/vigia_fio.py:498 I e app/chamados/services/vigia_fio.py:555 C; app/chamados/juizes.py:1698 C local.
- app/chamados/services/tarefa_do_dia.py:641 I `fio_pelos_verbos`; app/chamados/services/tarefa_do_dia.py:642 e app/chamados/services/tarefa_do_dia.py:649 C; app/chamados/juizes.py:1710 C local.
- app/chamados/services/tarefa_do_dia.py:515 I `particao_do_app`; app/chamados/services/tarefa_do_dia.py:516 C; app/chamados/services/pergunta_no_app.py:73 C local.
- app/chamados/juizes.py:1636 I `perguntas_do_app`; app/chamados/juizes.py:1642 C; app/chamados/services/pergunta_no_app.py:73, app/chamados/services/pergunta_no_app.py:208 e app/chamados/services/pergunta_no_app.py:231 C locais; app/colaboradores/services/ficha.py:105 I módulo e app/colaboradores/services/ficha.py:166 C por atributo.
- app/core/views.py:252 I `consulta_do_app` e app/core/views.py:253 C; app/api/views.py:993 e app/api/views.py:2533 I, app/api/views.py:994 e app/api/views.py:2534 C; app/chamados/services/cobranca.py:47 I e app/chamados/services/cobranca.py:48 C; app/chamados/services/aviso_pendencia.py:8 I e app/chamados/services/aviso_pendencia.py:9 C; app/chamados/services/pergunta_no_app.py:100 C local.
- app/ponto/management/commands/escalonar_chamados_supervisao.py:18 I `prazo_estourou` do C9 e app/ponto/management/commands/escalonar_chamados_supervisao.py:96 C; app/chamados/services/cobranca.py:33 I e app/chamados/services/cobranca.py:255 C; app/chamados/services/fila.py:30 I e app/chamados/services/fila.py:169 C.
- app/ponto/views.py:1729 I homônimo `prazo_estourou` de justificativas e app/ponto/views.py:1734 C; app/ponto/management/commands/alertar_justificativas_sla.py:10 I desse homônimo e app/ponto/management/commands/alertar_justificativas_sla.py:20 C; o selo proíbe pelo nome terminal — app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:74.

- app/chamados/reconciliador.py:398 I `pergunta_viva` também em linhas 651, 758, 1079 e 1552; C nas linhas 399, 662, 759, 1087 e 1595 do mesmo arquivo.
- app/chamados/models.py:352 I `pergunta_viva` também em linhas 1528, 1938, 3200 e reexportação 4226; C nas linhas 368, 1556, 1960, 3127, 3222 e 3484 do mesmo arquivo.
- app/chamados/juizes.py:1496 C local `pergunta_viva`, também nas linhas 1501, 1513, 1625, 1725 e 2388 do mesmo arquivo.
- app/chamados/views.py:1238 I `pergunta_viva` por alias, também em linhas 1256 e 1415; C nas linhas 1242, 1257 e 1417 do mesmo arquivo.
- app/api/views.py:2607 I `pergunta_viva` via models; app/api/views.py:2611 C.
- app/colaboradores/services/calendario.py:478 I `pergunta_viva` como `_pv`; app/colaboradores/services/calendario.py:486 C.
- app/ponto/services/dia_do_colab.py:62 I `pergunta_viva` via models; app/ponto/services/dia_do_colab.py:71 C.
- app/chamados/services/cobranca.py:225 I `pergunta_viva`; app/chamados/services/cobranca.py:241 C.
- app/chamados/services/vigia_fio.py:704 e app/chamados/services/vigia_fio.py:761 I `pergunta_viva`; app/chamados/services/vigia_fio.py:713 e app/chamados/services/vigia_fio.py:771 C.
- app/chamados/services/pergunta_no_app.py:51 e app/chamados/services/pergunta_no_app.py:171 I `pergunta_viva` via models; app/chamados/services/pergunta_no_app.py:61 e app/chamados/services/pergunta_no_app.py:180 C.
- app/chamados/services/tarefa_do_dia.py:474 I `pergunta_viva`; app/chamados/services/tarefa_do_dia.py:523 e app/chamados/services/tarefa_do_dia.py:527 C.
- app/chamados/services/acoes_chamado.py:357 I `pergunta_viva` via models; app/chamados/services/acoes_chamado.py:358 C.
- app/chamados/services/disputa_emissao.py:1823 e app/chamados/services/disputa_emissao.py:1918 I `pergunta_viva`; app/chamados/services/disputa_emissao.py:1828, app/chamados/services/disputa_emissao.py:1973 e app/chamados/services/disputa_emissao.py:2055 C.
- app/chamados/services/competencia_trancada.py:83 I `pergunta_viva`; app/chamados/services/competencia_trancada.py:144 e app/chamados/services/competencia_trancada.py:167 C.
- app/chamados/services/fio.py:267 I `pergunta_viva` também em linhas 643, 812, 988 e 2174; C nas linhas 268, 644, 820, 999, 2180 e 2185 do mesmo arquivo.
- app/chamados/services/relavra_via.py:209 I `pergunta_viva`; app/chamados/services/relavra_via.py:242 C.
- app/chamados/management/commands/disparar_perguntas_competencia.py:96 I `pergunta_viva` via models; app/chamados/management/commands/disparar_perguntas_competencia.py:111 C.
- app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:434 I `pergunta_viva`; app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:449 C.
- app/chamados/management/commands/auditar_invariantes_chamados.py:192 I `pergunta_viva` por aliases também em linhas 521, 854, 885 e 1615; C nas linhas 202, 540, 858, 894, 1653, 1710, 1769 e 1782 do mesmo arquivo.
- app/chamados/management/commands/reabrir_orfas_enterradas.py:58 I `pergunta_viva`; app/chamados/management/commands/reabrir_orfas_enterradas.py:83 C.
- app/chamados/juizes.py:1658 lê `FIO_DO_VERBO`; app/folha/fora_da_folha.py:84 importa `FIO_SEM_PORTA`, lido em app/folha/fora_da_folha.py:85; app/ponto/services/pautas_dos_discordantes.py:62 importa essa constante, lida em app/ponto/services/pautas_dos_discordantes.py:81.

escritores:

- app/chamados/services/tarefa_do_dia.py:672 grava o campo cru `out['fio']`; a seleção em app/chamados/services/tarefa_do_dia.py:643 admite `colab/admin/chamado/sistema/nao_sei/nada`; nenhum desses caminhos escreve `fio=None`.
- app/chamados/services/tarefa_do_dia.py:627 dá `colab` somente com pergunta pendente no app e ação RESPONDER do colaborador; prazo não participa desse degrau.
- app/chamados/services/tarefa_do_dia.py:630 dá candidato `admin` para decidir ausência, resolver dia ou validar com âncora; a prioridade final continua abaixo de colab — app/chamados/services/tarefa_do_dia.py:643.
- app/chamados/services/tarefa_do_dia.py:639 exclui do residual `cobrar`, `None` e `FORA_DA_LISTA_PADRAO`; app/chamados/services/tarefa_do_dia.py:642 consulta `fio_pelos_verbos`, que devolve admin para validar e chamado para os demais residuais — app/chamados/juizes.py:1655.
- app/chamados/services/tarefa_do_dia.py:635 dá candidato `sistema` para ação sistêmica sem portas ligadas ao dia ou com porta nas ações alcançáveis; as portas sistêmicas nativas são strings emissor/cartorio/materializacao — app/chamados/services/tarefa_do_dia.py:69 — e sistema sem porta termina com rota `None` em app/chamados/services/tarefa_do_dia.py:657.
- app/chamados/services/tarefa_do_dia.py:644 escreve `nao_sei` quando não há candidato e há desconhecimento; caso contrário escreve `nada`; ambos ficam sem rota — app/chamados/services/tarefa_do_dia.py:650.
- app/chamados/services/tarefa_do_dia.py:667 sobrescreve a escolha com `admin/porta_acao` quando Resolver dia foi oferecido e a primeira passagem ficou sem rota.
- app/chamados/services/tarefa_do_dia.py:683 escreve o fio publicado copiando o cru; rota ausente produz porta `None`, não `{'fio': None}`; sistema/nao_sei/nada internos acabam ocultados por essa projeção — app/chamados/services/tarefa_do_dia.py:650 e app/chamados/services/tarefa_do_dia.py:657.
- app/chamados/juizes.py:1575 e app/chamados/juizes.py:1588 escrevem verbo `None` como alimentação, não fio; o filtro atual de app/chamados/services/tarefa_do_dia.py:639 impede que esse residual sozinho dê dono.
- app/folha/fora_da_folha.py:254 escreve o fio derivado no resultado de `o_que_segura`; app/folha/retrato_folha.py:1213 grava a representação agregada sob a chave `dono`, com `ninguem` para grupo D — app/folha/retrato_folha.py:368.

decisoes:

- SIM, há respostas diferentes a jusante: para um dia histórico retido sem porta, Folha responde `nada/grupo D`, enquanto o retrato responde balde Admin; isso decorre de app/folha/fora_da_folha.py:85, app/folha/fora_da_folha.py:93, app/folha/retrato_folha.py:368 e app/folha/retrato_folha.py:192; ocorrência individual nesta cópia [nao verificado].
- Para uma porta presente, Folha e vigia conservam o mesmo fio; o vigia apenas calcula `exige_admin` pela igualdade com admin — app/folha/fora_da_folha.py:85 e app/chamados/services/vigia_fio.py:601.
- O caso `sistema/rota=None` distingue trabalho sistêmico de porta publicada: a tarefa pode exigir materializar/rejulgar enquanto Folha responde nada; o backfill lê essas ações, sem ler o fio cru — app/chamados/services/tarefa_do_dia.py:163, app/chamados/services/tarefa_do_dia.py:165, app/chamados/services/tarefa_do_dia.py:683 e app/chamados/management/commands/backfill_fio_morto_e4b.py:101.
- A fixture citada não prova `fio=='sistema'`: a palavra está no comentário; a asserção verifica somente desconhecimento falso, rota None e porta None — app/ponto/tests/test_cxr_tela_le_o_motor.py:382 e app/ponto/tests/test_cxr_tela_le_o_motor.py:385.
- A cura deve distinguir fio interno, porta publicada e agrupamento da UI; a substituição do agrupamento de sem porta por balde próprio já tem resposta em LU-05 — app/docs/CORTES.md:437; residual decidir como admin já está decidido em LU-01 — app/docs/CORTES.md:433.
- O selo atual fiscaliza somente `fora_da_folha.py`, identificado em app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:14; confere imports, nomes, atributos, strings e chamadas proibidas em app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:48.
- O selo atual exige chamadas de carga e porta, resolvendo aliases; importar sem chamar já não basta — app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:26, app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:83 e app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:99.
- O selo prova presença sintática de chamadas, sem provar execução, alcance de todos os caminhos ou uso dos retornos: coleta `ast.Call` em app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:45 e exige presença dos nomes em app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:87.
- Os 60 mutantes abaixo têm alvo literal com 1 casamento no respectivo arquivo; nenhum tem 0 ou >1; o executor exige unicidade em app/chamados/tests/e3_degrau_mutantes.py:202; morte efetiva dos mutantes nesta cópia [nao verificado].
- `cobrar_residual_colab`, `historico_da_dono`, `nulo_da_dono`: alvo `if verbos[pk] not in (*FORA_DA_LISTA_PADRAO, None, 'cobrar')` → 1 casamento, app/chamados/services/tarefa_do_dia.py:639.
- `devida_fora_do_app_colab`: alvo `i['pergunta_id'] in pendentes_app and` → 1 casamento, app/chamados/services/tarefa_do_dia.py:627.
- `admin_antes_colab`: alvo `('colab', 'admin', 'chamado', 'sistema')` → 1 casamento, app/chamados/services/tarefa_do_dia.py:643.
- `app_sem_rota`: alvo `elif fio == 'colab':\n        rota = 'porta_app'` → 1 casamento, app/chamados/services/tarefa_do_dia.py:652.
- `acao_direta_sem_dono`: alvo `com_dono = admin_alcancavel` → 1 casamento, app/chamados/services/tarefa_do_dia.py:634.
- `rota_colab_pela_fila`, `app_com_cobrar_vira_fila`: alvo `elif fio == 'colab':` → 1 casamento, app/chamados/services/tarefa_do_dia.py:652.
- `porta_acao_some`: alvo `'porta_acao' if fio == 'admin' else None` → 1 casamento, app/chamados/services/tarefa_do_dia.py:657.
- `residual_ignora_acao_do_colab`: alvo `portas - {a['porta'] for a in com_dono}` → 1 casamento, app/chamados/services/tarefa_do_dia.py:639.
- `rota_sem_dono_publicada`: alvo `if fio in ('nada', 'nao_sei'):\n        rota = None` → 1 casamento, app/chamados/services/tarefa_do_dia.py:650.
- `sistema_sem_alcance`: alvo `(not portas or a['porta'] in portas_acoes)` → 1 casamento, app/chamados/services/tarefa_do_dia.py:635.
- `documento_da_colab`: alvo `a['acao'] == RESPONDER and a['de'] == COLABORADOR` → 1 casamento, app/chamados/services/tarefa_do_dia.py:628.
- `sem_memo_por_colab`: alvo `if self._vida_carregada:` → 1 casamento, app/chamados/services/tarefa_do_dia.py:472.
- `carga_sem_relogio`: alvo `pergunta_viva(p, hoje=hoje, carga=carga, agora=self.agora)))` → 1 casamento, app/chamados/services/tarefa_do_dia.py:527.
- `relogio_perdido`, `consulta_fora_do_app`: alvo `em_lote=True, agora=agora` → 1 casamento, app/chamados/services/pergunta_no_app.py:73.
- `predicado_pendente_afrouxado`: alvo `p.respondida_em is None and p.disputa.fechada_em is None` → 1 casamento, app/chamados/services/pergunta_no_app.py:81.
- `canal_nao_conta_app`: alvo `sum(1 for r in rota.values() if r == 'porta_app')` → 1 casamento, app/chamados/services/furo_sem_canal.py:116.
- `vigia_contador_deslocado`: alvo `'alcance_porta_app': 0` → 1 casamento, app/chamados/services/vigia_fio.py:517.
- `vigia_log_omite_app`: alvo `|porta_app=%d|` → 1 casamento, app/chamados/services/vigia_fio.py:667.
- `command_omite_app`: alvo `| app %s ;` → 1 casamento, app/chamados/management/commands/lavrar_furo_sem_canal.py:37.
- `canal_nao_conta_acao`: alvo `sum(1 for r in rota.values() if r == 'porta_acao')` → 1 casamento, app/chamados/services/furo_sem_canal.py:117.
- `vigia_contador_acao_deslocado`: alvo `'alcance_porta_acao': 0` → 1 casamento, app/chamados/services/vigia_fio.py:517.
- `vigia_log_omite_acao`: alvo 

[truncado em 20000 de 26580 caracteres]

## 14/11 23:21 · sessao caf66e264376 · - · - · origem=Ronald (tmux)

Censo concluído: o selo atual exige chamadas sintáticas de `carga_da_tarefa` e `porta_da_tarefa`, além de fiscalizar imports e nomes proibidos; não prova execução nem consumo efetivo dos retornos (`app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:26`, `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:73`, `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:83`).

Os 60 mutantes têm exatamente um casamento literal no respectivo arquivo; inclusive `sem_memo_por_colab` em `app/chamados/services/tarefa_do_dia.py:472` e `carga_sem_relogio` em `app/chamados/services/tarefa_do_dia.py:527`. Inventários completos, textos dos alvos e referências foram enviados nas mensagens anteriores (`app/chamados/tests/e3_degrau_mutantes.py:17`, `app/chamados/tests/e3_degrau_mutantes.py:105`, `app/chamados/tests/e3_degrau_mutantes.py:141`).

As únicas importações de funções de `chamados` em `fora_da_folha.py` são carga em `app/folha/fora_da_folha.py:63` e porta em `app/folha/fora_da_folha.py:69` e `app/folha/fora_da_folha.py:231`; suas chamadas estão em `app/folha/fora_da_folha.py:64`, `app/folha/fora_da_folha.py:70` e `app/folha/fora_da_folha.py:243`.

`FIO_DO_VERBO` é definido e consumido em `app/chamados/juizes.py:1650` e `app/chamados/juizes.py:1658`. `FIO_SEM_PORTA` é definido em `app/chamados/juizes.py:1652`, importado e consumido por `app/folha/fora_da_folha.py:84` e `app/folha/fora_da_folha.py:85`, e por `app/ponto/services/pautas_dos_discordantes.py:62` e `app/ponto/services/pautas_dos_discordantes.py:81`.

## 14/11 23:20 · sessao 96f929834b13 · - · - · origem=Ronald (tmux)

Censo do item (1), somente pelo código:

- `app/chamados/services/tarefa_do_dia.py:643` escolhe o fio cru na ordem `colab > admin > chamado > sistema`; sem candidato, escreve `nao_sei` se `out['nao_sei']`, senão `nada` (`:644`).
- `app/chamados/services/tarefa_do_dia.py:667` sobrescreve a escolha com `fio='admin', rota='porta_acao'` quando a autoridade oferece Resolver dia e a primeira passagem ficou sem rota.
- `app/chamados/services/tarefa_do_dia.py:672` é o único ponto encontrado de gravação do campo `out['fio']`; recebe essas escolhas. `None` não é produzido como valor do fio cru (`:643-644`, `:667`).
- `app/chamados/services/tarefa_do_dia.py:627` supõe `colab` = pergunta pendente no app e ação RESPONDER do colaborador; `:630-633` supõe `admin` = ação administrativa com alcance; `:639-642` deriva residual dos verbos, excluindo `None`, `cobrar` e `FORA_DA_LISTA_PADRAO`; `app/chamados/juizes.py:1655-1661` traduz residual `validar` em admin e os demais em chamado.
- `app/chamados/services/tarefa_do_dia.py:634-635` supõe sistema = ação de sistema sem nenhuma porta ligada ao dia ou cuja porta inteira esteja nas portas das ações; as ações nativas de sistema usam apenas strings `emissor`, `materializacao`, `cartorio` (`:69`, `:163`, `:165`, `:317`, `:321`), portanto o caso nativo sistema só entra sem portas ligadas; termina com rota None (`:657`). Publicação de `fio='sistema'` num dict não é alcançada por essas ações nativas; incidência em produção [nao verificado].
- `app/chamados/services/tarefa_do_dia.py:683` é o único leitor direto encontrado do fio cru e escritor do fio publicado: copia `tarefa.get('fio')` quando rota é verdadeira; quando rota é None, retorna **porta None**, não um dict com `fio=None`. Logo sistema/nao_sei/nada crus se tornam ausência de porta (`:650-657`).

Leitores diretos e de existência de porta:

- `app/folha/fora_da_folha.py:85` `_fio_da_porta`: porta None → `FIO_SEM_PORTA`, definido como `nada` em `app/chamados/juizes.py:1652`; qualquer dict → preserva seu fio, sem validar.
- `app/folha/fora_da_folha.py:193-200` `fora_da_folha`: lê a projeção, acumula contadores por fio; admin/colab/chamado/nada são as únicas chaves (`:37`, `:184`). Dict publicado com sistema/nao_sei/None/outro resultaria em KeyError em `:200`; isso não prova ocorrência em produção.
- `app/folha/fora_da_folha.py:93-99` `_grupo`: nada → D; senão chamado → C; senão colab → B; qualquer conjunto restante não vazio → A; conjunto vazio → None (`:91-92`). Semântica nominal dos quatro grupos em `:38-42`.
- `app/folha/fora_da_folha.py:243-254` `o_que_segura`: publica fio lido e busca `DESTRAVA[_f]` em `:252`; admin = executar ação administrativa, colab = responder, chamado = resolver porta do chamado, nada = SEM PORTA (`:45-54`). Valor publicado sistema/nao_sei/None/outro também daria KeyError em `:252`.
- `app/chamados/services/vigia_fio.py:354`/`:366-370` `censo`: lê apenas presença da porta; qualquer porta é fio vivo; None segue aos contadores históricos sem porta, independentemente do fio cru.
- `app/chamados/services/vigia_fio.py:589`/`:594-602` `censo_alcance`: lê porta e desconhecimento; porta presente conta rota e conserva o fio como papel; exige admin somente quando `fio == 'admin'`; qualquer outro fio presente tem exige_admin falso. Porta None segue a `de_hoje_pra_frente` (`:604-606`), subdivisões históricas (`:610-614`) ou sem_porta (`:655`). As subdivisões são explicitamente papel da fonte, não fio, em `:240-245`.
- `app/chamados/services/furo_sem_canal.py:97-104` `medir`: não lê fio; porta presente fornece rota, None entra em sem_canal; lê desconhecimento da porta ou da tarefa (`:100-102`).
- `app/escala/services/cadastro_realidade.py:469` `beco_em_lote`: não lê fio; com perguntas vivas, porta None permite candidato a beco; qualquer porta exclui.
- `app/ponto/services/pautas_dos_discordantes.py:81-84` `dias_sem_fio`: por `_fio_do_dia` → `_fio_da_porta` (`app/folha/fora_da_folha.py:79`), conserva apenas `nada`, portanto porta None; os demais fios publicados excluem o dia.

Instrumento, separado de produção funcional:

- `app/chamados/management/commands/diff_e3_sombra.py:49-59` projeta porta None como folha fio nada, vigia fio None/exige_admin falso e beco/pauta sem porta verdadeiro; porta presente preserva fio para folha e vigia, exige_admin só para admin. `:143` usa rota/fio na assinatura da comparação. É projeção declarada, não execução dos consumidores (`:47`).

Leitores transitivos do fio agregado:

- `app/folha/retrato_folha.py:1181` chama fora_da_folha; `:1213` grava `dono` por `_fio_cru`; grupos A/B/C/D viram admin/colab/chamado/ninguem (`:177-178`, `:368`); sem grupo também vira ninguem; motivo diverso de furo vira admin (`:364-368`).
- `app/folha/retrato_folha.py:438-441` `fio_da_linha` aceita os quatro fios gravados de `:175`; valor ausente/inválido cai no legado.
- `app/folha/retrato_folha.py:474-481` `dono_da_linha` traduz admin→Admin, colab→Colab, chamado/ninguem→Admin pelo mapa `:192`; ninguem vira Hoje quando todas as datas são de hoje em diante (`:477-480`). É um balde de UI, distinto do fio de alcance.
- `app/folha/retrato_folha.py:1038` `portas_da_linha` usa o fio gravado como fallback de link; mapas admin/colab/chamado em `:975-977`, ninguem sem link de fio. Ações publicadas têm prioridade (`:1025-1037`).
- `app/folha/retrato_folha.py:1081-1092` `o_que_fazer` lê fio e balde UI, expõe separadamente “Depende de” e texto literal de DESTRAVA.
- `app/colaboradores/views.py:986-987` lê essas respostas prontas e passa ao template (`:996-997`); `app/templates/colaboradores/partials/painel_colaborador.html:26` inclui a parcial; `app/templates/folha/_retrato_o_que_fazer.html:33-38` só renderiza rótulo/valor/cor; não interpreta fio.
- `app/folha/views_retrato.py:49-52`/`:318` usa agrupamento/quadros do módulo do retrato; o leitor por grupo dono está em `app/folha/retrato_folha.py:576`, apontando para dono_da_linha.
- `app/folha/vigia_retido_calado.py:47-49` lê fio gravado ninguem e separa roteados/calados pela lista Cadastro x Realidade; não reinterpreta o fio cru.

Resposta à divergência:

- **Não há dois leitores diretos de produção atribuindo fios diferentes ao mesmo fio publicado**: o único que materializa o fio da porta é `_fio_da_porta`, que preserva o valor (`app/folha/fora_da_folha.py:85`); o vigia preserva o mesmo fio quando há porta (`app/chamados/services/vigia_fio.py:601-602`). O caso interno sistema/rota None não é um segundo leitor cru: a projeção o elimina (`app/chamados/services/tarefa_do_dia.py:683`).
- **Sim há respostas diferentes a jusante, por perguntas diferentes**: num dia histórico retido sem porta, Folha dá nada/grupo D (`app/folha/fora_da_folha.py:85`, `:93-94`), enquanto o retrato grava ninguem e exibe balde Admin (`app/folha/retrato_folha.py:368`, `:192`, `:481`). Esse comportamento é declarado como agrupamento da UI em `:189-191`; ocorrência nominal no hash [nao verificado].
- **Também há ação sistêmica apesar da Folha dizer nada**: `app/chamados/management/commands/backfill_fio_morto_e4b.py:87` lê a tarefa e `:101-106` seleciona perguntar/materializar/rejulgar das ações de sistema; o mesmo tipo de tarefa pode ter fio sistema e porta None (`app/chamados/services/tarefa_do_dia.py:163-165`, `:643-657`), portanto Folha devolve nada (`app/folha/fora_da_folha.py:85`). Ele não lê `tarefa['fio']`; lê ações.
- A fixture confirma somente a ausência da porta e desconhecimento falso, não igualdade do fio com sistema: `app/ponto/tests/test_cxr_tela_le_o_motor.py:382-386`; a palavra sistema está no comentário, enquanto a asserção verifica `(nao_sei, rota, porta) == (False,None,None)`.

## 14/11 23:20 · sessao 8d6200514031 · - · - · origem=Ronald (tmux)

- app/chamados/services/tarefa_do_dia.py:73 — divergência objetiva: “[(instancia, pos, tipo, hora)]”; o terceiro elemento real é o dicionário `lp`, escrito em app/chamados/services/tarefa_do_dia.py:82 e consumido como dicionário em app/chamados/services/tarefa_do_dia.py:116.
- app/chamados/services/tarefa_do_dia.py:8 — “nenhum cache” precisa de qualificação: há memos por carga para grade, marcos, instâncias e tarefas, respectivamente em app/chamados/services/tarefa_do_dia.py:534, app/chamados/services/tarefa_do_dia.py:541, app/chamados/services/tarefa_do_dia.py:547 e app/chamados/services/tarefa_do_dia.py:582; se a frase pretendia excluir somente cache persistente, essa intenção é [nao verificado].
- app/chamados/services/tarefa_do_dia.py:28 — “rota, fila e verbos leem alimentar_fila” é uma descrição incompleta: `verbos` vêm dessa alimentação em app/chamados/services/tarefa_do_dia.py:626, mas a rota colaborador depende dos pendentes do app em app/chamados/services/tarefa_do_dia.py:627 e a oferta do calendário pode escrever `admin/porta_acao` em app/chamados/services/tarefa_do_dia.py:658.
- As regiões antigas indicadas pelo anexo não conservam automaticamente as descrições abandonadas: a docstring atual da carga está em app/chamados/services/tarefa_do_dia.py:363; a descrição de memo e calendário plural em app/chamados/services/tarefa_do_dia.py:552 corresponde a app/chamados/services/tarefa_do_dia.py:582; a descrição da publicação em app/chamados/services/tarefa_do_dia.py:677 corresponde ao retorno em app/chamados/services/tarefa_do_dia.py:683.
- app/chamados/services/tarefa_do_dia.py:21 — “por colaborador sem porta” corresponde a uma chamada plural por colaborador com algum dia sem rota: constrói todos os dias em app/chamados/services/tarefa_do_dia.py:585, seleciona os dias sem rota em app/chamados/services/tarefa_do_dia.py:588 e só chama a plural quando esse conjunto existe em app/chamados/services/tarefa_do_dia.py:589.
- app/chamados/services/tarefa_do_dia.py:41 — a enumeração do fio bruto é compatível com os escritores em app/chamados/services/tarefa_do_dia.py:643 e app/chamados/services/tarefa_do_dia.py:667; `None` é retorno integral de `porta_da_tarefa`, não um valor escrito no fio, conforme app/chamados/services/tarefa_do_dia.py:683.
- app/chamados/services/tarefa_do_dia.py:606 — “A/S so tem tela pelo Resolver dia” continua correspondendo à extensão `fato_em_ausencia`: ela cria `VALIDAR/admin` sem porta em app/chamados/services/tarefa_do_dia.py:348, não passa no teste de alcance de validar em app/chamados/services/tarefa_do_dia.py:632 e depende da oferta do calendário em app/chamados/services/tarefa_do_dia.py:658.

## 14/11 22:23 · sessao 4302027cff09 · JUSTIFICATIVA-PAGA-O-PERIODO · construtor · origem=Code

Entrega, censo e comandos em [c9_entrega.txt](/home/ronald/wt-codex-justificativa-paga-o-periodo-220358/app/ponto/tests/c9_entrega.txt:1).

- C1 usa o mesmo W do julgamento; C3 caracteriza os números atuais. [Referências](/home/ronald/wt-codex-justificativa-paga-o-periodo-220358/app/ponto/tests/c9_entrega.txt:5).
- **78 testes passaram; 53/53 mutantes morreram por asserção**, com controles verdes. [Saídas](/home/ronald/wt-codex-justificativa-paga-o-periodo-220358/app/ponto/tests/c9_green.txt:90). [nao verificado]
- **C2 permanece parcial:** fonte e leitores adaptados preservam `None/sem_ata`, mas a autoridade completa ainda encontra `TypeError` no motor. [Reprodução](/home/ronald/wt-codex-justificativa-paga-o-periodo-220358/app/ponto/tests/c9_limites.txt:2).

**PERGUNTA pendente:** como representar esse desconhecido no contrato numérico do motor inviolável? Retirar a chave pode pagar inteiro; usar zero inventa saldo. [Censo do bloqueio](/home/ronald/wt-codex-justificativa-paga-o-periodo-220358/app/ponto/tests/c9_entrega.txt:19).

Persistência ORM [nao verificado]. Sem certificação.

## 14/11 22:15 · sessao e70026a4847b · - · - · origem=Ronald (tmux)

Dois achados concretos:

- `retratar_exportada` pode gravar zero sobre desconhecido: recebe os novos `None` de `por_dia_da_grade` em `ponto/services/dia_pago.py:471` e aplica `or 0` em `:484`, `:485`, `:489` e `:491`. Antes a ausência de ata bloqueava o caminho. Cabe uma guarda antes de qualquer escrita, preservando as linhas e devolvendo `sem_ata`.
- O contador ainda afirma zero quando o dia justificado sem ata também não possui `DiaPago` positivo: o universo consultado vem exclusivamente desses pagamentos (`ponto/services/dia_pago.py:709–714`), e o laço vazio retorna zero (`:727`). O teste novo cobre pagamento já existente (`ponto/tests/test_justificativa_c9_sem_banco.py:125`), sem cobrir hoje/futuro sem lavratura.

O resumo preserva `dias_abono=None` e motivo (`relatorios/cartao_pela_celula.py:191–192`, `:208`), mas a renderização PDF ainda oculta esse desconhecido pelo `or 0` do badge (`relatorios/pdf_espelho.py:787`).

C1 mantém cobertura antes do pareamento e entrega o W final explicitamente à fase financeira (`escala/utils.py:1891`, `:2101`, `:2114`); não identifiquei outra regressão concreta nessa sequência.

## 14/11 22:06 · sessao 908abe507e39 · - · - · origem=Ronald (tmux)

C1 exige separar cobertura e saldo em duas fases; mover o overlay inteiro depois de W faria o juiz ler lâmpadas anteriores ou incompletas. As lâmpadas desta montagem alimentam `turnos_do_colab` antes do cálculo final (`escala/utils.py:1882`, `escala/utils.py:1903`, `escala/utils.py:1922`).

Fluxo sugerido:

1. Aplicar cobertura da justificativa sem consultar realizado nem saldo (`escala/utils.py:1198`).
2. Preservar a única chamada a `realizado_do_dia` e escrever `minutos_realizados` no dia (`escala/utils.py:2065`, `escala/utils.py:2078`).
3. Alimentar a fase financeira explicitamente com esse mesmo W, sem repetir alterações nas lâmpadas.
4. Conferir que `ata_do_dia` recebe e copia W (`ponto/services/cartorio.py:738`).

O teste existente exige exatamente um sítio de chamada a `realizado_do_dia`; adicionar outro violaria o contrato (`ponto/tests/test_realizado_do_dia_autoridade.py:176`).

Teste recomendado: builder inteiro por AST, bordas ORM em memória, P480/W240, justificativa aprovada e nenhuma célula/ata inicial; exigir uma chamada ao juiz, nenhuma leitura de W da ata anterior, `min_justificados=240` e ata produzida com `minutos_realizados=240`. Execução desse cenário: [nao verificado]. [nao verificado]

## 14/11 22:05 · sessao b66d80ea94bc · - · - · origem=Ronald (tmux)

Censo da base, somente leitura. Nenhum `AGENTS.md` localizado dentro de `app` pela busca de arquivos; instruções em ancestrais [não verificado].

| Leitor | Hoje | Com `None`, sem adaptação |
|---|---|---|
| `efeitos_da_ausencia` — `ponto/services/efeito_ausencia.py:49` | Lê `trabalhado != 0`, transporta `minutos`; hoje ausência de ata já levanta na fonte `:129` | `None != 0` inclui `{dia: None}` no feed |
| Overlay `_marcar_ausencias` — `escala/utils.py:1198`, `:1257`, `:1277` | Lê fonte, copia minutos para a linha, usa cobertura separadamente | Copia `None`; identidade/cobertura pode continuar preservada |
| `grade_do_fechamento` — `folha/export.py:339`, `:341` | Transporta `p['minutos']` para grade | Transporta `None`, perde motivo por selecionar só minutos |
| `montar_dias` — `ponto/services/espelho.py:520`, `:522` | Mesmo transporte para grade | Mesmo problema do motivo |
| `grade_da_celula` — `escala/services/leitor_celula.py:460` | Copia minuto por presença da chave em dia trabalho | Copia `None` para celula existente; dia sem celula sai antes, em `:427`, sem transportar aprovação ou motivo |
| `por_dia_da_grade` — `ponto/services/dia_pago.py:398` | `truthy` seleciona crédito | `None` desaparece silenciosamente do mapa de abono |
| `dias_com_abono` — `ponto/services/dia_pago.py:713`, `:716` | Lê W lavrado; hoje ausência de ata levanta | Se leitor passar a retornar `None`, `ata.get` ainda levanta quando ata é `None`; precisa tratamento antes |
| `_totais_da_lavratura` — `relatorios/cartao_pela_celula.py:190` | Único chamador de produção de `dias_com_abono` | Propaga o retorno ao dict; consumidores posteriores precisam ser conferidos se retorno mudar |

Únicos leitores diretos de `minutos_realizados_da_ata` encontrados em produção: fonte (`ponto/services/efeito_ausencia.py:129`) e contador (`ponto/services/dia_pago.py:713`). O próprio leitor hoje rejeita ata ausente/campo `None` em `escala/services/leitor_celula.py:360`.

Feed financeiro chega ao motor pelos seguintes pontos:

- Fechamento: `ponto/services/fechamento.py:667`, `:755`, `:831`.
- Autoridade/espelho: `ponto/services/espelho.py:1137`.
- PDF: `relatorios/pdf_espelho.py:407`.
- Esmeril: `ponto/services/esmeril_espelho.py:166`.
- Comando de comparação: `ponto/management/commands/diff_espelho_alimentacao.py:56`, `:124`.
- Extrato parcial: `relatorios/management/commands/diff_extrato_parcial.py:62`.

Bloqueio concreto para PERGUNTA: o motor soma `_mj[d]` em `ponto/motor_calculo_v2.py:2633`; valor `None` gera `TypeError`. Omitir o dia permite pagamento inteiro por `_abono_inteiro` em `:2629`; usar zero inventa número. O contrato atual não representa desconhecido. Como preservar esse dia no feed e no resultado sem modificar o motor inviolável? Qualquer adaptação financeira adicional precisa respeitar essa pergunta explícita.

PDF soma o resultado com parcial em `relatorios/pdf_espelho.py:480`; autoridade compõe fatias usando soma com `or 0` em `ponto/services/fechamento.py:298`; portanto converter apenas `resultado.horas_abono=None` antes dessa composição também apagaria o desconhecido. Esmeril hoje captura qualquer exceção em `ponto/services/esmeril_espelho.py:169`, silenciosamente ficando sem assinaturas A1/A6.

Consumidores subsequentes do mapa `por_dia_da_grade` também exigem atenção:

- `ponto/services/fechamento.py:1067`: soma todos os abonos.
- `folha/export.py:1109`: soma para prontidão.
- `ponto/services/dia_pago.py:479`, `:484`: retrato converte ausente/`None` em zero.
- `folha/porta_export.py:142`: comparador converte valores `None` em zero.

Escrita e semântica verificadas: builder chama `realizado_do_dia` em `escala/utils.py:2065`, grava W na linha em `:2078`; cartório copia W para ata em `ponto/services/cartorio.py:740`. Overlay atualmente acontece antes, em `escala/utils.py:1868`. O W passado explicitamente durante o julgamento deve substituir a leitura da ata antiga; leitores fora desse julgamento continuam dependentes do W lavrado.

## 14/11 21:53 · sessao 4bacac0064e3 · MAPAS-DOS-RIOS · leitura · origem=Code

# MAPA DOS RIOS — OUTBOX, CRONS E PUSH [nao verificado]

## 1. RESUMO

1. **Dois outboxes persistentes:** `EventoCelula`, com 3 estados derivados, e `MensagemSaida`, com 4 valores de status (`app/chamados/reconciliador.py:1250`; `app/chamados/reconciliador.py:1256`; `app/escala/models.py:1861`; `mensageria/nucleo/models.py:82`).
2. `EventoCelula`: **1 INSERT**, alimentado por **9 sitios em 8 funcoes**; o consumidor tambem escreve tentativas, erro e consumo (`app/ponto/portas/celula.py:954`; nascentes enumeradas em 2a; `app/chamados/reconciliador.py:1396`; `app/chamados/reconciliador.py:1402`).
3. Seu limite e **5 falhas**; a retencao remove processados depois de **7 dias**, mas nao remove mortos (`app/chamados/reconciliador.py:1210`; `app/chamados/reconciliador.py:1211`; `app/chamados/reconciliador.py:1409`).
4. `MensagemSaida`: **3 sitios de nascimento**, **1 sender**, limite de **3 tentativas**; recusa permanente descarta imediatamente (`mensageria/nucleo/management/commands/cobrar.py:60`; `mensageria/nucleo/management/commands/cobrar.py:75`; `mensageria/nucleo/management/commands/entregar.py:54`; `mensageria/nucleo/management/commands/enviar_outbox.py:15`; `mensageria/nucleo/management/commands/enviar_outbox.py:67`).
5. Crons: **89 entradas de relogio — 76 C e 13 RAW — mais 15 ENC**, total **104 declaracoes**; expansao estrutural para **105 linhas de relogio**, antes de derivar horarios (`app/config/crons.py:281`; `app/config/crons.py:755`; `app/config/crons.py:1523`; `app/config/crons.py:1801`; `app/config/crons.py:17`; `app/config/crons.py:1888`).
6. FCM: **26 chamadas literais em `app/` e 1 chamada gerada por script**; envio externo unico em `messaging.send`, sem carimbo local de consumo no aparelho nesse caminho (`app/chamados/fcm_utils.py:85`; nascentes em 2c; `bin/alarme_sessao_ociosa.py:162`).
7. Web Push: **5 sitios de driver**; expiracao desativa em 1 e apaga em 4 (`app/ponto/webpush.py:50`; `app/chamados/push_utils.py:40`; `app/ponto/management/commands/processar_alertas_turno.py:288`; `app/ponto/management/commands/processar_alertas_avancados.py:215`; `app/ponto/management/commands/processar_alertas_avancados.py:250`).
8. A fila `EnvioHolerite` tem **4 estados e 2 terminais declarados**; command e polling podem escrever erro (`app/holerite/models.py:236`; `app/holerite/models.py:241`; `app/holerite/management/commands/enviar_holerites_em_lote.py:74`; `app/holerite/views.py:690`).
9. Dois recibos podem dizer `enviado` sem confirmacao positiva do transporte: `AlertaTurno` e `cobertura.push_status` (`app/ponto/management/commands/processar_alertas_turno.py:231`; `app/ponto/management/commands/processar_alertas_turno.py:244`; `app/ponto/services/cobertura.py:160`; `app/ponto/services/cobertura.py:166`).
10. **15 selos cron** recebem `expectedFailure` ate **20/11/2026**; existencia do teste nao equivale a verde atual (`app/core/tests/marca_cron15.py:24`; `app/core/tests/marca_cron15.py:29`; `app/core/tests/marca_cron15.py:57`).

## 2. TABELA DE TRANSICOES

`Pendente vivo`, `morto` e `processado` abaixo sao abreviacoes dos predicados efetivos de `EventoCelula`, nao valores inventados de uma coluna status (`app/chamados/reconciliador.py:1250`; `app/chamados/reconciliador.py:1256`; `app/chamados/reconciliador.py:1402`).

### 2a. Outboxes

| # | estado de partida -> estado de chegada | juiz (arquivo:linha) | operacao (funcao) | escritor (arquivo:linha) | o que persiste (campos/modelos/trilha) | leitores (arquivo:linha) | prova (teste ou selo que morde, arquivo::nome, ou "nenhum") | [nao verificado]
|---|---|---|---|---|---|---|---|
| O1 | EventoCelula inexistente -> pendente vivo | Criterios das 9 nascentes abaixo; o escritor nao deduplica (`app/ponto/portas/celula.py:945`; `app/escala/models.py:1850`) | `emitir_evento_celula` | `app/ponto/portas/celula.py:954` | IDs, data, criado_em; processado_em=NULL, tentativas=0, erro vazio (`app/escala/models.py:1857`) | Selecao do consumidor e vigias (`app/chamados/reconciliador.py:1250`; `app/chamados/reconciliador.py:1274`) | `app/ponto/tests/test_o_outbox_celula.py::test_lavrar_veredito_com_save_gera_1_evento`, linha 56; `::test_save_e_evento_entram_juntos_ou_nenhum`, linha 142 |
| O2 | pendente vivo, tentativas 0–3 -> pendente vivo, tentativas+1 | Falhas acumuladas ou excecao; selecao ainda abaixo do limite (`app/chamados/reconciliador.py:1250`; `app/chamados/reconciliador.py:1389`; `app/chamados/reconciliador.py:1395`) | `consumir_eventos_celula` | `app/chamados/reconciliador.py:1396` | Incremento atomico F+1; erro concatenado, limitado a 500; processado_em continua NULL (`app/chamados/reconciliador.py:1396`) | Proxima selecao e contador de mortos (`app/chamados/reconciliador.py:1250`; `app/chamados/reconciliador.py:1256`) | `app/ponto/tests/test_o_outbox_celula.py::test_MORDE_falha_de_UM_chamado_deixa_os_eventos_do_colab_pendentes`, linha 209 |
| O3 | pendente vivo, tentativas=4 -> morto, tentativas=5 | Mesmo ramo de falha; corte em 5 (`app/chamados/reconciliador.py:1210`; `app/chamados/reconciliador.py:1395`) | `consumir_eventos_celula` | `app/chamados/reconciliador.py:1396` | tentativas=5, erro, processado_em=NULL; sai da selecao viva (`app/chamados/reconciliador.py:1250`) | `eventos_mortos` e command (`app/chamados/reconciliador.py:1256`; `app/chamados/management/commands/consumir_eventos_celula.py:37`) | **Parcial:** `app/ponto/tests/test_o_outbox_celula.py::test_MORDE_evento_morto_conta_a_parte_e_nao_cega_vigia_nem_semeadura`, linha 460; monta morto por update na linha 467 |
| O4 | pendente vivo -> processado | Sem falhas e sem adiamento (`app/chamados/reconciliador.py:1395`; `app/chamados/reconciliador.py:1397`; `app/chamados/reconciliador.py:1401`) | `consumir_eventos_celula` | `app/chamados/reconciliador.py:1402` | processado_em=instante capturado no inicio; nao limpa erro/tentativas anteriores (`app/chamados/reconciliador.py:1338`; `app/chamados/reconciliador.py:1402`) | Retencao; selecao viva passa a excluir (`app/chamados/reconciliador.py:1409`; `app/chamados/reconciliador.py:1250`) | `app/ponto/tests/test_o_outbox_celula.py::test_MORDE_consumir_2x_e_1_retratacao_0_duplicata`, linha 163; `app/chamados/tests/test_e4_c20.py::test_consumidor_chama_fabrica_antes_de_marcar_evento`, linha 30 |
| O5 | processado -> linha removida | processado_em anterior ao limite de 7 dias (`app/chamados/reconciliador.py:1211`; `app/chamados/reconciliador.py:1409`) | Retencao no fim de `consumir_eventos_celula` | `app/chamados/reconciliador.py:1409` | DELETE do evento (`app/chamados/reconciliador.py:1409`) | Consulta de retencao (`app/chamados/reconciliador.py:1409`) | nenhum |
| M1 | MensagemSaida inexistente -> pendente | Cliente ativo/nao pausado; cobranca elegivel, escalada WhatsApp ou entrega fora da sombra; dedup_key (`mensageria/nucleo/management/commands/cobrar.py:18`; `mensageria/nucleo/management/commands/cobrar.py:46`; `mensageria/nucleo/management/commands/entregar.py:35`) | `cobrar.Command.handle`; `entregar` | `mensageria/nucleo/management/commands/cobrar.py:60`; `mensageria/nucleo/management/commands/cobrar.py:75`; `mensageria/nucleo/management/commands/entregar.py:54` | Destino, canal, corpo, dedup_key unico, status default, tentativas e datas (`mensageria/nucleo/models.py:84`) | Sender; admin (`mensageria/nucleo/management/commands/enviar_outbox.py:36`; `mensageria/nucleo/admin.py:12`) | `mensageria/nucleo/tests/test_cobrar.py::test_03_dedup_por_colab_dia`, linha 46; `mensageria/nucleo/tests/test_entregar.py::test_09_fora_da_sombra_materializa_uma_linha_por_alerta_por_dia`, linha 83 |
| M2 | pendente -> pendente, com tentativa+1 | Excecao nao permanente, contador resultante <3 (`mensageria/nucleo/management/commands/enviar_outbox.py:64`; `mensageria/nucleo/management/commands/enviar_outbox.py:67`; `mensageria/nucleo/management/commands/enviar_outbox.py:69`) | `_enviar` | `mensageria/nucleo/management/commands/enviar_outbox.py:65` | tentativas+1, erro limitado a 1000, status permanece pendente; log de erro (`mensageria/nucleo/management/commands/enviar_outbox.py:66`; `mensageria/nucleo/management/commands/enviar_outbox.py:71`) | Proxima selecao; admin (`mensageria/nucleo/management/commands/enviar_outbox.py:37`; `mensageria/nucleo/admin.py:12`) | `mensageria/nucleo/tests/test_enviar_outbox.py::test_erro_transitorio_continua_pendente`, linha 130 |
| M3 | pendente -> falha | Excecao nao permanente e contador resultante >=3 (`mensageria/nucleo/management/commands/enviar_outbox.py:67`; `mensageria/nucleo/management/commands/enviar_outbox.py:69`) | `_enviar` | `mensageria/nucleo/management/commands/enviar_outbox.py:70`; `mensageria/nucleo/management/commands/enviar_outbox.py:71` | status falha, tentativa e erro (`mensageria/nucleo/management/commands/enviar_outbox.py:65`) | Admin; sender exclui por selecionar pendente (`mensageria/nucleo/admin.py:12`; `mensageria/nucleo/management/commands/enviar_outbox.py:37`) | `mensageria/nucleo/tests/test_enviar_outbox.py::test_03_falha_conta_tentativa_e_sela_na_terceira`, linha 47 |
| M4 | pendente -> descartada | RecusaPermanente: motivos/flag do core ou classificacao WhatsApp (`mensageria/nucleo/management/commands/enviar_outbox.py:19`; `mensageria/nucleo/management/commands/enviar_outbox.py:52`; `mensageria/nucleo/whatsapp.py:33`; `mensageria/nucleo/whatsapp.py:65`) | `_enviar` | `mensageria/nucleo/management/commands/enviar_outbox.py:68`; `mensageria/nucleo/management/commands/enviar_outbox.py:71` | status descartada, tentativa+1, erro e log (`mensageria/nucleo/management/commands/enviar_outbox.py:65`) | Escalada para outro canal; admin (`mensageria/nucleo/management/commands/cobrar.py:25`; `mensageria/nucleo/admin.py:12`) | `mensageria/nucleo/tests/test_enviar_outbox.py::test_sem_token_vira_descartada_na_primeira`, linha 118; `::test_descartada_nao_e_reprocessada`, linha 124 |
| M5 | pendente -> enviada | Cliente ativo, saldo do teto; push com resposta dict/ok ou WhatsApp sem excecao (`mensageria/nucleo/management/commands/enviar_outbox.py:28`; `mensageria/nucleo/management/commands/enviar_outbox.py:32`; `mensageria/nucleo/management/commands/enviar_outbox.py:49`; `mensageria/nucleo/management/commands/enviar_outbox.py:61`) | `_enviar` | `mensageria/nucleo/management/commands/enviar_outbox.py:74` | status enviada, enviada_em=now, erro vazio; nao persiste ID do provedor (`mensageria/nucleo/management/commands/enviar_outbox.py:74`; `mensageria/nucleo/models.py:84`) | Teto, escalada, admin (`mensageria/nucleo/management/commands/enviar_outbox.py:29`; `mensageria/nucleo/management/commands/cobrar.py:25`; `mensageria/nucleo/admin.py:12`) | `mensageria/nucleo/tests/test_enviar_outbox.py::test_01_envia_e_respeita_teto`, linha 24; `::test_04_enviada_nao_reenvia`, linha 58 |

**Nascentes completas do INSERT O1 — 9 sitios, 8 funcoes:**

- `reverter_regeneracao_dia`: `app/ponto/portas/celula.py:838`.
- `salvar_e_emitir`: `app/ponto/portas/celula.py:965`; exige intersecao dos campos salvos com `{veredito, ata}` (`app/ponto/portas/celula.py:942`; `app/ponto/portas/celula.py:961`; `app/ponto/portas/celula.py:964`).
- `ChamadoColaborador.abrir`: `app/chamados/models.py:1027`; criterio no sitio `app/chamados/models.py:1018`.
- `ChamadoColaborador.criar`: `app/chamados/models.py:1146`; criterio no sitio `app/chamados/models.py:1144`.
- `PerguntaDisputa.acordar_celulas`: `app/chamados/models.py:3304`; resolucao/dedup das celulas em `app/chamados/models.py:3295`.
- `_cartorio_post_save_chamado`: `app/chamados/signals.py:209`; criterios em `app/chamados/signals.py:193` e `app/chamados/signals.py:203`; INSERT protegido por savepoint em `app/chamados/signals.py:207`.
- `semear_eventos_celula`: dois universos, dois sitios: `app/chamados/reconciliador.py:1298` e `app/chamados/reconciliador.py:1311`; ausencia de pendente vivo conferida em `app/chamados/reconciliador.py:1296` e `app/chamados/reconciliador.py:1309`.
- `_emitir_eventos_dos_turnos_fechados`: `app/ponto/nucleo.py:93`; chamada e filtro em `app/ponto/nucleo.py:64` e `app/ponto/nucleo.py:92`.

Ancestrais imediatos de `salvar_e_emitir`: `app/ponto/portas/celula.py:1066`, `app/ponto/portas/celula.py:1098`, `app/ponto/portas/celula.py:1167`, `app/ponto/portas/celula.py:1201`, `app/ponto/portas/celula.py:1216`. O ramo que salva somente status nao emite, pela intersecao dos campos (`app/ponto/portas/celula.py:1083`; `app/ponto/portas/celula.py:961`).

Ancestrais imediatos de `acordar_celulas`, citados somente como sitios cruzados: `app/chamados/signals.py:259`; `app/chamados/models.py:1615`; `app/chamados/models.py:3063`; `app/chamados/models.py:4010`; `app/chamados/services/fio.py:934`; `app/chamados/services/fio.py:1046`; `app/chamados/services/disputa_emissao.py:1633`.

**Quem consome e quem marca O2–O5:** o command chama o consumidor em `app/chamados/management/commands/consumir_eventos_celula.py:31`; seu cron esta em `app/config/crons.py:394`. A exclusao de execucao esta no command, mediante advisory lock, e nao dentro da funcao consumidora (`app/chamados/management/commands/consumir_eventos_celula.py:19`; `app/chamados/reconciliador.py:1236`). `--semear` emite e retorna sem consumir (`app/chamados/management/commands/consumir_eventos_celula.py:28`).

O consumidor chama os sitios cruzados em `app/chamados/reconciliador.py:1365`, `app/chamados/reconciliador.py:1366`, `app/chamados/reconciliador.py:1369`, `app/chamados/reconciliador.py:1373`, `app/chamados/reconciliador.py:1376`, `app/chamados/reconciliador.py:1384` e `app/chamados/reconciliador.py:1388`; so depois escreve a marca (`app/chamados/reconciliador.py:1393`). Se o colaborador nao existir, pode marcar processado sem executar esses sitios (`app/chamados/reconciliador.py:1357`; `app/chamados/reconciliador.py:1358`; `app/chamados/reconciliador.py:1402`).

Adiamento **nao constitui estado novo**: mantem NULL e nao incrementa tentativa (`app/chamados/reconciliador.py:1379`; `app/chamados/reconciliador.py:1397`). Prova: `app/chamados/tests/test_orfas_pelo_evento.py::test_MORDE_lampada_de_hoje_encerra_amanha_sem_batida_nova`, linha 235.

**Nascentes M1–M5:** comandos `cobrar`, `entregar` e `enviar_outbox` (`mensageria/nucleo/management/commands/cobrar.py:39`; `mensageria/nucleo/management/commands/entregar.py:79`; `mensageria/nucleo/management/commands/enviar_outbox.py:22`). `entregar` tambem chega pelo cron RAW da sentinela (`app/config/crons.py:688`; `mensageria/bin/sentinela_diario.sh:14`). Agendamento efetivo de `cobrar` e `enviar_outbox`: **[nao verificado]**.

O rio entre servicos e: sender → POST do cliente → API mensageria → FCM → fallback Web Push → resposta `ok` → status da mensagem (`mensageria/nucleo/management/commands/enviar_outbox.py:46`; `mensageria/nucleo/core_client.py:83`; `app/api/views_mensageria.py:495`; `app/api/views_mensageria.py:506`; `app/api/views_mensageria.py:516`; `mensageria/nucleo/management/commands/enviar_outbox.py:74`).

### 2b. Fila de envio e recibos de notificacao

| # | estado de partida -> estado de chegada | juiz (arquivo:linha) | operacao (funcao) | escritor (arquivo:linha) | o que persiste (campos/modelos/trilha) | leitores (arquivo:linha) | prova (teste ou selo que morde, arquivo::nome, ou "nenhum") | [nao verificado]
|---|---|---|---|---|---|---|---|
| H1 | EnvioHolerite inexistente -> pendente | Selecao dos ainda nao avisados, conforme modo, e quantidade acima do corte (`app/holerite/views.py:635`; `app/holerite/views.py:636`) | `_talvez_job`; nascentes em `app/holerite/views.py:615` e `app/holerite/views.py:740` | `app/holerite/views.py:638` | Alvo congelado, total, opcoes, criador; default pendente; depois pid/iniciado_em e evento de disparo (`app/holerite/models.py:252`; `app/holerite/views.py:644`; `app/holerite/views.py:671`) | Polling e command (`app/holerite/views.py:729`; `app/holerite/management/commands/enviar_holerites_em_lote.py:43`) | `app/holerite/tests/test_selo_job_envio.py::test_acima_do_corte_o_request_NAO_empurra_nada`, linha 80 |
| H2 | pendente -> enviando | Estado pertence a EM_CURSO (`app/holerite/management/commands/enviar_holerites_em_lote.py:47`) | `Command._rodar`, disparado por Popen ou command (`app/holerite/views.py:664`; `app/holerite/views.py:669`) | `app/holerite/management/commands/enviar_holerites_em_lote.py:51` | estado, pid e iniciado_em (`app/holerite/management/commands/enviar_holerites_em_lote.py:52`) | Tela e detector de morte (`app/holerite/views.py:699`; `app/holerite/models.py:283`) | `app/holerite/tests/test_selo_job_envio.py::test_o_job_envia_carimba_progresso_e_fecha_o_placar`, linha 143 |
| H3 | enviando -> concluido | `servicos.enviar` retorna sem excecao (`app/holerite/management/commands/enviar_holerites_em_lote.py:66`; `app/holerite/management/commands/enviar_holerites_em_lote.py:79`) | `Command._rodar` | `app/holerite/management/commands/enviar_holerites_em_lote.py:79` | estado, placar, progresso=total, concluido_em, pid=None (`app/holerite/management/commands/enviar_holerites_em_lote.py:80`) | Tela termina polling (`app/holerite/views.py:702`) | `app/holerite/tests/test_selo_job_envio.py::test_o_job_envia_carimba_progresso_e_fecha_o_placar`, linha 143 |
| H4 | enviando -> erro | Excecao no envio **ou** processo considerado morto no polling (`app/holerite/management/commands/enviar_holerites_em_lote.py:71`; `app/holerite/views.py:688`; `app/holerite/models.py:283`) | `Command._rodar`; `_tela_envio` | `app/holerite/management/commands/enviar_holerites_em_lote.py:74`; `app/holerite/views.py:690` | estado erro, mensagem, concluido_em, pid=None (`app/holerite/management/commands/enviar_holerites_em_lote.py:75`; `app/holerite/views.py:691`) | Tela terminal (`app/holerite/views.py:702`) | **Ramo processo morto:** `app/holerite/tests/test_selo_job_envio.py::test_MORDE_job_morto_vira_erro_e_nao_barra_eterna`, linha 221 |
| H5 | pendente -> erro | `job_morreu`: estado em curso, pid presente, carencia vencida e ProcessLookupError (`app/holerite/models.py:283`; `app/holerite/models.py:285`; `app/holerite/models.py:289`) | `_tela_envio`, tambem chamada pelo polling (`app/holerite/views.py:688`; `app/holerite/views.py:729`) | `app/holerite/views.py:690` | estado erro, mensagem, concluido_em, pid=None (`app/holerite/views.py:691`) | Tela terminal (`app/holerite/views.py:702`) | nenhum especifico para partida pendente |
| B1 | recibo de cobertura inexistente -> push_status=pendente | Validacoes de `designar_cobertura` e criacao atomica (`app/ponto/services/cobertura.py:52`; `app/ponto/services/cobertura.py:98`) | `designar_cobertura` | `app/ponto/services/cobertura.py:101` | push_status pendente; push_enviado_em inicialmente NULL (`app/ponto/services/cobertura.py:112`; `app/ponto/models.py:1115`) | Emissor sincrono (`app/ponto/services/cobertura.py:156`) | `app/ponto/tests/test_cobertura_caracterizacao.py::test_caminho_feliz_201`, linha 109 |
| B2 | recibo inexistente -> push_status='' | Declaracao passada com substituto, marcos e ordem ainda inexistente (`app/ponto/services/cobertura.py:275`; `app/ponto/services/cobertura.py:281`; `app/ponto/services/cobertura.py:286`; `app/ponto/services/cobertura.py:290`) | `declarar_cobertura_passada` | `app/ponto/services/cobertura.py:291` | push_status vazio; esse ramo deliberadamente nao envia (`app/ponto/services/cobertura.py:294`; `app/ponto/services/cobertura.py:203`) | Sitio de reutilizacao da ordem (`app/ponto/services/cobertura.py:288`) | `app/ponto/tests/test_declarar_cobertura_passada.py::test_MORDE_com_nome_a_ordem_nasce_concluida_no_turno_e_sem_push`, linha 54 |
| B3 | push_status=pendente -> enviado | push_ok: Web Push sem excecao ou FCM com ok; retorno negativo Web Push e ignorado (`app/ponto/services/cobertura.py:160`; `app/ponto/services/cobertura.py:166`; `app/ponto/services/cobertura.py:178`) | `designar_cobertura` | `app/ponto/services/cobertura.py:185` | push_status enviado e push_enviado_em=now (`app/ponto/services/cobertura.py:186`) | Resposta JSON (`app/ponto/services/cobertura.py:192`) | *

[truncado em 20000 de 49172 caracteres]

## 14/11 21:51 · sessao 5d7ac1db0277 · MAPAS-DOS-RIOS · leitura · origem=Code

# MAPA DOS RIOS — VINCULO E ESCALA

Referencias relativas a `app/`. Hash informado: `a0bf6e78e9a2e7d67f9efee9b6a14141e33e3a5f`; correspondencia com um commit Git [nao verificado].

## 1. RESUMO

1. O vinculo e `EscalaColaborador`: estado composto por `ativa`, inicio, fim, template e posto; nao ha enum de situacao do vinculo (`escala/models.py:871`).
2. As portas declaradas sao **3 modulos**: vinculo, cadastro de template e celula (`core/portas.py:112`, `core/portas.py:470`, `core/portas.py:474`).
3. `executar_vinculo` tem **4 chamadas diretas** de producao: tela, regua, chat e comando (`colaboradores/views.py:423`, `escala/regua_defesa.py:321`, `colaboradores/services/troca_pelo_chat.py:75`, `ponto/management/commands/censo_fase_12x36.py:423`).
4. Template tem **6 funcoes de persistencia**, alimentadas por **7 sitios diretos** (`escala/services/cadastro_tipo.py:116`, `:141`, `:382`, `:397`, `:421`, `:431`; nascentes discriminadas abaixo).
5. A porta da celula declara **4 operacoes**; o modelo tem **2 origens** e `trabalha` admite **3 valores** (`ponto/portas/celula.py:74`, `escala/models.py:1686`, `escala/models.py:1690`).
6. Celula tem **10 vereditos de cartorio, 3 humanos e 11 status**; sao campos distintos da origem (`escala/models.py:1751`, `escala/models.py:1759`, `escala/models.py:1813`).
7. Regeneracao de vinculo tem **13 chamadas externas**; regeneracao de um dia tem **2**; reversao tem **1** — sitios enumerados abaixo (`ponto/portas/celula.py:112`, `ponto/portas/celula.py:741`, `ponto/portas/celula.py:779`).
8. Ha **1 ramo** que reescreve DNA sem atualizar `dna_anterior`, `regenerada_em` ou `regeneracoes`: perda de cobertura em data `<=hoje` (`ponto/portas/celula.py:272`, `ponto/portas/celula.py:279`).
9. Edicao humana existe na porta; nascente externa de `editar_dia`/`trocar_plantao` nao foi localizada; alcance por despacho dinamico [nao verificado] (`ponto/portas/celula.py:572`, `ponto/portas/celula.py:670`, `ponto/portas/celula.py:713`).
10. O selo chamado “UM_ESCRITOR” de vigencia verifica passagem pela lei, permitindo varias escritoras; nao prova uma funcao escritora unica (`escala/tests/test_vigencia_um_escritor.py:174`, `escala/tests/test_vigencia_um_escritor.py:206`).

## 2. TABELA DE TRANSICOES

As listas de nascentes depois das tabelas integram as linhas indicadas. Recusa e repeticao sem mudanca nao ganharam transicao propria: o codigo possui retornos sem escrita para esses casos (`colaboradores/services/vinculo.py:119`, `ponto/portas/celula.py:1076`).

### VINCULO

| # | estado de partida -> estado de chegada | juiz (arquivo:linha) | operacao (funcao) | escritor (arquivo:linha) | o que persiste (campos/modelos/trilha) | leitores (arquivo:linha) | prova (teste ou selo que morde, arquivo::nome, ou "nenhum") | [nao verificado]
|---|---|---|---|---|---|---|---|
| V1 | inexistente -> vinculo ativo | `julgar_vinculo`: piso, posto, desligamento, ancora e identidade (`colaboradores/services/vinculo.py:52`, `colaboradores/services/vinculo.py:133`) | `executar_vinculo`; nascentes NV | `colaboradores/services/vinculo.py:287` | Tipo/posto/inicio/ativa/ancora/origem/feriado; HistoricoVinculo e LogAuditoria (`colaboradores/services/vinculo.py:354`, `colaboradores/services/vinculo.py:381`) | `escala/alimentacao.py:148` | `colaboradores/tests/test_trilha_do_vinculo.py:121` :: `test_vinculo_novo_grava_CRIAR_e_o_anterior_fechado_grava_EDITAR` |
| V2 | linha existente ativa/inativa -> ativa, fim nulo, cadastro atualizado | Identidade por colaborador+inicio e intencao; invalidacao de ancora automatica por geometria (`colaboradores/services/vinculo.py:101`, `colaboradores/services/vinculo.py:268`) | `executar_vinculo`; NV | `colaboradores/services/vinculo.py:274`, `colaboradores/services/vinculo.py:283` | Tipo/posto/ativa/fim/ancora; Log com diferencas e Historico (`colaboradores/services/vinculo.py:349`, `colaboradores/services/vinculo.py:370`) | `escala/alimentacao.py:269` | `colaboradores/tests/test_trilha_do_vinculo.py:84` :: `test_MORDE_trocar_tipo_de_vinculo_existente_grava_EDITAR_com_antes_e_depois` |
| V3 | vinculo selecionado -> inativo, fim determinado | Filtro inicio<=fim no lote; `validar_vigencia` no encerramento humano (`colaboradores/services/vinculo.py:622`, `colaboradores/services/vinculo.py:641`, `colaboradores/services/vinculo.py:835`) | `fechar_vigencia`, `fechar_vinculos_ativos`, `encerrar_vinculo`; NF | Update: `colaboradores/services/vinculo.py:641`; save: `colaboradores/services/vinculo.py:845` | `ativa=False`, `data_fim`; lote registra por PK quando recebe autor; humano registra diferenca; regeneracao futura e, no save, signals (`colaboradores/services/vinculo.py:642`, `:659`, `:847`; `escala/signals.py:35`) | `escala/alimentacao.py:205` | `escala/tests/test_vigencia_um_escritor.py:72` :: `test_MORDE_o_PAR_fecha_o_anterior_e_NAO_toca_o_posterior`; `:119` :: `test_MORDE_trilha_que_falha_DESFAZ_a_escrita` |
| V4 | ativa com inicio>=D -> inativa, fim preservado | Filtro de ativas/inicio e exclusao do PK preservado (`colaboradores/services/vinculo.py:788`) | `absorver_vigencias_posteriores`; chamadas `colaboradores/services/vinculo.py:253`, `escala/management/commands/corrigir_vinculo_vigencia.py:120` | `colaboradores/services/vinculo.py:799` | So `ativa=False`; Log condicionado ao autor; Historico com motivo `vinculo_absorvido` (`colaboradores/services/vinculo.py:808`, `colaboradores/services/vinculo.py:817`) | `escala/alimentacao.py:189` | `colaboradores/tests/test_vinculo_linha_do_tempo.py:95` :: `test_MORDE_a_absorcao_deixa_trilha_nos_DOIS_lugares` |
| V5 | fim<inicio -> ativa, intervalo reparado | Estado atual impossivel, novo intervalo valido e motivo quando ha autor (`colaboradores/services/vinculo.py:715`, `:724`, `:727`) | `reabrir_vigencia_impossivel`; comando `escala/management/commands/corrigir_vinculo_vigencia.py:86` | `colaboradores/services/vinculo.py:730` | Fim/ativa; Log+Historico condicionados ao autor; futuras regeneradas (`colaboradores/services/vinculo.py:731`, `colaboradores/services/vinculo.py:750`) | `escala/alimentacao.py:205` | `escala/tests/test_porta_repara_vigencia_impossivel.py:69` :: `test_MORDE_vinculo_SADIO_e_RECUSADO`; `:88` :: `test_MORDE_IDEMPOTENTE_chamar_2x_nao_grava_2a_trilha` |
| V6 | inexistente -> trecho retroativo inativo | Intervalo e ancora futura (`colaboradores/services/vinculo.py:969`, `colaboradores/management/commands/corrigir_escala_retroativa.py:185`) | `criar_trecho_retroativo`; comando `colaboradores/management/commands/corrigir_escala_retroativa.py:196` | `colaboradores/services/vinculo.py:970` | Tipo/posto/inicio/fim/ancora/feriado, `ativa=False`; Log no chamador (`colaboradores/management/commands/corrigir_escala_retroativa.py:198`) | `escala/alimentacao.py:148` | `escala/tests/test_vigencia_um_escritor.py:171` :: `test_UM_ESCRITOR_nenhum_outro_sitio_grava_data_fim_de_vinculo` — estrutural |
| V7 | vinculo existente -> tipo/ancora/fim corrigidos no lugar | Intervalo e ancora (`colaboradores/services/vinculo.py:979`, `colaboradores/management/commands/corrigir_escala_retroativa.py:185`) | `corrigir_vinculo_retroativo`; comando `colaboradores/management/commands/corrigir_escala_retroativa.py:212`; propositor `escala/services/propositor.py:765` | `colaboradores/services/vinculo.py:983` | Tipo/ancora/fim; Log no chamador (`colaboradores/management/commands/corrigir_escala_retroativa.py:213`) | `escala/signals.py:56`, `escala/signals.py:113` | Mesmo selo estrutural de V6 (`escala/tests/test_vigencia_um_escritor.py:171`) |
| V8 | linha atual -> valores do snapshot | Valida todos intervalos e recusa remocao de geradora de celula editada (`colaboradores/services/vinculo.py:1015`, `colaboradores/services/vinculo.py:1027`) | `reverter_para_snapshot`; recusa de aceite em `chamados/services/acoes_chamado.py:149` | `colaboradores/services/vinculo.py:1031` | Tipo/inicio/fim/ativa do snapshot; suplemento futuro no atomic (`colaboradores/services/vinculo.py:1034`) | `escala/alimentacao.py:269` | `colaboradores/tests/test_escala_aceite.py:126` :: `test_recusar_escala_reverte_ao_anterior` |
| V9 | vinculo extra ou ajuste desfeito -> linha excluida | Snapshot protege geradora humana; desfazer protege geradora humana e valida ajuste (`colaboradores/services/vinculo.py:1027`, `:1103`; `escala/services/escala_auto_executor.py:128`) | `reverter_para_snapshot`, `desfazer_ajuste_do_sistema`; nascentes V8 e NA | `colaboradores/services/vinculo.py:1029`, `colaboradores/services/vinculo.py:1105` | Exclusao da linha; FK geradora da celula declara `SET_NULL` (`escala/models.py:1693`); execucao dessa cascata [nao verificado] | `escala/alimentacao.py:148` | `ponto/tests/test_cadastro_futuro_sem_banco.py:554` :: `test_snapshot_com_editada_recusa_antes_de_apagar_geradora`; `:564` :: `test_desfazer_auto_preserva_geradora_da_editada` |
| V10 | inexistente -> ativo com template derivado do sistema | Aval com `!`, propostas automaticas e inicio futuro (`escala/services/escala_auto_executor.py:96`) | `ajustar_vinculo_pelo_sistema`; NA; fechamento anterior em V3 | `colaboradores/services/vinculo.py:1089` | Novo tipo/inicio; preserva posto/fase/origem/feriado/calendario; Historico no executor (`escala/services/escala_auto_executor.py:110`) | `escala/alimentacao.py:269` | `escala/tests/test_escala_auto_executor.py:66` :: `test_MORDE_template_derivado_trilha_e_passado_intocado` |
| V11 | antigo fechado -> antigo ativo, fim nulo | Desfazer valido e volta igual ao inicio do ajuste (`escala/services/escala_auto_executor.py:128`, `colaboradores/services/vinculo.py:1101`) | `desfazer_ajuste_do_sistema`; NA | `colaboradores/services/vinculo.py:1106` | Reabre antiga; exclusao do novo em V9; Historico no executor (`escala/services/escala_auto_executor.py:149`) | `escala/alimentacao.py:205` | `escala/tests/test_escala_auto_executor.py:86` :: `test_MORDE_desfazer_volta_o_vinculo_antigo` |
| V12 | inexistente -> novo ativo com tipo antigo, a partir da volta | Desfazer valido e volta diferente do inicio do ajuste (`escala/services/escala_auto_executor.py:128`, `colaboradores/services/vinculo.py:1101`) | `desfazer_ajuste_do_sistema`; NA; fecha ajuste por V3 | `colaboradores/services/vinculo.py:1110` | Nova linha com tipo/posto/fase/feriado/calendario antigos (`colaboradores/services/vinculo.py:1111`) | `escala/alimentacao.py:269` | Teste de desfazer em `escala/tests/test_escala_auto_executor.py:86`; cobertura especifica deste ramo [nao verificado] |
| V13 | ancora preenchida -> ancora nula | Geometria alterada+origem automatica, ou ancora anterior ao inicio em ciclo permitido (`colaboradores/services/vinculo.py:268`, `colaboradores/services/vinculo.py:990`) | `executar_vinculo`; `invalidar_ancoras_incoerentes`, chamado por `escala/utils.py:2110`, desde `colaboradores/services/vinculo.py:309` | Save `colaboradores/services/vinculo.py:283`; update `colaboradores/services/vinculo.py:995` | Ancora nula; update nao atravessa save/signals (`colaboradores/services/vinculo.py:995`) | `escala/models.py:1252` | `colaboradores/tests/test_vinculo_ancora_geometria.py:56` :: `test_geometria_muda_invalida_e_cobra_declaracao` |
| V14 | calendario atual -> outro calendario/None | Compara antes/depois (`colaboradores/services/vinculo.py:459`) | `trocar_calendario_de_folga`; tela `colaboradores/views.py:239`; lote `escala/views.py:409` | `colaboradores/services/vinculo.py:462` | Calendario; sincronizacao futura; Log com ambos valores quando muda (`colaboradores/services/vinculo.py:465`, `colaboradores/services/vinculo.py:467`) | `escala/models.py:1252` | `colaboradores/tests/test_trilha_do_vinculo.py:141` :: `test_MORDE_trocar_calendario_grava_editar_com_os_dois_calendarios_e_e_idempotente` |
| V15 | calendario nulo -> padrao do template | Padrao existe e escolha manual ausente (`colaboradores/services/vinculo.py:157`) | `_copiar_folga_padrao`; executor `colaboradores/services/vinculo.py:284`, `colaboradores/services/vinculo.py:298` | `colaboradores/services/vinculo.py:162` | Calendario e foto corrente/proxima (`colaboradores/services/vinculo.py:165`) | `escala/models.py:1252` | `colaboradores/tests/test_vinculo_folga_padrao.py:52` :: `test_create_copia_padrao_e_materializa_foto` |
| V16 | trabalha_em_feriado atual -> desejado | Igualdade encerra sem efeito (`colaboradores/services/vinculo.py:890`) | `marcar_trabalha_em_feriado`; comando `colaboradores/management/commands/sincronizar_feriado_admin.py:56` | `colaboradores/services/vinculo.py:894` | Flag; regeneracao passada e futura delegada a porta da celula (`colaboradores/services/vinculo.py:904`, `colaboradores/services/vinculo.py:910`) | `escala/models.py:1373` | `ponto/tests/test_cadastro_futuro_sem_banco.py:445` :: `test_marcar_trabalha_em_feriado` |
| V17 | escore/classe/contador atuais -> resultado da avaliacao | Diagnostico, alvo, corroboracao e histerese (`escala/regua_defesa.py:735`, `:737`, `:740`, `:754`) | `avaliar_e_gravar_regua`; comando `escala/management/commands/termometro_regua.py:43`; cron `config/crons.py:428` | `gravar_regua`: `colaboradores/services/vinculo.py:1070` | Confianca/classe/avaliada_em/dias_baixo; Log de diferencas, excluindo carimbo isolado (`escala/regua_defesa.py:765`, `colaboradores/services/vinculo.py:1072`) | `escala/regua_defesa.py:641` | `escala/tests/test_termometro_grava_com_trilha.py:35` :: `test_MORDE_avaliar_que_derruba_o_escore_grava_editar_com_antes_e_depois` |
| V18 | sem corroboracao -> cadastro confirmado como errado | Vinculo existe e nao estava corroborado (`escala/regua_defesa.py:655`) | `corroborar_regua`; lista `escala/views.py:459`; legado `chamados/views.py:443` | `escala/regua_defesa.py:662` -> `colaboradores/services/vinculo.py:1070` | Escore 0, dias_baixo 2, corroboracao/em/por; Log na porta (`escala/regua_defesa.py:666`, `colaboradores/services/vinculo.py:1077`) | `escala/regua_defesa.py:644` | `escala/tests/test_termometro_vira_lista.py:89` :: `test_MORDE_corroborar_pela_view_grava_o_ip_e_o_usuario` |
| V19 | corroborada -> sem corroboracao, escore>=faixa incerta | Corroboracao existe e motivo preenchido (`escala/regua_defesa.py:709`) | `desfazer_corroboracao`; comando com apply `escala/management/commands/desfazer_corroboracao_regua.py:44` | `escala/regua_defesa.py:719` -> `colaboradores/services/vinculo.py:1070` | Limpa corroboracao, zera dias_baixo, ajusta escore; trilha nomeada; rejulgamento (`escala/regua_defesa.py:722`, `escala/regua_defesa.py:725`) | `escala/regua_defesa.py:644` | `escala/tests/test_termometro_grava_com_trilha.py:81` :: `test_corroborar_e_desfazer_tem_uma_linha_editar_cada_com_o_usuario` |
| V20 | corroborada -> corroboracao expirada pelo diagnostico | Corroborada e diagnostico confiavel (`escala/regua_defesa.py:741`) | `avaliar_e_gravar_regua`; mesma nascente/cron de V17 | `escala/regua_defesa.py:742` -> `colaboradores/services/vinculo.py:1070` | Limpa corroboracao; depois grava avaliacao V17 (`escala/regua_defesa.py:743`, `escala/regua_defesa.py:765`) | `escala/regua_defesa.py:740` | `escala/tests/test_termometro_camada3.py:33` :: `test_corroborar_liga_o_veto_na_hora_e_expira_ao_corrigir` |

**NV — nascentes do nascimento/troca.** As quatro chamadas diretas sao tela (`colaboradores/views.py:423`), regua (`escala/regua_defesa.py:321`), chat (`colaboradores/services/troca_pelo_chat.py:75`) e ramo aplicador do censo (`ponto/management/commands/censo_fase_12x36.py:423`). A regua recebe lista e acao legada (`escala/views.py:471`, `chamados/views.py:451`, `escala/regua_defesa.py:329`); o chat recebe chamada e declaracao dinamica (`core/views_ia.py:1008`, `core/intake.py:121`). O cron do censo usa `--lavrar`, sem acionar esse ramo aplicador (`config/crons.py:638`).

**NF — nascentes do fechamento.** `fechar_vigencia` recebe executor de vinculo em dois sitios, desligamento, ajuste, desfazer e comando de vigencia (`colaboradores/services/vinculo.py:214`, `:241`, `:862`, `:1087`, `:1108`; `escala/management/commands/corrigir_vinculo_vigencia.py:107`). `encerrar_vinculo` recebe tela e comando de vinculo de desligado (`escala/views.py:269`, `colaboradores/management/commands/fechar_vinculo_de_desligado.py:56`). O sitio externo de desligamento chama a porta em `colaboradores/services/desligamento.py:148`; suas nascentes sao tela e comando agendado (`colaboradores/views.py:1437`, `colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`), com cron diario **06:06 --apply** (`config/crons.py:440`).

**NA — ajuste/desfazer do sistema.** O executor chama ajustar e desfazer (`escala/services/escala_auto_executor.py:103`, `escala/services/escala_auto_executor.py:145`); o comando oferece ambos os caminhos (`escala/management/commands/ajustar_escalas_auto.py:95`, `escala/management/commands/ajustar_escalas_auto.py:39`). O catalogo declara esse comando fora dos crons (`config/crons.py:1194`).

### ESCALA / TEMPLATE

| # | estado de partida -> estado de chegada | juiz (arquivo:linha) | operacao (funcao) | escritor (arquivo:linha) | o que persiste (campos/modelos/trilha) | leitores (arquivo:linha) | prova (teste ou selo que morde, arquivo::nome, ou "nenhum") | [nao verificado]
|---|---|---|---|---|---|---|---|
| T1 | inexistente -> template editavel | Payload: nome, ciclo, base, folga e horas; validacao do modelo (`escala/services/cadastro_tipo.py:148`, `:152`, `:221`; `escala/models.py:798`) | `salvar_tipo`; wizard `escala/views_wizard.py:108` | `escala/services/cadastro_tipo.py:175`, `escala/services/cadastro_tipo.py:307` | `is_template=True`, `editavel=True`, desenho, hierarquia, jornada/folgas/HE (`escala/services/cadastro_tipo.py:175`, `:184`) | `escala/views.py:83`, `ponto/management/commands/gerar_celulas.py:85` | `escala/tests/test_cadastro_tipo_service.py:11` :: `test_criar_completo_um_passo` |
| T2 | template editavel -> desenho alterado | `editavel`; validacoes payload/modelo (`escala/services/cadastro_tipo.py:145`, `escala/models.py:798`) | `salvar_tipo`; wizard `escala/views_wizard.py:108`; aceite de folga `chamados/services/limbo_folgas.py:92`, vindo de `chamados/views.py:433` | `escala/services/cadastro_tipo.py:307` | Desenho; modo de intervalo derivado no save; propagacao passada e futura; invalida fechamento (`escala/models.py:790`, `escala/services/cadastro_tipo.py:308`, `:353`, `:363`, `:376`) | `ponto/management/commands/gerar_celulas.py:90` | `escala/tests/test_cadastro_tipo_service.py:63` :: `test_editar_nao_muda_canonico`; `:81` :: `test_editar_guard_editavel`; `:101` :: `test_editar_apagar_intervalo_volta_duracao` |
| T3 | inexistente -> tipo cadastrado pelo admin | Validacao propria do modelo; ModelForm declarado, implementacao externa [nao verificado] (`escala/models.py:798`, `escala/services/cadastro_tipo.py:383`) | `salvar_tipo_no_admin`; `TipoEscalaAdmin.save_model` (`escala/admin.py:15`) | `escala/services/cadastro_tipo.py:385` | Objeto do formulario; save deriva codigo/nivel/intervalo (`escala/models.py:763`, `:772`, `:790`) | `escala/views.py:83` | `escala/tests/test_chokepoint_escala_gate.py:55` :: `test_MORDE_nenhuma_escrita_da_familia_fora_da_porta` — estrutural |
| T4 | tipo existente -> alterado pelo admin | Modelo valida; esta funcao nao testa `editavel` (`escala/models.py:798`, `escala/services/cadastro_tipo.py:382`) | `salvar_tipo_no_admin`; `escala/admin.py:15` | `escala/services/cadastro_tipo.py:385` | Alteracao e propagacao **somente futura** neste caminho (`escala/services/cadastro_tipo.py:386`) | `ponto/management/commands/gerar_celulas.py:85` | Mesmo selo estrutural (`escala/tests/test_chokepoint_escala_gate.py:55`); comportamental especifico: nenhum |
| T5 | inexistente -> copia editavel do original | Permissao na view e validacao do modelo (`escala/views.py:139`, `escala/models.py:798`) | `copiar_tipo`; view `escala/views.py:148` | `escala/services/cadastro_tipo.py:399` | **18 campos explicitos**; mesmo pai do original, `is_template=True`, `editavel=True`; nao e clone integral (`escala/services/cadastro_tipo.py:399`) | `escala/views

[truncado em 20000 de 45877 caracteres]

## 14/11 21:51 · sessao b168aae3dfe7 · MAPAS-DOS-RIOS · leitura · origem=Code

# MAPA DOS RIOS — BATIDA

Referencias relativas a `app/`. `∅` indica ausencia do registro/item, nao um estado adicional. Os estados abaixo descrevem campos existentes: `tipo`, `origem`, `aprovada`, `retratada_em`, `foto` e `pergunta_origem` (`ponto/models.py:24`, `ponto/models.py:40`, `ponto/models.py:45`, `ponto/models.py:63`, `ponto/models.py:67`, `ponto/models.py:78`).

## 1. RESUMO

1. O modelo declara **2 tipos**, **6 origens** e nenhum campo `status`; o plantio usa ainda **2 origens fora das choices**: `disputa_s84_retro` e `disputa_ancora` (`ponto/models.py:8-109`; `chamados/services/materializacao.py:934`, `chamados/services/materializacao.py:1014`).
2. Nascimento: **11 chamadas de `registrar_batida`** — 3 app, 4 manual, 1 excecao, 1 regularizacao e 2 disputa; sitios discriminados na matriz (`api/views.py:654`; `api/views_core.py:645`; `ponto/services/triagem_batida.py:702`; `ponto/services/justificativa.py:188`, `ponto/services/justificativa.py:361`; demais nas linhas #5–9).
3. A porta declarada e **1 modulo**, mas existem **2 sitios escritores fora dela**: corrigir tipo/instante e apagar pela regularizacao externa (`core/portas.py:103`; `chamados/services/regularizacao_ext.py:137`, `chamados/services/regularizacao_ext.py:154-156`).
4. Retratar tem **4 sitios persistentes de chamada direta**; desfazer tem **1 nascente**, com prazo de **7 dias** e exigencia de ultima trilha humana (`chamados/services/acoes_chamado.py:277`; `ponto/services/retratar_humano.py:179`; `ponto/management/commands/detectar_par_relampago.py:224`; `ponto/management/commands/passivo_disputa_retro.py:164`; `ponto/views.py:3332`; `ponto/services/retratar_humano.py:32`, `ponto/services/retratar_humano.py:48-53`).
5. Flip tem **3 nascentes**; foto tem **7**; anotacao direta externa tem **8 sitios** (`ponto/views.py:3306`; `ponto/services/veredito_celula.py:317`; `ponto/management/commands/flip_automatico.py:68`; catalogo nas linhas #18–20 e seus escritores).
6. A fila PWA tem **3 chamadas nas telas atuais**, **3 gatilhos de sincronizacao** e **2 ramos de remocao**: sucesso HTTP e erro 400 (`templates/ponto/bater_ponto_v2.html:333`, `templates/ponto/bater_ponto_v2.html:346`; `templates/ponto/triagem/apto.html:282`; `static/pwa.js:16`, `static/pwa.js:320`, `static/pwa.js:328`, `static/pwa.js:158-175`).
7. Universo apuravel: `retratada_em IS NULL`; **nao exige aprovacao nem origem especifica**. Retratar/desfazer retiram/recolocam a mesma Batida nesse universo (`ponto/turnos.py:2876-2883`; `ponto/registro_batida.py:274`; `ponto/services/retratar_humano.py:207`).
8. Os testes abaixo foram lidos; resultado de execucao e volumes atuais de BATIDA em producao: **[nao verificado]**. [nao verificado]

## 2. TABELA DE TRANSICOES

O juiz comum de nascimento faz: dedup do mesmo tipo ativo em ±60 segundos; reuso da chave exata, inclusive retratada; guarda de competencia quando `fato_de_chao=False`; colisao com tipo oposto em janela de 60 ou 300 segundos, ressalvado marco proprio e autoridade humana (`ponto/registro_batida.py:21-49`, `ponto/registro_batida.py:106-156`). Dedup, recusa e retry ficam nas guardas: nao sao transicoes adicionais (`ponto/registro_batida.py:114-122`, `ponto/registro_batida.py:137`, `ponto/registro_batida.py:151`; `static/pwa.js:177-179`).

| # | estado de partida -> estado de chegada | juiz (arquivo:linha) | operacao (funcao) | escritor (arquivo:linha) | o que persiste (campos/modelos/trilha) | leitores (arquivo:linha) | prova (teste ou selo que morde, arquivo::nome, ou "nenhum") | [nao verificado]
|---|---|---|---|---|---|---|---|
| 1 | ∅ local → item em `batidas_pendentes` | Requisitos da tela e ramo rede/offline (`templates/ponto/bater_ponto_v2.html:285-312`, `templates/ponto/bater_ponto_v2.html:333-346`; `templates/ponto/triagem/apto.html:282`) | `registrarBatidaOffline`; **3 nascentes atuais** nos sitios anteriores | IndexedDB `.add` (`static/pwa.js:112-121`) | Tipo, instante corrigido, instante bruto, gap, GPS, precisao, captura e foto (`static/pwa.js:113-118`) | Sync `getAll`; indicador `count` (`static/pwa.js:130-133`, `static/pwa.js:188-191`) | nenhum |
| 2 | Item pendente → ∅ local, apos sucesso HTTP | Online, perfil colaborador, `resp.ok` e JSON legivel (`static/pwa.js:127-128`, `static/pwa.js:158-159`) | `sincronizarBatidas`; nascentes: mensagem SW, evento online, inicializacao online (`static/pwa.js:16`, `static/pwa.js:320`, `static/pwa.js:328`) | IndexedDB `.delete(b.id)` (`static/pwa.js:160-165`) | Remove item; nao grava estado local “sincronizada” (`static/pwa.js:160-165`) | Indicador posterior (`static/pwa.js:182`, `static/pwa.js:188-191`) | nenhum |
| 3 | Item pendente → ∅ local, apos HTTP 400 | Online, perfil colaborador e `resp.status === 400` (`static/pwa.js:127-128`, `static/pwa.js:166`) | `sincronizarBatidas`; mesmos **3 gatilhos** (`static/pwa.js:16`, `static/pwa.js:320`, `static/pwa.js:328`) | IndexedDB `.delete(b.id)` (`static/pwa.js:170-175`) | Remove item; motivo apenas em `console.warn` nesse ramo (`static/pwa.js:168-175`) | Indicador posterior (`static/pwa.js:182`, `static/pwa.js:188-191`) | nenhum |
| 4 | ∅ servidor → Batida `origem=app`, aprovada conforme nascente | Situacao/tipo nas APIs; dedup de 5 minutos nas duas APIs e web; aprovacao web por horario/sinal de acesso; juiz comum da porta (`api/views.py:403-427`, `api/views.py:527-546`; `api/views_core.py:389-413`, `api/views_core.py:513`; `ponto/services/triagem_batida.py:662-712`; `ponto/registro_batida.py:106-156`) | `registrar_batida`; **3 sitios**: `api/views.py:654`, `api/views_core.py:645`, `ponto/services/triagem_batida.py:702` | `Batida.objects.create` (`ponto/registro_batida.py:152`, `ponto/registro_batida.py:157`) | Batida, canal, sequencia, GPS; APIs guardam instante bruto; web pode nascer `aprovada=False`; evento de fechamento (`api/views.py:654-666`; `ponto/services/triagem_batida.py:702-713`; `ponto/registro_batida.py:160`) | Universo/cartorio (`ponto/turnos.py:2876`; `ponto/services/cartorio.py:1172`) | `api/tests/test_l030_batida_pela_porta.py::test_MORDE_sem_coordenadas_ativo_e_afastado_gravam_nos_dois_kernels` (:59); `ponto/tests/test_batida_offline.py::test_idempotencia_mesmo_timestamp` (:78) |
| 5 | ∅ → Batida `origem=manual` | Guardas de justificativa/saida, lote ou simulacao do veredito, mais juiz comum (`ponto/services/justificativa.py:152-199`, `ponto/services/justificativa.py:346-374`; `ponto/views.py:1811-1832`; `ponto/services/veredito_celula.py:293-329`; `ponto/registro_batida.py:106-156`) | `registrar_batida`; **4 sitios**: justificativa `ponto/services/justificativa.py:188`, retroativa `ponto/services/justificativa.py:361`, lote `ponto/views.py:1818`, Resolver dia `ponto/services/veredito_celula.py:323` | `Batida.objects.create` (`ponto/registro_batida.py:152`, `ponto/registro_batida.py:157`) | Batida aprovada, autor/campos conforme chamador; justificativa ou intervalo conforme ramo; evento de fechamento (`ponto/services/justificativa.py:188-194`, `ponto/services/justificativa.py:361-368`; `ponto/services/veredito_celula.py:323-324`; `ponto/registro_batida.py:160`) | Universo/cartorio (`ponto/turnos.py:2876`; `ponto/services/cartorio.py:1172`) | `ponto/tests/test_registrar_batida.py::test_oposto_60s_humano_cria_e_flaga` (:45), prova da porta comum |
| 6 | ∅ → Batida `origem=excecao_autorizada` | Chamado ainda nao encerrado; juiz comum com autoridade humana (`chamados/services/acoes_chamado.py:25-26`, `chamados/services/acoes_chamado.py:44-50`; `ponto/registro_batida.py:106-156`) | `acao_aprovar_excecao` → `registrar_batida`; nascente humana `chamados/views.py:427` | `Batida.objects.create` (`ponto/registro_batida.py:152`, `ponto/registro_batida.py:157`) | Tipo/instante, sequencia, justificativa, aprovacao/autor; foto pode vir depois no mesmo fluxo (`chamados/services/acoes_chamado.py:44-67`) | Universo/cartorio (`ponto/turnos.py:2876`; `ponto/services/cartorio.py:1172`) | `ponto/tests/test_registrar_batida.py::test_oposto_60s_humano_cria_e_flaga` (:45), prova da porta comum |
| 7 | ∅ → Batida `origem=reg_externa` | Lote confirmado, ausencia de disputa aberta, chave ainda inexistente e juiz comum (`chamados/services/regularizacao_ext.py:52-83`; `ponto/registro_batida.py:106-156`) | `_cria_batida` → `registrar_batida` (`chamados/services/regularizacao_ext.py:71-83`); cadeia de nascentes R abaixo | `Batida.objects.create` (`ponto/registro_batida.py:152`, `ponto/registro_batida.py:157`) | Batida aprovada, aprovador/data, justificativa; vinculos do item; auditoria `criar` no chamador (`chamados/services/regularizacao_ext.py:79-103`, `chamados/services/regularizacao_ext.py:173-189`) | Universo/cartorio (`ponto/turnos.py:2876`; `ponto/services/cartorio.py:1172`) | `chamados/tests/test_chokepoint_reg_externa.py::test_reg_externa_nao_gera_par_oposto_em_60s` (:23) |
| 8 | ∅ → Batida `origem=disputa_s84_retro` | Texto humano obrigatorio, reuso por janela, paridade/marco e juiz comum com `fato_de_chao=False` (`chamados/services/materializacao.py:835`, `chamados/services/materializacao.py:865`, `chamados/services/materializacao.py:881-930`, `chamados/services/materializacao.py:933-939`) | `_materializar_resposta_hora` → `registrar_batida` (`chamados/services/materializacao.py:933`); cadeia M abaixo, inclusive commands | `Batida.objects.create` (`ponto/registro_batida.py:152`, `ponto/registro_batida.py:157`) | Batida aprovada, autor/data; depois vinculo `pergunta_origem` pela transicao #20 (`chamados/services/materializacao.py:933-955`) | Universo; prova do plantio (`ponto/turnos.py:2876`; `chamados/juizes.py:250-279`) | `ponto/tests/test_porta_batida.py::test_MORDE_registrar_2x_a_mesma_batida_e_uma_so` (:49), prova da porta comum |
| 9 | ∅ → Batida `origem=disputa_ancora`, `tipo=E` | Parse da resposta data/hora; juiz comum com `fato_de_chao=False` (`chamados/services/materializacao.py:1001-1025`; `ponto/registro_batida.py:106-156`) | `_materializar_resposta_data_hora` → `registrar_batida` (`chamados/services/materializacao.py:1013`); cadeia M abaixo, inclusive commands | `Batida.objects.create` (`ponto/registro_batida.py:152`, `ponto/registro_batida.py:157`) | Entrada, sequencia 1, justificativa, aprovacao/autor/data (`chamados/services/materializacao.py:1013-1025`) | Universo/cartorio (`ponto/turnos.py:2876`; `ponto/services/cartorio.py:1172`) | `ponto/tests/test_porta_batida.py::test_MORDE_registrar_2x_a_mesma_batida_e_uma_so` (:49), prova da porta comum |
| 10 | `origem=offline_pendente`, `aprovada=False` → `origem=app`, `aprovada=True` | Permissao, POST, selecao pela origem/flag e `acao=aprovar` (`ponto/views.py:439`, `ponto/views.py:451-475`) | `batidas_offline_pendentes` → `anotar`; **1 nascente** (`ponto/views.py:475`) | `setattr` + `save(update_fields)` (`ponto/registro_batida.py:252-254`) | Origem, aprovacao, aprovador/data; trilha `aprovar` (`ponto/views.py:475-478`; `ponto/registro_batida.py:263`) | Fila DP e universo, cujo filtro nao muda pela aprovacao (`ponto/views.py:444-449`; `ponto/turnos.py:2876`) | nenhum |
| 11 | `offline_pendente`, nao aprovada → ∅ servidor | Mesma selecao, `acao=rejeitar`; porta exige PK (`ponto/views.py:459-483`; `ponto/registro_batida.py:288-290`) | `batidas_offline_pendentes` → `apagar`; **1 nascente** (`ponto/views.py:482`) | `batida.delete()` (`ponto/registro_batida.py:291`) | Registro eliminado; auditoria `deletar` com PK e snapshot (`ponto/views.py:467-483`; `ponto/registro_batida.py:288-292`) | Recompute/rejulgamento por `post_delete` (`ponto/signals.py:48-50`, `ponto/signals.py:138-140`) | `ponto/tests/test_offline_pendentes_trilha.py::test_rejeitar_deixa_trilha` (:31) |
| 12 | `tipo=E` → `tipo=S`, via flip | Nao retratada, autor, competencia aberta, tipo inversivel; automatico/veredito acrescentam simulacao (`ponto/services/flip_batida.py:15-28`; `ponto/services/flip_auto.py:285-312`; `ponto/services/veredito_celula.py:293-317`) | `flip_tipo`; **3 nascentes**: `ponto/views.py:3306`, `ponto/services/veredito_celula.py:317`, `ponto/management/commands/flip_automatico.py:68`; cron `config/crons.py:707` | `anotar({'tipo': novo})` → save (`ponto/services/flip_batida.py:30`; `ponto/registro_batida.py:254`) | Tipo; auditoria com antes/depois e causa; recompute (`ponto/services/flip_batida.py:30-39`) | Pareamento/cartorio; marca de flip na trilha (`ponto/nucleo.py:47`; `ponto/services/cartorio.py:1172`; `ponto/services/flip_batida.py:66-80`) | `ponto/tests/test_flip_batida.py::test_post_flip_inverte_com_trilha` (:20) |
| 13 | `tipo=S` → `tipo=E`, via flip | Mesmos juizes; inversao explicitamente simetrica (`ponto/services/flip_batida.py:15-28`; `ponto/services/flip_auto.py:285-312`) | `flip_tipo`; mesmos **3 sitios** (`ponto/views.py:3306`; `ponto/services/veredito_celula.py:317`; `ponto/management/commands/flip_automatico.py:68`), cron `config/crons.py:707` | `anotar` → save (`ponto/services/flip_batida.py:30`; `ponto/registro_batida.py:254`) | Tipo, trilha e recompute (`ponto/services/flip_batida.py:30-39`) | Pareamento/cartorio e trilha de flip (`ponto/nucleo.py:47`; `ponto/services/cartorio.py:1172`; `ponto/services/flip_batida.py:66-80`) | nenhum especifico desta direcao |
| 14 | Tipo/instante anteriores → tipo/instante informados na regularizacao | Confirmada, sem disputa aberta, item nao aplicado, alvo existente e hora informada; trata colisao de unicidade (`chamados/services/regularizacao_ext.py:52-66`, `chamados/services/regularizacao_ext.py:125-158`) | `materializar_regularizacao_externa`, ramo `corrigir`; cadeia R abaixo | **Fora da porta:** atribuicoes + `alvo.save` (`chamados/services/regularizacao_ext.py:154-156`) | `tipo`, `timestamp`; item `aplicada_em`; auditoria `corrigir_batida` (`chamados/services/regularizacao_ext.py:154-164`) | Signals de save; cartorio (`ponto/signals.py:34-36`, `ponto/signals.py:131-135`) | `chamados/tests/test_reg_externa_corrigir_remover.py::test_corrigir_tipo_E_para_S` (:32); `::test_MORDE_competencia_trancada_NAO_segura_o_corrigir` (:55) |
| 15 | Batida alvo da regularizacao → ∅ servidor | Confirmada, sem disputa aberta, item nao aplicado e alvo existente (`chamados/services/regularizacao_ext.py:52-66`, `chamados/services/regularizacao_ext.py:125-132`) | `materializar_regularizacao_externa`, ramo `remover`; cadeia R abaixo | **Fora da porta:** `alvo.delete()` (`chamados/services/regularizacao_ext.py:137`) | Auditoria `remover_batida`; alvo eliminado; item sem alvo e com `aplicada_em` (`chamados/services/regularizacao_ext.py:133-140`) | Signals de delete (`ponto/signals.py:48-50`, `ponto/signals.py:138-140`) | `chamados/tests/test_reg_externa_corrigir_remover.py::test_remover` (:46) |
| 16 | `retratada_em=NULL` → instante preenchido, `aprovada=False`; sai da apuracao | Idempotencia na porta; humano, geofence, par relampago e passivo possuem juizes proprios, discriminados abaixo (`ponto/registro_batida.py:271-276`; `ponto/services/retratar_humano.py:163-175`; `chamados/services/acoes_chamado.py:260-273`; `ponto/management/commands/detectar_par_relampago.py:185-248`; `ponto/services/passivo_s84.py:89-116`) | `retratar_batida`; **4 sitios diretos persistentes**, catalogados abaixo | `anotar` → save (`ponto/registro_batida.py:274`; `ponto/registro_batida.py:252-254`) | Preserva Batida; grava aprovacao/retratacao, trilha conforme chamador e evento de fechamento (`ponto/registro_batida.py:274-281`) | Apuracao exclui; calendario continua exibindo (`ponto/turnos.py:2876`; `colaboradores/services/calendario.py:239-255`) | `ponto/tests/test_porta_retratar_batida.py::test_MORDE_com_motivo_sai_da_apuracao_e_continua_no_banco` (:94); `ponto/tests/test_retratacao_relavra_o_dia.py::test_retratar_chama_a_relavratura_por_evento` (:33) |
| 17 | Retratada → `retratada_em=NULL`, `aprovada=True`; retorna a apuracao | Permissao/ator, marca existente, ultima trilha humana e prazo de 7 dias (`ponto/services/retratar_humano.py:131-147`, `ponto/services/retratar_humano.py:194-205`, `ponto/services/retratar_humano.py:48-53`) | `desfazer`; **1 nascente** `ponto/views.py:3332` | `anotar` → save (`ponto/services/retratar_humano.py:207`; `ponto/registro_batida.py:254`) | Limpa marca, aprova; trilha `desfazer_retratacao_batida` (`ponto/services/retratar_humano.py:31`, `ponto/services/retratar_humano.py:207-209`) | Universo, calendario e signals (`ponto/turnos.py:2876`; `colaboradores/services/calendario.py:249-255`; `ponto/signals.py:131-135`) | `ponto/tests/test_porta_retratar_batida.py::test_MORDE_desfazer_vale_7_dias_e_depois_nao` (:132); `::test_MORDE_nao_desfaz_o_que_esta_porta_nao_retratou` (:157) |
| 18 | Foto ausente/anterior → foto anexada | Existencia do arquivo; upload verifica dono; rota web exige foto ainda ausente; `anotar` compara valores (`api/views.py:769`, `api/views.py:838-842`; `ponto/views.py:2939-2943`; `ponto/registro_batida.py:244-249`) | `anexar_foto`; **7 nascentes**: `api/views.py:772`, `api/views.py:847`, `api/views_core.py:766`, `api/views_core.py:831`, `ponto/services/triagem_batida.py:752`, `ponto/views.py:2943`, `chamados/services/acoes_chamado.py:67` | `anotar` para Batida; create/update_or_create para FotoBatida (`ponto/registro_batida.py:305-311`) | `Batida.foto`, trilha; `FotoBatida` nos modos `criar`/`atualizar` (`ponto/registro_batida.py:296-311`) | Juiz de selfie recebe existencia da foto (`ponto/registro_batida.py:80-91`); upload documenta leitor de `Batida.foto` (`api/views.py:845`) | `ponto/tests/test_chokepoint_batida_gate.py::test_MORDE_nenhuma_escrita_de_batida_fora_da_porta` (:42), estrutural |
| 19 | Justificativa anterior → justificativa acrescida de conflito | Conflito devolvido pela criacao; idempotencia de `anotar` (`chamados/services/acoes_chamado.py:52-55`; `ponto/services/justificativa.py:200-203`, `ponto/services/justificativa.py:375-378`; `ponto/views.py:1836-1839`; `ponto/registro_batida.py:248-249`) | `anotar`; **4 nascentes de conflito** nos sitios anteriores | `setattr` + save + trilha (`ponto/registro_batida.py:252-263`) | `justificativa`; auditoria `anotar_conflito_batida` (`ponto/services/justificativa.py:202-203`; `ponto/registro_batida.py:263`) | Leitura anterior para concatenacao no proprio chamador (`ponto/services/justificativa.py:202`; `ponto/views.py:1838`) | `ponto/tests/test_porta_batida.py::test_MORDE_anotar_2x_mesmo_estado_1_trilha` (:56) |
| 20 | `pergunta_origem` ausente/diferente → pergunta do plantio | Batida resultante existente e vinculo diferente; idempotencia de `anotar` (`chamados/services/materializacao.py:952-956`; `ponto/registro_batida.py:244-249`) | `anotar`; **1 sitio**, inclusive quando o nascimento reutilizou Batida (`chamados/services/materializacao.py:865-877`, `chamados/services/materializacao.py:954`) | `setattr` + save + trilha (`ponto/registro_batida.py:252-263`) | FK `pergunta_origem`; auditoria `vincular_pergunta_origem` (`chamados/services/materializacao.py:954-956`) | Relacao `batidas_plantadas` e prova do plantio (`ponto/models.py:40-44`; `chamados/juizes.py:250-279`) | `chamados/tests/test_selo_batida_com_origem.py::test_MORDE_o_plantio_deixa_o_vinculo` (:134), estrutural |

### Nascentes indiretas e comandos da matriz

- **#4, app:** a web chega por `bater_ponto` (`ponto/views.py:349`); as APIs possuem roteamentos separados UI/core (`config/urls.py:37`; `api/urls.py:67`; `config/urls_core.py:6`; `api/urls_core.py:14`). A fila PWA envia para `/api/ponto/bater/` (`static/pwa.js:153`).
- **#5, manual:** aprovacao individual chama o servico em `ponto/views.py:1919`; retroativa em `ponto/views.py:3186`; lote cria diretamente em `ponto/views.py:1818`; Resolver dia chama o despachante em `ponto/views.py:3380`. A chamada de `declarar_dia` fixa `faltou`, portanto nao percorre o ramo criador `trabalhou` (`ponto/services/declarar_dia.py:215`; `ponto/services/veredito_celula.py:228`).
- **Cadeia M, #8–9 e #20:** materializacao em lote recebe chamadas de `chamados/services/fio.py:670`, `chamados/services/acoes_disputa.py:139`, `chamados/services/validacao.py:492` e **command** `chamad

[truncado em 20000 de 32143 caracteres]

## 14/11 21:50 · sessao f134f15d7b57 · MAPAS-DOS-RIOS · leitura · origem=Code

# MAPA DOS RIOS — PERGUNTA E FIO

Referencias abaixo relativas a `app/`. Os rotulos de Pergunta sao derivados dos campos; `viva` e outro resultado de leitura, e pode ser falsa enquanto o rotulo continua `pendente` ou `respondida`. As setas abaixo respeitam essa diferenca. (`chamados/models.py:2672`, `chamados/models.py:3090`, `chamados/juizes.py:1399`)

## 1. RESUMO

1. Pergunta tem **4 rotulos derivados**, **17 vias de resolucao** e **8 atos de decisao**; nao tem coluna de status. (`chamados/models.py:2672`, `chamados/models.py:2581`, `chamados/models.py:3913`, `chamados/models.py:3090`)
2. Ha **2 portas de nascimento**: datada, com dedup do fato, e conversa, sem esse dedup; recebem respectivamente **4 e 3 chamadas diretas** de producao. (`chamados/models.py:2832`, `chamados/models.py:2850`; nascentes discriminadas abaixo)
3. A resposta tem **3 operacoes escritoras**: resposta normal, automatica e recuperada pela trilha. Reenviar a mesma resposta normal move o instante novamente. (`chamados/services/disputa_emissao.py:1425`, `chamados/services/disputa_emissao.py:1638`, `chamados/services/disputa_emissao.py:1661`, `chamados/tests/test_porta_resposta_pergunta.py:196`)
4. Validacao, devolucao ao admin e recusa ao colaborador sao atos diferentes, com linhas diferentes na sequencia. (`chamados/services/validacao.py:162`, `chamados/services/validacao.py:228`, `chamados/services/validacao.py:620`)
5. O carimbo comum tem **8 chamadas diretas** e consulta C6; identidade tem **5 chamadas**, correspondentes a **5 vias**, julgadas pelo chamador. (`chamados/models.py:3195`, `chamados/tests/test_o261e_f2_porta_carimbo.py:385`, `chamados/tests/test_o261e_f2_porta_carimbo.py:399`; sitios abaixo)
6. A via tem **4 funcoes escritoras declaradas**; a ancora tem **2 escritores declarados**, incluindo o reuso no nascimento. (`chamados/tests/test_o261e_f2_porta_carimbo.py:51`, `chamados/services/fio.py:808`)
7. Nao existe rotulo `expirada`: vence a janela da lavra. Silencio decidido deixa ato proprio; tranca tambem cala por leitura, sem carimbo novo. (`chamados/models.py:2672`, `chamados/veredito_lavrado.py:107`, `chamados/models.py:1980`, `chamados/juizes.py:1403`)
8. Fio/tarefa tem **0 escritores proprios de estado persistido**; coexistem C17, com **3 leitores diretos**, e C19, com **2 entradas externas**. (`chamados/services/tarefa_do_dia.py:8`, `chamados/juizes.py:1622`; leitores discriminados abaixo)

## 2. TABELA DE TRANSICOES

“Carimbo” abaixo significa `materializada_em`, `materializada_por` e `via_resolucao`; a atribuicao conjunta mora em `_escrever_carimbo`, e a volta em lote tem escritor proprio. (`chamados/models.py:3153`, `chamados/models.py:3308`)

As provas sao testes/selos **lidos**; resultado de execucao nesta copia: **[nao verificado]**. [nao verificado]

| # | estado de partida -> estado de chegada | juiz (arquivo:linha) | operacao (funcao) | escritor (arquivo:linha) | o que persiste (campos/modelos/trilha) | leitores (arquivo:linha) | prova (teste ou selo que morde, arquivo::nome, ou "nenhum") | [nao verificado]
|---|---|---|---|---|---|---|---|
| 1 | inexistente -> pendente, pergunta datada | Dia do turno, universo/canonica do fato, destino e contrato de saida (`chamados/models.py:2895`, `chamados/models.py:2931`, `chamados/models.py:2944`, `chamados/models.py:2961`, `chamados/models.py:2877`) | `abrir -> _sob_contrato -> _nascer`; nascentes N1 | `chamados/models.py:3070`, `chamados/models.py:3073` | Identidade, dia, celula, destino, texto/tipo/opcoes; assinaturas recebidas viram decisoes; log `pergunta_emitida`. (`chamados/models.py:3070`, `chamados/models.py:3075`, `chamados/models.py:3081`) | `chamados/models.py:3090`; `chamados/services/tarefa_do_dia.py:130` | `chamados/tests/test_porta_pergunta_identidade.py:167::test_MORDE_abrir_2x_o_mesmo_fato_e_uma_pergunta_e_uma_trilha` |
| 2 | inexistente -> pendente, conversa | Dia civil; destino valido e contrato; porta nao faz dedup do fato. (`chamados/models.py:2850`, `chamados/models.py:2857`, `chamados/models.py:2895`) | `abrir_conversa -> _sob_contrato -> _nascer`; nascentes N2 | `chamados/models.py:3070`, `chamados/models.py:3073` | Mesma insercao, com `conversa=True`. (`chamados/models.py:2857`, `chamados/models.py:3070`) | `chamados/models.py:3090`; `chamados/services/tarefa_do_dia.py:130` | `chamados/tests/test_e1_entradas.py:178::test_MORDE_conversa_e_fato_de_nascimento` |
| 3 | pendente -> respondida; respondida -> respondida; validada/resolvida -> respondida quando reativada | Nas bordas, C6; no core, texto/tipo/opcoes; queda do carimbo depende da via sem lastro. (`chamados/views.py:1256`, `api/views.py:2604`, `chamados/services/disputa_emissao.py:1432`, `chamados/models.py:3369`) | `responder_pergunta_disputa_core`; nascentes R1 | `chamados/services/disputa_emissao.py:1467`, `chamados/services/disputa_emissao.py:1480`, `chamados/services/disputa_emissao.py:1508` | Texto/instante; limpa validacao anterior; eventualmente descarimba; Decisao `resposta`, evento da validacao desfeita e nota. O rotulo final continua sujeito a C6/carimbos preservados. (`chamados/services/disputa_emissao.py:1485`, `chamados/services/disputa_emissao.py:1511`, `chamados/services/disputa_emissao.py:1520`, `chamados/services/disputa_emissao.py:1555`, `chamados/models.py:3097`) | `chamados/models.py:3090`; `chamados/services/tarefa_do_dia.py:133`; `api/views.py:2543` | `chamados/tests/test_porta_resposta_pergunta.py:225::test_MORDE_resposta_NOVA_invalida_a_validacao_anterior`; `chamados/tests/test_porta_resposta_pergunta.py:196::test_CARACTERIZACAO_reenviar_a_MESMA_resposta_move_respondida_em` |
| 4 | pendente -> respondida automaticamente | Ja tem texto: recusa. (`chamados/services/disputa_emissao.py:1647`) | `responder_automaticamente`; chamada `api/foto_recebida.py:48` | `chamados/services/disputa_emissao.py:1652`, `chamados/services/disputa_emissao.py:1655` | Texto/instante e Decisao `resposta`, com motivo automatico. (`chamados/services/disputa_emissao.py:1656`) | `chamados/models.py:3090`; `chamados/services/tarefa_do_dia.py:133` | `chamados/tests/test_porta_resposta_pergunta.py:110::test_cada_escrita_mora_num_ato_declarado` |
| 5 | pendente com texto sem assinatura -> respondida | Exige texto e `respondida_em` vazio. (`chamados/services/disputa_emissao.py:1670`) | `carimbar_resposta_pela_trilha`; command `chamados/management/commands/curar_perguntas_em_limbo.py:67` | `chamados/services/disputa_emissao.py:1674`, `chamados/services/disputa_emissao.py:1676` | Instante fornecido pela trilha; Decisao `resposta` e evento. (`chamados/services/disputa_emissao.py:1677`, `chamados/services/disputa_emissao.py:1679`) | `chamados/models.py:3090`; `chamados/services/tarefa_do_dia.py:133` | `chamados/tests/test_porta_resposta_pergunta.py:241::test_MORDE_carimbar_pela_trilha_2x_nao_mexe_no_instante` |
| 6 | respondida -> validada; depois pode seguir para resolvida ou devolucao | Existe, respondeu, ainda nao validada; plausibilidade; folga tem ramo proprio. (`chamados/services/validacao.py:408`, `chamados/services/validacao.py:416`, `chamados/services/validacao.py:419`, `chamados/services/validacao.py:427`, `chamados/services/validacao.py:435`) | `validar_pergunta -> _carimbar_validacao`; nascentes V1 | `chamados/services/validacao.py:157`, `chamados/services/validacao.py:160` | `validada_em/por` e Decisao `validacao`; chama materializacao. (`chamados/services/validacao.py:162`, `chamados/services/validacao.py:484`, `chamados/services/validacao.py:492`) | `chamados/models.py:3097`; `chamados/services/tarefa_do_dia.py:141` | `chamados/tests/test_e1_entradas.py:94::test_MORDE_validacao_e_linha_com_o_relogio_do_carimbo` |
| 7 | validada -> resolvida; pendente aprovada pelo juiz -> resolvida | Unidade de materializacao, juiz do lastro e C6 depois do carimbo. (`chamados/services/materializacao.py:1164`, `chamados/services/materializacao.py:1433`, `chamados/models.py:3195`) | `_materializar_uma`; `materializar_pendentes_aprovadas`; nascentes M1 | `chamados/services/materializacao.py:1283`, `chamados/services/materializacao.py:1437` -> `chamados/models.py:3208` | Carimbo; unidade pode completar assinatura e lavra. Efeitos externos sao feitos pelo materializador. (`chamados/models.py:3186`, `chamados/models.py:3205`, `chamados/services/materializacao.py:1283`) | `chamados/models.py:3097`; `chamados/services/tarefa_do_dia.py:141` | `chamados/tests/test_porta_desfecho_pergunta.py:256::test_MORDE_pendente_que_o_juiz_reprova_fica_pendente`; `chamados/tests/test_o261e_f2_porta_carimbo.py:687::test_MORDE_pendente_com_resposta_por_validar_nao_sai_pelo_fecho_da_disputa` |
| 8 | pergunta do escape/irma -> resolvida por absorcao, se C6 aceitar | Escape define ausencia; `declarou_hora` escolhe via; C6 aceita ou recusa cada irma. (`chamados/services/materializacao.py:603`, `chamados/services/materializacao.py:555`, `chamados/models.py:3195`) | `_materializar_ausencia_de_resposta`; chamada pelo fluxo de resposta/ausencia (`chamados/services/disputa_emissao.py:1525`, `chamados/services/materializacao.py:502`) | `chamados/services/materializacao.py:567` -> `chamados/models.py:3208` | Carimbo e eventual assinatura; Decisao `absorcao` liga pergunta, escape e ausencia. Recusada deixa evento `irma_viva_na_ausencia`. (`chamados/services/materializacao.py:569`, `chamados/services/materializacao.py:575`) | `chamados/services/tarefa_do_dia.py:152`; `chamados/services/materializacao.py:609` | `chamados/tests/test_e2_tarefa_do_dia.py:238::test_absorcao_segue_a_decisao_da_ausencia` |
| 9 | validada -> respondida, devolvida ao admin | Hora declarada, sem materializacao e com motivo escrito. (`chamados/services/validacao.py:174`, `chamados/services/validacao.py:213`, `chamados/services/validacao.py:215`) | `devolver_validacao_recusada`; chamada `chamados/services/validacao.py:487` | `chamados/services/validacao.py:222`, `chamados/services/validacao.py:225` | Limpa assinatura, preserva resposta; Decisao `devolucao`; evento quando a foto mudou. (`chamados/services/validacao.py:228`, `chamados/services/validacao.py:244`) | `chamados/services/tarefa_do_dia.py:139`; `chamados/models.py:3090` | `chamados/tests/test_o261_recusa_fala.py:148::test_idempotente_validar_2x_a_recusada_mesmo_estado_uma_trilha` |
| 10 | respondida -> pendente, recusa humana | Motivo obrigatorio; validada nao pode; precisa ter resposta. (`chamados/services/validacao.py:588`, `chamados/services/validacao.py:600`, `chamados/services/validacao.py:603`) | `recusar_pergunta -> reabrir_para_correcao(ato='recusa')`; casca `chamados/services/acoes_disputa.py:80` | Motivo: `chamados/services/validacao.py:617`; reset: `chamados/services/disputa_emissao.py:1632` | Motivo; limpa resposta/assinatura; Decisao `recusa` com foto, autor e motivo; nota/evento. (`chamados/services/validacao.py:620`, `chamados/services/validacao.py:625`, `chamados/services/disputa_emissao.py:1615`) | `chamados/services/tarefa_do_dia.py:137`; `chamados/models.py:3090` | `chamados/tests/test_porta_validacao_pergunta.py:224::test_MORDE_recusar_SEM_request_carimba_reabre_e_deixa_trilha` |
| 11 | respondida/validada/resolvida -> pendente, quando o universo permite | `perguntas_corrigiveis -> pergunta_sem_lastro_ou_viva`; absorcao rejeitada usa grupo proprio. (`chamados/juizes.py:1671`, `chamados/juizes.py:1492`, `chamados/services/materializacao.py:609`) | `reabrir_para_correcao`; nascentes D1 | `chamados/services/disputa_emissao.py:1593`, `chamados/services/disputa_emissao.py:1630`, `chamados/services/disputa_emissao.py:1632` | Limpa texto/resposta/validacao; carimbo somente com `desfazer_absorcao`; Decisao com foto anterior e evento de celula. Sem retirar carimbo restante, o rotulo depende dele. (`chamados/services/disputa_emissao.py:1615`, `chamados/services/disputa_emissao.py:1629`, `chamados/models.py:3097`) | `chamados/services/tarefa_do_dia.py:137`; `chamados/models.py:3090` | `chamados/tests/test_reabertura_fantasma.py:57::test_reabertura_limpa_o_texto_junto_com_o_carimbo` |
| 12 | pendente/respondida/validada -> resolvida por fato do destino ou fecho sem credor | C6, tranca, pergunta em voo e testemunha externa; fecho sem credor consulta C6 novamente. (`chamados/services/disputa_emissao.py:1960`, `chamados/services/disputa_emissao.py:1966`, `chamados/services/disputa_emissao.py:1970`, `chamados/services/disputa_emissao.py:1979`, `chamados/services/disputa_emissao.py:2048`) | `resolver_perguntas_por_chamado_resolvido`; nascentes F1 | `chamados/services/disputa_emissao.py:1986`, `chamados/services/disputa_emissao.py:2060` -> `chamados/models.py:3208` | Carimbo; completa assinatura apenas havendo texto; primeiro ramo relavra; segundo deixa `fecho_sem_credor`. (`chamados/models.py:3186`, `chamados/services/disputa_emissao.py:1987`, `chamados/services/disputa_emissao.py:2063`) | `chamados/models.py:3090`; `chamados/services/tarefa_do_dia.py:141` | `chamados/tests/test_porta_resposta_pergunta.py:288::test_MORDE_resolver_nao_enterra_resposta_do_colaborador` |
| 13 | pendente -> resolvida, sem turno aberto | Juizo de turno e C6 antes/depois do carimbo. (`chamados/reconciliador.py:341`, `chamados/reconciliador.py:399`, `chamados/models.py:3195`) | `lavrar_sem_turno_aberto` (`chamados/reconciliador.py:341`) | `chamados/reconciliador.py:406` -> `chamados/models.py:3208` | Carimbo `sem_turno_aberto`, se aceito. (`chamados/reconciliador.py:406`) | `chamados/models.py:3090`; `chamados/juizes.py:679` | `chamados/tests/test_o261e_f2_porta_carimbo.py:226::test_so_a_porta_escreve_o_carimbo` |
| 14 | pendente -> resolvida, celula absolveu | Selecao de absolvidas e C6 com o carimbo. (`chamados/reconciliador.py:550`, `chamados/reconciliador.py:621`, `chamados/models.py:3195`) | `retratar_perguntas_absolvidas_por_celula`; consumidor `chamados/reconciliador.py:1316` | `chamados/reconciliador.py:596` -> `chamados/models.py:3208` | Carimbo `celula_absolveu`. (`chamados/reconciliador.py:596`) | `chamados/models.py:3090`; `chamados/juizes.py:1448` | `chamados/tests/test_o261e_f2_porta_carimbo.py:226::test_so_a_porta_escreve_o_carimbo` |
| 15 | pendente -> resolvida, fantasma confirmado | `veredito_orfa`/lampada e C6. (`chamados/reconciliador.py:1136`, `chamados/models.py:3195`) | `encerrar_orfas_pela_lampada -> encerrar_fantasma`; consumidor `chamados/reconciliador.py:1376` | `chamados/reconciliador.py:1148` -> `chamados/models.py:3343` | Carimbo `fantasma` e evento do encerramento. (`chamados/reconciliador.py:1150`) | `chamados/models.py:3090`; `chamados/juizes.py:679` | `chamados/tests/test_o261e_f2_porta_carimbo.py:665::test_MORDE_encerrar_fantasma_nao_carimba_pergunta_viva` |
| 16 | pendente -> resolvida por silencio decidido; ou pergunta -> resolvida por decisao do admin sobre o dia | Silencio: motivo, respostas sem veredito e C6; decisao do dia pertence ao chamador. (`chamados/models.py:1942`, `chamados/models.py:1952`, `chamados/models.py:2003`, `chamados/models.py:3225`) | `fechar_por_admin -> carimbar_identidade`; nascentes I1 | `chamados/models.py:3120`, `chamados/models.py:3246` | Carimbo `admin_decidiu`; no silencio, Decisao `silencio` e evento com motivo. (`chamados/models.py:1980`, `chamados/models.py:1983`) | `chamados/services/tarefa_do_dia.py:150`; `chamados/models.py:3090` | `chamados/tests/test_porta_pergunta_identidade.py:194::test_MORDE_fechar_por_admin_2x_nao_mexe_no_carimbo` |
| 17 | validada -> resolvida por paridade recusada | Guarda do plantio; identidade aceita somente via permitida. (`chamados/services/materializacao.py:930`, `chamados/models.py:3234`) | `_materializar_resposta_hora -> carimbar_identidade` | `chamados/services/materializacao.py:930` -> `chamados/models.py:3246` | Carimbo `paridade_recusada`. (`chamados/services/materializacao.py:930`) | `chamados/juizes.py:1437`; `chamados/models.py:3097` | `chamados/tests/test_o261e_f2_porta_carimbo.py:505::test_MORDE_paridade_gravando_admin_decidiu` |
| 18 | validada -> resolvida por fato ocupado | Recusa/colisao do materializador e conjunto de identidade. (`chamados/services/materializacao.py:1089`, `chamados/models.py:3234`) | `_carimbar_recusa -> carimbar_identidade` | `chamados/services/materializacao.py:1105` -> `chamados/models.py:3246` | Carimbo `fato_ocupado` e motivo. (`chamados/services/materializacao.py:1105`) | `chamados/juizes.py:1437`; `chamados/models.py:3097` | `chamados/tests/test_o261e_f2_porta_carimbo.py:498::test_cada_juiz_de_identidade_grava_a_SUA_via` |
| 19 | pergunta excedente -> resolvida, fora do fato por duplicata | Canonica/plano de retratacao e guarda de identidade. (`chamados/services/fio.py:1226`, `chamados/services/fio.py:1341`, `chamados/models.py:3236`) | `retratar_para_canonica -> retratar_duplicata`; nascentes I2 | `chamados/services/fio.py:1360` -> `chamados/models.py:3246` | Via `duplicata`, referencia a canonica no motivo; conserva carimbo anterior quando existente. (`chamados/services/fio.py:1360`, `chamados/models.py:3238`) | `chamados/juizes.py:1412`; `chamados/models.py:3257` | `chamados/tests/test_leitores_ouvem_a_lei.py:28::test_duplicata_respondida_sai_da_fila` |
| 20 | pergunta do motivo removido -> resolvida, fora do fato | Remocao do motivo e guarda de identidade. (`chamados/models.py:2027`, `chamados/models.py:3236`) | `DisputaSupervisao.remover_motivo -> carimbar_identidade` | `chamados/models.py:2117` -> `chamados/models.py:3246` | Decisao `retratacao` e via `motivo_retratado`; preserva a pergunta. (`chamados/models.py:2112`, `chamados/models.py:2117`) | `chamados/juizes.py:1412`; `chamados/models.py:3257` | `chamados/tests/test_e1_entradas.py:142::test_MORDE_a_retratada_morre_e_tem_linha` |
| 21 | resolvida -> pendente/respondida/validada, conforme assinaturas preservadas | Via especifica nos atalhos; C6 no sem lastro; fora do fato recusa. (`chamados/models.py:3352`, `chamados/models.py:3369`, `chamados/models.py:3376`, `chamados/services/relavra_via.py:242`, `chamados/models.py:3262`) | `reabrir_fantasma`, `reabrir_por_competencia`, `reabrir_por_resposta`, `desfazer_carimbo_sem_lastro -> descarimbar`; nascentes D2 | `chamados/models.py:3264`, `chamados/models.py:3268`; resposta salva junto em `chamados/services/disputa_emissao.py:1508` | Limpa os tres campos do carimbo; sem lastro deixa evento. Nao limpa resposta/validacao por regra geral. (`chamados/models.py:3266`, `chamados/services/relavra_via.py:248`) | `chamados/models.py:3097`; `chamados/juizes.py:1399` | `chamados/tests/test_porta_veredito_pergunta.py:408::test_MORDE_carimbo_desfeito_devolve_a_pergunta_para_a_mesa` |
| 22 | resolvida muda -> pendente, ressuscitacao | Universo proprio; sem texto; marco nao aceso; C6 sobre copia descarimbada. (`chamados/models.py:1530`, `chamados/models.py:1550`, `chamados/models.py:1553`, `chamados/models.py:1556`) | `ressuscitar_perguntas -> descarimbar_em_lote`; nascentes D3 | `chamados/models.py:1613`, `chamados/models.py:3320` | Limpa carimbo/resposta/validacao; acorda celula; trilha identifica voltadas e puladas. (`chamados/models.py:1615`, `chamados/models.py:1636`) | `chamados/models.py:3090`; `chamados/services/tarefa_do_dia.py:116` | `chamados/tests/test_porta_pergunta_identidade.py:301::test_MORDE_ressuscitar_2x_e_o_mesmo_estado_e_UMA_trilha` |
| 23 | mesma pergunta: ancora anterior/ausente -> nova ancora; rotulo recalculado | `resolvedora_valida` e `_costura_nao_mata_o_fio`; duas entradas escritoras consultam ambos. (`chamados/services/fio.py:908`, `chamados/services/fio.py:910`, `chamados/models.py:3028`, `chamados/models.py:3036`) | `reapontar_resolvedora`; reuso de `_nascer`; nascentes A1 | Porta: `chamados/services/fio.py:926`, `chamados/services/fio.py:928`; reuso: `chamados/models.py:3038`, `chamados/models.py:3062` | Destino/celula, eventual dia, lavra no mesmo save; acorda celula anterior; reuso recusado deixa `costura_recusada`. (`chamados/services/fio.py:927`, `chamados/services/fio.py:934`, `chamados/models.py:3051`) | Ancora de listagem `chamados/services/fio.py:97`; C19 `cham

[truncado em 20000 de 43885 caracteres]

_5358 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
