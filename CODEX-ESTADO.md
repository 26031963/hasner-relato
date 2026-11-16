# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 08:18** · publicado em **16/11/2026 08:18** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 16/11 08:18 · sessao f0b9a79b79d4 · - · - · origem=Ronald (tmux)

Causa candidata dos **dois dias col112: borda mensal deslocando a fase do pareamento sequencial**. A leitura mensal começa em 20/10 às 12h, excluindo a entrada de 06:55 desse dia; o oráculo recebe apenas timestamps e pareia por posição, sem considerar E/S (`app/ponto/management/commands/e6_oraculo.py:149-153`; `app/ponto/calculador/nucleo.py:245-247`).

Reprodução em memória apenas com as batidas fornecidas:

| Dia | Par produzido no subconjunto mensal | Minutos |
|---|---|---:|
| 21/10 | S 21/10 17:03:39.947643 → E 22/10 07:01:34.438467 | 837.9081804 |
| 22/10 | S 22/10 17:02:17.198077 → E 23/10 06:56:52.572410 | 834.5895722166666 |

Os valores reproduzem exatamente os E6 mensais fornecidos; o mecanismo está no recorte de leitura e na soma por posição (`app/ponto/management/commands/e6_oraculo.py:149-153`; `app/ponto/calculador/nucleo.py:245-247`). Incluindo a manhã de 20/10, a mesma função produz 605.7907565833334 e 600.7126601666666, os E6_JANELA informados (`app/ponto/calculador/nucleo.py:216-247`).

O gap noturno inferior a 14h com uma batida acumulada não corta; o vão maior que 14h corta depois do par invertido. Como a contagem acumulada nesse corte é par, `corte_que_orfanou` fica falso (`app/ponto/calculador/nucleo.py:216-225`). A soma é atribuída à data da primeira batida do turno (`app/ponto/calculador/nucleo.py:236-247`).

O juiz classifica zero batidas como contagem par: sem outros indicadores, cai no resíduo ESTRUTURA. Isso explica por que dias sem turno iniciado no dia podem continuar nessa classe; não comprova a causa específica de cada divergência (`app/ponto/management/commands/e6_oraculo.py:98-110`).

A quantidade de batidas ímpar na janela de três dias não equivale ao `impar` do dia-alvo: o primeiro é contagem do universo; o segundo depende dos turnos atribuídos à data de início (`app/ponto/calculador/nucleo.py:235-244`).

Minutos do espelho, pausa reconhecida e efeito efetivamente aplicado permanecem [nao verificado]. [nao verificado]

## 16/11 08:17 · sessao dbabf29f7de9 · ESPELHO-VERDADE-FECHO-V0 · sonda · origem=Code

1. O `TOTAL` fornecido tem **dias=19, ok=19, erro=0**; portanto, **ok+erro=19**. Não há registros `ERRO`, classes de erro registradas nem marcador `INCOMPLETO` no trecho. Isso descreve a execução da sonda; concordância entre E6 e espelho permanece **[nao verificado]**.

Os minutos e a fonte do espelho permanecem **[nao verificado]**: o anexo informa `nao lido: grava`. O leitor exige `minutos_realizados` ou `pago_h`; sem ambos, retorna ausência de lavratura, não zero (`app/ponto/management/commands/e6_oraculo.py:263-269`).

2. Na tabela, os campos de produção são transcrições do anexo, todos **[nao verificado]** por arquivo:linha. Os minutos estão arredondados a seis casas. `T/O` significa quantidade de turnos da janela / `corte_que_orfanou`; marcos seguem `hi/hii/hfi/hf`; `A` mostra ausências confirmadas/candidatas; `J` mostra justificativas. **[nao verificado]**

As causas são hipóteses: sua aplicação efetiva pelo espelho permanece **[nao verificado]**. As referências sustentam os mecanismos descritos.

| Colaborador | Dia | E6 mensal / E6_JANELA, minutos | T/O | Batidas da janela; E6.impar | Célula: existe / trabalha / tipo_ata; marcos | Ausências e justificativas | CAUSA CANDIDATA ou fato faltante |
|---|---|---:|---|---|---|---|---|
| col129 | 2026-10-12 | 779,158268 / 659,427085 | 3/não | 5, ímpar; não | sim/sim/trabalho; 21:30/—/—/05:42; intervalo livre | A=0/3: atestado_medico aguardando_decisao; folga_compensatoria aprovada; folga rejeitada. J=0. **[nao verificado]** | **Não nomeada.** Falta a composição dos turnos **mensais** que produziu o valor diferente da janela. O agrupamento depende do acumulador e dos cortes (`app/ponto/calculador/nucleo.py:186-228,236-247`). |
| col218 | 2026-10-15 | 0 / 0 | 3/não | 7, ímpar; não | sim/sim/trabalho; 21:00/02:00/03:00/05:00 | A=0/30: ferias aprovadas. J=0. **[nao verificado]** | **D — atribuição do dia:** turnos E6 começam em 14 ou 16/10; o realizado pode atribuir madrugada à véspera (`app/ponto/calculador/nucleo.py:236-247`; `app/ponto/turnos.py:285-309`). |
| col252 | 2026-09-23 | 594,347367 / 594,347367 | 3/não | 12, par; não | sim/sim/trabalho; 07:00/12:38/13:38/17:00 | A=0/2: atestado aguardando_decisao; declaracao aprovada. J=0. **[nao verificado]** | **P — pausa E6 de 13,361900 min abaixo do piso**, com possível desconto da janela declarada (`app/ponto/turnos.py:542-546,756-764`). |
| col252 | 2026-09-24 | 597,758567 / 597,758567 | 3/não | 12, par; não | sim/sim/trabalho; 07:00/12:38/13:38/17:00 | A=0/2: atestado aguardando_decisao; declaracao aprovada. J=0. **[nao verificado]** | **P — pausa E6 de 13,569383 min abaixo do piso**, com possível desconto da janela declarada (`app/ponto/turnos.py:542-546,756-764`). |
| col252 | 2026-09-25 | 603,521650 / 603,521650 | 2/não | 8, par; não | sim/sim/trabalho; 07:00/12:38/13:38/17:00 | A=0/2: atestado aguardando_decisao; declaracao aprovada. J=0. **[nao verificado]** | **P — pausa E6 de 12,438000 min abaixo do piso**, com possível desconto da janela declarada (`app/ponto/turnos.py:542-546,756-764`). |
| col252 | 2026-09-28 | 580,809029 / 580,809029 | 2/não | 6, par; não | sim/sim/trabalho; 07:00/12:38/13:38/17:00 | A=0/2: atestado aguardando_decisao; declaracao aprovada. J=0. **[nao verificado]** | **P — pausa E6 de 14,634152 min abaixo do piso**, com possível desconto da janela declarada (`app/ponto/turnos.py:542-546,756-764`). |
| col252 | 2026-10-01 | 590,132750 / 590,132750 | 2/não | 8, par; não | sim/sim/trabalho; 07:00/12:38/13:38/17:00 | A=0/3: atestado aguardando_decisao; declaracao aprovada; atestado aprovado. J=0. **[nao verificado]** | **P — pausa E6 de 14,443733 min abaixo do piso**, com possível desconto da janela declarada (`app/ponto/turnos.py:542-546,756-764`). |
| col252 | 2026-10-02 | 592,629317 / 592,629317 | 2/não | 8, par; não | sim/sim/trabalho; 07:00/12:38/13:38/17:00 | A=0/3: atestado aguardando_decisao; declaracao aprovada; atestado aprovado. J=0. **[nao verificado]** | **P — pausa E6 de 13,069783 min abaixo do piso**, com possível desconto da janela declarada (`app/ponto/turnos.py:542-546,756-764`). |
| col252 | 2026-10-06 | 597,051900 / 597,051900 | 2/não | 8, par; não | sim/sim/trabalho; 07:00/12:38/13:38/17:00 | A=0/4: atestado aguardando_decisao; declaracao aprovada; dois atestados aprovados. J=0. **[nao verificado]** | **P — pausa E6 de 14,038883 min abaixo do piso**, com possível desconto da janela declarada (`app/ponto/turnos.py:542-546,756-764`). |
| col252 | 2026-10-09 | 591,991200 / 591,991200 | 2/não | 8, par; não | sim/sim/trabalho; 07:00/12:38/13:38/17:00 | A=0/4: atestado aguardando_decisao; declaracao aprovada; dois atestados aprovados. J=0. **[nao verificado]** | **P — pausa E6 de 11,989283 min abaixo do piso**, com possível desconto da janela declarada (`app/ponto/turnos.py:542-546,756-764`). |
| col369 | 2026-10-12 | 416,214867 / 416,214867 | 2/não | 8, par; não | sim/não/folga; marcos ausentes | A=0/32: 30 ferias aprovadas; folga_abonada aprovada; declaracao aprovada. J=0. **[nao verificado]** | **Não nomeada.** Nas quatro batidas do alvo, tipos E,E,S,S; faltam papéis efetivos da ATA, turnos do juiz e fonte do espelho. E6 soma posições; o pareador consulta papéis (`app/ponto/calculador/nucleo.py:245-253`; `app/ponto/turnos.py:1310-1325,1382`). |
| col37 | 2026-10-05 | 0 / 0 | 2/não | 6, par; não | sim/sim/trabalho; 19:00/02:00/03:00/07:00 | A=0/30: ferias aprovadas. J=0. **[nao verificado]** | **D — atribuição do dia:** turnos E6 começam em 04 e 06/10; madrugada pode pertencer à véspera (`app/ponto/calculador/nucleo.py:236-247`; `app/ponto/turnos.py:285-309`). |
| col37 | 2026-10-09 | 0 / 0 | 2/não | 6, par; não | sim/sim/trabalho; 19:00/02:00/03:00/07:00 | A=0/30: ferias aprovadas. J=0. **[nao verificado]** | **D — atribuição do dia:** turnos E6 começam em 08 e 10/10; madrugada pode pertencer à véspera (`app/ponto/calculador/nucleo.py:236-247`; `app/ponto/turnos.py:285-309`). |
| col382 | 2026-09-30 | 0 / 0 | 3/não | 9, ímpar; não | sim/sim/trabalho; 23:50/03:50/04:50/07:50 | A=0/0. J=0. **[nao verificado]** | **D — atribuição do dia:** turnos E6 começam em 29/09 ou 01/10; há segmento atravessando meia-noite (`app/ponto/calculador/nucleo.py:236-247`; `app/ponto/turnos.py:285-309`). |
| col451 | 2026-09-23 | 718,211750 / 718,211750 | 3/não | 4, par; não | sim/sim/trabalho; 19:00/—/—/07:00; intervalo livre | A=0/0. J=0. **[nao verificado]** | **O — pareamento por posição versus papel:** E6 conta S 07:00→E 18:58; a S da madrugada seguinte fica em outro turno E6 (`app/ponto/calculador/nucleo.py:216-247`; `app/ponto/turnos.py:1382,1402-1419,1491-1498`). |
| col451 | 2026-09-25 | 718,302517 / 718,302517 | 3/não | 4, par; não | sim/sim/trabalho; 19:00/—/—/07:00; intervalo livre | A=0/0. J=0. **[nao verificado]** | **O — pareamento por posição versus papel:** E6 conta S 07:02→E 19:00; a S da madrugada seguinte fica em outro turno E6 (`app/ponto/calculador/nucleo.py:216-247`; `app/ponto/turnos.py:1382,1402-1419,1491-1498`). |
| col478 | 2026-09-25 | 7,766517 / 7,766517 | 3/não | 4, par; não | sim/sim/trabalho; 19:00/—/—/07:00; intervalo livre | A=0/1: atestado aguardando_decisao. J=1: esqueci_saida rejeitada. **[nao verificado]** | **O — pareamento por posição versus papel:** E6 conta E 18:57→E 19:04; a S seguinte fica separada (`app/ponto/calculador/nucleo.py:216-247`; `app/ponto/turnos.py:1382,1417-1419`). |
| col515 | 2026-10-09 | 0 / 0 | 2/não | 6, par; não | sim/sim/trabalho; 19:00/02:00/03:00/07:00 | A=0/4: faltas aprovadas. J=0. **[nao verificado]** | **D — atribuição do dia:** turnos E6 começam em 08 e 10/10; madrugada pode pertencer à véspera (`app/ponto/calculador/nucleo.py:236-247`; `app/ponto/turnos.py:285-309`). |
| col515 | 2026-10-11 | 0 / 0 | 2/não | 6, par; não | sim/sim/trabalho; 19:00/02:00/03:00/07:00 | A=0/4: faltas aprovadas. J=0. **[nao verificado]** | **D — atribuição do dia:** turnos E6 começam em 10 e 12/10; madrugada pode pertencer à véspera (`app/ponto/calculador/nucleo.py:236-247`; `app/ponto/turnos.py:285-309`). |

**Zero ausências confirmadas não significa ausência comprovada de cobertura** nos casos com candidatas: a cobertura dessas candidatas permanece **[nao verificado]**. Sem data final, a regra depende do conjunto vigente de tipos em aberto (`app/ponto/turnos.py:2798-2814`; `app/ponto/catalogo/ausencias.py:430-435`).

A quantidade ímpar de batidas **da janela** não determina a paridade do **dia atribuído pelo E6**: este agrupa por turno e registra a paridade na data da primeira batida (`app/ponto/calculador/nucleo.py:236-245`). Tampouco `corte_que_orfanou=false` no alvo exclui cortes órfãos atribuídos a datas vizinhas (`app/ponto/calculador/nucleo.py:216-223`).

3. Agrupamento dos 19 dias, com contagens derivadas do anexo **[nao verificado]**:

| Causa candidata | Dias | Contagem |
|---|---|---:|
| **P — pausa abaixo do piso e possível desconto da janela declarada.** E6 mede a pausa cronológica; o realizado filtra intervalos abaixo de 15 minutos e pode usar a janela declarada (`app/ponto/calculador/nucleo.py:245-253`; `app/ponto/turnos.py:499,542-546,756-764`). | col252: 23, 24, 25 e 28/09; 01, 02, 06 e 09/10. **[nao verificado]** | **8 [nao verificado]** |
| **D — data inicial do turno E6 versus atribuição da madrugada à jornada da véspera** (`app/ponto/calculador/nucleo.py:236-247`; `app/ponto/turnos.py:285-309`). | col218: 15/10; col37: 05 e 09/10; col382: 30/09; col515: 09 e 11/10. **[nao verificado]** | **6 [nao verificado]** |
| **O — pares cronológicos versus papéis/tipos do pareador** (`app/ponto/calculador/nucleo.py:245-247`; `app/ponto/turnos.py:1382,1417-1419,1491-1498`). | col451: 23 e 25/09; col478: 25/09. **[nao verificado]** | **3 [nao verificado]** |
| **Não nomeada:** faltam turnos E6 mensais em col129; papéis/turnos efetivos e fonte do espelho em col369. **[nao verificado]** | col129 e col369: 12/10. **[nao verificado]** | **2 [nao verificado]** |
| Total | **8+6+3+2 [nao verificado]** | **19 [nao verificado]** |

Em col369, a célula de folga isoladamente não explica a divergência: o realizado seleciona turnos por `data_turno`, sem excluir folga nesse filtro (`app/ponto/turnos.py:719-732`). Em col478, E/E isoladamente não autoriza trocar o dono para BATIDA: esse caminho exige realizado aberto e entrada real solta (`app/ponto/management/commands/e6_oraculo.py:198-212`).

