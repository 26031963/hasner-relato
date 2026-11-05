# CODEX-ESTADO

> Ultima resposta processada: **05/11/2026 15:29** · publicado em **05/11/2026 15:29** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 05/11 15:29 · sessao 2f176748e217 · REVISAO-JUIZ-1-V5 · verificador · origem=Code

**Há achados nos escritores e na leitura temporal.** Os contraexemplos abaixo são cadastros construídos a partir do código; ocorrência, quantidade de afetados e valores em produção: **[nao verificado]**. Execução da suíte Django e dos mutantes completos: **[nao verificado]**. [nao verificado]

**1. Quem escreve e quem lê os campos e valores alterados**

| Campo ou valor | Quem escreve ou produz | Quem lê |
|---|---|---|
| `EscalaColaborador.trabalha_em_feriado` | Nasce pelo padrão do vínculo; a porta alterada grava a flag. O comando de sincronização escolhe o valor pelo cargo atual. (`app/colaboradores/services/vinculo.py:287`; `app/colaboradores/services/vinculo.py:843`; `app/colaboradores/management/commands/sincronizar_feriado_admin.py:47`) | Gerador, precedência e classificação financeira do feriado. (`app/escala/models.py:1390`; `app/ponto/precedencia.py:376`; `app/ponto/services/fechamento.py:940`; `app/ponto/calculador/alimentacao.py:521`) |
| `Posto.municipio` | `criar_posto`, `atualizar_posto` e `definir_municipio_posto`; os dois últimos disparam o evento novo. (`app/colaboradores/services/estrutura.py:42`; `app/colaboradores/services/estrutura.py:64`; `app/colaboradores/services/estrutura.py:86`) | `lugar_do_vinculo` lê a FK atual; alcance e nome do feriado saem desse município. (`app/core/feriados.py:121`; `app/core/feriados.py:57`; `app/core/feriados.py:98`) |
| `EscalaColaborador.posto` | A criação usa o posto do colaborador; o reaproveitamento de vínculo também pode substituir esse posto. (`app/colaboradores/services/vinculo.py:266`; `app/colaboradores/services/vinculo.py:280`) | Novos leitores de lugar, presença e veto de cobrança. (`app/core/feriados.py:121`; `app/colaboradores/services/feriado_do_posto.py:105`; `app/ponto/management/commands/detectar_ausencias.py:628`) |
| `Colaborador.posto` e `empresa` | A transferência escreve ambos pela porta do colaborador. O diff acrescenta recusa de alteração de posto em lote. (`app/colaboradores/views.py:1385`; `app/colaboradores/services/colaborador.py:73`; `app/colaboradores/services/colaborador.py:86`) | A empresa atual ainda decide janela, lavra e régua de competência passada; o posto do cadastro permanece como rótulo na API. (`app/colaboradores/services/vinculo.py:851`; `app/ponto/portas/celula.py:199`; `app/core/regua_cct.py:352`; `app/api/views_mensageria.py:1060`) |
| Vínculo escolhido para cada dia | `vinculo_do_dia` escolhe; `vinculos_dos_dias` e as cascas carregam a alimentação. (`app/escala/alimentacao.py:269`; `app/escala/alimentacao.py:285`; `app/escala/alimentacao.py:350`) | Conjunto de feriados, fila por causa, universo de retratação do posto e localização da API. (`app/core/feriados.py:151`; `app/chamados/services/fila_por_causa.py:173`; `app/colaboradores/management/commands/marcar_feriado_do_posto.py:107`; `app/api/views_mensageria.py:976`) |
| Conjunto `feriados` | `feriados_do_colab` compõe o alcance pelo vínculo de cada data. (`app/core/feriados.py:139`) | Precedência, calendário, espelho, fechamento, PDF e comandos de comparação/diagnóstico. (`app/ponto/precedencia.py:331`; `app/colaboradores/services/calendario.py:318`; `app/ponto/services/espelho.py:1029`; `app/ponto/services/fechamento.py:639`; `app/relatorios/pdf_espelho.py:298`; `app/relatorios/management/commands/diff_extrato_parcial.py:58`; `app/ponto/management/commands/diff_espelho_alimentacao.py:93`; `app/ponto/management/commands/desvio_o68b.py:113`) |
| `feriado_so_no_pagamento`, `faixa`, `tipo_do_dia`, `suprimidos` e nome do feriado | A precedência agora usa `not ec.feriado_vira_folga(data)` e produz a classificação. (`app/ponto/precedencia.py:376`; `app/ponto/precedencia.py:382`; `app/ponto/precedencia.py:402`) | Reavaliação de faltas, calendário, status do dia e verificação de folha sem furo. (`app/chamados/services/feriado_reavaliar.py:33`; `app/colaboradores/services/calendario.py:507`; `app/ponto/status_dia.py:298`; `app/folha/services/sem_furo.py:25`) |
| `CelulaDia.trabalha`, `dna`, `escala_geradora`, `dna_anterior`, `regenerada_em`, `regeneracoes` | A regeneração cria ou substitui os valores; o evento de município passa a acioná-la. (`app/ponto/portas/celula.py:243`; `app/ponto/portas/celula.py:260`; `app/colaboradores/services/vinculo.py:903`) | A escolha do vínculo lê a geradora; a precedência lê `trabalha`; a alteração agenda julgamento, dinheiro e reavaliação dos chamados. (`app/escala/alimentacao.py:269`; `app/ponto/precedencia.py:347`; `app/ponto/portas/celula.py:360`; `app/ponto/portas/celula.py:384`; `app/ponto/portas/celula.py:428`) |
| `ChamadoColaborador.status_local`, nota e trilha de reconciliação | O evento chama `encerrar`, que usa `reconciliar`, acrescenta nota e registra `reconciliar_feriado`. (`app/chamados/services/feriado_reavaliar.py:43`; `app/chamados/models.py:607`) | O universo e o vigia leem chamados vivos. Na frota, a nova chamada de reavaliação ocorre sem a rejunção de `celula_id`. (`app/chamados/services/feriado_reavaliar.py:17`; `app/ponto/management/commands/reavaliar_ausencias_feriado.py:35`; `app/ponto/portas/celula.py:433`) |
| Valores de `FechamentoMensal` e `derivado_em` | A porta da flag agenda recálculo; o fechamento grava as rubricas. O evento de município usa `recalculo=False` e invalida a projeção. (`app/colaboradores/services/vinculo.py:857`; `app/ponto/portas/celula.py:397`; `app/ponto/services/fechamento.py:1144`; `app/colaboradores/services/vinculo.py:904`; `app/ponto/services/fechamento.py:1636`) | A autoridade do período do espelho consulta o fechamento gravado. (`app/ponto/services/espelho.py:1024`) |
| Contadores de presença e sugestão por posto | `presenca_em_feriado` passa a atribuir a observação ao vínculo fornecido à juíza. (`app/colaboradores/services/feriado_do_posto.py:102`; `app/colaboradores/services/feriado_do_posto.py:116`) | Pisos de observação, sugestões e pauta da supervisão. (`app/colaboradores/services/feriado_do_posto.py:121`; `app/colaboradores/services/feriado_do_posto.py:136`; `app/colaboradores/services/feriado_do_posto.py:146`) |
| Argumento e papel do cron | A configuração troca `--apply` por `--alarme`, retira o comando dos juízes e declara `vigia`. (`app/config/crons.py:608`; `app/config/crons.py:2015`; `app/config/crons.py:2130`) | O comando conta e produz `SystemExit(2)` quando encontra espúrios com alarme. (`app/ponto/management/commands/reavaliar_ausencias_feriado.py:35`; `app/ponto/management/commands/reavaliar_ausencias_feriado.py:40`) |

**2. Contraexemplos concretos**

**A — Corrigir município troca indevidamente o dono do dia.** Considere **10/12/2026**, com célula gerada por `ecQ`, posto em Maringá, 5x2, flag `False`, vínculo ativo desde janeiro. Existe também `ecP`, iniciado em fevereiro, inativo com término em 20/12, no posto P sem município. Ambos cobrem a data, mas a juíza responde **ecQ**, porque a geradora válida da célula manda. O cadastro admite inícios distintos e intervalo válido. (`app/escala/alimentacao.py:214`; `app/escala/alimentacao.py:269`; `app/escala/models.py:1004`; `app/escala/models.py:1037`)

Ao corrigir P para Londrina em 10/12, o evento seleciona `ecP` apenas porque cobre hoje. A porta aceita substituir a geradora diferente, grava `trabalha=False` e passa a célula para `ecP`. O município corrigido de P acaba reescrevendo o dia que pertencia a Q. (`app/colaboradores/services/vinculo.py:899`; `app/ponto/portas/celula.py:251`; `app/ponto/portas/celula.py:260`)

Pela autoridade declarada, o vínculo do dia continua sendo a geradora válida Q; a correção do município de P não constitui troca desse vínculo. O resultado contraria essa escolha e altera o chão usado para julgar a falta. (`app/docs/CORTES.md:284`; `app/docs/JUIZES.md:98`; `app/ponto/portas/celula.py:439`; `app/chamados/services/feriado_reavaliar.py:33`)

**B — Empresa atual deixa passar dia exportado por outra empresa.** Considere A e B com corte 21: vínculo 5x2 em Londrina, flag `True`, célula de **10/12/2026** como trabalho; A exportou dezembro, sem holerite publicado. Em **05/01/2027**, o cadastro é transferido para B, que não exportou dezembro; o vínculo permanece no posto de A. O fluxo de transferência escreve posto/empresa e histórico, sem encerrar o vínculo. (`app/colaboradores/views.py:1383`; `app/colaboradores/views.py:1392`)

Ao marcar a flag `False`, a janela anterior alcança 10/12. A guarda consulta os TXT de **B**, pela empresa atual do colaborador, e não encontra a exportação de A; a célula pode ser reescrita para folga. O corte específico da obra exige evento somente em competência aberta. (`app/colaboradores/services/vinculo.py:850`; `app/ponto/janelas.py:66`; `app/ponto/portas/celula.py:199`; `app/ponto/management/commands/gerar_celulas.py:161`; `app/ponto/portas/celula.py:238`; `app/docs/CORTES.md:362`)

Esse caminho também agenda recálculo financeiro usando a empresa atual. Ocorrência real e diferença monetária: **[nao verificado]**. (`app/ponto/portas/celula.py:388`; `app/ponto/portas/celula.py:397`)

**C — Célula sem geradora desaparece da presença por posto.** Considere vínculo cobrindo **10/12/2026** em Londrina e célula com `trabalha=True`, lâmpadas preenchidas e `escala_geradora=None`. A juíza deveria usar o vínculo vigente, mas `presenca_em_feriado` fornece lista vazia e descarta a observação. A FK permite nulo. (`app/escala/models.py:1692`; `app/escala/alimentacao.py:285`; `app/colaboradores/services/feriado_do_posto.py:102`; `app/colaboradores/services/feriado_do_posto.py:106`)

Com duas outras observações de feriado, perder essa terceira impede atingir o piso de três e elimina o posto da sugestão. Isso altera a alimentação da autoridade e o universo contado. (`app/colaboradores/services/feriado_do_posto.py:36`; `app/colaboradores/services/feriado_do_posto.py:121`; `app/docs/JUIZES.md:98`)

**3. Regra de negócio alterada sem lastro demonstrado**

O evento de município introduz a regra prática **“todo vínculo deste posto que cobre hoje pode regenerar a célula”**. Ela permite substituir a geradora de outro posto, como no caso A; o lastro citado estabelece que quem decide o vínculo do dia é a juíza da geradora, não esse conjunto de candidatos. (`app/colaboradores/services/vinculo.py:899`; `app/ponto/portas/celula.py:251`; `app/docs/CORTES.md:284`)

Há também mudança explícita da exceção do feriado: a precedência passa de consultar a flag para consultar `feriado_vira_folga`, deixando o 12x36 com flag `False` como trabalho. O código atribui isso ao aval MANHA-0511; confirmação desse aval nos LEIS/CORTES anexados: **[nao verificado]**. A L-086 trata pagamento, e seu próprio registro distingue as regras de dobra por ciclo. (`app/ponto/precedencia.py:371`; `app/ponto/tests/test_a2_precedencia.py:305`; `app/docs/LEIS.md:118`)

**4. Selo que não morde**

O teste alterado **`test_com_lugar_nao_acusa`** só entrega ao analisador uma string contendo `eh_feriado(d, lugar_do_vinculo(ec))`. Não executa a função de lugar nem verifica a origem do município. O analisador aceita qualquer segundo argumento. Portanto, esse teste permanece verde com a cura de localização revertida. (`app/core/tests/test_contract_feriado_com_praca.py:65`; `app/core/contrato_feriado.py:75`)

Na execução isolada, em memória, o analisador retornou `[]` para a string nova, para `lugar_do_colab(colab)` e para `None`; esse comportamento corresponde à condição que verifica somente a quantidade de argumentos. (`app/core/contrato_feriado.py:73`)

O teste **`test_RED_transferencia_as_20h_nao_reescreve_hoje`** também deixa passar uma reversão parcial: ele verifica célula e chamado após transferir o cadastro, mas não consulta a precedência novamente. Restaurar apenas o lugar antigo na precedência preserva essas asserções, pois `gravar` apenas salva o cadastro. Execução desse mutante completo: **[nao verificado]**. (`app/chamados/tests/test_jv1_volta4_posto_do_dia.py:119`; `app/chamados/tests/test_jv1_volta4_posto_do_dia.py:121`; `app/colaboradores/services/colaborador.py:55`)

**5. Cadastro atual decidindo dia ou competência passada**

