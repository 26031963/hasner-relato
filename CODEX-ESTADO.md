# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 18:25** · publicado em **13/11/2026 18:26** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 18:25 · sessao 6adac308e008 · RAIA-PREPARACAO · censo · origem=Code

**LEU** = sítio aberto na cópia fornecida; **INFERIU** = interpretação ou proposta apoiada nos sítios citados. A identidade Git do HEAD, o histórico dos commits e a aplicação das refs são **[nao verificado]**: esta cópia não disponibiliza repositório Git. [nao verificado]

Os cruzamentos e os indicadores `main_moveu` abaixo vêm dos anexos fornecidos; confirmação independente desses metadados: **[nao verificado]**. Os estados do BACKLOG são tratados como registros, sem substituir a leitura do código.

## COBPARC-FILA-RELOGIO

1. **O que cura — LEU.** Fora do julgamento, `agora_deste_julgamento()` pode devolver `None`; recebendo teto ausente, `houve_fato_novo` consulta novamente o relógio. Assim, chamadas do mesmo lote podem avaliar instantes diferentes. Sítios: `app/ponto/services/cartorio.py:82`, `app/ponto/services/cartorio.py:373`.

2. **Existe no HEAD? SIM — LEU.** O c2 ainda passa o instante potencialmente ausente; o cluster faz o mesmo, e a retenção chega ao predicado sem instante fornecido pela borda. A guarda de reabertura também consulta fato novo sem teto explícito. Fontes: `app/chamados/reconciliador.py:877`, `app/chamados/reconciliador.py:884`, `app/ponto/services/cluster_espurio.py:164`, `app/ponto/services/cartorio.py:428`, `app/chamados/services/pergunta_no_app.py:144`, `app/chamados/services/disputa_emissao.py:940`, `app/chamados/models.py:1347`.

3. **Falta / reaplicação.** O BACKLOG registra mutantes_v4, suíte, opus e resposta do padrão pendentes; o censo da raia registrado ali não prova o HEAD. Fonte: `app/docs/BACKLOG.md:860`.

   **INFERIU:** exige adaptação ao caminho atual. O comando `reconciliar_chamados` agora é vigia; a retratação foi extraída para `retratar_premissa_morta_colab`, alcançada pelo consumidor. Reaplicar o antigo laço escritor do cron recuperaria um desenho ultrapassado. Fontes: `app/chamados/management/commands/reconciliar_chamados.py:1`, `app/chamados/management/commands/reconciliar_chamados.py:75`, `app/chamados/reconciliador.py:1446`, `app/chamados/reconciliador.py:1457`.

   O anexo marca `main_moveu=True` nestes arquivos — **[nao verificado]** por Git:

   - `app/chamados/management/commands/{disparar_perguntas_competencia,reabrir_orfas_enterradas,reconciliar_chamados,smoke_chamados}.py`;
   - `app/chamados/{models,reconciliador}.py`;
   - `app/chamados/services/{adesao,becos,competencia_trancada,disputa_emissao,fabrica_da_celula,pergunta_no_app}.py`;
   - `app/chamados/tests/test_selo_universo_reconciliador.py`;
   - `app/ponto/management/commands/{detectar_ausencias,detectar_cluster_espurio,processar_alertas_avancados,processar_alertas_turno,reconciliar_fantasmas,supra_juiz}.py`;
   - `app/ponto/services/{cartorio,cluster_espurio,geofence,justificativa}.py`;
   - `app/ponto/tests/test_o_outbox_celula.py` e `app/ponto/views.py`.

   Aplicação limpa e conflitos textuais: **[nao verificado]**.

4. **Arquivos em comum.** Comparação dos anexos: `app/chamados/models.py` com **BUG-PAR/O168/E3**; `app/chamados/services/disputa_emissao.py` com **BUG-PAR/E3**; nenhum com HJ-090. Confirmação fora dos anexos: **[nao verificado]**.

5. **Depende de.** Resposta do padrão e estacionamento por aval continuam registrados; a própria linha distingue estacionamento de trava L1/contrato 20. Não registra `!` do motor como bloqueio desta obra. Fonte: `app/docs/BACKLOG.md:860`.

6. **PROPOSTA — INFERIU: CONCLUIR**, após estabilizar as sobreposições com BUG-PAR/O168/E3 e resolver o estacionamento registrado, mantendo a prioridade inferior às obras bloqueantes L1/contrato 20. O defeito persiste, mas a retomada deve seguir os caminhos por evento do HEAD. Fontes: `app/docs/BACKLOG.md:860`, `app/ponto/services/cartorio.py:373`, `app/chamados/reconciliador.py:1446`.

## V3-RETOMA-PERIODOS-DA-CELULA

1. **O que cura — LEU.** A alimentação entrega segmentos como períodos e passa pausa agregada do núcleo; a intra perde a unidade de cada período. Além disso, a CCT é resolvida em `ini`, e o split noturno usa as extras do motor da fatia. Fontes: `app/ponto/calculador/alimentacao.py:158`, `app/ponto/calculador/alimentacao.py:515`, `app/ponto/calculador/regras.py:764`, `app/ponto/calculador/alimentacao.py:390`, `app/ponto/calculador/regras.py:827`.

2. **Existe no HEAD? PARCIAL — LEU.**

   - **Permanece:** segmentos em `pares_por_dia`, intra diária, CCT em `ini` e split pelos minutos do motor. Fontes: `app/ponto/calculador/alimentacao.py:158`, `app/ponto/calculador/alimentacao.py:390`, `app/ponto/calculador/regras.py:764`, `app/ponto/calculador/regras.py:827`.
   - **P4 permanece:** período sem dia/carimbo incrementa contador e segue por `continue`; a porta inicia o cálculo sem a recusa proposta. Fontes: `app/ponto/calculador/alimentacao.py:79`, `app/ponto/calculador/alimentacao.py:91`, `app/ponto/calculador/alimentacao.py:449`.
   - **P5 já está presente:** união dos intervalos, cortes por horas e teto no total. O RELATO atribui o pouso a **16c27de8**. Fontes: `app/ponto/motor_calculo_v2.py:162`, `app/ponto/motor_calculo_v2.py:186`, `app/ponto/motor_calculo_v2.py:196`, `app/docs/RELATO.md:7673`.
   - **Outra cura já presente:** a dobra deixou de retornar antes da noturna/pontualidade, e os intervalos já atravessam a alimentação. O RELATO registra **424632fd**, merge de **f9a65d66**. Fontes: `app/ponto/calculador/regras.py:793`, `app/ponto/calculador/alimentacao.py:107`, `app/ponto/calculador/alimentacao.py:527`, `app/docs/RELATO.md:7720`.

   Os hashes acima são atribuições documentais; ancestralidade Git: **[nao verificado]**.

3. **Falta / reaplicação.** Falta concluir períodos e pausas por período, CCT do dia, distribuição reversa da HE e recusa P4, com os REDs e a catraca pedidos. Fonte: `app/docs/BACKLOG.md:964`.

   **INFERIU:** P4 precisa chegar como resultado declarado ao consumidor, pois o fechamento captura genericamente falhas do oráculo. Fontes: `app/ponto/services/fechamento.py:1266`, `app/ponto/services/fechamento.py:1297`.

   O anexo marca `main_moveu=True` em `app/ponto/calculador/{alimentacao,periodo,regras}.py`, `app/ponto/motor_calculo_v2.py` e `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py` — **[nao verificado]** por Git.

   **INFERIU:** reaproveitar somente os deltas restantes, preservando P5, a retirada do retorno da dobra e o transporte atual dos intervalos. Esses trechos da raia já se sobrepõem a curas presentes. Fontes: `app/ponto/motor_calculo_v2.py:162`, `app/ponto/calculador/regras.py:793`, `app/ponto/calculador/alimentacao.py:107`. Conflitos textuais: **[nao verificado]**.

4. **Arquivos em comum.** Nenhum dos arquivos da raia aparece na lista fornecida das quatro obras em voo. Confirmação fora dos anexos: **[nao verificado]**.

5. **Depende de.** O desenho de períodos, ancoramento e transbordo reverso já está registrado; “espera P5” não descreve mais integralmente o código atual. Fontes: `app/docs/BACKLOG.md:964`, `app/ponto/motor_calculo_v2.py:162`.

   Nova alteração do motor exige aval explícito; a troca V3 ainda tem cobertura integral do impacto e lista de `!` por exportada registradas como pendentes. RECORTE-LONGO e HE-NOTURNA-POR-PEÇA são obras distintas. Fontes: `CLAUDE.md:142`, `app/docs/BACKLOG.md:1797`, `app/docs/BACKLOG.md:966`, `app/docs/BACKLOG.md:972`.

6. **PROPOSTA — INFERIU: CONCLUIR**, como restante da retomada V3, preservando as partes já recebidas. Manter a posição documentada da V3 — após O10-B, antes de OTIMIZA-3 — condicionada ao impacto completo; situação atual dessas obras antecedentes: **[nao verificado]**. Fontes: `app/docs/BACKLOG.md:1797`, `app/ponto/calculador/alimentacao.py:158`, `app/ponto/calculador/regras.py:764`.

## V3-RETOMA-FILA

1. **O que cura — LEU.** O instrumento descarta a lavra real e deriva sozinho o lado v2 do DIFF, sem ler HE100 de feriado nem os splits noturnos; também reúne toda folga trabalhada numa rubrica. Fontes: `app/ponto/management/commands/diff_calculador.py:123`, `app/ponto/management/commands/diff_calculador.py:137`, `app/ponto/management/commands/diff_calculador.py:245`, `app/ponto/management/commands/diff_calculador.py:263`.

2. **Existe no HEAD? SIM para o instrumento — LEU.** A lavra continua substituída por `_nada`, e os laços próprios continuam formando o lado v2. A lavra verdadeira contém as rubricas e a separação de folgas omitidas. Fontes: `app/ponto/management/commands/diff_calculador.py:137`, `app/ponto/management/commands/diff_calculador.py:247`, `app/ponto/services/dia_pago.py:181`, `app/ponto/services/dia_pago.py:234`, `app/ponto/services/dia_pago.py:244`.

   O item CCT em `ini` também persiste; os números de divergência e o caso nominal de cadastro dos demais itens são registros do BACKLOG, sem confirmação atual da frota. Fontes: `app/ponto/calculador/alimentacao.py:390`, `app/docs/BACKLOG.md:976`; ocorrência atual: **[nao verificado]**.

3. **Falta / reaplicação.** A raia fornecida resolve o instrumento; não encerra, por si, o re-DIFF, a classificação das divergências ou o cadastro. A linha ainda registra dependência do pouso da V3. Fonte: `app/docs/BACKLOG.md:976`.

   **INFERIU:** a captura precisa distinguir lavra completa, ausente e falha, porque o fechamento absorve exceções da lavratura. Fonte: `app/ponto/services/fechamento.py:1238`.

   Segundo o anexo, **nenhum** dos dois arquivos tem `main_moveu=True`. Isso não certifica aplicação limpa; aplicação e conflitos textuais: **[nao verificado]**.

4. **Arquivos em comum.** Nenhum dos dois arquivos da raia consta na lista das obras em voo. Confirmação fora dos anexos: **[nao verificado]**.

5. **Depende de.** Pouso próprio de instrumento L-105 e dependência POUSO-DA-V3 registrada. A correção da CCT deve ser consolidada com a obra de períodos, pois ambas atingem o mesmo sítio — **INFERIU**. Fontes: `app/docs/BACKLOG.md:976`, `app/docs/BACKLOG.md:964`, `app/ponto/calculador/alimentacao.py:390`.

6. **PROPOSTA — INFERIU: CONCLUIR**, em pouso de instrumento, antes do próximo DIFF usado para apresentar o impacto da V3. Essa posição sugerida precisa ser conciliada pelo chat com o bloqueio POUSO-DA-V3 registrado; não considero essa dependência revogada. Fontes: `app/docs/BACKLOG.md:976`, `app/ponto/management/commands/diff_calculador.py:247`.

## OTIMIZA-1

1. **O que cura — LEU.** Falta registrar as suítes inteiras e publicar inteiras por pouso no ESTADO. A trava atual mantém o dono do lock e executa o comando, sem produzir essa medição. Fontes: `app/docs/RELATO.md:1648`, `bin/trava_teste.sh:35`, `bin/trava_teste.sh:42`.

2. **Existe no HEAD? SIM — LEU.** A execução termina em `"$@"`, e a composição atual do painel não inclui o quadro contratado de suítes/pouso. Fontes: `bin/trava_teste.sh:42`, `bin/gerar_estado.py:370`. Commit de eventual cura por outro caminho: **[nao verificado]**.

3. **Falta / reaplicação.** Suíte, replay, verificador e pouso próprio permanecem registrados; PRONTO 5 não tem mapa histórico promovido no relato consultado. Fontes: `app/docs/BACKLOG.md:2083`, `app/docs/RELATO.md:1637`.

   O anexo marca `main_moveu=True` em `bin/gerar_estado.py`, `bin/suite.sh` e `bin/tests/test_suite_sh.sh` — **[nao verificado]** por Git.

   **INFERIU:** o porte deve preservar as novas seções do painel e as guardas de motivo da suíte inteira, junto do selo correspondente. Fontes: `bin/gerar_estado.py:277`, `bin/suite.sh:133`, `bin/tests/test_suite_sh.sh:31`. Conflitos textuais: **[nao verificado]**.

4. **Arquivos em comum.** A raia original, composta por `bin/*`, não cruza a lista das obras em voo. O complemento de carga alcança `app/chamados/management/commands/vigiar_fio_morto.py`, comum a **BUG-PAR/HJ-090/E3** na lista raspada. Cruzamentos fora dos anexos: **[nao verificado]**. O complemento está expressamente registrado para depois da E3: `app/docs/BACKLOG.md:738`.

5. **Depende de.** SUITE-POR-IMPACTO/PRONTO 5 para fechar a medição do comboio e pouso separado de instrumento pela L-105. O complemento de carga exige contrato anterior e depende da E3; isso não é evidência de implementação do complemento. Fontes: `app/docs/RELATO.md:1645`, `app/docs/LEIS.md:131`, `app/docs/BACKLOG.md:738`.

6. **PROPOSTA — INFERIU: CONCLUIR**, como fechamento do mesmo comboio de SUITE-POR-IMPACTO, depois da prova PRONTO 5. Manter o complemento de carga separado como pendência pós-E3, sem declará-lo construído. Fontes: `app/docs/BACKLOG.md:2083`, `app/docs/RELATO.md:1637`, `app/docs/BACKLOG.md:738`.

## SUITE-POR-IMPACTO [nao verificado]

1. **O que cura — LEU.** Selecionar testes por cobertura teste→arquivo, mantendo os casos de suíte inteira e certificando a árvore do pouso. Hoje `--volta` seleciona por `vizinhos_da_volta.py`; não entrega o contrato de coverage. Fontes: `app/docs/PROMPTS.md:1682`, `bin/suite.sh:82`, `bin/suite.sh:88`.

2. **Existe no HEAD? SIM no núcleo; PARCIAL nas guardas adjacentes — LEU.** A seleção por vizinhos permanece. A régua ainda possui saída “JÁ VERDE” por impressão de `.py/.html` antes dos selos de host; porém o pre-push já exige também hash da árvore. Portanto a descrição ampla do BACKLOG sobre essas duas portas envelheceu parcialmente. Fontes: `bin/suite.sh:88`, `bin/regua.sh:33`, `bin/regua.sh:84`, `bin/regua.sh:138`, `bin/pre-push.sh:65`, `app/docs/BACKLOG.md:1037`. Commit responsável: **[nao verificado]**.

3. **Falta / reaplicação.** Mapa, replay PRONTO 5 e rebase continuam registrados. O diagnóstico histórico relata falhas na geração e mapa expirado; não certifica o replay. Fontes: `app/docs/BACKLOG.md:1036`, `app/docs/RELATO.md:1635`, `app/docs/RELATO.md:1637`.

   O contrato exige testar alterações de testes e reproduzir a regressão com o mapa apropriado; o complemento distingue alterações de `.py` de produção que forçam inteira. Fontes: `app/docs/PROMPTS.md:1683`, `app/docs/PROMPTS.md:1686`.

   Mesmos `main_moveu=True` de OTIMIZA-1: `bin/gerar_estado.py`, `bin/suite.sh`, `bin/tests/test_suite_sh.sh` — **[nao verificado]** por Git. **INFERIU:** preservar a detecção de seleção efetivamente inteira e os motivos `pre-opus|pouso`. Fontes: `bin/suite.sh:140`, `bin/suite.sh:173`, `bin/tests/test_suite_sh.sh:42`. Aplicação limpa: **[nao verificado]**.

4. **Arquivos em comum.** Nenhum da raia original cruza as quatro obras em voo. O complemento DIFF-POR-IMPACTO não fornece lista de arquivos de implementação suficiente para outro cruzamento: **[nao verificado]** além dos anexos.

5. **Depende de.** PRONTO 5, certificação e pouso próprio L-105; o aval original coloca o instrumento depois do CALENDÁRIO. O BACKLOG registra CALENDARIO-HOJE-MINIMO fechado, sem que isso prove ancestralidade Git nesta cópia. Fontes: `app/docs/BACKLOG.md:1036`, `app/docs/LEIS.md:131`, `app/docs/PROMPTS.md:1682`, `app/docs/BACKLOG.md:819`.

   **Separação necessária:** o complemento de 13/11 entra após E3; GATE-PELA-CLASSE é descrito como lei nova que ainda exige corte. Essa pendência não equivale a falta de autorização do núcleo antigo. Fonte: `app/docs/PROMPTS.md:2243`.

6. **PROPOSTA — INFERIU: CONCLUIR**, primeiro no comboio com OTIMIZA-1, começando pela recuperação de PRONTO 5 e adaptação às guardas atuais. O complemento segue na posição pós-E3 registrada, com o corte de lei pendente. Fontes: `app/docs/BACKLOG.md:1036`, `app/docs/RELATO.md:1637`, `app/docs/PROMPTS.md:2243`.

## JUSTIFICATIVA-PAGA-O-PERIODO

1. **O que cura — LEU.** A justificativa aprovada ainda limpa todos os marcos faltantes do dia, sem ler seu tipo; o efeito financeiro recebe a data inteira como justificada e não calcula o período da justificativa. Fontes: `app/escala/utils.py:1194`, `app/escala/utils.py:1254`, `app/ponto/services/efeito_ausencia.py:40`, `app/ponto/services/fechamento.py:660`.

2. **Existe no HEAD? SIM — LEU, com correção da premissa do BACKLOG.** O código não corresponde à frase “já cobre só o marco”: a grade consulta apenas datas e limpa todos os `missing`. O acumulador parcial de `efeito_ausencia` soma Ausência, enquanto o motor distingue trabalho/abono pelos conjuntos de datas. Fontes: `app/escala/utils.py:1199`, `app/escala/utils.py:1256`, `app/ponto/services/efeito_ausencia.py:46`, `app/ponto/motor_calculo_v2.py:2600`, `app/ponto/motor_calculo_v2.py:2606`.

   O caso HJ-093 e os totais de triagem são registros fornecidos; a própria linha admite outras causas possíveis. Não são contagem comprovada de ocorrências deste defeito. Fonte: `app/docs/BACKLOG.md:1872`; confirmação nominal atual: **[nao verificado]**.

3. **Falta / reaplicação.** Falta ligar cobertura do marco, período pago e consumidores financeiros, produzir DIFF novo e demonstrar as condições do aval. Fontes: `app/docs/BACKLOG.md:1872`, `app/docs/CORTES.md:336`, `app/docs/CORTES.md:353`.

   O anexo marca `main_moveu=True` em `app/core/juizes.py`, `app/escala/utils.py`, `app/escala/tests/test_grade_segue_a_precedencia.py`, `app/ponto/services/{cartorio,efeito_ausencia,fechamento}.py` e `app/ponto/turnos.py` — **[nao verificado]** por Git.

   **INFERIU:** o porte precisa adaptar três mudanças do HEAD:

   - A grade agora recebe deslocamentos da cadeia do turno, em vez de reconstruí-los pelas colunas antigas. Fonte: `app/escala/utils.py:1156`, `app/escala/utils.py:1178`.
   - O cartório já propaga `dispensada` por marco, mecanismo a preservar na integração. Fonte: `app/ponto/services/cartorio.py:592`.
   - O fechamento eliminou o acumulador mensal antigo e usa `creditos_por_dia`, cujo universo atual continua sendo Ausência. Somar apenas em `efeito_ausencia.min_parcial_pagos` não demonstra consumo pelo fechamento. Fontes: `app/ponto/services/fechamento.py:672`, `app/ponto/services/fechamento.py:994`, `app/ponto/services/credito_parcial.py:15`, `app/ponto/services/credito_parcial.py:26`.

   Aplicação limpa e conflitos textuais: **[nao verificado]**.

4. **Arquivos em comum.** `app/core/juizes.py` cruza **BUG-PAR/O168/E3**; nenhum outro arquivo da raia original cruza a lista fornecida. Confirmação fora dos anexos: **[nao verificado]**.

5. **Depende de.** O desenho da cobertura e do período está registrado; o `!` do motor **já foi concedido**, com escopo específico e condições, portanto não deve ser apresentado como autorização ainda inexistente. Fontes: `app/docs/CORTES.md:319`, `app/docs/CORTES.md:336`, `app/docs/CORTES.md:353`.

   Permanecem as condições: DIFF prévio das competências indicadas, nenhuma semana adicional de DSR perdida fora das exceções nomeadas e hash do TXT igual nas exportadas. CADASTRO/BATIDA seguem os destinos definidos no corte; atendimento dessas condições hoje: **[nao verificado]**. Fonte: `app/docs/CORTES.md:353`.

6. **PROPOSTA — INFERIU: CONCLUIR**, imediatamente após HJ-090 na fila de negócio, como já registrado, coordenando `core/juizes.py` com BUG-PAR/O168/E3. Concluir a obra completa evita pousar isoladamente o estreitamento da cobertura, expressamente condicionado a existir quem pague o período. Fontes: `app/docs/BACKLOG.md:1872`, `app/docs/CORTES.md:330`.

| id | cura | existe no HEAD | falta | depende | proposta |
|---|---|---|---|---|---|
| COBPARC-FILA-RELOGIO | Instante único por ato (`app/ponto/services/cartorio.py:373`) | **SIM** (`app/chamados/reconciliador.py:884`) | Porte aos eventos, mutantes/suíte/opus (`app/docs/BACKLOG.md:860`) | Padrão e estacionamento; sobreposições conforme anexos **[nao verificado]** | **CONCLUIR**, após estabilizar sobreposições — INFERIU |
| V3-RETOMA-PERIODOS-DA-CELULA | Período/pausa/CCT diária (`app/ponto/calculador/alimentacao.py:158`) | **PARCIAL**; P5/dobra presentes (`app/ponto/motor_calculo_v2.py:162`; `app/ponto/calculador/regras.py:793`) | Períodos, intra, split e P4 (`app/docs/BACKLOG.md:964`) | Impacto integral e `!` da troca/exportadas (`app/docs/BACKLOG.md:1797`) | **CONCLUIR**, restante da retomada V3 — INFERIU |
| V3-RETOMA-FILA | DIFF lê a lavra (`app/ponto/management/commands/diff_calculador.py:137`) | **SIM** (`app/ponto/management/commands/diff_calculador.py:247`) | Instrumento completo e re-DIFF (`app/docs/BACKLOG.md:976`) | L-105; conciliar POUSO-DA-V3 (`app/docs/BACKLOG.md:976`) | **CONCLUIR**, a

[truncado em 20000 de 20942 caracteres]

## 13/11 18:24 · sessao 026a019ad69b · RAIA-PREPARACAO · censo · origem=Code

**LEU** indica sítio aberto na cópia; **INFERIU** indica conclusão ou proposta baseada nesses sítios. Aplicação das refs, conflitos textuais de merge, testes executados agora e situação atual de produção: **[nao verificado]**. [nao verificado]

## SILENCIO-VENCIDO-VOLTA

1. **O que cura — LEU.** O contador aceita silêncio vencido, mas a partição o transforma em HISTORICO e a lista o esconde (`app/chamados/services/painel.py:190`; `app/chamados/services/verbos.py:51`; `app/chamados/services/painel.py:328`).  
   A lista do colaborador também exige prazo nulo, excluindo quem teve silêncio já vencido (`app/chamados/services/janela_colab.py:73`).

2. **Existe no HEAD? SIM — LEU/INFERIU.** A contradição permanece nesses caminhos. Parte da infraestrutura já existe: `qs_na_fila` recebe `agora`, e a instância tem `silenciado_em`; isso não corrige HISTORICO nem a seleção do colaborador (`app/chamados/services/painel.py:158`; `app/chamados/models.py:508`; `app/chamados/services/verbos.py:51`). Commit de introdução dessas partes: **[nao verificado]**.

3. **Falta/aplicabilidade.** A SPEC atual é o transplante b4 da E3, mais `verbos.py` e a lista do colaborador, com pouso junto da QUARENTENA (`app/docs/BACKLOG.md:782`). O registro posterior informa que b4/b6a foram retiradas da raia E3; portanto sua existência naquela raia não demonstra cura entregue (`app/docs/RELATO.md:243`). Os arquivos com `main_moveu=True` e os conflitos semânticos estão no quadro compartilhado abaixo; aplicação integral da raia: **[nao verificado]**.

4. **Arquivos comuns.** São os do quadro compartilhado SILENCIO/QUARENTENA abaixo. O anexo contém uma raia cumulativa; atribuir todos esses arquivos exclusivamente à fatia SILENCIO: **[nao verificado]**.

5. **Depende de.** QUARENTENA e integração da SPEC retirada da E3 (`app/docs/BACKLOG.md:782`). A regra de silêncio com prazo está na L-062 (`app/docs/LEIS.md:96`). Necessidade direta de alterar motor, gravado exportado ou dados de escala/vínculo nesta fatia: **[nao verificado]**.

6. **Proposta — INFERIU: CONCLUIR junto da QUARENTENA**, após o pouso E3, conforme a condição mais recente de reabertura. O defeito continua legível; “pronta” no cadastro não basta para encerrá-lo (`app/docs/BACKLOG.md:2469`; `app/chamados/services/verbos.py:51`; `app/chamados/services/janela_colab.py:73`).

## QUARENTENA-DO-COLAB

1. **O que cura — LEU.** O contrato troca o silêncio escrito em cada chamado por isolamento do colaborador por período, com juiz e porta únicos (`app/docs/BACKLOG.md:783`).  
   O sítio atual ainda aplica silêncio chamado a chamado, acionado também pelos sinais (`app/chamados/services/silencio_isentos.py:34`; `app/chamados/signals.py:270`).

2. **Existe no HEAD? SIM — LEU/INFERIU.** A fila considera status e prazo; a emissão datada segue para `PerguntaDisputa.abrir`; o comando de isentos ainda escreve silêncio (`app/chamados/services/painel.py:188`; `app/chamados/services/disputa_emissao.py:443`; `app/chamados/management/commands/silenciar_chamados_isentos.py:49`). Uma cura equivalente em outro caminho não foi demonstrada: **[nao verificado]**.

3. **Falta/aplicabilidade.** O HEAD manda reaplicar a versão antiga como SPEC numa cópia nova e produzir DIFF de frota (`app/docs/BACKLOG.md:783`; `app/docs/BACKLOG.md:2469`). O relato detalha oito achados: fechar a janela ao desligar isenção; vigia chegar a zero; eliminar segundo escritor; migration reversível sem importar serviço vivo; LEI-AKITA; RED da retenção; DIFF pela consulta real; início pela ligação correta (`app/docs/RELATO.md:251`). Esse mesmo registro não estabelece suite aprovada; a retomada posterior parou com apenas um RED não rodado (`app/docs/RELATO.md:251`; `app/docs/RELATO.md:190`). Aplicação limpa da raia antiga: **[nao verificado]**; divergências abaixo.

4. **Arquivos comuns.** Quadro compartilhado abaixo, calculado dos anexos; comparação com as refs atuais: **[nao verificado]**.

5. **Depende de.** Pouso E3 e decisão sobre início da isenção: o desenho registra última ligação versus uma janela por período da trilha, com proposta de 16 janelas em vez de 15 (`app/docs/BACKLOG.md:2469`; `app/docs/RELATO.md:191`). O contrato exige espelho, folha e TXT intactos; comprovação atual dessa invariância: **[nao verificado]** (`app/docs/BACKLOG.md:783`). Qualquer execução que alcance motor, gravado exportado ou dados de escala/vínculo entra no âmbito de `!` (`CLAUDE.md:265`).

6. **Proposta — INFERIU: CONCLUIR com SILENCIO**, depois da E3 e coordenada com SAIDA-NOTURNA. Usar a raia antiga como especificação e resolver a reprovação antes de certificar; essa é a retomada registrada, já interrompida novamente (`app/docs/BACKLOG.md:2469`; `app/docs/RELATO.md:190`; `app/docs/RELATO.md:245`).

**Raia compartilhada SILENCIO/QUARENTENA — ponto 3 das duas obras.**

O anexo marca `main_moveu=True` nestes 24 arquivos; conflitos textuais e aplicação efetiva: **[nao verificado]**.

- `app/chamados/management/commands/auditar_invariantes_chamados.py`;
  `app/chamados/{models,signals,views_arquivar}.py`. **[nao verificado]**
- `app/chamados/services/{aviso_pendencia,cobrar_dia,disputa_emissao,materializacao,painel,pergunta_no_app,silencio_ausencia,verbos,vigia_fio}.py`. **[nao verificado]**
- `app/chamados/tests/test_o261_vigia_fio.py`. **[nao verificado]**
- `app/colaboradores/services/{colaborador,ficha}.py`;
  `app/colaboradores/views.py`. **[nao verificado]**