## 16/11 08:15 · sessao 0c083aab5ae1 · - · - · origem=Ronald (tmux)

Causas candidatas para os 10 dias examinados; todas dependem de confirmação do caminho efetivamente usado pelo espelho, cujos minutos permanecem [nao verificado].

| Grupo | Dias | Mecanismo e evidência de código | Fato faltante |
|---|---|---|---|
| Atribuição do dia: calendário E6 versus jornada noturna | **6**: col218 15/10; col37 05 e 09/10; col382 30/09; col515 09 e 11/10 | E6 usa a data da primeira batida do turno (`app/ponto/calculador/nucleo.py:236-247`). A autoridade dos turnos pode atribuir entradas/saídas da madrugada à véspera, conforme o DNA (`app/ponto/turnos.py:285-309,312-324,1253-1302`). | `data_turno`, papel e turnos efetivos do espelho [nao verificado]. |
| Pareamento por posição versus tipos/papéis | **3**: col451 23 e 25/09; col478 25/09 | E6 soma posições consecutivas sem consultar o tipo (`app/ponto/calculador/nucleo.py:236-247`). No dado, col451 reúne S matinal com E vespertina; col478 reúne duas E próximas, separando a S da madrugada seguinte [nao verificado: apenas dado fornecido]. O pareador real usa papel da ATA ou tipo e distingue E que abre e S que fecha (`app/ponto/turnos.py:1377-1419,1491-1498`). | Papéis efetivos, estado aberto e pareamento produzido [nao verificado]. |
| Dependência do contexto da leitura E6 | **1**: col129 12/10 | O acumulador `cur` atravessa os instantes até ocorrer corte; composição e paridade dependem das batidas anteriores (`app/ponto/calculador/nucleo.py:157,186-228`). Mensal 779,1582679 versus janela 659,4270846666666 no dado [nao verificado: apenas dado fornecido]. | Turnos E6 produzidos na leitura mensal; sem eles, o deslocamento exato não pode ser explicado [nao verificado]. |

`corte_que_orfanou=false` para o alvo não afirma que todos os turnos vizinhos sejam pares: o marcador usa a data inicial do turno cortado (`app/ponto/calculador/nucleo.py:216-223`).

E/E não autoriza reclassificar col478 como BATIDA: o juiz precisa de `realizado.aberto` e entrada real solta no turno do dia (`app/ponto/management/commands/e6_oraculo.py:198-212`). A classificação ESTRUTURA continua sendo a premissa fornecida [nao verificado: apenas dado fornecido].

Nenhum AGENTS.md apareceu na busca da árvore nem nos ancestrais consultados [nao verificado: resultado da inspeção, sem arquivo citável].

## 16/11 08:15 · sessao 892fac3ba71d · - · - · origem=Ronald (tmux)

Para **col252 (8 dias)**, causa candidata sustentada: **pausa cronológica abaixo de 15 minutos no E6 versus piso de intervalo e possível desconto da janela declarada no realizado**.

- E6 soma pares sequenciais e mede o vão entre eles sem piso: `app/ponto/calculador/nucleo.py:245-253`.
- O realizado reconhece apenas intervalos marcados de pelo menos 15 minutos: `app/ponto/turnos.py:499,542-546`.
- Sem intervalo reconhecido, se `livre=False`, pode descontar a interseção da janela declarada com o turno: `app/ponto/turnos.py:756-764`.
- O espelho fornece `livre` e o intervalo do dia ao realizado: `app/ponto/services/espelho.py:286-294`.
- Nos oito dias fornecidos, as pausas E6 são inferiores a 15 minutos e a janela declarada é 12:38–13:38; execução desse ramo e desconto efetivo permanecem **[nao verificado]**.

Para **col369 em 12/10**, **folga não constitui causa suficiente**:

- O realizado filtra por `data_turno`, sem eliminar dias de folga: `app/ponto/turnos.py:719-732`.
- Célula existente sem marcos declara ausência de pausa: `app/escala/models.py:1189-1207`.
- E6 soma pela posição cronológica, enquanto o pareador consulta papel da ata e depois o tipo gravado: `app/ponto/calculador/nucleo.py:245-253`; `app/ponto/turnos.py:1310-1325,1377-1382`.
- Candidata ampla: **pareamento cronológico versus pareamento por papel/tipo**. Para uma causa específica, usar **“não nomeada: faltam os papéis efetivos da ata, os turnos do juiz e a fonte real do número do espelho”**. O leitor prioriza `minutos_realizados` e depois `pago_h`: `app/ponto/management/commands/e6_oraculo.py:263-269`.

Nenhuma regra encontrada exige atribuir divergência ao fato isolado de a célula ser folga ou ter sido regenerada; essa atribuição permanece **[nao verificado]**.

## 16/11 08:13 · sessao 0f7a28ff15fc · RESPOSTAS-DA-MANHA-1611 · verificador · origem=Code

| item | pergunta | PROCEDE/NAO PROCEDE | arquivo:linha | motivo |
|---|---|---|---|---|
| 1 | 1 — Executável no literal? | **PROCEDE** | anexo:respostas_da_manha_1611.txt:2; anexo:ressusc_diff.txt:466-474; app/docs/RELATO.md:3 | A comprovação literal não fecha: o anexo é uma **saída de diff**, sem instrumento executável, hashes das árvores ou identificação do retrato. Mostra 97 mudanças de ação/dono, 105 somente de rótulo e 81 iguais na saída `>`; não evidencia `total_erros=0`, vitalidade ou retenção. Refazer B1 sobre o HEAD atual não fixa automaticamente a referência da comparação. Qual main produziu esse diff e se continua sendo a referência: **[nao verificado]**. Isso não demonstra impossibilidade da cura. |
| 1 | 2 — Falta “senão”? | **NAO PROCEDE** | app/docs/LEIS.md:62; anexo:cauda_da_madrugada_1611.txt:1; anexo:respostas_da_manha_1611.txt:2 | Existe saída escrita: condição não satisfeita = parar com o número; registrar motivo e o que destrava, seguindo para o próximo item. Se não alcançar 283 iguais dentro das proibições, B1 não pousa. A autorização não permite reduzir a meta nem criar decisão ou juiz para contorná-la. |
| 1 | 3 — Colisão com lei, aval ou obra em voo? | **NAO PROCEDE** | anexo:construir_r3_c17_e_f1_nova.txt:1; anexo:producao_maxima.txt:1; anexo:retoma_he_revogada_e_lote3.txt:1; anexo:prepara_volta_bug_par.txt:4; anexo:cauda_da_madrugada_1611.txt:1 | A preservação de fila já integra B1. Os avais posteriores mantêm teto **2 de construtores**; o teto **3** do RETOMA é da pista, e PRODUCAO-MAXIMA é anterior. Voltas de R3/F1 conservam precedência; BUG-PAR continua condicionada e B2 depende do pouso B1. Colisão efetiva de arquivos entre as três cópias ausentes, seus HEADs e ocupação atual das vagas: **[nao verificado]**; não há liberação comprovada por ausência de cruzamento. | [nao verificado]
| 1 | 4 — Referências, unidade e universo conferem? | **PROCEDE** | app/chamados/models.py:1610-1622; app/chamados/services/tarefa_do_dia.py:152-177; app/docs/RELATO.md:7; anexo:ressusc_diff.txt:466-474 | No archive, `models.py:1621` percorre disputas fechadas; não registra `REABERTURA`. `tarefa_do_dia.py:152` chama `chamado_que_lista`; a devolução por `REABERTURA` está em **163-164**, e silêncio sem ação em **176-177**. O RELATO atribui as referências à cópia B1: conferência nela **[nao verificado]**. A unidade pergunta e os agregados 97/105/283 aparecem no diff, mas ele não contém a lista completa das 283 nem prova individualmente as 81 omitidas. |
| 2 | 1 — Executável no literal? | **NAO PROCEDE** | anexo:respostas_da_manha_1611.txt:3; anexo:opus_final.md:3; app/colaboradores/views.py:324-332 | O laudo identifica exatamente `test_lote3_c6_recusa_nada_gravado.py:230-239`, com o teste `test_MUTANTE_gravou_e_a_trilha_falhou_linha_mostra_o_novo`. Segundo ele, o teste atual **exige o defeito**; fazê-lo morder exige inverter a expectativa para posto antigo no banco **e na linha** após falha da trilha, junto do `atomic` comum. O código disponível confirma as gravações separadas. |
| 2 | 2 — Falta “senão”? | **NAO PROCEDE** | app/docs/LEIS.md:62; anexo:cauda_da_madrugada_1611.txt:1; anexo:respostas_da_manha_1611.txt:3 | Se a cura ou o teste não satisfizerem o contrato, a obra para com motivo e condição de desbloqueio, e a fila segue. Mesmo com a cura aprovada, o pouso continua esperando o `!` de vínculo. |
| 2 | 3 — Colisão com lei, aval ou obra em voo? | **NAO PROCEDE** | anexo:retoma_he_revogada_e_lote3.txt:1; anexo:respostas_da_manha_1611.txt:3; anexo:opus_final.md:3; anexo:cauda_da_madrugada_1611.txt:1; app/docs/LEIS.md:65 | RETOMA dizia literalmente achado procedente **do pre-verificador**; o aval novo aceita expressamente o achado do **opus final** para esta volta. Essa autorização específica cobre o bloqueio de atomicidade/trilha e conserva o `!`; “entra” não altera expressamente a ordem LOTE7 antes de LOTE3. Arquivos comuns e possibilidade de execução simultânea nas cópias B1/c7/c6: **[nao verificado]**. |
| 2 | 4 — Referências, unidade e universo conferem? | **NAO PROCEDE** | anexo:respostas_da_manha_1611.txt:1,3; anexo:opus_final.md:1,3; app/colaboradores/views.py:329-332,1387-1403; app/colaboradores/services/historico_vinculo.py:7-17 | A unidade obra confere. `vincular_posto:329-332` corresponde às duas gravações sem bloco comum; `registrar_mudanca` cria `HistoricoVinculo`. `transferir_posto` já engloba escrita e histórico no `atomic` iniciado em **1387**; **1400-1403** são argumentos/final da chamada dentro dele. Arquivo, linhas e nome do teste conferem **no laudo**. Sua presença e conteúdo na cópia c6, e o resultado futuro da c7: **[nao verificado]**. |

**AVAL-DEVOLVIDO: RESPOSTAS-DA-MANHA-1611 | item 1**, pelos PROCEDE acima, conforme anexo:revisao_do_aval.txt:6.

## 16/11 08:12 · sessao 77d4b90b75c4 · ESPELHO-VERDADE-FECHO-V0 · sonda · origem=Code

A saída integral da **ESPELHO-VERDADE-FECHO V0 r2**, com os 55 dias ESTRUTURA, não está disponível no material recebido; não consigo repassá-la sem inventar linhas. [nao verificado]

O trecho fornecido contém registros `DET|...` e termina em `DET|col252|2026-11-10|BATIDA|...`, sem apresentar a linha `TOTAL` nem os resultados `OK`/`ERRO` da r2. [nao verificado]

| Conferência solicitada | Resultado |
|---|---|
| `TOTAL` tem `dias=55`? | [nao verificado] |
| `ok+erro=55`? | [nao verificado] |
| Quantos `ERRO` por classe? | [nao verificado] |
| Há marcador `INCOMPLETO` na saída integral? | [nao verificado] |

Os campos `"estado":"nao lido: grava"` e `"[nao verificado]"` dos registros recebidos não permitem determinar o resultado da r2. [nao verificado]

Falta fornecer a saída da r2 até seu encerramento, incluindo os 55 resultados, os erros e a linha `TOTAL`. [nao verificado]

## 16/11 03:25 · sessao 9f08f9b53053 · ESPELHO-VERDADE-FECHO-V0 · leitura · origem=Code

