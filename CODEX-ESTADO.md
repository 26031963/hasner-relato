# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 21:17** · publicado em **14/11/2026 21:20** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 14/11 21:17 · sessao 6670240d856c · E3-V10-DEGRAU-DONO-E-PORTA · construtor · origem=Code

Entrega em [e3_lu3_entrega.txt:1](chamados/tests/e3_lu3_entrega.txt:1), com tabela dos 55, diffs, censos e comandos.

- Execução local: [168 testes sem banco verdes](chamados/tests/e3_lu3_suite.txt:176).
- Mutantes: [60/60 mortos, com controle verde](chamados/tests/e3_lu3_mutantes.txt:1954).
- ORM e pilhas SQL de P5: [não verificado]; comandos no [relato:129](chamados/tests/e3_lu3_entrega.txt:129).

Permanece uma pergunta de desenho: **C11 deve aceitar o M1 sem âncora?** O filtro atual exige âncora terminal; preservei os asserts positivos e não alterei essa regra. [cadastro_realidade.py:496](escala/services/cadastro_realidade.py:496).

## 14/11 20:53 · sessao 1ed7d5734d2d · JUSTIFICATIVA-PAGA-O-PERIODO · verificador · origem=Code

**Reprovo a c7 por dois bloqueios:** o motor deixa de ler o saldo da fonte nos dias sem trabalho; e o excedente real não tem entrega garantida à lista do admin. Há também repetição relevante dessas apurações, inclusive no caminho disparado pela escrita de batida. (`app/ponto/services/efeito_ausencia.py:48`, `app/ponto/motor_calculo_v2.py:2628`, `app/ponto/services/espelho.py:724`, `app/ponto/registro_batida.py:160`)

A identidade Git da cópia com `c9e419f39` e a comparação histórica com `8928f955a` são **[não verificado]**: a cópia não contém `.git`. As remoções abaixo foram confrontadas com o delta fornecido; a implementação atual foi aberta na cópia. Banco, logs e execução de produção: **[não verificado]**.

**P5 — chamadores e custo**

Chamadores diretos encontrados no código de produção, incluindo comandos:

| Função chamada | Chamadores |
|---|---|
| `periodos_justificados` | `efeitos_da_ausencia`, `_marcar_ausencias`, `grade_do_fechamento`, `montar_dias`. (`app/ponto/services/efeito_ausencia.py:46`; `app/escala/utils.py:1198`; `app/folha/export.py:339`; `app/ponto/services/espelho.py:520`) |
| `efeitos_da_ausencia` | Coletor PDF, `assinaturas_do_colab`, recálculo do fechamento, `autoridade_do_periodo`, comando `diff_espelho_alimentacao`, comando `diff_extrato_parcial`. (`app/relatorios/pdf_espelho.py:333`; `app/ponto/services/esmeril_espelho.py:161`; `app/ponto/services/fechamento.py:665`; `app/ponto/services/espelho.py:1071`; `app/ponto/management/commands/diff_espelho_alimentacao.py:117`; `app/relatorios/management/commands/diff_extrato_parcial.py:41`) |

`grade_da_celula` deixou de consultar pagamento implicitamente: agora recebe o mapa ou usa `{}`. Isso retira essa consulta dos leitores de lâmpadas, mas os leitores financeiros continuam pedindo o pagamento separadamente. (`app/escala/services/leitor_celula.py:369`, `app/escala/services/leitor_celula.py:381`; `app/folha/export.py:339`; `app/ponto/services/espelho.py:520`)

Nas contagens seguintes, **E** = chamada de `efeitos_da_ausencia`; **P** = chamada de `periodos_justificados`, já contando o P interno de cada E. São contagens estruturais por passagem completa, sem exceção; frequência efetiva em produção: **[não verificado]**. (`app/ponto/services/efeito_ausencia.py:46`)

| Caminho por colaborador | E | P | Evidência |
|---|---:|---:|---|
| `autoridade_do_periodo` | 1 | 1 | `app/ponto/services/espelho.py:1071` |
| Recálculo do fechamento, inclusive leitura | 1 | 2 | E em `app/ponto/services/fechamento.py:665`; grade em `app/ponto/services/fechamento.py:1019` → `app/folha/export.py:339` |
| `grade_do_fechamento` isolada | 0 | 1 | `app/folha/export.py:339` |
| Builder por turno isolado | 0 | 1 | `app/escala/utils.py:1868` → `app/escala/utils.py:1198` |
| Lista de fechamento | 0 | 0 ou 1 | Prontidão em `app/ponto/services/fechamento_lista.py:85`; snapshot retorna antes em `app/folha/export.py:1048`; alternativa consulta grade em `app/folha/export.py:1098` |
| POST de exportação, após aceitar o portão | 1 | 2 | Recálculo em leitura antes da classificação. (`app/folha/views.py:207`, `app/folha/views.py:208`, `app/folha/views.py:212`) |

O efeito é carregado antes das fatias e seu mapa é reutilizado pelos motores: esse custo não se multiplica pelo número de vínculos. (`app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:755`, `app/ponto/services/fechamento.py:758`)

Para espelho/tela:

- **G** = consulta financeira da grade, quando a guarda de escala/tipo permite. (`app/ponto/services/espelho.py:484`, `app/ponto/services/espelho.py:520`)
- **B** = passagem adicional pelo builder, pelo trecho histórico ou pela ata agregada. (`app/ponto/services/espelho.py:533`, `app/ponto/services/espelho.py:534`, `app/ponto/services/espelho.py:563`)
- **F** = recálculo online alcançado por `folha_manda`: período corresponde à competência, existe fechamento e o colaborador é processado. Existindo fechamento, não há teste de frescor antes de chamar `fechamento_lido`. (`app/relatorios/cartao_pela_celula.py:321`, `app/relatorios/cartao_pela_celula.py:323`, `app/relatorios/cartao_pela_celula.py:71`, `app/relatorios/cartao_pela_celula.py:73`; `app/ponto/services/fechamento.py:1764`)

Assim, o espelho custa **E = 1 + F; P = 1 + G + B + 2F**. Com G=F=1 e B=0: **E2/P4**; com builder histórico: **E2/P5**. Admin, app e ficha alcançam esse caminho. (`app/ponto/services/espelho.py:1231`, `app/ponto/services/espelho.py:520`, `app/ponto/services/espelho.py:534`, `app/ponto/services/espelho.py:1288`; `app/ponto/views.py:161`; `app/api/views.py:1354`; `app/colaboradores/services/ficha.py:199`)

O PDF acrescenta efeito próprio, dois builders, chama o espelho inteiro e aplica `folha_manda` novamente. Com as condições anteriores, são **E4/P9**, ou **E4/P10** com o builder histórico interno. (`app/relatorios/pdf_espelho.py:333`, `app/relatorios/pdf_espelho.py:476`, `app/relatorios/pdf_espelho.py:544`, `app/relatorios/pdf_espelho.py:599`, `app/relatorios/pdf_espelho.py:629`)

O calendário pode acrescentar **E1/P1 + F×(E1/P2)** quando foi solicitado calcular dias em aberto sem fornecer a lista; recebendo a lista, evita esse ramo. (`app/colaboradores/services/calendario.py:563`, `app/colaboradores/services/calendario.py:566`, `app/colaboradores/services/calendario.py:570`, `app/colaboradores/services/calendario.py:583`)

**O portão de medição do export tem uma ampliação de universo:** mede espelho, PDF e autoridade para os classificados `entra` da empresa; depois chama `cartao_x_txt_divergentes` sem argumento de empresa, e esse comando gera outro PDF para cada `entra` de **todas as empresas ativas**. (`app/folha/porta_export.py:469`, `app/folha/porta_export.py:476`, `app/folha/porta_export.py:496`, `app/folha/porta_export.py:597`, `app/folha/porta_export.py:667`; `app/relatorios/management/commands/cartao_x_txt_divergentes.py:51`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:63`)

Se U é o universo `entra` da empresa, V o universo `entra` das empresas ativas, e Q as consultas adicionais de grade para campos divergentes, nas condições E2/P4 por espelho e E4/P9 por PDF o total é **E=7|U|+4|V|; P=14|U|+9|V|+Q**. Um colaborador presente nos dois universos recebe **E11/P23**, antes de builders adicionais. Q pode acrescentar até três grades por colaborador com dado divergente exportado, inclusive fora do TXT. (`app/folha/porta_export.py:78`, `app/folha/porta_export.py:139`, `app/folha/porta_export.py:451`, `app/folha/porta_export.py:461`, `app/folha/porta_export.py:658`; `app/relatorios/management/commands/cartao_x_txt_divergentes.py:63`)

**A escrita de batida ainda chega aos três cálculos:**

1. `create` → `_fechamento_por_evento` → `recalcular_por_evento` → callback `on_commit` → fechamento → E → P → `realizado_do_dia`. O ramo de fechamento acrescenta E1/P2 quando processa o colaborador. (`app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:155`, `app/ponto/registro_batida.py:157`, `app/ponto/registro_batida.py:160`, `app/ponto/registro_batida.py:177`; `app/ponto/services/fechamento.py:1815`, `app/ponto/services/fechamento.py:1829`, `app/ponto/services/fechamento.py:665`; `app/ponto/services/efeito_ausencia.py:125`)
2. O próprio `post_save` julga o dia da batida e a véspera; o cartório monta grade, cujo overlay chama P e cujo laço consulta `realizado_do_dia`. (`app/ponto/signals.py:108`, `app/ponto/signals.py:112`, `app/ponto/signals.py:131`, `app/ponto/signals.py:135`, `app/ponto/signals.py:93`; `app/ponto/services/cartorio.py:1175`; `app/escala/utils.py:1868`, `app/escala/utils.py:2065`)
3. Não há teto universal de “duas chamadas por batida”: existem guardas de reentrada/célula/encerramento e possíveis repetições do corpo do julgamento. Se C corpos forem executados, o builder por turno acrescenta P=C e duas consultas diretas de realizado por corpo individual, além das internas de P. (`app/ponto/signals.py:90`; `app/ponto/services/cartorio.py:996`, `app/ponto/services/cartorio.py:1017`, `app/ponto/services/cartorio.py:1073`, `app/ponto/services/cartorio.py:1173`, `app/ponto/services/cartorio.py:1174`, `app/ponto/services/cartorio.py:2025`, `app/ponto/services/cartorio.py:2037`)

Cada P sem aprovação retorna após a consulta inicial. Havendo aprovação, recarrega células, vínculos e turnos, e consulta realizado por dia aprovado distinto. Mesmo fornecendo turnos, não fornece `livre`/`intervalo`, portanto o realizado consulta novamente o vínculo do dia. Tempo e quantidade efetiva de queries na frota: **[não verificado]**. (`app/ponto/services/efeito_ausencia.py:89`, `app/ponto/services/efeito_ausencia.py:95`, `app/ponto/services/efeito_ausencia.py:99`, `app/ponto/services/efeito_ausencia.py:104`, `app/ponto/services/efeito_ausencia.py:125`; `app/ponto/turnos.py:781`, `app/ponto/turnos.py:788`)

**P6 — apagados e ausência de chamadores**

O censo textual executado na cópia não encontrou referências executáveis aos oito símbolos abaixo em produção, templates, URLs, JS ou comandos. Como a regra impede gravar o resultado do censo, sua prova persistida por arquivo:linha fica **[não verificado]**; chamadas construídas dinamicamente também ficam **[não verificado]**.

| Símbolo removido | Resultado do censo |
|---|---|
| `_ordem_e_sentido_dos_marcos` | Zero referência executável encontrada. **[não verificado por artefato persistido]** |
| `_trecho_do_marco` | Zero referência executável encontrada. **[não verificado por artefato persistido]** |
| `trabalhado_do_turno` | Zero referência executável encontrada. **[não verificado por artefato persistido]** |
| `marcos_que_nao_cruzam_o_trabalhado` | Zero referência executável encontrada. **[não verificado por artefato persistido]** |
| `minutos_do_periodo_justificado` | Zero referência executável encontrada. **[não verificado por artefato persistido]** |
| `intervalos_do_periodo_justificado` | Zero referência executável encontrada. **[não verificado por artefato persistido]** |
| `_minutos_sem_repetir` | Zero referência executável encontrada. **[não verificado por artefato persistido]** |
| `justificativas_para_revisao` | Zero referência executável encontrada; há teste atual exigindo sua ausência. (`app/ponto/tests/test_justificativa_soma_sem_banco.py:428`, `app/ponto/tests/test_justificativa_soma_sem_banco.py:452`) |

`_cobertura_de_uma` **não foi apagada**: mudou de assinatura e continua chamada pela fonte. `fila_justificativas` e sua URL também continuam vivas. (`app/ponto/services/efeito_ausencia.py:114`, `app/ponto/services/efeito_ausencia.py:132`; `app/ponto/views.py:1698`; `app/ponto/urls.py:38`)

Foram retirados o ramo `status=revisao`, a projeção `j.revisao` e o card correspondente. A URL antiga com essa query agora cai na listagem sem filtro de status; somente pendente/aprovada/rejeitada possuem ramos. Links externos ou bookmarks existentes: **[não verificado]**. (`app/ponto/views.py:1703`, `app/ponto/views.py:1712`, `app/ponto/views.py:1731`; `app/templates/ponto/fila_justificativas.html:48`, `app/templates/ponto/fila_justificativas.html:112`; `app/ponto/tests/test_justificativa_soma_sem_banco.py:451`)

Restou uma promessa contraditória: o comentário ainda diz “sem ancora = lista do admin”, enquanto a fonte atual apenas registra `conserva`. (`app/ponto/turnos.py:2662`; `app/ponto/services/efeito_ausencia.py:116`)

Quanto aos testes apagados: [nao verificado]

| Remoção informada no delta | Regra que permanece / avaliação |
|---|---|
| Helpers de teste `minutos`, `nao_cruza`; classe `MinutosDoPeriodoTest`, com seus nove métodos | Medição da cauda pela âncora foi substituída pelo saldo diário. Pausas e noturno seguem cobertos pelos testes atuais de 550 minutos. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:214`, `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:343`) |
| Classe `CruzamentoComOTrabalhadoTest`, com seus sete métodos | A retirada de cobertura por sobreposição foi substituída pela preservação de cobertura e desconto no saldo. Há teste atual explícito dessa regra. (`app/ponto/tests/test_justificativa_soma_sem_banco.py:345`) |
| `test_03b_uniao_de_trechos_distintos_sobrepostos_e_disjuntos` | A união de intervalos pagos perdeu objeto; pagamento único com duas aprovações continua testado. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:220`) |
| `test_fila_e_projecao_do_motivo_da_fonte` | Foi retirada a projeção antiga; A4 apenas verifica a ausência dela. (`app/ponto/tests/test_justificativa_soma_sem_banco.py:424`) |
| `test_central_revisao_le_fila_uma_vez_por_colab_e_nao_rejulga` | Foi retirado o teste do caminho da Central; A5 verifica somente ausência textual. Não o substitui por excedente→HE→lista. (`app/ponto/tests/test_justificativa_soma_sem_banco.py:451`) |

Não identifiquei teste apagado que fosse a única prova **ainda correta** sob a lei nova; exclusividade histórica perante toda a suíte: **[não verificado]**. Os dois testes antigos de lista usavam col788, mas o caso atual desse colaborador é 59 trabalhados + 361 pagos = 420 previstos, sem excedente real. A lacuna atual é a falta de prova da entrega de um excedente real à HE. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:504`, `app/ponto/tests/test_justificativa_paga_o_periodo.py:541`)

**P7 — nomes, docstrings, asserções e mutantes**

Nos inventários abaixo, “coerente” significa coerente **no escopo executado**, não prova de produção. Os harnesses substituem bordas ORM. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:1`; `app/ponto/tests/test_justificativa_soma_sem_banco.py:5`)

Em `app/ponto/tests/test_justificativa_paga_o_periodo.py`:

| Teste novo/alterado | O que prova | [nao verificado]
|---|---|
| `test_RED_C8_ancora_numa_saida_conserva_o_dia_inteiro` | Só retorno de cobertura e motivo; comentário alterado não prova dinheiro/lista. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:55`) |
| `test_pausa_fora_do_cadastro_e_a_volta_abre_a_saida` | Cobre `hf`; não mede saldo. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:77`) |
| `test_mapa_injetado_690_abona_sem_falta_nem_DSR_perdido` | Nome corrigido: prova o motor com mapa manual de 690, independentemente da cura na fonte. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:145`) |
| `test_soma_S1_realizado_483_paga_zero_sem_perder_cobertura` | Coerente: realizado483, saldo0, dia justificado e zero na grade. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:215`) |
| `test_soma_S2_igualdade_real_paga_420` | Prova 720−300=420; não testa a borda trabalhado=previsto. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:254`) |
| `test_soma_S3_um_minuto_real_acima_reduz_para_419` | Prova 720−301=419. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:257`) |
| `test_v2_fechamento_real_grava_mesmos_660_do_motor_e_do_dia_pago` | Exercita motor, fechamento e DiaPago; usa mock de `ler_lavra`. Execução ORM nesta análise: **[não verificado]**. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:302`, `app/ponto/tests/test_justificativa_paga_o_periodo.py:328`) |
| `test_C5_ancorada_cobre_so_a_saida_e_completa_720` | Coerente: cobertura,720, feed vazio sem trabalho e idempotência. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:346`) |
| `test_RED_D1_pausa_fora_do_cadastro_completa_saldo` | Prova 720−210=510; a mensagem “não cruza” é desigualdade numérica, não prova geométrica. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:373`) |
| `test_RED_D2_saida_real_fora_do_raio_fecha_o_turno` | Coerente: inteiro conservado,840 trabalhados,saldo0; não verifica lista HE. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:384`) |
| `test_RED_D3_ultima_batida_e_saida_fecha_o_turno` | Coerente: inteiro conservado,243 trabalhados,saldo477. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:396`) |
| `test_RED_A_ancora_solta_horas_antes_do_turno_real` | Coerente:720 trabalhados,saldo0,`hf` preservado. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:413`) |
| `test_RED_P1_o_papel_e_da_ata_nao_do_tipo_gravado` | Coerente: prova papel pela ata e depois saldo1. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:426`) |
| `test_C7_sem_ancora_conserva_o_dia_inteiro` | Coerente: inteiro,720,feed vazio,sem missing. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:444`) |
| `test_RED_C8_ancora_de_papel_errado_conserva_o_dia_inteiro` | Coerente na fixture; menção à frota no docstring não prova a frota. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:458`) |
| `test_C9_outro_segue_o_dia_inteiro` | Prova justificado/feed vazio/sem missing; não asserta diretamente720. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:474`) |
| `test_c7_forma_col788_fonte_grade_motor_e_dsr_361` | Fonte/overlay/motor361,sem falta,DSR0; a chamada do leitor recebe361 manualmente. Fixture não é medição de col788 em produção. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:486`, `app/ponto/tests/test_justificativa_paga_o_periodo.py:509`) |
| `test_c7_A1_noturno_sobreposto_paga_60` | Fonte60,motor60,sem falta; não asserta contador semanal DSR. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:517`) |
| `test_c7_A2_T2_motor_completo_preserva_falta_e_DSR_com_pagamento_zero` | Depois sem falta/abono; DSR igual ao antes. Não compara os contadores de falta antes/depois nem verifica HE. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:541`) |

Em `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py`:

| Teste alterado | O que prova | [nao verificado]
|---|---|
| `test_01_gravado_le_660_da_linha_sem_recalcular` | Fonte660 e leitor respeitando mapa, inclusive123 manual; não executa gravação. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:200`) |
| `test_03_duas_ancoras_pagamento_unico_510` | Saldo510, com realizado210 fornecido pelo stub. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:220`) |
| `test_04_ausencia_inteira_precede_480_mapa_vazio` | Mapa vazio no efeito;480 sai de uma grade fabricada separadamente. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:230`) |
| `test_08_grade_lida_pelo_fechamento_transporta_o_numero` | Leitor alimentado manualmente transporta720; não executa fechamento. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:285`) |
| `test_09_mesmo_numero_na_grade_e_no_bloco_real_de_abono_do_motor` | Cinco instruções reais do motor e grade manual; nome/comentário delimitam esse escopo. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:307`, `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:322`) |
| `test_11_ancora_invalida_e_inteiro_conservam_sem_credito_parcial` | **Nome não é provado:** não verifica mapa parcial. Cria E07/S07:30, mas aceita trabalhado0/pago720 porque o stub depende de `self.trab`, vazio. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:173`, `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:349`) |
| `test_14_regressao_dos_casos_puros_da_v1` | Agora preserva somente a classe de cobertura, não as antigas classes de medição/cruzamento. Comentário atualizado é coerente. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:390`) |

Em `app/ponto/tests/test_justificativa_soma_sem_banco.py`:

| Teste novo/alterado | O que prova | [nao verificado]
|---|---|
| `test_S2_igualdade_e_inclusiva` | 480−240=240; não testa trabalhado=previsto. (`app/ponto/tests/test_justificativa_soma_sem_banco.py:97`) |
| `test_S3_mais_um_minuto_trabalhado_reduz_saldo` | Coerente:241→239. (`app/ponto/tests/test_justificativa_soma_sem_banco.py:104`) |
| `test_S4_480_sem_par_fechado_desconta_pausa` | Coerente: previsto líquido480,realizado0,saldo480. (`app/ponto/tests/test_justificativa_soma_sem_banco.py:111`) |
| `test_S6_duas_ancoras_nao_duplicam_saldo_do_dia` | Coerente: duas aprovações,um saldo240. (`app/ponto/tests/test_justificativa_soma_sem_banco.py:133`) |
| `test_realizado_liquido_da_autoridade_nao_span_bruto` | Coerente:360 realizados após pausa,saldo120. (`app/ponto/tests