- `app/core/{juizes,portas}.py`;
  `app/core/tests/{test_selo_performance,test_tripwire_contagem_de_pendencia}.py`. **[nao verificado]**
- `app/docs/{ARQUITETURA.mmd,MAPA.md}`;
  `app/ponto/tests/test_selo_chokepoint_escrita.py`. **[nao verificado]**

**Conflitos semânticos — INFERIU:** reaplicar corpos antigos pode desfazer a delegação do aviso a `consulta_do_app`, a trilha anterior ao cartório e a distinção entre conversa e fato datado. São pontos concretos para reconciliar, além do relógio `agora` já incorporado (`app/chamados/services/aviso_pendencia.py:7`; `app/chamados/services/cobrar_dia.py:31`; `app/chamados/services/disputa_emissao.py:424`; `app/chamados/services/painel.py:158`).

**Interseção com as obras em voo — ponto 4 das duas obras, segundo os anexos:**

| Em voo | Arquivos comuns à raia compartilhada |
|---|---|
| BUG-PAR e E3 | `app/chamados/models.py`; `app/chamados/services/{disputa_emissao,materializacao,vigia_fio}.py`; `app/chamados/tests/test_o261_vigia_fio.py`; `app/core/juizes.py`; `app/core/tests/test_tripwire_contagem_de_pendencia.py`. **[nao verificado]** |
| O168 | `app/chamados/models.py`; `app/core/juizes.py`; `app/ponto/tests/test_selo_chokepoint_escrita.py`. **[nao verificado]** |
| HJ-090 | `app/chamados/tests/test_o261_vigia_fio.py`. **[nao verificado]** |

## V3-E1

1. **O que cura — LEU.** Fecha a certificação da v3 por casos validados e propriedades fixas; o sítio de ensaio é `reler`, explicitamente destinado à bateria e ao DIFF (`app/docs/BACKLOG.md:821`; `app/ponto/calculador/periodo.py:543`).  
   O registro específico da interrupção informa VALOR verde nos casos validados e falhas de STATUS, esclarecendo o “motivo: não sei” do cadastro (`app/docs/RELATO.md:2055`; `app/docs/BACKLOG.md:2081`).

2. **Existe no HEAD? PARCIAL — LEU/INFERIU.** A bateria E1 continua descrita pelos testes atuais como pertencente a outra raia (`app/ponto/tests/test_e3_status_lavrado.py:7`). Parte dos defeitos históricos já tem resposta no HEAD: previsto sem batida com veredito acusador resulta em `irregular`; isento resulta em `ok`, com testes correspondentes (`app/ponto/status_dia.py:150`; `app/ponto/status_dia.py:154`; `app/ponto/tests/test_e3_status_lavrado.py:55`; `app/ponto/tests/test_e3_status_lavrado.py:62`). Cura dos demais casos históricos e commit responsável por cada um: **[nao verificado]**.

3. **Falta/aplicabilidade.** Reconstituir a bateria sobre o HEAD, reavaliar STATUS e preservar as pendências do DP. O relato da validação inclui C05/C06/C16/C17 pendentes; a decisão C17 — HE 0/intra 60 — já está registrada (`app/docs/RELATO.md:2055`; `app/docs/RELATO.md:2064`). Conferir também o gate CCT e o impacto por rubrica, lacunas registradas (`app/docs/RELATO.md:2065`; `app/docs/BACKLOG.md:821`). No anexo, nenhum dos cinco arquivos tem `main_moveu=True`; compatibilidade atual dos testes e aplicação limpa: **[nao verificado]**. A API `reler` permanece no HEAD (`app/ponto/calculador/periodo.py:543`).

4. **Arquivos comuns.** Interseção literal com BUG-PAR/O168/HJ-090/E3 vazia segundo os anexos; confirmação das raias: **[nao verificado]**.

5. **Depende de.** DP/contador para os casos pendentes e certificação pela regra (`app/docs/RELATO.md:2055`; `app/docs/BACKLOG.md:821`). Alterar v2 ou dados protegidos exige `!`; a troca para v3 também está explicitamente condicionada a `!` (`CLAUDE.md:265`; `app/docs/BACKLOG.md:963`).

6. **Proposta — INFERIU: CONCLUIR na pista de certificação, antes da troca v3.** A cura posterior de alguns STATUS reduz o trabalho residual, mas não substitui a bateria nem resolve as pendências do DP (`app/ponto/tests/test_e3_status_lavrado.py:7`; `app/docs/RELATO.md:2055`; `app/docs/BACKLOG.md:821`).

## CODEX-CONTEXTO-DA-CASA

1. **O que cura — LEU.** O contrato pede contexto gerado com hashes e brief de verificador restrito a caminhos, hash e pergunta (`app/docs/BACKLOG.md:1038`).  
   O sítio atual monta o cabeçalho e transcreve integralmente o arquivo de pergunta (`bin/codex_pergunta.sh:335`; `bin/codex_pergunta.sh:355`).

2. **Existe no HEAD? SIM — LEU/INFERIU.** A transcrição crua permanece, e a guarda do verificador nesse caminho consulta B3 (`bin/codex_pergunta.sh:355`; `bin/codex_pergunta.sh:390`). A publicação de GLOSSARIO já existe, mas não demonstra a implementação do contrato de contexto e neutralidade do brief (`bin/relato.sh:111`; `app/docs/BACKLOG.md:1038`). Commit de cura completa: **[nao verificado]**.

3. **Falta/aplicabilidade.** Reconciliar briefs com a exigência `PERGUNTA:` e resolver teto/fontes: o relato documenta aproximadamente 42,7 KB para teto de 30.000 B, além da classificação das leis das seções 6b/7b (`app/docs/BACKLOG.md:1038`; `app/docs/RELATO.md:1943`). O bloqueio histórico por F1/V-FASE não comprova que esses construtores continuem em voo (`app/docs/BACKLOG.md:1038`); estado atual deles: **[nao verificado]**. O anexo marca `main_moveu=True` em `bin/{codex_limite_estado.py,codex_pergunta.sh,relato.sh}`; conflitos textuais: **[nao verificado]**. **INFERIU:** preservar o papel construtor, inventário, relógio por papel e publicação atuais ao incorporar a cura (`bin/codex_pergunta.sh:85`; `bin/codex_pergunta.sh:195`; `bin/relato.sh:111`).

4. **Arquivos comuns.** Interseção literal com as quatro obras em voo vazia segundo os anexos; confirmação das raias: **[nao verificado]**.

5. **Depende de.** Decisão sobre teto/fontes e pouso próprio de instrumento (`app/docs/RELATO.md:1943`; `app/docs/LEIS.md:131`). SEM-CONDUTA é complemento sucessor: o registro manda construí-lo depois de ENXUTO e DA-CASA (`app/docs/RELATO.md:1993`). Necessidade direta de `!` de motor/exportada/escala: **[nao verificado]**.

6. **Proposta — INFERIU: CONCLUIR em vaga de instrumento após o produto**, reconciliando o contrato sucessor SEM-CONDUTA. A lacuna do brief permanece; o desenho de fontes/teto precisa de resolução antes da certificação (`bin/codex_pergunta.sh:355`; `app/docs/RELATO.md:1943`; `app/docs/LEIS.md:131`).

## PADRAO-DE-CIMA

1. **O que cura — LEU.** O contrato pede reconhecer a segunda pergunta de desenho da mesma entidade/programa em 24 horas e bloquear construtor/verificador até a decisão consumida uma vez (`app/docs/BACKLOG.md:836`).  
   O mecanismo atual acumula e decide reprovações por obra (`bin/disjuntor.py:997`; `bin/disjuntor.py:1004`).

2. **Existe no HEAD? SIM — LEU/INFERIU.** O PreToolUse aplica B3 por obra e o censo do construtor, sem realizar ali o agrupamento contratado por entidade/24 h (`bin/disjuntor.py:1826`; `bin/disjuntor.py:1842`; `bin/disjuntor.py:1850`). Também permanece o problema de prova discutido no desenho: `_mutante` aceita qualquer retorno não zero, sem identificar a causa da morte (`bin/tests/test_disjuntor.sh:1522`; `app/docs/RELATO.md:1845`). Commit de cura completa: **[nao verificado]**.

3. **Falta/aplicabilidade.** Decidir como provar a causa da morte do mutante e certificar a versão escolhida. O relato distingue a versão estacionada da volta 10, que ficou sem verificador e sem opus final (`app/docs/RELATO.md:1845`). O anexo marca `main_moveu=True` em `bin/{codex_pergunta.sh,disjuntor.py,hook_stop_fila1.py}` e `bin/tests/{test_disjuntor.sh,test_hook_esperando_com_prova.sh,test_lei_akita.sh}`; aplicação limpa: **[nao verificado]**. **INFERIU:** reconciliar com CENSO-ANTES-DA-VAGA, papel construtor e recorte atual do topo (`bin/disjuntor.py:1850`; `bin/codex_pergunta.sh:195`; `bin/hook_stop_fila1.py:467`).

4. **Arquivos comuns.** Interseção com BUG-PAR/O168/HJ-090/E3 vazia segundo os anexos. Entre estas interrompidas, compartilha `bin/codex_pergunta.sh` com CODEX-CONTEXTO-DA-CASA. Confirmação das refs: **[nao verificado]**.

5. **Depende de.** PDC-DESENHO, contrato de instrumento e pouso separado de produto (`app/docs/RELATO.md:1845`; `CLAUDE.md:394`; `app/docs/LEIS.md:131`). B3 já possui implementação no HEAD (`bin/disjuntor.py:1004`). Necessidade direta de `!` de motor/exportada/escala: **[nao verificado]**.

6. **Proposta — INFERIU: CONCLUIR após a decisão de desenho, mantendo posição antes de B1-ESPERA-OCIOSA.** O contrato ainda tem lacuna no código, e a posição relativa está registrada (`bin/disjuntor.py:1826`; `app/docs/BACKLOG.md:837`). A reprovação repetida exige resolver o desenho; não demonstra cura ou superação da necessidade (`app/docs/RELATO.md:1845`).

## BUG-ENC-CELULAS-FORMA

1. **O que cura — LEU.** `dias_encerrados` repassa células com chave `(colaborador_id,data)` ao pareador, cujo contrato recebe chave `data`; a conversão posterior produz chave incompatível para consultar a geradora (`app/ponto/turnos.py:2132`; `app/ponto/turnos.py:2150`; `app/ponto/turnos.py:1854`; `app/ponto/turnos.py:1956`).  
   O cartório fornece exatamente esse mapa composto, sem `turnos`, e apenas para seu intervalo de julgamento (`app/ponto/services/cartorio.py:1131`; `app/ponto/services/cartorio.py:1154`).

2. **Existe no HEAD? SIM — LEU/INFERIU.** O repasse defeituoso permanece em `app/ponto/turnos.py:2150`. O pareador só carrega sua janela com pad quando recebe `celulas=None`; o mapa fornecido impede essa carga (`app/ponto/turnos.py:1884`). A via singular do vigia também passa chave composta, que chega diretamente ao pareador (`app/ponto/services/vigia_de_hora.py:58`; `app/ponto/turnos.py:2098`). O lote já converte corretamente para chave `data`, mas isso não cura a casca (`app/ponto/turnos.py:3038`). Commit dessa implementação do lote: **[nao verificado]**.

3. **Falta/aplicabilidade.** Completar a via singular; selos de booleano e pad; validação; suite; verificação final; impacto. São pendências explícitas do registro da interrupção (`app/docs/RELATO.md:7799`). E4-CARGA-PARIDADE deve preceder a cura, com fixture mista e mutante sem `mais=` (`app/docs/BACKLOG.md:932`). No anexo, somente `app/ponto/turnos.py` tem `main_moveu=True`; aplicação integral: **[nao verificado]**. **INFERIU:** o hunk fornecido ainda tem alvo reconhecível em `:2150`, mas resolve apenas a casca; a singular continua exigindo tratamento próprio (`app/ponto/turnos.py:2150`; `app/ponto/turnos.py:2098`).

4. **Arquivos comuns.** Interseção literal dos três caminhos da raia com as quatro obras em voo vazia segundo os anexos; confirmação das refs: **[nao verificado]**.

5. **Depende de.** E4-CARGA-PARIDADE e prova do impacto da mudança de veredito (`app/docs/BACKLOG.md:931`; `app/docs/BACKLOG.md:932`). O lote E4-7 já está implementado no HEAD (`app/ponto/turnos.py:2959`). O patch descrito toca `turnos.py`; eventual alteração de v2, gravado exportado ou dados de escala/vínculo exige `!` (`CLAUDE.md:265`). Alcance atual sobre produção: **[nao verificado]**.

6. **Proposta — INFERIU: CONCLUIR na próxima vaga de bug liberada, com E4-CARGA-PARIDADE primeiro no mesmo pacote.** Há defeito estático atual em chamadores concretos, e o cadastro lhe atribui fila 1; a espera histórica por F2 era ordem de vaga, sem dependência de código provada naquele registro (`app/ponto/turnos.py:2150`; `app/ponto/services/vigia_de_hora.py:58`; `app/docs/BACKLOG.md:931`; `app/docs/RELATO.md:1824`).

| id | cura | existe no HEAD | falta | depende | proposta |
|---|---|---|---|---|---|
| SILENCIO-VENCIDO-VOLTA | Retorno coerente do vencido (`app/chamados/services/verbos.py:51`) | **SIM** (`app/chamados/services/janela_colab.py:73`) | SPEC + verbos/lista (`app/docs/BACKLOG.md:782`) | QUARENTENA/E3 (`app/docs/BACKLOG.md:2469`) | **CONCLUIR junto, após E3** — INFERIU (`app/docs/BACKLOG.md:2469`) |
| QUARENTENA-DO-COLAB | Isolamento por período (`app/docs/BACKLOG.md:783`) | **SIM** (`app/chamados/services/silencio_isentos.py:43`) | Oito achados, validação e DIFF (`app/docs/RELATO.md:251`) | E3; desenho 16×15 (`app/docs/BACKLOG.md:2469`; `app/docs/RELATO.md:191`) | **CONCLUIR com SILENCIO** — INFERIU (`app/docs/BACKLOG.md:2469`) |
| V3-E1 | Certificação v3 (`app/docs/BACKLOG.md:821`) | **PARCIAL** (`app/ponto/tests/test_e3_status_lavrado.py:7`; `app/ponto/status_dia.py:150`) | Revalidar bateria/DP/CCT (`app/docs/RELATO.md:2055`; `app/docs/RELATO.md:2065`) | DP; troca com `!` (`app/docs/BACKLOG.md:963`) | **CONCLUIR antes da troca** — INFERIU (`app/docs/BACKLOG.md:821`) |
| CODEX-CONTEXTO-DA-CASA | Contexto verificável e brief neutro (`app/docs/BACKLOG.md:1038`) | **SIM** (`bin/codex_pergunta.sh:355`) | Wrapper, fontes/teto (`app/docs/RELATO.md:1943`) | Decisão; SEM-CONDUTA sucessor (`app/docs/RELATO.md:1993`) | **CONCLUIR em vaga de instrumento** — INFERIU (`app/docs/LEIS.md:131`) |
| PADRAO-DE-CIMA | Desenho por entidade/24 h (`app/docs/BACKLOG.md:836`) | **SIM** (`bin/disjuntor.py:1004`) | Causa do mutante e certificação (`app/docs/RELATO.md:1845`) | PDC-DESENHO (`app/docs/BACKLOG.md:836`) | **CONCLUIR após decisão, antes de B1** — INFERIU (`app/docs/BACKLOG.md:837`) |
| BUG-ENC-CELULAS-FORMA | Contratos de células e pad (`app/ponto/turnos.py:2150`) | **SIM** (`app/ponto/services/vigia_de_hora.py:58`) | Singular, selos, suite/impacto (`app/docs/RELATO.md:7799`) | E4-CARGA-PARIDADE (`app/docs/BACKLOG.md:932`) | **CONCLUIR na vaga de bug** — INFERIU (`app/docs/BACKLOG.md:931`) |

## 13/11 18:24 · sessao 27b58837747d · RAIA-PREPARACAO · censo · origem=Code

**LEU** identifica código/documento aberto na cópia; **INFERIU** identifica conclusão da comparação. SIM/PARCIAL abaixo qualificam o código: ocorrência atual em produção, passivo e impacto financeiro são **[não verificado]**. Histórico Git, ancestralidade e aplicação mecânica das raias também são **[não verificado]**; os commits citados têm atribuição documental. [nao verificado]

**O261-PAI-TERMINAL**

1. **O que cura — LEU.** Disputa aberta pode conservar pai terminal: o emissor retorna a disputa existente sem verificar o estado do pai. (`app/chamados/services/disputa_emissao.py:531`, `app/chamados/services/disputa_emissao.py:566`)  
   Pergunta sem destino continua ancorada nesse pai; a fila cobra apenas chamados vivos. (`app/chamados/services/fio.py:117`, `app/chamados/services/painel.py:155`, `app/chamados/services/painel.py:189`)

2. **Ainda existe? PARCIAL — LEU.** O retorno sem conferir o pai permanece; `reabrir_sistema` reabre a disputa sem religar sua âncora, e o nascimento admite `chamado_destino=None`. (`app/chamados/services/disputa_emissao.py:566`, `app/chamados/models.py:2164`, `app/chamados/models.py:3015`) Entretanto, a O261-E já introduziu guarda no encerramento: considera perguntas da competência aberta, reata por pergunta ou recusa o fechamento. Esse sítio **proíbe expressamente `religar_pai_vivo` por não separar competências**. (`app/chamados/models.py:364`, `app/chamados/models.py:374`, `app/chamados/models.py:410`) A absorção do objetivo pela O261-E consta do BACKLOG; isso não comprova cura completa do nascimento/passivo. (`app/docs/BACKLOG.md:892`)

3. **Falta / aplicação — LEU + INFERIU.** Falta resolver a âncora por pergunta/dia, preservando a separação entre aberta e exportada. A condição das disputas mistas está escrita no corte; o desenho atual já escolheu a reata por pergunta. (`app/docs/CORTES.md:397`, `app/chamados/models.py:374`)  
   `main_moveu=True`, segundo o anexo: `app/chamados/models.py`, `app/chamados/services/acoes_disputa.py`, `app/chamados/services/vigia_fio.py`, `app/chamados/management/commands/vigiar_fio_morto.py` e `app/chamados/tests/test_o261_vigia_fio.py`. **[não verificado: comparação histórica fornecida]**  
   **INFERIU:** há conflitos de desenho e API: a raia religa a disputa inteira, usa ordem Disputa→Colaborador e chama a antiga porta de pergunta na reabertura; o HEAD exige ordem Colaborador→Disputa→Chamado→Pergunta e usa `abrir_conversa` nesse ato. (`app/chamados/models.py:380`, `app/chamados/models.py:2834`, `app/chamados/services/acoes_disputa.py:263`; raia: anexo, **[não verificado na cópia]**)

4. **Arquivos em comum.** BUG-PAR/E3: os quatro arquivos de produção acima e `test_o261_vigia_fio.py`; O168: `models.py`; HJ-090: o comando `vigiar_fio_morto.py` e `test_o261_vigia_fio.py`. **[não verificado na cópia; cruzamento dos anexos]**

5. **Depende de — LEU.** Separação por competência, juízes da vida/âncora e conclusão pelo desenho O261-E/FIO; exportada deve seguir a condição específica do corte. (`app/docs/CORTES.md:397`, `app/chamados/models.py:324`, `app/chamados/models.py:377`) Tocar o gravado exportado exige `!`; religar indiscriminadamente a disputa não atende à condição escrita. (`CLAUDE.md:265`, `app/chamados/models.py:380`)

6. **Proposta — INFERIU: DEMOLIR a solução antiga por disputa.** O desenho foi superado pela reata por pergunta e está explicitamente vedado no HEAD. Encaminhar o residual de nascimento/reabertura à O261-E/FIO, junto das obras em voo que tocam esses arquivos, aproveitando os casos de teste como insumo. (`app/chamados/models.py:380`, `app/docs/BACKLOG.md:892`, `app/docs/PROMPTS.md:1685`) A proposta não declara o defeito encerrado. (`app/chamados/services/disputa_emissao.py:566`)

**O10-C**

1. **O que cura — LEU.** Entregar ao admin os lançamentos SOLO e permitir declarar o par pela tela, usando a porta existente. Esse é o desenho da volta 2 registrado. (`app/docs/RELATO.md:1338`, `app/docs/BACKLOG.md:2082`)  
   A operação de troca já existe, mas exige dois lados e duas cartas; SOLO é pedido, não troca confirmada. (`app/ponto/portas/celula.py:670`, `app/ponto/portas/celula.py:691`, `app/ponto/portas/celula.py:702`)

2. **Ainda existe? SIM, no objetivo lista+UI — LEU + INFERIU.** A base O10-B está presente: trava as cartas e edita os dois lados no mesmo atomic. (`app/ponto/portas/celula.py:695`, `app/ponto/portas/celula.py:711`) Não foi encontrado chamador de UI na busca da cópia; a ausência global fica **[não verificado]**, apoiada pela definição, rotas abertas e registro explícito da lacuna. (`app/ponto/portas/celula.py:670`, `app/ponto/urls.py:53`, `app/docs/RELATO.md:1338`) O BACKLOG atribui a base ao merge `c3ba27d7`; não atribui a ele a entrega C. (`app/docs/BACKLOG.md:2033`)

3. **Falta / aplicação — LEU + INFERIU.** Falta Pauta DP por dia com link para view/form que receba substituto, duas cartas e motivo; o caso de aceite é SOLO produzir zero edição do par e uma linha com porta alcançável. (`app/docs/RELATO.md:1338`, `app/docs/RELATO.md:1341`)  
   `main_moveu=True`: somente `app/config/crons.py`, segundo o anexo. **[não verificado: comparação histórica fornecida]**  
   **INFERIU:** o lote antigo pode fornecer classificação e casos, mas imprimir `so_um_lado` não entrega a lista persistida nem o formulário. A raia precisa incorporar a volta 2; a região de comandos avulsos também evoluiu. (`app/docs/RELATO.md:1338`, `app/config/crons.py:1370`; lote: anexo, **[não verificado na cópia]**)

4. **Arquivos em comum.** `app/config/crons.py` com BUG-PAR e HJ-090; nenhum dos arquivos fornecidos da raia com O168/E3. **[não verificado na cópia; cruzamento dos anexos]**

5. **Depende de — LEU.** O desenho SOLO já está decidido: o admin declara o par. A porta barra competência lavrada e dia sem vínculo. (`app/docs/RELATO.md:1338`, `app/ponto/portas/celula.py:606`, `app/ponto/portas/celula.py:622`) O apply em exportada tem condição própria de DRY e `!`; alteração de dado de escala/vínculo também exige `!`. (`app/docs/RELATO.md:1342`, `CLAUDE.md:265`) **INFERIU:** a UI pode chamar a porta existente sem alterar o motor. (`app/docs/RELATO.md:1338`, `app/ponto/portas/celula.py:670`)

6. **Proposta — INFERIU: CONCLUIR pela volta 2.** Sugerida para a primeira vaga de negócio depois das prioridades estruturais, entregando lista e ação alcançável. O lote sozinho não satisfaz o desenho registrado. (`CLAUDE.md:291`, `app/docs/RELATO.md:1338`, `app/docs/BACKLOG.md:2082`)

**O275**

1. **O que cura — LEU.** Permitir que cadastro do tipo faça um feriado criar dia previsto onde o padrão semanal não previa trabalho. (`app/docs/BACKLOG.md:1557`)  
   Hoje o semanal consulta os dias cadastrados, e o vínculo apenas pode suprimir trabalho por feriado; falta o caminho feriado `False→True`. (`app/escala/models.py:830`, `app/escala/models.py:1365`)

2. **Ainda existe? SIM — LEU.** `TipoEscala` não apresenta o novo campo entre `apelido` e os campos de ciclo; o cálculo mantém a base do template e a supressão de feriado. (`app/escala/models.py:114`, `app/escala/models.py:117`, `app/escala/models.py:1365`) Payload e formulário abertos também não oferecem esse cadastro. (`app/escala/views_wizard.py:34`, `app/templates/escala/wizard_tipo_escala.html:173`) Cura equivalente ou commit que a introduziu: **[não verificado]**.

3. **Falta / aplicação — LEU + INFERIU.** Falta decidir **FORMA A** — todos os ciclos menos intermitente — ou **FORMA B** — somente 6x1/5x2/semanal. O bloqueio é escolha de lei com códigos diferentes; “default desligado” não escolhe entre elas. (`app/docs/BACKLOG.md:1557`, `app/docs/RELATO-ARQUIVO.md:750`, `app/docs/RELATO-ARQUIVO.md:753`) Os 12 h × 36 h são relato histórico; reprodução atual **[não verificado]**. (`app/docs/RELATO-ARQUIVO.md:748`)  
   `main_moveu=True`: `app/config/crons.py`, `app/escala/models.py`, `app/escala/services/cadastro_tipo.py`, `app/escala/views_wizard.py` e `app/templates/escala/tipos_lista.html`. **[não verificado: comparação histórica fornecida]**  
   **INFERIU:** precisa conciliar o leitor/contador antigos com as autoridades atuais de lugar e cobertura do vínculo. (`app/escala/models.py:929`, `app/escala/models.py:1385`; implementação antiga: anexo, **[não verificado na cópia]**) Há também conflito de grafo: a raia acrescenta migração após `0042`, enquanto o HEAD já tem `0043_eventocelula` e uma `0044` dependente dela; será necessário reencadear a migração. (`app/escala/migrations/0043_eventocelula.py:8`, `app/escala/migrations/0044_celuladia_status_dia_celuladia_status_rotulo.py:9`; migração da raia: **[não verificado na cópia]**)

4. **Arquivos em comum.** `app/config/crons.py` com BUG-PAR e HJ-090; nenhum dos arquivos fornecidos com O168/E3. **[não verificado na cópia; cruzamento dos anexos]**

5. **Depende de — LEU.** Corte FORMA A/B, cadastro pela UI, derivador único e migration conciliada. O277 vem depois da O275. (`app/docs/BACKLOG.md:1557`, `app/docs/BACKLOG.md:1559`) Alterar dados de escala/vínculo exige `!`; eventual toque no motor tem aval próprio. (`CLAUDE.md:265`, `CLAUDE.md:142`)

6. **Proposta — INFERIU: CONCLUIR após o corte A/B**, em vaga de negócio e antes da O277. A capacidade pedida permanece ausente; a decisão de alcance impede tratar a raia como pronta para pouso. (`app/escala/models.py:1365`, `app/docs/RELATO-ARQUIVO.md:753`, `app/docs/BACKLOG.md:1559`)

**LASTRO-DEGRAU-2**

1. **O que cura — LEU.** Elimina o casamento próprio de batida com marco quando falta lâmpada, fazendo o recuo ler o juiz da atribuição. (`app/docs/BACKLOG.md:1756`, `app/core/juizes.py:212`)  
   O sítio ainda combina a hora cobrada com a data civil e busca batida do mesmo tipo pela distância linear. (`app/ponto/services/lastro.py:304`)

2. **Ainda existe? SIM — LEU.** `TOLERANCIA_MIN=90` permanece e o degrau 2 ainda usa `abs(timestamp-alvo) <= tol`. (`app/ponto/services/lastro.py:59`, `app/ponto/services/lastro.py:307`) Outra parte evoluiu: LASTRO-PARTE-2 levou o fecho ao evento e tornou o comando vigia; o BACKLOG atribui isso a `a7372499`, e o código confirma o comportamento. Isso não removeu a conta linear. (`app/docs/BACKLOG.md:753`, `app/ponto/management/commands/fechar_cobranca_com_lastro.py:44`, `app/ponto/services/lastro.py:364`)

3. **Falta / aplicação — LEU + INFERIU.** Falta migrar o recuo para o juiz e verificar/corrigir a invariância da grade entre janela unitária e lote. A proposta antiga de consultar somente o raio já foi explicitamente superada. (`app/docs/BACKLOG.md:1756`, `app/docs/BACKLOG.md:1066`) Parte da causa mudou: folga/ausência agora são aplicadas por pessoa e vínculo do dia; a régua ainda reúne escalas da janela. O caso histórico continua ocorrendo hoje: **[não verificado]**. (`app/escala/utils.py:1843`, `app/escala/utils.py:1313`, `app/escala/utils.py:1350`)  
   `main_moveu=True`: `app/ponto/management/commands/fechar_cobranca_com_lastro.py`, `app/ponto/services/cartorio.py`, `app/ponto/services/lastro.py`, `app/ponto/tests/test_bug114_fechador_de_dois_dias.py`, `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py`. **[não verificado: comparação histórica fornecida]**  
   **INFERIU:** exige recomposição: a raia usa o antigo `fechar()`, enquanto o HEAD preserva fecho por evento e o balde `retido_pelo_fechamento`; seu compositor também omite `agora`, hoje obrigatório no builder. (`app/ponto/services/lastro.py:317`, `app/ponto/services/lastro.py:364`, `app/escala/utils.py:2084`; chamadas antigas: anexo, **[não verificado na cópia]**)

4. **Arquivos em comum.** Nenhum dos seis arquivos fornecidos da raia aparece nas quatro listas de obras em voo. **[não verificado na cópia; cruzamento dos anexos]**

5. **Depende de — LEU.** Juiz único do marco, equivalência da grade e fechamento do grupo (1) de leitores. (`app/core/juizes.py:212`, `app/docs/BACKLOG.md:811`, `app/docs/BACKLOG.md:1756`) Eventual alteração de motor, dados escala/vínculo ou gravado exportado exige o `!` correspondente. (`CLAUDE.md:265`)

