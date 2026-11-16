# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 08:23** · publicado em **16/11/2026 08:23** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 16/11 08:23 · sessao d98b3122a3d2 · - · - · origem=Ronald (tmux)

**C deve ser (e), pela leitura estrita.** O tema exato é exigir uma pergunta de lei/predicado nomeado antes de acrescentar o literal da Justificativa. Isso difere de:

- duplicar juiz **registrado** (`codex_v1.q`, item 7);
- inventariar leitores/escritores e conferir significados (`codex_v1.q`, item 1);
- reutilizar o predicado de **Ausência** (`codex_v1.q`, item 2).

Nenhuma pergunta transcrita exige nomear o predicado faltante de Justificativa ou aplicar JUIZ-ANTES-DO-SITIO. Minha recomendação anterior de a confundiu tema adjacente com a exigência concreta de C. O conteúdo dos contratos apenas mencionados como anexos permanece [nao verificado].

**E comporta b somente com qualificação:** “visto e deixado sem cura pela triagem”. `entrega.md`, triagem v2 item 6, diz **“NAO CURADO, ANEXO ao opus”**, não que o diagnóstico era improcedente. Isso sustenta recusa/adiamento da correção; não sustenta afirmar descarte explícito do achado. As parcelas específicas de consultas para todo fim NULL e de selo restrito à porta tampouco foram expressamente diagnosticadas nas respostas.

## 16/11 08:23 · sessao f156027a2f58 · - · - · origem=Ronald (tmux)

A chave E6 é o calendário da primeira batida de cada grupo; os pares são posições consecutivas, sem consultar `tipo` nem DNA (`app/ponto/calculador/nucleo.py:236-253`). O espelho recebe turnos do pareador (`app/ponto/services/espelho.py:609-617`), cujo papel da ata pode prevalecer sobre `tipo` (`app/ponto/turnos.py:1192-1215,1310-1325,1377-1382`).