```python
# OBRA: ESPELHO-VERDADE-FECHO-V0
# Arvore solicitada: 3580b35e028267d85318cb75fd5b3d0c40e04a00.
# Texto para o envelope ja inicializado em SOMBRA/juliani, como o anexo.
# Execucao, desempenho e compatibilidade deste script: [nao verificado].
# Funcoes auxiliares deste script, biblioteca padrao e implementacoes internas
# do Django fora da copia: [nao verificado].
#
# Restricao relevante:
# espelho_do_colab -> autoridade_do_periodo -> efeitos_da_ausencia ->
# parciais_vigentes -> vigente escreve cache:
# app/ponto/services/espelho.py:1227
# app/ponto/services/espelho.py:1068
# app/ponto/services/efeito_ausencia.py:38
# app/ponto/catalogo/ausencias.py:357
# app/ponto/catalogo/ausencias.py:279-280
#
# Portanto, NAO chamar espelho_do_colab. Tambem NAO fabricar o dict que
# minutos_do_espelho receberia: {} significaria indevidamente "sem lavratura".
# Leitor real, puro, mas sem entrada real disponivel por caminho autorizado:
# app/ponto/management/commands/e6_oraculo.py:242-269
#
# A pausa efetivamente descontada tambem fica bloqueada:
# seu carregador de turnos preenche caches locais:
# app/ponto/turnos.py:1906
# app/ponto/turnos.py:1917
# app/ponto/turnos.py:1163
# Funcao real do espelho que dependeria desses turnos:
# app/ponto/services/espelho.py:267-294
# Nao substituir esses turnos pelos do E6.
#
# "ok" conta dias processados sem excecao; fatos bloqueados continuam
# explicitamente "nao lido: grava" / [nao verificado].

import sys

sys.dont_write_bytecode = True

import datetime as dt
import json
import logging
import re
from contextlib import redirect_stderr, redirect_stdout
from unittest.mock import patch

DADO = """col129|10/2026|2026-10-12
col218|10/2026|2026-10-15
col252|10/2026|2026-09-23
col252|10/2026|2026-09-24
col252|10/2026|2026-09-25
col252|10/2026|2026-09-28
col252|10/2026|2026-10-01
col252|10/2026|2026-10-02
col252|10/2026|2026-10-06
col252|10/2026|2026-10-09
col369|10/2026|2026-10-12
col37|10/2026|2026-10-05
col37|10/2026|2026-10-09
col382|10/2026|2026-09-30
col451|10/2026|2026-09-23
col451|10/2026|2026-09-25
col478|10/2026|2026-09-25
col515|10/2026|2026-10-09
col515|10/2026|2026-10-11
col723|10/2026|2026-10-08
col746|10/2026|2026-09-24
col841|10/2026|2026-10-20
col854|10/2026|2026-10-04
col864|10/2026|2026-10-12
col868|10/2026|2026-09-22
col876|10/2026|2026-09-21
col110|11/2026|2026-11-12
col112|11/2026|2026-10-21
col112|11/2026|2026-10-22
col113|11/2026|2026-10-30
col113|11/2026|2026-11-01
col114|11/2026|2026-11-08
col114|11/2026|2026-11-15
col130|11/2026|2026-10-29
col200|11/2026|2026-10-29
col200|11/2026|2026-11-02
col218|11/2026|2026-11-14
col220|11/2026|2026-11-10
col252|11/2026|2026-11-09
col252|11/2026|2026-11-10
col252|11/2026|2026-11-11
col252|11/2026|2026-11-13
col296|11/2026|2026-11-02
col37|11/2026|2026-11-14
col382|11/2026|2026-11-10
col516|11/2026|2026-10-23
col516|11/2026|2026-10-28
col727|11/2026|2026-11-10
col874|11/2026|2026-10-22
col884|11/2026|2026-11-08
col922|11/2026|2026-11-01
col922|11/2026|2026-11-04
col989|11/2026|2026-10-23
col996|11/2026|2026-10-28
col996|11/2026|2026-11-10"""

NV = "[nao verificado]"
UNIVERSO = [tuple(linha.split("|")) for linha in DADO.splitlines()]
if len(UNIVERSO) != 55 or len(set(UNIVERSO)) != 55:
    raise RuntimeError("UniversoIncompativel")

SAIDA = sys.stdout


def imprimir(*partes):
    print(*partes, file=SAIDA, flush=True)


class Descartar:
    def write(self, texto):
        return len(texto)

    def flush(self):
        return None


def consumir(qs):
    # Iterator evita preencher o cache de resultados do QuerySet.
    # Implementacao Django: [nao verificado].
    it = qs.iterator(chunk_size=128)
    try:
        return list(it)
    finally:
        it.close()


def unico(qs):
    objetos = consumir(qs)
    if len(objetos) != 1:
        raise LookupError("QuantidadeIncompativel")
    return objetos[0]


def classe_segura(exc):
    nome = type(exc).__name__
    return nome if re.fullmatch(r"[A-Za-z_][A-Za-z0-9_]{0,79}", nome) else "Excecao"


def sonda():
    ok = erro = 0
    feitos = set()

    try:
        with (
            patch.object(logging.Logger, "handle", lambda *a, **k: None),
            redirect_stdout(Descartar()),
            redirect_stderr(Descartar()),
        ):
            from django.apps import apps
            from django.conf import settings
            from django.db import connection, transaction
            from django.db.models import Q
            from django.utils import timezone

            if (
                not apps.ready
                or settings.SETTINGS_MODULE != "config.settings.sombra"
                or not settings.BANCO_DE_ENSAIO
                or connection.settings_dict["NAME"] != "sombra"
                or connection.schema_name != "juliani"
            ):
                raise RuntimeError("AmbienteIncompativel")

            # Configuracao SOMBRA: app/config/settings/sombra.py:49-50,65.
            # Envelope/tenant/papel real nesta execucao: [nao verificado].
            from ponto.janelas import janela_fechamento
            from ponto.management.commands.e6_oraculo import minutos_do_oraculo
            from ponto.turnos import batidas_apuraveis, dias_da_ausencia

            # Modelos consultados; nenhum __str__ e chamado:
            # app/colaboradores/models.py:283
            # app/colaboradores/models.py:40
            # app/escala/models.py:1635
            # app/escala/models.py:871
            # app/ponto/models.py:660
            # app/ponto/models.py:115
            Colaborador = apps.get_model("colaboradores", "Colaborador")
            Empresa = apps.get_model("colaboradores", "Empresa")
            CelulaDia = apps.get_model("escala", "CelulaDia")
            EscalaColaborador = apps.get_model("escala", "EscalaColaborador")
            Ausencia = apps.get_model("ponto", "Ausencia")
            Justificativa = apps.get_model("ponto", "Justificativa")

            def local(valor):
                # timezone.localtime: implementacao Django [nao verificado].
                # Mesmo uso do E6:
                # app/ponto/management/commands/e6_oraculo.py:152.
                return None if valor is None else timezone.localtime(valor).isoformat()

            def meia_noite(data):
                # Mesmo timezone/make_aware do carregamento E6:
                # app/ponto/management/commands/e6_oraculo.py:149-151.
                return timezone.make_aware(
                    dt.datetime.combine(data, dt.time.min),
                    timezone.get_current_timezone(),
                )

            def oraculo_com_turnos(instantes):
                if sys.getprofile() is not None:
                    raise RuntimeError("PerfilPreexistente")
                capturados = []

                def observar(frame, evento, arg):
                    if (
                        evento == "return"
                        and frame.f_code is minutos_do_oraculo.__code__
                        and arg is not None
                    ):
                        # Le a variavel REAL; nao replica o corte/pareamento.
                        # app/ponto/calculador/nucleo.py:157,224,228.
                        capturados.append(
                            tuple(tuple(t) for t in frame.f_locals["turnos"])
                        )

                sys.setprofile(observar)
                try:
                    # Alias real do comando:
                    # app/ponto/management/commands/e6_oraculo.py:42.
                    # Funcao real, sem pares/envelopes:
                    # app/ponto/calculador/nucleo.py:42-254.
                    resultado = minutos_do_oraculo(instantes)
                finally:
                    sys.setprofile(None)

                if len(capturados) != 1:
                    raise RuntimeError("TurnosDoOraculoNaoCapturados")
                return resultado, capturados[0]

            def ler_dia(codigo, competencia, texto_dia):
                cid = int(codigo[3:])
                dia = dt.date.fromisoformat(texto_dia)
                mes, ano = map(int, competencia.split("/"))
                linhas = []

                def fato(nome, ref, **campos):
                    campos["ref"] = ref
                    linhas.append(
                        "DET|%s|%s|%s|%s" % (
                            codigo, texto_dia, nome,
                            json.dumps(
                                campos, ensure_ascii=True, allow_nan=False,
                                separators=(",", ":"),
                            ),
                        )
                    )

                c = unico(
                    Colaborador.objects.filter(pk=cid).only("pk", "empresa_id")
                )
                empresa = unico(
                    Empresa.objects.filter(pk=c.empresa_id).only(
                        "pk", "dia_inicio_competencia"
                    )
                )

                # Funcao real: app/ponto/janelas.py:54-57.
                ini, fim = janela_fechamento(mes, ano, empresa)
                if not ini <= dia <= fim:
                    raise RuntimeError("DiaForaDaCompetencia")

                # Mesma alimentacao mensal do E6, inclusive os limites:
                # app/ponto/management/commands/e6_oraculo.py:149-153.
                tz = timezone.get_current_timezone()
                lo = timezone.make_aware(
                    dt.datetime.combine(ini, dt.time.min)
                    - dt.timedelta(hours=12), tz,
                )
                hi = timezone.make_aware(
                    dt.datetime.combine(fim + dt.timedelta(days=1), dt.time.min)
                    + dt.timedelta(hours=14), tz,
                )

                # Funcao real: app/ponto/turnos.py:2868-2895.
                ts_mes = [
                    timezone.localtime(ts)
                    for ts in consumir(
                        batidas_apuraveis(c, lo, hi).values_list(
                            "timestamp", flat=True
                        )
                    )
                ]
                mensal, _turnos_mensais = oraculo_com_turnos(ts_mes)
                chave = dia.isoformat()
                fato(
                    "E6",
                    "app/ponto/calculador/nucleo.py:42-254;"
                    "app/ponto/management/commands/e6_oraculo.py:149-153,350",
                    competencia=competencia,
                    minutos=mensal.minutos.get(chave, 0.0),
                    envelope_min=mensal.envelope.get(chave, 0.0),
                    pausa_min=mensal.pausas.get(chave, 0.0),
                    impar=chave in mensal.impares,
                    corte_que_orfanou=chave in mensal.cortes,
                    leitura_ini=local(lo),
                    leitura_fim_inclusivo=local(hi),
                )

                # Tres dias civis completos, no horario local.
                anterior = dia - dt.timedelta(days=1)
                seguinte = dia + dt.timedelta(days=1)
                a = meia_noite(anterior)
                z = meia_noite(dia + dt.timedelta(days=2))
                # batidas_apuraveis usa <= no limite superior:
                # app/ponto/turnos.py:2891-2892.
                batidas = consumir(
                    batidas_apuraveis(c, a, z - dt.timedelta(microseconds=1))
                )
                janela, turnos = oraculo_com_turnos(
                    [timezone.localtime(b.timestamp) for b in batidas]
                )
                fato(
                    "E6_JANELA",
                    "app/ponto/calculador/nucleo.py:216-254",
                    janela_ini=anterior.isoformat(),
                    janela_fim=seguinte.isoformat(),
                    minutos=janela.minutos.get(chave, 0.0),
                    turnos=len(turnos),
                    cortes=max(0, len(turnos) - 1),
                    corte_que_orfanou=chave in janela.cortes,
                    contexto="tres_dias; numero_mensal_publicado_em_E6",
                )
                for i, turno in enumerate(turnos):
                    fato(
                        "E6_TURNO",
                        "app/ponto/calculador/nucleo.py:216-228,236-247",
                        indice=i,
                        data_inicio=turno[0].date().isoformat(),
                        batidas=[local(ts) for ts in turno],
                        # Append intermediario e corte; ultimo append fecha
                        # apenas a leitura: nucleo.py:224,228.
                        corte_apos=i < len(turnos) - 1,
                    )

                fato(
                    "BATIDAS_RESUMO",
                    "app/ponto/turnos.py:2868-2895",
                    quantidade=len(batidas),
                    janela_ini=local(a),
                    janela_fim_exclusivo=local(z),
                )
                for b in batidas:
                    fato(
                        "BATIDA",
                        "app/ponto/models.py:8-79;"
                        "app/ponto/turnos.py:1310-1315",
                        id=b.pk,
                        hora_local=local(b.timestamp),
                        tipo=b.tipo,
                        is_intervalo_gravado=b.is_intervalo,
                        # Esses atributos nao sao colunas de Batida.
                        # Nao inventar papel a partir de tipo/is_intervalo.
                        papel=b.__dict__.get("_papel_ata", NV),
                        data_turno=(
                            b.__dict__["data_turno"].isoformat()
                            if b.__dict__.get("data_turno") is not None
                            else NV
                        ),
                        retratada_em=local(b.retratada_em),
                    )

                celulas = consumir(
                    CelulaDia.objects.filter(colaborador_id=cid, data=dia)
                )
                if len(celulas) > 1:
                    raise RuntimeError("CelulaDuplicada")
                cel = celulas[0] if celulas else None

                if cel is None:
                    fato(
                        "CELULA",
                        "app/escala/models.py:1681-1688",
                        existe=False,
                    )
                else:
                    dna = cel.dna or {}
                    ata = cel.ata or {}
                    marcos = dna.get("marcos")
                    # Somente horarios do vocabulario da entidade;
                    # nao imprimir o JSON inteiro nem textos livres.
                    # Funcao real:
                    # app/escala/models.py:1659-1669.
                    chaves = [k for _tipo, k in CelulaDia.chaves_do_turno()]
                    m = (
                        None if marcos is None else {
                            k: (
                                None if marcos[k] is None
                                else dt.time.fromisoformat(marcos[k]).isoformat()
                            )
                            for k in chaves if k in marcos
                        }
                    )
                    fato(
                        "CELULA",
                        "app/escala/models.py:1681-1691,1697-1701,1718,1778;"
                        "app/ponto/management/commands/gerar_celulas.py:104-111",
                        existe=True,
                        id=cel.pk,
                        trabalha=cel.trabalha,
                        tipo_ata=ata.get("tipo_dia", NV),
                        tipo_ciclo_dna=dna.get("tipo_ciclo", NV),
                        tipo_escala_id_dna=dna.get("tipo_escala_id", NV),
                        marcos=m,
                        janela_intervalo_dna=(
                            None if m is None else {
                                "%s/%s" % (s, e): [m.get(s), m.get(e)]
                                for s, e in CelulaDia.PARES_PAUSA_DNA
                                if s in m or e in m
                            }
                        ),
                        intervalo_livre_dna=dna.get("intervalo_livre", NV),
                        origem=cel.origem,
                        regenerada_em=local(cel.regenerada_em),
                    )

                    if cel.escala_geradora_id is not None:
                        ec = unico(
                            EscalaColaborador.objects.filter(
                                pk=cel.escala_geradora_id,
                                colaborador_id=cid,
                            )
                        )
                        # Funcao REAL com celula fornecida: nao cai na leitura
                        # do template nem em _celulas_do_ato_para.
                        # app/escala/models.py:1135-1164,1182-1207.
                        declarado = ec.intervalo_do_dia(
                            dia, celulas={(cid, dia): cel}
                        )
                        fato(
                            "INTERVALO_DECLARADO_FUNCAO_REAL",
                            "app/escala/models.py:1135-1164,1182-1207",
                            inicio=(
                                declarado[0].isoformat()
                                if declarado[0] is not None else None
                            ),
                            fim=(
                                declarado[1].isoformat()
                                if declarado[1] is not None else None
                            ),
                            desconto_efetivo_espelho=NV,
                        )

                fato(
                    "ESPELHO",
                    "app/ponto/management/commands/e6_oraculo.py:242-269;"
                    "app/ponto/services/espelho.py:1227,1068;"
                    "app/ponto/services/efeito_ausencia.py:38;"
                    "app/ponto/catalogo/ausencias.py:357,279-280",
                    estado="nao lido: grava",
                    minutos=NV,
                    fonte=NV,
                    funcao_bloqueada="espelho_do_colab",
                    leitor_nao_chamado_por_falta_de_linha_real="minutos_do_espelho",
                )
                fato(
                    "PAUSA_RECONHECIDA",
                    "app/ponto/turnos.py:1906,1917,1163;"
                    "app/ponto/services/espelho.py:267-294",
                    estado="nao lido: grava",
                    pausas=NV,
                )
                fato(
                    "INTERVALO_EFETIVO_ESPELHO",
                    "app/ponto/services/espelho.py:817-819;"
                    "app/ponto/turnos.py:756-771,1906,1917,1163",
                    estado="nao lido: grava",
                    minutos=NV,
                )

                # Nao chamar ausencias_sobrepostas/_q_cobertura:
                # o conjunto TIPOS_EM_ABERTO consulta vigente(), que grava.
                # app/ponto/turnos.py:2773-2774,2843-2847
                # app/ponto/catalogo/ausencias.py:403-407,430-435,279-280.
                candidatas = consumir(
                    Ausencia.objects.filter(
                        colaborador_id=cid, data__lte=dia,
                    ).filter(
                        Q(data_fim__gte=dia) | Q(data_fim__isnull=True)
                    ).order_by("data", "pk")
                )
                confirmadas = pendentes = 0
                for ausencia in candidatas:
                    if ausencia.data_fim is not None:
                        # Funcao REAL; neste ramo nao consulta o catalogo:
                        # app/ponto/turnos.py:2798-2814, especificamente :2803-2804.
                        cobre = dia in dias_da_ausencia(
                            ausencia.tipo, ausencia.data, ausencia.data_fim,
                            dia, dia,
                        )
                    elif ausencia.data == dia:
                        # O dia inicial pertence ao intervalo em ambos os
                        # ramos sem data_fim: turnos.py:2805-2809.
                        cobre = True
       

[truncado em 20000 de 24347 caracteres]

## 16/11 03:23 · sessao 9f5d91a62d6d · LOTE3-C6 · censo · origem=Code

Referências `arquivo:linha` dos anexos contam as linhas do texto colado, incluindo linhas vazias, sem contar o cabeçalho `== anexo: ... ==`.

**As classes fornecidas não cobrem todos os estados documentados:** há achado visto e mantido aberto, além de achados vistos sem descarte documentado. Por isso, não atribuo **(b)** automaticamente a tudo que ficou sem cura. (`pv_c6.r:25`; `pv_c6_v3.r:23`; `entrega.md:37`.)

No item **2**, separo o residual já identificado do caminho adicional via `engolir`: o primeiro foi visto; o segundo não aparece nas explicações das respostas, embora rollback e `post_save` estivessem perguntados. (`pv_c6.q:6-7`; `pv_c6_v2.r:9-14`; `pv_c6_v3.r:7-10`.)

| achado | classe | trecho .q | trecho .r |
|---|---|---|---|
| **1 — Posto gravado sem histórico; teste conserva o defeito** | **Visto; (b) não comprovada.** A triagem registra “mutantes (2) verdes nos dois lados, por desenho”, mas o descarte específico da atomicidade posto+histórico é **[nao verificado]**. (`entrega.md:24`.) | “ha caminho em que a recusa ainda deixa algo gravado”; “selo/RED sem mordida”; “pouso antes do pre-requisito”. (`pv_c6.q:6`; `pv_c6.q:9`.) | “Falha do histórico após gravação continua deixando o posto novo persistido”; teste 5 “Caracteriza a gravação consumada” e “Não precisa reprovar o comportamento anterior”. O Codex viu e aceitou essa caracterização. (`pv_c6.r:25`; `pv_c6.r:70`.) | [nao verificado]
| **2 — `_reabrir` deixa anexo órfão; caminho extra pelo cartório/`engolir`** | **Residual principal: visto e mantido aberto — sem classe correspondente. Caminho extra: (a).** A triagem diz “ABERTO” e remete a cura ao LOTE 7; isso documenta limite de intervenção, sem tornar o diagnóstico estático fora do alcance. (`entrega.md:37`; `pv_c6_v3.q:1`.) | “Ausencia (nova ou reaberta por _reabrir) e o arquivo escrito neste ato fica no storage”; “que caminhos ainda deixam arquivo […] (inclusive _reabrir)”; inclui efeitos do `post_save`. (`pv_c6_v2.q:3`; `pv_c6_v3.q:4`; `pv_c6.q:6-7`.) | “PROCEDENTE — falha intermediária em `_reabrir` deixa o upload órfão”; “Permanece 1 PROCEDENTE”. A explicação cobre documento → descrição/status → rollback, sem identificar a rota de envenenamento via `engolir`. (`pv_c6_v2.r:9-14`; `pv_c6_v3.r:1`; `pv_c6_v3.r:7-10`.) |
| **3 — Ordem global composta / ContadorPauta** | **(b).** Descarte explícito: “DESCARTADO como cura: ja existia dentro do atomic de `a.save()` […] vai ANEXO ao opus”. (`entrega.md:31`.) | “Ela respeita a ordem global […]? Ha select_for_update fora dessa porta no caminho?” (`pv_c6.q:7`.) | “Observação — ordem global composta”; “existem travas adicionais de contador/competência”. (`pv_c6.r:90`.) |
| **4 — Comentário velho versus `_A14 CURADO`** | **(b).** Descarte explícito: “texto pre-existente movido, nao reescrito; fila”. (`entrega.md:31`.) | Checklist de “derivacao paralela de fonte unica” e confronto com `JUIZES.md`: o tema da autoridade registrada estava presente, embora o comentário não fosse nomeado. (`pv_c6.q:9-10`.) | “Observação — comentário desatualizado: ‘lei em aberto’ diverge da autorização já registrada no vivo.” (`pv_c6.r:91`.) |
| **5 — Rótulo do padrão nos commits / L-123** | **(e) NAO PERGUNTADO.** Os questionários não solicitam conferir o padrão declarado nos commits contra o realizado pela cura. (`pv_c6.q:6-11`; `pv_c6_v2.q:3-8`; `pv_c6_v3.q:3-7`.) | Não há trecho correspondente; os checklists enumerados não incluem essa conferência. (`pv_c6.q:9`; `pv_c6_v2.q:7`; `pv_c6_v3.q:6`.) | Avaliação específica desse rótulo: **[nao verificado]**. A declaração “c6 = Policy Enforcement Point” pertence à entrega, não comprova que tenha sido perguntada ou verificada. (`entrega.md:41`.) | [nao verificado]
| **6 — Reversor sem reperguntar cobertura sob trava** | **Visto; descarte pela triagem [nao verificado].** Não cabe classificá-lo como omissão do Codex: a corrida está explicitamente descrita na resposta. (`pv_c6_v3.r:23`.) | “Checklist: […] 2o escritor”; também “QUEM ESCREVE […] Colaborador.situacao”. (`pv_c6_v3.q:6`; `pv_c6.q:10`.) | “permanece o reversor, que seleciona antes de chamar `gravar`, sem revalidar a cobertura sob a trava. É uma corrida possível na cópia”. Ocorrência em produção permanece **[nao verificado]**. (`pv_c6_v3.r:23`.) |

## 16/11 03:21 · sessao 0f8d2d7e0419 · - · - · origem=Ronald (tmux)

Sob interpretação literal incluindo caches locais, pule a cadeia de turnos do espelho: `turnos_do_colab` preenche `_ub_cache` e `_mpd_cache` (`app/ponto/turnos.py:1906`, `app/ponto/turnos.py:1917`); seu pareador chama `_fechar_aberto_na_pausa_sem_volta`, que preenche `_ult_cache` (`app/ponto/turnos.py:1502`, `app/ponto/turnos.py:1163`).

Emita `PAUSA_RECONHECIDA|nao lido: grava` e `INTERVALO_EFETIVO_ESPELHO|nao lido: grava`. Pode complementar com o intervalo **declarado**, chamando `vinculo_do_dia` (`app/escala/alimentacao.py:228`) e `ec.intervalo_do_dia(data, celulas=...)` (`app/escala/models.py:1135`); isso não demonstra o desconto aplicado (`app/ponto/turnos.py:756-771`).

`_realizado_do_dia_tela` pode receber turnos já julgados sem chamar seu carregador (`app/ponto/services/espelho.py:267-294`), mas não há fonte equivalente sem a cadeia encontrada: equivalência de turnos E6 ou objetos montados pela sonda é `[nao verificado]`.

## 16/11 03:19 · sessao df82b3e41f69 · - · - · origem=Ronald (tmux)

Funções reais seguras para leitura:

- `minutos_do_espelho(de)` retorna `(minutos, fonte)` exatamente pela precedência solicitada; não grava. `app/ponto/management/commands/e6_oraculo.py:242`, decisão em `:263-269`.
- `InsumosDoDono(c, ini, fim, hoje, agora=agora)` calcula o E6 com o universo mensal real: `[ini − 12h, fim + 1d + 14h]`, convertendo timestamps por `timezone.localtime`. O construtor somente lê e preenche a instância; acessar `.oraculo` não dispara outras funções. `app/ponto/management/commands/e6_oraculo.py:128-154`.
- O número comparado pelo comando é `ins.oraculo.minutos.get(str(dia), 0.0)`. `app/ponto/management/commands/e6_oraculo.py:350`.
- A competência usa `janela_fechamento(mes, ano, c.empresa)`. `app/ponto/management/commands/e6_oraculo.py:304`; implementação em `app/ponto/janelas.py:54-57`.
- `minutos_do_oraculo(ts)` é a derivação real, sem escrita externa, importando somente biblioteca padrão. `app/ponto/calculador/nucleo.py:22-25`, `:42-254`.

Para mostrar os turnos efetivamente montados, sem copiar regra, capture `frame.f_locals["turnos"]` no evento `return` de um `sys.setprofile` temporário, restringindo por `frame.f_code is minutos_do_oraculo.__code__`, durante a construção de `InsumosDoDono`; restaure o profiler anterior em `finally`. A variável real recebe os turnos em `app/ponto/calculador/nucleo.py:157`, `:224`, `:228`; não integra o retorno `Veredito`, cujo contrato está em `:28-37`.

Distinga:

- `corte_apos=True` em cada turno capturado exceto o último: os anteriores foram adicionados pelo ramo de corte; o último pelo fechamento da leitura. `app/ponto/calculador/nucleo.py:216-228`.
- `.cortes` significa especificamente corte que deixou batida órfã por contagem ímpar, e não qualquer corte. `app/ponto/calculador/nucleo.py:222-223`.

`batidas_apuraveis(c, ini, fim)` somente monta `QuerySet` ordenado, com limites inclusivos e exclusão de retratadas. `app/ponto/turnos.py:2868-2895`. Para três dias civis completos, o limite superior deve ser `dia+2 à meia-noite − 1 microssegundo`, pois usa `timestamp__lte`. `app/ponto/turnos.py:2891-2892`.

`Batida` possui `tipo`, `is_intervalo` e `retratada_em`, mas não campo persistido `data_turno` ou `papel`. `app/ponto/models.py:8-79`. O papel `_papel_ata` surge somente como atributo efêmero do pareador; não equivale automaticamente ao `tipo`. `app/ponto/turnos.py:1310-1315`.

## 16/11 03:19 · sessao 0f8d2d7e0419 · - · - · origem=Ronald (tmux)

- Espelho completo proibido pela regra literal de cache: `espelho_do_colab` chama `autoridade_do_periodo` em [app/ponto/services/espelho.py:1227](/home/ronald/codex_ws/3580b35e028267d85318cb75fd5b3d0c40e04a00/app/ponto/services/espelho.py:1227); esta chama `efeitos_da_ausencia` em `app/ponto/services/espelho.py:1068`; ela chama `CAT.parciais_vigentes()` em `app/ponto/services/efeito_ausencia.py:38`; esta lê `vigente()` em `app/ponto/catalogo/ausencias.py:357`, que escreve `_CACHE` em `app/ponto/catalogo/ausencias.py:279-280`.
- Não há equivalente armazenado comprovado para a linha escolhida pelo E6: `minutos_realizados` é produzido pelo montador em `app/ponto/services/espelho.py:783`; `pago_h` é produzido em `app/ponto/services/espelho.py:811-812`. Ler apenas DiaPago não prova a fonte que a função E6 escolheria, pois ela prioriza `minutos_realizados` (`app/ponto/management/commands/e6_oraculo.py:263-269`).
- Para o fato ESPELHO, emitir `nao lido: grava`, número/fonte `[nao verificado]`, sem chamar `espelho_do_colab` nem passar um dict fabricado a `minutos_do_espelho`; dict vazio resultaria falsamente em “sem lavratura” (`app/ponto/management/commands/e6_oraculo.py:263-269`).
- Diagnóstico seguro de pausa com função real: `_realizado_do_dia_tela(colaborador, d, turnos_juiz, escalas_periodo, celulas)` (`app/ponto/services/espelho.py:267-294`); ela usa `vinculo_do_dia`, puro (`app/escala/alimentacao.py:228-286`), `_intervalo_livre`, puro (`app/escala/models.py:243-250`), `intervalo_do_dia`/`pausas_do_dia`, leitores (`app/escala/models.py:1135-1190`), e `realizado_do_dia` com argumentos completos (`app/ponto/turnos.py:774-795`), que encaminha à função pura `realizado_dos_turnos` (`app/ponto/turnos.py:707-771`).
- `RealizadoDoDia` publica `minutos`, `sem_turno`, `aberto`, `longo`, `janela_descontada`, `borda_posicional` (`app/ponto/turnos.py:596-601`). O desconto declarado aplicado é publicado em `janela_descontada` (`app/ponto/turnos.py:760-771`); o espelho o publica como `realizado_janela_descontada` (`app/ponto/services/espelho.py:817-819`).
- Pausas reconhecidas por turno: `intervalos_do_turno(turno)` é leitor puro que aplica o piso aos pares já marcados (`app/ponto/turnos.py:502-546`); pausa por borda posicional é decisão separada, acessível pela função pura `_borda_posicional(turnos_do_dia)` (`app/ponto/turnos.py:638-704`).
- Contexto precisa ficar explícito: o espelho carrega seus turnos para toda a janela visual (`app/ponto/services/espelho.py:614`); resultado de pausa obtido usando turnos de `dia-1..dia+1` deve ser rotulado diagnóstico da janela da sonda. Igualdade com desconto efetivamente publicado pelo espelho completo é `[nao verificado]`.

## 16/11 03:18 · sessao c8b4644a799e · - · - · origem=Ronald (tmux)

Auditoria dos seis itens, sem forçar classes que os anexos não sustentam:

1. **Visto pelo Codex; descarte pela triagem não explícito.** `.q`: `pv_c6.q`, pergunta 1: “há caminho em que a recusa ainda deixa algo gravado”; pergunta 4: “pouso antes do pre-requisito”. `.r`: `pv_c6.r`, seção Posto: “Falha do histórico após gravação continua deixando o posto novo persistido”; teste 5 “Caracteriza a gravação consumada”. `entrega.md` registra “mutantes (2) verdes nos dois lados, por desenho”, mas não identifica isso como descarte da atomicidade posto+histórico. **(b) somente se qualificado: visto e não curado; descarte formal [nao verificado].** [nao verificado]

2. **Núcleo visto, aceito e mantido aberto; causa adicional não vista.** `.q`: `pv_c6_v2.q`, pergunta 1: “Ausencia (nova ou reaberta por _reabrir) ... arquivo ... fica no storage”; `pv_c6_v3.q`, pergunta 2: “que caminhos ainda deixam arquivo ... (inclusive _reabrir)?”. `.r`: `pv_c6_v2.r` e `pv_c6_v3.r` declaram PROCEDENTE exatamente a falha intermediária. `entrega.md`, Residual: “ABERTO ... Cura na ORIGEM ... arquivo da obra LOTE 7, proibido aqui”. Isso **não é descarte** e não é ausência de acesso a RED/suite/prod. A rota específica `post_save` → `engolir` → transação envenenada não aparece nas respostas; esse componente admite **(a)**, com tema perguntado também em `pv_c6.q`, perguntas 1–2, incluindo efeitos/post_save. Item composto exige separar núcleo e caminho extra. [nao verificado]

3. **(b), inequívoco.** `.q`: `pv_c6.q`, pergunta 2, sequência global e `select_for_update` fora da porta. `.r`: `pv_c6.r`, “Observação — ordem global composta”. `entrega.md`, v1: “DESCARTADO como cura: já existia dentro do atomic de a.save() ... vai ANEXO ao opus”.

4. **(b), inequívoco.** `.q`: `pv_c6.q`, pergunta 4 inclui “derivação paralela de fonte única” e pergunta 5 confronto com JUIZES. `.r`: `pv_c6.r`, lista final: “Observação — comentário desatualizado: ‘lei em aberto’ diverge da autorização já registrada no vivo”. `entrega.md`, v1: “descartado: texto pre-existente movido, não reescrito; fila”.

5. **(e).** Nenhuma das três `.q` pergunta se o padrão declarado nos commits corresponde ao padrão efetivamente realizado. Os checklists não incluem essa verificação. `entrega.md` declara o padrão, mas isso não torna o tema perguntado. Não inventar trecho positivo em `.r`; registrar ausência nos três questionários/respostas. [nao verificado]

6. **Visto pelo Codex; descarte pela triagem [nao verificado].** `.q`: `pv_c6_v3.q`, pergunta 4: “2o escritor”; também `pv_c6.q`, pergunta 5, escritores de `Colaborador.situacao`. `.r`: `pv_c6_v3.r`, checklist: “permanece o reversor, que seleciona antes de chamar gravar, sem revalidar a cobertura sob a trava. É uma corrida possível na cópia”. `entrega.md` não traz descarte específico desse reversor. Não cabe (a), (d), (e); tampouco (c) para o defeito estático, cuja ocorrência em produção apenas fica não verificada.

A taxonomia literal é incompleta para “viu e manteve aberto” e “viu, mas a triagem não registra destino”. Recomendo declarar isso em vez de atribuir descarte inexistente aos itens 1, 2 e 6.

## 16/11 02:52 · sessao 38680d8215aa · ESPELHO-VERDADE-FECHO-V0 · leitura · origem=Code

As referências `estrutura_dias.txt:N` numeram os registros do anexo recebido. Identificação, competência, dia, diferença e classe abaixo são transcritos desse anexo (`estrutura_dias.txt:1-57`).

`dono_da_divergencia` não recebe a classe: retorna CADASTRO para vínculo ausente/folga indefinida; BATIDA para batida real fora de turno ou quantidade ímpar de horários do dia; CADASTRO para fase deslocada ou cadastro × realidade; ESTRUTURA é o restante (`app/ponto/management/commands/e6_oraculo.py:98-110`). Os valores individuais desses insumos não constam no anexo (`estrutura_dias.txt:1-57`).

| Classe presente | Como pode resultar em ESTRUTURA |
|---|---|
| `esp_zero_e6_trabalho` | Espelho zero não determina dono; precisa passar pelas guardas acima, com quantidade par de horários (`app/ponto/management/commands/e6_oraculo.py:98-110`, `:370-378`). |
| `e6_zero_esp_trabalho` | E6 zero não determina dono; zero horários é quantidade par e não aciona BATIDA por paridade (`app/ponto/management/commands/e6_oraculo.py:106-110`, `:370-378`). |
| `diverge_10_60` | A magnitude da diferença só determina a classe; o dono continua dependendo das mesmas guardas (`app/ponto/management/commands/e6_oraculo.py:362-378`). |
| `diverge_acima_60` | A diferença acima de 60 minutos não determina dono; ESTRUTURA continua sendo o resíduo das guardas (`app/ponto/management/commands/e6_oraculo.py:98-110`, `:370-378`). |
| `e6_zero_esp_trabalho/FORA` | IMPAR pode vir de fragmentos ímpares produzidos pelo corte, enquanto o conjunto diário contém dois horários; o dono testa o conjunto, e `/FORA` apenas compara o espelho com a faixa do E6 (`app/ponto/calculador/nucleo.py:236-245`; `app/ponto/management/commands/e6_oraculo.py:106-110`, `:395-404`). |
| `diverge_acima_60/FORA` | O mesmo vale com quatro horários: fragmentos ímpares não tornam ímpar o conjunto usado pelo dono; `/FORA` não muda o dono (`app/ponto/calculador/nucleo.py:236-245`; `app/ponto/management/commands/e6_oraculo.py:106-110`, `:395-404`). |

Para interpretar as sondas propostas: a **fonte** é `minutos_realizados` ou, quando ausente, `pago_h × 60`; ela não consta no anexo (`app/ponto/management/commands/e6_oraculo.py:263-269`; `estrutura_dias.txt:1-57`). **Pares/turnos** incluem entrada, saída, `data_turno`, tipos e marcas de pausa; **célula** inclui modo livre/fixo e intervalo efetivo, insumos usados pelo realizado (`app/ponto/turnos.py:707-795`, `:798-829`).

O destino proposto **“obra a registrar: FONTE-PAR-V0”** significa obter os dados indicados antes de definir a cura específica; essa cura ainda não consta. [nao verificado] A obra existente exige apurar assinaturas e separar ponto cego do oráculo de bug (`app/docs/BACKLOG.md:1822`). Os dois cortes demonstráveis seguem como cenários de BATERIA, com resultado esperado pela regra, conforme o tratamento de cenários novos e dúvidas (`app/docs/BACKLOG.md:1271`, `:1623`).

| colNNN | comp | dia | dif_min | classe | causa (arquivo:linha) | destino |
|---|---|---|---:|---|---|---|
| col129 | 10/2026 | 2026-10-12 | 779 | esp_zero_e6_trabalho | nao nomeada — faltam fonte, pares/turnos e célula para explicar o zero diante do par noturno (`app/ponto/management/commands/e6_oraculo.py:263-269`; `app/ponto/turnos.py:719-771`). | obra a registrar: FONTE-PAR-V0 |
| col218 | 10/2026 | 2026-10-15 | -105 | e6_zero_esp_trabalho | nao nomeada — faltam fonte e turnos com `data_turno`, incluindo batidas da véspera, para localizar os 105 minutos (`app/ponto/management/commands/e6_oraculo.py:263-269`; `app/ponto/turnos.py:719-721`). | obra a registrar: FONTE-PAR-V0 |
| col252 | 10/2026 | 2026-09-23 | 46 | diverge_10_60 | nao nomeada — faltam fonte, pausa reconhecida e intervalo efetivo da célula para decidir se houve desconto declarado após rejeição da pausa curta (`app/ponto/turnos.py:545-546`, `:756-764`). | obra a registrar: FONTE-PAR-V0 |
| col252 | 10/2026 | 2026-09-24 | 46 | diverge_10_60 | nao nomeada — faltam fonte, pausa reconhecida e intervalo efetivo da célula para decidir se houve desconto declarado após rejeição da pausa curta (`app/ponto/turnos.py:545-546`, `:756-764`). | obra a registrar: FONTE-PAR-V0 |
| col252 | 10/2026 | 2026-09-25 | 48 | diverge_10_60 | nao nomeada — faltam fonte, pausa reconhecida e intervalo efetivo da célula para decidir se houve desconto declarado após rejeição da pausa curta (`app/ponto/turnos.py:545-546`, `:756-764`). | obra a registrar: FONTE-PAR-V0 |
| col252 | 10/2026 | 2026-09-28 | 46 | diverge_10_60 | nao nomeada — faltam fonte, pausa reconhecida e timestamps completos para decidir a aplicação do piso de pausa e da janela declarada (`app/ponto/turnos.py:545-546`, `:756-764`). | obra a registrar: FONTE-PAR-V0 |
| col252 | 10/2026 | 2026-10-01 | 45 | diverge_10_60 | nao nomeada — faltam fonte, pausa reconhecida e intervalo efetivo da célula para distinguir janela declarada de outra composição do realizado (`app/ponto/turnos.py:545-546`, `:756-764`). | obra a registrar: FONTE-PAR-V0 |
| col252 | 10/2026 | 2026-10-02 | 48 | diverge_10_60 | nao nomeada — faltam fonte, pausa reconhecida e intervalo efetivo da célula para decidir se houve desconto declarado após rejeição da pausa curta (`app/ponto/turnos.py:545-546`, `:756-764`). | obra a registrar: FONTE-PAR-V0 |
| col252 | 10/2026 | 2026-10-06 | 46 | diverge_10_60 | nao nomeada — faltam fonte, pausa reconhecida e intervalo efetivo da célula para decidir se houve desconto declarado após rejeição da pausa curta (`app/ponto/turnos.py:545-546`, `:756-764`). | obra a registrar: FONTE-PAR-V0 |
| col252 | 10/2026 | 2026-10-09 | 48 | diverge_10_60 | nao nomeada — faltam fonte, pausa reconhecida e intervalo efetivo da célula para decidir se houve desconto declarado após rejeição da pausa curta (`app/ponto/turnos.py:545-546`, `:756-764`). | obra a registrar: FONTE-PAR-V0 |
| col369 | 10/2026 | 2026-10-12 | -60 | diverge_10_60 | nao nomeada — faltam fonte, turnos e marcas da pausa para explicar o espelho bruto; o cálculo do realizado não recebe o veredito feriado (`app/ponto/turnos.py:707-718`, `:756-771`). | obra a registrar: FONTE-PAR-V0 |
| col37 | 10/2026 | 2026-10-05 | -241 | e6_zero_esp_trabalho | nao nomeada — faltam fonte e turnos com `data_turno`, incluindo batidas vizinhas, para localizar os minutos sem horários no E6 (`app/ponto/management/commands/e6_oraculo.py:263-269`; `app/ponto/turnos.py:719-721`). | obra a registrar: FONTE-PAR-V0 |
| col37 | 10/2026 | 2026-10-09 | -241 | e6_zero_esp_trabalho | nao nomeada — faltam fonte e turnos com `data_turno`, incluindo batidas vizinhas, para localizar os minutos sem horários no E6 (`app/ponto/management/commands/e6_oraculo.py:263-269`; `app/ponto/turnos.py:719-721`). | obra a registrar: FONTE-PAR-V0 |
| col382 | 10/2026 | 2026-09-30 | -364 | e6_zero_esp_trabalho | nao nomeada — faltam fonte e turnos com `data_turno`, incluindo batidas vizinhas, para localizar os minutos sem horários no E6 (`app/ponto/management/commands/e6_oraculo.py:263-269`; `app/ponto/turnos.py:719-721`). | obra a registrar: FONTE-PAR-V0 |
| col451 | 10/2026 | 2026-09-23 | 11 | diverge_10_60 | nao nomeada — faltam fonte, bordas do turno e intervalo efetivo da célula para explicar o desconto diante de duas batidas (`app/ponto/turnos.py:735-771`). | obra a registrar: FONTE-PAR-V0 |
| col451 | 10/2026 | 2026-09-25 | 13 | diverge_10_60 | nao nomeada — faltam fonte, bordas do turno e intervalo efetivo da célula para explicar o desconto diante de duas batidas (`app/ponto/turnos.py:735-771`). | obra a registrar: FONTE-PAR-V0 |
| col478 | 10/2026 | 2026-09-25 | -711 | diverge_acima_60 | nao nomeada — faltam fonte, tipos/papéis e turnos das datas vizinhas para explicar os 719 minutos diante do par curto (`app/ponto/management/commands/e6_oraculo.py:263-269`; `app/ponto/turnos.py:609-622`, `:735-771`). | obra a registrar: FONTE-PAR-V0 |
| col515 | 10/2026 | 2026-10-09 | -244 | e6_zero_esp_trabalho | nao nomeada — faltam fonte e turnos com `data_turno`, incluindo batidas vizinhas, para localizar os minutos sem horários no E6 (`app/ponto/management/commands/e6_oraculo.py:263-269`; `app/ponto/turnos.py:719-721`). | obra a registrar: FONTE-PAR-V0 |
| col515 | 10/2026 | 2026-10-11 | -245 | e6_zero_esp_trabalho | nao nomeada — faltam fonte e turnos com `data_turno`, incluindo batidas vizinhas, para localizar os minutos sem horários no E6 (`app/ponto/management/commands/e6_oraculo.py:263-269`; `app/ponto/turnos.py:719-721`). | obra a registrar: FONTE-PAR-V0 |
| col723 | 10/2026 | 2026-10-08 | -95 | diverge_acima_60 | nao nomeada — faltam fonte, marcas do intervalo 10:44–13:20 e janela da célula para decidir por que o espelho não descontou esse vão integralmente (`app/ponto/turnos.py:756-766`). | obra a registrar: FONTE-PAR-V0 |
| col746 | 10/2026 | 2026-09-24 | -96 | diverge_acima_60 | nao nomeada — faltam fonte, marcas do intervalo 13:02–15:39 e janela da célula para decidir por que o espelho não descontou esse vão integralmente (`app/ponto/turnos.py:756-766`). | obra a registrar: FONTE-PAR-V0 |
| col841 | 10/2026 | 2026-10-20 | -159 | diverge_acima_60 | nao nomeada — faltam fonte, bordas, papéis e marcas das quatro batidas para identificar qual vão o espelho descontou (`app/ponto/turnos.py:735-766`, `:798-829`). | obra a registrar: FONTE-PAR-V0 |
| col854 | 10/2026 | 2026-10-04 | -711 | diverge_acima_60 | nao nomeada — faltam fonte, tipos/papéis e turnos das datas vizinhas para explicar os 718 minutos diante do par curto (`app/ponto/management/commands/e6_oraculo.py:263-269`; `app/ponto/turnos.py:609-622`, `:735-771`). | obra a registrar: FONTE-PAR-V0 |
| col864 | 10/2026 | 2026-10-12 | -60 | diverge_10_60 | nao nomeada — faltam fonte, turnos e marcas da pausa para explicar o espelho bruto; o cálculo do realizado não recebe o veredito feriado (`app/ponto/turnos.py:707-718`, `:756-771`). | obra a registrar: FONTE-PAR-V0 |
| col868 | 10/2026 | 2026-09-22 | 679 | esp_zero_e6_trabalho | nao nomeada — faltam fonte, bordas/papéis dos turnos noturnos e célula para identificar por que o espelho zerou os pares do E6 (`app/ponto/management/commands/e6_oraculo.py:263-269`; `app/ponto/turnos.py:719-771`). | obra a registrar: FONTE-PAR-V0 |
| col876 | 10/2026 | 2026-09-21 | 115 | diverge_acima_60 | nao nomeada — faltam fonte, bordas/papéis e pausas reconhecidas dos turnos noturnos para identificar a parcela descontada (`app/ponto/turnos.py:735-771`, `:798-829`). | obra a registrar: FONTE-PAR-V0 |
| col1021 | 11/2026 | 2026-11-06 | -841 | e6_zero_esp_trabalho/FORA | O E6 corta 05:55→19:56 por ultrapassar 14 horas, formando dois fragmentos unitários sem par fechado e produzindo E6 e teto zero (`app/ponto/calculador/nucleo.py:215-247`). | cenario de BATERIA — corte >14h com duas batidas (`app/docs/BACKLOG.md:1271`, `:1822`) |
| col110 | 11/2026 | 2026-11-12 | -241 | e6_zero_esp_trabalho | nao nomeada — faltam fonte e turnos com `data_turno`, incluindo batidas vizinhas, para localizar os minutos sem horários no E6 (`app/ponto/management/commands/e6_oraculo.py:263-269`; `app/ponto/turnos.py:719-721`). | obra a registrar: FONTE-PAR-V0 |
| col112 | 11/2026 | 2026-10-21 | 292 | diverge_acima_60 | nao nomeada — faltam fonte, bordas do turno noturno e janela efetiva da célula para explicar o desconto diante do único par do E6 (`app/ponto/turnos.py:735-771`). | obra a registrar: FONTE-PAR-V0 |
| col112 | 11/2026 | 2026-10-22 | 294 | diverge_acima_60 | nao nomeada — faltam fonte, bordas do turno noturno e janela efetiva da célula para explicar o desconto diante do único par do E6 (`app/ponto/turnos.py:735-771`). | obra a registrar: FONTE-PAR-V0 |
| col113 | 11/2026 | 2026-10-30 | -242 | e6_zero_esp_trabalho | nao nomeada — faltam fonte e turnos com `data_turno`, incluindo batidas vizinhas, para localizar os minutos sem horários no E6 (`app/ponto/management/commands/e6_oraculo.py:263-269`; `app/ponto/turnos.py:719-721`). | obra a registrar: FONTE-PAR-V0 |
| col113 | 11/2026 | 2026-11-01 | -241 | e6_zero_esp_trabalho | nao nomeada — faltam fonte e turnos com `data_turno`, incluindo batidas vizinhas, para localizar os minutos sem horários no E6 (`app/ponto/management/commands/e6_oraculo.py:263-269`; `app/ponto/turnos.py:719-721`). | obra a registrar: FONTE-PAR-V0 |
| col114 | 11/2026 | 2026-11-08 | -10 | diverge_10_60 | nao nomeada — faltam fonte, marcas da pausa 14:50–16:00 e janela da célula para distinguir pausa reconhecida de desconto declarado (`app/ponto/turnos.py:756-766`). | obra a registrar: FONTE-PAR-V0 |
| col114 | 11/2026 | 2026-11-15 | -59 | diverge_10_60 | nao nomeada — faltam fonte, turnos e marcas da pausa para explicar o espelho bruto; o cálculo do realizado não recebe o veredito feriado (`app/ponto/turnos.py:707-718`, `:756-771`). | obra a registrar: FONTE-PAR-V0 |
| col130 | 11/2026 | 2026-10-29 | 337 | diverge_acima_60 | nao nomeada — faltam fonte e pares com `data_turno` para identificar a inclusão do segmento 18:56→00:33 no espelho (`app/ponto/turnos.py:719-740`; `app/ponto/management/commands/e6_oraculo.py:263-269`). | obra a registrar: FONTE-PAR-V0 |
| col200 | 11/2026 | 2026-10-29 | -180 | e6_zero_esp_trabalho | nao nomeada — faltam fonte e turnos com `data_turno`, incluindo batidas vizinhas, para localizar os minutos sem horários no E6 (`app/ponto/management/commands/e6_oraculo.py:263-269`; `app/ponto/turnos.py:719-721`). | obra a registrar: FONTE-PAR-V0 |
| col200 | 11/2026 | 2026-11-02 | -180 | e6_zero_esp_trabalho | nao nomeada — faltam fonte e turnos com `data_turno`, incluindo batidas vizinhas, para localizar os minutos sem horários no E6 (`app/ponto/management/commands/e6_oraculo.py:263-269`; `app/ponto/turnos.py:719-721`). | obra a registrar: FONTE-PAR-V0 |
| col218 | 11/2026 | 2026-11-14 | -103 | e6_zero_esp_trabalho | nao nomeada — faltam fonte e turnos com `data_turno`, incluindo batidas vizinhas, para localizar os minutos sem horários no E6 (`app/ponto/management/commands/e6_oraculo.py:263-269`; `app/ponto/turnos.py:719-721`). | obra a registrar: FONTE-PAR-V0 |
| col220 | 11/2026 | 2026-11-10 | 93 | diverge_acima_60 | nao nomeada — faltam fonte, bordas/papéis e pausas reconhecidas dos turnos noturnos para identificar a parcela descontada (`app/ponto/turnos.py:735-771`, `:798-829`). | obra a registrar: FONTE-PAR-V0 |
| col252 | 11/2026 | 2026-11-09 | 46 | diverge_10_60 | nao nomeada — faltam fonte, pausa reconhecida e intervalo efetivo da célula para decidir se houve desconto declarado após rejeição da pausa curta (`app/ponto/turnos.py:545-546`, `:756-764`). | obra a registrar: FONTE-PAR-V0 |
| col252 | 11/2026 | 2026-11-10 | 47 | diverge_10_60 | nao nomeada — faltam fonte, pausa reconhecida e intervalo efetivo da célula para decidir se houve desconto declarado após rejeição da pausa curta (`app/ponto/turnos.py:545-546`, `:756-764`). | obra a registrar: FONTE-PAR-V0 |
| col252 | 11/2026 | 2026-11-11 | 48 | diverge_10_60 | nao nomeada — faltam fonte, pausa reconhecida e intervalo efetivo da célula para decidir se houve desconto declarado após rejeição da pausa curta (`app/ponto/turnos.py:545-546`, `:756-764`). | obra a registrar: FONTE-PAR-V0 |
| col252 | 11/2026 | 2026-11-13 | 46 | diverge_10_60 | nao nomeada — faltam fonte, pausa reconhecida e intervalo efetivo da célula para decidir se houve desconto declarado após rejeição da pausa curta (`app/ponto/turnos.py:545-546`, `:756-764`). | obra a registrar: FONTE-PAR-V0 |
| col296 | 11/2026 | 2026-11-02 | -345 | e6_zero_esp_trabalho | nao nomeada — faltam fonte e turnos com `data_turno`, incluindo batidas vizinhas, para localizar os minutos sem horários no E6 (`app/ponto/management/commands/e6_oraculo.py:263-269`; `app/ponto/turnos.py:719-721`). | obra a registrar: FONTE-PAR-V0 |
| col37 | 11/2026 | 2026-11-14 | -241 | e6_zero_esp_trabalho | nao nomeada — faltam fonte e turnos com `data_turno`, incluindo batidas vizinhas, para localizar os minutos sem horários no E6 (`app/ponto/management/commands/e6_oraculo.py:263-269`; `app/ponto/turnos.py:719-721`). | obra a registrar: FONTE-PAR-V0 |
| col382 | 11/2026 | 2026-11-10 | -418 | e6_zero_esp_trabalho | nao nomeada — faltam fonte e turnos com `data_turno`, incluindo batidas vizinhas, para localizar os minutos sem horários no E6 (`app/ponto/management/commands/e6_oraculo.py:263-269`; `app/ponto/turnos.py:719-721`). | obra a registrar: FONTE-PAR-V0 |
| col516 | 11/2026 | 2026-10-23 | -361 | e6_zero_esp_trabalho | nao nomeada — faltam fonte e turnos com `data_turno`, incluindo batidas vizinhas, para localizar os minutos sem horários no E6 (`app/ponto/management/commands/e6_oraculo.py:263-269`; `app/ponto/turnos.py:719-721`). | obra a registrar: FONTE-PAR-V0 |
| col516 | 11/2026 | 2026-10-28 | -372 | e6_zero_esp_trabalho | nao nomeada — faltam fonte e turnos com `data_turno`, incluindo batidas vizinhas, para localizar os minutos sem horários no E6 (`app/ponto/management/commands/e6_oraculo.py:263-269`; `app/ponto/turnos.py:719-721`). | obra a registrar: FONTE-PAR-V0 |
| col727 | 11/2026 | 2026-11-10 | -307 | e6_zero_esp_trabalho | nao nomeada — faltam fonte e turnos com `data_turno`, incluindo batidas vizinhas, para localizar os minutos sem horários no E6 (`app/ponto/management/commands/e6_oraculo.py:263-269`; `app/ponto/turnos.py:719-721`). | obra a registrar: FONTE-PAR-V0 |
| col874 | 11/2026 | 2026-10-22 | -284 | diverge_acima_60 | nao nomeada — faltam fonte, tipos/papéis e turnos das datas vizinhas para explicar os minutos superiores ao envelope do E6 (`app/ponto/management/commands/e6_oraculo.py:263-269`; `app/ponto/turnos.py:719-771`). | obra a registrar: FONTE-PAR-V0 |
| col884 | 11/2026 | 2026-11-08 | 20 | diverge_10_60 | nao nomeada — faltam fonte, marcas da pausa 13:05–13:45 e janela da célula para distinguir pausa reconhecida de desconto declarado (`app/ponto/turnos.py:756-766`). | obra a registrar: FONTE-PAR-V0 |
| col909 | 11/2026 | 2026-11-11 | -586 | diverge_acima_60/FORA | Ao receber 23:00, o E6 corta o turno iniciado às 07:00 por ultrapassar 14 horas, soma somente o primeiro par e deixa 13:13 e 23:00 em fragmentos sem par (`app/ponto/calculador/nucleo.py:215-247`). | cenario de BATERIA — corte >14h com pausa (`app/docs/BACKLOG.md:1271`, `:1822`) |
| col922 | 11/2026 | 2026-11-01 | -1314 | diverge_acima_60 | nao nomeada — faltam fonte, tipos/papéis e turnos das datas vizinhas para explicar os 1.382 minutos diante do par 22:01–23:09 (`app/ponto/management/commands/e6_oraculo.py:263-269`; `app/ponto/turnos.py:719-771`). | obra a registrar: FONTE-PAR-V0 |
| col922 | 11/2026 | 2026-11-04 | -218 | diverge_acima_60 | nao nomeada — faltam fonte, bordas e `data_turno` dos turnos noturnos para explicar os minutos superiores ao par do E6 (`app/ponto/management/commands/e6_oraculo.py:263-269`; `app/ponto/turnos.py:719-771`). | obra a registrar: FONTE-PAR-V0 |
| col989 | 11/2026 | 2026-10-23 | -154 | diverge_acima_60 | nao nomeada — faltam fonte, bordas e `data_turno` dos turnos para explicar os minutos superiores ao par 08:32–17:59 (`app/ponto/management/commands/e6_oraculo.py:263-269`; `app/ponto/turnos.py:719-771`). | obra a registrar: FONTE-PAR-V0 |
| col996 | 11/2026 | 2026-10-28 | 580 | esp_zero_e6_trabalho | nao nomeada — faltam fonte, pares/turnos e célula para explicar o zero diante do par noturno, sem deduzir a causa do veredito folga (`app/ponto/management/commands/e6_oraculo.py:263-269`; `app/ponto/turnos.py:719-771`). | obra a registrar: FONTE-PAR-V0 |
| col996 | 11/2026 | 2026-11-10 | 583 | diverge_acima_60 | nao nomeada — faltam fonte e todos os turnos com `data_turno` para identificar quais dos três pares o espelho atribuiu ao dia (`app/ponto/turnos.py:719-771`; `app/ponto/calculador/nucleo.py:236-247`). | obra a registrar: FONTE-PAR-V0 |

O `dif_min=-10` de col114 permanece na classe recebida: a classificação usa a diferença antes do arredondamento, enquanto números e horários publicados perdem precisão; os segundos desse dia não constam (`estrutura_dias.txt:33`; `app/ponto/management/commands/e6_oraculo.py:362-372`, `:401`; `app/ponto/calculador/n

