# CODEX-ESTADO

> Ultima resposta processada: **27/11/2026 11:44** · publicado em **27/11/2026 11:44** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 27/11 11:44 · sessao ed0d5daab456 · FERIADO-NAO-PREVISTO-L086 · sonda · origem=Code

# FERIADO-NAO-PREVISTO-L086 — medida na sombra

## Totais por empresa

O universo da competência **11/2026**, de **21/10 a 20/11**, reúne os dias em que `feriados_do_colab` inclui a data e a célula tem `trabalha=False`. Os totais de horas cobrem os registros diários `DiaPago` da versão `motor` com `horas_trabalhadas > 0`. `r.md:28–36` `r.md:41–89` `app/core/feriados.py:135–167`

Nas referências, `saida-sonda.tsv` é a saída raspada da mensagem, numerada a partir de `sonda_leitura` na linha 1. As colunas de noturnas são somas dos valores decimais impressos nos detalhes. O resumo da sonda imprime os totais de trabalhadas e de `extras_100`. `saida-sonda.tsv:1–91` `r.md:126–143`

| Empresa | Dias-colab no universo | Com trabalhadas > 0 | Com lavra e trabalhadas ≤ 0 | Sem lavra | Horas trabalhadas | Horas noturnas | Horas `extras_100` | Horas `extras_100_noturna` | Fonte |
|---:|---:|---:|---:|---:|---:|---:|---:|---:|---|
| 2 | 753 | 69 | 72 | 612 | 505.6704230627778 | 21.15548214285714177 | 0.0 | 0.0 | `saida-sonda.tsv:5–73,88` |
| 3 | 182 | 13 | 12 | 157 | 111.14582922305556 | 29.699891428571429 | 0.0 | 0.0 | `saida-sonda.tsv:74–86,89` |
| 4 | 39 | 0 | 9 | 30 | 0.0 | 0.0 | 0.0 | 0.0 | `saida-sonda.tsv:87–90` |

O universo total impresso é **974 dias-colab**. As **82 linhas** detalhadas têm `extras_100 = 0.0` e `extras_100_noturna = 0.0`. `saida-sonda.tsv:5–86,91`

## Colab 1036 em 20/11

**Sim, aparece na empresa 2.** A linha traz **7.28502 h trabalhadas**, **1.163055238095238 h noturnas**, **0.0 h de `extras_100`** e **0.0 h de `extras_100_noturna`**. Os valores conferem com **7,29 / 1,16 / 0,0** do aval, com as duas primeiras medidas arredondadas a duas casas. `saida-sonda.tsv:72` `aval.txt:1`

## Horário e limites da medida

- **`medido_em = 2026-11-27T11:40:09-03:00`**. A sonda registra esse horário antes de abrir a transação de leitura. A rotina da cópia aponta para o backup diário das **04:00**, então **27/11 às 04:00** é uma hipótese se a rotina seguiu o fluxo diário. O código também prevê `--dump-agora` e atualização incremental. A data e a hora efetivas da base lida estão **[nao verificado]**. `saida-sonda.tsv:2` `r.md:113–119` `app/config/crons.py:182` `bin/sombra.sh:194–215,236–253,299–305`
- **HE100 no restante do universo [nao verificado].** A saída só conta os dias com lavra e `horas_trabalhadas <= 0` e os dias sem lavra. O código pode lançar horas diretamente em HE100 de feriado sem somá-las a `horas_trabalhadas`, deixando o dia fora dos detalhes. O campo específico `horas_extras_100_feriado` também ficou fora da consulta. `saida-sonda.tsv:87–90` `r.md:66–98` `app/ponto/models.py:326–332` `app/ponto/services/dia_pago.py:241–253`
- **Causa do zero de 1036 [nao verificado].** A saída traz a lavra diária. O conjunto de feriados recebido na apuração original, `datas_previstas_trabalho`, as batidas, a régua aplicada e o resultado do crivo de escala certa para esse dia ficam fora da leitura. `r.md:55–76` `app/ponto/services/fechamento.py:953–974` `aval.txt:1`
- **Valores atuais em produção, resultado de recálculo, TXT e pagamento efetivo [nao verificado].** A sonda consulta o `DiaPago` gravado na sombra. A geração do TXT na cópia usa os objetos do fechamento mensal. `r.md:66–76,103–124` `app/folha/views.py:214` `app/folha/export.py:420`

## Lista completa com `horas_trabalhadas > 0`

A lista está ordenada por empresa, data e ID do colaborador. O negrito marca `extras_100 = 0.0`. A empresa 4 tem zero linhas nesse recorte. `saida-sonda.tsv:5–90`

| Empresa | Data | Colab ID | Trabalhadas (h) | Noturnas (h) | `extras_100` (h) | Fonte |
|---:|---|---:|---:|---:|---:|---|
| 2 | 2026-11-02 | 43 | 4.088383333333334 | 0.0 | **0.0** | `saida-sonda.tsv:5` |
| 2 | 2026-11-02 | 203 | 7.069715555555556 | 0.0 | **0.0** | `saida-sonda.tsv:6` |
| 2 | 2026-11-02 | 207 | 5.991126388888889 | 0.0 | **0.0** | `saida-sonda.tsv:10` |
| 2 | 2026-11-02 | 278 | 7.0710894444444445 | 0.0 | **0.0** | `saida-sonda.tsv:13` |
| 2 | 2026-11-02 | 365 | 12.0 | 0.0 | **0.0** | `saida-sonda.tsv:16` |
| 2 | 2026-11-02 | 366 | 10.998848333333335 | 5.997973333333333 | **0.0** | `saida-sonda.tsv:19` |
| 2 | 2026-11-02 | 369 | 7.0080025 | 0.0 | **0.0** | `saida-sonda.tsv:20` |
| 2 | 2026-11-02 | 384 | 7.31101361111111 | 0.0 | **0.0** | `saida-sonda.tsv:22` |
| 2 | 2026-11-02 | 583 | 8.078643055555556 | 0.0 | **0.0** | `saida-sonda.tsv:26` |
| 2 | 2026-11-02 | 622 | 6.974738382777777 | 0.0 | **0.0** | `saida-sonda.tsv:27` |
| 2 | 2026-11-02 | 784 | 4.133950277777777 | 0.0 | **0.0** | `saida-sonda.tsv:32` |
| 2 | 2026-11-02 | 788 | 16.0160475 | 1.1082016666666665 | **0.0** | `saida-sonda.tsv:34` |
| 2 | 2026-11-02 | 789 | 7.3611770241666665 | 0.0 | **0.0** | `saida-sonda.tsv:35` |
| 2 | 2026-11-02 | 822 | 7.177437222222222 | 0.9663374603174603 | **0.0** | `saida-sonda.tsv:38` |
| 2 | 2026-11-02 | 830 | 12.066969811944444 | 0.0 | **0.0** | `saida-sonda.tsv:41` |
| 2 | 2026-11-02 | 961 | 7.146785555555555 | 0.0 | **0.0** | `saida-sonda.tsv:51` |
| 2 | 2026-11-02 | 962 | 7.5270697222222225 | 0.0 | **0.0** | `saida-sonda.tsv:53` |
| 2 | 2026-11-02 | 967 | 7.394691666666666 | 0.0 | **0.0** | `saida-sonda.tsv:55` |
| 2 | 2026-11-02 | 968 | 6.9958141666666664 | 0.9541666666666667 | **0.0** | `saida-sonda.tsv:56` |
| 2 | 2026-11-02 | 975 | 3.9582752777777777 | 0.0 | **0.0** | `saida-sonda.tsv:58` |
| 2 | 2026-11-02 | 988 | 7.342193096111111 | 0.0 | **0.0** | `saida-sonda.tsv:59` |
| 2 | 2026-11-02 | 990 | 7.317387678888889 | 0.0 | **0.0** | `saida-sonda.tsv:61` |
| 2 | 2026-11-02 | 991 | 7.042927222222222 | 0.0 | **0.0** | `saida-sonda.tsv:62` |
| 2 | 2026-11-15 | 203 | 7.1011894444444446 | 0.0 | **0.0** | `saida-sonda.tsv:7` |
| 2 | 2026-11-15 | 207 | 6.187036434444445 | 0.0 | **0.0** | `saida-sonda.tsv:11` |
| 2 | 2026-11-15 | 278 | 7.403098888888889 | 0.0 | **0.0** | `saida-sonda.tsv:14` |
| 2 | 2026-11-15 | 365 | 11.036849166666666 | 0.0 | **0.0** | `saida-sonda.tsv:17` |
| 2 | 2026-11-15 | 369 | 6.9213000000000005 | 0.0 | **0.0** | `saida-sonda.tsv:21` |
| 2 | 2026-11-15 | 384 | 6.144064166666666 | 0.0 | **0.0** | `saida-sonda.tsv:23` |
| 2 | 2026-11-15 | 622 | 4.0098577777777775 | 0.0 | **0.0** | `saida-sonda.tsv:28` |
| 2 | 2026-11-15 | 735 | 3.9637504116666666 | 0.0 | **0.0** | `saida-sonda.tsv:30` |
| 2 | 2026-11-15 | 789 | 7.428601666666666 | 0.0 | **0.0** | `saida-sonda.tsv:36` |
| 2 | 2026-11-15 | 822 | 8.112541666666667 | 1.065664761904762 | **0.0** | `saida-sonda.tsv:39` |
| 2 | 2026-11-15 | 830 | 11.885994444444444 | 0.0 | **0.0** | `saida-sonda.tsv:42` |
| 2 | 2026-11-15 | 899 | 8.89184510277778 | 0.0 | **0.0** | `saida-sonda.tsv:44` |
| 2 | 2026-11-15 | 903 | 4.243828611111111 | 0.0 | **0.0** | `saida-sonda.tsv:46` |
| 2 | 2026-11-15 | 944 | 6.867059856944445 | 0.0 | **0.0** | `saida-sonda.tsv:49` |
| 2 | 2026-11-15 | 968 | 7.0078652777777775 | 0.9576526984126983 | **0.0** | `saida-sonda.tsv:57` |
| 2 | 2026-11-15 | 991 | 7.026201944444444 | 0.0 | **0.0** | `saida-sonda.tsv:63` |
| 2 | 2026-11-15 | 1000 | 3.9718216666666666 | 0.0 | **0.0** | `saida-sonda.tsv:65` |
| 2 | 2026-11-15 | 1023 | 8.376993055555555 | 0.0 | **0.0** | `saida-sonda.tsv:67` |
| 2 | 2026-11-15 | 1026 | 7.02381 | 0.956779365079365 | **0.0** | `saida-sonda.tsv:68` |
| 2 | 2026-11-20 | 203 | 7.032373333333334 | 0.0 | **0.0** | `saida-sonda.tsv:8` |
| 2 | 2026-11-20 | 206 | 4.199098888888889 | 0.0 | **0.0** | `saida-sonda.tsv:9` |
| 2 | 2026-11-20 | 207 | 6.012959166666666 | 0.0 | **0.0** | `saida-sonda.tsv:12` |
| 2 | 2026-11-20 | 278 | 7.165294444444444 | 0.0 | **0.0** | `saida-sonda.tsv:15` |
| 2 | 2026-11-20 | 365 | 11.008139166666666 | 0.0 | **0.0** | `saida-sonda.tsv:18` |
| 2 | 2026-11-20 | 384 | 7.568888055555556 | 0.0 | **0.0** | `saida-sonda.tsv:24` |
| 2 | 2026-11-20 | 415 | 6.995728888888889 | 0.0 | **0.0** | `saida-sonda.tsv:25` |
| 2 | 2026-11-20 | 727 | 6.987754444444445 | 0.0 | **0.0** | `saida-sonda.tsv:29` |
| 2 | 2026-11-20 | 735 | 4.312736620277778 | 0.0 | **0.0** | `saida-sonda.tsv:31` |
| 2 | 2026-11-20 | 784 | 3.956977222222222 | 0.0 | **0.0** | `saida-sonda.tsv:33` |
| 2 | 2026-11-20 | 789 | 3.544456188888889 | 0.0 | **0.0** | `saida-sonda.tsv:37` |
| 2 | 2026-11-20 | 822 | 7.05717611111111 | 0.9719514285714285 | **0.0** | `saida-sonda.tsv:40` |
| 2 | 2026-11-20 | 830 | 12.009573333333332 | 0.0 | **0.0** | `saida-sonda.tsv:43` |
| 2 | 2026-11-20 | 899 | 7.019682479444445 | 0.0 | **0.0** | `saida-sonda.tsv:45` |
| 2 | 2026-11-20 | 903 | 8.047641666666667 | 0.0 | **0.0** | `saida-sonda.tsv:47` |
| 2 | 2026-11-20 | 924 | 3.115772222222222 | 0.0 | **0.0** | `saida-sonda.tsv:48` |
| 2 | 2026-11-20 | 944 | 6.701955833333332 | 0.0 | **0.0** | `saida-sonda.tsv:50` |
| 2 | 2026-11-20 | 961 | 7.668902500000001 | 0.0 | **0.0** | `saida-sonda.tsv:52` |
| 2 | 2026-11-20 | 962 | 7.2847275 | 0.0 | **0.0** | `saida-sonda.tsv:54` |
| 2 | 2026-11-20 | 988 | 10.377472973611113 | 0.0 | **0.0** | `saida-sonda.tsv:60` |
| 2 | 2026-11-20 | 991 | 7.0027872222222225 | 0.0 | **0.0** | `saida-sonda.tsv:64` |
| 2 | 2026-11-20 | 1000 | 3.9380341666666667 | 0.0 | **0.0** | `saida-sonda.tsv:66` |
| 2 | 2026-11-20 | 1026 | 7.228945 | 0.9553495238095238 | **0.0** | `saida-sonda.tsv:69` |
| 2 | 2026-11-20 | 1031 | 10.85031111111111 | 6.014245833333333 | **0.0** | `saida-sonda.tsv:70` |
| 2 | 2026-11-20 | 1035 | 11.299347000833333 | 0.04410416666666667 | **0.0** | `saida-sonda.tsv:71` |
| 2 | 2026-11-20 | 1036 | 7.28502 | 1.163055238095238 | **0.0** | `saida-sonda.tsv:72` |
| 2 | 2026-11-20 | 1050 | 8.322671111111113 | 0.0 | **0.0** | `saida-sonda.tsv:73` |
| 3 | 2026-11-02 | 85 | 11.897632222222223 | 10.16872253968254 | **0.0** | `saida-sonda.tsv:76` |
| 3 | 2026-11-02 | 114 | 7.004955277777778 | 0.0 | **0.0** | `saida-sonda.tsv:78` |
| 3 | 2026-11-02 | 146 | 13.50255722222222 | 9.765453015873016 | **0.0** | `saida-sonda.tsv:81` |
| 3 | 2026-11-02 | 749 | 7.395433621944446 | 0.0 | **0.0** | `saida-sonda.tsv:82` |
| 3 | 2026-11-15 | 82 | 7.559408888888888 | 0.0 | **0.0** | `saida-sonda.tsv:74` |
| 3 | 2026-11-15 | 114 | 8.323086386944444 | 0.0 | **0.0** | `saida-sonda.tsv:79` |
| 3 | 2026-11-15 | 911 | 6.466329444444445 | 0.0 | **0.0** | `saida-sonda.tsv:83` |
| 3 | 2026-11-15 | 1024 | 7.3869547580555555 | 0.0 | **0.0** | `saida-sonda.tsv:85` |
| 3 | 2026-11-20 | 82 | 7.109137536666666 | 0.0 | **0.0** | `saida-sonda.tsv:75` |
| 3 | 2026-11-20 | 85 | 11.545001388888888 | 9.765715873015873 | **0.0** | `saida-sonda.tsv:77` |
| 3 | 2026-11-20 | 114 | 7.974111795277778 | 0.0 | **0.0** | `saida-sonda.tsv:80` |
| 3 | 2026-11-20 | 911 | 7.600584166666666 | 0.0 | **0.0** | `saida-sonda.tsv:84` |
| 3 | 2026-11-20 | 1024 | 7.3806365130555545 | 0.0 | **0.0** | `saida-sonda.tsv:86` |

## 27/11 11:41 · sessao 004a35688bb5 · FERIADO-NAO-PREVISTO-L086 · leitura · origem=Code

## (a) De onde vêm os feriados da competência

**Na cópia, o fechamento e o calendário chamam `feriados_do_colab`.** A chave territorial do conjunto é o **município do posto do vínculo de cada dia**. A referência à “praça do colab” em `MotorBase.eh_feriado` está na docstring. O corpo do método consulta o conjunto recebido. `app/ponto/services/fechamento.py:668`, `app/colaboradores/services/calendario.py:347`, `app/core/feriados.py:131`, `app/core/feriados.py:160`, `app/ponto/motor_calculo_v2.py:1003`, `app/ponto/motor_calculo_v2.py:1007`.

1. `recalcular_fechamento_mes` resolve a janela da competência pelo corte da **empresa**. Para os feriados, carrega um envelope que cobre os cortes 2 a 28, com um dia a mais em cada ponta. Para cada colaborador, chama `feriados_do_colab` com as linhas de feriado e as células carregadas em lote. `app/ponto/services/fechamento.py:468`, `app/ponto/services/fechamento.py:604`, `app/ponto/services/fechamento.py:615`, `app/ponto/services/fechamento.py:625`, `app/ponto/services/fechamento.py:641`, `app/ponto/services/fechamento.py:668`.
2. `feriados_do_colab` resolve `vinculo_do_dia` → `lugar_do_vinculo` → `vinculo.posto.municipio` → `feriados_de`, com cache por `Municipio.pk`. `Posto.praca` e `Posto.municipio` são FKs distintas. A praça entra na resolução da **CCT**, pelo vínculo na data de referência da régua, passada pelo fechamento como início da fatia. `app/core/feriados.py:124`, `app/core/feriados.py:160`, `app/core/feriados.py:164`, `app/colaboradores/models.py:230`, `app/colaboradores/models.py:231`, `app/ponto/services/fechamento.py:756`, `app/core/regua_cct.py:329`, `app/core/regua_cct.py:330`, `app/core/regua_cct.py:458`.
3. `apurar_por_fatia` chama `_montar_motor` e depois `motor.calcular_mes`. `_montar_motor` passa `feriados=` a `get_motor_cct`, que o repassa a `get_motor`, que o entrega ao construtor. `MotorBase` copia o conjunto para `self.feriados` e guarda o mesmo objeto em `_feriados_injetados`. Data presente retorna `True`. Data ausente com esse objeto injetado retorna `False` na linha 1010, inclusive se o conjunto for vazio. O `MotorIntermitente` substitui ambos por um conjunto vazio. `app/ponto/services/fechamento.py:298`, `app/ponto/services/fechamento.py:306`, `app/ponto/services/fechamento.py:755`, `app/ponto/services/fechamento.py:758`, `app/core/regua_cct.py:457`, `app/core/regua_cct.py:479`, `app/ponto/motor_calculo_v2.py:815`, `app/ponto/motor_calculo_v2.py:822`, `app/ponto/motor_calculo_v2.py:1007`, `app/ponto/motor_calculo_v2.py:1011`, `app/ponto/motor_calculo_v2.py:3698`, `app/ponto/motor_calculo_v2.py:3815`.
4. O resultado alimenta `FechamentoMensal` e a lavra diária `DiaPago(versao='motor')`. A geração do TXT chama a **mesma apuração** em `somente_leitura=True, retornar_objeto=True`, passa os objetos mensais a `montar_linhas_export` e lê deles as HE 100% para as rubricas. O calendário lê a linha diária já gravada. `app/ponto/services/fechamento.py:942`, `app/ponto/services/fechamento.py:1168`, `app/ponto/services/fechamento.py:1184`, `app/ponto/services/fechamento.py:1225`, `app/ponto/services/fechamento.py:1251`, `app/folha/views.py:215`, `app/folha/views.py:275`, `app/folha/views.py:288`, `app/folha/export.py:420`, `app/folha/export.py:739`, `app/colaboradores/services/calendario.py:319`, `app/colaboradores/services/calendario.py:688`.

## (b) O universo dos dois conjuntos

`feriados_do_colab` usa a geradora da célula se o vínculo for encontrado e a data estiver dentro da sua vigência. Se a célula nomeia uma geradora ausente ou fora das datas, o juiz devolve `None`. Sem geradora, escolhe entre os vínculos que cobrem a data, priorizando ativo e depois início mais recente. O município vem do posto desse vínculo. Sem município, só o nacional alcança o dia. `app/escala/alimentacao.py:214`, `app/escala/alimentacao.py:269`, `app/escala/alimentacao.py:279`, `app/escala/alimentacao.py:286`, `app/core/feriados.py:57`, `app/core/feriados.py:131`, `app/core/feriados.py:161`.

`feriados_de` aplica **um município fixo** à janela. O nacional vale para todos, o estadual exige a UF e o municipal exige UF e município normalizado. `feriados_do_colab` retém, para cada data, a resposta desse mesmo filtro no município daquele dia. Se o município permanece o mesmo, com a mesma janela e as mesmas linhas, os conjuntos coincidem. Numa troca de município, o conjunto fixo pode incluir datas do posto antigo após a troca e omitir datas do posto novo. Com `carregados=`, `feriados_de` confia no recorte entregue, enquanto `feriados_do_colab` só adiciona dias que percorre na janela pedida. `app/core/feriados.py:27`, `app/core/feriados.py:32`, `app/core/feriados.py:57`, `app/core/feriados.py:81`, `app/core/feriados.py:150`, `app/core/feriados.py:160`.

## (c) O que chega em `datas_previstas_trabalho`

O fechamento inicia um `set()` por colaborador. Dentro de cada fatia, `_montar_motor` só adiciona a data se `esc.eh_dia_trabalho(data)` for verdadeiro e ela estiver fora das datas suprimidas. `eh_dia_trabalho` retorna `cel.trabalha` quando há célula. Logo, **com a célula fornecida, 20/11 fica fora do conjunto e `_previstas_fornecidas=True`**, pois o construtor recebeu um conjunto, mesmo se vazio. O motor de dias órfãos também recebe um conjunto vazio explícito. A origem e o DNA concretos da célula do 1036 são [nao verificado]. `app/ponto/services/fechamento.py:726`, `app/ponto/services/fechamento.py:749`, `app/ponto/services/fechamento.py:760`, `app/ponto/services/fechamento.py:838`, `app/escala/models.py:1098`, `app/escala/models.py:1106`, `app/ponto/motor_calculo_v2.py:826`.

## (d) Onde o 100% pode ficar em zero

**O ramo que produziu o zero do 1036 é [nao verificado].** Os 7,29 h trabalhados e 1,16 h noturnos vêm do dado raspado. A lavra pode registrar ambos com HE 100% zero, inclusive quando o período vai para `ft_sem_escala`. Nesse caminho, a condição decisiva é `escala_certa_no_dia(...) == False` no filtro da linha **959**. O dado fornecido ainda permite outros caminhos. `app/ponto/services/fechamento.py:958`, `app/ponto/services/fechamento.py:1005`, `app/ponto/services/dia_pago.py:223`, `app/ponto/services/dia_pago.py:259`.

| Ordem da dobra direta | Valor no caso |
|---|---|
| 1. O cálculo usa o dia de início do turno. Nos cálculos por par, usa o mapa do juiz ou a data local da entrada crua quando falta a chave. Nos recálculos por dia, exige períodos no grupo. | Há batidas no recorte informado. O dia atribuído a cada par e a classe do motor são [nao verificado]. `app/ponto/motor_calculo_v2.py:976`, `app/ponto/motor_calculo_v2.py:3197`, `app/ponto/motor_calculo_v2.py:3442`, `app/ponto/motor_calculo_v2.py:3461`, `app/docs/LEIS.md:117`. |
| 2. O chamador exige `duracao > 0` ou `trab_dia > 0`. | A lavra mostra trabalhadas positivas. O valor na guarda do ramo que rodou é [nao verificado]. Os quatro chamadores são 12x36 por par, Turno Partido por dia, Comercial por par e Comercial por dia. `app/ponto/motor_calculo_v2.py:2925`, `app/ponto/motor_calculo_v2.py:3208`, `app/ponto/motor_calculo_v2.py:3385`, `app/ponto/motor_calculo_v2.py:3467`. |
| 3. `bool(self.feriado_em_dobra)`. Se falso, a expressão da linha 961 encerra a decisão. | [nao verificado]. O padrão vem do ciclo, com `12x36` no conjunto de feriado simples. A CCT ativa pode ligar a dobra para esse ciclo. `app/ponto/motor_calculo_v2.py:961`, `app/ponto/motor_calculo_v2.py:3740`, `app/ponto/motor_calculo_v2.py:3810`, `app/core/regua_cct.py:440`, `app/core/regua_cct.py:477`. |
| 4. `self.eh_feriado(dia)`. Data presente dá `True`. Data ausente no conjunto injetado dá `False` na linha 1010. | Para **20/11**, com os mesmos insumos do calendário e o conjunto preservado, dá `True` na linha 1008. O conjunto efetivo da lavra histórica e o dia consultado são [nao verificado]. O intermitente injeta vazio e retorna `False`. `app/ponto/services/fechamento.py:668`, `app/colaboradores/services/calendario.py:347`, `app/ponto/motor_calculo_v2.py:1007`, `app/ponto/motor_calculo_v2.py:1010`, `app/ponto/motor_calculo_v2.py:3698`. |

Se a dobra direta passa, os chamadores atribuem o trabalhado a `minutos_extra_100` e a `minutos_extra_100_feriado`. Para `resultado.periodos`, a lavra soma esses minutos divididos por 60 em `DiaPago.horas_extras_100` e no subconjunto de feriado. O leitor expõe `horas_extras_100` como `extras_100`. **A previsão fica fora da expressão da dobra**. A consulta a `datas_previstas_trabalho` na linha 996 pertence à folga semanal. `app/ponto/motor_calculo_v2.py:961`, `app/ponto/motor_calculo_v2.py:996`, `app/ponto/motor_calculo_v2.py:2927`, `app/ponto/motor_calculo_v2.py:3217`, `app/ponto/motor_calculo_v2.py:3387`, `app/ponto/motor_calculo_v2.py:3475`, `app/ponto/services/dia_pago.py:182`, `app/ponto/services/dia_pago.py:915`.

Para o caso com célula `False`, a sequência que pode mudar a rubrica depois da dobra direta é esta. `app/ponto/motor_calculo_v2.py:2558`, `app/ponto/services/fechamento.py:953`.

1. Com vínculo não intermitente e `data_jornada=20/11`, o `False` da célula faz `folga_do_vinculo=True` e o período vai para `periodos_ft`. Se foi esse o percurso do 1036, [nao verificado]. Antes da separação, a janela de HE pode zerar um par inteiro se houver folga ou feriado, janela de empresa vigente, previsto zero e ausência de autorização, com a exceção da folga sem escala certa. Um par zerado ali também perde seus minutos trabalhados e noturnos. Esses insumos por par são [nao verificado]. `app/ponto/motor_calculo_v2.py:1554`, `app/ponto/motor_calculo_v2.py:1563`, `app/ponto/motor_calculo_v2.py:1583`, `app/ponto/motor_calculo_v2.py:1587`, `app/ponto/motor_calculo_v2.py:2560`, `app/ponto/motor_calculo_v2.py:2574`, `app/ponto/motor_calculo_v2.py:2605`.
2. Em `periodos_ft`, **primeiro** passa pela guarda de escala certa. `LIMBO`, `A2` ou `A8` barram o período na linha 959. A lavra de `ft_sem_escala` conserva trabalhadas e noturnas e deixa o 100% desse período em zero. O motivo e a presença de `folga_sem_escala_min` na linha do 1036 são [nao verificado]. `app/ponto/services/fechamento.py:958`, `app/ponto/services/fechamento.py:1005`, `app/escala/services/escala_certa.py:31`, `app/escala/services/escala_certa.py:42`, `app/escala/services/escala_certa.py:51`, `app/ponto/services/dia_pago.py:259`.
3. **Depois**, se há vínculo e `feriado_vira_folga` dá `True`, o fechamento promove o período a HE 100% de feriado. O predicado exige `trabalha_em_feriado=False`, ciclo da lista `6x1`/`5x2`/`semanal` e feriado no município do posto. Se a promoção falha após passar pela escala certa, o período fica na rubrica de folga trabalhada. Vínculo, ciclo, chave e rubrica efetiva no caso são [nao verificado]. `app/ponto/services/fechamento.py:970`, `app/ponto/services/fechamento.py:979`, `app/escala/models.py:32`, `app/escala/models.py:1400`, `app/ponto/services/dia_pago.py:234`, `app/ponto/services/dia_pago.py:244`.