[truncado em 20000 de 32805 caracteres]

## 14/11 20:52 · sessao ccf51d2110b5 · - · - · origem=Ronald (tmux)

P5 permanece exclusivamente diagnóstico; não executei banco nem escrevi arquivos. [nao verificado]

- A carga pergunta a plural uma vez por colaborador cuja janela tem algum dia sem rota; o resultado fica memoizado por colaborador. A primeira leitura julga toda a janela, não apenas o dia solicitado. (`chamados/services/tarefa_do_dia.py:582`, `chamados/services/tarefa_do_dia.py:585`, `chamados/services/tarefa_do_dia.py:588`, `chamados/services/tarefa_do_dia.py:597`)
- Cada plural chama `contexto_calendario` uma vez por bloco guloso cuja amplitude é até 366 dias. Para carga com amplitude até 366, chamadas de contexto = número de colaboradores consultados com ao menos um dia sem porta. Para amplitude maior, é a soma dos blocos desses colaboradores. (`colaboradores/services/calendario.py:114`, `colaboradores/services/calendario.py:128`, `colaboradores/services/calendario.py:132`, `colaboradores/services/calendario.py:137`)
- Os quatro testes Q conservam igualdade/ausência de refetch; não cabe mudar o orçamento. (`chamados/tests/test_e2_tarefa_do_dia.py:938`, `chamados/tests/test_e2_tarefa_do_dia.py:1180`, `chamados/tests/test_n1_alimentacao_unica.py:169`, `chamados/tests/test_n1_alimentacao_unica.py:183`)

Pilha estática verificável do caminho adicional:

```text
Carga.tarefa                            tarefa_do_dia.py:589
resolver_dia_oferecido_nas_datas          calendario.py:132
contexto_calendario
  autoridade_do_periodo                  calendario.py:570
  folha_manda                           calendario.py:583
    totais_da_folha [se competência]     cartao_pela_celula.py:323
      fechamento_lido                   cartao_pela_celula.py:73
        recalcular_fechamento_mes        fechamento.py:1759
          apurar_por_fatia               fechamento.py:754
            motor.calcular_mes          fechamento.py:286
              _previsto_do_dia
                refetch Colaborador     motor_calculo_v2.py:1240
```

A atribuição exata das duas SQL fornecidas a essa pilha é **[não verificado]**: exige `CaptureQueriesContext` acompanhado de `connection.execute_wrapper` coletando `traceback.extract_stack()` no instante de cada execução. Os testes atuais capturam SQL sem pilha. (`chamados/tests/test_n1_alimentacao_unica.py:160`, `chamados/tests/test_n1_alimentacao_unica.py:166`, `chamados/tests/test_n1_alimentacao_unica.py:177`)

Opções de cura na origem, para registrar sem implementar:

1. Extrair alimentação em lote do mesmo juiz do calendário e fazer a oferta ler somente dias pedidos; preserva a autoridade e evita montar todos os elementos visuais. Hoje a plural chama o contexto completo. (`colaboradores/services/calendario.py:132`, `colaboradores/services/calendario.py:159`)
2. Aceitar/reutilizar a carga geométrica já disponível no calendário, reduzindo leituras repetidas; isoladamente não demonstra retorno ao orçamento constante. (`colaboradores/services/calendario.py:221`, `colaboradores/services/calendario.py:314`, `colaboradores/services/calendario.py:606`)
3. Alimentar os memos existentes no montador do fechamento, como o espelho já faz. O fechamento alimenta células e ausência, mas não os dois memos do previsto; o espelho preenche ambos para toda a janela. Não exige tocar o motor; tem alcance maior no caminho monetário. (`ponto/services/fechamento.py:724`, `ponto/services/fechamento.py:750`, `ponto/services/espelho.py:1151`, `ponto/services/espelho.py:1157`, `ponto/motor_calculo_v2.py:1232`)

Execução sem banco existente:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_lu2_suite
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_degrau_mutantes --todos
```

A suíte carrega sete módulos e classes adicionais por AST. (`chamados/tests/e3_lu2_suite.py:9`, `chamados/tests/e3_lu2_suite.py:16`) O executor de mutantes exige controle verde e morte por falha de assert sem erros; atualmente uma exceção no controle/intercâmbio interrompe `--todos`, portanto convém coletar o erro por mutante e continuar. (`chamados/tests/e3_degrau_mutantes.py:144`, `chamados/tests/e3_degrau_mutantes.py:158`, `chamados/tests/e3_degrau_mutantes.py:163`, `chamados/tests/e3_degrau_mutantes.py:190`)

Para P1, o teste M8 atual executa somente o prólogo do contexto até calcular `hoje`; não alcança `folha_manda`, `ate_ontem` nem `_pv`. (`chamados/tests/test_e3_degrau_dono_e_porta.py:613`, `chamados/tests/test_e3_degrau_dono_e_porta.py:624`) Extensões necessárias:

- `relogio_engolido`: retirar o novo keyword de `folha_manda` no contexto.
- Mutante de C6: retirar `agora=_agora` de `_pv`.
- Mutante de desconhecimento: converter `None` em `False` no transporte para a tarefa.

Os sítios atuais são `colaboradores/services/calendario.py:476`, `colaboradores/services/calendario.py:583`, `colaboradores/services/calendario.py:135` e `chamados/services/tarefa_do_dia.py:593`.

## 14/11 20:52 · sessao 4c765f637a3e · - · - · origem=Ronald (tmux)

P4(a) tem impedimento no código: M1 sem âncora passa pela regra de porta, mas C11 exige pelo menos uma âncora terminal e descarta o caso sem ela. Portanto a fixture M1 sozinha não recupera C11; registrar pergunta de desenho antes de alargar esse predicado. (`escala/services/cadastro_realidade.py:496`; `escala/services/cadastro_realidade.py:497`; `chamados/tests/test_e3_degrau_dono_e_porta.py:529`)

A fixture `_beco` também é compartilhada com os testes de reata. Preserve ali a versão com âncora terminal mediante parâmetro explícito ou helper separado. (`chamados/tests/test_f3_beco1_reata_limbo.py:28`; `escala/tests/test_f3_beco1_folga_na_lista.py:58`)

Para P4(b), `_dia` precisa:

- Criar vínculo com turno 07:00–16:00 e ligar `escala_geradora` à célula, acompanhando o padrão existente de criação de vínculo/célula. Sem vínculo, o teto não casa e a tarefa termina em `marco_fora_do_turno`. (`chamados/tests/test_o261_vigia_fio.py:87`; `chamados/tests/test_e2_tarefa_do_dia.py:82`; `chamados/tests/test_e2_tarefa_do_dia.py:97`; `chamados/services/tarefa_do_dia.py:293`)
- Conservar uma âncora concreta no caso da resposta `FOLGA_CONTESTA` sem validação: atualmente `_dia` grava `chamado_destino=None`; depois da resposta, vínculo sozinho não fornece o modal exigido por M1. (`chamados/tests/test_o261_vigia_fio.py:110`; `chamados/tests/test_o261_vigia_fio.py:642`; `chamados/services/tarefa_do_dia.py:630`)
- No selo do carimbo, declarar o mesmo dublê de `alimentar_fila` que converte residual `decidir` em `cobrar`, durante o comando e a leitura de comparação. O helper `_censo` já faz isso, mas o comando e a comparação chamam o censo real; por isso a fixture validada mantém porta residual e não produz o morto pretendido. Preserve `fio_morto>=1` e `fio_vivo>=1`. (`chamados/tests/test_o261_vigia_fio.py:128`; `chamados/tests/test_o261_vigia_fio.py:801`; `chamados/tests/test_o261_vigia_fio.py:812`; `chamados/tests/test_o261_vigia_fio.py:831`)

Nenhum arquivo editado; execução dos testes e resultados dessas propostas: [não verificado]. [nao verificado]

## 14/11 20:52 · sessao db2e7bea00f8 · JUSTIFICATIVA-PAGA-O-PERIODO · verificador · origem=Code

**Há divergências entre a fonte, o motor e os leitores financeiros.** As principais são: entrada aberta com realizado zero perde o crédito no motor; ausência inteira com trabalho perde o saldo no motor; ausência parcial conta como dia inteiro em `dias_com_abono`; e o PDF pode acrescentar o crédito parcial ao saldo da justificativa. (`app/ponto/services/efeito_ausencia.py:48`, `:64`; `app/ponto/motor_calculo_v2.py:2611`; `app/ponto/services/dia_pago.py:704`; `app/relatorios/pdf_espelho.py:480`.)

Abri a cópia indicada. A identidade criptográfica com `c9e419f39`, a comparação direta com `8928f955a` e o HEAD de `main` ficam **[nao verificado]**: a cópia disponível não tem repositório Git.

**P1. Escritores, leitores e significados**

Os mapas abaixo são por colaborador e por **data do turno**, selecionando justificativas aprovadas com `data_turno` preenchida. A composição ocorre por dia antes do cálculo do pagamento. (`app/ponto/services/efeito_ausencia.py:89`, `:106`, `:122`.)

| Campo | Quem escreve; significado | Quem lê; interpretação |
|---|---|---|
| `periodos_justificados.minutos` | `periodos_justificados`: `max(0, previsto − trabalhado)`, calculado uma vez por dia. Data ausente: nenhuma justificativa selecionada. Zero: aprovação presente sem crédito. (`app/ponto/services/efeito_ausencia.py:89`, `:122`, `:128`.) | `efeitos_da_ausencia` copia somente dias com trabalho; `_marcar_ausencias` copia para linhas de trabalho cobertas; folha e espelho projetam **todos** os dias para a grade financeira. (`app/ponto/services/efeito_ausencia.py:48`; `app/escala/utils.py:1251`, `:1257`, `:1277`; `app/folha/export.py:339`; `app/ponto/services/espelho.py:520`.) |
| `periodos_justificados.trabalhado` | `realizado_do_dia` fornece os minutos; a fonte transforma `None` em zero. Zero reúne tanto ausência de turno quanto turno sem par fechado aproveitável. (`app/ponto/services/efeito_ausencia.py:125`; `app/ponto/turnos.py:719`, `:735`, `:771`.) | `efeitos_da_ausencia` usa `> 0` para decidir se o dia entra no mapa de saldo. Esse é o consumo financeiro direto observado desse campo. (`app/ponto/services/efeito_ausencia.py:48`.) |
| `periodos_justificados.cobre` | A fonte compõe a resposta dos juízes: `None` significa cobertura inteira; conjunto identifica marcos dispensados; vazio significa nenhum marco dispensado. Várias aprovações unem os marcos; cobertura inteira encerra a composição daquele dia. (`app/ponto/services/efeito_ausencia.py:109`, `:121`, `:162`; `app/ponto/turnos.py:2703`.) | A própria composição lê `None` como inteiro. `_marcar_ausencias` limpa os furos inteiros ou dispensa somente os marcos indicados. O efeito financeiro passou a justificar **todas** as datas da fonte, sem testar `cobre`. (`app/ponto/services/efeito_ausencia.py:47`, `:109`; `app/escala/utils.py:1251`, `:1258`.) |
| `periodos_justificados.conserva` | A fonte registra o motivo pelo qual a âncora não delimitou marcos: célula sem marco, sem âncora, papel errado ou turno fechado na ponta. Ausência da chave significa ausência desse registro; o tipo sem marco também pode produzir inteiro sem `conserva`. (`app/ponto/services/efeito_ausencia.py:111`, `:117`, `:145`, `:162`; `app/ponto/turnos.py:2689`.) | O contrato atual declara que esse motivo não cria fila de HE. Não há encaminhamento à HE dentro da função que devolve o campo. (`app/ponto/services/efeito_ausencia.py:79`, `:129`.) |
| `efeitos_da_ausencia.justificadas` | Começa com **todas** as datas da fonte, incluindo pagamento zero e folga; acrescenta ausência inteira ABONA. É conjunto, portanto zero não se aplica. Data ausente não significa automaticamente falta. (`app/ponto/services/efeito_ausencia.py:47`, `:62`.) | O motor usa presença para proteger contra falta e DSR e, combinada com dia obrigatório sem período de trabalho, para conceder abono inteiro. (`app/ponto/motor_calculo_v2.py:2600`, `:2605`, `:2611`.) |
| `efeitos_da_ausencia.min_justificados` | Mapa de minutos dos dias com `trabalhado > 0`; ausência inteira ABONA remove a data. Zero explícito permanece. Ausência de chave reúne situações diferentes: sem justificativa, realizado zero ou precedência de ausência inteira. (`app/ponto/services/efeito_ausencia.py:48`, `:64`.) | O motor considera chave presente como saldo, inclusive zero; chave ausente permite abono inteiro **somente se** o dia estiver em `_abono`. Ele não recupera diretamente `periodos_justificados.minutos`. (`app/ponto/motor_calculo_v2.py:2611`, `:2628`.) |
| `efeitos_da_ausencia.dia_inteiro` | É `{data: Ausencia}`, exclusivamente cobertura ABONA que explica o dia inteiro; não é mapa de minutos nem conjunto das justificativas integrais. (`app/ponto/services/efeito_ausencia.py:70`; `app/ponto/turnos.py:2764`, `:2775`.) | O motor usa o objeto para zerar atraso e saída antecipada, preservando o trabalho. Data ausente significa que essa proteção específica de ausência inteira não foi fornecida. (`app/ponto/motor_calculo_v2.py:1090`, `:1097`.) |
| Parâmetro `grade_da_celula(min_justificados=...)` | Folha e espelho fornecem todas as datas de `periodos_justificados`, inclusive sem trabalho. O padrão `None` vira `{}`. (`app/folha/export.py:339`; `app/ponto/services/espelho.py:520`; `app/escala/services/leitor_celula.py:381`.) | A grade apenas transporta valores para `tipo_dia == 'trabalho'`, preservando zero por teste de presença. Chave ausente não provoca consulta nem preenchimento do crédito. (`app/escala/services/leitor_celula.py:449`.) |

Os transportadores do efeito até o motor são:

- **Fechamento:** copia `justificadas` e `min_justificados`; monta `dia_inteiro` pela mesma autoridade de cobertura e alimenta o motor da fatia e o motor órfão. (`app/ponto/services/fechamento.py:664`, `:688`, `:754`, `:823`, `:830`.)
- **Espelho e PDF:** passam o conjunto de datas e os dois mapas ao motor. (`app/ponto/services/espelho.py:1117`, `:1136`; `app/relatorios/pdf_espelho.py:396`, `:406`.)
- **Esmeril e comandos de comparação:** também transportam esses campos, sem redefinir sua unidade. (`app/ponto/services/esmeril_espelho.py:160`; `app/relatorios/management/commands/diff_extrato_parcial.py:41`, `:61`; `app/ponto/management/commands/diff_espelho_alimentacao.py:48`, `:56`, `:123`.)

**Os contratos não são equivalentes. Os pares divergentes encontrados são:**

1. **`trabalhado == 0` da fonte × `datas_trabalho` do motor.** A fonte mede minutos; o motor inclui qualquer período normal, inclusive aberto com zero minutos. Assim, retirar o dia do mapa não garante que ele receberá abono inteiro. (`app/ponto/services/efeito_ausencia.py:48`; `app/ponto/motor_calculo_v2.py:2499`, `:2501`, `:2581`, `:2611`.)

2. **Ausência inteira no efeito × ausência na grade.** O efeito remove o saldo da justificativa; havendo trabalho, o motor também exclui o abono inteiro. A grade continua pagando previsto menos realizado. (`app/ponto/services/efeito_ausencia.py:64`; `app/ponto/motor_calculo_v2.py:2611`; `app/ponto/services/dia_pago.py:396`.)

3. **Ausência parcial no motor × ausência no contador.** O efeito preserva o saldo da justificativa e o motor conta zero dias inteiros; a grade classifica a parcial como `ausencia`, e `dias_com_abono` aceita essa classe inteira sem olhar `cobertura_parcial`. (`app/ponto/services/efeito_ausencia.py:58`; `app/escala/utils.py:1217`, `:1227`; `app/ponto/motor_calculo_v2.py:2629`; `app/ponto/services/dia_pago.py:704`.)

4. **Abono do motor/grade × abono preliminar do PDF.** O PDF acrescenta `min_parcial_pagos` ao abono já calculado pelo motor. (`app/relatorios/pdf_espelho.py:479`.)

5. **Ata incompleta no contador × ata incompleta na grade.** A grade converte realizado ausente para zero; o contador exige literalmente `ata.get('minutos_realizados') == 0`. Também exige `tipo_dia` explícito, enquanto a grade admite derivá-lo de `cel.trabalha`. (`app/escala/services/leitor_celula.py:419`, `:443`; `app/ponto/services/dia_pago.py:705`.)

6. **Cartão com fechamento × cartão somente com lavratura.** O primeiro substitui horas, mas não dias de abono; o segundo substitui ambos usando `dias_com_abono`. (`app/relatorios/cartao_pela_celula.py:93`, `:118`, `:186`, `:190`, `:323`.)

O teste truthy de `por_dia_da_grade` também transforma crédito explícito zero em ausência de chave no mapa de abono. Isso preserva o valor monetário zero, mas perde a distinção de representação mantida pelo motor. (`app/ponto/services/dia_pago.py:398`; `app/ponto/motor_calculo_v2.py:2629`.)

**P2. Valores por cenário**

A LEI fornecida determina os **minutos da justificativa**, mas não especifica formalmente o contador “dias de abono”: essa determinação normativa fica **[nao verificado]**. A convenção declarada no código é abono inteiro sem trabalho versus saldo com trabalho. (`app/ponto/services/efeito_ausencia.py:31`; `app/ponto/services/dia_pago.py:691`.)

Os cenários não fornecem atas, linhas `DiaPago` nem existência de fechamento. Portanto, os resultados abaixo são consequências do código com esses dados coerentes com o previsto e trabalhado informados; o retorno produtivo efetivamente persistido fica **[nao verificado]**.

Na tabela, `min/dias` significa crédito de abono e quantidade de dias inteiros. A grade calcula minutos, sem devolver contador de dias. (`app/ponto/services/dia_pago.py:387`, `:405`.)

| Cenário | LEI: justificativa → trabalho + justificativa | Motor bruto via fechamento | Grade/folha: crédito | `dias_com_abono`, com crédito lavrado e ata coerente | PDF antes da substituição pela folha |
|---|---:|---:|---:|---:|---:|
| **a)** P480, sem batida | 480 → 480 | 480/1 | 480 | 1 | 480/1 |
| **b)** P720, W600 | 120 → 720 | 120/0 | 120 | 0 | 120/0 |
| **c)** P450, W483 | 0 → 483; excedente 33 | 0/0 | 0 | 0 | 0/0 |
| **d)** 22–06, E18 aberta + E22/S05 | 60 → 480 | 60/0 | 60 | 0 | 60/0 |
| **g)** folga, P0 | 0 → W | 0/0 | 0 | 0 | 0/0 |

A fórmula e a soma do trabalho por dia estão em `app/ponto/services/efeito_ausencia.py:122` e `app/ponto/turnos.py:719`; o ramo de abono do motor está em `app/ponto/motor_calculo_v2.py:2600`; a grade financeira recebe a fonte em `app/folha/export.py:339` e apura em `app/ponto/services/dia_pago.py:388`; o contador está em `app/ponto/services/dia_pago.py:700`; o PDF inicializa os dois valores em `app/relatorios/pdf_espelho.py:479`. A geometria específica de **d** também consta no teste `app/ponto/tests/test_justificativa_paga_o_periodo.py:516`.

**e) Ausência ABONA inteira + justificativa:** faltam P e W para um número único **[nao verificado]**. Sem trabalho nem período, o motor abona P e conta um dia. Havendo trabalho, a ausência remove o mapa e o motor não concede abono inteiro; a grade paga o saldo. (`app/ponto/services/efeito_ausencia.py:64`; `app/ponto/motor_calculo_v2.py:2611`, `:2629`; `app/ponto/services/dia_pago.py:396`.)

Contraexemplo concreto de código, **P480/W240**:

| Leitor | Resultado |
|---|---:|
| LEI: justificativa; trabalho + justificativa | 240; 480 |
| Motor bruto via fechamento | **0 min / 0 dias** |
| Grade/folha | **240 min** |
| PDF livre, antes de substituição | **0 min / 0 dias** |
| `dias_com_abono`, após lavrar esses 240 | **1 dia** |

Essas diferenças seguem da remoção em `app/ponto/services/efeito_ausencia.py:64`, da exclusão de dias trabalhados em `app/ponto/motor_calculo_v2.py:2611` e da regra de ausência em `app/ponto/services/dia_pago.py:396`, `:704`.

**f) Ausência parcial paga + justificativa:** também faltam P, W e minutos da parcial para um número único **[nao verificado]**. Com W positivo, o motor recebe o saldo da justificativa; o PDF soma ainda a parcial. A grade mantém a classe `ausencia` e calcula seu próprio saldo pela ata. (`app/ponto/services/efeito_ausencia.py:48`, `:53`, `:58`; `app/relatorios/pdf_espelho.py:480`; `app/escala/utils.py:1217`; `app/ponto/services/dia_pago.py:396`.)

Contraexemplo de código, **P480/W240/parcial120**:

| Leitor | Resultado |
|---|---:|
| LEI: justificativa; trabalho + justificativa | 240; 480 |
| Motor bruto via fechamento | **240 min / 0 dias** |
| Grade/folha | **240 min** |
| PDF livre | **360 min / 0 dias** |
| `dias_com_abono`, após lavrar os 240 | **1 dia** |

O PDF soma parcelas diferentes das usadas pela grade; a autorização normativa para cumular a parcial com o saldo integral da justificativa fica **[nao verificado]**. A divergência numérica está explícita em `app/relatorios/pdf_espelho.py:480`, `app/ponto/services/dia_pago.py:396` e `:704`.

**h) Duas justificativas no mesmo dia:** a fonte produz **um saldo**, nunca dois saldos. Sem P/W, o valor numérico fica **[nao verificado]**. Os leitores seguem o caso correspondente acima: sem trabalho e sem período, P/1 no motor; com trabalho, saldo/0; com ausência sobreposta, aplicam-se as divergências de e/f. (`app/ponto/services/efeito_ausencia.py:106`, `:109`, `:121`, `:122`; `app/ponto/motor_calculo_v2.py:2611`, `:2629`.)

**Espelho, PDF final e `cartao_pela_celula`, em cada cenário:** o resultado depende desta seleção de fonte:

- **Competência com fechamento:** as horas finais vêm de `fechamento_lido.minutos_abonados`, calculadas pela grade; `dias_abono` continua sendo o valor anteriormente fornecido pelo motor. Assim, e/f do exemplo terminam em **240 min/0 dias**. (`app/relatorios/cartao_pela_celula.py:69`, `:118`; `app/ponto/services/fechamento.py:1065`; `app/ponto/services/espelho.py:1256`; `app/relatorios/pdf_espelho.py:479`.)
- **Competência sem fechamento, com lavratura:** horas vêm de `DiaPago`; dias vêm de `dias_com_abono`. Assim, e/f terminam em **240 min/1 dia**. (`app/relatorios/cartao_pela_celula.py:186`, `:190`, `:342`.)
- **Período livre:** a seleção da folha não substitui esses totais. O PDF conserva motor mais parcial; o resumo do espelho inicializa dias pelo motor e não inicializa `horas_abono` nesse trecho. (`app/relatorios/cartao_pela_celula.py:321`; `app/relatorios/pdf_espelho.py:480`; `app/ponto/services/espelho.py:1234`, `:1256`, `:1288`.)

Portanto, **e e f demonstram leitores com números diferentes**, mesmo com ata e lavratura coerentes; h herda essas diferenças quando contiver essas sobreposições. (`app/ponto/services/efeito_ausencia.py:64`; `app/ponto/services/dia_pago.py:396`, `:704`; `app/relatorios/pdf_espelho.py:480`.)

Há ainda um contraexemplo fora dos oito enunciados: **P480, somente uma E aberta, justificativa aprovada**. A fonte paga 480; o feed omite a data por W0; o motor considera o período aberto como dia trabalhado e paga **0/0**; a grade recebe 480 e o contador pode contar um dia. É a quebra mais direta do “um número só”. (`app/ponto/services/efeito_ausencia.py:48`, `:128`; `app/ponto/motor_calculo_v2.py:2499`, `:2581`, `:2611`; `app/folha/export.py:339`; `app/ponto/services/dia_pago.py:705`.)

**P3. Nova falta ou novo DSR perdido**

**Não encontrei contraexemplo demonstrado de nova falta/DSR no próprio dia aprovado pelo ramo aberto do motor.** Toda data aprovada entra em `justificadas`; falta e perda de DSR exigem dia obrigatório ausente tanto de trabalho quanto de justificadas. O mapa de minutos não participa desse teste. (`app/ponto/services/efeito_ausencia.py:47`; `app/ponto/motor_calculo_v2.py:2605`, `:2610`.)

O override comercial também consulta `datas_justificadas`, mas possui outro cálculo de dias: usa a data civil da entrada, enquanto o ramo base usa o mapa do dia da jornada. Isso é regra própria de leitor/classe e impede tratar os dois caminhos como identidade geral. (`app/ponto/motor_calculo_v2.py:2583`, `:3477`, `:3490`.)

A grade também conserva a distinção entre justificar financeiramente o dia e dispensar marcos: `esqueci_saida` pode dispensar somente `hf`, deixando outros marcos faltantes. Isso não é, sozinho, prova de falta inteira ou perda de DSR. (`app/ponto/turnos.py:2712`; `app/escala/utils.py:1258`, `:1278`.)

**“Não ocorre nenhuma regressão contra `8928f955a` nem contra main” permanece [nao verificado].** O contraexemplo da entrada aberta prova perda de **pagamento**, sem provar nova falta/DSR. (`app/ponto/motor_calculo_v2.py:2605`, `:2611`.)

**P4. Origem e momento dos dados de `dias_com_abono`**

A função cruza **dinheiro lavrado por competência/versão** com **classe e realizado da ata atual**; não conta simplesmente as mesmas linhas ou o mesmo predicado do motor. (`app/ponto/services/dia_pago.py:700`.)

| Dado usado | Origem, escritor e momento |
|---|---|
| Colaborador, mês, ano e versão | Argumentos; versão padrão `motor`. O cartão chama a função ao montar os totais da lavratura. (`app/ponto/services/dia_pago.py:690`; `app/relatorios/cartao_pela_celula.py:190`.) |
| `DiaPago.data`, tipo diário e crédito positivo | `lavrar` recebe mapas por data, escreve `minutos_abonados`, cria as linhas e substitui a competência/versão. (`app/ponto/services/dia_pago.py:288`, `:345`, `:359`.) |
| Mapa que fornece o crédito | Fechamento monta a grade, chama `por_dia_da_grade` e passa o mapa à lavratura. (`app/ponto/services/fechamento.py:1018`, `:1065`, `:1217`.) |
| Quando ocorre essa lavratura | No fechamento normal ou na leitura com `lavrar_dias_pagos=True`; leitura pura não entra nessa escrita. (`app/ponto/services/fechamento.py:1196`.) |
| Outro escritor do crédito | `retratar_exportada` cria/atualiza os campos `minutos_*` pela grade; sua guarda pode impedir a escrita. (`app/ponto/services/dia_pago.py:454`, `:467`, `:484`, `:541`.) |
| `ata.tipo_dia` | Builder classifica ausência inteira e parcial como `ausencia`; a parcial recebe carimbo adicional. (`app/escala/utils.py:1217`, `:1227`.) |
| `ata.minutos_realizados` | Builder consulta `realizado_do_dia` e transporta o resultado. (`app/escala/utils.py:2065`, `:2078`.) |
| Persistência da ata | Cartório copia tipo e realizado, entrega à lavra com `apply_` e carimba; a porta atribui a ata e salva os campos. (`app/ponto/services/cartorio.py:738`, `:1850`, `:1873`; `app/ponto/portas/celula.py:1091`, `:1201`.) |

A resposta à equivalência com `motor.dias_abono` é **não**: e/f com trabalho e crédito positivo já dão **contador 1 versus motor 0**; entrada aberta com W0 pode produzir a mesma diferença. (`app/ponto/services/dia_pago.py:704`; `app/ponto/motor_calculo_v2.py:2611`, `:2629`.)

Além disso, mudar a ata pode mudar a contagem sem alterar `DiaPago`: a consulta não exige correspondência de carimbo ou versão entre as duas fontes. A ocorrência produtiva dessa mistura fica **[nao verificado]**. (`app/ponto/services/dia_pago.py:700`.)

Sobre o excedente: a fonte limita o crédito corretamente, mas não devolve um encaminhamento de excedente à HE; a lista existente seleciona `he_fora_da_janela` do espelho. Provar que todo excedente da LEI chega ao admin exige mais do que o saldo calculado. (`app/ponto/services/efeito_ausencia.py:128`; `app/ponto/services/he_pendente_lavrado.py:132`.) A passagem dos 33 minutos de c, a nova medição dos 41, os nove colaboradores da competência 11 e a integridade produtiva da exportada ficam **[nao verificado]**.

Os selos sem banco não eliminam esses achados: o teste de abono inteiro fornece `datas_trabalho` vazio, e o teste da ausência inteira verifica o mapa/grade sem confrontar o abono do motor naquele dia trabalhado. (`app/ponto/tests/test_justificativa_soma_sem_banco.py:391`, `:417`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:230`.)

