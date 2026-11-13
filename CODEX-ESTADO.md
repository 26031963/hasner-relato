# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 20:21** · publicado em **13/11/2026 20:22** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 20:21 · sessao 37a98bab2c73 · - · - · origem=Ronald (tmux)

```text
BO-ADMIN-2710@777|I|Medir oito falhas relatadas pelo admin|V
TENTATIVA-DO-ADMIN-VIRA-COBRANCA@779|U|Prever cobranças antes de salvar vínculo
CODEX-CONTEXTO-DA-CASA@1040|I|Consulta recebe contexto verificável da casa|X
PENDENCIA-SEM-JUIZ-3@782|L|Contagem de pendências segue juiz único|dep SILENCIO-VENCIDO-VOLTA
COBERTURA-PARCIAL-UM-JUIZ@786|L|Cobertura parcial mantém restante do dia|P
JUIZ-DO-PAR-GRUPO-1@812|L|Leitores consultam pareador único|V
DISPARAR-PERGUNTAS-NO-CONTRATO-20@814|L|Disparar perguntas pela autoridade do contrato|P|+........|dep E3-A,PAR
```

`@N` = `app/docs/BACKLOG.md:N`. Confirmadas as quatro restaurações: respectivamente conclusão parcial, reabertura com PAREI, pouso parcial e apenas censo fechado. `app/docs/BACKLOG.md:782`; `app/docs/BACKLOG.md:786`; `app/docs/BACKLOG.md:812`; `app/docs/BACKLOG.md:814`.

| balde | incluídas corrigidas |
|---|---:|
| ESPINHA-E | 50 |
| 20/20 | 0 |
| LATERAL-ESTRUTURA | 174 |
| INSTRUMENTO | 88 |
| UI | 24 |
| CEREJA | 9 |
| ? | 3 |
| **Total** | **348** |

**Excluídas: 107. Conferência: 348 + 107 = 455.** Contagem do inventário corrigido; classificação e estados apoiados nas linhas acima e no inventário anterior.

## 13/11 20:21 · sessao 3916451eddb2 · E3-FORMA-SQL · construtor · origem=Code

A cura está escrita, mas **a entrega não está toda GREEN**: resta um assert da volta 5 que exige `admin` onde a decisão desta volta exige `colab`. Ele foi preservado. ([test_e3_portas_tarefa.py:248](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/test_e3_portas_tarefa.py:248))

O diff por arquivo:função ficou assim:

- **`chamados/services/tarefa_do_dia.py`: docstring e `_portas_da_tarefa`** — documenta o significado único de `fio`; une portas das ações, chamados da célula e âncoras carregadas do dia, sempre intersectadas com `fila_ids`; combina donos das ações com donos dos chamados residuais. ([contrato:41](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/services/tarefa_do_dia.py:41), [função:549](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/services/tarefa_do_dia.py:549))
- **Mesmo arquivo: `porta_da_tarefa`** — publica também `nao_sei`. ([linha 595](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/services/tarefa_do_dia.py:595))
- **`chamados/services/furo_sem_canal.py`: `medir`** — contabiliza desconhecimento independentemente da existência de porta; `sem_canal` continua definido pela ausência de porta. ([linhas 98 e 101](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/services/furo_sem_canal.py:98))
- **`chamados/tests/test_e3_portas_v6_sem_banco.py`: duas classes novas** — cobre donos, combinação com residual, dispensas, âncora não exposta, rotas e consumidores puros. ([linha 15](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/test_e3_portas_v6_sem_banco.py:15))
- **`chamados/tests/e3_portas_v6_mutantes.py`: executor novo** — reproduz RED na base informada e aplica mutações exclusivamente em memória, exigindo que o teste alvo passe antes da mutação. ([linha 89](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/e3_portas_v6_mutantes.py:89))

Os comandos abaixo usam esta abreviação, executada em `app/`:

```bash
m() { env -u DJANGO_SETTINGS_MODULE PYTHONDONTWRITEBYTECODE=1 python3 -m "$@"; }
```

| Comando | Resultado observado |
|---|---|
| `m chamados.tests.e3_portas_v6_mutantes --base` | Ran 10; FAILED, 17 falhas contando subcasos, zero erros. |
| `m unittest chamados.tests.test_e3_portas_v6_sem_banco` | Ran 10; OK. |
| `m chamados.tests.e3_portas_regra` | Ran 10; FAILED, uma falha, zero erros: expectativa `admin` preservada. |
| `m unittest chamados.tests.test_e3_portas_tarefa` | Ran 1; FAILED, errors=1 na importação. |
| `m unittest chamados.tests.test_e3_leitores_c17_chamadores` | Ran 1; FAILED, errors=1 na importação. |
| `m unittest chamados.tests.test_o261e_f5b_juiz_porta` | Ran 1; FAILED, errors=1 na importação. |
| `m unittest chamados.tests.test_e2_tarefa_do_dia` | Ran 1; FAILED, errors=1 na importação. |

O RED usa o hash solicitado; o recorte da volta 5 mantém os corpos originais dos testes. ([executor:113](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/e3_portas_v6_mutantes.py:113), [recorte:23](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/e3_portas_regra.py:23)) [nao verificado]

Nos quatro módulos completos, o erro foi `ModuleNotFoundError: No module named 'django.contrib'`; esse “Ran 1” corresponde à falha do carregador, sem executar os casos ORM. Execução Django efetiva: **[nao verificado]**.

Também passaram **48 testes existentes sem banco**, com: [nao verificado]

```bash
m unittest chamados.tests.test_e3_tarefa_admin_sem_banco chamados.tests.test_e3_c8b_sem_banco chamados.tests.test_e3_c8c_sem_banco chamados.tests.test_e3_pos_suite_sem_banco
```

São os testes dos contratos anteriores de tarefa, carga e bordas temporais. ([test_e3_tarefa_admin_sem_banco.py:124](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:124), [test_e3_c8b_sem_banco.py:54](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/test_e3_c8b_sem_banco.py:54)) [nao verificado]

Os **15 mutantes foram detectados por assert, todos com ERROR=0**. Os comandos individuais são:

```bash
m chamados.tests.e3_portas_v6_mutantes --mutante razao_esconde
m chamados.tests.e3_portas_v6_mutantes --mutante sistema_esconde
m chamados.tests.e3_portas_v6_mutantes --mutante residual_A_some
m chamados.tests.e3_portas_v6_mutantes --mutante ancora_some
m chamados.tests.e3_portas_v6_mutantes --mutante ancora_so_exposta
m chamados.tests.e3_portas_v6_mutantes --mutante fora_da_fila_entra
m chamados.tests.e3_portas_v6_mutantes --mutante historico_nomeado
m chamados.tests.e3_portas_v6_mutantes --mutante verbo_inventado
m chamados.tests.e3_portas_v6_mutantes --mutante fio_unificado_pela_escada
m chamados.tests.e3_portas_v6_mutantes --mutante fio_residual_admin
m chamados.tests.e3_portas_v6_mutantes --mutante fio_residual_ignorado
m chamados.tests.e3_portas_v6_mutantes --mutante fio_acao_sem_porta_ignorada
m chamados.tests.e3_portas_v6_mutantes --mutante encerrada_rota_pergunta
m chamados.tests.e3_portas_v6_mutantes --mutante projecao_sem_nao_sei
m chamados.tests.e3_portas_v6_mutantes --mutante contador_so_sem_porta
```

Alvos e testes correspondentes estão no mapa de mutantes; RC 1 significa mordida por assert, RC 2 indica problema. ([mapa:21](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/e3_portas_v6_mutantes.py:21), [critério:99](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/e3_portas_v6_mutantes.py:99)) [nao verificado]

Quem escreve e quem lê cada campo:

| Campo | Quem escreve/fornece | Quem lê |
|---|---|---|
| `portas` local | `_portas_da_tarefa`, união em `tarefa_do_dia.py:570`; recebe fila de `chamados/juizes.py:1549` e âncoras da carga em `tarefa_do_dia.py:543`. | A própria função, para verbos, residuais e rota: `tarefa_do_dia.py:575`, `:579`, `:585`. |
| `fila` | `_portas_da_tarefa`, `tarefa_do_dia.py:576`; publicada em `:595`. | Folha nomeia chamados em `folha/fora_da_folha.py:239`; diff projeta em `chamados/management/commands/diff_e3_sombra.py:49`. |
| `verbos` | Painel escreve o mapa em `chamados/services/verbos.py:52`; tarefa seleciona em `tarefa_do_dia.py:575`. | Dono dos residuais em `tarefa_do_dia.py:582`; assinatura do diff em `diff_e3_sombra.py:144`. |
| `fio` | Donos das ações em `tarefa_do_dia.py:578`; residual via `chamados/juizes.py:1633`; combinação em `tarefa_do_dia.py:583`. | Folha em `fora_da_folha.py:89`; vigia em `vigia_fio.py:653`; pauta indiretamente em `pautas_dos_discordantes.py:81`. |
| `nao_sei` | `_tarefa_pura`, inicialização em `tarefa_do_dia.py:193` e marcação em `:319`; projeção em `:595`. | Fallback do fio em `tarefa_do_dia.py:584`; contador em `furo_sem_canal.py:98`. |
| `rota` | `_portas_da_tarefa`, `tarefa_do_dia.py:585`; projeção em `:595`. | Vigia em `vigia_fio.py:652`; furo em `furo_sem_canal.py:97`; cadastro testa existência em `cadastro_realidade.py:497`; diff em `diff_e3_sombra.py:53`. |

Os efeitos abaixo decorrem desses leitores; integração ORM e quantidades em produção permanecem **[nao verificado]**:

| Leitor | Caso em que muda |
|---|---|
| Folha `_fio_da_porta` / `_grupo` | Porta antes omitida passa de `nada` ao dono publicado. Com residual `decidir`, passa a `chamado`; grupo D→C quando não há outro dia pior. (`folha/fora_da_folha.py:86`, `:92`) |
| `vigia_fio.censo_alcance` | Dia agora com porta entra em `alcance_porta_fila`; `exige_admin` depende exclusivamente do fio publicado. (`chamados/services/vigia_fio.py:648`) |
| `pautas_dos_discordantes` | Dia discordante que ganhou fio deixa a seleção de dias sem fio. (`ponto/services/pautas_dos_discordantes.py:81`) |
| `furo_sem_canal` | Dia acusador que ganhou porta sai de `total`; se desconhecido, continua contado em `nao_sei`. (`chamados/services/furo_sem_canal.py:98`, `:101`) |
| `cadastro_realidade.beco` | Com as demais condições do cadastro e pergunta viva satisfeitas, a nova porta impede a classificação como beco. Sem pergunta viva, esse trecho não muda. (`escala/services/cadastro_realidade.py:497`) |
| `diff_e3_sombra` | `candidata` recebe a nova projeção; compara alterações de fio, fila, rota e existência. Suas projeções e assinaturas não incluem `nao_sei`. (`chamados/management/commands/diff_e3_sombra.py:43`, `:46`, `:142`) |

O contraexemplo adicional é **dia isento com âncora externa não exposta nas instâncias**: 202 está na fila; 303 só está no universo de verbos. A saída conserva somente 202, com `porta_fila`, `fila=()` e fio `chamado`, sem alterar as instâncias. Esse caso falhou antes da correção complementar e agora passa. ([teste:61](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/test_e3_portas_v6_sem_banco.py:61)) [nao verificado]

Permanecem três incompatibilidades nos asserts preservados:

- O RED exige `admin` para `responder/colaborador + validar`; a cura conserva `colab`. ([teste:248](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/test_e3_portas_tarefa.py:248)) [nao verificado]
- O teste GPS exige só B em `verbos`; sua reprodução original, sem ORM, falha porque a cura conserva também A. ([assert:91](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/test_e3_portas_tarefa.py:91), [reprodução:77](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/e3_portas_apoio.py:77)) [nao verificado]
- O F5B compara integralmente a porta C17 de quatro campos com a projeção que agora obrigatoriamente tem `nao_sei`. Incompatibilidade identificada por leitura; execução ORM **[nao verificado]**. ([assert:244](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/test_o261e_f5b_juiz_porta.py:244), [C17:1687](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/juizes.py:1687), [projeção:595](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/services/tarefa_do_dia.py:595))

Restauração dos 79 casos, medição dos 25 SC-A, suite completa e certificação final: **[nao verificado]**. [nao verificado]

## 13/11 20:20 · sessao 37a98bab2c73 · - · - · origem=Ronald (tmux)

| balde | incluídas |
|---|---:|
| ESPINHA-E | 50 |
| 20/20 | 0 |
| LATERAL-ESTRUTURA | 170 |
| INSTRUMENTO | 86 |
| UI | 23 |
| CEREJA | 9 |
| ? | 3 |
| **Total** | **341** |

**Excluídas na lista: 111.** Contagem do texto enviado, incluindo os três acréscimos finais; sem reler fontes.

## 13/11 20:20 · sessao df08623b5c4e · - · - · origem=Ronald (tmux)

Contagem do inventário enviado, já removidas as 38 exclusões e consolidados os dois IDs repetidos:

| balde | obras |
|---|---:|
| ESPINHA-E | 15 |
| 20/20 | 7 |
| LATERAL-ESTRUTURA | 197 |
| INSTRUMENTO | 123 |
| UI | 22 |
| CEREJA | 0 |
| ? | 0 |
| **TOTAL** | **364** |

Base aritmética: 404 linhas − 38 excluídas − 2 duplicatas = 364. Antes das duplicatas: E15, 20/20=7, L198, I124, U22. [app/docs/BACKLOG.md:1701; app/docs/BACKLOG.md:2104]

Duplicatas consolidadas:

- `VERIFICADOR-EM-FABLE`: 1785 + 1911 → uma INSTRUMENTO. [app/docs/BACKLOG.md:1785; app/docs/BACKLOG.md:1911]
- `BUG-PLANTIO-NOTURNO-MAIS-1`: 1852 + 1975 → uma ESPINHA-E; retiro a classificação L da linha antiga. [app/docs/BACKLOG.md:1852; app/docs/BACKLOG.md:1975]

Absorções explícitas ainda **não descontadas** dos 364:

- `FERIAS-SALDO-SEM-TRAVA` → `HJ-090`: descontar 1 L. [app/docs/BACKLOG.md:1873; app/docs/BACKLOG.md:1874]
- `BUG-PAR-ESTADO-EXPLICITO` → `BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS`: descontar 1 L. [app/docs/BACKLOG.md:2019; app/docs/BACKLOG.md:2018]
- `TIPO-NA-FONTE-LAMPADA-INSTANTE` → `TIPO-DNA-9-CHAVES`: descontar 1 L. [app/docs/BACKLOG.md:2087; app/docs/BACKLOG.md:2093]

Aplicando essas três: **361 obras; L194**, demais baldes iguais. Os registros `C20-O168` e `O168-GUARDA-NO-GATE-DO-PAI` estão individualizados nos sete de 20/20; ambos correspondem ao pouso O168 hoje, portanto consolidar com a O168 do outro lote exige retirar essas duas entradas desta contribuição. [app/docs/BACKLOG.md:2049; app/docs/BACKLOG.md:1942; anexo:relato_1311.md:9; anexo:relato_1311.md:11]

## 13/11 20:20 · sessao 0ebec7918415 · - · - · origem=Ronald (tmux)

Inventário enviado em três parcelas: **361 linhas incluídas** no intervalo `BACKLOG.md:1192–1700`, antes de consolidar aliases e duplicatas. Contagens: ESPINHA-E **7**, 20/20 **1**, LATERAL-ESTRUTURA **202**, INSTRUMENTO **100**, UI **37**, CEREJA **10**, `?` **4**. Somente O168 consta como NO AR hoje pelo anexo; faltam **360** nesse inventário. [app/docs/BACKLOG.md:1192; app/docs/BACKLOG.md:1700; anexo:relato_1311.md:9]

Atenções para a consolidação:

- **O50-DESLIGAR-ENCERRA-TUDO** declara “NAO e obra nova”: consolidar com O50; os complementos O50-CELULA-APOS-DESLIGAMENTO e O50-COMPLEMENTO-LAUDO-1311 apontam a mesma obra. [app/docs/BACKLOG.md:1332; app/docs/BACKLOG.md:1333; app/docs/BACKLOG.md:1334]
- **O59** foi absorvida pela UI-RESCISAO. [app/docs/BACKLOG.md:1343; app/docs/BACKLOG.md:1344]
- **O81** repete o ID de outra linha identificada pelo agente principal. [app/docs/BACKLOG.md:1383; nao verificado: outra ocorrência]
- **O139** declara PRODUTO NO AR em 08/10, mas também prevê selo em outro pouso e conserva “REGISTRADO, nada construido” na coluna seguinte: divergência interna, sem prova de encerramento integral. [app/docs/BACKLOG.md:1393] [nao verificado]
- **O221** declara três pousos no ar e conserva “EM FILA, nada juntado”: divergência interna. [app/docs/BACKLOG.md:1215]
- **O232** declara pousos A/B NO AR 09/10, mas incorpora complemento posterior sobre a ata: manter o residual explícito. [app/docs/BACKLOG.md:1196]
- **O218** afirma que a bateria não está fechada; “INSTRUMENTO NO AR” não encerra a obra inteira. [app/docs/BACKLOG.md:1214]
- **O169** nesta linha ainda espera lei; conferir o alias O169-FECHADA tratado em outro intervalo antes da contagem final. [app/docs/BACKLOG.md:1424; nao verificado: alias]
- **O275** permanece INTERROMPIDA pela gramática e pelo anexo. [app/docs/BACKLOG.md:1560; anexo:interrompidas.txt:19]
- Os IDs em `BACKLOG.md:1504` e `:1540` contêm nome e foram enviados raspados; recuperar identidade exclusivamente pela linha. [app/docs/BACKLOG.md:1504; app/docs/BACKLOG.md:1540]
- “CURADA”, “FEITA”, “POUSADA” e “CANCELADA” não provam, sozinhas, NO AR hoje. Os estados derivados enviados nesses casos precisam conservar a ressalva `[nao verificado]` quando a enumeração não representa o texto original. [app/docs/BACKLOG.md:1213; app/docs/BACKLOG.md:1507; app/docs/BACKLOG.md:1524; app/docs/BACKLOG.md:1700]
- **O228**, excluída na relação preliminar abaixo, diz NO AR inteiro, porém sem data do pouso. O pedido 08/10 não prova a idade do NO AR; se o corte temporal exigir data explícita, restaurar com essa lacuna declarada. O mesmo cuidado vale para encerramentos sem data de evento, sobretudo O36, O15–O18 e O286. [app/docs/BACKLOG.md:1201; app/docs/BACKLOG.md:1317; app/docs/BACKLOG.md:1294; app/docs/BACKLOG.md:1295; app/docs/BACKLOG.md:1296; app/docs/BACKLOG.md:1297; app/docs/BACKLOG.md:1572]

**148 exclusões preliminares**, em 509 linhas físicas, por FECHADA/NO AR antigo ou fechamento explícito em outra célula da própria linha; revisar a ressalva temporal acima antes de usar a contagem como final. [app/docs/BACKLOG.md:1192; app/docs/BACKLOG.md:1700]

```text
O219@1197 O223@1198 O224@1200 O228@1201 O220@1216 O211@1217
O126@1221 O130@1223 O119@1226 O120@1227 O124@1231 O118@1233
O116@1235 O115@1236 O114@1237 O113@1238 O112@1239 O111@1240
O110@1241 O109@1242 O107@1244 O106@1245 O104@1247 O103@1248
O102@1249 O101@1250 O100@1251 O97@1253 O1@1254 O2@1255
O3@1256 O4@1257 O5@1258 O6@1259 O7@1260 O8@1261 O11@1264
O12@1265 PONTUALIDADE-SO-NO-12X36@1266 HIGIENE-DE-CONTEXTO@1267
GESTAO-HE-CALENDARIO-CONTROLE@1268 ATALHO-HE-NA-CENTRAL@1270
L-084-VENCE-O-T8@1272 DOIS-JUIZES-DA-ENTRADA-LONGE@1273
PONTUALIDADE-EM-TURNO-ABERTO@1274 AVAIS-NA-MESA@1275
09-TURNO-ABERTO-EXPOSTA@1277 COL369-VINCULO-1296@1278
SUITE-UMA-VEZ-POR-ARVORE@1280 L-093-TRABALHADO-REAL@1281 [nao verificado]
UI-RESPOSTA-DIZ-O-QUE-E@1283 FILA-DA-NOITE@1286
COL369-FOLGA-NAO-DECLARADA@1289 COL369-TIPOS-INVERTIDOS@1290
JANELA-DA-AUTORIDADE-PERDE-O-DIA-20@1291 O13@1292 O14@1293
O15@1294 O16@1295 O17@1296 O18@1297 O20@1299 O21@1300
O22@1301 O23@1302 F1@1303 O25@1306 O26@1307 O29@1310 O36@1317
O41@1322 O56@1340 O69@1356 O78@1365 O80@1367 O91@1376 O84@1380
O131@1384 O134@1387 O135@1388 O154@1407 O158@1411 O186@1441
O191@1446 O195@1450 O197@1452 O199@1454 O200@1455 O204@1459
O206@1461 O207@1462 O209@1463 O231@1471 O247@1479 O253@1485
O259@1489 BUG-LINHA-X-BADGE@1491 BUG-SOBRA-TIPO-TROCADO@1492
O261@1494 BO-ATESTADO-409-MUDO@1500 HOOK-PAREI-COM-ANDAVEL@1514
ATA-NAO-DISPENSA-JUSTIFICADA@1515 CONCORDE-CLASSE-B@1516
SONDA-PARA-O-[redigido]@1519 ESPELHO-FERIADO-DOBRA@1531
UI-PILULAS-QUEBRAM-LINHA@1543 O262@1546 O263@1547 O263-FT@1548
O272@1557 O273@1558 O278@1563 O286@1572 O288@1574 O289@1575
O291@1577 O292@1578 O295@1581 O306@1592 O307@1593 O309@1595
O310@1596 O313@1599 O310-RAIOX@1605 O318@1613
REGUA-DO-PERIODO-VEM-DA-ESCALA-DE-HOJE@1615 O320@1623 O322@1624
BO-HE-RECUSAR-LOTE-500@1663 SEM-CELULA-11-DEVE-TER-CELULA@1664
PLACAR-R6-CONTRATOS-COPIA-VELHA@1668
JUIZ-CLASSIFICAR-POSICAO-NAO-POUSOU@1669 [nao verificado]
ITEM-3-TRANCA-SAI-PELA-L111@1671 REJULGAR-4-EXPORTADAS@1673
CENSO-DAS-DUAS-CASAS@1674 CASA-ESCALA-E1@1676 CASA-CHAMADO-C1@1677
PERGUNTA-RESPONDIDA-E-MARCO-ACESO@1678 CASA-ESCALA-E2@1679
CASA-ESCALA-E3@1681 REABRIR-PERGUNTA-A-PREMISSA@1686
FECHADOR-POR-BATIDA-X-ENTRADA-ADIANTADA@1688 RAIA20-REGRAVADOS@1693
ATA-X-TURNO@1695 ATA-X-TURNO-ENTRADA@1696 CONTRATO-19-DE-VOLTA@1697
FECHAR-PERGUNTA-A-PREMISSA@1698 R1-A-i-FILTRO-DATA-CIVIL@1699
```

Cada `@NNNN` acima significa `app/docs/BACKLOG.md:NNNN`. [app/docs/BACKLOG.md:1197]

