# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 19:56** · publicado em **15/11/2026 19:56** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 19:56 · sessao 37c3dd6890b3 · - · - · origem=Ronald (tmux)

`trabalha_em_feriado` — censo de referências executáveis de produção:

| Papel | Quem |
|---|---|
| Declara/default | `app/escala/models.py:896` (`default=True` em :897). |
| Decide padrão novo (16/09) | `app/escala/models.py:905`; posto declarado vence em :911-913; comercial 5x2/6x1 em :916-917. |
| Escreve ao criar vínculo | `app/colaboradores/services/vinculo.py:287`, valor pelo padrão em :296. |
| Escreve alteração existente | `app/colaboradores/services/vinculo.py:869`, compara/lê :890-892, atribui e salva :893-894. |
| Lê/copia em novos trechos | `app/colaboradores/services/vinculo.py:974`, :1092, :1113. |
| Lê para regra feriado | `app/escala/models.py:1390`. |
| Lê para contador vigentes | `app/escala/models.py:929`. |
| Lê no comando de sincronização | `app/colaboradores/management/commands/sincronizar_feriado_admin.py:48`, :52; alteração passa pela porta em :56. |

`feriado_vira_folga` — quatro leitores executáveis encontrados; o aval nomeia os três externos e omite o gerador: `app/escala/models.py:1369`; `app/ponto/services/fechamento.py:949`; `app/ponto/precedencia.py:377`; `app/ponto/calculador/alimentacao.py:521`.

Leitores indiretos relevantes da flag pelo fallback `eh_dia_trabalho → eh_dia_trabalho_calculado → feriado_vira_folga`: `app/ponto/motor_calculo_v2.py:2448`, :3449 (previstos semanais BH); `app/ponto/services/fechamento.py:731`; `app/ponto/calculador/alimentacao.py:445`; `app/ponto/precedencia.py:538`. A cadeia está em `app/escala/models.py:1094-1096` e :1369. Portanto, deixar o arquivo do motor intocado não prova que sua saída ficará igual; os leitores mencionados estão no motor (`app/ponto/motor_calculo_v2.py:2448`, :3449).

`CelulaDia` — escritores:

- Porta única declarada: `app/core/portas.py:107-112`.
- Geração/regeneração, gravações reais: `app/ponto/portas/celula.py:265`, :279, :293, :310, :914, :926; gerador consulta a aritmética em :289 e :911.
- Edição humana: `app/ponto/portas/celula.py:572`, criação :638 e atualização :647.
- Reversão de regeneração: `app/ponto/portas/celula.py:779`, atualização :836.
- Veredito/ata/status: `app/ponto/portas/celula.py:1032-1036`, atribuições :1091-1096 e gravação :1098; método da entidade só delega (`app/escala/models.py:1819-1828`).
- Carimbo: `app/ponto/portas/celula.py:1181-1201`; precedência na ata :1205; insumos em lote :1237; reset do julgamento futuro :1124.
- Persistência central comum: `app/ponto/portas/celula.py:957-965`.

`CelulaDia` — leitores centrais para esta revisão:

| Pergunta | Leitor |
|---|---|
| Carga da janela | `app/escala/alimentacao.py:54`, query :75. |
| Vínculo do dia | `app/escala/alimentacao.py:269-286` (geradora e vigência). |
| Dia previsto | `app/escala/models.py:1069`, query :1092, retorno :1095. |
| Marcos/DNA e horários | `app/escala/models.py:1122`, :1187, :1232. |
| Grade/célula | `app/escala/utils.py:1054`, :2138; seleção da geradora :2181, :2191. |
| Precedência | `app/ponto/precedencia.py:308` (carga); juiz declarado em `app/docs/JUIZES.md:114-115`. |
| Fechamento | `app/ponto/services/fechamento.py:604-607`. |
| Motor | `app/ponto/motor_calculo_v2.py:2448`, :3449 por `eh_dia_trabalho`; leitura de célula pelo método em `app/escala/models.py:1089-1095`. |
| Alimentação do calculador | `app/ponto/calculador/alimentacao.py:445` pelo mesmo método. |
| Exportação/folha | `app/folha/export.py:949`, :1247; `app/folha/previa.py:50`; `app/folha/validacao_pdf.py:44`; `app/folha/porta_export.py:532`. |
| Cartório | `app/ponto/services/cartorio.py:390`, :2027. |
| Turnos/núcleo | `app/ponto/turnos.py:1586`, :2334; `app/ponto/nucleo.py:92`. |
| Status/veredito | `app/ponto/services/veredito_celula.py:30`, :107, :153; autoridade de status em `app/docs/JUIZES.md:120-121`. |
| Chamados/porta | `app/chamados/juizes.py:411`, :1286, :1631; `app/chamados/models.py:1287`, :2979, :3325, :3327; `app/chamados/reconciliador.py:693`, :704, :868, :879, :1027, :1295, :1307. |
| Relatórios/telas | `app/relatorios/cartao_pela_celula.py:286`; `app/relatorios/dia_da_frota.py:34`; `app/core/views.py:396`; `app/pautas/services.py:423`; `app/colaboradores/views.py:2874`; `app/colaboradores/services/calendario.py:411`. |

Há muitos outros leitores diagnósticos/vigias/comandos além desses; tabela acima **não é censo exaustivo de toda consulta a `CelulaDia`**. Exemplos comprovados: `app/ponto/services/vigia_entrada.py:78`, `app/ponto/services/vigia_ausencia_veredito.py:42`, `app/ponto/services/vigia_cadastro_futuro.py:31`, `app/escala/services/cadastro_realidade.py:166`, `app/chamados/services/foto_e4b.py:269`.

Anexo real existente: `app/docs/JUIZES.md:101-134`. Define feriado em :101-102; dia previsto “célula; senão aritmética do VÍNCULO” em :112-113; vínculo do dia em :130-131; cobertura em :132-134. Registro vivo confirma dia previsto/precedência em `app/core/juizes.py:186-192`; porta de escrita da célula em `app/core/portas.py:107-112`.

## 15/11 19:56 · sessao c8cda0f10a4a · DISPUTA-ZUMBI-1110 · sonda · origem=Code

Transcrição do dado fornecido; conferência independente em produção: [nao verificado].

```text
UNIVERSO abertas=468 pode_fechar_True=0 total_erros=0 HOJE=2026-11-15
abertas                                 468
pode_fechar_False                       468
total_erros=0
```

**Sim, `total_erros=0`.** Portanto, pelo dado fornecido, a medida conclui **zero disputas abertas com C7=True**, entre 468 abertas, em 2026-11-15. [nao verificado]

Na cópia, C7 retorna True quando não há respostas sem veredito nem fio vivo. (`app/chamados/models.py:1911–1917`) A identidade dessa cópia com o HEAD/hash informado é [nao verificado]: o diretório não contém metadados Git.

**Não há PF_TRUE para classificar individualmente no dado fornecido.** Assim, não há caso que exija apontar campo faltante ou caminho de fechamento omitido. [nao verificado]

| Classe conferida no código da cópia | PF_TRUE no dado fornecido |
|---|---:|
| (i) Vazia, com abstenção por ata: o emissor consulta C7 e preserva aberta quando `_absteve_por_ata`. (`app/chamados/services/disputa_emissao.py:1292–1301`) | 0 [nao verificado] |
| (ii) Pai vivo fora do conjunto encerrável: após C7=True, separa em `disputas_com_pai_vivo_fora`. (`app/chamados/services/competencia_trancada.py:173–183`) | 0 [nao verificado] |
| (iii) Todas as perguntas em dias trancados: a tranca elimina a vivacidade e exclui essas respostas da lista sem veredito. (`app/chamados/juizes.py:1406–1409`; `app/chamados/models.py:1903–1909`) | 0 [nao verificado] |
| (iv) Beco FECHAVEL: o classificador consulta C7; a entrada do evento encaminha o FECHAVEL ao fechamento. (`app/chamados/services/becos.py:25–33`, `66–78`) | 0 [nao verificado] |
| (v) Outro: nenhum PF_TRUE fornecido para identificar caminho omitido. [nao verificado] | 0 [nao verificado] |

**Total distribuído: 0 PF_TRUE.** [nao verificado]

## 15/11 19:56 · sessao 57ad847f7f6f · - · - · origem=Ronald (tmux)

Q3 — a condição é possível sob uma referência específica, mas o literal deixa ambiguidades que precisam ser explicitadas:

- Comparar **antes do lote** com **cura + lote** pode produzir 31/31 iguais: o movimento declarado é produzido pela alteração do flag no fallback, e a candidata (a) pretende neutralizar essa dependência. Comparar **impacto anterior depois do lote** com **impacto curado** exige que col940 mude −6,44→−4,24; nesse referencial, “31/31 iguais” contradiz a estabilização pretendida. A mudança anterior está registrada em `app/docs/RELATO.md:4`; a escolha exata dos lados da nova comparação é **[nao verificado]**.
- O histórico declara 16 vínculos em 13/11 (`app/docs/RELATO.md:738`; `app/docs/BACKLOG.md:729`), enquanto o impacto atual declara 15 vínculos/31 colab-competência (`app/docs/RELATO.md:4`). A identidade do excluído, o motivo e o universo congelado desde 13/11 são **[nao verificado]**.
- “Qualquer campo gravado” alcança um efeito adicional da porta: `marcar_trabalha_em_feriado` salva o vínculo (`app/colaboradores/services/vinculo.py:894`); o signal invalida o previsto (`app/escala/signals.py:137`, `app/escala/signals.py:146`); `invalidar_previsto` zera `previsto_em` e altera `atualizado_em` de todos os fechamentos daquele colaborador que tenham `previsto_em`, sem filtro de competência/exportação (`app/ponto/services/fechamento.py:1607`). Existir fechamento entregue nessas condições é **[nao verificado]**. O próprio backlog registra esse efeito como fila (`app/docs/BACKLOG.md:730`).
- O retorno padrão da sombra cobre apenas 26 campos e omite `motivos_espelho`, `causa_espelho`, `previsto_em` e `atualizado_em`; não basta para provar literalmente “qualquer campo gravado” (`app/ponto/services/fechamento.py:454`, `app/ponto/services/fechamento.py:1128`). Um confronto indiscriminado de carimbos recalculados também inclui valores de relógio: `previsto_em=timezone.now()` (`app/ponto/services/fechamento.py:1086`).

O teste oposto já existe: [nao verificado]

- Teste direto: vínculo com início 01/01, feriado de 21/04, flag False → `eh_dia_trabalho=False`; True → trabalho (`app/escala/tests/test_escala_models.py:255`, `app/escala/tests/test_escala_models.py:268`, `app/escala/tests/test_escala_models.py:275`).
- Vigência delimitada explicitamente: vínculo 01/01–13/12 (`app/ponto/tests/test_precedencia_vinculo_encerrado.py:43`), feriado em 10/12 (`app/ponto/tests/test_precedencia_vinculo_encerrado.py:36`), flag False suprime previsto (`app/ponto/tests/test_precedencia_vinculo_encerrado.py:68`). Resultado de execução nesta revisão: **[nao verificado]**.

Q4 — não é possível afirmar “nenhuma outra exportada muda” com os dados fornecidos: a população da frota e o impacto depois da cura são **[nao verificado]**. O caminho monetário é comprovável sem editar o motor: o banco pergunta ao vínculo por todos os sete dias da semana, inclusive anteriores ao início da fatia (`app/ponto/motor_calculo_v2.py:3436`, `app/ponto/motor_calculo_v2.py:3449`), usa a quantidade como denominador (`app/ponto/motor_calculo_v2.py:3451`) e grava o saldo resultante (`app/ponto/services/fechamento.py:1104`, `app/ponto/services/fechamento.py:1180`). Portanto, alterar `escala/models.py` pode alterar comportamento monetário do motor sem alterar o arquivo do motor (`app/escala/models.py:1096`, `app/escala/models.py:1369`).

Há também leitores de rubrica: fechamento separa folga de feriado por `feriado_vira_folga` e promove HE100 (`app/ponto/services/fechamento.py:949`, `app/ponto/services/fechamento.py:955`); calculador alimenta a mesma pergunta (`app/ponto/calculador/alimentacao.py:521`) e escolhe a rubrica (`app/ponto/calculador/regras.py:684`). O impacto anterior declara zero mudança dessa rubrica nos 31, sem demonstrar a frota inteira (`app/docs/BACKLOG.md:801`).

O candidato (a), isoladamente, pode ser implementado sem escritor de dados e sem editar `motor_calculo_v2.py`; a implementação concreta ainda é **[nao verificado]**. O lote existente, porém, escreve flag e células pelas portas (`app/colaboradores/services/vinculo.py:894`, `app/colaboradores/services/vinculo.py:904`, `app/colaboradores/services/vinculo.py:910`), além da invalidação de fechamento acima.

## 15/11 19:56 · sessao 7ffdf687cbb2 · FERIADO-USA-A-LEI-DE-1609 · sonda · origem=Code

1. Transcrição do dado raspado; execução não conferida em banco/log. [não verificado]

```text
UNIVERSO HOJE=2026-11-15 colab_comp_examinados=3809 dia_colab_feriado_examinados=7662 competencias=7+14 (ENTREGUE+ABERTA) candidatos_A=118 dia-colab em 87 colab-comp | POPULACAO ENTREGUE_FORA=1 ENTREGUE_LOTE=1 ABERTA_FORA=6 ABERTA_LOTE=0 | total_erros=0
A_colab_comp_candidatos               87
A_dia_colab_candidatos               118
B_fechamento_lido_chamadas            87
B_sem_resposta                        12
competencias_ABERTA                   14
competencias_ENTREGUE                  7
lote_vinculos                         15
total_erros=0
```

**Sim: o dado informa `total_erros=0`.** Não apresenta traceback; o significado das 12 ocorrências de `B_sem_resposta` depende do script ausente. [não verificado]

2. ACERTOS transcritos. FORA = flag gravada; LOTE = flag da lei em memória, conforme definição fornecida. [não verificado]

| Classe | Modo | Colaborador | Competência | Dia | EC | Leitor | Fonte |
|---|---|---|---|---|---|---|---|
| ENTREGUE | LOTE | col940 | 09/2026 | 2026-09-07 | 1208 | ponto/motor_calculo_v2.py:3449 | [não verificado] |
| ENTREGUE | FORA | col945 | 09/2026 | 2026-09-07 | 1255 | ponto/motor_calculo_v2.py:3449 | [não verificado] |
| ABERTA | FORA | col985 | 10/2026 | 2026-10-12 | 1418 | ponto/motor_calculo_v2.py:3449 | [não verificado] |
| ABERTA | FORA | col987 | 10/2026 | 2026-10-12 | 1421 | ponto/motor_calculo_v2.py:3449 | [não verificado] |
| ABERTA | FORA | col990 | 10/2026 | 2026-10-12 | 1424 | ponto/motor_calculo_v2.py:3449 | [não verificado] |
| ABERTA | FORA | col1018 | 11/2026 | 2026-11-02 | 1517 | ponto/motor_calculo_v2.py:3449 | [não verificado] |
| ABERTA | FORA | col1023 | 11/2026 | 2026-11-02 | 1522 | ponto/motor_calculo_v2.py:3449 | [não verificado] |
| ABERTA | FORA | col1024 | 11/2026 | 2026-11-02 | 1523 | ponto/motor_calculo_v2.py:3449 | [não verificado] |

3. O único leitor citado corresponde a **`MotorComercial.calcular_mes`**, na apuração semanal do banco de horas. A classe e o método estão em `app/ponto/motor_calculo_v2.py:3199` e `app/ponto/motor_calculo_v2.py:3308`; a consulta ocorre em `app/ponto/motor_calculo_v2.py:3449`.

**É caminho de motor e fechamento:** `fechamento_lido` chama `recalcular_fechamento_mes(..., somente_leitura=True)`, que passa por `apurar_por_fatia` e chama `motor.calcular_mes`. (`app/ponto/services/fechamento.py:1759`, `app/ponto/services/fechamento.py:754`, `app/ponto/services/fechamento.py:286`)

Sem `CelulaDia`, `eh_dia_trabalho` chama o cálculo aritmético; uma base `True` vira `False` quando `feriado_vira_folga` aceita flag, ciclo e feriado. (`app/escala/models.py:1094`, `app/escala/models.py:1096`, `app/escala/models.py:1369`, `app/escala/models.py:1390`, `app/escala/models.py:1392`, `app/escala/models.py:1395`)

Li os arquivos disponíveis da cópia; sem metadados Git, a correspondência com o HEAD solicitado não pôde ser certificada. [não verificado]

4. **Não foi possível auditar o pré-filtro:** `logs/FERIADO-LEI-1609/g_frota/sonda_populacao.py` não está disponível nesta cópia. Portanto, A ser superconjunto de B permanece **[não verificado]**.

Há uma condição concreta para confrontar com A: o leitor recua até segunda-feira, monta semanas completas e consulta os sete dias; só depois restringe a contagem ao período. Assim, consulta também dias anteriores a `data_ini` e posteriores a `data_fim`. (`app/ponto/motor_calculo_v2.py:3436`, `app/ponto/motor_calculo_v2.py:3440`, `app/ponto/motor_calculo_v2.py:3441`, `app/ponto/motor_calculo_v2.py:3449`, `app/ponto/motor_calculo_v2.py:3450`)

Se A limitar os candidatos às datas da competência/fatia, descartará feriados dessas bordas que B consulta. A existência desse corte em A é **[não verificado]**; sem o script, isso é um contraexemplo condicional, não um defeito confirmado.

5. **ENTREGUE FORA = 1** (`col945`, 09/2026); **ENTREGUE LOTE = 1** (`col940`, 09/2026), conforme o dado raspado. [não verificado]

## 15/11 19:56 · sessao aae31e3519ff · - · - · origem=Ronald (tmux)

Q2: **não identifiquei colisão do desenho (a)**, desde que se limite a “SEM célula e FORA da vigência” e preserve o resultado dentro dela. A célula é soberana e o fallback é aritmético/template — `CLAUDE.md:148-157`, `app/docs/LEIS.md:71`. Sem célula, `vinculo_do_dia` declara usar “a aritmética do vínculo que cobre a data” — `app/escala/alimentacao.py:244-247`; cobertura exige `data_inicio <= data`, fim inclusivo e vigência declarada — `app/escala/alimentacao.py:205-216`. E1 diz literalmente: “a geradora da celula so vale DENTRO da vigencia (...) fora dela a juiza devolve None (...) Vigencia, nao o flag ativo” — `app/escala/alimentacao.py:272-283`. As autoridades únicas estão registradas em `app/docs/CORTES.md:335` e `app/docs/CORTES.md:338`.

A lei de **16/09 sobre o padrão de feriado** trata do nascimento de vínculo NOVO: “O posto que DECLAROU (...) vence”; sem declaração, comerciais 5x2/6x1 folgam, demais trabalham; “Vinculo que ja existe nao passa por aqui — quem muda esses e o DP” — `app/escala/models.py:900-917`. Ela não autoriza ler o flag do vínculo em qualquer data, nem manda regravar vínculos existentes — `app/escala/models.py:906-910`.

**Item 4: PROCEDE como registro de pergunta, sem implementação ou decisão.** L-117 manda levar a pergunta ao topo com números e seguir a fila, sem propor lei nova — `app/docs/LEIS.md:138`, `CLAUDE.md:269-273`. E1 determina quem vale na data; o código do banco semanal, entretanto, consulta o mesmo `_ec` pelos sete dias da semana e usa os dias previstos no denominador — `app/ponto/motor_calculo_v2.py:3433-3453`. Portanto não concluir que o corte (a) resolve a pergunta (b) inteira.

