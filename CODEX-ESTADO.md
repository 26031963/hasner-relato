# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 00:53** · publicado em **31/10/2026 00:56** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 00:53 · sessao 5bb56cea7746 · O261-E-F1-CLASSE-4 · verificador · origem=Code

1. **Os dois laços explícitos obedecem ao C8, mas isso não garante a ordem de todos os fechamentos do dia.** O imediato ordena antes de chamar `reconciliar()`; o adiado também. Havendo um não protestante com fio, o delta envia todos os não protestantes juntos para os adiados. (`app/ponto/services/cartorio.py:1217`, `app/ponto/services/cartorio.py:1225`, `app/ponto/services/cartorio.py:756`)

   `sobrevivente_por_ultimo` põe por último o escolhido por `cobranca_viva_do_dia`, que busca o mais novo por `-criado_em, -pk`; preserva a ordem dos demais. (`app/chamados/juizes.py:1627`, `app/chamados/juizes.py:1645`)

   **Sim, existem fechamentos fora dessa ordenação.** Antes do laço adiado, E2 pode fechar uma disputa, cujo signal reconcilia o chamado-pai sem passar pelo ordenador. Além disso, os adiados continuam em `vivos`: o ramo `FIO-DUP` pode reconciliar um deles ainda no primeiro corpo, também sem C8. (`app/ponto/services/cartorio.py:753`, `app/chamados/reconciliador.py:635`, `app/chamados/signals.py:42`, `app/ponto/services/cartorio.py:1329`)

2. **Em trabalho, o delta também desloca o sem fio para o lote adiado**, desde que ambos tenham os próprios marcos acesos: ambos não protestam e basta um deles estar em `_com_fio`. O sem fio deixa de tentar fechar em `_porm` antes da lavra e passa normalmente a tentar depois da lavra/E2, **antes da segunda execução do corpo**; o com fio já seguia esse caminho. (`app/ponto/services/cartorio.py:1155`, `app/ponto/services/cartorio.py:1218`, `app/ponto/services/cartorio.py:1225`, `app/ponto/services/cartorio.py:753`, `app/ponto/services/cartorio.py:766`)

   O delta não acrescenta veto: marcos acesos regularizam a premissa e o gate aceita essa regularização. A tentativa no laço pode ser um no-op se E2/signal já fechou o chamado. A ressalva é o sem fio composto de perguntas duplicatas mortas: ele ainda pode fechar pelo `FIO-DUP` no primeiro corpo; sem perguntas, esse bloco o pula. (`app/chamados/models.py:1229`, `app/chamados/models.py:699`, `app/chamados/signals.py:42`, `app/ponto/services/cartorio.py:1325`, `app/ponto/services/cartorio.py:1329`)

   Nos testes conferidos, não encontrei exigência de que esse par misto feche em etapas distintas: o teste do evento verifica fechamento no mesmo evento; o v5 usa um único adiado e verifica a ordem das datas; o teste de ressuscita verifica resultado e contadores com um chamado. Ausência dessa dependência em toda a suíte: **[não verificado]**. (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:390`, `app/chamados/tests/test_o261e_f1_codex_v5.py:169`, `app/chamados/tests/test_o261e_f1_codex_v5.py:182`, `app/ponto/tests/test_o261_cobrado_le_o_fio.py:409`)

3. **Sim, a fixture produz o velho com fio, inclusive pela leitura de `pergunta_viva`.** Ela cria a pergunta sem informar `celula`, cuja FK aceita nulo. Em folga, `classificar_falta` retorna sem falta; porém C6 continua até `materializada_em is None` e retorna `True`. `fio_vivo` consulta esse C6, exatamente como `_com_fio`. (`app/ponto/tests/test_fechar3_troca_escala_gate.py:194`, `app/chamados/models.py:2150`, `app/escala/servico_jornada.py:303`, `app/chamados/juizes.py:1074`, `app/chamados/juizes.py:1080`, `app/chamados/juizes.py:1115`, `app/ponto/services/cartorio.py:1094`)

   Essa pergunta **não é absolvida pelo E2**, que exige célula associada. Com o delta, o velho tenta primeiro, fecha por dedup e reata a pergunta ao novo. (`app/chamados/reconciliador.py:588`, `app/chamados/juizes.py:1651`, `app/chamados/models.py:702`, `app/chamados/models.py:418`)

   **Pelo contrafactual do delta fornecido, as asserções finais ficam vermelhas sem a alteração de `_adiar`:** o novo sem fio fecharia imediatamente apoiado no velho; depois o velho ficaria sem irmão, com a premissa da véspera ainda viva, e teria o fechamento recusado. (`app/ponto/services/cartorio.py:1225`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:175`, `app/chamados/juizes.py:424`, `app/chamados/models.py:702`, `app/chamados/models.py:725`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:200`)

4. **Sim, por leitura, `[RECONCILIA-MARCO] ` é o caminho nas duas ordens da fixture anterior.** Em `trabalha=False`, nenhum protesta; sem fio, seguem para `_porm`, ordenado pelo C8. O velho fecha por dedup apoiado no novo, produzindo a linha nesse bloco. O B5.3b depende dos códigos de cobrança, cujo insumo agora recebe zero chamados protestantes. (`app/ponto/tests/test_fechar3_troca_escala_gate.py:134`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:137`, `app/ponto/services/cartorio.py:1155`, `app/ponto/services/cartorio.py:1159`, `app/ponto/services/cartorio.py:1225`, `app/ponto/services/cartorio.py:1231`, `app/ponto/services/cartorio.py:1252`, `app/ponto/supra_juiz.py:441`)

Execução dos testes e da mutação: **[não verificado]**. [nao verificado]

## 31/10 00:53 · sessao 7ab06b1c856c · - · - · origem=Ronald (tmux)

- Em trabalho, `marcos_acesos=True` faz ambos não protestarem; havendo um com fio, `_adiar` recebe ambos. O sem fio deixa de tentar `_porm` no primeiro corpo e normalmente tenta fechar após lavra/E2, ainda antes do segundo corpo; o com fio já seguia esse caminho. `app/ponto/services/cartorio.py:1155`, `app/ponto/services/cartorio.py:1217`, `app/ponto/services/cartorio.py:1225`, `app/ponto/services/cartorio.py:744`, `app/ponto/services/cartorio.py:753`, `app/ponto/services/cartorio.py:756`, `app/ponto/services/cartorio.py:766`.

- O delta não acrescenta veto ao fechamento: marcos acesos regularizam a premissa, e o gate aceita a premissa regularizada. A tentativa pode resultar em no-op se E2/signal já fechou o chamado. `app/chamados/models.py:1229`, `app/chamados/models.py:699`, `app/chamados/reconciliador.py:638`, `app/chamados/signals.py:42`, `app/chamados/models.py:615`.

- Ressalva concreta à ordem: adiados continuam em `vivos`; se o sem fio possui somente perguntas `VIA_DUPLICATA`, `FIO-DUP` tenta reconciliá-lo ainda no primeiro corpo, sem C8. Sem perguntas, esse bloco pula o chamado. `app/ponto/services/cartorio.py:1220`, `app/ponto/services/cartorio.py:1321`, `app/ponto/services/cartorio.py:1325`, `app/ponto/services/cartorio.py:1329`.

