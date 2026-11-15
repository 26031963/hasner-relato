# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 11:34** · publicado em **15/11/2026 11:34** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 11:34 · sessao 6ad4362f604d · - · - · origem=Ronald (tmux)

- A assinatura é `dono_da_divergencia(horas_do_dia, em_cadastro_x_realidade, folga_nao_definida=False, sem_vinculo_no_dia=False, sem_vinculo_em_dia_de_trabalho=False, batida_fora_de_turno=False, fase_deslocada=False)`. Não recebe colaborador; julga **um dia-colab**, retornando uma string. `app/ponto/management/commands/e6_oraculo.py:53-55`.

- A precedência efetiva é: folga indefinida/sem vínculo → CADASTRO; batida fora de turno → BATIDA; quantidade ímpar → BATIDA; fase deslocada → CADASTRO; pertencimento à lista Cadastro × Realidade → CADASTRO; restante → ESTRUTURA. Essa precedência resolve condições **do mesmo dia**, não conflitos entre dias de um colaborador. `app/ponto/management/commands/e6_oraculo.py:98-110`.

- O leitor existente para montar os insumos é `InsumosDoDono(colaborador, ini, fim, hoje, espelho=None, *, agora, dias_de_fase=None, cadastro=None, sem_motor=False, cxr=None)`; sua API de julgamento é `dono(data)`. Inicializa batidas/oráculo; carrega turnos e fase sob demanda; aceita espelho já obtido para evitar outra chamada ao motor. `app/ponto/management/commands/e6_oraculo.py:128-195`.

- **O E6 não atribui um dono único ao colaborador.** Acrescenta `c.pk` em conjuntos independentes por dono, portanto o mesmo colaborador pode estar em vários conjuntos. Imprime as contagens por dono e a união separadamente. `app/ponto/management/commands/e6_oraculo.py:378-381`, `app/ponto/management/commands/e6_oraculo.py:429-441`. Outro consumidor agrupa pautas por `(dono, colaborador, competência)`, igualmente permitindo mais de um dono por colaborador. `app/ponto/services/pautas_dos_discordantes.py:88-97`, `app/ponto/services/pautas_dos_discordantes.py:134-151`.

- Assim, usar diretamente esses conjuntos para `ESTRUTURA a / CADASTRO b / BATIDA c` não produz partição exclusiva de colaboradores. Uma precedência mensal entre donos, ou eleição de um dia representante, precisaria de regra explícita; regra existente para isso [não verificado]. O próprio E6 soma donos em **dia-colab**, enquanto calcula colaboradores divergentes por união. `app/ponto/management/commands/e6_oraculo.py:425-441`.

- Além disso, o universo original do comando E6 vem de `FechamentoMensal`, por empresa ativa; não aplica ali filtro `Colaborador.situacao='ativo'`. A opção `--so-txt` restringe aos classificados como `entra`; isso não é uma medição de todos os ativos com trabalho. `app/ponto/management/commands/e6_oraculo.py:303-314`.

- Custo: construir `InsumosDoDono` consulta batidas e deriva o oráculo; `dono(data)` usa turnos/fase cacheados, mas pode consultar cadastro por dia e verificar batidas soltas. Sem espelho fornecido, pode pedir `espelho_do_colab` ao resolver Cadastro × Realidade. Reutilizar a instância por colaborador/janela e o espelho existente evita repetição; tempo em produção [não verificado]. `app/ponto/management/commands/e6_oraculo.py:149-195`, `app/ponto/management/commands/e6_oraculo.py:198-212`, `app/ponto/management/commands/e6_oraculo.py:328`.

## 15/11 11:30 · sessao c7ffade3ce53 · - · - · origem=Ronald (tmux)

Classificação das dez falhas herdadas, sem editar arquivos:

| Falhas | Diagnóstico | Cura estreita |
|---|---|---|
| C8b ausência, memo e dois ranges: 4 | Fixture ainda representa fechamento somente por `materializada_em`; E4c exige ato aplicável. A expectativa sobre ausência permanece correta. | Acrescentar `SILENCIO` em `ato_em=AGORA` à fixture materializada e fornecer `perguntas=ps` ao acervo. Manter as assertivas. |
| C8b duas testemunhas: 2 | Fixture conserva texto, mas remove `respondida_em` e não fornece `RESPOSTA`; a leitura retorna texto vazio e o slot desaparece. | Fornecer DP `RESPOSTA` com texto e instante fixos; usar célula `concorde/admin` com marco ainda pendente para chegar ao ramo E. Manter os cinco resultados esperados. |
| C8b slot noturno: 1 | Mesma ausência de DP `RESPOSTA`. | Acrescentar ato `RESPOSTA '06:00'`; preservar assertiva do slot e testemunha noturna. |
| C8c via futura: 1 | Candidato a defeito F1: a projeção temporal recorta R/V, mas entrega via crua sem ato aplicável. | Recortar vias não A06 pela testemunha temporal. |
| E4b via futura: 1 | Mesmo problema da via não A06. | Mesma cura produtiva. |
| Pós-suite E=True: 1 | Assertiva antiga usa C futuro para invalidar testemunha E que o próprio teste fixa como verdadeira em qualquer instante. | Pela E4c, manter E como fonte e esperar pergunta morta nos dois instantes; acrescentar caso separado de testemunha futura corretamente recortada. | [nao verificado]

Evidências:

- Fixtures C8b: `chamados/tests/e3_c8b_apoio.py:215`; ausência/memo/ranges: `chamados/tests/test_e3_c8b_sem_banco.py:287`, `:301`, `:337`.
- Texto sem resposta testemunhada: `chamados/tests/test_e3_c8b_sem_banco.py:368`, `:371`, `:390`; consumo do texto lido: `chamados/juizes.py:422`, `:437`, `:451`.
- Fechamento automático exige terminal: `chamados/juizes.py:755`.
- Via crua na projeção: `chamados/juizes.py:1454`; expectativas C8c/E4b: `chamados/tests/test_e3_c8c_sem_banco.py:134`, `chamados/tests/test_e4b_classe_e_memoria.py:387`.
- E=True independente de instante: `chamados/tests/test_e3_pos_suite_sem_banco.py:97`; assertiva antiga: `:98`.

Experimentos exclusivamente em memória confirmaram:

- DP `SILENCIO`: vida=False com ausência tanto com carga quanto avulso; retirar ausência da carga retorna vida=True.
- DP `RESPOSTA` + célula `concorde/admin`: os cinco casos de testemunhas devolvem exatamente `False/True/True/False/False`, com zero erros registrados.

Comando dos experimentos: `PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY' ... PY`. Saída não foi salva em arquivo; certificação independente [nao verificado].

## 15/11 11:23 · sessao 05d4cfe8fe27 · E4C-F1V2 · construtor · origem=Code

Recortes AST expandidos para executar a leitura real da F1, com ordenação `(ato_em, pk)`. Produção intocada; asserções anteriores preservadas. [Auditoria](chamados/tests/e4c_f1v2_resultados.txt:646)

Resultados locais: oito regressões novas passam, C7 passa 4/4 e 15 mutantes reconstruídos mordem. A bateria permanece **RED, com 11 falhas de asserção**; o selo zero continua acusando duas leituras residuais. [Resultados](chamados/tests/e4c_f1v2_entrega.txt:107)

O catálogo original `e4c_f1_mutantes.txt` está ausente da base; sua reexecução fica **[nao verificado]**. [Limitação](chamados/tests/e4c_f1v2_entrega.txt:156)

[Entrega completa: arquivos, antes/depois, censo, comandos e LEI-AKITA](chamados/tests/e4c_f1v2_entrega.txt:1). A reprodução confirmou NameError em C7/E4b; no terceiro módulo, confirmou a falha do selo zero. [RED reproduzível](chamados/tests/e4c_f1v2_entrega.txt:145)

## 15/11 10:45 · sessao 45ccd1a14409 · COMPETENCIA-PAGA-UM-JUIZ · verificador · origem=Code

**Não daria aval ao diff:** a passagem REGEN é forjável, há escritores fora da guarda e o cadastro pode confirmar sem trilha. (`app/ponto/services/fechamento.py:48`, `app/ponto/services/fechamento.py:1415`, `app/core/observ.py:133`.)

Identidade da cópia com o hash/base, execução da suíte e estado efetivo de produção: **[nao verificado]**. [nao verificado]

1. **Quem escreve e quem lê**

   **Cadastro novo:**

   | Campo | Escritor | Leitores |
   |---|---|---|
   | `id` | ORM na criação | Foto da trilha, identificação do log e retorno. (`app/folha/services/competencia_paga.py:59`, `:77`, `:79`, `:83`.) |
   | `empresa`, `mes`, `ano` | `marcar_paga_fora` | Busca da marcação viva, K10, enumeração das competências e histórico. (`app/folha/services/competencia_paga.py:74`, `:78`, `:93`; `app/folha/export.py:624`; `app/ponto/management/commands/gerar_celulas.py:175`; `app/templates/folha/competencias_pagas.html:50`.) |
   | `marcada_por`, `marcada_em`, `motivo` | `marcar_paga_fora`, recebendo autor/instante/motivo | `_foto`, ordenação e histórico apresentado. (`app/folha/services/competencia_paga.py:60`, `:78`; `app/folha/views_competencia_paga.py:72`; `app/templates/folha/competencias_pagas.html:52`.) |
   | `desmarcada_por`, `desmarcada_em`, `desmarcada_motivo` | Inicialmente nulos/vazio; depois `desmarcar_paga_fora` | `_foto`, filtro de linha viva, K10, histórico e `__str__`. (`app/folha/models.py:394`; `app/folha/services/competencia_paga.py:62`, `:97`; `app/folha/export.py:624`; `app/templates/folha/competencias_pagas.html:55`; `app/folha/models.py:409`.) |

   **LogAuditoria:** marcar/desmarcar fornecem ação, modelo, id, descrição e antes/depois; `_foto` inclui todos os campos do cadastro. O escritor final acrescenta usuário/IP/ator e grava o log; `criado_em` nasce pelo relógio do modelo. (`app/folha/services/competencia_paga.py:56`, `:79`, `:99`; `app/core/observ.py:108`, `:134`; `app/core/models.py:307`.)

   As ações de marcação não têm leitor específico demonstrado; o leitor genérico é a auditoria. A “trilha” da tela nova lê **o cadastro**, não LogAuditoria. O bloqueio do recálculo/retrato grava outra ação, contada por prefixo de descrição, sem separar empresa. (`app/core/views.py:705`; `app/folha/views_competencia_paga.py:69`; `app/ponto/services/fechamento.py:591`, `:58`; `app/ponto/services/dia_pago.py:456`.)

   **FechamentoMensal/DiaPago:** o recálculo cria/salva/atualiza FM e chama a lavratura DP; o retrato lê FM e altera/cria DP. Exportação lê FM; comparadores e testemunhas leem DP. Os caminhos ainda capazes de escrever em competência paga estão no item 3. (`app/ponto/services/fechamento.py:921`, `:1167`, `:1183`, `:1224`; `app/ponto/services/dia_pago.py:462`, `:539`; `app/folha/export.py:760`; `app/folha/porta_export.py:284`.)

   **Retorno do juiz:** bool vai à guarda e ao wrapper do dia; fontes vão ao K9, tela e diagnóstico “sem TXT”; registro vai ao comparador e vigia. `registro=True` retorna exclusivamente o K6, podendo ser `None` com K10 verdadeiro. (`app/ponto/services/fechamento.py:1558`; `app/chamados/services/vigia_fio.py:883`; `app/folha/export.py:656`, `:706`, `:693`, `:617`; `app/folha/views_competencia_paga.py:65`; `app/folha/services/vigia_txt.py:224`.)

   **Leitores fora do diff:** para marcação externa viva, sem TXT nem holerite, K10 responde verdadeiro e K6 falso. A diferença abaixo é demonstrável; leitores que perguntam especificamente pelo arquivo continuam fazendo outra pergunta. (`app/folha/export.py:591`, `:623`, `:659`.)

   | Leitor fora do diff | Pergunta/efeito na competência paga fora |
   |---|---|
   | `app/folha/views.py:354` | “Já exportou?” Não mostra aviso de arquivo; diferença coerente com a pergunta TXT. |
   | `app/ponto/services/fechamento.py:1580` | `exportada=False`; `paga` depende de holerite, podendo também ser falsa. |
   | `app/folha/services/ranking_he.py:89` | Todas as empresas entregaram? Marcação externa não completa a condição. |
   | `app/ponto/services/esmeril_espelho.py:58` | Até onde retroceder antes da primeira entregue? Marcação externa não interrompe. |
   | `app/ponto/services/he_pendente_lavrado.py:223` | Manter competência anterior ainda não entregue? Continua incluindo-a. |
   | `app/chamados/services/cobranca_folha.py:68` | Dia ainda pode mudar? Marcação externa continua admitida na cobrança. |
   | `app/ponto/services/autorizacao_he_periodo.py:118`, `:299` | Criar Pauta DP por alteração em exportada? Marcação externa não satisfaz a condição. |
   | `app/folha/porta_export.py:341` | Pelo apelido `_exp_no_escopo`, escolhe campos do contador DP; marcação externa segue no tratamento de não exportada. |
   | `app/chamados/models.py:364` | Competência aberta para proteção da pergunta viva? Continua aberta pelo K6. |
   | `app/chamados/services/fio.py:815` | Costura em competência aberta? Continua usando entrega, embora o veto do mesmo arquivo tenha migrado. |
   | `app/chamados/reconciliador.py:745` | Entregue fica fora do alcance da cura? Marcação externa não fica. |
   | `app/chamados/services/disputa_emissao.py:1858` | Entrega impede reabertura do credor? Marcação externa não impede por essa condição. |
   | `app/chamados/services/fabrica_da_celula.py:231` | Limbo entregue vai para Pauta DP? Marcação externa não vai por esse predicado. |
   | `app/chamados/services/tarefa_do_dia.py:463` | Quais triplas estão entregues? Exclui marcação sem TXT. |
   | `app/chamados/services/vigia_fio.py:837` | Dono “exportada” ou “aberta”? Marca externa fica “aberta”. |
   | `app/chamados/services/foto_e4b.py:238`, `:245` | Foto de entrega por dia/competência; continua falsa. |
   | `app/chamados/management/commands/backfill_fio_morto_e4b.py:173`, `:256`, `:304` | Plano e rechecagem de entrega continuam falsos. |
   | `app/chamados/management/commands/backfill_decisoes_pergunta.py:471`, `:498` | Relatório conta entrega; pode discordar do veto migrado usado pelo apply. |
   | `app/chamados/management/commands/reabrir_orfas_enterradas.py:95` | Exportada exclui reabertura; externa sem TXT não exclui. |
   | `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:115` | Entrega recusa rederivação; externa sem TXT não recusa. |
   | `app/ponto/management/commands/lavrar_status_competencia.py:43` | Apenas informa entrega; informa falsa. |
   | `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:124` | Universo da pauta pago/devido exige entrega. |
   | `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:93` | Universo da pauta de folga exige entrega. |
   | `app/folha/management/commands/pre_fechamento.py:74` | Lê os marcos antigos, inclusive “paga” por holerite. |
   | `app/chamados/services/passivo_validado.py:127`, `:435` | Seu próprio “exportada” decide REGEN/Pauta; externa sem TXT não entra. |

   Acompanham a marcação externa **por composição com K9**, sem novo predicado: portas de célula, vigência impossível, correção retroativa e apply col900. (`app/ponto/portas/celula.py:221`, `:607`, `:821`; `app/escala/services/vigencia_impossivel.py:26`; `app/colaboradores/management/commands/corrigir_escala_retroativa.py:60`; `app/ponto/management/commands/aplicar_col900.py:64`.)

