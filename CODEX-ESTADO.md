# CODEX-ESTADO

> Ultima resposta processada: **28/11/2026 14:52** · publicado em **28/11/2026 14:53** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 28/11 14:52 · sessao 2f786fab1487 · FERIADO-BOTAO-FAIXA · construtor · origem=Code

**Resumo**  
Apliquei o diff em um único template. No layout `mes`, a faixa fica acima da rolagem, com ou sem botão. [_calendario_grade.html:99](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/partials/_calendario_grade.html:99)   [nao verificado]
`git diff --check` passou. Geometria no navegador, suíte e RED [nao verificado].   [nao verificado]
Próximo passo da obra é o Opus escrever o RED de geometria e o Code rodar a suíte. [Smoke existente:588](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:588) [nao verificado]

### Alcance nas duas cascas

- `calendario_partial.html` inclui a grade com `layout='mes'`. A ficha inteira estende `base.html` e carrega esse calendário por HTMX. A aba **Espelho** do drawer carrega o mesmo calendário em `base.html` e `base_app.html`. [calendario_partial.html:67](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/calendario_partial.html:67) · [detalhe.html:1](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/detalhe.html:1) · [detalhe.html:215](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/detalhe.html:215) · [base.html:399](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/base.html:399) · [base_app.html:154](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/base_app.html:154) · [_drawer_generico.html:134](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/core/_drawer_generico.html:134)
- Os outros includes diretos usam `layout='fita'` no diagnóstico e o layout padrão na auditoria. Ambos renderizam a lista completa de badges no lugar original. [_drawer_diagnostico.html:35](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/partials/_drawer_diagnostico.html:35) · [furos.html:42](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/relatorios/furos.html:42) · [_calendario_grade.html:148](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/partials/_calendario_grade.html:148)

Nas duas cascas, a faixa fica logo abaixo do botão quando ele aparece e logo abaixo do cabeçalho do dia nos demais feriados. Batidas, detalhes e outros badges ficam acessíveis pela rolagem da célula. Meses de cinco e seis semanas mantêm sete colunas e células de 132 px em todas as semanas. A seleção usa `dia.feriado` e a posição do primeiro badge, conforme a ordem do serviço. O texto e a cor vêm desse badge. [_calendario_grade.html:65](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/partials/_calendario_grade.html:65) · [_calendario_grade.html:96](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/partials/_calendario_grade.html:96) · [_calendario_grade.html:136](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/partials/_calendario_grade.html:136) · [_calendario_grade.html:148](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/partials/_calendario_grade.html:148) · [calendario.py:711](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/colaboradores/services/calendario.py:711)

### RED para o Opus

O smoke `test_clica_no_feriado_na_pagina_e_no_drawer_das_duas_cascas` é o teste de tela afetado. A faixa passa a ocupar espaço acima da região cujo tamanho o smoke exercita ao clicar no botão, abrir o popover e rolar até a última batida. [Smoke:588](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:588) · [Rolagem:469](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:469) [nao verificado]

Reusar o feriado de 20/11/2026 com quatro batidas e acrescentar um segundo badge. Rodar na ficha real e no Espelho das duas cascas, com e sem botão, em meses de cinco e seis semanas. Forçar o transbordamento com conteúdo antes da posição antiga da faixa, por exemplo aumentando o bloco de ícone e detalhes. Localizar a faixa pelo texto na célula e exigir exatamente uma ocorrência, com os quatro lados visíveis em `scrollTop=0` e após rolar até a última batida. Conferir também que o outro badge é alcançável, que o botão abre o popover quando oferecido e que as células continuam com 132 px. [Fixture:26](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:26) · [Sonda de geometria:329](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:329) · [_calendario_grade.html:68](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/partials/_calendario_grade.html:68)

**Mutantes propostos**

- Devolver a faixa à posição antiga dentro do conteúdo rolável, mantendo texto e cor. A checagem dos quatro lados deve acusar o recorte. [_calendario_grade.html:105](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/partials/_calendario_grade.html:105) · [_calendario_grade.html:148](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/partials/_calendario_grade.html:148)
- Condicionar a rolagem à presença do botão. O feriado sem ação e com conteúdo extra deve perder acesso à última batida ou ao outro badge. [_calendario_grade.html:96](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/partials/_calendario_grade.html:96) · [_calendario_grade.html:105](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/partials/_calendario_grade.html:105)
- Manter a faixa acima da rolagem e também renderizar o primeiro badge no laço. A checagem de ocorrência única deve falhar. [_calendario_grade.html:100](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/partials/_calendario_grade.html:100) · [_calendario_grade.html:149](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/partials/_calendario_grade.html:149)

## 28/11 14:49 · sessao a40e931c5c8a · FERIADO-SEM-LIMITE-DO-SUPRIMIDO · construtor · origem=Code

**Resumo**  
Cura aplicada em [alimentacao.py](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-144715/app/ponto/calculador/alimentacao.py:365). A projeção do `oraculo` usa a mesma porta da promoção.   [nao verificado]
Veredito estático: sintaxe válida, `git diff --check` limpo e 0 ocorrências de `except` no diff. Único arquivo alterado: `ponto/calculador/alimentacao.py:353`.  
Próximo: Opus relê a expectativa adicional de 26 h e escreve os RED e mutantes. Certificação pelo Code e pelo Opus [não verificado].

### Leitores e unidades

- A projeção usa `resultado.dia_da_jornada` com `_dia_de`, como o mensal e a lavra `motor`. A porta devolve minutos de dobra e do subconjunto noturno. Os três sítios convertem para horas em `horas_extras_100`, `horas_extras_100_feriado` e `horas_extras_100_noturna`. `horas_noturnas` continua vindo dos minutos noturnos legais. `ponto/calculador/alimentacao.py:360–384`, `ponto/services/dia_pago.py:139–156,261–269`, `ponto/services/fechamento.py:1003–1015`.
- `rubricas_por_dia` passa as rubricas em horas, pela chave do dia da jornada, a `regras.do_dia`. A regra incorpora os valores e a lavra `oraculo` os grava na linha desse dia. `soma_da_competencia` soma os mesmos campos em horas para a versão solicitada. `ponto/calculador/alimentacao.py:525,578–596`, `ponto/calculador/regras.py:788,818,958`, `ponto/services/fechamento.py:1295–1320`, `ponto/services/dia_pago.py:354–379,585–598`.
- O vigia usa a mesma conta para o teto em **minutos** e agrupa pela data de `entrada`. A diferença de chave pode aparecer na virada da meia-noite. `folha/porta_export.py:542–576`, `ponto/services/dia_pago.py:126–132,149–155`.

### Outros usos encontrados pelo grep

- `diff_calculador` aplica a projeção diretamente no lado `MOTOR`. O lado calculador também a recebe via `rubricas_por_dia` quando usa os pares da autoridade e a partição classificada. A comparação usa horas por dia. No lado `MOTOR`, a chamada direta também alcança a partição histórica se ela contiver promoção. `ponto/management/commands/diff_calculador.py:70,255–257,406–415,518–546`, `ponto/calculador/regras.py:393–404`.
- Os usos diretos nos testes estão em `ponto/tests/test_feriado_um_juiz.py:836,854,875,967,1074,1097`. Além dos três casos de porta, `test_RED_par_26h_split_nao_supera_he100` ainda exige **26 h** em `horas_extras_100`. Pela porta comum, a expectativa passa a **25 h**. A asserção de **1.500 min** no período do motor já está escrita. `ponto/tests/test_feriado_um_juiz.py:826–842`, `ponto/services/dia_pago.py:149–156`. Os alvos textuais do catálogo de mutantes continuam presentes. `ponto/tests/feriado_um_juiz_mutantes.py:294,404,408,484`, `ponto/calculador/alimentacao.py:360,378,386`, `ponto/management/commands/diff_calculador.py:255`.

### RED e mutantes para o Opus

- **Três RED existentes.** No 6x1 de 21h a 7h, a lavra deve dar **9 h** de HE100 feriado, **9 h** de HE100 noturna e **10,29 h** de noturno legal. No plantão de 26 h iniciado às 22h, as três rubricas de HE100 devem dar **25 h**. No plantão de 21h a 23h do dia seguinte, as três devem dar **25 h**, com **28,57 h** de noturno legal. `ponto/tests/test_feriado_um_juiz_porta.py:138–148,350–357,611–632`.
- **RED de agrupamento.** Dois pares promovidos com entradas em datas civis distintas e o mesmo dia no mapa. O primeiro tem 120 min todos noturnos e 60 min suprimidos. O segundo tem 120 min todos diurnos e zero suprimido. Cobrar uma linha no dia do juiz com **3 h** de HE100 total e feriado, das quais **2 h** noturnas. `ponto/services/dia_pago.py:126–132,149–156`, `ponto/motor_calculo_v2.py:753–785`.
- **Mutantes propostos.** Restaurar o trabalhado bruto nas duas HE100 totais deve cair nos três RED existentes. Restaurar só o split cheio deve cair no plantão iniciado às 22h, com **26 h** de HE100 noturna em vez de **25 h**. Chamar a porta par a par deve cair no RED de agrupamento, com **1 h** noturna em vez de **2 h**. Agrupar por data civil deve cair na chave da linha desse mesmo RED. Execução dos mutantes [não verificado]. `ponto/calculador/alimentacao.py:360–384`, `ponto/services/dia_pago.py:149–156`, `ponto/tests/test_feriado_um_juiz_porta.py:350–357,611–632`.

## 28/11 14:49 · sessao 1b673b01adf2 · - · - · origem=Ronald (tmux)

A partição efetivamente consumida pelo mensal está em `app/ponto/services/fechamento.py:995–1021`. Outubro escolhe o vínculo via `escala_vigente` e novembro usa o contexto do período ou do turno. Para g2, observe a rubrica comum retornada, `eh_feriado=True` e `feriado_vira_folga=False` no vínculo efetivo. A recusa vigente é um marcador adicional para g3, inclusive quando o dia já pertence a g1 ou g2. `app/ponto/calculador/regras.py:371–453`, `app/ponto/models.py:1265–1288`, `app/ponto/motor_calculo_v2.py:1511–1532`.

O gancho `coletar_furos` recebe a apuração já derivada no caminho de leitura. O cálculo diário canônico pode ser reaproveitado em memória interceptando as duas operações finais de persistência de `DiaPago`. Comparar os deltas diários completos permite apontar movimentos adicionais dentro de um dia que também recebeu g1 ou g2. `app/ponto/services/fechamento.py:1152–1173`, `app/ponto/services/fechamento.py:1237–1273`, `app/ponto/services/dia_pago.py:344–373`.

O método-base está em `app/ponto/motor_calculo_v2.py:1536–1602` e o acréscimo vai após a linha 1584. O prefixo do blob local é `891199180`, correspondente ao anexo. O launcher seleciona `juliani` e realiza manutenção de credencial e trilha próprias. A execução e os resultados na sombra estão [nao verificado]. `bin/sonda_leitura.sh:115–143`, `bin/sonda_leitura.sh:235–251`.

## 28/11 14:49 · sessao 06e8e544ee84 · - · - · origem=Ronald (tmux)

Os 24 campos numéricos persistidos estão em `app/ponto/services/fechamento.py:1199–1222`. O retorno de leitura contém os mesmos campos, mais mês e ano, em `app/ponto/services/fechamento.py:1165–1173` e `app/ponto/management/commands/aplicar_09_corte_b.py:67–74`. Cinco campos podem ser nulos por contrato da grade (`app/ponto/services/fechamento.py:1124–1136`).

A execução em lote nativa já visita cada colaborador do universo. Duas passadas independentes por empresa e competência mantêm a seleção real e evitam reutilizar objetos de batida que o motor muta (`app/ponto/services/fechamento.py:490–507,619–622,685–699,865–874`).

O método pré-hunk ocupa `app/ponto/motor_calculo_v2.py:1536–1602`. A inserção em memória após a linha 1584 reproduziu o hash pós-hunk `65f7046b2a1690e6673cc20e611fdaeb24abb00d` do anexo.

O launcher monta `juliani` na sombra, porém mantém arquivos operacionais próprios. A transação e o Python da sonda podem ser estritamente de leitura (`bin/sonda_leitura.sh:93–96,126–135,216–218,235–250`).

## 28/11 14:49 · sessao 62020b2ac988 · - · - · origem=Ronald (tmux)

A batida criada e a retratação chamam `_fechamento_por_evento`, que delega a `recalcular_por_evento`. O ajuste retroativo que materializa saída passa pela mesma porta da batida. A aprovação simples de justificativa invalida o fechamento. `app/ponto/registro_batida.py:170–198,287–302`, `app/ponto/services/justificativa.py:187–216,478–488`.

A edição ou regeneração de célula chama `_o_chao_do_dia_mudou`, que agenda um recálculo por competência afetada. A resposta validada usa `data_evento`, o aceite de folga usa o dia da folga, e `decidir_he` chama a porta pela data da decisão após gravá-la. `app/ponto/portas/celula.py:392,490–503,682`, `app/chamados/services/validacao.py:627–628,787–799`, `app/ponto/portas/he.py:206–230`.

O `on_commit` resolve a competência pela janela da empresa e chama `recalcular_fechamento_mes` com um PK em modo de escrita. Se o fato cai na competência 10, o escritor usa `save` ou `update` conforme o status. A guarda da cópia lista apenas 09/2026. `app/ponto/services/fechamento.py:37–51,510–513,623–630,1174–1227,1634–1644,1847–1864`, `app/ponto/services/dia_pago.py:714–731`. Ocorrências e valores efetivos em produção [nao verificado].

## 28/11 14:41 · sessao e1f21a73d56d · FERIADO-SEM-LIMITE-DO-SUPRIMIDO · leitura · origem=Code

**RESUMO**  
O “oráculo intocado” do corte é o calculador v3 de rubricas, `ponto/calculador/regras.py`. [PROMPTS.md:2956](/home/ronald/codex_ws/0f16e9470b5430e70fda70ef291fffc1f6c503af/app/docs/PROMPTS.md:2956) · [RELATO.md:430](/home/ronald/codex_ws/0f16e9470b5430e70fda70ef291fffc1f6c503af/app/docs/RELATO.md:430)  
A lavra `oraculo` usa esse calculador e lhe passa a projeção de folga trabalhada. Essa saída grava `DiaPago` de sombra. [app/ponto/services/fechamento.py:1292] · [app/ponto/calculador/alimentacao.py:571] · [app/ponto/services/fechamento.py:1311]  
Na folga promovida a feriado, a projeção usa `minutos_trabalhados / 60` para a HE100. O juiz F3 registrado responde se a dobra cabe. [app/ponto/calculador/alimentacao.py:363] · [app/core/juizes.py:172] · [app/core/juizes.py:320]

### 1. A que código o corte se refere?

O item 5 do RELATO identifica o código pelo nome e registra a comparação da c1: “oraculo v3 calculador/regras.py da 540, o v2 agora 480”. A resposta “oraculo intocado” está em `PROMPTS.md`. No código da cópia, `regras.do_dia` usa `dobra_de_feriado_do_dia` quando a dobra normal ainda não veio informada e o dia não recebeu rubricas da partição de folga. Esse helper devolve o trabalhado inteiro se `MotorBase._feriado_dobra_no_dia` der verdadeiro. [app/docs/RELATO.md:430] · [app/docs/PROMPTS.md:2956] · [app/ponto/calculador/regras.py:361] · [app/ponto/calculador/regras.py:855]

O BACKLOG registra o defeito da HE100 pelo trabalhado inteiro e liga a bateria v3 à L-110. LEIS e CORTES registram que os casos devem ter a resposta pela regra e que a comparação com o cálculo anterior mede impacto. A inclusão efetiva de todos os casos `S1–S5/C1–C7` na bateria desta cópia fica [nao verificado]. [app/docs/BACKLOG.md:2022] · [app/docs/BACKLOG.md:1707] · [app/docs/LEIS.md:136] · [app/docs/CORTES.md:383] · [app/docs/PROMPTS.md:2956]

### 2. Quem chama `_rubricas_da_particao_ft` e o que sai?

É **o mesmo calculador de rubricas**, com duas peças no fluxo. `rubricas_por_dia` chama `_rubricas_da_particao_ft` quando recebe a partição e entrega o mapa do dia a `regras.do_dia` via `rubricas_ft`. No ramo de folga trabalhada, `do_dia` usa os valores desse mapa. O teste de porta consulta explicitamente as versões `motor` e `oraculo` de `DiaPago` e confere o mensal e os eventos de exportação em separado. [app/ponto/calculador/alimentacao.py:518] · [app/ponto/calculador/alimentacao.py:571] · [app/ponto/calculador/regras.py:767] · [app/ponto/calculador/regras.py:788] · [app/ponto/tests/test_feriado_um_juiz_porta.py:67] [nao verificado]

- **Recalcular o fechamento.** A partir da comp. 11/2026, o caminho de escrita passa a partição para `rubricas_por_dia` e entrega o resultado a `lavrar(..., versao='oraculo')`. A lavra atribui as rubricas aos campos do dia e substitui as linhas `DiaPago` daquela versão, incluindo a linha de ajuste da competência. [app/folha/export.py:385] · [app/ponto/services/fechamento.py:603] · [app/ponto/services/fechamento.py:1228] · [app/ponto/services/fechamento.py:1292] · [app/ponto/services/fechamento.py:1311] · [app/ponto/services/dia_pago.py:331] · [app/ponto/services/dia_pago.py:354] · [app/ponto/services/dia_pago.py:370]
- **Comando `diff_calculador`.** Chama o helper diretamente para montar o lado motor nos dias com folga trabalhada e também pode alcançá-lo via `rubricas_por_dia` no lado calculador. Imprime a comparação por rubrica e, com `--csv`, escreve as divergências em arquivo. O comando é de diagnóstico e roda por ato. [app/ponto/management/commands/diff_calculador.py:255] · [app/ponto/management/commands/diff_calculador.py:406] · [app/ponto/management/commands/diff_calculador.py:541] · [app/ponto/management/commands/diff_calculador.py:646] · [app/config/crons.py:1040]

**Folha, TXT e holerite.** O fechamento mensal e a lavra `motor` recebem a mesma partição e fazem suas próprias somas da folga promovida. O TXT da tela lê `horas_extras_100_feriado` do `FechamentoMensal` calculado em memória. O espelho lê `DiaPago` na versão padrão `motor`. O módulo de holerite armazena e entrega o PDF recebido. O valor efetivamente pago ou impresso em produção fica [nao verificado]. [app/ponto/services/fechamento.py:995] · [app/ponto/services/fechamento.py:1237] · [app/ponto/services/fechamento.py:1263] · [app/ponto/services/dia_pago.py:238] · [app/folha/views.py:215] · [app/folha/views.py:276] · [app/folha/export.py:434] · [app/ponto/services/espelho.py:420] · [app/ponto/services/dia_pago.py:845] · [app/holerite/motor.py:473] · [app/holerite/views.py:1445]

### 3. Quem decide a dobra e quem calcula o valor?

**A classificação vem da autoridade.** `particionar_ft` consulta o ciclo pela constante registrada, chama `motor_turno._feriado_dobra_no_dia(dia_turno)` e usa o resultado na escolha da rubrica. O registro vivo `_F3` aponta para `CICLOS_FERIADO_SIMPLES`, que contém `12x36`. `JUIZES.md` confirma o endereço. O predicado diário combina o flag `feriado_em_dobra` com `eh_feriado(dia)`. [app/ponto/calculador/regras.py:314] · [app/ponto/calculador/regras.py:410] · [app/ponto/calculador/regras.py:428] · [app/core/juizes.py:172] · [app/core/juizes.py:320] · [app/docs/JUIZES.md:114] · [app/ponto/motor_calculo_v2.py:3740] · [app/ponto/motor_calculo_v2.py:961]

**O valor da folga promovida é calculado no helper.** Para essa parcela, ele lança `periodo.minutos_trabalhados / 60` em `horas_extras_100_feriado` e `horas_extras_100`, sem descontar o intra suprimido nessa conta. Para os períodos normais que caem no mesmo dia, ele soma o `minutos_extra_100_feriado` já apurado pelo motor. A diferença está na origem do **valor**, depois de decidida a **rubrica**. [app/ponto/calculador/alimentacao.py:363] · [app/ponto/calculador/alimentacao.py:370] · [app/ponto/calculador/alimentacao.py:379]