- Nos testes conferidos, nenhum exige que o par misto trabalho/com fio/sem fio feche em corpos distintos: o teste do evento verifica fechamento no mesmo evento e ausência de terminal com pergunta viva; o v5 usa um único adiado e verifica ordem das datas; o teste de ressuscita usa um chamado e verifica resultado/contadores, pelo caminho B5.3b. `app/chamados/tests/test_o261e_f1_porta_encerramento.py:358`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:390`, `app/chamados/tests/test_o261e_f1_codex_v5.py:169`, `app/chamados/tests/test_o261e_f1_codex_v5.py:182`, `app/ponto/tests/test_o261_cobrado_le_o_fio.py:393`, `app/ponto/tests/test_o261_cobrado_le_o_fio.py:416`. Ausência dessa dependência em toda a suíte: [não verificado].

## 31/10 00:52 · sessao 5b464716b57a · - · - · origem=Ronald (tmux)

**3. Sim, por leitura estática a fixture alcança o lote misto e distingue o delta.**

- O velho recebe a pergunta via `objects.create(... chamado_destino=velho, data_evento=DIA)`, sem `celula`; a FK permite nulo. A associação automática à célula pertence a `abrir()`, que a fixture não chama. (`app/ponto/tests/test_fechar3_troca_escala_gate.py:193`, `app/chamados/models.py:2150`, `app/chamados/models.py:2363`)
- `trabalha=False` faz `classificar_falta()` retornar `(None, None, None)`, mas isso apenas impede o retorno antecipado `True` naquele degrau de C6. Como a pergunta está sem célula e sem materialização, C6 chega a `materializada_em is None` e retorna `True`. Portanto ela está viva mesmo no dia de folga. (`app/escala/servico_jornada.py:303`, `app/chamados/juizes.py:1054`, `app/chamados/juizes.py:1074`, `app/chamados/juizes.py:1080`)
- `fio_vivo` consulta `pergunta_viva`; `_com_fio` consulta `fio_vivo` nas resolvedoras do chamado. Assim, o velho entra em `_com_fio`, enquanto o novo não recebeu pergunta. (`app/chamados/juizes.py:1115`, `app/ponto/services/cartorio.py:1094`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:194`)
- O E2 não absolve essa pergunta da fixture: seu filtro exige `celula__isnull=False`. O fechamento do velho pode ocorrer pela exceção de dedup com reata da pergunta ao novo. (`app/chamados/reconciliador.py:588`, `app/chamados/models.py:702`, `app/chamados/models.py:371`, `app/chamados/models.py:418`)
- Com o delta, ambos vão ao lote adiado e C8 põe o novo por último. Sem a alteração de `_adiar` da volta 1, o novo sem fio seria reconciliado imediatamente sozinho, apoiado no velho; depois o velho ficaria sem irmão e seria recusado pela premissa ainda viva da véspera. As expectativas finais ficariam invertidas: velho vivo e novo resolvido. (`app/ponto/services/cartorio.py:1217`, `app/ponto/services/cartorio.py:1225`, `app/ponto/services/cartorio.py:756`, `app/chamados/juizes.py:1651`, `app/chamados/models.py:702`, `app/chamados/models.py:725`, `app/chamados/juizes.py:424`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:175`)

Resultado efetivo de execução do teste ou da mutação: **[não verificado]**. [nao verificado]

## 31/10 00:46 · sessao 9bae963174b4 · O261-E-F1-CLASSE-4 · verificador · origem=Code

O diff `HEAD × HEAD~1` ficou **[nao verificado]**: `git show HEAD` falhou porque a cópia entregue não contém repositório Git. Abaixo estão conclusões sobre o código atual; atribuí-las à introdução neste commit seria **[nao verificado]**. Ocorrência e quantidade em produção também são **[nao verificado]**. [nao verificado]

1. **Quem escreve e quem lê**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `CelulaDia.trabalha` | Porta da célula: regeneração, reversão e geração (`app/ponto/portas/celula.py:231`, `:260`, `:532`, `:612`). | Juiz `EscalaColaborador.eh_dia_trabalho` (`app/escala/models.py:1072`); cartório (`app/ponto/services/cartorio.py:1156`); premissa dos chamados (`app/chamados/juizes.py:389`); leitor da grade (`app/escala/services/leitor_celula.py:344`). |
| `dna['marcos']` | Conteúdo produzido por `montar_dna`, somente quando `trabalha is True` (`app/ponto/management/commands/gerar_celulas.py:79`, `:83`, `:100`); persistido pelas operações da porta (`app/ponto/portas/celula.py:260`, `:530`, `:614`). | Juiz dos marcos do vínculo (`app/escala/models.py:1099`); produtor das lâmpadas (`app/ponto/services/cartorio.py:434`); juiz do marco perguntado (`app/chamados/juizes.py:849`). |
| `ata['lampadas']` | Produzida por `lampadas_do_dia`/`ata_do_dia`; teto aplicado por `_ata_ate_o_teto` (`app/ponto/services/cartorio.py:418`, `:555`, `:703`). Persistência por `lavrar_veredito` e `carimbar` (`app/ponto/portas/celula.py:690`, `:728`). | Lastro: acesos, inexistentes, apagados e pendentes (`app/ponto/services/lastro.py:89`, `:115`, `:128`, `:148`); pergunta (`app/chamados/juizes.py:866`); grade (`app/escala/services/leitor_celula.py:354`); folha (`app/folha/services/sem_furo.py:22`). |
| Veredito `cobrado` | Escolhido pelo cartório quando há fio que ainda protesta (`app/ponto/services/cartorio.py:1195`); persistido pela porta (`app/ponto/portas/celula.py:692`). | Pertence a `VEREDITOS_ACUSAM` (`app/escala/models.py:1735`); lido pelo C6 (`app/chamados/juizes.py:1056`), E2 (`app/chamados/reconciliador.py:598`), fábrica (`app/chamados/services/fabrica_da_celula.py:86`) e retenção da folha (`app/folha/export.py:1215`). |
| Pergunta materializada/retratada | Neste caminho, E2 escreve `materializada_em`, autor e `VIA_CELULA_ABSOLVEU` (`app/chamados/reconciliador.py:620`). Há outros escritores efetivos: materializador, emissão, dedup e portas do modelo (`app/chamados/services/materializacao.py:509`; `app/chamados/services/disputa_emissao.py:1562`; `app/chamados/services/fio.py:894`; `app/chamados/models.py:2456`). | C6 lê carimbo, via, resposta e célula (`app/chamados/juizes.py:1020`, `:1044`, `:1050`, `:1080`); `estado` delega a ele (`app/chamados/models.py:2435`). Desfazer carimbo também consulta C6 (`app/chamados/services/relavra_via.py:227`). |
| Estado de `ChamadoColaborador` | Transições persistem por `_gravar_estado` (`app/chamados/models.py:222`, `:279`); nascimentos têm portas próprias (`app/chamados/models.py:1039`). | Motor decide transições (`app/chamados/models.py:637`); cartório seleciona vivos (`app/ponto/services/cartorio.py:1089`); C8 escolhe cobrança sobrevivente (`app/chamados/juizes.py:1625`); C6 também considera o estado do destino (`app/chamados/models.py:313`). |

O modelo desses caminhos chama-se **`PerguntaDisputa`**, com estado `PENDENTE`; E2 “retrata” pelo carimbo e pela via de resolução (`app/chamados/models.py:2142`, `:2310`; `app/chamados/reconciliador.py:620`).

**Sim, há leitores com critérios diferentes:**

- O cartório exclui do protesto `trabalha=False` ou cobrança com todos os próprios marcos acesos; `dia_sem_trabalho` considera também `trabalha=None` e qualquer `ata.tipo_dia` diferente de trabalho. Os critérios divergem (`app/ponto/services/cartorio.py:1156`; `app/chamados/juizes.py:389`).
- Os comandos `supra_juiz` e `emitir_furo_retroativo` continuam entregando **todos os chamados vivos** ao classificador, enquanto o cartório entrega `_protestam`. Portanto, a mesma classificação recebe insumos diferentes (`app/ponto/management/commands/supra_juiz.py:207`; `app/ponto/management/commands/emitir_furo_retroativo.py:100`; `app/ponto/services/cartorio.py:1159`).
- `q_vivas` mantém toda célula provisória como candidata, sem a exceção pelo próprio marco aceso. Isso é uma seleção mais larga, permitida pelo contrato explícito de **continência**, com julgamento posterior; isoladamente, não é defeito (`app/chamados/models.py:2577`, `:2556`).
- A absolvição de **chamados de turno** ainda exige ausência de qualquer marco pendente; E2 agora individualiza a pergunta. São universos diferentes: dia do chamado versus marco perguntado (`app/chamados/reconciliador.py:983`, `:610`).

2. **Onde o desenho falha ou permanece incompleto**

**A partição entre imediatos e adiados quebra a escolha do sobrevivente C8.** Considere dois `turno_aberto` no dia de folga, com véspera furada: o mais velho tem pergunta viva sob o veredito anterior acusador; o mais novo não tem fio. Esse módulo pertence ao universo lido pelo cartório, e C6 pode manter a pergunta viva pelo veredito anterior (`app/chamados/catalogo/furo_modulos.py:15`; `app/chamados/juizes.py:1054`).

O caminho fica assim:

- `_com_fio` é calculado antes da nova lavra; o velho vai para `_adiar`, o novo para `_porm` (`app/ponto/services/cartorio.py:1094`, `:1214`, `:1222`).
- Cada subconjunto tem um elemento; o ordenador retorna sem consultar C8. O novo fecha por dedup apoiado no velho ainda vivo (`app/chamados/juizes.py:1645`; `app/chamados/models.py:702`).
- Depois da lavra e de E2, o velho é tentado sozinho. Para `turno_aberto`, a folga não regulariza diretamente: a premissa consulta a véspera furada. Sem outro vivo, o fechamento é recusado (`app/ponto/services/cartorio.py:753`, `:756`; `app/chamados/models.py:1225`; `app/chamados/juizes.py:424`; `app/chamados/models.py:721`).

**Resultado do caminho: novo fechado, velho vivo**, contrariando C8, que escolhe o mais novo por `-criado_em, -pk`. Ordenar separadamente os subconjuntos não ordena o lote inteiro. Reprodução com banco: **[nao verificado]** (`app/chamados/juizes.py:1627`; `app/ponto/services/cartorio.py:1222`, `:756`).

**Há leitura obsoleta no memo do C6.** A chave não inclui ata nem DNA, embora a decisão dependa deles. Confirmei com funções extraídas em memória: pergunta provisória inicialmente viva; depois, alterando só a ata para acender seu marco, a mesma instância continua retornando `True`, enquanto `_pergunta_viva_calc` retorna `False`. Reprodução integrada com banco: **[nao verificado]** (`app/chamados/juizes.py:767`, `:793`, `:849`, `:866`, `:1068`).

**O relógio não está inteiramente na borda recebida.** `julgar_celula(agora=...)` calcula `hoje` pelo relógio real e compara o fim do turno com outro `now()`, antes de encaminhar o `agora` fornecido. Assim, o instante injetado não governa todas as guardas (`app/ponto/services/cartorio.py:1597`, `:1618`, `:1621`).

**Não constatei segundo escritor persistente novo nesses caminhos.** O cartório continua chamando a porta da célula; a sobreposição de ata em `celula_do_dia_agora` é somente em memória. E2 já é escritor sancionado no censo de `materializada_em`; esse campo possui escritores plurais declarados (`app/ponto/services/cartorio.py:1470`, `:1485`; `app/chamados/juizes.py:382`; `app/core/tests/test_contract_escritores_estado.py:83`).

**Há risco de ordem inversa entre duas travas de chamados.** `reconciliar` trava o próprio chamado; a reata escolhe outro vivo e a porta de costura trava esse destino. Duas transações A→B e B→A podem solicitar as linhas em ordem inversa; não há ordenação conjunta por PK nesse caminho. Deadlock reproduzido: **[nao verificado]** (`app/chamados/models.py:672`, `:371`, `:418`; `app/chamados/services/fio.py:762`).

**Idempotência não é uniforme.** O fechamento sequencial relê o banco e o motor impede novo `RECONCILIAR` sobre resolvido; a recusa de fechamento deduplica a trilha. Porém `reabrir` recusado por premissa regularizada ou admin sem fato novo escreve `reabertura_recusada` novamente em cada chamada: mesmo estado, várias linhas (`app/chamados/models.py:636`, `:378`, `:1335`, `:1341`; `app/chamados/catalogo/motor.py:111`).

**Existe caminho de vai-e-volta por assimetria dia/turno.** O fechamento por dedup aceita outro vivo do **dia**; a reabertura procura irmão do **turno**, exigindo `entrada_timestamp`. O teste dos dois chamados em folga omite essa chave. Com véspera furada e vínculo sem veto, o fechado por sistema pode reabrir sem fato novo e voltar a fechar por dedup; a exigência de fato novo é específica do fechamento administrativo. Reprodução integrada: **[nao verificado]** (`app/chamados/models.py:702`, `:1283`, `:1294`, `:1373`; `app/chamados/juizes.py:1674`; `app/ponto/tests/test_fechar3_troca_escala_gate.py:131`, `:145`).

3. **Chamado legítimo fechado e pergunta provisória sem batida**

**O gate atual permite fechar chamado legítimo em ausência parcial.** A grade mantém `tipo_dia='ausencia'`, marca cobertura parcial e conserva as células faltantes; o classificador reconhece isso como dia de cobrança. Entretanto, `dia_sem_trabalho` responde verdadeiro para esse tipo, e `premissa_regularizada` aceita a resposta **antes de consultar o marco apagado** (`app/escala/utils.py:1180`, `:1190`, `:1195`; `app/ponto/supra_juiz.py:279`; `app/chamados/juizes.py:389`; `app/chamados/models.py:1225`).

Logo, se `reconciliar()` for chamado para esse furo e não houver pergunta viva que retenha o fechamento, ele pode ser resolvido mesmo com marco realmente faltante; a mesma premissa depois impede reabrir. É defeito do gate atual, sem prova de introdução pelo HEAD ou ocorrência no dado de produção: **[nao verificado]** (`app/chamados/models.py:646`, `:649`, `:618`, `:1289`).

**Nos ramos `_adiar`/`_porm`, trabalho verdadeiro com próprio marco apagado ou pendente continua protestando.** `marcos_acesos` não retorna verdadeiro nesse caso, portanto esses conjuntos não o selecionam somente pela mudança examinada (`app/ponto/services/lastro.py:175`, `:179`; `app/ponto/services/cartorio.py:1156`, `:1214`, `:1222`).

**Pergunta provisória cujo próprio marco continua sem batida permanece protegida na nova condição.** Lâmpada `acesa=None`/`False` não prova o fato; C6 e E2 exigem literalmente `is True` para dispensar o teto. E2 ainda consulta `pergunta_viva` antes de carimbar (`app/chamados/juizes.py:869`, `:1068`; `app/chamados/reconciliador.py:610`, `:616`).

4. **Ordem, conjunto reconciliado e testes existentes** [nao verificado]

**Há um teste existente cuja expectativa de caminho precisa mudar:** ele exige `[RECONCILIA] `, mas os chamados de folga agora entram em `_porm`, que registra `[RECONCILIA-MARCO]`. As expectativas de estado e sobrevivente desse fixture continuam pertinentes (`app/ponto/tests/test_fechar3_troca_escala_gate.py:153`, `:154`, `:158`; `app/ponto/services/cartorio.py:1222`, `:1228`).

**Esse teste não cobre a falha da partição:** cria ambos sem perguntas. Falta a variante “velho com fio, novo sem fio”, que percorre os dois subconjuntos e pode preservar o velho; isso pede cobertura adicional e correção da ordem global, não trocar a expectativa para aceitar o sobrevivente errado (`app/ponto/tests/test_fechar3_troca_escala_gate.py:142`; `app/ponto/services/cartorio.py:1214`, `:1222`, `:756`; `app/chamados/juizes.py:1627`).

**O teste da segunda passada continua compatível:** a lista é ordenada por data antes do corpo; seu chamado falso único não provoca consulta C8, pois o ordenador retorna para lote menor que dois (`app/ponto/services/cartorio.py:766`; `app/chamados/tests/test_o261e_f1_codex_v5.py:169`, `:183`; `app/chamados/juizes.py:1645`).

**Os testes antigos de provisória sem batida devem continuar exigindo pergunta viva e nenhum carimbo.** Eles usam justamente o próprio marco `acesa=None, pendente=True`, que não satisfaz a exceção nova (`app/chamados/tests/test_o261_pergunta_viva_teto.py:54`; `app/chamados/tests/test_o261_e2_absolvicao_exige_fato_encerrado.py:27`, `:64`). Resultado da suíte completa: **[nao verificado]**.

## 31/10 00:45 · sessao 6a0aa9eab820 · - · - · origem=Ronald (tmux)

A partição quebra a escolha do sobrevivente C8 no cenário proposto, por leitura do caminho; execução com banco **[não verificado]**.

1. `turno_aberto` pertence a `FURO_MODULES` (`app/chamados/catalogo/furo_modulos.py:15`) e ao universo do gate (`app/chamados/catalogo/modulos.py:237`, `app/chamados/models.py:1187`).
2. A pergunta do velho pode estar viva com `trabalha=False` e veredito anterior acusador: C6 retorna True pelo veredito, sem consultar `trabalha` (`app/chamados/juizes.py:1054`). `_com_fio` é calculado antes da nova lavra (`app/ponto/services/cartorio.py:1094`).
3. Na folga, ambos deixam de protestar (`app/ponto/services/cartorio.py:1155`). O velho com fio vai para `_adiar` (`app/ponto/services/cartorio.py:1214`); o novo sem fio vai para `_porm` (`app/ponto/services/cartorio.py:1222`).
4. Cada subconjunto contém um elemento: `sobrevivente_por_ultimo` retorna sem consultar C8 (`app/chamados/juizes.py:1645`). O novo fecha por dedup, apoiado no velho ainda vivo (`app/chamados/models.py:702`).
5. A lavra grava o veredito da folga (`app/ponto/services/cartorio.py:1470`); E2 pode então carimbar a pergunta do velho, pois a célula não acusa e não tem pendentes (`app/chamados/reconciliador.py:584`, `app/chamados/reconciliador.py:610`, `app/chamados/juizes.py:1068`).
6. O laço adiado tenta fechar o velho sozinho (`app/ponto/services/cartorio.py:755`). Para `turno_aberto`, folga não regulariza diretamente a premissa (`app/chamados/models.py:1225`); ela consulta a véspera furada (`app/chamados/juizes.py:424`). Sem outro vivo, o gate recusa (`app/chamados/models.py:702`, `app/chamados/models.py:721`).

**Resultado: novo fechado, velho vivo**, contrariando o C8, que escolhe o mais novo por `-criado_em, -pk` (`app/chamados/juizes.py:1627`). O teste existente de sobrevivência cria ambos sem perguntas e não cobre a partição por fio (`app/ponto/tests/test_fechar3_troca_escala_gate.py:142`).

## 31/10 00:44 · sessao edb37687a75c · - · - · origem=Ronald (tmux)

Achados conferidos, somente por leitura:

- **Chamado legítimo pode fechar por ausência parcial.** A grade conserva `tipo_dia='ausencia'`, `cobertura_parcial=True` e os marcos faltantes em `app/escala/utils.py:1180` e `app/escala/utils.py:1184`; o suprajuiz considera essa combinação dia de cobrança em `app/ponto/supra_juiz.py:274`. Entretanto, `dia_sem_trabalho` responde True para qualquer `ata.tipo_dia != 'trabalho'`, mesmo com `cel.trabalha=True` (`app/chamados/juizes.py:386`). `premissa_regularizada` aceita essa resposta antes de consultar as lâmpadas (`app/chamados/models.py:1220`). Assim, um chamado de furo sem pergunta viva ancorada pode passar pelo gate e ser reconciliado (`app/chamados/models.py:646`, `app/chamados/models.py:649`, `app/chamados/models.py:618`); depois a mesma premissa impede sua reabertura (`app/chamados/models.py:1289`). A ata não conserva `cobertura_parcial` (`app/ponto/services/cartorio.py:557`). Introdução desse defeito no HEAD: [não verificado].

- **Provisória sem a batida continua protegida no novo E2.** O carimbo exige ausência de pendentes ou prova positiva do próprio marco; depois ainda consulta C6 (`app/chamados/reconciliador.py:608`, `app/chamados/reconciliador.py:616`). `fato_do_marco_no_espelho` exige o marco do DNA, lâmpadas correspondentes e todas acesas (`app/chamados/juizes.py:842`, `app/chamados/juizes.py:863`, `app/chamados/juizes.py:869`). Uma lâmpada própria com `acesa=None, pendente=True` não satisfaz isso. Os testes de provisória sem batida seguem coerentes (`app/chamados/tests/test_o261_pergunta_viva_teto.py:52`, `app/chamados/tests/test_o261_e2_absolvicao_exige_fato_encerrado.py:62`).

- **A irmã dos chamados conserva o teto global.** `_absolvido_pela_celula` continua recusando qualquer célula com marcos pendentes (`app/chamados/reconciliador.py:981`). O novo E2 individualiza a pergunta, enquanto essa irmã examina o dia inteiro (`app/chamados/reconciliador.py:608`, `app/chamados/reconciliador.py:983`).

- **Não nasceu escritor novo de estado de chamado nesse caminho.** `_retratar` chama `ch.reconciliar` (`app/chamados/reconciliador.py:524`); `reconciliar` usa o motor e `_gravar_estado` (`app/chamados/models.py:614`). A escrita persistida de `status_local` está em `app/chamados/models.py:222`, e a trilha precede o save no mesmo atomic (`app/chamados/models.py:274`). Os nascimentos são exceções próprias (`app/chamados/models.py:890`, `app/chamados/models.py:1039`).

- **E2 já é escritor sancionado de materialização.** A gravação continua em `app/chamados/reconciliador.py:620`; o censo declara esse arquivo em `app/core/tests/test_contract_escritores_estado.py:83`. Há pluralidade de escritores preexistente; não é correto dizer que todo `materializada_em` tem uma porta única global (`app/core/tests/test_contract_escritores_estado.py:83`).

- **Idempotência sequencial do fechamento:** a segunda chamada relê o banco e o motor recusa novo RECONCILIAR sobre resolvido (`app/chamados/models.py:636`, `app/chamados/catalogo/motor.py:111`). A recusa de fechamento também deduplica trilha (`app/chamados/models.py:378`). **Contraexemplo atual ao contrato “2×, uma linha”**: `reabrir` recusado por admin sem fato novo ou premissa regularizada grava `reabertura_recusada` sem dedup a cada chamada (`app/chamados/models.py:1335`, `app/chamados/models.py:1341`). Introdução no HEAD: [não verificado].

- **Risco estático de ciclo entre duas travas de chamados:** reconciliar trava primeiro o próprio chamado (`app/chamados/models.py:669`), escolhe outro vivo para receber a pergunta (`app/chamados/models.py:371`) e a costura trava esse destino (`app/chamados/services/fio.py:758`, `app/chamados/services/fio.py:837`). C8 aceita o outro excluindo o próprio (`app/chamados/juizes.py:1625`). Duas transações A→B e B→A podem portanto pedir as duas linhas em ordem inversa; reprodução de deadlock: [não verificado]. Os testes atuais conferem trava própria antes do plano e trava do destino isoladamente (`app/chamados/tests/test_o261e_f1_codex_v5.py:25`, `app/chamados/tests/test_o261e_f1_codex_v5.py:65`).

- **SQL com regra global própria:** `q_vivas` ainda usa qualquer lâmpada pendente para manter candidata (`app/chamados/models.py:2575`), enquanto C6 passou a absolver pelo próprio marco (`app/chamados/juizes.py:1068`). Isso amplia candidatos mortos, porém o contrato documentado é continência, com julgamento posterior, e não igualdade (`app/chamados/models.py:2556`). A property `.viva` delega ao C6 (`app/chamados/models.py:2597`).

- **Ordenação:** os adiados passam agora por `sobrevivente_por_ultimo` (`app/ponto/services/cartorio.py:754`); o fechamento imediato também (`app/ponto/services/cartorio.py:1221`). Esse ordenador consulta C8 e põe o sobrevivente por último, preservando a ordem dos demais (`app/chamados/juizes.py:1644`). A segunda passada continua ordenada por data (`app/ponto/services/cartorio.py:766`), como exige o teste existente (`app/chamados/tests/test_o261e_f1_codex_v5.py:164`).

## 31/10 00:44 · sessao 6a0aa9eab820 · - · - · origem=Ronald (tmux)

Conferências do recorte:

- **Um teste existente precisa mudar de caminho/log:** `test_MORDE_rec_dois_turno_aberto_em_folga_com_vespera_furada_sobrevive_o_mais_novo` exige `[RECONCILIA] ` em `app/ponto/tests/test_fechar3_troca_escala_gate.py:153`. Com `trabalha=False`, `_protesta` devolve False (`app/ponto/services/cartorio.py:1155`), o lote passa por `_porm` (`app/ponto/services/cartorio.py:1222`) e escreve `[RECONCILIA-MARCO]` (`app/ponto/services/cartorio.py:1228`). `_protestam` vazio não gera a cobrança indevida que acionaria `_rec` (`app/ponto/services/cartorio.py:1157`, `app/ponto/services/cartorio.py:1249`). A expectativa de sobrevivência do mais novo continua compatível com `sobrevivente_por_ultimo` (`app/chamados/juizes.py:1648`) e com o gate de dedup (`app/chamados/models.py:702`).

- **O teste de adiados não exige mock novo:** seu lote contém um único chamado falso (`app/chamados/tests/test_o261e_f1_codex_v5.py:169`); `sobrevivente_por_ultimo` devolve lotes com menos de dois elementos antes de consultar ORM (`app/chamados/juizes.py:1645`). A ordenação da segunda passada continua protegida pelo `sorted` (`app/ponto/services/cartorio.py:766`) e pelo teste (`app/chamados/tests/test_o261e_f1_codex_v5.py:183`).

- **Os testes antigos do teto continuam válidos:** exercitam justamente o marco perguntado com `acesa=None, pendente=True` (`app/chamados/tests/test_o261_e2_absolvicao_exige_fato_encerrado.py:27`), que não satisfaz `fato_do_marco_no_espelho` (`app/chamados/juizes.py:869`). Portanto continuam esperando ausência de carimbo (`app/chamados/tests/test_o261_e2_absolvicao_exige_fato_encerrado.py:64`) e pergunta viva (`app/chamados/tests/test_o261_pergunta_viva_teto.py:54`).

- **Leitor que não migrou literalmente, mas tem contrato mais largo:** `q_vivas` mantém a proteção de toda célula provisória, sem a exceção pelo marco aceso (`app/chamados/models.py:2575`). Entretanto seu contrato é prefiltragem generosa por continência; o consumidor deve consultar o juiz para resposta exata (`app/chamados/models.py:2556`, `app/chamados/models.py:2594`). Isso isoladamente não prova divergência indevida.

- **Memo incompleto:** a decisão nova depende de `cel.ata` e `cel.dna` (`app/chamados/juizes.py:1068`, `app/chamados/juizes.py:849`, `app/chamados/juizes.py:866`), mas `_chave_memo_pv` não contém esses campos (`app/chamados/juizes.py:767`). Alterar somente a ata, mantendo o veredito, permite devolver a resposta cacheada (`app/chamados/juizes.py:793`). O teste que promete invalidar por qualquer campo lido não cobre ata/DNA (`app/chamados/tests/test_selo_memo_pergunta_viva.py:117`). Não prova loop persistente: E2 consulta novas instâncias (`app/chamados/reconciliador.py:584`).

- **Vai-e-volta possível por assimetria preexistente:** fechamento por dedup usa outro chamado vivo do **dia** (`app/chamados/models.py:702`); reabertura verifica irmão do **turno** (`app/chamados/models.py:1294`), que exige `entrada_timestamp` (`app/chamados/juizes.py:1674`). O fixture dos dois `turno_aberto` em folga omite essa chave (`app/ponto/tests/test_fechar3_troca_escala_gate.py:145`), mantém a véspera furada (`app/ponto/tests/test_fechar3_troca_escala_gate.py:131`) e fecha o velho por dedup (`app/ponto/tests/test_fechar3_troca_escala_gate.py:154`). A premissa do turno em folga continua consultando a véspera (`app/chamados/models.py:1225`, `app/chamados/juizes.py:424`). Com vínculo sem veto, o velho pode reabrir: exigência de fato novo só vale para encerramento administrativo (`app/chamados/models.py:1282`); caminho permitido grava reabertura (`app/chamados/models.py:1373`). Rejulgamento pode fechá-lo por dedup novamente. Execução com banco: **[não verificado]**.

- **Nomenclatura:** o modelo destes caminhos é `PerguntaDisputa` (`app/chamados/models.py:2142`), com estado `PENDENTE` (`app/chamados/models.py:2310`); a existência de um modelo chamado `PerguntaPendente` não foi encontrada no censo textual da cópia **[não verificado]**.

- **Outra regra de “sem trabalho” já existente:** `dia_sem_trabalho` considera `trabalha is not True` ou ata com tipo diferente de trabalho (`app/chamados/juizes.py:386`), enquanto o predicado novo só dispensa por `trabalha is False` ou marcos acesos (`app/ponto/services/cartorio.py:1155`).

## 31/10 00:43 · sessao 96bb4a47aa74 · - · - · origem=Ronald (tmux)

Censo e consequências conferidas:

- **`CelulaDia.trabalha` e `dna`**: escritos na porta por `regenerar_celulas_vinculo` (`app/ponto/portas/celula.py:231`, `:243`, `:260`), `reverter_regeneracao_dia` (`:530`) e `gerar_celulas_periodo` (`:590`). O produtor do conteúdo é `montar_dna`, que só põe marcos quando `trabalha is True` (`app/ponto/management/commands/gerar_celulas.py:73`, `:79`, `:100`). Leitores canônicos: `eh_dia_trabalho` devolve `cel.trabalha` (`app/escala/models.py:1048`, `:1072`); `marcos_do_dia` lê DNA e, com marcos vazios/nulos, recorre ao template (`:1099`, `:1108`). Outros leitores relevantes: cartório (`app/ponto/services/cartorio.py:1156`), pergunta por marco (`app/chamados/juizes.py:849`), leitura da grade soberana (`app/escala/services/leitor_celula.py:344`).

- **`ata['lampadas']`**: conteúdo produzido por `lampadas_do_dia`/`ata_do_dia`, usando DNA e casamento da grade (`app/ponto/services/cartorio.py:418`, `:434`, `:555`); teto transforma somente apagadas ainda pendentes em `acesa=None, pendente=True` (`:696`, `:703`). Persistência passa por `lavrar_veredito` e `carimbar` (`:1468`, `:1470`, `:1485`; `app/ponto/portas/celula.py:647`, `:690`, `:728`). `carimbar_precedencia` altera outra chave da mesma ata (`app/ponto/portas/celula.py:732`, `:740`). `celula_do_dia_agora` sobrepõe a ata apenas no objeto em memória (`app/chamados/juizes.py:379`, `:382`); a grade também faz uma ata transitória para alimentar o juiz, chamando o produtor existente (`app/escala/utils.py:1827`, `:1832`). Não são segundos escritores persistentes nesses sítios.

- **Leitores das lâmpadas**: lastro da cobrança (`app/ponto/services/lastro.py:82`, `:153`), marcos inexistentes (`:98`), apagados (`:125`) e pendentes (`:138`); fato da pergunta (`app/chamados/juizes.py:831`, `:866`); concordância resposta/luz (`:957`, `:981`); grade soberana (`app/escala/services/leitor_celula.py:331`, `:354`); folha prévia (`app/folha/previa.py:84`); sem furo (`app/folha/services/sem_furo.py:22`); horário de entrada (`app/ponto/services/vigia_entrada.py:80`, `:123`).

- **`cobrado`**: decisão explícita do cartório depende agora do fio e `_protesta` (`app/ponto/services/cartorio.py:1195`, `:1196`); a porta valida e persiste (`app/ponto/portas/celula.py:678`, `:692`). É acusador (`app/escala/models.py:1735`). Leitores: pergunta viva (`app/chamados/juizes.py:1054`), concordância da resposta (`:1002`), SQL de perguntas (`app/chamados/models.py:2579`), fábrica (`app/chamados/services/fabrica_da_celula.py:86`), emissão (`app/chamados/services/disputa_emissao.py:705`), retenção da folha (`app/folha/export.py:1215`).

**Divergências reais entre leitores:**

1. Cartório usa `_protesta = cel.trabalha is not False and marcos_acesos(...) is not True` (`app/ponto/services/cartorio.py:1155`). `dia_sem_trabalho` entende “sem trabalho” como `trabalha is not True` **ou** ata de tipo diferente de trabalho (`app/chamados/juizes.py:386`). Divergem para `trabalha=None` e `trabalha=True` com ata não trabalho. O leitor da grade também prioriza `ata.tipo_dia` sobre o campo (`app/escala/services/leitor_celula.py:344`). Consequência operacional desses casos no dado fornecido: **[não verificado]**.

2. O comando supra e o emissor retroativo continuam passando **todos os vivos** ao classificador (`app/ponto/management/commands/supra_juiz.py:195`, `:207`; `app/ponto/management/commands/emitir_furo_retroativo.py:93`, `:100`), enquanto cartório passa `_protestam` (`app/ponto/services/cartorio.py:1157`, `:1159`). Portanto podem gerar `COBRANCA_SEM_FURO`/`COBRANCA_EM_DIA_SEM_TRABALHO` quando o cartório exclui a cobrança desse insumo; os predicados desses códigos estão em `app/ponto/supra_juiz.py:418` e `:441`. O supra ainda executa retratação com `--executar` (`app/ponto/management/commands/supra_juiz.py:243`, `:248`; trava desligada em `:41`), sujeito a conversa e cobrança órfã (`:225`, `:239`). **Vai-e-volta concreto provocado por isso: [não verificado]**.

3. `q_vivas` preserva toda célula provisória na seleção, mesmo quando o fato da pergunta acendeu (`app/chamados/models.py:2577`); juiz agora absolve esse caso (`app/chamados/juizes.py:1068`). Isso é seleção mais ampla, compatível com o contrato declarado de continência e filtro posterior (`app/chamados/models.py:2556`).

**Legítimos e provisórios:**

- Para `trabalha=True` com marco nomeado realmente apagado/pendente, `marcos_acesos` devolve `False` (`app/ponto/services/lastro.py:175`, `:179`); `_protesta=True` e o chamado não entra nos novos conjuntos `_adiar` e `_porm` (`app/ponto/services/cartorio.py:1156`, `:1214`, `:1222`). Fechamento novo indevido desse legítimo por esses ramos: não demonstrado.
- Pergunta provisória sem a batida tem fato `False`/`None`, nunca `True` (`app/chamados/juizes.py:863`, `:869`); a exceção nova exige literalmente `is True` (`:1068`). Os testes existentes preservam o caso sem batida (`app/chamados/tests/test_o261_pergunta_viva_teto.py:52`, `:62`; `app/chamados/tests/test_o261_e2_absolvicao_exige_fato_encerrado.py:62`).

**Relógio e idempotência:**

- `julgar_colab` recebe `hoje/agora`, e `marcos_vencidos` compara com esse instante (`app/ponto/services/cartorio.py:709`, `:680`). Contudo a unidade `julgar_celula(agora=...)` ainda consulta relógio real para `hoje` e guarda de fim (`:1597`, `:1618`), antes de passar o `agora` fornecido (`:1621`). O corpo também chama `afastado_na_batida(c)` sem data (`:1056`), que delega com `dia=None` (`app/ponto/services/afastado_avisa.py:38`). Não são novidades demonstráveis de HEAD sem diff.
- Porta da célula evita nova gravação/evento com mesma ata/veredito/protestos (`app/ponto/portas/celula.py:687`) e mesmo carimbo (`:717`). Prova global de “duas chamadas = uma linha de trilha”: **[não verificado]**.

A cópia não contém `.git`; o diff exato HEAD/HEAD~1 permaneceu **[não verificado]**. Não executei suite nem acessei banco/log. [nao verificado]

## 31/10 00:21 · sessao 93fd0110e578 · O261-E-F5B · sonda · origem=Code

Transcrição do dado raspado fornecido, sem conferência em PROD. [nao verificado]

| Contador | Dias | Referência |
|---|---:|---|
| universo | 1844 | [nao verificado] |
| porta_fila | 810 | [nao verificado] |
| porta_pergunta | 169 | [nao verificado] |
| de_hoje_pra_frente | 0 | [nao verificado] |
| fio_morto_alcance | 617 | [nao verificado] |
| decisao_do_dia_pendente | 222 | [nao verificado] |
| aguarda_decisao_de_ausencia | 23 | [nao verificado] |
| escala_nao_descreve_o_dia | 2 | [nao verificado] |
| fio_do_admin | 1 | [nao verificado] |
| porta_exige_o_admin | 171 | [nao verificado] |

Os quatro baldes fora do fio estão definidos em [vigia_fio.py:502](/home/ronald/codex_ws/68821bbd7d83c65b55dde18bc3733e2ba5df9aa0/app/chamados/services/vigia_fio.py:502).

| rota | papel | Dias | Referência |
|---|---|---:|---|
| aguarda_decisao_de_ausencia | Aguardando decisao do DP | 14 | [nao verificado] |
| aguarda_decisao_de_ausencia | Aguardando documento do colaborador | 9 | [nao verificado] |
| decisao_do_dia_pendente | regularizar_batida | 222 | [nao verificado] |
| escala_nao_descreve_o_dia | fora da lista do admin | 2 | [nao verificado] |
| fio_do_admin | admin-batida | 1 | [nao verificado] |
| porta_fila | cobrar | 673 | [nao verificado] |
| porta_fila | cobrar,decidir | 18 | [nao verificado] |
| porta_fila | cobrar,validar | 1 | [nao verificado] |
| porta_fila | decidir | 65 | [nao verificado] |
| porta_fila | decidir,validar | 3 | [nao verificado] |
| porta_fila | validar | 50 | [nao verificado] |
| porta_pergunta | cobrar | 127 | [nao verificado] |
| porta_pergunta | cobrar,validar | 1 | [nao verificado] |
| porta_pergunta | historico | 8 | [nao verificado] |
| porta_pergunta | validar | 33 | [nao verificado] |
| sem_porta | nenhuma porta | 617 | [nao verificado] |
| **Soma** | | **1844** | [nao verificado] |

**A soma bate: 1844 = universo 1844; diferença 0.** [nao verificado]

`810 + 169 + 0 + 617 + 222 + 23 + 2 + 1 = 1844`. [nao verificado]

| Papel | Dias na tabela fornecida | Referência |
|---|---:|---|
| admin-cadastro | 0 | [nao verificado] |
| admin-batida | 1 | [nao verificado] |
| fora da lista do admin | 2 | [nao verificado] |
| lista sem resposta | 0 | [nao verificado] |

## 31/10 00:21 · sessao 540e573b89ba · O261-E-F5B · verificador · origem=Code

Há achados no código atual. A atribuição exata aos três commits e o resultado dos testes no pai ficam **[nao verificado]**: a cópia não contém `.git`. Não consultei banco nem logs; casos abaixo são contraprovas construídas a partir do código. [nao verificado]

1. **Falhas encontradas e classes pedidas**

- **Falha da lista não chega ao contador.** No ramo de folga contestada validada, `dono_fora_do_fio` consulta `papel_do_dia` e retorna antes de `dia_do_admin`. Se a lista falha, o papel vira `"lista sem resposta"`, mas somente `dia_do_admin` registra o dia em `_sem_resposta`. Caso concreto: único dia retido, sem porta, com `FOLGA_CONTESTA` validada e lista indisponível; o alcance publica a falha, porém o comando carimba `fio_do_admin_sem_resposta=0`. Isso contradiz o contrato de contar os dias consultados sem resposta. Referências: `app/chamados/services/vigia_fio.py:192`, `:224`, `:231`, `:242`, `:292-295`; `app/chamados/management/commands/vigiar_fio_morto.py:169`.

- **A porta pode declarar alcançável um chamado ocultado pelo painel padrão.** Um chamado aberto, com célula e `silenciado_ate` vencido, entra em `qs_na_fila`; a partição atribui `historico`; o painel padrão exclui esse verbo. O juiz aceita a entrada e devolve `porta_fila`, `fio='chamado'`. A fórmula literal do corte é seguida, mas sua equivalência com uma porta visível no painel falha no código atual. Referências: `app/chamados/services/painel.py:186-188`, `:326-329`; `app/chamados/services/verbos.py:45-48`; `app/chamados/juizes.py:1213`, `:1220`. A introdução dessa inconsistência nestes commits é **[nao verificado]**.

- **Fonte única dos donos ainda duplicada.** O vigia declara `DONOS_DA_MESA_DO_ADMIN=('BATIDA','CADASTRO')`; pautas declara `DONOS_DE_PAUTA=('CADASTRO','BATIDA')`. Ambos implementam a separação L-099 entre trabalho do admin e estrutura. Hoje os conjuntos coincidem; são duas declarações independentes. Uma alteração em apenas uma delas permite retirar um dia do fio morto sem a mesma classificação na emissão de pautas. Referências: `app/chamados/services/vigia_fio.py:168`, `:226`, `:272-275`; `app/ponto/services/pautas_dos_discordantes.py:41`, `:136`; `CLAUDE.md:311-316`.

- **O selo de unicidade não alcança essa duplicação.** O teste “predicado existe UMA vez” inspeciona exclusivamente o AST de `vigia_fio.py`; portanto passa com a declaração paralela de pautas presente. Prova unicidade local, não fonte única entre os consumidores. Referências: `app/chamados/tests/test_o261e_f5_alcance.py:235-246`; `app/ponto/services/pautas_dos_discordantes.py:41`.

- **Há relógio fora da borda na fonte consumida.** O comando captura `agora/hoje` uma vez e passa a data à lista. Entretanto `cadastro_realidade.lista` chama `_jornada_que_mente` sem essa data; o helper usa `date.today()` e corte literal 21. Caso: rodada com `hoje` injetado, ou atravessando meia-noite, pode montar C2 para outra janela e alterar a presença do colaborador usada na causa `d`. Referências: `app/chamados/management/commands/vigiar_fio_morto.py:158-168`; `app/chamados/services/vigia_fio.py:214-216`, `:630`; `app/escala/services/cadastro_realidade.py:186-187`, `:518`. É problema da fonte atual; introdução no diff **[nao verificado]**.

- **Restaram leitores com afirmações que a nova saída não garante.** `linhas()` continua afirmando “o dono é o CADASTRO” para todo `escala_nao_descreve_o_dia`, embora o papel agora possa ser `admin-batida` ou `fora da lista do admin`. Além disso, `DESTRAVA['chamado']` afirma “não há pergunta aberta”, mas o juiz admite pergunta viva sem crédito, com verbo `decidir` e fio `chamado`. Casos concretos: o cenário BATIDA já usado pelo teste de rótulo; uma pergunta viva excluída do crédito por materialização de fato. Referências: `app/chamados/services/vigia_fio.py:830-832`; `app/chamados/tests/test_o261e_f5_alcance.py:334-338`; `app/folha/fora_da_folha.py:43`; `app/chamados/juizes.py:1200-1202`, `:1175-1181`.

Nas demais classes:

| Classe | Resultado |
|---|---|
| Segundo juiz duplicando `porta_alcancavel_do_dia` | **Sem achado** nos leitores examinados: folha e alcance chamam o registrado. `app/folha/fora_da_folha.py:54-67`; `app/chamados/services/vigia_fio.py:595`; `app/core/juizes.py:253`. |
| Segundo escritor introduzido pela mudança | **Sem achado** nos caminhos examinados; alimentação e juiz consultam as fontes. Há múltiplos sítios anteriores de escrita dos carimbos, discriminados abaixo. `app/chamados/juizes.py:1129-1163`, `:1208-1220`. |
| Tabela verbo→fio na folha | **Sem achado**: está no juiz; a folha lê `porta['fio']`. `app/chamados/juizes.py:1170-1181`; `app/folha/fora_da_folha.py:65-67`. |
| Ordem das decisões do alcance | **Sem achado**: porta antes de hoje/futuro, depois os baldes de lei. `app/chamados/services/vigia_fio.py:595-609`. |
| Pouso antes dos pré-requisitos | **[nao verificado]**: sem histórico, execução da suíte ou evidência de pouso. | [nao verificado]
| Teto escondido no alcance | **Sem achado**: contador sobe antes do corte; `sem_porta` sempre entra; demais omissões são contadas. `app/chamados/services/vigia_fio.py:583-589`. |

2. **Quem escreve e quem lê**

O quadro registra escritores e leitores localizados na cadeia auditada e em consumidores externos a ela; exaustividade de todos os usos do repositório **[nao verificado]**. Quais arquivos efetivamente estão fora do diff também é **[nao verificado]**, pela ausência do histórico.

| Campo | Escritores localizados | Leitores localizados |
|---|---|---|
| `ChamadoColaborador.status_local` | Transição em `app/chamados/models.py:185-193`; nascimento em `:699-704`, `:834-851`. | Fila em `app/chamados/services/painel.py:155`, `:187`; modal em `app/chamados/services/fio.py:385-399`; sinais em `app/chamados/signals.py:161-180`; API em `app/api/views.py:1047`, `:1145`; calendário em `app/colaboradores/services/calendario.py:557`. O novo juiz recebe o resultado da fila em `app/chamados/juizes.py:1144-1148`. |
| `silenciado_ate` | Arquivar/desarquivar em `app/chamados/services/arquivo.py:166-171`, `:201-208`. | Fila em `app/chamados/services/painel.py:188`; verbo em `app/chamados/services/verbos.py:42-48`; propriedade em `app/chamados/models.py:304-307`; sinal em `app/chamados/signals.py:237-242`; contador em `app/chamados/services/fila.py:122-127`; pesquisa em `app/chamados/views_arquivar.py:156-157`. |
| `ChamadoColaborador.celula` | Nascimento em `app/chamados/models.py:716-722`, `:829-831`; ligação posterior em `app/chamados/services/vinculos.py:92-99`. | Dia do fato e alimentação em `app/chamados/juizes.py:89-90`, `:1146-1148`; reconciliação em `app/chamados/models.py:866-868`; modal pelo dia do juiz em `app/chamados/services/fio.py:365-374`. |
| `respondida_em` / `resposta_colab` | Resposta em `app/chamados/services/disputa_emissao.py:1247-1266`; reabertura em `:1358-1377`; resposta automática/reparo em `:1392-1410`; ressuscitação em `app/chamados/models.py:1258-1259`; complementação dos carimbos em `app/chamados/services/materializacao.py:511-529`, `:1190-1198`, e `app/chamados/services/disputa_emissao.py:1570-1577`. | C6 em `app/chamados/juizes.py:1020-1032`; resposta vazia em `:1126`; SQL em `app/chamados/models.py:2342-2355`; créditos em `app/chamados/services/raiox.py:133-149`; API em `app/api/views.py:1173-1175`, `:2632-2634`; modal em `app/templates/chamados/partials/modal_fio.html:210`; prévia em `app/folha/previa.py:142`; cartório em `app/ponto/services/cartorio.py:1263`; ficha em `app/colaboradores/services/ficha.py:171`. |
| `validada_em` | Validação/devolução em `app/chamados/services/validacao.py:127-133`, `:180-190`; nova resposta desfaz em `app/chamados/services/disputa_emissao.py:1250-1266`; reabertura em `:1358-1377`; ressuscitação em `app/chamados/models.py:1258`; também carimbada por `app/chamados/services/materializacao.py:514-529`, `:1194-1198`, e `app/chamados/services/disputa_emissao.py:1574-1577`. | C6 em `app/chamados/juizes.py:1006-1007`; estado/SQL em `app/chamados/models.py:2204-2211`, `:2353`; ações em `app/chamados/services/acoes_disputa.py:341`; procedência em `app/ponto/services/procedencia_batida.py:74`; placar em `app/chamados/services/painel.py:66-67`. |
| `materializada_em` | Portas do modelo em `app/chamados/models.py:2228`, `:2242`, `:2267`, `:2282`, `:2302`, `:2312`; ressuscitação em `:1258`; reconciliador em `app/chamados/reconciliador.py:402`, `:617`; fio em `app/chamados/services/fio.py:846`; relavra em `app/chamados/services/relavra_via.py:231`; materialização em `app/chamados/services/materializacao.py:509`, `:858`, `:1043`, `:1149`, `:1339`; emissão em `app/chamados/services/disputa_emissao.py:1562`, `:1644`. | C6 em `app/chamados/juizes.py:1015`, `:1044`, `:1050-1052`, `:1076-1083`; clique morto/crédito em `app/chamados/services/raiox.py:133-134`, `:173-185`; estado em `app/chamados/models.py:2206-2209`; procedência em `app/ponto/services/procedencia_batida.py:74`; validação em `app/chamados/services/validacao.py:182`; placar em `app/chamados/services/painel.py:67`. |
| `chamado_destino` | Nascimento/reuso em `app/chamados/models.py:2160-2182`; reapontamento em `app/chamados/services/fio.py:785-800`. | Âncora em `app/chamados/services/fio.py:117`; crédito em `app/chamados/services/raiox.py:135-147`; API em `app/api/views.py:1046`, `:2626`, `:2642-2644`; materialização em `app/chamados/services/materializacao.py:939`, `:1021`, `:1169-1172`; app em `app/chamados/services/pergunta_no_app.py:116`, `:139-142`; validação em `app/chamados/services/validacao.py:788`; tranca em `app/chamados/services/competencia_trancada.py:115`. |

Assim, a descrição de `validar_pergunta` como “escritor único de `validada_em`” não descreve todos os sítios atuais: materialização e emissão também atribuem esse carimbo. Isso é constatável no código; ser uma mudança destes commits é **[nao verificado]**. Referências: `app/docs/JUIZES.md:38-39`; `app/chamados/services/materializacao.py:514-529`; `app/chamados/services/disputa_emissao.py:1574-1577`.

| Valor derivado | Quem produz | Quem lê |
|---|---|---|
| Mapa de `particionar_verbos` | Crédito + natureza + silêncio em `app/chamados/services/verbos.py:31-59`. | Alimentação do juiz em `app/chamados/juizes.py:1153`; painel em `app/chamados/services/painel.py:230`, `:320`, `:326`, `:381`, `:472`; lente em `app/chamados/services/verbos.py:174`; inteligência em `app/chamados/services/na_fila.py:19-25`; ficha em `app/colaboradores/services/ficha.py:158`; mensageria em `app/api/views_mensageria.py:395`, `:453`, `:1143`; cobrar/arquivar em `app/chamados/views_cobrar.py:78`, `app/chamados/views_arquivar.py:116`. Navbar lê contagens em `app/core/context_processors.py:89-90`. |
| Retorno da porta: `rota`, `verbos`, `fio` | Juiz em `app/chamados/juizes.py:1212-1220`; escada em `:1170-1181`. | Folha lê `fio` em `app/folha/fora_da_folha.py:67`; `o_que_segura` lê IDs de `verbos` em `:193`; alcance lê rota/verbos em `app/chamados/services/vigia_fio.py:595-600`. |
| `FIO_SEM_PORTA` | Declaração em `app/chamados/juizes.py:1172`. | Folha em `app/folha/fora_da_folha.py:65-67`; seleção de pautas em `app/ponto/services/pautas_dos_discordantes.py:59`, `:76`. |
| Fio de `_fio_do_dia` | Leitura da porta em `app/folha/fora_da_folha.py:59-67`. | Grupos/contadores em `:148-166`; detalhe em `:191-198`; pautas em `app/ponto/services/pautas_dos_discordantes.py:76`. |
| Papel de `dono_fora_do_fio` | Permissão de Resolver dia, estado de ausência ou papel da lista em `app/chamados/services/vigia_fio.py:287-295`. | Alcance em `:608-612`; agregação/lista em `:583-587`; relatório em `:672-676`. O censo antigo chama sem papel em `:398`. |
| `ListaDoAdmin.dia_na_lista` | Mapa carregado por `lista` + `dias_na_lista` em `app/chamados/services/vigia_fio.py:214-215`, consultado em `:228-234`. | `papel_do_dia` em `:240`; causas do alcance em `:614`, `:630`, `:634`. |
| `ListaDoAdmin.papel_do_dia` | Dono lavrado formatado ou sentinela em `app/chamados/services/vigia_fio.py:240-243`. | Ramos escala/admin em `:293`, `:295`. |

**Há impacto indireto em outros leitores.** Grupo e fio alimentam `montar_linhas`, que grava `dono` no retrato; a lavra persiste esse resultado e a tela lê o retrato gravado. Portanto uma classificação diferente chega aos contadores, PDF e raio-X após nova lavra, sem exigir mudança nesses consumidores. Referências: `app/folha/retrato_folha.py:1156-1188`; `app/folha/management/commands/lavrar_retrato_folha.py:65-76`; `app/folha/views_retrato.py:196`, `:318`, `:331`, `:337`. A diferença efetiva de valores contra o pai é **[nao verificado]**.

3. **Chamado `registrado` com célula**

Sozinho, sem outra porta por pergunta, fica em **fio `nada`**. `registrado` pertence a `VIVOS`, mas não a `COBRANCA`; a fila o exclui, o juiz retorna `None` e a folha usa `FIO_SEM_PORTA`. Referências: `app/chamados/catalogo/motor.py:24-29`; `app/chamados/services/painel.py:187`; `app/chamados/juizes.py:1218-1219`; `app/folha/fora_da_folha.py:65-67`.

Ele continua acessível pelo drill `?status=registrado` e pelo modal que carrega todos os chamados do colaborador. O modal pode oferecer resposta conforme permissão; geofence registrado pode ser puxado para análise e decidido. Referências: `app/chamados/services/painel.py:304-305`; `app/chamados/services/fio.py:174-176`; `app/templates/chamados/partials/modal_fio.html:643-650`; `app/chamados/services/acoes_chamado.py:229-233`; `app/chamados/services/fio.py:527-540`.

**Sem achado contra a definição literal do corte nesse caso:** o juiz exclui expressamente acesso manual por modal/URL do conceito de porta adotado. Referência: `app/chamados/juizes.py:1203-1205`.

4. **`pergunta_sem_acao_possivel`: outra pergunta**

Ela responde: **“esta pergunta já foi materializada de fato, tornando o clique inútil?”** Exige `materializada_em`; lê o veredito lavrado e, sem ele, consulta `_pergunta_materializada_de_fato`. Não pergunta por fila, âncora ou alcance do dia. Referência: `app/chamados/services/raiox.py:151-185`.

O chamador direto localizado em produção é `montar_chamados_fio`: combina pergunta viva com ausência de clique morto para definir `tem_pergunta_viva`. A partição usa a versão do crédito em SQL, não chama diretamente essa função. Referências: `app/chamados/services/fio.py:275-281`; `app/chamados/services/raiox.py:133-149`; `app/chamados/services/verbos.py:31`.

**Sem achado de segundo juiz da alcançabilidade aqui.** Fazer essa função delegar à porta confundiria materialização da pergunta com existência de acesso ao dia, perguntas distintas nos contratos atuais. Referências: `app/chamados/services/raiox.py:152-162`; `app/chamados/juizes.py:1187-1202`.

5. **Fonte dos rótulos**

Tanto `escala_nao_descreve_o_dia` quanto `fio_do_admin`, quando solicitados com papel, consultam `ListaDoAdmin.papel_do_dia`. A classe carrega a mesma `cadastro_realidade.lista` e extrai os dias por `dias_na_lista`; o papel é `admin-<dono lavrado>`, ou informa ausência/falha da lista. Referências: `app/chamados/services/vigia_fio.py:214-215`, `:236-243`, `:293-295`.

O dono diário vem de `dono_do_dia` das assinaturas datadas, incluindo C9; sua lavra recebe o dono do oráculo e o persiste em snapshot. Referências: `app/escala/services/cadastro_realidade.py:595-604`, `:655-670`; `app/ponto/services/teto_na_lista.py:86`, `:110-116`.

**Há conjunto paralelo de donos**, conforme o achado do item 1: vigia e pautas repetem BATIDA/CADASTRO. Também permanece o texto agregado fixo de CADASTRO, embora o papel detalhado siga a lista. Referências: `app/chamados/services/vigia_fio.py:168`, `:830-832`; `app/ponto/services/pautas_dos_discordantes.py:41`.

6. **Mordida dos testes** [nao verificado]

Não executei a suíte Django, que depende de banco **[nao verificado]**. O vermelho efetivo no pai e a identificação exata de novos/alterados são **[nao verificado]**. Abaixo está a avaliação por leitura; ausência de símbolo no pai produziria erro de importação, não prova semântica da cura. [nao verificado]

| Teste | Mordida e contrafactual | [nao verificado]
|---|---|
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:108` | Exige nenhuma porta no cenário p#34375. Sem juiz no pai: falha mecânica; RED semântico isolado não demonstrado. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:112` | **Morde:** exige folha `nada`; ficaria vermelho contra folha que transforma pergunta viva respondida sob terminal em `admin`. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:117` | **Morde pela folha:** vigia deve dizer causa `c` e folha `nada`; igualdade não basta, há resultados explícitos. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:123` | **Morde:** mantém portas A/B, créditos corretos e `cobrar` vencendo no fio. Pai sem juiz: apenas erro mecânico. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:136` | Caracterização positiva de chamado sem pergunta; não demonstra necessidade da cura anterior. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:141` | Caracterização positiva de resposta→validar/admin; folha antiga que já desse admin passaria nessa parte. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:147` | **Morde:** registrado deve dar `nada`; vermelho contra folha que o aceita por estar em `VIVOS`. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:153` | Propriedade lote=avulso; detecta divergência entre alimentações, não o defeito antigo por si. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:165` | **Morde ordem da escada** e tratamento de `None`; ausência do helper no pai seria erro mecânico. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:175` | Pré-condição: registrado fora da fila. Deve continuar verde onde essa regra já existia. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:185` | Morde falta de registro; não testa alcançabilidade. |
| `app/folha/tests/test_haiku_fora_da_folha.py:93` | **Forte:** injeta `fio='xyz'` sob cobrar; tradução paralela devolveria colab. |
| `app/folha/tests/test_haiku_fora_da_folha.py:114` | Morde tabela literal de verbos na folha; não exclui toda derivação possível por outros mecanismos. |
| `app/folha/tests/test_haiku_fora_da_folha.py:160` | Morde retorno a `VIVOS`/`.viva`; parte da conferência de fontes é apenas presença textual. |

Para o arquivo de alcance, cujo subconjunto alterado não pode ser determinado sem o diff:

| Teste | O que morde; vermelho no pai | [nao verificado]
|---|---|
| `app/chamados/tests/test_o261e_f5_alcance.py:110` | Universo positivo, dia morto e partição. Pai **[nao verificado]**. |
| `app/chamados/tests/test_o261e_f5_alcance.py:119` | Porta da fila retira dia do morto. Pai **[nao verificado]**. |
| `app/chamados/tests/test_o261e_f5_alcance.py:126` | Pergunta no app com âncora na fila abre porta. Pai **[nao verificado]**. |
| `app/chamados/tests/test_o261e_f5_alcance.py:134` | Silêncio vigente vira causa `a`; não cobre silêncio vencido. Pai **[nao verificado]**. |
| `app/chamados/tests/test_o261e_f5_alcance.py:141` | Registrado sem porta vira causa `a`. Pai **[nao verificado]**. |
| `app/chamados/tests/test_o261e_f5_alcance.py:146` | Respondida vazia vira causa `b`. Pai **[nao verificado]**. |
| `app/chamados/tests/test_o261e_f5_alcance.py:154` | Respondida sob terminal vira causa `c`. Pai **[nao verificado]**. |
| `app/chamados/tests/test_o261e_f5_alcance.py:161` | Pendente sob terminal recebe detalhe próprio. Pai **[nao verificado]**. |
| `app/chamados/tests/test_o261e_f5_alcance.py:173` | Colaborador listado, dia ausente: causa `d`. Pai **[nao verificado]**. |
| `app/chamados/tests/test_o261e_f5_alcance.py:179` | BATIDA sai para admin; ESTRUTURA permanece morto. Pai **[nao verificado]**. |
| `app/chamados/tests/test_o261e_f5_alcance.py:190` | Âncora não basta sem pergunta no app. Pai **[nao verificado]**. |
| `app/chamados/tests/test_o261e_f5_alcance.py:198` | Falha da lista não apaga dia morto; não cobre folga validada. Pai **[nao verificado]**. |
| `app/chamados/tests/test_o261e_f5_alcance.py:204` | Resolver dia vira balde próprio. Pai **[nao verificado]*

[truncado em 20000 de 23682 caracteres]

## 31/10 00:19 · sessao 655f21619021 · - · - · origem=Ronald (tmux)

Achado confirmado: falha da lista no novo ramo `escala_nao_descreve_o_dia` não chega ao contador. Esse ramo retorna antes de `dia_do_admin` e usa `papel_do_dia` (`app/chamados/services/vigia_fio.py:293`); `dia_na_lista` devolve `None` na falha sem registrar o dia (`app/chamados/services/vigia_fio.py:231`), e o rótulo vira `lista sem resposta` (`app/chamados/services/vigia_fio.py:242`). O único registro em `_sem_resposta` fica no ramo pulado (`app/chamados/services/vigia_fio.py:224`). Caso: folga contestada validada, lista indisponível, único dia da rodada; imprime falta da lista, mas o command grava `fio_do_admin_sem_resposta=0` (`app/chamados/management/commands/vigiar_fio_morto.py:169`). Contraria o contrato de contar dias distintos que perguntaram e não receberam resposta (`app/chamados/services/vigia_fio.py:192`). Reproduzido somente em memória com a classe extraída por AST: `papel_do_dia(...) → 'lista sem resposta'; sem_resposta → 0`; a reprodução não escreveu arquivos [nao verificado por arquivo:linha].

O teste novo de rótulo cobre CADASTRO, BATIDA e lista vazia; falta cobrir lista indisponível (`app/chamados/tests/test_o261e_f5_alcance.py:327`). Os testes de falha existentes usam dia sem a folga validada e percorrem `dia_do_admin`, portanto não pegam a regressão (`app/chamados/tests/test_o261e_f5_alcance.py:223`, `app/chamados/tests/test_o261e_f5_alcance.py:248`, `app/chamados/tests/test_o261e_f5_alcance.py:266`).

Outro achado: o teste chamado “predicado existe UMA vez” só analisa o AST de `vigia_fio.py` (`app/chamados/tests/test_o261e_f5_alcance.py:239`). Ele passa com o mesmo conjunto declarado em `DONOS_DA_MESA_DO_ADMIN` e `DONOS_DE_PAUTA` (`app/chamados/services/vigia_fio.py:168`, `app/ponto/services/pautas_dos_discordantes.py:41`); ambos dizem aplicar a separação CADASTRO/BATIDA versus ESTRUTURA da L-099 (`app/chamados/services/vigia_fio.py:273`, `app/ponto/services/pautas_dos_discordantes.py:17`). O selo prova unicidade local; não a fonte única entre leitores.

Inventário dos testes específicos do juiz — são **11**, corrigindo minha contagem inicial. Resultado histórico no pai e lista exata dos alterados: [nao verificado], porque a cópia não contém histórico Git. Abaixo, a mordida verificável pela leitura e o contrafactual semântico; erro por símbolo inexistente não conta como RED semântico. [nao verificado]

| Arquivo:linha | Teste / mordida | [nao verificado]
|---|---|
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:108` | Ricardo, juiz retorna nenhuma porta. Se juiz não existe no pai, falha por importação; sozinho não demonstra RED semântico. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:112` | Ricardo, folha deve dizer `nada`. RED semântico contra folha que toma pergunta viva respondida por fio admin; essa divergência antiga é descrita no próprio caso (`:8`), mas pai executado [nao verificado]. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:117` | Mesmo caso, folha e vigia concordam. A metade do vigia já corresponde ao comportamento antigo descrito em `:10`; quem deve ficar vermelho é a folha (`:120`). |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:123` | A sem pergunta + B com pergunta: exige ambos os verbos reais, `decidir` e `cobrar`, e fio `colab`. Morde perda de uma porta e crédito recalculado para o chamado errado. No pai, eventual importação ausente deve ser separada dessa mordida. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:136` | Só chamado da fila: fio `chamado`. Caracterização positiva; não prova que a correção é necessária. Novo símbolo ausente daria falha mecânica. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:141` | Resposta sob âncora na fila: `validar`/`admin`. Caracterização positiva; uma folha antiga que já desse admin também passaria na parte comportamental. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:147` | `registrado` com célula: nenhuma porta e folha `nada`. Morde folha que usa VIVOS e classifica o registrado como chamado. Histórico concreto [nao verificado]. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:153` | Alimentação avulsa e lote iguais. Morde divergência entre as duas vias; é propriedade da implementação nova, não RED semântico do defeito anterior. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:165` | Escada no juiz: cobrar vence validar, validar vence decidir, outro/None dá chamado. Morde ordem errada; ausência de `fio_pelos_verbos` no pai daria só erro de importação. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:175` | Registrado fora de COBRANCA e da fila. É condição/caracterização; deve ficar verde numa versão anterior que já tenha essa definição (`app/chamados/catalogo/motor.py:29`). Pai [nao verificado]. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:185` | Registro em `core.JUIZES`. Morde falta de registro; não testa a regra de alcançabilidade. |

Testes da folha diretamente ligados à troca: [nao verificado]

| Arquivo:linha | Mordida / contrafactual |
|---|---|
| `app/folha/tests/test_haiku_fora_da_folha.py:93` | Forte: injeta fio `xyz` sob verbo cobrar; uma tradução paralela devolveria colab. Também testa None e número de consultas ao juiz (`:101`). Vermelho semântico contra leitor que ainda traduz verbo; pai concreto [nao verificado]. |
| `app/folha/tests/test_haiku_fora_da_folha.py:114` | AST proíbe literais cobrar/validar/decidir e atributo `_FIO_DO_VERBO`. Morde a tabela literal anterior descrita no contexto; não impede toda derivação possível por outros mecanismos. Pai concreto [nao verificado]. |
| `app/folha/tests/test_haiku_fora_da_folha.py:160` | Exige import do juiz e proíbe import VIVOS, atributo `.viva` e certos status textuais. Morde retorno ao predicado próprio antigo. `motivos_retencao_celula` e `classificar_export` são conferidos por presença textual, inclusive em comentários/docstrings (`:174`). Pai concreto [nao verificado]. |

Inventário completo do arquivo de alcance, para consolidar apenas o que efetivamente estiver no diff. Em **todos**, vermelho no pai permanece [nao verificado]; não há como distinguir novos de preexistentes nesta cópia.

| `app/chamados/tests/test_o261e_f5_alcance.py:linha` | O que efetivamente morde |
|---|---|
| `:110` | Universo positivo, dia sem porta conta um, partição e competência. |
| `:119` | Mesmo dia com chamado na fila sai do contador morto. |
| `:126` | Pergunta no app sob âncora na fila é porta. |
| `:134` | Silenciado não é porta e recebe causa a. |
| `:141` | Registrado não é porta e recebe causa a. |
| `:146` | Respondida com texto vazio não é porta, causa b. |
| `:154` | Respondida sob destino terminal recebe causa c. |
| `:161` | Pendente sob terminal recebe detalhe nomeado. |
| `:173` | Colaborador presente na lista sem o dia recebe causa d. |
| `:179` | BATIDA na lista sai para admin; ESTRUTURA permanece morto. |
| `:190` | Âncora na fila não basta quando pergunta fora do app. |
| `:198` | Falha da lista não apaga o dia morto; não contém folga validada. |
| `:204` | Resolver dia oferecido vira decisão pendente. |
| `:212` | Censo e alcance compartilham classificação e uma carga da lista. |
| `:223` | Mesma falha nos dois universos conta uma vez; não contém folga validada. |
| `:235` | Unicidade do predicado/carregador apenas dentro de vigia_fio; não morde cópia em pautas. |
| `:248` | Command lê a falha depois dos dois universos; não contém folga validada. |
| `:266` | Command compartilha mesma instância entre os dois censos. |
| `:287` | Papel alterna cobrar→validar quando a pergunta recebe resposta; consulta partição real. |
| `:301` | Forte: juiz injetado com verbo xyz deve produzir rótulo xyz; morde tabela própria do vigia. |
| `:310` | Rótulos sem porta, Resolver dia, hoje/futuro e donos CADASTRO/BATIDA. |
| `:327` | Rótulo escala segue dono da lista e acusa dia ausente; falta exceção da lista. |
| `:340` | A constante de permissão aparece em chamada textual do drawer. Morde mudança literal divergente; não testa a permissão em execução. |
| `:347` | Rótulo de ausência segue STATUS_CHOICES para dois estados. |
| `:357` | Limites 0/1/500 mantêm soma e omitidos; sem porta nunca some. |
| `:374` | Âncora registrada fora da fila também é véu. |
| `:381` | Dia de hoje sem porta é balde próprio e mantém soma. |
| `:387` | Porta da fila vence teto de hoje. |
| `:393` | Porta da pergunta vence teto de hoje. |
| `:399` | Validada não entra nas causas de pergunta viva. |
| `:408` | Dia presente sem dono não vira CADASTRO por presunção. |
| `:413` | Universo vazio marca cego. |
| `:418` | Caracteriza diferença de universo entre censo antigo e alcance. |
| `:424` | Competência entregue sai do universo por autoridade existente. |
| `:431` | Command carimba contador e esperado zero; explicitamente mantém alcance fora do alarme enquanto houver passivo (`:443`). |

Outros itens pedidos na minha subauditoria: não identifiquei teto oculto no contador novo de alcance; contador é incrementado antes do corte e omitidos são declarados (`app/chamados/services/vigia_fio.py:583`). A ordem porta antes de hoje e baldes está no código (`app/chamados/services/vigia_fio.py:595`, `app/chamados/services/vigia_fio.py:602`, `app/chamados/services/vigia_fio.py:608`) e tem testes nas linhas `:387` e `:393` acima. Pouso efetivo, suite verde e pré-requisito cumprido em execução: [nao verificado].

## 31/10 00:19 · sessao f43bd3a6ba89 · - · - · origem=Ronald (tmux)

Achados e censo, somente sobre a cópia atual; quais linhas pertencem exatamente ao diff e o comportamento no pai são [nao verificado], pois a cópia não contém `.git`.

1. **Falha da lista escapa ao contador no novo rótulo de escala.** `dono_fora_do_fio` retorna no ramo de folga validada, chamando apenas `papel_do_dia`, antes de chegar a `dia_do_admin` (`app/chamados/services/vigia_fio.py:292`, `:293`, `:294`). `papel_do_dia` recebe `None` de `dia_na_lista` e retorna `"lista sem resposta"` sem registrar o dia; quem adiciona a `_sem_resposta` é exclusivamente `dia_do_admin` (`app/chamados/services/vigia_fio.py:228`, `:231`, `:240`, `:242`, `:223`, `:224`).
   
   Caso concreto: único dia retido, sem porta, com FOLGA_CONTESTA validada; a lista lança exceção. O alcance escreve `escala_nao_descreve_o_dia|lista sem resposta`, mas `lista.sem_resposta` continua 0 e o comando carimba `fio_do_admin_sem_resposta=0` (`app/chamados/services/vigia_fio.py:608`, `:612`; `app/chamados/management/commands/vigiar_fio_morto.py:169`, `:216`). Isso contraria o contrato de contar dias que perguntaram e não tiveram resposta (`app/chamados/services/vigia_fio.py:190`, `:192`, `:606`).
   
   Verificação isolada sem escrita: compilei por AST a classe real, simulei lista já carregada sem resposta (`_carregada=True`, `_dias=None`); `papel_do_dia` retornou `"lista sem resposta"` com contador 0, enquanto `dia_do_admin` elevou o mesmo contador a 1. A cadeia do ramo de folga é estática nos endereços acima. O teste de rótulo não cobre falha; cobre apenas CADASTRO/BATIDA/fora da lista (`app/chamados/tests/test_o261e_f5_alcance.py:327`, `:332`, `:334`, `:337`).

2. **Conjunto L-099 declarado em paralelo, ainda sem divergência de conteúdo.** O vigia declara `DONOS_DA_MESA_DO_ADMIN=('BATIDA','CADASTRO')`; pautas declara `DONOS_DE_PAUTA=('CADASTRO','BATIDA')` (`app/chamados/services/vigia_fio.py:168`; `app/ponto/services/pautas_dos_discordantes.py:41`). Os consumidores são o predicado de retirada de fio morto e o alvo de emissão de pauta, respectivamente (`app/chamados/services/vigia_fio.py:226`; `app/ponto/services/pautas_dos_discordantes.py:136`). Ambos afirmam realizar a mesma separação L-099 CADASTRO/BATIDA para lista do admin (`app/chamados/services/vigia_fio.py:272`; `app/ponto/services/pautas_dos_discordantes.py:17`; `app/docs/LEIS.md:122`).
   
   O selo “predicado existe UMA vez” só lê AST de `vigia_fio`, logo não encontra a cópia de pautas; uma alteração futura em apenas um conjunto passa por esse selo (`app/chamados/tests/test_o261e_f5_alcance.py:238`, `:239`, `:240`, `:245`). É duplicação comprovada; divergência atual entre os conjuntos: sem achado pelos valores citados.

3. **Relógio fora da borda na fonte consumida.** O comando captura uma única borda temporal e passa `hoje` à lista (`app/chamados/management/commands/vigiar_fio_morto.py:158`, `:162`, `:166`; `app/chamados/services/vigia_fio.py:214`). Porém `cadastro_realidade.lista` chama `_jornada_que_mente(empresas)` sem essa data; o helper lê `date.today()` e fixa corte 21 (`app/escala/services/cadastro_realidade.py:518`, `:186`, `:187`).
   
   Caso: rodada com `hoje` injetado ou atravessando meia-noite pode montar C2 de outra janela. C2 altera presença do colaborador na lista, presença capturada em `_colabs`, que decide a causa `d` do alcance quando o dia está fora da lista (`app/escala/services/cadastro_realidade.py:215`; `app/chamados/services/vigia_fio.py:216`, `:248`, `:630`). Trata-se de problema preexistente na fonte; introdução nesses três commits [nao verificado].

4. **Outro leitor ainda publica CADASTRO fixo para escala.** O rótulo de papel do alcance segue a lista, inclusive BATIDA e ausência da lista (`app/chamados/services/vigia_fio.py:293`; `app/chamados/tests/test_o261e_f5_alcance.py:333`, `:335`, `:338`). Entretanto `linhas()` continua publicando “o dono é o CADASTRO, vai para a lista do admin” para todo o balde; o comando imprime esse texto (`app/chamados/services/vigia_fio.py:830`, `:831`; `app/chamados/management/commands/vigiar_fio_morto.py:173`). No cenário BATIDA ou fora da lista já existente no teste, coexistem o papel correto do alcance e a declaração fixa no relatório; avaliar como falha do leitor remanescente, pois o nome técnico do campo e o texto descritivo são distintos nos endereços citados.

Censo de fontes e leitores:

| Valor | Escritor/fonte | Leitores |
|---|---|---|
| Dono de cada dia da lista | `teto_na_lista.medir` recebe `_ins.dono(d)` e grava em `dono`; `lavrar` persiste o detalhe em snapshot (`app/ponto/services/teto_na_lista.py:86`, `:110`, `:113`, `:116`) | `_teto_do_dia` copia para `dono_do_dia`; a lista usa para o dono e a ação; `dias_na_lista` monta o mapa por dia (`app/escala/services/cadastro_realidade.py:310`, `:313`, `:598`, `:599`, `:670`) |
| `ListaDoAdmin._dias/_colabs` | `_carregar`: lista real e `dias_na_lista`, mais presença dos colaboradores (`app/chamados/services/vigia_fio.py:214`, `:215`, `:216`) | `dia_do_admin`, `dia_na_lista`, `colab_na_lista` (`app/chamados/services/vigia_fio.py:226`, `:234`, `:248`) |
| `dia_na_lista` | Consulta o mapa carregado, retorna presença/dono ou falha (`app/chamados/services/vigia_fio.py:228`, `:234`) | `papel_do_dia` e classificação das causas do alcance (`app/chamados/services/vigia_fio.py:240`, `:614`, `:630`, `:634`) |
| `papel_do_dia` | `"admin-"+dono.lower()` ou sentinelas fora da lista/sem resposta (`app/chamados/services/vigia_fio.py:240`, `:242`, `:243`) | Ramos `escala_nao_descreve_o_dia` e `fio_do_admin` de `dono_fora_do_fio` (`app/chamados/services/vigia_fio.py:293`, `:295`) |
| Papel de `dono_fora_do_fio` | Resolver dia: constante da permissão; ausência: `Ausencia.STATUS_CHOICES`; escala/admin: lista (`app/chamados/services/vigia_fio.py:287`, `:288`, `:289`, `:291`, `:293`, `:295`) | `censo` chama sem papel; alcance chama com papel e grava em lista e contador por rota/papel (`app/chamados/services/vigia_fio.py:398`, `:608`, `:612`, `:584`, `:587`) |
| Rótulos agregados do alcance | Contador por rota/papel (`app/chamados/services/vigia_fio.py:585`) | `linhas_alcance`, stdout do comando e detalhe do carimbo (`app/chamados/services/vigia_fio.py:672`, `:675`; `app/chamados/management/commands/vigiar_fio_morto.py:175`, `:218`) |

A tela e o PDF Cadastro x Realidade também leem a mesma `lista`, com empresas visíveis; não passam por `ListaDoAdmin` (`app/escala/views.py:487`, `:490`, `:492`, `:495`). Sua fonte compartilhada pode mudar o rótulo consumido pelo vigia sem mudar os leitores; quais desses leitores estão fora do diff [nao verificado].

Quanto a teto escondido: **sem achado no alcance**. Todo dia `sem_porta` entra na lista mesmo com `limite=0`; os demais cortes incrementam `alcance_dias_omitidos`; o contador por rota/papel sobe antes do corte (`app/chamados/services/vigia_fio.py:584`, `:585`, `:586`, `:589`). Os testes cobram soma do universo e omissão declarada (`app/chamados/tests/test_o261e_f5_alcance.py:364`, `:365`, `:366`).

## 31/10 00:19 · sessao 134634b540d5 · - · - · origem=Ronald (tmux)

| Campo | Escritores encontrados | Leitores relevantes |
|---|---|---|
| `ChamadoColaborador.status_local` | Transição: `app/chamados/models.py:185-193`; nascimento: `:699-704`, `:834-851`. | Fila: `app/chamados/services/painel.py:155`, `:186-188`; verbo/modal: `app/chamados/services/fio.py:385-399`; sinais/cartório: `app/chamados/signals.py:161-180`; API: `app/api/views.py:1047`, `:1145`; calendário: `app/colaboradores/services/calendario.py:557`; folha por nova alimentação: `app/chamados/juizes.py:1144-1148`. |
| `silenciado_ate` | `app/chamados/services/arquivo.py:166-171`, `:201-208`. | Fila: `app/chamados/services/painel.py:188`; partição: `app/chamados/services/verbos.py:42-48`; predicado: `app/chamados/models.py:304-307`; sinal: `app/chamados/signals.py:237-242`; contador: `app/chamados/services/fila.py:122-127`; pesquisa: `app/chamados/views_arquivar.py:156-157`. |
| `ChamadoColaborador.celula` | Nascimento: `app/chamados/models.py:716-722`, `:829-831`; ligação posterior: `app/chamados/services/vinculos.py:92-99`. | Dia do fato: `app/chamados/juizes.py:89-90`; alimentação da porta: `:1146-1148`; reconciliação: `app/chamados/models.py:866-868`; modal usa dia do juiz: `app/chamados/services/fio.py:365-374`. |
| `respondida_em`, `resposta_colab` | Resposta: `app/chamados/services/disputa_emissao.py:1247-1266`; automática: `:1392-1394`; reparo pela trilha: `:1407-1410`; reabertura: `:1358-1377`; ressuscitação zera carimbo: `app/chamados/models.py:1258-1259`; materialização também completa carimbo: `app/chamados/services/materializacao.py:511-529`, `:1190-1198`; fechamento automático: `app/chamados/services/disputa_emissao.py:1570-1575`, `:1655-1657`. | Juiz C6: `app/chamados/juizes.py:1020-1032`; filtro SQL: `app/chamados/models.py:2342-2355`; resposta vazia: `app/chamados/juizes.py:1126`; créditos: `app/chamados/services/raiox.py:133-149`; API: `app/api/views.py:1173-1175`, `:2632-2634`; modal: `app/templates/chamados/partials/modal_fio.html:210`; prévia folha: `app/folha/previa.py:142`; cartório: `app/ponto/services/cartorio.py:1263`; ficha: `app/colaboradores/services/ficha.py:171`. |
| `validada_em` | Assinatura/devolução: `app/chamados/services/validacao.py:127-133`, `:180-190`; nova resposta desfaz: `app/chamados/services/disputa_emissao.py:1250-1266`; reabertura: `:1358-1377`; ressuscitação: `app/chamados/models.py:1258`; materializadores também carimbam: `app/chamados/services/materializacao.py:514-529`, `:1194-1198`; fechamento automático: `app/chamados/services/disputa_emissao.py:1574-1575`. | C6 começa por ela: `app/chamados/juizes.py:1006-1007`; estado: `app/chamados/models.py:2204-2211`; SQL de vivas: `:2353`; ações: `app/chamados/services/acoes_disputa.py:341`; procedência: `app/ponto/services/procedencia_batida.py:74`; placar/ETag: `app/chamados/services/painel.py:66-67`. |
| `materializada_em` | Modelo: `app/chamados/models.py:2228`, `:2242`, `:2267`, `:2282`, `:2302`, `:2312`; ressuscitação `:1258`; reconciliação: `app/chamados/reconciliador.py:402`, `:617`; fio: `app/chamados/services/fio.py:846`; relavra: `app/chamados/services/relavra_via.py:231`; materializador: `app/chamados/services/materializacao.py:509`, `:858`, `:1043`, `:1149`, `:1339`; emissão: `app/chamados/services/disputa_emissao.py:1562`, `:1644`. | C6: `app/chamados/juizes.py:1015`, `:1044`, `:1050-1052`, `:1076-1083`; clique morto/crédito: `app/chamados/services/raiox.py:133-134`, `:173-185`; estado: `app/chamados/models.py:2206-2209`; procedência: `app/ponto/services/procedencia_batida.py:74`; validação: `app/chamados/services/validacao.py:182`; placar: `app/chamados/services/painel.py:67`. |
| `chamado_destino` | Nascimento/reuso: `app/chamados/models.py:2160-2182`; reapontamento: `app/chamados/services/fio.py:785-800`. | Âncora do fio: `app/chamados/services/fio.py:80-81`, `:117`; créditos: `app/chamados/services/raiox.py:135-147`; API: `app/api/views.py:1046`, `:2626`, `:2642-2644`; materialização: `app/chamados/services/materializacao.py:939`, `:1021`, `:1169-1172`; pergunta no app: `app/chamados/services/pergunta_no_app.py:116`, `:139-142`; validação: `app/chamados/services/validacao.py:788`; competência trancada: `app/chamados/services/competencia_trancada.py:115`. |

**Registrado com célula:** sozinho, sem outra porta por pergunta, fica em `nada`: `registrado` pertence a `VIVOS`, porém não a `COBRANCA` (`app/chamados/catalogo/motor.py:24-29`); `qs_na_fila` filtra `COBRANCA` (`app/chamados/services/painel.py:186-188`); alimentação só inclui esses chamados (`app/chamados/juizes.py:1144-1148`); porta vazia retorna `None` (`:1218-1219`); folha converte ausência de porta em `FIO_SEM_PORTA` (`app/folha/fora_da_folha.py:65-67`).

Há ação manual: o drill `?status=registrado` aceita status explícito (`app/chamados/services/painel.py:304-305`); modal admin carrega todos os chamados do colaborador (`app/chamados/services/fio.py:174-176`); caixa de resposta aparece para registrado, conforme permissão (`app/templates/chamados/partials/modal_fio.html:643-650`); geofence registrado pode ser puxado para análise e decidido (`app/chamados/services/acoes_chamado.py:229-233`, `app/chamados/services/fio.py:527-540`). O juiz exclui explicitamente acesso manual pelo modal de seu conceito de porta (`app/chamados/juizes.py:1203-1205`): portanto isso não prova, sozinho, descumprimento do corte.

**Achado candidato — porta da fila com verbo `historico`:** chamado aberto, célula acusadora e `silenciado_ate` passado entra em `qs_na_fila` (`app/chamados/services/painel.py:186-188`), mas a partição atribui `historico` (`app/chamados/services/verbos.py:45-48`), e a lista padrão o exclui (`app/chamados/services/painel.py:326-329`). A nova porta aceita esse chamado e devolve `fio='chamado'` (`app/chamados/juizes.py:1213`, `:1175-1181`, `:1220`). Assim o dia pode ser chamado de alcançável pela fila embora o painel padrão não mostre o cartão; acesso manual segue possível (`app/chamados/services/fio.py:174-176`). A inconsistência fila/partição já existe no código; sua introdução neste diff está [nao verificado].

**Leitores indiretos cujo resultado pode mudar sem alteração própria:** `_fio_do_dia` alimenta grupo, `dias_por_fio` e `por_fio` (`app/folha/fora_da_folha.py:148-167`); `montar_linhas` lê esse grupo e grava o fio em `dono` (`app/folha/retrato_folha.py:1156-1188`); command chama essa função e persiste o retrato (`app/folha/management/commands/lavrar_retrato_folha.py:65-76`); UI lê retrato gravado (`app/folha/views_retrato.py:196`), e contadores usam comprimento das linhas (`app/folha/retrato_folha.py:114-121`). Portanto o efeito chega após nova lavra também a barra, PDF e raio-X (`app/folha/views_retrato.py:318`, `:331`, `:337`).

**`o_que_segura['chamado']`:** entrega IDs do mapa de verbos da porta (`app/folha/fora_da_folha.py:193-198`), incluindo eventual âncora de pergunta em outro chamado, pois o juiz soma `chamado_que_lista(p)` à porta (`app/chamados/juizes.py:1214-1217`). O valor anterior ao diff está [nao verificado].

**Rótulo possivelmente inexato:** `DESTRAVA['chamado']` afirma “não há pergunta aberta” (`app/folha/fora_da_folha.py:43`), mas o juiz documenta pergunta viva sem crédito mantendo verbo `decidir`, portanto fio `chamado` (`app/chamados/juizes.py:1200-1202`, `:1175-1181`). Esse caso não permite concluir ausência de pergunta a partir do fio.

## 31/10 00:14 · sessao d4ba538432c4 · O261-E-F1-CLASSE-4 · censo · origem=Code

**O ponto central é que `cobranca_indevida` está em `VEREDITOS_ACUSAM`: esse veredito mantém a acusação para perguntas, reconciliação e folha. Ao mesmo tempo, integra `VEREDITOS_QUE_VETAM`, impedindo novas cobranças pelos emissores que consultam esse veto.** (`app/escala/models.py:1735`; `app/ponto/marcos.py:119`, `app/ponto/marcos.py:161`)

**1. Censo de escritores e derivação**

O caminho que produz e persiste o valor é:

| Função | O que faz |
|---|---|
| `cartorio._julgar_colab_corpo` | Extrai os códigos de `classificar_dia` e chama a lavra com `via='cartorio'`, `codigos=cods`, `save=False`; depois chama `carimbar` para persistir. (`app/ponto/services/cartorio.py:1148`, `app/ponto/services/cartorio.py:1455`, `app/ponto/services/cartorio.py:1470`) |
| `CelulaDia.lavrar_veredito` | Delega à porta de escrita. (`app/escala/models.py:1775`, `app/escala/models.py:1782`) |
| `ponto.portas.celula.lavrar_veredito` | Calcula `veredito or derivar_veredito(codigos or [])` e faz a atribuição material `cel.veredito = v`. (`app/ponto/portas/celula.py:678`, `app/ponto/portas/celula.py:692`) |
| `ponto.portas.celula.carimbar` / `salvar_e_emitir` | Inclui os campos lavrados no mesmo `save` do carimbo. (`app/ponto/portas/celula.py:726`, `app/ponto/portas/celula.py:728`, `app/ponto/portas/celula.py:640`) |

`derivar_veredito` associa os dois códigos — `COBRANCA_EM_DIA_SEM_TRABALHO` e `COBRANCA_SEM_FURO` — a `cobranca_indevida`. A precedência é pelo primeiro grupo correspondente: códigos de `furo` vencem esse grupo; `cobranca_indevida` vence os grupos posteriores. Sem códigos, retorna `concorde`. (`app/escala/models.py:1579`, `app/escala/models.py:1581`, `app/escala/models.py:1594`, `app/escala/models.py:1597`)

As outras chamadas de lavra não produzem esse valor: o ramo sem adesão fornece `chamados_vivos=0` e lavra `_vd_nb`; a decisão administrativa aceita somente `trabalhou`, `faltou` e `justificado`. (`app/ponto/services/cartorio.py:985`, `app/ponto/services/cartorio.py:1000`; `app/ponto/services/veredito_celula.py:18`, `app/ponto/services/veredito_celula.py:201`, `app/ponto/services/veredito_celula.py:330`)

As condições dos dois códigos são:

| Código | Condições efetivamente testadas |
|---|---|
| `COBRANCA_EM_DIA_SEM_TRABALHO` | Pessoa não isenta; `tipo_dia != 'trabalho'`; tipo fora de `('fora_vinculo', 'sem_celula')`; previsto zero/falso; `chamados_vivos` verdadeiro; `cobertura_status is None`. Não exige turno encerrado nem fio vivo. (`app/ponto/supra_juiz.py:46`, `app/ponto/supra_juiz.py:195`, `app/ponto/supra_juiz.py:441`) |
| `COBRANCA_SEM_FURO` | Pessoa não isenta; `tipo_dia == 'trabalho'`; previsto verdadeiro; realizado `>=` previsto; `chamados_vivos` verdadeiro; células não vazias e todas casadas. Não exige fio vivo nem turno encerrado nesse ramo. (`app/ponto/supra_juiz.py:195`, `app/ponto/supra_juiz.py:414`, `app/ponto/supra_juiz.py:418`) |

Para “todas casadas”, usa `dia['n_casadas']` quando informado; caso contrário, reconta células que sejam dicionários, não estejam `missing`, não sejam de isenção e tenham `hora`. (`app/ponto/supra_juiz.py:63`, `app/ponto/supra_juiz.py:415`)

A origem dos insumos no julgamento é:

| Insumo | Origem |
|---|---|
| `dia`, com tipo, minutos e células; `marcos` | Grade montada por `montar_grade_prevista_periodo`, alimentada com batidas apuráveis; o cartório indexa seus dias por data. No julgamento com teto, entrega `_dia_ate_o_teto` ao classificador. (`app/ponto/services/cartorio.py:890`, `app/ponto/services/cartorio.py:893`, `app/ponto/services/cartorio.py:896`, `app/ponto/services/cartorio.py:1125`) |
| `cobertura_status` | `ausencia_cobre(c, data, para='cobranca')`, seguido de `getattr(cob, 'status', None)`. (`app/ponto/services/cartorio.py:1060`, `app/ponto/services/cartorio.py:1151`) |
| `isento_ponto` | `bool(c.isento_ponto)`. (`app/ponto/services/cartorio.py:1153`) |
| `chamados_vivos` | Quantidade de chamados vivos do dia cujos marcos **não** tenham sido comprovados todos acesos na ata deste julgamento: `len(_protestam)`. (`app/ponto/services/cartorio.py:1132`, `app/ponto/services/cartorio.py:1147`, `app/ponto/services/cartorio.py:1149`) |
| `chamados_total` | `len(lista)`, incluindo os estados encerrados presentes na coleta do dia. (`app/ponto/services/cartorio.py:1059`, `app/ponto/services/cartorio.py:1149`) |
| `cobrancas_vivas` | `len(_com_fio)`: chamados vivos com alguma pergunta viva segundo `fio_vivo`. É separado de `chamados_vivos`. (`app/ponto/services/cartorio.py:1093`, `app/ponto/services/cartorio.py:1150`; `app/chamados/juizes.py:1111`) |

Especificamente, o universo de `chamados_vivos` é construído assim:

1. Consulta chamados do colaborador nos módulos `batida_ausente`, `turno_aberto`, `intervalo_pendente` e `entrada_bloqueada_turno_aberto`; essa consulta inicial não restringe status nem período. (`app/ponto/services/cartorio.py:85`; `app/chamados/catalogo/furo_modulos.py:15`)
2. Agrupa pelo resultado de `data_do_chamado`; nesses quatro módulos, a chave declarada é `data_turno`. Portanto, entram no julgamento da data indicada no contexto, independentemente de `criado_em`. (`app/ponto/services/cartorio.py:93`, `app/ponto/services/cartorio.py:1059`; `app/chamados/catalogo/modulos.py:105`, `app/chamados/catalogo/modulos.py:107`, `app/chamados/catalogo/modulos.py:111`, `app/chamados/catalogo/modulos.py:182`, `app/chamados/catalogo/modulos.py:225`)
3. Filtra estados `aberto`, `em_analise` e `registrado`. (`app/ponto/services/cartorio.py:1088`; `app/chamados/catalogo/motor.py:24`)
4. Exclui do insumo somente chamados para os quais `marcos_acesos(x, _ata_agora) is True`. Sem marcos nomeados, a resposta é `None`; portanto, continuam contando. (`app/ponto/services/cartorio.py:1147`; `app/ponto/services/lastro.py:170`)

**2. Censo de leitores**

Nos consumidores da tupla abaixo, `cobranca_indevida` participa como veredito acusador porque está expressamente nela. (`app/escala/models.py:1735`)

| Leitor | O que decide |
|---|---|
| `folha/export.py::motivos_retencao_celula` | Retém com motivo `espelho_cobranca_indevida`, salvo protestos integralmente não cobráveis ou isenção por pausa; esse ramo não exige `trabalha=True`. (`app/folha/export.py:1215`, `app/folha/export.py:1235`) |
| `folha/export.py::motivos_retencao_espelho` | Delega a decisão ao retentor da célula. (`app/folha/export.py:1248`) |
| `folha/export.py::classificar_export` | Havendo motivos de retenção, classifica como `fora`, motivo `furo_espelho`. (`app/folha/export.py:942`) |
| `folha/fora_da_folha.py::_dias_que_retem` | Obtém os dias pelo mesmo retentor, com `com_dias=True`. (`app/folha/fora_da_folha.py:93`) |
| `folha/fora_da_folha.py::fora_da_folha` | Agrupa os retidos segundo o fio desses dias. (`app/folha/fora_da_folha.py:139`, `app/folha/fora_da_folha.py:154`) |
| `folha/fora_da_folha.py::o_que_segura` | Expõe os dias retidos, veredito, fio e ação correspondente. (`app/folha/fora_da_folha.py:193`, `app/folha/fora_da_folha.py:209`) |
| `folha/previa.py::_cobre_a_retencao` | Considera a proposta suficiente somente quando há dias retidos e todos estão entre os propostos. (`app/folha/previa.py:169`) |
| `PerguntaDisputa.q_vivas` | Inclui célula acusadora no pré-filtro SQL de perguntas vivas, sujeito às condições de validação, identidade e tranca; o resultado exato continua sendo do C6. (`app/chamados/models.py:2556`, `app/chamados/models.py:2578`, `app/chamados/models.py:2581`) |
| `chamados/juizes.py::_pergunta_viva_calc` | Ao alcançar o degrau da célula, retorna `True` se seu veredito acusa. (`app/chamados/juizes.py:1054`) |
| `chamados/juizes.py::_resposta_concorda_com_a_luz` | Mesmo com concordância pela régua, só considera a resposta resolutiva se o veredito não acusa. (`app/chamados/juizes.py:1000`) |
| `chamados/reconciliador.py::retratar_perguntas_absolvidas_por_celula` | Exclui células acusadoras da seleção para carimbar `celula_absolveu`. (`app/chamados/reconciliador.py:598`, `app/chamados/reconciliador.py:619`) |
| `chamados/reconciliador.py::_absolvido_pela_celula` | Recusa considerar absolvido chamado cuja célula acusa. (`app/chamados/reconciliador.py:978`) |
| `chamados/services/disputa_emissao.py::motivo_fora_da_fabrica` | Pergunta já materializada só exclui o chamado por esse caminho se a célula estiver ausente ou não acusar. (`app/chamados/services/disputa_emissao.py:695`, `app/chamados/services/disputa_emissao.py:705`) |
| `chamados/services/fabrica_da_celula.py::motivos_a_perguntar` | Recusa veredito não acusador; antes disso já recusa folga, colaborador inativo e dia futuro. (`app/chamados/services/fabrica_da_celula.py:78`, `app/chamados/services/fabrica_da_celula.py:86`) |
| `chamados/services/limbo_folgas.py::filhos_no_defeito` | Célula absolvente faz o filho deixar de segurar o pai; célula acusadora ainda passa pelos testes de defeito e fio. (`app/chamados/services/limbo_folgas.py:194`, `app/chamados/services/limbo_folgas.py:199`) |
| `chamados/services/resposta_tardia.py::receber` | Tenta renascer chamado encerrado quando a célula acusa. (`app/chamados/services/resposta_tardia.py:45`) |
| `chamados/services/silencio_ausencia.py::_premissa_morre` | No caminho examinado, só considera a premissa morta por ausência se o veredito não acusa. (`app/chamados/services/silencio_ausencia.py:34`, `app/chamados/services/silencio_ausencia.py:41`) |
| `chamados/services/vigia_fio.py::censo` | Seleciona células acusadoras apontadas por chamados vivos para medir fio morto. (`app/chamados/services/vigia_fio.py:182`, `app/chamados/services/vigia_fio.py:228`) |
| `ponto/services/vigia_ausencia_veredito.py::censo` | Conta discordância entre ausência aprovada integral e célula acusadora, exceto `fato_em_ausencia`. (`app/ponto/services/vigia_ausencia_veredito.py:41`, `app/ponto/services/vigia_ausencia_veredito.py:51`) |
| `folha/services/vigia_pausa.py::_dias_isentos` | Conta células acusadoras dispensadas da retenção pela regra de pausa. (`app/folha/services/vigia_pausa.py:155`) |
| `relatorios/dia_da_frota.py::dia_da_frota` | Pula dias sem trabalho; nos demais inclui esse veredito em `acusam`, com rótulo “cobranca indevida”. (`app/relatorios/dia_da_frota.py:11`, `app/relatorios/dia_da_frota.py:47`, `app/relatorios/dia_da_frota.py:55`) |
| `relatorios/furos.py::estado_do_dia` | Converte o veredito em `parcial`, preservando os marcos furados encontrados pela grade. (`app/relatorios/furos.py:26`, `app/relatorios/furos.py:45`, `app/relatorios/furos.py:52`) |
| `colaboradores/services/calendario.py::_classificar` | Mapeia para `ok`; não aplica esse `ok` sem batida, salvo isento, nem substitui os estados `previsto`, `aberto`, `folga` e `folga_trabalhada`. (`app/colaboradores/services/calendario.py:72`, `app/colaboradores/services/calendario.py:171`) |
| `ponto/marcos.py::celula_veta_cobranca` | Retorna primeiro `FOLGA` quando `trabalha=False`; nos demais casos, esse veredito produz veto `COBRANCA_INDEVIDA`. (`app/ponto/marcos.py:119`, `app/ponto/marcos.py:155`) |
| `disparar_perguntas_competencia.Command.handle` | Permite célula acusadora e pula veredito não acusador, além dos demais filtros da emissão. (`app/chamados/management/commands/disparar_perguntas_competencia.py:86`, `app/chamados/management/commands/disparar_perguntas_competencia.py:120`) |
| `ausencias_sem_efeito_na_celula.classificar` | Conta ausência integral abonada com célula acusadora, exceto `fato_em_ausencia`, ou com lâmpada explicitamente apagada. (`app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:36`, `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:48`) |
| `auditar_invariantes_chamados._contraprova_f1t` | Alarma quando o piso F1t contém célula não acusadora. (`app/chamados/management/commands/auditar_invariantes_chamados.py:197`) |
| `auditar_invariantes_chamados.contraprova_piso` | Exclui célula acusadora, folga ou dia com movimento da denúncia de absolvição falsa. (`app/chamados/management/commands/auditar_invariantes_chamados.py:342`, `app/chamados/management/commands/auditar_invariantes_chamados.py:375`) |
| Mesmo auditor, `Command.handle`, A4 | Mantém candidato a encerramento com premissa viva quando o dia trabalha e acusa. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1387`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1433`) |
| Mesmo auditor, C2 | Denuncia pergunta sem materialização, validação ou resposta cuja célula tem veredito não acusador. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1597`) |
| Mesmo auditor, F1t | Separa como piso “cobrança com admin” perguntas mortas por via nomeada cuja célula ainda acusa. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1643`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1703`) |