2. **Segundo juiz**

   **Sim, para K6:** `_esta_exportada` consulta `haiku_export.exportacoes_da_competencia()['vigente']`, sem chamar K6. O haiku escolhe `e_vigente`, que identifica o último registro; K6 ainda exige `invalidada_em is None`. Assim, podem discordar numa revogada sem substituta. (`app/chamados/services/passivo_validado.py:116`; `app/folha/haiku_export.py:69`; `app/folha/models.py:134`; `app/folha/export.py:591`.)

   Há também derivação paralela chamada **“paga”**, baseada em holerite publicado. Não equivale ao novo pagamento e alimenta `marco`. (`app/ponto/services/fechamento.py:1582`, `:1586`.)

   Selecionar o último TXT para memória/reversão continua existindo no regerador; esse uso não prova sozinho outro juiz de pagamento. (`app/folha/management/commands/regerar_txt_dominio.py:69`.)

   Ausência absoluta de outras cópias do OR K10: **[nao verificado]**. Os apelidos encontrados acima não são, por si, derivadores: `_exp_no_escopo` continua chamando o leitor K6. (`app/folha/porta_export.py:341`.)

3. **Segundo escritor e passagem REGEN**

   Não identifiquei segundo escritor efetivo do cadastro; contudo, o modelo não protege `save/delete`, e a catraca não cobre aliases nem `bin/`. Portanto, exclusividade garantida: **[nao verificado]**. (`app/folha/models.py:373`; `app/folha/tests/test_competencia_paga_um_juiz.py:347`, `:363`.)

   Há caminhos concretos de escrita fora da guarda:

   - **FM financeiro:** `restaurar_fechamento` atualiza campos pedidos apenas exigindo motivo, sem K10/passagem; é chamado por applies e reversões. (`app/ponto/services/fechamento.py:1401`, `:1415`; `app/ponto/management/commands/aplicar_09_he_intra.py:149`; `app/ponto/management/commands/aplicar_09_corte_b.py:220`, `:266`.)
   - **DP financeiro:** `lavrar` é pública, sem guarda própria, e apaga/recria as linhas. (`app/ponto/services/dia_pago.py:135`, `:359`.)
   - **DP sem alterar rubricas:** backfill escreve `furo_apurado/lavrado_em` sem K10. (`app/ponto/services/dia_pago.py:646`, `:672`.)
   - **FM administrativo:** aprovar/reabrir e invalidar carimbos continuam escrevendo sem essa guarda. (`app/ponto/services/fechamento.py:1360`, `:1369`, `:1472`, `:1656`, `:1699`.)
   - **Scripts:** restaurações salvam/apagam FM e recriam DP diretamente. (`bin/restore_relavratura_10_2026.py:69`; `bin/restore_o263_comp10.py:43`; `bin/restore_badge_linha_09_10.py:61`.)
   - **Recálculo geral de empresa inativa:** a guarda enumera somente empresas ativas; o universo de colaboradores não aplica esse filtro. Pode alcançar competência paga de empresa inativa. (`app/ponto/services/fechamento.py:1533`, `:447`, `:450`, `:576`.)

   O command de lavratura e a regeneração de vínculo normalmente chegam às guardas; o command ainda roteia por TXT, enquanto a célula recebe K9. (`app/ponto/management/commands/lavrar_dias_pagos.py:104`, `:123`; `app/ponto/portas/celula.py:221`.)

   **Passagem forjável e reutilizável:** qualquer chamador pode construir `PassagemRegen(object(), 'x')`. Não há vínculo com colaborador/empresa/competência, staff, uso único ou contexto da porta; os atributos são mutáveis e as guardas verificam apenas o tipo. O próprio teste fabrica a passagem fora da porta. (`app/ponto/services/fechamento.py:46`, `:48`, `:578`; `app/ponto/services/dia_pago.py:451`; `app/folha/tests/test_competencia_paga_um_juiz.py:280`.)

4. **Relógio**

   A view nova lê o relógio na borda, e o cadastro recebe `agora`. Porém a auditoria lê outro instante implicitamente por `auto_now_add`. (`app/folha/views_competencia_paga.py:46`, `:57`; `app/folha/services/competencia_paga.py:78`, `:97`; `app/core/models.py:307`.)

   Funções tocadas continuam lendo relógio fora da borda: recálculo e retrato. `hoje` da REGEN não é transmitido a elas. (`app/ponto/services/fechamento.py:639`, `:1109`, `:1166`, `:1212`; `app/ponto/services/dia_pago.py:534`; `app/ponto/portas/regen_exportada.py:188`, `:197`.)

   Os testes novos usam constantes/freezer; não há leitura solta explícita. Mas `HOJE='12:00'` não declara offset, enquanto `AGORA` é 12h local: a equivalência entre os instantes não está assegurada pelo teste, com fuso configurado em São Paulo. (`app/folha/tests/test_competencia_paga_um_juiz.py:28`, `:79`; `app/config/settings/base.py:93`.)

5. **Os testes mordem?** [nao verificado]

   **Parcialmente.** Abaixo, mutantes plausíveis que o **caso citado** deixaria passar; sobrevivência à suíte completa não foi executada: **[nao verificado]**. [nao verificado]

   Todas as referências desta tabela são ao arquivo `app/folha/tests/test_competencia_paga_um_juiz.py`.

   | Caso | Mutante que não detecta |
   |---|---|
   | `test_competencia_paga_um_juiz.py:82` — nada pago | Ignorar completamente o cadastro externo. |
   | `test_competencia_paga_um_juiz.py:87` — entregue | Contar TXT invalidado como entrega. |
   | `test_competencia_paga_um_juiz.py:93` — marcada | Retirar filtro por ano; todas as fixtures usam 2026. |
   | `test_competencia_paga_um_juiz.py:102` — desmarcada | Não preservar autor/motivo da desmarcação. |
   | `test_competencia_paga_um_juiz.py:110` — K9 | Eliminar a metade holerite. |
   | `test_competencia_paga_um_juiz.py:119` — sem TXT | Comparar hash incorretamente quando existe TXT. |
   | `test_competencia_paga_um_juiz.py:127` — cinco leitores | Usar `dia.month` em vez da janela; só exercita dias anteriores ao corte. |
   | `test_competencia_paga_um_juiz.py:150` — memo | Omitir empresa da chave; só consulta uma empresa. |
   | `test_competencia_paga_um_juiz.py:163` — marcar duas vezes | Remover a foto detalhada do log, mantendo uma linha de auditoria. |
   | `test_competencia_paga_um_juiz.py:175` — desmarcar | Não salvar `desmarcada_motivo`. |
   | `test_competencia_paga_um_juiz.py:188` — remarcar | Criar linha nova, mas retornar a linha antiga ou omitir a trilha da remarcação. |
   | `test_competencia_paga_um_juiz.py:196` — staff | Remover a conferência de permissão somente da desmarcação. |
   | `test_competencia_paga_um_juiz.py:201` — motivo | Remover validação de competência ou de motivo na desmarcação. |
   | `test_competencia_paga_um_juiz.py:206` — relógio | Gravar instante diferente do recebido, mantendo recusa a `None`. |
   | `test_competencia_paga_um_juiz.py:236` — escreve | Gravar qualquer número diferente de `123.45`, sem cálculo correto. |
   | `test_competencia_paga_um_juiz.py:242` — externa recusa | Regravar outro campo FM ou apagar DP preexistente; não há sentinela DP. |
   | `test_competencia_paga_um_juiz.py:251` — exportada 08 | Alterar campo FM diferente de `horas_trabalhadas`. |
   | `test_competencia_paga_um_juiz.py:258` — backfill barrado | Não produzir DP por falha da lavra, sem efetivamente barrar. |
   | `test_competencia_paga_um_juiz.py:264` — leitura | Devolver somente `{colaborador_id: {}}`. |
   | `test_competencia_paga_um_juiz.py:269` — desmarcada escreve | Remover a guarda inteira. |
   | `test_competencia_paga_um_juiz.py:275` — retrato | Escrever e depois devolver `barrado_pela_guarda=True`. |
   | `test_competencia_paga_um_juiz.py:280` — passagem manual | Remover a guarda inteira; também aceita a fabricação direta da passagem. |
   | `test_competencia_paga_um_juiz.py:287` — passagem inválida | Aceitar autor arbitrário, passagem de outra competência ou reutilizada. |
   | `test_competencia_paga_um_juiz.py:295` — REGEN real | Retirar o retrato ou não produzir Pauta; verifica somente mudança das horas. |
   | `test_competencia_paga_um_juiz.py:312` — tela staff | Ocultar os botões; o teste faz POST manual. | [nao verificado]
   | `test_competencia_paga_um_juiz.py:326` — não staff | POST responder 500 sem gravar; não confere seu status. |
   | `test_competencia_paga_um_juiz.py:336` — sem motivo | Responder 500 sem gravar; não confere status/feedback. |

   **Cada tripwire e seu “MORDE”:**

   | Tripwire/caso | Mutante sobrevivente |
   |---|---|
   | Escritor único e exemplo sintético, `test_competencia_paga_um_juiz.py:431`, `:436` | `from ... import CompetenciaPagaForaDoSistema as C; C.objects.create(...)`. |
   | Construção da passagem e exemplo sintético, `test_competencia_paga_um_juiz.py:441`, `:448` | `from ... import PassagemRegen as P; P(...)`. |
   | Leitores migrados e exemplo de alias, `test_competencia_paga_um_juiz.py:453`, `:463` | Chamar juiz sob `if False`, ou ignorar seu retorno e usar um helper que rederive. |
   | Competência literal e exemplo sintético, `test_competencia_paga_um_juiz.py:468`, `:476` | `mes == 9 and ano == 2026`; a detecção procura tuplas. |

   As evasões por alias e chamada sem efeito foram confirmadas em AST, em memória; decorrem dos detectores por nome/cadeia e presença sintática. (`app/folha/tests/test_competencia_paga_um_juiz.py:374`, `:379`, `:405`, `:422`.)

   **Vacuidade:** os casos “desmarcada escreve” e “passagem manual passa” ficam verdes com a guarda removida; são controles positivos, não provas da cura. O backfill não é necessariamente vácuo: uma lavra bem-sucedida sempre cria AJUSTE, mas sua exceção é engolida e o teste só exige ausência. (`app/folha/tests/test_competencia_paga_um_juiz.py:269`, `:280`, `:258`; `app/ponto/services/dia_pago.py:348`; `app/ponto/services/fechamento.py:1261`.)

6. **Palavras/unidades com dois significados**

   - **“Paga”:** K10 significa TXT vigente ou cadastro externo; `marcos_da_competencia()['paga']` significa holerite publicado. **“Exportadas”** em `_competencias_exportadas` passa a incluir também cadastro externo e holerite. (`app/folha/export.py:620`; `app/ponto/services/fechamento.py:1582`; `app/ponto/management/commands/gerar_celulas.py:175`, `:184`, `:188`.)
   - **Retorno:** bool/fontes respondem pagamento; `registro=True` responde existência de TXT. Confundir `bool(registro)` com pagamento perde justamente a marcação externa. (`app/folha/export.py:610`, `:617`.)
   - **Mês civil × competência:** a tela começa no mês civil e lista seis meses; o juiz usa a competência cujo fim cai no mês informado. Após o corte, a competência corrente pertence ao mês seguinte e fica fora da tela — por exemplo, corte21 em 21/11 aponta para12, enquanto a tela começa11. (`app/folha/views_competencia_paga.py:16`, `:57`; `app/ponto/janelas.py:10`, `:24`.)
   - **Dia:** o wrapper recebe dia e aplica a janela; não converte timestamp civil em dia do turno. A lavratura recebe o mapa `dia_da_jornada`, enquanto a documentação do modelo ainda diz “dia da entrada”. (`app/chamados/services/vigia_fio.py:880`; `app/ponto/services/fechamento.py:1240`; `app/ponto/models.py:298`.)
   - **Unidades:** DP mistura rubricas em horas e três campos em minutos, explicitamente separados; não vi conversão nova errada nesse diff. (`app/ponto/services/dia_pago.py:34`, `:39`.)

7. **Travas, transações e parcialidade**

   - **Marca × marca:** `select_for_update` só encontra linha existente; duas primeiras marcações podem disputar a inserção. A constraint impede duplicidade viva, mas a segunda pode receber `IntegrityError`, não tratado pela view. (`app/folha/services/competencia_paga.py:72`; `app/folha/models.py:404`; `app/folha/views_competencia_paga.py:47`.)
   - **Marca × recálculo/retrato:** não compartilham trava. A guarda pode ler “não paga”, a marcação confirmar e depois ocorrer a escrita anteriormente liberada. (`app/ponto/services/fechamento.py:576`, `:921`, `:1167`; `app/ponto/services/dia_pago.py:453`, `:535`; `app/folha/services/competencia_paga.py:73`.)
   - **Trilha perdida:** o savepoint da auditoria permite desfazer somente o log e confirmar o cadastro. Bloqueios também conservam a recusa sem garantir trilha. (`app/core/observ.py:105`, `:133`, `:139`; `app/ponto/services/fechamento.py:591`; `app/ponto/services/dia_pago.py:454`.)
   - **REGEN concorrente:** “antes”, hash e régua são lidos antes do atomic, sem trava, podendo deixar de representar o estado imediatamente anterior à escrita. (`app/ponto/portas/regen_exportada.py:121`, `:174`.)
   - **REGEN parcial:** apesar da transação externa, o recálculo engole falhas por colaborador e de DP após salvar FM. A porta ignora seu retorno e segue retrato/trilha; o retrato não recompõe todas as rubricas. (`app/ponto/services/fechamento.py:1167`, `:1261`, `:1320`, `:1326`; `app/ponto/portas/regen_exportada.py:188`, `:197`; `app/ponto/services/dia_pago.py:482`, `:517`.)