## 14/11 20:52 · sessao 2b69994a518a · - · - · origem=Ronald (tmux)

Inspeção somente leitura concluída.

- **P2:** acrescentar `*, agora=None` aos três dublês: [bloqueantes:68](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/chamados/tests/test_e3_bloqueantes.py:68), [leitores:66](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/chamados/tests/test_e3_leitores_c17_chamadores.py:66), [backfill:36](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/chamados/tests/test_e4_backfill.py:36).
- **P6:** `git diff de702cb2f HEAD -- chamados/models.py chamados/catalogo/modulos.py` devolveu vazio. A captura best effort do `KeyError` já existia, preservando emissão; [models.py:988](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/chamados/models.py:988). A ocorrência concreta dos módulos `ponto|teste` não foi reproduzida [não verificado].
- **P4(c):** para os casos que medem ausência de canal, declarar plural `False` no teste mantém os asserts positivos existentes; acrescentar irmão com plural `True`, exigindo `admin/porta_acao/resolver_dia`. Os locais são [FuroSemCanalTest:88](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/chamados/tests/test_o66_furo_sem_canal.py:88), [Juiz:200](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/chamados/tests/test_o261e_f5b_juiz_porta.py:200), [Juiz:276](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/chamados/tests/test_o261e_f5b_juiz_porta.py:276), [Haiku:74](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/folha/tests/test_haiku_fora_da_folha.py:74). Golden com plural `False` conserva a transição sem porta → chamado → admin; irmão `True` cobre Resolver, [Golden:494](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/folha/tests/test_haiku_fora_da_folha.py:494). Resultado depois dessas curas [não verificado]. [nao verificado]
- **`fila`:** escritor publica portas do dono em [tarefa:669](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/chamados/services/tarefa_do_dia.py:669). Folha converte `()` em `chamado=[]`, [fora_da_folha:247](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/folha/fora_da_folha.py:247); sombra projeta `()` e registra `fila=0`, [diff:50](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/chamados/management/commands/diff_e3_sombra.py:50), [diff:143](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/chamados/management/commands/diff_e3_sombra.py:143).

Censo produtivo `folha_manda`: calendário:583; espelho:1284; PDF:628. Definição: cartao_pela_celula:296. `ate_ontem`: cartao:412 omite `hoje`; PDF:287 e :598 e comando diff_pdf_x_espelho:76 já o fornecem. Essas referências correspondem aos arquivos inspecionados antes das edições do agente principal.

## 14/11 20:49 · sessao 7070790a6a5f · - · - · origem=Ronald (tmux)

Complemento P5:

**O comando aninhado amplia o universo para todas as empresas ativas.** `medir(empresa)` chama `cartao_x_txt_divergentes` passando apenas mês/ano/agora (`app/folha/porta_export.py:667`). O comando percorre todas as empresas ativas (`app/relatorios/management/commands/cartao_x_txt_divergentes.py:51`), classifica cada uma (`app/relatorios/management/commands/cartao_x_txt_divergentes.py:53`), mantém `status='entra'` e colaborador existente (`app/relatorios/management/commands/cartao_x_txt_divergentes.py:57`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:59`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:60`) e executa **um PDF inteiro por colaborador** (`app/relatorios/management/commands/cartao_x_txt_divergentes.py:63`). A comparação posterior não adiciona recálculo: lê fechamento gravado (`app/relatorios/management/commands/cartao_x_txt_divergentes.py:68`, `app/relatorios/cartao_pela_celula.py:249`, `app/relatorios/cartao_pela_celula.py:250`, `app/relatorios/cartao_pela_celula.py:253`).

**Fórmula do custo total de uma chamada a `medir(empresa)`, sem exceções:**

- `U`: colaboradores `entra` dessa empresa (`app/folha/porta_export.py:469`, `app/folha/porta_export.py:470`).
- `V`: colaboradores `entra` de todas as empresas ativas, pelo comando aninhado (`app/relatorios/management/commands/cartao_x_txt_divergentes.py:51`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:57`).
- `S(c)`: custo do espelho; `D(c)`: custo do PDF; `A=(E1,P1)`: autoridade isolada (`app/folha/porta_export.py:476`, `app/folha/porta_export.py:496`, `app/folha/porta_export.py:597`, `app/folha/porta_export.py:78`).
- `Q`: quantidade de chamadas de grade para campos divergentes de competência exportada; **0–3 por colaborador com dado**, incluindo fora do TXT (`app/folha/porta_export.py:347`, `app/folha/porta_export.py:451`, `app/folha/porta_export.py:455`, `app/folha/porta_export.py:461`, `app/folha/porta_export.py:658`, `app/folha/porta_export.py:662`, `app/folha/porta_export.py:139`).

Total:

`ΣU[S(c)+D(c)+A] + ΣV[D(c)] + (E0,PQ)`, pelas chamadas acima.

Com as condições anteriores `G=F=1,B=0`, portanto `S=(E2,P4)` e `D=(E4,P9)`, fica **E=7|U|+4|V|; P=14|U|+9|V|+Q**. Um colaborador da empresa ativa presente nos dois universos custa **E11/P23**, mais builders históricos/eventuais e as próprias chamadas de `Q` (`app/ponto/services/espelho.py:1231`, `app/ponto/services/espelho.py:520`, `app/ponto/services/espelho.py:1288`, `app/relatorios/pdf_espelho.py:333`, `app/relatorios/pdf_espelho.py:476`, `app/relatorios/pdf_espelho.py:544`, `app/relatorios/pdf_espelho.py:599`, `app/relatorios/pdf_espelho.py:629`).

Se o operador mede diversas empresas, cada chamada volta a percorrer `V` inteiro; não há argumento de empresa no comando aninhado nem restrição correspondente no laço (`app/folha/porta_export.py:667`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:20`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:51`). Quantos colaboradores/repetições ocorrem efetivamente: [não verificado].

**Esmeril não acrescenta E/P nesses caminhos financeiros.** O juiz `escala_certa_no_dia` pode ser chamado pelo fechamento e pelo motor (`app/ponto/services/fechamento.py:933`, `app/ponto/motor_calculo_v2.py:1470`, `app/ponto/motor_calculo_v2.py:1474`), mas chama `assinaturas_do_colab(..., so_escala=True)` (`app/escala/services/escala_certa.py:42`), que retorna antes do motor e de `efeitos_da_ausencia` (`app/ponto/services/esmeril_espelho.py:101`, `app/ponto/services/esmeril_espelho.py:105`, `app/ponto/services/esmeril_espelho.py:161`). O cache desse juiz é por colaborador/competência (`app/escala/services/escala_certa.py:38`, `app/escala/services/escala_certa.py:39`, `app/escala/services/escala_certa.py:40`, `app/escala/services/escala_certa.py:42`).

Já o esmeril completo isolado acrescenta **E1/P1** quando tem células, não usa `so_escala` e possui vínculo com tipo; retorna antes sem células ou com `so_escala=True` (`app/ponto/services/esmeril_espelho.py:95`, `app/ponto/services/esmeril_espelho.py:96`, `app/ponto/services/esmeril_espelho.py:101`, `app/ponto/services/esmeril_espelho.py:155`, `app/ponto/services/esmeril_espelho.py:161`). `anomalias_do_colab` executa o completo uma vez (`app/ponto/services/esmeril_espelho.py:197`); `anomalias_da_frota` executa uma vez por colaborador em operação não isento (`app/ponto/services/esmeril_espelho.py:217`, `app/ponto/services/esmeril_espelho.py:218`, `app/ponto/services/esmeril_espelho.py:222`).

## 14/11 20:49 · sessao 524713d2ee1e · - · - · origem=Ronald (tmux)

Inventário direto do delta. “Coerente” significa que a asserção demonstra o enunciado no escopo executado; não significa prova de produção. Os harnesses declaram bordas ORM substituídas e ausência de prova de frota/banco. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:1`, `app/ponto/tests/test_justificativa_soma_sem_banco.py:5`)

**`app/ponto/tests/test_justificativa_paga_o_periodo.py`**

| Teste novo/alterado | Verificação | [nao verificado]
|---|---|
| `test_RED_C8_ancora_numa_saida_conserva_o_dia_inteiro` | Só comentário alterado; testa retorno `None` e motivo de papel errado, não dinheiro/lista. :55 |
| `test_pausa_fora_do_cadastro_e_a_volta_abre_a_saida` | Só comentário alterado; prova cobertura `hf`, não saldo. :77 |
| `test_mapa_injetado_690_abona_sem_falta_nem_DSR_perdido` | Coerente com renomeação: injeta 690; não consulta a fonte da cura. :145 |
| `test_soma_S1_realizado_483_paga_zero_sem_perder_cobertura` | Coerente; realizado 483, saldo zero, data justificada e zero transportado à grade. :215 |
| `test_soma_S2_igualdade_real_paga_420` | Prova 720−300=420; “igualdade” não é a borda trabalhado=previsto. :254 |
| `test_soma_S3_um_minuto_real_acima_reduz_para_419` | Prova 720−301=419. :257 |
| `test_v2_fechamento_real_grava_mesmos_660_do_motor_e_do_dia_pago` | Chama motor, fechamento e DiaPago; acrescenta dia de abono=1. Tem mock de `ler_lavra`; execução ORM nesta análise [não verificado]. :302 |
| `test_C5_ancorada_cobre_so_a_saida_e_completa_720` | Coerente: cobertura, saldo720, feed vazio sem trabalho, grade720 e idempotência. :346 |
| `test_RED_D1_pausa_fora_do_cadastro_completa_saldo` | Prova 720−210=510 e `hf`; mensagem “não cruza” é inferência numérica, não medição geométrica. :373 |
| `test_RED_D2_saida_real_fora_do_raio_fecha_o_turno` | Coerente: conserva inteiro, trabalhado840, saldo0, feed0 e dia justificado. Não verifica HE/lista. :384 |
| `test_RED_D3_ultima_batida_e_saida_fecha_o_turno` | Coerente: conserva inteiro e saldo477 com trabalhado243. :396 |
| `test_RED_A_ancora_solta_horas_antes_do_turno_real` | Coerente: trabalhado720, saldo0, `hf` permanece. Não verifica lista. :413 |
| `test_RED_P1_o_papel_e_da_ata_nao_do_tipo_gravado` | Coerente: primeiro prova pareamento pela ata, depois saldo1 e conservação. :426 |
| `test_C7_sem_ancora_conserva_o_dia_inteiro` | Coerente: cobertura inteira, saldo720, feed vazio e nenhum missing. :444 |
| `test_RED_C8_ancora_de_papel_errado_conserva_o_dia_inteiro` | Coerente no fixture: inteiro,720, feed vazio e nenhum missing; referências à “frota” no docstring não são prova de frota. :458 |
| `test_C9_outro_segue_o_dia_inteiro` | Prova dia justificado/feed vazio/grade sem missing; não asserta diretamente o valor720. :474 |
| `test_c7_forma_col788_fonte_grade_motor_e_dsr_361` | Fonte361, overlay361 e motor361/sem falta/DSR0; chamada de `grade_da_celula` recebe361 manualmente. É fixture, não produção col788. :486 |
| `test_c7_A1_noturno_sobreposto_paga_60` | Coerente: fonte60 e autoridade/motor60, sem falta. Não asserta DSR semanal. :517 |
| `test_c7_A2_T2_motor_completo_preserva_falta_e_DSR_com_pagamento_zero` | Asserta depois sem falta,abono0 e ambos contadores DSR iguais ao antes; não asserta igualdade antes/depois dos contadores de falta. Execução ORM [não verificado]. :541 |

**`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py`**

| Teste alterado | Verificação | [nao verificado]
|---|---|
| `test_01_gravado_le_660_da_linha_sem_recalcular` | Prova fonte660 e leitor `por_dia_da_grade` respeitando inclusive123 manual; nenhuma gravação/persistência executada. :200 |
| `test_03_duas_ancoras_pagamento_unico_510` | Coerente com realizado210 fornecido por stub, saldo510 uma vez. :220 |
| `test_04_ausencia_inteira_precede_480_mapa_vazio` | Mapa vazio vem do efeito real;480 vem de outra grade fabricada manualmente, não do fechamento integrado. :230 |
| `test_08_grade_lida_pelo_fechamento_transporta_o_numero` | Prova leitor da ata alimentado manualmente com mapa720 e projeção720; não chama fechamento. :285 |
| `test_09_mesmo_numero_na_grade_e_no_bloco_real_de_abono_do_motor` | Coerente com escopo explicitado: extrai cinco instruções reais, compara1320/600 com grade manual. Não executa motor completo. :307 |
| `test_11_ancora_invalida_e_inteiro_conservam_sem_credito_parcial` | Problema: não verifica o mapa parcial. Último caso cria E07/S07:30, mas espera trabalhado0/pago720 porque o stub soma `self.trab`, deixado vazio. Não prova conta com par fechado. :349; stub :173 |
| `test_14_regressao_dos_casos_puros_da_v1` | Agora executa apenas `JuizDoMarcoCobertoTest`, limite >10; não preserva as antigas classes de medição/cruzamento. Comentário atualizado é coerente. :390 |

**`app/ponto/tests/test_justificativa_soma_sem_banco.py`**