## 13/11 20:18 · sessao df08623b5c4e · - · - · origem=Ronald (tmux)

Segundo lote completo; `@N` significa `app/docs/BACKLOG.md:N`. Campos omitidos: estado `REGISTRADA`, grade `.........`, resolve/dependências `-`. Baldes `E`=ESPINHA-E, `20`=20/20, `L`=LATERAL-ESTRUTURA, `I`=INSTRUMENTO, `U`=UI. [app/docs/BACKLOG.md:2076]

```text
MOTOR-LE-O-TURNO@1879|L|Motor usa par e dia oficiais|||deps FIO-E0-V4-E2-FUNCAO,FIO-E0-V4-E3-LEITORES
MOTOR-LE-O-TURNO-INSTRUMENTO@1880|I|Mede cálculos fora dos juízes||..+...++.
MOTOR-TURNO-INSTR-FILA@1881|I|Explica limites da medição|||deps MOTOR-LE-O-TURNO
DISJUNTOR-B3-TARDIO@1882|I|Detecta reprovação antes do pouso [nao verificado]
HOLERITE-ZIP-MATRICULA@1883|L|Importa holerite pela identidade autorizada
HOLERITE-CAMPO-MATCH-LIVRE@1884|L|Impede associação por campo livre|||deps HOLERITE-ZIP-MATRICULA
PLACAR-LIVENESS@1885|I|Mostra pendências estruturais reais
FIO-COM-ASSENTO-E0-V3@1886|I|Confere desenho contra código existente
FIO-E0-V4-E1-ENTRADAS@1888|E|Cada fato gera pergunta identificável|EM VOO|+.+~.....
BUG-DEPLOY-PORTAO-ANTES-DO-MIGRATE@1890|I|Confere publicação antes de migrar||..+...++. [nao verificado]
DEPLOY-RESPAWN-SEM-PORTAO@1891|I|Investiga reinício sem conferência [nao verificado]
ATOMICIDADE-DA-PORTA-DIVIDA@1892|I|Expõe violações de transação
SIGPIPE-PRINTF-CAT-GREP-Q@1893|I|Evita falha enganosa nos selos
E1-FILA-V2-3-A-6@1894|L|Completa proteção dos vínculos de pergunta
E1-FILA-OPUS6-FILA@1895|L|Investiga concorrência na troca de vínculo
E1-FILA-V2-2-OUTRAS-PORTAS@1897|L|Ordena travas das demais portas
E1-FILA-V2-2-RESIDUOS@1898|L|Explica efeito e custo da trava
DEPLOY-PORTAO-RESTOS-V6@1899|I|Completa conferência da publicação [nao verificado]
DEPLOY-EXCLUSAO-ENVELOPE@1900|I|Evita publicação durante rotina concorrente|||deps FIO-E0-V4-E2-FUNCAO [nao verificado]
FIO-E0-V4-E3-LEITORES@1902|20|Telas concordam sobre próxima ação|EM VOO|+.+~.~...
E3-MATRIZ-DA-RESPOSTA@1903|E|Centraliza registro e leitura da resposta|EM VOO|+........
E4B-D2-ANCORA-RAIOX@1904|L|Raio X usa mesma âncora
E4B-D3-ARQUIVAR-PRESCRITOS@1905|L|Identifica consequência da rotina desligada
E3-X1-FOLGA-NAO-E-SEM-COBERTURA@1906|L|Julga corretamente folga sem adesão
E3-V15-A-S@1907|L|Dá destino aos casos sem ação
E3-CADASTRO-ADMIN@1908|L|Encaminha cadastro inconsistente ao admin
E3-X2-NUNCA-JULGADAS@1909|L|Investiga células nunca julgadas
E3-COL962-C6-X-LAMPADA@1910|L|Confere pergunta contra lâmpada
VERIFICADOR-EM-FABLE@1911|I|Compara qualidade e custo da revisão|EM VOO|..+.+~...
RAIZ-DA-DISPUTA-F0F1@1912|L|Explica origem e vínculo das perguntas
ORDEM-ATE-O-ZERO@1913|L|Ordena entregas até fechar contratos
INTERMITENTE-SEM-CONVOCACAO@1914|L|Define previsão diante de convocação
FIO-E0-V4-E4-ESCRITORES@1915|E|Atos deixam uma trilha coerente|EM VOO|..~......
FIO-E0-V4-E4-RETIRADA-FISICA@1916|E|Remove campos legados após migração|||deps FIO-E0-V4-E4-ESCRITORES
FIO-E0-V4-FATO-A03@1917|E|Registra fato próprio da reabertura|||deps E4D
FIO-E0-V4-FATO-A05@1918|E|Registra fato próprio da assinatura|||deps E4D
FIO-E0-V4-FATO-A06@1919|E|Registra identidade da duplicata|||deps E4D
FIO-E0-V4-FATO-A17@1920|E|Registra pendência técnica da aplicação|||deps E4D
FIO-E0-V4-FATO-A18@1921|E|Registra recusa técnica da aplicação|||deps E4D
FIO-E0-V4-FATO-A20@1922|E|Registra conferência de data|||deps E4D
FIO-E0-V4-E4B@1924|20|Recupera perguntas sem caminho acionável||..+...++.
E4B-C12-DIFF-DE-PAUTA@1925|20|Confere pautas geradas por recusa|||deps FIO-E0-V4-E4B
E4B-FILA-OPUS-C14@1926|L|Completa vínculo e controle das pautas
BO-0611-05@1927|L|Permite gravar batida sem localização|PARADA|+.+~+~...
BO-DISPUTA-SEM-PERGUNTA@1928|L|Impede nascer disputa sem pergunta|PARADA|+.+~.~...
BO-DISPUTA-MATRIZ-ANTES-DO-OPUS@1929|L|Confere todos os nascimentos de disputa
MAPA-DA-FILA-BO@1930|I|Explica mecanismo dos problemas da fila||+........
BO-0611-05-COMPLEMENTO-POUSO@1931|I|Confere dinheiro e roteiro de publicação||+..+..... [nao verificado]
BO-0611-05-DESLIGADO-FERIAS@1932|L|Investiga batida nas férias e desligamento
BUG-PAR-FROTA-1110@1933|L|Mede pausas com metade pendente|||=complemento BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS
RAIZ-DA-DISPUTA@1934|L|Mantém disputa e perguntas coerentes
REGULARIZAR-TRAVA-PELO-DIA@1935|L|Libera regularização no dia correto|||deps RAIZ-DA-DISPUTA
PORTA-QUE-NAO-AGE@1936|I|Mede dias com ação inalcançável
SAIDA-NOTURNA-NO-DIA-CERTO@1937|L|Aplica saída ao plantão correto||+........
RESPOSTAS-1211-1230@1938|I|Consolida cortes e medidas pendentes
DECLARADO-X-GRAVADO@1939|I|Compara resposta declarada com batida|EM VOO
DECLARADO-X-GRAVADO-PASSIVO-11@1940|L|Corrige passivo da competência aberta|||deps DECLARADO-X-GRAVADO
E3-10-CURA-NO-ESCRITOR@1941|E|Impede resposta carimbada sem texto|EM VOO|~........
O168-GUARDA-NO-GATE-DO-PAI@1942|20|Preserva conversa que aguarda julgamento|NO AR hoje|+.+..++++|anexo relato_1311.md:9,11
E3-FORMA-SQL-DO-JUIZ@1943|20|Consulta resposta pela mesma regra|EM VOO|+.+~.~...
E3-IMPACTO-E-CONTRATOS-DE-ENTRADA@1944|E|Explica impacto da troca de leitores|EM VOO|~........
HOOK-B3-PAREI-LEGITIMO@1945|I|Reconhece espera legítima por liberação
F4-PAR-PELA-CELULA@1946|L|Pausa usa horários da célula||+........|deps RAIZ-DA-DISPUTA
RAIA-2-BO-VISIVEL@1947|L|Ordena correções visíveis ao admin
MAPA-ATE-O-20@1948|I|Mostra andamento das obras estruturais
GABARITO-FABLE-VERIFICADOR@1949|I|Compara revisões com casos conferidos|EM VOO
BO-ROTULO-ENCERRADO@1950|U|Distingue resolvido de fechado|EM VOO|..~..~...
SONDA3-METODO-CORRECAO@1951|I|Mede ação com perfil real
DISPUTA-ZUMBI-1110@1952|L|Encerra disputa quando regra permite
PERGUNTA-DUPLICATA-1110@1953|E|Mede perguntas repetidas na disputa|||deps E4D
AUDITORIA-TRAVAS-POS-RESET-PADRAO-NA@1954|I|Revisa custo e alcance do selo
BO-0611-05-RESTO-DA-CLASSE@1955|L|Investiga demais barreiras da batida
TRIAGEM-OFFLINE-SEM-CAMERA@1956|U|Permite fluxo sem câmera disponível
APP-MENSAGENS-MSG-0@1957|L|Justificativa se liga à cobrança correta|||deps TRIAGEM-OFFLINE-SEM-CAMERA
APP-MENSAGENS (MSG-1..4)@1958|U|Unifica mensagens e anexos no aplicativo|||deps E4C,RAIZ-DA-DISPUTA
GABARITO-FABLE-VIRA-SOMBRA@1959|I|Compara revisão sem bloquear entrega
GABARITO-FABLE-SOMBRA-AJUSTE@1960|I|Distingue perda de divergência de gravidade
DATA-EVENTO-CRU@1962|L|Migra leitura para dia do turno||+........
BATIDA-ID-DO-CLIQUE@1963|L|Impede duplicação do mesmo clique
MAPA-20-DERIVADO@1964|I|Deriva avanço de provas escritas|PARADA|..+..~...
BO-ROTULO-GLPI-SELETOR-MORTO@1965|U|Impede oferecer mudança sem efeito
BO-ROTULO-REABRIR-RECUSADO@1966|U|Oferece reabertura somente quando permitida
BO-ROTULO-TERCEIRO-VOCABULARIO@1967|U|Usa mesmos rótulos de estado
BO-ROTULO-FILA-DA-FORMA@1968|I|Completa alcance do selo de rótulos
TRIAGEM-OFFLINE-FILA@1969|U|Completa indicação de câmera e localização
PRONTO-AUSENTE-RASCUNHO@1970|I|Lista critérios escritos de conclusão
BO-0611-05-TEM-GPS-LEITORES@1971|L|Todos reconhecem localização pela mesma regra||+........|deps BO-0611-05
GPS-ESTADO-NA-TELA@1972|U|Mostra situação real da localização
ADMIN-FECHA-SEM-MOTIVO@1973|L|Exige motivo no encerramento crítico
BRIEF-COM-PROCEDENCIA@1974|I|Exige fonte das decisões do brief|||deps AUDITORIA-DE-TRAVAS
BUG-PLANTIO-NOTURNO-MAIS-1@1975|E|Grava saída noturna no dia correto
ABSOLVIDA-PELA-FOLGA@1976|E|Evita absolver pelo dia errado
LOTE-VALIDAR-REGRESSAO-0311@1977|L|Investiga demora ao validar lote
PREVIEW-REOFERECE-RECUSADA@1978|L|Evita oferecer novamente resposta recusada
BO-0611-07-E4B-COBRE@1979|20|Confere cobertura do passivo pelo reparo|||deps FIO-E0-V4-E4B
BO-0611-10-TELA-DA-TROCA@1980|U|Oferece troca pela tela|||deps FIO-E0-V4-E4-ESCRITORES
LAVRA-PREMATURA-CONCORDE@1981|L|Investiga julgamento antes do fim
HOOK-ESPERANDO-PID-MORTO@1982|I|Retoma quando processo esperado termina
PLACAR-FABRICA-MTIME@1983|I|Confere idade pela fonte correta
FIO-ESTADO-CARIMBO-VELHO@1984|I|Reconhece campos ausentes no carimbo
CARIMBO-DIA-CIVIL-X-TURNO@1985|L|Distingue dia civil de turno
FIO-E0-V4-VIGIA-FABRICA-POR-EVENTO-CRESCE@1986|L|Explica crescimento das perguntas não emitidas
E4A1-C6-X-PASSADO-SEM-TURNO@1988|L|Confere pergunta antiga sem turno
E4A2-RELOGIO-FIO-VIVO@1989|L|Julga fio com relógio recebido
PLACAR-PNA-ESPERADO-POR-DONO@1990|I|Separa pendências pelo responsável
O169-JUIZES-760@1991|L|Exige lastro correto do encerramento
REVERTER-LEITOR-JANELAS@1992|L|Reversão alcança leitura das janelas
REVERTER-SEM-SELECT-FOR-UPDATE@1993|L|Protege reversão contra concorrência
REVERTER-VOLTA-TRILHA-NA-PORTA@1994|L|Registra motivo completo da reversão
REVERTER-IDA-TRILHA-FRACA@1995|L|Fortalece prova do ato original
REVERTER-ANTES-LOG-ANTES-DO-JSON@1996|L|Evita trilha apontando reversão inexistente
O169-NAO-MEDIDO-ESCAPA@1997|I|Conta encerramento sem insumo medido
O169-RC2-ENGOLIDO@1998|I|Propaga alarme da verificação
O169-TESTE-ORM-RC2@1999|I|Prova alarme com banco de teste [nao verificado]
O169-DONO-VOCABULARIO@2000|L|Usa responsáveis do vocabulário autorizado
O169-CRONS-MD-ALARME@2001|I|Documenta rotina como alarme
O169-V5-GRAVAR-ESTADO-SEM-TRILHA@2002|L|Toda mudança de estado deixa trilha
O169-V5-SEM-TESTE-CONCORRENTE@2003|I|Prova comportamento de aplicadores concorrentes [nao verificado]
PERGUNTA-VIVA-COM-CELULA-ABSOLVEU@2004|L|Concilia pergunta com absolvição registrada
PLACAR-A5-RC2-SEM-ALARME@2005|I|Faz alarme chegar ao placar
PLACAR-FABRICA-UNIDADE@2006|I|Distingue perguntas de pautas no total
E4A2-JANELA-POR-EMPRESA@2007|L|Respeita competência própria da empresa
E4A2-LIMBO-EXPORTADO-COMO-ALVO@2008|L|Evita alarme de passivo já encaminhado
E4A2-SELO-6G-SUBSTRING@2009|I|Impede conferência por trecho enganoso
O169-FECHADA@2010|L|Revê encerramentos sem lastro comprovado|PARADA
SELO-MENSAGEM-NO-COMMIT-MSG@2011|I|Confere mensagem antes de consolidar|||deps FIO-E0-V4-E4-ESCRITORES [nao verificado]
PAUTA-SEM-TURNO-PREFIXO-RESERVADO@2012|L|Reserva identidade da pauta automática|PARADA
PAUTA-PREFIXO-IDENTIDADE-NO-TEXTO@2013|L|Separa identidade da pauta e texto|PARADA
PASSADO-SEM-TURNO-SEM-PAUTA-NAO-CONTADO@2014|L|Conta dia sem turno e sem pauta
REVERTER-DIA-SEM-RECALCULO@2015|L|Reversão atualiza pagamento do dia||..+...++.
E3-ENTRADA-ORFA-SEM-TURNO@2016|L|Define ação diante de entrada órfã|PARADA
ACHADO-POR-CASA-TOKEN-EM-PROSA@2017|I|Evita exigir autor por citação textual
BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS@2018|L|Aplica saída e volta juntas|EM VOO|+.+~+~...
BUG-PAR-ESTADO-EXPLICITO@2019|L|Explica espera pela outra metade|||=absorvida BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS
SONDA-SEM-GRANT@2020|I|Consulta produção sem alterar permissões|EM VOO|+........
SONDA-MATRIZ-DE-TRANSICOES@2021|I|Confere estados e permissões da consulta||+........|deps CODEX-2-RAIAS-PORTAO-CPU
AUDITORIA-0911@2022|I|Classifica causas das pendências medidas|||deps SONDA-SEM-GRANT
SEGREDO-ARGV-RESIDUO@2023|I|Retira segredo dos argumentos de processo|||deps SONDA-SEM-GRANT
SOMBRA-CARIMBO-MENTE@2024|I|Invalida certificado quando sombra muda|||deps SONDA-SEM-GRANT
SOMBRA-HOJE-NA-BORDA@2025|I|Mede dia correto após espera
PLACAR-RETIDO-DO-LASTRO@2026|I|Conta retenção do lastro corretamente
CODEX-2-RAIAS-PORTAO-CPU@2027|I|Abre segunda construção conforme capacidade|EM VOO
CODEX-ALIVIA-1-WRAPPER-BIN@2028|I|Permite construção preservando arquivos protegidos|||=complemento CODEX-2-RAIAS-PORTAO-CPU
CODEX-ALIVIA-3-ESMERIL@2029|I|Entrega auditoria técnica por família|||deps CODEX-ALIVIA-1-WRAPPER-BIN
PECAS-APOSENTAVEIS-NO-ESTADO@2030|I|Mostra código sem uso de produção|||deps CODEX-2-RAIAS-PORTAO-CPU
PLANTIO-SILENCIO-SEM-DIA@2031|L|Dá destino ao fato sem dia|PARADA||deps BUG-PLANTIO-NOTURNO-MAIS-1
PLANTIO-DIA-CIVIL-VIZINHO@2032|L|Usa dia do turno no plantio|PARADA||deps BUG-PLANTIO-NOTURNO-MAIS-1
PWA-FILA-OFFLINE-400@2033|L|Preserva batida recusada ou sem envio
CXR-NA-FILA@2034|I|Confere cobertura das classes estruturais
G6-PAGINA@2035|U|Mantém página após ação do admin
O10-TROCA-DE-PLANTAO-ENTRA@2036|L|Troca de plantão atualiza células||..+...++.
O10-B-FILA@2037|L|Protege troca e leitura de exportadas
O10-D-PERMUTA-RECIPROCA@2038|L|Permuta plantões de dois colaboradores
RETRATO-POR-EVENTO@2039|L|Mudança atualiza retrato da competência|||deps FIO-E0-V4-E2-FUNCAO,FIO-E0-V4-E3-LEITORES
AKITA-MERGE-SEM-LINHA@2041|I|Confere merge sem declaração
BO-0511-G2-TRIAGEM@2042|I|Organiza problemas por causa||+........
E-SEM-VIVA-EMPRESA-1@2043|I|Confere presença de empresa de teste [nao verificado]
E-SEM-VIVA-CACA@2044|I|Investiga dias sem pergunta viva||+........
G1-PORTA@2046|I|Confere caminho dos dias retentores||+........
H12H06-B-FILA-OPUS@2047|L|Completa limites e controles da correção
G3G4-ESTRUTURA-RESIDUO@2048|L|Endereça dias estruturais sem obra
C20-O168@2049|20|Preserva resposta ainda sem julgamento|NO AR hoje|+.+..++++|anexo relato_1311.md:9,11
F3-BECO-2-COBRANCA-MORTA@2051|L|Dá destino à cobrança morta
C6-IDENTITY-MAP@2052|L|Reduz demora ao montar fila
CRONS-MARCA-RELOGIO@2053|I|Passa data correta à rotina
CRIVO-AINDA-VALE@2054|I|Confere promessa antiga contra código
CODEX-FUNIL-28@2055|I|Filtra achados por impacto comprovado
FUNIL-28-PROCEDENTES@2056|I|Organiza achados conferidos sem construção
G3G4-DONO-MEDIDO@2057|I|Classifica responsável pelos casos||+........
CODEX-RED-POR-EXEMPLO@2058|I|Escreve casos da regra antes do código|EM VOO
F-346-FILA-OPUS@2059|L|Completa remarcação e trilha das férias
CODEX-PRE-CODIGO-DUPLO@2060|I|Compara duas leituras antes da construção
JV1-MERGE-FILA@2061|L|Completa provas das janelas de feriado
MARCO-DE-CLEAR-NAO-GRAVOU@2062|I|Registra reinício real do contexto|EM VOO|+........
TOAST-B-SELO-FINO@2063|I|Testa limites das mensagens de erro
SELOS-HOST-VERMELHOS-NO-MAIN@2065|I|Mede selos vermelhos na origem
E4-8-FORA-VINCULO@2066|L|Investiga cobrança fora do vínculo
E4-8-COMP-ANTIGA@2067|L|Confere cobrança em competências antigas
E4-8-FORMA@2068|L|Unifica nomes e declaração de origem
E4-8-ROTULO-OK@2069|L|Rótulo descreve julgamento realmente feito
FUROS-EMPRESA-TIMEOUT@2070|L|Evita demora excessiva no relatório
TOAST-SELO-APELIDO@2071|I|Confere selo de nomes repetidos
OTIMIZA-DUPLA@2072|I|Reduz custo da conferência e construção
OTIMIZA-2-WRAPPER-CODEX@2073|I|Melhora execução e retorno da construção|PARADA
O261-E-F3-DORMENTE@2074|L|Dá destino a perguntas presas
CORTES-JSON-DEFASADO@2075|I|Preserva cortes escritos ao regenerar
CONGELA-O-20@2076|I|Limita mapa à lista autorizada
CICLO-C-CEL-TEMPLATE@2077|L|Investiga travas concorrentes do cadastro
FAIXA-NO-CADASTRO@2078|L|Cadastro declara precedência da ausência
EFEITO-SEM-FALLBACK-CATALOGO@2079|L|Evita voltar silenciosamente à regra antiga
LASTRO-FILA-PREVERIF-C11@2080|L|Expõe recusas e falhas antes ocultas
BO-ROTULO-REABRIR-SO-NO-DETALHE@2081|U|Oferece reabertura no detalhe correto
PROPORCAO-FILA-OPUS-V11@2082|I|Completa alcance da conferência proporcional|||deps AUDITORIA-DE-TRAVAS
O261-PAI-TERMINAL@2083|L|Preserva referência e encaminha residual|PARADA||deps O261-E-PAI-TERMINAL-RESIDUAL;trava DEMOLIR desenho vedado
V3-E1@2084|I|Certifica cálculo novo em cenários|INTERROMPIDA||deps MOTIVO-NAO-SEI;anexo interrompidas:27-28
O10-C@2085|L|Troca histórica usa porta da célula|INTERROMPIDA||deps SOLO-LISTA-DO-ADMIN;anexo interrompidas:29-30
OTIMIZA-1@2086|I|Mostra custo de testes por entrega|INTERROMPIDA|+........|deps SUITE-POR-IMPACTO;anexo interrompidas:31-32;brief OTIMIZA-1 pronto [nao verificado]
TIPO-NA-FONTE-LAMPADA-INSTANTE@2087|L|Grava instante da lâmpada com fuso|||=absorvida TIPO-DNA-9-CHAVES
RELOGIO-NA-BORDA@2088|L|Usa mesmo instante em todo ato
RELOGIO-NA-BORDA-OPUS-C5E@2089|L|Completa relógio e transação do cadastro
VIGIA-PAREADOR-DIA-CIVIL@2090|L|Vigia usa dia do turno
TIPO-ATA-12-CHAVES-A@2091|L|Leitura e escrita passam pelo acessor|||deps TIPO-NA-FONTE,BUG-CADASTRO-c5e
TIPO-ATA-12-CHAVES-B@2092|L|Horário do fato ganha instante com fuso|||deps TIPO-ATA-12-CHAVES-A
TIPO-DNA-9-CHAVES@2093|L|Cadastro gravado usa formato uniforme|||deps TIPO-ATA-12-CHAVES-B
ACESA-ORFA-PROXIMIDADE@2094|L|Confere lâmpada acesa por aproximação|||deps TIPO-NA-FONTE
TRANCA-VIVAS-08-FILA@2095|L|Completa trilha e medida da tranca
NAO-LAVRADA-PASSADO@2096|L|Dá destino a dias sem ata
E3-PENDENTE-NAO-BOOL@2097|L|Exige pendência com tipo definido|||deps TIPO-ATA-12-CHAVES-A
SOMBRA-COM-TETO@2098|I|Limita memória da conferência
SETNULL-ORFA-DNA@2099|L|Preserva coerência ao apagar vínculo
TIPO-ITEM3-CONTRACT@2100|L|Exige formato certificado após migração|||deps TIPO-NA-FONTE
SOMBRA-LEITURA-V3@2101|I|Permite leituras concorrentes controladas
TIPO-V5-NAO-BLOQUEANTES@2102|L|Completa relógio e dia usados pelo vigia
CONTRATO-DE-ENTRADA-DO-FIO@2103|L|Foto retroativa identifica episódio correto|BRIEF PRONTO|+........|resolve HJ-095;deps FIO-E0-V4-E3-LEITORES,LEI-DESLIGADO-NAO-TRAVA,FERIADO-USA-A-LEI-DE-1609;anexo briefs.md:1-24
CODEX-CENSO-CONTRATOS-DE-ENTRADA@2104|I|Identifica desencontros entre produtor e leitor||+........
```