6. **Proposta — INFERIU: CONCLUIR o residual**, após verificar/corrigir o construtor e antes de declarar zerado o grupo (1). Preservar fecho por evento, guardas e contagem de retidos já presentes. (`app/docs/BACKLOG.md:811`, `app/ponto/services/lastro.py:317`, `app/ponto/services/lastro.py:364`)

**ATA-X-TURNO-PAUSA**

1. **O que cura — LEU + INFERIU.** A pausa reconhecida pelo juiz pode perder atribuição no cluster; a reconciliação posterior trata somente as bordas do turno. (`app/escala/utils.py:561`, `app/escala/utils.py:572`, `app/escala/utils.py:1975`)  
   Também falta publicar a acusação e os detalhes `l084_pausa` na CxR, como pede a obra. (`app/docs/BACKLOG.md:1725`, `app/ponto/services/cxr_na_lista.py:29`)

2. **Ainda existe? PARCIAL — LEU.** O juiz já alinha por papel, mas o fluxo posterior termina a reconciliação das bordas sem o bloco da pausa; acusador e campos específicos também faltam. (`app/escala/utils.py:502`, `app/escala/utils.py:2039`, `app/ponto/motor_calculo_v2.py:1673`, `app/ponto/motor_calculo_v2.py:2717`, `app/ponto/services/cxr_na_lista.py:30`)  
   A subobra JUIZ-DO-PAR-DUAS-ENTRADAS foi absorvida pela cura na fonte, documentalmente `b3bf529e`; o sítio atual confirma a atribuição dentro do juiz. Isso não absorveu a ATA-PAUSA inteira. (`app/docs/RELATO.md:212`, `app/docs/RELATO.md:2736`, `app/escala/utils.py:590`, `app/docs/RELATO.md:210`) Caso col841 e dinheiro atual: **[não verificado]**.

3. **Falta / aplicação — LEU + INFERIU.** Faltam 10/10 explicados e dinheiro igual na frota; são condições, não resultados comprovados aqui. O despacho manda refazer o bloco consultando a autoridade e atualizar a fixture. (`app/docs/CORTES.md:304`, `app/docs/RELATO.md:1082`)  
   `main_moveu=True`: `app/core/tests/test_medida_turno_do_par.py`, `app/escala/services/cadastro_realidade.py`, `app/escala/utils.py`, `app/ponto/turnos.py`. **[não verificado: comparação histórica fornecida]**  
   **INFERIU:** recompor conservação de órfãs, reconciliação e CxR sobre os sítios atuais; reavaliar o hunk antigo que passa papel pelo motor, considerando a cura na fonte. A catraca de linhas também evoluiu. (`app/escala/utils.py:1816`, `app/escala/utils.py:1975`, `app/escala/services/cadastro_realidade.py:360`, `app/core/tests/test_medida_turno_do_par.py:253`, `app/docs/RELATO.md:212`) `main_moveu=False` no motor, informado pelo anexo, não comprova compatibilidade do desenho. **[não verificado: comparação histórica fornecida]**

4. **Arquivos em comum.** `app/escala/services/cadastro_realidade.py` com BUG-PAR/E3; `app/core/tests/test_medida_turno_do_par.py` com BUG-PAR/HJ-090; nenhum com O168. **[não verificado na cópia; cruzamento dos anexos]**

5. **Depende de — LEU.** O `! ATA-PAUSA-NO-MOTOR` tem escopo condicionado a 10/10 e igualdade financeira. A pergunta de desenho consta respondida, com instrução de reconstrução. (`app/docs/CORTES.md:304`, `app/docs/RELATO.md:1082`, `app/docs/RELATO.md:1090`) Ocupação e pausas devem consultar as autoridades registradas. (`app/core/juizes.py:212`, `app/core/juizes.py:214`)

6. **Proposta — INFERIU: CONCLUIR**, em fila 1, depois das precedências C7/COBPARC registradas e da liberação dos arquivos comuns com E3/BUG-PAR. A conclusão destrava CXR-PRONTO1-COL841. (`app/docs/RELATO.md:1003`, `app/docs/BACKLOG.md:798`; cruzamento em voo: **[não verificado na cópia]**)

| id | cura | existe no HEAD | falta | depende | proposta |
|---|---|---|---|---|---|
| O261-PAI-TERMINAL | Âncora viva para pergunta (`app/chamados/services/fio.py:117`) | **PARCIAL** (`app/chamados/services/disputa_emissao.py:566`) | Residual por pergunta/dia (`app/chamados/models.py:374`) | O261-E + separação de competência (`app/docs/CORTES.md:397`) | **DEMOLIR raia antiga**, desenho vedado; residual na O261-E (`app/chamados/models.py:380`) |
| O10-C | SOLO na lista + troca pela UI (`app/docs/RELATO.md:1338`) | **SIM**; porta pronta (`app/ponto/portas/celula.py:670`) | Volta 2 UI/lista (`app/docs/BACKLOG.md:2082`) | Par declarado; vínculo/exportada (`app/ponto/portas/celula.py:606`, `app/ponto/portas/celula.py:691`) | **CONCLUIR**, vaga de negócio após estruturais — INFERIU (`CLAUDE.md:291`) |
| O275 | Feriado cria previsto por cadastro (`app/docs/BACKLOG.md:1557`) | **SIM** (`app/escala/models.py:1365`) | A/B e migration conciliada (`app/docs/BACKLOG.md:1557`, `app/escala/migrations/0043_eventocelula.py:8`) | Corte A/B (`app/docs/RELATO-ARQUIVO.md:750`) | **CONCLUIR** após corte, antes O277 — INFERIU (`app/docs/BACKLOG.md:1559`) |
| LASTRO-DEGRAU-2 | Recuo lê juiz do marco (`app/core/juizes.py:212`) | **SIM** (`app/ponto/services/lastro.py:304`) | Janela, evento e relógio (`app/docs/BACKLOG.md:1756`, `app/escala/utils.py:2084`) | Construtor + grupo (1) (`app/docs/BACKLOG.md:811`) | **CONCLUIR** após construtor — INFERIU (`app/docs/BACKLOG.md:1756`) |
| ATA-X-TURNO-PAUSA | Pausa na ata e CxR (`app/docs/BACKLOG.md:1725`) | **PARCIAL** (`app/escala/utils.py:2039`, `app/ponto/services/cxr_na_lista.py:30`) | Reconstrução, 10/10, dinheiro igual (`app/docs/RELATO.md:1082`, `app/docs/CORTES.md:304`) | `!` motor condicionado; arquivos em voo **[não verificado]** | **CONCLUIR**, após precedências registradas — INFERIU (`app/docs/RELATO.md:1003`) |

## 13/11 18:22 · sessao 497fecd73508 · - · - · origem=Ronald (tmux)

**O10-C**

1. **LEU — cura:** falta conduzir troca histórica SOLO à lista do admin e permitir que o admin declare o substituto e as duas cartas pela UI. A porta de negócio já existe e recusa par incompleto; o código lido não tem chamador de produção de `trocar_plantao` fora da própria definição/registro (`app/ponto/portas/celula.py:670`, `app/ponto/portas/celula.py:691`, `app/ponto/urls.py:53`; ausência global por busca em Python/HTML não-teste, sem um sítio positivo: [não verificado]).

2. **SIM — objetivo ainda ausente; base parcialmente pronta.** O10-B já implementou a operação com duas cartas, trava e duas edições no mesmo atomic (`app/ponto/portas/celula.py:695`, `app/ponto/portas/celula.py:702`, `app/ponto/portas/celula.py:711`). Falta o fluxo UI/lista descrito pelo pedido; o RELATO registra precisamente essa falta e seu desenho, mas é registro documental, não prova de execução (`app/docs/RELATO.md:1338`). O merge `c3ba27d7` é atribuição documental do BACKLOG; ancestralidade não foi verificada (`app/docs/BACKLOG.md:2033`).

3. **Falta:** volta 2 com Pauta DP por dia e link para view/form da troca, admin declarando substituto, duas cartas e motivo; RED “SOLO → zero edição do par + uma linha com porta alcançável” (`app/docs/RELATO.md:1338`, `app/docs/PROMPTS.md:1907`, `app/docs/BACKLOG.md:2082`). **INFERIU:** o lote da raia é semente aproveitável, mas imprimir `so_um_lado` não atende à entrega UI/lista; não pousar a raia como está (`app/docs/RELATO.md:1338`; conteúdo do lote só no anexo [não verificado na cópia]). `main_moveu=True` somente `app/config/crons.py` no anexo [não verificado na cópia]; a região one-shot continua em `app/config/crons.py:1370`. Conflito textual de merge [não verificado].

4. **Em comum com obras em voo:** `app/config/crons.py` com BUG-PAR e HJ-090, segundo `arquivos_em_voo.txt` [não verificado na cópia]. Novos arquivos UI/lista ainda não definidos na raia fornecida [não verificado na cópia].

5. **Depende:** desenho SOLO já respondido: substituto vem do admin e nunca é deduzido (`app/docs/PROMPTS.md:1907`). Porta requer vínculo e barra competência lavrada (`app/ponto/portas/celula.py:606`, `app/ponto/portas/celula.py:622`). `!` específico para comp. 09 após DRY por colaborador; dado de escala/vínculo e gravado exportado exigem `!` (`app/docs/RELATO.md:1342`, `CLAUDE.md:265`). Não precisa tocar motor para implementar a UI que chama a porta existente — **INFERIU** do desenho registrado e da porta (`app/docs/RELATO.md:1338`, `app/ponto/portas/celula.py:670`).

6. **PROPOSTA — CONCLUIR o objetivo O10-C**, pela volta 2 UI/lista, na primeira vaga de negócio após as obras estruturais prioritárias; a operação existente continua sem entrega alcançável ao admin (`app/docs/RELATO.md:1338`, `app/docs/PROMPTS.md:1898`, `CLAUDE.md:291`). Não demolir o objetivo; retirar a ideia de pousar somente o lote, pois não cumpre o corte SOLO (`app/docs/PROMPTS.md:1907`).

**O275**

1. **LEU — cura:** permitir declarar no TipoEscala que um feriado cria dia previsto, inclusive quando o padrão semanal não previa trabalho. Hoje o semanal decide apenas pelos dias cadastrados e o vínculo só transforma `True` em folga por feriado (`app/escala/models.py:830`, `app/escala/models.py:1365`). O corte exige cadastro novo desligado por padrão e intermitente intocado (`app/docs/PROMPTS.md:1229`).

2. **SIM.** O campo não existe entre `apelido` e os campos de ciclo (`app/escala/models.py:114`, `app/escala/models.py:117`); o método termina em base do template e supressão de feriado, sem caminho que faça `False` virar `True` por feriado (`app/escala/models.py:1365`, `app/escala/models.py:1369`). Payload e formulário tampouco oferecem o flag (`app/escala/views_wizard.py:34`, `app/templates/escala/wizard_tipo_escala.html:173`). Nenhuma cura equivalente encontrada; commit de cura [não verificado].

3. **Falta:** decidir a lei FORMA A/B antes do pouso (`app/docs/BACKLOG.md:1557`). A = todo ciclo menos intermitente; B = só 6x1/5x2/semanal, preservando fase dos plantões; essa diferença está expressamente documentada (`app/docs/RELATO-ARQUIVO.md:750`). O relato dos **12 h versus 36 h** é medição histórica documental, não revalidada nesta auditoria (`app/docs/RELATO-ARQUIVO.md:748`; realidade de produção [não verificado]).

   `main_moveu=True`: crons, models, cadastro_tipo, views_wizard, tipos_lista, segundo anexo [não verificado na cópia]. **LEU:** locais ainda recebem as inserções conceitualmente, mas houve evolução da propagação do cadastro (`app/escala/services/cadastro_tipo.py:323`, `app/escala/services/cadastro_tipo.py:328`), do lugar do feriado para o vínculo do dia (`app/escala/models.py:1385`) e do contador de vínculos para `q_vinculo_cobre_a_data` (`app/escala/models.py:929`). **INFERIU:** a raia antiga precisa conciliar essas autoridades; não transportar sem revisão o contador com `ativa=True` e o novo leitor com `lugar_do_colab` do anexo [não verificado na cópia].

   **Conflito concreto de grafo:** a raia traz migração `0043` dependente de `0042` [não verificado na cópia]; HEAD já tem `0043_eventocelula` dependente de `0042` e `0044` dependente daquela (`app/escala/migrations/0043_eventocelula.py:8`, `app/escala/migrations/0044_celuladia_status_dia_celuladia_status_rotulo.py:9`). **INFERIU:** precisa reencadear/conciliar a migration antes de pousar; não é só deslocamento de linhas.

4. **Em comum:** `app/config/crons.py` com BUG-PAR/HJ-090, pelo anexo de arquivos em voo [não verificado na cópia].

5. **Depende:** corte FORMA A/B ainda pedido, regra de um derivador, cadastro pela UI e migration/deploy (`app/docs/BACKLOG.md:1557`, `app/docs/RELATO-ARQUIVO.md:750`, `app/docs/RELATO-ARQUIVO.md:753`). O277 depende do pouso da O275, não é pré-requisito dela (`app/docs/PROMPTS.md:1239`). Aplicar alteração de dado escala/vínculo exige `!`; tocar motor na O277 também exige aval próprio (`CLAUDE.md:265`, `CLAUDE.md:142`). A O275 fornecida não altera o motor [não verificado na cópia; diff do anexo].

6. **PROPOSTA — CONCLUIR após resposta de lei**, em vaga de negócio e antes da O277; não liberá-la como pronta por “default desligado”, pois A e B são comportamentos diferentes (`app/docs/RELATO-ARQUIVO.md:753`, `app/docs/PROMPTS.md:1239`). Não há evidência de cura por outro caminho que justifique demolir (`app/escala/models.py:1365`).

| id | cura | existe no HEAD | falta | depende | proposta |
|---|---|---|---|---|---|
| O10-C | SOLO com lista e troca declarada pela UI (`RELATO.md:1338`) | SIM; porta pronta (`celula.py:670`) | volta 2 UI/lista (`BACKLOG.md:2082`) | admin declara par; exportada/vínculo `!` (`PROMPTS.md:1907`, `CLAUDE.md:265`) | CONCLUIR objetivo na vaga de negócio; não pousar só lote (`RELATO.md:1338`) |
| O275 | feriado cria previsto por cadastro (`PROMPTS.md:1229`) | SIM (`models.py:1365`) | A/B e conciliação da migration (`BACKLOG.md:1557`, `0044…py:9`) | lei A/B; escala/vínculo `!` (`RELATO-ARQUIVO.md:750`, `CLAUDE.md:265`) | CONCLUIR depois da lei, antes O277 (`PROMPTS.md:1239`) |

## 13/11 18:22 · sessao 13b256a6e4d6 · - · - · origem=Ronald (tmux)

**SILENCIO-VENCIDO-VOLTA**

1. **LEU — cura:** o juiz inclui o silêncio vencido na fila (`app/chamados/services/painel.py:188`), mas a partição o transforma em HISTORICO (`app/chamados/services/verbos.py:48`) e a lista exclui esse verbo (`app/chamados/services/painel.py:328`). A lista do colaborador exige ausência de qualquer prazo, inclusive vencido (`app/chamados/services/janela_colab.py:70`).

2. **LEU — SIM, defeito ainda existe:** os três caminhos acima permanecem contraditórios. Há cura parcial do relógio: `qs_na_fila` já aceita `agora` (`app/chamados/services/painel.py:158`) e a instância possui `silenciado_em` (`app/chamados/models.py:508`), mas isso não cura a partição nem a lista. A resposta ainda tenta levantar todo prazo não nulo, mesmo vencido (`app/chamados/services/materializacao.py:718`). Commit da introdução desses trechos [nao verificado].

3. **LEU — falta:** implementar a SPEC atual, composta pelo transplante b4 da E3 mais a remoção de HISTORICO para vivo e a correção da lista do colaborador; pousar junto da QUARENTENA (`app/docs/BACKLOG.md:782`). “Pronta” nessa mesma célula não é prova: o HEAD conserva o defeito nos sítios acima. A validade atual dos números históricos 12/34/13 [nao verificado]. A descrição de revert da b4/b6a é registro documental, não inspeção de commit (`app/docs/RELATO.md:243`).

4. **Arquivos comuns:** ver quadro compartilhado abaixo; a raia do anexo acumula as duas obras, portanto não demonstra que a fatia isolada de SILENCIO precise de todos aqueles arquivos [nao verificado].

5. **LEU — depende:** QUARENTENA e integração da SPEC retirada da E3 (`app/docs/BACKLOG.md:782`); L-062 é regra de silêncio com prazo (`app/docs/LEIS.md:96`). Nenhuma necessidade de tocar `motor_calculo_v2`, exportada ou escala/vínculo foi demonstrada para esta fatia [nao verificado].

6. **INFERIU — CONCLUIR:** como bloco com QUARENTENA, após o pouso E3; essa é a condição de reabertura atual, superior à ordem antiga do comentário do backlog (`app/docs/BACKLOG.md:2469`). O defeito é diretamente legível nos sítios citados, portanto não há fundamento para DEMOLIR (`app/chamados/services/verbos.py:48`; `app/chamados/services/janela_colab.py:70`).

**QUARENTENA-DO-COLAB**

1. **LEU — cura:** substituir o isolamento por silêncio escrito em cada chamado por isolamento do colaborador por período, com juiz e porta únicos; este é o contrato registrado (`app/docs/BACKLOG.md:783`). Hoje a porta de isentos ainda grava o véu chamado a chamado (`app/chamados/services/silencio_isentos.py:34`) e os sinais continuam acionando essa porta (`app/chamados/signals.py:270`).

2. **LEU — SIM, falta a cura:** a fila ainda decide somente por status e prazo (`app/chamados/services/painel.py:188`); a criação de pergunta datada segue direto para `PerguntaDisputa.abrir` (`app/chamados/services/disputa_emissao.py:443`), e o agrupador protege apenas ausência e duplicação (`app/chamados/services/disputa_emissao.py:830`). O vigia ainda dispõe de `--apply` e escreve silêncio (`app/chamados/management/commands/silenciar_chamados_isentos.py:49`). Não encontrei símbolos `quarentena` nos diretórios de produção pesquisados; a ausência global de outra cura equivalente [nao verificado].

3. **LEU — falta:** reaplicar a SPEC numa cópia nova do HEAD, resolver os achados da reprovação e obter DIFF de frota (`app/docs/BACKLOG.md:783`). O registro detalhado traz **oito** achados: desligar isenção levanta em vez de fechar; vigia não zera; segundo escritor; migration não reversível/importando serviço vivo; LEI-AKITA ausente; retenção reordenada sem RED; DIFF copiando consulta; início pela primeira ligação (`app/docs/RELATO.md:251`). A retomada também foi suspensa com apenas um RED não rodado (`app/docs/RELATO.md:190`; `app/docs/BACKLOG.md:2469`). A suite antiga não possui veredito no registro, portanto “suite feita” não está estabelecido (`app/docs/RELATO.md:251`).

4. **Arquivos comuns:** quadro abaixo, integralmente proveniente dos anexos, sem confirmação por comparação das refs [nao verificado].

5. **LEU — depende:** decisão de desenho sobre início da isenção, última ligação e proposta de 16 janelas em vez das 15 do aval (`app/docs/RELATO.md:191`); E3 deve pousar antes da reabertura (`app/docs/BACKLOG.md:2469`). O contrato exige que espelho, folha e TXT não mudem (`app/docs/BACKLOG.md:783`); cumprimento disso pela implementação proposta [nao verificado]. Nenhum `!` de motor/exportada/escala foi demonstrado por esta leitura [nao verificado].

6. **INFERIU — CONCLUIR:** retomar com SILENCIO após E3 e coordenar com SAIDA-NOTURNA, cujo registro de fila também depende de E3 (`app/docs/BACKLOG.md:1934`; `app/docs/BACKLOG.md:2469`). Tratar o diff antigo como SPEC, não como patch aprovado; a interrupção e a reprovação permanecem documentadas (`app/docs/RELATO.md:190`; `app/docs/RELATO.md:251`).

**Raia compartilhada: divergências e interseções**

`main_moveu=True` no anexo para os arquivos abaixo; sem `.git` e sem as versões-base/raia disponíveis para inspeção, conflito textual e aplicação limpa [nao verificado]:

- `app/chamados/management/commands/auditar_invariantes_chamados.py`;
- `app/chamados/models.py`, `signals.py`, `views_arquivar.py`;
- `app/chamados/services/{aviso_pendencia,cobrar_dia,disputa_emissao,materializacao,painel,pergunta_no_app,silencio_ausencia,verbos,vigia_fio}.py`;
- `app/chamados/tests/test_o261_vigia_fio.py`;
- `app/colaboradores/services/{colaborador,ficha}.py`, `app/colaboradores/views.py`;
- `app/core/{juizes,portas}.py`, `app/core/tests/{test_selo_performance,test_tripwire_contagem_de_pendencia}.py`;
- `app/docs/{ARQUITETURA.mmd,MAPA.md}`;
- `app/ponto/tests/test_selo_chokepoint_escrita.py`. [nao verificado]

**Conflitos semânticos LEU/INFERIU:** o patch antigo do aviso filtrava perguntas diretamente; o HEAD agora delega a `consulta_do_app`, cuja seleção separa pendentes e histórico pela E3 (`app/chamados/services/aviso_pendencia.py:7`; `app/chamados/services/pergunta_no_app.py:61`). Reaplicar o corpo antigo arriscaria desfazer essa delegação — INFERIU. A cobrança manual ganhou trilha antes do cartório, que deve ser preservada ao inserir devolução da quarentena (`app/chamados/services/cobrar_dia.py:31`). A emissão agora distingue conversa de fato datado, distinção que o filtro antigo deve respeitar (`app/chamados/services/disputa_emissao.py:424`). O relógio `agora` já está presente, portanto esse pedaço da raia não é mudança inédita (`app/chamados/services/painel.py:158`).

Interseção literal dos dois anexos com `arquivos_em_voo.txt`:

| Obra em voo | Arquivos comuns à raia compartilhada |
|---|---|
| BUG-PAR e E3 | `app/chamados/models.py`; `app/chamados/services/{disputa_emissao,materializacao,vigia_fio}.py`; `app/chamados/tests/test_o261_vigia_fio.py`; `app/core/juizes.py`; `app/core/tests/test_tripwire_contagem_de_pendencia.py`. [nao verificado] |
| O168 | `app/chamados/models.py`; `app/core/juizes.py`; `app/ponto/tests/test_selo_chokepoint_escrita.py`. [nao verificado] |
| HJ-090 | `app/chamados/tests/test_o261_vigia_fio.py`. [nao verificado] |

| id | cura | existe no HEAD | falta | depende | proposta |
|---|---|---|---|---|---|
| SILENCIO-VENCIDO-VOLTA | Vencido retorna coerentemente à fila (`verbos.py:48`, caminho completo acima) | SIM (`app/chamados/services/verbos.py:51`) | SPEC b4 + verbos/lista (`app/docs/BACKLOG.md:782`) | QUARENTENA/E3 (`app/docs/BACKLOG.md:2469`) | CONCLUIR junto, após E3 — INFERIU (`app/docs/BACKLOG.md:2469`) |
| QUARENTENA-DO-COLAB | Isolamento por período em vez de véu por chamado (`app/docs/BACKLOG.md:783`) | SIM (`app/chamados/services/silencio_isentos.py:43`) | Oito achados, RED/validação e DIFF (`app/docs/RELATO.md:251`) | E3 + desenho 16×15 (`app/docs/BACKLOG.md:2469`; `app/docs/RELATO.md:191`) | CONCLUIR com SILENCIO, após E3 — INFERIU (`app/docs/BACKLOG.md:2469`) |

## 13/11 18:21 · sessao 70257b8980f9 · - · - · origem=Ronald (tmux)

**CODEX-CONTEXTO-DA-CASA**

1. **O que cura — LEU:** falta de contexto da casa gerado e verificável por hash, e brief de verificador capaz de carregar achados/hipóteses; estes são os objetivos registrados em `app/docs/BACKLOG.md:1038`. No HEAD, o wrapper constrói o cabeçalho e transcreve integralmente o arquivo de pergunta, sem parser de `PERGUNTA:` nesse caminho (`bin/codex_pergunta.sh:335`, `bin/codex_pergunta.sh:355`).

2. **Ainda existe — SIM, LEU/INFERIU:** a transcrição crua está presente (`bin/codex_pergunta.sh:355`); a guarda anterior à chamada do verificador consulta B3, não a neutralidade do brief (`bin/codex_pergunta.sh:390`). Não encontrei `AGENTS.md`, `bin/gerar_agents_md.py` nem `bin/codex_brief_verificador.py` na cópia; ausência sem arquivo:linha = [nao verificado]. O item secundário de publicar GLOSSARIO já possui implementação no HEAD (`bin/relato.sh:111`); isso não prova a cura do contexto/brief. Commit dessa implementação = [nao verificado].

3. **Falta e aplicação — LEU:** BACKLOG registra falta de briefs e incompatibilidade rc 5 sem `PERGUNTA:` (`app/docs/BACKLOG.md:1038`); não se deve transformar a referência histórica a F1/V-FASE em confirmação de que continuam em voo. O complemento SEM-CONDUTA exige outras fontes e exclusão de conduta (`app/docs/BACKLOG.md:961`); a pergunta de desenho sobre ~42,7 KB de fontes, teto 30.000 B e inclusão das leis 6b/7b continua documentada (`app/docs/RELATO.md:1943`). Na raia, `main_moveu=True` para `bin/codex_limite_estado.py`, `bin/codex_pergunta.sh`, `bin/relato.sh` — dado do anexo, aplicação mecânica/conflitos textuais [nao verificado]. **INFERIU:** há reconciliação semântica necessária: wrapper atual acrescentou papel construtor e inventário (`bin/codex_pergunta.sh:195`), relógio por papel (`bin/codex_pergunta.sh:85`), e publicação atual já inclui GLOSSARIO/CLAUDE (`bin/relato.sh:111`, `bin/relato.sh:117`); copiar a versão antiga integralmente arriscaria essas mudanças.

4. **Arquivos em comum:** interseção da lista da raia com BUG-PAR/O168/HJ-090/E3 fornecida no prompt = vazia; arquivo local dessa lista não disponível para citar, [nao verificado].

5. **Depende — LEU:** pouso próprio de instrumento, separado de produto, pela L-105 (`app/docs/LEIS.md:131`); resolver teto/fontes e classificação das seções (`app/docs/RELATO.md:1943`). SEM-CONDUTA foi registrado como sucessor, **depois** de DA-CASA e ENXUTO, não substituição explícita de DA-CASA (`app/docs/RELATO.md:1993`). ENXUTO está registrado como pousado em `9ce5181d` (`app/docs/BACKLOG.md:958`), mas esse commit não foi conferido por Git. Nenhuma necessidade direta de `!` de motor/exportada/escala foi demonstrada [nao verificado].

6. **Proposta — INFERIU: CONCLUIR**, na fila de instrumentos depois do produto que está em voo, integrando o contrato SEM-CONDUTA antes de certificar o contexto; o defeito do brief permanece no código (`bin/codex_pergunta.sh:355`) e a ordem própria de instrumento está na L-105 (`app/docs/LEIS.md:131`). Não declarar “PRONTO” pelo BACKLOG: a medição documentada foi neutra (`app/docs/RELATO.md:2259`) e as decisões de fonte/teto permanecem registradas (`app/docs/RELATO.md:1943`).

---

**PADRAO-DE-CIMA**

1. **O que cura — LEU:** repetir perguntas de desenho da mesma entidade/programa sem reconhecer a classe comum; o contrato pede segunda ocorrência em 24 h bloquear construtor/verificador até PADRAO-DE-CIMA consumido uma vez (`app/docs/BACKLOG.md:836`, `app/docs/RELATO.md:1876`). O mecanismo presente conta reprovações por **obra**, não esse agrupamento (`bin/disjuntor.py:997`, `bin/disjuntor.py:1004`).

2. **Ainda existe — SIM, LEU/INFERIU:** PreToolUse atual aplica B3 por obra no wrapper e no verificador final, além do censo do construtor (`bin/disjuntor.py:1826`, `bin/disjuntor.py:1842`, `bin/disjuntor.py:1850`); não implementa ali a segunda pergunta por entidade/24 h. O defeito de prova discutido no desenho também é visível: `_mutante` considera a prova suficiente se o caso sair com qualquer retorno diferente de zero, sem identidade da causa (`bin/tests/test_disjuntor.sh:1522`; equivalente em `bin/tests/test_disjuntor.sh:1537`). O RELATO aponta explicitamente essa questão de “por que o mutante morreu” (`app/docs/RELATO.md:1845`). Nenhum commit de cura completa foi demonstrado [nao verificado].

3. **Falta e aplicação — LEU:** decidir o desenho da autoridade da causa de morte; o registro informa que a volta 10 ficou acima da estacionada sem verificador nem opus final (`app/docs/RELATO.md:1845`). Na raia, `main_moveu=True`: `bin/codex_pergunta.sh`, `bin/disjuntor.py`, `bin/hook_stop_fila1.py`, `bin/tests/test_disjuntor.sh`, `bin/tests/test_hook_esperando_com_prova.sh`, `bin/tests/test_lei_akita.sh` — dado do anexo; conflitos de merge/aplicação real [nao verificado]. **INFERIU:** rebase semântico obrigatório no PreToolUse, hoje com CENSO-ANTES-DA-VAGA (`bin/disjuntor.py:1850`), no wrapper com papel construtor (`bin/codex_pergunta.sh:195`), e no recorte do topo que já separa faixa de censo da janela B3 (`bin/hook_stop_fila1.py:467`). O número histórico `test_disjuntor.sh:1475` do RELATO envelheceu; `_mutante` relevante está em `bin/tests/test_disjuntor.sh:1522`.

