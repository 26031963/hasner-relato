# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 14:45** · publicado em **16/11/2026 14:46** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 16/11 14:45 · sessao 427f2c880889 · PRIORIDADE-EV-ITEM1 · sonda · origem=Code

1. A saída informa **`TOTAL|parte=3/3|dias=19|ok=19|erro=0`** e **`rc_sombra=0`**. A linha desses registros no arquivo não foi fornecida: [nao verificado]. **Disparo de tripwire: [nao verificado]**; não há resultado explícito desse controle no trecho apresentado.

2. Na tabela, **espelho = `minutos_realizados` e classe = `STATUS.veredito`**; o E6 é o inteiro da linha do universo, confirmado por `confere_e6_arquivo=true`. (`app/ponto/services/espelho.py:783-819,764-895`; `app/ponto/management/commands/e6_oraculo.py:263-269,401`)

| Código | Data | Espelho: minutos; classe | E6 universo | Categoria/causa apontada pelos detalhes |
|---|---|---|---:|---|
| col220 | 2026-11-10 | 337; `trabalhou` | 430 | **Pareamento e desconto diferentes:** espelho usa turno até 04:31 e desconta 60 min; E6 inclui 06:00 e forma dois pares, com pausa de 55,643 min. (`estrutura_dias.txt:39`; `app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:224-247`) |
| col252 | 2026-11-09 | 551; `trabalhou` | 597 | **Pausa abaixo do piso:** espelho não reconhece a pausa curta e desconta 60 min declarados; E6 desconta 13,556 min entre batidas. (`estrutura_dias.txt:40`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:224-247`) |
| col252 | 2026-11-10 | 551; `trabalhou` | 598 | **Pausa abaixo do piso:** desconto de 60 min no espelho versus 13,463 min no E6. (`estrutura_dias.txt:41`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:224-247`) |
| col252 | 2026-11-11 | 550; `trabalhou` | 598 | **Pausa abaixo do piso:** desconto de 60 min no espelho versus 12,814 min no E6. (`estrutura_dias.txt:42`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:224-247`) |
| col252 | 2026-11-13 | 551; `trabalhou` | 597 | **Pausa abaixo do piso:** desconto de 60 min no espelho versus 13,942 min no E6. (`estrutura_dias.txt:43`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:224-247`) |
| col296 | 2026-11-02 | 345; `trabalhou` | 0 | **Associação ao dia divergente:** espelho atribui ao dia 02 um turno de 03/11, 00:15–07:00, calculando 405−60; E6 tem zero turnos no dia 02. (`estrutura_dias.txt:44`; `app/ponto/services/espelho.py:267-294`; `app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:224-254`) |
| col37 | 2026-11-14 | 241; `trabalhou` | 0 | **Associação ao dia divergente:** espelho atribui ao dia 14 um turno de 15/11, 02:00–07:01, calculando 301−60; E6 tem zero turnos no dia 14. (`estrutura_dias.txt:45`; `app/ponto/services/espelho.py:267-294`; `app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:224-254`) |
| col382 | 2026-11-10 | 418; `trabalhou` | 0 | **Associação ao dia divergente:** espelho usa turno de 11/11, 00:02–07:51, com pausa reconhecida de 51 min: 469−51; E6 tem zero turnos no dia 10. (`estrutura_dias.txt:46`; `app/ponto/services/espelho.py:267-294`; `app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:224-254`) |
| col516 | 2026-10-23 | 361; `folga` | 0 | **Folga com trabalho apurado:** espelho conta 01:00–07:01, sem desconto; célula registra `fato_sem_previsao` e há alerta `folga_trabalhada`. E6 tem zero turnos nesse dia; causa completa desse zero: [nao verificado]. (`estrutura_dias.txt:47`; `app/ponto/turnos.py:725-767`; `app/escala/models.py:1652-1701`; `app/ponto/services/espelho.py:450-460,767`) |
| col516 | 2026-10-28 | 372; `trabalhou` | 0 | **Associação ao dia divergente:** espelho atribui ao dia 28 o turno de 29/10, 00:43–06:55, sem desconto; E6 tem zero turnos no dia 28. (`estrutura_dias.txt:48`; `app/ponto/services/espelho.py:267-294`; `app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:224-254`) |
| col727 | 2026-11-10 | 307; `em_aberto` | 0 | **Associação ao dia e classificação divergentes:** espelho atribui ao dia 10 o turno fechado de 11/11, 01:00–07:07, calculando 367−60; mantém classe `em_aberto`. E6 tem zero turnos no dia 10; o motor também aponta `dia_sem_previsto`/`folga_trabalhada`. (`estrutura_dias.txt:49`; `app/ponto/services/espelho.py:764-895,450-460`; `app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:224-254`) |
| col874 | 2026-10-22 | 667; `trabalhou` | 383 | **Corte e pareamento diferentes:** espelho conta 10:00–22:07 menos 60 min; no E6, 10:00 fecha fragmento iniciado em 21/10, e o dia 22 recebe somente 15:45–22:07. (`estrutura_dias.txt:50`; `app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:215-247`) |
| col884 | 2026-11-08 | 679; `rejeitado` | 699 | **Desconto e estado de ausência:** espelho desconta 60 min sem pausa reconhecida; E6 desconta 39,546 min posicionais. Há atestado `rejeitada` cobrindo o dia. (`estrutura_dias.txt:51`; `app/ponto/turnos.py:725-767,2798-2814`; `app/ponto/calculador/nucleo.py:224-247`; `app/ponto/models.py:690-705,743-754`) |
| col909 | 2026-11-11 | 831; `em_aberto` | 245 | **Borda posicional versus cortes:** realizado usa 07:00–23:00 menos 129 min, embora haja um turno fechado e outro aberto. E6 conta apenas 07:00–11:04 e registra `impar=true`, `corte_que_orfanou=true`. (`estrutura_dias.txt:52`; `app/ponto/turnos.py:598-601,707-767`; `app/ponto/calculador/nucleo.py:215-254`) |
| col922 | 2026-11-01 | 1382; `trabalhou` | 68 | **Turnos e pares diferentes:** realizado soma 901+481 min, sem desconto; espelho exibe apenas o turno noturno. E6 atribui ao dia somente o par 22:01–23:09, entre uma saída e uma entrada. (`estrutura_dias.txt:53`; `app/ponto/turnos.py:725-767`; `app/ponto/services/espelho.py:450-460,767`; `app/ponto/calculador/nucleo.py:215-247`) |
| col922 | 2026-11-04 | 676; `trabalhou` | 458 | **Turno adicional no realizado:** soma 218 min da madrugada e 458 min do turno noturno; espelho exibe só o noturno. E6 mantém a madrugada no fragmento iniciado em 03/11 e atribui ao dia 04 apenas o noturno. (`estrutura_dias.txt:54`; `app/ponto/turnos.py:725-767`; `app/ponto/services/espelho.py:450-460,767`; `app/ponto/calculador/nucleo.py:215-247`) |
| col989 | 2026-10-23 | 721; `trabalhou` | 567 | **Pareamento deslocado:** espelho conta entrada 17:59 até saída 06:00 seguinte, sem desconto; E6 pareia a saída 08:32 com a entrada 17:59 e corta antes de 06:00. (`estrutura_dias.txt:55`; `app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:215-247`) |
| col996 | 2026-10-28 | 0; `folga` | 580 | **Saída órfã versus par posicional:** juiz apresenta apenas saída às 21:18, sem entrada, e o espelho não exibe turno; E6 pareia essa saída com a entrada de 29/10 às 06:57. (`estrutura_dias.txt:56`; `app/ponto/services/espelho.py:267-294,450-460`; `app/ponto/calculador/nucleo.py:224-247`) |
| col996 | 2026-11-10 | 443; `em_aberto` | 1026 | **Par adicional no E6:** realizado conta o turno diurno, 533−90; mantém saída órfã às 21:16. E6 acrescenta o par dessa saída com a entrada de 11/11 às 06:59. (`estrutura_dias.txt:57`; `app/ponto/turnos.py:725-767`; `app/ponto/services/espelho.py:267-294`; `app/ponto/calculador/nucleo.py:224-247`) |

3. **Dias com `ERRO`: nenhum segundo o TOTAL informado (`erro=0`); portanto, nenhuma exclusão numérica por ERRO nesta parte.** Arquivo:linha do registro TOTAL: [nao verificado].

## 16/11 14:41 · sessao c91636553c1d · PRIORIDADE-EV-ITEM1 · sonda · origem=Code

1. **TOTAL da parte:** `N=19; ok=19; erro=0; rc_sombra=0`, conforme as linhas finais fornecidas; localização dessas linhas em arquivo:linha **[nao verificado]**. **Tripwire:** disparo em algum dia **[nao verificado]**; `rc_sombra=0` não fornece, sozinho, esse resultado.

2. A classe abaixo é `STATUS.veredito`; os minutos do espelho são `minutos_realizados`. (`app/ponto/services/espelho.py:764-819`) A coluna e6 usa o inteiro conferido contra a linha do universo indicada em cada registro. (`app/ponto/management/commands/e6_oraculo.py:263-269,401`)

| Código, data | Espelho: minutos / classe | e6 do universo | Categoria/causa evidenciada na saída |
|---|---:|---:|---|
| col723, 2026-10-08 | 440 / trabalhou | 345 (`estrutura_dias.txt:20`) | **Desconto de intervalo diferente:** realizado desconta 60 min declarados, sem pausa reconhecida; e6 desconta 155,277 min entre os dois pares posicionais. (`app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`) |
| col746, 2026-09-24 | 289 / trabalhou | 193 (`estrutura_dias.txt:21`) | **Desconto de intervalo diferente:** realizado desconta 60 min declarados; e6 desconta 156,819 min entre os pares. (`app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`) |
| col841, 2026-10-20 | 681 / em_aberto | 522 (`estrutura_dias.txt:22`) | **Agrupamento e pareamento diferentes:** juiz mantém uma saída isolada às 11:00 e o turno 18:46–07:04, com desconto de 57 min; e6 pareia 11:00–18:46 e 21:44–22:41, cortando antes da saída das 07:04. (`app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:215-247`) |
| col854, 2026-10-04 | 718 / trabalhou | 7 (`estrutura_dias.txt:23`) | **Corte do e6:** realizado conta 18:54–06:52, sem desconto; e6 pareia somente 18:54–19:01 e corta antes das 06:52, após um gap de 711,349 min. (`app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:215-247`) |
| col864, 2026-10-12 | 509 / feriado | 449 (`estrutura_dias.txt:24`) | **Intervalo sem desconto no realizado:** nenhuma pausa reconhecida e nenhuma janela declarada usada; e6 desconta 59,804 min. A célula consta como folga, sem marcos. (`app/ponto/turnos.py:707-771`; `app/ponto/calculador/nucleo.py:224-254`; `app/escala/models.py:1652-1701,1718-1724`) |
| col868, 2026-09-22 | 0 / em_aberto | 679 (`estrutura_dias.txt:25`) | **Formação de turno diferente:** juiz tem apenas saída sem entrada e nenhum turno exibido; e6 agrupa quatro batidas entre 18:48 e 07:01 do dia seguinte. O cadastro mostrado prevê jornada diurna 6x1; dono dessa divergência **[nao verificado]**. (`app/ponto/turnos.py:502-567,719`; `app/ponto/services/espelho.py:450-460,767`; `app/ponto/calculador/nucleo.py:224-247`; `app/escala/models.py:103-138,170-175`) |
| col876, 2026-09-21 | 550 / em_aberto | 665 (`estrutura_dias.txt:26`) | **Conjunto de batidas diferente:** realizado conta 18:29–04:39 menos 60 min e mantém a saída das 06:31 sem entrada; e6 também usa 05:36 e conta o segundo par até 06:31. Motivo de 05:36 não aparecer no turno do juiz **[nao verificado]**. (`app/ponto/turnos.py:502-567,725-771`; `app/ponto/calculador/nucleo.py:224-247`) |
| col1021, 2026-11-06 | 841 / folga | 0 (`estrutura_dias.txt:27`) | **Corte que orfanou no e6:** realizado mantém 05:55–19:56, sem desconto; e6 separa as duas batidas em fragmentos unitários, com `impar=true` e `corte_que_orfanou=true`. (`app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:215-254`) |
| col110, 2026-11-12 | 241 / em_aberto | 0 (`estrutura_dias.txt:28`) | **Atribuição ao dia:** juiz atribui a 12/11 o trecho 01:59–07:00 de 13/11 e desconta 60 min; e6 não tem turno atribuído a 12/11. Destino dessas batidas no e6 **[nao verificado]**. (`app/ponto/turnos.py:502-567,719,725-771`; `app/ponto/calculador/nucleo.py:224-254`) |
| col112, 2026-10-21 | 546 / trabalhou | 838 (`estrutura_dias.txt:29`) | **Pareamento noturno no e6:** realizado conta 06:57–17:03 menos 60 min; e6 pareia a saída das 17:03 com a entrada das 07:01 de 22/10. A entrada das 06:57 aparece no fragmento anterior. (`app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:215-247`) |
| col112, 2026-10-22 | 541 / trabalhou | 835 (`estrutura_dias.txt:30`) | **Pareamento noturno no e6:** realizado conta 07:01–17:02 menos 60 min; e6 pareia 17:02 com 06:56 de 23/10. A entrada das 07:01 aparece no fragmento anterior. (`app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:215-247`) |
| col113, 2026-10-30 | 242 / em_aberto | 0 (`estrutura_dias.txt:31`) | **Atribuição ao dia:** juiz mantém saída isolada às 02:00 e conta 03:00–07:02 de 31/10 em 30/10, sem desconto; e6 não tem turno em 30/10. Destino no e6 **[nao verificado]**. (`app/ponto/turnos.py:502-567,719,725-767`; `app/ponto/calculador/nucleo.py:224-254`) |
| col113, 2026-11-01 | 241 / em_aberto | 0 (`estrutura_dias.txt:32`) | **Atribuição ao dia:** juiz mantém saída isolada às 02:01 e conta 03:00–07:01 de 02/11 em 01/11, sem desconto; e6 não tem turno em 01/11. Destino no e6 **[nao verificado]**. (`app/ponto/turnos.py:502-567,719,725-767`; `app/ponto/calculador/nucleo.py:224-254`) |
| col114, 2026-11-08 | 439 / trabalhou | 429 (`estrutura_dias.txt:33`) | **Desconto de intervalo diferente:** realizado desconta 60 min declarados, sem pausa reconhecida; e6 desconta 70,666 min entre os pares. (`app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`) |
| col114, 2026-11-15 | 499 / feriado | 440 (`estrutura_dias.txt:34`) | **Intervalo sem desconto no realizado:** realizado conta o envelope sem pausa reconhecida ou janela declarada; e6 desconta 59,538 min. (`app/ponto/turnos.py:707-771`; `app/ponto/calculador/nucleo.py:224-254`) |
| col130, 2026-10-29 | 335 / trabalhou | 672 (`estrutura_dias.txt:35`) | **Conjunto e pareamento diferentes:** juiz encerra às 01:31 e desconta 60 min; e6 inclui 07:06 e forma os pares 18:56–00:33 e 01:31–07:06. Motivo da exclusão de 07:06 pelo juiz **[nao verificado]**. (`app/ponto/turnos.py:502-567,725-771`; `app/ponto/calculador/nucleo.py:224-247`) |
| col200, 2026-10-29 | 180 / em_aberto | 0 (`estrutura_dias.txt:36`) | **Atribuição ao dia:** juiz conta 02:00–06:00 de 30/10 em 29/10 e desconta 60 min; e6 não tem turno em 29/10. Destino no e6 **[nao verificado]**. (`app/ponto/turnos.py:502-567,719,725-771`; `app/ponto/calculador/nucleo.py:224-254`) |
| col200, 2026-11-02 | 180 / em_aberto | 0 (`estrutura_dias.txt:37`) | **Atribuição ao dia:** juiz mantém uma saída isolada e conta 02:00–06:00 de 03/11 em 02/11, menos 60 min; e6 não tem turno em 02/11. Destino no e6 **[nao verificado]**. (`app/ponto/turnos.py:502-567,719,725-771`; `app/ponto/calculador/nucleo.py:224-254`) |
| col218, 2026-11-14 | 103 / em_aberto | 0 (`estrutura_dias.txt:38`) | **Atribuição ao dia:** juiz mantém saída isolada às 02:17 e conta 03:17–05:00 de 15/11 em 14/11, sem desconto; e6 não tem turno em 14/11. Destino no e6 **[nao verificado]**. (`app/ponto/turnos.py:502-567,719,725-767`; `app/ponto/calculador/nucleo.py:224-254`) |