| Caso | Causa candidata e limite |
|---|---|
| col296 2026-11-02 | **Cauda noturna atribuída ao calendário seguinte pelo E6**: nenhum grupo E6 começa em 02, enquanto o grupo de 03 tem batidas 00:15/01:29/07:00 e a célula de 02 declara 19–07. `_data_do_turno` pode atribuir retorno da madrugada e saída órfã à véspera, se houver seu DNA/envelope (`app/ponto/turnos.py:285-309,1253-1302`). Papéis/data_turno efetivos [nao verificado]. |
| col37 2026-11-14 | **Mesma candidata**: cauda 02:00/03:00/07:01 em 15 diante da célula 19–07 de 14; E6 chaveia 15, enquanto juiz pode chavear 14 (`app/ponto/calculador/nucleo.py:236-247`; `app/ponto/turnos.py:285-309,1253-1302`). Papéis/data_turno efetivos [nao verificado]. |
| col727 2026-11-10 | **Mesma candidata**: cauda 01:00/02:00/07:07 em 11 diante da célula 19–07 de 10 (`app/ponto/calculador/nucleo.py:236-247`; `app/ponto/turnos.py:285-309,1253-1302`). Papéis/data_turno efetivos [nao verificado]. |
| col516 2026-10-28 | **Mesma candidata**: par 00:43→06:55 em 29 pode pertencer à célula 19–07 de 28; E6 chaveia 29 (`app/ponto/calculador/nucleo.py:236-247`; `app/ponto/turnos.py:285-309,1253-1302`). Entrada noturna ausente na janela fornecida; papéis/data_turno efetivos [nao verificado]. |
| col382 2026-11-10 | **Entrada após meia-noite atribuída à véspera pelo juiz**: E6 mantém o grupo 11/11 00:02→07:51 em 11, mas a célula de 10 declara 23:50–07:50; o juiz pode trazê-lo para 10 (`app/ponto/turnos.py:299-309,1253-1302`). O grupo iniciado 09/11 23:48 continua chaveado em 09 pelo E6 (`app/ponto/calculador/nucleo.py:236-247`). Não confundir esse grupo anterior com a candidata que explica minutos potenciais em 10; data_turno/papéis reais [nao verificado]. |
| col874 2026-10-22 | **Pareamento sequencial versus marcos/papéis**: E6 consome `E21/10 22:03 → E22/10 10:00` como par no dia 21 e soma apenas `15:45→22:07` em 22; o marco `hi=10:00` da célula de 22 pode reivindicar a batida que a sequência utilizou como segunda posição do par anterior (`app/ponto/calculador/nucleo.py:236-247`; `app/ponto/turnos.py:1192-1215,1565-1576,1631-1657`). Resultado de `periodos_do_dia`/papéis [nao verificado]. |
| col989 2026-10-23 | **Pareamento sequencial de S→E versus jornada noturna por marcos**: E6 soma `S08:32→E17:59`; DNA declara `hi18:00/hf06:00` e há `E17:59→S24/10 06:00`. O pareador abre com E e fecha com S, sujeito ao papel da ata (`app/ponto/turnos.py:1377-1382,1402-1419,1421-1496`), enquanto E6 soma posições (`app/ponto/calculador/nucleo.py:236-247`). Papéis/data_turno efetivos [nao verificado]. |
| col996 2026-11-10 | **E6 soma dois grupos no mesmo dia, incluindo S→E noturno**: o total E6 contém o grupo diurno e `S21:16→E11/11 06:59`; E6 acumula ambos sob 10 (`app/ponto/calculador/nucleo.py:236-253`). O pareador depende de E/S e do papel da ata, com nova E encerrando o aberto anterior e S sem E virando órfã (`app/ponto/turnos.py:1377-1382,1417-1419,1495-1502`). A candidata é **sequência versus papéis/turnos**; tratamento real do grupo noturno [nao verificado]. |
| col516 2026-10-23 | **Não nomeada**: E6=0, célula folga e grupo iniciado em 22 não bastam para explicar divergência no dia 23; falta saber quais turnos/papéis o espelho atribuiu a 23 e sua fonte de minutos [nao verificado]. O juiz consulta DNA da véspera, que não veio no dado desse caso (`app/ponto/turnos.py:1253-1302,285-309`). |
| col996 2026-10-28 | **Não nomeada**: E6 soma `S21:18→E29/10 06:57` na folga de 28 por posição; falta saber se/como a ata redefiniu esses papéis ou se o pareador os deixou como órfã/aberto [nao verificado]. Sem papel emitido, vale o tipo gravado (`app/ponto/turnos.py:1377-1382,1417-1419,1495-1502`); folga sem marcos não autoriza inferir um par efetivo no espelho. |

Para os casos de sequência versus ata, a existência dos marcos não garante emissão de papel: o código exige luzes ligadas a batidas reais, turno fechado, bordas identificadas e guardas de alinhamento (`app/ponto/turnos.py:1631-1700`). Esses resultados não vieram no dado [nao verificado].

## 16/11 08:22 · sessao 75ad592e41f2 · - · - · origem=Ronald (tmux)

Para os quatro dias de col252, a causa candidata é **pausa crua do E6 abaixo do piso de 15 minutos, seguida de possível desconto da janela fixa declarada no realizado**: o E6 soma pares e registra o vão sem piso (`app/ponto/calculador/nucleo.py:245-253`); o realizado só reconhece intervalos marcados de pelo menos 15 minutos (`app/ponto/turnos.py:499,542-546`) e, sem intervalo reconhecido, desconta a janela declarada (`app/ponto/turnos.py:756-764`). O espelho recebe o intervalo do dia por `app/ponto/services/espelho.py:286-294`, cuja leitura usa o DNA da célula (`app/escala/models.py:1163-1164,1189-1201`). A execução desse ramo e seu desconto efetivo permanecem [nao verificado].

Para col884 em 08/11, a candidata é **diferença entre a pausa posicional do E6 e o reconhecimento da pausa com tipos gravados invertidos E→S**. O reconhecedor permite superar os tipos gravados quando há carimbos da ata nas duas pontas, ou abertura com papel S e marca na volta (`app/ponto/turnos.py:823-883`). Conforme esse reconhecimento, o realizado desconta a pausa marcada ou recorre à janela fixa (`app/ponto/turnos.py:756-764`). Faltam os papéis da ata, os carimbos, os turnos reais reconhecidos e o desconto efetivo para escolher o ramo; tudo isso está [nao verificado].