## (e) Autoridade e censo em `app/`

**São duas interfaces da mesma regra de alcance, com entradas diferentes.** `feriados_de` responde por município fixo. `feriados_do_colab` resolve o município da pessoa no dia e chama `feriados_de`. `JUIZES.md` e o registro vivo nomeiam `core.feriados.eh_feriado` para a existência do feriado. O próprio `eh_feriado` delega a `feriados_de`. A dobra é outra pergunta, registrada em `CICLOS_FERIADO_SIMPLES`. `app/core/feriados.py:70`, `app/core/feriados.py:101`, `app/core/feriados.py:135`, `app/core/feriados.py:164`, `app/docs/JUIZES.md:115`, `app/docs/JUIZES.md:130`, `app/core/juizes.py:196`, `app/core/juizes.py:320`.

| Função | Chamadas diretas na cópia |
|---|---|
| `feriados_de` | `eh_feriado` e `feriados_do_colab` em `app/core/feriados.py:105` e `app/core/feriados.py:164`. Fila de chamados em `app/chamados/services/fila_por_causa.py:134`. Dias úteis de VT/VA em `app/colaboradores/models.py:1003`. Consulta e presença por posto em `app/colaboradores/services/feriado_do_posto.py:57` e `app/colaboradores/services/feriado_do_posto.py:112`. |
| `feriados_do_colab` | Fechamento em `app/ponto/services/fechamento.py:668`, calendário em `app/colaboradores/services/calendario.py:347`, precedência em `app/ponto/precedencia.py:331`, espelho em `app/ponto/services/espelho.py:1065`, PDF em `app/relatorios/pdf_espelho.py:298`. Comandos em `app/ponto/management/commands/desvio_o68b.py:113`, `app/ponto/management/commands/diff_espelho_alimentacao.py:93`, `app/relatorios/management/commands/diff_extrato_parcial.py:58`. Testes em `app/chamados/tests/test_jv1_volta4_posto_do_dia.py:84`, `app/chamados/tests/test_jv1_volta4_posto_do_dia.py:93`, `app/chamados/tests/test_jv1_volta4_posto_do_dia.py:105`, `app/ponto/tests/test_caracterizacao_espelho_unit.py:120`, `app/ponto/tests/test_caracterizacao_espelho_unit.py:133`. |

**Nenhuma das duas funções deve sair sob a L-002.** A derivação paralela que a lei mandaria retirar seria uma escolha independente de praça fixa para responder pelo colaborador. Nessa rota, a chamada executável já usa o vínculo do dia. A própria L-086 registra que a cláusula original de feriado foi superada pela regra de ciclo e dia de início. A promoção de folga-feriado tem a guarda adicional de escala certa, registrada no BACKLOG como `FERIADO-SO-NA-ESCALA-CERTA`. `app/docs/LEIS.md:60`, `app/docs/LEIS.md:118`, `app/ponto/services/fechamento.py:668`, `app/ponto/motor_calculo_v2.py:944`, `app/docs/BACKLOG.md:2164`.

O BACKLOG registra o pouso de `FERIADO-POR-POSTO` sem P4 e a porta única **da marca**. Essa porta propaga `Posto.opera_em_feriado` para `vinculo.trabalha_em_feriado` nos vínculos elegíveis. O corte da O275 restringe “trabalha em feriado” às escalas semanais e remete plantões à precedência existente. Esses registros tratam da escala, enquanto o alcance geográfico segue em `core.feriados`. `app/docs/BACKLOG.md:1984`, `app/docs/CORTES.md:616`, `app/colaboradores/services/feriado_do_posto.py:179`, `app/colaboradores/services/feriado_do_posto.py:368`, `app/core/feriados.py:160`.

## (f) Sonda de leitura

Para distinguir os ramos, a sonda mostra o conjunto reconstruído com o cadastro atual, a chave de feriado do vínculo, o motivo do juiz de escala certa, a decisão vigente de HE e o `DiaPago` gravado. Na linha lavrada, `fd=` em `versao_motor` é o carimbo da régua e `folga_sem_escala_min` identifica os minutos que passaram pelo ramo sem escala certa. `folga_feriado_min` identifica a promoção e `o85_dia_sem_vinculo_min` identifica trabalhadas órfãs. A coincidência entre o cadastro atual e o da lavra histórica é [nao verificado]. `app/ponto/services/fechamento.py:304`, `app/core/regua_cct.py:421`, `app/ponto/services/dia_pago.py:257`, `app/ponto/services/dia_pago.py:266`, `app/ponto/services/dia_pago.py:269`, `app/ponto/services/dia_pago.py:342`, `app/ponto/models.py:1265`.

```python
# Envie o bloco ao manage.py shell no tenant do caso.
from datetime import date, timedelta

from django.db import connection, transaction

from colaboradores.models import Colaborador
from core.feriados import carregar, feriados_de, feriados_do_colab, lugar_do_vinculo
from escala.alimentacao import celulas_do_periodo, escalas_do_periodo, vinculo_do_dia
from escala.services.escala_certa import motivo_escala_errada
from ponto.janelas import janela_fechamento, periodo_apuracao
from ponto.models import DecisaoHE, DiaPago
from ponto.turnos import dia_das_batidas

cid, mes, ano = 1036, 11, 2026
a, d, b = (date(ano, mes, n) for n in (19, 20, 21))
um = timedelta(days=1)

with transaction.atomic():
    with connection.cursor() as cur:
        cur.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")

    col = Colaborador.objects.select_related("empresa").get(pk=cid)
    ini, fim = periodo_apuracao(mes, ano, 2)[0], periodo_apuracao(mes, ano, 28)[1]
    celulas = celulas_do_periodo([cid], ini, fim)
    vinculos = escalas_do_periodo(
        cid, ini - um, fim + um, mais=[c.escala_geradora_id for c in celulas.values()]
    )
    v = vinculo_do_dia(cid, d, vinculos, celulas=celulas)
    posto, mun = getattr(v, "posto", None), lugar_do_vinculo(v)
    fer = feriados_do_colab(
        col, ini - um, fim + um, celulas=celulas, vinculos=vinculos,
        carregados=carregar(ini - um, fim + um),
    )

    print("colab, empresa, janela", cid, col.empresa_id, janela_fechamento(mes, ano, col.empresa))
    print("motor e calendario (vinculo, posto, praca, municipio usado)",
          getattr(v, "pk", None), getattr(posto, "pk", None),
          getattr(posto, "praca_id", None), getattr(mun, "pk", None))
    print("feriados_de_municipio_fixo", sorted(feriados_de(mun, a, b)))
    print("feriados_do_colab_injecao", sorted(x for x in fer if a <= x <= b))

    cel = celulas.get((cid, d))
    print("celula", None if cel is None else (cel.pk, cel.trabalha, cel.origem, cel.dna))
    print("vinculo (tipo_id, base, ciclo, trabalha_em_feriado, feriado_vira_folga)",
          None if v is None else (v.tipo_escala_id, v.tipo_escala.tipo_base,
                                  v.tipo_escala.tipo_ciclo, v.trabalha_em_feriado,
                                  v.feriado_vira_folga(d)))
    print("motivo_escala_errada", motivo_escala_errada(cid, d))
    print("dia_por_id_batida", sorted(dia_das_batidas(col, a, b).items()))
    print("janela_he (ativa, desde)", getattr(col.empresa, "janela_he_ativa", None),
          getattr(col.empresa, "janela_he_desde", None))
    print("decisao_he_vigente", list(DecisaoHE.objects.vigentes().filter(
        colaborador_id=cid, data=d).values_list("pk", "data", "estado")))

    dp = DiaPago.objects.filter(
        colaborador_id=cid, mes=mes, ano=ano, data=d, tipo=DiaPago.TIPO_DIA, versao="motor"
    ).values(
        "pk", "data", "lavrado_em", "versao_motor", "horas_trabalhadas",
        "horas_noturnas", "horas_extras_100", "horas_extras_100_feriado",
        "horas_folga_trabalhada", "detalhe",
    ).first()
    if dp is not None:
        detalhe = dp.pop("detalhe") or {}
        for chave in ("folga_sem_escala_min", "folga_feriado_min", "o85_dia_sem_vinculo_min"):
            if chave in detalhe:
                dp[chave] = detalhe[chave]
    print("dia_lavrado", dp)
```

## 27/11 11:39 · sessao b4b559a914e3 · FERIADO-NAO-PREVISTO-L086 · leitura · origem=Code

## 1. Sonda de medida

A tela usa `core.feriados.feriados_do_colab` e cruza a data retornada com `CelulaDia.trabalha is False`. A função resolve o município pelo posto do vínculo que responde pelo dia. `app/colaboradores/services/calendario.py:346` `app/colaboradores/services/calendario.py:785` `app/core/feriados.py:124` `app/core/feriados.py:135` `app/core/feriados.py:161`

A fonte diária gravada é `DiaPago`, com `tipo='dia'` e `versao='motor'`. O fechamento grava essas linhas a partir da mesma apuração que alimenta `FechamentoMensal`. A geração do TXT nesta cópia monta as rubricas a partir dos objetos mensais que recebe de `recalcular_fechamento_mes`; a sonda atende ao recorte de **lavra gravada** lendo `DiaPago`, sem chamar essa rotina. `app/ponto/models.py:311` `app/ponto/services/fechamento.py:1219` `app/ponto/services/fechamento.py:1251` `app/folha/views.py:214` `app/folha/views.py:275` `app/folha/export.py:420`

O resumo distingue dia com lavra e `horas_trabalhadas <= 0` de dia sem linha lavrada. O filtro usa o campo literal `horas_trabalhadas > 0`. Assim, um dia cuja hora foi registrada integralmente em HE100 de feriado pode ficar fora dos detalhes e ainda contar no universo. `app/colaboradores/services/calendario.py:777` `app/ponto/models.py:325` `app/ponto/services/dia_pago.py:244`

Bloco para a entrada padrão de `python -B manage.py shell --settings=config.settings.sombra`. A configuração aponta para `sombra_db`, banco `sombra`, onde o schema do tenant é `juliani`. Execução e totais da sombra **[nao verificado]**. `app/config/settings/sombra.py:7` `app/config/settings/sombra.py:29` `app/config/settings/sombra.py:47`

```python
from collections import defaultdict
from datetime import date
from math import fsum

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone
from django_tenants.utils import schema_context

from colaboradores.models import Empresa
from core.feriados import carregar, feriados_do_colab
from escala.models import CelulaDia
from ponto.janelas import janela_fechamento
from ponto.models import DiaPago


MES, ANO = 11, 2026
EMPRESAS = (2, 3, 4)
JANELA_PEDIDA = (date(2026, 10, 21), date(2026, 11, 20))


def medir_empresa(empresa):
    inicio, fim = janela_fechamento(MES, ANO, empresa)
    if (inicio, fim) != JANELA_PEDIDA:
        raise RuntimeError(
            f"Empresa {empresa.pk}: janela encontrada {inicio} a {fim}"
        )

    # O mapa entregue a feriados_do_colab cobre a janela inteira.
    # app/core/feriados.py:135
    celulas = {
        (celula.colaborador_id, celula.data): celula
        for celula in CelulaDia.objects.filter(
            colaborador__empresa_id=empresa.pk,
            data__range=(inicio, fim),
        ).only("colaborador", "data", "trabalha", "escala_geradora")
    }
    folgas_por_colab = defaultdict(set)
    for (colab_id, dia), celula in celulas.items():
        if celula.trabalha is False:
            folgas_por_colab[colab_id].add(dia)

    feriados_cadastrados = carregar(inicio, fim)
    alvos = set()
    for colab_id, folgas in folgas_por_colab.items():
        feriados = feriados_do_colab(
            colab_id, inicio, fim,
            carregados=feriados_cadastrados,
            celulas=celulas,
        )
        alvos.update((colab_id, dia) for dia in folgas & feriados)

    # DiaPago.data ja e uma data. A chave do dia vem da lavra.
    # app/ponto/models.py:315 e app/ponto/services/dia_pago.py:109
    linhas = DiaPago.objects.filter(
        colaborador_id__in={colab_id for colab_id, _ in alvos},
        mes=MES,
        ano=ANO,
        tipo=DiaPago.TIPO_DIA,
        versao="motor",
        data__range=(inicio, fim),
    ).values_list(
        "colaborador_id", "data", "horas_trabalhadas", "horas_noturnas",
        "horas_extras_100", "horas_extras_100_noturna",
    )
    lavra = {(colab_id, dia): tuple(horas) for colab_id, dia, *horas in linhas}

    detalhes = []
    sem_lavra = 0
    com_lavra_sem_horas_positivas = 0
    for colab_id, dia in sorted(alvos):
        horas = lavra.get((colab_id, dia))
        if horas is None:
            sem_lavra += 1
        elif horas[0] > 0:
            detalhes.append((colab_id, empresa.pk, dia.isoformat(), *horas))
        else:
            com_lavra_sem_horas_positivas += 1

    resumo = (
        empresa.pk,
        len(alvos),
        len(detalhes),
        com_lavra_sem_horas_positivas,
        sem_lavra,
        fsum(linha[3] for linha in detalhes),
        fsum(linha[5] for linha in detalhes),
    )
    return detalhes, resumo


def sondar():
    if (
        not getattr(settings, "HASNER_SOMBRA", False)
        or connection.settings_dict["NAME"] != "sombra"
        or connection.settings_dict["HOST"] != "sombra_db"
    ):
        raise RuntimeError("Use a configuracao do banco sombra.")
    if connection.in_atomic_block or not connection.get_autocommit():
        raise RuntimeError("A sonda exige uma transacao externa propria.")

    medido_em = timezone.localtime(timezone.now())
    with schema_context("juliani"), transaction.atomic(durable=True):
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            cursor.execute("SELECT current_database(), current_schema()")
            if cursor.fetchone() != ("sombra", "juliani"):
                raise RuntimeError("Banco ou schema diferente do solicitado.")

        resultados = []
        for empresa_id in EMPRESAS:
            empresa = Empresa.objects.only("dia_inicio_competencia").get(pk=empresa_id)
            resultados.append(medir_empresa(empresa))

    print("medido_em", medido_em.isoformat(timespec="seconds"), sep="\t")
    print(
        "colab_id", "empresa_id", "data", "horas_trabalhadas", "horas_noturnas",
        "horas_extras_100", "horas_extras_100_noturna", sep="\t",
    )
    for detalhes, _ in resultados:
        for linha in detalhes:
            print(*linha, sep="\t")

    print(
        "empresa_id", "universo", "com_horas_positivas",
        "com_lavra_sem_horas_positivas", "sem_lavra",
        "total_horas_trabalhadas", "total_horas_extras_100", sep="\t",
    )
    resumos = [resumo for _, resumo in resultados]
    for resumo in resumos:
        print(*resumo, sep="\t")
    print("universo_total", sum(resumo[1] for resumo in resumos), sep="\t")


sondar()
```

## 2. Dono e regra da dobra

**O78** é o registro explícito da L-086, **fechado como selo**. **FERIADO-POR-POSTO** é a obra da marca cadastral por posto, registrada **no ar em 22/11 às 13:34**, com o P4 destacado como `P4-SYNC-FERIADO`. Para a fatia de dinheiro, o antecedente direto é **ESPELHO-FERIADO-DOBRA**, fechado, e o ramo de pagamento do **BO-B (3)** que alimenta o fechamento e o `DiaPago`. `app/docs/BACKLOG.md:1501` `app/docs/BACKLOG.md:1984` `app/docs/BACKLOG.md:1985` `app/docs/BACKLOG.md:2044` `app/docs/BACKLOG.md:1667` `app/ponto/services/fechamento.py:960`

O grep também localiza **FERIADO-DIA-CHAVE**, registrado para a atribuição do dia na virada da meia-noite, e **O275/O277**, respectivamente interrompida e registrada, com O277 dependente da O275 no registro. O aval anexo põe a cura de 1036 na fila de dinheiro de `SO-DOIS-ALIVIOS-v2`. O registro formal dessa fatia pelo identificador `FERIADO-NAO-PREVISTO-L086` nos três arquivos é **[nao verificado]**. `app/docs/BACKLOG.md:1668` `app/docs/GRADE-MESTRA.md:91` `app/docs/GRADE-MESTRA.md:233` `app/docs/GRADE-MESTRA.md:234` `app/docs/CORTES.md:616` `aval.txt:1`

**Quem decide no motor.** `MotorBase._feriado_dobra_no_dia(dia)` retorna `bool(self.feriado_em_dobra) and self.eh_feriado(dia)`. No caminho normal do fechamento, **sim, é a mesma fonte que a tela usa**. O fechamento chama `feriados_do_colab` e injeta o conjunto no motor, cobrindo também as bordas da janela de batidas. Sem injeção, `eh_feriado` recorre a `is_feriado(data)` sem município, que enxerga só o nacional. `MotorIntermitente` injeta um conjunto vazio próprio. `app/ponto/motor_calculo_v2.py:941` `app/ponto/motor_calculo_v2.py:1000` `app/ponto/motor_calculo_v2.py:30` `app/ponto/motor_calculo_v2.py:3694` `app/ponto/services/fechamento.py:611` `app/ponto/services/fechamento.py:668` `app/ponto/services/fechamento.py:754`

| Campo ou insumo | Quem escreve e quem lê |
|---|---|
| `feriados` e `_feriados_injetados` | `feriados_do_colab` compõe as datas. `MotorBase.__init__` grava os atributos em memória e `MotorBase.eh_feriado` os lê. `MotorIntermitente.__init__` grava o vazio nos dois. `app/core/feriados.py:159` `app/ponto/motor_calculo_v2.py:817` `app/ponto/motor_calculo_v2.py:1007` `app/ponto/motor_calculo_v2.py:3698` |
| `Feriado.data`, `abrangencia`, `uf`, `municipio` e lugar do vínculo | `form_feriado` cria e edita o cadastro, e o importador insere nacionais. `_qs`, `alcanca` e `feriados_de` leem as datas e o alcance. `executar_vinculo` grava o posto no vínculo. A porta de estrutura grava `Posto.municipio`, e `lugar_do_vinculo` lê esse município do vínculo escolhido por `vinculo_do_dia`. `app/core/views_config.py:64` `app/core/views_config.py:83` `app/core/views_config.py:144` `app/core/feriados.py:27` `app/core/feriados.py:57` `app/core/feriados.py:70` `app/colaboradores/services/vinculo.py:275` `app/colaboradores/services/vinculo.py:287` `app/colaboradores/services/estrutura.py:67` `app/colaboradores/services/estrutura.py:99` `app/escala/alimentacao.py:269` `app/core/feriados.py:124` |
| `TipoEscala.tipo_ciclo` e `feriado_em_dobra` | `salvar_tipo` grava o ciclo. `get_motor` lê o ciclo e resolve o parâmetro por `CICLOS_FERIADO_SIMPLES`, hoje `{'12x36'}`. `MotorBase.__init__` guarda o booleano, e `_feriado_dobra_no_dia` o lê. Com a régua CCT ativa e cláusula aplicável, `get_motor_cct` fornece `True` para 12x36. O formulário de sindicato grava `feriado_12x36_em_dobra` e `regua_para` lê a cláusula da convenção aplicável. Escritor da chave global `regua_cct_ativa` **[nao verificado]**. `app/escala/services/cadastro_tipo.py:152` `app/escala/services/cadastro_tipo.py:307` `app/ponto/motor_calculo_v2.py:3740` `app/ponto/motor_calculo_v2.py:3765` `app/ponto/motor_calculo_v2.py:3810` `app/ponto/motor_calculo_v2.py:814` `app/ponto/motor_calculo_v2.py:961` `app/core/views_config.py:282` `app/core/views_config.py:312` `app/core/views_config.py:323` `app/core/regua_cct.py:366` `app/core/regua_cct.py:444` `app/core/regua_cct.py:473` |
| Dia do turno e minutos trabalhados | `_dia_do_turno` monta o mapa de instante da entrada para dia do turno. `calcular_mes` guarda o mapa e `_dia_do_turno_do_par` o lê, usando a data local da entrada crua se a chave faltar. A apuração do período escreve `minutos_trabalhados`, e os ramos de dobra exigem duração positiva antes de gravar HE100. `app/ponto/motor_calculo_v2.py:911` `app/ponto/motor_calculo_v2.py:2510` `app/ponto/motor_calculo_v2.py:963` `app/ponto/motor_calculo_v2.py:2898` `app/ponto/motor_calculo_v2.py:2925` `app/ponto/motor_calculo_v2.py:3208` `app/ponto/motor_calculo_v2.py:3372` `app/ponto/motor_calculo_v2.py:3385` `app/ponto/motor_calculo_v2.py:3467` |
| `CelulaDia.trabalha`, `escala_geradora`, `trabalha_em_feriado` e `opera_em_feriado` | A porta da célula grava `trabalha` e `escala_geradora` na geração, regeneração e edição. A tela lê `trabalha`, o juiz do vínculo lê `escala_geradora`, e o motor lê a resposta de `eh_dia_trabalho` para classificar folga. A porta da marca grava `opera_em_feriado` e propaga para vínculos elegíveis via `marcar_trabalha_em_feriado`. O nascimento do vínculo também preenche a chave. `feriado_vira_folga` lê essa chave, o ciclo e o feriado local. `app/ponto/portas/celula.py:289` `app/ponto/portas/celula.py:655` `app/colaboradores/services/calendario.py:791` `app/escala/alimentacao.py:269` `app/escala/models.py:1105` `app/ponto/motor_calculo_v2.py:2560` `app/colaboradores/services/estrutura.py:90` `app/colaboradores/services/feriado_do_posto.py:334` `app/colaboradores/services/feriado_do_posto.py:368` `app/colaboradores/services/vinculo.py:297` `app/colaboradores/services/vinculo.py:903` `app/escala/models.py:1400` |

**O ramo que importa quando a célula diz folga.** O motor pode encaminhar o período para `periodos_ft`. O fechamento filtra esses períodos por `escala_certa_no_dia` e promove a HE100 de feriado aqueles cujo vínculo do dia diz `feriado_vira_folga`. O fechamento grava os totais mensais e `dia_pago.lavrar` grava a mesma partição por dia. Nesse ramo, `feriado_vira_folga` consulta `core.feriados.eh_feriado` pelo lugar do vínculo, a mesma autoridade geográfica usada por `feriados_do_colab`. `app/ponto/motor_calculo_v2.py:2560` `app/ponto/motor_calculo_v2.py:2581` `app/ponto/motor_calculo_v2.py:2605` `app/ponto/services/fechamento.py:953` `app/ponto/services/fechamento.py:968` `app/ponto/services/fechamento.py:978` `app/ponto/services/dia_pago.py:244` `app/escala/models.py:1400` `app/core/feriados.py:101`

### O que os registros mandam para feriado do colaborador, célula `False` e batida

| Lei, corte ou obra | Efeito para o caso |
|---|---|
| **L-086** | O texto original mandava dobrar o feriado trabalhado fora da escala. A própria linha registra a revogação da cláusula de feriado. A regra remanescente trata do domingo e preserva o dia de início da jornada. `app/docs/LEIS.md:117` `app/docs/LEIS.md:118` `app/ponto/motor_calculo_v2.py:980` |
| **Corte de 27/09 03:xx** | Para o feriado, valem o dia de início do turno e a régua de dobra do ciclo. `trabalha=False` por si só é insuficiente para concluir pela dobra. No 12x36, o padrão é simples e uma CCT aplicável pode autorizar a dobra. `app/ponto/motor_calculo_v2.py:941` `app/ponto/motor_calculo_v2.py:3740` `app/core/regua_cct.py:473` |
| **O78** | O registro histórico esperava dobra fora da escala. O selo atual testa início no feriado com dobra habilitada, véspera, ciclo sem dobra e a regra de domingo. O selo atual admite dobra mesmo com trabalho previsto quando a régua do ciclo permite. `app/docs/BACKLOG.md:1501` `app/ponto/tests/test_selo_l086_feriado_previsto_nao_dobra.py:43` `app/ponto/tests/test_selo_l086_feriado_previsto_nao_dobra.py:74` |
| **O277 — SDF-FERIADO-SEM-DOBRA** | O corte manda hora sem dobra para SDF e intermitente, e manda o cartão omitir a afirmação de pagamento em dobro. Para SDF, o registro vincula a aplicação ao cadastro da O275. Para intermitente, a cópia já mostra a neutralização do calendário de dobra no motor v2. `app/docs/BACKLOG.md:1698` `app/docs/BACKLOG.md:2245` `app/docs/GRADE-MESTRA.md:234` `app/ponto/motor_calculo_v2.py:3694` |
| **CORTE-CADERNO-1330-4-FERIADO-SO-SEMANAL** | Restringe a chave de previsão “trabalha em feriado” à escala semanal, citando 6x1 e 5x2. Plantões 12x36 e 24x48 seguem a precedência existente. O corte trata da previsão do dia e deixa a rubrica a cargo da regra de pagamento aplicável. `app/docs/CORTES.md:276` `app/docs/CORTES.md:616` |
| **BO-B (3), 23/10 15:0x** | Para trabalho em feriado numa escala que folga em feriado, o corte manda lançar HE100 de feriado. O ramo implementado exige um período em `periodos_ft` que passe por `escala_certa_no_dia` e pelo `feriado_vira_folga` do vínculo do dia. `app/ponto/services/fechamento.py:953` `app/ponto/services/fechamento.py:960` `app/ponto/services/fechamento.py:973` `app/ponto/services/dia_pago.py:244` |

O aval relata para **1036 em 20/11** feriado do colaborador, célula `False`, batidas, **7,29 h trabalhadas, 1,16 h noturnas e 0,0 h de HE100**. O ciclo, a régua aplicável, a chave `trabalha_em_feriado` do vínculo e o resultado do crivo de escala certa para esse dia estão **[nao verificado]**. Esses insumos são necessários para atribuir o zero ao ramo exato. `aval.txt:1` `app/ponto/motor_calculo_v2.py:961` `app/ponto/services/fechamento.py:953` `app/ponto/services/fechamento.py:973`

## 27/11 11:37 · sessao 318029dd4fa3 · FERIADO-BOTAO-CORTADO · construtor · origem=Code

**FERIADO-BOTAO-CORTADO** — O botão fica logo abaixo da data sempre que a célula o oferece. No mês, a altura continua em 132 px e o conteúdo abaixo do botão rola com mouse, toque ou teclado. Na fita, a ação vem antes dos horários e o rótulo quebra linha. Esse arranjo usa a mesma ordem nos dois layouts e mantém disponíveis as marcações originais, inclusive as retratadas. [Grade](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:68) · [Ação e rolagem](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:95) · [Marcações](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:133)

### Diff e RED

- A grade move o include acima das marcações, reserva espaço para a ação no mês e torna o restante da célula rolável. A condição do include continua `pode_editar_dia and dia.feriado_vira_trabalho_oferecido`. [Grade](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:95)
- O partial limita a largura do controle à célula e permite a quebra do rótulo. [Controle do feriado](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/ponto/partials/_feriado_vira_trabalho.html:5)
- O teste renderiza o include real com uma fixture fixa no formato do contexto do calendário. Em `mes` e `fita`, confere o formulário na célula de 20/11, as quatro marcações e a ação em um bloco irmão anterior às marcações. Também confere o botão em 15/11 sem batidas e sua ausência no dia comum. Na fonte pré-cura, a ação vinha depois dos horários, invertendo a ordem exigida pelo `assertLess`. Execução RED [nao verificado]. [Fixture](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/colaboradores/tests/test_feriado_botao_cortado.py:32) · [Asserção RED](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/colaboradores/tests/test_feriado_botao_cortado.py:92) · [Controles](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/colaboradores/tests/test_feriado_botao_cortado.py:109) [nao verificado]

### Execução e mutantes

O vizinho `test_relogio_riscado_tamanho` rodou **8 testes, todos passaram**, com `PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -v core.tests.test_relogio_riscado_tamanho`. RED, cura, M1–M3 e a rodada dos vizinhos com [nome] pararam na importação de `django.core`. O resultado dessas asserções permanece [nao verificado]. [Vizinho](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_relogio_riscado_tamanho.py:61) · [Entrada da suíte](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/manage.py:11) [nao verificado]

Comandos da suíte Code, a partir de `app/`. Cada mutante restaura o template ao sair. M1 retira o layout em coluna, M2 corta a rolagem e M3 volta a impedir a quebra do rótulo. [Asserções correspondentes](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/colaboradores/tests/test_feriado_botao_cortado.py:95) [nao verificado]