| Teste novo/alterado | Verificação | [nao verificado]
|---|---|
| `test_S2_igualdade_e_inclusiva` | Prova saldo240 para480−240; não testa trabalhado=previsto. :97 |
| `test_S3_mais_um_minuto_trabalhado_reduz_saldo` | Coerente:241 trabalhados→239. :104 |
| `test_S4_480_sem_par_fechado_desconta_pausa` | Coerente: previsto líquido480,realizado0,saldo480. :111 |
| `test_S6_duas_ancoras_nao_duplicam_saldo_do_dia` | Coerente: duas aprovações,um saldo240. :133 |
| `test_realizado_liquido_da_autoridade_nao_span_bruto` | Coerente: realizado360 após pausa, saldo120. :148 |
| `test_trabalho_de_outro_dia_nao_entra_na_soma` | Coerente: outro dia excluído,realizado0,saldo540. :157 |
| `test_propriedade_saldo_maximo_zero_ou_previsto_menos_trabalho` | Prova fórmula para601 valores previstos e trabalho fixo120, mais idempotência. Não cobre todas as geometrias possíveis. :163 |
| `test_entrada_aberta_as_16_completa_480_sem_par_fechado` | Coerente. :201 |
| `test_sem_turno_none_da_autoridade_e_zero_trabalho` | Coerente para dois tipos e previsto480/0; verifica ausência no feed. :217 |
| `test_ramo_real_motor_abona_361_sem_falta_e_decide_dsr_ok` | Renomeação correta: executa ramo, decide `dsr_ok`, não contadores semanais. :255 |
| `test_grade_e_fonte_compartilham_previsto_e_zero_explicito` | Prova leitor de previsto compartilhado e transporte do zero fornecido; não integração completa do chamador financeiro. :312 |
| `test_A1_sobreposicao_nao_apaga_saldo_60_nem_marco` | Coerente: saldo60,cobertura `hf`,feed60. :345 |
| `test_A1_formula_e_feed_independem_de_cobertura_vazia` | Coerente sob retorno vazio forçado por mock; não prova ocorrência real dessa geometria. :353 |
| `test_A2_T2_sobreposicao_paga_zero_e_preserva_dia_e_dispensa` | Coerente no harness:zero,dispensa,motor sem falta/abono,decisão DSR; não contadores semanais. :364 |
| `test_A3_sem_trabalho_fora_do_mapa_motor_paga_um_dia_igual_periodo` | **Oculta bug de fonte única:** a conta do motor é substituída por lambda com o mesmo480 escolhido para a fonte. Não testa ata divergente dos marcos. :406; lambda :397 |
| `test_A4_T1_sem_motivo_orfao_de_excedente` | Prova saldo361 e ausência da chave/função antiga; não a chegada do excedente real à HE. :424 |
| `test_A7_feed_real_entrega_saldo_361_ao_mapa` | Coerente: efeito real do harness entrega361. :431 |
| `test_A3_dias_lavrados_separam_ausencia_inteira_de_saldo_em_trabalho` | Testa três atas e função real; queries são lambdas que ignoram todos filtros. Não prova seleção ORM/crédito positivo/competência. :436 |
| `test_A5_central_sem_segunda_lista_de_HE` | **Nome excede prova:** só verifica ausência de três strings, não ausência de outro juiz/lista nem funcionamento da porta HE. :451 |
| `test_A9_leitor_de_lampadas_nao_consulta_pagamento` | Coerente no leitor extraído, mocks garantem zero chamadas às duas funções. :456 |
| `test_A9_folha_fornece_numero_inteiro_a_grade_financeira` | Coerente na fronteira: função real chama Mock da grade com480; não executa grade/export inteiro. :479 |
| `test_A9_espelho_fornece_saldo_a_grade_financeira` | Executa apenas duas atribuições extraídas; prova argumento361, não ramo/try/`montar_dias` completo. :492 |

Outros testes afetados indiretamente pelo helper `limitado` agora também exigem ausência de `motivo`: `test_S1_saida_330_mais_483_paga_zero_sem_perder_dia`(:83), `test_S1_par_fechado_antes_da_E_solta_no_mesmo_turno`(:91), `test_S5_entrada_simetrica_completa_saldo`(:117), `test_S6_dois_marcos_somam_por_dia`(:124), `test_previsto_lavrado_vence_template_e_zero_nao_e_ausente`(:140), `test_T1_col788_paga_361_e_conserva_dia`(:185), `test_T7_folga_zero_conserva_cobertura`(:194); helper :72. “330” em S1 não é medido; “somam” em S6 prova união de marcos e saldo único239. O cabeçalho ainda diz “somam por dia, depois da união”. (`app/ponto/tests/test_justificativa_soma_sem_banco.py:4`, `app/ponto/tests/test_justificativa_soma_sem_banco.py:83`, `app/ponto/tests/test_justificativa_soma_sem_banco.py:124`)

**Sem mordida / execução real**

- O teste renomeado de mapa690 passa independentemente da cura na fonte: fornece o próprio mapa e só executa motor; é corretamente limitado pelo novo nome. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:145`)
- A5 passa com qualquer erro financeiro que preserve ausência das três strings; não chama pagamento. (`app/ponto/tests/test_justificativa_soma_sem_banco.py:451`)
- A3 pode passar com produção divergindo: usa `_minutos_previstos_dos_dias=lambda...previsto`, fonte e motor recebem o mesmo480. (`app/ponto/tests/test_justificativa_soma_sem_banco.py:397`, `app/ponto/tests/test_justificativa_soma_sem_banco.py:407`)
- Fiz contraprova em memória: ata420,marcos08–16=480,justificativa `outro`,sem turnos; extraindo os três métodos reais de previsto e o ramo DSR, a fonte pagou420 e o motor480. Causa: fonte lê ata; feed remove zero trabalho; motor reconsulta `minutos_previstos_do_dia`. A ocorrência em produção é [não verificado]. (`app/escala/services/leitor_celula.py:361`, `app/ponto/services/efeito_ausencia.py:48`, `app/ponto/motor_calculo_v2.py:1258`, `app/ponto/motor_calculo_v2.py:2631`)
- Runners executam trechos de produção reais extraídos por AST, não todos os caminhos de produção: `funcoes` compila apenas FunctionDef escolhidas; V2 substitui realizado/pareamento por lambdas; Soma restaura realizado real, mas turnos vêm de `self.ts`, montados no teste. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:38`, `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:173`, `app/ponto/tests/test_justificativa_soma_sem_banco.py:34`, `app/ponto/tests/test_justificativa_soma_sem_banco.py:43`, `app/ponto/tests/test_justificativa_soma_sem_banco.py:53`)
- Executei ambos runners com `PYTHONDONTWRITEBYTECODE=1 python3 -B`: Soma controle33/17 mortos; V2 controle14/17 mortos,rc0. Os runners exigem falha de asserção e rejeitam erro de execução; essa aprovação continua limitada aos harnesses acima. (`app/ponto/tests/mutantes_justificativa_soma.py:103`, `app/ponto/tests/mutantes_justificativa_soma.py:129`, `app/ponto/tests/mutantes_justificativa_periodo_v2.py:82`, `app/ponto/tests/mutantes_justificativa_periodo_v2.py:98`)

**Apagados e regra sobrevivente**