[truncado em 20000 de 20808 caracteres]

## 16/11 02:49 · sessao 21a26ee77c6f · - · - · origem=Ronald (tmux)

O anexo permite identificar assinaturas, mas não escolher univocamente a causa do espelho: `minutos_do_espelho` usa `minutos_realizados` e, se ausente, `pago_h`; o campo fonte não consta no anexo (`app/ponto/management/commands/e6_oraculo.py:263-268`).

- **col252, 12 linhas:** padrão compatível com espelho descontando janela fixa de 60 min enquanto o E6 desconta pausa batida de aproximadamente 12–15 min; o piso de 15 min elimina a pausa curta e libera o desconto declarado (`app/ponto/turnos.py:545-546`, `app/ponto/turnos.py:756-764`). Não constitui prova única sem fonte do espelho, turnos, pausas marcadas, `livre` e intervalo efetivo da célula (`app/ponto/services/espelho.py:286-294`). Esses dados não constam.
- **Feriados col369/col864/col114:** veredito `feriado` não explica sozinho espelho bruto; o cálculo realizado não recebe feriado e decide por turnos, pausas e janela declarada (`app/ponto/turnos.py:707-771`). Faltam fonte, turnos, marcas de pausa, modo e intervalo da célula.
- **Longas pausas col723/col746/col841 e col114 08/11:** números são compatíveis com desconto declarado menor que o intervalo visível, mas a aplicação depende de pausas reconhecidas e modo fixo (`app/ponto/turnos.py:756-766`). Mesmos dados decisórios não constam.
- **Dias sem batidas no anexo:** não provam transferência de jornada. A lista exibida usa `dia_das_batidas`, enquanto o realizado consulta os turnos e a fonte alternativa pode vir do pago (`app/ponto/services/espelho.py:450-451`, `app/ponto/services/espelho.py:574-577`, `app/ponto/services/espelho.py:675-676`, `app/ponto/management/commands/e6_oraculo.py:266-268`). Pedir fonte, turnos com entrada/saída/data_turno e batidas completas das datas vizinhas.
- **Noturnos, zeros e grandes excessos:** HH:MM sem tipo, ATA e marcos não distingue absorção de pausa, inversão de papel, turno incompleto e atribuição à véspera; a ATA pode prevalecer sobre o tipo, a absorção depende de marcas/marcos e o dia depende do envelope (`app/ponto/turnos.py:1195-1204`, `app/ponto/turnos.py:1326-1376`, `app/ponto/turnos.py:1382`, `app/ponto/turnos.py:285-309`). Esses dados não constam.
- **Destino existente geral:** `R1-ESTRUTURA-91` registra expressamente assinaturas −60, −180, −240, +46 e +660, exigindo separar ponto cego do oráculo de bug, um caso por assinatura e cenário pela regra antes da cura (`app/docs/BACKLOG.md:1822`). Isso autoriza destino de apuração; não comprova cura específica do dia.
- **Bateria:** cenário novo de produção deve entrar na bateria (`app/docs/BACKLOG.md:1271`); cenário com dúvida entra sem número inventado e com pergunta escrita (`app/docs/BACKLOG.md:1623`).