```bash
teste() { [nao verificado]
  PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --settings=config.settings.ci \
    colaboradores.tests.test_feriado_botao_cortado
}
mutar() (
  alvo=$1
  original=$(<"$alvo")
  trap 'printf "%s\n" "$original" > "$alvo"' EXIT
  alterado=$(sed "$2" <<<"$original")
  [[ "$alterado" != "$original" ]] || exit 2
  printf '%s\n' "$alterado" > "$alvo"
  teste [nao verificado]
)

grade=templates/colaboradores/partials/_calendario_grade.html
acao=templates/ponto/partials/_feriado_vira_trabalho.html

teste [nao verificado]
mutar "$grade" 's/display:flex;flex-direction:column;{% endif %}/display:block;{% endif %}/'
mutar "$grade" 's/min-height:0;overflow-y:auto;/min-height:0;overflow-y:hidden;/'
mutar "$acao" 's/white-space:normal;overflow-wrap:anywhere;/white-space:nowrap;/'

mapfile -t vizinhos < <(rg -l '_calendario_grade' -g 'test_*.py' */tests |
  sort | sed 's|/|.|g; s|\.py$||')
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --settings=config.settings.ci \
  "${vizinhos[@]}" core.tests.test_smoke_retratar_1366 ponto.tests.test_kaua_feriado_pela_tela
```

### Smoke do Ronald

1. No perfil do colaborador do anexo, abrir **Calendário de Ponto → Mês → novembro → 20/11**. Conferir o rótulo inteiro, abrir o formulário sem enviar e percorrer as quatro marcações dentro da célula. Em **15/11**, conferir o botão sem batidas. O identificador `colNN` é [nao verificado]. [Tela do perfil](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/detalhe.html:204) · Anexo `FERIADO-BOTAO-CORTADO-2711-1120.txt:1`.
2. No drawer de diagnóstico do mesmo colaborador, abrir **Escala x batidas**, deslizar até **20/11** e conferir o rótulo inteiro, a abertura do formulário e os quatro horários. Smoke em navegador [nao verificado]. [Fita do diagnóstico](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_drawer_diagnostico.html:33)

## 27/11 11:31 · sessao 307a837071f5 · CASO-LITERAL-14 · sonda · origem=Code

A sonda fecha **163 em 18/11 como (a)**. A resposta de volta às `05:07`, a batida `158099` às `05:06` e a pausa reconhecida pelo pareador identificam a marca que ficou sem casar na ata. `app/chamados/services/materializacao.py:595` · `app/ponto/turnos.py:502`

| Dia | Classe | Prova na saída da sonda | Porta |
|---|---|---|---|
| **163 · 18/11** | **(a)** | `ERROR pergunta_id=59341 motivo=intervalo_volta resposta=05:07`, com `marcos_faltantes=E x ata=S`. `PAUSAS 1` liga `158092` às `04:07:13` a `158099` às `05:06:08`, e `TURNO 17/11` fecha em `158201`. `LUZ 140478 3` mostra `03:00` apagada e `PAPEL_X` lista `158099`. A volta declarada ficou sem casar na ata. `app/chamados/services/materializacao.py:595` · `app/ponto/turnos.py:502` | **obra que cura** |
| **163 · 20/11** | **(d)** | `PAUSAS 1` liga `160127` a `160135`, `TURNO 19/11` fecha em `160202` e `LUZ 140480 3` está apagada. `PERG 60094` mostra o slot `03:00/E`, que pode vir do marco fixo. `PERG 60098` liga `160135` à pergunta e `ATO 6391` indica texto presente. Falta a declaração que confirme `160135` como volta da pausa [nao verificado]. `app/chamados/services/materializacao.py:702` · `app/ponto/turnos.py:1316` | não sei |
| **334 · 10/11** | **(d)** | `LUZ 132554 2` atribui `02:02` ao fechamento previsto às `08:00`, mas `TURNO 09/11` fecha em `150563` e outro abre em `150585`, com `saida=None`. `PERG 55996` está sem resposta e tem `via_celula_absolveu=1`. Falta confirmar qual marca fechou o plantão ou localizar uma saída posterior a `150585` [nao verificado]. `app/ponto/juiz_batida.py:263` · `app/chamados/models.py:2847` | não sei |
| **334 · 16/11** | **(d)** | `TURNO 15/11` fecha `155980 → 156062`, mas `LUZ 132560 2` mostra o fechamento apagado. `TRILHA 1012709`, `1012700` e `1012703` registram retratação humana de `156083`, `157001` e `157080`. Falta confirmar o papel de `156062` e esclarecer se houve trabalho na sequência iniciada por `157001` e como terminou [nao verificado]. `app/ponto/juiz_batida.py:263` · `app/ponto/services/retratar_humano.py:89` | não sei |
| **334 · 17/11** | **(d)** | `BAT 157080` tem `retratada_em=27/11 11:08:35`. `TURNO 17/11` deixa `157102` e `157339` em registros separados, ambos com `entrada=None`. `PERG 58935` tem `via_celula_absolveu=1`. Falta atribuir as saídas ao trabalho efetivamente realizado [nao verificado]. `app/ponto/turnos.py:312` · `app/chamados/models.py:2847` | não sei |
| **446 · 01/11** | **(d)** | `TURNO 01/11` abre em `143320`, contém `143451` e `143543` e tem `saida=None`. `LUZ 136277 4` mostra `23:00` apagada. Falta a saída verdadeira após `143543` [nao verificado]. `app/ponto/turnos.py:321` · `app/ponto/juiz_batida.py:263` | não sei |
| **219 · 14/11** | **(d)** | `BAT 159534` tem `pergunta_origem_id=57759`, e `ATO 6118` confirma a validação. `TURNO 13/11` fecha em `154644` e `TURNO 14/11` separa `154790 → 154793`. `PERG 57827/57828` têm `via_celula_absolveu=1`. Falta comprovar se o par de `07:03–07:04` foi trabalho ou toque excedente [nao verificado]. `app/chamados/models.py:2847` · `app/chamados/juizes.py:771` | não sei |
| **446 · 25/10** | **(d)** | `TURNO 25/10` deixa `136793` aberto e usa `136990` como entrada do período que fecha em `144667`. `BAT 144666/144667` apontam para `PERG 52823/52825`, com slots `18:00/S` e `23:00/S`. Falta confirmar se `136793 → 136990` foi trabalho e a qual jornada pertencem as duas saídas plantadas [nao verificado]. `app/ponto/turnos.py:312` · `app/chamados/juizes.py:771` | não sei |
| **446 · 27/10** | **(d)** | `TURNO 27/10` deixa `138687` aberto e fecha `138784 → 145038`, com `144664` no meio. `BAT 144664/145038` apontam para `PERG 52820/53401`; `PERG 52821` tem slot `19:00/E` e `validada_em=None`. Falta a declaração do `CHAMADO 36462` que identifique o trabalho e as saídas de cada sequência [nao verificado]. `app/chamados/juizes.py:360` · `app/chamados/juizes.py:771` | não sei |
| **465 · 12/11** | **(d)** | `TURNO 12/11` separa `152760 → 152798` de `152988 → 153457`, com pausa `152993 → 153079`. `LUZ 130759 1–4` acende às `07:33`, `12:31`, `13:34` e `19:32`. Falta confirmar o trabalho antes de `152798` e o papel de `152988` frente à pausa [nao verificado]. `app/ponto/juiz_batida.py:230` · `app/ponto/turnos.py:502` | não sei |
| **707 · 27/10** | **(d)** | `DNA 130929` mostra bordas anteriores `11:00–23:00` e atuais `07:00–19:00`. `CEL 130929` registra `regenerada_em=27/11 10:11:06`. O marco anterior de `16:00` coincide com `BAT 152915`, retratada na `TRILHA 1012218`. O turno atual fecha `152914 → 151794` e inclui `152916`, cujo flip consta na `TRILHA 1012246`. A troca de régua está provada. Falta o lastro que atribua as marcas concorrentes ao trabalho ou à escala anterior [nao verificado]. `app/ponto/portas/celula.py:297` · `app/ponto/services/flip_batida.py:89` | não sei |
| **1010 · 04/11** | **(d)** | `TRILHA 932956` confirma a retratação humana de `146307`, com `motivo_retratacao.id=1` e observação presente. `CEL 143296` tem `trabalha=0`. O `TURNO` vigente deixa `146083` aberto. `TURNO 146307` fecha `146083 → 146380` só na reapresentação. Faltam o conteúdo do motivo e a confirmação de trabalho iniciado em `146083` [nao verificado]. `app/ponto/services/retratar_humano.py:95` · `app/ponto/turnos.py:1920` | não sei |
| **1010 · 05/11** | **(d)** | `TURNO` vigente atribui `146321 → 146380` a `05/11`, separado de `147009 → 147338`. `TURNO 146307` desloca as duas primeiras marcas para o plantão de `04/11` ao reapresentar a batida retratada. Falta confirmar se eram volta e saída da véspera ou um período próprio [nao verificado]. `app/ponto/turnos.py:285` · `app/ponto/turnos.py:1920` | não sei |
| **1010 · 07/11** | **(d)** | `BAT 151808` tem `pergunta_origem_id=54816`. `PERG 54816` declara `data_evento=06/11` e slot `07/11 05:54/S`, com validação no `ATO 1137`. `CEL 143298` tem `trabalha=0` e o `TURNO 07/11` mostra `151808` com `entrada=None`. Falta provar o trabalho de `06/11` e se `151808` completou ou duplicou a saída [nao verificado]. `app/chamados/juizes.py:771` · `app/ponto/turnos.py:312` | não sei |
| **989 · 25/10** | **(d) nesta volta** | A saída traz `DIA 2 446 2026-10-25`. Para **989**, os campos de `BAT 137193`, o modo de intervalo, a ata anterior e o consumo das marcas pelos dias vizinhos seguem [nao verificado]. A classe histórica (a) vem de `r.md`; o predicado exato segue sem prova nesta rodada. `app/escala/models.py:243` · `app/escala/utils.py:1686` | não sei |

**Totais da tabela, 15 dias** — **(a) 1 · (b) 0 · (c) 0 · (d) 14**.

## 27/11 11:28 · sessao ede27dcbb6db · FERIADO-BOTAO-CORTADO · censo · origem=Code

## CENSO

- leitores: A grade lê `pode_editar_dia` e `dia.feriado_vira_trabalho_oferecido` na mesma condição e inclui o formulário com colaborador, data e URL de retorno (`app/templates/colaboradores/partials/_calendario_grade.html:131-132`). O POST verifica `editar_escalas` e relê a oferta para o dia solicitado (`app/ponto/views.py:3607-3609`, `app/ponto/views.py:3623-3633`). Três templates incluem a grade. O calendário do perfil usa `layout='mes'` (`app/colaboradores/views.py:1990-2010`, `app/templates/colaboradores/calendario_partial.html:67`). O drawer de diagnóstico usa `layout='fita'` (`app/colaboradores/views.py:2987-2992`, `app/colaboradores/services/drawer.py:122-124`, `app/templates/colaboradores/partials/_drawer_diagnostico.html:33-35`). A Auditoria de Espelhos usa a grade automática com `only`, passando `dias` e `resumo`, sem a permissão do botão (`app/relatorios/views.py:1609`, `app/relatorios/views.py:1634`, `app/relatorios/furos.py:164-168`, `app/templates/relatorios/furos.html:37-42`). O perfil carrega o fragmento por HTMX e a aba Espelho do drawer genérico carrega a mesma rota por `fetch` (`app/templates/colaboradores/detalhe.html:207-215`, `app/templates/colaboradores/partials/painel_colaborador.html:181-185`, `app/templates/core/_drawer_generico.html:132-142`). O Painel Situacional e a lista de pendências abrem o drawer de diagnóstico (`app/templates/colaboradores/partials/_ps_linha.html:4`, `app/templates/colaboradores/partials/pendencias_disputa.html:9`). `fita-mes.js` mede os filhos diretos da fita e lê `data-mes`. `popover-acao.js` mede o `summary` para posicionar o painel (`app/static/js/fita-mes.js:5-17`, `app/static/js/popover-acao.js:33-45`). Os leitores de estrutura e geometria são os selos de altura e grade, sete colunas, chips, pílulas, popovers e oferta do feriado, discriminados nas NOTAS.
- escritores: `contexto_calendario` grava `feriado_vira_trabalho_oferecido` quando a data é feriado daquele colaborador e a célula existe com `trabalha is False` (`app/colaboradores/services/calendario.py:343-347`, `app/colaboradores/services/calendario.py:785-793`). `calendario_ponto` põe `pode_editar_dia=True` quando `editar_escalas` é permitida. `contexto_drawer` põe o booleano retornado pela mesma permissão (`app/colaboradores/views.py:2005-2010`, `app/colaboradores/services/drawer.py:156-162`). Os includes escolhem o layout e a grade escreve a geometria inline. O modo `mes` usa sete colunas e célula com `height:132px;min-height:132px;overflow:hidden`. A fita usa contêiner horizontal rolável e `flex:0 0 78px` na célula. A grade escreve também o bloco de horários e o contêiner da ação, após os horários (`app/templates/colaboradores/calendario_partial.html:67`, `app/templates/colaboradores/partials/_drawer_diagnostico.html:35`, `app/templates/relatorios/furos.html:42`, `app/templates/colaboradores/partials/_calendario_grade.html:64-68`, `app/templates/colaboradores/partials/_calendario_grade.html:124-132`). O partial da ação escreve o `summary`, o formulário e seus estilos. Ao abrir, o JS existente aplica `position:fixed` e calcula a posição do painel (`app/templates/ponto/partials/_feriado_vira_trabalho.html:5-12`, `app/static/js/popover-acao.js:35-76`, `app/static/js/popover-acao.js:88-92`).
- decisoes:
  - **Posição da ação e altura.** O brief delega o desenho. A opção mais simples para `mes` é reservar espaço para a ação no rodapé da própria célula e dar rolagem vertical à área de conteúdo acima, mantendo os 132 px. Na `fita`, usar o mesmo controle e manter a altura natural. Permitir que o rótulo quebre dentro da largura disponível, pois o `summary` hoje usa `white-space:nowrap`. O corte UI-GRADE-CALENDARIO fixa a altura em `mes`, e UI-PILULAS-QUEBRAM-LINHA registra que a célula não cresce. A oferta vale para qualquer número de batidas nos dois layouts (`q.md:4`, `FERIADO-BOTAO-CORTADO-2711-1120.txt:1`, `app/templates/colaboradores/partials/_calendario_grade.html:2-8`, `app/templates/colaboradores/partials/_calendario_grade.html:65-68`, `app/templates/colaboradores/partials/_calendario_grade.html:83`, `app/templates/ponto/partials/_feriado_vira_trabalho.html:6`).
  - **Acesso às marcações.** O brief delega rolagem, resumo com acesso ao detalhe ou outro desenho equivalente. A rolagem interna reaproveita as linhas existentes e mantém cada horário acessível, inclusive as retratadas riscadas e as ações das marcações. O corte é a exibição do cru da Portaria 671, registrada na L-025 (`q.md:4`, `app/docs/LEIS.md:76`, `CLAUDE.md:171`, `app/templates/colaboradores/partials/_calendario_grade.html:113-129`).
  - **Predicado, formulário e escopo.** O aval fecha a oferta existente. A L-002 exige que a tela leia a mesma autoridade do POST. Reaproveitar `details`, `summary` e os ganchos do popover mantém o formulário ligado à mesma rota e dispensa JS novo. O brief limita a mudança a template/CSS e ao teste pedido (`FERIADO-BOTAO-CORTADO-2711-1120.txt:1`, `q.md:5-6`, `app/docs/LEIS.md:60`, `app/ponto/views.py:3623-3633`, `app/templates/ponto/partials/_feriado_vira_trabalho.html:5-12`, `app/static/js/popover-acao.js:33-45`).
  - **Prova da regressão.** O brief deixa escolher entre a view real e o include com contexto real. Para o desenho acima, a asserção estrutural que morde a cópia é exigir, dentro da célula do feriado, o controle como irmão da região rolável que contém as quatro marcações. Hoje o bloco de horários e o controle seguem no fluxo normal da célula com corte, sem essa região. Cobrir `mes` e `fita`, o feriado sem batida e o dia comum sem oferta, com datas fixas. O selo de front cobre as duas cascas por render ou grafo de includes (`q.md:5`, `app/templates/colaboradores/partials/_calendario_grade.html:68`, `app/templates/colaboradores/partials/_calendario_grade.html:124-132`, `CLAUDE.md:207`).
  - **Smoke.** O aval pede o clique do Ronald no Calendário de Ponto do perfil de `[nome]`, em novembro, no dia 20/11. Conferir o controle inteiro, abrir o formulário sem enviar e acessar os quatro horários. Usar 15/11 como controle e repetir a checagem de layout na fita do diagnóstico. O smoke da obra vem do aval. A trava geral FRONT SEM SMOKE NAO SOBE cobre JS, service worker, templates-base e os partials que eles incluem (`FERIADO-BOTAO-CORTADO-2711-1120.txt:1`, `q.md:4`, `q.md:7`, `app/templates/colaboradores/detalhe.html:201-215`, `app/templates/colaboradores/partials/_drawer_diagnostico.html:33-35`, `CLAUDE.md:206-209`, `app/docs/LEIS.md:99`).

perguntas de lei: 0

## NOTAS

- **`fita`.** O código não aplica à fita o corte vertical de 132 px do mês. O `overflow-y:hidden` está no contêiner horizontal, que não declara altura fixa, e a célula da fita tem base horizontal de 78 px. O rótulo sem quebra merece conferência visual nessa largura. Corte do botão na fita em navegador ou em produção [nao verificado]. (`app/templates/colaboradores/partials/_calendario_grade.html:65-68`, `app/templates/ponto/partials/_feriado_vira_trabalho.html:5-7`).
- **Render direto encontrado pela busca.** `app/colaboradores/tests/test_ui_pilulas_quebram_linha.py:52-62` mede a fileira de ações e a pílula do chamado. `app/colaboradores/tests/test_espuria_tela_retratar_orfa.py:29-32` verifica as portas por batida. `app/colaboradores/tests/test_cal09_dia_so_com_saidas.py:39-54` verifica os horários. `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:113-121` verifica os títulos de status. `app/colaboradores/tests/test_calendario_mostra_o_carro.py:75-85` e `app/colaboradores/tests/test_calendario_mostra_o_carro.py:144-165` verificam título, chamado e Resolver dia. `app/colaboradores/tests/test_e4cal_l1_r7_grade_le_o_rotulo.py:29-50` verifica os rótulos. `app/colaboradores/tests/test_e4cal_l1_r8_rodape_le_o_nucleo.py:39-55` renderiza o rodapé com `dias=[]`. `app/core/tests/test_selo_marca_do_veredito.py:105-128` verifica as marcas de decisão. `app/ponto/tests/test_espelho_decidiu_icone.py:161-174` verifica o ícone e o título de quem decidiu.
- **Render pelas rotas.** Sete colunas abre o perfil e busca a grade (`app/colaboradores/tests/test_calendario_sete_colunas.py:138-148`). Chips usa o perfil, a fita e a Auditoria de Espelhos (`app/colaboradores/tests/test_chip_ausencia_duas_linhas.py:171-181`, `app/colaboradores/tests/test_chip_ausencia_duas_linhas.py:403-429`). [nome] testa `mes` e `fita`, oferta, permissão e negativos (`app/ponto/tests/test_kaua_feriado_pela_tela.py:83-86`, `app/ponto/tests/test_kaua_feriado_pela_tela.py:103-144`, `app/ponto/tests/test_kaua_feriado_pela_tela.py:249-258`). Os selos de sinais e de retratação também obtêm a grade pela view (`app/core/tests/test_selo_espelho_sinais.py:69-95`, `app/core/tests/test_smoke_retratar_1366.py:143-156`). Outros consumidores HTTP encontrados conferem contexto, resposta ou ações vizinhas (`app/colaboradores/tests/test_calendario_caracterizacao.py:13`, `app/colaboradores/tests/test_calendario_falta_futura.py:51-52`, `app/colaboradores/tests/test_calendario_badge_minutos.py:73-82`, `app/colaboradores/tests/test_calendario_cross_midnight.py:47-48`, `app/colaboradores/tests/test_drawer_caracterizacao.py:61-86`, `app/colaboradores/tests/test_get_sanitizado.py:42-47`, `app/colaboradores/tests/test_guarda_views_smoke.py:43-44`, `app/colaboradores/tests/test_guarda_views_smoke.py:60-61`, `app/core/tests/test_selo_performance.py:411-413`, `app/ponto/tests/test_porta_retratar_batida.py:238-243`, `app/ponto/tests/test_veredito_view.py:72-76`, `app/ponto/tests/test_veredito_view.py:94-105`, `app/ponto/tests/test_o137_esmeril_ausencia_le_o_cadastro.py:176-181`).
- **O que pode quebrar ao mover a ação.** [nome] compara o `action` e a sequência literal `name="dia" value="…" data-feriado-vira-trabalho`. Mudar só a posição do include preserva essas asserções, que não medem visibilidade nem posição na célula (`app/ponto/tests/test_kaua_feriado_pela_tela.py:105-144`). O selo de altura lê a fonte e exige `data-celula-dia` e a sequência inline de altura e overflow. Mover o CSS pode quebrar a regex mesmo com a geometria preservada (`app/colaboradores/tests/test_ui_grade_calendario.py:27-49`). Sete colunas depende de `[data-mes]`, mede o transbordamento e altera estilos literais para provocar a regressão. Chips mede a altura de 132 px e a contenção nos três layouts. Pílulas mede a largura dos filhos da fileira de ações. Mudanças na estrutura compartilhada podem afetar esses selos (`app/colaboradores/tests/test_calendario_sete_colunas.py:78`, `app/colaboradores/tests/test_calendario_sete_colunas.py:170-199`, `app/colaboradores/tests/test_chip_ausencia_duas_linhas.py:240-254`, `app/colaboradores/tests/test_chip_ausencia_duas_linhas.py:403-429`, `app/colaboradores/tests/test_ui_pilulas_quebram_linha.py:25-38`). Se o rearranjo mexer nas marcações, CAL09 exige horários no HTML, o selo de sinais exige o `summary` e dois botões no trecho das batidas, e o smoke de retratação mede os painéis abertos. O selo de popover varre os ganchos na fonte e renderiza uma grade com CSS copiado, usando o partial de Resolver dia (`app/colaboradores/tests/test_cal09_dia_so_com_saidas.py:43-54`, `app/core/tests/test_selo_espelho_sinais.py:114-122`, `app/core/tests/test_smoke_retratar_1366.py:171-186`, `app/core/tests/test_selo_popover_acao.py:42-59`, `app/core/tests/test_selo_popover_acao.py:76-86`, `app/core/tests/test_selo_popover_acao.py:124-125`).
- **Demais ocorrências da busca.** São inspeções de fonte, de includes ou comentários sobre a grade (`app/pautas/tests/test_pauta_do_dia.py:65-70`, `app/relatorios/tests/test_palavra_do_dia.py:307-314`, `app/folha/tests/test_retrato_folha_quadros.py:966-975`, `app/chamados/tests/test_o261_vigia_fio.py:1067-1075`, `app/colaboradores/tests/test_relogio_riscado_calendario.py:132-150`, `app/core/tests/test_relogio_riscado_tamanho.py:104-109`, `app/ponto/tests/test_contract_espelho_sem_emoji.py:111-114`, `app/ponto/tests/test_contract_espelho_sem_emoji.py:227-230`, `app/ponto/tests/test_contract_espelho_sem_emoji.py:323-332`, `app/ponto/tests/test_glifo_saida.py:8`, `app/ponto/tests/test_o130_em_aberto_diz_o_que_falta.py:138`).
- **Limites do censo.** Na cópia, a frase “a célula não cresce” está na linha 83 (`app/templates/colaboradores/partials/_calendario_grade.html:83`). O relato de 15/11 e 20/11 vem do aval fornecido (`FERIADO-BOTAO-CORTADO-2711-1120.txt:1`). Hash Git da cópia, identificação de `[nome]` como `colNN`, reprodução visual e resultados de execução dos testes [nao verificado].

## 27/11 11:27 · sessao 79188cd4f4b4 · CASO-LITERAL-14 · leitura · origem=Code

# CASO-LITERAL-14 — fase 3

**As duas entradas diurnas reproduzem o mesmo descasamento na cópia. A sobra noturna passa por um ramo diferente da grade, mas o predicado que deixou `137193` órfã em produção continua [nao verificado].** Os 14 dias da classe **(d)** continuam **não sei**. O ensaio foi feito em memória com os horários colados, sem a ata, as flags e a janela original de produção. O hash Git da cópia está [nao verificado]. `app/escala/utils.py:638`, `app/escala/utils.py:1634`, `app/escala/utils.py:1686`, `app/ponto/turnos.py:1192`

## 1. Os três dias da classe (a)

| Dia e batida | Sítio, predicado e casamento pela regra |
|---|---|
| **446 · 20/11 · `160414`** | No ensaio, `parear_turnos` fechou `160414 → 160528` e `160559 → 160790`. A `S` das 15:52 está a 72 min 59 s de `hi`, então falha a guarda `distância à borda > 90` que permitiria juntar os períodos pela pausa deslocada. Na grade, `_turno_do_juiz_no_dia` exige **um** turno fechado. Com dois, `_match_marcos` cai no alinhamento posicional. Seu filtro `timestamp >= hi − 90 min` exclui `13:00:43`, e o ensaio deixa `15:52` ocupar a entrada. A reconciliação de pontas só recupera a entrada se o marco estiver `missing`. O casamento correto preserva `160414` como entrada real e `160790` como saída final, com `160528 → 160559` no miolo se confirmada a pausa. A L-084 mantém a comparação com o cadastro, a L-087 governa o papel, e a L-097 governa a HE da antecipação. Esse ramo na lavra de produção está [nao verificado]. `app/ponto/turnos.py:1466`, `app/escala/utils.py:1634`, `app/escala/utils.py:642`, `app/escala/utils.py:2053`, `app/docs/LEIS.md:115`, `app/docs/LEIS.md:116`, `app/docs/LEIS.md:119` |
| **1036 · 14/11 · `154907`** | O mesmo caminho reproduzido. `155066`, às 15:50:47, está a 70 min 47 s de `hi`, e o pareador fecha `154907 → 155066` e `155091 → 155363`. A grade recusa o papel de turno único e exclui `12:00:48` do envelope que começa às 13:10. No ensaio, `15:50` ocupa a entrada e impede a recuperação pelo predicado `missing`. Pela L-084 e pela L-087, `154907` é a entrada da sequência e `155363` é a saída final. A pausa candidata é `155066 → 155091`. A entrada fica a 2 h 39 min 12 s de `hi` e a saída a 24 s de `hf`. Esse ramo na lavra de produção está [nao verificado]. `app/ponto/turnos.py:1466`, `app/ponto/turnos.py:1491`, `app/escala/utils.py:1634`, `app/escala/utils.py:642`, `app/escala/utils.py:2053`, `app/docs/LEIS.md:116`, `app/docs/LEIS.md:119` |
| **989 · 25/10 · `137193`** | O pareador em modo duração reconhece `137193 → 137210` como pausa de 51 min 33 s e fecha `136992 → 137291` no dia **25/10**. O ponto a conferir é `montar_grade_prevista_periodo_por_turno`: com `hii/hfi` nulos, a chamada a `_match_marcos` passa `turno=None`. A grade cria marcos internos por flags `is_intervalo` na janela ou, no ramo de intervalo livre, pelas sobras do primeiro alinhamento que passam os filtros de posição e distância. A reconciliação final trata só das bordas externas. Com dois marcos e sem flags, o ensaio **não reproduziu a sobra exata**: `22:11` ocupou a entrada e `23:02` sobrou. Faltam `TipoEscala.intervalo_modo`, `Batida.is_intervalo`, a ata anterior e o consumo das batidas pelos dias vizinhos para identificar o predicado efetivo [nao verificado]. A L-085 atribui a cauda de 26/10 ao plantão de 25/10. A L-087 governa os papéis, e `intervalos_do_turno` lê a pausa efetiva sob a regra de intrajornada do art. 71 registrada na cópia. `app/ponto/turnos.py:385`, `app/ponto/turnos.py:431`, `app/ponto/turnos.py:502`, `app/ponto/turnos.py:537`, `app/escala/models.py:243`, `app/escala/utils.py:1686`, `app/escala/utils.py:1748`, `app/escala/utils.py:2029`, `app/docs/LEIS.md:117`, `app/docs/LEIS.md:119` |