4. **Arquivos em comum:** interseção com as quatro obras em voo do anexo = vazia; sem arquivo local citável, [nao verificado]. Há interseção entre estas duas obras interrompidas em `bin/codex_pergunta.sh`, pelo dado dos anexos [nao verificado].

5. **Depende — LEU:** decisão PDC-DESENHO (`app/docs/BACKLOG.md:836`, `app/docs/RELATO.md:1845`), contrato de instrumento antes da construção (`CLAUDE.md:394`), pouso separado de produto (`app/docs/LEIS.md:131`). B3 já existe no código com limiares de 1 reprovação final/3 voltas Codex (`bin/disjuntor.py:136`), portanto “esperar construir B3” não é dependência técnica atual inferível. DISJUNTOR-B1-ESPERA-OCIOSA está registrado depois de PDC (`app/docs/BACKLOG.md:837`). Nenhum `!` de motor/exportada/escala foi demonstrado [nao verificado].

6. **Proposta — INFERIU: CONCLUIR**, mantendo a posição registrada antes de B1-ESPERA-OCIOSA (`app/docs/BACKLOG.md:837`), mas construção condicionada à decisão concreta de desenho (`app/docs/RELATO.md:1845`). Há lacuna viva no agrupamento do PreToolUse (`bin/disjuntor.py:1826`) e na prova por retorno genérico do harness (`bin/tests/test_disjuntor.sh:1522`); não existe evidência suficiente para DEMOLIR por cura ou desenho superado.

| id | cura | existe no HEAD | falta | depende | proposta |
|---|---|---|---|---|---|
| CODEX-CONTEXTO-DA-CASA | Contexto verificável/brief neutro (`app/docs/BACKLOG.md:1038`) | SIM: pergunta crua (`bin/codex_pergunta.sh:355`) | Reconciliar wrapper e teto/fontes (`app/docs/RELATO.md:1943`) | SEM-CONDUTA; L-105 (`app/docs/RELATO.md:1993`; `app/docs/LEIS.md:131`) | CONCLUIR após produto; INFERIU |
| PADRAO-DE-CIMA | Agrupar desenho por entidade/24 h (`app/docs/RELATO.md:1876`) | SIM: B3 por obra e harness rc genérico (`bin/disjuntor.py:1004`; `bin/tests/test_disjuntor.sh:1522`) | Juiz da causa e certificação da volta (`app/docs/RELATO.md:1845`) | PDC-DESENHO (`app/docs/BACKLOG.md:836`) | CONCLUIR antes de B1, após decisão (`app/docs/BACKLOG.md:837`); INFERIU |

## 13/11 18:21 · sessao bc14b04d4500 · - · - · origem=Ronald (tmux)

**V3-RETOMA-PERIODOS-DA-CELULA**

1. **O que cura — LEU:** a alimentação entrega segmentos cortados pela pausa como `pares_por_dia`, e a intra recebe a pausa agregada do núcleo; perde-se a unidade do período para noturna e intra (`app/ponto/calculador/alimentacao.py:158`, `app/ponto/calculador/alimentacao.py:515`, `app/ponto/calculador/regras.py:764`). O split ainda lê HE do período do motor, enquanto a CCT é resolvida uma vez em `ini` (`app/ponto/calculador/regras.py:827`, `app/ponto/calculador/alimentacao.py:390`).

2. **Existe no HEAD? PARCIAL — LEU.** Permanecem segmentos em lugar de envelopes, intra diária, CCT em `ini` e split pelos minutos do motor (`app/ponto/calculador/alimentacao.py:158`, `app/ponto/calculador/alimentacao.py:390`, `app/ponto/calculador/regras.py:764`, `app/ponto/calculador/regras.py:827`). A recusa P4 também não está nessa porta: períodos sem dia/carimbo incrementam contadores e seguem por `continue`; `rubricas_por_dia` inicia a conta sem recusar esses contadores (`app/ponto/calculador/alimentacao.py:79`, `app/ponto/calculador/alimentacao.py:91`, `app/ponto/calculador/alimentacao.py:449`).
   
   Duas partes já chegaram por outro caminho: **P5**, cujo corpo une intervalos, corta por horas e limita a parte ao total (`app/ponto/motor_calculo_v2.py:162`, `app/ponto/motor_calculo_v2.py:186`, `app/ponto/motor_calculo_v2.py:196`); o relato registra o pouso **16c27de8** (`app/docs/RELATO.md:7675`). E **V3-DOBRA-SEM-NOTURNA** retirou o retorno da dobra e passou intervalos pela porta (`app/ponto/calculador/regras.py:793`, `app/ponto/calculador/alimentacao.py:107`, `app/ponto/calculador/alimentacao.py:527`); o relato registra o merge **424632fd**, de **f9a65d66** (`app/docs/RELATO.md:7720`). A existência desses commits no histórico Git não foi verificável; os identificadores são registros documentais, corroborados pelo código presente [nao verificado].

3. **Falta / aplicação — LEU + INFERIU:** falta portar a cura remanescente dos períodos, pausas por período, CCT do dia, distribuição reversa da HE e recusa P4; o contrato pedido inclui RED e censo AST (`app/docs/BACKLOG.md:964`, `app/ponto/calculador/alimentacao.py:158`, `app/ponto/calculador/alimentacao.py:390`, `app/ponto/calculador/regras.py:827`). O fechamento captura genericamente a falha do oráculo, portanto P4 precisa de resultado declarado até o consumidor, não apenas de uma exceção dentro da alimentação — **INFERIU** (`app/ponto/services/fechamento.py:1266`, `app/ponto/services/fechamento.py:1297`).
   
   O anexo informa `main_moveu=True` nos quatro arquivos de produção: `alimentacao.py`, `periodo.py`, `regras.py`, `motor_calculo_v2.py` [nao verificado]. **Conflito semântico comprovado:** a raia inclui novamente P5 e a retirada do retorno da dobra, já presentes; também substitui uma alimentação que hoje já carrega intervalos (`app/ponto/motor_calculo_v2.py:162`, `app/ponto/calculador/regras.py:793`, `app/ponto/calculador/alimentacao.py:107`). Aplicabilidade textual/cherry-pick e conflitos Git [nao verificado]. A recomendação técnica é reaproveitar os deltas restantes sobre o HEAD, preservando as curas presentes — **INFERIU**, pelos mesmos sítios.

4. **Arquivos em comum:** nenhum dos arquivos listados dessa raia aparece no `arquivos_em_voo.txt` fornecido; confronto restrito ao anexo [nao verificado].

5. **Depende de:** desenho já registrado: fonte única dos períodos, ancoramento por período, transbordo reverso e recusa do período não guardado (`app/docs/BACKLOG.md:964`). P5 já tem corpo no HEAD; o bloqueio “espera P5” não pode ser repetido como fato atual (`app/ponto/motor_calculo_v2.py:162`). Qualquer nova alteração do motor exige aval explícito (`CLAUDE.md:142`); trocar leitores e alcançar exportadas continua dependendo do fechamento do impacto e da lista de `!` por exportada (`app/docs/BACKLOG.md:1797`). **Não incorporar silenciosamente** RECORTE-LONGO ou HE-NOTURNA-POR-PEÇA: estão registrados como obras distintas (`app/docs/BACKLOG.md:966`, `app/docs/BACKLOG.md:972`).

6. **Proposta — INFERIU: CONCLUIR**, dentro da retomada V3, depois de um instrumento de impacto confiável e na posição já registrada “após O10-B, antes de OTIMIZA-3”; preservar P5 e DOBRA já recebidas (`app/docs/BACKLOG.md:1797`, `app/ponto/motor_calculo_v2.py:162`, `app/ponto/calculador/regras.py:793`). A demolição da obra inteira deixaria defeitos concretos presentes (`app/ponto/calculador/alimentacao.py:158`, `app/ponto/calculador/regras.py:764`).

**V3-RETOMA-FILA**

1. **O que cura — LEU:** o instrumento descarta a lavra real e monta sozinho o lado v2, omitindo HE100 de feriado e os splits noturnos; também reúne toda folga trabalhada numa rubrica (`app/ponto/management/commands/diff_calculador.py:123`, `app/ponto/management/commands/diff_calculador.py:137`, `app/ponto/management/commands/diff_calculador.py:245`, `app/ponto/management/commands/diff_calculador.py:263`). A lavra real lê essas rubricas e distingue folga certa, feriado e sem escala (`app/ponto/services/dia_pago.py:181`, `app/ponto/services/dia_pago.py:183`, `app/ponto/services/dia_pago.py:234`, `app/ponto/services/dia_pago.py:244`, `app/ponto/services/dia_pago.py:259`).

2. **Existe no HEAD? SIM para o item (1) — LEU:** `_dpm.lavrar` continua substituída por `_nada`, e o lado v2 continua derivado pelos laços próprios (`app/ponto/management/commands/diff_calculador.py:123`, `app/ponto/management/commands/diff_calculador.py:137`, `app/ponto/management/commands/diff_calculador.py:247`). O item (3), CCT em `ini`, também permanece no consumidor da v3 (`app/ponto/calculador/alimentacao.py:390`). Os números de noturna e o caso de cadastro dos itens (2)/(4) são alegações registradas, não diagnóstico confirmado pela leitura (`app/docs/BACKLOG.md:976`); situação atual da frota [nao verificado].

3. **Falta / aplicação:** o BACKLOG registra “falta: pouso da V3”, mas a raia fornecida constrói apenas a correção do instrumento; os itens (2)–(4) exigem nova medição/classificação e o item (3) cruza a obra de períodos (`app/docs/BACKLOG.md:976`, `app/ponto/calculador/alimentacao.py:390`). Na retomada, verificar captura de toda lavra, falhas e cobertura completa: o fechamento absorve exceções de lavratura, logo silêncio não prova resultado — **INFERIU** (`app/ponto/services/fechamento.py:1238`).
   
   Segundo o anexo, nenhum dos dois arquivos tem `main_moveu=True`; isso favorece reaproveitamento, mas aplicação textual e conflitos Git [nao verificado]. Não há evidência de outra cura presente para o laço independente (`app/ponto/management/commands/diff_calculador.py:247`).

4. **Arquivos em comum:** nenhum dos dois arquivos listados dessa raia aparece no `arquivos_em_voo.txt` fornecido; confronto restrito ao anexo [nao verificado].

5. **Depende de:** pouso próprio de instrumento L-105 e gate POUSO-DA-V3 registrados na linha; não exige modificar motor, escala/vínculo ou exportada para corrigir o instrumento (`app/docs/BACKLOG.md:976`). A parte CCT deve usar o desenho da obra de períodos; não duplicar uma segunda cura para o mesmo sítio — **INFERIU** (`app/docs/BACKLOG.md:964`, `app/ponto/calculador/alimentacao.py:390`). Números antigos não fecham o impacto atual; a cobertura integral e a lista de `!` seguem abertas no registro V3 (`app/docs/BACKLOG.md:1797`).

6. **Proposta — INFERIU: CONCLUIR**, em pouso próprio na fila de instrumentos, antes do próximo re-DIFF usado para decidir a V3; manter explícito o gate registrado “pouso da V3” e levar ao chat qualquer inversão dessa dependência (`app/docs/BACKLOG.md:976`, `app/ponto/management/commands/diff_calculador.py:137`). O defeito do instrumento continua observável e invalida comparações dessas rubricas por construção (`app/ponto/management/commands/diff_calculador.py:253`, `app/ponto/services/dia_pago.py:181`).

| id | cura | existe no HEAD | falta | depende | proposta |
|---|---|---|---|---|---|
| V3-RETOMA-PERIODOS-DA-CELULA | Preservar período/pausa e distribuir HE por período (`alimentacao.py:158`, `regras.py:827`, caminhos acima) | PARCIAL; P5 e dobra recebidos (`motor_calculo_v2.py:162`, `regras.py:793`) | Fonte por período, intra, CCT diária, P4 (`app/docs/BACKLOG.md:964`) | Impacto integral e `!` por exportada (`app/docs/BACKLOG.md:1797`) | CONCLUIR na retomada V3 — INFERIU (`app/docs/BACKLOG.md:1797`) |
| V3-RETOMA-FILA | Lado v2 do DIFF pela lavra (`app/ponto/management/commands/diff_calculador.py:137`) | SIM (`app/ponto/management/commands/diff_calculador.py:247`) | Instrumento completo + re-DIFF/classificação (`app/docs/BACKLOG.md:976`) | Pouso próprio/gate V3 (`app/docs/BACKLOG.md:976`) | CONCLUIR antes de novo re-DIFF decisório — INFERIU (`app/ponto/management/commands/diff_calculador.py:247`) |

## 13/11 18:21 · sessao e758d2df5363 · - · - · origem=Ronald (tmux)

**ATA-X-TURNO-PAUSA — recomendação: CONCLUIR, reaproveitando seletivamente a raia; existência PARCIAL no HEAD.** A cura de alinhamento pelo papel já está presente, mas falta a reconciliação posterior da pausa e sua publicação na CxR. **LEU:** `app/escala/utils.py:502`, `app/escala/utils.py:572`, `app/escala/utils.py:1975`, `app/escala/utils.py:2039`, `app/ponto/services/cxr_na_lista.py:30`.

1. **O que cura.** A ata pode deixar órfãs as batidas da pausa reconhecida pelo juiz: o alinhamento assenta o par, mas o cluster pode retirar um marco; a reconciliação posterior percorre apenas entrada/saída. **LEU:** `app/escala/utils.py:561`, `app/escala/utils.py:572`, `app/escala/utils.py:1975`, `app/escala/utils.py:1993`.  
   A obra também acrescenta acusação `l084_pausa` e seus detalhes à CxR; o HEAD ainda só publica os formatos anteriores. **LEU:** `app/ponto/motor_calculo_v2.py:1655`, `app/ponto/motor_calculo_v2.py:2717`, `app/ponto/services/cxr_na_lista.py:29`, `app/escala/services/cadastro_realidade.py:365`.

2. **Existe no HEAD? PARCIAL.** Já há `_papeis_do_turno`, pares e alinhamento por papel no juiz; portanto não se pode repetir a hipótese de que todo o problema continua intacto. **LEU:** `app/escala/utils.py:437`, `app/escala/utils.py:502`, `app/escala/utils.py:585`. A reconciliação “METADE PAUSA” do anexo não está entre o fim das bordas e o cálculo do realizado. **LEU:** `app/escala/utils.py:2039`. A acusação específica e os campos de pausa também faltam. **LEU:** `app/ponto/motor_calculo_v2.py:1673`, `app/ponto/motor_calculo_v2.py:2717`, `app/ponto/services/cxr_na_lista.py:30`. Ocorrência atual do caso col841, quantidade de afetados e dinheiro atual: **[não verificado]**.

   **Cura por outro caminho:** o registro identifica `b3bf529e` como JUIZ-DO-PAR-CURA-NA-FONTE, sem editar o motor, e classifica JUIZ-DO-PAR-DUAS-ENTRADAS como absorvida; o código atual confirma o papel dentro de `_match_marcos`. **LEU documental:** `app/docs/RELATO.md:212`, `app/docs/RELATO.md:2696`, `app/docs/RELATO.md:2736`; **LEU código:** `app/escala/utils.py:590`. A ancestralidade Git desse commit é **[não verificado]**.

3. **Falta / aplicação.** Exigem-se 10/10 explicados e dinheiro igual; isso é condição registrada, não resultado verificado nesta auditoria. **LEU:** `app/docs/BACKLOG.md:1725`, `app/docs/CORTES.md:304`. O despacho posterior manda refazer o bloco para perguntar à autoridade e refazer a fixture após E1. **LEU:** `app/docs/RELATO.md:1082`.

   `main_moveu=True` no anexo: `app/core/tests/test_medida_turno_do_par.py`, `app/escala/services/cadastro_realidade.py`, `app/escala/utils.py`, `app/ponto/turnos.py`; comparação histórica confirmada somente pelo anexo **[não verificado]**. Sítios atuais abertos:
   - CxR mudou de posição e continua sem formato da pausa: `app/escala/services/cadastro_realidade.py:360`.
   - Conservação de órfãs continua sem `_orfa_civil`; reconciliação atual só de bordas: `app/escala/utils.py:1816`, `app/escala/utils.py:1975`.
   - `turnos_de_batidas` continua sem argumento `papel_por_minuto`: `app/ponto/turnos.py:1517`, `app/ponto/turnos.py:1535`.
   - A catraca congela linhas e já registra ocorrências atuais do motor: `app/core/tests/test_medida_turno_do_par.py:17`, `app/core/tests/test_medida_turno_do_par.py:253`.

   **INFERIU:** a raia precisa de recomposição; portar integralmente a entrada antiga de papel no motor exige justificar efeito adicional depois da cura na fonte, já absorvida. **Base:** `app/docs/RELATO.md:212`, `app/docs/RELATO.md:2780`, `app/ponto/motor_calculo_v2.py:867`. Conflito textual exato/merge limpo: **[não verificado]**.

4. **Arquivos em comum com voo.** Pelo anexo: `app/escala/services/cadastro_realidade.py` com BUG-PAR e E3; `app/core/tests/test_medida_turno_do_par.py` com BUG-PAR e HJ-090; nenhum com O168. Anexo sem numeração: **[não verificado]**.

5. **Depende.** `! ATA-PAUSA-NO-MOTOR` já tem escopo e condições explícitos; dinheiro igual não dispensa essas condições. **LEU:** `app/docs/CORTES.md:304`. Autoridades vivas: ocupação do marco em `_match_marcos`, pausa em `intervalos_do_turno`. **LEU:** `app/core/juizes.py:212`, `app/core/juizes.py:214`. Exportada é lida pela CxR sem recalcular. **LEU:** `app/ponto/services/cxr_na_lista.py:25`. A antiga pergunta de desenho está marcada respondida, com instrução concreta no despacho; não tratá-la como autorização para uma implementação diferente. **LEU:** `app/docs/RELATO.md:1082`, `app/docs/RELATO.md:1090`.

6. **Proposta. CONCLUIR.** Fila 1, respeitando E3 em voo, os arquivos comuns e a posição registrada depois de C7/COBPARC; a conclusão destrava CXR-PRONTO1-COL841. **INFERIU proposta**, baseada em `app/docs/RELATO.md:1003`, `app/docs/BACKLOG.md:798`, `app/docs/BACKLOG.md:1783`. Não demolir a obra inteira: sua parte de CxR/pausa ainda não aparece no HEAD. **LEU:** `app/ponto/services/cxr_na_lista.py:30`, `app/escala/utils.py:2039`.

## 13/11 18:21 · sessao 427c2402e28c · - · - · origem=Ronald (tmux)

V3-E1 — censo somente leitura

1. **O que cura — LEU.** É uma bateria de certificação da v3 pela regra, não uma correção de produção: o requisito pede casos validados, propriedades fixas e DIFF somente como impacto; o sítio de ensaio é `app/ponto/calculador/periodo.py:543`, cuja docstring declara `reler` como leitura de ensaio para bateria/DIFF (`app/docs/BACKLOG.md:821`; `app/ponto/calculador/periodo.py:546`). A interrupção “motivo: não sei” é a célula do cadastro; o relato mais específico registra 25 validados verdes na v3 e 14 falhas de STATUS em 11 casos, não falha generalizada da v3 (`app/docs/BACKLOG.md:2081`; `app/docs/RELATO.md:2055`).

2. **Existe no HEAD? PARCIAL — LEU + INFERIU.** A necessidade de incorporar/reconferir a certificação permanece registrada; testes atuais dizem expressamente que a bateria E1 mora noutra raia (`app/docs/BACKLOG.md:2081`; `app/ponto/tests/test_e3_status_lavrado.py:3-7`). O pacote nominal da raia contém cinco arquivos ausentes nesta cópia: `E1-PERGUNTA-AO-DP-CONTADOR.md`, três `test_e1_lote*` e `test_v3_e1_valor.py`; ausência constatada na leitura do inventário, sem sítio existente para citar [nao verificado].
   
   Parte dos defeitos históricos de STATUS recebeu cura por E3/E4: o HEAD devolve `irregular` para previsto sem batida com veredito acusador e `ok` para isento, cobrindo exemplos S02/S19 (`app/ponto/status_dia.py:150-155`); os testes locais afirmam esses resultados (`app/ponto/tests/test_e3_status_lavrado.py:54-64`). Isso **não prova** que os 11 casos históricos estejam todos curados [nao verificado]. Há registro de pouso posterior da E4-CALENDARIO-L1 em `c960973b`, mas ancestralidade Git e execução atual não foram verificadas (`app/docs/BACKLOG.md:877`) [nao verificado].

3. **O que falta e aplicabilidade — LEU / INFERIU.** Reconstituir o pacote de certificação sobre o HEAD e rodar seus casos, separando VALOR de STATUS; não carregar os 14 vermelhos de 31/10 como resultado atual (`app/docs/RELATO.md:2055`; `app/ponto/tests/test_e3_status_lavrado.py:3-7`). Preservar marcadores de pendência: o BACKLOG lista C06/C16/C17, mas a medição posterior inclui **também C05** em `PENDENTE_DP`; a consolidação deve explicar essa diferença (`app/docs/BACKLOG.md:896`; `app/docs/RELATO.md:2055`). C17 já foi reescrito para HE 0/intra 60; não falta repetir essa decisão (`app/docs/RELATO.md:2061-2064`). Conferir o gate CCT, antes não discriminado, propriedades e DIFF com dono por rubrica (`app/docs/RELATO.md:2065`; `app/docs/BACKLOG.md:821`). Pelo anexo, os cinco arquivos têm `main_moveu=False`; aplicação textual/integridade das APIs/testes não foi testada e fica [nao verificado]. O HEAD ainda possui `reler(resultado, regua_de, marcos=None, fora_por_lei=None, cct=None)` (`app/ponto/calculador/periodo.py:543`).

4. **Em comum com obras em voo — dado dos anexos.** A interseção literal dos cinco caminhos da V3-E1 com BUG-PAR/O168/HJ-090/E3 é vazia; conferência apenas do dado fornecido, sem raias disponíveis [nao verificado].

5. **Depende de — LEU.** Respostas do DP/contador para casos ainda pendentes, identificação do dono das divergências pela L-099 e certificação pela L-110 (`app/docs/BACKLOG.md:896`; `app/docs/RELATO.md:2055`; `app/docs/LEIS.md:136`; `app/ponto/management/commands/e6_oraculo.py:53-70`). A bateria em si não exige alterar a v2; eventual alteração de `motor_calculo_v2.py`, gravado exportado ou dados de escala/vínculo exige `!` pelo contrato da casa (`CLAUDE.md:265-267`). A troca para v3 é expressamente separada e “só com !” na V3-RETOMA (`app/docs/BACKLOG.md:963`). A documentação do ADR mantém valor em `DiaPago`, status na célula e autoridade única da lavra (`app/docs/ADR/0002-celula-e-o-dia.md:8-14`).

6. **Proposta — INFERIU: CONCLUIR**, na pista de certificação da v3, antes de sua troca; reavaliar os casos STATUS contra E3/E4 já existentes, sem tratar a raia como cura de produção (`app/docs/BACKLOG.md:821`; `app/ponto/tests/test_e3_status_lavrado.py:3-7`; `app/docs/BACKLOG.md:963`). Não demolir: há utilidade independente de certificação e pendências explícitas, mesmo com curas posteriores de alguns status (`app/docs/LEIS.md:136`; `app/docs/BACKLOG.md:896`; `app/ponto/status_dia.py:150-155`).

| id | cura | existe no HEAD | falta | depende | proposta |
|---|---|---|---|---|---|
| V3-E1 | Certificação v3 pela regra (`BACKLOG.md:821`) | PARCIAL: bateria noutra raia; exemplos STATUS curados (`test_e3_status_lavrado.py:7`; `status_dia.py:150`) | Revalidar pacote, pendências DP e impacto (`RELATO.md:2055`; `RELATO.md:2065`) | DP/L-110; `!` só se atingir âmbito protegido ou troca (`BACKLOG.md:896`; `CLAUDE.md:265`; `BACKLOG.md:963`) | CONCLUIR antes da troca v3 — INFERIU (`BACKLOG.md:821`) |

Os caminhos abreviados da tabela referem-se aos arquivos completos citados acima; nenhum teste foi executado, portanto verde atual [nao verificado]. [nao verificado]

## 13/11 18:21 · sessao ed13a48ec55c · - · - · origem=Ronald (tmux)

**LASTRO-DEGRAU-2 — LEU: defeito SIM; INFERIU: concluir a cura residual, reconstruindo sobre o HEAD.** O recuo ainda calcula `abs(timestamp - combine(dia, hora)) <= tolerancia`, sem consultar a atribuição do juiz do marco; `TOLERANCIA_MIN = 90` permanece. Isso conserva os problemas de data do noturno e de distinguir papéis com o mesmo tipo/hora. O algoritmo está comprovado; a reprodução dos casos de produção hoje é [não verificado]. (`app/ponto/services/lastro.py:59`, `app/ponto/services/lastro.py:299`, `app/ponto/services/lastro.py:304`, `app/core/juizes.py:212`)

**O que cura, em duas linhas:** elimina o segundo casamento de batida com marco no degrau sem lâmpada, lendo a resposta da grade/juiz do par.  
O sítio vivo continua em `julgar`, que pareia sem alimentação específica e depois compara a marca com uma hora civil do dia. (`app/ponto/services/lastro.py:268`, `app/ponto/services/lastro.py:304`)

**O que falta:** o BACKLOG pede cura anterior no construtor e substitui explicitamente a solução “perguntar ao raio” por “perguntar ao juiz”; exige RED/DIFF dos leitores do grupo (1). A dependência de janela aparece registrada, mas não a tome como reprodução atual comprovada. (`app/docs/BACKLOG.md:1756`, `app/docs/BACKLOG.md:811`, `app/docs/BACKLOG.md:1066`)

Parte da causa descrita na raia já mudou: a grade agora aplica folga e ausência uma vez por pessoa, contra o vínculo do dia. A régua ainda depende do conjunto de vínculos da janela, e o cartório monta a janela a partir das extremidades de `cels`; logo, é preciso repetir a equivalência unitária × lote antes de fechar. **LEU** o mecanismo; **INFERIU** a necessidade da nova verificação; caso col879 hoje [não verificado]. (`app/escala/utils.py:1843`, `app/escala/utils.py:1313`, `app/escala/utils.py:1350`, `app/ponto/services/cartorio.py:1131`, `app/ponto/services/cartorio.py:1156`)

**A raia não serve para pouso literal — LEU/INFERIU:** o HEAD transformou o comando em vigia, retirou `--apply` e fecha por `fechar_pelo_marco_colab`; a raia fornecida ainda altera `fechar()` e chama essa API antiga. Deve preservar o caminho de evento, o balde `retido_pelo_fechamento` e a distinção de batida/dispensa/isenção. (`app/ponto/management/commands/fechar_cobranca_com_lastro.py:20`, `app/ponto/management/commands/fechar_cobranca_com_lastro.py:44`, `app/ponto/services/lastro.py:364`, `app/ponto/services/lastro.py:292`, `app/ponto/services/lastro.py:317`, `app/ponto/services/lastro.py:327`; diff antigo: [não verificado])

Além disso, o compositor da raia fornecida chama `montar_grade_prevista_periodo` sem `agora`, enquanto o HEAD exige esse argumento nomeado; a extração precisa adaptar o relógio. O cartório ainda compõe a grade inline. (`app/escala/utils.py:2084`, `app/ponto/services/cartorio.py:1175`; chamada da raia: [não verificado])

**Cura parcial por outra obra:** o registro atribui a passagem do cron para evento à LASTRO-PARTE-2, commit `a7372499`; o código lido confirma cron `--alarme` e fecho no evento. Isso não curou a conta linear do degrau 2. Histórico Git/ancestralidade [não verificado]. (`app/docs/BACKLOG.md:753`, `app/config/crons.py:729`, `app/config/crons.py:731`, `app/ponto/services/lastro.py:364`, `app/ponto/services/lastro.py:304`)

O registro identifica `6f583cbc` como pouso GRADE-FOLGA-JUIZ; o overlay atual confirma mudança pertinente na causa das folgas, mas não prova equivalência completa entre janelas. (`app/docs/RELATO.md:7795`, `app/escala/utils.py:1843`)

**Arquivos `main_moveu=True`:** comando, `cartorio.py`, `lastro.py`, `test_bug114_fechador_de_dois_dias.py`, `test_tripwire_b5j_universo_de_apuracao.py`, conforme anexo; flags e conflitos de merge mecânicos [não verificado]. Os conflitos semânticos comprovados são comando vigia × fechador antigo, APIs do lastro e relógio obrigatório do builder. (`app/ponto/management/commands/fechar_cobranca_com_lastro.py:44`, `app/ponto/services/lastro.py:364`, `app/escala/utils.py:2084`)

**Arquivos em comum:** nenhum dos seis arquivos da raia LASTRO aparece nas listas fornecidas de BUG-PAR/O168/HJ-090/E3; não existe arquivo local com essa lista para citação: [não verificado].

**Depende de:** juiz único e leitura da lâmpada; verificar/corrigir a invariância da grade antes de concluir. Não há alteração direta de `motor_calculo_v2.py` na raia fornecida [não verificado]; eventual alteração do motor, escala/vínculo ou gravado exportado depende do `!` correspondente. (`CLAUDE.md:169`, `CLAUDE.md:178`, `CLAUDE.md:265`, `app/docs/BACKLOG.md:1756`)

**Proposta — INFERIU: CONCLUIR**, como continuação do grupo (1), após a verificação/cura do construtor e antes de declarar esse grupo zerado. A solução antiga pelo raio foi superada, mas o defeito linear permanece; deve manter o fecho por evento já presente. (`app/docs/BACKLOG.md:811`, `app/docs/BACKLOG.md:1756`, `app/ponto/services/lastro.py:304`, `app/ponto/services/lastro.py:364`)