8. **Migration**

   Campos, nulabilidade/defaults, nomes reversos, `PROTECT`, ordenação e constraint condicional conferem estaticamente. `BigAutoField` acompanha o default configurado. (`app/folha/models.py:387`; `app/folha/migrations/0013_competenciapagaforadosistema.py:20`; `app/config/settings/base.py:104`.)

   Modelo usa `'auth.User'`; migration usa `settings.AUTH_USER_MODEL`. Divergência efetiva com usuário customizado e resultado de `makemigrations --check`: **[nao verificado]**. (`app/folha/models

[truncado em 20000 de 22573 caracteres]

## 15/11 10:44 · sessao 65cfc6d9ac47 · FIO-MORTO-CLASSE-D · sonda · origem=Code

Convenção: `saída:N` numera o bloco final fornecido, começando na primeira linha `S motivo=…` como 1 e terminando em `FIM ok=1` como 25.

**(1)** RC do processo: **[não verificado]**, pois não foi informado. Linha final: **`FIM ok=1`**. Isso não elimina os erros nomeados de campo abaixo. (`saída:25`, `saída:23`, `saída:24`)

**(2)** Classe d: **`S soma=212` — bate 212**. Outros: **`S O soma=36` — bate 36**. (`saída:14`, `saída:22`)

**(3)** Motivo × dono × competência, com totais calculados dos resumos:

- **A** = `outro:C11_fora_queryset+assinaturas_sem_data`. (`saída:1`)
- **B** = `outro:C11_fora_queryset+somente_outros_dias`. (`saída:5`)
- **C** = `outro:C11_sem_pergunta_viva`. (`saída:11`)

| Motivo | Dono informado | 10/2026 | 11/2026 | Total | Fonte |
|---|---|---:|---:|---:|---|
| A | erro:E6IsencaoOuAusencia | 6 | 8 | 14 | saída:1–2 |
| A | erro:E6SemLavratura | 84 | 70 | 154 | saída:3–4 |
| B | ESTRUTURA | — | 1 | 1 | saída:5 |
| B | erro:E6IsencaoOuAusencia | 3 | 4 | 7 | saída:6–7 |
| B | erro:E6SemLavratura | 3 | 11 | 14 | saída:8–9 |
| B | sem_divergencia | — | 4 | 4 | saída:10 |
| C | erro:E6SemLavratura | 1 | 8 | 9 | saída:11–12 |
| C | sem_divergencia | — | 9 | 9 | saída:13 |
| **Total** | | **97** | **115** | **212** | saída:1–14 |

“—” significa competência sem linha correspondente no resumo fornecido. (`saída:1–13`)

**(4)** Erros nomeados por campo:

| Campo | Erro | n | Fonte |
|---|---|---:|---|
| dono | E6IsencaoOuAusencia | 21 | saída:23 |
| dono | E6SemLavratura | 177 | saída:24 |

São **198 resultados de erro em `dono`**, não 198 donos classificados como ESTRUTURA. O E6 exclui da comparação isenção/ausência e falta de lavratura; falta de lavratura não significa zero. (`saída:23–24`; [e6_oraculo.py:339](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/management/commands/e6_oraculo.py:339), [e6_oraculo.py:343](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/management/commands/e6_oraculo.py:343))

**(5)** Não foi comprovada obra que zere integralmente nenhum dos oito recortes. A classificação rigorosa é **“sem obra comprovada para zerar”**, mantendo a cobertura exata como **[não verificado]**:

| Recorte | n | Obra que zera |
|---|---:|---|
| A / erro:E6IsencaoOuAusencia | 14 | Sem obra comprovada [não verificado] |
| A / erro:E6SemLavratura | 154 | Sem obra comprovada [não verificado] |
| B / ESTRUTURA | 1 | Sem obra comprovada [não verificado] |
| B / erro:E6IsencaoOuAusencia | 7 | Sem obra comprovada [não verificado] |
| B / erro:E6SemLavratura | 14 | Sem obra comprovada [não verificado] |
| B / sem_divergencia | 4 | Sem obra comprovada [não verificado] |
| C / erro:E6SemLavratura | 9 | Sem obra comprovada [não verificado] |
| C / sem_divergencia | 9 | Sem obra comprovada [não verificado] |

Os números vêm de `saída:1–13`. Existem obras relacionadas, mas seus contratos não demonstram esse zero:

- **E3-LEITORES** migra leitores para `tarefa_do_dia`. ([BACKLOG.md:1955](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/BACKLOG.md:1955))
- **E4B** escolhe atos pela tarefa; classe/detalhe são apenas rótulos. `tarefa_nada` pode sair sem ato reparador, e o efeito precisa ser medido. ([backfill_fio_morto_e4b.py:12](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/management/commands/backfill_fio_morto_e4b.py:12), [mesmo arquivo:117](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/management/commands/backfill_fio_morto_e4b.py:117), [mesmo arquivo:26](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/management/commands/backfill_fio_morto_e4b.py:26))
- **CXR-TELA** explicitamente não cura d/ESTRUTURA. ([RELATO.md:1770](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/RELATO.md:1770))

**(6)** Há **212 dias sem cobertura de cura comprovada**, distribuídos na tabela acima; quantos deles **necessariamente exigem LEI ou DESENHO é [não verificado]**. Não se pode converter os 198 erros de dono em decisões pendentes de lei. (`saída:14`, `saída:23–24`)

Perguntas condicionais para esclarecer os recortes, sem propor regra nova:

| Recortes | n envolvido | Pergunta; necessidade de LEI/DESENHO |
|---|---:|---|
| A/B com E6IsencaoOuAusencia | 14 + 7 | Qual autoridade responde pelo atendimento desses dias excluídos da comparação E6? Necessidade de decisão: [não verificado]. |
| A/B/C com E6SemLavratura | 154 + 14 + 9 | Falta lavra, falta leitura ou o dia está fora do alcance da comparação? Qual obra cobre a causa efetiva? Necessidade de decisão: [não verificado]. |
| B/ESTRUTURA | 1 | Qual ato e porta existentes atendem esse dia fora do C11? Necessidade de decisão: [não verificado]. |
| B/C com sem_divergencia | 4 + 9 | O que retém cada dia e qual ação a tarefa exige, apesar de não haver divergência E6? Necessidade de decisão: [não verificado]. |

As quantidades são de `saída:1–13`; as perguntas são hipóteses de investigação **[não verificado]**. A pergunta de desenho já registrada — “dia que a folha retém e a tarefa não manda nada” — existe, mas enquadrar qualquer quantidade desta sonda nesse estado continua **[não verificado]**. ([RELATO.md:610](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/docs/RELATO.md:610))

## 15/11 10:43 · sessao dc5c9b3e1911 · - · - · origem=Ronald (tmux)

Evidências para a consolidação:

- **C11 não cobre genericamente dias fora da lista.** O queryset exige `veredito='indefinida'` e protesto `PREVISTO_COM_DIA_INDEFINIDO`; depois exige pergunta viva, nenhuma porta da tarefa, âncora terminal e folga não definida. (`app/escala/services/cadastro_realidade.py:450`, `:469`, `:496`, `:499`)
- **Causa d significa colaborador presente na lista, dia ausente e nenhuma pergunta viva da célula**, após os filtros anteriores do censo. (`app/chamados/services/vigia_fio.py:696`)
- **Os dois erros não são donos L-099.** O E6 exclui da comparação dias com isenção/ausência e dias sem lavratura; ausência de lavratura não equivale a zero. (`app/ponto/management/commands/e6_oraculo.py:339`, `:343`, `:245`)
- O juiz de dono retorna `CADASTRO`, `BATIDA` ou `ESTRUTURA`; esta última é residual. (`app/ponto/management/commands/e6_oraculo.py:98`, `:104`, `:106`, `:110`)
- **E4B não promete zerar pelo rótulo d:** classe/detalhe não escolhem cura; atos vêm da tarefa; `tarefa_nada`/`nao_sei` são listados sem ato; efeito exige nova medição. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:12`, `:15`, `:23`, `:26`, `:117`)
- Há pergunta de desenho existente sobre **C11 aceitar pergunta respondida sem âncora ou manter apenas âncora terminal**. (`app/docs/RELATO.md:14`) Vincular qualquer quantidade da sonda a esse M1 é **[não verificado]**.
- O mapa anterior registra d `SEM_ACAO` e pergunta sobre “folha retém, tarefa nada”, mas declara que a sonda daquele mapa não chamou C19. (`app/docs/RELATO.md:8820`, `:8831`) Não transferir seus números para esta saída.
- E3-RESTO-R3 existe como obra do lote LU e achados restantes. (`app/docs/BACKLOG.md:768`) Cobrir ou zerar algum dos recortes desta sonda é **[não verificado]**.

A saída não fornece C19/razão da tarefa, ata, estado das perguntas e âncoras ou efeito antes/depois; identificar a cura e quantificar quais dias **necessariamente** exigem LEI/DESENHO a partir disso é **[não verificado]**.

## 15/11 10:42 · sessao e2755ca12b7b · - · - · origem=Ronald (tmux)

Achados firmes para consolidar:

- **Host:** K10 está registrado em `app/core/juizes.py:356`, mas falta no MAPA-JUIZES; `app/docs/MAPA-JUIZES.md:191` ainda informa nove juízes. A cobrança por referência está em `bin/tests/test_mapa_juizes.sh:38-53`.
- **Diagramas:** `app/docs/ARQUITETURA.mmd:352` continua nove; gerador passa a produzir dez (`app/core/management/commands/gerar_diagrama.py:344-347`). Quebra a comparação de `app/core/tests/test_selo_diagrama_do_codigo.py:44-47`. MAPA também falta K10 (`app/docs/MAPA.md:211`).
- **Testes antigos incompatíveis:** exportada deve recalcular sem passagem e deixar carimbo em `app/ponto/tests/test_o80_selo_l092.py:98`, `:117`, `:131`, `:169`, `:188`, `:216`; novo bloqueio e carimbo condicionado à passagem contradizem essas expectativas (`app/ponto/services/fechamento.py:553`, `:576`).
- **Migration:** campos, nulabilidade, constraint, related_names, PROTECT e ordenação conferem estaticamente (`app/folha/models.py:387`, `app/folha/migrations/0013_competenciapagaforadosistema.py:20`). Modelo usa `auth.User`, migration usa setting; settings da cópia usam padrão, sem divergência efetiva demonstrada. BigAutoField confere (`app/config/settings/base.py:104`). `makemigrations --check` não executado [nao verificado].

Evasões AST comprovadas **em memória**, sem arquivos:

- Importar modelo `as C` e executar `C.objects.create(...)` retorna nenhuma escrita detectada (`app/folha/tests/test_competencia_paga_um_juiz.py:363-375`).
- Importar `PassagemRegen as P` e construir `P(...)` retorna nenhuma construção detectada (`app/folha/tests/test_competencia_paga_um_juiz.py:379-382`).
- Importar juiz, chamá-lo sob `if False`, retornar `[]`: detector aprova leitura do juiz (`app/folha/tests/test_competencia_paga_um_juiz.py:405-418`).
- Literal `mes == 9 and ano == 2026` escapa da catraca de tuplas (`app/folha/tests/test_competencia_paga_um_juiz.py:422-426`).

Mutantes sobreviventes dos casos novos foram enviados por mensagem com as linhas de cada caso. Principais lacunas: todos os anos são 2026; nenhum teste exerce fronteira dia21; nenhum guarda DiaPago preexistente; log só tem contagem/booleanos parcialmente conferidos; retrato só verifica flag; REGEN real só verifica mudança de horas (`app/folha/tests/test_competencia_paga_um_juiz.py:93`, `:134`, `:217`, `:170`, `:278`, `:306`).

**Não declarar o backfill necessariamente vácuo:** mesmo sem grade, `lavrar` cria AJUSTE (`app/ponto/services/dia_pago.py:350-360`). Entretanto, o teste só afirma ausência (`app/folha/tests/test_competencia_paga_um_juiz.py:258-262`), e erros da lavra são engolidos (`app/ponto/services/fechamento.py:1261-1264`); falta controle positivo e conferência da trilha. Retirar só a guarda mantendo execução bem-sucedida deve produzir AJUSTE; confirmação executada [nao verificado].

## 15/11 10:42 · sessao 6b6106da1749 · - · - · origem=Ronald (tmux)

Não localizei obra com contrato demonstrando zerar integralmente qualquer dos sete recortes motivo/dono da saída; a cobertura exata continua [não verificado].

Obras relacionadas, sem equivaler a garantia de zero:

- **E3-LEITORES** migra leitores para `tarefa_do_dia`, mas seu registro não identifica os recortes da sonda. `app/docs/BACKLOG.md:1955`.
- **E4B** cobre backfill das classes a–e, porém registra resíduo `tarefa_nada` e perguntas de lei `sem_porta`; não demonstra eliminar os recortes atuais. `app/docs/BACKLOG.md:1977`.
- **E3-X2-NUNCA-JULGADAS** trata universo específico de células nunca julgadas. O dado atual não permite equiparar esse universo aos 177 erros `E6SemLavratura`. `app/docs/BACKLOG.md:1962`; equivalência [não verificado].
- **E3-X1-FOLGA-NAO-E-SEM-COBERTURA** trata ramo `adesao=None` do cartório. A saída não revela esse predicado. `app/docs/BACKLOG.md:1959`; cobertura atual [não verificado].
- **E3-CADASTRO-ADMIN** é lista de cadastro para o administrador, explicitamente sem cura por código. `app/docs/BACKLOG.md:1961`.
- **INTERMITENTE-SEM-CONVOCACAO** registra a pergunta sobre como a célula sabe do dia convocado; não pertence à E3. A saída atual não demonstra quais linhas satisfazem essa causa. `app/docs/BACKLOG.md:1967`; vínculo [não verificado].
- **FATO-A05** registra fato próprio para assinatura após E4D, mantendo provisoriamente a projeção como fonte. Isso não prova resolver `assinaturas_sem_data`. `app/docs/BACKLOG.md:1971`; equivalência [não verificado].

Evidências especialmente úteis para evitar atribuição indevida:

- O registro da classe d diz que o brief da E3 não tinha decisão escrita para “dia que a folha retém e a tarefa não manda nada”. Trata-se de outra medição; não substituir seus números pelos 212 atuais. `app/docs/RELATO.md:610`.
- **CXR-TELA não cura d/ESTRUTURA**: pode apenas mudar a classificação para e/dia na lista. `app/docs/RELATO.md:1770`.
- O carregador do dono distingue ausência de resposta da lista do motor de lista vazia e pode retornar `None`; não autoriza converter erro em ESTRUTURA ou sem divergência. `app/ponto/management/commands/e6_oraculo.py:142`, `app/ponto/management/commands/e6_oraculo.py:193`.
- `minutos_do_espelho` declara que `None` significa sem lavratura, não zero. `app/ponto/management/commands/e6_oraculo.py:243`.

Recomendação para consolidação: usar **“sem obra comprovada para zerar este recorte [não verificado]”**, com os números do `S` fornecido; não afirmar que todos necessariamente exigem LEI/DESENHO. Essa necessidade e sua distribuição não são estabelecidas pela saída. [não verificado]

## 15/11 10:42 · sessao 745ce1ea1d34 · UI-CAMERA-ALTERNATIVA · verificador · origem=Code

**As cinco correções estão presentes, com limites de cobertura. Não identifiquei defeito novo no fluxo normal nem teste globalmente vazio.** (`T:253–273`; `S:95–126`, `S:137–140`, `S:184`) [nao verificado]

Referências: **T** = [bater_ponto_v2.html](/home/ronald/codex_ws/93b532e5ad0c59acd80b27c79c1df55391b229d4/app/templates/ponto/bater_ponto_v2.html); **S** = [test_camera_alternativa_previa.py](/home/ronald/codex_ws/93b532e5ad0c59acd80b27c79c1df55391b229d4/app/ponto/tests/test_camera_alternativa_previa.py).

| Achado da volta 1 | Veredito nesta cópia |
|---|---|
| Revogar antes de retirar a imagem antiga | **NÃO PROCEDENTE — curado no código.** `velha` guarda a URL anterior; o pintor substitui os filhos do placeholder antes de revogá-la. Isso também vale para o ramo sem blob. (`T:253–269`) |
| Teste aceitar prévia fora de `#v2-cam-ph` | **NÃO PROCEDENTE — curado.** `img()` agora consulta `ph.querySelector('img')`; ambas as verificações de prévia usam essa função. (`S:95`, `S:100–103`, `S:117–120`) | [nao verificado]
| Teste não consultar o chip | **NÃO PROCEDENTE nesse alcance — curado parcialmente.** Exige o texto “selfie pronta” depois da primeira seleção. Não verifica classe, visibilidade ou chip depois da segunda seleção. (`S:106`, `S:116–126`, `S:138`) | [nao verificado]
| Primeiro arquivo poder passar como segundo por igualdade de tamanho/MIME | **NÃO PROCEDENTE para conservar especificamente f1 — curado.** O resultado exige `f1.size !== f2.size` e o envio com tamanho/MIME de f2. Se os tamanhos coincidirem, falha; não passa silenciosamente. Outro blob com tamanho/MIME de f2 continua podendo escapar. (`S:125–126`, `S:140`, `S:184`) |
| Teste não exigir que a URL exibida permaneça sem revogação | **NÃO PROCEDENTE nos dois instantes medidos — curado.** Consulta a URL de i1 e depois a de i2 na lista de revogadas. Não acompanha todo o ciclo de vida. (`S:85–86`, `S:108`, `S:119`) | [nao verificado]