**Dono no BACKLOG.** Para **446 e 1036**, o antecedente direto é `SAIDA-VALIDA-VIRA-ORFA`, registrada como fechada. `CONTADOR-PAREADOR-X-CELULA` registra o resíduo de borda pareada que a grade deixa órfã. A cobertura destes dois dias pela cura D2 está [nao verificado]. Para **989**, `ATA-X-TURNO-PAUSA` é o item mais próximo: trata a pausa que o juiz reconhece e a ata deixa órfã. A entrada desse caso específico, com pausa nula nos marcos do envelope, no escopo da obra está [nao verificado]. `PAR-PELO-TURNO` cobre o corte pelo teto de duração e a atribuição à véspera. `RAMO2-B` cobre o trabalhado após a âncora de saída, inclusive em turno que cruza. Os ensaios acima fecham os pares sem necessidade dessas duas mudanças. `app/docs/BACKLOG.md:1225`, `app/docs/BACKLOG.md:1646`, `app/docs/BACKLOG.md:1865`, `app/docs/BACKLOG.md:2027`, `app/docs/BACKLOG.md:2159`, `app/docs/RELATO.md:241`, `app/ponto/turnos.py:1447`

## 2. Os 14 dias da classe (d)

| Grupo | Dia | Prova que falta |
|---|---|---|
| **1 · Continuidade** | **163 · 18/11** | Na célula `140478` de 17/11, quais `ata.lampadas` acenderam e quais `dna.marcos` valiam? Entre `158092`, `158099` e `158201`, qual encerrou, integrou ou abriu jornada? Conferir o ato e o slot da pergunta `59348`. [nao verificado] `app/ponto/turnos.py:285`, `app/ponto/turnos.py:1631`, `app/chamados/juizes.py:360` |
| **1** | **163 · 20/11** | A mesma prova na célula `140480` de 19/11, para `160127`, `160135` e `160202`, com a pergunta `60098`. [nao verificado] `app/ponto/turnos.py:285`, `app/ponto/turnos.py:1631`, `app/chamados/juizes.py:360` |
| **1** | **334 · 10/11** | Saída apurável depois de `150585`, marcos e ata da célula `132554`, e lastro da pergunta `55996` para distinguir pausa de saída em `150563`. No ramo sem bordas, a continuidade exige uma `S` posterior dentro do envelope. [nao verificado] `app/ponto/turnos.py:393`, `app/ponto/turnos.py:413`, `app/ponto/turnos.py:2961` |
| **1** | **334 · 16/11** | Qual `S` fechou a jornada que começou em 15/11, e se `156062 → 156083` foi pausa. Separadamente, qual `S` fechou a jornada aberta por `157001`. Cruzar março da célula, papéis e perguntas `58903` e `58935`. [nao verificado] `app/ponto/turnos.py:312`, `app/ponto/turnos.py:1491`, `app/ponto/juiz_batida.py:240` |
| **1** | **334 · 17/11** | Qual de `157080`, `157102` e `157339` encerrou o trabalho iniciado por `157001`, e qual foi o fato correspondente às demais. Conferir as atas de 16 e 17 e a pergunta `58935`. [nao verificado] `app/ponto/turnos.py:293`, `app/ponto/turnos.py:1491`, `app/ponto/turnos.py:1655` |
| **1** | **446 · 01/11** | `Batida.timestamp/tipo/retratada_em` da saída após `143543` e a lâmpada de fechamento da célula `136277`. Isso distingue uma jornada que começou em `143320` e continuou após a pausa de uma sequência com outra borda ou ainda sem fechamento. [nao verificado] `app/ponto/turnos.py:1356`, `app/ponto/turnos.py:1466`, `app/ponto/juiz_batida.py:260` |
| **2 · Identidade das marcas concorrentes** | **219 · 14/11** | Fechamento e ata de 13/11, origem de `159534` e decisão sobre o par `154790/154793`. Os 70 s passam o piso geométrico de 60 s. A via `celula_absolveu` das perguntas `57827/57828` é um veredito sobre a célula. A confirmação de trabalho ou de toque indevido continua [nao verificado]. `app/ponto/turnos.py:130`, `app/ponto/turnos.py:1487`, `app/chamados/models.py:2847` |
| **2** | **446 · 25/10** | Se `136793 → 136990` foi trabalho completo, parte da jornada ou marca sem trabalho, e a quais fatos pertencem as saídas plantadas `144666/144667`. Conferir as perguntas de origem, atos, bordas efetivas e ata `136270`. [nao verificado] `app/ponto/models.py:40`, `app/chamados/juizes.py:759`, `app/ponto/juiz_batida.py:260` |
| **2** | **446 · 27/10** | O mesmo confronto para `138687 → 138784` e as saídas `144664/145038`, com a declaração associada ao chamado `36462` e os marcos da célula `136272`. [nao verificado] `app/chamados/juizes.py:360`, `app/chamados/juizes.py:802`, `app/ponto/juiz_batida.py:240` |
| **2** | **465 · 12/11** | O papel de `152798` e `152988` frente à pausa candidata `152993 → 153079`. É preciso identificar qual sequência foi trabalho antes de atribuir uma classe única às duas sobras. As luzes e os atos vinculados podem discriminar as sequências. [nao verificado] `app/ponto/turnos.py:1356`, `app/ponto/turnos.py:1606`, `app/ponto/juiz_batida.py:240` |
| **3 · Estado anterior ao ato** | **707 · 27/10** | `CelulaDia.dna_anterior`, data da regeneração, declaração `50832` e sequência dos atos de `152914`, `152915`, `152916` e `151794`. Falta a decisão sobre qual `E` iniciou a jornada e qual `S` a encerrou. `dna_anterior` guarda só a foto imediatamente anterior. [nao verificado] `app/ponto/portas/celula.py:297`, `app/chamados/models.py:4507`, `app/ponto/services/flip_batida.py:72` |
| **3** | **1010 · 04/11** | Motivo e lastro da exclusão de `146307`, e prova de que `146083` abriu trabalho em 04/11. Conferir a folga da célula `143296` e o fato do chamado `36783`. A reapresentação em memória mede a dependência geométrica. A decisão sobre a realidade do ato continua [nao verificado]. `app/ponto/services/retratar_humano.py:76`, `app/ponto/turnos.py:1920`, `app/escala/models.py:1105` |
| **3** | **1010 · 05/11** | A mesma decisão sobre `146307`, agora para dizer se `146321/146380` eram volta e saída de 04/11 ou período próprio de 05/11. Separar do plantão que começa em `147009`. [nao verificado] `app/ponto/turnos.py:285`, `app/ponto/turnos.py:1920`, `app/ponto/turnos.py:2961` |
| **4 · Plantio com dia declarado** | **1010 · 07/11** | Para `151808`, conferir o slot e a validação de `54816`, a entrada e os marcos da célula `143298` de **06/11**, e se aquela jornada já tinha saída verdadeira. O `data_evento` identifica o dia declarado pela pergunta. A realidade da jornada e se o plantio completou ou duplicou a saída continuam [nao verificado]. `app/chamados/models.py:3095`, `app/chamados/juizes.py:360`, `app/chamados/juizes.py:802`, `app/docs/LEIS.md:117` |

**Como a prova muda a classe.** Se a mesma linha contiver batidas com explicações diferentes, a classificação única do dia continua **(d)** até resolver a divergência. O critério de dono segue a L-099 e as classes do aval fornecido. `app/docs/LEIS.md:122`

| Grupo | Resposta que leva a (a), (b) ou (c) |
|---|---|
| **1** | **(a)** A batida é borda ou pausa comprovada e o juiz a perde ou a põe no dia errado com cadastro coerente. **(b)** Há lastro explícito de toque excedente. **(c)** O trabalho se confirma e a escala autorizada diverge da célula ou do vínculo. Uma saída ainda ausente, duas `S` sem atribuição ou uma ata incompleta deixam **(d)**. `app/docs/LEIS.md:116`, `app/docs/LEIS.md:117`, `app/docs/LEIS.md:122`, `app/ponto/turnos.py:407` |
| **2** | **(a)** A prova identifica o par ou a pausa verdadeiros que o sistema descasou. **(b)** A prova identifica a marca excedente ou o plantio duplicado, com a sequência verdadeira identificada. **(c)** A jornada verdadeira foi em horário ou em dia diferente do cadastro autorizado na célula. Duração curta, aprovação e encerramento automático isolados deixam **(d)**. `app/ponto/turnos.py:130`, `app/chamados/models.py:2847`, `app/chamados/juizes.py:771`, `app/docs/LEIS.md:122` |
| **3** | **(a)** A declaração ou o histórico comprova a marca verdadeira e isola o descasamento no juiz. **(b)** A decisão sobre a própria sobra prova que foi indevida. Para `1010`, uma retratação legítima de `146307` decide somente sobre `146307`. **(c)** O DNA vigente à época ou a escala autorizada explica a divergência. Se só a reapresentação produz fechamento, sem prova do fato, fica **(d)**. `app/ponto/turnos.py:1920`, `app/ponto/services/retratar_humano.py:76`, `app/ponto/portas/celula.py:305`, `app/docs/LEIS.md:122` |
| **4** | **(a)** `151808` é a saída verdadeira de 06/11 e o juiz a deixa sobrando ou a atribui a 07/11. **(b)** A saída já foi cumprida e há prova de que `151808` é plantio indevido ou duplicado. **(c)** A jornada comprovada diverge do cadastro autorizado de 06/11. Vínculo com a pergunta e validação, por si, deixam **(d)**. `app/docs/LEIS.md:117`, `app/docs/LEIS.md:122`, `app/chamados/juizes.py:771` |

### Sonda de leitura para os quatro grupos

O bloco roda os quatro grupos em `python -B manage.py shell`, com uma transação PostgreSQL `READ ONLY`. `BAT`, `PAPEL`, `TURNO` e `PAUSAS` dão a geometria atual. `CEL`, `LUZ`, `DNA` e `GRADE` permitem comparar a ata gravada com a montagem atual. `PERG`, `ATO`, `CHAMADO` e `TRILHA` apontam o lastro por PK. A saída usa PKs, contagens, indicadores numéricos e instantes em `timezone.localtime`. Quando o fato está exclusivamente em texto livre, a sonda aponta a linha para conferência no sistema. O texto e a conclusão factual permanecem [nao verificado]. Validei a sintaxe e as 14 chaves de dia. A execução com banco está [nao verificado]. `app/config/settings/prod.py:11`, `app/ponto/services/cartorio.py:1187`, `app/ponto/turnos.py:1850`, `app/chamados/juizes.py:360`, `app/chamados/models.py:4344`

```python
from datetime import date, datetime, time, timedelta

from django.conf import settings
from django.db import connection, transaction
from django.db.models import Q
from django.utils import timezone

from chamados.catalogo.motor import VIVOS
from chamados.juizes import _slot_esperado, dia_do_fato, pergunta_respondida
from chamados.models import ChamadoColaborador, DecisaoPergunta, PerguntaDisputa
from colaboradores.models import Colaborador
from core.models import LogAuditoria
from escala import utils
from escala.alimentacao import celulas_do_periodo
from escala.models import CelulaDia
from escala.servico_jornada import escala_vigente
from escala.services.regeneracao import janelas_de_regeneracao
from ponto.juiz_batida import periodos_do_dia
from ponto.models import Batida
from ponto.services.flip_batida import ACOES_FLIP
from ponto.services.retratar_humano import ACAO, ACAO_DESFAZER
from ponto.turnos import (
    batidas_apuraveis, intervalos_do_turno, janela_com_pad,
    papel_por_minuto_da_ata, pausas_abaixo_do_piso, turnos_do_colab,
)

# Colaborador, data, batidas de interesse, perguntas e chamados conhecidos.
CASOS = {
    1: [
        (163, "11-18", (158092, 158099, 158201), (59348,), (35571,)),
        (163, "11-20", (160127, 160135, 160202), (60098,), (35571,)),
        (334, "11-10", (150563, 150585), (55996,), ()),
        (334, "11-16", (156062, 156083, 157001, 157080), (58903, 58935), (17636,)),
        (334, "11-17", (157001, 157080, 157102, 157339), (58935,), (17636, 39479)),
        (446, "11-01", (143320, 143451, 143543), (), ()),
    ],
    2: [
        (219, "11-14", (154790, 154793, 159534), (57827, 57828), (16611, 39246)),
        (446, "10-25", (136793, 136990, 144666, 144667), (), ()),
        (446, "10-27", (138687, 138784, 144664, 145038), (), (36462,)),
        (465, "11-12", (152760, 152798, 152988, 152993, 153079), (), ()),
    ],
    3: [
        (707, "10-27", (152914, 152915, 152916, 151794), (50832,), (30747, 35226)),
        (1010, "11-04", (146083, 146307, 146321, 146380), (), (36783,)),
        (1010, "11-05", (146083, 146307, 146321, 146380), (), (36783,)),
    ],
    4: [(1010, "11-07", (151808, 148746, 149056), (54816,), (36652, 37414))],
}


def local(ts):
    if ts is None:
        return None
    if timezone.is_naive(ts):
        ts = timezone.make_aware(ts)
    return timezone.localtime(ts).isoformat()


def dia0(d):
    return timezone.make_aware(datetime.combine(d, time.min))


def pk(obj):
    return obj.pk if obj is not None else None


def inspecionar(grupo, cid, md, alvos, qids, hids, agora):
    d = date.fromisoformat("2026-" + md)
    um = timedelta(days=1)
    us = timedelta(microseconds=1)
    ini, fim = d - um, d + um
    lo, hi = dia0(ini - um), dia0(fim + 3 * um)
    pad_ini, pad_fim = janela_com_pad(ini, fim)
    c = Colaborador.objects.get(pk=cid)
    print("DIA", grupo, cid, local(dia0(d)), local(agora))

    bs = list(Batida.objects.filter(
        colaborador_id=cid, timestamp__gte=dia0(pad_ini), timestamp__lt=hi
    ).order_by("timestamp", "pk"))
    ap = list(batidas_apuraveis(c, lo, hi - us))
    ap_turno = list(batidas_apuraveis(c, dia0(pad_ini), dia0(pad_fim + um) - us))
    vivas = {b.pk for b in ap} | {b.pk for b in ap_turno}
    por_pk = {b.pk: b for b in bs}
    print("BAT_CONTAGEM", len(bs), len(ap), len(ap_turno))
    print("ALVOS_PRESENTES", sum(bid in por_pk for bid in alvos))
    for b in bs:
        print("BAT", b.pk, local(b.timestamp), int(b.tipo == "E"), int(b.tipo == "S"),
              int(b.is_intervalo), int(b.pk in vivas), b.aprovada_por_id,
              b.pergunta_origem_id, local(b.retratada_em))

    cels = celulas_do_periodo([cid], pad_ini, pad_fim)
    papeis = papel_por_minuto_da_ata(
        c, pad_ini, pad_fim, batidas=ap_turno,
        celulas={dia: cel for (_, dia), cel in cels.items()},
    )
    for papel in ("E", "S", "X", "Xi"):
        ids = [b.pk for b in ap_turno if papeis.get(
            timezone.localtime(b.timestamp).replace(tzinfo=None, second=0, microsecond=0)
        ) == papel]
        print("PAPEL_" + papel, len(ids), ids)

    for dia in (ini, d, fim):
        cel = cels.get((cid, dia))
        ec = escala_vigente(c, dia, celulas=cels)
        te = ec.tipo_escala if ec else None
        cadeia = utils.cadeia_do_dia(
            dia, ec.marcos_do_dia(dia, celulas=cels), ec.pausas_do_dia(dia, celulas=cels)
        ) if ec else []
        print("VINCULO", local(dia0(dia)), pk(cel), pk(ec), pk(te),
              len(cadeia), [local(ts) for _, ts in cadeia])
        if cel is None:
            continue
        at = periodos_do_dia(cel, te)
        print("CEL", cel.pk, cel.escala_geradora_id,
              int(cel.trabalha is True), int(cel.trabalha is False),
              int(bool(te and te._intervalo_livre())), int(at["desalinhado"]),
              len(at["orfas"]), local(cel.julgada_em),
              local(cel.regenerada_em), local(cel.editada_em))
        for i, luz in enumerate(at["papeis"], 1):
            print("LUZ", cel.pk, i, int(luz.get("acesa") is True),
                  int(luz.get("acesa") is False),
                  local(luz.get("instante_marco")), local(luz.get("instante_luz")))
        for foto, dna in enumerate((cel.dna, cel.dna_anterior), 1):
            marcos = CelulaDia.marcos_presentes((dna or {}).get("marcos"))
            seq = [(tipo, time.fromisoformat(h)) for tipo, _, h in marcos]
            print("DNA", cel.pk, foto, int(dna is not None), len(seq),
                  [local(ts) for _, ts in utils.encadear_marcos(dia, seq)])

    def mostrar_turnos(reapresentada=None):
        ts = turnos_do_colab(c, ini, fim, batida_reapresentada=reapresentada)
        print("TURNOS_CONTAGEM", pk(reapresentada), len(ts))
        for t in ts:
            print("TURNO", pk(reapresentada), local(dia0(t.data_turno)),
                  pk(t.entrada), pk(t.saida), int(t.aberto),
                  len(t.batidas), [b.pk for b in t.batidas])
            for i, pares in enumerate((intervalos_do_turno(t), pausas_abaixo_do_piso(t)), 1):
                print("PAUSAS", i, len(pares), [(local(a), local(z)) for a, z in pares])

    mostrar_turnos()
    if grupo == 3 and cid == 1010 and 146307 in por_pk and 146307 not in vivas:
        mostrar_turnos(por_pk[146307])

    ref = fim + um
    grade = utils.montar_grade_prevista_periodo(
        c, ap, dias=(fim - ini).days + 2, ref=ref, agora=agora,
    )
    for i, linha in enumerate(grade["dias"]):
        print("GRADE", local(dia0(ref - i * um)), len(linha.get("orfas") or []),
              sum(bool(x and x.get("missing")) for x in linha.get("celulas") or []))

    ces = [x.pk for x in cels.values() if ini <= x.data <= fim]
    qset = PerguntaDisputa.objects.filter(colaborador_id=cid).filter(
        Q(pk__in=qids) | Q(data_evento__range=(ini, fim)) | Q(celula_id__in=ces) |
        Q(batida_origem_id__in=alvos) | Q(batidas_plantadas__pk__in=alvos) |
        ((Q(chamado_destino_id__in=hids) | Q(disputa__chamado_id__in=hids)) &
         Q(data_evento__isnull=True))
    ).distinct().select_related("disputa", "chamado_destino").prefetch_related("decisoes")

    chamados = set(hids)
    for p in qset.order_by("pk"):
        

[truncado em 20000 de 28989 caracteres]

## 27/11 11:24 · sessao 9158a4607d0c · PAR-PELO-TURNO · construtor · origem=Code

Adicionei os cinco REDs em um único arquivo. No HEAD `4ba908393`, os cinco falharam em asserções na execução em memória, com dez falhas de subteste. O runner Django parou no import de `django.core`. Suíte integrada e certificação [nao verificado]. [test_par_c1v8_reds.py](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_c1v8_reds.py:77) · [manage.py:11](/home/ronald/wt-codex-par-pelo-turno-111717/app/manage.py:11)

| Item | O que o teste afirma | Por que dá RED no HEAD | [nao verificado]
|---|---|---|
| 1 | A entrada de 23/10 fica em **23/10** e, no cenário da troca de vínculo, a última saída fica em **22/10** em todos os leitores. | A janela de 21/10 deixa a primeira entrada sem a saída e ela herda 22/10. Na troca, `turnos=` muda a ordem dos turnos e a última atribuição grava 21/10 para a saída. [Teste](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_c1v8_reds.py:81) · [turnos.py:287](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/turnos.py:287) · [turnos.py:3040](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/turnos.py:3040) · [turnos.py:3085](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/turnos.py:3085) | [nao verificado]
| 2 | Com e sem bordas, o par é **19:00→07:00**, com **60 min** de pausa. | Com bordas, a saída de 30 s zera `limite` na pré-passagem. O resultado são dois pares, ambos sem pausa. [Teste](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_c1v8_reds.py:111) · [turnos.py:508](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/turnos.py:508) | [nao verificado]
| 3 | A casca e a folha datam as duas marcas em **21/10** e o saldo fica em **0,00 h** com ou sem elas. | A projeção da folha passa `ultima_batida=None`, perde o fato posterior da O95 e data as marcas em 20/10. Isso conta como movimento e o saldo cai para **−193,60 h**. [Teste](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_c1v8_reds.py:124) · [fechamento.py:165](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/services/fechamento.py:165) · [fechamento.py:679](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/services/fechamento.py:679) · [fechamento.py:1123](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/services/fechamento.py:1123) | [nao verificado]
| 4 | O par fechado de 26 h é reconhecido com vínculo noturno, diurno ou ausente em 22/10. | A guarda retorna `False` nos braços diurno e sem vínculo antes de consultar o par. [Teste](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_c1v8_reds.py:178) · [servico_jornada.py:232](/home/ronald/wt-codex-par-pelo-turno-111717/app/escala/servico_jornada.py:232) · [servico_jornada.py:236](/home/ronald/wt-codex-par-pelo-turno-111717/app/escala/servico_jornada.py:236) | [nao verificado]
| 5 | A janela de 21/10 soma **8,00 h** com a borda antiga do PDF e com `ini−4`. | Com a carga ampla, o vão de 5 h faz o motor sobrescrever o dia do juiz com o da jornada anterior. O recorte exclui o segundo par e soma **0,00 h**. [Teste](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_c1v8_reds.py:200) · [motor_calculo_v2.py:2521](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/motor_calculo_v2.py:2521) · [motor_calculo_v2.py:2542](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/motor_calculo_v2.py:2542) | [nao verificado]

Na fixture, usei `08:00–17:48` para o cadastro diurno e representei a marca posterior de 23/10 às 08:00 como `E`. O item 3 executa os trechos reais de projeção, movimento e saldo em memória. [test_par_c1v8_reds.py:26](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_c1v8_reds.py:26) · [test_par_c1v8_reds.py:135](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_c1v8_reds.py:135) · [test_par_c1v8_reds.py:140](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_c1v8_reds.py:140)

### Comandos em `app/`

Cada `red` retornou `1`. Cada `mutante` retirou em memória a condição que dispara o RED e retornou `0`. O executor reutiliza os dublês da sonda vizinha. [par_pelo_turno_c1v6_verificacao.py:59](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/par_pelo_turno_c1v6_verificacao.py:59) · [test_par_c1v8_reds.py:77](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_c1v8_reds.py:77)

```bash
rodar() {
  PYTHONDONTWRITEBYTECODE=1 python3 - "$@" <<'PY'
import importlib
import sys
import unittest
from pathlib import Path

from ponto.tests.par_pelo_turno_c1v6_verificacao import _instalar_dubles_de_infraestrutura

_instalar_dubles_de_infraestrutura()
mod = importlib.import_module('ponto.tests.test_par_c1v8_reds')
modo, item = sys.argv[1:]
mutantes = {
    '1': [('for dia in dias:', 'for dia in dias[1:]:')],
    '2': [('in (None, (dt.time(19), dt.time(7))):', 'in (None,):')],
    '3': [('for marcas in ([], [e, s]):', 'for marcas in ([],):'),
          ('self.assertEqual(saldos, [0.0, 0.0])', 'self.assertEqual(saldos, [0.0])')],
    '4': [("in ('noturno', 'diurno', 'sem vínculo'):", "in ('noturno',):")],
    '5': [("            ('ini−4', meia_noite - 4 * UM_DIA),\n", '')],
}
if modo == 'mutante':
    fonte = Path(mod.__file__).read_text()
    for antes, depois in mutantes[item]:
        assert fonte.count(antes) == 1
        fonte = fonte.replace(antes, depois, 1)
    exec(compile(fonte, mod.__file__, 'exec'), vars(mod))
testes = unittest.defaultTestLoader.loadTestsFromTestCase(mod.ParPeloTurnoC1v8RedsTest) [nao verificado]
suite = unittest.TestSuite(t for t in testes if t.id().split('.')[-1].startswith(f'test_{item}_')) [nao verificado]
assert suite.countTestCases() == 1 [nao verificado]
raise SystemExit(not unittest.TextTestRunner().run(suite).wasSuccessful()) [nao verificado]
PY
}

rodar red 1
rodar red 2
rodar red 3
rodar red 4
rodar red 5

rodar mutante 1
rodar mutante 2
rodar mutante 3
rodar mutante 4
rodar mutante 5
```

## 27/11 11:23 · sessao 80619ebac554 · RAMO2-B-EM-TURNO-QUE-CRUZA · construtor · origem=Code

Adicionei somente [test_ramo2b_motor_reds.py](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-111717/app/ponto/tests/test_ramo2b_motor_reds.py:1). No ensaio isolado, o HEAD `1f9541e71` deu **2 REDs por asserção e 2 testes de paridade verdes**. Com o motor do main `59600163c` carregado em memória, os quatro passaram. O ensaio usou adaptações mínimas para carregar o motor sem Django completo. A execução na suíte do Code fica [nao verificado]. [Bateria de testes](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-111717/app/ponto/tests/test_ramo2b_motor_reds.py:68)

| Item e teste | Valor afirmado e derivação | Resultado no HEAD | [nao verificado]
|---|---|---|
| 1 · Intermitente | **720 trabalhados, 540 noturnos de relógio, 30 fora**. O main corta às 07h. São 12h desde 19h e 9h noturnas desde 22h, com prorrogação. Main `app/ponto/motor_calculo_v2.py:1863`, `:2898`. [Asserção](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-111717/app/ponto/tests/test_ramo2b_motor_reds.py:69) | **RED.** A classe herda a cauda e devolve **750, 570, 0**. [Herança](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-111717/app/ponto/motor_calculo_v2.py:3841) · [Recorte](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-111717/app/ponto/motor_calculo_v2.py:1917) |
| 2 · Comercial mensal | **610 trabalhados, 130 min de HE, saldo −36,00 h**. O previsto é `720 − 60 = 660`. Com saída às 07h, o main aplica `610 − 480` para a HE e `(610 − 130 − 2640) / 60` para o saldo. Main `app/ponto/motor_calculo_v2.py:3504`, `:3526`, `:3570`. [Asserção](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-111717/app/ponto/tests/test_ramo2b_motor_reds.py:82) | **RED.** O limite sobe para 660 mesmo com saída no marco. O resultado é **0 min de HE e −33,83 h**. [Limite](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-111717/app/ponto/motor_calculo_v2.py:3676) · [Saldo](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-111717/app/ponto/motor_calculo_v2.py:3709) |
| 3 · Partido e Partido Noturno, caso de 490 | **490 trabalhados** (`250 + 240`), **360 noturnos de relógio** (`120 + 240`), **HE, intra e dobra zero**. O vão de 180 min foi gozado, e `490 < 540`. O abono reduz o atraso de 50 para 20 min. As saídas às 07h e 07h20 deixam 0 e 20 min fora, respectivamente. Main `app/ponto/motor_calculo_v2.py:3002`, `:3103`, `:3244`. [Asserções](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-111717/app/ponto/tests/test_ramo2b_motor_reds.py:106) | **Verde em ambos os motores.** Com a cauda reintroduzida em memória, a saída às 07h20 sobe para **510 trabalhados** e quebra a asserção. [Recorte do Partido](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-111717/app/ponto/motor_calculo_v2.py:3103) |
| 3 · Partido e Partido Noturno, HE legal e dobra | **560 trabalhados** (`320 + 240`), **380 noturnos de relógio** (`140 + 240`), **20 min de intra** (`180 − 160`). Em dia comum, a HE50 é `560 + 380/7 − (480 + 20) = 800/7` min e a dobra é zero. No feriado, a HE100 de dobra é **320 + 240 min** e a HE50 é zero. As saídas às 07h e 07h30 preservam essas rubricas. Main `app/ponto/motor_calculo_v2.py:3103`, `:3213`, `:3248`. [Asserções](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-111717/app/ponto/tests/test_ramo2b_motor_reds.py:120) | **Verde em ambos os motores.** Os mutantes de cauda, intra, ganho noturno da HE e dobra quebram as respectivas asserções. [Campos conferidos](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-111717/app/ponto/tests/test_ramo2b_motor_reds.py:93) |

Os dois primeiros mutantes já estão presentes no HEAD. Os quatro do Partido foram aplicados em memória. Cada variante saiu com `rc 1` por asserção, com zero erros. Os comandos abaixo usam o Django do ambiente do Code, cuja execução aqui fica [nao verificado]. [Alvos](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-111717/app/ponto/tests/test_ramo2b_motor_reds.py:68) [nao verificado]

```bash
ensaio_motor() {
  PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.base python3 - "$1" <<'PY'
from contextlib import nullcontext
import sys
import unittest
from unittest.mock import patch

import django
django.setup()
from ponto import motor_calculo_v2 as motor

classe = "ponto.tests.test_ramo2b_motor_reds.Ramo2bMotorRedsTest"
intermitente = ".test_intermitente_mantem_o_recorte_no_marco"
comercial = ".test_comercial_mensal_com_saida_no_marco_mantem_limite_de_480"
partido_490 = ".test_partidos_preservam_os_490_minutos_com_abono"
partido = ".test_partidos_preservam_intra_he_legal_e_dobra"
variantes = {
    "head": (nullcontext(), [""]),
    "intermitente_cauda": (nullcontext(), [intermitente]),
    "comercial_previsto_sem_cauda": (nullcontext(), [comercial]),
    "partido_cauda": (patch.object(motor.MotorTurnoPartido, "CAUDA_DO_TURNO_QUE_CRUZA_NO_TRABALHADO", True),
                      [partido_490, partido]),
    "partido_intra": (patch.object(motor, "TOLERANCIA_INTRA_MIN", 20), [partido]),
    "partido_he": (patch.object(motor, "_ganho_noturno_da_base_da_he", return_value=0), [partido]),
    "partido_dobra": (patch.object(motor.MotorTurnoPartido, "_feriado_dobra_no_dia", return_value=False), [partido]),
}
mutacao, alvos = variantes[sys.argv[1]]
with mutacao:
    suite = unittest.defaultTestLoader.loadTestsFromNames([classe + alvo for alvo in alvos]) [nao verificado]
    resultado = unittest.TextTestRunner(verbosity=2).run(suite) [nao verificado]
raise SystemExit(2 if resultado.errors else int(bool(resultado.failures)))
PY
}