## 28/11 14:30 · sessao 7d83c1dc9dbb · C7-TELA-ADMIN-ENCERRA · sonda · origem=Code

**RESUMO — dado raspado fornecido [nao verificado em nova execução]**  
88 perguntas no alvo, em 7 disputas.  
7 disputas com irmãs vivas fora do alvo, 420 irmãs vivas fora do alvo e 0 perguntas em disputa já fechada.  
Motivos: `saida_sem_entrada` 122, `orfao_14h` 106, `intervalo_volta` 95, `intervalo_saida` 91, `sem_posto` 1, `batida_fora_escala_grave` 5.  
Distribuição: `{53: 1, 21: 1, 46: 1, 70: 1, 17: 1, 210: 1, 3: 1}`. As perguntas `61520` e `61521` estão no alvo, na disputa `7979`, com 3 irmãs vivas fora do alvo cada.

**Saída raspada fornecida, na íntegra** [nao verificado em nova execução]

```text
sonda_leitura: sonda.py [20704dd2b53e1772] no cpuset 4-7, lendo sombra como leitor (LEITURA)

alvo 88 disputas 7
disputas_com_irma_viva_fora_do_alvo 7 irmas_vivas_fora_do_alvo 420 perguntas_em_disputa_ja_fechada 0
motivos_das_irmas {'saida_sem_entrada': 122, 'orfao_14h': 106, 'intervalo_volta': 95, 'intervalo_saida': 91, 'sem_posto': 1, 'batida_fora_escala_grave': 5}
distribuicao_irmas_por_disputa {53: 1, 21: 1, 46: 1, 70: 1, 17: 1, 210: 1, 3: 1}
pergunta 61520 no_alvo True disputa 7979 irmas_vivas_fora_do_alvo 3
pergunta 61521 no_alvo True disputa 7979 irmas_vivas_fora_do_alvo 3
```

A tentativa local parou antes de executar a sonda, com código de saída `1` e erro `sonda_leitura: ./sonda.py nao existe`. A tentativa não produziu contagens. `bin/sonda_leitura.sh:71`

**Conferência do critério.** Pelo critério descrito no pedido, sim: o predicado de vida é o mesmo. `admin_encerra` chama `fechar` com `FECHOU_ADMIN_ENCERRA`. `fechar` aplica `chamados.juizes.pergunta_viva(q)` a todas as perguntas da disputa, reavalia a lista sob a trava e chama `fechar_por_admin` para cada viva, inclusive as que estão fora do alvo da sonda. Respostas sem veredito podem bloquear o fechamento antes desse laço. `app/chamados/models.py:2217`, `app/chamados/models.py:2078`, `app/chamados/models.py:2122`, `app/chamados/models.py:2123`, `app/chamados/models.py:2132`, `app/chamados/models.py:2175`, `app/chamados/models.py:2184`, `app/chamados/juizes.py:911`

A implementação do recorte e os argumentos usados em `sonda.py` ficam [nao verificado].

## 28/11 14:23 · sessao d8f55c0f4fab · E4C-F5-INSTANTE · leitura · origem=Code

Validei em memória o delta que recupera os vínculos aninhados e faz o gate acusar `projecao=?`. `app/core/censo_escritas.py:284` · `app/chamados/tests/test_chokepoint_chamado_gate.py:322` no delta.  
**5 REDs** deram 6 falhas de asserção e zero erros na cópia. A cura passou **22/22** métodos com `unittest.TestCase` e os analisadores reais. `app/chamados/tests/test_e4c_f5_c4_red.py:1551` · `app/chamados/tests/test_chokepoint_chamado_gate.py:347` na cópia.  
**4/4 mutantes mortos** por asserção. Censo **543 → 543**, com igualdade das tuplas completas. `app/core/censo_escritas.py:611` na cópia.  
Próximo passo do aceite é a conferência pela sessão. Sessão, suíte Django e execução da base `c9fded10a` [nao verificado].

### Vínculos e REDs

A leitura única dos registros de `ligacoes` fica em `_Ligacao.ler`, com a iteração em `app/core/censo_escritas.py:284` no delta. A inferência em `:469` e a projeção em `:590` passam por ela. Para atribuições simples, anotadas e nomeadas, ambas chegam ao mesmo RHS armazenado em `vinculo.valor`. `app/core/censo_escritas.py:253,420,430,442,609` no delta.

O trecho da cópia em `:340` passa a propagar os ramos em `:416` do delta, e o de `:363` passa a registrar o RHS sem condicioná-lo à profundidade em `:440`. A trava de `:519` dá lugar à seleção das escritas anteriores ao uso, da mais recente para a mais antiga. A primeira escrita garantida encerra a busca. Candidatos que levam a RHS distintos geram `projecao=?`. O controle verifica `p` e `q` nos respectivos ramos e a indeterminação após a junção. `app/core/censo_escritas.py:340,363,519` na cópia · `app/core/censo_escritas.py:595,609,630,641` · `app/chamados/tests/test_e4c_f5_c4_red.py:1663,1682` no delta.

Cada hunk abaixo teve **1** ocorrência do trecho original na cópia. O conjunto passou em `git apply --check -p1`. `app/core/censo_escritas.py:58,204,311,359,376,498` · `app/chamados/tests/test_e4c_f5_c4_red.py:1660` · `app/chamados/tests/test_chokepoint_chamado_gate.py:318,424` na cópia.

```diff
--- a/app/core/censo_escritas.py
+++ b/app/core/censo_escritas.py
@@ -58,6 +58,7 @@
 import ast
 import os
 from collections import deque
+from typing import NamedTuple
 
 CRIAR = frozenset({'create', 'get_or_create', 'update_or_create', 'bulk_create'})
 ALTERAR = frozenset({'update', 'bulk_update'})
@@ -204,11 +205,83 @@
 )
 
 
+_NOS_TRY = (ast.Try, ast.TryStar)
+_NOS_COM_RAMOS = (
+    ast.If, ast.IfExp, ast.Match, ast.With, ast.AsyncWith,
+    ast.For, ast.AsyncFor, ast.While, *_NOS_TRY,
+)
+
+
+def _inicio(no):
+    return no.lineno, no.col_offset
+
+
+def _fim(no):
+    return no.end_lineno, no.end_col_offset
+
+
+def _contem(bloco, ponto):
+    return bloco and _inicio(bloco[0]) <= ponto < _fim(bloco[-1])
+
+
+def _filhos_com_ramos(no):
+    for campo, filhos in ast.iter_fields(no):
+        if not isinstance(filhos, list):
+            filhos = (filhos,)
+        for filho in filhos:
+            if not isinstance(filho, ast.AST):
+                continue
+            if isinstance(no, ast.BoolOp) and campo == 'values' and filho is not filhos[0]:
+                yield filho, ((no, campo, (filho,)),)
+            elif (isinstance(no, _NOS_COM_RAMOS)
+                  and campo in ('body', 'orelse', 'handlers', 'finalbody', 'cases')):
+                bloco = (filho,) if campo in ('handlers', 'cases') else filhos
+                yield filho, ((no, campo, bloco),)
+            else:
+                yield filho, ()
+
+
+class _Vinculo(NamedTuple):
+    ordem: int
+    local: bool
+    valor: ast.AST | None
+    ponto: tuple[int, int]
+    ramos: tuple
+    outra_evidencia: ast.AST | None = None
+    valor_e_evidencia: bool = True
+
+    def evidencia(self):
+        return self.valor if self.valor_e_evidencia else self.outra_evidencia
+
+    def alcance(self, uso):
+        # True = executado ao chegar ao uso; False = possivel; None = outro ramo.
+        garantido = True
+        for no, campo, bloco in self.ramos:
+            if _contem(bloco, uso) or campo == 'finalbody':
+                continue
+            if uso < _fim(no):
+                if isinstance(no, (ast.If, ast.IfExp, ast.Match)):
+                    return None
+                if isinstance(no, _NOS_TRY):
+                    if campo == 'body' and _contem(no.orelse, uso):
+                        continue
+                    if campo == 'handlers' and (
+                        _contem(no.orelse, uso)
+                        or (isinstance(no, ast.Try) and not _contem(no.finalbody, uso))
+                    ):
+                        return None
+            garantido = False
+        return garantido
+
+
 class _Ligacao:
     def __init__(self):
-        self.vinculos = []  # (ordem, local, valor da projecao, evidencia do modelo)
+        self.vinculos = []
         self.campos = set()
         self.origem = None
+
+    def ler(self, locais=False):
+        return (v for v in self.vinculos if v.local or not locais)
 
 
 class _Varredor(ast.NodeVisitor):
@@ -311,46 +384,50 @@
                 self.modulos.add(a.asname or a.name)
 
     # -- escopos
-    def _ligar_alvo(self, ligacoes, alvo, valor, origem, local, ordem):
+    def _ligar_alvo(self, ligacoes, alvo, vinculo):
         if isinstance(alvo, ast.Name):
-            ligacoes.setdefault(alvo.id, _Ligacao()).vinculos.append((ordem, local, valor, origem))
+            ligacoes.setdefault(alvo.id, _Ligacao()).vinculos.append(vinculo)
         elif isinstance(alvo, ast.Starred):
-            self._ligar_alvo(ligacoes, alvo.value, None, None, local, ordem)
+            self._ligar_alvo(ligacoes, alvo.value, vinculo._replace(valor=None, outra_evidencia=None))
         elif isinstance(alvo, (ast.Tuple, ast.List)):
+            valor = vinculo.valor
             elts = ()
             if (isinstance(valor, (ast.Tuple, ast.List)) and len(alvo.elts) == len(valor.elts)
                     and not any(isinstance(e, ast.Starred) for e in (*alvo.elts, *valor.elts))):
                 elts = valor.elts
             for i, item in enumerate(alvo.elts):
-                evid = origem if i == 0 and isinstance(alvo, ast.Tuple) and isinstance(item, ast.Name) else None
-                self._ligar_alvo(ligacoes, item, elts[i] if elts else None, evid, local, ordem)
+                evidencia = (vinculo.evidencia()
+                             if i == 0 and isinstance(alvo, ast.Tuple) and isinstance(item, ast.Name)
+                             else None)
+                self._ligar_alvo(ligacoes, item, vinculo._replace(
+                    valor=elts[i] if elts else None, outra_evidencia=evidencia, valor_e_evidencia=False))
 
     def _colher_ligacoes(self, no, inferir):
-        # A origem do modelo usa a ordem em largura e pode ler escopos filhos,
-        # como antes. A tupla usa apenas os vinculos lexicais do escopo.
+        # A ordem em largura preserva a precedencia da inferencia de modelos.
         ligacoes = {}
-        fila = deque((sub, True, True) for sub in ast.iter_child_nodes(no))
+        fila = deque((sub, True, ramos) for sub, ramos in _filhos_com_ramos(no))
         ordem = 0
         while fila:
-            sub, local, direta = fila.popleft()
+            sub, local, ramos = fila.popleft()
             ordem += 1
             if not local and not inferir:
                 continue
             filhos_locais = local and not isinstance(sub, _NOS_DE_ESCOPO)
-            fila.extend((filho, filhos_locais, direta and isinstance(sub, ast.Expr))
-                        for filho in ast.iter_child_nodes(sub))
+            fila.extend((filho, filhos_locais, ramos + novos_ramos)
+                        for filho, novos_ramos in _filhos_com_ramos(sub))
             alvos, valor, origem = (), None, None
             if isinstance(sub, ast.Assign):
-                alvos, valor, origem = sub.targets, sub.value, sub.value
+                alvos, valor = sub.targets, sub.value
                 for alvo in alvos:
                     if (inferir and isinstance(alvo, ast.Attribute) and isinstance(alvo.value, ast.Name)
                             and alvo.attr in self.unicos):
                         ligacoes.setdefault(alvo.value.id, _Ligacao()).campos.add(self.unicos[alvo.attr])
             elif isinstance(sub, ast.AnnAssign) and sub.value is None:
                 if local and isinstance(sub.target, ast.Name):
-                    self._ligar_alvo(ligacoes, sub.target, sub, None, local, ordem)
+                    self._ligar_alvo(ligacoes, sub.target, _Vinculo(
+                        ordem, local, sub, _fim(sub), ramos, valor_e_evidencia=False))
             elif isinstance(sub, (ast.AnnAssign, ast.NamedExpr)):
-                alvos, valor, origem = (sub.target,), sub.value, sub.value
+                alvos, valor = (sub.target,), sub.value
             elif isinstance(sub, ast.AugAssign):
                 alvos, origem = (sub.target,), sub.value
             elif isinstance(sub, (ast.For, ast.AsyncFor, ast.comprehension)):
@@ -359,8 +436,13 @@
                 alvos, origem = (sub.optional_vars,), sub.context_expr
             elif isinstance(sub, (ast.Delete, ast.TypeAlias)):
                 alvos = sub.targets if isinstance(sub, ast.Delete) else (sub.name,)
-            for alvo in alvos:
-                self._ligar_alvo(ligacoes, alvo, valor if direta else None, origem, local, ordem)
+            if alvos:
+                vinculo = _Vinculo(
+                    ordem, local, valor, _fim(valor or origem or sub), ramos,
+                    outra_evidencia=origem, valor_e_evidencia=origem is None,
+                )
+                for alvo in alvos:
+                    self._ligar_alvo(ligacoes, alvo, vinculo)
             if not local:
                 continue
             nomes = ()
@@ -376,14 +458,15 @@
                                   ast.MatchAs, ast.MatchStar, ast.TypeVar, ast.ParamSpec, ast.TypeVarTuple)):
                 nomes = (sub.name,) if sub.name is not None else ()
             for nome in nomes:
-                ligacoes.setdefault(nome, _Ligacao()).vinculos.append((ordem, True, None, None))
+                ligacoes.setdefault(nome, _Ligacao()).vinculos.append(
+                    _Vinculo(ordem, local, None, _inicio(sub), ramos))
         return ligacoes
 
     def _inferir_amarras(self, ligacoes):
         # Ponto fixo sobre os mesmos vinculos: qs = X.objects.filter(); for b in qs: b.save().
         evidencias = sorted(
-            ((ordem, nome, valor) for nome, ligacao in ligacoes.items()
-             for ordem, _, _, valor in ligacao.vinculos if valor is not None),
+            ((v.ordem, nome, evidencia) for nome, ligacao in ligacoes.items()
+             for v in ligacao.ler() if (evidencia := v.evidencia()) is not None),
             key=lambda item: item[0],
         )
         mudou = True
@@ -498,31 +581,57 @@
         return (isinstance(classe, ast.ClassDef) and classe.name == 'DecisaoPergunta'
                 and isinstance(metodo, ast.FunctionDef) and metodo.name == 'registrar')
 
+    def _candidatos_da_projecao(self, nome, limite):
+        for i in range(limite, -1, -1):
+            escopo, ligacoes = self._escopos[i]
+            if isinstance(escopo, ast.ClassDef) and i != limite:
+                continue
+            ligacao = ligacoes.get(nome.id)
+            locais = list(ligacao.ler(locais=True)) if ligacao is not None else []
+            if locais:
+                break
+        else:
+            return []
+
+        uso = _inicio(nome)
+        escritas = [v for v in locais if not isinstance(v.valor, ast.AnnAssign)]
+        # Um fechamento pode ser chamado depois de uma escrita textual posterior ao seu corpo.
+        if i != limite and any(v.ponto > uso for v in escritas):
+            return []
+        anteriores = sorted(
+            (v for v in escritas if v.ponto <= uso),
+            key=lambda v: (v.ponto, v.ordem), reverse=True,
+        )
+        candidatos = []
+        for vinculo in anteriores:
+            alcance = vinculo.alcance(uso)
+            if alcance is not None:
+                candidatos.append((vinculo.valor, i))
+                if alcance:
+                    break
+        return candidatos
+
     def _valor_da_projecao(self, valor):
-        limite = len(self._escopos) - 1
-        while isinstance(valor, (ast.Name, ast.NamedExpr)):
+        pendentes = [(valor, len(self._escopos) - 1)]
+        visitados = set()
+        encontrado = None
+        while pendentes:
+            valor, limite = item = pendentes.pop()
+            if item in visitados:
+                continue
+            visitados.add(item)
             if isinstance(valor, ast.NamedExpr):
-                valor = valor.value
-                continue
-            for i in range(limite, -1, -1):
-                escopo, ligacoes = self._escopos[i]
-                if isinstance(escopo, ast.ClassDef) and i != limite:
-                    continue
-                ligacao = ligacoes.get(valor.id)
-                if ligacao is not None:
-                    locais = [v for _, local, v, _ in ligacao.vinculos if local]
-                    if locais:
-                        break
+                pendentes.append((valor.value, limite))
+            elif isinstance(valor, ast.Name):
+                candidatos = self._candidatos_da_projecao(valor, limite)
+                if not candidatos:
+                    return None
+                pendentes.extend(candidatos)
+            elif valor is None or (encontrado is not None and valor is not encontrado):
+                return None
             else:
-                return None
-            locais = [v for v in locais if not isinstance(v, ast.AnnAssign)]
-            if len(locais) != 1 or locais[0] is None:
-                return None
-            ligado = locais[0]
-            if (ligado.end_lineno, ligado.end_col_offset) > (valor.lineno, valor.col_offset):
-                return None
-            valor, limite = ligado, i
-        return valor
+                encontrado = valor
+        return encontrado
 
     def _projecao(self, no, valor):
         """`projecao=(inst, campos)` = `inst.save(update_fields=campos)` neste sitio (`ESCRITA_POR_PROJECAO`)."""
--- a/app/chamados/tests/test_e4c_f5_c4_red.py
+++ b/app/chamados/tests/test_e4c_f5_c4_red.py
@@ -1660,6 +1660,77 @@
                     (linha, None, 'save', 'DP.registrar(projecao=?) [projecao]'),
                 ])
 
+    def test_RED_projecao_ligada_em_if_elif_else(self):
+        fonte = (
+            'from chamados.models import DecisaoPergunta as DP\n'
+            'def f(p, q, linhas, a, b):\n'
+            '    if a:\n'
+            "        proj = (p, ['respondida_em'])\n"
+            '        DP.registrar(linhas, projecao=proj)\n'
+            '    elif b:\n'
+            "        proj = (p, ['respondida_em'])\n"
+            '        DP.registrar(linhas, projecao=proj)\n'
+            '    else:\n'
+            "        proj = (q, ['respondida_em'])\n"
+            '        DP.registrar(linhas, projecao=proj)\n'
+            '    DP.registrar(linhas, projecao=proj)\n'
+        )
+        self.assertEqual(self._varrer(fonte), [
+            (5, 'PerguntaDisputa', 'save', 'p.save() [update_fields] [projecao]'),
+            (8, 'PerguntaDisputa', 'save', 'p.save() [update_fields] [projecao]'),
+            (11, 'PerguntaDisputa', 'save', 'q.save() [update_fields] [projecao]'),
+            (12, None, 'save', 'DP.registrar(projecao=?) [projecao]'),
+        ])
+
+    def test_RED_projecao_ligada_em_try_except_else_finally(self):
+        fonte = (
+            'from chamados.models import DecisaoPergunta as DP\n'
+            'def f(p, q, r, linhas):\n'
+            '    try:\n'
+            "        proj = (p, ['respondida_em'])\n"
+            '        DP.registrar(linhas, projecao=proj)\n'
+            '    except ValueError:\n'
+            "        proj = (q, ['validada_em'])\n"
+            '        DP.registrar(linhas, projecao=proj)\n'
+            '    else:\n'
+            '        DP.registrar(linhas, projecao=proj)\n'
+            "        proj = (r, ['respondida_em'])\n"
+            '        DP.registrar(linhas, projecao=proj)\n'
+            '    finally:\n'
+            "        proj = (p, ['respondida_em'])\n"
+            '        DP.registrar(linhas, projecao=proj)\n'
+            '    DP.registrar(linhas, projecao=proj)\n'
+        )
+        self.assertEqual(self._varrer(fonte), [
+            (linha, 'PerguntaDisputa', 'save', f'{inst}.save() [update_fields] [projecao]')
+            for linha, inst in ((5, 'p'), (8, 'q'), (10, 'p'), (12, 'r'), (15, 'p'), (16, 'p'))
+        ])
+
+    def test_RED_projecao_ligada_em_with(self):
+        fonte = (
+            'from chamados.models import DecisaoPergunta as DP\n'
+            'def f(p, linhas):\n'
+            '    with contexto():\n'
+            "        proj = (p, ['respondida_em'])\n"
+            '        DP.registrar(linhas, projecao=proj)\n'
+        )
+        self.assertEqual(self._varrer(fonte), [
+            (5, 'PerguntaDisputa', 'save', 'p.save() [update_fields] [projecao]'),
+        ])
+
+    def test_RED_projecao_reatribuida_duas_vezes(self):
+        fonte = (
+            'from chamados.models import DecisaoPergunta as DP\n'
+            'def f(p, linhas):\n'
+            '    proj = obter()\n'
+            "    proj = (outro, ['respondida_em'])\n"
+            "    proj = (p, ['respondida_em'])\n"
+            '    DP.registrar(linhas, projecao=proj)\n'
+        )
+        self.assertEqual(self._varrer(fonte), [
+            (6, 'PerguntaDisputa', 'save', 'p.save() [update_fields] [projecao]'),
+        ])
+
     def test_os_dois_saves_da_porta_ficam_visiveis(self):
         import ast
 
```