**Dono ORACULO/SISTEMA/CADASTRO por causa:** **[nao verificado]**; os mecanismos acima não bastam para atribuir responsabilidade definitiva.

3. **Dias com ERRO:** nenhum registrado no `TOTAL` fornecido (`erro=0`); arquivo:linha desse registro **[nao verificado]**. Portanto, não há dia listado para invalidar por ERRO nessa parte; arquivo:linha **[nao verificado]**.

## 16/11 14:38 · sessao af08e3068de7 · PRIORIDADE-EV-ITEM1 · sonda · origem=Code

1. **TOTAL da parte:** `N=19 | ok=19 | erro=0 | rc_sombra=0`, conforme os registros finais fornecidos; arquivo:linha desses registros **[nao verificado]**. **Tripwire:** disparo **[nao verificado]**; não há resultado explícito dele na saída.

2. Na tabela, **minutos do espelho = `minutos_realizados`** e **classe = `STATUS.veredito`**, conforme as referências da sonda: `app/ponto/services/espelho.py:783-819,764-895`. As categorias abaixo descrevem os mecanismos apresentados, sem atribuir dono definitivo **[nao verificado]**.

| Código | Data | Espelho: minutos; classe | e6 do universo | Categoria/causa apontada |
|---|---|---|---:|---|
| col129 | 2026-10-12 | 0; `em_aberto` | 779 | **Pareamento diferente:** juiz recebeu duas saídas sem entrada; e6 pareou a saída de 17:30 com a saída de 06:30 do dia seguinte. `estrutura_dias.txt:1`; `app/ponto/turnos.py:502-567,719`; `app/ponto/calculador/nucleo.py:224-247`. |
| col218 | 2026-10-15 | 105; `em_aberto` | 0 | **Atribuição do turno noturno:** falta entrada das 21h; espelho conta o par 03:15–05:00 do dia 16 no dia 15; e6 não apresenta turno no dia 15. `estrutura_dias.txt:2`; `app/ponto/services/espelho.py:764-895`; `app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:224-254`. |
| col252 | 2026-09-23 | 548; `trabalhou` | 594 | **Desconto de intervalo:** realizado desconta 60 min declarados; pausa de 13,3619 min fica abaixo do piso reconhecido; e6 desconta essa pausa. `estrutura_dias.txt:3`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col252 | 2026-09-24 | 552; `trabalhou` | 598 | **Desconto de intervalo:** realizado desconta 60 min; pausa abaixo do piso de 13,569383 min é descontada pelo e6. `estrutura_dias.txt:4`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col252 | 2026-09-25 | 556; `trabalhou` | 604 | **Desconto de intervalo:** realizado desconta 60 min; pausa abaixo do piso de 12,438 min é descontada pelo e6. `estrutura_dias.txt:5`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col252 | 2026-09-28 | 535; `trabalhou` | 581 | **Desconto de intervalo:** realizado desconta 60 min; pausa abaixo do piso de 14,634152 min é descontada pelo e6. `estrutura_dias.txt:6`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col252 | 2026-10-01 | 545; `trabalhou` | 590 | **Desconto de intervalo:** realizado desconta 60 min; pausa abaixo do piso de 14,443733 min é descontada pelo e6. `estrutura_dias.txt:7`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col252 | 2026-10-02 | 545; `trabalhou` | 593 | **Desconto de intervalo:** realizado desconta 60 min; pausa abaixo do piso de 13,069783 min é descontada pelo e6. `estrutura_dias.txt:8`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col252 | 2026-10-06 | 551; `trabalhou` | 597 | **Desconto de intervalo:** realizado desconta 60 min; pausa abaixo do piso de 14,038883 min é descontada pelo e6. `estrutura_dias.txt:9`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col252 | 2026-10-09 | 544; `trabalhou` | 592 | **Desconto de intervalo:** realizado desconta 60 min; pausa abaixo do piso de 11,989283 min é descontada pelo e6. `estrutura_dias.txt:10`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col369 | 2026-10-12 | 476; `feriado` | 416 | **Intervalo não reconhecido no realizado:** célula de folga sem marcos; batidas internas E–S, nenhuma pausa reconhecida e nenhum desconto. e6 desconta 59,582783 min entre as batidas internas. `estrutura_dias.txt:11`; `app/escala/models.py:1652-1701`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col37 | 2026-10-05 | 241; `em_aberto` | 0 | **Atribuição do turno noturno:** falta entrada das 19h; juiz tem saída isolada às 02h e par 03:00–07:01 do dia 6 atribuído ao dia 5; e6 não apresenta turno no dia 5. `estrutura_dias.txt:12`; `app/ponto/services/espelho.py:764-895`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:224-254`. |
| col37 | 2026-10-09 | 241; `em_aberto` | 0 | **Atribuição do turno noturno:** falta entrada das 19h; saída isolada às 02h e par 03:00–07:01 do dia 10 atribuído ao dia 9; e6 não apresenta turno no dia 9. `estrutura_dias.txt:13`; `app/ponto/services/espelho.py:764-895`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:224-254`. |
| col382 | 2026-09-30 | 364; `trabalhou` | 0 | **Atribuição do turno noturno com pausa reconhecida:** espelho atribui 01:01–07:50 do dia 1 ao dia 30 e calcula 409−45=364 min; e6 não apresenta turno no dia 30. `estrutura_dias.txt:14`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col451 | 2026-09-23 | 707; `em_aberto` | 718 | **Pareamento diferente:** espelho conta 18:58–06:45 do dia seguinte; e6 pareia a saída isolada de 07h com a entrada de 18:58 e corta antes da saída seguinte. `estrutura_dias.txt:15`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:215-247`. |
| col451 | 2026-09-25 | 705; `em_aberto` | 718 | **Pareamento diferente:** espelho conta 19:00–06:45 do dia seguinte; e6 pareia a saída isolada de 07:02 com a entrada de 19h e corta antes da saída seguinte. `estrutura_dias.txt:16`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:215-247`. |
| col478 | 2026-09-25 | 719; `trabalhou` | 8 | **Entrada adicional e corte do e6:** espelho usa 18:57–06:56, com a entrada de 19:04 marcada `X`; e6 pareia as duas entradas, obtém 7,766517 min e corta antes da saída. `estrutura_dias.txt:17`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:215-247`. |
| col515 | 2026-10-09 | 244; `em_aberto` | 0 | **Atribuição do turno noturno:** falta entrada das 19h; saída isolada às 02h e par 02:59–07:03 do dia 10 atribuído ao dia 9; e6 não apresenta turno no dia 9. `estrutura_dias.txt:18`; `app/ponto/services/espelho.py:764-895`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:224-254`. |
| col515 | 2026-10-11 | 245; `em_aberto` | 0 | **Atribuição do turno noturno:** falta entrada das 19h; saída isolada às 01:50 e par 03:00–07:05 do dia 12 atribuído ao dia 11; e6 não apresenta turno no dia 11. `estrutura_dias.txt:19`; `app/ponto/services/espelho.py:764-895`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:224-254`. |

3. **Dias com ERRO: nenhum**, conforme `TOTAL|parte=1/3|dias=19|ok=19|erro=0`; arquivo:linha desse registro **[nao verificado]**. Portanto, nenhuma exclusão de número por ERRO foi indicada nessa parte; arquivo:linha **[nao verificado]**.

## 16/11 14:36 · sessao a12a65a6e084 · PRIORIDADE-EV-ITEM6 · sonda · origem=Code

[nao verificado]
```text
PEND|col373|2026-08-04|08/2026|emp2
PEND|col60|2026-06-23|07/2026|emp3
PEND|col152|2026-06-23|07/2026|emp3
PEND|col152|2026-06-29|07/2026|emp3
PEND|col888|2026-08-28|09/2026|emp2
PEND|col920|2026-09-02|09/2026|emp2
PEND|col616|2026-09-05|09/2026|emp2
PEND|col920|2026-09-04|09/2026|emp2
PEND|col727|2026-09-04|09/2026|emp2
PEND|col439|2026-09-03|09/2026|emp2
PEND|col212|2026-08-26|09/2026|emp2
PEND|col112|2026-09-06|09/2026|emp3
PEND|col920|2026-09-06|09/2026|emp2
PEND|col926|2026-09-07|09/2026|emp2
PEND|col152|2026-09-07|09/2026|emp3
PEND|col236|2026-09-07|09/2026|emp2
PEND|col363|2026-09-07|09/2026|emp2
PEND|col358|2026-09-07|09/2026|emp2
PEND|col928|2026-09-07|09/2026|emp2
PEND|col878|2026-09-07|09/2026|emp2
PEND|col507|2026-09-07|09/2026|emp2
PEND|col49|2026-09-07|09/2026|emp3
PEND|col112|2026-09-08|09/2026|emp3
PEND|col920|2026-09-08|09/2026|emp2
PEND|col616|2026-09-07|09/2026|emp2
PEND|col148|2026-09-10|09/2026|emp3
PEND|col112|2026-09-10|09/2026|emp3
PEND|col920|2026-09-10|09/2026|emp2
PEND|col616|2026-09-09|09/2026|emp2
PEND|col920|2026-09-12|09/2026|emp2
PEND|col616|2026-09-11|09/2026|emp2
PEND|col862|2026-09-13|09/2026|emp2
PEND|col242|2026-09-13|09/2026|emp2
PEND|col901|2026-09-14|09/2026|emp2
PEND|col920|2026-08-27|09/2026|emp2
PEND|col920|2026-08-29|09/2026|emp2
PEND|col920|2026-08-31|09/2026|emp2
PEND|col207|2026-09-05|09/2026|emp2
PEND|col869|2026-09-06|09/2026|emp2
PEND|col869|2026-09-13|09/2026|emp2
PEND|col899|2026-09-05|09/2026|emp2
PEND|col903|2026-09-14|09/2026|emp2
PEND|col129|2026-08-30|09/2026|emp3
PEND|col129|2026-09-08|09/2026|emp3
PEND|col129|2026-09-13|09/2026|emp3
PEND|col129|2026-09-15|09/2026|emp3
PEND|col382|2026-09-03|09/2026|emp2
PEND|col491|2026-08-27|09/2026|emp2
PEND|col382|2026-09-10|09/2026|emp2
PEND|col382|2026-09-17|09/2026|emp2
PEND|col616|2026-09-17|09/2026|emp2
PEND|col901|2026-09-17|09/2026|emp2
PEND|col572|2026-09-18|09/2026|emp2
PEND|col648|2026-09-15|09/2026|emp2
PEND|col148|2026-09-13|09/2026|emp3
PEND|col902|2026-09-13|09/2026|emp2
PEND|col824|2026-09-15|09/2026|emp2
PEND|col824|2026-09-19|09/2026|emp2
PEND|col902|2026-09-19|09/2026|emp2
PEND|col499|2026-09-19|09/2026|emp2
PEND|col148|2026-09-19|09/2026|emp3
PEND|col822|2026-09-20|09/2026|emp2
PEND|col572|2026-09-20|09/2026|emp2
PEND|col491|2026-09-20|09/2026|emp2
PEND|col902|2026-09-20|09/2026|emp2
PEND|col148|2026-09-20|09/2026|emp3
PEND|col491|2026-09-21|10/2026|emp2
PEND|col616|2026-09-21|10/2026|emp2
PEND|col824|2026-09-21|10/2026|emp2
PEND|col903|2026-09-21|10/2026|emp2
PEND|col704|2026-09-21|10/2026|emp2
PEND|col857|2026-09-22|10/2026|emp4
PEND|col418|2026-09-22|10/2026|emp2
PEND|col245|2026-09-22|10/2026|emp2
PEND|col203|2026-09-22|10/2026|emp2
PEND|col616|2026-09-23|10/2026|emp2
PEND|col824|2026-09-23|10/2026|emp2
PEND|col859|2026-09-23|10/2026|emp2
PEND|col951|2026-09-24|10/2026|emp2
PEND|col616|2026-09-25|10/2026|emp2
PEND|col824|2026-09-05|09/2026|emp2
PEND|col824|2026-09-25|10/2026|emp2
PEND|col369|2026-09-25|10/2026|emp2
PEND|col885|2026-09-26|10/2026|emp2
PEND|col616|2026-09-27|10/2026|emp2
PEND|col242|2026-09-27|10/2026|emp2
PEND|col457|2026-09-27|10/2026|emp2
PEND|col155|2026-09-27|10/2026|emp3
PEND|col373|2026-09-27|10/2026|emp2
PEND|col666|2026-09-27|10/2026|emp2
PEND|col859|2026-09-02|09/2026|emp2
PEND|col859|2026-09-16|09/2026|emp2
PEND|col784|2026-09-28|10/2026|emp2
PEND|col857|2026-09-29|10/2026|emp4
PEND|col373|2026-09-29|10/2026|emp2
PEND|col89|2026-09-30|10/2026|emp3
PEND|col202|2026-10-02|10/2026|emp2
PEND|col876|2026-10-03|10/2026|emp3
PEND|col373|2026-10-06|10/2026|emp2
PEND|col131|2026-10-08|10/2026|emp3
PEND|col974|2026-10-10|10/2026|emp2
PEND|col155|2026-10-11|10/2026|emp3
PEND|col924|2026-10-10|10/2026|emp2
```

| Competência | Linhas PEND |
|---|---:|
| 07/2026 | 3 [nao verificado] |
| 08/2026 | 1 [nao verificado] |
| 09/2026 | 65 [nao verificado] |
| 10/2026 | 34 [nao verificado] |
| 11/2026 | 0 [nao verificado] |

[nao verificado]
```text
TOTAL_DIAS 253 TOTAL_PENDENTE 103 ERROS 0
FIM
```

## 16/11 14:34 · sessao 34889e7d5722 · PRIORIDADE-EV-ITEM6 · sonda · origem=Code

Referência: somente o bloco colado; sem arquivo:linha disponível, os resultados levam [nao verificado].

1. **Marcadores presentes:** `READ_ONLY on`, `ERROS 0` e `FIM`. Todas as linhas O199 têm `erro=0`; a saída passa pelo critério solicitado de integridade. [nao verificado]

2. **Conferência dos valores:** [nao verificado]

| competencia | empresa | exportada | dias | ja_folga | pendente | colabs_pendentes | referência |
|---|---:|:---:|---:|---:|---:|---:|---|
| 07/2026 | 3 | s | 3 | 0 | 3 | 2 | [nao verificado] |
| 08/2026 | 2 | s | 6 | 5 | 1 | 1 | [nao verificado] |
| 09/2026 | 2 | s | 91 | 39 | 52 | 29 | [nao verificado] |
| 09/2026 | 3 | s | 19 | 6 | 13 | 5 | [nao verificado] |
| 10/2026 | 2 | n | 74 | 47 | 27 | 20 | [nao verificado] |
| 10/2026 | 3 | n | 17 | 12 | 5 | 4 | [nao verificado] |
| 10/2026 | 4 | n | 3 | 1 | 2 | 1 | [nao verificado] |
| 11/2026 | 1 | n | 1 | 1 | 0 | 0 | [nao verificado] |
| 11/2026 | 2 | n | 29 | 29 | 0 | 0 | [nao verificado] |
| 11/2026 | 3 | n | 8 | 8 | 0 | 0 | [nao verificado] |
| 11/2026 | 4 | n | 2 | 2 | 0 | 0 | [nao verificado] |
| **TOTAL** | — | — | **253** | **150** | **103** | **62** | [nao verificado] |

**Os totais fecham:** soma de `dias=253`, soma de `pendente=103` e `150+103=253`. Em todas as linhas, `dias=ja_folga+pendente`. Os 62 são a soma de `colabs_pendentes` das linhas; quantidade de colaboradores únicos não está demonstrada. [nao verificado]

3. **Pendências e Corte C:** [nao verificado]

- **Exportadas (`s`): 69 dias pendentes**, destinados à lista do DP pelo critério solicitado. [nao verificado]
- **Abertas pelo flag (`n`): 34 dias pendentes**, todos em **10/2026**. [nao verificado]
- **11/2026: zero pendentes O199**; portanto, o recorte autorizado do Corte C apresenta **zero dias O199 para apply**, que continua condicionado ao `!` de escala. [nao verificado]

**Divergência:** o anexo chama 10 de competência paga, enquanto a saída marca 10 como `exportada=n`. Os 34 dias de outubro não podem ser considerados autorizados para apply pelo Corte C, limitado à 11. A saída também não apresenta medição identificável de `SEM-CELULA-11-DEVE-TER-CELULA (col305)`. [nao verificado]

## 16/11 14:33 · sessao fbd083b15a88 · ESPELHO-VERDADE-FECHO-V0 · leitura · origem=Code

A função é `espelho_do_colab`: a tela a chama diretamente e o PDF consome seus dias (`app/ponto/views.py:160-162`; `app/relatorios/pdf_espelho.py:596-603`). O patch abaixo intercepta somente as atribuições ao cache do catálogo, preservando sua leitura e os turnos reais (`app/ponto/catalogo/ausencias.py:275-281`).

```python
# OBRA: ESPELHO-VERDADE-FECHO-V0 — R3
# Arvore solicitada: 3580b35e028267d85318cb75fd5b3d0c40e04a00
# Texto para o envelope JA inicializado em SOMBRA/juliani.
# Execucao, desempenho e compatibilidade do script: [nao verificado].
# Auxiliares deste script, biblioteca padrao, sqlparse e implementacoes
# internas do Django fora da copia: [nao verificado].
#
# PARTE recorta estrutura_dias.txt:1-57 na ordem recebida: 19/19/19.
PARTE = 1