ensaio_motor head
ensaio_motor intermitente_cauda
ensaio_motor comercial_previsto_sem_cauda
ensaio_motor partido_cauda
ensaio_motor partido_intra
ensaio_motor partido_he
ensaio_motor partido_dobra
```

## 27/11 11:15 · sessao 50281f6e60fe · RC-SEM-PROXIMA · verificador · origem=Code

## REPROVADO

### 1. MÉDIA — R1 curado na origem para remoção ou troca após a visita, com nova regressão de leitura

`_obras_vivas` captura mtime com `fstat` e lê o texto pelo mesmo descritor. `sem_colher` extrai o `rc=` do texto já capturado. Nas sondas em memória, a c2 perdeu a saída após remoção e misturou `10:00` com `rc=7` após troca. A c3 preservou `10:00 rc=1` nos dois casos. `caso_leitura_removido` e `caso_leitura_trocado` exigem essa linha e rc `3`. Os mutantes `le-tarde-removido` e `le-tarde-trocado` reintroduzem a abertura tardia e caíram nas sondas. [bin/rc_sem_colher.py:90–100](/home/ronald/codex_ws/1e824c3f77f507fdc1a0ee18f7ac268e18d1b3d9/bin/rc_sem_colher.py:90), `bin/rc_sem_colher.py:103–116`, `bin/tests/test_rc_sem_colher.sh:152–156,179–183,212–218,349–350`.

A c3 abre **todo** `.rc` regular antes de filtrar idade e escolher o mais novo. Na sonda, um `.rc` vencido com `PermissionError` derrubou a chamada inteira, enquanto a c2 listou o órfão válido. O mesmo ocorreu com um `.rc` colhido e ilegível que a c2 listava em `SEM PROXIMA`, classe que dispensa seu conteúdo. O selo cobre remoção e troca **após** a leitura e erro de diretório. Esses dois casos de erro de arquivo ficam descobertos. `c2:bin/rc_sem_colher.py:77–91,97–106,124–138`, `bin/rc_sem_colher.py:84–100,137–152,176–187`, `bin/tests/test_rc_sem_colher.sh:179–183,212–218,257–260,297`.

### 2. MÉDIA — R2 curado na origem, com selo no juiz e lacuna no leitor

O juiz imprime uma linha final opcional a partir de `len(colher)`, `len(proxima)` e `min_idade`, usando as próprias listas que acabou de emitir. `caso_resumo` exige uma linha de órfão, três de parada, um único resumo final, `min_idade` correto e ausência de resumo na chamada sem a flag. `caso_resumo_vazio` exige os dois zeros com rc `0`. Os mutantes `resumo-trocado`, `resumo-soma`, `resumo-sem-min-idade` e `resumo-mudo-no-zero` caíram nas sondas em memória. `bin/rc_sem_colher.py:156–187`, `bin/tests/test_rc_sem_colher.sh:228–240,353–356`.

O `vercodex.diff` chama `--min-idade 900 --resumo` e incorpora ao cabeçalho a última linha `RESUMO ` tal como veio. O limite de 15 linhas e 120 caracteres vale para os detalhes. Confirmei com uma resposta simulada contendo uma linha de detalhe e resumo `7/21`. O leitor imprimiu `7/21`. Para rc `0` e `3`, imprime cabeçalho e detalhes. Para outro rc, imprime também a última linha do stderr, ou `(stderr vazio)`. Se faltar resumo, imprime `SEM RESUMO do juiz`, inclusive com rc `0` ou `3`. Esse bloco não propaga o rc do juiz. O selo do juiz continuaria verde se o vercodex perdesse as flags ou voltasse a contar linhas. `vercodex.py:165–176` no anexo `vercodex.diff`, `bin/tests/test_rc_sem_colher.sh:64,228–240,353–356`.

### 3. ALTA — R3 corrige a colisão original, mas ainda lista com 10 segundos reais em DST

A função gera um instante por dia civil, no dia do `.rc` e no seguinte, com `mktime(..., tm_isdst=-1)`. Aplica o piso inclusivo do início do minuto do `.rc` e o teto inclusivo de `agora`, e escolhe o maior instante restante. A listagem exige **mais de 900 s desde o fim** do minuto escolhido. `caso_dia_da_marca` passou de rc `3` na c2 para rc `0` vazio na c3. `caso_dia_da_marca_ontem` manteve rc `3` às `00:06:01`. Os mutantes `marca-mais-antiga` e `marca-sem-teto` caíram, respectivamente, nesses casos. `bin/rc_sem_colher.py:119–129,143–152`, `bin/tests/test_rc_sem_colher.sh:157–163,220–226,351–352`.

- **Dois dias civis com menos de 24 h reais.** Em `America/New_York`, `.rc` em `07/03/2026 23:50`, marca real em `09/03 00:10:10` e `agora` às `00:10:20`. O `.rc` tinha `84.020 s` e passou pelo corte. O juiz inferiu `08/03 00:10` e o `main` retornou rc `3` após **10 s reais**. Um `.rc` com mais de `86.400 s` reais fica fora já na passagem pela árvore. [bin/rc_sem_colher.py:77–95](/home/ronald/codex_ws/1e824c3f77f507fdc1a0ee18f7ac268e18d1b3d9/bin/rc_sem_colher.py:77), `bin/rc_sem_colher.py:119–129,146–152`.
- **Hora repetida e todas as marcas `None`.** Em processos novos com `America/New_York`, em `01/11/2026`, um `.rc` às `00:50` e uma marca às `01:10:10 -0500` deram listagem após **10 s**, porque a candidata foi `01:10 -0400`. Com `.rc` às `01:00 -0500` e a mesma marca, a primeira ocorrência caiu abaixo do piso e o dia seguinte passou do teto. A parada real de **20 min** sumiu das duas listas. É coerente deixar o prazo indeterminado quando faltam candidatas. Aqui faltou representar uma ocorrência válida, e `sem_colher` já excluiu a obra pela chave da marca. `bin/rc_sem_colher.py:106–115,119–129,141–151`.
- **Virada comum e mtime futuro.** `.rc` às `23:50` e marca às `00:10` do dia seguinte funcionam. Se a marca entra perto do fim da janela de 24 h, o `.rc` pode vencer antes de o prazo amadurecer. A colisão das `23:50` também pode ocultar uma marca realmente antiga quando a candidata de hoje acaba de entrar no teto. Já um `rc_m` futuro pode ganhar a seleção: com piso futuro recebe `None`, e no mesmo minuto ainda fica aquém do prazo. `bin/rc_sem_colher.py:77–95,108–115,119–129,146–152`, `bin/tests/test_rc_sem_colher.sh:97–104,157–163`.

### 4. BAIXA — O selo novo do resumo usa relógio de parede

O instrumento lê `time.time()` uma vez no `main` e passa o mesmo `agora` à varredura e às duas classificações. `_cenario` injeta instantes fixos. Já `caso_resumo` e `caso_resumo_vazio` usam `_arvore_proxima`, que lê `date +%s`, e chamam o juiz, que consulta o relógio novamente. O caso misto faz três chamadas em instantes distintos. `bin/rc_sem_colher.py:173–179`, `bin/tests/test_rc_sem_colher.sh:64–82,122–131,167–170,228–240`.

## Escritores, leitores e compatibilidade

| Valor | Caminho |
|---|---|
| Tupla `(ob, d, rc_m, rc_p, rc_txt, outro)` | `_obras_vivas` monta e emite. `main` materializa uma vez. `sem_colher` usa os seis campos. `sem_proxima` usa os outros cinco e descarta `_txt`. Esses são os consumidores encontrados na cópia. `bin/rc_sem_colher.py:74–115,132–142,176–179`. |
| `rc=` do `.rc` | `codex_pergunta.sh::_saida` grava o valor e publica por `mv`. `_obras_vivas` lê até 200 caracteres. `_rc_de` procura `rc=(\d+)`, depois o primeiro grupo de dígitos, ou devolve `?`. `sem_colher` entrega o valor a `main`, que imprime `rc=…`. `bin/codex_pergunta.sh:565–574`, `bin/rc_sem_colher.py:64–67,90–95,108–115,180–181`. |
| `RESUMO` e rc do processo | `main` escreve o resumo quando solicitado. `--min-idade` filtra só `SEM COLHER`. O vercodex proposto lê a linha pronta. `ESPERA_UNICA` chama sem flags, deixa passar stdout, entra no laço com rc `0`, pula com `3` e termina com sucesso para ambos. O `find` final também pode imprimir caminhos. `espera_nega` examina o comando Bash. `bin/rc_sem_colher.py:108–115,166–187`, `bin/disjuntor.py:1963–1969,2157–2174`, `vercodex.py:165–174` no anexo. |

**Saída sem `--resumo`, byte a byte:** a constante `ESPERA_UNICA` e os formatos das duas linhas ficaram iguais. A sonda mista comum deu stdout idêntico entre c2 e c3. A igualdade geral é falsa: no caso das `23:50`, a c2 emitiu `SEM PROXIMA` e pulou o laço, enquanto a c3 teve stdout vazio no juiz e entrou no laço. Confirmei esse desvio com o Bash da espera e dependências simuladas. `bin/disjuntor.py:1967–1969`, `c2:bin/rc_sem_colher.py:164–169`, `bin/rc_sem_colher.py:180–187`, `bin/tests/test_rc_sem_colher.sh:157–163,220–226`.

**Esmeril final.** Contagem de `^\+.*except`: **0** no `diff.patch` e **0** no `vercodex.diff`. O `except Exception` é contexto. A classificação e as contas ficam no juiz. Dentro dele, o parser puro roda duas vezes sobre o mesmo texto e o predicado de folga aparece nas duas listas. As sondas foram em memória e `bash -n` passou. RED/GREEN integrais, `test_disjuntor.sh`, Ruff, instalação do vercodex e ocorrência desses cenários em produção ficam [nao verificado]. `bin/rc_sem_colher.py:48–61,106–115,135–150,173–187`, `bin/tests/test_rc_sem_colher.sh:35–37,297–314`, `vercodex.py:167–176` no anexo.

## 27/11 11:11 · sessao c71da07a488e · E4C-F5-INSTANTE · verificador · origem=Code

## APROVADO — ver3 do E4C-F5-INSTANTE

A volta cura P3 na origem e fecha a lacuna de trava na auditoria pós-rollback. O teste multiempresa confirma a regra de estoque já presente na cura2. O item 6 continua fora desta volta. [materializacao.py:846](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:846) · [validacao.py:833](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:833) · [test_e4c_f5_c4_red.py:932](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:932) · [test_e4c_f5_c4_red.py:634](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:634)

### Procedentes do ver2

1. **MÉDIA · P3 curado na origem.** Os dois caminhos de veto passam `unica=True` pelo ponto comum `trilha_do_veto_k6`. O escritor procura uma linha com a mesma identidade e só insere se ela faltar. O RED original exige estado preservado e uma linha após duas validações. O RED fora de request repete o mesmo usuário e comando e exige uma linha, além de exigir uma linha distinta para outro comando. **M44** desliga a opção no chamador e **M46** a ignora no escritor. Ambos atingem a contagem do segundo RED. A cura evita novas duplicatas e reaproveita uma linha se já houver histórico igual. [materializacao.py:846](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:846) · [observ.py:138](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:138) · [test_e4c_f5_c4_red.py:1055](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1055) · [e4c_f5_c4red2_mutantes.py:757](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/e4c_f5_c4red2_mutantes.py:757)
2. **MÉDIA · Pós-rollback curado na origem.** O novo `atomic` toma a mesma trava do dono pela porta existente e relê a pergunta antes de auditar. O RED força o veto tardio em duas tentativas, exige uma linha, confere a foto após o rollback e exige o dono registrado nas duas recusas. **M45** remove a aquisição. A mordida vem da asserção do sentinela, mesmo que a contagem ainda dê um em execução serial. O teste retira previamente os sentinelas da fixture e simula o momento da leitura do K6. Uma disputa real entre conexões nesse ramo fica **[nao verificado]**. [validacao.py:833](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:833) · [test_e4c_f5_c4_red.py:1087](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1087) · [test_f5_v3_guarda_sob_trava.py:28](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_f5_v3_guarda_sob_trava.py:28) · [e4c_f5_c4red2_mutantes.py:762](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/e4c_f5_c4red2_mutantes.py:762)
3. **BAIXA · Multiempresa é controle.** A ordem do teste é Empresa A → dono A → Empresa B → dono B. A regra herdada já recusa B por vir depois do **primeiro** dono do lote. O teste exige a mensagem específica e as duas fotos intactas. **M40** continua ligado ao RED de uma Empresa e um dono. O controle multiempresa veio sem mutante próprio. [test_e4c_f5_c4_red.py:932](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:932) · [export.py:602](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/folha/export.py:602) · [e4c_f5_c4red2_mutantes.py:724](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/e4c_f5_c4red2_mutantes.py:724)
4. **BAIXA · M16 conserva a mordida.** Cada passagem de materialização e passivo ganha seu savepoint revertido. As asserções ainda exigem uma trilha nova com fotos e motivo para cada metade. M16 apaga fotos e descrição e derruba a asserção de conteúdo nos quatro subcasos. O teste sem banco já substitui a função inteira de trilha e continua provando o encaminhamento de uma tentativa. [test_e4c_f5_c4_red.py:1017](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1017) · [test_e4c_f5_c4_red.py:1170](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1170) · [e4c_f5_c4red2_mutantes.py:254](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/e4c_f5_c4red2_mutantes.py:254) · [e4c_f5_c4red2_mutantes.py:475](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/e4c_f5_c4red2_mutantes.py:475) · [test_bug_par_sem_banco.py:153](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_bug_par_sem_banco.py:153)

### Campos de `evento(unica=True)`

`evento` escreve todos os campos pelo mesmo `create`. A consulta de duplicata lê os oito campos de uma cópia de `registro`. A tabela aponta quem fornece cada valor e onde a trilha é lida no recorte. [observ.py:138](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:138)

| Campo | Quem fornece | Leitor da trilha |
|---|---|---|
| `acao` | `trilha_do_veto_k6` fornece `veto_competencia_do_fato`. [materializacao.py:858](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:858) | RED seleciona a ação. [test_e4c_f5_c4_red.py:1065](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1065) |
| `modelo` | A trilha fornece `PerguntaDisputa`. [materializacao.py:858](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:858) | `_trilha` seleciona o modelo. [test_e4c_f5_c4_red.py:1014](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1014) |
| `objeto_id` | A trilha passa `p.pk`. [materializacao.py:858](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:858) | `_trilha` seleciona o PK. [test_e4c_f5_c4_red.py:1014](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1014) |
| `descricao` | A guarda produz o motivo e a trilha passa `texto[:500]`. [materializacao.py:826](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:826) · [materializacao.py:858](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:858) | `_ver_trilha` verifica o motivo e a tela o exibe. [test_e4c_f5_c4_red.py:1025](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1025) · [log_auditoria.html:73](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/templates/core/log_auditoria.html:73) |
| `valor_antes` | A trilha monta a foto do texto e dos três instantes. [materializacao.py:854](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:854) | RED pós-rollback compara a foto. [test_e4c_f5_c4_red.py:1119](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1119) |
| `valor_depois` | A trilha passa `dict(foto)`. [materializacao.py:859](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:859) | O mesmo RED exige igualdade com a foto anterior. [test_e4c_f5_c4_red.py:1119](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1119) |
| `usuario` | `evento` usa o autor explícito com PK ou, se o argumento `ator` for `None`, o usuário autenticado da request. Sem autor resolvido com PK, grava nulo. [observ.py:113](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:113) | `_trilha` seleciona `self.admin` e a tela filtra pela FK. [test_e4c_f5_c4_red.py:1014](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1014) · [views.py:705](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/views.py:705) |
| `ator` | `evento` grava `sistema:<comando>` quando reconhece o processo fora de request, inclusive com usuário explícito. Em request ou sem comando reconhecido, o campo fica nulo. [observ.py:22](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:22) · [observ.py:113](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:113) | O RED distingue os dois comandos pelo campo `ator`. [test_e4c_f5_c4_red.py:1081](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1081) |

**Mesma autoria no filtro e no `create`, sim.** `usuario` e `ator` são resolvidos uma vez. Deixar `ip` fora da identidade é coerente com o veto por pergunta, motivo, fotos e autoria. Uma repetição por outro IP reutiliza a linha encontrada e conserva o IP dela. `unica` é opt-in e a trilha K6 é o único chamador que o ativa na cópia. Os demais, inclusive `evento_kw` e `registrar_log`, seguem direto ao `create` com os valores resolvidos. [observ.py:94](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:94) · [observ.py:113](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:113) · [observ.py:138](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:138) · [observ.py:157](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:157) · [auditoria.py:22](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/auditoria.py:22)

**JSONField.** No Django fixado pela cópia, `create` com `None` simples numa `JSONField(null=True)` grava **SQL NULL**. `campo__isnull=True` casa esse valor. Um `null` dentro do dicionário continua sendo parte do JSON comparado por igualdade. A trilha K6 sempre passa dois dicionários, então seus REDs deixam o ramo de `None` no campo inteiro sem exercício. SQL efetivamente emitido **[nao verificado]**. [requirements.txt:13](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/requirements.txt:13) · [models.py:300](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/models.py:300) · [observ.py:143](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:143) · [materializacao.py:854](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:854)

### Travas em todos os caminhos da trilha

- **`validacao.py:242`, antes do carimbo.** A recusa de `:797` roda dentro do `atomic` que já chamou `travar_o_ato_do_plantio`. Essa função toma o dono pela `trava_na_ordem`. [validacao.py:242](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:242) · [validacao.py:755](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:755) · [validacao.py:795](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:795) · [materializacao.py:2223](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:2223)
- **`validacao.py:242`, depois do rollback.** O rollback elimina a aquisição e o sentinela feitos naquele bloco. Uma trava já detida pelo `atomic` externo sobrevive, como pode ocorrer na validação em lote. O novo bloco usa a porta reentrante e relê a pergunta. Há precedente de `trava_na_ordem(colab_id)` só para proteger consulta de identidade e escrita em `lavrar`. [validacao.py:833](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:833) · [trava_do_colaborador.py:16](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/trava_do_colaborador.py:16) · [validacao.py:1172](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:1172) · [par_relampago_na_lista.py:116](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/ponto/services/par_relampago_na_lista.py:116)
- **`materializacao.py:2004`.** O laço da disputa toma a trava antes da unidade. O passivo `reparar` toma a trava antes de `_trabalhar_o_par` → `_plantar_o_dia` → `_materializar_uma`. A validação chega à mesma unidade pela tentativa do par ou pela metade avulsa sob a trava de `:758`. A chamada recursiva para a irmã ocorre dentro do ato da principal, sob o mesmo dono. O RED do passivo chama `_plantar_o_dia` diretamente e prova conteúdo da trilha, enquanto a aquisição da trava no caminho de produção se verifica em `reparar`. [materializacao.py:2004](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:2004) · [materializacao.py:2301](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:2301) · [passivo_validado.py:393](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/passivo_validado.py:393) · [passivo_validado.py:491](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/passivo_validado.py:491) · [passivo_validado.py:585](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/passivo_validado.py:585) · [validacao.py:687](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:687) · [validacao.py:826](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:826) · [materializacao.py:1841](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:1841) · [test_e4c_f5_c4_red.py:1182](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1182)

**Empresa antes do dono.** Nenhuma das duas entradas de `_recusa_da_entrega` exige a trava da Empresa. A primeira já tem o dono e chega com a decisão da guarda. A segunda toma só o dono e reutiliza o veto obtido antes do rollback. A exigência explícita de Empresa antes do dono pertence ao registro de **estoque**, que confere a ordem antes do K6. Assim, nos caminhos da recusa, a ausência da trava da Empresa é compatível com o contrato da trilha. A trava do dono serializa consulta e inserção da auditoria. O motivo do veto tardio continua sendo o da tentativa anterior ao rollback, enquanto a foto vem da releitura posterior. [validacao.py:237](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:237) · [validacao.py:795](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:795) · [validacao.py:833](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/validacao.py:833) · [models.py:4873](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/models.py:4873) · [models.py:4892](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/models.py:4892)

### Esmeril

5. **BAIXA · Semântica e autoria.** O parâmetro `ator` recebe o autor humano, enquanto a coluna `ator` identifica o processo. Essa diferença é herdada e o filtro usa as duas colunas já resolvidas. Também seguem herdados `data_evento` como dia do turno ou dia civil da conversa e `entrada_em` como instante de inserção do ato ou marco convencional do estoque. [observ.py:113](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:113) · [models.py:3086](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/models.py:3086) · [models.py:3097](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/models.py:3097) · [models.py:4402](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/models.py:4402)
6. **BAIXA · Alcance dos selos.** M44 e M46 provam que a dedupe é usada. O RED fora de request varia o comando e mantém sempre o mesmo usuário, portanto a retirada isolada de `usuario` do filtro escaparia desse teste. M45 prova a presença da trava pelo registro transacional. M20 continua apontando para o RED já vermelho do item 6, onde o `post_save` e `DecisaoPergunta.registrar` chegam ao mesmo emissor. [test_e4c_f5_c4_red.py:1068](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1068) · [test_e4c_f5_c4_red.py:1106](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:1106) · [e4c_f5_c4red2_mutantes.py:489](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/e4c_f5_c4red2_mutantes.py:489) · [signals.py:253](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/signals.py:253) · [models.py:4791](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/models.py:4791)
7. **BAIXA · Relógio, derivação, escritor e `except`.** O delta usa os instantes da pergunta na foto e o `criado_em` já definido no modelo. Os testes novos usam o congelamento da classe. Segue herdado `carimbar` chamar `pergunta_viva(self)` sem passar o relógio que resolveu, com possibilidade de `localdate()` no degrau da janela. K6 segue no juiz registrado. A identidade da trilha deriva do mesmo `registro` que alimenta o único `create`. No diff fornecido, **`^\+.*except` = 0**. A captura existente continua em `engolir`. [materializacao.py:854](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/services/materializacao.py:854) · [models.py:307](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/models.py:307) · [test_e4c_f5_c4_red.py:68](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/tests/test_e4c_f5_c4_red.py:68) · [models.py:3460](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/models.py:3460) · [models.py:3496](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/models.py:3496) · [juizes.py:642](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/chamados/juizes.py:642) · [juizes.py:350](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/juizes.py:350) · [observ.py:85](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:85) · [observ.py:138](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:138)

O veredito é técnico, sobre o delta fornecido. A alteração em `core/observ.py` extrapola a lista dos briefs 3 e 3b anexados. O aval específico da 3c, a reprodução independente dos números da pista e a ocorrência em produção ficam **[nao verificado]**. [observ.py:94](/home/ronald/codex_ws/8ea46953ecf2787e1db084b82bba42ba76876648/app/core/observ.py:94)

## 27/11 11:09 · sessao ccb1176605bd · ORFAS-VOLTOU · sonda · origem=Code

## B1 — Universo

A sonda terminou com `FIM 1895 1895`, sem erro de execução na saída fornecida. O `confere False` é a divergência com o aval. `sonda_leitura:3,137`

| Medida | Sonda | Aval | Diferença |
|---|---:|---:|---:|
| Perguntas | 1.895 | 1.890 | +5 |
| Colabs | 184 | 183 | +1 |
| Células | 658 | 657 | +1 |
| Células com chamado | 481 | 480 | +1 |
| Células sem chamado | 177 | 177 | 0 |
| `indefinida` | 347 | 347 | 0 |
| `furo` | 175 | 174 | +1 |
| `nunca_bateu` | 51 | 50 | +1 |
| Outros | 85 | 86 | −1 |

Os totais com e sem chamado coincidem pelos dois critérios da sonda, FK da célula e `dia_do_fato`. Há 12 perguntas sem FK de célula. Os 85 outros vereditos são `concorde` 61, nulo 3, `cobrado` 8, `cobranca_indevida` 4, `discordante` 8 e `fato_em_ausencia` 1. `sonda_leitura:3–6` `aval_v2.txt:1` `app/chamados/juizes.py:89–130`

O corte declarado é 21/10 nos dois anexos. A sonda usa `data_evento >= 2026-10-21`, destino, validação e materialização nulos e via vazia, sem exigir disputa aberta. A consulta original, o instante da leitura do aval e os PKs da diferença estão [nao verificado]. A saída permite medir a diferença, mas sua causa — mudança de estoque, diferença de filtro ou de data — está [nao verificado]. `aval_v2.txt:1` `sonda_leitura:3–6`

## B2 — Classes e portas

As **103 classes somam 1.895**. Cada ID da saída aparece uma vez abaixo. Uso **(a)** para histórico e, se vazio, c1/c2, e **(b)** para destino vivo, conforme a nomenclatura da sonda. `sonda_leitura:7–109,137` `app/chamados/reconciliador.py:1711–1779,1800–1826`

| Classes da sonda | Perguntas | Porta existente e condição de fecho |
|---|---:|---|
| 1–2 | 5 | Disputa já fechada, pergunta ainda viva. **Nenhuma porta automática para esse resíduo no estado medido.** O reate e o S128 exigem disputa aberta. A decisão explícita do admin sobre o fato do dia tem porta própria, cuja aplicação a essas linhas está [nao verificado]. `app/chamados/reconciliador.py:1654–1661` `app/chamados/services/disputa_emissao.py:2038–2042,2155–2170` `app/ponto/services/veredito_celula.py:178–202` |
| 26–27 | 2 | C6 dá a pergunta por morta e a célula está `concorde`. **Lavra da célula → `retratar_perguntas_absolvidas_por_celula`**, que carimba `celula_absolveu` se a tranca permitir. O **S128 sem credor** também pode alcançar as duas numa execução para o colab, se passar pelas guardas da disputa. `app/chamados/reconciliador.py:695–707,747–789,1513–1515` `app/chamados/services/disputa_emissao.py:2155–2193` |
| 13, 28 | 11 | Fora do reate, vivas e sem FK de célula. A leitura da lâmpada fica sem prova. O `sem_chamado` dessas linhas vem da falta da FK na sonda. **Nenhuma porta automática de fecho demonstrada**. O ato humano por colab e data existe para pergunta datada e fora de conversa, se houver decisão sobre o fato. `sonda_leitura:19,34` `app/chamados/reconciliador.py:1654–1661` `app/chamados/juizes.py:979–995` `app/ponto/services/veredito_celula.py:178–202` |
| 3–12, 14–25, 29–34 | 212 | Vivas e fora do reate. **Cartório → retratação pela célula** quando a lavra absolver e o C6 confirmar. **Lista do admin** quando o juiz apontar CADASTRO ou BATIDA. Com o C6 vivo, o S128 sem credor pula a pergunta. A porta que se aplica a cada caso deste grupo está [nao verificado]. `app/chamados/reconciliador.py:747–789,1513–1515,1654–1661` `app/chamados/services/disputa_emissao.py:2171–2181` `app/escala/services/cadastro_realidade.py:821–835` |
| 51, 67, 85, 103 | 8 | **Reate (b)**. A derivação encontrou destino vivo e `resolvedora_valida` aceitou. A porta é `reapontar_resolvedora`. A costura sob trava e o fecho posterior estão [nao verificado]. `app/chamados/reconciliador.py:1711–1714,1821–1826` `app/chamados/services/fio.py:1040–1073` |
| 35, 41–42, 52, 58–59, 69, 75–76, 87, 94–95 | 149 | **Reate (a)**, histórico vazio ou descartado pelo C6, com veto `NUNCA_BATEU` à reemissão. A lista do admin tem a entrada **C5** para `nunca_bateu`, sujeita à janela e à situação do colab. O C5 lê o dono do dia. Presença na lista e dono por linha estão [nao verificado]. `app/chamados/reconciliador.py:1748–1779,1008–1012` `app/ponto/marcos.py:119–163` `app/escala/services/cadastro_realidade.py:165–175,205–244,757–759` |
| 68, 86 | 2 | **Reate (a)** com histórico descartado e veto `FOLGA` à reemissão. A porta de fecho pelo cartório é o evento da lavra, se o marco da própria pergunta estiver comprovado, o dia encerrado e as demais guardas passarem. Esses insumos estão [nao verificado]. `app/chamados/reconciliador.py:1008–1012,1182–1228,1275–1291,1523–1525` `app/ponto/marcos.py:155–163` |
| 40, 48, 57, 64, 73–74, 82, 89, 92–93, 101 | 1.176 | **Reate (a)** com célula `indefinida`. A porta para o veredito é a **lavra do cartório**. Quando o problema é de dado, a rota é a **lista do admin**, com a ação do dono pela L-099. O C11 tem predicado próprio para folga indefinida. Fora das quatro células do BO, o protesto e o dono por linha estão [nao verificado]. `app/chamados/reconciliador.py:1748–1779` `app/ponto/services/cartorio.py:1905–1913` `app/escala/services/cadastro_realidade.py:535–559,579–599,793–798,821–835` |
| 36–39, 53–56, 70–72, 88, 90–91 | 140 | **Reate (a)**. O C6 esvazia o histórico. c1 recusa a viva e c2 pode costurar no chamado da sua chave se estiver vivo, ou reemitir se passar pelas guardas. Para `orfao_14h` e `saida_sem_entrada`, também existe o fecho pela lâmpada do cartório. A sonda deixou essas guardas [nao verificado]. `app/chamados/reconciliador.py:362–402,956–1032,1156–1173,1204–1216,1748–1779` |
| 43–47, 49–50, 60–63, 65–66, 77–78, 80–81, 83–84, 96–100, 102 | 189 | **Reate (a)** com histórico vazio. c1 recusa pelo motivo ou pelo C6. A porta é c2, sujeito à chave, aos protestos e aos demais vetos. O fecho pela lâmpada continua disponível para os dois motivos que o seletor aceita. Resultado dessas portas [nao verificado]. `app/chamados/reconciliador.py:333,362–402,956–1032,1156–1173,1204–1216,1766–1779` |
| 79 | 1 | **Reate (a)** com histórico vazio e sem FK de célula. c1 recusa a viva. O fecho pela lâmpada fica sem prova. c2 consulta a célula por colab e data, então a FK nula da pergunta, sozinha, deixa a elegibilidade ao c2 [nao verificado]. `sonda_leitura:85` `app/chamados/juizes.py:979–995` `app/chamados/reconciliador.py:400–402,956–992,1766–1779` |
| **Total** | **1.895** | **`FIM 1895 1895`**. `sonda_leitura:137` |

Na L-099, o juiz `dono_da_divergencia` decide. **CADASTRO** recebe a ação da assinatura, **BATIDA** recebe a regularização da batida do dia e, para **ESTRUTURA**, a lista devolve **nenhuma porta**. A sonda deixou o resultado desse juiz por linha [nao verificado]. A falta de fecho automático observada, por si só, deixa o dono [nao verificado]. `app/ponto/management/commands/e6_oraculo.py:53–70,98–110` `app/escala/services/cadastro_realidade.py:821–835`

## B3 — As quatro células do colab PK 502

Em cada linha, as quatro auditorias `pergunta_emitida` são **posteriores** ao `resolvido_em` do chamado correspondente. O horário exato do `INSERT` e sua ordem em relação à resolução estão [nao verificado] para cada pergunta, porque a auditoria é gravada depois do `INSERT`. `sonda_leitura:110–136` `app/chamados/models.py:254–261,3330–3347`

| Célula e data | Chamado e `resolvido_em` | `saida_sem_entrada` | `intervalo_saida` | `intervalo_volta` | `orfao_14h` |
|---|---|---|---|---|---|
| 123535 · 01/11 | 36329 · 10:30:07.660433 | 52488 · 10:30:08.348888 | 52574 · 14:30:11.426806 | 52600 · 15:30:12.604666 | 52684 · 18:50:06.563843 |
| 123541 · 07/11 | 37532 · 10:30:13.380408 | 54887 · 10:30:13.624269 | 54965 · 14:30:10.807721 | 54984 · 15:30:10.999976 | 55065 · 18:50:07.243566 |
| 123547 · 13/11 | 38788 · 10:30:11.853324 | 57477 · 10:30:12.101662 | 57580 · 14:30:27.963646 | 57599 · 15:30:19.322599 | 57681 · 18:50:09.607853 |
| 123553 · 19/11 | 39854 · 10:30:14.733435 | 59786 · 10:30:15.144317 | 59891 · 14:30:23.025397 | 59917 · 15:30:18.774954 | 59984 · 18:50:08.329437 |

Horários locais `−03:00`. As colunas de pergunta mostram PK e horário da auditoria. `sonda_leitura:110–133`

- **Reate.** Para as 16, a seleção medida dá **(a), histórico esvaziado pelo C6**. A derivação ficou sem destino vivo e o histórico de encerrados foi descartado porque a pergunta estava viva no alcance do O261(C). No c1, as oito de intervalo são recusadas pelo motivo. As oito de `saida_sem_entrada` e `orfao_14h` são recusadas enquanto o C6 as der por vivas. No c2, o único protesto impresso nas quatro células é `PREVISTO_COM_DIA_INDEFINIDO`, que fica fora de `PROTESTOS_QUE_COBRAM`. Sem um chamado vivo na chave `batida_ausente` do c2, a reemissão cai nessa guarda, se tiver passado pelas anteriores. Com c1 e c2 recusados, o reate devolve `False` na linha 1779. O resultado da consulta exata à chave do c2 e a primeira guarda que barrou cada execução em produção estão [nao verificado]. `sonda_leitura:46,63,79,98,110–133` `app/chamados/reconciliador.py:333,362–402,956–992,1711–1779` `app/ponto/services/furos_sem_cobranca.py:35`
- **S128.** Na fotografia da sonda, as 16 estão sem `chamado_destino`, então ficam fora do carimbo por destino. O fecho sem credor admite FK nula, mas pula pergunta viva pelo C6. A passagem por `_ancoradas()` poderia tentar reabrir o credor se o chamado fosse o pai da disputa e as demais guardas passassem. A sonda não mostrou essa âncora. O `post_save` do encerramento é síncrono e só lê as perguntas que já consegue consultar naquele ato. Quais linhas estavam visíveis naquele instante, se houve outra chamada do S128 e o resultado dessas chamadas estão [nao verificado]. `sonda_leitura:110–133` `app/chamados/signals.py:91–119` `app/chamados/models.py:349–366` `app/chamados/services/disputa_emissao.py:1924–1948,2038–2042,2071–2084,2155–2193`
- **Célula.** As quatro têm lavra do cartório com `PREVISTO_COM_DIA_INDEFINIDO`, que deriva `indefinida`. O suprajuiz pode emitir esse protesto por falta de vínculo que sustente o dia ou por falta de âncora, como folga sem cadastro. Encerrar o chamado não resolve essa premissa de **CADASTRO**. O S128, quando carimba com `lavrar_veredito=True`, grava o veredito da pergunta. O cartório grava o da célula, e `indefinida` ainda conta como veredito que acusa no C6. A causa cadastral exata e a presença no C11 estão [nao verificado]. `sonda_leitura:110,116,122,128` `app/ponto/supra_juiz.py:346–372,464–469` `app/escala/models.py:1622–1642,1780–1782` `app/chamados/models.py:3502–3514` `app/chamados/juizes.py:1551–1555` `app/ponto/services/cartorio.py:1905–1913`

## B4 — Vigia

**38 órfãs reatáveis para o colab PK 502**, pela função real `perguntas_orfas_reatavies(502).count()`. O total da frota e os totais I8/I8o estão [nao verificado]. `sonda_leitura:134–137` `app/chamados/reconciliador.py:1640–1661` `app/chamados/management/commands/reconciliar_chamados.py:57–58,97–98`

O cron publica essa contagem como medida em `logs/reconciliador.log` a cada 15 minutos. A soma que dispara o `ALARME` com RC 2 usa outros contadores. I8/I8o saem em `logs/invariantes.log` e em `/app/media/invariantes_chamados.html`. O silêncio dos logs desde 29/09 está [nao verificado]. `app/config/crons.py:395,542–543,1913–1917` `app/chamados/management/commands/reconciliar_chamados.py:115–124` `app/chamados/management/commands/auditar_invariantes_chamados.py:1866–1889`

## 27/11 11:04 · sessao 8f2663b90aab · ORFAS-VOLTOU · leitura · origem=Code

## A1. Ramo e recusas do reate

**Um chamado `resolvido` na célula fica fora da derivação de destino vivo.** Se `_derivar_chamado_destino` encontrar outro destino vivo elegível, a pergunta segue pelo **(b)**. Se devolver `None`, segue pelo **(a)**. A derivação procura chamados vivos do colaborador da disputa, exclui `disputa_supervisao`, exige que o chamado atenda o motivo, descarta chamado já ocupado por pergunta do mesmo motivo com `materializada_em IS NULL` e compara os dias quando ambos são conhecidos. Havendo vários candidatos, tenta desempatar pela entrada do turno aberto e pelo estágio do chamado. Com zero ou mais de um candidato ao final, devolve `None`. `app/chamados/services/disputa_emissao.py:97–163,191–245` `app/chamados/reconciliador.py:1711–1714,1821–1826`

| Motivo | Histórico no alcance O261(C), com C6 morta | Histórico fora do alcance O261(C) | Histórico vazio |
|---|---|---|---|
| `orfao_14h` | Aceita categoria ou subcategoria reconhecida por `motivos_atendidos`. | O mapa legado precisa devolver `orfao_14h`. | Tenta c1 e, se recusar, c2. |
| `saida_sem_entrada` | Aceita categoria ou subcategoria reconhecida por `motivos_atendidos`. | A categoria precisa mapear para `saida_sem_entrada`, sem uma subcategoria mapeada que prevaleça. | Tenta c1 e, se recusar, c2. |
| `intervalo_saida` | Aceita categoria ou subcategoria reconhecida por `motivos_atendidos`. | Os mapas legados não têm esse valor. | c1 recusa pelo motivo. c2 exige alcance O261(C) e C6 viva. |
| `intervalo_volta` | Aceita categoria ou subcategoria reconhecida por `motivos_atendidos`. | Os mapas legados não têm esse valor. | c1 recusa pelo motivo. c2 exige alcance O261(C) e C6 viva. |

Fontes da tabela. `app/chamados/juizes.py:2331–2342` `app/chamados/models.py:4133–4170` `app/chamados/reconciliador.py:333,362–375,838–880,918–932,1028–1031,1748–1779`

- **(a), seleção do histórico.** Sem data na pergunta e sem data no contexto do motivo, sai `False` na linha 1725. O histórico exige o mesmo colaborador, status `resolvido` ou `fechado`, `contexto_json.data_turno` igual ao dia e categoria diferente de `disputa_supervisao`. Uma FK para a célula, sozinha, não satisfaz essa consulta. No alcance O261(C), C6 viva esvazia o histórico. Dia inválido ou ausente, competência entregue ou dia trancado levam à seleção legada. Com mais de um candidato, sai `False` na linha 1779. Com zero, sai pela mesma linha se c1 e c2 recusarem. `app/chamados/reconciliador.py:857–871,1711–1779`
- **(a), c1.** `lavrar_sem_turno_aberto` recusa motivo fora dos dois permitidos, resposta presente pelo juiz, materialização já preenchida, turno aberto no núcleo ou C6 viva. Também recusa se `carimbar` falhar pelo estado fora do fato, C6 viva após aplicar o carimbo em memória, par ato/coluna inválido `(False, True)` ou retenção ativa. O sucesso do c1 pode deixar a FK nula. `app/chamados/reconciliador.py:333,362–417,1774–1775` `app/chamados/models.py:3460–3519` `app/chamados/services/fio.py:1620–1632`
- **(a), c2.** `reemitir_chamado_do_dia` recusa motivo fora dos quatro, dia ausente ou inválido, colaborador fora de `ativo`, isenção aplicável, ausência que cobre, veto de `celula_veta_cobranca` ou veto de `EMITIR` pelo motor. Sem um `batida_ausente` vivo na chave do c2, também pode recusar por horizonte, ausência de protesto que cobra, `parcial_so_lista` ou veto da régua. Os dois intervalos ainda exigem `_viva_em_competencia_aberta`. Depois disso, sai `False` se `abrir` devolver `None`, se devolver chamado fora de `VIVOS` ou se a costura recusar. `app/chamados/reconciliador.py:874–880,930–1080`
- **(a), recusas dentro de `abrir`.** Um nascimento pode devolver `None` por absolvição pela célula ou veto da régua. Na reutilização de encerrado, `abrir` devolve o objeto mesmo se o renascimento recusar. Esse renascimento verifica situação ativa, via `cascade`, tranca e régua, depois passa por `reabrir`, que também verifica fechamento administrativo sem fato novo, premissa regularizada, agrupamento, irmão vivo e transição permitida. Se o objeto continuar encerrado, o c2 recusa na linha 1062. `app/chamados/models.py:972–998,1025–1037,1068–1090,1403–1426` `app/chamados/reconciliador.py:1055–1062`
- **Costura, nos dois ramos.** `reapontar_resolvedora` recusa destino de outro colaborador. `resolvedora_valida` pode recusar quando o juiz não reconhece o episódio como fato do turno, a ponte A2 alcança mais de um dia ou há divergência comprovada entre o dia do fato e o do chamado. Para dias adjacentes, aceita a travessia de meia-noite confirmada ou indeterminada. A guarda seguinte relê o destino e recusa se ele estiver terminal, a competência estiver comprovadamente aberta e a pergunta permanecer viva com o destino aplicado em memória. Se a costura também estiver datando uma pergunta, há recusas adicionais para conversa, dia do fato inválido e colisão que a porta não pode resolver. Esse último grupo fica fora do universo datado do aval. `app/chamados/services/fio.py:916–947,1017–1073,1098–1133,1911–1924,1946–2005` `app/chamados/juizes.py:2440–2498`
- **Saídas da costura.** Com histórico único, a recusa da costura sai `False` na linha 1804. Se a cerca vetar alguma escrita atingida pela costura ou pelo S128, desfaz o ato e sai `False` na linha 1815. Se a costura passar e o S128 retornar normalmente, o reate pode devolver `True` com zero perguntas carimbadas. No **(b)**, a recusa da mesma costura sai `False` na linha 1825. `app/chamados/reconciliador.py:1800–1826` `app/chamados/services/fio.py:1470–1480,1536–1561,1564–1592`

**Por que pode nascer solta.** A emissão pode gravar a pergunta sem destino quando a derivação devolve `None`. A tentativa imediata de reate exige pergunta recém-criada, FK ainda nula e motivo do grupo do dia. A fábrica deduplica pelo fato já representado, inclusive se a linha estiver solta. O estoque elegível volta ao reate pelo laço do colaborador, acionado por batida ou pela saída de `VIVOS` de um filho listado por pai vivo. Para as 16 perguntas do aval, a condição de nascimento, o ramo efetivamente executado e a recusa concreta estão [nao verificado]. `app/chamados/services/disputa_emissao.py:402–478` `app/chamados/services/fabrica_da_celula.py:138–157` `app/chamados/reconciliador.py:1674–1685` `app/ponto/signals.py:211–223` `app/chamados/signals.py:351–371`

## A2. O que o S128 alcança

**O S128 pode alcançar uma pergunta com FK nula pelo fecho sem credor.** O filtro por FK vale para o primeiro laço. Há três leituras relevantes em `resolver_perguntas_por_chamado_resolvido`. `app/chamados/services/disputa_emissao.py:2038–2042,2071–2084,2155–2193`

1. **Carimbo pelo destino.** O ORM exige exatamente `chamado_destino=chamado`, `materializada_em__isnull=True` e `disputa__fechada_em__isnull=True`. O laço pula pergunta em voo, viva pelo C6, de dia trancado ou respondida pelo juiz sem materialização de fato. As restantes só contam se `carimbar(...)` aceitar. Não há filtro inicial por data de criação nem por `validada_em`. Se o chamado tiver via `competencia_trancada`, o S128 retorna zero antes dos laços. `app/chamados/services/disputa_emissao.py:2030–2042,2087–2122`
2. **Âncora para reabertura.** `_ancoradas()` pode incluir FK nula se `disputa.chamado_id == chamado.pk` e a pergunta estiver sem validação pelo ato. Esse laço tenta devolver um credor à pergunta viva sem resposta. `app/chamados/models.py:349–366` `app/chamados/services/disputa_emissao.py:1924–1948,2071–2084`
3. **Fecho sem credor.** A varredura inclui todas as disputas abertas do colaborador do chamado. Pula a disputa se o pai estiver em `VIVOS` ou se qualquer pergunta tiver destino em `VIVOS`. Entre as restantes, examina perguntas com `materializada_em is None` e `DecisaoPergunta.composta(q).validada_em is None`, inclusive com FK nula. Pula as em voo e as vivas pelo C6, depois tenta `carimbar`. A disputa só fecha se não houver pergunta em voo e `pode_fechar()` aceitar. `app/chamados/services/disputa_emissao.py:2155–2193,2213–2226` `app/chamados/models.py:2047–2053`

**Gatilho principal.** `resolve_perguntas_ao_resolver` é `post_save` síncrono de `ChamadoColaborador`, com savepoint próprio. Exige status resultante em `ENCERRADOS` e save que escreveu status. Isso inclui criação, `update_fields` contendo `status_local` mesmo com valor igual, ou save integral em que o valor difere do `pre_save`. Um `QuerySet.update()` não emite esse sinal. Também há chamadas diretas na retratação e no reate histórico, no fecho de beco acionado após commit de `PerguntaDisputa`, e nos caminhos de aplicação manual de `fechar_becos_disputa` e `reconciliar_fantasmas`. Os agendamentos desses dois comandos usam `--alarme`. `app/chamados/signals.py:76–119,180–186,229–240` `app/chamados/reconciliador.py:602–616,1800–1807` `app/chamados/services/becos.py:38–61,80–94` `app/chamados/management/commands/fechar_becos_disputa.py:22–30,52–57` `app/ponto/management/commands/reconciliar_fantasmas.py:144–153,264–288` `app/config/crons.py:392,663`

Uma pergunta criada depois do save de encerramento estava ausente daquela execução. Uma pergunta com FK nula fica fora do laço por destino enquanto permanecer assim, mesmo se já existia no fechamento. O fecho sem credor ainda pode alcançá-la em outra chamada, sujeito aos predicados acima. Para as quatro células, a ordem dos eventos e o motivo específico de o S128 não ter carimbado as perguntas estão [nao verificado]. `app/chamados/signals.py:91–119` `app/chamados/services/disputa_emissao.py:2038–2042,2155–2193`

## A3. Por que a célula pode continuar `indefinida`

O escritor operacional do par é `ponto.portas.celula.lavrar_veredito`. `CelulaDia.lavrar_veredito` delega a essa porta. O cartório e a decisão do admin chegam por ela, com precedência para o admin. A mesma porta de célula também contém a operação que apaga a lavra do cartório ao regenerar uma célula futura. O `lavrar_veredito=True` passado por S128 a `PerguntaDisputa.carimbar` grava o veredito **da pergunta**. `app/escala/models.py:1832–1841` `app/ponto/portas/celula.py:1007–1024,1069–1142,1145–1150,1161–1204` `app/ponto/services/veredito_celula.py:357` `app/chamados/models.py:3502–3514`

A lavra da célula pode ocorrer com perguntas abertas ou sem lavra própria. O cartório classifica os insumos do dia, entre eles a quantidade de cobranças vivas com fio, e passa os códigos à lavra. `indefinida` vem de `PREVISTO_COM_DIA_INDEFINIDO`, `FASE_NAO_DECLARADA_COM_PREVISTO` ou `PARCIAL_SEM_PERIODO` quando nenhum código de maior precedência vence. Resolver o chamado pode mudar o insumo de cobrança e ainda deixar presente o motivo de cadastro ou de cobertura que produz `indefinida`. A própria `indefinida` integra `VEREDITOS_ACUSAM` e pode manter a pergunta viva quando o C6 chega à leitura da célula. `app/ponto/services/cartorio.py:1418–1423,1498–1516,1905–1913` `app/escala/models.py:1622–1643,1780–1782` `app/ponto/supra_juiz.py:346–372,392–402,450–469` `app/chamados/juizes.py:1499–1518,1551–1567`

**O gatilho de rejulgamento existe.** Na criação ou mudança de status, `_cartorio_post_save_chamado` tenta `julgar_celula` quando consegue o dia por `dia_do_fato(..., fortes=True)` e o cartório não está em julgamento. O lote `processar_cartorio` também chama o juiz para células passadas. Uma impressão igual pode pular a lavra. A impressão inclui PK e status dos chamados que o cartório lê, recortados pelos módulos de furo e pela chave de data do catálogo. Qual gatilho faltou nas quatro células, ou se houve rejulgamento que manteve o mesmo veredito, está [nao verificado]. `app/chamados/signals.py:180–218` `app/ponto/services/cartorio.py:131–157,490–503,1432–1448,2078–2115` `app/ponto/management/commands/processar_cartorio.py:45–65` `app/config/crons.py:616`

## A4. O que os vigias publicam

| Vigia | Universo e destino |
|---|---|
| `reconciliar_chamados --alarme` | A linha **`orfas reataveis`** soma `perguntas_orfas_reatavies(c).count()` pelos colaboradores do tenant. O seletor exige destino e materialização nulos, disputa aberta e um dos quatro motivos. Inclui colaboradores com evento pendente e dispensa C6, corte de data, via vazia e validação nula. Sai em stdout a cada 15 minutos em `logs/reconciliador.log`. O estoque fica fora da soma de alarme. Quando os outros contadores da soma disparam, o comando escreve em stderr e sai com RC 2. `app/chamados/reconciliador.py:1637–1661` `app/chamados/management/commands/reconciliar_chamados.py:57–62,89–124` `app/config/crons.py:395,1913–1917` |
| `auditar_invariantes_chamados --html` | **I8/I8o** partem de FK nula, colaborador ativo, motivo na união de `MOTIVOS_QUE_PRECISAM_CHAMADO_DESTINO_SET` com os quatro do dia, e C6 viva. A consulta base não exige disputa aberta, data mínima, via vazia, validação ou materialização nulas. `_piso_i8_sem_entrada_no_contexto` põe em I8o quem tem dois ou mais candidatos e nenhum com `entrada_timestamp`. Os candidatos casam por `contexto_json.data_turno` ou `celula.data`, mesmo colaborador e motivo atendido, com exclusão de `disputa_supervisao` e sem filtro de status. O restante é I8. O cron das 07:18 publica em stdout para `logs/invariantes.log` e escreve `/app/media/invariantes_chamados.html`. I8o tem o tamanho do piso na descrição e `0` na coluna de violações. A contraprova pode acrescentar texto à descrição. RC 2 depende de F1/F2, sem contribuição de I8 ou I8o. `app/chamados/models.py:4126,4169–4170` `app/chamados/management/commands/auditar_invariantes_chamados.py:51–110,818–831,860–872,1866–1889` `app/config/crons.py:542–543,1913–1917` |

`cron_run.sh` redireciona a execução pelo cron e grava duração e RC em `app/logs/placar.jsonl`. `pipeline_placar.medidos` lê esse RC e contabiliza o `2` como alarme de negócio. A pauta para TI do próprio runner trata RC diferente de `0` e `2`. Leitor automático das linhas de estoque ou de I8/I8o, leitor humano efetivo, silêncio dos logs desde 29/09 e números atuais dos vigias estão [nao verificado]. `app/config/crons.py:747,1515–1518,1913–1917` `bin/cron_run.sh:2–13,19–38` `app/core/management/commands/pipeline_placar.py:61–82,110–137`

## B. Sonda de leitura

O aval informa **1.890 perguntas, 183 colaboradores e 657 células**, com quatro perguntas soltas por célula nos quatro casos. As contagens por classe e os dados individuais ainda estão [nao verificado]. `aval_v2.txt:1`

A sonda cruza os dois seletores reais, o C6, o ramo que a seleção atual indica, o estado dos chamados do dia e o veredito da célula. Para mostrar chamados do dia, usa `dia_do_fato(..., fortes=True)`. Para diagnosticar o histórico do reate, usa a chave `contexto_json.data_turno` exigida por aquele ramo. `juiz_aceitou` mede apenas `resolvedora_valida`. A execução da costura, do c1, do c2 e da cerca fica [nao verificado]. `app/chamados/juizes.py:89–130,2414–2498` `app/chamados/reconciliador.py:1166–1176,1640–1661,1711–1826`

`PerguntaDisputa` não tem `criada_em`. A sonda lê apenas PK e instante da auditoria `pergunta_emitida`, gravada depois da inserção e em regime best-effort. `emissao_antes` e `emissao_depois` comparam esse registro com `resolvido_em`. Um registro posterior deixa o instante exato da inserção [nao verificado]. `ChamadoColaborador` usa `resolvido_em`, preservado na passagem de `resolvido` para `fechado`. Quando disponível, a sonda mostra também o carimbo da transição auditada para `fechado`. `app/chamados/migrations/0014_perguntadisputa.py:16–29` `app/chamados/models.py:62–80,254–261,304–308,3330–3348` `app/core/models.py:295–307` `app/core/observ.py:94–105`

Em B4, os totais de frota ficam dentro dos respectivos `handle`. A sonda imprime essa indisponibilidade e chama o seletor real para o número no recorte `--colab` das células-alvo. O Python usa transação `REPEATABLE READ, READ ONLY`. O envelope já seleciona `juliani` e tem escritas administrativas próprias, incluindo credencial, permissões quando aplicáveis, arquivo temporário e trilha. A sintaxe Python foi conferida em memória. A execução com o ORM da aplicação e em produção está [nao verificado]. `app/chamados/management/commands/reconciliar_chamados.py:57–58,97–98` `app/chamados/management/commands/auditar_invariantes_chamados.py:860–872` `app/chamados/tests/cef_c3_sonda.py:16–22` `bin/sonda_leitura.sh:115–143,153–181,216–240,245–251`

```python
from collections import Counter, defaultdict
from datetime import date
import logging