**3. FOLGA, chamado somente com `data_turno`: ordem e persistência**

“Não respondida” não basta para garantir pergunta viva: validação, tranca, identidade materializada e um veredito anterior absolvente podem fazê-la morrer antes da tentativa de fechamento. A guarda também permite fechamento com reata válida a outro chamado. Portanto, o desfecho depende desses estados adicionais. (`app/chamados/juizes.py:1006`, `app/chamados/juizes.py:1039`, `app/chamados/juizes.py:1054`; `app/chamados/models.py:370`, `app/chamados/models.py:413`)

**Quando as perguntas permanecem vivas pelo C6, são da competência aberta e não há sobrevivente apto a recebê-las**, a sequência é:

1. **Entrada e eventual pulo.** `julgar_celula` aceita a folga de hoje sem esperar fim de turno, busca a célula e chama `julgar_colab`. O corpo ainda pode pular por impressão igual, salvo `forcar`. (`app/ponto/services/cartorio.py:1593`, `app/ponto/services/cartorio.py:1599`, `app/ponto/services/cartorio.py:1606`, `app/ponto/services/cartorio.py:1111`)

2. **Ata atual antes da classificação.** O corpo monta `_ata_agora` e a disponibiliza em `_tl.atas`. Sem `marcos_faltantes`, `marcos_acesos` retorna `None`; assim, o chamado permanece em `_protestam`. (`app/ponto/services/cartorio.py:1132`, `app/ponto/services/cartorio.py:1136`, `app/ponto/services/cartorio.py:1147`; `app/ponto/services/lastro.py:75`, `app/ponto/services/lastro.py:170`)