**PROCEDENTE — a cobertura da ordem continua insuficiente.** A sonda registra quais URLs foram revogadas, mas consulta os resultados depois das seleções; não observa se a imagem antiga ainda estava anexada no momento da revogação. Portanto, restaurar a ordem anterior pode passar nessas asserções, embora o código atual tenha corrigido essa ordem. (`S:85–86`, `S:97–119`; `T:260–269`)

Quem escreve e quem lê:

| Variável | Escritores | Leitores |
|---|---|---|
| `_previaUrl` | Inicialização nula; limpeza a cada pintura; recebimento da URL recém-criada. (`T:251`, `T:254`, `T:256`) | Cópia para `velha`; atribuição ao `img.src`. (`T:253`, `T:258`) |
| `velha` | Uma atribuição local por chamada, copiando `_previaUrl` antes de sua limpeza. (`T:253`) | Condição e argumento de `URL.revokeObjectURL`. (`T:269`) |

**NÃO PROCEDENTE — novo defeito no pintor/listener pelo uso dessas variáveis.** Zerar `_previaUrl` não revoga a URL: sua referência anterior permanece em `velha`. O pintor instala a nova imagem e revoga a anterior ao final; o `else` impede que o ramo de aviso sobrescreva “selfie pronta”. O listener preserva o estado quando não recebe arquivo e, quando recebe, escreve `_fotoBlob` e chama o pintor. (`T:253–273`)

**NÃO PROCEDENTE — asserção globalmente vazia.** Isoladamente, `exibida_viva` pode resultar verdadeiro sem imagem, pois consulta `''`; a parcela de URL atual em `revoga` tem a mesma limitação. Entretanto, a igualdade integral também exige presença de prévia, decodificação, área e troca: remover a imagem não deixa o teste verde. (`S:102–108`, `S:118–120`, `S:137–140`, `S:184`) [nao verificado]

**PROCEDENTE ainda — falta limpeza explícita da última URL.** Manter viva a URL atualmente exibida não resolve sua liberação ao encerrar a tela: a revogação continua ocorrendo somente em outra pintura; sucesso e retomada não a executam. (`T:150–157`, `T:269`, `T:321–325`)

Execução RED/GREEN, tamanhos efetivamente produzidos e comportamento em produção: **[não verificado]**. Não executei a suíte, cujo caminho cria e apaga arquivos temporários. (`S:170–174`) [nao verificado]

## 15/11 10:41 · sessao 42099f31c7e3 · - · - · origem=Ronald (tmux)

Achados para itens 3, 4, 6, 7 e 10:

- **Passagem REGEN forjável/reutilizável:** construtor público só exige autor não nulo e motivo não vazio; não verifica staff, competência, colaborador, validade nem consumo. A guarda aceita apenas pelo tipo; qualquer chamador pode instanciar, reutilizar e passar num recálculo em lote. O próprio teste chama diretamente o recálculo com a passagem fabricada (`app/ponto/services/fechamento.py:40-52,578-583`; `app/folha/tests/test_competencia_paga_um_juiz.py:280-285`). Retrato também aceita qualquer instância (`app/ponto/services/dia_pago.py:451-453`).

- **Bypass de empresa inativa:** sem filtros, escopo da guarda enumera somente empresas ativas; universo do recálculo inclui todas as empresas. Logo colaborador de empresa inativa com competência paga pode ser regravado no recálculo geral (`app/ponto/services/fechamento.py:447-453,576-587,921-924,1166-1167,1533`).

- **Relógio:** borda nova lê `now/localdate`, cadastro recebe `agora` (`app/folha/views_competencia_paga.py:46,57`; `app/folha/services/competencia_paga.py:78,97`). Entretanto auditoria lê implicitamente outro relógio por `auto_now_add`, sem receber `agora` (`app/core/auditoria.py:8-24`; `app/core/models.py:307`). Funções tocadas continuam lendo relógio fora da borda: recálculo (`app/ponto/services/fechamento.py:639,1109,1166,1184,1212`) e retrato (`app/ponto/services/dia_pago.py:534`). REGEN recebe `hoje`, mas não o transmite ao recálculo/retrato (`app/ponto/portas/regen_exportada.py:107-108,188-197`).

- **Testes e fuso:** classes funcionais usam freezer; constantes são fixas (`app/folha/tests/test_competencia_paga_um_juiz.py:28-29,79,167,218,309`). Porém `HOJE` não contém offset e `AGORA` é 12h local; sob São Paulo são instantes separados por três horas, permitindo auditoria anterior ao ato de marcação (`app/config/settings/base.py:93-95`; `app/core/models.py:307`). Execução desse comportamento [nao verificado].

- **Mês civil × competência:** tela enumera seis meses civis até o atual; janela usa mês do término e corte da empresa. Após o corte, competência corrente é mês seguinte e não aparece. Exemplo derivado: 21/11 com corte21 pertence a12, mas tela começa11 (`app/folha/views_competencia_paga.py:16-23,57`; `app/ponto/janelas.py:10-21,24-33`; `app/folha/export.py:47-51`). Meses anteriores aos seis listados também não têm formulário acessível; inputs são ocultos (`app/templates/folha/competencias_pagas.html:24-26`).

- **Dois contratos no retorno:** bool/fontes dizem paga; `registro=True` responde somente TXT e retorna `None` mesmo paga fora, conscientemente documentado. Chamador que aplicar `bool(registro)` responde entrega, não pagamento (`app/folha/export.py:610-628`). K9 acrescenta holerite publicado, outra condição (`app/folha/export.py:656-665`).

- **Corrida cadastro × recálculo/retrato:** cadastro tranca somente linha viva já existente; guarda lê pagamento sem trava e grava muito depois, sem chave comum de exclusão (`app/folha/services/competencia_paga.py:72-78`; `app/ponto/services/fechamento.py:576-587,921,1167,1183`; `app/ponto/services/dia_pago.py:453,462,535-546`). Intercalação permite marcação commitar antes da escrita já autorizada pela leitura antiga.

- **Dupla marcação concorrente:** sem linha viva, ambos passam pela consulta; constraint preserva unicidade, mas segunda inserção pode levantar `IntegrityError`, sem tratamento/idempotência concorrente. View só captura `MarcacaoRecusada` (`app/folha/services/competencia_paga.py:72-78`; `app/folha/models.py:404-405`; `app/folha/views_competencia_paga.py:44-48`).

- **Trilha perdida:** `atomic` do cadastro não exige sucesso de auditoria. `registrar_log` engole falha; `evento` usa savepoint e retorna `None`, permitindo commit do fato sem LogAuditoria (`app/folha/services/competencia_paga.py:72-82,91-103`; `app/core/auditoria.py:17-29`; `app/core/observ.py:105-139`). Recálculo barrado também conserva bloqueio sem garantir trilha (`app/ponto/services/fechamento.py:590-598`; `app/ponto/services/dia_pago.py:454-461`).

- **REGEN concorrente/parcial:** hash, valores e régua anteriores são lidos antes do `atomic`, sem trava; outro escritor pode alterar o “antes” (`app/ponto/portas/regen_exportada.py:121-127,174`). Embora doc prometa erro subir, recálculo engole erros por colaborador e de lavratura após salvar Fechamento; porta ignora retorno e segue retrato/auditoria, permitindo correção parcial (`app/ponto/portas/regen_exportada.py:95-96,188-221`; `app/ponto/services/fechamento.py:1167,1183,1261-1264,1320-1331`). Retrato cobre apenas minutos de grade/ajustes, não todas as rubricas (`app/ponto/services/dia_pago.py:482-498,517-546`).

- **Cadastro inacessível após inativação:** empresas da tela vêm de filtro que exclui inativas; histórico também fica restrito àquelas empresas, impedindo desmarcar pela tela uma marcação antiga da empresa inativada (`app/folha/views_competencia_paga.py:36,39,69-72`; `app/colaboradores/queries.py:78-85`).

- **Consumidor REGEN possivelmente incompleto:** único chamador encontrado da porta só entra se `exportada` e houve plantio; pagamento externo sem TXT pode não chegar à porta/Pauta, conforme origem de `exportada` a rastrear (`app/chamados/services/passivo_validado.py:435-449`).

## 15/11 10:41 · sessao 97863fde7433 · - · - · origem=Ronald (tmux)

Achados de maior peso:

- **Segundo juiz K6 real:** `chamados/services/passivo_validado.py:116-128` pergunta “tem TXT vigente?” por `haiku_export.exportacoes_da_competencia()['vigente']`, sem chamar K6. `app/folha/haiku_export.py:65-74` escolhe `e_vigente`, que significa “último registro”, sem desconsiderar invalidação (`app/folha/models.py:120-137`); K6 desconsidera `invalidada_em` (`app/folha/export.py:588-594`). Assim pode responder exportada em TXT revogado sem substituta e responde falsa na paga fora sem TXT. Esse resultado decide se chama REGEN e cria Pauta DP (`app/chamados/services/passivo_validado.py:435-449`).
- **Passagem não é capacidade protegida:** construtor público aceita qualquer `autor != None` e qualquer motivo não vazio; não exige staff, colaborador, empresa, competência, prazo, uso único ou contexto da porta (`app/ponto/services/fechamento.py:45-61`). As guardas só exigem `isinstance` (`app/ponto/services/fechamento.py:578-584`, `app/ponto/services/dia_pago.py:450-455`). Pode ser construída, alterada e reutilizada por outro chamador; o próprio teste faz isso (`app/folha/tests/test_competencia_paga_um_juiz.py:291-296`).
- **Cadastro pode confirmar sem LogAuditoria:** `registrar_log` engole falha (`app/core/auditoria.py:17-29`), `evento` também usa best effort e savepoint próprio (`app/core/observ.py:94-105`, `app/core/observ.py:124-140`). Portanto o atomic da marcação não garante linha e trilha juntas (`app/folha/services/competencia_paga.py:72-84`, `app/folha/services/competencia_paga.py:91-103`).
- **Dinheiro ainda tem bypass independente de REGEN:** `restaurar_fechamento` permite atualizar rubricas arbitrárias com motivo, sem K10 nem Passagem (`app/ponto/services/fechamento.py:1381-1419`); há chamadores de apply de competência 09 (`app/ponto/management/commands/aplicar_09_he_intra.py:137-152`, também `aplicar_09_corte_b.py:220,266`, `diff_reclassificar_partido.py:224`, `desvio_o68b.py:197`). `DiaPago.lavrar` é função pública sem guarda própria, apaga e recria (`app/ponto/services/dia_pago.py:135-163`, `app/ponto/services/dia_pago.py:359-360`); chamadores atuais de produção ficam em fechamento (`app/ponto/services/fechamento.py:1223`, `app/ponto/services/fechamento.py:1275`).
- **Scripts fora app também escrevem:** `bin/restore_relavratura_10_2026.py:69-86` e `bin/restore_o263_comp10.py:43-56` salvam/apagam FM e recriam DP diretamente; `bin/restore_badge_linha_09_10.py:61-63` recria DP 09/10. Os tripwires novos só varrem APP (`app/folha/tests/test_competencia_paga_um_juiz.py:32`, `app/folha/tests/test_competencia_paga_um_juiz.py:362-373`).

Cadastro, por campo:

| Campo | Escritor | Leitores |
|---|---|---|
| id | ORM no create `app/folha/services/competencia_paga.py:77-78` | `_foto`, objeto do log, retorno: `:59`, `:79`, `:84`; `__str__` `app/folha/models.py:409-411` |
| empresa, mes, ano | marcação `:77-78` | juiz `app/folha/export.py:623-624`; buscar viva `competencia_paga.py:73-74,92-93`; `_foto :59`; enumeração `app/ponto/management/commands/gerar_celulas.py:175-177`; histórico `app/folha/views_competencia_paga.py:69-72`; template `app/templates/folha/competencias_pagas.html:50-51` |
| marcada_por, marcada_em, motivo | marcação `competencia_paga.py:77-78` | `_foto :60-61`; histórico/template `app/templates/folha/competencias_pagas.html:52-54`; ordering `app/folha/models.py:403`; view `:72` |
| desmarcada_por/em/motivo | defaults `app/folha/models.py:395-398`; desmarcação `competencia_paga.py:97-98` | `_foto :62-64`; viva no juiz `app/folha/export.py:624`; serviço `:74,93`; template `app/templates/folha/competencias_pagas.html:55`; `__str__ app/folha/models.py:411` |

Nenhum segundo escritor efetivo do cadastro foi encontrado nos arquivos pesquisados; o cadastro não tem proteção própria de `save/delete` (`app/folha/models.py:373-411`). Isso é ausência de ocorrência no censo, não impossibilidade de bypass.

Retorno juiz:

- Bool: guardas via `empresas_pagas_no_escopo` (`app/ponto/services/fechamento.py:1557-1558`), wrapper do dia (`app/chamados/services/vigia_fio.py:878-887`), veto do fio (`app/chamados/services/fio.py:1353-1356`).
- Lista fontes: K9 (`app/folha/export.py:656`), tela (`app/folha/views_competencia_paga.py:65-68`), TXT sem arquivo (`app/folha/export.py:706`).
- Registro: TXT comparador (`app/folha/export.py:693`) e vigia (`app/folha/services/vigia_txt.py:224`); `registro=True` é atalho direto ao K6 e não informa paga fora (`app/folha/export.py:617-618`).

LogAuditoria:

- Marca/desmarca escrevem ações, modelo/id/descrição, antes/depois por `_foto` (`app/folha/services/competencia_paga.py:79-83,99-103`); escritor final `evento` (`app/core/observ.py:134-140`).
- Recalculo barrado escreve ação, modelo, descrição e até 200 colaboradores (`app/ponto/services/fechamento.py:591-599`); retrato barrado escreve descrição apenas (`app/ponto/services/dia_pago.py:455-461`). Leitor específico conta prefixo de descrição, sem empresa (`app/ponto/services/fechamento.py:66-70`), comando imprime (`app/ponto/management/commands/recalcular_fechamento.py:126-131`).
- Marca/desmarca não têm leitor específico de LogAuditoria; tela mostra o próprio cadastro (`app/folha/views_competencia_paga.py:69-73`); leitor genérico do log é `app/core/views.py:705-738`. ACOES não enumera verbos novos, embora campo aceite gravação livre (`app/core/models.py:286-307`).

FM/DP escritos fora dessas guardas:

- Aprovar/reabrir status, autor/data e observação: `app/ponto/services/fechamento.py:1346-1377`, reabertura de período `:1472-1475`.
- Restaurar números: `app/ponto/services/fechamento.py:1381-1419`.
- Invalidar carimbos FM sem consultar pagamento: `app/ponto/services/fechamento.py:1650-1656,1691-1699`.
- Backfill DP `furo_apurado/lavrado_em`: `app/ponto/services/dia_pago.py:646-675`, command `app/ponto/management/commands/apurar_furo_da_exportada.py:131`. Sem dinheiro; ainda é escrita em DP.
- Lavratura command passa pelas guardas, mas permanece roteada por TXT (`app/ponto/management/commands/lavrar_dias_pagos.py:104-123`); paga fora entra no ramo motor e é barrada.
- Regen de vínculo de célula recebe K9 via `_competencias_exportadas`, portanto passa a respeitar paga fora (`app/ponto/portas/celula.py:218-221`); `apesar_da_lavra` pula guarda de célula, mas recálculo segue sem Passagem fora REGEN (`app/ponto/portas/celula.py:218-221`; passagem só em `app/ponto/portas/regen_exportada.py:187-197`).