from django.db import connection, transaction
from django.db.models import BooleanField, Case, Exists, OuterRef, Q, Value, When
from django.utils import timezone

from chamados.catalogo.motor import ENCERRADOS, VIVOS
from chamados.juizes import dia_do_fato, motivos_atendidos, pergunta_viva, resolvedora_valida
from chamados.models import (
    MAPA_CATEGORIA_PARA_MOTIVO_DISPUTA,
    MAPA_SUBCATEGORIA_PARA_MOTIVO_DISPUTA,
    ChamadoColaborador,
    PerguntaDisputa,
)
from chamados.reconciliador import (
    _dia_fora_do_alcance_c,
    _orfas_abertas,
    perguntas_orfas_reatavies,
)
from chamados.services.disputa_emissao import _derivar_chamado_destino
from core.models import LogAuditoria
from core.observ import engolidos
from escala.models import CelulaDia
from ponto.marcos import celula_veta_cobranca

INICIO = date(2026, 10, 21)
CASOS = (123535, 123541, 123547, 123553)
AVAL = (1890, 183, 657)
NV = "[nao verificado]"


def local(instante):
    return timezone.localtime(instante).isoformat() if instante is not None else None


def cronologia(emissao, chamado):
    if chamado.resolvido_em is None:
        return "sem_resolvido_em"
    if emissao is None:
        return "sem_trilha_de_emissao"
    if emissao < chamado.resolvido_em:
        return "emissao_antes"
    if emissao > chamado.resolvido_em:
        return "emissao_depois"
    return "mesmo_instante"