## 16/11 02:48 · sessao 2ab7c8a330fd · - · - · origem=Ronald (tmux)

Destinos existentes e escopo comprovado:

| Destino | Referência | Aplicação e limite |
|---|---|---|
| ATA-X-TURNO-PAUSA | app/docs/BACKLOG.md:1785 | Cita especificamente col841, 20/10, pausa 21:44–22:41 e conflito ata × juiz do par; ainda exige confirmar a causa atual pelo código e pelo dado permitido. |
| MOTOR-PASSA-O-TURNO-A-VOLTA | app/docs/BACKLOG.md:868 | Cura pergunta de cumprimento da volta feita por período em vez de turno. |
| MOTOR-LE-O-TURNO | app/docs/BACKLOG.md:1960 | Absorve divergências motor × juiz no par e no dia; inclui volta de intervalo, turno aberto e atribuição noturna. Exige demonstrar essa divergência no dia. |
| ESPELHO-VERDADE-FECHO-V2 | app/docs/BACKLOG.md:2230 | Abrange E1 sem previsão e E3 turno aberto; ausência de batidas no anexo, isoladamente, não demonstra essas condições. |
| V3-BATERIA-DE-CASOS-PELA-REGRA | app/docs/BACKLOG.md:1623 | Destino para cenário com cadastro, batidas e esperado derivados da regra; dúvidas entram sem número inventado. |
| V3-BATERIA-O-RESTO-DOS-38 | app/docs/BACKLOG.md:1722 | Inclui virada de competência, intervalo em folga, atestado em folga e igualdade entre leitores. |
| R1-ESTRUTURA-91 | app/docs/BACKLOG.md:1822 | Exige separar ponto cego do oráculo — inclusive 12x36 com pausa e corte de 14h — de bug do sistema. |

