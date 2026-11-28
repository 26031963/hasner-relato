# CODEX-ESTADO

> Ultima resposta processada: **27/11/2026 21:05** · publicado em **27/11/2026 21:07** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 27/11 21:05 · sessao 96fd5c7b7b0f · FIM-DE-SEMANA-JA · leitura · origem=Code

## 1. Dono L‑099 por linha

**11 CADASTRO, 7 ESTRUTURA, 0 BATIDA**, em 18 dias. As referências a `sombra.out` contam a linha `sonda_leitura` como linha 1. O `dono_motivo` veio `-` nas 18 linhas. O motivo abaixo vem de seguir os insumos até o retorno de `dono_da_divergencia`, chamado por `InsumosDoDono.dono`. `sombra.out:1–21`, `app/docs/LEIS.md:122`, `app/ponto/management/commands/e6_oraculo.py:182–195`.

- **Vínculo ausente.** `sem_vinculo_no_dia` e `sem_vinculo_em_dia_de_trabalho` vieram `true`. Esse ramo prevalece sobre as batidas. A geradora nomeada pela célula estava fora da vigência do dia, e a leitora do vínculo devolve `None` nessa condição. `app/ponto/management/commands/e6_oraculo.py:98–99`, `app/escala/alimentacao.py:269–284`, `app/ponto/supra_juiz.py:206–210`.
- **Retorno residual.** Os três indicadores de cadastro do primeiro ramo vieram `false`, com zero ou quatro horas no oráculo, sem batida fora de turno, fase deslocada ou presença do dia na lavra CxR. O último retorno dá `ESTRUTURA`. `sombra.out:14–21`, `app/ponto/management/commands/e6_oraculo.py:98–110`.

| Colab · dia de 2026 | Dono | Evidência do dia e ramo decisivo |
|---|---|---|
| col97 · 01/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:4`, `e6_oraculo.py:98–99`. |
| col97 · 03/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:5`, `e6_oraculo.py:98–99`. |
| col97 · 05/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:6`, `e6_oraculo.py:98–99`. |
| col97 · 07/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:7`, `e6_oraculo.py:98–99`. |
| col97 · 09/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:8`, `e6_oraculo.py:98–99`. |
| col97 · 11/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:9`, `e6_oraculo.py:98–99`. |
| col97 · 13/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:10`, `e6_oraculo.py:98–99`. |
| col97 · 15/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:11`, `e6_oraculo.py:98–99`. |
| col97 · 17/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:12`, `e6_oraculo.py:98–99`. |
| col97 · 19/11 | CADASTRO | Célula gerada, indefinida, sem vínculo no dia. `sombra.out:13`, `e6_oraculo.py:98–99`. |
| col296 · 10/11 | ESTRUTURA | `COBRANCA_SEM_FURO`, 660 previstos e 660 realizados. Quatro horas no oráculo e retorno residual. `sombra.out:14`, `e6_oraculo.py:110`. |
| col327 · 10/11 | ESTRUTURA | `PARCIAL_SEM_PERIODO`, declaração aprovada de 60 min sem início e fim. Quatro horas e retorno residual. `sombra.out:15`, `e6_oraculo.py:110`. |
| col344 · 07/11 | ESTRUTURA | Célula `concorde`, agendamento 103 cobre o dia e há batida civil apurável. Quatro horas e retorno residual. `sombra.out:16`, `e6_oraculo.py:110`. |
| col348 · 20/11 | CADASTRO | Célula editada, indefinida, sem vínculo no dia, com 712 min realizados. `sombra.out:17`, `e6_oraculo.py:98–99`. |
| col354 · 21/10 | ESTRUTURA | `PARCIAL_SEM_PERIODO`, declaração aprovada de 120 min sem início e fim. Quatro horas e retorno residual. `sombra.out:18`, `e6_oraculo.py:110`. |
| col407 · 29/10 | ESTRUTURA | `FURO_PARCIAL`, 720 previstos e 0 realizados. Oráculo vazio e retorno residual. `sombra.out:19`, `e6_oraculo.py:106–110`. |
| col655 · 21/10 | ESTRUTURA | `PARCIAL_SEM_PERIODO`, declaração aprovada de 180 min sem início e fim. Quatro horas e retorno residual. `sombra.out:20`, `e6_oraculo.py:110`. |
| col673 · 21/10 | ESTRUTURA | `FURO_PARCIAL`, 720 previstos e 3 realizados. Oráculo vazio e retorno residual. `sombra.out:21`, `e6_oraculo.py:106–110`. |

Nos 11 casos de vínculo, a célula traz `PREVISTO_COM_DIA_INDEFINIDO`, e `cadastro.falta_previsto` aponta um vínculo que responda pela data. O retrato lavrado inclui as datas dos 11. Para col97, a lista de portas do retrato veio vazia. Para col348, trouxe **Vínculo da escala** e duas entradas **Resolver dia no calendário**. Essas portas vêm da linha do colaborador na competência. `sombra.out:4–13`, `sombra.out:17`, `app/folha/retrato_folha.py:930–935`, `app/folha/retrato_folha.py:1024–1059`.

**Onde o S2 divergiu.** O S2 tratou o protesto `PARCIAL_SEM_PERIODO` como se já fosse a decisão L‑099. O supra-juiz identifica corretamente o campo faltante e manda cobrar o cadastro. O cartório também prevê encaminhar a parcial sem período à lista do admin. Para a **contagem pedida**, a L‑099 aponta para `dono_da_divergencia`. Esse juiz recebe vínculo, folga, batidas, fase e CxR, e os três parciais caíram no retorno `ESTRUTURA`. Os dois `FURO_PARCIAL` tinham dono do fato em aberto no S2 e agora têm `ESTRUTURA` medido. O S2 já descrevia col296 como estrutural na seção de cobrança, mas o deixou fora da soma C1 de 14 + 2 + 1. A S3 também identificou nominalmente o caso de férias e mediu seu dono. `S2_r.md:11–14`, `app/docs/LEIS.md:122`, `app/ponto/supra_juiz.py:395–402`, `app/ponto/services/cartorio.py:423–446`, `app/ponto/management/commands/e6_oraculo.py:53–54`, `app/ponto/management/commands/e6_oraculo.py:98–110`, `sombra.out:14–21`.

## 2. Lista para o Mauricio

**Vínculo da escala**, formulário **Vincular Escala**, é a tela indicada em `tela.previsto` para cada linha abaixo. URL name `colaboradores:painel_vinculo`, template `colaboradores/partials/painel_vinculo.html`. `app/folha/retrato_folha.py:1003–1006`, `app/colaboradores/urls.py:12–13`, `app/colaboradores/views.py:306`, `app/templates/colaboradores/partials/painel_vinculo.html:128–137`.

| Colab | Dia de 2026 | Motivo para o DP e cadastro faltante | Tela |
|---|---|---|---|
| col97 | 01/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:4`. | Vínculo da escala |
| col97 | 03/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:5`. | Vínculo da escala |
| col97 | 05/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:6`. | Vínculo da escala |
| col97 | 07/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:7`. | Vínculo da escala |
| col97 | 09/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:8`. | Vínculo da escala |
| col97 | 11/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:9`. | Vínculo da escala |
| col97 | 13/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:10`. | Vínculo da escala |
| col97 | 15/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:11`. | Vínculo da escala |
| col97 | 17/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:12`. | Vínculo da escala |
| col97 | 19/11 | Falta vínculo de escala que cubra o dia. A geradora 12×36 terminou em 30/10. `sombra.out:13`. | Vínculo da escala |
| col348 | 20/11 | Houve trabalho registrado e falta vínculo de escala que cubra o dia. A geradora intermitente terminou em 20/10. `sombra.out:17`. | Vínculo da escala |

## 3. ESTRUTURA, ato faltante, escritor e obra

Nas sete linhas, a S3 encontrou `colab_na_lista=true`, `dia_na_lista=false`, `dono_lavrado=null`, `mesa_admin=false` e `destino_DP=-`. A consulta à lista distingue a presença do colaborador da presença daquele dia. Em todas, o calendário da tarefa veio `false` e o retrato de 27/11 inclui a data. `sombra.out:14–16`, `sombra.out:18–21`, `app/chamados/services/vigia_fio.py:202–241`.

| Caso · célula | Linha C1 preenchida com a evidência disponível | Ato, escritor canônico e BACKLOG |
|---|---|---|
| **col296 · 10/11** · 137154 | Chamado 38343 `turno_aberto` ainda aberto, 38318 resolvido. Perguntas 56532 e 56545 ancoradas em 38343, C6 `true`, sem resposta ou ato. A 56612 tem C6 `false` e projeção `fantasma`. Tarefa sem porta e fila vazia. Retrato com **Cobrar no fio** e **Vínculo da escala**. `sombra.out:14`. | Baixa de 38343 e rejulgamento, **se o gate autorizar**. O cartório chama a reconciliação em `app/ponto/services/cartorio.py:_julgar_colab_corpo:1618–1629`. A baixa passa por `app/chamados/models.py:ChamadoColaborador.reconciliar:661–675`. **E4B-SCA-25-COBRANCA-INDEVIDA** é a obra da classe. Inclusão nominal no estoque dos 25 e motivo da permanência [nao verificado]. Faltam `vivos`/`_protestam` da lavra e resultado ou recusa da reconciliação de 38343. `app/docs/BACKLOG.md:2318`, `app/ponto/services/cartorio.py:1498–1507`, `app/chamados/models.py:683–712`. |
| **col327 · 10/11** · 122970 | Declaração 5515, 60 min, `hora_ini=null`, `hora_fim=null`. Chamado 38375 `ausencia_colab` resolvido. Sem pergunta, ato, porta ou link no retrato. `sombra.out:15`. | Falta publicar o dia para o admin e declarar o período da ausência. Escritores e obra candidata das três parciais abaixo. |
| **col344 · 07/11** · 136655 | Agendamento 103, de 06/10 a 07/11, concluído, aquisitivo 639 concedido. Batida civil 148780, `E`, às 18:33:49 locais. Chamado 37604 registrado. Pergunta 55062, âncora e fila 37605, C6 `true`. `porta_pergunta` publica `validar` com `nao_sei=true`, e a instância está `passivo_sem_linha`. Retrato com **Validar no fio**. `sombra.out:16`. | No fio, falta `DecisaoPergunta` de **resposta** para 55062 em `2026-11-07T21:33:56.900522+00:00`. Escritor do passivo abaixo. Para férias, **HJ‑090, item 7**, é a obra da lista com dono L‑099. Ela referencia `CASOS-103-114-PELA-PORTA`, cuja decisão depende das datas do DP. Falta a linha `Ausencia(tipo=ferias)` de 07/11, com status e agendamento de origem, para confirmar entrada no relatório da obra. A correção do agendamento ou da batida fica [nao verificado]. `app/docs/BACKLOG.md:2137`, `app/docs/BACKLOG.md:751`, `app/ferias/management/commands/listar_ferias_com_batida.py:1–12`, `app/ferias/management/commands/listar_ferias_com_batida.py:41–65`. |
| **col354 · 21/10** · 128888 | Declaração 5125, 120 min, `hora_ini=null`, `hora_fim=null`. Sem chamado, pergunta ou ato. Tarefa sem porta. Retrato com **Resolver dia no calendário**. `sombra.out:18`. | Falta publicar o dia para o admin e declarar o período da ausência. Escritores e obra candidata das três parciais abaixo. |
| **col407 · 29/10** · 133670 | Chamado e âncora 35801 fechados. Pergunta 51727, C6 `true`, sequência vazia, resposta só na projeção. Instância `passivo_sem_linha`, fio `nao_sei`, sem rota ou fila. Retrato com duas entradas **Resolver dia no calendário**. `sombra.out:19`. | Falta `DecisaoPergunta` de **resposta** para 51727 em `2026-11-01T10:02:34.925352+00:00`. **FIO-E0-V4-E4B** é a obra do passivo. Escritor abaixo. `app/docs/BACKLOG.md:2315`. |
| **col655 · 21/10** · 129497 | Declaração 5076, 180 min, `hora_ini=null`, `hora_fim=null`. Chamados 33158 e 33163 resolvidos, com 33158 no cartório. Sem pergunta ou ato. Tarefa sem porta. Retrato com **Cobrar no fio**. `sombra.out:20`. | Falta publicar o dia para o admin e declarar o período da ausência. Escritores e obra candidata das três parciais abaixo. |
| **col673 · 21/10** · 135708 | Âncora 33282 fechada. 33494 resolvido, 34029 fechado. A pergunta 46935 tem C6 `true`, sequência vazia e `passivo_sem_linha`. A 46936 tem C6 `false` e projeção materializada por `declaracao`. Fio `nao_sei`, sem rota, fila ou link no retrato. `sombra.out:21`. | Falta `DecisaoPergunta` de **resposta** para 46935 em `2026-10-22T10:08:24.022007+00:00`. **FIO-E0-V4-E4B** é a obra do passivo. O BACKLOG nomeia col673 e a pergunta **46936** em **DIA-DA-BATIDA-JUIZ-UNICO**, para a atribuição da batida ao turno. `app/docs/BACKLOG.md:2315`, `app/docs/BACKLOG.md:1864`. |

**Três parciais.** O ato de cadastro é salvar o início e o fim declarados pelo DP no painel `ponto:painel_ausencia`, template `ponto/partials/painel_ausencia.html`. O POST `ponto:editar_periodo_ausencia` chama `decidir`, que grava pela função `app/ponto/services/ausencia.py:_aplicar_periodo:1051–1054` e persiste em `app/ponto/services/ausencia.py:_decidir_no_ato:1101–1104`. Para o encaminhamento ausente, **COBPARC-B-CAUSA-NO-MOTOR** descreve a classe. O escritor atual das causas CxR é `app/ponto/motor_calculo_v2.py:_acusa_cadastro_x_realidade:1766–1783`, com chamadas nas pontas da janela de HE. A ausência das três datas na lista está medida. A causa exata da omissão e o escritor que deveria publicar essas três linhas ficam [nao verificado]. Faltam a assinatura por `(colab, dia)` na origem da lista e o resultado da sua publicação. **BO2910-ESTRUTURA (05)** cobre a aprovação pela tela sem exigir período. Para atribuir essa falha histórica a uma das três declarações, faltam canal e trilha da aprovação. `app/ponto/urls.py:68–72`, `app/ponto/views.py:2836–2849`, `app/ponto/services/cartorio.py:423–446`, `app/ponto/motor_calculo_v2.py:1822–1824`, `app/ponto/motor_calculo_v2.py:1860–1862`, `app/docs/BACKLOG.md:1105`, `app/docs/BACKLOG.md:1191`, `sombra.out:15`, `sombra.out:18`, `sombra.out:20`.

**Três respostas sem linha.** O escritor canônico é `app/chamados/models.py:DecisaoPergunta.registrar:4590–4609`. O backfill despacha a recomposição em `app/chamados/management/commands/backfill_fio_morto_e4b.py:_executar_dia:161–164`. A correspondência exige a mesma pergunta, o mesmo ato e o mesmo instante. **E4C-F8** trata da migração de leitores da projeção para a leitura do ato. A inclusão nominal de 51727 ou 46935 nas nove unidades dessa obra fica [nao verificado], pois falta identificar o leitor específico na lista de escopo da F8. `app/chamados/models.py:4416–4447`, `app/docs/BACKLOG.md:2211`, `app/docs/BACKLOG.md:2315`, `sombra.out:16`, `sombra.out:19`, `sombra.out:21`.

**Férias de col344.** A Folha já publica a retenção a partir do agendamento e do dia civil da batida. A S3 incluiu o caso pelo ramo sem porta **do tipo** `ferias`. A tarefa e o retrato puderam publicar **Validar no fio** pela pergunta. O ato de férias ainda depende de confirmar o gozo real, o dia de turno, a origem da batida e a decisão do DP. Se o DP confirmar datas diferentes, o escritor das datas é `app/ferias/services.py:_trocar_janela:694–729`. A tela encontrada é `ferias:secao_ferias_colab`, template `ferias/_secao_colab.html`. `s3.py:92–97`, `sombra.out:16`, `app/folha/export.py:490–504`, `app/folha/export.py:885–893`, `app/folha/retrato_folha.py:1003–1017`, `app/ferias/views.py:851–861`.

## 4. O que segue [nao verificado]

- **Identidade Git da cópia, execução independente da S3 e equivalência com produção.** A cópia consultada está sem metadados Git. O cabeçalho do anexo identifica a sonda, e o código traz as guardas de ambiente da sombra. O resultado disponível é a saída raspada fornecida. `sombra.out:1–3`, `s3.py:16–22`. [nao verificado]
- **Valores corretos e histórico das correções.** Faltam o vínculo e a vigência corretos para col97/col348, os horários declarados das três parciais, o retorno histórico do gate de 38343 e a decisão do DP sobre a sobreposição de férias. A saída mostra o estado consultado e a sobreposição civil. `sombra.out:4–21`, `app/chamados/models.py:683–712`, `app/folha/export.py:490–504`. [nao verificado]
- **Origem e elegibilidade das três respostas sem linha.** Faltam o escritor original, a trilha do ato, a presença no plano do backfill e o estado da competência no gate. A sonda mede a falta da linha, e o backfill ainda aplica seus critérios de alcance, competência e entrega. `sombra.out:16`, `sombra.out:19`, `sombra.out:21`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:307–343`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:383–405`. [nao verificado]
- **Alcance por um ator e efeito na Folha.** O anexo traz rótulo e URL name das portas do retrato. Faltam o destino completo de cada link, o perfil e a autorização do ator, o resultado da ação e uma nova classificação da Folha. Liberação do TXT e efeito em pagamento ficam [nao verificado]. `sombra.out:14–21`, `app/folha/retrato_folha.py:1024–1065`, `app/folha/export.py:699`, `app/folha/export.py:774`.

## 27/11 20:59 · sessao 5e148d3706d5 · FERIADO-UM-JUIZ · construtor · origem=Code

A c11 está na cópia. O RED de FT passou na cura e falhou nos três subcasos do mutante com `ValueError not raised`. O teste de `do_motor` também passou na cura e falhou na c10 e no mutante quando o dia estava sem entrada em `rubricas`. [RED de FT](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:395) · [RED de `do_motor`](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:418) [nao verificado]

### Diff por arquivo

| Arquivo | Diff | Alteração |
|---|---:|---|
| [dia_pago.py:316](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/services/dia_pago.py:316) | +4 −6 | Uma chamada a `_DONOS_DECLARADOS` por dia, com `do_motor` nos dois casos. Saiu o `or CAMPOS_DIA`. |
| [fechamento.py:682](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/services/fechamento.py:682) | +1 −0 | Comentário do contador registra recusas por colaborador e competência e remete ao R1. |
| [test_feriado_um_juiz.py:395](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:395) | +45 −0 | REDs de FT isolado e de `do_motor`. O R7 ganhou a docstring de [caracterização](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz.py:476). |
| [feriado_um_juiz_mutantes.py:167](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/feriado_um_juiz_mutantes.py:167) | +15 −4 | Dois mutantes novos e ajuste do alvo textual do [M1](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/feriado_um_juiz_mutantes.py:484). |

**`except` no diff dos quatro arquivos acima: 0.** O controle local rodou 52 testes, com uma falha esperada. O M1 produziu as duas falhas previstas. [Controle](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/feriado_um_juiz_mutantes.py:716) · [Alvos do M1](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/feriado_um_juiz_mutantes.py:122) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.feriado_um_juiz_mutantes controle
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.feriado_um_juiz_mutantes c11_foto_sem_dia_so_com_ft
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.feriado_um_juiz_mutantes c11_ignora_do_motor_sem_rubrica
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.feriado_um_juiz_mutantes M1
```

### `--only` para o Code

Da pasta `app/`. Suite e savepoint real: **[nao verificado]**. [Teste do savepoint real](/home/ronald/wt-codex-feriado-um-juiz-152427/app/ponto/tests/test_feriado_um_juiz_porta.py:462) [nao verificado]

```bash
../bin/suite.sh --dir /home/ronald/wt-codex-feriado-um-juiz-152427 \
  --only "ponto.tests.test_feriado_um_juiz ponto.tests.test_feriado_um_juiz_porta"
```

## 27/11 20:58 · sessao 7c0cb70cf686 · FIM-DE-SEMANA-JA · leitura · origem=Code

A S1 registrou 18 ocorrências em 9 colaboradores. `col296`, em 10/11, está entre as 17 linhas nominais. A ocorrência adicional é `ferias_com_batida`, publicada apenas no total. A identidade, o dia e as portas dessa ocorrência, assim como o dono L‑099 de cada linha, ficam [nao verificado]. A identidade Git da cópia e a execução da S3 também ficam [nao verificado]. `c1_s1.txt:26`, `c1_s1.txt:2972`, `c1_s1.txt:2980`, `c1_s1.txt:3006`. [nao verificado]

## Peça 1 — Sonda S3

A seleção conserva o teste de saída da S1. A sonda lê as portas do retrato gravado depois de selecionar o universo, pois o drawer também consulta o fio e pode publicar um link genérico em férias. `dono=` é a resposta de `InsumosDoDono.dono`, que chama `dono_da_divergencia` com os insumos da casa e a lavra CxR, seguindo o caminho do relatório de férias. A S3 usa um `AGORA` e um `HOJE`. A classificação do export mantém o relógio interno já documentado no caminho da S1. `app/folha/retrato_folha.py:989`, `app/folha/retrato_folha.py:1010`, `app/folha/retrato_folha.py:1066`, `app/colaboradores/views.py:1001`, `app/ponto/management/commands/e6_oraculo.py:182`, `app/ponto/management/commands/e6_oraculo.py:53`, `app/ferias/management/commands/listar_ferias_com_batida.py:53`, `app/folha/fora_da_folha.py:137`.

```python
import sys

sys.dont_write_bytecode = True

import json
import logging
from collections import defaultdict
from datetime import datetime, time
from functools import lru_cache
from urllib.parse import urlsplit

from django.conf import settings
from django.db import connection
from django.utils import timezone
from django_tenants.utils import schema_context

if getattr(settings, "HASNER_SOMBRA", False) is not True:
    raise SystemExit("PAREI: HASNER_SOMBRA ausente")
db = connection.settings_dict
if (db["NAME"], db["HOST"]) != ("sombra", "sombra_db"):
    raise SystemExit("PAREI: conexao fora da Sombra")
if connection.in_atomic_block or not connection.get_autocommit():
    raise SystemExit("PAREI: a sonda exige transacao propria")

from django.contrib.auth.models import User
from django.db.models import Q
from django.urls import resolve, reverse

from chamados.juizes import dia_do_fato, pergunta_viva
from chamados.models import ChamadoColaborador, DecisaoPergunta, PerguntaDisputa
from chamados.services.fio import chamado_que_lista
from chamados.services.tarefa_do_dia import porta_da_tarefa, tarefa_do_dia
from chamados.services.vigia_fio import ListaDoAdmin, dono_fora_do_fio
from colaboradores.models import Colaborador, Empresa
from colaboradores.queries import empresas_visiveis
from colaboradores.services.calendario import resolver_dia_oferecido_nas_datas
from escala.alimentacao import celulas_do_periodo, vinculo_do_dia_carregado
from ferias.services import ferias_na_janela
from folha.fora_da_folha import _carga, _fio_da_porta, dias_que_seguram
from folha.models import RetratoFolha
from folha.retrato_folha import (
    PORTA_DO_TIPO, _balde, linha_do_colab, portas_da_linha, tipo_da_linha,
)
from ponto.janelas import janela_fechamento
from ponto.management.commands.e6_oraculo import InsumosDoDono, batida_fora_de_turno
from ponto.services.cartorio import chamados_que_falam_de_batida
from ponto.services.cxr_na_lista import cxr_lavrado
from ponto.supra_juiz import cadastro_do_dia_carregado
from ponto.turnos import (
    batidas_apuraveis, cobre_parte_do_dia, cobertura_ausencia_periodo,
    periodo_da_parcial_no_dia,
)

EMPRESA_ID = 2
MES, ANO = 11, 2026
AGORA = timezone.now()
HOJE = timezone.localdate(AGORA)
ESPERADO = 18
NV = "[nao verificado]"
FONTE_DONO = "app/ponto/management/commands/e6_oraculo.py:182,53"
ESCRITOR_PASSIVO = (
    "app/chamados/management/commands/backfill_fio_morto_e4b.py:164",
    "app/chamados/models.py:4590",
)


def origem_s1(empresa, inicio, fim, lista):
    # Mesmo critério de pertencimento da S1 c1. As leituras extras vêm depois.
    u = dias_que_seguram(empresa, MES, ANO, hoje=HOJE)
    itens = sorted(
        (i for i in u["itens"] if _balde(i) == "bloqueado"),
        key=lambda i: i["colaborador_id"],
    )
    datas = {
        cid: [r["data"] for r in dias if inicio <= r["data"] <= fim]
        for cid, (_, dias) in u["dias"].items() if dias
    }
    carga = _carga([u["colabs"][cid] for cid in datas], inicio, fim, AGORA) if datas else None
    cel_ids = {
        r["celula_id"] for _, dias in u["dias"].values() for r in dias
        if inicio <= r["data"] <= fim and r["celula_id"] is not None
    }
    perguntas = defaultdict(list)
    for p in PerguntaDisputa.objects.filter(celula_id__in=cel_ids).select_related(
        "disputa__colaborador__empresa", "celula"
    ).prefetch_related("decisoes"):
        perguntas[p.celula_id].append(p)

    casos = []
    for item in itens:
        cid = item["colaborador_id"]
        if item["motivo"] != "furo_espelho":
            if not PORTA_DO_TIPO.get(tipo_da_linha(item)):
                for dia in item.get("dias_ferias_com_batida") or (None,):
                    if dia is None or inicio <= dia <= fim:
                        casos.append((item, dia, None, None))
            continue

        colab = u["colabs"][cid]
        _, dias = u["dias"][cid]
        if not dias:
            casos.append((item, None, None, None))
        calendario = resolver_dia_oferecido_nas_datas(colab, datas.get(cid, ()), agora=AGORA)
        for r in dias:
            dia, cel_id = r["data"], r["celula_id"]
            if not inicio <= dia <= fim:
                continue
            tarefa = tarefa_do_dia(colab, dia, AGORA, carga=carga)
            publicada = porta_da_tarefa(tarefa) [nao verificado]
            fio = _fio_da_porta(publicada) [nao verificado]
            fora, papel = dono_fora_do_fio(colab, dia, perguntas[cel_id], lista, com_papel=True)
            lista.dia_na_lista(cid, dia.isoformat())
            mesa = lista.dia_do_admin(cid, dia.isoformat())
            destino = lista.destino_fora_da_mesa(cid, dia.isoformat())
            cal = calendario[dia]
            if (
                ((publicada or {}).get("rota") and fio in ("admin", "colab")) [nao verificado]
                or cal is True or fora or mesa or destino
            ):
                continue
            casos.append((item, dia, r, (tarefa, publicada, fio, cal, fora, papel))) [nao verificado]
    return casos