### Balde

Na cópia, `tabela` recolhe `entidade=None` e o comando imprime esses achados com `--nao-atribuiveis`. O gate de chamado pula `None` em `:321`. O delta faz o gate consumir qualquer `None` com sufixo `[projecao]` antes do filtro de portas. O RED exige a acusação para ramos divergentes e para tupla cujo receptor ficou sem entidade, no próprio `chamados/models.py`. O `p.save()` comum da fixture continua sujeito ao filtro de `None`. `app/core/censo_escritas.py:649` · `app/core/management/commands/censo_escritas.py:37` · `app/chamados/tests/test_chokepoint_chamado_gate.py:321` · `app/core/portas.py:396` na cópia · `app/chamados/tests/test_chokepoint_chamado_gate.py:322,430,447` no delta.

```diff
--- a/app/chamados/tests/test_chokepoint_chamado_gate.py
+++ b/app/chamados/tests/test_chokepoint_chamado_gate.py
@@ -318,6 +318,9 @@
     ruins = []
     for x in achados:
         ent = x['entidade']
+        if ent is None and x['expr'].endswith(' [projecao]'):
+            ruins.append('%s:%d projecao=? %s' % (x['arquivo'], x['linha'], x['expr']))
+            continue
         if ent is None or portas.familia_de(ent) != FAMILIA:
             continue
         if x['arquivo'] in (portas.porta_de(ent) or ()):
@@ -424,6 +427,28 @@
         self.assertEqual(len(fora_da_porta(
             _achados(fonte, 'chamados/management/commands/x.py'))), 1)
 
+    def test_RED_o_gate_acusa_projecao_sem_entidade(self):
+        inicio = 'from chamados.models import DecisaoPergunta as DP\ndef f(p, q, linhas, flag):\n'
+        casos = (
+            ('ramos distintos',
+             '    if flag:\n'
+             "        proj = (p, ['respondida_em'])\n"
+             '    else:\n'
+             "        proj = (q, ['respondida_em'])\n"
+             '    DP.registrar(linhas, projecao=proj)\n',
+             7, 'DP.registrar(projecao=?) [projecao]'),
+            ('tupla sem entidade',
+             '    DP.registrar(linhas, projecao=(p, campos))\n',
+             3, 'p.save() [projecao]'),
+        )
+        arquivo = 'chamados/models.py'
+        for caso, corpo, linha, expr in casos:
+            with self.subTest(caso=caso):
+                fo

[truncado em 20000 de 24782 caracteres]

## 28/11 14:12 · sessao d7103919d556 · C7-TELA-ADMIN-ENCERRA · leitura · origem=Code

**Resumo** — Módulo completo abaixo, com 7 testes e 6 mutantes de um hunk. `app/chamados/urls.py:22`, `app/chamados/models.py:2210`  
Os seis diffs passaram em `git apply --check`. O selo AST passou na cópia e acusou o pai, a chamada removida e a escrita direta. `app/chamados/views_disputa_admin.py:49`, `app/chamados/views_disputa_admin.py:57`  
Próximo na sessão: rodar o módulo em `b82a5ef08`, em `d04973b65` e com cada mutante isolado. HTTP, ORM e trilha em execução seguem [nao verificado].

## 1. `app/chamados/tests/test_c7_tela_admin_encerra.py`

Copiei o molde de `TestCase` com `Empresa`, `Praca`, `Posto` e `Colaborador` da base C7, o usuário `sistema` do teste de reabertura vizinho e o relógio de `2026-10-22 15:00:00` congelado com `freezegun` da C7. A sessão `via_app` segue o teste das cascas. `app/chamados/tests/test_c7_red_minimos.py:88`, `app/chamados/tests/test_c7_red_minimos.py:92`, `app/chamados/tests/test_c7_red_minimos.py:127`, `app/chamados/tests/test_reabrir_disputa_admin_sem_aberta.py:9`, `app/core/tests/test_casca_telas_colab.py:35`

A recusa de `fechar` e o POST válido exercitam a mesma disputa muda. No pai, a recusa já existe e o RED esperado é a asserção de que o POST delegou à porta. As URLs literais deixam o teste do link chegar ao HTML da ficha. No teste de permissão, o controle positivo com staff impede que o 404 de rota ausente satisfaça o teste inteiro. `b82a5ef08988438256b90c5d0b9d94f4552156e5/app/chamados/models.py:2141`, `b82a5ef08988438256b90c5d0b9d94f4552156e5/app/chamados/urls.py:21`, `b82a5ef08988438256b90c5d0b9d94f4552156e5/app/templates/chamados/fio_colaborador.html:9`, `app/chamados/views_disputa_admin.py:36`

```python
import ast
import re
from datetime import date, datetime, timezone
from pathlib import Path
from unittest.mock import patch

from django.conf import settings
from django.contrib.auth.models import Group, User
from django.test import TestCase
from freezegun import freeze_time

from chamados.models import DecisaoPergunta, DisputaSupervisao, PerguntaDisputa
from colaboradores.models import Colaborador, Empresa, Posto, Praca
from core.models import LogAuditoria


RELOGIO = '2026-10-22 15:00:00'
INSTANTE = datetime.fromisoformat(RELOGIO).replace(tzinfo=timezone.utc)
MOTIVO = 'Plantão conferido com a supervisão'
SUCESSO = 'A disputa foi encerrada e o motivo ficou registrado.'


@freeze_time(RELOGIO)
class C7TelaAdminEncerraTest(TestCase):
    @classmethod
    def setUpTestData(cls):
        User.objects.get_or_create(username='sistema')
        empresa = Empresa.objects.create(
            razao_social='C7 TELA LTDA', cnpj='19131243000197', ativa=True,
        )
        praca = Praca.objects.create(nome='Praça C7', cidade='Londrina', estado='PR')
        posto = Posto.objects.create(nome='Posto C7', empresa=empresa, praca=praca)
        cls.staff = User.objects.create_user('c7_tela_staff', is_staff=True)
        cls.admin = User.objects.create_user('c7_tela_admin')
        cls.gestor = User.objects.create_user('c7_tela_gestor')
        cls.admin.groups.add(Group.objects.get_or_create(name='admin_ti')[0])
        cls.gestor.groups.add(Group.objects.get_or_create(name='gestor')[0])
        cls.colab = Colaborador.objects.create(
            nome='Colaborador C7 Tela', cpf='[cpf]', empresa=empresa,
            posto=posto, situacao='ativo', data_admissao=date(2026, 1, 1),
        )

    def setUp(self):
        self.ficha_url = f'/chamados/colaborador/{self.colab.pk}/'
        self.lista_url = f'{self.ficha_url}disputas/'
        self.encerrar_url = f'{self.lista_url}encerrar/'
        self.disputa = DisputaSupervisao.objects.create(
            colaborador=self.colab, motivos=['orfao_14h', 'intervalo_volta'],
        )
        self.pergunta = PerguntaDisputa.objects.create(
            disputa=self.disputa, motivo='orfao_14h', data_evento=date(2026, 10, 20),
            pergunta='A que horas terminou o plantão?', tipo_resposta='hora',
        )

    def _estado(self):
        self.disputa.refresh_from_db()
        return (
            self.disputa.fechada_em, self.disputa.fechada_por_id,
            self.disputa.resolucao, self.disputa.motivo_fechamento,
        )

    def _trilha(self):
        return LogAuditoria.objects.filter(
            modelo='DisputaSupervisao', objeto_id=self.disputa.pk, acao='disputa_fechada',
        )

    def _assert_aberta_sem_trilha(self):
        self.assertEqual(self._estado(), (None, None, '', ''))
        self.pergunta.refresh_from_db()
        self.assertIsNone(self.pergunta.materializada_em)
        self.assertIsNone(self.pergunta.materializada_por_id)
        self.assertEqual(self.pergunta.via_resolucao, '')
        self.assertFalse(self.pergunta.decisoes.exists())
        self.assertFalse(self._trilha().exists())

    def _login_app(self, ator):
        self.client.force_login(ator)
        sessao = self.client.session
        sessao['via_app'] = True
        sessao['plataforma_sessao'] = 'app'
        sessao.save()

    def test_ficha_mostra_link_so_para_staff_ou_admin_ti(self):
        link = rf'<a\b[^>]*\bhref="{re.escape(self.lista_url)}"[^>]*>\s*Disputas\s*</a>'
        for ator in (self.staff, self.admin):
            with self.subTest(ator=ator.username):
                self.client.force_login(ator)
                resposta = self.client.get(self.ficha_url)
                self.assertEqual(resposta.status_code, 200)
                self.assertRegex(resposta.content.decode(), link)
        self.client.force_login(self.gestor)
        resposta = self.client.get(self.ficha_url)
        self.assertEqual(resposta.status_code, 200)
        self.assertNotRegex(resposta.content.decode(), link)

    def test_lista_todas_as_perguntas_antes_do_form(self):
        irma = PerguntaDisputa.objects.create(
            disputa=self.disputa, motivo='intervalo_volta', data_evento=date(2026, 10, 19),
            pergunta='A que horas voltou do intervalo?', tipo_resposta='hora',
            resposta_colab='13:00', respondida_em=INSTANTE,
        )
        esperadas = ((self.pergunta, 'Pendente'), (irma, 'Respondida'))
        self._login_app(self.admin)
        resposta = self.client.get(self.lista_url)
        self.assertContains(resposta, f'id="disputa-{self.disputa.pk}"')
        self.assertTemplateUsed(resposta, 'base_app.html')

        antes, inicio_form, formulario = resposta.content.decode().partition(
            f'<form method="post" action="{self.encerrar_url}">',
        )
        self.assertTrue(inicio_form, 'O formulário precisa apontar para a porta HTTP.')
        secao = antes.split(f'id="disputa-{self.disputa.pk}"', 1)[1]
        itens = re.findall(
            r'<li\b[^>]*>\s*<strong>Pergunta #(\d+)</strong>(.*?)</li>', secao, re.S,
        )
        self.assertCountEqual([pk for pk, _ in itens], [str(p.pk) for p, _ in esperadas])
        por_pk = dict(itens)
        for pergunta, estado in esperadas:
            item = por_pk[str(pergunta.pk)]
            self.assertIn(estado, item)
            self.assertIn(f'Marco: {pergunta.instancia_marco}', item)
            self.assertIn(f'Dia: {pergunta.data_evento:%d/%m/%Y}', item)
            self.assertIn(pergunta.pergunta, item)

        formulario = formulario.split('</form>', 1)[0]
        self.assertIn(f'name="disputa_pk" value="{self.disputa.pk}"', formulario)
        self.assertIn('name="csrfmiddlewaretoken"', formulario)
        self.assertRegex(formulario, r'<textarea\b[^>]*\bname="motivo"[^>]*\brequired\b')
        self.assertIn('Encerrar disputa com motivo', formulario)

    def test_fechar_recusa_muda_e_post_com_motivo_delega(self):
        with self.assertRaisesRegex(ValueError, 'Encerrar com motivo'):
            self.disputa.fechar(self.staff)
        self._assert_aberta_sem_trilha()

        self.client.force_login(self.staff)
        motivo_postado = f'  {MOTIVO}  '
        porta_real = DisputaSupervisao.admin_encerra
        with patch.object(DisputaSupervisao, 'admin_encerra', autospec=True, side_effect=porta_real) as porta:
            resposta = self.client.post(self.encerrar_url, {
                'disputa_pk': self.disputa.pk, 'motivo': motivo_postado,
            })
        porta.assert_called_once_with(self.disputa, self.staff, motivo_postado)
        self.assertContains(resposta, SUCESSO)
        self.assertTemplateUsed(resposta, 'base.html')
        self.assertEqual(
            self._estado(),
            (INSTANTE, self.staff.pk, MOTIVO, DisputaSupervisao.FECHOU_ADMIN_ENCERRA),
        )
        self.pergunta.refresh_from_db()
        self.assertEqual(self.pergunta.via_resolucao, PerguntaDisputa.VIA_ADMIN_DECIDIU)
        self.assertEqual(self.pergunta.materializada_por_id, self.staff.pk)
        self.assertEqual(self.pergunta.materializada_em, INSTANTE)
        self.assertEqual(list(self.pergunta.decisoes.values_list('ato', 'ato_por_id', 'ato_motivo')), [
            (DecisaoPergunta.SILENCIO, self.staff.pk, f'admin_encerra: {MOTIVO}'),
        ])
        self.assertEqual(list(self._trilha().values_list('usuario_id', 'valor_depois')), [
            (self.staff.pk, {'motivo_fechamento': 'admin_encerra', 'resolucao': MOTIVO}),
        ])

    def test_post_sem_motivo_mostra_recusa_e_preserva_disputa(self):
        self.client.force_login(self.staff)
        porta_real = DisputaSupervisao.admin_encerra
        for vazio in (None, '', ' \t  '):
            with self.subTest(motivo=vazio):
                dados = {'disputa_pk': self.disputa.pk}
                if vazio is not None:
                    dados['motivo'] = vazio
                with patch.object(
                    DisputaSupervisao, 'admin_encerra', autospec=True, side_effect=porta_real,
                ) as porta:
                    resposta = self.client.post(self.encerrar_url, dados)
                porta.assert_called_once_with(self.disputa, self.staff, vazio)
                self._assert_aberta_sem_trilha()
                self.assertContains(resposta, 'exige o MOTIVO escrito')

    def test_reenvio_delega_e_preserva_uma_linha_de_trilha(self):
        self.assertIs(self.disputa.admin_encerra(self.admin, MOTIVO), True)
        estado = self._estado()
        trilha = list(self._trilha().values_list('pk', flat=True))
        decisoes = self.pergunta.decisoes.values_list('pk', 'ato', 'ato_em', 'ato_motivo')
        antes = list(decisoes.all())
        self.assertEqual(len(trilha), 1)
        self.assertEqual([linha[1] for linha in antes], [DecisaoPergunta.SILENCIO])

        retornos = []
        porta_real = DisputaSupervisao.admin_encerra

        def observar(disputa, ator, motivo):
            retorno = porta_real(disputa, ator, motivo)
            retornos.append(retorno)
            return retorno

        self._login_app(self.admin)
        motivo_postado = f'  {MOTIVO}  '
        with freeze_time('2026-10-22 16:00:00'), patch.object(
            DisputaSupervisao, 'admin_encerra', autospec=True, side_effect=observar,
        ) as porta:
            resposta = self.client.post(self.encerrar_url, {
                'disputa_pk': self.disputa.pk, 'motivo': motivo_postado,
            })
        porta.assert_called_once_with(self.disputa, self.admin, motivo_postado)
        self.assertEqual(retornos, [False])
        self.assertContains(resposta, 'Nada mudou: a disputa já estava encerrada com este motivo.')
        self.assertTemplateUsed(resposta, 'base_app.html')
        self.assertEqual(self._estado(), estado)
        self.assertEqual(list(self._trilha().values_list('pk', flat=True)), trilha)
        self.assertEqual(list(decisoes.all()), antes)
        self.pergunta.refresh_from_db()
        self.assertEqual(self.pergunta.materializada_em, INSTANTE)

    def test_sem_permissao_recusa_get_e_post_sem_tocar_na_disputa(self):
        dados = {'disputa_pk': self.disputa.pk, 'motivo': MOTIVO}
        self.client.force_login(self.gestor)
        porta_real = DisputaSupervisao.admin_encerra
        with patch.object(DisputaSupervisao, 'admin_encerra', autospec=True, side_effect=porta_real) as porta:
            lista = self.client.get(self.lista_url)
            escrita = self.client.post(self.encerrar_url, dados)
        self.assertIn(lista.status_code, (403, 404))
        self.assertIn(escrita.status_code, (403, 404))
        porta.assert_not_called()
        self._assert_aberta_sem_trilha()

        self.client.force_login(self.staff)
        self.assertEqual(self.client.get(self.encerrar_url).status_code, 405)
        self.assertEqual(
            self.client.post(self.encerrar_url, dados).status_code, 200,
            'O mesmo POST precisa estar disponível ao staff.',
        )

    def test_ast_view_so_delega_a_decisao_de_fechamento(self):
        fonte = Path(settings.BASE_DIR) / 'chamados' / 'views_disputa_admin.py'
        self.assertTrue(fonte.is_file(), 'Falta a view que delega o encerramento à porta.')
        nos = list(ast.walk(ast.parse(fonte.read_text(encoding='utf-8'))))
        chamadas = [no for no in nos if isinstance(no, ast.Call)]
        self.assertEqual([
            ast.unparse(no) for no in chamadas
            if isinstance(no.func, ast.Attribute) and no.func.attr == 'admin_encerra'
        ], ['disputa.admin_encerra(request.user, motivo)'])

        funcoes_http = {'_exigir_admin', '_renderizar_disputas', 'get_object_or_404', 'render', 'str'}
        self.assertEqual([
            (no.lineno, no.func.id) for no in chamadas
            if isinstance(no.func, ast.Name) and no.func.id not in funcoes_http
        ], [])
        atributos_http = {
            'user.is_staff', 'user.groups', "user.groups.filter(name='admin_ti').exists",
            'user.groups.filter', 'colaborador.disputas', 'colaborador.disputas.filter',
            'colaborador.disputas.filter(fechada_em__isnull=True).prefetch_related',
            'request.user', 'request.POST', 'request.POST.get', 're.fullmatch',
            'DisputaSupervisao.objects', 'DisputaSupervisao.objects.select_related',
            'disputa.admin_encerra', 'disputa.colaborador',
        }
        self.assertEqual([
            (no.lineno, ast.unparse(no)) for no in nos if isinstance(no, ast.Attribute)
            if not isinstance(no.ctx, ast.Load) or ast.unparse(no) not in atributos_http
        ], [])
        self.assertCountEqual(
            [ast.unparse(no.test) for no in nos if isinstance(no, ast.If)],
            [
                "not (user.is_staff or user.groups.filter(name='admin_ti').exists())",
                "disputa_pk is None or not re.fullmatch('[0-9]{1,19}', disputa_pk)",
                'encerrou',
            ],
        )
        self.assertEqual(
            [no.lineno for no in nos if isinstance(no, (ast.IfExp, ast.Match, ast.While))], [],
        )
```

## 2. Mutantes

**M1 — Remover a chamada à porta.** Devem falhar `test_fechar_recusa_muda_e_post_com_motivo_delega`, `test_post_sem_motivo_mostra_recusa_e_preserva_disputa`, `test_reenvio_delega_e_preserva_uma_linha_de_trilha` e `test_ast_view_so_delega_a_decisao_de_fechamento`. As três falhas HTTP são esperadas [nao verificado]. `app/chamados/views_disputa_admin.py:52`

```diff
--- a/app/chamados/views_disputa_admin.py
+++ b/app/chamados/views_disputa_admin.py
@@ -49,7 +49,7 @@
     motivo = request.POST.get('motivo')
     # A recusa de negócio da porta precisa chegar ao admin com o motivo original.
     try:
-        encerrou = disputa.admin_encerra(request.user, motivo)
+        encerrou = True
     except ValueError as e:
         tipo_mensagem = 'error'
         mensagem = str(e)