3. **Classificação.** Com tipo de folga, previsto zero e cobertura `None`, o classificador recebe esse chamado em `chamados_vivos` e emite `COBRANCA_EM_DIA_SEM_TRABALHO`. (`app/ponto/services/cartorio.py:1149`, `app/ponto/services/cartorio.py:1151`; `app/ponto/supra_juiz.py:441`)

4. **Sem adiamento por lâmpada.** Esse chamado não entra em `_adiados_pela_lampada` nem na reconciliação por marco, pois ambas exigem `marcos_acesos is True`; também não é “marco inexistente”, porque não nomeia marcos. (`app/ponto/services/cartorio.py:1204`, `app/ponto/services/cartorio.py:1208`, `app/ponto/services/cartorio.py:1219`; `app/ponto/services/lastro.py:110`)

5. **Tentativa de reconciliar antes da lavra.** O ramo compartilhado dos dois `COBRANCA_*` chama `reconciliar()`. Primeiro passa pela guarda da premissa: para `batida_ausente`, a célula sem trabalho regulariza a premissa. Depois consulta a guarda da pergunta viva. (`app/ponto/services/cartorio.py:1234`, `app/ponto/services/cartorio.py:1242`; `app/chamados/models.py:646`, `app/chamados/models.py:649`, `app/chamados/models.py:1225`)