def campos(obj, *nomes):
    return {nome: getattr(obj, nome) for nome in nomes} if obj is not None else None


def tela(nome, template, post=None, args=(), **dados):
    out = {"url": resolve(reverse(nome, args=args)).view_name, "template": template}
    if post is not None:
        out["post"] = resolve(reverse(post, args=args)).view_name
    out.update(dados)
    return out


def dados_vinculo(ec):
    if ec is None:
        return None
    return {
        **campos(ec, "pk", "ativa", "data_inicio", "data_fim", "data_ancora_colaborador"),
        "escala": campos(ec.tipo_escala, "pk", "tipo_ciclo", "folga_dia_semana", "editavel"),
    }


def cadastro_e_tela(colab, dia, cel, protestos, cadastro):
    cid = colab.pk
    dados, telas = {}, {}
    if "PREVISTO_COM_DIA_INDEFINIDO" in protestos:
        ec = vinculo_do_dia_carregado(cid, dia)
        geradora = cel.escala_geradora if cel is not None and cel.escala_geradora_id else None
        dados["vinculo"] = dados_vinculo(ec)
        dados["geradora"] = dados_vinculo(geradora)
        telas["previsto"] = []
        if cadastro["sem_vinculo_no_dia"]:
            dados["falta_previsto"] = "vinculo_que_responda_pelo_dia"
            telas["previsto"].append(tela(
                "colaboradores:painel_vinculo", "colaboradores/partials/painel_vinculo.html",
                "colaboradores:vincular_escala", args=(cid,),
            ))
        if cadastro["folga_nao_definida"]:
            dados["falta_previsto"] = ["folga_semanal_no_tipo", "FolgaDia_no_mes"]
            if ec.ativa:
                telas["previsto"].append(tela(
                    "escala:plano_folgas", "escala/plano_folgas.html",
                    "escala:plano_folgas_aplicar",
                    filtros={"empresa": EMPRESA_ID, "ano": dia.year, "mes": dia.month},
                ))
            if ec.tipo_escala.editavel:
                telas["previsto"].append(tela(
                    "escala:wizard_tipo_editar", "escala/wizard_tipo_escala.html",
                    "escala:wizard_salvar_editar", args=(ec.tipo_escala_id,),
                ))
        if not telas["previsto"]:
            telas["previsto"] = NV
        dados.setdefault("falta_previsto", NV)

    if "PARCIAL_SEM_PERIODO" in protestos:
        a = cobertura_ausencia_periodo(colab, dia, dia, para="folha").get(dia)
        dados["ausencia_folha"] = campos(
            a, "pk", "tipo", "data", "data_fim", "status", "minutos", "hora_ini", "hora_fim",
        )
        dados["falta_periodo"] = NV
        telas["parcial"] = NV
        if (
            a is not None and a.status == "aprovada" and cobre_parte_do_dia(a)
            and periodo_da_parcial_no_dia(a, ()) == "sem_periodo"
        ):
            dados["falta_periodo"] = [k for k in ("hora_ini", "hora_fim") if getattr(a, k) is None]
            telas["parcial"] = tela(
                "ponto:painel_ausencia", "ponto/partials/painel_ausencia.html",
                "ponto:editar_periodo_ausencia", args=(a.pk,),
            )
    return dados, telas


def perguntas_no_dia(cid, dia, cel_id, instancias):
    # Relação da célula, relação por fato usada pela tarefa e IDs que ela publicou.
    q = Q(colaborador_id=cid, conversa=False, data_evento=dia)
    if cel_id is not None:
        q |= Q(celula_id=cel_id)
    q |= Q(pk__in=[i["pergunta_id"] for i in instancias if i["pergunta_id"] is not None])
    return list(PerguntaDisputa.objects.filter(q).select_related(
        "disputa__colaborador__empresa", "disputa__chamado", "chamado_destino", "celula",
    ).prefetch_related("decisoes").order_by("pk"))


def ato_e_projecao(perguntas, instancias):
    passivo = {i["pergunta_id"] for i in instancias if i["razao"] == "passivo_sem_linha"}
    faltantes = defaultdict(list)
    for ln in DecisaoPergunta.linhas_da_projecao_sem_linha([p for p in perguntas if p.pk in passivo]):
        if ln["ato_em"] <= AGORA:
            faltantes[ln["pergunta_id"]].append({"ato": ln["ato"], "em": ln["ato_em"]})
    out = []
    for p in perguntas:
        lido = DecisaoPergunta.composta(p)
        foto = {
            "pergunta": p.pk,
            "sequencia_gravada": [campos(d, "pk", "ato", "ato_em") for d in p.decisoes.all()],
            "projecao": campos(p, "respondida_em", "validada_em", "materializada_em", "via_resolucao"),
            "leitura_composta": campos(lido, "respondida_em", "validada_em", "materializada_em", "via_resolucao"),
        }
        if p.pk in passivo:
            foto["atos_faltantes"] = faltantes.get(p.pk) or NV
            foto["escritor_passivo"] = ESCRITOR_PASSIVO
        out.append(foto)
    return out


def dados_ferias(colab, dia):
    de = timezone.make_aware(datetime.combine(dia, time.min))
    ate = timezone.make_aware(datetime.combine(dia, time.max))
    ags = ferias_na_janela(dia, dia, colaborador_ids=[colab.pk]).select_related(
        "periodo_aquisitivo"
    ).order_by("pk")
    return {
        "agendamentos": [
            {
                **campos(ag, "pk", "data_inicio", "data_fim", "status"),
                "aquisitivo": campos(ag.periodo_aquisitivo, "pk", "data_inicio", "data_fim", "status"),
            }
            for ag in ags
        ],
        "batidas_apuraveis_civis": [
            {"pk": b.pk, "hora_local": timezone.localtime(b.timestamp), "tipo_gravado": b.tipo}
            for b in batidas_apuraveis(colab, de, ate).order_by("timestamp", "pk")
        ],
        "tela_do_modulo": tela("ferias:secao_ferias_colab", "ferias/_secao_colab.html", args=(colab.pk,)),
    }


def portas_retrato(retrato, cid, dia):
    linha = linha_do_colab(retrato, cid)
    if linha is None:
        return {"portas": NV, "motivo": "sem_linha_no_retrato_visivel"}
    return {
        "lavrado_em": retrato.lavrado_em,
        "motivo_lavrado": linha.get("motivo"),
        "dia_nas_datas_lavradas": str(dia) in linha["datas"] if linha.get("datas") is not None else NV,
        "portas": [
            {"url": resolve(urlsplit(p["href"]).path).view_name, "rotulo": p["rotulo"]}
            for p in portas_da_linha(linha)
        ],
    }


def molede_linhas(empresa, inicio, fim):
    lista = ListaDoAdmin(HOJE, [empresa], agora=AGORA)
    casos = origem_s1(empresa, inicio, fim, lista)
    colabs = Colaborador.objects.select_related("empresa").in_bulk(
        {i["colaborador_id"] for i, _, _, _ in casos}
    )
    cxr_da_competencia = cxr_lavrado()
    retrato = RetratoFolha.objects.filter(
        empresa_id=empresa.pk, mes=MES, ano=ANO, empresa__in=empresas_visiveis(),
    ).first()

    @lru_cache(maxsize=None)
    def cadastro_atual(cid, dia):
        return cadastro_do_dia_carregado(cid, dia)

    @lru_cache(maxsize=None)
    def fonte_dono(cid):
        cxr = cxr_da_competencia(cid, inicio)
        ins = InsumosDoDono(
            colabs[cid], inicio, fim, HOJE, agora=AGORA, sem_motor=True, cxr=cxr,
            cadastro=lambda dia: cadastro_atual(cid, dia),
        )
        return ins, cxr

    @lru_cache(maxsize=None)
    def chamados_do_colab(cid):
        por_dia = defaultdict(list)
        for ch in ChamadoColaborador.objects.filter(colaborador_id=cid).select_related("celula").order_by("pk"):
            por_dia[dia_do_fato(ch, fortes=True)].append(ch)
        return por_dia, chamados_que_falam_de_batida(colabs[cid])

    linhas = []
    for item, dia, retencao, leitura_s1 in casos:
        cid = item["colaborador_id"]
        colab = colabs[cid]
        membro = lista.colab_na_lista(cid)
        row = dict.fromkeys((
            "celula", "chamados/ancoras/C6", "rota/acao/razao", "ato/projecao",
            "dia_na_lista/dono", "insumos_L099", "destino_DP",
        ), NV)
        row.update(
            colab=f"col{cid}", dia=dia if dia is not None else NV, motivo=item["motivo"],
            colab_na_lista=NV if membro is None else membro,
            dono=NV, dono_fonte=FONTE_DONO, dono_motivo="S1_sem_dia_publicado",
            cadastro=None, tela=None, ferias=None,
            porta_tipo_S1=PORTA_DO_TIPO.get(tipo_da_linha(item)),
            retrato=portas_retrato(retrato, cid, dia),
        )
        if dia is None:
            linhas.append(row)
            continue

        cel = celulas_do_periodo([cid], dia, dia).get((cid, dia))
        cel_id = cel.pk if cel is not None else None
        protestos = sorted(cel.protestos or ()) if cel is not None else []
        ata = (cel.ata or {}) if cel is not None else {}
        if leitura_s1 is None:
            tarefa = tarefa_do_dia(colab, dia, AGORA)
            porta = porta_da_tarefa(tarefa)
            fio = _fio_da_porta(porta)
            cal = resolver_dia_oferecido_nas_datas(colab, [dia], agora=AGORA)[dia]
            ligadas = PerguntaDisputa.objects.filter(celula_id=cel_id) if cel_id is not None else ()
            fora, papel = dono_fora_do_fio(colab, dia, ligadas, lista, com_papel=True)
        else:
            tarefa, porta, fio, cal, fora, papel = leitura_s1

        instancias = tarefa["instancias"]
        perguntas = perguntas_no_dia(cid, dia, cel_id, instancias)
        por_dia, cartorio = chamados_do_colab(cid)
        cadastro = cadastro_atual(cid, dia)
        ins, cxr = fonte_dono(cid)
        iso = dia.isoformat()
        dono = ins.dono(dia)
        na_lista = lista.dia_na_lista(cid, iso)
        destino = lista.destino_fora_da_mesa(cid, iso)
        dados, telas = cadastro_e_tela(colab, dia, cel, protestos, cadastro)
        row.update({
            "celula": None if cel is None else {
                **campos(cel, "pk", "trabalha", "origem", "escala_geradora_id", "ausencia_id",
                         "julgada_em", "veredito", "veredito_via"),
                "protestos": protestos,
                "ata": {k: ata.get(k) for k in (
                    "tipo_dia", "minutos_previstos", "minutos_realizados",
                    "sem_ancora", "cobertura_parcial", "periodo_parcial",
                )},
            },
            "motivo_dia": retencao["motivo"] if retencao is not None else item["motivo"],
            "chamados/ancoras/C6": {
                "dia_forte": [campos(ch, "pk", "status_local", "modulo_origem") for ch in por_dia.get(dia, ())],
                "cartorio": [campos(ch, "pk", "status_local", "modulo_origem") for ch in cartorio.get(iso, ())],
                "fila_publicada": (porta or {}).get("fila", ()),
                "perguntas": [{
                    **campos(p, "pk", "disputa_id", "celula_id", "chamado_destino_id"),
                    "pai": p.disputa.chamado_id,
                    "ancora": chamado_que_lista(p),
                    "C6": pergunta_viva(p, hoje=HOJE, agora=AGORA),
                } for p in perguntas],
            },
            "rota/acao/razao": {
                "porta": porta, "fio": fio, "razao": tarefa["razao"],
                "instancias": [{k: i[k] for k in ("instancia", "pergunta_id", "razao", "acoes")} for i in instancias],
                "calendario": NV if cal is None else cal, "fora_fio": fora, "papel": papel,
            },
            "ato/projecao": ato_e_projecao(perguntas, instancias),
            "dia_na_lista/dono": NV if na_lista is None else {
                "dia_na_lista": na_lista[0], "dono_lavrado": na_lista[1],
                "mesa_admin": lista.dia_do_admin(cid, iso),
            },
            "insumos_L099": {
                **cadastro,
                "horas_oraculo": ins.horas.get(iso, []),
                "batida_fora_de_turno": batida_fora_de_turno(colab, dia, ins.turnos()),
                "fase_deslocada": iso in ins.fase(),
                "em_cadastro_x_realidade": NV if cxr is None else iso in cxr,
            },
            "dono": NV if dono is None else dono,
            "dono_motivo": "lavra_CxR_sem_cobertura_e_insumo_necessario" if dono is None else None,
            "destino_DP": NV if na_lista is None else destino,
            "cadastro": dados,
            "tela": telas,
            "ferias": dados_ferias(colab, dia) if item["motivo"] == "ferias_com_batida" else None,
        })
        linhas.append(row)
    return linhas


def colher():
    with schema_context("juliani"):
        connection.set_autocommit(False)
        try:
            with connection.cursor() as cursor:
                cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
                cursor.execute(
                    "SELECT current_schema(), current_setting('transaction_read_only'), "
                    "current_setting('transaction_isolation')"
                )
                if cursor.fetchone() != ("juliani", "on", "repeatable read"):
                    raise SystemExit("PAREI: schema ou modo de transacao incorreto")
            if not User.objects.filter(username="sistema").exists():
                raise SystemExit("PAREI: usuario de sistema ausente na Sombra")
            empresa = Empresa.objects.get(pk=EMPRESA_ID)
            inicio, fim = janela_fechamento(MES, ANO, empresa)
            linhas = molede_linhas(empresa, inicio, fim)
            with connection.cursor() as cursor:
                cursor.execute("SELECT 1")
            return linhas
        finally:
            connection.rollback()
            connection.set_autocommit(True)