import sys
sys.dont_write_bytecode = True

import datetime as dt
import decimal
import json
import logging
import re
from contextlib import ExitStack, redirect_stderr, redirect_stdout
from unittest.mock import patch

NV = "[nao verificado]"
SAIDA = sys.stdout

# Mesma data civil do R1 recebido; instante/snapshot original: [nao verificado].
# O instante AGORA sera real, unico por execucao e publicado. [nao verificado]
HOJE = dt.date(2026, 11, 16)

# Transcricao dos campos identificadores e numeros de estrutura_dias.txt:1-57.
# Inclui col1021 e col909: o universo aqui e 57, nao os 55 do R2.
DADO = """col129|10/2026|2026-10-12|0|779
col218|10/2026|2026-10-15|105|0
col252|10/2026|2026-09-23|548|594
col252|10/2026|2026-09-24|552|598
col252|10/2026|2026-09-25|556|604
col252|10/2026|2026-09-28|535|581
col252|10/2026|2026-10-01|545|590
col252|10/2026|2026-10-02|545|593
col252|10/2026|2026-10-06|551|597
col252|10/2026|2026-10-09|544|592
col369|10/2026|2026-10-12|476|416
col37|10/2026|2026-10-05|241|0
col37|10/2026|2026-10-09|241|0
col382|10/2026|2026-09-30|364|0
col451|10/2026|2026-09-23|707|718
col451|10/2026|2026-09-25|705|718
col478|10/2026|2026-09-25|719|8
col515|10/2026|2026-10-09|244|0
col515|10/2026|2026-10-11|245|0
col723|10/2026|2026-10-08|440|345
col746|10/2026|2026-09-24|289|193
col841|10/2026|2026-10-20|681|522
col854|10/2026|2026-10-04|718|7
col864|10/2026|2026-10-12|509|449
col868|10/2026|2026-09-22|0|679
col876|10/2026|2026-09-21|550|665
col1021|11/2026|2026-11-06|841|0
col110|11/2026|2026-11-12|241|0
col112|11/2026|2026-10-21|546|838
col112|11/2026|2026-10-22|541|835
col113|11/2026|2026-10-30|242|0
col113|11/2026|2026-11-01|241|0
col114|11/2026|2026-11-08|439|429
col114|11/2026|2026-11-15|499|440
col130|11/2026|2026-10-29|335|672
col200|11/2026|2026-10-29|180|0
col200|11/2026|2026-11-02|180|0
col218|11/2026|2026-11-14|103|0
col220|11/2026|2026-11-10|337|430
col252|11/2026|2026-11-09|551|597
col252|11/2026|2026-11-10|551|598
col252|11/2026|2026-11-11|550|598
col252|11/2026|2026-11-13|551|597
col296|11/2026|2026-11-02|345|0
col37|11/2026|2026-11-14|241|0
col382|11/2026|2026-11-10|418|0
col516|11/2026|2026-10-23|361|0
col516|11/2026|2026-10-28|372|0
col727|11/2026|2026-11-10|307|0
col874|11/2026|2026-10-22|667|383
col884|11/2026|2026-11-08|679|699
col909|11/2026|2026-11-11|831|245
col922|11/2026|2026-11-01|1382|68
col922|11/2026|2026-11-04|676|458
col989|11/2026|2026-10-23|721|567
col996|11/2026|2026-10-28|0|580
col996|11/2026|2026-11-10|443|1026"""

TODOS = []
for numero, texto in enumerate(DADO.splitlines(), 1):
    codigo, competencia, data, esp_min, e6_min = texto.split("|")
    TODOS.append((numero, codigo, competencia, data, int(esp_min), int(e6_min)))

if PARTE not in (1, 2, 3):
    raise ValueError("ParteInvalida")
if len(TODOS) != 57 or len({r[1:4] for r in TODOS}) != 57:
    raise RuntimeError("UniversoIncompativel")
UNIVERSO = TODOS[(PARTE - 1) * 19:PARTE * 19]
N = len(UNIVERSO)
if N != 19:
    raise RuntimeError("FatiaIncompativel")


def imprimir(*partes):
    print(*partes, file=SAIDA, flush=True)


class Descartar:
    def write(self, texto):
        return len(texto)

    def flush(self):
        return None


class escrita_nao_interceptada(RuntimeError):
    pass


class CacheSomenteLeitura:
    """Proxy de escrita; a leitura vem INTEGRALMENTE do objeto original."""

    def __init__(self, original):
        self.original = original
        self.suprimidas = 0

    def __getitem__(self, chave):
        return self.original[chave]

    def __setitem__(self, chave, valor):
        if chave not in ("v", "t"):
            raise RuntimeError("EscritorDeCacheIncompativel")
        self.suprimidas += 1
        # SOMENTE as atribuicoes sao absorvidas; nenhum valor e inventado.
        # Escritor nominal: ponto.catalogo.ausencias._CACHE.__setitem__
        # nos sitios app/ponto/catalogo/ausencias.py:279-280.
        # vigente() ainda calcula v pela fonte real (:278) e retorna v (:281).


def classe_segura(exc):
    nome = type(exc).__name__
    return nome if re.fullmatch(r"[A-Za-z_][A-Za-z0-9_]{0,79}", nome) else "Excecao"