Leitores fora do diff: todos que continuam em K6 terão **False** em paga fora sem TXT, versus **True** do K10; isso é diferença demonstrável, e a pergunta específica decide se é defeito:

| Local | Pergunta/efeito |
|---|---|
| `app/folha/views.py:353-364` | já exportado? aviso com arquivo/autor; diferença coerente com TXT |
| `app/ponto/services/fechamento.py:1580-1583` | exportada por K6; **paga significa holerite publicado**, não K10 |
| `app/folha/services/ranking_he.py:78-91` | todas empresas entregues para status “lavrado”; paga fora continua “parcial” |
| `app/ponto/services/esmeril_espelho.py:51-62` | retrocede janelas até primeira entregue; paga fora não encerra busca |
| `app/ponto/services/he_pendente_lavrado.py:218-224` | mantém anterior enquanto não entregue; paga fora continua universo |
| `app/chamados/services/cobranca_folha.py:47-72` | exclui exportada da cobrança; paga fora continua cobrança |
| `app/ponto/services/autorizacao_he_periodo.py:113-118,288-302` | exportada decide criar Pauta DP; paga fora não gera pauta apesar de gravado barrado |
| `app/folha/porta_export.py:339-347` | alias `_exp_no_escopo`, TXT escolhe campos do contador DP; paga fora tratada aberta |
| `app/chamados/models.py:353-364` | competência aberta para proteger perguntas vivas no fechamento do chamado |
| `app/chamados/services/fio.py:813-815` | competência aberta para impedir costura em terminal; este permanece K6 enquanto veto migrou |
| `app/chamados/reconciliador.py:743-748` | entregue fica fora do alcance novo de cura |
| `app/chamados/services/disputa_emissao.py:1855-1867` | exportada impede reabertura do credor |
| `app/chamados/services/fabrica_da_celula.py:223-231` | limbo entregue vai Pauta DP; paga fora continua pergunta |
| `app/chamados/services/tarefa_do_dia.py:451-467` | lote de dias exportados da carga da tarefa |
| `app/chamados/services/vigia_fio.py:837` | dono exportada/aberta; wrapper K6 `:843-865` |
| `app/chamados/services/foto_e4b.py:238-245` | foto entregue por dia ou competência |
| `app/chamados/management/commands/backfill_fio_morto_e4b.py:173,256,304` | plano e rechecagem de entregue |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:471,498` | contador/relatório exportadas, divergindo do veto migrado usado pelo apply |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py:95` | exportada via `marcos`, fora reabertura |
| `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:107-116` | recusa rederivar se entregue |
| `app/ponto/management/commands/lavrar_status_competencia.py:35-43` | informa entregue, não é guarda |
| `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:109-124` | só entrega entra pauta pago/devido |
| `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:83-93` | só entrega entra pauta de folga |
| `app/folha/management/commands/pre_fechamento.py:72-74` | lê marcos, inclusive “paga”=holerite |
| `app/chamados/services/passivo_validado.py:116-128` | derivação paralela acima |

Leitores fora diff que **acompanham** paga fora por K9, sem segundo predicado: `app/ponto/portas/celula.py:221,607,821`, `app/escala/services/vigencia_impossivel.py:26-42`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:60-82`, `app/ponto/management/commands/aplicar_col900.py:59-64`, `app/ponto/management/commands/tripwire_celulas.py:318-319`, aliases dos censos `app/chamados/management/commands/auditar_invariantes_chamados.py:141,888,1379`. Os nomes “exportadas” desses wrappers passam a incluir paga fora/holerite; é polissemia demonstrada por `app/ponto/management/commands/gerar_celulas.py:175-188`.

## 15/11 10:39 · sessao bf1ce396d09f · UI-CAMERA-ALTERNATIVA · verificador · origem=Code

O diff corrige o caminho normal da câmera alternativa sem mudar os valores consumidos pelo POST ou pela fila. Há ressalvas procedentes sobre cobertura do teste, limpeza da última URL e prévia após enfileiramento offline. (`T:255–271`, `T:305`, `T:339`, `T:352`, `T:365`) [nao verificado]

Referências usadas abaixo:

- **T** = [app/templates/ponto/bater_ponto_v2.html](/home/ronald/codex_ws/a50f605dba2aeb0289b52ebad626c92171197e92/app/templates/ponto/bater_ponto_v2.html)
- **S** = [app/ponto/tests/test_camera_alternativa_previa.py](/home/ronald/codex_ws/a50f605dba2aeb0289b52ebad626c92171197e92/app/ponto/tests/test_camera_alternativa_previa.py)
- **P** = [app/static/pwa.js](/home/ronald/codex_ws/a50f605dba2aeb0289b52ebad626c92171197e92/app/static/pwa.js)
- **H** = [app/core/tests/test_smoke_chromium.py](/home/ronald/codex_ws/a50f605dba2aeb0289b52ebad626c92171197e92/app/core/tests/test_smoke_chromium.py)

**T−** designa o mesmo arquivo antes da alteração, com linhas reconstruídas do diff anexado. Autenticação independente dos commits: **[não verificado]**. Execução do teste/RED/GREEN: **[não verificado]**; ele cria e apaga arquivos temporários, incompatíveis com esta auditoria somente leitura. (`S:163–167`, `H:94–100`) [nao verificado]

**1. Leitores, escritores e significado dos campos**

**NÃO PROCEDENTE — alteração do envio.** O novo pintor acrescenta leitura de `_fotoBlob` para apresentação; não escreve nesse campo. `capturarFrame`, `garantirRequisitos`, POST e chamadas da fila conservam suas operações anteriores. (`T−:256–269`, `T:275–288`; `T−:285–297`, `T:304–316`; `T−:320`, `T:339`; `T−:333`, `T:352`; `T−:346`, `T:365`)

| Objeto | Antes: todos os acessos relevantes | Depois: todos os acessos relevantes |
|---|---|---|
| `_fotoBlob` | **Escritores:** inicialização nula `T−:146`; arquivo recebido `T−:252`; resultado da captura, preservando valor anterior `T−:286`. **Leitores:** fallback da captura `T−:259`; atribuição/guarda dos requisitos `T−:286–287`; POST `T−:320`; duas chamadas offline `T−:333`, `T−:346`. | **Escritores:** `T:146`, `T:271`, `T:305`. **Leitores:** novos ramos de pintura/criação da URL `T:255–256`; captura `T:278`; requisitos `T:305–306`; POST `T:339`; fila `T:352`, `T:365`. |
| `_stream` | **Escritores:** inicialização `T−:146`, resultado de `getUserMedia` `T−:238`. **Leitores:** parar tracks no sucesso `T−:151`; atribuir ao vídeo `T−:239`. | Mesmos acessos e linhas: `T:146`, `T:151`, `T:238–239`. Não há atribuição posterior de `null`. (`T:146–151`, `T:235–245`) |
| `#v2-cam-ph` | Markup/classe/conteúdo inicial `T−:89`; localização em `abrirCamera` `T−:236`; `style.display='none'` `T−:239`; `className` `T−:243`; `innerHTML` do aviso `T−:244–245`; cursor e `onclick` `T−:246`. | Inicial/localização/ocultação `T:89`, `T:236`, `T:239`; cursor e `onclick` `T:243`; localização pelo pintor `T:253`; classe, limpeza de `innerHTML` e imagem anexada `T:260`; classe/conteúdo do aviso `T:264–266`. Não há escrita nova em `style.display` nem substituição de `onclick`. (`T:252–267`) |
| `#v2-cam-chip` | Inicial `T−:87`; localização `T−:236`, `T−:252`; classe/texto ao vivo `T−:240`, falha `T−:247`, selfie recebida `T−:252`. | Inicial/localização `T:87`, `T:236`, `T:253`; classe/texto ao vivo `T:240`, selfie pronta `T:261`, sem câmera `T:267`. Não há leitor JS do texto/classe para decidir envio. (`T:275–288`, `T:304–373`) |
| `#v2-cam` | Markup inicial `T−:88`; localização `T−:236`, `T−:258`; escrita em `srcObject`/`style.display` `T−:239`; leitura de display, largura, altura e conteúdo na captura `T−:259–261`. | Mesmas operações: `T:88`, `T:236`, `T:239`, `T:277–280`. |
| `#v2-cam-fallback` | Markup/atributos `T−:90`; chamada de `.click()` `T−:246`; instalação de `change` `T−:250`; leitura de `files[0]` `T−:251`. | Markup `T:90`; `.click()` `T:243`; instalação de `change` `T:269`; leitura de `files[0]` `T:270`. Não há escrita/reset de `files` pelo código da tela. (`T:269–272`) |

O navegador aplica às apresentações as regras de quadro, vídeo, placeholder e chip; essas regras continuam nas mesmas linhas. (`T−:14–21`, `T:14–21`)

**NÃO PROCEDENTE — consumidor supondo significado incompatível.** A captura consulta o vídeo, não o conteúdo de `#v2-cam-ph`. POST e fila recebem `_fotoBlob`, não URL nem `<img>`. A fila armazena esse argumento em `foto` e posteriormente o envia como multipart. (`T:277–280`, `T:339`, `T:352`, `T:365`; `P:107–118`, `P:143–156`)

**2. Coerência da tela nos caminhos pedidos**

| Caminho | Veredito e evidência |
|---|---|
| Falha da câmera, seguida de selfie válida | **NÃO PROCEDENTE: aviso persistente.** O handler guarda o arquivo e repinta; o pintor remove o aviso, anexa a imagem e atualiza o chip. O `onclick` permanece no contêiner. (`T:243`, `T:255–261`, `T:269–271`) |
| Seleção cancelada, sem arquivo | **NÃO PROCEDENTE: perda da selfie anterior.** O retorno antecipado preserva blob, imagem, URL e chip. Se ainda não havia selfie, preserva o aviso. (`T:270–271`) |
| Segunda selfie | **NÃO PROCEDENTE: prévia anterior permanente.** O arquivo substitui `_fotoBlob`; o pintor revoga a URL anterior, cria outra e substitui os filhos do quadro. (`T:254–260`, `T:271`) |
| Prévia com `_fotoBlob=null` | **NÃO PROCEDENTE nos escritores normais.** A imagem só é criada dentro de `if(_fotoBlob)`; nenhum escritor posterior limpa o blob. (`T:146`, `T:255–260`, `T:271`, `T:305`) |
| Câmera ao vivo funcionando | **NÃO PROCEDENTE: regressão introduzida pela cura.** O sucesso continua exibindo vídeo, escondendo placeholder e marcando “câmera ao vivo”; a confirmação captura o vídeo. (`T:238–240`, `T:277–285`) |
| Câmera ao vivo para depois de funcionar | **PROCEDENTE como limite anterior.** Não há acompanhamento de encerramento dos tracks para corrigir o chip; “câmera ao vivo” pode permanecer depois da interrupção. Isso já decorre do ramo anterior, não do novo pintor. (`T−:235–249`, `T:235–245`, `T:151`) |
| `pageshow`/bfcache | **NÃO PROCEDENTE: preservação indefinida deliberada da selfie antiga.** `persisted` solicita reload; retorno visível após cinco minutos também. A nova execução começa com blob nulo e markup inicial. O comportamento efetivo no iPhone/PWA e eventual restauração do input são **[não verificado]**. (`T:319–324`, `T:146`, `T:87–90`) |
| Reload | O código não reconstrói `_fotoBlob` a partir do input ou da fila; reinicia o estado local. Persistência da selfie através de reload: **NÃO PROCEDENTE como garantia desta tela**. (`T:146`, `T:269–271`) |
| Offline ou queda de rede | **PROCEDENTE: a prévia posterior pode diferir da foto já enfileirada.** Após registrar f1, apenas o botão é escondido; o quadro continua permitindo selecionar f2. A UI passa a mostrar f2 ao lado do feedback da batida que guardou f1. Isso não altera retroativamente a fila. (`T:122–126`, `T:269–271`, `T:352–357`, `T:365–370`; `P:107–118`) |
| `v2Sucesso` | **NÃO PROCEDENTE: chip divergente exposto por esse caminho.** O sucesso para os tracks e cobre a tela com overlay antes do redirecionamento; não limpa os elementos subjacentes. (`T:150–157`, `T:346`) |

**PROCEDENTE, com limite de alcance — blob não garante imagem decodificável.** A tela marca “selfie pronta” imediatamente, sem esperar `load` nem tratar `error`. Um arquivo não decodificável pode produzir imagem quebrada com chip pronto; ocorrência no dado de produção: **[não verificado]**. Não é demonstração de `_fotoBlob` ausente. (`T:90`, `T:256–261`, `T:270–271`)

**3. Revogação, limpeza, declaração e conteúdo não confiável**

- **PROCEDENTE na ordem literal:** a URL antiga é revogada enquanto o `<img>` antigo ainda a referencia; a retirada ocorre depois, na mesma execução síncrona. Imagem quebrada persistente no caminho normal: **[não verificado]**. (`T:254–260`)
- **NÃO PROCEDENTE — acúmulo de imagens/URLs a cada troca:** a pintura limpa os filhos anteriores e revoga a URL anterior. (`T:254`, `T:260`)
- **PROCEDENTE — falta limpeza explícita da última URL:** somente outra pintura a revoga. `v2Sucesso` e os handlers de retomada não fazem essa limpeza. Isso demonstra retenção durante a vida do documento, não vazamento ilimitado comprovado. (`T:254`, `T:150–157`, `T:319–324`)
- **NÃO PROCEDENTE — corrida pela declaração tardia:** declarar `abrirCamera` não a executa. `_camNegada`/`_previaUrl` são inicializadas antes de registrar o listener que chama a função. (`T:235`, `T:251`, `T:273`)
- **NÃO PROCEDENTE — injeção pelo novo `innerHTML`:** `_camNegada` escolhe entre duas mensagens constantes; o valor do erro não entra no HTML. A foto recebe URL criada pelo navegador em propriedade DOM, sem concatenar nome ou conteúdo do arquivo. (`T:242`, `T:256–266`)

**4. O teste MORDE?** [nao verificado]

**Morde parcialmente por construção; não há comprovação executada de RED/GREEN.** O arquivo não injeta mutantes nem exige que eles falhem: executa os cenários esperados e compara o resultado. (`S:171–177`)

| Asserção | Mutante alcançado e limite |
|---|---|
| Chromium presente | Reprova ambiente sem executável; sozinha não demonstra execução de JS. (`S:158`, `H:76–77`) |
| Input presente no HTML | Pega remoção do id; sozinha aceita elemento inutilizável. (`S:162`) |
| Veredito não nulo | Pega ausência do marcador; silêncio da sonda não vira verde. (`S:66–68`, `S:168`; `H:123–125`) | [nao verificado]
| Ausência de `erro` | Pega elementos ausentes, falta do aviso inicial e exceções capturadas. A precondição verifica texto, não visibilidade. (`S:82–84`, `S:125`, `S:176`) |
| `aviso=False` | Pega manutenção das palavras do aviso. Trocar apenas o texto passa esta parcela, mas falha nas parcelas de imagem. (`S:77`, `S:99–101`) |
| `previa=True` | Pega ausência de `<img blob:>`. Procura no **pai** do placeholder; não exige descendência de `#v2-cam-ph`. (`S:83`, `S:94`, `S:100`) |
| `decodificada=True` | Pega texto sem prévia e imagem quebrada; exige imagem completa com largura natural 4. Não compara pixels, bytes ou altura. (`S:101`) |
| `area=True` | Pega área geométrica nula; não verifica opacidade, `visibility` ou oclusão. (`S:102–103`) |
| `toque=True` | Pega perda do caminho de clique no centro medido. Não exige especificamente `ph.onclick`; `.click()` do input é substituído por contador, portanto não comprova abertura real da câmera do sistema. (`S:87`, `S:104–108`) |
| `troca=True` | Pega URL de prévia estagnada na segunda seleção. (`S:109–113`) |
| `revoga=True` | Pega omissão da revogação anterior; não cobra ordem de remoção nem limpeza da última URL. (`S:85–86`, `S:114`) |
| `decodificada2=True` | Pega segunda prévia quebrada ou ainda com largura 4; não repete área ou clique depois da segunda seleção. (`S:110–115`) |
| `aviso2=False` | Pega retorno textual do aviso após a segunda seleção. (`S:116`) |
| `envio=True` | Pega ausência de foto/POST, quantidade diferente de um envio e tamanho/MIME divergentes. Outro blob com mesmo tamanho e MIME passa. (`S:60–64`, `S:118–120`) |
| Igualdade integral com `ESPERADO` | Exige todos os resultados e o conjunto exato de chaves; não basta desaparecer o aviso. (`S:131–134`, `S:177`) |