Não usar estes IDs como cura automática:

- `INTERVALO-MINIMO-SEM-LEITOR` trata do campo de CCT sem leitor, não demonstra por si a causa dos deltas +45…+48. app/docs/BACKLOG.md:1707
- `FERIADO-DIA-CHAVE` trata de chave de dia e predicado em cruzamento da meia-noite, não explica automaticamente pausa ignorada em feriado diurno. app/docs/BACKLOG.md:1589
- `ESPELHO-FERIADO-DOBRA` corrigiu a testemunha `espelho_x_dia_pago`, e não a comparação E6 de pausas. app/docs/BACKLOG.md:1588
- `R1-A-i-FILTRO-DATA-CIVIL` cita col218 15/10 e parcial 105, mas consta fechada; sua causa histórica depende de cauda e célula ausentes do anexo. app/docs/BACKLOG.md:1756
- `R1-FLIP-NAO-VIROU` terminou com os pares estudados atribuídos a BATIDA/CADASTRO, sem ESTRUTURA; não serve de explicação genérica para os feriados presentes. app/docs/BACKLOG.md:1804

## 16/11 02:48 · sessao 66f612b8d92f · - · - · origem=Ronald (tmux)

O dono é decidido sem consultar `classe`: primeiro CADASTRO se folga indefinida ou vínculo ausente; depois BATIDA se há batida real fora de turno ou quantidade ímpar de `horas_do_dia`; depois CADASTRO se fase deslocada; por fim CADASTRO se consta em cadastro × realidade, senão ESTRUTURA. [app/ponto/management/commands/e6_oraculo.py:98-110]