Q4, cautela relevante: uma mudança em `EscalaColaborador.eh_dia_trabalho` pode alterar **indiretamente** o banco do motor sem editar `motor_calculo_v2.py`, porque ele chama esse método em `app/ponto/motor_calculo_v2.py:3449`, calcula a proporção em `:3450-3452` e grava o resultado em `:3455`. Quantos colaboradores/competências mudariam é **[nao verificado]** sem a sonda de frota.

Os limites escritos continuam: mexer no GRAVADO exportado, dado de ESCALA/VÍNCULO ou `motor_calculo_v2.py` exige `!` — `CLAUDE.md:265-268`, `CLAUDE.md:142`; dinheiro não exportado requer reversão prévia, exportada intacta e prova de frota — `CLAUDE.md:406-410`; exportado exige critério e porta própria — `CLAUDE.md:411-417`. **Não assumir proteção automática pela recusa do recalculador:** L-113 documenta que a recusa genérica de exportada caiu, substituída por trilha — `app/docs/LEIS.md:154`.

## 15/11 19:55 · sessao 58dc16a71f83 · - · - · origem=Ronald (tmux)

O único leitor citado, `ponto/motor_calculo_v2.py:3449`, corresponde a `MotorComercial.calcular_mes`: classe em `app/ponto/motor_calculo_v2.py:3199`, método em `app/ponto/motor_calculo_v2.py:3308`.

- **É caminho de motor e fechamento.** `fechamento_lido` chama `recalcular_fechamento_mes` com `somente_leitura=True`; este chama `apurar_por_fatia`, que executa `motor.calcular_mes`. (`app/ponto/services/fechamento.py:1759`, `app/ponto/services/fechamento.py:754`, `app/ponto/services/fechamento.py:286`)
- **O leitor consulta semanas completas:** recua o cursor até segunda, constrói os sete dias e chama `eh_dia_trabalho` em todos; somente depois conta os dias dentro do período. Pode consultar dias anteriores a `data_ini` e posteriores a `data_fim`. (`app/ponto/motor_calculo_v2.py:3436`, `app/ponto/motor_calculo_v2.py:3441`, `app/ponto/motor_calculo_v2.py:3449`, `app/ponto/motor_calculo_v2.py:3450`)
- **O vínculo consultado é o da fatia:** o motor recebe `escala_colaborador=esc`, armazena em `self._ec`, recuperado como `_vinc_bh`. (`app/ponto/services/fechamento.py:289`, `app/ponto/motor_calculo_v2.py:2309`, `app/ponto/motor_calculo_v2.py:3433`)
- **Sem CelulaDia**, `eh_dia_trabalho` usa o cálculo aritmético; quando `base is True`, `feriado_vira_folga` pode devolver folga. O predicado exige flag falsa, ciclo contemplado e feriado no lugar do vínculo; não testa vigência. (`app/escala/models.py:1087`, `app/escala/models.py:1096`, `app/escala/models.py:1369`, `app/escala/models.py:1389`)
- **Ponto para confrontar com Etapa A:** qualquer pré-filtro limitado à janela da competência/fatia descartaria feriado na borda da semana que o leitor real consulta; isso é um contraexemplo condicional, cuja presença no script depende de sua leitura. (`app/ponto/motor_calculo_v2.py:3436`, `app/ponto/motor_calculo_v2.py:3440`, `app/ponto/motor_calculo_v2.py:3449`)

## 15/11 19:52 · sessao fa73c9ea025e · DISPUTA-ZUMBI-1110 · censo · origem=Code

**1. Existe contador hoje? Sim, em recortes; não localizei um contador global exato de “aberta + C7=True” nos caminhos lidos.** O estado aberto é `fechada_em is None`; C7 consulta perguntas, respostas sem veredito e fio vivo. (`app/chamados/models.py:1871`, `1911–1917`)

| Caminho | O que efetivamente cobre |
|---|---|
| `becos` / `fechar_becos_disputa --alarme` | **Conta o estado pedido dentro dos becos:** disputa aberta, pai terminal e nenhuma pergunta considerada visível pelo filtro local. Só depois pergunta C7; imprime fecháveis/barradas e sai com rc 2 se houver fechável. Não há janela de datas. (`app/chamados/services/becos.py:25–33`; `app/chamados/management/commands/fechar_becos_disputa.py:32–44`, `67–70`) |
| `vigia_carimbo` | Conta **perguntas de hora/data_hora validadas**, com ou sem registro, independentemente de disputa aberta/fechada. Não pergunta C7. (`app/chamados/services/vigia_carimbo.py:95–132`) |
| `desenlace` | Apresentação por chamado: recebe uma explicação com `pode_fechar` e produz estado/ação. Não varre nem conta disputas. (`app/chamados/services/desenlace.py:44–67`) |
| `reconciliador` | Pergunta C7 para fechar disputas atingidas pelos atos de reconciliação; não acumula o censo global pedido. O vigia `reconciliar_chamados` conta premissas mortas, perguntas absolvidas, rejunção e outbox. (`app/chamados/reconciliador.py:412–414`, `609–615`, `1158–1173`; `app/chamados/management/commands/reconciliar_chamados.py:89–124`) |
| `crons_quebrados` | Conta últimas execuções com saída diferente de 0 e 2; seu universo são corridas de cron nas últimas 26 horas, não disputas. (`app/core/services/crons_quebrados.py:16–17`, `24–47`) |
| `config/crons.py` | Agenda beco às **07:10**, reconciliador a cada **15 minutos** e carimbo como elo após fio morto. Essas entradas não acrescentam um censo global de disputas. (`app/config/crons.py:663`, `395`, `1774–1776`) |

**Outros contadores encontrados:**

- **Relatório de pendências:** percorre todas as abertas e usa C7 para `PRONTA_FECHAR`, mas separa as sem perguntas em `SEM_PERGUNTA` **antes** de chamar C7. Portanto, `PRONTA_FECHAR` não equivale ao total global pedido. É sob demanda, sem saída de alarme. (`app/chamados/relatorios_disputa.py:11–28`, `45–58`; `app/chamados/management/commands/relatorio_pendencias_disputa.py:14–40`; `app/config/crons.py:1463`)
- **Competência trancada:** conta abertas com C7=True ligadas às perguntas daquela competência, **excluindo pai vivo fora do conjunto que será encerrado**. O comando mostra disputas e uma soma de chamados+perguntas+disputas; `placar_code.sh` recolhe essa soma. Não tem cron próprio nem rc 2 nesse comando. (`app/chamados/services/competencia_trancada.py:119–126`, `173–183`, `346–355`; `app/chamados/management/commands/chamados_em_competencia_trancada.py:12–30`; `bin/placar_code.sh:285–289`; `app/config/crons.py:1295–1297`)
- **Vigia do fio:** `disputa_vazia()` conta o motivo de fechamento `vazia`; `viva_em_disputa_fechada()` conta perguntas vivas em disputas **fechadas**. Nenhum corresponde ao estado solicitado. (`app/chamados/services/vigia_fio.py:748–780`)

Quantidade atual em produção, execuções efetivamente instaladas e ocorrência concreta desses estados: **[nao verificado]**.

**2. Opções de sítio, preservando C7 e sem alterar `config/crons.py`.**

Considere **N** disputas abertas, **B** abertas que passam pelo filtro de beco e **E** abertas sem perguntas; são universos derivados dos filtros existentes. (`app/chamados/management/commands/fechar_becos_disputa.py:32–43`; `app/chamados/services/becos.py:27–33`; `app/chamados/relatorios_disputa.py:12–14`)

| Opção técnica | Universo e custo de C7 |
|---|---|
| **(a) Estender `fechar_becos_disputa --alarme`** | Contador geral sobre N abertas: **N chamadas**, uma por disputa. Hoje são B chamadas. Acrescentar uma passagem global mantendo a classificação atual sem reaproveitamento custaria **N+B**. Já existe execução diária com `--alarme`; a aplicação pode continuar restrita aos becos. (`app/chamados/services/becos.py:25–33`; `app/chamados/management/commands/fechar_becos_disputa.py:30–57`; `app/config/crons.py:663`) |
| **(b) Acrescentar a outro vigia existente** | No **carimbo**, seriam N chamadas novas, além da classificação atual das perguntas validadas; é necessário carregar as abertas separadamente. No **reconciliador**, seriam N chamadas novas **a cada 15 minutos**, além dos censos atuais. (`app/chamados/services/vigia_carimbo.py:97–116`; `app/chamados/management/commands/reconciliar_chamados.py:57–100`; `app/config/crons.py:395`, `1774–1776`) |
| **(c) Contador sob demanda / relatório** | Um contador próprio sobre abertas custaria N chamadas. Reaproveitar o relatório atual exige incluir as E vazias: hoje ele chama C7 apenas N−E vezes. O relatório não possui `--alarme` nem agendamento. (`app/chamados/relatorios_disputa.py:12–25`, `45–52`; `app/chamados/management/commands/relatorio_pendencias_disputa.py:11–40`; `app/config/crons.py:1463`) |

**Custo de uma chamada:** C7 materializa a lista inteira de perguntas; avalia respostas sem veredito e, se essa lista estiver vazia, executa `fio_vivo`, que interrompe na primeira pergunta viva. Sem `perguntas=` ou prefetch, há carregamento da relação. (`app/chamados/models.py:1889–1917`; `app/chamados/juizes.py:1507–1516`)

As consultas transitivas variam conforme o caso: tranca da competência e toque administrativo; célula/veredito/ata/lâmpadas; classificação de falta; provas de materialização, incluindo batidas e trilha. Logo **não há número fixo de SQL por disputa**; prefetch e memos alteram o custo. (`app/chamados/juizes.py:977–995`, `1013–1024`, `1402–1488`, `755–800`; `app/chamados/models.py:1905–1909`) Quantidade de queries e duração atuais em produção: **[nao verificado]**.

**Como o rc chega ao placar:**

1. O comando levanta `SystemExit(2)`; o envelope captura a saída, grava `exit` no JSONL e devolve o mesmo rc. Rc 2 também deixa carimbo de conclusão. (`app/chamados/management/commands/fechar_becos_disputa.py:67–70`; `bin/cron_run.sh:11–25`, `38`)
2. `crons_quebrados.EXITS_DE_ALARME=(0,2)` exclui rc 2 dos quebrados; o envelope também não abre pauta de quebra por esse rc. (`app/core/services/crons_quebrados.py:16`, `46–47`; `bin/cron_run.sh:27–35`)
3. `pipeline_placar` acumula rc 2 em `alarmes` e pode mostrar a linha como `ALARME`. **Esse estado não entra na lista que determina o rc do próprio pipeline**: entram `ERRO`, `NAO_RODOU` e `DEPENDENCIA_QUEBRADA`. Assim, pode haver linha `ALARME` junto de `A5 OK`. (`app/core/management/commands/pipeline_placar.py:74–80`, `118–137`, `151–163`)
4. Um comando novo, sem cadastro e executado pelo envelope, aparece como `ORFAO`; isso sozinho não o inclui nos alarmes A5. (`app/core/management/commands/pipeline_placar.py:134–137`, `156–163`)

A opção **(a)** já dispõe de universo inicial, agenda e transporte de rc; porém sua mensagem atual atribui o achado ao **evento de beco**, significado que precisaria distinguir do contador geral. (`app/chamados/management/commands/fechar_becos_disputa.py:32–33`, `68–70`; `app/config/crons.py:663`)

**3. Quem escreve, quem lê e onde a semântica diverge.**

**Escritores de `fechada_em`:**

- `DisputaSupervisao.fechar()` grava instante, autor, resolução e motivo. (`app/chamados/models.py:1996–2000`)
- `reabrir_admin()` e `reabrir_sistema()` limpam esses campos. (`app/chamados/models.py:2213–2225`, `2237–2241`)
- `pode_fechar()` é leitura: produz bool; não consulta o próprio `fechada_em`, o pai nem flags de operação em curso. (`app/chamados/models.py:1911–1917`)

**Chamadores diretos de C7 identificados no código:**

| Finalidade | Leitores |
|---|---|
| Classificação/apresentação | Relatório e explicador. (`app/chamados/relatorios_disputa.py:24`; `app/chamados/services/explicador.py:139`) |
| Beco e tranca | Classificar/fechar beco; medir/aplicar encerramento da competência. (`app/chamados/services/becos.py:33`, `56`; `app/chamados/services/competencia_trancada.py:178`, `227`) |
| Emissão/materialização | Autoencerramento, resolução por chamado e conclusão da materialização. (`app/chamados/services/disputa_emissao.py:1292`, `2112`; `app/chamados/services/materializacao.py:591`, `1322`) |
| Reconciliação | Fechamento das disputas alcançadas pelos respectivos atos. (`app/chamados/reconciliador.py:412`, `612`, `1171`) |
| Entrada/foto | Sincronização de entrada e upload de foto. (`app/ponto/services/entrada_sincronizada.py:59`; `app/api/foto_recebida.py:83`) |

**Leitores de `fechada_em` com efeitos ou apresentação relevantes:**

- Signals pedem fechamento do pai, retomam regularizações e materializam pendentes aprovadas. (`app/chamados/signals.py:29–46`, `131–146`, `155–165`)
- Regularização externa bloqueia qualquer disputa aberta; o helper público também procura qualquer aberta. (`app/chamados/services/regularizacao_ext.py:59–66`; `app/chamados/services/disputa_emissao.py:34–42`)
- App particiona pendência/histórico; API seleciona abertas; tela conta abertas. (`app/chamados/services/pergunta_no_app.py:76–85`; `app/api/views.py:1016–1035`; `app/chamados/views.py:909–912`)
- Situação e painel identificam colaborador com disputa aberta. (`app/colaboradores/services/situacional.py:117`; `app/colaboradores/painel_cell.py:31–32`)

**ESMERIL-SEMANTICA — diferenças concretas:**

- C7=True **não significa “tudo validado e gravado”**: aceita disputa vazia e perguntas de dias trancados; `desenlace` usa exatamente aquele texto para `pode_fechar=True`. (`app/chamados/models.py:1903–1917`; `app/chamados/services/desenlace.py:62–65`)
- O relatório rotula `PRONTA_FECHAR` como **“FALTA MATERIALIZAR”**, embora a classificação venha de C7. (`app/chamados/relatorios_disputa.py:24–25`; `app/chamados/management/commands/relatorio_pendencias_disputa.py:29`)
- Disputa vazia é fechável pelo C7, mas bloqueia o encerramento do pai no leitor `disputa_viva_do_chamado`. São respostas diferentes em domínios diferentes. (`app/chamados/models.py:1916–1917`; `app/chamados/juizes.py:2440–2446`; `app/chamados/models.py:153–160`)
- Há leitor legado que aceita **qualquer disputa fechada do colaborador** como prova de regularização de chamado de furo sem dia. (`app/chamados/models.py:187–190`)
- Rc 2 significa alarme de negócio, não cron quebrado; sua não propagação ao agregado A5 está inclusive fixada por teste. (`app/core/services/crons_quebrados.py:16`, `46`; `app/chamados/tests/test_contract_crons.py:186–208`)

**4. Testes existentes de `fechar_becos_disputa` e impacto.** [nao verificado]

| Teste | Contrato fixado | [nao verificado]
|---|---|
| `BecoPeloEventoDaPergunta.test_o_cron_virou_VIGIA_alarme_nao_escreve_e_sai_2` | `--alarme --apply` dá rc **2** e mantém a disputa aberta. (`app/chamados/tests/test_varredura_becos_evento_pergunta.py:97–109`) |
| `FecharBecosCommandTest.test_command_fecha_beco_pai_resolvido` | Apesar do nome, exige **C7=False e disputa aberta**, pois a pergunta continua viva. (`app/chamados/tests/test_fio_disputa_resolvido.py:149–156`) |
| `test_command_nao_fecha_pai_aberto` | `--apply` preserva disputa com pai aberto. (`app/chamados/tests/test_fio_disputa_resolvido.py:158–162`) |
| `test_command_dry_run_nao_fecha` | Execução sem `--apply` preserva aberta. (`app/chamados/tests/test_fio_disputa_resolvido.py:164–168`) |
| `test_idempotente_e_uma_linha_de_trilha` | Rodar `--apply` após o evento conserva uma trilha de fechamento. (`app/chamados/tests/test_varredura_becos_evento_pergunta.py:83–95`) |
| `test_o_cron_e_o_receiver_chamam_a_MESMA_porta` | Command delega a `fechar_beco`, sem incorporar o corpo de resolução. (`app/chamados/tests/test_varredura_becos_evento_pergunta.py:111–117`) |

Nos testes encontrados, stdout/stderr são capturados sem comparação textual; o assert explícito de rc é o primeiro acima. (`app/chamados/tests/test_fio_disputa_resolvido.py:152`, `160`, `166`; `app/chamados/tests/test_varredura_becos_evento_pergunta.py:106–109`)

**Se apenas o alarme ganhar o universo geral, preservando `--apply`, nenhum desses asserts precisa necessariamente mudar.** Eles não fixam rc normal nem saída para aberta+C7=True fora do beco; o novo contrato precisaria desses casos. (`app/chamados/tests/test_varredura_becos_evento_pergunta.py:97–117`; `app/chamados/tests/test_fio_disputa_resolvido.py:149–168`) Resultado de execução dos testes: **[nao verificado]**.

**5. Contraexemplos e resposta de C7.**

| Caso | Resposta de C7 e evidência |
|---|---|
| **Sem ata, aguardando informação** | **True para disputa vazia**, mas o emissor deliberadamente não fecha quando `_absteve_por_ata`. O teste fixa zero perguntas, disputa aberta e aviso `auto_fecho_disputa`. É um contraexemplo concreto à inferência “C7=True implica fechamento esquecido”. (`app/chamados/models.py:1916–1917`; `app/chamados/juizes.py:1516`; `app/chamados/services/disputa_emissao.py:1292–1301`; `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:51–61`) |
| **Pai vivo fora da competência encerrável** | **True**, explicitamente: o código passa por C7 e depois conserva a disputa em `disputas_com_pai_vivo_fora`, para evitar encerrar o pai por tabela. (`app/chamados/services/competencia_trancada.py:24–28`, `173–183`) |
| **Todas as perguntas em dias trancados** | **True**: C6 as considera mortas e a lista sem veredito exclui respostas trancadas. Há testes explícitos, inclusive com resposta do colaborador. Tranca isoladamente não prova que manter aberta seja o destino esperado: também há teste exigindo fechamento pela porta da tranca. (`app/chamados/juizes.py:1406–1409`; `app/chamados/models.py:1903–1909`; `app/chamados/tests/test_c7_red_minimos.py:361–382`, `329–350`) |
| **Materialização em curso** | C7 **não testa “em voo”**; o S143 pula esse fechamento por guarda anterior. Portanto pode continuar True conforme as perguntas. A flag é ContextVar dentro de transação; observação desse intervalo por um cron externo: **[nao verificado]**. (`app/chamados/models.py:1911–1917`; `app/chamados/services/disputa_emissao.py:2110–2113`; `app/chamados/services/materializacao.py:1137–1161`) |
| **Pai legitimamente aguardando conversa humana** | Isso não determina sozinho C7 da disputa: o gate do pai admite que a disputa feche enquanto ele permanece humano. Se a própria disputa possui resposta sem veredito, não materializada e fora da tranca, C7 responde **False**. (`app/chamados/models.py:699–712`, `1905–1917`) |

**Os casos acima demonstram que “aberta+C7=True” não equivale automaticamente a “o evento de beco falhou”.** No contador literal solicitado, esses estados entram; uma regra que os dispense de alarmar não está estabelecida pelo material fornecido: **[nao verificado]**. (`app/chamados/services/becos.py:25–33`; `app/chamados/services/disputa_emissao.py:1292–1295`; `app/chamados/services/competencia_trancada.py:178–183`)