Os testes removidos de medição de cauda/recusa de sobreposição provavam a fórmula antiga, substituída pela lei do saldo; deduplicação/pausas/noturno têm cobertura atual pelos testes510/550/550 e saldo único. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:214`, `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:220`, `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:343`, `app/ponto/tests/test_justificativa_soma_sem_banco.py:133`)

Os removidos `test_fila_e_projecao_do_motivo_da_fonte` e `test_central_revisao_le_fila_uma_vez_por_colab_e_nao_rejulga` eram os testes do caminho retirado de lista; na cópia atual, A4/A5 só exigem sua ausência e não há substituto nesses arquivos provando excedente→porta HE→lista admin. A exclusividade em toda a história/base é [não verificado], pois não há `.git`; não atribuir a esses testes apagados uma prova da porta HE, que eles também não exercitavam. (`app/ponto/tests/test_justificativa_soma_sem_banco.py:424`, `app/ponto/tests/test_justificativa_soma_sem_banco.py:451`)

## 14/11 20:48 · sessao 7070790a6a5f · - · - · origem=Ronald (tmux)

P5 — censo estático da cópia, sem banco/log. Frequência efetiva por requisição, flags do ambiente e duração de produção: [não verificado].

**Chamadores diretos completos encontrados por nome/import alias no Python de produção:**

- `periodos_justificados`: `efeitos_da_ausencia` (`app/ponto/services/efeito_ausencia.py:46`); overlay `_marcar_ausencias` (`app/escala/utils.py:1198`); `grade_do_fechamento` (`app/folha/export.py:339`); `montar_dias` (`app/ponto/services/espelho.py:520`).
- `efeitos_da_ausencia`: coletor PDF (`app/relatorios/pdf_espelho.py:333`); `assinaturas_do_colab`, condicionado a vínculo/tipo (`app/ponto/services/esmeril_espelho.py:155`, `app/ponto/services/esmeril_espelho.py:161`); fechamento (`app/ponto/services/fechamento.py:665`); `autoridade_do_periodo` (`app/ponto/services/espelho.py:1071`); comandos de diagnóstico `diff_espelho_alimentacao` (`app/ponto/management/commands/diff_espelho_alimentacao.py:117`) e `diff_extrato_parcial._antes` (`app/relatorios/management/commands/diff_extrato_parcial.py:41`).
- `grade_da_celula` deixou de consultá-los implicitamente; recebe mapa ou `{}` (`app/escala/services/leitor_celula.py:369`, `app/escala/services/leitor_celula.py:381`).

**Contagens estruturais, quando chega ao fim sem exceção:**

Use `E=efeitos_da_ausencia`, `P=periodos_justificados`; cada `E` inclui um `P` (`app/ponto/services/efeito_ausencia.py:46`).

| Caminho por colaborador | E | P | Prova |
|---|---:|---:|---|
| `autoridade_do_periodo` | 1 | 1 | `app/ponto/services/espelho.py:1071` |
| Recalcular fechamento, inclusive modo leitura | 1 | 2 | `app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:1019`, `app/folha/export.py:339` |
| `grade_do_fechamento` isolada | 0 | 1 | `app/folha/export.py:339` |
| Builder por turno isolado | 0 | 1 | `app/escala/utils.py:1868`, `app/escala/utils.py:1198` |
| Lista de fechamento | 0 | 0 ou 1 | Só chama prontidão com fechamento+empresa: `app/ponto/services/fechamento_lista.py:85`; snapshot evita grade: `app/folha/export.py:1048`; fallback chama grade: `app/folha/export.py:1098` |
| Export TXT aceito pelo portão | 1 | 2 | Recalcula em leitura a empresa inteira antes do classificador: `app/folha/views.py:207`, `app/folha/views.py:208`, `app/folha/views.py:212` |

No recálculo, o laço é uma vez por colaborador elegível (`app/ponto/services/fechamento.py:619`); o efeito é anterior ao fatiamento e o mapa é reutilizado pelos motores (`app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:755`, `app/ponto/services/fechamento.py:758`). Portanto **não multiplica por número de vínculos/fatias**, mas a mesma apuração é repetida quando diferentes leitores chamam fechamento/espelho (`app/ponto/services/fechamento.py:758`, `app/ponto/services/espelho.py:1231`, `app/relatorios/cartao_pela_celula.py:73`).

Para o espelho/tela, defina:

- `G=1` quando entra na guarda de escala/tipo e alcança a consulta financeira da grade (`app/ponto/services/espelho.py:484`, `app/ponto/services/espelho.py:520`).
- `B=1` quando também monta o builder para o trecho anterior à competência ou pela exceção `AtaAgregadaSemMarcos` (`app/ponto/services/espelho.py:533`, `app/ponto/services/espelho.py:534`, `app/ponto/services/espelho.py:540`, `app/ponto/services/espelho.py:563`).
- `F=1` quando período pedido é competência, existe fechamento gravado e o colaborador participa do recálculo; `folha_manda` chama `totais_da_folha`, que **sempre recalcula com FM existente**, sem teste de frescor (`app/relatorios/cartao_pela_celula.py:321`, `app/relatorios/cartao_pela_celula.py:323`, `app/relatorios/cartao_pela_celula.py:71`, `app/relatorios/cartao_pela_celula.py:73`, `app/ponto/services/fechamento.py:1764`).

Assim `espelho_do_colab` custa **E=1+F; P=1+G+B+2F**: autoridade inicial, grade financeira, builder eventual e folha online (`app/ponto/services/espelho.py:1231`, `app/ponto/services/espelho.py:520`, `app/ponto/services/espelho.py:534`, `app/ponto/services/espelho.py:1288`). Com `G=F=1,B=0`: **E2/P4**; com trecho histórico: **E2/P5** pelas mesmas referências. Admin e app chamam esse caminho (`app/ponto/views.py:161`, `app/api/views.py:1354`); ficha também (`app/colaboradores/services/ficha.py:199`).

O PDF acrescenta **E1/P3**, chama o espelho completo e chama novamente `folha_manda`: efeito direto (`app/relatorios/pdf_espelho.py:333`), **dois builders** (`app/relatorios/pdf_espelho.py:476`, `app/relatorios/pdf_espelho.py:544`), espelho (`app/relatorios/pdf_espelho.py:599`), folha final (`app/relatorios/pdf_espelho.py:629`). Para os dois `F=1`, `G=1,B=0`: **E4/P9**; com builder histórico interno: **E4/P10**, pelas referências anteriores. Flags de produção e exceções podem mudar a contagem; o default do builder por turno é `True` (`app/config/settings/base.py:165`).

Calendário pode pagar **E1/P1 + F*(E1/P2)** quando foi solicitado o cálculo de dias em aberto sem fornecer a lista; com lista fornecida não entra nesse ramo (`app/colaboradores/services/calendario.py:563`, `app/colaboradores/services/calendario.py:564`, `app/colaboradores/services/calendario.py:566`, `app/colaboradores/services/calendario.py:570`, `app/colaboradores/services/calendario.py:583`).

O portão que **mede** exportação é mais caro que o POST: por colaborador classificado `entra`, chama um espelho e um PDF, mais uma autoridade independente para comparar DiaPago (`app/folha/porta_export.py:469`, `app/folha/porta_export.py:470`, `app/folha/porta_export.py:476`, `app/folha/porta_export.py:496`, `app/folha/porta_export.py:597`, `app/folha/porta_export.py:78`). Há acréscimos condicionais de grade por campo divergente (`app/folha/porta_export.py:461`, `app/folha/porta_export.py:139`) e um comando agregado ao final; não fechei o custo total desse comando aninhado [não verificado] (`app/folha/porta_export.py:667`). O POST só confere o carimbo antes do recálculo, não executa essa medição (`app/folha/views.py:128`, `app/folha/views.py:129`).

**Escrita de batida continua chegando aos três.**

1. Criação chama `_fechamento_por_evento` após `create`, inclusive conflito humano (`app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:155`, `app/ponto/registro_batida.py:157`, `app/ponto/registro_batida.py:160`). A porta chama `recalcular_por_evento` (`app/ponto/registro_batida.py:177`, `app/ponto/registro_batida.py:178`), agenda callback em `on_commit` (`app/ponto/services/fechamento.py:1829`) e executa recálculo do colaborador/competência (`app/ponto/services/fechamento.py:1814`, `app/ponto/services/fechamento.py:1815`). Esse caminho custa **E1/P2** se o colaborador for processado, e cada `P` chega a `realizado_do_dia` por dia aprovado (`app/ponto/services/efeito_ausencia.py:122`, `app/ponto/services/efeito_ausencia.py:125`).

2. O próprio `post_save` julga **ontem e hoje** (`app/ponto/signals.py:108`, `app/ponto/signals.py:112`, `app/ponto/signals.py:131`, `app/ponto/signals.py:135`), via cartório (`app/ponto/signals.py:92`, `app/ponto/signals.py:93`). Pode parar por reentrada, célula inexistente/futura ou turno de hoje não encerrado (`app/ponto/signals.py:90`, `app/ponto/services/cartorio.py:2025`, `app/ponto/services/cartorio.py:2028`, `app/ponto/services/cartorio.py:2033`, `app/ponto/services/cartorio.py:2037`).

3. Cada corpo efetivamente executado do cartório monta um builder (`app/ponto/services/cartorio.py:1175`), cujo overlay chama `P` (`app/escala/utils.py:1868`, `app/escala/utils.py:1198`) e cujo laço consulta `realizado_do_dia` por dia do envelope (`app/escala/utils.py:2061`, `app/escala/utils.py:2065`). Para julgamento individual o envelope tem **dois dias** (`app/ponto/services/cartorio.py:1173`, `app/ponto/services/cartorio.py:1174`). O corpo pode repetir por reconciliação e por emissão, portanto não é teto fixo 2 chamadas por batida (`app/ponto/services/cartorio.py:996`, `app/ponto/services/cartorio.py:1017`, `app/ponto/services/cartorio.py:1021`, `app/ponto/services/cartorio.py:1073`, `app/ponto/services/cartorio.py:1086`). Se `C` é número de corpos executados, esse ramo acrescenta **P=C**, e aproximadamente **2C consultas diretas de realizado + consultas internas dos P por dia aprovado**, no builder por turno (`app/ponto/services/cartorio.py:1174`, `app/escala/utils.py:2061`, `app/ponto/services/efeito_ausencia.py:122`).

4. Retratação também agenda fechamento (`app/ponto/registro_batida.py:274`, `app/ponto/registro_batida.py:281`); `anotar.save()` aciona os receivers (`app/ponto/registro_batida.py:254`, `app/ponto/signals.py:131`); apagar aciona `post_delete` do cartório (`app/ponto/registro_batida.py:291`, `app/ponto/signals.py:138`, `app/ponto/signals.py:140`). Deduplicação/recusa saem antes da criação e da chamada explícita de fechamento (`app/ponto/registro_batida.py:114`, `app/ponto/registro_batida.py:121`, `app/ponto/registro_batida.py:150`).

**Outro custo oculto:** cada `P` sem justificativa aprovada faz apenas a consulta inicial e retorna; havendo aprovada, recarrega células, todos os vínculos e turnos com ±1 dia, e chama realizado por dia distinto (`app/ponto/services/efeito_ausencia.py:89`, `app/ponto/services/efeito_ausencia.py:95`, `app/ponto/services/efeito_ausencia.py:99`, `app/ponto/services/efeito_ausencia.py:100`, `app/ponto/services/efeito_ausencia.py:104`, `app/ponto/services/efeito_ausencia.py:122`). Embora forneça turnos, não fornece `livre`/`intervalo`, então realizado volta a consultar o vínculo do dia (`app/ponto/services/efeito_ausencia.py:125`, `app/ponto/turnos.py:781`, `app/ponto/turnos.py:788`). Medição de queries/tempo dessa repetição nesta produção: [não verificado].

## 14/11 20:48 · sessao de9b8d116772 · - · - · origem=Ronald (tmux)

P6/P8 — leitura concluída; nenhum arquivo alterado. Execução de produção, banco e logs: [não verificado].

**P8: a entrega universal do excedente à lista NÃO está implementada.** `periodos_justificados` calcula `trabalhado` e `max(0, previsto-trabalhado)`, mas não publica excedente nem chama HE; o próprio docstring declara que `conserva` não cria fila (`app/ponto/services/efeito_ausencia.py:79-83,122-129`). A lista existente recebe **pontas fora de marcos**, pergunta diferente de `trabalhado>previsto` (`app/ponto/services/espelho.py:724-752`; `app/ponto/services/he_pendente_lavrado.py:132-153`).

Contraprova disponível no próprio teste novo: a empresa da fixture nasce sem configurar HE (`app/ponto/tests/test_justificativa_paga_o_periodo.py:181`), cujo default é `janela_he_ativa=False` e `desde=None` (`app/colaboradores/models.py:58-59,117-120`); A2 declara 450 previstos, 483 trabalhados e paga zero, mas só asserta pagamento/falta/DSR, nenhuma lista (`app/ponto/tests/test_justificativa_paga_o_periodo.py:541-568`). No montador dessa configuração `_he_pendencias=[]` não entra no ramo de HE (`app/ponto/services/espelho.py:724-725`), e dia sem essa lista é descartado do retrato (`app/ponto/services/he_pendente_lavrado.py:132-134`). É uma contraprova estática; resultado executado desse cenário: [não verificado].

Caminho que existe quando as condições permitem:

1. `montar_dias` lê cadastro HE uma vez (`app/ponto/services/espelho.py:384-392`), marca pontas da grade, acrescenta relato do motor e dia sem previsto (`app/ponto/services/espelho.py:725-752`), publica `he_fora_da_janela` (`app/ponto/services/espelho.py:779`).
2. `lavrar_he_pendente` chama `apurar`, que pede `espelho_do_colab` por colaborador ativo e lê essa lista (`app/ponto/management/commands/lavrar_he_pendente.py:62`; `app/ponto/services/he_pendente_lavrado.py:92-94,132-153`). `--apply` grava `MetricaSnapshot` (`app/ponto/management/commands/lavrar_he_pendente.py:70-73`; `app/ponto/services/he_pendente_lavrado.py:177-190`).
3. Cron configurado dispara o lavrador após `lavrar_deslogados`; depois dispara recusa de ponta pequena (`app/config/crons.py:1572-1587`). Execução efetiva do cron: [não verificado].
4. Rota `gestao-he/` chama a view (`app/ponto/urls.py:17`), que lê snapshot e chama `enriquecer`, sem medir o vivo (`app/ponto/views.py:932-946`). Template desenha colabs e dias (`app/templates/ponto/gestao_he.html:92-125`).
5. Form de decisão passa minutos à porta (`app/ponto/views.py:532-534`); `decidir_he` grava pela `_gravar` e agenda recalcular (`app/ponto/portas/he.py:181-202`). A porta não faz o excedente chegar à lista; recebe a decisão sobre o que já foi mostrado (`app/ponto/portas/he.py:143-149`).

Ramos que podem deixar o excedente sem essa lista:

- HE desligada, `desde` nulo ou dia anterior à vigência: nenhuma pendência (`app/ponto/services/espelho.py:724-725`).
- Só primeira entrada e última saída dos **marcos** são pontas; células `missing` puladas, saída depende de `saida_ativa`, delta sem sinal de fora é pulado (`app/ponto/janela_he.py:194-211`).
- Extra declarada absorve a ponta ou sobra dentro do piso: pulada (`app/ponto/janela_he.py:215-217`).
- Fallback do relato só aceita causa “janela de HE”, mesmo dia, sentido conhecido, saída ativa, sentido ainda não marcado, célula existente e `missing`, mesmo marco, minutos positivos (`app/ponto/janela_he.py:267-315`). Logo não cobre toda diferença entre soma do dia e previsto.
- Universo do retrato só colaboradores `situacao='ativo'`; erro de espelho exclui colaborador e incrementa erros; dia sem data legível/fora da janela excluído (`app/ponto/services/he_pendente_lavrado.py:92-100,114-119`).
- Sem pendência de HE publicada, dia excluído (`app/ponto/services/he_pendente_lavrado.py:132-134`).
- Sem snapshot, leitor devolve `(None,None)` e enriquecimento retorna lista vazia (`app/ponto/services/he_pendente_lavrado.py:193-206`; `app/ponto/services/gestao_he.py:236-244`). Snapshot atual/ocorrência em produção: [não verificado].
- Automação padrão atualiza competência atual e apenas anterior não entregue; anteriores mais antigas e anterior exportada ficam fora desse ciclo (`app/ponto/services/he_pendente_lavrado.py:218-225`).
- Busca, praça, posto, estado e dono podem retirar dias/colabs da tela (`app/ponto/services/gestao_he.py:317-337`). Ponta pequena **não desaparece** da lista geral: recebe estado `nao`; somente filtro `dono=admin` a exclui (`app/ponto/portas/he.py:276-279`; `app/ponto/services/gestao_he.py:278-286,333-337`).
- Acesso à tela exige `ver_folha` ou `autorizar_he` e empresa visível (`app/ponto/views.py:914-924`).

**P6: censo lexical dos apagados.** Executei busca recursiva em toda cópia e adicional em produção/comandos/templates/JS, excluindo testes/docs/migrations. Zero ocorrências executáveis para cada um: `_ordem_e_sentido_dos_marcos`, `_trecho_do_marco`, `trabalhado_do_turno`, `marcos_que_nao_cruzam_o_trabalhado`, `minutos_do_periodo_justificado`, `intervalos_do_periodo_justificado`, `_minutos_sem_repetir`, `justificativas_para_revisao`, `pago_mais_trabalhado_passa_do_previsto`, `status=revisao`, `j.revisao`. O resultado negativo não possui arquivo:linha persistido; prova por artefato de censo: [não verificado]. As únicas ocorrências na cópia são documentação histórica e evidências/testes; o arquivo `evidencia_justificativa_c7_auditoria.txt` inclui saída antiga que ainda mostra definições removidas, portanto não deve ser usado como estado atual (`app/ponto/tests/evidencia_justificativa_c7_auditoria.txt:1-14`). O teste atual só certifica ausência textual dos três elementos da Central (`app/ponto/tests/test_justificativa_soma_sem_banco.py:451-454`).

A remoção não apagou `fila_justificativas` nem sua URL (`app/ponto/views.py:1698`; `app/ponto/urls.py:38`). `?status=revisao` agora cai sem filtro de status e lista todas as justificativas: só pendente/aprovada/rejeitada têm ramos (`app/ponto/views.py:1703,1712-1718,1731-1736`). Link interno antigo não encontrado pelo censo; links externos/bookmarks: [não verificado]. Template não tem card de revisão e célula status mostra apenas status (`app/templates/ponto/fila_justificativas.html:48-58,112-113`).

Restou comentário contraditório “sem ancora = lista do admin” em `app/ponto/turnos.py:2662-2664`; implementação só registra `conserva` (`app/ponto/services/efeito_ausencia.py:116-117`).

Testes apagados pelo delta fornecido: [nao verificado]

- `MinutosDoPeriodoTest`: C1,C2,C3,C4,C5,C6, nada coberto, entrada coberta, propriedade parte+falta. Exclusão em arquivo antigo só consta do anexo, sem linhas da cópia: [não verificado]. Medir da ponta da âncora não é mais lei; pausas/noturno continuam cobertos por `test_02_duas_pausas_550` e `test_10_noturno_duas_pausas_nao_perde_a_data_do_turno` (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:214-218,343-347`).
- `CruzamentoComOTrabalhadoTest`: os sete casos da guarda de cruzamento. Exclusão só consta do anexo: [não verificado]. Retirar cobertura por sobreposição contraria agora o comportamento explicitamente testado por A1 (`app/ponto/tests/test_justificativa_soma_sem_banco.py:341-348`).
- `test_03b_uniao_de_trechos_distintos_sobrepostos_e_disjuntos`: perdeu objeto porque o pagamento é saldo único; duas âncoras continuam verificadas em `test_03_duas_ancoras_pagamento_unico_510` (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:220-227`).
- `test_fila_e_projecao_do_motivo_da_fonte` e `test_central_revisao_le_fila_uma_vez_por_colab_e_nao_rejulga`: exclusão só consta do anexo [não verificado]. **Não afirmar que eram prova correta de excedente real**: ambos usavam col788, cujo teste atual prova 59 trabalhados+361 pagos=420 previstos, portanto sem excedente (`app/ponto/tests/test_justificativa_paga_o_periodo.py:504-516`). O que falta é teste substituto da entrega do excedente real pela HE; os testes novos sobre fila só exigem remoção de símbolos (`app/ponto/tests/test_justificativa_soma_sem_banco.py:424-429,451-454`).

Não identifiquei teste removido que fosse a única prova **ainda correta sob o corte novo**; exclusividade absoluta perante toda suite não executada: [não verificado]. A lacuna comprovável é que A2 já constrói excedente real, mas termina sem verificar HE/lista (`app/ponto/tests/test_justificativa_paga_o_periodo.py:541-568`).

## 14/11 20:47 · sessao 2c6de581f85c · - · - · origem=Ronald (tmux)

Achados do motor, todos por leitura da cópia; a comparação executável com base/main ficou [não verificado].

- **Divergência concreta adicional: entrada única aberta, sem par fechado.** `periodos_justificados` calcula `trabalhado=0` e paga o previsto (`app/ponto/services/efeito_ausencia.py:125`). O feed remove esse dia de `min_justificados` (`app/ponto/services/efeito_ausencia.py:48`). Entretanto o motor inclui períodos abertos, com zero minutos, em `resultado.periodos` (`app/ponto/motor_calculo_v2.py:2499`, `app/ponto/motor_calculo_v2.py:2501`), e considera qualquer período como dia trabalhado (`app/ponto/motor_calculo_v2.py:2581`). Por isso o dia não entra em `_abono` e não recebe abono nenhum: **fonte=previsto; motor=0 minutos/0 dias**, embora não vire falta nem perca DSR (`app/ponto/motor_calculo_v2.py:2605`, `app/ponto/motor_calculo_v2.py:2611`, `app/ponto/motor_calculo_v2.py:2628`).

- **Ausência ABONA inteira com trabalho + justificativa:** a ausência remove o saldo do mapa (`app/ponto/services/efeito_ausencia.py:62`); o motor só paga abono inteiro quando o dia não consta em `datas_trabalho` (`app/ponto/motor_calculo_v2.py:2611`). Portanto, havendo período de trabalho, motor=**0 minutos/0 dias de abono**, enquanto a fonte bruta de justificativa continua retornando `max(0,P−T)` (`app/ponto/services/efeito_ausencia.py:128`). A grade financeira recebe a fonte bruta diretamente (`app/folha/export.py:340`).

Tabela do ramo real do motor, extraído por AST e executado em memória, com um dia obrigatório:

| Cenário | Minutos de abono | Dias | Fonte |
|---|---:|---:|---|
| (a) P480 sem batida | 480 | 1 | `app/ponto/motor_calculo_v2.py:2611`, `app/ponto/motor_calculo_v2.py:2630` |
| (b) P720/T600 | 120 | 0 | `app/ponto/services/efeito_ausencia.py:48`, `app/ponto/motor_calculo_v2.py:2633` |
| (c) P450/T483 | 0 | 0 | `app/ponto/services/efeito_ausencia.py:128`, `app/ponto/motor_calculo_v2.py:2633` |
| (d) P480/T420 | 60 | 0 | `app/ponto/services/efeito_ausencia.py:128`, `app/ponto/motor_calculo_v2.py:2633` |
| (e) ABONA inteira, sem trabalho/período | P | 1 | `app/ponto/services/efeito_ausencia.py:64`, `app/ponto/motor_calculo_v2.py:2630` |
| (e) ABONA inteira com período de trabalho | 0 | 0 | `app/ponto/services/efeito_ausencia.py:64`, `app/ponto/motor_calculo_v2.py:2611` |
| (f) parcial + justificativa, T>0 | max(0,P−T) | 0 | `app/ponto/services/efeito_ausencia.py:48`, `app/ponto/services/efeito_ausencia.py:58`, `app/ponto/motor_calculo_v2.py:2633` |
| (g) folga fora dos dias obrigatórios | 0 | 0 | `app/ponto/motor_calculo_v2.py:2601`, `app/ponto/motor_calculo_v2.py:2633` |
| (h) duas justificativas | mesmo saldo único | conforme trabalho/períodos | `app/ponto/services/efeito_ausencia.py:106`, `app/ponto/services/efeito_ausencia.py:122` |

Essas execuções verificam somente o ramo de falta/abono, **não motor completo, ORM, produção nem contadores semanais** [não verificado]. No ramo executado, todos os casos apresentaram `faltas=0` e `dsr_ok=True`; isso decorre de todo dia aprovado entrar em `justificadas`, independentemente de cobertura e pagamento (`app/ponto/services/efeito_ausencia.py:47`, `app/ponto/motor_calculo_v2.py:2605`).

Para P3: **não achei contraexemplo de nova falta/DSR no ramo do motor**; justificativas aprovadas sempre protegem o dia nesse cálculo (`app/ponto/services/efeito_ausencia.py:43`, `app/ponto/motor_calculo_v2.py:2607`). O override comercial também consulta `datas_justificadas` para decidir DSR (`app/ponto/motor_calculo_v2.py:3478`, `app/ponto/motor_calculo_v2.py:3490`). Isso não prova todos os outros leitores nem a comparação com base/main [não verificado].

Caminhos reais de injeção:

- Fechamento: efeitos → `datas_justificadas`/mapa → motor da fatia e motor órfão (`app/ponto/services/fechamento.py:664`, `app/ponto/services/fechamento.py:742`, `app/ponto/services/fechamento.py:755`, `app/ponto/services/fechamento.py:823`, `app/ponto/services/fechamento.py:831`).
- Espelho: mesmos efeitos → motor (`app/ponto/services/espelho.py:1071`, `app/ponto/services/espelho.py:1117`, `app/ponto/services/espelho.py:1137`).
- PDF: mesmos efeitos → motor (`app/relatorios/pdf_espelho.py:333`, `app/relatorios/pdf_espelho.py:407`).
- Esmeril: mesmos efeitos → motor (`app/ponto/services/esmeril_espelho.py:165`).
- Contagens de abono e DSR são somadas entre fatias (`app/ponto/services/fechamento.py:98`, `app/ponto/services/fechamento.py:298`).

Cuidado com resultado final: espelho chama `folha_manda`, então seus totais finais podem vir da lavratura, divergindo do resultado bruto do motor (`app/ponto/services/espelho.py:1287`). PDF preliminar acrescenta ausência parcial paga ao abono do motor (`app/relatorios/pdf_espelho.py:480`).

`dia_inteiro` tem efeito próprio no motor: zera atraso e saída antecipada, preservando trabalho; justificativas por si só não alimentam esse mapa de ausências (`app/ponto/services/efeito_ausencia.py:70`, `app/ponto/motor_calculo_v2.py:1090`, `app/ponto/motor_calculo_v2.py:1098`).

Porta de HE existente: decisão vigente é carregada por dia e a janela considera autorização (`app/ponto/motor_calculo_v2.py:1402`, `app/ponto/motor_calculo_v2.py:1658`). A chegada excedente pode ser clipada e os minutos separados (`app/ponto/janela_he.py:76`, `app/ponto/janela_he.py:102`). Provar que **todo** excedente dos casos fornecidos chega à lista do admin exige seguir o retrato/porta com dados de cadastro não fornecidos [não verificado].

## 14/11 20:47 · sessao e05797a4bd53 · - · - · origem=Ronald (tmux)

Auditoria dos leitores financeiros, somente por leitura.

- **Fonte e grade usam mapas distintos.** `efeitos_da_ausencia.min_justificados` inclui somente dias com `trabalhado > 0`; `grade_do_fechamento` e o espelho fornecem à grade todos os dias de `periodos_justificados`, inclusive dias sem trabalho. A grade só transporta o mapa quando `tipo_dia == 'trabalho'`. (`app/ponto/services/efeito_ausencia.py:48`; `app/folha/export.py:339`; `app/ponto/services/espelho.py:520`; `app/escala/services/leitor_celula.py:449`)

- **Ausência parcial tem precedência sobre a justificativa na grade.** `_marcar_ausencias` escreve `tipo_dia='ausencia'` também para parcial e, por usar `elif`, não aplica a justificativa naquele dia. `por_dia_da_grade` trata qualquer ausência como crédito `previsto - min(realizado, previsto)`, ignorando o período parcial e `min_justificados`. (`app/escala/utils.py:1207`; `app/escala/utils.py:1217`; `app/escala/utils.py:1227`; `app/escala/utils.py:1251`; `app/ponto/services/dia_pago.py:394`)

- **PDF tem soma própria adicional.** Antes de `folha_manda`, calcula `horas_abono = resultado.horas_abono + min_parcial_pagos/60`. Portanto, com previsto 480, trabalho 240, parcial paga 120 e justificativa pagando 240, o PDF de período livre informa crédito de **360 minutos**; motor e grade informam **240**. Na competência, `folha_manda` pode sobrescrever as horas para o número da grade. (`app/relatorios/pdf_espelho.py:479`; `app/relatorios/pdf_espelho.py:480`; `app/ponto/services/efeito_ausencia.py:48`; `app/ponto/services/efeito_ausencia.py:50`; `app/relatorios/cartao_pela_celula.py:321`; `app/relatorios/cartao_pela_celula.py:346`)

- **Cartão varia conforme existência de fechamento.** Havendo fechamento, lê `fechamento_lido` e devolve `horas_abono` de `minutos_abonados`, mas `_totais_do_fechamento` não devolve `dias_abono`: permanecem os dias previamente escritos pelo motor. Sem fechamento, havendo DiaPago, devolve horas da soma lavrada e dias por `dias_com_abono`. (`app/relatorios/cartao_pela_celula.py:69`; `app/relatorios/cartao_pela_celula.py:93`; `app/relatorios/cartao_pela_celula.py:118`; `app/relatorios/cartao_pela_celula.py:186`; `app/relatorios/cartao_pela_celula.py:190`; `app/relatorios/cartao_pela_celula.py:323`; `app/relatorios/cartao_pela_celula.py:342`)

- **Dias da parcial são contados como abono inteiro pela lavratura.** `dias_com_abono` aceita qualquer ata `tipo_dia='ausencia'`, sem testar `cobertura_parcial` ou realizado. Assim uma parcial com crédito positivo conta **1 dia**, enquanto a justificativa com trabalho conta **0 dias no motor**. (`app/ponto/services/dia_pago.py:700`; `app/ponto/services/dia_pago.py:703`; `app/ponto/motor_calculo_v2.py:2628`)

Tabela da **grade financeira**, assumindo ata coerente com o realizado informado:

| P2 | Crédito da grade |
|---|---:|
| a | 480 |
| b | 120 |
| c | 0 |
| d | 60 |
| e | `max(0, P − T_da_ata)` |
| f | `max(0, P − T_da_ata)` |
| g | 0 |
| h | um único saldo do dia |

A grade recebe saldo único de PJ para dias de trabalho; para ausências usa o previsto menos realizado da ata. Folga fica fora do universo da soma. (`app/folha/export.py:339`; `app/escala/services/leitor_celula.py:449`; `app/ponto/services/dia_pago.py:388`; `app/ponto/services/dia_pago.py:394`; `app/ponto/services/efeito_ausencia.py:106`; `app/ponto/services/efeito_ausencia.py:122`)

Contraexemplo **e**, previsto 480 e trabalho 240: ausência ABONA inteira retira o saldo do mapa do motor; como existe trabalho, o motor não inclui o dia em `_abono`, ficando **0 minutos/0 dias**. Grade devolve **240 minutos**; `dias_com_abono` devolve **1** se esse crédito foi lavrado. Cartão com fechamento fica **240 minutos/0 dias**; cartão apenas com lavratura fica **240 minutos/1 dia**. (`app/ponto/services/efeito_ausencia.py:62`; `app/ponto/services/efeito_ausencia.py:64`; `app/ponto/motor_calculo_v2.py:2611`; `app/ponto/motor_calculo_v2.py:2628`; `app/ponto/services/dia_pago.py:396`; `app/ponto/services/dia_pago.py:703`; `app/relatorios/cartao_pela_celula.py:118`; `app/relatorios/cartao_pela_celula.py:190`)

O espelho inicializa `dias_abono` com o resultado do motor; as horas de abono entram no resumo por `folha_manda`. Fora da competência, sua montagem não escreve `horas_abono` no resumo; o PDF livre escreve explicitamente motor mais parcial. (`app/ponto/services/espelho.py:1234`; `app/ponto/services/espelho.py:1256`; `app/ponto/services/espelho.py:1287`; `app/relatorios/cartao_pela_celula.py:321`; `app/relatorios/pdf_espelho.py:480`)

## 14/11 20:47 · sessao f710eb5486a5 · - · - · origem=Ronald (tmux)

P4: `dias_com_abono` tem regra própria de classe e cruza dados de momentos diferentes. Não é equivalente ao motor em todos os cenários. (`app/ponto/services/dia_pago.py:700-705`; `app/ponto/motor_calculo_v2.py:2611-2634`.)

**Dados efetivamente lidos e origem**

| Dado | Origem/escritor/quando |
|---|---|
| `colab, mes, ano, versao` | Argumentos do chamador; versão padrão `'motor'`. Único chamador de produção encontrado: `_totais_da_lavratura`, que passa colaborador/competência e usa a versão padrão. (`app/ponto/services/dia_pago.py:690`; `app/relatorios/cartao_pela_celula.py:141-143`; `app/relatorios/cartao_pela_celula.py:190`.) |
| `DiaPago.data`, `tipo='dia'`, `minutos_abonados>0` | `lavrar` recebe mapas da grade por data, grava `minutos_abonados` e cria a linha diária; substitui todas as linhas da competência/versão. (`app/ponto/services/dia_pago.py:135-141`; `app/ponto/services/dia_pago.py:288-292`; `app/ponto/services/dia_pago.py:345-360`.) |
| Mapa de abono que alimenta `lavrar` | Fechamento chama `grade_do_fechamento`, deriva os mapas com `por_dia_da_grade`, soma o abono no fechamento e passa os mapas para a lavratura. (`app/ponto/services/fechamento.py:1018-1019`; `app/ponto/services/fechamento.py:1065-1069`; `app/ponto/services/fechamento.py:1205-1218`.) |
| Regra do mapa de abono | Só considera classes `trabalho` e `ausencia`. `ausencia`: previsto menos realizado com cap; `trabalho`: apenas `min_justificados` truthy. (`app/ponto/services/dia_pago.py:388-404`.) |
| Outro escritor dos três campos `minutos_*` | `retratar_exportada` usa a mesma grade, cria/atualiza os três campos, zera dias que saíram da grade e grava por bulk update/create. A guarda pode barrar o retrato. (`app/ponto/services/dia_pago.py:454-486`; `app/ponto/services/dia_pago.py:491-500`; `app/ponto/services/dia_pago.py:537-548`.) |
| `CelulaDia.ata.tipo_dia` | Builder marca ausência inteira **e parcial** como `'ausencia'`; parcial recebe `cobertura_parcial=True`, mas a classe continua ausência. (`app/escala/utils.py:1217-1231`.) |
| `CelulaDia.ata.minutos_realizados` | Builder consulta `realizado_do_dia`, declara zero só quando sem turno e guarda os minutos reais sem o cap da lavratura. (`app/escala/utils.py:2065-2079`.) |
| Escrita persistente desses dois atributos da ata | `cartorio.ata_do_dia` copia ambos; cartório monta a grade e, com `apply_`, entrega a ata à lavra e carimba. Porta atribui `cel.ata`; `carimbar` salva junto dos demais campos. (`app/ponto/services/cartorio.py:738-740`; `app/ponto/services/cartorio.py:1175`; `app/ponto/services/cartorio.py:1850-1853`; `app/ponto/services/cartorio.py:1872-1874`; `app/ponto/portas/celula.py:1090-1098`; `app/ponto/portas/celula.py:1181-1201`.) |
| Momento da lavratura `DiaPago` | Fechamento normal, ou leitura com `lavrar_dias_pagos=True` para backfill; leitura pura não entra. (`app/ponto/services/fechamento.py:1196-1205`; `app/ponto/services/fechamento.py:1242`.) |

**Divergências concretas**

1. **P2(f), parcial paga + justificativa, com trabalho e saldo positivo**: por exemplo P=720, W=600, parcial=60. O feed preserva `min_justificados[d]=120` porque parcial faz `continue`; motor conta **0 dias**, pois o dia está no mapa. Grade classifica a parcial como ausência e gera abono 120; `dias_com_abono` conta **1 dia**, ignorando `cobertura_parcial` e W=600. (`app/ponto/services/efeito_ausencia.py:48`; `app/ponto/services/efeito_ausencia.py:58-64`; `app/ponto/motor_calculo_v2.py:2628-2634`; `app/escala/utils.py:1217-1231`; `app/ponto/services/dia_pago.py:396-399`; `app/ponto/services/dia_pago.py:703-705`.)

2. **P2(e), inteira ABONA + justificativa, com trabalho e saldo positivo**: efeitos remove o dia do mapa de saldo; se W pertence a `resultado.periodos`, `datas_trabalho` contém o dia, o motor não o inclui em `_abono` e resulta **0 dias / 0 abono**. Grade ausência gera P−W e `dias_com_abono` conta **1 dia**. Exemplo P720/W600: grade/DiaPago 120 e contador 1 contra motor 0/0. (`app/ponto/services/efeito_ausencia.py:62-64`; `app/ponto/motor_calculo_v2.py:2581-2584`; `app/ponto/motor_calculo_v2.py:2611-2634`; `app/ponto/services/dia_pago.py:394-399`; `app/ponto/services/dia_pago.py:703-705`.)

3. **ATA ausente/incompleta**: DiaPago pode ter 480 abonados, mas se não existe CelulaDia, `ata` é vazia ou `minutos_realizados` está ausente, `dias_com_abono` devolve **0**, pois exige `ata` truthy e igualdade literal `==0`; não trata ausência de chave como zero. O leitor da grade, em contraste, converte ausência da chave para zero. (`app/ponto/services/dia_pago.py:700-705`; `app/escala/services/leitor_celula.py:443`.) A existência desta situação em produção é [não verificado].

4. **Mistura de snapshots**: contador filtra versão/competência apenas no DiaPago e busca a **ata atual** por data; não verifica carimbo, versão ou correspondência temporal entre os dois. Relavra da ata pode mudar a contagem sem mudança no dinheiro lavrado. (`app/ponto/services/dia_pago.py:700-705`; `app/ponto/portas/celula.py:1058-1066`; `app/ponto/portas/celula.py:1090-1098`.) Ocorrência produtiva [não verificado].

Para P2(a), supondo ata atual `trabalho/realizados=0` e crédito lavrado, conta 1; b/d contam 0 com W>0; c conta 0 por crédito zero; g conta 0 porque crédito zero e folga não entra na classe; h conta por data, não por justificativa. (`app/ponto/services/dia_pago.py:700-705`; `app/ponto/services/efeito_ausencia.py:106-128`.) Sem dados de ata/lavratura fornecidos, resultados concretos produtivos são [não verificado].

**`cartao_pela_celula` seleciona outra regra conforme a fonte**

- Com fechamento existente, usa `fechamento_lido`; as **horas** de abono vêm de `fe.minutos_abonados`, mas `_totais_do_fechamento` não escreve `dias_abono`, deixando o valor do resumo/motor. (`app/relatorios/cartao_pela_celula.py:69-76`; `app/relatorios/cartao_pela_celula.py:93-123`; `app/ponto/services/espelho.py:1256`; `app/relatorios/pdf_espelho.py:479`.)
- Sem fechamento, havendo lavratura, usa horas da soma de DiaPago e **dias da nova `dias_com_abono`**. Portanto P2(f) pode apresentar os mesmos minutos e mudar de 0 para 1 dia só pela presença/ausência do fechamento. (`app/relatorios/cartao_pela_celula.py:186-190`; `app/relatorios/cartao_pela_celula.py:321-346`.)
- Em período livre, `folha_manda` não substitui os totais por esses ramos de competência. (`app/relatorios/cartao_pela_celula.py:321-346`.)

## 14/11 20:45 · sessao 8dd5b2ceddde · E3-V10-DEGRAU-DONO-E-PORTA · censo · origem=Code

A triagem resulta em **F=26, C=23, R=2, Q=4; total=55**, conforme as linhas abaixo. A regra aplicada é porta do dono, ação administrativa com tela e consulta plural por colaborador sem porta; a classificação anterior da entrega não foi tomada como veredito. (app/chamados/services/tarefa_do_dia.py:28; app/chamados/services/tarefa_do_dia.py:41; app/chamados/tests/e3_lu2_entrega.txt:49)

A comparação histórica exata com `de702cb2f` ficou **[não verificado]**: a base não está acessível nesta cópia; o executor anexado depende de `git show`, não contém o código anterior. (app/chamados/tests/e3_degrau_mutantes.py:178) Reexecução da suíte e comportamento depois das correções propostas: **[não verificado]**.

**Os 26 ERROR — F.** M8 prevê passar `agora`; a chamada produtiva envia `agora=self.agora`, mas três lambdas dos testes recusam esse argumento. (app/chamados/tests/e3_lu2_entrega.txt:26; app/chamados/services/tarefa_do_dia.py:589)

Formas exatas referidas na tabela:

- **B:** `lambda _c, datas, *, agora=None: dict.fromkeys(datas, False)` em app/chamados/tests/test_e3_bloqueantes.py:68.
- **L:** `lambda _c, datas, *, agora=None: dict.fromkeys(datas, False)` em app/chamados/tests/test_e3_leitores_c17_chamadores.py:66.
- **E:** `_juiz(sim)` retorna `lambda _colab, datas, *, agora=None: dict.fromkeys(datas, sim)` em app/chamados/tests/test_e4_backfill.py:36; patch herdado em app/chamados/tests/test_e4_backfill.py:74.

| teste | classe | arquivo:linha do sítio que decide | o que mudar | [nao verificado]
|---|---|---|---|
| `BloqueantesE3Test.test_MORDE_8_chamado_TERMINAL_segue_sem_canal` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar o dublê para B. |
| `BloqueantesE3Test.test_MORDE_8_o_vigia_nao_le_o_predicado_cru` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar o dublê para B. |
| `ChamadoresDoC17LeemATarefaTest.test_MORDE_nenhum_chamador_migrado_pergunta_ao_C17` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_RED_dias_sem_fio_nao_inclui_dia_so_fila` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_RED_fora_da_folha_o_dia_vai_ao_grupo_C` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_RED_o_que_segura_chamado_da_celula_sem_tarefa_tem_fio` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_controle_dias_sem_fio_pergunta_na_fila_tem_fio` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_controle_fora_da_folha_pergunta_na_fila_e_grupo_B` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_controle_o_que_segura_pergunta_na_fila_e_colab` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar o dublê para L. |
| `BackfillE4bTest.test_backfill_2x_mesmo_estado_uma_trilha` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` para E. |
| `BackfillE4bTest.test_backfill_dry_usa_porta_e_nao_persiste` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` para E. |
| `BackfillE4bTest.test_exportada_nao_muda_hash_fm_txt` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` para E. |
| `BackfillE4bTest.test_sem_competencia_so_lista_e_nao_chama_porta` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` para E. |
| `PassivoE4bTest.test_classe_c_passivo_entra_pela_porta_da_E1_e_2x_e_uma_linha` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` herdado para E. |
| `PassivoE4bTest.test_classe_e_viva_sob_terminal_reabre_pela_porta_com_nota` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` herdado para E. |
| `PassivoE4bTest.test_classes_e_universos_nao_somem_para_zerar` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` herdado para E. |
| `PassivoE4bTest.test_mutante_sem_porta_morde_caracterizacao_classe_e` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_Ch12_P90_D90_exportada_desfaz` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_Ch12_P90_D90_fora_da_competencia_K6_false_desfaz` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_DRY_diff_por_colaborador_le_dinheiro_da_autoridade` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_callback_desconhecido_nomeado_e_fila_revertida` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_callback_real_de_retomada_desfaz` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_escrita_EventoCelula_por_porta_aparece_no_diff` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_foto_inteira_sob_C_e_reversao_antes_do_primeiro_ato` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_lote_misto_desfaz_recusado_enquanto_vizinho_pousa` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_tabela_nao_fotografada_desfaz` | F | app/chamados/services/tarefa_do_dia.py:589 | Atualizar `_juiz` herdado para E. |

**Os 29 FAIL.** Nos testes de portas, `fila` agora nomeia chamados das portas **do dono**; `verbos` continua contexto do painel. Isso fundamenta as sete inversões abaixo. (app/chamados/services/tarefa_do_dia.py:41; app/chamados/services/tarefa_do_dia.py:624; app/chamados/tests/test_e3_degrau_dono_e_porta.py:551)

| teste | classe | arquivo:linha do sítio que decide | o que mudar | [nao verificado]
|---|---|---|---|
| `PortasDaTarefaTest.test_fila_A_pergunta_sob_B` | C | app/chamados/services/tarefa_do_dia.py:669 | LU-07/M5/M7: exigir `('porta_app', (), {a.pk:'decidir', b.pk:'cobrar'}, 'colab')`. |
| `PortasDaTarefaTest.test_passivo_sem_linha_ancora_fora_da_celula_porta_pergunta` | C | app/chamados/services/tarefa_do_dia.py:669 | LU-07/M5: exigir `('porta_pergunta', (b.pk,), {b.pk:'validar'})`, conservando passivo/`nao_sei`. |
| `PortasDaTarefaTest.test_porta_fila_quando_ancora_e_da_celula` | C | app/chamados/services/tarefa_do_dia.py:669 | LU-07/M7: exigir `('porta_app', (), {ch.pk:'cobrar'}, 'colab')`. |
| `PortasDaTarefaTest.test_v8_R10_fora_do_fato_conserva_ancora` | C | app/chamados/services/tarefa_do_dia.py:669 | LU-07/M5: exigir `('porta_fila', (b.pk,), {b.pk})`, mantendo ações vazias. |
| `PortasDaTarefaTest.test_v8_R4_encerrada_ancora_externa` | C | app/chamados/services/tarefa_do_dia.py:681 | LU-07/M5: exigir `fila=(b.pk,)` e acrescentar `acao=None, ausencia=None` ao dict esperado. |
| `PortasDaTarefaTest.test_v8_R8_escritor_real_sem_celula_dia_forte` | C | app/chamados/services/tarefa_do_dia.py:681 | LU-07/M5: exigir `fila=(ch.pk,)`, `acao=None, ausencia=None`, preservando a ligação pelo dia forte. |
| `PortasDaTarefaTest.test_v8_R9_ancora_com_nao_sei` | C | app/chamados/services/tarefa_do_dia.py:681 | LU-07/M5: exigir `fila=(b.pk,)`, `acao='validar', ausencia=None`, mantendo `nao_sei=True`. |
| `F3Beco1NaListaTest.test_RED_F6_C9_lavrado_divergente_nao_tira_o_dia_CADASTRO_da_mesa` | R | app/escala/services/cadastro_realidade.py:988 | Consultar o dono atual mesmo após excluir C11: o contraexemplo deixa C9=`BATIDA` antigo e o atalho conserva esse valor, embora o dia deva continuar `CADASTRO`. |
| `Selo14JuizEmLote.test_lote_responde_o_mesmo_que_o_avulso_e_com_queries_constantes` | Q | app/chamados/services/tarefa_do_dia.py:589 | Substituir igualdade do total de queries por orçamento da base mais consultas plurais por colaborador sem porta, mantendo a paridade dos resultados. |
| `SeloDeterminismoERelogio.test_lote_tem_queries_constantes` | Q | app/chamados/services/tarefa_do_dia.py:589 | Separar custo constante da base do custo adicional do calendário por colaborador e conferir memo/releituras sem nova consulta. |
| `FioMortoCenso.test_RED_censo_dono_fora_do_fio_le_uma_instancia_do_colab` | R | app/chamados/services/vigia_fio.py:347 | Consumir o gerador lendo/semeando `c.colaborador` antes de avançar: materializá-lo antes dessa leitura deixa três células do mesmo colaborador com três instâncias. |
| `AlimentacaoUnicaDoCensoAlcance.test_RED_fora_da_folha_semeia_o_mapa_com_os_retidos_que_carregou` | Q | app/chamados/services/tarefa_do_dia.py:589; app/ponto/motor_calculo_v2.py:1240 | Preservar zero refetch por PK e alimentar/reaproveitar colaborador e memos no caminho adicional do calendário; a associação exata das duas SQL a esse sítio é **[não verificado]**. |
| `AlimentacaoUnicaDoCensoAlcance.test_RED_o_que_segura_semeia_o_mapa_com_o_colaborador_pedido` | Q | app/chamados/services/tarefa_do_dia.py:589; app/ponto/motor_calculo_v2.py:1240 | Preservar zero refetch por PK e corrigir a alimentação do caminho adicional do calendário; a associação exata das duas SQL a esse sítio é **[não verificado]**. |
| `VigiaDoFioMortoTest.test_MORDE_a_property_viva_LE_o_juiz_e_NAO_tem_regra_propria` | C | app/chamados/services/tarefa_do_dia.py:293; app/chamados/services/vigia_fio.py:368 | LU-02/07/M7: manter a delegação property/C6, mas nestes fatos sem vínculo exigir ambas sem porta, `fio_morto=2` e `fio_vivo=0`. |
| `VigiaDoFioMortoTest.test_MORDE_folga_CONTESTADA_e_validada_e_ESCALA_NAO_DESCREVE_O_DIA_e_nao_fio_morto` | C | app/chamados/services/tarefa_do_dia.py:630; app/chamados/services/vigia_fio.py:259 | LU-03/M1/M7: sem âncora, marco exigível ou oferta, incluir `cel_n` no morto e exigir `(escala_nao_descreve_o_dia,fio_morto,fio_vivo)=(1,2,0)`. |
| `VigiaDoFioMortoTest.test_MORDE_o_carimbo_guarda_os_CONTADORES_que_a_rodada_mediu` | C | app/chamados/services/tarefa_do_dia.py:637 | LU-02/07: manter igualdade carimbo/censo e trocar `fio_morto>=1` por zero nestes fatos, pois a alimentação real conserva residual `decidir` com porta. |
| `VigiaDoFioMortoTest.test_MORDE_o_fio_vivo_e_o_fio_morto_dao_CONTAS_DIFERENTES` | C | app/chamados/services/tarefa_do_dia.py:293; app/chamados/services/vigia_fio.py:368 | LU-02/07/M7: exigir ambos mortos, `fio_morto=2`, `fio_vivo=0`, pois C6 vivo sozinho não fornece instância `responder` exigível. |
| `VigiaDoFioMortoTest.test_MORDE_sem_dia_de_fio_morto_a_PORTA_nao_e_varrida` | C | app/chamados/services/tarefa_do_dia.py:293; app/chamados/services/vigia_fio.py:368 | LU-02/07/M7: nestes fatos exigir `fio_vivo=0`, `fio_morto=1`; para conservar a prova de não varredura, fabricar efetivamente uma porta positiva. |
| `VigiaDoFioMortoTest.test_as_PARTES_SOMAM_o_total_e_o_fino_e_subconjunto` | C | app/chamados/services/tarefa_do_dia.py:656 | LU-03/07/M4/M7: exigir `fio_vivo=1`, pois somente Resolver abre porta nesta mistura, preservando as igualdades de partição. |
| `JuizDaPortaAlcancavelTest.test_MORDE_membro_da_fila_que_a_particao_nao_viu_nao_abre_porta` | C | app/chamados/services/tarefa_do_dia.py:656 | LU-02 + LU-03/M4: manter que verbo `None` não dá dono residual, mas exigir Folha `admin/porta_acao/resolver_dia` pela oferta independente. |
| `JuizDaPortaAlcancavelTest.test_registrado_na_celula_nao_e_porta` | C | app/chamados/services/tarefa_do_dia.py:656 | LU-03/M4: conservar C17 histórico sem porta e exigir Folha `admin/porta_acao/resolver_dia`. |
| `FuroSemCanalTest.test_MORDE_chamado_TERMINAL_nao_e_canal` | C | app/chamados/services/tarefa_do_dia.py:656 | LU-03/M4: excluir a célula da amostra sem canal, pois o terminal não impede a porta independente de Resolver. |
| `FuroSemCanalTest.test_a_celula_sem_canal_entra_no_contador` | C | app/chamados/services/tarefa_do_dia.py:656 | LU-03/M4: exigir exclusão da amostra e presença em `com_porta_acao`, apesar da ausência de pergunta/chamado. |
| `FuroSemCanalTest.test_esperado_e_zero_e_o_contador_nao_e_juiz` | C | app/chamados/services/furo_sem_canal.py:104 | LU-03/M4: manter `esperado=0` e exigir `total=0` nesta entrada; total positivo precisa de contraprova sem oferta. |
| `FuroSemCanalTest.test_o_contador_separa_furo_de_cobrado` | C | app/chamados/services/furo_sem_canal.py:105 | LU-03/M4: exigir `por_veredito={}` nesta entrada e testar a separação furo/cobrado sobre dias efetivamente sem porta. |
| `FuroSemCanalTest.test_pergunta_aberta_crua_SEM_porta_da_tarefa_nao_e_canal` | C | app/chamados/services/tarefa_do_dia.py:656 | LU-03/M4: manter que o carimbo cru não decide canal, mas exigir exclusão da amostra quando Resolver publica porta. |
| `HaikuForaDaFolhaTest.test_o_dia_que_acusa_diz_o_veredito_e_o_que_DESTRAVA` | C | app/chamados/services/tarefa_do_dia.py:656; app/folha/fora_da_folha.py:249 | LU-03/M4: exigir fio `admin`, ação/link `resolver_dia`, destrava administrativo e grupo A. |
| `GoldenDonoDoBloqueadoTest.test_GOLDEN_quantos_bloqueados_dependem_SO_do_admin_na_competencia` | C | app/chamados/services/tarefa_do_dia.py:656 | LU-03/M4: exigir fios `{'admin':2}`, preservando os dois bloqueados no balde Admin. |
| `GoldenDonoDoBloqueadoTest.test_MORDE_a_GOLDEN_o_dono_ANDA_quando_o_fio_do_dia_muda` | C | app/chamados/services/tarefa_do_dia.py:656; app/chamados/services/tarefa_do_dia.py:637 | LU-03/07/M4: exigir a transição `admin/Resolver → chamado/porta_fila → admin/porta_fila`, mantendo paridade ferramenta/retrato. |

Nos controles do vigia, **C não significa que C6 esteja errado**: a regra nova exige conjuntamente instância `responder` e pergunta pendente no app; `_dia` fabrica célula/pergunta sem vínculo, e o marco sem teto termina em `marco_fora_do_turno`, sem essa ação. Não é erro de assinatura/forma de dublê, portanto não cabe F na definição fornecida. (app/chamados/services/tarefa_do_dia.py:43; app/chamados/tests/test_o261_vigia_fio.py:87; app/chamados/services/tarefa_do_dia.py:293)

Nos dois testes de queries constantes, **M8 promete uma chamada plural por colaborador sem porta, memoizada; não promete total de SQL constante entre quantidades diferentes de colaboradores**. A base conserva sua promessa de consultas constantes, mas o caminho novo monta calendários por colaborador, podendo ainda dividir janelas longas. (app/chamados/services/tarefa_do_dia.py:20; app/chamados/tests/e3_lu2_entrega.txt:49; app/colaboradores/services/calendario.py:128)

Nos dois N1, os chamadores já passam os colaboradores à carga; alterar apenas essa semente não explica nem corrige necessariamente as SQL observadas. O caminho adicional verificável é calendário → `folha_manda` → totais da folha → `fechamento_lido` → apuração, cujo motor pode buscar o colaborador por PK; atribuir **exatamente as duas SQL do anexo** a esse caminho permanece **[não verificado]**, pois não há pilha das queries. (app/folha/fora_da_folha.py:181; app/folha/fora_da_folha.py:239; app/colaboradores/services/calendario.py:583; app/relatorios/cartao_pela_celula.py:73; app/ponto/services/fechamento.py:1759; app/ponto/motor_calculo_v2.py:1240)

**P1 — assinatura e propagação de `agora`.**

| função/caminho | recebimento e repasse |
|---|---|
| `resolver_dia_oferecido_nas_datas(colaborador, datas, *, agora=None)` | Recebe da tarefa e repassa ao contexto. (app/chamados/services/tarefa_do_dia.py:589; app/colaboradores/services/calendario.py:117; app/colaboradores/services/calendario.py:134) |
| `contexto_calendario(colaborador, GET, *, datas_em_aberto=None, agora=None)` | Usa o instante recebido para hoje, desfazer e D4; o default lê `timezone.now()`. (app/colaboradores/services/calendario.py:159; app/colaboradores/services/calendario.py:163; app/colaboradores/services/calendario.py:250; app/colaboradores/services/calendario.py:606) |
| `censo` e `censo_alcance` → `ListaDoAdmin` | Os dois repassam o instante à lista; o construtor e o repasse seguinte já existiam segundo a entrega. (app/chamados/services/vigia_fio.py:332; app/chamados/services/vigia_fio.py:573; app/chamados/services/vigia_fio.py:194; app/chamados/tests/e3_lu2_entrega.txt:52) |
| Comando `vigiar_fio_morto` | Lê o instante e envia à lista e aos dois censos. (app/chamados/management/commands/vigiar_fio_morto.py:171; app/chamados/management/commands/vigiar_fio_morto.py:175; app/chamados/management/commands/vigiar_fio_morto.py:180) |

Há **omissões produtivas**: `dataset_furos` recebe `agora` e passa ao D4, mas o omite no calendário do card; o drawer também lê um instante próprio e o omite ao chamar o calendário. A view chama o contexto pelo default. A entrega preserva esses defaults; serem regressões introduzidas neste commit é **[não verificado]**. (app/relatorios/furos.py:103; app/relatorios/furos.py:123; app/relatorios/furos.py:160; app/colaboradores/services/drawer.py:19; app/colaboradores/services/drawer.py:121; app/colaboradores/views.py:1949; app/chamados/tests/e3_lu2_entrega.txt:51)

Há também um repasse interno incompleto: o contexto chama `_pv(_q)` sem `agora`, embora C6 o aceite; o caminho de janela viva pode então cair em `date.today()`. Portanto a propagação até a plural/contexto não certifica ausência de relógio interno em toda a cadeia. (app/colaboradores/services/calendario.py:476; app/chamados/juizes.py:862; app/chamados/juizes.py:592)

Os dublês que quebram no dado são os três de teste B/L/E. Dublê produtivo quebrado ou chamador dinâmico adicional: **[não verificado]**. (app/chamados/tests/test_e3_bloqueantes.py:68; app/chamados/tests/test_e3_leitores_c17_chamadores.py:66; app/chamados/tests/test_e4_backfill.py:36)

**P2 — `fila`, rota por rota.** O “antes” abaixo é a expectativa antiga preservada nos testes, corroborada pelo mutante que recoloca `da_celula`; equivalência exata com a fonte de `de702cb2f`: **[não verificado]**. (app/chamados/tests/e3_degrau_mutantes.py:117; app/chamados/tests/e3_degrau_mutantes.py:178)

| rota | antes evidenciado | depois pela regra |
|---|---|---|
| `porta_app` | Chamado local podia aparecer: `(a.pk,)` ou `(ch.pk,)`. (app/chamados/tests/test_e3_portas_tarefa.py:53; app/chamados/tests/test_e3_portas_tarefa.py:83) | `()`: o dono é colaborador, a porta é o app. (app/chamados/services/tarefa_do_dia.py:650; app/chamados/services/tarefa_do_dia.py:669) |
| `porta_pergunta` | Âncora externa publicava `()`. (app/chamados/tests/test_e3_portas_tarefa.py:192; app/chamados/tests/test_e3_portas_tarefa.py:299) | Inclui âncora na fila pertencente ao dono: `(b.pk,)`. (app/chamados/services/tarefa_do_dia.py:644; app/chamados/services/tarefa_do_dia.py:669) |
| `porta_fila` | Chamados locais; externa/dia forte podia resultar em `()`. (app/chamados/tests/test_e3_portas_tarefa.py:254; app/chamados/tests/test_e3_portas_tarefa.py:291; app/chamados/tests/test_e3_portas_tarefa.py:309) | Portas do dono, incluindo externa/dia forte e excluindo residual de outro dono. (app/chamados/services/tarefa_do_dia.py:618; app/chamados/services/tarefa_do_dia.py:646; app/chamados/services/tarefa_do_dia.py:669) |
| `porta_acao` | Existência/forma na base: **[não verificado]**. | Ação administrativa alcançável sem chamado na fila; `fila=()`. (app/chamados/services/tarefa_do_dia.py:655; app/chamados/services/tarefa_do_dia.py:669) |
| `None` | Valor histórico exato: **[não verificado]**. | Sem dono publicável, `fila=()` nos casos `nada/nao_sei`. (app/chamados/services/tarefa_do_dia.py:648; app/chamados/services/tarefa_do_dia.py:669) |

M5 explicita a filtragem: residual `101:cobrar` e ação administrativa por `202:validar` resultam em `porta_pergunta`, `fila=(202,)`, embora ambos permaneçam em `verbos`. (app/chamados/tests/test_e3_degrau_dono_e_porta.py:551; app/chamados/services/tarefa_do_dia.py:624)

Quem lê o conteúdo em produção: `o_que_segura` transforma `fila` em `chamado`; `diff_e3_sombra` compara e conta seus elementos. `porta_da_tarefa` apenas transporta o campo. (app/folha/fora_da_folha.py:247; app/fol

[truncado em 20000 de 22722 caracteres]

## 14/11 20:43 · sessao 1b8bfc9ca4a2 · - · - · origem=Ronald (tmux)

Para as duas falhas N1, **Q**: conservar a exigência de nenhuma busca unitária do colaborador (`app/chamados/tests/test_n1_alimentacao_unica.py:169`; `:183`) e corrigir a alimentação/reutilização no caminho que o calendário acrescentou (`app/chamados/services/tarefa_do_dia.py:589`; `app/colaboradores/services/calendario.py:583`).

O caminho compatível com o SELECT completo por PK é:

`calendario.py:583` → `cartao_pela_celula.py:323` → `cartao_pela_celula.py:73` → `fechamento.py:1759` → builder `fechamento.py:724` → `fechamento.py:286` → `motor_calculo_v2.py:2618` → `:1294` → `:1273` → **SELECT em `:1240`**.

O builder do fechamento alimenta células e ausência, mas retorna sem preencher os memos de previsto/intervalo (`app/ponto/services/fechamento.py:750`–`:752`); o builder do espelho preenche esses memos (`app/ponto/services/espelho.py:1151`–`:1158`), portanto a chamada anterior do calendário à autoridade em `app/colaboradores/services/calendario.py:570` não basta para explicar nem evitar esse segundo caminho.

Atribuir **cada uma das duas SQL fornecidas** especificamente a `motor_calculo_v2.py:1240`: **[não verificado]**, porque o anexo não contém stack das SQL. O caminho estático e a forma da query sustentam essa inferência.

M8 promete uma consulta plural por colaborador necessário e memo na carga (`app/chamados/tests/e3_lu2_entrega.txt:26`; `:49`), não custo total constante em queries; a própria entrega deixa o custo em queries sem verificação (`app/chamados/tests/e3_lu2_entrega.txt:50`).

## 14/11 20:43 · sessao 4a4316c60eb6 · JUSTIFICATIVA-PAGA-O-PERIODO · construtor · origem=Code

Cura escrita na cópia. Evidência local: **47 testes sem banco verdes**, **17/17 mutantes c7 mortos** e **17/17 mutantes v2 mortos**, com controle verde antes de cada runner. Certificação pela suite/opus e resultados ORM: **[nao verificado]**. Saídas: [testes:50](ponto/tests/evidencia_justificativa_c7_verde.txt:50), [c7:36](ponto/tests/evidencia_justificativa_c7_mutantes.txt:36), [v2:19](ponto/tests/evidencia_justificativa_c7_v2_mutantes.txt:19).

| Item | Alteração e testemunha |
|---|---|
| **A1** | Retirada a guarda geométrica anterior à fórmula. O saldo é calculado independentemente de `cobre`: `ponto/services/efeito_ausencia.py:122`. Noturno paga 60; cobertura vazia também tem testemunha: `ponto/tests/test_justificativa_soma_sem_banco.py:345,353`. |
| **A2** | Toda aprovação permanece em `justificadas`; trabalho acima do previsto mantém zero explícito no feed: `ponto/services/efeito_ausencia.py:47`. T2 verifica cobertura, dispensa e crédito zero: `ponto/tests/test_justificativa_soma_sem_banco.py:364`. |
| **A3** | Dia sem trabalho sai de `min_justificados`, permitindo ao motor pagar/contar o dia inteiro. Com trabalho, permanece o saldo: `ponto/services/efeito_ausencia.py:48`; `ponto/motor_calculo_v2.py:2628`. O contador da lavratura distingue ausência inteira de saldo em trabalho: `ponto/services/dia_pago.py:690`. A propriedade período = pagamento integral do motor está em `ponto/tests/test_justificativa_soma_sem_banco.py:406`. |
| **A4** | O predicado pelo bruto e o campo público `motivo` foram eliminados após perderem o leitor com A5. T1 exige 361 e ausência desse campo: `ponto/tests/test_justificativa_soma_sem_banco.py:424`. O `motivo` local restante explica a conservação da âncora: `ponto/services/efeito_ausencia.py:162`. |
| **A5** | Removidos filtro, projeção, apresentação e testes exclusivos da segunda lista: `ponto/views.py:1714`; `templates/ponto/fila_justificativas.html:57`; censo em `ponto/tests/evidencia_justificativa_c7_auditoria.txt:32`. |
| **A6** | O grep da base confirmou o medidor sem chamador de produção. Removidos ele, geometria/bruto tornados órfãos e testes exclusivos. Grep anterior e posterior preservados: `ponto/tests/evidencia_justificativa_c7_auditoria.txt:1,32`. |
| **A7** | `feed_entrega_bruto` altera o **feed real** e é morto pela asserção `{dia: 361}` sobre `efeitos_da_ausencia`: `ponto/tests/mutantes_justificativa_soma.py:40`; `ponto/tests/test_justificativa_soma_sem_banco.py:431`. |
| **A8** | Renomeados os testes da propriedade e do ramo isolado de DSR; corrigidos cabeçalho e contratos ORM: `ponto/tests/test_justificativa_soma_sem_banco.py:163,255`; `ponto/tests/test_justificativa_paga_o_periodo.py:1`. |
| **A9** | Retirada a chamada financeira incondicional de `grade_da_celula`. Agora os leitores financeiros fornecem o mapa: `escala/services/leitor_celula.py:369`; `folha/export.py:340`; `ponto/services/espelho.py:521`. A chamada original está preservada em `ponto/tests/evidencia_justificativa_c7_auditoria.txt:17`. Retorno aos tetos 144/218: **[nao verificado]**. |

O RED foi executado antes da alteração de produção: registrou saldo `0 != 60`, desaparecimento de T2, dia sem trabalho no mapa parcial, contador `1 != 0` e consulta financeira pelo leitor de lâmpadas. Saída integral em [evidencia_justificativa_c7_red.txt:13](ponto/tests/evidencia_justificativa_c7_red.txt:13).

Sobre falta/DSR em T2:

- **Motor:** `datas_trabalho` nasce dos períodos; falta exige ausência tanto de trabalho quanto de justificativa: `ponto/motor_calculo_v2.py:2581,2607`. O ramo comercial conserva essa proteção: `ponto/motor_calculo_v2.py:3490`.
- **Grade:** a saída continua coberta porque a sobreposição deixou de esvaziar `cobre`; o overlay dispensa o marco e transporta zero: `ponto/services/efeito_ausencia.py:165`; `escala/utils.py:1258,1277`.
- **Espelho:** recebe o resultado da autoridade e transporta suas datas de falta: `ponto/services/espelho.py:1229,1254`. A comparação dos contadores completos de DSR antes/depois está escrita para execução ORM: `ponto/tests/test_justificativa_paga_o_periodo.py:541`; resultado **[nao verificado]**.

Sobre a HE de T2: **483 trabalhados / 450 previstos, isoladamente, não garante entrada**. Com janela ligada, entram as pontas reconhecidas conforme piso, saída ativa e extra declarada: `ponto/janela_he.py:204`; `ponto/services/espelho.py:725`. Com janela desligada, `_he_pendencias` permanece vazia: `ponto/services/espelho.py:724`. O retrato ignora lista vazia e a Gestão lê esse retrato: `ponto/services/he_pendente_lavrado.py:132`; `ponto/services/gestao_he.py:236`. Encaminhamento concreto em produção: **[nao verificado]**.

O diff de produção ficou nestes arquivos e funções:

| Arquivo::função | Resumo |
|---|---|
| `ponto/services/efeito_ausencia.py::periodos_justificados/_cobertura_de_uma/efeitos_da_ausencia` | Saldo diário, cobertura preservada e separação entre saldo e abono inteiro: linhas 28,72,132. |
| `ponto/turnos.py` | Exclusão dos helpers órfãos; censo em `ponto/tests/evidencia_justificativa_c7_auditoria.txt:32`. |
| `ponto/views.py::fila_justificativas` e template | Exclusão da revisão: `ponto/views.py:1714`. |
| `ponto/services/dia_pago.py::dias_com_abono` | Contagem de dias inteiros: linha 690. |
| `escala/services/leitor_celula.py::grade_da_celula` | Recebe o mapa financeiro por argumento: linha 369. |
| `folha/export.py::grade_do_fechamento` e `ponto/services/espelho.py::montar_dias` | Fornecem o mapa à grade financeira: linhas 340 e 521. |
| Testes e runners | REDs, contratos corrigidos e mutações em memória: `ponto/tests/test_justificativa_soma_sem_banco.py:345`; `ponto/tests/mutantes_justificativa_soma.py:95`; `ponto/tests/mutantes_justificativa_periodo_v2.py:84`. |

O contrato dos campos ficou assim:

| Campo | Quem escreve | Quem lê e significado |
|---|---|---|
| `minutos` | Fonte, fórmula diária: `efeito_ausencia.py:128` | Feed: `efeito_ausencia.py:48`; overlay: `escala/utils.py:1257,1277`; grade financeira: `folha/export.py:341`, `espelho.py:522`. Minutos pagos pela justificativa. |
| `cobre` | Juiz existente e união: `ponto/turnos.py:2727`; `efeito_ausencia.py:119,121` | Overlay: `escala/utils.py:1251,1258`. Dispensa de marcos, independente do pagamento. |
| `motivo` público | Removido | Sem leitor de produção; grep: `evidencia_justificativa_c7_auditoria.txt:32`. |
| `justificadas` | Fonte: `efeito_ausencia.py:47,63` | Fechamento: `fechamento.py:666`; espelho: `espelho.py:1117`; PDF: `pdf_espelho.py:334`; motor: `motor_calculo_v2.py:2607`. Aprovação protege o dia da falta. |
| `min_justificados` | Fonte: `efeito_ausencia.py:48,64` | Motor recebe pelo fechamento/espelho/PDF: `fechamento.py:755,831`; `espelho.py:1137`; `pdf_espelho.py:407`. Contém saldo dos dias com trabalho, inclusive zero; ausência da chave permite abono inteiro. |
| `dia_inteiro` | Juiz existente: `ponto/turnos.py:2764`; fonte transporta em `efeito_ausencia.py:70` | `espelho.py:1136`, `pdf_espelho.py:406`, motor em `motor_calculo_v2.py:1090`. Ausência inteira que zera descontos de pontualidade. |
| `trabalhado` | Autoridade existente transportada pela fonte: `efeito_ausencia.py:125,127` | Feed em `efeito_ausencia.py:48`. Distingue saldo de dia sem trabalho. |

Censo adicional dos leitores de fechamento, esmeril e comandos: `ponto/tests/evidencia_justificativa_c7_auditoria.txt:36`.

Comandos executados:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -v -k test_A ponto.tests.test_justificativa_soma_sem_banco

PYTHONDONTWRITEBYTECODE=1 python3 -m unittest ponto.tests.test_justificativa_soma_sem_banco ponto.tests.test_justificativa_periodo_v2_sem_banco -v

PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_soma --todos

PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_periodo_v2 --todos
```

Cada mutante c7 tem seu comando individual e resultado preservados em `ponto/tests/evidencia_justificativa_c7_mutantes.txt:2`.

Para a sessão executar com ORM, **não executado aqui [nao verificado]**:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  ponto.tests.test_justificativa_paga_o_periodo \
  relatorios.test_espelho_falta \
  relatorios.tests.test_s3_cartao_le_dia_pago \
  ponto.tests.test_selo_chokepoint_escrita
```

Nenhum caminho proibido aparece no delta rastreado; sintaxe e whitespace foram conferidos: `ponto/tests/evidencia_justificativa_c7_auditoria.txt:98,102,116`. Medições dos 41, dos nove colaboradores, impacto produtivo, exportadas e contagens reais de queries permanecem **[nao verificado]**. As divergências preexistentes D1/D2 e a janela HE ficaram fora da cura; delta em `ponto/tests/evidencia_justificativa_c7_auditoria.txt:81`.

`LEI-AKITA: origem=aval Ronald 14/11 19:4x (efeito_ausencia.py:76); testemunha=T1/noturno/T2 (test_justificativa_soma_sem_banco.py:185,345,364); RED=evidencia_justificativa_c7_red.txt:13; quem-mais-le=evidencia_justificativa_c7_auditoria.txt:36; juizes novos=0; padrao=single source of truth / um juiz por pergunta.`

## 14/11 20:42 · sessao dfbffc8f5424 · - · - · origem=Ronald (tmux)

Os 16 casos abaixo são **C** pela definição estrita de F: nenhuma destas falhas decorre de assinatura de dublê; as expectativas tratam C6 vivo ou ausência de chamado acionável como suficientes para decidir a porta, enquanto a lei exige a tarefa publicada e admite Resolver como ação direta. (`app/chamados/services/tarefa_do_dia.py:43`, `app/chamados/services/tarefa_do_dia.py:46`, `app/chamados/tests/e3_lu2_entrega.txt:22`)

| Teste | Classe | Sítio que decide | O que mudar | [nao verificado]
|---|---|---|---|
| `test_MORDE_o_fio_vivo_e_o_fio_morto_dao_CONTAS_DIFERENTES` | C | `app/chamados/services/tarefa_do_dia.py:293`; `app/chamados/services/vigia_fio.py:368` | LU-02/07/M7: com os fatos sem vínculo desta fixture, esperar ambos mortos, `fio_morto=2`, `fio_vivo=0`; C6 vivo sozinho não constitui instância `responder` exigível (`app/chamados/tests/test_o261_vigia_fio.py:145`, `app/chamados/tests/test_o261_vigia_fio.py:87`). |
| `test_MORDE_a_property_viva_LE_o_juiz_e_NAO_tem_regra_propria` | C | `app/chamados/services/tarefa_do_dia.py:293`; `app/chamados/services/vigia_fio.py:368` | LU-02/07/M7: conservar os asserts de delegação C6/property, mas esperar ambas as células sem porta e `fio_morto=2`, `fio_vivo=0` nos fatos atuais sem vínculo (`app/chamados/tests/test_o261_vigia_fio.py:249`). |
| `test_MORDE_folga_CONTESTADA_e_validada_e_ESCALA_NAO_DESCREVE_O_DIA_e_nao_fio_morto` | C | `app/chamados/services/tarefa_do_dia.py:293`; `app/chamados/services/tarefa_do_dia.py:630`; `app/chamados/services/vigia_fio.py:259` | LU-03/M1/M7: a resposta não validada sem âncora, vínculo ou oferta não abre porta; esperar `cel_n` no morto e contagens `(escala_nao_descreve_o_dia,fio_morto,fio_vivo)=(1,2,0)` (`app/chamados/tests/test_o261_vigia_fio.py:110`, `app/chamados/tests/test_o261_vigia_fio.py:657`, `app/chamados/tests/test_o261_vigia_fio.py:662`). |
| `test_MORDE_o_carimbo_guarda_os_CONTADORES_que_a_rodada_mediu` | C | `app/chamados/services/tarefa_do_dia.py:637`; `app/chamados/services/tarefa_do_dia.py:640` | LU-02/07: o command usa a alimentação real, cujo residual `decidir` dá porta; substituir o requisito `fio_morto>=1` por zero nesses fatos, preservando a igualdade carimbo/censo (`app/chamados/tests/test_o261_vigia_fio.py:797`, `app/chamados/tests/test_o261_vigia_fio.py:812`, `app/chamados/tests/test_o261_vigia_fio.py:831`). |
| `test_MORDE_sem_dia_de_fio_morto_a_PORTA_nao_e_varrida` | C | `app/chamados/services/tarefa_do_dia.py:293`; `app/chamados/services/vigia_fio.py:368` | LU-02/07/M7: a pergunta C6 viva sem marco exigível não garante porta; nos fatos presentes esperar `fio_vivo=0`, `fio_morto=1`, e construir um caso separado com instância responder alcançável para conservar o propósito de custo (`app/chamados/tests/test_o261_vigia_fio.py:460`). |
| `test_as_PARTES_SOMAM_o_total_e_o_fino_e_subconjunto` | C | `app/chamados/services/tarefa_do_dia.py:293`; `app/chamados/services/tarefa_do_dia.py:656` | LU-03/07/M4/M7: nesta mistura somente Resolver constitui porta, portanto esperar `fio_vivo=1`, mantendo as igualdades de partição e os demais baldes (`app/chamados/tests/test_o261_vigia_fio.py:674`, `app/chamados/tests/test_o261_vigia_fio.py:684`, `app/chamados/tests/test_o261_vigia_fio.py:686`). |
| `test_MORDE_chamado_TERMINAL_nao_e_canal` | C | `app/chamados/services/tarefa_do_dia.py:656` | LU-03/M4: terminal não cria porta residual, mas Resolver oferecido cria `admin/porta_acao/resolver_dia`; retirar a inclusão obrigatória dessa célula na amostra sem canal (`app/chamados/tests/test_o66_furo_sem_canal.py:55`, `app/chamados/tests/test_o66_furo_sem_canal.py:117`). |
| `test_a_celula_sem_canal_entra_no_contador` | C | `app/chamados/services/tarefa_do_dia.py:656` | LU-03/M4: ausência de pergunta/chamado não impede Resolver; esperar a célula fora da amostra e contabilizada em `com_porta_acao` (`app/chamados/tests/test_o66_furo_sem_canal.py:41`, `app/chamados/tests/test_o66_furo_sem_canal.py:95`). |
| `test_esperado_e_zero_e_o_contador_nao_e_juiz` | C | `app/chamados/services/furo_sem_canal.py:104` | LU-03/M4: manter `esperado=0`, mas aceitar `total=0` nesta fixture cujos quatro dias ganharam porta; exigir total positivo requer contraprova sem oferta (`app/chamados/tests/test_o66_furo_sem_canal.py:134`, `app/chamados/services/tarefa_do_dia.py:656`). |
| `test_o_contador_separa_furo_de_cobrado` | C | `app/chamados/services/furo_sem_canal.py:105` | LU-03/M4: esperar `por_veredito={}` nestes fatos com portas e reservar a separação furo/cobrado para uma entrada sem oferta (`app/chamados/tests/test_o66_furo_sem_canal.py:128`). |
| `test_pergunta_aberta_crua_SEM_porta_da_tarefa_nao_e_canal` | C | `app/chamados/services/tarefa_do_dia.py:656` | LU-03/M4: preservar que `respondida_em=None` cru não decide canal, mas esperar exclusão da amostra quando o juiz publica a porta independente de Resolver (`app/chamados/tests/test_o66_furo_sem_canal.py:98`). |
| `test_MORDE_membro_da_fila_que_a_particao_nao_viu_nao_abre_porta` | C | `app/chamados/services/tarefa_do_dia.py:637`; `app/chamados/services/tarefa_do_dia.py:656` | LU-02 + LU-03/M4: manter que verbo `None` não dá dono residual, mas substituir o fio Folha esperado por `admin`, com `porta_acao/resolver_dia` oferecido independentemente desse membro (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:292`). |
| `test_registrado_na_celula_nao_e_porta` | C | `app/chamados/services/tarefa_do_dia.py:656` | LU-03/M4: conservar o C17 histórico sem porta e esperar Folha `admin/porta_acao/resolver_dia`, pois registrado não impede a oferta (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:200`). |
| `test_o_dia_que_acusa_diz_o_veredito_e_o_que_DESTRAVA` | C | `app/chamados/services/tarefa_do_dia.py:656`; `app/folha/fora_da_folha.py:249` | LU-03/M4: substituir `nada/SEM PORTA/grupo D` por `admin`, ação `resolver_dia`, respectivo link e grupo A (`app/folha/tests/test_haiku_fora_da_folha.py:74`, `app/folha/fora_da_folha.py:99`). |
| `test_GOLDEN_quantos_bloqueados_dependem_SO_do_admin_na_competencia` | C | `app/chamados/services/tarefa_do_dia.py:656` | LU-03/M4: o furo sem chamado já tem Resolver; esperar fios `{'admin':2}`, preservando os dois bloqueados e o balde Admin (`app/folha/tests/test_haiku_fora_da_folha.py:491`). |
| `test_MORDE_a_GOLDEN_o_dono_ANDA_quando_o_fio_do_dia_muda` | C | `app/chamados/services/tarefa_do_dia.py:656`; `app/chamados/services/tarefa_do_dia.py:637` | LU-03/07/M4: atualizar a transição para `admin/Resolver → chamado/porta_fila → admin/porta_fila`, preservando paridade ferramenta/retrato e o balde Admin (`app/folha/tests/test_haiku_fora_da_folha.py:517`, `app/folha/tests/test_haiku_fora_da_folha.py:522`, `app/folha/tests/test_haiku_fora_da_folha.py:535`). |

P3: nos três casos `'nada' != 'admin'`, quem decide é a segunda passagem: tarefas sem rota são enviadas ao juiz plural (`app/chamados/services/tarefa_do_dia.py:588`); oferta verdadeira acrescenta ação `resolver_dia` e publica `admin/porta_acao` (`app/chamados/services/tarefa_do_dia.py:656`). O predicado da autoridade exige último recurso, passado e status em `falta/irregular/aberto/sem_certidao` (`app/colaboradores/services/calendario.py:94`, `app/colaboradores/services/calendario.py:103`). A Folha apenas lê o fio publicado (`app/folha/fora_da_folha.py:85`). Pela lei M4, **admin tem razão nestes fatos**, inclusive nos dois casos cujo C17 histórico continua sem porta ou cujo residual tem verbo `None` (`app/chamados/tests/e3_lu2_entrega.txt:22`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:545`).