def ramo_de_hoje(p, viva, historicos):
    # Seleção inline de reatar_pergunta_orfa para data_evento preenchida.
    # app/chamados/reconciliador.py:1711–1779,1821–1826
    if not p.no_reate:
        return "fora_do_laco", None
    colab = p.disputa.colaborador
    vivo, _ = _derivar_chamado_destino(p.disputa, p.motivo, data_pergunta=p.data_evento)
    if vivo is not None:
        ok, _ = resolvedora_valida(vivo, p)
        return "b_derivou_vivo", "juiz_aceitou" if ok else "juiz_recusou"

    encerrados = historicos.get((colab.pk, p.data_evento.isoformat()), ())
    if encerrados and not _dia_fora_do_alcance_c(p, colab, p.data_evento):
        if viva:
            veto = celula_veta_cobranca(colab.pk, p.data_evento)
            return "a_historico_esvaziado_pelo_C6", veto
        candidatos = [
            c for c in encerrados
            if p.motivo in motivos_atendidos(c.categoria, c.subcategoria)
        ]
    else:
        candidatos = []
        for c in encerrados:
            sub = (c.subcategoria or "").lower().strip()
            cat = (c.categoria or "").lower().strip()
            motivo = (
                MAPA_SUBCATEGORIA_PARA_MOTIVO_DISPUTA.get(sub)
                or MAPA_CATEGORIA_PARA_MOTIVO_DISPUTA.get(cat)
            )
            if motivo == p.motivo:
                candidatos.append(c)

    if len(candidatos) > 1:
        return "a_historico_multiplo", None
    if len(candidatos) == 1:
        ok, _ = resolvedora_valida(candidatos[0], p)
        return "a_historico_unico", "juiz_aceitou" if ok else "juiz_recusou"
    return "a_historico_vazio", celula_veta_cobranca(colab.pk, p.data_evento)


def estado_do_dia(chamados):
    estados = set()
    for chamado in chamados:
        if chamado.status_local in VIVOS:
            estados.add("vivo")
        elif chamado.status_local in ENCERRADOS:
            estados.add("resolvido_fechado")
        else:
            estados.add("outro_status")
    return "+".join(sorted(estados)) or "sem_chamado"