## 15/11 19:50 · sessao 4317c1388896 · - · - · origem=Ronald (tmux)

Inventário de leitura/escrita consolidado:

- `fechada_em`: `fechar()` grava o instante em `app/chamados/models.py:1996` e salva junto com autor, resolução e motivo em `:2000`; `reabrir_admin()` limpa em `:2213` e salva em `:2225`; `reabrir_sistema()` limpa em `:2237` e salva em `:2241`. Desligamento chama a porta, sem `update` paralelo: `app/colaboradores/services/desligamento.py:164-167`.
- `pode_fechar()` calcula somente ausência de resposta sem veredito e ausência de fio vivo; não verifica aberta/fechada, pai ou operação em curso: `app/chamados/models.py:1911-1917`.
- Chamadores C7: `app/chamados/relatorios_disputa.py:24`; `app/api/foto_recebida.py:83`; `app/chamados/services/becos.py:33,56`; `app/chamados/services/competencia_trancada.py:178,227`; `app/chamados/services/explicador.py:139`; `app/chamados/services/disputa_emissao.py:1292,2112`; `app/chamados/services/materializacao.py:591,1322`; `app/chamados/reconciliador.py:412,612,1171`; `app/ponto/services/entrada_sincronizada.py:59`.
- Leitores estruturais de `fechada_em`: signals fecham pai pelo gate (`app/chamados/signals.py:29-46`), retomam regularizações após commit (`:131-146`) e materializam pendentes aprovadas (`:155-165`); regularização externa bloqueia qualquer aberta (`app/chamados/services/regularizacao_ext.py:59-66`); app usa aberta para pendência/histórico (`app/chamados/services/pergunta_no_app.py:81-85`; `app/api/views.py:1016-1035`); tela conta todas abertas (`app/chamados/views.py:909-912`).
- Fora chamados: mapa de situação (`app/colaboradores/services/situacional.py:117`), célula de painel (`app/colaboradores/painel_cell.py:31-32`), banner (`app/colaboradores/services/drawer.py:69-78`). Helper central consulta qualquer aberta em `app/chamados/services/disputa_emissao.py:34-42`; consumidores indiretos: `app/ponto/services/disputa.py:31`, `app/ponto/views.py:143-144`, `app/api/views.py:1335-1336`, `app/colaboradores/mixins.py:304-307`, `app/relatorios/pdf_espelho.py:1082-1085`. Comportamento indireto da folha: [nao verificado].

Semânticas divergentes:

- Disputa vazia tem C7=True (`app/chamados/models.py:1916-1917`; `app/chamados/juizes.py:1516`), mas relatório classifica antes como `SEM_PERGUNTA` (`app/chamados/relatorios_disputa.py:13-14`) e juiz do bloqueio do pai considera vazia bloqueadora (`app/chamados/juizes.py:2440-2446`; consumidor `app/chamados/models.py:153-160`).
- Leitor legado de chamado de furo sem dia considera existência de qualquer disputa fechada do colaborador prova de regularização: `app/chamados/models.py:187-190`.
- `EXITS_DE_ALARME=(0,2)` exclui rc 2 de cron quebrado (`app/core/services/crons_quebrados.py:16,46-47`); pipeline distingue rc 2 como alarme (`app/core/management/commands/pipeline_placar.py:76-80`), mostra `ALARME` (`:124-125`), porém não o inclui no agregado A5 (`:131-132`), que determina seu próprio rc (`:158-163`).

Contraexemplos de aberta+C7=True:

- **Sem ata:** escritor deliberadamente preserva aberta mesmo C7=True quando `_absteve_por_ata` (`app/chamados/services/disputa_emissao.py:1292-1295`); teste `DisputaMudaSemAtaTest.test_sem_ata_nao_fecha_e_acende` fixa perguntas=0 e aberta (`app/chamados/tests/test_hx_disputa_muda_sem_ata.py:51-61`). É contraexemplo concreto de teste/código, não ocorrência demonstrada em produção.
- **Pai vivo fora da tranca:** medidor só depois de C7=True preserva disputa cujo pai vivo não será encerrado (`app/chamados/services/competencia_trancada.py:173-183`); propósito declarado em `:24-28`.
- **Materialização em voo:** fecho S143 pula disputa com pergunta em voo antes de C7 (`app/chamados/services/disputa_emissao.py:2110-2113`); sinal é ContextVar dentro de atomic (`app/chamados/services/materializacao.py:1137-1161`). C7 não olha esse sinal (`app/chamados/models.py:1911-1917`); observabilidade desse intervalo por cron externo: [nao verificado].
- **Tranca:** C6 retorna False na tranca (`app/chamados/juizes.py:1406-1409`) e lista sem veredito exclui resposta trancada (`app/chamados/models.py:1903-1909`); portanto, se todas as perguntas estão assim, C7=True. Testes fixam isso em `app/chamados/tests/test_c7_red_minimos.py:361-367,377-382`. Tranca sozinha não justifica afirmar C7=False.
- **Conversa legítima do pai:** guarda do pai e guarda da disputa têm domínios distintos; pai pode ficar humano enquanto disputa fecha (`app/chamados/models.py:699-712`). C7=False quando a própria disputa possui resposta não materializada nem validada fora da tranca (`app/chamados/models.py:1905-1917`).

## 15/11 19:50 · sessao 0792a45ace27 · - · - · origem=Ronald (tmux)

Leitura concluída, sem alterações. Evidências para Q1/Q2:

- **Existe contador parcial de aberta+C7=True:** `fechar_becos_disputa` seleciona todas as abertas, mas chama C7 somente após `becos.classificar` exigir pai terminal e nenhuma pergunta visível; imprime candidatos e dá rc2 com `--alarme`. Universo: becos, sem janela temporal. `app/chamados/management/commands/fechar_becos_disputa.py:32–44,67–70`; `app/chamados/services/becos.py:25–33`.
- **Existe outra classificação parcial:** relatório de pendências percorre todas abertas e classifica `PRONTA_FECHAR` pelo C7, exceto as sem perguntas, classificadas antes como `SEM_PERGUNTA`. Command imprime categorias/contagens sem rc2; é sob demanda. `app/chamados/relatorios_disputa.py:11–28,45–58`; `app/chamados/management/commands/relatorio_pendencias_disputa.py:14–40`; `app/config/crons.py:1463`.
- **Existe contador de competência trancada:** `medir` pergunta C7 às abertas vinculadas às perguntas da competência e conta `disputas`; exclui pai vivo fora do conjunto de chamados que encerra. `medir_frota` soma por competências com tranca viva; command imprime contagens e soma chamados+perguntas+disputas, sem rc2. Está no `placar_code`, sem cron próprio. `app/chamados/services/competencia_trancada.py:119–126,173–183,331–356`; `app/chamados/management/commands/chamados_em_competencia_trancada.py:12–30`; `app/config/crons.py:1291–1297`; `bin/placar_code.sh:287–289`.
- **Não encontrei contador exato global de todas abertas+C7=True nos caminhos lidos.** O vigia de carimbo conta perguntas hora/data_hora validadas, independentemente de disputa aberta/fechada; não chama C7. `app/chamados/services/vigia_carimbo.py:95–132`. `desenlace` apresenta estado/ação recebendo explicação, sem varredura nem contagem. `app/chamados/services/desenlace.py:44–67`. Reconciliador chama C7 para fechar disputas atingidas por determinados eventos, não para censo global: `app/chamados/reconciliador.py:411–414,600–615,1158–1173`.

**Sítios possíveis, ainda sem implementação:**

1. **Estender `fechar_becos_disputa --alarme`:** N chamadas C7 para N abertas; hoje são B chamadas, B=becos candidatos à classificação. Se a implementação acrescentar consulta global e conservar `classificar` sem reaproveitamento, seriam N+B. `app/chamados/management/commands/fechar_becos_disputa.py:32–43`; `app/chamados/services/becos.py:25–33`. Já agendado 07:10 com `--alarme`. `app/config/crons.py:663`.
2. **Acrescentar ao vigia de carimbo:** N chamadas C7 novas além do seu universo atual P de perguntas validadas; a alimentação atual não cobre todas disputas abertas. Ele já executa `--alarme`, como elo após fio morto; rc2 hoje significa censo cego, e passivo só entra com outra flag. `app/chamados/services/vigia_carimbo.py:97–132`; `app/chamados/management/commands/vigiar_carimbo_sem_registro.py:38–39,56–78`; `app/config/crons.py:1770–1776`.
3. **Acrescentar a `reconciliar_chamados --alarme`:** N chamadas C7 novas a cada 15 minutos. Universo atual é chamados vivos, perguntas e resíduos do outbox, com exclusões próprias por evento pendente; não representa todas abertas. `app/chamados/management/commands/reconciliar_chamados.py:57–100,118–124`; `app/config/crons.py:395`.
4. **Relatório/contador sob demanda:** reaproveitar relatório exige incluir vazias para equivaler ao predicado exato; hoje são N−E chamadas C7, E=abertas sem perguntas. Não possui caminho próprio de rc2 nem agendamento. `app/chamados/relatorios_disputa.py:11–25`; `app/chamados/management/commands/relatorio_pendencias_disputa.py:11–40`; `app/config/crons.py:1463`.

**Custo comum do C7:** carrega lista de perguntas, avalia respostas sem veredito e, se nenhuma, `fio_vivo` com curto circuito. `app/chamados/models.py:1889–1917`; `app/chamados/juizes.py:1507–1516`. Respostas sem veredito podem consultar tranca por pergunta; C6 consulta tranca, célula/ata/lâmpadas, classificação de falta e testemunha de materialização conforme os ramos percorridos. Contagem exata de SQL depende dos estados e caches; não é constante por disputa. `app/chamados/models.py:1905–1909`; `app/chamados/juizes.py:977–995,1013–1024,1402–1488`. Tempo/quantidade de queries na produção atual: [nao verificado].

**Caminho do rc2 ao placar e semântica:**

- `cron_run.sh` recolhe rc, grava JSONL e devolve rc; rc2 também deixa carimbo de fim e não gera pauta de cron quebrado. `bin/cron_run.sh:11–13,19–25,27–38`.
- `crons_quebrados` usa `EXITS_DE_ALARME=(0,2)` e exclui ambos do contador de quebrados. `app/core/services/crons_quebrados.py:16,39–47`.
- `pipeline_placar.medidos` conta rc2 como `alarmes`, separado de `erros`; `placar` pode mostrar situação `ALARME`. Entretanto só `ERRO`, `NAO_RODOU` e `DEPENDENCIA_QUEBRADA` entram na lista que faz o próprio pipeline sair rc2. Assim uma linha `ALARME` pode coexistir com `A5 OK` e saída normal. `app/core/management/commands/pipeline_placar.py:61–81,118–137,151–163`.
- Diferença adicional: `crons_quebrados` considera última corrida nas 26h; pipeline agrega todas corridas do dia. `app/core/services/crons_quebrados.py:17,39–46`; `app/core/management/commands/pipeline_placar.py:66–81`.
- Mudar o alarme de beco para universo global altera significado específico da mensagem atual “beco(s) que o evento nao fechou”; o transporte do rc2 permanece “alarme de negócio”. `app/chamados/management/commands/fechar_becos_disputa.py:68–70`; `bin/cron_run.sh:19–22`.

**Contraexemplo de código:** competência trancada conserva disputa aberta com C7=True quando seu pai vivo está fora do conjunto encerrável. É exclusão explícita para impedir fechamento do pai por tabela. `app/chamados/services/competencia_trancada.py:24–28,173–183`. Ocorrência concreta na produção: [nao verificado].

## 15/11 19:49 · sessao 15f9f9749b4d · - · - · origem=Ronald (tmux)

Inventário somente leitura, sem execução de testes. [nao verificado]

- **Único teste encontrado que fixa o rc do command:** `BecoPeloEventoDaPergunta.test_o_cron_virou_VIGIA_alarme_nao_escreve_e_sai_2`, em `app/chamados/tests/test_varredura_becos_evento_pergunta.py:97`; cria beco via `update` (:102–104), executa `--alarme --apply` (:106), exige `SystemExit(2)` (:107) e disputa ainda aberta (:109). Esse contrato continua válido ao ampliar o universo do alarme.
- **Não achei teste que compare texto de stdout/stderr desse command.** Os chamadores capturam `StringIO()` sem examinar seu conteúdo: `app/chamados/tests/test_fio_disputa_resolvido.py:152`, `:160`, `:166`; `app/chamados/tests/test_varredura_becos_evento_pergunta.py:93`, `:106`.
- **Outros contratos do command:** `test_command_fecha_beco_pai_resolvido` (:149), apesar do nome, exige `pode_fechar=False` e disputa aberta quando pergunta continua viva (:154–156); `test_command_nao_fecha_pai_aberto` (:158–162); `test_command_dry_run_nao_fecha` (:164–168), todos em `app/chamados/tests/test_fio_disputa_resolvido.py`.
- **Delegação e idempotência:** `test_idempotente_e_uma_linha_de_trilha`, em `app/chamados/tests/test_varredura_becos_evento_pergunta.py:83`, chama `--apply` (:93) e exige uma trilha (:94–95); `test_o_cron_e_o_receiver_chamam_a_MESMA_porta` (:111–117) exige referência a `fechar_beco` e proíbe corpo próprio contendo `resolver_perguntas_por_chamado_resolvido`.
- **Quais mudariam:** se a ampliação afetar somente a contagem de `--alarme` e preservar aplicação/porta, nenhum desses asserts precisa mudar obrigatoriamente; falta acrescentar teste que morda aberta+C7=True fora do beco, zero candidatos/rc normal e saída nova. A inexistência desses asserts decorre do inventário acima; o resultado após alteração é [nao verificado]. [nao verificado]

Casos úteis para pergunta 5:

1. **Pai aberto legítimo, pergunta já validada:** `test_pai_aberto_NAO_fecha`, em `app/chamados/tests/test_varredura_becos_evento_pergunta.py:71–81`, mantém pai aberto, valida a pergunta e exige disputa ainda aberta. O resultado `pode_fechar=True` é inferência do corpo de C7, que só consulta perguntas/respostas, sem pai: `app/chamados/models.py:1911–1917`. O filtro atual recusa esse universo antes de C7: `app/chamados/services/becos.py:29–33`. O teste não afirma que esse caso deva ou não alarmar.
2. **Dia trancado:** fixture `_trancada`, em `app/chamados/tests/test_c7_red_minimos.py:352–359`, cria disputa e pergunta, depois apenas `PeriodoFechado`; `test_RED_o_C6_le_a_tranca_sem_escrever` (:361–367) exige pergunta morta e `pode_fechar=True`; `test_RED_resposta_de_dia_trancado_nao_segura_a_disputa` (:377–382) exige `respostas_sem_veredito=[]` e `pode_fechar=True`, mesmo com resposta. Tranca é causa de C7=True, não exceção interna de alarme: `app/chamados/models.py:1903–1909`, `:1917`.
3. **“Não deveria alarmar” por estar em tranca:** não estabelecido pelo teste acima; pelo contrário, `test_RED_trancada_fecha_pela_porta_com_trilha_e_nao_carimba`, em `app/chamados/tests/test_c7_red_minimos.py:329–350`, exige que `competencia_trancada.encerrar` feche a disputa (:340–344). A política de tolerar a janela entre trancar e encerrar é [nao verificado].
4. **Fechamento em curso:** `_EM_CURSO` protege reentrada do evento, em `app/chamados/services/becos.py:62–80`; C7 não consulta essa estrutura: `app/chamados/models.py:1911–1917`. Logo C7 pode continuar True durante essa fase (inferência); flag global de “em curso” como exclusão do contador ou concorrência real de produção é [nao verificado].

## 15/11 19:42 · sessao 0de49a4c7cf5 · E3-RESTO-R3 · leitura · origem=Code

**(a) 4.2 muda a origem das empresas, a cobertura declarada e o alarme estrutural.** Hoje o vigia descobre empresas pelos retratos existentes; lavrador e frescor começam em `empresas_visiveis()`. Portanto, uma empresa visível sem foto desaparece do vigia antes de suas competências serem examinadas (`app/folha/vigia_retido_calado.py:46`, `app/folha/vigia_retido_calado.py:51`, `app/folha/management/commands/lavrar_retrato_folha.py:51`, `app/folha/retrato_folha.py:1410`).

O universo passa a ser **visíveis ∩ recorte recebido × competências por empresa**. A lista nomeada de faltas já existe; falta preenchê-la também para empresas sem nenhuma foto e consumi-la como estrutural. Hoje `ALARMA_SEMPRE` contém somente `cego`, que não acusa cobertura parcial quando existe algum bloqueado (`app/folha/vigia_retido_calado.py:58`, `app/folha/vigia_retido_calado.py:89`, `app/folha/management/commands/vigiar_retido_calado.py:16`).

**Chamadores e testes atingidos** [nao verificado]

| Classe | Sítios |
|---|---|
| Produção direta | Command importa e chama `censo`: `app/folha/management/commands/vigiar_retido_calado.py:14`, `app/folha/management/commands/vigiar_retido_calado.py:33`. |
| Produção indireta | Cron executa com `--alarme`, dependente do lavrador: `app/config/crons.py:1681`, `app/config/crons.py:1683`. |
| Testes ORM diretos | `app/folha/tests/test_vigia_retido_calado.py:29`, `app/folha/tests/test_vigia_retido_calado.py:90`, `app/folha/tests/test_vigia_retido_calado.py:126`, `app/folha/tests/test_vigia_retido_calado.py:139`, `app/folha/tests/test_vigia_retido_calado.py:140`, `app/folha/tests/test_vigia_retido_calado.py:161`. |
| Testes ORM pelo command | `app/folha/tests/test_vigia_retido_calado.py:62`, `app/folha/tests/test_vigia_retido_calado.py:65`, `app/folha/tests/test_vigia_retido_calado.py:67`, `app/folha/tests/test_vigia_retido_calado.py:149`; interceptação com função real em `app/folha/tests/test_vigia_retido_calado.py:148`. |
| Testes sem banco, função real extraída | `app/chamados/tests/test_e3_r3_c4_sem_banco.py:154`, `app/chamados/tests/test_e3_r3_c4_sem_banco.py:157`, `app/chamados/tests/test_e3_r3_c5_sem_banco.py:103`, `app/chamados/tests/test_e3_r3_c6_sem_banco.py:253`, `app/chamados/tests/test_e3_r3_c14_sem_banco.py:350`, `app/chamados/tests/test_e3_r3_c15_sem_banco.py:85`, `app/chamados/tests/test_e3_r3_c16_sem_banco.py:74`. |
| Testes negativos da assinatura | Omissão de `hoje` deve continuar recusada: `app/chamados/tests/test_e3_r3_c5_sem_banco.py:243`, `app/chamados/tests/test_e3_r3_c5_sem_banco.py:245`. |
| Testes do command com substituto de `censo` | `app/chamados/tests/test_e3_r3_c11_sem_banco.py:275`, `app/chamados/tests/test_e3_r3_c11_sem_banco.py:281`, `app/chamados/tests/test_e3_r3_c14_sem_banco.py:317`, `app/chamados/tests/test_e3_r3_c14_sem_banco.py:321`. |

Precisam de mudança ou ampliação:

- **Sem foto alguma:** exigir os pares ausentes nomeados, além de `cego=1`; os casos atuais só cobram vazio/calados/cego (`app/folha/tests/test_vigia_retido_calado.py:31`, `app/chamados/tests/test_e3_r3_c16_sem_banco.py:111`).
- **Cobertura parcial estrutural:** o teste atual aceita `--alarme` com apenas a foto 10, faltando 11; esse resultado passa a ser alarme, ou a fixture precisa completar a cobertura (`app/folha/tests/test_vigia_retido_calado.py:64`, `app/folha/tests/test_vigia_retido_calado.py:65`).
- **Positivo preservado apesar da falta:** acrescentar a prova do alarme ao que já verifica falta nomeada e positivo conhecido (`app/folha/tests/test_vigia_retido_calado.py:186`, `app/chamados/tests/test_e3_r3_c16_sem_banco.py:103`).
- **Visibilidade e recorte:** acrescentar visível sem foto, inativa/diagnóstica com foto, seleção mista e seleção vazia; o selo atual cobre compartilhamento das competências, sem cobrar a origem das empresas (`app/chamados/tests/test_e3_r3_c16_sem_banco.py:127`, `app/chamados/tests/test_e3_r3_c16_sem_banco.py:133`).
- **Infraestrutura sem banco:** adaptar os substitutos acima à consulta de empresas visíveis; na fase leitora eles oferecem essencialmente retratos e serviços auxiliares (`app/chamados/tests/test_e3_r3_c4_sem_banco.py:149`, `app/chamados/tests/test_e3_r3_c5_sem_banco.py:96`, `app/chamados/tests/test_e3_r3_c6_sem_banco.py:246`, `app/chamados/tests/test_e3_r3_c14_sem_banco.py:341`, `app/chamados/tests/test_e3_r3_c15_sem_banco.py:77`, `app/chamados/tests/test_e3_r3_c16_sem_banco.py:63`).
- **Substituto do command:** acrescentar a saída estrutural; hoje o dict c11 só fornece três chaves, enquanto o command acessa `d[k]` (`app/chamados/tests/test_e3_r3_c11_sem_banco.py:275`, `app/folha/management/commands/vigiar_retido_calado.py:38`).
- **Mutantes:** manter alvos vivos após refatorar o loop; os de seleção/falta dependem das expressões atuais (`app/chamados/tests/e3_r3_c16_mutantes.py:26`, `app/chamados/tests/e3_r3_c16_mutantes.py:28`).

**Campos: quem escreve, quem lê e significado**

4.2 pode conservar as nove chaves existentes: a mudança é de seleção e cobertura; `censo` monta um dict em memória e o devolve (`app/folha/vigia_retido_calado.py:61`, `app/folha/vigia_retido_calado.py:90`).

| Campo/valor | Quem escreve → quem lê; significado |
|---|---|
| `--empresa` → `empresas` | Command produz IDs ou `None`; vigia lê o recorte. O lavrador já intersecta sua PK com visíveis; o vigia atual apenas filtra fotos (`app/folha/management/commands/vigiar_retido_calado.py:31`, `app/folha/vigia_retido_calado.py:47`, `app/folha/management/commands/lavrar_retrato_folha.py:53`). |
| Empresa: `pk`, `ativa`, `cnpj`, `razao_social`, `dia_inicio_competencia` | Cadastro encaminha valores às portas `criar_empresa`/`atualizar_empresa`; elas gravam. Há também normalização de CNPJ. Visibilidade lê ativa/CNPJ e ordena razão social; janelas leem o corte (`app/colaboradores/views.py:521`, `app/colaboradores/views.py:532`, `app/colaboradores/services/estrutura.py:15`, `app/colaboradores/services/estrutura.py:21`, `app/colaboradores/services/estrutura.py:28`, `app/colaboradores/queries.py:85`, `app/ponto/janelas.py:50`). |
| `hoje` | Command fornece o dia da borda; vigia entrega ao helper de competências, ao balde e à C5 (`app/folha/management/commands/vigiar_retido_calado.py:33`, `app/folha/vigia_retido_calado.py:55`, `app/folha/vigia_retido_calado.py:60`, `app/folha/vigia_retido_calado.py:66`). |
| `(mes, ano)` esperado | `competencias_do_retrato` produz fins de anterior/atual, sem repetição; lavrador, frescor e vigia consomem. É competência pelo corte empresarial (`app/folha/retrato_folha.py:1380`, `app/folha/retrato_folha.py:1382`, `app/folha/management/commands/lavrar_retrato_folha.py:61`, `app/folha/retrato_folha.py:1411`, `app/folha/vigia_retido_calado.py:55`). |
| Foto: `empresa/mes/ano/linhas/lavrado_em` | Lavrador grava por trio, com linhas e instante inicial da rodada. Vigia lê par/linhas; frescor lê também o carimbo. `lavrado_em` não seleciona o universo do vigia (`app/folha/management/commands/lavrar_retrato_folha.py:77`, `app/folha/management/commands/lavrar_retrato_folha.py:79`, `app/folha/vigia_retido_calado.py:56`, `app/folha/vigia_retido_calado.py:65`, `app/folha/retrato_folha.py:1413`). |
| Linha: `colab/status/motivo/datas/dono/dias_sem_leitura` | `montar_linhas` escreve; porta de bloqueados lê status/motivo, dono lê fio/datas, vigia lê colaborador e presença/tamanho dos dias sem leitura. **`dono` gravado significa fio**, depois projetado em balde (`app/folha/retrato_folha.py:1237`, `app/folha/retrato_folha.py:1248`, `app/folha/retrato_folha.py:91`, `app/folha/retrato_folha.py:446`, `app/folha/retrato_folha.py:490`, `app/folha/vigia_retido_calado.py:67`, `app/folha/vigia_retido_calado.py:72`). |
| C5: conjunto de colaboradores roteados | `colabs_nunca_bateu` produz; vigia cruza com `linha.colab`. Deve receber os IDs **efetivos** do recorte visível: hoje `None` significa todas as empresas no auxiliar. Sua janela continua anterior.início até ontem (`app/escala/services/cadastro_realidade.py:163`, `app/escala/services/cadastro_realidade.py:167`, `app/folha/vigia_retido_calado.py:60`, `app/folha/vigia_retido_calado.py:67`). |

| Saída do dict | Escritor → leitor; efeito |
|---|---|
| `retratos` | Vigia acrescenta `(empresa,mes,ano)`; formatador conta e `cego` verifica vazio. Exclui fotos de empresas fora do universo (`app/folha/vigia_retido_calado.py:76`, `app/folha/vigia_retido_calado.py:95`, `app/folha/vigia_retido_calado.py:89`). |
| `competencias_sem_retrato` | Vigia produz; formatador conta e nomeia. Passa a abranger empresa sem foto alguma e a alimentar o estrutural (`app/folha/vigia_retido_calado.py:58`, `app/folha/vigia_retido_calado.py:61`, `app/folha/vigia_retido_calado.py:95`, `app/folha/vigia_retido_calado.py:96`). |
| `bloqueados/calados/roteados` | Vigia soma; formatador publica. Command lê `calados` no passivo. Unidade permanece pessoa por retrato/competência (`app/folha/vigia_retido_calado.py:16`, `app/folha/vigia_retido_calado.py:77`, `app/folha/vigia_retido_calado.py:99`, `app/folha/vigia_retido_calado.py:101`, `app/folha/vigia_retido_calado.py:102`, `app/folha/management/commands/vigiar_retido_calado.py:17`). |
| `dias_sem_leitura_sistema` | Vigia agrega; formatador e alarme passivo leem. Positivo conhecido sobrevive; `None` permanece distinto de zero (`app/folha/vigia_retido_calado.py:88`, `app/folha/vigia_retido_calado.py:103`, `app/folha/management/commands/vigiar_retido_calado.py:17`). |
| `linhas_sem_campo_leitura` | Vigia soma; formatador publica. Mede legado sem campo, não competência sem foto (`app/folha/vigia_retido_calado.py:74`, `app/folha/vigia_retido_calado.py:80`, `app/folha/vigia_retido_calado.py:104`). |
| `por_empresa` | Vigia grava mês/ano e contadores; formatador lê mês/ano/bloqueados/calados/roteados. A chave é **`(empresa,ano,mes)`**, ordem diferente das duas listas; testes leem também o aninhado sem leitura (`app/folha/vigia_retido_calado.py:83`, `app/folha/vigia_retido_calado.py:98`, `app/chamados/tests/test_e3_r3_c14_sem_banco.py:309`). |
| `cego` | Vigia calcula “sem fotos ou sem bloqueados”; formatador e estrutural atual leem. Não substitui o contador de pares ausentes (`app/folha/vigia_retido_calado.py:89`, `app/folha/vigia_retido_calado.py:102`, `app/folha/management/commands/vigiar_retido_calado.py:16`). |

**Não há significado uniforme em todos os consumidores.** Frescor usa visíveis; a tela pode selecionar empresa oculta por PK; C5 com `None` usa todas. Competência automática vem da janela, mas lavra explícita e URL podem indicar outro par. O dia civil do carimbo não é o mês da competência (`app/folha/retrato_folha.py:1410`, `app/folha/views_retrato.py:161`, `app/escala/services/cadastro_realidade.py:163`, `app/folha/management/commands/lavrar_retrato_folha.py:57`, `app/folha/views_retrato.py:181`, `app/ponto/janelas.py:11`).

**Destinos das saídas**

| Destino | O que muda |
|---|---|
| Cron | Mantém `--alarme`; falta parcial passa a poder produzir rc2. Comentário “só pelo cego” precisa acompanhar o novo estrutural (`app/config/crons.py:1679`, `app/config/crons.py:1681`, `app/folha/management/commands/vigiar_retido_calado.py:41`). |
| Placar | Não lê o dict VRC: envelope grava `exit`; placar conta `exit==2` como alarme. Muda potencialmente o valor, sem exigir nova chave no placar (`bin/cron_run.sh:13`, `app/core/management/commands/pipeline_placar.py:77`). |
| Tela | Não consome o dict VRC: consulta fotos e frescor. Seu aviso continua eliminando empresas sem foto; 4.2 sozinho não corrige isso (`app/folha/views_retrato.py:192`, `app/folha/views_retrato.py:209`, `app/folha/views_retrato.py:210`). |
| RELATO | É registro histórico de universo/medidas, não leitor executável. Números novos de produção: **[nao verificado]** (`app/docs/RELATO.md:4349`, `app/docs/RELATO.md:1644`). |
| Contador de frescor | `retratos_desatualizados` permanece separado, contando também `nunca lavrado`; não recebe VRC (`app/folha/retrato_folha.py:1454`). |

**Outros universos próprios**

Não consigo certificar a lista **exata dentro do diff `f31a8d09e × base geral`**: comparação **[nao verificado]**. Os artefatos disponíveis identificam bases parciais diferentes, LU3 e c12 (`app/chamados/tests/e3_lu3_entrega.txt:2`, `app/chamados/tests/e3_r3_c12_entrega.txt:1`).

Na cópia, além do vigia retido, permanecem:

| Leitor | Seleção própria |
|---|---|
| Recorte de tela/PDF/raiox | PK/preferência pode escapar das visíveis; `todas` aplica a competência atual da primeira empresa; URL e navegação escolhem outros pares (`app/folha/views_retrato.py:161`, `app/folha/views_retrato.py:179`, `app/folha/views_retrato.py:181`, `app/folha/views_retrato.py:189`, `app/folha/views_retrato.py:192`). |
| Aviso de frescor | Restringe às empresas com foto existente (`app/folha/views_retrato.py:209`, `app/folha/views_retrato.py:211`). |
| Painel do colaborador | Empresa do colaborador cruzada com visíveis; par exato da URL (`app/colaboradores/views.py:975`, `app/colaboradores/views.py:982`). |
| Vigia fio, alcance | Visíveis/interseção; atual e anterior somente se não entregue (`app/chamados/services/vigia_fio.py:517`, `app/chamados/services/vigia_fio.py:535`, `app/ponto/services/he_pendente_lavrado.py:223`). |
| Vigia fio antigo / ListaDoAdmin | Antigo parte de chamados vivos com célula; lista cruza visíveis separadamente (`app/chamados/services/vigia_fio.py:290`, `app/chamados/services/vigia_fio.py:292`, `app/chamados/services/vigia_fio.py:191`). |
| C5 subsidiária | Todas quando `None`; anterior.início até ontem (`app/escala/services/cadastro_realidade.py:163`, `app/escala/services/cadastro_realidade.py:167`). |
| Drawer | Recompõe anterior por `janela_atual(início_atual−1 dia)` e aplica piso visual (`app/colaboradores/services/drawer.py:115`, `app/colaboradores/services/drawer.py:116`, `app/colaboradores/services/drawer.py:118`). |
| Calendário / consulta plural | Modos civil/competência/custom escolhem períodos; consulta plural calcula a atual para excluir dias da oferta (`app/colaboradores/services/calendario.py:194`, `app/colaboradores/services/calendario.py:205`, `app/colaboradores/services/calendario.py:227`, `app/colaboradores/services/calendario.py:143`). |
| `CargaDaTarefa._ler_exportadas` | Deriva competência de cada dia recebido (`app/chamados/services/tarefa_do_dia.py:471`, `app/chamados/services/tarefa_do_dia.py:482`). |
| `escalas_em_limbo` | Escalas ativas e foto `FolgaDia` do mês civil; é universo diagnóstico próprio (`app/relatorios/furos.py:60`, `app/relatorios/furos.py:88`). |

Calendário, drawer, vigia fio e dataset constam do diff parcial LU3 documentado; isso não prova a pertença de todos esses sítios ao diff integral solicitado (`app/chamados/tests/e3_lu3_entrega.txt:151`, `app/chamados/tests/e3_lu3_entrega.txt:163`, `app/chamados/tests/e3_lu3_entrega.txt:164`, `app/chamados/tests/e3_lu3_entrega.txt:168`).

**(b) A lista decidida não equivale ao conjunto de relógios no salto 1.**

| Classificação | Sítio e motivo |
|---|---|
| Dentro correto | `furos_espelho → escalas_em_limbo`: `localdate()` próprio no salto 1, apesar do instante/dia disponíveis na view (`app/relatorios/views.py:1519`, `app/relatorios/views.py:1520`, `app/relatorios/views.py:1554`, `app/relatorios/furos.py:59`). |
| Falta no conjunto salto 1, mas fora expresso | PDF → `gerar_pdf_auditavel`: outro `now()`; recorte devolve somente o dia (`app/folha/views_retrato.py:245`, `app/folha/views_retrato.py:441`, `app/relatorios/pdf_base.py:133`). |
| Falta no conjunto salto 1, mas fora expresso | Raiox → `placar_aceite`: **duas chamadas**, `now()` e `localdate()`, no ramo colab (`app/folha/views_retrato.py:511`, `app/folha/views_retrato.py:513`, `app/chamados/services/verbos.py:286`, `app/chamados/services/verbos.py:287`). |
| Falta sintática, fallback suprimido | Calendário/drawer → `contexto_calendario`: contém `now()`, mas recebe instante nesses caminhos (`app/colaboradores/views.py:1952`, `app/colaboradores/services/drawer.py:124`, `app/colaboradores/services/calendario.py:192`). |
| Falta sintática considerando também auxiliares do anexo | `ListaDoAdmin._carregar → lista`: `now()` no salto 1; suprimido quando `self.agora` foi fornecido (`app/chamados/services/vigia_fio.py:179`, `app/chamados/services/vigia_fio.py:194`, `app/escala/services/cadastro_realidade.py:780`). |
| Sobra **como sítio de relógio salto 1** | `dataset_furos` não chama relógio; repassa `agora`. Seu D4 fica no salto 2. Exigir argumento continua sendo correção de contrato dentro do escopo decidido (`app/relatorios/furos.py:103`, `app/relatorios/furos.py:123`, `app/relatorios/furos.py:164`, `app/ponto/status_dia.py:253`). |

Selo e docstrings não são nós de relógio; não sobram da obra por isso. O defeito do selo é concreto: conjunto de nomes elimina chamadas repetidas, e o outro helper conta alvos de atribuição. A identificação exata do rótulo “docstrings G3” permanece **[nao verificado]** (`app/chamados/tests/test_e3_r3_c15_sem_banco.py:162`, `app/chamados/tests/test_e3_r3_c15_sem_banco.py:182`).

Essas faltas descrevem a diferença para o censo; PDF e placar continuam sendo exclusões expressas da decisão recebida **[nao verificado quanto a qualquer ampliação posterior do escopo]**.

**(c) Contrato proposto para c17 — execução e resultados [nao verificado].**

1. Visíveis ∩ `--empresa` governam vigia e C5 → teste da função real com visível/oculta, recorte misto/vazio e IDs entregues à C5 (`app/folha/vigia_retido_calado.py:46`, `app/folha/vigia_retido_calado.py:60`).
2. Empresa visível sem foto sai com todos os pares faltantes → ampliar o caso vazio para cobrar pares e saída nomeada (`app/chamados/tests/test_e3_r3_c16_sem_banco.py:111`, `app/folha/vigia_retido_calado.py:96`).
3. Falta parcial é estrutural, positivo permanece → teste real do command exige rc2 sem passivo e conserva contagens conhecidas (`app/folha/tests/test_vigia_retido_calado.py:65`, `app/chamados/tests/test_e3_r3_c16_sem_banco.py:103`).
4. Janela por empresa conserva meia-noite/relavra e exclui competência velha explícita → preservar casos comportamentais e mutantes com alvo vivo (`app/chamados/tests/test_e3_r3_c16_sem_banco.py:78`, `app/chamados/tests/test_e3_r3_c16_sem_banco.py:86`, `app/chamados/tests/test_e3_r3_c16_sem_banco.py:95`).
5. Unidades, legado/`None` e ordem dos trios permanecem explícitos → testes de pessoa/competência, dia-colab e positivo entre empresas (`app/folha/vigia_retido_calado.py:76`, `app/folha/vigia_retido_calado.py:83`, `app/chamados/tests/test_e3_r3_c14_sem_banco.py:324`).
6. Limbo usa o dia da borda → teste da função real junto à meia-noite, proibindo relógio interno e verificando o dia recebido (`app/relatorios/views.py:1554`, `app/relatorios/furos.py:59`).
7. `dataset_furos` exige instante válido → omissão/`None` recusados; teste real verifica transporte ao D4/calendário e atualiza chamadores de teste (`app/relatorios/furos.py:103`, `app/relatorios/furos.py:123`, `app/relatorios/tests/test_e4_8_furos_le_a_lavra.py:92`).
8. “Um relógio” conta chamadas → selo rejeita dois `now()`, chamada solta e alias; conversões com instante explícito não somam leitura (`app/chamados/tests/test_e3_r3_c15_sem_banco.py:159`, `app/chamados/tests/test_e3_r3_c15_sem_banco.py:180`).
9. Docstrings descrevem o contrato efetivo e suas exclusões → revisão G3 identificada, sem prometer fecho transitivo sem relógios (`app/folha/vigia_retido_calado.py:16`, `app/folha/retrato_folha.py:1372`, `app/relatorios/furos.py:109`).

## 15/11 19:40 · sessao dd6001c0d496 · - · - · origem=Ronald (tmux)

Todas as referências abaixo são relativas à raiz da cópia; não executei testes nem fiz alterações. [nao verificado]

**Chamadores de `censo`**

| Classe | Sítios |
|---|---|
| Produção direta | Import e chamada no command: `app/folha/management/commands/vigiar_retido_calado.py:14`, `:33`. |
| Produção indireta | Cron `ENC(... args='--alarme')`: `app/config/crons.py:1681`; dependência do lavrador `:1683`; papel `vigia` `:2260`. |
| Testes ORM diretos | Helpers `app/folha/tests/test_vigia_retido_calado.py:29`, `:161`; chamadas `:90`, `:126`, `:139`, `:140`; command interceptado com `wraps=vrc.censo` `:148`. |
| Testes ORM pelo command | `app/folha/tests/test_vigia_retido_calado.py:62`, `:65`, `:67`, `:149`. |
| Testes sem banco, função real extraída | `app/chamados/tests/test_e3_r3_c4_sem_banco.py:154`, `:157`; `test_e3_r3_c5_sem_banco.py:103`; `test_e3_r3_c6_sem_banco.py:253`; `test_e3_r3_c14_sem_banco.py:350`; `test_e3_r3_c15_sem_banco.py:85`; `test_e3_r3_c16_sem_banco.py:74`. |
| Chamadas negativas de assinatura | `app/chamados/tests/test_e3_r3_c5_sem_banco.py:243`, `:245`: sem `hoje`, devem continuar `TypeError`. |
| Testes do command com censo substituído | `app/chamados/tests/test_e3_r3_c11_sem_banco.py:275`, `:281`; `test_e3_r3_c14_sem_banco.py:317`, `:321`. |