| Cadastro atual | Escritor localizado | Decisão sobre passado |
|---|---|---|
| `situacao` e `data_demissao` | Desligamento grava ambos; reativação volta para ativo e apaga a demissão. (`app/colaboradores/services/desligamento.py:104`; `app/colaboradores/services/desligamento.py:210`) | O universo de qualquer competência consulta esses valores atuais. (`app/ponto/services/fechamento.py:422`; `app/ponto/services/fechamento.py:430`) |
| `posto` | Transferência altera o posto do colaborador; o reaproveitamento pode alterar o posto do próprio vínculo. (`app/colaboradores/views.py:1385`; `app/colaboradores/services/vinculo.py:266`) | O feriado migrou para o vínculo do dia, mas lê o posto e o município atuais desse objeto. (`app/core/feriados.py:121`; `app/core/feriados.py:151`) |
| `empresa` | Transferência acompanha a empresa do novo posto. (`app/colaboradores/views.py:1386`) | Define universo, janela histórica, guarda de lavra e empresa da régua. É o mecanismo do caso B. (`app/ponto/services/fechamento.py:435`; `app/ponto/services/fechamento.py:612`; `app/ponto/portas/celula.py:199`; `app/core/regua_cct.py:352`) |
| `regime_trabalhista` | Formulário de empresa → porta da estrutura. (`app/colaboradores/views.py:512`; `app/colaboradores/services/estrutura.py:17`) | A régua recebe uma data histórica, mas escolhe CLT/CCT pelo regime atual da empresa atual. (`app/core/regua_cct.py:329`; `app/core/regua_cct.py:352`; `app/core/regua_cct.py:366`) |
| `isento_ponto` | Passe livre → porta do colaborador. (`app/colaboradores/views.py:173`; `app/colaboradores/services/colaborador.py:55`) | A flag atual elimina faltas afirmáveis da competência e inconsistências dos dias do espelho. (`app/core/constants.py:18`; `app/ponto/services/fechamento.py:357`; `app/ponto/services/espelho.py:600`; `app/ponto/services/espelho.py:669`) |
| Vínculo atual | As portas criam, reaproveitam e encerram vínculos. (`app/colaboradores/services/vinculo.py:266`; `app/colaboradores/services/vinculo.py:280`; `app/colaboradores/services/vinculo.py:780`) | O comando escolhe vínculos ativos e o cargo atual para mudar a flag; a porta nova regenera janela anterior e atual. A seleção de feriado por dia usa a geradora/vigência, mas os demais atributos continuam vivos. (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:42`; `app/colaboradores/management/commands/sincronizar_feriado_admin.py:47`; `app/colaboradores/services/vinculo.py:850`; `app/escala/alimentacao.py:269`) |

O município atual já produz uma divergência expressa no teste novo: em **05/12**, corrigir o posto para Londrina mantém a célula de **02/12** como trabalho, mas faz o vigia contar sua falta como espúria. O leitor financeiro também recebe feriados calculados pelo município atual. (`app/chamados/tests/test_jv1_volta3_municipio_e_quem_chegou.py:59`; `app/chamados/tests/test_jv1_volta3_municipio_e_quem_chegou.py:68`; `app/chamados/tests/test_jv1_volta3_municipio_e_quem_chegou.py:74`; `app/ponto/services/fechamento.py:639`)

As leituras temporais estão localizadas; sua ocorrência em produção e o desacordo de regra isolado para retroatividade de situação, regime ou isenção permanecem **[nao verificado]**.

**VEREDITO: ACHADO**

1. `app/colaboradores/services/vinculo.py:899` — **itens 1, 2 e 3**: evento de município substitui a geradora de outro vínculo; **toca dinheiro: sim**, com alteração do previsto e invalidação do fechamento. (`app/ponto/portas/celula.py:260`; `app/colaboradores/services/vinculo.py:905`)
2. `app/ponto/portas/celula.py:199` — **itens 1, 2 e 5**: empresa atual pode ocultar exportação da empresa anterior; **toca dinheiro: sim**. (`app/ponto/management/commands/gerar_celulas.py:161`; `app/ponto/portas/celula.py:397`)
3. `app/core/feriados.py:121` — **item 5**: município atual classifica dias passados e alimenta o fechamento; **toca dinheiro: sim**. (`app/core/feriados.py:151`; `app/ponto/services/fechamento.py:639`)
4. `app/colaboradores/services/feriado_do_posto.py:103` — **itens 1 e 2**: célula sem geradora perde observação apesar de vínculo vigente; **toca dinheiro: nao**, o resultado é sugestão para decisão humana. (`app/escala/alimentacao.py:285`; `app/colaboradores/services/feriado_do_posto.py:136`)
5. `app/core/tests/test_contract_feriado_com_praca.py:65` — **item 4**: teste sintático permanece verde com a cura revertida; **toca dinheiro: nao**, o teste apenas analisa fonte. (`app/core/contrato_feriado.py:75`)

## 05/11 15:28 · sessao 807cf5343cb9 · REVISAO-CRON-MOTOR · verificador · origem=Code

**ACHADO:** o agrupamento pode mudar a decisão do dia; há também cadastro atual decidindo períodos passados nos leitores alcançados pelo diff. (`app/colaboradores/services/calendario.py:132`; `app/core/feriados.py:113`; `app/core/regua_cct.py:352`)

Os contraexemplos abaixo são sintéticos, conferidos com funções da cópia executadas em memória. Ocorrência em produção, integração completa do cron e resultado da suíte: **[nao verificado]**. [nao verificado]

**1. Quem escreve e quem lê os valores alterados**

| Campo/valor | Escritor/produtor | Leitor |
|---|---|---|
| `status_dia`, `status_rotulo` | `lavrar_veredito`, por `_aplicar_status`: `app/ponto/portas/celula.py:705`, `app/ponto/portas/celula.py:709` | D4 lê a coluna ou decide disponibilidade; calendário recebe o resultado: `app/ponto/status_dia.py:287`, `app/ponto/status_dia.py:292`, `app/colaboradores/services/calendario.py:663` |
| `pode_ultimo_recurso` | Calendário combina janela atual da empresa e pergunta viva: `app/colaboradores/services/calendario.py:463`, `app/colaboradores/services/calendario.py:476`, `app/colaboradores/services/calendario.py:738` | Predicado `resolver_dia_oferecido`: `app/colaboradores/services/calendario.py:103` |
| `resolver_dia_oferecido` | Predicado escreve o campo de cada dia: `app/colaboradores/services/calendario.py:769` | Template e porta plural: `app/templates/colaboradores/partials/_calendario_grade.html:125`, `app/colaboradores/services/calendario.py:135` |
| Mapas `datas`, `inst`, `oferecido` | Agrupamento por colaborador e consulta plural: `app/chamados/services/vigia_fio.py:265`, `app/chamados/services/vigia_fio.py:269` | Dois censos e classificação do dono: `app/chamados/services/vigia_fio.py:428`, `app/chamados/services/vigia_fio.py:635`, `app/chamados/services/vigia_fio.py:305` |
| `portas`, `balde`, `papel`, contadores e listas | Porta antecipada, classificação e acumulação: `app/chamados/services/vigia_fio.py:634`, `app/chamados/services/vigia_fio.py:303`, `app/chamados/services/vigia_fio.py:431`, `app/chamados/services/vigia_fio.py:656` | Command imprime e carimba a rodada: `app/chamados/management/commands/vigiar_fio_morto.py:167`, `app/chamados/management/commands/vigiar_fio_morto.py:206` |
| Cadastro de feriados e conjunto por lugar | Tela grava data/abrangência/UF/município; `feriados_de` produz o conjunto: `app/core/views_config.py:63`, `app/core/views_config.py:81`, `app/core/feriados.py:83` | Espelho, fechamento e PDF: `app/ponto/services/espelho.py:1034`, `app/ponto/services/fechamento.py:634`, `app/relatorios/pdf_espelho.py:297` |
| `feriados_motor`, recorte visual e bordas ampliadas | Espelho produz conjunto e recorte; fechamento amplia envelope; PDF usa bordas das batidas: `app/ponto/services/espelho.py:1034`, `app/ponto/services/espelho.py:1035`, `app/ponto/services/fechamento.py:586`, `app/relatorios/pdf_espelho.py:297` | Injeção nos motores e desenho da tela: `app/ponto/services/espelho.py:1118`, `app/ponto/services/espelho.py:1271`, `app/ponto/services/fechamento.py:725`, `app/relatorios/pdf_espelho.py:395` |
| `self.feriados`, `_feriados_injetados` | Motor cria conjunto e guarda identidade; intermitente substitui conjunto; contexto v3 mantém marcador `None`: `app/ponto/motor_calculo_v2.py:790`, `app/ponto/motor_calculo_v2.py:795`, `app/ponto/motor_calculo_v2.py:3557`, `app/ponto/calculador/regras.py:295` | `eh_feriado`; transferência para CCT da v3: `app/ponto/motor_calculo_v2.py:963`, `app/ponto/motor_calculo_v2.py:965`, `app/ponto/calculador/nucleo.py:350` |

O resultado de `eh_feriado` alcança dobra, HE sem previsto, classificação dos períodos e soma semanal. Os resultados monetários chegam a `FechamentoMensal` e `DiaPago`. (`app/ponto/motor_calculo_v2.py:934`, `app/ponto/motor_calculo_v2.py:1449`, `app/ponto/motor_calculo_v2.py:2796`, `app/ponto/motor_calculo_v2.py:3082`, `app/ponto/motor_calculo_v2.py:3157`, `app/ponto/motor_calculo_v2.py:3251`, `app/ponto/motor_calculo_v2.py:3330`; `app/ponto/services/fechamento.py:1140`; `app/ponto/services/fechamento.py:1181`)

**2. Contraexemplos concretos**

**Agrupar altera a decisão.** Cadastro sintético: colaborador sem vínculo, sem marcos/ata/status lavrado; célula de **04/09/2026** com `trabalha=False`; sem pergunta viva respondida; consulta em **05/11/2026**, empresa com corte 21. Existência desse cadastro em produção: **[nao verificado]**.

Batidas do caso, em horário local:

| Tipo | `timestamp` | `criado_em` |
|---|---|---|
| E | 01/09/2026 08:00 | 04/09/2026 08:00:01 |
| E | 03/09/2026 20:00 | 04/09/2026 08:00:00 |
| E | 04/09/2026 08:00 | 04/09/2026 08:00:02 |
| S | 04/09/2026 17:00 | 04/09/2026 17:00:00 |

Esses campos são justamente os insumos da identificação de eco; ela ordena por `criado_em` e compara cada entrada com a anterior dessa ordem. (`app/ponto/turnos.py:353`, `app/ponto/turnos.py:364`, `app/ponto/turnos.py:372`)

- **Consulta individual de 04/09:** o pad começa em 02/09; a entrada de 01/09 fica fora. A entrada de 04/09 vira eco da entrada de 03/09, deixando **nenhum turno próprio em 04/09**. O D4 retorna `folga`; Resolver dia resulta **False**. (`app/ponto/turnos.py:1845`; `app/ponto/turnos.py:368`; `app/ponto/turnos.py:1396`; `app/ponto/status_dia.py:312`; `app/colaboradores/services/calendario.py:103`)
- **Consulta conjunta de 01/09 e 04/09:** a entrada antiga entra na ordenação de chegada e interrompe aquela comparação. A entrada de 04/09 deixa de ser eco e forma **08:00–17:00**. O D4 retorna `sem_certidao`; Resolver dia resulta **True**. (`app/colaboradores/services/calendario.py:132`; `app/ponto/turnos.py:372`; `app/ponto/turnos.py:1417`; `app/ponto/status_dia.py:292`; `app/colaboradores/services/calendario.py:103`)

O corte anexado vincula o vigia ao mesmo campo oferecido pela tela. Aqui, a tela customizada de um dia e a consulta agrupada podem discordar para o mesmo cadastro e dia; o booleano diferente muda o balde para `decisao_do_dia_pendente`. (`app/docs/CORTES.md:82`; `app/templates/colaboradores/partials/_calendario_grade.html:125`; `app/chamados/services/vigia_fio.py:305`)

**Isenção atual apaga eventos de competência passada.** Outro cadastro sintético: fechamento **09/2026** com **2 horas de HE50**, pessoa não isenta naquela competência e incluída no Passe livre em **05/11/2026**. A função real devolveu o evento de 2 horas com a flag falsa e `[]` após mudar somente a flag atual, preservando o mesmo fechamento. (`app/colaboradores/views.py:176`; `app/folha/export.py:407`; `app/folha/export.py:430`)

Isso permite suprimir dinheiro já apurado pela condição atual do colaborador, enquanto a L-095 manda os leitores consumirem a lavratura do dia. É comportamento preexistente, alcançado pela revisão dos leitores. (`app/docs/LEIS.md:113`; `app/core/constants.py:18`; `app/folha/export.py:408`)

**3. Regra de negócio alterada e lastro**

O agrupamento introduz uma dependência nova: **outros dias pedidos na rodada podem decidir se este dia tem decisão pendente**. O contraexemplo acima contraria a equivalência com a leitura individual que o próprio teste declara preservar. (`app/colaboradores/services/calendario.py:130`; `app/ponto/turnos.py:353`; `app/chamados/tests/test_sombra_c1_resolver_em_lote.py:68`; `app/docs/CORTES.md:82`)

No motor, também mudou o contrato: ausência no conjunto injetado passou a ser resposta negativa definitiva, inclusive para conjunto vazio. O teste exige expressamente ignorar o feriado nacional cadastrado nesse caso. (`app/ponto/motor_calculo_v2.py:965`; `app/ponto/tests/test_sombra_c1_motor_feriado_injetado.py:47`)

O aval F3 item 3 é citado nos comentários; seu teor como autorização independente nos anexos fornecidos: **[nao verificado]**. (`app/ponto/tests/test_sombra_c1_motor_feriado_injetado.py:3`)

**4. Selo que passa com a cura revertida**

O teste de paridade passa se a porta plural voltar a montar **um calendário por dia**, mantendo sua assinatura: ele compara respostas, sem contar calendários. Já os dois testes de desempenho detectam essa reversão pela contagem. (`app/chamados/tests/test_sombra_c1_resolver_em_lote.py:71`; `app/chamados/tests/test_sombra_c1_resolver_em_lote.py:41`; `app/chamados/tests/test_sombra_c1_resolver_em_lote.py:64`)

No motor, `test_none_segue_perguntando_ao_juiz` e `test_intermitente_fica_como_hoje` também passam com o comportamento anterior: são controles de preservação. Os testes de zero consulta e conjunto vazio distinguem a mudança. Resultado de execução da suíte: **[nao verificado]**. (`app/ponto/tests/test_sombra_c1_motor_feriado_injetado.py:34`; `app/ponto/tests/test_sombra_c1_motor_feriado_injetado.py:40`; `app/ponto/tests/test_sombra_c1_motor_feriado_injetado.py:47`; `app/ponto/tests/test_sombra_c1_motor_feriado_injetado.py:51`)

**5. Cadastro de hoje decidindo passado**

| Cadastro atual | Quem escreve | Decisão histórica alcançada |
|---|---|---|
| `situacao`, `data_demissao` | Desligamento grava ambos; reativação apaga a demissão: `app/colaboradores/services/desligamento.py:104`, `app/colaboradores/services/desligamento.py:210` | Universo da competência passada lê esses valores atuais: `app/ponto/services/fechamento.py:423`, `app/ponto/services/fechamento.py:430` |
| `posto` | Transferência substitui o posto: `app/colaboradores/views.py:1385` | Município atual decide feriado de qualquer período solicitado: `app/core/feriados.py:113`; três chamadores alterados: `app/ponto/services/fechamento.py:634`, `app/ponto/services/espelho.py:1034`, `app/relatorios/pdf_espelho.py:297` |
| `empresa` | Transferência substitui a empresa: `app/colaboradores/views.py:1386` | Filtro, janela da competência antiga e habilitação de último recurso: `app/ponto/services/fechamento.py:435`, `app/ponto/services/fechamento.py:612`, `app/colaboradores/services/calendario.py:463` |
| `regime_trabalhista` | Formulário e porta de estrutura: `app/colaboradores/views.py:515`, `app/colaboradores/services/estrutura.py:17` | Régua recebe data histórica, mas lê empresa/regime atuais: `app/core/regua_cct.py:352`, `app/core/regua_cct.py:453` |
| `isento_ponto` | Passe livre: `app/colaboradores/views.py:176` | Suprime furos, inconsistências e eventos do fechamento passado: `app/ponto/services/fechamento.py:357`, `app/ponto/services/espelho.py:676`, `app/folha/export.py:407` |
| Vínculo atual | Criação/edição do vínculo: `app/colaboradores/services/vinculo.py:274`, `app/colaboradores/services/vinculo.py:278` | Os caminhos principais consultam vínculo histórico: `app/ponto/services/espelho.py:1070`, `app/relatorios/pdf_espelho.py:307`, `app/ponto/services/fechamento.py:838`. A consulta inicial ao ativo no calendário possui complemento pela geradora: `app/colaboradores/services/calendario.py:258`, `app/ponto/precedencia.py:327` |

Exemplo temporal adicional: desligado em **10/09/2026**, reativado em **01/11/2026**. Antes da reativação fica fora da competência **10/2026**, corte 21; depois, com `situacao='ativo'` e demissão apagada, entra nessa mesma competência passada. A seleção deixa de representar o desligamento fora daquela janela. (`app/colaboradores/services/desligamento.py:210`; `app/ponto/services/fechamento.py:430`; `app/ponto/janelas.py:10`; `app/docs/CORTES.md:81`)

VEREDITO: **ACHADO**

1. `app/colaboradores/services/calendario.py:132` — itens **2–3**: agrupamento muda a decisão do dia; toca dinheiro **não**.
2. `app/folha/export.py:407` — itens **2 e 5**: isenção atual suprime eventos de competência passada; preexistente; toca dinheiro **sim**.
3. `app/core/feriados.py:113` — item **5**: posto atual decide feriado passado; preexistente; toca dinheiro **sim**.
4. `app/core/regua_cct.py:352`, `app/ponto/services/fechamento.py:612` — item **5**: empresa/regime atuais decidem régua e janela passadas; preexistente; toca dinheiro **sim**.
5. `app/colaboradores/services/desligamento.py:210`, `app/ponto/services/fechamento.py:430` — item **5**: reativação altera elegibilidade de competência passada; preexistente; toca dinheiro **sim**.

## 05/11 15:28 · sessao 02a683dc80c3 · - · - · origem=Ronald (tmux)

Extratos normativos para montar os casos, sem leitura da implementação dos módulos proibidos:

| Tema | Regra e referência |
|---|---|
| Previsto | A grade é a fonte única de previsto; não derivar novamente por leitor. `app/docs/LEIS.md:75` — L-024. |
| Geometria | A definição/desenho da escala decide o regime; o rótulo não manda no cálculo. `app/docs/LEIS.md:124` — L-091. |
| Intervalo cadastrado | Sempre sai da jornada; intervalo deslocado dentro do envelope continua sendo intervalo, sem HE. `app/docs/LEIS.md:80` — L-032. |
| Intervalo ausente | `intervalo_indenizavel=True`: indenizar só o suprimido; `False`: pré-assinalado, sem HE e sem indenização, permanecendo pergunta pela marcação faltante. `app/docs/LEIS.md:79` — texto de L-031. |
| Intervalo ausente não vira HE | Corte confirma indenizar só o suprimido, HE zero. `app/docs/CORTES.md:411`. |
| Pagabilidade | Entrada e saída presentes, faltando apenas intervalo: dia pagável; pergunta continua sem bloquear folha. `app/docs/LEIS.md:81` — L-033. |
| Resposta sobre pausa | Resposta validada pelo admin vira registro do intervalo e retira indenização; sem resposta ou recusada permanece a política de indenização; deduplicação pela chave do fato. `app/docs/CORTES.md:273`. |
| Falta decidida | Falta exige ausência lançada de tipo que desconta. `app/docs/LEIS.md:87` — L-039. Furo é pergunta em aberto, não decisão de falta. `app/docs/LEIS.md:120` — L-088. |
| Abono sem sobreposição | Abono cobre só o previsto não coberto pelas batidas. `app/docs/CORTES.md:277` — B-B21. |
| Justificativa parcial | Cobre somente tipo e período; trabalhou: pagar realizado mais abonado do período. `app/docs/CORTES.md:312` e `:315`. Sem período explícito, parcial vai para CADASTRO, não se infere cobertura. `app/docs/CORTES.md:319`. |
| Parcial noturna | Hora final anterior à inicial significa cruzamento da meia-noite, pertencente ao dia inicial do turno. `app/docs/CORTES.md:330`. |
| Folga | Ausência que abona/desconta só alcança dia previsto de trabalho; folga não gera efeito nem consome saldo. `app/docs/LEIS.md:121` — L-089. |
| HE de pontas | Minutos antes/depois dos marcos ficam riscados e pendentes; só passam a contar após autorização humana com trilha. “Não” mantém exclusão e registra ciência. Exportação bloqueada enquanto houver HE pendente. `app/docs/LEIS.md:115` — L-097. |
| Nova ponta após “Não” | Negativa cobre os minutos da trilha, não todo o dia; minutos novos podem gerar nova pendência. `app/docs/CORTES.md:274`. |
| Pontualidade | Atraso mais saída antecipada não superam previsto menos trabalhado; previsto zero implica teto zero. `app/docs/LEIS.md:111` — L-093. |
| Turno em outro horário | Somente quando ambas as pontas distam mais de três horas dos marcos: não cobrar pontualidade, contar trabalho, indicar CADASTRO × REALIDADE. `app/docs/LEIS.md:116` — L-084. |
| Meia-noite | Toda jornada pertence ao dia inicial; cauda não cria trabalho em D+1. `app/docs/LEIS.md:117` — L-085. |
| Domingo/feriado previsto | Plantão previsto nunca se torna HE100 apenas por domingo/feriado. `app/docs/LEIS.md:118` — L-086. |
| CCT Londrina | Cl.38-d: no 12x36, hora normal de 60 minutos, mantendo adicional noturno de 20%. Texto literal em `app/core/regua_cct.py:129`–`:139`; proibição de embutir adicional no piso em `:146`–`:149`; L-036 em `app/docs/LEIS.md:84`. |
| Prorrogação após 05h | CLT + 12x36: não; CLT comum: sim; CCT: depende do cadastro da convenção. Não presumir que cl.38-d sozinha decide prorrogação. `app/docs/LEIS.md:83` — L-035; texto normativo em `app/core/regua_cct.py:156`–`:168`. |
| Janela do intervalo noturno | Intervalo usa a mesma janela de prorrogação do turno; parte ≤ todo. `app/docs/CORTES.md:418`. |
| Furo com número ainda indefinido | Caso de batida ímpar: “Em aberto”, sem número. `app/docs/CORTES.md:278`. |
| Estado antes de encerramento | “Previsto”/“aberto”; depois, ler status lavrado. `app/docs/CORTES.md:415`. |
| Leitores sincronizados | Consumidor lê célula/ata. `app/docs/LEIS.md:95` — L-061. Valor do dia lavrado uma vez, todos leem sem recalcular. `:113` — L-095. Coerência inclusive com cadastro ruim: `:122` — L-099. |
| Invariantes | Partes somam total, idempotência, todo leitor dá mesmo número: `app/docs/LEIS.md:148` — L-114. Previsto independente do realizado, julgamento repetido igual e jornada no dia inicial: `app/docs/CORTES.md:333`. |

**Atenção ao personalizado de 6h corridas:** CLT art.71 §1 exige 15 minutos de intervalo quando o trabalho ultrapassa quatro e não excede seis horas. Cadastro sem pausa não prova dispensa legal. Para preservar o exemplo de 6h, marcar **“lei: cadastro sem intervalo confronta piso CLT; falta norma autorizadora”**; não inventar intervalo registrado nem desconto automático. Outra opção é personalizado de quatro horas sem intervalo, pois “6h” foi dado como exemplo. [CLT, fonte primária](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm). Essa exigência não foi localizada nos trechos normativos locais examinados. [não verificado]

**Números úteis, condicionados ao cadastro descrito:** 07:00–15:20 menos pausa 11:00–12:00 = 440 min; 07:00–19:00 menos 12:00–13:00 = 660 min; 19:00–07:00 menos 00:00–01:00 = 660 min, dos quais 360 noturnos em janela estrita 22:00–05:00, ou 480 se a CCT cadastrada estender até 07:00. Hora de 60 minutos e adicional de 20%, sem converter 360 em 411,43 nem 480 em 548,57. São cálculos de cenários propostos, sustentados pela dedução do intervalo e regras de janela acima; não são medições do sistema. `app/docs/LEIS.md:80`, `:83`, `:84`; `app/core/regua_cct.py:132`–`:139`.

## 05/11 15:28 · sessao d536100f04c1 · - · - · origem=Ronald (tmux)

Leitura restrita aos três documentos; não consultei implementação.

| Tema | Regra normativa | Referência |
|---|---|---|
| Mesma resposta nos leitores | Tela, PDF, badge e contador leem a mesma autoridade; chamado, pergunta, tela e folha leem célula/ata. | `app/docs/LEIS.md:60` L-002; `app/docs/LEIS.md:95` L-061 |
| Valores do dia | Normais, HE, noturno, intra, pontualidade e falta são lavrados e lidos; leitores não recalculam. | `app/docs/LEIS.md:113` L-095 |
| Coerência com qualquer dado | Com o cadastro disponível, sistema deve ser coerente, determinístico e idempotente; erro ou acerto deve aparecer igual em todo lugar. | `app/docs/LEIS.md:122` L-099 |
| Previsto | Grade é fonte única; não derivar previsto em paralelo. | `app/docs/LEIS.md:75` L-024 |
| Falta versus pergunta | Falta é decidida por ausência cujo tipo desconta; furo é marco não cumprido e pergunta em aberto, não decisão. | `app/docs/LEIS.md:87` L-039; `app/docs/LEIS.md:120` L-088 |
| Intervalo faltante | Indenizável=True indeniza só suprimido; False é pré-assinalado, sem HE/indenização, com pergunta. Intervalo cadastrado sempre sai da jornada. | `app/docs/LEIS.md:79` L-031; `app/docs/LEIS.md:80` L-032 |
| Intervalo não bloqueia folha | Entrada e saída batidas, faltando só intervalo: dia pagável e pergunta segue aberta. | `app/docs/LEIS.md:81` L-033 |
| Resposta sobre intervalo | Validada pelo admin vira registro do intervalo e sai indenização; sem resposta ou recusada, segue política de indenizar; dedup pela chave do fato. | `app/docs/CORTES.md:273` |
| Resposta sem validação | Resposta do colaborador segue viva, “só Validar”; encerrar disputa não decide o dia. | `app/docs/CORTES.md:342` |
| Fechar disputa | Exige fio vivo vazio **e nenhuma resposta sem veredito**. | `app/docs/CORTES.md:272` |
| Justificativa aprovada | Encerra chamado de origem pela porta do chamado na mesma transação da aprovação. | `app/docs/CORTES.md:362` item 2 |
| Parcial | Justificativa cobre só tipo e período; ausência parcial conflita somente com batida dentro do período declarado. | `app/docs/CORTES.md:315` |
| Parcial sem horário | Início/fim são cadastro preenchido pelo admin; sem período, lista CADASTRO “parcial sem período”, sem inferir ponta. | `app/docs/CORTES.md:319` |
| Parcial noturna | Fim menor que início cruza meia-noite e pertence ao dia de início do turno. | `app/docs/CORTES.md:330` item 2 |
| Abono | Cobre somente previsto não coberto pelas batidas; justificativa aprovada produz efeito parcial e período pago deve vir da mesma fonte. | `app/docs/CORTES.md:277`; `app/docs/CORTES.md:336`; `app/docs/CORTES.md:353` |
| Folga | Ausência que abona/desconta não produz efeito nem consome saldo em folga. | `app/docs/LEIS.md:121` L-089 |
| Batida que resolve pergunta | Batida que chega absolve na passada seguinte; veredito não lê seu próprio chamado vivo. | `app/docs/CORTES.md:413` |
| Propriedades fixas | Partes somam total; idempotência; previsto independente de realizado; todos leitores com mesmo número. | `app/docs/CORTES.md:240`; `app/docs/LEIS.md:148` L-114 |
| Intercalações | Responder, validar, recusar, fechar, reabrir e lote devem preservar determinismo e trilha. | `app/docs/CORTES.md:333` |

**lei:** os documentos não fixam convergência instantânea dos leitores após o envio de uma resposta; há consumidor a cada um minuto e absolvição “na passada seguinte”. A bateria deve distinguir resposta enviada, resposta validada e processamento concluído; o prazo exato para igualdade após cada ato permanece não decidido. `app/docs/CORTES.md:351`; `app/docs/CORTES.md:413`; `[não verificado]`

O glossário classifica espelho e DiaPago como projeções reconstruíveis e a deduplicação como idempotência pela chave do fato. `app/docs/GLOSSARIO.md:22`; `app/docs/GLOSSARIO.md:27`

## 05/11 15:28 · sessao 30b205fbfa06 · F3-BECO-1 · verificador · origem=Code

1. **Achado 1 — fechado pelo delta. Gravidade: fila; retiro o bloqueio de atomicidade.** O snapshot do defeito, o delete, o `bulk_create` e a re-lavra agora ficam dentro do mesmo `atomic`; uma exceção propagada pela re-lavra desfaz a foto (`app/escala/folgas.py:162`, `app/escala/folgas.py:179`, `app/escala/folgas.py:180`, `app/escala/folgas.py:184`, `app/escala/folgas.py:203`).

   Não encontrei chamador que capture essa falha para aproveitar a foto parcialmente gravada. A view de vinculação e o comando deixam a exceção propagar; os outros dois caminhos já envolviam a geração em transação externa, portanto já desfaziam suas escritas quando a exceção escapava (`app/colaboradores/views.py:245`, `app/escala/management/commands/lavrar_foto_folgas.py:170`, `app/escala/views.py:407`, `app/escala/views.py:411`, `app/colaboradores/services/vinculo.py:187`, `app/colaboradores/services/vinculo.py:275`, `app/colaboradores/services/vinculo.py:289`).

   **O teste novo morde a retirada do `atomic`, pela leitura.** Ele injeta `RuntimeError` na re-lavra, captura a exceção e consulta a foto imediatamente. Sem o `atomic` próprio, a folga de 30/10 permanece na transação do `TestCase`, fazendo falhar a exigência de zero linhas; `assertRaises` não desfaz essa escrita. A fixture efetivamente fornece a data 30 (`app/escala/tests/test_f3_beco1_cadastro_entrou.py:32`, `app/escala/tests/test_f3_beco1_cadastro_entrou.py:183`, `app/escala/tests/test_f3_beco1_cadastro_entrou.py:187`, `app/escala/tests/test_f3_beco1_cadastro_entrou.py:190`, `app/escala/folgas.py:590`). Execução GREEN/RED do mutante: **[não verificado]**.

2. **Achado 2 — permanece a possibilidade de duas solicitações mensais; duplicação de emissão não decorre disso automaticamente. Gravidade: fila.** O snapshot antecede a escrita, sem trava comum de vínculo/mês, e a seleção mensal depende de ANTES verdadeiro e DEPOIS falso (`app/escala/folgas.py:309`, `app/escala/folgas.py:311`, `app/escala/folgas.py:312`, `app/escala/folgas.py:220`, `app/escala/folgas.py:229`).

   Para enumerar os entrelaçamentos, considero duas inclusões que preservam a folga e confirmam com sucesso. A/B são intercambiáveis; o defeito é a inexistência de FolgaDia naquele vínculo/mês (`app/escala/models.py:1249`, `app/escala/folgas.py:312`).

   | Entrelaçamento sob READ COMMITTED | Solicitações de re-julgamento mensal | [nao verificado]
   |---|---|
   | A confirma antes do snapshot de B: A vê ANTES verdadeiro; B vê falso. | **1 vez** (`app/escala/folgas.py:311`, `app/escala/folgas.py:220`, `app/escala/folgas.py:227`). |
   | Ambos tiram snapshot antes do primeiro commit: ambos veem verdadeiro; cada leitura DEPOIS vê ao menos sua própria folga. As comparações DEPOIS podem ocorrer antes ou depois do commit alheio. | **2 vezes** (`app/escala/models.py:1249`, `app/escala/folgas.py:229`, `app/escala/folgas.py:231`). |
   | O mês já estava fora do defeito antes dos dois snapshots. | **0 vezes** (`app/escala/folgas.py:220`, `app/escala/folgas.py:227`). |

   **Com mês inicialmente no defeito e duas inclusões bem-sucedidas, zero não é resultado do entrelaçamento:** a primeira transação a confirmar necessariamente capturou o defeito e vê sua própria inclusão. Na mesma data, eventual espera no `get_or_create` ocorre depois do snapshot e não apaga o verdadeiro já capturado (`app/escala/folgas.py:311`, `app/escala/folgas.py:312`, `app/escala/models.py:1600`). Essas contagens são de solicitações mensais; guardas e exceções podem reduzir os julgamentos efetivamente concluídos (`app/ponto/portas/celula.py:294`, `app/ponto/portas/celula.py:399`, `app/ponto/portas/celula.py:405`).

   No caso de duas passagens, `forcar=True` remove o pulo pela impressão, mas conserva a fábrica e suas deduplicações (`app/ponto/services/cartorio.py:1362`, `app/ponto/services/cartorio.py:997`):

   - **PerguntaDisputa:** a fábrica exclui motivos já existentes por colaborador/dia; `perguntar` encaminha à emissão, cujo gate final procura colaborador/motivo/data e retorna a pergunta existente (`app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/fabrica_da_celula.py:126`, `app/chamados/services/fabrica_da_celula.py:139`, `app/chamados/models.py:2381`, `app/chamados/models.py:2420`, `app/chamados/models.py:2464`).
   - **Chamado:** `abrir` trava a linha do colaborador antes da consulta e retorna o chamado existente; há proteção concorrente explícita nessa porta (`app/chamados/models.py:863`, `app/chamados/models.py:866`, `app/chamados/models.py:867`, `app/chamados/models.py:885`).
   - **Pauta:** o ramo limbo/competência entregue procura cabeças anteriores da âncora dia/DP/marcador, incluindo feitas e canceladas, e escreve somente motivos ausentes. No re-julgamento mensal, exportadas já são excluídas pela guarda (`app/chamados/services/fabrica_da_celula.py:171`, `app/chamados/services/fabrica_da_celula.py:219`, `app/chamados/services/fabrica_da_celula.py:234`, `app/ponto/portas/celula.py:295`).

   Portanto, **a segunda passagem que enxerga a primeira não duplica essas entidades**. Isso não certifica toda corrida: PerguntaDisputa e Pauta usam consulta seguida de criação sem trava própria nesses gates; duplicação efetiva nesse cenário: **[não verificado]** (`app/chamados/models.py:2420`, `app/chamados/models.py:2465`, `app/chamados/services/fabrica_da_celula.py:234`, `app/pautas/services.py:180`).

3. **Achado 3 — permanece a perda possível da cura automática. Gravidade: bloqueia.** A exceção por dia é capturada e o processamento continua; uma falha Python antes da conclusão pode permitir confirmar a folga, consumindo a transição mensal que acionaria nova tentativa (`app/ponto/portas/celula.py:402`, `app/ponto/portas/celula.py:405`, `app/escala/folgas.py:220`, `app/escala/folgas.py:227`).

   O bloco já aparece no parecer anterior anexado e não é alterado pelo diff2 fornecido; confirmação de ancestralidade, autoria e data por Git/blame: **[não verificado]**. Sua localização atual é `app/ponto/portas/celula.py:405`.

   **Existem outros caminhos de produção no código, mas nenhum encontrado garante repetir esse dia apenas porque a folga corrigiu o mês:**

   - O cron configurado executa `processar_cartorio --apply`, sem força. Ele percorre células passadas, porém impressão igual permite pular `indefinida`; a exceção explícita desse atalho é `nunca_bateu` (`app/config/crons.py:597`, `app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/management/commands/processar_cartorio.py:65`, `app/ponto/services/cartorio.py:1358`, `app/ponto/services/cartorio.py:1362`).
   - Signals podem provocar nova avaliação por outro evento, mas chamam sem força; a vigia de hora limita seu universo a hoje/ontem (`app/ponto/signals.py:93`, `app/chamados/signals.py:169`, `app/chamados/signals.py:180`, `app/ponto/services/vigia_de_hora.py:23`).
   - Outro ato de vínculo pode forçar o período novamente; “Cobrar dia” também força uma data passada. São recuperações condicionadas a outro ato (`app/colaboradores/services/vinculo.py:330`, `app/colaboradores/services/pos_vinculo.py:72`, `app/chamados/services/cobrar_dia.py:26`, `app/chamados/services/cobrar_dia.py:39`).

   A impressão não inclui explicitamente a existência mensal de FolgaDia; portanto, só a correção desse insumo não assegura que o cron ultrapasse o atalho (`app/ponto/services/cartorio.py:443`, `app/ponto/services/cartorio.py:452`, `app/ponto/services/cartorio.py:1348`). Recuperação efetivamente ocorrida em produção: **[não verificado]**.

4. **Achado 4 — permanece. Gravidade: bloqueia.** A seleção em `qs_na_fila` ocorre fora da transação da costura. Depois vêm coleta da reversão, gravação do arquivo e carregamento dos pais/perguntas; o `atomic` da porta começa somente ao reapontar (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:59`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:86`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:91`, `app/chamados/services/fio.py:837`).

   **Para a primeira costura efetiva, há quatro SELECTs explícitos após a seleção da fila e antes do UPDATE:** destinos anteriores, pais, perguntas e pai recarregado sob trava. A lavra anterior ao UPDATE acrescenta consultas dependentes do estado; total SQL exato: **[não verificado]** (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:86`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:92`, `app/chamados/services/limbo_folgas.py:237`, `app/chamados/services/fio.py:763`, `app/chamados/services/fio.py:846`, `app/chamados/services/fio.py:847`, `app/chamados/veredito_lavrado.py:219`).

   A trava recarrega o pai, mas a porta aceita imediatamente status não terminal. Assim, silêncio vigente ou mudança para `registrado` entre seleção e trava não impedem a costura, embora retirem o pai da fila (`app/chamados/services/fio.py:765`, `app/chamados/services/painel.py:174`, `app/chamados/services/painel.py:187`, `app/chamados/services/painel.py:188`).

   **Não encontrei chamador pronto que revalide a fila sob trava:** os caminhos examinados desembocam na mesma porta, inclusive migração e reconciliação (`app/chamados/services/limbo_folgas.py:243`, `app/chamados/services/fio.py:947`, `app/chamados/reconciliador.py:1570`). O predicado existente pode ser reutilizado sem inventar regra: `qs_na_fila` aceita `_base`; falta executar essa validação dentro do ato que mantém a trava até a costura (`app/chamados/services/painel.py:158`, `app/chamados/services/painel.py:186`, `app/chamados/services/fio.py:837`).