| id | cura | existe no HEAD | falta | depende | proposta |
|---|---|---|---|---|---|
| LASTRO-DEGRAU-2 | Recuo lê juiz do marco | SIM — `app/ponto/services/lastro.py:304` | Adaptar ao evento/relógio; verificar janela — `app/ponto/services/lastro.py:364`, `app/escala/utils.py:2084` | Construtor + grupo (1) — `app/docs/BACKLOG.md:1756` | CONCLUIR após construtor — inferência apoiada em `app/docs/BACKLOG.md:811` |

## 13/11 18:21 · sessao b4d6e652fb2d · - · - · origem=Ronald (tmux)

**OTIMIZA-1 — LEU no HEAD**

1. **Cura:** falta medir suites inteiras por pouso e publicar o resultado diário no ESTADO; o contrato pede início/fim, chamador, escopo e rc real. Hoje a trava apenas registra quem segura o lock e executa o comando, sem essa trilha (`bin/trava_teste.sh:35`; contrato: `app/docs/RELATO.md:1648`).

2. **Existe no HEAD? SIM.** A trava termina em `"$@"`; o painel compõe as seções atuais sem a medição suites/pouso (`bin/trava_teste.sh:42`; `bin/gerar_estado.py:370`). Não há commit de cura verificável nesta cópia [nao verificado].

3. **Falta:** suite, replay, verificador e pouso próprio, como registrado no BACKLOG; PRONTO 5 continua sem mapa histórico promovido (`app/docs/BACKLOG.md:2083`; `app/docs/RELATO.md:1637`). O diagnóstico escrito atribui as falhas ao conteúdo histórico e deixa a causa sem nome: é relato, não prova atual de execução (`app/docs/RELATO.md:1635`).  
   **Raia:** o anexo marca `main_moveu=True` em `gerar_estado.py`, `suite.sh` e `test_suite_sh.sh` [nao verificado]. LEU mostra conteúdo novo a preservar: guardas de motivo da inteira e seu selo (`bin/suite.sh:133`; `bin/tests/test_suite_sh.sh:31`), além de novas seções do painel (`bin/gerar_estado.py:277`). Aplicabilidade limpa/conflito textual: [nao verificado].

4. **Arquivos em comum:** nenhum da raia original com as quatro obras em voo, conforme comparação dos anexos [nao verificado]. O complemento novo toca `vigiar_fio_morto.py`, comum a BUG-PAR, HJ-090 e E3, segundo a lista raspada [nao verificado]; o complemento está registrado para depois da E3 (`app/docs/BACKLOG.md:738`).

5. **Depende de:** SUITE-POR-IMPACTO/PRONTO 5 para a medição de evitadas e fechamento do comboio (`app/docs/RELATO.md:1645`). Instrumento exige pouso separado do produto pela L-105 (`app/docs/LEIS.md:131`). O complemento de carga pede contrato antes e só após E3 (`app/docs/BACKLOG.md:738`). Não há exigência de `!` do motor no núcleo descrito (`app/docs/RELATO.md:1648`).

6. **Proposta — INFERIU: CONCLUIR**, como fechamento do instrumento SUITE-POR-IMPACTO, depois de resolver PRONTO 5; preservar o trabalho da mesma raia e certificar ambos num pouso de instrumento. O defeito permanece no escritor e no painel (`bin/trava_teste.sh:35`; `bin/gerar_estado.py:370`); não há fundamento lido para demolir.

**SUITE-POR-IMPACTO — LEU no HEAD** [nao verificado]

1. **Cura:** reduzir as voltas intermediárias por cobertura teste→arquivo, com fallback seguro para inteira e certificação da árvore que pousa (`app/docs/PROMPTS.md:1682`). Hoje `--volta` chama o seletor `vizinhos_da_volta.py`, sem o mapa de coverage previsto (`bin/suite.sh:82`).

2. **Existe no HEAD? SIM no núcleo; PARCIAL nas guardas adjacentes.** A seleção atual continua por vizinhos (`bin/suite.sh:88`). A regua ainda pode sair “JA VERDE” comparando só o FP de `.py/.html`, antes do loop de selos de host (`bin/regua.sh:33`; `bin/regua.sh:84`; `bin/regua.sh:138`). Entretanto, o pre-push já exige também hash da árvore; portanto a afirmação ampla de cegueira de ambos no BACKLOG está parcialmente envelhecida (`bin/pre-push.sh:65`; `app/docs/BACKLOG.md:1037`). Commit responsável: [nao verificado].

3. **Falta:** mapa/replay PRONTO 5 e rebase (`app/docs/BACKLOG.md:1036`). O aval pede reproduzir os cinco FAIL da O261(C), com mapa gerado no pai da regressão; teste novo/alterado deve rodar sempre, e só criação/remoção/renomeação de `.py` de produção força inteira (`app/docs/PROMPTS.md:1682`; `app/docs/PROMPTS.md:1683`; `app/docs/PROMPTS.md:1686`). A documentação registra falha da geração no pai e mapa antigo expirado; não certifica o replay (`app/docs/RELATO.md:1635`; `app/docs/RELATO.md:1637`).  
   **Raia:** mesmas três entradas `main_moveu=True` do OTIMIZA-1 no anexo [nao verificado]. A adaptação precisa incorporar a guarda atual de seleção efetivamente inteira, o motivo `pre-opus|pouso` e o selo correspondente (`bin/suite.sh:140`; `bin/suite.sh:173`; `bin/tests/test_suite_sh.sh:42`). Conflito textual e rebase limpo: [nao verificado].

4. **Arquivos em comum:** nenhum da raia `bin/*` com os anexos das obras em voo [nao verificado]. O complemento DIFF-POR-IMPACTO ainda não tem lista de arquivos de implementação fornecida [nao verificado].

5. **Depende de:** PRONTO 5, certificação final e pouso próprio L-105 (`app/docs/BACKLOG.md:1036`; `app/docs/LEIS.md:131`). O aval original exige pousar depois do CALENDARIO (`app/docs/PROMPTS.md:1682`); o BACKLOG registra CALENDARIO-HOJE-MINIMO fechado, mas isso sozinho não prova ancestralidade nesta cópia (`app/docs/BACKLOG.md:819`). O complemento entra na ordem viva somente após E3; GATE-PELA-CLASSE é lei nova pendente de corte e não deve ser confundido com o núcleo já autorizado (`app/docs/BACKLOG.md:1036`; `app/docs/PROMPTS.md:2243`).

6. **Proposta — INFERIU: CONCLUIR**, primeiro no comboio de instrumentos com OTIMIZA-1, após o portão da E3 registrado para a retomada; prioridade para recuperar a prova PRONTO 5 e adaptar as guardas novas. O seletor atual reduz por outro mecanismo, mas não entrega o contrato de coverage e certificado (`bin/suite.sh:88`; `app/docs/PROMPTS.md:1683`). Não há evidência lida de desenho superado que justifique demolir.

| id | cura | existe no HEAD | falta | depende | proposta |
|---|---|---|---|---|---|
| OTIMIZA-1 | Inteiras/pouso no ESTADO (`RELATO.md:1648`) | SIM (`bin/trava_teste.sh:42`) | Suite/replay/verificador/pouso (`BACKLOG.md:2083`) | SUITE-POR-IMPACTO; L-105; complemento pós E3 (`BACKLOG.md:738`) | CONCLUIR junto ao fechamento do instrumento — INFERIU |
| SUITE-POR-IMPACTO | Seleção por coverage + certificado (`PROMPTS.md:1682`) | SIM; guardas adjacentes parciais (`bin/suite.sh:88`; `bin/pre-push.sh:65`) | Mapa/replay PRONTO 5 + rebase (`BACKLOG.md:1036`) | PRONTO 5; L-105; complemento pós E3 e corte de lei (`BACKLOG.md:1036`) | CONCLUIR antes do fechamento OTIMIZA-1 — INFERIU |

## 13/11 18:21 · sessao 9b191b42339e · - · - · origem=Ronald (tmux)

**COBPARC-FILA-RELOGIO**

1. **O que cura — LEU:** o mesmo lote pode julgar “fato novo” em instantes diferentes: fora do julgamento, `agora_deste_julgamento()` retorna `None`, e cada `houve_fato_novo(..., ate=None)` lê `now()` novamente. Sítios: `app/ponto/services/cartorio.py:82`, `app/ponto/services/cartorio.py:373`.

2. **Ainda existe: SIM — LEU.**
   - c2 passa o relógio potencialmente `None`: `app/chamados/reconciliador.py:877` e `app/chamados/reconciliador.py:884`.
   - cluster captura `None` uma vez, mas o predicado abaixo resolve esse `None` a cada dia: `app/ponto/services/cluster_espurio.py:164`, `app/ponto/services/cluster_espurio.py:166`, `app/ponto/services/cluster_espurio.py:169`, `app/ponto/services/cartorio.py:428`.
   - retenção chama `dia_na_lista_do_admin` sem instante, que pergunta `agora_deste_julgamento()`: `app/chamados/services/pergunta_no_app.py:144`, `app/chamados/services/disputa_emissao.py:940`.
   - reabertura consulta fato novo sem teto explícito: `app/chamados/models.py:1347`.
   - não há evidência de cura desses sítios por outra obra; commit de história não acessível nesta cópia [nao verificado]. [nao verificado]

3. **Falta e reaplicação.** BACKLOG registra mutantes_v4, suíte, opus e resposta do padrão; a alegação de “census 28 sítios a 0” pertence à raia e não comprova HEAD: `app/docs/BACKLOG.md:860`. **INFERIU:** reaproveitar a ideia exige adaptar a costura atual: o cron da raia que chamava `reconciliar_chamados_colab` foi substituído por vigia, cujo HEAD só classifica/conta (`app/chamados/management/commands/reconciliar_chamados.py:1`, `app/chamados/management/commands/reconciliar_chamados.py:75`); o laço de retratação foi extraído para `retratar_premissa_morta_colab`, inclusive chamado pelo consumidor do O-OUTBOX (`app/chamados/reconciliador.py:1446`, `app/chamados/reconciliador.py:1457`). Aplicação textual/merge real: [nao verificado].

   O anexo marca `main_moveu=True` nos seguintes arquivos — metadados de raia não verificáveis por Git nesta cópia [nao verificado]:
   - `app/chamados/management/commands/{disparar_perguntas_competencia,reabrir_orfas_enterradas,reconciliar_chamados,smoke_chamados}.py`;
   - `app/chamados/{models,reconciliador}.py`;
   - `app/chamados/services/{adesao,becos,competencia_trancada,disputa_emissao,fabrica_da_celula,pergunta_no_app}.py`;
   - `app/chamados/tests/test_selo_universo_reconciliador.py`;
   - `app/ponto/management/commands/{detectar_ausencias,detectar_cluster_espurio,processar_alertas_avancados,processar_alertas_turno,reconciliar_fantasmas,supra_juiz}.py`;
   - `app/ponto/services/{cartorio,cluster_espurio,geofence,justificativa}.py`;
   - `app/ponto/tests/test_o_outbox_celula.py`; `app/ponto/views.py`.

4. **Em comum, segundo anexos:** `app/chamados/models.py` com BUG-PAR, O168 e E3; `app/chamados/services/disputa_emissao.py` com BUG-PAR e E3. Nenhum com HJ-090. Comparação dos anexos, sem arquivo local com linhas [nao verificado].

5. **Depende de:** resposta do padrão, explicitamente aberta, e do estacionamento por aval; a própria célula diz que é fila e não trava L1 nem contrato 20: `app/docs/BACKLOG.md:860`. A regra de revisão enumera relógio fora da borda e exige suíte inteira antes do opus: `CLAUDE.md:374`, `CLAUDE.md:378`. Não há `!` do motor, exportada ou vínculo indicado nessa célula: `app/docs/BACKLOG.md:860`.

6. **PROPOSTA — INFERIU: CONCLUIR**, após estabilização de BUG-PAR/O168/E3 nos dois arquivos compartilhados e resposta do padrão; manter abaixo de obra que trave L1/contrato 20, conforme o estacionamento registrado (`app/docs/BACKLOG.md:860`). O defeito continua demonstrável nos sítios acima; o antigo caminho do cron precisa ser descartado da reaplicação em favor dos caminhos por evento do HEAD (`app/chamados/management/commands/reconciliar_chamados.py:6`, `app/chamados/reconciliador.py:1446`). Posição ordinal em fila global sem leitura das demais obras: [nao verificado].

## 13/11 18:16 · sessao f735193635be · RAIA-PREPARACAO · censo · origem=Code

**LEU** = código presente nesta cópia. **INFERIU** = consequência indicada a partir dele. Não consegui certificar o HEAD: a pasta não contém repositório Git consultável; o hash informado permanece **[nao verificado]**.

Os anexos abaixo são os textos da mensagem, não arquivos conferidos no HEAD **[nao verificado]**. Para referências: `complemento_hj_091.txt` começa em “aval Ronald” como linha 1; `passivo.r` começa em `sonda_leitura` como linha 1 do bloco. A linha HJ-091 não aparece nas quatro linhas do BACKLOG fornecido; sua redação completa permanece **[nao verificado]**.

**1. Censo AST: script, resultado e autoridades**

Script de análise, para executar por stdin com `python3 -B`; não importa Django nem a aplicação e não escreve arquivos. `--detalhe` imprime os candidatos por linha. As atribuições de tipo não demonstradas pelo AST continuam sendo candidatos, não escritores/leitores certificados. **[nao verificado: script entregue em memória, sem artefato no HEAD]**

```python
import ast
import sys
from pathlib import Path
from collections import Counter

TARGETS = {
    "ponto.services.ausencia": """
        criar_ausencia garantir_ausencia editar_ausencia_registro
        rejulgar_status_pelo_tipo mudar_estado decidir anexar_documento
        reverter_rejeicao _reabrir escalonar_aguardando_documento
        espelhar_ferias apagar_ausencias_de_ferias
        rejeitar_ausencias_de_ferias apagar_ausencias_em_ensaio
        rederivar_fim derivar_data_fim
    """.split(),
    "ponto.views": ["_a4_decidir"],
    "ferias.services": """
        _trocar_janela em_gozo_hoje ferias_na_janela
    """.split(),
    "ferias.models": ["status_por_data"],
    "ponto.turnos": """
        ausencia_cobre cobertura_ausencia_periodo dias_da_ausencia
    """.split(),
}
WANTED = {m + "." + n for m, ns in TARGETS.items() for n in ns}
FIELDS = set("""
    colaborador colaborador_id tipo data data_fim dias_corridos minutos
    descricao status documento documento_referencia periodo_aquisitivo
    data_inicio dias agendado_por aprovado_por aprovada_em atualizado_em
    agendamento_ferias agendamento_ferias_id
""".split())
ORIGINS = {
    "ponto.services.ausencia", "ponto.models", "ponto.views",
    "ferias.models", "ferias.services", "ferias.situacao", "folha.export",
}
calls, fields, models = set(), set(), set()
errors, files = [], 0

for root in ("app", "bin", "mensageria"):
    for p in sorted(Path(root).rglob("*.py")):
        if any(x in {"tests", "migrations", "__pycache__"} for x in p.parts):
            continue
        if p.name.startswith(("test_", "tests")):
            continue
        try:
            tree = ast.parse(p.read_text(encoding="utf-8"), filename=str(p))
        except (SyntaxError, UnicodeError) as e:
            errors.append((str(p), str(e)))
            continue
        files += 1
        parts = p.with_suffix("").parts
        module = ".".join(parts[1:] if root == "app" else parts)
        package = module.split(".")[:-1]

        class Scan(ast.NodeVisitor):
            def __init__(self):
                self.scope, self.env = [], [{}]

            def resolve(self, n):
                if isinstance(n, ast.Name):
                    return next(
                        (d[n.id] for d in reversed(self.env) if n.id in d),
                        n.id,
                    )
                if isinstance(n, ast.Attribute):
                    return self.resolve(n.value) + "." + n.attr
                if isinstance(n, ast.Call):
                    return self.resolve(n.func) + "()"
                return "?"

            def visit_ImportFrom(self, n):
                base = (
                    ".".join(package[:len(package)-n.level+1]
                             + ([n.module] if n.module else []))
                    if n.level else (n.module or "")
                )
                for a in n.names:
                    self.env[-1][a.asname or a.name] = base + "." + a.name

            def visit_Import(self, n):
                for a in n.names:
                    self.env[-1][a.asname or a.name.split(".")[0]] = (
                        a.name if a.asname else a.name.split(".")[0]
                    )

            def body(self, n):
                self.env.append({
                    x.name: module + "." + x.name for x in n.body
                    if isinstance(x, (
                        ast.FunctionDef, ast.AsyncFunctionDef, ast.ClassDef
                    ))
                })
                for x in n.body:
                    self.visit(x)
                self.env.pop()

            def visit_Module(self, n):
                self.body(n)

            def visit_ClassDef(self, n):
                self.scope.append(n.name)
                self.body(n)
                self.scope.pop()

            def visit_FunctionDef(self, n):
                self.scope.append(n.name)
                self.body(n)
                self.scope.pop()

            visit_AsyncFunctionDef = visit_FunctionDef

            def visit_Call(self, n):
                name = self.resolve(n.func)
                scope = ".".join(self.scope) or "<module>"
                if name in WANTED:
                    calls.add((str(p), n.lineno, scope, name))
                if name.startswith((
                    "ponto.models.Ausencia.objects.",
                    "ferias.models.AgendamentoFerias.objects.",
                )) or name in {
                    "ponto.models.Ausencia",
                    "ferias.models.AgendamentoFerias",
                }:
                    models.add((str(p), n.lineno, scope, name))
                if module in ORIGINS:
                    for k in n.keywords:
                        if k.arg and k.arg.split("__")[0] in FIELDS:
                            fields.add((
                                str(p), k.value.lineno, scope, "KW", k.arg
                            ))
                self.generic_visit(n)

            def visit_Attribute(self, n):
                if module in ORIGINS and n.attr in FIELDS:
                    fields.add((
                        str(p), n.lineno,
                        ".".join(self.scope) or "<module>",
                        type(n.ctx).__name__, n.attr,
                    ))
                self.generic_visit(n)

        Scan().visit(tree)

print("FILES", files, "PARSE_ERRORS", len(errors),
      "CALLS", len(calls), "MODEL_CALLS", len(models),
      "FIELD_CANDIDATES", len(fields))
for e in errors:
    print("ERROR", *e)
for name in sorted(WANTED):
    sites = [
        f"{p}:{line}::{scope}"
        for p, line, scope, target in sorted(calls) if target == name
    ]
    print("CALL", name, " | ".join(sites) or "0")
print("MODEL_FILES")
for p, n in sorted(Counter(r[0] for r in models).items()):
    print(p, n)
print("FIELD_COUNTS")
for p in sorted({r[0] for r in fields}):
    print(p, dict(sorted(Counter(
        r[3] for r in fields if r[0] == p
    ).items())))
if "--detalhe" in sys.argv:
    for row in sorted(models):
        print("MODEL", *row)
    for row in sorted(fields):
        print("FIELD", *row)
```

Resultado da execução resumida: **881 arquivos, 0 erros de parse, 109 chamadas diretas aos alvos, 208 chamadas sintáticas de modelo/gerente e 728 candidatos de campos**. Esses totais são da execução em memória, sem relatório persistido no HEAD **[nao verificado]**.

| Arquivo | Load | Store | Keywords |
|---|---:|---:|---:|
| `app/ponto/services/ausencia.py` | 81 | 13 | 55 |
| `app/ponto/models.py` | 51 | 4 | 8 |
| `app/ponto/views.py` | 98 | 6 | 162 |
| `app/ferias/models.py` | 61 | 1 | 15 |
| `app/ferias/services.py` | 27 | 15 | 52 |
| `app/ferias/situacao.py` | 14 | 0 | 12 |
| `app/folha/export.py` | 32 | 0 | 21 |

Contagens sintáticas da execução, **[nao verificado]** como artefato no HEAD. `KW` pode ser filtro, construção ou argumento de serviço; `Store` pode ser só alteração em memória. O script não prova alcance por despacho dinâmico, aliases por atribuição, SQL, templates ou chamadas indiretas; estes exigem leitura complementar **[nao verificado]**.

**LEU — escritores e caminhos de entrada confirmados**

| Escritor | Campos/operação e quem chega |
|---|---|
| `criar_ausencia` | Criação geral; também procura rejeitada por pessoa/tipo/data e pode reabri-la. Chamadores: web lançar/submeter, API, espelho, veredito, IA e materializadores. `app/ponto/services/ausencia.py:130`, `:254`, `:291`, `:385`; `app/ponto/views.py:2415`, `:2536`; `app/api/views.py:1805`; `app/ponto/services/ausencia.py:1203`; `app/ponto/services/veredito_celula.py:337`; `app/ponto/services/checar_ausencia.py:46`; `app/chamados/services/materializacao.py:491`, `:527`. |
| `garantir_ausencia` | Delega ao criador; chamados atuais de regularização/abandono. `app/ponto/services/ausencia.py:461`; `app/ponto/services/regularizacao.py:118`; `app/ponto/management/commands/marcar_abandono_provisorio.py:76`. |
| `editar_ausencia_registro` | Escreve tipo, data, fim, dias, descrição e minutos; salva e invalida fechamento. Entradas web e comando de rederivação. `app/ponto/services/ausencia.py:498`, `:509`, `:524`; `app/ponto/views.py:2794`; `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:169`. |
| `mudar_estado` | Escreve estado/prazo e persiste derivados. Chamado por decisão, rejulgamento, anexo, escalonamento e reabertura. `app/ponto/services/ausencia.py:546`, `:825`, `:836`, `:875`, `:912`, `:1003`, `:1142`. |
| `decidir` / `_aplicar_periodo` | Estado, decisor, carimbo, descrição e horas parciais. Adaptador HTTP `_a4_decidir`. `app/ponto/services/ausencia.py:894`, `:912`, `:915`, `:918`, `:921`, `:928`; `app/ponto/views.py:85`. |
| `anexar_documento` | Documento; pode mudar estado. Web, reenvio API e API de documento. `app/ponto/services/ausencia.py:858`, `:860`, `:875`; `app/ponto/views.py:2151`; `app/api/views.py:1838`, `:3108`. |
| `reverter_rejeicao` / `_reabrir` | Documento, descrição, estado e prazo. Reversão HTTP e reabertura pelo criador. `app/ponto/views.py:2584`; `app/ponto/services/ausencia.py:291`, `:1103`, `:1129`, `:1139`, `:1142`, `:1147`. |
| `rejulgar_status_pelo_tipo` / escalonador | Estado/prazo; alcançam férias se estas entrarem nas seleções gerais. `app/ponto/services/ausencia.py:541`, `:546`, `:551`, `:993`, `:1003`; `app/ponto/management/commands/rejulgar_status_por_tipo.py:31`; `app/ponto/management/commands/escalonar_documentos_ausencia.py:26`. |
| `espelhar_ferias` | Cria férias diárias pelo criador; apaga selecionadas quando agendamento inativo. Chamado por `_sync_ausencia`. `app/ponto/services/ausencia.py:1189`, `:1191`, `:1203`; `app/ferias/models.py:392`. |
| Apagador/rejeitador de férias | Selecionam por pessoa/tipo/janela, sem origem. Entradas: `AgendamentoFerias.delete`, `_trocar_janela`, cancelamento administrativo. `app/ponto/services/ausencia.py:1263`, `:1274`, `:1284`; `app/ferias/models.py:455`; `app/ferias/services.py:445`, `:411`. |
| `rederivar_fim` / apagador de ensaio | Novo `save` geral ou exclusão do queryset recebido. `app/ponto/services/ausencia.py:1287`, `:1297`; `app/ponto/management/commands/curar_ausencia_sem_fim.py:46`; `app/ponto/management/commands/zerar_dados_operacionais.py:90`. |
| `Ausencia.save` | Carimba decisão e deriva minutos de falta, dias e fim. Não é guarda de proveniência. `app/ponto/models.py:872`, `:877`, `:894`, `:907`. |

As portas de **aprovar, rejeitar e cancelar**, individuais e em lote, chegam ao mesmo adaptador; editar período também. A reversão e a edição geral entram por outros serviços. Portanto, proteger somente `cancelar_ausencia` deixaria outros caminhos. **INFERIU** de `app/ponto/views.py:2290`, `:2319`, `:2351`, `:2643`, `:2673`, `:2701`, `:2734`, `:2584`, `:2794`.

Dois achados AST não significam produção efetiva de férias: os materializadores atuais recebem mapas de **falta/atestado**; a prévia normal da IA recusa férias. **LEU:** `app/chamados/models.py:3717`; `app/chamados/services/materializacao.py:475`, `:476`, `:603`; `app/chamados/services/disputa_emissao.py:1541`; `app/ponto/services/checar_ausencia.py:21`.

**LEU — leitores dos campos impactados**

| Campos/pergunta | Leitores e interpretação |
|---|---|
| Pessoa, tipo, data e fim | Cobertura do dia/período; distingue folha de cobrança. `app/ponto/turnos.py:2511`, `:2538`, `:2554`. |
| Estado | Folha exige aprovada; cobrança exclui rejeitada. `app/ponto/turnos.py:2517`, `:2519`. |
| Data/tipo/fim após escrita | Signals rejulgam células e reconciliam consumidores. `app/ponto/signals.py:123`, `:143`, `:150`, `:261`, `:271`. |
| Datas, dias, estado, documento e descrição | Histórico e painel. `app/ponto/services/dia_decidido.py:593`, `:615`, `:619`, `:624`; `app/ponto/views.py:2074`; `app/templates/ponto/partials/painel_ausencia.html:50`, `:57`, `:123`. |
| `documento_referencia` | Agrupamento documental, não identidade de agendamento. `app/ponto/views.py:2074`, `:2077`; `app/ponto/models.py:791`. |
| Agendamento: pessoa, datas e estado | `ferias_na_janela`, consumido por `em_gozo_hoje`, folha e disponibilidade. `app/ferias/services.py:47`, `:51`, `:63`; `app/folha/export.py:491`; `app/ponto/utils.py:198`. |
| Datas/estado do agendamento | Sincronização do espelho e classificação temporal. `app/ferias/models.py:273`, `:444`, `:450`. |
| FK nova | Não existe na definição de `Ausencia` lida; leitores/escritores futuros ainda não implementados. `app/ponto/models.py:743`, `:786`, `:791`, `:814`. |

O AST localizou consumidores das autoridades de cobertura em **API/espelho, calendário, grade, cartório, fechamento, efeito da ausência, chamados, vigias e relatórios**. Exemplos concretos: `app/api/views.py:1560`; `app/colaboradores/services/calendario.py:485`, `:533`; `app/escala/utils.py:1188`; `app/ponto/services/cartorio.py:1228`; `app/ponto/services/fechamento.py:683`; `app/ponto/services/efeito_ausencia.py:53`; `app/chamados/services/materializacao.py:79`; `app/ponto/services/vigia_ausencia.py:92`; `app/relatorios/views.py:205`, `:292`.

**Juízes versus derivações próprias**

- **Registrados no lookup:** cobertura por `ausencia_cobre/cobertura_ausencia_periodo`, abertura por `ABERTAS`, saldo/abono/vencimento e aviso. **Registro vivo:** acrescenta explicitamente efeito, veto, estado inicial e `em_gozo_hoje`. `app/docs/JUIZES.md:55`; `app/core/juizes.py:251`.
- **Derivação temporal na origem:** `status_por_data` se declara juiz do estado pelas datas; não está nomeado na entrada AUSENCIA/FERIAS consultada do lookup/registro. Não concluir daí autorização para criar outro. `app/ferias/models.py:273`; `app/docs/JUIZES.md:55`; `app/core/juizes.py:251`; `CLAUDE.md:74`.
- **Derivações próprias de identidade:** espelho trata presença na janela como sua; `_guard_sobreposicao` chama de `_minhas` todas as férias na janela antiga; apagador/rejeitador também deduzem propriedade pela janela. `app/ponto/services/ausencia.py:1183`, `:1200`, `:1267`, `:1282`; `app/ferias/models.py:422`, `:430`.
- **Derivação de apresentação:** painel calcula intervalo por irmãos documentais; histórico calcula duração do registro. Não demonstram período do agendamento. `app/templates/ponto/partials/painel_ausencia.html:50`; `app/ponto/services/dia_decidido.py:619`.
- **Leitor delegado correto:** férias com batida consulta `ferias_na_janela`; não necessita casar descrição de Ausencia. `app/folha/export.py:484`, `:491`.

O selo atual libera escritas pelo **arquivo da porta**, sem distinguir os escritores internos. Logo, não prova a exclusividade específica pedida pela HJ-091. **INFERIU** de `app/ponto/tests/test_chokepoint_ausencia_gate.py:29`, `:35`, `:65`; `app/ponto/services/ausencia.py:1203`.

**2. ESMERIL-SEMANTICA — decisões do construtor, sem decisão aqui**