Sobre os mutantes propostos:

- **Só trocar o texto, sem prévia: seria pego**, pelas verificações de imagem, decodificação e área. (`S:100–103`)
- **Prévia sem revogar: seria pego**, pela anotação de `revokeObjectURL`. (`S:85–86`, `S:114`)
- **Prévia fora de `#v2-cam-ph`, sem clique próprio: pode escapar**, se continuar dentro do pai pesquisado, tiver área e deixar o centro do placeholder acessível ao clique antigo. Perder todo caminho de clique naquele centro seria pego. (`S:83`, `S:94`, `S:104–108`)
- **Quebrar o envio da segunda selfie: cobertura insuficiente.** Enviar blob errado de mesmo tamanho/MIME escapa. Conservar especificamente o primeiro PNG seria pego somente se seus tamanhos diferirem; tamanhos efetivamente produzidos nesta execução: **[não verificado]**. (`S:70–75`, `S:95`, `S:110`, `S:120`)

**NÃO PROCEDENTE — asserção globalmente vazia.** Há precondição de aviso, exigência positiva de imagem e veredito obrigatório. **PROCEDENTE — lacunas:** nenhuma asserção consulta o chip; a sonda força câmera rejeitada, conexão online e resposta HTTP 599, deixando fora câmera funcionando, cancelamento, offline, sucesso e retomada. Os erros de console retornados são descartados. (`S:48–64`, `S:80–120`, `S:165–177`)

**NÃO PROCEDENTE — acesso inválido ao `_fotoBlob` privado.** A sonda alcança um leitor legítimo: `window.v2Confirmar` fecha sobre o IIFE, chama requisitos/captura e coloca `_fotoBlob` no `FormData`; o fetch interceptado observa esse argumento. (`T:138`, `T:146`, `T:278`, `T:305`, `T:326`, `T:339`; `S:62`, `S:118`)

**NÃO PROCEDENTE — somente uma casca renderizada.** O laço percorre ambas, e o loader associa `base.html` ao conteúdo do arquivo escolhido. Ambas possuem o bloco `content` usado pela tela. Isso verifica renderização direta, não seleção real da casca pela sessão/rota. (`S:38`, `S:137–150`, `S:172–175`; `T:1–3`; `app/templates/base.html:336`; `app/templates/base_app.html:94`)

**5. Classes que já reprovaram obras**

| Classe | Veredito |
|---|---|
| Segundo juiz duplicando autoridade | **NÃO PROCEDENTE.** O registro vivo contém `selfie_ausente`, que julga foto da batida persistida e prazo do canal; o pintor decide apenas apresentação do blob local. O próprio lookup manda consultar o registro vivo. (`app/docs/JUIZES.md:10–15`; `app/core/juizes.py:1202–1205`; `T:255–267`) |
| Segundo escritor fora da porta | **NÃO PROCEDENTE para dados/envio.** O pintor não escreve `_fotoBlob` nem persiste batida; escreve apresentação e recurso visual. Os escritores anteriores do blob permanecem. (`T:146`, `T:254–271`, `T:305`; `P:107–118`) |
| Relógio lido fora da borda | **NÃO PROCEDENTE no diff.** A nova pintura não consulta relógio; as leituras existentes permanecem nos fluxos anteriores de relógio, GPS, retomada e feedback. (`T:252–271`, `T:163–170`, `T:311–323`, `T:353–368`) |
| Derivação paralela de fonte única | **NÃO PROCEDENTE.** A presença da prévia deriva diretamente de `_fotoBlob`; `_previaUrl` identifica o recurso visual, e `_camNegada` escolhe o aviso quando falta blob. (`T:251–267`) |
| Selo/RED sem mordida | **NÃO PROCEDENTE como teste apenas textual; PROCEDENTE como ausência de prova de mutação e cobertura incompleta.** Há medições de imagem, clique, troca e POST, mas não execução de mutantes dentro deste teste. (`S:99–120`, `S:171–177`) | [nao verificado]
| Campo com dois significados incompatíveis | **NÃO PROCEDENTE no contrato de envio.** O placeholder passa a conter também a prévia, mas nenhum consumidor do envio interpreta seu conteúdo. `_fotoBlob` continua sendo o material da foto; captura e fallback já produziam Blob/File. (`T−:252`, `T−:265`; `T:255–280`, `T:305`, `T:339`) |

**6. Outras telas e JS**

**NÃO PROCEDENTE — dependência localizada dos ids/markup substituído nas telas citadas.** Os consumidores externos examinados usam estes contratos:

| Arquivo | Contrato localizado |
|---|---|
| `static/pwa.js` | Recebe a foto como argumento, guarda em `foto` e envia multipart; não precisa do placeholder ou da imagem de prévia. (`P:107–118`, `P:143–156`) |
| `static/sw.js` | Trata método/URL/cache e solicita sincronização por mensagem, sem participar da pintura da câmera. (`app/static/sw.js:33–85`, `app/static/sw.js:112–125`) |
| `triagem/apto.html` | Usa estado e elementos próprios, como `camera`, `preview` e `foto-input`. (`app/templates/ponto/triagem/apto.html:138–150`, `app/templates/ponto/triagem/apto.html:167–190`, `app/templates/ponto/triagem/apto.html:229–232`) |
| `dashboard_colaborador.html` | Liga à rota da tela v2. (`app/templates/core/dashboard_colaborador.html:39–46`) |
| `dashboard_colaborador_v2.html` | Liga à mesma rota. (`app/templates/core/dashboard_colaborador_v2.html:31–38`) |

O teste novo é consumidor explícito do placeholder/input e pressupõe o pai como quadro da imagem. Dependências externas à cópia ou geradas dinamicamente: **[não verificado]**. (`S:80–94`) [nao verificado]

## 15/11 10:35 · sessao 3a515ace382d · - · - · origem=Ronald (tmux)

Use `T = app/templates/ponto/bater_ponto_v2.html`.

- **NÃO PROCEDENTE — aviso persistente após selfie no fluxo normal.** O handler só aceita arquivo existente, grava `_fotoBlob` e repinta; o ramo com blob apaga o aviso, insere a imagem e troca o chip conjuntamente (`T:269-271`, `T:255-261`).
- **NÃO PROCEDENTE — cancelar a segunda seleção perde a primeira selfie.** `if(!f)return` preserva `_fotoBlob`, URL, imagem e chip anteriores; cancelamento não significa “apagar selfie” nesse contrato (`T:270-271`).
- **NÃO PROCEDENTE — segunda selfie conserva imagem anterior.** Primeiro revoga a URL anterior, cria URL do novo blob e remove o filho antigo por `innerHTML=''` antes de anexar o novo (`T:254-261`).
- **NÃO PROCEDENTE — prévia com `_fotoBlob` nulo por algum escritor normal.** A imagem só é criada sob `if(_fotoBlob)`; os escritores são inicialização nula, arquivo recebido e captura com preservação do blob anterior; nenhum escritor posterior limpa o blob (`T:146`, `T:255-260`, `T:271`, `T:305`).
- **Limite PROCEDENTE — blob existente não garante imagem decodificável.** O handler não verifica conteúdo nem decodificação, e a pintura marca “selfie pronta” antes de `load`, sem `error` handler; um arquivo inválido pode dar imagem quebrada com chip pronto. Existência desse caso em produção: **[não verificado]** (`T:90`, `T:270-271`, `T:256-261`).
- **NÃO PROCEDENTE — fluxo ao vivo passa a depender da prévia.** Sucesso de `getUserMedia` continua exibindo vídeo, escondendo placeholder e marcando câmera ao vivo; `capturarFrame` continua lendo vídeo quando há largura e usando `_fotoBlob` quando não há quadro (`T:238-240`, `T:277-285`). Uma câmera que pare depois de ter funcionado não tem observador de tracks/error para corrigir o chip; esse limite já pertence ao fluxo ao vivo, não à pintura nova (`T:235-245`, `T:151`).
- **NÃO PROCEDENTE — bfcache conserva indefinidamente selfie antiga por decisão desta tela.** `pageshow.persisted` pede reload; retorno visível após mais de cinco minutos também pede reload; nova execução inicializa `_fotoBlob=null` e markup inicial. Comportamento efetivo no iPhone/PWA e restauração de campos pelo navegador: **[não verificado]** (`T:319-324`, `T:146`, `T:87-90`).
- **Observação PROCEDENTE — prévia posterior não identifica a foto já salva offline.** Os dois ramos offline escondem apenas o botão, mantendo input/quadro interativos; selecionar outra selfie muda `_fotoBlob` e prévia, enquanto a fila conserva a foto passada no registro anterior. Pode haver f2 visível junto do aviso “Batida registrada localmente” referente a f1; isso não demonstra mudança do envio pelo diff (`T:269-271`, `T:352-357`, `T:365-370`; `app/static/pwa.js:107-118`; aviso em `T:122-126`).
- **NÃO PROCEDENTE — `v2Sucesso` deixa chip divergente visível.** Ele para tracks e cobre a página com overlay antes do redirecionamento; não limpa estado interno nem URL. O vídeo/chip subjacentes podem ficar congelados, mas o overlay os cobre (`T:150-157`, chamada `T:346`).

Item 3:

- **PROCEDENTE apenas na ordem literal — URL revogada ainda referenciada pelo `<img>`.** Revogação em `T:254` precede remoção da imagem em `T:260`. Isso ocorre na mesma execução síncrona; imagem quebrada persistente no caminho normal não está demonstrada. Uma exceção entre essas linhas poderia conservar a referência antiga; ocorrência concreta: **[não verificado]** (`T:254-260`).
- **NÃO PROCEDENTE — acúmulo de `<img>` a cada selfie.** `ph.innerHTML=''` remove a imagem anterior; a URL anterior também é revogada (`T:254`, `T:260`).
- **PROCEDENTE — falta cleanup explícito da última URL.** Há revogação somente na próxima pintura; `v2Sucesso`, `pageshow` e `visibilitychange` não revogam nem limpam a última imagem. É retenção durante a vida do documento, não prova de vazamento ilimitado (`T:254`, `T:150-157`, `T:319-324`).
- **NÃO PROCEDENTE — declaração tardia de `var` provoca corrida com `abrirCamera`.** A inicialização em `T:251` acontece antes do registro do listener em `T:273`; declarar a função antes não a executa (`T:235`, `T:251`, `T:273`).
- **NÃO PROCEDENTE — `innerHTML` novo recebe valor não confiável.** `_camNegada` apenas seleciona entre duas strings constantes; nome, conteúdo e nome de arquivo não são concatenados no HTML; a imagem é criada por DOM e recebe URL gerada (`T:242`, `T:256-266`).

## 15/11 10:35 · sessao c0a8833e7ae8 · - · - · origem=Ronald (tmux)

Item 4 — **MORDE parcialmente; há lacunas procedentes, sem asserção globalmente vazia.** A conclusão abaixo é leitura estática da sonda; execução e resultado verde/RED **[não verificado]**. O teste criaria e removeria diretório temporário, e o helper escreveria HTML/symlink/cópia; não foi executado para preservar a regra de somente leitura (`app/ponto/tests/test_camera_alternativa_previa.py:163`, `app/ponto/tests/test_camera_alternativa_previa.py:167`, `app/core/tests/test_smoke_chromium.py:94`, `app/core/tests/test_smoke_chromium.py:100`).

| Asserção | O que alcança; mutante plausível |
|---|---|
| `assertIsNotNone(chromium())` | Cobra executável presente, sem skip; não prova que JS executou (`app/ponto/tests/test_camera_alternativa_previa.py:158`, `app/core/tests/test_smoke_chromium.py:76`). |
| `assertIn('id="v2-cam-fallback"', html)` | Cobra presença literal no HTML renderizado; sozinha aceitaria elemento inutilizável, mas a sonda cobra acesso DOM e change (`app/ponto/tests/test_camera_alternativa_previa.py:162`, `app/ponto/tests/test_camera_alternativa_previa.py:80`, `app/ponto/tests/test_camera_alternativa_previa.py:91`). |
| `assertIsNotNone(veredito)` | Cobra marcador emitido pela sonda; ausência de JS/veredito falha, sem verde por silêncio (`app/ponto/tests/test_camera_alternativa_previa.py:66`, `app/ponto/tests/test_camera_alternativa_previa.py:168`, `app/core/tests/test_smoke_chromium.py:123`). |
| `assertNotIn('erro', r)` | Cobra elemento presente, aviso inicial e ausência das exceções capturadas; aviso inicial é somente `textContent`, não visibilidade (`app/ponto/tests/test_camera_alternativa_previa.py:82`, `app/ponto/tests/test_camera_alternativa_previa.py:84`, `app/ponto/tests/test_camera_alternativa_previa.py:125`, `app/ponto/tests/test_camera_alternativa_previa.py:176`). |
| `aviso=False` | Remove palavras do aviso do texto de todo quadro; trocar somente texto poderia passar esta parcela, mas falharia as parcelas de imagem (`app/ponto/tests/test_camera_alternativa_previa.py:77`, `app/ponto/tests/test_camera_alternativa_previa.py:99`, `app/ponto/tests/test_camera_alternativa_previa.py:131`). |
| `previa=True` | Exige primeiro `img` do pai de `ph` com URL `blob:`; não exige imagem descendente de `#v2-cam-ph` nem id `v2-cam-previa` (`app/ponto/tests/test_camera_alternativa_previa.py:83`, `app/ponto/tests/test_camera_alternativa_previa.py:94`, `app/ponto/tests/test_camera_alternativa_previa.py:100`). |
| `decodificada=True` | Exige imagem completa com largura natural 4; mata simples texto e imagem quebrada, mas não compara bytes, pixels nem altura (`app/ponto/tests/test_camera_alternativa_previa.py:89`, `app/ponto/tests/test_camera_alternativa_previa.py:95`, `app/ponto/tests/test_camera_alternativa_previa.py:101`). |
| `area=True` | Cobra área geométrica positiva; não verifica `visibility`, opacidade ou oclusão da imagem (`app/ponto/tests/test_camera_alternativa_previa.py:102`, `app/ponto/tests/test_camera_alternativa_previa.py:103`). |
| `toque=True` | Faz hit test real no centro de `ph` e cobra uma chamada ao `inp.click` substituído; mata perda do caminho de clique nesse ponto, mas não exige propriedade `ph.onclick`, nem abertura real da câmera do sistema (`app/ponto/tests/test_camera_alternativa_previa.py:87`, `app/ponto/tests/test_camera_alternativa_previa.py:104`, `app/ponto/tests/test_camera_alternativa_previa.py:108`). |
| `troca=True` | Exige URL blob diferente na segunda imagem; mata prévia estagnada, mas não prova local DOM ou conteúdo por si só (`app/ponto/tests/test_camera_alternativa_previa.py:109`, `app/ponto/tests/test_camera_alternativa_previa.py:113`). |
| `revoga=True` | Mata omissão da revogação anterior; não examina ordem relativa à retirada da imagem, nem se a URL nova também foi revogada indevidamente (`app/ponto/tests/test_camera_alternativa_previa.py:85`, `app/ponto/tests/test_camera_alternativa_previa.py:86`, `app/ponto/tests/test_camera_alternativa_previa.py:114`). |
| `decodificada2=True` | Cobra largura natural 8 e imagem completa; não compara bytes/altura e não mede área nem clique depois da segunda selfie (`app/ponto/tests/test_camera_alternativa_previa.py:110`, `app/ponto/tests/test_camera_alternativa_previa.py:115`). |
| `aviso2=False` | Cobra ausência textual depois da segunda selfie; novamente não inspeciona chip ou visibilidade (`app/ponto/tests/test_camera_alternativa_previa.py:116`). |
| `envio=True` | Cobra exatamente um POST anotado, foto presente e igualdade de tamanho/tipo com a segunda selfie; mata ausência de foto/POST e primeira foto com tamanho diferente, mas não compara conteúdo (`app/ponto/tests/test_camera_alternativa_previa.py:60`, `app/ponto/tests/test_camera_alternativa_previa.py:62`, `app/ponto/tests/test_camera_alternativa_previa.py:118`, `app/ponto/tests/test_camera_alternativa_previa.py:120`). |
| `assertEqual(r, ESPERADO)` | Cobra todos os dez resultados e conjunto exato de chaves; não há passe apenas por ausência do aviso (`app/ponto/tests/test_camera_alternativa_previa.py:131`, `app/ponto/tests/test_camera_alternativa_previa.py:177`). |