A principal ressalva interpretativa nos quatro controles positivos do vigia é explícita: a lei pede **instância com responder + pergunta pendente no app**, não apenas `pergunta_viva=True`; o cenário sem vínculo perde a primeira condição em `marco_fora_do_turno` (`app/chamados/services/tarefa_do_dia.py:43`, `app/chamados/services/tarefa_do_dia.py:293`). Modernizar a fixture para fabricar um vínculo real preservaria o objetivo original, mas classificá-la F ampliaria a definição restrita de F dada pelo usuário. (`app/chamados/tests/test_o261_vigia_fio.py:87`)

## 14/11 20:41 · sessao 6a39842ebef1 · - · - · origem=Ronald (tmux)

Sete **C** e uma **R** neste recorte, pela definição de `fila` como portas do dono e pela preservação explícita de F6 na entrega (`app/chamados/services/tarefa_do_dia.py:41`; `app/chamados/tests/e3_lu2_entrega.txt:117`).

| Teste | Classe | Sítio que decide | O que mudar | [nao verificado]
|---|---|---|---|
| `PortasDaTarefaTest.test_fila_A_pergunta_sob_B` | C | `app/chamados/services/tarefa_do_dia.py:669` | LU-07/M5: exigir `('porta_app', (), {a.pk:'decidir', b.pk:'cobrar'}, 'colab')`; a pergunta pendente no app pertence ao colaborador (`app/chamados/tests/test_e3_portas_tarefa.py:53`; contraprova atualizada `:427`). |
| `PortasDaTarefaTest.test_passivo_sem_linha_ancora_fora_da_celula_porta_pergunta` | C | `app/chamados/services/tarefa_do_dia.py:669` | LU-07/M5: exigir `('porta_pergunta', (b.pk,), {b.pk:'validar'})`, preservando passivo/`nao_sei` (`app/chamados/tests/test_e3_portas_tarefa.py:190`; contraprova `:609`). |
| `PortasDaTarefaTest.test_porta_fila_quando_ancora_e_da_celula` | C | `app/chamados/services/tarefa_do_dia.py:669` | LU-07/M7: exigir `('porta_app', (), {ch.pk:'cobrar'}, 'colab')` (`app/chamados/tests/test_e3_portas_tarefa.py:83`; lei `app/chamados/services/tarefa_do_dia.py:43`). |
| `PortasDaTarefaTest.test_v8_R10_fora_do_fato_conserva_ancora` | C | `app/chamados/services/tarefa_do_dia.py:669` | LU-07/M5: exigir `('porta_fila', (b.pk,), {b.pk})`, preservando ações vazias (`app/chamados/tests/test_e3_portas_tarefa.py:309`; contraprova `:625`). |
| `PortasDaTarefaTest.test_v8_R4_encerrada_ancora_externa` | C | `app/chamados/services/tarefa_do_dia.py:669` | LU-07/M5: exigir `fila=(b.pk,)` e incluir `acao=None, ausencia=None` na projeção esperada (`app/chamados/tests/test_e3_portas_tarefa.py:254`; contraprova `:550`; projeção `app/chamados/services/tarefa_do_dia.py:681`). |
| `PortasDaTarefaTest.test_v8_R8_escritor_real_sem_celula_dia_forte` | C | `app/chamados/services/tarefa_do_dia.py:669` | LU-07/M5: exigir `fila=(ch.pk,)`, `acao=None, ausencia=None`, preservando porta/fio do chamado ligado pelo dia forte (`app/chamados/tests/test_e3_portas_tarefa.py:291`; contraprova `:598`; ligação `app/chamados/services/tarefa_do_dia.py:618`). |
| `PortasDaTarefaTest.test_v8_R9_ancora_com_nao_sei` | C | `app/chamados/services/tarefa_do_dia.py:669` | LU-07/M5: exigir `fila=(b.pk,)`, `acao='validar', ausencia=None`, preservando `nao_sei=True` (`app/chamados/tests/test_e3_portas_tarefa.py:299`; contraprova `:609`). |
| `F3Beco1NaListaTest.test_RED_F6_C9_lavrado_divergente_nao_tira_o_dia_CADASTRO_da_mesa` | R | `app/escala/services/cadastro_realidade.py:988` | Preservar a consulta ao dono atual do dia mesmo quando LU-03 exclui C11: o exemplo deixa somente C9 lavrado `BATIDA` e o atalho de assinatura única conserva o dono antigo, contrariando `CADASTRO` exigido e prometido na entrega (`app/escala/tests/test_f3_beco1_folga_na_lista.py:287`; `app/escala/services/cadastro_realidade.py:469`; `app/chamados/tests/e3_lu2_entrega.txt:117`). |