| Campo/parâmetro | LEU / divergência | DECISÃO e opções de encaixe |
|---|---|---|
| Origem / nova FK | O espelho recebe um agendamento, mas reaproveita qualquer férias da pessoa no dia; `#N` é somente escrito. `app/ponto/services/ausencia.py:1183`, `:1200`, `:1207`. | Propagar identidade por parâmetro interno do produtor **ou** por caminho específico que preserve a porta. Não deduzir origem pela coincidência de janela. |
| `se_existir` / rejeitada | O criador pode retornar/reabrir por pessoa/tipo/data antes da criação. `app/ponto/services/ausencia.py:247`, `:254`, `:291`, `:297`. | Fazer a identidade atravessar essas buscas **ou** impedir que o caminho genérico alcance derivadas. RED: rejeitada de outro agendamento no mesmo dia. |
| Origem × unicidade | A unicidade viva é pessoa/tipo/data, sem agendamento. `app/ponto/models.py:827`. | Definir tradução da colisão pela porta: recusa com identificação/lista/Pauta existente. Acrescentar FK não autoriza duplicar férias vivas. |
| Estado de Ausencia × estado do agendamento | Espelho considera `agendado/em_gozo/concluido` ativo e pula inclusive Ausencia rejeitada; cobertura de folha exige aprovada. `app/ponto/services/ausencia.py:1181`, `:1200`; `app/ponto/turnos.py:2517`. | Separar presença, aprovação e origem nos contadores; escolher sua representação e denominadores. O passivo fornecido deve ser listado, sem reparo automático. Anexo `complemento_hj_091.txt:4`, `:6` **[nao verificado]**. |
| “Cancelar” × “rejeitar” × apagar | Tela cancela mediante rejeição; agendamento cancelado provoca exclusão das ausências selecionadas. `app/ponto/views.py:2734`; `app/ferias/services.py:411`, `:417`; `app/ponto/services/ausencia.py:1189`. | Centralizar operação por identidade no espelho **ou** manter auxiliar com origem explícita. Não igualar os vocabulários de estado. |
| Janela × propriedade / `exceto` | Limpeza usa data inicial da ausência dentro da janela; `exceto` preserva ambas as bordas da janela nova. `app/ponto/services/ausencia.py:1267`, `:1270`; `app/ferias/services.py:445`. | Acrescentar origem ao auxiliar **ou** delegar limpeza ao espelho. Linha deslocada fora da janela precisa ser denunciada; sobrevivência é **INFERIU** de `app/ponto/services/ausencia.py:1185`, `:1189`. |
| `_minhas` | A guarda exclui todas as férias da janela antiga como se fossem deste agendamento. `app/ferias/models.py:422`, `:430`, `:437`. | Alimentar o veto com IDs da FK **ou** mecanismo equivalente por identidade. Manter exclusão por janela conservaria a ambiguidade. |
| Data diária × período inteiro / documento | Espelho cria um dia, `dias_corridos=1`; painel reúne irmãos documentais. `app/ponto/services/ausencia.py:1204`; `app/ponto/views.py:2074`; `app/templates/ponto/partials/painel_ausencia.html:50`. | Definir apresentação do período pelo agendamento e do dia consultado separadamente; definir link específico ou abertura localizada na gestão. Não expandir o registro diário para produzir a aparência do período. |
| `data_fim=NULL` | Texto do campo sugere aberto; leitor distingue dia único de tipo em aberto. `app/ponto/models.py:751`; `app/ponto/turnos.py:3150`. | Texto específico de férias **ou** apresentação separada do registro e da origem; não interpretar NULL isoladamente. |
| Dias / inclusão | Agendamento e Ausencia usam datas inclusivas e dias corridos. Não li divergência de unidade nesta dupla. `app/ferias/models.py:380`; `app/ponto/models.py:899`; `app/ponto/services/ausencia.py:1196`. | Sem decisão de lei; preservar unidade e bordas na implementação. |
| Minutos | Espelho passa zero; duração prevista não deve ser fabricada para mostrar férias. `app/ponto/services/ausencia.py:1205`; `app/ponto/models.py:759`; `CLAUDE.md:148`. | Apresentar dias do agendamento; não converter em duração de jornada por conta própria. |
| Dia civil × dia do turno × instante | Folha cruza batida pela data local; ausência parcial noturna pertence ao início do turno; carimbo de aprovação é DateTime. `app/folha/export.py:496`; `app/ponto/models.py:767`, `:810`. | Declarar a unidade dos novos contadores como par agendamento/dia de ausência; não usar carimbo ou data civil da batida como substituto. |
| `hoje` / fuso | Modelos/serviços usam `date.today`; cron usa `timezone.localdate`. `app/ferias/model

[truncado em 20000 de 27724 caracteres]

## 13/11 18:16 · sessao 92e7cc643279 · RAIA-PREPARACAO · censo · origem=Code

**O diff antigo não pode ser retomado literalmente.** No HEAD, há três pontos adicionais: o leitor da ata transforma qualquer dispensa por justificativa em dia inteiro justificado; a cadeia temporal já vem do turno; e o abono persistido tem uma fonte diferente da soma parcial do PDF. (`app/escala/services/leitor_celula.py:81`, `app/escala/utils.py:1178`, `app/ponto/services/dia_pago.py:395`, `app/relatorios/pdf_espelho.py:479`)

LEU = código/documento aberto nesta cópia. INFERIU = consequência indicada das fontes. Hash, base `b264acbbc`, refs da raia e situação independente de produção: **[nao verificado]**. Os números raspados são evidência fornecida, sem confirmação independente: **[nao verificado]**.

**1. Censo AST: script e resultado**

Script executado por `python3 -B`, via entrada padrão, sem importar a aplicação. O instrumento é entregue abaixo, sem arquivo criado. Resultado instrumental, sem arquivo de prova: **[nao verificado]**.

```python
import ast
from pathlib import Path
from collections import defaultdict

FUNCS = set("""_marcar_ausencias marcos_cobertos_por_justificativa
minutos_do_periodo_justificado efeito_da_justificativa efeitos_da_ausencia
creditos_por_dia dispensar_por_ausencia por_dia_da_grade horas_reais_do_dia
periodo_da_parcial_no_dia encadear_marcos deslocamentos_na_cadeia
_justificado_lavrado""".split())
KEYS = set("""justificado justificativa_parcial marcos_justificados
min_justificados min_parcial min_parcial_pagos datas_justificadas
justificadas dispensada""".split())
STRUCT = set("""celulas marcos missing delta luz acesa tipo_dia orfas
minutos_previstos minutos_realizados cobertura_parcial periodo_parcial""".split())
STRUCT_FILES = {
    "app/escala/utils.py", "app/escala/services/leitor_celula.py",
    "app/ponto/turnos.py", "app/ponto/services/cartorio.py",
    "app/ponto/services/efeito_ausencia.py", "app/ponto/services/dia_pago.py",
    "app/ponto/services/fechamento.py", "app/relatorios/pdf_espelho.py",
    "app/ponto/motor_calculo_v2.py",
}
MODEL = set("""tipo status data_turno batida_entrada batida_entrada_id
saida_informada batida_gerada batida_gerada_id aprovada_em aprovada_por
criado_em atualizado_em chamado_origem""".split())
# Nomes locais conferidos por leitura; isto nao e inferencia global de tipos.
WRITERS = {
    "app/api/views.py": {"j"}, "app/ponto/views.py": {"j"},
    "app/ponto/services/regularizacao.py": {"j"},
    "app/ponto/services/justificativa.py": {"j"},
    "app/ponto/services/entrada_sincronizada.py": {"just"},
    "app/ponto/services/triagem_batida.py": {"just"},
    "app/chamados/reconciliador.py": {"_j"},
}
def string(n):
    return n.value if isinstance(n, ast.Constant) and isinstance(n.value, str) else None
def chain(n):
    if isinstance(n, ast.Name): return n.id
    if isinstance(n, ast.Attribute): return chain(n.value) + "." + n.attr
    if isinstance(n, ast.Call): return chain(n.func) + "()"
    return "?"
rows, errors, parsed, omitted = set(), [], 0, 0
for p in sorted(Path("app").rglob("*.py")):
    try: tree = ast.parse(p.read_text(encoding="utf-8"), filename=str(p))
    except (SyntaxError, UnicodeError) as e:
        errors.append((str(p), str(e))); continue
    parsed += 1
    if "tests" in p.parts or "migrations" in p.parts or p.name.startswith("test"):
        omitted += 1; continue
    active = KEYS | (STRUCT if str(p) in STRUCT_FILES else set())
    aliases = {a.asname or a.name: a.name for n in ast.walk(tree)
               if isinstance(n, ast.ImportFrom) for a in n.names}
    def add(key, mode, n):
        rows.add((key, mode, str(p), n.lineno))
    for n in ast.walk(tree):
        if isinstance(n, (ast.FunctionDef, ast.AsyncFunctionDef)) and n.name in FUNCS:
            add(n.name, "DEF", n)
        if isinstance(n, ast.Call):
            raw = n.func.id if isinstance(n.func, ast.Name) else getattr(n.func, "attr", "")
            f = aliases.get(raw, raw)
            if f in FUNCS: add(f, "CALL", n)
            if chain(n.func).startswith("Justificativa.objects."):
                add("Justificativa", "ORM:" + getattr(n.func, "attr", ""), n)
            if isinstance(n.func, ast.Attribute) and n.func.attr in {
                "get", "pop", "setdefault", "update", "values", "values_list"}:
                for a in n.args:
                    if string(a) in active: add(string(a), "KEY:" + n.func.attr, n)
            for kw in n.keywords:
                if (kw.arg or "").split("__")[0] in active:
                    add(kw.arg.split("__")[0], "KW:" + f, n)
        if isinstance(n, ast.Subscript) and string(n.slice) in active:
            add(string(n.slice), type(n.ctx).__name__, n)
        if isinstance(n, ast.Dict):
            for k in n.keys:
                if string(k) in active: add(string(k), "DICT", k)
        if isinstance(n, ast.Name) and n.id in active:
            add(n.id, type(n.ctx).__name__, n)
        if isinstance(n, ast.Attribute):
            if n.attr in active: add(n.attr, type(n.ctx).__name__, n)
            if (isinstance(n.ctx, ast.Store) and isinstance(n.value, ast.Name)
                    and n.value.id in WRITERS.get(str(p), set()) and n.attr in MODEL):
                add("Justificativa." + n.attr, "Store?", n)
group = defaultdict(lambda: defaultdict(list))
for key, mode, path, line in sorted(rows):
    group[key][path].append((line, mode))
print("PARSED", parsed, "OMITTED_TEST_MIGRATION", omitted,
      "ERRORS", errors, "ROWS", len(rows))
for key in sorted(FUNCS | KEYS | set(group)):
    print(key)
    for p, entries in sorted(group[key].items()):
        print(" ", p, " ".join(f"{m}:{l}" for l, m in sorted(entries)))
```

Saída: `PARSED 3369 OMITTED_TEST_MIGRATION 2608 ERRORS [] ROWS 640`. Houve um `SyntaxWarning` no teste `app/ponto/tests/test_tela_gestao_he_calendario.py:645`, sem erro de parse. Contagens da execução inline: **[nao verificado]**.

Limite: são ocorrências sintáticas, não 640 leitores/escritores semanticamente distintos. `DICT` pode ser modelo/rótulo; `Store?` depende dos nomes locais conferidos; aliases ORM, chamadas dinâmicas, templates e identidade interprocedural completa não são resolvidos. Completude fora desse contrato: **[nao verificado]**.

Resultado consolidado das funções:

| Função | Definição → chamadas encontradas |
|---|---|
| `_marcar_ausencias` | `app/escala/utils.py:1156` → mesmo arquivo: `807`, `1008`, `1848` |
| `encadear_marcos` | `app/escala/utils.py:330` → mesmo arquivo: `363`, `427`, `773`, `1619` |
| `deslocamentos_na_cadeia` | `app/escala/utils.py:366` → mesmo arquivo: `787`, `1752` |
| `horas_reais_do_dia` | `app/ponto/turnos.py:2646` → `app/escala/utils.py:1234` |
| `periodo_da_parcial_no_dia` | `app/ponto/turnos.py:2616` → `app/escala/utils.py:1234`, `app/escala/utils.py:1249`, `app/chamados/services/materializacao.py:83` |
| `dispensar_por_ausencia` | `app/ponto/services/cartorio.py:781` → mesmo arquivo: `1251`, `1437`, `1851` |
| `_justificado_lavrado` | `app/escala/services/leitor_celula.py:71` → mesmo arquivo: `439` |
| `efeitos_da_ausencia` | `app/ponto/services/efeito_ausencia.py:28` → `app/ponto/services/espelho.py:1068`, `app/relatorios/pdf_espelho.py:333`, instrumento `app/ponto/management/commands/diff_espelho_alimentacao.py:116` |
| `creditos_por_dia` | `app/ponto/services/credito_parcial.py:15` → `app/ponto/services/fechamento.py:999` |
| `por_dia_da_grade` | `app/ponto/services/dia_pago.py:364` → `app/ponto/services/fechamento.py:1061`, `app/ponto/services/dia_pago.py:464`, `app/folha/porta_export.py:139` |

O script não encontrou definições/chamadas de `marcos_cobertos_por_justificativa`, `minutos_do_periodo_justificado`, `efeito_da_justificativa`, nem ocorrências das chaves `marcos_justificados`, `min_justificados`, `justificativa_parcial`, no universo de produção examinado. Resultado negativo do instrumento: **[nao verificado: sem arquivo de saída]**. O nascimento autorizado do primeiro está escrito em `app/docs/CORTES.md:320`.

LEU — escritores e leitores do modelo:

| Campo | Escritores encontrados/conferidos | Leitores relevantes à obra |
|---|---|---|
| Criação, `data_turno`, `tipo`, âncoras | API `app/api/views.py:1992`; web `app/ponto/views.py:1635`; regularização `app/ponto/services/regularizacao.py:91`; cron `app/ponto/management/commands/processar_alertas_turno.py:394` | Grade `app/escala/utils.py:1197`; efeito `app/ponto/services/efeito_ausencia.py:41`; fechamento `app/ponto/services/fechamento.py:661` |
| Alteração de `tipo` | `app/api/views.py:1986`; `app/ponto/views.py:1625` | Os três leitores financeiros acima consultam aprovação/data e não selecionam o tipo. (`app/escala/utils.py:1199`, `app/ponto/services/efeito_ausencia.py:44`, `app/ponto/services/fechamento.py:664`) |
| `batida_entrada` | Criações acima; alteração em `app/ponto/views.py:1630` | Aprovação/materialização `app/ponto/services/justificativa.py:155`; bloqueio de órfão `app/ponto/utils_turno_orfao.py:53` |
| `saida_informada` | `app/ponto/views.py:1628`; `app/ponto/services/regularizacao.py:103`; `app/ponto/services/justificativa.py:382` | Validação e registro da saída `app/ponto/services/justificativa.py:179` |
| `batida_gerada` | `app/ponto/services/justificativa.py:204`, `381`; lote `app/ponto/views.py:1831` | Guarda de idempotência `app/ponto/services/justificativa.py:184` |
| `status` | Aprovação/rejeição/materialização `app/ponto/services/justificativa.py:205`, `283`, `383`; lote `app/ponto/views.py:1796`, `1875`; regularização `app/ponto/services/regularizacao.py:105`; autorrejeições `app/ponto/services/entrada_sincronizada.py:41`, `app/ponto/services/triagem_batida.py:608`, `app/chamados/reconciliador.py:533` | Grade/efeito/fechamento acima; SLA `app/ponto/management/commands/alertar_justificativas_sla.py:19` |
| `aprovada_por`, `aprovada_em` | `app/ponto/services/justificativa.py:206`, `284`, `384`; lote `app/ponto/views.py:1797`, `1876` | Carimbos de fato novo `app/ponto/services/cartorio.py:387` |
| `chamado_origem` | `app/api/views.py:1989`; `app/ponto/views.py:1632`; criação da regularização `app/ponto/services/regularizacao.py:91` | Encerramento pela origem `app/ponto/services/justificativa.py:217` |
| `criado_em`, `atualizado_em` | Automáticos no modelo `app/ponto/models.py:179` | Fato novo `app/ponto/services/cartorio.py:387` |

Outros leitores ORM encontrados: histórico em `app/chamados/views.py:596`; API em `app/api/views.py:2088`; contadores em `app/core/views.py:205`, `244`, `1191`; telas em `app/ponto/views.py:998`, `1673`, `1704`, `1736`, `1738`, `1742`; instrumento em `app/relatorios/management/commands/diff_extrato_parcial.py:43`. O comando existente de limpeza também referencia o modelo; foi apenas lido: `app/ponto/management/commands/zerar_dados_operacionais.py:44`, `95`.

LEU — estado derivado e dinheiro:

| Campo | Escritor/origem | Leitores/reescritores |
|---|---|---|
| `celulas`, `missing`, `delta` | Casamento em `app/escala/utils.py:585`; delta absoluto em `app/escala/utils.py:325`; overlay da justificativa em `app/escala/utils.py:1256` | Ata `app/ponto/services/cartorio.py:721`; reconstrução `app/escala/services/leitor_celula.py:121`; horas reais `app/ponto/turnos.py:2660` |
| `justificado` | Grade escreve `True` em `app/escala/utils.py:1259` | Cartório `app/ponto/services/cartorio.py:802`; reconstrução escreve `True` em `app/escala/services/leitor_celula.py:440`; supra retira `FURO_*` em `app/ponto/supra_juiz.py:371` |
| `acesa`, `luz`, `dispensada` | Ata em `app/ponto/services/cartorio.py:711`, `722`; dispensa em `app/ponto/services/cartorio.py:808` | Leitor `app/escala/services/leitor_celula.py:119`, `145`; lastro `app/ponto/services/lastro.py:336`; status `app/ponto/status_dia.py:187`; supra `app/ponto/supra_juiz.py:312`; furos `app/relatorios/furos.py:32` |
| `cobertura_parcial`, `periodo_parcial` | Grade `app/escala/utils.py:1230`, `1234`; ata transporta em `app/ponto/services/cartorio.py:766` | Leitor transporta em `app/escala/services/leitor_celula.py:433`; guarda antirretroativo `app/ponto/services/cartorio.py:424` |
| `justificadas` / `datas_justificadas` | Efeito consulta datas em `app/ponto/services/efeito_ausencia.py:40`; fechamento repete consulta em `app/ponto/services/fechamento.py:660` | PDF `app/relatorios/pdf_espelho.py:334`; espelho `app/ponto/services/espelho.py:1114`; motor `app/ponto/motor_calculo_v2.py:796`, `2602`, `2608`, `3466` |
| `min_parcial` / `min_parcial_pagos` | Soma somente Ausência em `app/ponto/services/efeito_ausencia.py:46`, `50`, `64` | Leitura da chave em `app/relatorios/pdf_espelho.py:336`; soma em horas em `app/relatorios/pdf_espelho.py:479` |
| Crédito diário de atraso | `app/ponto/services/credito_parcial.py:15` | Fechamento aplica em `app/ponto/services/fechamento.py:999`; abatimento em `app/ponto/services/credito_parcial.py:50` |
| Abono persistido | `app/ponto/services/dia_pago.py:395`; fechamento grava em `app/ponto/services/fechamento.py:1062` | Resumo da competência lê em `app/relatorios/cartao_pela_celula.py:118`, `186`; retrato exportado chama a fonte em `app/ponto/services/dia_pago.py:464` |

**Juízes registrados versus derivações próprias — LEU:**

- Turno/marcos, casamento, cálculo/CCT e janela estão no lookup: `app/docs/JUIZES.md:20`, `22`, `27`, `29`, `35`. Cobertura de Ausência está em `app/docs/JUIZES.md:55`; período parcial está no registro vivo em `app/core/juizes.py:261`.
- `marcos_cobertos_por_justificativa` tem corte de nascimento, mas não está no bloco vivo de Ausência lido nesta cópia. (`app/docs/CORTES.md:320`, `app/core/juizes.py:251`)
- Derivam por conta própria: a grade considera qualquer data aprovada suficiente para limpar o dia; efeito e fechamento repetem a seleção de datas; o leitor da ata deriva dia inteiro por `any(dispensada)`; `_delta_min` recalcula por HH:MM; o PDF acrescenta localmente o parcial ao abono. (`app/escala/utils.py:1197`, `1256`; `app/ponto/services/efeito_ausencia.py:40`; `app/ponto/services/fechamento.py:660`; `app/escala/services/leitor_celula.py:32`, `81`; `app/relatorios/pdf_espelho.py:479`)
- `por_dia_da_grade` é fonte compartilhada de abono/previsto/realizado; seus chamadores não repetem essa conta. Isso está declarado e implementado em `app/ponto/services/dia_pago.py:364`, `395`, com consumidores em `app/ponto/services/fechamento.py:1061` e `app/folha/porta_export.py:139`.

**2. ESMERIL-SEMANTICA: decisões para o construtor**

As opções abaixo são alternativas de contrato/encaixe; nenhuma foi escolhida neste censo.

| Campo/parâmetro | LEU: significados divergentes | DECISÃO a explicitar |
|---|---|---|
| `data_turno` | Modelo diz dia do turno; web procura entrada pelo dia civil; regularização usa data local da entrada; API recebe payload. (`app/ponto/models.py:144`, `app/ponto/views.py:1598`, `app/ponto/services/regularizacao.py:73`, `app/api/views.py:1948`) | Transportar identidade do turno existente ou converter a data na fronteira usando o juiz. A regra do dia de início já está respondida por L-085. (`app/docs/LEIS.md:117`) |
| `tipo` e papel E/S | Tipo da justificativa nomeia problema; papel da batida deve vir do marco, não de `Batida.tipo`. (`app/ponto/models.py:116`, `app/docs/LEIS.md:119`) | Representar cobertura por instância do marco ou por chave estável do marco; preservar o papel dado pela autoridade. |
| `batida_entrada`, `chamado_origem` | Modelo vincula ao turno/episódio; o diff fornecido reduz a `{data: tipos}`. (`app/ponto/models.py:139`, `173`; algoritmo do anexo **[nao verificado no HEAD]**) | Alcance por dia/tipo ou por registro ancorado: lacuna 1 abaixo. |
| `saida_informada`, `batida_gerada`, `status` | Aprovação pode materializar uma saída real, enquanto a cobertura pretendida dispensa marco sem batida. (`app/ponto/services/justificativa.py:179`, `204`, `205`; `app/ponto/services/cartorio.py:808`) | Calcular efeito após o estado materializado ou excluir explicitamente o trecho já realizado; a representação precisa impedir pagamento duplicado. |
| `marcos`, índices e deslocamentos | Régua de colunas não é ordem cronológica do turno noturno; o diff reencadeia a régua. (`app/escala/utils.py:353`, `366`; algoritmo do anexo **[nao verificado no HEAD]**) | Carregar cadeia/instâncias ou deslocamentos já fornecidos pelo builder. Reencadear colunas restaura o defeito documentado. (`app/ponto/turnos.py:2655`) |
| `delta` | Grade: diferença de instantes absolutos, arredondada; leitor: menor distância circular de HH:MM, limitada por ±720. (`app/escala/utils.py:325`, `app/escala/services/leitor_celula.py:32`) | Transportar delta/proveniência lavrada ou restringir e verificar formalmente o domínio da reconstrução. Divergência efetiva na frota: **[nao verificado]**. |
| `None`, `missing`, `dispensada`, `luz` | `None` pode significar coluna fora do jogo; dispensa tem `acesa=True`, `luz=None`; batida real tem hora. (`app/ponto/services/cartorio.py:719`, `786`; `app/escala/services/leitor_celula.py:145`) | Cobertura explícita por marco ou marcador equivalente já transportável; não usar “acesa” como prova de batida. |
| `justificado` | Grade atual significa dia todo; leitor reconstruído usa qualquer dispensa. (`app/escala/utils.py:1259`, `app/escala/services/leitor_celula.py:81`) | Carimbo integral explícito ou composição integral na autoridade. **INFERIU:** dispensar um único marco com o leitor atual calaria o furo residual. (`app/escala/services/leitor_celula.py:440`, `app/ponto/supra_juiz.py:371`) |
| `min_justificados` / `min_parcial_pagos` | Raia propõe duração de período; HEAD fornece escalar de minutos de Ausência para o PDF. (anexo **[nao verificado no HEAD]**; `app/ponto/services/efeito_ausencia.py:64`) | Mapa por dia com período/proveniência ou mapa diário de minutos com perdas de informação declaradas; escalar mensal sozinho não identifica o dia do DSR. |
| Parcial pago versus crédito de atraso | Crédito abate atraso apenas do próprio dia; PDF trata total como abono. (`app/ponto/services/credito_parcial.py:50`, `app/relatorios/pdf_espelho.py:479`) | Compor efeitos distintos a partir da mesma cobertura ou ampliar o contrato diário existente; não equiparar implicitamente período pago a atraso. |
| `justificadas` versus abono monetário | Set de datas afasta falta inteira; motor só abona dia sem trabalho; folha abona complemento em dia de Ausência. (`app/ponto/motor_calculo_v2.py:2600`, `2606`; `app/ponto/services/dia_pago.py:395`) | Entrada comum que distinga integral/parcial ou canais explícitos consumindo a mesma origem. Acrescentar uma data ao set não representa duração parcial. |
| Minutos versus horas | Grade/fechamento usam minutos; motor/PDF expõem horas com `/60`. (`app/ponto/services/dia_pago.py:367`, `app/ponto/motor_calculo_v2.py:2618`, `app/relatorios/pdf_espelho.py:479`) | Fixar a conversão numa fronteira ou manter tipos/unidades explícitos em cada contrato; medir arredondamento. O recorte fornecido não contém segundos da batida 139122. **[nao verificado]** |
| Zero versus período ilegível | HEAD admite `None`/`nao_sei`; raia devolve zero quando não consegue ler delta. (`app/ponto/turnos.py:2624`, `2663`; anexo **[nao verificado no HEAD]**) | Resultado com valor e estado de conhecimento ou recusa explícita de apuração; zero não pode esconder ausência de proveniência. |
| Fuso e dia do DSR/crédito | Banco UTC/apresentação UTC-3; DSR base usa dia da jornada, override comercial e crédito usam data local da entrada. (`CLAUDE.md:146`, `app/ponto/motor_calculo_v2.py:2577`, `3465`, `app/ponto/services/credito_parcial.py:36`) | Transportar a chave soberana ou convertê-la na fronteira. Dia da jornada já está definido; não é pergunta de lei. (`app/docs/LEIS.md:117`) |
| Bordas temporais | Parcial usa `[ini,fim]` para pertinência; fato novo usa `(desde,até]`; batidas da sonda interna usam `[D−1,D+2)`; janelas de datas financeiras usam limites inclusivos. (`app/ponto/turnos.py:2641`, `app/ponto/services/cartorio.py:376`, `380`, `app/ponto/services/efeito_ausencia.py:43`) | Documentar separadamente pertinência de p

[truncado em 20000 de 27756 caracteres]

## 13/11 18:15 · sessao 89abfd41e1c4 · RAIA-PREPARACAO · censo · origem=Code

**perguntas de lei: 1 — alcance temporal da medida de 24 horas.** O contrato define início/fim das corridas, mas não escolhe qual deles determina a pertença à janela. (`app/docs/RELATO.md:1648`)

**LEU:** OTIMIZA-1 permanece interrompida; o complemento exige pós-E3, DIFF do vigia zero e instrumento RSS separado. A preparação autoriza somente leitura/desenho. (`app/docs/BACKLOG.md:2083`; `app/docs/BACKLOG.md:738`; `app/docs/BACKLOG.md:739`)

Nesta resposta, **HEAD** significa os arquivos disponíveis nesta cópia. `.git` não está presente; identidade do commit, base `4d5fd8df6` e blobs integrais da raia: **[nao verificado]**. A construção histórica está documentada, sem comprovar implementação nesta cópia. (`app/docs/RELATO.md:1645`)

**1. Censo AST: script e resultado**

Script executado em memória, sem importar o app. `W` identifica construção/atribuição sintática; não prova escrita persistente. Importações relativas, referências por strings, despacho dinâmico e identidade dos objetos exigem conferência manual. **[nao verificado: completude para chamadas dinâmicas]**

```python
import ast, sys
from pathlib import Path
from collections import defaultdict

F = set("""
bin.gerar_estado.gerar
chamados.management.commands.vigiar_fio_morto._carimbar
chamados.management.commands.vigiar_fio_morto.caminho_do_carimbo
chamados.services.vigia_fio.censo_alcance
chamados.services.tarefa_do_dia.carga_da_tarefa
chamados.services.tarefa_do_dia.tarefa_do_dia
chamados.juizes.alimentar_portas
chamados.juizes.alimentar_fila
chamados.juizes.porta_alcancavel_do_dia
""".split())
K = set("""
medido_em hoje esperado censo alcance gritou alarma_em_0
fila_ids fila_cel verbo vivas instancias no_app
_pares _portas agora ini fim pico_rss_mb inteiras_por_pouso
""".split())
P = set("""
bin/fio_estado.py
app/core/management/commands/pipeline_placar.py
app/chamados/management/commands/vigiar_fio_morto.py
app/chamados/services/vigia_fio.py
app/chamados/services/tarefa_do_dia.py
app/chamados/juizes.py
""".split())
A = """
.git bin/pista_estado.py bin/suite_impacto.py
bin/suite_impacto.sh bin/suite_impacto
""".split()

out = defaultdict(set)
errors = []
total = 0
for p in sorted(Path(".").rglob("*.py")):
    rel = p.as_posix()
    mod = rel[:-3].replace("/", ".")
    if mod.startswith("app."):
        mod = mod[4:]
    try:
        tree = ast.parse(p.read_text(encoding="utf-8"), filename=rel)
    except (OSError, UnicodeError, SyntaxError) as e:
        errors.append((rel, type(e).__name__))
        continue
    total += 1
    aliases = {}
    for n in ast.walk(tree):
        if isinstance(n, ast.ImportFrom) and n.module:
            for a in n.names:
                aliases[a.asname or a.name] = n.module + "." + a.name
        elif isinstance(n, ast.Import):
            for a in n.names:
                aliases[a.asname or a.name.split(".")[0]] = (
                    a.name if a.asname else a.name.split(".")[0]
                )

    def resolve(n):
        if isinstance(n, ast.Name):
            return aliases.get(n.id, mod + "." + n.id)
        if isinstance(n, ast.Attribute):
            return resolve(n.value) + "." + n.attr
        return ""

    def add(kind, key, n):
        out[(kind, key)].add(f"{rel}:{n.lineno}")

    for n in ast.walk(tree):
        if isinstance(n, (ast.FunctionDef, ast.AsyncFunctionDef)):
            if mod + "." + n.name in F:
                add("DEF", mod + "." + n.name, n)
        if isinstance(n, ast.Call) and resolve(n.func) in F:
            add("CALL", resolve(n.func), n)
        if rel not in P:
            continue
        if isinstance(n, ast.Dict):
            for k in n.keys:
                if isinstance(k, ast.Constant) and k.value in K:
                    add("W", k.value, k)
        if isinstance(n, ast.keyword) and n.arg in K:
            add("KW", n.arg, n)
        if isinstance(n, ast.arg) and n.arg in K:
            add("PARAM", n.arg, n)
        if isinstance(n, ast.Attribute) and n.attr in K:
            add("R" if isinstance(n.ctx, ast.Load) else "W", n.attr, n)
        if isinstance(n, ast.Subscript):
            if isinstance(n.slice, ast.Constant) and n.slice.value in K:
                add("R" if isinstance(n.ctx, ast.Load) else "W",
                    n.slice.value, n)
        if isinstance(n, ast.Call) and isinstance(n.func, ast.Attribute):
            if n.func.attr in ("get", "pop", "setdefault") and n.args:
                k = n.args[0]
                if isinstance(k, ast.Constant) and k.value in K:
                    add(n.func.attr, k.value, n)