## 05/11 15:27 · sessao 992a292411ec · - · - · origem=Ronald (tmux)

Extração normativa concluída apenas em `app/docs/CORTES.md` e `app/docs/GLOSSARIO.md`; não consultei as implementações citadas pelos documentos. [nao verificado]

| Tema | Regra escrita e referência |
|---|---|
| Método obrigatório | O gabarito nasce de CLT/CCT/L-NNN aplicada ao cadastro e às batidas; casos antes de código; lacuna vira pergunta de lei; implementação anterior serve apenas para medir impacto. `app/docs/CORTES.md:240` |
| Propriedades fixas | Partes somam total; idempotência; previsto independe de realizado; todo leitor lê o mesmo número. `app/docs/CORTES.md:240` |
| Determinismo | Mesmos dados devem produzir resultado coerente, determinístico e idempotente em todo lugar; dia ímpar deve aparecer “EM ABERTO” igualmente nos cinco leitores. `app/docs/CORTES.md:228` |
| Repetição | “Julgar 2x = julgar 1x”; jornada pertence ao dia de início; previsto não depende de realizado. `app/docs/CORTES.md:333` |
| Falta | Falta significa ausência cujo tipo desconta ou veredito “falta” na célula. Furo sem decisão fica “dias em aberto (a decidir)”, nunca vira falta sozinho. `app/docs/CORTES.md:179` |
| Abono com trabalho | Abono cobre somente o previsto que as batidas não cobriram. `app/docs/CORTES.md:277` |
| Justificativa parcial | Cobre apenas tipo e período; trabalho paga realizado mais abonado do período. `app/docs/CORTES.md:312`; `app/docs/CORTES.md:315` |
| Parcial sem horário | Hora inicial/final é cadastro do admin; ausência parcial sem período vai à lista CADASTRO, sem inferir dentro/fora nem fabricar FATO_EM_AUSENCIA. `app/docs/CORTES.md:319` |
| Parcial noturna | Fim menor que início cruza meia-noite e pertence ao dia inicial do turno. `app/docs/CORTES.md:330` |
| Folga do calendário | Pertence ao vínculo e só vale nos dias dele. `app/docs/CORTES.md:417` |
| Folga avulsa | Pertence à pessoa e sobrevive à troca de vínculo. `app/docs/CORTES.md:417` |
| Chamado em dia sem trabalho | Cobrança cala quando célula lavrada informa `trabalha=False` ou ata preenchida com tipo diferente de trabalho; `trabalha=None` sem ata de outro tipo não é absolvição. `app/docs/CORTES.md:416` |
| Estado do dia | Leitor deve retornar status lavrado da célula; antes do encerramento, previsto/aberto. `app/docs/CORTES.md:415` |
| Espelho/PDF | PDF usa exatamente os dias do espelho, linha a linha, sem regra própria; dia diferente de “trabalhou” leva palavra do veredito na grade e PDF; falta nunca fica vazia. `app/docs/CORTES.md:198` |
| Tela/app/resumo | Na competência, resumo lê fechamento gravado; período livre é calculado uma vez e rotulado “apurado, não é folha”; sem fechamento, “sem apuração ainda”. `app/docs/CORTES.md:188` |
| Tela/folha | Tela nunca deve diferir da folha, inclusive dia passado com fim previsto. `app/docs/CORTES.md:405` |
| Arredondamento/soma | Minutos inteiros na fonte; topo e rodapé são soma dos minutos das linhas, com formatação no fim; proibidos ajuste de rodapé e tolerância. `app/docs/CORTES.md:210` |
| Marca humana | Decisão pelo Resolver dia mostra marca com quem/quando/o quê, lida da ata. `app/docs/CORTES.md:194` |
| Batida ímpar | Dia em aberto, sem número fechado. `app/docs/CORTES.md:278` |
| Intervalo ausente | Corte remete a L-031/L-032: indeniza apenas período suprimido; HE 0 no caso referido. `app/docs/CORTES.md:411` |
| Noturno do intervalo | Intervalo usa a mesma janela de prorrogação do turno; parte ≤ todo. `app/docs/CORTES.md:418` |
| Ausência e retenção | Bateria validada registra “C18 ausência não retém”; não fornece nessa linha valores de minutos ou regra completa de colisão. `app/docs/CORTES.md:412` |
| Fonte e projeções | Célula soberana = Single Source of Truth/Aggregate Root; espelho, DiaPago, fechamento, retrato e pílulas = projeções reconstruíveis. `app/docs/GLOSSARIO.md:11`; `app/docs/GLOSSARIO.md:22` |
| Idempotência do chamado | Glossário define dedup pela chave do fato; não especifica ordem jurídica de folga/falta/atestado/férias. `app/docs/GLOSSARIO.md:27` |
| Prevalência terminológica | Alias da casa é cosmético; em divergência, vale termo técnico e lei citada. `app/docs/GLOSSARIO.md:3` |