Isso vale igualmente para todas as classes do anexo: `esp_zero_e6_trabalho`, `e6_zero_esp_trabalho`, `diverge_10_60`, `diverge_acima_60`, `e6_zero_esp_trabalho/FORA` e `diverge_acima_60/FORA`; as classes são calculadas antes e não são argumento de `dono_da_divergencia`. [app/ponto/management/commands/e6_oraculo.py:370-400]

Zero batidas significa quantidade par: não aciona o ramo BATIDA por paridade. [app/ponto/management/commands/e6_oraculo.py:106-110]

`IMPAR` marca pelo menos um turno cortado com contagem ímpar, enquanto o dono testa a quantidade total de horas atribuídas ao dia; assim dois ou quatro horários no anexo podem coexistir com `IMPAR` e ESTRUTURA. [app/ponto/calculador/nucleo.py:238-245; app/ponto/management/commands/e6_oraculo.py:106-110]

`FORA` significa espelho fora do intervalo `[e6 − 10, teto + 10]`; não escolhe o dono nem prova sozinho erro do espelho. [app/ponto/management/commands/e6_oraculo.py:395-404]

Diagnósticos individuais demonstráveis:

| Linha | Causa demonstrável |
|---|---|
| col1021, 11/2026, 2026-11-06, −841 | O oráculo corta 05:55→19:56 porque supera 14 horas, produzindo dois turnos unitários sem par fechado; por isso `e6=teto=0` e marca IMPAR, embora o conjunto do dia seja par. [app/ponto/calculador/nucleo.py:215-247] |
| col909, 11/2026, 2026-11-11, −586 | Ao receber 23:00, o oráculo corta o turno iniciado 07:00 porque seu vão supera 14 horas: soma 07:00→11:04, deixa 13:13 solta e abre outro turno unitário às 23:00; o segundo par exibido pelo espelho deixa de entrar na soma do oráculo. [app/ponto/calculador/nucleo.py:215-247] |

Os minutos do oráculo e o teto usam os instantes completos em segundos; os horários exibidos são truncados para `%H:%M`, e os quatro números da saída são arredondados separadamente. Assim diferenças de um minuto e `dif_min=-10` com classe `diverge_10_60` não provam erro de limiar: a classificação usa a diferença não arredondada. [app/ponto/calculador/nucleo.py:238-247; app/ponto/management/commands/e6_oraculo.py:362-372; app/ponto/management/commands/e6_oraculo.py:401]

`esp_min` pode vir de `minutos_realizados` ou, se ausente, de `pago_h × 60`; o anexo não informa a fonte. [app/ponto/management/commands/e6_oraculo.py:263-269]

O oráculo não lê célula, tipo/marco das batidas ou ausências; lê o universo apurável e soma pares por sequência e dia de início dos turnos que ele próprio corta. Portanto coincidências como col130 ter diferença próxima do primeiro segmento noturno não demonstram qual ramo do espelho deixou esse segmento fora: faltam fonte do minuto, pares/períodos com `data_turno`, célula e ausências pertinentes. [app/ponto/management/commands/e6_oraculo.py:150-154; app/ponto/calculador/nucleo.py:173-176; app/ponto/calculador/nucleo.py:236-247]

A obra histórica `E6-CAUDA-2` está fechada e redirecionada para PLACAR-ESTRUTURAL: não representa cura futura autorizada desses cortes. [app/docs/BACKLOG.md:1231]

## 16/11 02:44 · sessao 3f63e80f5938 · LOTE3-C6 · verificador · origem=Code

**Permanece 1 PROCEDENTE: a falha intermediária em `_reabrir` ainda pode deixar o anexo novo sem referência após o rollback.** (`app/ponto/services/ausencia.py:1200`, `app/ponto/services/ausencia.py:1210`, `app/ponto/views.py:2496`)

1. **Não-INSS: o envoltório transacional externo foi removido.** A view usa `nullcontext()` e não executa a guarda antecipada do Colaborador nesses tipos. A criação mantém seu `atomic` interno; a reabertura segue sem `atomic` próprio. Isso confirma a restauração desse desenho transacional; identidade integral com `7f9d2e631`: **[não verificado]**, pois o conteúdo dessa revisão não está disponível na cópia. (`app/ponto/views.py:2463`, `app/ponto/views.py:2469`, `app/ponto/services/ausencia.py:350`, `app/ponto/services/ausencia.py:478`)

2. **INSS: a janela intermediária continua descoberta.** O caminho concreto é:

   - A view inicia `ausencia = None`; a atribuição só termina quando `criar_ausencia` retorna. (`app/ponto/views.py:2464`, `app/ponto/views.py:2472`)
   - Encontrando uma rejeitada do mesmo colaborador/tipo/data, o serviço chama `_reabrir`. (`app/ponto/services/ausencia.py:309`, `app/ponto/services/ausencia.py:350`)
   - `_reabrir` salva o upload; seu `except` protege somente esse primeiro `save`. Depois, pode falhar ao salvar a descrição ou mudar o status. (`app/ponto/services/ausencia.py:1197`, `app/ponto/services/ausencia.py:1200`, `app/ponto/services/ausencia.py:1210`, `app/ponto/services/ausencia.py:1211`, `app/ponto/services/ausencia.py:900`)
   - A exceção desfaz o `atomic` externo, mas `ausencia` continua `None`; a limpeza da view é pulada. A linha rejeitada original permanece com o documento anterior, enquanto o arquivo novo fica sem linha apontando para ele. (`app/ponto/views.py:2469`, `app/ponto/views.py:2496`)

   **O alcance existe pelo código:** lançar INSS cria uma aprovada e marca o colaborador afastado; cancelar permite torná-la rejeitada; afastado continua selecionável; repetir o lançamento com a mesma data alcança `_reabrir`. Não há espera mínima nesses caminhos. Existência desse caso em produção: **[não verificado]**. (`app/ponto/views.py:2477`, `app/ponto/views.py:2489`, `app/ponto/views.py:2805`, `app/ponto/views.py:2811`, `app/ponto/views.py:2405`, `app/ponto/services/ausencia.py:309`)

   Os caminhos protegidos são a falha no próprio `save` inicial e a falha posterior ao retorno, como em `_gravar_colab`. A limpeza ainda depende da releitura do banco e da exclusão no storage; falha ao excluir é capturada e retorna `False`. (`app/ponto/services/ausencia.py:478`, `app/ponto/services/ausencia.py:1201`, `app/ponto/views.py:2496`, `app/ponto/services/ausencia.py:174`)

3. **O teste alterado morde a janela que exercita; não passa vazio.** Ele exige uma chamada ao segundo escritor, um arquivo adicional no instante dessa chamada, mensagem da falha, rollback da ausência, situação ativa e inventário final igual ao anterior. Retirar a limpeza deixa a última asserção exposta; impedir a gravação prévia derruba a asserção intermediária. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:168`, `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:173`)

   **Ele não cobre o PROCEDENTE acima:** injeta a falha em `gravar`, alcançado após o retorno do serviço; não prepara rejeitada nem falha dentro de `_reabrir`. Passe real da execução: **[não verificado]**. (`app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:164`, `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:171`, `app/ponto/views.py:2489`)

4. **Checklist — observações:**

   - **Relógio:** a guarda consulta o estado gravado, sem data; o teste congela o instante. Carimbos e cobertura ainda consultam relógio nos serviços/signals. (`app/colaboradores/services/desligamento.py:32`, `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:125`, `app/ponto/services/ausencia.py:408`, `app/ponto/signals.py:122`)
   - **Segundo escritor:** permanece o reversor, que seleciona antes de chamar `gravar`, sem revalidar a cobertura sob a trava. É uma corrida possível na cópia; ocorrência em produção e presença na revisão anterior: **[não verificado]**. (`app/ponto/management/commands/reverter_situacao_afastado.py:61`, `app/ponto/management/commands/reverter_situacao_afastado.py:73`, `app/ponto/services/afastado_avisa.py:126`, `app/colaboradores/services/colaborador.py:133`)
   - **Travas:** INSS trava Colaborador antes da ausência. Não-INSS não recebe essa guarda antecipada, mas pode travar indiretamente pela reconciliação dos signals; portanto “não-INSS não trava” é amplo demais. Havia essa trava antes de c6: **[não verificado]**. (`app/ponto/views.py:2471`, `app/chamados/models.py:2744`, `app/ponto/signals.py:269`, `app/chamados/services/silencio_ausencia.py:102`, `app/chamados/models.py:758`)
   - **Dois significados:** `situacao` mistura vínculo e afastamento por dia; `data_fim=NULL` também representa tanto dia único quanto período aberto, conforme o tipo. (`app/colaboradores/services/desligamento.py:32`, `app/ponto/views.py:2489`, `app/ponto/turnos.py:2730`, `app/ponto/services/ausencia.py:115`)

**Achados finais: 1 PROCEDENTE — `_reabrir`: upload salvo seguido de falha intermediária escapa da compensação da view.** (`app/ponto/services/ausencia.py:1200`, `app/ponto/services/ausencia.py:1210`, `app/ponto/services/ausencia.py:1211`, `app/ponto/views.py:2496`)

## 16/11 02:44 · sessao c63f7dd6b250 · - · - · origem=Ronald (tmux)

Checklist consolidado, somente inspeção estática:

- **Relógio:** a guarda antecipada não consulta datas: `esta_desligado` compara somente `situacao`. Os testes c6 fixam o instante; continuam existindo leituras de relógio nos serviços de ausência e nos signals, fora da borda. Isso não foi introduzido pelo delta. (`app/colaboradores/services/desligamento.py:23`, `app/colaboradores/tests/test_lote3_c6_recusa_nada_gravado.py:58`, `:125`, `:193`; `app/ponto/services/ausencia.py:398`, `:408`, `:1216`; `app/ponto/signals.py:122`.)

- **Segundo escritor:** o reversor continua escrevendo `situacao='ativo'` pela mesma porta. Ele materializa a seleção antes da trava e não revalida a cobertura depois dela; a guarda da porta verifica somente desligamento. Assim, por inspeção, um INSS novo entre seleção e escrita pode acabar seguido pelo `ativo` do reversor. Observação herdada, fora do delta; ocorrência em produção **[não verificado]**. (`app/ponto/management/commands/reverter_situacao_afastado.py:60`, `:61`, `:73`; `app/ponto/services/afastado_avisa.py:124`; `app/colaboradores/services/colaborador.py:55`, `:130`.)

- **Ordem de trava:** no INSS, o atomic da view chama a guarda antes de criar a ausência, passando pela porta global e adquirindo Colaborador primeiro; `gravar` repete a aquisição dentro da mesma transação. (`app/ponto/views.py:2469`, `:2471`, `:2472`, `:2489`; `app/colaboradores/services/colaborador.py:84`, `:94`, `:130`; `app/chamados/models.py:2743`.)

- **Ressalva ao “não-INSS não trava”:** ele não recebe a **guarda antecipada da view**, mas o fluxo pode adquirir Colaborador indiretamente. O `on_commit` da ausência chama `reconciliar_colab`, que pode chegar a `ChamadoColaborador.reconciliar`; este abre atomic e recarrega o estado pela porta global de travas. (`app/ponto/signals.py:263`, `:272`; `app/chamados/services/silencio_ausencia.py:101`, `:126`; `app/chamados/models.py:651`, `:673`, `:754`, `:758`.) A situação histórica exata de `7f9d2e631` é **[não verificado]**, pois a cópia não contém histórico Git.

- **Campo com dois significados:** `data_fim=NULL` representa tanto dia único quanto período aberto; a interpretação depende de `TIPOS_EM_ABERTO`, embora o help text mencione só indeterminado/aberto. Observação herdada. (`app/ponto/services/ausencia.py:115`, `:119`, `:127`; `app/ponto/models.py:751`; `app/ponto/turnos.py:2550`, `:2567`.) Também permanece a mistura em `situacao`: estado de vínculo e afastamento derivado por dia; a view marca afastado imediatamente, enquanto o juiz verifica cobertura do dia. (`app/ponto/views.py:2489`; `app/colaboradores/services/desligamento.py:32`; `app/ponto/turnos.py:2730`.)

**0 procedente novo identificado neste checklist do delta.** As observações acima não demonstram correção do caminho `_reabrir`, cuja auditoria ficou com o agente principal. (`app/ponto/views.py:2469`; `app/ponto/services/ausencia.py:1176`.)

## 16/11 02:43 · sessao e221f6cd633a · - · - · origem=Ronald (tmux)

**PROCEDENTE: reabertura com falha intermediária ainda deixa upload órfão.** A view inicializa `ausencia = None` e só a preenche quando `criar_ausencia` retorna; a limpeza depende de ela estar preenchida. Se `_reabrir` salvar o upload e falhar depois, a transação externa desfaz o vínculo, mas a limpeza da view é ignorada. (`app/ponto/views.py:2464`, `app/ponto/views.py:2469`, `app/ponto/views.py:2472`, `app/ponto/views.py:2496`.)

- `_reabrir` protege somente o próprio `save(documento)`; depois executa, fora desse `try`, o `save(descricao)` e `mudar_estado`, cujo `save(status)` também pode falhar. São pontos concretos de falha após o upload e antes do retorno. (`app/ponto/services/ausencia.py:1197`, `app/ponto/services/ausencia.py:1200`, `app/ponto/services/ausencia.py:1207`, `app/ponto/services/ausencia.py:1210`, `app/ponto/services/ausencia.py:1211`, `app/ponto/services/ausencia.py:900`.)
- Com upload novo, `estado_inicial` retorna `aguardando_decisao, False`; portanto o `save(prazo_documento)` não integra esse caminho específico. (`app/ponto/services/ausencia.py:855`, `app/ponto/services/ausencia.py:1214`.)
- Após rollback, pode continuar existindo a linha rejeitada original, com seu documento anterior; “sem linha” aqui significa **sem linha apontando para o arquivo novo**, não ausência absoluta de registro. (`app/ponto/services/ausencia.py:1197`, `app/ponto/views.py:2494`.)

**Alcance estrutural real confirmado:** lançar INSS nesta tela solicita status aprovada e grava colaborador afastado; o cancelamento permite transformar essa aprovada em rejeitada, sem excluir INSS; afastado continua selecionável. Um novo lançamento com mesmo colaborador/tipo/data encontra a rejeitada e chama `_reabrir`. (`app/ponto/views.py:2477`, `app/ponto/views.py:2488`, `app/ponto/views.py:2805`, `app/ponto/views.py:2811`, `app/ponto/services/ausencia.py:834`, `app/ponto/views.py:2405`, `app/ponto/services/ausencia.py:309`, `app/ponto/services/ausencia.py:350`.)

A constraint não proíbe rejeitadas: só impõe unicidade às linhas vivas. (`app/ponto/models.py:837`, `app/ponto/models.py:839`.)

A oferta efetiva do tipo depende do catálogo vigente, que consulta tipos ativos no banco; sua disponibilidade concreta em produção e a existência de INSS rejeitada no dado de produção fornecido ficam **[não verificado]**. (`app/ponto/catalogo/ausencias.py:249`, `app/ponto/catalogo/ausencias.py:278`, `app/ponto/catalogo/ausencias.py:313`.)

**Observação:** não tratar falha ordinária de auditoria como propagação garantida: `registrar_log` captura exceções e `evento` usa best-effort com savepoint próprio. (`app/core/auditoria.py:17`, `app/core/auditoria.py:26`, `app/core/observ.py:105`, `app/core/observ.py:133`.)

## 16/11 02:41 · sessao 3c73764e38f1 · LOTE7-C6 · verificador · origem=Code

**Os contraexemplos específicos dos itens 2 e 3 foram corrigidos; o item 4 continua parcial.** O teto alcança competências futuras carimbadas, e uma exceção que escape da composição de `decidir` desfaz suas escritas. Ainda passam variantes do selo e falhas de invalidação que a própria função engole. (`app/ponto/services/ausencia.py:147`, `app/ponto/services/ausencia.py:1079`, `app/ponto/services/fechamento.py:1661`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:243`)