6. **A guarda que recusa é `_gate_pergunta_viva`.** Ela busca perguntas não validadas ancoradas pelo `chamado_destino` ou, sem destino, pelo chamado da disputa; exige competência não entregue e pergunta viva pelo C6, simulando o destino terminal em memória. Procura outro chamado do mesmo colaborador-dia, excluindo o próprio, e exige costura válida. Sem receptor válido, retorna `False`, motivo `pergunta_viva_na_competencia_aberta`. (`app/chamados/models.py:327`, `app/chamados/models.py:339`, `app/chamados/models.py:345`, `app/chamados/models.py:349`, `app/chamados/models.py:371`, `app/chamados/models.py:403`)

7. **A recusa precede a lavra.** Nesse momento o C6 lê a célula relacionada à pergunta e seu veredito já gravado; a nova lavra só acontece depois, com os códigos calculados, derivando `cobranca_indevida` e persistindo o carimbo. (`app/chamados/models.py:329`; `app/chamados/juizes.py:1017`, `app/chamados/juizes.py:1054`; `app/ponto/services/cartorio.py:1455`, `app/ponto/services/cartorio.py:1470`; `app/escala/models.py:1581`)

8. **E2 e segunda passada não são automáticos nesse caso.** `julgar_colab` chama E2 nesse ponto somente se recebeu `_adiados_pela_lampada`; esse chamado não foi adiado. Mesmo que outro chamado faça E2 rodar, sua seleção exclui `cobranca_indevida`. A recusa também não enfileira `rejulgar`: nesse ramo, isso exige algum fechamento bem-sucedido. (`app/ponto/services/cartorio.py:745`, `app/ponto/services/cartorio.py:752`, `app/ponto/services/cartorio.py:1243`; `app/chamados/reconciliador.py:598`)