**PROCEDENTE — alcance mais amplo anunciado que o efetivamente medido:** uma prévia irmã de `ph`, dentro do quadro e posicionada fora do centro clicado, com `ph` ainda abrindo input, poderia passar; a busca é pelo pai e o clique por um ponto de `ph`, sem verificar descendência ou `onclick` da imagem (`app/ponto/tests/test_camera_alternativa_previa.py:83`, `app/ponto/tests/test_camera_alternativa_previa.py:94`, `app/ponto/tests/test_camera_alternativa_previa.py:106`). O chip também não é consultado pela sonda (`app/ponto/tests/test_camera_alternativa_previa.py:80`, `app/ponto/tests/test_camera_alternativa_previa.py:120`).

**PROCEDENTE — identidade da segunda foto insuficiente:** um mutante que envia outro blob de mesmo tamanho e MIME passa `envio`; não há comparação dos bytes da segunda foto (`app/ponto/tests/test_camera_alternativa_previa.py:120`). O mutante simples “guardar primeira foto e mostrar segunda” é detectado se os PNG gerados tiverem tamanhos diferentes; os tamanhos efetivos nesta execução **[não verificado]** (`app/ponto/tests/test_camera_alternativa_previa.py:70`, `app/ponto/tests/test_camera_alternativa_previa.py:95`, `app/ponto/tests/test_camera_alternativa_previa.py:110`).

**NÃO PROCEDENTE — sonda tentando ler variável privada inexistente:** `_fotoBlob` está no IIFE, mas a sonda chama a função global `window.v2Confirmar`, que usa o fecho, passa por `garantirRequisitos`, cai no fallback de `capturarFrame` quando vídeo não tem quadro e coloca `_fotoBlob` em `FormData`; o fetch interceptado observa esse valor por um leitor válido (`app/templates/ponto/bater_ponto_v2.html:138`, `app/templates/ponto/bater_ponto_v2.html:146`, `app/templates/ponto/bater_ponto_v2.html:278`, `app/templates/ponto/bater_ponto_v2.html:305`, `app/templates/ponto/bater_ponto_v2.html:326`, `app/templates/ponto/bater_ponto_v2.html:339`, `app/ponto/tests/test_camera_alternativa_previa.py:62`).

**NÃO PROCEDENTE — apenas uma casca no código do teste:** percorre `base.html` e `base_app.html`, remapeia o nome `base.html` no loader locmem para o conteúdo da casca escolhida, e renderiza o template que estende esse nome; ambas têm bloco `content`, que inclui o script da página (`app/ponto/tests/test_camera_alternativa_previa.py:38`, `app/ponto/tests/test_camera_alternativa_previa.py:141`, `app/ponto/tests/test_camera_alternativa_previa.py:148`, `app/ponto/tests/test_camera_alternativa_previa.py:172`, `app/templates/ponto/bater_ponto_v2.html:1`, `app/templates/ponto/bater_ponto_v2.html:3`, `app/templates/base.html:336`, `app/templates/base_app.html:94`). A seleção real de casca por sessão/rota não é exercitada: usa renderização direta e `AnonymousUser` (`app/ponto/tests/test_camera_alternativa_previa.py:148`, `app/ponto/tests/test_camera_alternativa_previa.py:150`).

**PROCEDENTE — não há prova de mutação RED neste teste:** o arquivo tem cenário verde esperado, mas não reinjeta cura defeituosa nem exige falha de mutante; o nome `test_MORDE` não acrescenta essa execução (`app/ponto/tests/test_camera_alternativa_previa.py:171`, `app/ponto/tests/test_camera_alternativa_previa.py:177`). Diferentemente, o helper smoke possui outro teste que injeta defeito explicitamente, mas esse teste não é chamado pelo teste da câmera (`app/core/tests/test_smoke_chromium.py:191`, `app/core/tests/test_smoke_chromium.py:212`, `app/ponto/tests/test_camera_alternativa_previa.py:35`).

**PROCEDENTE — caminhos ausentes da cobertura:** a câmera sempre rejeita, rede sempre é marcada online e o POST recebe HTTP 599; logo esse caso não cobre câmera funcionando, offline/fila, sucesso, cancelamento, pageshow/bfcache ou reload (`app/ponto/tests/test_camera_alternativa_previa.py:48`, `app/ponto/tests/test_camera_alternativa_previa.py:57`, `app/ponto/tests/test_camera_alternativa_previa.py:63`, `app/ponto/tests/test_camera_alternativa_previa.py:88`, `app/ponto/tests/test_camera_alternativa_previa.py:118`). Os erros de console retornados pelo helper são descartados (`app/ponto/tests/test_camera_alternativa_previa.py:165`, `app/core/tests/test_smoke_chromium.py:124`).

## 15/11 10:35 · sessao af0477d00d94 · - · - · origem=Ronald (tmux)

**Item 5 — classes da casa**

| Classe | Veredito sobre o diff | Evidência |
|---|---|---|
| Segundo juiz registrado | **NÃO PROCEDENTE.** O juiz registrado `selfie_ausente` decide ausência de foto da batida persistida e prazo do canal; o pintor apenas apresenta a selfie local já guardada. | `app/core/juizes.py:1202-1205`; `app/templates/ponto/bater_ponto_v2.html:255-267`. |
| Segundo escritor fora da porta | **NÃO PROCEDENTE para envio/persistência.** O handler continua atribuindo o arquivo a `_fotoBlob`; o novo pintor lê esse estado, cria a URL e escreve quadro/chip. As portas existentes continuam lendo `_fotoBlob` para FormData e fila. | `app/templates/ponto/bater_ponto_v2.html:254-271`, `:305`, `:339`, `:352`, `:365`; `app/static/pwa.js:107-118`. |
| Relógio fora da borda | **NÃO PROCEDENTE no diff.** O novo pintor não lê relógio; leituras existentes permanecem em relógio/GPS/retomada/feedback, fora do trecho alterado fornecido. | `app/templates/ponto/bater_ponto_v2.html:252-271`, `:163-170`, `:177-178`, `:311-323`, `:353-368`; comparação com `diff_v1.txt` anexado. |
| Derivação paralela de fonte única | **NÃO PROCEDENTE como nova fonte de verdade.** A decisão “prévia ou aviso” deriva diretamente de `_fotoBlob`; `_previaUrl` guarda o recurso visual, `_camNegada` seleciona um de dois avisos literais. | `app/templates/ponto/bater_ponto_v2.html:251-267`. |
| Selo/RED sem mordida | A análise detalhada cabe ao agente do teste; **não há prova de execução RED/GREEN no dado fornecido [não verificado]**. O teste contém verificações de prévia, troca, revogação e envio, portanto a acusação genérica de “só selo textual” não corresponde à implementação. | `app/ponto/tests/test_camera_alternativa_previa.py:99-125`, `:131-134`, `:170-177`. |
| Campo com dois significados | **NÃO PROCEDENTE como incompatibilidade de contrato.** `#v2-cam-ph` passa de placeholder/aviso para contêiner de prévia; o leitor de captura usa `#v2-cam`, não o conteúdo do quadro. `_fotoBlob` continua sendo o material enviado; File e Blob já são os valores originados pelo fallback e captura. | `app/templates/ponto/bater_ponto_v2.html:89`, `:255-271`, `:275-286`, `:305`, `:339`. |

**Item 6 — dependências externas**

- **NÃO PROCEDENTE: leitores externos dos ids/markup trocado.** A busca textual em `app` por `v2-cam` localizou os ids no próprio template e no teste novo; não localizou leitores executáveis nas outras telas/JS citados. A ausência é delimitada à cópia e à busca literal; dependências por código gerado ou serviços externos **[não verificado]**. Definição/escrita dos ids: `app/templates/ponto/bater_ponto_v2.html:87-90`, `:236-277`; consumo do teste: `app/ponto/tests/test_camera_alternativa_previa.py:80-81`, `:162`.
- `static/pwa.js` recebe a foto como argumento, grava-a em IndexedDB e depois a põe no FormData; não precisa inspecionar a prévia nem o placeholder. **NÃO PROCEDENTE.** `app/static/pwa.js:107-118`, `:143-156`.
- `sw.js` seleciona tratamento por método/URL e comunica sincronização por mensagem; a troca interna do quadro não participa desses contratos. **NÃO PROCEDENTE.** `app/static/sw.js:33-85`, `:112-125`; `app/static/pwa.js:16`.
- `triagem/apto.html` possui seu próprio estado e ids (`camera`, `preview`, `foto-input`, `camera-off`), sem consumir `v2-cam-*`. **NÃO PROCEDENTE.** `app/templates/ponto/triagem/apto.html:138-150`, `:167-186`, `:229-232`, `:242-282`.
- Ambos os dashboards apenas apontam para a rota `ponto:bater_ponto_v2`; não precisam do markup do quadro. **NÃO PROCEDENTE.** `app/templates/core/dashboard_colaborador.html:39-46`; `app/templates/core/dashboard_colaborador_v2.html:31-38`.

**Limite da comparação:** esta cópia não tem repositório Git utilizável pelo comando executado; comparação com os commits depende do diff anexado **[não verificado independentemente]**. [nao verificado]

## 15/11 10:34 · sessao ef7e8a8767ea · FATO-DE-AUSENCIA-ACORDA-A-PROJECAO · verificador · origem=Code

1. **Sim, a consulta é válida por análise estática para o Django 5.2.14 declarado.** `values_list` seleciona inclusive a PK da própria `Ausencia`, compatível com `of=('self',)`; o filtro usa a FK diretamente, e `order_by('pk')` substitui o `Meta.ordering` que levaria a `colaborador__nome`. A forma resultante termina em `ORDER BY id ASC FOR UPDATE OF ponto_ausencia`. Compilação e execução reais no Django/Postgres: **[nao verificado]**. (`app/requirements.txt:13`; `app/ponto/services/ausencia.py:1416`; `app/ponto/services/ausencia.py:1419`; `app/ponto/services/ausencia.py:1421`; `app/ponto/models.py:829`.)

   Os bloqueios de linha abrangem as Ausencias **daquele agendamento**, tipo `ferias`, com status diferente de `rejeitada`, em PK crescente. Não abrangem Colaborador, Agendamento ou Periodo por essa consulta. O `UPDATE pk__in` posterior encontra essas linhas já travadas; sua própria ordem de execução não introduz outra aquisição entre elas. (`app/ponto/services/ausencia.py:1420`; `app/ponto/services/ausencia.py:1421`; `app/ponto/services/ausencia.py:1425`.)

   Comparação com os demais usos efetivos de `of=('self',)`:

   - `trava_na_ordem`: também combina PK crescente com `values_list('pk')`, sobre `PerguntaDisputa`. (`app/chamados/models.py:2770`.)
   - `trocar_plantao`: carrega instâncias e faz `select_related('colaborador')`, mas restringe o bloqueio às Ausencias; ordena por PK. (`app/ponto/portas/celula.py:695`.)
   - H06: busca uma única Ausencia por PK, remove a ordenação padrão e restringe o bloqueio a `self`. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:138`.)

2. **Não identifiquei ordem cruzada entre essas Ausencias nos fluxos normais examinados.** A rejeição agora segue PK crescente; o backfill e o registro normalmente consumido pela reversão seguem essa mesma ordem. (`app/ponto/services/ausencia.py:1420`; `app/ponto/services/ausencia.py:1608`; `app/ponto/services/ausencia.py:1619`; `app/ponto/services/ausencia.py:1650`.)

   - **Backfill:** trava Agendamentos e respectivos Periodos pelo join, ordenando por PK de Agendamento; depois trava Ausencias de férias sem origem por PK crescente, antes dos updates individuais. (`app/ponto/services/ausencia.py:1601`; `app/ponto/services/ausencia.py:1607`; `app/ponto/services/ausencia.py:1622`.)
   - **Reversão:** cada `UPDATE` por PK adquire o bloqueio correspondente. Consome a ordem do JSON, sem ordenar novamente; o arquivo produzido pelo apply herda a ordem crescente das Ausencias. (`app/ponto/services/ausencia.py:1537`; `app/ponto/services/ausencia.py:1479`; `app/ponto/services/ausencia.py:1619`; `app/ponto/services/ausencia.py:1650`.)
   - **Cancelamento admin:** adquire Agendamento → Periodo → Ausencias, chamando esta função depois das duas primeiras travas. (`app/ferias/services.py:636`; `app/ferias/services.py:639`; `app/ferias/services.py:679`; `app/ferias/services.py:683`.)
   - **Troca de plantão:** pode tecnicamente travar férias recebidas como cartas, pois não filtra tipo; adquire as Ausencias por PK crescente, depois as células por colaborador/data. (`app/ponto/portas/celula.py:695`; `app/ponto/portas/celula.py:711`.)
   - **H06:** trava uma linha por transação e confere sua identidade, incluindo tipo; divergência impede a edição. As portas genéricas de edição e mudança de estado recusam férias. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:137`; `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:142`; `app/ponto/services/ausencia.py:579`; `app/ponto/services/ausencia.py:920`.)

   Há também updates históricos por PK nas migrations, fora deste delta, sem exclusão explícita de férias. (`app/ponto/migrations/0057_a4_backfill_pendente.py:43`; `app/ponto/migrations/0058_a4_relogio_recomeca.py:42`; `app/ponto/migrations/0059_a4_d0_no_primeiro_dia_do_cron.py:36`.)