Exclusões: **37** linhas com fechamento/publicação antiga, antes da consolidação dos IDs. [app/docs/BACKLOG.md:1701]

```text
COL174-FECHA-AO-NASCER@1701 — FECHADA26/10
CELULA-CONCORDE-NO-MEIO-DO-TURNO@1702 — FECHADA26/10
HE100-SEM-PREVISTO-SOZINHA@1705 — FECHADA/NO AR27/10
ABONADO-COM-SAIDA-ANTECIPADA@1706 — FECHADA/NO AR27/10
HE100-32-DIA-DE-TRABALHO@1731 — FECHADA26/10
FOLGA-DA-CELULA-X-DIA-DO-MOTOR@1732 — FECHADA27/10
DIA-DA-DEMISSAO-FORA-DO-UNIVERSO@1735 — FECHADA26/10
FOTO-X-VIVO@1742 — FECHADA26/10
R1-FLIP-NAO-VIROU@1747 — FECHADA26/10
FECHAMENTO-REFERENCIA-POR-POSICAO@1753 — FECHADA26/10
CRON-NAO-CABE-SOMBRA-65-MIN@1761 — FECHADA25/10
BO-GESTAO-HE-ACESSO@1764 — NO AR25/10
TOAST-E-NOME-CALENDARIO@1778 — FECHADA/NO AR04/11
PEP-REENTRADA@1791 — NO AR06/11
OTIMIZA-3-LOADER@1793 — FECHADA07/11 sem pouso [nao verificado]
FIO-MORTO-POR-DONO-0611@1794 — FECHADA06/11
CAUSA-D-ESTRUTURA@1795 — FECHADA06/11
R1-REMEDE@1796 — FECHADA06/11
CAUSA-D-MATERIAL@1799 — FECHADA06/11
CONTRATO-20-CENSO@1817 — FECHADA05/11
MEDE-CODE-POR-PAPEL@1818 — FECHADA05/11
C20-O169@1821 — FECHADA05/11
F-346@1826 — NO AR05/11
C20-JUIZ-3@1843 — FECHADA/NO AR05/11
DUAS-TELAS@1871 — FECHADA06/11
FERIAS-CANCELAR-2X@1872 — NO AR06/11
FIO-COM-ASSENTO-E0-V4-PERGUNTA@1887 — FECHADA06/11
FIO-E0-V4-E1-FILA@1889 — NO AR09/11
E1-FILA-V2-2-TRAVA@1896 — NO AR09/11
FIO-E0-V4-E2-FUNCAO@1901 — NO AR09/11
FIO-E0-V4-E4A-2@1923 — NO AR10/11
PROPORCAO-POR-RISCO@1961 — FECHADA/NO AR11/11
MODO-COMUM-PASSADO-SEM-TURNO@1987 — FECHADA09/11
AKITA-MERGE-DE-VOLTA@2040 — FECHADA06/11
E-SEM-VIVA-B@2045 — FECHADA06/11, parcela desligado superada13/11
F3-BECO-1-VOLTA-3@2050 — NO AR05/11
BUG-SW-RESPOND-UNDEFINED@2064 — NO AR05/11
```

**Contagem:** 404 linhas tabulares no intervalo, 37 excluídas, 367 incluídas antes de consolidar duplicatas/aliases; retirando também E4B publicado em 12/11 08:46, são 38/366. O complemento novo SC-A25 deve permanecer registrado na obra E4B. [app/docs/BACKLOG.md:1924; anexo:relato_1311.md:20] [nao verificado]

**Consolidação e divergências:**