**Retorno e leitores**

`censo` escreve as nove chaves: `retratos`, `competencias_sem_retrato`, `bloqueados`, `calados`, `roteados`, `dias_sem_leitura_sistema`, `linhas_sem_campo_leitura`, `por_empresa`, `cego` (`app/folha/vigia_retido_calado.py:61`, `:62`, `:88`, `:89`).

| Saída | Quem escreve | Quem lê e efeito de 4.2 |
|---|---|---|
| `retratos`: `(empresa,mes,ano)` | `app/folha/vigia_retido_calado.py:76` | `linhas` imprime quantidade `:95`; `cego` usa vazio `:89`. Passa a conter somente fotos nas visíveis/interseção e nas janelas. |
| `competencias_sem_retrato`: `(empresa,mes,ano)` | `app/folha/vigia_retido_calado.py:58`, `:61` | `linhas` conta `:95` e nomeia cada par `:96`. Precisa passar a incluir empresa visível sem **nenhuma** foto; hoje loop só existe a partir das fotos `:51`. |
| `bloqueados`, `calados`, `roteados` | `app/folha/vigia_retido_calado.py:77`, `:78`, `:79` | `linhas`: universo `:95`, roteados `:101`, contador `:102`; command alarma `calados` somente com passivo, `app/folha/management/commands/vigiar_retido_calado.py:17`, `:37`. Valores podem mudar pela seleção de fotos. |
| `dias_sem_leitura_sistema` | `app/folha/vigia_retido_calado.py:88` | `linhas` `:103`, `:105`; command passivo `app/folha/management/commands/vigiar_retido_calado.py:17`. Mantém positivo conhecido; `None` só sem soma conhecida e com legado `app/folha/vigia_retido_calado.py:88`. |
| `linhas_sem_campo_leitura` | `app/folha/vigia_retido_calado.py:80` | `linhas` `:104`, `:105`; muda somente seleção do universo. |
| `por_empresa`: chave `(empresa,ano,mes)` | `app/folha/vigia_retido_calado.py:83` | `linhas` `:98`, `:99` lê mês/ano/bloqueados/calados/roteados. Campos aninhados de leitura são cobrados nos testes `app/chamados/tests/test_e3_r3_c6_sem_banco.py:269`, `test_e3_r3_c14_sem_banco.py:309`, `:352`. **Ordem difere** de `retratos` e `competencias_sem_retrato`. |
| `cego` | `app/folha/vigia_retido_calado.py:89` | `linhas` `:102`; único estrutural atual, `app/folha/management/commands/vigiar_retido_calado.py:16`, acionado `:38`. Não identifica cobertura parcial se já existe um bloqueado. |

O contador de ausências **já é impresso**, mas ainda não é estrutural no alarme: o command só tem `ALARMA_SEMPRE=('cego',)` (`app/folha/management/commands/vigiar_retido_calado.py:16`), enquanto `competencias_sem_retrato` é apenas listado (`app/folha/vigia_retido_calado.py:94`, `:96`). Para cumprir “par sem retrato no contador estrutural”, é preciso consumir a falta no alarme, preservando sua lista nomeada; a alternativa exata de nome/schema adicional não foi decidida no texto **[nao verificado]**.

**Cron, placar, tela, RELATO, contador**

- Cron continua chamando o command com `--alarme`; comentário “só pelo cego” fica incorreto se a falta de par virar estrutural (`app/config/crons.py:1679`, `:1681`; `app/folha/management/commands/vigiar_retido_calado.py:5`).
- Placar não recebe o dict: `encadeado` passa pelo envelope (`bin/encadeado.sh:102`); o envelope grava `nome/emp/ini/fim/dur/exit` (`bin/cron_run.sh:13`); `pipeline_placar` conta `exit==2` como alarme (`app/core/management/commands/pipeline_placar.py:77`). Assim mudam potencialmente rc/alarmes/duração, sem nova chave no JSON do placar.
- Tela consulta fotos diretamente (`app/folha/views_retrato.py:192`), e frescor separado (`:210`); template mostra `frescor_aviso[*].rotulo_admin` (`app/templates/folha/_retrato_barra.html:22`). Não é migração de consumidor VRC: permanece divergência de universo porque aviso só aceita empresas com foto (`app/folha/views_retrato.py:209`, `:211`).
- `RELATO` registra medidas históricas, não é consumidor executável: explica origem/unidade e command (`app/docs/RELATO.md:4347`, `:4349`), conserva smoke histórico (`:4353`, `:1644`). Não inferir números novos desses registros **[nao verificado]**.
- Contador separado `retratos_desatualizados` é `len(frescor)` com `desatualizado` **ou** `nunca lavrado`, sem VRC (`app/folha/retrato_folha.py:1448`, `:1454`).

**Testes que precisam mudar ou ser ampliados** [nao verificado]

1. Empresa sem qualquer foto: exigir pares nomeados; hoje só exige `cego=1` (`app/folha/tests/test_vigia_retido_calado.py:31`, `:33`; sem banco `app/chamados/tests/test_e3_r3_c16_sem_banco.py:111`, `:113`).
2. Alarme de cobertura parcial: hoje aceita `--alarme` com só foto 10 e falta 11 (`app/folha/tests/test_vigia_retido_calado.py:64`, `:65`); passa a precisar foto 11 ou esperar rc2.
3. Caso de falta parcial preserva positivo (`app/folha/tests/test_vigia_retido_calado.py:186`, `:190`; sem banco `app/chamados/tests/test_e3_r3_c16_sem_banco.py:103`, `:108`): acrescentar verificação estrutural/command.
4. Selo compartilhado prova apenas helper de competências, não empresas (`app/chamados/tests/test_e3_r3_c16_sem_banco.py:127`, `:133`, `:134`): acrescentar visíveis/interseção e empresa sem fotos.
5. Adaptar fixtures sem banco ao import/consulta de `empresas_visiveis`; hoje c4/c5/c6/c14/c15/c16 só simulam fotos na fase leitora (`app/chamados/tests/test_e3_r3_c4_sem_banco.py:149`; `test_e3_r3_c5_sem_banco.py:96`; `test_e3_r3_c6_sem_banco.py:246`; `test_e3_r3_c14_sem_banco.py:341`; `test_e3_r3_c15_sem_banco.py:77`; `test_e3_r3_c16_sem_banco.py:63`).
6. Mock c11 precisa chave estrutural se command mantiver acesso `d[k]`; retorno atual tem apenas `cego/calados/dias_sem_leitura_sistema` (`app/chamados/tests/test_e3_r3_c11_sem_banco.py:275`; acesso `app/folha/management/commands/vigiar_retido_calado.py:38`).
7. Mutantes de seleção/falta c16 dependem de strings do loop antigo e precisam alvo vivo se refatorado (`app/chamados/tests/e3_r3_c16_mutantes.py:26`, `:28`). Mutantes de c4/c5/c6/c14 preservam contratos de balde/unidade/desconhecido (`e3_r3_c4_mutantes.py:33`, `e3_r3_c5_mutantes.py:44`, `e3_r3_c6_mutantes.py:45`, `e3_r3_c14_mutantes.py:53`).

**Insumos: escritor/leitor e significado**

- `Empresa.ativa/cnpj/razao_social`: visibilidade definida por `empresas_visiveis` = ativa, exclui CNPJ `11222333*`, ordena razão social (`app/colaboradores/queries.py:85`). Escritores desses campos não investigados **[nao verificado]**.
- `--empresa`: command escreve lista de IDs ou `None` (`app/folha/management/commands/vigiar_retido_calado.py:31`); censo lê filtro (`app/folha/vigia_retido_calado.py:47`). Lavrador usa uma PK intersectando visíveis (`app/folha/management/commands/lavrar_retrato_folha.py:51`, `:53`).
- `hoje`: command escreve pelo relógio da borda (`app/folha/management/commands/vigiar_retido_calado.py:33`); helper usa para janelas (`app/folha/retrato_folha.py:1379`), balde usa para hoje/futuro (`:491`), C5 recebe (`app/folha/vigia_retido_calado.py:60`).
- Foto `empresa/mes/ano/linhas/lavrado_em`: escritor lavrador `update_or_create` por trio, defaults linhas/carimbo (`app/folha/management/commands/lavrar_retrato_folha.py:77`); unicidade garantida (`app/folha/models.py:366`). Censo lê empresa/mes/ano/linhas (`app/folha/vigia_retido_calado.py:51`, `:56`, `:65`), não usa `lavrado_em` para universo; frescor lê carimbo (`app/folha/retrato_folha.py:1413`).
- Linhas `colab/status/motivo/dias/datas/dias_sem_leitura/dias_sem_porta/dono`: montador escreve (`app/folha/retrato_folha.py:1237`); VRC pede bloqueados pela porta `linhas_do_retrato` (`app/folha/vigia_retido_calado.py:65`), balde por `dono_da_linha` (`:66`), C5 pelo `colab` (`:67`), dias sem leitura por presença e tamanho (`:72`, `:73`). `dono` gravado significa **fio**, não balde (`app/folha/retrato_folha.py:1246`, `:1248`; leitor `:446`, `:486`).
- C5: hoje recebe IDs originais; `None` significa **todas** em `colabs_nunca_bateu`, inclusive ocultas (`app/folha/vigia_retido_calado.py:60`; `app/escala/services/cadastro_realidade.py:163`). C17 deve passar IDs efetivamente visíveis/intersectados para alinhar empresas; sua janela continua anterior até ontem (`cadastro_realidade.py:162`, `:167`), não “competência do dia civil”.
- Não há mesmo significado em todos os consumidores: consulta documenta que export/internos não usam visibilidade de propósito (`app/colaboradores/queries.py:82`); vigia hoje parte de todas as fotos (`app/folha/vigia_retido_calado.py:46`), frescor usa visíveis (`app/folha/retrato_folha.py:1410`), tela por PK pode pegar oculta (`app/folha/views_retrato.py:161`). Competência é fim da janela por empresa (`app/folha/retrato_folha.py:1380`), enquanto filtro civil `lavrado_em` é explicitamente recusado (`app/folha/vigia_retido_calado.py:23`).

## 15/11 19:39 · sessao bb4d3cb27c21 · - · - · origem=Ronald (tmux)

(b) **O conjunto de correções de relógio não coincide com “todos os sítios de relógio no salto 1”**; o anexo distingue leituras próprias, fallbacks suprimidos e omissões, e o código confirma essa distinção (`app/relatorios/furos.py:59`, `app/colaboradores/services/calendario.py:192`, `app/chamados/juizes.py:888`).

| Classificação | Sítio e motivo |
|---|---|
| **DENTRO correto** | `furos_espelho → escalas_em_limbo`: salto 1, leitura própria `timezone.localdate()`, embora a view já tenha `_agora` e `hoje`; falta transmitir o dia recebido (`app/relatorios/views.py:1519`, `app/relatorios/views.py:1520`, `app/relatorios/views.py:1554`, `app/relatorios/furos.py:59`). |
| **FALTA ao conjunto de sítios salto 1, mas FORA expresso** | `retrato_pdf → gerar_pdf_auditavel`: salto 1, outra chamada `timezone.now()`; `_recorte_da_barra` só devolve `hoje`, não `_agora` (`app/folha/views_retrato.py:245`, `app/folha/views_retrato.py:386`, `app/folha/views_retrato.py:441`, `app/relatorios/pdf_base.py:133`). |
| **FALTA ao conjunto de sítios salto 1, mas FORA expresso** | `retrato_raiox → placar_aceite`: salto 1, **duas** leituras próprias, `.now()` e `.localdate()`, no ramo `dono == ['colab']` (`app/folha/views_retrato.py:511`, `app/folha/views_retrato.py:513`, `app/chamados/services/verbos.py:286`, `app/chamados/services/verbos.py:287`). |
| **Sítio salto 1 ausente da lista; fallback já suprimido** | `calendario_ponto/contexto_drawer → contexto_calendario`: o helper contém `.now()`, mas ambas as bordas fornecem `agora`; não representa nova leitura nesses caminhos (`app/colaboradores/views.py:1952`, `app/colaboradores/services/drawer.py:123`, `app/colaboradores/services/drawer.py:124`, `app/colaboradores/services/calendario.py:192`). |
| **Sítio salto 1 adicional se incluídas as entradas auxiliares do anexo** | `ListaDoAdmin._carregar → cadastro_realidade.lista`: `.now()` no salto 1; fica suprimido quando `self.agora` foi fornecido, mas o construtor permite `None` (`app/chamados/services/vigia_fio.py:177`, `app/chamados/services/vigia_fio.py:179`, `app/chamados/services/vigia_fio.py:194`, `app/escala/services/cadastro_realidade.py:780`). |
| **SOBRA somente se a lista for intitulada “sítios de relógio salto 1”** | `dataset_furos` não lê relógio: tem `agora=None` e repassa o argumento ao D4 e calendário; o D4 está no salto 2 da view. Tornar `agora` obrigatório é endurecimento de contrato válido, não remoção de relógio próprio no salto 1 (`app/relatorios/furos.py:103`, `app/relatorios/furos.py:123`, `app/relatorios/furos.py:164`, `app/ponto/status_dia.py:253`). |

**Não classificaria selo e docstrings como SOBRA da obra:** são instrumentos e documentação, não nós de relógio do grafo; o defeito do selo é concreto, pois `_le_relogio` usa conjunto de nomes e `_instante_da_borda` conta alvos de atribuição (`app/chamados/tests/test_e3_r3_c15_sem_banco.py:162`, `app/chamados/tests/test_e3_r3_c15_sem_banco.py:182`). O selo do drawer só compara `['now']`; o do relatório combina esse conjunto com a quantidade de alvos, sem contar todas as chamadas (`app/chamados/tests/test_e3_r3_c16_sem_banco.py:152`, `app/chamados/tests/test_e3_r3_c16_sem_banco.py:156`, `app/chamados/tests/test_e3_r3_c16_sem_banco.py:158`).

**Não são relógios salto 1 faltantes:** `viva_em_disputa_fechada` e `carimbos_sem_lastro_por_dono` chamam `pergunta_viva` sem argumentos no salto 1, mas `pergunta_viva` não consulta relógio: apenas deriva o dia se recebeu instante; os relógios surgem nos degraus seguintes (`app/chamados/services/vigia_fio.py:718`, `app/chamados/services/vigia_fio.py:776`, `app/chamados/juizes.py:886`, `app/chamados/juizes.py:888`, `app/chamados/juizes.py:897`). A propriedade `estado` é salto 2 a partir de `dono_fora_do_fio`, não salto 1 dos censos principais (`app/chamados/services/vigia_fio.py:259`, `app/chamados/services/vigia_fio.py:387`, `app/chamados/services/vigia_fio.py:615`).

A identidade exata de “docstrings G3” não aparece nomeada nos arquivos examinados **[nao verificado]**. Documentação evidentemente afetada por 4.2: o vigia promete “universo do retrato gravado” e descreve faltantes, embora derive empresas apenas dos retratos existentes; a docstring do helper promete que os três consumidores medem o mesmo universo (`app/folha/vigia_retido_calado.py:16`, `app/folha/vigia_retido_calado.py:27`, `app/folha/vigia_retido_calado.py:51`, `app/folha/retrato_folha.py:1372`). O help text do carimbo já diz “INICIO da rodada”, também na migração declarada apenas de help text (`app/folha/models.py:358`, `app/folha/migrations/0013_retrato_lavrado_em_help_text_e3r3.py:1`, `app/folha/migrations/0013_retrato_lavrado_em_help_text_e3r3.py:16`).

## 15/11 19:39 · sessao 94c8f53bb9d0 · - · - · origem=Ronald (tmux)

A comparação exata `f31a8d09e × base da obra` fica **[nao verificado]**: não há metadados Git nesta cópia; os artefatos identificam bases parciais distintas — LU3 `522b7eb48` (`app/chamados/tests/e3_lu3_censo.txt:3`; `app/chamados/tests/e3_lu3_entrega.txt:2`), LU2 `de702cb2f` (`app/chamados/tests/e3_lu2_censo.txt:1`) e c12 `20f5a0338` (`app/chamados/tests/e3_r3_c12_entrega.txt:1`).

Outros leitores que montam universos, confirmados no código:

| Leitor | Universo próprio e divergência |
|---|---|
| `_recorte_da_barra`, compartilhado por tela/PDF/raiox | Dropdown visível, mas seleção por PK não cruza visibilidade; `todas` usa a competência atual da primeira empresa; URL escolhe outro par; navegação calcula ±1 mês (`app/folha/views_retrato.py:147`, `:153`, `:157`, `:161`, `:164`, `:179`, `:181`, `:189`, `:192`). As três views chamam o recorte (`app/folha/views_retrato.py:258`, `:386`, `:487`). |
| Aviso de frescor | Refiltra o frescor pelas empresas com foto existente, omitindo justamente as sem retrato (`app/folha/views_retrato.py:209`, `:211`). |
| Painel do colaborador | Empresa do colaborador cruzada com visíveis, competência exata da URL; não percorre as duas janelas (`app/colaboradores/views.py:975`, `:977`, `:982`). |
| `vigia_fio.censo_alcance` | Visíveis com interseção, mas usa `competencias_a_lavrar`, cuja anterior só entra se não entregue (`app/chamados/services/vigia_fio.py:517`, `:520`, `:535`; `app/ponto/services/he_pendente_lavrado.py:220`, `:223`). |
| `vigia_fio.censo` antigo | Chamados vivos com célula, sem cruzar visibilidade quando `empresas=None`; não escolhe duas competências (`app/chamados/services/vigia_fio.py:290`, `:292`). |
| `ListaDoAdmin` | Cruza novamente empresas visíveis e o recorte fornecido (`app/chamados/services/vigia_fio.py:191`, `:193`, `:194`). |
| C5 `colabs_nunca_bateu` | `None` significa todas as empresas; seleciona células entre início da anterior e ontem. Em 4.2 deve receber os IDs **efetivos visíveis**, para não conservar o universo largo subsidiário (`app/escala/services/cadastro_realidade.py:162`, `:163`, `:167`; chamada atual `app/folha/vigia_retido_calado.py:60`). |
| `contexto_drawer` | Recompõe atual e anterior por `_ja(atual_ini-1dia)`, com piso visual próprio (`app/colaboradores/services/drawer.py:111`, `:115`, `:116`, `:118`, `:123`). |
| `contexto_calendario` | Default mês/ano civil; modo competência consulta `janela_fechamento`; demais modos constroem períodos próprios (`app/colaboradores/services/calendario.py:194`, `:205`, `:209`, `:210`, `:227`, `:239`). |
| Consulta plural do calendário | Calcula janela atual da empresa para excluir dias correntes da oferta (`app/colaboradores/services/calendario.py:140`, `:143`, `:150`). |
| `CargaDaTarefa._ler_exportadas` | Deriva competência de cada dia recebido, sem usar as duas competências de hoje (`app/chamados/services/tarefa_do_dia.py:467`, `:471`, `:479`, `:482`). |
| `escalas_em_limbo` | Universo de escalas ativas, recorte opcional de empresa e foto `FolgaDia` do mês civil (`app/relatorios/furos.py:60`, `:63`, `:86`, `:88`). |