**A chamada termina, mas esse estado não converge apenas repetindo o julgamento.** Sem mudança dos insumos, a próxima chamada pode pular pela impressão; forçando o julgamento, o chamado continua contando, o mesmo código reaparece e o veredito continua acusador para o C6. E2 continua excluindo a célula. O mecanismo de `rejulgar` executa uma segunda passagem e descarta eventual sobra com contador; não mantém um laço indefinido dentro da chamada. (`app/ponto/services/cartorio.py:1111`, `app/ponto/services/cartorio.py:1147`, `app/ponto/services/cartorio.py:1242`, `app/ponto/services/cartorio.py:761`, `app/ponto/services/cartorio.py:770`; `app/chamados/juizes.py:1056`; `app/chamados/reconciliador.py:598`)

**4. Mesmo fluxo para `COBRANCA_SEM_FURO`**

**Com o mesmo contexto contendo somente `data_turno`, a dependência circular permanece.** Mesmo com todas as células casadas, a ausência de marcos nomeados faz `marcos_acesos` devolver `None`; o chamado continua em `_protestam`. Havendo trabalho, previsto verdadeiro e realizado `>=` previsto, nasce `COBRANCA_SEM_FURO`, que deriva o mesmo `cobranca_indevida`. (`app/ponto/services/lastro.py:170`; `app/ponto/services/cartorio.py:1147`; `app/ponto/supra_juiz.py:414`, `app/ponto/supra_juiz.py:418`; `app/escala/models.py:1581`)

Nesse dia de trabalho, a guarda da premissa consulta a **ata deste julgamento**: sem lâmpadas apagadas e sem órfãs que prendam o dia, `_celula_furada` retorna `False`, regularizando a premissa. Isso não dispensa a guarda da pergunta viva, que ainda pode recusar o fechamento nas condições descritas acima. (`app/chamados/juizes.py:373`, `app/chamados/juizes.py:433`, `app/chamados/juizes.py:436`, `app/chamados/juizes.py:438`, `app/chamados/juizes.py:446`; `app/chamados/models.py:1240`, `app/chamados/models.py:646`)

O caminho muda quando o chamado **nomeia marcos e todos estão acesos na ata atual**: ele sai de `_protestam`; também não sustenta `_vd='cobrado'`. Se não houver outro protesto, a primeira lavra resulta em `concorde`; com fio vivo, o chamado é adiado, E2 carimba a pergunta absolvida, a reconciliação fecha o chamado e a segunda passagem forçada grava a célula já sem ele. O contexto somente `data_turno` não percorre esse caminho, porque fornece `None`, não `True`. (`app/ponto/services/cartorio.py:1147`, `app/ponto/services/cartorio.py:1185`, `app/ponto/services/cartorio.py:1204`, `app/ponto/services/cartorio.py:745`, `app/ponto/services/cartorio.py:755`, `app/ponto/services/cartorio.py:765`; `app/escala/models.py:1595`; `app/ponto/services/lastro.py:170`)

## 31/10 00:12 · sessao 308ea00e1d2e · - · - · origem=Ronald (tmux)

A constante inclui explicitamente `'cobranca_indevida'` entre os vereditos acusadores; portanto os consumidores de `VEREDITOS_ACUSAM` tratam esse valor como acusação. (`app/escala/models.py:1735`)

| Leitor | Decisão |
|---|---|
| `folha/export.py::motivos_retencao_celula` | Produz `espelho_cobranca_indevida`, salvo protestos integralmente não cobráveis ou isenção por pausa. (`app/folha/export.py:1215`, `app/folha/export.py:1235`) |
| `folha/export.py::classificar_export` | Motivos do retentor colocam colaborador `fora`, por `furo_espelho`. (`app/folha/export.py:942`) |
| `folha/fora_da_folha.py::_dias_que_retem` | Delega ao retentor com `com_dias=True`. (`app/folha/fora_da_folha.py:93`) |
| `folha/fora_da_folha.py::fora_da_folha` | Usa dias retidos para agrupar colaboradores pelo fio que falta. (`app/folha/fora_da_folha.py:139`, `app/folha/fora_da_folha.py:154`) |
| `folha/fora_da_folha.py::o_que_segura` | Expõe dias retidos, veredito, fio e ação correspondente. (`app/folha/fora_da_folha.py:193`, `app/folha/fora_da_folha.py:209`) |
| `folha/previa.py::_cobre_a_retencao` | A proposta cobre a retenção somente se os dias retidos forem não vazios e estiverem todos nos propostos. (`app/folha/previa.py:169`) |
| `PerguntaDisputa.q_vivas` | Inclui célula acusadora na condição SQL de pergunta viva, respeitando validação, tranca e identidade. (`app/chamados/models.py:2578`, `app/chamados/models.py:2581`) |
| `chamados/juizes.py::_pergunta_viva_calc` | Retorna `True` ao chegar ao degrau de célula com veredito acusador. (`app/chamados/juizes.py:1054`) |
| `chamados/juizes.py::_resposta_concorda_com_a_luz` | Mesmo havendo concordância pela régua, só retorna concordância resolutiva se o dia não acusa. (`app/chamados/juizes.py:1000`) |
| `chamados/reconciliador.py::retratar_perguntas_absolvidas_por_celula` | Exclui perguntas de células acusadoras da seleção para carimbar `celula_absolveu`. (`app/chamados/reconciliador.py:598`, `app/chamados/reconciliador.py:619`) |
| `chamados/reconciliador.py::_absolvido_pela_celula` | Não considera absolvido chamado cujo dia tem veredito acusador; também exige ausência de marcos pendentes. (`app/chamados/reconciliador.py:978`) |
| `chamados/services/disputa_emissao.py::motivo_fora_da_fabrica` | Pergunta anteriormente materializada só exclui o chamado da fábrica se a célula não acusa ou está ausente. (`app/chamados/services/disputa_emissao.py:695`, `app/chamados/services/disputa_emissao.py:705`) |
| `chamados/services/limbo_folgas.py::filhos_no_defeito` | Filho com célula absolvente não segura o pai; célula acusadora ainda passa pelos testes de defeito e fio vivo. (`app/chamados/services/limbo_folgas.py:194`, `app/chamados/services/limbo_folgas.py:199`) |
| `chamados/services/resposta_tardia.py::receber` | Tenta renascer chamado encerrado se a célula acusa; conseguindo, devolve `reaberto`. (`app/chamados/services/resposta_tardia.py:45`) |
| `chamados/services/silencio_ausencia.py::_premissa_morre` | Para os módulos examinados, a cobrança só morre por ausência se o veredito não acusa. (`app/chamados/services/silencio_ausencia.py:34`, `app/chamados/services/silencio_ausencia.py:41`) |
| `chamados/services/fabrica_da_celula.py::motivos_a_perguntar` | Recusa veredito não acusador; antes disso já recusa folga, colaborador inativo e dia futuro. (`app/chamados/services/fabrica_da_celula.py:78`, `app/chamados/services/fabrica_da_celula.py:86`) |
| `chamados/services/vigia_fio.py::censo` | Seleciona células acusadoras apontadas por chamados vivos para medir fio morto. (`app/chamados/services/vigia_fio.py:182`, `app/chamados/services/vigia_fio.py:228`) |
| `ponto/services/vigia_ausencia_veredito.py::censo` | Conta discordância de célula acusadora, exceto `fato_em_ausencia`, com ausência aprovada integral. (`app/ponto/services/vigia_ausencia_veredito.py:41`, `app/ponto/services/vigia_ausencia_veredito.py:51`) |
| `folha/services/vigia_pausa.py::_dias_isentos` | Conta células acusadoras isentas pela regra de pausa. (`app/folha/services/vigia_pausa.py:155`) |
| `relatorios/dia_da_frota.py::dia_da_frota` | Pula dias sem trabalho; nos demais inclui o valor em `acusam`, rotulando “cobranca indevida”. (`app/relatorios/dia_da_frota.py:11`, `app/relatorios/dia_da_frota.py:47`, `app/relatorios/dia_da_frota.py:55`) |
| `relatorios/furos.py::estado_do_dia` | Traduz o valor para `parcial`, preservando os índices furados. (`app/relatorios/furos.py:26`, `app/relatorios/furos.py:45`) |
| `colaboradores/services/calendario.py::_classificar` | Mapeia o valor para `ok`; não aplica esse `ok` sem batida, salvo isento, nem substitui `previsto`, `aberto`, `folga` ou `folga_trabalhada`. (`app/colaboradores/services/calendario.py:72`, `app/colaboradores/services/calendario.py:171`) |
| `ponto/marcos.py::celula_veta_cobranca` | Veta por `FOLGA` quando `trabalha=False`; nos demais casos esse veredito retorna `COBRANCA_INDEVIDA` como motivo de veto. (`app/ponto/marcos.py:119`, `app/ponto/marcos.py:155`) |
| `disparar_perguntas_competencia.Command.handle` | Permite célula acusadora e pula veredito não acusador, além dos filtros de trabalho, atividade e motivos vencidos. (`app/chamados/management/commands/disparar_perguntas_competencia.py:76`, `app/chamados/management/commands/disparar_perguntas_competencia.py:120`) |
| `ausencias_sem_efeito_na_celula.classificar` | Conta ausência integral abonada cuja célula acusa, exceto `fato_em_ausencia`, ou cuja ata tem lâmpada explicitamente apagada. (`app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:36`, `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:48`, `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:62`) |
| `auditar_invariantes_chamados._contraprova_f1t` | Alarma quando o piso F1t contém célula que não acusa. (`app/chamados/management/commands/auditar_invariantes_chamados.py:197`) |
| `auditar_invariantes_chamados.contraprova_piso` | Exclui célula acusadora, folga ou dia com movimento da denúncia de absolvição falsa. (`app/chamados/management/commands/auditar_invariantes_chamados.py:342`, `app/chamados/management/commands/auditar_invariantes_chamados.py:375`) |
| `auditar_invariantes_chamados.Command.handle`, A4 | Só mantém candidato à denúncia de encerrado com premissa viva quando o dia trabalha e acusa. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1387`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1433`) |
| Mesmo auditor, C2 | Denuncia pergunta sem materialização, validação ou resposta cuja célula tem veredito não acusador. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1597`) |
| Mesmo auditor, F1t | Classifica como piso “cobrança com admin” perguntas mortas por via nomeada com célula ainda acusadora. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1703`) |

Em `ponto/services/espelho.py`, `_status_dia` decide por ausência, inconsistência, turnos/batidas, isenção, folga e previsto; esse corpo não lê o veredito da célula. (`app/ponto/services/espelho.py:152`) `aplicar_palavra_do_dia` delega para `dia_decidido.do_dia`, usando fatos, decisão, cobertura e pendências. (`app/ponto/services/espelho.py:339`, `app/ponto/services/espelho.py:351`)

## 31/10 00:12 · sessao 2667dfc67a82 · - · - · origem=Ronald (tmux)

Censo complementar concluído:

- Chamadas diretas de produção ao método: `_julgar_colab_corpo`, nas linhas `1000` e `1455`, e `veredito_celula`, na linha `330`; o método do modelo delega para a porta. A atribuição material aparece em `ponto.portas.celula.lavrar_veredito`. (`app/ponto/services/cartorio.py:1000`, `:1455`; `app/ponto/services/veredito_celula.py:330`; `app/escala/models.py:1782`; `app/ponto/portas/celula.py:692`).
- Só a chamada principal do cartório fornece códigos que podem gerar `cobranca_indevida`; a chamada do ramo sem adesão fornece `chamados_vivos=0`, e o admin aceita somente `trabalhou/faltou/justificado`. (`app/ponto/services/cartorio.py:985`, `:1149`, `:1455`; `app/ponto/services/veredito_celula.py:18`, `:201`; `app/ponto/supra_juiz.py:419`, `:442`).
- `celula_veredito_velho.Command.handle` é uma entrada de rejulgamento, sem escritor/derivador alternativo: `_espia_lavra` chama a porta original; o comando chama `julgar_colab(...apply_=True,forcar=True)`, mantendo ou revertendo a transação conforme `--aplicar`. (`app/ponto/management/commands/celula_veredito_velho.py:121`, `:122`, `:144`, `:145`, `:156`, `:157`, `:161`).
- `supra_juiz.Command.handle` identifica os dois códigos, mas sua ação explícita é `_retr(_ch, conteudo=motivo_retratacao(...))`, dirigida ao chamado; esse bloco não lavra `CelulaDia.veredito`. (`app/ponto/management/commands/supra_juiz.py:218`, `:243`, `:247`, `:248`).
- O selo estrutural existente percorre o app excluindo testes/migrações e verifica escritas por `update`, `bulk_update`, raw/SQL e saves fora da porta; sua asserção exige lista vazia. Não executei esse selo. (`app/ponto/tests/test_selo_veredito_so_pela_porta.py:1`, `:3`, `:98`, `:155`, `:156`, `:164`, `:177`).

Na busca estática realizada em Python de produção de `app/`, não apareceu atribuição alternativa ao campo; a própria declaração do modelo nomeia `lavrar_veredito()` como escritor único. (`app/escala/models.py:1717`; `app/ponto/portas/celula.py:692`).

## 31/10 00:12 · sessao 72af1ce290cc · - · - · origem=Ronald (tmux)

Para o cenário que efetivamente contém perguntas **vivas pelo C6**, em competência aberta e sem outro chamado apto a recebê-las, o ciclo **não converge por repetição do julgamento**: `cobranca_indevida` pertence a `VEREDITOS_ACUSAM`, portanto sustenta a vitalidade da pergunta e fica excluído de E2 (`app/escala/models.py:1735`; `app/chamados/juizes.py:1054-1057`; `app/chamados/reconciliador.py:595-599`).

Ordem observada para FOLGA, contexto somente `data_turno`:

1. `julgar_celula` permite a FOLGA de hoje sem esperar fim de turno e chama `julgar_colab`; isso pressupõe data não futura e célula existente (`app/ponto/services/cartorio.py:1591-1607`).
2. Primeira passagem recebe `adiar_pela_lampada=True` (`app/ponto/services/cartorio.py:744`).
3. O corpo lê chamados vivos e fio vivo, calcula a impressão e pode pular julgamento se impressão igual, veredito presente e sem `forcar` (`app/ponto/services/cartorio.py:1088-1113`).
4. Monta a ata deste julgamento e a disponibiliza em `_tl.atas` **antes** de classificar e tentar reconciliar (`app/ponto/services/cartorio.py:1132-1136`).
5. Sem `marcos_faltantes`, `_marcos_cobrados` devolve lista vazia e `marcos_acesos` devolve `None`; consequentemente o chamado fica em `_protestam`, porque a condição exclui apenas `is True` (`app/ponto/services/lastro.py:74-79`, `app/ponto/services/lastro.py:170-172`; `app/ponto/services/cartorio.py:1147`).
6. `classificar_dia` recebe `chamados_vivos=len(_protestam)` e, se tipo não trabalho e não sem jurisdição, previsto zero e cobertura `None`, emite `COBRANCA_EM_DIA_SEM_TRABALHO` (`app/ponto/services/cartorio.py:1148-1158`; `app/ponto/supra_juiz.py:441-445`).
7. O chamado sem marcos **não** entra em `_adiados_pela_lampada`, nem na reconciliação por marcos acesos, porque ambas exigem `marcos_acesos is True`; também não entra em `marcos_inexistentes`, que recusa contexto sem marcos (`app/ponto/services/cartorio.py:1204-1208`, `app/ponto/services/cartorio.py:1219-1221`; `app/ponto/services/lastro.py:110-114`).
8. O ramo `COBRANCA_*` tenta `reconciliar()` **antes de lavrar** o veredito (`app/ponto/services/cartorio.py:1234-1245`, `app/ponto/services/cartorio.py:1452-1471`).
9. `reconciliar` pede `_decisao_do_fechamento`; primeiro roda `_gate_premissa_do_fechamento`, depois `_gate_pergunta_viva` (`app/chamados/models.py:605-619`, `app/chamados/models.py:645-653`).
10. Para `batida_ausente` em FOLGA, a premissa deixa fechar: célula do dia sem trabalho e módulo que não começa com `turno_aberto` resulta `True` (`app/chamados/models.py:1220-1226`; `app/chamados/juizes.py:386-389`).
11. Quem recusa, nas condições do primeiro parágrafo, é **`_gate_pergunta_viva`**, motivo `pergunta_viva_na_competencia_aberta` (`app/chamados/models.py:391-412`). Ela lê perguntas não validadas ancoradas pelo `chamado_destino` ou, sem destino, pelo chamado da disputa; consulta se competência foi entregue e o C6, simulando o estado terminal em memória; tenta achar sobrevivente do mesmo colaborador-dia excluindo o próprio chamado e validar a costura (`app/chamados/models.py:327-350`, `app/chamados/models.py:355-375`). Sem sobrevivente válido, retorna plano de recusa e o fechamento não ocorre (`app/chamados/models.py:373-374`, `app/chamados/models.py:403-412`).
12. Depois da recusa, a lavra recebe `codigos=cods`, deriva `cobranca_indevida` pelo mapa e carimba célula/impressão (`app/ponto/services/cartorio.py:1452-1471`; `app/escala/models.py:1581`, `app/escala/models.py:1592-1599`).
13. Voltando a `julgar_colab`, E2 só é chamado se houver `_adiados_pela_lampada`; esse chamado sem marcos não foi adiado. Mesmo que outro chamado faça E2 rodar, o filtro exclui células com qualquer `VEREDITOS_ACUSAM`, incluindo `cobranca_indevida` (`app/ponto/services/cartorio.py:745-755`; `app/chamados/reconciliador.py:595-599`; `app/escala/models.py:1735`).
14. A recusa não coloca a célula em `rejulgar`: nesse ramo, o enfileiramento depende de algum `reconciliar()` devolver `True`. Segunda passagem só roda se `rejulgar` estiver preenchido; sobra da segunda é descartada com contador, sem terceira passagem por esse mecanismo (`app/ponto/services/cartorio.py:1242-1245`, `app/ponto/services/cartorio.py:761-772`).
15. Próxima chamada com mesmos insumos pode pular pela impressão; com `forcar`, repete classificação, recusa e a mesma lavra. Nenhum passo desse circuito remove a pergunta viva, pois o novo veredito continua acusando (`app/ponto/services/cartorio.py:1111-1113`, `app/ponto/services/cartorio.py:1147-1149`, `app/ponto/services/cartorio.py:1242-1245`; `app/chamados/juizes.py:1054-1057`).

**Ressalva indispensável:** “não respondida” não basta para garantir recusa. Pergunta validada, competência trancada e certas vias de identidade já podem produzir `False`; veredito anterior definitivo fora de `VEREDITOS_ACUSAM` também pode matá-la antes da tentativa de reconciliar (`app/chamados/juizes.py:1006-1016`, `app/chamados/juizes.py:1039-1045`, `app/chamados/juizes.py:1054-1065`). O gate também ignora competência entregue/indecidível e permite reata válida; logo o desfecho absoluto depende desses dados não especificados (`app/chamados/models.py:345-346`, `app/chamados/models.py:370-375`, `app/chamados/models.py:413-420`).

Para **COBRANCA_SEM_FURO com o mesmo contexto sem marcos**, o circuito é equivalente: exige trabalho, previsto positivo, realizado `>=` previsto, células não vazias e todas casadas, além de `chamados_vivos` (`app/ponto/supra_juiz.py:414-422`). Sem marcos nomeados o chamado continua em `_protestam` e não entra nos adiados; o ramo compartilhado tenta reconciliar antes da lavra e pode sofrer a mesma recusa F1; depois grava o mesmo `cobranca_indevida`, que E2 exclui (`app/ponto/services/lastro.py:170-172`; `app/ponto/services/cartorio.py:1147-1149`, `app/ponto/services/cartorio.py:1204-1208`, `app/ponto/services/cartorio.py:1234-1245`, `app/ponto/services/cartorio.py:1455`; `app/escala/models.py:1581`, `app/chamados/reconciliador.py:598`).

Nesse caso de trabalho, a premissa lê a ata deste julgamento; sem lâmpada apagada e sem órfãs que a prendam, `_celula_furada` retorna `False`, liberando a guarda da premissa, mas sem eliminar a guarda da pergunta viva (`app/chamados/juizes.py:373-383`, `app/chamados/juizes.py:433-446`; `app/chamados/models.py:1240-1242`, `app/chamados/models.py:646-653`).

Há um caminho distinto quando o chamado **nomeia marcos e todos estão acesos**: ele sai de `_protestam`; se tem fio vivo vai a `_adiados_pela_lampada`; a primeira passagem pode lavrar `concorde`, depois E2 carimba, então o chamado reconcilia e a célula segue à segunda passagem forçada (`app/ponto/services/cartorio.py:1147-1149`, `app/ponto/services/cartorio.py:1185-1186`, `app/ponto/services/cartorio.py:1204-1207`, `app/ponto/services/cartorio.py:745-765`; `app/escala/models.py:1594-1596`). Esse mecanismo exige marcos comprovadamente acesos; o contexto somente `data_turno` devolve `None` e não o percorre (`app/ponto/services/lastro.py:170-181`).

## 31/10 00:12 · sessao 2667dfc67a82 · - · - · origem=Ronald (tmux)

Q1 — referências e conclusões:

- **Escritor material único:** `ponto.portas.celula.lavrar_veredito` atribui `cel.veredito = v` e os metadados; com `save=True`, chama `salvar_e_emitir`, que salva os campos. O método `CelulaDia.lavrar_veredito` apenas delega. (`app/ponto/portas/celula.py:692`, `:693`, `:694`, `:696`, `:697`, `:634`, `:640`; `app/escala/models.py:1775`, `:1781`, `:1782`).
- **Orquestrador que pode produzir `cobranca_indevida`:** `_julgar_colab_corpo` calcula `cods`, deixa `_vd=None` quando há código e chama `cel.lavrar_veredito(via='cartorio', veredito=_vd, codigos=cods, save=False)`; `carimbar` persiste junto os campos devolvidos. (`app/ponto/services/cartorio.py:1148`, `:1159`, `:1161`, `:1162`, `:1455`, `:1470`; `app/ponto/portas/celula.py:726`, `:728`).
- **Entrada pública:** `julgar_celula` busca uma célula e chama `julgar_colab`; este chama `_julgar_colab_corpo`. (`app/ponto/services/cartorio.py:1593`, `:1606`, `:744`).
- **Admin não grava esse valor pela porta:** admin aceita somente `('trabalhou','faltou','justificado')`; a porta valida esse vocabulário. (`app/escala/models.py:1727`; `app/ponto/portas/celula.py:683`).
- **Ramo nunca bateu não emite os dois códigos:** chama `classificar_dia` com `chamados_vivos=0`, e ambos os códigos exigem valor verdadeiro nesse insumo; usa `_vd_nb` ou renomeia como `nunca_bateu`. (`app/ponto/services/cartorio.py:984`, `:985`, `:989`, `:999`, `:1000`; `app/ponto/supra_juiz.py:419`, `:442`).