`lei:` estes dois documentos, isoladamente, não estabelecem uma precedência completa entre folga/falta/atestado/férias, nem a diferença jurídica entre atestado iniciado antes e durante férias. Não permitem concluir “último lançamento vence” nem atribuir abono a férias por inferência; isso precisa vir de LEIS/CLT/CCT ou ficar `[nao verificado]`. [nao verificado]

## 05/11 15:27 · sessao 2793496fa1ae · - · - · origem=Ronald (tmux)

Referências normativas de `app/docs/LEIS.md`, usando a coluna **LEI**, sem tomar os relatos de implementação como gabarito:

| Tema | Regra escrita | Referência |
|---|---|---|
| Método | Número esperado vem da REGRA aplicada ao cadastro e às batidas; código atual não é gabarito. | `app/docs/LEIS.md:136` — L-110 |
| Previsto | Grade é a fonte única; existindo fonte, nenhuma derivação paralela. | `app/docs/LEIS.md:75` — L-024 |
| Falta decidida | Falta significa ausência lançada cujo tipo desconta. | `app/docs/LEIS.md:87` — L-039 |
| Falta versus furo | Falta exige dia previsto, sem trabalho e sem justificativa, e custa o previsto daquele dia. Marco não cumprido é pergunta em aberto. | `app/docs/LEIS.md:120` — L-088 |
| Folga versus ausência | Ausência que abona/desconta só alcança dia que a escala previa; em folga não produz efeito nem consome saldo. | `app/docs/LEIS.md:121` — L-089 |
| Intervalo | Intervalo cadastrado sempre sai da jornada. | `app/docs/LEIS.md:80` — L-032 |
| Intervalo ausente | Cadastro decide indenização; pré-assinalado não produz HE nem indenização e a batida faltante vira pergunta. | `app/docs/LEIS.md:79` — L-031 |
| Intervalo e retenção | Entrada/saída presentes e somente intervalo faltante: dia pagável, pergunta segue sem bloquear. | `app/docs/LEIS.md:81` — L-033 |
| Noturno | CLT 12x36 não prorroga após 05h; CLT comum prorroga; CCT segue a regra cadastrada. | `app/docs/LEIS.md:83` — L-035 |
| CCT 12x36 | Cl.38-d: hora normal de 60 minutos; adicional noturno de 20% continua devido. | `app/docs/LEIS.md:84` — L-036 |
| Virada da noite | Jornada inteira pertence ao dia de início, inclusive domingo/feriado/semana/DSR; madrugada não cria trabalho no dia seguinte. | `app/docs/LEIS.md:117` — L-085 |
| HE100 | Plantão previsto não vira HE100 por domingo/feriado; dobra ocorre fora da escala. | `app/docs/LEIS.md:118` — L-086 |
| Pontas de HE | Minutos antes/depois dos marcos não contam sem autorização; aparecem riscados e pendentes; autorização exige trilha. | `app/docs/LEIS.md:115` — L-097 |
| Leitores | Tela, PDF, TXT, badge e contador leem mesma autoridade. | `app/docs/LEIS.md:60` — L-002 |
| Chamado | Chamado, pergunta, tela e folha leem célula/ata sem rejulgar. | `app/docs/LEIS.md:95` — L-061 |
| Espelho/PDF | Cartão = espelho; PDF não possui regra própria. | `app/docs/LEIS.md:85` — L-037; `app/docs/LEIS.md:86` — L-038 |
| DiaPago | Dinheiro diário por rubrica é lavrado uma vez e todos os leitores o consomem. | `app/docs/LEIS.md:113` — L-095 |
| Coerência | Sistema deve ser coerente, determinístico e idempotente com o cadastro que tem. | `app/docs/LEIS.md:122` — L-099 |
| Invariantes | Mesmo número em todo leitor, idempotência e partes somando o total valem para qualquer dado, limpo ou sujo. | `app/docs/LEIS.md:148` — L-114 |
| Horário local | Banco usa UTC; apresentação em UTC−3. | `app/docs/LEIS.md:107` — L-080 |

**Lacunas para marcar `lei:`:** não encontrei na coluna normativa de LEIS precedência explícita entre férias, atestado e falta concorrentes, nem política expressa de aceitação/recusa/substituição de lançamentos em cada ordem. Não atribuir automaticamente “vence o último”, “vence férias” ou “vence atestado”. `[nao verificado]`

**Consequência diretamente defensável:** depois de reconhecida a folga válida como dia não previsto, falta/abono por ausência devem ter efeito zero, sem consumir saldo, independentemente da ordem de apresentação dos mesmos fatos; a escolha de qual lançamento estabelece validamente essa folga ainda requer a regra de precedência dos outros textos. `app/docs/LEIS.md:121`, `app/docs/LEIS.md:122`

## 05/11 15:26 · sessao 1942b1d38384 · REVISAO-CRONS-INSTALL · verificador · origem=Code

**ACHADO de cobertura:** a integração do comando pode voltar ao comportamento anterior sem os testes novos perceberem. Também há leitores de cadastro atual decidindo competência passada fora do diff. (`app/core/tests/test_crons_marca_cobre_install.py:85`; `bin/tests/test_crons_sh_so_install_passa_o_host.sh:21`; `app/folha/export.py:767`)

Cadastro real afetado, frequência e valor financeiro em produção: **[não verificado]**.

**1. Quem escreve e quem lê os valores envolvidos**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| Tipo `CronNaoCabe`, mensagem, `nome`, `vizinho` | `inicio_derivado` levanta a exceção; o construtor grava os atributos. (`app/config/crons.py:239`; `app/config/crons.py:271`) | O fallback captura o tipo e consulta `nome`; o teste verifica tipo, nome e mensagem. (`app/core/management/commands/crons.py:95`; `app/core/management/commands/crons.py:97`; `app/core/tests/test_crons_marca_cobre_install.py:66`) Leitor funcional de `.vizinho`: **[não verificado]**. |
| `DUR_MAX_S` e duração usada na derivação | O medidor grava o JSON; a configuração incorpora seus valores. (`bin/crons_duracao.py:97`; `app/config/crons.py:159`) | `inicio_derivado` lê duração, vizinho, piso e tolerância. (`app/config/crons.py:245`; `app/config/crons.py:259`) |
| `sched` da sombra | Nasce como callable; o fallback substitui pelos cinco campos do host e restaura no `finally`. (`app/config/crons.py:350`; `app/core/management/commands/crons.py:111`; `app/core/management/commands/crons.py:115`) | Acesso normal deriva o callable; `linhas`, `corrida_no_horario` e `pipeline_placar` consultam o horário. (`app/config/crons.py:92`; `app/config/crons.py:1819`; `app/config/crons.py:2257`; `app/core/management/commands/pipeline_placar.py:44`) |
| `--crontab-do-host` e conteúdo do host | O shell fornece `crontab -l` por stdin; o parser recebe a opção. (`bin/crons.sh:24`; `app/core/management/commands/crons.py:33`) | `handle` lê stdin/arquivo; o helper procura uma linha ativa, extrai horário e exige igualdade da linha reconstruída. (`app/core/management/commands/crons.py:52`; `app/core/management/commands/crons.py:63`; `app/core/management/commands/crons.py:103`; `app/core/management/commands/crons.py:116`) |
| `PRAZO`, `ITEM_DE_FILA`, data da consulta | Constantes da marca; data padrão capturada no import. (`app/core/tests/marca_cron15.py:24`; `app/core/tests/marca_cron15.py:49`) | `no_prazo` decide validade; o fallback consulta a validade e monta o aviso. (`app/core/tests/marca_cron15.py:52`; `app/core/management/commands/crons.py:97`; `app/core/management/commands/crons.py:118`) |
| Texto renderizado, aviso e `R` | Helper devolve texto/aviso; comando escreve stdout/stderr; shell captura stdout em `R`. (`app/core/management/commands/crons.py:118`; `app/core/management/commands/crons.py:57`; `bin/crons.sh:24`) | Shell compara host/render e entrega `R` ao instalador. (`bin/crons.sh:29`; `bin/crons.sh:36`; `bin/crons.sh:40`) |
| `--seco` | Segundo argumento do shell. (`bin/crons.sh:37`) | Após imprimir o diff, o ramo encerra antes do backup/instalação. (`bin/crons.sh:36`; `bin/crons.sh:37`) |
| Durações e horário fabricados nos testes | Constantes e `mock.patch.dict`. (`app/core/tests/test_crons_marca_cobre_install.py:23`; `app/core/tests/test_crons_marca_cobre_install.py:46`) | Fixtures do host e chamadas diretas ao helper. (`app/core/tests/test_crons_marca_cobre_install.py:50`; `app/core/tests/test_crons_marca_cobre_install.py:99`) |