print("PARSED", total, "ERRORS", errors)
for a in A:
    print("EXISTS", a, Path(a).exists())
for (kind, key), sites in sorted(out.items()):
    if kind in ("DEF", "CALL") or "--sites" in sys.argv:
        print(kind, key, "=>", "; ".join(sorted(sites)))
for k in sorted(K):
    print("FIELD", k, {
        t: len(out[(t, k)])
        for t in ("W", "R", "get", "pop", "setdefault", "KW", "PARAM")
        if out[(t, k)]
    })
```

Resultado operacional: `PARSED 3574 ERRORS []`; os cinco caminhos de `A` retornaram `False`. As chaves candidatas `pico_rss_mb` e `inteiras_por_pouso` não apareceram no recorte `P`. **[nao verificado: saída transitória, sem arquivo gravado pela restrição de leitura]**

Chamadas encontradas fora dos testes — os sítios abaixo foram conferidos no código: [nao verificado]

| Função | Definição | Chamadores |
|---|---|---|
| `gerar` | `bin/gerar_estado.py:178` | `bin/gerar_estado.py:396` |
| `_carimbar` | `app/chamados/management/commands/vigiar_fio_morto.py:120` | Mesmo arquivo, `:232` |
| `caminho_do_carimbo` | `app/chamados/management/commands/vigiar_fio_morto.py:107` | Mesmo arquivo, `:132`; `app/core/management/commands/pipeline_placar.py:92` |
| `censo_alcance` | `app/chamados/services/vigia_fio.py:542` | `app/chamados/management/commands/vigiar_fio_morto.py:178`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:78` |
| `carga_da_tarefa` | `app/chamados/services/tarefa_do_dia.py:557` | Mesmo arquivo, `:569`; `app/chamados/management/commands/diff_e3_sombra.py:115` |
| `tarefa_do_dia` | `app/chamados/services/tarefa_do_dia.py:562` | `app/chamados/management/commands/diff_e3_sombra.py:125`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:87` |
| `alimentar_fila` | `app/chamados/juizes.py:1531` | Mesmo arquivo, `:1576`; `app/chamados/services/tarefa_do_dia.py:529` |
| `alimentar_portas` | `app/chamados/juizes.py:1552` | Mesmo arquivo, `:1656`; `app/chamados/services/vigia_fio.py:609`; `app/chamados/management/commands/diff_e3_sombra.py:114`; `app/escala/services/cadastro_realidade.py:456`; `app/folha/fora_da_folha.py:62` |
| `porta_alcancavel_do_dia` | `app/chamados/juizes.py:1622` | `app/chamados/services/vigia_fio.py:640`; `app/escala/services/cadastro_realidade.py:460`; `app/folha/fora_da_folha.py:67` |

Os testes também aparecem na saída do script: carga/tarefa, portas, alcance, carimbo e placar têm consumidores de teste existentes. Exemplos: `app/chamados/tests/test_e2_tarefa_do_dia.py:443`; `app/chamados/tests/test_e3_portas_tarefa.py:120`; `app/chamados/tests/test_o261_vigia_fio.py:895`; `app/core/tests/test_fio_mudo_placar.py:65`.

**LEU — escritores e leitores dos dados:**

| Dados | Escritor/origem | Leitores e distinções |
|---|---|---|
| `inicio`, `fim`, `quem`, `escopo`, `rc` da pista | O contrato atribui uma linha por run à trava. O corpo HEAD termina executando `"$@"`, sem essa emissão. | Leitor previsto: `pista_estado.ler/linha` e seção no gerador. É contrato/proposta, não implementação comprovada no HEAD. (`app/docs/RELATO.md:1648`; `bin/trava_teste.sh:42`; `bin/gerar_estado.py:265`) |
| `ESTEIRA_QUEM`, `ONLY`, `LABELS` | Chamadores fornecem identidade e seleção; a trava acrescenta PID a `QUEM`. | Trava lê identidade; executores leem seleção. Shell foi conferido diretamente, **não por AST Python**. (`bin/suite.sh:226`; `bin/suite.sh:232`; `bin/regua.sh:165`; `bin/pre-push.sh:142`; `bin/trava_teste.sh:23`) |
| `REGUA_*` | Régua grava impressão, árvore, resultado e instante. | Pre-push lê o carimbo e confere correspondência com o push. Campos adicionais de certificado do anexo: **[nao verificado no HEAD]**. (`bin/regua.sh:196`; `bin/pre-push.sh:65`; `bin/pre-push.sh:68`) |
| `medido_em`, `hoje`, `esperado`, `censo`, `alcance`, alarmes | `handle` monta o registro; `_carimbar` serializa com troca atômica. | ESTADO lê tempo, contadores e esperados; placar lê o mesmo caminho e contadores nomeados. (`app/chamados/management/commands/vigiar_fio_morto.py:232`; `:137`; `bin/fio_estado.py:121`; `app/core/management/commands/pipeline_placar.py:92`) |
| `ini`, `fim`, `agora`, `_pares`, `_portas` | Base recebe limites; carga recebe instante, materializa pares e carrega portas sob demanda. | `tarefa` usa janela fechada e instante da carga. (`app/ponto/turnos.py:2976`; `app/chamados/services/tarefa_do_dia.py:367`; `:420`; `:427`; `:529`; `:517`) |
| `fila_ids`, `fila_cel`, `verbo` | `alimentar_fila` monta a alimentação. | Censo lê pertença global; tarefa lê pertença, verbos e recorte da célula. (`app/chamados/juizes.py:1549`; `app/chamados/services/vigia_fio.py:610`; `app/chamados/services/tarefa_do_dia.py:546`) |
| `vivas`, `instancias`, `no_app` | `alimentar_portas` monta vivas/mapa; `_no_app` preenche cache. | Censo usa vivas, identidade compartilhada e app. Isso não equivale à seleção de perguntas da carga. (`app/chamados/juizes.py:1580`; `:1585`; `:1593`; `app/chamados/services/vigia_fio.py:615`; `:620`; `:625`) |
| RSS e razão inteiras/pouso | Nenhum escritor desses novos dados foi comprovado nos corpos HEAD examinados. | RSS pertence ao complemento; a razão pertence ao contrato da pista. (`app/chamados/management/commands/vigiar_fio_morto.py:120`; `app/docs/BACKLOG.md:738`; `app/docs/RELATO.md:1648`) |

**Juízes versus derivações:**

- **Registrado no lookup:** `tarefa_do_dia`, puro, relógio `agora`, lote `CargaDaTarefa`. Também consta no registro vivo. (`app/docs/JUIZES.md:78`; `app/docs/JUIZES.md:84`; `app/core/juizes.py:295`)
- **Registrado no vivo:** `porta_alcancavel_do_dia`, pergunta C17. Sua autoridade consta expressamente em `core/juizes.py`; não atribuo sua inscrição ao trecho do lookup que descreve C19. (`app/core/juizes.py:132`; `app/core/juizes.py:293`)
- **Alimentadores:** `alimentar_fila` e `alimentar_portas` carregam fontes dos juízes; não são autorização para substituir uma pergunta por outra. (`app/chamados/juizes.py:1532`; `app/chamados/juizes.py:1556`)
- **Derivações próprias de instrumento:** censo categoriza alcance; leitor do ESTADO calcula idade e divergência dos esperados; placar compara contadores. `_carimbar` escreve fotografia. (`app/chamados/services/vigia_fio.py:630`; `bin/fio_estado.py:125`; `bin/fio_estado.py:132`; `app/core/management/commands/pipeline_placar.py:105`; `app/chamados/management/commands/vigiar_fio_morto.py:21`)
- **Pista:** o contrato dá ao Git a autoridade dos pousos e ao leitor a agregação da medida. “Juiz da seleção”, escrito no anexo, não comprova registro em JUIZES. (`app/docs/RELATO.md:1648`; **[nao verificado: registro do seletor da raia]**)

**2. ESMERIL-SEMANTICA — decisões para o construtor, sem escolher**

Nas linhas referentes ao patch, **LEU anexo** significa precisamente **[nao verificado no HEAD]**.

| Campo/parâmetro | Divergência ou concordância lida | DECISÃO / opções e sítios |
|---|---|---|
| `escopo` | Anexo identifica recorte pela presença de `ONLY`. HEAD reconhece inteira pela cobertura efetiva da seleção, inclusive com `--only`. | Transportar o resultado do reconhecimento existente ou uma descrição explícita equivalente; não criar outro classificador. (`bin/suite.sh:155`; `bin/suite.sh:173`; anexo **[nao verificado no HEAD]**) |
| `modo`, `--motivo`, `--cobertura` | `_porta` anexada chama inteiras sem motivo. HEAD exige motivo e possui interface posterior à base da raia. | Adaptar os chamadores preservando a guarda; definir encaixe compatível da cobertura. Remover a guarda contraria lei escrita. (`CLAUDE.md:101`; `bin/suite.sh:53`; `bin/suite.sh:173`; anexo **[nao verificado no HEAD]**) |
| `quem`, `inteira`, `evitada` | `_teste_irmao` atende CI e control-plane; este último roda mesmo quando o arsenal é pulado. | Manter total de todos os runs e explicitar suas partições; escolher apresentação que não faça “evitada” significar ausência de qualquer execução. (`bin/pre-push.sh:142`; `:159`; `:186`; `:190`; `:198`; `app/docs/RELATO.md:1648`) |
| `inicio`, `fim`, janela | Anexo grava ao terminar, mas seleciona pelo início; corrida ainda viva não aparece. | Início, fim ou sobreposição; explicitar tratamento da corrida em andamento e bordas. É a pergunta de alcance nº 1. (`app/docs/RELATO.md:1648`; anexo **[nao verificado no HEAD]**) |
| `desde`, `agora`, fuso | Anexo usa datas ingênuas, somente limite inferior e Git sem offset explícito. | Definir representação comum de instante e limites; a apresentação UTC−3 já está escrita. Fuso real dos processos e pertença temporal dos merges: **[nao verificado]**. (`CLAUDE.md:146`; `app/docs/RELATO.md:1648`; anexo **[nao verificado no HEAD]**) |
| `pousos` | Anexo converte Git ilegível em `None`, mas apresenta isso como “sem pouso”, igual a zero. | Estado explícito por fonte ou propagação de indisponibilidade. A ausência de sinal não permite afirmar zero. (`CLAUDE.md:35`; `bin/gerar_estado.py:258`; anexo **[nao verificado no HEAD]**) |
| `runs`, `evitadas`, `escapou`, `suspensa` | Anexo descarta linhas inválidas e converte medida auxiliar ilegível em lista vazia. Suspensão é presença atual de arquivo, enquanto os contadores são de janela. | Distinguir ausência, parcialidade, zero observado e estado atual; registrar descartes ou invalidar a fonte afetada. (`app/docs/RELATO.md:1648`; `CLAUDE.md:192`; anexo **[nao verificado no HEAD]**) |
| `inteiras_por_pouso`, `meta` | Anexo arredonda antes de comparar; uma razão ligeiramente superior pode parecer dentro de `1,5`. | Comparação crua com exibição arredondada, ou mudança explícita do contrato se o responsável quiser tolerância. Não há tolerância escrita. (`app/docs/RELATO.md:1648`; anexo **[nao verificado no HEAD]**) |
| `rc`, “run” | Contrato pede rc real; trava retorna 75 antes de executar quando não obtém vez. Docker pode falhar depois de obter a trava. | Explicitar na apresentação “comando executado” versus “testes iniciados”, preservando rc e a contagem contratada. (`app/docs/RELATO.md:1648`; `bin/trava_teste.sh:36`; `:38`; `:42`; `bin/pre-push.sh:148`) |
| RSS, `esperado`, operador | Complemento pede `<=1500`; ESTADO compara por igualdade e comando alarma por valor verdadeiro do contador. | Bloco de memória com operador explícito, ou representação de violação com esperado zero; avaliar leitores sem mudar a igualdade dos contadores existentes. (`app/docs/BACKLOG.md:738`; `bin/fio_estado.py:125`; `app/chamados/management/commands/vigiar_fio_morto.py:187`) |
| RSS, unidade e período | `_carimbar` só serializa; coleta de pico não está implementada ali. O aval escolhe rodada/próprio processo. | Contratar fonte, conversão MB/MiB, início/fim da acumulação e falha de coleta. Reduzir retenção não comprova redução do pico já ocorrido. (`app/chamados/management/commands/vigiar_fio_morto.py:120`; `app/docs/BACKLOG.md:738`; `app/docs/RELATO.md:14`) |
| `carga`, último leitor | HEAD `:176` abre tranca; o censo consulta C17. A localização pós-E3 fornecida não corresponde ao caminho atual. | Reidentificar todos os aliases/último uso após E3; liberar no chamador ou encerrar escopo auxiliar. Último leitor futuro: **[nao verificado]**. (`app/chamados/management/commands/vigiar_fio_morto.py:176`; `app/chamados/services/vigia_fio.py:640`) |
| `ini`, `fim`, `dia_do_turno` | Há concordância: limites inclusivos e pergunta por dia do turno. | Preservar esses contratos; encurtar o retângulo é alteração diferente de soltar a carga. (`app/chamados/services/tarefa_do_dia.py:376`; `:420`; `:517`; `:562`) |
| `agora`, fotografia histórica | Carga exige o mesmo instante, mas declara leitura de vários estados atuais. Alimentação C17 repassa `agora` à fila e chama `pergunta_viva(p)` sem esse argumento. | Não tratar o lote como fotografia histórica integral; decidir eventual alinhamento temporal em escopo próprio, sem embuti-lo na liberação de memória. (`app/chamados/services/tarefa_do_dia.py:350`; `:355`; `:570`; `app/chamados/juizes.py:1576`; `:1583`) |
| `fila_ids`, `vivas`, `instancias`, `no_app` | C17 e carga não possuem o mesmo universo: célula/C6/app versus colaborador/janela/pergunta do fato. | Preservar as alimentações; substituição por interface pública equivalente exigiria outra demonstração de contrato. Não deduzir equivalência por sobreposição de SQL. (`app/chamados/juizes.py:1581`; `:1593`; `app/chamados/services/tarefa_do_dia.py:375`; `app/chamados/services/vigia_fio.py:615`) |
| `hoje`, `medido_em` | Vigia deriva dia civil local de um instante; placar exige o dia civil julgado; ESTADO converte offset ao fuso do host. | Confirmar fusos e rótulos; não reinterpretar `hoje` como dia do turno nesta obra. Igualdade dos fusos efetivos: **[nao verificado]**. (`app/chamados/management/commands/vigiar_fio_morto.py:168`; `:233`; `app/core/management/commands/pipeline_placar.py:86`; `:95`; `bin/fio_estado.py:129`) |

**3. Arquivos em comum**

A associação **cópia → arquivo** abaixo foi **LEU exclusivamente no anexo `arquivos_em_voo.txt`**; estado atual dessas raias: **[nao verificado]**.

| Necessidade da obra | Arquivo | Cruzamento fornecido |
|---|---|---|
| Principal: gerador, trava, declaração de escopo e selo | `bin/gerar_estado.py`, `bin/trava_teste.sh`, `bin/suite.sh`, `bin/regua.sh`, `bin/pre-push.sh`, futuros leitor/selo | Nenhum caminho correspondente no anexo. **[nao verificado fora da lista fornecida]** |
| Complemento: coleta/carimbo/alarme | `app/chamados/management/commands/vigiar_fio_morto.py` | `wt-bugpar-124848`, `wt-codex-hj-090-152256`, `wt-e3-b6-114947` |
| Complemento: possível local de liberação pós-E3 | `app/chamados/services/vigia_fio.py` | `wt-bugpar-124848`; necessidade exata pós-E3 **[nao verificado]** |
| Se reutilizar teste existente do vigia | `app/chamados/tests/test_o261_vigia_fio.py` | `wt-bugpar-124848`, `wt-codex-hj-090-152256`, `wt-e3-b6-114947` |

**INFERIU:** mudar o juiz/lote ou o cron não decorre necessariamente do complemento, que pede liberação no caminho consumidor e instrumento RSS. (`app/docs/BACKLOG.md:738`)

Caso o desenho amplie esses toques, os cruzamentos fornecidos são:

| Arquivo condicional | Cópias do anexo — **[nao verificado no HEAD]** |
|---|---|
| `app/chamados/services/tarefa_do_dia.py` | `wt-bugpar-124848`, `wt-e3-b6-114947` |
| `app/chamados/juizes.py` | `wt-bugpar-124848`, `wt-o168v6-233014`, `wt-e3-b6-114947` |
| `app/config/crons.py` | `wt-bugpar-124848`, `wt-codex-hj-090-152256` |

**4. perguntas de lei: 1**

**INFERIU — lacuna de alcance do instrumento:** quais corridas pertencem às “últimas 24 h”? Candidatas: início na janela; término na janela; qualquer sobreposição. Cada candidata precisa declarar bordas inclusivas/exclusivas, teto `agora` e tratamento das corridas ainda abertas. O contrato enumera início/fim, mas não escolhe essa pertença. (`app/docs/RELATO.md:1648`)

A exigência de unidade/universo/exclusões manda explicitar a resposta, sem fornecer a escolha. Não escolhi nenhuma candidata. (`CLAUDE.md:192`)

**Não contei novamente questões já respondidas:**

- Pouso = merges first-parent do main; fonte = Git; meta `<=1,5`; escopo declarado pelo chamador; falha da escrita não altera rc; log ilegível = sem medida. (`app/docs/RELATO.md:1648`)
- Suite inteira exige motivo, inclusive seleção efetivamente completa; a árvore certificada deve ser a que pousa. (`CLAUDE.md:101`; `CLAUDE.md:320`) [nao verificado]
- Instrumento exige contrato de cinco linhas e pouso próprio; não pode virar juiz de negócio. (`app/docs/CORTES.md:394`; `CLAUDE.md:327`; `CLAUDE.md:172`)
- Complemento é pós-E3, sem mudar E3,

[truncado em 20000 de 22288 caracteres]

## 13/11 18:15 · sessao f564a3cfb31d · RAIA-PREPARACAO · censo · origem=Code

**Achado principal — LEU:** o retro entrega `foto_ausente.batida_id`; o nascimento da identidade e o gerador procuram `ultima_batida.id`. A chave gravada pelo retro não satisfaz esses leitores. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:100`; `app/chamados/models.py:3464`; `app/chamados/services/disputa_emissao.py:387`)

**Limite de proveniência:** a cópia não contém repositório Git reconhecido por `git rev-parse HEAD`; vínculo ao HEAD/hash informado **[nao verificado]**. As referências abaixo são das linhas efetivamente lidas nesta cópia. Não gravei arquivos, importei a aplicação ou consultei banco/log **[nao verificado — registro desta execução]**.

**1. LEITORES E ESCRITORES POR AST**

Escopo: costura da obra B — envelope da batida, porta, gerador e derivação do episódio. O censo geral de todos os contratos de entrada mencionado no item C do aval fornecido fica fora deste levantamento **[nao verificado no HEAD — delimitação pelo anexo]**.

Script executado por `python3 -B -`, integralmente por stdin. `LITERAL` identifica candidato sintático; persistência e domínio são conferidos na leitura posterior. Despacho dinâmico, `getattr`, aliases por atribuição e fluxo interprocedural completo **[nao verificado]**.

```python
import ast
from pathlib import Path

TARGETS = {"criar_ou_atualizar_disputa", "gerar_perguntas_disputa",
           "batida_origem_do_nascimento"}
KEYS = {"ultima_batida", "foto_ausente"}
SKIP = {"tests", "migrations", "logs", ".git", "node_modules", ".venv", "venv"}
rows, errors = set(), []
parsed = 0

def literal(node):
    return node.value if isinstance(node, ast.Constant) else None

def source(node):
    return ast.unparse(node).replace("\n", " ")

for path in sorted(Path(".").rglob("*.py")):
    if SKIP.intersection(path.parts) or path.name.startswith("test"):
        continue
    try:
        tree = ast.parse(path.read_text(encoding="utf-8"), filename=str(path))
    except (SyntaxError, UnicodeError) as exc:
        errors.append((str(path), str(exc)))
        continue
    parsed += 1
    parents = {}
    for parent in ast.walk(tree):
        for child in ast.iter_child_nodes(parent):
            parents[child] = parent
    aliases = {n.asname or n.name: n.name for n in ast.walk(tree)
               if isinstance(n, ast.ImportFrom) for n in n.names}

    def owner(node):
        while node in parents:
            node = parents[node]
            if isinstance(node, (ast.FunctionDef, ast.AsyncFunctionDef)):
                return node.name
        return "<module>"

    def emit(kind, node, detail):
        rows.add((str(path), node.lineno, kind, owner(node), detail))

    def dict_leaves(node, prefix):
        if not isinstance(node, ast.Dict):
            return
        for key, value in zip(node.keys, node.values):
            k = literal(key)
            if isinstance(k, str):
                if isinstance(value, ast.Dict):
                    dict_leaves(value, prefix + "." + k)
                else:
                    emit("LITERAL", key,
                         prefix + "." + k + " <- " + source(value))

    for node in ast.walk(tree):
        if isinstance(node, (ast.FunctionDef, ast.AsyncFunctionDef)):
            if node.name in TARGETS:
                emit("DEF", node, node.name + "(" + source(node.args) + ")")
        if isinstance(node, ast.Call):
            name = (aliases.get(node.func.id, node.func.id)
                    if isinstance(node.func, ast.Name)
                    else node.func.attr
                    if isinstance(node.func, ast.Attribute) else "")
            if name in TARGETS:
                emit("CALL", node, source(node))
            if (isinstance(node.func, ast.Attribute)
                    and node.func.attr == "get" and node.args):
                k = literal(node.args[0])
                if k in KEYS or (
                    k in {"id", "timestamp", "tipo"}
                    and source(node.func.value) in {"ub", "ult"}
                ):
                    emit("R-key", node, source(node))
        if isinstance(node, ast.Subscript) and literal(node.slice) in KEYS:
            emit("W-key" if isinstance(node.ctx, ast.Store) else "R-key",
                 node, source(node))
            parent = parents.get(node)
            if isinstance(node.ctx, ast.Store) and isinstance(parent, ast.Assign):
                dict_leaves(parent.value, str(literal(node.slice)))
        if isinstance(node, ast.Dict):
            for key, value in zip(node.keys, node.values):
                if literal(key) in KEYS:
                    dict_leaves(value, str(literal(key)))

print("PARSED", parsed, "ERRORS", len(errors))
for path, line, kind, function, detail in sorted(rows):
    print(f"{kind} {path}:{line} {function}: {detail}")
for path, error in errors:
    print("PARSE_ERROR", path, error)
```

Resultado instrumental: `PARSED 887 ERRORS 0`; 5 chamadas da porta, 5 do gerador e 3 do derivador de origem **[nao verificado em arquivo — saída desta execução]**. Sítios correspondentes:

| Função | Chamadores encontrados — LEU |
|---|---|
| `criar_ou_atualizar_disputa` | `app/api/views.py:764`; `app/api/views_core.py:758`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:96`; `app/ponto/services/triagem_batida.py:735`; `app/chamados/services/fabrica_da_celula.py:172` |
| `gerar_perguntas_disputa` | `app/chamados/management/commands/smoke_chamados.py:103`; `app/chamados/services/disputa_emissao.py:294`; `app/chamados/services/disputa_emissao.py:670`; `app/chamados/services/disputa_emissao.py:682`; `app/chamados/services/fabrica_da_celula.py:174` |
| `batida_origem_do_nascimento` | `app/chamados/models.py:2867`; `app/chamados/models.py:3450`; `app/chamados/services/disputa_emissao.py:326` |

A porta é definida em `app/chamados/services/disputa_emissao.py:634` e reexportada por `app/chamados/models.py:4094`; os cinco emissores acima a importam por `chamados.models`. O gerador está em `app/chamados/services/disputa_emissao.py:300`; o derivador, em `app/chamados/models.py:3453`.

| Emissor | Payload relevante — LEU | Família |
|---|---|---|
| API principal | `ultima_batida={id:batida.pk, tipo:tipo, timestamp:agora.isoformat()}`; `foto_ausente={tipo_batida:tipo}`. (`app/api/views.py:758`; `app/api/views.py:453`) | Pode emitir BATIDA; `foto_ausente` pertence a BATIDA. (`app/chamados/catalogo/motivos.py:43`) |
| API gêmea | Mesmo formato. (`app/api/views_core.py:752`; `app/api/views_core.py:439`) | Mesma classificação. (`app/chamados/catalogo/motivos.py:43`) |
| Retro | Somente `foto_ausente={tipo_batida,batida_id,batida_em,_por}`. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:98`) | BATIDA literal. (`app/chamados/catalogo/motivos.py:43`) |
| Triagem | `saida_sem_entrada={data,hora_tentativa}`. (`app/ponto/services/triagem_batida.py:739`) | MARCO literal. (`app/chamados/catalogo/motivos.py:35`) |
| Fábrica | `{motivo:{data:dia.isoformat()}}`. (`app/chamados/services/fabrica_da_celula.py:172`) | **INFERIU MARCO no fluxo lido:** motivos vêm da ata e da tabela de marcos. (`app/chamados/services/fabrica_da_celula.py:139`; `app/chamados/services/fabrica_da_celula.py:71`; `app/chamados/services/declaracao_texto.py:42`) |

Assim, “os outros quatro emissores cumprem”, no anexo, não demonstra quatro outros emissores BATIDA: a cópia mostra duas APIs BATIDA, uma triagem MARCO e uma fábrica cujo fluxo lido produz MARCO. Cobertura de chamadas dinâmicas **[nao verificado]**.

| Campo da costura | Escritores — LEU | Leitores — LEU |
|---|---|---|
| `ultima_batida.id` | APIs: `app/api/views.py:759`; `app/api/views_core.py:753` | Origem: `app/chamados/models.py:3464`; dia: `app/chamados/services/disputa_emissao.py:72`, `app/chamados/services/disputa_emissao.py:387`; upload: `app/api/foto_recebida.py:68` |
| `ultima_batida.timestamp` | APIs: `app/api/views.py:761`; `app/api/views_core.py:755` | Último degrau do dia: `app/chamados/juizes.py:160`, com `.date()` em `app/chamados/juizes.py:166` |
| `ultima_batida.tipo` | APIs: `app/api/views.py:760`; `app/api/views_core.py:754` | Nenhum leitor específico localizado; leitura dinâmica/genérica **[nao verificado]** |
| `foto_ausente.tipo_batida` | APIs e retro: `app/api/views.py:453`; `app/api/views_core.py:439`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:99` | Nenhum leitor específico desse caminho localizado **[nao verificado]** |
| `foto_ausente.batida_id` | Retro: `app/ponto/management/commands/marcar_foto_ausente_retro.py:100` | Os leitores de episódio procuram outro caminho: `app/chamados/models.py:3464`; `app/chamados/services/disputa_emissao.py:387` |
| `foto_ausente.batida_em` / `_por` | Retro: `app/ponto/management/commands/marcar_foto_ausente_retro.py:101`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:102` | Nenhum leitor semântico específico localizado **[nao verificado]** |

O escritor persistente do contexto é `_abrir_ou_agregar_disputa`: nascimento em `app/chamados/services/disputa_emissao.py:590`; atualização em `app/chamados/services/disputa_emissao.py:562`. Ele funde o dicionário do motivo, acrescenta `qtd`/`ultima_em` e substitui outras chaves de topo. (`app/chamados/services/disputa_emissao.py:548`; `app/chamados/services/disputa_emissao.py:556`; `app/chamados/services/disputa_emissao.py:561`)

A coluna de episódio nasce pelo derivador em `_nascer` e na preparação da identidade; o contrato escrito diz que ela é preenchida uma vez. (`app/chamados/models.py:2867`; `app/chamados/models.py:3450`; `app/chamados/models.py:2437`)

Homônimos descartados após leitura: o literal `foto_ausente` do catálogo de perguntas e as `ultima_batida` de ficha/ranking não são esse envelope de disputa. (`app/chamados/models.py:3604`; `app/colaboradores/services/ficha.py:215`; `app/escala/services/furos_diarios.py:281`; `app/relatorios/views.py:1675`)

**Autoridade registrada versus derivação própria:**