**Derivação:**

- `classificar_dia` devolve tuplas `(codigo, detalhe)`; o cartório extrai somente os códigos. A porta usa `v = veredito or derivar_veredito(codigos or [])`. (`app/ponto/supra_juiz.py:185`; `app/ponto/services/cartorio.py:1148`; `app/ponto/portas/celula.py:678`).
- `derivar_veredito` transforma códigos em conjunto e retorna o primeiro grupo correspondente: `furo` precede `cobranca_indevida`, que precede `coberta`, `indefinida`, `discordante`, etc.; ambos `COBRANCA_EM_DIA_SEM_TRABALHO` e `COBRANCA_SEM_FURO` estão no grupo `cobranca_indevida`. Portanto, se também houver algum código do primeiro grupo, o resultado será `furo`. (`app/escala/models.py:1579`, `:1580`, `:1581`, `:1594`, `:1597`, `:1598`, `:1599`).
- Códigos vazios resultam em `concorde`; código sem correspondência resulta em `ValueError`. (`app/escala/models.py:1595`, `:1596`, `:1600`).

**Insumos efetivamente necessários:**

- A guarda global `isento_ponto` retorna lista vazia antes dos ramos. (`app/ponto/supra_juiz.py:195`).
- `COBRANCA_EM_DIA_SEM_TRABALHO`: `tipo_dia != 'trabalho'`, tipo fora de `SEM_JURISDICAO=('fora_vinculo','sem_celula')`, `minutos_previstos` falso/zero, `chamados_vivos` verdadeiro, `cobertura_status is None`; não exige encerramento do turno, quantidade realizada, fio vivo ou contagem de marcos. (`app/ponto/supra_juiz.py:46`, `:197`, `:198`, `:441`, `:442`, `:443`).
- `COBRANCA_SEM_FURO`: `tipo_dia=='trabalho'`, previsto verdadeiro, realizado `>=` previsto, `chamados_vivos` verdadeiro e `todas_casadas`; `todas_casadas` exige lista `celulas` não vazia e `n_casadas == len(celulas)`. Usa `dia['n_casadas']` se diferente de `None`, senão reconta `_celulas_com_hora`, que exige dict, não missing, não isenção e hora verdadeira. Não exige fio vivo ou turno encerrado. (`app/ponto/supra_juiz.py:58`, `:63`, `:64`, `:65`, `:197`, `:198`, `:199`, `:414`, `:415`, `:416`, `:417`, `:418`, `:419`, `:420`).

**Origem no cartório:**

- `dia`/`marcos`: batidas apuráveis da janela `[d0-1, d1+3)` alimentam `eu.montar_grade_prevista_periodo`; `marcos=g['marcos']`; cada dia é indexado por `ref-i`; no julgamento busca `dias_por_data[data]`. (`app/ponto/services/cartorio.py:849`, `:874`, `:875`, `:890`, `:891`, `:892`, `:893`, `:894`, `:896`, `:897`, `:1114`).
- `tipo_dia`, previsto, realizado e `celulas` são as chaves do dicionário do dia que `classificar_dia` lê. No modo por turno, a grade monta folga com previsto 0 e células `None`, ou trabalho com células pareadas e previsto do mapa; realizado vem de `realizado_do_dia`, sendo zero explícito quando `sem_turno`. (`app/ponto/supra_juiz.py:197`, `:198`, `:199`, `:414`; `app/escala/utils.py:1743`, `:1744`, `:1749`, `:1750`, `:1751`, `:1985`, `:1998`).
- O dispatcher usa builder por turno se `settings.GRADE_POR_TURNO` for verdadeiro, senão legacy; não assumir em relatório que a configuração em execução é verdadeira sem verificar. (`app/escala/utils.py:2024`, `:2026`, `:2027`, `:2029`).
- `cobertura_status`: `ausencia_cobre(c,data,para='cobranca')`, depois `getattr(cob,'status',None)`. (`app/ponto/services/cartorio.py:1060`, `:1151`).
- `isento_ponto`: `bool(c.isento_ponto)`. (`app/ponto/services/cartorio.py:1153`).
- No teto de hoje, o classificador recebe `_dia_ate_o_teto(...)`; fora dele recebe o dia original. (`app/ponto/services/cartorio.py:1125`, `:1149`).

**`chamados_vivos` — detalhe fundamental deste HASH:**

1. `chamados_que_falam_de_batida(c)` consulta **todos os chamados do colaborador** cujo módulo está em `FURO_MODULES`, com prefetch das perguntas; a query não restringe status ou intervalo de datas. (`app/ponto/services/cartorio.py:80`, `:85`, `:86`, `:1023`).
2. `FURO_MODULES = ('batida_ausente','turno_aberto','intervalo_pendente','entrada_bloqueada_turno_aberto')`. (`app/chamados/catalogo/furo_modulos.py:15`, `:16`).
3. Agrupa pelo dia retornado por `data_do_chamado(modulo, contexto_json)`; só inclui se esse dia for verdadeiro; para os quatro módulos a chave declarada é `data_turno`, lida diretamente do contexto. Portanto o chamado entra no julgamento do **mesmo dia de `contexto_json.data_turno`**, inclusive se a batida for noturna; não usa `criado_em` nem dia civil da batida. (`app/ponto/services/cartorio.py:93`, `:94`, `:95`, `:96`, `:1059`; `app/chamados/catalogo/modulos.py:105`, `:107`, `:111`, `:182`, `:219`, `:222`, `:225`).
4. Para a célula, `lista=chs.get(data.isoformat(),[])`; `vivos` filtra status em `VIVOS=('aberto','em_analise','registrado')`. (`app/ponto/services/cartorio.py:1059`, `:1088`; `app/chamados/catalogo/motor.py:24`).
5. O HASH **não passa simplesmente `len(vivos)`**: monta a ata deste julgamento, e `_protestam = [x for x in vivos if marcos_acesos(x,_ata_agora) is not True]`; passa `chamados_vivos=len(_protestam)`, `chamados_total=len(lista)` e, separadamente, `cobrancas_vivas=len(_com_fio)`. (`app/ponto/services/cartorio.py:1132`, `:1133`, `:1147`, `:1149`, `:1150`).
6. `_com_fio` usa o juiz `fio_vivo(x.perguntas_que_resolvem.all())`; não é o mesmo insumo que `chamados_vivos`. (`app/ponto/services/cartorio.py:1027`, `:1093`, `:1150`).
7. Chamado sem marcos nomeados retorna `None` em `marcos_acesos`, logo continua em `_protestam` por `is not True`. (`app/ponto/services/lastro.py:170`, `:171`, `:172`; `app/ponto/services/cartorio.py:1147`).

## 31/10 00:06 · sessao 67dcc465444f · V-FASE-DONO-AS-OF · verificador · origem=Code

**Reprovaria a obra:** a segunda lavra no mesmo dia pode desfazer a re-lavra histórica; os carimbos não cobrem todas as alterações consumidas pelo censo; e há leitores de fatos passados ainda presos à última lavra. (`app/escala/services/fase_12x36.py:91`, `app/escala/services/fase_12x36.py:174`, `app/escala/signals.py:108`, `app/chamados/services/fila_por_causa.py:174`)

Análise estática da cópia fornecida. Execução dos testes/mutantes, RED/GREEN anunciados e ocorrência dos cenários em produção: **[nao verificado]**. [nao verificado]

**1. Onde o diff falha**

- **Repetir no mesmo dia pode ressuscitar a lavra velha.** A primeira execução reabre outubro e grava sua cobertura e `itens_reabertos`. Sem fato novo, a segunda execução não reabre outubro; entretanto, `update_or_create` substitui o **mesmo snapshot diário**, agora sem aquela cobertura histórica. O leitor volta à lavra anterior que ainda cobre outubro, recuperando o resultado desatualizado. O teste repete somente no dia seguinte, quando nasce outro snapshot. (`app/escala/services/fase_12x36.py:91`, `app/escala/services/fase_12x36.py:171`, `app/escala/services/fase_12x36.py:186`, `app/ponto/management/commands/censo_fase_12x36.py:298`, `app/ponto/management/commands/censo_fase_12x36.py:303`, `app/escala/tests/test_vfase_dono_as_of.py:73`)

- **O gatilho não observa integralmente os insumos do censo.** Ele agrega três carimbos de células existentes; o censo também lê vínculo, âncora e template vivos, além de aceitar dias sem célula. Portanto, pode mudar o resultado sem aparecer carimbo naquela janela — caminhos detalhados na resposta 3. (`app/escala/services/fase_12x36.py:182`, `app/ponto/management/commands/censo_fase_12x36.py:156`, `app/ponto/management/commands/censo_fase_12x36.py:171`, `app/escala/alimentacao.py:253`)

- **“Última cobertura” tem duas implementações diferentes.** O leitor escolhe por **dia contido em intervalo**; a reabertura guarda a última ocorrência do **triplo exato empresa/início/fim**. Se mudar o corte da empresa, intervalos antigos e correntes podem sobrepor. O censo mede ambos separadamente, com paridades próprias; depois o leitor une `itens` e `itens_reabertos`, sem resolver resultados conflitantes do mesmo dia na mesma lavra. Isso permite que a janela volte a influenciar a classificação. (`app/escala/services/fase_12x36.py:114`, `app/escala/services/fase_12x36.py:137`, `app/escala/services/fase_12x36.py:173`, `app/ponto/janelas.py:36`, `app/ponto/management/commands/censo_fase_12x36.py:124`, `app/ponto/management/commands/censo_fase_12x36.py:175`, `app/escala/services/fase_12x36.py:144`)

- **Empresa atual e empresa do item não são reconciliadas.** `InsumosDoDono` consulta pela empresa **atual** do colaborador; o censo também agrupa colaboradores pela empresa atual. Entretanto, depois de selecionar uma lavra pela cobertura da empresa solicitada, o leitor aceita o item apenas por colaborador/data, ignorando `x['empresa']`. Com transferência ou cortes diferentes, a cobertura usada e a empresa do item podem divergir. Transferências concretas no dado fornecido: **[nao verificado]**. (`app/ponto/management/commands/e6_oraculo.py:164`, `app/ponto/management/commands/censo_fase_12x36.py:110`, `app/escala/services/fase_12x36.py:137`, `app/escala/services/fase_12x36.py:146`)

- **O universo pode desaparecer justamente quando o cadastro muda.** Só entram colaboradores que ainda possuem algum vínculo cujo template é 12x36; só suas empresas e células participam da reabertura. Alterar o ciclo do último template 12x36 pode excluir o colaborador do detector, deixando sua lavra histórica sem reconstrução. (`app/ponto/management/commands/censo_fase_12x36.py:104`, `app/ponto/management/commands/censo_fase_12x36.py:110`, `app/escala/services/fase_12x36.py:174`, `app/escala/services/fase_12x36.py:182`)

- **A leitura pode combinar versões diferentes.** Primeiro são lidos PK/cobertura; depois, o detalhe dessas PKs. Uma sobrescrita diária entre as consultas pode entregar cobertura de uma versão e itens de outra. Na escrita, a escolha das reaberturas, a medição e a publicação também são etapas separadas; o timestamp de início não prova quais commits ficaram visíveis durante cada leitura. (`app/escala/services/fase_12x36.py:134`, `app/escala/services/fase_12x36.py:142`, `app/escala/services/fase_12x36.py:91`, `app/ponto/management/commands/censo_fase_12x36.py:62`, `app/ponto/management/commands/censo_fase_12x36.py:116`, `app/ponto/management/commands/censo_fase_12x36.py:282`)

- **O as-of implementado é diário e sobrescrevível.** O filtro temporal usa `data_ref <= hoje`; o `agora` recebido por `InsumosDoDono` não limita a seleção da fase. A única linha daquele dia pode ser substituída, sem preservar a versão intradiária anterior. (`app/escala/services/fase_12x36.py:108`, `app/escala/services/fase_12x36.py:91`, `app/ponto/management/commands/e6_oraculo.py:130`, `app/ponto/management/commands/e6_oraculo.py:164`)

- **O pouso proposto não recompõe o passado inteiro.** Lavras antigas sem `janelas` são excluídas tanto da leitura quanto da descoberta de reaberturas. A primeira execução nova mede somente anterior+atual; dias anteriores a essas duas janelas continuam sem cobertura e sem candidato a reabertura, mesmo que seus fatos mudem. (`app/escala/services/fase_12x36.py:105`, `app/escala/services/fase_12x36.py:171`, `app/ponto/management/commands/censo_fase_12x36.py:81`)

**2. Quem escreve e quem lê**

A porta efetiva grava o JSON inteiro em `MetricaSnapshot`, chave `desvio_de_cadastro_12x36`, escopo `tenant/0`, identificado pela data. O censo monta o conteúdo e chama essa porta. (`app/escala/services/fase_12x36.py:76`, `app/escala/services/fase_12x36.py:91`, `app/ponto/management/commands/censo_fase_12x36.py:271`, `app/ponto/management/commands/censo_fase_12x36.py:282`)

| Campo | Escritor/origem | Leitores efetivos |
|---|---|---|
| `detalhe.janelas` | Censo monta correntes e reabertas. (`app/ponto/management/commands/censo_fase_12x36.py:298`) | Índice; leitor por dia; descoberta de reaberturas. (`app/escala/services/fase_12x36.py:110`, `app/escala/services/fase_12x36.py:137`, `app/escala/services/fase_12x36.py:172`) |
| `detalhe.agora` | `_agora = timezone.now()` na entrada; serializado no detalhe. (`app/ponto/management/commands/censo_fase_12x36.py:62`, `app/ponto/management/commands/censo_fase_12x36.py:302`) | Índice transporta; reabertura compara com carimbos. O leitor por dia descarta `_agora`. (`app/escala/services/fase_12x36.py:110`, `app/escala/services/fase_12x36.py:179`, `app/escala/services/fase_12x36.py:134`) |
| `detalhe.itens` | Censo, incluindo `dono_do_dia` perguntado ao juiz. (`app/ponto/management/commands/censo_fase_12x36.py:274`, `app/ponto/management/commands/censo_fase_12x36.py:291`) | As-of; C10; propositor. (`app/escala/services/fase_12x36.py:144`, `app/escala/services/cadastro_realidade.py:345`, `app/escala/services/propositor.py:492`) |
| `detalhe.itens_reabertos` | Censo, somente linhas reabertas em desvio. (`app/ponto/management/commands/censo_fase_12x36.py:246`, `app/ponto/management/commands/censo_fase_12x36.py:303`) | Classificação por dia no as-of; `ultimo_lavrado` também transporta o JSON inteiro. (`app/escala/services/fase_12x36.py:144`, `app/escala/services/fase_12x36.py:205`) |
| `calculado_em` | ORM, `auto_now=True`; não é o instante de início da medição. (`app/inteligencia/models.py:96`, `app/ponto/management/commands/censo_fase_12x36.py:62`) | Ordenação dos dois leitores; exposição como `medido_em`. (`app/escala/services/fase_12x36.py:109`, `app/escala/services/fase_12x36.py:200`, `app/escala/services/fase_12x36.py:207`) |
| `data_ref` | Porta recebe `_hoje`, derivado de `_agora`. (`app/ponto/management/commands/censo_fase_12x36.py:63`, `app/ponto/management/commands/censo_fase_12x36.py:307`, `app/escala/services/fase_12x36.py:92`) | Filtro/ordenação e metadados dos leitores; vigia e API. (`app/escala/services/fase_12x36.py:108`, `app/escala/services/fase_12x36.py:199`, `app/escala/services/fase_12x36.py:206`, `app/ponto/management/commands/vigiar_familia_escala.py:26`, `app/api/views_mensageria.py:1676`) |

| Carimbo | Escritores reais | Leitores |
|---|---|---|
| `julgada_em` | `carimbar`, usando o `agora` recebido pelo cartório; impressão igual permite não escrever. (`app/ponto/portas/celula.py:708`, `app/ponto/portas/celula.py:717`, `app/ponto/portas/celula.py:721`) | Novo gatilho; salto de julgamento do cartório; invalidação da lavra de perguntas. (`app/escala/services/fase_12x36.py:183`, `app/ponto/services/cartorio.py:943`, `app/ponto/services/cartorio.py:1092`, `app/chamados/veredito_lavrado.py:153`) |
| `regenerada_em` | Regeneração do vínculo **e reversão de regeneração do dia**. (`app/ponto/portas/celula.py:262`, `app/ponto/portas/celula.py:536`) | Novo gatilho; detecção de fato novo; invalidação das perguntas; cobrança órfã. (`app/escala/services/fase_12x36.py:183`, `app/ponto/services/cartorio.py:285`, `app/chamados/veredito_lavrado.py:153`, `app/escala/services/regeneracao.py:80`) |
| `editada_em` | **Sem escritor de produção declarado na cópia**; o contrato registra expressamente essa ausência. A docstring nova atribui-lhe um produtor humano inexistente nesse catálogo. (`app/escala/tests/test_d3_guarda_da_linha_celula.py:50`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:69`, `app/escala/services/fase_12x36.py:160`) | Novo gatilho; detecção de fato novo; invalidação das perguntas. (`app/escala/services/fase_12x36.py:184`, `app/ponto/services/cartorio.py:285`, `app/chamados/veredito_lavrado.py:154`) |

Outros usos dos carimbos incluem cobrança órfã no gerador/supra-juiz/materialização, exibição e diagnósticos, backfill e vigias de integridade. (`app/ponto/management/commands/gerar_celulas.py:242`, `app/ponto/management/commands/supra_juiz.py:152`, `app/chamados/management/commands/materializar_passivo_validado.py:220`, `app/ponto/services/dia_do_colab.py:91`, `app/ponto/management/commands/celula_veredito_velho.py:109`, `app/ponto/management/commands/backfill_b5_insumos.py:61`, `app/ponto/management/commands/vigia_b5_ausencia.py:55`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1593`)

**Leitores de passado que ficaram fora:**

- **Fila por causa:** conhece a data do fato, mas lê `colabs_de_desvio` da última lavra e decide `grade_errada` apenas pelo colaborador. Um chamado antigo pode mudar de gaveta quando a janela anda; a classificação também pode alcançar dias que nunca foram itens de fase. (`app/chamados/services/fila_por_causa.py:161`, `app/chamados/services/fila_por_causa.py:174`, `app/chamados/services/fila_por_causa.py:199`, `app/chamados/services/fila_por_causa.py:220`)
- **Propositor com janela ampliada:** a CLI aceita `--janela` arbitrária, enquanto o gatilho de fase continua lendo somente `ultimo_lavrado().itens`. A justificativa de “28 dias dentro de anterior+atual” não cobre essa porta existente. (`app/escala/management/commands/propostas_de_escala.py:12`, `app/escala/management/commands/propostas_de_escala.py:29`, `app/escala/services/propositor.py:416`, `app/escala/services/propositor.py:492`)

O novo leitor chega a e6, teto/C9 e pautas discordantes por `InsumosDoDono`; ele fornece **dias de fase**, não o `dono_do_dia` armazenado: o dono é novamente composto com outros insumos. (`app/ponto/management/commands/e6_oraculo.py:164`, `app/ponto/management/commands/e6_oraculo.py:167`, `app/ponto/management/commands/e6_oraculo.py:310`, `app/ponto/services/teto_na_lista.py:72`, `app/ponto/management/commands/pautas_discordantes.py:82`)

**Leitores do “hoje”:** C10/propositor continuam em `itens`; fila/pauta continuam nos IDs; vigia/API continuam nos metadados e linhas correntes. As reabertas são retiradas dessas listas. Porém, na transição, `ultimo_lavrado` aceita legado com `itens` sem `janelas`, enquanto o as-of o exclui: C10 e dono podem discordar até sobre dias das janelas correntes. (`app/escala/services/cadastro_realidade.py:345`, `app/escala/services/propositor.py:492`, `app/chamados/services/fila_por_causa.py:174`, `app/escala/services/fase_12x36.py:219`, `app/ponto/management/commands/censo_fase_12x36.py:247`, `app/escala/services/fase_12x36.py:201`, `app/escala/services/fase_12x36.py:105`)

**3. Mudanças que podem alterar o censo sem mover os três carimbos**

**Não, os carimbos não cobrem tudo.** Estes são caminhos concretos da cópia; sua ocorrência em produção permanece **[nao verificado]**.

| Mudança | Caminho e buraco |
|---|---|
| **Âncora de vínculo antigo** | O censo usa a âncora viva. O signal regenera somente desde a janela atual; mudança para `None` retorna sem regenerar. Assim, o passado antigo pode mudar de previsto sem carimbo nele. (`app/ponto/management/commands/censo_fase_12x36.py:171`, `app/escala/services/propositor.py:218`, `app/escala/signals.py:108`, `app/escala/signals.py:114`) |
| **Invalidação de âncoras por `update`** | `invalidar_ancoras_incoerentes` escreve `None` por `QuerySet.update`, sem atravessar o signal de âncora; pode alcançar outros vínculos ativos além daquele regenerado pelo ato principal. (`app/colaboradores/services/vinculo.py:849`, `app/colaboradores/services/vinculo.py:852`, `app/colaboradores/services/vinculo.py:857`, `app/escala/utils.py:2049`) |
| **Reversão de vínculo por recusa** | A recusa chama `reverter_para_snapshot`: apaga vínculos e restaura tipo/vigência/ativa por `update`, sem regeneração nessa função. A exclusão pode zerar a geradora via `SET_NULL`; isso muda a escolha do vínculo sem carimbar a célula. (`app/chamados/services/acoes_chamado.py:141`, `app/colaboradores/services/vinculo.py:864`, `app/colaboradores/services/vinculo.py:874`, `app/escala/models.py:1660`, `app/escala/alimentacao.py:237`) |
| **Ciclo/geometria de template histórico** | A propagação alcança só vínculos ativos e desde a competência anterior. O censo consulta ciclo vivo; o pareador também recebe geometria viva. Vínculos encerrados e passado mais antigo podem mudar sem carimbo. (`app/escala/services/cadastro_tipo.py:334`, `app/escala/services/cadastro_tipo.py:337`, `app/ponto/management/commands/censo_fase_12x36.py:157`, `app/ponto/turnos.py:1889`) |
| **Correção retroativa com regeneração parcial ou barrada** | A correção muda o vínculo; a regeneração pode ter piso posterior ao trecho alterado, pular competência lavrada ou preservar célula humana. O censo continua lendo o vínculo alterado. (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:212`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:225`, `app/ponto/portas/celula.py:238`, `app/ponto/portas/celula.py:251`, `app/ponto/management/commands/censo_fase_12x36.py:171`) |
| **Batida em janela sem células** | Inserção/exclusão tenta julgar, mas o cartório retorna se não houver célula. O censo mede pelo vínculo e pelas batidas; o gatilho não tem linha para agregar. O mesmo problema alcança retratação salva pelo receiver. (`app/ponto/signals.py:131`, `app/ponto/signals.py:138`, `app/ponto/services/cartorio.py:1547`, `app/escala/alimentacao.py:253`, `app/ponto/management/commands/censo_fase_12x36.py:172`, `app/escala/services/fase_12x36.py:182`) |
| **Batida movida para outra data distante** | A regularização externa altera `timestamp`; o receiver julga apenas a **nova** data e sua véspera. A janela de origem não recebe carimbo no evento, embora perca a batida; depende da varredura posterior. (`app/chamados/services/regularizacao_ext.py:149`, `app/chamados/services/regularizacao_ext.py:155`, `app/ponto/signals.py:108`, `app/ponto/signals.py:135`, `app/ponto/management/commands/processar_cartorio.py:49`) |
| **Nascimento de célula ou enriquecimento de DNA antigo** | O gerador cria célula ou preenche DNA nulo sem escrever os três carimbos. Isso pode mudar vínculo escolhido/geometria dos turnos. `gerada_em`, que registra o nascimento, ficou fora do gatilho. (`app/ponto/portas/celula.py:592`, `app/ponto/portas/celula.py:603`, `app/escala/alimentacao.py:237`, `app/ponto/turnos.py:1946`, `app/ponto/turnos.py:1973`, `app/escala/services/fase_12x36.py:183`) |
| **DNA apagado fora da vigência** | Esse ramo salva somente `trabalha`/`dna`. O julgamento posterior depende de `recalculo` e exclui vereditos humanos; portanto não há garantia de atualização de nenhum dos três carimbos pelo ato. (`app/ponto/portas/celula.py:231`, `app/ponto/portas/celula.py:233`, `app/ponto/portas/celula.py:360`, `app/ponto/portas/celula.py:365`) |