P2: a comparação histórica exata com `de702cb2f` permanece **[não verificado]**: a cópia não contém `.git`, e o executor de base busca fontes por `git show`, sem embuti-las (`app/chamados/tests/e3_degrau_mutantes.py:178`). A evidência de antes disponível são as expectativas antigas dos testes e o mutante que repõe `da_celula` (`app/chamados/tests/e3_degrau_mutantes.py:117`).

| Rota | Antes documentado pelos testes | Depois | [nao verificado]
|---|---|---|
| `porta_app` | Chamados locais, exemplos `(a.pk,)` e `(ch.pk,)` (`app/chamados/tests/test_e3_portas_tarefa.py:53`; `:83`). | `()` porque o dono é colaborador (`app/chamados/services/tarefa_do_dia.py:669`). |
| `porta_pergunta` | `()` para âncora externa (`app/chamados/tests/test_e3_portas_tarefa.py:192`; `:299`). | Âncoras na fila pertencentes ao dono, exemplos `(b.pk,)` (`app/chamados/services/tarefa_do_dia.py:644`; `:669`). |
| `porta_fila` | Chamados da célula; âncora externa/dia forte podia publicar `()` (`app/chamados/tests/test_e3_portas_tarefa.py:254`; `:291`; `:309`). | Portas vivas do dono, incluindo externa/dia forte; exclusão de residual de outro dono (`app/chamados/services/tarefa_do_dia.py:646`; `:669`; M5 `app/chamados/tests/test_e3_degrau_dono_e_porta.py:551`). |
| `porta_acao` | Rota anterior na base: **[não verificado]**. | Ação administrativa sem chamado na fila; `fila=()` (`app/chamados/services/tarefa_do_dia.py:655`; `:669`). |
| `None` | Valor anterior exato: **[não verificado]**. | Sem porta publicada e `fila=()` nos casos sem dono (`app/chamados/services/tarefa_do_dia.py:648`; `:669`). |