- `VERIFICADOR-EM-FABLE` é o mesmo ID em duas linhas; consolidar fontes 1785 e 1911. `BUG-PLANTIO-NOTURNO-MAIS-1` repete nas linhas 1852 e 1975. [app/docs/BACKLOG.md:1785; app/docs/BACKLOG.md:1911; app/docs/BACKLOG.md:1852; app/docs/BACKLOG.md:1975]
- `C20-O168` e `O168-GUARDA-NO-GATE-DO-PAI` são fases da O168; o estado antigo EM CONSTRUÇÃO no BACKLOG é superado pelo pouso/publicação hoje no RELATO. [app/docs/BACKLOG.md:2049; app/docs/BACKLOG.md:1942; anexo:relato_1311.md:9; anexo:relato_1311.md:11]
- E3, E3-FORMA-SQL e matriz da resposta são complementos; evitar contá-los como entregas independentes se a unidade adotada for a obra principal E3. O BACKLOG registra a relação de complemento explicitamente. [app/docs/BACKLOG.md:1903; app/docs/BACKLOG.md:1943]
- `OTIMIZA-1` tem brief pronto e continua interrompida no medidor; `JUSTIFICATIVA-PAGA-O-PERIODO` voltou à ordem viva no BACKLOG, mas continua interrompida no medidor. Preservar divergência sem inventar retomada. [app/docs/BACKLOG.md:2086; app/docs/BACKLOG.md:1875; anexo:interrompidas.txt:25; anexo:interrompidas.txt:31; anexo:brief

[truncado em 20000 de 20552 caracteres]

## 13/11 20:18 · sessao 37a98bab2c73 · - · - · origem=Ronald (tmux)

Legenda: `E=ESPINHA-E; L=LATERAL-ESTRUTURA; I=INSTRUMENTO; U=UI; C=CEREJA; ?=balde não declarado`; estado omitido = REGISTRADA; `V=EM VOO; B=BRIEF PRONTO; X=INTERROMPIDA; P=PARADA; A=NO AR hoje`. Grade omitida = `.........`. Cada `@N` aponta `app/docs/BACKLOG.md:N`. Em `?`, o motivo é ausência de classificação expressa suficiente na linha; não inferir prioridade só pelo caminho do arquivo.

```text
FERIADO-ESCALA@728|L|Feriado segue regra da escala|V|resolve HJ-074
FERIADO-1609-FILA@729|L|Corrigir efeitos laterais de feriado|dep FERIADO-ESCALA
LEI-DESLIGADO-NAO-TRAVA@730|L|Desligado deixa de travar folha|A|+++++++++|ver relato_1311:61,73,76,105,110
DESLIGAR-LIQUIDA-TUDO@731|L|Desligar encerra cobranças e respostas|V|+........|dep LEI-DESLIGADO-NAO-TRAVA,O50-DESLIGAR-ENCERRA-TUDO
DESLIGADOS-RETIDOS-CONTADOR@732|L|Mostrar desligados que retêm folha|V|..+......|dep LEI-DESLIGADO-NAO-TRAVA
PARIDADE-ABSOLVIDAS-168@733|L|Devolver respostas antigas ao admin|V|+........|dep BUG-PAR-DESCONGELA
RAIOX-DIAS-DERIVA@734|L|Raio-X lê dias do juiz|dep LEI-DESLIGADO-NAO-TRAVA
SITUACAO-LEITOR-CRU@735|L|Telas consultam situação única|dep O50-DESLIGAR-ENCERRA-TUDO
DESLIGADOS-RETIDOS-UNIVERSO@736|?|Contador representa todos os retidos|dep LEI-DESLIGADO-NAO-TRAVA
RETRATO-DOIS-RELOGIOS@737|L|Retrato usa um instante único|dep LEI-DESLIGADO-NAO-TRAVA
OTIMIZA-1-COMPLEMENTO-CARGA@738|I|Vigia usa menos memória|dep E3-LEITORES,OTIMIZA-1
RAIA-PREPARACAO@739|I|Preparar obras antes da construção|V|+........|ver relato_1311:16,25,28,29,30,31,33
BO-[nome]-1311-ROTEIRO@740|I|Admin recebe roteiro dos dez casos|V|+........|resolve casos 01–10
HJ-092@741|L|Admin recebe batidas para corrigir|resolve HJ-092
TIPO-DE-BATIDA-DE-PERGUNTA-SEM-AVISO@742|U|Avisar quando tipo da batida muda|resolve HJ-092
HJ-093@743|L|Pagar período coberto por justificativa|+........|resolve HJ-093|dep JUSTIFICATIVA-PAGA-O-PERIODO,O168
API-RETIDOS-ROTULO@744|U|Distinguir retido de fora da folha
LANCAR-AUSENCIA-AFASTA-DESLIGADO@745|L|Ausência preserva situação do desligado
FERIADO-RUBRICA-DO-FLAG-CORRENTE@746|L|Feriado usa flag da época|dep FERIADO-ESCALA
SELOS-CANON-I1-FILA@748|I|Corrigir falhas do censo obrigatório
SELOS-CANON-I3@749|I|Etapas precisam de resultado observável|B|+........
SELOS-CANON-I2@750|I|Aval declara universo dos dados|P|dep SELOS-CANON-I3
PERGUNTA-SEM-SELO@751|L|Eliminar decisões fora das autoridades|V|dep FECHAR-PERGUNTA-A-PREMISSA
PILHA-DA-BATIDA-RECALCULO-INLINE@753|L|Reduzir demora para registrar batida|+........|dep O-OUTBOX
O-PROJECOES@756|E|Telas leem projeções reconstruíveis|dep REJULGAR-COMP10-RESTO
ADR-NAS-LEIS@757|I|Leis explicam escolhas e consequências
O-BITEMPORAL-2@758|L|Consultar cadastro vigente em cada época|dep PERGUNTA-SEM-SELO
IA-DOMINIO-POR-FERRAMENTA@759|L|Assistente respeita domínio permitido
RECUSA-DO-ADMIN-DIZ-O-QUE-FAZER@760|U|Recusa explica fatos e ações
TELA-6X1-NAO-MOSTRA-HORARIOS-POR-DIA@761|U|Editar horários diferentes por dia
JORNADA-EXCEPCIONAL-POR-DATA@762|C|Cadastrar jornada excepcional por data|dep TROCA-DE-PLANTAO
LISTA-DE-QUARENTENA@763|U|Admin consulta e levanta quarentenas|dep QUARENTENA-DO-COLAB
O148-EDITAR@764|U|Editar calendário preservando seu passado|P|dep O148,O152
LAUDO-[nome]-COL94-1610@765|I|Medir inconsistências de batidas|V
REJULGAR-COMP10-4-REVERTIDOS@766|L|Rejulgar dias revertidos com segurança|V|+......+.
SOMBRA-X-PROD-COL988-124639@767|I|Explicar diferença entre ensaio e produção
SOMBRA-NAO-CABE-ANTES-DO-VIZINHO@769|I|Ensaio cabe na janela disponível|V|.......+.|dep SOMBRA-PORTAO-POR-COMANDO
CRON-NAO-CABE-SOMBRA-87-MIN@770|I|Agenda identifica ensaio demorado|P
TROCA-DE-POSTO-SEM-VINCULO@771|L|Troca de posto acompanha vínculo
N1-ALIMENTACAO-UNICA@774|L|Evitar carregar perguntas duas vezes
SOMBRA-PORTAO-POR-COMANDO@775|I|Deploy confere ensaio dos comandos [nao verificado]
REGISTRADO-INSTRUMENTO@781|I|Contagem de pendências segue significado|V|...+...+.
SILENCIO-VENCIDO-VOLTA@783|L|Cobrança volta quando silêncio vence|X|..+......|dep QUARENTENA-DO-COLAB,E3-LEITORES
QUARENTENA-DO-COLAB@784|L|Isolar cobranças por colaborador e período|X|...+.....|dep E3-LEITORES
ISENTO-EMISSOR-NAO-PERGUNTA@785|L|Isento tem cobranças isoladas|dep QUARENTENA-DO-COLAB
ATA-DEFASADA-COMP09@787|L|Identificar atas antigas divergentes
CLAUDE-MD-LOOKUP-2-4b-6@788|I|Encontrar instruções em referências menores
GRADE-POS-POUSO@789|L|Conciliar versões da grade [nao verificado]
COBPARC-CONTROLE@790|I|Separar efeito da cura e cadastro
COBPARC-ATA-DEFASADA-09@791|L|Explicar diferenças antigas da ata
COBPARC-AUSENCIA-CALA-DOIS-JUIZES@792|L|Ausência parcial segue um juiz|dep COBERTURA-PARCIAL-UM-JUIZ
COBPARC-FUROS-LEITOR-PARCIAL@793|L|Mostrar furos restantes da ausência|dep COBERTURA-PARCIAL-UM-JUIZ
FECHAR3-SOBREVIVENTE-NO-RECONCILIAR@797|L|Conferir cobrança sobrevivente na reconciliação
CXR-PRONTO1-COL841@799|L|Lista acompanha pausa realmente cumprida|dep ATA-X-TURNO-PAUSA
E1-ACERVO-5-GRUPOS-09@800|E|Tratar acervo de competência exportada
REJULGAR-61-COBPARC@801|L|Atualizar atas antigas de cobertura
RETRATADA-CONVERSA-E-REABRIR@802|L|Evitar perguntas sobre batida retratada|dep E1
E1-A2-VAZIO-MEDIR@803|E|Medir episódios sem dia alcançável|dep E1
E1-RECUSAS-SEM-CONTADOR@804|E|Mostrar recusas sem origem válida|dep E1
E1-DUPLICATA-SOBRE-RETRATADA@805|E|Tratar duplicatas de episódio retratado|dep E1,E3-LEITORES
CXR-PRONTO5-DEFINICAO@806|L|Definir saída correta da lista
CXR-MEDIDO-EM-AUTO-NOW@807|?|Publicar hora real da medição
CXR-BECO-SEM-BATIDA-FORA-DA-LAVRA@808|?|Incluir colaboradores sem batida na medição
TETO-NA-LISTA-DOIS-RELOGIOS@809|L|Lista usa um instante único
CODEX-ESTADO-NO-RELATO@810|I|Consultar respostas finais publicadas|V|...+...+. [nao verificado]
TRAVA-COLAB-X-DISPUTA@811|L|Evitar travamento entre colaborador e disputa
MOTOR-PASSA-O-TURNO-A-VOLTA@813|L|Pausa usa turno já reconhecido|dep JUIZ-DO-PAR-GRUPO-1
JUSTIFICATIVA-ADMIN-FORA-DA-PORTA@815|L|Admin altera justificativa pela porta
O261-COMPLEMENTO-[nome]@816|L|Corrigir perguntas sem aplicação comprovada|V|..+......
REATAR-ESCREVE-EM-DIA-TRANCADO@817|L|Reatar respeita dia já trancado
C6-TETO-MARCO-PENDENTE@818|L|Cobrança distingue marco ainda pendente
PASSIVO-VALIDADO-K6@819|L|Passivo consulta competência no juiz
PROGRAMA-CELULA-E-O-DIA@821|E|Dia tem resposta única nas telas|V
PROGRAMA-CELULA-E-O-DIA-DUAS-PISTAS@822|E|Status e cálculo avançam com prioridade|V|dep PROGRAMA-CELULA-E-O-DIA
E4-CAL-RETIDO-FOLHA-NO-LEITOR@823|E|Calendário lê retenção da folha|dep E4-CALENDARIO-L1
E4-CAL-FALLBACK-STATUS-ROTULO@824|E|Calendário usa status e rótulo oficiais|dep E4-CALENDARIO-L1
E4-CAL-DOIS-CONTRATOS-DE-CELULAS@825|E|Calendário recebe células no formato correto|dep E4-CALENDARIO-L1
E4-INCLUI-CONTRATO-20@826|E|Leitores e crons seguem autoridade única
GRADE-FOLGA-DO-VINCULO@827|E|Folga de calendário respeita vínculo|V
GRADE-LEITURA-DA-EXPORTADA@828|E|Admin consulta ata da exportada|dep GRADE-FOLGA-DO-VINCULO
ODOMETRO-DE-REGRAS@829|I|Medir crescimento das instruções|dep CLAUDE-MD-RACIONAL
REPROVA-E-DESENHO@830|I|Reprovação exige decisão antes de seguir|V
REPROVA-E-DESENHO-CODEX@831|I|Conter repetição de verificações|dep REPROVA-E-DESENHO
TRAVA-MENOS-FILA@832|I|Reduzir espera dos testes [nao verificado]
TRAVA-COBERTURA@834|I|Jobs mantêm dono e volta|P|dep COPIA-DO-HEAD-MECANISMO
ADR-0003-CELULA-TRES-NATUREZAS@835|I|Documentar natureza das informações do dia|V
CORRECAO-ADR-0003@836|I|Distinguir célula viva de fotografia
PADRAO-DE-CIMA@837|I|Repetição de desenho pede decisão|X
DISJUNTOR-B1-ESPERA-OCIOSA@838|I|Evitar agente esperando sem trabalho|dep PADRAO-DE-CIMA
LEI-AKITA-RESIDUO@839|I|Revisar falhas do selo de commits [nao verificado]
B3-CODEX-VOLTA-NUNCA-TRIADA@840|I|Detectar verificação sem triagem
GRADE-V5-SEM-PORTAO@844|L|Folga correta vale em qualquer competência|V|..+......
GRADE-FOLGA-JUIZ@845|L|Folga lançada tem autoridade única|V
FASE-CONFLITANTE-VINCULO-DO-DIA@846|L|Conflito usa vínculo de cada dia|+........
E4-7-D4-ABERTO-SEM-TURNO@848|E|Explicar dia aberto sem turno
B3-RAIZ-DO-WRAPPER@849|I|Compartilhar estado correto do verificador
B3-CHAVE-DA-OBRA-NOS-DOIS-CONTADORES@850|I|Contadores reconhecem a mesma obra
B3-NAO-SEI-SEM-SAIDA@851|I|Dar saída para triagem ilegível
B3-CODEX-PREEXISTENTES@853|I|Corrigir brechas antigas do disjuntor
CODEX-CUSTO-POR-OBRA@854|I|Mostrar custo de cada obra
E4-CAL-DIA-COM-FIM-LE-A-FOLHA@855|E|Calendário lê folha do dia passado|dep E4-CALENDARIO-L1
PARCIAL-NA-PORTA@857|L|Cobertura parcial entra pela porta|dep E4-CALENDARIO-L1
COBPARC-ROTULO-FILA-V2@859|L|Dispensa tem rótulo e escritor únicos
COBPARC-PORTAO-PARCIAL-JUIZ-UNICO@860|L|Cobertura parcial decide numa autoridade
COBPARC-FILA-RELOGIO@861|L|Julgamento compartilha um instante único|X|..+......
COBPARC-FILA-MATERIALIZACAO@862|L|Materialização lê cobertura da ata|dep COBERTURA-PARCIAL-UM-JUIZ
COBPARC-FILA-CENSO-V7@863|L|Corrigir decisões paralelas de cobertura|dep COBERTURA-PARCIAL-UM-JUIZ
COBPARC-FILA-V8-PARCIAL-DEMISSAO@864|L|Conciliar parcial no dia da demissão|dep COBERTURA-PARCIAL-UM-JUIZ
COBPARC-FILA-V8-VOCABULARIO@865|L|Justificativa mantém significado nos leitores|dep COBERTURA-PARCIAL-UM-JUIZ
COBPARC-FILA-V8-PARCIAL-JUSTIF@866|L|Cobrar restante não explicado do dia|P|dep COBPARC-FILA-RELOGIO
COBPARC-FILA-H10-PARCIAIS-09@867|L|Preservar parciais da competência exportada
COBPARC-FILA-H10-COL926@868|L|DP recebe caso de batida
E4-CALENDARIO-QUEM-PUBLICA-EH-TRABALHO@869|E|Calendário lê natureza oficial do dia|V
D5-VIA-NULA@870|E|Via ausente não permite lavra indevida
E4CAL-P1-REVERTER-AMPLO@871|I|Reversão cobre todos os efeitos
E4CAL-P1-PAREI-FM-CORRENTE@872|I|Ensaio distingue atualização legítima
E4CAL-P1-SELO-CAMPOS-ACORDAM@873|E|Provar campos que disparam atualização
E4CAL-P1-ROTULO-SEM-CERTIDAO@874|E|Explicar rótulo antigo após status novo
E4CAL-P3-ULTIMO-RECURSO-ENGOLE@875|E|Falha de calendário deixa evidência
E4CAL-P3-CENSO-HOJE-PORTA-CURTA@876|E|Censo usa mesma data na porta
E4CAL-P3-LEITURA-ESCREVE@877|E|Calendário não falha ao materializar
E4CAL-P3-DOCS-VELHAS@879|I|Atualizar documentação do calendário
PAREADOR-JANELA-ORFA@880|L|Mesmo turno independe da janela
E4CAL-CARD-LE-NUCLEO@881|E|Cards leem status do núcleo
STATUS-ESCALA-SUSPEITA-VOCAB@882|E|Rótulo distingue batida de ausência
A2-RAISE-NO-FUROS@883|E|Tela de furos trata recusa
TT6-PENDENTE-VELHO@884|E|Atualizar pendência já alterada
E4-APP-MAPA@885|E|App lê status lavrado do dia|dep E4-CALENDARIO
DISJUNTOR-TETO-DA-OBRA@886|I|Conter excesso de voltas|dep CONTENCAO-QUE-MORDE-V2
CONTENCAO-QUE-MORDE@887|I|Conter repetição sem resultado|dep CONTENCAO-QUE-MORDE-V2
CONTENCAO-6-MODELO-PELO-PAPEL@888|I|Escolher modelo conforme tarefa|dep CONTENCAO-QUE-MORDE-V2
CONTENCAO-QUE-MORDE-V2@889|I|Conter trabalho sem prova suficiente|P
CONTENCAO-V2-HORA-DE-LINHA@890|I|Publicação não antecipa hora dos fatos|dep CONTENCAO-QUE-MORDE-V2
V3-LAVRAR-ATRASO-CREDITO@892|C|Atraso preserva crédito já descontado
O261-E-PAI-TERMINAL-RESIDUAL@894|L|Pergunta conserva âncora viva|dep O261-E
O261-E-F5B-JUIZ-PORTA-ALCANCAVEL@895|L|Cobrança tem ação realmente acessível|dep O261-E
O261-E-F5C-PERGUNTA-SEM-ACAO-POSSIVEL@896|L|Identificar pergunta sem ação possível
O261-E-F5C-FILA-DO-OPUS@897|L|Corrigir sobras da porta de cobrança
E1-PERGUNTA-AO-DP-CONTADOR@898|E|DP esclarece cenários de intervalo
E1-STATUS-VERMELHOS@899|E|Corrigir cenários de status divergente
V-FASE-AS-OF-TRANSFERIDO@900|L|Fase passada respeita empresa da época
V-FASE-AS-OF-PARIDADE-UNIVERSO@901|L|Comparar fase no mesmo universo
V-FASE-AS-OF-CUSTO@902|L|Reduzir custo da consulta histórica
V-FASE-AS-OF-CADASTRO-SEM-LINHA@903|L|Cadastro divergente aparece na lista
DEGRAU-HE-100-CADASTRO@904|L|Convênio configura limite de extra
V-FASE-AS-OF-SEM-CELULA@905|L|Mudança reabre dia sem célula
O261-E-F1C4-LEN-VIVOS@906|L|Cobrança lê decisão já lavrada
LENVIVOS-A6-ALARME@907|L|Alarme lê cobrança lavrada
LENVIVOS-SOBRAS-ESMERIL@908|L|Revisar sobras da cobrança lavrada
LENVIVOS-EMITIR-INERTE@909|L|Emissor declara parâmetros realmente usados
LENVIVOS-CHAVE-DO-DIA@910|L|Emissor usa dia definido pelo juiz
LENVIVOS-COBRANCAS-VIVAS@911|L|Protesto conta cobranças realmente vivas
O261-E-F1C4-MEMO-C6@912|L|Reutilização respeita ata e cadastro|+........
MEMO-C6-BATIDAS-PREFETCH@913|L|Consulta usa batidas atuais|[nao verificado]
V-FASE-AS-OF-ATRASO@916|L|Mudança de cadastro atualiza fase
V-FASE-AS-OF-UNIVERSO@917|L|Fase antiga não mantém cobertura indevida
V-FASE-AS-OF-SOBREPOSTA@918|L|Mudança de competência evita sobreposição
V-FASE-AS-OF-INTRADIA@919|L|Histórico preserva mudanças no mesmo dia
PROPOSITOR-JANELA-LONGA@920|L|Proposta funciona em janela longa
FILA-POR-CAUSA-AS-OF@921|L|Fila usa cadastro da época
F1C4-3-NONE-LEITORES@923|L|Leitores distinguem dia ainda indefinido
CARTORIO-PROTESTA-1A-CLAUSULA@925|L|Protesto respeita natureza do dia
F1C4-3-RG-SEM-CELULA-CASO@926|L|Provar cobrança sem célula e âncora|V
E3-GRADE-SUBTIPO-FOLGA@927|E|Status distingue espécies de folga
E3-GRADE-POR-TURNO-MEDIR@928|E|Medir configuração que altera status
E3-HASH-A2@929|E|Mudança de folga atualiza status
EMIS-LEITORES-RESTANTES@930|L|Leitores consultam dia sem trabalho|P|dep COBERTURA-PARCIAL-UM-JUIZ
EMIS-PAUTA-DEDUP@931|L|Evitar pauta duplicada por concorrência
EMIS-CATRACA-FLUXO@932|I|Provar fluxo completo do emissor
BUG-ENC-CELULAS-FORMA@933|L|Dias encerrados recebem células corretas|X|+........|dep E4-CARGA-PARIDADE
E4-CARGA-PARIDADE@934|E|Carga e tela recebem mesma janela|dep BUG-ENC-CELULAS-FORMA
RELATO-ESCRITOR-FORA-DO-FLOCK@935|I|Publicação respeita trava única
PDC-RELOGIO-NA-CLI@936|I|Comando recebe instante pela borda
PDC-PAINEL-SEM-DUVIDA@937|I|Painel distingue zero de desconhecido
E4-S4-ENCERRADOS@938|E|Provar custo com dia encerrado
E4-NAO-LAVRADO-PASSADO@939|E|Passado sem lavra mostra falta de prova
GRADE-09-PAUTA@940|L|DP recebe diferenças da grade exportada|dep GRADE-V5-SEM-PORTAO
E3-STATUS-EXPORTADA@942|E|Status atualiza sem alterar pagamento
CODS-DO-ATO@943|L|Ato altera somente dias que julgou|V|...+..++.
ISENTO-H10-MENORES@945|E|Reforçar prova do horizonte de isento
BACKFILL-UNICO@947|L|Passivo usa uma porta única
CODS-ORFAO14H-SEM-GATILHO@948|L|Explicar nascimento de cobrança órfã|+........
ENCADEADO-0111-SEM-CARIMBO@949|I|Corrente registra término dos comandos|+........|dep NOREVERSE-UI-NO-CORE
DEPLOY-RELOAD-RESIDUOS@951|I|Verificar contrato e recarga do deploy [nao verificado]
TELA-PREFIXO-SEM-GUARDA@952|I|Provar rotas das diferentes cascas
NOREVERSE-UI-NO-CORE@953|L|Cron funciona sem rotas da interface|V
E3-ISENTO-INSUMOS@954|E|Isenção integra entradas do julgamento|P|dep ISENTO-H10
ISENTO-PONTO-JUIZ-REGISTRADO@955|L|Isenção tem autoridade declarada
ISENTO-FLAG-ATEMPORAL@956|L|Definir alcance temporal da isenção|dep E3-ISENTO-INSUMOS
REVERSAO-PELA-PORTA@957|I|Reversão usa porta de escrita
E3-L02-PENDENTE@958|E|Status lê lista de cadastro pronta
B3-TOPO-FILA@959|I|Disjuntor identifica obra corretamente
CLAUDE-MD-PISTA-NO-ANEXO@961|I|Anexo separa instrução de contexto
CODEX-CONTEXTO-SEM-CONDUTA@963|I|Consulta recebe fatos e leis|P|dep CLAUDE-MD-ENXUTO
CODEX-LICOES@964|I|Consulta reaproveita lições verificadas|+........|dep CODEX-CONTEXTO-SEM-CONDUTA
V3-RETOMA@965|C|Retomar cálculo certificado pela regra
V3-RETOMA-PERIODOS-DA-CELULA@966|C|Cálculo usa períodos oficiais da célula|X|..~......|dep P5-MOTOR
V3-P5-PRORROGACAO-POR-INTERVALO@967|C|Noturno respeita prorrogação por intervalo|V
V3-HE-NOTURNA-POR-PECA@968|C|Extra noturna considera cada período|+........|dep V3-P5-PRORROGACAO-POR-INTERVALO
EMIS-FALSE-ATA-TRABALHO@969|L|Conciliar folga da célula e ata
GRADE-FOLGA-C5@970|L|Auditoria respeita vínculo da folga
GRADE-FOLGA-GERADOR@971|L|Gerador consulta folga pelo juiz
V3-MOTOR-FERIADO-HE100-NOTURNA@972|C|Feriado conserva extra noturna devida
COBPARC-B-CAUSA-NO-MOTOR@973|L|Lista mostra causa da parcial
V3-NOTURNO-RECORTE-LONGO@974|C|Noturno funciona em plantão longo|P
F1C4-3-EMISSORES-FILA@975|L|Emissores consultam natureza oficial do dia
REVERTER-REGEN-TRIESTADO@976|L|Reversão preserva dia indefinido
E3-OPUS-FILA@977|E|Corrigir sobras do status lavrado
V3-RETOMA-FILA@978|I|Comparar cálculo e medir diferenças|X|dep V3-RETOMA
LEI-AKITA-UNIVERSO-DO-PUSH@979|I|Push verifica apenas commits devidos [nao verificado]
LEI-AKITA-NO-PRE-PUSH@980|I|Pre-push executa selo de commits [nao verificado]
SELO-SETUP-COPIADO-DE-FREEZE@981|I|Detectar teste que reaproveita preparação [nao verificado]
HX-AUTOR-MUDO-REGEX@982|I|Detectar ações sem autor real
F1C4-FILA-OPUS@983|L|Revisar sobras da autoridade de cobrança
E6-POR-DONO@984|E|Separar divergências por responsável|dep F1C4
P14-CONFIRMAR-IMPLAUSIVEL-SEM-MOTIVO@985|L|Confirmação implausível exige motivo
F5-RELOGIO-DO-EXPORT@987|L|Exportação usa instante da rodada
RESPONDIDA-SEM-TEXTO-DUAS-COPIAS@988|L|Resposta vazia tem definição única
PERGUNTA-VIVA-LOGA-PREVISTO-COMO-FALHA@989|L|Separar ocorrência prevista de falha
E2-D3-TRABALHA-FORA-DA-IMPRESSAO@990|E|Mudança do trabalho atualiza julgamento
E2-D3-VINCULO-NA-IMPRESSAO@991|E|Mudança do vínculo atualiza julgamento
E2-D3-FERIADO-NA-IMPRESSAO@992|E|Mudança de feriado atualiza julgamento
E2-D3-CCT-NA-IMPRESSAO@993|E|Mudança da convenção atualiza julgamento
E2-D3-ISENCAO-NA-IMPRESSAO@994|E|Mudança da isenção atualiza julgamento
E2-D3-JUSTIFICATIVA-APROVADA@995|E|Justificativa aprovada atualiza julgamento
E2-D3-BATIDA-DA-VESPERA@996|E|Batida da véspera atualiza julgamento
E2-D3-EDICAO-HUMANA-SEM-PORTA@997|E|Edição humana usa porta rastreável
O261-V-FASE-NA-LISTA@999|L|Admin vê fase divergente com dias|V|dep O261-VETO-NA-PORTA
JUIZ-UNICO-DA-ANCORA@1000|L|Escala usa uma âncora única|dep O261-V-FASE-NA-LISTA
DONO-NAO-VE-FASE-DESLOCADA@1001|L|Divergência identifica cadastro fora de fase
FOLGA-TRABALHADA-RESOLVE@1002|L|Admin resolve trabalho em folga|V
BO-[nome]-DESTRAVA@1003|U|Retrato informa ação que destrava|+........
ALERTA-DESCARTES-DO-CODEX@1004|I|Mostrar descartes recentes da revisão
CODEX-ESCREVE-RED-WRAPPER@1005|I|Criar testes vermelhos em cópia [nao verificado]
FIO-DO-ADMIN-NO-ESTADO@1006|I|Mostrar cobertura da fila do admin
FIO-DO-ADMIN-LAVRA-VELHA@1007|L|Vigia identifica lista desatualizada
VETO-IMPRESSAO-NUNCA-BATEU@1008|L|Veto acompanha entradas corretas do dia
VETO-TRILHA-CICLO@1009|L|Registrar veto que retorna depois
VETO-VIGIA-NO-PLACAR@1010|I|Placar mostra cobrança em dia vetado
AVISO-ATIVO-SEM-VINCULO-NO-DIA@1011|U|Avisar ativo sem vínculo no dia
TESTES-QUE-NAO-PROVAM-[nome]@1012|I|Provar nascimento real das perguntas [nao verificado]
CLEAR-SEM-MARCO@1013|I|Registrar reinício de contexto corretamente
ANALISAR-RELE-O-BANCO@1014|L|Análise consulta estado atualizado
REABRIR-DISPUTA-ADMIN-IGNORA-RETORNO@1015|L|Reabertura informa resultado efetivo
VETO-ATO-HUMANO-MOSTRA-E-TRILHA@1016|L|Admin vê e registra veto
TESTE-DESLIGADO-C2-VACUO@1017|I|Provar proteção do desligado [nao verificado]
DIAS-ENCERRADOS-CHAVE-DO-CARTORIO@1018|L|Dias encerrados usam células corretas
VETO-TRILHA-FRASE-DO-REABRIR@1019|L|Trilha descreve a reabertura correta
VETO-NA-PORTA-POR-ELIMINACAO@1020|L|Recusa tem causa comprovada
SELO-CARIMBO-VIRADA@1021|I|Provar carimbo na virada do dia
CXR-FALHA-CALADA@1022|L|Lista revela falha de leitura
FASE-12X36-SEM-PAPEL-DA-ATA@1023|L|Fase respeita papel lavrado das batidas
PLANTIO-ENVELOPE-PELO-GERADOR@1024|L|Batida usa vínculo gerador do dia
DIA-DA-PERGUNTA-E-O-TURNO@1025|L|Pergunta pertence ao dia do turno|+........
COL309-SETEMBRO@1027|L|Corrigir células preservando folha paga|V|.......+.
COL309-09-RECALCULO-DEPOIS-DO-ADMIN@1028|L|Recalcular folha após decisão do admin|P
CAL-PALAVRA-BANDAID@1030|I|Documentar custo real do calendário
CAL-PREDICADO-D-MENOR-HOJE-2-COPIAS@1031|L|Calendário consulta janela da folha
CAL-CUSTO-NO-MAPA-DE-FUROS@1032|L|Reduzir consultas da tela de furos
CAL-DOIS-RELOGIOS@1033|L|Calendário usa um instante único
CAL-TESTE-HOJE-SEM-PREVISTO@1034|I|Provar hoje sem previsão [nao verificado]

[truncado em 20000 de 31906 caracteres]

## 13/11 20:08 · sessao f780beef7e42 · E3-FORMA-SQL · construtor · origem=Code

Parei pela trava do item **(c)**. No teste existente `test_verbo_nao_e_traducao_da_acao_da_tarefa`, a entrada `responder/colaborador` com verbo `validar` muda o fio de **`colab` para `admin`** ao unificar pela escada. A reprodução foi sem ORM, com alteração só em memória ([evidência](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/e3_portas_v5_desenho.r:2)). [nao verificado]

O teste foi preservado e a cura não foi aplicada ([integridade](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/e3_portas_v5_integridade.r:1)). [nao verificado]

Os seis REDs das cinco classes falharam contra o HEAD. No recorte completo: **10 testes, 11 falhas contando subcasos, zero erros** ([resultado](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/e3_portas_v5_red.r:1)). Diff, comandos, escritores/leitores, contraprovas e pendências estão no [relato da entrega](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/e3_portas_v5_entrega.txt:25). [nao verificado]

**Pergunta de desenho:** nessa entrada, deve prevalecer `fio='admin'`, pelo verbo `validar`, mesmo com ação `responder/colaborador`? ([caso](/home/ronald/wt-codex-e3-forma-sql-195919/app/chamados/tests/test_e3_portas_tarefa.py:84))

## 13/11 20:07 · sessao 25b2389f9aaa · BUG-PAR-DESCONGELA · leitura · origem=Code

**1. No contrato atual, `data_evento` significa dia do turno.** Para perguntas de intervalo, os motivos correspondem aos marcos `int_ida` e `int_volta`; a porta datada exige o dia do turno. Esse dia pode ser diferente do dia civil da batida após meia-noite. (`app/chamados/services/declaracao_texto.py:42-52`; `app/chamados/models.py:2832-2844`; `app/escala/servico_jornada.py:69-94`.)

**Escritores de produção do campo:**

| Escritor | Valor que escreve |
|---|---|
| `gravar_dia_do_fato` | Única atribuição direta: `linha.data_evento = dia_do_turno`. Não calcula nem salva. `app/chamados/services/fio.py:791-802`. |
| Nascimento, `_nascer` | Resolve `_dia_fato` por `dia_do_turno_da_linha`, entrega ao escritor único e insere a pergunta. `app/chamados/models.py:2931-2934`, `app/chamados/models.py:3070-3073`. |
| `reapontar_resolvedora` | Quando a pergunta está sem data e recebe uma, resolve `_dia` pelo mesmo helper e salva. As atribuições de `None` nos ramos de recusa apenas desfazem a alteração em memória. `app/chamados/services/fio.py:868-880`, `app/chamados/services/fio.py:895-913`, `app/chamados/services/fio.py:928-929`. |
| `rechavear_data_evento` | Grava exatamente `novo_dia`, recebido do chamador, junto da célula e da lavra. `app/chamados/services/fio.py:1035-1044`. |
| Reuso de pergunta em `abrir` | Quando necessário, encaminha a mudança para `rechavear_no_reuso`, que chama o rechaveamento acima. `app/chamados/models.py:2996-3008`; `app/chamados/services/fio.py:1075`. |
| Comando de migração/reversão | Usa o mesmo rechaveamento; seu universo normal contém `orfao_14h` e `saida_sem_entrada`, sem intervalo. `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:53`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:400`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:492`. |

**Origem dos valores nas perguntas de intervalo:**

| Caminho | Data fornecida ao nascimento |
|---|---|
| Fábrica da célula/cartório | `cel.data` → contexto `{motivo: {'data': ...}}` → `gerar_perguntas_disputa` → `abrir`. `app/ponto/services/cartorio.py:1043-1045`; `app/chamados/services/fabrica_da_celula.py:172-174`, `app/chamados/services/fabrica_da_celula.py:219`; `app/chamados/services/disputa_emissao.py:372-375`, `app/chamados/services/disputa_emissao.py:424-451`. |
| Geração sem data no contexto | Dia do turno da batida-origem, confirmado pela grade. `app/chamados/services/disputa_emissao.py:350-357`, `app/chamados/services/disputa_emissao.py:65-86`. |
| Complementação de marcos | `dia` devolvido pelos marcos faltantes da célula; exige igualdade com o dia solicitado. `app/chamados/services/declaracao_texto.py:106-109`, `app/chamados/services/declaracao_texto.py:139`; `app/chamados/services/disputa_emissao.py:873-882`, `app/chamados/services/disputa_emissao.py:837-844`. |
| Declaração direta do colaborador | `dia` do marco faltante casado com a hora; também pode datar pergunta existente pela costura. `app/chamados/services/declaracao_texto.py:341-346`, `app/chamados/services/declaracao_texto.py:370`, `app/chamados/services/declaracao_texto.py:376-377`. |
| Declaração textual aplicada pelo admin | `p['dia']`, proveniente do marco faltante casado. `app/chamados/services/declaracao_texto.py:625-635`, `app/chamados/services/declaracao_texto.py:666-667`. |
| Agrupamento legado de `intervalo_ausente` | Lê literalmente `marcos_faltantes[*]['data']`. Aqui existe outra origem semântica: o detector legado produz essa data com `localtime(marco_absoluto).date()`, portanto **dia civil do marco**. O detector atualmente suprime a emissão, mas o consumidor legado continua no agrupador. `app/chamados/services/disputa_emissao.py:1138-1154`; `app/ponto/management/commands/detectar_intervalo_ausente.py:77-87`, `app/ponto/management/commands/detectar_intervalo_ausente.py:150-157`. |

`dia_do_turno_da_celula(d)` **não converte** uma data civil para dia do turno: retorna a mesma data com o tipo `DiaDoTurno`. Portanto, passar por esse helper não corrige o valor do caminho legado. (`app/ponto/turnos.py:2939-2943`.)

A porta de conversa escreve `localdate()`, com significado de dia civil da conversa; o gerador impede que pergunta de hora sem dia nasça por essa alternativa. (`app/chamados/models.py:2850-2858`; `app/chamados/services/disputa_emissao.py:424-430`.) Qual desses caminhos escreveu cada PK anexado é **[nao verificado]**.

**2. A tela mostra a data da pergunta, mas captura somente uma hora.**

O PWA renderiza `chamados/meu_atendimento.html`, que inclui `_card_atendimento.html`. O cartão mostra **“Estamos falando do dia”**, seguido de `p.data_evento`, e depois `p.pergunta`. (`app/chamados/views.py:632-633`; `app/templates/chamados/meu_atendimento.html:127`; `app/templates/chamados/partials/_card_atendimento.html:87-93`.)

Os textos canônicos pedem “saída do intervalo **no dia {data}**” e “volta do intervalo **no dia {data}**”. O formulário oferece `type="time"`; não coleta data nem deslocamento `+1`. (`app/chamados/catalogo/perguntas.py:124-134`; `app/templates/chamados/partials/_card_atendimento.html:111-134`.)

O POST entrega a string ao core, que valida `HH:MM` e grava `resposta_colab`; não transforma a resposta em instante nessa etapa. **A interpretação computacional posterior é hora dentro do turno identificado por `data_evento`.** (`app/chamados/views.py:1265`, `app/chamados/views.py:1283`; `app/chamados/services/disputa_emissao.py:1441-1449`, `app/chamados/services/disputa_emissao.py:1467-1469`; `app/chamados/services/materializacao.py:419-426`.)

A tela não esclarece “dia civil”, “dia do início do turno” ou “dia seguinte” nesse formulário. O entendimento do colaborador e a tela efetivamente usada em cada resposta são **[nao verificado]**. (`app/templates/chamados/partials/_card_atendimento.html:78-157`.)

Além disso, o texto persistido pode conservar uma data anterior ao rechaveamento: a emissão recebe texto e data separadamente, e o rechaveamento não regrava `pergunta`. A ocorrência disso nos PKs anexados é **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:447-451`; `app/chamados/services/fio.py:1038-1044`.)

**3. O avanço para o dia seguinte ocorre na cadeia de marcos e na escolha do candidato overnight.**

A sequência é:

1. `_ts_tipo_do_plantio` escolhe `p.data_evento`, com fallback no contexto do motivo. (`app/chamados/services/materializacao.py:345-353`.)
2. Para intervalo, pode substituir a hora respondida pela hora do marco faltante do chamado. (`app/chamados/services/materializacao.py:409-416`.)
3. Combina a data escolhida com essa hora e chama `_ajustar_saida_overnight`. (`app/chamados/services/materializacao.py:417-427`.)
4. `marcos_turno` encadeia os marcos e acrescenta um dia enquanto o próximo instante for `<=` ao anterior. (`app/escala/servico_jornada.py:82-94`.)
5. O ajuste constrói o envelope `[primeiro marco − 90 min, último marco + 90 min]`, testa primeiro **D**, depois **D+1**, com a mesma hora local, e devolve o primeiro candidato contido. Portanto, chega a D+1 quando o candidato em D não cabe e o de D+1 cabe; sem marcos ou candidato conserva o instante inicial. Vale para tipos `E` e `S`. (`app/chamados/juizes.py:312-331`.)

Pelo contrato de **dia do turno**, D+1 é uma representação prevista para a madrugada daquele turno; a desigualdade entre as datas, isoladamente, não demonstra mudança do evento declarado. O próprio código exemplifica a volta `00:00` do turno de `08/08` como `09/08 00:00`. (`app/chamados/juizes.py:306-309`.)

Entretanto, **o ajuste de data preserva a hora que recebeu, e essa hora já pode ter sido substituída**. No anexo, por exemplo, PK 41706 informa `01:50 → 02:00`, portanto não conserva o horário declarado **[nao verificado]**; o mecanismo correspondente está em `app/chamados/services/materializacao.py:412-419`.

**4. Leitores: há uma diferença comprovada entre chave de turno e chave civil.**