A impressão do cartório contém batidas e partes do DNA, mas não a âncora/vigência viva que o censo usa. Logo, mesmo executar o cartório não transforma automaticamente esses carimbos num rastreador completo dos insumos do censo. (`app/ponto/services/cartorio.py:323`, `app/ponto/services/cartorio.py:327`, `app/ponto/services/cartorio.py:1092`, `app/ponto/management/commands/censo_fase_12x36.py:156`, `app/ponto/management/commands/censo_fase_12x36.py:171`)

**4. Checklist das classes de reprovação**

| Classe | Aparece neste diff? |
|---|---|
| **Segundo juiz duplicando juiz registrado** | **Não identifiquei essa duplicação introduzida.** Fase continua julgada pelo censo; vínculo e janelas continuam delegados às autoridades existentes. `_cobre` pergunta cobertura da lavra, não cobertura de vínculo. (`app/ponto/management/commands/censo_fase_12x36.py:246`, `app/ponto/management/commands/censo_fase_12x36.py:156`, `app/ponto/management/commands/censo_fase_12x36.py:84`, `app/escala/services/fase_12x36.py:113`) |
| **Segundo escritor fora da porta** | **Não para essa chave.** O diff continua publicando por `lavrar`; o defeito é substituir a fotografia diária e perder cobertura histórica. (`app/ponto/management/commands/censo_fase_12x36.py:271`, `app/ponto/management/commands/censo_fase_12x36.py:282`, `app/escala/services/fase_12x36.py:91`) |
| **Relógio lido fora da borda** | **Nenhuma leitura nova de relógio no miolo adicionado.** O censo captura `_agora` na entrada. O problema temporal é tratar carimbo/início como prova de leitura e permitir sobrescrita, não um novo `now()` escondido. (`app/ponto/management/commands/censo_fase_12x36.py:62`, `app/escala/services/fase_12x36.py:179`, `app/escala/services/fase_12x36.py:186`, `app/escala/services/fase_12x36.py:91`) |
| **Derivação paralela de fonte única** | **Sim, na escolha da lavra cobridora:** por dia no leitor, por identidade exata da janela no gatilho. Elas divergem quando as coberturas sobrepõem. (`app/escala/services/fase_12x36.py:137`, `app/escala/services/fase_12x36.py:173`) |
| **Selo/RED sem mordida suficiente** | **Sim, cobertura insuficiente:** os testes deixam sobreviver retirada dos carimbos de cadastro e do filtro de empresa; também não repetem a re-lavra no mesmo dia. Mutantes abaixo; execução deles **[nao verificado]**. (`app/escala/tests/test_vfase_dono_as_of.py:50`, `app/escala/tests/test_vfase_dono_as_of.py:73`, `app/escala/tests/test_o261_v_fase_na_lista.py:52`) |
| **Ordem de trava** | **Inversão de aquisição de travas: [nao verificado].** Há uma falha comprovável de consistência entre seleção e leitura/publicação, que não deve ser chamada de deadlock. (`app/escala/services/fase_12x36.py:134`, `app/escala/services/fase_12x36.py:142`, `app/escala/services/fase_12x36.py:91`) |
| **Pouso antes do pré-requisito** | **Sim, no desenho:** o leitor novo exige cobertura que o legado não possui. Além disso, o gatilho passou a depender de julgamento, mas o cron continua declarando somente `reconciliar_vinculo`, com comentário dispensando o cartório. Horário nominal não garante conclusão. Pouso efetivamente realizado: **[nao verificado]**. (`app/escala/services/fase_12x36.py:105`, `app/escala/services/fase_12x36.py:183`, `app/config/crons.py:617`, `app/config/crons.py:620`) |

**5. Os testes mordem? Qual mutante cada um deixa verde?** [nao verificado]

**Mordem partes do defeito original; não sustentam a condição completa do aval.** Há seis testes novos, com uma empresa na fixture; a

[truncado em 20000 de 23568 caracteres]

## 31/10 00:06 · sessao f3688f19738e · DISJUNTOR-E-CLEAR-RAPIDO · verificador · origem=Code

Há motivos para reprovar esta FINAL: o quarto verificador pode escapar, o orçamento diário tem desvios, a chave pode ser contornada e há caminhos que negam ou travam com medida desconhecida. (`bin/disjuntor.py:749`, `bin/disjuntor.py:696`, `bin/disjuntor.py:706`, `bin/disjuntor.py:117`, `bin/disjuntor.py:173`)

**a) Falsos positivos e permissões indevidas**

| Ponto | Comportamento encontrado | Evidência |
|---|---|---|
| Guarda da chave | Nega `Read` da chave e `Bash` como `cat DISJUNTOR_DESLIGADO`. A condição verifica nome, sem distinguir consulta de alteração; também alcança outros arquivos com o mesmo prefixo. | `bin/disjuntor.py:117`, `bin/disjuntor.py:119` |
| Proteção incompleta | `Bash` pode sobrescrever `.claude/settings.json` sem citar a chave. `Edit` pode trocar apenas `pretool` por outro argumento: a guarda exige que `old_string` contenha `disjuntor.py`. Nome da chave montado por concatenação também escapa. | `bin/disjuntor.py:116`, `bin/disjuntor.py:127` |
| ESCADA | Nega `Agent`, mas `Write/Edit/Bash` da sessão principal normalmente saem antes da medição. Portanto, “sem obra nova” fica como orientação, sem impedir sua abertura pela própria sessão. | `bin/disjuntor.py:728`, `bin/disjuntor.py:744` |
| Palavra “escada” | Só reconhece o começo da última mensagem humana elegível; outra mensagem encerra a escada. “Agora use a escada”, por exemplo, não ativa. Essa restrição de posição/duração excede a definição “a palavra dele” fornecida. | `bin/disjuntor.py:64`, `bin/disjuntor.py:516`, `bin/disjuntor.py:529` |
| Quarto verificador | PreToolUse consulta apenas o estado acumulado. As três reprovações são descobertas no fim do Stop, depois de várias saídas antecipadas. Outro verificador lançado antes dessa varredura passa. | `bin/disjuntor.py:749`, `bin/hook_stop_fila1.py:1004`, `bin/hook_stop_fila1.py:1256` |
| Identificação de reprovação | Exige meta `verificador`, prompt com `LEITURA FINAL`, `obra <ID>` conforme regex e veredito no `SubagentHandback`. Outra grafia, canal ou verificação via Codex não recebe a mesma guarda. | `bin/disjuntor.py:71`, `bin/disjuntor.py:412`, `bin/disjuntor.py:419`, `bin/disjuntor.py:435`, `bin/disjuntor.py:747` |
| Rearme B3 | Guarda os IDs antigos em `rearmado`, mas a varredura seguinte ignora esse campo e recoloca as mesmas reprovações. Pode voltar a negar imediatamente depois do rearme. | `bin/disjuntor.py:443`, `bin/disjuntor.py:468` |
| Orçamento diário | Fora de `contrato`, só corta papel literal `verificador`. `censo`, `leitura`, `sonda` e papel não reconhecido passam, contrariando a formulação “só a volta FINAL usa Codex”. | `bin/disjuntor.py:702`, `bin/disjuntor.py:706` |
| ESCADA × orçamento | Com Codex >85% e escada ativa, retorna autorização **antes** de verificar contrato e orçamento diário. Assim, contrato e volta intermediária acima do orçamento também passam. | `bin/disjuntor.py:696`, `bin/disjuntor.py:702` |
| Interpretação de Bash | Trata texto de `echo`/comentário como chamada. `… CODEX_VOLTA=final bash bin/codex_pergunta.sh …` perde papel/volta no parser: FINAL com anexo pode ser negada; chamada intermediária sem anexo pode escapar. `codex exec` direto nem entra nessa fiscalização. | `bin/disjuntor.py:649`, `bin/disjuntor.py:659`, `bin/disjuntor.py:687`, `bin/disjuntor.py:728` |
| Trava de sombra | Ocupada ou ilegível, suspende inclusive B3, B4 e controle Codex; portanto deixa passar chamadas que os limiares mandariam negar. | `bin/disjuntor.py:237`, `bin/disjuntor.py:244`, `bin/disjuntor.py:730` |

A negativa de contrato apenas **manda** escrever `sem Codex: orcamento`; o registro automático no RELATO contém regra, ferramenta e alvo, sem garantir essa anotação na linha da obra. (`bin/disjuntor.py:703`, `bin/disjuntor.py:165`)

**b) Medidas ilegíveis, ausentes ou inesperadas**

- **Cota ausente, vencida ou JSON inválido:** vira `None`; sem palavra escada legível, deixa passar. Contudo, `NaN` é aceito como número e acaba negando agente com motivo `nan% >=75%`, pois tanto `NaN >=85` quanto `NaN <75` são falsos. (`bin/disjuntor.py:485`, `bin/disjuntor.py:526`, `bin/disjuntor.py:541`)
- **Statusline com estrutura inesperada:** `rate_limits="x"` ou `seven_day="x"` levanta fora do bloco protetor; `context_window="x"` também pode levantar. Sem atualização, a cota anterior continua válida até o reset; o leitor não verifica idade nem sessão. (`bin/statusline_cota.py:28`, `bin/statusline_cota.py:30`, `bin/statusline_cota.py:42`, `bin/disjuntor.py:485`)
- **Estado do disjuntor ilegível:** vira `{}`, perdendo memória de B1/B3. Estruturas internas erradas podem provocar exceção; no PreToolUse ela normalmente libera. Inclusive cota conhecida que mandaria negar pode passar se a gravação do estado falhar antes do retorno da negativa. (`bin/disjuntor.py:206`, `bin/disjuntor.py:224`, `bin/disjuntor.py:763`, `bin/disjuntor.py:780`)
- **Transcript B1 ilegível:** não dispara. Porém, **cópia ilegível é diferente**: erros de `scandir/stat` são ignorados, retornando “nenhum arquivo novo”; isso pode disparar B1 indevidamente. Meta ilegível também perde a isenção de agente somente leitura. (`bin/disjuntor.py:275`, `bin/disjuntor.py:305`, `bin/disjuntor.py:324`, `bin/disjuntor.py:329`, `bin/disjuntor.py:366`)
- **Transcript humano inválido:** linhas inválidas são puladas; uma mensagem anterior pode continuar determinando escada. Falha global de leitura retorna `None`. (`bin/disjuntor.py:501`, `bin/disjuntor.py:516`, `bin/disjuntor.py:518`)
- **Piso:** procura o primeiro assistente com tokens positivos, podendo pular o verdadeiro primeiro turno sem `usage`. Se não medir no primeiro Stop, consome `piso_pendente` e não tenta novamente naquele ciclo. (`bin/orcamento_contexto.py:265`, `bin/hook_stop_fila1.py:866`)

**Sim, há exceção que resulta em bloqueio:** falha do orçamento retorna ao caminho normal do Stop; como CLEAR está excluído do PAREI genérico, um par válido que normalmente liberaria pode terminar bloqueado até o TETO. Além disso, erro de `stat` em qualquer novo arquivo protegido invalida até uma prova ESPERANDO apresentada por outro arquivo legítimo. (`bin/hook_stop_fila1.py:977`, `bin/hook_stop_fila1.py:1080`, `bin/hook_stop_fila1.py:1260`, `bin/hook_stop_fila1.py:563`)

**Há travamento sem prazo no código:** o lock do RELATO usa `flock(LOCK_EX)` bloqueante; a coleta Codex usa `open` comum em `.jsonl`, podendo esperar em FIFO. A abertura protegida contra FIFO existe em `orcamento_contexto.abre`, mas não nessa coleta Codex. (`bin/disjuntor.py:173`, `bin/codex_limite_estado.py:141`, `bin/orcamento_contexto.py:74`)

**c) Escritores e leitores**

Os caminhos operacionais são `logs/sessao/*`. `logs/disjuntor/contrato.txt` aparece como referência documental; autor, conteúdo e consumidores desse contrato ficam **[nao verificado]**. (`bin/disjuntor.py:2`, `bin/disjuntor.py:47`)

| Arquivo | Quem escreve | Quem lê |
|---|---|---|
| `logs/sessao/disjuntor.log` | `disjuntor.trilha`, chamado pelos disparos/falhas/rearme. | `disjuntor.ler`, publicado pelo gerador de ESTADO. (`bin/disjuntor.py:85`, `bin/disjuntor.py:826`, `bin/gerar_estado.py:280`) |
| `disjuntor_estado.json` | `_com_estado`, usado por PreToolUse, Stop e CLI de rearme; temporário `.disjuntor_estado.*` seguido de replace. | `_com_estado`, `ler_estado` e decisões B1/B3/B4. (`bin/disjuntor.py:185`, `bin/disjuntor.py:209`, `bin/disjuntor.py:218`) |
| `disjuntor_estado.json.lock` | Criado/aberto por `_com_estado`. | Os mesmos participantes usam flock, com tentativas limitadas. (`bin/disjuntor.py:191`, `bin/disjuntor.py:193`) |
| `disjuntor_estado.json.relato.lock` | Criado/aberto por `relato_negacao`. | As negativas do disjuntor usam esse lock; o escritor antigo não o adquire. (`bin/disjuntor.py:171`, `app/core/esteira_vigia.py:830`) |
| `cota.json` | `statusline_cota`, por `.cota.*` e replace. | `disjuntor.ler_cota`, usado pelas decisões e ESTADO. (`bin/statusline_cota.py:34`, `bin/statusline_cota.py:39`, `bin/disjuntor.py:480`) |
| `pisos.log` | `registrar_piso`, acionado pelo Stop **e** por `--semear-pisos`. | `ultimo_piso`/`gatilho_tokens` e painel do disjuntor. (`bin/orcamento_contexto.py:198`, `bin/orcamento_contexto.py:309`, `bin/hook_stop_fila1.py:870`, `bin/disjuntor.py:869`) |
| `orcamento_estado.json` | Continua sendo `_orc_salva` do Stop, com temporário e replace. | Stop, `orcamento_contexto.ler` e `disjuntor.ler`. (`bin/hook_stop_fila1.py:722`, `bin/hook_stop_fila1.py:817`, `bin/orcamento_contexto.py:353`, `bin/disjuntor.py:850`) |
| `orcamento.log` | `_orc_trilha` do Stop. | Avaliador/painel de orçamento e painel do disjuntor. (`bin/hook_stop_fila1.py:699`, `bin/orcamento_contexto.py:359`, `bin/disjuntor.py:826`) |
| `ultimo_clear.json` e lock | `marco_de_clear.py`, preservando clears anteriores e usando replace. | Próprio escritor e `orcamento_contexto.ler_marco`. (`bin/marco_de_clear.py:38`, `bin/marco_de_clear.py:47`, `bin/marco_de_clear.py:61`, `bin/orcamento_contexto.py:152`) |
| `RELATO.md` e `.novo` | Novo chamador `relato_negacao` reutiliza `esteira_vigia.no_relato`. | Stop, handoff e publicação. (`bin/disjuntor.py:176`, `app/core/esteira_vigia.py:845`, `bin/hook_stop_fila1.py:408`, `bin/handoff_sessao.sh:88`, `bin/relato.sh:104`) |
| `DISJUNTOR_DESLIGADO` | O código operacional descrito apenas consulta; o manifesto atribui `touch/rm` ao dono. Exclusividade efetiva: **[nao verificado]**, pelas brechas da guarda. | `desligado`, no PreToolUse, Stop e painel. (`bin/disjuntor_settings.json:3`, `bin/disjuntor.py:103`, `bin/disjuntor.py:775`, `bin/hook_stop_fila1.py:994`) |

**Existe concorrência de escritores do RELATO.** Reutilizar a função não cria escritor único por processo: o vigia antigo continua chamando `no_relato` sem o lock novo; ambos usam o temporário fixo `RELATO.md.novo`. Há ainda `gerar_cortes.py`, que reescreve o RELATO diretamente. Isso permite perda de atualização; ocorrência em produção **[nao verificado]**. (`app/core/esteira_vigia.py:845`, `app/core/esteira_vigia.py:1129`, `bin/disjuntor.py:171`, `bin/gerar_cortes.py:136`)

A negativa é emitida **antes** de registrar no RELATO. RELATO ausente causa retorno silencioso; erro na gravação mantém a negativa e apenas tenta registrar FALHA. Portanto, “cada negação no RELATO” não está garantido. (`bin/disjuntor.py:169`, `bin/disjuntor.py:179`, `bin/disjuntor.py:778`)

Não aparece novo escritor direto do estado antigo de orçamento: muda o conteúdo que o mesmo Stop salva. O orçamento **Codex** é calculado das sessões, sem novo estado persistente próprio. (`bin/hook_stop_fila1.py:875`, `bin/codex_limite_estado.py:158`)

A configuração entregue aponta ao projeto; preservação da configuração de usuário efetivamente instalada fica **[nao verificado]**. (`.claude/settings.json:68`, `bin/disjuntor_settings.json:2`)

**d) Mudanças nas decisões antigas de Stop**

Sim; não se limita a acrescentar recados:

- CLEAR passa a exigir handoff novo, pode ser adiado por agente/trava e ganha um bloqueio adicional quando o handoff aparece. (`bin/hook_stop_fila1.py:882`, `bin/hook_stop_fila1.py:895`, `bin/hook_stop_fila1.py:935`)
- A validade cai para dez minutos; tempo e quantidade de deploys deixam de disparar, substituídos pelo limiar adaptativo de tokens. Esses comportamentos não constam dos requisitos transcritos; aval adicional **[nao verificado]**. (`bin/orcamento_contexto.py:56`, `bin/orcamento_contexto.py:225`, `bin/orcamento_contexto.py:335`)
- `PAREI ! COTA` perde a liberação genérica mesmo com disjuntor desligado ou sem escada ativa. (`bin/hook_stop_fila1.py:994`, `bin/hook_stop_fila1.py:1080`)
- B2 acrescenta liberação antecipada e conta **todo `BLOCK`**, incluindo orçamento, B4 e RELATO sem prova, em vez de identificar somente `siga` da fila. (`bin/disjuntor.py:591`, `bin/hook_stop_fila1.py:1156`, `bin/hook_stop_fila1.py:1179`, `bin/hook_stop_fila1.py:1251`)
- Sem o par COTA+handoff, B4 bloqueia uma vez e depois vira lembrete. O fluxo antigo pode então liberar por PAREI comum, ESPERANDO, fila vazia, TETO ou B2, sem comprovar fechamento do marco. (`bin/disjuntor.py:633`, `bin/hook_stop_fila1.py:1182`, `bin/hook_stop_fila1.py:1187`, `bin/hook_stop_fila1.py:1201`, `bin/hook_stop_fila1.py:1240`)

**e) Selos que não mordem**

| Mutação | Por que as asserções funcionais não a distinguem |
|---|---|
| `< B4_NEGA` → `<= B4_NEGA` | Muda exatamente 75%, mas as entradas são 74,9%, 80% e 90%. (`bin/disjuntor.py:541`, `bin/tests/test_disjuntor.sh:305`, `bin/tests/test_disjuntor.sh:310`, `bin/tests/test_disjuntor.sh:312`) |
| `>= B4_PARA` → `> B4_PARA` | Muda exatamente 85% Claude; o teste de escada por cota usa 90%. (`bin/disjuntor.py:526`, `bin/tests/test_disjuntor.sh:312`) |
| `agora - desde <= janela` → `< janela` | Muda o instante exato de dez minutos; os testes usam imediato, nove, onze e vinte minutos. (`bin/hook_stop_fila1.py:918`, `bin/tests/test_disjuntor.sh:159`, `bin/tests/test_disjuntor.sh:161`, `bin/tests/test_orcamento_contexto.sh:188`, `bin/tests/test_orcamento_contexto.sh:192`, `bin/tests/test_orcamento_contexto.sh:408`) |

As duas primeiras alterações também mexem em âncoras textuais do executor de mutantes: eventual falha por “âncora sumiu” não demonstra que o caso funcional detectou o erro. (`bin/tests/test_disjuntor.sh:463`, `bin/tests/test_disjuntor.sh:487`, `bin/tests/test_disjuntor.sh:492`)

Além disso, várias autorizações são verificadas apenas por stdout vazio, exatamente a saída de uma exceção capturada antes do deny. Esses casos não distinguem autorização correta de falha aberta. (`bin/tests/test_disjuntor.sh:365`, `bin/tests/test_disjuntor.sh:375`, `bin/disjuntor.py:780`)

Execução integral dos selos e resultado dos mutantes declarados: **[nao verificado]**; os scripts criam e removem arquivos, incompatíveis com a regra desta revisão. (`bin/tests/test_disjuntor.sh:35`, `bin/tests/test_disjuntor.sh:37`, `bin/tests/test_orcamento_contexto.sh:101`)

_840 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