Os módulos `vigia_fio`, calendário, drawer e furos constam do diff **parcial LU3** documentado (`app/chamados/tests/e3_lu3_entrega.txt:151`, `:163`, `:164`, `:168`); pertença de cada função ao diff integral solicitado: **[nao verificado]**.

Escritores e significado das fontes usadas por 4.2:

- `Empresa.ativa`, `cnpj` e `dia_inicio_competencia` são campos definidos em `app/colaboradores/models.py:43`, `:44`, `:46`. A tela recebe os valores e chama `atualizar_empresa`/`criar_empresa` (`app/colaboradores/views.py:492`, `:493`, `:496`, `:521`, `:532`); as portas gravam por `create`/`save` (`app/colaboradores/services/estrutura.py:13`, `:15`, `:18`, `:21`). Também existe escritor explícito de normalização do CNPJ (`app/colaboradores/services/estrutura.py:25`, `:28`), e `EmpresaAdmin` deixa apenas `regime_trabalhista` somente leitura (`app/colaboradores/admin.py:8`, `:19`).
- `empresas_visiveis` lê `ativa=True` e exclui CNPJ `11222333*` (`app/colaboradores/queries.py:85`, `:86`); o contrato documentado do helper diz que integrações e lógica interna não o usam deliberadamente (`app/colaboradores/queries.py:79`, `:82`). Logo, a equivalência “empresa = visível” **não é universal** (`app/escala/services/cadastro_realidade.py:163`; `app/chamados/services/vigia_fio.py:290`).
- `competencias_do_retrato` deriva mês/ano do fim de anterior/atual, com deduplicação (`app/folha/retrato_folha.py:1377`, `:1380`, `:1382`). As janelas leem o corte empresarial; competência corresponde ao mês do **fim**, não ao mês civil de hoje (`app/ponto/janelas.py:10`, `:16`, `:36`, `:50`, `:60`, `:66`).
- `RetratoFolha.empresa/mes/ano/linhas/lavrado_em` são gravados pelo lavrador via `update_or_create`; `lavrado_em` recebe o instante inicial da rodada (`app/folha/management/commands/lavrar_retrato_folha.py:49`, `:77`, `:79`). A chave única é empresa/mês/ano (`app/folha/models.py:366`). Frescor lê o par e o carimbo (`app/folha/retrato_folha.py:1412`, `:1413`); vigia lê o par e as linhas, sem selecionar por `lavrado_em` (`app/folha/vigia_retido_calado.py:51`, `:55`, `:65`).
- O lavrador explícito pode gravar competência fora das duas janelas (`app/folha/management/commands/lavrar_retrato_folha.py:57`, `:58`); a tela e o painel podem lê-la por URL (`app/folha/views_retrato.py:181`, `:192`; `app/colaboradores/views.py:975`, `:982`). Portanto, “competência lida = competência das duas janelas automáticas” também **não é universal**.

## 15/11 18:53 · sessao 3b3ebdd4be3c · E3-RESTO-R3 · censo · origem=Code

Os caminhos abaixo são relativos a `app/`. Distingo **leitura própria**, **fallback suprimido pelo argumento recebido** e **omissão de argumento**. Execução efetiva em produção: **[não verificado]**.

O caminho solicitado `folha/vigia_fio.py` não foi localizado **[não verificado]**. Incluí separadamente o módulo encontrado, `chamados/services/vigia_fio.py`, cujas entradas são `censo` e `censo_alcance`; equivalência com o caminho solicitado: **[não verificado]** (`chamados/services/vigia_fio.py:266`, `chamados/services/vigia_fio.py:487`).

**1. Relógio transitivo — até seis chamadas**

**Instante disponível nas bordas**

| Borda | O que tem em mãos |
|---|---|
| `lavrar_retrato_folha.Command.handle` | `_agora` e seu dia local (`folha/management/commands/lavrar_retrato_folha.py:49`, `folha/management/commands/lavrar_retrato_folha.py:50`). |
| `vigiar_retido_calado.Command.handle` | Lê somente o dia, diretamente no argumento de `censo`; não conserva um `datetime` (`folha/management/commands/vigiar_retido_calado.py:33`). |
| `_recorte_da_barra` | `_agora` e `_hoje`; devolve o dia às views, mas não devolve `_agora` (`folha/views_retrato.py:177`, `folha/views_retrato.py:178`, `folha/views_retrato.py:238`, `folha/views_retrato.py:245`). |
| `calendario_ponto` | `_agora`, passado ao calendário (`colaboradores/views.py:1951`, `colaboradores/views.py:1952`). |
| `contexto_drawer` | `agora` e dia local (`colaboradores/services/drawer.py:21`, `colaboradores/services/drawer.py:22`). |
| `furos_espelho` | `_agora`, passado ao dataset (`relatorios/views.py:1519`, `relatorios/views.py:1543`). |
| `frescor_dos_retratos` | `agora` obrigatório (`folha/retrato_folha.py:1386`). |
| `vigia_fio.censo` / `censo_alcance` | O primeiro recebe ou lê `agora`; o segundo o exige (`chamados/services/vigia_fio.py:266`, `chamados/services/vigia_fio.py:282`, `chamados/services/vigia_fio.py:487`). |

**Lavrador**

- **Salto 0:** `handle → timezone.now()` — leitura da borda (`folha/management/commands/lavrar_retrato_folha.py:49`).
- **Salto 3:** `handle → contadores → linhas_do_retrato → recortar`. `recortar` aceita `hoje=None`, e `linhas_do_retrato` o omite. Aqui só há eixo `f` ou nenhum grupo; não se consulta o leitor de dono (`folha/management/commands/lavrar_retrato_folha.py:68`, `folha/retrato_folha.py:120`, `folha/retrato_folha.py:121`, `folha/retrato_folha.py:111`, `folha/retrato_folha.py:635`, `folha/retrato_folha.py:632`).
- **Salto 4:** `handle → montar_linhas → _informativo_rescisao → informativo_rescisao → _dias_sem_decisao`. Contém `_tz.localdate()`, mas **o fallback está suprimido**: o dia da borda atravessa todas essas chamadas (`folha/management/commands/lavrar_retrato_folha.py:65`, `folha/retrato_folha.py:1220`, `folha/retrato_folha.py:1288`, `folha/export.py:1228`, `folha/export.py:1238`).
- **Salto 5:** `handle → montar_linhas → fora_da_folha → dias_que_seguram → classificar_export → motivos_retencao_espelho`. A última chamada **omite `hoje`**, cujo default é `None`. `motivos_retencao_celula` fica no salto 6; seu `_dias_sem_decisao`, com relógio, fica no **salto 7**, fora deste teto (`folha/management/commands/lavrar_retrato_folha.py:65`, `folha/retrato_folha.py:1214`, `folha/fora_da_folha.py:200`, `folha/fora_da_folha.py:147`, `folha/export.py:968`, `folha/export.py:1301`, `folha/export.py:1307`, `folha/export.py:1212`).
- **Salto 6:** `handle → montar_linhas → fora_da_folha → _carga → carga_da_tarefa → CargaDaTarefa.__init__ → qs_na_fila`. Contém `timezone.now()`, **suprimido pelo `agora` propagado**. O construtor é chamado pela classe devolvida pela fábrica local (`folha/management/commands/lavrar_retrato_folha.py:65`, `folha/retrato_folha.py:1214`, `folha/fora_da_folha.py:205`, `folha/fora_da_folha.py:67`, `chamados/services/tarefa_do_dia.py:730`, `chamados/services/tarefa_do_dia.py:452`, `chamados/services/painel.py:190`).

**Vigia do retido calado**

- **Salto 0:** `handle → timezone.localdate()` — leitura da borda (`folha/management/commands/vigiar_retido_calado.py:33`).
- **Salto 2:** `handle → censo → colabs_nunca_bateu`. Contém `timezone.localdate()`, **suprimido pelo `hoje` posicional recebido** (`folha/management/commands/vigiar_retido_calado.py:33`, `folha/vigia_retido_calado.py:60`, `escala/services/cadastro_realidade.py:161`).
- **Salto 3:** `handle → censo → linhas_do_retrato → recortar`. **Omite `hoje=None`**, no mesmo caminho restrito ao eixo `f` descrito acima (`folha/management/commands/vigiar_retido_calado.py:33`, `folha/vigia_retido_calado.py:65`, `folha/retrato_folha.py:111`, `folha/retrato_folha.py:635`).

**Recorte e suas três views**

- `_recorte_da_barra → timezone.now()` é a leitura direta. `retrato_folha`, `retrato_pdf` e `retrato_raiox` chegam a ela por suas chamadas ao recorte (`folha/views_retrato.py:177`, `folha/views_retrato.py:258`, `folha/views_retrato.py:386`, `folha/views_retrato.py:487`).
- `_recorte_da_barra → linhas_do_retrato → recortar` **omite `hoje=None`**, no salto 2 do recorte e no salto 3 de cada view. Novamente, não consulta o eixo de dono nesse caminho (`folha/views_retrato.py:198`, `folha/retrato_folha.py:111`, `folha/retrato_folha.py:632`, `folha/retrato_folha.py:635`).
- `retrato_pdf → gerar_pdf_auditavel → timezone.now()` acrescenta uma **leitura própria**, no salto 1. A view já recebeu o dia do recorte; o instante completo não veio no retorno (`folha/views_retrato.py:386`, `folha/views_retrato.py:441`, `folha/views_retrato.py:245`, `relatorios/pdf_base.py:133`).
- `retrato_raiox → placar_aceite` acrescenta **duas leituras próprias**, `_tz.now()` e `_tz.localdate()`, no salto 1, quando `dono == ['colab']`. O import de `placar_aceite` é local (`folha/views_retrato.py:511`, `folha/views_retrato.py:512`, `folha/views_retrato.py:513`, `chamados/services/verbos.py:286`, `chamados/services/verbos.py:287`).
- `frescor_dos_retratos` não acrescenta leitura autônoma nesse fecho: converte `agora`, passa data explícita às janelas e formata carimbos explícitos (`folha/retrato_folha.py:1408`, `folha/retrato_folha.py:1411`, `folha/retrato_folha.py:1441`, `folha/retrato_folha.py:1444`, `ponto/janelas.py:60`, `ponto/janelas.py:66`).

**Família compartilhada por calendário, drawer e relatório**

Use estes prefixos para as cadeias da tabela:

- **K:** `calendario_ponto → contexto_calendario`, passando `agora` (`colaboradores/views.py:1952`).
- **D:** `contexto_drawer → contexto_calendario`, passando `agora` (`colaboradores/services/drawer.py:123`, `colaboradores/services/drawer.py:124`).
- **F:** `furos_espelho → dataset_furos → contexto_calendario`, passando `agora` nas duas arestas (`relatorios/views.py:1543`, `relatorios/furos.py:160`, `relatorios/furos.py:164`).

| Cadeia depois do prefixo | Salto K/D; F | Leitura ou omissão |
|---|---:|---|
| `contexto_calendario` | 1; 2 | `timezone.now()` **suprimido** pelo instante recebido (`colaboradores/services/calendario.py:192`). |
| `contexto_calendario → status_dos_dias_lavrados` | 2; 3 | `now()` **suprimido**; recebe `_agora` (`colaboradores/services/calendario.py:638`, `ponto/status_dia.py:252`, `ponto/status_dia.py:253`). |
| `contexto_calendario → pergunta_viva` | 2; 3 | Import local com apelido `_pv`; **omite `hoje=None`**, mas fornece `agora`, do qual o juiz deriva o dia (`colaboradores/services/calendario.py:498`, `colaboradores/services/calendario.py:506`, `chamados/juizes.py:865`, `chamados/juizes.py:888`). |
| `… → pergunta_viva → _pergunta_viva_calc → _dentro_da_janela_viva` | 4; 5 | `date.today()` **suprimido** pelo dia derivado do instante (`chamados/juizes.py:897`, `chamados/juizes.py:1468`, `chamados/juizes.py:595`). |
| `… → pergunta_viva → _pergunta_viva_calc → classificar_falta` | 4; 5 | Import local `_cf`; `_tz.now()` **suprimido** pelo `agora` recebido (`chamados/juizes.py:1470`, `chamados/juizes.py:1472`, `chamados/juizes.py:1474`, `escala/servico_jornada.py:387`). |
| `contexto_calendario → folha_manda → turno_aberto_de` | 3; 4 | `now()` **suprimido** pelo instante recebido (`colaboradores/services/calendario.py:614`, `relatorios/cartao_pela_celula.py:398`, `ponto/turnos.py:2291`). |
| `contexto_calendario → folha_manda → ate_ontem` | 3; 4 | `localdate()` **suprimido** pelo dia fornecido (`colaboradores/services/calendario.py:614`, `relatorios/cartao_pela_celula.py:413`, `relatorios/cartao_pela_celula.py:40`). |
| `contexto_calendario → folha_manda → totais_da_folha → fechamento_lido → recalcular_fechamento_mes` | 5; 6 | **Leituras próprias** em `timezone.now()` nas linhas 616 e 1086; o instante da borda não é transportado até o recálculo (`colaboradores/services/calendario.py:614`, `relatorios/cartao_pela_celula.py:323`, `relatorios/cartao_pela_celula.py:73`, `ponto/services/fechamento.py:1759`, `ponto/services/fechamento.py:616`, `ponto/services/fechamento.py:1086`). |
| `… → recalcular_fechamento_mes → motivos_retencao_espelho` | 6; 7, fora | Import local `_mre`; **omite `hoje=None`** (`ponto/services/fechamento.py:1066`, `ponto/services/fechamento.py:1067`, `folha/export.py:1301`). |
| `contexto_calendario → CargaDaJanela.turnos → _turnos_do_colab_puro → papel_por_minuto_da_ata → periodos_do_dia → intervalo_cadastrado` | 6; 7, fora | **Leitura própria de `date.today()`**, quando existe tipo de escala; inclui despacho pela instância `_carga` (`colaboradores/services/calendario.py:252`, `ponto/turnos.py:3036`, `ponto/turnos.py:1939`, `ponto/turnos.py:1633`, `ponto/juiz_batida.py:270`, `ponto/juiz_batida.py:117`, `ponto/juiz_batida.py:125`). |

Duas condições delimitam essa tabela:

- `folha_manda → totais_da_folha` exige competência inteira e fechamento existente. `recalcular_fechamento_mes` também contém relógios nas linhas 1143, 1161 e 1189, mas esses ramos de escrita ficam **suprimidos** por `somente_leitura=True` nesse caminho (`relatorios/cartao_pela_celula.py:321`, `relatorios/cartao_pela_celula.py:71`, `ponto/services/fechamento.py:1761`, `ponto/services/fechamento.py:1128`, `ponto/services/fechamento.py:1143`, `ponto/services/fechamento.py:1161`, `ponto/services/fechamento.py:1189`).
- Pelo prefixo **F**, os caminhos através de `folha_manda` são apenas alcançabilidade **sintática**: `NAO_PERGUNTADO` impede a execução desse bloco (`relatorios/furos.py:164`, `colaboradores/services/calendario.py:594`, `colaboradores/services/calendario.py:597`, `colaboradores/services/calendario.py:614`).

**Ramos adicionais do drawer e do relatório**

- `contexto_drawer → perguntas_vivas_de → pergunta_viva`: import local `_vivas_de` de `chamados.models`, reexportado de `chamados.juizes`. **Omite `hoje` e `agora`**, apesar de o drawer já possuir ambos (`colaboradores/services/drawer.py:21`, `colaboradores/services/drawer.py:79`, `colaboradores/services/drawer.py:80`, `chamados/models.py:4226`, `chamados/juizes.py:1504`, `chamados/juizes.py:865`).
- Esse ramo chega a `_dentro_da_janela_viva` e `classificar_falta` no **salto 4**, permitindo suas leituras próprias; pela perna `_pergunta_materializada_de_fato`, chega novamente aos dois no **salto 5** (`chamados/juizes.py:897`, `chamados/juizes.py:1468`, `chamados/juizes.py:1472`, `chamados/juizes.py:1486`, `chamados/juizes.py:696`, `chamados/juizes.py:701`, `chamados/juizes.py:595`, `escala/servico_jornada.py:387`).
- Na mesma cadeia, `_pergunta_materializada_de_fato` **omite efetivamente `agora`** no salto 4, e `_slot_esperado` o omite no salto 5: os `**` condicionais viram `{}` quando o instante é `None`. As assinaturas têm default `None` (`chamados/juizes.py:1486`, `chamados/juizes.py:1487`, `chamados/juizes.py:665`, `chamados/juizes.py:755`, `chamados/juizes.py:344`).
- `contexto_drawer → turnos_do_colab → _turnos_do_colab_puro → papel_por_minuto_da_ata → periodos_do_dia → intervalo_cadastrado`: **`date.today()` próprio no salto 5**. O import local é `_tdc` (`colaboradores/services/drawer.py:44`, `colaboradores/services/drawer.py:47`, `ponto/turnos.py:1915`, `ponto/turnos.py:1939`, `ponto/turnos.py:1633`, `ponto/juiz_batida.py:270`, `ponto/juiz_batida.py:125`).
- `furos_espelho → escalas_em_limbo`: **`timezone.localdate()` próprio no salto 1**, embora `_agora` já exista na borda (`relatorios/views.py:1519`, `relatorios/views.py:1554`, `relatorios/furos.py:59`).
- `furos_espelho → dataset_furos → status_dos_dias_lavrados`: contém `now()` no **salto 2**, **suprimido** pelo instante recebido. O default `agora=None` de `dataset_furos` permanece, mas essa borda fornece o argumento (`relatorios/views.py:1543`, `relatorios/furos.py:103`, `relatorios/furos.py:123`, `ponto/status_dia.py:253`).

**Complemento: módulo real `chamados/services/vigia_fio.py`**

Para tornar as cadeias seguintes explícitas:

- **T(C/A):** `censo:359` ou `censo_alcance:593 → CargaDaTarefa.tarefa → resolver_dia_oferecido_nas_datas → contexto_calendario` (`chamados/services/vigia_fio.py:359`, `chamados/services/vigia_fio.py:593`, `chamados/services/tarefa_do_dia.py:613`, `colaboradores/services/calendario.py:157`).
- **L(C):** `censo → dono_fora_do_fio → dia_do_admin → _carregar → cadastro_realidade.lista` (`chamados/services/vigia_fio.py:387`, `chamados/services/vigia_fio.py:261`, `chamados/services/vigia_fio.py:202`, `chamados/services/vigia_fio.py:194`).
- **L(A):** `censo_alcance → dia_na_lista → _carregar → cadastro_realidade.lista` (`chamados/services/vigia_fio.py:621`, `chamados/services/vigia_fio.py:210`, `chamados/services/vigia_fio.py:194`).
- **P(C/A):** `censo:387` ou `censo_alcance:615 → dono_fora_do_fio → PerguntaDisputa.estado → pergunta_viva`. A propriedade **omite ambos os argumentos**; acesso à propriedade conta como chamada (`chamados/services/vigia_fio.py:387`, `chamados/services/vigia_fio.py:615`, `chamados/services/vigia_fio.py:259`, `chamados/models.py:3127`, `chamados/juizes.py:865`).