**2. Contraexemplo concreto**

Um contraexemplo com **cadastro real fornecido** e violação de regra temporal expressa nas LEIS/CORTES anexadas: **[não verificado]**.

Há uma contraprova **sintética e alcançável pelo código**: em **05/11/2026**, colaborador ativo, demissão agendada para **10/11/2026**, empresa com corte 21 e fechamento de **10/2026**, cuja janela é **21/09–20/10**. O agendamento grava a data futura e mantém a situação ativa; o cálculo da janela decorre do corte informado. (`app/colaboradores/services/desligamento.py:77`; `app/colaboradores/services/desligamento.py:85`; `app/ponto/janelas.py:10`)

Ao classificar outubro, o código devolve `fora / rescisao_modulo_proprio` somente porque `data_demissao` está preenchida, sem comparar com a janela. Isso diverge do comentário local que restringe a exclusão à demissão **na janela**. (`app/folha/export.py:834`; `app/folha/export.py:840`)

Esse critério temporal específico nas LEIS/CORTES anexadas: **[não verificado]**. O anexo JUIZES identifica `classificar_export` como autoridade, mas não especifica essa fronteira da demissão. (`app/docs/JUIZES.md:74`)

**3. Regra de negócio alterada sem lastro**

A alteração operacional demonstrada é permitir o install quando a sombra não cabe, mantendo sua linha do host durante o prazo. O próprio diff registra um aval específico para isso. (`app/core/management/commands/crons.py:74`; `app/core/tests/test_crons_marca_cobre_install.py:4`)

A L-062 exige prazo, tripwire e item de fila; a marca declara esses três elementos. (`app/docs/LEIS.md:96`; `app/core/tests/marca_cron15.py:7`)

Registro específico desse aval nas LEIS/CORTES **anexadas**, ou alteração de regra financeira introduzida pelo diff: **[não verificado]**. A ausência desse registro nos anexos não demonstra, sozinha, ausência de autorização.

**4. Selo que não morde**

Reverter **somente a integração de `Command.handle`** para `self.stdout.write(render(), ending='')`, mantendo parser e helper, deixa os testes novos sem observar a perda da cura: os testes Python chamam diretamente `render_mantendo_sombra_do_host`, enquanto o shell usa um `docker` falso que imprime uma resposta constante. (`app/core/management/commands/crons.py:49`; `app/core/tests/test_crons_marca_cobre_install.py:85`; `app/core/tests/test_crons_marca_cobre_install.py:99`; `bin/tests/test_crons_sh_so_install_passa_o_host.sh:21`)

Nesse cenário, o install continua enviando o host, mas o comando deixa de consumi-lo; o render puro recusa a sombra e o shell encerra com “render falhou”. (`bin/crons.sh:24`; `app/config/crons.py:271`)

O caso feliz morde a retirada do **helper**, mas não essa reversão da **integração**. (`app/core/tests/test_crons_marca_cobre_install.py:94`)

**5. Cadastro de hoje decidindo passado**

O caminho alcança competência passada: a configuração declara `lavrar_retrato_folha --apply`; o comando consulta também `janela_anterior`, monta o retrato e chega a `classificar_export`. (`app/config/crons.py:1594`; `app/folha/management/commands/lavrar_retrato_folha.py:60`; `app/folha/retrato_folha.py:1157`; `app/folha/fora_da_folha.py:119`)

| Campo | Escritor identificado | Uso sobre passado |
|---|---|---|
| `situacao` | Desligamento/reativação. (`app/colaboradores/services/desligamento.py:104`; `app/colaboradores/services/desligamento.py:210`) | Situação atual participa da trava de âncora da competência. (`app/folha/export.py:791`) |
| `data_demissao` | Agendamento, desligamento e reativação. (`app/colaboradores/services/desligamento.py:85`; `app/colaboradores/services/desligamento.py:105`; `app/colaboradores/services/desligamento.py:211`) | Qualquer data preenchida exclui da competência consultada. (`app/folha/export.py:840`) |
| `empresa` | Transferência acompanha a empresa do novo posto. (`app/colaboradores/views.py:1385`) | O universo dos fechamentos passados filtra a empresa atual do colaborador. (`app/folha/export.py:767`) |
| `regime_trabalhista` | Formulário envia o valor à porta de estrutura. (`app/colaboradores/views.py:512`; `app/colaboradores/views.py:523`; `app/colaboradores/services/estrutura.py:17`) | A régua recebe competência datada, mas consulta empresa e regime atuais. (`app/core/regua_cct.py:297`; `app/core/regua_cct.py:352`) |
| `isento_ponto` | Tela Passe Livre grava o booleano. (`app/colaboradores/views.py:176`) | O booleano sem data pode dispensar retenção de dias passados e suprimir eventos do fechamento. (`app/core/constants.py:13`; `app/folha/export.py:1174`; `app/folha/export.py:407`) |
| vínculo atual | Executor cria/reativa; encerramento grava `ativa=False/data_fim`. (`app/colaboradores/services/vinculo.py:267`; `app/colaboradores/services/vinculo.py:278`; `app/colaboradores/services/vinculo.py:615`) | A trava de âncora consulta `ativa=True`, sem recorte da competência. (`app/folha/export.py:787`) |
| `posto` | Transferência altera o posto atual. (`app/colaboradores/views.py:1385`) | Na régua examinada, a praça vem do posto do vínculo **da competência**. Leitor do posto atual decidindo passado nesse percurso: **[não verificado]**. (`app/core/regua_cct.py:329`) |

Esses leitores estão fora do diff fornecido; suas ocorrências e consequências em cadastros reais permanecem **[não verificado]**.

**VEREDITO: ACHADO**

1. `app/core/tests/test_crons_marca_cobre_install.py:85` + `bin/tests/test_crons_sh_so_install_passa_o_host.sh:21` — **item 4**: reversão da integração escapa dos testes novos; **toca dinheiro: não**.
2. `app/folha/export.py:840` — **item 5**: data de demissão atual decide exclusão de competência passada; **toca dinheiro: sim**.
3. `app/folha/export.py:767` + `app/folha/export.py:787` — **item 5**: empresa, situação e vínculo atuais decidem universo/trava da competência passada; **toca dinheiro: sim**.
4. `app/core/regua_cct.py:352` + `app/folha/export.py:407` — **item 5**: regime e isenção atuais participam da decisão sobre fechamento passado; **toca dinheiro: sim**.

## 05/11 15:26 · sessao 52722dc7edd4 · - · - · origem=Ronald (tmux)

Achado principal novo, demonstrável pelo fluxo estático:

- Em **10/12/2026**, cadastro construído com ecQ em Maringá (`ativa=True`, início 01/01) e ecP no posto P (`ativa=False`, início 01/02, fim 20/12), ambos 5x2 e `trabalha_em_feriado=False`. A célula de 10/12 foi gerada por ecQ e diz trabalho; a juíza responde ecQ, pois a geradora válida vence os demais candidatos (`app/escala/alimentacao.py:269-284`). Esse cadastro não viola a unicidade colab+início nem a constraint início/fim (`app/escala/models.py:1004`, `app/escala/models.py:1037-1042`).
- Ao corrigir P.municipio de vazio para Londrina, o evento percorre ecP por ele **cobrir hoje**, sem verificar se é o vínculo escolhido pela juíza (`app/colaboradores/services/estrutura.py:58-69`; `app/colaboradores/services/vinculo.py:899-904`).
- A regeneração calcula folga pelo município de P e sobrescreve a célula de ecQ: a condição aceita geradora diferente e grava `trabalha=False`, `escala_geradora=ecP` e DNA novo (`app/ponto/portas/celula.py:243-265`). O E2 passa a reavaliar a falta usando esse chão alterado (`app/ponto/portas/celula.py:428-441`; `app/chamados/services/feriado_reavaliar.py:33-39`).
- Contraria a escolha soberana do vínculo do dia registrada em `app/docs/CORTES.md:284` e a soberania da célula em `app/docs/LEIS.md:71`. A mudança de município passa a trocar o dono do dia mesmo sem alteração de vínculo autorizada pelo ato (`app/colaboradores/services/vinculo.py:899-904`; `app/ponto/portas/celula.py:251-265`). **Itens 1–3; toca dinheiro: sim**, porque muda trabalho/folga e DNA; a chamada usa `recalculo=False` e invalida o fechamento, adiando a derivação (`app/colaboradores/services/vinculo.py:904-905`; `app/ponto/services/fechamento.py:1633-1640`). Ocorrência em produção: **[nao verificado]**.

Outros pontos para consolidação:

1. **Município atual ainda decide passado.** `lugar_do_vinculo` lê a FK viva `Posto.municipio`, sem versão por data (`app/core/feriados.py:118-119`). Os escritores são `atualizar_posto` e `definir_municipio_posto` (`app/colaboradores/services/estrutura.py:64-69`, `app/colaboradores/services/estrutura.py:86-93`). `feriados_do_colab` reaplica essa localização para cada dia histórico (`app/core/feriados.py:147-153`), consumida pela precedência (`app/ponto/precedencia.py:330-331`, `app/ponto/precedencia.py:376-383`). O teste do próprio diff preserva a célula passada como trabalho, mas exige que o vigia passe a contar a falta após a correção atual do município (`app/chamados/tests/test_jv1_volta3_municipio_e_quem_chegou.py:59-74`). **Item 5; toca dinheiro: sim**, pelo leitor de feriados do fechamento (`app/ponto/services/fechamento.py:638-639`). Validade desse recorte por aval externo MANHA-0511: **[nao verificado]**; seu literal não consta nos LEIS/CORTES anexados.