O atestado 5452 rejeitado não sustenta causa de abono: folha seleciona apenas ausências aprovadas e cobrança exclui rejeitadas (`app/ponto/turnos.py:2523-2531`).

## 16/11 08:21 · sessao d98b3122a3d2 · - · - · origem=Ronald (tmux)

Recomendação: **A=c; B=a com ressalva; C=a; D=a; E=b com ressalva.**

- **A — c:** `codex_v1.r`, item 14: “suite completa e RED de cada cura contra a base: [nao verificado]”. A pergunta incluía “selo/RED sem mordida” (`codex_v1.q`, item 7), mas o achado depende do resultado RED fornecido apenas no Opus. [nao verificado]
- **B — a quanto ao portão/aval e à conclusão incorreta da entrega.** O tema estava em `codex_v1.q`, itens 7–8. Porém **o alcance técnico foi visto**: `codex_v1.r`, item 13, registra “Dias intermediários também atravessam a porta alterada” e as consultas adicionais de Ausência no kernel. A triagem diz “13 kernel — registrado acima”; **não diz DESCARTADO**. Portanto, chamar B simplesmente de b extrapola a evidência. O erro é reconhecimento parcial seguido de síntese incorreta, não descarte explícito.
- **C — a:** `codex_v1.q`, item 7, pergunta por “2o juiz que duplica juiz registrado”. `codex_v1.r`, item 14: “Segundo juiz registrado novo: não achei”. O inventário de `Justificativa.status` no item 11 não identifica a falta do predicado nomeado. A entrega já reconhece “Justificativa nao tem predicado nomeado”, mas não há diagnóstico correspondente nas respostas Codex.
- **D — a:** `codex_v1.q`, item 4, exige “comportamento dos signals post_save” e efeito sobre atomic; `codex_v3.q`, item 2, explicita atomic e exceções engolidas. `codex_v1.r`, item 12, chama julgamento de “imediato e best effort”; `codex_v3.r`, item 2, destaca signals síncronos. Nenhum explicita o elo **erro de banco capturado por engolir sem savepoint → transação marcada para rollback → mensagem enganosa**. A falha engolida por `invalidar_fechamento`, discutida e descartada na triagem v3, é outro mecanismo, com savepoint.
- **E — b para relógio e fragilidade das provas.** `codex_v1.q`, itens 6–7; `codex_v1.r`, item 6: “inclusive antes de descobrir que os snapshots são iguais”. `codex_v2.r`, item 6: “regressão da leitura antecipada não detectada”. Triagem v2, item 6: “leitura antecipada do relogio nao detectada [...] NAO CURADO [...] fortalece teste, nao muda comportamento”. **Ressalva:** as respostas não diagnosticam expressamente a consulta a FechamentoMensal para todo `data_fim=NULL`, nem o alcance limitado de `_acorda_falso_sem_acordador`; essas parcelas não sustentam b. [nao verificado]

Nenhuma evidência exige **e**: os temas aparecem nas perguntas. **d** não acrescenta uma distinção demonstrável sobre os achados já formulados pelo Opus; seu universo de “nenhum” precisaria ser definido para separá-lo de a.

## 16/11 08:20 · sessao 7cce8f5c690e · ESPELHO-VERDADE-FECHO-V0 · sonda · origem=Code

**O TOTAL fornecido fecha:** `dias=19`, `ok=19`, `erro=0`; portanto, `ok+erro=19`. Não aparece registro `ERRO` de nenhuma classe nem marcador `INCOMPLETO`; consta `rc_sombra=0`. Isso confere o texto colado; localização da saída original em arquivo:linha **[nao verificado]**.

Os valores abaixo vêm exclusivamente do dado raspado, com minutos arredondados a seis casas; localização das linhas `DET` em arquivo **[nao verificado]**. As referências de código fundamentam os mecanismos candidatos, sem confirmar o resultado do espelho.