**1. Fechamento dos itens da volta 2**

| Item | Veredito |
|---|---|
| **2 — cobertura das competências** | Fecha o exemplo de dezembro futuro: o teto considera o fim da maior competência carimbada. Cada lado continua fornecendo seu próprio trecho, preservando a correção setembro→novembro. A conclusão pressupõe empresa/corte estáveis; há ressalva histórica abaixo. (`app/ponto/services/ausencia.py:147`, `app/ponto/services/ausencia.py:186`, `app/ponto/services/ausencia.py:192`) |
| **3 — transição persistida após falha intermediária** | Fecha a exceção propagada entre a transição e o acordador: todo `_decidir_no_ato` está dentro do `atomic`. Não transforma falhas engolidas em falhas do ato. (`app/ponto/services/ausencia.py:1079`, `app/ponto/services/ausencia.py:1088`, `app/ponto/services/ausencia.py:1122`, `app/ponto/services/fechamento.py:1661`) |
| **4 — selo** | Os exemplos antigos receberam mutantes específicos, mas continuam existindo escritores e silenciadores aceitos sem acordador executável, demonstrados no item 4 abaixo. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:348`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:213`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:247`) |

Um caso concreto ainda permitido é: **a atualização do carimbo falha dentro de `invalidar_fechamento`**. O savepoint desfaz aquela atualização, o `except` retorna zero, `_decidir_no_ato` ignora esse resultado e a decisão pode confirmar com projeção ainda carimbada. Isso é diferente da exceção intermediária corrigida; ocorrência produtiva **[nao verificado]**. (`app/ponto/services/fechamento.py:1651`, `app/ponto/services/fechamento.py:1656`, `app/ponto/services/fechamento.py:1661`, `app/ponto/services/ausencia.py:1122`)

**2. Quem chama `decidir` e o efeito do `atomic`**

O único chamador produtivo direto encontrado em `app/` é `_a4_decidir`, que repassa os argumentos e relança `TransicaoInvalida`. São sete pontos de chamada desse wrapper. (`app/ponto/views.py:85`, `app/ponto/views.py:88`, `app/ponto/views.py:90`)

| Chamador | O que muda |
|---|---|
| `aprovar_ausencia_lote` | Cada item ganha sua transação. O contador sobe após o retorno; captura apenas ausência inexistente e `AusenciaInvalida`. Erro inesperado interrompe a requisição, conservando itens anteriores concluídos. (`app/ponto/views.py:2292`, `app/ponto/views.py:2302`, `app/ponto/views.py:2304`, `app/ponto/views.py:2305`) |
| `rejeitar_ausencia_lote` | Mesma atomicidade por item e mesmos limites do tratamento; não ganha transação coletiva. (`app/ponto/views.py:2333`, `app/ponto/views.py:2338`, `app/ponto/views.py:2342`, `app/ponto/views.py:2343`) |
| `cancelar_ausencia_lote` | Atomicidade por item. A trilha adicional de cancelamento permanece depois da porta, fora do `atomic` dela; `ok` sobe depois dessa chamada de trilha. (`app/ponto/views.py:2378`, `app/ponto/views.py:2384`, `app/ponto/views.py:2387`, `app/ponto/views.py:2390`) |
| `aprovar_ausencia` | Já possui `atomic` envolvendo todos os irmãos; cada `decidir` passa a abrir savepoint interno. Exceção propagada sai do bloco coletivo antes do tratamento e desfaz os irmãos anteriores. Pushes vêm depois do bloco. (`app/ponto/views.py:2674`, `app/ponto/views.py:2676`, `app/ponto/views.py:2679`, `app/ponto/views.py:2694`) |
| `editar_periodo_ausencia` | A escrita do período e o restante da porta ficam juntos. Captura `AusenciaInvalida`, `ValueError` e ausência inexistente; outras exceções sobem. (`app/ponto/views.py:2709`, `app/ponto/views.py:2710`) |
| `rejeitar_ausencia` | Cada irmão fica atômico, mas não existe atomicidade coletiva. O tratamento amplo pode devolver erro após irmãos anteriores já confirmados; antes, também podia conservar a transição parcial do próprio irmão que falhou. (`app/ponto/views.py:2735`, `app/ponto/views.py:2737`, `app/ponto/views.py:2755`, `app/ponto/services/ausencia.py:1079`) |
| `cancelar_ausencia` | A decisão fica atômica; a trilha adicional continua fora da porta. Captura ausência inexistente e `AusenciaInvalida`, sem tratamento amplo dos demais erros. (`app/ponto/views.py:2773`, `app/ponto/views.py:2778`, `app/ponto/views.py:2784`, `app/ponto/views.py:2787`) |

**Não encontrei chamador produtivo adicional em `api/` ou no caminho `saas_core`.** Este último encaminha para `api.urls_core`/`views_core`; suas rotas não incluem decisão de ausência. A porta de anexo da API chama `anexar_documento`, não `decidir`. (`app/config/urls_core.py:6`, `app/api/urls_core.py:5`, `app/api/urls_core.py:7`, `app/api/views.py:3115`, `app/api/views.py:3117`)

As chamadas diretas restantes são testes nestes oito arquivos; recebem a mesma transação interna, eventualmente aninhada na transação do teste. (`app/ponto/services/ausencia.py:1079`)

- `app/ponto/tests/test_porta_ausencia_ferias.py:113`, `app/ponto/tests/test_porta_ausencia_ferias.py:114`.
- `app/ponto/tests/test_anexo_nao_sobrevive_a_linha_que_nao_nasceu.py:168`.
- `app/ponto/tests/test_a4_estados_com_porta.py:171`, `app/ponto/tests/test_a4_estados_com_porta.py:180`, `app/ponto/tests/test_a4_estados_com_porta.py:259`, `app/ponto/tests/test_a4_estados_com_porta.py:269`, `app/ponto/tests/test_a4_estados_com_porta.py:328`, `app/ponto/tests/test_a4_estados_com_porta.py:345`, `app/ponto/tests/test_a4_estados_com_porta.py:357`.
- `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:186`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:306`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:315`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:320`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:330`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:335`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:346`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:380`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:400`.
- `app/escala/tests/test_cobertura_parcial_periodo.py:164`, `app/escala/tests/test_cobertura_parcial_periodo.py:180`, `app/escala/tests/test_cobertura_parcial_periodo.py:189`, `app/escala/tests/test_cobertura_parcial_periodo.py:193`, `app/escala/tests/test_cobertura_parcial_periodo.py:198`, `app/escala/tests/test_cobertura_parcial_periodo.py:227`.
- `app/api/tests/test_atestado_por_minutos.py:114`.
- `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:69`, `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:137`.
- `app/chamados/tests/test_s4_pendente_suspende_cobranca.py:87`.

Os efeitos transversais são:

- **Signals continuam síncronos.** O primeiro `save` e o segundo, quando necessário, continuam acionando o cartório; `atomic` não os adia nem os deduplica. Suas escritas participantes ficam sujeitas ao rollback de uma exceção posterior. (`app/ponto/services/ausencia.py:988`, `app/ponto/services/ausencia.py:1104`, `app/ponto/signals.py:143`, `app/ponto/signals.py:147`)
- **`on_commit` passa a esperar a confirmação da porta**, ou a transação externa da aprovação individual. Antes, em autocommit, executava após cada escrita. Continuam podendo existir dois callbacks; ambos capturam a mesma instância e agora observam seus valores finais. Rollback descarta os callbacks do trecho desfeito. (`app/ponto/signals.py:252`, `app/ponto/signals.py:254`, `app/ponto/signals.py:272`, `app/ponto/views.py:2674`)
- **A instância Python não é restaurada pelo rollback.** Status, decisor e período foram atribuídos antes; o teste novo precisa fazer `refresh_from_db()` para conferir o estado persistido. (`app/ponto/services/ausencia.py:976`, `app/ponto/services/ausencia.py:1091`, `app/ponto/services/ausencia.py:1102`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:402`)
- **Auditoria real continua sendo best effort.** `registrar_log` engole exceções e `evento` possui savepoint próprio. O teste novo injeta uma exceção no mock de `registrar_log`; prova rollback de exceção propagada, não que uma falha real da trilha seja obrigatoriamente propagada. (`app/core/auditoria.py:25`, `app/core/observ.py:105`, `app/core/observ.py:133`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:394`)

**3. Teto, competência e empresa**

**Com empresa/corte estáveis e invalidações bem-sucedidas, não encontrei omissão nem competência acrescentada indevidamente pelo teto.** O escritor calcula a janela com `colab.empresa` e grava os mesmos `mes/ano`; o teto reconstrói essa janela com `colaborador.empresa`. Competência significa mês/ano do **fim** da janela nos dois lados. (`app/ponto/services/fechamento.py:621`, `app/ponto/services/fechamento.py:898`, `app/ponto/services/ausencia.py:151`, `app/ponto/janelas.py:11`)

A maior competência por `(ano, mes)` também tem o maior fim, pois o corte é o mesmo. A linha aberta ocupa continuamente de `data` até esse teto; o invalidante enumera as janelas tocadas e atualiza somente fechamentos carimbados. Uma linha sem fim de tipo fora de `TIPOS_EM_ABERTO` continua ocupando apenas seu dia. (`app/ponto/services/ausencia.py:147`, `app/ponto/janelas.py:15`, `app/ponto/turnos.py:2803`, `app/ponto/turnos.py:2808`, `app/ponto/services/dia_pago.py:715`, `app/ponto/services/fechamento.py:1653`)

**A equivalência histórica não está garantida:** `FechamentoMensal` não guarda a empresa/corte usados na derivação. O cadastro permite alterar o corte, e a transferência permite trocar a empresa do colaborador. (`app/ponto/models.py:405`, `app/ponto/models.py:408`, `app/ponto/models.py:464`, `app/colaboradores/views.py:523`, `app/colaboradores/views.py:1389`)

Contraexemplo condicional, derivado dessas regras:

- Corte **28**, ausência aberta desde **25/10**, outubro carimbado: a janela usada na derivação é **28/09–27/10**, portanto conta a linha. Depois, alterar o corte para **21** e rejeitar a ausência: outubro passa a terminar em **20/10**; o trecho iniciado em 25/10 invalida novembro em diante, deixando o antigo outubro carimbado. (`app/ponto/janelas.py:15`, `app/ponto/services/fechamento.py:639`, `app/colaboradores/services/estrutura.py:18`, `app/ponto/services/ausencia.py:192`, `app/ponto/services/dia_pago.py:716`)
- No sentido inverso, outubro derivado com corte **21** não contava essa linha de 25/10; mudar para **28** faz a invalidação alcançar outubro, embora a projeção originalmente gravada não a contivesse. (`app/ponto/janelas.py:15`, `app/ponto/turnos.py:2543`, `app/ponto/services/ausencia.py:192`, `app/ponto/services/dia_pago.py:716`)

Esses casos dizem respeito à **janela que produziu o gravado**. O leitor atual da folha acompanha o corte atual, assim como o teto; existência dessas mudanças e desses carimbos em produção **[nao verificado]**. (`app/ponto/services/fechamento.py:621`, `app/ponto/services/ausencia.py:151`)

**4. Selo: resultado atual e escapes restantes**

No replay AST em memória, o fonte atual apresentou nove escritores, `faltam=[]` e nenhum apontamento de `_acorda_falso_sem_acordador`; não encontrei falso positivo atual. Os mutantes fornecidos e o controle de `while False … else` passaram nesse replay. Isso verifica a análise estrutural, não a execução Django. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:221`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:263`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:279`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:348`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:374`)

**Continuam passando estes casos concretos:**

**a) `parametro_vale` considera reatribuições, mas esquece a origem inicial do parâmetro.** Com `x=False` e uma `Ausencia` como argumento, este escritor não é enumerado pelo selo. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:135`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:138`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:247`)

```python
def _m(tipo_ausencia, x):
    from ponto.models import TipoAusencia
    if x:
        tipo_ausencia = TipoAusencia.objects.get(pk=x)
    tipo_ausencia.save()
```

**b) Fábrica parâmetro com alias global.** `_m(lambda: ausencia)` salva a ausência, mas o censo produz entidade canônica `TipoAusencia`; a regra procura esse nome nos parâmetros, onde existe apenas `TA`. O `save` posterior é suprimido porque a construção já contou. (`app/core/censo_escritas.py:285`, `app/core/censo_escritas.py:372`, `app/core/censo_escritas.py:409`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:243`)

```python
from ponto.models import TipoAusencia as TA

def _m(TA):
    t = TA()
    t.save()
```

**c) `_acorda=False` dentro de `**kwargs`.** A regra exige `k.arg == '_acorda'`; nesse formato, `k.arg` é `None`. O replay aceita a função sem acordador. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:213`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:214`)

```python
def _m(a):
    mudar_estado(a, 'rejeitada', **{'_acorda': False})
```

**d) Saída de ramo não propagada ao bloco externo.** O replay aceita este acordador inalcançável: `_vivos` corta somente o bloco que contém diretamente o `return`. Também aceita `False and _acordar_projecao(...)`, pois a visita não trata curto-circuito. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:161`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:178`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:189`)

```python
def _m(a):
    a.save()
    if True:
        return a
    _acordar_projecao(a.colaborador, {}, {}, motivo='x')
```

Há ainda **falso positivo latente introduzido pelo descarte de corpos aninhados**: uma função que salva, define `_f` contendo o acordador e efetivamente chama `_f()` recebe `faltam=['_m']`. A regra descarta o corpo sem verificar essa chamada. Não encontrei esse padrão no fonte atual. (`app/ponto/tests/test_lote7_selo_escritor_acorda.py:176`, `app/ponto/tests/test_lote7_selo_escritor_acorda.py:263`)

Execução da suíte Django, acesso a banco/log e incidência produtiva dos contraexemplos: **[nao verificado]**. [nao verificado]

_5925 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