| Cadeia | Saltos C / A | Leitura e transporte do instante |
|---|---:|---|
| `censo → now()` | 0 / — | Fallback da própria borda, quando não recebeu `agora` (`chamados/services/vigia_fio.py:282`). |
| `censo/censo_alcance → carga_da_tarefa → CargaDaTarefa.__init__ → qs_na_fila` | 3 / 3 | `now()` **suprimido** pelo instante propagado (`chamados/services/vigia_fio.py:357`, `chamados/services/vigia_fio.py:592`, `chamados/services/tarefa_do_dia.py:730`, `chamados/services/tarefa_do_dia.py:452`, `chamados/services/painel.py:190`). |
| `T(C/A)` | 3 / 3 | `contexto_calendario:192`, fallback suprimido; o instante atravessa a função plural (`chamados/services/tarefa_do_dia.py:614`, `colaboradores/services/calendario.py:159`, `colaboradores/services/calendario.py:192`). |
| `T(C/A) → status_dos_dias_lavrados` | 4 / 4 | `now()` suprimido (`colaboradores/services/calendario.py:638`, `ponto/status_dia.py:253`). |
| `T(C/A) → folha_manda → turno_aberto_de / ate_ontem` | 5 / 5 | `now()` e `localdate()` suprimidos (`colaboradores/services/calendario.py:614`, `relatorios/cartao_pela_celula.py:398`, `relatorios/cartao_pela_celula.py:413`, `ponto/turnos.py:2291`, `relatorios/cartao_pela_celula.py:40`). |
| `… → tarefa → ler → alimentar_fila → particionar_verbos / qs_na_fila` | 4 / 4 | Função local `ler`; ambos os relógios suprimidos pelo instante recebido (`chamados/services/tarefa_do_dia.py:600`, `chamados/services/tarefa_do_dia.py:588`, `chamados/services/tarefa_do_dia.py:590`, `chamados/juizes.py:1573`, `chamados/juizes.py:1576`, `chamados/juizes.py:1584`, `chamados/services/verbos.py:35`, `chamados/services/painel.py:190`). |
| `L(C/A)` | 4 / 3 | `lista → now()`, suprimido pelo `self.agora` fornecido pelos censos (`chamados/services/vigia_fio.py:194`, `escala/services/cadastro_realidade.py:780`). |
| `L(C/A) → _jornada_que_mente` | 5 / 4 | **`date.today()` próprio**; `lista` passa somente empresas (`escala/services/cadastro_realidade.py:791`, `escala/services/cadastro_realidade.py:224`). |
| `L(C/A) → _afastado_sem_registro` | 5 / 4 | `localdate()` suprimido pelo `hoje` passado (`escala/services/cadastro_realidade.py:808`, `escala/services/cadastro_realidade.py:588`). |
| `L(C/A) → _afastado_sem_registro → afastados_hoje` | 6 / 5 | **`localdate()` próprio**: a chamada não fornece o dia (`escala/services/cadastro_realidade.py:592`, `colaboradores/services/situacao_exibida.py:45`). |
| `L(C/A) → _nunca_bateu → colabs_nunca_bateu` | 6 / 5 | `localdate()` suprimido pelo `hoje` propagado (`escala/services/cadastro_realidade.py:864`, `escala/services/cadastro_realidade.py:179`, `escala/services/cadastro_realidade.py:161`). |
| `L(A) → _jornada_que_mente → periodos_do_dia → intervalo_cadastrado` | 7, fora / 6 | **`date.today()` próprio**, quando existe tipo de escala (`escala/services/cadastro_realidade.py:238`, `ponto/juiz_batida.py:270`, `ponto/juiz_batida.py:125`). |
| `P(C/A) → _pergunta_viva_calc → _dentro_da_janela_viva / classificar_falta` | 5 / 5 | **Leituras próprias**: a propriedade perdeu o instante que os censos possuem (`chamados/models.py:3127`, `chamados/juizes.py:897`, `chamados/juizes.py:1468`, `chamados/juizes.py:1472`, `chamados/juizes.py:595`, `escala/servico_jornada.py:387`). |
| `P(C/A) → _pergunta_viva_calc → _pergunta_materializada_de_fato →` esses dois leitores | 6 / 6 | Mesmas leituras próprias; `agora` é omitido pela expansão condicional (`chamados/juizes.py:1486`, `chamados/juizes.py:1487`, `chamados/juizes.py:696`, `chamados/juizes.py:701`). |

Ainda no fecho desses censos:

- `censo → vigia_carimbo.classificar → _pergunta_materializada_de_fato`: **omite `hoje` e `agora` no salto 2**. O import local `_juiz` passa por reexportação de `chamados.models`. Seus leitores `_dentro_da_janela_viva` e `classificar_falta` ficam no salto 3 e podem ler relógio próprio (`chamados/services/vigia_fio.py:415`, `chamados/services/vigia_carimbo.py:95`, `chamados/services/vigia_carimbo.py:106`, `chamados/models.py:4226`, `chamados/juizes.py:665`, `chamados/juizes.py:696`, `chamados/juizes.py:701`).
- `censo → classificar → _slot_esperado`: **omite `agora` no salto 2**; também existe a omissão condicional pela perna `_pergunta_materializada_de_fato → _slot_esperado` (`chamados/services/vigia_fio.py:415`, `chamados/services/vigia_carimbo.py:110`, `chamados/juizes.py:344`, `chamados/juizes.py:755`).
- `censo_alcance → dias_que_seguram → classificar_export → motivos_retencao_espelho`: **omite `hoje` no salto 3**. Seguindo `motivos_retencao_celula → _dias_sem_decisao`, alcança **`localdate()` próprio no salto 5**. Pela outra perna, `_dias_que_retem`, o mesmo leitor fica no salto 4, mas com fallback suprimido (`chamados/services/vigia_fio.py:537`, `folha/fora_da_folha.py:147`, `folha/export.py:968`, `folha/export.py:1307`, `folha/export.py:1212`, `folha/export.py:1238`, `folha/fora_da_folha.py:165`, `folha/fora_da_folha.py:121`).
- `censo_alcance → alimentar_portas → alimentar_fila → particionar_verbos / qs_na_fila`: relógios no **salto 3**, suprimidos pelo instante propagado (`chamados/services/vigia_fio.py:560`, `chamados/juizes.py:1619`, `chamados/juizes.py:1573`, `chamados/juizes.py:1576`, `chamados/services/verbos.py:35`, `chamados/services/painel.py:190`).
- `censo_alcance → alimentar_portas → pergunta_viva`: **omite `hoje` no salto 2**, mas fornece `agora`; seus leitores ficam no salto 4 e têm fallbacks suprimidos (`chamados/services/vigia_fio.py:560`, `chamados/juizes.py:1628`, `chamados/juizes.py:888`, `chamados/juizes.py:1468`, `chamados/juizes.py:1472`).
- `T(C/A) → pergunta_viva` **omite `hoje` no salto 4**, fornecendo o instante. `… → tarefa → _ler_vida → pergunta_viva` fornece ambos. As expansões condicionais posteriores fornecem `agora` nesses caminhos (`colaboradores/services/calendario.py:506`, `chamados/services/tarefa_do_dia.py:574`, `chamados/services/tarefa_do_dia.py:537`, `chamados/services/tarefa_do_dia.py:541`, `chamados/juizes.py:1487`, `chamados/juizes.py:755`).
- `censo/censo_alcance → tarefa → _ler_vida → carga_da_vida → CargaDaVida.__init__ → _slot_esperado`: **omite `agora` no salto 5**, mas passa `carga=self`; o juiz recupera o instante dessa carga (`chamados/services/vigia_fio.py:359`, `chamados/servic

[truncado em 20000 de 35668 caracteres]

## 15/11 18:49 · sessao c68596c909d7 · - · - · origem=Ronald (tmux)

Confirmado:

- `_carregar → lista → _folga_nao_definida_sem_porta → beco_em_lote → alimentar_portas → alimentar_fila → particionar_verbos/qs_na_fila`: relógios no salto **6**, guardados pelo instante propagado (`app/chamados/services/vigia_fio.py:194`; `app/escala/services/cadastro_realidade.py:850`, `:559`, `:460`; `app/chamados/juizes.py:1619`, `:1573`, `:1576`, `:1584`; `app/chamados/services/verbos.py:35`; `app/chamados/services/painel.py:190`).
- No mesmo prefixo, `alimentar_portas → pergunta_viva` omite `hoje` no salto **5**; recupera-o de `agora` (`app/chamados/juizes.py:1628`, `:888`, `:890`).
- `dia_do_admin`, `dia_na_lista` e `colab_na_lista` acrescentam **1**: omissão no salto **6**, relógios no **7**, fora (`app/chamados/services/vigia_fio.py:202`, `:210`, `:228`).
- `papel_do_dia → dia_na_lista` acrescenta **2**: omissão no **7**, fora (`app/chamados/services/vigia_fio.py:220`, `:210`).
- `dono_fora_do_fio → dia_do_admin → _carregar → lista → _jornada_que_mente → periodos_do_dia → intervalo_cadastrado`: `date.today()` no salto **6**, dentro (`app/chamados/services/vigia_fio.py:261`, `:202`, `:194`; `app/escala/services/cadastro_realidade.py:791`, `:238`; `app/ponto/juiz_batida.py:270`, `:125`).

## 15/11 18:47 · sessao c68596c909d7 · - · - · origem=Ronald (tmux)

Fecho consolidado do módulo real `app/chamados/services/vigia_fio.py`. Abaixo, profundidade é número de chamadas a partir da função indicada; acesso à propriedade `estado` conta como chamada (`app/chamados/services/vigia_fio.py:259`, `app/chamados/models.py:3117`).

**Cadeias comuns usadas nas listas**

- **T(C)**: `censo:359 → CargaDaTarefa.tarefa:613 → resolver_dia_oferecido_nas_datas:157 → contexto_calendario`. **T(A)** substitui a primeira chamada por `censo_alcance:593`. Arquivos: `app/chamados/services/vigia_fio.py:359`, `:593`; `app/chamados/services/tarefa_do_dia.py:613`; `app/colaboradores/services/calendario.py:157`.
- **L(C)**: `censo:387 → dono_fora_do_fio:261 → ListaDoAdmin.dia_do_admin:202 → ListaDoAdmin._carregar:194 → cadastro_realidade.lista`. Arquivos: `app/chamados/services/vigia_fio.py:387`, `:261`, `:202`, `:194`.
- **L(A)**: `censo_alcance:621 → ListaDoAdmin.dia_na_lista:210 → ListaDoAdmin._carregar:194 → cadastro_realidade.lista`. Arquivo: `app/chamados/services/vigia_fio.py:621`, `:210`, `:194`.
- **P(C)**: `censo:387 → dono_fora_do_fio:259 → PerguntaDisputa.estado:3127 → pergunta_viva`. **P(A)** substitui a primeira chamada por `censo_alcance:615`. Arquivos: `app/chamados/services/vigia_fio.py:387`, `:615`, `:259`; `app/chamados/models.py:3127`.
- **V**: `pergunta_viva → _pergunta_viva_calc`, pela chamada sem carga em `app/chamados/juizes.py:897`; os caminhos alimentados usam a chamada com carga em `:892`.

**`censo` — todos os leitores físicos alcançados até seis saltos**

| Profundidade | Cadeia e leitor | Instante |
|---:|---|---|
| 0 | `censo → timezone.now()` (`app/chamados/services/vigia_fio.py:282`) | Lê somente quando `agora is None`; a função admite instante externo (`:266`, `:282`). |
| 3 | `censo:357 → carga_da_tarefa:730 → CargaDaTarefa.__init__:452 → qs_na_fila`, leitor `timezone.now()` (`app/chamados/services/painel.py:190`) | `agora` propagado; fallback suprimido (`app/chamados/services/tarefa_do_dia.py:452`). |
| 3 | T(C), leitor `contexto_calendario → timezone.now()` (`app/colaboradores/services/calendario.py:192`) | `agora` propagado (`:159`); fallback suprimido. |
| 3 | `censo:415 → vigia_carimbo.classificar:106 → _pergunta_materializada_de_fato:696 → _dentro_da_janela_viva`, leitor `date.today()` (`app/chamados/juizes.py:595`) | Instante não propagado pelo chamador `_juiz(p)` (`app/chamados/services/vigia_carimbo.py:106`). |
| 3 | Mesmo prefixo por `classificar:106 → _pergunta_materializada_de_fato:701 → classificar_falta`, leitor `_tz.now()` (`app/escala/servico_jornada.py:387`) | `agora=None` chega do `_juiz(p)`; leitura condicionada a existir escala e tipo (`:385`). |
| 4 | `censo:359 → tarefa:600 → função local ler:588 → alimentar_fila:1573 → particionar_verbos`, leitor `now()` (`app/chamados/services/verbos.py:35`) | Instante propagado (`app/chamados/services/tarefa_do_dia.py:590`; `app/chamados/juizes.py:1573`). |
| 4 | T(C) `→ contexto_calendario:638 → status_dos_dias_lavrados`, leitor `now()` (`app/ponto/status_dia.py:253`) | Instante propagado (`app/colaboradores/services/calendario.py:638`); fallback suprimido. |
| 4 | L(C), leitor `lista → now()` (`app/escala/services/cadastro_realidade.py:780`) | Instante propagado (`app/chamados/services/vigia_fio.py:194`); fallback suprimido. |
| 5 | T(C) `→ contexto_calendario:614 → folha_manda:398 → turno_aberto_de`, leitor `now()` (`app/ponto/turnos.py:2291`) | Instante propagado (`app/relatorios/cartao_pela_celula.py:398`); fallback suprimido. |
| 5 | T(C) `→ contexto_calendario:614 → folha_manda:413 → ate_ontem`, leitor `localdate()` (`app/relatorios/cartao_pela_celula.py:40`) | `hoje` derivado do instante e passado (`:413`); fallback suprimido. |
| 5 | L(C) `→ lista:791 → _jornada_que_mente`, leitor **incondicional** `date.today()` (`app/escala/services/cadastro_realidade.py:224`) | A borda já dispõe de instante e dia; a chamada passa somente empresas (`:791`). |
| 5 | L(C) `→ lista:808 → _afastado_sem_registro`, leitor `localdate()` (`app/escala/services/cadastro_realidade.py:588`) | `hoje` passado em `:808`; fallback suprimido. |
| 6 | L(C) `→ lista:808 → _afastado_sem_registro:592 → afastados_hoje`, leitor `localdate()` (`app/colaboradores/services/situacao_exibida.py:45`) | `dia` omitido em `app/escala/services/cadastro_realidade.py:592`; leitura própria. |
| 6 | L(C) `→ lista:864 → _nunca_bateu:179 → colabs_nunca_bateu`, leitor `localdate()` (`app/escala/services/cadastro_realidade.py:161`) | `hoje` passado nas duas chamadas (`:864`, `:179`); fallback suprimido. |

Há ainda caminhos relevantes para leitores já enumerados: P(C) `→ V → _dentro_da_janela_viva:595` ou `classificar_falta:387`, profundidade 5; P(C) `→ V → _pergunta_materializada_de_fato →` esses mesmos leitores, profundidade 6. Nesses caminhos `estado` omite o instante (`app/chamados/models.py:3127`; `app/chamados/juizes.py:1468`, `:1472`, `:1486`, `:696`, `:701`).

O `intervalo_cadastrado:125` fica no salto **7** por L(C), portanto fora: `lista:791 → _jornada_que_mente:238 → periodos_do_dia:270 → intervalo_cadastrado` (`app/escala/services/cadastro_realidade.py:791`, `:238`; `app/ponto/juiz_batida.py:270`, `:125`).

**`censo_alcance` — todos os leitores físicos alcançados até seis saltos**

- Não lê relógio diretamente: exige `agora` e deriva o dia por `_dia_de(agora)` (`app/chamados/services/vigia_fio.py:487`, `:516`).
- Herda os cinco leitores da cadeia T(A), nas mesmas profundidades de T(C): `contexto_calendario` 3, `status_dos_dias_lavrados` 4, `turno_aberto_de` 5, `ate_ontem` 5; todos recebem instante ou dia explícitos (`app/chamados/services/vigia_fio.py:593`; `app/chamados/services/tarefa_do_dia.py:613`; `app/colaboradores/services/calendario.py:157`, `:159`, `:614`, `:638`; `app/relatorios/cartao_pela_celula.py:398`, `:413`).
- `censo_alcance:560 → alimentar_portas:1619 → alimentar_fila:1573/1576/1584 → particionar_verbos/qs_na_fila`, profundidade 3; leitores `app/chamados/services/verbos.py:35` e `app/chamados/services/painel.py:190`. Recebem `agora`; fallbacks suprimidos (`app/chamados/juizes.py:1619`, `:1573`, `:1576`, `:1584`).
- A construção da carga também alcança `qs_na_fila` no salto 3: `censo_alcance:592 → carga_da_tarefa:730 → CargaDaTarefa.__init__:452 → qs_na_fila:190`, com instante (`app/chamados/services/vigia_fio.py:592`; `app/chamados/services/tarefa_do_dia.py:730`, `:452`; `app/chamados/services/painel.py:190`).
- L(A) alcança `lista:780` no salto 3, `_jornada_que_mente:224` no 4, `_afastado_sem_registro:588` no 4, `afastados_hoje:45` no 5 e `colabs_nunca_bateu:161` no 5. Os leitores próprios são `_jornada_que_mente` e `afastados_hoje`; os outros fallbacks recebem o argumento (`app/chamados/services/vigia_fio.py:621`, `:210`, `:194`; `app/escala/services/cadastro_realidade.py:791`, `:808`, `:592`, `:864`, `:179`).
- L(A) `→ lista:791 → _jornada_que_mente:238 → periodos_do_dia:270 → intervalo_cadastrado`, profundidade **6**, lê `date.today()` quando o tipo de escala existe (`app/ponto/juiz_batida.py:117`, `:125`, `:270`).
- `censo_alcance:537 → dias_que_seguram:165 → _dias_que_retem:121 → motivos_retencao_celula:1212 → _dias_sem_decisao`, profundidade 4, contém `localdate()` em `app/folha/export.py:1238`, suprimido pelo `hoje` propagado (`app/folha/fora_da_folha.py:165`, `:121`).
- **Outra cadeia até o mesmo leitor**: `censo_alcance:537 → dias_que_seguram:147 → classificar_export:968 → motivos_retencao_espelho:1307 → motivos_retencao_celula:1212 → _dias_sem_decisao:1238`, profundidade **5**; aqui `hoje` foi omitido em `classificar_export:968`, portanto o relógio próprio pode executar (`app/folha/fora_da_folha.py:147`; `app/folha/export.py:968`, `:1307`, `:1212`, `:1238`).
- `censo_alcance:560 → alimentar_portas:1628 → pergunta_viva → V → _dentro_da_janela_viva/classificar_falta`, profundidade 4; contém `date.today()` e `now()` em `app/chamados/juizes.py:595` e `app/escala/servico_jornada.py:387`, mas `agora` permite obter `hoje` e suprime os fallbacks (`app/chamados/juizes.py:888`, `:890`, `:1628`).
- P(A) alcança esses dois leitores no salto **5**, agora sem instante; passando por `_pergunta_materializada_de_fato`, alcança-os no salto **6** (`app/chamados/services/vigia_fio.py:615`, `:259`; `app/chamados/models.py:3127`; `app/chamados/juizes.py:897`, `:1468`, `:1472`, `:1486`, `:696`, `:701`).
- `censo_alcance:652 → função local no_app:577 → _no_app:1645 → perguntas_do_app:61 → pergunta_viva → V → _dentro_da_janela_viva/classificar_falta`, profundidade **6**; recebe o instante de `alim['agora']`, suprimindo os fallbacks (`app/chamados/services/vigia_fio.py:652`, `:577`; `app/chamados/juizes.py:1645`; `app/chamados/services/pergunta_no_app.py:61`).

**Omissões de `hoje/agora=None` no fecho, incluindo as que recuperam o instante**

- P(C)/P(A): `pergunta_viva(self)` omite **ambos**, profundidade 3 (`app/chamados/models.py:3127`; assinatura `app/chamados/juizes.py:865`).
- `censo:415 → classificar:106 → _juiz(p)` omite ambos de `_pergunta_materializada_de_fato`, profundidade 2; `_juiz` é importado com apelido de `chamados.models`, que reexporta o juiz (`app/chamados/services/vigia_carimbo.py:95`, `:106`; assinatura `app/chamados/juizes.py:665`).
- `censo:415 → classificar:110 → _slot_esperado(p)` omite `agora`, profundidade 2 (`app/chamados/services/vigia_carimbo.py:110`; assinatura `app/chamados/juizes.py:344`).
- `_pergunta_materializada_de_fato:755 → _slot_esperado` omite dinamicamente `agora` quando `agora is None`; a expansão o inclui quando existe (`app/chamados/juizes.py:755`).
- `_pergunta_viva_calc:1486 → _pergunta_materializada_de_fato` faz a mesma expansão condicional de `agora`; alcançável no salto 5 por P(C)/P(A), no salto 4 pela alimentação de alcance e no salto 5 por `tarefa → _ler_vida → pergunta_viva` (`app/chamados/juizes.py:1486`, `:1487`; `app/chamados/services/tarefa_do_dia.py:574`, `:537`, `:541`).
- `censo_alcance:560 → alimentar_portas:1628 → pergunta_viva` omite `hoje`, profundidade 2; recupera-o do instante (`app/chamados/juizes.py:1628`, `:888`, `:890`).
- T(C)/T(A) `→ contexto_calendario:506 → _pv` omite `hoje`, profundidade 4; passa `agora` (`app/colaboradores/services/calendario.py:506`).
- Caminho de `no_app` acima: `perguntas_do_app:61 → pergunta_viva` omite `hoje`, profundidade 4; passa `agora` (`app/chamados/services/pergunta_no_app.py:61`).
- `censo_alcance:537 → dias_que_seguram:147 → classificar_export:968 → motivos_retencao_espelho` omite `hoje`, profundidade 3 (`app/folha/export.py:968`, assinatura `:1301`).
- **Classe interna adicional:** `censo:359`/`censo_alcance:593 → tarefa:574 → _ler_vida:534 → carga_da_vida:196 → CargaDaVida.__init__:119 → _slot_esperado`, profundidade **5**; omite `agora`, mas passa `carga=self`, da qual o instante é recuperado (`app/chamados/services/carga_pergunta_viva.py:119`, `:196`; `app/chamados/juizes.py:347`, `:348`).

**Funções diagnósticas públicas**

- `viva_em_disputa_fechada:718 → pergunta_viva → V → _dentro_da_janela_viva/classificar_falta`, profundidade 3; leitores `date.today()`/`now()` em `app/chamados/juizes.py:595` e `app/escala/servico_jornada.py:387`. A chamada omite ambos os argumentos (`app/chamados/services/vigia_fio.py:718`).
- `carimbos_sem_lastro_por_dono:776` tem o mesmo fecho e profundidades, igualmente sem ambos (`app/chamados/services/vigia_fio.py:776`).
- `materializada_viva:813 → carimbos_sem_lastro_por_dono:776` acrescenta um salto: mesmos leitores na profundidade 4 (`app/chamados/services/vigia_fio.py:813`, `:776`).
- Nessas três funções, a perna `V → _pergunta_materializada_de_fato` também alcança os mesmos leitores, um salto depois, e `_slot_esperado` recebe omissão dinâmica de `agora`; profundidades respectivas 4/4/5 para o slot (`app/chamados/juizes.py:1486`, `:1487`, `:755`).
- `disputa_vazia`, `respondida_sem_resposta` e `competencia_do_dia_entregue` não trouxeram leitores ou omissões qualificadas em seus fechos até seis saltos (`app/chamados/services/vigia_fio.py:690`, `:737`, `:785`; chamadas `:697`, `:749`, `:801`, `:804`, `:806`).
- `CargaDaVida.__init__` usa `localtime(ts)` com argumento, sem leitura própria; seus métodos `grade`, `vinculo`, `turnos`, `plantadas`, `batida_na_janela` e `retratada` não acrescentaram leitor físico até seis saltos dos censos (`app/chamados/services/carga_pergunta_viva.py:119`, `:134`, `:152`, `:160`, `:164`, `:185`, `:188`, `:192`).

## 15/11 18:45 · sessao 3ffda51003e4 · - · - · origem=Ronald (tmux)

Adendo confirmado:

- Drawer → `perguntas_vivas_de` → `pergunta_viva` → `_pergunta_viva_calc` → `_pergunta_materializada_de_fato` → `_slot_esperado`: **5 saltos**. `_slot_esperado` aceita `agora=None` em `app/chamados/juizes.py:344`; a chamada em `:755` omite efetivamente `agora` quando ele é `None`, pois o `**` condicional vira `{}`. A borda possui instante em `app/colaboradores/services/drawer.py:21`.
- Nos caminhos através de `contexto_calendario`, o mesmo `**` fornece `agora`, porque a borda o passou e o calendário conserva `_agora` (`app/colaboradores/views.py:1952`; `app/colaboradores/services/drawer.py:124`; `app/colaboradores/services/calendario.py:192`).
- A nova passada AST, considerando `**` condicional, encontrou somente esses candidatos de omissão: `pergunta_viva(hoje)` em `app/colaboradores/services/calendario.py:506`; `pergunta_viva(hoje, agora)` em `app/chamados/juizes.py:1504`; `_pergunta_materializada_de_fato(agora)` em `:1486`; `_slot_esperado(agora)` em `:755`; `motivos_retencao_espelho(hoje)` em `app/ponto/services/fechamento.py:1067`.
- A leitura de `grade_da_celula` e helpers não acrescentou leitor de relógio; o fallback de previsto chama `minutos_previstos_do_dia` em `app/escala/services/leitor_celula.py:411`. Novos leitores por despacho dinâmico fora do fecho lido [não verificado].

## 15/11 18:44 · sessao 3ffda51003e4 · - · - · origem=Ronald (tmux)

Censo das três bordas, por AST com resolução de imports locais/aliases e leitura dos ramos. Caminhos são estáticos; execução em produção permanece [não verificado].

**Bordas já têm instante:** `calendario_ponto` lê `_tz.now()` em `app/colaboradores/views.py:1951`; `contexto_drawer` lê `timezone.now()` em `app/colaboradores/services/drawer.py:21`; `furos_espelho` lê `timezone.now()` em `app/relatorios/views.py:1519`.

**Família compartilhada pelo calendário e drawer**

Prefixos:

- Calendário: `calendario_ponto → contexto_calendario`, chamada `app/colaboradores/views.py:1952`, passa `agora`.
- Drawer: `contexto_drawer → contexto_calendario`, chamada `app/colaboradores/services/drawer.py:123`, passa `agora` na linha 124.

A partir desses prefixos, funções que contêm relógio ou satisfazem o critério de argumento omitido:

1. `contexto_calendario`: `timezone.now()` em `app/colaboradores/services/calendario.py:192`. Fallback **inativo nesses caminhos**, porque ambas as bordas passam `agora` (`views.py:1952`; `drawer.py:124`).
2. `contexto_calendario → status_dos_dias_lavrados`: chamada `app/colaboradores/services/calendario.py:638`; relógio `app/ponto/status_dia.py:253`, guardado por `agora is None` na linha 252. Recebe `_agora`, portanto fallback inativo.
3. `contexto_calendario → pergunta_viva`: alias local `_pv` importado em `app/colaboradores/services/calendario.py:498`, chamada na linha 506. Assinatura `hoje=None, agora=None` em `app/chamados/juizes.py:865`; chamada **omite hoje**, mas passa `agora`. O dia deriva do instante em `app/chamados/juizes.py:888`.
4. `… → pergunta_viva → _pergunta_viva_calc → _dentro_da_janela_viva`: chamadas `app/chamados/juizes.py:897` e `:1468`; leitura `date.today()` em `:595`. Recebe `hoje` derivado do instante; fallback inativo.
5. `… → pergunta_viva → _pergunta_viva_calc → classificar_falta`: import local `_cf` em `app/chamados/juizes.py:1470`, chamada `:1472`, passa `agora` em `:1474`; leitura `_tz.now()` em `app/escala/servico_jornada.py:387`. Fallback inativo porque recebe instante.
6. `contexto_calendario → folha_manda → ate_ontem`: chamadas `app/colaboradores/services/calendario.py:614` e `app/relatorios/cartao_pela_celula.py:413`; leitura `timezone.localdate()` em `app/relatorios/cartao_pela_celula.py:40`. `folha_manda` recebe `_agora` e passa dia derivado dele; fallback inativo.
7. `contexto_calendario → folha_manda → turno_aberto_de`: chamadas `app/colaboradores/services/calendario.py:614` e `app/relatorios/cartao_pela_celula.py:398`; leitura `timezone.now()` em `app/ponto/turnos.py:2291`. Recebe `agora`; fallback inativo. A própria chamada depende de `datas_turno_aberto` não vazio (`cartao_pela_celula.py:396`).
8. `contexto_calendario → folha_manda → totais_da_folha → fechamento_lido → recalcular_fechamento_mes`: chamadas `app/colaboradores/services/calendario.py:614`, `app/relatorios/cartao_pela_celula.py:323`, `:73`, `app/ponto/services/fechamento.py:1759`. **Salto 5** de calendário/drawer. Leituras possíveis `timezone.now()` em `app/ponto/services/fechamento.py:616` e `:1086`; esse caminho não recebe o instante da borda. Exige janela de competência inteira (`cartao_pela_celula.py:321`) e fechamento existente (`:71`). A mesma função contém relógios em `fechamento.py:1143`, `:1161`, `:1189`, mas estes são ramos de escrita **inativos neste caminho**, pois `fechamento_lido` passa `somente_leitura=True` em `:1761` e o ramo de leitura é `:1128`.
9. `… → recalcular_fechamento_mes → motivos_retencao_espelho`: import local `_mre` em `app/ponto/services/fechamento.py:1066`, chamada `:1067`. **Salto 6**; omite `hoje`, cujo default é `None` em `app/folha/export.py:1301`. A função encaminha `hoje=None` à autoridade seguinte em `:1307`; o relógio seguinte ultrapassa o limite de seis saltos.
10. `contexto_calendario → CargaDaJanela.turnos → _turnos_do_colab_puro → papel_por_minuto_da_ata → periodos_do_dia → intervalo_cadastrado`: chamadas `app/colaboradores/services/calendario.py:252`, `app/ponto/turnos.py:3036`, `:1939`, `:1633`, `app/ponto/juiz_batida.py:270`; leitura `date.today()` em `app/ponto/juiz_batida.py:125`. **Salto 6** de calendário/drawer; leitura própria, embora borda já tenha instante. É chamada de instância `_carga.turnos`, que um grafo limitado a nomes importados perde.

**Ramo adicional do drawer**

11. `contexto_drawer → perguntas_vivas_de → pergunta_viva`: import local alias `_vivas_de` em `app/colaboradores/services/drawer.py:79`, chamada `:80`; reexport de `chamados.models` para `chamados.juizes` em `app/chamados/models.py:4225` e `:4226`; `perguntas_vivas_de` chama `pergunta_viva(p)` em `app/chamados/juizes.py:1504`. **Omite hoje e agora**, ambos `None` por default (`:865`), embora drawer já tenha `agora` (`drawer.py:21`).
12. Esse ramo chega a `_dentro_da_janela_viva` no **salto 4**, via `pergunta_viva:897 → _pergunta_viva_calc:1468`, e pode ler `date.today()` em `app/chamados/juizes.py:595`.
13. Mesmo ramo chega a `classificar_falta` no **salto 4**, chamada `app/chamados/juizes.py:1472`, passa `agora=None` em `:1474`; pode ler `_tz.now()` em `app/escala/servico_jornada.py:387`.
14. `… → _pergunta_viva_calc → _pergunta_materializada_de_fato`: chamada `app/chamados/juizes.py:1486`; assinatura com defaults `hoje=None/agora=None` em `:665`. Passa `hoje=hoje`, mas **omite agora efetivamente nesse ramo**: o `**{'agora': agora}` só existe quando não `None` (`:1487`). A função volta a `_dentro_da_janela_viva` em `:696` e `classificar_falta` em `:701`, leitores no **salto 5**, com dia/instante `None`.
15. `contexto_drawer → turnos_do_colab → _turnos_do_colab_puro → papel_por_minuto_da_ata → periodos_do_dia → intervalo_cadastrado`: alias local `_tdc` importado `app/colaboradores/services/drawer.py:44`, chamada `:47`; demais chamadas `app/ponto/turnos.py:1915`, `:1939`, `:1633`, `app/ponto/juiz_batida.py:270`; `date.today()` em `:125`. **Salto 5**, leitura própria.

**Furos**

16. `furos_espelho → escalas_em_limbo`: import local em `app/relatorios/views.py:1517`, chamada `:1554`; leitura `timezone.localdate()` em `app/relatorios/furos.py:59`, **salto 1**, leitura própria apesar do `_agora` já disponível (`views.py:1519`).
17. `furos_espelho → dataset_furos → status_dos_dias_lavrados`: chamadas `app/relatorios/views.py:1543`, `app/relatorios/furos.py:123`; leitura guardada em `app/ponto/status_dia.py:253`, **salto 2**. Instante passado nas duas arestas, portanto fallback inativo.
18. `furos_espelho → dataset_furos → contexto_calendario`: chamadas `app/relatorios/views.py:1543`, `app/relatorios/furos.py:160`; passa `agora` em `:164`. Herda os itens 1, 3, 4 e 5 da família compartilhada com um salto extra: relógios guardados de `contexto_calendario:192`, `_dentro_da_janela_viva:595`, `classificar_falta:387`; `pergunta_viva` omite `hoje` em `calendario.py:506`, mas recebe instante.
19. **Somente no grafo sintático** de furos, também aparecem `folha_manda → ate_ontem`, `turno_aberto_de` e `… → recalcular_fechamento_mes` (este no salto 6). O ramo não executa nesta borda: `dataset_furos` fornece `datas_em_aberto=NAO_PERGUNTADO` em `app/relatorios/furos.py:164`, tornando falso `_perguntou` em `app/colaboradores/services/calendario.py:594` e excluindo o bloco que chama `folha_manda` (`:597` até `:614`).
20. O caminho de furos até `intervalo_cadastrado` exige **sete** saltos e está fora do teto: `furos_espelho → dataset_furos → contexto_calendario → CargaDaJanela.turnos → _turnos_do_colab_puro → papel_por_minuto_da_ata → periodos_do_dia → intervalo_cadastrado`; alternativa pelo D4 chama `_carga.turnos` em `app/ponto/status_dia.py:273`, também totaliza sete.

Não encontrei chamadas de `salario_em`, `custo_mensal`, `vinculos_5x2_6x1_trabalha_feriado` ou `recusa_ancora_futura` no AST de `app/ponto/services/fechamento.py`, `app/ponto/services/espelho.py` ou `app/ponto/motor_calculo_v2.py`; nenhuma dessas arestas diretas deve ser acrescentada [não verificado além dos ASTs lidos].

## 15/11 18:43 · sessao c68596c909d7 · - · - · origem=Ronald (tmux)

Resultados adicionais para consolidar, todos sob `app/`:

- `lavrar_retrato_folha.handle:65 → montar_linhas:1214 → fora_da_folha:205 → _carga:67 → carga_da_tarefa:730 → CargaDaTarefa.__init__:452 → qs_na_fila`; o leitor é `chamados/services/painel.py:190`, mas recebe `agora`, suprimindo seu fallback. A fábrica `_carga_da_tarefa_cls()` devolve a classe; o construtor é chamado pelo corpo de `carga_da_tarefa`, não pela fábrica (`chamados/services/tarefa_do_dia.py:368`, `:392`, `:624`, `:730`).

- `lavrar.handle:65 → montar_linhas:1214 → fora_da_folha:200 → dias_que_seguram:147 → classificar_export:968 → motivos_retencao_espelho`: omite `hoje`, cujo default é `None` (`folha/export.py:1301`). Esta função chama `motivos_retencao_celula` no salto 6 (`folha/export.py:1307`); `_dias_sem_decisao` e seu relógio ficam no salto 7 (`folha/export.py:1212`, `:1238`).

- Outro caminho até `_dias_sem_decisao` está dentro do limite: `lavrar.handle:65 → montar_linhas:1220 → _informativo_rescisao:1288 → informativo_rescisao:1228 → _dias_sem_decisao:1238`. Nele `hoje` é propagado e o fallback não executa (`folha/management/commands/lavrar_retrato_folha.py:65`, `folha/retrato_folha.py:1220`, `:1288`, `folha/export.py:1228`, `:1238`).

- `contadores → linhas_do_retrato → recortar` omite `hoje=None`; também ocorre pelo vigia e pelas três views. É omissão de argumento, mas esses caminhos passam somente eixo `f` ou nenhum grupo, sem consultar dono (`folha/retrato_folha.py:111`, `:120`, `:121`, `:629`, `:632`, `:635`; `folha/vigia_retido_calado.py:65`; `folha/views_retrato.py:198`).

- `vigiar_retido_calado.handle:33 → censo:60 → colabs_nunca_bateu`: possui fallback `timezone.localdate()` em `escala/services/cadastro_realidade.py:161`, suprimido pelo `hoje` passado (`folha/vigia_retido_calado.py:60`).

- `_recorte_da_barra` lê `timezone.now()` em `folha/views_retrato.py:177`; suas três views chamadoras estão em `:258`, `:386`, `:487`. PDF acrescenta `gerar_pdf_auditavel → timezone.now()` (`folha/views_retrato.py:441`, `relatorios/pdf_base.py:133`). Raio-X acrescenta, condicionado a dono `colab`, `placar_aceite → _tz.now()` e `_tz.localdate()` (`folha/views_retrato.py:511`, `:513`, `chamados/services/verbos.py:286`, `:287`).

- `frescor_dos_retratos` exige `agora`; usa conversão com argumento, competências com data explícita e formatação de carimbos com argumento. Nenhum relógio autônomo apareceu nesse fecho (`folha/retrato_folha.py:1386`, `:1408`, `:1411`, `:1441`, `:1444`; `ponto/janelas.py:60`, `:66`).

Para o módulo real `chamados/services/vigia_fio.py`, atenção aos métodos que um grafo apenas de chamadas nomeadas perde:

- `ListaDoAdmin._carregar → lista` passa ambos os valores (`chamados/services/vigia_fio.py:194`). `lista` tem fallback `now` (`escala/services/cadastro_realidade.py:780`), mas chama `_jornada_que_mente(empresas)` em `:791`, cujo `date.today()` é **incondicional** (`:224`).

- `lista:808 → _afastado_sem_registro:592 → afastados_hoje` omite `dia`; este lê `localdate()` (`colaboradores/services/situacao_exibida.py:37`, `:45`). O fallback próprio de `_afastado_sem_registro` está suprimido pelo `hoje` recebido (`escala/services/cadastro_realidade.py:588`, `:808`).

- `_jornada_que_mente:238 → periodos_do_dia:270 → intervalo_cadastrado:125` lê outro `date.today()` quando existe tipo de escala (`ponto/juiz_batida.py:117`, `:125`, `:270`).

- `lista:864 → _nunca_bateu:179 → colabs_nunca_bateu:161` contém fallback de `localdate`, mas propaga `hoje` (`escala/services/cadastro_realidade.py:864`, `:179`, `:161`).

- `censo:359` e `censo_alcance:593` chamam o método `CargaDaTarefa.tarefa`; este chama `_ler_vida` e a função local `ler`, além de `resolver_dia_oferecido_nas_datas(...agora=self.agora)` (`chamados/services/tarefa_do_dia.py:574`, `:578`, `:600`, `:613`). A função local chama `alimentar_fila` com instante (`:588`, `:590`). Esses métodos e a função local precisam integrar o limite de saltos.

_5740 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