class SemErroComDadosLivres:
    def __enter__(self):
        return self

    def __exit__(self, tipo, erro, quadro):
        if tipo is None or issubclass(tipo, (SystemExit, KeyboardInterrupt)):
            return False
        while quadro.tb_next is not None:
            quadro = quadro.tb_next
        arquivo = quadro.tb_frame.f_code.co_filename.rsplit("/", 1)[-1]
        raise SystemExit(f"PAREI: {tipo.__name__} em {arquivo}:{quadro.t

[truncado em 20000 de 32230 caracteres]

## 27/11 20:54 · sessao f4be0ffb7b4d · - · - · origem=Ronald (tmux)

- A autoridade do dono é `InsumosDoDono(..., sem_motor=True, cxr=cxr_lavrado()(cid, ini)).dono(dia)`, que chama `dono_da_divergencia`. O mesmo caminho já existe no relatório nominal de férias. `None` significa que a lavra CxR não cobre a competência do colaborador e que o juiz precisaria desse insumo para resolver o dono. Registre horas do oráculo, fase, turno aberto com batida e as três chaves de cadastro pelos mesmos leitores. O juiz pode devolver `BATIDA` ou `ESTRUTURA` para um protesto cuja origem o supra-juiz descreve como cadastro, pois `PARCIAL_SEM_PERIODO` não é um argumento do E6. `app/ponto/management/commands/e6_oraculo.py:53`, `app/ponto/management/commands/e6_oraculo.py:182`, `app/ponto/management/commands/e6_oraculo.py:198`, `app/ferias/management/commands/listar_ferias_com_batida.py:53`, `app/ponto/services/cxr_na_lista.py:165`, `app/ponto/supra_juiz.py:180`, `app/ponto/supra_juiz.py:395`.
- IDs exatos da pergunta do `passivo_sem_linha` vêm das instâncias da tarefa. `DecisaoPergunta.linhas_da_projecao_sem_linha` devolve o tipo e o instante do ato faltante. O leitor da tarefa corta em `ato_em <= AGORA`, e `composta` lê a projeção combinada. `DecisaoPergunta.registrar` é o escritor único da sequência. Resposta humana e validação são publicadas pelas respectivas portas junto à projeção, e o backfill tem a correção específica do passivo. A porta que omitiu a linha de cada caso segue [nao verificado]. `app/chamados/services/tarefa_do_dia.py:338`, `app/chamados/services/tarefa_do_dia.py:433`, `app/chamados/models.py:4416`, `app/chamados/models.py:4507`, `app/chamados/models.py:4590`, `app/chamados/services/disputa_emissao.py:1565`, `app/chamados/services/validacao.py:176`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:164`.
- A S1 usa só `PORTA_DO_TIPO['ferias']`, que vale `None`. A publicação do drawer consulta também o fio. O contrato vigente da linha de férias produz fio `admin` e permite o link genérico `chamados:modal_fio`. Leia a publicação efetivamente lavrada por `linha_do_colab` + `portas_da_linha`, chamadas pela view do painel. A existência de pergunta ou ação nesse link para o caso nominal segue [nao verificado]. `app/folha/retrato_folha.py:351`, `app/folha/retrato_folha.py:989`, `app/folha/retrato_folha.py:1003`, `app/folha/retrato_folha.py:1066`, `app/colaboradores/views.py:996`, `app/folha/management/commands/lavrar_retrato_folha.py:77`.
- O cruzamento da Folha usa `ferias_na_janela` para o agendamento, e `batidas_apuraveis` pela data civil. O relatório nominal de férias usa um universo distinto, de `Ausencia` espelho. A seção administrativa de férias e a página de saldo chamam um escritor de status durante o GET. `app/folha/export.py:464`, `app/ferias/services.py:49`, `app/ponto/turnos.py:2941`, `app/ferias/management/commands/listar_ferias_com_batida.py:4`, `app/ferias/views.py:121`, `app/ferias/views.py:838`.
- Para `col296`, o leitor canônico de chamados que compõem o vocabulário do cartório é `chamados_que_falam_de_batida`. A versão de chamados vivos que protestou no julgamento é um subconjunto filtrado pela ata daquele ato. A data forte do chamado, as âncoras por pergunta (destino ou pai), o C6 e o histórico da disputa são leituras distintas. A causa da permanência da cobrança segue [nao verificado]. `app/ponto/services/cartorio.py:131`, `app/ponto/services/cartorio.py:1498`, `app/ponto/services/cartorio.py:1618`, `app/chamados/juizes.py:89`, `app/chamados/juizes.py:911`, `app/chamados/services/fio.py:97`.
- A lista administrativa lê a pertença do colaborador, a presença do dia, o dono lavrado e o destino DP por portas distintas. O encaminhamento `pauta-dp` é publicado para o C13 entregue, com fonte no serviço de pares-relâmpago. `app/chamados/services/vigia_fio.py:210`, `app/chamados/services/vigia_fio.py:228`, `app/chamados/services/vigia_fio.py:238`, `app/escala/services/cadastro_realidade.py:1274`, `app/ponto/services/par_relampago_na_lista.py:375`.

## 27/11 20:54 · sessao 416874d39030 · - · - · origem=Ronald (tmux)

- **`PREVISTO_COM_DIA_INDEFINIDO`.** Ler `cadastro_do_dia_carregado(cid, dia)`, `vinculo_do_dia_carregado(cid, dia)` e `celulas_do_periodo([cid], dia, dia)`. Os parâmetros do juiz distinguem folga 6x1 indefinida e vínculo inválido. A célula traz o vínculo gerador, `trabalha` e `ata['sem_ancora']`. Esse protesto tem vários ramos, de modo que o campo exato de cada linha depende desses insumos. `app/ponto/supra_juiz.py:179`, `app/ponto/supra_juiz.py:350`, `app/ponto/supra_juiz.py:369`, `app/ponto/supra_juiz.py:408`, `app/ponto/supra_juiz.py:467`, `app/escala/alimentacao.py:54`, `app/escala/alimentacao.py:289`, `app/ponto/services/cartorio.py:749`.

- **Folga do mês.** `reverse('escala:plano_folgas')`, com empresa, ano e mês civil do dia. A view `plano_folgas` usa `escala/plano_folgas.html`. O POST `escala:plano_folgas_aplicar` chama `aplicar_folgas_em_lote`. O POST `escala:plano_folgas_toggle` usa `lancar_folga` e `alterar_folga`. O grid e os dois POSTs exigem vínculo ativo. `defeito_folga_nao_definida` lê `TipoEscala.folga_dia_semana` e `FolgaDia` do mês. A C11 indica Plano de Folgas para o caso que publica com dono de cadastro. `app/escala/urls.py:8`, `app/escala/views_plano_folgas.py:29`, `app/escala/views_plano_folgas.py:98`, `app/escala/views_plano_folgas.py:144`, `app/escala/folgas.py:303`, `app/escala/folgas.py:369`, `app/escala/folgas.py:385`, `app/escala/models.py:1252`, `app/escala/services/cadastro_realidade.py:793`.

- **Folga semanal no template.** `reverse('escala:wizard_tipo_editar', args=[tipo.pk])`, `escala/wizard_tipo_escala.html`, POST `escala:wizard_salvar_editar`. O escritor `salvar_tipo` atribui `folga_dia_semana`. `app/escala/urls.py:25`, `app/escala/urls.py:27`, `app/escala/views_wizard.py:65`, `app/escala/views_wizard.py:98`, `app/escala/services/cadastro_tipo.py:261`.

- **Vínculo.** `reverse('colaboradores:painel_vinculo', args=[cid])`, `colaboradores/partials/painel_vinculo.html`, POST `colaboradores:vincular_escala`. A view chama `executar_vinculo` para atribuição do template, início, fase e reativação de vínculo do mesmo início. Data de fim histórica segue os escritores de vigência, e o ajuste específico de vigência inválida localizado é o comando `corrigir_vinculo_vigencia`. `app/colaboradores/urls.py:12`, `app/colaboradores/views.py:262`, `app/colaboradores/views.py:306`, `app/templates/colaboradores/partials/painel_vinculo.html:136`, `app/colaboradores/views.py:392`, `app/colaboradores/views.py:442`, `app/colaboradores/services/vinculo.py:259`, `app/colaboradores/services/vinculo.py:582`, `app/escala/management/commands/corrigir_vinculo_vigencia.py:86`.

- **`PARCIAL_SEM_PERIODO`.** Usar a cobertura da grade `cobertura_ausencia_periodo(colab, dia, dia, para='folha').get(dia)`, ler `pk`, `tipo`, `data`, `data_fim`, `status`, `minutos`, `hora_ini`, `hora_fim`. `cobre_parte_do_dia` e `periodo_da_parcial_no_dia(a, ())` identificam a falta atual. A ata registra o período visto pelo cartório. O painel `reverse('ponto:painel_ausencia', args=[a.pk])` usa `ponto/partials/painel_ausencia.html` e o form POST `ponto:editar_periodo_ausencia`. A view lê as duas horas e chama `_a4_decidir` → `decidir` → `_aplicar_periodo`, que grava os campos. `app/escala/utils.py:1230`, `app/ponto/turnos.py:2619`, `app/ponto/turnos.py:2653`, `app/ponto/turnos.py:2816`, `app/ponto/services/cartorio.py:785`, `app/ponto/views.py:2095`, `app/templates/ponto/partials/painel_ausencia.html:310`, `app/ponto/views.py:2836`, `app/ponto/services/ausencia.py:1051`, `app/ponto/services/ausencia.py:1102`.

- **Férias.** `ferias_na_janela(dia, dia, colaborador_ids=[cid])` e `batidas_apuraveis(colab, de, ate)` com janela civil inclusiva em fuso local são as fontes do export. Agendamentos são escritos por `executar_agendamento`, `_trocar_janela`, canceladores e `alinhar_status_por_data`. Batidas são escritas por `registrar_batida`, com `retratar_batida` e `restaurar_batida` alterando sua participação na apuração. Para portas publicadas no drawer ler `linha_do_colab(retrato, cid)` e `portas_da_linha(linha)` quando a linha lavrada contiver o motivo e dia. A porta estática `ferias` é `None`. A Gestão de Férias tem a URL genérica `ferias:lista_ferias`. `app/ferias/services.py:49`, `app/folha/export.py:490`, `app/ponto/turnos.py:2941`, `app/ferias/services.py:581`, `app/ferias/services.py:694`, `app/ferias/services.py:940`, `app/ponto/registro_batida.py:118`, `app/ponto/registro_batida.py:287`, `app/ponto/registro_batida.py:305`, `app/folha/retrato_folha.py:930`, `app/folha/retrato_folha.py:1010`, `app/folha/retrato_folha.py:1024`, `app/colaboradores/views.py:1001`, `app/ferias/urls.py:7`.

- **Passivo e dono.** `DecisaoPergunta.linhas_da_projecao_sem_linha` devolve ato, instante e pergunta que a projeção contém e a sequência carece. A tarefa filtra pelo instante e o backfill passa esses atos ao escritor `registrar`. O juiz L‑099 usa vínculo/folga, batida/turno, fase e CxR. O pedido de cadastro da ausência parcial vem do supra-juiz e do predicado do cartório, enquanto o retorno nominal da L‑099 depende de insumos do dia [nao verificado]. `app/chamados/models.py:4416`, `app/chamados/models.py:4590`, `app/chamados/services/tarefa_do_dia.py:433`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:161`, `app/ponto/management/commands/e6_oraculo.py:98`, `app/ponto/supra_juiz.py:395`, `app/ponto/services/cartorio.py:416`.

## 27/11 20:54 · sessao 5a41dcb3b87d · - · - · origem=Ronald (tmux)

- O retrato da Folha traduz as portas diretamente com `linha_do_colab` e `portas_da_linha`. Férias têm `PORTA_DO_TIPO['ferias'] = None`. Um retrato com fio `admin` pode publicar o link genérico `chamados:modal_fio` pelo fallback. O escritor do retrato é `lavrar_retrato_folha`, e o drawer o lê sem recalcular. `app/folha/retrato_folha.py:351`, `app/folha/retrato_folha.py:930`, `app/folha/retrato_folha.py:989`, `app/folha/retrato_folha.py:1010`, `app/folha/retrato_folha.py:1024`, `app/folha/management/commands/lavrar_retrato_folha.py:77`, `app/colaboradores/views.py:996`.
- A aba `ferias:secao_ferias_colab` é uma tela independente. O GET sincroniza e pode escrever o período aquisitivo. A sonda pode resolver a URL e consultar `ferias_na_janela(dia, dia, colaborador_ids=[cid])` e `batidas_apuraveis` com os limites locais do dia. `app/ferias/urls.py:14`, `app/ferias/views.py:832`, `app/ferias/models.py:152`, `app/ferias/services.py:49`, `app/folha/export.py:490`, `app/ponto/turnos.py:2941`.
- Os chamados por FK ou por dia forte podem ser lidos com `dia_do_fato(ch, fortes=True)`, a âncora da pergunta é `chamado_que_lista(p)` e C6 é `pergunta_viva(p, hoje=HOJE, agora=AGORA)`. A tarefa inclui perguntas por colaborador e `data_evento`, inclusive sem célula, enquanto o filtro original da S1 passa só as da célula a `dono_fora_do_fio`. A fila publicada deve ser exibida à parte dos chamados ligados ao fato. `app/chamados/juizes.py:89`, `app/chamados/juizes.py:911`, `app/chamados/juizes.py:1695`, `app/chamados/services/fio.py:97`, `app/chamados/services/tarefa_do_dia.py:532`, `app/chamados/services/tarefa_do_dia.py:805`.
- As leituras `colab_na_lista`, `dia_na_lista` e `destino_fora_da_mesa` vêm de `ListaDoAdmin`. O sinal entregue do C13 publica `encaminhamento` em `pendentes`, consumido por `dias_da_pauta_dp`. Se a lista inteira ficou sem resposta, `destino=None` também fica indeterminado. `app/chamados/services/vigia_fio.py:210`, `app/chamados/services/vigia_fio.py:228`, `app/chamados/services/vigia_fio.py:238`, `app/ponto/services/par_relampago_na_lista.py:341`, `app/escala/services/cadastro_realidade.py:1274`.
- A guarda da conta `sistema` é útil sem os atores da S1, pois o C6 consulta um helper que usa `get_or_create`. A chamada real da S1 já possui resíduo de relógio interno documentado na classificação do export. `app/core/utils.py:6`, `app/chamados/juizes.py:1323`, `app/folha/fora_da_folha.py:137`, `app/folha/export.py:1247`.

## 27/11 20:36 · sessao 12c65147695d · FIM-DE-SEMANA-JA · leitura · origem=Code

Usei a cópia local para as referências de código e numerei o `hasner.txt` colado a partir do primeiro `S1|UNIVERSO`. A identidade Git da cópia com o hash informado fica [nao verificado]. Os números são do recorte J.A., empresa 2, competência 11, de 21/10 a 20/11. `hasner.txt:1`.

Pela L-099, o dono do **fato** e o dono de uma **falha de encaminhamento** podem ser diferentes. Um cadastro incompleto pode explicar a retenção do dia, enquanto uma divergência entre a porta e a tela é ESTRUTURA. Atribuo o dono e o sítio apenas até onde o código e o anexo sustentam. [app/docs/LEIS.md:122](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/LEIS.md:122).

## C1 — 18 dias sem saída e três marcas

**Escritor e leitores comuns.** O cartório chama `classificar_dia`, entrega os protestos a `CelulaDia.lavrar_veredito` e grava pela porta da célula. `motivos_retencao_celula` lê a lavra para decidir a retenção e `dias_que_seguram` dá esse universo à S1. Para o fio, `tarefa_do_dia` decide e `porta_da_tarefa` publica rota, ator e ação. `app/ponto/services/cartorio.py:1505`, `app/ponto/services/cartorio.py:1908`, `app/ponto/portas/celula.py:1069`, `app/folha/export.py:1282`, `app/folha/export.py:1302`, `app/folha/fora_da_folha.py:125`, `app/chamados/services/tarefa_do_dia.py:805`, `app/chamados/services/tarefa_do_dia.py:809`.

**`hasner_sem_saida` — 18 dias, 9 colabs.** A S1 usa os leitores reais, mas monta o rótulo pela união de tarefa, calendário, lista e Pauta DP. Um falso positivo possível é a rota com fio `chamado`, que o vigia real aceita e a S1 exclui da sua primeira condição. A Folha também distingue `nao_sei` de `nada`; a S1 inclui dois `nao_sei` com alcance indeterminado. O `chamados=0` do total conta o primeiro PK da fila publicada pela tarefa, de modo que a existência de chamados por outras âncoras fica [nao verificado]. `hasner.txt:11`, `hasner.txt:94`, `hasner.txt:96`, `app/chamados/services/vigia_fio.py:642`, `app/folha/fora_da_folha.py:215`.

- Dos 17 marcados sem saída, 11 têm `PREVISTO_COM_DIA_INDEFINIDO` e três têm `PARCIAL_SEM_PERIODO`, ambos **CADASTRO** pelo juiz. Tirar esses dias da retenção sem acertar o cadastro ocultaria, por exemplo, uma ausência parcial aprovada sem período. Nos dois `FURO_PARCIAL`, o dono do fato fica [nao verificado]. A falta de leitura com `passivo_sem_linha` é **ESTRUTURA** na trilha de `DecisaoPergunta`; a tarefa se abstém quando a projeção carece do ato. A E4C-F8 está aberta e migra leitores da projeção. A cobertura desses dois casos pela obra fica [nao verificado]. O padrão é escritor único e tipo soma, preservando a distinção entre leitura desconhecida e ausência de porta. `hasner.txt:80–96`, `app/ponto/supra_juiz.py:350`, `app/ponto/supra_juiz.py:369`, `app/ponto/supra_juiz.py:395`, `app/ponto/management/commands/e6_oraculo.py:182`, `app/chamados/services/tarefa_do_dia.py:340`, `app/chamados/models.py:4416`, `app/docs/BACKLOG.md:2211`, `app/docs/GLOSSARIO.md:12`, `app/docs/GLOSSARIO.md:34`.
- O 18º é `ferias_com_batida`. O escritor da classificação é `classificar_export`, que cruza férias e batidas apuráveis. Para esse ramo, a S1 usa só `PORTA_DO_TIPO`, onde `ferias` não tem link. O dono da sobreposição e a existência de saída por outra tela ficam [nao verificado]. Liberar o TXT só para zerar o caso pode pagar novamente um dia que sai pelo módulo de férias. `hasner.txt:45`, `app/folha/export.py:464`, `app/folha/export.py:885`, `app/folha/retrato_folha.py:1003`, `app/folha/retrato_folha.py:1024`.

**`cobranca_indevida` — 7 marcas.** A S1 lê o veredito real. Os sete registros exibem `COBRANCA_SEM_FURO`: cinco têm `validar`, col246 tem lista do admin e col296 saiu sem porta na S1. Para a cobrança que subsiste após o fato ter sido satisfeito, o problema é **ESTRUTURA** no encerrador do cartório, `_julgar_colab_corpo`, no ramo que chama `reconciliar`. A **E4B-SCA-25-COBRANCA-INDEVIDA**, aberta no BACKLOG, cita col246 em 01/11 e ainda pede a causa da não baixa. O padrão é a lâmpada do fato acionando a reconciliação pelo escritor único. O contraexemplo é a resposta sem veredito que precisa sobreviver mesmo com a lâmpada acesa. A causa da permanência nos sete casos fica [nao verificado]. `hasner.txt:27`, `hasner.txt:90`, `hasner.txt:158`, `hasner.txt:174`, `hasner.txt:180–183`, `app/ponto/supra_juiz.py:475`, `app/ponto/services/cartorio.py:1618`, `app/chamados/models.py:718`, `app/docs/BACKLOG.md:2318`, `app/docs/GLOSSARIO.md:12–13`.

**`discordante` — 12 marcas.** A S1 lê o veredito real, que agrega batida órfã fora da tolerância, realizado inflado e realizado zero com turno. As 12 linhas têm saída na reconstrução: quatro pelo app, sete por `resolver_dia` e uma por `decidir`. O leitor de produção da Pauta de discordantes consulta também dias sem fio e o juiz do dono. Dono, sítio e obra por caso ficam [nao verificado]. Se houver divergência entre leitores com os mesmos insumos, aplica-se o padrão de fonte única. Zerar a marca por filtro esconderia, por exemplo, o realizado zero apesar dos marcos casados. `hasner.txt:20`, `hasner.txt:22–23`, `hasner.txt:26`, `app/escala/models.py:1627`, `app/ponto/supra_juiz.py:294`, `app/ponto/management/commands/pautas_discordantes.py:72`, `app/ponto/management/commands/pautas_discordantes.py:118`, `app/ponto/management/commands/e6_oraculo.py:98`, `app/docs/GLOSSARIO.md:11`.

**`fio_ninguem_nao_sei` — 21 marcas.** Aqui a marca é da S1, que junta dois resultados do leitor real. Quatro dias têm `fio_do_admin` fora da tarefa e 17 foram rotulados sem saída. O dono do fato segue os protestos e a L-099. O `nao_sei` é falha de leitura do sistema. Para uma omissão confirmada no ramo sem perguntas vivas, com o colab na lista e o dia fora dela, o sítio é `vigia_fio.censo_alcance`, ramo `d`, tocado pela **CLASSE-D-NA-FRENTE-DA-E4D**, aberta no BACKLOG. O enquadramento dos 17 nesse ramo fica [nao verificado]. O padrão é ler o dono e a rota da fonte única. Transformar toda abstenção em porta de cobrança esconderia a falta de ato no passivo. `hasner.txt:19`, `hasner.txt:21`, `hasner.txt:25`, `app/folha/fora_da_folha.py:57`, `app/folha/fora_da_folha.py:86`, `app/chamados/services/vigia_fio.py:202`, `app/chamados/services/vigia_fio.py:680`, `app/chamados/services/tarefa_do_dia.py:340`, `app/docs/BACKLOG.md:2239`, `app/docs/GLOSSARIO.md:11`, `app/docs/GLOSSARIO.md:34`.

**Linha faltante:** `C1|colab|dia|celula|chamados/ancoras/C6|rota/acao/razao|ato/projecao|colab_na_lista|dia_na_lista/dono|insumos_L099|destino_DP`. Para férias, falta ainda a linha nominal com agendamento, batida apurável e portas efetivamente publicadas. `app/chamados/services/vigia_fio.py:210`, `app/chamados/services/vigia_fio.py:238`, `app/ponto/management/commands/e6_oraculo.py:182`, `app/folha/export.py:490`, `hasner.txt:45`.

## C2 — `chamado_com_bloqueio_sem_acao_gestor_ou_colab`

**38 chamados distintos, 10 colabs.** Basta um par chamado–pergunta sem ação no recorte para o chamado entrar no total. O escritor de estado é `ChamadoColaborador._gravar_estado`; o nascimento da pergunta e o vínculo com o destino passam por `_nascer`. Os leitores reais são o plano e o gate de pergunta viva e, no caminho de `reconciliar`, o gate da conversa do pai, que consulta `perguntas_que_seguram_csf`. `hasner.txt:5`, `hasner.txt:47–53`, `app/chamados/models.py:213`, `app/chamados/models.py:3137`, `app/chamados/models.py:393`, `app/chamados/models.py:429`, `app/chamados/models.py:683`, `app/chamados/models.py:718`, `app/ponto/supra_juiz.py:561`.

A S1 usa `_vivas_ancoradas("fechado")` e acrescenta o CSF, mas recompõe o bloqueio. O gate real deixa fechar se **todas** as vivas puderem ser reatadas. A sonda também exclui a disputa própria selecionada e exige C6 das perguntas trazidas pelo CSF. Para alcance, renderiza cartões, simula três verbos com uma conta gestora e estima login novo do colab. Um falso positivo concreto é **Conferir data**: a S1 retira `pode_conferir_data` do contexto, embora o parcial publique o controle e exista uma porta real. A ação disponível por outros atores e o resultado do POST ficam [nao verificado]. `app/chamados/models.py:332`, `app/chamados/models.py:393`, `app/chamados/models.py:451`, `app/ponto/supra_juiz.py:573`, `app/chamados/services/fio.py:610`, `app/templates/chamados/partials/modal_fio.html:169`, `app/chamados/views.py:1288`, `app/api/login_helpers.py:118`.

**Dono, sítio e padrão.** Uma contradição confirmada entre a ação autorizada e o cartão é **ESTRUTURA** em `fio.contexto_parcial_fio`. A **E4C-F5-82-FALTA**, aberta, toca esse sítio na divergência porta × cartão por dono. O padrão é a tela ler o mesmo juiz de âncora e autorização que a porta aplica. Os 38 vêm só agregados, então dono e causa por chamado ficam [nao verificado]. Contraexemplo: liberar o pai pelo sistema para zerar a classe pode enterrar uma resposta que ainda exige decisão humana. Falta `C2|chamado|pergunta|origem/destino|plano_de_reata|CSF|C6|ator/perfil|canal/acao|motivo_da_recusa`, por par. `hasner.txt:47–53`, `app/chamados/services/fio.py:97`, `app/chamados/services/fio.py:573`, `app/chamados/models.py:735`, `app/docs/BACKLOG.md:2059`, `app/docs/GLOSSARIO.md:12`, `app/docs/GLOSSARIO.md:33`.

## C3 — `premissa_morta:*`

**Leitura comum.** `_veredito_premissa` coleta o fato e `catalogo.premissa.avaliar` devolve o motivo real. A S1 conta o veredito antes dos descartes. O vigia de produção exclui colab com evento pendente e separa retenção da retratação e recusa de `reconciliaria()`. O escritor `retratar_premissa_morta_colab` chama `retratar_com_motivo`, que relê sob trava, aplica a cerca da competência e chama `reconciliar` para gravar. Assim, o resultado real do juiz pode virar falso positivo **de falta de saída** na contagem bruta. Os 20 `retido_pelo_fechamento` vêm de outra leitura da união e seu cruzamento com os 54 fica [nao verificado]. `hasner.txt:54–59`, `app/chamados/catalogo/premissa.py:115`, `app/chamados/reconciliador.py:248`, `app/chamados/reconciliador.py:587`, `app/chamados/reconciliador.py:602`, `app/chamados/reconciliador.py:1600`, `app/chamados/management/commands/reconciliar_chamados.py:62`.

- **`turno_aberto ausente no nucleo` — 48 chamados, 31 colabs.** O catálogo aceita vários módulos de nascimento. O leitor usa o turno denunciado, por entrada ou dia, e cai no teste global quando falta data. O escritor do fato no núcleo é `recompute_turnos`, que emite evento quando o turno aberto some do recorte e existe célula. Se o evento devido desse recompute faltou, o dono é **ESTRUTURA** e o sítio é `recompute_turnos`. A obra que cobre esse sítio, **C20-JUIZ-3**, consta **FECHADA**. Obra aberta nesse sítio e causa dos 48 ficam [nao verificado]. O padrão é **Transactional Outbox**. Contraexemplo: a premissa do núcleo pode morrer enquanto o chamado ainda é a cobrança viva de um dia furado para a folha; o gate deve retê-lo. `hasner.txt:58`, `app/chamados/catalogo/premissa.py:45`, `app/chamados/reconciliador.py:118`, `app/chamados/reconciliador.py:287`, `app/ponto/nucleo.py:40`, `app/ponto/nucleo.py:86`, `app/chamados/models.py:783`, `app/docs/BACKLOG.md:2094`, `app/docs/PLANO-PADROES.md:10`.
- **`filhos_vivos ausente` — 6 chamados, 6 colabs.** `criar_disputa_a_partir_de_chamados_orfaos` abre o pai agregador e grava os IDs; `_filhos_vivos` lê o estado desses filhos. O receiver `_filho_morto_acorda_o_pai` chama a reconciliação no commit quando um filho sai de `VIVOS`. Se esse despertar devido falhou, o dono é **ESTRUTURA** e o sítio é esse receiver. Obra aberta nesse sítio e causa dos seis ficam [nao verificado]. O padrão é reconciliação dirigida pelo evento do filho. Contraexemplo: o último filho pode morrer deixando no pai uma resposta ainda sem consumo. `hasner.txt:57`, `app/chamados/services/disputa_emissao.py:1041`, `app/chamados/services/disputa_emissao.py:1118`, `app/chamados/reconciliador.py:239`, `app/chamados/signals.py:351`, `app/chamados/signals.py:371`, `app/chamados/reconciliador.py:440`, `app/docs/GLOSSARIO.md:16`.

**Linha faltante para as duas:** `C3|chamado|modulo/subcategoria|dia/entrada_ou_ids_e_estados_dos_filhos|mudanca_do_fato|evento/criacao/processamento/erro|retencao|reconciliaria/guarda|cerca_no_ato`. Se ela apontar a cerca do escritor, o sítio comum é `retratar_com_motivo`, tocado pela **E4C-F1-B2b** aberta. O BACKLOG registra o F0 já no ar com os instrumentos desligados. `app/chamados/reconciliador.py:558`, `app/chamados/reconciliador.py:1546`, `app/docs/BACKLOG.md:828`, `app/docs/BACKLOG.md:2210`.

## C4 — `hasner_sem_porta`

**8 disputas.** `_abrir_ou_agregar_disputa` grava a disputa aberta, `_nascer` grava a pergunta, `responder_pergunta_disputa_core` grava a resposta e `DisputaSupervisao.fechar` grava o fechamento. C6 e `respostas_sem_veredito` são leituras reais, consumidas por `pode_fechar` no C7. A S1 reconstrói o alcance com as mesmas limitações de cartões, verbos e atores de C2. Aplica ainda o corte de 30 dias e rotula a disputa inteira quando **uma** pergunta fica sem porta, mesmo com outras portas na mesma disputa. O dia impresso pode ser de uma pergunta irmã. A disputa 1398 ilustra a coexistência de sem porta, veredito e resposta. `hasner.txt:60–63`, `hasner.txt:98`, `app/chamados/services/disputa_emissao.py:582`, `app/chamados/services/disputa_emissao.py:1529`, `app/chamados/models.py:3137`, `app/chamados/models.py:2000`, `app/chamados/models.py:2047`, `app/chamados/models.py:2199`, `app/chamados/services/fio.py:610`.

**Dono, sítio e padrão.** Se o C7 já autoriza fechar e a disputa fica aberta por falha de reação aos filhos, é **ESTRUTURA** no agregado `DisputaSupervisao`, no fluxo de `fechar`. A **RAIZ-DA-DISPUTA** consta aberta e toca esse elo. O padrão é **Aggregate Root** com escritor único. Dono e mecanismo por disputa ficam [nao verificado]. Contraexemplo: fechar por idade a disputa 1362 esconderia a resposta sem veredito da pergunta 39872, que também aparece sem porta na S1. Na mesma disputa, a pergunta 53621 aparece em C5 como guarda do chamado 36799. Falta `C4|disputa|pergunta|dia_do_fato|C6|C7_sem_veredito|pode_fechar|ator/canal|acao/recusa|insumos_L099`, por pergunta e sem truncar IDs. `hasner.txt:97`, `hasner.txt:116`, `app/chamados/models.py:2122`, `app/docs/BACKLOG.md:1632`, `app/docs/BACKLOG.md:2326`, `app/docs/GLOSSARIO.md:11–12`.

## C5 — `so_saida_pelo_desligado` e `sem_guarda`

**Escritor e leitores comuns.** `executar_desligamento` grava situação e credencial, tenta fechar primeiro as disputas sem resposta pendente de veredito, chama `encerrar` nos chamados e grava S89. `encerrar` relê o estado sob trava e usa o gate de pergunta viva antes de gravar. A recusa CSF do pai pertence ao caminho de `reconciliar`, que o fechamento da disputa pode disparar antes de `encerrar`. `app/colaboradores/services/desligamento.py:128`, `app/colaboradores/services/desligamento.py:164`, `app/colaboradores/services/desligamento.py:175`, `app/colaboradores/services/desligamento.py:182`, `app/colaboradores/services/desligamento.py:195`, `app/chamados/models.py:1779`.

- **`so_saida_pelo_desligado` — 31 chamados, quatro colabs.** A S1 usa o gate real em seco. O “só saída” é reconstruído: todas as perguntas devolvidas pelo plano esperam texto, aparecem em app ou web, a elegibilidade estimada falha e a conta DP medida fica sem os três verbos. Quando uma reata falha, o plano devolve **todas** as vivas. A S1 também fotografa a guarda atual antes da sequência que, no ato, tenta fechar a disputa e pode carimbar as mudas. Logo, os 31 criados antes do S89 não provam 31 recusas históricas. O contraexemplo está no teste da cascata: fecha o chamado da pergunta muda e conserva aberto o da resposta sem veredito. `hasner.txt:67`, `hasner.txt:78`, `app/chamados/models.py:393`, `app/chamados/services/fio.py:213`, `app/api/login_helpers.py:118`, `app/colaboradores/services/desligamento.py:164`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:327`.
- **`sem_guarda` — chamado 36413, col427.** A foto seca tem plano sem bloqueio de `ENCERRAR` e `csf_pai_recusa=0`. O plano deixa passar tanto lista vazia como vivas todas reatáveis. A história do chamado e a permissão da porta humana ficam [nao verificado]. Contraexemplo: a transição humana recusa justificativa pendente. Fechar à força para zerar a classe esconderia essa pendência. `hasner.txt:66`, `hasner.txt:113`, `app/chamados/models.py:393`, `app/chamados/models.py:451`, `app/chamados/models.py:147`.

**Dono, sítio e padrão para ambas.** Uma falha confirmada da cascata é **ESTRUTURA**, com um sítio, `executar_desligamento`. O BACKLOG marca **C7-DISPUTA-FECHA-POR-UM-JUIZ** `EM CURSO` e nomeia esse sítio; a chamada à porta C7 já está na cópia. A fatia específica **DESLIGAR-LIQUIDA-TUDO**, O50(d), consta com despacho de censo e sonda. O padrão é escritor único. Falta `C5|chamado|s89|antes/depois|C7/perguntas/resultado|ENCERRAR/plano/reatas/resultado|trilha_de_estado_e_recusa_ate_hoje|justificativa|ator/canal`. A frase de fechamento do S89 é fixa e a causa histórica fica [nao verificado]. `app/docs/LEIS.md:122`, `app/docs/BACKLOG.md:732`, `app/docs/BACKLOG.md:1466`, `app/docs/BACKLOG.md:1632`, `app/docs/GLOSSARIO.md:12`, `app/colaboradores/services/desligamento.py:199`.

## AVISO — `tipo_marco_divergente` — 1.077

`_tipo_pelo_marco` compara o tipo E/S do marco cobrado com o tipo do marco da ata mais próximo da hora respondida, dentro da tolerância. Havendo os dois tipos e divergência, registra o aviso e devolve o tipo cobrado. O derivador descarta o booleano `_div` e usa o tipo escolhido. A S1 conta emissões de log, portanto 1.077 é o número de ocorrências, sem deduplicação por pergunta. `hasner.txt:79`, `app/chamados/services/materializacao.py:539`, `app/chamados/services/materializacao.py:567`, `app/chamados/services/materializacao.py:595`, `app/chamados/services/materializacao.py:683`, `app/core/observ.py:73`.

O aviso em si só incrementa observabilidade. O **tipo escolhido** pode alimentar o slot do C6 e a aplicabilidade de uma resposta, alcançando a leitura de portas/guardas em C1, C2, C4 e C5. Em C3, esse caminho pode alcançar a guarda seca, enquanto os dois vereditos de premissa vêm do turno ou dos filhos. A interseção dos avisos com qualquer subclasse e uma eventual mudança de classificação ficam [nao verificado]. Falta `AVISO_CASO|classe/etapa|colab/dia|chamado/disputa/pergunta|tipo_cobrado/ata/usado|resultado_C6/aplicabilidade/porta`. Para provar efeito na classe, falta também o resultado com o tipo confirmado pela autoridade do caso. `app/chamados/juizes.py:404`, `app/chamados/juizes.py:802`, `app/chamados/juizes.py:1587`, `app/chamados/services/materializacao.py:880`, `app/chamados/reconciliador.py:287`, `app/chamados/models.py:708`, `hasner.txt:79`.

## 27/11 20:36 · sessao 0483e8190b85 · - · - · origem=Ronald (tmux)

**C2 — 38 chamados, 10 colaboradores.** A unidade é o chamado com pelo menos um par testemunha. A S1 registrou 2.203 pares visíveis sem ação para o gestor amostrado, e nenhum par ausente de todas as superfícies testadas ou só na gaveta. O anexo traz os totais, sem as linhas dos 38. `hasner.txt:47–53`

- **Escrita e leitura.** `ChamadoColaborador.abrir` cria o chamado e `_gravar_estado` escreve o status. `PerguntaDisputa._nascer` grava o vínculo, inclusive ao reaproveitar pergunta, e `fio.reapontar_resolvedora` move a âncora. Para reconciliar, o modelo consulta motor, premissa, conversa do pai e vivas ancoradas. A guarda de vivas libera o fechamento se todas puderem ser reatadas. A guarda de conversa protege a resposta sem veredito do pai ou destino. `app/chamados/models.py:213–241,683–716,718–759,899,1161,3297–3336`; `app/chamados/services/fio.py:964,1085`; `app/ponto/supra_juiz.py:561–575`
- **Divergência.** A S1 reúne leitores reais, mas reconstrói o resultado. Usa `_vivas_ancoradas("fechado")` sem o plano de reata, soma respostas da guarda do pai, exige C6 também nessas respostas e exclui uma disputa própria aberta antes de contar. `reconciliar` usa o destino `resolvido` e chama as guardas em sequência. `encerrar`, com destino `fechado`, chama a guarda de vivas, sem chamar a de conversa. A S1 renderiza contextos reais sob um gestor, testa só três verbos e infere o login do colaborador. Uma pergunta reatável, uma ação disponível a outro gestor autorizado ou uma conferência de data utilizável podem produzir falso positivo. A montagem parcial real oferece conferência de data, que a S1 remove da renderização. `app/chamados/models.py:332–347,393–450,683–716,1779–1794`; `app/chamados/catalogo/motor.py:109–115`; `app/chamados/utils.py:47–83`; `app/chamados/services/fio.py:607–610`; `app/templates/chamados/partials/modal_fio.html:162–181`; `app/chamados/views.py:1288–1310`; `app/api/login_helpers.py:107–149`
- **Dono e controle.** ESTRUTURA quando uma ação devida ao ator autorizado fica inalcançável pelo desencontro entre a âncora da tela e a da porta. O sítio desse defeito é `fio.chamado_que_lista`. `E4C-F5-82-FALTA` está aberta e toca `fio.py`, com porta × cartão por dono no escopo. Padrões **fonte única** e **Policy Enforcement Point**. Dono e causa caso a caso [nao verificado]. Forçar o fechamento esconderia uma resposta real aguardando decisão humana ou deixaria uma pergunta viva sem âncora. `app/docs/LEIS.md:122`; `app/docs/GLOSSARIO.md:11–12,33`; `app/docs/BACKLOG.md:2059`; `app/chamados/services/fio.py:97–117`; `app/chamados/models.py:429–450,735–759`
- **Linha de sonda faltante.** Uma por chamado/pergunta, com `ancora`, `dia_fato`, `estado`, `C6`, `CSF`, destino e resultado de `reconciliaria` e da reata, área do chamado e portas elegíveis por perfil, conferência de data, app/PWA, login e código de recusa. `hasner.txt:47–53`; `app/chamados/models.py:393–450,677–716`; `app/chamados/utils.py:47–83`

**C4 — oito disputas.** A S1 exige disputa aberta com mais de 30 dias e marca se uma pergunta do universo C6 ou C7 ficar sem porta medida. Uma disputa com qualquer data na janela traz todas as suas perguntas para a análise e uma disputa sem data também pode entrar. Em d1398, P60396 ficou sem porta medida, ao lado de portas de veredito e resposta de outras perguntas. `hasner.txt:60–63,98`; `app/chamados/models.py:2000–2053`

- **Escrita e leitura.** `_abrir_ou_agregar_disputa` abre, `_nascer` grava a pergunta, `responder_pergunta_disputa_core` grava a fala e `validar_pergunta` escreve o veredito. `DisputaSupervisao.pode_fechar` pede zero C6 vivas e zero C7 respostas sem veredito. `fechar` faz a guarda sob trava e grava o fechamento. O explicador do modal lê o mesmo C7. `app/chamados/services/disputa_emissao.py:483,582–606,1471,1529–1567`; `app/chamados/models.py:3330–3336,2047–2053,2122–2146,2163–2177,2199–2203`; `app/chamados/services/validacao.py:697,742–787`; `app/chamados/services/explicador.py:179–186`
- **Divergência e dono.** C6 e a lista C7 são o código real. A idade e o teste de alcançabilidade são da S1, com as mesmas lacunas do C2. O explicador também descreve correção de cadastro, batida ou pendência administrativa, saídas fora dos três verbos medidos. O C7 está barrando por fundamento em todas as marcações da S1, logo ausência de um evento de fechamento, por si, não explica a classe. ESTRUTURA só se comprovada incoerência da porta devida, no sítio de publicação da âncora em `fio.py` coberto pela F5 8.2. **Aggregate Root/fonte única** é o padrão de `RAIZ-DA-DISPUTA`, ABERTA, para defeito demonstrado no ciclo dos filhos e do agregado. A sonda não localiza esse defeito nos oito [nao verificado]. `app/chamados/models.py:2047–2053`; `app/chamados/services/explicador.py:98–150`; `app/chamados/services/fio.py:97–117,228`; `app/docs/LEIS.md:122`; `app/docs/GLOSSARIO.md:11–12,26`; `app/docs/BACKLOG.md:2059,2326`
- **Controle e linha faltante.** Em d1362, P39872 está tanto em `sem_porta` quanto em `c7_sem_veredito`. Fechar por idade ocultaria uma resposta que a porta preserva. Na mesma disputa, P53621 reaparece no C5 sem login elegível no teste, com saída em app/web antes desse filtro, evidenciando por que convém ler pergunta a pergunta. Falta linha com `disputa`, `pergunta`, `dia_fato`, C6, C7, motivo, estado aplicado, portas por ator e resultado e insumos da L-099, sem truncar. `hasner.txt:97,116`; `app/chamados/models.py:2122–2146`; `app/docs/BACKLOG.md:1632`; `app/ponto/management/commands/e6_oraculo.py:98–110`

**Aviso.** `_tipo_pelo_marco` registra a divergência entre tipo do marco cobrado e tipo do marco mais próximo da ata, e devolve o tipo cobrado primeiro. A aplicação usa esse tipo e descarta o booleano de divergência. A ocorrência de log não muda a classe. A seleção de tipo entra no slot do C6 e na aplicabilidade que a S1 usa para avaliar portas, de modo que associação e efeito em C2/C4 ficam [nao verificado]. O total 1.077 é de ocorrências, sem chaves de pergunta ou classe no anexo. Falta detalhe `etapa, pergunta, chamado, disputa, tipo_cobrado, tipo_ata, tipo_usado, C6, estado_aplicacao, classe, sub`, sem texto de resposta. `hasner.txt:79`; `app/chamados/services/materializacao.py:567–603,683–721,908–973`; `app/chamados/juizes.py:404–418,802–819,1580–1589`; `app/core/observ.py:73–82`

## 27/11 20:35 · sessao 5ce08b334a9d · - · - · origem=Ronald (tmux)

### C1

- **Escritor e leitores comuns.** O cartório usa `supra_juiz.classificar_dia`, depois chama a porta de lavra de `CelulaDia`, que grava protestos, veredito, via e instante. O retentor real lê o veredito gravado e mantém as exceções de protestos não cobráveis e furo só de pausa. A tarefa deriva o fio e publica sua porta. O vigor de um dia na lista do admin vem do dono lavrado naquela lista. `dono` no retrato da folha é o fio, distinto do dono L-099. `app/ponto/services/cartorio.py:1505`, `:1908`; `app/escala/models.py:1622`, `:1832`; `app/ponto/portas/celula.py:1018`, `:1069`, `:1129`; `app/folha/export.py:1268`, `:1282`, `:1302`; `app/chamados/services/tarefa_do_dia.py:724`, `:766`, `:805`, `:809`; `app/chamados/services/vigia_fio.py:202`; `app/folha/retrato_folha.py:1246`.
- **`hasner_sem_saida`.** Os 18 dias e 9 colabs são do anexo, 17 marcados e 1 de férias [nao verificado fora do anexo]. A S1 usa o universo real de `dias_que_seguram` e o juiz da tarefa, depois reconstrói a ausência de saída. Aceita rota só com fio admin/colab, enquanto `censo_alcance` aceita qualquer rota, inclusive `chamado`. Soma calendário, destinos do vigia, mesa e Pauta DP por fora. Na produção o calendário só entra na tarefa para dia inicialmente sem rota, e o vigia consulta o destino fora do fio depois da rota. Col407/673 trazem `alcance=[nao verificado]`: abstenção entra no total S1. Chamado `-` é ausência na fila publicada que a S1 consultou, não prova de inexistência. `app/folha/fora_da_folha.py:125`, `:215`; `app/chamados/services/tarefa_do_dia.py:661`, `:692`, `:809`; `app/chamados/services/vigia_fio.py:642`, `:650`, `:656`.
- **Donos dos 17 dias marcados sem saída.** Os 11 `PREVISTO_COM_DIA_INDEFINIDO` e 3 `PARCIAL_SEM_PERIODO` têm causa de CADASTRO segundo o classificador. O parcial sem período vai para a mesa do admin preservando o dia e sem criar cobrança. Um dia fora da lista pode ser lacuna separada de encaminhamento ESTRUTURA. A classe D do vigia exige nenhuma pergunta viva, ausência do dia na lista e presença do colab nela. A obra aberta `CLASSE-D-NA-FRENTE-DA-E4D` cobre esse ramo; pertença dos dias S1 [nao verificado]. O C11 da lista também exige suas condições de folga, perguntas e âncoras; o protesto sozinho não o aciona. `app/ponto/supra_juiz.py:38`, `:398`, `:453`; `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:2`, `:139`; `app/chamados/services/vigia_fio.py:680`; `app/docs/BACKLOG.md:2239`; `app/escala/services/cadastro_realidade.py:500`, `:582`.
- **Férias com batida.** A produção cruza gozo agendado e batida apurável em `dias_ferias_com_batida`, depois retém pelo risco de duplicar o pagamento no TXT mensal e no módulo de férias. Agendamento e batida têm suas próprias portas de origem. A S1 só usa `tipo_da_linha` e `PORTA_DO_TIPO`, cujo `ferias` é `None`; `portas_da_linha` da produção também consulta o fio, e a linha de férias entra no retrato com fio admin. Assim a falta de link específico para férias não prova que o DP está sem destino. Estado e causa de cada registro, dono L-099 e cobertura de obra [nao verificado]. Liberar o pagamento mensal apesar do gozo válido é o contraexemplo. `app/folha/export.py:464`, `:499`, `:885`; `app/ferias/services.py:273`; `app/ponto/registro_batida.py:118`; `app/folha/retrato_folha.py:372`, `:989`, `:1003`, `:1066`, `:1248`.
- **`cobranca_indevida`.** Sete ocorrências sobrepostas às outras marcas. No anexo, cinco têm `validar`, col246 tem mesa CxR e col296 fica sem saída [nao verificado fora do anexo]. A marca lê o veredito real, cuja fonte é `COBRANCA_SEM_FURO` e `COBRANCA_EM_DIA_SEM_TRABALHO`. O primeiro exige todos os marcos/células casados, realizado ≥ previsto e chamado vivo protestando. A contradição de manter cobrança sem lastro é ESTRUTURA. O sítio para o resíduo é a porta de reconciliação que o cartório invoca, e `E4B-SCA-25-COBRANCA-INDEVIDA` está aberta e cita o col246 01/11. Separar a causa pela qual cada caso atual ainda acusa [nao verificado]. Contraexemplo: fechar por veredito de cobrança indevida e atropelar resposta legítima pendente de validação. `app/ponto/supra_juiz.py:475`, `:507`; `app/escala/models.py:1624`; `app/ponto/services/cartorio.py:1477`, `:1618`, `:1626`; `app/chamados/reconciliador.py:1112`; `app/docs/BACKLOG.md:2318`; `app/docs/GLOSSARIO.md:13`.
- **`discordante`.** As 12 marcas têm porta no anexo, 1 `decidir`, 7 `resolver_dia` e 4 `porta_app` [nao verificado fora do anexo]. A marca relê o veredito real da lavra pelos protestos de realizado zero com turno, inflado ou batida órfã fora da tolerância. O leitor real comparável da O320 pede `dias_sem_fio` e `InsumosDoDono.dono`; a marca sozinha não pede nenhum dos dois. Dono e sítio de cura por dia [nao verificado] sem paridade, batida fora de turno, vínculo/folga/fase e CxR lavrada ou do motor. Contraexemplo: silenciar batida real órfã com realizado zero esconderia subapuração. `app/ponto/supra_juiz.py:38`, `:294`, `:513`, `:527`; `app/ponto/management/commands/pautas_discordantes.py:56`, `:118`; `app/ponto/services/pautas_dos_discordantes.py:46`; `app/ponto/management/commands/e6_oraculo.py:98`, `:182`; `app/docs/BACKLOG.md:1759`; `app/docs/GLOSSARIO.md:11`.
- **`fio_ninguem_nao_sei`.** A S1 junta o fio real em dois estados e contabiliza 21, dos quais 4 têm saída pela mesa do admin [nao verificado fora do anexo]. O leitor da folha distingue `nada` de `nao_sei`. No segundo, a leitura falha e o dono da falha é ESTRUTURA. O `FURO_PARCIAL` subjacente tem dono [nao verificado]. A razão `passivo_sem_linha` nos dois dias aponta para a trilha dos atos de `DecisaoPergunta`, cujo escritor é `registrar`. A origem histórica do passivo e uma obra em voo que repõe esses atos [nao verificado]. E4C-F8 está aberta para migração de leitores da projeção, sem prova de que reponha atos desses casos. O corte F5 preserva `passivo_sem_linha` pendente. Os quatro encaminhados à mesa podem ser BATIDA ou CADASTRO, e a S1 descarta o papel devolvido. Contraexemplo: tratar falta de trilha como ausência de resposta e voltar a cobrar uma pergunta com ato humano já praticado. `app/folha/fora_da_folha.py:44`, `:57`, `:86`; `app/chamados/services/tarefa_do_dia.py:154`, `:340`, `:429`; `app/chamados/models.py:4416`, `:4590`; `app/chamados/services/vigia_fio.py:202`, `:218`, `:252`; `app/docs/BACKLOG.md:2211`; `app/docs/CORTES.md:606`; `app/docs/GLOSSARIO.md:12`, `:19`, `:34`.

Linha C1 faltante por dia: `celula_id|veredito_em|dono_L099|insumos_dono|chamados_vivos_ids_e_guardas|perguntas_C6_ids_e_ancoras|tarefa_acoes_completas|ato_e_projecao|colab_na_lista|dia_na_lista_dono_e_assinatura|destino_DP|porta_real`. Para o dia de férias a S1 não executa `imprimir` sobre o resto; falta a linha nominal com dia, agendamento/estado, batida/origem e destinos.

### Aviso

`_tipo_pelo_marco` compara E/S do `marcos_faltantes` do destino, selecionado pelo motivo, com E/S do marco mais próximo da hora respondida na ata dentro da tolerância. Havendo os dois com diferença, lança registro best-effort em nível ERROR sem lançar a exceção criada, preserva o tipo do chamado e entrega o booleano à `_ts_tipo_do_plantio`, que o descarta. O 1077 da fábrica S1 é contagem de registros, sem deduplicação ou atribuição por classe [nao verificado por caso]. `app/chamados/services/materializacao.py:539`, `:568`, `:573`, `:595`, `:683`, `:700`; `app/core/observ.py:73`.

O registro não altera a classificação diretamente. O **tipo eleito** alimenta estado de aplicação e a leitura de validação C2/C4/C5, com janela, paridade e espera. Também pode ser lido pela vitalidade C6 via `_slot_esperado`, alcançando C1 e guardas auxiliares C3. A causa de premissa `turno_aberto` ou `filhos_vivos` em C3 vem do juiz próprio, não do aviso. Quantas classes mudariam com uma autoridade de tipo corrigida [nao verificado]. Faltaria `S1|AVISO_CASO|classe/fase/caller|colab/dia/chamado/disputa/pergunta|tipo_chamado|tipo_ata|tipo_eleito|estado_aplicacao|C6|resultado_porta`, com o resultado da autoridade válida para o mesmo caso. `app/chamados/services/materializacao.py:927`, `:935`, `:968`; `app/chamados/services/fio.py:461`, `:582`; `app/chamados/juizes.py:408`, `:802`, `:1587`; `app/chamados/reconciliador.py:248`.

## 27/11 20:34 · sessao 24a4ce798286 · - · - · origem=Ronald (tmux)

### C5

- **Escritor e leitor.** `executar_desligamento` grava a situação e desativa a credencial, tenta fechar primeiro cada disputa sem resposta pendente de veredito e depois chama `encerrar` para os chamados. `encerrar` relê o estado sob trava, aplica o motor e o gate de pergunta viva, que aceita o fechamento se a lista estiver vazia ou se todas as perguntas puderem ser reatadas. A gravação do estado passa por `_gravar_estado`. O sinal de fechamento da disputa tenta `reconciliar` no pai antes da etapa `encerrar`, por isso `csf_pai_recusa` no anexo descreve outro caminho. [app/colaboradores/services/desligamento.py:128] [app/colaboradores/services/desligamento.py:164] [app/colaboradores/services/desligamento.py:175] [app/colaboradores/services/desligamento.py:182] [app/chamados/models.py:393] [app/chamados/models.py:429] [app/chamados/models.py:1779] [app/chamados/models.py:213] [app/chamados/signals.py:43]
- **`so_saida_pelo_desligado` — 31 chamados, quatro colabs.** A sonda usa o plano e o gate reais em modo seco. A parte “só saída” é uma composição dos cartões, três verbos de pergunta e uma conta DP, mais a elegibilidade estimada para login novo. A escrita passa pelos POSTs, com autenticação, conferência de vitalidade e validação da resposta. O chamador pode ter outras portas, como decisão na célula ou `admin_encerra` para o silêncio com motivo. A disponibilidade dessas portas nos 31 chamados é [nao verificado]. O rótulo não prova recusa histórica na cascata. `criado=antes` compara com o instante S89, cuja descrição afirma sempre que a cascata fechou. [hasner.txt:67] [hasner.txt:78] [app/chamados/services/fio.py:213] [app/api/login_helpers.py:118] [app/api/views.py:2607] [app/chamados/views.py:1228] [app/chamados/models.py:177] [app/chamados/models.py:2210] [app/colaboradores/services/desligamento.py:195]
- **`sem_guarda` — #36413, col427.** A foto seca mostra `aberto`, criado antes do S89, sem bloqueio de `ENCERRAR` e sem recusa de conversa do pai. O plano também passa quando há perguntas vivas e todas podem ser reatadas. Mesmo com o retorno vazio, a transição humana verifica justificativa pendente e disputa viva, e a execução sob trava pode ter uma foto diferente. Encerrar genericamente pode esconder a justificativa pendente ou perder a âncora de perguntas que precisavam ser reatadas. [hasner.txt:113] [app/chamados/catalogo/motor.py:114] [app/chamados/models.py:140] [app/chamados/models.py:393] [app/chamados/models.py:451] [app/chamados/models.py:761]
- **Dono e origem.** ESTRUTURA para a incoerência de ciclo de vida, com o sítio único `executar_desligamento`. A causa histórica de cada registro é [nao verificado]. `C7-DISPUTA-FECHA-POR-UM-JUIZ` figura `EM CURSO` e toca esse sítio; seu uso da porta `DisputaSupervisao.fechar` está na cópia. A obra específica é `O50-DESLIGAR-ENCERRA-TUDO` (d), `DESLIGAR-LIQUIDA-TUDO`, com despacho de censo e sonda, e a grade registra separadamente o complemento de reabertura. Padrão da fatia (d): **Single Writer Principle**. A O50 usa também o padrão bitemporal para vínculos. O contraexemplo do teste mantém aberto o chamado da resposta sem veredito e encerra o da pergunta muda, preservando a resposta do colaborador segundo a porta da cópia. [app/docs/LEIS.md:122] [app/colaboradores/services/desligamento.py:164] [app/docs/BACKLOG.md:732] [app/docs/BACKLOG.md:733] [app/docs/BACKLOG.md:1466] [app/docs/BACKLOG.md:1632] [app/docs/GLOSSARIO.md:12] [app/docs/GLOSSARIO.md:27] [app/chamados/tests/test_o261e_f1_porta_encerramento.py:327] [nao verificado]
- **Linha faltante.** `C5|chamado|s89|estado_antes/depois|disputa|C7_perguntas_e_resultado|ENCERRAR_perguntas_reatas_e_resultado|eventos_estado/via/recusa_ate_hoje|justificativa|ator/canal_e_motivo_da_recusa`. Isso permite distinguir tentativa recusada, fechamento seguido de reabertura e ausência de tentativa, hoje [nao verificado]. [app/chamados/models.py:304] [app/chamados/models.py:416] [app/chamados/models.py:1428] [app/colaboradores/services/desligamento.py:182]
- **Aviso.** `_tipo_pelo_marco` conta divergência entre o tipo do marco cobrado e o tipo do marco mais próximo na ata; o retorno escolhido pode alimentar a aplicação e o slot usado em ramos do C6. `_div` é descartado pelo chamador. A testemunha de aplicação exige resposta e o slot não usa texto sem resposta vigente, portanto não se atribui o aviso à pergunta muda do C5 pela etiqueta. A ocorrência em outras perguntas consultadas para o mesmo chamado é [nao verificado]. Falta `AVISO_CASO|classe|chamado|pergunta|chamador|tipo_cobrado|tipo_ata|tipo_usado|resultado_relevante`. [hasner.txt:79] [app/chamados/services/materializacao.py:567] [app/chamados/services/materializacao.py:595] [app/chamados/services/materializacao.py:683] [app/chamados/services/materializacao.py:897] [app/chamados/juizes.py:409] [app/chamados/juizes.py:802] [app/chamados/juizes.py:1580]

## 27/11 20:17 · sessao aff0ee44922f · INVENTARIO-PELA-REGRA · leitura · origem=Code

## Bloco 1 — `causas_diff_comp10.py`

A sonda fica no corpo da resposta. Validei a sintaxe em memória e conferi os **57 colaboradores, 60 pares e 1.800 linhas `CZ|DIA`** esperadas. Execução com Django e PostgreSQL, conteúdo de `Sindicato.obs` e hash Git da cópia [nao verificado]. (`amostra.txt:1–60`, anexo)

`CZ|RUB` mostra o FM gravado, o cálculo em memória e a projeção com as regras de evento do exportador. `CZ|BASE` abre as parcelas que ajudam a localizar a diferença. `CZ|8792` mostra quantas vezes a data entra na lista real, inclusive quando há mais de um registro. A captura diária depende do ponto em que esta cópia publica o fechamento em memória e interrompe a execução se esse ponto mudar. (`app/folha/export.py:159–185`, `app/folha/export.py:402–452`, `app/ponto/services/fechamento.py:1139–1159`)

```python
# causas_diff_comp10.py
# Executar a partir de app/, no ambiente Django da sombra:
# python3 -B - < /caminho/causas_diff_comp10.py

import logging
import os
import re
import sys
from collections import Counter, defaultdict
from datetime import date, datetime, time, timedelta
from linecache import getline
from types import SimpleNamespace
from unittest.mock import patch

MES, ANO = 10, 2026
INI, FIM = date(2026, 9, 21), date(2026, 10, 20)
EMPRESAS = (2, 3, 4)

# amostra.txt:1-60 (anexo)
AMOSTRA = {
    "0025": (300, 128, 146, 788, 980, 865, 348, 85, 612, 76),
    "0150": (263, 155, 129, 297, 923, 988, 879, 899, 915, 82),
    "0200": (953, 949, 696, 608, 521, 490, 302, 274, 272, 226),
    "0243": (186, 263, 60, 922, 835, 300, 82, 252, 960, 736),
    "8069": (87, 820, 42, 735, 368, 634, 821, 926, 924, 655),
    "8792": (674, 717, 650, 607, 534, 391, 271, 210, 646, 942),
}
PARES = tuple((rub, pk) for rub, pks in AMOSTRA.items() for pk in pks)
DIAS = tuple(INI + timedelta(days=i) for i in range((FIM - INI).days + 1))
OBS = re.compile(r"feriad|12\s*[x×]\s*36|\b38\b|dobr|s[uú]mula\s+444|hora\s+noturna", re.I)


def cz(tipo, *posicionais, **campos):
    return "|".join(("CZ", tipo, *map(str, posicionais),
                     *(f"{chave}={valor}" for chave, valor in campos.items())))


def leitura_diaria(fc, emp, ids):
    # app/ponto/services/fechamento.py:468-491, 1139-1159, 1215.
    alvo = fc.recalcular_fechamento_mes.__code__
    linha = 1159
    if not getline(alvo.co_filename, linha).lstrip().startswith("leitura[colab.pk] ="):
        raise SystemExit("PAREI: ponto de coleta mudou")
    apurados = {}
    furos = {}

    def observar(frame, evento, argumento):
        if evento == "exception" and not isinstance(argumento[1], fc._GradeSemCelula):
            raise SystemExit(f"PAREI: fechamento emp={emp} linha={frame.f_lineno}")
        if evento == "line" and frame.f_lineno == linha:
            v = frame.f_locals
            pk = v["colab"].pk
            apurados[pk] = (
                v["_ap"], v["_g"], v["feriados"], v["batidas"],
                v["_ft_feriado"], v["_ft_certa"], v["_ft_sem_escala"],
                v["_atrasos_dia"], v["_creditos_dia"],
            )
        return observar

    def selecionar(frame, evento, argumento):
        return observar if frame.f_code is alvo else None

    anterior = sys.gettrace()
    sys.settrace(selecionar)
    try:
        fechamentos = fc.recalcular_fechamento_mes(
            MES, ANO, empresa_id=emp, colaborador_ids=ids,
            somente_leitura=True, lavrar_dias_pagos=False,
            retornar_objeto=True, coletar_furos=furos,
        )
    finally:
        sys.settrace(anterior)
    if set(fechamentos) != set(apurados) or set(fechamentos) != set(furos):
        raise SystemExit(f"PAREI: coleta inconsistente emp={emp}")
    return fechamentos, apurados, furos


def dias_do_motor(dados, abater, dia_de):
    ap, _, _, _, ft_feriado, ft_certa, ft_sem_escala, atrasos, creditos = dados
    res = ap.resultado
    mapa_dia = getattr(res, "dia_da_jornada", None)
    campos = defaultdict(Counter)
    periodos = defaultdict(list)

    for p in (*res.periodos, *res.periodos_ft):
        d = dia_de(p, mapa_dia)
        a = campos[d]
        # app/ponto/motor_calculo_v2.py:208-252, 303-321, 363-370.
        a["noturno"] += p.minutos_noturnos_legais or 0
        a["noturno_relogio"] += p.minutos_noturnos_relogio or 0
        a["intra"] += p.minutos_intrajornada_indenizada or 0
        a["pausa"] += p.intra_descontada or 0
        a["trabalhado"] += p.minutos_trabalhados or 0
        a["fora_janela"] += p.minutos_fora_janela or 0
        periodos[d].append(p)

    for p in res.periodos:
        d = dia_de(p, mapa_dia)
        # app/ponto/services/dia_pago.py:173-186.
        campos[d]["he50"] += p.minutos_extra_50 or 0
        campos[d]["he100"] += p.minutos_extra_100 or 0
        campos[d]["he100_feriado"] += p.minutos_extra_100_feriado or 0
        campos[d]["saida_ant"] += p.minutos_saida_antecipada or 0

    for p in ft_feriado:
        d = dia_de(p, mapa_dia)
        # app/ponto/services/fechamento.py:968-981.
        campos[d]["he100"] += p.minutos_trabalhados
        campos[d]["he100_feriado"] += p.minutos_trabalhados
    for p in ft_certa:
        # app/ponto/services/fechamento.py:989-990.
        campos[dia_de(p, mapa_dia)]["folga100"] += p.minutos_trabalhados
    for p in ft_sem_escala:
        # app/ponto/services/fechamento.py:1004-1009.
        campos[dia_de(p, mapa_dia)]["folga_sem_escala"] += p.minutos_trabalhados

    for d, bruto in atrasos.items():
        # app/ponto/services/fechamento.py:1018-1026.
        liquido, usado = abater({d: bruto}, creditos)
        campos[d]["atraso"] = liquido
        campos[d]["credito"] = usado
    return campos, periodos


def previsto(grade):
    # app/escala/services/leitor_celula.py:114-149, 418-424.
    marcos = grade.get("marcos") or ()
    if not marcos or marcos[0]["tipo"] != "E" or marcos[-1]["tipo"] != "S":
        return "-"
    inicio, fim = marcos[0]["hora"], marcos[-1]["hora"]
    return f"{inicio[:5]}-{fim[:5]}" if inicio and fim else "-"


def total_rubrica(ex, mapa, rub, fm, **regra):
    if fm is None:
        return "-"
    # app/folha/export.py:402-452, 739-748, 993-1000.
    return round(sum(valor for ev, valor in ex.eventos_do_fechamento(fm, **regra)
                     if ev in mapa and mapa[ev].codigo_fopag.zfill(4) == rub), 2)


def cru(fm, modelo):
    # Projeção usada no anexo sonda.py::evento_fm.
    if fm is None:
        return None
    return SimpleNamespace(**{f.attname: getattr(fm, f.attname)
                              for f in modelo._meta.concrete_fields})


def dados_da_sombra():
    from django.conf import settings
    from django.db import connection, transaction
    from django.utils.timezone import localtime, make_aware
    from django_tenants.utils import schema_context

    db = connection.settings_dict
    if (getattr(settings, "HASNER_SOMBRA", False) is not True
            or (db["NAME"], db["HOST"]) != ("sombra", "sombra_db")):
        raise SystemExit("PAREI: configuração fora da sombra")
    if connection.in_atomic_block or not connection.get_autocommit():
        raise SystemExit("PAREI: a sonda exige transação própria")

    from colaboradores.models import Colaborador, Empresa
    from core.feriados import feriados_do_colab
    from core.models import Sindicato
    from core.regua_cct import get_motor_cct, regua_para
    from escala.alimentacao import vinculo_do_dia_carregado
    from escala.servico_jornada import escala_vigente
    from escala.services.leitor_celula import grade_da_celula
    from folha import export as ex
    from ponto.catalogo.ausencias import descontam_vigentes
    from ponto.models import Ausencia, FechamentoMensal
    from ponto.services import fechamento as fc
    from ponto.services.credito_parcial import abater_no_proprio_dia
    from ponto.services.dia_pago import _dia_de
    from ponto.turnos import batidas_apuraveis, dia_das_batidas

    def hora_local(instante):
        return localtime(instante).strftime("%H:%M") if instante is not None else "-"

    linhas = []
    dias_emitidos = 0
    ids = {pk for _, pk in PARES}
    # app/config/settings/sombra.py:13-14, 47-60.
    with schema_context("juliani"), transaction.atomic():
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            cursor.execute("SELECT current_schema(), current_setting('transaction_read_only')")
            if cursor.fetchone() != ("juliani", "on"):
                raise SystemExit("PAREI: schema ou proteção inválida")

        empresas = Empresa.objects.in_bulk(EMPRESAS)
        if set(empresas) != set(EMPRESAS) or any(
            ex.janela_competencia(e, MES, ANO) != (INI, FIM) for e in empresas.values()
        ):
            raise SystemExit("PAREI: empresas ou janelas diferentes do anexo")
        colabs = Colaborador.objects.select_related("empresa").in_bulk(ids)
        if set(colabs) != ids or any(c.empresa_id not in empresas for c in colabs.values()):
            raise SystemExit("PAREI: amostra indisponível nas empresas da sonda")

        mapas = {emp: ex._rubricas_map(e) for emp, e in empresas.items()}
        gravados = {f.colaborador_id: f for f in FechamentoMensal.objects.filter(
            colaborador_id__in=ids, mes=MES, ano=ANO)}
        ausencias = defaultdict(list)
        # app/ponto/models.py:690-705 e app/folha/export.py:63-70.
        for a in Ausencia.objects.filter(colaborador_id__in=ids, data__lte=FIM).only(
            "colaborador_id", "data", "data_fim", "tipo", "status"
        ).order_by("pk"):
            for d in ex.datas_no_periodo(a, INI, FIM):
                ausencias[a.colaborador_id, d].append((a.tipo, a.status))
        tipos_falta = descontam_vigentes()

        lidos, apurados, furos = {}, {}, {}
        for emp in EMPRESAS:
            grupo = {pk for pk, c in colabs.items() if c.empresa_id == emp}
            esperado = set(fc.universo_da_competencia(MES, ANO, emp)
                           .filter(pk__in=grupo).values_list("pk", flat=True))
            f, a, u = leitura_diaria(fc, emp, sorted(grupo))
            if set(f) != esperado:
                raise SystemExit(f"PAREI: leitura incompleta emp={emp}")
            lidos.update(f)
            apurados.update(a)
            furos.update(u)

        for pk, c in sorted(colabs.items()):
            col = f"col{pk}"
            emp = c.empresa_id  # app/ponto/services/fechamento.py:639-641.
            ciclo = ex._ciclo_do_colab(c, MES, ANO)  # app/folha/export.py:341-360.
            dobra = ex.feriado_12x36_em_dobra(c, MES, ANO)  # app/folha/export.py:363-380.
            regua = regua_para(c, FIM)  # app/core/regua_cct.py:297-380.
            ec = vinculo_do_dia_carregado(c, FIM)
            motor = get_motor_cct(c, FIM, tipo_escala=ec.tipo_escala if ec else None)
            linhas.append(cz(
                "COLAB", col=col, emp=emp, ciclo=ciclo or "-",
                regime=c.empresa.regime_trabalhista or "-",  # app/colaboradores/models.py:146-156.
                fonte_regua=regua["fonte"],  # app/core/regua_cct.py:352-380.
                hora_reduzida_afastada=int(motor.hora_reduzida_afastada),  # app/core/regua_cct.py:441-472.
                feriado_12x36_em_dobra=int(dobra),
                # Valores cadastrados. Efeito em app/core/regua_cct.py:72-79.
                tolerancia_marcacao=regua["tolerancia_marcacao_min"],
                tolerancia_dia=regua["tolerancia_dia_min"],
                intrajornada_min=regua["intrajornada_minima_min"],
            ))

            dados = apurados.get(pk)
            if dados is None:
                grade = grade_da_celula(c, INI, FIM)
                feriados = feriados_do_colab(c, INI, FIM)
                comeco = make_aware(datetime.combine(INI - timedelta(days=1), time.min))
                termino = make_aware(datetime.combine(FIM + timedelta(days=1), time.max))
                batidas = list(batidas_apuraveis(c, comeco, termino))
                campos, periodos, cobertos = {}, {}, set()
            else:
                ap, grade, feriados, batidas, *_ = dados
                campos, periodos = dias_do_motor(dados, abater_no_proprio_dia, _dia_de)
                cobertos = ap.dias_cobertos
            grade_dia = {g["data"]: g for g in grade["dias"]}
            # app/ponto/services/fechamento.py:652-653 e app/ponto/turnos.py:2971-3004.
            dia_batida = dia_das_batidas(c, INI, FIM)
            n_batidas = Counter(dia_batida[b.pk] for b in batidas if b.pk in dia_batida)
            # app/folha/export.py:87-92, 159-185, 749-753.
            faltas = Counter(ex.datas_falta(c, empresas[emp], MES, ANO))
            furo = set(furos.get(pk, ())) if dados is not None else None
            gravado_cru = cru(gravados.get(pk), FechamentoMensal)
            lido_cru = cru(lidos.get(pk), FechamentoMensal)

            for rub, amostrado in PARES:
                if amostrado != pk:
                    continue
                if rub != "8792":
                    linhas.append(cz(
                        "RUB", rub=rub, col=col,
                        fm=total_rubrica(ex, mapas[emp], rub, gravado_cru),
                        lido=total_rubrica(ex, mapas[emp], rub, lido_cru),
                        lido_com_regra_export=total_rubrica(
                            ex, mapas[emp], rub, lidos.get(pk),
                            tipo_ciclo=ciclo, feriado_12x36_em_dobra=dobra),
                        eventos=",".join(ev for ev, r in sorted(mapas[emp].items())
                                         if r.codigo_fopag.zfill(4) == rub) or "-",
                    ))
                for d in DIAS:
                    g = grade_dia[d]
                    tipo = g["tipo_dia"]  # app/escala/services/leitor_celula.py:390-424.
                    aus = ausencias[pk, d]  # app/folha/export.py:63-70.
                    m = campos.get(d, Counter())
                    tem_motor = d in cobertos or d in campos

                    def minutos(campo):
                        return round(m[campo], 4) if tem_motor else "-"

                    # app/folha/export.py:169-185: sim = data na lista real.
                    decidida = ("sim" if faltas[d] else "nao"
                                if tipo in ("trabalho", "ausencia") or aus or (furo and d in furo)
                                else "-")
                    linhas.append(cz(
                        "DIA", rub=rub, col=col, dia=d,
                        celula=tipo,
                        previsto=previsto(g),
                        n_batidas_apuraveis=n_batidas[d],  # app/ponto/turnos.py:2941-2968, 3034-3045.
                        feriado=int(d in feriados),  # app/core/feriados.py:135-167.
                        ausencia=",".join(f"{t}/{s}" for t, s in aus) or "-",
                        noturno_min=minutos("noturno"),  # app/ponto/motor_calculo_v2.py:303-321.
                        intra_min=minutos("intra"),  # app/ponto/motor_calculo_v2.py:363-370.
                        he100_min=minutos("he100"),  # app/ponto/services/fechamento.py:942-943, 978-980.
                        atraso_min=minutos("atraso"),  # app/ponto/services/fechamento.py:1018-1025.
                        saida_ant_min=minutos("saida_ant"),  # app/ponto/services/fechamento.py:1026.
                        falta_decidida=decidida,
                    ))
                    dias_emitidos += 1
                    if rub == "8792":
                        # app/ponto/services/fechamento.py:1139-1151 e app/folha/export.py:87-92, 169-185.
                        linhas.append(cz(
                            "8792", col=col, dia=d, conta=faltas[d],
                            furo_motor="-" if furo is None else int(d in furo),
                            trabalho_sem_batida=int(tipo == "trabalho" and not n_batidas[d]),
                            ausencia_sem_aprovacao=int(any(s != "aprovada" for _, s in aus)),
                            aprovada_sem_desconto=int(any(
                                s == "aprovada" and t not in tipos_falta for t, s in aus)),
                        ))
                    else:
                        ec_fer = (escala_vigente(c, d)
                                  if rub == "0200" and d in feriados and m["folga_sem_escala"]
                                  else None)
                        # app/ponto/services/fechamento.py:300-305, 938-1009, 1018-1026.
                        linhas.append(cz(
                            "BASE", rub=rub, col=col, dia=d,
                            he50_min=minutos("he50"), he100_feriado_min=minutos("he100_feriado"),
                            folga100_min=minutos("folga100"), noturno_relogio_min=minutos("noturno_relogio"),
                            folga_sem_escala_min=minutos("folga_sem_escala"),
                            feriado_vira_folga=int(ec_fer.feriado_vira_folga(d)) if ec_fer else "-",
                            previsto_min=g["minutos_previstos"],
                            pausa_descontada_min=minutos("pausa"), credito_atraso_min=minutos("credito"),
                            trabalhado_min=minutos("trabalhado"), fora_janela_min=minutos("fora_janela"),
                            pares_motor=",".join(
                                f"{hora_local(p.entrada)}-{hora_local(p.saida)}"
                                for p in periodos.get(d, ())) or "-",
                            regua_dia=ap.regua_por_dia.get(d, "-").replace("|", ",") if dados else "-",
                        ))

        # app/core/models.py:34-64, 143-156 e app/core/regua_cct.py:253-263.
        sindicatos = Sindicato.objects.filter(
            aplicacoes__empresa_id__in=EMPRESAS, aplicacoes__ativo=True,
            cct_vigencia_inicio__lte=FIM, cct_vigencia_fim__gte=FIM,
        ).distinct().order_by("pk")
        for s in sindicatos:
            # Rótulo de exibição: Sindicato.nome (app/core/models.py:34-40).
            linhas.append(cz(
                "CCT", f"sind={s.pk}", s.nome,
                feriado_12x36_em_dobra=int(s.feriado_12x36_em_dobra),
                hora_reduzida_afastada_12x36=int(s.hora_reduzida_afastada_12x36),
            ))
            for paragrafo in re.split(r"\r?\n[ \t]*\r?\n(?:[ \t]*\r?\n)*", s.obs):
                if OBS.search(paragrafo):
                    linhas.append(cz("OBS", f"sind={s.pk}", paragrafo))

        linhas.append(cz("UNIVERSO", colabs=len(colabs), pares=len(PARES), dias=dias_emitidos))
        linhas.append(cz("FIM"))
        transaction.set_rollback(True)
    return linhas


def main():
    sys.dont_write_bytecode = True
    # app/ponto/services/fechamento.py:1327-1331.
    logging.disable(logging.CRITICAL)
    sys.excepthook = lambda *_: print("PAREI: falha da sonda", file=sys.stderr)
    if os.environ.setdefault("DJANGO_SETTINGS_MODULE", "config.settings.sombra") != "config.settings.sombra":
        raise SystemExit("PAREI: settings fora da sombra")

    import django
    from django.conf import settings

    def sem_mkdir(caminho, *, exist_ok=False):
        if caminho != "/app/logs" or not exist_ok:
            raise SystemExit("PAREI: inicialização pediu diretório inesperado")

    # Sombra herda o mkdir e os handlers de arquivo de app/config/settings/saas.py:117-183.
    with patch.object(os, "makedirs", sem_mkdir):
        settings.LOGGING_CONFIG = None
        django.setup()
    for linha in dados_da_sombra():
        print(linha)


if __name__ == "__main__":
    main()
```

## Bloco 2 — Leitura da regra e classificação

**Feriado no 12x36 dos vigilantes de Londrina.** A leitura registrada na cópia é que **não há cláusula de dobra**. O comentário do campo é explícito sobre as convenções da casa: “nenhuma das nossas diz isso hoje, e por isso o default e False.” A cláusula 38-d transcrita trata da duração da hora noturna e do adicional. `CLAUDE.md` remete a fundamentação integral para `Sindicato.obs` e registra que o PDF da CCT está fora do sistema. O conteúdo cadastrado de `ob

[truncado em 20000 de 30325 caracteres]

## 27/11 20:06 · sessao 0144c70af86d · FIM-DE-SEMANA-JA · leitura · origem=Code

Segue a S1 em texto. A sonda mantém os recortes da S0, publica os denominadores antes dos subtotais e conta C2 por PK exato da pergunta. Para C2 e C4, `S1_GESTOR_PK` identifica a conta de gestão a medir. `S1_DP_PK` pode indicar outra conta para C5 e, se omitido, usa a mesma conta. A sonda confere as permissões dessas contas no código da aplicação. A execução, os subtotais e a correspondência do snapshot com o hash informado ficam [nao verificado]. `sonda_S0.py:61–267` `app/chamados/views.py:35–38` `app/chamados/utils.py:47–83`

```python
import logging
from collections import defaultdict
from dataclasses import dataclass
from datetime import timedelta
from functools import lru_cache
from html.parser import HTMLParser
from os import environ
from types import SimpleNamespace

from django.conf import settings
from django.db import connection
from django.utils import timezone
from django_tenants.utils import schema_context

# Mesmas guardas de topo da S0.
if getattr(settings, "HASNER_SOMBRA", False) is not True:
    raise SystemExit("PAREI: HASNER_SOMBRA ausente")
db = connection.settings_dict
if (db["NAME"], db["HOST"]) != ("sombra", "sombra_db"):
    raise SystemExit("PAREI: conexao fora da Sombra")
if connection.in_atomic_block or not connection.get_autocommit():
    raise SystemExit("PAREI: a sonda exige transacao propria")

from django.contrib.auth.models import User
from django.template import Context, engines
from django.utils.dateparse import parse_datetime

from api.login_helpers import is_admin_user
from chamados.catalogo.motor import COBRANCA, NAO_FECHADOS, VIVOS, decidir
from chamados.juizes import (
    FIO_SEM_PORTA,
    _dia_de_turno_da_pergunta,
    competencia_do_chamado,
    dia_do_fato,
    pergunta_viva,
    perguntas_vivas_de,
)
from chamados.models import (
    ChamadoColaborador,
    DecisaoPergunta,
    DisputaSupervisao,
    PerguntaDisputa,
    motivo_implausivel_validacao,
)
from chamados.reconciliador import (
    _absolvido_pela_celula,
    _chamados_de_turno_vivos,
    _veredito_premissa,
    eventos_pendentes,
    filtro_evento_morto,
    retencao_da_retratacao,
)
from chamados.services.acoes_chamado import montar_contexto_detalhe
from chamados.services.fio import (
    chamado_que_lista,
    contexto_parcial_fio,
    disputas_do_chamado_em_lote,
    espera_o_texto_do_colaborador,
    montar_chamados_fio,
)
from chamados.services.materializacao import (
    _data_em_disputa,
    estado_aplicacao_pergunta,
    espera_declarado_x_gravado,
)
from chamados.services.painel import qs_na_fila
from chamados.services.pergunta_no_app import consulta_do_app
from chamados.services.tarefa_do_dia import (
    aguarda_admin_pela_tarefa,
    exigencia_das_perguntas,
    porta_da_tarefa,
    tarefa_do_dia,
)
from chamados.services.validacao import (
    _e_aceite_de_folga,
    _resposta_de_hora,
    entrega_que_barra_a_validacao,
    fila_validar_classificada,
)
from chamados.services.verbos import particionar_verbos
from chamados.services.vigia_fio import ListaDoAdmin, dono_fora_do_fio
from chamados.utils import pode_agir_chamado, pode_agir_departamento
from chamados.views import _is_gestor
from chamados.views_cobrar import _pode_validar_lote
from colaboradores.mixins import tem_acao
from colaboradores.models import Empresa, HistoricoVinculo
from colaboradores.services.calendario import resolver_dia_oferecido_nas_datas
from colaboradores.services.desligamento import esta_desligado
from escala.models import CelulaDia, EventoCelula
from folha.fora_da_folha import _carga, _fio_da_porta, dias_que_seguram
from folha.retrato_folha import PORTA_DO_TIPO, _balde, tipo_da_linha
from ponto.janelas import janela_fechamento
from ponto.services.declarar_dia import checar_declaracao, o_dia_ja_e_folga
from ponto.services.lastro import julgar as julgar_lastro, marcos_acesos_do_dia
from ponto.supra_juiz import perguntas_que_seguram_csf

EMPRESA_ID = 2
MES, ANO = 11, 2026
AGORA = timezone.now()
HOJE = timezone.localdate(AGORA)
NV = "[nao verificado]"
S89 = "desligamento_revisao"
AVISOS = defaultdict(int)

VERBOS_DA_PERGUNTA = {
    "validar_pergunta", "recusar_pergunta", "corrigir_resposta",
}


@dataclass
class Linha:
    sub: str
    colab: int
    chamado: int = None
    disputa: int = None
    pergunta: int = None
    dia: object = None
    det: str = ""


def valor(x):
    if x is None:
        return "-"
    return x.isoformat() if hasattr(x, "isoformat") else str(x)


def ids(pks, limite=8):
    pks = sorted({pk for pk in pks if pk is not None})
    texto = ",".join(map(str, pks[:limite])) or "-"
    return texto if len(pks) <= limite else f"{texto},+{len(pks) - limite}"


def dia_da_pergunta(p):
    # Mesmo fallback da S0. app/chamados/juizes.py:58,339
    dia = _dia_de_turno_da_pergunta(p)
    if dia is None and not p.conversa and p.celula_id:
        return p.celula.data
    return dia


def c1(empresa, inicio, fim):
    # app/folha/fora_da_folha.py:61–91,125–174
    # app/folha/export.py:1268–1305
    u = dias_que_seguram(empresa, MES, ANO, hoje=HOJE)
    itens = sorted(
        (i for i in u["itens"] if _balde(i) == "bloqueado"),
        key=lambda i: i["colaborador_id"],
    )
    datas = {
        cid: [r["data"] for r in dias if inicio <= r["data"] <= fim]
        for cid, (_, dias) in u["dias"].items() if dias
    }
    carga = _carga([u["colabs"][cid] for cid in datas], inicio, fim, AGORA) if datas else None
    cel_ids = {
        r["celula_id"] for _, dias in u["dias"].values() for r in dias
        if inicio <= r["data"] <= fim and r["celula_id"] is not None
    }
    cels = CelulaDia.objects.only("protestos", "veredito_via").in_bulk(cel_ids)
    perguntas = defaultdict(list)
    for p in PerguntaDisputa.objects.filter(celula_id__in=cel_ids).select_related(
        "disputa__colaborador__empresa", "celula"
    ).prefetch_related("decisoes"):
        perguntas[p.celula_id].append(p)
    lista = ListaDoAdmin(HOJE, [empresa], agora=AGORA)
    marcadas, resto = [], defaultdict(list)

    for item in itens:
        cid, motivo = item["colaborador_id"], item["motivo"]
        if motivo != "furo_espelho":
            tipo = tipo_da_linha(item)
            porta_tipo = PORTA_DO_TIPO.get(tipo)
            sub = f"producao_com_porta:{tipo}" if porta_tipo else "hasner_sem_saida"
            for dia in item.get("dias_ferias_com_batida") or (None,):
                if dia is None or inicio <= dia <= fim:
                    resto[motivo].append(Linha(sub, cid, dia=dia))
            continue

        colab = u["colabs"][cid]
        _, dias = u["dias"][cid]
        if not dias:
            resto["furo_espelho_sem_dia"].append(Linha("hasner_sem_saida", cid))
        calendario = resolver_dia_oferecido_nas_datas(
            colab, datas.get(cid, ()), agora=AGORA
        )
        for r in dias:
            dia, cel_id = r["data"], r["celula_id"]
            if not inicio <= dia <= fim:
                continue
            tarefa = tarefa_do_dia(colab, dia, AGORA, carga=carga)
            porta = porta_da_tarefa(tarefa)
            fio = _fio_da_porta(porta)
            porta = porta or {}
            rota = porta.get("rota")
            acao = porta.get("acao")
            fila = porta.get("fila") or ()
            fora, _ = dono_fora_do_fio(
                colab, dia, perguntas[cel_id], lista, com_papel=True
            )
            na_lista = lista.dia_na_lista(cid, dia.isoformat())
            mesa = lista.dia_do_admin(cid, dia.isoformat())
            pauta = bool(lista.destino_fora_da_mesa(cid, dia.isoformat()))
            cal = calendario[dia]

            # Prioridade de apresentação da S1. As saídas concorrentes vão em det.
            publicadas = [] [nao verificado]
            if rota and fio in ("admin", "colab"):
                publicadas.append(acao or rota) [nao verificado]
            if cal is True:
                publicadas.append("calendario") [nao verificado]
            if fora:
                publicadas.append(fora) [nao verificado]
            if mesa:
                publicadas.append("cadastro_x_realidade") [nao verificado]
            if pauta:
                publicadas.append("pauta_dp") [nao verificado]
            sub = f"producao_com_porta:{publicadas[0]}" if publicadas else "hasner_sem_saida" [nao verificado]

            peso = []
            if fio in (FIO_SEM_PORTA, "nao_sei"):
                peso.append("fio_ninguem_nao_sei")
            if r["veredito"] in ("discordante", "cobranca_indevida"):
                peso.append(r["veredito"])

            cel = cels.get(cel_id)
            protestos = ",".join(sorted(cel.protestos or ())) if cel else ""
            razoes = sorted({tarefa["razao"], *(i.get("razao") for i in tarefa["instancias"])}
                            - {None, ""})
            incerto = (
                fio == "nao_sei" or cal is None or na_lista is None
                or (rota and fio not in ("admin", "colab"))
            )
            linha = Linha(
                sub, cid, next(iter(fila), None), dia=dia,
                det=(
                    f"aval={','.join(peso) or '-'} motivo={r['motivo']} "
                    f"protestos={protestos or '-'} via={valor(cel.veredito_via if cel else None)} "
                    f"fio={'ninguem' if fio == FIO_SEM_PORTA else fio} "
                    f"razao={','.join(razoes) or '-'} rota={valor(rota)} acao={valor(acao)} "
                    f"calendario={NV if cal is None else int(cal)} "
                    f"lista_admin={int(mesa)} cadastro_x_realidade="
                    f"{NV if na_lista is None else int(na_lista[0])} "
                    f"fora_fio={valor(fora)} pauta_dp={int(pauta)} "
                    f"saidas={','.join(dict.fromkeys(publicadas)) or '-'}" [nao verificado]
                    + (f" alcance={NV}" if incerto else "")
                ),
            )
            if peso:
                marcadas.append((linha, peso))
            else:
                resto[r["motivo"]].append(linha)
    return marcadas, resto


class CartoesHTML(HTMLParser):
    """Extrai somente IDs e verbos de formulários. O HTML fica em memória."""

    def __init__(self, html):
        super().__init__()
        self.fio = defaultdict(set)
        self.gaveta = defaultdict(set)
        self.locais = set()
        self.forms = []
        self.form = None
        self.feed(html)
        self.close()

    def handle_starttag(self, tag, attrs):
        a = dict(attrs)
        if "data-s1-fio" in a:
            self.fio[int(a["data-s1-fio"])].add(int(a["data-s1-ch"]))
        if "data-s1-gaveta" in a:
            self.gaveta[int(a["data-s1-gaveta"])].add(int(a["data-s1-ch"]))
        prefixo, _, ref = (a.get("id") or "").partition("-")
        if prefixo == "pergunta" and ref.isdecimal():
            self.locais.add(int(ref))
        if tag == "form":
            self.form = {}
        elif tag == "input" and self.form is not None and a.get("type") == "hidden":
            if a.get("name") in {"acao", "chamado_id", "pergunta_id", "confirmar_implausivel"}:
                self.form[a["name"]] = a.get("value", "")

    def handle_endtag(self, tag):
        if tag == "form" and self.form is not None:
            if self.form.get("acao") in VERBOS_DA_PERGUNTA:
                self.forms.append(self.form)
            self.form = None


@lru_cache(maxsize=1)
def templates():
    e = engines["django"].engine
    fonte = e.get_template("chamados/partials/modal_fio.html").source
    # Marcadores dentro das ramificações reais. app/templates/chamados/partials/modal_fio.html:157–160,468–476
    for alvo, marcador in (
        ("{{ perg.pergunta }}</div>",
         '<span data-s1-fio="{{ perg.pk }}" data-s1-ch="{{ chamado.pk }}"></span>'),
        ("{{ it.dia }}</span>",
         '<span data-s1-gaveta="{{ it.pergunta_id }}" data-s1-ch="{{ chamado.pk }}"></span>'),
    ):
        if fonte.count(alvo) != 1:
            raise SystemExit("PAREI: marcador de template mudou")
        fonte = fonte.replace(alvo, marcador + alvo, 1)
    return e.from_string(fonte), e.get_template("chamados/partials/_pergunta_card.html")


@lru_cache(maxsize=None)
def chamado_de(pk):
    return ChamadoColaborador.objects.select_related("colaborador__empresa").filter(pk=pk).first()


def validar_em_leitura(p, confirma):
    # Testemunhas da porta. app/chamados/services/validacao.py:218–234,755–784
    if not p.respondida or DecisaoPergunta.composta(p).validada_em is not None:
        return False
    if not confirma and motivo_implausivel_validacao(p):
        return False
    if _e_aceite_de_folga(p):
        dia, colab = p.data_evento, p.disputa.colaborador
        return bool(dia and (
            o_dia_ja_e_folga(colab, dia)
            or checar_declaracao(colaborador=colab, dias=[dia], tipo="folga")["aceitos"]
        ))
    if entrega_que_barra_a_validacao(p):
        return False
    if _resposta_de_hora(p):
        if espera_declarado_x_gravado(p):
            return False
        aplicacao = estado_aplicacao_pergunta(p.disputa, p) or {}
        return aplicacao.get("estado") in ("aplicavel", "ja_satisfeita", "aplicada") [nao verificado]
    if p.tipo_resposta == "data_hora" and _data_em_disputa(p):
        return False
    return True


class PortasGestor:
    def __init__(self, user, verbos):
        self.user = user
        self.req = SimpleNamespace(user=user, GET={})
        self.pode_inline = tem_acao(user, "gerir_chamados")
        self.verbos = verbos
        self.lote = (
            set(fila_validar_classificada()["refs_lote"])
            if _pode_validar_lote(self.req) is None else set()
        )

    @lru_cache(maxsize=None)
    def modal(self, colab):
        # GET real marca lidas. Usar o construtor de leitura.
        # app/chamados/views.py:870–890,928–949
        ctx = contexto_parcial_fio(self.req, colab)
        ctx.pop("pode_conferir_data", None)
        ctx["cor_disputas_map"] = DisputaSupervisao.cor_map_por_colaborador(colab)
        tela = CartoesHTML(templates()[0].render(Context(ctx, use_l10n=False)))
        cards = {ch.pk: ch for ch in ctx["chamados"]}
        eleitas = disputas_do_chamado_em_lote(cards)
        forms = defaultdict(list)
        for f in tela.forms:
            pid, cid = int(f["pergunta_id"]), int(f["chamado_id"])
            forms[pid].append((cards[cid], eleitas.get(cid), f))
        return tela, forms

    @lru_cache(maxsize=None)
    def local(self, pk):
        # app/chamados/views.py:359–372,550–552
        ch = chamado_de(pk)
        if ch is None or ch.ticket_id_glpi is not None:
            return set(), defaultdict(list)
        ctx = montar_contexto_detalhe(self.req, ch, True, ())
        visiveis, forms = set(), defaultdict(list)
        for p in ctx["perguntas_disputa"]:
            html = templates()[1].render(Context({**ctx, "perg": p}, use_l10n=False))
            tela = CartoesHTML(html)
            visiveis.update(tela.locais)
            for f in tela.forms:
                forms[int(f["pergunta_id"])].append(f)
        return visiveis, forms

    @lru_cache(maxsize=None)
    def inline(self, pk):
        ch = chamado_de(pk)
        if not (self.pode_inline and self.verbos.get(pk) == "validar"
                and pode_agir_chamado(self.user, ch)):
            return set()
        d = disputas_do_chamado_em_lote([pk]).get(pk)
        if d is None or d.colaborador_id != ch.colaborador_id:
            return set()
        return {p.pk for p in perguntas_vivas_de(d) if p.respondida}

    def consultar(self, p, ch=None):
        pid, d = p.pk, p.disputa
        ancora = chamado_que_lista(p)
        card_ids = {d.chamado_id, ancora, ch.pk if ch else None} - {None}
        cards = [chamado_de(pk) for pk in sorted(card_ids)]
        colabs = {d.colaborador_id: d.colaborador}
        for card in cards:
            colabs[card.colaborador_id] = card.colaborador

        local, fio, gaveta, glpi_local, acoes = set(), set(), set(), set(), set()
        glpi_remoto = False
        for card in cards:
            if card.ticket_id_glpi is not None:
                # O roteador primeiro testa colisão com PK local. app/chamados/views.py:219–222
                alvo = chamado_de(card.ticket_id_glpi)
                if alvo is not None and alvo.ticket_id_glpi is None:
                    if pid in self.local(alvo.pk)[0]:
                        glpi_local.add(alvo.pk)
                    card_ids.add(alvo.pk)
                else:
                    glpi_remoto = True

        def acao_do_form(f, card, canal, eleita=None):
            acao = f["acao"]
            if card.pk != ancora or not pode_agir_chamado(self.user, card):
                return
            if canal == "local" and not d.aberta:
                return
            if acao == "validar_pergunta":
                if canal == "fio" and (
                    card.colaborador_id != d.colaborador_id
                    or (not d.aberta and not pergunta_viva(p, hoje=HOJE))
                ):
                    return
                aceito = validar_em_leitura(p, f.get("confirmar_implausivel") == "1")
            else:
                if not d.aberta or (canal == "fio" and eleita is None):
                    return
                leitura = DecisaoPergunta.composta(p)
                aceito = leitura.validada_em is None
                if acao == "recusar_pergunta":
                    aceito = aceito and p.respondida and not p.fora_do_fato()
                else:
                    # A hora corrigida é entrada humana. A validação dela fica em aberto.
                    aceito = aceito and p.tipo_resposta == "hora"
            if aceito:
                acoes.add(f"{canal}:{acao}:{card.pk}")

        for pk in sorted(card_ids):
            visiveis, forms = self.local(pk)
            if pid in visiveis:
                local.add(pk)
            for f in forms.get(pid, ()):
                acao_do_form(f, chamado_de(pk), "local")
            if pid in self.inline(pk) and validar_em_leitura(p, False):
                acoes.add(f"inline:validar_pergunta:{pk}")

        for colab in colabs.values():
            tela, forms = self.modal(colab)
            fio.update(tela.fio.get(pid, ()))
            gaveta.update(tela.gaveta.get(pid, ()))
            for card, eleita, f in forms.get(pid, ()):
                acao_do_form(f, card, "fio", eleita)
        lote = pid in self.lote
        if lote and validar_em_leitura(p, False):
            acoes.add("lote:validar_pergunta")
        return local, fio, gaveta, glpi_local, glpi_remoto, lote, acoes


@lru_cache(maxsize=None)
def portas_colab(colab):
    # app/chamados/services/pergunta_no_app.py:68–92
    # app/api/views.py:2553–2574
    pendentes = consulta_do_app(colab, agora=AGORA)["pendentes"]
    exigencias = exigencia_das_perguntas(pendentes, AGORA)
    app = set()
    for p in pendentes:
        razao, acoes = exigencias[p.pk]
        if not aguarda_admin_pela_tarefa(razao, acoes, conservado=p.estado == p.RESPONDIDA):
            app.add(p.pk)
    web = {
        p.pk for card in montar_chamados_fio(colab, so_janela=True)
        for p in card.perguntas_disputa if p.viva
    }
    user = colab.usuario
    conta_ativa = bool(user and user.is_active)
    # Elegibilidade de login novo, sem autenticar nem testar sessão.
    app_login = bool(
        conta_ativa and colab.situacao == "ativo"
        and colab.empresa.ativa and not is_admin_user(user)
    )
    return app, web, app_login, conta_ativa


def c2(chamados, inicio, fim, gestor):
    linhas, sem_gestor, sem_ator = [], [], []
    for ch, dia_ch in chamados.values():
        candidatas = {p.pk: (p, {"ancora"}) for p in ch._vivas_ancoradas("fechado")}
        if not ch._gate_conversa_do_pai(registrar=False):
            extras = PerguntaDisputa.objects.filter(
                pk__in=perguntas_que_seguram_csf(ch)
            ).select_related(
                "disputa__colaborador__empresa", "disputa__chamado", "chamado_destino", "celula"
    

[truncado em 20000 de 47222 caracteres]

## 27/11 20:01 · sessao 70043781f402 · - · - · origem=Ronald (tmux)

C2 pode cruzar o PK pela projeção real do detalhe local, pelos cartões e itens da gaveta do fio e pelas referências da classe A na prévia. O modal HTTP marca notas como lidas no GET. A sonda deve usar `montar_chamados_fio` ou seu construtor de contexto e renderizar os fragmentos somente em memória. O formulário renderizado identifica a ação oferecida, e as guardas de leitura identificam recusas previsíveis. O resultado da escrita fica [nao verificado]. `app/chamados/services/acoes_chamado.py:337–358`, `app/chamados/views.py:870–890`, `app/chamados/services/fio.py:573–618,694–760,776–790`, `app/chamados/views_cobrar.py:352–399`.

O detalhe GLPI pode redirecionar para um cartão local por colisão do ID do ticket com um PK local. O ramo GLPI só apresenta descrição e followups sem vínculo estruturado por PK de pergunta. Menções em texto remoto ficam [nao verificado]. `app/chamados/views.py:219–269`, `app/templates/chamados/detalhe.html:65–143`.

C4 usa a união por PK do C6 com `respostas_sem_veredito` sobre a disputa inteira. O app expõe os PKs pendentes em `consulta_do_app`. Meu Atendimento usa `montar_chamados_fio(so_janela=True)` e o predicado `p.viva`. O acesso do colaborador também depende da conta e da situação cadastral. `app/chamados/models.py:2000–2053`, `app/chamados/services/pergunta_no_app.py:68–92`, `app/chamados/views.py:560–570,1229–1247`, `app/templates/chamados/partials/_card_atendimento.html:73–78`, `app/api/login_helpers.py:121–148`.

C5 deve ler `_plano_pergunta_viva('fechado')` para distinguir uma guarda de um caso que seria reatado. O S89 registra a execução em `contexto_json['executado_em']`, antes dos dois laços da cascata. Abertos hoje e anteriores a esse instante são candidatos. A relação causal com aquela execução fica [nao verificado] sem trilha histórica. `app/chamados/models.py:393–464,1779–1794`, `app/colaboradores/services/desligamento.py:150–214`.

## 27/11 20:00 · sessao c7d29a82460e · - · - · origem=Ronald (tmux)

- **C4.** O universo continua sendo o filtro da S0. Para cada disputa, `respostas_sem_veredito(perguntas=ps)` e `pergunta_viva(p)` fornecem a união dos PKs bloqueantes de C7. `fio_vivo(ps)` devolve um booleano. A ordem exclusiva fica sem porta para algum bloqueante, ação de gestor pendente, resposta de colaborador pendente e, com a união vazia, `sem_bloqueante`. `app/chamados/models.py:2000–2053`, `app/chamados/juizes.py:1603–1619`.
- **Porta do colaborador.** Cruze o PK com `consulta_do_app(colab, agora=AGORA)['pendentes']` e os cartões de `montar_chamados_fio(colab, so_janela=True)` com `p.viva`. A view de Meu Atendimento grava trilha, portanto use o leitor. Confira a credencial ativa e as guardas do login nativo. A API ainda devolve `aguarda_admin` calculado pela tarefa. O drawer de arquivados leva ao detalhe local, cujo bloco tipado exige gestor. `app/chamados/services/pergunta_no_app.py:68–92`, `app/chamados/services/fio.py:228–244`, `app/chamados/views.py:560–594,1228–1247`, `app/api/login_helpers.py:121–149`, `app/api/views.py:2549–2575`, `app/templates/chamados/partials/_card_atendimento.html:73–78`, `app/templates/chamados/partials/_meus_arquivados.html:26–36`, `app/templates/chamados/detalhe_local.html:296–310`.
- **C5.** Use a guarda seca `ch._gate_pergunta_viva('fechado', registrar=False)` e, quando ela recusar, os PKs de `ch._plano_pergunta_viva('fechado')`. Cruze só esses PKs com `DisputaSupervisao.sem_veredito_entre(...)` e com as portas de DP. A conversa do pai recusa `reconciliar()`, enquanto o `encerrar()` da cascata chama a guarda da pergunta viva. Uma resposta sem veredito sem porta de DP é uma guarda real. A incidência dessa combinação fica [nao verificado] e a partição em três rótulos precisa explicitar como lida com ela. `app/chamados/models.py:393–450,705–708,1779–1794,2019–2045`, `app/colaboradores/services/desligamento.py:164–190`.
- **S89 e data.** Prefira `contexto_json.executado_em`. Para o antes/depois sem S89, `HistoricoVinculo.criado_em` da linha efetiva é um candidato quando coincide com a data civil e exclui o agendamento. Por auditoria, compare todos os chamados `NAO_FECHADOS` e disputas ainda abertas do colaborador criados antes do instante da cascata, fora inclusive da janela S0. Separe contagens de auditorias com chamado, disputa e qualquer um dos dois. O estado presente, mesmo com guarda seca, deixa causalidade histórica [nao verificado] quando o item pode ter fechado e reaberto. `app/colaboradores/services/desligamento.py:112–117,138–155,182–217,235–259`, `app/colaboradores/models.py:577–584`, `app/chamados/catalogo/motor.py:24–32`, `app/chamados/models.py:2414–2450`.

## 27/11 19:59 · sessao 05af44278a51 · - · - · origem=Ronald (tmux)

C1 deve preservar a seleção da S0 e ler `protestos` pelo `celula_id` do retentor. Use `tarefa_do_dia(..., carga=carga)` com `porta_da_tarefa` e `_fio_da_porta`. A razão aparece também em `instancias[*].razao`, portanto a saída precisa incluir as duas origens. `app/folha/export.py:1268`, `app/escala/models.py:1786`, `app/chamados/services/tarefa_do_dia.py:292,809,830`, `app/folha/fora_da_folha.py:86`.

Para complementar, use `ListaDoAdmin` e `dono_fora_do_fio` com todas as perguntas da célula. O segundo retorna o primeiro encaminhamento reconhecido. `dia_na_lista`, `dia_do_admin` e `destino_fora_da_mesa` permitem registrar o que a mesma lista publicou para o dia. A oferta do calendário vem de `resolver_dia_oferecido_nas_datas`, com permissão de tela à parte. `app/chamados/services/vigia_fio.py:166,202,210,228,252,607`, `app/colaboradores/services/calendario.py:97,125`, `app/templates/colaboradores/partials/_calendario_grade.html:134`.

Para o agregado sem marca, `PORTA_DO_TIPO` com `tipo_da_linha` distingue a porta publicada de Cadastro da ausência de link próprio de férias. `portas_da_linha` pode oferecer o legado “Validar no fio” por padrão em motivos não-espelho, inclusive férias. Uma porta oferecida e o primeiro carimbo verdadeiro do papel da tarefa sustentam a classificação da sonda. Sua eficácia para resolver o caso e a atribuição ao Hasner ficam [nao verificado]. `app/folha/retrato_folha.py:361,436,496,1003,1066`, `app/chamados/services/tarefa_do_dia.py:762`.

Enviei também ao agente principal as referências de C3 para a união anterior aos descartes, registro dos eventos e leitura das guardas secas. `app/ponto/services/lastro.py:185,238,316`, `app/chamados/reconciliador.py:440,1140,1344,1389,1550`, `app/chamados/models.py:677,683`.

## 27/11 19:58 · sessao 674d858a35ca · INVENTARIO-PELA-REGRA · leitura · origem=Code

A prévia segue a porta desta cópia. Ela recalcula em memória, classifica esses mesmos objetos e os entrega ao montador real. O FM gravado é lido em separado. Para a 8792, a prévia conta datas de falta e o FM guarda `horas_falta`. Por isso, `fm=-` em dias significa quantidade indisponível nessa fonte. [app/folha/views.py:214] [app/folha/views.py:275] [app/folha/export.py:749] [app/ponto/models.py:427]

Execução dos scripts e resultado de outubro em produção [nao verificado].

## Script A — sonda Django na Sombra

Para uma sessão Django já inicializada. O script aplica as três guardas do molde, faz a leitura em uma transação própria e imprime o DPH após o rollback. A inicialização fica fora da sonda porque a configuração herdada prepara diretório e handlers de log. [modelo_sonda_S1.py:14] [app/config/settings/sombra.py:27] [app/config/settings/saas.py:118] [app/config/settings/saas.py:160]

```python
import logging
import re
import sys
from collections import Counter, defaultdict, deque
from decimal import Decimal
from pathlib import Path
from traceback import extract_tb
from types import SimpleNamespace

from django.conf import settings
from django.db import DatabaseError, connection, transaction
from django.utils import timezone
from django_tenants.utils import schema_context

if getattr(settings, "HASNER_SOMBRA", False) is not True:
    raise SystemExit("PAREI: HASNER_SOMBRA ausente")
db = connection.settings_dict
if (db["NAME"], db["HOST"]) != ("sombra", "sombra_db"):
    raise SystemExit("PAREI: conexao fora da Sombra")
if connection.in_atomic_block or not connection.get_autocommit():
    raise SystemExit("PAREI: a sonda exige transacao propria")

from colaboradores.models import Empresa, HistoricoVinculo
from core.constants import isento_ponto
from folha import export as ex
from folha.models import EVENTO_CHOICES, RubricaDominio
from ponto.models import FechamentoMensal
from ponto.services import fechamento as fc

MES, ANO = 10, 2026
EMPRESAS = (2, 3, 4)
COMP = "2026-10"
COMP_TXT = "202610"
FALTA = "falta_diurna"


def pare(motivo):
    raise SystemExit(f"PAREI: {motivo}")


def sql_estrito(execute, sql, params, many, context):
    try:
        return execute(sql, params, many, context)
    except DatabaseError:
        raise SystemExit("PAREI: erro SQL na transacao da sonda") from None


def recalcular_leitura(emp, universo):
    # app/ponto/services/fechamento.py:74, 1069, 1105, 1111, 1327.
    # Interrompe a função real antes dos handlers que degradam ou pulam o vínculo.
    alvo = fc.recalcular_fechamento_mes.__code__

    def rastrear_frame(frame, evento, arg):
        if evento == "exception" and not isinstance(arg[1], fc._GradeSemCelula):
            raise SystemExit(
                f"PAREI: recálculo em memória, empresa {emp}, "
                f"app/ponto/services/fechamento.py:{frame.f_lineno}"
            ) from None
        return rastrear_frame

    def rastrear_chamada(frame, evento, arg):
        if frame.f_code is alvo:
            return rastrear_frame
        return None

    anterior = sys.gettrace()
    sys.settrace(rastrear_chamada)
    try:
        lidos = fc.recalcular_fechamento_mes(
            MES, ANO, empresa_id=emp, somente_leitura=True,
            lavrar_dias_pagos=False, retornar_objeto=True,
        )
    finally:
        sys.settrace(anterior)
    if set(lidos) != set(universo):
        pare(f"empresa {emp}: recálculo incompleto")
    return lidos


def dph(tipo, **campos):
    return "|".join(["DPH", tipo] + [f"{k}={v}" for k, v in campos.items()])


def matricula(c):
    codigo = (c.codigo_dominio or "").strip()
    if re.fullmatch(r"[0-9]{1,10}", codigo):
        return codigo.zfill(10)
    return "-"


def unidade(rub):
    if rub.tipo == "horas":
        return "h"
    if rub.tipo == "dias":
        return "dias"
    pare("tipo de RubricaDominio desconhecido")


def cent_h(quantidade):
    # Escalar de app/folha/export.py:42, sem montagem de layout.
    return int(round(float(quantidade) * 100))


def equivalente_min(quantidade):
    return Decimal(str(quantidade)).quantize(Decimal("0.01")) * 60


def evento_fm(fm):
    if fm is None:
        return []
    # Projeta apenas os campos persistidos, antes de isenção e supressão 12x36.
    snapshot = SimpleNamespace(**{
        campo.attname: getattr(fm, campo.attname)
        for campo in FechamentoMensal._meta.concrete_fields
    })
    return ex.eventos_do_fechamento(snapshot)


def evento_lido(fm):
    return ex.eventos_do_fechamento(
        fm, tipo_ciclo=ex._ciclo_do_colab(fm.colaborador, MES, ANO),
        feriado_12x36_em_dobra=ex.feriado_12x36_em_dobra(fm.colaborador, MES, ANO),
    )


def sondar(empresa, medido_em):
    emp = empresa.pk
    ini, fim = ex.janela_competencia(empresa, MES, ANO)
    cod_empresa = ex.codigo_no_sistema_dominio(empresa)
    if not re.fullmatch(r"[0-9]{10}", cod_empresa):
        pare(f"empresa {emp}: código de integração inválido")

    mapa = ex._rubricas_map(empresa)
    globais = Counter(RubricaDominio.objects.filter(
        empresa__isnull=True, ativo=True,
    ).values_list("evento", flat=True))
    mapas = {}
    for evento in sorted({e for e, _ in EVENTO_CHOICES} | set(mapa)):
        rub = mapa.get(evento)
        if rub is None:
            mapas[evento] = ("-", "-", "-")
            continue
        if rub.empresa_id is None and globais[evento] != 1:
            pare(f"empresa {emp}: default ativo ambíguo")
        codigo = rub.codigo_fopag.zfill(4)
        if not re.fullmatch(r"[0-9]{4}", codigo):
            pare(f"empresa {emp}: código de rubrica inválido")
        fonte = "RubricaDominio" if rub.empresa_id is not None else "default"
        mapas[evento] = (codigo, unidade(rub), fonte)

    universo = {c.pk: c for c in fc.universo_da_competencia(MES, ANO, emp)}
    gravados = {
        f.colaborador_id: f for f in FechamentoMensal.objects.filter(
            colaborador__empresa=empresa, mes=MES, ano=ANO,
        ).select_related("colaborador")
    }
    colabs = {cid: f.colaborador for cid, f in gravados.items()}
    colabs.update(universo)
    codigos = {cid: matricula(c) for cid, c in colabs.items()}
    validos = [codigo for codigo in codigos.values() if codigo != "-"]
    if len(set(validos)) != len(validos):
        pare(f"empresa {emp}: matrícula duplicada após formatação do layout")

    # O marcador vem do ato de transferência. A consulta traz somente IDs.
    mudancas = set(HistoricoVinculo.objects.filter(
        colaborador_id__in=colabs, motivo="transferencia", data__gte=ini,
        obs__contains="[CNPJ mudou:",
    ).values_list("colaborador_id", flat=True))

    lidos = recalcular_leitura(emp, universo)
    lista_lida = list(lidos.values())
    itens = ex.classificar_export(empresa, MES, ANO, fechamentos=lista_lida)
    juiz = {i["colaborador_id"]: i for i in itens}
    if set(juiz) != set(lidos) or len(juiz) != len(itens):
        pare(f"empresa {emp}: classificação incompleta ou duplicada")

    linhas, entra = ex.montar_linhas_export(
        empresa, MES, ANO, itens=itens, fechamentos=lista_lida,
    )
    if ex.validar_layout_dominio(linhas):
        pare(f"empresa {emp}: prévia rejeitada pelo validador real")

    proj_lida = {cid: evento_lido(f) for cid, f in lidos.items()}
    dias = {cid: len(ex.datas_falta(c, empresa, MES, ANO))
            for cid, c in colabs.items()}

    # A quantidade TXT vem exclusivamente dos registros reais. A fila só
    # identifica evento/unidade e confere cada registro com o montador real.
    esperados = deque()
    for cid, f in lidos.items():
        if cid not in entra or f.status not in ex.STATUS_NO_TXT:
            continue
        for evento, valor in [*proj_lida[cid], (FALTA, dias[cid])]:
            rub = mapa.get(evento)
            if not valor or rub is None:
                continue
            linha = ex.montar_linha_10(
                codigos[cid], COMP_TXT, rub.codigo_fopag, rub.tipo,
                valor, cod_empresa,
            )
            esperados.append((linha, cid, evento))

    saidas_txt = defaultdict(list)
    for linha in linhas:
        parte = ex.partes_da_linha(linha)
        if parte is None:
            pare(f"empresa {emp}: registro TXT ilegível")
        if parte["reg"] == "11":
            continue
        if not esperados:
            pare(f"empresa {emp}: registro 10 sem evento correspondente")
        esperada, cid, evento = esperados.popleft()
        if linha != esperada:
            pare(f"empresa {emp}: evento e registro 10 divergentes")
        saidas_txt[cid].append((evento, parte["valor"]))
    if esperados:
        pare(f"empresa {emp}: registro 10 esperado ausente")

    fm_status = Counter(f.status for f in gravados.values())
    yield dph("META", emp=emp, comp=COMP, janela=f"{ini}..{fim}", motor="-",
              fm_status=",".join(f"{s}:{n}" for s, n in sorted(fm_status.items())) or "-",
              hash_txt=ex.hash_do_txt(ex.conteudo_do_txt(linhas)),
              codigo_dominio_empresa=cod_empresa, medido_em=medido_em)
    for evento, (rub, un, fonte) in mapas.items():
        yield dph("MAP", emp=emp, evento=evento, rubrica=rub, fonte=fonte,
                  unidade=un)

    for cid, c in sorted(colabs.items()):
        fm = gravados.get(cid)
        lido = lidos.get(cid)
        item = juiz.get(cid)
        isento = isento_ponto(c)
        txt = saidas_txt[cid]
        codigo = codigos[cid]
        fora = ((c.data_admissao is not None and c.data_admissao > fim)
                or (c.data_demissao is not None and c.data_demissao < ini))

        classe, motivo = "elegivel", "-"
        if ex.sai_pela_rescisao(c, fim):
            classe, motivo = "rescisao", "rescisao_modulo_proprio"
        elif item is None:
            classe, motivo = "retido", "fora_universo_fechamento"
        elif item["status"] != "entra":
            classe, motivo = "retido", item["motivo"]
        elif codigo == "-":
            classe, motivo = "retido", "codigo_invalido_dominio"
        elif lido.status not in ex.STATUS_NO_TXT:
            classe, motivo = "retido", "status_fora_txt"
        elif isento and not txt:
            classe, motivo = "isento", "isento_eventos_horas"

        col = f"col{cid}"
        yield dph("COLAB", emp=emp, col=col, cod=codigo,
                  fm=fm.pk if fm else "-", status=fm.status if fm else "-",
                  export=classe, motivo=motivo, juiz=item["status"] if item else "-",
                  universo=int(cid in universo), fora_janela=int(fora),
                  mudanca_empresa=int(cid in mudancas), isento=int(isento),
                  fm_falta_min=equivalente_min(fm.horas_falta) if fm else "-",
                  fm_dsr_min=Decimal(str(fm.horas_reflexo_dsr)) * 60 if fm else "-")

        # Agrupa parcelas só depois de converter cada evento. Código ausente
        # fica separado por evento. Unidade física do FM vem do campo gravado.
        grupos = defaultdict(lambda: defaultdict(dict))
        for lado, eventos in (("fm", evento_fm(fm)), ("lido", proj_lida.get(cid, []))):
            for evento, horas in eventos:
                rub, _, _ = mapas[evento]
                chave = (rub, "h", evento if rub == "-" else "")
                grupos[chave][evento][lado] = (cent_h(horas), equivalente_min(horas))
        if dias[cid]:
            rub, _, _ = mapas[FALTA]
            chave = (rub, "dias", FALTA if rub == "-" else "")
            grupos[chave][FALTA]["lido"] = (dias[cid], dias[cid])
        for evento, valor in txt:
            rub, un, _ = mapas[evento]
            grupos[(rub, un, "")][evento]["txt"] = (valor, None)

        for (rub, un, _), parcelas in sorted(grupos.items()):
            lados = {
                lado: [p[lado] for p in parcelas.values() if lado in p]
                for lado in ("fm", "lido", "txt")
            }
            if not lados["txt"] and not any(
                bruto for lado in ("fm", "lido") for _, bruto in lados[lado]
            ):
                continue
            totais = {lado: sum(q for q, _ in itens) if itens else "-"
                      for lado, itens in lados.items()}
            fonte = next((lado for lado in ("fm", "lido") if lados[lado]), None)
            bruto = sum(b for _, b in lados[fonte]) if fonte else "-"
            fonte_bruto = (
                "dias_falta_vivos" if un == "dias" and fonte else
                f"{fonte}_horas_x60" if fonte else "-"
            )
            yield dph("RUB", emp=emp, col=col, cod=codigo, rubrica=rub,
                      evento=",".join(sorted(parcelas)), unidade=un,
                      fm=totais["fm"], txt=totais["txt"], bruto_min=bruto,
                      lido=totais["lido"], fonte_bruto=fonte_bruto)


nivel_log = logging.root.manager.disable
logging.disable(logging.CRITICAL)
try:
    with schema_context("juliani"), transaction.atomic():
        with connection.execute_wrapper(sql_estrito):
            with connection.cursor() as cursor:
                cursor.execute(
                    "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
                )
            medido_em = timezone.now().isoformat()
            registros = [
                linha for emp in EMPRESAS
                for linha in sondar(Empresa.objects.get(pk=emp), medido_em)
            ]
            transaction.set_rollback(True)
except Exception as erro:
    ponto = extract_tb(erro.__traceback__)[-1]
    raise SystemExit(
        f"PAREI: {type(erro).__name__} em {Path(ponto.filename).name}:{ponto.lineno}"
    ) from None
finally:
    logging.disable(nivel_log)

for registro in registros:
    print(registro)
print(dph("FIM", n=len(registros)))
```

## Script B — `join.py`

A cobertura entra inteira no universo do DP, inclusive os 151 vínculos sem linha selecionada. O join confere os totais do anexo e prova a ausência de colisões dentro de cada empresa e fonte antes de parear os códigos. As duas saídas são abertas em modo exclusivo quando o script for executado no host. [parecer.md:8–13] [formato_csv.md:3] [formato_csv.md:7]

```python
import csv
import io
import os
import re
import sys
from collections import Counter, defaultdict
from decimal import Decimal
from pathlib import Path

EMP = {"JA": 2, "JSP": 3, "RA": 4}
COB_ESPERADA = {2: 431, 3: 118, 4: 23}
RUB_ESPERADA = {2: 788, 3: 203, 4: 27}
FOCO = {"0025": "h", "0150": "h", "0200": "h", "0243": "h",
        "8069": "h", "8792": "dias"}
ESPECIAIS = {
    "DSR": set("0237 0244 0250 0854 8125 8794 9360".split()),
    "Horista": {"0204"},
    "Intermitente": {"0251", "0284", "0286"},
    "Ausência justificada": {"8932"},
}
SELECAO = set(FOCO).union(*ESPECIAIS.values())
CAB_RUB = (
    "competencia empresa cnpj codigo_empregado_dominio rubrica descricao "
    "unidade_proposta referencia_original vencimento_brl desconto_brl pagina_pdf"
).split()
CAB_COB = (
    "competencia empresa cnpj codigo_empregado_dominio paginas_pdf "
    "linhas_rubricas_ponto recibo_conferido"
).split()
CAMPOS_DPH = {
    "META": "emp comp janela motor fm_status hash_txt codigo_dominio_empresa medido_em",
    "MAP": "emp evento rubrica fonte unidade",
    "COLAB": "emp col cod fm status export motivo juiz universo fora_janela "
             "mudanca_empresa isento fm_falta_min fm_dsr_min",
    "RUB": "emp col cod rubrica evento unidade fm txt bruto_min lido fonte_bruto",
    "FIM": "n",
}
MOTIVOS = set(
    "- rescisao_modulo_proprio sem_codigo_dominio ferias_com_batida cadastro_zero "
    "furo_espelho nao_homologado fora_universo_fechamento codigo_invalido_dominio "
    "status_fora_txt isento_eventos_horas".split()
)
UNIDADES_TXT = {"h", "dias"}


def erro(local, mensagem):
    raise SystemExit(f"{local}: {mensagem}")


def exige(sim, local, mensagem):
    if not sim:
        erro(local, mensagem)


def padrao(valor, regex, local, campo):
    exige(re.fullmatch(regex, valor) is not None, local, f"{campo} inválido")
    return valor


def inteiro(valor, local, campo, opcional=False, sinal=False):
    if opcional and valor == "-":
        return None
    regex = r"-?[0-9]+" if sinal else r"[0-9]+"
    padrao(valor, regex, local, campo)
    return int(valor)


def linhas_csv(caminho, cabecalho):
    with caminho.open(encoding="utf-8-sig", newline="") as arquivo:
        leitor = csv.reader(arquivo, delimiter=";", strict=True)
        try:
            exige(next(leitor, None) == cabecalho, f"{caminho}:1", "cabeçalho inesperado")
            while True:
                primeira = leitor.line_num + 1
                campos = next(leitor, None)
                if campos is None:
                    return
                local = f"{caminho}:{primeira}"
                exige(len(campos) == len(cabecalho), local, "número de campos incorreto")
                yield local, dict(zip(cabecalho, campos))
        except (csv.Error, UnicodeError):
            erro(f"{caminho}:{leitor.line_num + 1 if leitor.line_num == 0 else leitor.line_num}",
                 "CSV ou UTF-8 inválido")


def linhas_dph(caminho):
    total = 0
    fim = False
    with caminho.open(encoding="utf-8-sig") as arquivo:
        for numero, linha in enumerate(arquivo, 1):
            local = f"{caminho}:{numero}"
            exige(not fim, local, "dados após DPH|FIM")
            marca, tipo, *campos = linha.rstrip("\n").split("|")
            exige(marca == "DPH" and tipo in CAMPOS_DPH, local, "registro DPH inválido")
            valores = {}
            for campo in campos:
                nome, separador, valor = campo.partition("=")
                exige(separador and nome not in valores, local, "campo DPH inválido ou repetido")
                valores[nome] = valor
            exige(set(valores) == set(CAMPOS_DPH[tipo].split()), local, "campos DPH inesperados")
            if tipo == "FIM":
                exige(inteiro(valores["n"], local, "n") == total, local, "DPH incompleto")
                fim = True
            else:
                total += 1
                yield local, tipo, valores
    exige(fim, f"{caminho}:{total + 1}", "DPH sem marcador FIM")


def identidade_dp(linha, local):
    exige(linha["competencia"] == "2026-10", local, "competência diferente de 2026-10")
    exige(linha["empresa"] in EMP, local, "empresa DP desconhecida")
    codigo = padrao(linha["codigo_empregado_dominio"], r"[0-9]+", local, "código DP")
    return EMP[linha["empresa"]], codigo


def candidato(codigo):
    return codigo.lstrip("0") or "0"


def prova_zeros(indice, emp, codigo, identidade, local):
    # Usado apenas para prova. O join começa depois de ler ambas as fontes.
    chave = emp, candidato(codigo)
    anterior, origem = indice.setdefault(chave, (identidade, local))
    exige(anterior == identidade, local, f"colisão de zeros à esquerda com {origem}")


def referencia(texto, unidade, local):
    padrao(texto, r"[0-9]+(?:,[0-9]+)?", local, "referência DP")
    valor = Decimal(texto.replace(",", "."))
    if unidade == "h":
        valor *= 100
    if unidade in UNIDADES_TXT:
        exige(valor == valor.to_integral_value(), local, "precisão incompatível com a unidade")
        return int(valor)
    return valor


def ler_dp(rubricas, cobertura):
    cob, rub, prova = {}, {}, {}
    n_cob, n_rub, por_vinculo = Counter(), Counter(), Counter()
    for local, linha in linhas_csv(cobertura, CAB_COB):
        emp, codigo = chave = identidade_dp(linha, local)
        prova_zeros(prova, emp, codigo, codigo, local)
        exige(chave not in cob, local, "vínculo DP duplicado na cobertura")
        exige(linha["recibo_conferido"] == "sim", local, "recibo sem conferência")
        n = inteiro(linha["linhas_rubricas_ponto"], local, "linhas_rubricas_ponto")
        cob[chave] = (n, local)
        n_cob[emp] += 1

    for local, linha in linhas_csv(rubricas, CAB_RUB):
        emp, codigo = vinculo = identidade_dp(linha, local)
        prova_zeros(prova, emp, codigo, codigo, local)
        exige(vinculo in cob, local, "rubrica DP sem vínculo na cobertura")
        r =

[truncado em 20000 de 40900 caracteres]

## 27/11 19:56 · sessao 5063728d707e · FIM-DE-SEMANA-JA · censo · origem=Code

# FIM-DE-SEMANA-JA · Triagem T0

**O aval de 0 BO Hasner fica [nao verificado].** Contei as **38 linhas marcadas de C1, em 22 colaboradores**. A transcrição de `partida_2711.out` termina no meio de C2, sem o rodapé e sem as linhas de C4 e C5. Usei os totais do enunciado como declarados. A cópia tem o código citado abaixo. A identidade Git do HEAD ficou [nao verificado], pois a cópia não tem metadados Git acessíveis. A análise foi só por leitura, com os REDs descritos como propostas. `sonda.py:98–111`.

## C1 · Dias que seguram a folha

O retentor lê o veredito da célula e os protestos. A sonda imprime o veredito, o fio e, quando existe, o primeiro PK da fila. A divisão abaixo é exata para as **38 linhas marcadas**. A causa dentro de cada veredito fica [nao verificado] quando depende dos protestos ou da razão da tarefa, campos ausentes da saída. `app/folha/export.py:1268–1271,1282–1305`, `sonda.py:98–111`.

| Motivo / fio | Linhas / colabs | Causa e classificação |
|---|---:|---|
| `indefinida / ninguem` | 15 / 6 | A célula acusa previsto em dia indefinido, fase sem declaração ou ausência parcial sem período. A tarefa ficou sem rota. **Hasner** se nenhuma saída complementar existir. **Produção** se o dia estiver encaminhado para corrigir vínculo, escala ou ausência. A divisão fica [nao verificado]. `app/escala/models.py:1626`, `app/chamados/services/vigia_fio.py:268–278`, `app/folha/retrato_folha.py:1003–1020`. |
| `furo / ninguem` | 1 / 1 | O veredito agrega cinco protestos de furo. A tarefa ficou sem rota. **Hasner** se também faltar saída no calendário, na lista do admin ou em outro destino previsto. Protesto e saída complementar [nao verificado]. `app/escala/models.py:1623`, `app/folha/fora_da_folha.py:86–91`, `app/chamados/services/vigia_fio.py:654–706`. |
| `furo / nao_sei` | 2 / 2 | **Hasner na leitura da tarefa.** Os ramos incluem célula ausente, ata sem lâmpadas e marco sem leitura. O ramo exato por dia fica [nao verificado]. `app/chamados/services/tarefa_do_dia.py:236–246,304–310`. |
| `cobrado / nao_sei` | 1 / 1 | **Hasner na leitura da tarefa.** Há veredito de cobrança lavrado e a tarefa deixou a rota indeterminada. A razão exata fica [nao verificado]. `app/ponto/services/cartorio.py:1544–1545`, `app/chamados/services/tarefa_do_dia.py:731–733,766–779`. |
| `cobranca_indevida / ninguem` | 2 / 2 | **Candidatas Hasner.** A célula acusa cobrança com todos os marcos casados ou cobrança em dia sem trabalho, e a tarefa ficou sem rota. Qual dos dois protestos, a atualidade da lavra e a presença de uma conversa a julgar ficam [nao verificado]. `app/ponto/supra_juiz.py:479–484,507–512,578–588`. |
| `cobranca_indevida / admin` | 5 / 2 | A tarefa publicou porta de admin. **Produção** quando o gestor precisa julgar uma resposta preservada. **Hasner** quando a cobrança já perdeu a premissa, está apta a fechar e segue retendo. A divisão fica [nao verificado]. `app/ponto/supra_juiz.py:578–588`, `app/ponto/services/lastro.py:207–228`, `app/templates/chamados/partials/modal_fio.html:306–339`. |
| `discordante / colab` | 4 / 4 | **Produção se a pergunta procede.** A porta publicada é responder no app. O veredito pode vir de batida órfã, realizado inflado ou realizado zero com turno. A pertinência da cobrança por linha fica [nao verificado]. `app/escala/models.py:1627`, `app/chamados/services/tarefa_do_dia.py:748–750,775–776`, `app/api/views.py:2538–2555,2634–2656`. |
| `discordante / admin` | 8 / 5 | **Produção se a ação publicada procede.** O admin pode validar ou decidir no fio, resolver o dia no calendário ou decidir a ausência. A ação específica e o protesto por linha ficam [nao verificado]. `app/escala/models.py:1627`, `app/folha/retrato_folha.py:1016–1020`, `app/templates/chamados/partials/modal_fio.html:306`, `app/templates/colaboradores/partials/_calendario_grade.html:134`, `app/ponto/views.py:2095`. |

Nas **18 linhas `ninguem`**, a ausência confirmada é a da rota da tarefa. O próprio vigia ainda cruza ausência aberta, folga contestada, Cadastro x Realidade e Pauta DP. Nas **3 linhas `nao_sei`**, há também uma falha de apresentação reproduzível pela leitura estática: o retrato guarda `dias_sem_leitura` e o drawer só marca `Sem porta` ao lado da data. A quantidade de casos que esse detalhe escondeu na tela T0 fica [nao verificado]. `app/chamados/services/vigia_fio.py:252–279,630–706`, `app/folha/retrato_folha.py:1244–1245`, `app/templates/folha/_retrato_o_que_fazer.html:42–46`.

**As 1.000 linhas `aval=-` declaradas, em três linhas:**

- `espelho_*` com fio `colab` ou `admin` tem rota humana publicada. **Produção** quando falta responder no app, validar no fio, resolver no calendário ou decidir a ausência. `app/chamados/services/tarefa_do_dia.py:748–790`, `app/templates/chamados/partials/_card_atendimento.html:73–78`, `app/folha/retrato_folha.py:1016–1020`.
- `sem_codigo_dominio` tem porta humana de **produção**. Abrir Cadastro do colaborador, editar **Código folha** e salvar. `app/folha/export.py:881–884`, `app/folha/retrato_folha.py:1009`, `app/templates/colaboradores/editar.html:62`, `app/colaboradores/views.py:1353–1365`.
- `ferias_com_batida` pede conferência do DP. O retrato deixa esse motivo sem link próprio. A aba Férias oferece correção do agendamento quando cabível. A ação correta para a batida do anexo fica [nao verificado]. `app/folha/export.py:885–893`, `app/folha/retrato_folha.py:1010`, `app/templates/colaboradores/partials/painel_colaborador.html:43`, `app/templates/ferias/_secao_colab.html:145–171`.

## C2 · Pergunta que segura o chamado

**Número real de chamados cujo gestor não vê ao menos uma pergunta bloqueante: [nao verificado].** Os **2.527 pares em 1.018 chamados** são o total declarado para o filtro da sonda. Esse filtro compara a disputa da pergunta com a primeira disputa própria aberta do detalhe local. Para GLPI, a disputa de comparação fica vazia. A saída não traz o container da disputa, o estado aberto ou fechado, a rota efetiva, o perfil do gestor nem as perguntas renderizadas. O teste de diferença de disputa tem **falsos positivos demonstráveis pelo código**. `app/chamados/services/acoes_chamado.py:345–358`, `app/templates/chamados/detalhe_local.html:296–310`, `app/chamados/services/fio.py:97–117,251–273,305–323`.

| Subclasse a medir | Classificação pelo código | Linhas / colabs |
|---|---|---:|
| Pergunta visível e com ação pelo modal do fio | Falso positivo de invisibilidade. O modal lista por `chamado_destino`, com fallback para o container, e pode juntar perguntas de **containers diferentes** no mesmo cartão. A viva de disputa fechada também pode entrar. **Produção** se falta validar, rejeitar ou corrigir onde o botão é aceito. O POST usa o PK da pergunta e a âncora que a lista. `app/chamados/views.py:870–890,928–936`, `app/chamados/services/fio.py:291–323,728–760,776–790`, `app/templates/chamados/partials/modal_fio.html:123,157–160,306–339`. | [nao verificado] |
| Pergunta fora do cartão, com outra via | A gaveta **detalhes do caso** pode mostrar texto e dia da disputa eleita. A prévia **Validar em lote** mostra as respostas elegíveis da classe A e permite confirmar. O botão inline atua sobre a disputa aberta eleita. Para o aval, uma pergunta apenas legível segue candidata **Hasner** se nenhuma via permite resolver o bloqueio. `app/chamados/services/fio.py:335–346,421–436`, `app/templates/chamados/partials/modal_fio.html:461–476`, `app/chamados/views_cobrar.py:145–172,371–399`, `app/templates/chamados/partials/_lote_preview.html:21–24`. | [nao verificado] |
| Pergunta ausente das vias acessíveis | **Hasner** quando a omissão esconde o bloqueio do gestor autorizado. A segunda aberta do **mesmo container** pode ficar fora da lista do modal, inclusive se tiver destino. Uma disputa sem container pode alimentar a explicação quando eleita, permanecendo fora da lista de cartões. O detalhe GLPI renderiza followups e texto livre. Pode haver menção em texto, cujo conteúdo fica [nao verificado]. Um chamado GLPI também pode aparecer no modal. `app/chamados/services/fio.py:242–273,335–348`, `app/chamados/tests/test_e4c_f5_82c3c_cartao_misto.py:217–234`, `app/chamados/views.py:243–269`, `app/templates/chamados/detalhe.html:69–103,116–143`. | [nao verificado] |

Para contar, cruzar o **PK exato da pergunta** com o detalhe, o modal incluindo a gaveta, e a prévia de lote para o gestor autorizado. Publicar separadamente `COUNT(DISTINCT chamado_id)` com alguma pergunta invisível e com algum bloqueio sem ação alcançável. Manter os dois predicados da guarda. `ancora` é C6 no pós-estado. `conversa_pai` inclui respostas sem veredito de origem e de destino, que podem aparecer em outro cartão. Para medir todo o universo de bloqueios, preservar também respostas que o CSF segura e o segundo filtro C6 da sonda descarta. `app/chamados/models.py:332–391,718–758`, `app/ponto/supra_juiz.py:561–575`, `app/chamados/utils.py:47–72`.

A sonda etiqueta C2 com `JUIZ-PERGUNTA-VIVA-MUDA-SEM-FATO`. O código liga essa obra ao carimbo de validação transitório e à releitura da disputa na devolução. A divergência de tela é demonstrável por outra causa. Atribuir alguma linha T0 ao juiz que mudou sem fato ou a uma mudança real de dia fica [nao verificado]. `app/chamados/services/validacao.py:319–327,734–742`, `app/chamados/services/fio.py:251–273`.

## C3 · Caso que o reconciliador deixou vivo

**0 emitidos é o total declarado. A causa do zero e a identidade dos 112 ficam [nao verificado].** A sonda pode omitir candidatos antes de chamar `reconciliaria()`. No ramo de premissa, exclui o colaborador inteiro se houver evento pendente. Isso segue a cobertura prevista pelo consumidor, que agrupa por colaborador e revisita seus chamados, inclusive os de outro dia. O ramo de célula usa o PK da célula. O de lastro descarta `retido_pelo_fechamento` e preserva como escape o lastro por tolerância sem lâmpada, mesmo com evento no colaborador. A fila de lastro da sonda também deixa fora `registrado` e silenciado. `app/chamados/management/commands/reconciliar_chamados.py:62–85`, `app/chamados/reconciliador.py:1389–1398,1414–1424,1493–1533`, `app/ponto/services/lastro.py:316–319,388–402,412–424`, `app/chamados/services/painel.py:188–190`.

**Consulta para procurar os 112.** Na mesma empresa e janela, partir dos `VIVOS` e fazer a união por PK de três conjuntos, antes dos descartes. Incluir premissa morta por `_veredito_premissa`, absolvição por `_absolvido_pela_celula` no universo de `_chamados_de_turno_vivos`, e `com_lastro` mais `retido_pelo_fechamento` de `julgar`. No lastro, anotar presença na fila original e se os marcos estão acesos ou se a prova veio da tolerância. Em cada chamado, conservar todos os motivos e anotar evento por colaborador e por célula, idade, tentativas, erro, `retencao_da_retratacao` e a guarda que impede fechar. A consulta recupera os candidatos que esses filtros podem ter escondido. Retornar exatamente 112 fica [nao verificado]. `app/chamados/reconciliador.py:248,440–463,1140,1344,1547–1556`, `app/ponto/services/lastro.py:196–249,299–319`, `app/chamados/models.py:677–712`.

- **Hasner** se premissa morta, célula que absolveu ou lastro suficiente deixam um chamado apto sem gatilho efetivo. Separar evento em curso, falha ou esgotamento, e tolerância que o consumidor não executa. Subtotais [nao verificado]. `app/chamados/reconciliador.py:1389–1404,1518–1533,1547–1556`, `app/ponto/services/lastro.py:412–424`.
- **Produção** se uma resposta de irmã ou do pai ainda exige veredito e **Validar/Rejeitar** está alcançável. **Hasner** se a retenção esconde o motivo ou deixa a pergunta sem saída. Subtotais [nao verificado]. `app/chamados/reconciliador.py:440–453`, `app/chamados/models.py:718–758`, `app/templates/chamados/partials/modal_fio.html:306–339`.
- `reconciliaria()` é a decisão seca da transição. O ato completo de retratação ainda passa pela cerca de competência entregue. O resultado seco fica como coluna da consulta. `app/chamados/models.py:677–681`, `app/chamados/reconciliador.py:588–607`.

## C4 · Disputas abertas há mais de 30 dias

**260 disputas é o total declarado. Hasner sem porta, produção esperando resposta, produção esperando validação e respectivos colabs ficam [nao verificado].** `listar_c4` imprime idade e recorta a competência pelas datas disponíveis. A marca `SEM-JUIZ` é literal na sonda. A contagem por idade tem equivalente no auditor da cópia, com outro limiar. A guarda real de fechamento pergunta por C6 e por respostas sem veredito. `app/chamados/management/commands/auditar_disputas.py:132–144`, `app/chamados/juizes.py:1603–1619`, `app/chamados/models.py:2000–2053`.

A consulta de C4 deve agrupar por disputa e verificar as duas guardas e as portas de cada pergunta bloqueante:

- **Hasner, sem porta.** Alguma pergunta segura e nenhuma via resolve aquela pergunta. Isso inclui cartão oculto e botão suprimido quando falta via alternativa. `app/chamados/services/fio.py:251–273`, `app/templates/chamados/partials/modal_fio.html:123,251–276`.
- **Produção, resposta pendente.** O colaborador alcança **Responder** no app ou em Meu Atendimento. **Cobrar resposta** no modal avisa o colaborador. `app/chamados/services/pergunta_no_app.py:68–92`, `app/chamados/views.py:560–570,1229–1247`, `app/templates/chamados/partials/_card_atendimento.html:73–78`, `app/templates/chamados/partials/modal_fio.html:363–380`.
- **Produção, veredito pendente.** O gestor tem **Validar/Rejeitar/Corrigir** conforme o cartão e a guarda da ação. Uma disputa pode ter espera pelos dois atores. Para totais exclusivos, contar primeiro a disputa com algum bloqueio sem porta, depois a com veredito pendente e depois a que só espera resposta. `app/chamados/models.py:2041–2053`, `app/templates/chamados/partials/modal_fio.html:306–339`.
- **Sem bloqueante C7.** Uma disputa vazia ou com perguntas já mortas pode continuar aberta e requer inspeção do caminho de fechamento. O vigia de becos tem recorte próprio pelo pai resolvido. **Regularizar** pode devolver “Nenhuma pergunta validada para materializar”. O método `admin_encerra` existe, e sua ligação a uma tela fica [nao verificado]. `app/chamados/models.py:2047–2069,2210–2217`, `app/chamados/services/becos.py:25–35,56–61`, `app/chamados/services/acoes_disputa.py:199–203`.

## C5 · Chamados de desligados

**56 linhas declaradas, 23 `registrado s89` e 33 `aberto`/`em_analise`.** Colabs distintos, divisão dos 33 por estado e causas por PK ficam [nao verificado]. A sonda usa `situacao='desligado'` e o módulo de origem para marcar S89. `app/colaboradores/services/desligamento.py:23–32,191–217`.

- **23 S89 decl. — auditoria com porta do DP.** A emissão pede resposta oficial e aponta ao detalhe local. O catálogo os cria como `registrado` e a fila padrão de cobrança os deixa fora. O DP autorizado chega pelo link emitido ou pelo filtro `status=registrado`, abre **Responder como DP / Gestão** e envia a resposta livre. É **produção** se a conferência humana ainda estiver pendente. O estado `registrado` sozinho não informa se alguém já respondeu. `app/colaboradores/services/desligamento.py:195–223`, `app/chamados/catalogo/natureza.py:18–23,36–45`, `app/chamados/models.py:1126–1130`, `app/chamados/services/painel.py:300–316`, `app/templates/chamados/detalhe_local.html:374–407`, `app/chamados/services/acoes_chamado.py:292–305`.
- **33 abertos/em análise decl. — três causas a verificar.** Resposta já recebida sem veredito pode manter disputa e chamado abertos pela cascata. É **produção** quando o DP consegue validar ou rejeitar no fio. Se a única saída ainda exige resposta do desligado, cujo login a rotina bloqueou, é **Hasner**. Se nenhuma guarda legítima segura o chamado, cruzar a data de criação com a execução do desligamento para distinguir sobra da cascata, emissão posterior ou ação de DP ainda devida. A partição fica [nao verificado]. `app/colaboradores/services/desligamento.py:128–137,164–190`, `app/core/credenciais.py:100–114`, `app/chamados/models.py:1779–1794`, `app/templates/chamados/partials/modal_fio.html:306–339`.
- **Hasner reproduzível no texto do S89.** Quando a cascata mantém um chamado ou disputa aberto pelas guardas, o S89 ainda grava a frase de que a cascata fechou chamados e disputas abertas. A incidência nos 23 fica [nao verificado]. `app/colaboradores/services/desligamento.py:164–190,195–208`.

As obras da tabela são atribuições de triagem propostas. A confirmação de titularidade para causas novas fica [nao verificado]. Os REDs descrevem a prova a obter.

| FATIA | classe/subclasse | n | obra dona | arquivos | RED | mão |
|---|---|---:|---|---|---|---|
| Registrar causa e porta por dia | C1, todas as marcadas | 38 / 22 colabs | `REGISTRA-ACHADOS` | `sonda.py:98–111`, `app/chamados/services/vigia_fio.py:252–279,654–706` | Dois dias saem `ninguem` na sonda atual. Um tem correção de cadastro alcançável, o outro fica sem saída. A nova leitura distingue os dois e traz protesto, razão, rota e ação. | Dupla |
| Mostrar falha de leitura por data | C1, `nao_sei` | 3 / 3 colabs na sonda | `REGISTRA-ACHADOS` | `app/templates/folha/_retrato_o_que_fazer.html:46`, `app/folha/retrato_folha.py:1244` | Mesmo colaborador com um dia com porta e outro sem leitura. O segundo exibe **Sem leitura** na data. | Exceção, toca Folha |
| Medir visibilidade e ação real | C2, detalhe local, modal, GLPI e lote | 2.527 pares / 1.018 chamados decl. Subtotais [nao verificado] | `REGISTRA-ACHADOS` | `sonda.py`, `app/chamados/services/fio.py:251–348,421–436`, `app/chamados/views_cobrar.py:371–399` | O cruzamento reconhece a pergunta de outro pai visível no modal e a elegível no lote. Separa a visível só na gaveta da ausente de todas as telas. | Dupla |
| Expor a pergunta que bloqueia | C2/C4, segunda aberta do mesmo pai ou disputa fora da carga, sem alternativa efetiva | [nao verificado] | sem obra | `app/chamados/services/fio.py:251–273,335–348`, `app/templates/chamados/partials/modal_fio.html:123,157` | Duas abertas do mesmo pai e um caso sem container. O gestor autorizado encontra o PK bloqueante e a ação aponta para a disputa certa. O perfil sem permissão segue barrado. | Dupla na UI |
| Selar a mudança sem fato | C2/C4, se reproduzir a oscilação de C6 | [nao verificado] | `JUIZ-PERGUNTA-VIVA-MUDA-SEM-FATO` | `app/chamados/services/validacao.py:319–327,734–742`, `app/chamados/tests/test_pvmuda_carimbo_em_voo.py:1` | Duas leituras com os mesmos fatos persistidos dão o mesmo veredito. Validação recusada nunca publica o carimbo intermediário ao fechador. | Exceção se alterar materialização de ponto ou célula |
| Consumir a mudança de dia na origem | C1/C2/C4, se o dia mudou e a pergunta de turno continuou cobrando | [nao verificado] | `PERGUNTA-VIVA-NAO-MORRE-NO-DIA-MUDADO` | `app/ponto/services/veredito_celula.py:160–203`, `app/chamados/juizes.py:1499–1589` | Ato que transforma trabalho em folga encerra a cobrança do turno quando a lei libera. Uma conversa do mesmo dia civil permanece disponível para decisão. | Exceção, toca ponto e célula |
| Inventariar antes dos descartes | C3, premissa, célula e lastro | 0 emitidos decl. / 112 citados [nao verificado] | `RECONCILIAR-SEM-GATILHO` | `app/chamados/management/commands/reconciliar_chamados.py:62–87`, `app/ponto/services/lastro.py:316–319,412–424` | Candidato com evento de outro dia e candidato retido aparecem com suas razões. O evento que cobre o primeiro fica fora do alarme de falta de gatilho. | Dupla, só leitor |
| Ligar o fato ao produtor de evento | C3, apto sem gatilho confirmado | [nao verificado] | `RECONCILIAR-SEM-GATILHO` | `app/ponto/services/vigia_de_hora.py:87–113`, `app/ponto/portas/celula.py:981`, `app/chamados/reconciliador.py:1518–1533` | O prazo vence sem batida nova. O evento fecha o apto, a guarda humana conserva o retido e a segunda passagem é idempotente. | Exceção, toca ponto e célula |
| Medir bloqueio por disputa | C4, espera humana, sem porta ou sem bloqueante C7 | 260 decl. Subtotais [nao verificado] | `REGISTRA-ACHADOS` | `sonda.py`, `app/cham

[truncado em 20000 de 21108 caracteres]

## 27/11 19:55 · sessao 9cc8f9239142 · - · - · origem=Ronald (tmux)

**C2. O número de chamados entre os 1.018 em que o gestor realmente deixa de ver pelo menos uma pergunta que segura ficou [nao verificado].** A sonda compara o ID da disputa com a primeira aberta própria do detalhe local. O recorte pode produzir falso positivo e o anexo termina dentro de C2, sem estado da disputa, container, destino, RBAC, resposta ou conteúdo efetivamente mostrado por linha. O detalhe local escolhe a primeira aberta própria e inclui suas perguntas C6 vivas. `app/chamados/services/acoes_chamado.py:345–358`, `app/chamados/views.py:363,550–552`, `app/templates/chamados/detalhe_local.html:296–310`.

- **Pergunta visível no modal.** O modal de gestor do painel lê disputas abertas próprias eleitas e todas as fechadas com perguntas C6 vivas. Agrupa pelo `chamado_destino`, com fallback para o container, e exibe a lista quando o cartão tem disputa no cabeçalho e autorização departamental. Disputas de containers diferentes podem alimentar o mesmo cartão. No empate de `aberta_em`, a eleição do cabeçalho desempata por `-pk`, enquanto a carga das perguntas carece desse desempate. `app/templates/chamados/painel_gestao.html:495,528–534`, `app/chamados/views.py:870–890,928–934`, `app/chamados/services/fio.py:97–117,136–156,242–273,291–322,334–362`, `app/templates/chamados/partials/modal_fio.html:123,157–160`, `app/chamados/utils.py:47–72`.
- **Pergunta fora da lista interativa.** A segunda aberta do mesmo container fica fora da carga da lista, mesmo quando seu destino difere. Disputa sem container pode ser eleita pelo destino e servir à explicação, sem entrar na lista interativa. A gaveta de detalhes pode exibir texto e dia de itens não resolvidos da disputa eleita, inclusive quando o explicador recarrega as perguntas da segunda aberta. O fio de página inteira renderiza a conversa livre e os links do GLPI. A timeline GLPI recebe os followups externos e renderiza texto livre. O modal aceita cartões GLPI. `app/chamados/services/fio.py:251–273,335–346,421–436`, `app/chamados/services/explicador.py:98–150,166–186`, `app/templates/chamados/partials/modal_fio.html:461–476`, `app/chamados/tests/test_e4c_f5_82c3c_cartao_misto.py:217–234`, `app/templates/chamados/fio_colaborador.html:234–285`, `app/chamados/views.py:243–269`, `app/templates/chamados/detalhe.html:69–103,116–143`.
- **Portas alternativas.** A prévia da Central mostra pergunta e resposta para a classe A da fila global de respondidas vivas, com confirmação do gestor autorizado. O inline valida a disputa aberta eleita e usualmente opera sem mostrar cada pergunta. Para contar, separe texto disponível, ação alcançável e invisibilidade real, por `(chamado, pergunta)` e depois por chamados distintos com ao menos um bloqueador invisível. `app/chamados/juizes.py:1863–1868`, `app/chamados/services/validacao.py:1283–1301,1336–1357`, `app/chamados/views_cobrar.py:145–172,180–226,370–404`, `app/templates/chamados/painel_gestao.html:14`, `app/templates/chamados/partials/_lote_preview.html:20–26`.
- **Duas guardas.** `ancora` é o filtro pela mesma âncora, na competência aberta e no pós-estado. `conversa_pai` é a resposta sem veredito nas disputas próprias ou no destino, sem que a etiqueta prove `p.conversa`. O gate registra IDs em nota quando `registrar=True`. Aplicar C6 também a essa lista pode omitir um bloqueador de CSF cujo C6 dê morto. `app/chamados/models.py:332–388,718–758,2041–2045`, `app/ponto/supra_juiz.py:552–575`.

**C4. A divisão das 260 disputas e seus colabs ficou [nao verificado].** `listar_c4` em `sonda.py` mede aberta há mais de 30 dias e usa datas de todas as perguntas ou o fato forte do chamado, mantendo também casos sem data. A saída só identifica idade e disputa. No código, o relatório análogo mede idade e motivos, e o fechamento exige zero C6 vivas e zero respostas sem veredito. Portanto há pelo menos três classes a testar. `app/chamados/management/commands/auditar_disputas.py:132–144`, `app/chamados/models.py:1930–1931,2000–2053`, `app/chamados/juizes.py:1603–1606`.

1. **Produção, aguarda resposta.** Porta do colaborador `Enviar resposta` no PWA ou a pendente no nativo. O nativo consulta C6 por colaborador, sem depender do container, e separa a pergunta sem resposta e de disputa aberta. A supervisão pode cobrar pelo modal. `app/chamados/views.py:560–570,1229–1247`, `app/templates/chamados/partials/_card_atendimento.html:73–78,163–165`, `app/chamados/services/pergunta_no_app.py:45–92`, `app/api/views.py:2538–2555,2609–2640`, `app/templates/chamados/partials/modal_fio.html:363–380`.
2. **Produção, aguarda veredito.** Validar, rejeitar ou corrigir conforme a pergunta e as guardas do modal, ou validar pelas portas alternativas da Central. Pergunta renderizada com ação suprimida e sem alternativa real passa à classe Hasner. `app/templates/chamados/partials/modal_fio.html:217–250,274–276,306–340`, `app/chamados/views.py:952–955`, `app/chamados/services/fio.py:694–709,721–760,776–790`, `app/chamados/views_cobrar.py:145–226,370–404`.
3. **Hasner sem saída, ou disputa já fechável pelo C7 a desempatar.** Se a retenção carece de porta para ambos os atores, é Hasner. Se `pode_fechar()` passa, separe vazia, mortas e eventual fecho/regularização pendente. A presença do botão Regularizar pode terminar em “Nenhuma pergunta validada para materializar”; a mera presença do botão não encerra o diagnóstico. `admin_encerra` existe no modelo para silêncio, ligação a UI [nao verificado]. `app/chamados/models.py:2047–2069,2210–2217`, `app/chamados/services/becos.py:25–35,56–61`, `app/chamados/services/vigia_fio.py:736–743`, `app/templates/chamados/partials/modal_fio.html:382–389`, `app/chamados/services/fio.py:797–824,847–848`.

**Fatias propostas.** Atribuição à obra da sonda é triagem [nao verificado]. A obra `JUIZ-PERGUNTA-VIVA-MUDA-SEM-FATO` tem causa histórica específica, hoje com releitura da disputa e validação atômica. A divergência visual isolada pode ser “sem obra”; registrar o censo em `REGISTRA-ACHADOS` é proposta [nao verificado]. `app/chamados/services/validacao.py:319–327,734–746`, `app/docs/BACKLOG.md:1209`.

- **C2/C4 apresentação, dupla.** Em `acoes_chamado.py` e `detalhe_local.html`/`detalhe.html`, indicar o bloqueador pelo ID e oferecer acesso ao cartão que o lista, preservando RBAC. Se a própria carga omite o bloqueador, ajustar `services/fio.py` e `partials/modal_fio.html` para renderizá-lo sob âncora válida, inclusive quando a disputa escolhida no cabeçalho difere. RED com dois pais no mesmo destino, duas abertas no mesmo container, sem container, GLPI, e gestor sem departamento autorizado. Provar texto e ação sobre o ID certo, mantendo a eleição do cabeçalho e os acessos. `app/chamados/services/fio.py:136–156,251–273,319–348,721–760`, `app/chamados/tests/test_e4c_f5_82c3c_cartao_misto.py:217–234`.
- **C4 dia alterado, mão única se alcançar célula/ponto.** Hipótese `PERGUNTA-VIVA-NAO-MORRE-NO-DIA-MUDADO` [nao verificado]. Só abrir com prova de antes/depois, distinguindo dia do turno de data de conversa, marca respondida de marco vizinho e efeito da ata. RED muda o fato do dia e testa C6, C7 e ausência de cobrança indevida com trilha, mantendo viva a irmã de outro marco. `app/chamados/juizes.py:339–343,1499–1589`, `app/chamados/services/relavra_via.py:187–207,236–243`, `app/ponto/services/veredito_celula.py:160–203`.

## 27/11 19:54 · sessao 098942e39778 · - · - · origem=Ronald (tmux)

- A porta oficial calcula o fechamento em memória e passa os mesmos objetos a `classificar_export` e `montar_linhas_export`. O recálculo precisa de `somente_leitura=True, retornar_objeto=True, lavrar_dias_pagos=False`. Há exceções internas que continuariam após falha, inclusive com log nominal. Interceptar essas exceções e confirmar que os IDs retornados cobrem todo o universo evita uma prévia parcial. [app/folha/views.py:214] [app/folha/views.py:275] [app/ponto/services/fechamento.py:402] [app/ponto/services/fechamento.py:1105] [app/ponto/services/fechamento.py:1111] [app/ponto/services/fechamento.py:1215] [app/ponto/services/fechamento.py:1327]
- O TXT carrega a quantidade inteira e o código da rubrica. O tipo vem do mapa efetivo e vários eventos podem convergir para a mesma rubrica. Converta cada evento antes de agregar e confronte todos os registros `10`. `falta_diurna` usa datas de ausências aprovadas e o FM tem apenas `horas_falta`. A presença de `detalhe.falta_min` no `DiaPago` é evidência diária separada e inclui falta de zero minuto. [app/folha/export.py:34] [app/folha/export.py:169] [app/folha/export.py:224] [app/folha/export.py:439] [app/folha/export.py:739] [app/folha/export.py:993] [app/ponto/models.py:427] [app/ponto/services/dia_pago.py:274]
- A identidade ausente deve permanecer sem pareamento, com `col<pk>` disponível no `RUB` para localizar o vínculo. Antes da junção, valide a normalização por empresa e fonte, rejeitando duas grafias DP distintas colapsadas na mesma chave e dois colaboradores Hasner colapsados no mesmo código. Conserve a presença de cada fonte, inclusive zero explícito, e use a cobertura para distinguir ausência de lançamento, ausência de recibo e recorte não selecionado. [formato_csv.md:3] [formato_csv.md:7] [app/folha/export.py:737] [app/folha/export.py:881]
- Na agregação, use `sum(delta)` e `sum(abs(delta))` por unidade. METODO descreve a diferença DP–TXT nas referências padronizadas e a diferença FM–TXT precisa de categoria própria. Rubricas de DSR e ausência têm evidências distintas, e `datas_abonadas` não é chamada pela rotina que monta a prévia. [app/folha/export.py:201] [app/folha/export.py:453] [app/folha/export.py:739] [nao verificado] para os resultados da competência.

## 27/11 19:54 · sessao fd87f6bca18e · - · - · origem=Ronald (tmux)

**C1 tem 38 linhas marcadas, de 22 colabs.** A sonda agrupa ausência de porta e ausência de leitura na mesma marca, mas publica `fio` distinto. O PK do chamado é só o primeiro elemento de `fila`. `sonda.py:98–111`, `app/folha/fora_da_folha.py:86–91`.

| Motivo e fio | Linhas / colabs | Triagem |
|---|---:|---|
| `indefinida / ninguem` | 15 / 6 | Previsto com dia indefinido, fase não declarada ou parcial sem período. A causa exata e a eventual porta de cadastro ficam [nao verificado]. `app/escala/models.py:1626`, `app/chamados/services/vigia_fio.py:268–278`. |
| `furo / ninguem` | 1 / 1 | A tarefa ficou sem rota. O protesto específico, o estado da pergunta e a rota alternativa ficam [nao verificado]. `app/escala/models.py:1623`, `app/chamados/services/vigia_fio.py:654–706`. |
| `furo / nao_sei`; `cobrado / nao_sei` | 2 / 2; 1 / 1 | Falta leitura da tarefa. Causa interna [nao verificado]. A tela do retrato exibe por data `Sem porta` e omite a marca distinta `dias_sem_leitura`. `app/chamados/services/tarefa_do_dia.py:236–246`, `app/chamados/services/tarefa_do_dia.py:304–310`, `app/folha/retrato_folha.py:1244`, `app/templates/folha/_retrato_o_que_fazer.html:46`. |
| `discordante / colab`; `discordante / admin` | 4 / 4; 8 / 5 | Há porta publicada. Colab responde pendência do app. Admin tem ação exigível conforme a tarefa, que pode ser validar/decidir no fio, resolver dia ou decidir ausência. A pertinência da ação por linha fica [nao verificado], pois a divergência agrega órfã, inflado e realizado zero. `app/escala/models.py:1627`, `app/api/views.py:2538–2555`, `app/api/views.py:2634–2656`, `app/folha/retrato_folha.py:1016–1020`. |
| `cobranca_indevida / ninguem`; `cobranca_indevida / admin` | 2 / 2; 5 / 2 | A cobrança sobrevive ao furo ou ao dia de trabalho. As duas primeiras têm beco na tarefa; as cinco têm porta publicada. Distinguir passivo ou erro do cartório de conversa recebida que exige decisão humana é [nao verificado]. O cartório filtra o chamado que não pode protestar antes de julgar. `app/ponto/supra_juiz.py:479–484`, `app/ponto/supra_juiz.py:507–512`, `app/ponto/services/cartorio.py:1498–1508`, `app/ponto/services/lastro.py:207–228`. |

As 18 linhas `ninguem` exigem cruzamento por dia com ausência aberta, folga contestada validada, Cadastro x Realidade e Pauta DP. A ausência dessas saídas caracteriza beco Hasner. A sonda C1 omite esse cruzamento. `sonda.py:98–111`, `app/chamados/services/vigia_fio.py:252–279`, `app/chamados/services/vigia_fio.py:650–706`.

As 1.000 linhas `aval=-` cabem em três linhas:

- `espelho_*` com fio `colab` ou `admin` tem porta da tarefa. Responder no app, validar/decidir no fio, Resolver dia no calendário ou decidir ausência, conforme a ação publicada. `app/chamados/services/tarefa_do_dia.py:748–790`, `app/templates/chamados/partials/modal_fio.html:306`, `app/templates/colaboradores/partials/_calendario_grade.html:134`, `app/ponto/views.py:2095`.
- `sem_codigo_dominio` tem porta humana. Editar colaborador, preencher Código folha e salvar. `app/folha/export.py:881`, `app/templates/colaboradores/partials/painel_colaborador.html:155`, `app/templates/colaboradores/editar.html:62`, `app/colaboradores/views.py:1353–1365`.
- `ferias_com_batida` exige conferência do DP. Aba Férias oferece corrigir datas ou lançamento quando o cadastro estiver errado. A ação correta para a batida do anexo fica [nao verificado]. `app/folha/export.py:885–893`, `app/templates/colaboradores/partials/painel_colaborador.html:43`, `app/templates/ferias/_secao_colab.html:145–171`.

Propostas de fatia com dono formal [nao verificado]:

- **REGISTRA-ACHADOS**. Na sonda, registrar por dia os protestos, a razão e as instâncias da tarefa, a ação e a rota e o resultado de `dono_fora_do_fio`. RED com dois dias `ninguem`, um alcançado por Cadastro x Realidade e um fora da lista, que hoje saem com o mesmo fio. Mão dupla. `sonda.py:98–111`, `app/chamados/services/vigia_fio.py:252–279`.
- **REGISTRA-ACHADOS**. No drawer do retrato, exibir `dias_sem_leitura` por data. RED em dia sem leitura na mesma linha de outro dia com porta, preservando a distinção de `Sem porta`. Exceção à mão dupla por tocar Folha. `app/folha/retrato_folha.py:1244`, `app/templates/folha/_retrato_o_que_fazer.html:46`.
- **RECONCILIAR-SEM-GATILHO**, se o diagnóstico reproduzir cobrança com premissa morta apta à reconciliação. RED com os dois ramos da cobrança indevida, e controle de conversa divergente ainda à espera do admin. A alteração no cartório ou na retenção fica na exceção à mão dupla. `app/ponto/services/cartorio.py:1498–1508`, `app/folha/export.py:1282–1303`, `app/ponto/services/lastro.py:207–228`.

_8788 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