| Sítio | Classificação |
|---|---|
| `turnos_do_colab` | Juiz registrado no lookup e no vivo. (`app/docs/JUIZES.md:20`; `app/core/juizes.py:208`) |
| `dia_do_turno_da_batida` | Adaptador da resposta desse juiz, pelo mapa por PK; não cria outra regra de dia. (`app/ponto/turnos.py:2927`; `app/ponto/turnos.py:2935`) |
| `selfie_ausente` | Registrado no vivo, com corte escrito; entrada nominal no lookup não localizada **[nao verificado]**. O vivo prevalece. (`app/core/juizes.py:1199`; `app/docs/CORTES.md:283`; `app/docs/JUIZES.md:14`) |
| Retro | Delega ausência/prazo ao juiz; monta o próprio envelope incompatível. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:74`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:98`) |
| Gerador / derivador de origem | Selecionam insumos do contexto; registro como juízes próprios não localizado **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:372`; `app/chamados/models.py:3464`) |
| `_dia_do_contexto_fraco` | Deriva dia civil do timestamp por conta própria, como último degrau declarado. (`app/chamados/juizes.py:151`; `app/chamados/juizes.py:166`) |
| `resolver_disputas_de_foto` | Seleciona disputa pelo contexto e encaminha perguntas em conjunto; a validação delega ao juiz registrado. (`app/api/foto_recebida.py:67`; `app/api/foto_recebida.py:73`; `app/api/foto_recebida.py:51`; `app/docs/JUIZES.md:38`) |

**Selos lidos:** o selo AST da família prende a declaração de família e os índices; o da selfie prende delegação ao juiz. Nenhum desses prende o envelope entregue pela porta. (`app/chamados/tests/test_e1_v5_familia.py:25`; `app/chamados/tests/test_e1_v5_familia.py:98`; `app/ponto/tests/test_selfie_ausente_juiz.py:130`)

O teste do retro verifica `foto_ausente.batida_id`; sua idempotência conta disputas, sem verificar identidade das perguntas. **INFERIU:** ele pode deixar passar esta quebra de contrato. Execução dos testes **[nao verificado]**. (`app/ponto/tests/test_bug126_selfie_invisivel.py:71`; `app/ponto/tests/test_bug126_selfie_invisivel.py:113`)

**2. DIVERGÊNCIAS SEMÂNTICAS — DECISÕES PARA O CONSTRUTOR**

As alternativas abaixo são de desenho; não escolhi nenhuma.

| DECISÃO | Desencontro e opções |
|---|---|
| **D1 — Transporte do episódio** | O retro entrega PK em `foto_ausente.batida_id`; os consumidores exigem `ultima_batida.id`. Opções: montar o envelope consumido diretamente no emissor; ou usar um montador comum dos emissores que entregue esse mesmo contrato. Alterar consumidores para aceitar outro transporte acrescenta alcance de arquivos. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:100`; `app/chamados/models.py:3464`; `app/chamados/services/disputa_emissao.py:387`) |
| **D2 — Evento desta emissão versus contexto acumulado** | O merge preserva chaves antigas ausentes no payload novo. **INFERIU:** o retro pode deixar uma `ultima_batida` de outro evento disponível ao nascimento. Opções: envelope completo e substituído no ato; ou transporte explícito do episódio corrente, independente do agregado. O acervo não pode ganhar episódio pela última batida do contexto fundido. (`app/chamados/services/disputa_emissao.py:548`; `app/chamados/services/disputa_emissao.py:561`; `app/chamados/models.py:3457`) |
| **D3 — Instante do fato versus recepção** | A API grava a batida com `ts_efetivo`, mas escreve `ultima_batida.timestamp=agora`; o retro escreve `batida_em` com o timestamp efetivo local. Opções: declarar e manter separados os instantes de recepção e fato; ou normalizar o campo existente, com alcance explícito sobre seus leitores. Não são sinônimos apenas convertendo fuso. (`app/api/views.py:507`; `app/api/views.py:655`; `app/api/views.py:761`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:101`) |
| **D4 — Dia transportado versus dia do episódio** | O gerador tenta `motivo.data` antes de consultar o episódio; a porta, quando possui episódio, consulta o juiz para o dia efetivo. Opções: transportar episódio e delegar o dia; ou transportar também o dia previamente obtido do mesmo juiz. Extrair o prefixo de `batida_em` como dia do turno não encontra apoio nesse contrato. (`app/chamados/services/disputa_emissao.py:372`; `app/chamados/services/disputa_emissao.py:387`; `app/chamados/models.py:2876`; `app/ponto/turnos.py:2928`) |
| **D5 — Upload por disputa versus identidade por episódio** | O upload compara uma `ultima_batida.id`, depois encaminha todas as perguntas de foto — ou todas da disputa — à validação. A identidade BATIDA é por episódio. Opções: demonstrar por bateria que o alcance coletivo preserva os episódios; ou incluir a migração desse leitor para seleção por episódio numa fatia declarada. (`app/api/foto_recebida.py:68`; `app/api/foto_recebida.py:73`; `app/api/foto_recebida.py:79`; `app/chamados/catalogo/motivos.py:16`) |
| **D6 — Contagem de emissões versus episódios** | `qtd` incrementa a cada merge do motivo e `ultima_em` recebe o instante do merge. Esses campos não contam batidas distintas. Opções: mantê-los declarados como recorrência de emissão; ou declarar separadamente uma necessidade de medida por episódio. Não usar `qtd` como prova de quantidade de fatos. (`app/chamados/services/disputa_emissao.py:552`; `app/chamados/services/disputa_emissao.py:557`; `app/chamados/services/disputa_emissao.py:558`) |

Conferência dos demais campos/parâmetros:

| Campo/parâmetro | Significado lido |
|---|---|
| `colaborador` / PK da batida | O derivador exige que a batida pertença ao colaborador da disputa. (`app/chamados/models.py:3468`) |
| `motivos` | A porta elimina repetições da lista; a família vem do catálogo, não da presença de uma chave no payload. (`app/chamados/services/disputa_emissao.py:659`; `app/chamados/catalogo/motivos.py:53`) |
| `origem` da porta / `_por` | `origem` participa da descrição/trilha; `_por` é metadado do retro. Não são a chave de identidade do episódio. (`app/chamados/services/disputa_emissao.py:664`; `app/chamados/services/disputa_emissao.py:597`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:102`; `app/chamados/models.py:3490`) |
| `tipo` / `tipo_batida` | APIs transportam a variável `tipo`; retro transporta `b.tipo`. Equivalência em todos os caminhos de dedup/conflito **[nao verificado]**. (`app/api/views.py:760`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:99`) |
| `data_evento` / `conversa` | Pergunta de fato usa dia do turno; conversa usa dia civil do pedido. São domínios distintos já escritos. (`app/chamados/models.py:2784`; `app/chamados/models.py:2795`; `app/chamados/models.py:2802`) |
| `tem_foto` | `FotoBatida OU Batida.foto`, coerente entre cron e juiz. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:75`; `app/ponto/registro_batida.py:83`) |
| `decorrido_min` / prazo | Minutos reais desde a batida; igualdade vence: `>=`. PWA=0; APK/NULL=90; override não altera PWA. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:76`; `app/ponto/registro_batida.py:72`; `app/ponto/registro_batida.py:89`; `app/ponto/registro_batida.py:91`) |
| `--dias` | Duração de varredura desde `agora`, com ambas as bordas inclusivas; não representa quantidade de dias de turno. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:65`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:67`) |
| Fuso da apresentação | O retro converte `batida_em` para local, mas seu cabeçalho formata `agora`/`corte` crus. Divergência lateral com a regra de apresentação; necessidade de incluí-la nesta obra **[nao verificado]**. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:101`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:81`; `CLAUDE.md:146`) |

**3. ARQUIVOS EM COMUM**

**INFERIU — desenho restrito à cura no emissor:** a origem necessária é o retro; o teste existente do retro precisa receber cobertura da pergunta/episódio, além do selo AST solicitado. Esses dois arquivos não aparecem na lista fornecida. Localização do selo futuro ainda não definida **[nao verificado]**. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:96`; `app/ponto/tests/test_bug126_selfie_invisivel.py:65`; anexo `arquivos_em_voo.txt` **[nao verificado no HEAD]**)

Se as decisões acima ampliarem a alteração aos consumidores, há estes cruzamentos. As atribuições de raia são leitura do anexo, sem comprovação no HEAD **[nao verificado]**:

| Arquivo candidato a alteração adicional | Raia(s) do anexo | Motivo da dependência |
|---|---|---|
| `app/chamados/services/disputa_emissao.py` | `wt-bugpar-124848`, `wt-e3-b6-114947` | Consome envelope e funde contexto. (`app/chamados/services/disputa_emissao.py:387`; `app/chamados/services/disputa_emissao.py:548`) |
| `app/chamados/models.py` | As duas anteriores + `wt-o168v6-233014` | Deriva episódio e mantém identidade. (`app/chamados/models.py:2867`; `app/chamados/models.py:3464`) |
| `app/api/views.py` | `wt-bugpar-124848`, `wt-e3-b6-114947` | Escritor de `ultima_batida`, se D3 alterar seu significado. (`app/api/views.py:758`) |
| `app/chamados/juizes.py` | As duas anteriores + `wt-o168v6-233014` | Leitor do timestamp, se D3 alterar seu significado. (`app/chamados/juizes.py:159`) |

`app/api/foto_recebida.py` e `app/api/views_core.py` não aparecem no anexo; sua eventual alteração depende de D5/D3, respectivamente **[nao verificado no HEAD quanto ao voo]**. (`app/api/foto_recebida.py:67`; `app/api/views_core.py:752`)

**4. perguntas de lei: 0**

**INFERIU, no alcance examinado:** as candidatas abaixo já têm resposta escrita; não foram contadas como lacunas.

| Candidata | Resposta existente |
|---|---|
| “Foto identifica dia ou batida?” | BATIDA identifica episódio; `foto_ausente` é dessa família. (`app/chamados/catalogo/motivos.py:16`; `app/chamados/catalogo/motivos.py:43`) |
| “Qual dia para uma batida noturna?” | Juiz do turno; batida não reclamada recebe o dia local em que aconteceu. (`app/ponto/turnos.py:2928`; `app/ponto/turnos.py:2955`) |
| “Pode extrair o episódio depois, do contexto fundido?” | O contrato explica por que isso não recupera o episódio do 

[truncado em 20000 de 23356 caracteres]

## 13/11 18:12 · sessao 4922a0386829 · - · - · origem=Ronald (tmux)

`_declarar_ausencia_de_pergunta` não produz férias no fluxo atual: resolve tipo pelo mapa em `app/chamados/services/materializacao.py:475`, retorna se desconhecido em `:476`, e o mapa contém somente falta/atestado (`app/chamados/models.py:3717`). Chamador exige motivo `declara_falta` (`app/chamados/services/materializacao.py:1083`). Portanto, esse achado AST não exige editar `materializacao.py` na HJ-091; inferência baseada nesses caminhos.

A irmã genérica `_materializar_ausencia_de_resposta` recebe tipos exclusivamente do mapa falta/atestado no chamador atual (`app/chamados/services/materializacao.py:603`; `app/chamados/services/disputa_emissao.py:1541`).

Quem mais lê, para caracterizar regressão:

- Célula: data/tipo/fim e eventos de save/delete (`app/ponto/signals.py:123`, `:124`, `:143`, `:150`).
- Fio: reconciliação e ausência decidida após commit (`app/ponto/signals.py:261`, `:265`, `:271`, `:272`).
- Cobrança: `ausencia_cobre(..., para='cobranca')`, explicação integral/parcial (`app/chamados/services/materializacao.py:79`, `:82`).
- Cobertura: folha exige aprovada; cobrança exclui rejeitada (`app/ponto/turnos.py:2517`, `:2519`); filtro por data/fim e desempate data/PK (`:2554`, `:2559`).
- Folha: filtro de aprovadas na janela e expansão delegada ao juiz (`app/folha/export.py:87`, `:70`).
- Histórico: raízes documentais, estado, datas/dias/documento/descrição (`app/ponto/services/dia_decidido.py:593`, `:615`, `:619`, `:621`, `:623`, `:624`).

## 13/11 18:12 · sessao 83dfb72e585c · - · - · origem=Ronald (tmux)

AST executado com `python3 -B`: **3369 arquivos parseados, 0 erros de parse, 103 registros**; contagem derivada em memória, sem arquivo de prova. [nao verificado: resultado instrumental desta execução] Cada etapa de uma cadeia ORM aparece como chamada separada; não são 103 sítios independentes. [nao verificado]

```python
import ast, pathlib

fields = {
    'tipo', 'status', 'data_turno', 'batida_entrada', 'batida_entrada_id',
    'saida_informada', 'batida_gerada', 'batida_gerada_id', 'aprovada_em',
    'atualizado_em', 'criado_em',
}
writers = {
    'app/api/views.py': {'j'},
    'app/ponto/views.py': {'j'},
    'app/ponto/services/regularizacao.py': {'j'},
    'app/ponto/services/justificativa.py': {'j'},
    'app/ponto/services/entrada_sincronizada.py': {'just'},
    'app/ponto/services/triagem_batida.py': {'just'},
    'app/chamados/reconciliador.py': {'_j'},
}

def chain(n):
    if isinstance(n, ast.Name):
        return n.id
    if isinstance(n, ast.Attribute):
        return chain(n.value) + '.' + n.attr
    if isinstance(n, ast.Call):
        return chain(n.func) + '()'
    return '?'

rows, parsed, errors = [], 0, []
for p in sorted(pathlib.Path('app').rglob('*.py')):
    try:
        tree = ast.parse(p.read_text(), filename=str(p))
        parsed += 1
    except (SyntaxError, UnicodeError) as e:
        errors.append((str(p), str(e)))
        continue
    parent = {c: n for n in ast.walk(tree)
              for c in ast.iter_child_nodes(n)}

    def owner(n):
        while n in parent:
            n = parent[n]
            if isinstance(n, (ast.FunctionDef, ast.AsyncFunctionDef)):
                return n.name
        return '<module>'

    if ('tests' in p.parts or 'migrations' in p.parts
            or p.name.startswith('test_')):
        continue
    for n in ast.walk(tree):
        if (isinstance(n, ast.Call)
                and chain(n.func).startswith('Justificativa.objects.')):
            rows.append((
                str(p), n.lineno, owner(n), 'ORM', chain(n.func),
                ','.join(k.arg or '**' for k in n.keywords),
            ))
        if (isinstance(n, ast.Attribute)
                and isinstance(n.ctx, ast.Store)
                and isinstance(n.value, ast.Name)
                and n.value.id in writers.get(str(p), set())
                and n.attr in fields):
            rows.append((
                str(p), n.lineno, owner(n), 'W', chain(n), '',
            ))
for r in sorted(rows):
    print('|'.join(map(str, r)))
print('PARSED', parsed, 'ERRORS', errors, 'ROWS', len(rows))
```

Resultado agrupado, com sítios positivos:

| Grupo | Sítios encontrados |
|---|---|
| Criação/payload | `app/api/views.py:1992`, `app/ponto/views.py:1635`, `app/ponto/services/regularizacao.py:91`, `app/ponto/management/commands/processar_alertas_turno.py:394` |
| Alteração `tipo` | `app/api/views.py:1986`, `app/ponto/views.py:1625` |
| Alteração `saida_informada` | `app/ponto/views.py:1628`, `app/ponto/services/regularizacao.py:103`, `app/ponto/services/justificativa.py:382` |
| Alteração `batida_entrada` | `app/ponto/views.py:1630` |
| Alteração `batida_gerada` | `app/ponto/services/justificativa.py:204`, `app/ponto/services/justificativa.py:381`, `app/ponto/views.py:1831` |
| Alteração `status` | `app/ponto/services/justificativa.py:205`, `app/ponto/services/justificativa.py:283`, `app/ponto/services/justificativa.py:383`, `app/ponto/views.py:1796`, `app/ponto/views.py:1875`, `app/ponto/services/regularizacao.py:105`, `app/ponto/services/entrada_sincronizada.py:41`, `app/ponto/services/triagem_batida.py:608`, `app/chamados/reconciliador.py:533` |
| Alteração `aprovada_em` | `app/ponto/services/justificativa.py:207`, `app/ponto/services/justificativa.py:285`, `app/ponto/services/justificativa.py:385`, `app/ponto/views.py:1798`, `app/ponto/views.py:1877` |
| Cobertura/financeiro | `app/escala/utils.py:1197`, `app/ponto/services/efeito_ausencia.py:41`, `app/ponto/services/fechamento.py:661`, `app/relatorios/management/commands/diff_extrato_parcial.py:43` |
| Fato novo | `app/ponto/services/cartorio.py:387` |
| Bloqueio por turno órfão | `app/ponto/utils_turno_orfao.py:53` |
| Telas/histórico/contadores | `app/api/views.py:2088`, `app/chamados/views.py:596`, `app/core/views.py:205`, `app/core/views.py:244`, `app/core/views.py:1191`, `app/ponto/views.py:998`, `app/ponto/views.py:1673`, `app/ponto/views.py:1704`, `app/ponto/views.py:1736`, `app/ponto/views.py:1738`, `app/ponto/views.py:1742` |
| SLA | `app/ponto/management/commands/alertar_justificativas_sla.py:19` |
| Comando destrutivo existente, apenas lido | `app/ponto/management/commands/zerar_dados_operacionais.py:44`, `app/ponto/management/commands/zerar_dados_operacionais.py:95` |

Limites: o filtro ORM exige nome literal `Justificativa`; aliases como `_Just` não são resolvidos. Stores usam os nomes locais previamente lidos e não fazem inferência de tipos global. Não inclui templates, SQL dinâmico, defaults aninhados como Stores nem gravações indiretas; os kwargs de criação e `defaults` aparecem na linha da chamada. [nao verificado: limites do instrumento entregue]

## 13/11 18:11 · sessao de30e1eb0201 · - · - · origem=Ronald (tmux)

LEU — AST em `app/**/*.py` de produção, excluindo `tests/`, `migrations/` e arquivos `test*`, confirmou estes sítios:

| Campo da costura | Escritores | Leitores específicos |
|---|---|---|
| `ultima_batida.id` | `app/api/views.py:759`; `app/api/views_core.py:753` | `app/api/foto_recebida.py:68`; `app/chamados/models.py:3464`; `app/chamados/services/disputa_emissao.py:72`, `:73`, `:387` |
| `ultima_batida.timestamp` | `app/api/views.py:761`; `app/api/views_core.py:755` | `app/chamados/juizes.py:159`, `:160`, `:166` |
| `ultima_batida.tipo` | `app/api/views.py:760`; `app/api/views_core.py:754` | Nenhum leitor literal específico localizado nessa varredura; consumidores dinâmicos/genéricos [nao verificado]. |
| `foto_ausente.tipo_batida` | `app/api/views.py:453`; `app/api/views_core.py:439`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:99` | Nenhum leitor específico desse caminho localizado; consumidores dinâmicos/genéricos [nao verificado]. |
| `foto_ausente.batida_id` | `app/ponto/management/commands/marcar_foto_ausente_retro.py:100` | Nenhum leitor específico desse caminho localizado; consumidores dinâmicos/genéricos [nao verificado]. |
| `foto_ausente.batida_em` | `app/ponto/management/commands/marcar_foto_ausente_retro.py:101` | Nenhum leitor literal localizado; consumidores dinâmicos/genéricos [nao verificado]. |
| `foto_ausente._por` | `app/ponto/management/commands/marcar_foto_ausente_retro.py:102` | Nenhum leitor específico desse caminho localizado; consumidores dinâmicos/genéricos [nao verificado]. |

LEU — `foto_recebida` também depende da chave problemática: compara `d.contexto.ultima_batida.id` à batida recebida e depois valida todas as perguntas `foto_ausente` da disputa, ou todas as perguntas quando só resta aquele motivo. Não filtra perguntas por `batida_origem`. (`app/api/foto_recebida.py:67`, `:68`, `:73`, `:79`)

INFERIU — harmonizar o emissor altera também esse caminho de upload tardio; exige cenário com duas batidas/fotos na mesma disputa e fotos recebidas em ordem diferente. A necessidade decorre do contexto mutável usado para selecionar a disputa e da validação coletiva. (`app/api/foto_recebida.py:67`, `:73`, `:79`; `app/chamados/services/disputa_emissao.py:548`)

LEU — há três significados temporais diferentes:

- `ultima_batida.timestamp` nos APIs recebe **agora**, o instante de recepção/processamento. (`app/api/views.py:497`, `:761`; `app/api/views_core.py:483`, `:755`)
- `Batida.timestamp` recebe **ts_efetivo**, escolhido por `hora_do_aparelho`; pode diferir de agora. (`app/api/views.py:507`, `:654`; `app/api/views_core.py:493`, `:645`)
- `foto_ausente.batida_em` recebe o timestamp da batida convertido para horário local. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:101`)

LEU — o consumidor fraco do dia usa `datetime.fromisoformat(...).date()` sobre `ultima_batida.timestamp`; os consumidores fortes usam `ultima_batida.id` e o juiz do turno. (`app/chamados/juizes.py:159`, `:166`; `app/chamados/services/disputa_emissao.py:72`, `:76`, `:387`, `:391`)

DECISÃO DE DESENHO — declarar se o payload harmonizado mantém instante de detecção e instante da batida como conceitos distintos, ou se muda o significado do timestamp existente e migra seus leitores. Não tratar `batida_em` e `ultima_batida.timestamp` como sinônimos sem essa decisão. (`app/api/views.py:761`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:101`; `app/chamados/juizes.py:166`)

LEU — o selo AST existente da família verifica `Motivo(..., familia=FAMILIA_*)`; os testes dos índices verificam listas e chaves das famílias. Não examinam o contexto entregue pelos emissores. (`app/chamados/tests/test_e1_v5_familia.py:25`, `:98`, `:104`, `:111`)

LEU — o selo AST da porta de disputa impede a declaração de fabricar diretamente o chamado pai; não verifica o episódio no payload dos emissores. (`app/chamados/tests/test_porta_disputa_emissao.py:144`)

LEU — testes existentes constroem manualmente `ultima_batida` e testam contexto ausente como conversa; não exercitam o payload emitido pelo retro. (`app/chamados/tests/test_a2_data_evento_batida.py:21`, `:53`)

Selo AST geral “todo emissor FAMILIA_BATIDA entrega episódio consumível”: não localizado por busca nominal e leitura dos selos acima; inexistência completa [nao verificado].

Homônimos excluídos da costura: `tipo_batida` de ações de chamado é lido no contexto raiz, e `ultima_batida` de ficha/ranking é data ou timestamp apresentado, não o dict de disputa. (`app/chamados/services/acoes_chamado.py:30`, `:195`; `app/colaboradores/services/ficha.py:147`, `:215`; `app/escala/services/furos_diarios.py:281`)

Nenhum template com caminho literal `foto_ausente.batida_id`, `foto_ausente.batida_em` ou `ultima_batida.id` foi localizado; renderização genérica de contexto [nao verificado].

## 13/11 18:11 · sessao 88a38d37473c · - · - · origem=Ronald (tmux)

LEU por AST (python3 -B, `ast.parse`, sem importar app): **5 sítios de chamada de produção** da porta; todos importam por `chamados.models`, que reexporta o símbolo em `app/chamados/models.py:4094`.

| Sítio | Contexto entregue | Família |
|---|---|---|
| `app/api/views.py:764` | `contexto_anomalia`; acrescenta `ultima_batida={id:batida.pk,tipo:tipo,timestamp:agora.isoformat()}` em `:758` | Pode BATIDA; `foto_ausente` escrito em `:453`; catálogo BATIDA em `app/chamados/catalogo/motivos.py:43`. |
| `app/api/views_core.py:758` | Mesmo formato, escrito em `:752` | Pode BATIDA; `foto_ausente` em `:439`; catálogo `motivos.py:43`. |
| `app/ponto/management/commands/marcar_foto_ausente_retro.py:96` | Apenas `foto_ausente={tipo_batida:b.tipo,batida_id:b.pk,batida_em:localtime(b.timestamp).isoformat(),_por:...}` em `:98` | BATIDA literal (`motivos.py:43`). |
| `app/ponto/services/triagem_batida.py:735` | `saida_sem_entrada={data:_dia_sse.isoformat(),hora_tentativa:agora_local.strftime('%H:%M')}` em `:739` | MARCO literal (`motivos.py:35`). |
| `app/chamados/services/fabrica_da_celula.py:172` | `{motivo:{data:dia.isoformat()}}` | **INFERIU MARCO**: motivo dinâmico; fluxo `:207` → `:139` → `:62`/`:71` → tabela `app/chamados/services/declaracao_texto.py:42`, cujos quatro valores são MARCO no catálogo `motivos.py:34`, `:35`, `:48`, `:49`. |

Logo, a frase do dado “outros 4 emissores cumprem” descreve quatro outros chamadores da porta, mas **não quatro outros chamadores BATIDA identificados nesta cópia**: dois são MARCO no fluxo lido; cobertura dinâmica completa [nao verificado]. Referências acima.

**Desencontros e decisões de desenho, sem escolher:**

1. **Episódio no caminho errado:** retro escreve `foto_ausente.batida_id` (`marcar_foto_ausente_retro.py:100`); identidade lê exclusivamente `ultima_batida.id` (`models.py:3464`) e data do turno também (`disputa_emissao.py:387`). Opções para o construtor: emissor entregar o envelope atualmente lido, ou contrato explícito de parâmetro de episódio consumido pelos dois leitores. Aval fornecido já manda cura na origem; mudança de leitores não é necessidade demonstrada pelo código. Validação de pertencimento já existe em `models.py:3468`.

2. **Instante de chegada versus instante do fato:** API define `agora` em `views.py:497`, resolve `ts_efetivo` em `:507` e grava Batida com este em `:654`; porém envelope registra `timestamp=agora.isoformat()` em `:761`. Gêmeo: `views_core.py:483`, `:493`, `:645`, `:755`. Retro `batida_em` usa instante efetivo da Batida em `marcar_foto_ausente_retro.py:101`. Opções: declarar metadado como instante da chegada, distinguindo-o do fato; ou alinhar envelope ao instante da Batida. Não é apenas UTC versus local: offline permite fatos anteriores ao recebimento (`views.py:504`, `:519`). Os dois leitores principais usam o ID e consultam a Batida, não os timestamps do envelope (`models.py:3464`, `disputa_emissao.py:387`).

3. **Contexto agregado versus episódio imutável:** merge sobrescreve chaves genéricas por `ctx[k]=v` em `disputa_emissao.py:561`; modelo adverte que ler contexto posteriormente pode apontar outra anomalia em `models.py:3457`. Identidade deve nascer uma vez (`models.py:3454`). Opções de desenho: garantir transporte correto no ato e preservar coluna imutável; avaliar separadamente consumidores tardios do agregado. Não retroderivar acervo pelo contexto atual: proibição expressa no comentário `models.py:3458`.

4. **Upload por disputa versus pergunta por episódio:** `app/api/foto_recebida.py:67` seleciona disputa pela `ultima_batida.id`; `:73` valida todas as perguntas `foto_ausente` dela, ou `:79` todas as perguntas quando motivo único. Episódios distinguem perguntas BATIDA (`models.py:3474`, `disputa_emissao.py:321`). Opções: demonstrar por bateria que a seleção agregada preserva os episódios, ou tratar leitor por episódio numa fatia autorizada. Isso merece medição/RED; não ampliar silenciosamente a cura do emissor. Identidade por episódio já escrita, não pergunta de lei.

**Sem divergência de unidade/borda no predicado de prazo lido:** cron calcula minutos reais por `total_seconds()/60` (`marcar_foto_ausente_retro.py:76`) e juiz cobra inclusive `decorrido_min >= prazo` (`registro_batida.py:91`); `tem_foto` é `FotoBatida OU Batida.foto` nos dois lados (`marcar_foto_ausente_retro.py:75`, `registro_batida.py:83`). PWA=0, APK=90 e desconhecido=APK (`registro_batida.py:72`, `:77`). `--minutos` só sobrepõe APK/NULL (`registro_batida.py:89`), período de varredura é intervalo fechado `timestamp__gte=desde, timestamp__lte=agora` (`marcar_foto_ausente_retro.py:67`). Não alterar estes parâmetros como cura de episódio.

**Selos existentes lidos, não executados:** `app/ponto/tests/test_bug126_selfie_invisivel.py:65` verifica disputa e metadado `batida_id` em `:71`, mas não episódio/data da pergunta; idempotência em `:108` conta disputas (`:113`), não perguntas. `app/ponto/tests/test_selfie_ausente_juiz.py:130` usa AST para impedir `_eh_apk` próprio nas APIs; `:142` sela papel de cron/delegação. Não sela envelope BATIDA da chamada. `app/chamados/tests/test_a2_data_evento_batida.py:21` monta envelope correto `ultima_batida`, e `:53` testa sem envelope nascer conversa. **INFERIU:** selos podem estar verdes e ainda permitir o defeito do retro, pois verificam condições distintas; execução [nao verificado].

Script executado (consulta, sem gravar):

```python
import ast
from pathlib import Path

for p in sorted(Path(".").rglob("*.py")):
    if any(x in p.parts for x in (".git", "__pycache__", "tests", "migrations")):
        continue
    try:
        tree = ast.parse(p.read_text())
    except (SyntaxError, UnicodeError):
        continue
    aliases = {"criar_ou_atualizar_disputa"}
    for node in ast.walk(tree):
        if isinstance(node, ast.ImportFrom):
            for imported in node.names:
                if imported.name == "criar_ou_atualizar_disputa":
                    aliases.add(imported.asname or imported.name)
    for node in ast.walk(tree):
        if not isinstance(node, ast.Call):
            continue
        fn = node.func
        match = (
            isinstance(fn, ast.Name) and fn.id in aliases
        ) or (
            isinstance(fn, ast.Attribute)
            and fn.attr == "criar_ou_atualizar_disputa"
        )
        if match:
            print(f"{p}:{node.lineno} {ast.unparse(node)}")
```

Saída: exatamente as cinco chamadas da tabela. Limite: AST encontra nomes/atributos e aliases de import direto; despacho dinâmico, atribuição de alias e prova interprocedural [nao verificado].

_4764 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