3. **Os dois asserts mordem a regressão pretendida e nenhum exige um resultado falso.** A execução integral da suíte ORM/Postgres permanece **[nao verificado]**. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:141`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:271`.)

   **Editar:** o resultado legítimo atual é `data_fim=None`: um dia deriva `None`, a edição grava `dias_corridos=1`, e o modelo só recompõe fim vazio quando há mais de um dia. A releitura rejeita o antigo `21/10`. No ensaio dos corpos reais em memória, o código atual passou; retirar o `save()` deixou `21/10` e derrubou o assert. (`app/ponto/services/ausencia.py:164`; `app/ponto/services/ausencia.py:606`; `app/ponto/services/ausencia.py:615`; `app/ponto/models.py:912`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:140`.)

   **Desfazer:** a fixture nasce aprovada; retratar grava `False`; desfazer grava expressamente `True` e limpa `retratada_em`. No ensaio em memória, remover apenas a restauração de `aprovada` preservou `False` e derrubou o assert novo, mesmo limpando a retratação. (`app/ponto/models.py:63`; `app/ponto/registro_batida.py:276`; `app/ponto/services/retratar_humano.py:207`; `app/ponto/registro_batida.py:256`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:271`.)

4. **Nenhum outro defeito funcional identificado no delta fornecido.** A ressalva é o alcance do comentário “nunca”: a ordenação sustenta a aquisição dessas Ausencias, mas não constitui garantia global contra deadlocks envolvendo outras tabelas ou travas previamente adquiridas pelo chamador. (`app/ponto/services/ausencia.py:1418`; `app/ponto/services/ausencia.py:1420`; `app/ferias/services.py:679`.)

   Completude da comparação direta entre `1d806e163` e o hash informado, além do diff anexado: **[nao verificado]**.

## 15/11 10:33 · sessao b0adef3e92f4 · - · - · origem=Ronald (tmux)

Os dois asserts são corretos e mordem a regressão pretendida; a execução da suíte ORM/Postgres permanece **[nao verificado]**. [nao verificado]

- **Editar:** o estado legítimo atual é `data_fim=None`: `derivar_data_fim` retorna `None` para um dia; a edição atribui `dias_corridos=1`, atribui o fim e salva; `Ausencia.save` só recompõe fim vazio quando dias > 1. O assert aceita esse estado e rejeita o antigo `2026-10-21`. (`app/ponto/services/ausencia.py:154`, `app/ponto/services/ausencia.py:606`, `app/ponto/services/ausencia.py:615`, `app/ponto/models.py:903`, `app/ponto/models.py:912`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:141`.)

- **Desfazer:** a fixture nasce aprovada pelo default; retratar grava `aprovada=False`; desfazer manda gravar expressamente `aprovada=True` e `retratada_em=None`; `anotar` persiste ambos com `save(update_fields=...)`. Portanto o novo assert não exige um estado falso para essa fixture. Um teste anterior já cobra a mesma aprovação após desfazer. (`app/ponto/models.py:63`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:256`, `app/ponto/registro_batida.py:276`, `app/ponto/services/retratar_humano.py:207`, `app/ponto/registro_batida.py:254`, `app/ponto/registro_batida.py:256`, `app/ponto/tests/test_porta_retratar_batida.py:138`, `app/ponto/tests/test_porta_retratar_batida.py:142`.)

Executei os corpos reais extraídos por AST, com persistência e dependências simuladas em memória: editar atual → `None`, assert passa; mutante sem `ausencia.save()` → releitura `21/10`, assert falha. Desfazer atual → `True/None`, assert passa; mutante removendo apenas a chave `aprovada` → `False/None`, assert falha. Essa checagem comprova a mordida isolada dos asserts; integração com ORM, signals e banco **[nao verificado]**. (`app/ponto/services/ausencia.py:615`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:140`, `app/ponto/services/retratar_humano.py:207`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:269`.)

## 15/11 10:33 · sessao aa7283c56f82 · - · - · origem=Ronald (tmux)

- **Backfill:** dentro de `atomic`, trava agendamentos e períodos pelo join, ordenando por PK de agendamento; depois trava todas as Ausencias de férias sem origem por PK crescente e atualiza uma a uma nessa ordem (`app/ponto/services/ausencia.py:1599`, `:1601`, `:1607`, `:1622`, `:1624`).
- **Reversão:** faz `UPDATE` por PK, sem `select_for_update`, na ordem dos pares do registro; o registro produzido pelo apply herda a ordem crescente das Ausencias (`app/ponto/services/ausencia.py:1479`, `:1608`, `:1619`, `:1646`, `:1650`, `:1537`).
- **Cancelamento administrativo:** trava Agendamento → Periodo antes de chamar a rejeição; a rejeição passa a travar Ausencias por PK crescente (`app/ferias/services.py:636`, `:639`, `:679`, `:683`; `app/ponto/services/ausencia.py:1419`).
- **Troca de plantão:** pode receber Ausencias de qualquer tipo, pois a consulta não filtra `tipo`; trava as duas cartas por PK crescente, somente `self`, e depois as células por colaborador/data. Não cria inversão entre PKs de Ausencia frente à rejeição (`app/ponto/portas/celula.py:695`, `:696`, `:708`, `:711`).
- **H06:** trava somente uma Ausencia por transação; sua lista é de atestados/faltas e a identidade inclui tipo, portanto uma linha que virou férias é pulada antes da edição (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:50`, `:67`, `:137`, `:139`, `:142`, `:146`).
- **Portas genéricas:** edição, mudança de estado, anexo, decisão e reversão da rejeição recusam férias antes da gravação (`app/ponto/services/ausencia.py:579`, `:920`, `:966`, `:1017`, `:1209`; guarda em `:92`).
- **Migrações históricas:** também contêm updates por PK, sem filtro explícito excluindo férias, seguindo a iteração do queryset; não são consumidores novos do delta (`app/ponto/migrations/0057_a4_backfill_pendente.py:35`, `:43`; `0058_a4_relogio_recomeca.py:37`, `:42`, `:59`, `:63`; `0059_a4_d0_no_primeiro_dia_do_cron.py:32`, `:36`).

**Conclusão restrita ao delta:** não identifiquei ordem cruzada entre as Ausencias da rejeição e os registros normalmente produzidos pelo backfill/reversão; todos seguem PK crescente (`app/ponto/services/ausencia.py:1420`, `:1608`, `:1619`, `:1650`). A frase “nunca se travam em ordem cruzada” não prova ausência global de deadlocks: backfill adquire múltiplos agendamentos e seus períodos por join, enquanto cancelamento adquire um agendamento e depois seu período; eventual ciclo com período compartilhado seria preexistente e anterior à entrada em `rejeitar_ausencias_de_ferias` (`app/ponto/services/ausencia.py:1601`; `app/ferias/services.py:636`, `:639`, `:683`); ocorrência/SQL executado [nao verificado].

## 15/11 10:32 · sessao edbbd489bd52 · - · - · origem=Ronald (tmux)

Nenhum outro defeito funcional identificado no delta fornecido: a alteração de produção restringe e ordena a seleção das Ausencias; as outras duas alterações acrescentam verificações de persistência. (`app/ponto/services/ausencia.py:1416`, `app/ponto/services/ausencia.py:1419`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:140`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:271`)

Ressalva: a frase “duas chamadas nunca se travam em ordem cruzada” não demonstra ausência global de deadlock envolvendo outras tabelas ou uma transação externa; o trecho ordena apenas a aquisição dessas Ausencias. (`app/ponto/services/ausencia.py:1418`, `app/ponto/services/ausencia.py:1420`)

A completude do delta entre os hashes, além do anexo fornecido, permanece [nao verificado].

## 15/11 10:28 · sessao 80cd60d8871b · FATO-DE-AUSENCIA-ACORDA-A-PROJECAO · verificador · origem=Code

**O rollback protege o cancelamento inteiro no chamador produtivo; os testes ainda deixam passar regressões de concorrência e atomicidade do lote.** Evidências: `app/ferias/services.py:674`, `app/ferias/services.py:683`, `app/ponto/services/ausencia.py:1416`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:198`. Execução dos testes, mutantes e concorrência: [não verificado].

1. **Chamadores, transações e travas**

   O único chamador direto produtivo encontrado é `executar_cancelamento_admin`: abre `atomic`, trava **agendamento → período**, devolve saldo e chama a rejeição. A função acrescenta um `atomic` interno e o `select_for_update` das Ausências. Portanto, nesse caminho, a ordem é **agendamento → período → Ausências** (`app/ferias/services.py:674`, `app/ferias/services.py:679`, `app/ferias/services.py:683`, `app/ferias/services.py:636`, `app/ferias/services.py:639`, `app/ponto/services/ausencia.py:1416`).

   As entradas indiretas são cancelamento individual, correção de concluída e cancelamento em massa, pelo adaptador da view; há também a simulação de `listar_ferias_dp`, sob outro `atomic` (`app/ferias/views.py:396`, `app/ferias/views.py:444`, `app/ferias/views.py:605`, `app/ferias/views.py:557`, `app/ferias/management/commands/listar_ferias_dp.py:98`). Os demais chamadores diretos são os testes, nas linhas 180, 191 e 200 (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:180`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:191`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:200`).

   Quando `evento` falha, seu próprio savepoint desfaz a tentativa de trilha; a exceção é engolida e ele retorna `None`. A rejeição transforma esse retorno em `RuntimeError` **dentro** do seu `atomic`: desfaz status, `atualizado_em` e quaisquer trilhas anteriores daquela chamada; não alcança `_acordar_projecao` nem retorna `n` (`app/core/observ.py:87`, `app/core/observ.py:105`, `app/core/observ.py:133`, `app/core/observ.py:139`, `app/ponto/services/ausencia.py:1423`, `app/ponto/services/ausencia.py:1428`, `app/ponto/services/ausencia.py:1433`, `app/ponto/services/ausencia.py:1435`).

   No chamador produtivo, a exceção também sai do `atomic` externo: desfaz a devolução de saldo já salva e impede a gravação de `status='cancelado'`, observação e retorno normal (`app/ferias/services.py:680`, `app/ferias/services.py:682`, `app/ferias/services.py:683`, `app/ferias/services.py:687`, `app/ferias/services.py:690`). As três views capturam somente `AgendamentoNaoCancelavel`; o `RuntimeError` escapa desses tratamentos. A resposta HTTP efetiva é [não verificado] (`app/ferias/views.py:397`, `app/ferias/views.py:445`, `app/ferias/views.py:606`). A simulação captura a exceção, imprime `ERRO:RuntimeError:...` e recarrega o agendamento (`app/ferias/management/commands/listar_ferias_dp.py:112`).

   **Há risco de inversão entre linhas de Ausência**, embora o caminho principal respeite a hierarquia agendamento/período/Ausência. O novo SELECT não fixa `order_by`, herdando `-data, colaborador__nome`; a reversão do backfill atualiza as mesmas Ausências uma por vez, na ordem do arquivo, sem tomar antes as travas de agendamento/período (`app/ponto/services/ausencia.py:1417`, `app/ponto/models.py:829`, `app/ponto/services/ausencia.py:1647`, `app/ponto/services/ausencia.py:1535`). O apply produz o registro a partir de linhas ordenadas por PK (`app/ponto/services/ausencia.py:1605`, `app/ponto/services/ausencia.py:1617`).

   Interleaving plausível: A1 tem PK menor/data anterior; A2, PK maior/data posterior. Cancelamento trava A2 e espera A1; reversão trava A1 e espera A2. As travas anteriores do chamador não serializam a reversão, que não as solicita. Esse deadlock é uma possibilidade deduzida dessas ordens; ocorrência real [não verificado] (`app/ponto/models.py:829`, `app/ponto/services/ausencia.py:1648`).

   Também há possível ampliação da trava ao colaborador: a ordenação usa `colaborador__nome` e o SELECT não restringe `of=('self',)`. É inferência do ORM; SQL efetivo e bloqueios observados [não verificado] (`app/ponto/models.py:829`, `app/ponto/services/ausencia.py:1417`).

2. **Os quatro testes novos/alterados mordem, com estas lacunas** [nao verificado]

   Os mutantes abaixo são deduções estáticas; sobrevivência executada [não verificado].

   | Teste | O que verifica / mutante que passa | [nao verificado]
   |---|---|
   | Encolher o fim | Confere a fixture inicial e os dois carimbos, mas não relê o fato depois da edição. **Retirar `ausencia.save()`**, mantendo a alteração em memória e `_acordar_projecao`, passa sem persistir a edição (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:137`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:139`, `app/ponto/services/ausencia.py:604`, `app/ponto/services/ausencia.py:615`, `app/ponto/services/ausencia.py:625`). |
   | Rejeitar com trilha por dia | A nova comparação dos PKs mata “cinco trilhas, mas uma Ausência duplicada e outra omitida”. **Retirar `select_for_update`** continua passando: as duas chamadas são sequenciais (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:182`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:186`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:191`, `app/ponto/services/ausencia.py:1417`). |
   | Falha de trilha desfaz rejeição | O mock falha sempre, portanto já na primeira trilha. **Trocar o atomic do conjunto por um atomic por Ausência** passa: a primeira desfaz e não sobra rejeitada. Falhar só na segunda/quinta deixaria anteriores confirmadas, caso não exercitado (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:169`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:198`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:201`, `app/ponto/services/ausencia.py:1416`). |
   | Desfazer retratação | A releitura nova mata um falso desfazer que apenas chama recálculo. **Retirar `'aprovada': True`** do `anotar` passa, conservando `aprovada=False`: o teste só exige chamada e `retratada_em=None` (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:265`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:267`, `app/ponto/services/retratar_humano.py:207`, `app/ponto/registro_batida.py:276`). |

   **Sim, o patch alcança o nome usado.** O import de `evento as _ev_rej` é local à função e acontece enquanto o `mock.patch('core.observ.evento')` está ativo; não existe alias congelado no módulo nesse caminho (`app/ponto/services/ausencia.py:1414`, `app/ponto/services/ausencia.py:1428`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:198`).

3. **A fixture de edição chega ao helper, pela leitura estática**

   A criação com três dias deriva `data_fim=21/10`; o teste confirma isso antes de carimbar. A edição com um dia deriva fim nulo, salva e chama o helper com os fatos **anterior e posterior** (`app/ponto/models.py:912`, `app/ponto/models.py:917`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:134`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:137`, `app/ponto/services/ausencia.py:164`, `app/ponto/services/ausencia.py:615`, `app/ponto/services/ausencia.py:625`).

   Não há recusa por tranca nessa porta; a sobreposição exclui o próprio PK. A fixture não acrescenta outra Ausência, e essa função não exige documento nesse percurso (`app/ponto/services/ausencia.py:591`, `app/ponto/services/ausencia.py:593`, `app/ponto/turnos.py:2830`, `app/ponto/turnos.py:2851`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:45`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:134`, `app/ponto/services/ausencia.py:570`).

   `atestado` consta no seed ativo; a validação consulta o catálogo vigente. Não há indicação nessa fixture de catálogo que o exclua, mas o estado efetivo do catálogo/cache durante uma execução é [não verificado] (`app/ponto/migrations/0052_seed_tipoausencia.py:17`, `app/ponto/migrations/0052_seed_tipoausencia.py:36`, `app/ponto/catalogo/ausencias.py:278`, `app/ponto/catalogo/ausencias.py:284`, `app/ponto/services/ausencia.py:580`).

4. **Outro defeito exposto pelo delta: falha sem tratamento no lote**

   A nova exceção interrompe o cancelamento em massa antes da mensagem de resultado. Cada item usa sua própria unidade atômica; o loop só trata `AgendamentoNaoCancelavel`. Assim, sem transação externa adicional, itens anteriores permanecem confirmados, o item falho desfaz e os seguintes não são processados. Uma transação externa de request efetivamente configurada é [não verificado] (`app/ferias/views.py:600`, `app/ferias/views.py:605`, `app/ferias/views.py:606`, `app/ferias/views.py:611`, `app/ferias/services.py:674`).

   Os testes do delta chamam diretamente a rejeição; não verificam esse tratamento nas views nem o rollback conjunto de saldo/agendamento no chamador produtivo (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:180`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:200`).

_5599 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