Para ler a tabela:

- **E6/J** = minutos do dia na leitura mensal / minutos em `E6_JANELA`; **T/C** = turnos/cortes da janela. Quantidade de batidas refere-se à janela de três dias. Paridade dessa quantidade difere do indicador diário E6 (`app/ponto/calculador/nucleo.py:235-247`).
- **Em todos os 19 registros:** `impar_E6=false` e `corte_que_orfanou=false`, conforme dado fornecido; localização original **[nao verificado]**. A flag de orfanamento só marca corte com contagem acumulada ímpar (`app/ponto/calculador/nucleo.py:216-223`).
- **Célula:** `existe/trabalha/tipo_ata`; marcos na ordem `hi / hii / hfi / hf`. **A** = ausências confirmadas/candidatas; **Jus** = justificativas. Cobertura das candidatas **[nao verificado]**; `confirmadas=0` não demonstra inexistência de ausência (`app/ponto/turnos.py:2798-2814`; `app/ponto/catalogo/ausencias.py:430-435`).
- Nas referências abreviadas, `nucleo.py` é `app/ponto/calculador/nucleo.py`; `turnos.py` é `app/ponto/turnos.py`; `espelho.py` é `app/ponto/services/espelho.py`.

| Colaborador | Dia | E6/J — minutos | T/C; batidas da janela | Célula e marcos | Ausências e justificativas | Causa candidata ou fato faltante |
|---|---|---:|---|---|---|---|
| col723 | 2026-10-08 | 344.964253 / 344.964253 | 3/2; 10, par | sim/sim/trabalho; 6x1; 09:00 / 13:20 / 14:20 / 17:20 | A=0/7: atestado ×1 aguardando_decisao, ×6 rejeitada; Jus=0 | **P:** sequência bruta E–E–S–S; E6 pareia posições e trata o vão central como pausa (`nucleo.py:245-253`). Correspondência com papéis/pausas do espelho **[nao verificado]**. |
| col746 | 2026-09-24 | 192.832833 / 192.832833 | 3/2; 12, par | sim/sim/trabalho; 6x1; 11:00 / 14:00 / 15:00 / 19:30 | A=0/2: atestado aguardando_decisao; falta aprovada; Jus=0 | **P:** E–E–S–S; pausa posicional 13:02:51→15:39:40, diferente da janela declarada. Mecanismo E6 (`nucleo.py:245-253`); reconhecimento/desconto no espelho **[nao verificado]**. |
| col841 | 2026-10-20 | 522.438250 / 522.438250 | 3/2; 8, par | sim/sim/trabalho; 12x36; 19:00 / 01:00 / 02:00 / 07:00 | A=0/2: troca_de_plantao aprovada ×2; Jus=0 | **C:** corte separa 20/10 22:41:16 de 21/10 07:04:32: gap ≥8h após quatro batidas (`nucleo.py:216-228`). E6 já incorporou a S das 11:00; papel dessa batida e fecho real **[nao verificado]**. |
| col854 | 2026-10-04 | 6.587167 / 6.587167 | 3/2; 4, par | sim/sim/trabalho; 12x36; 19:00 / — / — / 07:00; intervalo livre | A=0/0; cobertura completa informada; Jus=0 | **C:** duas E, 18:54:44→19:01:20, viram par; o gap ≥8h separa a S das 06:52 do dia seguinte (`nucleo.py:216-228,245-247`). Não é deduplicação E6 (`nucleo.py:48-52`). Pareamento efetivo do espelho **[nao verificado]**. |
| col864 | 2026-10-12 | 449.226067 / 449.226067 | 2/1; 8, par | sim/não/folga; 6x1; sem marcos; regenerada em 30/10 | A=0/1: declaracao rejeitada; Jus=0 | **Não nomeada:** folga com batidas e célula regenerada não bastam para explicar diferença numérica. Realizado soma por `data_turno` (`turnos.py:719-771`); faltam turnos, papéis, pausas efetivos e estado anterior da célula **[nao verificado]**. |
| col868 | 2026-09-22 | 678.729067 / 678.729067 | 2/1; 7, ímpar | sim/sim/trabalho; 6x1; 12:50 / 14:40 / 15:50 / 21:20 | A=0/1: folga_compensatoria aprovada; Jus=0 | **P:** turno E6 noturno com tipos S–E–E–E; sequência soma pares e pausa central independentemente dos tipos (`nucleo.py:245-253`). Papéis da ata e atribuição real **[nao verificado]**. |
| col876 | 2026-09-21 | 664.961317 / 664.961317 | 2/1; 7, ímpar | sim/sim/trabalho; 12x36; 19:00 / 02:00 / 03:00 / 07:00 | A=0/2: abono aprovado; falta aprovada; Jus=0 | **P:** sequência E–S–S–S; E6 considera pausa 04:39→05:36 por posição (`nucleo.py:245-253`). Papel da terceira S e pausa reconhecida **[nao verificado]**. |
| col110 | 2026-11-12 | 0 / 0 | 2/1; 4, par | sim/sim/trabalho; 12x36; 19:00 / 02:00 / 03:00 / 07:00 | A=0/0; cobertura completa informada; Jus=0 | **D:** nenhum turno E6 começa no dia 12; fragmento da madrugada começa no dia 13. E6 chaveia pelo primeiro instante (`nucleo.py:237`); a casa admite atribuição à véspera (`turnos.py:293-308`). `data_turno` efetiva **[nao verificado]**. |
| col112 | 2026-10-21 | 837.908180 / 605.790757 | 3/2; 6, par | sim/sim/trabalho; 5x2; 07:00 / 12:00 / 13:00 / 17:00 | A=0/0; cobertura completa informada; Jus=0 | **M:** borda mensal exclui a entrada de 20/10 antes de 12h; deslocamento do pareamento produz S 21/10 17:03→E 22/10 07:01, reproduzindo o E6 mensal (`app/ponto/management/commands/e6_oraculo.py:149-153`; `nucleo.py:216-247`). Efeito comparado ao espelho **[nao verificado]**. |
| col112 | 2026-10-22 | 834.589572 / 600.712660 | 3/2; 6, par | sim/sim/trabalho; 5x2; 07:00 / 12:00 / 13:00 / 17:00 | A=0/0; cobertura completa informada; Jus=0 | **M:** mesma fase deslocada produz S 22/10 17:02→E 23/10 06:56, reproduzindo o E6 mensal (`app/ponto/management/commands/e6_oraculo.py:149-153`; `nucleo.py:216-247`). Efeito comparado ao espelho **[nao verificado]**. |
| col113 | 2026-10-30 | 0 / 0 | 2/1; 6, par | sim/sim/trabalho; 12x36; 19:00 / 02:00 / 03:00 / 07:00 | A=0/6: atestado aprovado ×3, rejeitado ×1; falta aprovada; abono aprovado; Jus=0 | **D:** turnos E6 começam em 29 e 31/10; não aparece entrada noturna em 30/10 no recorte. Zero decorre do chaveamento E6 (`nucleo.py:237`); possível atribuição da madrugada à véspera (`turnos.py:293-308`), efetivação **[nao verificado]**. |
| col113 | 2026-11-01 | 0 / 0 | 2/1; 6, par | sim/sim/trabalho; 12x36; 19:00 / 02:00 / 03:00 / 07:00 | A=0/6: atestado aprovado ×3, rejeitado ×1; falta aprovada; abono aprovado; Jus=0 | **D:** turnos E6 começam em 31/10 e 02/11; não aparece entrada noturna em 01/11. Chaveamento E6 (`nucleo.py:237`) versus possível atribuição à véspera (`turnos.py:293-308`); turnos reais **[nao verificado]**. |
| col114 | 2026-11-08 | 428.569840 / 428.569840 | 3/2; 12, par | sim/sim/trabalho; 6x1; 10:00 / 14:00 / 15:00 / 18:20; regenerada em 09/11 | A=0/32: ferias aprovadas ×30; atestado aprovado ×2; Jus=0 | **P:** E–E–S–S; pausa E6 posicional 14:50→16:00 (`nucleo.py:245-253`). Papéis/pausa do espelho e eventual efeito da regeneração **[nao verificado]**. |
| col114 | 2026-11-15 | 439.847646 / 439.847646 | 2/1; 8, par | sim/não/folga; 6x1; sem marcos; regenerada em 09/11 | A=0/32: ferias aprovadas ×30; atestado aprovado ×2; Jus=0 | **Não nomeada:** folga com batidas não determina divergência; realizado soma turnos independentemente desse rótulo (`turnos.py:719-771`; `espelho.py:675-679`). Faltam turnos/papéis/pausas efetivos **[nao verificado]**. |
| col130 | 2026-10-29 | 671.905433 / 671.905433 | 2/1; 7, ímpar | sim/sim/trabalho; 12x36; 19:00 / 23:00 / 00:00 / 07:00 | A=0/0; cobertura completa informada; Jus=0 | **P:** E–E–S–E; E6 toma 00:33→01:31 como pausa por posição (`nucleo.py:245-253`). Papéis e pausa reconhecida pelo espelho **[nao verificado]**. |
| col200 | 2026-10-29 | 0 / 0 | 3/2; 10, par | sim/sim/trabalho; 6x1; 22:00 / 02:00 / 03:00 / 06:00 | A=0/6: falta aprovada ×3; atestado aguardando_decisao, aprovado e rejeitado; Jus=0 | **D:** turno que atravessa a madrugada de 29 começa em 28/10; próximo fragmento começa em 30/10, sem entrada noturna de 29 no recorte. Chaveamento E6 (`nucleo.py:237`) versus véspera (`turnos.py:293-308`); aplicação real **[nao verificado]**. |
| col200 | 2026-11-02 | 0 / 0 | 3/2; 8, par | sim/sim/trabalho; 6x1; 22:00 / 02:00 / 03:00 / 06:00 | A=0/7: falta aprovada ×3; atestado aguardando_decisao, aprovado, rejeitado e aguardando_documento; Jus=0 | **D:** nenhum turno E6 começa em 02/11; fragmento seguinte começa às 02:00 de 03/11. Chaveamento E6 (`nucleo.py:237`) versus possível atribuição à véspera (`turnos.py:293-308`); papéis/data_turno **[nao verificado]**. |
| col218 | 2026-11-14 | 0 / 0 | 4/3; 11, ímpar | sim/sim/trabalho; 6x1; 21:00 / 02:00 / 03:00 / 05:00 | A=0/30: ferias aprovadas ×30; Jus=0 | **D:** madrugada de 14 pertence ao turno E6 iniciado em 13; fragmento seguinte começa em 15/11, sem entrada noturna de 14 no recorte. Chaveamento E6 (`nucleo.py:237`) versus véspera (`turnos.py:293-308`); atribuição real **[nao verificado]**. |
| col220 | 2026-11-10 | 430.420683 / 430.420683 | 1/0; 4, par | sim/sim/trabalho; 6x1; 22:00 / 02:00 / 03:00 / 06:00 | A=0/1: folga_abonada aprovada; Jus=0 | **P:** E–S–S–E; E6 toma 03:35→04:31 como pausa e soma o último par S→E (`nucleo.py:245-253`). Papéis da ata, fecho e pausa reconhecida **[nao verificado]**. |