Leitores efetivos de `fila`: `o_que_segura` converte a tupla em `chamado` para o dia (`app/folha/fora_da_folha.py:247`; `:254`); `diff_e3_sombra` compara a projeção e imprime sua quantidade (`app/chamados/management/commands/diff_e3_sombra.py:50`; `:143`). `porta_da_tarefa` transporta o campo (`app/chamados/services/tarefa_do_dia.py:681`).

P4: a mensagem exata nasce em `motivo_de` quando o módulo não pertence a `MOTIVOS` nem aos aposentados (`app/chamados/catalogo/modulos.py:208`; `:216`). O caminho de abertura consulta o catálogo, captura esse `KeyError` e registra `abrir_fora_da_lei`; o comentário declara que a emissão continua (`app/chamados/models.py:988`; `:994`; `:995`; `:997`). O módulo concreto e o traceback dessa ocorrência não aparecem no dado dos 55: **[não verificado]**. O relato do diff não lista esses dois arquivos entre as alterações, mas a confirmação histórica de que o código não mudou nesta obra é **[não verificado]** (`app/chamados/tests/e3_lu2_entrega.txt:129`).

## 14/11 20:41 · sessao 1b8bfc9ca4a2 · - · - · origem=Ronald (tmux)

Os **26 ERROR são F no ponto exibido**, pela propagação prevista em M8 (`app/chamados/tests/e3_lu2_entrega.txt:26`; `app/chamados/tests/test_e3_degrau_dono_e_porta.py:609`): a produção chama a plural com `agora=self.agora` em `app/chamados/services/tarefa_do_dia.py:589`, e os três dublês antigos rejeitam esse keyword (`app/chamados/tests/test_e3_bloqueantes.py:68`; `app/chamados/tests/test_e3_leitores_c17_chamadores.py:66`; `app/chamados/tests/test_e4_backfill.py:36`). Eventuais falhas após corrigir o dublê: [não verificado].

Formas exatas usadas nas linhas abaixo:

- **B:** `lambda _c, datas, *, agora=None: dict.fromkeys(datas, False)` em `app/chamados/tests/test_e3_bloqueantes.py:68`.
- **L:** `lambda _c, datas, *, agora=None: dict.fromkeys(datas, False)` em `app/chamados/tests/test_e3_leitores_c17_chamadores.py:66`.
- **E:** `_juiz(sim)` deve retornar `lambda _colab, datas, *, agora=None: dict.fromkeys(datas, sim)` em `app/chamados/tests/test_e4_backfill.py:36`; o patch herdado está em `app/chamados/tests/test_e4_backfill.py:74`.

| teste | classe | sítio que decide | o que mudar | [nao verificado]
|---|---|---|---|
| `BloqueantesE3Test.test_MORDE_8_chamado_TERMINAL_segue_sem_canal` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e3_bloqueantes.py:195` | Atualizar o dublê para B. |
| `BloqueantesE3Test.test_MORDE_8_o_vigia_nao_le_o_predicado_cru` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e3_bloqueantes.py:201` | Atualizar o dublê para B. |
| `ChamadoresDoC17LeemATarefaTest.test_MORDE_nenhum_chamador_migrado_pergunta_ao_C17` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e3_leitores_c17_chamadores.py:184` | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_RED_dias_sem_fio_nao_inclui_dia_so_fila` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e3_leitores_c17_chamadores.py:154` | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_RED_fora_da_folha_o_dia_vai_ao_grupo_C` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e3_leitores_c17_chamadores.py:135` | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_RED_o_que_segura_chamado_da_celula_sem_tarefa_tem_fio` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e3_leitores_c17_chamadores.py:124` | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_controle_dias_sem_fio_pergunta_na_fila_tem_fio` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e3_leitores_c17_chamadores.py:159` | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_controle_fora_da_folha_pergunta_na_fila_e_grupo_B` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e3_leitores_c17_chamadores.py:143` | Atualizar o dublê para L. |
| `ChamadoresDoC17LeemATarefaTest.test_controle_o_que_segura_pergunta_na_fila_e_colab` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e3_leitores_c17_chamadores.py:131` | Atualizar o dublê para L. |
| `BackfillE4bTest.test_backfill_2x_mesmo_estado_uma_trilha` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4_backfill.py:150` | Atualizar `_juiz` para E. |
| `BackfillE4bTest.test_backfill_dry_usa_porta_e_nao_persiste` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4_backfill.py:119` | Atualizar `_juiz` para E. |
| `BackfillE4bTest.test_exportada_nao_muda_hash_fm_txt` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4_backfill.py:169` | Atualizar `_juiz` para E. |
| `BackfillE4bTest.test_sem_competencia_so_lista_e_nao_chama_porta` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4_backfill.py:137` | Atualizar `_juiz` para E. |
| `PassivoE4bTest.test_classe_c_passivo_entra_pela_porta_da_E1_e_2x_e_uma_linha` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4_passivo.py:57` | Atualizar `_juiz` herdado para E. |
| `PassivoE4bTest.test_classe_e_viva_sob_terminal_reabre_pela_porta_com_nota` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4_passivo.py:72` | Atualizar `_juiz` herdado para E. |
| `PassivoE4bTest.test_classes_e_universos_nao_somem_para_zerar` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4_passivo.py:98` | Atualizar `_juiz` herdado para E. |
| `PassivoE4bTest.test_mutante_sem_porta_morde_caracterizacao_classe_e` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4_passivo.py:92` | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_Ch12_P90_D90_exportada_desfaz` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4b_c4_orm.py:446` | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_Ch12_P90_D90_fora_da_competencia_K6_false_desfaz` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4b_c4_orm.py:346` | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_DRY_diff_por_colaborador_le_dinheiro_da_autoridade` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4b_c4_orm.py:550` | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_callback_desconhecido_nomeado_e_fila_revertida` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4b_c4_orm.py:171` | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_callback_real_de_retomada_desfaz` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4b_c4_orm.py:149` | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_escrita_EventoCelula_por_porta_aparece_no_diff` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4b_c4_orm.py:202` | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_foto_inteira_sob_C_e_reversao_antes_do_primeiro_ato` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4b_c4_orm.py:509` | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_lote_misto_desfaz_recusado_enquanto_vizinho_pousa` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4b_c4_orm.py:349` | Atualizar `_juiz` herdado para E. |
| `CercaC4ORMTest.test_RED_tabela_nao_fotografada_desfaz` | F | `app/chamados/services/tarefa_do_dia.py:589`; teste `app/chamados/tests/test_e4b_c4_orm.py:189` | Atualizar `_juiz` herdado para E. |

P1:

- As assinaturas novas documentadas são `resolver_dia_oferecido_nas_datas(colaborador, datas, *, agora=None)` (`app/colaboradores/services/calendario.py:117`) e `contexto_calendario(colaborador, GET, *, datas_em_aberto=None, agora=None)` (`app/colaboradores/services/calendario.py:159`); o contrato M8 exige recebimento/reenvio e contexto sem nova leitura de relógio (`app/chamados/tests/e3_lu2_entrega.txt:26`; `app/chamados/tests/test_e3_degrau_dono_e_porta.py:575`; `app/chamados/tests/test_e3_degrau_dono_e_porta.py:613`).
- Cadeia principal correta: `CargaDaTarefa.tarefa` envia `self.agora` à plural (`app/chamados/services/tarefa_do_dia.py:589`); plural envia `agora` ao contexto (`app/colaboradores/services/calendario.py:132`); contexto usa o instante recebido, só chama `timezone.now()` quando omitido (`app/colaboradores/services/calendario.py:163`), usa o mesmo instante no desfazer (`app/colaboradores/services/calendario.py:250`) e no D4 (`app/colaboradores/services/calendario.py:606`).
- Os dois censos agora enviam o instante ao `ListaDoAdmin` (`app/chamados/services/vigia_fio.py:332`; `app/chamados/services/vigia_fio.py:573`); o construtor já aceita `agora` (`app/chamados/services/vigia_fio.py:177`) e `_carregar` repassa à lista (`app/chamados/services/vigia_fio.py:194`), distinção expressa na entrega (`app/chamados/tests/e3_lu2_entrega.txt:52`). O comando abre o relógio em `app/chamados/management/commands/vigiar_fio_morto.py:171`, envia à lista em `:175` e a ambos os censos em `:180`/`:181`.
- Os demais chamadores do contexto são view (`app/colaboradores/views.py:1949`), drawer (`app/colaboradores/services/drawer.py:121`) e relatório (`app/relatorios/furos.py:160`), todos sem `agora`; a entrega preserva defaults dos demais chamadores (`app/chamados/tests/e3_lu2_entrega.txt:51`).
- Há omissão concreta de um instante já disponível no relatório: `dataset_furos` recebe `agora` (`app/relatorios/furos.py:103`) e o envia ao D4 (`app/relatorios/furos.py:123`), mas não ao calendário do card (`app/relatorios/furos.py:160`/`:164`), cujo default lê outro relógio (`app/colaboradores/services/calendario.py:163`). Drawer também lê `agora` no início (`app/colaboradores/services/drawer.py:19`) e o omite no calendário (`app/colaboradores/services/drawer.py:121`), além de leituras próprias de data (`app/colaboradores/services/drawer.py:43`; `:111`). Serem regressões introduzidas neste commit: [não verificado sem base/diff].
- Não há dublê de produção encontrado entre as referências estáticas dos símbolos; os dublês que quebraram estão nos testes citados. Chamada/dublê dinâmico: [não verificado]. [nao verificado]

## 14/11 20:27 · sessao defe70908db2 · E3-V10-DEGRAU-DONO-E-PORTA · construtor · origem=Code

A [entrega completa]( /home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/chamados/tests/e3_lu2_entrega.txt:1) contém diff por função, matriz M1–M8, leitores dos campos, custo por carga, remoções L-111 e classificação dos 65 métodos do recorte.

Execuções locais:

- **159 testes OK**, sem banco. [Saída](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/chamados/tests/e3_lu2_suite.txt:167) [nao verificado]
- **45/45 mutantes mortos**, com controle verde. [Saída e comandos individuais](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/chamados/tests/e3_lu2_mutantes.txt:1478) [nao verificado]
- **RED anterior à cura preservado**; M7 foi controle verde. [Saída original](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/chamados/tests/e3_lu2_red.txt:1) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_lu2_suite
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_degrau_mutantes --todos
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_degrau_mutantes --base
```

O último comando reproduz os REDs na base e retorna **1 esperado**. [Registro](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/chamados/tests/e3_lu2_entrega.txt:8)

Suite ORM completa, frota e timeout de 30 segundos: **[não verificado]**. Certificação do Code e opus final: **[não verificado]**. [nao verificado]

_5281 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