def sondar():
    filtro = Q(
        data_evento__gte=INICIO,
        chamado_destino__isnull=True,
        via_resolucao="",
        validada_em__isnull=True,
        materializada_em__isnull=True,
    )
    perguntas = list(
        PerguntaDisputa.objects.filter(filtro 

[truncado em 20000 de 28111 caracteres]

## 27/11 10:55 · sessao 77187f8251ee · RC-SEM-PROXIMA · verificador · origem=Code

## 1. P1–P4 na origem

| Achado | Cura e mordida |
|---|---|
| **P1 — parcial.** | `main` lê `agora` uma vez, materializa `vivas` uma vez e calcula as duas listas antes de imprimir. Na mesma passagem, a chave que exclui um `.rc` de `SEM COLHER` é a que permite sua entrada em `SEM PROXIMA`. Em memória, a c1 falhou nos três cenários e a c2 passou. `dois-walks` voltou a listar a mesma obra duas vezes, `dois-walks-ilegivel` voltou a dar rc `1`, e `dois-relogios` voltou a listar cedo. `caso_misto` confirma uma linha de cada classe para obras distintas. `bin/rc_sem_colher.py:94–106,121–138,157–169` · `bin/tests/test_rc_sem_colher.sh:165–180,287–289` |
| **P2 — corrigido na proposta, sem selo do leitor.** | O diff chama o juiz com `--min-idade 900`, conta cada prefixo separadamente e mostra diagnóstico para rc fora de `0`/`3`. Em memória, uma saída só de `SEM PROXIMA` passou de uma falsa contagem de órfãos para `0` órfãos e `1` parada. O teto global de 15 linhas pode ocultar todos os detalhes de `SEM PROXIMA` e de `OUTRAS LINHAS`, embora as contagens apareçam. `caso_min_idade` e `sem-min-idade` exercitam a CLI do juiz. Restaurar a contagem antiga ou remover o argumento **só do vercodex** deixa o selo intacto. `vercodex.py:165–176` (anexo `vercodex.diff`) · `bin/tests/test_rc_sem_colher.sh:58,202,243–248,271` |
| **P3 — corrigido para o minuto inferido.** | O juiz soma `60` s ao início do minuto e exige **mais de** `900` s desse limite. Em memória, `899/900/901` deram rc `0/0/3` na c2 e `3/3/3` na c1. `prazo-do-inicio-do-minuto` voltou a listar em `899` e `900`. A data inferida ainda pode estar errada. `bin/rc_sem_colher.py:37–38,110–118,135–138` · `bin/tests/test_rc_sem_colher.sh:182–186,286` |
| **P4 — corrigido no caso da próxima etapa.** | `caso_proxima_espera` exige rc `0` do Bash, linha `SEM PROXIMA` e ausência da testemunha de entrada no corpo do laço. Na sonda do Bash com árvore simulada, `proxima-desligada-espera` preservou a linha, mas entrou no laço. `proxima-vazia-espera` entrou no laço e perdeu a linha. A medição deixou de usar `date +%s`, embora a montagem da fixture ainda use horas relativas. O comportamento positivo já passa com o juiz da c1. A mordida específica vem dos mutantes. `bin/tests/test_rc_sem_colher.sh:60–76,107–113,221–222,290–291` · `c1:bin/tests/test_rc_sem_colher.sh:104–108` |

**Regressão de P1 reproduzida em memória.** Ao começar a varrer `OBRA-B`, removi o `r.rc` já visitado de `OBRA-A`. A c1 já tinha lido seu conteúdo e retornou rc `3`. A c2 guardou apenas caminho e mtime, abriu o caminho depois de varrer `OBRA-B` e terminou com `FileNotFoundError`, rc `1` e stdout vazio. Na variante com substituição do arquivo, a c2 imprimiu `10:00 rc=7`, juntando o mtime do arquivo antigo ao conteúdo do novo. A publicação por `mv` existe no produtor. Os dois casos de walk do selo disparam somente numa **segunda visita** à mesma obra, por isso deixam passar a troca durante a primeira passagem. `c1:bin/rc_sem_colher.py:89–102,153–163` · `bin/rc_sem_colher.py:57–61,77–91,99–106,160–169` · `bin/codex_pergunta.sh:572–574` · `bin/tests/test_rc_sem_colher.sh:151–171`

Nenhum dos seis mutantes novos passou nas sondas em memória. `caso_proxima_vazia` sozinho passa com a funcionalidade desligada, como controle negativo. `caso_misto` também passa na c1 com árvore estável. RED anterior à cura, GREEN integral, `test_disjuntor.sh` e instalação do `vercodex.diff` ficam [nao verificado]. `bin/tests/test_rc_sem_colher.sh:87–90,176–180,243–260,286–291`

## 2. Quem escreve e quem lê

| Valor | Escritor e leitores |
|---|---|
| **`agora`** | `main` captura `time.time()`. `_obras_vivas` usa o valor para o corte de 24 h, `sem_colher` para a idade mínima e `sem_proxima` para o prazo da marca. `bin/rc_sem_colher.py:68–85,94–100,121–136,160–163` |
| **`vivas`, mtime e conteúdo do `.rc`** | O wrapper publica o `.rc` com `rc=` e `fim=`. `_obras_vivas` lê o mtime, escolhe o caminho e registra o maior mtime de outro arquivo. `main` materializa as tuplas. Ambas as listas leem as tuplas e `sem_colher` abre o `.rc` para extrair `rc=`. `bin/codex_pergunta.sh:565–574` · `bin/rc_sem_colher.py:57–61,68–91,99–106,126–131,160–163` |
| **Marcas do RELATO** | A sessão colhe e registra `colhido` ou `superado` no formato documentado. O comando exato de escrita fica [nao verificado]. `main` lê o texto uma vez. `marcas_datadas` extrai os campos, `sem_colher` consulta a existência da chave e `sem_proxima` consulta tipo e hora da linha. `CLAUDE.md:332–337` · `app/docs/RELATO.md:13639–13646` · `bin/rc_sem_colher.py:34–54,97–105,124–136,157–159` |
| **rc `3` e as duas linhas** | `main` escreve os dois formatos em stdout e retorna `3` se houver alguma linha. `ESPERA_UNICA` deixa passar stdout, lê `$?`, pula o laço com `3` e fecha o Bash com `0` para rc `0` ou `3` do juiz. O `find` final ainda pode imprimir caminhos. O rc da linha `rc=<n>` vem do `.rc`. O vercodex captura stdout e código de saída. `bin/rc_sem_colher.py:32,57–61,164–173` · `bin/disjuntor.py:1963–1969` · `vercodex.py:165–176` (anexo) |

- **`disjuntor.py` e PreToolUse.** A espera chama a CLI, cujas opções continuam aceitas. `espera_nega` lê o texto do Bash e confere `find` e teto. Durante o laço, a espera observa ctime de `.rc`; uma marca atingir 15 minutos enquanto o laço roda não acorda a espera por si só. `bin/rc_sem_colher.py:142–156` · `bin/disjuntor.py:1967–1969,2157–2174,2217–2228`
- **`logs/espera_unica.sh`.** O RELATO cita o script e `logs/` é ignorado. Conteúdo, leitores e compatibilidade do script ficam [nao verificado]. `app/docs/RELATO.md:13598` · `.gitignore:53–54`
- **`vercodex` e `hook_stop`.** O vercodex antigo soma as duas classes sob `SEM COLHER`; o diff proposto corrige a contagem. O Stop lê o topo do RELATO para `PAREI`/`ESPERANDO`, o rc do checador de prova e as decisões B1/B3/B4 do disjuntor. Esses caminhos não consomem as listas nem o rc deste instrumento. Os chamadores Python encontrados das duas assinaturas alteradas estão no próprio `main`. `/home/ronald/vercodex.py:163–171` (anexo `ver1_r.md`) · `vercodex.py:165–176` (anexo `vercodex.diff`) · `bin/hook_stop_fila1.py:462–481,783–831,1305–1329,1434–1446,1489–1501` · `bin/rc_sem_colher.py:94,121,162–163`

## 3. Esmeril de semântica e classes

- **Relógio e unidades.** `agora` é um instante em segundos Unix lido na borda da chamada. A janela admite mtime exatamente no corte de 24 h. `--min-idade 900` só admite órfãos com idade **maior que** `900` s. A docstring sustenta o argumento; a frase “menos de 15 min” do RELATO contradiz o predicado. A espera usa ctime contra o arquivo de referência para detectar publicação por `mv`. `bin/rc_sem_colher.py:3–4,68–85,94–103,160–163` · `app/docs/RELATO.md:13640–13643` · `bin/disjuntor.py:1963–1969`
- **Dois `HH:MM`.** O de dentro dos parênteses identifica o minuto local do mtime do `.rc`. O prefixo informa o minuto local da colheita. A gramática distingue os campos, mas o prefixo não traz dia, offset ou segundos. `_instante_da_marca` infere o dia a partir do minuto do `.rc`, arredonda o piso sem perder o mesmo minuto por fração e soma `86400` para a virada. Meia-noite comum funciona. Em sondas com mudança de offset, a inferência deslocou a hora válida em uma hora. `bin/rc_sem_colher.py:34–48,99–104,110–118,130–136` · `bin/tests/test_rc_sem_colher.sh:91–105`
- **Juiz e derivação paralela.** O vercodex lê a classificação emitida e conta linhas. A regra de elegibilidade continua no instrumento. A dupla observação de relógio e árvore da c1 saiu. Ainda há duas chamadas ao mesmo parser puro sobre o mesmo texto e duas cópias do predicado de folga. Isso deixa duplicação interna, com o risco de divergência demonstrado acima concentrado na leitura tardia do conteúdo do `.rc`. `CLAUDE.md:17–22,35–36` · `app/core/juizes.py:3–14` · `bin/rc_sem_colher.py:2–4,41–54,97–106,124–136,157–163` · `vercodex.py:165–174` (anexo)
- **`except` adicionado, contando `^\+.*except`.** `diff.patch`: **0**. `vercodex.diff`: **0**. O `except Exception` do leitor é contexto do hunk. `bin/rc_sem_colher.py:94–169` · `bin/tests/test_rc_sem_colher.sh:107–186,243–296` · `vercodex.py:175–178` (anexo)

## 4. Descartados da c1

- **Mesmo caminho e minuto, chave sem dia, `touch` e marca sem prefixo** já existiam na c1. A chave pode reaproveitar a marca de outro `.rc`, um `touch` elegível mais de 5 s depois conta como escrita e uma marca sem prefixo fica fora da gramática. Eu manteria esses critérios fora do bloqueio desta volta, salvo a garantia de prazo abaixo. `c1:bin/rc_sem_colher.py:30–49,72–85,94–101,122–132` · `CLAUDE.md:334–337`
- **Hora anterior ao `.rc` e DST** já eram assim na c1. Eu bloquearia a garantia incondicional de 15 minutos reais. Em ambas as versões, um `.rc` de ontem às `23:50:59,9` e uma marca de hoje às `23:50:10` podem listar às `23:50:20`, com **10 s reais** de espera. O `.rc` ainda cabe na janela de 24 h e a data inferida para a marca é ontem. A c2 mudou a margem em segundos, preservando a ambiguidade de data. `c1:bin/rc_sem_colher.py:105–113,125–134` · `bin/rc_sem_colher.py:13–17,68–85,110–118,130–138`
- **`TMPDIR` e `caso_espera` com `date +%s`** também preexistem. `ESPERA_UNICA` chama `mktemp` sem fixar `/tmp`. Se a referência for parar dentro de uma obra elegível, pode entrar na conta de outro arquivo. A configuração efetiva fica [nao verificado]. A medição antiga permanece em `caso_espera` de órfão, fora do caso P4 corrigido. Eu não bloquearia a c2 por esses dois itens. `bin/disjuntor.py:1967–1969` · `bin/rc_sem_colher.py:77–91,99–101,126–128` · `c1:bin/tests/test_rc_sem_colher.sh:134–142` · `bin/tests/test_rc_sem_colher.sh:107–113,212–222`

## Veredito: REPROVADO

1. **P1 introduz uma regressão de leitura.** A c2 pode combinar mtime antigo com conteúdo novo ou perder toda a resposta por remoção do `.rc` durante a primeira passagem. O selo atual não exercita essa janela. `bin/rc_sem_colher.py:57–61,77–91,99–106,160–169` · `bin/tests/test_rc_sem_colher.sh:151–171`
2. **P2 tem correção proposta sem RED no leitor.** O selo fica verde se a contagem antiga ou a chamada sem `--min-idade 900` voltar no vercodex. `CLAUDE.md:28–29` · `bin/tests/test_rc_sem_colher.sh:202,243–248,271` · `vercodex.py:165–174` (anexo)
3. **A promessa de 15 minutos reais excede o que a marca prova.** A borda `899/900/901` morde para a data inferida. A colisão de dia ainda permite listar após 10 segundos reais. `bin/rc_sem_colher.py:13–17,110–118,130–138` · `bin/tests/test_rc_sem_colher.sh:182–186,286`

## 27/11 10:55 · sessao 6d8bb13e8944 · E4C-F5-INSTANTE · construtor · origem=Code

Implementei a cura3c na cópia. A deduplicação do veto K6 agora usa a autoria resolvida pelo próprio escritor da trilha. [observ.py:113](/home/ronald/wt-codex-e4c-f5-instante-012038/app/core/observ.py:113) · [materializacao.py:858](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/materializacao.py:858)

- `core/observ.py::evento` ganhou `unica=False`. Quando ativado, retorna a linha existente com a mesma ação, modelo, objeto, descrição, fotos, usuário e ator, quando o campo está disponível. A consulta e a criação usam o mesmo `engolir` e o mesmo savepoint. A docstring atribui a serialização ao chamador, pela trava do dono. [observ.py:94](/home/ronald/wt-codex-e4c-f5-instante-012038/app/core/observ.py:94) · [observ.py:127](/home/ronald/wt-codex-e4c-f5-instante-012038/app/core/observ.py:127) · [observ.py:138](/home/ronald/wt-codex-e4c-f5-instante-012038/app/core/observ.py:138)
- `chamados/services/materializacao.py::trilha_do_veto_k6` monta a foto e chama `evento(..., unica=True)`. A busca na cópia confirmou que esse é o único chamador de `evento` que passa `unica`. [materializacao.py:846](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/materializacao.py:846)
- `chamados/tests/test_e4c_f5_c4_red.py::Item13PorPKTest.test_RED_item13_veto_repetido_fora_de_request_deduplica_por_usuario_e_ator` usa relógio congelado e autor explícito. Exige uma linha para o comando que já havia vetado e uma única linha para o comando que veta duas vezes. Pela leitura do código, a cura3b produziria zero linhas para o segundo comando, e cada mutante produziria duas. A execução está **[nao verificado]**. [test_e4c_f5_c4_red.py:983](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:983) · [test_e4c_f5_c4_red.py:1068](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:1068)
- `chamados/tests/e4c_f5_c4red2_mutantes.py::veto_sem_deduplicacao` faz o M44 trocar `unica=True` por `unica=False` na trilha. `evento_ignora_unica` faz o M46 pular a consulta no escritor. Ambos apontam para a mesma asserção do RED novo. [e4c_f5_c4red2_mutantes.py:757](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:757) · [e4c_f5_c4red2_mutantes.py:768](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:768)

O diff inteiro da cópia contra `0ba8b22b7b486543caaced318666c264a03b1e81` teve **0** linhas em `^\+.*except`. A sintaxe dos quatro arquivos e o ponto único de aplicação de cada mutante foram verificados estaticamente. A suite e as mordidas ficam **[nao verificado]**.

**Comandos para o Code**

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4c_f5_c4_red.Item13PorPKTest.test_RED_item13_veto_repetido_fora_de_request_deduplica_por_usuario_e_ator
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.e4c_f5_c4red2_mutantes.M44VetoSemDeduplicacao.test_mordida
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.e4c_f5_c4red2_mutantes.M46EventoIgnoraUnica.test_mordida
```

## 27/11 10:51 · sessao 2f6e479709bf · E4C-F5-INSTANTE · construtor · origem=Code

Implementei a cura3b na cópia. Após o rollback da validação, `validar_pergunta` abre um `atomic` próprio, toma a trava existente do dono, relê a pergunta e registra o veto com a foto restaurada. A execução dos testes e a mordida dos mutantes ficam **[nao verificado]**. [validacao.py:834](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/validacao.py:834) [nao verificado]

- **Trava.** O rollback remove o registro adquirido no savepoint desfeito. O novo bloco usa `PerguntaDisputa.trava_na_ordem` só com o dono, como já faz `validar_lote_classe_a` antes da releitura. A trilha consulta e grava `LogAuditoria`, sem adquirir trava de Empresa, preservando a ordem Empresa → dono. [trava_do_colaborador.py:16](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/trava_do_colaborador.py:16) · [validacao.py:1171](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/validacao.py:1171) · [materializacao.py:871](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/materializacao.py:871) · [observ.py:133](/home/ronald/wt-codex-e4c-f5-instante-012038/app/core/observ.py:133)
- **Deduplicação.** `trilha_do_veto_k6` procura uma linha com a mesma pergunta, autoria, ação, motivo e fotos completas. A autoria usa o usuário efetivo quando presente e a identificação de sistema registrada nos demais casos. A criação segue por `core.observ.evento`. A validação inicial, o laço de materialização e a entrada do passivo já tomam a trava do dono antes de chegar à trilha. [materializacao.py:846](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/materializacao.py:846) · [observ.py:108](/home/ronald/wt-codex-e4c-f5-instante-012038/app/core/observ.py:108) · [validacao.py:755](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/validacao.py:755) · [validacao.py:795](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/validacao.py:795) · [materializacao.py:2317](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/materializacao.py:2317) · [passivo_validado.py:393](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/passivo_validado.py:393) · [passivo_validado.py:585](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/passivo_validado.py:585)
- **RED e mutantes.** M44 desliga a deduplicação, de modo que o RED existente deve voltar à contagem de duas linhas. O RED pós-rollback exige uma única linha com as fotos restauradas e observa o registro real da trava nas duas recusas finais. M45 retira a aquisição do bloco final e deve falhar nessa última asserção em uma thread. A fixture usa o auxiliar existente para remover os registros das travas da preparação. Contenção entre transações **[nao verificado]**. [test_e4c_f5_c4_red.py:1055](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:1055) · [test_e4c_f5_c4_red.py:1068](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:1068) · [test_f5_v3_guarda_sob_trava.py:28](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_f5_v3_guarda_sob_trava.py:28) · [e4c_f5_c4red2_mutantes.py:756](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:756)
- **Isolamento e M16.** Cada porta do teste do item 13 agora roda em um savepoint revertido, com as asserções originais. M16 continua apontado para a asserção que exige fotos completas nas duas metades de cada porta. O dublê sem banco já substitui `trilha_do_veto_k6` inteira e dispensa ajuste. [test_e4c_f5_c4_red.py:1151](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:1151) · [test_e4c_f5_c4_red.py:1024](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:1024) · [e4c_f5_c4red2_mutantes.py:253](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:253) · [e4c_f5_c4red2_mutantes.py:474](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:474) · [test_bug_par_sem_banco.py:153](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_bug_par_sem_banco.py:153) [nao verificado]
- **Multiempresa — CONTROLE por leitura estática.** A Empresa B é travada depois do primeiro dono e antes do próprio dono. A porta já passa todas as Empresas do lote à guarda, que deve levantar `EntregaNaoTravada` para B. [test_e4c_f5_c4_red.py:932](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:932) · [models.py:4867](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/models.py:4867) · [export.py:613](/home/ronald/wt-codex-e4c-f5-instante-012038/app/folha/export.py:613)

A checagem estática confirmou que os quatro arquivos compilam, que M44 e M45 têm um ponto de aplicação cada e que o patch adiciona **0** linhas em `^\+.*except`. As asserções existentes do item 13 e o RED do item 6 foram preservados. [validacao.py:841](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/validacao.py:841) · [materializacao.py:846](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/materializacao.py:846) · [test_e4c_f5_c4_red.py:634](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:634) · [e4c_f5_c4red2_mutantes.py:756](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:756)

**Comandos para o Code**

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4c_f5_c4_red.Item13PorPKTest.test_RED_item13_veto_repetido_deixa_uma_linha_de_trilha
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4c_f5_c4_red.Item13PorPKTest.test_RED_item13_veto_pos_rollback_repetido_audita_uma_vez_sob_trava
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4c_f5_c4_red.Item13PorPKTest.test_RED_item13_materializacao_e_passivo_preservam_e_auditam_por_pk
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4c_f5_c4_red.EstoqueConcorrenteComEntregaTest.test_porta_do_estoque_exige_todas_as_empresas_antes_do_primeiro_dono
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_bug_par_sem_banco.BugParSemBancoTest.test_unidade_entregue_sai_antes_de_recusa_e_carimbo
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.e4c_f5_c4red2_mutantes.M16TrilhaVazia.test_mordida
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.e4c_f5_c4red2_mutantes.M44VetoSemDeduplicacao.test_mordida
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.e4c_f5_c4red2_mutantes.M45TrilhaPosRollbackSemTrava.test_mordida
```

## 27/11 10:50 · sessao d0ab90c9cfc0 · E4C-F5-INSTANTE · sonda · origem=Code

**1. Casos (i): zero no recorte da sonda.** A saída é consistente com o script. Os subtotais por mês fecham com os totais por empresa, e `FIM 701 0` indica 701 candidatas, todas da classe (ii). A incidência fora do recorte ou após a coleta fica **[nao verificado]**. (`r1.md:90–96`, saída de `sonda.py:3–14`)

| Empresa | Total (i) | Casos (i) por mês de `data_evento` | Fonte |
|---|---:|---|---|
| 2 | 0 | 2026-08: 0 · 2026-09: 0 · 2026-10: 0 · 2026-11: 0 | Saída de `sonda.py:3,6–9` |
| 3 | 0 | 2026-10: 0 · 2026-11: 0 | Saída de `sonda.py:4,10–11` |
| 4 | 0 | 2026-10: 0 · 2026-11: 0 | Saída de `sonda.py:5,12–13` |

**2. Competência entregue: 0 de 0.** O script coleta até dez casos da classe (i). Com a amostra vazia, nenhuma consulta de entrega K6 foi feita pelo ramo da amostra. (`r1.md:73–88`, saída de `sonda.py:13–14`)

**3. Guarda da cura2: nenhum caso da amostra para barrar.** Pelo anexo, a recusa ocorreria se, após C6 e a checagem de presença, a redução real ainda desse T1 e a coluna trouxesse T2 posterior sem linha correspondente. A última `RESPOSTA` persistida, sozinha, não estabelece que T1 continua vigente, pois `RECUSA` ou `REABERTURA` pode limpá-la. (`ver2_r.md:27`, `app/chamados/models.py:4455`, `app/chamados/models.py:4495`)

A origem de um T2 em produção, seja conservação legítima de legado ou escrita de `carimbar`, fica **[nao verificado]**. A cópia mostra que `carimbar` pode gravar `respondida_em` sem criar a `RESPOSTA` correspondente quando recebe a coluna nula e há texto. Se a instância já chega com T2 preenchido, a chamada preserva esse instante. (`app/chamados/models.py:3474–3482`, `app/chamados/models.py:3514`, `app/chamados/models.py:4419`)

## 27/11 10:49 · sessao 86646eb56732 · E4C-F5-INSTANTE · leitura · origem=Code

**E4C-F5-INSTANTE.** A cópia tem `DecisaoPergunta.linhas_da_projecao_sem_linha`, que verifica a correspondência por pergunta, tipo de ato e instante, e `DecisaoPergunta.leitura`, que combina as linhas reais com a projeção sem linha. `composta` usa essa leitura, e `par_ato_coluna` compara a presença da validação. O redutor `_reduzir`, a leitura `_reais_reduzidos` e a guarda de instante da cura2 estão ausentes da cópia. A confirmação do SHA por Git, a versão implantada e a incidência atual em produção ficam **[nao verificado]**. [app/chamados/models.py:4416](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:4416) · [app/chamados/models.py:4473](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:4473) · [app/chamados/models.py:4507](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:4507) · [app/chamados/models.py:4518](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:4518) · [app/chamados/models.py:3492](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:3492)

### Sonda

No ambiente de produção, abra `python -B manage.py tenant_command shell --schema=SEU_SCHEMA --interface=python` e cole o bloco. A sonda usa uma transação `REPEATABLE READ, READ ONLY` e imprime o estado atual do universo, com os dez menores PKs de (i). A sintaxe foi conferida em memória. A execução com banco fica **[nao verificado]**. [app/docs/APP-ATESTADO.md:71](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/APP-ATESTADO.md:71) · [app/chamados/tests/cef_c3_sonda.py:21](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/tests/cef_c3_sonda.py:21)

A classe (i) usa a última `RESPOSTA` persistida na ordem `(ato_em, pk)` e a ausência de uma `RESPOSTA` correspondente na coluna. `leitura_antiga_em_T2` indica se a leitura antiga termina em T2. Uma recusa ou reabertura posterior pode limpar a resposta nessa leitura. As contagens mensais usam o mês civil de `data_evento`, como pedido. A entrega usa a competência da janela da empresa para esse dia, por `competencia_do_fato`, que chama `folha.export.competencia_entregue`. Sem `data_evento`, a pergunta entra na contagem e a entrega sai como `NA`. [app/chamados/models.py:4378](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:4378) · [app/chamados/models.py:4438](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:4438) · [app/chamados/models.py:4495](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:4495) · [app/chamados/services/materializacao.py:803](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/materializacao.py:803) · [app/chamados/services/passivo_validado.py:91](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/passivo_validado.py:91)

```python
exec(r"""
from collections import Counter, defaultdict

from django.conf import settings
from django.db import connection, transaction
from django.db.models import Exists, OuterRef, Prefetch
from django.utils import timezone

from chamados.models import DecisaoPergunta as D, PerguntaDisputa
from chamados.services.materializacao import competencia_do_fato

schema = connection.schema_name
if not schema or schema == getattr(settings, "PUBLIC_SCHEMA_NAME", "public"):
    raise RuntimeError("Abra o shell no schema do tenant.")

respostas = D.objects.filter(pergunta_id=OuterRef("pk"), ato=D.RESPOSTA)
decisoes = D.objects.only(
    "pk", "pergunta_id", "ato", "ato_em", "ato_texto", "resposta_mantem_validacao",
)
candidatas = (
    PerguntaDisputa.objects.filter(
        Exists(respostas),
        respondida_em__isnull=False,
        validada_em__isnull=True,
        materializada_em__isnull=True,
    )
    .select_related("disputa__colaborador__empresa")
    .only("data_evento", "respondida_em", "validada_em", "resposta_colab",
          "disputa__colaborador__empresa__dia_inicio_competencia")
    .prefetch_related(Prefetch("decisoes", queryset=decisoes))
    .order_by("pk")
)

totais = Counter()
por_empresa = defaultdict(Counter)
por_mes = defaultdict(Counter)
amostra = []

with transaction.atomic(durable=True):
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
        cursor.execute("SELECT current_schema(), pg_is_in_recovery()")
        schema_atual, em_recuperacao = cursor.fetchone()
        if schema_atual != schema or em_recuperacao:
            raise RuntimeError("A sonda exige o schema do tenant no banco primário.")

    for p in candidatas.iterator(chunk_size=500):
        reais = list(p.decisoes.all())
        ultima = next(d for d in reversed(reais) if d.ato == D.RESPOSTA)
        ja = {(p.pk, d.ato, d.ato_em) for d in reais}
        falta_resposta = any(
            linha["ato"] == D.RESPOSTA
            for linha in D.linhas_da_projecao_sem_linha([p], ja=ja)
        )
        leitura = D.leitura(p)
        classe = "i" if falta_resposta and ultima.ato_em < p.respondida_em else "ii"

        empresa = p.disputa.colaborador.empresa
        mes = p.data_evento.strftime("%Y-%m") if p.data_evento else "SEM_DATA_EVENTO"
        totais[classe] += 1
        por_empresa[empresa.pk][classe] += 1
        por_mes[empresa.pk, mes][classe] += 1

        if classe == "i" and len(amostra) < 10:
            k6 = competencia_do_fato(p, empresa)
            if k6 is None:
                competencia, entregue = "SEM_DATA_EVENTO", "NA"
            else:
                mes_k6, ano_k6, foi_entregue = k6
                competencia = f"{ano_k6:04d}-{mes_k6:02d}"
                entregue = "SIM" if foi_entregue else "NAO"
            ato_em = timezone.localtime(ultima.ato_em).isoformat()
            respondida_em = timezone.localtime(p.respondida_em).isoformat()
            amostra.append(
                f"PK {p.pk} empresa={empresa.pk} mes_data_evento={mes} "
                f"ato_em={ato_em} respondida_em={respondida_em} "
                f"competencia_k6={competencia} entregue={entregue} "
                f"leitura_antiga_em_T2={int(leitura.respondida_em == p.respondida_em)}"
            )

for empresa_id, n in sorted(por_empresa.items()):
    print(f"EMPRESA {empresa_id} i={n['i']} ii={n['ii']}")
for (empresa_id, mes), n in sorted(por_mes.items()):
    print(f"MES_DATA_EVENTO {empresa_id} {mes} i={n['i']} ii={n['ii']}")
for linha in amostra:
    print(linha)
print(f"FIM {totais['i'] + totais['ii']} {totais['i']}")
""")
```

### De onde pode vir T2

- **`PerguntaDisputa.carimbar` é o escritor de aplicação encontrado que preenche `respondida_em` sem registrar a `RESPOSTA` correspondente.** Com `resposta='responder'` ou `'validar'`, texto preenchido e `self.respondida_em` nulo, atribui `quando`. Se as guardas passam, grava a projeção por `save`. Nessa base, o próprio `carimbar(resposta='validar')` termina nesse `save`, sem registrar a foto `_sem_linha`. O fecho sem credor chama com `'responder'`. A absorção chama com `'validar'` e registra `ABSORCAO`. [app/chamados/models.py:3474](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:3474) · [app/chamados/models.py:3504](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:3504) · [app/chamados/services/disputa_emissao.py:2192](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/disputa_emissao.py:2192) · [app/chamados/services/materializacao.py:1108](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/materializacao.py:1108) · [app/chamados/services/materializacao.py:1129](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/materializacao.py:1129)
- **O acervo anterior à E1 também pode trazer o instante só na coluna.** A migração 0051 converteu as linhas antigas em `REABERTURA`. Na base, `_nascer` registra as assinaturas recebidas, e os escritores de resposta humana, automática e pela trilha registram a `RESPOSTA` no mesmo instante que gravam na coluna. [app/chamados/models.py:4419](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:4419) · [app/chamados/migrations/0051_decisao_pergunta.py:41](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/migrations/0051_decisao_pergunta.py:41) · [app/chamados/models.py:3333](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:3333) · [app/chamados/services/disputa_emissao.py:1562](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/disputa_emissao.py:1562) · [app/chamados/services/disputa_emissao.py:1757](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/disputa_emissao.py:1757) · [app/chamados/services/disputa_emissao.py:1793](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/disputa_emissao.py:1793)
- **`save` e `descarimbar_em_lote` são mecanismos de persistência sem `registrar` próprio.** Nos chamadores do lote que passam `respondida_em`, o valor passado é `None`. O `descarimbar` unitário pode tirar a materialização preservando T2, como faz `desfazer_carimbo_sem_lastro` quando C6 volta a dar a pergunta por viva. [app/chamados/models.py:3840](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:3840) · [app/chamados/models.py:3657](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:3657) · [app/chamados/models.py:1739](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:1739) · [app/chamados/services/disputa_emissao.py:1661](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/disputa_emissao.py:1661) · [app/chamados/services/validacao.py:476](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/validacao.py:476) · [app/chamados/models.py:3572](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:3572) · [app/chamados/services/relavra_via.py:242](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/relavra_via.py:242)

**T2 só na coluna é possível por construção.** Para o caso misto, `carimbar` exige que o instante esteja nulo na instância que recebe, então preserva um T1 já preenchido nessa instância. A sonda mede a divergência persistida. A origem e a legitimidade de cada T2, bem como a chegada de cada candidata ao ponto de recusa da cura2, ficam **[nao verificado]**. [app/chamados/models.py:3471](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:3471) · [app/chamados/models.py:3483](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/models.py:3483)

## 27/11 10:47 · sessao 4c37eb8792c5 · ORFAS-VOLTOU · leitura · origem=Code

O `sem_evento=0` informado às 06:36 é a leitura do vigia do **carimbo por lâmpada**. O estoque de perguntas candidatas a reate é publicado em `logs/reconciliador.log`, na linha `orfas reataveis (UNIVERSO, MEDIDA ...)`. O total atual da frota, a empresa das quatro células e o ramo das 16 perguntas estão [nao verificado]. `app/ponto/management/commands/reconciliar_perguntas_orfas.py:29–36` `app/chamados/management/commands/reconciliar_chamados.py:97–115` `app/config/crons.py:395`

## 1. Porta de reate, chamadores e trilha

`reatar_pergunta_orfa(p, colab)` primeiro deriva um destino vivo, usando `p.data_evento` antes da data do contexto. Sem destino derivado, monta o histórico de chamados `resolvido` ou `fechado` do colaborador para `contexto_json.data_turno`, excluindo `disputa_supervisao`. Havendo históricos, para pergunta datada em dia que o juiz põe no alcance do O261(C), C6 viva esvazia o histórico e C6 morta casa pelo juiz `motivos_atendidos`. Fora desse alcance, inclusive dia entregue ou trancado, o histórico usa os mapas de categoria e subcategoria. `app/chamados/reconciliador.py:838–871,1711–1765` `app/chamados/catalogo/motor.py:30`

| Retorno | Predicado da saída | Fonte |
|---|---|---|
| `False` | Sem destino vivo derivado e sem data da pergunta nem data no contexto do motivo. | `app/chamados/reconciliador.py:1711–1725` |
| `True` | Histórico vazio e `lavrar_sem_turno_aberto` consegue carimbar. É o c1, que pode deixar a FK nula. | `app/chamados/reconciliador.py:403–417,1766–1775` |
| `True` | Histórico vazio, c1 devolve falso e `reemitir_chamado_do_dia` consegue costurar a pergunta ao chamado vivo. | `app/chamados/reconciliador.py:1032–1080,1774–1777` |
| `False` | Histórico vazio e c1 e c2 devolvem falso, ou o histórico tem mais de um candidato. | `app/chamados/reconciliador.py:1766–1779` |
| `False` | Histórico único e a costura recusa, ou a cerca veta a escrita, desfaz o ato e registra a retenção no chamado histórico. | `app/chamados/reconciliador.py:1800–1816` |
| `True` | Histórico único, costura aceita, S128 retorna normalmente e a cerca libera. A porta descarta o retorno do S128, que pode ter carimbado zero perguntas. | `app/chamados/reconciliador.py:1800–1816` `app/chamados/services/disputa_emissao.py:2033–2042,2087–2122` |
| `False` | Há destino vivo derivado, mas `reapontar_resolvedora` devolve recusa. | `app/chamados/reconciliador.py:1821–1825` |
| `True` | Há destino vivo derivado e `reapontar_resolvedora` aceita. | `app/chamados/reconciliador.py:1821–1826` |
| Exceção | O corpo não tem `raise` explícito. Falhas não tratadas das chamadas internas sobem. Os dois parses locais tratam `ValueError` convertendo a data a `None`. A cerca própria converte veto anotado em recusa. Com cerca externa, a exceção do ato pode subir para quem abriu essa cerca. | `app/chamados/reconciliador.py:1697–1826` `app/chamados/services/fio.py:1564–1592` |

O **c1** exige motivo `orfao_14h` ou `saida_sem_entrada`, ausência de resposta pelo juiz, ausência de carimbo, ausência de turno aberto pelo núcleo, C6 morta e carimbo aceito. O **c2** passa pelos gates de motivo, dia, colaborador ativo, isenção, ausência, veto da célula e motor. Para nascer ou renascer, aplica também horizonte, protesto que cobra, regra da parcial só lista e régua. Para os dois motivos de intervalo, exige C6 viva e dia dentro do alcance O261(C). Por fim precisa obter chamado vivo e passar pela costura. Como `abrir` vem antes da costura, `False` no c2 pode vir depois de uma escrita no chamado. `app/chamados/reconciliador.py:333–417,838–880,930–1080`

**Chamadores diretos na cópia** são `gerar_perguntas_disputa` na emissão e `reconciliar_perguntas_orfas_colab` no laço por colaborador. A batida chega ao laço pelo `post_save` de `Batida`. Também chega o `on_commit` de um chamado filho que sai de `VIVOS`, quando há pai vivo do mesmo colaborador que lista o PK do filho. Execuções concretas para as quatro células [nao verificado]. `app/chamados/services/disputa_emissao.py:468–478` `app/chamados/reconciliador.py:1583–1588,1674–1685` `app/ponto/signals.py:211–223` `app/chamados/signals.py:351–371`

**Trilha.** A costura grava destino, veredito lavrado e evento de célula. A porta escreve log `INFO` em recusas de costura e, no veto do histórico, usa `registrar_retencao` para gravar `retratacao_retida_competencia_entregue` no chamado. Ramos delegados podem gravar trilhas de nascimento de chamado, fechamento de disputa e `fecho_sem_credor` em pergunta. A autoria humana da tentativa precisa ser registrada pelo chamador. `app/chamados/services/fio.py:1082–1095` `app/chamados/reconciliador.py:493–509,1800–1826` `app/chamados/models.py:1037–1046,2199–2207` `app/chamados/services/disputa_emissao.py:2190–2202`

A porta existente para a sessão é **`core.observ.evento`**, com `modelo="PerguntaDisputa"`, `objeto_id=p.pk`, `ator` igual ao objeto `User` do executor e o motivo em `descricao`. `LogAuditoria` guarda `usuario` e `criado_em`. O helper de request `core.auditoria.registrar_log` delega a ela. A sonda usa o precedente de levantar erro dentro do `atomic` se `evento` devolver `None`. `app/core/observ.py:94–139` `app/core/models.py:295–307` `app/core/auditoria.py:8–30` `app/chamados/services/fio.py:1202–1208` `app/chamados/services/validacao.py:395–405,497–498`

## 2. Elegibilidade e campos

| Seletor | O que exige |
|---|---|
| `perguntas_orfas_reatavies(colab)` | `chamado_destino` e `materializada_em` nulos, disputa aberta do colaborador e motivo em `orfao_14h`, `saida_sem_entrada`, `intervalo_saida`, `intervalo_volta`. `app/chamados/reconciliador.py:1637–1661` `app/chamados/models.py:4169–4170` |
| `_orfas_abertas(colaborador_ids=None)` | `materializada_em` e `validada_em` nulos, disputa aberta, `data_evento` presente e motivo `orfao_14h` ou `saida_sem_entrada`. Aceita qualquer valor de `chamado_destino`, com recorte de colaboradores opcional. `app/chamados/reconciliador.py:1162–1176` |

Com destino nulo, **disputa fechada, `materializada_em` preenchido ou motivo fora dos quatro** já bastam, cada um, para ficar fora dos dois universos. `validada_em` preenchido, `data_evento` nulo ou motivo de intervalo podem tirar só do universo do carimbo. Os dois seletores leem as condições acima, sem filtrar `via_resolucao` ou `veredito_lavrado`. `app/chamados/reconciliador.py:1166–1176,1654–1661` `app/chamados/models.py:4169–4170`

Na **pergunta**, “sem via” corresponde a `via_resolucao == ""` e veredito nunca lavrado a `veredito_lavrado == ""`. `julgado_em` é o instante da lavra. Na **célula**, os campos são `veredito` e `veredito_via`, ambos anuláveis. Há ainda o sentido de **resposta sem veredito**, que usa a validação da leitura composta, a materialização, o juiz de resposta e a tranca. A sonda mostra os campos da pergunta e da célula. Qual leitura produziu a descrição raspada [nao verificado]. `app/chamados/models.py:2040–2045,2872–2906` `app/escala/models.py:1782–1785`

## 3. Vigia da frota e o zero histórico

- **Estoque para reate.** `reconciliar_chamados.Command.handle` soma `perguntas_orfas_reatavies(c).count()` para todos os colaboradores do tenant quando roda sem `--colab`. A configuração agenda `reconciliar_chamados --alarme` a cada 15 minutos e grava a saída em `logs/reconciliador.log`. É a linha para ler o número já medido. Ela conta o universo elegível do item 2, inclui colaboradores com evento pendente, dispensa o C6 e a tentativa de reate, e fica fora da soma do alarme. O placar de corridas grava metadados da execução e o código de saída. `app/chamados/management/commands/reconciliar_chamados.py:57–62,89–124` `app/config/crons.py:17–21,29,395,1908–1917` `bin/cron_run.sh:2–13`
- **Fio solto.** O auditor diário das 07:18 também publica `I8` em `logs/invariantes.log` e no HTML. Ele parte de perguntas com destino nulo, motivo atendível, colaborador ativo e C6 viva. A faixa `I8o` registra à parte os casos com ao menos dois candidatos e nenhum `entrada_timestamp`. `app/config/crons.py:542–543` `app/chamados/management/commands/auditar_invariantes_chamados.py:51–95,818–831,860–872,1866–1884`
- **`332 → 0` de 29/10.** O BACKLOG resume o pouso `ac721607`. O RELATO documenta `sem_evento=332`, semeadura de 379 eventos, 332 perguntas encerradas e `sem_evento=0` depois. A função continua sendo `orfas_pela_lampada_sem_evento`. Hoje ela mede o mesmo resíduo de carimbo por lâmpada. Em `_orfas_abertas`, exige pergunta sem resposta pelo juiz, lâmpada do próprio marco acesa, C6 viva, dia passado e encerrado, e colaborador sem evento vivo pendente. A comparação sustentada pelo dado fornecido é **`sem_evento` 0 em 29/10 e 0 em 27/11**. O estoque `orfas reataveis` de hoje está [nao verificado]. `app/docs/BACKLOG.md:17,810` `app/docs/RELATO.md:7010–7015` `app/chamados/reconciliador.py:1182–1233,1325–1341`

## 4. Por que o cron da manhã não reata

O **06:40** era o horário antigo. Desde 29/09, a configuração encadeia `reconciliar_perguntas_orfas` ao fim de `disparar_perguntas_competencia`, com `--alarme` e saída em `perguntas_orfas.log`. O comando sempre conta e imprime `[VIGIA] sem_evento=N (esperado 0)`. Com `--alarme` e `N > 0`, também imprime até 20 PKs em stderr e levanta `SystemExit(2)`. Sem a flag, termina depois da contagem. O corpo tem apenas leitura e saída. O BACKLOG registra que a falta de redirecionamento do log foi sanada e que o arquivo já voltara a ser gravado em 08/11. `app/config/crons.py:479–485,1528–1531,1548–1554,1646–1651` `app/ponto/management/commands/reconciliar_perguntas_orfas.py:24–36` `app/docs/BACKLOG.md:811`

Em **23/10**, VARREDURA 1b-iii parte 3 extraiu a porta do laço antigo e ligou o reate ao nascimento na emissão. O RELATO cita a cópia `2263122b`. Em **29/10**, `ac721607` moveu o **carimbo fantasma** ao consumidor de `EventoCelula`. Em **05/11**, JUIZ-VARREDURA-3, documentado no deploy `de9fd69b`, converteu o `reconciliar_chamados` de `*/15`, que ainda reatava, em vigia. `app/chamados/reconciliador.py:1688–1695` `app/docs/RELATO.md:8882–8888,7010–7015,13032–13041` `app/config/crons.py:2094–2104` `app/chamados/management/commands/reconciliar_chamados.py:1–16`

A emissão pode criar com FK nula quando o derivador termina com zero ou múltiplos candidatos após o desempate. A tentativa imediata só ocorre se a pergunta foi **criada nessa chamada**, segue com FK nula e tem motivo do grupo do dia. Pode ficar sem vínculo se a tentativa recusa, falha no savepoint ou só carimba pelo c1. Uma emissão posterior de outra pergunta não percorre o estoque nesse bloco. O consumidor de `EventoCelula` faz o carimbo e chama a fábrica para os dias do lote, e a fábrica deduplica o fato já representado mesmo quando sua pergunta segue sem destino. O estoque elegível volta à porta no laço do colaborador, acionado pela batida ou pelo evento de filho descrito acima. Se a ambiguidade some sem um desses gatilhos, ou a pergunta já está fora do seletor do laço, não há nova tentativa por esses caminhos. Qual condição ocorreu nas 16 [nao verificado]. `app/chamados/services/disputa_emissao.py:191–244,417–478` `app/chamados/reconciliador.py:1523–1537,1583–1588,1654–1685,1774–1775` `app/chamados/services/fabrica_da_celula.py:138–174,207–219,288–302` `app/chamados/management/commands/reconciliar_chamados.py:24–25`

## 5. Sonda para o shell do tenant

Abra `python -B manage.py tenant_command shell --schema=SEU_SCHEMA` e cole o `exec` abaixo. O modo padrão é `ensaio`. Em `aplicar`, preencha `ATOR_PK` com o PK do usuário executor. `FIM` conta como reatada a pergunta que saiu de FK nula para FK preenchida com retorno `True`. A foto do ensaio é tirada antes do rollback de cada pergunta. A porta expõe um booleano e alguns logs de ramo, sem diagnóstico estruturado que cubra todas as recusas. Por isso a saída por tentativa traz retorno e estado. Sintaxe conferida. Execução com Django e banco [nao verificado]. `bin/sonda_frota.sh:45–46` `app/core/observ.py:108–137` `app/chamados/reconciliador.py:1774–1826`

A sonda valida os quatro pares de PK e data, as quatro perguntas por célula e o vínculo ainda nulo sob a trava existente. Em `aplicar`, registra também as tentativas que devolvem `False`, pois o c2 pode escrever antes da recusa. Ela interrompe o `aplicar` se encontrar regularização externa confirmada e pendente do colaborador. O fechamento de uma disputa pode retomar essa regularização depois do commit e escrever batidas. A checagem lê a pendência existente naquele instante. Uma confirmação concorrente pode gravar seu estado por outra transação, e a ausência dessa concorrência está [nao verificado]. `app/chamados/models.py:2930–2949` `app/chamados/reconciliador.py:410–417,1032–1080,1800–1807` `app/chamados/signals.py:122–146` `app/chamados/services/regularizacao_ext.py:71–82,122–156,211–225,348–357`

```python
exec(r"""
from collections import Counter
from datetime import date

from django.conf import settings
from django.contrib.auth import get_user_model
from django.db import connection, transaction
from django.utils import timezone

from chamados.juizes import pergunta_viva
from chamados.models import PerguntaDisputa, RegularizacaoExterna
from chamados.reconciliador import (
    _orfas_abertas,
    perguntas_orfas_reatavies,
    reatar_pergunta_orfa,
)
from chamados.services.validacao import TrilhaNaoGravada
from core.observ import evento
from escala.models import CelulaDia

MODO = "ensaio"
ATOR_PK = None  # Em aplicar, PK do auth.User de quem executa.
AVAL = "ORFAS-VOLTOU-2711-1045"
DIAS = {
    123535: date(2026, 11, 1),
    123541: date(2026, 11, 7),
    123547: date(2026, 11, 13),
    123553: date(2026, 11, 19),
}


class DesfazerEnsaio(Exception):
    pass


def local(instante):
    return timezone.localtime(instante).isoformat() if instante is not None else None


def estado(p):
    return {
        "chamado_destino_id": p.chamado_destino_id,
        "materializada_em": local(p.materializada_em),
    }


def executar():
    if MODO not in ("ensaio", "aplicar"):
        raise ValueError("MODO deve ser ensaio ou aplicar")
    schema = getattr(connection, "schema_name", None)
    if not schema or schema == getattr(settings, "PUBLIC_SCHEMA_NAME", "public"):
        raise RuntimeError("Abra o shell no schema do tenant")
    if not transaction.get_autocommit():
        raise RuntimeError("A sonda precisa de uma transacao independente por pergunta")

    celulas = CelulaDia.objects.select_related("colaborador").in_bulk(DIAS)
    if {pk: c.data for pk, c in celulas.items()} != DIAS:
        raise RuntimeError("PKs e datas das celulas divergem do aval")
    donos = {c.colaborador_id for c in celulas.values()}
    if len(donos) != 1:
        raise RuntimeError("As quatro celulas precisam ter o mesmo colaborador")
    colab = celulas[next(iter(DIAS))].colaborador
    print("colaborador_id", colab.pk, "empresa_id", colab.empresa_id)
    for c in celulas.values():
        print("celula_id", c.pk, "data", c.data, "celula.veredito", c.veredito,
              "celula.veredito_via", c.veredito_via)

    qs = PerguntaDisputa.objects.select_related("celula", "disputa__colaborador")
    perguntas = list(qs.filter(celula_id__in=DIAS).order_by("celula_id", "pk"))
    pks = [p.pk for p in perguntas]
    reataveis = set(perguntas_orfas_reatavies(colab).filter(pk__in=pks)
                    .values_list("pk", flat=True))
    abertas = set(_orfas_abertas([colab.pk]).filter(pk__in=pks)
                  .values_list("pk", flat=True))

    for p in perguntas:
        print({
            "pk": p.pk, "celula_id": p.celula_id, "motivo": p.motivo,
            "data_evento": p.data_evento.isoformat() if p.data_evento else None,
            **estado(p), "validada_em": local(p.validada_em),
            "via_resolucao": p.via_resolucao, "veredito_lavrado": p.veredito_lavrado,
            "disputa_id": p.disputa_id, "disputa.fechada_em": local(p.disputa.fechada_em),
            "perguntas_orfas_reatavies": p.pk in reataveis,
            "_orfas_abertas": p.pk in abertas, "pergunta_viva": pergunta_viva(p),
        })

    if Counter(p.celula_id for p in perguntas) != {pk: 4 for pk in DIAS}:
        raise RuntimeError("A lista atual diverge das quatro perguntas por celula do aval")

    ator = None
    if MODO == "aplicar":
        if ATOR_PK is None:
            raise ValueError("Preencha ATOR_PK com o PK do usuario executor")
        ator = get_user_model().objects.get(pk=ATOR_PK)

    n_reatadas = 0
    for original in perguntas:
        try:
            with transaction.atomic():
                PerguntaDisputa.trava_do_colaborador(colab.pk)
                p = qs.get(pk=original.pk)
                if (p.celula_id != original.celula_id or
                    p.colaborador_id != colab.pk or
                    p.disputa.colaborador_id != colab.pk or
                    p.chamado_destino_id is not None):
                    raise RuntimeError(f"Pergunta {p.pk} diverge do alvo orfao do aval")

                if MODO == "aplicar" and RegularizacaoExterna.objects.filter(
                    colaborador_id=colab.pk, status="confirmada", materializada_em__isnull=True,
                ).exists():
                    raise RuntimeError(f"Colaborador {colab.pk} tem regularizacao confirmada pendente")

                antes = estado(p)
                retorno = reatar_pergunta_orfa(p, p.disputa.colaborador)
                p.refresh_from_db()
                depois = estado(p)
                resultado = {"pk": p.pk, "retorno": retorno, **depois}
                if MODO == "ensaio":
                    raise DesfazerEnsaio(resultado)

                log = evento(
                    "reatar_pergunta_orfa", "PerguntaDisputa", p.pk,
                    descricao=f"{AVAL} | Tentativa de reatar a pergunta pela porta existente",
                    antes=antes, depois={**depois, "retorno": retorno}, ator=ator,
                )
                if log is None:
                    raise TrilhaNaoGravada(f"{AVAL} | Pergunta {p.pk}")
        except DesfazerEnsaio as ensaio:
            resultado, = ensaio.args
        print(resultado)
        if resultado["retorno"] is True and resultado["chamado_destino_id"] is not None:
            n_reatadas += 1
    print(f"FIM {len(perguntas)} {n_reatadas}")


executar()
""")
```

## 27/11 10:44 · sessao 52b726a95a4a · E4C-F5-INSTANTE · construtor · origem=Code

**Parei conforme a condição do P3.** Em `validar_pergunta`, o veto detectado após a tentativa de plantio desfaz o `atomic` que adquiriu a trava do dono. Só depois do rollback a função relê a pergunta e chama `_recusa_da_entrega`, que chama `trilha_do_veto_k6`. A trava adquirida naquele bloco já caiu. [validacao.py:755](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/validacao.py:755) · [validacao.py:834](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/validacao.py:834) · [validacao.py:237](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/validacao.py:237) · [trava_do_colaborador.py:16](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/trava_do_colaborador.py:16)

- **Caminhos com trava.** O veto inicial da validação chama a trilha dentro do `atomic` que tomou a trava. O laço da materialização e o fluxo do passivo também tomam a trava antes de chegar a `_materializar_uma`. [validacao.py:755](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/validacao.py:755) · [validacao.py:795](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/validacao.py:795) · [materializacao.py:2228](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/materializacao.py:2228) · [materializacao.py:2300](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/materializacao.py:2300) · [materializacao.py:2318](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/materializacao.py:2318) · [passivo_validado.py:393](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/services/passivo_validado.py:393)
- **M16.** Pela leitura do código, o mutante apaga as fotos e a descrição. A asserção de fotos completas reprova as duas metades em cada porta. Execução **[nao verificado]**. [e4c_f5_c4red2_mutantes.py:253](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:253) · [e4c_f5_c4red2_mutantes.py:474](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:474) · [test_e4c_f5_c4_red.py:996](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:996)
- **Multiempresa.** Controle esperado por leitura estática. A porta passa todas as Empresas do lote à guarda, que encontra o primeiro dono e lança `EntregaNaoTravada` pela Empresa registrada depois dele. O teste novo ficou pendente com a parada. Execução **[nao verificado]**. [models.py:4868](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/models.py:4868) · [export.py:613](/home/ronald/wt-codex-e4c-f5-instante-012038/app/folha/export.py:613) [nao verificado]

Nenhum arquivo foi alterado, nenhum `except` foi adicionado e nenhum teste foi executado. A deduplicação, o isolamento e o M44 ficaram pendentes. [nao verificado]

Comandos dos casos existentes para o Code. [test_e4c_f5_c4_red.py:1034](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:1034) · [e4c_f5_c4red2_mutantes.py:474](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:474)

```bash
PYTHONDONTWRITEBYTECODE=1 python manage.py test chamados.tests.test_e4c_f5_c4_red.Item13PorPKTest.test_RED_item13_veto_repetido_deixa_uma_linha_de_trilha
PYTHONDONTWRITEBYTECODE=1 python manage.py test chamados.tests.e4c_f5_c4red2_mutantes.M16TrilhaVazia.test_mordida
```

_8641 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