**Agrupamento dos 19 dias**, pelas candidatas acima; confirmação como causa da divergência **[nao verificado]**:

| Grupo | Quantidade | Fundamento e limite |
|---|---:|---|
| **P — pareamento/pausa por posição versus papéis efetivos** | 7 | col723, col746, col868, col876, col114 em 08/11, col130, col220. E6 usa posições (`app/ponto/calculador/nucleo.py:245-253`); o pareador prioriza papel da ata sobre tipo gravado (`app/ponto/turnos.py:1377-1382`). Tipos brutos não provam os papéis consumidos **[nao verificado]**. |
| **C — corte E6 com contagem par separando fecho noturno** | 2 | col841 e col854. Regra e ausência de flag de orfanamento explicadas por `app/ponto/calculador/nucleo.py:216-228`. Fecho do espelho **[nao verificado]**. |
| **D — atribuição noturna pela data do primeiro instante** | 6 | col110; col113 em dois dias; col200 em dois dias; col218. E6 usa primeira data (`app/ponto/calculador/nucleo.py:237`); a casa dispõe de atribuição à véspera (`app/ponto/turnos.py:293-308`). Atribuições efetivas **[nao verificado]**. |
| **M — borda mensal deslocando o pareamento** | 2 | col112 em 21 e 22/10. Os pares reproduzem os dois E6 mensais fornecidos (`app/ponto/management/commands/e6_oraculo.py:149-153`; `app/ponto/calculador/nucleo.py:216-247`). Comparação efetiva com o espelho **[nao verificado]**. |
| **Não nomeada — folga com batidas** | 2 | col864 e col114 em 15/11. Folga não basta: seleção e soma dependem dos turnos (`app/ponto/turnos.py:719-771`). Faltam os insumos efetivamente reconhecidos **[nao verificado]**. |