2. **Alteração da flag sem trilha própria.** A porta grava `trabalha_em_feriado` antes da regeneração e não registra evento próprio (`app/colaboradores/services/vinculo.py:843-860`). Num 12x36 com todas as células já materializadas, mudar True→False não muda o dia (`app/escala/models.py:1390-1394`); se a regeneração alterar zero células, sua trilha fica suprimida por `if n` (`app/ponto/portas/celula.py:297-315`). O chamador canônico omite autor e motivo (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:54-56`). Isso deixa sem trilha da flag os requisitos usuário/antes/depois/motivo da L-007 (`app/docs/LEIS.md:65`). **Itens 1/3; toca dinheiro: sim em geral**, conforme o efeito da porta declarado e executado (`app/colaboradores/services/vinculo.py:857-860`; `app/ponto/portas/celula.py:384-399`). Ocorrência concreta em produção: **[nao verificado]**.

3. **Selo isolado que passa sem o evento.** `test_RED_transferido_dentro_da_janela_o_feriado_do_posto_antigo_fica` apenas exige preservação da célula e da falta antigas após a troca de município (`app/chamados/tests/test_jv1_volta3_municipio_e_quem_chegou.py:23-44`). Reverter a adição do evento de município em `atualizar_posto` deixa esse caso sem ação capaz de modificar o passado e preserva suas asserções (`app/colaboradores/services/estrutura.py:64-69`; patch anexado desse escritor). **Item 4; toca dinheiro: não diretamente.** Resultado executado do mutante: **[nao verificado]**.

Nenhum teste foi executado; as conclusões acima são de leitura estática. **[nao verificado]** [nao verificado]

## 05/11 15:26 · sessao 2e237d18a7de · - · - · origem=Ronald (tmux)

Regras verificadas sem ler implementações:

| Tema | Resultado normativo | Referência |
|---|---|---|
| 12x36 noturno CCT | Hora de 60 minutos; adicional de 20% preservado. A cláusula transcrita não determina prorrogação após 05h. | `app/core/regua_cct.py:132`–149; L-036, `app/docs/LEIS.md:84` |
| Pós-05h | L-035 remete ao cadastro da CCT. Há corte explícito de prorrogação **desmarcada para SINEEPRES em 19/10**; pode integrar o cadastro do cenário, sem generalizar automaticamente a toda CCT de Londrina. | `app/docs/LEIS.md:83`; `app/docs/CORTES.md:279` |
| Intervalo | 07–19 ou 19–07 com uma hora de intervalo gozado = **660 minutos efetivos previstos**, por exclusão do intervalo da jornada. | L-032, `app/docs/LEIS.md:80`; CLT71§2, [Câmara](https://www2.camara.leg.br/legin/fed/declei/1940-1949/decreto-lei-5452-1-maio-1943-415500-normaatualizada-pe.html) |
| Personalizado de 6h corridas | CLT71§1 exige 15 minutos quando a duração ultrapassa quatro horas até seis. Cadastro “sem intervalo” não demonstra exceção normativa. Se efetivamente trabalhadas seis horas contínuas, não fabricar intervalo nem reduzir automaticamente o trabalhado: há supressão de 15 minutos a tratar separadamente; CLT71§4 prevê indenização do suprimido com acréscimo de 50%. | [CLT71 §§1,2,4 — Câmara](https://www2.camara.leg.br/legin/fed/declei/1940-1949/decreto-lei-5452-1-maio-1943-415500-normaatualizada-pe.html); ausência de exceção específica: [nao verificado] |
| Atestado de dias | Nos primeiros quinze dias consecutivos de afastamento por doença, a empresa deve pagar salário integral. Doença comprovada é ausência justificada para DSR. | [Lei8.213, art60§3 — Planalto](https://www.planalto.gov.br/ccivil_03/leis/l8213cons.htm); [Lei605, art6§1f e §2 — Câmara](https://www2.camara.leg.br/legin/fed/lei/1940-1949/lei-605-5-janeiro-1949-367115-normaatualizada-pl.html); sem arquivo local específico: [nao verificado] |
| “Atestado de horas” | Convém declarar no cenário **documento válido e abono aprovado**, com período explícito. Declaração de comparecimento sem recomendação de afastamento não equivale automaticamente a incapacidade: a resolução admite abono com anuência do empregador. | [ResoluçãoCFM2.381/2024, art4I e III](https://sistemas.cfm.org.br/normas/arquivos/resolucoes/BR/2024/2381_2024.pdf); sem arquivo local: [nao verificado] |
| Noturno durante ausência abonada | Minutos efetivamente trabalhados à noite podem ser separados objetivamente. Os textos consultados não fecham como converter manutenção salarial por doença em **minutos de AN** no DiaPago. Não fixar AN pago zero por ausência apenas porque não houve labor: marcar **lei:** para integração remuneratória/critério da rubrica. | L-095, `app/docs/LEIS.md:113`; alcance da cl.38-d, `app/core/regua_cct.py:132`; critério específico: [nao verificado] |

CLT58§1 também prevê tolerância de cinco minutos por registro, limitada a dez diários; usar diferenças de uma hora nos casos evita introduzir essa variável. [CLT58 — Câmara](https://www2.camara.leg.br/legin/fed/declei/1940-1949/decreto-lei-5452-1-maio-1943-415500-normaatualizada-pe.html); sem arquivo local específico: [nao verificado]

No piso CLT do 12x36, o art59-A, parágrafo único, considera compensadas as prorrogações noturnas; essa regra não autoriza ignorar a escolha normativa da CCT expressamente exigida por L-035. `app/docs/LEIS.md:83`; [CLT59-A — Câmara](https://www2.camara.leg.br/legin/fed/declei/1940-1949/decreto-lei-5452-1-maio-1943-415500-normaatualizada-pe.html).

## 05/11 15:26 · sessao 9d760e829384 · - · - · origem=Ronald (tmux)

Achado principal: **a empresa atual pode liberar regeneração de dia pertencente a competência exportada de outra empresa**. A nova porta da flag usa `ec.colaborador.empresa` para escolher a janela anterior (`app/colaboradores/services/vinculo.py:850-858`); a porta de regeneração consulta a empresa atual do banco para identificar exportações (`app/ponto/portas/celula.py:199-202`); o carregador enumera apenas os TXT dessa empresa (`app/ponto/management/commands/gerar_celulas.py:161-162`).

Contraexemplo sintético: A e B com corte 21; vínculo 5x2 no posto de Londrina, flag `True`, célula de 10/12/2026 gerada como trabalho; A exportou 12/2026, sem holerite publicado; em 05/01/2027 o cadastro é transferido para B, que não exportou dezembro; o vínculo permanece no posto de A. A transferência grava `posto` e `empresa` do cadastro (`app/colaboradores/views.py:1383-1388`), sem encerrar o vínculo nesse fluxo (`app/colaboradores/views.py:1392-1400`). Ao marcar a flag `False`, a janela anterior inclui 10/12 (`app/ponto/janelas.py:66-72`); a consulta de lavra olha B, não encontra o TXT de A, e libera a reescrita para folga (`app/ponto/portas/celula.py:238-265`). O evento admite exclusivamente competência aberta pelo corte `app/docs/CORTES.md:362`; ocorrência concreta em produção **[nao verificado]**.

**Toca dinheiro: sim**: a regeneração chama `recalcular_por_evento` por competência usando novamente a empresa atual (`app/ponto/portas/celula.py:384-399`), e o fechamento persiste os valores calculados (`app/ponto/services/fechamento.py:1121-1128`, `app/ponto/services/fechamento.py:1144-1173`). O teste de exportada do diff usa uma única empresa e não exerce transferência (`app/chamados/tests/test_jv1_feriado_vira_vigia.py:118-132`).

Outras leituras de cadastro atual no caminho do diff, sem concluir isoladamente violação:

| Campo atual | Escritor localizado | Leitor de passado |
|---|---|---|
| `situacao`, `data_demissao` | `app/colaboradores/services/desligamento.py:103-108`; reativação limpa demissão em `:210-214` | Universo da competência em `app/ponto/services/fechamento.py:422-435` |
| `empresa` | Transferência em `app/colaboradores/views.py:1383-1388`; edição em `:1313` e salvamento em `:1340-1342` | Janela passada em `app/ponto/services/fechamento.py:610-612`; régua em `app/core/regua_cct.py:352-366` |
| `regime_trabalhista` | `app/colaboradores/views.py:512-523` → `app/colaboradores/services/estrutura.py:17-20` | `app/core/regua_cct.py:352-366`, chamado pelo motor da fatia histórica em `app/ponto/services/fechamento.py:725-731` |
| `isento_ponto` | `app/colaboradores/views.py:173-186` → `app/colaboradores/services/colaborador.py:55-61` | Faltas afirmáveis da competência em `app/ponto/services/fechamento.py:355-358`, `:1099-1111`; inconsistência de cada dia do espelho em `app/ponto/services/espelho.py:599-600`, `:653-670` |
| Vínculo atual | A praça e o ciclo da régua perguntam corretamente pelo vínculo da data em `app/core/regua_cct.py:328-339` | A empresa/regime continuam saindo do colaborador atual em `app/core/regua_cct.py:352-366` |

A ocorrência real desses cenários, número de afetados e impacto financeiro em produção são **[nao verificado]**.

## 05/11 15:26 · sessao 3c00a0526c4f · - · - · origem=Ronald (tmux)

**Achado 2 — conclusão sugerida: fila para repetição mensal; duplicação efetiva não demonstrada.** A transição mensal não é serializada antes do snapshot: os escritores fazem leitura ANTES, escrita e leitura DEPOIS sem trava comum de vínculo/mês (`app/escala/folgas.py:309`; `app/escala/folgas.py:311`; `app/escala/folgas.py:355`; `app/escala/folgas.py:383`; `app/escala/folgas.py:229`).

Para dois lançamentos que mantêm a folga, mês inicialmente no defeito, transações que confirmam e ignorando falhas/guardas:

| Entrelaçamento | Re-julgamentos mensais solicitados |
|---|---|
| A lê ANTES verdadeiro, grava, compara DEPOIS falso e confirma; B lê ANTES após commit de A, obtendo falso | **1** (`app/escala/folgas.py:311`; `app/escala/folgas.py:312`; `app/escala/folgas.py:314`; `app/escala/folgas.py:227`) |
| A e B leem ANTES verdadeiro antes do primeiro commit; cada um grava e lê DEPOIS falso, enxergando ao menos a própria folga | **2**, independentemente de uma comparação DEPOIS ocorrer antes ou depois do commit alheio (`app/escala/models.py:1249`; `app/escala/folgas.py:229`; `app/escala/folgas.py:231`) |
| Ambas leem ANTES falso porque o mês já tinha folga | **0** (`app/escala/models.py:1249`; `app/escala/folgas.py:220`; `app/escala/folgas.py:227`) |

**Zero não decorre somente do entrelaçamento no cenário inicial verdadeiro + duas inserções bem-sucedidas**: a primeira transação a confirmar necessariamente viu ANTES verdadeiro; sua própria escrita torna DEPOIS falso (`app/escala/models.py:1249`; `app/escala/folgas.py:311`; `app/escala/folgas.py:312`; `app/escala/folgas.py:229`). Zero julgamentos efetivos ainda pode decorrer de guardas ou exceções, questão distinta da seleção mensal (`app/ponto/portas/celula.py:294`; `app/ponto/portas/celula.py:399`; `app/ponto/portas/celula.py:405`). Folgas na mesma data podem esperar no `get_or_create` pela unicidade vínculo/data; isso ocorre **depois** do snapshot e não apaga um ANTES verdadeiro já obtido (`app/escala/folgas.py:311`; `app/escala/folgas.py:312`; `app/escala/models.py:1600`).

**Dois julgamentos não implicam duas emissões.** `forcar=True` remove o pulo pela impressão, mas conserva a fábrica e suas guardas (`app/ponto/services/cartorio.py:1362`; `app/ponto/services/cartorio.py:997`):

- **PerguntaDisputa:** antes de emitir, a fábrica coleta motivos existentes por colaborador/dia e exclui os já perguntados; ignora somente linhas marcadas como duplicata (`app/chamados/services/fabrica_da_celula.py:118`; `app/chamados/services/fabrica_da_celula.py:126`). `perguntar` chama a emissão de disputa/perguntas; o gate final busca identidade por colaborador/motivo/data e retorna existente (`app/chamados/services/fabrica_da_celula.py:139`; `app/chamados/services/fabrica_da_celula.py:141`; `app/chamados/models.py:2381`; `app/chamados/models.py:2420`; `app/chamados/models.py:2464`). A emissão também contém filtros locais por disputa/motivo/data (`app/chamados/services/disputa_emissao.py:370`; `app/chamados/services/disputa_emissao.py:386`).
- **Chamado:** `ChamadoColaborador.abrir` abre `atomic`, trava Colaborador **antes da leitura**, relê a chave e retorna existente; portanto há proteção concorrente real nessa porta (`app/chamados/models.py:863`; `app/chamados/models.py:866`; `app/chamados/models.py:867`; `app/chamados/models.py:885`). A fábrica de disputa passa por ela (`app/chamados/services/disputa_emissao.py:504`).
- **Pauta:** só existe o desvio limbo/competência entregue; a guarda procura todas as cabeças anteriores da âncora dia/DP/marcador e retira motivos já presentes, incluindo histórico feito/cancelado (`app/chamados/services/fabrica_da_celula.py:171`; `app/chamados/services/fabrica_da_celula.py:185`; `app/chamados/services/fabrica_da_celula.py:219`; `app/chamados/services/fabrica_da_celula.py:222`; `app/chamados/services/fabrica_da_celula.py:234`). No caminho mensal, dias exportados são barrados antes de incluir `rejulgar` (`app/ponto/portas/celula.py:294`).

**Limite:** a idempotência de PerguntaDisputa/Pauta comprova que uma segunda passagem que **enxerga** a primeira não duplica; os respectivos gates não mostram trava própria entre a consulta de existência e o create (`app/chamados/models.py:2420`; `app/chamados/models.py:2465`; `app/chamados/services/fabrica_da_celula.py:234`; `app/pautas/services.py:180`). No fluxo mensal, gravação da célula precede a fábrica e pode serializar aquele dia quando há escrita; porém `carimbar` permite no-op, logo não equivale a trava explícita universal (`app/ponto/services/cartorio.py:1802`; `app/ponto/services/cartorio.py:1822`; `app/ponto/services/cartorio.py:997`; `app/ponto/portas/celula.py:669`; `app/ponto/portas/celula.py:826`). Duplicação efetiva de PerguntaDisputa/Pauta nas condições integrais da produção: **[não verificado]**.

## 05/11 15:26 · sessao 0b11d97c29de · - · - · origem=Ronald (tmux)

4. **Conclusão: permanece; gravidade: bloqueia.** O comando materializa os pais na fila fora de um `atomic` envolvente e depois grava a reversão, busca pais e só então costura (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:46`, `:59`, `:86`, `:91`, `:92`, `:95`). A transação da costura começa apenas em `fio.reapontar_resolvedora`, que recarrega/trava o pai, mas aceita imediatamente qualquer status não terminal; não confere silêncio nem pertença à fila (`app/chamados/services/fio.py:837`, `:760`, `:762`, `:763`, `:765`). A fila exige status de cobrança e exclui silêncio vigente (`app/chamados/services/painel.py:186`, `:187`, `:188`).