| Leitor | Significado utilizado |
|---|---|
| Paridade principal | Usa o `ts` derivado e procura vizinhas cronológicas; acompanha o ajuste do plantio, sem recombinar `data_evento`. `app/chamados/services/materializacao.py:86-105`, `app/chamados/services/materializacao.py:852`, `app/chamados/services/materializacao.py:881-883`. |
| Exceção `_plantio_preenche_furo_na_data` | Consulta `CelulaDia.data = localtime(ts).date()`: **dia civil do plantio**, potencialmente D+1. É uma chave diferente do dia do turno; não é leitura direta de `data_evento`. `app/chamados/services/materializacao.py:157`, `app/chamados/services/materializacao.py:171-182`. |
| `estado_aplicacao_pergunta` | Reutiliza o derivador, a paridade e essa mesma exceção; não possui outra interpretação independente. `app/chamados/services/materializacao.py:449-459`. |
| Recusa `DATA_EM_DISPUTA` | Compara `p.data_evento` com o dia do fato do chamado, não com a data civil do `ts`. Pressupõe dia do turno. `app/chamados/services/fio.py:84-88`; `app/chamados/services/materializacao.py:1197-1204`. |
| Texto da recusa | Diz “dia da pergunta (onde a batida nasceria)”, expressão que não distingue turno de data civil; esse texto não executa conversão. `app/chamados/catalogo/recusa.py:45-51`. |
| Cartório | Trabalha por `cel.data` e consulta `fio_vivo`; este delega a `pergunta_viva`, cujo leitor de dia devolve `data_evento` para fatos e exclui conversas. Pressupõe dia do turno. `app/ponto/services/cartorio.py:1353-1355`, `app/ponto/services/cartorio.py:1389`; `app/chamados/juizes.py:1513`, `app/chamados/juizes.py:1464-1471`, `app/chamados/juizes.py:334-338`. |
| Prova de materialização | Aceita batida vinculada na hora esperada em D **ou** D+1, com tolerância de 20 minutos; não exige data civil igual a `data_evento`. `app/chamados/juizes.py:267-279`, `app/chamados/juizes.py:752-761`. |

O efeito concreto da consulta da paridade à célula civil D+1 depende das células e atas dessas linhas: **[nao verificado]**. A existência de batida efetivamente persistida para cada resultado calculado também é **[nao verificado]**.

**Conclusão: para o conjunto, o código não permite dizer que o instante calculado seja o declarado: D significa dia do turno, a captura guarda apenas HH:MM e falta a data civil efetivamente declarada; as linhas com hora substituída já diferem no horário (`app/chamados/models.py:2839`; `app/chamados/services/disputa_emissao.py:1467`; `app/chamados/services/materializacao.py:412-427`; declaração civil por PK [nao verificado]).**

## 13/11 20:06 · sessao 21865d8212f3 · - · - · origem=Ronald (tmux)

Escritores de `data_evento` no código atual:

| Caminho | Valor atribuído/persistido |
|---|---|
| Ponto único `gravar_dia_do_fato` | Atribui exatamente `dia_do_turno`, sem calcular nem salvar (`app/chamados/services/fio.py:791`, `:801`). |
| Nascimento `_nascer` | Obtém `_dia_fato` de `dia_do_turno_da_linha`, atribui e salva (`app/chamados/models.py:2931`, `:3072`, `:3073`). |
| Costura de pergunta sem data | Só data quando recebeu `data_evento` e a pergunta não tinha data; obtém `_dia` do mesmo helper, atribui e salva (`app/chamados/services/fio.py:868`, `:877`, `:880`, `:928`). As atribuições de `None` em recusas desfazem a alteração em memória antes do retorno (`:898`, `:913`). |
| Rechaveamento | Grava exatamente `novo_dia`, recebido do chamador (`app/chamados/services/fio.py:1038`, `:1044`). |
| Reuso de `abrir` | Se o dia gravado difere de `_dia_fato`, chama `rechavear_no_reuso` (`app/chamados/models.py:2996`, `:3008`), que usa `rechavear_data_evento` (`app/chamados/services/fio.py:1075`). |
| Comando de migração / reversão | Usa a mesma porta no apply e na reversão (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:400`, `:492`). Seu universo normal contém somente `orfao_14h` e `saida_sem_entrada`, não intervalo (`:53`). |
| Campo / migration | Campo nullable, sem default (`app/chamados/models.py:2479`); migration somente adiciona o campo, sem backfill (`app/chamados/migrations/0025_perguntadisputa_data_evento.py:13`). |

Para intervalo, os produtores de valor que alimentam esses escritores são:

1. **Fábrica da célula, ativa:** o cartório entrega `_cel` a `perguntar_pela_celula` (`app/ponto/services/cartorio.py:1043`); esta passa `cel.data` para `perguntar` (`app/chamados/services/fabrica_da_celula.py:219`), que escreve `{motivo: {'data': dia.isoformat()}}` no contexto da disputa (`:172`). `gerar_perguntas_disputa` lê literalmente esse valor (`app/chamados/services/disputa_emissao.py:372`), mantém para intervalo a data do contexto e descarta a data retornada pelo derivador de destino (`:408`), escolhe `_dia_p` (`:424`) e chama `abrir(data_evento=dia_do_turno_da_celula(_dia_p))` (`:444`, `:451`). Nesse caminho o produtor é `cel.data`, portanto dia do turno (`app/ponto/turnos.py:2939`).

2. **Fallback da geração sem data no contexto:** `gerar_perguntas_disputa` chama `_data_furo_da_grade` (`app/chamados/services/disputa_emissao.py:350`), que localiza a batida-origem, consulta `dia_do_turno_da_batida` e devolve esse dia se a célula confirmar trabalho (`:65`, `:76`, `:83`, `:86`). O valor é dia do turno por derivação, não a data civil do marco (`:60`).

3. **Complementação de marcos do dia:** `_complementar_marcos_faltantes` lê `_marcos_faltantes`, percorre `(dia,pos,tipo,hora)`, exige `dia == data_evento` e chama `_criar_pergunta_se_couber(..., dia_do_turno_da_celula(dia),...)` (`app/chamados/services/disputa_emissao.py:859`, `:873`, `:874`, `:881`); esta chama `PerguntaDisputa.abrir` passando o mesmo `data_evento` (`:837`, `:844`). `_marcos_faltantes` obtém `data_dia` da célula e devolve esse dia, não o dia civil absoluto da hora (`app/chamados/services/declaracao_texto.py:106`, `:109`, `:139`). Os motivos do intervalo vêm de `int_ida/S` e `int_volta/E` (`:42`).

4. **Declaração direta do colaborador:** casa a hora com um marco faltante (`app/chamados/services/declaracao_texto.py:341`), obtém `dia,pos,tipo,marco` (`:345`) e cria pergunta com `data_evento=dia_do_turno_da_celula(dia)` (`:376`). Se a candidata existente está sem destino, usa `reapontar_resolvedora(...,data_evento=dia)` (`:370`).

5. **Declaração textual aplicada pelo admin:** a proposta obtém `dia` do marco faltante casado (`app/chamados/services/declaracao_texto.py:625`, `:628`, `:635`); `aplicar_declaracao` cria com `data_evento=dia_do_turno_da_celula(p['dia'])` (`:666`).

6. **Caminho legado de chamado `intervalo_ausente`: atenção à divergência.** O agrupador lê `marcos_faltantes[*]['data']`, converte essa string diretamente em `_de` e chama a porta com `dia_do_turno_da_celula(_de)` (`app/chamados/services/disputa_emissao.py:1143`, `:1148`, `:1154`). O produtor legado `furos_intervalo`, entretanto, constrói `data` como **dia civil do marco absoluto encadeado**, por `localtime(...).date()`, enquanto a célula tem seu próprio `cel.data` (`app/ponto/management/commands/detectar_intervalo_ausente.py:77`, `:78`, `:82`). Esse detector atualmente não emite: há `continue` após contabilizar supressão (`:156`, `:157`). O agrupador para chamados legados continua presente (`app/chamados/services/disputa_emissao.py:1138`).

O contrato atual explicita “DIA DO TURNO”, obrigatório e sem default na porta datada (`app/chamados/models.py:2839`); intervalo é marco (`app/chamados/services/declaracao_texto.py:45`), e `dia_do_turno_da_linha` devolve `dia_do_turno_da_celula(d)` para marco (`app/chamados/services/fio.py:1639`). Esse construtor **não converte a data**, somente a identifica como dia da célula (`app/ponto/turnos.py:2940`).

Portanto, contrato atual e produtor ativo principal dizem dia do turno; existe código de consumo legado que rotula dia civil de marco como dia do turno sem conversão (`app/chamados/models.py:2839`; `app/chamados/services/fabrica_da_celula.py:219`; `app/chamados/services/disputa_emissao.py:1148`; `app/ponto/management/commands/detectar_intervalo_ausente.py:82`). Qual caminho escreveu cada PK do anexo, e qual texto/data efetivamente foi mostrado quando respondeu, é `[nao verificado]`.

## 13/11 20:05 · sessao c5d2c9e5c938 · - · - · origem=Ronald (tmux)

- O PWA do colaborador usa `meu_atendimento`, monta o fio e renderiza `chamados/meu_atendimento.html`; as duas abas incluem `_card_atendimento.html`. (`app/chamados/views.py:559`, `app/chamados/views.py:569`, `app/chamados/views.py:632`, `app/templates/chamados/meu_atendimento.html:127`, `app/templates/chamados/meu_atendimento.html:144`)
- A pergunta viva mostra literalmente **“Estamos falando do dia”**, seguido de **`p.data_evento`**, e depois o texto persistido **`p.pergunta`**. Não calcula nem apresenta a data civil futura do plantio nesse bloco. (`app/templates/chamados/partials/_card_atendimento.html:87`, `app/templates/chamados/partials/_card_atendimento.html:90`, `app/templates/chamados/partials/_card_atendimento.html:93`)
- Os textos canônicos de intervalo são “Confirme o horário da saída do intervalo **no dia {data}**” e “Confirme o horário da volta do intervalo **no dia {data}**”; ambos recebem resposta do tipo `hora`. (`app/chamados/catalogo/perguntas.py:124`, `app/chamados/catalogo/perguntas.py:125`, `app/chamados/catalogo/perguntas.py:126`, `app/chamados/catalogo/perguntas.py:132`, `app/chamados/catalogo/perguntas.py:133`, `app/chamados/catalogo/perguntas.py:134`)
- O formulário captura **somente HH:MM**: input `type=time`; o JavaScript oferece horas 0–23 e minutos 0–59 e monta `HH:MM`, sem data nem indicador de deslocamento de dia. (`app/templates/chamados/partials/_card_atendimento.html:132`, `app/templates/chamados/meu_atendimento.html:292`, `app/templates/chamados/meu_atendimento.html:294`, `app/templates/chamados/meu_atendimento.html:295`, `app/templates/chamados/meu_atendimento.html:312`, `app/templates/chamados/meu_atendimento.html:313`)
- O POST web lê essa string e a entrega ao core; o core apenas valida seu formato e salva `resposta_colab`/`respondida_em`, sem converter data. A interpretação como **dia do turno** ocorre posteriormente no materializador, não na captura da declaração. (`app/chamados/views.py:1265`, `app/chamados/views.py:1283`, `app/chamados/services/disputa_emissao.py:1441`, `app/chamados/services/disputa_emissao.py:1448`, `app/chamados/services/disputa_emissao.py:1467`, `app/chamados/services/disputa_emissao.py:1469`, `app/chamados/services/materializacao.py:419`, `app/chamados/services/materializacao.py:426`)
- **Semântica apresentada:** o bloco não diz “dia civil”, “dia do início do turno” ou “dia seguinte”; mostra uma data e pede uma hora. O contrato interno declara expressamente `data_evento` como **dia do turno**, mas a leitura mental do colaborador como esse dia ou como o dia civil mostrado é **[nao verificado]**. (`app/templates/chamados/partials/_card_atendimento.html:78`, `app/templates/chamados/partials/_card_atendimento.html:157`, `app/chamados/models.py:2839`, `app/chamados/models.py:2844`)
- O texto da pergunta pode conservar outra data: ele é interpolado de `contexto[motivo]['data']`, recebido separadamente de `data_evento` na emissão; o rechaveamento posterior grava `data_evento`, célula e lavra sem regravar `pergunta`. A ocorrência concreta dessa divergência nas linhas anexadas é **[nao verificado]**. (`app/chamados/catalogo/query.py:58`, `app/chamados/catalogo/query.py:64`, `app/chamados/services/disputa_emissao.py:344`, `app/chamados/services/disputa_emissao.py:447`, `app/chamados/services/disputa_emissao.py:451`, `app/chamados/services/fio.py:1038`, `app/chamados/services/fio.py:1044`)
- A API de chamados fornece `data_evento` e `data_evento_label`; a API específica de disputa fornece o texto `pergunta`, mas seu `_item` não inclui a coluna de data; ambas recebem a resposta pelo mesmo core. A interface nativa efetivamente vista e a porta usada para cada PK anexado são **[nao verificado]**. (`app/api/views.py:1153`, `app/api/views.py:1154`, `app/api/views.py:2542`, `app/api/views.py:2557`, `app/api/views.py:2614`, `app/api/views.py:2620`)

## 13/11 20:05 · sessao 12a5e43bc12a · - · - · origem=Ronald (tmux)

Leitores relevantes e significado:

- **Paridade principal:** `_materializar_resposta_hora` usa `ts` derivado por `_ts_tipo_do_plantio`; a guarda procura vizinha cronológica em ±12h desse instante, sem recombinar `data_evento` — portanto acompanha a interpretação de **dia do turno** do derivador. (`app/chamados/services/materializacao.py:852`, `app/chamados/services/materializacao.py:881`, `app/chamados/services/materializacao.py:86`–105.)
- **Exceção da paridade:** `_plantio_preenche_furo_na_data` usa `loc = localtime(ts)` e busca `CelulaDia.data=loc.date()`: chave de **dia civil do plantio**, que pode ser D+1, e não `p.data_evento`. É diferença objetiva de chave; seu efeito nestas perguntas depende da ata/células, ausentes no anexo `[nao verificado]`. (`app/chamados/services/materializacao.py:157`, `app/chamados/services/materializacao.py:171`–182.)
- **`estado_aplicacao_pergunta`:** chama o mesmo derivador e a mesma guarda/exceção; não possui interpretação alternativa de `data_evento`. (`app/chamados/services/materializacao.py:449`–459.)
- **Recusa `DATA_EM_DISPUTA`:** `data_do_fato_de` compara `p.data_evento` com o dia do chamado resolvido por `dia_do_fato(..., fortes=True)`; este prioriza `chamado.celula.data`, depois catálogo/contexto e `data_turno`. O gate pressupõe **dia do turno**, não compara `localdate(ts)` com `data_evento`. (`app/chamados/services/fio.py:84`–88; `app/chamados/juizes.py:98`–128; `app/chamados/services/materializacao.py:1197`–1204.)
- **Registro da recusa:** `_carimbar_recusa` traduz e grava motivo/via; não lê `data_evento` e não deriva data. O catálogo conserva texto “dia da pergunta (onde a batida nasceria)”, mas é texto explicativo, não nova conversão. (`app/chamados/services/materializacao.py:1089`–1113; `app/chamados/catalogo/recusa.py:45`–51.)
- **Cartório:** não lê diretamente `p.data_evento` em seu corpo; julga por `cel.data` e consulta `fio_vivo` nas perguntas do chamado. `fio_vivo` delega a `pergunta_viva`; seu fallback lê `_dia_de_turno_da_pergunta`, que devolve `data_evento` para perguntas de fato e exclui conversas. Significado: **dia do turno**. (`app/ponto/services/cartorio.py:1353`–1355, `app/ponto/services/cartorio.py:1384`–1389; `app/chamados/juizes.py:1504`–1513; `app/chamados/juizes.py:1464`–1471; `app/chamados/juizes.py:334`–338.)
- **Prova de materialização:** `_plantio_na_hora_do_slot` usa `data_evento` como âncora do turno, aceitando prova ligada na mesma hora em **D ou D+1**, ±20min. Isso relaxa a data civil aceita como prova; não lê `data_evento` como dia civil obrigatório. (`app/chamados/juizes.py:267`–279; `app/chamados/juizes.py:752`–761.)
- **Via por fato:** procura `TurnoMaterializado.data_turno=data_evento`; no ramo específico de adesão, testa qualquer batida apurável entre D00:00 e D23:59, recorte civil explicitamente mais amplo para rejeitar “não bateu”. Intervalos não são absolvidos pela mera saída final do turno. (`app/chamados/juizes.py:1728`–1743; `app/chamados/juizes.py:1751`–1765.)
- **Passivo/competência:** usa `p.data_evento`, não a data civil do instante overnight. (`app/chamados/services/passivo_validado.py:98`–113.)
- **Tarefa do dia:** associa a pergunta a `dia` por igualdade `p.data_evento == dia`, declaradamente dia do turno. (`app/chamados/services/tarefa_do_dia.py:218`–236.)

Trecho decisivo para o overnight: `_ajustar_saida_overnight` resolve vínculo em D, monta marcos do turno, considera envelope [primeiro−90min, último+90min], testa primeiro D e depois D+1 com a mesma hora local e devolve o primeiro candidato contido; sem marcos ou sem candidato conserva `ts`. (`app/chamados/juizes.py:296`–331.)

## 13/11 20:02 · sessao 3c84ced51e81 · BUG-PAR-DESCONGELA · sonda · origem=Code

Não localizei `sonda163.py` na cópia; não reexecutei a sonda. Segue a transcrição integral da saída fornecida, sem verificação independente. [nao verificado]

```text
sonda_leitura: sonda163.py [19cd94b4001bf974] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

UNIVERSO 60 = 60 | 103 = 103 (pks pedidos 103)
2026-11-13 19:57:47,228 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_volta pergunta_id=44363 resposta=21:05 tipo_pelo_motivo=E causa=Exception('marcos_faltantes=E x ata=S')
NoneType: None
2026-11-13 19:57:47,251 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_volta pergunta_id=45400 resposta=13:55 tipo_pelo_motivo=E causa=Exception('marcos_faltantes=E x ata=S')
NoneType: None
2026-11-13 19:57:47,260 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_saida pergunta_id=45598 resposta=12:06 tipo_pelo_motivo=S causa=Exception('marcos_faltantes=S x ata=E')
NoneType: None
2026-11-13 19:57:47,276 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_saida pergunta_id=46363 resposta=21:44 tipo_pelo_motivo=S causa=Exception('marcos_faltantes=S x ata=E')
NoneType: None
2026-11-13 19:57:47,302 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_volta pergunta_id=47492 resposta=13:50 tipo_pelo_motivo=E causa=Exception('marcos_faltantes=E x ata=S')
NoneType: None
2026-11-13 19:57:47,325 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_volta pergunta_id=49167 resposta=02:06 tipo_pelo_motivo=E causa=Exception('marcos_faltantes=E x ata=S')
NoneType: None
2026-11-13 19:57:47,334 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_volta pergunta_id=49170 resposta=02:09 tipo_pelo_motivo=E causa=Exception('marcos_faltantes=E x ata=S')
NoneType: None
2026-11-13 19:57:47,376 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_saida pergunta_id=50176 resposta=17:00 tipo_pelo_motivo=S causa=Exception('marcos_faltantes=S x ata=E')
NoneType: None
2026-11-13 19:57:47,661 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_saida pergunta_id=4695 resposta=01:10 tipo_pelo_motivo=S causa=Exception('marcos_faltantes=S x ata=E')
NoneType: None
2026-11-13 19:57:47,740 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_volta pergunta_id=7463 resposta=21:12 tipo_pelo_motivo=E causa=Exception('marcos_faltantes=E x ata=S')
NoneType: None
2026-11-13 19:57:47,754 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_volta pergunta_id=9485 resposta=21:02 tipo_pelo_motivo=E causa=Exception('marcos_faltantes=E x ata=S')
NoneType: None
2026-11-13 19:57:47,884 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_volta pergunta_id=22627 resposta=01:00 tipo_pelo_motivo=E causa=Exception('marcos_faltantes=E x ata=S')
NoneType: None
2026-11-13 19:57:47,917 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_saida pergunta_id=26782 resposta=12:38 tipo_pelo_motivo=S causa=Exception('marcos_faltantes=S x ata=E')
NoneType: None
2026-11-13 19:57:47,924 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_saida pergunta_id=26963 resposta=13:00 tipo_pelo_motivo=S causa=Exception('marcos_faltantes=S x ata=E')
NoneType: None
2026-11-13 19:57:47,939 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_volta pergunta_id=27039 resposta=05:05 tipo_pelo_motivo=E causa=Exception('marcos_faltantes=E x ata=S')
NoneType: None
2026-11-13 19:57:47,948 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_saida pergunta_id=27109 resposta=12:37 tipo_pelo_motivo=S causa=Exception('marcos_faltantes=S x ata=E')
NoneType: None
2026-11-13 19:57:47,956 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_saida pergunta_id=27110 resposta=12:37 tipo_pelo_motivo=S causa=Exception('marcos_faltantes=S x ata=E')
NoneType: None
2026-11-13 19:57:47,963 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_saida pergunta_id=27112 resposta=12:37 tipo_pelo_motivo=S causa=Exception('marcos_faltantes=S x ata=E')
NoneType: None
2026-11-13 19:57:47,970 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_volta pergunta_id=27135 resposta=01:00 tipo_pelo_motivo=E causa=Exception('marcos_faltantes=E x ata=S')
NoneType: None
2026-11-13 19:57:47,999 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_volta pergunta_id=27209 resposta=20:22 tipo_pelo_motivo=E causa=Exception('marcos_faltantes=E x ata=S')
NoneType: None
2026-11-13 19:57:48,055 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_saida pergunta_id=27450 resposta=11:13 tipo_pelo_motivo=S causa=Exception('marcos_faltantes=S x ata=E')
NoneType: None
2026-11-13 19:57:48,063 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_volta pergunta_id=27451 resposta=12:13 tipo_pelo_motivo=E causa=Exception('marcos_faltantes=E x ata=S')
NoneType: None
2026-11-13 19:57:48,087 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_saida pergunta_id=27464 resposta=16:31 tipo_pelo_motivo=S causa=Exception('marcos_faltantes=S x ata=E')
NoneType: None
2026-11-13 19:57:48,124 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_saida pergunta_id=27673 resposta=12:00 tipo_pelo_motivo=S causa=Exception('marcos_faltantes=S x ata=E')
NoneType: None
2026-11-13 19:57:48,150 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_volta pergunta_id=27680 resposta=14:05 tipo_pelo_motivo=E causa=Exception('marcos_faltantes=E x ata=S')
NoneType: None
2026-11-13 19:57:48,179 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_saida pergunta_id=27798 resposta=12:37 tipo_pelo_motivo=S causa=Exception('marcos_faltantes=S x ata=E')
NoneType: None
2026-11-13 19:57:48,188 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_volta pergunta_id=29469 resposta=20:06 tipo_pelo_motivo=E causa=Exception('marcos_faltantes=E x ata=S')
NoneType: None
2026-11-13 19:57:48,195 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_saida pergunta_id=30009 resposta=03:00 tipo_pelo_motivo=S causa=Exception('marcos_faltantes=S x ata=E')
NoneType: None
2026-11-13 19:57:48,217 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_volta pergunta_id=30012 resposta=13:00 tipo_pelo_motivo=E causa=Exception('marcos_faltantes=E x ata=S')
NoneType: None
2026-11-13 19:57:48,240 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_volta pergunta_id=31373 resposta=14:05 tipo_pelo_motivo=E causa=Exception('marcos_faltantes=E x ata=S')
NoneType: None
2026-11-13 19:57:48,262 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_volta pergunta_id=32646 resposta=02:00 tipo_pelo_motivo=E causa=Exception('marcos_faltantes=E x ata=S')
NoneType: None
2026-11-13 19:57:48,269 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_saida pergunta_id=32649 resposta=12:38 tipo_pelo_motivo=S causa=Exception('marcos_faltantes=S x ata=E')
NoneType: None
2026-11-13 19:57:48,277 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_saida pergunta_id=32914 resposta=10:00 tipo_pelo_motivo=S causa=Exception('marcos_faltantes=S x ata=E')
NoneType: None
2026-11-13 19:57:48,331 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_saida pergunta_id=36077 resposta=13:00 tipo_pelo_motivo=S causa=Exception('marcos_faltantes=S x ata=E')
NoneType: None
2026-11-13 19:57:48,339 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_saida pergunta_id=36567 resposta=13:00 tipo_pelo_motivo=S causa=Exception('marcos_faltantes=S x ata=E')
NoneType: None
2026-11-13 19:57:48,361 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_saida pergunta_id=38433 resposta=12:00 tipo_pelo_motivo=S causa=Exception('marcos_faltantes=S x ata=E')
NoneType: None
2026-11-13 19:57:48,375 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_saida pergunta_id=38647 resposta=13:00 tipo_pelo_motivo=S causa=Exception('marcos_faltantes=S x ata=E')
NoneType: None
2026-11-13 19:57:48,429 ERROR chamados.materializacao best-effort falhou: operacao=tipo_marco_divergente motivo=intervalo_saida pergunta_id=41988 resposta=12:00 tipo_pelo_motivo=S causa=Exception('marcos_faltantes=S x ata=E')
NoneType: None
('103', 'DIFERENTE(data)') 24
('103', 'DIFERENTE(hora)') 46
('103', 'DIFERENTE(hora+data)') 13
('103', 'DIFERENTE(tipo)') 4
('103', 'IGUAL') 15
('103', 'NAO_GRAVA(invalida)') 1
('60', 'DIFERENTE(data)') 40
('60', 'DIFERENTE(hora)') 10
('60', 'DIFERENTE(hora+data)') 5
('60', 'DIFERENTE(tipo+data)') 1
('60', 'IGUAL') 4
('total', 'DIFERENTE') 143
('total', 'IGUAL') 19
('total', 'NAO_GRAVA') 1
ERROS_SONDA 0 (erros>0 = NAO MEDIDO, nunca zero)
LINHAS grupo pk col motivo data declarado_hora declarado_tipo gravaria classe erro
('60', 36255, 824, 'intervalo_saida', '2026-09-01', '22:00', 'S', '2026-09-01 22:00 S', 'IGUAL', '')
('60', 39363, 926, 'intervalo_saida', '2026-10-02', '14:02', 'S', '2026-10-02 16:00 S', 'DIFERENTE(hora)', '')
('60', 39872, 821, 'intervalo_saida', '2026-10-04', '13:00', 'S', '2026-10-04 13:00 S', 'IGUAL', '')
('60', 41692, 594, 'intervalo_saida', '2026-10-08', '00:00', 'S', '2026-10-09 00:00 S', 'DIFERENTE(data)', '')
('60', 41693, 594, 'intervalo_volta', '2026-10-08', '01:00', 'E', '2026-10-09 01:00 E', 'DIFERENTE(data)', '')
('60', 41706, 841, 'intervalo_volta', '2026-10-08', '01:50', 'E', '2026-10-09 02:00 E', 'DIFERENTE(hora+data)', '')
('60', 44363, 468, 'intervalo_volta', '2026-10-15', '21:05', 'E', '2026-10-15 18:00 E', 'DIFERENTE(hora)', '')
('60', 45277, 834, 'intervalo_saida', '2026-10-17', '03:00', 'S', '2026-10-18 03:00 S', 'DIFERENTE(data)', '')
('60', 45278, 834, 'intervalo_volta', '2026-10-17', '04:00', 'E', '2026-10-18 04:00 E', 'DIFERENTE(data)', '')
('60', 45400, 309, 'intervalo_volta', '2026-10-18', '13:55', 'E', '2026-10-18 11:30 E', 'DIFERENTE(hora)', '')
('60', 45598, 707, 'intervalo_saida', '2026-10-18', '12:06', 'S', '2026-10-18 16:00 S', 'DIFERENTE(hora)', '')
('60', 45599, 707, 'intervalo_volta', '2026-10-18', '13:06', 'E', '2026-10-18 17:00 E', 'DIFERENTE(hora)', '')
('60', 46363, 841, 'intervalo_saida', '2026-10-20', '21:44', 'S', '2026-10-21 01:00 S', 'DIFERENTE(hora+data)', '')
('60', 47207, 467, 'intervalo_saida', '2026-10-21', '01:00', 'S', '2026-10-22 01:00 E', 'DIFERENTE(tipo+data)', '')
('60', 47208, 467, 'intervalo_volta', '2026-10-21', '02:00', 'E', '2026-10-22 02:00 E', 'DIFERENTE(data)', '')
('60', 47492, 309, 'intervalo_volta', '2026-09-26', '13:50', 'E', '2026-09-26 11:30 E', 'DIFERENTE(hora)', '')
('60', 48611, 908, 'intervalo_volta', '2026-10-15', '13:00', 'E', '2026-10-15 13:00 E', 'IGUAL', '')
('60', 49165, 297, 'intervalo_saida', '2026-10-01', '01:00', 'S', '2026-10-02 02:00 S', 'DIFERENTE(hora+data)', '')
('60', 49167, 297, 'intervalo_volta', '2026-10-05', '02:06', 'E', '2026-10-06 03:00 E', 'DIFERENTE(hora+data)', '')
('60', 49170, 297, 'intervalo_volta', '2026-10-17', '02:09', 'E', '2026-10-17 22:00 E', 'DIFERENTE(hora)', '')
('60', 49345, 879, 'intervalo_saida', '2026-10-23', '00:00', 'S', '2026-10-24 00:00 S', 'DIFERENTE(data)', '')
('60', 49346, 879, 'intervalo_volta', '2026-10-23', '01:00', 'E', '2026-10-24 01:00 E', 'DIFERENTE(data)', '')
('60', 49731, 879, 'intervalo_saida', '2026-10-24', '00:00', 'S', '2026-10-25 00:00 S', 'DIFERENTE(data)', '')
('60', 49732, 879, 'intervalo_volta', '2026-10-24', '01:00', 'E', '2026-10-25 01:00 E', 'DIFERENTE(data)', '')
('60', 50176, 450, 'intervalo_saida', '2026-10-25', '17:00', 'S', '2026-10-25 22:00 S', 'DIFERENTE(hora)', '')
('60', 50177, 450, 'intervalo_volta', '2026-10-25', '18:00', 'E', '2026-10-25 23:00 E', 'DIFERENTE(hora)', '')
('60', 51522, 960, 'intervalo_saida', '2026-10-21', '01:00', 'S', '2026-10-22 01:00 S', 'DIFERENTE(data)', '')
('60', 51523, 960, 'intervalo_volta', '2026-10-21', '02:00', 'E', '2026-10-22 02:00 E', 'DIFERENTE(data)', '')
('60', 51524, 960, 'intervalo_saida', '2026-10-23', '01:00', 'S', '2026-10-24 01:00 S', 'DIFERENTE(data)', '')
('60', 51525, 960, 'intervalo_volta', '2026-10-23', '02:00', 'E', '2026-10-24 02:00 E', 'DIFERENTE(data)', '')
('60', 51526, 960, 'intervalo_saida', '2026-10-25', '01:01', 'S', '2026-10-26 01:00 S', 'DIFERENTE(hora+data)', '')
('60', 51527, 960, 'intervalo_volta', '2026-10-25', '02:00', 'E', '2026-10-26 02:00 E', 'DIFERENTE(data)', '')
('60', 51528, 960, 'intervalo_saida', '2026-10-27', '01:00', 'S', '2026-10-28 01:00 S', 'DIFERENTE(data)', '')
('60', 51529, 960, 'intervalo_volta', '2026-10-27', '02:00', 'E', '2026-10-28 02:00 E', 'DIFERENTE(data)', '')
('60', 51530, 960, 'intervalo_saida', '2026-09-25', '01:00', 'S', '2026-09-26 01:00 S', 'DIFERENTE(data)', '')
('60', 51531, 960, 'intervalo_volta', '2026-09-25', '02:00', 'E', '2026-09-26 02:00 E', 'DIFERENTE(data)', '')
('60', 51532, 960, 'intervalo_saida', '2026-09-27', '01:00', 'S', '2026-09-28 01:00 S', 'DIFERENTE(data)', '')
('60', 51533, 960, 'intervalo_volta', '2026-09-27', '02:00', 'E', '2026-09-28 02:00 E', 'DIFERENTE(data)', '')
('60', 51534, 960, 'intervalo_saida', '2026-09-29', '01:00', 'S', '2026-09-30 01:00 S', 'DIFERENTE(data)', '')
('60', 51535, 960, 'intervalo_volta', '2026-09-29', '02:00', 'E', '2026-09-30 02:00 E', 'DIFERENTE(data)', '')
('60', 51536, 960, 'intervalo_saida', '2026-10-01', '01:00', 'S', '2026-10-02 01:00 S', 'DIFERENTE(data)', '')
('60', 51537, 960, 'intervalo_volta', '2026-10-01', '02:00', 'E', '2026-10-02 02:00 E', 'DIFERENTE(data)', '')
('60', 51538, 960, 'intervalo_saida', '2026-10-03', '01:00', 'S', '2026-10-04 01:00 S', 'DIFERENTE(data)', '')
('60', 51539, 960, 'intervalo_volta', '2026-10-03', '02:00', 'E', '2026-10-04 02:00 E', 'DIFERENTE(data)', '')
('60', 51540, 960, 'intervalo_saida', '2026-10-05', '01:00', 'S', '2026-10-06 01:00 S', 'DIFERENTE(data)', '')
('60', 51541, 960, 'intervalo_volta', '2026-10-05', '02:00', 'E', '2026-10-06 02:00 E', 'DIFERENTE(data)', '')
('60', 51542, 960, 'intervalo_saida', '2026-10-09', '01:00', 'S', '2026-10-10 01:00 S', 'DIFERENTE(data)', '')
('60', 51543, 960, 'intervalo_volta', '2026-10-09', '02:00', 'E', '2026-10-10 02:00 E', 'DIFERENTE(data)', '')
('60', 51544, 960, 'intervalo_saida', '2026-10-11', '01:00', 'S', '2026-10-12 01:00 S', 'DIFERENTE(data)', '')
('60', 51545, 960, 'intervalo_volta', '2026-10-11', '02:00', 'E', '2026-10-12 02:00 E', 'DIFERENTE(data)', '')
('60', 51546, 960, 'intervalo_saida', '2026-10-13', '01:00', 'S', '2026-10-14 01:00 S', 'DIFERENTE(data)', '')
('60', 51547, 960, 'intervalo_volta', '2026-10-13', '02:00', 'E', '2026-10-14 02:00 E', 'DIFERENTE(data)', '')
('60', 51548, 960, 'intervalo_saida', '2026-10-15', '01:00', 'S', '2026-10-16 01:00 S', 'DIFERENTE(data)', '')
('60', 51549, 960, 'intervalo_volta', '2026-10-15', '02:00', 'E', '2026-10-16 02:00 E', 'DIFERENTE(data)', '')
('60', 51550, 960, 'intervalo_saida', '2026-10-17', '01:00', 'S', '2026-10-18 01:00 S', 'DIFERENTE(data)', '')
('60', 51551, 960, 'intervalo_volta', '2026-10-17', '02:00', 'E', '2026-10-18 02:00 E', 'DIFERENTE(data)', '')
('60', 51739, 947, 'intervalo_volta', '2026-10-29', '13:30', 'E', '2026-10-29 13:00 E', 'DIFERENTE(hora)', '')
('60', 55142, 852, 'intervalo_saida', '2026-11-07', '01:00', 'S', '2026-11-08 01:00 S', 'DIFERENTE(data)', '')
('60', 55156, 852, 'intervalo_volta', '2026-11-07', '02:00', 'E', '2026-11-08 02:00 E', 'DIFERENTE(data)', '')
('60', 56382, 549, 'intervalo_saida', '2026-11-10', '13:20', 'S', '2026-11-10 13:20 S', 'IGUAL', '')
('103', 4695, 460, 'intervalo_saida', '2026-07-28', '01:10', 'S', '2026-07-29 00:00 S', 'DIFERENTE(hora+data)', '')
('103', 4797, 104, 'intervalo_saida', '2026-07-18', '12:00', 'S', '2026-07-18 12:00 S', 'IGUAL', '')
('103', 4798, 104, 'intervalo_volta', '2026-07-18', '13:00', 'E', '2026-07-18 13:00 E', 'IGUAL', '')
('103', 4800, 366, 'intervalo_volta', '2026-08-01', '03:00', 'E', '2026-08-02 03:00 E', 'DIFERENTE(data)', '')
('103', 5040, 468, 'intervalo_saida', '2026-07-28', '20:42', 'S', '2026-07-28 20:42 S', 'IGUAL', '')
('103', 5346, 171, 'intervalo_saida', '2026-07-01', '12:28', 'S', '2026-07-01 11:00 S', 'DIFERENTE(hora)', '')
('103', 5367, 859, 'intervalo_saida', '2026-07-21', '03:00', 'S', '2026-07-22 03:00 S', 'DIFERENTE(data)', '')
('103', 6228, 98, 'intervalo_volta', '2026-07-16', '11:15', 'E', '2026-07-16 11:15 E', 'IGUAL', '')
('103', 7463, 468, 'intervalo_volta', '2026-08-06', '21:12', 'E', '2026-08-06 18:00 E', 'DIFERENTE(hora)', '')
('103', 8778, 865, 'intervalo_volta', '2026-08-03', '01:00', 'E', '2026-08-04 01:00 E', 'DIFERENTE(data)', '')
('103', 9485, 468, 'intervalo_volta', '2026-08-10', '21:02', 'E', '2026-08-10 18:00 E', 'DIFERENTE(hora)', '')
('103', 12770, 334, 'intervalo_saida', '2026-08-12', '02:13', 'S', '2026-08-13 02:13 S', 'DIFERENTE(data)', '')
('103', 12771, 825, 'intervalo_volta', '2026-08-12', '18:01', 'E', '2026-08-12 18:00 E', 'DIFERENTE(hora)', '')
('103', 13033, 338, 'intervalo_saida', '2026-07-28', '12:10', 'S', '2026-07-28 12:10 E', 'DIFERENTE(tipo)', '')
('103', 14356, 334, 'intervalo_volta', '2026-08-04', '03:00', 'E', '2026-08-05 03:00 E', 'DIFERENTE(data)', '')
('103', 14359, 334, 'intervalo_saida', '2026-08-04', '02:00', 'S', '2026-08-05 02:00 S', 'DIFERENTE(data)', '')
('103', 14363, 334, 'intervalo_saida', '2026-08-06', '02:10', 'S', '2026-08-07 02:10 S', 'DIFERENTE(data)', '')
('103', 14365, 334, 'intervalo_volta', '2026-08-10', '03:00', 'E', '2026-08-11 03:00 E', 'DIFERENTE(data)', '')
('103', 14368, 334, 'intervalo_saida', '2026-08-10', '02:00', 'S', '2026-08-11 02:00 S', 'DIFERENTE(data)', '')
('103', 14953, 843, 'intervalo_saida', '2026-08-18', '00:00', 'S', '-', 'NAO_GRAVA(invalida)', 'Pergunta #14953: 00:00 e o default do relogio e nao casa marco do dia (03:00, 04')
('103', 14960, 334, 'intervalo_volta', '2026-08-14', '04:00', 'E', '2026-08-15 04:00 E', 'DIFERENTE(data)', '')
('103', 14963, 334, 'intervalo_saida', '2026-08-14', '03:00', 'S', '2026-08-15 03:00 S', 'DIFERENTE(data)', '')
('103', 15014, 107, 'intervalo_saida', '2026-08-19', '14:00', 'S', '2026-08-19 14:00 E', 'DIFERENTE(tipo)', '')
('103', 16327, 881, 'intervalo_volta', '2026-08-25', '01:00', 'E', '2026-08-25 01:00 E', 'IGUAL', '')
('103', 20890, 107, 'intervalo_saida', '2026-08-10', '14:00', 'S', '2026-08-10 14:00 E', 'DIFERENTE(tipo)', '')
('103', 20960, 72, 'intervalo_saida', '2026-08-26', '23:00', 'S', '2026-08-26 23:00 S', 'IGUAL', '')
('103', 22470, 881, 'intervalo_volta', '2026-08-23', '01:00', 'E', '2026-08-23 01:00 E', 'IGUAL', '')
('103', 22586, 444, 'intervalo_volta', '2026-08-29', '16:00', 'E', '2026-08-29 16:00 E', 'IGUAL', '')
('103', 22627, 489, 'intervalo_volta', '2026-08-30', '01:00', 'E', '2026-08-31 02:00 E', 'DIFERENTE(hora+data)', '')
('103', 23137, 749, 'intervalo_saida', '2026-09-04', '14:03', 'S', '2026-09-04 14:03 S', 'IGUAL', '')
('103', 26588, 107, 

[truncado em 20000 de 28681 caracteres]

## 13/11 19:42 · sessao 8a778cbd3dc4 · E3-FORMA-SQL · verificador · origem=Code

**Veredito geral: BLOQUEIA.** A cura mantém exceções que deixam chamado vivo da fila sem porta e introduz dois critérios para o significado de `fio`. ([tarefa_do_dia.py:562](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:562), [tarefa_do_dia.py:572](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:572))

**1. BLOQUEIA — cumprimento parcial do aval (ii).**

O ramo novo não cria ação nem encerra chamado: monta portas e atualiza o resultado em memória. O encerramento persistente continua passando por `encerrar → _gravar_estado`. ([tarefa_do_dia.py:563](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:563), [models.py:1620](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/models.py:1620))

Entretanto, chamado vivo da fila continua sem porta quando o dia tem `razao` ou existe ação com porta textual de sistema. Isso preserva a omissão no leitor, embora não mate o chamado no banco. A recuperação exige simultaneamente `not out['razao']` e **todas** as ações com `porta is None`. ([tarefa_do_dia.py:562](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:562))

Também permanece a exclusão do chamado residual A quando existe ação sob B: o teste exige expressamente que A não apareça em `verbos`. Nesse caso o dia conserva porta por B, mas A não é publicado por ela. ([test_e3_portas_tarefa.py:44](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/tests/test_e3_portas_tarefa.py:44))

A restauração dos 79 casos de produção não está demonstrada pelo material fornecido. **[nao verificado]**

**2. BLOQUEIA — a união de âncoras também amplia a antiga porta de pergunta.**

Há duas comparações distintas:

- Contra `_portas_da_tarefa` anterior, os três testes novos exigem portas antes ausentes: marco aceso, passivo com âncora própria e passivo com âncora externa. ([test_e3_portas_tarefa.py:146](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/tests/test_e3_portas_tarefa.py:146), [test_e3_portas_tarefa.py:164](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/tests/test_e3_portas_tarefa.py:164), [test_e3_portas_tarefa.py:181](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/tests/test_e3_portas_tarefa.py:181))
- Contra a antiga `porta_pergunta` do C17, a classe é mais ampla: C17 exige pergunta viva, presença no app e ausência de resposta carimbada vazia; a regra nova exige somente pergunta exposta pela tarefa e sua âncora em `fila_ids`. ([juizes.py:1603](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/juizes.py:1603), [juizes.py:1681](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/juizes.py:1681), [tarefa_do_dia.py:566](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:566))

Uma classe concreta do código: pergunta validada, lavra `sim`, sem efeito, classificada `encerrada_pelo_juiz`, com ações vazias. A tarefa conserva seu `pergunta_id`; C6 rejeita pergunta validada; a regra nova recupera sua âncora externa da fila e publica `porta_pergunta`. Portanto, abre porta para um dia ao qual C17 não dava essa porta. ([tarefa_do_dia.py:144](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:144), [tarefa_do_dia.py:308](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:308), [juizes.py:1389](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/juizes.py:1389), [tarefa_do_dia.py:565](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:565))

Isso excede **manter a porta do ATUAL**: a âncora é viva, mas a relação adicional de alcance por pergunta encerrada não existia naquela porta. A presença dessa classe em produção e sua autorização específica no aval não estão demonstradas. **[nao verificado]**

**3. APROVA a reutilização da autoridade; BLOQUEIA a uniformidade semântica.**

`fio_pelos_verbos` não duplica a escada: é a mesma função chamada pelo C17 e pelo ramo novo. O registro vivo já declara a migração dos leitores de C17 para C19; `JUIZES.md` também aponta essa migração. ([juizes.py:1633](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/juizes.py:1633), [juizes.py:1688](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/juizes.py:1688), [core/juizes.py:132](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/core/juizes.py:132), [JUIZES.md:85](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/docs/JUIZES.md:85))

Todos os caminhos citados usam a projeção da tarefa, mas **nem todos leem `fio`**:

| Leitor | O que efetivamente lê |
|---|---|
| Folha `_fio_da_porta` / `_grupo` | `fio` literal; depois agrupa `nada→D`, `chamado→C`, `colab→B`, restante→A. ([fora_da_folha.py:86](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/folha/fora_da_folha.py:86)) |
| `vigia_fio.censo_alcance` | `rota`, `fio`; “exige admin” significa `fio == 'admin'`. ([vigia_fio.py:648](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/vigia_fio.py:648)) |
| `pautas_dos_discordantes` | Fio pela folha; seleciona `FIO_SEM_PORTA`. ([pautas_dos_discordantes.py:79](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/ponto/services/pautas_dos_discordantes.py:79)) |
| `furo_sem_canal` | Existência e rota da porta. ([furo_sem_canal.py:94](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/furo_sem_canal.py:94)) |
| `cadastro_realidade.beco` | Existência da porta. ([cadastro_realidade.py:497](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/escala/services/cadastro_realidade.py:497)) |
| `diff_e3_sombra.candidata` | Alias direto de `porta_da_tarefa`. ([diff_e3_sombra.py:43](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/management/commands/diff_e3_sombra.py:43)) |

O problema está no produtor: no caminho normal `fio` significa **prioridade dos donos das ações**; na recuperação significa **escada dos verbos do painel**. Esses critérios discordam: o teste existente admite ação `responder` com verbo `validar`; o primeiro critério produz `colab`, enquanto a escada desse verbo produz `admin`. A projeção não informa qual critério produziu o campo. ([test_e3_portas_tarefa.py:79](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/tests/test_e3_portas_tarefa.py:79), [tarefa_do_dia.py:571](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:571))

**4. BLOQUEIA — sistema+fila e razão de dia continuam sendo furos.**

Ação de sistema usa portas como `emissor`, `materializacao` e `cartorio`; essas strings impedem a recuperação pela fila. `ata_sem_lampadas` preenche a razão de dia e também a impede. São exceções à regra de visibilidade que o próprio trecho anuncia. ([tarefa_do_dia.py:149](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:149), [tarefa_do_dia.py:217](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:217), [tarefa_do_dia.py:305](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:305), [tarefa_do_dia.py:548](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:548))

Os testes afirmam expressamente: [nao verificado]

- Sistema+fila → nenhuma porta; isento+fila → nenhuma porta. ([test_e3_portas_tarefa.py:36](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/tests/test_e3_portas_tarefa.py:36), [test_e3_portas_tarefa.py:61](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/tests/test_e3_portas_tarefa.py:61))
- Folha permanece sem fio/grupo D e vigia permanece `sem_porta` no caso sistema+fila. ([test_e3_leitores_c17_chamadores.py:124](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/tests/test_e3_leitores_c17_chamadores.py:124), [test_e3_leitores_c17_chamadores.py:170](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/tests/test_e3_leitores_c17_chamadores.py:170))

Os testes localizados de `ata_sem_lampadas` afirmam razão e desconhecimento; não verificam a combinação chamado na fila + porta. ([test_e2_tarefa_do_dia.py:391](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/tests/test_e2_tarefa_do_dia.py:391), [test_e2_tarefa_do_dia.py:840](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/tests/test_e2_tarefa_do_dia.py:840))

**5. BLOQUEIA — o rótulo não acompanha a porta publicada.** [nao verificado]

`nao_sei` permanece na tarefa, mas `porta_da_tarefa` publica somente `rota`, `fila`, `verbos` e `fio`. Existe leitor que consulta também o booleano: `furo_sem_canal`; contudo, ele só o contabiliza no `elif` em que **não há porta**. Portanto, o passivo agora com porta não aparece nesse contador como `nao_sei`. ([tarefa_do_dia.py:589](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:589), [furo_sem_canal.py:95](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/furo_sem_canal.py:95))

Os testes novos provam preservação interna do booleano, não visibilidade do rótulo nos leitores. ([test_e3_portas_tarefa.py:162](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/tests/test_e3_portas_tarefa.py:162))

O aval fornecido exige “visíveis com o rótulo nao_sei”, sem nomear leitor ou formato específico; sua presença como arquivo numerado no HEAD e a demonstração dessa visibilidade não foram verificadas. **[nao verificado]**

**6. BLOQUEIA — ESMERIL-SEMÂNTICA.**

| Campo/valor | Quem escreve ou fornece | Quem lê; significado |
|---|---|---|
| Ação `porta` | `_acao`; `_pela_decisao` fornece âncora inteira/`None`; ações de sistema usam strings. | `_portas_da_tarefa` distingue essas três formas. ([tarefa_do_dia.py:115](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:115), [tarefa_do_dia.py:128](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:128), [tarefa_do_dia.py:561](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:561)) |
| `portas` | Conjunto local escrito por `_portas_da_tarefa`: ações inteiras; depois, condicionalmente, união célula/âncoras. | Produz `verbos`, `da_celula`, `fila` e `rota`; não é estado persistido. ([tarefa_do_dia.py:561](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:561)) |
| `fila_ids` | `alimentar_fila`, lendo `qs_na_fila` e excluindo IDs ausentes do mapa de verbos. | Tarefa, C17, vigia e diff: pertença à fila com verbo disponível. ([juizes.py:1559](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/juizes.py:1559), [vigia_fio.py:609](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/vigia_fio.py:609), [diff_e3_sombra.py:66](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/management/commands/diff_e3_sombra.py:66)) |
| `chamado_que_lista` | Função pura que retorna destino, senão pai; não grava vínculo. | `_pela_decisao`, carga e novo mapa de âncoras usam esse ID. ([fio.py:97](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/fio.py:97), [tarefa_do_dia.py:416](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:416), [tarefa_do_dia.py:535](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:535)) |
| `fio` | Tarefa: donos das ações **ou** escada dos verbos. C17: escada dos verbos. | Projeção e leitores do item 3 recebem o mesmo nome, sem distinção de origem semântica. ([tarefa_do_dia.py:572](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:572), [juizes.py:1688](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/juizes.py:1688)) |
| `nao_sei` | Corpo puro inicializa e marca desconhecimento. | Composição do fio e leitores diretos da tarefa; excluído da projeção de porta. ([tarefa_do_dia.py:185](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:185), [tarefa_do_dia.py:312](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:312), [tarefa_do_dia.py:589](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:589)) |

O contrato do módulo ainda descreve `fila/verbos` como portas exigíveis e `fio` exclusivamente como resumo dos donos das ações, sem enumerar `chamado`. A documentação não acompanhou a ampliação. ([tarefa_do_dia.py:38](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:38))

**7. APROVA a mordida conjunta dos mutantes na tarefa, com limites.**

| Teste novo | Reverter `_portas_da_tarefa` | Fixar o fio novo em `admin` | [nao verificado]
|---|---|---|
| Marco aceso | Morde pela porta exigida. | Morde: o esperado é a escada do verbo `decidir`, isto é, `chamado`. ([test_e3_portas_tarefa.py:138](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/tests/test_e3_portas_tarefa.py:138)) |
| Passivo da célula | Morde pela porta exigida. | Não separa o mutante quando o verbo esperado é `validar→admin`. ([test_e3_portas_tarefa.py:157](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/tests/test_e3_portas_tarefa.py:157)) |
| Passivo com âncora externa | Morde pela `porta_pergunta` exigida. | Não afirma `fio`. ([test_e3_portas_tarefa.py:179](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/tests/test_e3_portas_tarefa.py:179)) |

O helper F5B preserva a proteção contra universo errado e falso `admin`: mantém verbo literal `decidir`, fio literal `chamado`, exige igualdade completa tarefa/C17 e verifica o fio da folha. Não perdeu essa mordida; substituiu a antiga expectativa de invisibilidade. ([test_o261e_f5b_juiz_porta.py:232](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/tests/test_o261e_f5b_juiz_porta.py:232))

Há uma ressalva ao alvo do mutante: se a própria **função compartilhada** `fio_pelos_verbos` for fixada em `admin`, os dois novos asserts que a usam como oráculo ficam correlacionados com a implementação. F5B conserva expectativas literais que mordem essa alteração. ([test_e3_portas_tarefa.py:148](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/tests/test_e3_portas_tarefa.py:148), [test_o261e_f5b_juiz_porta.py:293](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/tests/test_o261e_f5b_juiz_porta.py:293))

Execução RED em `91bed1694`, GREEN dos testes Django e suite inteira: **[nao verificado]**.

**8. BLOQUEIA — classes reprovadoras presentes e ausentes.**

Está presente **campo com dois critérios semânticos**, acompanhado de contrato documental divergente. Também está presente cobertura que ainda exige omissão de vivo nas exceções sistema/razão de dia. ([tarefa_do_dia.py:38](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:38), [tarefa_do_dia.py:562](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:562), [tarefa_do_dia.py:572](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:572))

A chamada nova não constitui, por si, segundo juiz: reutiliza a escada existente. O trecho novo tampouco introduz escritor persistente ou relógio interno; recebe a alimentação carregada no `agora` da carga. ([tarefa_do_dia.py:530](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:530), [tarefa_do_dia.py:575](/home/ronald/codex_ws/e01da1d515e9f6744caf4d5b4efd74e5e8b3e0fa/app/chamados/services/tarefa_do_dia.py:575))

Ordem operacional de trava, pouso, complemento E4b com medição dos 25, mescla com `9e2c540` e opus final: **[nao verificado]**.

## 13/11 19:38 · sessao c56b8a116909 · - · - · origem=Ronald (tmux)

1. **Aval (ii): parcial, bloqueante nas exceções.** O delta publica porta residual apenas sem razão de dia e com todas as ações tendo `porta is None`; razão de dia e ação do sistema com porta textual continuam suprimindo a porta do vivo da fila. `ata_sem_lampadas` escreve razão de dia. (app/chamados/services/tarefa_do_dia.py:217,562-567; portas do sistema:149-151,301-305)

2. **Não cria ação nem encerra chamado.** O novo ramo só monta conjuntos/mapas e atualiza `out`; ações vêm de `_acao` e do julgamento anterior. Encerramento persistente continua em `ChamadoColaborador.encerrar → _gravar_estado`. (app/chamados/services/tarefa_do_dia.py:115-116,535-578; app/chamados/models.py:1606-1620,201-228)

3. **Classe nova além da antiga porta de pergunta:** pergunta exposta pela tarefa, sem ação com porta, cuja âncora está na fila, mesmo sem cumprir C6/app. C17 exigia pergunta viva, não respondida sem texto e presença no app; o fallback novo exige apenas exposição do `pergunta_id` e âncora na fila. (app/chamados/juizes.py:1599-1604,1679-1682; app/chamados/services/tarefa_do_dia.py:565-567)

   Um recorte concreto definido pelo código é pergunta validada, com lavra `sim`, sem efeito, razão `encerrada_pelo_juiz`, ações vazias: a tarefa conserva `pergunta_id`, mas C6 rejeita pergunta validada. Se a âncora está em outra célula, o fallback dá porta de pergunta que C17 não dava. A existência dessa classe em produção é **[não verificado]**. (app/chamados/services/tarefa_do_dia.py:144-152,307-314,565-577; app/chamados/juizes.py:1389-1392)

4. **`fio_pelos_verbos` não é novo juiz isoladamente:** é a função existente de C17, agora reutilizada; o registro vivo declara migração dos leitores C17 para C19. Entretanto, `fio` passa a ter dois critérios na própria tarefa: dono da ação no caminho principal; escada do verbo no fallback. O contrato do módulo ainda diz que `fio` resume os donos das ações e nem enumera `chamado`. (app/chamados/juizes.py:1633-1639; app/core/juizes.py:132-138,153-159; app/chamados/services/tarefa_do_dia.py:38-40,571-578)

5. **Mapa dos valores:**

| Valor | Escritor/fonte | Leitores relevantes |
|---|---|---|
| `portas` | Set local: ações inteiras ∩ `fila_ids`; fallback união célula/âncoras ∩ `fila_ids`. | `verbos`, `da_celula`, `rota`; não é campo persistente. (app/chamados/services/tarefa_do_dia.py:561-577) |
| Ação `porta` | `_acao`; âncora inteira/None em `_pela_decisao`; portas de sistema são strings. | `_portas_da_tarefa` diferencia inteiro, None e string. (app/chamados/services/tarefa_do_dia.py:115-116,128-163,301-305,561-562) |
| `fila_ids` | `alimentar_fila`: IDs de `qs_na_fila` que também possuem verbo; exclui ausência de verbo por corrida. | Tarefa, C17, vigia e instrumento de diff. (app/chamados/juizes.py:1559-1567,1681; app/chamados/services/tarefa_do_dia.py:561,567; app/chamados/services/vigia_fio.py:609; app/chamados/management/commands/diff_e3_sombra.py:66) |
| `chamado_que_lista` | Função que devolve destino, senão pai; não grava estado. | Carga, `_pela_decisao`, mapa novo de âncoras e C17. (app/chamados/services/fio.py:97-117; app/chamados/services/tarefa_do_dia.py:128-130,416-419,535-536; app/chamados/juizes.py:1680) |
| `fio` | Tarefa ou C17; fallback usa helper existente. | Projeção de porta publica o valor, sem recalcular. (app/chamados/services/tarefa_do_dia.py:572-589; app/chamados/juizes.py:1688) |
| `nao_sei` | Corpo puro inicializa e marca desconhecimento. | `_portas_da_tarefa` lê para fio sem fallback; `porta_da_tarefa` elimina o rótulo da projeção. (app/chamados/services/tarefa_do_dia.py:185-186,208,217,253,275,279,299,312,573,589) |

6. **Classes reprovadoras:** há campo com dois critérios e documentação divergente; há omissão residual de vivo nas classes explicitamente bloqueadas pelo fallback. O delta não introduz escritor persistente nem relógio novo; comprovação de merge, ordem operacional, suite inteira e opus final é **[não verificado]**. (app/chamados/services/tarefa_do_dia.py:38-40,530-578,601-607)

## 13/11 19:38 · sessao 8252db73f2fc · - · - · origem=Ronald (tmux)

1. **Q4 — BLOQUEIA pela regra literal do aval.** A condição `all(a['porta'] is None...)` exclui ação de sistema com `porta='emissor'`, mesmo havendo chamado vivo na fila; `not out['razao']` exclui qualquer razão de dia, inclusive `ata_sem_lampadas`. São exclusões expressas, sem encerramento do chamado: `app/chamados/services/tarefa_do_dia.py:561-567`, `:217`.

   Os testes afirmam a exclusão sistema+fila em `app/chamados/tests/test_e3_portas_tarefa.py:36-42` e isento+fila em `:61-69`. Os leitores preservam essa exclusão: folha sem fio/grupo D em `app/chamados/tests/test_e3_leitores_c17_chamadores.py:124-140`; vigia `sem_porta` em `:170-175`. Os testes localizados de `ata_sem_lampadas` afirmam razão/`nao_sei`, mas não chamado na fila nem ausência de porta: `app/chamados/tests/test_e2_tarefa_do_dia.py:391-394`, `:840-847`.

2. **Q7 — APROVA a mordida conjunta dos mutantes indicados, com limites.**

   | Teste novo | Voltar ao anterior | Fixar fio do fallback em `admin` | [nao verificado]
   |---|---|---|
   | Marco aceso | Morde: exige rota/fila/verbos | Morde: sem pergunta → `decidir` → `chamado` |
   | Passivo da célula | Morde: exige rota/fila/verbos | Sobrevive: resposta com texto → `validar` → `admin` |
   | Passivo com âncora externa | Morde: exige `porta_pergunta` | Sobrevive: não compara `fio` |

   As asserções ficam em `app/chamados/tests/test_e3_portas_tarefa.py:146-148`, `:162-166`, `:179-181`. O primeiro não cria pergunta (`:138-140`); os passivos escrevem projeção respondida via `app/chamados/tests/e3_fixtures.py:75-76`. O crédito transforma resposta com texto em `validar`: `app/chamados/services/raiox.py:139-155`, `app/chamados/catalogo/natureza.py:48-49`, `:59`; a escada resulta `admin`/`chamado` em `app/chamados/juizes.py:1628-1639`.

3. **Helper F5B não perdeu a mordida contra universo errado ou `admin` falso.** Conserva comparação com verbo do painel, literal `{c.pk: 'decidir'}` e literal fio C17 `'chamado'`; passa a exigir igualdade completa entre porta da tarefa e C17, além de literal fio da folha `'chamado'`. Reversão anterior falha na igualdade de porta; fallback `admin` falha no fio. Fontes: `app/chamados/tests/test_o261e_f5b_juiz_porta.py:232-247`; os dois casos acionadores são destino silenciado e registrado em `:249-255`. A expectativa antiga `None`/`nada` foi substituída conforme o delta fornecido; isso abandona a exclusão anterior, não a proteção contra crédito errado. [anterior: dado fornecido, sem arquivo HEAD]

4. **Limite da verificação:** executei somente o corpo extraído por AST em memória, sem importar projeto/ORM ou escrever arquivos; não executei os testes Django. A simulação confirmou a matriz e as exclusões do corpo em `app/chamados/services/tarefa_do_dia.py:560-578`. Execução RED/GREEN dos testes reais e suite inteira: **[não verificado]**.

## 13/11 19:37 · sessao 25e4214905f7 · - · - · origem=Ronald (tmux)

1. **Q3 — APROVA quanto a não duplicar a escada.** O delta chama a função existente `fio_pelos_verbos`, também usada pelo C17; não copia sua tradução `cobrar→colab`, `validar→admin`, demais→chamado. A migração dos leitores para C19 já consta no registro. (`app/chamados/services/tarefa_do_dia.py:574-576`; `app/chamados/juizes.py:1628-1639,1688`; `app/core/juizes.py:132-159,293-295`; `app/docs/JUIZES.md:78-87`.)

2. **Q3 — ressalva semântica material:** `fio` continua resumindo donos de ações no caminho normal, mas passa a resumir verbos do painel no fallback. Esses critérios podem discordar: o teste existente fixa ação `responder` com verbo `validar`; o caminho normal produz `colab`, enquanto a escada daquele verbo produz `admin`. A diferença fica sem indicador de origem na projeção. (`app/chamados/services/tarefa_do_dia.py:571-589`; `app/chamados/tests/test_e3_portas_tarefa.py:79-86`; `app/chamados/juizes.py:1628-1639`.)

3. **Todos usam a mesma projeção, mas não todos leem `fio`:**

   - Folha devolve literalmente `porta['fio']`; o agrupamento aplica `nada→D`, `chamado→C`, `colab→B`, restante→A. (`app/folha/fora_da_folha.py:71-103`.)
   - Vigia lê `rota` e `fio`, e define `alcance_porta_exige_o_admin` por `fio == 'admin'`. (`app/chamados/services/vigia_fio.py:640-654`.)
   - Pauta lê o fio pela folha e seleciona somente `FIO_SEM_PORTA`. (`app/ponto/services/pautas_dos_discordantes.py:62-84`.)
   - Furo lê existência e rota da porta; beco lê somente existência. (`app/chamados/services/furo_sem_canal.py:94-101`; `app/escala/services/cadastro_realidade.py:492-499`.)
   - `diff_e3_sombra.candidata` é alias direto de `porta_da_tarefa`. (`app/chamados/management/commands/diff_e3_sombra.py:41-59`.)

4. **Não confundir autoridades com nomes parecidos.** A entrada “FIO / DISPUTA” aponta para um lookup de disputa aberta, não para a classificação `admin/colab/chamado`. `fio_vivo` responde outra pergunta: booleano `any(pergunta_viva(...))`. (`app/docs/JUIZES.md:40`; `app/chamados/services/fio.py:120-137`; `app/chamados/juizes.py:1494-1503`; `app/core/juizes.py:291`.)

5. **Q5 — BLOQUEIA a demonstração de visibilidade do rótulo.** `porta_da_tarefa` omite `nao_sei`. Existe leitor de porta que também lê esse campo: `furo_sem_canal`; contudo, ele só o conta quando **não existe porta**. Portanto, o novo passivo com porta não publica nem contabiliza seu rótulo por esse leitor. Os testes novos verificam o booleano na tarefa, não sua exposição a jusante. (`app/chamados/services/tarefa_do_dia.py:589`; `app/chamados/services/furo_sem_canal.py:94-113`; `app/chamados/tests/test_e3_portas_tarefa.py:162-166,179-181`.)

6. O aval fornecido exige casos “visíveis com o rótulo nao_sei”; não especifica qual leitor ou formato deve recebê-lo. A conformidade apenas pela preservação interna não fica demonstrada. [não verificado: anexo `aval.txt` fornecido no prompt, sem arquivo local numerado.]

## 13/11 18:45 · sessao 963ccf45aa88 · HJ-090 · construtor · origem=Code

Curas e testes escritos. Suíte e mordidas reais dos mutantes: **[nao verificado]**, sem execução nesta volta. [nao verificado]

A [entrega completa](/home/ronald/wt-codex-hj-090-152256/app/HJ090_VOLTA5_ENTREGA.txt:6) lista os **12 arquivos alterados/adicionados** e contém o comando individual de cada teste e mutante. [nao verificado]

- **C:** valida cada identidade interna, incluindo tipos e datas ISO canônicas, antes das reversões — [command:67](/home/ronald/wt-codex-hj-090-152256/app/ferias/management/commands/realinhar_concedidos_ferias.py:67).
- **PA apagado:** confere e trava todos os PAs antes do primeiro ato; lista todos os ausentes e recusa o arquivo. A reversão passou a usar transação de lote — [command:119](/home/ronald/wt-codex-hj-090-152256/app/ferias/management/commands/realinhar_concedidos_ferias.py:119). [nao verificado]
- **D:** criação e edição usam a admissão como âncora comum. Fora dos aniversários, o próprio início ancora o ciclo, conforme docstring — [services:752](/home/ronald/wt-codex-hj-090-152256/app/ferias/services.py:752), [views:167](/home/ronald/wt-codex-hj-090-152256/app/ferias/views.py:167).
- **Colisão:** traduz `ValidationError` da guarda existente para `RecusaNaPorta`, dentro da transação — [services:787](/home/ronald/wt-codex-hj-090-152256/app/ferias/services.py:787).
- **Relógio:** `hoje` obrigatório nas portas, com chamadores produtivos e testes anteriores ajustados — [services:499](/home/ronald/wt-codex-hj-090-152256/app/ferias/services.py:499), [services:636](/home/ronald/wt-codex-hj-090-152256/app/ferias/services.py:636), [entrega:25](/home/ronald/wt-codex-hj-090-152256/app/HJ090_VOLTA5_ENTREGA.txt:25). [nao verificado]

Mapa preparado **teste → mutante**; confirmação RED de cada mordida: **[nao verificado]**. Os comandos individuais estão na [entrega:35](/home/ronald/wt-codex-hj-090-152256/app/HJ090_VOLTA5_ENTREGA.txt:35). [nao verificado]

| Teste | Mutantes | [nao verificado]
|---|---|
| [Identidade inválida:42](/home/ronald/wt-codex-hj-090-152256/app/ferias/test_hj090_volta5.py:42) | `c_sem_identidade_interna`, `c_sem_data_valida` |
| [PA apagado:61](/home/ronald/wt-codex-hj-090-152256/app/ferias/test_hj090_volta5.py:61) | `pa_sem_preflight`, `pa_so_um_ausente` |
| [Outro PA não bloqueia:79](/home/ronald/wt-codex-hj-090-152256/app/ferias/test_hj090_volta5.py:79) | `b_sem_filtro_pa` |
| [Outro PA não libera replay:87](/home/ronald/wt-codex-hj-090-152256/app/ferias/test_hj090_volta5.py:87) | `b_sem_guarda_ato` |
| [Criação 29/02:137](/home/ronald/wt-codex-hj-090-152256/app/ferias/test_hj090_volta5.py:137) | `d_criacao_conta_independente`, `d_edicao_sem_ancora` |
| [Criação 31/01:140](/home/ronald/wt-codex-hj-090-152256/app/ferias/test_hj090_volta5.py:140) | `d_criacao_364`, `d_fim_364`, `d_limite_365` |
| [Início fora do aniversário:143](/home/ronald/wt-codex-hj-090-152256/app/ferias/test_hj090_volta5.py:143) | `d_forca_aniversario` |
| [Colisão sem escrita:154](/home/ronald/wt-codex-hj-090-152256/app/ferias/test_hj090_volta5.py:154) | `colisao_sem_traducao` |
| [Hoje no agendamento:172](/home/ronald/wt-codex-hj-090-152256/app/ferias/test_hj090_volta5.py:172) | `hoje_ag_aviso`, `hoje_ag_status_pa` |
| [Hoje no intake:180](/home/ronald/wt-codex-hj-090-152256/app/ferias/test_hj090_volta5.py:180) | `hoje_intake_nao_propaga`, `hoje_intake_abono` |
| [Hoje nas duas portas de troca:191](/home/ronald/wt-codex-hj-090-152256/app/ferias/test_hj090_volta5.py:191) | `hoje_troca_aviso`, `hoje_troca_status_ag`, `hoje_troca_status_pa`, `hoje_remarcar_nao_propaga`, `hoje_corrigir_nao_propaga` |

## 13/11 18:41 · sessao 0edada80a8c4 · JUSTIFICATIVA-PAGA-O-PERIODO · sonda · origem=Code

Não fiz nova execução: o envelope reescreve credenciais, cria arquivo temporário e acrescenta uma trilha, contrariando a regra de não escrever arquivos (`bin/sonda_leitura.sh:126–134`, `bin/sonda_leitura.sh:216–218`, `bin/sonda_leitura.sh:248–250`).

Segue a transcrição literal inteira da saída fornecida, sem validação independente da execução. [nao verificado]

```text
sonda_leitura: sonda.py [3f1bd3f83f2f34bc] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

UNIVERSO 45
CHAVES_LAMPADA ['acesa', 'dispensada', 'hora', 'luz', 'tipo', 'tipo_real']
MULT just_aprovadas_mesmo_tipo>=2 0 ancoras_distintas_mesmo_tipo>=2 0 lampadas_dispensadas_mesmo_marco>=2 28 sem_justificativa_aprovada_no_dia 0
HIST (n_just_aprovadas, n_lampadas_dispensadas) -> [((1, 1), 17), ((1, 2), 1), ((1, 3), 27)]
CASO (118689, 56, '2026-10-07', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'E': 1}, 2)
CASO (140889, 56, '2026-10-25', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'E': 1}, 2)
CASO (127808, 976, '2026-10-26', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (112074, 567, '2026-10-12', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (135337, 496, '2026-10-22', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 1}, 2)
CASO (3283, 408, '2026-08-17', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (114134, 206, '2026-09-22', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (2261, 855, '2026-08-18', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (110059, 341, '2026-10-07', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 1}, 2)
CASO (108830, 340, '2026-10-08', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (24651, 697, '2026-09-01', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'E': 1, 'S': 2}, 4)
CASO (24660, 697, '2026-09-10', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (140580, 750, '2026-10-26', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 1}, 2)
CASO (113955, 438, '2026-09-23', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (112352, 347, '2026-09-30', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 1}, 2)
CASO (112358, 347, '2026-10-06', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 1}, 2)
CASO (106510, 922, '2026-09-28', {'outro': 1}, {'outro': 1}, {'S': 1}, 2)
CASO (106526, 922, '2026-10-14', {'outro': 1}, {'outro': 1}, {'S': 1}, 2)
CASO (110352, 389, '2026-09-30', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 1}, 2)
CASO (105323, 789, '2026-10-11', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (8127, 518, '2026-07-25', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (8131, 518, '2026-07-29', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'E': 1, 'S': 2}, 4)
CASO (8148, 518, '2026-08-15', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'E': 1, 'S': 2}, 4)
CASO (8150, 518, '2026-08-17', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (141802, 996, '2026-10-26', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (131421, 912, '2026-10-23', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (105365, 938, '2026-09-23', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 1}, 2)
CASO (33812, 564, '2026-09-24', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (116944, 358, '2026-10-02', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (121745, 962, '2026-10-25', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2}, 4)
CASO (105667, 931, '2026-09-25', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'E': 1}, 2)
CASO (33879, 310, '2026-09-21', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'E': 2, 'S': 1}, 4)
CASO (21683, 228, '2026-09-14', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 1}, 2)
CASO (104743, 228, '2026-10-01', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'E': 1}, 2)
CASO (113727, 468, '2026-10-05', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (116515, 829, '2026-09-23', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'E': 2, 'S': 1}, 4)
CASO (106125, 331, '2026-10-03', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (118347, 643, '2026-09-25', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 1}, 2)
CASO (16234, 510, '2026-08-10', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (129875, 932, '2026-10-27', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 1}, 2)
CASO (143004, 1004, '2026-10-27', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'E': 2, 'S': 1}, 4)
CASO (111910, 396, '2026-09-28', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 1}, 2)
CASO (1013, 863, '2026-08-10', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (1021, 863, '2026-08-18', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
CASO (1023, 863, '2026-08-20', {'esqueci_saida': 1}, {'esqueci_saida': 1}, {'S': 2, 'E': 1}, 4)
FIM
```

1. **Contagens impressas para 2+ no mesmo colab-dia:**

   | Medida | Casos |
   |---|---:|
   | Justificativas aprovadas do mesmo tipo | 0 [nao verificado] |
   | Âncoras `batida_entrada` distintas do mesmo tipo | 0 [nao verificado] |
   | Lâmpadas dispensadas agrupadas pelo tipo do marco | 28 [nao verificado] |

   **A medida de âncoras distintas é a que corresponde a episódios/turnos distintos entre as três:** `batida_entrada` nomeia a “Batida de entrada do turno com problema”, e a identidade declarada da justificativa combina colaborador, data e essa âncora (`app/ponto/models.py:139–144`, `app/ponto/models.py:186–187`).

   A lâmpada nomeia o **marco** por `tipo` e `hora`, mantendo separadamente `tipo_real` da batida (`app/ponto/services/cartorio.py:648–650`, `app/ponto/services/cartorio.py:706–713`). Portanto, os agrupamentos impressos `E`/`S` mostram repetição de **tipo de marco**, sem demonstrar repetição do mesmo marco completo `(tipo, hora)` nem de episódios. [nao verificado] Uma justificativa pode acompanhar várias lâmpadas dispensadas porque o código percorre todas as lâmpadas ainda não acesas quando o dia está justificado (`app/ponto/services/cartorio.py:802–810`).

2. **Sim, bate numericamente: 45 = 9+23+3+10**, distribuição registrada em `app/docs/BACKLOG.md:1872`; o dado fornecido declara 45, lista 45 casos e seu histograma soma 17+1+27=45. [nao verificado] A identidade dos IDs com `logs/HJ-093/sonda2/r.r` e a equivalência dos filtros não foram verificadas. [nao verificado]

3. **O dado não mostra erro nem a falha de chave exemplificada:** `CHAVES_LAMPADA` contém `tipo` e `hora`, e os agrupamentos aparecem como `E`/`S`, sem `?`; nenhuma das três medidas impressas está vazia. [nao verificado] A ausência de uma chave literalmente chamada `marco` não caracteriza falha: o código representa o marco usando `tipo` e `hora` (`app/ponto/services/cartorio.py:648–650`, `app/ponto/services/cartorio.py:711`). O corpo da sonda, o tratamento de âncoras nulas e o código de saída da execução permanecem [nao verificado].

## 13/11 18:37 · sessao 85aae53bb678 · RAIA-PREPARACAO · censo · origem=Code

**O universo global não fechou: total de costuras e total global com desencontro = [nao verificado].** A revisão individualizada abaixo resulta em **30 itens: 23 reais, 6 escritas mortas, 0 ramos mortos demonstrados e 1 falso**. Dos 23 originais, são **16 reais, 6 escritas mortas e 1 falso**; os sete adicionais repetem a omissão da data catalogada no leitor em lote (`app/chamados/juizes.py:114`, `:187`; provas individuais no quadro).

“Real” abaixo significa incompatibilidade demonstrada em caminho alcançável pelo código. Ocorrência e quantidade efetiva em produção continuam **[nao verificado]**.

**Contagem e limite do universo.** Usei AST em memória sobre `Path('app/chamados').rglob('*.py')`, excluindo componentes `tests`, `migrations`, `__pycache__` e arquivos `test_*`. Os padrões foram `ast.Dict`, `ast.DictComp`, chamadas `dict(...)`, chamadas com `keyword.arg is None`, funções com `args.kwarg`, subscritos e chamadas `.get/.setdefault/.update/.items/.values/.keys`. A busca textual complementar usou:

```text
contexto_json|merge_contexto|contexto_novo
JsonResponse|resposta_render|render\(
\*\*(kwargs|campos|chaves|extras|ln)
\.(get|pop|setdefault|update|items|values|keys)\(
```

Esse procedimento levanta **sítios candidatos**, não resolve sozinho os pares emissor→consumidor. Não concluí a expansão de todos os chamadores, apelidos, variantes, encaminhamentos integrais e leitores Python/templates/JS; portanto, não apresento sua contagem sintática como universo **[nao verificado]**.

Os sítios concretos ainda sem expansão global fechada são:

| Fronteira | Por que contar literais ou chaves nominais não fecha |
|---|---|
| Contexto por kwargs | Recebe chaves variáveis, compara e persiste o grupo: `app/chamados/services/contexto.py:62`, `:71`, `:76`, `:82`, `:100`, `:102`. |
| Contexto fundido | Percorre qualquer chave recebida e a incorpora: `app/chamados/services/contexto.py:146`, `:154`, `:156`, `:161`. |
| Contexto da disputa | Mescla chaves dinâmicas; o nascimento recebe o contexto inteiro: `app/chamados/services/disputa_emissao.py:548`, `:556`, `:561`, `:590`. |
| Kwargs de decisão | Cada linha recebida vira kwargs do modelo: `app/chamados/models.py:3952`, `:3954`. |
| Leitura catalogada | O nome da chave lida vem do catálogo: `app/chamados/catalogo/modulos.py:225`. |
| Leitura integral | O painel mostra todas as chaves/valores; ausência de leitura nominal não prova escrita morta de `contexto_json`: `app/templates/chamados/partials/painel_chamado_turno.html:35`. |
| Auditoria | Persiste os dicionários integrais recebidos: `app/core/observ.py:94`, `:136`. |

No quadro, caminhos sem prefixo são relativos a **`app/chamados/`**; `T/` significa **`app/templates/chamados/`**.

- **N** = selo específico que prenda a diferença não demonstrado; inexistência global **[nao verificado]**.
- **S1** = existe teste do par, explicado após o quadro; execução verde **[nao verificado]**. [nao verificado]
- **E** = ESTRUTURA: diferença no protocolo do código, pela separação da L-099 (`CLAUDE.md:179`). Em D07, **E\*** indica somente o domínio auditado; não há defeito ao qual atribuir culpa.

| ID | Emissor arquivo:linha | Chave(s) | Consumidor arquivo:linha | Classe | Prova que decide | Selo | Dono |
|---|---|---|---|---|---|---|---|
| D01 | `app/ponto/management/commands/marcar_foto_ausente_retro.py:98` | Grava `foto_ausente.batida_id,batida_em`; leitor procura `[motivo].data,ultima_batida.id` | `services/disputa_emissao.py:372`, `:387` | **real** | Contexto do nascimento é preservado em `services/disputa_emissao.py:590`; sem dia, segue para conversa em `:431`, `:439`. Retro agendado com `--apply`: `app/config/crons.py:686`. | N | E |
| D02 | `app/ponto/management/commands/marcar_foto_ausente_retro.py:98` | `foto_ausente.batida_id` → `ultima_batida.id` | `models.py:3464` | **real** | Retorna origem `None` em `models.py:3465`; esse resultado decide o dedup em `services/disputa_emissao.py:325`, `:326`. | N | E |
| D03 | `app/ponto/management/commands/marcar_foto_ausente_retro.py:98` | Mesma troca de identidade | `app/api/foto_recebida.py:67` | **real** | Sem identidade correspondente, pula a disputa em `app/api/foto_recebida.py:68`. Upload chama o consumidor em `app/api/views.py:858`. | N | E |
| D04 | `services/abertura.py:183`, `:191`; catálogo `catalogo/modulos.py:120` | `ausencia_colab.data`; lote procura `data_turno` | `juizes.py:187`, `:197`, `:226` | **real** | Singular lê catálogo em `juizes.py:114`; lote omite esse degrau. Sem célula, pode contar chamado datado como sem dia: `services/juizes_do_dia.py:94`, `:95`. | N | E |
| D05 | `services/adesao.py:79`, `:86` | `data_inicio,periodo`; lote procura `data_turno` | `juizes.py:187`, `:226` | **real** | Catálogo exige `data_inicio` para esse módulo em `catalogo/modulos.py:126`; leitor do lote não a consulta em `juizes.py:187`. | N | E |
| D06 | `services/abertura.py:183`, `:191` | Não fornece `ausencia_id` | `views.py:1512` | **real** | O problema provado é a nota: quando `analisar()` avança, grava “registro da ausencia #None” em `views.py:1513`, `:1516`. O redirecionamento geral tem alternativa explícita em `:1517`. | N | E |
| D07 | `services/abertura.py:183`, `:191` | Não fornece `ausencia_id` | `juizes.py:2331` | **falso** | Ausência é fonte opcional. A competência já recebe `dia_do_fato`, que lê a data catalogada: `juizes.py:114`, `:2359`, `:2362`. Não depende desse ID para obter a data humana. | N | E* |
| D08 | `services/abertura.py:183`, `:191` | Não fornece `tem_documento,ausencia_id` | `T/partials/_card_atendimento.html:65`, `:70` | **real** | Ausência de `tem_documento` oferece o anexo; ID vazio monta `/api/ausencias//documento/`: `T/partials/_card_atendimento.html:65`, `:70`. | N | E |
| D09 | `reconciliador.py:708`; formato `app/ponto/services/lastro.py:134` | `marcos_faltantes[].motivo='lampada_apagada'` | `juizes.py:551`, `:576` | **real** | Leitor exige motivo da pergunta; qualquer motivo preenchido bloqueia alternativa por tipo. Resultado influencia tipo e hora do plantio: `services/materializacao.py:275`, `:414`. | N | E |
| D10 | `management/commands/nomear_marco_por_lampada.py:61`, `:109` | Mesmo formato de D09 | `juizes.py:551`, `:576` | **real** | Comando entrega a saída de `marcos_apagados`; o leitor trata a etiqueta da lâmpada como motivo da pergunta: `app/ponto/services/lastro.py:134`, `juizes.py:551`. Caminho de escrita condicionado a `--apply`: comando `:97`, `:109`. | N | E |
| D11 | `push_utils.py:28` | `requireInteraction,url`; leitor usa `urgente` e destino fixo | `app/static/sw.js:97`, `:107` | **real** | `requireInteraction=True` não alimenta a opção; clique abre `/` ou apenas focaliza janela: `app/static/sw.js:97`, `:106`, `:107`. | N | E |
| D12 | `glpi_client.py:138` | Ticket sem `urgencia` | `T/partials/tabela_chamados.html:30` | **real** | Formato chega diretamente ao template por `views.py:323`, `:340`; ausência de urgência termina em “Normal”: `T/partials/tabela_chamados.html:37`. | N | E |
| D13 | `glpi_client.py:209` | `data`; não fornece `criado_em,autor_tipo,autor_user` | `T/detalhe.html:83`, `:98`, `:101` | **real** | Transporte direto em `views.py:246`, `:262`; data fica vazia e autor cai em “Colaborador”: `T/detalhe.html:98`, `:101`. | N | E |
| D14 | `views_arquivar.py:62` | `erro` singular → `erros` plural | `T/partials/_lista_chamados.html:134` | **real** | Botão chama desarquivar em `T/partials/_lista_chamados.html:85`; recusa específica vira “Falha ao arquivar” em `:134`. Visualização não exige permissão de agir: `utils.py:71`. | N | E |
| D15 | `services/fio.py:461` | Omite `contador_ativos,contador_msgs_mes,contador_disputas_abertas,cor_disputas_map` | `T/partials/modal_fio.html:14`, `:76` | **real** | Callback renderiza esse contexto em `services/fio.py:494`; contadores somem e cor ausente cai no cinza em `templatetags/chamados_tags.py:107`. Completo fornece os campos em `views.py:945`. | N | E |
| D16 | `views.py:927` | Omite `pode_conferir_data` | `T/partials/modal_fio.html:172` | **real** | GET usa esse contexto em `views.py:956`; usuário autorizado recebe o ramo de bloqueio em `T/partials/modal_fio.html:180`. Parcial calcula a permissão em `services/fio.py:474`. | N | E |
| D17 | `views_cobrar.py:263`, `:265` | `ok,erros,nao_gravados`; JS lê apenas contagem/recusados | `app/templates/core/_copiloto.html:251`, `:254` | **real** | Serviço distingue validação sem gravação em `services/validacao.py:753`, `:756`; outro template mostra esse aviso em `T/partials/_lote_preview.html:9`, mas copiloto o omite. Também não lê `ok/erros`: `app/templates/core/_copiloto.html:252`. | N | E |
| D18 | `services/fio.py:1114` | `fonte` excedente | `management/commands/backfill_decisoes_pergunta.py:553` | **escrita morta** | Consumidor imprime os demais campos com cabeçalho fixo em `management/commands/backfill_decisoes_pergunta.py:554`, `:555`; não usa `fonte`. | N | E |
| D19 | `services/fio.py:1181` | `fonte` excedente | `management/commands/backfill_decisoes_pergunta.py:559` | **escrita morta** | Mesmo descarte no retorno: `management/commands/backfill_decisoes_pergunta.py:560`, `:561`. | N | E |
| D20 | `services/fio.py:1206` | `fonte` excedente | `management/commands/backfill_decisoes_pergunta.py:565` | **escrita morta** | Produção imprime cabeçalho fixo e demais campos em `management/commands/backfill_decisoes_pergunta.py:566`, `:567`. Há leitura de `fonte` em teste: `tests/test_e1_v15h.py:186`. | **Sim, S1** | E |
| D21 | `services/fio.py:1902` | `fonte` excedente | `management/commands/backfill_decisoes_pergunta.py:545` | **escrita morta** | Impressão usa `n,esperado,motivo,pks`, sem `fonte`: `management/commands/backfill_decisoes_pergunta.py:546`, `:547`. | N | E |
| D22 | `services/fio.py:1343` | `merito_recusado,episodios_unidos,sem_episodio_intocada,canonica_sem_episodio` excedentes | `management/commands/backfill_decisoes_pergunta.py:331` | **escrita morta** | Esse chamador e `services/fio.py:2181` extraem só `retratadas`; `services/fio.py:922` ignora o retorno inteiro. Classificação vale para a saída; o plano anterior tem leitores em `services/fio.py:2162`, `:2165`. | N | E |
| D23 | `reconciliador.py:1340` | `rejuntados,perguntas,lastro` excedentes | `management/commands/consumir_eventos_celula.py:31` | **escrita morta** | Interpolação nomeia oito campos, sem esses três: `management/commands/consumir_eventos_celula.py:32`, `:35`. Os efeitos continuam executados: `reconciliador.py:1365`, `:1366`, `:1384`. | N | E |
| D24 | `app/colaboradores/services/desligamento.py:203` | `desligamento_revisao.data_demissao` | `juizes.py:187` | **real** | Chave declarada em `catalogo/modulos.py:141`, lida pelo singular em `juizes.py:114`, ignorada pelo lote que alimenta `services/juizes_do_dia.py:94`. | N | E |
| D25 | `app/colaboradores/services/vinculo.py:419`, `:424` | `escala_colaborador.data_inicio` | `juizes.py:187` | **real** | Declaração em `catalogo/modulos.py:137`; mesma perda no ramo sem célula do lote: `juizes.py:184`, `:187`, `:226`. | N | E |
| D26 | `app/escala/regua_defesa.py:72`, `:76` | `regua_degenerada.data_exemplo` | `juizes.py:187` | **real** | Declaração em `catalogo/modulos.py:133`; emissão chamada pelo detector em `app/ponto/management/commands/detectar_ausencias.py:601`; lote ignora a chave. | N | E |
| D27 | `app/ponto/utils.py:120`, `:122` | `colaborador_em_ferias.data_inicio` | `juizes.py:187` | **real** | Declaração em `catalogo/modulos.py:123`; emissor chamado por `detector_anomalias.py:116`, acionado pela API em `app/api/views.py:434`. | N | E |
| D28 | `app/ponto/management/commands/detectar_ausencias.py:224`, `:231` | `sem_previsao_escala/folga_nao_definida.data_inicio` | `juizes.py:187` | **real** | Chave catalogada em `catalogo/modulos.py:126`; lote sem célula passa por `juizes.py:187`, `:226` sem consultá-la. | N | E |
| D29 | `app/ponto/management/commands/detectar_ausencias.py:294`, `:297` | `ancora_limbo.data_inicio` | `juizes.py:187` | **real** | Chave declarada em `catalogo/modulos.py:131`; mesma omissão do lote, observada pelo contador em `services/juizes_do_dia.py:94`, `:95`. | N | E |
| D30 | `app/ponto/management/commands/detectar_ausencias.py:532` | `sem_previsao_escala/dia_sem_grade.data_inicio` | `juizes.py:187` | **real** | O próprio chamador exige célula inexistente antes de emitir: `app/ponto/management/commands/detectar_ausencias.py:660`, `:668`. Assim, o primeiro degrau do lote não recupera a data emitida. | N | E |

A condição dos nove itens de data é relevante: **célula ligada pode esconder a incompatibilidade**, pois o lote a consulta primeiro; a porta permite nascimento sem essa ligação (`app/chamados/juizes.py:184`, `app/chamados/models.py:1241`, `:1243`). O consumidor que conta “sem dia” seleciona todos os estados vivos, incluindo `registrado` (`app/chamados/services/juizes_do_dia.py:92`, `app/chamados/catalogo/motor.py:24`).

As omissões dos contextos do modal não são preenchidas pelo helper de resposta: ele encaminha o contexto diretamente ao render (`app/core/respostas.py:74`, `:83`). Os processadores configurados também não fornecem esses campos (`app/config/settings/base.py:76`; retornos em `app/core/context_processors.py:154`, `:189`, `:210`, `app/core/context_processors_ia.py:31`, `app/pautas/context_processors.py:35`).

**O anexo errou em D07 e exagerou a conclusão possível de D18–D23.** D07 não demonstra incompatibilidade, porque a data humana já chega pela outra fonte. D18–D23 descrevem campos excedentes de retorno, sem decisão errada demonstrada nos consumidores de produção rastreados; não são funções inteiras mortas (`app/chamados/juizes.py:2359`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:547`, `:555`, `:561`, `:567`, `:331`; `app/chamados/management/commands/consumir_eventos_celula.py:32`).

Também é incorreto generalizar “zero selo de par demonstrado”: **S1 existe em D20**, chama o backfill e verifica sua saída. Ele não exige imprimir `fonte`, portanto não transforma esse metadado excedente em defeito real (`app/chamados/tests/test_e1_v15h.py:194`, `:197`, `:200`, `:201`). Nos demais itens, os testes citados pelo anexo cobrem aspectos separados: contexto canônico em `app/chamados/tests/test_a2_data_evento_batida.py:21`; emissão humana em `app/chamados/tests/test_form_catalogo.py:76`; escritor/leitor de marcos separadamente em `app/chamados/tests/test_emissor_nomeia_o_marco.py:54` e `app/chamados/tests/test_o306_marco_do_motivo_nao_e_do_vizinho.py:45`; modal completo em `app/chamados/tests/test_paridade_fio_modal_f8.py:65`. Cobertura global dos pares permanece **[nao verificado]**.

Os totais são **do quadro individualizado**, não do módulo inteiro:

| Classe | D01–D23 | D24–D30 | Total |
|---|---:|---:|---:|
| real | 16 | 7 | **23** |
| escrita morta | 6 | 0 | **6** |
| ramo morto demonstrado | 0 | 0 | **0** |
| falso | 1 | 0 | **1** |
| Itens examinados | **23** | **7** | **30** |

Entre os reais, estas são repetições da mesma causa:

| Itens | Mesma classe de defeito | Prova |
|---|---|---|
| **D01–D03** | Identidade da batida emitida em um caminho e procurada em outro: **CONTRATO-DE-ENTRADA-DO-FIO**, já registrado. | `app/docs/BACKLOG.md:2100`; `app/chamados/models.py:3464`; `app/api/foto_recebida.py:67`. |
| **D04, D05, D24–D30** | Lote omite o degrau da chave de data catalogada que o singular consulta. | `app/chamados/juizes.py:114`, `:187`. |
| **D06, D08** | Emissão humana de ausência consumida como se trouxesse referência a uma entidade `Ausencia`. | `app/chamados/services/abertura.py:183`; `app/chamados/views.py:1516`; `app/templates/chamados/partials/_card_atendimento.html:70`. |
| **D09, D10** | `motivo` significa etiqueta da lâmpada no emissor e motivo da pergunta no leitor. | `app/ponto/services/lastro.py:134`; `app/chamados/juizes.py:551`, `:576`. |
| **D15, D16** | Dois construtores do mesmo contexto entregam conjuntos diferentes de campos ao mesmo template. | `app/chamados/services/fio.py:461`; `app/chamados/views.py:927`; `app/templates/chamados/partials/modal_fio.html:14`, `:172`. |

D11, D12, D13, D14 e D17 continuam como incompatibilidades próprias dos respectivos protocolos; agrupá-las numa única causa além de “contrato divergente” seria **[nao verificado]**. O total global e a afirmação de que não há outros desencontros continuam **[nao verificado]**.

_4799 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