**Nenhum número do espelho foi inferido.** Minutos, fonte, pausas reconhecidas e desconto efetivo permanecem **[nao verificado]**, conforme `estado="nao lido: grava"` no dado fornecido. O código distingue campo ausente de zero e pode ler fontes diferentes (`app/ponto/management/commands/e6_oraculo.py:263-269`); o desconto depende dos intervalos reconhecidos e, subsidiariamente, da janela declarada (`app/ponto/turnos.py:756-764`).

## 16/11 08:18 · sessao 4d3778f887fc · - · - · origem=Ronald (tmux)

- **E6 cru soma por posição e data do primeiro instante**, sem usar tipo E/S: `app/ponto/calculador/nucleo.py:236-247`. O espelho pede turnos do juiz e intervalo do vínculo: `app/ponto/services/espelho.py:286-294,783`.
- **Tipos brutos EESS/ESSE/SEEE são indícios, não prova do pareamento real**: o pareador consulta e carimba o papel da ata (`app/ponto/turnos.py:1310-1315`), que precede o tipo gravado (`app/ponto/turnos.py:1377-1382`). Os papéis efetivos permanecem [nao verificado].
- **Seis zeros candidatos a atribuição noturna diferente**: col110 12/11; col113 30/10 e 01/11; col200 29/10 e 02/11; col218 14/11. Nos E6_TURNO fornecidos não há início no dia alvo [nao verificado: dado raspado sem arquivo:linha]. Isso explica o zero pelo chaveamento E6 em `app/ponto/calculador/nucleo.py:237`; a casa admite trazer fragmentos de madrugada à véspera em `app/ponto/turnos.py:293-308,312-324,1263-1302`. Confirmação depende dos turnos reais/data_turno/papéis [nao verificado].
- **col854 04/10**: candidato específico “duas E próximas no início + corte por gap com contagem par separando saída noturna”. O par de E gera os 6,587… min e o gap até 06:52 separa a saída pelas regras `app/ponto/calculador/nucleo.py:216-228,245-247`; a flag de orfanamento fica falsa porque o corte ocorre com contagem par (`app/ponto/calculador/nucleo.py:222-223`). E6 não deduplica essas E (`app/ponto/calculador/nucleo.py:48-52`). Pareamento do espelho [nao verificado].
- **Intervalo declarado versus pausa reconhecida**: a casa desconta intervalos reconhecidos; só na ausência deles usa a janela declarada, caso não seja livre (`app/ponto/turnos.py:756-771`). O piso do intervalo reconhecido é aplicado em `app/ponto/turnos.py:542-546`. Célula existente sem marcos declara nenhuma pausa (`app/escala/models.py:1189-1207`). O desconto efetivo dos dias permanece [nao verificado].
- **Folga com batidas não basta para nomear causa numérica**: realizado seleciona por `data_turno` e soma os turnos sem guarda pelo flag folga (`app/ponto/turnos.py:719-771`); o espelho publica folga separadamente (`app/ponto/services/espelho.py:675-679,770,783`). Para col864 12/10 e col114 15/11, faltam os turnos reais e pausas reconhecidas [nao verificado].

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

_5931 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