```

**M2 — Aceitar motivo ausente, vazio ou em branco.** Deve falhar `test_post_sem_motivo_mostra_recusa_e_preserva_disputa` na asserção de disputa aberta [nao verificado]. `app/chamados/models.py:2214`

```diff
--- a/app/chamados/models.py
+++ b/app/chamados/models.py
@@ -2212,8 +2212,6 @@
         OBRIGATORIO -- *"a opcao coerente que o admin nao tinha"*. As mudas saem com `admin_decidiu` e trilha
         (quem, quando, motivo). Resposta sem veredito continua recusando: a porta e para o silencio. -> bool"""
         texto = (motivo or '').strip()
-        if not texto:
-            raise ValueError('Encerrar a disputa #%s exige o MOTIVO escrito (auditoria CLT).' % self.pk)
         return self.fechar(user, resolucao=texto, motivo=self.FECHOU_ADMIN_ENCERRA)
 
     def aparar_motivo_respondido(self, motivo):
```

**M3 — Abrir a permissão ao usuário autenticado.** Deve falhar `test_sem_permissao_recusa_get_e_post_sem_tocar_na_disputa` no status da lista do gestor [nao verificado]. `app/chamados/views_disputa_admin.py:13`, `app/chamados/views_disputa_admin.py:28`, `app/chamados/views_disputa_admin.py:36`

```diff
--- a/app/chamados/views_disputa_admin.py
+++ b/app/chamados/views_disputa_admin.py
@@ -12,7 +12,7 @@
 
 def _exigir_admin(user):
     if not (user.is_staff or user.groups.filter(name='admin_ti').exists()):
-        raise PermissionDenied
+        return
 
 
 def _renderizar_disputas(request, colaborador, *, mensagem=None, tipo_mensagem=None):
```

**M4 — Remover o link da ficha.** Deve falhar `test_ficha_mostra_link_so_para_staff_ou_admin_ti` na asserção do link renderizado [nao verificado]. `app/templates/chamados/fio_colaborador.html:11`

```diff
--- a/app/templates/chamados/fio_colaborador.html
+++ b/app/templates/chamados/fio_colaborador.html
@@ -8,9 +8,6 @@
 
 <div class="mb-4">
   {% include "core/_voltar.html" with voltar_url=voltar_url voltar_rotulo="Voltar" %}
-  {% if is_admin_ti %}
-  <a class="hx-btn" href="{% url 'chamados:disputas_admin_colaborador' colaborador.pk %}{% if request.GET.app == '1' %}?app=1{% endif %}">Disputas</a>
-  {% endif %}
 </div>
 <div class="painel-acompanhamento" style="border:1px solid #e0e6ef;border-radius:10px;padding:16px;margin-bottom:16px;background:#fafcff;">
   <div style="display:flex;justify-content:space-between;align-items:center;gap:12px;">
```

**M5 — Mostrar só a primeira pergunta.** Deve falhar `test_lista_todas_as_perguntas_antes_do_form` na comparação dos PKs visíveis antes do formulário [nao verificado]. `app/templates/chamados/disputa_admin.html:18`

```diff
--- a/app/templates/chamados/disputa_admin.html
+++ b/app/templates/chamados/disputa_admin.html
@@ -15,7 +15,7 @@
     <h2 id="disputa-{{ disputa.pk|unlocalize }}">Disputa #{{ disputa.pk|unlocalize }}</h2>
     <p>O encerramento vale para a disputa inteira, incluindo todas as perguntas abaixo.</p>
     <ul>
-      {% for pergunta in disputa.perguntas.all %}
+      {% for pergunta in disputa.perguntas.all|slice:":1" %}
       <li class="mb-2">
         <strong>Pergunta #{{ pergunta.pk|unlocalize }}</strong> · {{ pergunta.estado|capfirst }}<br>
         Marco: {% if pergunta.instancia_marco %}{{ pergunta.instancia_marco }}{% else %}Sem marco registrado{% endif %} ·
```

**M6 — Gravar estado diretamente na view.** `test_ast_view_so_delega_a_decisao_de_fechamento` falhou na checagem em memória. O mutante mantém a chamada à porta e repete uma gravação com o mesmo valor para exercitar o selo de escrita independentemente do valor final. `app/chamados/views_disputa_admin.py:52`, `app/chamados/views_disputa_admin.py:57`, `app/chamados/models.py:2199`

```diff
--- a/app/chamados/views_disputa_admin.py
+++ b/app/chamados/views_disputa_admin.py
@@ -55,6 +55,8 @@
         mensagem = str(e)
     else:
         if encerrou:
+            disputa.motivo_fechamento = DisputaSupervisao.FECHOU_ADMIN_ENCERRA
+            disputa.save(update_fields=['motivo_fechamento'])
             tipo_mensagem = 'success'
             mensagem = (
                 'A disputa foi encerrada e o motivo ficou registrado. '
```

## 3. Quem escreve e quem lê

O registro de autoridades aponta `DisputaSupervisao.pode_fechar` para C7 e `pergunta_respondida` para C21. Na tela, a view lê `fechada_em` para listar abertas e consome o retorno de `admin_encerra` no POST. A escrita persistente do fechamento está em `DisputaSupervisao.fechar`. `app/docs/JUIZES.md:62`, `app/docs/JUIZES.md:102`

[truncado em 20000 de 26212 caracteres]

## 28/11 13:37 · sessao bfebdada828f · O30-F3-CONSTRUIR · leitura · origem=Code

**Candidato:** `ac4ad27ce` (c19b), na cópia `/home/ronald/wt-codex-o30-f3-construir-225036`. A cura de C está no código. `wts.txt:5–8`, `ac4ad27ce:app/relatorios/tests/test_atestados_lote_tela.py:213–235`  
**Veredito:** a obra é **mão única**. Aprovação, RC e horário da suíte inteira, da certificação da Pista e do verificador final **[nao verificado]**. `CLAUDE.md:256–264`, `ls.txt:1–9`   [nao verificado]
**Próximo:** preparar a árvore de pouso sobre o main, conferir os dois caminhos em comum e certificar essa árvore antes do Opus final. `CLAUDE.md:347–354,423–433` [nao verificado]

### 1. Candidato e provas

`ac4ad27ce`, de **26/11 às 23:49:25 −03**, é o HEAD que inclui a leitura dos seis campos do atestado no CSV e no PDF. O pai `c1027999c` ajusta o teste do PDF para aceitar a quebra de linha em `Dias do atestado: 0`. As outras três cópias listadas param na c17, no merge de pouso da c17 ou no merge intermediário da c18. `wts.txt:1–16`, `ac4ad27ce:app/ponto/services/atestados_lote.py:102–136,201–207`, `ac4ad27ce:app/relatorios/tests/test_atestados_lote_tela.py:213–235`

A listagem mostra `suite.rc`, `suite.log`, `espera.txt` e `certifica.txt` sob `suite_c19/`, e `diff.patch` sob `final/`. O vínculo desses artefatos com o SHA, os resultados e os horários estão **[nao verificado]**. O `rc=0` às **09:04 de 27/11** é da conta de smoke na sombra. `ls.txt:1–9`, `relato_o30.txt:14198,14203` [nao verificado]

### 2. Caminhos alterados dos dois lados

Comparei os diffs desde a base comum `59600163c` até `ac4ad27ce` e até o main arquivado `d97d10939f4ba861be231bc031dd856c22d8c803`. A interseção tem **dois caminhos**. `wts.txt:5–8,13–16`, referências abaixo.

| Caminho | Mudanças a preservar no merge |
|---|---|
| `app/core/tests/test_medida_turno_do_par.py` | A F3 atualiza as linhas esperadas de `regularizar_retorno` e `extrato_parcial`. O main atualiza outras entradas do mesmo teste, incluindo exportação e fechamento. `ac4ad27ce:app/core/tests/test_medida_turno_do_par.py:341–346`, `d97d10939:app/core/tests/test_medida_turno_do_par.py:289,329–334` |
| `app/relatorios/views.py` | A F3 altera `atestados_lote`. O main acrescenta o motivo `codigo_dominio_invalido` em `ranking_furos`. `ac4ad27ce:app/relatorios/views.py:373–401`, `d97d10939:app/relatorios/views.py:1722–1731` |

### 3. Classificação pelo princípio 7b

**Mão única para a obra.** O diff toca `ponto/`, inclui a migration `0079` e altera a exportação do lote. O aval classifica **o achado D** como mão dupla, com dinheiro zero, e o manda para a fila, sem varredor novo. A linha de D já consta no BACKLOG fornecido. `CLAUDE.md:256–264`, `ac4ad27ce:app/ponto/migrations/0079_ausencia_atestado_dados.py:4–24`, `ac4ad27ce:app/relatorios/views.py:389–401`, `respostas_2811_1335.txt:1 (5)`, `backlog.txt:2`

### 4. Passos mínimos para pousar

1. **Registrar a retomada.** Marcar a resposta como `RESPONDIDA` e escrever no topo do RELATO `ERRO-BOBO O30-F3-CONSTRUIR`, com motivo `D mão dupla, à fila`. Se a marca de pausa ainda existir **[nao verificado]**, soltá-la pela porta `bin/pausar.sh --soltar`, com trilha. `respostas_2811_1335.txt:1 (5)`, `relato_o30.txt:14627`, `CLAUDE.md:284–287,295–306`, `bin/pausar.sh:39–52`
2. **Montar a árvore em cópia nova do main e integrar `ac4ad27ce`.** Conferir os dois caminhos acima. Tratar `bin/sombra_regras_pessoais.py` em pouso próprio pela L-105 e conferir a máscara dos campos da `0079` no ensaio. Na conferência do escopo, o lote ainda usa tipos fixos e consulta por empresa, sem receber `q`, colaborador ou tipo como filtros. Dispensa desses filtros **[nao verificado]**. `CLAUDE.md:37–40,347–355`, `ac4ad27ce:bin/sombra_regras_pessoais.py:55–56,80,112`, `ac4ad27ce:app/relatorios/views.py:352–387`, `backlog.txt:1`
3. **Rodar a suíte inteira na árvore que vai pousar.** Na Pista 2, colher o log e o recibo de espera e obter `bin/pista_certifica.sh <sha> <log> <espera>` com **rc 0**. A prova exige camadas iguais às da VM, SHA exato, todas as labels, contagem reconciliada, rc 0 do run e `oom=0`. Se a Pista devolver rc 1 na certificação, a suíte de pouso roda na VM. Conferir revisão Codex, censo e mutantes e ter o **Opus final depois da inteira verde**. `CLAUDE.md:129,261–264,417–433`, `bin/pista_certifica.sh:4–17`
4. **Pousar a árvore certificada em um ato.** Conferir a sombra verde do dia. Fazer `merge --no-ff --no-commit`, comparar arquivo a arquivo com `cmp` contra a cópia certificada, registrar a comparação e `OBRA: O30-F3-CONSTRUIR` no commit, com a linha `LEI-AKITA`, e chamar imediatamente `bin/deploy.sh` **com a migration `0079`**. Registrar o resultado e o smoke no RELATO. `CLAUDE.md:62–64,90–92,284–287,347–355,477–491`, `relato_o30.txt:14055`

## 28/11 13:19 · sessao bfd44151da34 · FERIADO-SEM-LIMITE-DO-SUPRIMIDO · censo · origem=Code

**Feito.** Censo somente leitura da cópia `db9adafb21a643521da06ef6f2b2060a265b8fbe`. O diretório veio sem `.git`, portanto o hash por Git fica [nao verificado].  
**Veredito.** Refazer a diferença na cópia nova. O juiz do feriado já está no lugar, mas os quatro ramos regulares e a promoção ainda pagam o trabalhado inteiro. `app/ponto/motor_calculo_v2.py:2925,3208,3385,3467` · `app/ponto/services/fechamento.py:995–1008`  
**Números da regra.** Emp2, comp. 11. Col243 em 15/11 deve ter **420,0 min** de HE100 feriado. Col1046 em 20/11 deve ter **419,7 min**. `app/docs/RELATO.md:425` · `FERIADO-C1-LEIS-2511.txt:1`  
**Próximo.** Portar a conta diária para motor, promoção e vigia no mesmo ato. K1 tem RED marcado e aval condicional para a origem. DIFF e zero pós-apply [nao verificado]. `app/docs/BACKLOG.md:2014,2201` · `app/ponto/tests/test_feriado_um_juiz_porta.py:380–394`

## 1. Mapa curto

| Etapa | O que a cópia já faz e onde está a diferença |
|---|---|
| **Motor** | `_feriado_dobra_no_dia` lê o calendário e a política do ciclo pelo dia do turno. Os quatro ramos gravam o trabalhado inteiro em HE100, HE100 feriado e HE total. O motor separa `periodos` de `periodos_ft` depois de aplicar a janela de HE. `app/ponto/motor_calculo_v2.py:941–978,2558–2574,2581–2607,2925–2929,3213–3220,3385–3389,3471–3478` |
| **Fechamento e promoção** | O fechamento compõe a apuração por fatia com o mapa do dia e o contexto do motor/vínculo. Na via regular, lê os totais de `ResultadoMes`. Na FT, `particionar_ft` decide dia, rubrica e split noturno. A partição ainda entrega `periodo`, noturno de relógio e noturno legal. O mensal usa `f.periodo.minutos_trabalhados` para o valor promovido. `app/ponto/services/fechamento.py:344–383,988–1012` · `app/ponto/calculador/regras.py:371–448` |
| **DiaPago** | A lavra do motor recebe o mesmo resultado e a mesma partição do mensal. Soma os campos de HE dos pares regulares e usa o trabalhado inteiro da FT promovida. A intra entra em rubrica própria. A versão `oraculo` passa pela alimentação do v3 e pela mesma porta de lavra. `app/ponto/services/fechamento.py:1228–1254,1287–1317` · `app/ponto/services/dia_pago.py:176–189,217–219,238–251,331–339` |
| **Porta** | `medir` visita quem tem `status == 'entra'` no classificador do TXT. `minuto_em_duas_rubricas` lê `resultado.periodos` do espelho vivo pela data civil de entrada. Aplica a fórmula comum `max(0, T − previsto − min(S, declarado))` e acusa acima de **teto + 1 min**. O contador entra em `falhas_proprias`. A comparação mensal × soma do DiaPago é outra testemunha. `app/folha/porta_export.py:457–482,538–571,693–703` |
| **TXT** | A emissão passa pela porta e usa o `FechamentoMensal`. Desde a comp. 11, lê a classificação mensal por fatia. Separa HE100 comum como `HE100 total − HE100 feriado`, emite a parcela feriado e a intra em eventos próprios, e converte as horas em centésimos. HE100 noturna é subconjunto. O adicional noturno tem evento próprio. `app/folha/views.py:123–140` · `app/folha/export.py:34–45,383–394,434–462,747–760` |

## 2. Censo fechado dos quatro campos

`E` = `minutos_extra`, `H` = `minutos_extra_100`, `F` = `minutos_extra_100_feriado`, `N` = `minutos_extra_100_noturna`. O censo cobre os acessos de código aos quatro campos no caminho da dobra, incluindo resets, projeções e instrumentos que leem esses campos. `app/ponto/motor_calculo_v2.py:216–225` · `app/ponto/calculador/periodo.py:178–183` · `app/core/medida_leitor_do_dia.py:50–53`

| Escritor | Campos e efeito na dobra |
|---|---|
| **Motor por par, corpo `Motor12x36`** | Escreve `E/H/F = duracao`. `Motor12x36ComEscala` e plantão reutilizam esse corpo. O split chama `_aplicar_he_noturna`. `app/ponto/motor_calculo_v2.py:2925–2929,2971,3651–3665` |
| **Motor por mês, turno partido** | Zera `E/H/F/N` dos regulares e regrava `E/H/F = trabalhado de cada par`. Chama o split por par. O partido noturno herda o ramo. A via FT recebe mapa de dia, mas fica fora do laço que escreve o suprimido da pausa entre pares. `app/ponto/motor_calculo_v2.py:3045–3055,3067–3122,3197–3229,3677–3684` |
| **Motor comercial por par e por mês** | O par escreve `E/H/F = duracao` e chama o split. O mês zera `E/H/F/N` dos regulares, regrava `E/H/F = trabalhado de cada par` e chama o split novamente. `app/ponto/motor_calculo_v2.py:3385–3389,3411,3443–3455,3467–3478` |
| **Escritas transversais** | `PeriodoCalculo` inicializa os quatro em zero. `_aplicar_he_noturna` lê `H` e escreve `N` quando o split é positivo. O bloqueio L-097 pode zerar os quatro e o suprimido antes da partição. `app/ponto/motor_calculo_v2.py:216–225,1051–1066,1563–1594` |
| **Releitura v3** | `reler` zera `E/H/F` inclusive na FT, soma `F` regular quando há CCT e recoloca a dobra em `E/H/F` no último par regular. O reset de repetidos também cobre `E/H/F`. `N` fica fora dessas escritas. O ramo de dobra é condicionado a `_dobra > 0`, detalhe a observar no caso de dobra classificada com saldo zero. `app/ponto/calculador/periodo.py:178–183,731–739,755–775,819–843` |

| Leitor | Campos ou projeção |
|---|---|
| **Propriedades e contas do motor** | `PeriodoCalculo.horas_extra` lê `E`. `ResultadoMes` soma `H/F/N` dos regulares e deriva HE100 comum de `H−F`. O DSR usa `HE50+H−F`. O banco comercial deduz `HE50+H` como pago, portanto reduzir `H` sem carregar a parcela retirada faria o saldo subir. `app/ponto/motor_calculo_v2.py:264–265,328–344,2744–2753,3558–3572,3603–3608` |
| **Promoção, mensal e DiaPago** | A partição lê `N` e compara `F` com `T` para decidir se refaz o split da dobra cheia na FT. O mensal lê as propriedades; o DiaPago lê `H/F/N` dos regulares. Os dois projetam `H/F` promovidos a partir de `T` e o `N` a partir da partição. `detalhe['folga_feriado_min']` registra `T` para o trabalhado mostrado ao leitor. `app/ponto/calculador/regras.py:394–397,433–448` · `app/ponto/services/fechamento.py:988–1012` · `app/ponto/services/dia_pago.py:176–189,238–251,902–910` |
| **Alimentação v3** | Lê `H` para as pontas do split, `F` para a dobra regular do dia, e `H/F/N` regulares em dias com FT. Para a FT promovida, projeta `T` e o split recebido. As chamadas avulsas de `do_dia` também têm caminhos de dobra cheia. `app/ponto/calculador/alimentacao.py:101–108,353–387,518–523,571–585` · `app/ponto/calculador/regras.py:361–364,767–814,850–874,915–923,958–962` |
| **Espelho, PDF, calendário e comparação** | O resumo vivo do espelho lê `E` via `horas_extra` e lê `H` dos regulares. Na FT, soma `T` ao total vivo de extras. O PDF usa `H` bruto para presença da coluna e lê o valor de `DiaPago`; o calendário usa `minutos_extra` projetado de `HE50+HE100` lavrados. Cartão e TXT leem as rubricas em horas. `app/ponto/services/espelho.py:81–114,450–455` · `app/relatorios/pdf_espelho.py:832–862` · `app/ponto/services/dia_pago.py:922–940` · `app/colaboradores/services/calendario.py:698` · `app/relatorios/cartao_pela_celula.py:89–101,172–181` · `app/folha/export.py:434–462` |
| **Vigia e instrumentos** | `minuto_em_duas_rubricas` lê `E` dos regulares. `diff_calculador` lê `H/F/N`; `desvio_o68b` lê `H/N`; `esmeril_espelho` lê `E`. `medida_leitor_do_dia` enumera os quatro como vocabulário da inspeção AST. `app/folha/porta_export.py:541–571` · `app/ponto/management/commands/diff_calculador.py:246–248` · `app/ponto/management/commands/desvio_o68b.py:117,128` · `app/ponto/services/esmeril_espelho.py:185–186` · `app/core/medida_leitor_do_dia.py:50–83` |

## 3. Leis herdadas

- **Origem, leitor e prova.** L-001/L-002/L-007 põem a cura na fonte e a leitura na autoridade existente. L-095 exige uma lavra do dinheiro e o mesmo número nos leitores. L-099 exige coerência e idempotência. L-110 manda cobrar a regra na bateria. `app/docs/LEIS.md:59–65,113,122,136` · `app/docs/CORTES.md:383`
- **Dia e classificação.** L-085 atribui a jornada ao dia de início. A cláusula de feriado da L-086 foi revogada. O calendário e o ciclo decidem a dobra. O aval UM-JUIZ leva esse juiz à FT, usa a certeza da escala para a rubrica e mantém o 12×36 simples fora da promoção. O corte NOTURNO-A alinha a janela de HE ao dia do turno. O corte semanal do CADERNO trata a opção de cadastro “trabalha em feriado” nas escalas semanais. `app/docs/LEIS.md:117–118` · `app/docs/CORTES.md:616,646` · `FERIADO-UM-JUIZ-2711-1210.txt:1` · `app/ponto/calculador/regras.py:410–432`
- **Dinheiro da dobra.** O aval inicial retira da HE100 o minuto já indenizado como intra. C1 fixa `S` inteiro, diurnos primeiro, adicional noturno apurado pela régua vigente, banco comercial congelado no dia em dobra e cálculo por dia mesmo se `S` exceder um par. C1 reserva F10h à família INTERVALO, manda contar o padrão no DIFF e pede S1–S5/C1–C7 na bateria com o oráculo intocado. `feriado_sem_limite_do_suprimido_183x.txt:1` · `FERIADO-C1-LEIS-2511.txt:1` · `app/docs/BACKLOG.md:2013–2014` · `app/docs/RELATO.md:425`
- **K1 e L-097.** O `!` K1 escolheu a opção **(b)**, uma apuração de intervalo no motor para regular e `periodos_ft`, no trecho autorizado. O aval condiciona o pouso ao DIFF por rubrica e manda parar com número e lista se aparecer mudança fora de feriado de folga. O HEAD ainda carrega o bloqueio pré-partição da L-097, registrado para obras próprias. `FERIADO-K1-MOTOR-2611.txt:1` · `app/ponto/motor_calculo_v2.py:1563–1594,3067–3122` · `app/docs/LEIS.md:62,115` · `app/docs/BACKLOG.md:2190–2191,2201`
- **Saída.** DIFF de frota da comp. 11 por rubrica no RELATO antes do pouso, vigia zero nas emp2/3/4 e competência exportada sob a guarda da L-092. Se a cura perder o TXT, o aval destina os dois dias à Pauta DP. O anexo relata `emp2 = 2`. O zero pós-apply, a contagem isolada do vigia na emp4 e o DIFF desta volta ficam [nao verificado]. `app/docs/LEIS.md:62,125` · `app/docs/BACKLOG.md:2014` · `feriado_sem_limite_do_suprimido_183x.txt:1` · `estado1310.md:1`

## 4. Contrato de entrada do limite

| Dimensão | Contrato |
|---|---|
| **Fonte e unidade** | `T = Σ minutos_trabalhados` dos pares apurados da dobra. `S = Σ minutos_intrajornada_indenizada` desses pares, já julgados pela origem. Ambos em **minutos de relógio**, com frações. O intervalo gozado já compõe a apuração de `T`. O limite consome a indenização que o motor apurou, inclusive a de K1. `app/ponto/motor_calculo_v2.py:2872–2898,3078–3122,3340–3372` · `FERIADO-C1-LEIS-2511.txt:1` |
| **Universo** | Colaborador × dia da jornada × pares classificados em dobra, com o vínculo e a régua da fatia correta. A via regular e a FT promovida entregam as parcelas desse dia uma vez. A janela da competência vem do corte da empresa. Na porta, o universo de emissão vem de `classificar_export`. `app/docs/LEIS.md:76–77,117` · `app/ponto/janelas.py:36–57` · `app/ponto/services/fechamento.py:344–383` · `app/ponto/calculador/regras.py:410–448` · `app/folha/porta_export.py:475–482` |
| **Exclusões** | Dia comum, feriado simples, 12×36 simples, folga da rubrica própria e intermitente ficam fora da conta da dobra. Par sem trabalho apurado contribui zero. O 12×36 com cláusula cadastrada pode chegar em dobra no ramo regular do motor. A promoção do UM-JUIZ tem a guarda própria do ciclo simples. `app/ponto/motor_calculo_v2.py:1563–1594,3208–3218,3694–3698,3740,3810–3811` · `app/core/regua_cct.py:473–479` · `app/ponto/calculador/regras.py:371–377,410–432` |
| **Resposta** | `B = max(0, T − S)`. Na dobra, `E = H = F = B`, com HE50 zero; a intra continua `S`. Com `N₀` de relógio na dobra cheia, `N = min(N₀, B)` e a parcela diurna é `B − N`. A passagem distribui o corte pelo dia antes de atingir o noturno. O noturno legal continua vindo da apuração própria. Para o excedente comum, a fórmula vigente é `previsto + min(S, declarado)`. `FERIADO-C1-LEIS-2511.txt:1` · `app/ponto/motor_calculo_v2.py:1034–1049,1898–1916,3197–3229` · `app/folha/export.py:434–462` |
| **Escala de saída** | Somar em minutos antes de converter em horas. DiaPago armazena horas em `FloatField`; o mensal grava duas casas e o TXT emite centésimos. `folga_feriado_min` continua representando o trabalhado da FT para a leitura do dia. O vigia usa a classificação e o dia julgados para escolher o teto, e compara a HE observada com esse teto. `app/ponto/models.py:330–332,417–419` · `app/ponto/services/dia_pago.py:238–251,562–575,902–910` · `app/ponto/services/fechamento.py:988–1008` · `app/folha/export.py:34–45` · `app/folha/porta_export.py:541–571` |

## 5. Matriz dos REDs descritos

| Caso e entrada | Resposta da regra | Sítio que responde diferente ou controle a cobrar |
|---|---|---|
| **col243, 15/11, emp2.** `T=436,3`, `S=16,3`. | **420,0 min** de HE100 e HE100 feriado. **16,3 min** na intra. | O dado relata 436,3 na dobra cheia. Os quatro escritores e a promoção usam `T`. Ramo concreto, batidas, previsto, declarado e split do caso [nao verificado]. `app/docs/RELATO.md:425` · `app/docs/BACKLOG.md:2014` · `app/ponto/motor_calculo_v2.py:2925–2929,3213–3220,3385–3389,3471–3478` · `app/ponto/services/fechamento.py:1001–1008` |
| **col1046, 20/11, emp2.** `T=479,7`, `S=60`. | **419,7 min** de HE100 e HE100 feriado. **60 min** na intra. | O dado relata 479,7 na dobra cheia. A projeção promovida também usa `T`. Ramo concreto e parcelas do caso [nao verificado]. `app/docs/RELATO.md:425` · `app/docs/BACKLOG.md:2014` · `app/ponto/services/dia_pago.py:238–246` |
| **K1.** `10:00–14:00` + `14:30–18:20`. `T=470`, pausa julgada 30, mínimo 60. | **440 HE100 + 30 intra**, tanto regular quanto FT promovida. Com pausa julgada de 60 e o mesmo `T`, **470 HE100 + 0 intra**. | Na regular o código apura 30 e grava 470 na dobra. Na FT, o laço de intervalo fica fora e a promoção usa 470. O RED existente cobra `7,33 h / 0,50 h` na promoção. `app/ponto/motor_calculo_v2.py:3067–3122,3197–3220` · `app/ponto/services/fechamento.py:1001–1008` · `app/ponto/tests/test_feriado_um_juiz_porta.py:380–394` · `FERIADO-K1-MOTOR-2611.txt:1` |
| **Noturno e dois pares.** Entrada sintética já apurada com `T=510`, `N₀=420` em um par e 90 diurnos no outro, `S=60`. | **450 HE100 = 30 diurnos + 420 noturnos**. Controle de saturação com `T=450`, `N₀=420`, `S=60`: **390 HE100 noturnos**, com o adicional calculado sobre o noturno legal original. | Hoje os ramos gravam `T`. A promoção mede o split sobre a dobra cheia e o limita por `T`. O split comum se ancora no fim do par, razão para cobrar o corte diurno no conjunto do dia. `FERIADO-C1-LEIS-2511.txt:1` · `app/ponto/motor_calculo_v2.py:409–451,3213–3220,3471–3478` · `app/ponto/calculador/regras.py:433–448` |
| **Por dia, com `S` maior que um par.** Pares de 400 e 120 min, `S=420`. Segundo controle com `S≥520`. | **100 min** na primeira geometria e **0** na segunda. A classificação do dia continua sendo de dobra, inclusive quando o valor chega a zero. | Os escritores atuais gravam 520. Uma subtração truncada por par daria 120 na primeira geometria. O v3 usa positividade do valor para entrar no ramo da dobra. `FERIADO-C1-LEIS-2511.txt:1` · `app/docs/RELATO.md:425` · `app/ponto/motor_calculo_v2.py:3213–3220,3471–3478` · `app/ponto/calculador/periodo.py:819–843` |
| **12×36 simples, fora.** Feriado do ciclo sem cláusula, inclusive em folga. | **Zero HE100 feriado**. O controle de intervalo F10h só entra na contagem do padrão para o DIFF. | Controle negativo. O ciclo simples já fica fora e a promoção tem guarda explícita. F10h tem obra própria. `app/ponto/motor_calculo_v2.py:3740,3810–3811` · `app/ponto/calculador/regras.py:415–432` · `app/ponto/tests/test_feriado_um_juiz_porta.py:298–305` · `app/docs/BACKLOG.md:2013` |
| **Dia sem feriado.** Mesmas batidas, previsto, declarado e `S`, antes e depois. | Delta **zero por rubrica**, inclusive banco e intra. | Controle negativo. O ramo comum usa o teto próprio; há controle de competência sem feriado para o arredondamento do mensal. `app/ponto/motor_calculo_v2.py:1898–1916,3258–3274,3525–3538` · `app/ponto/tests/test_feriado_um_juiz_porta.py:421–433` · `FERIADO-K1-MOTOR-2611.txt:1` |
| **Vigia pela `medir` real.** Colaborador emitível, `T=480`, `S=60`, previsto 420, declarado 60. Rodar com HE 480 e com HE 420. Repetir com promoção e cauda de D+1. | Acusa **480** e aceita **420** na dobra. A cauda integra a jornada de início e a FT promovida entra na mesma pergunta. | Hoje o teto comum é **0** nessa entrada, portanto acusaria ambas na via regular. A leitura de `periodos` exclui a promoção e `.entrada.date()` pode separar a cauda. A outra testemunha da porta já tem a leitura do mapa de jornada. `app/folha/porta_export.py:100–121,475–482,541–571` · `app/docs/LEIS.md:117` |
| **Partes somam o total e idempotência.** Múltiplos pares, `S=0`, `0<S<T` e `S≥T`. Reapurar e relavrar duas vezes com os mesmos fatos. | `E=HE50+H`, `F=H` na dobra, `0≤N≤H` e `diurno+N=H`. Totais e linhas iguais nas duas rodadas. No comercial, o corte em HE100 produz **delta zero** no banco. | Hoje o total fica em `T`. O banco subtrai a HE gravada, ponto onde um corte isolado mudaria o saldo. A lavra substitui as linhas da versão em transação e já há controle de relavra fracionária. Prova da nova conta [nao verificado]. `app/ponto/motor_calculo_v2.py:3213–3220,3471–3478,3558–3572` · `app/ponto/services/dia_pago.py:349–373` · `app/ponto/tests/test_o280_bateria_da_lei.py:2880–2914` · `app/ponto/tests/test_feriado_um_juiz_porta.py:396–419` |
| **Todo leitor lê o mesmo número, incluindo K2.** Dobra regular e promovida, com segundos, comparada por dia, rubrica e competência. | Motor, partição, mensual, DiaPago, leitores e TXT concordam na unidade de cada saída. Conferir também o centésimo na soma da lavra × mensal. | A projeção FT do v3 e o resumo vivo ainda usam `T`. A porta atual tolera até `0,015 h` nessa comparação. O `1,07 × 1,06` está relatado na volta antiga; reprodução nesta cópia [nao verificado]. `app/ponto/calculador/alimentacao.py:353–387` · `app/ponto/services/espelho.py:95–114` · `app/ponto/services/dia_pago.py:562–575` · `app/folha/porta_export.py:245,457–464` · `app/docs/RELATO.md:343` |

## 6. Arquivos da diferença × obras citadas

Lista para a construção, com as menções conferidas nas linhas correspondentes do BACKLOG. Menção a arquivo é evidência de referência, e concorrência ativa de escrita fica [nao verificado].

| Arquivo | Obras que o BACKLOG cita |
|---|---|
| `app/ponto/motor_calculo_v2.py` | O214, FOLGA-TRABALHADA-TRES-NUMEROS, O251, O254, O263, O281, O296, RETIFICACAO-DA-10-8069-PRIMEIRO, INTERVALO-12X36-PAUSA-ENTRE-PARES e V2-I2-FILA. Na fila de feriado, NOTURNO-A, FERIADO-L097-MOTOR, FERIADO-L097-HERANCA-D1 e FERIADO-K1-MOTOR. `app/docs/BACKLOG.md:1308,1420,1623,1626,1688,1708,1723,1808,2002,2013,2190–2191,2201,2335` |
| `app/ponto/calculador/regras.py` | S5b-TROCA-NAO-CRIA-DESCONTO e O263. O FERIADO-L097-HERANCA-D1 cita a partição diretamente. `app/docs/BACKLOG.md:1425,1688,2191` |
| `app/ponto/services/fechamento.py` | V3-RETOMA-FILA, O-DIA-PAGO, FOLGA-TRABALHADA-TRES-NUMEROS, S5b-TROCA-NAO-CRIA-DESCONTO, O181, O184, FURO-APURADO-FALSO-EM-20378-LINHAS, REVERSAO-DA-10-NAO-COBRE-O-CARIMBO-DO-ESPELHO, EFEITO-NONE-UM-LEITOR, ESMERIL-SEM-INSTANTE, RETRATAR-COMP-DO-TURNO, FERIADO-UM-JUIZ e COMPETENCIA-PAGA-UM-JUIZ. O282 e REGUA-DO-PERIODO-COMPLEMENTO-19-10 citam o fechamento pelo caminho `folha/services/fechamento.py`. `app/docs/BACKLOG.md:1112,1267,1420,1425,1576,1579,1709,1766,1775,1782,1966,2144,2162,2189,2331` |
| `app/ponto/services/dia_pago.py` | O-DIA-PAGO S3, FURO-APURADO-FALSE-EM-VEZ-DE-NONE, V3-IMPACTO-COM-DONO, ABONO-LE-O-CATALOGO, RELAVRA-11-DOS-50 e JUSTIFICATIVA-PREVISTO-ATA-ANTIGA. O antecedente da lavra está em O-DIA-PAGO. `app/docs/BACKLOG.md:1260,1267,1794,1950,2170,2226,3108` |
| `app/ponto/calculador/alimentacao.py` | O281. O arquivo entra na diferença como projeção dos valores já julgados da FT. `app/docs/BACKLOG.md:1708` · `app/ponto/calculador/alimentacao.py:353–387` |
| `app/folha/porta_export.py` | O-DIA-PAGO S3, O214, O293, FURO-APURADO-FALSO-EM-20378-LINHAS, CONTADOR-ELEGIVEIS-DA-11, V2-I2-FILA e ATOMICIDADE-PORTA-NO-MAIN. `app/docs/BACKLOG.md:1260,1308,1720,1766,2228,2335,2458` |
| `app/ponto/tests/

[truncado em 20000 de 21538 caracteres]

## 28/11 13:12 · sessao 768f1e997dbb · FERIADO-SEM-LIMITE-DO-SUPRIMIDO · leitura · origem=Code

**Resumo**  
O HEAD ainda paga o trabalhado inteiro nos quatro ramos de dobra do motor e na promoção da folga-feriado. O FERIADO-UM-JUIZ resolveu a classificação, e o K1 segue aberto. `app/ponto/motor_calculo_v2.py:2925,3208,3385,3467` · `app/ponto/services/fechamento.py:995–1012` · `app/docs/BACKLOG.md:2201`  
Pela regra da obra, a HE100 do col243 em 15/11 é **420,0 min** e a do col1046 em 20/11 é **419,7 min**. `app/docs/BACKLOG.md:2014` · `app/docs/RELATO.md:425`  
**Veredito (a)** — refazer a diferença numa cópia nova do HEAD. Entregar motor, promoção e vigia com a mesma conta, depois medir a comp. 11. `app/ponto/calculador/regras.py:386–453` · `app/folha/porta_export.py:541–571` · `ordem_viva_2811_1310.txt:1`  
O selo fornecido registra **emp2 = 2**. Zero pós-apply e DIFF restrito aos dois dias **[nao verificado]**. `ordem_viva_2811_1310.txt:1`

## 1. Cobertura no HEAD

| Sítio | Onde o suprimido entra hoje |
|---|---|
| **Motor, 12x36 e plantão** | A dobra por par grava `duracao` inteira em `minutos_extra`, HE100 e HE100 feriado. O caminho de excedente usa `_limite_com_suprimido` quando há previsto. `Motor12x36ComEscala` e plantão passam pelo mesmo corpo. O ciclo 12x36 é simples por padrão e pode ter cláusula de dobra. O intermitente limpa o calendário do motor. `app/ponto/motor_calculo_v2.py:2925–2972,3651–3665,3694–3698,3740,3810–3811` · `app/core/regua_cct.py:473–479` |
| **Motor, turno partido** | Na via regular, o laço indeniza a pausa curta no par seguinte, mas a dobra grava o trabalhado inteiro de cada par e sai antes do limite diário. O excedente diário usa `_limite_com_suprimido`. A via semanal de 44 h exclui os feriados da soma. Para `periodos_ft` o método preenche o mapa de dia, e a apuração da pausa curta só percorre `resultado.periodos`. `app/ponto/motor_calculo_v2.py:3059–3122,3197–3239,3258–3260,3277–3287` |
| **Motor, comercial** | `calcular_periodo` grava `duracao` inteira na dobra. `calcular_mes` zera as extras regulares do par e grava de novo o trabalhado inteiro na dobra do dia. O limite com suprimido aparece nos dois caminhos de excedente. No mês, o feriado simples segue para esse limite. `app/ponto/motor_calculo_v2.py:3385–3411,3444–3499,3525–3526` |
| **`fechamento.py` e `dia_pago.py`** | Na via regular, o mensal e a lavra leem os campos de HE100 do motor. Na promoção, o mensal soma `f.periodo.minutos_trabalhados` e a lavra soma `p.minutos_trabalhados`. Ambos acrescentam o split noturno da partição. A intra viaja em rubrica própria. `app/ponto/services/fechamento.py:988–1012` · `app/ponto/services/dia_pago.py:176–189,217–219,238–251` · `app/ponto/motor_calculo_v2.py:328–344,362–370` |
| **`calculador/regras.py`** | O excedente usa `previsto + min(S, declarado)`. A dobra avulsa devolve o trabalhado inteiro e a folga classificada avulsamente também atribui o trabalhado inteiro à rubrica escolhida. Na comp. 11, o caminho alimentado recebe a dobra normal do motor e a partição da folga. Nessa projeção, a promoção ainda usa o trabalhado inteiro. `app/ponto/calculador/regras.py:361–364,767–788,843–874` · `app/ponto/calculador/alimentacao.py:353–387,518–523,571–585` · `app/folha/export.py:385` |
| **`calculador/periodo.py`** | `excesso_do_dia` tem o equivalente ao limite comum. `reler` soma a HE100 feriado já gravada pelos pares regulares e recoloca a soma no último par. O suprimido que calcula entra no excedente. A dobra é herdada do motor. `app/ponto/calculador/periodo.py:387–417,755–775,820–843` |
| **`folha/porta_export.py`** | O vigia aplica a fórmula do excedente comum também ao feriado em dobra. O teto específico `max(0, T − S)` está ausente. `app/folha/porta_export.py:541–571` |

O **FERIADO-UM-JUIZ** cobriu a decisão de **qual dia e qual rubrica** na promoção, com contexto por fatia, juiz do ciclo, escala certa e corte do intermitente. Também deixou o RED de K1 tagueado. O valor da promoção continua no trabalhado inteiro, e o bloqueio L-097 do motor continua em outro ponto. O pouso `a497c0cce` consta do RELATO. `app/ponto/calculador/regras.py:371–448` · `app/ponto/services/fechamento.py:350–351,995–1012` · `app/ponto/tests/test_feriado_um_juiz_porta.py:380–394` · `app/ponto/motor_calculo_v2.py:1563–1590` · `app/docs/RELATO.md:15239` · `app/docs/BACKLOG.md:2189–2190,2201`

## 2. Hunks de `obra.diff` contra a cópia

Os números `−N` são os inícios dos hunks no anexo. A classificação abaixo é por confronto textual com os sítios atuais. O arquivo veio sem `.git`, portanto hash, ancestralidade e resultado de `git apply --check` ficam **[nao verificado]**.

| Arquivo | Ainda encaixa | Conflita ou perdeu a função |
|---|---|---|
| `app/ponto/motor_calculo_v2.py` | **Os dez sítios** mantêm o contexto relevante. `−723` e `−1818` inserem os helpers junto a `:750–755` e `:1916–1918`. `−2578` entra antes do DSR em `:2674–2682`. `−2826/−2871` entram em `:2923–2972`. `−3112` entra em `:3208–3230`. `−3286/−3311` entram em `:3382–3414`. `−3358/−3370/−3461` entram em `:3455–3495,3558–3565`. | **K1 continua faltando ao diff**, mesmo que os dez hunks sejam reaproveitados. O laço atual de intra do partido só escreve na via regular. `app/ponto/motor_calculo_v2.py:3067–3122` |
| `app/folha/porta_export.py` | `−316` e `−544` encaixam na extração e chamada do vigia. `−335`, `−458` e `−644` também têm âncoras atuais. `app/folha/porta_export.py:316–319,332–341,457–464,544–572,644–650` | Os três últimos hunks citados são de **K2** — importação, tolerância e comentário sobre a soma. O HEAD ainda usa `_TOL_H` nessa comparação. O comentário novo depende da mudança de agregação, e esses hunks por si só deixam o teto do vigia como está. `app/folha/porta_export.py:245,338,457–464,566–568` · `app/ponto/services/dia_pago.py:562–575` |
| `app/folha/tests/test_porta_do_export.py` | `−131` encaixa no selo que inspeciona a fórmula dentro de `medir`. `app/folha/tests/test_porta_do_export.py:130–142` | A troca de âncora acompanha a extração. A prova dos dois comportamentos passa pela `medir` real. `app/folha/porta_export.py:475–482,541–571,693–701` |
| `app/ponto/services/dia_pago.py` | `−25`, `−141`, `−162`, `−170`, `−179`, `−291` e `−547` têm âncora. São imports/campos, preparação e soma de parcelas, e agregação de K2. `app/ponto/services/dia_pago.py:23–44,145–189,296–305,558–575` | `−132` conflita com `RecusaDeProcedencia` e introduz a assinatura antiga de promoção. `−241` conflita com a lavra atual de `(p, split_noturno, legais)`. Reaproveitar a conta na partição atual preserva os valores noturnos vindos da fatia. `app/ponto/services/dia_pago.py:132–145,217–219,235–251` |
| `app/ponto/services/fechamento.py` | `−69`, só a redação da docstring, ainda encaixa. `app/ponto/services/fechamento.py:82–85` | `−77/−207/−298/−327` colidem com os mapas de contexto vigentes. O mapa paralelo `prorrogacao_por_par` ficou obsoleto nesse desenho, pois a partição já recebe motor e vínculo. `−925/−943/−956` conflitam com o recuo e a nova partição, e o filtro antigo `_ft_certa → feriado_vira_folga` foi superado. `−1214/−1227/−1290/−1297` devem ser refeitos nas duas chamadas atuais, com a partição e a transação que o HEAD já usa. A soma do trabalhado inteiro em `:1002` segue sendo a diferença a corrigir. `app/ponto/services/fechamento.py:91–98,249–257,344–383,988–1012,1228–1254,1284–1317` |
| `app/core/tests/test_medida_turno_do_par.py` | No `−284`, a entrada da porta ainda aponta para `544`, mas o contexto ao redor mudou. `app/core/tests/test_medida_turno_do_par.py:287–293` | `−255/−300/−326/−345` conflitam com coordenadas novas. Os destinos antigos de **todos os cinco hunks** ficaram obsoletos. Exemplos do HEAD são `909/2521` no motor e `1609/257` no fechamento. O brief pede só o mapa de deslocamentos da futura alteração. `app/core/tests/test_medida_turno_do_par.py:253–262,304–315,329–352` · `brief_c4e.md` |
| `app/ponto/tests/test_motor.py` e `test_s6_intermitente_feriado.py` | Encaixam `−534/−544` e `−166`, respectivamente. Os controles ainda esperam **28 h** nos dois feriados e **12 h** no 12x36 com dobra ligada. `app/ponto/tests/test_motor.py:534–549` · `app/ponto/tests/test_s6_intermitente_feriado.py:168–177` | A diferença a cobrar nas mesmas geometrias é **26 h** e **11 h**, respectivamente, com a intra indenizada em rubrica própria. `app/docs/BACKLOG.md:2014` · `obra.diff` |
| `app/ponto/tests/test_o280_bateria_da_lei.py` | `−74/−210/−302/−330/−1592/−1726/−1752/−6048` mantêm as âncoras de import, matriz, casos, F10h, F11c e fim da classe. `app/ponto/tests/test_o280_bateria_da_lei.py:73–78,209–215,300–308,328–335,1593–1630,1720–1762,6045–6051` | A expectativa F11c deve ser conferida pela regra no caminho vigente. A fixture antiga usa outubro, enquanto a partição nova começa em novembro. F10h continua sendo caracterização de uma falha de intervalo com obra própria. `app/ponto/tests/test_o280_bateria_da_lei.py:1740–1753` · `app/ponto/calculador/regras.py:393–409` · `app/folha/export.py:385` · `app/docs/BACKLOG.md:2013` |
| Três arquivos novos em `app/ponto/tests/` | `test_feriado_sem_limite_do_suprimido.py`, `test_feriado_dobra_noturna_g0.py` e `mutantes_feriado_sem_limite_do_suprimido.py` entram como adições, sem arquivo homônimo visível na cópia. `obra.diff` · `app/ponto/tests/test_feriado_um_juiz_porta.py:380–394` | Reaproveitar os cenários. O K1 já tem entrada no HEAD, e os mutantes da cópia velha dependem de nomes, fontes e revisões daquele desenho. Execução desses mutantes no HEAD **[nao verificado]**. `app/ponto/tests/test_feriado_um_juiz_porta.py:380–394` · `commits.txt` · `obra.diff` |

## 3. Valores e REDs

A conta aprovada é **HE100 = HE100 feriado = `max(0, T − S)` no dia da jornada**. Deduz-se todo o `S` indenizado, começando pelos minutos diurnos de todos os pares. Preserva-se a HE100 noturna da dobra cheia até o limite da HE100 restante, e o adicional noturno conserva sua apuração. No comercial, o que sai da dobra continua contado como pago para o banco. O helper de excedente atual soma apenas `min(S, declarado)` ao previsto, portanto chamá-lo com previsto zero ainda produziria uma conta diferente. `app/docs/BACKLOG.md:2014` · `FERIADO-C1-LEIS-2511.txt:1` · `app/ponto/motor_calculo_v2.py:1898–1916,3558–3570`

| RED | Trabalhado no dado | Intra indenizada | HE100 esperada | HE100 inteira relatada |
|---|---:|---:|---:|---:|
| **col243 · 15/11** · pausa curta | 436,3 min | 16,3 min | **420,0 min** | 436,3 min |
| **col1046 · 20/11** · sem pausa | 479,7 min | 60,0 min | **419,7 min** | 479,7 min |

Os valores vêm do relato da obra. O primeiro caso também aparece no diff com 436,33 e 16,33, cuja diferença continua 420,0. O ramo exato, as batidas, o previsto, o declarado e o split noturno de cada um dos dois dias **[nao verificado]**. Se o selo rodou esse mesmo `medir`, a acusação veio de pares em `resultado.periodos`, que é a lista que o vigia percorre. `app/docs/RELATO.md:425` · `obra.diff` · `app/folha/porta_export.py:538–550`

- **Dois REDs do dinheiro.** Cobrar os totais da tabela na HE100 e HE100 feriado, com a intra em sua própria rubrica. Os quatro ramos de dobra do motor hoje escrevem o valor da última coluna. Exercitar também a promoção, que hoje soma o trabalhado inteiro no mensal e no DiaPago. No TXT, conferir as duas rubricas pelos campos do mensal. `app/ponto/motor_calculo_v2.py:2925–2929,3213–3220,3385–3389,3471–3478` · `app/ponto/services/fechamento.py:1001–1008` · `app/ponto/services/dia_pago.py:238–246` · `app/folha/export.py:434–462`
- **RED K1.** `10:00–14:00` + `14:30–18:20` dá **470 trabalhados**, **30 de intra** com mínimo de 60, e **440 HE100** em ambas as vias. O **440 + 30** da via regular descrito no brief é da c4d. No HEAD, a via regular já apura a intra, mas ainda grava a dobra inteira. A promoção tem o segundo defeito, pois o laço da pausa fica fora de `periodos_ft`. O RED existente cobra **7,33 h de HE100 / 0,50 h de intra**. Os controles do brief cobram gap suficiente com intra zero nas duas vias e dia fora de feriado sem mudança de rubrica. `app/ponto/motor_calculo_v2.py:3067–3122,3197–3220` · `app/ponto/tests/test_feriado_um_juiz_porta.py:380–394` · `app/docs/BACKLOG.md:2201` · `brief_c4e.md`
- **RED de dia e noturno.** Um par com 400 trabalhados e `S=420`, mais outro com 120, deve dar **100 min** no dia. Se houver diurnos suficientes em outro par, o desconto preserva o split noturno da dobra cheia. F10h tem dono próprio para a pausa entre pares, e o DIFF deve contar os dias desse padrão. `app/ponto/tests/test_o280_bateria_da_lei.py:1587–1630` · `app/docs/BACKLOG.md:2013` · `FERIADO-C1-LEIS-2511.txt:1` · `obra.diff`

## 4. Próxima volta e arquivos

**(a) Refazer no HEAD.** Os hunks do motor ainda servem de referência. A partição da folga, a identificação da fatia, o split de plantão longo e a lavra já têm contratos no HEAD que a cópia antiga antecede. A menor mudança é fazer esses leitores consumirem a conta comum da dobra no contrato atual. `app/ponto/services/fechamento.py:350–383,995–1012` · `app/ponto/calculador/regras.py:410–448` · `app/ponto/tests/test_feriado_um_juiz_porta.py:592–628`

**ARQUIVOS:**

- **Opus, motor e bateria.** `app/ponto/motor_calculo_v2.py` para os quatro escritores, a conta por dia, o congelamento do banco comercial e a extração K1 onde hoje está o laço. `app/ponto/tests/test_o280_bateria_da_lei.py` para S1–S5 e C1–C7, `app/ponto/tests/test_feriado_um_juiz_porta.py` para o RED K1 existente, e os dois controles que fixam a dobra cheia em `app/ponto/tests/test_motor.py` e `app/ponto/tests/test_s6_intermitente_feriado.py`. Entregar o mapa de deslocamentos da catraca para conferência. `app/ponto/motor_calculo_v2.py:2677–2678,2925,3078–3122,3208,3385,3467,3561–3570` · `app/ponto/tests/test_o280_bateria_da_lei.py:209–215` · `app/ponto/tests/test_feriado_um_juiz_porta.py:380–394` · `app/ponto/tests/test_motor.py:534–549` · `app/ponto/tests/test_s6_intermitente_feriado.py:168–177` · `brief_c4e.md`
- **Codex construtor, app e teste da porta.** `app/ponto/calculador/regras.py` no contrato de `particionar_ft`, para transportar a HE100 apurada junto do split, `app/ponto/services/fechamento.py` e `app/ponto/services/dia_pago.py` para consumir esse valor, e `app/folha/porta_export.py` com `app/folha/tests/test_porta_do_export.py` para o teto e o RED da porta. Para a lavra do oráculo da comp. 11 ler a HE100 **promovida** corrigida, há ainda `app/ponto/calculador/alimentacao.py:353–377`, que hoje projeta o trabalhado cru. Eu trataria esse ponto como adaptação da leitura da partição, com a regra independente do oráculo preservada conforme C1. Se “oráculo intocado” também abranger essa projeção, a paridade ali fica por resolver. `app/ponto/calculador/regras.py:380–448` · `app/ponto/services/fechamento.py:1000–1012,1292–1317` · `app/ponto/services/dia_pago.py:238–251,331–339` · `app/folha/tests/test_porta_do_export.py:130–142` · `FERIADO-C1-LEIS-2511.txt:1`
- **K2 na verificação.** O arredondamento fracionário já tem controles no FERIADO-UM-JUIZ, mas isso não comprova igualdade centesimal mensal × soma da lavra. O HEAD ainda soma horas gravadas e compara com tolerância de `0,015 h`. Cobrar o centésimo no RED e usar o DIFF para decidir se os hunks de parcelas do c4d são necessários. Resultado dessa prova **[nao verificado]**. `app/ponto/tests/test_feriado_um_juiz_porta.py:396–433` · `app/ponto/services/dia_pago.py:562–575` · `app/folha/porta_export.py:245,457–464`

## 5. Vigia e critério de saída

**Sim.** Para cada data civil de entrada em `resultado.periodos`, `medir` soma HE, intra e trabalhado. Quando HE e intra são positivas, consulta previsto e declarado e acusa se **`HE > max(0, T − previsto − min(S, declarado)) + 1 min`**. O resultado entra na contagem de falhas da porta. A fórmula pode acusar até a dobra já corrigida quando o teto comum fica abaixo de `T − S`. `app/folha/porta_export.py:538–571,693–701`

A volta deve entregar, **no mesmo ato**, a HE100 corrigida no motor e na promoção, e o teto **`max(0, T − S)`** para a dobra no vigia. O RED do vigia precisa rejeitar `HE=T` e aceitar `HE=T−S` com a mesma intra, pela `medir` real e com o colaborador no universo do TXT. Para os dias comuns, a referência segue sendo o teto atual. Se a validação incluir promoção ou cauda após meia-noite, o vigia precisa ler a classificação e o dia já julgados: hoje ele enxerga só `periodos` e agrupa por `_p.entrada.date()`. `ordem_viva_2811_1310.txt:1` · `app/folha/porta_export.py:475–482,538–571` · `app/ponto/calculador/regras.py:410–448`

O pronto é o DIFF da comp. 11 por rubrica no RELATO, limitado aos minutos da obra, e `selo_leitores_no_mesmo_numero --mes 11 --ano 2026` **pós-apply**, com emp2 em zero conforme a ordem e o vigia conferido nas emp2/3/4 conforme o BACKLOG. O `9` da emp4 no selo fornecido é o valor relatado da porta com HE pendente do admin. A contagem isolada desse vigia na emp4 **[nao verificado]**. Sem pouso antes do TXT, a saída registrada para os dois dias é **Pauta DP**. `app/docs/BACKLOG.md:2014` · `app/folha/porta_export.py:693–705` · `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:218–220` · `ordem_viva_2811_1310.txt:1`

## 28/11 12:59 · sessao b8bfa6617f2b · PUSH-2811 · verificador · origem=Code

- **L-085 — RESPEITA no diff.** A jornada pertence ao dia de início, inclusive após meia-noite. O pouso `e216b756a` acrescenta a reapresentação de uma batida histórica ao mesmo pareador. `app/docs/LEIS.md:117`, `app/docs/RELATO.md:256`, `app/ponto/turnos.py:1920`
- **L-113 — VIOLA.** Exportada fora da guarda recalcula com trilha, TXT anterior preservado, DIFF prévio para ato de frota e aviso ao DP com os dois valores se mudar. `a1377afb4` muda a partição de feriado, `5792a7bfe` torna a lavra atômica e `8639de6ca` habilita propagar falhas na revisão. A escrita genérica pode mudar valores sem emitir o aviso exigido. `app/docs/LEIS.md:155–156`, `commits.txt:4–8`, `app/ponto/services/fechamento.py:604–612`, `app/ponto/services/fechamento.py:1197–1206`
- **L-120 — RESPEITA no diff.** Faturar exige 20/20 e 15 invariantes medidas na frota aberta, com zero violações estruturais por uma sequência longa de rodadas. O pouso `a8623781e` acrescenta vagas Codex e `8ea3d525c` acrescenta a fotografia de disco ao ESTADO. `app/docs/LEIS.md:141`, `commits.txt:10`, `commits.txt:14`, `bin/gerar_estado.py:283–287`, `bin/gerar_estado.py:320–321`
- **GERAL — BARRA O PUSH.** Citar as três leis é honesto como registro dos toques. A citação satisfaz o teste textual, e a violação da cláusula de aviso da L-113 continua. `bin/tests/test_lei_protege_sitio.sh:175–189`, `app/docs/LEIS.md:156`, `app/ponto/services/fechamento.py:604–612`

- **L-085.** “Hora depois da meia-noite herda o dia em que o plantão começou.” O diff acrescenta `batidas.append(reapresentada)` e entrega a lista a `_turnos_do_colab_puro`. O pareador produz `data_turno` por `_data_do_turno`. `app/docs/LEIS.md:117`, `app/ponto/turnos.py:318`, `app/ponto/turnos.py:1920–1928`, `app/ponto/turnos.py:2005–2008`
- O selo escrito espera que a saída reapresentada às 00:02 pertença à véspera. O efeito de reapresentar uma só marca quando as duas foram retratadas está em aberto no backlog. Impacto em produção [nao verificado]. `app/ponto/tests/test_lavrar_estoque_par_relampago.py:261–270`, `app/docs/BACKLOG.md:2186`
- **L-113.** “Fora dela, exportada recalcula pelo caminho genérico” e “toda exportada que mudar gera aviso ao DP com os dois números”. O filtro genérico exige `_guarda and _exportadas`, e a única linha da guarda é 09/2026. O desvio do C13 entregue para Pauta DP tem aval próprio. `app/docs/LEIS.md:156`, `app/ponto/services/fechamento.py:37–51`, `app/ponto/services/fechamento.py:623–629`, `app/docs/PROMPTS.md:3008`
- O diff passa a usar `_particao_ft = particionar_ft(...)` e grava a nova rubrica via `.update(...)`. No caminho genérico acionado por evento, o `WARNING` de exportada antecede o cálculo e traz competência, empresas e motivo. A escrita pode terminar sem comparar os valores ou avisar o DP. Ocorrência em produção [nao verificado]. `app/ponto/services/fechamento.py:604–612`, `app/ponto/services/fechamento.py:995–1008`, `app/ponto/services/fechamento.py:1197–1206`, `app/ponto/services/fechamento.py:1847–1853`
- `a8eec39b3`, `a9be3ee84` e `0516d86be` tratam, respectivamente, da contagem após falha, do savepoint do oráculo e da distinção entre recusa de procedência e falha técnica. `commits.txt:1–5`, `app/ponto/services/fechamento.py:1274–1287`, `app/ponto/services/fechamento.py:1320–1339`
- **L-120.** A lei pede “N/15 em zero e a contagem de rodadas limpas seguidas”. As adições `_linha_cv(_ler_cv(raiz))` e `_linha_disco(raiz)` publicam telemetria operacional. O placar de faturamento segue registrado como trabalho a construir. Faturamento efetivo [nao verificado]. `app/docs/LEIS.md:141`, `app/docs/BACKLOG.md:1678–1679`, `bin/gerar_estado.py:283–285`, `bin/gerar_estado.py:320–321`
- Os três blobs da cópia batem com o diff fornecido. A cópia veio sem metadados Git. Contagem dos 178 commits, corpos das mensagens e atribuição linha a linha entre os commits de cada pouso [nao verificado]. `diff.txt` (cabeçalhos `index`) [nao verificado]

## 28/11 12:42 · sessao da9ef6c862e5 · CRON-15-SOMBRA-CABE · leitura · origem=Code

**Estado** — Medida inicial e dossiê colhidos. A construção aguarda resposta à pergunta de desenho. `relato.txt:15024,15027`  
**Trava** — Só os comandos em série têm p95 de 5.095 s, ante teto bruto de 2.200 s. Quatro trilhas ainda dão 3.997 s com as dependências e a ordem de escrita. `relato.txt:27,15024`  
**Próximo passo** — Ronald responder à pergunta de desenho. O aval exige manter todas as etapas e o cron de prod, com última prorrogação em 04/12. `app/config/crons.py:344–345`; `relato.txt:27,15027`; `backlog.txt:2248`  
**Lei aberta** — Zero perguntas de lei no dossiê. `relato.txt:15027`

- **Medida por etapa** — Faltam os recortes `.etapas` com início e fim datados por rodada, as duas linhas de espera e os TSVs dos comandos. Durações completas por rodada e hash do HEAD [nao verificado]. As linhas abaixo são da cópia. `logs_sombra.txt:1–3`; `bin/sombra.sh:380–398`; `bin/sombra_trava.sh:27–31`
- **Duas esperas pela trava** — Antes de `refazer` e de `bloco`. Cada espera vai de `t0` ao cálculo de `espera_s` em `sombra_espera.jsonl`. `bin/sombra.sh:438,451`; `bin/sombra_trava.sh:17,27–31`
- **Refazer e preparar dumps** — Refazer inteiro entre as travas ≈357 s no relato. Preparo, da garantia do banco até `sombra: dumps`, em `bin/sombra.sh:238–253`. `relato.txt:15024`
- **Recriar e restaurar** — Drop/criação `bin/sombra.sh:255–256`, restore `public` `bin/sombra.sh:257–258`, restore `juliani` `bin/sombra.sh:259–260`, resumo `bin/sombra.sh:263–264`.
- **Máscaras e selo pessoal** — Máscara 1 `bin/sombra.sh:266–268`, máscara 2 `bin/sombra.sh:272–274` (≈261 s no relato), selo pessoal `bin/sombra.sh:275–277`. `relato.txt:15024`
- **Divergência, completude e carimbo** — Das consultas ao resumo de divergência, seguido do carimbo. `bin/sombra.sh:286–301,302–305`
- **Bloco** — Preparo, lavra e lista `bin/sombra.sh:329–350`, execução `bin/sombra.sh:351–387`, fechamento `bin/sombra.sh:390–407`. P95 da soma dos comandos de 15–28/11 = 5.095 s. P95 ponta a ponta [nao verificado]. `relato.txt:15024`
- **Por comando** — O TSV mede de `t0` à gravação da duração. A reposição da lavra ocorre antes de `t0` e precisa de marcações próprias para ser medida. `bin/sombra.sh:378–385,398`
- **Três mais longos** — `selo_leitores` 1.222 s, `lavrar_he` 670 s, `lavrar_cxr` 634 s no p95. Na rodada reportada, 1.174 s, 670 s e 634 s, respectivamente. `relato.txt:15024,15132`
- **Hipótese de encaixe** — Testar leituras em lote na origem de `espelho_do_colab`, mantendo as três etapas, seus gatilhos e suas escritas. Na rodada reportada, mesmo zerando o custo das três na conta, sobram 2.617 s de comandos antes do refazer. O encaixe exige ganhos também no restante do bloco. Ganho efetivo [nao verificado]. `app/folha/porta_export.py:475–483`; `app/ponto/services/he_pendente_lavrado.py:80–81`; `app/ponto/services/cxr_na_lista.py:62–78`; `app/config/crons.py:1581–1583,1808–1820`; `relato.txt:15024,15132`

## 28/11 12:40 · sessao 88bb13529e50 · RAZAO-DA-SOBRA · censo · origem=Code

**Resumo · Perímetro da cura**, sob `app/` — `escala/{orfa_classe,utils}.py`, `escala/services/{leitor_celula,furos_diarios}.py`, `ponto/{turnos,juiz_batida,status_dia,supra_juiz}.py`, `ponto/services/{cartorio,fechamento,espelho,dia_pago,bordas_realizado,cluster_espurio,vigia_pareador_celula,vigia_de_hora,ponta_pequena}.py`, `ponto/portas/he.py` e `ponto/management/commands/{tripwire_celulas,recusar_ponta_pequena}.py`. `backlog.txt:2109`, `2ce5e49b5:app/escala/tests/test_razao_sobra_fonte.py:165`, `2ce5e49b5:app/ponto/tests/test_razao_sobra_leitores.py:30`, `app/docs/RELATO.md:14627`  
**Perímetro, continuação** — `chamados/{juizes,models,reconciliador,signals}.py`, `chamados/services/{declaracao_texto,materializacao,fabrica_da_celula,relavra_via,disputa_emissao,fio,cobrar_dia,vigia_fio}.py`, `folha/{causas,export}.py`, `relatorios/{cartao_pela_celula,pdf_espelho}.py`, `templates/ponto/espelho.html`, `core/juizes.py` e `docs/CORTES.{md,json}`. `2ce5e49b5:app/ponto/tests/test_razao_sobra_leitores.py:18`, `2ce5e49b5:app/ponto/tests/test_razao_sobra_leitores.py:155`, `2ce5e49b5:app/chamados/tests/test_razao_sobra_e2_fabrica.py:10`, `2ce5e49b5:app/chamados/tests/test_razao_sobra_repasse.py:11`, `a4a9b6bfe:app/core/juizes.py:1333`  
**Obras com alterações no perímetro fora do `main`** — PAR-PELO-TURNO, E1-PREVISAO-24H, ABONO-X-SAIDA-ANTECIPADA, DESLIGADO-PORTA-DO-CHAMADO, E4C-F5-INSTANTE, E4D-CONTRATO-ROTA-B, DIA-DA-BATIDA-JUIZ-UNICO, FERIADO-SEM-LIMITE-DO-SUPRIMIDO e O30-F3. Alterações sem commit [nao verificado]. `worktrees.txt:1`, `worktrees.txt:57`, `worktrees.txt:63–71`, `worktrees.txt:76`, `worktrees.txt:83–90`, `worktrees.txt:105`, `worktrees.txt:126`   [nao verificado]
**Interseção** — `turnos.py:PAR/ABONO/DIA`; `utils.py` e `espelho.py:PAR/E1`; `fechamento.py:PAR/E1/DIA/FERIADO`; `cartao_pela_celula.py` e `pdf_espelho.py:PAR`; `leitor_celula.py` e `cartorio.py:E1`; `dia_pago.py:E1/FERIADO`; `export.py:E1/E4C`; `core/juizes.py:E1/O30`; `CORTES.{md,json}:E1`; `chamados/{models,signals}.py:DESLIGADO/E4C`; `chamados/juizes.py`, `declaracao_texto.py` e `cluster_espurio.py:DESLIGADO`; `materializacao.py:DESLIGADO/E4C`; `disputa_emissao.py:DESLIGADO/E4C/E4D`; `fio.py:E4D`. `worktrees.txt:57`, `worktrees.txt:63–71`, `worktrees.txt:76`, `worktrees.txt:88`, `worktrees.txt:105`, `worktrees.txt:126`  
**Veredito — não.** A PAR ainda cruza a cura, e a ordem vigente põe a RAZAO-DA-SOBRA depois da PAR. `app/docs/RELATO.md:14627`, `app/docs/BACKLOG.md:1926`, `worktrees.txt:88`

- **PAR-PELO-TURNO** — c1v13 `84f8a88d4`, com c1v16 `aebeee441` no `wt-codex-par-pelo-turno-111717`. Nenhuma das duas pousou no `main`. `app/docs/RELATO.md:14630`, `worktrees.txt:88`
- **E1-PREVISAO-24H** — `8d81e64c5` no `wt-codex-e1-previsao-24h-083708`, E1a c2 `339368be7` e base c3 `8cc31f45c`. Esses commits seguem fora do `main`. `worktrees.txt:66–67`, `worktrees.txt:105`
- **ABONO-X-SAIDA-ANTECIPADA** — `6cc0c2a7e` e `f38300c20` alteram `turnos.py` e seguem fora do `main`, inclusive na cópia c1h `90b161ba3`. A c1d pousou por `19810f51c`. `worktrees.txt:6`, `worktrees.txt:11`, `worktrees.txt:17`, `worktrees.txt:57`
- **DESLIGADO-PORTA-DO-CHAMADO** — base `540ab73c0` e c5 `c28de9ee3`, ambas fora do `main`. `worktrees.txt:50`, `worktrees.txt:63`, `app/docs/RELATO.md:15149`
- **E4C-F5-INSTANTE** — c3 `330e159fb`, candidata a pouso `dbc5b8b4e`, cura5 `63680b06e`, cura6c `486a1ffc4` e OS6 `c9fded10a`, todos fora do `main`. `worktrees.txt:68–70`, `worktrees.txt:106–107`
- **E4D-CONTRATO-ROTA-B** — cura4 `f300431af`, fora do `main`. `worktrees.txt:71`
- **DIA-DA-BATIDA-JUIZ-UNICO** — F2 `a4d3675c7` e F4e `fb8194db0`, fora do `main`. `worktrees.txt:64–65`
- **FERIADO-SEM-LIMITE-DO-SUPRIMIDO** — c4d `28eb07f5d`, fora do `main`. **FERIADO-UM-JUIZ** pousou por `a497c0cce`, com sobreposição em folha, espelho e relatórios. `worktrees.txt:76`, `worktrees.txt:111`
- **O30-F3** — `ac4ad27ce` e candidata `e2955107b`, fora do `main`. **CODEX-CONSTROI-MOTOR** tinha alterações nos arquivos de cortes nas cópias c11/c12 `0b3939987`/`eea12c760`; a versão que pousou foi a c2, por `279aac944`. `worktrees.txt:81–84`, `worktrees.txt:126`, `app/docs/RELATO.md:14898`
- **Demais pousos com arquivos em comum** — PONTA-PEQUENA itens 2–3 `b8f12a692` e v5b `71efa33d7`; PAR-RELAMPAGO `e216b756a`; PORTA-CONFERE-TRAVA `eb76e7be7`; CODIGO-DOMINIO-VALIDO `3545ebc07`; RETRATO-PDF item 2 `b82a5ef08`. `backlog.txt:2109`, `backlog.txt:2440`, `app/docs/RELATO.md:108`, `app/docs/RELATO.md:495`, `worktrees.txt:61`, `worktrees.txt:96`

## 28/11 12:35 · sessao e4470d0b72a6 · RAZAO-DA-SOBRA · leitura · origem=Code

**Resumo**  
**Estado** — Obra descongelada. `reds1` commitada em `2ce5e49b5`, com seis módulos só de teste. Na VM, 52 testes, 30 falhas, 1 erro e 5 pulados (`relato.txt:14632`, `relato.txt:14654`).  
**Trava** — O perímetro da cura ainda cruzava PAR c1v13 e E1 na medição das 15:25. Cruzamento e vaga atuais [não verificado] (`relato.txt:14633`, `relato.txt:14645`, `relato.txt:14661`).  
**Próximo andável** — Codex leitura/censo reconfere o cruzamento antes do despacho. Com interseção zero e vaga, segue para Codex construtor (`relato.txt:14633`, `relato.txt:14645`, `relato.txt:14661`).  
**Lei aberta** — Zero no despacho, pelo aval RAZAO-LEIS-1-6-2611 (`relato.txt:14220`).

**Brief do Codex**

- Na sessão com as worktrees, contar e listar os arquivos em comum entre o perímetro do dossiê §8 e o `git diff --name-only <merge-base>..HEAD` de PAR e E1. Registrar o resultado por worktree e a disponibilidade de vaga (`relato.txt:14633`, `relato.txt:14645`, `relato.txt:14661`).
- Na cópia, seguir a sobra do pareador e da grade até o classificador e a ata. Conferir onde o instante vira `HH:MM` e onde dia de trabalho sem marco vazio recebe `INDEFINIDA` (`app/ponto/turnos.py:1192`, `app/escala/utils.py:1748`, `app/escala/utils.py:1883`, `app/escala/orfa_classe.py:53`, `app/escala/orfa_classe.py:68`, `app/ponto/services/cartorio.py:761`).
- No censo AST, localizar leituras cruas de `orfas` e `n_orfas`, incluindo ata antiga, espelho, PDF e efeito no fechamento. O aval de construção prevê o selo catraca e `sobras_sem_razao` (`app/escala/services/leitor_celula.py:180`, `app/ponto/services/espelho.py:703`, `app/relatorios/pdf_espelho.py:477`, `app/folha/causas.py:43`, `app/ponto/services/fechamento.py:1117`, `backlog.txt:2109`).
- **REDs registrados** — R1–R6, A1, A2, V1 e V2 (`relato.txt:14654`). Enunciados individuais e causa do erro [não verificado].

## 28/11 12:34 · sessao 4ff1449fdd7f · SONDA-ESPURIAS-RAMO2 · leitura · origem=Code

- **Estado.** Sonda s2 pronta com `rc 0`. Os quatro lados cobriram 963 colabs nas empresas 2/3/4, comp. 11, com 99 sobras dia a dia. RAMO2 mudou 0 dos 14 colabs do teto. (backlog.txt:2246, relato.txt:14229)
- **Trava.** A pausa da sonda foi retirada às 14:43. A c2u do RAMO2-CRUZA aguarda vaga do opus. (relato.txt:14632, 14447)
- **Próximo passo andável.** Ronald envia o aviso do CASO-LITERAL-14 para contraprova. A etapa do Codex na sonda já foi concluída. (backlog.txt:2111, relato.txt:14229)
- **Lei.** Nenhuma pergunta aberta registrada para a sonda. A regra do RAMO2 está decidida. A âncora preserva o trabalhado e L-097 só classifica a HE. (backlog.txt:2183, 2246)

## 28/11 12:28 · sessao 4b8be07b248d · TESTE-DO-ESTRANHO · leitura · origem=Code

**RESUMO — acertou 0 · parcial 5 · travou 0.** A nota considera todos os pontos explícitos de cada tarefa no gabarito. `gabarito.md:2`, `gabarito.md:6`, `gabarito.md:10`, `gabarito.md:12`, `gabarito.md:19`

- **T1 — parcial.** Acertou a ordem `ORDEM-VIVA-2811-1200`, as duas sequências e a meta. Faltou explicitar que o hook usa `ORDEM-VIVA-TOPO` como autoridade e que o marcador ficou na `1511-NOITE` até 28/11. Destino → glossário. `resposta.md:8`, `app/docs/BACKLOG.md:8`, `app/docs/BACKLOG.md:1926`, `app/docs/BACKLOG.md:1927`, `bin/hook_stop_fila1.py:188`
- **T2 — parcial.** A causa confere. As perguntas 61520/61521 foram emitidas em 23/11 às 12h36, quando a célula 122468 previa 11h/12h, e ficaram pendentes após a regeneração das 12h42 retirar o intervalo. É órfã de regeneração, com cura de classe no E2 `retirar_contexto`. Faltou identificar o 40588 como um dos seis da `CHAMADO-PRESO-CLASSE-PORTAS-2611` e dizer que a porta `admin_encerra` já estava no ar, sem tela que a oferecesse. Destino → diagrama. `resposta.md:18`, `resposta.md:20`, `app/docs/RELATO.md:15208`, `app/docs/BACKLOG.md:836`, `app/docs/BACKLOG.md:1640`
- **T3 — parcial.** Identificou o juiz da dobra, `MotorBase._feriado_dobra_no_dia`. Faltou explicar que a guarda L-097 soma os minutos a `minutos_fora_janela`, zera o trabalhado e colapsa o par. O `FERIADO-UM-JUIZ` pousou em `a497c0cce`. A cura da virada no 6x1, `dc876f5d1`, foi reprovada por perder 240 minutos e ficou fora do pouso. Destino → diagrama. `resposta.md:28`, `resposta.md:30`, `app/core/juizes.py:320`, `app/ponto/motor_calculo_v2.py:941`, `app/ponto/motor_calculo_v2.py:1585`, `app/docs/RELATO.md:14867`, `app/docs/RELATO.md:14872`, `app/docs/RELATO.md:15231`
- **T4 — parcial.** Faltaram o comando exato da suíte, a verificação na sequência geral, o arquivo de reversão em `logs/` e a linha `LEI-AKITA` no commit. Rito do gabarito → cópia `wt` → `bin/suite.sh --dir <copia> --motivo "pouso <OBRA>"` na árvore do pouso → verificador ou Opus final → reversão prévia em `logs/` → `git merge --no-ff --no-commit`, commit com `OBRA:` e `LEI-AKITA`, e `bin/deploy.sh` no mesmo ato. Reversão do merge → `git revert -m 1 <merge>` e `bin/deploy.sh`. `resposta.md:38`, `CLAUDE.md:318`, `CLAUDE.md:105`, `CLAUDE.md:347`, `CLAUDE.md:429`, `CLAUDE.md:456`, `CLAUDE.md:63`, `CLAUDE.md:91`, `bin/commit_msg.sh:29`, `app/docs/RELATO.md:15081`
- **T5 — parcial.** Começou pelo handoff e pulou a comparação com a ordem mais nova e a consulta explícita ao hook. A sequência é marcador × ordem mais nova → saída de `bin/hook_stop_fila1.py` → `PAREI` e `lei` no topo do RELATO → `python3 bin/rc_sem_colher.py` → processos e jobs vivos (`bin/trava_teste.sh --quem`, `bin/codex_vagas_estado.py` e pista) → `HANDOFF-SESSAO.md`. Destino → diagrama. `resposta.md:50`, `gabarito.md:20`, `app/docs/BACKLOG.md:1926`, `bin/hook_stop_fila1.py:188`, `CLAUDE.md:272`, `CLAUDE.md:278`, `CLAUDE.md:338`, `bin/rc_sem_colher.py:13`, `bin/trava_teste.sh:16`, `bin/codex_vagas_estado.py:2`, `app/docs/HANDOFF-SESSAO.md:3`

- TESTE-DO-ESTRANHO T1: explicitar a autoridade de `ORDEM-VIVA-TOPO` e registrar o marcador preso na ordem anterior -> glossário (`bin/hook_stop_fila1.py:188`, `app/docs/BACKLOG.md:1927`)
- TESTE-DO-ESTRANHO T2: ligar o 40588 à fatia dos seis chamados e mostrar a porta admin ainda sem tela -> diagrama (`app/docs/BACKLOG.md:836`, `app/docs/BACKLOG.md:1640`)
- TESTE-DO-ESTRANHO T3: mostrar onde a L-097 zera o par e registrar a reprovação da cura da virada 6x1 -> diagrama (`app/ponto/motor_calculo_v2.py:1585`, `app/docs/RELATO.md:14872`)
- TESTE-DO-ESTRANHO T4: registrar comando da suíte, verificação, arquivo de reversão e linha `LEI-AKITA` no rito de pouso -> inventário (`CLAUDE.md:105`, `CLAUDE.md:429`, `CLAUDE.md:456`, `CLAUDE.md:63`) [nao verificado]
- TESTE-DO-ESTRANHO T5: ordenar o diagnóstico da fila vazia, da comparação do marcador até o handoff -> diagrama (`gabarito.md:20`, `app/docs/BACKLOG.md:1927`, `app/docs/HANDOFF-SESSAO.md:3`)

## 28/11 12:27 · sessao 56360ec16f5a · FERIADO-L097-MOTOR · leitura · origem=Code

## Resumo

- A FERIADO-UM-JUIZ pousou em `a497c0cce` e o apply da 11 rodou. A L-097 segue fora do `main`, com a c2 `dc876f5d1` reprovada pelo ver2. `app/docs/RELATO.md:14867,14872,15264–15270`
- Seguem abertos os 240 min perdidos no feriado D+1 e a divergência de data entre autorização, pendência e oráculo. Faltam o DIFF da 10 e a lista final da J.A com reversão [nao verificado]. `app/docs/RELATO.md:14867,14872,14876`
- O `!` da herança já foi respondido no topo e o v2 manda tratar D+1 em volta própria **depois** do pouso da L-097. As perguntas de lei (0)–(2) têm decisões restritivas contestáveis. `app/docs/RELATO.md:16,29,14877`
- Já dá para transportar a cura do feriado do próprio turno, limitada à guarda, e revalidar pagamento, pendência e segmentos na base pós-FUJ. `app/docs/RELATO.md:40,14867,14877`
- Próximo passo na vaga opus é portar c1+c2, rodar RED e mutantes, inteira e opus final. Medir o DIFF da 10 e a lista da 11 na árvore candidata ao pouso. `app/docs/BACKLOG.md:1926,2190` · `app/docs/RELATO.md:14867,14872`

## BRIEF c3 — opus construtor

- **Base.** `main` HEAD `96e66c5f2`. Cherry-pick em ordem `05ec19a6f` → `dc876f5d1`, ou rebase dos dois desde `1d2eb496c`. A c2 isolada pressupõe a c1. A prévia de merge foi limpa em leitura. `app/docs/RELATO.md:14796–14797,14867,15270`
- **Guarda no HEAD.** Inserir o retorno no ramo de folga com escala certa, após `escala_certa_no_dia`. Exigir feriado, ciclo fora de `CICLOS_FERIADO_SIMPLES` e `_dia_do_turno_do_par(entrada)[0] == dia`. A partição usa o mesmo juiz e escolhe a rubrica. `app/ponto/motor_calculo_v2.py:963–978,1579–1590,3740` · `app/ponto/calculador/regras.py:410–432`
- **PROIBIDO.** Sem `except` novo, sem relógio solto, motor só no hunk da guarda. Fora do delta de produção ficam o laço de herança, data da autorização, publicação da pendência, partição, fechamento, guarda de competência e TXT. `app/docs/PROMPTS.md:3176` · `app/docs/RELATO.md:14867,14877` · `app/docs/PLANO-PADROES.md:97–103`
- **CENSO, escritores no HEAD.** Motor grava dia herdado e mapa por período, zera e colapsa o par, publica alerta e `{dia, hora, minutos}`. A porta de HE grava a decisão que o motor consulta. `app/ponto/motor_calculo_v2.py:2513–2522,2570–2574,1585–1602,2833,1569–1571` · `app/ponto/portas/he.py:44`
- **CENSO, escritores do pago.** Fechamento monta contexto por dia e por período, `particionar_ft` pode remapear dia/vínculo e escolhe rubrica, fechamento salva `FechamentoMensal` e chama as duas lavras de `DiaPago`. `app/ponto/services/fechamento.py:332–351,995–999,1181,1237,1300` · `app/ponto/calculador/regras.py:410–432` · `app/ponto/services/dia_pago.py:372`
- **CENSO, leitores da pendência.** Espelho traduz `sentido='dia'` e agrega dias. Retrato lê o espelho e grava `MetricaSnapshot`, Gestão de HE lê o retrato, portão do TXT lê pendência e decisão, PDF lê pendência e alertas. `app/ponto/services/espelho.py:757–763,939` · `app/ponto/services/he_pendente_lavrado.py:138,183–195` · `app/ponto/services/gestao_he.py:426–434` · `app/folha/porta_export.py:181–201,687–692` · `app/relatorios/pdf_espelho.py:935–945,968–979`
- **CENSO, leitores do par e do pago.** Oráculo cruza dia/hora da pendência com as pontas efetivas e consome a partição. TXT lê o mensal. Calendário lê `DiaPago`. `app/ponto/calculador/alimentacao.py:73–83,114–123,353–371,514–518` · `app/folha/export.py:434,460,734–742` · `app/colaboradores/services/calendario.py:684–689`
- **RED — feriado 6x1 no próprio turno, janela ativa.** Reativar o teste pulado. Certa e incerta totalizam 4 h com e sem autorização. A rubrica segue a partição: FT certa soma 4 h, ou HE100-feriado quando o feriado é a causa da folga. Conferir mensal e lavras. Mutante: retirar o retorno da guarda para a certa. `app/ponto/tests/test_feriado_um_juiz_porta.py:547–563` · `dc876f5d1:app/ponto/tests/test_feriado_um_juiz_porta.py:579–605`
- **RED — feriado em D, dois turnos.** D 18h–22h e D+1 03h–07h, vão de 5 h. Em 6x1→6x1 e 6x1→12x36, exigir 4 h HE100 em D e somente o par das 03h com 240 min pendentes. Mutante: retirar a igualdade do dia do turno e voltar a liberar pelo dia herdado. `dc876f5d1:app/ponto/tests/test_feriado_um_juiz_porta.py:810–841,859–862` · `dc876f5d1:app/ponto/motor_calculo_v2.py:1599`
- **RED — pendência isolada.** Feriado liberado sem `sentido='dia'`. Folga comum com uma pendência de 240 min às 07h no motor e no espelho. Mutante: suprimir só o `append` da pendência e manter o bloqueio. `dc876f5d1:app/ponto/tests/test_feriado_um_juiz_porta.py:903–923` · `app/ponto/motor_calculo_v2.py:1585–1602`
- **RED — segmentos.** Um segmento de 240 min no feriado. Folga comum sem segmento, com um `par_bloqueado_sem_previsto`. Mutante: omitir só o colapso de `efetivo_saida`, mantendo zeragem e pendência. `dc876f5d1:app/ponto/tests/test_feriado_um_juiz_porta.py:925–936` · `app/ponto/motor_calculo_v2.py:1591–1602`
- **Controles da guarda.** Folga comum sem autorização zera e com autorização paga. Feriado sem folga e previsto zero segue bloqueado. 12x36 segue bloqueado mesmo com dobra ligada pela CCT. Mutantes respectivos: dispensar feriado, dispensar folga, trocar o teste de ciclo por `feriado_em_dobra`. Reexecutar os controles de intermitente por base e ciclo. `app/docs/RELATO.md:29` · `dc876f5d1:app/ponto/tests/test_feriado_um_juiz_porta.py:607–651` · `app/ponto/tests/test_feriado_um_juiz_porta.py:358–378`
- **Controles da virada.** Em 6x1→12x36 com feriado D+1 e 12x36→6x1 com feriado D, ambos os pares ficam bloqueados. Um único turno do feriado com pares 21h–00h e 01h–05h paga 7 h. Mutantes: trocar o ciclo pelo vínculo vizinho e trocar o dia do turno pela data civil da entrada. `dc876f5d1:app/ponto/tests/test_feriado_um_juiz_porta.py:864–895` · `app/ponto/motor_calculo_v2.py:963–978`
- **D+1 na volta própria.** Preservar os dois `expectedFailure` de 4 h HE100, sem troca e em 12x36→6x1. O opus final decide se isola a asserção do pago para erro de fixture não contar como prova. Na volta D+1, o mutante é restaurar o bloqueio pelo dia herdado. `app/docs/RELATO.md:16,14877` · `dc876f5d1:app/ponto/tests/test_feriado_um_juiz_porta.py:850–857,880–884`
- **(0)** Feriado sem folga e com previsto zero entra na liberação? Corte atual mantém o bloqueio. `app/docs/RELATO.md:29`
- **(1)** 12x36 em folga real de feriado mantém o bloqueio integral ou só o corte da rubrica 59-A/CCT? Corte atual mantém o bloqueio integral. `app/docs/RELATO.md:29`
- **(2)** FT como única rubrica com trabalhadas zero entra no TXT? Classificar como INCOMPLETO na lista. O HEAD tem ressalva para HE100-feriado emitível. A classificação dos casos da frota está [nao verificado]. `app/docs/RELATO.md:29` · `app/folha/export.py:931–945,977–998`
- **Aceite.** Fixar janela, decisão e `agora` na fixture. Para cada RED, mostrar a asserção esperada na base sem a cura e a queda com seu mutante. Rodar inteira e anexar a triagem ver2 ao opus final. Execução na base c3 [nao verificado]. `app/docs/PROMPTS.md:3224–3225` · `dc876f5d1:app/ponto/tests/test_feriado_um_juiz_porta.py:903–923` · `app/docs/RELATO.md:14867–14872`
- **Rito de pouso.** DIFF da 10 pela L-113, publicado antes do pouso/ato de frota, com aviso ao DP. A triagem chama a 10 de exportada. O HEAD registra paga à mão, sem TXT e sem guarda. Situação atual em prod [nao verificado]. Preservar a 09 guardada. Lista J.A da 11 por colab, dia e minutos, com reversão antes do apply L-092 da L-097. `app/docs/RELATO.md:25,14872,14876` · `app/ponto/services/fechamento.py:37–50,603–629,1847–1853` · `app/docs/BACKLOG.md:2190`
- **Padrão.** `Single Writer Principle` / `juiz único`: a guarda decide a efetividade do par e a partição existente decide a rubrica. `app/docs/GLOSSARIO.md:12` · `app/ponto/motor_calculo_v2.py:1585–1602` · `app/ponto/calculador/regras.py:428–432`

## 28/11 12:25 · sessao ca3cc270be5e · TESTE-DO-ESTRANHO · leitura · origem=Code

## RESUMO

**5 resolvidas · 0 parciais · 0 travadas.** A ordem vigente está em `app/docs/BACKLOG.md:1926` e a causa do chamado 40588 em `app/docs/RELATO.md:15208`.  
**Principal limite:** o handoff é um retrato de 28/11 às 11h58. O estado ao vivo da sessão, das filas e da produção ficou [nao verificado]. `app/docs/HANDOFF-SESSAO.md:3`

## T1. O que está em construção agora, em que ordem e por quê?

**RESPOSTA** — Vale a **ORDEM-VIVA-2811-1200**. A meta é zerar os problemas atribuídos à Hasner na J.A, competência 11, até segunda, com uma sonda no domingo à noite. Codex, pista 2, pista 3 e Opus verificador devem receber trabalho sempre que houver item executável. Pré-requisito pendente herda a vez, e obras que disputam o mesmo arquivo precisam ser sequenciadas. `app/docs/BACKLOG.md:8` · `app/docs/BACKLOG.md:1926` · `CLAUDE.md:318` · `CLAUDE.md:444`

- **Opus construtor:** CUSTO F1 v7 → OBRA-PRONTA-ACORDA → FERIADO-L097-MOTOR c2 → DESLIGADO c4/c5 → CUSTO F2/F3. A F1 v7 já tinha commit em cópia e aguardava a coleta dos resultados de testes e mutantes. `app/docs/BACKLOG.md:1926` · `app/docs/RELATO.md:15212`
- **Codex e pistas:** canário E4C a3/c4 e E4D v4 → C7-TELA-ADMIN-ENCERRA, para os 88 casos → RETRATO-PORTA-PELO-MOTIVO, depois da OS6 → RAMO2-B e RAMO2-HERANCA-VAO → PAR → ORFAS-FOLHA-E-ALARME itens 2 e 3 → E4d → faixa do BOTAO → RAZAO-DA-SOBRA → CRON-15-SOMBRA-CABE → SONDA-ESPURIAS/CASO-LITERAL-14 → E1C. No último registro, a E4C a3 estava em aceite, a E4D v4 tinha entrega para aceite e a C7 tinha código em cópia, com testes, verificação e smoke pela frente. `app/docs/BACKLOG.md:1926` · `app/docs/RELATO.md:15234` · `app/docs/RELATO.md:15237` · `app/docs/RELATO.md:15241`
- **FERIADO-UM-JUIZ** já estava no ar às 11h36, com o apply da competência 11 registrado como concluído e o pacote ao Fernando preparado. O envio ficou com Ronald. `app/docs/RELATO.md:15231` · `app/docs/RELATO.md:15266` · `app/docs/RELATO.md:15270`

**AUTO-AVALIAÇÃO** — resolvi. **ONDE TRAVOU** — a ordem e o andamento estão no BACKLOG e no RELATO. A ocupação das vagas após o retrato ficou [nao verificado]. `app/docs/HANDOFF-SESSAO.md:18` · `app/docs/BACKLOG.md:1926` **TEMPO** — ~1 min e 30 s [nao verificado].

## T2. Chamado 40588, de 20/11

**RESPOSTA** — As perguntas **61520 e 61521** foram emitidas em **23/11 às 12h36**, quando a célula **122468** previa os marcos de **11h e 12h**. Às **12h42**, a célula foi regenerada para **07h–19h sem intervalo**, e as duas perguntas continuaram pendentes. A causa é **órfã de regeneração**. `app/docs/PROMPTS.md:3223` · `app/docs/RELATO.md:15208`

A correção da classe pertence à fatia **CHAMADO-PRESO** da obra **ESCRITOR-UNICO-DE-CHAMADO**, no **E2 `retirar_contexto`**, após o pouso da E4C. **INTERVALO-SEM-PREVISTO**, da RAIA-INTERVALO, encontrou a causa. **C7-TELA-ADMIN-ENCERRA** é a via manual para o admin encerrar esses casos com motivo e trilha. `app/docs/BACKLOG.md:835` · `app/docs/BACKLOG.md:2654` · `app/docs/PROMPTS.md:3229`

**AUTO-AVALIAÇÃO** — resolvi. **ONDE TRAVOU** — busquei `40588` no PROMPTS, RELATO e BACKLOG. O log bruto da sonda ficou [nao verificado]. `app/docs/RELATO.md:15208` **TEMPO** — ~1 min [nao verificado].

## T3. Feriado trabalhado em folga no 6x1

**RESPOSTA** — Na classificação mensal vigente desde a competência **11/2026**, os minutos apurados de feriado trabalhado em folga no **6x1** entram uma vez como **HE 100% de feriado**, ou seja, em dobro. O teste de 6x1 sem escala certa confirma **5h30 na rubrica de feriado e zero em horas comuns**. `app/folha/export.py:385` · `app/ponto/services/fechamento.py:1002` · `app/ponto/tests/test_he100_dia_sem_previsto.py:250`

O juiz da dobra é **`MotorBase._feriado_dobra_no_dia`**, que usa o feriado do **dia de início do plantão** e a regra do ciclo. Só o `12x36` consta da lista padrão de ciclos com feriado simples. **`rubrica_da_folga`** escolhe a rubrica, e **`feriado_vira_folga`** verifica se o feriado causou a folga daquele vínculo. `app/ponto/motor_calculo_v2.py:941` · `app/ponto/motor_calculo_v2.py:961` · `app/ponto/motor_calculo_v2.py:3740` · `app/ponto/calculador/regras.py:371` · `app/escala/models.py:1384`

A janela **L-097** ainda pode zerar o par antes da classificação. No apply registrado, os **13 dias de 10 colaboradores 6x1** do pacote seguiram com os mesmos valores. A cura para pagar o feriado diretamente pelo ciclo pertence à **FERIADO-L097-MOTOR**, que estava fora do HEAD registrado. `app/ponto/motor_calculo_v2.py:1562` · `app/ponto/motor_calculo_v2.py:1585` · `app/docs/PROMPTS.md:3176` · `app/docs/RELATO.md:15270`

**AUTO-AVALIAÇÃO** — resolvi. **ONDE TRAVOU** — conferi motor, calculador, fechamento e testes. O pagamento efetivo ficou [nao verificado]. `app/ponto/calculador/regras.py:428` · `app/docs/RELATO.md:15270` **TEMPO** — ~1 min [nao verificado].

## T4. Como pousar uma obra com reversão

**RESPOSTA**

1. **Prepare em cópia.** Confirme escopo, `!` e B3. Faça RED antes da cura. Para mão única, concorrência, schema ou multi-tenant, passe por censo, mutantes, revisão do Codex e Opus final após a suíte. `CLAUDE.md:28` · `CLAUDE.md:255` · `CLAUDE.md:274` · `CLAUDE.md:423` [nao verificado]
2. **Certifique a árvore que vai pousar.** Exija suíte inteira verde. Se rodar na pista, exija `bin/pista_certifica.sh` com rc 0 ou rode na VM. Confira os arquivos com `cmp` contra a cópia certificada e registre a prova no commit. `CLAUDE.md:104` · `CLAUDE.md:129` · `CLAUDE.md:347`
3. **Prepare a reversão antes do ato.** Registre no RELATO `git revert -m 1 <commit-do-merge>` seguido de `bin/deploy.sh`. Para apply de folha, grave antes o snapshot dos campos por colaborador e prove depois a integridade das competências exportadas por hash, com prova de frota no mesmo marco. Para alterar gravado de competência guardada, exija `!`, DIFF prévio e a porta staff. `app/docs/PROMPTS.md:3202` · `app/docs/RELATO.md:351` · `CLAUDE.md:455`
4. **Pouse e publique no mesmo ato.** Faça `git merge --no-ff --no-commit` → resolva → commit com `OBRA: <id>` → `bin/deploy.sh`. Produto pousa primeiro e instrumento tem pouso próprio. O deploy confere os portões, migra quando aplicável, recarrega as três cascas e prova as rotas. `CLAUDE.md:284` · `CLAUDE.md:352` · `app/docs/INFRA.md:38` · `bin/deploy.sh:416`
5. **Faça o smoke e registre o resultado.** Se reprovar, execute o revert previsto, republique e repita as provas. Se houve escrita de dados, siga também a reversão específica pela porta, restaurando só os campos-alvo do snapshot. `CLAUDE.md:477` · `app/docs/RELATO.md:4575` · `app/docs/RELATO-ARQUIVO.md:32867`

**AUTO-AVALIAÇÃO** — resolvi. **ONDE TRAVOU** — conferi o procedimento no CLAUDE, no deploy e em exemplos de reversão no RELATO. A execução ficou [nao verificado]. `CLAUDE.md:347` · `bin/deploy.sh:308` · `app/docs/RELATO.md:4575` **TEMPO** — ~2 min [nao verificado].

## T5. O Code parou e a esteira está vazia

**RESPOSTA** — Eu conferiria nesta ordem:

1. **Ordem e parada declarada.** Abra o `HANDOFF-SESSAO.md`, o topo do `RELATO.md` e o bloco `OBRAS` do BACKLOG. Confira se `ORDEM-VIVA-TOPO` aponta para um item aberto e a qual obra pertence o `PAREI: !` ou `ESPERANDO:`. Marcador ausente ou apontando para obra fechada pode dar a impressão de fila vazia. O hook de Stop hoje pode encerrar com item aberto e emitir só um aviso. `app/docs/HANDOFF-SESSAO.md:18` · `CLAUDE.md:272` · `bin/hook_stop_fila1.py:286` · `bin/hook_stop_fila1.py:1499`
2. **Sessão e trabalho em voo.** Veja o painel tmux e a última atividade do transcript. Para `ESPERANDO`, confira produtor, PID e arquivo de sinal. Confira também as travas de teste e sombra e os `.rc` concluídos ainda sem registro de coleta. Se houver recusa, leia o motivo na trilha do disjuntor ou do wrapper Codex. `bin/alarme_sessao_ociosa.py:18` · `CLAUDE.md:272` · `bin/handoff_sessao.sh:252` · `bin/rc_sem_colher.py:6` · `bin/disjuntor.py:4` · `bin/codex_pergunta.sh:517`
3. **Filas e portões da esteira.** Use o relatório `bin/esteira_status.sh --lista` para conferir a fila de fatias e a de integração. Veja se há fatia pronta e livre fora da integração, pronta com portão ou já no ar aguardando smoke. Confira `esteira.pausada` pelo dono, motivo e condição de saída, além de árvore vermelha, quarentena e controlador de teto. `bin/esteira_status.sh:3` · `bin/esteira_status.sh:41` · `bin/esteira_status.sh:63` · `bin/fabrica_estado.py:24` · `bin/esteira.sh:53`
4. **Descubra por que a vaga ficou vazia.** Se a fábrica estiver habilitada, confira o timer e o fim de `logs/fabricante.log`. A mensagem `registro sem item livre com alvo vivo` indica que o fabricante rodou e ficou sem alvo elegível. Se a sessão estiver tocando a fila, confira o próximo item da ordem, seu dossiê, pré-requisito e vaga. Uma trava de lei ou `!` de uma obra deixa seguir a próxima independente. `bin/hasner-fabricante.timer:7` · `bin/trava_a_vazia.sh:46` · `CLAUDE.md:278` · `CLAUDE.md:412` · `CLAUDE.md:444`

**AUTO-AVALIAÇÃO** — resolvi. **ONDE TRAVOU** — o handoff registra `esteira.pausada` na árvore do host e indica que havia item executável na fila 1. O motivo da pausa e o estado ao vivo do tmux e dos timers ficaram [nao verificado]. `app/docs/HANDOFF-SESSAO.md:14` · `app/docs/HANDOFF-SESSAO.md:27` **TEMPO** — ~2 min [nao verificado].

_8935 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