def sonda():
    ok = erro = 0
    feitos = set()
    guarda = None

    try:
        with (
            patch.object(logging.Logger, "handle", lambda *a, **k: None),
            redirect_stdout(Descartar()),
            redirect_stderr(Descartar()),
        ):
            from django.apps import apps
            from django.conf import settings
            from django.db import connection, connections, transaction
            from django.db.models import Q
            from django.utils import timezone
            import sqlparse
            from sqlparse import tokens as SQLTOK

            # Mesmo molde de ambiente do anexo r2_script_p1.py.
            # Configuracao: app/config/settings/sombra.py:49-50,65.
            # Envelope efetivo: verificado apenas quando este texto executar.
            if (
                not apps.ready
                or settings.SETTINGS_MODULE != "config.settings.sombra"
                or not settings.BANCO_DE_ENSAIO
                or connection.settings_dict["NAME"] != "sombra"
                or connection.schema_name != "juliani"
                or connection.in_atomic_block
                or not connection.get_autocommit()
            ):
                raise RuntimeError("AmbienteIncompativel")

            class GuardaSQL:
                def __init__(self):
                    self.tentou = False
                    self.permite_set_inicial = True

                def bloquear(self):
                    self.tentou = True
                    raise escrita_nao_interceptada()

                def __call__(self, execute, sql, params, many, context):
                    # execute_wrapper / sqlparse: implementacoes [nao verificado].
                    # O flag e aderente ao dia: except Exception internos nao
                    # podem converter uma tentativa de escrita em dia "ok".
                    # Exemplo de captura interna:
                    # app/ponto/catalogo/ausencias.py:246-254.
                    if context["connection"].alias != "default":
                        self.bloquear()

                    comandos = [
                        s for s in sqlparse.parse(str(sql))
                        if str(s).strip()
                    ]
                    if len(comandos) != 1:
                        self.bloquear()

                    partes = []
                    for token in comandos[0].flatten():
                        if token.is_whitespace:
                            continue
                        if token.ttype in SQLTOK.Comment:
                            continue
                        if token.ttype in SQLTOK.Literal.String:
                            continue
                        # Identificador entre aspas nao e comando.
                        if token.value.startswith('"'):
                            continue
                        partes.append(token.value)
                    plano = " ".join(partes).upper().strip().rstrip(";").strip()

                    # Inclui DML dentro de WITH e SELECT ... FOR UPDATE/SHARE.
                    # TRUNCATE/DDL/COPY/CALL tambem nao sao leitura autorizada.
                    if re.search(
                        r"\b(INSERT|UPDATE|DELETE|LOCK|MERGE|TRUNCATE|"
                        r"CREATE|ALTER|DROP|GRANT|REVOKE|COPY|CALL|DO|"
                        r"VACUUM|ANALYZE|REFRESH|REINDEX|CLUSTER|SHARE)\b",
                        plano,
                    ):
                        self.bloquear()
                    if re.search(r"\bPG_(?:TRY_)?ADVISORY_", plano):
                        self.bloquear()

                    if plano == "SET TRANSACTION READ ONLY":
                        if not self.permite_set_inicial:
                            self.bloquear()
                    elif not re.match(
                        r"^(SELECT|WITH|SHOW|SAVEPOINT|RELEASE|ROLLBACK)\b",
                        plano,
                    ):
                        self.bloquear()

                    return execute(sql, params, many, context)

            guarda = GuardaSQL()

            def serializar(valor):
                if isinstance(valor, dt.datetime):
                    # Mesmo uso local do E6:
                    # app/ponto/management/commands/e6_oraculo.py:152.
                    return timezone.localtime(valor).isoformat()
                if isinstance(valor, (dt.date, dt.time)):
                    return valor.isoformat()
                if isinstance(valor, decimal.Decimal):
                    return float(valor)
                # Nunca str(modelo), repr(modelo), nome ou texto de excecao.
                raise TypeError("TipoDeSaidaNaoAutorizado")

            def local(valor):
                return None if valor is None else timezone.localtime(valor).isoformat()

            def hora(valor):
                if valor is None or isinstance(valor, dt.time):
                    return valor
                if isinstance(valor, str) and re.fullmatch(
                    r"\d{2}:\d{2}(?::\d{2}(?:\.\d+)?)?", valor
                ):
                    return dt.time.fromisoformat(valor).isoformat()
                return NV

            def batida(b):
                if b is None:
                    return None
                # Campos: app/ponto/models.py:23-26,67.
                # Papeis em memoria: app/ponto/turnos.py:890-909.
                return {
                    "id": b.pk,
                    "hora_local": local(b.timestamp),
                    "tipo": b.tipo,
                    "is_intervalo": b.is_intervalo,
                    "retratada_em": local(b.retratada_em),
                    "papel_ata": b.__dict__.get("_papel_ata", NV),
                    "intra_ata": b.__dict__.get("_intra_ata", NV),
                    "intra_dur": b.__dict__.get("_intra_dur", NV),
                    "intra_declarado": b.__dict__.get("_intra_declarado", NV),
                    "eco_flush": b.__dict__.get("_eco_flush", NV),
                }

            def alertas_sem_nomes(alertas):
                # Publica categorias e numeros, nunca o texto livre.
                # Campo real: app/ponto/motor_calculo_v2.py:253.
                # Exemplos de produtores:
                # :1484-1486,2144-2147,2220-2231,2485-2490,
                # :2561-2563,2754,2779-2782,2814-2828,3188-3190.
                prefixos = (
                    ("DIA SEM JORNADA PREVISTA", "dia_sem_previsto"),
                    ("Entrada ", "horario_deslocado"),
                    ("Atraso:", "atraso"),
                    ("Saída antecipada:", "saida_antecipada"),
                    ("FOLGA TRABALHADA", "folga_trabalhada"),
                    ("Turno aberto", "turno_aberto"),
                    ("Período aberto", "periodo_aberto"),
                    ("Periodo aberto", "periodo_aberto"),
                    ("Intrajornada suprimida", "intra_suprimida"),
                    ("Feriado em", "feriado"),
                    ("HE semanal:", "he_semanal"),
                )
                saida = []
                for texto in alertas or ():
                    if not isinstance(texto, str):
                        raise TypeError("AlertaIncompativel")
                    codigo = next(
                        (cod for inicio, cod in prefixos if texto.startswith(inicio)),
                        "texto_livre_suprimido",
                    )
                    saida.append({
                        "codigo": codigo,
                        "numeros_do_texto": re.findall(
                            r"\d+(?:[.,:/]\d+)*", texto
                        ),
                    })
                return saida

            with ExitStack() as pilha:
                # Tripwire instalado antes dos imports de codigo da aplicacao.
                pilha.enter_context(connection.execute_wrapper(guarda))
                for alias in connections:
                    if alias != "default":
                        pilha.enter_context(connections[alias].execute_wrapper(guarda))

                with transaction.atomic():
                    with connection.cursor() as cursor:
                        cursor.execute("SET TRANSACTION READ ONLY")
                        guarda.permite_set_inicial = False
                        cursor.execute(
                            "SELECT current_database(), current_user, "
                            "current_schema(), "
                            "current_setting('transaction_read_only')"
                        )
                        if cursor.fetchone() != ("sombra", "leitor", "juliani", "on"):
                            raise RuntimeError("BancoPapelOuTenantIncompativel")

                    from ponto.catalogo import ausencias as catalogo
                    from ponto.services import espelho as esp
                    from ponto import turnos as geometria
                    from ponto.management.commands.e6_oraculo import (
                        minutos_do_espelho,
                        minutos_do_oraculo,
                    )
                    from ponto.janelas import janela_fechamento
                    from escala.alimentacao import (
                        q_vinculo_cobre_a_data,
                        vinculo_cobre_a_data,
                    )

                    # Modelos: app/colaboradores/models.py:283;
                    # app/escala/models.py:871,1635;
                    # app/ponto/models.py:115,660.
                    Colaborador = apps.get_model("colaboradores", "Colaborador")
                    CelulaDia = apps.get_model("escala", "CelulaDia")
                    EC = apps.get_model("escala", "EscalaColaborador")
                    Ausencia = apps.get_model("ponto", "Ausencia")
                    Justificativa = apps.get_model("ponto", "Justificativa")

                    # Falha explicitamente se os sitios usados pelo observador
                    # deixarem de corresponder ao codigo desta copia.
                    for funcao, linha in (
                        (esp.espelho_do_colab, 1194),
                        (esp._realizado_do_dia_tela, 267),
                        (geometria.realizado_do_dia, 774),
                        (geometria.realizado_dos_turnos, 707),
                        (minutos_do_oraculo, 42),
                    ):
                        if funcao.__code__.co_firstlineno != linha:
                            raise RuntimeError("SitioDaArvoreIncompativel")

                    # Patch NOMINAL apenas do destino das atribuicoes :279-280.
                    # _CACHE original continua sendo a fonte de todos os reads.
                    # Nao troca vigente(), _do_banco(), catalogo ou turnos.
                    cache = CacheSomenteLeitura(catalogo._CACHE)
                    pilha.enter_context(patch.object(catalogo, "_CACHE", cache))

                    # Os append de :1163,1906,1917 sao listas LOCAIS Python.
                    # Fontes: SELECT agregado e leitor de marcos; nao precisam
                    # GRANT e nao sao removidos:
                    # app/ponto/turnos.py:1154-1164,1901-1918.

                    AGORA = timezone.now()
                    if guarda.tentou:
                        raise escrita_nao_interceptada()

                    def ler_dia(registro):
                        numero, codigo, comp, texto_dia, esp_r1, e6_r1 = registro
                        cid = int(codigo[3:])
                        dia = dt.date.fromisoformat(texto_dia)
                        mes, ano = map(int, comp.split("/"))
                        chave = dia.isoformat()
                        linhas = []
                        inicio_cache = cache.suprimidas

                        def fato(nome, ref, **campos):
                            campos["ref"] = ref
                            linhas.append(
                                "DET|%s|%s|%s|%s" % (
                                    codigo, texto_dia, nome,
                                    json.dumps(
                                        campos, default=serializar,
                                        ensure_ascii=True, allow_nan=False,
                                        separators=(",", ":"),
                                    ),
                                )
                            )

                        c = Colaborador.objects.select_related("empresa").get(pk=cid)
                        # Funcao real: app/ponto/janelas.py:54-57.
                        ini, fim = janela_fechamento(mes, ano, c.empresa)
                        if not ini <= dia <= fim or dia > HOJE:
                            raise RuntimeError("DiaForaDaJanela")

                        captura = {
                            "raizes": [],
                            "corpos": [],
                            "descontos": [],
                            "oraculos": [],
                            "cortes_e6": [],
                            "pares_e6": [],
                        }
                        lendo_e6 = False

                        # Observacao de frames: nao altera argumentos, retornos,
                        # regras, pareamento ou objetos da aplicacao.
                        def observar_raiz(frame, evento, retorno):
                            if evento == "return" and retorno is not None:
                                L = frame.f_locals
                                captura["raizes"].append({
                                    "resultado": retorno,
                                    "ec": L.get("ec"),
                                    "celulas": L["celulas"],
                                    "turnos": tuple(
                                        t for t in L["turnos_juiz"]
                                        if t.data_turno == dia
                                    ),
                                })
                            return observar_raiz

                        def observar_corpo(frame, evento, retorno):
                            L = frame.f_locals
                            # Le pausas/bruto/fim JA decididos pelo corpo real,
                            # imediatamente antes de liquido = bruto - pausas:
                            # app/ponto/turnos.py:756-767.
                            if evento == "line" and frame.f_lineno == 767:
                                captura["descontos"].append({
                                    "modo": "turno",
                                    "entrada": L["t"].entrada.timestamp,
                                    "fim_contado": L["fim"].timestamp,
                                    "bruto_min": L["bruto"],
                                    "pausa_descontada_min": L["pausas"],
                                    "intervalos_reconhecidos": tuple(L["_itv_t"]),
                                })
                            if evento == "return" and retorno is not None:
                                captura["corpos"].append({
                                    "resultado": retorno,
                                    "livre": L["livre"],
                                    "intervalo"

[truncado em 20000 de 54344 caracteres]

## 16/11 14:31 · sessao b6bb56501dae · PRIORIDADE-EV-ITEM4 · sonda · origem=Code

Os números abaixo são os da saída fornecida; os ponteiros são os nela indicados. **Fatos persistidos compartilhados não demonstram quantidade de chamadas de cada endpoint.** (`app/ponto/portas/he.py:94-99`; `app/chamados/models.py:2325`)

- **TOTAL:** informado **55**, contado **55**: 26 `USO`, 19 `ERRO`, 8 `RECUSA`, 2 `EVIDENCIA`. Fecha na transcrição; correspondência com a execução integral: [nao verificado].
- **ERRO da própria sonda:** [nao verificado]. As 19 linhas `ERRO` apresentadas descrevem cobertura ou métricas: 17 de `5xx_traceback` desconhecidos e duas contagens restritas de zero; não comprovam falha de execução da sonda. (`app/config/settings/saas.py:173-181`; `app/ponto/services/he_pendente_lavrado.py:94-100,177-189`; `app/chamados/services/fio.py:713-714`)
- **rc:** [nao verificado]. Porta invalidada por erro da própria sonda: [nao verificado].

Na tabela, **L** significa: atribuição HTTP, 5xx/traceback e quantidade de recusas indevidas **[nao verificado]**. Os zeros persistidos abaixo têm cobertura restrita e não demonstram zero erros HTTP. (`app/config/settings/saas.py:173-181`)

| Obra | Porta | Uso n | Erro/recusa persistidos n | O que a sonda não prova |
|---|---|---:|---|---|
| O-JANELA-HE | `relatorios:index` — `app/relatorios/views.py:33-51` | [nao verificado] | [nao verificado] | L |
| O-JANELA-HE | `ponto:gestao_he` — `app/ponto/views.py:932-966` | [nao verificado] | **0**, somente apuração nas fotos atuais — `app/ponto/services/he_pendente_lavrado.py:94-100,177-189` | L; fotos sobrescritas não constituem histórico HTTP |
| O-JANELA-HE | `ponto:decidir_he` — `app/ponto/portas/he.py:94-99` | [nao verificado] | [nao verificado]; recusa do POST não persistida — `app/ponto/views.py:535-537,621-629` | L; separação do domínio compartilhado |
| O-JANELA-HE | `ponto:recusar_he_em_lote` — `app/ponto/portas/he.py:263-268` | [nao verificado] | [nao verificado]; recusa do POST não persistida — `app/ponto/views.py:535-537,621-629` | L; separação do domínio compartilhado |
| O-JANELA-HE | `ponto:gestao_he_pdf` — `app/ponto/views.py:870-878` | [nao verificado] | [nao verificado] | L; **16** evidências candidatas por assinatura não provam chamadas HTTP |
| BO-HE-RECUSAR-LOTE-500 | `ponto:gestao_he` — `app/ponto/views.py:932-966` | [nao verificado] | [nao verificado] | L |
| BO-HE-RECUSAR-LOTE-500 | `ponto:decidir_he` — `app/ponto/portas/he.py:94-99` | [nao verificado] | [nao verificado]; recusa do POST não persistida — `app/ponto/views.py:535-537,621-629` | L; separação do domínio compartilhado |
| BO-HE-RECUSAR-LOTE-500 | `ponto:recusar_he_em_lote` — `app/ponto/portas/he.py:263-268` | [nao verificado] | [nao verificado]; recusa do POST não persistida — `app/ponto/views.py:535-537,621-629` | L; separação do domínio compartilhado |
| REABRIR-NAO-DESTROI | `chamados:reabrir_disputa_supervisao` — `app/chamados/views.py:1435-1456` | [nao verificado] | [nao verificado] | L |
| REABRIR-NAO-DESTROI | `chamados:fio_colaborador` — `app/chamados/views.py:667-674` | [nao verificado] | [nao verificado] | L |
| REABRIR-NAO-DESTROI | `chamados:modal_fio` — `app/chamados/views.py:877-880` | [nao verificado] | [nao verificado] | L; **1.630** notas marcadas lidas não são exclusivas do modal — `app/chamados/services/nota.py:126-133` |
| REABRIR-NAO-DESTROI | `chamados:modal_fio@corrigir_resposta` — `app/chamados/services/acoes_disputa.py:375-383` | [nao verificado] | [nao verificado] | L |
| REABRIR-NAO-DESTROI | `chamados:modal_fio@regularizar_disputa` — `app/chamados/services/fio.py:713-714` | [nao verificado] | [nao verificado] por endpoint | L |
| REABRIR-NAO-DESTROI | `chamados:detalhe_local` — `app/chamados/views.py:469-475` | [nao verificado] | [nao verificado] | L |
| REABRIR-NAO-DESTROI | `chamados:solicitar_correcao_disputa` — `app/chamados/models.py:2318-2325` | [nao verificado] | [nao verificado] | L; separação HTML/API |
| REABRIR-NAO-DESTROI | `api:api_solicitar_correcao_disputa` — `app/api/views.py:2686` | [nao verificado] | [nao verificado] | L; separação HTML/API |
| REABRIR-NAO-DESTROI | `chamados:cobrar_chamado` — `app/chamados/views_cobrar.py:54-56` | [nao verificado] | [nao verificado] | L; atribuição exclusiva das notas |
| O-JANELA-HE | `dominio:decidir_he[compartilhada]` — `app/ponto/portas/he.py:94-99` | **72 fatos** | **10 decisões “não”**; erro/recusa indevida [nao verificado] | L; distribuição entre endpoints |
| BO-HE-RECUSAR-LOTE-500 | `dominio:decidir_he[compartilhada]` — `app/ponto/portas/he.py:94-99` | **71 fatos** | **10 decisões “não”**; erro/recusa indevida [nao verificado] | L; distribuição entre endpoints |
| REABRIR-NAO-DESTROI | `dominio:reabrir_para_correcao` — `app/chamados/services/disputa_emissao.py:1631-1648` | **471 perguntas afetadas** | [nao verificado] | L; número de chamadas e atribuição por endpoint |
| REABRIR-NAO-DESTROI | `dominio:recusar_pergunta` — `app/chamados/services/validacao.py:628-629` | [nao verificado] | **9 recusas humanas de resposta**; caráter indevido [nao verificado] | L; essas recusas não demonstram falhas técnicas |
| REABRIR-NAO-DESTROI | `dominio:reabrir_disputa` — `app/chamados/views.py:1456`; `app/chamados/services/acoes_disputa.py:113` | **8 atos** | [nao verificado] | L; URL de origem |
| REABRIR-NAO-DESTROI | `dominio:corrigir_resposta_admin` — `app/chamados/services/acoes_disputa.py:375-383` | **42 atos** | [nao verificado] | L; URL de origem |
| REABRIR-NAO-DESTROI | `dominio:regularizar_disputa_granular` — `app/chamados/services/fio.py:713-714`; `app/chamados/services/acoes_disputa.py:144-145` | **7 atos** | **0 erros retornados de materialização**; classificação indevida [nao verificado] | L; URL de origem e erros fora dessa materialização |
| REABRIR-NAO-DESTROI | `dominio:recobrar_questionario` — `app/chamados/services/disputa_emissao.py:1755-1759` | **17 atos** | [nao verificado] | L; URL de origem |
| REABRIR-NAO-DESTROI | `dominio:cobranca` — `app/chamados/views_cobrar.py:54-56` | **13 notas** | [nao verificado] | L; distribuição entre endpoints |
| REABRIR-NAO-DESTROI | `dominio:solicitacao_correcao[HTML_API]` — `app/chamados/models.py:2325` | **38 notas** | [nao verificado] | L; separação HTML/API |
| REABRIR-NAO-DESTROI | `dominio:validar_pergunta` — `app/chamados/services/validacao.py:513-521` | [nao verificado] | **138 recusas em validações compartilhadas**; caráter indevido [nao verificado] | L; atribuição por endpoint |

**Smoke pelo ITEM 4: [nao verificado] nas três obras.** Há fatos persistidos positivos, mas “uso da porta > 0 e erros = 0”, incluindo 5xx/traceback e recusa indevida, não está demonstrado pelos dados apresentados. (`app/config/settings/saas.py:173-181`; referências e limites por porta na tabela)

## 16/11 14:31 · sessao 920e00533953 · PRIORIDADE-EV-ITEM6 · sonda · origem=Code

Referências `script.py:N` indicam linhas da **saída fornecida**, contando a primeira `sonda_leitura` como linha 1; no anexo, `aval Ronald` é a linha 1.

1. **O199:** 0 em 09/2026, 0 em 10/2026 e 0 em 11/2026. Nenhum caso individual aparece listado. (`script.py:4`, `script.py:5`, `script.py:6`)

2. **SC11 (col305):** o total informado para 11/2026 é **0**. Quantidades separadas de dias sem célula e com célula julgada errada: **[nao verificado]**, pois não há detalhamento desses grupos. A cobertura informa apenas `existentes_ate_hoje_sem_juizo=0`; cobertura integral de novembro e julgamento dos dias sem célula: **[nao verificado]**. (`script.py:7`, `script.py:8`)

3. **Erros:** não aparece linha `ERRO`; os totais zero não apresentam divergência visível com o detalhamento vazio; `rc_sombra=0`. Não há erro explícito na saída que invalide os números reportados. (`script.py:4`, `script.py:5`, `script.py:6`, `script.py:7`, `script.py:8`, `script.py:9`)

O destino por competência e a exigência de **! de escala** vêm do corte C. (`prioridade_espelho_verdade_tres_cortes_1611.txt:13`)

| obra | competencia | n | exportada | destino |
|---|---|---:|---|---|
| O199 | 09/2026 | 0 (`script.py:4`) | Sim, conforme enunciado | lista DP |
| O199 | 10/2026 | 0 (`script.py:5`) | Sim, conforme enunciado | lista DP |
| O199 | 11/2026 | 0 (`script.py:6`) | Não; aberta, conforme enunciado | apply na 11 com **! de escala** |
| SC11 (col305) | 11/2026 | 0 (`script.py:7`) | Não; aberta, conforme enunciado | apply na 11 com **! de escala** |

## 16/11 14:30 · sessao 114bf448442c · PRIORIDADE-EV-ITEM4 · leitura · origem=Code

**O dado fornecido não permite fechar nenhuma das três obras por “uso > 0 e erro 0”: as contagens posteriores aos pousos permanecem [nao verificado].** A cópia permite montar a sonda, mas a auditoria não guarda URL nem status HTTP, e vários endpoints compartilham o mesmo escritor. (`app/core/models.py:295-307`, `app/ponto/portas/he.py:263-268`, `app/chamados/services/acoes_disputa.py:113-114`)

**1. Portas e evidências**

Os prefixos são `/ponto/`, `/chamados/`, `/relatorios/` e `/api/`. (`app/config/urls.py:28-37`)

| Obra / porta | Rota, view | Uso persistido e escritor | Erro ou recusa persistidos |
|---|---|---|---|
| O-JANELA-HE — listagem | `ponto:gestao_he`; `/ponto/gestao-he/`; `gestao_he`. (`app/ponto/urls.py:17`, `app/ponto/views.py:887`) | A view lê e renderiza, sem gravar abertura. O retrato existente foi escrito pelo lavrador, não pelo acesso à tela. (`app/ponto/views.py:932-966`, `app/ponto/services/he_pendente_lavrado.py:177-189`) | `MetricaSnapshot.detalhe["erros"]`: falhas de apuração na **foto atual**, não erros HTTP nem histórico desde o pouso. Incremento em `he_pendente_lavrado.py:94-100`; gravação em `:177-189`; campos em `app/inteligencia/models.py:95-96`. |
| O-JANELA-HE — Autorizar/Não diário | `ponto:decidir_he`; `/ponto/gestao-he/decidir/`; `decidir_he`. (`app/ponto/urls.py:19`, `app/ponto/views.py:499`) | `DecisaoHE.estado`, `minutos_na_decisao`, `origem`, timestamps; criação/alteração em `_gravar`. Histórico: `LogAuditoria`, ação `decidir_he`, modelo `DecisaoHE`, `valor_antes/depois`, `criado_em`. (`app/ponto/models.py:1300-1317`, `app/ponto/portas/he.py:66-99`, `app/core/models.py:295-307`) | `estado="nao"` é ciência/decisão de negócio. A exceção `DecisaoHERecusada` apenas vira resposta da view; não há gravação desse retorno nesses caminhos. (`app/ponto/models.py:1283-1288`, `app/ponto/portas/he.py:175-193`, `app/ponto/views.py:535-537`) |
| O-JANELA-HE — Não em lote | `ponto:recusar_he_em_lote`; `/ponto/gestao-he/recusar-lote/`; `recusar_he_em_lote`. (`app/ponto/urls.py:21`, `app/ponto/views.py:549`) | Chama a mesma porta diária, produzindo a mesma trilha `decidir_he`, por dia alterado. **Não identifica o lote/URL.** (`app/ponto/portas/he.py:263-273`, `app/ponto/portas/he.py:94-99`) | Recusas ficam numa lista em memória e na resposta; itens malformados também só aparecem na resposta. (`app/ponto/portas/he.py:267-268`, `app/ponto/views.py:621-629`) |
| O-JANELA-HE — PDF | `ponto:gestao_he_pdf`; `/ponto/gestao-he/pdf/`; `gestao_he_pdf`. (`app/ponto/urls.py:27`, `app/ponto/views.py:772`) | `RegistroPDFAuditavel.tipo="outro"`, `filtros_json`, `gerado_em`; escritor em `pdf_base.py:152-166`. A chamada usa filtros `empresa/mes/ano/q/praca/posto/estado`, mas não persiste a rota. (`app/ponto/views.py:870-878`, `app/relatorios/models.py:36-55`) | O registro precede a segunda montagem do PDF; sua existência não prova conclusão nem entrega HTTP. (`app/relatorios/pdf_base.py:152-180`) |
| O-JANELA-HE — atalho na Central | `relatorios:index`; `/relatorios/`; `index`. (`app/relatorios/urls.py:9`, `app/relatorios/views.py:22`) | Lê contador e renderiza, sem trilha de abertura nessa view. (`app/relatorios/views.py:33-51`) | Falha do contador chama `logger.exception`; não grava erro de acesso no banco. (`app/relatorios/views.py:44-50`) |
| BO-HE-RECUSAR-LOTE-500 | As portas de decisão diária e recusa em lote acima; a listagem é a entrada da tela. (`app/docs/BACKLOG.md:1737`, `app/ponto/urls.py:17-21`) | As mesmas evidências anteriores, recortadas pelo pouso desta obra. A transação está na porta, antes do `select_for_update`. (`app/ponto/portas/he.py:151-159`, `app/ponto/portas/he.py:195`) | O caminho não registra 5xx no banco; o tratamento das recusas permanece em retorno/UI. (`app/ponto/views.py:535-537`, `app/ponto/views.py:621-629`, `app/config/settings/saas.py:173-181`) |

O pouso da O-JANELA nomeia as quatro rotas da Gestão de HE; os “13 passos” são referidos nesse registro. A inclusão exata de outras telas nos 13 passos é [nao verificado]. (`app/docs/RELATO-ARQUIVO.md:28278-28285`)

Na REABRIR, **o modelo atual é `DecisaoPergunta`**: a migration renomeou `RespostaAnterior`, preservou o instante como `ato_em` e atribuiu `ato="reabertura"` às linhas existentes. (`app/chamados/migrations/0051_decisao_pergunta.py:41-61`)

| Porta / fluxo REABRIR-NAO-DESTROI | Rota, view | Uso persistido e escritor | Erro ou recusa |
|---|---|---|---|
| Reabrir pela supervisão | `chamados:reabrir_disputa_supervisao`; `/chamados/local/<id>/reabrir-disputa/`; `reabrir_disputa_supervisao`. (`app/chamados/urls.py:20`, `app/chamados/views.py:1372`) | `DecisaoPergunta(ato="reabertura", ato_em)`; preparação em `disputa_emissao.py:1631-1646`, INSERT em `models.py:4120`. Auditoria `reabrir_disputa/DisputaSupervisao` em `views.py:1456`. | Sem permissão/sem disputa/sem pergunta corrigível viram respostas; não são gravados como recusas nesses ramos. (`app/chamados/views.py:1403-1434`) |
| Reabrir pelo fio/modal | `chamados:fio_colaborador`, `/chamados/colaborador/<id>/`, `fio_colaborador`; `chamados:modal_fio`, `/chamados/modal/<id>/`, `modal_fio_colaborador`. (`app/chamados/urls.py:21,39`, `app/chamados/views.py:659,869`) | POST delegado ao fio; mesma reabertura e mesma auditoria, sem distinguir a URL. (`app/chamados/views.py:667-674,951-954`, `app/chamados/services/fio.py:679-682`, `app/chamados/services/acoes_disputa.py:102-114`) | As recusas anteriores ao ato só retornam mensagem. (`app/chamados/services/acoes_disputa.py:95-101`) |
| Pedir correção — HTML/API | `chamados:solicitar_correcao_disputa`, `/chamados/disputa/<pk>/solicitar-correcao/`; `api:api_solicitar_correcao_disputa`, `/api/disputa/<pk>/solicitar-correcao/`. Views em `app/chamados/views.py:1317` e `app/api/views.py:2670`; rotas em `app/chamados/urls.py:19`, `app/api/urls.py:94`. | Ambas usam `reabrir_por_colab`; grava snapshot, contador e nota `[Solicitacao de correcao - `. HTML/API ficam indistinguíveis nessa evidência. (`app/chamados/views.py:1334`, `app/api/views.py:2686`, `app/chamados/models.py:2318-2325`) | Fechada/limite/sem resposta retornam estado, sem evento de recusa nesse método. (`app/chamados/models.py:2304-2312`) |
| Corrigir administrativamente | POST do modal com `acao=corrigir_resposta`; também fio e detalhe local. (`app/templates/chamados/partials/modal_fio.html:301-312`, `app/chamados/services/fio.py:663-677`, `app/chamados/views.py:525-536`) | Auditoria `corrigir_resposta_admin/PerguntaDisputa`, após gravar a resposta, **antes da validação/materialização**. (`app/chamados/services/acoes_disputa.py:370-383`) | `LogAuditoria(acao="val_perg_nao_gravou", modelo="PerguntaDisputa")` registra recusa de materialização, compartilhada com outras validações. (`app/chamados/services/validacao.py:513-521`) |
| Cobrar resposta | `chamados:cobrar_chamado`; `/chamados/cobrar/<cid>/`; `cobrar_chamado`. (`app/chamados/urls.py:25`, `app/chamados/views_cobrar.py:24`) | Nota `FollowupChamado.conteudo`, `criado_em`, prefixo `[COBRANCA]`; chamada em `views_cobrar.py:54-56`, INSERT em `services/nota.py:103,107`; campos em `models.py:1732-1738`. | Permissão, área, limite diário e falha de push retornam antes da nota. (`app/chamados/views_cobrar.py:25-53`) |
| Recobrar questionário pendente | Mesma rota `reabrir_disputa_supervisao`, ramo pendente. (`app/chamados/views.py:1421-1425`) | Auditoria `recobrar_questionario/DisputaSupervisao` e nota `[Re-cobranca do questionario - `. (`app/chamados/services/disputa_emissao.py:1741-1759`) | Falhas de nota/push usam `engolir`, que registra logging e contador de processo. (`app/chamados/services/disputa_emissao.py:1741-1754`, `app/core/observ.py:73-91`) |
| Encerrar/regularizar | POST do modal com `acao=regularizar_disputa`; também fio e `chamados:detalhe_local`, `/chamados/local/<id>/`, `detalhe_chamado_local`. (`app/templates/chamados/partials/modal_fio.html:353-359`, `app/chamados/urls.py:14`, `app/chamados/views.py:359,469-475`) | Auditoria `regularizar_disputa_granular/DisputaSupervisao`. (`app/chamados/services/fio.py:713-714`, `app/chamados/services/acoes_disputa.py:144-145`) | A descrição guarda `Mat=N Pend=N Fechada=B Erros=N`; guardas anteriores ao ato só retornam mensagem. (`app/chamados/services/fio.py:685-714`, `app/chamados/services/acoes_disputa.py:123-145`) |
| Abrir o histórico no modal | GET `chamados:modal_fio`, acima. (`app/chamados/urls.py:39`) | Carimba `FollowupChamado.lido_dp_em`, mas somente notas ainda não lidas; não é contador de aberturas nem evidência exclusiva desta tela. (`app/chamados/views.py:877-880`, `app/chamados/services/nota.py:126-133`, `app/chamados/models.py:1736`) | A presença do histórico não prova sua renderização; o template chama o leitor de respostas anteriores. (`app/templates/chamados/partials/modal_fio.html:204-205`, `app/chamados/models.py:3667-3683`) |

Uma reabertura produz linhas **por pergunta afetada**, não por clique; pendente limpa não produz versão. Decisão HE repetida também retorna sem trilha nova. (`app/chamados/services/disputa_emissao.py:1631-1648`, `app/ponto/portas/he.py:197-204`)

**2. Texto da sonda**

O script mantém `[nao verificado]` para contagens HTTP sem atribuição; imprime contagens separadas das portas de domínio compartilhadas. Para os horários incompletos, calcula limites: se houver registros dentro do minuto/faixa incerta, informa `n_min/n_max`, sem inventar o instante do pouso. Os horários publicados são os do backlog. (`app/docs/BACKLOG.md:1208`, `app/docs/BACKLOG.md:1737`, `app/docs/BACKLOG.md:1168`)

O envelope deve estar previamente inicializado; banco PROD `saas_hasner`, role `leitor` e tenant `juliani` correspondem ao envelope da cópia. **O wrapper não deve ser executado nesta tarefa:** ele próprio contém criação de arquivos e administração da role. (`bin/sonda_leitura.sh:28-29,42-46,216-218,239-240,248-250`)

```python
# OBRA: PRIORIDADE-EV-ITEM4
# Somente TEXTO para envelope PROD previamente inicializado; nao executado.
# Execucao, desempenho, Django/PostgreSQL e compatibilidade em PROD:
# [nao verificado].
# Nao chama views, servicos, juizes, lavradores nem django.setup().
# Biblioteca padrao e implementacoes externas: [nao verificado].
#
# Ambiente esperado: bin/sonda_leitura.sh:42-46,237-240.
# Datas publicadas: app/docs/BACKLOG.md:1208,1737,1168.
# "x" nao e convertido em minuto exato: contam-se limites inferior/superior.
# Offset -03:00 aplicado ao horario local informado: [nao verificado]. [nao verificado]
# TOTAL conta todas as linhas emitidas ANTES da propria linha TOTAL.
# Falha de sonda nunca e rotulada como erro ocorrido no produto.

import sys
sys.dont_write_bytecode = True

import datetime as dt
import logging
import re
from contextlib import redirect_stdout, redirect_stderr
from unittest.mock import patch

NV = "[nao verificado]"
SAIDA = sys.stdout
LINHAS = []

JANELA = "O-JANELA-HE"
BO = "BO-HE-RECUSAR-LOTE-500"
REABRIR = "REABRIR-NAO-DESTROI"

# Rotulo do pouso, menor instante possivel, primeiro instante certamente posterior. [nao verificado]
# Segundos exatos dos pousos: [nao verificado].
CORTES = {
    JANELA: (
        "2026-10-01T09:2x-03:00",
        "2026-10-01T09:20:00-03:00",
        "2026-10-01T09:30:00-03:00",
    ),
    BO: (
        "2026-10-22T10:26-03:00",
        "2026-10-22T10:26:00-03:00",
        "2026-10-22T10:27:00-03:00",
    ),
    REABRIR: (
        "2026-10-25T07:4x-03:00",
        "2026-10-25T07:40:00-03:00",
        "2026-10-25T07:50:00-03:00",
    ),
}

# Endpoints HTTP: nao converter trilha compartilhada em acesso atribuido.
# Rotas/views:
# app/ponto/urls.py:17-27; app/ponto/views.py:499,549,772,887.
# app/relatorios/urls.py:9; app/relatorios/views.py:22-51.
# app/chamados/urls.py:14,19-21,25,39.
# app/api/urls.py:94; app/api/views.py:2670.
PORTAS = {
    JANELA: (
        ("relatorios:index", "app/relatorios/views.py:33-51"),
        ("ponto:gestao_he", "app/ponto/views.py:932-966"),
        ("ponto:decidir_he", "app/ponto/portas/he.py:94-99"),
        ("ponto:recusar_he_em_lote", "app/ponto/portas/he.py:263-268"),
        ("ponto:gestao_he_pdf", "app/ponto/views.py:870-878"),
    ),
    BO: (
        ("ponto:gestao_he", "app/ponto/views.py:932-966"),
        ("ponto:decidir_he", "app/ponto/portas/he.py:94-99"),
        ("ponto:recusar_he_em_lote", "app/ponto/portas/he.py:263-268"),
    ),
    REABRIR: (
        ("chamados:reabrir_disputa_supervisao",
         "app/chamados/views.py:1435-1456"),
        ("chamados:fio_colaborador",
         "app/chamados/views.py:667-674"),
        ("chamados:modal_fio",
         "app/chamados/views.py:877-880"),
        ("chamados:modal_fio@corrigir_resposta",
         "app/chamados/services/acoes_disputa.py:375-383"),
        ("chamados:modal_fio@regularizar_disputa",
         "app/chamados/services/fio.py:713-714"),
        ("chamados:detalhe_local",
         "app/chamados/views.py:469-475"),
        ("chamados:solicitar_correcao_disputa",
         "app/chamados/models.py:2318-2325"),
        ("api:api_solicitar_correcao_disputa",
         "app/api/views.py:2686"),
        ("chamados:cobrar_chamado",
         "app/chamados/views_cobrar.py:54-56"),
    ),
}


class Descartar:
    def write(self, texto):
        return len(texto)

    def flush(self):
        return None


def classe_segura(exc):
    nome = type(exc).__name__
    return nome if re.fullmatch(r"[A-Za-z_][A-Za-z0-9_]{0,79}", nome) else "Excecao"


def adicionar(*campos):
    LINHAS.append("|".join(str(c) for c in campos))


def emitir_medida(tipo, obra, porta, valor, extras=(), subtipo=None):
    campos = [tipo, obra, porta]
    if subtipo is not None:
        campos.append(subtipo)
    if tipo == "USO":
        campos.extend(("desde=" + CORTES[obra][0], "n=" + str(valor)))
    elif tipo in ("ERRO", "RECUSA"):
        campos.append(str(valor))
    else:
        campos.append("n=" + str(valor))
    campos.extend(extras)
    adicionar(*campos)


def principais():
    # Ausencia de campo URL/status: app/core/models.py:295-307.
    for obra, portas in PORTAS.items():
        for porta, ref in portas:
            emitir_medida(
                "USO", obra, porta, NV,
                ("atribuicao_http=nao_disponivel", "ref=" + ref),
            )
            emitir_medida(
                "ERRO", obra, porta, NV,
                ("cobertura=banco_nao_conta_http",
                 "ref=app/config/settings/saas.py:173-181"),
                subtipo="5xx_traceback",
            )

    # Recusas do POST, distintas de decisao de negocio "Nao".
    # app/ponto/views.py:535-537,621-629.
    for obra in (JANELA, BO):
        for porta in ("ponto:decidir_he", "ponto:recusar_he_em_lote"):
            emitir_medida(
                "RECUSA", obra, porta, NV,
                ("criterio=nao_ha", "escopo=recusa_do_post_nao_persistida",
                 "ref=app/ponto/views.py:535-537,621-629"),
            )


def sonda():
    principais()
    try:
        with (
            patch.object(logging.Logger, "handle", lambda *a, **k: None),
            redirect_stdout(Descartar()),
            redirect_stderr(Descartar()),
        ):
            from django.apps import apps
            from django.conf import settings
            from django.db import connection, transaction

            # Configuracao tenant-aware: app/config/settings/saas.py:83-95.
            # Identidade real do envelope e GRANTs em PROD: [nao verificado].
            if (
                not apps.ready
                or settings.SETTINGS_MODULE != "config.settings.saas"
                or getattr(settings, "BANCO_DE_ENSAIO", False)
                or connection.settings_dict["NAME"] != "saas_hasner"
                or connection.schema_name != "juliani"
                or connection.in_atomic_block
                or not connection.get_autocommit()
            ):
                raise RuntimeError("AmbienteIncompativel")

            # Modelos/campos efetivamente consultados:
            # LogAuditoria: acao, modelo, valor_depois, descricao, criado_em
            # app/core/models.py:295-307.
            # Escritor efetivo: app/core/observ.py:133-138.
            Log = apps.get_model("core", "LogAuditoria")

            # DecisaoPergunta: ato, ato_em.
            # app/chamados/models.py:4039-4040.
            # Rename de RespostaAnterior:
            # app/chamados/migrations/0051_decisao_pergunta.py:41-61.
            # Escritor: app/chamados/models.py:4110-4122.
            Decisao = apps.get_model("chamados", "DecisaoPergunta")

            # FollowupChamado: conteudo, criado_em, lido_dp_em.
            # app/chamados/models.py:1735-1738.
            # Escritores: app/chamados/services/nota.py:103,107,126-133.
            Nota = apps.get_model("chamados", "FollowupChamado")

            # MetricaSnapshot: chave, escopo, detalhe, calculado_em.
            # app/inteligencia/models.py:87-96.
            # Escritor: app/ponto/services/he_pendente_lavrado.py:177-189.
            Snapshot = apps.get_model("inteligencia", "MetricaSnapshot")

            # RegistroPDFAuditavel: tipo, filtros_json, gerado_em.
            # app/relatorios/models.py:36-55.
            # Escritor: app/relatorios/pdf_base.py:152-166.
            PDF = apps.get_model("relatorios", "RegistroPDFAuditavel")

            with transaction.atomic():
                with connection.cursor() as cursor:
                    cursor.execute(
                        "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
                    )
                    cursor.execute("SET LOCAL statement_timeout = '30s'")
                    cursor.execute(
                        "SELECT current_database(), current_user, session_user, "
                        "current_schema(), "
                        "current_setting('transaction_read_only'), "
                        "CURRENT_TIMESTAMP"
                    )
                    banco, papel, sessao, schema, leitura, fim = cursor.fetchone()
                    if (banco, papel, sessao, schema, leitura) != (
                        "saas_hasner", "leitor", "leitor", "juliani", "on"
                    ):
                        raise RuntimeError("BancoPapelOuTenantIncompativel")

                def medir(tipo, obra, porta, qs, campo, ref, *,
                          subtipo=None, extras=(), reduzir=None):
                    # Conta somente fatos anteriores ao inicio desta transacao.
                    # Duas leituras na mesma fotografia transacional.
                    try:
                        with transaction.atomic():
                            _, texto_lo, texto_hi = CORTES[obra]
                            lo = dt.datetime.fromisoformat(texto_lo)
                            hi = dt.datetime.fromisoformat(texto_hi)
                            base = qs.filter(**{campo + "__lt": fim})
                            desde_lo = base.filter(**{campo + "__gte": lo})
                            desde_hi = base.filter(**{campo + "__gte": hi})
                            maior = (reduzir(desde_lo) if reduzir
                                     else desde_lo.count())
                            menor = (reduzir(desde_hi) if reduzir
                                     else desde_hi.count())
                            if menor < 0 or maior < menor:
                                raise RuntimeError("ContagemIncompativel")
                        valor = menor if menor == maior else NV
                        emitir_medida(
                            tipo, obra, porta, valor,
                            (
                                "desde=" + CORTES[obra][0]
                                if tipo != "USO" else "unidade=fatos_persistidos",
                                "n_min=" + str(menor),
                                "n_max=" + str(maior),
                                "n_janela_incerta=" + str(maior - menor),
                                "ref=" + ref,
                            ) + tuple(extras),
               

[truncado em 20000 de 31626 caracteres]

## 16/11 14:28 · sessao 8531c1e56279 · PRIORIDADE-EV-ITEM6 · leitura · origem=Code

1. **O199:** o juiz da pendência é `o_dia_ja_e_folga`, usado pelo próprio aceite; ele delega a `folga_lancada_do_dia`, registrado em JUIZES. Universo: respostas `FOLGA_CONTESTA` aceitas (`validada_em` preenchido), deduplicadas por colaborador e `data_evento`. (`app/ponto/services/declarar_dia.py:78`, `app/chamados/services/validacao.py:305`, `app/docs/JUIZES.md:127`, `app/chamados/services/validacao.py:259`, `app/chamados/services/validacao.py:159`, `app/chamados/services/validacao.py:293`.)

2. **SC11:** a obrigação de existir célula vem de `vinculo_cobre_a_data`; `vinculo_do_dia` identifica seu dono. Universo: dias de col305 na competência empresarial 11/2026 cobertos por vínculo, sem filtro por situação. (`app/docs/JUIZES.md:135`, `app/docs/JUIZES.md:137`, `app/escala/alimentacao.py:205`, `app/escala/alimentacao.py:228`, `app/ponto/portas/celula.py:870`.)

**Limite do pedido:** juiz geral de “célula errada” para toda a competência: `[nao verificado]`. O medidor existente julga futuras; comparar o template atual com células passadas não certifica erro. Por isso o script distingue casos confirmados de cobertura incompleta e não publica total integral falso. (`app/ponto/services/vigia_cadastro_futuro.py:24`, `app/ponto/services/vigia_cadastro_futuro.py:31`, `app/ponto/management/commands/tripwire_celulas.py:4`.)

O estado abaixo é o **gravado na célula**; status atual pelo juiz e possibilidade de declarar ficam `[nao verificado]`: seus caminhos alcançam carregadores/cache vedados pelo molde. Exportada significa `competencia_entregue`, sem confundir com fechamento por holerite. (`app/escala/models.py:1816`, `app/ponto/status_dia.py:270`, `app/ponto/services/declarar_dia.py:120`, `app/ponto/catalogo/ausencias.py:279`, `app/folha/export.py:556`, `app/folha/export.py:597`.)

```python
# OBRA: PRIORIDADE-EV-ITEM6
# Copia solicitada: a5765647153708b3fdd1c5dae4e6003bbc8fb4bf.
# Texto para envelope JA inicializado em SOMBRA / juliani / leitor.
# Nao executado; compatibilidade, desempenho e resultados: [nao verificado].
# Auxiliares deste texto, biblioteca padrao e internos do Django:
# [nao verificado].
#
# O199: inventario por competencia; 09/10 somente lista do DP.
# Elegibilidade para eventual apply restrita a 11, conforme anexo ITEM 6.
# Nenhuma porta de escrita e chamada.
#
# SC11:
# - ausentes: obrigacao de existencia pelo juiz de cobertura;
# - existentes futuras: medidor real, chamado numa janela de um dia;
# - existentes ate hoje: juizo geral de erro [nao verificado].
# TOTAL_PARCIAL conta linhas confirmadas.
# TOTAL recebe [nao verificado] quando a cobertura nao permite total integral.
#
# NAO chamar:
# checar_declaracao -> veto_de_lancamento -> ausencias_sobrepostas
# -> TIPOS_EM_ABERTO -> vigente, que escreve cache.
# app/ponto/services/declarar_dia.py:120,147
# app/ponto/precedencia.py:461
# app/ponto/turnos.py:2843
# app/ponto/catalogo/ausencias.py:267,279-280,406,430
#
# NAO chamar status_do_dia_lavrado com carga/insumos fabricados.
# app/ponto/status_dia.py:201,221,270,273,298
#
# NAO chamar lavrar(medida).
# app/ponto/services/vigia_cadastro_futuro.py:99-108

import sys
sys.dont_write_bytecode = True

import datetime as dt
import json
import logging
from collections import Counter
from contextlib import redirect_stderr, redirect_stdout
from unittest.mock import patch

NV = "[nao verificado]"
SAIDA = sys.stdout
ALVO = (2026, 11)


class Descartar:
    def write(self, texto):
        return len(texto)

    def flush(self):
        return None


def emitir(*campos):
    print("|".join(str(c) for c in campos), file=SAIDA, flush=True)


def compacto(valor):
    return json.dumps(
        valor, ensure_ascii=True, allow_nan=False, separators=(",", ":")
    )


def consumir(qs):
    it = qs.iterator(chunk_size=128)
    try:
        return list(it)
    finally:
        it.close()


def competencia(ano_mes):
    ano, mes = ano_mes
    return "%02d/%04d" % (mes, ano)


def sonda():
    linhas = []
    totais_o199 = Counter({(2026, 9): 0, (2026, 10): 0, ALVO: 0})
    total_sc11 = 0
    sem_data_o199 = 0
    existentes_sem_juizo = 0

    try:
        with (
            patch.object(logging.Logger, "handle", lambda *a, **k: None),
            redirect_stdout(Descartar()),
            redirect_stderr(Descartar()),
        ):
            from django.apps import apps
            from django.conf import settings
            from django.db import connection, transaction
            from django.utils import timezone

            if (
                not apps.ready
                or settings.SETTINGS_MODULE != "config.settings.sombra"
                or not settings.BANCO_DE_ENSAIO
                or connection.settings_dict["NAME"] != "sombra"
                or connection.settings_dict["HOST"] != "sombra_db"
                or connection.schema_name != "juliani"
            ):
                raise RuntimeError("AmbienteIncompativel")

            # Configuracao: app/config/settings/sombra.py:47-50,65.
            # Inicializacao efetiva do envelope: [nao verificado].

            # Predicado exato do aceite:
            # app/chamados/services/validacao.py:259-261.
            from chamados.services.validacao import _e_aceite_de_folga

            # Pergunta usada pela porta real de aceite:
            # app/ponto/services/declarar_dia.py:78-95.
            # Chamador real: app/chamados/services/validacao.py:305.
            from ponto.services.declarar_dia import o_dia_ja_e_folga

            # Juiz de competencia exportada:
            # app/folha/export.py:556-594.
            from folha.export import competencia_entregue

            # Competencia empresarial, sem corte 21 duplicado:
            # app/ponto/janelas.py:54-63.
            from ponto.janelas import janela_atual, janela_fechamento

            # Carregadores reais:
            # app/escala/alimentacao.py:54-78,163-186.
            # Juizes:
            # app/escala/alimentacao.py:205-216,228-286.
            from escala.alimentacao import (
                celulas_do_periodo,
                escalas_do_periodo,
                vinculo_cobre_a_data,
                vinculo_do_dia,
            )

            # Leitor real; nao importar/chamar lavrar:
            # app/ponto/services/vigia_cadastro_futuro.py:24-87.
            from ponto.services.vigia_cadastro_futuro import medir

            # Modelos:
            # app/chamados/models.py:2511,2528,2552-2554.
            # app/colaboradores/models.py:283.
            # app/escala/models.py:1635,1681-1695,1813-1816.
            Pergunta = apps.get_model("chamados", "PerguntaDisputa")
            Colaborador = apps.get_model("colaboradores", "Colaborador")
            CelulaDia = apps.get_model("escala", "CelulaDia")

            status_permitidos = {k for k, _ in CelulaDia.STATUS_DIA_CHOICES}

            def estado_gravado(cel):
                # Colunas reais; nenhuma chamada ao juiz de status.
                # app/escala/models.py:1686-1688,1816.
                return {
                    "folga_decidida_pela_porta": False,
                    "celula_existe": cel is not None,
                    "trabalha_gravado": None if cel is None else cel.trabalha,
                    "status_gravado": (
                        None if cel is None else
                        cel.status_dia if (
                            cel.status_dia is None
                            or cel.status_dia in status_permitidos
                        ) else NV
                    ),
                    "status_atual_pelo_juiz": NV,
                    "pode_declarar_agora": NV,
                }

            def retrato_vinculo(ec):
                # Apenas identificador e vigencia; nunca __str__.
                # Campos consumidos pelo juiz:
                # app/escala/alimentacao.py:214-216.
                return {
                    "id": ec.pk,
                    "ativa": ec.ativa,
                    "inicio": (
                        ec.data_inicio.isoformat()
                        if ec.data_inicio is not None else None
                    ),
                    "fim": (
                        ec.data_fim.isoformat()
                        if ec.data_fim is not None else None
                    ),
                }

            with transaction.atomic():
                with connection.cursor() as cursor:
                    cursor.execute(
                        "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, "
                        "READ ONLY"
                    )
                    cursor.execute(
                        "SELECT current_database(), current_user, "
                        "current_schema(), "
                        "current_setting('transaction_read_only')"
                    )
                    if cursor.fetchone() != (
                        "sombra", "leitor", "juliani", "on"
                    ):
                        raise RuntimeError("BancoPapelOuTenantIncompativel")

                # Relogio Django/envelope: [nao verificado].
                hoje = timezone.localdate()

                # O aceite real usa disputa.colaborador e data_evento.
                # app/chamados/services/validacao.py:293-294.
                # Carimbo da aceitacao:
                # app/chamados/services/validacao.py:152-166.
                perguntas = consumir(
                    Pergunta.objects.filter(validada_em__isnull=False)
                    .select_related("disputa__colaborador__empresa")
                    .order_by("pk")
                )
                feitos = set()

                for perg in perguntas:
                    # app/chamados/services/validacao.py:259-261.
                    if not _e_aceite_de_folga(perg):
                        continue

                    c = perg.disputa.colaborador
                    dia = perg.data_evento
                    if dia is None:
                        sem_data_o199 += 1
                        linhas.append((
                            "PENDENCIA", "O199", "col%d" % c.pk,
                            NV, "aceite_sem_data_evento",
                        ))
                        continue

                    chave = (c.pk, dia)
                    if chave in feitos:
                        continue
                    feitos.add(chave)

                    if c.empresa_id is None:
                        raise RuntimeError("EmpresaAusente")

                    # app/ponto/janelas.py:60-63.
                    ini, fim = janela_atual(dia, c.empresa)
                    par = (fim.year, fim.month)
                    if not ini <= dia <= fim:
                        raise RuntimeError("CompetenciaIncompativel")
                    totais_o199.setdefault(par, 0)

                    # Juiz real de pendencia do backfill:
                    # app/ponto/services/declarar_dia.py:78-95.
                    if o_dia_ja_e_folga(c, dia):
                        continue

                    # app/folha/export.py:556-594.
                    exportada = competencia_entregue(
                        c.empresa_id, fim.month, fim.year
                    )

                    # Leitura da coluna; nao julgamento do status.
                    # app/escala/models.py:1681-1688,1816.
                    cs = consumir(CelulaDia.objects.filter(
                        colaborador_id=c.pk, data=dia
                    ))
                    if len(cs) > 1:
                        raise RuntimeError("CelulaDuplicada")
                    cel = cs[0] if cs else None

                    linhas.append((
                        "O199", "col%d" % c.pk, dia.isoformat(),
                        competencia(par), "s" if exportada else "n",
                        compacto(estado_gravado(cel)),
                    ))
                    totais_o199[par] += 1

                # SC11: exclusivamente col305 na competencia empresarial 11.
                # Modelo: app/colaboradores/models.py:283.
                c305 = Colaborador.objects.select_related("empresa").get(pk=305)
                if c305.empresa_id is None:
                    raise RuntimeError("EmpresaAusente")

                # app/ponto/janelas.py:54-63.
                ini, fim = janela_fechamento(11, 2026, c305.empresa)
                if janela_atual(hoje, c305.empresa) != (ini, fim):
                    raise RuntimeError("OnzeNaoECompetenciaCorrente")

                # app/escala/alimentacao.py:54-78.
                celulas = celulas_do_periodo([305], ini, fim)

                # Incluir geradoras nomeadas fora da candidatura da janela.
                # app/escala/alimentacao.py:163-186.
                escalas = escalas_do_periodo(
                    305, ini, fim,
                    mais={
                        cel.escala_geradora_id
                        for cel in celulas.values()
                        if cel.escala_geradora_id is not None
                    },
                )

                # app/folha/export.py:556-594.
                exp305 = competencia_entregue(c305.empresa_id, 11, 2026)

                dia = ini
                while dia <= fim:
                    cel = celulas.get((305, dia))

                    # Cobertura nao se presume pelo flag ativa.
                    # app/escala/alimentacao.py:205-216.
                    cobrem = [
                        ec for ec in escalas
                        if vinculo_cobre_a_data(ec, dia)
                    ]

                    # Nao escolher outro vinculo por fora da autoridade.
                    # app/escala/alimentacao.py:228-286.
                    dono = vinculo_do_dia(
                        305, dia, escalas, celulas=celulas
                    )

                    motivos = []
                    if cel is None:
                        if cobrem:
                            motivos.append("sem_celula_com_vinculo")
                    elif dia > hoje:
                        # Janela de UM dia: obtem casos usando o medidor
                        # existente, sem copiar seu predicado.
                        # app/ponto/services/vigia_cadastro_futuro.py:24-87.
                        medida = medir(
                            dia, dia, hoje=hoje, colaborador_id=305
                        )
                        motivos = sorted(
                            k
                            for grupo in ("contadores", "selos")
                            for k, n in medida[grupo].items()
                            if n > 0
                        )
                    else:
                        # Medidor nao possui jurisdicao neste ramo.
                        # app/ponto/services/vigia_cadastro_futuro.py:31.
                        existentes_sem_juizo += 1

                    if motivos:
                        linhas.append((
                            "SC11", "col305", dia.isoformat(), "11/2026",
                            "s" if exp305 else "n",
                            compacto({
                                "existe": cel is not None,
                                "trabalha": (
                                    None if cel is None else cel.trabalha
                                ),
                            }),
                            compacto({
                                "do_dia": (
                                    None if dono is None else dono.pk
                                ),
                                "cobrem": [
                                    retrato_vinculo(ec)
                                    for ec in sorted(cobrem, key=lambda e: e.pk)
                                ],
                                "motivos": motivos,
                            }),
                        ))
                        total_sc11 += 1

                    dia += dt.timedelta(days=1)

        # Publicar somente depois de a leitura inteira terminar.
        for linha in linhas:
            emitir(*linha)

        for par, n in sorted(totais_o199.items()):
            emitir(
                "TOTAL", "O199", competencia(par),
                NV if sem_data_o199 else n,
            )
            if sem_data_o199:
                emitir("TOTAL_PARCIAL", "O199", competencia(par), n)

        emitir(
            "TOTAL", "SC11", "11/2026",
            NV if existentes_sem_juizo else total_sc11,
        )
        if existentes_sem_juizo:
            emitir("TOTAL_PARCIAL", "SC11", "11/2026", total_sc11)

        emitir(
            "COBERTURA", "SC11", "11/2026",
            "existentes_ate_hoje_sem_juizo=%d" % existentes_sem_juizo,
            NV if existentes_sem_juizo else "medidores_nomeados",
        )
        if sem_data_o199:
            emitir(
                "COBERTURA", "O199",
                "aceites_sem_data=%d" % sem_data_o199, NV,
            )

    except Exception:
        # Nao imprimir excecao, traceback, objeto ou texto livre:
        # podem conter nomes. Nenhum total parcial vira total integral.
        emitir("ERRO", "SONDA", NV)
        emitir("TOTAL", "O199", "11/2026", NV)
        emitir("TOTAL", "SC11", "11/2026", NV)


sonda()
```

3. **Portas do eventual apply:** O199 → `declarar_dia` (`app/ponto/services/declarar_dia.py:153`); SC11 por dia → `regenerar_celulas_dia`, que delega a `regenerar_celulas_vinculo` sem estender horizonte (`app/ponto/portas/celula.py:741`, `app/ponto/portas/celula.py:773`). Números atuais de produção: `[nao verificado]`.

## 16/11 14:26 · sessao 7ac5cdb58d24 · PRIORIDADE-EV-ITEM3 · leitura · origem=Code

| entrada | item O54 | cadeia (arquivo:linha) | confianca |
|---|---|---|---|
| BO-ADMIN-2710/3 | - | Justificativa limpa marcos faltantes (`app/escala/utils.py:1254-1259`) → resposta “dia já estava completo” (`app/ponto/services/veredito_celula.py:242-247`). E6 compara minutos realizados/pagos (`app/ponto/management/commands/e6_oraculo.py:263-268,362-381`); falta demonstrar que essa cura reduziria os divergentes de `E6-sistema-bate` (`app/core/espelho_verdade.py:180-181`). | [nao verificado] |
| BO2910/01-a | - | Troca de posto e histórico (`app/colaboradores/views.py:325-336`) → possível mudança de destino empresarial do TXT, cujo leitor filtra empresa (`app/folha/export.py:719-722`). Redução de número O54 por corrigir esse destino não demonstrada. | [nao verificado] |
| BO2910/01-b | - | O ramo relê o banco e devolve linha/toast de erro (`app/colaboradores/views.py:337-361`); falta ligação demonstrada desse tratamento de falha até uma medida O54. | [nao verificado] |
| BO2910/02 | - | Célula e marcos absolvem o dia (`app/chamados/reconciliador.py:1024-1032`) → retratação do chamado (`app/chamados/reconciliador.py:1001-1011`). Falta demonstrar que ampliar essa retratação reduziria algum número O54. | [nao verificado] |
| BO2910/05 | - | Validação condicionada ao envio de hora (`app/ponto/services/ausencia.py:1068-1070`) → atualização/invalidação da projeção (`app/ponto/services/ausencia.py:1101-1124`). `E5-le-da-celula` espera status `encerrado` (`app/core/espelho_verdade.py:150-151`), e E6 exclui dias com ausência (`app/ponto/management/commands/e6_oraculo.py:339-341`); redução não demonstrada. | [nao verificado] |
| BO2910/06 | - | Desligamento chama fechamento dos vínculos (`app/colaboradores/services/desligamento.py:147-149`); reativação exige revínculo administrativo (`app/colaboradores/services/desligamento.py:235-247`). A menção às portas em `R4` (`app/core/placar_estrutural.py:389-390`) não demonstra redução sem identificar vínculo/início e a divergência correspondente. | [nao verificado] |
| BO2910/08 | - | Regeneração futura chama desfazimento (`app/ponto/portas/celula.py:297-315`), que conserva decisão administrativa (`app/ponto/portas/celula.py:1155-1164`). Falta ligar essa conservação a um divergente efetivamente contado pelo E6 (`app/ponto/management/commands/e6_oraculo.py:362-381`). | [nao verificado] |
| BO-ADMIN-2710/5 | - | Leitura de `missing` identifica marco a cobrar (`app/chamados/services/declaracao_texto.py:130-139`). `R3` obtém a frase diretamente dos marcos da grade (`app/ponto/services/dia_decidido.py:74-87`); fazer nascer a pergunta não demonstra redução desse item (`app/core/placar_estrutural.py:146-153`). | [nao verificado] |
| HJ-017-U1 | - | Causa atual não comprovada no quadro fornecido; falta o ponto inicial da cadeia causa → medida. [nao verificado] | [nao verificado] |
| HJ-074 | - | Causa atual não comprovada no quadro fornecido; falta o ponto inicial da cadeia causa → medida. [nao verificado] | [nao verificado] |
| HJ-005-U1 | - | Causa atual não comprovada no quadro fornecido; falta o ponto inicial da cadeia causa → medida. [nao verificado] | [nao verificado] |
| HJ-012-U2 | - | Causa atual não comprovada no quadro fornecido; falta o ponto inicial da cadeia causa → medida. [nao verificado] | [nao verificado] |
| HJ-005-U2 | - | Causa atual não comprovada no quadro fornecido; falta o ponto inicial da cadeia causa → medida. [nao verificado] | [nao verificado] |
| HJ-005-U4 | - | Causa atual não comprovada no quadro fornecido; falta o ponto inicial da cadeia causa → medida. [nao verificado] | [nao verificado] |
| BO-ADMIN-2710/1 | - | Causa atual não comprovada no quadro fornecido; falta o ponto inicial da cadeia causa → medida. [nao verificado] | [nao verificado] |
| BO-ADMIN-2710/8.1 | - | Causa atual não comprovada no quadro fornecido; falta o ponto inicial da cadeia causa → medida. [nao verificado] | [nao verificado] |

## 16/11 14:26 · sessao 24490930eb08 · - · - · origem=Ronald (tmux)

- O pouso de O-JANELA-HE nomeia quatro rotas: `gestao_he`, `decidir_he`, `recusar_he_em_lote`, `gestao_he_pdf`; os “13 passos” estão referidos, sem roteiro enumerado encontrado. `app/docs/RELATO-ARQUIVO.md:28278-28285`, `app/docs/RELATO-ARQUIVO.md:19705`.

- Central original: `relatorios:index`, `/relatorios/`, `relatorios.views.index`; lê contador, renderiza sem trilha de uso; falha do contador chama `logger.exception`, não grava contador de erro no banco. `app/relatorios/urls.py:9`, `app/relatorios/views.py:22-51`. O painel de chamados também lê esse contador; seu `except` apenas esconde o atalho. `app/chamados/urls.py:9`, `app/chamados/views.py:759-767`.

- Listagem: `ponto:gestao_he`, `/ponto/gestao-he/`, view `ponto.views.gestao_he`; corpo lê e renderiza, sem escrita de uso. `app/ponto/urls.py:17`, `app/ponto/views.py:887-966`.

- PDF: `ponto:gestao_he_pdf`, `/ponto/gestao-he/pdf/`, view `ponto.views.gestao_he_pdf`; chama pipeline auditável com `tipo='outro'` e filtros `{empresa, mes, ano, q, praca, posto, estado}`. `app/ponto/urls.py:27`, `app/ponto/views.py:772`, `app/ponto/views.py:870-878`. Escritor `RegistroPDFAuditavel.objects.create`, campos `tipo`, `gerado_em`, `filtros_json`, `total_registros`; não há campo título/URL. `app/relatorios/pdf_base.py:152-166`, `app/relatorios/models.py:36-55`. Registro nasce antes da segunda passagem do PDF, portanto registro não prova resposta HTTP concluída. `app/relatorios/pdf_base.py:168-180`.

- Assinatura de filtros do PDF HE pode fornecer contagem candidata: demais produtores `tipo='outro'` encontrados usam filtros distintos — RetratoFolha `{empresa,mes,ano,recorte,n,total}` e férias `{empresa_id,situacao}`. Isso é inferência sobre a cópia atual, sem garantia de atribuição histórica da rota. `app/ponto/views.py:872-876`, `app/folha/views_retrato.py:443-449`, `app/relatorios/services.py:338-346`.

- Portas laterais, caso incluídas no escopo: `ponto:espelho` → `/ponto/espelho/` → `espelho_ponto`; `colaboradores:calendario_ponto` → `/colaboradores/<cid>/calendario/` → `calendario_ponto`; `folha:exportar` → `/folha/exportar/` → `tela_exportar_dominio`; `folha:gerar_txt` → `/folha/exportar/gerar/` → `gerar_dominio_txt`. `app/ponto/urls.py:10`, `app/ponto/views.py:97`, `app/colaboradores/urls.py:24`, `app/colaboradores/views.py:1965`, `app/folha/urls.py:13-14`, `app/folha/views.py:83`, `app/folha/views.py:102`. Prefixos em `app/config/urls.py:26-34`. Inclusão dessas portas nos 13 passos específicos: `[nao verificado]`.

- Export TXT: sucesso grava `LogAuditoria(acao='export_txt', modelo='ExportacaoDominio', valor_depois={empresa,mes,ano,colabs,linhas,retidos,arquivo})`; recusa do portão apenas retorna mensagem. `app/folha/views.py:128-133`, `app/folha/views.py:285-300`. `MetricaSnapshot` do portão guarda falhas e pendências, sem registrar chamada ao export ou recusa ocorrida. `app/folha/porta_export.py:772-796`, `app/folha/porta_export.py:800-814`, `app/folha/porta_export.py:851-867`.

- Data de REABRIR mais precisa na cópia: deploy com migrate em **25/10 07:49**, sem segundos; isso permanece relato histórico, não medição desta leitura. `app/docs/RELATO.md:4766`, `app/docs/RELATO.md:4775`.

## 16/11 14:26 · sessao 98d2e0d17bff · - · - · origem=Ronald (tmux)

REABRIR-NAO-DESTROI — achados para consolidação:

- `RespostaAnterior` foi renomeada para `DecisaoPergunta`; `versao_reaberta_em` virou `ato_em` e os registros antigos recebem `ato='reabertura'`. A sonda da cópia deve consultar o modelo atual. `app/chamados/migrations/0051_decisao_pergunta.py:41-61`
- Campos consultáveis: `pergunta`, `ato`, `ato_em`, `ato_por`, `ato_texto`, `ato_motivo`, fotos da resposta/validação. Escritor único: `registrar()`, cujo INSERT ocorre em `bulk_create`. `app/chamados/models.py:4038-4051`; `app/chamados/models.py:4110-4122`
- Reabrir gera uma linha por pergunta que perdeu texto, resposta, validação ou carimbo; não gera linha para pendente limpa. Logo, número de linhas não é número de cliques. `app/chamados/services/disputa_emissao.py:1631-1648`
- Modelo de auditoria não guarda URL, request ID nem status HTTP. Portanto não permite separar rotas que compartilham a mesma ação. `app/core/models.py:295-307`

| Porta/fluxo | Rota e view | Evidência de uso / escritor |
|---|---|---|
| Reabrir supervisão | `chamados:reabrir_disputa_supervisao`, `/chamados/local/<id>/reabrir-disputa/`; view `views.py:1372` — `urls.py:20`, prefixo `config/urls.py:28` | `DecisaoPergunta(ato='reabertura', ato_em)` pelo escritor acima; `LogAuditoria(acao='reabrir_disputa', modelo='DisputaSupervisao')` em `views.py:1456` |
| Reabrir pelo fio/modal | `chamados:fio_colaborador`, `/chamados/colaborador/<id>/`, view `views.py:659`; `chamados:modal_fio`, `/chamados/modal/<id>/`, view `views.py:869` — `urls.py:21,39` | POST delegado em `views.py:667-674,951-954`; ação `reabrir_disputa` em `services/fio.py:679-682`; mesmo LogAuditoria acima em `services/acoes_disputa.py:113-114`, portanto indistinguível da rota própria |
| Colaborador pede correção HTML | `chamados:solicitar_correcao_disputa`, `/chamados/disputa/<pk>/solicitar-correcao/`, view `views.py:1317` — `urls.py:19` | Chama `reabrir_por_colab` em `views.py:1334`; snapshot e contador em `models.py:2318-2321`; nota com prefixo `[Solicitacao de correcao - ` em `models.py:2325` |
| Colaborador pede correção API | `api_solicitar_correcao_disputa`, `/api/disputa/<pk>/solicitar-correcao/`, view `api/views.py:2670` — `api/urls.py:94`, prefixo `config/urls.py:37` | Mesmo método em `api/views.py:2686`; indistinguível do HTML no fato persistido |
| Corrigir resposta | POST `chamados:modal_fio`, `acao=corrigir_resposta`; botão `templates/chamados/partials/modal_fio.html:301-312` | `LogAuditoria(acao='corrigir_resposta_admin', modelo='PerguntaDisputa')` em `services/acoes_disputa.py:375-378`; é escrito antes da validação, que pode recusar em `:379-383` |
| Cobrar resposta | `chamados:cobrar_chamado`, `/chamados/cobrar/<cid>/`, view `views_cobrar.py:24` — `urls.py:25`; botão `modal_fio.html:349-351` | `FollowupChamado.conteudo` começa `[COBRANCA]`, `criado_em`; chamada escritora em `views_cobrar.py:54-56`; INSERT em `services/nota.py:103,107`; campos em `models.py:1732-1738` |
| Encerrar/regularizar | POST `chamados:modal_fio`, `acao=regularizar_disputa`; botão `modal_fio.html:353-359`; também fio e `chamados:detalhe_local` (`urls.py:14`) | `LogAuditoria(acao='regularizar_disputa_granular', modelo='DisputaSupervisao')`; descrição `Mat=N Pend=N Fechada=B Erros=N` em `services/fio.py:713-714` e `services/acoes_disputa.py:144-145` |

Filtros indicados, sempre com `criado_em__gte=corte` para auditoria/followup e `ato_em__gte=corte` para decisões:

- Reaberturas por pergunta: `DecisaoPergunta.objects.filter(ato="reabertura", ato_em__gte=corte).count()`. `app/chamados/models.py:4025,4039-4040`
- Reabertura supervisão/fio conjunta: auditoria `acao="reabrir_disputa", modelo="DisputaSupervisao"`. `app/chamados/views.py:1456`; `app/chamados/services/acoes_disputa.py:113`
- Correção administrativa: auditoria `acao="corrigir_resposta_admin", modelo="PerguntaDisputa"`. `app/chamados/services/acoes_disputa.py:375`
- Cobrança: followup `conteudo__startswith="[COBRANCA]"`; não usar `tipo`, que não é coluna de FollowupChamado. `app/chamados/views_cobrar.py:36-39,55`; `app/chamados/models.py:1732-1755`
- Regularização conjunta: auditoria `acao="regularizar_disputa_granular", modelo="DisputaSupervisao"`; contar tentativas com erro pelo `Erros=N` persistido, sem equiparar erro a indevido. `app/chamados/services/fio.py:713-714`
- Recusa humana de resposta: `DecisaoPergunta(ato="recusa")`; preferível à auditoria `recusar_pergunta`, gravada duas vezes pelo mesmo caminho. `app/chamados/services/validacao.py:628-629,637-645`
- Recusa de materialização: auditoria `acao="val_perg_nao_gravou", modelo="PerguntaDisputa"`; compartilhada entre validações, sem atribuição segura à correção/modal. `app/chamados/services/validacao.py:517-521`

As recusas por limite, disputa fechada ou ausência de resposta apenas retornam estado; não geram fato de recusa nesse método. `app/chamados/models.py:2304-2312` As recusas de cobrança por permissão, área, throttle ou push são respostas HTTP anteriores à nota. `app/chamados/views_cobrar.py:25-53` As guardas da regularização também retornam sem a auditoria posterior. `app/chamados/services/fio.py:685-699`

`recusa_motivo` representa estado atual e pode ser sobrescrito; notas de materialização são deduplicadas por pergunta/código, portanto não contam tentativas. `app/chamados/services/materializacao.py:1095-1113` Não há nesses registros um critério que classifique a recusa como indevida. [nao verificado]

A abertura do modal chama `marcar_lidas`, escrevendo no banco; a sonda deve ler modelos diretamente e não chamar a view. `app/chamados/views.py:877-880`; `app/chamados/services/nota.py:126-133` O histórico é renderizado pelo template, mas sua existência no banco não prova que foi mostrado corretamente. `app/templates/chamados/partials/modal_fio.html:204-205`; `app/chamados/models.py:3667-3683`

Falhas de auditoria/best-effort vão para logging e contador de memória, sem registro adicional no banco. `app/core/observ.py:73-82,105,134-139` A cobertura de logs da VM desde 04/11 13:22 veio apenas do dado fornecido. [nao verificado]

## 16/11 14:26 · sessao 6a1bdea30721 · - · - · origem=Ronald (tmux)

| entrada | item O54 | cadeia / motivo | confiança |
|---|---|---|---|
| BO-ADMIN-2710/3 | - | `app/escala/utils.py:1254-1259` apaga os marcos faltantes por justificativa → `app/ponto/services/veredito_celula.py:242-247` declara o dia completo. Não se demonstrou redução do número E6: ele lê realizado/pago (`app/ponto/management/commands/e6_oraculo.py:263-268,342-373`), enquanto o abono prometido é outra medida (`app/docs/BACKLOG.md:1976`; `app/ponto/motor_calculo_v2.py:2606-2622`). | [nao verificado] |
| BO2910/06 | - | O desligamento atual chama `fechar_vinculos_ativos` (`app/colaboradores/services/desligamento.py:147-149`), cuja porta já recusa fim anterior ao início (`app/colaboradores/services/vinculo.py:855-864`); a reativação exige revínculo pelo admin (`app/colaboradores/services/desligamento.py:235-247`). Sem definir vínculo/início da cura, não se demonstra redução de R4; sua referência às portas (`app/core/placar_estrutural.py:389-390`) não fecha essa cadeia. | [nao verificado] |
| HJ-017-U1, HJ-074, HJ-005-U1, HJ-012-U2, HJ-005-U2, HJ-005-U4, BO-ADMIN-2710/1, BO-ADMIN-2710/8.1 | - | O quadro fornecido não comprova produtor atual; substituir essa lacuna por causas históricas não demonstra a cadeia da entrada atual. [nao verificado] | [nao verificado] |

O placar registra expressamente que a causa dos 26+9 dias ESTRUTURA não foi nomeada (`app/core/espelho_verdade.py:190-191`; `app/core/placar_estrutural.py:130`). Portanto, compartilhar motor, célula ou cadastro com o medidor não comprova participação nesses números.

## 16/11 14:26 · sessao c5a20957395e · - · - · origem=Ronald (tmux)

| entrada | item O54 | cadeia (arquivo:linha) | confianca |
|---|---|---|---|
| BO2910/05 | - | Período opcional na decisão (`app/ponto/services/ausencia.py:1068-1070`) → invalidação da projeção (`app/ponto/services/ausencia.py:1123-1124`); não demonstra redução de item: `E5-le-da-celula` espera especificamente status encerrado (`app/core/espelho_verdade.py:150-151`). Além disso, o espelho publica a ausência (`app/ponto/services/espelho.py:582,845`) e o medidor E6 exclui dias com ausência (`app/ponto/management/commands/e6_oraculo.py:339-341`). | [nao verificado] |
| BO2910/08 | - | Regeneração de futura chama desfazimento (`app/ponto/portas/celula.py:312-315`), que conserva expressamente decisão administrativa (`app/ponto/portas/celula.py:1155-1164`); não há vínculo demonstrado entre essa conservação e um divergente contado no E6 (`app/ponto/management/commands/e6_oraculo.py:362-381`). | [nao verificado] |
| BO-ADMIN-2710/5 | - | `_marcos_faltantes` lê `missing` para identificar marco a cobrar (`app/chamados/services/declaracao_texto.py:130-139`); R3 compõe sua frase diretamente dos marcos faltantes (`app/ponto/services/dia_decidido.py:74-87,102-127`), e E6 compara minutos (`app/ponto/management/commands/e6_oraculo.py:350-381`). Fazer nascer a pergunta não demonstra alteração desses números. | [nao verificado] |

## 16/11 14:26 · sessao 67deb7deb439 · - · - · origem=Ronald (tmux)

SC11 tem duas autoridades registradas:

- Cobertura: `vinculo_cobre_a_data(ec,dia)` — `app/escala/alimentacao.py:205-216`; registro `app/core/juizes.py:1322-1323`, tabela `app/docs/JUIZES.md:137-138`.
- Vínculo do dia: `vinculo_do_dia(cid,dia,escalas,celulas=...)` — `app/escala/alimentacao.py:228-286`; tabela `app/docs/JUIZES.md:135-136`. Carregador completo disponível em `vinculo_do_dia_carregado`:289-317.

Universo de existência: dias cobertos por vínculo, sem filtro por situação pessoal; lei e delegação estão em `app/ponto/portas/celula.py:870-880,906`. Sem célula e sem cobertura não é pendência desse gerador; contador existente confirma em `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:51-57`.

Não localizei juiz geral puro registrado que diga “esta célula está errada” em toda a competência: `[nao verificado]`. Cuidado com substitutos:

- `EscalaColaborador.eh_dia_trabalho` lê a própria `cel.trabalha` quando existe; compará-los seria tautologia — `app/escala/models.py:1069-1096`.
- Divergência do cadastro tem medidor real `ponto.services.vigia_cadastro_futuro.medir(ini,fim,*,hoje,colaborador_id=None)` — `app/ponto/services/vigia_cadastro_futuro.py:24-87`. A jurisdição é **futuro**, e os baldes são **geradas**:31,49-50. Ele mede cobertura, cadastro/DNA e selos. Pode ser chamado por dia para obter casos sem copiar o predicado: `medir(d,d,hoje=hoje,colaborador_id=305)`. Não chamar `lavrar`:99-108.
- Comparar template vivo com todo passado contraria o próprio tripwire: passado divergente pode ser irretroatividade — `app/ponto/management/commands/tripwire_celulas.py:4-6`.
- `supra_juiz.classificar_dia` é puro, mas sua docstring diz que detecta contradições e **não decide quem está certo** — `app/ponto/supra_juiz.py:217-224`.
- `celula_veredito_velho` não serve: chama julgamento com escrita — `app/ponto/management/commands/celula_veredito_velho.py:19-21,144-145`.

Porta para eventual apply por dia: `ponto.portas.celula.regenerar_celulas_dia` — `app/ponto/portas/celula.py:741-776`; delega ao escritor `regenerar_celulas_vinculo`:112, com horizonte desligado:773-776. Cria ausente e corrige gerada em :289-310. Não chamá-la na leitura: até recusa de vínculo sem vigência grava evento — :147-162.

Sutileza: com célula existente, `vinculo_do_dia` pode devolver geradora inativa sem fim (:279-284), enquanto `vinculo_cobre_a_data` a recusa (:214-216). Exibir essas respostas sem presumir equivalência.

## 16/11 14:25 · sessao 3ac856a91cc7 · - · - · origem=Ronald (tmux)

- **Portas da HE:** prefixo `/ponto/` em `app/config/urls.py:29`. `ponto:gestao_he` = `/ponto/gestao-he/`, view `gestao_he` em `app/ponto/views.py:887`; `ponto:decidir_he` = `/ponto/gestao-he/decidir/`, view em `app/ponto/views.py:499`; `ponto:recusar_he_em_lote` = `/ponto/gestao-he/recusar-lote/`, view em `app/ponto/views.py:549`; `ponto:gestao_he_pdf` = `/ponto/gestao-he/pdf/`, view em `app/ponto/views.py:772`. Rotas em `app/ponto/urls.py:17-27`.

- **GET listagem:** lê snapshot em `app/ponto/views.py:932-946` e renderiza em `app/ponto/views.py:954-966`; não escreve trilha de abertura nessa view. Portanto número de aberturas não é obtido desse caminho no banco. O snapshot disponível não significa que a tela foi aberta: seu escritor é `lavrar`, `app/ponto/services/he_pendente_lavrado.py:177-189`.

- **Uso de decisão:** escritor único `_gravar` cria/atualiza `DecisaoHE` em `app/ponto/portas/he.py:66-86` e solicita `LogAuditoria(acao='decidir_he', modelo='DecisaoHE', objeto_id=pk, valor_antes, valor_depois)` em `app/ponto/portas/he.py:94-99`, chamado pela porta em `app/ponto/portas/he.py:206-208`. Modelo/campos `DecisaoHE`: `colaborador`, `data`, `estado`, `minutos_na_decisao`, `origem`, `decidida_em`, `atualizada_em` em `app/ponto/models.py:1300-1317`. Modelo/campos auditoria: `acao`, `modelo`, `objeto_id`, JSONs, `criado_em` em `app/core/models.py:295-307`; escritor efetivo em `app/core/observ.py:133-138`.

- **Não há atribuição inequívoca ao endpoint:** lote chama `decidir_he` por dia em `app/ponto/portas/he.py:263-266`, e autorização por período também chama a mesma porta em `app/ponto/portas/he.py:595-596`. A trilha não guarda URL/rota entre os campos escritos em `app/core/observ.py:134-137`. Assim, contar `acao='decidir_he'` mede alterações por dia da porta compartilhada; não mede POSTs individuais, lotes ou cliques separadamente. Repetição idempotente retorna sem trilha em `app/ponto/portas/he.py:197-204`.

- **“Não” gravado é decisão de negócio:** estado `'nao'` em `DecisaoHE` é ciência, não erro, segundo `app/ponto/models.py:1283-1288`. O estado atual não conserva cada troca; a porta atualiza a mesma linha em `app/ponto/portas/he.py:75-86`. Para histórico por instante usar `LogAuditoria.criado_em` e `valor_depois.estado`, cujos escritores estão em `app/ponto/portas/he.py:94-99`.

- **Recusa da porta:** validações levantam `DecisaoHERecusada` em `app/ponto/portas/he.py:175-193`; a view individual converte em resposta em `app/ponto/views.py:535-537`. Lote conserva recusas apenas em lista de retorno em `app/ponto/portas/he.py:267-268`, exibida em `app/ponto/views.py:621-629`. Esses caminhos não persistem a recusa nem um indicador “indevida”. Não contar tais recusas como zero a partir da ausência de trilha. Critério persistido que distingue recusa indevida de devida nesses caminhos: **[nao verificado]**.

- **Erro persistido do retrato:** `apurar` incrementa `erros` quando falha `espelho_do_colab` em `app/ponto/services/he_pendente_lavrado.py:94-100`, devolve `detalhe['erros']` em `app/ponto/services/he_pendente_lavrado.py:169-174`, e grava por `MetricaSnapshot.objects.update_or_create(... detalhe=retrato)` em `app/ponto/services/he_pendente_lavrado.py:177-189`. Campos `MetricaSnapshot.detalhe` e `calculado_em` em `app/inteligencia/models.py:95-96`. É foto atual por empresa/competência, sobrescrita; não soma histórica de erros HTTP desde deploy nem evidência de abertura da tela. Se reportar, usar tipo explícito como `apuracao_snapshot_atual`.

- **Falha de trilha:** `evento` é best effort, envolve criação em `engolir` em `app/core/observ.py:105-138`; erro vira contador de processo e logging em `app/core/observ.py:73-82`, não outra linha de banco. `registrar_log` também captura e chama `logger.exception` em `app/core/auditoria.py:26-30`. Portanto ausência de auditoria pode ser falha da própria auditoria, além de ausência de uso.

- **Relavratura:** depois da decisão, `recalcular_por_evento` é chamado em `app/ponto/portas/he.py:229-230`; agenda `on_commit` em `app/ponto/services/fechamento.py:1823-1827`; falha de execução apenas gera warning em `app/ponto/services/fechamento.py:1818-1821`. Decisão gravada não prova relavratura bem-sucedida.

- **PDF:** grava `RegistroPDFAuditavel` pelo pipeline em `app/relatorios/pdf_base.py:152-166`, chamado com `tipo='outro'`, `colaborador=None` e filtros empresa/mes/ano/q/praca/posto/estado em `app/ponto/views.py:870-878`. Campos de registro em `app/relatorios/models.py:30-57`; não há título/rota persistido nessa criação. `tipo='outro'` e formato dos filtros não bastam para identificação inequívoca da porta HE. Registro nasce antes da segunda montagem do PDF em `app/relatorios/pdf_base.py:168-178`, portanto também não prova resposta HTTP entregue.

- **500 histórico da atomicidade:** explicação e correção visíveis em `app/ponto/portas/he.py:151-159`; `@transaction.atomic` está na porta em `app/ponto/portas/he.py:158`, antes do `select_for_update` em `app/ponto/portas/he.py:195`. Dados de erros reais posteriores ao pouso não estão na cópia **[nao verificado]**. Contexto de access log vai em cabeçalhos para Gunicorn, não em modelo, conforme `app/core/middleware.py:96-105` e `app/core/middleware.py:111-128`.

## 16/11 14:25 · sessao 37406318bec5 · - · - · origem=Ronald (tmux)

O bloqueio do R2 foi baseado em escrita Python em memória, não em escrita no banco:

- `vigente()` consulta `TipoAusencia` e escreve somente o dicionário global `_CACHE`; retorna o catálogo real calculado. `app/ponto/catalogo/ausencias.py:240-254,257,267-281`.
- `_ult_cache.append(...)`, `_ub_cache.append(...)` e `_mpd_cache.append(...)` são listas locais. As fontes são valor calculado, `SELECT aggregate(Max(...))` e `marcos_dna_periodo(...)`. Impedir esses `append` quebra seus retornos indexados. `app/ponto/turnos.py:1154-1164,1901-1918`.
- `fechamento_lido()` chama o escritor real com `somente_leitura=True, retornar_objeto=True`; o ramo devolve o objeto sem salvá-lo e o ramo de lavratura fica excluído. `app/ponto/services/fechamento.py:1128-1144,1191-1194,1735-1761`.

Para cumprir o patch nominal exigido, a interceptação mínima pode substituir **somente o objeto `ponto.catalogo.ausencias._CACHE`** por um proxy que:

1. Delega `__getitem__` ao `_CACHE` original.
2. Intercepta `__setitem__` exclusivamente das chaves `"v"` e `"t"`, contabilizando e descartando as escritas.
3. Não inventa catálogo, não troca `vigente()` e não troca turnos.

Esse patch intercepta precisamente `app/ponto/catalogo/ausencias.py:279-280`. Em falta de cache, a função continua calculando `v` pelo leitor real e retornando-o; apenas deixa de memoizá-lo. `app/ponto/catalogo/ausencias.py:275-281`.

O tripwire precisa guardar uma **flag persistente por dia**: `_do_banco()` captura `Exception` e pode engolir sua exceção, retornando fallback. Após a chamada/dia, a flag deve obrigar `ERRO|escrita_nao_interceptada`, mesmo que a função tenha retornado. `app/ponto/catalogo/ausencias.py:246-254`.

Não encontrei chamadas a cache persistente nos arquivos investigados; cobertura completa de todos os caminhos transitivos: [nao verificado].

## 16/11 14:25 · sessao a0cb305f5b49 · - · - · origem=Ronald (tmux)

O199 tem duas perguntas distintas:

- **Já está decidida como folga?** A porta usa `o_dia_ja_e_folga(colaborador, data)`, em `app/ponto/services/declarar_dia.py:78-95`; o aceite real a consulta em `app/chamados/services/validacao.py:305`. Ela delega ao juiz registrado em `app/docs/JUIZES.md:127` e `app/core/juizes.py:213`: `folga_lancada_do_dia`, corpo puro em `app/ponto/precedencia.py:212-219`, predicado em `:222-239`, alimentado por todas as folgas da pessoa em `:194-209`. Folga à mão sobrevive à troca; calendário exige vínculo do dia, `app/ponto/precedencia.py:225-239`.
- **Pode declarar agora?** `checar_declaracao`, `app/ponto/services/declarar_dia.py:126-150`. Não equivale à primeira pergunta: `o_dia_ja_e_folga` distingue “não precisa” de “não posso”, conforme `:84-93`.

Universo correto: perguntas cuja resposta, após `strip`, seja `RESPOSTA_DE_FOLGA`, conforme `app/chamados/services/validacao.py:259-261` e constante `:79`; aceite gravado em `validada_em`, conforme escritor `:152-166`; colaborador e dia usados pelo ato são `perg.disputa.colaborador` e `perg.data_evento`, `:293-294`. Campos em `app/chamados/models.py:2521-2538,2552-2554`. Deduplicar por colaborador-dia, porque o próprio ato trata quatro perguntas do mesmo dia, `app/chamados/services/validacao.py:301-305`.

**Restrição para o script:** `checar_declaracao` chega a cache de catálogo: `declarar_dia.py:147,120` → `precedencia.py:461` → `turnos.py:2843-2847` (`Ausencia.TIPOS_EM_ABERTO`) → `catalogo/ausencias.py:406-407,430-435` → `vigente`, que escreve `_CACHE` em `:279-280`. `fatos_do_dia` também alcança o catálogo por `precedencia.py:256,273-278`. Se o molde proíbe inclusive essas escritas de cache, não chamar nem fabricar resultado equivalente.

O juiz de status é `status_do_dia_lavrado`, registrado em `app/docs/JUIZES.md:125`; avulso `app/ponto/status_dia.py:201-218` delega ao lote `:221`, que carrega turnos e pode consultar a natureza pela precedência (`:230-235`). Ler coluna atual de `CelulaDia` pode ser apresentado como snapshot gravado, mas não como resultado desse juiz.

Porta de apply O199: `ponto.services.declarar_dia.declarar_dia`, `app/ponto/services/declarar_dia.py:153`; ramo folga chama `_escrever_folga` em `:167-168`, que passa por `escala.folgas.lancar_folga` em `:179-193` e fecha o fio em `:205-209`.

## 16/11 14:25 · sessao a67016e6deab · - · - · origem=Ronald (tmux)

| entrada | item O54 | cadeia (arquivo:linha) | confiança |
|---|---|---|---|
| BO2910/01-a | - | A porta atribui `posto`, salva e registra histórico (`app/colaboradores/views.py:325-336`); a outra porta também atribui `empresa` (`app/colaboradores/views.py:1404-1407`). O TXT filtra pela empresa (`app/folha/export.py:719-722`), mas falta demonstrar que corrigir esse destino reduziria algum número O54; o termômetro compara minutos (`app/ponto/management/commands/e6_oraculo.py:342-379`). | [nao verificado] |
| BO2910/01-b | - | O ramo relê o colaborador e responde com linha/toast de erro (`app/colaboradores/views.py:337-361`); não há cadeia demonstrada desse tratamento de erro até medida de item O54. | [nao verificado] |
| BO2910/02 | - | A célula e seus marcos servem de predicado para retratar o chamado (`app/chamados/reconciliador.py:1024-1032` → `app/chamados/reconciliador.py:1001-1011`); falta cadeia demonstrada dessa retratação até redução de número O54. | [nao verificado] |

Complementos úteis: a troca de posto cadastral deixou de mudar o lugar do dia e regenerar células (`app/colaboradores/services/colaborador.py:118-123`). O item `R6` usa contratos declarados e reatividade (`app/core/placar_estrutural.py:433-449,623-625`); a matriz declara sete famílias sem família própria de colaborador/posto (`app/core/contratos_estruturais.py:45-53`), portanto contar portas de posto por semelhança com “um escritor” também não demonstra redução desse item.

_6073 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