A janela começa no SELECT da linha 59 e só termina no UPDATE da pergunta, após a lavra; inclui inclusive I/O do arquivo de reversão (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:59`, `:91`; `app/chamados/services/fio.py:845`, `:846`, `:847`). Pai silenciado ou tornado `registrado` nesse intervalo continua elegível à porta se permanecer não terminal (`app/chamados/services/painel.py:174`, `:187`, `:188`; `app/chamados/services/fio.py:765`).

**Consultas:** para a primeira costura efetiva, depois da seleção da fila existem **quatro SELECTs explícitos antes do UPDATE**, mais as consultas variáveis da lavra: destinos anteriores, pais, perguntas e pai sob trava (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:86`, `:92`; `app/chamados/services/limbo_folgas.py:237`; `app/chamados/services/fio.py:763`). E4-c não acrescenta consulta para o pai LIMBO: seu módulo tem escopo `PERIODO`, que retorna antes das verificações de turno (`app/chamados/catalogo/modulos.py:126`; `app/chamados/juizes.py:1910`, `:1912`, `:1914`). A lavra executada antes do UPDATE chama o juiz de materialização, cujo número de consultas depende de tipo, via, resposta e relações carregadas; total exato **[não verificado]** (`app/chamados/services/fio.py:846`; `app/chamados/veredito_lavrado.py:219`; `app/chamados/juizes.py:645`, `:670`, `:716`, `:718`, `:772`, `:776`). Para perguntas posteriores há também todas as consultas e escritas das anteriores; não há transação única para o lote (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:94`; `app/chamados/services/limbo_folgas.py:237`, `:243`; `app/chamados/services/fio.py:837`).

**Não encontrei outro chamador que ofereça revalidação de fila sob trava.** Os caminhos existentes usam a mesma porta: helper LIMBO, agrupamento de furo, migração de resolvedoras, declaração do colaborador, gate de fechamento e reconciliador (`app/chamados/services/limbo_folgas.py:243`, `:276`; `app/chamados/services/fio.py:947`; `app/chamados/services/declaracao_texto.py:320`; `app/chamados/models.py:420`; `app/chamados/reconciliador.py:922`, `:1570`, `:1582`). A migração deduplica perguntas por fato, sem validar fila (`app/chamados/services/fio.py:931`, `:937`, `:947`). O gate seleciona sobrevivente pelo C8, que filtra `VIVOS`, sem silêncio/fila e sem trava nessa consulta (`app/chamados/models.py:373`; `app/chamados/juizes.py:1839`, `:1844`). Portanto não há, nesses caminhos examinados, operação pronta que o comando possa simplesmente chamar para ganhar essa garantia; o predicado existente reutilizável continua sendo `qs_na_fila`, inclusive com `_base`, mas sua execução sob a trava teria de ser acrescentada ao ato (`app/chamados/services/painel.py:158`, `:186`; `app/chamados/services/fio.py:837`, `:840`).

## 05/11 15:26 · sessao 03898f3b1a52 · - · - · origem=Ronald (tmux)

Achado 3 — **o risco permanece; recuperação automática garantida não encontrada** (`app/ponto/portas/celula.py:402-409`; `app/ponto/services/cartorio.py:1358-1364`).

- A chamada usa `forcar=True`, mas captura `Exception` por dia e continua; uma falha Python pode deixar a transação confirmar sem concluir o dia (`app/ponto/portas/celula.py:402-409`). Falhas SQL que invalidem a transação têm efeito distinto; ocorrência concreta [não verificado].
- O `except` não foi introduzido pelo diff2 fornecido, que altera somente folgas e teste; comprovação por Git/blame e data de introdução **[não verificado]**: a raiz da cópia não contém `.git`, e `git` respondeu “not a git repository” **[não verificado]**. [nao verificado]
- O cron configurado chama `processar_cartorio --apply`, sem `--forcar` (`app/config/crons.py:597`). O comando enumera todas as células passadas, mas o juiz pula impressão igual mesmo com veredito `indefinida` — a única exceção explícita nesse teste é `nunca_bateu` (`app/ponto/management/commands/processar_cartorio.py:49-65`; `app/ponto/services/cartorio.py:1358-1364`).
- Signals de batida, ausência e justificativa chamam sem força; podem recuperar quando outro insumo mudar, mas não constituem repetição garantida pela cura mensal (`app/ponto/signals.py:85-93`; `app/ponto/signals.py:131-165`). O signal de chamado também chama sem força e só segue quando criado ou quando muda status (`app/chamados/signals.py:165-180`).
- A vigia de hora alcança hoje/ontem, com filtros de turno, e chama sem força; não cobre genericamente o passado mensal (`app/ponto/services/vigia_de_hora.py:15-23`; `app/ponto/services/vigia_de_hora.py:120-131`).
- Existem recuperações forçadas por outro ato: alteração de vínculo e correção retroativa usam `reconciliar_apos_vinculo`, que percorre dias e exclui humanos/já julgados (`app/colaboradores/services/pos_vinculo.py:50-76`; `app/colaboradores/services/vinculo.py:330`; `app/colaboradores/management/commands/corrigir_escala_retroativa.py:237`). A ação manual “Cobrar dia” força julgamento passado (`app/chamados/services/cobrar_dia.py:26-39`).
- `furos_sem_cobranca` força julgamento, porém seleciona somente `veredito='furo'`, excluindo o caso `indefinida` (`app/ponto/services/furos_sem_cobranca.py:61-63`; `app/ponto/services/furos_sem_cobranca.py:128`).
- `supra_juiz` lê a lavra para retratar; o cron de perguntas stale chama `lavrar(p)` de PerguntaDisputa. Esses caminhos não re-julgam a célula (`app/ponto/management/commands/supra_juiz.py:165-166`; `app/ponto/management/commands/supra_juiz.py:245-250`; `app/chamados/management/commands/lavrar_perguntas_stale.py:54`; `app/chamados/management/commands/lavrar_perguntas_stale.py:85-87`).

Gravidade: **bloqueia a garantia de cura completa após exceção**, embora a anterioridade histórica desse tratamento e seu enquadramento como regressão permaneçam **[não verificado]** (`app/ponto/portas/celula.py:405-409`; `app/ponto/services/cartorio.py:1362-1364`).

## 05/11 15:25 · sessao f75be3e86c96 · H-02 · sonda · origem=Code

Referências **S1–S34**: linhas do bloco fornecido, contando `sonda_leitura:` como S1 e incluindo linhas vazias; são referências à saída, não ao script.

**VEREDITO: dono=BATIDA no caminho alimentado descrito abaixo, salvo prioridade de CADASTRO. O dono efetivamente retornado em produção é [não verificado]: a sonda não imprime todos os insumos do juiz.** (`app/ponto/management/commands/e6_oraculo.py:98`, `:104`, `:168`.)

1. **O par e o nascimento do zero.**

   A sonda imprime E **124339**, 11/10 17:31:27, e S **124645**, 12/10 06:31:31, ambas não retratadas; imprime também um turno fechado, atribuído a **11/10**, cruzando meia-noite. Isso comprova aquele resultado da sonda; seus argumentos de pareamento são **[não verificado]**. (S24–S25; S32, linha `TURNO ... date(2026, 10, 11)`.)

   **Com os marcos do DNA alimentados, a cópia separa esse par.** O teto usa `hi→hf + 4 h`: 21:30→05:42 = **492 + 240 = 732 minutos**, independentemente dos `minutos_jornada=432`. O vão real é aproximadamente **780 minutos**, portanto excede o teto. A exceção L-084 exige **as duas pontas** distantes mais de 180 minutos; aqui são aproximadamente 239 na E e 49 na S, logo não se aplica. (`app/ponto/turnos.py:64`, `:1428`, `:1438`, `:1445`; S10, S24–S25.)

   Nesse caminho, fica **E aberta em 11/10 e S órfã em 12/10**; o aberto de 11/10 impede devolver a S à véspera. A pós-passagem também respeita o teto. O teste específico de col129 fixa essa mesma separação em 25→26/10. (`app/ponto/turnos.py:1293`, `:1451`, `:1081`; `app/ponto/tests/test_col129_par_separado_pelo_teto.py:57`.)

   **Órfã do turno e órfã do marco são perguntas distintas:** apesar da separação geométrica, a ata de 11/10 reconhece S06:31 no marco S05:42, mantém E21:30 apagada e registra `orfas=['17:31']`. (S11–S12.) O casamento por marco tem envelope próprio; não equivale ao fechamento de um turno. (`app/escala/utils.py:585`, `:625`.)

   Eco de flush não explica essa E no universo fornecido: a regra exige outra E distante entre 30 minutos e 14 horas; as E disponíveis estão separadas por mais de 19 horas. (`app/ponto/turnos.py:342`, `:368`; S20, S22, S24.) Os horários de chegada, `criado_em`, são **[não verificado]**.

   **O zero nasce na apuração dos turnos sem par fechado:** `realizado_dos_turnos` marca aberto e não soma turno sem fim; a grade copia o resultado para `minutos_realizados`, e o cartório o grava na ata. No motor, o adaptador entrega `(entrada, None)` e transforma S órfã somente em anomalia; a ata com entrada apagada não recompõe o par. (`app/ponto/turnos.py:731`, `:769`; `app/escala/utils.py:2066`; `app/ponto/services/cartorio.py:704`; `app/ponto/adaptador_motor.py:30`; `app/ponto/motor_calculo_v2.py:632`.)

   Portanto, **há zero anterior à tela comprovado na célula**: `minutos_realizados: 0`. A saída efetiva do motor, `DiaPago`, fechamento e tela desse dia são **[não verificado]**; não vieram na sonda. (S12.)

2. **Dono pela L-099.**

   O E6 não usa diretamente o turno cru impresso na sonda: `InsumosDoDono` pede `turnos_do_colab`, que carrega o mapa DNA e o passa ao pareador. (`app/ponto/management/commands/e6_oraculo.py:144`; `app/ponto/turnos.py:1907`, `:1990`.)

   Com a separação acima, `batida_fora_de_turno` encontra realizado aberto e procura especificamente uma **entrada apurável daquele dia sem saída**. A batida identificadora é **E124339**; sua existência e não retratação estão na sonda. Esse insumo faz o juiz devolver **BATIDA antes da paridade**, mesmo havendo duas pontas cruas. (`app/ponto/management/commands/e6_oraculo.py:181`, `:191`, `:104`; S24.)

   Contudo, `folga_nao_definida` ou ausência de vínculo válido fazem **CADASTRO vencer primeiro**. Esses indicadores não estão impressos; `dna.vinculo_id=853` sozinho não demonstra sua validade atual. O retorno completo em produção permanece **[não verificado]**. (`app/ponto/management/commands/e6_oraculo.py:98`; `app/ponto/supra_juiz.py:166`, `:193`; S10.)

   A ata fornecida satisfaz a classe **`BATIDA_ORFA_FORA_TOLERANCIA`** — trabalho, previsto positivo, realizado zero e órfã presente. Isso é classe do classificador, não prova de dono ESTRUTURA. (`app/ponto/supra_juiz.py:515`; S12.) A origem específica do `veredito trabalhou`, sem `veredito_via`, protestos ou histórico, é **[não verificado]**. (S10.)

3. **Quem escreve e quem lê.**

   | Campo | Quem produz e grava | Quem lê no caminho relevante |
   |---|---|---|
   | `ata.orfas` | A grade produz as sobras; `ata_do_dia` copia as horas; a porta persiste a ata. (`app/escala/utils.py:621`, `:1853`; `app/ponto/services/cartorio.py:711`; `app/ponto/portas/celula.py:775`.) | `_orfas_da_ata`, `periodos_do_dia` e o classificador. (`app/escala/services/leitor_celula.py:175`; `app/ponto/juiz_batida.py:161`; `app/ponto/supra_juiz.py:514`.) |
   | `ata.lampadas` | `lampadas_do_dia` traduz as células da grade; `ata_do_dia` monta a ata; a mesma porta grava. (`app/ponto/services/cartorio.py:677`, `:702`; `app/ponto/portas/celula.py:775`.) | Leitor da célula e juiz dos períodos; o pareador recebe os papéis derivados dessa leitura. (`app/escala/services/leitor_celula.py:114`; `app/ponto/juiz_batida.py:157`; `app/ponto/turnos.py:1631`, `:1680`.) |
   | `veredito` | O cartório consulta `classificar_dia` e chama `lavrar_veredito`; a porta atribui e persiste, respeitando decisão administrativa anterior. (`app/ponto/services/cartorio.py:1413`, `:1802`; `app/ponto/portas/celula.py:745`, `:778`.) | `grade_da_celula` transporta o valor gravado. (`app/escala/services/leitor_celula.py:395`.) |
   | Minutos realizados | `realizado_dos_turnos` calcula; a grade publica; o cartório copia para a ata. (`app/ponto/turnos.py:705`; `app/escala/utils.py:2079`; `app/ponto/services/cartorio.py:706`.) | O leitor da célula lê os minutos da ata; o espelho também consulta a autoridade dos turnos e publica minutos/horas. (`app/escala/services/leitor_celula.py:394`; `app/ponto/services/espelho.py:675`, `:783`, `:799`.) |

   Os valores observados desses campos em 11/10 são E apagada, S06:31 acesa, órfã17:31, realizado0 e veredito `trabalhou`. (S10–S12.)

4. **Há cadastro de HOJE no caminho?**

   **Sim, existe leitura explícita pela data de hoje:** `intervalo_cadastrado` chama `tipo_escala.pausas_do_dia(date.today())`, inclusive quando falta uma borda. O leitor dos papéis também pode selecionar vínculo por `ativa,data_inicio`. (`app/ponto/juiz_batida.py:125`, `:265`; `app/ponto/turnos.py:1556`, `:1591`.)

   **Isso não demonstra que essa leitura causou o zero de 11/10.** As bordas desse dia vêm do DNA; com hi apagada, o juiz declara `em_aberto`, e o motor preserva a geometria. O teto explicado no item 1 usa os marcos congelados fornecidos. (`app/ponto/juiz_batida.py:157`, `:259`; `app/ponto/motor_calculo_v2.py:632`; `app/escala/utils.py:103`; S10–S11.) O cadastro vivo efetivamente encontrado e seu efeito nessa execução são **[não verificado]**.

5. **Contraexemplo concreto e limite da evidência.**

   **Contraexemplo estrito — E fora do raio de atribuição e S junto ao marco seguinte, tratadas diferentemente — [não verificado].** A sonda só traz outro col129: em 09 e 10/10 as E estão a 7 e 6 minutos do marco, com realizados540 e539; em 12/10, a batida17:30 é **S**, não E. Esses dias não reproduzem a forma estrita da queixa. (S4–S9, S26.)

   Na cópia há um controle mais fraco: E16/10 **19:55**→S17/10 **05:10**, marcos20:00→05:00, é exigido fechado pelo mesmo pareador; porém a E está apenas cinco minutos antes do marco. (`app/ponto/tests/test_h_portao_teto_do_dia.py:28`, `:38`.) Já o caso forte col129, E25/10 **17:30**→S26/10 **06:30**, mesmo DNA da queixa, exige **o mesmo tratamento de separação**, não tratamento diferente. (`app/ponto/tests/test_col129_par_separado_pelo_teto.py:57`.)

## 05/11 15:25 · sessao 12bcc9dc9b9e · - · - · origem=Ronald (tmux)

1. **Item 4 — selo que não morde a reversão do lugar na precedência.** `test_RED_transferencia_as_20h_nao_reescreve_hoje` transfere o cadastro e verifica somente `CelulaDia.trabalha` e `Chamado.status_local`; não consulta o juiz depois da transferência (`app/chamados/tests/test_jv1_volta4_posto_do_dia.py:111`, `:119`, `:121`, `:124`). Restaurar a leitura antiga de município do cadastro na precedência, com o helper antigo correspondente, mantendo o gerador atual, deixa esse teste passar: a célula nasce em Maringá e `gravar` apenas salva o novo posto, sem regeneração (`app/colaboradores/services/colaborador.py:55`, `:61`). O defeito reapareceria na consulta posterior ao juiz, hoje migrada para `feriados_do_colab` (`app/ponto/precedencia.py:330`). Derivação estática; execução do mutante [nao verificado]. **Toca dinheiro: sim**, pois a função de feriados também alimenta o fechamento (`app/ponto/services/fechamento.py:639`).

2. **Item 4 — selo sintático independente da cura.** O teste modificado `test_com_lugar_nao_acusa` analisa apenas uma string com `eh_feriado(d, lugar_do_vinculo(ec))`; não executa `lugar_do_vinculo`, nem verifica qual posto ela lê (`app/core/tests/test_contract_feriado_com_praca.py:65`). O verificador aceita qualquer chamada com dois argumentos, independentemente do conteúdo do segundo (`app/core/contrato_feriado.py:73`, `:75`). Portanto, esse teste passa com a implementação da cura revertida; execução [nao verificado]. **Toca dinheiro: não diretamente**; a cobertura omitida alcança o feriado usado no fechamento (`app/ponto/services/fechamento.py:639`).

3. **Itens 1–3 — alimentação incompleta na presença por posto.** Quando a célula não tem geradora, `presenca_em_feriado` passa `[]` à juíza e descarta a observação por falta de posto (`app/colaboradores/services/feriado_do_posto.py:102`, `:103`, `:106`). Entretanto, a juíza resolve célula sem geradora pelo vínculo vigente fornecido na alimentação (`app/escala/alimentacao.py:285`); o carregador de um dia faz precisamente essa carga quando falta a geradora (`app/escala/servico_jornada.py:55`, `:58`). A autoridade é a declarada no corte `JUIZ-VINCULO-DO-DIA-NASCE` (`app/docs/CORTES.md:284`).

   Contraexemplo de cadastro permitido: vínculo cobrindo **10/12/2026**, posto em Londrina, célula `trabalha=True`, ATA com lâmpadas e `escala_geradora=None`; a presença desse dia é omitida, embora o vínculo do dia possa ser resolvido pela vigência (`app/colaboradores/services/feriado_do_posto.py:94`, `:97`, `:103`; `app/escala/alimentacao.py:285`). A FK aceita nulo e usa `SET_NULL` na exclusão do vínculo (`app/escala/models.py:1692`); existe exclusão de vínculos na reversão de snapshot (`app/colaboradores/services/vinculo.py:944`). Com duas outras observações de feriado, perder a terceira impede atingir o piso de três e elimina o posto do resultado (`app/colaboradores/services/feriado_do_posto.py:36`, `:121`). Existência desse cadastro em produção [nao verificado]. **Toca dinheiro: não**: o resultado é sugestão para decisão humana (`app/colaboradores/services/feriado_do_posto.py:28`, `:136`).

4. **Item 1 — escritor relevante fora do diff.** `EscalaColaborador.posto` não é histórico imutável: `executar_vinculo` reaproveita o vínculo da mesma data de início e substitui seu posto pelo posto atual do colaborador (`app/colaboradores/services/vinculo.py:250`, `:266`, `:274`). Os leitores novos obtêm o município pelo objeto atual desse posto (`app/core/feriados.py:121`, `:122`, `:151`). O próprio fluxo depois solicita regeneração retroativa, portanto essa escrita isoladamente não demonstra violação da exceção de cadastro da L-020 (`app/colaboradores/services/vinculo.py:306`; `app/docs/LEIS.md:71`). Efeito indevido concreto em produção [nao verificado].

## 05/11 15:25 · sessao b9bdbaa56058 · - · - · origem=Ronald (tmux)

Extraí apenas textos normativos dos documentos; não consultei os `.py` proibidos. [nao verificado]

| Tema | Regra escrita e referência |
|---|---|
| Mesmo número em todos os leitores | **L-002:** tela, PDF, TXT, badge e contador leem a mesma autoridade; derivação própria é proibida. `app/docs/LEIS.md:60` |
| Fonte do previsto | **L-024:** a grade é fonte única do previsto. `app/docs/LEIS.md:75` |
| Dinheiro e leitores | **L-095:** normais, HE, noturno, intra, pontualidade, folga trabalhada e falta são lavrados uma vez; tela, PDF, cartão, fechamento, TXT, calendário e app leem a lavratura. `app/docs/LEIS.md:113` |
| Célula, chamado e pergunta | **L-061:** chamado, pergunta, tela e folha leem célula/ata, sem novo julgamento. `app/docs/LEIS.md:95` |
| DiaPago | O glossário classifica **espelho, DiaPago, fechamento, retrato e pílulas** como projeções reconstruíveis/CQRS; combinado com L-002/L-095, DiaPago deve conservar o mesmo resultado monetário da autoridade, sem derivação paralela. `app/docs/GLOSSARIO.md:22`; `app/docs/LEIS.md:60`; `app/docs/LEIS.md:113` |
| Texto no espelho/cartão | Dia diferente de “trabalhou” leva a palavra do veredito na grade/PDF; falta nunca fica vazia. `app/docs/CORTES.md:198` |
| Marca do ato humano | Decisão no Resolver dia mostra marca com quem, quando e o quê, lida da ata. `app/docs/CORTES.md:194` |
| Falta decidida versus furo | Falta = ausência cujo tipo desconta ou veredito “falta” na célula; furo sem decisão aparece como “dias em aberto (a decidir)”, com número, sem sumir nem virar falta sozinho. `app/docs/CORTES.md:179`; **L-039**, `app/docs/LEIS.md:87` |
| Custo da falta | **L-088:** falta é dia previsto sem trabalho e sem justificativa e custa o previsto daquele dia; furo é marco não cumprido/pergunta aberta. `app/docs/LEIS.md:120` |
| Folga e atestado | **L-089:** ausência que abona/desconta só alcança dia previsto de trabalho; sobre folga não gera efeito nem consome saldo. `app/docs/LEIS.md:121` |
| Parcial com período | Período é cadastro `hora_ini/hora_fim`, declarado pelo admin; sem período, vai à lista de CADASTRO como “parcial sem período”, sem inferir ponta nem emitir `FATO_EM_AUSENCIA`. `app/docs/CORTES.md:319` |
| Batida durante parcial | `FATO_EM_AUSENCIA` só quando a batida está **dentro** do período declarado; justificativa cobre só tipo e período. `app/docs/CORTES.md:315` |
| Pagamento de parcial | Texto explícito: “trabalhou paga realizado + abonado do período”; cobertura e abono usam o mesmo juiz. `app/docs/CORTES.md:312` |
| Não converter parcial em falta inteira | O corte proíbe trocar abono integral por falta inteira com DSR perdido sem pagar o período; depois determina que justificativa aprovada vire efeito parcial pela mesma porta da ausência. `app/docs/CORTES.md:330`; `app/docs/CORTES.md:336` |
| Parcial noturna | `hora_fim < hora_ini` cruza meia-noite e pertence ao dia de início do turno. `app/docs/CORTES.md:330`; **L-085**, `app/docs/LEIS.md:117` |
| Justificativa aprovada e chamado | Aprovar **Justificativa** encerra o chamado de origem pela porta do chamado na mesma transação; esse texto fala especificamente da entidade Justificativa, não autoriza presumir que todo registro de Ausência tem essa mesma transição nominal. `app/docs/CORTES.md:362` |
| Cobrança sobre folga | Cobrança cala quando célula lavrada declara dia sem trabalho; todos os emissores devem perguntar à mesma autoridade. `app/docs/CORTES.md:416` |

Propriedades fixas que podem ser citadas diretamente:

- **Partes somam total, idempotência e todos os leitores dão o mesmo número**, para qualquer dado limpo ou sujo: **L-114**, `app/docs/LEIS.md:148`.
- **Julgar 2× = julgar 1×**, **previsto não depende do realizado**, jornada pertence ao dia de início e partes somam total: `app/docs/CORTES.md:333`.
- **Determinismo:** mesmas entradas dão o mesmo veredito; nenhum estado muda sem trilha: `app/docs/CORTES.md:333`; **L-007**, `app/docs/LEIS.md:65`.
- **Totais de período:** arredondar uma vez na fonte; rodapé e topo somam minutos inteiros das linhas impressas, formatados no fim; sem tolerância ou ajuste no rodapé: `app/docs/CORTES.md:210`.
- **Pontualidade:** atraso + saída antecipada ≤ previsto − trabalhado; previsto zero implica teto zero: **L-093**, `app/docs/LEIS.md:111`.
- **Sem duplicar abono, trabalho e falta na mesma partição** é aplicação aritmética de “partes somam total”, não uma fórmula explícita encontrada nos documentos; convém apresentar a fórmula como decomposição dos casos e citar `app/docs/CORTES.md:333`, sem atribuir fórmula nova à CLT. [nao verificado]

Lacunas para marcar **“lei:”**:

- Não encontrei nos textos lidos regra explícita de **precedência/transição entre falta já lançada e atestado posterior**, nem se o registro antigo deve ser rejeitado, substituído ou cancelado; o resultado deve respeitar justificativa e ausência sem duplicação, mas a operação nominal exige regra. [nao verificado]
- Não há nestes textos um esquema normativo completo dos campos do **DiaPago**; pode exigir equivalência de rubricas com a lavratura, sem inventar nomes de campo. `app/docs/GLOSSARIO.md:22`; `app/docs/LEIS.md:113`.
- Não extrapolar “aprovar Justificativa encerra chamado de origem” para toda **Ausência/atestado** sem regra específica. `app/docs/CORTES.md:362`.
- Não somar **noturno** outra vez ao total de minutos físicos: L-095 o enumera como rubrica, mas a fórmula completa de particionamento é matéria dos casos/CLT/CCT, não destes textos. `app/docs/LEIS.md:113`; [nao verificado]

## 05/11 15:25 · sessao 46da3fd09e0b · - · - · origem=Ronald (tmux)

Regras extraídas apenas dos textos normativos:

| Tema | Regra escrita | Referência |
|---|---|---|
| Gabarito | A referência é REGRA aplicada ao cadastro e às batidas; implementação existente não decide a resposta. | `app/docs/LEIS.md:136`; `app/docs/CORTES.md:240` |
| Falta inteira | Falta é dia previsto sem trabalho e sem justificativa; custa os minutos previstos daquele dia. | L-088, `app/docs/LEIS.md:120` |
| Falta decidida × furo | Falta exige decisão; marco ausente sem decisão permanece “em aberto (a decidir)”, nunca vira falta sozinho. | L-039, `app/docs/LEIS.md:87`; `app/docs/CORTES.md:179` |
| Falta parcial por saída antecipada | Desconta em minutos, e a palavra deve mostrar “Saída antecipada (desconta Xh)”. | `app/docs/CORTES.md:202` |
| Teto de pontualidade | Atraso + saída antecipada ≤ previsto − trabalhado; previsto zero implica desconto zero. | L-093, `app/docs/LEIS.md:111` |
| Abono e trabalho | Abono cobre somente o previsto não coberto pelas batidas; não duplicar minutos realizados e abonados. | `app/docs/CORTES.md:88`; confirmação em `app/docs/CORTES.md:277` |
| Justificativa parcial | Cobre somente tipo e período; paga realizado + abonado do período. | `app/docs/CORTES.md:312`; `app/docs/CORTES.md:315` |
| Período parcial | `hora_ini/hora_fim` devem ser declarados pelo admin; sem período vai à lista CADASTRO “parcial sem período”; não inferir a ponta pelo tipo, nem acusar FATO_EM_AUSENCIA. | `app/docs/CORTES.md:319` |
| Batida em ausência parcial | FATO_EM_AUSENCIA somente com batida dentro do período declarado. | `app/docs/CORTES.md:315` |
| Parcial noturna | Fim menor que início significa período atravessando meia-noite, pertencente ao dia inicial do turno. | `app/docs/CORTES.md:330` |
| Atendido o período | Não transformar abono parcial em falta inteira/DSR perdido; período pago deve chegar ao cálculo pela mesma fonte dos leitores. | `app/docs/CORTES.md:330`; `app/docs/CORTES.md:336`; `app/docs/CORTES.md:353` |
| Atestado em folga | Ausência que abona/desconta só alcança dia previsto de trabalho; folga não gera efeito nem consome saldo. | L-089, `app/docs/LEIS.md:121` |
| Dia inicial | Jornada noturna pertence integralmente ao dia em que começou; não cria trabalho na folga seguinte. | L-085, `app/docs/LEIS.md:117` |
| Intervalo | Intervalo cadastrado sempre sai da jornada; sem batida, indenização depende do cadastro e não gera HE. | L-031/L-032, `app/docs/LEIS.md:79`; `app/docs/LEIS.md:80` |
| HE | Nenhuma HE nasce automaticamente; pontas externas ficam pendentes e não compensam faltas dentro da jornada. | L-097, `app/docs/LEIS.md:115` |
| Noturno 12x36 Londrina | Hora normal de 60 minutos e adicional de 20% devido. | L-036, `app/docs/LEIS.md:84` |
| Prorrogação | Para CCT, pós-05h depende do cadastro da convenção, não apenas de dizer “12x36 noturno”. | L-035, `app/docs/LEIS.md:83` |
| Leitores | Espelho, app, DiaPago, PDF, folha, chamado e pergunta devem ler a mesma lavratura/célula; não recalculam por conta própria. | L-002/L-061/L-095, `app/docs/LEIS.md:60`; `app/docs/LEIS.md:95`; `app/docs/LEIS.md:113` |
| Rótulo | Todo dia diferente de “trabalhou” leva a palavra do veredito na grade e no PDF; falta não fica vazia. | `app/docs/CORTES.md:198` |
| Propriedades | Partes somam total; julgar duas vezes equivale a uma; previsto independe do realizado; leitores dão o mesmo número. | `app/docs/CORTES.md:240`; `app/docs/CORTES.md:333`; L-114, `app/docs/LEIS.md:148` |
| Trilha | Ato crítico registra usuário, motivo, antes/depois; um escritor por estado. | L-007, `app/docs/LEIS.md:65` |

**Lacunas que precisam de “lei:” no gabarito:**

- **lei: atestado após falta lançada.** Os textos examinados não estabelecem expressamente precedência automática, cancelamento ou substituição entre os dois registros aprovados. A regra de sobreposição exige mostrar número, tipo, período inteiro, status, documento e ações possíveis; não decide sozinha qual efeito vence. `app/docs/CORTES.md:338`. A necessidade de resolver a colisão sem dupla contagem decorre das propriedades, mas o procedimento/vencedor não foi escrito nesses trechos. `[nao verificado]`
- **lei: noturno abonado em atestado.** L-036 garante adicional e define unidade de 60 minutos; não diz expressamente se campo “noturno” deve conter somente trabalho noturno efetivo ou também horas noturnas abonadas. `app/docs/LEIS.md:84`; `[nao verificado]`
- **lei: atestado de horas sem período declarado.** Pode-se afirmar CADASTRO e proibir inferência da ponta; não há número fechado de falta/abono até resolver o período. `app/docs/CORTES.md:319`.
- **lei: status exato do chamado após falta/atestado.** A norma exige refletir a célula e não cobrar fato já coberto, mas os trechos examinados não prescrevem um único nome de status para cada um dos seis cenários de Q1. L-061, `app/docs/LEIS.md:95`; `[nao verificado]`

Cuidado para não usar “12x36 = previsto 720”: o intervalo cadastrado sai da jornada, portanto 07–19 ou 19–07 com 1h de intervalo corresponde a 660 minutos previstos de trabalho. L-032, `app/docs/LEIS.md:80`.

## 05/11 15:25 · sessao 6956be04fe0c · - · - · origem=Ronald (tmux)

Contraexemplo de lote confirmado no pareador real extraído por AST em memória: a janela ampla inclui uma entrada antiga cujo `criado_em` interrompe a sequência usada para reconhecer eco de flush; o dia 04/09/2026 passa de nenhum turno próprio para turno fechado 08:00–17:00. A causa está em `app/ponto/turnos.py:353–372`; o diff amplia a janela em `app/colaboradores/services/calendario.py:126–135`.

Com cadastro sem vínculo, células sem DNA/ata/status e `trabalha=False` em 04/09, a leitura individual resulta `folga`, enquanto a ampla resulta `sem_certidao`, pelo D4 (`app/ponto/status_dia.py:273`, `app/ponto/status_dia.py:292`, `app/ponto/status_dia.py:312`). Isso muda `resolver_dia_oferecido` de falso para verdadeiro (`app/colaboradores/services/calendario.py:103`), respeitadas as demais condições de último recurso (`app/colaboradores/services/calendario.py:738`). Integração completa com o universo real do cron e ocorrência em produção: [nao verificado].

O script exato e o mapa escritor/leitor foram enviados por mensagem. O teste novo de paridade mede uma fixture sem essa sequência adversarial (`app/chamados/tests/test_sombra_c1_resolver_em_lote.py:71`); sozinho, passaria com implementação plural por dia, revertendo a otimização e preservando a API. Os testes de quantidade de calendários efetivamente mordem essa reversão (`app/chamados/tests/test_sombra_c1_resolver_em_lote.py:41`, `app/chamados/tests/test_sombra_c1_resolver_em_lote.py:64`).

Cadastro atual alcança o passado por posto/município (`app/core/feriados.py:113`) e por empresa/janela usada para habilitar último recurso (`app/colaboradores/services/calendario.py:463`). A seleção inicial de vínculo ativo possui complemento pela geradora; não é suficiente, isoladamente, para provar erro temporal (`app/ponto/precedencia.py:327`).

## 05/11 15:25 · sessao 491dab573827 · - · - · origem=Ronald (tmux)

Mapa do item 5, com os limites de prova:

| Campo | Escritor concreto | Leitor que decide passado |
|---|---|---|
| `situacao` | Desligamento grava `'desligado'`; reativação grava `'ativo'`: `app/colaboradores/services/desligamento.py:104`, `:210`; persistência em `app/colaboradores/services/colaborador.py:65`. | `classificar_export` usa `colaborador__situacao='ativo'` para a trava de âncora da competência: `app/folha/export.py:791`. O cartório também condiciona emissão de furo passado à situação atual: `app/ponto/services/cartorio.py:1696`, `:1762`. |
| `data_demissao` | Agendamento futuro grava a data sem desligar: `app/colaboradores/services/desligamento.py:82`, `:85`; desligamento grava data: `:105`; reativação apaga: `:211`. | Qualquer data não nula exclui do TXT da competência consultada, sem comparar com sua janela: `app/folha/export.py:840`. Outros leitores comparam temporalmente: `:1061`, `:1182`. |
| `empresa` | Transferência grava a empresa do novo posto: `app/colaboradores/views.py:1386`, persistindo em `:1388`; edição também atribui empresa: `:1313`. | Universo do fechamento passado filtra a empresa atual do colaborador: `app/folha/export.py:767`; a janela usa `colab.empresa`: `:326`, `:1055`. Regra dos anexos exigindo empresa histórica para essa seleção: [não verificado]. |
| `regime` | Formulário lê `regime_trabalhista` e o envia à porta: `app/colaboradores/views.py:512`, `:523`; gravação: `app/colaboradores/services/estrutura.py:17`. | Caminho comprovado: `classificar_export` → homologação em `app/folha/export.py:948` → `feriado_12x36_em_dobra` em `:978` → `regua_para(colab, fim_da_competencia)` em `:359`, `:369`. A régua lê empresa e regime atuais: `app/core/regua_cct.py:352`. Os anexos designam essa autoridade, sem fixar vigência histórica do campo regime: `app/docs/JUIZES.md:30`. |
| `isento_ponto` | Passe livre envia o novo booleano à porta: `app/colaboradores/views.py:173`, `:176`; persistência: `app/colaboradores/services/colaborador.py:47`. | O juiz lê somente o booleano atual, sem data: `app/core/constants.py:13`. Isso dispensa retenção de dias passados: `app/folha/export.py:1174`; altera prontidão da competência: `:1011`; e suprime eventos de um fechamento já apurado: `:407`. No cartório o booleano atual entra no julgamento do dia passado: `app/ponto/services/cartorio.py:1419`. |
| vínculo atual | Criação com `ativa=True`: `app/colaboradores/services/vinculo.py:278`; reutilização reativa e apaga fim: `:267`; encerramento grava `ativa=False/data_fim`: `:615`. | A trava de aprovação da competência consulta `ativa=True`, sem janela: `app/folha/export.py:787`. Em contraste, o ciclo e a praça da régua consultam o vínculo da data da competência: `app/folha/export.py:348`, `app/core/regua_cct.py:329`. A autoridade temporal consta expressamente no anexo: `app/docs/JUIZES.md:98`. |
| `posto` | Transferência altera posto atual: `app/colaboradores/views.py:1385`; o executor copia esse posto ao vínculo criado: `app/colaboradores/services/vinculo.py:280`. | No caminho comprovado da régua, a praça vem do posto do **vínculo da competência**, não de `colab.posto`: `app/core/regua_cct.py:329`, `:330`. Leitor do posto atual decidindo passado neste caminho: [não verificado]. Existe leitor atual em `app/core/feriados.py:113`, mas não fechei sua ligação transitiva ao percurso auditado: [não verificado]. |

O caminho até a competência passada é concreto: sombra executa `crons sombra` e cada comando listado (`bin/sombra.sh:346`, `:381`); `bloco_da_manha` inclui encadeados (`app/config/crons.py:1874`); `lavrar_retrato_folha --apply` está declarado (`:1594`); o lavrador consulta também `janela_anterior` (`app/folha/management/commands/lavrar_retrato_folha.py:60`), chama `montar_linhas` (`:65`), que chama `fora_da_folha` (`app/folha/retrato_folha.py:1157`), chegando a `classificar_export` (`app/folha/fora_da_folha.py:119`).

Contraexemplo sintético: em 05/11/2026, colaborador ativo, com demissão agendada para 10/11/2026 e fechamento de outubro/2026. O agendamento mantém o colaborador ativo e grava a data futura (`app/colaboradores/services/desligamento.py:77`, `:85`), mas o juiz exclui seu fechamento de outubro apenas porque a data está preenchida (`app/folha/export.py:840`). O comentário local restringe a exclusão à demissão **na janela** (`:834`); essa restrição temporal não aparece nas LEIS/CORTES anexadas: [não verificado]. Ocorrência em cadastro real: [não verificado].

Contraexemplo sintético do vínculo: outubro/2026 possui vínculo 12x36 sem âncora, encerrado em 20/10; desde 21/10 há vínculo 6x1 ativo. A trava de outubro ignora o 12x36 encerrado e não devolve `sem_ancora_12x36`, porque consulta somente `ativa=True` (`app/folha/export.py:787`, `:813`). O anexo manda consultar o vínculo do dia (`app/docs/JUIZES.md:98`); seu efeito específico sobre a regra de aprovação por âncora não está explicitado ali: [não verificado].

Esses leitores existem fora do diff informado; não há base para atribuir sua introdução ao CRONS-INSTALL. [não verificado]

_1624 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
